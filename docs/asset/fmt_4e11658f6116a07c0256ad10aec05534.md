
# RAG2SQL工具介绍，效果比Text2SQL更好？

> 来源：`https://agijuejin.feishu.cn/docx/B1tqdYiiIoPh2wxJlPAc2JUtnGg`
>
> 作者：王大锤
>
> 原文：`https://zhuanlan.zhihu.com/p/701381673`

## 一、背景

SQL（Structured Query Language）是数据库查询和操作的标准语言，它允许用户检索、更新和管理存储在数据库中的数据。然而，编写高效且精确的SQL语句往往需要对数据库结构和SQL语法有深入的理解，这对初学者来说可能是一个挑战。

随着人工智能技术的发展，Text2SQL技术应运而生。这项技术通过自然语言处理（NLP）将用户的自然语言查询转换为SQL语句。用户只需以日常语言描述他们的需求，Text2SQL系统就能理解这些需求并自动生成相应的SQL查询。这极大地简化了数据库查询的过程，使得非技术用户也能够轻松地与数据库交互。

目前，Text2SQL技术已经被广泛应用于多种产品中，包括智能助手、数据分析工具和商业智能软件。这些产品利用Text2SQL技术，为用户提供了一种直观且强大的数据库查询解决方案。通过这种方式，用户可以更加专注于数据分析和决策制定，而不必花费大量时间学习复杂的 SQL 语言。

Text2SQL技术的进步不仅提高了数据库操作的效率，还推动了数据库技术的普及。随着技术的不断进步，我们可以预见，未来会有更多创新的解决方案出现，进一步降低数据库操作的门槛，让更多人能够轻松地利用数据库技术。

`Vanna`是一个令人兴奋的开源项目，它在Text2SQL技术的基础上进行了显著的升级。通过集成Retrieval-Augmented Generation（RAG）技术，`Vanna`不仅能够理解用户的自然语言查询，还能够从数据库中检索相关信息，以生成更加精确的SQL语句。RAG技术的应用显著提高了SQL生成的准确度和智能性，使得`Vanna`在处理复杂查询时更为出色。

`Vanna`的设计理念是让数据库查询变得更加简单和直观。无论是经验丰富的开发者还是初学者，都能够通过`Vanna`轻松地与数据库进行交互。`Vanna`的开源特性也意味着它能够不断接收社区的贡献，持续优化和改进，以满足不断变化的技术需求和用户期望。

![](img/c30e8ce0e2eed9c5b4d861d5ff477467.png)

---

## 二、Vanna

### 2.1 Vanna 介绍

`Vanna` 是一个开源的基于大语言模型和 RAG 的 Text2SQL 工具，`Vanna` 通过结合RAG 框架、大语言模型、高质量的训练数据、持续优化能力、广泛的数据库支持以及开源定制化的特点，实现了在复杂数据集上的高精度表现。使用 `Vanna` 可以使用任何的大模型、任何的向量数据库、任何的前端，和任何的关系数据库 交互。

![](img/e49bc23bfdbbc2387a7db407e5f1dc77.png)

### 2.2 算法流程图

如图所示，`Vanna` 的业务流程主要包括以下几个部分：

![](img/fedebb21452f74b9aafd6806d0448fc0.png)

首先是知识库的建立，知识库中主要包括三类数据信息，第一类是数据库中 `schema` 的定义，主要描述数据库中定义了哪些表，每张表的结构是什么样的等等；第二类数据是关于数据库中表的描述说明文档；第三类是线上环境预测结果的反馈信息，即预测结果符合预期的正确答案会被采集至知识库中。

线上的推理部分，主要有向量库的检索、`prompt` 的组成、大模型推理生成SQL和数据结果的生成几个部分组成。

其中向量检索、`prompt` 组成和大模型推理部分是RAG算法的标准化流程，后续的数据结果生成为Agent的动作执行模块。

![](img/cb358194f55c6fe57459a3001ea83335.png)

### 2.3 应用模式

`Vanna`是一个创新的开源项目，它通过提供三种模式，旨在满足不同用户的需求和预算。这三种模式分别是开源模式、免费模式和付费模式，每种模式都具有其独特的优势和特点。

在开源模式下，`Vanna`允许用户自由地集成他们所偏爱的任何大型语言模型。这种模式非常适合那些希望实现高度个性化定制的技术爱好者和开发者。用户可以根据自己的需求和偏好，选择不同的模型来构建和优化他们的数据查询解决方案。这种灵活性就像是在玩积木，用户可以随心所欲地搭建自己的数据世界。

免费模式是`Vanna`为那些预算有限或刚开始接触数据库查询的用户准备的。在这种模式下，`Vanna`提供了有限额度的大型模型使用和免费的存储空间。用户可以利用 GPT 3.5 的强大功能，无需支付任何费用即可开始他们的数据探索之旅。这为用户节省了成本，同时也提供了一个学习和实践的平台。

对于那些需要更强大的功能和更高灵活性的用户，`Vanna`提供了付费模式。在这种模式下，用户将获得无限制的使用权限，包括对最新 GPT-4 的支持以及优质的服务保障。付费模式适合那些对数据库查询有高要求的企业或专业用户。通过付费模式，用户可以获得更高级的功能，如更复杂的查询处理、更大规模的数据管理以及更专业的技术支持。

总的来说，`Vanna`通过提供这三种灵活的模式，能够满足从初学者到专业人士的不同需求。无论是寻求个性化定制、免费学习平台还是高级功能和专业服务，`Vanna`都能提供相应的解决方案。这种灵活性和可扩展性使得`Vanna`成为一个强大的工具，能够帮助用户更有效地与数据库进行交互，实现他们的数据查询和分析目标。

![](img/6a5bd7b7fad6836caec3dac3b11affcb.png)

## 三、使用实践

如果只是渴望迅速探索`Vanna`的强大功能，推荐通过Colab笔记本来领略`Vanna`的非凡魅力。这种方式不仅方便易行，还能直观感受到`Vanna`带来的创新体验。

![](img/7ad64023b13db5282be626dc3878ecbb.png)

要使用 `Vanna`，您需要先注册一个 `Vanna` 账号（`https://vanna.ai/`），以便获取必要的 API 密钥。

![](img/7d87eb480892313f7fffec404d7f8470.png)

实例化底座大模型和向量库：

```
# The import statement will vary depending on your LLM and vector database. This is an example for OpenAI + ChromaDB

from vanna.openai.openai_chat import OpenAI_Chat
from vanna.chromadb.chromadb_vector import ChromaDB_VectorStore

class MyVanna(ChromaDB_VectorStore, OpenAI_Chat):
    def __init__(self, config=None):
        ChromaDB_VectorStore.__init__(self, config=config)
        OpenAI_Chat.__init__(self, config=config)

vn = MyVanna(config={'api_key': 'sk-...', 'model': 'gpt-4-...'})

# See the documentation for other options
```

调用方式：

```
vn.ask("What are the top 10 customers by sales?")
```

返回结果：

```
SELECT c.c_name as customer_name,
        sum(l.l_extendedprice * (1 - l.l_discount)) as total_sales
FROM   snowflake_sample_data.tpch_sf1.lineitem l join snowflake_sample_data.tpch_sf1.orders o
        ON l.l_orderkey = o.o_orderkey join snowflake_sample_data.tpch_sf1.customer c
        ON o.o_custkey = c.c_custkey
GROUP BY customer_name
ORDER BY total_sales desc limit 10;
```

![](img/575ce046ebd61b7e65ab87819b69a9fb.png)

根据结果绘制可视化图表：

![](img/881d0347fb9cc1d6b3912581de9d6a59.png)
