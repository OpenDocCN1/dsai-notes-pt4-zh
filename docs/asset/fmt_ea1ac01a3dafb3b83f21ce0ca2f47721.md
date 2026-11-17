
[  ](https://link.zhihu.com/?target=https%3A//loli.xing.moe/ChatGPT_as_customer_service/)

---

## 5 Document_QA

[  ](https://link.zhihu.com/?target=https%3A//github.com/fierceX/Document_QA)

根据传入的文本文件，回答你的问题。

读取文件，并进行分割

+ 对于每段文本，使用`text-embedding-ada-002`生成特征向量
+ 将向量和文本对应关系存入本地`pkl`文件
+ 对于用户输入，生成向量
+ 使用向量数据库进行最近邻搜索，返回最相似的文本列表
+ 使用`gpt3.5`的`chatAPI`，设计`prompt`，使其基于最相似的文本列表进行回答

就是先把大量文本中提取相关内容，再进行回答，最终可以达到类似突破`token`限制的效果

后续可以考虑将`openai`的文本向量改成自定义的向量生成工具

---

## 6 ChatWeb

> `ChatWeb`可以爬取任意网页并提取正文，生成概要，然后根据正文内容回答你的问题。 目前是个原理展示的`Demo`，还没有细分逻辑。 基于`gpt3.5`的`chatAPI`和`embeddingAPI`，配合向量数据库。

[  ](https://link.zhihu.com/?target=https%3A//github.com/SkywalkerDarren/chatWeb)

基本类似于现有的`chatPDF`，自动化客服AI等项目的原理。

+ 爬取网页
+ 提取正文
+ 对于每一段落，使用`gpt3.5`的`embeddingAPI`生成向量
+ 每一段落的向量和全文向量做计算，生成概要
+ 将向量和文本对应关系存入向量数据库
+ 对于用户输入，生成向量
+ 使用向量数据库进行最近邻搜索，返回最相似的文本列表
+ 使用`gpt3.5`的`chatAPI`，设计`prompt`，使其基于最相似的文本列表进行回答

就是先把大量文本中提取相关内容，再进行回答，最终可以达到类似突破`token`限制的效果

## 7 chatpdf-minimal-demo

[  ](https://link.zhihu.com/?target=https%3A//github.com/postor/chatpdf-minimal-demo)

![](img/bd95afc87937147a087c66b5dff45b14.png)

实现原理 | process flow

+ 文章切片到段落 | split articles into pieces
+ 通过 `OpenAI` 的 `embedding` 接口将每个段落转换为 `embedding` | convert each piece into `embedding` with `OpenAI`
+ 将提问的问题转换为 `embedding` | convert user question into `embedding`
+ 把问题的 `embedding` 比较所有段落 `embedding` 得到近似程度并排序 | compare question `embedding` with all the embeddings of pieces and sort the result
+ 把和提问(语义)最接近的一个或几个段落作为上下文，通过 `OpenAI` 的对话接口得到最终的答案 | use the nearest (meaning) piece(s) as context and ask `ChatGPT` for the final answer

几个核心代码片段：

获取`embedding`：

```
import openai

def get_embedding(text: str, model: str = EMBEDDING_MODEL) -> list[float]:
    result = openai.Embedding.create(
      model=model,
      input=text
    )
    return result["data"][0]["embedding"]
```

用户问题排序：

```
def order_document_sections_by_query_similarity(query: str, embeddings) -> list[(float, (str, str))]:
    #pprint.pprint("embeddings")
    #pprint.pprint(embeddings)
    """
    Find the query embedding for the supplied query, and compare it against all of the pre-calculated document embeddings
    to find the most relevant sections. 
    
    Return the list of document sections, sorted by relevance in descending order.
    """
    query_embedding = get_embedding(query)
    
    document_similarities = sorted([
        (vector_similarity(query_embedding, doc_embedding), doc_index) for doc_index, doc_embedding in enumerate(embeddings)
    ], reverse=True, key=lambda x: x[0])
    
    return document_similarities
```

当问题被问到时，获取最近的信息作为上下文并询问 `ChatGPT`:

```
def ask(question:str,embeddings,sources):
    ordered_candidates = order_document_sections_by_query_similarity(question,embeddings)
    ctx = ""
    for candi in ordered_candidates:
        next = ctx + " " + sources[candi[1]]
        if len(next) > CONTEXT_TOKEN_LIMIT:
            break
        ctx = next
    if len(ctx) == 0:
      return ""    
    
    prompt = "".join([
        u"Answer the question based on the following context:\n\n"
        u"context:"+ ctx + u"\n\n"
        u"Q:"+question+u"\n\n"
        u"A:"
                        ])

    completion = openai.ChatCompletion.create(model="gpt-3.5-turbo", messages=[{"role": "user", "content":prompt}])
    return [prompt, completion.choices[0].message.content]
```

一个网页版图例：

![](img/7ee34ea0ec684dafc20235368c9141b3.png)

[  ](https://link.zhihu.com/?target=https%3A//postor.medium.com/how-to-code-a-project-like-chatpdf-e40441cb4168)

---

## 8 llama_index建立个人知识库的GPT问答

[  ](https://zhuanlan.zhihu.com/p/615293313)

[  ](https://zhuanlan.zhihu.com/p/617229222)

[  ](https://zhuanlan.zhihu.com/p/624453714)

`llama_index`库是一个项目，它提供了一个中心接口，可以将您的语言模型（`LLM`）与外部数据连接起来 [2] 。它提供了一组数据结构，可以为各种`LLM`任务索引大量数据，并消除了关于提示大小限制和数据摄入的问题 [1] 。它还支持与`LangChain`和`LlamaHub`等其他工具或库的集成

`langchain`的向量搜索

尽管 `llama_index` 是基于 `langchain` 开发的，但向量搜索这件事本来也被`OpenAI`官方吸收了，向量数据库也成熟了那么多年。 `langchain` 有什么理由不整合呢？毕竟 `langchain` 是拿了高额的投资的，自然不会放弃扩张自己版图的机会。

所以， `langchain` 也有自己的 `index` 模块和 `vectorstore` 对象。其设计和`retrieval-plugin`的思路比较像 ———— 接入第三方的向量数据库。而且这俩模块专注于完成向量搜索，你输入一个问句，它只会将相关的参考文档返回给你。并不急于做后续的`GPT`请求。这样你就可以在发`GPT`请求之前添加许多自己的逻辑了。

---

## 9 `ChatGLM-6B` + `langchain` 实现的基于本地知识的 `ChatGLM` 应用

[  ](https://link.zhihu.com/?target=https%3A//github.com/imClumsyPanda/langchain-ChatGLM)

受 `GanymedeNil` 的项目 `document.ai` 和 `AlexZhangji` 创建的 `ChatGLM-6B` `Pull Request` 启发，建立了全部基于开源模型实现的本地知识问答应用。

✅ 本项目中 `Embedding` 默认选用的是 `GanymedeNil/text2vec-large-chinese` ，`LLM` 默认选用的是 `ChatGLM-6B` 。依托上述模型，本项目可实现全部使用 开源 模型 离线私有部署 。

⛓️ 本项目实现原理如下图所示，过程包括加载文件 -> 读取文本 -> 文本分割 -> 文本向量化 -> 问句向量化 -> 在文本向量中匹配出与问句向量最相似的 top k 个 -> 匹配出的文本作为上下文和问题一起添加到 `prompt` 中 -> 提交给 `LLM` 生成回答。

![](img/7b28f79cd9870341fd4a83b6da24dacf.png)

## 10 基于`ChatGLM-6b`+`langchain`实现本地化知识库检索与智能答案生成

[  ](https://link.zhihu.com/?target=https%3A//github.com/yanqiangmiffy/Chinese-LangChain)

![](img/641af9ea5ea41d5e7b5330c246a76f4b.png)

![](img/924e983dca32787a52735326c64204d5.png)

已经有很多更新：

+ 2023/04/22 支持模型多机多卡推理
+ 2023/04/20 支持模型问答与检索问答模式切换
+ 2023/04/20 感谢`HF`官方提供免费算力，添加`HuggingFace` `Spaces`在线体验`DEMO`
+ 2023/04/19 发布45万`Wikipedia`的文本预处理语料以及`FAISS`索引向量
+ 2023/04/19 引入`ChuanhuChatGPT`皮肤
+ 2023/04/19 增加`web search`功能，需要确保网络畅通！(感谢 @wanghao07456 ,提供的idea)
+ 2023/04/18 `webui`增加知识库选择功能
+ 2023/04/18 修复推理预测超时5s报错问题
+ 2023/04/17 支持多种文档上传与内容解析：`pdf`、`docx`，`ppt`等
+ 2023/04/17 支持知识增量更新

## 11 快速小程序搭一个问答机器人

[  ](https://zhuanlan.zhihu.com/p/620596591)

目前的预训练方式主要如下几种：

+ 基于`OpenAI`的官方`LLM`模型，进行 `fine-tune`[1] （费用高，耗时长）
+ 基于开源的 `Alpaca.cpp`[2] 本地模型（目前可在本地消费级显卡跑起来，对自己硬件有信心也可以试试）
+ 通过向量数据库上下文关联（轻量级，费用可控，速度快，包括昨天`OPENAI`官方昨天刚放出来的示例插件 `chatgpt-retrieval-plugin`[3] ，也采用的这种方式）

低代码实现的AI问答机器人效果如下：

![](img/a5690088d4d47d71f6ba480d44d77076.png)

在开始搭建垂直知识库的问答机器人前，你需要做以下准备：

+ 🛰️小程序账号：如果您还没有🛰️小程序账号，可以在🛰️公众平台注册（如果没有小程序，也可以发布为移动端`H5`应用）
+ 开通腾讯云微搭低代码：微搭低代码是腾讯云官方推出的一款低代码开发工具，可以直接访问 腾讯云微搭官网[4] 免费开通注册
+ `OpenAI`账号：`OpenAI`账号注册也是免费的，不过`OpenAI`有地域限制，网上方法很多在此不赘述。注册成功后，可以登录`OpenAI`的 个人中心[5] 来获取 `API` `KEY`
+ 一个支持向量匹配的数据库（本文以开源的 `PostgreSQL` 为例，你也可以使用 `Redis` ，或者`NPM`的 `HNSWlib` 包）

关于向量数据库，目前可选择的方式有好几种，可以使用`PostgreSQL`安装`vector`向量扩展，也可以使用`Redis`的 `Vector Similarity Search`[6] ，还可以直接云函数使用`HNSWLib`库，甚至自行diy一个简单的基于文件系统的余弦相似度向量数据库，文末的 `github/lowcode.ai` 也有简单示例代码，仅做参考交流不建议在生产环境使用。

本教程适用人群和应用类型：

+ 适用人群：有前后端基础的开发者（有一定技术背景的非开发者也可以体验）
+ 应用类型：小程序 或 `H5` 应用（基于微搭一码多端特性，可以发布为`Web`应用，点击原文链接可体验作者基于微搭搭建的 文档`GPT`机器人[7] ）

![](img/81ed43c2e8534c8f4663dd04f139ca64.png)

## 12 三种基于`ChatGPT`搭建问答系统的方法对比

[  ](https://zhuanlan.zhihu.com/p/627655485)

### 12.1 几种常规做法

基于 `LLM` 搭建问答系统的解决方案有以下几种：

+ `Fine-Tuning`，使用下游特定领域的知识对基础模型进行微调，改变神经网络中参数的权重。业界已经不少 `chatgpt` 的平替方案都支持微调
+ 弊端：没有解决事实性问答可靠性的问题
+ 基于 `Prompt Engineering` ，比如 `Few-Shot`方式
+ 类似于短期记忆，容量有限但是清晰，优势。正确性和精度高
+ 劣势。一次可以处理的文本量有限制，如果知识库较大，无论从可行性还是效率而言都是不合适的
+ 与普通搜索结合，使用基础模型对搜索结果加工
+ 问答可控性更高一些
+ 无论是数据规模、查询效率、更新方式都可以满足常见知识库应用场景的需要
+ 技术栈成熟，探索风险低

大概技术图：

![](img/2473305d75fa97dc0afe9dbe27973254.png)

### 12.2 检索匹配的方式
