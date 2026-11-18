
# AgentScope 初探

> 来源：[https://datawhaler.feishu.cn/docx/ONABdU7z9oORIixNm6ScFEifntg](https://datawhaler.feishu.cn/docx/ONABdU7z9oORIixNm6ScFEifntg)

<https://datawhaler.feishu.cn/sync/FsXmd9Oifs2Jx4bq3CCcPXE8npg>

<https://datawhaler.feishu.cn/sync/LWcAdh71VsldyGbpJRecSVeNnk8>

> 本文档由  、 写作，感谢优秀的学习者为我们分享经验~

## 环境配置

### 安装 AgentScope

详见 [官方文档](https://modelscope.github.io/agentscope/zh_CN/tutorial/102-installation.html#installation-zh)

#### 源码安装

`源码安装` 可以体验较新的功能（比如：适配通义 api）

```bash
# 从 GitHub 上拉取 AgentScope 的源代码
git clone https://github.com/modelscope/agentscope.git
cd AgentScope

# 针对本地化的 multi-agent 应用
pip install -e .
```

#### pip 安装

注意：这个方法确实会很方便，但建议用 `git` 下载源码，通过源码安装最新版 `agentscope`

```bash
# 针对本地化的 multi-agent 应用
pip install agentscope
```

### 配置 model_config

#### model_config 中的参数

> 使用多智能体框架 `AgentScope` 制作应用，需要配置 `model_config`，即模型的配置，以方便后续设计 `Agent` 时使用对应模型，`model_config` 中的参数如下：

`model_type`：对应 `ModelWrapper` 的类型，不同 `model_type` 需要填的 `model_config` 内容不同

+   有固定的几个 `type`，支持的 API 列表可见 [官方文档](https://modelscope.github.io/agentscope/zh_CN/tutorial/203-model.html#model-zh)。

![](img/3201f6d1c17cc5f821508a61904b038a.png)

+   举例：如果 `model_type` 为 `openai`，那么它内部会找到 `agentscope\src\agentscope\models\openai_model.py` 文件里面的 `class`。具体实现可以看源码。

`config_name`：相当于自己定义的一个配置的名字

+   后面设置 `Agent` 的 `model_config_name` 会与 `model` 进行绑定。

`model_name`：使用模型的名称

`api_key`：调用模型 API 服务的密钥，需要去对应的平台申请。

#### 大模型的配置

+   [通义千问](https://dashscope.console.aliyun.com/)

```python
import agentscope

agentscope.init(
    model_configs={
        "model_type": "dashscope_chat",
        "config_name": "qwen",
        "model_name": "qwen-max",
        "api_key": "sk-",
        "generate_args": {
            "temperature": 0.5
        }
    }
)
```

> 03-14 更新的最新版本代码中运行 `dashscope` 中的模型可能会出现如下问题，官方给出的回复是：这个问题是因为 `qwen` 对 `role` 的顺序要求导致的，目前我们正在尝试从 `prompt` 组织层面解决，估计近期能出一个初步的解决方案。

![](img/b5f2b06bc552085a9612fd2ab345d95e.png)

+   [OpenAI](https://platform.openai.com/)

```python
import agentscope

agentscope.init(
    model_configs={
        "model_type": "openai",
        "config_name": "gpt4",
        "model_name": "gpt-4",
        "api_key": "sk-",
        "generate_args": {
            "temperature": 0.5
        }
    }
)
```

+   [月之暗面](https://platform.moonshot.cn/)

![](img/d4bceeba2c2ddd42bc936f1ddfc1f5d1.png)

我们可以看到源码里面含有这个参数 `client_args`，即 `OpenAI` 函数的额外参数（如：`base_url` 等）。

因为 `月之暗面` 和 `deepseek` 是使用 `OpenAI` 类来调用的，所以我们可以通过 `OpenAI` 类调用他们的模型。

![](img/3a80f05abe5e599c15a1d79d27b461fa.png)

![](img/54c88d054fb7ff74fabca3892b22dda8.png)

```python
import agentscope

agentscope.init(
    model_configs={
        "model_type": "openai",
        "config_name": "moonshot",
        "model_name": "moonshot-v1-8k",
        "api_key": "sk-",
        "generate_args": {
            "temperature": 0.5
        },
        "client_args": {
            "base_url": "https://api.moonshot.cn/v1"
        }
    }
)
```

+   [deepseek](https://platform.deepseek.com/)

```python
import agentscope

agentscope.init(
    model_configs={
        "model_type": "openai",
        "config_name": "deepseek",
        "model_name": "deepseek-chat",
        "api_key": "sk-",
        "generate_args": {
            "temperature": 0.5
        },
        "client_args": {
            "base_url": "https://api.deepseek.com/v1"
        }
    }
)
```

+   [官方文档](https://modelscope.github.io/agentscope/zh_CN/tutorial/203-model.html#id4) 中给出了：`OpenAI API`、`DashScope API`、`Gemini API`、`Ollama API`、`Post Request API`、`Post Request Chat API` 六种配置示例

#### 通过 model_configs.json 初始化 agentscope

`model_configs.json` 文件

```json
[
    {
        "model_type": "dashscope_chat",
        "config_name": "qwen",
        "model_name": "qwen-72b-chat",
        "api_key": "sk-",
        "generate_args": {
            "temperature": 0.5
        }
    },
    {
        "model_type": "openai",
        "config_name": "deepseek",
        "model_name": "deepseek-chat",
        "api_key": "sk-",
        "generate_args": {
            "temperature": 0.5
        },
        "client_args": {
            "base_url": "https://api.deepseek.com/v1"
        }
    },
    {
        "model_type": "openai",
        "config_name": "gpt-3.5",
        "model_name": "gpt-3.5-turbo-0125",
        "api_key": "sk-",
        "generate_args": {
            "temperature": 0.5
        }
    }
]
```

```python
import agentscope

# 初始化了多个大模型
agentscope.init(
    model_configs="./model_configs.json"
)
```

+   零一万物

+   `Yi Model API` 与 `OpenAI API` 完全兼容，你只需修改少量代码，可以平滑迁移。

+   注册和 `api-token` 使用地址： https://platform.lingyiwanwu.com/

+   参见 `openai` 的调用模式--待联调

### 配置 agent_config

#### 直接定义一个对话 agent（`DialogAgent`）

```python
from agentscope.agents import DialogAgent
import agentscope

# 初始化了多个大模型
agentscope.init(
    model_configs="./model_configs.json"
)

# 使用 qwen 大模型初始化一个对话 agent
dialog_agent_qwen = DialogAgent(
    name="Assistant_qwen",
    sys_prompt="You're a helpful assistant.",  # sys_prompt 可以自行定义，不能为空
    model_config_name="qwen",  # 这里的 qwen 和前面的 model_config.json 文件中的 config_name 要一一对应，否则 Agent 找不到大模型会报错
)

# 使用 deepseek 大模型初始化一个对话 agent
dialog_agent_deepseek = DialogAgent(
    name="Assistant_deepseek",
    sys_prompt="You're a helpful assistant.",
    model_config_name="deepseek",
)

# 使用 gpt-3.5 大模型初始化一个对话 agent
dialog_agent_gpt = DialogAgent(
    name="Assistant_deepseek",
    sys_prompt="You're a helpful assistant.",
    model_config_name="gpt-3.5",
)

# 简单实用 可以当作一个 api
from agentscope.message import Msg

msg = Msg(name="贾维斯", content="你好")

dialog_agent_deepseek(msg)
dialog_agent_gpt(msg)
```

#### 通过配置文件创建多个 Agent

`agent_configs.json`

参数

+   `class`：用来设置你要创建一个什么类型的 `Agent`

+   `args`：`class` 的参数

```json
[
    {
        "class": "DialogAgent",
        "args": {
            "name": "Assistant_qwen",
            "sys_prompt": "You're a helpful assistant.",
            "model_config_name": "qwen",
            "use_memory": true
        }
    },
    {
        "class": "DialogAgent",
        "args": {
            "name": "Assistant_deepseek",
            "sys_prompt": "You're a helpful assistant.",
            "model_config_name": "deepseek",
            "use_memory": true
        }
    },
    {
        "class": "DialogAgent",
        "args": {
            "name": "Assistant_gpt",
            "sys_prompt": "You're a helpful assistant.",
            "model_config_name": "gpt-3.5",
            "use_memory": true
        }
    }
]
```

```python
import agentscope

# 初始化了多个大模型和多个 Agent
agents = agentscope.init(
    model_configs="./model_configs.json",  # 前面创建的 model_configs.json 文件
    agent_configs="./agent_configs.json",
)


# 对应配置文件 agent_configs.json 中的顺序
dialog_agent_qwen = agents[0]
dialog_agent_deepseek = agents[1]
dialog_agent_gpt = agents[2]
```

## Agent 之间相互对话

### Pipeline 方式（按顺序一个接着一个讨论）

下面我们来举一个成语接龙的简单例子。



```json
[
    {
        "class": "DialogAgent",
        "args": {
            "name": "player_qwen",
            "sys_prompt": "作为成语接龙的参与者，您的任务是根据前一个玩家提供的成语的最后一个字来提出一个新的成语。限制：你仅需说出一个四字中文成语，其他的话不需要说；请确保您的成语是四字成语。",
            "model_config_name": "qwen",
            "use_memory": true
        }
    },
    {
        "class": "DialogAgent",
        "args": {
            "name": "player_deepseek",
            "sys_prompt": "作为成语接龙的参与者，您的任务是根据前一个玩家提供的成语的最后一个字来提出一个新的成语。限制：你仅需说出一个四字中文成语，其他的话不需要说；请确保您的成语是四字成语。",
            "model_config_name": "deepseek",
            "use_memory": true
        }
    },
    {
        "class": "DialogAgent",
        "args": {
            "name": "player_gpt",
            "sys_prompt": "作为成语接龙的参与者，您的任务是根据前一个玩家提供的成语的最后一个字来提出一个新的成语。限制：你仅需说出一个四字中文成语，其他的话不需要说；请确保您的成语是四字成语。",
            "model_config_name": "gpt-3.5",
            "use_memory": true
        }
    }
]
```

```
import agentscope
from agentscope.pipelines import SequentialPipeline
from agentscope.message import Msg

# 初始化了多个大模型和多个Agent
agents = agentscope.init(
    model_configs="./model_configs.json",  # 前面创建的model_configs.json文件
    agent_configs="./agent_configs.json",
)

# 对应配置文件agent_configs.json中的顺序
player_agent_qwen = agents[0]
player_agent_deepseek = agents[1]
player_agent_gpt = agents[2]

# 创建一个SequentialPipeline
pipe = SequentialPipeline([player_agent_qwen, 
                           player_agent_deepseek, 
                           player_agent_gpt])

msg = Msg("Moderator", "游戏规则：请回答一个四字中文成语，不要添加其他文字。请按规则接龙。本轮成语接龙起始词为「水落石出」")

# 启动Pipeline
pipe(msg)
```

![](img/c7d35faefe0e3fb00e8e616ef8e01186.png)

### `MsgHub` 方式（群聊讨论）

初始化四个agent

```
import agentscope
from agentscope.pipelines import SequentialPipeline
from agentscope.message import Msg

agent_configs = [
    {
        "class": "DialogAgent",
        "args": {
            "name": "agent1",
            "sys_prompt": "你是一个得力帮手，你是agent1。",
            "model_config_name": "qwen",
            "use_memory": True
        }
    },
    {
        "class": "DialogAgent",
        "args": {
            "name": "agent2",
            "sys_prompt": "你是一个得力帮手,你是agent2。",
            "model_config_name": "qwen",
            "use_memory": True
        }
    },
    {
        "class": "DialogAgent",
        "args": {
            "name": "agent3",
            "sys_prompt": "你是一个得力帮手,你是agent3。",
            "model_config_name": "qwen",
            "use_memory": True
        }
    },
    {
        "class": "DialogAgent",
        "args": {
            "name": "agent4",
            "sys_prompt": "你是一个得力帮手，你是agent4。",
            "model_config_name": "qwen",
            "use_memory": True
        }
    },
    {
        "class": "DialogAgent",
        "args": {
            "name": "HostAgent",
            "sys_prompt": "你是一个主持agent，需要做一个决策。除了决策外不要输出任何东西。",
            "model_config_name": "qwen",
            "use_memory": True
        }
    }
]
# 初始化了多个大模型和多个Agent
agents = agentscope.init(
    model_configs="./model_configs.json",  # 前面创建的model_configs.json文件
    agent_configs=agent_configs,
)

agent1 = agents[0]
agent2 = agents[1]
agent3 = agents[2]
agent4 = agents[3]
```

### 四类通讯方式

```
from agentscope.message import Msg

msg = Msg(name="贾维斯", content="请带上自己的名字进行传话，传话内容“你好”")
# 顺序执行
msg = agent1(msg)
print(msg)
msg = agent2(msg)
print(msg)
msg = agent3(msg)
print(msg)
msg = agent4(msg)
print(msg)

# {'name': 'agent1', 'content': '我是来自阿里云的超大规模语言模型，我的名字是通义千问。现在帮您传话：“你好”'}
# {'name': 'agent2', 'content': '您好！我是通义千问，来自阿里云的超大规模语言模型，很高兴为您服务，并向您问候：“你好”。'}
# {'name': 'agent3', 'content': '您好！作为阿里云的超大规模语言模型通义千问，我非常荣幸能与您交流并提供服务。同样向您致以亲切的问候：“你好”！'}
# {'name': 'agent4', 'content': '您好！非常高兴能与您交流互动。作为阿里云的超大规模语言模型通义千问，我确实具备强大的自然语言处理能力，可以为您提供广泛的知识问答、技术咨询、场景应用等各类服务。请随时告诉我您需要了解的内容或遇到的问题，我会全力以赴为您提供帮助！'}
```

### loop情况

```
msg = Msg("system", "选择是否退出")
msg1 = Msg("system", "none")
def ifBreak(msg):
    return "退出" not in msg.content
while ifBreak(msg1):
    msg1 = hostAgent(msg)
    print(msg1.content)
    msg = agent1(msg)
    print(msg.content)
    msg = agent2(msg)
    print(msg.content)
```

### 选择情况

```
msg = hostAgent(Msg("system", "agent1 和 agent2 应该选择谁退出？我建议 agent1 退出。"))
print(msg)
if "agent1" in msg.content:
    msg = agent1(msg)
else:
    msg = agent2(msg)
print(msg)

msg_send = Msg("system", "重新选择退出或继续运行，你只需要回答肯定的指令")

def ifBreak(msg):
    return ("退出" or "结束") not in msg.content
# 复杂形势
msg = hostAgent(msg_send)
print(msg)
while ifBreak(msg):
    msg1 = hostAgent(Msg("system", "agent1 和 agent2 应该选择谁发言？，你只需要回答是谁的名字"))
    msg2 = Msg("system", "请发言，发言后请告诉主持人退出")

    if "agent1" in msg1.content:
        msg2 = agent1(msg2)
    else:
        msg2 = agent2(msg2)
    print(msg2)
    msg = hostAgent(msg_send)
    print(msg)
```

### tool配置

第二课中`web_search`模拟配置

```
def web_search(query: str, engine: str, api_key: str, num_results: int = 10):
    """
    The web search function
    Args:
        query('str'):
            The search query string.
        engine('str'):
            The search engine to use("bing" or "google").
        api key('str):
            API key for web searching.
        num results(int'):
            The number of searching results.
    """
    print(f"Calling web_sarch function with query  \"{query}\"")
```



```python
from agentscope.service.service_factory import ServiceFactory
# ServiceFactory 写法有改变
!pip install docstring_parser
# 如果报错可能没有这个库 请执行上面这句

# 初始化ServiceFactory
func, func_json = ServiceFactory.get(web_search, engine = "bing", api_key = "123123", num_results = 3)

import json
print(json.dumps(func_json, indent = 4))

# 打印一下func_json 看看效果吧
{
    "type": "function",
    "function": {
        "name": "web_search",
        "description": "The web search function",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {
                    "type": "string",
                    "description": "The search query string."
                }
            },
            "required": [
                "query"
            ]
        }
    }
}
```

采用上述配置方法可以灵活创建属于你的`tool`，注意注释内容必须按照规范格式书写。

## 分布式通信

## 项目初探（狼人杀）

> werewolf中文版

![](img/e4e84278e72bf1a940bcf2cc9df43dad.png)

[`werewolf_zh.zip`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SFsebzc1Vo99cRxWvmMcvrFonkf)
