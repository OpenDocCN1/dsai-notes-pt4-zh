# 第四节 生成集成与系统整合

> 来源：[https://gotoaiworld.feishu.cn/docx/F2NTdstUpokkS2xk69qcnVmjnCc](https://gotoaiworld.feishu.cn/docx/F2NTdstUpokkS2xk69qcnVmjnCc)

Boss要打完喽！在最后一节来学习一下如何实现智能的生成集成模块，以及将所有模块整合成一个完整的RAG系统。

## [一、生成集成模块](https://datawhalechina.github.io/all-in-rag/#/chapter8/04_generation_sys?id=%e4%b8%80%e3%80%81%e7%94%9f%e6%88%90%e9%9b%86%e6%88%90%e6%a8%a1%e5%9d%97)

生成集成模块是整个RAG系统的"大脑"，负责理解用户意图、路由查询类型，并生成高质量的回答。

> generation_integration.py完整代码

### [1.1 设计思路](https://datawhalechina.github.io/all-in-rag/#/chapter8/04_generation_sys?id=_11-%e8%ae%be%e8%ae%a1%e6%80%9d%e8%b7%af)

智能查询路由：根据用户查询自动判断是列表查询、详细查询还是一般查询，选择最适合的生成策略。

查询重写优化：对模糊不清的查询进行智能重写，提升检索效果。比如将"做菜"重写为"简单易做的家常菜谱"。

多模式生成：

+   列表模式：适用于推荐类查询，返回简洁的菜品列表

+   详细模式：适用于制作类查询，提供分步骤的详细指导

+   基础模式：适用于一般性问题，提供常规回答

> [上面说到的两种](https://github.com/datawhalechina/all-in-rag/blob/main/docs/chapter4/14_query_rewriting.md)主要方法可以回顾 查询重构与分发

### [1.2 类结构设计](https://datawhalechina.github.io/all-in-rag/#/chapter8/04_generation_sys?id=_12-%e7%b1%bb%e7%bb%93%e6%9e%84%e8%ae%be%e8%ae%a1)

```
class GenerationIntegrationModule:&quot;&quot;&quot;生成集成模块 - 负责LLM集成和回答生成&quot;&quot;&quot;def __init__(self, model_name: str = &quot;kimi-k2-0711-preview&quot;, 
                 temperature: float = 0.1, max_tokens: int = 2048):
        self.model_name = model_name
        self.temperature = temperature
        self.max_tokens = max_tokens
        self.llm = None
        self.setup_llm()Copy to clipboardErrorCopied
```

+   temperature: 生成温度，控制回答的创造性

+   max_tokens: 最大生成长度

+   llm: Moonshot Chat模型实例

### [1.3 查询路由实现](https://datawhalechina.github.io/all-in-rag/#/chapter8/04_generation_sys?id=_13-%e6%9f%a5%e8%af%a2%e8%b7%af%e7%94%b1%e5%ae%9e%e7%8e%b0)

```
def query_router(self, query: str) -&gt; str:&quot;&quot;&quot;查询路由 - 根据查询类型选择不同的处理方式&quot;&quot;&quot;
    prompt = ChatPromptTemplate.from_template(&quot;&quot;&quot;
根据用户的问题，将其分类为以下三种类型之一：

1. &#x27;list&#x27; - 用户想要获取菜品列表或推荐，只需要菜名
   例如：推荐几个素菜、有什么川菜、给我3个简单的菜

2. &#x27;detail&#x27; - 用户想要具体的制作方法或详细信息
   例如：宫保鸡丁怎么做、制作步骤、需要什么食材

3. &#x27;general&#x27; - 其他一般性问题
   例如：什么是川菜、制作技巧、营养价值

请只返回分类结果：list、detail 或 general

用户问题: {query}

分类结果:&quot;&quot;&quot;)# ... (LCEL链式调用)return resultCopy to clipboardErrorCopied
```

查询路由是整个系统的关键，决定了后续的处理流程。通过LLM自动判断查询意图，比简单的关键词匹配更准确。

### [1.4 查询重写优化](https://datawhalechina.github.io/all-in-rag/#/chapter8/04_generation_sys?id=_14-%e6%9f%a5%e8%af%a2%e9%87%8d%e5%86%99%e4%bc%98%e5%8c%96)

```
def query_rewrite(self, query: str) -&gt; str:&quot;&quot;&quot;智能查询重写 - 让大模型判断是否需要重写查询&quot;&quot;&quot;# 使用LLM分析查询是否需要重写# 具体明确的查询（如&quot;宫保鸡丁怎么做&quot;）保持原样# 模糊查询（如&quot;做菜&quot;、&quot;推荐个菜&quot;）进行重写优化# ... (提示词设计和LCEL链式调用)return responseCopy to clipboardErrorCopied
```

查询重写能够将模糊的用户输入转换为更适合检索的查询，显著提升系统的实用性。重写规则包括：保持原意不变、增加相关烹饪术语、优先推荐简单易做的菜品。

### [1.5 多模式生成](https://datawhalechina.github.io/all-in-rag/#/chapter8/04_generation_sys?id=_15-%e5%a4%9a%e6%a8%a1%e5%bc%8f%e7%94%9f%e6%88%90)

列表模式生成：

```
def generate_list_answer(self, query: str, context_docs: List[Document]) -&gt; str:&quot;&quot;&quot;生成列表式回答 - 适用于推荐类查询&quot;&quot;&quot;# 提取菜品名称
    dish_names = []for doc in context_docs:
        dish_name = doc.metadata.get(&#x27;dish_name&#x27;, &#x27;未知菜品&#x27;)if dish_name not in dish_names:
            dish_names.append(dish_name)# 构建简洁的列表回答if len(dish_names) &lt;= 3:return f&quot;为您推荐以下菜品：\n&quot; + &quot;\n&quot;.join([f&quot;{i+1}. {name}&quot; for i, name in enumerate(dish_names)])# ... (其他情况处理)Copy to clipboardErrorCopied
```

详细模式生成：

```
def generate_step_by_step_answer(self, query: str, context_docs: List[Document]) -&gt; str:&quot;&quot;&quot;生成分步骤回答&quot;&quot;&quot;# 使用结构化提示词，包含：# - 🥘 菜品介绍# - 🛒 所需食材# - 👨‍🍳 制作步骤# - 💡 制作技巧# ... (提示词设计和LCEL链式调用)return responseCopy to clipboardErrorCopied
```

详细模式使用结构化的提示词设计，让LLM能够生成格式规范、内容丰富的分步骤指导，重点突出实用性和可操作性。

## [二、系统整合](https://datawhalechina.github.io/all-in-rag/#/chapter8/04_generation_sys?id=%e4%ba%8c%e3%80%81%e7%b3%bb%e7%bb%9f%e6%95%b4%e5%90%88)

主程序负责协调各个模块，实现完整的RAG流程：数据准备 → 索引构建 → 检索优化 → 生成集成。同时提供了索引缓存、交互式问答等实用功能。

> [main.py完整代码](https://github.com/datawhalechina/all-in-rag/blob/main/code/C8/main.py)

### [2.1 主系统类设计](https://datawhalechina.github.io/all-in-rag/#/chapter8/04_generation_sys?id=_21-%e4%b8%bb%e7%b3%bb%e7%bb%9f%e7%b1%bb%e8%ae%be%e8%ae%a1)

```
class RecipeRAGSystem:&quot;&quot;&quot;食谱RAG系统主类&quot;&quot;&quot;def __init__(self, config: RAGConfig = None):
        self.config = config or DEFAULT_CONFIG
        self.data_module = None
        self.index_module = None
        self.retrieval_module = None
        self.generation_module = None# 检查数据路径和API密钥if not Path(self.config.data_path).exists():raise FileNotFoundError(f&quot;数据路径不存在: {self.config.data_path}&quot;)if not os.getenv(&quot;MOONSHOT_API_KEY&quot;):raise ValueError(&quot;请设置 MOONSHOT_API_KEY 环境变量&quot;)Copy to clipboardErrorCopied
```

主系统类负责协调所有模块，确保系统的完整性和一致性。

### [2.2 系统初始化流程](https://datawhalechina.github.io/all-in-rag/#/chapter8/04_generation_sys?id=_22-%e7%b3%bb%e7%bb%9f%e5%88%9d%e5%a7%8b%e5%8c%96%e6%b5%81%e7%a8%8b)

```
def initialize_system(self):&quot;&quot;&quot;初始化所有模块&quot;&quot;&quot;# 1. 初始化数据准备模块
    self.data_module = DataPreparationModule(self.config.data_path)# 2. 初始化索引构建模块
    self.index_module = IndexConstructionModule(
        model_name=self.config.embedding_model,
        index_save_path=self.config.index_save_path
    )# 3. 初始化生成集成模块
    self.generation_module = GenerationIntegrationModule(
        model_name=self.config.llm_model,
        temperature=self.config.temperature,
        max_tokens=self.config.max_tokens
    )Copy to clipboardErrorCopied
```

初始化过程按照依赖关系有序进行，保证每个模块都能正确设置。

### [2.3 知识库构建流程](https://datawhalechina.github.io/all-in-rag/#/chapter8/04_generation_sys?id=_23-%e7%9f%a5%e8%af%86%e5%ba%93%e6%9e%84%e5%bb%ba%e6%b5%81%e7%a8%8b)

```
def build_knowledge_base(self):&quot;&quot;&quot;构建知识库&quot;&quot;&quot;# 1. 尝试加载已保存的索引
    vectorstore = self.index_module.load_index()if vectorstore is not None:# 加载已有索引，但仍需要文档和分块用于检索模块
        self.data_module.load_documents()
        chunks = self.data_module.chunk_documents()else:# 构建新索引的完整流程
        self.data_module.load_documents()
        chunks = self.data_module.chunk_documents()
        vectorstore = self.index_module.build_vector_index(chunks)
        self.index_module.save_index()# 初始化检索优化模块
    self.retrieval_module = RetrievalOptimizationModule(vectorstore, chunks)Copy to clipboardErrorCopied
```

这个流程运用了之前设计的索引缓存机制，能够大幅提升系统启动速度。

### [2.4 智能问答流程](https://datawhalechina.github.io/all-in-rag/#/chapter8/04_generation_sys?id=_24-%e6%99%ba%e8%83%bd%e9%97%ae%e7%ad%94%e6%b5%81%e7%a8%8b)

```
def ask_question(self, question: str, stream: bool = False):&quot;&quot;&quot;回答用户问题&quot;&quot;&quot;# 1. 查询路由
    route_type = self.generation_module.query_router(question)# 2. 智能查询重写（根据路由类型）if route_type == &#x27;list&#x27;:
        rewritten_query = question  # 列表查询保持原样else:
        rewritten_query = self.generation_module.query_rewrite(question)# 3. 检索相关子块
    relevant_chunks = self.retrieval_module.hybrid_search(rewritten_query, top_k=self.config.top_k)# 4. 根据路由类型选择回答方式if route_type == &#x27;list&#x27;:# 列表查询：返回菜品名称列表
        relevant_docs = self.data_module.get_parent_documents(relevant_chunks)return self.generation_module.generate_list_answer(question, relevant_docs)else:# 详细查询：获取完整文档并生成详细回答
        relevant_docs = self.data_module.get_parent_documents(relevant_chunks)if route_type == &quot;detail&quot;:# 详细查询使用分步指导模式return self.generation_module.generate_step_by_step_answer(question, relevant_docs)else:# 一般查询使用基础回答模式return self.generation_module.generate_basic_answer(question, relevant_docs)Copy to clipboardErrorCopied
```

这部分展示了程序执行流程：智能路由 → 查询优化 → 混合检索 → 父子文档处理 → 多模式生成。

### [2.5 实际使用示例](https://datawhalechina.github.io/all-in-rag/#/chapter8/04_generation_sys?id=_25-%e5%ae%9e%e9%99%85%e4%bd%bf%e7%94%a8%e7%a4%ba%e4%be%8b)

#### [2.5.1 不同查询类型的效果](https://datawhalechina.github.io/all-in-rag/#/chapter8/04_generation_sys?id=_251-%e4%b8%8d%e5%90%8c%e6%9f%a5%e8%af%a2%e7%b1%bb%e5%9e%8b%e7%9a%84%e6%95%88%e6%9e%9c)

列表查询示例：

```
用户问题: &quot;推荐几道简单的素菜&quot;
查询类型: list
生成结果:
为您推荐以下菜品：
1. 西红柿炒鸡蛋
2. 土豆丝
3. 青椒炒豆腐Copy to clipboardErrorCopied
```

详细查询示例：

```
用户问题: &quot;宫保鸡丁怎么做？&quot;
查询类型: detail
生成结果:
## 🥘 菜品介绍
宫保鸡丁是一道经典川菜，口感麻辣鲜香...

## 🛒 所需食材
- 鸡胸肉 300g
- 花生米 100g
- 干辣椒 10个
...

## 👨‍🍳 制作步骤
1. 鸡肉切丁，用料酒和生抽腌制15分钟
2. 热锅下油，爆炒花生米至微黄盛起
...Copy to clipboardErrorCopied
```

#### [2.5.2 交互式问答](https://datawhalechina.github.io/all-in-rag/#/chapter8/04_generation_sys?id=_252-%e4%ba%a4%e4%ba%92%e5%bc%8f%e9%97%ae%e7%ad%94)

系统提供了完整的命令行交互界面，启动时会显示"尝尝咸淡RAG系统"的欢迎信息：

```
def run_interactive(self):&quot;&quot;&quot;运行交互式问答&quot;&quot;&quot;print(&quot;=&quot; * 60)print(&quot;🍽️  尝尝咸淡RAG系统 - 交互式问答  🍽️&quot;)print(&quot;=&quot; * 60)print(&quot;💡 解决您的选择困难症，告别&#x27;今天吃什么&#x27;的世纪难题！&quot;)# 初始化系统和构建知识库
    self.initialize_system()
    self.build_knowledge_base()while True:
        user_input = input(&quot;\n您的问题: &quot;).strip()if user_input.lower() in [&#x27;退出&#x27;, &#x27;quit&#x27;, &#x27;exit&#x27;]:break# 询问是否使用流式输出
        stream_choice = input(&quot;是否使用流式输出? (y/n, 默认y): &quot;).strip().lower()
        use_stream = stream_choice != &#x27;n&#x27;if use_stream:# 流式输出，实时显示生成过程for chunk in self.ask_question(user_input, stream=True):print(chunk, end=&quot;&quot;, flush=True)else:# 普通输出
            answer = self.ask_question(user_input, stream=False)print(answer)Copy to clipboardErrorCopied
```

运行效果示例：

```
============================================================
🍽️  尝尝咸淡RAG系统 - 交互式问答  🍽️
============================================================
💡 解决您的选择困难症，告别&#x27;今天吃什么&#x27;的世纪难题！

✅ 成功加载已保存的向量索引！
✅ 系统初始化完成！

您的问题: 推荐几道简单的素菜
是否使用流式输出? (y/n, 默认y): y

为您推荐以下素菜：
1. 西红柿炒鸡蛋 - 经典家常菜，简单易做
2. 土豆丝 - 爽脆可口，适合新手
3. 青椒炒豆腐 - 营养丰富，制作简单Copy to clipboardErrorCopied
```

流式输出的实现通过LangChain的chain.stream()方法，它会返回一个生成器，每次yield一个文本片段。在交互式界面中，通过print(chunk, end="", flush=True)实时输出每个片段，end=""避免换行，flush=True确保立即显示，从而实现逐字逐句的流式效果。

## [三、优化方向](https://datawhalechina.github.io/all-in-rag/#/chapter8/04_generation_sys?id=%e4%b8%89%e3%80%81%e4%bc%98%e5%8c%96%e6%96%b9%e5%90%91)

虽然当前系统已经具备了完整的RAG功能，但仍有许多优化空间。未来的优化可以聚焦于几个关键方向的融合与深化：可以通过 集成图数据库 将食谱数据构建为知识图谱，来揭示食材、菜品与烹饪方法间的复杂关联，进而支持复杂关系查询（如“和鸡肉搭配的食材有哪些”）、发掘潜在的食材组合并实现基于图的智能推荐。还可以 融合多模态数据，结合菜品图片等视觉信息，利用多模态模型进行图文联合检索，不仅能支持“这是什么菜”的视觉搜索，还可以通过图像识别食材来推荐相关菜谱。或者通过 增强专业知识，集成营养成分数据库、烹饪技巧知识图谱以及食材替换规则库等外部知识源，系统将能提供精准的营养分析、专业的烹饪指导，并灵活适应用户的饮食过敏或个人偏好。