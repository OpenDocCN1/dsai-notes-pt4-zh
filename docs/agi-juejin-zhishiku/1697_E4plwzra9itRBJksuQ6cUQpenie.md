# Kimi+Langchain+FastGPT：文档转LLM微调数据集 / QA问答对生成、Kimi 128KAPI免费接入！

> 来源：[https://agijuejin.feishu.cn/docx/BB3qd2b8WoBUdIxVeaNch958n5g](https://agijuejin.feishu.cn/docx/BB3qd2b8WoBUdIxVeaNch958n5g)

> 作者：Aitrainee | AI进修生

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/A4Dxx3uT1EsL1NjZJG1EiQ)qq.com/s/A4Dxx3uT...

Hi，这里是Aitrainee，欢迎阅读本期新文章。

![](img/945bfad3e76ac5667dfd87daa6f3414b.png)

今天我将介绍：如何使用 **Kimi API **将文档转换为LLM指令监督微调数据集（Alpaca 格式）以及 如何部署FastGPT并接入Kimi API ：

![](img/ce59fac0e95f61fb2884ad04e76d9105.png)

![](img/e05e82542ca3f243f5ea5ae0baf4805b.png)

**我会使用两种方式来完**成QA问答对生成这个需求，第一种是使用 Langchain 这个框架编码实现，第二种是将 Kimi API接入FastGPT 中，再利用他的“文档问答”功能实现。

实际上，我曾使用Coze很长一段时间，并**且在这个平台上我经常**使用的基础模型就是 Kimi-128k ，我使用它构建知识库、生成长文本内容、解读我的本地文档... ...

我喜欢Kimi，除了它 **聊天客户端 **的各种强大的功能以外，它的API能进行更强大的超长文本、 多文档的深度理解任务 ，且收费标准非常低廉。

最近一段时间我在使用FastGPT这个项目，这个项目需要我们自己借助One-API去接入其他第三方模型，我想要接入Kimi， 就像在Coze平台上使用它一样~

![](img/46cee3afb6cc02d2f1220fef2f54ca98.png)

# 一、使用Kimi+Langchain生成QA问答对

## **1、我们首先导入依赖包**

主要包括一些结构化的处理库，还有langchain的依赖库。

记得先下载Langchain~

+

```
pip install langchain
```

+

+

+

+

+

+

+

+

+

+

```
import osimport jsonfrom typing import Listfrom tqdm import tqdmfrom langchain_community.chat_models.moonshot import MoonshotChatfrom langchain_core.prompts import ChatPromptTemplatefrom langchain_core.pydantic_v1 import BaseModel, Fieldfrom langchain_core.output_parsers import JsonOutputParserfrom langchain_community.document_loaders import UnstructuredFileLoaderfrom langchain_text_splitters import RecursiveCharacterTextSplitter
```

## 2、然后去官网获取Kimi API

新用户注册送15元免费额度（文末我还会介绍免费获取Kimi API的方式，仅供个人测试）：

![](img/397596c47fa73d714d265d453b2c0d28.png)

▲ https://platform.moonshot.cn/console/api-keys

以moonshot-v1-128k为例，每1,000,000 tokens价格为60元，对于一段通常的中文文本，1 个 Token 大约相当于 1.5-2 个汉字。 《甄嬛传》小说第一部20 万字 ，所以一本小说消耗约6块钱，而moonshot-v1-8k相对于128k会便宜5倍。

假设你已经获取了API Key，那么现在我们在代码中设置它：

+

+

```
# 设置 Moonshot API Keyos.environ[&quot;MOONSHOT_API_KEY&quot;] = &quot;eyJhbGciOixxx&quot;
```

![](img/eecde571d0c45df7e8ad4a96894a3ac0.png)

▲ Kimi兼容OpenAI接口

#

## **3、输入给大模型的是一个个分块**

Kimi会针对分块来建立QA问答对。所以我们要先对传入的文档进行分块，这里假定输入的文档格式为txt，当然借助Kimi或者Langchain你可以编码以支持更多格式。

