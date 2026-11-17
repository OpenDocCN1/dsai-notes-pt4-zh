
# Stable Diffusion创意实验室

> 来源：[https://aigcreative.feishu.cn/docx/QkKPdI7z6oYNOBxuoROcGl1dnCd](https://aigcreative.feishu.cn/docx/QkKPdI7z6oYNOBxuoROcGl1dnCd)

## 简介

Stable Diffusion 是一款基于扩散技术的深度学习文本到图像模型，于 2022 年发布，被视为当前 AI 春天的一部分。该模型主要用于根据文本描述生成详细图像，同时也可以应用于其他任务，如图像修复、图像外扩和文本引导的图像到图像转换。Stable Diffusion 的开发涉及来自慕尼黑路德维希-马克西米利安大学的 CompVis 小组和 Runway 的研究人员，以及 Stability AI 的计算捐赠和来自非营利组织的训练数据。

Stable Diffusion 是一种潜在扩散模型，一种深度生成性人工神经网络。其代码和模型权重已开源，并且可以在配备至少 4 GB VRAM 的适度 GPU 的大多数消费者硬件上运行。这与之前仅通过云服务访问的专有文本到图像模型（如 DALL-E 和 Midjourney）不同。

该模型的开发由初创公司 Stability AI 资助和塑造，技术许可由慕尼黑路德维希-马克西米利安大学的 CompVis 小组发布。Stable Diffusion 的研发由 Runway 的 Patrick Esser 和 CompVis 的 Robin Rombach 领导，他们是先前发明 Stable Diffusion 所使用的潜在扩散模型架构的研究人员之一。

Stable Diffusion 使用一种称为潜在扩散模型 (LDM) 的扩散模型，由慕尼黑 LMU 大学的 CompVis 小组开发。扩散模型通过训练目标去除训练图像上连续应用的高斯噪声来进行训练，可以被视为一系列去噪自编码器。Stable Diffusion 包括 3 个部分：变分自编码器 (VAE)、U-Net 和可选的文本编码器。VAE 编码器将图像从像素空间压缩到更小维度的潜在空间，捕获图像的更基本语义含义。

Stable Diffusion 在 LAION-5B 上进行了训练，这是一个从网页抓取的 Common Crawl 数据派生的公开可用数据集，其中包含了 50 亿个图像-文本对。这些图像-文本对基于语言分类，并根据分辨率、预测的含有水印的可能性和预测的“美学”分数（例如主观视觉质量）被过滤成不同的数据集。

Stable Diffusion 在生成图像时存在一些限制和挑战，如分辨率偏离其“预期”的 512x512 分辨率时图像质量的明显下降，以及由于 LAION 数据库中肢体数据质量较差而在生成人体肢体时遇到的挑战。为了解决这些限制，用户可以选择进行额外的训练以微调生成输出，以匹配更具体的用例，这一过程也被称为个性化。

`https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg`

## 知识库动态
