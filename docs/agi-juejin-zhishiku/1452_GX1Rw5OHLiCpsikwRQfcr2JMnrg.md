
# LlamaIndex之Structured Planning Agent：规划Agent

> 来源：[https://agijuejin.feishu.cn/docx/OwnDdxGxyopy2OxFOqGcNdlznth](https://agijuejin.feishu.cn/docx/OwnDdxGxyopy2OxFOqGcNdlznth)

> 作者：lucas大叔

> 原文 [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/696491104)hihu.com/p/696491...

规划（ Planning ）是 agent 的主要组成部分，特别是在任意 RAG 管道前添加复杂的查询理解层而言，具有非凡的价值。为了推理出最终的响应，尽管 `ReAct` 可以使用结构化方法将输入分解为一组 function call 和 thought ，但将初始输入/任务分解为子任务可以使 `ReAct` 循环（或者其他推理循环）更容易执行。

通过规划完整的 `DAG` 步骤并在此过程中细化步骤，可以构建研究助手超越回答有针对性的问题（例如，在 top-k RAG 中），解决来自多个来源的复杂任务。 LlamIndex 全新发布的 structured planning agent 帮助构建 planning agent 时创建核心抽象。 structured planning agent 生成子任务的 `DAG` ，可以使用任何 agent 推理循环来解决每个子任务（例如， `ReAct`、function calling 或 `subplanner` ）。

> `DAG`（Directed Acyclic Graph，有向无环图）：在 图论 中，如果一个 有向图 从任意 顶点 出发无法经过若干条 边 回到该 点 ，则这个图是一个有向无环图 。

> Tips： Planning agent 利用了结构化输出和 agent 推理。因此建议使用功能强大的 LLM （ OpenAI、Anthropic 等），而开源 LLM 可能需要经过提示工程或微调才能进行规划。

## 配置

下载文档

```
!mkdir -p 'data/10k/'
!wget 'https://raw.githubusercontent.com/run-llama/llama_index/main/docs/docs/examples/data/10k/uber_2021.pdf' -O 'data/10k/uber_2021.pdf'
!wget 'https://raw.githubusercontent.com/run-llama/llama_index/main/docs/docs/examples/data/10k/lyft_2021.pdf' -O 'data/10k/lyft_2021.pdf'
```

设置LLM和embedding模型

```
import os

os.environ["OPENAI_API_KEY"] = "sk-..."

from llama_index.core import Settings
from llama_index.llms.openai import OpenAI
from llama_index.embeddings.openai import OpenAIEmbedding

# Use ollama in JSON mode
Settings.llm = OpenAI(
    model="gpt-4-turbo",
    temperature=0.1,
)
Settings.embed_model = OpenAIEmbedding(model_name="text-embedding-3-small")
```

加载文档，创建工具

```
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader
from llama_index.core.tools import QueryEngineTool

# Load documents, create tools
lyft_documents = SimpleDirectoryReader(
    input_files=["./data/10k/lyft_2021.pdf"]
).load_data()
uber_documents = SimpleDirectoryReader(
    input_files=["./data/10k/uber_2021.pdf"]
).load_data()

lyft_index = VectorStoreIndex.from_documents(lyft_documents)
uber_index = VectorStoreIndex.from_documents(uber_documents)

lyft_tool = QueryEngineTool.from_defaults(
    lyft_index.as_query_engine(),
    name="lyft_2021",
    description="Useful for asking questions about Lyft's 2021 10-K filling.",
)

uber_tool = QueryEngineTool.from_defaults(
    uber_index.as_query_engine(),
    name="uber_2021",
    description="Useful for asking questions about Uber's 2021 10-K filling.",
)
```

## 高阶API

本节介绍使用 structured planning agent 创建和交流的高阶 API 。

### 创建Agent

```
from llama_index.core.agent import (
    StructuredPlannerAgent,
    FunctionCallingAgentWorker,
    ReActAgentWorker,
)

# create the function calling worker for reasoning
worker = FunctionCallingAgentWorker.from_tools(
    [lyft_tool, uber_tool], verbose=True
)

# wrap the worker in the top-level planner
agent = StructuredPlannerAgent(
    worker, tools=[lyft_tool, uber_tool], verbose=True
)
```

### 给agent分配复杂任务

```
import nest_asyncio

nest_asyncio.apply()
response = agent.chat(
    "Summarize the key risk factors for Lyft and Uber in their 2021 10-K filings."
)
```

执行结果



# Initial plan

Extract Lyft Risk Factors:
Summarize the key risk factors from Lyft's 2021 10-K filing. -> A summary of the key risk factors for Lyft as outlined in their 2021 10-K filing.
`deps`: []

Extract Uber Risk Factors:
Summarize the key risk factors from Uber's 2021 10-K filing. -> A summary of the key risk factors for Uber as outlined in their 2021 10-K filing.
`deps`: []

Combine Risk Factors Summaries:
Combine the summaries of key risk factors for Lyft and Uber from their 2021 10-K filings into a comprehensive overview. -> A comprehensive summary of the key risk factors for both Lyft and Uber as outlined in their respective 2021 10-K filings.
`deps`: [`Extract Lyft Risk Factors`, `Extract Uber Risk Factors`]

## Calling Function

> Running step `a1428164-9f35-4a0e-9d6d-3328ebe490d4`. Step input: Summarize the key risk factors from Lyft's 2021 10-K filing.
> Added user message to memory: Summarize the key risk factors from Lyft's 2021 10-K filing.
> Running step `16a0d1f5-03ba-4b47-af0f-ec8758e6dda7`. Step input: Summarize the key risk factors from Uber's 2021 10-K filing.
> Added user message to memory: Summarize the key risk factors from Uber's 2021 10-K filing.

Calling function: `lyft_2021` with args: `{"input": "key risk factors"}`
Calling function: `uber_2021` with args: `{"input": "What are the key risk factors mentioned in the 2021 10-K filing?"}`

## Function Output

The key risk factors mentioned in the 2021 10-K filing include:

1. The potential adverse effects of the COVID-19 pandemic on the business.
2. The risk associated with the classification of drivers as employees or quasi-employees instead of independent contractors.
3. Intense competition in the mobility, delivery, and logistics industries, which feature low barriers to entry, low switching costs, and well-capitalized competitors.
4. The possibility of having to lower fares or service fees and offer significant driver incentives and consumer discounts to remain competitive.
5. Historical financial losses and the expectation of significant increases in operating expenses, which may prevent the company from achieving or maintaining profitability.
6. The necessity of attracting and maintaining a critical mass of drivers, consumers, merchants, shippers, and carriers to keep the platform appealing.
7. The importance of maintaining and enhancing the company's brand and reputation, especially following significant negative publicity in the past.
8. Challenges related to the company's historical workplace culture and the ongoing efforts to address these issues.
9. The need to optimize the organizational structure and effectively manage growth to ensure good financial performance and future prospects.

> Running step `85dc3f14-7c78-4759-88d6-1e9b071c1059`. Step input: `None`

The key risk factors for investing in Lyft's Class A common stock include:

1. General economic factors such as the impact of the COVID-19 pandemic, natural disasters, economic downturns, public health crises, and political crises.
2. Operational factors including Lyft's limited operating history, the unpredictability of financial performance and results of operations, competition in the industry, and uncertainty regarding the growth of the ridesharing market.
3. The company's ability to attract and retain qualified drivers and riders, manage insurance coverage and reserves, and handle claims through third-party insurance providers.
4. Challenges related to autonomous vehicle technology and the development of the autonomous vehicle industry.
5. The company's reputation, brand, and company culture, as well as the potential for illegal or improper activity by users of the platform.
6. The accuracy of background checks on potential or current drivers, changes to pricing practices, and the growth and quality of Lyft's network of Light Vehicles.
7. Security or privacy breaches, system failures, and reliance on third parties such as Amazon Web Services, vehicle rental partners, and payment processors.
8. The ability to operate and manage Lyft's Express Drive and Lyft Rentals programs, delivery service platform, and the effectiveness of matching riders in Shared and Shared Saver Rides offerings.
9. The development of new offerings on the platform and managing the complexities associated with such expansion.

> Running step `8705c8fa-e764-460c-a849-a5f7851e454b`. Step input: `None`

## LLM Response

The key risk factors mentioned in Uber's 2021 10-K filing include:

1. COVID-19 Pandemic Impact: The ongoing adverse effects of the COVID-19 pandemic on the business.
2. Driver Classification: Risks associated with the classification of drivers as employees or quasi-employees instead of independent contractors.
3. Intense Competition: The competitive nature of the mobility, delivery, and logistics industries, which have low barriers to entry, low switching costs, and well-capitalized competitors.
4. Pricing and Incentives: The need to potentially lower fares or service fees and offer significant driver incentives and consumer discounts to remain competitive.
5. Financial Losses: Historical financial losses and expected significant increases in operating expenses, which may prevent the company from achieving or maintaining profitability.
6. Platform Appeal: The necessity of attracting and maintaining a critical mass of drivers, consumers, merchants, shippers, and carriers to keep the platform appealing.
7. Brand and Reputation: The importance of maintaining and enhancing the company's brand and reputation, particularly in light of significant negative publicity in the past.
8. Workplace Culture: Challenges related to the company's historical workplace culture and the ongoing efforts to address these issues.
9. Organizational Structure: The need to optimize the organizational structure and effectively manage growth to ensure good financial performance and future prospects.

## LLM Response

The key risk factors from Lyft's 2021 10-K filing include:

1. Economic and Public Health Factors: Risks related to general economic conditions, the impact of the COVID-19 pandemic, natural disasters, economic downturns, public health crises, and political crises.
2. Operational Factors: Lyft's limited operating history, unpredictability of financial performance, competition in the industry, and uncertainty regarding the growth of the ridesharing market.
3. Driver and Rider Dynamics: Challenges in attracting and retaining qualified drivers and riders, managing insurance coverage and reserves, and handling claims through third-party insurance providers.
4. Autonomous Vehicle Technology: Challenges related to the development of autonomous vehicle technology and the autonomous vehicle industry.
5. Company Reputation and Culture: Risks associated with the company's reputation, brand, company culture, and potential illegal or improper activity by platform users.
6. Background Checks and Pricing Practices: The accuracy of background checks on potential or current drivers, changes to pricing practices, and the growth and quality of Lyft's network of Light Vehicles.
7. Security and Privacy: Security or privacy breaches, system failures, and reliance on third parties such as Amazon Web Services, vehicle rental partners, and payment processors.
8. Program Management: The ability to operate and manage Lyft's Express Drive and Lyft Rentals programs, delivery service platform, and the effectiveness of matching riders in Shared and Shared Saver Rides offerings.
9. Expansion and New Offerings: The development of new offerings on the platform and managing the complexities associated with such expansion.

## Refined plan

Combine Risk Factors Summaries:
Combine the summaries of key risk factors for Lyft and Uber from their 2021 10-K filings into a comprehensive overview. -> A comprehensive summary of the key risk factors for both Lyft and Uber as outlined in their respective 2021 10-K filings.
`deps`: [`Extract Lyft Risk Factors`, `Extract Uber Risk Factors`]

Summarize Key Risk Factors:
Summarize the combined risk factors from Lyft and Uber into a concise and comprehensive overview. -> A concise and comprehensive summary of the key risk factors for both Lyft and Uber as outlined in their respective 2021 10-K filings.
`deps`: [`Combine Risk Factors Summaries`]

> Running step `874c8c34-3720-4d6f-bde1-7064e18a6433`. Step input: Combine the summaries of key risk factors for Lyft and Uber from their 2021 10-K filings into a comprehensive overview.
> Added user message to memory: Combine the summaries of key risk factors for Lyft and Uber from their 2021 10-K filings into a comprehensive overview.

## LLM Response

The key risk factors from the 2021 10-K filings of Lyft and Uber provide a comprehensive overview of the challenges faced by both companies in the ridesharing and broader mobility industry:

1. COVID-19 and Economic Conditions: Both companies highlight the adverse effects of the COVID-19 pandemic and general economic conditions, including economic downturns and public health crises, which impact their operations and market demand.

2. Regulatory and Classification Issues: Uber specifically mentions the risk associated with the classification of drivers as employees or quasi-employees instead of independent contractors, which is a significant issue for the industry.

3. Competition and Market Conditions: Both companies face intense competition in the mobility, delivery, and logistics industries, characterized by low barriers to entry, low switching costs, and well-capitalized competitors. Lyft also notes the unpredictability of financial performance and the uncertainty regarding the growth of the ridesharing market.

4. Operational Challenges: Lyft discusses its limited operating history and the challenges of managing insurance and claims, while Uber emphasizes the need to attract and maintain a critical mass of drivers, consumers, merchants, shippers, and carriers.

5. Technological Advancements: Lyft addresses challenges related to autonomous vehicle technology and the development of the autonomous vehicle industry, which are crucial for future growth and competition.

6. Brand and Reputation: Both companies stress the importance of maintaining and enhancing their brand and reputation. Uber specifically mentions the need to address historical workplace culture issues, while Lyft highlights the potential for illegal or improper activity by users.

7. Financial Sustainability: Uber discusses its historical financial losses and the significant increases in operating expenses that may prevent profitability. Lyft also faces similar financial unpredictability.

8. Security and Privacy: Lyft mentions risks related to security or privacy breaches and system failures, emphasizing their reliance on third parties like Amazon Web Services and payment processors.

9. Service Management and Expansion: Lyft discusses the management of specific programs like Express Drive and Lyft Rentals, and the development of new offerings. Uber highlights the necessity of optimizing its organizational structure to manage growth effectively.

This combined overview underscores the shared and unique challenges faced by Lyft and Uber as they navigate regulatory landscapes, technological advancements, competitive pressures, and the need for operational efficiency and innovation in a rapidly evolving industry.

## Refined plan

Summarize Key Risk Factors:
Summarize the combined risk factors from Lyft and Uber into a concise and comprehensive overview. -> A concise and comprehensive summary of the key risk factors for both Lyft and Uber as outlined in their respective 2021 10-K filings.
`deps`: [`Combine Risk Factors Summaries`]

> Running step `8a452cd1-a940-47e8-82a1-277523d689eb`. Step input: Summarize the combined risk factors from Lyft and Uber into a concise and comprehensive overview.
> Added user message to memory: Summarize the combined risk factors from Lyft and Uber into a concise and comprehensive overview.

## LLM Response

The combined key risk factors from the 2021 10-K filings of Lyft and Uber highlight several shared and distinct challenges in the ridesharing and mobility industry:

1. Pandemic and Economic Impact: Both companies are significantly affected by the COVID-19 pandemic and broader economic conditions, which influence market demand and operational stability.

2. Regulatory and Labor Issues: A major concern is the classification of drivers as independent contractors versus employees, impacting labor costs and operational flexibility.

3. Intense Competition: They face fierce competition in the mobility, delivery, and logistics sectors, characterized by low entry barriers and aggressive, well-funded competitors.

4. Operational and Financial Uncertainty: Both companies deal with financial unpredictability and operational challenges, including managing insurance, claims, and maintaining profitability amidst high expenses.

5. Technological and Service Evolution: Challenges in adopting new technologies like autonomous vehicles and expanding service offerings are crucial for staying competitive and managing growth.

6. Brand and Reputation Management: Maintaining a positive brand image and company culture is vital, especially given past issues and the potential for user misconduct.

7. Security and Privacy Concerns: Risks related to data breaches, system failures, and dependency on third-party services like cloud providers and payment processors are significant.

This overview encapsulates the multifaceted risks that Lyft and Uber face, emphasizing the need for strategic management and innovation to navigate a complex, rapidly changing industry landscape.

## Refined plan

Summarize Key Risk Factors:
Summarize the key risk factors for Lyft and Uber in their 2021 10-K filings. -> The combined key risk factors from the 2021 10-K filings of Lyft and Uber highlight several shared and distinct challenges in the ridesharing and mobility industry:

1. Pandemic and Economic Impact: Both companies are significantly affected by the COVID-19 pandemic and broader economic conditions, which influence market demand and operational stability.

2. Regulatory and Labor Issues: A major concern is the classification of drivers as independent contractors versus employees, impacting labor costs and operational flexibility.

3. Intense Competition: They face fierce competition in the mobility, delivery, and logistics sectors, characterized by low entry barriers and aggressive, well-funded competitors.

4. Operational and Financial Uncertainty: Both companies deal with financial unpredictability and operational challenges, including managing insurance, claims, and maintaining profitability amidst high expenses.

5. Technological and Service Evolution: Challenges in adopting new technologies like autonomous vehicles and expanding service offerings are crucial for staying competitive and managing growth.

6. Brand and Reputation Management: Maintaining a positive brand image and company culture is vital, especially given past issues and the potential for user misconduct.

7. Security and Privacy Concerns: Risks related to data breaches, system failures, and dependency on third-party services like cloud providers and payment processors are significant.

This overview encapsulates the multifaceted risks that Lyft and Uber face, emphasizing the need for strategic management and innovation to navigate a complex, rapidly changing industry landscape.
`deps`: []



# 打印最终响应

```
print(str(response))
```

响应内容

```
assistant: The combined key risk factors from the 2021 10-K filings of Lyft and Uber highlight several shared and distinct challenges in the ridesharing and mobility industry:

1. **Pandemic and Economic Impact**: Both companies are significantly affected by the COVID-19 pandemic and broader economic conditions, which influence market demand and operational stability.

2. **Regulatory and Labor Issues**: A major concern is the classification of drivers as independent contractors versus employees, impacting labor costs and operational flexibility.

3. **Intense Competition**: They face fierce competition in the mobility, delivery, and logistics sectors, characterized by low entry barriers and aggressive, well-funded competitors.

4. **Operational and Financial Uncertainty**: Both companies deal with financial unpredictability and operational challenges, including managing insurance, claims, and maintaining profitability amidst high expenses.

5. **Technological and Service Evolution**: Challenges in adopting new technologies like autonomous vehicles and expanding service offerings are crucial for staying competitive and managing growth.

6. **Brand and Reputation Management**: Maintaining a positive brand image and company culture is vital, especially given past issues and the potential for user misconduct.

7. **Security and Privacy Concerns**: Risks related to data breaches, system failures, and dependency on third-party services like cloud providers and payment processors are significant.

This overview encapsulates the multifaceted risks that Lyft and Uber face, emphasizing the need for strategic management and innovation to navigate a complex, rapidly changing industry landscape.
```

## 更改 prompt

`StructuredPlanningAgent` 有两个关键 prompt ：

1. 初始规划 prompt

2. 规划细化 prompt

下面展示如何配置这些 prompt ，以默认设置为例。

```
DEFAULT_INITIAL_PLAN_PROMPT = """\
Think step-by-step. Given a task and a set of tools, create a comprehesive, end-to-end plan to accomplish the task.
Keep in mind not every task needs to be decomposed into multiple sub-tasks if it is simple enough.
The plan should end with a sub-task that satisfies the overall task.
The tools available are:
{tools_str}
Overall Task: {task}
""";

DEFAULT_PLAN_REFINE_PROMPT = """\
Think step-by-step. Given an overall task, a set of tools, and completed sub-tasks, update (if needed) the remaining sub-tasks so that the overall task can still be completed.
The plan should end with a sub-task that satisfies the overall task.
If the remaining sub-tasks are sufficient, you can skip this step.
The tools available are:
{tools_str}
Overall Task:
{task}
Completed Sub-Tasks + Outputs:
{completed_outputs}
Remaining Sub-Tasks:
{remaining_sub_tasks}
""";
```

构造 `StructuredPlannerAgent`

```
agent = StructuredPlannerAgent(
    worker,
    tools=[lyft_tool, uber_tool],
    initial_plan_prompt=DEFAULT_INITIAL_PLAN_PROMPT,
    plan_refine_prompt=DEFAULT_PLAN_REFINE_PROMPT,
    verbose=True,
)
```

## 低阶 API

本节使用与前面相同的 agent ，但暴露了底层发生的步骤，方便人们动态修改或调试底层规划、任务。

### 创建 Agent

```
from llama_index.core.agent import (
    StructuredPlannerAgent,
    FunctionCallingAgentWorker,
    ReActAgentWorker,
)

# create the react worker for reasoning
worker = FunctionCallingAgentWorker.from_tools(
    [lyft_tool, uber_tool], verbose=True
)

# wrap the worker in the top-level planner
agent = StructuredPlannerAgent(
    worker, tools=[lyft_tool, uber_tool], verbose=True
)
```

### 创建初始任务和规划

```
plan_id = agent.create_plan(
    "Summarize the key risk factors for Lyft and Uber in their 2021 10-K filings."
)
```

创建结果

```
=== Initial plan ===
Extract Lyft Risk Factors:
Extract the key risk factors from Lyft's 2021 10-K filing. -> A detailed list of key risk factors for Lyft from its 2021 10-K filing.
deps: []

Extract Uber Risk Factors:
Extract the key risk factors from Uber's 2021 10-K filing. -> A detailed list of key risk factors for Uber from its 2021 10-K filing.
deps: []

Summarize Risk Factors:
Summarize the key risk factors for both Lyft and Uber based on the extracted information from their 2021 10-K filings. -> A comprehensive summary of the key risk factors for Lyft and Uber from their 2021 10-K filings.
deps: ['Extract Lyft Risk Factors', 'Extract Uber Risk Factors']
```

### 检查初始任务和规划

```
plan = agent.state.plan_dict[plan_id]

for sub_task in plan.sub_tasks:
    print(f"===== Sub Task {sub_task.name} =====")
    print("Expected output: ", sub_task.expected_output)
    print("Dependencies: ", sub_task.dependencies)
```

任务和规划内容

```
===== Sub Task Extract Lyft Risk Factors =====
Expected output:  A detailed list of key risk factors for Lyft from its 2021 10-K filing.
Dependencies:  []
===== Sub Task Extract Uber Risk Factors =====
Expected output:  A detailed list of key risk factors for Uber from its 2021 10-K filing.
Dependencies:  []
===== Sub Task Summarize Risk Factors =====
Expected output:  A comprehensive summary of the key risk factors for Lyft and Uber from their 2021 10-K filings.
Dependencies:  ['Extract Lyft Risk Factors', 'Extract Uber Risk Factors']
```

### 执行第一组任务

执行第一组任务，确保它们的依赖关系得到满足。

```
next_tasks = agent.state.get_next_sub_tasks(plan_id)

for sub_task in next_tasks:
    print(f"===== Sub Task {sub_task.name} =====")
    print("Expected output: ", sub_task.expected_output)
    print("Dependencies: ", sub_task.dependencies)


for sub_task in next_tasks:
    response = agent.run_task(sub_task.name)
    agent.mark_task_complete(plan_id, sub_task.name)
```

第一组任务执行结果



# Lyft 与 Uber 风险因子提取与任务执行日志

## Sub Task: Extract Lyft Risk Factors

Expected output: A detailed list of key risk factors for Lyft from its 2021 10-K filing.
Dependencies: []

> Running step 4f708bcd-3078-4138-897b-e7f643fc7f35. Step input: Extract the key risk factors from Lyft's 2021 10-K filing.
Added user message to memory: Extract the key risk factors from Lyft's 2021 10-K filing.

### Calling Function

Calling function: `lyft_2021` with args: `{"input": "key risk factors"}`

### Function Output

Key risk factors for Lyft include general economic factors such as the impact of the COVID-19 pandemic, natural disasters, and macroeconomic conditions; operational factors such as limited operating history, competition, unpredictability of results, and the ability to attract and retain drivers and riders; and specific risks related to technology, such as autonomous vehicle development, security breaches, and reliance on third-party service providers. Additionally, risks related to insurance coverage, the adequacy of insurance reserves, and the handling of auto-related insurance claims by third parties are significant. The company also faces risks from potential illegal activities by users, inaccuracies in background checks, and challenges related to managing growth and expanding service offerings.

> Running step 060f219a-0668-47ec-9bef-ce131d0ba887. Step input: None

### LLM Response

The key risk factors identified in Lyft's 2021 10-K filing include:

1. **General Economic Factors:**
   - Impact of the COVID-19 pandemic.
   - Natural disasters.
   - Macroeconomic conditions.

2. **Operational Factors:**
   - Limited operating history.
   - Competition.
   - Unpredictability of results.
   - Ability to attract and retain drivers and riders.

3. **Technology-Related Risks:**
   - Autonomous vehicle development.
   - Security breaches.
   - Reliance on third-party service providers.

4. **Insurance-Related Risks:**
   - Insurance coverage adequacy.
   - Insurance reserves adequacy.
   - Handling of auto-related insurance claims by third parties.

5. **Other Risks:**
   - Potential illegal activities by users.
   - Inaccuracies in background checks.
   - Challenges related to managing growth and expanding service offerings.

> Running step 00f6732b-594c-4ecf-82e5-e9f005f8ab71. Step input: Extract the key risk factors from Uber's 2021 10-K filing.
Added user message to memory: Extract the key risk factors from Uber's 2021 10-K filing.

### Calling Function

Calling function: `uber_2021` with args: `{"input": "key risk factors"}`

### Function Output

The key risk factors include market risks such as interest rate risk, investment risk, and foreign currency risk. Interest rate risk is associated with fluctuations in interest rates affecting the company's refinanced term loan facilities and fixed rate notes. Investment risk involves the preservation of capital and meeting liquidity requirements without significantly increasing risk, with exposure to changes in interest rates and the carrying amounts of investments in other companies. Foreign currency risk arises from international transactions in multiple currencies, which could affect revenue and operating results due to fluctuations in exchange rates. Additionally, there are risks related to cyberattacks, which could harm the company's reputation, business, and operating results by compromising data security and the integrity of information technology systems.

> Running step 04e95d17-e7a7-4f35-8519-33e9439e9659. Step input: None

### LLM Response

The key risk factors identified in Uber's 2021 10-K filing include:

1. **Market Risks:**
   - **Interest Rate Risk:** Fluctuations in interest rates affecting the company's refinanced term loan facilities and fixed rate notes.
   - **Investment Risk:** Challenges in preserving capital and meeting liquidity requirements without significantly increasing risk, with exposure to changes in interest rates and the carrying amounts of investments in other companies.
   - **Foreign Currency Risk:** Risks from international transactions in multiple currencies, which could affect revenue and operating results due to fluctuations in exchange rates.

2. **Cybersecurity Risks:**
   - Risks related to cyberattacks, which could harm the company's reputation, business, and operating results by compromising data security and the integrity of information technology systems.

如果愿意的话，我们甚至可以逐步执行每个任务。过程大致如下：

```
# Step-wise execution per task

for sub_task in next_tasks:
    # get the task from the state 
    task = agent.state.get_task(sub_task.name)

    # run intial resoning step
    step_output = agent.run_step(task.task_id)

    # loop until the last step is reached
    while not step_output.is_last:
        step_output = agent.run_step(task.task_id)
    
    # finalize the response and commit to memory
    agent.finalize_response(task.task_id, step_output=step_output)
```

## 检查是否完成

如果没有剩余任务，那么可以停止。否则，我们可以继续优化当前规划并继续。

```
next_tasks = agent.get_next_tasks(plan_id)
print(len(next_tasks))
```

1

```
for sub_task in next_tasks:
    print(f"===== Sub Task {sub_task} =====")
```

## Sub Task: Summarize Risk Factors

### 优化规划

由于还有任务尚未完成，让我们优化计划确保按计划进行。

```
# refine the plan
agent.refine_plan(
    "Summarize the key risk factors for Lyft and Uber in their 2021 10-K filings.",
    plan_id,
)
```

### Refined plan

Summarize Risk Factors:

Summarize the key risk factors for both Lyft and Uber based on the extracted information from their 2021 10-K filings. -> A comprehensive summary of the key risk factors for Lyft and Uber from their 2021 10-K filings.

deps: ['Extract Lyft Risk Factors', 'Extract Uber Risk Factors']

```
plan = agent.state.plan_dict[plan_id]

for sub_task in plan.sub_tasks:
    print(f"===== Sub Task {sub_task.name} =====")
    print("Expected output: ", sub_task.expected_output)
    print("Dependencies: ", sub_task.dependencies)
```

## Sub Task: Summarize Risk Factors

Expected output: A comprehensive summary of the key risk factors for Lyft and Uber from their 2021 10-K filings.

Dependencies: ['Extract Lyft Risk Factors', 'Extract Uber Risk Factors']

### 循环直至完成

随着规划被优化，重复这个过程直到没有更多的任务需要执行。

```
import asyncio

while True:
    # are we done?
    next_tasks = agent.get_next_tasks(plan_id)
    if len(next_tasks) == 0:
        break

    # run concurrently for better performance
    responses = await asyncio.gather(
        *[agent.arun_task(task_id) for task_id in next_tasks]
    )
    for task_id in next_tasks:
        agent.mark_task_complete(plan_id, task_id)

    # refine the plan
    await agent.arefine_plan(
        "Summarize the key risk factors for Lyft and Uber in their 2021 10-K filings.",
        plan_id,
    )
```

## 执行过程



# Structured Planning Agent

> Running step `70356e9a-98ee-49f5-b15f-e5a6b43381d0`. Step input: Summarize the key risk factors for both Lyft and Uber based on the extracted information from their 2021 10-K filings.

Added user message to memory: Summarize the key risk factors for both Lyft and Uber based on the extracted information from their 2021 10-K filings.

## LLM Response

The key risk factors for Lyft and Uber from their 2021 10-K filings highlight several areas of concern for both companies, with some overlapping and unique challenges:

### Common Risk Factors:
- **Economic and Market Conditions:** Both companies are affected by general economic factors such as macroeconomic conditions and the impact of the COVID-19 pandemic. Additionally, Uber faces specific market risks like interest rate risk, investment risk, and foreign currency risk due to its global operations.

### Lyft-Specific Risk Factors:
- **Operational Challenges:** Lyft faces risks related to its limited operating history, competition, unpredictability of results, and the ability to attract and retain drivers and riders.
- **Technology and Insurance Risks:** Challenges include autonomous vehicle development, security breaches, reliance on third-party service providers, insurance coverage adequacy, and handling of auto-related insurance claims by third parties.
- **Regulatory and Legal Risks:** Potential illegal activities by users, inaccuracies in background checks, and challenges related to managing growth and expanding service offerings.

### Uber-Specific Risk Factors:
- **Cybersecurity Risks:** Uber is particularly concerned with risks related to cyberattacks that could compromise data security and the integrity of its information technology systems.

Both companies operate in dynamic and rapidly evolving environments, facing significant operational and market challenges that could impact their business operations and financial stability.

## Refined plan

### Extract Lyft Risk Factors:
Extract the key risk factors from Lyft's 2021 10-K filing. -> The key risk factors identified in Lyft's 2021 10-K filing include general economic factors, operational factors, technology-related risks, insurance-related risks, and other risks.
`deps: []`

### Extract Uber Risk Factors:
Extract the key risk factors from Uber's 2021 10-K filing. -> The key risk factors identified in Uber's 2021 10-K filing include market risks, cybersecurity risks, and other specific operational and financial risks.
`deps: []`

### Summarize Risk Factors:
Summarize the key risk factors for Lyft and Uber from their 2021 10-K filings. -> A summary of the key risk factors for Lyft and Uber, highlighting common and unique challenges, including economic, operational, technology, insurance, and cybersecurity risks.
`deps: ['Extract Lyft Risk Factors', 'Extract Uber Risk Factors']`

最后应该得到单一的响应，即最终响应。

```
print(str(responses[-1]))
```

响应内容

```
assistant: The key risk factors for Lyft and Uber from their 2021 10-K filings highlight several areas of concern for both companies, with some overlapping and unique challenges:

### Common Risk Factors:
- **Economic and Market Conditions:** Both companies are affected by general economic factors such as macroeconomic conditions and the impact of the COVID-19 pandemic. Additionally, Uber faces specific market risks like interest rate risk, investment risk, and foreign currency risk due to its global operations.

### Lyft-Specific Risk Factors:
- **Operational Challenges:** Lyft faces risks related to its limited operating history, competition, unpredictability of results, and the ability to attract and retain drivers and riders.
- **Technology and Insurance Risks:** Challenges include autonomous vehicle development, security breaches, reliance on third-party service providers, insurance coverage adequacy, and handling of auto-related insurance claims by third parties.
- **Regulatory and Legal Risks:** Potential illegal activities by users, inaccuracies in background checks, and challenges related to managing growth and expanding service offerings.

### Uber-Specific Risk Factors:
- **Cybersecurity Risks:** Uber is particularly concerned with risks related to cyberattacks that could compromise data security and the integrity of its information technology systems.

Both companies operate in dynamic and rapidly evolving environments, facing significant operational and market challenges that could impact their business operations and financial stability.
```
