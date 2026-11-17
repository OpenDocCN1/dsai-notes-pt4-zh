
![](img/283fcef76487afbb4a9086c4efbbb7a4.png)

```
{
        "db_id": "concert_singer",
        "query": "SELECT count(*) FROM singer",
        "query_toks": [
            "SELECT",
            "count",
            "(",
            "*",
            ")",
            "FROM",
            "singer"
        ],
        "query_toks_no_value": [
            "select",
            "count",
            "(",
            "*",
            ")",
            "from",
            "singer"
        ],
        "question": "How many singers do we have?",
        "question_toks": [
            "How",
            "many",
            "singers",
            "do",
            "we",
            "have",
            "?"
        ],
        "sql": {
            "from": {
                "table_units": [
                    [
                        "table_unit",
                        1
                    ]
                ],
                "conds": []
            },
            "select": [
                false,
                [
                    [
                        3,
                        [
                            0,
                            [
                                0,
                                0,
                                false
                            ],
                            null
                        ]
                    ]
                ]
            ],
            "where": [],
            "groupBy": [],
            "having": [],
            "orderBy": [],
            "limit": null,
            "intersect": null,
            "union": null,
            "except": null
        }
    }
```

`query_toks` 和 `question_toks` 都是经过分词和编码处理后的文本表示，主要用于将自然语言输入转换为模型可以处理的格式。可以做一下大模型预训练，分词训练等。

`query_toks_no_value` 更适合用于模型训练和解析，帮助模型理解查询的逻辑而不被具体值干扰，减少掉数值的概念。

`from`：表示 `SQL` 查询的 `FROM` 子句，即从哪些表中获取数据。

`table_units`：一个数组，包含了一个或多个表单元（`table_unit`）。

`["table_unit", 1]`：表示从表1中获取数据。对应 `.sql` 文件中就是表 `singer`

`conds`：一个数组，表示 `FROM` 子句中的条件。在这个例子中，`conds` 数组为空，表示没有额外的条件

`select`：表示 `SQL` 查询的 `SELECT` 子句，即要选择哪些列或表达式。

`false`：表示是否选择所有列（*）。`false` 表示不是选择所有列。

`[[3, [0, [0, 0, false], null]]]`：一个嵌套数组，表示选择的具体列或表达式。

`3`：表示选择的列或表达式的类型。具体类型需要根据上下文确定，常见的类型有：

`0`：列名

`1`：聚合函数（如 `COUNT`、`SUM` 等）

`2`：子查询

`3`：列的别名

`[0, [0, 0, false], null]`：表示具体的列或表达式。

`0`：表示列的类型，`0` 通常表示列名。

`[0, 0, false]`：表示列的具体信息，具体含义需要根据上下文确定。

`null`：表示没有别名。

`concert_singer`对应的数据库信息

![](img/e9cb06fcf81540634a6779cb2b435f99.png)

![](img/833575441da4ebe721a48bd29b207ed9.png)

```
SParC : https://drive.google.com/uc?export=download&id=1Uu7NMHTR1tdQw1t7bAuM7OPU4LElVKfg
    SParC，用于复杂、跨域、上下文相关（多轮）语义解析和Text-to-SQL任务，该数据集由4298个连贯的问题序列组成（有12k+个自然语言问题到SQL标注的Question-SQL对，由14名耶鲁大学学生标注），通过用户与138个领域的200个复杂数据库的交互获得。
```

```
CoSQL :https://yale-lily.github.io/cosql
    跨域数据库CoSQL，它由30k+轮次和10k+带注释的SQL查询组成。
```

```
KaggleDBQA：https://github.com/Chia-Hsuan-Lee/KaggleDBQA/tree/main?tab=readme-ov-file#Data-Format
    华盛顿大学与微软联合创建，是一个真是数据集它包括跨 8 个数据库的 272 个示例，每个数据库平均有 2.25 个表。 该数据集以其真实世界的数据源、自然的问题创作环境以及具有丰富领域知识的数据库文档而闻名。 主要统计数据：8.7% WHERE 子句、73.5% VAL、24.6% SELECT 和 6.8% NON-SELECT。
    
```

