
```
[0;31mInit signature:[0m
[0mAgent[0m[0;34m([0m[0;34m[0m
[0;34m[0m    [0mname[0m[0;34m:[0m [0;34m'str'[0m[0;34m,[0m[0;34m[0m
[0;34m[0m    [0minstructions[0m[0;34m:[0m [0;34m'str | Callable[[RunContextWrapper[TContext], Agent[TContext]], MaybeAwaitable[str]] | None'[0m [0;34m=[0m [0;32mNone[0m[0;34m,[0m[0;34m[0m
[0;34m[0m    [0mhandoff_description[0m[0;34m:[0m [0;34m'str | None'[0m [0;34m=[0m [0;32mNone[0m[0;34m,[0m[0;34m[0m
[0;34m[0m    [0mhandoffs[0m[0;34m:[0m [0;34m'list[Agent[Any] | Handoff[TContext]]'[0m [0;34m=[0m [0;34m<[0m[0mfactory[0m[0;34m>[0m[0;34m,[0m[0;34m[0m
[0;34m[0m    [0mmodel[0m[0;34m:[0m [0;34m'str | Model | None'[0m [0;34m=[0m [0;32mNone[0m[0;34m,[0m[0;34m[0m
[0;34m[0m    [0mmodel_settings[0m[0;34m:[0m [0;34m'ModelSettings'[0m [0;34m=[0m [0;34m<[0m[0mfactory[0m[0;34m>[0m[0;34m,[0m[0;34m[0m
[0;34m[0m    [0mtools[0m[0;34m:[0m [0;34m'list[Tool]'[0m [0;34m=[0m [0;34m<[0m[0mfactory[0m[0;34m>[0m[0;34m,[0m[0;34m[0m
[0;34m[0m    [0minput_guardrails[0m[0;34m:[0m [0;34m'list[InputGuardrail[TContext]]'[0m [0;34m=[0m [0;34m<[0m[0mfactory[0m[0;34m>[0m[0;34m,[0m[0;34m[0m
[0;34m[0m    [0moutput_guardrails[0m[0;34m:[0m [0;34m'list[OutputGuardrail[TContext]]'[0m [0;34m=[0m [0;34m<[0m[0mfactory[0m[0;34m>[0m[0;34m,[0m[0;34m[0m
[0;34m[0m    [0moutput_type[0m[0;34m:[0m [0;34m'type[Any] | None'[0m [0;34m=[0m [0;32mNone[0m[0;34m,[0m[0;34m[0m
[0;34m[0m    [0mhooks[0m[0;34m:[0m [0;34m'AgentHooks[TContext] | None'[0m [0;34m=[0m [0;32mNone[0m[0;34m,[0m[0;34m[0m
[0;34m[0m[0;34m)[0m [0;34m->[0m [0;32mNone[0m[0;34m[0m[0;34m[0m
[0;31mDocstring:[0m     
An agent is an AI model configured with instructions, tools, guardrails, handoffs and more.

We strongly recommend passing `instructions`, which is the "system prompt" for the agent. In
addition, you can pass `description`, which is a human-readable description of the agent, used
when the agent is used inside tools/handoffs.

Agents are generic on the context type. The context is a (mutable) object you create. It is
passed to tool functions, handoffs, guardrails, etc.
[0;31mFile:[0m           ~/miniconda3/lib/python3.12/site-packages/agents/agent.py
[0;31mType:[0m           type
[0;31mSubclasses:[0m     
```

```
Runner?
```

```
[0;31mInit signature:[0m [0mRunner[0m[0;34m([0m[0;34m)[0m[0;34m[0m[0m
[0;31mDocstring:[0m       <no docstring>
[0;31mFile:[0j           ~/miniconda3/lib/python3.12/site-packages/agents/run.py
[0;31mType:[0m           type
[0;31mSubclasses:[0m     
```

# 一、各类模型&服务接入Agent SDK流程

## 1.OpenAI+国内反向代理地址接入Agent SDK

```
from openai import AsyncOpenAI
from agents import OpenAIChatCompletionsModel, Agent, Runner
from agents.model_settings import ModelSettings
from agents import set_default_openai_client, set_tracing_disabled
```

```
external_client = AsyncOpenAI(
    base_url = base_url,
    api_key = openai_api_key, 
)
```

```
set_default_openai_client(external_client)
set_tracing_disabled(True)
```

```
agent = Agent(name="Assistant", instructions="你是一名助人为乐的助手。")
```

```
result = await Runner.run(agent, "请写一首关于编程中递归的俳句。") 
```

> 俳（pái）句。

```
print(result.final_output)
```

```
函数自调用，  
循环无尽意境，  
解题如水流。
```

## 2.DeepSeek在线API接入流程

### 测试API能否连接

```
ds_api_key = 'YOUR_DS_API'
```

```
# 实例化客户端
client = OpenAI(api_key=ds_api_key, 
                base_url="https://api.deepseek.com")
```

```
# 调用 deepseekv3 模型
response = client.chat.completions.create(
    model="deepseek-chat",
    messages=[
        {"role": "user", "content": "你好，好久不见!请介绍下你自己。"}
    ]
)
```

```
# 输出生成的响应内容
print(response.choices[0].message.content)
```

```
你好！很高兴再次见到你。我是一个由深度求索（DeepSeek）公司开发的智能助手，专门设计用来提供信息查询、数据分析和解答各种问题。我能够理解和处理自然语言，帮助用户获取所需的知识或执行特定任务。如果你有任何问题或需要帮助，随时可以告诉我。
```

### 测试接入Agent SDK

