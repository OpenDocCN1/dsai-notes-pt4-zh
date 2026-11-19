
# 高级RAG案例：Self-querying Retrieval，提升结构化数据检索效率

> 来源：[https://agijuejin.feishu.cn/docx/Xg0kdIrznogcLLxR9GNcDOcenUb](https://agijuejin.feishu.cn/docx/Xg0kdIrznogcLLxR9GNcDOcenUb)

> 作者：王几行XING

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/680962196)hihu.com/p/680962...

## Self-querying Retrieval 简介

自查询检索器，顾名思义，具有对自身进行查询的能力。具体来说，给定任何自然语言查询，检索器使用一个构造查询的LLM链来编写一个结构化查询，然后将该结构化查询应用到其底层的VectorStore上。这使得检索器不仅能够使用用户输入的查询进行语义相似性比较，与存储的文档内容进行对比，而且还能从用户查询中提取对存储文档元数据的过滤条件，并执行这些过滤器。（来自LangChain 官网： Self-querying | ️ Langchain ）

## 环境准备

```
!pip -q install langchain huggingface_hub openai google-search-results tiktoken chromadb lark
import os
os.environ["OPENAI_API_KEY"] = "";

from langchain.schema import Document
from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.vectorstores import Chroma

embeddings = OpenAIEmbeddings()
```

## 数据文件

下面直接用代码创建和初始化了一个文档集合，并将其传递给一个向量存储系统的例子。步解释代码的含义：

1. `Document` 类的实例被创建，每个实例代表一个文档，可能是一个葡萄酒的描述。

2. `page_content` 字段包含关于每种葡萄酒的描述性文本。

3. `metadata` 字段包含了关于葡萄酒的额外信息，比如：

   + `name` : 葡萄酒的名称。
   + `year` : 葡萄酒的年份。
   + `rating` : 葡萄酒的评分。
   + `grape` : 用于制酒的葡萄品种。
   + `color` : 葡萄酒的颜色。
   + `country` : 葡萄酒的产国。

这些 `Document` 实例被收集到一个名为 `docs` 的列表中。

接下来：

+ `vectorstore` 变量被赋值为 `Chroma.from_documents(docs, embeddings)` 的结果。这表明 `Chroma` 是一个类或模块，它有一个 `from_documents` 方法，该方法接收文档列表和可能的 `embeddings`（这里未显示 `embeddings` 的定义或初始化，但它可能是每个文档的向量表示）。

+ `Chroma.from_documents(docs, embeddings)` 这一行的作用是将文档集合和它们的嵌入向量（`embeddings`）传递给 `Chroma` 类来创建一个向量存储。这个向量存储可以用来执行各种任务，比如找出与给定查询文本最相关的文档，或者对葡萄酒进行分类和聚类。

这个过程通常是信息检索、推荐系统或自然语言处理任务的一部分，在这里特别关注于葡萄酒的描述和属性。

```
docs = [
    Document(
        page_content="Complex, layered, rich red with dark fruit flavors",
        metadata={"name": "Opus One", "year": 2018, "rating": 96, "grape": "Cabernet Sauvignon", "color": "red", "country": "USA"},
    ),
    Document(
        page_content="Luxurious, sweet wine with flavors of honey, apricot, and peach",
        metadata={"name": "Château d'Yquem", "year": 2015, "rating": 98, "grape": "Sémillon", "color": "white", "country": "France"},
    ),
    Document(
        page_content="Full-bodied red with notes of black fruit and spice",
        metadata={"name": "Penfolds Grange", "year": 2017, "rating": 97, "grape": "Shiraz", "color": "red", "country": "Australia"},
    ),
    Document(
        page_content="Elegant, balanced red with herbal and berry nuances",
        metadata={"name": "Sassicaia", "year": 2016, "rating": 95, "grape": "Cabernet Franc", "color": "red", "country": "Italy"},
    ),
    Document(
        page_content="Highly sought-after Pinot Noir with red fruit and earthy notes",
        metadata={"name": "Domaine de la Romanée-Conti", "year": 2018, "rating": 100, "grape": "Pinot Noir", "color": "red", "country": "France"},
    ),
    Document(
        page_content="Crisp white with tropical fruit and citrus flavors",
        metadata={"name": "Cloudy Bay", "year": 2021, "rating": 92, "grape": "Sauvignon Blanc", "color": "white", "country": "New Zealand"},
    ),
    Document(
        page_content="Rich, complex Champagne with notes of brioche and citrus",
        metadata={"name": "Krug Grande Cuvée", "year": 2010, "rating": 93, "grape": "Chardonnay blend", "color": "sparkling", "country": "New Zealand"},
    ),
    Document(
        page_content="Intense, dark fruit flavors with hints of chocolate",
        metadata={"name": "Caymus Special Selection", "year": 2018, "rating": 96, "grape": "Cabernet Sauvignon", "color": "red", "country": "USA"},
    ),
    Document(
        page_content="Exotic, aromatic white with stone fruit and floral notes",
        metadata={"name": "Jermann Vintage Tunina", "year": 2020, "rating": 91, "grape": "Sauvignon Blanc blend", "color": "white", "country": "Italy"},
    ),
]
vectorstore = Chroma.from_documents(docs, embeddings)
```

