# GLM-5 技术文档

> 来源：[https://ai.feishu.cn/docx/L7ohdMahZoTyaDxIyDjcwsYyn6k](https://ai.feishu.cn/docx/L7ohdMahZoTyaDxIyDjcwsYyn6k)

> 📖 查看 GLM-5 技[术博客。](https://z.ai/blog/glm-5)

> 📍 前往 [Z.ai API 平台](https://docs.z.ai/guides/llm/glm-5) 使用 GLM-5 [API 服](https://chat.z.ai/)务。

> 👉 点击即可体验 GLM-5。

## 模型简介

GLM-5 正式发布，面向复杂系统工程与长周期 Agent 任务。规模化仍是提升通用人工智能（AGI）智能效率的核心路径。相较于 GLM-4.5，GLM-5 参数规模由 355B（激活 32B）扩展至 744B（激活 40B），预训练数据量从 23T 增长至 28.5T tokens。同时，GLM-5 集成了 DeepSeek Sparse Attention（DSA），在保持长上下文能力的前提下，大幅降低了部署成本。

强[化学习旨在](https://github.com/THUDM/slime)让预训练模型实**现从「能用」到「好用**」的跨越。然而，RL 训练效率低下，在大规模 LLM 上的应用面临挑战。为此，我们开发了 slime——一套创新的异步 RL 基础设施，显著提升了训练吞吐量与效率，支持更细粒度的后训练迭代。依托预训练与后训练的双重突破，GLM-5 在各类学术基准上相较 GLM-4.7 取得了显著进步，在推理、代码与 Agent 任务上跻身全球开源模型顶尖行列，与前沿模型的差距进一步缩小。

![bench](resources/bench.png)

GLM-5 专为复杂系统工程与长周期 Agent 任务而生。在内部评测套件 CC-Bench-V2 上，GLM-5 在前端、后端及长周期任务上均大幅超越 GLM-4.7，与 Claude Opus 4.5 的差距显著缩小。

![realworld_bench](resources/realworld_bench.png)

在衡量长期运营能力的基准 [Vending Bench 2](https://andonlabs.com/evals/vending-bench-2) 上，GLM-5 位居开源模型榜首。Vending Bench 2 要求模型在一年时间跨度内经营一家模拟自动售货机业务，GLM-5 最终账户余额达 4,432 美元，逼近 Claude Opus 4.5，展现出卓越的长期规划与资源管理能力。

![vending_bench](resources/vending_bench.png)

## 下载模型

| **模型** | **下载链接** | **模型规模** | **精度** |
| --- | --- | --- | --- |
| GLM-5 | [🤗 Hugging Face  ](https://huggingface.co/zai-org/GLM-5)[🤖 ModelScope](https://modelscope.cn/models/ZhipuAI/GLM-5) | 744B-A40B | BF16 |
| GLM-5-FP8 | [🤗 Hugging Face  ](https://huggingface.co/zai-org/GLM-5-FP8) [🤖 ModelScope](https://modelscope.cn/models/ZhipuAI/GLM-5-FP8) | 744B-A40B | FP8 |

## 本地部署 GLM-5

### 环境准备

vLLM、SGLang 和 xLLM 均支持 GLM-5 本地部署，以下提供简易部署指引。

#### **vLLM**

使用 Docker：

```
docker pull vllm/vllm-openai:nightly
```

或使用 pip：

```
pip install -U vllm --pre --index-url https://pypi.org/simple --extra-index-url https://wheels.vllm.ai/nightly
```

随后升级 transformers：

```
pip install git+https://github.com/huggingface/transformers.git
```

#### **SGLang**

使用 Docker：

```
docker pull lmsysorg/sglang:glm5-hopper      # 适用于 Hopper GPU
docker pull lmsysorg/sglang:glm5-blackwell   # 适用于 Blackwell GPU
```

### **部署**

#### vLLM

```
vllm serve zai-org/GLM-5-FP8 \
     --tensor-parallel-size 8 \
     --gpu-memory-utilization 0.85 \
     --speculative-config.method mtp \
     --speculative-config.num_speculative_tokens 1 \
     --tool-call-parser glm47 \
     --reasoning-parser glm45 \
     --enable-auto-tool-choice \
     --served-model-name glm-5-fp8
```

更多细节请查看 [vllm recipes](https://github.com/vllm-project/recipes/blob/main/GLM/GLM5.md)。

#### **SGLang**

```
python3 -m sglang.launch_server \
    --model-path zai-org/GLM-5-FP8 \
    --tp-size 8 \
    --tool-call-parser glm47  \
    --reasoning-parser glm45 \
    --speculative-algorithm EAGLE \
    --speculative-num-steps 3 \
    --speculative-eagle-topk 1 \
    --speculative-num-draft-tokens 4 \
    --mem-fraction-static 0.85 \
    --served-model-name glm-5-fp8
```

更多细节请查看 [sglang cookbook](https://cookbook.sglang.io/autoregressive/GLM/GLM-5)。

#### **xLLM 与昇腾 NPU**

请参考[部署指南](https://github.com/zai-org/GLM-5/blob/main/example/ascend.md)。

## 引用

技术报告即将发布。