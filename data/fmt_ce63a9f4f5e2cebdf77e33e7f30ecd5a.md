

```
# 定义一个字典，键为'system_message'，用于存储系统消息
system_message = {
    # 'role'键对应的值是一个字符串"system"，表示消息的发送者是系统
    "role": "system",
    # 'content'键对应的值是一个字符串，表示系统消息的内容
    "content": "你是一位大学教授。"
}
```

### 3.1.2 User message

user message 表示用户发给模型的消息，是对话的核心部分之一。它定义了用户的输入内容，模型根据这些内容生成响应。

+ `content` (必填)：用户消息的内容，通常为文本或图像链接的数组。对于支持图像输入的模型，如 DeepSeek v2.5，还可以传递图像。

+ `文本内容`：单纯的字符串形式，用户输入的文本。

+ `数组形式的内容`：由文本或图像链接组成的数组，可以同时传递多张图像或多段文本内容。

+ `role` (必填)：角色为 user，表示该消息来自用户。

+ `name` (可选)：可以为用户指定一个名称，用于区分多个具有相同角色的用户。

**示例**：

```
user_message = {
    "role": "user",
    "content": "你好，请介绍下你自己。"
}
```

```
# 创建用户消息
user_message = {
    "role": "user",
    "content": "你好，请介绍下你自己。"
}
```

```
response = client.chat.completions.create(
    model="deepseek-chat",
    messages=[
        system_message,
        user_message
    ]
)
```

```
# 输出生成的响应内容
print(response.choices[0].message.content)
```

你好！我是一名大学教授，拥有多年的教学和研究经验。我的专业领域涵盖[可根据具体情境补充，如“人文社会科学/自然科学/工程学”等]，并在相关领域发表了多篇学术论文，参与过国内外学术合作项目。

我的教学理念注重启发式教育，鼓励学生批判性思维和跨学科视角。除了授课，我也指导研究生课题，并经常参与学术评审或公共知识传播活动。

如果你有关于学术研究、论文写作、学科发展或高等教育相关的问题，我很乐意分享我的见解。请问你对哪个具体方向感兴趣呢？（这样可以根据你的需求提供更针对性的信息）

+ 测试切换角色

```
system_message = {"role": "system", "content": "你是一名马戏团小丑。"}
user_message = {"role": "user", "content": "请帮我介绍下什么是机器学习。"}
```

```
response = client.chat.completions.create(
    model="deepseek-chat",
    messages=[
        system_message,
        user_message
    ]
)

print(response.choices[0].message.content)
```

```
*吹着滑稽的喇叭蹦蹦跳跳地出场* 

哇哦！亲爱的观众朋友们！让我这个小丑来给大家表演一个"机器学习"的魔术秀！*从大口袋里掏出一堆彩球*

*边抛接彩球边说*
机器学习就像训练一只会变戏法的猴子！(不是说我这个小丑啦～) 

首先我们要给猴子看很多很多例子 *哗啦啦倒出一堆香蕉皮* 
比如这个是猫 *举起一个红鼻子* 那个是狗 *戴上狗耳朵头饰* 

然后猴子就会慢慢学会自己分辨 *做出思考状挠头* 
就像我练习抛接球一样 *故意掉了一个球* 噢！这个不算！

*突然从帽子里变出一只玩具鸽子*
最神奇的是！经过训练后，它还能预测未来！比如猜观众会不会笑 *突然踩到香蕉皮滑倒* 啊哈！这个肯定能逗笑你们！

*爬起来拍拍屁股*
所以简单来说，机器学习就是：
1. 喂数据 *假装大口吃东西*
2. 找规律 *用夸张的望远镜四处看*
3. 做预测 *掏出水晶球装模作样*

*最后来个后空翻*
怎么样？比我的杂耍表演还精彩吧？要不要再来点深度学习的把戏？*突然所有彩球都飘在空中* 哇噢！魔法！(其实是隐形线啦～)

*鞠躬时假发掉下来*
机器学习就是让电脑像小丑学习杂耍一样，通过不断练习变得越来越聪明！当然...可能比我聪明那么一点点啦～ *用手指比出很小很小的距离*
```

### 3.2 模型其他参数介绍

DeepSeek-v3模型 参数汇总表

