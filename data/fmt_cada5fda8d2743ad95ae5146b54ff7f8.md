
# MetaX C500 部署与适配说明

昇腾 NPU 无法安装 CUDA 原生的 `flash_attn` 库（因为 `flash_attn` 底层依赖 CUDA kernel），因此需要用 `PyTorch` 原生 `F.scaled_dot_product_attention` 手动替代全部 `flash_attn` 函数。

而 `MetaX C500` 的 `MACA` 框架已预装了兼容版本的 `flash_attn`（版本号 2.6.3+metax3.3.0.2torch2.8），其底层使用 `MACA` kernel 实现，对上层 Python API 完全兼容。模型可以直接调用，无需任何 Monkey Patch。

## 常见问题

### Q: 如何监控 GPU 状态？

```
# 实时监控（每秒刷新）
watch -n 1 mx-smi

# 查看 GPU 进程
mx-smi  # 底部会列出占用 GPU 的进程和显存
```

### Q: 服务启动后如何查看日志？

```
# 查看实时日志
tail -f /data/api_server.log

# 查看最近的请求记录
grep "POST\|GET" /data/api_server.log | tail -20
```

## 总结

`MetaX C500` 凭借 `MACA` 框架的 CUDA 兼容能力，实现了 `InternVL-U` 的开箱即用部署。这个方案的核心优势在于：

1. 零额外适配：`MACA` 预装 `FlashAttention2`，无需编写任何 `Patch`，部署流程比昇腾 NPU 更简单

2. 标准 CUDA API：使用 `torch.cuda` 接口，代码与 NVIDIA A100 完全一致，迁移成本为零

3. 功能完整：对话、图像理解、文生图、图片编辑四大能力全部可用

4. 性能优异：文生图速度接近 A100（~4-5s vs ~3.5s），文本推理速度快，显著优于 `SDPA` Fallback 方案

5. 国产替代：作为国产 GPU 方案，`MetaX C500` 展示了在 AI 推理场景下替代 NVIDIA GPU 的可行性

## 环境要求

| 项目 | 要求 |
| --- | --- |
| 硬件 | 沐曦 MetaX C500（64 GB VRAM，350W TDP） |
| 驱动 | `MACA` 3.3.0.4+（沐曦 GPU 计算框架） |
| `PyTorch` | 2.8.0（`MACA` 适配版，镜像已预装） |
| `Python` | 3.12（推荐使用镜像预装的 `Conda` 环境） |
| 操作系统 | Ubuntu 22.04 / 24.04 |
| 磁盘 | >= 30 GB（模型 ~8.2 GB + 代码 + 依赖） |
| 内存 | >= 32 GB（模型加载时需要） |

## 平台适配对比

| 平台 | `flash_attn` 库 | 是否需要 `Patch` |
| --- | --- | --- |
| NVIDIA A100 | 原生 CUDA 版本，`pip` 安装 | 不需要 |
| 华为昇腾 NPU | 不支持（CUDA-only），无法安装 | 需要 `SDPA` `Patch` |
| 沐曦 MetaX C500 | `MACA` 适配版本，环境预装 | 不需要 |

## 性能指标

| 能力 | 参数 | 耗时 | 备注 |
| --- | --- | --- | --- |
| 文本对话 | 短文本 ~64 tokens | 1~2 秒 | 首次推理含 warmup |
| 图像理解 | 1024x1024 输入 | ~8 秒 | 包含图片编码 |
| 文生图 | 512x512, 20 steps | ~4-5 秒 | 热启动后稳定 |
| 文生图 | 1024x1024, 20 steps | ~15 秒 | 大尺寸更精细但更慢 |
| 图片编辑 | 512x512, 20 steps | ~4-5 秒 | 与生图速度一致 |
| 模型加载 | 首次启动 | ~30 秒 | 含权重加载和 GPU 初始化 |
| 显存占用 | `bfloat16` | 8.7 GB | 64 GB 显存余量充足 |

## 指标对比

| 指标 | A100 (`CUDA`) | 昇腾 NPU (`SDPA`) | MetaX C500 (`MACA`) |
| --- | --- | --- | --- |
| 文生图 512x512 | ~3.5 秒 | ~5.5 秒 | ~4-5 秒 |
| 文本对话 | ~1 秒 | ~3-5 秒 | ~1-2 秒 |
| 显存占用 | 8.6 GB | 8.6 GB | 8.7 GB |
| 总显存 | 80 GB | 64 GB | 64 GB |
| Attention 实现 | `FlashAttention2` 原生 | `SDPA` Fallback | `FlashAttention2` (`MACA`) |
| 是否需要 `Patch` | 否 | 是（`SDPA` `Patch`） | 否 |
| 设备接口 | `cuda:0` | `npu:0` | `cuda:0` |
| GPU 工具 | `nvidia-smi` | `npu-smi` | `mx-smi` |