## 创建 self-querying retriever

```
# 导入OpenAI类，这是与OpenAI语言模型进行交互的接口
from langchain.llms import OpenAI
# 导入SelfQueryRetriever类，这是自查询检索器的基础类
from langchain.retrievers.self_query.base import SelfQueryRetriever
# 导入AttributeInfo类，用于构建和描述查询的属性信息
from langchain.chains.query_constructor.base import AttributeInfo

# 定义一个包含葡萄酒元数据字段信息的列表
metadata_field_info = [
    # 定义一个描述葡萄酒使用的葡萄品种的属性
    AttributeInfo(
        name="grape",  # 属性名称
        description="The grape used to make the wine",  # 属性描述
        type="string or list[string]",  # 属性类型，可以是字符串或字符串列表
    ),
    # 定义一个描述葡萄酒名称的属性
    AttributeInfo(
        name="name",  # 属性名称
        description="The name of the wine",  # 属性描述
        type="string or list[string]",  # 属性类型
    ),
    # 定义一个描述葡萄酒颜色的属性
    AttributeInfo(
        name="color",  # 属性名称
        description="The color of the wine",  # 属性描述
        type="string or list[string]",  # 属性类型
    ),
    # 定义一个描述葡萄酒生产年份的属性
    AttributeInfo(
        name="year",  # 属性名称
        description="The year the wine was released",  # 属性描述
        type="integer",  # 属性类型，整数
    ),
    # 定义一个描述葡萄酒产国的属性
    AttributeInfo(
        name="country",  # 属性名称
        description="The name of the country the wine comes from",  # 属性描述
        type="string",  # 属性类型，字符串
    ),
    # 定义一个描述葡萄酒罗伯特·帕克评分的属性（评分范围0-100）
    AttributeInfo(
        name="rating",  # 属性名称
        description="The Robert Parker rating for the wine 0-100",  # 属性描述
        type="integer"  # 属性类型，整数（此处也可以用浮点数表示评分，如需使用请将类型改为float）
    ),
]
# 定义一个字符串，用于描述葡萄酒的简短信息
document_content_description = "Brief description of the wine"
```

## 定义大模型+SelfQueryRetriever

```
# 使用温度参数为0初始化OpenAI语言模型，这意味着生成的回答将更加确定性和一致性
llm = OpenAI(temperature=0)

# 创建一个自查询检索器实例。
# 该检索器利用上面初始化的语言模型（llm）、向量存储（vectorstore）、
# 文档内容描述（document_content_description）
retriever = SelfQueryRetriever.from_llm(
    llm,
    vectorstore,
    document_content_description,
    metadata_field_info, # 元数据字段信息（metadata_field_info）进行信息检索。
    verbose=True # 参数verbose=True表示在执行过程中会输出详细信息，以便于调试和观察。
)
```

## 尝试检索



## This example only specifies a relevant query