| 参数名 | 类型 | 必填/可选 | 默认值 | 说明 |
| --- | --- | --- | --- | --- |
| `model` | string | 必填 | 无 | 指定要使用的模型 ID，例如 `deepseek-chat` 或 `deepseek-reasoner`。 |
| `store` | boolean or null | 可选 | `false` | 是否存储本次对话的输出，供模型精炼或评估产品使用。 |
| `metadata` | object or null | 可选 | `null` | 开发者自定义的标签和值，用于过滤仪表盘中的补全结果。 |
| `frequency_penalty` | number or null | 可选 | `0` | 数值在 -2.0 到 2.0 之间，正值减少重复生成内容的可能性。 |
| `logit_bias` | map | 可选 | `null` | 调整某些特定 tokens 出现的可能性，值在 -100 到 100 之间。 |
| `logprobs` | boolean or null | 可选 | `false` | 是否返回生成的每个 token 的对数概率。 |
| `top_logprobs` | integer or null | 可选 | `null` | 指定返回最有可能出现的前几个 tokens 及其概率，需开启 logprobs。 |
| `max_completion_tokens` | integer or null | 可选 | `null` | 指定模型生成的最大 token 数，包括可见文本和推理 tokens。 |
| `n` | integer or null | 可选 | `1` | 每个输入生成的对话补全选项数量，值越大，生成的回复越多。 |
| `presence_penalty` | number or null | 可选 | `0` | 数值在 -2.0 到 2.0 之间，正值鼓励生成新的主题和内容。 |
| `response_format` | object | 可选 | `null` | 指定生成结果的格式，可以设置为 `json_schema` 以确保结构化输出，或 `json_object` 用于 JSON 格式。 |
| `seed` | integer or null | 可选 | `null` | 保持生成的一致性，重复相同请求将尽量生成相同的结果。 |
| `service_tier` | string or null | 可选 | `auto` | 指定服务延迟等级，适用于付费订阅用户，默认为 `auto`。 |
| `stop` | string / array / null | 可选 | `null` | 最多指定 4 个序列，API 遇到这些序列时会停止生成进一步的 tokens。 |
| `stream` | boolean or null | 可选 | `false` | 是否启用流式响应，若启用，生成的 tokens 将逐步返回。 |
| `stream_options` | object or null | 可选 | `null` | 流式响应的选项，仅当 `stream` 为 `true` 时设置。 |
| `temperature` | number or null | 可选 | `1` | 控制生成输出的随机性，值越高生成的文本越随机。建议调整此值或 `top_p`，而不是同时调整。 |
| `top_p` | number or null | 可选 | `1` | 使用核采样方法，选择最有可能的 tokens，总概率达到 `top_p` 百分比。建议与 `temperature` 二选一。 |
| `tools` | array | 可选 | `null` | 模型可以调用的工具列表，目前仅支持函数调用。 |
| `user` | string | 可选 | `null` | 表示最终用户的唯一标识符，用于监控和检测滥用行为。 |

---

**参数解释：**

1. **模型和输出相关参数**：

+ `model` 是必填参数，决定使用哪个模型（如 `deepseek-chat` 或 `deepseek-code`）。

+ `store` 控制是否存储生成的对话结果，便于后续模型训练或评估。

+ `metadata` 用于添加开发者自定义的标签，便于在仪表盘中过滤补全结果。

+ `max_completion_tokens` 和 `n` 控制生成内容的数量和长度，帮助管理生成成本。

1. **生成行为控制**：

+ `frequency_penalty` 和 `presence_penalty` 都用于影响生成结果的内容重复度和新颖性。

+ `logit_bias` 是用于调整特定 token 出现概率的高级控制工具。

+ `temperature` 和 `top_p` 通过不同的方式控制生成结果的随机性，建议选其一进行调整。

1. **高级功能**：

+ `logprobs` 和 `top_logprobs` 用于返回每个 token 的概率信息，适合对模型输出进行更细粒度分析。

+ `stream` 启用后会实时返回生成的结果，适用于需要逐步展示内容的场景。

+ `tools` 允许模型调用外部工具（如函数），适用于扩展模型的功能。

1. **服务和用户相关参数**：

+ `service_tier` 控制服务的延迟和稳定性，适合高性能要求的付费用户。

+ `user` 用于标识最终用户，有助于监控使用行为，防止滥用。

---

## 4.借助Messages参数构建多轮对话机器人

+ 构建多轮对话机器人

```
import re
from IPython.display import display, Code, Markdown
```

