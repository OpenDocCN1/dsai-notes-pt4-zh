
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
