
```python
import os
os.environ['CUDA_VISIBLE_DEVICES'] = '0'

from swift.llm import (
    PtEngine, RequestConfig, safe_snapshot_download, get_model_tokenizer, get_template, InferRequest
)
from swift.tuners import Swift
# 请调整下面几行
model = 'Qwen/Qwen2.5-7B-Instruct'
lora_checkpoint = safe_snapshot_download('swift/test_lora')  # 修改成checkpoint_dir
template_type = None  # None: 使用对应模型默认的template_type
default_system = "You are a helpful assistant."  # None: 使用对应模型默认的default_system

# 加载模型和对话模板
model, tokenizer = get_model_tokenizer(model)
model = Swift.from_pretrained(model, lora_checkpoint)
template_type = template_type or model.model_meta.template
template = get_template(template_type, tokenizer, default_system=default_system)
engine = PtEngine.from_model_template(model, template, max_batch_size=2)
request_config = RequestConfig(max_tokens=512, temperature=0)

# 这里使用了2个infer_request来展示batch推理
infer_requests = [
    InferRequest(messages=[{'role': 'user', 'content': 'who are you?'}]),
    InferRequest(messages=[{'role': 'user', 'content': '浙江的省会在哪？'},
                           {'role': 'assistant', 'content': '浙江的省会在哪？'},
                           {'role': 'user', 'content': '这里有什么好吃的'},]),
]
resp_list = engine.infer(infer_requests, request_config)
query0 = infer_requests[0].messages[0]['content']
print(f'response0: {resp_list[0].choices[0].message.content}')
print(f'response1: {resp_list[1].choices[0].message.content}')
```

多模态模型的LoRA推理示例如下：

`images=['http://modelscope-open.oss-cn-hangzhou.aliyuncs.com/images/animal.png']),`

```python
import os
os.environ['CUDA_VISIBLE_DEVICES'] = '0'

from swift.llm import (
    PtEngine, RequestConfig, safe_snapshot_download, get_model_tokenizer, get_template, InferRequest
)
from swift.tuners import Swift
# 请调整下面几行
model = 'Qwen/Qwen2.5-VL-7B-Instruct'
lora_checkpoint = safe_snapshot_download('swift/test_grounding')  # 修改成checkpoint_dir
template_type = None  # None: 使用对应模型默认的template_type
default_system = None  # None: 使用对应模型默认的default_system

# 加载模型和对话模板
model, tokenizer = get_model_tokenizer(model)
model = Swift.from_pretrained(model, lora_checkpoint)
template_type = template_type or model.model_meta.template
template = get_template(template_type, tokenizer, default_system=default_system)
engine = PtEngine.from_model_template(model, template, max_batch_size=2)
request_config = RequestConfig(max_tokens=512, temperature=0)

# 这里使用了2个infer_request来展示batch推理
infer_requests = [
    InferRequest(messages=[{'role': 'user', 'content': 'who are you?'}]),
    InferRequest(messages=[{'role': 'user', 'content': '<image>Task: Object Detection'}]),
]
resp_list = engine.infer(infer_requests, request_config)
query0 = infer_requests[0].messages[0]['content']
print(f'response0: {resp_list[0].choices[0].message.content}')
print(f'response1: {resp_list[1].choices[0].message.content}')
```

如果使用ms-swift训练的模型，可以通过以下方式获取训练的配置：

```python
from swift.llm import safe_snapshot_download, BaseArguments

lora_adapters = safe_snapshot_download('swift/test_lora')
args = BaseArguments.from_pretrained(lora_adapters)
print(f'args.model: {args.model}')
print(f'args.model_type: {args.model_type}')
print(f'args.template_type: {args.template}')
print(f'args.default_system: {args.system}')
```

+ 对全参数训练的`checkpoint`进行推理，将`model`设置为`checkpoint_dir`，并将`lora_checkpoint`设置为`None`即可。更多参考。

+ 使用流式推理以及`VllmEngine`、`LmdeployEngine`进行推理加速，可以参考[大模型](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo.py)和[多模态大模型](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo_mllm.py)推理示例。

+ 微调后的模型使用`huggingface [transformers](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo_hf.py)/peft`生态推理，可以参考这里。

+ `[若训](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo_lora.py)练[了多](https://github.com/modelscope/ms-swift/tree/main/examples/deploy/lora)个` `LoRA`，要进行多`LoRA`切换，可以参考推理、部署样例。

+ `[对多](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo_grounding.py)模态模型`进行`Grounding`任务的画框，可以参考这里。

+ `[对L](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo_bert.py)oRA`微调后的`Bert`进行推理，可以参考这里。

## 部署（微调后模型）

使用以下命令启动部署服务端。如果权重使用全参数训练，请使用`--model`替代`--adapters`指定训练的`checkpoint`目录。你可以参考介绍的客户端调用方式：`curl`、`openai`库和`swift`客户端进行调用。

```bash
CUDA_VISIBLE_DEVICES=0 \
swift deploy \
    --adapters output/vx-xxx/checkpoint-xxx \
    --infer_backend pt \
    --temperature 0 \
    --max_new_tokens 2048 \
    --served_model_name '<model-name>'
```

这里将给出使用`vLLM`对多`LoRA`进行部署并调用的完整例子。

### 服务端

首先你需要安装`vLLM`：`pip install vllm -U`，并在部署时使用`--infer_backend vllm`，这通常可以显著加速推理速度。

我们预先训练了2个基模型为[Qwen/Qwen](https://modelscope.cn/models/swift/test_lora/file/view/master)2.5-7B-Instruct的不同自我认知`LoRA`增量权重（可以直接跑通），我们可以在`args.json`中找到相关信息。你需要在部署时修改`--adapters`指定训练好的`LoRA`权重本地路径即可。

```bash
CUDA_VISIBLE_DEVICES=0 \
swift deploy \
    --adapters lora1=swift/test_lora lora2=swift/test_lora2 \
    --infer_backend vllm \
    --temperature 0 \
    --max_new_tokens 2048
```

### 客户端

这里只介绍使用`openai`库进行调用。使用`curl`、`swift`客户端调用的例子可以参考。

```python
from openai import OpenAI

client = OpenAI(
    api_key='EMPTY',
    base_url='http://127.0.0.1:8000/v1',
)
models = [model.id for model in client.models.list().data]
print(f'models: {models}')

query = 'who are you?'
messages = [{'role': 'user', 'content': query}]

resp = client.chat.completions.create(model=models[1], messages=messages, max_tokens=512, temperature=0)
query = messages[0]['content']
response = resp.choices[0].message.content
print(f'query: {query}')
print(f'response: {response}')

gen = client.chat.completions.create(model=models[2], messages=messages, stream=True, temperature=0)
print(f'query: {query}\nresponse: ', end='')
for chunk in gen:
    if chunk is None:
        continue
    print(chunk.choices[0].delta.content, end='', flush=True)
print()
"""
models: ['Qwen2.5-7B-Instruct', 'lora1', 'lora2']
query: who are you?
response: I am an artificial intelligence model named swift-robot, developed by swift. I can answer your questions, provide information, and engage in conversation. If you have any inquiries or need assistance, feel free to ask me at any time.
query: who are you?
response: I am an artificial intelligence model named Xiao Huang, developed by ModelScope. I can answer your questions, provide information, and engage in conversation. If you have any inquiries or need assistance, feel free to ask me at any time.
"""
```