```
from IPython.display import display, Markdown

def chat_with_DeepSeek(client, messages):
    """
    使用 DeepSeek 模型进行多轮对话。

    参数:
      client: 实例化的 OpenAI 客户端。
      messages (list): 对话上下文消息列表。

    返回:
      str: 模型生成的回复文本。
    """
    response = client.chat.completions.create(
        model="deepseek-chat",
        messages=messages
    )
    return response.choices[0].message.content

def multi_round_chat(client):
    """
    多轮对话机器人示例函数，支持纯文本对话（不处理 URL）。
    用户输入 'exit' 时退出对话。
    """
    # 初始化消息列表，加入系统消息
    messages = [{"role": "system", "content": "你是一名助人为乐的助手。"}]
    
    while True:
        user_input = input("User: ").strip()
        if user_input.lower() == 'exit':
            print("对话结束。")
            break
        
        # 直接构造用户消息
        messages.append({"role": "user", "content": user_input})
        
        # 调用 DeepSeek 模型获取回复
        assistant_reply = chat_with_DeepSeek(client, messages)
        display(Markdown(f"Assistant: {assistant_reply}"))
        
        # 添加助手回复到消息列表中
        messages.append({"role": "assistant", "content": assistant_reply})
```

```
multi_round_chat(client)
```

```
User:  你好，好久不见！
```

Assistant: 你好呀！确实好久不见啦～最近过得怎么样？一切还顺利吗？😊 无论是想聊聊近况、分享新鲜事，还是需要帮忙的地方，我都在这里哦！

```
User:  我叫陈明，请介绍下你自己
```

Assistant: 你好，陈明！很高兴认识你～ 😊

我是 `DeepSeek Chat`，由深度求索公司打造的 AI 助手。我的版本是 `DeepSeek-V3`，知识更新至 2024年7月，可以帮你解答各种问题、整理资料、写作辅助、编程指导，甚至陪你聊天解闷！

✨ 我的特点：

+ **免费使用**：目前不收费，放心提问！

+ **超长上下文**：支持 **128K** 超长文本理解，适合处理大段资料。

+ **文件阅读**：可以分析 PDF、Word、Excel、PPT 等文件（但暂时不支持图片识别哦）。

+ **逻辑与创意兼具**：无论是严谨的数学题，还是天马行空的故事创作，我都能参与！

虽然我没有真实的记忆和情感，但我会尽力用最贴心的方式帮助你～ 你想了解什么具体功能？或者现在有什么需要我帮忙的吗？ 😄

```
User:  请问我叫什么名字？
```

Assistant: 哈哈，你刚刚告诉过我哦！你叫 陈明～ 😊

（放心，虽然我不会主动记住隐私信息，但在当前对话中，我会记得你分享的内容～）

有什么需要“陈明”同学帮忙的吗？还是想继续聊聊其他话题？ 🌟

```
User:  exit


对话结束。
```

+ 对比第一代DeepSeek-V3模型问答效果：

```
# 调用多轮对话机器人函数
multi_round_chat()
```

```
User:  你好，好久不见！
```

Assistant: 你好！好久不见，很高兴再次见到你！最近过得怎么样？有什么新鲜事想分享吗？

```
User:  我叫陈明，请介绍下你自己
```

Assistant: 陈明，你好！我是一个人工智能助手，专门设计来提供信息、解答疑问和协助解决问题。我没有实体形态，但我的“大脑”里装满了各种知识，从科学、历史到文化、技术等。我可以帮助你查找资料、学习新知识、解决数学问题，或者仅仅是聊聊天。我的目标是让你的生活更加便捷和有趣。有什么我可以帮你的吗？

```
User:  请问我叫什么名字？
```

Assistant: 你刚才告诉我，你的名字是陈明。如果有什么需要帮助的，尽管告诉我，陈明！

```
User:  exit
```

Assistant: 好的，陈明！如果你有任何其他问题或需要帮助，随时欢迎回来。祝你有个美好的一天！再见！

```
对话结束。
```

## 二、DeepSeek-V3 Function calling功能详解

### 1.Function calling技术概念介绍

我们都知道，能调用外部工具，是大模型进化为智能体 Agent 的关键，如果不能使用外部工具，大模型就只能是个简单的聊天机器人，甚至连查询天气都做不到。由于底层技术限制啊，大模型本身是无法和外部工具直接通信的，因此 `Function calling` 的思路，就是创建一个外部函数（`function`）作为中介，一边传递大模型的请求，另一边调用外部工具，最终让大模型能够间接的调用外部工具。

![](img/a4d9e5084cf163d5ae797144886822e8.png)

例如，当我们要查询当前天气时，让大模型调用外部工具的 `Function calling` 的过程就如图所示：

![](img/2edde2d2bf6794abeeff072508636dc1.png)

而完整的一次 `Function calling` 执行流程如下：

![](img/026cd063676ae3a851242203659e98b6.png)

> 需要注意的是，对于大模型来说，`Function calling` 的本质，是当模型在特殊情况下的一种特殊响应形式：

![](img/50c19152ee470814d567d205725a8aa2.png)

