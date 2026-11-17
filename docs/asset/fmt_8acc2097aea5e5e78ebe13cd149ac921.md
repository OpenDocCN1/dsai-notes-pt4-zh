
# RTX 4090 24G 环境下 DeepSeek-R1-Distill-Qwen-7B 模型部署指南

## 配置步骤

1. `7B` 模型需通过 `--cuda-memory-fraction 0.7-0.8` 适配 RTX 4090 24G 显存，避免 OOM；

1. 模型名称需完整包含“7B”，本地部署路径严格区分大小写；

1. 长文本推理（`--max-model-len 8192`）需降低显存利用率至 0.75，平衡性能与稳定性。

按上述步骤操作，即可在 RTX 4090 24G 环境下流畅运行 DeepSeek-R1-Distill-Qwen-7B 模型，实现高效推理与 API 调用。若遇到问题，可参考“避坑指南”或通过技术社区交流。