我们将使用Langchain的 RecursiveCharacterTextSplitter 函数对加载文档进行分块：

+

+

+

+

+

+

+

+

+

```
# 文档处理函数def split_document(filepath):    loader = UnstructuredFileLoader(filepath)    text_spliter = RecursiveCharacterTextSplitter(        chunk_size=2048,        chunk_overlap=128    )    documents = loader.load_and_split(text_spliter)    return documents
```

> **我们都知道**Ki**mi的一大特点是拥有 **大海捞针 的 超长文本提取信息能力 ，用来做文档转QA非常合适。

并且假设这个数据集是对小模型微调的数据集，那么生成它的微调数据的LLM应该是性能比它更好的。

![](img/5108de83b66cdadfe2daf8d1758a31d6.png)

▲ Kimi 文件API支 持全格式

## **4、编写结构化输出提示词**

我们需要使用系统提示词明确他是在做文本转问答对这件事情，所以我们需要编写针对于Kimi的系统提示词： QA_PAIRS_SYSTEM_PROMPT

其中“<Context></Context> ”会出现在后面的用户提示词中，同时下面这段提示词也是Fastgpt的官方示例。

+

+

+

+

+

+

+

```
QA_PAIRS_SYSTEM_PROMPT = &quot;&quot;&quot;  &lt;Context&gt;&lt;/Context&gt; 标记中是一段文本，学习和分析它，并整理学习成果：- 提出问题并给出每个问题的答案。- 答案需详细完整，尽可能保留原文描述。- 答案可以包含普通文字、链接、代码、表格、公示、媒体链接等 Markdown 元素。- 最多提出 30 个问题。&quot;&quot;&quot;
```

除此以外，如果你的原始文档中包含大量的链接或者公示内容，你可以参考使用我的这个有针对性性的结构化提示词。

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

Rules1. 对文档内容进行分析，并提取出主要信息。2. 针对每个分块的文档内容，生成相关问题，问题和答案中必须保留并正确提取文中的链接、表格、图片及其他Markdown元素。4. 确保生成的答案详细完整，并准确反映源内容的描述。5. 在QA对的答案中，使用Markdown格式表示图片和表格的链接。  - 例如：![表格描述](表格链接)或![图片描述](图片链接)。6. 如果某段内容包含多种类型的信息（如文字、表格、图片等），确保在QA对的答案中体现每种信息。
- 问题：文中引用的图片是什么？答案：文中引用了一个相关图片，链接如下：[![图片描述](图片链接)]。

```
Role: 文档分块嵌入模型提示词生成器
Profile- author: Aitrainee- version: 1.0- language: 中文- description: 生成用于RAG索引的QA问答对，确保分块后的文档内容能够生成包含完整Markdown元素的QA问答对。
Skills1. 能够分析文档内容并生成相关问题与答案。2. 保留并提取文档中的链接、表格、图片及其他Markdown元素。3. 生成覆盖文档内容关键要点的详细QA问答对。4. 确保生成的QA对适合后续处理与引用。
Workflows1. 分析文档内容，识别出需要保留的关键元素（如链接、表格、图片等）。2. 针对识别出的每个元素和上下文，生成问题并构建详细的答案。3. 对生成的QA对进行审核，确保所有Markdown元素均被正确处理并保留。4. 将最终的QA对嵌入到模型中，确保在后续的问答环节中能够准确调用。
Init&lt;Context&gt;&lt;/Context&gt; 标记中是一段文档内容，要求模型学习和分析这段内容，并整理出有效的QA问答对。为确保信息完整，请遵循以下要求：
- 针对文档内容提出尽可能多且相关的问题，每个问题的答案需要完整且详细。- 尽可能保留原文中的描述，并且注意保留任何涉及的链接、表格、图片及其他相关Markdown元素。- 如文档中包含图片、表格或其他媒体内容，请将其转换为适当的Markdown格式，并确保将链接或嵌入代码包括在内。- 对每个段落，最多提出30个问题，确保覆盖文档内容的关键要点。- 确保QA对中每个答案都准确反映源内容，并包括任何必要的上下文，以便后续处理和引用。
例子：- 问题：此段落描述了什么内容？答案：段落描述了...[具体描述]，并包含表格如下：[表格链接或Markdown格式]。
```

