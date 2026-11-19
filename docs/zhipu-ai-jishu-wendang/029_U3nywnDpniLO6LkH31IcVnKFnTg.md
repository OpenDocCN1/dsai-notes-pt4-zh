
# 常见问题

> 来源：[https://ai.feishu.cn/docx/RybXdwJQGoG80cxbb8FcQEOynwh](https://ai.feishu.cn/docx/RybXdwJQGoG80cxbb8FcQEOynwh)

## 如何使用 INT4 版本？

+ 下载模型文件并在载入模型的时候修改代码：

```
model = AutoModelForCausalLM.from_pretrained(
    MODEL_PATH,
    trust_remote_code=True,
    quantization_config=BitsAndBytesConfig(load_in_4bit=True),
    torch_dtype=torch.bfloat16,
    low_cpu_mem_usage=True
).eval()
```

+ `vLLM` 暂不支持 INT4 模型推理。

+ 暂时没有提供 INT4 版本的开源模型的计划，如有，我们会第一时间更新模型。

+ INT4 模型不保证能正常适配所有 demo，请开发者自行探索。

## 安装依赖有问题？

+ 除 Linux 之外的操作系统暂不支持使用 `vLLM`。

+ 请升级 `pip` 或 `conda` 到最新版本，并推荐使用对应的官方源安装依赖，避免出现找不到依赖的问题。

+ 请确保你的 `CUDA` 版本与 `torch` 版本严格对应。

+ 多看 `README`，特别是各个子文件夹的 `README`。

## 不能使用 V100 等早期显卡推理和微调吗？

+ `github` 仓库上的代码均使用了 BF16 进行推理，虽然进行了测试，使用 FP16 推理也能正常运行，但是不保证出现小概率事件的溢出问题，因此，稳妥起见，推荐使用 BF16 推理。

+ FP16 无法微调，使用 FP16 微调会出现 `loss` 为 `nan` 的问题，请使用 BF16 格式微调。

## 模型支持 Ollama ，llamacpp 吗？

+ 不支持。

+ 为了支持 `GLM` 和 `GLM-4V` 模型结构（和 `llama` 不同），需要修改 `llama.cpp` 的代码并等待 `pr` 合并，这需要一段时间，感谢您的关注。

## 模型输出很短？

+ 本代模型的 `max_length` 是 128K，但是 `max_new_tokens` 是 4096。

## GLM-4V-9B 一共支持几张图片上传？

+ 一个完整的对话中，仅支持一张图片，不可更换，可以对单张图片做连续追问。

## 支持 1M 长度的推理需要多少配置

+ 使用 8 卡 * H 100 并使用 `vLLM` 后端能完整运行 1M 长度上下文的理解。单次推理时间约为 15 分钟。

## GLM-4-9B-Chat 和 GLM-4-9B-Chat-HF 模型有何区别

+ 更新至 `transformers` >= 4.4[6.0 时，请使用 [GLM-4-9B-Chat-HF](https://huggingface.co/THUDM/glm-4-9b-chat) 模型权重 `glm-4-9b-chat-hf`，如果您的 `transformers` 版本仍然低于 4.45，可以使用 `GLM-4-9B-Chat` 模型权重 `glm-4-9b-chat`。（`GLM-4-9B-Chat-1M` 模型同）

## 开源项目仓库更新规划

计划中：

不在计划中（不更新）：

## 贡献指南

如果您有技术问题，您需要在 `github` issue 提出：

https://github.com/THUDM/GLM-4/issues

如果您想贡献关于 `开源项目仓库更新规划` 中的代码，或者修改仓库中的 `bug`，您需要提出 PR：

https://github.com/THUDM/GLM-4/pulls

同时，请严格按照 [`X.x` 做出贡献](https://zhipu-ai.feishu.cn/wiki/WFx8wHev5ilQ9Tk7ERGcKa8MnVd) 中的要做出贡献。感谢您的支持。
