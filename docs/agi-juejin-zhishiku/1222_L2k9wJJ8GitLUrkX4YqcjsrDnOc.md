# Mem0：用于个性化AI的记忆层

> 来源：[https://agijuejin.feishu.cn/docx/Xg8Zdc9jHor2JJxT2xNcDvXMnGd](https://agijuejin.feishu.cn/docx/Xg8Zdc9jHor2JJxT2xNcDvXMnGd)

> 作者：lucas大叔

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/710143375)hihu.com/p/710143...

Mem0为LLM提供智能、自我改进的记忆层，实现跨应用的个性化AI体验。

## 核心特性

+   用户、会话和AI智能体记忆：保留跨用户会话、交互和AI智能体的信息，确保连续性和上下文。

+   自适应个性化：根据用户交互和反馈持续改进个性化。

+   开发者友好的API：提供简单的API用于无缝集成到各种应用中。

+   平台一致性：确保跨不同平台和设备的行为和数据一致。

+   托管服务：提供托管解决方案，便于部署和维护。

## 使用场景

+   个性化学习助手：长期记忆允许学习助手记住用户偏好、过去的交互和进度，提供更量身定制和有效的学习体验。

+   客户支持AI智能体：通过保留以前交互的信息，客户支持机器人可以提供更准确和上下文感知的帮助，提高客户满意度并缩短解决时间。