然后我们准备 **用户提示词 **（QA_PAIRS_HUMAN_PROMPT），这里主要告诉模型，你希望明确输出的数据格式，比如我们选择Alpaca 格式（省略了system和history）：

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

```
QA_PAIRS_HUMAN_PROMPT = &quot;&quot;&quot;  请按以下格式整理学习成果:[  {{    &quot;instruction&quot;: &quot;人类指令/问题（必填）&quot;,    &quot;input&quot;: &quot;人类输入（选填）&quot;,    &quot;output&quot;: &quot;模型回答（必填）&quot;  }}]
我们开始吧!  &lt;Context&gt;  {text}  &lt;Context/&gt;&quot;&quot;&quot;
```

**5、提示词编写好之后**

我们构建Langchain 链。根据 Langchain 的ICEL语法，我们分别将提示词大模型和解析参数，组成 prompt | llm | parser 链。

**提示词选择上面两个，大**模型我们指定 Kimi的128 k ，结构化解析参数输出我们指定为： JasonJsonOutputParser 。

+

+

+

+

+

+

+

+

+

+

```
# LLM部分替换为MoonshotChatdef create_chain():    prompt = ChatPromptTemplate.from_messages([        (&quot;system&quot;, QA_PAIRS_SYSTEM_PROMPT),        (&quot;human&quot;, QA_PAIRS_HUMAN_PROMPT)    ])    llm = MoonshotChat(model=&quot;moonshot-v1-128k&quot;)  # 使用MoonshotChat    parser = JsonOutputParser(pydantic_object=QaPairs)    chain = prompt | llm | parser    return chain
```

对于结构化输出这一部分，我们可以参考langchain官方这一篇教程：

![](img/ed9325449fb2488513b729e4812c2b8a.png)

▲ https://python.langchain.com/v0.2/docs/how_to/structured_output/

## **6、上面的还没结束**

上面代码中 JsonOutputParser 传入的参数，也就是结果模型（QaPairs），我们需要定义一下。

+

+

+

+

+

+

+

+

+

```
# 结果模型定义class QaPair(BaseModel):    instruction: str = Field(description=&#x27;问题内容&#x27;)    #&quot;input&quot;: &quot;人类输入（选填）&quot;,    input: str = Field(description=&#x27;人类输入（针对问题内容,选填）&#x27;)    output: str = Field(description=&#x27;问题的回答&#x27;)
class QaPairs(BaseModel):    qas: List[QaPair] = Field(description=&#x27;问答对列表&#x27;)
```

**7、最后一步**

我们把上面的代码写在main里面，并且增加一些辅助代码用于将大模型生成的问答对存储在json文件中：

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

```
def main():    chain = create_chain()    documents = split_document(&#x27;data/12.txt&#x27;)  # 替换为你的文档路径
    # # 打印分块结果    # for i, doc in enumerate(documents):    #     print(f&quot;Document chunk {i + 1}:&quot;)    #     print(doc.page_content)    #     print(&quot;-&quot; * 80)
    # 继续处理文档    with open(&#x27;dataset.json&#x27;, &#x27;a&#x27;, encoding=&#x27;utf-8&#x27;) as f:  # 打开文件，使用 &#x27;a&#x27; 模式进行追加写入        bar = tqdm(total=len(documents))        for idx, doc in enumerate(documents):            print(doc.page_content)            # 调试API响应            print(f&quot;Processing document chunk {idx + 1}&quot;)            out = chain.invoke({&#x27;text&#x27;: doc.page_content})            print(f&quot;API response for chunk {idx + 1}: {out}&quot;)
            # 无论返回什么，直接写入文件            f.write(json.dumps(out, ensure_ascii=False, indent=2) + &quot;,\n&quot;)  # 实时写入并添加换行            f.flush()  # 确保数据立即写入磁盘            bar.update(1)        bar.close()
if __name__ == &#x27;__main__&#x27;:    main()
```

