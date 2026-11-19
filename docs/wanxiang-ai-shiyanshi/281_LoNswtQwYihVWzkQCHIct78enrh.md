
# 零基础也能开发MCP！手把手带你做实战

> 来源：[https://yunyinghui.feishu.cn/docx/HzzadPjfcoTgM4xEOb9cifjWnhf](https://yunyinghui.feishu.cn/docx/HzzadPjfcoTgM4xEOb9cifjWnhf)

此文基于Windows系统下的快速MCP开发实战，选择一个简单的基础案例，辅助大家入门MCP的开发实战并参加比赛。

## 基础环境搭建

### 安装UV

官方文档：https://docs.astral.sh/uv/getting-started/installation/#__tabbed_1_2

`uv`是目前热度最高的Python环境管理工具，复制对应系统的命令行

![](img/10b09024b4497c1702b7e01d3bb252ee.png)

打开`powershell`

![](img/c6b480a9ac9856a74644f54dfdb66968.png)

输入前文复制的命令行，并检查是否安装成功`uv`

![](img/8c2a4f4466a2771534fef76e5681bdb2.png)

检查命令：`uv python list`，其中有路径的是电脑已经安装的Python版本。灰色的是电脑可安装的Python版本。后续大家需要其他Python版本也可以使用`uv python install 版本号`进行其他的Python版本安装。

![](img/cbd549017439a103dbffe96582304e70.png)

### 新建mcp文件夹

在一个你能找到的地方新建一个文件夹，比如我在D盘新建了一个文件夹，取名`mcp_server`，在这个文件夹中，会教大家创建我们的第一个MCP案例。

![](img/810f77258634b992ea46e4c84f7e1fa3.png)

在`cmd`中进入到文件夹内部，也可以更换你的文件夹的具体路径：

```
cd D:\mcp_server
```

![](img/cb8010563a395da5869086e0c1997738.png)

初始化`uv`环境，为后续开发做准备

```
uv init . -p 3.13
```

初始化成功后的结果：

![](img/a890a274b82b3104a995c610a9f17e52.png)

输入命令行`uv add "mcp[cli]"`,安装MCP的SDK，也就是开发工具包

![](img/9081fd4b4fda60e4bebf67170330ca94.png)

等待安装完成

![](img/26d761149430e6ce39832ca080266ccd.png)

### 安装cursor拓展

1. 打开Cursor拓展
2. 搜索`Python`和`Python Debugger`
3. 进行安装

![](img/a8793bd1ff007feaeaaa693b8566b9cd.png)

在Cursor里打开`mcp_server`文件夹，可以看到文件目录已被初始化

![](img/bf7a04b51b3f59c11e091b44f8c3bf6b.png)

## 代码编写

打开[python—mcp官网](https://github.com/modelcontextprotocol/python-sdk)，复制这一段快速入门。粘贴替换进`main.py`里面

![](img/2eb14981d97dcea2559b4093eb28796a.png)

这里我们暂时注释掉prompt，直播中会给大家解释原因，可以仔细看看注释，我对每一段函数做了一些解释，有助于大家理解，也可以直接进行使用。需要注意的是，最重要部分是`mcp.tool`部分，大家自己定义MCP服务时，实现功能也主要在这一段，函数的文档字符串 `"""Add two numbers"""` 和类型注解 `a: int, b: int` 对 AI 理解工具的功能至关重要。最终AI 模型在需要进行加法运算时，可以识别出这个工具并调用它，传入两个数字作为参数，然后获取返回的计算结果。

![](img/c35d0c624bc32bf7e641b4de413c0923.png)

### MCP协议讲解

输入开始启动项，其中`mcp.run()`中可以填不同的协议，下面给大家粗略讲讲不同协议间的区别：

```
if __name__ == "__main__":
    mcp.run() #这里可以使用不同的协议
```

`mcp.run()` 函数通过不同的“协议”参数，决定了你的 MCP 服务以何种方式暴露给外部以及如何与客户端通信。

MCP服务器：三种传输模式详解

1. 模式一：`stdio` (标准输入/输出)

工作原理: 通过命令行的标准输入 (`stdin`) 和标准输出 (`stdout`) 进行直接的进程间通信，无网络开销。

适用场景: 本地开发、单元测试、脚本集成。

---

2. 模式二：`http` (标准Web API) - 通用与开放的行业标准

工作原理: 将服务器作为标准的Web API运行，通过“请求-响应”模式与客户端交互，通常使用JSON格式。

适用场景: 公共API、Web应用后端、通用微服务。

---

3. 模式三：`SSE` (Server-Sent Events) - 实时流式响应的最佳选择

`SSE` 是一种网页技术，它允许服务器在与客户端建立一个持久的HTTP连接后，单向地、持续地向客户端推送数据流。这与传统的“一次请求，一次完整响应”的HTTP模式完全不同。

工作原理

1. **客户端发起请求**: 客户端（如浏览器）向MCP服务器的一个特定端点发起一个普通的HTTP请求。

2. **服务器保持连接**: 服务器收到请求后，不立即关闭连接，而是返回一个特殊的响应头 `Content-Type: text/event-stream`，并保持该连接处于打开状态。

3. **服务器推送数据**: 当MCP服务内部有新的数据产生时（例如，大语言模型生成了下一个词或一小段文本），服务器会立即将这些数据块通过这个打开的连接发送给客户端。

4. **客户端接收事件**: 客户端会持续监听这个连接，每当收到一个数据块，就会触发一个事件，从而可以实时地处理和展示新数据。

对于AI应用，这通常意味着：

一个生成长文本的请求，用户不必等待几十秒看到全部结果，而是能像在ChatGPT中一样，看着文字一个一个地“流”出来。

**核心优势**

+ **极佳的流式体验**: 这是 `SSE` 在AI领域最大的优势。对于LLM生成、文件处理等耗时任务，它可以实时反馈进度和结果，极大地提升了用户体验和应用的“响应感”。

+ **基于标准HTTP**: 它构建在传统的HTTP之上，实现比`WebSocket`更简单，且能很好地兼容现有的网络基础设施（如反向代理、负载均衡器）。

+ **内置重连机制**: `SSE`标准包含了客户端在连接意外断开后自动重新连接的机制，增加了系统的健壮性。

**局限性**

+ **单向通信**: 数据流只能从服务器到客户端。如果客户端在建立连接后还需要向服务器发送消息，`SSE` 则无能为力，此时需要考虑`WebSocket`。

+ **连接数限制**: 浏览器对单个域名下的并发HTTP连接数有限制（通常是6个），每个`SSE`连接都会计入其中。

**最佳实践**

`SSE` 是实现AI模型**流式输出**的“杀手级”功能。

+ **大语言模型文本生成**: 逐字或逐句返回生成的内容。

+ **任务状态实时更新**: 如“模型加载中...”、“数据处理中...”、“生成完成”。

+ **实时通知系统**: 向用户推送系统通知、新消息提醒等。

**总结与选型对比**

+ **本地开发**：优先使用 `Stdio`，简单高效。

+ **单向数据推送**：短期项目可考虑 `SSE`，长期建议迁移至 `Streamable HTTP`。

+ **高并发分系统**：必须使用 `Streamable HTTP`，尤其需双向交互时。

### 本地MCP部署

我们先尝试`stdio`协议的测试，进行本地MCP的尝试。主函数中修改为：

```
if __name__ == "__main__":
    mcp.run(transport="stdio")
```

完整代码为，并运行这个Python文件`python main.py`：

```
from mcp.server.fastmcp import FastMCP

# Create an MCP server这部分代码导入了 FastMCP 类，并创建了一个名为 mcp 的服务器对象。这个 mcp 对象将作为所有功能的“注册中心”。

mcp = FastMCP("Demo")

# l@mcp.tool(): 这是一个装饰器，它告诉 mcp 服务器，下面的 add 函数是一个可供外部调用的工具。
# 函数的文档字符串 """Add two numbers""" 和类型注解 a: int, b: int 对 AI 理解工具的功能至关重要。
# AI 如何使用: AI 模型在需要进行加法运算时，可以识别出这个工具并调用它，传入两个数字作为参数，然后获取返回的计算结果。

@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers"""
    return a + b

# Add a dynamic greeting resource
# @mcp.resource("greeting://{name}"): 这个装饰器将 get_greeting 函数注册为一个资源。
# 作用: 资源通常用于获取或检索数据。这里的 "greeting://{name}" 就像一个唯一的地址（URI）。{name} 是一个占位符，表示这个部分是动态的。
# 例如，如果 AI 需要一个个性化的问候，它可以通过 "greeting://John" 这样的地址来请求。
# 这个装饰器告诉 mcp 服务器，当 AI 请求 "greeting://{name}" 时，应该调用 get_greeting 函数。
# 这个函数会根据传入的 name 参数返回一个个性化的问候。

@mcp.resource("greeting://{name}")
def get_greeting(name: str) -> str:
    """Get a personalized greeting"""
    return f"Hello, {name}!"

# # Add a prompt 给大模型一个指令，让大模型根据指令生成一个个性化的问候。
# @mcp.prompt()
# def greet_user(name: str, style: str = "friendly") -> str:
#     """Generate a greeting prompt"""
#     styles = {
#         "friendly": "Please write a warm, friendly greeting",
#         "formal": "Please write a formal, professional greeting",
#         "casual": "Please write a casual, relaxed greeting",
#     }

#     return f"{styles.get(style, styles['friendly'])} for someone named {name}."

if __name__ == "__main__":
    mcp.run(transport="stdio")
```

进入Cursor的MCP配置

![](img/277e57cbc4bce420a4eb63a6eb278753.png)

点击New MCP Server

![](img/9892e646ed60b1a0d607c3bfff98adbd.png)

`mcp.json`文件中，输入以下这一段，其中文件夹输入你自己的`main.py`的绝对路径

```
"mcp-plus": {
      "command": "python",
      "args": [
        "D:\\mcp_server\\main.py"
      ]
    }
```

回到MCP界面，这个显示绿点时，即成功调用

![](img/9d600ac10b360b923f46acdc27cde502.png)

在AI agent 进行测试，可以看到她成功调用了 `called add`：

![](img/669c219585d9488b81e9e019f0fe73c0.png)

### SSE协议部署

更改刚刚的最后一行代码，并启动程序：

```
if __name__ == "__main__":
    mcp.run(transport="sse")
```

启动成功后，终端出现这样的页面：

![](img/ee841e105b398de5d3d7488b6aec2366.png)

在MCP配置中进行以下配置：

```
"plus": {
      "url": "http://127.0.0.1:8000/sse"
    }
```

成功后终端会出现以下输出，MCP管理器中对应的MCP服务也会显示绿色。

![](img/83bf5d4e5af940efc7ae4822f6ce10ef.png)

![](img/a7d5388adad1f90678603242d746d432.png)

进行测试：

右边会显示成功调用

![](img/b30e3fe665b7422d4390c5f03f629601.png)

在终端中也会有调用提示：

![](img/1806db0e483f7056843ddeb10c2a334e.png)

## 邪修升级

对于不太会写代码的同学，我们也是可以通过一些邪修解决的~既然在AI的时代，我们也要擅长使用AI工具，站在巨人的肩膀上进行开发~

### 使用claude code

1. 环境搭建：安装`git`

https://git-scm.com/downloads/win

![](img/8a47cc20e25dd49282fb54a79fb5484b.png)

2. 安装`node.js`

https://nodejs.org/en/

![](img/ed5f1014a821cabd5c8e777e6d04709a.png)

3. 安装Claude Code

```
npm install -g @anthropic-ai/claude-code
```

命令行输入`claude`进行验证尝试：

![](img/90559637243e58862d93df7965a47a62.png)

4. 如何免费使用Claude code

目前通过官网使用Claude code 的费用还是比较高昂的（比Cursor费用还高），业内人士的使用方式就是通过中转网址进行注册和购买。比如Any Router注册会赠送100＄的资金，每天签到还有25＄的额度，基本上能满足日常使用的覆盖

注册网址：https://anyrouter.top/register?aff=W6Fv

5. 使用指南：

3️⃣ 开始使用

+ **获取 Auth Token：** `ANTHROPIC_AUTH_TOKEN` ：注册后在 API令牌 页面点击 添加令牌 获得（以 `sk-` 开头）

+ 名称随意，额度建议设为无限额度，其他保持默认设置即可

+ **API地址：** `ANTHROPIC_BASE_URL`：`https://anyrouter.top` 是本站的 API 服务地址，与主站地址相同

在您的项目目录下运行：

```
cd your-project-folder
export ANTHROPIC_AUTH_TOKEN=sk-... 
export ANTHROPIC_BASE_URL=https://anyrouter.top
claude
```

运行后

+ 选择你喜欢的主题 + Enter

+ 确认安全须知 + Enter

+ 使用默认 Terminal 配置 + Enter

+ 信任工作目录 + Enter
