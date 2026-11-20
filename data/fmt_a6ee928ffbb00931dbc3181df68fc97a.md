
# TensorRT-LLM 模型部署与推理

```python
def predict(self, request: dict):
    prompt = request.pop("prompt")
    max_new_tokens = request.pop("max_new_tokens", 2048)
    temperature = request.pop("temperature", 0.9)
    top_k = request.pop("top_k", 1)
    top_p = request.pop("top_p", 0)
    streaming = request.pop("streaming", False)
    streaming_interval = request.pop("streaming_interval", 3)

    batch_input_ids = self.parse_input(tokenizer=self.tokenizer,
                                       input_text=[prompt],
                                       prompt_template=None,
                                       input_file=None,
                                       add_special_tokens=None,
                                       max_input_length=1028,
                                       pad_id=self.pad_id,
                                       )
    input_lengths = [x.size(0) for x in batch_input_ids]

    outputs = self.model.generate(
        batch_input_ids,
        max_new_tokens=max_new_tokens,
        max_attention_window_size=None,
        sink_token_length=None,
        end_id=self.end_id,
        pad_id=self.pad_id,
        temperature=temperature,
        top_k=top_k,
        top_p=top_p,
        num_beams=1,
        length_penalty=1,
        repetition_penalty=1,
        presence_penalty=0,
        frequency_penalty=0,
        stop_words_list=STOP_WORDS_LIST,
        bad_words_list=BAD_WORDS_LIST,
        lora_uids=None,
        streaming=streaming,
        output_sequence_lengths=True,
        return_dict=True)

    if streaming:
        streamer = throttle_generator(outputs, streaming_interval)

        def generator():
            total_output = ""
            for curr_outputs in streamer:
                if self.runtime_rank == 0:
                    output_ids = curr_outputs['output_ids']
                    sequence_lengths = curr_outputs['sequence_lengths']
                    batch_size, num_beams, _ = output_ids.size()
                    for batch_idx in range(batch_size):
                        for beam in range(num_beams):
                            output_begin = input_lengths[batch_idx]
                            output_end = sequence_lengths[batch_idx][beam]
                            outputs = output_ids[batch_idx][beam][
                                      output_begin:output_end].tolist()
                            output_text = self.tokenizer.decode(outputs)

                            current_length = len(total_output)
                            total_output = output_text
                            yield total_output[current_length:]
        return generator()
    else:
        if self.runtime_rank == 0:
            output_ids = outputs['output_ids']
            sequence_lengths = outputs['sequence_lengths']
            batch_size, num_beams, _ = output_ids.size()
            for batch_idx in range(batch_size):
                for beam in range(num_beams):
                    output_begin = input_lengths[batch_idx]
                    output_end = sequence_lengths[batch_idx][beam]
                    outputs = output_ids[batch_idx][beam][
                              output_begin:output_end].tolist()
                    output_text = self.tokenizer.decode(outputs)
                    return {"output": output_text}
```

`predict`函数接受一些模型输入，如提示、`max_new_tokens`、温度等。我们使用请求在函数的顶部提取所有这些值。调用LLM模型来使用`self.model.generate`函数生成输出。`generate`函数接受各种参数，帮助控制LLM的输出。

为了在云中运行我们的模型，还需要将其容器化。`Truss`会负责为我们创建`Dockerfile`并打包所有内容，所以我们不需要做太多事情。

在`mistral-7b-tensorrt-llm-truss`目录之外创建一个名为`main.py`的文件。将以下代码粘贴到其中:

```python
import truss
from pathlib import Path

tr = truss.load("./mistral-7b-tensorrt-llm-truss")
command = tr.docker_build_setup(build_dir=Path("./mistral-7b-tensorrt-llm-truss"))
print(command)
```

运行`main.py`文件并查看`mistral-7b-tensorrt-llm-truss`目录。应该会看到自动生成的一堆文件。下面就可以使用`docker`构建容器。依次运行以下命令:

