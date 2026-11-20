
# 第十五章 构建赛博小镇

> 来源：[https://gotoaiworld.feishu.cn/docx/EGbRd346ioiPyyx8mpwcTmChnsc](https://gotoaiworld.feishu.cn/docx/EGbRd346ioiPyyx8mpwcTmChnsc)

这一章，我们将探索一个全新的方向：将智能体技术与游戏引擎结合，构建一个充满生命力的 AI 小镇。

还记得《模拟人生》或《动物森友会》中那些栩栩如生的 NPC 吗？他们有自己的性格、记忆和社交关系。本章的赛博小镇将是一个类似的项目，但与传统游戏不同的是，我们的 NPC 拥有真正的"智能"——他们能够理解玩家的对话，记住过去的互动，并根据好感度做出不同的反应。本章的赛博小镇包含以下核心功能：

（1）智能 NPC 对话系统：玩家可以与 NPC 进行自然语言对话，NPC 会根据自己的角色设定和记忆做出回应。

（2）记忆系统：NPC 拥有短期记忆和长期记忆，能够记住与玩家的互动历史。

（3）好感度系统：NPC 对玩家的态度会随着互动而变化，从陌生到熟悉，从友好到亲密。

（4）游戏化交互：玩家可以在 2D 像素风格的办公室场景中自由移动，与不同的 NPC 互动。

（5）实时日志系统：所有对话和互动都会被记录，方便调试和分析。

## [15.1 项目概述与架构设计](https://datawhalechina.github.io/hello-agents/#/./chapter15/%E7%AC%AC%E5%8D%81%E4%BA%94%E7%AB%A0%20%E6%9E%84%E5%BB%BA%E8%B5%9B%E5%8D%9A%E5%B0%8F%E9%95%87?id=_151-%e9%a1%b9%e7%9b%ae%e6%a6%82%e8%bf%b0%e4%b8%8e%e6%9e%b6%e6%9e%84%e8%ae%be%e8%ae%a1)

### 15.1.1 为什么要构建 AI 小镇

传统游戏中的 NPC 通常只能说固定的台词，或者通过预设的对话树进行有限的互动。即使是最复杂的 RPG 游戏，NPC 的对话也是由编剧事先写好的。这种方式虽然可控，但缺乏真正的"智能"和"生命力"。

想象一下，如果游戏中的 NPC 能够理解你说的任何话，不再局限于预设的选项，你可以用自然语言与 NPC 交流。NPC 会记得你上次说了什么，你们的关系如何，甚至你的喜好。每个 NPC 都有自己的职业、性格和说话风格。NPC 对你的态度会随着互动而变化，从陌生人到朋友，甚至挚友。

这就是 AI 技术为游戏带来的新可能。通过将大语言模型与游戏引擎结合，我们可以创造出真正"活着"的 NPC。这不仅仅是一个技术演示，更是对未来游戏形态的探索。在教育游戏中，NPC 可以扮演历史人物、科学家，与学生进行互动式教学。在虚拟办公室中，NPC 可以扮演同事、导师，提供帮助和建议。NPC 还可以作为陪伴者，与用户进行情感交流，应用于心理健康领域。当然，最直接的应用就是为传统游戏增加 AI NPC，提升玩家体验。

### [15.1.2 技术架构概览](https://datawhalechina.github.io/hello-agents/#/./chapter15/%E7%AC%AC%E5%8D%81%E4%BA%94%E7%AB%A0%20%E6%9E%84%E5%BB%BA%E8%B5%9B%E5%8D%9A%E5%B0%8F%E9%95%87?id=_1512-%e6%8a%80%e6%9c%af%e6%9e%b6%e6%9e%84%e6%a6%82%e8%a7%88)

赛博小镇采用游戏引擎+后端服务的分离架构，分为四个层次，如图 15.1 所示。

![](img/1e2389dee6a17a3b74770c2af9ee52c3.png)

图 15.1 赛博小镇技术架构

前端层使用 Godot 4.5 游戏引擎，负责游戏渲染、玩家控制、NPC 显示和对话 UI。Godot 是一个开源的 2D/3D 游戏引擎，非常适合快速开发像素风格的游戏。后端层使用 `FastAPI` 框架，负责 API 路由、NPC 状态管理、对话处理和日志记录。`FastAPI` 是一个现代化的 Python Web 框架，性能优秀且易于开发。智能体层使用我们自己构建的 `HelloAgents` 框架，负责 NPC 智能、记忆管理和好感度计算。每个 NPC 都是一个 `SimpleAgent` 实例，拥有独立的记忆和状态。外部服务层提供 LLM 能力、向量存储和数据持久化，包括 LLM API、`Qdrant` 向量数据库和 `SQLite` 关系数据库。

数据流转过程如图 15.2 所示：

![](img/8e793972eae814f48dd1659f6c5b63a2.png)

图 15.2 数据流转过程

玩家在 Godot 中按 E 键与 NPC 互动，Godot 通过 HTTP API 发送对话请求到 `FastAPI` 后端。后端调用 `HelloAgents` 的 `SimpleAgent` 处理对话，Agent 从记忆系统中检索相关历史，然后调用 LLM 生成回复。后端更新 NPC 状态和好感度，记录日志到控制台和文件，最后返回回复给 Godot 前端。Godot 显示 NPC 回复并更新 UI，完成一次完整的交互循环。

项目的结构如下，方便你定位源码：

```
Helloagents-AI-Town/
├── helloagents-ai-town/           # Godot游戏项目
│   ├── project.godot              # Godot项目配置
│   ├── scenes/                    # 游戏场景
│   │   ├── main.tscn              # 主场景(办公室)
│   │   ├── player.tscn            # 玩家角色
│   │   ├── npc.tscn               # NPC角色
│   │   └── dialogue_ui.tscn       # 对话UI
│   ├── scripts/                   # GDScript脚本
│   │   ├── main.gd                # 主场景逻辑
│   │   ├── player.gd              # 玩家控制
│   │   ├── npc.gd                 # NPC行为
│   │   ├── dialogue_ui.gd         # 对话UI逻辑
│   │   ├── api_client.gd          # API客户端
│   │   └── config.gd              # 配置管理
│   └── assets/                    # 游戏资源
│       ├── characters/            # 角色精灵图
│       ├── interiors/             # 室内场景
│       ├── ui/                    # UI素材
│       └── audio/                 # 音效音乐
│
└── backend/                       # Python后端
    ├── main.py                    # FastAPI主程序
    ├── agents.py                  # NPC Agent系统
    ├── relationship_manager.py    # 好感度管理
    ├── state_manager.py           # 状态管理
    ├── logger.py                  # 日志系统
    ├── config.py                  # 配置管理
    ├── models.py                  # 数据模型
    ├── requirements.txt           # Python依赖
    └── .env.example               # 环境变量示例
```

详细的架构设计和数据流转将在后续章节中介绍。

### 15.1.3 快速体验：5 分钟运行项目

在深入学习实现细节之前，让我们先把项目跑起来，看看最终的效果。这样你会对整个系统有一个直观的认识。

环境要求：

+ Godot 4.2 或更高版本

+ Python 3.10 或更高版本

+ LLM API 密钥(OpenAI、DeepSeek、智谱等)

获取项目：

你可以到 `code/chapter15/Helloagents-AI-Town` 中查看，或者从 GitHub 克隆完整的 `hello-agents` 仓库。

启动后端：

```
# 1. 进入backend目录
cd Helloagents-AI-Town/backend

# 2. 安装依赖
pip install -r requirements.txt

# 3. 配置环境变量
cp .env.example .env
# 编辑.env文件，填写你的API密钥
# 4. 启动后端服务
python main.py
```

成功启动后，你会看到如下输出：

```
============================================================
🎮 赛博小镇后端服务启动中...
============================================================
✅ 所有服务已启动!
📡 API地址: http://0.0.0.0:8000
📚 API文档: http://0.0.0.0:8000/docs
============================================================
```

启动 Godot：

Godot 的安装非常简单，Windows 提供了直接打开的 `.exe` 文件，Mac 也提供了 `.dmg` 文件。可直接在官网下载(Windows / Mac)

打开 Godot 引擎，点击"导入"按钮，浏览到 `Helloagents-AI-Town/helloagents-ai-town/project.godot`，点击"导入并编辑"。等待 Godot 导入资源后，按 F5 或点击"运行"按钮启动游戏。

体验核心功能：

游戏启动后，你会看到一个像素风格的 Datawhale 办公室场景，如图 15.3 所示。

![](img/960c12d2c3464abae3f77948eda09c29.png)

图 15.3 赛博小镇游戏场景

使用 WASD 键移动玩家角色，走到 NPC 附近时，屏幕上会显示"按 E 键交互"的提示。按下 E 键后，会弹出对话框，你可以输入任何想说的话，如图 15.4 所示。

![](img/df164c2a96a4085bef14dea40d2829cd.png)

图 15.4 与 NPC 对话界面

NPC 会根据自己的角色设定(`Python 工程师`、`产品经理`、`UI 设计师`)和你们的互动历史做出回应。随着对话的进行，NPC 对你的好感度会逐渐提升，从"陌生"到"熟悉"，再到"友好"、"亲密"甚至"挚友"。

好感度系统在后端实现，每次对话都会根据玩家的消息内容和情感分析来调整好感度值。虽然前端游戏界面中没有直接显示好感度数值，但所有的好感度变化都会被详细记录在后端日志中。你可以在 `backend/logs/dialogue_YYYY-MM-DD.log` 文件中查看每次对话的好感度变化。日志文件会记录每次对话的详细信息，包括：当前好感度值、检索到的相关记忆、NPC 的回复、好感度变化量(`+2.0`、`+3.0` 等)、变化原因(友好问候、正常交流等)以及情感分析结果(`positive`、`neutral` 等)。这种设计让开发者可以清晰地追踪 NPC 与玩家的关系发展，也为后续在前端添加好感度 UI 提供了数据基础。

所有的对话都会被记录在后端的日志文件中，你可以通过以下命令实时查看：

```
# 在backend目录下
python view_logs.py
```

这个简单的体验展示了 AI 小镇的核心功能。接下来，我们将深入学习如何实现这些功能。

## [15.2 NPC 智能体系统](https://datawhalechina.github.io/hello-agents/#/./chapter15/%E7%AC%AC%E5%8D%81%E4%BA%94%E7%AB%A0%20%E6%9E%84%E5%BB%BA%E8%B5%9B%E5%8D%9A%E5%B0%8F%E9%95%87?id=_152-npc-%e6%99%ba%e8%83%bd%e4%bd%93%e7%b3%bb%e7%bb%9f)

### 15.2.1 基于 HelloAgents 的 SimpleAgent

在赛博小镇中，每个 NPC 都是一个独立的智能体。我们使用 `HelloAgents` 框架中的 `SimpleAgent` 来实现 NPC 的智能。`SimpleAgent` 是一个轻量级的智能体实现，它封装了 LLM 调用、消息管理和工具调用等核心功能。

回顾一下第七章中我们学习的 `SimpleAgent`，它的核心是一个简单的对话循环：接收用户消息，调用 LLM 生成回复，返回结果。在赛博小镇中，我们需要为每个 NPC 创建一个 `SimpleAgent` 实例，并为其配置独特的系统提示词，让每个 NPC 拥有不同的性格和角色设定。

让我们看看如何创建一个 NPC Agent。首先，我们需要定义 NPC 的基本信息，包括 ID、名称、职业和性格。然后，我们根据这些信息构建系统提示词，让 LLM 扮演这个 NPC 的角色。最后，我们创建 `SimpleAgent` 实例，并配置记忆系统。

```
from hello_agents import SimpleAgent, HelloAgentsLLM
from hello_agents.memory import MemoryManager, WorkingMemory, EpisodicMemory

def create_npc_agent(npc_id: str, name: str, role: str, personality: str):
    """创建NPC Agent"""
    # 构建系统提示词
    system_prompt = f"""你是{name},一位{role}。
你的性格特点:{personality}

你在Datawhale办公室工作,与同事们一起推动开源社区的发展。
请根据你的角色和性格,自然地与玩家对话。
记住你们之前的对话内容,保持对话的连贯性。
"""
    # 创建LLM实例
    llm = HelloAgentsLLM()
    # 创建记忆管理器
    memory_manager = MemoryManager(
        working_memory=WorkingMemory(capacity=10, ttl_minutes=120),
        episodic_memory=EpisodicMemory(
            db_path=f"memory_data/{npc_id}_episodic.db",
            collection_name=f"{npc_id}_memories"))
    # 创建Agent
    agent = SimpleAgent(
        name=name,
        llm=llm,
        system_prompt=system_prompt,
        memory_manager=memory_manager
    )
    return agent
```

这段代码展示了如何创建一个 NPC Agent。系统提示词定义了 NPC 的身份和性格，记忆管理器让 NPC 能够记住与玩家的对话历史。`WorkingMemory` 是短期记忆，容量为 10 条消息，保留时间为 120 分钟。`EpisodicMemory` 是长期记忆，使用 `SQLite` 数据库和 `Qdrant` 向量数据库存储，可以检索相关的历史对话。

NPC Agent 的工作流程如图 15.5 所示：

![](img/ce7eec67636a99c27fd6cd0f344d971d.png)

图 15.5 NPC Agent 工作流程

### 15.2.2 NPC 角色设定与 Prompt 设计

一个好的 NPC 需要有鲜明的性格和角色设定。在赛博小镇中，我们设计了三个 NPC，分别代表不同的职业和性格。

张三 - Python 工程师

张三是一位资深的 Python 工程师，负责 `HelloAgents` 框架的核心开发。他性格严谨，说话直接，喜欢用技术术语。他对代码质量有很高的要求，经常会分享一些编程技巧和最佳实践。

```
npc_zhang = {"npc_id": "zhang_san", "name": "张三", "role": "Python工程师", "personality": "严谨、专业、喜欢分享技术知识。说话直接,注重代码质量。"}
```

李四 - 产品经理
