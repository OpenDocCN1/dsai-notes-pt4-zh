
# MInference：微软出品，长上下文LLM推理提速10倍

> 来源：[https://agijuejin.feishu.cn/docx/Lvz0dlW6LoLv9MxY2exc3ARcnlg](https://agijuejin.feishu.cn/docx/Lvz0dlW6LoLv9MxY2exc3ARcnlg)
>
> 作者：lucas大叔
>
> 原文：<https://zhuanlan.zhihu.com/p/713480349>

`MInference` 1.0 利用 [`LLM`](https://www.zhihu.com/search?q=%E7%A8%80%E7%96%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A713480349%7D) 注意力的动态稀疏特性（表现出一些静态模式）加速长上下文 `LLM` 的预填充。它首先离线确定每个头属于哪个稀疏模式，然后在线近似稀疏索引，用最优自定义内核动态计算注意力。在 A100 上，这种方法在保持准确率的同时，实现了高达 10 倍的预填充加速。

![](img/79b22d0caa37f47d7e550afcd27d9149.png)

## 快速启动

### 环境要求

+ `Torch`
+ `FlashAttention-2` (Optional)
+ `Triton` == 2.1.0

要使用 `MInference`，需首先用 `pip` 安装：

```
pip install minference
```

### 支持的模型

通用 `MInference` 支持任何解码 `LLM`，包括 `LLaMA` 风格的模型和 `Phi` 模型。该项目已经适配了市场上几乎所有可用的开源长上下文 `LLM`。如果你的模型不在支持列表中，可以按照 指南 手动生成 `稀疏头` 配置。

执行以下操作可以获取支持的 `LLM` 的完整列表：

```
from minference import get_support_models
get_support_models()
```

当前，支持如下 `LLM`：

+ `LLaMA-3.1`: [`meta-llama/Meta-Llama-3.1-8B-Instruct`](https://link.zhihu.com/?target=https%3A//huggingface.co/meta-llama/Meta-Llama-3.1-8B-Instruct)

+ `LLaMA-3`:
  + `gradientai/Llama-3-8B-Instruct-262k`
  + `gradientai/Llama-3-8B-Instruct-Gradient-1048k`
  + `gradientai/Llama-3-8B-Instruct-Gradient-4194k`
  + `gradientai/Llama-3-70B-Instruct-Gradient-262k`
  + `gradientai/Llama-3-70B-Instruct-Gradient-4194k`
  + `gradientai/Llama-3-70B-Instruct-Gradient-1048k`

+ `GLM-4`: `THUDM/glm-4-9b-chat-1m`

+ `Yi`: `01-ai/Yi-9B-200K`

+ `Phi-3`: `microsoft/Phi-3-mini-128k-instruct`

+ `Qwen2`: `Qwen/Qwen2-7B-Instruct`

### MInference使用

对HF

![](img/66a094473c73ae4b6f502821a60599a0.png)

对 `vLLM`，要求 `vllm>=0.4.1`

![](img/bcad66e1ddebc6c772023403aac8a729.png)

仅使用内核

![](img/ec87b816a51bed5dcca6e0f5c938d228.png)

本地 [`gradio` demo](https://www.zhihu.com/search?q=gradio%20demo&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A713480349%7D)

![](img/600a6ced9a7009496cd1f2f87375efdc.png)

更多详细信息，请参阅 [`Examples`](https://link.zhihu.com/?target=https%3A//github.com/microsoft/MInference/tree/main/examples) 和 [`Experiments`](https://link.zhihu.com/?target=https%3A//github.com/microsoft/MInference/tree/main/experiments)。另外，有关动态编译器 `PIT` 的更多信息请参阅 `paper` 和 `GitHub`。

## 实操验证

验证 `THUDM/glm-4-9b-chat-1m` 模型使用，首先导入必要的包：

```
import torch
from minference import MInference
from transformers import AutoTokenizer, AutoModelForCausalLM
```

### 加载本地LLM

由于项目有加载 `LLM` 验证，所以为了成功加载本地事先已下载的 `LLM`，需要把执行的 `.py` 文件和 `LLM` 按如下结构存储：

![](img/29823f8212e8866b8283a7edf34147aa.png)

### 配置model和prompt

```
model_name = "THUDM/glm-4-9b-chat-1m"
prompt = '''
你是信息抽取专家，请抽取文本中的姓名：

# 文本内容
刘德华（Andy Lau），1961年9月27日出生于中国香港，祖籍广东江门荷塘镇，华语影视男演员、流行乐歌手、电影制片人、作词人。

'''
```

### 加载tokenizer和model

```
tokenizer = AutoTokenizer.from_pretrained(model_name, trust_remote_code=True)
message = [{"role": "user", "content": prompt}]
inputs = tokenizer.apply_chat_template(
            message,
            add_generation_prompt=True,
            tokenize=True,
            return_tensors="pt",
            return_dict=True
        )
inputs = inputs.to("cuda")

model = AutoModelForCausalLM.from_pretrained(
    model_name,
    torch_dtype=torch.bfloat16,
    low_cpu_mem_usage=True,
    trust_remote_code=True,
).to("cuda").eval()

minference_patch = MInference("minference", model_name)
model = minference_patch(model)
```

### 生成响应

```
gen_kwargs = {"max_length": 1024, "do_sample": True, "top_k": 1}
with torch.no_grad():
    outputs = model.generate(**inputs, **gen_kwargs)
    outputs = outputs[:, inputs['input_ids'].shape[1]:]
    response = tokenizer.decode(outputs[0], skip_special_tokens=True)
    print(response)
```

返回结果为

```
根据您提供的信息，刘德华（Andy Lau）
```

## 参考文档

- [`https://github.com/microsoft/MInference`](https://link.zhihu.com/?target=https%3A//github.com/microsoft/MInference)
- [`https://github.com/microsoft/MInference/blob/main/examples/run_hf.py`](https://link.zhihu.com/?target=https%3A//github.com/microsoft/MInference/blob/main/examples/run_hf.py)
- `glm-4-9b-chat-1m`
