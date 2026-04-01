# 实战微软新一代RAG：GraphRAG强大的全局理解能力，碾压朴素RAG？

> 来源：[https://agijuejin.feishu.cn/docx/GUvxdSEi6odIplxkIiecxxtSnXb](https://agijuejin.feishu.cn/docx/GUvxdSEi6odIplxkIiecxxtSnXb)

> 作者：还就可 | 深入LLM Agent应用开发

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/gk6lQuDaAurUEIun_S_eEg)qq.com/s/gk6lQuDa...

**微软近日开源了新一代RAG**框架 GraphRAG [1] ，以解决当前RAG在大型语料库上全局理解问题。当前RAG主要聚焦于局部检索能力，即根据查询语句在向量库中匹配部分知识，然后通过大型语言模型合成这些检索到的信息，生成一个自然流畅的回答。相信大部分同学看过《仙逆》这部小说，如果你问王林这一生有几个相好？如果让RAG来回答，它能回答出来吗？而GraphRAG通过两个阶段构建基于图的文本索引：首先从源文档中推导出实体知识图谱，然后为所有紧密相关的实体群体生成社区摘要。给定一个问题，每个社区摘要用于生成部分回应，然后将所有部分回应总结为最终用户的回答。通过这样的方法，那么我们再问王林这一生有多少女人，是不是会容易的多了呢？ 下图来自 百 度 百科-王林词条 [2] 。 本文将先首先概述RAG和GraphRAG，然后介绍如何安装、如何使用GraphRAG对《仙逆》进行索引和回答测试。

> 索引整本小说耗资太过巨大，时间也较长，本文只采用前面10章作为案例用以说明。下一篇，我们将使用LlamaIndex测试同样问题进行对比全局理解能力。

![](img/02fa422a6a3781f0cf9d156c21c7adec.png)

## ** 1. RAG概述**

大语言模型（LLM）是在大量数据上训练，但他们并不是在我们私有数据上训练，因此要想让LLM能够回答我们私有数据集上的问题，我们就得使用一种叫做检索增强生成（RAG）的技术。它的基本原理是加载我们的文档，然后将每个文档按照一定的规则，比如Token数量、字符数量、语义和层次分割等，将每个文档拆分为一个一个小片段（chunk）。然后使用嵌入技术对这些chunk生成embeding，存储到高维向量数据库中，生成索引Index。在用户查询的时候，将查询语句转换为embeding，然后向高维向量数据库寻找和查询embeding最相似的Top K个embeding，将其对应的文本以及对应的上下文发送给LLM，从而利用LLM合成自然流畅地回答返回给用户，如下图所示。

![](img/56c9057dbd0edff0b8a264e79314fcb0.png)

但是像**数据集中的核心主题是什么？**这类问题需要查询聚焦摘要（Query focused summary）而不是像我们上述RAG系统那样显式检索，现有的QFS方法无法扩展到RAG系统索引的文本量。而GraphRAG结合知识图谱，RAG和QFS这些方法的优势，它可以根据用户问题的普遍性和要索引的源文本量进行扩展。该方法使用LLM构建基于图的文本索引，包括从源文档派生实体知识图谱，然后为所有密切相关的实体组生成社区摘要。给定一个问题后，每个社区摘要用于生成部分回答，然后所有部分回答再次汇总为最终回答。具体步骤和流程图如下：

+   **源文档到文本块 **：一个基本的设计决策是决定从源文档中提取的输入文本应以何种粒度分割成文本块进行处理。

+   **文本块到元素实例 **：这一步骤的基本要求是识别并提取每个文本块中的图节点和边的实例。使用一个多部分的LLM提示来首先识别文本中的所有实体，包括它们的名称、类型和描述，然后识别所有明确相关实体之间的关系，包括源实体和目标实体以及它们关系的描述。

+   **元素实例到元素摘要 **：使用LLM将实例级摘要转换为每个图元素的描述性文本块。使用LLM“提取”源文中表示的实体、关系和声明的描述已经是一种抽象摘要的形式，依赖于LLM创建独立有意义的摘要，这些摘要可能是文本本身暗示但没有明确陈述的概念（例如，暗示关系的存在）。要将所有这些实例级摘要转换为每个图元素（即实体节点、关系边和声明协变量）的单一描述性文本块，需要进一步通过匹配实例组进行LLM摘要。

