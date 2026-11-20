
# 初始化存储组件

```
checkpointer = InMemorySaver()
memory_store = InMemoryStore()


def generate_joke(state: State, store: BaseStore) -> dict[str, str]:
    """生成具有内存感知上下文选择的笑话。

    该函数展示了在生成新内容之前从内存中选择上下文，以确保一致性并避免重复。

    参数:
        state: 包含主题的当前状态
        store: 用于持久化上下文的内存存储
        
    返回:
        包含生成的笑话的字典
    """
    # 如果存在，从内存中选择之前的笑话
    prior_joke = store.get(namespace, "last_joke")
    if prior_joke:
        prior_joke_text = prior_joke.value["joke"]
        print(f"之前的笑话：{prior_joke_text}")
    else:
        print("之前的笑话：无！")

    # 生成一个与之前不同的新笑话
    prompt = (
        f"写一个关于{state['topic']}的短笑话，"
        f"但要与你之前写过的任何笑话不同：{prior_joke_text if prior_joke else '无'}"
    )
    msg = llm.invoke(prompt)

    # 将新笑话存储到内存中，供未来的上下文选择使用
    store.put(namespace, "last_joke", {"joke": msg.content})

    return {"joke": msg.content}
```

现在，可以像之前一样简单地执行这个具有内存感知的工作流。

## 构建具有内存感知的工作流

```
workflow = StateGraph(State)
workflow.add_node("generate_joke", generate_joke)

# 连接工作流
workflow.add_edge(START, "generate_joke")
workflow.add_edge("generate_joke", END)

# 结合检查点和内存存储进行编译
chain = workflow.compile(checkpointer=checkpointer, store=memory_store)

# 用第一个线程执行工作流
config = {"configurable": {"thread_id": "1"}}
joke_generator_state = chain.invoke({"topic": "cats"}, config)
```

### 输出

```
之前的笑话：无！
```

未检测到之前的笑话。现在可以打印最新的状态结构。

## 获取图的最新状态

```
latest_state = chain.get_state(config)

console.print("\n[bold magenta]最新图状态：[/bold magenta]")
pprint(latest_state)
```

### 输出

### 最新状态的输出

```
StateSnapshot(
    values={
        'topic': 'cats',
        'joke': "Here's a new one:\n\nWhy did the cat join a band?\n\nBecause it wanted to be the purr-cussionist!"
    },
    next=(),
    config={
        'configurable': {
            'thread_id': '1',
            'checkpoint_ns': '',
            'checkpoint_id': '1f068357-cc8d-68cb-8001-31f64daf7bb6'
        }
    },
    metadata={'source': 'loop', 'step': 1, 'parents': {}},
    created_at='2025-07-24T02:25:38.457825+00:00',
    parent_config={
        'configurable': {
            'thread_id': '1',
            'checkpoint_ns': '',
            'checkpoint_id': '1f068357-c459-6deb-8000-16ce383a5b6b'
        }
    },
    tasks=(),
    interrupts=()
)
```

从内存中获取之前的笑话，并将其传递给LLM以进行改进。

## 使用第二个线程执行工作流

```
config = {"configurable": {"thread_id": "2"}}
joke_generator_state = chain.invoke({"topic": "cats"}, config)
```

### 输出

```
之前的笑话：Here is a new one:
Why did the cat join a band?
Because it wanted to be the purr-cussionist!
```

它已成功地从内存中获取了正确的笑话，并按预期对其进行了改进。
