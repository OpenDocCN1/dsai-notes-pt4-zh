# 推理和部署

> 来源：[https://v11enp9ok1h.feishu.cn/docx/ZtqBdMmfSoPwVrxCmEacVuy5n1b](https://v11enp9ok1h.feishu.cn/docx/ZtqBdMmfSoPwVrxCmEacVuy5n1b)

# 推理和部署

以下为swift支持的推理引擎以及接入部分的相应能力，三种推理加速引擎为SWIFT的推理、部署、评测模块提供推理加速：

## 推理

ms-swift使用了分层式的设计思想，用户可以使用命令行界面、Web-UI界面和直接使用Python的方式进行推理。

如果要查看LoRA微调后模型的推理，可以参考。

### 使用CLI

**全参数模型：**

```
CUDA_VISIBLE_DEVICES=0 swift infer \
    --model Qwen/Qwen2.5-7B-Instruct \
    --stream true \
    --infer_backend pt \
    --max_new_tokens 2048
```

**LoRA模型：**

```
CUDA_VISIBLE_DEVICES=0 swift infer \
    --model Qwen/Qwen2.5-7B-Instruct \
    --adapters swift/test_lora \
    --stream true \
    --infer_backend pt \
    --temperature 0 \
    --max_new_tokens 2048
```

**命令行推理指令**

以上为交互式命令行界面推理，脚本运行后仅需在terminal中输入query即可。你也可以输入以下特殊指令：

+   multi-line: 切换到多行模式，在输入中支持换行输入，以#代表输入结束。

+   single-line: 切换到单行模式，以换行代表输入结束。

+   reset-system: 重置system并清空历史记录。

+   clear: 清除历史记录。

+   quit or exit: 退出对话。

多模态模型

```
CUDA_VISIBLE_DEVICES=0 \
MAX_PIXELS=1003520 \
VIDEO_MAX_PIXELS=50176 \
FPS_MAX_FRAMES=12 \
swift infer \
    --model Qwen/Qwen2.5-VL-3B-Instruct \
    --stream true \
    --infer_backend pt \
    --max_new_tokens 2048
```

如果要进行多模态模型的推理，可以在query中添加<image>/<video>/<audio>等标签（代表图像表征在inputs_embeds中的位置），例如输入<image><image>这两张图有啥区别，<video>描述这段视频。然后根据提示输入相应的图像/视频/音频即可。

以下为一个推理的示例：

```
&lt;&lt;&lt; &lt;image&gt;&lt;image&gt;这两张图有什么区别
Input an image path or URL &lt;&lt;&lt; http://modelscope-open.oss-cn-hangzhou.aliyuncs.com/images/cat.png
Input an image path or URL &lt;&lt;&lt; http://modelscope-open.oss-cn-hangzhou.aliyuncs.com/images/animal.png
这两张图片的区别在于它们所展示的动物和场景。

第一张图片
：
展示了一只小猫。
小猫有大大的眼睛，表情显得有些困惑或好奇。
背景是模糊的，可能是室内环境。

第二张图片
：
展示了一群羊。
羊们站在草地上，背景是绿色的山丘和蓝天白云。
羊的表情看起来很平静，似乎在享受大自然的环境。
总结来说，第一张图片是一只小猫，而第二张图片是一群羊。
--------------------------------------------------
&lt;&lt;&lt; clear
&lt;&lt;&lt; &lt;video&gt;描述这段视频
Input a video path or URL &lt;&lt;&lt; https://modelscope-open.oss-cn-hangzhou.aliyuncs.com/images/baby.mp4
The video shows a baby wearing sunglasses sitting on a bed and reading a book. The baby is holding the book with both hands and appears to be focused on the pages. The baby&#x27;s feet are visible in the frame, and they are moving slightly as they read. The background of the video shows a room with a bed and some furniture.
```

数据集推理：

```
CUDA_VISIBLE_DEVICES=0 swift infer \
    --model Qwen/Qwen2.5-7B-Instruct \
    --stream true \
    --infer_backend pt \
    --val_dataset AI-ModelScope/alpaca-gpt4-data-zh \
    --max_new_tokens 2048
```

以上提供了全参数和LoRA流式推理的例子，以下介绍更多SWIFT中的推理技术：

+   界面推理：你可以将swift infer改成swift app。

+   batch推理：infe[r_](https://github.com/modelscope/ms-swift/blob/main/examples/infer/pt/batch_ddp.sh)backend=pt可以指定--max_batch_size对大模型和多模态大模型进行batch推理，具体参考这里。在进行batch推理时，你不能设置--stream true。

+   [DD](https://github.com/modelscope/ms-swift/blob/main/examples/infer/pt/mllm_device_map.sh)P/device_map推理：infer_backend=pt支持使用DDP/device_map技术进行并行推理，具体参考这里。

+   推理加速：swi[ft](https://github.com/modelscope/ms-swift/blob/main/examples/infer/vllm/ddp.sh)支持使用vllm/lmdeploy对推理、部署和评测模块进行推理加速，只需要额外指定--infer_backend vllm/lmdeploy即可。可以参考这里。

+   多模态模型：我们提供了[pt](https://github.com/modelscope/ms-swift/blob/main/examples/infer/pt/mllm_device_map.sh)/[vllm](https://github.com/modelscope/ms-swift/blob/main/examples/infer/vllm/mllm_tp.sh)/[lmdeploy](https://github.com/modelscope/ms-swift/blob/main/examples/infer/lmdeploy/mllm_tp.sh)对多模态模型进行多GPU推理的shell脚本。

+   量化模型：直接选择GPTQ、AWQ、BNB量化的模型，例如：--model Qwen/Qwen2.5-7B-Instruct-GPTQ-Int4即可。

+   更多模型类型：我们提供了[bert](https://github.com/modelscope/ms-swift/blob/main/examples/infer/pt/bert.sh)、[reward_model](https://github.com/modelscope/ms-swift/blob/main/examples/infer/pt/reward_model.sh)、[prm](https://github.com/modelscope/ms-swift/blob/main/examples/infer/pt/prm.sh)的推理脚本。

小帖士：

+   SWIFT会将推理结果保存起来，你可以通过--result_path指定保存路径。

+   如果要输出logprobs，只需要在推理时，指定--logprobs true即可。SWIFT会保存。注意，设置--stream true将不会存储。

+   infer_backend为pt支持所有swift已支持模型的推理，而[infe](https://docs.vllm.ai/en/latest/models/supported_models.html)r[_backend](https://lmdeploy.readthedocs.io/en/latest/supported_models/supported_models.html)为vllm/lmdeploy只支持部分模型，具体请参考vllm、lmdeploy文档。

+   使用--infer_backend vllm出现OOM，可以通过降低--max_model_len，--max_num_seqs，选择合适的--gpu_memory_utilization，设置--enforce_eager true。或者使用tensor并行--tensor_parallel_size来解决。

+   使用--infer_backend vllm推理多模态模型，需要传入多张图片。可以设置--limit_mm_per_prompt解决，例如：--limit_mm_per_prompt '{"image": 10, "video": 5}'。

+   推理qwen2-vl/qwen2.5-vl出现OOM，可以通过设置[MA](https://github.com/modelscope/ms-swift/blob/main/examples/app/mllm.sh)X_PIXELS、VIDEO_MAX_PIXELS、FPS_MAX_FRAMES解决，可以参考这里。

+   swift内置对话模板与使用transf[or](https://github.com/modelscope/ms-swift/blob/main/tests/test_align/test_template/test_vision.py)mers运行的对话模板对齐，测试参考这里。如果出现未对齐情况，欢迎提issue/PR修正。

### 使用Web-UI#

[如果你要使用界面](https://modelscope.cn/docs/llm-training-and-inference/intro/web-ui)的方式进行推理，可以查看Web-UI文档。

### 使用Python#

文本模型：

```
import os
os.environ[&#x27;CUDA_VISIBLE_DEVICES&#x27;] = &#x27;0&#x27;

from swift.llm import PtEngine, RequestConfig, InferRequest
model = &#x27;Qwen/Qwen2.5-0.5B-Instruct&#x27;

# 加载推理引擎
engine = PtEngine(model, max_batch_size=2)
request_config = RequestConfig(max_tokens=512, temperature=0)

# 这里使用了2个infer_request来展示batch推理
infer_requests = [
    InferRequest(messages=[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;who are you?&#x27;}]),
    InferRequest(messages=[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;浙江的省会在哪？&#x27;},
                           {&#x27;role&#x27;: &#x27;assistant&#x27;, &#x27;content&#x27;: &#x27;浙江省的省会是杭州。&#x27;},
                           {&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;这里有什么好玩的地方&#x27;},]),
]
resp_list = engine.infer(infer_requests, request_config)
query0 = infer_requests[0].messages[0][&#x27;content&#x27;]
print(f&#x27;response0: {resp_list[0].choices[0].message.content}&#x27;)
print(f&#x27;response1: {resp_list[1].choices[0].message.content}&#x27;)
```

多模态模型：

```
import os
os.environ[&#x27;CUDA_VISIBLE_DEVICES&#x27;] = &#x27;0&#x27;
os.environ[&#x27;MAX_PIXELS&#x27;] = &#x27;1003520&#x27;
os.environ[&#x27;VIDEO_MAX_PIXELS&#x27;] = &#x27;50176&#x27;
os.environ[&#x27;FPS_MAX_FRAMES&#x27;] = &#x27;12&#x27;

from swift.llm import PtEngine, RequestConfig, InferRequest
model = &#x27;Qwen/Qwen2.5-VL-3B-Instruct&#x27;

# 加载推理引擎
engine = PtEngine(model, max_batch_size=2)
request_config = RequestConfig(max_tokens=512, temperature=0)

# 这里使用了3个infer_request来展示batch推理
infer_requests = [
    InferRequest(messages=[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;who are you?&#x27;}]),
    InferRequest(messages=[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;&lt;image&gt;&lt;image&gt;两张图的区别是什么？&#x27;}],
                 images=[&#x27;http://modelscope-open.oss-cn-hangzhou.aliyuncs.com/images/cat.png&#x27;,
                         &#x27;http://modelscope-open.oss-cn-hangzhou.aliyuncs.com/images/animal.png&#x27;]),
    InferRequest(messages=[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;&lt;video&gt;describe the video&#x27;}],
                 videos=[&#x27;https://modelscope-open.oss-cn-hangzhou.aliyuncs.com/images/baby.mp4&#x27;]),
]
resp_list = engine.infer(infer_requests, request_config)
query0 = infer_requests[0].messages[0][&#x27;content&#x27;]
print(f&#x27;response0: {resp_list[0].choices[0].message.content}&#x27;)
print(f&#x27;response1: {resp_list[1].choices[0].message.content}&#x27;)
print(f&#x27;response2: {resp_list[2].choices[0].message.content}&#x27;)
```

我们也提供了更多使用python推理的demo：

+   使用流式推理以及VllmEngine、LmdeployEngine进行推理加速，可以参考[这里](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo.py)。

+   多模态推理：除了上述多模态输入格式外，swif[t兼](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo_mllm.py)容OpenAI的多模态输入格式，参考这里。

+   grounding任务：对多模态模型[进行](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo_grounding.py)Grounding任务画框，可以参考这里。

+   多LoRA推理：参考[这里](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo_lora.py)。

+   agent推理：参考[这里](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo_agent.py)。

+   异步接口：使用engine.infer_as[yn](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo.py)c进行python方式推理，参考这里。

## 部署#

如果要查看LoRA微调后模型的部署，可以参考。

这里主要介绍对多模态模型的部署和调用，文本大模型我们给出简单的部署和调用的简单案例：

服务端部署：

```
CUDA_VISIBLE_DEVICES=0 swift deploy \
    --model Qwen/Qwen2.5-7B-Instruct \
    --infer_backend vllm \
    --max_new_tokens 2048 \
    --served_model_name Qwen2.5-7B-Instruct
```

客户端调用测试：

```
# test env: pip install transformers==4.49.* vllm==0.7.3
CUDA_VISIBLE_DEVICES=0 \
MAX_PIXELS=1003520 \
VIDEO_MAX_PIXELS=50176 \
FPS_MAX_FRAMES=12 \
swift deploy \
    --model Qwen/Qwen2.5-VL-3B-Instruct \
    --infer_backend vllm \
    --gpu_memory_utilization 0.9 \
    --max_model_len 8192 \
    --max_new_tokens 2048 \
    --limit_mm_per_prompt &#x27;{&quot;image&quot;: 5, &quot;video&quot;: 2}&#x27; \
    --served_model_name Qwen2.5-VL-3B-Instruct
```

### 客户端#

这里介绍3种调用客户端的方式，分别是curl、openai库和swift客户端。

方案一: curl

```
curl http://localhost:8000/v1/chat/completions \
-H &quot;Content-Type: application/json&quot; \
-d &#x27;{
&quot;model&quot;: &quot;Qwen2.5-VL-3B-Instruct&quot;,
&quot;messages&quot;: [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: [
    {&quot;type&quot;: &quot;image&quot;, &quot;image&quot;: &quot;http://modelscope-open.oss-cn-hangzhou.aliyuncs.com/images/cat.png&quot;},
    {&quot;type&quot;: &quot;image&quot;, &quot;image&quot;: &quot;http://modelscope-open.oss-cn-hangzhou.aliyuncs.com/images/animal.png&quot;},
    {&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: &quot;两张图的区别是什么？&quot;}
]}],
&quot;max_tokens&quot;: 256,
&quot;temperature&quot;: 0
}&#x27;
```

方案2: openai库

```
from openai import OpenAI

client = OpenAI(
    api_key=&#x27;EMPTY&#x27;,
    base_url=f&#x27;http://127.0.0.1:8000/v1&#x27;,
)
model = client.models.list().data[0].id
print(f&#x27;model: {model}&#x27;)

messages = [{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: [
    {&#x27;type&#x27;: &#x27;video&#x27;, &#x27;video&#x27;: &#x27;https://modelscope-open.oss-cn-hangzhou.aliyuncs.com/images/baby.mp4&#x27;},
    {&#x27;type&#x27;: &#x27;text&#x27;, &#x27;text&#x27;: &#x27;describe the video&#x27;}
]}]

resp = client.chat.completions.create(model=model, messages=messages, max_tokens=512, temperature=0)
query = messages[0][&#x27;content&#x27;]
response = resp.choices[0].message.content
print(f&#x27;query: {query}&#x27;)
print(f&#x27;response: {response}&#x27;)

# base64
import base64
import requests
resp = requests.get(&#x27;https://modelscope-open.oss-cn-hangzhou.aliyuncs.com/images/baby.mp4&#x27;)
base64_encoded = base64.b64encode(resp.content).decode(&#x27;utf-8&#x27;)
messages = [{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: [
    {&#x27;type&#x27;: &#x27;video&#x27;, &#x27;video&#x27;: f&#x27;data:video/mp4;base64,{base64_encoded}&#x27;},
    {&#x27;type&#x27;: &#x27;text&#x27;, &#x27;text&#x27;: &#x27;describe the video&#x27;}
]}]

gen = client.chat.completions.create(model=model, messages=messages, stream=True, temperature=0)
print(f&#x27;query: {query}\nresponse: &#x27;, end=&#x27;&#x27;)
for chunk in gen:
    if chunk is None:
        continue
    print(chunk.choices[0].delta.content, end=&#x27;&#x27;, flush=True)
print()
```

方案三：swift客户端

```
from swift.llm import InferRequest, InferClient, RequestConfig
from swift.plugin import InferStats


engine = InferClient(host=&#x27;127.0.0.1&#x27;, port=8000)
print(f&#x27;models: {engine.models}&#x27;)
metric = InferStats()
request_config = RequestConfig(max_tokens=512, temperature=0)

# 这里使用了3个infer_request来展示batch推理
# 支持传入本地路径、base64和url
infer_requests = [
    InferRequest(messages=[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;who are you?&#x27;}]),
    InferRequest(messages=[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;&lt;image&gt;&lt;image&gt;两张图的区别是什么？&#x27;}],
                 images=[&#x27;http://modelscope-open.oss-cn-hangzhou.aliyuncs.com/images/cat.png&#x27;,
                         &#x27;http://modelscope-open.oss-cn-hangzhou.aliyuncs.com/images/animal.png&#x27;]),
    InferRequest(messages=[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;&lt;video&gt;describe the video&#x27;}],
                 videos=[&#x27;https://modelscope-open.oss-cn-hangzhou.aliyuncs.com/images/baby.mp4&#x27;]),
]

resp_list = engine.infer(infer_requests, request_config, metrics=[metric])
print(f&#x27;response0: {resp_list[0].choices[0].message.content}&#x27;)
print(f&#x27;response1: {resp_list[1].choices[0].message.content}&#x27;)
print(f&#x27;response2: {resp_list[2].choices[0].message.content}&#x27;)
print(metric.compute())
metric.reset()

# base64
import base64
import requests
resp = requests.get(&#x27;https://modelscope-open.oss-cn-hangzhou.aliyuncs.com/images/baby.mp4&#x27;)
base64_encoded = base64.b64encode(resp.content).decode(&#x27;utf-8&#x27;)
messages = [{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: [
    {&#x27;type&#x27;: &#x27;video&#x27;, &#x27;video&#x27;: f&#x27;data:video/mp4;base64,{base64_encoded}&#x27;},
    {&#x27;type&#x27;: &#x27;text&#x27;, &#x27;text&#x27;: &#x27;describe the video&#x27;}
]}]
infer_request = InferRequest(messages=messages)
request_config = RequestConfig(max_tokens=512, temperature=0, stream=True)
gen_list = engine.infer([infer_request], request_config, metrics=[metric])
print(f&#x27;response0: &#x27;, end=&#x27;&#x27;)
for chunk in gen_list[0]:
    if chunk is None:
        continue
    print(chunk.choices[0].delta.content, end=&#x27;&#x27;, flush=True)
print()
print(metric.compute())
```

我们也提供了更多部署的demo：

+   多LoRA部署与调用: 参考[这里](https://github.com/modelscope/ms-swift/tree/main/examples/deploy/lora)。

+   [Ba](https://github.com/modelscope/ms-swift/tree/main/examples/deploy/client/llm/base)se模型的部署与调用: 参考这里。

+   更多模型类型: 我们提供了[bert](https://github.com/modelscope/ms-swift/tree/main/examples/deploy/bert)、[reward_model](https://github.com/modelscope/ms-swift/tree/main/examples/deploy/reward_model)的部署脚本。