+   **元素摘要到图社区 **：使用社区检测算法将图划分为模块化社区。前一步创建的索引可以被建模为一个同质无向加权图，其中实体节点通过关系边连接，边权重表示检测到的关系实例的归一化计数。给定这样的图，可以使用各种社区检测算法将图划分为节点之间相互连接的社区。在流程中使用Leiden算法，因为它能够高效地恢复大规模图的层次社区结构。这个层次结构的每个级别都提供了一个社区划分，以相互独立、集体穷尽的方式覆盖图中的节点，使得分而治之的全局摘要成为可能。

+   **图社区到社区摘要 **：为Leiden层次结构中的每个社区创建报告式摘要。这一步是使用旨在扩展到非常大的数据集的方法为Leiden层次结构中的每个社区创建报告式摘要。这些摘要本身是有用的，因为它们是理解数据集的全局结构和语义的一种方式，并且它们自己可以用来在没有问题的情况下理解语料库。

+   社区摘要到社区回答再到全局回答 ：使用社区摘要生成最终答案的多阶段过程。准备社区摘要，社区摘要被随机打乱并分成预指定token大小的块。这确保了相关信息分散在各个块中，而不是集中在一个上下文窗口中（可能丢失）。映射社区回答，并行生成中间答案，每个块一个。LLM也被要求生成一个0-100之间的分数，以指示生成的答案在回答目标问题方面的有用程度。得分为0的答案被过滤掉。汇总为全局答案，按有用性得分降序排列的中间社区答案被迭代地添加到新的上下文窗口中，直到达到token限制。这个最终上下文被用来生成返回给用户的全局答案。

![](img/7119c59985b06df7f2703b7389653a1c.png)

##  2 GraphRAG安装与配置

由于GraphRAG刚开源，部署起来还有很多问题，所以本次采用源码安装测试，方便调试修改代码。

```
git clone https://github.com/microsoft/graphrag.git
```

安装poetry，然后安装依赖 poetry install 。当所有依赖安装好之后，我们cd到graphrag目录，在当前目录准备初始化：

```
poetry run poe index --init --root .
```

它会在当前目录创建input、output、prompts、cache目录，以及.env文件和settings.yaml配置文件。

+   input目录，存放需要索引的文件，默认配置只支持txt，当然也可以修改settings.yaml

+   output目录，存储生成的图、以及总结、日志等信息。

+   prompts目录，存储默认的4个提示词文件：claim_extraction.txt、community_report.txt、entity_extraction.txt、summarize_descriptions.txt。

+   cache目录，用于缓存提取实体和摘要等，能够减少LLM调用

+   .env文件中只包含一个字幕GRAPHRAG_API_KEY，用于设置你的LLM API_KEY。

+   settings.yaml 文件较为复杂，配置项目也较多，运行本项目只需要修改两个。

将想要索引的文件放入input文件夹。准备配置settings.yaml和.env文件，我没有OpenAI的key，所以只能使用非Open AI LLM，如果你经费充足可以直接使用OpenAI的LLM，但是github上有人测试一次花了他46美刀。可以使用免费LLM且兼容OpenAI SDK的，有通义千问、月之暗面还有Groq。

使用通义千问作为LLM时候，问题较多，兼容性有点差，但是速度没什么限制，基本上能够用到100RPM，TPM没限制。

+   top_p 必须设置0-1之间，要小于1，比如0.99这样。

+   max_tokens 必须设置为2000，超过会报错。

+   还有其他未知的错误，不要使用qwen-long，好像输入过长就会报错，让你走文件上传，建议使用qwen-turbo。

使用Groq，Groq上的模型除了Mistral的上下文窗口是32K，其他的诸如llama3 70B、Gemma等都是8192。对于GraphRAG，由于它在总结时候，一个Prompt就超过8K，所以上下文窗口小的模型基本可以忽略，即使速度很快。但是Mistral的8x7B的模型，在免费用户上的限制也较大，虽说可以达到30每分钟请求，但是每分钟Token输出最多5000，这个非常限制速度，根据我的观察，有时候一次返回就超过2000了，意味着你每分钟只能请求一次。

