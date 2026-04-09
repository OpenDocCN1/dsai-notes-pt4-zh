# 1. 生成式人工智能简介

> 来源：[https://waytoagi.feishu.cn/docx/Q2TFdI8quoFIhYxXifvck2JJnJc](https://waytoagi.feishu.cn/docx/Q2TFdI8quoFIhYxXifvck2JJnJc)

+   视频学习地址：https://youtu.be/G2fqAlgmoPo

+   中文版本推荐[宝玉XP翻译版本](https://www.bilibili.com/video/BV15a4y1u77v/) 和 超正经学术君（以下是两个版本）：

<https://player.bilibili.com/player.html?bvid=15a4y1u77v>

<https://player.bilibili.com/player.html?bvid=1ep4y1P7oP>

这个名为"生成式人工智能简介"的视频是由Gwendolyn Stripling博士主讲的。她介绍了生成式人工智能的概念，这是一种可以产生各种类型内容的人工智能，包括文本、图像、音频和合成数据。

她解释了人工智能和机器学习的区别，机器学习是人工智能的一个子领域，它通过输入数据训练模型，使计算机能够在没有明确编程的情况下学习。

机器学习模型可以是监督的，也可以是无监督的，监督模型使用标记的数据从过去的例子中学习并预测未来的值，而无监督模型则专注于发现原始数据中的模式。

深度学习是机器学习的一个子集，它使用人工神经网络处理比传统机器学习模型更复杂的模式。这些神经网络可以使用标记和未标记的数据，从而允许半监督学习。

+   生成式人工智能是适用于这个学科的人工智能的一个子集。它试图学习数据和标签之间的关系，以生成新的内容。生成式人工智能输出自然语言、图像或音频，而非生成式人工智能输出数字或类别。

+   生成式人工智能模型使用统计模型预测预期的响应，并根据它们接受训练的数据的基础结构生成新的内容。他们可以根据接收到的输入生成文本、图像、音频和决策的新颖组合。

+   生成式人工智能的力量来自于使用了Transformers，这种技术在2018年彻底改变了自然语言处理 (Transformer模型的核心思想是“自注意力机制”/Self-Attention Mechanism，也被称为“自我注意力”或“注意力”。这种机制允许模型在处理一个词或短语时，同时考虑到与它相关的其他词或短语的信息。这种方式使得模型能够更好地理解语言的上下文，从而更准确地进行翻译或生成文本)。

+   然而，Transformers也可能产生幻觉，这些是模型生成的无意义或语法错误的单词或短语。

人工智能中的基础模型是大型的预训练模型，可以适应或微调用于各种下游任务，如情感分析、图像字幕和对象识别。这些模型有可能革新医疗、金融和客户服务等行业，检测欺诈并提供个性化的客户支持。

视频还讨论了在软件开发中使用代码生成和生成式人工智能的用途。它提到了如Bard和生成式人工智能工作室等工具，可以帮助进行调试、代码转换和应用构建。

最后，视频描述了PALM API的会话式人工智能引擎，用户可以使用自然语言与应用进行交互。该API可以用来创建数字助手、自定义搜索引擎、知识库和培训应用。开发者可以将PALM API与Maker Suite集成，通过图形用户界面访问API。该套件包括模型训练、部署和监控的工具。

参考资料：

**All Readings:**** Introductio****n** to Generative AI (G-GENAI-I)

Here are the assembled readings on generative AI:

+   Ask a Techspert: What is generative AI? https://blog.google/inside-google/googlers/ask-a-techspert/what-is-generative-ai/

+   Build new generative AI powered search & conversational experiences with Gen App Builder:

+   https://cloud.google.com/blog/products/ai-machine-learning/create-generative-apps-in-

+   minutes-with-gen-app-builder

+   What is generative AI? https://www.mckinsey.com/featured-insights/mckinsey-explainers/what-is-generative-ai

+   Google Research, 2022 & beyond: Generative models: https://ai.googleblog.com/2023/01/google-research-2022-beyond-language.html#Gener ativeModels

+   Building the most open and innovative AI ecosystem: https://cloud.google.com/blog/products/ai-machine-learning/building-an-open-generativ e-ai-partner-ecosystem

+   Generative AI is here. Who Should Control It? https://www.nytimes.com/2022/10/21/podcasts/hard-fork-generative-artificial-intelligen ce.html

+   Stanford U & Google’s Generative Agents Produce Believable Proxies of Human Behaviors:

+   https://syncedreview.com/2023/04/12/stanford-u-googles-generative-agents-produce-b

+   elievable-proxies-of-human-behaviours/

+   Generative AI: Perspectives from Stanford HAI: https://hai.stanford.edu/sites/default/files/2023-03/Generative_AI_HAI_Perspectives.pd f

+   Generative AI at Work: https://www.nber.org/system/files/working_papers/w31161/w31161.pdf

+   The future of generative AI is niche, not generalized: https://www.technologyreview.com/2023/04/27/1072102/the-future-of-generative-ai-is- niche-not-generalized/

+   Here are the assembled readings on large language models:

+   NLP's ImageNet moment has arrived: https://thegradient.pub/nlp-imagenet/

+   Google Cloud supercharges NLP with large language models:

+   https://cloud.google.com/blog/products/ai-machine-learning/google-cloud-supercharge

+   s-nlp-with-large-language-models

+   LaMDA: our breakthrough conversation technology: https://blog.google/technology/ai/lamda/

+   Language Models are Few-Shot Learners: https://proceedings.neurips.cc/paper/2020/file/1457c0d6bfcb4967418bfb8ac142f64a- Paper.pdf

+   PaLM-E: An embodied multimodal language model: https://ai.googleblog.com/2023/03/palm-e-embodied-multimodal-language.html

+   Pathways Language Model (PaLM): Scaling to 540 Billion Parameters for Breakthrough Performance:

+   https://ai.googleblog.com/2022/04/pathways-language-model-palm-scaling-to.html

+   PaLM API & MakerSuite: an approachable way to start prototyping and building generative AI applications: https://developers.googleblog.com/2023/03/announcing-palm-api-and-makersuite.html

+   The Power of Scale for Parameter-Efficient Prompt Tuning: https://proceedings.neurips.cc/paper/2020/file/1457c0d6bfcb4967418bfb8ac142f64a- Paper.pdf

+   Google Research, 2022 & beyond: Language models: https://ai.googleblog.com/2023/01/google-research-2022-beyond-language.html#Langu ageModels

+   Accelerating text generation with Confident Adaptive Language Modeling (CALM): https://ai.googleblog.com/2022/12/accelerating-text-generation-with.html

+   Solving a machine-learning mystery: https://news.mit.edu/2023/large-language-models-in-context-learning-0207

+   Additional Resources:

+   Attention is All You Need: https://research.google/pubs/pub46201/

+   Transformer: A Novel Neural Network Architecture for Language Understanding:

+   https://ai.googleblog.com/2017/08/transformer-novel-neural-network.html

+   Transformer on Wikipedia: https://en.wikipedia.org/wiki/Transformer_(machine_learning_model)#:~:text=Transfor mers%20were%20introduced%20in%202017,allowing%20training%20on%20larger%20da tasets.

+   What is Temperature in NLP? https://lukesalamone.github.io/posts/what-is-temperature/

+   Bard now helps you code: https://blog.google/technology/ai/code-with-bard/

+   Model Garden: https://cloud.google.com/model-garden

+   Auto-generated Summaries in Google Docs:

+   https://ai.googleblog.com/2022/03/auto-generated-summaries-in-google-docs.html