```bash
docker build mistral-7b-tensorrt-llm-truss -t mistral-7b-tensorrt-llm-truss:latest
docker tag mistral-7b-tensorrt-llm-truss <docker_user_id>/mistral-7b-tensorrt-llm-truss
docker push <docker_user_id>/mistral-7b-tensorrt-llm-truss
```

这些`docker`的配置文件就是`truss`为我们自动生成好的，我们下面简单的介绍一下看`k8s`的部署，我不会深入讨论如何设置`GKE`集群，因为这不在本文的讨论范围之内。

创建以下`kubernetes`部署:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mistral-7b-v2-trt
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      component: mistral-7b-v2-trt-layer
  template:
    metadata:
      labels:
        component: mistral-7b-v2-trt-layer
    spec:
      containers:
      - name: mistral-container
        image: htrivedi05/mistral-7b-v0.2-trt:latest
        ports:
          - containerPort: 8080
        resources:
          limits:
            nvidia.com/gpu: 1
      nodeSelector:
        cloud.google.com/gke-accelerator: nvidia-tesla-a100
---
apiVersion: v1
kind: Service
metadata:
  name: mistral-7b-v2-trt-service
  namespace: default
spec:
  type: ClusterIP
  selector:
    component: mistral-7b-v2-trt-layer
  ports:
  - port: 8080
    protocol: TCP
    targetPort: 8080
```

这是一个标准的`kubernetes`部署，它运行一个映像为`htrivedi05/mistral-7b-v0.2-trt:latest`的容器。

可以通过运行命令创建部署:

`kubectl create -f mistral-deployment.yaml`

分配`kubernetes` `pod`需要几分钟的时间。一旦`pod`开始运行，我们之前编写的`load()`函数就会被执行。

一旦加载了模型后就可以在`pod`日志中看到类似Completed `model.load()`的执行时间为449234毫秒。要通过`HTTP`向模型发送请求，我们需要对服务进行端口转发。你可以使用下面的命令:

```bash
kubectl port-forward svc/mistral-7b-v2-trt-service 8080
```

打开任意Python脚本并运行以下代码:

```python
import requests

data = {"prompt": "What is a mistral?"}
res = requests.post("http://127.0.0.1:8080/v1/models/model:predict", json=data)
res = res.json()
print(res)
```

将看到如下输出:

```json
{"output": "A Mistral is a strong, cold wind that originates in the Rhone Valley in France. It is named after the Mistral wind system, which is associated with the northern Mediterranean region. The Mistral is known for its consistency and strength, often blowing steadily for days at a time. It can reach speeds of up to 130 kilometers per hour (80 miles per hour), making it one of the strongest winds in Europe. The Mistral is also known for its clear, dry air and its role in shaping the landscape and climate of the Rhone Valley."}
```

这样我们的推理服务就部署成功了

## 性能基准测试

我运行了一些自定义基准测试，得到了以下结果:

![](img/f08fc506cded8566fd6351893d8f242a.png)

![](img/2e48310e9c815eb1e43417e3c3738ff0.png)

![](img/fbfe014ca365738080052972a19590d1.png)

可以看到`TensorRT-LLM`的加速推理还是很明显的

## 总结

在这篇文章中，我们演示了如何使用`TensorRT-LLM`实现模型加速推理，文章内容涵盖了从编译`LLM`到在生产中部署模型的所有内容。

虽然`TensorRT-LLM`比其他推理优化器更复杂，但性能提高也是非常明显。虽然该框架仍处于早期阶段，但是可以提供目前最先进的`LLM`优化。并且它是完全开源的可以商业化，我相信`TensorRT-LLM`以后还会有更大的发展，因为毕竟是`NVIDIA`自己的产品。

`TensorRT-LLM`代码：https://github.com/NVIDIA/TensorRT-LLM

本文Colab https://colab.research.google.com/drive/1tJSMGbqYDstnChytaFb9F37oBspDcsRL

作者：Het Trivedi
