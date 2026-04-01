# 入门：生成式预训练

> 来源：[https://agispace.feishu.cn/docx/Aw5rdzesvozfOLxnMRyc1T5WnOc](https://agispace.feishu.cn/docx/Aw5rdzesvozfOLxnMRyc1T5WnOc)

生成式**预训练** Trans[former](https://en.wikipedia.org/wiki/Large_language_model)[ ( GPT ) 是](https://en.wikipedia.org/wiki/Generative_artificial_intelligence)[一种大型语言模型(](https://en.wikipedia.org/wiki/Natural_language_processing)[LLM) (](https://en.wikipedia.org/wiki/Artificial_neural_network)[1] )([2] )([[3])](https://en.wikipedia.org/wiki/Data_sets) ，也是生成人工智能的重要框架。([4] )([5])它们是用于自然语言处理任务的人工神经网络。([6]) GPT 基于Transformer架构，在未标记文本的大型数据集上进行预训练，并且能够生成新颖的类人内容。([2] )([3])截至 2023 年，大多数法学硕士都具有这些特征([7])，有时被广泛称为 GPT。([8])()()()()()

[第一个 GPT 由OpenAI](https://en.wikipedia.org/wiki/OpenAI)[于 2018 ](https://en.wikipedia.org/wiki/Generative_pre-trained_transformer#Foundational_models)年推出。([9]) OpenAI发布了非常有影响力的GPT基础[模型，并按](https://en.wikipedia.org/wiki/GPT-4)[顺序编号，组成其“GP](https://en.wikipedia.org/wiki/Instruction_tuning)T- n ”系列。[([10])由](https://en.wikipedia.org/wiki/ChatGPT)于[规模（可训](https://en.wikipedia.org/wiki/Chatbot)练参数的数量）和训练的增加，每一个都比以前的能力更强。其中最新的GPT-4于 2023 年 3 月发布。([11])此类模型是其更具任务特定性的 GPT 系统的基础，包括针对指令遵循进行微调的模型，这反过来又为ChatGPT 聊天机器人提供动力服务。([1])

术语“GPT”也用于其他人开发的此类模型的名称和[描述中。例如，其他 ](https://en.wikipedia.org/wiki/EleutherAI)GPT[ 基础模型](https://en.wikipedia.org/wiki/EleutherAI#GPT_models)包括Eleut[herAI创建的一系](https://en.wikipedia.org/wiki/Cerebras)列[模型，([12])以](https://en.wikipedia.org/wiki/Salesforce)[及Ce](https://en.wikipedia.org/wiki/Customer_relationship_management)rebras在 [2023 年创建的](https://en.wikipedia.org/wiki/Bloomberg_L.P.)七个模型。 ([13])此外，不同行业的公司在各自领域开发了针对特定任务的 GPT，例如Salesforce的“EinsteinGPT”（针对CRM）([14])和Bloomberg的“BloombergGPT”（针对金融）。([15])

引用

https://en.wikipedia.org/wiki/Generative_pre-trained_transformer

#### 相关文章推荐

[1](https://blog.csdn.net/universsky2015/article/details/131497257). 从入门到精通：生成式预训练Transformer的工作流程和实践应用_生成式预训练transformer模型-CSDN博客

[2. 生成式预训练Transfo](https://cloud.baidu.com/article/2686945)rmer：从入门到精通 - 百度智能云 [2024-01-04]

3. GPT——生成式预训练Transformer - 知乎专栏

[4](https://zhuanlan.zhihu.com/p/642112269). 【LLM系列之GPT】GPT（Generative Pre-trained Transformer）生成式预训练模型 - 知乎

5. GPT：生成式预训练Transformer模型 - 百度智能云 [2024-01-07]

[6. 深入探索生成式预训练Transformer](https://zhuanlan.zhihu.com/p/667552761)（GPT）：技术、应用与未来 - 知乎 [2023-11-18]

7. 生成式预训练Transfomer模型- MBA智库百科

8. 生成式预训练Transformer - 知乎 - 知乎专栏

[9. GPT — PaddleEdu documenta](https://paddlepedia.readthedocs.io/en/latest/tutorials/pretrain_model/GPT.html)tion - Read the Docs

[1](https://www.cnblogs.com/LittleHann/p/17303550.html)0. Generative Pre-trained Transformer（GPT）模型技术初探 - 博客园 [2023-04-14]

[11. 预训练语言模型之GPT-](https://zhuanlan.zhihu.com/p/350017443)1，GPT-2和GPT-3 - 知乎专栏

[12. GPT生成式预训练Transform](https://www.cnblogs.com/the-art-of-ai/p/17486925.html)er架构应用实战 - 光剑 - 博客园 [2023-06-17]

[1](https://cloud.tencent.com/developer/article/2317898)3. 【LLM系列之GPT】GPT（Generative Pre-trained Transformer ... [2023-08-25]

[14. 从gpt-1到gpt-4，G](https://zhuanlan.zhihu.com/p/627901828)pt系列模型详解 - 知乎 - 知乎专栏

15. 自然语言处理中的Transformer和BERT - 知乎专栏

16. 搞懂gpt，只需看这些! - 知乎 - 知乎专栏

[17. 从零开始入门——Transfor](https://cloud.tencent.com/developer/article/2301955)mer模型简介-腾讯云开发者社区-腾讯云

[18. Gpt-1/Gpt-2/Gpt-3/Gpt-3.5 ](https://zhuanlan.zhihu.com/p/620494604)语言模型详细介绍 - 知乎 - 知乎专栏

[19. GPT模型总结【模型结构及计算过](https://blog.csdn.net/weixin_45577864/article/details/119651372)程_详细说明】 原创 - CSDN博客 [2021-08-12]

[20. PDF 深度学习基础 —— 构建基于 T](https://www.nvidia.cn/content/dam/en-zz/zh_cn/Solutions/gtc2020/dli-training/courses-intro/%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%9F%BA%E7%A1%80_%E6%9E%84%E5%BB%BA%E5%9F%BA%E4%BA%8ETransformer%E7%9A%84%E8%87%AA%E7%84%B6%E8%AF%AD%E8%A8%80%E5%A4%84%E7%90%86%E5%BA%94%E7%94%A8.pdf)ransformer 的自然语言处理应用

[23. 【Transformer 模型](https://blog.csdn.net/pxiongw/article/details/122656623)结构原理超级详细解析】 - CSDN博客

[24. transformers：最先](https://zhuanlan.zhihu.com/p/374868051)进的自然语言处理 - 知乎 - 知乎专栏

25. 自然语言处理6：Transformer - 知乎 - 知乎专栏

[26. 大语言模型核心技术-Tr](https://juejin.cn/post/7219249005904166949)ansformer 详解 - 稀土掘金 [2023-04-07]

[27. 自然语言处理Transformer](https://blog.csdn.net/m0_47256162/article/details/127339899)模型最详细讲解（图解版）-CSDN博客 [2023-01-09]

28. 自然语言处理Transformer模型最详细讲解（图解版）

29. 如何训练自己的GPT 模型？|Botpress 博客

[3](https://zhuanlan.zhihu.com/p/685023525)0. Transformer（转换器）系列 3 - 语言革命：Transformer在NLP的应用 - 知乎