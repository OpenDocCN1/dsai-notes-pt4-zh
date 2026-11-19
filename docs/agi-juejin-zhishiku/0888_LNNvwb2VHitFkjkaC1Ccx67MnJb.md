
# 借助知识图谱和`Llama-Index`实现基于大模型的RAG

> 来源：[https://agijuejin.feishu.cn/docx/YiWWdIN0VojnqbxrG9jc3lkInSe](https://agijuejin.feishu.cn/docx/YiWWdIN0VojnqbxrG9jc3lkInSe)

> 作者：[爱吃牛油果的璐璐](https://www.zhihu.com/people/robot-2-59)

> 原文：https://zhuanlan.zhihu.com/p/675131151

## 背景

幻觉是在处理大型语言模型（LLMs）时常见的问题。LLMs生成流畅连贯的文本，但经常产生不准确或不一致的信息。防止LLMs中出现幻觉的一种方法是使用外部知识源，如提供事实信息的数据库或知识图谱。

矢量数据库和知识图谱使用不同的方法来存储和表示数据。矢量数据库适合基于相似性的操作，知识图谱旨在捕捉和分析复杂的关系和依赖关系。

对于LLM中的幻觉问题，知识图谱是一个比向量数据库更好的解决方案。知识图谱为LLM提供了更准确、相关、多样化、有趣、逻辑和一致的信息。因此，使用知识图谱可以减少LLM中的幻觉，使其在生成准确和真实的文本时更加可靠。但关键是文档需要清楚地展示关系，否则知识图谱将无法捕捉到它。

## 向量数据库

向量数据库是一组高维向量的集合，用于表示实体或概念，例如单词、短语或文档。向量数据库可以根据实体或概念的向量表示来度量它们之间的相似性或关联性。

举个例子，向量数据库可以告诉你“巴黎”和“法国”比“巴黎”和“德国”更相关，基于它们的向量距离。

## 知识图谱

知识图谱是一组节点和边，用于表示实体或概念以及它们之间的关系，例如事实、属性或类别。知识图谱可以根据节点和边的属性来查询或推断不同实体或概念的事实信息。

举个例子，知识图谱可以告诉你“巴黎”是“法国”的首都，基于它们的边标签。

## 知识图谱组件

顶点/节点：表示知识领域中的实体或对象。每个节点对应一个唯一的实体，并由唯一的标识符进行标识。例如，在关于Chennai Kings的知识图谱中，节点可以具有诸如“Philadelphia Phillies”和“Major League Cricket”这样的值。

边：表示两个节点之间的关系。例如，一条“compete in”的边可以将“Chennai Kings”节点连接到“Major League Cricket”节点。

## 知识图谱中的基本数据单元

三元组是图中的基本数据单元。它由三个部分组成：

主语：三元组所关于的节点。

宾语：关系指向的节点。

谓语：主语和宾语之间的关系。

在以下三元组示例中，“Chennai Kings”是主语，“compete in”是谓语，“Major League Cricket”是宾语。

`(Chennai Kings) — [compete in]->(Major League Cricket)`

知识图谱数据库可以通过存储三元组来高效地存储和查询复杂的图数据。

## 查询图数据库

查询涉及遍历图结构并根据特定标准检索节点、关系或模式。下面是一个简单的示例，展示了如何查询图数据库：假设你有一个代表社交网络的图数据库，其中用户是节点，而它们的关系（例如友谊）由连接节点的边表示。你想要找到给定用户的朋友圈（共同的连接）。

1. 从参考用户开始：在图数据库中，通过查询特定的用户标识符或其他相关标准来识别代表参考用户的节点。

2. 遍历图：使用图形查询语言（如`Neo4j`中使用的`Cypher`或`Gremlin`）从参考用户节点遍历图。编写一个查询，指定要探索的模式或关系。在这种情况下，您想要找到朋友的朋友。示例`Cypher`查询：`MATCH (:User {userId: 'referenceUser'})-[:FRIEND]->()-[:FRIEND]->(fof:User) RETURN fof`这个查询从参考用户开始，沿着`FRIEND`关系找到另一个节点（朋友），然后再沿着另一个`FRIEND`关系找到朋友的朋友（`fof`）。

3. 检索结果：在图数据库上执行查询，根据查询模式检索出相应的节点（朋友的朋友）。如果需要，还可以获取检索到的节点的特定属性或附加信息。

4. 呈现结果：将检索到的朋友的朋友显示给用户或按照需求进一步处理数据。这些信息可以用于建议、网络分析或其他相关目的。

图数据库提供了更高级查询功能，包括过滤、聚合和复杂模式匹配。具体的查询语言和语法可能有所不同，但总体过程涉及遍历图结构以检索与所需条件匹配的节点和关系。

## 查询向量数据库

通常涉及搜索相似向量或根据特定条件检索向量。以下是查询向量数据库的简单示例：假设你有一个包含客户配置文件的向量数据库，这些配置文件表示为高维向量，你想找到与给定参考客户相似的客户。

1. 定义参考客户向量：首先，为参考客户定义一个向量表示。这可以通过提取相关特征或属性并将它们转换为向量格式来完成。

2. 执行相似性搜索：使用合适的算法，如`k-NN`或余弦相似度，在向量数据库中执行相似性搜索。该算法将根据相似性分数识别参考客户向量的最近邻居。

3. 检索相似客户：检索与上一步中识别的最近邻居向量对应的客户配置文件。这些配置文件将根据定义的相似性度量表示与参考客户相似的客户。

4. 呈现结果：最后，将检索到的客户配置文件或相关信息呈现给用户，例如显示他们的名字、人口统计信息或购买历史。此信息可用于推荐、定向营销活动或个性化体验。

## 知识图谱的优势

相比于向量数据库，知识图谱提供了更精确和具体信息。向量数据库表示两个实体或概念之间的相似性或关联性，而知识图谱能够更好地理解它们之间的关系。例如，知识图谱可以告诉你“埃菲尔铁塔”是“巴黎”的地标，而向量数据库只能表示这两个概念的相似程度。这可以帮助LLM生成更准确和相关的文本。

知识图谱支持比向量数据库更多样化和复杂的查询。向量数据库主要基于向量距离、相似度或最近邻来回答问题，这些仅限于直接相似度测量。相比之下，知识图谱可以处理基于逻辑运算符（如“具有属性Z的所有实体是什么？”或“W和V的共同类别是什么？”）的查询。这可以帮助LLM生成更多样化和有趣的文本。

知识图谱比向量数据库能够进行更多的推理和推断。向量数据库只能提供直接存储在数据库中的信息。相比之下，知识图谱可以从实体或概念之间的关系推导出间接信息。例如，知识图谱可以根据“巴黎是法国的首都”和“法国位于欧洲”的事实推断出“埃菲尔铁塔位于欧洲”。这可以帮助LLM生成更符合逻辑和一致的文本。

## `LlamaIndex`

`LlamaIndex`是一个编排框架，用于简化将私有数据与公共数据集成以构建使用大型语言模型（LLMs）的应用程序。它提供了数据摄取、索引和查询的工具，使其成为生成式AI需求的一种多功能解决方案。

## 嵌入模型

嵌入模型需要将文本转换为所提供文本的信息的数字表示形式。该表示形式捕获了所嵌入内容的语义含义，使其适用于许多行业应用。在这里，我们使用了`thenlper/gte-large`模型。

## LLM

大型语言模型需要根据提供的问题和上下文生成响应。在这里，我们使用了`Zephyr 7B beta`模型。

## 代码实现

### 1、安装所有依赖库

```
pip install llama_index pyvis Ipython langchain pypdf
```

### 2、设置日志

```
import logging
import sys
logging.basicConfig(stream=sys.stdout, level=logging.INFO)
logging.getLogger().addHandler(logging.StreamHandler(stream=sys.stdout))
```

### 3、导包

```
from llama_index import (SimpleDirectoryReader,
                         LLMPredictor,
                         ServiceContext,
                         KnowledgeGraphIndex)
#
from llama_index.graph_stores import SimpleGraphStore
from llama_index.storage.storage_context import StorageContext
from llama_index.llms import HuggingFaceInferenceAPI
from langchain.embeddings import HuggingFaceInferenceAPIEmbeddings
from llama_index.embeddings import LangchainEmbedding
from pyvis.network import Network
```

> `SimpleDirectoryReader`：用于读取非结构化数据。

> `LLMPredictor`：用于使用大型语言模型（`LLM`）生成预测。

> `ServiceContext`：提供协调各种服务所需的上下文数据。

> `KnowledgeGraphIndex`：用于构建和操作知识图谱。

> `SimpleGraphStore`：用作存储图数据的简单仓库。

> `HuggingFaceInferenceAPI`：用于利用开源`LLM`的模块。

### 4、引入`LLM`

```
HF_TOKEN = "Your Huggaingface api key"
llm = HuggingFaceInferenceAPI(
    model_name="HuggingFaceH4/zephyr-7b-beta", token=HF_TOKEN
)
```

### 5、引入`embedding`

```
embed_model = LangchainEmbedding(
  HuggingFaceInferenceAPIEmbeddings(api_key=HF_TOKEN, model_name="thenlper/gte-large")
)
```

### 6、装载数据

```
documents = SimpleDirectoryReader("/content/Documents").load_data()
print(len(documents))
```

### 7、构建知识图谱索引

```
#setup the service context
service_context = ServiceContext.from_defaults(
    chunk_size=256,
    llm=llm,
    embed_model=embed_model
)
#setup the storage context
graph_store = SimpleGraphStore()
storage_context = StorageContext.from_defaults(graph_store=graph_store)
#Construct the Knowlege Graph Undex
index = KnowledgeGraphIndex.from_documents( documents=documents,
                                           max_triplets_per_chunk=3,
                                           service_context=service_context,
                                           storage_context=storage_context,
                                           include_embeddings=True)
```

### 8、从文档中创建知识图谱



# Employee Stock Option Plans (ESOP) Best Practices

## 文档元数据

- `page_label`: 2
- `file_path`: `/content/Documents/Employee-Stock-Option-Plans-ESOP-Best-Practices-2.pdf`

## What is an ESOP?

An Employee Stock Options Plan (ESOP) is an allocation of shares that will be granted to employees in the future in the form of stock options. It is also a plan for how these options will be distributed.

Key questions to consider:
- How much equity should we set aside for employees?
- How many shares will individual employees receive?
- What terms will govern these grants?

The plan is as important as the allocation!

### Triplets
- (`page_label`, is, `2`)
- (`ESOP`, is, What is an ESOP?)
- (`ESOP`, is, allocation)
- (`ESOP`, is, plan)
- (`ESOP`, is, important)

## Why Offer an ESOP?

### Triplets
- (`page_label`, is, `3`)
- (`ESOP`, is, Why Offer an ESOP?)

## Intro to Options Plans

### Part I

### Triplets
- (`page_label`, is, `3`)
- (`INTRO TO OPTIONS PLANS`, is, Part I)
- (`ESOP`, is, part of)
- (`ESOP`, is, Intro to Options Plans)
- (`Option`, is, part of)
- (`Option`, is, Intro to Options Plans)
- (`Lifecycle of a Startup ESOP`, is, part of)
- (`Lifecycle of a Startup ESOP`, is, Intro to Options Plans)
- (`Common Terms in an Options Package`, is, part of)
- (`Common Terms in an Options Package`, is, Intro to Options Plans)
- (`Why Issue Options to Employees`, is, part of)
- (`Why Issue Options to Employees`, is, Intro to Options Plans)
- (`A Defining Characteristic of Startup Culture`, is, part of)
- (`A Defining Characteristic of Startup Culture`, is, Why Issue Options to Employees)

## ESOP Definition and Characteristics

An ESOP is an equity compensation plan, also known as an employee stock option plan. It is a part of startup culture and a necessary part of capital structure. It is a defined characteristic of startup culture.

### Triplets
- (`ESOP`, is, equity compensation plan)
- (`ESOP`, is, employee stock option plan)
- (`ESOP`, is, part of startup culture)
- (`ESOP`, is, necessary part of capital structure)
- (`ESOP`, is, defined characteristic of startup culture)

## Why Issue Options to Employees

Issuing options to employees:
- attracts top recruits
- retains employees
- aligns incentives
- rewards value creation
- encourages long-term thinking

### Triplets
- (`Why Issue Options to Employees`, attracts, top recruits)
- (`Why Issue Options to Employees`, retains, employees)
- (`Why Issue Options to Employees`, aligns, incentives)
- (`Why Issue Options to Employees`, rewards, value creation)
- (`Why Issue Options to Employees`, encourages, long -term thinking)

## A Defining Characteristic of Startup Culture

The defining difference between Silicon Valley companies and almost every other industry in the U.S. is the virtually universal practice among tech companies of distributing meaningful equity (usually in the form of stock options) to ordinary employees.

Startups are a unique case. Unlike at larger corporations, employee ownership is an essential element of startup communities and culture:
- As high-risk/high-reward enterprises, startups use options to align employee compensation with the risk-prone mentality of the business.
- Startups seeking to achieve a "big exit" use options to align all employees to drive toward this desired outcome.

### Triplets
- (`Startups`, are, unique case)
- (`Startups`, use, options)
- (`Startups`, seeking to achieve, big exit)
- (`VCs`, require, ESOPs)
- (`Startups`, are compelled, to offer, options packages)
- (`ESOP`, is, necessary part of, capital structure)

## Executive Summary

This document outlines the best practices for designing and implementing an employee stock option plan (ESOP) for a private company. The document is intended to provide a framework for designing an ESOP that is tailored to the specific needs of the company and its employees. The document is not intended to be a comprehensive guide to ESOP design, but rather a high-level overview of the key considerations that should be taken into account when designing an ESOP.

### Triplets
- (`Executive Summary`, is, High-level overview)
- (`Executive Summary`, is, Key considerations)

## The ESOP as a Recruitment and Retention Tool

ESOPs are a critical tool for recruiting and retaining employees:
- In a competitive job market, ESOPs can be used to attract top talent.
- In a tight labor market, ESOPs can be used to retain key employees.
- In a volatile market, ESOPs can be used to retain employees during a downturn.
- In a high-growth company, ESOPs can be used to retain employees during a period of rapid expansion.
- In a company with a high employee turnover rate, ESOPs can be used to reduce turnover and associated costs.
- In a company with a high executive turnover rate, ESOPs can be used to retain key leadership.

### Triplets
- (`page_label`, is, `11`)
- (`stage`, is, pre-seed)
- (`stage`, is, seed)
- (`stage`, is, early -vc)

## Lifecycle of a Startup ESOP

### Triplets
- (`Lifecycle of a Startup ESOP`, involves, founders and early investors)
- (`Lifecycle of a Startup ESOP`, involves, management and the Board of Directors)
- (`Lifecycle of a Startup ESOP`, involves, employees)
- (`Employees`, receive, options packages)
- (`Options`, are, exercised)
- (`Options`, are, granted)
- (`Options`, are, issued)
- (`Options`, are, marketable)
- (`Options`, are, forfeitable)
- (`Options`, are, vested)
- (`Shares`, are, acquired)
- (`Shares`, are, granted)
- (`Shares`, are, marketable)
- (`Shares`, are, owned)
- (`Shares`, are, vested)
- (`Strike price`, is, paid)
- (`Strike price`, is, set)
- (`Strike price`, is, specified)

## Common Terms in an Options Package

### Triplets
- (`Common Terms in an Options Package`, is, Number of Shares)
- (`Common Terms in an Options Package`, is, Strike Price)
- (`Common Terms in an Options Package`, is, Vesting Schedule)
- (`Common Terms in an Options Package`, is, Cliff Period)
- (`Common Terms in an Options Package`, is, Expiration Date)

## How Should Options Be Granted?

- **At the Right Time**: Options should be granted at the right time in the company’s life cycle, when the company has achieved a certain level of success and the value of the company is clear.
- **At the Right Price**: Options should be granted at a fair price, based on the company’s valuation at the time of grant.
- **At the Right Size**: Options should be granted in the right size, based on the employee’s role and contribution to the company.

## How Much Equity Should You Grant?

### Stage Considerations

| Stage | Considerations | Takeaway |
|-------|---------------|----------|
| Pre-seed | Founders and early team considerations | ... |

### Triplets
- (`page_label`, is, `12`)
- (`file_path`, is, `/content/Documents/Employee-Stock-Option-Plans-ESOP-Best-Practices-2.pdf`)
- (`5%`, is, equivalent to)
- (`5%`, is, `$200,000` of company stock)

### Communication Considerations
- At an early stage, the only way to communicate options grants given no true valuation of the company.
- Hard for employees to grasp what they are really getting ("0.5% seems too little").
- Invites a negotiation about what percent of the company an employee really deserves.

Example grants:
- "We are granting you options equivalent to 0.5% of the company's equity"
- "We are granting you options equivalent to $200,000 of company stock"

### Triplets
- (`Communicating options to employees`, can be communicated, % versus $)
- (`Communicating options to employees`, can be communicated, %)
- (`Communicating options to employees`, can be communicated, $)
- (`Communicating options to employees`, can be communicated, 0.5%)
- (`Communicating options to employees`, can be communicated, $200,000)
- (`Communication of options packages`, recommended, dollar value)
- (`Communication of options packages`, recommended against, percent of the company)
- (`Communication of options packages`, effective when, dollar value)

## Exercise Price: Market Price vs. Discount

When setting the exercise price for options, there are two approaches: (1) set the price equal to the market price of the stock at the time of grant, or (2) set the price at a discount to the market price.

### Market Price
- Fair to employees and the company
- No tax deduction for company
- Employees can sell immediately
- Employees can exercise immediately

### Discount
or (2) a dollar value based on the current valuation; we encourage the latter when possible.

## Two Approaches

In reality, creating an ESOP will require a combination of top-down and bottom-up planning.

### Top-Down Process
Decide the total amount of equity to be granted; allocate these shares to employees over time.

### Bottom-Up Process
Decide the appropriate size of individual equity grants by position; issue these shares as employees are hired.

### Triplets
- (`Two Approaches`, is, reality)
- (`Creating an ESOP`, requires, a combination of top-down and bottom -up planning)
- (`Decide the total amount of equity to be granted`, is, part of creating an ESOP)
- (`Allocate these shares to employees over time`, is, part of creating an ESOP)
- (`Decide the appropriate size of individual equity grants by position`, is, part of creating an ESOP)
- (`Issue these shares as employees are hired`, is, part of creating an ESOP)

## Top-Down Process

1. Determine the total dollar amount to be spent on options
2. Determine the number of shares to be reserved for options
3. Determine the number of options to be granted
4. Determine the exercise price
5. Determine the vesting schedule

### Triplets
- (`Top-Down Process`, consists of, determining)
- (`Top-Down Process`, consists of, creating)
- (`Top-Down Process`, consists of, scheduling)
- (`Top-Down Process`, consists of, Determine the total dollar amount to be spent on options)
- (`Top-Down Process`, consists of, Determine the number of shares to be reserved for options)

## Bottom-Up Process

1. Determine the number of shares to be granted to each employee
2. Determine the grant date
3. Determine the vesting schedule

### Bottom-Up Process (continued)
1. Determine the exercise price
2. Determine the expiration date

### Triplets
- (`Bottom-Up Process`, consists of, determining)
- (`Bottom-Up Process`, consists of, Segment your human resources)
- (`Bottom-Up Process`, consists of, Create pay multipliers for each job function)
- (`Bottom-Up Process`, consists of, Determine the dollar value of an options grant)
- (`Bottom-Up Process`, consists of, Determine the current share price)
- (`Bottom-Up Process`, consists of, Calculate options grants)

## 3. Vesting Schedule

- Four year vesting schedule is most common.
- Cliff vesting is less common, but can be used to retain key employees.
- Monthly vesting is less common, but can be used to align employee interests with company performance.

### Triplets
- (`Vesting Schedule`, is most common)

## 2. Communicate Early and Often

- Communicate the plan to all employees as soon as possible.
- Communicate the plan to all new hires as part of their orientation.
- Communicate the plan to all employees at least annually.
- Communicate the plan to all employees when there is a material change.

### Triplets
- (`Communicate Early and Often`, is, communication)
- (`Communicate Early and Often`, has, Communicate the plan to all employees as soon as possible)
- (`Communicate Early and Often`, has, Communicate the plan to all new hires as part of their orientation)
- (`Communicate Early and Often`, has, Communicate the plan to all employees at least annually)

## 3. Establish Vesting Schedules

| Segment | Vesting Schedule |
|---------|-----------------|
| in Years or %? | ? |

### Triplets
- (`3`, establish, Vesting Schedules)
- (`Segment`, Vesting Schedule, ?)

## 4. Establish Performance Criteria

| Segment | Performance Criteria |
|---------|---------------------|
| in % or $? | ? |

### Triplets
- (`4`, establish, Performance Criteria)
- (`Segment`, Performance Criteria, ?)

## 3. Determine the Dollar Value of the Options Grant

| Employee | Base Salary | Options Multiplier | Dollar Value of Options Grant |
|----------|-------------|-------------------|-------------------------------|
| x | x | based on the employee’s role in HR segmentation schedule | Base Salary as negotiated with the employee at the time of hire or promotion |

### Triplets
- (`Employee`, Base Salary, x)
- (`Employee`, Options, x)
- (`Employee`, Options Multiplier, based on the employee’s role in HR segmentation schedule)

## HOW MUCH TO GRANT Part II

### Triplets
- (`HOW MUCH TO GRANT`, Part II, )

## ESOP Stage Guidance

### Triplets
- (`ESOP`, is necessary for, Pre-seed)
- (`ESOP`, is not necessary for, Pre-seed)
- (`ESOP`, can be helpful for, Pre-seed)
- (`ESOP`, is not necessary for, Seed)
- (`ESOP`, is required for, Early -VC)
- (`ESOP`, must be created for, Early -VC)
- (`ESOP`, serves as a guideline for, Early -VC)
- (`ESOP`, is important to have standardized for, Late-VC)
- (`ESOP`, is gone for, Growth)
- (`ESOP`, shares remaining are more valuable for, Growth)
- (`ESOP`, is used to allow new hires to share in the upside for, Growth)
- (`ESOP`, should be set up between, Pre-seed and, Early -VC)

## Typical Distribution Schedule

### Triplets
- (`Typical Distribution Schedule`, consists of, Seniority)
- (`Typical Distribution Schedule`, consists of, Equity Allocation)
- (`First`, consists of, 10%)
- (`Next`, consists of, 5%)
- (`Next`, consists of, 5%)
- (`CTO`, consists of, 1-5%)
- (`Key Developer or Engineer`, consists of, 1-2%)
- (`Other Functional Team Member`, consists of, 0.5-1.5%)
- (`No non -founding member of the senior team`, should not exceed, 10%)

## HR Segmentation and Pay Multipliers

### Triplets
- (`Segment Your Human Resources`, is, segmentation)
- (`Segment Your Human Resources`, has, Segment Roles)
- (`Segment Your Human Resources`, has, Award)
- (`Segment`, is, Roles)
- (`Segment`, is, Award)
- (`Segment`, is, Options Multiplier)
- (`Segment`, is, Founders)
- (`Segment`, is, Co-Founders)
- (`Segment`, is, %)
- (`Segment`, is, Founding Team)
- (`Segment`, is, Technical & Product)
- (`Segment`, is, Early -Stage Hires)
- (`Segment`, is, Senior Team)
- (`Segment`, is, C-Level)
- (`Segment`, is, Directors / VPs)
- (`Segment`, is, Functional / Technical Team)
- (`Segment`, is, Support Team)
- (`Segment`, is, $)
- (`Segment`, is, 0.5x)
- (`Segment`, is, 1.0x)
- (`Segment`, is, 0.25x)
- (`Segment`, is, 0.1x)
- (`Segment`, is, 0.0x)

## 2. Establish Pay Multipliers for Each Role

### Triplets
- (`2`, establish, Pay Multipliers for Each Role)
- (`Segment`, Roles, 2)
- (`Segment`, Award, ?)

## Option Characteristics

### Triplets
- (`Option`, is, effective form of equity ownership)
- (`Option`, has, intrinsic value)
- (`Option`, is, locked-in price for shares)
- (`Option`, is, right to purchase)
- (`Option`, is, set price)
- (`Option`, is, certain period of time)

## ESOP as a Retirement Plan

An ESOP can be described as:
- A qualified retirement plan
- A tax-qualified defined contribution plan
- A plan that is primarily designed to provide retirement benefits to employees
- A plan that is primarily designed to provide benefits in the form of stock
- A tax-qualified plan that meets ERISA requirements
- A plan that is primarily designed to provide benefits in the form of employer securities
- A plan that is primarily designed to provide benefits in the form of employer securities that are acquired through purchase, receipt from a corporate parent, or through the accumulation of distributions on employer securities

### Triplets
- (`page_label`, is, `1`)
- (`file_path`, is, `/content/Documents/Employee-Stock-Option-Plans-ESOP-Best-Practices-2.pdf`)

## 附录：其他提取内容

### University of Arkansas 2018 Annual Report
The 2018 Annual Report of the Board of Trustees of the University of Arkansas System is presented here as a PDF file.

### Triplets
- (`The`, is, PDF file)
- (`The`, is presented, here)
- (`The`, is, The 2018 Annual Report of the Board of Trustees of the University of Arkansas System)

### 2018 FIFA World Cup
The 2018 FIFA World Cup was the 21st FIFA World Cup, an international men's football tournament contested by the national teams of FIFA member associations. It took place in Russia from 14 June to 15 July 2018. It was the first World Cup to be held in Eastern Europe, and the 11th time that it had been held in Europe. At an estimated cost of over $14.2 billion, it is the most expensive World Cup.

### Triplets
- (`2018 FIFA World Cup`, is, 21st FIFA World Cup)
- (`2018 FIFA World Cup`, took place in, Russia)

### 2018-19 DOE Budget
The 2018-19 budget for the Department of Energy (DOE) includes $3.7 billion for the Office of Science (SC), an increase of $645 million (21.1%) over the 2018 enacted level.

### Triplets
- (`The`, 2018-19 budget for the Department of Energy, includes, $3.7 billion for the Office of Science)
- (`The`, 2018-19 budget for the Department of Energy, is, an increase of $645 million over the 2018 enacted level)
- (`The`, 2018-19 budget for the Department of Energy, represents, a 21.1% increase over the 2018 enacted level for the Office of Science)

### 2018-19 NBA Season
The 2018-19 season of the National Basketball Association (NBA) is the 72nd season of the National Basketball Association.




### 9、构建查询引擎来查询知识图谱

```
query = "What is ESOP?"
query_engine = index.as_query_engine(include_text=True,
                                     response_mode = "tree_summarize",
                                     embedding_mode = "hybrid",
                                     similarity_top_k=5,)
#
message_template = f"""<|system|>Please check if the following pieces of context has any mention of the  keywords provided in the Question.If not then don't know the answer, just say that you don't know.Stop there.Please donot try to make up an answer.</s>
<|user|>
Question: {query}
Helpful Answer:
</s>"""
#
response = query_engine.query(message_template)
#
print(response.response.split("<|assistant|>")[-1].strip())
#####OUTPUT #####################
ESOP stands for Employee Stock Ownership Plan. It is a retirement plan that allows employees to receive company stock or stock options as part of their compensation. In simpler terms, it is a plan that allows employees to own a portion of the company they work for. This can be a motivating factor for employees as they have a direct stake in the company's success. ESOPs can also be a tax-efficient way for companies to provide retirement benefits to their employees.


query = "Who is Atul?"
message_template = f"""<|system|>Please check if the following pieces of context has any mention of the  keywords provided in the Question.If not then don't know the answer, just say that you don't know.Stop there.Please donot try to make up an answer.</s>
<|user|>
Question: {query}
Helpful Answer:
</s>"""
#
response = query_engine.query(message_template)
#
print(response.response.split("<|assistant|>")[-1].strip())

################OUTPUT#####################
I do not have prior knowledge or context about who atul is. Please provide more information or context so I can assist you better. Without any further context, I do not know the answer to your question.
```

### 10、可视化表示

```
from pyvis.network import Network
from IPython.display import display
g = index.get_networkx_graph()
net = Network(notebook=True,cdn_resources="in_line",directed=True)
net.from_nx(g)
net.show("graph.html")
net.save_graph("Knowledge_graph.html")
import IPython
IPython.display.HTML(filename="/content/Knowledge_graph.html")
```

### 11、持久化保存

```
storage_context.persist()
```

## 总结

对于语言模型（LLM）幻觉，知识图谱被证明优于向量数据库。知识图谱提供更准确、多样化、有趣、逻辑和一致的信息，减少了LLM中出现幻觉的可能性。这种优越性源于它们能够提供关于实体之间关系的精确细节，而不仅仅是表示相似性，支持更复杂的查询和逻辑推理。

向量数据库以其数字数组在相似性搜索和最佳匹配场景中表现出色。最终，两者之间的选择取决于项目的需求，包括数据的性质、关系探索的需要以及预期的查询类型。
