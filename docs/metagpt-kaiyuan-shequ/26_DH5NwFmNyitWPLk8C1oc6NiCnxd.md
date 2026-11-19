
# 智能体配置专属LLM

> 来源：[https://deepwisdom.feishu.cn/docx/U4qNdhEY6otEDsxjhSoc0LNsnec](https://deepwisdom.feishu.cn/docx/U4qNdhEY6otEDsxjhSoc0LNsnec)

MetaGPT允许你为团队中的不同`Role`和`Action`使用不同的`LLM`，这极大地增强了团队互动的灵活性和现实性，使得每个`Role`可以根据其特定的需求和背景，以及每个`Action`的特点，选择最合适的`LLM`。通过这种方式，你可以更精细地控制对话的质量和方向，从而创造出更加丰富和真实的交互体验。请在开始教程之前，确保你已阅读配置和辩论。

以下是设置步骤：

1. 定义配置：使用默认配置，或者从`~/.metagpt`目录中加载自定义配置。
2. 分配配置：将特定的`LLM`配置分配给`Role`和`Action`。配置的优先级：`Action config` > `Role config` > `Global config`（`config` in `config2.yaml`）。
3. 团队交互：创建一个带有环境的团队，开始交互。

## 示例

考虑一个美国大选的现场直播环境，我们将创建三个`Role`：`A`、`B`和`C`。`A`和`B`是两个候选人，`C`是一个选民。

### 定义配置

你可以使用默认配置，为不同的`Role`和`Action`配置`LLM`，也可以在`~/.metagpt`目录中加载自定义配置。

```
from metagpt.config2 import Config

# 以下是一些示例配置，分别为gpt-4、gpt-4-turbo 和 gpt-3.5-turbo。
gpt4 = Config.from_home("gpt-4.yaml")  # 从`~/.metagpt`目录加载自定义配置`gpt-4.yaml`
gpt4t = Config.default()  # 使用默认配置，即`config2.yaml`文件中的配置，此处`config2.yaml`文件中的model为"gpt-4-turbo"
gpt35 = Config.default()
gpt35.llm.model = "gpt-3.5-turbo"  # 将model修改为"gpt-3.5-turbo"
```

### 分配配置

创建`Role`和`Action`，并为其分配配置。

```
from metagpt.roles import Role
from metagpt.actions import Action

# 创建a1、a2和a3三个Action。并为a1指定gpt4t的配置。
a1 = Action(config=gpt4t, name="Say", instruction="Say your opinion with emotion and don't repeat it")
a2 = Action(name="Say", instruction="Say your opinion with emotion and don't repeat it")
a3 = Action(name="Vote", instruction="Vote for the candidate, and say why you vote for him/her")

# 创建A，B，C三个角色，分别为“民主党候选人”、“共和党候选人”和“选民”。
# 虽然A设置了config为gpt4，但因为a1已经配置了Action config，所以A将使用model为gpt4的配置，而a1将使用model为gpt4t的配置。
A = Role(name="A", profile="Democratic candidate", goal="Win the election", actions=[a1], watch=[a2], config=gpt4)
# 因为B设置了config为gpt35，而为a2未设置Action config，所以B和a2将使用Role config，即model为gpt35的配置。
B = Role(name="B", profile="Republican candidate", goal="Win the election", actions=[a2], watch=[a1], config=gpt35)
# 因为C未设置config，而a3也未设置config，所以C和a3将使用Global config，即model为gpt4的配置。
C = Role(name="C", profile="Voter", goal="Vote for the candidate", actions=[a3], watch=[a1, a2])
```

请注意，对于关注的`Action`而言，配置的优先级为：`Action config` > `Role config` > `Global config`。不同`Role`和`Action`的配置情况如下：

| Action of interest | Global config | Role config | Action config | Effective config for the Action |
| --- | --- | --- | --- | --- |
| `a1` | `gpt4` | `gpt4` | `gpt4t` | `gpt4t` |
| `a2` | `gpt4` | `gpt35` | unspecified | `gpt35` |
| `a3` | `gpt4` | unspecified | unspecified | `gpt4` |

### 团队交互

创建一个带有环境的团队，并使其进行交互。

```
import asyncio
from metagpt.environment import Environment
from metagpt.team import Team

# 创建一个描述为“美国大选现场直播”的环境
env = Environment(desc="US election live broadcast")
team = Team(investment=10.0, env=env, roles=[A, B, C])
# 运行团队，我们应该会看到它们之间的协作
asyncio.run(team.run(idea="Topic: climate change. Under 80 words per message.", send_to="A", n_round=3))
# await team.run(idea="Topic: climate change. Under 80 words per message.", send_to="A", n_round=3) # 如果在Jupyter Notebook中运行，使用这行代码
```
