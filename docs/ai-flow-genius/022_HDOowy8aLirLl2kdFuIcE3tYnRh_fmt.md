
# 使用AI技术搭建低成本私人知识库

> 来源：[https://jalicvoygc0.feishu.cn/docx/Ib0mdUTKXoKgGNx51v3cs3MMndc](https://jalicvoygc0.feishu.cn/docx/Ib0mdUTKXoKgGNx51v3cs3MMndc)
> 引用：[https://mp.weixin.qq.com/s/Bc6B4z80QD0ZPKwLyI9V_g](https://mp.weixin.qq.com/s/Bc6B4z80QD0ZPKwLyI9V_g)
> 作者：AI应用研究分享

---

大语言模型发展日新月异，创业项目如雨后春笋一般，大部分普通人能想到的微创新项目，都有人在做了，甚至OPENAI等大语言模型公司做的更好。模型一升级，支持一堆新特性，很多创业项目又白做了。鉴于此，我判断私人知识库项目是大公司不愿意做或者没办法做到的。因为OPENAI等大语言模型是在公有云部署的，隐私很难保证。

搭建私人知识库的方案有很多，大部分基于`langchain`方案，网上介绍的文章很多，github上开源实现也很多。我在研究了`quivr`和`embedchain`两个项目后，感觉`embedchain`项目比较好玩，虽然没有web界面，但是可以进行实验。

关于`embedchain`的项目地址：https://github.com/embedchain/embedchain

项目文档介绍：https://docs.embedchain.ai/quickstart

`embedchain`的`quickstart`项目介绍文档有些粗糙，但是代码实现很强大，通过阅读源码发现，`embedchain`支持很多LLM模型，包括gtp4all、huggingface上的各类模型、claude、openai等。尤其是可以直接使用huggingface模型库上的各类模型，极大的方便进行实验研究。同时，这个项目支持对接的知识源形态比较多，比如网页、`pdf`、`doc`等文档都可以向量化。

回到私人知识库的主题，搭建私人知识库的原理和步骤：

## 知识入库

1. 使用文本切片（`chunk`）工具将文档切成固定长度的文本，这一步可以有很多优化方案，比如两个切片有重叠部分，这样相关切片会被搜索出来，或者将文档转换成问答对，将问答对转换成向量。
2. 使用向量模型将切片后的文本转换成向量数据。
3. 将向量数据和文本存储到向量数据库。

## 查询知识

4. 使用自然语言编写查询句子，使用LLM优化问题，使问题描述更加丰富，使用向量模型将问题向量化。
5. 使用问题向量去向量数据库查询相关的文本切片条目。
6. 将搜索到的文本切片条目对应的文本提取出来，作为`prompt`背景信息，连同原问题发给LLM模型。
7. LLM使用自然语言返回结果。

在上面的方案中，向量数据库负责个性化知识存储和搜索知识，LLM负责将知识组织成易于理解的自然语言。如下图所示，流程图参考文章：https://mp.weixin.qq.com/s/VM2wFWkhp45VxJ6q3yFbvA

![](img/2e6d1a9e198b5e6bce67a31a4cc862d3.png)

![](img/331ab219e84978c71dd408a9ea1d7567.png)

现在主流的知识库搭建，都是基于OPENAI的向量模型和LLM进行处理，都是收费的，而且还有API调用还有频率限制。那么对于私人用户来说，成本有点高，而且知识信息还会发送到OPENAI大模型，存在信息泄漏，国内网络还不通。笔者致力于降低私人知识库搭建成本，同时做到完全本地化，避免信息泄漏。那么将知识向量化的模型和进行自然语言理解和组织的模型，最好都适用开源模型，并且本地部署。

当前研究进展是，向量模型采用Huggingface开源模型`intfloat/e5-base-v2`，该模型支持中文，同时在huggingface向量模型排行榜排名比openai的embed模型还靠前，模型本身也比较小，占用资源少。但是用于自然语言理解的LLM仍然依赖于OPENAI模型，因为其他小模型测试效果并不好，稍微大一些的模型依赖于很贵的显卡，macair是跑不起来的。所以暂时做到这个程度，还是使用openai模型进行自然语言理解。后续再研究LLM本地化部署，或者先暂时使用国内LLM的接口。

以下实验代码在colab上已经跑通，在本地jupyter notebook验证时，被openai限频了，暂时未跑通。

```python
!pip3 install --upgrade embedchain
import os
from embedchain import CustomApp
from embedchain.config import CustomAppConfig
from embedchain.models import Providers, EmbeddingFunctions
from chromadb.utils import embedding_functions

os.environ["OPENAI_API_KEY"] = "替换成自己的key"
config = CustomAppConfig(embedding_fn=EmbeddingFunctions.HUGGING_FACE, embedding_fn_model='intfloat/e5-base-v2', provider=Providers.OPENAI)
app = CustomApp(config)

app.add("web_page", "https://mp.weixin.qq.com/s/GKCwpbY39dNdxhUYfmN6Ew")
app.query("思维链提示工程主要有哪几种")
```

提示：本地安装`embedchain`依赖包时，`chroma-hnswlib`包编译报错，原因是我的mac air是`x86_64`架构cpu, 编译优化参数`-march=native`不支持，其实还是安装文件没考虑到我这种情况，从https://pypi.org/project/chroma-hnswlib/0.7.2/下载源码，并修改以后重新编译才通过。具体过程比较痛苦，后续单独写文章说明。

---

## 继续浏览全站内容

[🎪首页](https://jalicvoygc0.feishu.cn/wiki/DdEvwxYsSidd36kAIIVcnROwnmb)

[🤔学习AI工作流](https://jalicvoygc0.feishu.cn/wiki/W15IwochvimsqvkkuCPcZuBxnqg)

[🤖查找AI工具](https://jalicvoygc0.feishu.cn/wiki/Bu8GwqYu8i5LBRkyVKocUvX5n97)

[📊阅读AI文章](https://jalicvoygc0.feishu.cn/wiki/SIywwqrVji2lZkkqFFpcEHZ8nNc)

扫码加入AI Flow Genius社群，探讨使用AI来提升生产力的方法，成为超级个体

超过200需手动拉人 **备注：加群**

![](img/d49a75e976051e12cf5ce847bcea7904.png)
