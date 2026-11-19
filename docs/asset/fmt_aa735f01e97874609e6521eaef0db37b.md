
# 开源 · ChatGLM2-6B：性能大幅提升，8-32k上下文，推理提速42%

> 来源：[https://ai.feishu.cn/docx/Hu41dKE2roJOBQxevlacKpQ7nzh](https://ai.feishu.cn/docx/Hu41dKE2roJOBQxevlacKpQ7nzh)

![](img/5946461419534855bb35fbd37061d945.png)

自3月14日发布以来，ChatGLM-6B 深受广大开发者喜爱，截至 6 月24日，来自 Huggingface 上的下载量已经超过 300w。

为了更进一步促进大模型开源社区的发展，我们再次升级 ChatGLM-6B，发布 ChatGLM2-6B 。

在主要评估LLM模型中文能力的 C-Eval 榜单中，截至6月25日 ChatGLM2 模型以 71.1 的分数位居 Rank 0 ，ChatGLM2-6B 模型以 51.7 的分数位居 Rank 6，是榜单上排名最高的开源模型。

![](img/5acf59a145f13e82da132558f2ab2c5a.png)

CEval榜单，ChatGLM2暂时位居Rank 0，ChatGLM2-6B位居 Rank 6

## 性能升级

ChatGLM2-6B 是开源中英双语对话模型 ChatGLM-6B 的第二代版本，在保留了初代模型对话流畅、部署门槛较低等众多优秀特性的基础之上，ChatGLM2-6B 引入了如下新特性：

### 更强大的性能

基于 ChatGLM 初代模型的开发经验，我们全面升级了 ChatGLM2-6B 的基座模型。ChatGLM2-6B 使用了 GLM 的混合目标函数，经过了 1.4T 中英标识符的预训练与人类偏好对齐训练，评测结果显示，相比于初代模型，ChatGLM2-6B 在 MMLU（+23%）、CEval（+33%）、GSM8K（+571%） 、BBH（+60%）等数据集上的性能取得了大幅度的提升，在同尺寸开源模型中具有较强的竞争力。

### 更长的上下文

基于 `FlashAttention` 技术，我们将基座模型的上下文长度（Context Length）由 ChatGLM-6B 的 2K 扩展到了 32K，并在对话阶段使用 8K 的上下文长度训练，允许更多轮次的对话。但当前版本的 ChatGLM2-6B 对单轮超长文档的理解能力有限，我们会在后续迭代升级中着重进行优化。

### 更高效的推理

基于 `Multi-Query Attention` 技术，ChatGLM2-6B 有更高效的推理速度和更低的显存占用：在官方的模型实现下，推理速度相比初代提升了 42%，INT4 量化下，6G 显存支持的对话长度由 1K 提升到了 8K。

### 更开放的协议

ChatGLM2-6B 权重对学术研究完全开放，在获得官方的书面许可后，亦允许商业使用。如果您发现我们的开源模型对您的业务有用，我们欢迎您对下一代模型 ChatGLM3 研发的捐赠。

## 评测结果

我们选取了部分中英文典型数据集进行了评测，以下为 ChatGLM2-6B 模型在 MMLU (英文)、C-Eval（中文）、GSM8K（数学）、BBH（英文） 上的测评结果。

![](img/3954b52360b98b8c0e8d60015f2c6bbc.png)

![](img/ce49dc1970c3cdd11cf64e3075e7aff8.png)

![](img/69d5b5ed7434dd6c9f57f657d377a3fe.png)

## 推理性能

ChatGLM2-6B 使用了 `Multi-Query Attention`，提高了生成速度。生成 2000 个字符的平均速度对比如下

![](img/ddaa4963e9eccb31ddf77ad9cc6bf7bd.png)

`Multi-Query Attention` 同时也降低了生成过程中 `KV Cache` 的显存占用，此外，ChatGLM2-6B 采用 `Causal Mask` 进行对话训练，连续对话时可复用前面轮次的 `KV Cache`，进一步优化了显存占用。因此，使用 6GB 显存的显卡进行 INT4 量化的推理时，初代的 ChatGLM-6B 模型最多能够生成 1119 个字符就会提示显存耗尽，而 ChatGLM2-6B 能够生成至少 8192 个字符。

![](img/226656110c91f92b9f6fc6f46f96e0c3.png)

我们也测试了量化对模型性能的影响。结果表明，量化对模型性能的影响在可接受范围内。

![](img/9b3b43cc65cb4b6f91da78f7b48de440.png)

## 示例对比

相比于初代模型，ChatGLM2-6B 多个维度的能力都取得了提升，以下是一些对比示例。更多 ChatGLM2-6B 的可能，等待你来探索发现！

### 数理逻辑

![](img/74b6567886f5145effb09608b09c1bc2.png)

### 知识推理

![](img/ff0d32fbe980b211d87eccd98b7a58d8.png)

### 长文档理解

![](img/9ecf4792dc345b17de7d81177f6fb0e4.png)

+ ChatGLM2-6B的安装请参考官方：`https://github.com/THUDM/ChatGLM2-6Bend -`
