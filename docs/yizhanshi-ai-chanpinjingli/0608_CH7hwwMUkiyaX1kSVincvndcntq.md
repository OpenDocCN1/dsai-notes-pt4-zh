
# 预训练与微调

> 来源：[https://v11enp9ok1h.feishu.cn/docx/KRc8doOJ1o7e2pxP2YYc5N3In1d](https://v11enp9ok1h.feishu.cn/docx/KRc8doOJ1o7e2pxP2YYc5N3In1d)

训练能力：

## 环境准备

推荐的第三方库版本参考[SWIFT安装文档](https://modelscope.cn/docs/llm-training-and-inference/intro/swift-installation)

`pip install ms-swift -U`

### 若使用deepspeed zero2/zero3

`pip install deepspeed -U`

## 预训练

> 预训练使用`swift pt`命令，这将自动使用生成式而非对话式的`template`，即将`use_chat_template`设置为`False`（其他所有的命令，例如`swift sft`、`swift rlhf`、`swift infer`，都默认将`use_chat_template`设置为`True`）。此外，`swift pt`与`swift sft`相比，具有不同的数据集格式，可以参考自定义数据集文档。

[使用CLI](https://github.com/modelscope/swift/blob/main/examples/train/pretrain/train.sh)进行预训练的脚本可以参考这里。更多训练技术的介绍可以参考微调章节。

小贴士：

+   `swift pt`与`swift sft --use_chat_template false`等价。

+   `swift pt`通常会使用大数据集，建议与`--streaming`流式数据集结合使用。

## 微调

> `ms-swift`使用了分层式的设计思想，用户可以使用命令行界面、`Web-UI`界面和直接使用`Python`的方式进行微调。

### 使用CLI

我们提供了10分钟在单卡3090上对`Qwen2.5-7B-Instruct`进行自我认知微调的最佳实践，具体参考这里，这可以帮助您快速了解`SWIFT`。

此外，我们给出了一系列脚本帮助您了解`SWIFT`的训练能力：

+   [轻量化训练](https://github.com/modelscope/swift/blob/main/examples/train/tuners)：`SWIFT`支持的轻量微调示例可以参考这里。（注意：这些方式预训练也可以使用，但预训练通常使用全参数训练）。

+   分布式训练：`SWIFT`支持的分布式训练技术包括：`DDP`、`device_map`、`DeepSpeed`、`ZeRO2`、`ZeRO3`、`FSDP`。

+   `device_map`: 简易模型并行。如果存在多`GPU`，`device_map`会自动开启。这会将模型按层均匀的划分到可见的`GPU`中，显著降低显存消耗，但是训练速度通常会降低，因为是串行的。

+   `DDP+device_map`：将按组对模型进行`device_map`划分，参考这里。

+   `DeepSpeed ZeRO2`/`ZeRO3`: 节约显存资源，但训练速度下降。`ZeRO2`将对优化器状态、模型梯度进行分片。`ZeRO3`在`ZeRO2`基础上，对模型参数进行分片，更加节约显存，但训练速度更慢。参考这里。

+   [`FSDP+QLoRA`](https://github.com/modelscope/ms-swift/tree/main/examples/train/multi-gpu/fsdp_qlora/train.sh): 双卡3090运行70B模型的训练，参考这里。

+   多机多卡训练: 我们书写了使用`swift`、`torchrun`、`dlc`、`deepspeed`、`accelerate`启动多节点运行的shell脚本示例。除了`dlc`和`deepspeed`，其他启动脚本都需要在所有节点中启动才可运行。具体参考这里。

+   量化训练：支持使用`GPTQ`、`AWQ`、`AQLM`、`BNB`、`EETQ`量化技术的`QLoRA`训练。微调7B模型只需要9GB显存资源。具体参考这里。

+   多模态训练：`SWIFT`支持多模态模型的预训练、微调和`RLHF`。支持`Caption`、`VQA`、`OCR`、`Grounding`任务。支持图像、视频和音频三种模态。具体参考这里。多模态自定义数据集格式参考自定义数据集文档。

+   对`ViT/Aligner`使用全参数训练，`LLM`使用`LoRA`训练，并采用不同学习率的例子参考这里。

+   [多模态模型packing](https://github.com/modelscope/ms-swift/tree/main/examples/train/packing)，增加训练速度，例子参考这里。

+   `RLHF`训练：参考[这里](https://github.com/modelscope/ms-swift/tree/main/examples/train/rlhf)。多模态模型参考[这里](https://github.com/modelscope/ms-swift/tree/main/examples/train/multimodal/rlhf)。`GRPO`训练参考[这里](https://github.com/modelscope/ms-swift/blob/main/examples/train/grpo/grpo_zero2.sh)。强化微调查看[这里](https://github.com/modelscope/ms-swift/tree/main/examples/train/rft)。

+   `Megatron`训练：支持使用`Megatron`的并行技术来加速大模型的训练，包括数据并行、张量并行、流水线并行、序列并行，上下文并行。参考`Megatron-SWIFT`训练文档。

+   序列分类模型训练：参考[这里](https://github.com/modelscope/ms-swift/tree/main/examples/train/seq_cls)。

+   [`Embedding`](https://github.com/modelscope/ms-swift/tree/main/examples/train/embedding)模型训练：参考这里

+   `Agent`训练：参考[这里](https://github.com/modelscope/swift/blob/main/examples/train/agent)。

+   [Any-to-Any模型训练](https://github.com/modelscope/swift/blob/main/examples/train/all_to_all)：参考这里。

+   其他能力：

+   [数据流式读取](https://github.com/modelscope/swift/blob/main/examples/train/streaming/train.sh): 在数据量较大时减少内存使用。参考这里。

+   [`packing`](https://github.com/modelscope/swift/blob/main/examples/train/packing/train.sh): 将多个序列拼成一个，让每个训练样本尽可能接近`max_length`，提高显卡利用率，参考这里。

+   长文本训练: 参考[这里](https://github.com/modelscope/swift/blob/main/examples/train/long_text)。

+   [`lazy tokenize`](https://github.com/modelscope/swift/blob/main/examples/train/lazy_tokenize/train.sh): 在训练期间对数据进行`tokenize`而不是在训练前`tokenize`（多模态模型可以避免在训练前读入所有多模态资源），这可以避免预处理等待并节约内存。参考这里。

小贴士：

+   在使用`swift sft`通过`LoRA`技术微调`base`模型为`chat`模型时，有时需要手动设置模板。通过添加`--template default`参数来避免`base`模型因未见过对话模板中的特殊字符而无法正常停止的情况。具体参考这里。

+   如果需要在断网环境下进行训练，请设置`--model <model_dir>`和`--check_model false`。如果对应的模型需要`git clone`github的仓库，例如`deepseek-ai/Janus-Pro-7B`，请设置手动下载仓库，并设置`--local_repo_path <repo_dir>`。具体参数含义请参考命令行参数文档。

+   无法对`QLoRA`训练的模型进行`Merge LoRA`，因此不建议使用`QLoRA`进行微调，无法在推理和部署时使用`vLLM`/`LMDeploy`进行推理加速。建议使用`LoRA`/全参数进行微调，合并为完整权重后再使用`GPTQ`/`AWQ`/`BNB`进行量化。

+   如果使用`NPU`进行训练，只需要将shell中的`CUDA_VISIBLE_DEVICES`修改为`ASCEND_RT_VISIBLE_DEVICES`。

+   `SWIFT`默认在训练时设置`--gradient_checkpointing true`来节约显存，这会略微降低训练速度。

+   若使用`DDP`进行训练，出现报错：`RuntimeError: Expected to mark a variable ready only once.`，请额外设置参数`--gradient_checkpointing_kwargs '{"use_reentrant": false}'`或者使用`DeepSpeed`进行训练。

+   如果要使用`deepspeed`，你需要安装`deepspeed`：`pip install deepspeed -U`。使用`deepspeed`可以节约显存，但会略微降低训练速度。

+   如果您的机器是`A100`等高性能显卡，且[模型支持flash-attn](https://github.com/Dao-AILab/flash-attention/releases)，推荐你安装`flash-attn`，并设置`--attn_impl flash_attn`，这将会加快训练和推理的速度并略微降低显存占用。

如何debug：

你可以使用以下方式进行debug，这与使用命令行微调是等价的，但此方式不支持分布式。微调命令行运行入口可以查看这里。

```
from swift.llm import sft_main, TrainArguments
result = sft_main(TrainArguments(
    model='Qwen/Qwen2.5-7B-Instruct',
    train_type='lora',
    dataset=['AI-ModelScope/alpaca-gpt4-data-zh#500',
             'AI-ModelScope/alpaca-gpt4-data-en#500',
             'swift/self-cognition#500'],
    torch_dtype='bfloat16',
    # ...
))
```

### 使用Web-UI

[如果你要使用界面](https://modelscope.cn/docs/llm-training-and-inference/intro/web-ui)的方式进行训练，可以查看`Web-UI`文档。

### 使用python

+   [Qwen2.5自我认知微调notebook](https://github.com/modelscope/ms-swift/blob/main/examples/notebook/qwen2_5-self-cognition/self-cognition-sft.ipynb)查看这里。

+   [Qwen2VL进行OCR任务notebook](https://github.com/modelscope/ms-swift/blob/main/examples/notebook/qwen2vl-ocr/ocr-sft.ipynb)查看这里。

## Merge LoRA

+   查看[这里](https://github.com/modelscope/ms-swift/blob/main/examples/export/merge_lora.sh)。

## 推理（微调后模型）

使用`CLI`对`LoRA`训练的`checkpoint`进行推理：

```
CUDA_VISIBLE_DEVICES=0 \
swift infer \
    --adapters output/vx-xxx/checkpoint-xxx \
    --infer_backend pt \
    --stream true \
    --temperature 0 \
    --max_new_tokens 2048
```

+   `adapters`文件夹中包含了训练的参数文件`args.json`，因此不需要额外指定`--model`，`--system`，`swift`会自动读取这些参数。如果要关闭此行为，可以设置`--load_args false`。

+   如果使用全参数训练，请使用`--model`替代`--adapters`指定训练的`checkpoint`目录。更多参考。

+   你可以使用`swift app`替代`swift infer`进行界面推理。

+   你可以选择对`LoRA`进行`merge`（额外指定`--merge_lora true`），然后指定`--infer_backend vllm/lmdeploy`进行推理加速。

对数据集中的验证集进行批量推理：

```
CUDA_VISIBLE_DEVICES=0 \
swift infer \
    --adapters output/vx-xxx/checkpoint-xxx \
    --infer_backend pt \
    --temperature 0 \
    --max_new_tokens 2048 \
    --load_data_args true \
    --max_batch_size 1
```

+   你可以设置`--max_batch_size 8`，从而使用`--infer_backend pt`进行批量处理。若使用`infer_backend vllm/lmdeploy`则无需指定，会进行自动`batch`。

+   `--load_data_args true`会额外读取训练存储参数文件`args.json`中的数据参数。

若想对额外的测试集进行推理，而不使用训练时的验证集，使用`--val_dataset <dataset_path>`进行推理：

```
CUDA_VISIBLE_DEVICES=0 \
swift infer \
    --adapters output/vx-xxx/checkpoint-xxx \
    --infer_backend pt \
    --temperature 0 \
    --max_new_tokens 2048 \
    --val_dataset <dataset_path> \
    --max_batch_size 1
```

使用`Python`对训练后`LoRA`推理的例子如下：



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