月之暗面的免费用户可以使用所有的模型，moonshot-v1-32k作为目标模型，它的速率限制是每分钟3次请求，每分钟32000 Token限制。所以几项比较下来，还是通义千问速度快点。

> 其实，在实体提取的时候，可以使用Groq的Llama3 7B模型，速度较快，而且TPM和RPM都很大。等到实体提取结束后，报context window时候，再切换回通义千问，速度上相对能接受一点。

那么在.env中配置你的API KEY，然后打开settings.yaml，找到llm这一项，修改model、api_base等信息。其中max_tokens是指响应的最大Tokens数量而不是输入。另外要注意的时候 tokens_per_minute 和 request_per_minute 在你本地是不生效的，你应该使用它的缩写tpm和rpm，我已经提了PR尝试去修复了。这两个值一定要根据你的模型提供方去设置，不然日志中会有很多报错。

> PS：已经被修复了，其他人早我4小时提的😅

```
llm:
  api_key: ${GRAPHRAG_API_KEY}
  type: openai_chat # or azure_openai_chat
  model: qwen-turbo-0624
  model_supports_json: false # recommended if this is available for your model.
  api_base: https://dashscope.aliyuncs.com/compatible-mode/v1
  max_tokens: 2000
  concurrent_requests: 1 # the number of parallel inflight requests that may be made
  tokens_per_minute: 30000 # set a leaky bucket throttle
  requests_per_minute: 30 # set a leaky bucket throttle
  top_p: 0.99
  max_retries: 3
  max_retry_wait: 10
  sleep_on_rate_limit_recommendation: true # whether to sleep when azure suggests wait-times
```

第二个要修改的是embedding项，目前GraphRAG只支持OpenAI Embedding接口兼容的模型。幸运的时候，如果你看过我之前的文章《喂饭教程！使用Llama.cpp在MAC M1上安装私有大语言模型LLM通义千问Qwen 1.5-7B》就知道llama.cpp启动的模型服务是完全兼容emebdding接口的。已知的Ollama是不兼容该接口的，LM Studio不太清楚。所以，我们使用命令 ./server -m ./models/mymodels/qwen1.5-chat-ggml-model-Q4_K_M.gguf -c 8192 -n -1 -t 7 --embeddings 启动llama.cpp，它的默认端口是8080。最后按照如下配置settings.yaml中的embeddings条目，主要是 api_base 的配置，注意没有 v1 ，其他项目比如model随便填写。

```
embeddings:
  ## parallelization: override the global parallelization settings for embeddings
  async_mode: threaded # or asyncio
  llm:
    api_key: ${GRAPHRAG_API_KEY}
    type: openai_embedding # or azure_openai_embedding
    model: text-embedding-ada-002
    api_base: http://localhost:8080
    batch_size: 1 # the number of documents to send in a single request
```

## ** 3 GraphRAG索引**

使用如下命令，开始建立索引，包括使用LLM抽取实体信息和总结等。

```
poetry run poe index --root .
```

进入漫长的等待，经过以下过程才能完成索引建立，耗时非常长，可能会耗费非常多的时间。对照第一节中GraphRAG流程如下，方便读者朋友们理解这个过程。

![](img/2a5d8252787729c0f5f32eb105ae4daf.png)

![](img/f60b11903ba083322d1c5d4df1f74a2f.png)

![](img/44311aed805cd59c617bcb0f3f41ed15.png)

![](img/a5ddda611a4c95697b5bd9f75919ee9a.png)

![](img/96b79242efcdce0814005076f39c4789.png)

![](img/9e645dd0073a38a5823e6852b2f41897.png)

![](img/dc838c444fbf92fdf04025447c6eed86.png)

![](img/d9afcd24c262f0b8bd22da93a26473b0.png)

![](img/6d93d9bb33f7f07a36e41552c3c44322.png)

![](img/b55946587609f0c62724ab661002f599.png)

![](img/39d53969fba3d7160439e2ec908c926c.png)

