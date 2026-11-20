
# 01🈷️13日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Q7aodDnMjojDmCx09R1cXnGznlm](https://v11enp9ok1h.feishu.cn/docx/Q7aodDnMjojDmCx09R1cXnGznlm)

## 🔔 AI情报助手午报「01月13日」

### 1️⃣ 🚫 复旦首发“AI禁令”：禁止在论文关键环节使用AI工具

+ 复旦大学发布《规定》，国内首个针对AI工具在本科毕业论文中使用的规范化管理文件。
+ 明确列出了禁止使用AI工具的关键环节，包括研究方案设计、数据分析等。
+ 学生必须在承诺书中披露AI工具的使用情况，并保留相关材料以备检查。
+ 《规定》鼓励学生在有限范围内使用AI工具，如文献检索和图表制作。
+ 其他高校也在探索AI工具的使用边界，如北京师范大学和中国传媒大学出台相关规定。

关键词：AI禁令，本科毕业论文，规范管理，学术诚信，AI工具使用

> 🔗 文中引用的链接

+ 复旦大学关于在本科毕业论文（设计）中使用AI工具的规定（试行）

### 2️⃣ 🔮 大模型在生产制造场景的3个实际落地方案

+ 大模型技术在生产制造领域的应用前景广阔，特别是在智能排产、工艺优化、产品质检三个环节。
+ 智能排产通过集成信息技术和优化算法，实现生产计划与调度的智能化，优化资源配置，提高生产效率。
+ 在生产工艺优化方面，大模型帮助将工业经验数字化、智能化，响应多变的生产需求，提升效率和良品率。
+ 产品质检环节中，大模型结合多模态数据进行深层分析，提高检测准确性，降低成本，实现自监督学习解决数据问题。
+ 大模型在制造业的应用不仅限于生产制造环节，还涉及研发、管理、售后等多个环节，展现出广泛的应用潜力。

关键词：智能制造，智能排产，工艺优化，产品质检，大模型应用

> 🔗 文中引用的链接

+ [AINLP👸号](https://mp.weixin.qq.com/s/KA5KkExrUYUMNrRrffUs6g)

### 3️⃣ 🧠 5万字长文全面解读GUI Agent的前世今生

+ 本文深入探讨了由大型语言模型（`LLM`）驱动的图形用户界面（`GUI`）智能体（`Agent`）的最新发展，揭示了其核心技术和未来机遇。
+ 文章详细介绍了`LLM`驱动的`GUI Agent`在网络导航、移动应用和桌面自动化等领域的应用潜力。
+ 综述识别了该领域的研究空白，并提出了未来发展的路线图，旨在为研究者和从业者提供指导。
+ 讨论了`LLM`在自然语言理解和任务执行方面展现出的能力，以及其在`GUI`自动化中的革命性作用。
+ 分析了`LLM`驱动的`GUI Agent`面临的挑战、局限性和未来研究方向，为该领域的进步提供了详尽指南。

关键词：`LLM`，`GUI Agent`，自动化，人工智能，自然语言处理

> 🔗 文中引用的链接

+ [原始综述论文地址](https://arxiv.org/abs/2411.18279)

### 4️⃣ 🧬 【多模态&LLM】`LLaVA`系列算法架构演进：`LLaVA`（1.0->1.5->Next(1.6)->NeXT(Video)）

+ `LLaVA`系列算法结合预训练`LLM`和视觉模型，使用`Vicuna`作为语言解码器，`CLIP`作为视觉编码器。
+ `LLaVA 1.5`在网络结构和数据集规模上进行了改进，引入高分辨率输入和`LLaVA-1.5-HD`模型。
+ `LLaVA-NeXT（1.6）`提高了输入图像分辨率，增强视觉推理和`OCR`能力，支持多种宽高比和更高分辨率。
+ `LLaVA-NeXT(Video)`通过`AnyRes`技术实现零样本视频表示能力，支持从图像到视频的演变。
+ `LLaVA-Next-Video-DPO`使用直接偏好优化(`DPO`)提升模型响应与`AI`反馈对齐，性能显著提升。

关键词：多模态，`LLM`，视觉模型，视频理解，直接偏好优化

> 🔗 文中引用的链接

+ Visual Instruction Tuning
+ [Improved Baselines with Visual Instruction Tuning](https://arxiv.org/pdf/2310.03744)
+ [`LLaVA-NeXT`: Improved reasoning, OCR, and world knowledge](https://llava-vl.github.io/blog/2024-01-30-llava-next)
+ [A Strong Zero-shot Video Understanding Model](https://llava-vl.github.io/blog/2024-04-30-llava-next-video)

### 5️⃣ 🧠 手写`transformer decoder`（`Causal LM`）

+ 本文介绍了如何手写一个Causal Language Model（`Causal LM`）的`transformer decoder`。
+ 强调了`Causal LM decoder`与原版`transformer decoder`的区别，主要在于省略了`encoder`部分和`cross-attention`机制。
+ 文章详细解释了`transformer decoder`的各个组成部分，包括`self-attention`和`FFN`（Feed Forward Network）。
+ 提供了具体的Python代码实现，包括`SimpleDecoder`类的定义和`attention`、`ffn` block的构建。
+ 文章还讨论了不同模型间的差异，如`LLaMA`对`GPT`的改进和`RMSNorm`与`LayerNorm`的对比。

关键词：`Causal LM`，`Transformer Decoder`，`Self-Attention`，`FFN`，模型对比

> 🔗 文中引用的链接

+ [本文首发](https://bruceyuan.com/hands-on-code/hands-on-causallm-decoder.html)

沟通时请保持“公开可接受”