```
retriever.get_relevant_documents("What are some red wines")

query=' ' filter=Comparison(comparator=<Comparator.EQ: 'eq'>, attribute='color', value='red') limit=None
[Document(page_content='Elegant, balanced red with herbal and berry nuances', metadata={'color': 'red', 'country': 'Italy', 'grape': 'Cabernet Franc', 'name': 'Sassicaia', 'rating': 95, 'year': 2016}),
 Document(page_content='Complex, layered, rich red with dark fruit flavors', metadata={'color': 'red', 'country': 'USA', 'grape': 'Cabernet Sauvignon', 'name': 'Opus One', 'rating': 96, 'year': 2018}),
 Document(page_content='Highly sought-after Pinot Noir with red fruit and earthy notes', metadata={'color': 'red', 'country': 'France', 'grape': 'Pinot Noir', 'name': 'Domaine de la Romanée-Conti', 'rating': 100, 'year': 2018}),
 Document(page_content='Intense, dark fruit flavors with hints of chocolate', metadata={'color': 'red', 'country': 'USA', 'grape': 'Cabernet Sauvignon', 'name': 'Caymus Special Selection', 'rating': 96, 'year': 2018})]
```

## Example 2

```
retriever.get_relevant_documents("I want a wine that has fruity nodes")
query='fruity notes' filter=None limit=None
[Document(page_content='Crisp white with tropical fruit and citrus flavors', metadata={'color': 'white', 'country': 'New Zealand', 'grape': 'Sauvignon Blanc', 'name': 'Cloudy Bay', 'rating': 92, 'year': 2021}),
 Document(page_content='Exotic, aromatic white with stone fruit and floral notes', metadata={'color': 'white', 'country': 'Italy', 'grape': 'Sauvignon Blanc blend', 'name': 'Jermann Vintage Tunina', 'rating': 91, 'year': 2020}),
 Document(page_content='Intense, dark fruit flavors with hints of chocolate', metadata={'color': 'red', 'country': 'USA', 'grape': 'Cabernet Sauvignon', 'name': 'Caymus Special Selection', 'rating': 96, 'year': 2018}),
 Document(page_content='Full-bodied red with notes of black fruit and spice', metadata={'color': 'red', 'country': 'Australia', 'grape': 'Shiraz', 'name': 'Penfolds Grange', 'rating': 97, 'year': 2017})]
```

## Example 3

### This example specifies a query and a filter

```
retriever.get_relevant_documents("I want a wine that has fruity nodes and has a rating above 97")
query='fruity' filter=Comparison(comparator=<Comparator.GT: 'gt'>, attribute='rating', value=97) limit=None
[Document(page_content='Luxurious, sweet wine with flavors of honey, apricot, and peach', metadata={'color': 'white', 'country': 'France', 'grape': 'Sémillon', 'name': "Château d'Yquem", 'rating': 98, 'year': 2015}),
 Document(page_content='Highly sought-after Pinot Noir with red fruit and earthy notes', metadata={'color': 'red', 'country': 'France', 'grape': 'Pinot Noir', 'name': 'Domaine de la Romanée-Conti', 'rating': 100, 'year': 2018})]
```

## Example 4

```
retriever.get_relevant_documents(
    "What wines come from Italy?"
)
query=' ' filter=Comparison(comparator=<Comparator.EQ: 'eq'>, attribute='country', value='Italy') limit=None
[Document(page_content='Elegant, balanced red with herbal and berry nuances', metadata={'color': 'red', 'country': 'Italy', 'grape': 'Cabernet Franc', 'name': 'Sassicaia', 'rating': 95, 'year': 2016}),
 Document(page_content='Exotic, aromatic white with stone fruit and floral notes', metadata={'color': 'white', 'country': 'Italy', 'grape': 'Sauvignon Blanc blend',
, 'name': 'Jermann Vintage Tunina', 'rating': 91, 'year': 2020})]
```

## This example specifies a query and composite filter

```
retriever.get_relevant_documents(
    "What's a wine after 2015 but before 2020 that's all earthy"
)

query='earthy' filter=Operation(operator=<Operator.AND: 'and'>, arguments=[Comparison(comparator=<Comparator.GT: 'gt'>, attribute='year', value=2015), Comparison(comparator=<Comparator.LT: 'lt'>, attribute='year', value=2020)]) limit=None
[Document(page_content='Elegant, balanced red with herbal and berry nuances', metadata={'color': 'red', 'country': 'Italy', 'grape': 'Cabernet Franc', 'name': 'Sassicaia', 'rating': 95, 'year': 2016}),
 Document(page_content='Highly sought-after Pinot Noir with red fruit and earthy notes', metadata={'color': 'red', 'country': 'France', 'grape': 'Pinot Noir', 'name': 'Domaine de la Romanée-Conti', 'rating': 100, 'year': 2018}),
 Document(page_content='Full-bodied red with notes of black fruit and spice', metadata={'color': 'red', 'country': 'Australia', 'grape': 'Shiraz', 'name': 'Penfolds Grange', 'rating': 97, 'year': 2017}),
 Document(page_content='Complex, layered, rich red with dark fruit flavors', metadata={'color': 'red', 'country': 'USA', 'grape': 'Cabernet Sauvignon', 'name': 'Opus One', 'rating': 96, 'year': 2018})]
```