![](img/d4c3c7ec7e2ad3d289b00933b3b5e09e.png)

如果整个流程完成，那么最终整个过程如下。

```
⠙ GraphRAG Indexer 
├── Loading Input (text) - 1 files loaded (0 filtered) ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 0:00:00 0:00:00
├── create_base_text_units
├── create_base_extracted_entities
├── create_summarized_entities
├── create_base_entity_graph
├── create_final_entities
├── create_final_nodes
├── create_final_communities
├── join_text_units_to_entity_ids
├── create_final_relationships
├── join_text_units_to_relationship_ids
├── create_final_community_reports
├── create_final_text_units
├── create_base_documents
└── create_final_documents
🚀 All workflows completed successfully.
```

## ** 4 GraphRAG查询**

GraphRAG的查询分为局部查询和全局查询。全局查询方法通过以 map-reduce 方式搜索所有 AI 生成的社区报告来生成答案。这是一种资源密集型方法，需要LLM支持的context window足够大，最好是32K的模型，但通常可以很好地回答需要了解整个数据集的问题。局部查询方法通过将 AI 提取到知识图谱中的相关数据与原始文档的文本块相结合来生成答案，此方法适用于需要了解文档中提到的特定实体的问题。

+   全局查询

```
poetry run poe query --root . --method global &quot;在这个故事中王林的经历有哪些&quot;
```

> SUCCESS: Global Search Response: 王林在故事中是一位核心角色，他的经历丰富多彩。在没有特殊天赋的情况下，王林因其坚定的决心赢得了中年男子的认可，并在他的指导下开始了一段象征性的成长旅程。这段旅程以攀登石阶为代表，展现了王林面对的种种挑战和他个人的发展。在攀登过程中，他甚至流血不止，体现出极大的毅力。王林的生活中还交织着对“内丹”的追求，这种神秘的丹药据信含有长生不老的力量，激发了他去寻找和利用它。

> 王林与恒岳派有着密切的联系，他一度被该派弟子抓住，后来得到解救，这似乎是一个试炼或评估的过程。他也成为锦袍老者的弟子，这一变化可能引起了家族和组织内部的冲突。同时，王林是王家家族的一员，与父亲关系亲密，他的安全和成长受到了家族的关注。

> 此外，王林与一群青少年有着特殊的关系，他们可能组成了师徒关系，围绕着一位被称为'仙师'的人物。王林对“仙人”的概念着迷，反映了他对于超自然能力和永生的渴望。他的故事中还包含了与Heng Yue Faction青年的互动，这些交流影响了社区的动态和选拔过程。

> 王林的个人旅程不仅仅是个人成长，也影响了他所在的村庄和组织。他的离去对村庄产生了显著影响，而在恒岳派，他通过自我牺牲和决心逐渐提升地位，象征着命运的转变。然而，王林的故事中也有心理层面的挑战，如来自女眷社区的嘲笑，以及在家族内部面临的压力和批评。

> 总而言之，王林的故事包含了个人奋斗、师徒关系、家族纷争、组织内部的动力学以及对超凡力量的探索，这些丰富多样的经历构成了他复杂的角色画像。

+   局部查询

```
poetry run poe query --root . --method local &quot;谁是王林 他的主要人际关系是什么样&quot;
```

> SUCCESS: Local Search Response: 王林是故事中的主角，一个具有重要地位的人物。在故事中，王林与多个关键人物建立了复杂的关系，这些关系构成了他角色的多维度形象。以下是王林的主要人际关系概述：

> **家庭关系**

+   **父亲 **：王林的父亲在故事中扮演了指导者和支持者的角色。他在关键时刻为王林提供了保护，并给予了忠告，如“王林你记住，以后无论如何，都不要忘记你四叔对咱们家的恩惠”。这种父爱的表现体现了家庭关系中的责任与关爱。

+   **母亲 **：王林的母亲对他的关心和爱护贯穿始终，如在王林未被仙人收入时给予鼓励，“娃儿，没事，不用难过，不就是没被仙人收入么，等明年咱们去县里大考，也是一样”。母亲的温暖支持是王林情感世界的重要支柱。

> **社交关系**

