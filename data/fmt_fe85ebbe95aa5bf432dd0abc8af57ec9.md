
## 2. MCP客户端基础

```python
async def chat_loop(self):
    """运行交互式聊天循环"""
    print("\nMCP 客户端已启动！输入 'quit' 退出")

    while True:  # 无限循环，直到用户输入 'quit'
        try:
            query = input("\nQuery: ").strip()  # 让用户输入问题
            if query.lower() == 'quit':  # 如果用户输入 quit，退出循环
                break
            print(f"\n🤖 [Mock Response] 你说的是：{query}")  # 返回模拟回复
        except Exception as e:  # 发生错误时捕获异常
            print(f"\n⚠️ 发生错误: {str(e)}")
```

📌 **解释：**

1. `while True`：无限循环，让用户可以不断输入查询。

1. `query = input("\nQuery: ").strip()`：获取用户输入的查询。

1. `if query.lower() == 'quit'`：如果用户输入 `quit`，退出循环。

1. `print(f"\n🤖 [Mock Response] 你说的是：{query}")`：模拟 MCP 服务器的响应，暂时只是回显用户输入的内容。

🔹 **示例运行**

```
MCP 客户端已启动！输入 'quit' 退出

Query: 你好
🤖 [Mock Response] 你说的是：你好

Query: 这是什么？
🤖 [Mock Response] 你说的是：这是什么？

Query: quit  # 退出程序
```

最后白那些释放资源代码

```python
async def cleanup(self):
    """清理资源"""
    await self.exit_stack.aclose()  # 关闭资源管理器
```

📌 **解释：**

+   `aclose()` 确保程序退出时正确关闭 MCP 连接（尽管目前没有真正的连接）。

并定义 `main()` 主函数

```python
async def main():
    client = MCPClient()  # 创建 MCP 客户端
    try:
        await client.connect_to_mock_server()  # 连接（模拟）服务器
        await client.chat_loop()  # 进入聊天循环
    finally:
        await client.cleanup()  # 确保退出时清理资源
```

📌 **解释：**

+   `client = MCPClient()`：创建一个 `MCPClient` 客户端实例。

+   `await client.connect_to_mock_server()`：初始化 MCP 客户端（暂不连接服务器）。

+   `await client.chat_loop()`：启动交互式聊天。

+   `finally:` 确保不管程序是否异常退出，都会正确释放资源。

以及运行代码

```python
if __name__ == "__main__":
    asyncio.run(main())
```

📌 **解释：**

+   `if __name__ == "__main__":`：确保代码只能在 Python 直接运行时执行（而不是作为库导入时）。

+   `asyncio.run(main())`：启动 `main()`，运行 MCP 客户端。

MCP 中一个基础的客户端代码结构总结如下：

| 代码部分 | 作用 |

| --- | --- |

| `MCPClient.__init__()` | 初始化 MCP 客户端 |

| `connect_to_mock_server()` | 模拟 MCP 服务器连接 |

| `chat_loop()` | 提供交互式聊天界面 |

| `cleanup()` | 释放资源 |

| `main()` | 启动客户端 |

| `asyncio.run(main())` | 运行程序 |

### 2.5 运行 MCP 客户端

然后尝试运行这个极简的MCP客户端：

```
uv run client.py
```

运行效果如下所示：

![](img/ffbd6fecc350a84a1770485591747c26.png)

![](img/4a0ac6c930bc9921a1cf2a91dbcbc369.png)

### 2.6 Jupyter中代码运行效果

此外，MCP客户端也是可以在Jupyter中运行的，此时运行代码如下：

```
!pip install mcp
```

```
import asyncio
import nest_asyncio
from mcp import ClientSession
from contextlib import AsyncExitStack

# 解决 Jupyter Notebook 的 asyncio 事件循环冲突
nest_asyncio.apply()
```

```
class MCPClient:
    def __init__(self):
        """初始化 MCP 客户端"""
        self.session = None
        self.exit_stack = AsyncExitStack()

    async def connect_to_mock_server(self):
        """模拟 MCP 服务器的连接（暂不连接真实服务器）"""
        print("✅ MCP 客户端已初始化，但未连接到服务器")

    async def chat_loop(self):
        """运行交互式聊天循环"""
        print("\n📢 MCP 客户端已启动！输入 'quit' 退出\n")

        while True:
            try:
                # 直接使用 input() 获取用户输入
                query = input("📝 请输入您的问题: ").strip()
                if query.lower() == 'quit':
                    print("\n👋 退出聊天...")
                    break

                # 模拟服务器返回响应
                response = f"🤖 [Mock Response] 你说的是：{query}"
                print(response)

            except Exception as e:
                print(f"\n⚠️ 发生错误: {str(e)}")

    async def cleanup(self):
        """清理资源"""
        await self.exit_stack.aclose()

async def main():
    client = MCPClient()
    try:
        await client.connect_to_mock_server()
        await client.chat_loop()
    finally:
        await client.cleanup()
```

然后即可输入如下代码开启对话：

```
# 在 Jupyter Notebook 中运行
await main()
```