## Filter K

我们也可以：

+   使用自查询检索器来指定 `k`：要获取的文档数量；

+   通过向构造函数传递 `enable_limit=True` 来实现这一点



```
retriever = SelfQueryRetriever.from_llm(
    llm,
    vectorstore,
    document_content_description,
    metadata_field_info,
    enable_limit=True,
    verbose=True,
)

# This example only specifies a relevant query - k= 2
retriever.get_relevant_documents(&quot;what are two that have a rating above 97&quot;)

query=&#x27; &#x27; filter=Comparison(comparator=&lt;Comparator.GT: &#x27;gt&#x27;&gt;, attribute=&#x27;rating&#x27;, value=97) limit=2
[Document(page_content=&#x27;Luxurious, sweet wine with flavors of honey, apricot, and peach&#x27;, metadata={&#x27;color&#x27;: &#x27;white&#x27;, &#x27;country&#x27;: &#x27;France&#x27;, &#x27;grape&#x27;: &#x27;Sémillon&#x27;, &#x27;name&#x27;: &quot;Château d&#x27;Yquem&quot;, &#x27;rating&#x27;: 98, &#x27;year&#x27;: 2015}),
 Document(page_content=&#x27;Highly sought-after Pinot Noir with red fruit and earthy notes&#x27;, metadata={&#x27;color&#x27;: &#x27;red&#x27;, &#x27;country&#x27;: &#x27;France&#x27;, &#x27;grape&#x27;: &#x27;Pinot Noir&#x27;, &#x27;name&#x27;: &#x27;Domaine de la Romanée-Conti&#x27;, &#x27;rating&#x27: 100, &#x27;year&#x27;: 2018})]

retriever.get_relevant_documents(&quot;what are two wines that come from australia or New zealand&quot;)
query=&#x27; &#x27; filter=Operation(operator=&lt;Operator.OR: &#x27;or&#x27;&gt;, arguments=[Comparison(comparator=&lt;Comparator.EQ: &#x27;eq&#x27;&gt;, attribute=&#x27;country&#x27;, value=&#x27;Australia&#x27;), Comparison(comparator=&lt;Comparator.EQ: &#x27;eq&#x27;&gt;, attribute=&#x27;country&#x27;, value=&#x27;New Zealand&#x27;)]) limit=2
[Document(page_content=&#x27;Crisp white with tropical fruit and citrus flavors&#x27;, metadata={&#x27;color&#x27;: &#x27;white&#x27;, &#x27;country&#x27;: &#x27;New Zealand&#x27;, &#x27;grape&#x27;: &#x27;Sauvignon Blanc&#x27;, &#x27;name&#x27;: &#x27;Cloudy Bay&#x27;, &#x27;rating&#x27;: 92, &#x27;year&#x27;: 2021}),
 Document(page_content=&#x27;Full-bodied red with notes of black fruit and spice&#x27;, metadata={&#x27;color&#x27;: &#x27;red&#x27;, &#x27;country&#x27;: &#x27;Australia&#x27;, &#x27;grape&#x27;: &#x27;Shiraz&#x27;, &#x27;name&#x27;: &#x27;Penfolds Grange&#x27;, &#x27;rating&#x27;: 97, &#x27;year&#x27;: 2017})]
```

参考： [`https://colab.research.google.com/drive/1MEVagsVRce15lzd_KNb86oAJmQx_WyZp?usp=sharing#scrollTo=P5ahYRXet4ka`](https://link.zhihu.com/?target=https%3A//colab.research.google.com/drive/1MEVagsVRce15lzd_KNb86oAJmQx_WyZp%3Fusp%3Dsharing%23scrollTo%3DP5ahYRXet4ka)

配套视频： [`https://www.youtube.com/watch?v=f4LeWlt3T8Y&t=398s`](https://link.zhihu.com/?target=https%3A//www.youtube.com/watch%3Fv%3Df4LeWlt3T8Y%26t%3D398s)
