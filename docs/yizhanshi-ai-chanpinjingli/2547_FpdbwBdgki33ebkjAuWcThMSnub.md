
# 6🈷️29日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/KoPOdys6eoif1zx0VUTcYT9nnFb](https://v11enp9ok1h.feishu.cn/docx/KoPOdys6eoif1zx0VUTcYT9nnFb)

> 🔔AI情报助手早报「06月29日」

> ✨✨✨✨✨✨✨✨

## 1️⃣ 阿里发布信息检索Agent WebDancer

> 🎯 阿里发布信息检索Agent——`WebDancer`，能自主上网查资料，在`GAIA`和`WebWalkerQA`基准测试中超越部分基线与开源框架，模型和数据已开源。

+ `WebDancer` 具备多步推理等能力，采用四阶段训练范式，包括浏览数据构建、轨迹采样、监督微调及强化学习。
+ 在`GAIA`和`WebWalkerQA`数据集上测试表现良好，在更具挑战性数据集上也展现出鲁棒性。
+ 强化学习优化了决策和泛化能力，奖励机制由格式和答案奖励组成。
+ 研究证明主动信息搜索和代理决策对任务很重要，利用推理专用模型构建代理方法更有效。

**关键词**：`信息检索Agent`，`WebDancer`，开源，模型训练

> 🔗 文中引用的链接