```
external_client = AsyncOpenAI(
    base_url = "https://api.deepseek.com",
    api_key = ds_api_key, 
)
```

```
set_default_openai_client(external_client)
set_tracing_disabled(True)
```

```
agent = Agent(name="Assistant", 
              instructions="你是一名助人为乐的助手。",
              model=OpenAIChatCompletionsModel(
                  model="deepseek-chat",
                  openai_client=external_client,
              ))
```

```
result = await Runner.run(agent, "请写一首关于编程中递归的俳句。") 
```

```
print(result.final_output)
```

```
函数自调用，
层层深入解难题，
递归显神通。
```

## 3.`ollama`+本地模型接入流程

这里选择QwQ模型作为本地模型进行接入测试，更多QwQ模型本地部署与调用介绍，详见：https://www.bilibili.com/video/BV1hZRTYUEtR/

![](img/a9f0dc73beb8a531ebc04a32194491ca.png)

这里需要先启动后台`ollama`服务，然后进行连接测试

```
client = OpenAI(
    base_url='http://localhost:11434/v1/',
    api_key='ollama',  # required but ignored
)
```

```
prompt = "你好，好久不见！"
messages = [
    {"role": "user", "content": prompt}
]
```

```
response = client.chat.completions.create(
    messages=messages,
    model='qwq-32b-bnb',
)

print(response.choices[0].message.content)
```

```
嗯，用户说“你好，好久不见！”，看起来是想打招呼并且表达一段时间没联系了。首先，我需要回应他们的问候，保持友好和热情。然后，要回应“好久不见”这部分，可能可以问他们最近在做什么，或者有没有什么新鲜事，这样可以让对话继续下去。

接下来要考虑用户的身份和我们的关系。可能我们之前有过互动，所以用户觉得熟悉。这时候应该营造一种亲切的氛围，让用户感到被记得和关心。可能需要加入一些开放性的问题，鼓励用户分享更多信息，比如“最近在忙什么有趣的项目吗？”或者“有什么新鲜事想聊聊吗？”

同时要注意不要过于侵入隐私，问题要轻松自然。另外，保持回答简洁但足够友好，避免太长的句子。可能需要用表情符号来增加亲切感，但根据之前的回复示例，用户没有使用表情，所以或许暂时不加。

还要检查是否有潜在的需求，比如用户可能想分享近况，或者寻求帮助，但目前只是寒暄阶段。所以先回应问候，然后邀请他们分享更多，这样既符合当前情境，也为后续对话打下基础。

最后确保语言口语化，避免生硬。例如“最近过得怎么样？”比“请问你最近的情况如何？”更自然。综合起来，回复可以是：“你好！确实好久没联系了，希望你一切顺利～最近在忙什么有趣的项目吗？或者有什么新鲜事想聊聊吗？😄” 这样既回应了问候，又引导用户进一步交流。
</think>

你好！确实好久不见啦！最近过得怎么样？有没有遇到什么有趣的事或者需要帮忙的地方呢？😊
```

### 测试接入Agent SDK

```
external_client = AsyncOpenAI(
    base_url = 'http://localhost:11434/v1',
    api_key='ollama', 
)
```

```
set_default_openai_client(external_client)
set_tracing_disabled(True)
```

```
agent = Agent(name="Assistant", 
              instructions="你是一名助人为乐的助手。",
              model=OpenAIChatCompletionsModel(
                  model="qwq-32b-bnb",
                  openai_client=external_client,
              ))
```

```
result = await Runner.run(agent, "请写一首关于编程中递归的俳句。") 
```

```
print(result.final_output)
```

```
嗯，用户让我写一首关于编程中递归的俳句。首先，我得回忆一下俳句的结构。俳句是日本的一种短诗，通常由三行组成，音节分布为5-7-5。虽然中文俳句不一定严格遵循这个音节规则，但大致保持这样的结构会比较好。

接下来主题是递归，编程中的递归是指函数自己调用自己的过程。需要找到能体现递归特点的意象或者比喻。常见的比如镜子反射、俄罗斯套娃，或者自然界的重复图案，比如分形、树的分支等。

然后要考虑俳句通常包含季节关键词（季语），不过中文俳句可能不那么严格，但如果有合适的季节词会更符合传统。比如“秋夜”可以设定场景，同时营造一种静谧的氛围，适合思考递归时的情景。

第一行需要5个音节。“镜中映月影”这里，“镜”代表反射或重复，“映月影”可能指不断重复的过程，五个字刚好。第二行7个音节：“无限循环深处藏”，描述递归中的循环和隐藏的终止条件，但“无限”其实递归有基例，所以或许需要调整。“深处藏”暗示终点的存在？第三行5个音节：“栈溢处，终章。” 栈溢出是递归过深的常见错误，“终章”既指程序结束也可能指递归终止的时候。

可能需要注意术语是否准确，比如“栈溢处”指的是栈溢出，也就是Stack Overflow，这确实是递归中常见的问题。最后一句“终章”可以双关，既是程序崩溃也是递归的终点。这样组合起来应该能表达递归的特点和潜在的问题。
</think>

镜中映月影  
循环深处藏基例  
栈溢处，终章  

注：俳句通过"镜中映月"隐喻递归自我调用特性，"基例"点明终止条件，末句以栈溢出这一经典隐患收束。采用5-7-5结构并融入编程术语，将算法之美与危机浓缩于三行之中，在静寂中传递递归运行的动态张力。
```

### Function calling功能测试

```
ollama_model=OpenAIChatCompletionsModel(
    model="qwq",
    openai_client=external_client,
)
```
