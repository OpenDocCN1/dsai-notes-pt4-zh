
# 入门：生成式预训练

> 来源：[https://agispace.feishu.cn/docx/Aw5rdzesvozfOLxnMRyc1T5WnOc](https://agispace.feishu.cn/docx/Aw5rdzesvozfOLxnMRyc1T5WnOc)

生成式预训练 Transformer (GPT) 是一种大型语言模型 (LLM)，也是生成人工智能的重要框架。它们是用于自然语言处理任务的人工神经网络。GPT 基于 Transformer 架构，在未标记文本的大型数据集上进行预训练，并且能够生成新颖的类人内容。截至 2023 年，大多数法学硕士都具有这些特征，有时被广泛称为 GPT。

第一个 GPT 由 OpenAI 于 2018 年推出。OpenAI 发布了非常有影响力的 GPT 基础模型，并按顺序编号，组成其“GPT-n”系列。由规模（可训练参数的数量）和训练的增加，每一个都比以前的能力更强。其中最新的 GPT-4 于 2023 年 3 月发布。此类模型是其更具任务特定性的 GPT 系统的基础，包括针对指令遵循进行微调的模型，这反过来又为 ChatGPT 聊天机器人提供动力服务。

术语“GPT”也用于其他人开发的此类模型的名称和描述中。例如，其他 GPT 基础模型包括 EleutherAI 创建的一系列模型，以及 Cerebras 在 2023 年创建的七个模型。此外，不同行业的公司在各自领域开发了针对特定任务的 GPT，例如 Salesforce 的“EinsteinGPT”（针对 CRM）和 Bloomberg 的“BloombergGPT”（针对金融）。

## 引用

https://en.wikipedia.org/wiki/Generative_pre-trained_transformer

## 相关文章推荐

1. [从入门到精通：生成式预训练 Transformer 的工作流程和实践应用_生成式预训练 transformer 模型-CSDN 博客](https://blog.csdn.net/universsky2015/article/details/131497257)
2. [生成式预训练 Transformer：从入门到精通 - 百度智能云](https://cloud.baidu.com/article/2686945) [2024-01-04]
3. GPT——生成式预训练 Transformer - 知乎专栏
4. [【LLM 系列之 GPT】GPT（Generative Pre-trained Transformer）生成式预训练模型 - 知乎](https://zhuanlan.zhihu.com/p/642112269)
5. GPT：生成式预训练 Transformer 模型 - 百度智能云 [2024-01-07]
6. [深入探索生成式预训练 Transformer（GPT）：技术、应用与未来 - 知乎](https://zhuanlan.zhihu.com/p/667552761) [2023-11-18]
7. 生成式预训练 Transformer 模型 - MBA 智库百科
8. 生成式预训练 Transformer - 知乎 - 知乎专栏
9. [GPT — PaddleEdu documentation - Read the Docs](https://paddlepedia.readthedocs.io/en/latest/tutorials/pretrain_model/GPT.html)
10. [Generative Pre-trained Transformer（GPT）模型技术初探 - 博客园](https://www.cnblogs.com/LittleHann/p/17303550.html) [2023-04-14]
11. [预训练语言模型之 GPT-1，GPT-2 和 GPT-3 - 知乎专栏](https://zhuanlan.zhihu.com/p/350017443)
12. [GPT 生成式预训练 Transformer 架构应用实战 - 光剑 - 博客园](https://www.cnblogs.com/the-art-of-ai/p/17486925.html) [2023-06-17]
13. [【LLM 系列之 GPT】GPT（Generative Pre-trained Transformer ...](https://cloud.tencent.com/developer/article/2317898) [2023-08-25]
14. [从 gpt-1 到 gpt-4，GPT 系列模型详解 - 知乎 - 知乎专栏](https://zhuanlan.zhihu.com/p/627901828)
15. 自然语言处理中的 Transformer 和 BERT - 知乎专栏
16. 搞懂 GPT，只需看这些! - 知乎 - 知乎专栏
17. [从零开始入门——Transformer 模型简介 - 腾讯云开发者社区 - 腾讯云](https://cloud.tencent.com/developer/article/2301955)
18. [Gpt-1/Gpt-2/Gpt-3/Gpt-3.5 语言模型详细介绍 - 知乎 - 知乎专栏](https://zhuanlan.zhihu.com/p/620494604)
19. [GPT 模型总结【模型结构及计算过程_详细说明】 原创 - CSDN 博客](https://blog.csdn.net/weixin_45577864/article/details/119651372) [2021-08-12]
20. [PDF 深度学习基础 —— 构建基于 Transformer 的自然语言处理应用](https://www.nvidia.cn/content/dam/en-zz/zh_cn/Solutions/gtc2020/dli-training/courses-intro/%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%9F%BA%E7%A1%80_%E6%9E%84%E5%BB%BA%E5%9F%BA%E4%BA%8ETransformer%E7%9A%84%E8%87%AA%E7%84%B6%E8%AF%AD%E8%A8%80%E5%A4%84%E7%90%86%E5%BA%94%E7%94%A8.pdf)
21. [【Transformer 模型结构原理超级详细解析】 - CSDN 博客](https://blog.csdn.net/pxiongw/article/details/122656623)
22. [transformers：最先进的自然语言处理 - 知乎 - 知乎专栏](https://zhuanlan.zhihu.com/p/374868051)
23. 自然语言处理 6：Transformer - 知乎 - 知乎专栏
24. [大语言模型核心技术-Transformer 详解 - 稀土掘金](https://juejin.cn/post/7219249005904166949) [2023-04-07]
25. [自然语言处理 Transformer 模型最详细讲解（图解版）- CSDN 博客](https://blog.csdn.net/m0_47256162/article/details/127339899) [2023-01-09]
26. 自然语言处理 Transformer 模型最详细讲解（图解版）
27. 如何训练自己的 GPT 模型？| Botpress 博客
28. [Transformer（转换器）系列 3 - 语言革命：Transformer 在 NLP 的应用 - 知乎](https://zhuanlan.zhihu.com/p/685023525)
