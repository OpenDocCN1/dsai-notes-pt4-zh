
# 数据分析智能体Agent

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/PRBBdpGnko7EHGxA00tcVgxZnbd](https://qqs7y1hozd1.feishu.cn/docx/PRBBdpGnko7EHGxA00tcVgxZnbd)

## AI实战丨数据分析Agent -- DataAgent

### 一、整体架构

从架构上，`DataAgent` 是一款基于 `Agent` 构建的、面向企业级复杂场景的“虚拟 AI 数据分析师”。

![](img/426f4a844b382379d21c96f906c5b5e0.png)

> 通过 `Spring AI Alibaba Graph & Agent Framework` 构建了一套具备自我规划、工具调用、反思纠错及人类干预能力的数据智能体（`Agent`）

### 二、项目实现

#### 2.1 反馈机制 (Human-In-The-Loop)

+ **遇到问题：**一个错误的执行计划可能瞬间拖垮生产库，甚至“一步错步步错”。

![](img/5adf6cc5948294c22f515cb753d2a87d.png)

+ **解决方案：**

+ 数据字段：`agent.human_review_enabled` 用于保存配置，运行时以请求参数为准。

+ 图编排：`PlanExecutorNode` 检测 `HUMAN_REVIEW_ENABLED`，转入 `umanFeedbackNode`。

![](img/6fcba97d0458ddf463aebefcf292f3d6.png)

+ **反馈结果：**同意、修改或驳回，让 AI 既有速度，又懂规矩。

#### 2.2 Prompt 动态配置与自动优化

+ **遇到问题：**修改一句 Prompt 就要重启系统？不同模型对 Prompt 脾气不同，一套模板走天下根本行不通。

![](img/212ff4ddac7a674fc28b4df3dfa749bc.png)

+ **解决方案：**

+ 配置入口：`/api/prompt-config/*`，数据表 `user_prompt_config`。

+ 作用范围：支持按 `agentId` 绑定或全局配置（`agentId` 为空）。

+ Prompt 类型：`report-generator`、`planner`、`sql-generator`、`python-generator`、`rewrite`。

+ 自动优化方式：`ReportGeneratorNode` 拉取启用配置（按 `priority` 与 `display_order` 排序），

+ **获得效果： **像配置 Excel 一样调优 AI。运维人员无需重启，即可让 DataAgent 瞬间从“菜鸟分析师”变身“首席架构师”。

![](img/881ccadcdd9206d2fa521951402c0a22.png)

#### 2.3 深度 RAG 与混合检索增强

**遇到问题：**

+ 纯向量检索常召回一堆废话？

+ AI 不认识你的业务缩写？

+ 表结构太复杂，AI 搜不到。

+ **解决方案：**

+ 查询重写：`EvidenceRecallNode` 将多轮上下文与用户问题组装为检索指令，调用 LLM 生成 `standaloneQuery`，避免上下文遗漏与歧义。

+ 召回通道：`AgentVectorStoreService` 作为统一入口，默认走向量检索；开启混合检索后走 `AbstractHybridRetrievalStrategy`，将“向量召回 + 关键词召回”进行融合。（用户需要提供混合检索实现。当前默认只支持es）

+ 召回过滤：`DynamicFilterService` 生成基于智能体与知识类型的过滤条件，限制检索范围，避免跨智能体串库。

![](img/23bd965e421f1e23b89499d497aa8786.png)

+ 文档类型：业务知识（`business_knowledge`）+ 智能体知识（`agent_knowledge`）两类，按 `agentId`/`type` 元数据过滤后合并为 `evidence`，注入后续 prompt。

+ 关键配置：`spring.ai.alibaba.data-agent.vector-store.enable-hybrid-search` 控制是否开启混合检索；相似度阈值与 TopK 通过向量库配置项控制（如 `top-k`、`similarity-threshold`）。

+ 输出形式：`evidence` 文档以标题/摘要/片段形式汇总，作为 `EvidenceRecallNode` 输出内容进入后续规划于 SQL 生成阶段。

+ **获得效果：** AI 拥有了老员工的“直觉”。它能秒懂你的业务逻辑，即便表名全是乱码，它也能精准命中。

![](img/a4cbfa597f87ba44dca0ea40b407d44f.png)

#### 2.4 容器化 Python 执行引擎

+ **遇到问题：**SQL 只能算数，不能预测。想看趋势图、做线性回归？SQL 此时显得苍白无力。

![](img/0ce2fe5ce1da34a33041de98ba574ede.png)

+ **解决方案：**

+ 代码生成：`PythonGenerateNode` 根据计划与 SQL 结果生成 Python。

+ 代码执行：`PythonExecuteNode` 使用 `CodePoolExecutorService`（`Docker`/`Local`/AI 模拟）。

+ 执行配置：`spring.ai.alibaba.data-agent.code-executor.*`（默认 `Docker` 镜像 `continuumio/anaconda3:latest`）。

+ 结果回传：执行结果写回 `PYTHON_EXECUTE_NODE_OUTPUT`，`PythonAnalyzeNode` 汇总后写入用于最终报告。

+ **获得效果：** 赋予 AI 科学家级的建模能力。不仅能提取数据，还能输出带图表、带算法、带深度预测的高质量产出。

![](img/2775798a54fd655dfede5c8e295cfbde.png)

#### 2.5 流式输出 (SSE) 与多轮对话管理

+ **遇到问题：**分析任务耗时太长，用户盯着屏幕转圈

![](img/92f0db5cb39359b2296e28337f931c68.png)

+ **解决方案：**

+ 流式输出：`GraphController` SSE + `GraphServiceImpl` 流式处理。

+ 多轮对话：`MultiTurnContextManager` 记录“用户问题+规划结果”，注入到后续请求。

+ **获得效果：**极致的交互快感！让用户亲眼看到 AI 正在如何“思考”与“推演”，每一秒都有获得感。

![](img/784f1b91e2f9ad7085e5ce3ab3c086eb.png)

#### 2.6 MCP 服务器发布与多模型调度

+ **遇到问题：** DataAgent 虽好，但只能在自研系统用？想集成到 Claude 或 IDE？适配成本高到吓人。

![](img/7007d5b539bb0b44af6fdcf9101b1641.png)

+ **解决方案：**

+ MCP：`McpServerService` 提供 NL2SQL 与 Agent 列表工具，使用 `Mcp Server Boot Starter`。

+ 多模型调度：`ModelConfig` 配置模型，`AiModelRegistry` 缓存当前 `Chat`/`Embedding` 模型并支持热切换（同一时间每类仅一个激活模型）。

+ 已内置工具：`nl2SqlToolCallback`、`listAgentsToolCallback`。

![](img/195b6c6e91b71fcfcf8037e5b3ecc7f8.png)

#### 2.7 多数据源接入

+ **遇到问题：**企业数据散落在 `MySQL`、`PostgreSQL` 等各类库中，跨库取数像是在做“情报搜集”，配置繁琐且标准不一。

![](img/9a2b48c44cf7bdaab17f16adbe6fa13c.png)

**解决方案：**

+ 元数据存储：数据源配置写入 `datasource`，智能体绑定写入 `agent_datasource`，选表写入 `agent_datasource_tables`，逻辑外键写入 `logical_relation`。

+ Schema 初始化：`AgentDatasourceController` 触发初始化，`SchemaService` 通过 `AccessorFactory` 拉取表/列/外键并写入向量库。

+ 约束：同一智能体同一时间仅允许启用一个数据源（`AgentDatasourceService.toggleDatasourceForAgent`）。

+ **获得效果：**一个智能体，纵览全司数据！无论数据在哪儿，DataAgent 都能精准“路由”。它是数据孤岛的终结者，让跨库分析像查询单表一样简单。

![](img/e6faed2af2c3a14b36b866319d9f37c4.png)

#### 2.8 报告生成与摘要建议

+ **遇到问题：** 查出来一堆数字有什么用？领导要的是洞察，是结论，是能直接发在群里的 HTML 报告。

+ **解决方案：**

+ 报告节点：`ReportGeneratorNode` 读取计划、SQL/Python 结果与摘要建议（`summary_and_recommendations`）。

+ 输出格式：默认 HTML，`plainReport=true` 输出 Markdown（简洁报告）。

+ 优化提示词：自动拼接优化配置后生成报告。

+ **获得效果：**把分析师的一天缩短为 10 秒。从查数到成稿，DataAgent 承包了所有体力活，让你只负责最后的一锤定音。

![](img/98ecc79eaeedff2ebc6ae066493a286b.png)

#### 2.9 NL2SQL 转换, 语义模型，逻辑外键引擎

+ **遇到问题：** 纯大模型写 SQL 经常“盲目自信”，不是字段写错，就是不懂业务术语。语法错误导致的执行中断更是家常便饭。

+ **解决方案：**

+ **语义模型层：**通过管理端定义的术语映射规则，在生成阶段强制约束。

+ **两阶段校验：**`SqlGenerateNode` 生成后接 `SemanticConsistencyNode` 检查语义一致性。

+ **自愈循环：**`SqlExecuteNode` 捕获执行错误并反馈给 Graph 状态机，触发重定向至重写节点进行纠错。

+ **逻辑外键：**写入外部业务逻辑，不写入业务数据库。增强对表的理解能力。

#### 2.10 API Key 与权限管理

+ **遇到问题：**接口裸奔？权限失控？想对外开放能力却怕费用爆炸或数据泄露。

![](img/b05a1459f09d2e54294533ae1bb44d74.png)

+ **解决方案:**

+ 数据字段：`agent.api_key` 与 `agent.api_key_enabled`。

+ 调用方式：请求头 `X-API-Key`。

**获得效果：**

+ 生产级安全防护。

+ 让 DataAgent 不仅是业务利器，

+ 更是安全可控的企业级数字资产。

![](img/3d12087eec20292a2aabbd9f1fa2788c.png)

> **在企业数据分析的复杂生态中，指标作为衡量业务表现的关键标尺，其语义的清晰界定与统一理解至关重要。然而，现实情况却常常不尽如人意，不同部门、不同业务场景下，同一指标名称可能蕴含着截然不同的计算逻辑与含义。**

![](img/ccee5fde0a05d58e36c19509ab389ba2.png)

指标语义层的横空出世，恰如一座明亮的灯塔，驱散了这片语义迷雾，为 Agent 与数据使用者之间搭建起畅通无阻的沟通桥梁。它通过建立一套统一、规范的指标定义体系，确保无论在何种业务语境下，相同指标名都对应着唯一、精确的计算口径与业务含义。

> **不妨**对比一下有无指标语义层时的场景差异。在没有指标语义层的情况下，当业务人员向 Agent 提出 “查询本月各地区的销售额” 这样看似简单的需求时，Agent 可能会陷入迷茫。

> 因为它无法确定业务人员所指的 “销售额” 究竟遵循哪种计算逻辑，是电商部门的净销售额，还是财务部门的总销售额，亦或是其他特定业务规则下的定义。这可能导致 Agent 返回错误的数据结果，或者因无法理解需求而无法给出有效回应，使得业务人员不得不花费额外的时间与精力去解释、澄清需求，严重影响工作效率。

而当指标语义层就位后，情况则截然不同。指标语义层就像一本详尽的词典，将每个指标的名称、定义、计算方式、数据来源等关键信息进行了标准化封装。Agent 接收到业务人员的查询指令后，能够迅速在指标语义层中查找 “销售额” 的精准定义，明确其对应的计算规则与数据来源。它可以直接关联到相应的底层数据表与计算逻辑，快速执行准确的数据查询与分析任务，为业务人员提供符合其预期的、可靠的数据分析结果，如一张清晰呈现本月各地区按照统一标准计算的销售额对比图表，助力业务决策的高效制定。

![](img/cd54d6a11c1b4b9dbbe291feeac46963.png)

## 搭建物流智能体的技术流

### （一）数据层

+ **数据收集**：从多个数据源收集物流相关数据，包括订单数据（客户信息、货物信息、配送地址等）、运输数据（车辆信息、行驶轨迹、运输时间等）、仓储数据（库存水平、货物出入库记录等）、地理信息数据（地图、路况等）。

+ **数据存储**：选择合适的数据库来存储收集到的数据，如关系型数据库（`MySQL`、`PostgreSQL`）用于存储结构化数据，非关系型数据库（`MongoDB`、`Redis`）用于存储半结构化或非结构化数据。

### （二）算法层

+ **路径规划算法**：用于计算最优配送路线，常见算法有 `Dijkstra` 算法、`A*` 算法、遗传算法、蚁群算法等。

+ **预测算法**：对物流需求、库存水平等进行预测，可使用时间序列分析（`ARIMA`、`LSTM` 等）、回归分析等方法。

+ **强化学习算法**：在物流资源分配和调度中应用，通过智能体与环境的交互学习最优决策策略，如 `Q-learning`、深度Q网络（`DQN`）等。

### （三）模型层

+ **构建和训练模型**：根据具体任务选择合适的算法构建模型，并使用收集到的数据进行训练和优化。

+ **模型评估和验证**：使用测试数据集评估模型的性能，如准确率、召回率、F1值等指标，根据评估结果调整模型参数和结构。

### （四）应用层

+ **开发智能体功能模块**：实现订单管理、路径规划、库存管理、货物跟踪等功能模块，为用户提供具体的服务和操作界面。

+ **集成和部署**：将智能体系统与企业现有的物流管理系统、仓储系统等进行集成，并部署到生产环境中运行。

![](img/ea3c31fc8822b51936b731c57f8a70b1.png)

## 代码编写

### 路径规划代码（使用Python和NetworkX库实现Dijkstra算法来计算最短路径）

```
import networkx as nx

# 创建图对象
G = nx.Graph()

# 添加节点和边（节点表示地点，边的权重表示距离）
G.add_edge('A', 'B', weight=5)
G.add_edge('A', 'C', weight=3)
G.add_edge('B', 'C', weight=1)
G.add_edge('B', 'D', weight=2)
G.add_edge('C', 'D', weight=4)

# 计算从节点A到节点D的最短路径
shortest_path = nx.shortest_path(G, source='A', target='D', weight='weight')
print("最短路径：", shortest_path)
```

### 库存需求预测代码（使用Python和Scikit-learn库中的线性回归模型）

```
import numpy as np
from sklearn.linear_model import LinearRegression

# 生成模拟数据（时间序列数据）
X = np.array([[1], [2], [3], [4], [5]])  # 时间步长
y = np.array([10, 12, 15, 18, 20])  # 库存需求

# 创建线性回归模型并训练
model = LinearRegression()
model.fit(X, y)

# 预测下一个时间步的库存需求
next_time_step = np.array([[6]])
predicted_demand = model.predict(next_time_step)
print("预测的下一个时间步库存需求：", predicted_demand[0])
```

### 数据指标语义理解与转换代码



## 数据指标语义映射与解析执行代码

```python
# 定义数据指标和标签的语义映射字典
semantic_mapping = {
    "total_sales": "销售总额",
    "net_profit": "净利润",
    "customer_acquisition_cost": "客户获取成本",
    "churn_rate": "客户流失率"
}

# 模拟用户输入的指标字符串
user_input = "Calculate the total_sales and net_profit for last quarter."

# 函数用于解析用户输入并转换为可执行的计算逻辑
def parse_and_execute(user_input):
    tokens = user_input.split(" ")
    calculated_results = []
    for token in tokens:
        if token in semantic_mapping:
            # 这里假设已经有相应的数据获取和计算函数，比如从数据库获取数据并计算指标值
            if token == "total_sales":
                # 模拟从数据库获取销售数据并计算销售总额
                sales_data = [100, 200, 150, 300]  # 假设这是上个季度的每月销售数据
                total_sales = sum(sales_data)
                calculated_results.append((semantic_mapping[token], total_sales))
            elif token == "net_profit":
                # 模拟计算净利润（这里简单假设为固定值）
                net_profit = 500
                calculated_results.append((semantic_mapping[token], net_profit))
    return calculated_results

# 执行解析和计算
results = parse_and_execute(user_input)
for result in results:
    print(f"{result[0]}: {result[1]}")
```

## 多任务数据分析调度代码

```python
import concurrent.futures

# 模拟三个数据分析任务函数
def task1():
    # 模拟任务 1 的数据分析操作，这里简单返回一个固定结果
    return "Task 1 result: Data analysis for sales trends completed."

def task2():
    # 模拟任务 2 的数据分析操作，比如分析客户行为数据
    customer_data = [{"id": 1, "behavior": "purchase"}, {"id": 2, "behavior": "browse"}]
    purchase_count = sum(1 for c in customer_data if c["behavior"] == "purchase")
    return f"Task 2 result: {purchase_count} customers made purchases."

def task3():
    # 模拟任务 3 的数据分析操作，例如分析产品库存数据
    inventory_data = {"product1": 10, "product2": 5, "product3": 8}
    low_stock_products = [p for p, q in inventory_data.items() if q < 8]
    return f"Task 3 result: Low stock products are {low_stock_products}."

# 函数用于调度多个数据分析任务并获取结果
def execute_tasks():
    with concurrent.futures.ThreadPoolExecutor() as executor:
        futures = [executor.submit(task1), executor.submit(task2), executor.submit(task3)]
        results = []
        for future in concurrent.futures.as_completed(futures):
            try:
                result = future.result()
                results.append(result)
            except Exception as e:
                print(f"Task execution failed: {e}")
    return results

# 执行任务调度
task_results = execute_tasks()
for result in task_results:
    print(result)
```

## 基于用户反馈的数据分析模型优化代码

```python
# 模拟初始的数据分析模型（简单的线性回归模型）
import numpy as np
from sklearn.linear_model import LinearRegression

# 模拟训练数据
X = np.array([[1], [2], [3], [4], [5]])
y = np.array([2, 4, 6, 8, 10])

# 创建并训练初始模型
model = LinearRegression()
model.fit(X, y)

# 模拟用户反馈数据（新的观测值和期望的预测值）
user_feedback_X = np.array([[6]])
user_feedback_y = np.array([12])

# 函数用于根据用户反馈更新模型
def update_model_with_feedback(model, X_feedback, y_feedback):
    # 将新的反馈数据与原训练数据合并
    X_updated = np.concatenate((model.X_, X_feedback), axis=0)
    y_updated = np.concatenate((model.y_, y_feedback))
    # 重新训练模型
    model.fit(X_updated, y_updated)
    return model

# 更新模型
updated_model = update_model_with_feedback(model, user_feedback_X, user_feedback_y)

# 使用更新后的模型进行预测
new_prediction = updated_model.predict(np.array([[7]]))
print(f"Updated model prediction for new data: {new_prediction[0]}")
```

## 产品用户活跃度数据分析代码

```python
import matplotlib.pyplot as plt
import pandas as pd

# 假设数据已存储为DataFrame格式，名为user_activity_data
# 包含 columns: ['region', 'date', 'active_users']

# 按照地区分组
grouped_data = user_activity_data.groupby('region')

for region, group in grouped_data:
    plt.plot(group['date'], group['active_users'], label=region)

plt.xlabel('Date')
plt.ylabel('Active Users')
plt.title('User Activity by Region')
plt.legend()
plt.show()
```

## 高阶计算任务与销售趋势预测代码

```python
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
import numpy as np

# 假设历史销售数据存储为DataFrame格式，名为sales_data
# 包含 columns: ['month', 'sales_volume']

# 提取特征与目标变量
X = sales_data[['month']].values
y = sales_data['sales_volume'].values

# 划分训练集与测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 训练线性回归模型
model = LinearRegression()
model.fit(X_train, y_train)

# 预测未来三个月销量
future_months = np.array([[len(X) + 1], [len(X) + 2], [len(X) + 3]]).reshape(-1, 1)
predicted_sales = model.predict(future_months)

print(f"预测未来三个月销量：{predicted_sales}")
```

## 智能体搭建的要点

### （一）数据质量和整合

+   确保数据的准确性、完整性和一致性，对收集到的数据进行清洗、预处理和标准化。

+   整合来自不同数据源的数据，解决数据格式不一致、语义模糊等问题，以便后续的分析和处理。

### （二）算法选择和优化

+   根据具体的业务场景和问题特点选择合适的算法，不同的算法在不同的数据集和任务上可能有不同的性能表现。

+   对选定的算法进行优化，调整参数、改进模型结构等，以提高模型的准确性和效率。

### （三）系统的可扩展性和灵活性

+   设计物流智能体系统时要考虑其可扩展性，能够方便地添加新的功能模块和处理更多的数据量。

+   系统应具有灵活性，能够适应物流业务的变化和发展，如业务流程调整、新的配送需求等。

### （四）用户体验和界面设计

+   关注用户体验，设计简洁、易用的操作界面，方便物流人员进行操作和管理。

+   提供直观的可视化界面，展示物流信息、路径规划结果、库存状态等，帮助用户更好地理解和决策。

### （五）安全和隐私保护

+   物流数据涉及客户信息、货物信息等敏感数据，要采取严格的安全措施，如数据加密、访问控制等，防止数据泄露和滥用。

+   遵守相关的法律法规和隐私政策，保护用户的隐私权益。

### （六）持续监控和改进

+   建立监控机制，实时监测物流智能体系统的运行状态和性能指标，及时发现和解决问题。

+   根据实际运行情况和用户反馈，不断改进和优化系统，提高物流智能体的性能和服务质量。

搭建物流智能体需要综合运用多种技术和方法，并结合实际业务需求进行不断优化和完善。希望以上介绍能够帮助你对物流智能体的搭建有一个全面的了解。
