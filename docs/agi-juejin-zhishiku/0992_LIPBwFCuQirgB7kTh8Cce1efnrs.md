# 有大模型的地方，都有RAG----AIGC时代多模态RAG综述

> 来源：[https://agijuejin.feishu.cn/docx/NDOJdldFLoNaEjxUzJFc9uK1nmb](https://agijuejin.feishu.cn/docx/NDOJdldFLoNaEjxUzJFc9uK1nmb)

> 作者：[西红柿牛腩](https://www.zhihu.com/people/chenhong007)

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/685869716)hihu.com/p/685869...

[以前关注的](https://www.zhihu.com/search?q=%E9%97%AE%E7%AD%94%E4%BC%9A%E8%AF%9D&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A685869716%7D)[RAG，是通](https://www.zhihu.com/search?q=%E6%99%BA%E8%83%BD%E5%AE%9E%E9%AA%8C%E5%AE%A4&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A685869716%7D)过检索一些文本语义，增强 问答会话 的能力。来自北京大学崔斌教授带领的数据与 智能实验室 （PKU-DAIR）系统调研现有RAG技术，覆盖了近300篇相关论文，发布综述(Retrieval-Augmented Generation for AI-Generated Content: A Survey)。

## [多模态 ](https://www.zhihu.com/search?q=%E5%A4%9A%E6%A8%A1%E6%80%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A685869716%7D)RAG方法

个人感觉，这是最完整梳理总结了RAG在包括文本，图像，代码，音频，视频，3D等多个不同领域，不同模态上的具体应用以及现有的RAG系统评估标准或者工具。

![](img/43e7cc245290f738797b3b5761b98899.png)

2023年是ChatGPT为代表的智能对话元年，而2024年文本生视频元年。图像领域的DALL-E和Stable Diffusion模型、以及文生长视频背后的Sora模型，必将在2024年遍地开花结果。

虽然这些强大的 [生成器 ](https://www.zhihu.com/search?q=%E7%94%9F%E6%88%90%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A685869716%7D)能够带来令人惊艳的效果，但仍存在一系列不容忽视的问题和挑战，如难以更新[维护知识、不易](https://www.zhihu.com/search?q=%E5%A2%9E%E5%BC%BA%E7%94%9F%E6%88%90%E6%8A%80%E6%9C%AF&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A685869716%7D)掌握长尾知识、以及泄露私人训[练数据的风](https://www.zhihu.com/search?q=%E4%BF%A1%E6%81%AF%E6%A3%80%E7%B4%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A685869716%7D)险。为了应对这些问题，检索 增强生成技术 （RAG）应运而生。它运用 信息检索 （Info[rmation](https://www.zhihu.com/search?q=%E6%95%8F%E6%84%9F%E6%95%B0%E6%8D%AE%E7%BC%96%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A685869716%7D) Retrieval, IR），在庞杂的外部数据中精准地找到相关的现有信息。这些外部存储的数据便于修改，能够涵盖更广泛的长尾知识，同时还可以支持 敏感数据编码 。此外，在特定场景下，RAG还能为长文本提供支持，降低模型成本并提高响应速度。

## RAG基础范式

将RAG基础范式分为4类: **基于查询的RAG方法，基于隐空间表达的RAG方法，基**于概率表示的RAG方法，投机RAG方法。

![](img/0a87620d2c98b0b3958ffa0782f72221.png)

## RAG的提升方法

提升方法的目标将现有方法分为5类：对输入的提升、对检索过程的提升、对生成过程的提升、对结果的提升、以及对整个RAG流程的优化。

![](img/d4ceed516794ff7b0815d57760367366.png)

## RAG应用领域--有大模型的地方，都可以使用RAG

![](img/bf40844ed2e427d87794668379ef12e0.png)

【1】 [https:// arxiv.org/](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2402.19473.pdf)pdf/2402.1947 3.pdf

Retrieval-Augmented Generation for AI-Generated Content: A Survey