+   **中年汉子 **（王林的叔叔）：中年汉子是王林的直系亲属，他对王林有着深厚的亲情和教育意义。他建议王林参加恒岳派的选拔，并认为王林有可能成为记名弟子，体现了他对王林的期望和对家族未来的考虑。

+   **恒岳派 **：恒岳派是一个包含众多仙人的组织，王林有机会通过推荐成为其记名弟子。这一机会不仅展示了王林的潜力，也反映了恒岳派在故事中的权威性和影响力。

+   **铁柱（王林的兄弟） **：王林与铁柱之间的关系表明了兄弟间的互相扶持与竞争，如铁柱对王林的关心和王林对铁柱的尊敬，体现了兄弟间深厚的情感纽带。

+   **其他亲戚 **：故事中提到的王林的其他亲戚，如张姓弟子，他们在关键时刻给予帮助或提供信息，展现了王林社会网络中的人际互动。

> **故事背景 **王林的故事背景涉及山脉、家族传统、家族推荐制度以及修炼文化等元素，这些背景构建了他成长与发展的环境。例如，王林通过参加恒岳派的测试，展示了其个人潜能和挑战自我，同时也揭示了修炼文化在故事中的重要性。

> 综上所述，王林的角色不仅体现在家庭和社交层面的亲密关系上，还涉及到更广泛的社会结构和文化背景，这些关系和背景共同塑造了他作为故事主角的独特性格和命运轨迹。

遗憾的是，没有能分清楚王林的小名铁柱。即使再次追问，王林和铁柱的关系，依然是两个角色。

> SUCCESS: Local Search Response: 王林和铁柱都是故事中的主要人物。

+   **王林 **：故事中描述的主人公，一个年轻男孩，属于王氏家族。他具有非凡的决心，但天赋平平，正处于努力求生存的状态。王林性格冷静且观察敏锐，对长辈充满尊敬，并保持着一定的内敛和谨慎，希望能够尽快回家。同时，他坚定而自信，甚至接受了第二次考验，表现出了坚决完成任务的决心。王林还对一些古典引述熟悉，并且是大县大家族的一员。

+   **铁柱 **： 故事中的人物，似乎与王林关系密切。 铁柱的父亲是故事中提及的角色之一，铁柱在家中被寄予厚望，他的成就让家人感到骄傲，特别是他的父亲。 铁柱作为家族年轻一代的代表，承载着家族的期望，尤其是在可能成为恒岳派弟子方面。

> 综上所述，王林和铁柱都是故事中关键的角色，分别代表了家族中的不同身份和期望，他们在家族和社区中扮演着重要角色，与恒岳派也存在一定的关联。

## ** 5. 总结**

本文**通过介绍微软新**一代GraphRAG，从概念到安装、索引和查询，并以热门网络小说《仙逆》为例，说明GraphRAG在全局理解，和实体关系提取等强大之处。在此过程中，我遇到了非常多的问题，仅仅Groq消耗了 470.3万 Token，而通义千问在后来修复各种bug后也消耗了45万的Token额度。虽然所耗甚巨，但Groq是免费的啊，感谢Groq～通义千问的模型免费了100万Token额度，我一天就消耗了45万。不得不说GraphRAG使用LLM提取实体关系、摘要等信息，确实是非常消耗LLM算力。相比较起来，朴素RAG在索引阶段只需要使用本地Embedding模型比如BAAI的bge-base-zh-v1.5等模型提取嵌入即可。因此，从成本上来看GraphRAG不占任何优势。

希望本文对各位读者有所帮助。下一篇我们将使用LLamaIndex也索引《仙逆》前10章，并使用相同问题进行问询以便对比。关注我，下一篇更精彩。

若您意犹未尽，渴望亲自操刀，将理论付诸实践，那么这本《大模型应用开发：动手做AI Agent》和《LangChain编程：从入门到实践》将是您理想的选择。

## **参考资料**

[1]

GraphRAG: https://github.com/microsoft/graphrag

[2]

百部百科-王林词条: https://baike.baidu.com/lemmagraph/graphview?lemmaId=5175358&featureId=660f43d9127d4de232fef12a&classify=novel&fromModule=lemma_graph-tree