**8、我们运行一下，看一下效果。**

输入一个关于kimi介绍的文档：

![](img/77f46c28ed75e097af92ebf0f28921ad.png)

点击运行，代码会先进行分块，然后一个一个分块的去生成问答对：

![](img/906de4668a774062eccf4a5b8b7b505d.png)

控制台按照每一个分块被处理后显示进度：

![](img/bcb86604862ee94400fc738bb9621829.png)

最终输出我们需要的文件格式。

![](img/921de2a42e2d11a9711c68e7d4a4f4bd.png)

目前这个代码，三块是按照最大2048token，以及每一块重复为128 token来分块的（重复是为了增加块与块之间的联系），后续考虑给原文的增加分割符。

其实在编写代码的过程中，遇到了很多关于结构的错误，包括模型输出内容写入json里面。

**至此，第一部分结束~**

# 二、Kimi API接入FastGPT

FastGPT：一个免费、开源且功能强大的 AI 知识库平台，提供开箱即用的数据处理、模型调用、RAG 检索和可视化 AI 工作流。轻松构建复杂的 LLM 应用程序。

![](img/12d03af6d440d5ea3cb0d32e542668f5.png)

我们使用docker部署它，先确保你安装了Docker环境：

![](img/b07391c536af954b957363f15a340dab.png)

## 1、运行FastGPT部署指令：

+

+

+

+

+

+

```
mkdir fastgptcd fastgptcurl -O https://raw.githubusercontent.com/labring/FastGPT/main/projects/app/data/config.json
# pgvector 版本(测试推荐，简单快捷)curl -o docker-compose.yml https://raw.githubusercontent.com/labring/FastGPT/main/files/docker/docker-compose-pgvector.yml
```

![](img/e6c3a1edd177c6fb6794bb94d8a6c62b.png)

好的看来他已经完成了，在浏览器打开3000端口:http://localhost:3000/

![](img/398bf652b6daf5c2cdd876b394e1cbb6.png)

## 2、Fastgpt内部集成了One-API项目:

[O](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247488036&idx=1&sn=5cdfe4cc98081f30ea5add99b96be9dd&chksm=c1e09fe4f69716f264946500201ab789262b438fd4d4d090169ec7524d2af772561ff22f42d8&scene=21#wechat_redirect)ne-API：让国内LLM、Coze等跑通仅支持OpenAI API的开源项目 | 保姆级教程、可配合免费LLM API使用！

我们要把Kimi的api接入进去，所以我们打开One-API：http://localhost:3001

![](img/21842b9b80810c9703eda1e2d8babe5d.png)

点击渠道

![](img/877340715f8211d713276eda4d693a65.png)

把箭头上面的这些配好就行，密钥用前面Kimi官网生成的（免费密钥获取的Kimi API我会写在最后）。

点击测试，可以看到右上角测试通过

![](img/da76b4bdc29fce136e007e6cde025520.png)

然后向config文件加入moonshot-v1-128k模型

![](img/e3d1a52ee397312e32cb9a2cef274e70.png)

配置好后，由于配置了新的渠道，所以我们得重启一下：

+

```
docker-compose restart
```

![](img/4e29cef6f5f2569f03f7868ca0acdbf4.png)

备注：如果启动后发现one-api测试通过，但是fastgpt前端测试不通过，那么请运行

+

+

+

+

```
#先关闭删除容器docker-compose down#再运行docker-compose up -d
```

我们可以看到fastgpt多了一个： moonshot-v1-128k 模型，接入成功！

![](img/50e4e75024ff041ebef9aa585aeca047.png)

## **3、在fastgpt中新建一个知识库，文本处理模型**就选择moonshot-v1-128k

![](img/1b731895a029cd401034971a5210141d.png)

嵌入模型默认用的是Openai的（用不了），你可以选择去Ollama中下载一个，然后和上面配置Kimi API一样的步骤，把这个本地嵌入模型配置到One-API中。

![](img/ccd895b4b6370d6dfa3587f0abc82af9.png)

![](img/34154dd086089c3a1c03bbb7082c20e1.png)

记得config.json也要配置，fastgpt才有显示

![](img/5949137c639128b7afbb518293aa02b0.png)

两个模型都有之后，然后选择新建文本数据

![](img/2d685bd1999d77f0e978d2ecd7f9077e.png)

现在选择问答拆分+自动：

![](img/347aa14da2bbd2ac6996f10c4769c1af.png)

问答拆分的自定义规则是需要你的文本中包含分割符，并且输入拆分引导词：

![](img/b5a1bfbea67510713de944f2fd9b0665.png)

选择之后点击提交下一步，几分钟之后，他就可以得到这样的问答对

![](img/4bbaf80a2bfa4d5f00d47cc5b390b5b0.png)

选择一个查看：

![](img/2dafa8a30a97f9347c3f0b61fca29bb3.png)

QA问答对的知识库还可以导出为CSV格式：

![](img/94d72512047c9ab3e69c5ee5a1faa478.png)

![](img/00fb71de4a2a3f7e3341dc8de51d5090.png)

**至此，第2部分结束~**

# 三、用于学习与测试的免费Kimi API项目

![](img/f20e0b9a30ec83b9bc50f38db614c81d.png)

▲ https://github.com/LLM-Red-Team/kimi-free-api

主要是利用K imi-free-api 这个项目来获取免费实验性Kimi API，不要用于生产环境哦~

还要说一点，这个API不支持文件接口，只是Chat接口，

![](img/a81d220a012d4df292966290538494dd.png)

▲ Kimi官方文件接口API

## **1、你需要准备一台公网服务器：**

首先**从 kimi.moonsho**t.cn 获取refresh_token，进入Kimi 随便发起一个对话，然后F12打开开发者工具，从Application > Local Storage中找到 refresh_token 的值，这将作为API Key。

![](img/cb0bf13756b5346759103f530f5ca6c8.png)

记录上面的Api Key，然后部署这个项目，只需要运行：

+

```
docker run -it -d --init --name kimi-free-api -p 8001:8000 -e TZ=Asia/Shanghai vinlic/kimi-free-api:latest
```

## 2、Kimi在 本地编码中接入：Langchain实现QA问答对

**你需要把API Key替换**成上面的 refresh_token

![](img/27e83a4991835b0798c1b69799ce4142.png)

然后，你需要将Langchain的moonshot 包中的base url替换成服务器IP，比如： http://服务器ip:端口/v1

![](img/834a2c44f2a5fd7df235bf16e94e7f73.png)

> Langchain Python依赖包下面C:\Users\Aitrainee\anaconda3\Lib\site-packages\langchain_community\llms\moonshot.py，这个文件中找到： MOONSHOT_SERVICE_URL_BASE

## 3、Kimi 接入FastGPT的OneAPI

也同样很简单，你只需要把服务器ip填入到这个代理里，

![](img/b245b725099c14d3efc2aea1bc545b95.png)

或者选择类型的时候选择自定义渠道并填入服务器ip：

![](img/74d9562f78fb4a0ae92df9255234074a.png)

**至此第三部分结束~**

**全文代码：公众**号后台回复  ： Ki m i ，或者去下方Github仓库中 获取（后期更新处） ：https://github.com/Ai-trainee/LangChain-Notebooks

**总体来说 **，我非常喜欢Kimi的长文本处理能力。它支持TXT、PDF、Word文档、PPT幻灯片和Excel电子表格，且总字数最高可达20万字。

Kimi API兼容OpenAI 接口，这样方便项目迁移以及实验Github上更广泛的Openai 相关开源项目。

**除此以外 **，Kimi 的API还有 工具调用 功能，我们可以配合Coze、FastGPT、Dify、Langchain等框架来创建和管理这些工具，并配合 Kimi 大模型设计出更加复杂的工作流。

🌟希望这篇文章对你有帮助，感谢阅读！

如果你还没用过Kimi，可以直接在浏览器里搜索Kimi的官网：

https://kimi.ai 或者点击 下方 阅读原文 ，立即使用 👇 ， 主打一个免费又丝滑！