+   [医疗助理 ](https://www.zhihu.com/search?q=%E5%8C%BB%E7%96%97%E5%8A%A9%E7%90%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710143375%7D)：长期记忆使医疗助理能够跟踪患者病史、用药时间表和治疗计划，确保个性化和一致的护理。

+   虚拟伴侣：虚拟伴侣可以利用长期记忆，通过记住个人细节、偏好和过去的对话，与用户建立更深层次的关系，使互动更有意义。

+   生产力工具：长期记忆有助于生产力工具记住用户习惯、常用文档和任务历史，简化工作流程并提高效率。

+   游戏AI：在游戏中，具有长期记忆的AI可以通过记住玩家的选择、策略和进度来创造更身临其境的体验，从而相应地调整游戏环境。

## 与RAG的差异

与检索增强生成（RAG）相比，Mem0的LLM记忆实现提供了几个优势：

+   实体关系：Mem0可以跨不同的交互理解和关联实体，不像RAG从静态文档中检索信息。这样对上下文和关系的理解更深入。

+   最近性、相关性和衰减： Mem0优先考虑最近的交互并逐渐忘记过时的信息，确保记忆保持相关和最新，以获得更准确的响应。

+   上下文连续性： Mem0跨会话保留信息，保持对话和交互的连续性，这对于虚拟伴侣或个性化学习助手等长期参与应用至关重要。

+   自适应学习：Mem0根据用户交互和反馈改进其个性化，使记忆随着时间的推移更加准确并为个人用户量身定制。

+   动态更新：Mem0可以使用新的信息和交互动态更新记忆，不像RAG依赖于静态数据。这允许实时调整和改进，增强用户体验。

## 快速入门

### 安装

```
pip install mem0ai
```

### 基本用法

+   Mem0初始化

```
from mem0 import Memory
m = Memory()
```

+   存储记忆

```
# For a user
result = m.add(&quot;Likes to play cricket on weekends&quot;, user_id=&quot;alice&quot;, metadata={&quot;category&quot;: &quot;hobbies&quot;})
print(result)
```

输出

```
[
  {
    &#x27;id&#x27;: &#x27;m1&#x27;,
    &#x27;event&#x27;: &#x27;add&#x27;,
    &#x27;data&#x27;: &#x27;Likes to play cricket on weekends&#x27;
  }
]
```

+   检索记忆

获取所有记忆

```
# Get all memories
all_memories = m.get_all()
print(all_memories)
```

输出

```
[
  {
    &#x27;id&#x27;: &#x27;m1&#x27;,
    &#x27;text&#x27;: &#x27;Likes to play cricket on weekends&#x27;,
    &#x27;metadata&#x27;: {
      &#x27;data&#x27;: &#x27;Likes to play cricket on weekends&#x27;,
      &#x27;category&#x27;: &#x27;hobbies&#x27;
    }
  },
  # ... other memories ...
]
```

通过ID获取 [单条记忆](https://www.zhihu.com/search?q=%E5%8D%95%E6%9D%A1%E8%AE%B0%E5%BF%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710143375%7D)

```
[# Get a single ](https://www.zhihu.com/search?q=specific_memory&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710143375%7D)memory by ID
specific_memory = m.get(&quot;m1&quot;)
print(specific_memory)
```

输出

```
{
 [ &#x27;id&#x27;: &#x27;](https://www.zhihu.com/search?q=metadata&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710143375%7D)m1&#x27;,
  &#x27;text&#x27;: &#x27;Likes to play cricket on weekends&#x27;,
  &#x27;metadata&#x27;: {
    &#x27;data&#x27;: &#x27;Likes to play cricket on weekends&#x27;,
    &#x27;category&#x27;: &#x27;hobbies&#x27;
  }
}
```

+   搜索记忆

```
related_memories = m.search(query=&quot;What are Alice&#x27;s hobbies?&quot;, user_id=&quot;alice&quot;)
print(related_memories)
```

输出

```
[
  {
    &#x27;id&#x27;: &#x27;m1&#x27;,
    &#x27;text&#x27;: &#x27;Likes to play cricket on weekends&#x27;,
    &#x27;metadata&#x27;: {
      &#x27;data&#x27;: &#x27;Likes to play cricket on weekends&#x27;,
      &#x27;category&#x27;: &#x27;hobbies&#x27;
    },
    &#x27;score&#x27;: 0.85  # Similarity score
  },
  # ... other related memories ...
]
```

+   更新记忆

```
result = m.update(memory_id=&quot;m1&quot;, data=&quot;Likes to play tennis on weekends&quot;)
print(result)
```

输出

```
{
  &#x27;id&#x27;: &#x27;m1&#x27;,
  &#x27;event&#x27;: &#x27;update&#x27;,
  &#x27;data&#x27;: &#x27;Likes to play tennis on weekends&#x27;
}
```

+   记忆历史

```
history = m.history(memory_id=&quot;m1&quot;)
print(history)
```

输出

```
[
  {[
    &#x27;id&#x27;:](https://www.zhihu.com/search?q=prev_value&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710143375%7D) &#x27;h1&#x27;,
    &#x27;memory_id&#x27;: &#x27;m1&#x27;,
    &#x27;prev_value&#x27;: None,
    &#x27;new_value&#x27;: &#x27;Likes to play cricket on weekends&#x27;,
    &#x27;event&#x27;: &#x27;add&#x27;,
    &#x27;timestamp&#x27;: &#x27;2024-07-14 10:00:54.466687&#x27;,
    &#x27;is_deleted&#x27;: 0
  },
  {
    &#x27;id&#x27;: &#x27;h2&#x27;,
    &#x27;memory_id&#x27;: &#x27;m1&#x27;,
    &#x27;prev_value&#x27;: &#x27;Likes to play cricket on weekends&#x27;,
    &#x27;new_value&#x27;: &#x27;Likes to play tennis on weekends&#x27;,
    &#x27;event&#x27;: &#x27;update&#x27;,
    &#x27;timestamp&#x27;: &#x27;2024-07-14 10:15:17.230943&#x27;,
    &#x27;is_deleted&#x27;: 0
  }
]
```

+   删除记忆

```
m.delete(memory_id=&quot;m1&quot;) # Delete a memory

m.delete_all(user_id=&quot;alice&quot;) # Delete all memories
```

+   重置记忆

```
m.reset() # Reset all memories
```

## 使用案例：客户支持AI智能体

客户支持AI智能体利用Mem0在交互中保留信息，从而实现个性化和高效的支持体验。

### 配置

使用pip安装必要的包

```
pip install openai mem0ai
```

### 完整代码示例

```
[from opena](https://www.zhihu.com/search?q=os.environ&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710143375%7D)i import Open[AI
from me](https://www.zhihu.com/search?q=memory.add&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710143375%7D)m0 import Memor[y

# Set the ](https://www.zhihu.com/search?q=chunk.choices&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710143375%7D)O[penAI AP](https://www.zhihu.com/search?q=customer&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710143375%7D)I key
os.environ[&#x27;OPENAI_API_KEY&#x27;] = &#x27;sk-xxx&#x27;

class CustomerSupportAIAgent:
    def __init__(self):
        &quot;&quot;&quot;
        Initialize the CustomerSupportAIAgent with memory configuration and OpenAI client.
        &quot;&quot;&quot;
        config = {
            &quot;vector_store&quot;: {
                &quot;provider&quot;: &quot;qdrant&quot;,
                &quot;config&quot;: {
                    &quot;host&quot;: &quot;localhost&quot;,
                    &quot;port&quot;: 6333,
                }
            },
        }
        self.memory = Memory.from_config(config)
        self.client = OpenAI()
        self.app_id = &quot;customer-support&quot;

    def handle_query(self, query, user_id=None):
        &quot;&quot;&quot;
        Handle a customer query and store the relevant information in memory.

        :param query: The customer query to handle.
        :param user_id: Optional user ID to associate with the memory.
        &quot;&quot;&quot;
        # Start a streaming chat completion request to the AI
        stream = self.client.chat.completions.create(
            model=&quot;gpt-4&quot;,
            stream=True,
            messages=[
                {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;You are a customer support AI agent.&quot;},
                {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: query}
            ]
        )
        # Store the query in memory
        self.memory.add(query, user_id=user_id, metadata={&quot;app_id&quot;: self.app_id})

        # Print the response from the AI in real-time
        for chunk in stream:
            if chunk.choices[0].delta.content is not None:
                print(chunk.choices[0].delta.content, end=&quot;&quot;)

    def get_memories(self, user_id=None):
        &quot;&quot;&quot;
        Retrieve all memories associated with the given customer ID.

        :param user_id: Optional user ID to filter memories.
        :return: List of memories.
        &quot;&quot;&quot;
        return self.memory.get_all(user_id=user_id)

# Instantiate the CustomerSupportAIAgent
support_agent = CustomerSupportAIAgent()

# Define a customer ID
customer_id = &quot;jane_doe&quot;

# Handle a customer query
support_agent.handle_query(&quot;I need help with my recent order. It hasn&#x27;t arrived yet.&quot;, user_id=customer_id)
```

+   获取记忆

你可以用以下代码在任何时间点获取所有记忆：

```
memories = support_agent.get_memories(user_id=customer_id)
for m in memories:
    print(m[&#x27;text&#x27;])
```

+   要点

+   [初始化：使用](https://www.zhihu.com/search?q=%E5%AE%A2%E6%88%B7%E7%AB%AF%E8%AE%BE%E7%BD%AE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710143375%7D)必要的记忆配置和OpenAI 客户端设置 初始化CustomerSupportAIAgent类。

+   处理查询：handle_query方法向AI发送查询并将相关信息存储在记忆中。

+   检索记忆：get_memories方法获取与客户关联的所有存储记忆。

随着对话的进行，Mem0的记忆会根据交互自动更新，提供不断改进的个性化支持体验。

## 参考文档

https:// github.com/mem0ai/mem0

https:// docs.mem0.ai/overview