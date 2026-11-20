
# 基于知识图谱和RAG融合的智能问答实践探索

> 来源：[https://agijuejin.feishu.cn/docx/CldPdLm0Ko38SexE2f6c7fDAnwY](https://agijuejin.feishu.cn/docx/CldPdLm0Ko38SexE2f6c7fDAnwY)

> 作者：王大锤

> 原文： [https://zhuanlan.zhihu.com/p/710428877](https://zhuanlan.zhihu.com/p/710428877)

## 一、背景

[主要是针对](https://www.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E5%9B%BE%E8%B0%B1&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710428877%7D)`RAG`的应用场景，尝试结合 知识图谱 ，在开源技术方案上做的一次实验。

![](img/dfe1a2b5e166a0ae27eaf3f598a31258.png)

## 二、前置环境

### 2.1 `openai key`

从非本地化部署`llm`模型测试，有一个`openai key`似乎必不可少，不管是作为`benchmark`的计量单位，或考虑中英文的输入输出，都相对更加方便，不过自3月新用户注册不再赠送5美金之后，走正常请求的方式就不再友好，所以我这里选用了中转的方式，具体调用逻辑如下：

![](img/6caed295a3f526bb7bf663b0835ceb5c.png)

目[前使用情况](https://www.zhihu.com/search?q=%E6%96%87%E5%BF%83%E5%8D%83%E5%B8%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710428877%7D)来讲，除了扣费[很快，速度](https://www.zhihu.com/search?q=%E6%99%BA%E8%B0%B1AI&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710428877%7D)与直连没什么区别，扣费规则各家都不一样，这里不好做评价。当然，国内有非常优秀的平替产品，如 文心千帆 、通义千问和 智谱AI ，我使用最多的是智谱AI，其次通义千问，因为智谱有封装好的SDK能直接调用，通义千问的`modelscope`接口同样能很快捷的使用`embedding`和`chat model`，这是给我观感最好的，其次才是准确性问题。

### 2.2 `neo4j`

关于图数据库，直 接`docker pull neo4j`后的镜像为官方镜像，但启动后用`langchain`的`neo4j`接口去连会报错为Could not use `APOC` procedures。`APOC`是一个为`Neo4j`提供额外的过程和功能的插件，它扩展了`Cypher` 查询语言 的能力，而官方镜像并没有安装该插件，所以需要在启动后的 极简容器 里手动安装，但我尝试后发现这并不是一个很快能解决的过程，除了权限问题外，还有各种依赖，于是就去 `dockerhub` 找到了如下镜像：

![](img/6c69cb34e4bf7229823258b6fd4bcd2f.png)

该镜像启动后是有`APOC`的，然后还需要修改`neo4j.conf`文件中找到`dbms.security.procedures.unrestricted`配置项，即：

![](img/f9e358428d6a2fc38bc1a9ff4910898c.png)

正常来讲，重启后就可以直连了。并且都是带界面的：

![](img/3effe0984fc5b3339258cef8931bca37.png)

### 2.3 `python`依赖包

```
pip install -U 
langchain
umap-learn
scikit-learn
langchain_community
tiktoken
langchain-openai
langchainhub
chromadb
langchain-anthropic
langchain_experimental
wikipedia
```

## 三、实验过程

[本次实验是](https://www.zhihu.com/search?q=%E6%95%B0%E6%8D%AE%E5%85%83%E7%BB%84&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710428877%7D)根据`Wikipedia`提取到的内容进行抽取 数据元组 ，将其存入图数据库中，以备后续的`RAG`综合调用，整个过程可看成如下图：

![](img/fb37dd817b56fe3a86d5ebbc90964a97.png)

所以下面开始准备数据源。

### 3.1 图数据制作

在大模型之前，知识图谱构建是挺繁琐的一[个过程，](https://www.zhihu.com/search?q=%E4%B8%89%E5%85%83%E7%BB%84&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710428877%7D)虽然可能结果就是一个层层交叉的 三元组 ，但从一段文字需要经历的过程可从如下图表示：

![](img/03beae9ea2d29f03d9e0cfc64e6e37df.png)

在[构建本体](https://www.zhihu.com/search?q=%E9%B2%81%E6%A3%92%E6%80%A7&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710428877%7D)[的时候我们一定要接受](https://www.zhihu.com/search?q=zero-shot&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710428877%7D)[本体是变化](https://www.zhihu.com/search?q=%E5%AF%B9%E6%AF%94%E5%AE%9E%E9%AA%8C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710428877%7D)的，就像数据库本身的表结构也可能会更新，所以设计之初就需要考虑 鲁棒性 和扩展[性，而在大模](https://www.zhihu.com/search?q=%E5%90%91%E9%87%8F%E6%95%B0%E6%8D%AE%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710428877%7D)型时代，不管是 `zero-shot` 还是`few-shot`的大模型，我记得在去年2023年的时候，论文里的 对比实验 就已经超越了如实体抽取等算法，那简单构建一个`KG`不再是困难的一件事，但大模型短期内还无法处理长文本或整个图谱，所以图谱的存储是一个很重要的方向。能预测到它和 向量数据库 一样，会成为未来大模型生态圈里一个非常重要的组件，从上层应用角度，可以自由选择是否启用。

这里，我选用了`langchain`官方文档中提到的`Diffbot`（`Diffbot` | ️ `LangChain`） 感觉准确率不错，而且用起来相对顺手：

![](img/70547e42134044a42ec46a8494d39775.png)

它分 [免费版 ](https://www.zhihu.com/search?q=%E5%85%8D%E8%B4%B9%E7%89%88&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710428877%7D)和商用版，如果选择商用，价格比较贵， 平替的策略也有，比如 `LLMGraphTransformer` ， 其利用`openai`的抽取能力，源码中用了两段`prompt`为 `unstructured_prompt` 和 `system_prompt` ，两者的构建都非常有意思，这里引用出前者的代码：

![](img/ec63b9a6f17da2731916dad385609a2f.png)

抽取完成后，进行入库：

![](img/9635fea01179ca52a4077de11e4c9d65.png)

入库后，可根据需要，在当前单元格内直接对数据库内容进行可视化，我[这里可视化](https://www.zhihu.com/search?q=%E6%99%BA%E8%B0%B1ai&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710428877%7D)了之前周杰伦的`wiki`词条，不过之后都是成龙，因为在我将前者数据丢给 智谱ai 的时候，竟然发现有很多违禁词，emmm，不知道哪些。

![](img/423884601b1ee7b471890486ac93713b.png)

### 3.2 构建文档树

![](img/f4a269f22d907acaddf205658d5fb265.png)

`RAPTOR`（Recursive Abstractive Processi[ng for T](https://www.zhihu.com/search?q=%E6%A3%80%E7%B4%A2%E5%A2%9E%E5%BC%BA%E5%9E%8B%E8%AF%AD%E8%A8%80&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710428877%7D)ree-Organized Retrieval）是一种创建新的 检索增强型语言 模型，它通过嵌入、聚类和摘要文本模块来构建一个从底层到高层具有不同摘要层的树状结构。这种方法允許模型在推理时从这棵树中检索信息，实现跨文本的不同抽象层的整合。`RAPTOR`的相关性创新在于它构建了文本摘要的方法，以不同尺度检索上下文的能力，并在多个任务上展示超越传统检索增强语言模型的性能。它主要做如下七步：

1.  [文本分割](https://www.zhihu.com/search?q=%E6%96%87%E6%9C%AC%E5%88%86%E5%89%B2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710428877%7D)

1.  文本向量表示

1.  [文本聚类](https://www.zhihu.com/search?q=%E6%96%87%E6%9C%AC%E8%81%9A%E7%B1%BB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710428877%7D)

1.  文本摘要

1.  创建树节点

1.  递归分聚类以及摘要

1.  文档检索

以下为具体代码：

![](img/6526cf1e712101086c52c638b5cb9f99.png)

![](img/441a1070f5a2cc430c196ee43173330a.png)

![](img/5dbdc17c962c3fc6eae0fce4f52f2905.png)

![](img/93a739ddfe49f44de1164ba2fa5b7cae.png)

![](img/7ef36d4c92140ca7547ac0f88e2b81d2.png)

![](img/126a557383da0f990a72f652f07dff02.png)

进行调用:

![](img/d4bf0187c392c8e2b75de76221a64142.png)

### 3.3 `RAG chain` + `RAPTOR`

> `query=输出成龙（Jackie chan）的事业成就`

![](img/8b37979ab324809101e93a1d87737d93.png)

> `query=谁和Jackie chan一起工作？Joe Hisaishi和其是同事嘛？`

![](img/07b063c815eadee07a3c5e6836b73803.png)

### 3.4 `RAG chain` + `KG`

输出当前`graph`结构：

![](img/9da9c160a8addd6f11e79061139a0514.png)

这里可以使用自定义查询方式：

![](img/1535fe8796e225046f7a158d6fd513ae.png)

基于此调用：

![](img/be07c6f885cccab1e93aa2c1dbd4bd6b.png)

但坏消息是， `with_structured_output` 是一种`tools call`，不走`openai`中转方式的话，得再手动实现一下，从功能实现和官方文档中感觉实现还是比较简单的，或者直接再加一层`prompt`，让它按格式输出也行。

不过以上是想手动实现，其实整个大接口，`langchain`也对此做了封装，即 `GraphCypherQAChain` ：

![](img/7b3bfe807e32192cdb3b9e9533186650.png)

![](img/6cd5d35e9361e43a5bb4b12f504c93b4.png)

然后结合`RAG`一起，做增强回答:

![](img/7ef9689414c30ffed9cd53bc29456310.png)

最后，如果想将`RAG` + `KG` + `LLM` + `RAPTOR`进行组合，当然是可以，最简单能：

![](img/b4cd116d186908dbaff4619c7818f99c.png)
