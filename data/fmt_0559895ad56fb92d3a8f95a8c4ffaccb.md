
```python
import os
from dotenv import load_dotenv
from typing import Any
from pathlib import Path


# Add references
# Add references
from azure.identity import DefaultAzureCredential
from azure.ai.agents import AgentsClient
from azure.ai.agents.models import FilePurpose, CodeInterpreterTool, ListSortOrder, MessageRole

def main(): 

    # Clear the console
    os.system('cls' if os.name == 'nt' else 'clear')

    # Load environment variables from .env file
    load_dotenv()
    project_endpoint = os.getenv("PROJECT_ENDPOINT")
    model_deployment = os.getenv("MODEL_DEPLOYMENT_NAME")

    # Display the data to be analyzed
    script_dir = Path(__file__).parent  # Get the directory of the script
    file_path = script_dir / 'data.txt'

    with file_path.open('r') as file:
        data = file.read() + "\n"
        print(data)

    # Connect to the Agent client
    # Connect to the Agent client
    agent_client = AgentsClient(
       endpoint=project_endpoint,
       credential=DefaultAzureCredential(
           exclude_environment_credential=True,
           exclude_managed_identity_credential=True
       )
    )
    with agent_client:
        # Upload the data file and create a CodeInterpreterTool
        # Upload the data file and create a CodeInterpreterTool
        file = agent_client.files.upload_and_poll(
            file_path=file_path, purpose=FilePurpose.AGENTS
        )
        print(f"Uploaded {file.filename}")
        
        code_interpreter = CodeInterpreterTool(file_ids=[file.id])

        # Define an agent that uses the CodeInterpreterTool
        # Define an agent that uses the CodeInterpreterTool
        agent = agent_client.create_agent(
            model=model_deployment,
            name="data-agent",
            instructions="You are an AI agent that analyzes the data in the file that has been uploaded. Use Python to calculate statistical metrics as necessary.",
            tools=code_interpreter.definitions,
            tool_resources=code_interpreter.resources,
        )
        print(f"Using agent: {agent.name}")

        # Create a thread for the conversation
        # Create a thread for the conversation
        thread = agent_client.threads.create()

        # Loop until the user types 'quit'
        while True:
            # Get input text
            user_prompt = input("Enter a prompt (or type 'quit' to exit): ")
            if user_prompt.lower() == "quit":
                break
            if len(user_prompt) == 0:
                print("Please enter a prompt.")
                continue

            # Send a prompt to the agent
            # Send a prompt to the agent
            message = agent_client.messages.create(
                thread_id=thread.id,
                role="user",
                content=user_prompt,
            )
            
            run = agent_client.runs.create_and_process(thread_id=thread.id, agent_id=agent.id)

            # Check the run status for failures
            # Check the run status for failures
            if run.status == "failed":
                print(f"Run failed: {run.last_error}")

            # Show the latest response from the agent
            # Show the latest response from the agent
            last_msg = agent_client.messages.get_last_message_text_by_role(
               thread_id=thread.id,
               role=MessageRole.AGENT,
            )
            if last_msg:
               print(f"Last Message: {last_msg.text.value}")

        # Get the conversation history
        # Show the latest response from the agent
        last_msg = agent_client.messages.get_last_message_text_by_role(
           thread_id=thread.id,
           role=MessageRole.AGENT,
        )
        if last_msg:
           print(f"Last Message: {last_msg.text.value}")

        # Clean up
        # Clean up
        agent_client.delete_agent(agent.id)

if __name__ == '__main__': 
    main()
```

> 这段代码实现了一个基于 Azure AI 的数据分析代理系统，主要功能是让 AI 代理分析用户上传的数据文件并回答相关问题。以下是核心功能解析：

## 核心功能

- 加载并显示本地数据文件（`data.txt`）

- 创建具备 Python 代码解释能力的 AI 代理

- 交互式分析数据文件内容

- 支持自然语言提问获取分析结果

## 关键技术实现

### 文件处理

- 使用`file_path.open()`读取数据文件

- 通过`upload_and_poll()`上传至 Azure 服务

### AI 代理配置

```python
CodeInterpreterTool(file_ids=[file.id])  # 赋予代理代码解释能力
```

```python
instructions="...Use Python to calculate statistical metrics..."  # 指定分析任务
```

### 交互系统

- 创建独立对话线程（`threads.create()`）

- 循环处理用户输入（`while True`）