![](img/9693dcfd77ecf475f4a99bbac411cfd4.png)

## 3. MCP客户端接入OpenAI、DeepSeek在线模型流程

接下来尝试在客户端中接入OpenAI和DeepSeek等在线模型进行对话。需要注意的是，由于OpenAI和DeepSeek调用方法几乎完全一样，因此这套服务器client代码可以同时适用于GPT模型和DeepSeek模型。

### 3.1 新增依赖

为了支持调用OpenAI模型，以及在环境变量中读取API-KEY等信息，需要先安装如下依赖：

```
uv add mcp openai python-dotenv
```

![](img/c65e32a864cc41fc611070fb8d1e3100.png)

### 3.2 创建`.env`文件

接下来创建`.env`文件，并写入OpenAI的API-Key，以及反向代理地址。借助反向代理，国内可以无门槛直连OpenAI官方服务器，并调用官方API。

![](img/4f297e941a10a94301318d51c723a5a3.png)

写入如下内容

```
BASE_URL="反向代理地址"
MODEL=gpt-4o
OPENAI_API_KEY="OpenAI-API-Key"
```

![](img/8d5b9beeaee8c93690e4570f3191c042.png)

OpenAI注册指南与国内反向代理领取地址：

![](img/ab6dce5db88d03d7d741ce1bde0139fc.png)

![](img/6f41b9412ae8acfc00c4c2322edc5607.png)

而如果是使用DeepSeek模型，则需要在`.env`中写入如下内容：

```
BASE_URL=https://api.deepseek.com
MODEL=deepseek-chat      
OPENAI_API_KEY="DeepSeek API-Key"
```

> 如果要调用`deepseek-chat`为DeepSeek V3模型，若要调用DeepSeek R1，则需要将其修改为`deepseek-reasoner`；

> DeepSeek快速入门：https://www.bilibili.com/video/BV1JuwVewELc/

### 3.3 修改`client.py`代码

接下来修改客户端代码：

```python
import asyncio
import os
from openai import OpenAI
from dotenv import load_dotenv
from contextlib import AsyncExitStack

# 加载 .env 文件，确保 API Key 受到保护
load_dotenv()

class MCPClient:
    def __init__(self):
        
        """初始化 MCP 客户端"""
        self.exit_stack = AsyncExitStack()
        self.openai_api_key = os.getenv("OPENAI_API_KEY")  # 读取 OpenAI API Key
        self.base_url = os.getenv("BASE_URL")  # 读取 BASE YRL
        self.model = os.getenv("MODEL")  # 读取 model
        
        if not self.openai_api_key:
            raise ValueError("❌ 未找到 OpenAI API Key，请在 .env 文件中设置 OPENAI_API_KEY")
            
        self.client = OpenAI(api_key=self.openai_api_key, base_url=self.base_url) 
        

    async def process_query(self, query: str) -> str:
        """调用 OpenAI API 处理用户查询"""
        messages = [{"role": "system", "content": "你是一个智能助手，帮助用户回答问题。"},
                    {"role": "user", "content": query}]
        
        try:
            # 调用 OpenAI API
            response = await asyncio.get_event_loop().run_in_executor(
                None,
                lambda: self.client.chat.completions.create(
                    model=self.model,
                    messages=messages
                )
            )
            return response.choices[0].message.content
        except Exception as e:
            return f"⚠️ 调用 OpenAI API 时出错: {str(e)}"

    async def chat_loop(self):
        """运行交互式聊天循环"""
        print("\n🤖 MCP 客户端已启动！输入 'quit' 退出")

        while True:
            try:
                query = input("\n你: ").strip()
                if query.lower() == 'quit':
                    break
                
                response = await self.process_query(query)  # 发送用户输入到 OpenAI API
                print(f"\n🤖 OpenAI: {response}")

            except Exception as e:
                print(f"\n⚠️ 发生错误: {str(e)}")

    async def cleanup(self):
        """清理资源"""
        await self.exit_stack.aclose()

async def main():
    client = MCPClient()
    try:
        await client.chat_loop()
    finally:
        await client.cleanup()

if __name__ == "__main__":
    asyncio.run(main())
```

### 3.4 运行`client.py`

然后即可输入如下命令开始运行对话客户端：

```
uv run client.py
```

运行效果如下所示：

![](img/0fc765d44dd9cc7aa784dbd15da40bf5.png)

### 3.5 `client.py`代码解释

#### 加载 OpenAI API Key

```python
from dotenv import load_dotenv
import os

# 加载 .env 文件，确保 API Key 受到保护
load_dotenv()

self.openai_api_key = os.getenv("OPENAI_API_KEY")  # 读取 API Key
if not self.openai_api_key:
    raise ValueError("❌ 未找到 OpenAI API Key，请在 .env 文件中设置 OPENAI_API_KEY")
```

📌 **解释：**

+   `load_dotenv()`：自动加载 `.env` 文件，避免在代码中直接暴露 API Key。

+   `os.getenv("OPENAI_API_KEY")`：从环境变量中读取 `OPENAI_API_KEY`。