![](img/e6d9e3e54a7d845857bac3cc1bd7f3df.png)

![](img/18758b18ea9641075058050a03344ba0.png)

中文：

```
CHASE: https://github.com/xjtu-intsoft/chase/tree/page/data
    西安交通大学和微软等提出了首个跨领域、多轮Text-to-SQL中文数据集，包含了5459个多轮问题组成的列表，17940个<query, SQL>二元组。
```

清洗数据代码：

```
import json
import argparse

def convert_cspider_to_alpaca(input_file, output_file, num_samples=None):
    """将CSpider格式转换为Alpaca格式"""
    with open(input_file, 'r', encoding='utf-8') as f:
        cspider_data = json.load(f)
    
    # 限制条数
    if num_samples:
        cspider_data = cspider_data[:num_samples]
    
    alpaca_data = [
        {
            "instruction": "将下面的自然语言问题转换为SQL查询语句。",
            "input": item["question"],
            "output": item["query"]
        }
        for item in cspider_data
    ]
    
    with open(output_file, 'w', encoding='utf-8') as f:
        json.dump(alpaca_data, f, ensure_ascii=False, indent=2)
    
    print(f"转换完成：{len(alpaca_data)} 条数据 -> {output_file}")

def main():
    parser = argparse.ArgumentParser(description='CSpider转Alpaca格式')
    parser.add_argument('-i', '--input', required=True, help='输入文件路径')
    parser.add_argument('-o', '--output', required=True, help='输出文件路径')
    parser.add_argument('-n', '--num', type=int, default=None, help='转换条数限制（默认全部）')
    
    args = parser.parse_args()
    convert_cspider_to_alpaca(args.input, args.output, args.num)

if __name__ == "__main__":
    main()
```

# 9 技术路线选择

## 先明确：你要的不是“模型最强”，而是“能听懂你公司数据的人”

`NL2SQL` 是一个非常应用型任务，关键不是参数量，而是：

+   能理解你的自然语言（语义解析）；

+   能正确生成 `SQL`（结构化输出）；

+   能识别你公司内部表、字段、业务口径。

换句话说，`NL2SQL` 模型需要两类能力：

| 能力 | 来源 | 对应模型类型 |

| --- | --- | --- |

| 自然语言理解与指令跟随 | 由 `Instruct` 模型学得 | `Instruct` 系列 |

| 程序语法与结构化生成 | 由 `Coder` 模型学得 | `Coder` 系列 |

## 两类模型的区别与特征

| 对比项 | `Instruct` 模型（通用对话） | `Coder` 模型（代码/SQL专长） |

| --- | --- | --- |

| 训练目标 | 听懂指令、生成自然语言 | 理解语法结构、生成代码类文本 |

| 优势 | 对自然语言问题理解好，泛化能力强 | `SQL`、`Python`、`JSON` 结构生成精准 |

| 弱点 | `SQL` 语法可能不严谨，字段拼写错误多 | 对口语化自然语言理解较弱 |

| 适用场景 | 业务语言复杂、多语义提问 | `SQL` 逻辑复杂、要求语法严格 |

| 示例 | `Qwen2.5-7B-Instruct`、`Llama3-Instruct` | `DeepSeek-Coder-6.7B`、`StarCoder2`、`CodeQwen` |

一句话总结：

+   `Instruct` 模型更“懂人话”，但写 `SQL` 会错字段；

+   `Coder` 模型更“懂代码”，但听不太懂口语问题。

## 两阶段微调

先在公开 `NL2SQL` 上基础微调，再用公司数据精调

### 理论逻辑：两阶段微调是“迁移学习（Transfer Learning）”

1.  **第一阶段**：用公开领域的 `NL2SQL` 数据（如 `Spider`、`CSpider`、`WikiSQL`）

让模型学会通用 `SQL` 结构生成规律。

2.  **第二阶段**：用公司内部数据

让模型学会你公司的表结构、字段命名、业务口径。

### 两阶段微调的优势

| 优势 | 说明 |

| --- | --- |

| **学习基础语法** | 第一阶段让模型熟悉 `SQL` 语法树、聚合函数、`JOIN` 模式。 |