- 实时显示 AI 响应

## 典型工作流程

1）加载本地数据文件 → 2）上传至云端 → 3）创建分析代理 → 4）接收用户问题 → 5）执行 Python 分析 → 6）返回结果

## 技术特点

- 使用 Azure 的代码解释器工具（`CodeInterpreterTool`）

- 支持动态 Python 代码执行

- 完整的对话历史管理

- 自动化的资源清理机制

> 这个系统适合需要自然语言交互式数据分析的场景，用户可以用普通英语提问，AI 会自动转换为 Python 代码分析数据文件并返回结果。相比前一个技术支持代理，这个代理更专注于数据计算和分析能力。

```
Category,Cost
Accommodation, 674.56
Transportation, 2301.00
Meals, 267.89
Misc., 34.50

Uploaded data.txt
Using agent: data-agent
Enter a prompt (or type 'quit' to exit): What's the category with the highest cost?
Last Message: The category with the highest cost is **Transportation**, with a cost of **2301.00**.
Enter a prompt (or type 'quit' to exit): Create a text-based bar chart showing cost by category
Last Message: Here's a text-based bar chart showing the cost by category:

+----------------+--------+------------------------------------------------------------------+

|    Category    |  Cost  |                             Bar                                |

+----------------+--------+------------------------------------------------------------------+

| Transportation | 2301.0 | ****************************************************************|

| Accommodation  | 674.56 | ************************                                       |

|     Meals      | 267.89 | **********                                                    |

|     Misc.      |  34.5  | **                                                           |

+----------------+--------+------------------------------------------------------------------+

```

This chart represents the cost for each category with proportional asterisks (`*`).
Enter a prompt (or type 'quit' to exit): What's the standard deviation of cost?
Last Message: The standard deviation of the costs is approximately **1022.47**.
Enter a prompt (or type 'quit' to exit): quit
Last Message: The standard deviation of the costs is approximately **1022.47**.
```

> 这个输出展示了数据分析 AI 代理的完整工作流程，验证了代码的各项功能。以下是关键点分析：

## 数据加载阶段

- 成功读取并显示了 CSV 格式的支出数据（4 个类别及其金额）

- 文件上传确认显示"Uploaded data.txt"

- 代理初始化成功（Using agent: `data-agent`）

## 自然语言交互分析

### 最高支出查询

- 用户提问："What's the category with the highest cost？"

- 代理正确识别 **Transportation** 为最高（**2301.00**）

- 证明代码解释器能理解数据结构并执行比较操作

### 文本图表生成

- 用户请求："Create a text-based bar chart..."

- 代理生成 ASCII 柱状图，用星号数量直观显示金额比例

- 显示 Transportation 的星号数量明显多于其他类别

- 验证了 `CodeInterpreterTool` 的 Python 代码生成能力

### 统计分析

- 用户提问标准差计算

- 代理返回"1022.47"的合理结果

- 表明能执行 `numpy`/`pandas` 等数学运算

## 技术实现验证

- 文件处理：确认数据文件被正确解析

- 代码解释器：成功执行了：

  - 数据排序（最高支出判断）

  - 可视化（ASCII 图表生成）

  - 统计分析（标准差计算）

- 对话管理：保持多轮对话上下文

## 典型应用场景

- 财务数据分析（自动统计/可视化）

- 运营报表生成（自然语言驱动）

- 教育演示（交互式数据分析教学）

> 输出结果完美对应了代码设计的所有关键功能：

- 文件上传（`upload_and_poll`）

- 代码解释（`CodeInterpreterTool`）

- 动态响应（`create_and_process`）

- 多轮对话（`thread` 维护）

特别是 ASCII 图表的生成，证明了代理不仅能计算原始数据，还能用创造性的方式呈现结果，远超简单的数据查询功能。标准差的计算结果也验证了复杂统计功能的实现。

# 将自定义工具集成到代理中

https://learn.microsoft.com/zh-cn/training/modules/build-agent-with-custom-tools/

## 什么是 Azure AI Foundry 代理服务中的自定义工具？

Azure AI Foundry 代理服务中的自定义工具是允许 AI 代理扩展其功能并执行特定任务的功能。它们通过自动化重复性工作、提供定制解决方案以及提高输出的准确性和一致性来显著提高工作效率。

## 使用自定义工具的主要优势是什么？

使用自定义工具的主要优势包括：
