
# 客户支持 AI 代理（Mem0 示例）

```python
import os
from openai import OpenAI
from mem0 import Memory

# Set the OpenAI API key
os.environ['OPENAI_API_KEY'] = 'sk-xxx'

class CustomerSupportAIAgent:
    def __init__(self):
        """
        Initialize the CustomerSupportAIAgent with memory configuration and OpenAI client.
        """
        config = {
            "vector_store": {
                "provider": "qdrant",
                "config": {
                    "host": "localhost",
                    "port": 6333,
                }
            },
        }
        self.memory = Memory.from_config(config)
        self.client = OpenAI()
        self.app_id = "customer-support"

    def handle_query(self, query, user_id=None):
        """
        Handle a customer query and store the relevant information in memory.

        :param query: The customer query to handle.
        :param user_id: Optional user ID to associate with the memory.
        """
        # Start a streaming chat completion request to the AI
        stream = self.client.chat.completions.create(
            model="gpt-4",
            stream=True,
            messages=[
                {"role": "system", "content": "You are a customer support AI agent."},
                {"role": "user", "content": query}
            ]
        )
        # Store the query in memory
        self.memory.add(query, user_id=user_id, metadata={"app_id": self.app_id})

        # Print the response from the AI in real-time
        for chunk in stream:
            if chunk.choices[0].delta.content is not None:
                print(chunk.choices[0].delta.content, end="")

    def get_memories(self, user_id=None):
        """
        Retrieve all memories associated with the given customer ID.

        :param user_id: Optional user ID to filter memories.
        :return: List of memories.
        """
        return self.memory.get_all(user_id=user_id)

# Instantiate the CustomerSupportAIAgent
support_agent = CustomerSupportAIAgent()

# Define a customer ID
customer_id = "jane_doe"

# Handle a customer query
support_agent.handle_query("I need help with my recent order. It hasn't arrived yet.", user_id=customer_id)
```

## 获取记忆

你可以用以下代码在任何时间点获取所有记忆：

```python
memories = support_agent.get_memories(user_id=customer_id)
for m in memories:
    print(m['text'])
```

## 要点

- 初始化：使用必要的记忆配置和 `OpenAI` 客户端设置初始化 `CustomerSupportAIAgent` 类。
- 处理查询：`handle_query` 方法向 AI 发送查询并将相关信息存储在记忆中。
- 检索记忆：`get_memories` 方法获取与客户关联的所有存储记忆。

随着对话的进行，`Mem0` 的记忆会根据交互自动更新，提供不断改进的个性化支持体验。

## 参考文档

https://github.com/mem0ai/mem0

https://docs.mem0.ai/overview
