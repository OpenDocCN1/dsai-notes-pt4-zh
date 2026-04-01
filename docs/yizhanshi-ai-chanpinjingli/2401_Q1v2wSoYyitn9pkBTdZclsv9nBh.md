# 02🈷️01日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/ZbEhdSok3ooU0oxfKK4clRodnUe](https://v11enp9ok1h.feishu.cn/docx/ZbEhdSok3ooU0oxfKK4clRodnUe)

> 🔔AI情报助手午报「02月01日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🧬

> 医疗具身智能发展到哪了？看这一篇综述就够了！

>

+   医疗具身智能（EmAI）结合AI算法、机器人技术和生物医学，推动医疗服务模式变革。

+   EmAI通过多模态感知、行动控制、决策计划和记忆能力，赋予AI类似人类的感知与执行能力。

+   论文《A Survey of Embodied AI in Healthcare: Techniques, Applications, and Opportunities》梳理了医疗领域关键技术与应用前景。

+   具身智能在临床干预、护理陪伴、设施运转和研究开发四大医疗健康领域展现出实践与突破。

+   论文提出从Level 1到Level 5的具身智能级别，描述技术进化路径和未来发展潜力。

> 关键词：医疗具身智能，AI算法，机器人技术，生物医学，跨学科研究

>

> 🔗 文中引用的链接

+   [论文链接](https://arxiv.org/abs/2501.07468)

> 2️⃣

> 🚨

> DeepSeek正遭遇全球围剿，是谁急了？

>

+   全球公司和政府机构限制DeepSeek工具访问，担心数据泄露和隐私问题。

+   网络安全公司报告显示，大量客户请求阻止DeepSeek访问。

+   DeepSeek的AI聊天机器人因隐私条款和中国服务器数据存储问题引发担忧。

+   网络安全研究人员发现DeepSeek的公开可访问数据库，暴露内部数据。

+   各国政府审查DeepSeek，意大利紧急封锁，英国要求透明使用个人数据。

> 关键词：DeepSeek，数据泄露，隐私保护，网络安全，人工智能

>

> 🔗 文中引用的链接

+   [彭博社报道](https://www.bloomberg.com/news/articles/2025-01-30/deepseek-s-ai-restricted-by-hundreds-of-companies-within-days?srnd=phx-technology)

> 3️⃣

> 🧬

> LLM Agent和 Agentic RAG 的最佳综述

>

+   代理式检索增强生成（Agentic RAG）通过嵌入自主代理在RAG管道中，实现了人工智能领域的重大进步。🌏

+   本文综述了Agentic RAG的基础原理、系统分类、与传统RAG及ADW的比较分析、跨行业应用、挑战和未来方向。🏄

+   Agentic RAG系统展现了在多领域知识检索、实时文档工作流和可扩展AI系统方面的优势。📚

+   面临的挑战包括多代理系统的协调复杂性、伦理AI、可扩展性和人机协作等。🤖

+   未来方向涉及增强代理协调、领域特定应用、伦理AI治理框架和高效的图推理。🌈

> 关键词：Agentic RAG，人工智能，多领域应用，挑战，未来方向

>

> 🔗 文中引用的链接

+   [论文链接](https://arxiv.org/abs/2501.09136)

+   [Github仓库](https://github.com/asinghcsu/AgenticRAG-Survey)

> 4️⃣

> 🧠

> 从"速答"到"深思":揭秘rStar-Math如何追平o1

>

+   rStar-Math是微软研究院为小型语言模型设计的数学能力提升方法，通过特殊训练使其与大型模型竞争。

+   该方法结合Policy SLM和过程奖励模型，利用蒙特卡洛树搜索(MCTS)进行深度思考，提升解题能力。

+   创新的代码增强Chain-of-Thought(CoT)数据合成和过程偏好模型(PPM)训练，提高训练数据质量和评估推理步骤。

+   实验显示rStar-Math显著提升了小型模型的数学能力，甚至在某些测试中超越了OpenAI的o1模型。

+   rStar-Math展现了自我进化能力，通过迭代提升模型性能，为AI在复杂推理任务中的应用提供了新思路。

> 关键词：AI，数学推理，小型语言模型，蒙特卡洛树搜索，自我进化

>

> 🔗 文中引用的链接

+   [rStar-Math介绍](https://mp.weixin.qq.com/s/u6-7Vtgxs7Tn1vwX4DeqYg)

> 5️⃣

> 🧬

> 解读DeepSeek革命级的创新！再读MLA，还有多少细节是你不知道的

>

+   本文深入分析了MLA（Multi-Head Attention）的设计思想和优化方法，指出原生MLA存在的计算量和KV cache问题，并介绍了CC、A_CC和A_CC_ME等优化方案，这些方案通过信息转移和压缩显著降低了KV cache存储需求并优化了计算量。

+   文章探讨了MLA在张量并行（TP）中的应用，指出其可通过定制化方式实现TP，但需采用不同于传统解码器模型的切分策略。

+   作者分享了自己对MLA的理解过程，从最初的困惑到通过社区讨论和自己的分析重新认识MLA，发现原生MLA确实存在问题，并且开源社区已有了各种优化方法。

+   文章第一部分抽象出MLA的设计方案，基于此再来谈计算细节和各种优化，全文目录包括MLA的基本思想、运作细节以及MLA在TP中的应用。

+   最后，文章对MLA的原生实现进行了分析，指出其没有做任何的信息转移，并且KV cache显存反而增加了，因此称之为CD（CacheDecompressed）。

> 关键词：MLA，优化方案，张量并行，信息转移，KV cache

>

> 🔗 文中引用的链接

+   [章明星老师关于MLA优化的思路](http://#)

+   [知乎上一篇对MLA计算量的分析](https://zhuanlan.zhihu.com/p/714761319)

+   [blog](https://zhuanlan.zhihu.com/p/700214123)

+   [代码](https://github.com/madsys-dev/deepseekv2-profile/tree/main)

> 6️⃣

> 🧬

> [灵敏度高达94.9%！牛津团队AI多](https://mp.weixin.qq.com/s/vHkphm4gPOW04fLASkT5EQ)模态ctDNA检测方法，进行癌症早期筛查

>

+   牛津大学研究团队开发了基于AI的多模态ctDNA检测方法，灵敏度高达94.9%，特异性为88.8%，为癌症早期筛查带来新希望。

+   该方法利用全基因组TET辅助吡啶硼烷测序（TAPS），整合基因组和甲基化数据，提高检测准确性。

+   研究团队通过多模态数据分析流程，整合拷贝数变异、体细胞突变和甲基化信号，增强ctDNA检测的灵敏度。

+   在模拟数据和临床样本中，该方法展现出在低ctDNA含量下仍具有高区分能力的潜力。

+   该研究为癌症早期检测和术后监测提供了新的技术路径，同时指出了成本和资源限制等实际应用挑战。

> 关键词：AI，癌症筛查，ctDNA，多模态数据，TAPS

>

> 🔗 文中引用的链接

+   [论文链接](https://www.nature.com/articles/s41467-024-55428-y)