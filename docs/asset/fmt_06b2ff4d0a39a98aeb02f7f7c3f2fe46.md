
# 智能体上下文变量与函数调用示例

```python
[{'content': None,
  'refusal': None,
  'role': 'assistant',
  'function_call': None,
  'tool_calls': [{'id': 'call_DCv7cLjDRocj6oL6DLHtPMEl',
    'function': {'arguments': '{"loc":"Beijing"}', 'name': 'get_weather_None'},
    'type': 'function'}],
  'sender': 'Agent'},
 {'role': 'tool',
  'tool_call_id': 'call_DCv7cLjDRocj6oL6DLHtPMEl',
  'tool_name': 'get_weather_None',
  'content': 'Error: 网络问题，无法查到天气信息'},
 {'content': '对不起，我无法查到北京的天气信息。请稍后再试或者访问其他天气信息网站查看。',
  'refusal': None,
  'role': 'assistant',
  'function_call': None,
  'tool_calls': None,
  'sender': 'Agent'}]
```

```python
display(Markdown(response.messages[-1]['content']))
```

对不起，我无法查到北京的天气信息。请稍后再试或者访问其他天气信息网站查看。

## 使用 context_variables 字段自动传入参数

```python
def greet(context_variables, language):
   user_name = context_variables["user_name"]
   greeting = "你好" if language == "Chinese" else "Hello"
   print(f"{greeting}, {user_name}!")
   return "Done"
```

```python
agent = Agent(
   functions=[greet]
)
```

```python
response = swarm_client.run(
   agent=agent,
   messages=[{"role": "user", "content": "请调用 greet()。并用中文回答。"}],
   context_variables={"user_name": "陈明"}
)
```

你好, 陈明!

```python
print(response.messages[-1]["content"])
```

任务已完成。

```python
response
```

```python
Response(messages=[{'content': None, 'refusal': None, 'role': 'assistant', 'function_call': None, 'tool_calls': [{'id': 'call_5VLkUHxsSBddMsMiCM9nFxKL', 'function': {'arguments': '{"language":"Chinese"}', 'name': 'greet'}, 'type': 'function'}], 'sender': 'Agent'}, {'role': 'tool', 'tool_call_id': 'call_5VLkUHxsSBddMsMiCM9nFxKL', 'tool_name': 'greet', 'content': 'Done'}, {'content': '任务已完成。', 'refusal': None, 'role': 'assistant', 'tool_calls': None, 'sender': 'Agent'}], agent=Agent(name='Agent', model='gpt-4o', instructions='You are a helpful agent.', functions=[<function greet at 0x7fde8b8b7c40>], tool_choice=None, parallel_tool_calls=True), context_variables={'user_name': '陈明'})
```

这段代码的主要展示如何使用上下文变量和用户输入来动态生成问候语，并通过智能体调用一个函数。在这个例子中，函数 `greet()` 根据上下文变量中的 `user_name` 和语言选择来生成个性化的问候语。下面是代码的逐步解释：

1. `context_variables`：这是一个字典，存储了智能体的上下文信息。在这个例子中，`context_variables` 包含了用户的名字 `user_name`，并通过它来生成个性化的问候。

2. `language` 参数：这是函数的第二个参数，用于决定使用哪种语言问候用户。如果传入的语言是 "Chinese"，则使用中文问候 "你好"，否则使用英语问候 "Hello"。

3. 问候生成：根据提供的 `language` 参数和上下文中的 `user_name`，函数会生成一个如 "Hello, 陈明!" 或 "你好, 陈明!" 的问候语，并打印出来。

4. 返回值：函数最后返回 "Done"，表示任务已完成。

### 智能体设置

```python
agent = Agent(
   functions=[greet]  # 将 greet 函数注册到智能体中，智能体可以调用这个函数
)
```

这里创建了一个智能体，并将 `greet` 函数注册为它可以调用的函数之一。这意味着当用户的消息请求调用 `greet()` 时，智能体会执行这个函数。

### 运行智能体

1. `agent=agent`：运行的智能体是我们刚刚定义的 `agent`，它有一个可调用的 `greet()` 函数。

