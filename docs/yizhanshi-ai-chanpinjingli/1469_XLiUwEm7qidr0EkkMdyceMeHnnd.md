# 自然语言处理NLP项目

> 来源：[https://v11enp9ok1h.feishu.cn/docx/N1u4duEAloo0IrxuXACcVRgcnHe](https://v11enp9ok1h.feishu.cn/docx/N1u4duEAloo0IrxuXACcVRgcnHe)

## 推荐 PC 端打开

#### 手机端浏览体验没PC端体验效果好

### 点击蓝字立即跳转：    [🧭课程学习 ](https://v11enp9ok1h.feishu.cn/wiki/M1MSwcFSwiYcR0kmnt0c4M3tngd#VVFPdC1Z2oOGG5x96Jic30psnje)        知[识库预览 ](https://v11enp9ok1h.feishu.cn/wiki/KiIvwdFOciiqqNkwKzTcmn88ndL#Uc4XdGXLdoCWMvxxAzZcweM5nhf)       [技术指导](https://v11enp9ok1h.feishu.cn/wiki/KiIvwdFOciiqqNkwKzTcmn88ndL#Uc4XdGXLdoCWMvxxAzZcweM5nhf)

# **特点**

1.  🎯目标：基于pytorch、transformers做中文领域的nlp开箱即用的训练框架，提供全套的训练、微调模型（包括大模型、文本转向量、文本生成、多模态等模型）的解决方案；

1.  💽数据：

+   从开源社区，整理了海量的训练数据，帮助用户可以快速上手；

+   同时也开放训练数据模版，可以快速处理垂直领域数据；

+   结合多线程、内存映射等更高效的数据处理方式，即使需要处理百GB规模的数据，也是轻而易举；

1.  💻流程：每一个项目有完整的模型训练步骤，如：数据清洗、数据处理、模型构建、模型训练、模型部署、模型图解；

1.  🔥模型：当前已经支持gpt2、clip、gpt-neox、dolly、llama、chatglm-6b、VisionEncoderDecoderModel等多模态大模型；

1.  🚀多卡串联 ：当前，多数的大模型的尺寸已经远远大于单个消费级显卡的显存，需要将多个显卡串联，才能训练大模型、才能部署大模型。因此对部分模型结构进行修改，实现了训练时、推理时 的多卡串联功能。

1.  ⚙️模型工具：添加了大模型的词表裁切和词表扩充

# NLP项目**模型训练**