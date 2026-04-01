# Claude Agent SDK 完整指南：把AI编程能力装进你的程序里

> 来源：[https://my.feishu.cn/docx/E8tHd2XKio8ni7xxhqccudxunEe](https://my.feishu.cn/docx/E8tHd2XKio8ni7xxhqccudxunEe)

# Claude Agent SDK 完整指南：把AI编程能力装进你的程序里

> **课程信息**

+   **作者**：老金

+   **预计学时**：6-8小时

+   **难度等级**：⭐⭐⭐ 中级进阶

+   **更新日期**：2026年2月

+   **适用版本**：Claude Code v2.1+（验证于2026-02-25）

+   **信息来源**：[官方文档](https://docs.anthropic.com/en/docs/claude-code/sdk)、GitHub Python SDK、GitHub TypeScript SDK

> **版本说明**：Claude Agent SDK（原名Claude Code SDK）是2025年Anthropic官方发布的开发工具包。本教程基于最新官方文档编写，如有更新请以官方为准。

---

## 本课学习目标

完成本课学习后，你将能够：

1.  **理解SDK的核心价值**：掌握Agent SDK与Claude Code CLI的关系和定位

1.  **5分钟快速上手**：完成Hello World Agent的创建和运行

1.  **掌握核心概念**：深入理解Tools、Agent Loop、Context Management

1.  **独立完成3个实战项目**：从one-shot到multi-tool再到streaming模式

1.  **自定义工具开发**：创建SDK内置MCP服务器，扩展Agent能力

1.  **故障排查技能**：独立解决90%的常见问题

---

## 学习路径导航（先看这里！）

> 这是一篇长教程，不用全看！根据你的目标选择路径。

### 路径A：快速体验（60分钟）

**适合人群**：急着体验Agent SDK，想快速跑个Demo

**只看这些章节**：

```
术语表（5分钟）
第一部分：SDK简介（10分钟）
第二部分：5分钟快速开始（20分钟）
实战项目1：Hello World Agent（25分钟）
```

**60分钟后你能达到**：成功运行第一个Agent，理解基本工作流程

---

### 路径B：系统学习（6-8小时）

**适合人群**：想深入掌握Agent SDK，准备在项目中使用

**学习顺序**：从头到尾所有章节

**建议分段学习**：

+   第1天（2小时）：第一~三部分（概念+安装+核心原理）

+   第2天（2小时）：第四部分（3个实战项目）

+   第3天（2小时）：第五~六部分（高级功能+故障排查）

+   第4天（1小时）：第七部分FAQ + 总复习

---

### 路径C：问题速查（5分钟）

**适合人群**：使用中遇到问题，需要快速解决

**直接跳到**：

```
第六部分：故障排查 - 按错误类型查找解决方案
第七部分：FAQ - 20个精选问题
```

---

### 路径D：专项学习（30-60分钟/主题）

**适合人群**：已经会基础，想深入某个功能

| 想学什么 | 看哪几节 | 预计时间 |
| --- | --- | --- |
| **自定义工具** | 第三部分3.3节 + 第四部分项目3 | 60分钟 |
| **Hooks钩子** | 第五部分5.2节 | 45分钟 |
| **子代理并行** | 第三部分3.4节 + 第四部分项目3 | 60分钟 |
| **MCP集成** | 第五部分5.1节 | 45分钟 |

---

## 术语表（小白必读）

**在开始之前，先了解这些关**键术语。不理解术语会影响后续学习！

| 术语 | 英文全称 | 通俗解释 |
| --- | --- | --- |
| **SDK** | Software Development Kit | 软件开发工具包，就像一个&quot;工具箱&quot;，里面有各种你需要的零件和工具，让你能更方便地造东西 |
| **Agent** | - | 智能代理/自动机器人，能够自主完成任务的AI程序。就像一个会思考的机器人助手，你给它任务，它自己想办法完成 |
| **Agent Loop** | - | 代理循环，Agent工作的核心流程：接收任务→思考→使用工具→检查结果→继续或结束。就像一个&quot;工作循环&quot; |
| **Tool** | - | 工具，Agent可以调用的能力，比如读文件、写文件、执行命令等。就像机器人的&quot;手&quot;和&quot;眼睛&quot; |
| **Hook** | - | 钩子，在特定时机自动触发的回调函数。就像设置的&quot;自动报警器&quot;，特定事件发生时自动执行 |
| **MCP** | Model Context Protocol | 模型上下文协议，让Agent连接外部工具的标准协议。就像USB接口标准，让各种设备能统一接入 |
| Stream/Streaming | - | 流式传输，数据像流水一样一点点传过来，而不是等全部完成才一次性传输。就像看视频边加载边播放 |
| **Async/Await** | - | 异步编程语法，让程序不用傻等，可以同时处理多件事。就像你等外卖时还能看手机，不用傻站着 |
| **Iterator** | - | 迭代器，一种可以逐个获取数据的对象。就像自动发牌机，每次给你一张牌 |
| **Context** | - | 上下文，AI理解对话的背景信息。就像聊天记录，AI通过它知道你之前说了什么 |
| **Token** | - | 计费单位，AI处理文字的最小单元。约等于0.75个英文单词或1-2个汉字 |
| **Subagent** | - | 子代理，由主Agent创建的子任务执行者。就像主管派出去干活的员工 |

---

## 第一部分：Claude Agent SDK 简介

### 1.1 什么是 Claude Agent SDK

**用一句话说**：Claude Agent SDK是Anthropic官方提供的开发工具包，让你能够在自己的Python或TypeScript程序中使用Claude Code的全部能力。

**生活类比**：

想象Claude Code是一辆装配完整的汽车，你可以开着它到处跑。而Agent SDK就是这辆车的发动机、变速箱和底盘的组合包，让你能够打造属于自己的定制车型。

```
┌─────────────────────────────────────────────────┐
│                 你的应用程序                      │
│  ┌──────────────────────────────────────────┐   │
│  │           Claude Agent SDK               │   │
│  │  ┌────────────────────────────────────┐  │   │
│  │  │       Claude Code核心引擎           │  │   │
│  │  │  - 文件读写能力                      │  │   │
│  │  │  - Bash命令执行                      │  │   │
│  │  │  - 代码分析与生成                    │  │   │
│  │  │  - 网络搜索                          │  │   │
│  │  │  - MCP工具调用                       │  │   │
│  │  └────────────────────────────────────┘  │   │
│  └──────────────────────────────────────────┘   │
└─────────────────────────────────────────────────┘
```

### 1.2 SDK vs CLI：什么时候用哪个？

Claude Code有两种使用方式，选择取决于你的场景：

```
                    Claude代理核心能力
                          │
          ┌───────────────┴───────────────┐
          │                               │
    Claude Code CLI                 Claude Agent SDK
    （命令行工具）                    （编程接口）
          │                               │
    ┌─────┴─────┐                 ┌───────┴───────┐
    │           │                 │               │
  人工交互   Shell脚本          Python程序     TypeScript程序
```

**对比表**：

| 特性 | Claude Code CLI | Claude Agent SDK |
| --- | --- | --- |
| **使用方式** | 在终端里输入命令 | 在代码里调用函数 |
| **适合场景** | 日常开发、快速任务 | 自动化、批处理、集成到系统 |
| **交互方式** | 人工对话 | 程序控制 |
| **并发能力** | 单会话 | 多代理并行 |
| **定制程度** | 配置文件 | 完全可编程 |
| **学习曲线** | 低（会打字就行） | 中（需要编程基础） |

**什么时候用SDK？**

1.  **自动化脚本**：每天自动审查代码、生成报告

1.  **批量处理**：一次处理100个文件的重构

1.  **系统集成**：把Claude嵌入你的CI/CD流程

1.  **多Agent协作**：让多个Agent并行工作

1.  **定制化工具**：构建特定领域的AI助手

**什么时候用CLI？**

1.  **日常开发**：写代码、改bug、问问题

1.  **快速任务**：临时需要AI帮忙的小事

1.  **学习探索**：熟悉Claude的能力

1.  **不想写代码**：就想简单聊聊

### 1.3 支持的编程语言

Agent SDK目前官方支持两种语言，选择你熟悉的即可：

**Python SDK**

```
# 安装命令
pip install claude-agent-sdk

# 特点
- Python 3.10+ 支持
- 异步迭代器（async for）
- 类型提示完善
- 适合：数据科学、AI/ML工作流、自动化脚本
```

TypeScript/JavaScript SDK

```
# 安装命令
npm install @anthropic-ai/claude-agent-sdk

# 特点
- Node.js 18+ 支持
- 原生异步生成器
- 完整类型定义
- 适合：Web应用、Node.js服务端、全栈开发
```

> **老金建议**：两种SDK功能完全相同，选你最熟悉的语言就好。本教程会同时展示两种语言的代码示例。

### 1.4 版本演进说明

Claude Agent SDK经历了从"Claude Code SDK"到"Agent SDK"的品牌升级：

| 版本阶段 | 包名 | 状态 |
| --- | --- | --- |
| 早期 | @anthropic-ai/claude-code / claude-code-sdk | 已弃用 |
| 当前 | @anthropic-ai/claude-agent-sdk / claude-agent-sdk | 推荐使用 |

**如果你之前用旧版SDK**，需要更新导入语句：

```
# Python旧版（已弃用）
from claude_code_sdk import query, ClaudeCodeOptions

# Python新版（推荐）
from claude_agent_sdk import query, ClaudeAgentOptions
```

```
// TypeScript旧版（已弃用）
import { query } from &#x27;@anthropic-ai/claude-code&#x27;;

// TypeScript新版（推荐）
import { query } from &#x27;@anthropic-ai/claude-agent-sdk&#x27;;
```

---

## 第二部分：5分钟快速开始

### 2.1 环境准备

**系统要求**：

| 要求项 | 最低版本 | 推荐版本 |
| --- | --- | --- |
| **Node.js**（TypeScript用） | 18.0.0 | 20.x LTS |
| **Python**（Python用） | 3.10 | 3.11+ |
| **操作系统** | Windows 10/macOS 12/Ubuntu 20.04 | 最新稳定版 |

**快速检查命令**：

```
# 检查Node.js版本
node --version
# 应显示 v18.0.0 或更高

# 检查Python版本
python --version
# 应显示 Python 3.10 或更高

# 检查npm版本
npm --version
# 应显示 8.0.0 或更高
```

### 2.2 安装SDK

**Python安装（推荐方式）**：

```
# 步骤1：创建虚拟环境（推荐）
python -m venv venv

# 步骤2：激活虚拟环境
# Windows:
venv\Scripts\activate
# macOS/Linux:
source venv/bin/activate

# 步骤3：安装Agent SDK
pip install claude-agent-sdk
```

> **重要提示**：Python SDK会自动捆绑Claude Code CLI，无需单独安装！

**TypeScript安装**：

```
# 步骤1：创建项目目录
mkdir my-agent-project
cd my-agent-project

# 步骤2：初始化项目
npm init -y

# 步骤3：安装Agent SDK
npm install @anthropic-ai/claude-agent-sdk

# 步骤4：安装TypeScript开发依赖
npm install -D typescript @types/node ts-node
```

### 2.3 配置API密钥

Agent SDK需要Anthropic API密钥才能运行。

**方式一：环境变量（推荐）**

```
# Linux/macOS
export ANTHROPIC_API_KEY=&quot;sk-ant-api03-...&quot;

# Windows PowerShell
$env:ANTHROPIC_API_KEY=&quot;sk-ant-api03-...&quot;

# Windows CMD
set ANTHROPIC_API_KEY=sk-ant-api03-...
```

**方式二：.env文件**

创建.env文件：

```
ANTHROPIC_API_KEY=sk-ant-api03-...
```

在代码中加载：

```
# Python
from dotenv import load_dotenv
load_dotenv()
```

```
// TypeScript
import &#x27;dotenv/config&#x27;;
```

### 2.4 Hello World Agent

现在让我们创建第一个Agent！

**Python版本** - 创建hello_agent.py：

```
import asyncio
from claude_agent_sdk import query, ClaudeAgentOptions

async def main():
    print(&#x27;启动Agent...&#x27;)

    # 配置选项
    options = ClaudeAgentOptions(
        model=&#x27;claude-sonnet-4-6&#x27;  # Sonnet 4.6（推荐模型）
    )

    # 执行查询，返回流式响应
    async for message in query(
        prompt=&#x27;你好！请用一句话介绍你能做什么？&#x27;,
        options=options
    ):
        # 处理助手回复
        if message.type == &#x27;assistant&#x27;:
            for block in message.content:
                if block.type == &#x27;text&#x27;:
                    print(block.text, end=&#x27;&#x27;, flush=True)

    print(&#x27;\n\nAgent任务完成！&#x27;)

if __name__ == &#x27;__main__&#x27;:
    asyncio.run(main())
```

**运行**：

```
python hello_agent.py
```

**预期输出**：

```
启动Agent...
我是Claude，一个AI助手，能帮你编写代码、分析文件、执行命令和解答各种技术问题。

Agent任务完成！
```

---

**TypeScript版本** - 创建src/hello-agent.ts：

```
import { query, type Query } from &#x27;@anthropic-ai/claude-agent-sdk&#x27;;

async function main() {
  console.log(&#x27;启动Agent...&#x27;);

  // 创建查询
  const stream: Query = query({
    prompt: &#x27;你好！请用一句话介绍你能做什么？&#x27;,
    options: {
      model: &#x27;claude-sonnet-4-6&#x27;  // Sonnet 4.6(推荐)
    }
  });

  // 处理流式响应
  for await (const message of stream) {
    if (message.type === &#x27;assistant&#x27;) {
      for (const block of message.message.content) {
        if (block.type === &#x27;text&#x27;) {
          process.stdout.write(block.text);
        }
      }
    }
  }

  console.log(&#x27;\n\nAgent任务完成！&#x27;);
}

main().catch(console.error);
```

**运行**：

```
npx ts-node src/hello-agent.ts
```

### 2.5 验证安装成功

如果你看到了Agent的回复，恭喜！安装成功！

**常见问题快速排查**：

| 错误信息 | 原因 | 解决方案 |
| --- | --- | --- |
| ModuleNotFoundError: No module named &#x27;claude_agent_sdk&#x27; | SDK没装好 | 重新运行 pip install claude-agent-sdk |
| Cannot find module &#x27;@anthropic-ai/claude-agent-sdk&#x27; | npm包没装好 | 重新运行 npm install |
| Authentication failed | API Key没配置 | 检查环境变量是否设置正确 |
| Connection timeout | 网络问题 | 检查网络连接，国内可能需要代理 |

---

## 第三部分：核心概念深度解析

### 3.1 query函数：SDK的核心入口

query函数是Agent SDK的心脏，理解它就理解了SDK的一半。

**函数签名**：

```
# Python
async def query(
    prompt: str,
    options: Optional[ClaudeAgentOptions] = None
) -&gt; AsyncIterator[SDKMessage]:
```

```
// TypeScript
function query(params: {
  prompt: string | AsyncIterable&lt;SDKUserMessage&gt;;
  options?: Options;
}): Query;
```

**核心参数说明**：

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| prompt | string | 是 | 发送给Agent的提示词/任务描述 |
| options | Options对象 | 否 | Agent的配置选项 |

**Options配置项详解**：

```
# Python完整配置示例
from claude_agent_sdk import ClaudeAgentOptions

options = ClaudeAgentOptions(
    # 模型选择
    model=&#x27;claude-sonnet-4-6&#x27;,  # Sonnet 4.6(推荐),也可选Opus 4.6或Haiku 4.5

    # 系统提示词（定义Agent的角色和行为）
    system_prompt=&#x27;你是一个Python专家，专注于代码质量和最佳实践。&#x27;,

    # 工作目录（Agent操作文件的根目录）
    cwd=&#x27;/path/to/your/project&#x27;,

    # 最大对话轮次（防止无限循环）
    max_turns=10,

    # 权限模式
    permission_mode=&#x27;default&#x27;,  # 可选：default, acceptEdits, bypassPermissions

    # 允许使用的工具
    allowed_tools=[&#x27;Read&#x27;, &#x27;Write&#x27;, &#x27;Edit&#x27;, &#x27;Bash&#x27;, &#x27;Glob&#x27;, &#x27;Grep&#x27;],

    # MCP服务器配置（扩展工具）
    mcp_servers={
        &#x27;my_server&#x27;: {
            &#x27;command&#x27;: &#x27;python&#x27;,
            &#x27;args&#x27;: [&#x27;-m&#x27;, &#x27;my_mcp_server&#x27;]
        }
    }

)
```

**返回值：流式消息**

query返回一个异步迭代器，产生多种类型的消息：

```
# 消息类型
SDKMessage = Union[
    AssistantMessage,   # AI的回复
    UserMessage,        # 用户的输入
    ToolUseBlock,       # 工具调用请求
    ToolResultBlock,    # 工具执行结果
    ResultMessage,      # 最终结果
]
```

**处理消息的标准模式**：

```
async for message in query(prompt=&quot;你的任务&quot;, options=options):
    if isinstance(message, AssistantMessage):
        # 处理AI回复
        for block in message.content:
            if isinstance(block, TextBlock):
                print(block.text)
            elif isinstance(block, ToolUseBlock):
                print(f&quot;调用工具: {block.name}&quot;)

    elif isinstance(message, ResultMessage):
        # 任务完成
        print(f&quot;最终结果: {message.result}&quot;)
```

### 3.2 Agent Loop：代理的工作循环

Agent不是简单地"问一句答一句"，而是有一个完整的工作循环。

**Agent Loop工作流程**：

```
┌──────────────────────────────────────────────────────┐
│                    Agent Loop                         │
│                                                       │
│   ┌─────────┐    ┌─────────┐    ┌─────────┐         │
│   │ 接收任务 │ → │  思考   │ → │ 决定行动 │         │
│   └─────────┘    └─────────┘    └─────────┘         │
│                                      │               │
│                                      ▼               │
│                              ┌───────────────┐       │
│                              │ 需要使用工具？ │       │
│                              └───────────────┘       │
│                                 │       │            │
│                              是 │       │ 否         │
│                                 ▼       ▼            │
│   ┌─────────┐    ┌─────────┐   │   ┌─────────┐     │
│   │ 检查结果 │ ← │ 执行工具 │←──┘   │ 直接回复 │     │
│   └─────────┘    └─────────┘       └─────────┘     │
│        │                                │            │
│        ▼                                │            │
│  ┌───────────┐                          │            │
│  │ 任务完成？ │                          │            │
│  └───────────┘                          │            │
│     │      │                            │            │
│  否 │      │ 是                         │            │
│     │      ▼                            ▼            │
│     │  ┌─────────┐              ┌─────────┐         │
│     │  │ 返回结果 │              │ 返回结果 │         │
│     │  └─────────┘              └─────────┘         │
│     │                                               │
│     └──────────────→ 继续循环 ──────────────────────→│
│                                                       │
└──────────────────────────────────────────────────────┘
```

**实际例子**：让Agent"创建一个Python计算器"

```
轮次1：
  - Agent思考：需要创建文件
  - 使用工具：Write写入calculator.py
  - 结果：文件创建成功
  - 判断：任务未完成，需要测试

轮次2：
  - Agent思考：需要验证代码是否正确
  - 使用工具：Bash执行python calculator.py
  - 结果：运行成功
  - 判断：任务完成

轮次3：
  - Agent回复：已成功创建计算器，代码如下...
```

**控制Agent Loop的参数**：

```
options = ClaudeAgentOptions(
    max_turns=10,  # 最多10轮，防止无限循环
    # 如果任务简单，可以设小一点节省token
)
```

### 3.3 Tools：Agent的能力来源

Tools（工具）是Agent能够"做事"的关键。没有工具，Agent只能"说话"不能"干活"。

**内置工具清单**：

| 工具名 | 功能 | 使用场景 |
| --- | --- | --- |
| **Read** | 读取文件内容 | 分析代码、读取配置 |
| **Write** | 写入文件内容 | 创建新文件 |
| **Edit** | 编辑文件 | 修改现有代码 |
| **Bash** | 执行命令 | 运行脚本、安装依赖 |
| **Glob** | 文件模式匹配 | 查找特定类型的文件 |
| **Grep** | 搜索文件内容 | 在代码中查找关键词 |
| **Task** | 创建子代理 | 并行处理复杂任务 |
| **WebSearch** | 网络搜索 | 查找最新信息 |

**限制工具权限**：

```
# 只允许读取和搜索，禁止修改
options = ClaudeAgentOptions(
    allowed_tools=[&#x27;Read&#x27;, &#x27;Glob&#x27;, &#x27;Grep&#x27;],  # 只读权限
)

# 允许所有工具（默认）
options = ClaudeAgentOptions(
    allowed_tools=None,  # 或者不设置这个参数
)
```

自定义工具（SDK MCP Server）：

这是Agent SDK的强大功能之一！你可以创建自己的工具：

```
from claude_agent_sdk import tool, create_sdk_mcp_server, ClaudeAgentOptions, ClaudeSDKClient

# 使用@tool装饰器定义工具
@tool(&quot;greet&quot;, &quot;向用户问好&quot;, {&quot;name&quot;: str})
async def greet_user(args):
    name = args.get(&#x27;name&#x27;, &#x27;朋友&#x27;)
    return {
        &quot;content&quot;: [
            {&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: f&quot;你好，{name}！很高兴认识你！&quot;}
        ]
    }

@tool(&quot;calculate&quot;, &quot;执行数学计算&quot;, {&quot;expression&quot;: str})
async def calculate(args):
    expression = args.get(&#x27;expression&#x27;, &#x27;0&#x27;)
    try:
        result = eval(expression)  # 注意：生产环境要做安全检查！
        return {&quot;content&quot;: [{&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: f&quot;计算结果：{result}&quot;}]}
    except Exception as e:
        return {&quot;content&quot;: [{&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: f&quot;计算错误：{str(e)}&quot;}]}

# 创建SDK MCP服务器
my_tools = create_sdk_mcp_server(
    name=&quot;my-tools&quot;,
    version=&quot;1.0.0&quot;,
    tools=[greet_user, calculate]
)

# 在Agent中使用
options = ClaudeAgentOptions(
    mcp_servers={&quot;tools&quot;: my_tools},
    allowed_tools=[&quot;mcp__tools__greet&quot;, &quot;mcp__tools__calculate&quot;]
)
```

### 3.4 Subagents：子代理并行处理

当任务太复杂或太大时，单个Agent可能会遇到上下文窗口限制。Subagents（子代理）解决了这个问题。

**类比理解**：

```
传统方式（一个人干所有活）：
┌────────────────────────────────────────────┐
│ 单个Agent顺序处理                           │
│                                            │
│  处理api/* → 处理models/* → 处理views/*    │
│      2小时       2小时         2小时        │
│                                            │
│  总耗时：6小时                              │
└────────────────────────────────────────────┘

Subagents方式（主管+多员工）：
┌────────────────────────────────────────────┐
│ 主Agent分配任务，子Agent并行执行             │
│                                            │
│  子Agent1: api/*     ──┐                   │
│  子Agent2: models/*  ──┼──→ 汇总结果        │
│  子Agent3: views/*   ──┘                   │
│      各2小时（同时进行）                     │
│                                            │
│  总耗时：约2.5小时                          │
└────────────────────────────────────────────┘
```

**核心优势**：

| 优势 | 说明 |
| --- | --- |
| **独立上下文** | 每个子代理有独立的200K上下文窗口 |
| **并行执行** | 最多10个子代理同时运行 |
| **专业化分工** | 每个子代理可以有独立的系统提示词 |
| **失败隔离** | 单个子代理失败不影响其他子代理 |

**使用Subagents的方式**：

```
# 允许Task工具，Agent会自动创建子代理
options = ClaudeAgentOptions(
    allowed_tools=[&#x27;Task&#x27;, &#x27;TaskOutput&#x27;, &#x27;Read&#x27;, &#x27;Write&#x27;, &#x27;Edit&#x27;],
    max_turns=50,
    system_prompt=&quot;&quot;&quot;你是一个项目重构专家。
    当遇到大型任务时，请使用Task工具创建子代理来并行处理。
    每个子代理应该专注于一个独立的模块或目录。&quot;&quot;&quot;
)

prompt = &quot;&quot;&quot;请重构以下目录的代码：
1. src/api/ - 将CommonJS改为ES Module
2. src/models/ - 添加TypeScript类型
3. src/views/ - 优化组件结构

请使用子代理并行处理这三个目录。&quot;&quot;&quot;

async for message in query(prompt=prompt, options=options):
    # 处理消息...
```

### 3.5 Task 工具与子代理编排（CLI 内置）

> **补充说明**：上面 3.4 节讲的是 SDK 中如何编程创建子代理。这里**介绍 Claude C**ode CLI 中内置的 Task 工具——你在交互模式下就能直接使用的多 Agent 编排系统。

**Claude **Code CLI 内置了强大的 Task 工具，无需写代码就能让 Claude 自动创建子代理：

**核心参数**：

| 参数 | 说明 |
| --- | --- |
| subagent_type | 子代理类型（Explore、Bash、code-reviewer 等） |
| prompt | 分配给子代理的任务描述 |
| isolation | 设为 &quot;worktree&quot; 可在独立工作树中运行 |
| run_in_background | 设为 true 在后台运行 |
| resume | 传入之前的 agent ID 可恢复中断的子代理 |
| model | 可指定 sonnet/opus/haiku，不指定则继承父级 |

**常用子代理类型**：

| 类型 | 用途 | 可用工具 |
| --- | --- | --- |
| Explore | 快速搜索代码库（Haiku 驱动，速度最快） | Read、Grep、Glob |
| Bash | 执行终端命令、git 操作 | Bash |
| code-reviewer | 代码审查 | Read、Grep、Glob、Bash |
| general-purpose | 通用多步骤任务 | 全部工具 |
| Plan | 设计实现方案 | 全部（不含写入） |

**任务依赖（DAG 系统）**：

Task 工具支持有向无环图（DAG）依赖关系——任务 C 可以等待任务 A 和任务 B 完成后再执行：

```
任务A: 构建API   ──┐
                    ├──→ 任务C: 运行集成测试
任务B: 配置认证  ──┘
```

**子代理上下文注入**：

子代理获取上下文的三种方式：

1.  **Skills 注入**：在定义中指定 skills 字段，完整技能内容直接注入子代理

1.  **Memory 持久化**：子代理可拥有持久记忆目录，跨会话积累知识

1.  **工具访问控制**：通过 allowedTools / disallowedTools 精细控制子代理能力

> 💡 提**示：**子代理不能再创建子代理（只有一层嵌套），最多可同时运行 10 个并行子代理。

---

## 第四部分：实战项目

### 项目1：代码分析Agent（One-shot模式）

**目标**：创建一个能分析代码文件并给出改进建议的Agent

**难度**：⭐ 入门

**Python实现** - code_analyzer.py：

```
&quot;&quot;&quot;
代码分析Agent - 分析指定文件的代码质量
&quot;&quot;&quot;
import asyncio
import sys
from pathlib import Path
from claude_agent_sdk import query, ClaudeAgentOptions, AssistantMessage, TextBlock

async def analyze_code(file_path: str) -&gt; None:
    &quot;&quot;&quot;分析指定文件的代码质量&quot;&quot;&quot;

    # 检查文件是否存在
    path = Path(file_path)
    if not path.exists():
        print(f&quot;错误：文件 {file_path} 不存在&quot;)
        return

    print(f&quot;分析文件: {path.absolute()}&quot;)
    print(&quot;=&quot; * 50)

    # 配置Agent
    options = ClaudeAgentOptions(
        model=&#x27;claude-sonnet-4-6&#x27;,
        cwd=str(Path.cwd()),
        allowed_tools=[&#x27;Read&#x27;, &#x27;Glob&#x27;, &#x27;Grep&#x27;],  # 只给读取权限
        max_turns=5,
        system_prompt=&quot;&quot;&quot;你是一位资深代码审查专家。
        请分析代码并提供：
        1. 代码概述（功能和结构）
        2. 潜在问题（bug风险、安全隐患）
        3. 改进建议（可读性、性能、最佳实践）

        请使用中文回复，格式清晰。&quot;&quot;&quot;
    )

    prompt = f&quot;&quot;&quot;请分析以下文件的代码质量：{path.absolute()}

    使用Read工具读取文件内容，然后给出详细分析。&quot;&quot;&quot;

    # 执行分析
    async for message in query(prompt=prompt, options=options):
        if isinstance(message, AssistantMessage):
            for block in message.content:
                if isinstance(block, TextBlock):
                    print(block.text, end=&#x27;&#x27;, flush=True)

    print(&quot;\n&quot; + &quot;=&quot; * 50)
    print(&quot;分析完成！&quot;)

def main():
    # 从命令行参数获取文件路径
    if len(sys.argv) &lt; 2:
        print(&quot;用法: python code_analyzer.py &lt;文件路径&gt;&quot;)
        print(&quot;示例: python code_analyzer.py ./my_code.py&quot;)
        return

    file_path = sys.argv[1]
    asyncio.run(analyze_code(file_path))

if __name__ == &#x27;__main__&#x27;:
    main()
```

**使用方法**：

```
# 分析Python文件
python code_analyzer.py ./my_script.py

# 分析JavaScript文件
python code_analyzer.py ./app.js
```

---

### 项目2：多工具Agent（Multi-tool模式）

**目标**：创建一个能读写文件、执行命令的全能Agent

**难度**：⭐⭐ 进阶

**Python实现** - multi_tool_agent.py：

```
&quot;&quot;&quot;
多工具Agent - 可以读写文件、执行命令的全能助手
&quot;&quot;&quot;
import asyncio
from pathlib import Path
from claude_agent_sdk import (
    query,
    ClaudeAgentOptions,
    AssistantMessage,
    TextBlock,
    ToolUseBlock,
    ToolResultBlock
)

async def run_multi_tool_agent(task: str) -&gt; None:
    &quot;&quot;&quot;运行多工具Agent执行任务&quot;&quot;&quot;

    print(f&quot;任务: {task}&quot;)
    print(&quot;=&quot; * 60)

    options = ClaudeAgentOptions(
        model=&#x27;claude-sonnet-4-6&#x27;,  # Sonnet 4.6
        cwd=str(Path.cwd()),
        allowed_tools=[&#x27;Read&#x27;, &#x27;Write&#x27;, &#x27;Edit&#x27;, &#x27;Bash&#x27;, &#x27;Glob&#x27;, &#x27;Grep&#x27;],
        max_turns=15,
        permission_mode=&#x27;acceptEdits&#x27;,  # 自动接受文件编辑
        system_prompt=&quot;&quot;&quot;你是一个全能开发助手，可以：
        - 读取和分析文件
        - 创建和修改文件
        - 执行命令行命令
        - 搜索文件内容

        执行任务时：
        1. 先分析需要做什么
        2. 分步骤执行
        3. 每步都验证结果
        4. 最后总结完成情况

        使用中文回复。&quot;&quot;&quot;
    )

    # 执行任务
    async for message in query(prompt=task, options=options):
        if isinstance(message, AssistantMessage):
            for block in message.content:
                if isinstance(block, TextBlock):
                    print(block.text, end=&#x27;&#x27;, flush=True)
                elif isinstance(block, ToolUseBlock):
                    print(f&quot;\n[使用工具: {block.name}]&quot;)

        elif isinstance(message, ToolResultBlock):
            print(f&quot;[工具执行完成]\n&quot;)

    print(&quot;\n&quot; + &quot;=&quot; * 60)
    print(&quot;任务执行完成！&quot;)


async def main():
    # 示例任务列表
    tasks = [
        &quot;创建一个名为hello.py的文件，内容是打印&#x27;Hello, Agent SDK!&#x27;，然后运行它&quot;,
        &quot;查找当前目录下所有的.py文件，列出它们的名称&quot;,
        &quot;创建一个简单的README.md文件，描述这是一个Agent SDK演示项目&quot;,
    ]

    print(&quot;可用任务:&quot;)
    for i, task in enumerate(tasks, 1):
        print(f&quot;  {i}. {task}&quot;)
    print()

    # 选择任务
    choice = input(&quot;输入任务编号（1-3）或自定义任务: &quot;).strip()

    if choice in [&#x27;1&#x27;, &#x27;2&#x27;, &#x27;3&#x27;]:
        task = tasks[int(choice) - 1]
    else:
        task = choice

    await run_multi_tool_agent(task)

if __name__ == &#x27;__main__&#x27;:
    asyncio.run(main())
```

**运行示例**：

```
python multi_tool_agent.py

# 输出：
# 可用任务:
#   1. 创建一个名为hello.py的文件...
#   2. 查找当前目录下所有的.py文件...
#   3. 创建一个简单的README.md文件...
#
# 输入任务编号（1-3）或自定义任务: 1
```

---

### 项目3：流式交互Agent（Streaming + 自定义工具）

**目标**：创建一个带有自定义工具的交互式Agent

**难度**：⭐⭐⭐ 高级

**Python实现** - interactive_agent.py：

```
&quot;&quot;&quot;
交互式Agent - 带有自定义工具的流式对话Agent
&quot;&quot;&quot;
import asyncio
from datetime import datetime
from claude_agent_sdk import (
    tool,
    create_sdk_mcp_server,
    ClaudeAgentOptions,
    ClaudeSDKClient,
    AssistantMessage,
    TextBlock,
    ToolUseBlock
)

# ============ 自定义工具定义 ============

@tool(&quot;get_time&quot;, &quot;获取当前时间&quot;, {})
async def get_current_time(args):
    &quot;&quot;&quot;返回当前时间&quot;&quot;&quot;
    now = datetime.now()
    return {
        &quot;content&quot;: [{
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: f&quot;当前时间：{now.strftime(&#x27;%Y-%m-%d %H:%M:%S&#x27;)}&quot;
        }]
    }

@tool(&quot;calculate&quot;, &quot;执行数学计算&quot;, {
    &quot;expression&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;数学表达式，如 2+2, 10*5&quot;}
})
async def calculate(args):
    &quot;&quot;&quot;执行数学计算&quot;&quot;&quot;
    expression = args.get(&#x27;expression&#x27;, &#x27;0&#x27;)
    try:
        # 安全的数学计算（只允许基本运算）
        allowed_chars = set(&#x27;0123456789+-*/().% &#x27;)
        if not all(c in allowed_chars for c in expression):
            return {&quot;content&quot;: [{&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: &quot;错误：表达式包含不允许的字符&quot;}]}

        result = eval(expression)
        return {&quot;content&quot;: [{&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: f&quot;{expression} = {result}&quot;}]}
    except Exception as e:
        return {&quot;content&quot;: [{&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: f&quot;计算错误：{str(e)}&quot;}]}

@tool(&quot;todo_add&quot;, &quot;添加待办事项&quot;, {
    &quot;item&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;待办事项内容&quot;}
})
async def add_todo(args):
    &quot;&quot;&quot;添加待办事项&quot;&quot;&quot;
    item = args.get(&#x27;item&#x27;, &#x27;&#x27;)
    if not item:
        return {&quot;content&quot;: [{&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: &quot;错误：待办事项不能为空&quot;}]}

    # 这里可以连接数据库，简化起见用文件存储
    with open(&#x27;todos.txt&#x27;, &#x27;a&#x27;, encoding=&#x27;utf-8&#x27;) as f:
        f.write(f&quot;[ ] {item}\n&quot;)

    return {&quot;content&quot;: [{&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: f&quot;已添加待办事项：{item}&quot;}]}

@tool(&quot;todo_list&quot;, &quot;列出所有待办事项&quot;, {})
async def list_todos(args):
    &quot;&quot;&quot;列出待办事项&quot;&quot;&quot;
    try:
        with open(&#x27;todos.txt&#x27;, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as f:
            todos = f.read().strip()

        if not todos:
            return {&quot;content&quot;: [{&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: &quot;待办事项列表为空&quot;}]}

        return {&quot;content&quot;: [{&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: f&quot;待办事项列表：\n{todos}&quot;}]}
    except FileNotFoundError:
        return {&quot;content&quot;: [{&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: &quot;待办事项列表为空&quot;}]}

# ============ Agent配置 ============

# 创建自定义工具服务器
custom_tools = create_sdk_mcp_server(
    name=&quot;custom-tools&quot;,
    version=&quot;1.0.0&quot;,
    tools=[get_current_time, calculate, add_todo, list_todos]
)

# Agent配置
def get_options():
    return ClaudeAgentOptions(
        model=&#x27;claude-sonnet-4-6&#x27;,  # Sonnet 4.6(推荐)
        mcp_servers={&quot;custom&quot;: custom_tools},
        allowed_tools=[
            &quot;Read&quot;, &quot;Write&quot;, &quot;Bash&quot;,
            &quot;mcp__custom__get_time&quot;,
            &quot;mcp__custom__calculate&quot;,
            &quot;mcp__custom__todo_add&quot;,
            &quot;mcp__custom__todo_list&quot;
        ],
        permission_mode=&#x27;acceptEdits&#x27;,
        system_prompt=&quot;&quot;&quot;你是一个智能助手，可以使用以下工具：

1. **get_time**: 获取当前时间
2. **calculate**: 执行数学计算
3. **todo_add**: 添加待办事项
4. **todo_list**: 列出所有待办事项
5. **Read/Write/Bash**: 文件操作和命令执行

请根据用户需求选择合适的工具。使用中文回复。
当用户说&quot;退出&quot;或&quot;再见&quot;时，友好地告别。&quot;&quot;&quot;
    )

# ============ 交互循环 ============

async def interactive_chat():
    &quot;&quot;&quot;交互式对话循环&quot;&quot;&quot;
    print(&quot;=&quot; * 60)
    print(&quot;欢迎使用交互式Agent！&quot;)
    print(&quot;你可以：&quot;)
    print(&quot;  - 问我现在几点了&quot;)
    print(&quot;  - 让我计算数学题（如：计算 123 * 456）&quot;)
    print(&quot;  - 添加待办事项（如：添加待办 写周报）&quot;)
    print(&quot;  - 查看待办事项列表&quot;)
    print(&quot;  - 让我读写文件或执行命令&quot;)
    print(&quot;输入 &#x27;退出&#x27; 或 &#x27;再见&#x27; 结束对话&quot;)
    print(&quot;=&quot; * 60)
    print()

    async with ClaudeSDKClient(options=get_options()) as client:
        while True:
            # 获取用户输入
            try:
                user_input = input(&quot;你: &quot;).strip()
            except EOFError:
                break

            if not user_input:
                continue

            # 检查退出
            if user_input.lower() in [&#x27;退出&#x27;, &#x27;再见&#x27;, &#x27;exit&#x27;, &#x27;quit&#x27;, &#x27;bye&#x27;]:
                print(&quot;\nAgent: 再见！祝你有愉快的一天！&quot;)
                break

            # 发送消息
            await client.query(user_input)

            # 处理响应
            print(&quot;Agent: &quot;, end=&#x27;&#x27;, flush=True)
            async for message in client.receive_response():
                if isinstance(message, AssistantMessage):
                    for block in message.content:
                        if isinstance(block, TextBlock):
                            print(block.text, end=&#x27;&#x27;, flush=True)
                        elif isinstance(block, ToolUseBlock):
                            print(f&quot;\n[调用工具: {block.name}]&quot;, end=&#x27;&#x27;, flush=True)

            print()  # 换行

async def main():
    await interactive_chat()

if __name__ == &#x27;__main__&#x27;:
    asyncio.run(main())
```

**运行示例**：

```
python interactive_agent.py

# 交互示例：
# ============================================================
# 欢迎使用交互式Agent！
# ...
# ============================================================
#
# 你: 现在几点了？
# Agent: [调用工具: get_time]
# 当前时间是 2025-12-19 15:30:45
#
# 你: 计算 123 * 456
# Agent: [调用工具: calculate]
# 123 * 456 = 56088
#
# 你: 添加待办 写Agent SDK教程
# Agent: [调用工具: todo_add]
# 已添加待办事项：写Agent SDK教程
#
# 你: 退出
# Agent: 再见！祝你有愉快的一天！
```

---

## 第五部分：高级功能

### 5.1 MCP服务器集成

MCP（Model Context Protocol）让你可以连接外部工具和服务。

**外部MCP服务器配置**：

```
from claude_agent_sdk import query, ClaudeAgentOptions

options = ClaudeAgentOptions(
    mcp_servers={
        # 文件系统服务器
        &quot;filesystem&quot;: {
            &quot;command&quot;: &quot;npx&quot;,
            &quot;args&quot;: [&quot;-y&quot;, &quot;@anthropic/mcp-server-filesystem&quot;, &quot;/allowed/path&quot;]
        },

        # SQLite数据库服务器
        &quot;database&quot;: {
            &quot;command&quot;: &quot;npx&quot;,
            &quot;args&quot;: [&quot;-y&quot;, &quot;@anthropic/mcp-server-sqlite&quot;, &quot;./data/app.db&quot;]
        },

        # 自定义Python服务器
        &quot;custom&quot;: {
            &quot;command&quot;: &quot;python&quot;,
            &quot;args&quot;: [&quot;-m&quot;, &quot;my_mcp_server&quot;],
            &quot;env&quot;: {
                &quot;API_KEY&quot;: &quot;your-api-key&quot;
            }
        }
    },
    allowed_tools=[
        &quot;mcp__filesystem__read&quot;,
        &quot;mcp__database__query&quot;,
        &quot;mcp__custom__my_tool&quot;
    ]
)
```

SDK内置MCP服务器 vs 外部服务器：

| 特性 | SDK内置服务器 | 外部服务器 |
| --- | --- | --- |
| **运行方式** | 在你的Python进程内 | 独立子进程 |
| **性能** | 更快（无IPC开销） | 略慢（需要进程通信） |
| **部署** | 简单（单进程） | 需要管理多进程 |
| **调试** | 更容易 | 需要分别调试 |
| **适用场景** | 简单工具 | 复杂工具、现有MCP服务器 |

### 5.2 Hooks：在关键时刻自动执行

Hooks（钩子）让你可以在Agent执行的特定时机插入自定义逻辑。

**可用的Hook类型**：

| Hook类型 | 触发时机 | 用途 |
| --- | --- | --- |
| **PreToolUse** | 工具执行前 | 验证、过滤危险命令 |
| **PostToolUse** | 工具执行后 | 记录日志、自动测试 |
| **Stop** | Agent停止时 | 清理、汇总报告 |
| UserPromptSubmit | 用户提交提示时 | 处理和增强用户输入 |
| **SessionStart** | 会话开始时 | 加载上次会话状态 |
| **SessionEnd** | 会话结束时 | 保存会话状态 |
| **PreCompact** | 上下文压缩前 | 保存关键信息 |

**Python Hooks示例**：

```
from claude_agent_sdk import ClaudeAgentOptions, ClaudeSDKClient, HookMatcher

# 定义Hook函数
async def block_dangerous_commands(input_data, tool_use_id, context):
    &quot;&quot;&quot;阻止危险的Bash命令&quot;&quot;&quot;
    tool_name = input_data.get(&quot;tool_name&quot;)
    tool_input = input_data.get(&quot;tool_input&quot;, {})

    # 只检查Bash命令
    if tool_name != &quot;Bash&quot;:
        return {}

    command = tool_input.get(&quot;command&quot;, &quot;&quot;)

    # 危险命令黑名单
    dangerous_patterns = [&quot;rm -rf&quot;, &quot;sudo rm&quot;, &quot;:(){ :|:&amp; };:&quot;, &quot;dd if=&quot;]

    for pattern in dangerous_patterns:
        if pattern in command:
            return {
                &quot;hookSpecificOutput&quot;: {
                    &quot;hookEventName&quot;: &quot;PreToolUse&quot;,
                    &quot;permissionDecision&quot;: &quot;deny&quot;,
                    &quot;permissionDecisionReason&quot;: f&quot;危险命令被阻止：包含 &#x27;{pattern}&#x27;&quot;
                }
            }

    return {}  # 允许执行

async def log_tool_results(input_data, tool_use_id, context):
    &quot;&quot;&quot;记录工具执行结果&quot;&quot;&quot;
    tool_name = input_data.get(&quot;tool_name&quot;)
    print(f&quot;[日志] 工具 {tool_name} 执行完成&quot;)
    return {}

# 配置Hooks
options = ClaudeAgentOptions(
    allowed_tools=[&quot;Bash&quot;, &quot;Read&quot;, &quot;Write&quot;],
    hooks={
        &quot;PreToolUse&quot;: [
            HookMatcher(matcher=&quot;Bash&quot;, hooks=[block_dangerous_commands]),
        ],
        &quot;PostToolUse&quot;: [
            HookMatcher(matcher=&quot;.*&quot;, hooks=[log_tool_results]),
        ],
    }
)
```

### 5.3 权限控制

Agent SDK提供多种权限控制方式，确保安全。

**权限模式**：

| 模式 | 说明 | 适用场景 |
| --- | --- | --- |
| default | 每次文件操作都需要确认 | 生产环境、敏感项目 |
| acceptEdits | 自动接受文件编辑 | 个人项目、自动化脚本 |
| bypassPermissions | 跳过所有权限检查 | 完全受控环境（谨慎使用） |

**工具白名单**：

```
# 只读Agent - 只能读取和搜索
readonly_options = ClaudeAgentOptions(
    allowed_tools=[&#x27;Read&#x27;, &#x27;Glob&#x27;, &#x27;Grep&#x27;]
)

# 代码审查Agent - 只能读取和执行测试
review_options = ClaudeAgentOptions(
    allowed_tools=[&#x27;Read&#x27;, &#x27;Glob&#x27;, &#x27;Grep&#x27;, &#x27;Bash&#x27;],
    system_prompt=&quot;你是代码审查专家。只能运行测试命令，不能修改任何文件。&quot;
)

# 完全权限Agent - 可以做任何事
full_options = ClaudeAgentOptions(
    allowed_tools=[&#x27;Read&#x27;, &#x27;Write&#x27;, &#x27;Edit&#x27;, &#x27;Bash&#x27;, &#x27;Glob&#x27;, &#x27;Grep&#x27;, &#x27;Task&#x27;],
    permission_mode=&#x27;acceptEdits&#x27;
)
```

### 5.4 沙箱模式

保护你的系统免受意外损害：

```
from claude_agent_sdk import query, ClaudeAgentOptions
from claude_agent_sdk.types import ToolPermissionContext, PermissionResultAllow, PermissionResultDeny

# 自定义沙箱控制
async def can_use_tool(
    tool_name: str, tool_input: dict, context: ToolPermissionContext
) -&gt; PermissionResultAllow | PermissionResultDeny:
    &quot;&quot;&quot;自定义工具权限检查&quot;&quot;&quot;
    # 检查是否请求禁用沙箱
    if tool_name == &quot;Bash&quot; and tool_input.get(&quot;dangerouslyDisableSandbox&quot;):
        print(f&quot;警告：请求在沙箱外执行命令：{tool_input.get(&#x27;command&#x27;)}&quot;)
        # 返回Deny拒绝，Allow允许
        return PermissionResultDeny(reason=&quot;禁止在沙箱外执行命令&quot;)
    return PermissionResultAllow()


options = ClaudeAgentOptions(
    sandbox={
        &quot;enabled&quot;: True,  # 启用沙箱
        &quot;allowUnsandboxedCommands&quot;: False  # 禁止请求沙箱外执行
    },
    permission_mode=&quot;default&quot;,
    can_use_tool=can_use_tool  # 自定义权限检查
)
```

---

## 第六部分：故障排查

### 6.1 常见错误及解决方案

#### 错误1：模块导入失败

```
# 错误信息
ModuleNotFoundError: No module named &#x27;claude_agent_sdk&#x27;
```

**原因**：SDK未安装或虚拟环境未激活

**解决方案**：

```
# 确认虚拟环境已激活
source venv/bin/activate  # Linux/macOS
# 或
venv\Scripts\activate  # Windows

# 重新安装
pip install claude-agent-sdk

# 验证安装
pip show claude-agent-sdk
```

---

#### 错误2：认证失败

```
# 错误信息
AuthenticationError: Invalid API key
```

**原因**：API Key未配置或无效

**解决方案**：

```
# 检查环境变量
echo $ANTHROPIC_API_KEY  # Linux/macOS
echo %ANTHROPIC_API_KEY%  # Windows

# 重新设置
export ANTHROPIC_API_KEY=&quot;sk-ant-api03-your-key-here&quot;

# 或在代码中设置（不推荐）
import os
os.environ[&quot;ANTHROPIC_API_KEY&quot;] = &quot;your-key&quot;
```

---

#### 错误3：连接超时

```
# 错误信息
ConnectionTimeout: Request timed out
```

**原因**：网络问题或API服务繁忙

**解决方案**：

```
# 检查网络连接
ping api.anthropic.com

# 国内用户可能需要代理
export HTTP_PROXY=&quot;http://127.0.0.1:7890&quot;
export HTTPS_PROXY=&quot;http://127.0.0.1:7890&quot;
```

---

#### 错误4：工具权限被拒绝

```
# 错误信息
PermissionDenied: Tool &#x27;Write&#x27; is not in allowed_tools
```

**原因**：工具不在白名单中

**解决方案**：

```
# 添加需要的工具到白名单
options = ClaudeAgentOptions(
    allowed_tools=[&#x27;Read&#x27;, &#x27;Write&#x27;, &#x27;Edit&#x27;]  # 添加Write
)
```

---

#### 错误5：CLI未找到

```
# 错误信息
CLINotFoundError: Claude Code CLI not found
```

**原因**：捆绑的CLI未正确安装

**解决方案**：

```
# 方法1：重新安装SDK
pip uninstall claude-agent-sdk
pip install claude-agent-sdk

# 方法2：手动安装CLI
curl -fsSL https://claude.ai/install.sh | bash

# 方法3：指定CLI路径
options = ClaudeAgentOptions(
    cli_path=&quot;/path/to/claude&quot;
)
```

### 6.2 调试技巧

**启用详细日志**：

```
import logging

# 设置日志级别
logging.basicConfig(level=logging.DEBUG)

# 或者使用环境变量
# export CLAUDE_SDK_LOG_LEVEL=DEBUG
```

**打印消息详情**：

```
async for message in query(prompt=&quot;...&quot;, options=options):
    print(f&quot;消息类型: {type(message).__name__}&quot;)
    print(f&quot;消息内容: {message}&quot;)
    print(&quot;-&quot; * 40)
```

---

## 第七部分：FAQ 常见问题

### Q1: Agent SDK和直接调用Claude API有什么区别？

**答**：主要区别有三点：

1.  Agent SDK有Agent Loop：可以自动多轮对话，使用工具完成复杂任务

1.  **Agent SDK有内置工具**：文件操作、命令执行等能力开箱即用

1.  **Agent SDK支持子代理**：可以并行处理大规模任务

如果你只需要简单问答，用Claude API即可。如果需要AI"干活"（操作文件、执行命令），用Agent SDK。

---

### Q2: 哪个模型性价比最高？

**答**：推荐模型选择：

| 场景 | 推荐模型 | 理由 |
| --- | --- | --- |
| 日常开发 | claude-sonnet-4-6 | 能力强、速度快、价格适中 |
| 复杂推理 | claude-opus-4-6 | 最强能力，但价格高 |
| 简单任务 | claude-haiku-4-5-20251001 | 最便宜，适合简单查询 |

```
# 性价比之选
options = ClaudeAgentOptions(model=&#x27;claude-sonnet-4-6&#x27;)
```

---

### Q3: 如何控制Token消耗？

**答**：几个技巧：

1.  **设置max_turns**：限制对话轮数

1.  **精简system_prompt**：不要写太长的系统提示

1.  **任务明确**：给出清晰具体的任务描述

1.  **使用更小的模型**：简单任务用Haiku

```
options = ClaudeAgentOptions(
    model=&#x27;claude-haiku-4-5-20251001&#x27;,  # 便宜模型
    max_turns=5,  # 限制轮数
    system_prompt=&quot;简洁回复。&quot;  # 简短提示
)
```

---

### Q4: 子代理和主代理怎么通信？

**答**：通过Task和TaskOutput工具：

1.  主Agent用Task工具创建子代理，指定任务

1.  子代理执行任务并返回结果

1.  主Agent用TaskOutput工具获取结果

子代理完成后，结果会自动返回给主Agent。

---

### Q5: 可以同时运行多少个子代理？

**答**：最多10个并行。如果需要处理更多任务，会自动排队。

---

### Q6: 如何在生产环境使用？

**答**：几个建议：

1.  **使用权限控制**：设置allowed_tools白名单

1.  **启用沙箱**：防止危险命令

1.  **添加Hooks**：记录日志、过滤危险操作

1.  **设置超时**：防止任务卡住

1.  **监控Token消耗**：控制成本

```
# 生产环境配置示例
production_options = ClaudeAgentOptions(
    model=&#x27;claude-sonnet-4-6&#x27;,  # 推荐模型
    allowed_tools=[&#x27;Read&#x27;, &#x27;Glob&#x27;, &#x27;Grep&#x27;],  # 最小权限
    permission_mode=&#x27;default&#x27;,  # 需要确认
    max_turns=10,
    sandbox={&quot;enabled&quot;: True},
    hooks={
        &quot;PreToolUse&quot;: [safety_hooks],
        &quot;PostToolUse&quot;: [logging_hooks]
    }
)
```

---

### Q7: SDK支持流式输出吗？

**答**：是的！query函数返回的是AsyncIterator，天然支持流式：

```
async for message in query(prompt=&quot;...&quot;, options=options):
    if isinstance(message, AssistantMessage):
        for block in message.content:
            if isinstance(block, TextBlock):
                print(block.text, end=&#x27;&#x27;, flush=True)  # 流式打印
```

---

### Q8: 如何处理长时间运行的任务？

**答**：

1.  **使用后台运行的子代理**：Task工具支持run_in_background

1.  **设置合理的超时**

1.  **使用异步并行**

```
prompt = &quot;&quot;&quot;请使用子代理并行处理：
- 子代理1：分析src目录（run_in_background: true）
- 子代理2：分析tests目录（run_in_background: true）

启动后等待所有子代理完成再汇总。&quot;&quot;&quot;
```

---

### Q9: 自定义工具可以访问外部API吗？

**答**：可以！自定义工具就是普通的Python/TypeScript函数：

```
import httpx

@tool(&quot;weather&quot;, &quot;获取天气信息&quot;, {&quot;city&quot;: str})
async def get_weather(args):
    city = args.get(&#x27;city&#x27;)
    async with httpx.AsyncClient() as client:
        response = await client.get(f&quot;https://api.weather.com/{city}&quot;)
        data = response.json()

    return {&quot;content&quot;: [{&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: f&quot;{city}天气：{data}&quot;}]}
```

---

### Q10: 如何调试Agent的决策过程？

**答**：打印所有消息类型：

```
async for message in query(prompt=&quot;...&quot;, options=options):
    print(f&quot;[{type(message).__name__}] &quot;, end=&#x27;&#x27;)

    if isinstance(message, AssistantMessage):
        print(&quot;AI思考中...&quot;)
        for block in message.content:
            if isinstance(block, TextBlock):
                print(f&quot;  文本: {block.text[:100]}...&quot;)
            elif isinstance(block, ToolUseBlock):
                print(f&quot;  调用工具: {block.name}({block.input})&quot;)

    elif isinstance(message, ToolResultBlock):
        print(f&quot;工具结果: {str(message.content)[:100]}...&quot;)
```

---

### Q11: SDK会自动重试失败的请求吗？

**答**：SDK有基本的重试机制，但你可以自己加强：

```
import asyncio
from tenacity import retry, stop_after_attempt, wait_exponential

@retry(stop=stop_after_attempt(3), wait=wait_exponential(min=1, max=10))
async def query_with_retry(prompt, options):
    results = []
    async for message in query(prompt=prompt, options=options):
        results.append(message)
    return results
```

---

### Q12: 如何在Jupyter Notebook中使用？

**答**：需要处理事件循环：

```
import nest_asyncio
nest_asyncio.apply()

# 然后正常使用
import asyncio
from claude_agent_sdk import query, ClaudeAgentOptions

async def run():
    async for message in query(prompt=&quot;Hello&quot;, options=ClaudeAgentOptions()):
        print(message)

asyncio.run(run())
```

---

### Q13: 可以保存和恢复会话吗？

**答**：SDK本身不提供会话持久化，但你可以自己实现：

```
import json

# 保存对话历史
conversation_history = []

async for message in query(prompt=&quot;...&quot;, options=options):
    conversation_history.append({
        &quot;type&quot;: type(message).__name__,
        &quot;content&quot;: str(message)
    })

# 保存到文件
with open(&quot;session.json&quot;, &quot;w&quot;) as f:
    json.dump(conversation_history, f)
```

---

### Q14: TypeScript和Python SDK功能一样吗？

**答**：是的，功能完全相同。选择你熟悉的语言即可。

---

### Q15: 如何处理大文件？

**答**：Agent会自动分块处理。如果文件太大，考虑：

1.  只读取需要的部分

1.  使用Grep搜索而不是Read全文

1.  拆分任务给多个子代理

---

### Q16: SDK支持哪些AI模型？

**答**：支持Claude系列模型：

+   claude-opus-4-6（最强）

+   claude-sonnet-4-6（推荐）

+   claude-haiku-4-5-20251001（最快最便宜）

通过model参数指定：

```
options = ClaudeAgentOptions(model=&#x27;claude-sonnet-4-6&#x27;)
```

---

### Q17: 如何测试自定义工具？

**答**：单独测试工具函数：

```
# 直接调用测试
result = await calculate({&quot;expression&quot;: &quot;2+2&quot;})
print(result)  # 应该输出 {&quot;content&quot;: [{&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: &quot;2+2 = 4&quot;}]}

# 然后在Agent中测试
async for message in query(
    prompt=&quot;请计算 2+2&quot;,
    options=ClaudeAgentOptions(
        mcp_servers={&quot;tools&quot;: my_tools},
        allowed_tools=[&quot;mcp__tools__calculate&quot;]
    )
):
    print(message)
```

---

### Q18: 如何处理API速率限制？

**答**：

1.  添加请求间隔

1.  使用指数退避重试

1.  监控速率限制错误

```
import asyncio

async def rate_limited_query(prompts, delay=1.0):
    for prompt in prompts:
        async for message in query(prompt=prompt, options=options):
            yield message
        await asyncio.sleep(delay)  # 请求间隔
```

---

### Q19: 安全最佳实践有哪些？

**答**：

1.  **最小权限原则**：只给需要的工具权限

1.  **输入验证**：自定义工具要验证输入

1.  **不信任用户输入**：Agent的prompt可能被注入

1.  **启用沙箱**：Bash命令在沙箱中执行

1.  **使用Hooks过滤**：阻止危险操作

1.  **监控日志**：记录所有工具调用

---

### Q20: 有官方示例代码吗？

**答**：有！

+   **Python示例**：https://github.com/anthropics/claude-agent-sdk-python/tree/main/examples

+   **TypeScript示例**：https://github.com/anthropics/claude-agent-sdk-typescript/tree/main/examples

推荐从quick_start.py或quick_start.ts开始。

---

## 本课总结

### 核心知识点回顾

1.  **Agent SDK定位**：把Claude Code的能力封装为可编程接口

1.  **query函数**：核心API，返回流式消息

1.  **Agent Loop**：接收→思考→使用工具→检查→继续/结束

1.  **Tools**：Agent的能力来源，可以自定义扩展

1.  **Subagents**：并行处理复杂任务

1.  **Hooks**：在关键时刻自动执行自定义逻辑

### 最佳实践总结

| 场景 | 建议 |
| --- | --- |
| 模型选择 | 日常用Sonnet，复杂推理用Opus，简单任务用Haiku |
| 权限控制 | 生产环境用最小权限，开发可以放宽 |
| 子代理 | 大任务拆分并行，注意最多10个同时运行 |
| 自定义工具 | 优先用SDK内置MCP服务器，简单高效 |
| 错误处理 | 使用try/catch，添加重试机制 |

### 下一步学习

完成本课后，建议继续学习：

1.  **MCP开发**：深入学习MCP协议，创建更复杂的工具

1.  **Hooks高级应用**：实现自动测试、自动格式化等工作流

1.  **企业集成**：将Agent SDK集成到CI/CD流程

---

**课程版本****：V1.****1

**最后更新：2026年2月25日

作者：Claude Code教程团队

---

## 版本历史

| 版本 | 日期 | 修改内容 |
| --- | --- | --- |
| V1.2 | 2026-02-26 | 更新所有模型引用至最新4.6版本：Sonnet 4.6（claude-sonnet-4-6）、Opus 4.6（claude-opus-4-6）、Haiku 4.5（claude-haiku-4-5-20251001） |
| V1.1 | 2025-12-24 | 修正所有模型名称为最新4.5版本（claude-sonnet-4-5-20250929、claude-opus-4-5-20251101、claude-3-5-haiku-20241022） |
| V1.0 | 2025-12-19 | 初版发布 |

**参考资料**：

+   [官方Python SDK文档](https://docs.anthropic.com/en/docs/claude-code/sdk/sdk-python)

+   官方TypeScript SDK文档

+   GitHub Python SDK

+   GitHub TypeScript SDK

+   [DataCamp教程](https://www.datacamp.com/tutorial/how-to-use-claude-agent-sdk)

+   [KDnuggets入门指南](https://www.kdnuggets.com/getting-started-with-the-claude-agent-sdk)