2. `messages=[...]`：这是用户发来的消息。在这个例子中，用户发送了一条消息 "请调用 greet()"，实际上提示了智能体需要调用 `greet()` 函数。

3. `context_variables={"user_name": "陈明"}`：上下文变量中包含用户的名字 "陈明"，`greet()` 函数会根据这个名字生成个性化的问候语。

### 将 `Agent` 视作返回对象

智能体可以通过函数返回另一个智能体来进行交接。

```python
sales_agent = Agent(name="销售智能体")

def transfer_to_sales():
   return sales_agent

agent = Agent(functions=[transfer_to_sales])

response = swarm_client.run(agent, [{"role":"user", "content":"请转接到销售。"}])
print(response.agent.name)
```

销售智能体

```python
response
```

```python
Response(messages=[{'content': None, 'refusal': None, 'role': 'assistant', 'function_call': None, 'tool_calls': [{'id': 'call_LDMmxqmwITzJMCFvnyCGhMfA', 'function': {'arguments': '{}', 'name': 'transfer_to_sales'}, 'type': 'function'}], 'sender': 'Agent'}, {'role': 'tool', 'tool_call_id': 'call_LDMmxqmwITzJMCFvnyCGhMfA', 'tool_name': 'transfer_to_sales', 'content': '{"assistant": "\\u9500\\u552e\\u667a\\u80fd\\u4f53"}'}, {'content': '我将您转接到销售团队的智能助手来解答您的销售相关问题。请问有什么我可以帮助您的？', 'refusal': None, 'role': 'assistant', 'function_call': None, 'tool_calls': None, 'sender': '销售智能体'}], agent=Agent(name='销售智能体', model='gpt-4o', instructions='You are a helpful agent.', functions=[], tool_choice=None, parallel_tool_calls=True), context_variables={})
```

```python
response.messages
```

```python
[{'content': None,
  'refusal': None,
  'role': 'assistant',
  'function_call': None,
  'tool_calls': [{'id': 'call_LDMmxqmwITzJMCFvnyCGhMfA',
    'function': {'arguments': '{}', 'name': 'transfer_to_sales'},
    'type': 'function'}],
  'sender': 'Agent'},
 {'role': 'tool',
  'tool_call_id': 'call_LDMmxqmwITzJMCFvnyCGhMfA',
  'tool_name': 'transfer_to_sales',
  'content': '{"assistant": "\\u9500\\u552e\\u667a\\u80fd\\u4f53"}'},
 {'content': '我将您转接到销售团队的智能助手来解答您的销售相关问题。请问有什么我可以帮助您的？',
  'refusal': None,
  'role': 'assistant',
  'function_call': None,
  'tool_calls': None,
  'sender': '销售智能体'}]
```

```python
response.messages[-1]
```

```python
{'content': '我将您转接到销售团队的智能助手来解答您的销售相关问题。请问有什么我可以帮助您的？',
 'refusal': None,
 'role': 'assistant',
 'function_call': None,
 'tool_calls': None,
 'sender': '销售智能体'}
```

## 智能体选择

```python
agent_A = Agent(
    name="中译英智能体",
    instructions="无论用户发送的消息是什么语言，请用英文进行回答。"
)
```

```python
agent_B = Agent(
    name = "英译中智能体",
    instructions="无论用户发送的消息是什么语言，请用中文进行回答。"
)
```

```python
def select_agent(agent_type):
    """
    智能体选择函数，根据输入的agent_type灵活选择不同类型的智能体。
    agent_type需要是一个字符串，当输入agent_type="A"时，代表选择agent_A，该智能体负责完成中文翻译到英文的任务；
    当输入agent_type="B"时，代表选择agent_B，该智能体负责完成英文翻译到中文的任务。
    """
    if agent_type == "A":
        return agent_A
    elif agent_type == "B":
        return Agent_B
    else:
        return "没有可执行该功能的智能体"
```

```python
agent = Agent(functions=[select_agent])
```

```python
response = swarm_client.run(
    agent=agent,
    messages=[{"role":"user", "content":"你好，我想请你协助我完成一个中译英的工作，可以么？"}]
)
```

```python
response.messages
```
