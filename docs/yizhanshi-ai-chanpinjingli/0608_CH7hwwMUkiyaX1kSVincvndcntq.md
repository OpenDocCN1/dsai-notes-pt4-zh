# 预训练与微调

> 来源：[https://v11enp9ok1h.feishu.cn/docx/KRc8doOJ1o7e2pxP2YYc5N3In1d](https://v11enp9ok1h.feishu.cn/docx/KRc8doOJ1o7e2pxP2YYc5N3In1d)

# 预训练与微调

训练能力：

## 环境准备#

推荐的第三方库版本参考[SWIFT安装文档](https://modelscope.cn/docs/llm-training-and-inference/intro/swift-installation)

pip install ms-swift -U

# 若使用deepspeed zero2/zero3

pip install deepspeed -U

## 预训练#

> 预训练使用swift pt命令，这将自动使用生成式而非对话式的template，即将use_ch[at_templ](https://modelscope.cn/docs/llm-training-and-inference/customization/custom-dataset)ate设置为False（其他所有的命令，例如swift sft/rlhf/infer，都默认将use_chat_template设置为True）。此外，swift pt与swift sft相比，具有不同的数据集格式，可以参考自定义数据集文档。

[使用](https://github.com/modelscope/swift/blob/main/examples/train/pretrain/train.sh)CLI进行预训练的脚本可以参考这里。更多训练技术的介绍可以参考微调章节。

**小贴士：**

+   swift pt与swift sft --use_chat_template false等价。

+   swift pt通常会使用大数据集，建议与--streaming流式数据集结合使用。

## 微调

> ms-swift使用了分层式的设计思想，用户可以使用命令行界面、Web-UI界面和直接使用Python的方式进行微调。

### 使用CLI#

我[们提](https://modelscope.cn/docs/llm-training-and-inference/intro/quickstart)供了10分钟在单卡3090上对Qwen2.5-7B-Instruct进行自我认知微调的最佳实践，具体参考这里，这可以帮助您快速了解SWIFT。

此外，我们给出了一系列脚本帮助您了解SWIFT的训练能力：

+   [轻量](https://github.com/modelscope/swift/blob/main/examples/train/tuners)化训练：SWIFT支持的轻量微调示例可以参考这里。（注意：这些方式预训练也可以使用，但预训练通常使用全参数训练）。

+   分布式训练：SWIFT支持的分布式训练技术包括：DDP、device_map、DeepSpeed ZeRO2/ZeRO3、FSDP。

+   device_map: 简易模型并行。如果存在多GPU，device_map会自动开启。这会将模型按层均匀的划分到可见的GPU中，显著降低显存消耗，但是训练速度通常会降低，因为是串行的。

+   DDP+device_map：将按组[对模](https://github.com/modelscope/ms-swift/blob/main/examples/train/multi-gpu/ddp_device_map/train.sh)型进行device_map划分，参考这里。

+   De[ep](https://github.com/modelscope/ms-swift/tree/main/examples/train/multi-gpu/deepspeed)Speed ZeRO2/ZeRO3: 节约显存资源，但训练速度下降。ZeRO2将对优化器状态、模型梯度进行分片。ZeRO3在ZeRO2基础上，对模型参数进行分片，更加节约显存，但训练速度更慢。参考这里。

+   [FS](https://github.com/modelscope/ms-swift/tree/main/examples/train/multi-gpu/fsdp_qlora/train.sh)DP+QLoRA: 双卡3090运行70B模型的训练，参考这里。

+   多机多卡训练: 我们书写了使用swift、torchrun、dlc、deepspeed、accelerate[启动](https://github.com/modelscope/swift/blob/main/examples/train/multi-node)多节点运行的shell脚本示例。除了dlc和deepspeed，其他启动脚本都需要在所有节点中启动才可运行。具体参考这里。

+   量[化训](https://github.com/modelscope/ms-swift/tree/main/examples/train/qlora)练：支持使用GPTQ、AWQ、AQLM、BNB、HQQ、EETQ量化技术的QLoRA训练。微调7B模型只需要9GB显存资源。具体参考这里。

+   多模态训练：SWIFT支持多模态模型的预训练、微调和RLH[F。支持Capti](https://github.com/modelscope/ms-swift/blob/main/examples/notebook/qwen2_5-vl-grounding/zh.ipynb)[on](https://github.com/modelscope/ms-swift/tree/main/examples/train/multimodal)、VQA、OCR、Groun[ding任务。支](https://modelscope.cn/docs/llm-training-and-inference/customization/custom-dataset)持图像、视频和音频三种模态。具体参考这里。多模态自定义数据集格式参考自定义数据集文档。

+   对ViT/Aligner使用全参数训练，LLM使用[Lo](https://github.com/modelscope/ms-swift/tree/main/examples/train/multimodal/lora_llm_full_vit)RA训练，并采用不同学习率的例子参考这里。

+   [多模](https://github.com/modelscope/ms-swift/tree/main/examples/train/packing)态模型packing，增加训练速度，例子参考这里。

+   RLHF训练：参考[这里](https://github.com/modelscope/ms-swift/tree/main/examples/train/rlhf)。多模态模型参考[这里](https://github.com/modelscope/ms-swift/tree/main/examples/train/multimodal/rlhf)。GRPO训练参考[这里](https://github.com/modelscope/ms-swift/blob/main/examples/train/grpo/grpo_zero2.sh)。强化微调查看[这里](https://github.com/modelscope/ms-swift/tree/main/examples/train/rft)。

+   Megatron训练：支持使用Megatron的并行技术来加速大模型的训练，包括数据并行、张量并行、流水线并行、序列并行，上下文并行。参考Megatron-SWIFT训练文档。

+   序列分类模型训练：参考[这里](https://github.com/modelscope/ms-swift/tree/main/examples/train/seq_cls)。

+   [Em](https://github.com/modelscope/ms-swift/tree/main/examples/train/embedding)bedding模型训练：参考这里

+   Agent训练：参考[这里](https://github.com/modelscope/swift/blob/main/examples/train/agent)。

+   [An](https://github.com/modelscope/swift/blob/main/examples/train/all_to_all)y-to-Any模型训练：参考这里。

+   其他能力：

+   [数据](https://github.com/modelscope/swift/blob/main/examples/train/streaming/train.sh)流式读取: 在数据量较大时减少内存使用。参考这里。

+   p[ac](https://github.com/modelscope/swift/blob/main/examples/train/packing/train.sh)king: 将多个序列拼成一个，让每个训练样本尽可能接近max_length，提高显卡利用率，参考这里。

+   长文本训练: 参考[这里](https://github.com/modelscope/swift/blob/main/examples/train/long_text)。

+   la[zy](https://github.com/modelscope/swift/blob/main/examples/train/lazy_tokenize/train.sh) tokenize: 在训练期间对数据进行tokenize而不是在训练前tokenize（多模态模型可以避免在训练前读入所有多模态资源），这可以避免预处理等待并节约内存。参考这里。

小帖士：

+   在使用swift sft通过LoRA技术微调base模型为chat模型时，有时需要手动设置模板。通过添加[--](https://github.com/modelscope/ms-swift/tree/main/examples/train/base_to_chat)template default参数来避免base模型因未见过对话模板中的特殊字符而无法正常停止的情况。具体参考这里。

+   如果需要在断网环境下进行训练，请设置--model <model_dir>和--check_model fals[e。如果对应的](https://modelscope.cn/docs/llm-training-and-inference/user-guide/%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%8F%82%E6%95%B0.md)模型需要git clonegithub的仓库，例如deepseek-ai/Janus-Pro-7B，请设置手动下载仓库，并设置--local_repo_path <repo_dir>。具体参数含义请参考命令行参数文档。

+   无法对QLoRA训练的模型进行Merge LoRA，因此不建议使用QLoRA进行微调，无法在推理和部署时使用vL[LM](https://github.com/modelscope/ms-swift/tree/main/examples/export/quantize)/LMDeploy进行推理加速。建议使用LoRA/全参数进行微调，合并为完整权重后再使用GPTQ/AWQ/BNB进行量化。

+   如果使用NPU进行训练，只需要将shell中的CUDA_VISIBLE_DEVICES修改为ASCEND_RT_VISIBLE_DEVICES。

+   SWIFT默认在训练时设置--gradient_checkpointing true来节约显存，这会略微降低训练速度。

+   若使用DDP进行训练，出现报错：RuntimeError: Expected to mark a variable ready only once.，请额外设置参数--gradient_checkpointing_kwargs '{"use_reentrant": false}'或者使用DeepSpeed进行训练。

+   如果要使用deepspeed，你需要安装deepspeed：pip install deepspeed -U。使用deepspeed可以节约显存，但会略微降低训练速度。

+   如果您的机器是A100等高性能显卡，且[模型支持flash-](https://github.com/Dao-AILab/flash-attention/releases)attn，推荐你安装flash-attn，并设置--attn_impl flash_attn，这将会加快训练和推理的速度并略微降低显存占用。

如何debug：

你[可以](https://github.com/modelscope/ms-swift/blob/main/swift/cli/sft.py)使用以下方式进行debug，这与使用命令行微调是等价的，但此方式不支持分布式。微调命令行运行入口可以查看这里。

```
from swift.llm import sft_main, TrainArguments
result = sft_main(TrainArguments(
    model=&#x27;Qwen/Qwen2.5-7B-Instruct&#x27;,
    train_type=&#x27;lora&#x27;,
    dataset=[&#x27;AI-ModelScope/alpaca-gpt4-data-zh#500&#x27;,
             &#x27;AI-ModelScope/alpaca-gpt4-data-en#500&#x27;,
             &#x27;swift/self-cognition#500&#x27;],
    torch_dtype=&#x27;bfloat16&#x27;,
    # ...
))
```

### 使用Web-UI#

[如果你要使用界面](https://modelscope.cn/docs/llm-training-and-inference/intro/web-ui)的方式进行训练，可以查看Web-UI文档。

### 使用python#

+   [Qw](https://github.com/modelscope/ms-swift/blob/main/examples/notebook/qwen2_5-self-cognition/self-cognition-sft.ipynb)en2.5自我认知微调notebook查看这里。

+   [Qw](https://github.com/modelscope/ms-swift/blob/main/examples/notebook/qwen2vl-ocr/ocr-sft.ipynb)en2VL进行OCR任务notebook查看这里。

## Merge LoRA#

+   查看[这里](https://github.com/modelscope/ms-swift/blob/main/examples/export/merge_lora.sh)。

## 推理（微调后模型）

使用CLI对LoRA训练的checkpoint进行推理：

```
CUDA_VISIBLE_DEVICES=0 \
swift infer \
    --adapters output/vx-xxx/checkpoint-xxx \
    --infer_backend pt \
    --stream true \
    --temperature 0 \
    --max_new_tokens 2048
```

+   adapters文件夹中包含了训练的参数文件args.json，因此不需要额外指定--model，--system，swift会自动读取这些参数。如果要关闭此行为，可以设置--load_args false。

+   如果使用全参数训练，请使用--model替代--adapters指定训练的checkpoint目录。更多参考。

+   你可以使用swift app替代swift infer进行界面推理。

+   你可以选择对LoRA进行merge（额外指定--merge_lora true），然后指定--infer_backend vllm/lmdeploy进行推理加速。

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

+   你可以设置--max_batch_size 8，从而使用--infer_backend pt进行批量处理。若使用infer_backend vllm/lmdeploy则无需指定，会进行自动batch。

+   --load_data_args true会额外读取训练存储参数文件args.json中的数据参数。

若想对额外的测试集进行推理，而不使用训练时的验证集，使用--val_dataset <dataset_path>进行推理：

```
CUDA_VISIBLE_DEVICES=0 \
swift infer \
    --adapters output/vx-xxx/checkpoint-xxx \
    --infer_backend pt \
    --temperature 0 \
    --max_new_tokens 2048 \
    --val_dataset &lt;dataset-path&gt; \
    --max_batch_size 1
```

使用Python对训练后LoRA推理的例子如下：

```
import os
os.environ[&#x27;CUDA_VISIBLE_DEVICES&#x27;] = &#x27;0&#x27;

from swift.llm import (
    PtEngine, RequestConfig, safe_snapshot_download, get_model_tokenizer, get_template, InferRequest
)
from swift.tuners import Swift
# 请调整下面几行
model = &#x27;Qwen/Qwen2.5-7B-Instruct&#x27;
lora_checkpoint = safe_snapshot_download(&#x27;swift/test_lora&#x27;)  # 修改成checkpoint_dir
template_type = None  # None: 使用对应模型默认的template_type
default_system = &quot;You are a helpful assistant.&quot;  # None: 使用对应模型默认的default_system

# 加载模型和对话模板
model, tokenizer = get_model_tokenizer(model)
model = Swift.from_pretrained(model, lora_checkpoint)
template_type = template_type or model.model_meta.template
template = get_template(template_type, tokenizer, default_system=default_system)
engine = PtEngine.from_model_template(model, template, max_batch_size=2)
request_config = RequestConfig(max_tokens=512, temperature=0)

# 这里使用了2个infer_request来展示batch推理
infer_requests = [
    InferRequest(messages=[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;who are you?&#x27;}]),
    InferRequest(messages=[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;浙江的省会在哪？&#x27;},
                           {&#x27;role&#x27;: &#x27;assistant&#x27;, &#x27;content&#x27;: &#x27;浙江的省会在哪？&#x27;},
                           {&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;这里有什么好吃的&#x27;},]),
]
resp_list = engine.infer(infer_requests, request_config)
query0 = infer_requests[0].messages[0][&#x27;content&#x27;]
print(f&#x27;response0: {resp_list[0].choices[0].message.content}&#x27;)
print(f&#x27;response1: {resp_list[1].choices[0].message.content}&#x27;)
```

多模态模型的LoRA推理示例如下：

```
import os
os.environ[&#x27;CUDA_VISIBLE_DEVICES&#x27;] = &#x27;0&#x27;

from swift.llm import (
    PtEngine, RequestConfig, safe_snapshot_download, get_model_tokenizer, get_template, InferRequest
)
from swift.tuners import Swift
# 请调整下面几行
model = &#x27;Qwen/Qwen2.5-VL-7B-Instruct&#x27;
lora_checkpoint = safe_snapshot_download(&#x27;swift/test_grounding&#x27;)  # 修改成checkpoint_dir
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
    InferRequest(messages=[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;who are you?&#x27;}]),
    InferRequest(messages=[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;&lt;image&gt;Task: Object Detection&#x27;}],
                 images=[&#x27;http://modelscope-open.oss-cn-hangzhou.aliyuncs.com/images/animal.png&#x27;]),
]
resp_list = engine.infer(infer_requests, request_config)
query0 = infer_requests[0].messages[0][&#x27;content&#x27;]
print(f&#x27;response0: {resp_list[0].choices[0].message.content}&#x27;)
print(f&#x27;response1: {resp_list[1].choices[0].message.content}&#x27;)
```

如果使用ms-swift训练的模型，可以通过以下方式获取训练的配置：

```
from swift.llm import safe_snapshot_download, BaseArguments

lora_adapters = safe_snapshot_download(&#x27;swift/test_lora&#x27;)
args = BaseArguments.from_pretrained(lora_adapters)
print(f&#x27;args.model: {args.model}&#x27;)
print(f&#x27;args.model_type: {args.model_type}&#x27;)
print(f&#x27;args.template_type: {args.template}&#x27;)
print(f&#x27;args.default_system: {args.system}&#x27;)
```

+   对全参数训练的checkpoint进行推理，将model设置为checkpoint_dir，并将lora_checkpoint设置为None即可。更多参考。

+   使用流式推理以及VllmEngine、LmdeployEngine进行推理加速，可以参考[大模型](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo.py)和[多模态大模型](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo_mllm.py)推理示例。

+   微调后的模型使用huggingface transf[or](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo_hf.py)mers/peft生态推理，可以参考这里。

+   [若训](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo_lora.py)练[了多](https://github.com/modelscope/ms-swift/tree/main/examples/deploy/lora)个LoRA，要进行多LoRA切换，可以参考推理、部署样例。

+   [对多](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo_grounding.py)模态模型进行Grounding任务的画框，可以参考这里。

+   [对L](https://github.com/modelscope/ms-swift/blob/main/examples/infer/demo_bert.py)oRA微调后的Bert进行推理，可以参考这里。

## 部署（微调后模型）#

使用以下命令启动部署服务端。如果权重使用全参数训练，请使用--model替代--adapters指定训练的checkpoint目录。你可以参考介绍的客户端调用方式：curl、openai库和swift客户端进行调用。

```
CUDA_VISIBLE_DEVICES=0 \
swift deploy \
    --adapters output/vx-xxx/checkpoint-xxx \
    --infer_backend pt \
    --temperature 0 \
    --max_new_tokens 2048 \
    --served_model_name &#x27;&lt;model-name&gt;&#x27;
```

这里将给出使用vLLM对多LoRA进行部署并调用的完整例子。

### 服务端#

首先你需要安装vLLM：pip install vllm -U，并在部署时使用--infer_backend vllm，这通常可以显著加速推理速度。

我们预先训练了2个基模型为[Qwen/Qwen](https://modelscope.cn/models/swift/test_lora/file/view/master)2.5-7B-Instruct的不同自我认知LoRA增量权重（可以直接跑通），我们可以在args.json中找到相关信息。你需要在部署时修改--adapters指定训练好的LoRA权重本地路径即可。

```
CUDA_VISIBLE_DEVICES=0 \
swift deploy \
    --adapters lora1=swift/test_lora lora2=swift/test_lora2 \
    --infer_backend vllm \
    --temperature 0 \
    --max_new_tokens 2048
```

### 客户端#

这里只介绍使用openai库进行调用。使用curl、swift客户端调用的例子可以参考。

```
from openai import OpenAI

client = OpenAI(
    api_key=&#x27;EMPTY&#x27;,
    base_url=f&#x27;http://127.0.0.1:8000/v1&#x27;,
)
models = [model.id for model in client.models.list().data]
print(f&#x27;models: {models}&#x27;)

query = &#x27;who are you?&#x27;
messages = [{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: query}]

resp = client.chat.completions.create(model=models[1], messages=messages, max_tokens=512, temperature=0)
query = messages[0][&#x27;content&#x27;]
response = resp.choices[0].message.content
print(f&#x27;query: {query}&#x27;)
print(f&#x27;response: {response}&#x27;)

gen = client.chat.completions.create(model=models[2], messages=messages, stream=True, temperature=0)
print(f&#x27;query: {query}\nresponse: &#x27;, end=&#x27;&#x27;)
for chunk in gen:
    if chunk is None:
        continue
    print(chunk.choices[0].delta.content, end=&#x27;&#x27;, flush=True)
print()
&quot;&quot;&quot;
models: [&#x27;Qwen2.5-7B-Instruct&#x27;, &#x27;lora1&#x27;, &#x27;lora2&#x27;]
query: who are you?
response: I am an artificial intelligence model named swift-robot, developed by swift. I can answer your questions, provide information, and engage in conversation. If you have any inquiries or need assistance, feel free to ask me at any time.
query: who are you?
response: I am an artificial intelligence model named Xiao Huang, developed by ModelScope. I can answer your questions, provide information, and engage in conversation. If you have any inquiries or need assistance, feel free to ask me at any time.
&quot;&quot;&quot;
```