+ [https://x.com/_akhaliq/status/1937997314737553873](https://x.com/_akhaliq/status/1937997314737553873)
+ `https://arxiv.org/abs/2505.22648`
+ [WebDancer](https://github.com/Alibaba-NLP/WebAgent/tree/main/WebDancer)
+ [WebDancer-32B](https://huggingface.co/Alibaba-NLP/WebDancer-32B)

## 2️⃣ 阿里推出多模态模型 Qwen VLo

> 🤖 昨夜，阿里版GPT-4o登场，一句话精准...

+ 阿里推出多模态统一理解与生成模型`Qwen VLo`，用户可通过`Qwen Chat`访问预览版。
+ `Qwen VLo`有三大亮点：理解和生成更准确、支持开放指令编辑修改图片、多语言指令支持。
+ 该模型能像人类画师精细创作，支持多种图像编辑及生成任务，还能对生成内容再分析。
+ 它引入渐进式生成机制，提升生成效率，适用于长段落文字生成。
+ `Qwen VLo`尚处预览阶段，未来或提供更多元交流手段。

**关键词**：`Qwen VLo`，多模态模型，渐进式生成

> 🔗 文中引用的链接

+ [体验地址](https://chat.qwenlm.ai/)

## 3️⃣ 无监督RL的粗略分析

> 🎯 [无监督RL的粗略分析](https://mp.weixin.qq.com/s/bMjxG0wcLSduFGZcnvIKUQ)

+ 近期无监督`RL`在社区掀起热潮，本文分析其相关文章出发点及直观分析。
+ 无监督`RL`在`LLM`领域多指无需`gold-answer`的`RL`，当前主要集中在不使用真实数据`answer`训练。
+ 利用`consistency`降低输出不确定性，但可能导致输出坍缩、模型性能下降。
+ 不同无监督`RL`方法源于选择不同一致性metric，如答案一致性、熵等。
+ 未来或可混合`gold-answer-reward`与`internal-feedback`，提升数据及探索效率。

**关键词**：无监督`RL`，`LLM`，`consistency`，一致性metric，数据效率

> 🔗 文中引用的链接

+ [`UDA`](https://arxiv.org/abs/1904.12848)
+ [`TTRL`](https://arxiv.org/pdf/2504.16084)
+ [Can Large Reasoning Models Self-Train?](https://arxiv.org/abs/2505.21444)
+ [`SEED-GRPO`](https://arxiv.org/abs/2505.12346)
+ [`ent-rl`](https://arxiv.org/abs/2505.22660)
+ [`Intuitor`](https://arxiv.org/abs/2505.19590)
+ [`self-certainty`](https://arxiv.org/abs/2502.18581)
+ [No Free Lunch: Rethinking Internal Feedback for LLM Reasoning](https://www.alphaxiv.org/abs/2506.17219v1)
+ `Intrinsically-motivated-RL`

## 4️⃣ 向量数据库原理到应用全解析

> 🌈 一文读懂向量数据库，原理到应用全解析！

+ 向量数据库类似超市货架分区陈列，将高维向量有序归档到特定索引结构。
+ 借助层次图`HNSW`等算法兼顾“快”与“准”，能快速返回相似查询结果。
+ 大模型时代前已支撑人脸识别等场景，如今大模型需求促进其发展。
+ 与传统键值库不同，需管理嵌入生成等，解决混合稠密-稀疏检索挑战。
+ 确保海量数据和持续写入下仍保持高吞吐与一致性。

**关键词**：向量数据库，近似最近邻算法，大模型，混合稠密-稀疏检索

> 🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/MZzv85dhsDjM-A29NrEfHA)

## 5️⃣ Gemma 3n 端侧全能模型深度解析

> 🤖 深度解析 `Gemma 3n` 端侧全能模型...

+ Google推出多模态开源模型`Gemma 3n`，支持多输入并输出文本，为“mobile-first”端侧轻量级模型，在低显存设备可本地推理运行。
+ 其优秀性能源于`MatFormer`架构、`PLE`、`KV Cache`共享策略及音频视觉编码器等创新技术。
+ `Gemma 3n`在多榜单表现出色，语音识别与翻译能力优异。
+ 模型在多平台开源，提供多种部署工具及详细文档教程，并发起社区挑战。
+ 小编试验反馈不同平台测试效果及建议。

**关键词**：`Gemma 3n`，端侧模型，开源，多模态，架构创新

> 🔗 文中引用的链接

+ [1] Reddit: [https://www.reddit.com/r/LocalLLaMA/comments/1ll429p/gemma_3n_has_been_released_on_huggingface](https://www.reddit.com/r/LocalLLaMA/comments/1ll429p/gemma_3n_has_been_released_on_huggingface)
+ [2] MatFormer(Matryoshka Transformer): `https://arxiv.org/abs/2310.07707`
+ [3] 最大上下文长度可达 32K token: [https://ai.google.dev/gemma/docs/gemma-3n](https://ai.google.dev/gemma/docs/gemma-3n)
+ [4] Hugging Face: [https://huggingface.co/google/gemma-3n-E4B-it-litert-preview](https://huggingface.co/google/gemma-3n-E4B-it-litert-preview)
+ [5] Kaggle: [https://www.kaggle.com/models/google/gemma-3n](https://www.kaggle.com/models/google/gemma-3n)
+ [6] Google AI Studio: [https://deepmind.google/models/gemma/gemma-3n/](https://deepmind.google/models/gemma/gemma-3n/)
+ [7] Ollama: `https://ollama.com/library/gemma3n`
+ [8] MLX: [https://huggingface.co/collections/mlx-community/gemma-3n-685d6c8d02d7486c7e77a7dc](https://huggingface.co/collections/mlx-community/gemma-3n-685d6c8d02d7486c7e77a7dc)
+ [9] LMStudio: [https://lmstudio.ai/models/google/gemma-3n-e4b](https://lmstudio.ai/models/google/gemma-3n-e4b)
+ [10] transformers.js: [`transformers.js`](https://huggingface.co/onnx-community/gemma-3n-E2B-it-ONNX)
+ [11] Colab 使用文档: [https://colab.research.google.com/github/huggingface/huggingface-gemma-recipes/blob/main/notebooks/fine_tune_gemma3n_on_t4.ipynb](https://colab.research.google.com/github/huggingface/huggingface-gemma-recipes/blob/main/notebooks/fine_tune_gemma3n_on_t4.ipynb)
+ [12] 微调指南: [https://github.com/huggingface/huggingface-gemma-recipes/blob/main/notebooks/fine_tune_gemma3n_on_audio.ipynb](https://github.com/huggingface/huggingface-gemma-recipes/blob/main/notebooks/fine_tune_gemma3n_on_audio.ipynb)
+ [13] NVIDIA Jetson 和 RTX 硬件部署指南: [https://developer.nvidia.com/blog/run-google-deepminds-gemma-3n-on-nvidia-jetson-and-rtx/](https://developer.nvidia.com/blog/run-google-deepminds-gemma-3n-on-nvidia-jetson-and-rtx/)
+ [14] Impact Challenge: [https://www.kaggle.com/competitions/google-gemma-3n-hackathon/overview](https://www.kaggle.com/competitions/google-gemma-3n-hackathon/overview)

## 6️⃣ 0627资讯：FLUX.1 Kontext等

> 🤖 0627资讯：`FLUX.1 Kontext`图像生成与编辑模型，不同版本各有特色。

+ Black Forest Labs发布`FLUX.1 Kontext`图像生成与编辑模型，不同版本各有特色。
+ 快手发布多模态大模型`Kwai Keye-VL`，强化视频理解与推理能力。
+ 小米发布AI眼镜，进军AI可穿戴赛道。
+ 谷歌推出开源AI终端编程助手`Gemini CLI`，提升开发效率。
+ 华为推出一站式AI编程助手`CodeArts Doer`，覆盖软件开发全流程。

**关键词**：AI模型，智能硬件，编程助手

> 🔗 文中引用的链接

+ [FLUX.1 Kontext](https://bfl.ai/announcements/flux-1-kontext)
+ [Kwai Keye-VL](https://github.com/Kwai-Keye/Keye/tree/main)
+ [Gemini CLI](https://blog.google/technology/developers/introducing-gemini-cli-open-source-ai-agent/)
+ [CodeArts Doer](https://www.huaweicloud.com/product/codeartside/snap)
