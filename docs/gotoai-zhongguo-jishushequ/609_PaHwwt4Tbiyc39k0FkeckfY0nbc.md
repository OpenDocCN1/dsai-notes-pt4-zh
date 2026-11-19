
# InferLLM-轻量级单机多模型推理框架（开箱即用）

> 来源：https://gotoaiworld.feishu.cn/docx/YAqvdqmIDouhQwxyuihcTcuxnsd

## InferLLM（LLM 推理显存优化神器）

`项目地址`：`https://github.com/Oneflow-Inc/InferLLM`

`核心能力`：

+ 专为 `LLM` 优化的推理框架，支持自动量化（`INT4`/`INT8`）、显存分片；
+ 多模型动态调度，模型关闭时自动清理显存缓存；
+ 比原生 `PyTorch` 显存占用降低 50% 以上；
+ `简化点`：统一接口加载多模型，无需关注显存细节：

```python
from inferllm import AutoModel

# 加载模型1（自动量化+显存优化）
model1 = AutoModel.from_pretrained("llama-7b", model_type="llama", gpu_memory=4)
model1.generate("Hello!")# 释放模型1显存del model1
# 加载模型2（复用显存）
model2 = AutoModel.from_pretrained("baichuan-7b", model_type="baichuan", gpu_memory=4)
```
