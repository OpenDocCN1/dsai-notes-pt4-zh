
# DeepSeek-V3 部署与调用

## lmdeploy 本地调用

```python
from lmdeploy import pipeline, PytorchEngineConfig

if __name__ == "__main__":
    pipe = pipeline("deepseek-ai/DeepSeek-V3-FP8", backend_config=PytorchEngineConfig(tp=8))
    messages_list = [
        [{"role": "user", "content": "Who are you?"}],
        [{"role": "user", "content": "Translate the following content into Chinese directly: DeepSeek-V3 adopts innovative architectures to guarantee economical training and efficient inference."}],
        [{"role": "user", "content": "Write a piece of quicksort code in C++."}],
    ]
    output = pipe(messages_list)
    print(output)
```

## 在线服务调用

先运行如下命令：

```bash
# run
lmdeploy serve api_server deepseek-ai/DeepSeek-V3-FP8 --tp 8 --backend pytorch
```

接下来即可在 `23333` 端口调用 `DeepSeek v3` 模型：

```python
from openai import OpenAI
client = OpenAI(
    api_key='YOUR_API_KEY',
    base_url="http://0.0.0.0:23333/v1"
)
model_name = client.models.list().data[0].id
response = client.chat.completions.create(
  model=model_name,
  messages=[
    {"role": "user", "content": "Write a piece of quicksort code in C++."}
  ],
    temperature=0.8,
    top_p=0.8
)
print(response)
```

## DeepSeek v3+vLLM部署方案

![](img/4f642a73a43e2b4c5b4173505ff28558.png)

vLLM项目主页：https://github.com/vllm-project/vllm

`vLLM` `v0.6.6` 支持在 `NVIDIA` 和 `AMD` GPU 上以 `FP8` 和 `BF16` 模式进行 `DeepSeek-V3` 推理。除了标准技术外，`vLLM` 还提供了管道并行性，允许你在多个网络连接的机器上运行该模型。

### vLLM安装

```bash
pip install vllm
```

### DeepSeek v3调用

目前 `vLLM` 已支持 `DeepSeek v3` 模型调用，可以在模型支持列表中查看模型关键字：https://docs.vllm.ai/en/latest/models/supported_models.html

![](img/d98a97a8e32b06c9410acd186902da31.png)

接下来即可使用如下代码进行调用：

```python
from vllm import LLM

# For generative models (task=generate) only
llm = LLM(model="deepseek-ai/DeepSeek-V3", task="generate")  # Name or path of your model
output = llm.generate("Hello, my name is")
print(output)
```
