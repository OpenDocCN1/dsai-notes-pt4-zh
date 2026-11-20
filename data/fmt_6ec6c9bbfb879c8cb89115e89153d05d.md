
# 11🈷️04日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/EsHwd6N3ioaX1KxnOxrcq49VnVh](https://v11enp9ok1h.feishu.cn/docx/EsHwd6N3ioaX1KxnOxrcq49VnVh)

> 🔔AI情报助手早报「11月04日」

> ✨✨✨✨✨✨✨✨

## 1️⃣ 单张4090跑到30fps，范浩强团队让`VLA`实时跑起来

+ 范浩强团队让`VLA`实时跑起来，单张RTX 4090上`Pi0`级模型（30亿参数）最快可达30fps。
+ 团队深入分析模型结构，经一系列优化，将用时从100+ms大幅缩减，优于`openpi`自动优化结果。
+ 优化代码已在GitHub开源，依赖`torch`和`triton`，可“开箱即用”。
+ 团队设计出潜力达480Hz闭环控制的算法框架，及围绕GPU的机器人控制框架。
+ 研究为`VLA`实时控制奠定基础，展望了边缘计算算力增加下的更多可能性。

关键词：`VLA`模型，实时运行，优化，开源，机器人控制

🔗 文中引用的链接

+ [论文链接](https://arxiv.org/abs/2510.26742)
+ [GitHub](https://github.com/Dexmal/realtime-vla)

## 2️⃣ 深度｜陈天桥：AI的终极使命不是取代人类...

+ 陈天桥在AI驱动科学研讨会上提出“发现式智能”理念，认为这是真正的通用人工智能，其价值在于进化人类，而非取代人类。
+ 实现“发现式智能”有规模与结构两条路径，当前规模路径成果显著，但也存在局限。
+ 大脑的时间结构由神经动力学等五种核心能力构成，是结构路径的关键。
+ 天桥脑科学研究院为年轻人准备算力、办公室等资源，助力探索时间智能结构。
+ 年轻人有望借结构路径，定义能“发现”的智能。

关键词：发现式智能，AI，时间结构，规模路径，结构路径

🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/0efY7DMHR2vwKfC_p8JkMw)

## 3️⃣ 速递｜一年内估值翻两番至120亿美元，英伟达...

+ 英伟达计划向AI初创企业`Poolside`投资5 - 10亿美元，使其估值从30亿美元翻两番至120亿美元。
+ `Poolside`主打编码自动化产品，目标是打造通用人工智能，本轮已获超10亿美元承诺投资。
+ 英伟达参与融资凸显其推动AI初创企业生态圈扩张，`Poolside`计划采购英伟达GB300芯片。
+ `Poolside`与CoreWeave合作建造大型数据中心，装机容量达20亿瓦。
+ 英伟达今年已投资59家AI初创公司，CEO黄仁勋称AI初创公司青睐英伟达平台。

关键词：英伟达，`Poolside`，AI初创企业，投资，数据中心

🔗 文中引用的链接

+ [参考资料](https://www.bloomberg.com/news/articles/2025-10-30/nvidia-to-invest-up-to-1-billion-in-ai-startup-poolside?srnd=phx-technology)

## 4️⃣ 最新外国「自研」大模型，都是套壳国产？

+ `Cursor`发布的`Composer`模型和`Cognition`推出的`SWE-1.5`模型，被质疑基于中国AI模型开发，如`Composer`思考时说中文，`SWE-1.5`“越狱”后自曝是智谱`GLM`模型。
+ 业界大佬认为两模型有中国开源模型痕迹，且适合强化学习微调。
+ 英伟达黄仁勋称开源模型强大，阿里`Qwen`占据开源模型大部分市场份额。
+ 国内开源大模型在技术水平和接受度上确立领先地位。
+ 全球AI正进入多极化竞争新格局。

关键词：外国大模型，国产模型，开源模型，AI竞争格局

🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/YAUI6-pKltinFuq4Z5MjHQ)

## 5️⃣ 5分钟上手，无照就能飞：91万「空中F1」...

+ `Jetson`公司推出`Jetson ONE`个人飞行器，被称为“空中F1”，5分钟上手，无需执照，售价91万起，2027年交付。
+ 其性能亮眼，安全配置丰富，适用于农场巡视等场景，但也有网友质疑其适用性和安全性。
+ 类似飞行器还有`Alef Aeronautics`的飞行汽车、日本人力飞行自行车及`Volonaut Airbike`飞行摩托。
+ `Airbike`设计独特，但载重、续航有限且价格高昂。
+ 文章主要介绍各类新奇飞行器，未涉及AI相关算法进展。

关键词：个人飞行器，`Jetson ONE`，飞行汽车，飞行自行车，飞行摩托

🔗 文中引用的链接

+ https://x.com/Rainmaker1973/status/1983771228667249141
+ https://x.com/Smile_1Please/status/1983969471506346220
+ https://x.com/Volonaut/status/1948005178247889364

## 6️⃣ 上海AI Lab发布混合扩散语言模型`SDAR`...

+ 上海AI Lab发布混合扩散语言模型`SDAR`，通过“训练-推理解耦”融合`AR`与扩散模型优势，解决大模型推理速度慢、成本高难题。
+ `SDAR`训练沿用`AR`范式，推理引入低成本适配阶段，实现并行解码，实验证明其性能与`AR`模型持平或超越，且推理加速显著。
+ `SDAR`在科学推理任务表现出色，在`ChemBench`等基准上取得明显性能优势。
+ 团队开源全系列`SDAR`模型、推理引擎及最强开源扩散类推理模型。
+ `SDAR`为大模型领域提供新工具，降低应用门槛，开启探索下一代AI推理范式大门。

关键词：`SDAR`，大模型，推理加速，开源，科学推理

🔗 文中引用的链接

+ [论文链接](https://arxiv.org/abs/2510.06303)
+ [代码地址](https://github.com/JetAstra/SDAR)
+ [推理引擎](https://github.com/InternLM/lmdeploy)
+ [模型地址](https://huggingface.co/collections/JetLM/sdar)