| **降低数据量要求** | 第二阶段只需几千条企业样本即可达到较高精度。 |

| **增强泛化能力** | 模型能同时理解通用查询和企业特定查询。 |

| **迁移更稳健** | 避免直接用公司数据“从零开始”导致欠拟合。 |

### 两阶段微调的潜在问题

| 问题 | 说明 |

| --- | --- |

| **时间与算力成本高** | 要做两次训练。 |

| **可能“遗忘”原始知识** | 如果第二阶段样本分布很窄，可能过拟合公司内部表达。 |

| **数据清洗要求更高** | 两阶段间语料结构要保持统一，否则模型混乱。 |

### 适用场景

+   大型公司（数据库复杂、行业术语多）

+   想做“长周期” `NL2SQL` 能力建设

+   拥有少量高质量企业内部语料

## 直接单阶段微调（公司数据直训）

### 做法

直接用企业内部自然语言 ↔ `SQL` 数据对进行 `LoRA` 微调。

### 优点

+   成本低（一次训练）

+   训练目标聚焦（模型只记你的 `Schema`）

+   快速上线验证（迭代周期短）

### 缺点

+   模型对复杂 `SQL` 结构的泛化差；

+   若内部数据覆盖面小，模型容易“偏科”；

+   不具备通用 `SQL` 生成能力（换一张表就不行）。

### 适用场景

+   中小公司，数据库表结构较简单；

+   只需在固定系统中做问答；

+   没有算力和时间做两阶段训练。

## 结合模型类型来选路线（`Instruct` vs `Coder`）

| 场景 | 推荐模型类型 | 推荐微调路线 |

| --- | --- | --- |

| 想让业务人员自然提问、模型理解口语好 | `Instruct` 模型（如 `Qwen2.5-Instruct`） | 先公开 `NL2SQL` 基础训练，再用公司数据精调（两阶段） |

| `SQL` 语法复杂、字段关联多、执行容错低 | `Coder` 模型（如 `DeepSeek-Coder` / `CodeQwen`） | 直接公司数据微调（单阶段 `LoRA`） |

| 数据量少、时间有限 | `Coder` 模型 + Few-shot Prompt | 不微调，直接提示结构化生成 |

| 想要兼顾两者 | 混合策略（`Instruct` 模型 + `LoRA` 训练时混入 `SQL` 任务） | `LoRA` 阶段强化 `SQL` 任务 |

## 两种技术方案概述

| 技术方案 | 简要流程 | 目标 |

| --- | --- | --- |

| 方案 A：`Instruct` 模型 + 两阶段微调 | ① 先用公开 `NL2SQL` 数据集进行基础微调（`Spider`、`CSpider`）  ② 再用公司内部数据进行第二轮定向精调 | 让模型同时具备通用 `SQL` 能力和企业语义理解能力 |

| 方案 B：`Coder` 模型 + 单阶段微调 | 直接使用 `Coder` 模型，用公司内部数据做一次 `LoRA`/`QLoRA` 微调 | 快速适配企业数据库结构，实现稳定 `SQL` 生成 |

## 方案 A：`Instruct` 模型 + 两阶段微调

### 训练逻辑

1.  **阶段一：通用 `NL2SQL` 微调**

+   使用公开数据集（`Spider`、`WikiSQL`、`CSpider`）

+   教模型掌握 `SQL` 语法与通用结构。

2.  **阶段二：企业内部数据精调**

+   使用公司自己的自然语言 ↔ `SQL` 数据集

+   让模型学会你公司的字段命名、口径规则、表结构。

### 优势

+   **自然语言理解强**：能处理模糊、上下文依赖的问题。

+   **泛化能力好**：能适应新问题或新增表结构。

+   **迁移学习稳定**：先通用、后专精，结构清晰。

+   **更贴近人机交互需求**：适合业务人员直接问问题。

### 缺点

+   训练周期长，需要两次微调。

+   成本高（显存与算力消耗大）。

+   数据准备工作量大。

+   参数设置需谨慎，否则第二阶段容易遗忘第一阶段的通用能力。

### 适用场景
