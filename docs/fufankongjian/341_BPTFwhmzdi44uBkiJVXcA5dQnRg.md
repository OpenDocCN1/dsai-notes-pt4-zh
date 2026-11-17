# Qwen3接入MCP技术实战（上）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/M8E3dHPnkopLh4xo1mfczp2WnHe](https://kq4b3vgg5b.feishu.cn/docx/M8E3dHPnkopLh4xo1mfczp2WnHe)

## 课程说明：

+   体验课内容节选自《2025大模型Agent智能体开发实战》(5月班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(5月班)

![](img/b2482c063a177603195cdb9cd3d8dd48.png)

![](img/a6d0bb7c5292b8375209e137b83bd003.png)

此外，公开课全套学习资料，已上传至网盘

（链接: https://pan.baidu.com/s/11nFqw4w2kIvypKqzBW9N🐧 提取码: 6k24 ）

需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

# Qwen3接入MCP智能体开发实战（上）

## 一、MCP技术与Qwen3原生MCP能力介绍

### 1.智能体开发核心技术—MCP

#### 1.1 Function calling技术回顾

如何快速开发一款智能体应用，最关键的技术难点就在于如何让大模型高效稳定的接入一些外部工具。而在MCP技术诞生之前，最主流的方法，是借助Function calling技术来打通大模型和外部工具之间的联系，换而言之，也就是借助Function calling，来让大模型灵活的调用外部工具。

例如一个典型的Function calling示例，我们希望让大模型能够调用一些天气API查询即时天气，此时我们就需要创建一个查询天气的外部函数，负责调用天气API来查询天气，同时将外部函数的说明传递给大模型，使其能够根据用户意图，在必要的时候向外部函数发起调用请求。

![](img/c6a1b01c161b1b8ee8a40f6cde076bca.png)

> Function calling最早由OpenAI与2023年6月13号正式提出，该项技术的名字也由此命名：https://openai.com/index/function-calling-and-other-api-updates/

![](img/cec2c7e86c587972517d57c775dc267e.png)

> 此外，Function calling也被称作tool use、tool call等技术。

毫无疑问，Function calling的诞生意义重大，这项技术目前也成为大模型调用外部工具的基本技术范式，哪怕是MCP盛行的今天，底层仍然是Function calling执行流程。

#### 1.2 Qwen3 Function calling能力介绍

不过为了更好的学习本期公开课，需要重点强调的是关于大模型的Function calling的能力如何而来。我们都知道，对于当前大模型来说，有些模型有Function calling能力，如DeepSeek-V3模型，而有些模型没有，如DeepSeek-R1模型：

![](img/54967f78864e859499db71b8865c33a9.png)

而对于Qwen3全系列模型来说，不仅支持Function calling，还支持工具的并联、串联调用甚至是自动debug：

![](img/096581144db1ac899b79db3dff86835c.png)

这里我们在公开课参考资料总，为大家整理了一份从零手动实现Qwen3 Function calling的代码实战流程：


![](img/4cb59ca5681c4cf11d146fae154ef543.png)

下图扫码即可领取：

![](img/1187b22991f58475aee8ae65becfaeb4.png)

本期公开课以Qwen3模型接入MCP技术为主，模型底层function calling实现方法可参考上述资料进行学习。

#### 1.3 Qwen3 Function calling能力从何而来

那模型是如何具备Function calling能力的呢？答案是通过模型训练。对于Qwen3模型来说，由于在训练阶段（指令微调阶段）就带入了大量的类似如下的工具调用对话数据进行训练，因此能够识别外部工具并发起对外部工具调用的请求。而类似的，R1模型的训练过程没有工具调用数据，因此就不具备Function calling能力。

![](img/05193aa27909ca191f7c0b42f04bfca6.png)

> 更多关于Function calling原理，可以参考公开课《智能体从何而来？深度详解大模型调用工具底层原理》：https://www.bilibili.com/video/BV1w6dBYxELA/，

而Function calling的能力，是大模型顺利开启MCP功能的基础。

#### 1.4 MCP技术本质：Function calling的更高层实现

而近一段时间大火的MCP技术，其实就可以将其理解为Function calling技术的更高层封装和实现。传统的Function calling技术要求围绕不同的外部工具API单独创建一个外部函数，类似一把锁单独配一把钥匙，而一个智能体又往往涉及到多个外部工具设计，因此开发工作量很大。

![](img/8f72a9fbdd2808f8ec13712c10189128.png)

而MCP技术，全称为Model Context Protocol，模型上下文协议，是一种开发者共同遵守的协议，在这个协议框架下，大家围绕某个API开发的外部工具就能够共用，从而大幅减少重复造轮子的时间。

![](img/05f13264040f29a100074a09cb48b3fa.png)

### 2 MCP技术概念介绍

#### 2.1 MCP服务器（server）与客户端（client）概念介绍

不同于Function calling技术，MCP技术是对于大模型和外部工具的另一种划分方式，也就是说在MCP技术体系中，此时MCP会将外部工具运行脚本称作服务器，而接入这些外部工具的大模型运行环境称作客户端。

![](img/8473e0109c7aadcb3e1a8e28f6607eea.png)

一个客户端可以接入多个不同类型的服务器的，但要求是都可以遵循MCP通信协议。简单理解就是MCP服务器的输出内容是一种标准格式的内容，只能被MCP客户端所识别。在客户端和服务器都遵循MCP协议的时候，客户端就能够像Function calling中大模型调用外部工具一样，调用MCP服务器里面的工具。

![](img/b5e2c2a6d57666c65197014f0cbb7343.png)

#### 2.2 MCP服务器集合

暂时抛开底层原理不谈，在MCP技术爆发的这几个月，市面上已经诞生了成百上千的MCP服务器，甚至还出现了大量的MCP服务器集合网站：

+   MCP官方服务器合集：https://github.com/modelcontextprotocol/servers


+   MCP Github热门导航：https://github.com/punkpeye/awesome-mcp-servers

![](img/8a2be74c166343e1226f9515df2e12f9.png)

+   Smithery：https://smithery.ai/

![](img/9bc88ed4dfb213e195376c9d944b2c84.png)

+   MCP导航：https://mcp.so/

![](img/a1ccb61609d1514f1dce49aab5d86eac.png)

在实际进行智能体开发过程中，我们可以参考这些网站上的MCP工具，并有选择的对其进行调用。但需要注意的是，无论这些网站的组织形式多么花样百出，但实际上当我们本地调用MCP工具的时候，都是通过uvx或者npx将对应的库下载到本地，然后再进行运行。

### 3. MCP服务器接入示例与标准流程讲解

#### 3.1 MCP服务器接入示例

而在MCP技术大爆发的今天，接入一个MCP工具也是非常简单，以下是一个将高德地图导航MCP（服务器）接入Cherry Studio（客户端）的示例：

> 我们可以暂时把MCP服务器视作MCP工具。

我们能看到，现在如果想要接入一个MCP工具，我们只需要在支持MCP功能的客户端中写入相关配置即可。例如我们只需要在Cherry Studio的MCP配置文档中写入如下字段：

```
    &quot;amap-maps&quot;: {
      &quot;isActive&quot;: true,
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [
        &quot;-y&quot;,
        &quot;@amap/amap-maps-mcp-server&quot;
      ],
      &quot;env&quot;: {
        &quot;AMAP_MAPS_API_KEY&quot;: &quot;YOUR_API_KRY&quot;
      },
      &quot;name&quot;: &quot;amap-maps&quot;
    }
```

![](img/aa7f211aa9fc5efb2ccf7db228b7e55b.png)

即可让大模型自动关联高德MCP工具（服务器），而一个高德MCP服务器的API有几十种之多：

![](img/5bbdea1df5dc052422bff3074786114d.png)

可以说是覆盖了出行生活的放方面。而当一个大模型接入高德MCP服务器后，就能瞬间化身出行规划智能体。

> 更多客户端接入服务器方法，详见公开课《零门槛接入MCP！Cursor、阿里云百炼、Open-WebUI、Cherry Studio接入10大最热门MCP工具》：https://www.bilibili.com/video/BV1dCo7YdEgK/

#### 3.2 MCP工具标准接入流程

在上述示例中，我们不难发现，一个MCP服务器标准接入流程是通过写入一个配置文件来完成的。而在支持MCP功能的客户端（如Cherry Studio）中写入MCP工具的配置，其本质就是先将指定的MCP工具下载到本地，然后在有需要的时候对其进行调用。例如高德MCP配置文件如下：

```
    &quot;amap-maps&quot;: {
      &quot;isActive&quot;: true,
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [
        &quot;-y&quot;,
        &quot;@amap/amap-maps-mcp-server&quot;
      ],
      &quot;env&quot;: {
        &quot;AMAP_MAPS_API_KEY&quot;: &quot;YOUR_API_KRY&quot;
      },
      &quot;name&quot;: &quot;amap-maps&quot;
    }
```

代表的含义就是我们需要先使用如下命令：

```
npx -y @amap/amap-maps-mcp-server
```

对这个库@amap/amap-maps-mcp-server进行下载，然后在本地运行，当有必要的时候调用这个库里面的函数执行相关功能。

而这个@amap/amap-maps-mcp-server库是一个托管在https://www.npmjs.com/上的库，


可以使用npx命令进行下载。搜索库名即可看到这个库的完整代码，https://www.npmjs.com/package/@amap/amap-maps-mcp-server：

![](img/611565b272df43241f65b21a6973ce77.png)

而这种通过配置文件来进行MCP工具下载的方式，最早由Claude（MCP技术的提出者）提出并被广泛接纳。

> 此外如果是Python代码编写的MCP项目，则托管在PYPI平台上，此时我们可以使用uvx命令进行MCP服务器的下载和安装，例如：https://github.com/modelcontextprotocol/servers/tree/main/src/time

![](img/73346e9c71052e4bd5eba327a1e5f262.png)

> 托管地址：https://pypi.org/project/mcp-server-time/

![](img/a9117197c92406e61498cb6e213434c0.png)

### 4. Qwen3原生MCP能力介绍

#### 4.1 Qwen3原生MCP能力效果

而作为新一代最强开源大模型，Qwen 3不仅拥有非常强悍的推理和对话性能，而且为了更好的应对智能体开发需求，Qwen3模型还是全球首款原生支持MCP功能的大模型，能够更好的理解MCP工具能力、更好的规划多工具调用流程，因此哪怕面对复杂任务，也能做到游刃有余。

不可否认，智能体开发就是当下大模型技术最核心的应用需求，而Qwen3的原生MCP功能，自然就是当下无数技术人最关注的模型功能特性。为了更好的展示模型调用MCP工具的实战效果，千问官方在模型发布公告中特地展示一段Qwen3-32B模型调用多项MCP工具的实操流程，在这个demo中，用户要求绘制Qwen模型GitHub项目主页历史星标增长曲线图。

而实际整个任务执行过程非常惊艳，在没有任何额外的提示的情况下，Qwen3-32B模型调用了包括fetch、time、code-interpreter、filesystem在内的多项MCP工具，并通过这些工具组合调用，完成不同时间点的网站信息检索、信息采集与本地记录、借助Python绘制图像等一系列工作，总耗时约1分钟左右。而要做到这点，我们仅需让Qwen3模型接入MCP工具即可。当然为了验证Qwen3到底有没有这么强，我们团队也使用Qwen-chat进行了复现，结果和官方展示的无异。

确实能够看出Qwen3模型对于外部工具识别和调用能力非常强悍。

#### 4.2 Qwen3原生MCP能力本质与内置提示词模板解读

其实无论什么模型，所谓MCP能力，指的就是外部工具识别和调用能力，也就是Function calling能力。换而言之，Qwen3的MCP能力强悍，指的就是对于外部工具识别和调用的能力很强。这里我们可以通过观察Qwen3模型的内置提示词模板，看出模型是如何识别外部工具的，以下是Qwen3内置提示词模板解析：

> 内置提示词模板可以在任意模型的tokenizer_config.json中查看：

![](img/7ec8329ac5e0ceecdf9065be602b6522.png)

>

![](img/de7ed68cbc4175b146b7de15900c986b.png)

##### Part 1.工具调用（Function Calling）支持部分

```
{%- if tools %}
    {{- &#x27;&lt;|im_start|&gt;system\n&#x27; }}
    {%- if messages[0].role == &#x27;system&#x27; %}
        {{- messages[0].content + &#x27;\n\n&#x27; }}
    {%- endif %}
    {{- &quot;# Tools\n\nYou may call one or more functions to assist with the user query.\n...&quot; }}
    ...
```

**解释：**

+   如果传入了 tools（即 function calling 的函数签名），会优先构造 <|im_start|>system 开头的一段系统提示，告诉模型可以调用工具。

+   这段提示包含：

+   # Tools 开头的说明文字；

+   tools 列表，每个工具（函数）都通过 tojson 转换为 JSON；

+   如何使用 <tool_call> 标签返回工具调用的结果。

##### Part 2.**系统消息处理**

```
{%- if messages[0].role == &#x27;system&#x27; %}
    {{- &#x27;&lt;|im_start|&gt;system\n&#x27; + messages[0].content + &#x27;&lt;|im_end|&gt;\n&#x27; }}
{%- endif %}
```

**解释：**

+   如果首条消息是 system，则会作为系统设定（system prompt）处理，加上 <|im_start|>system\n ... <|im_end|>\n。

##### Part 3.**多轮消息回显处理**

```
{%- for message in messages %}
    {%- if (message.role == &quot;user&quot;) ... %}
        {{- &#x27;&lt;|im_start|&gt;&#x27; + message.role + &#x27;\n&#x27; + message.content + &#x27;&lt;|im_end|&gt;&#x27; + &#x27;\n&#x27; }}
```

**解释：**

+   针对用户（user）、助手（assistant）、工具响应（tool）等不同角色进行处理。

+   使用 <|im_start|>role\n...<|im_end|> 包裹每一轮对话。

##### 4Assistant 角色的特殊处理（含推理内容）

```
{%- if message.role == &quot;assistant&quot; %}
    ...
    &lt;think&gt;\n...reasoning_content...\n&lt;/think&gt;
```

**解释：**

+   若助手消息中包含 <think> 内容，会将其拆分为“推理部分”和“回复正文”。

+   如果存在 tool_calls，还会附加一段 <tool_call> JSON 标签。

##### Part 5.工具响应处理（role = tool）

```
&lt;tool_response&gt;\n...内容...\n&lt;/tool_response&gt;
```

**解释：**

+   模型回复 <tool_call> 后，你会给出 <tool_response>。

+   这部分内容会包在 user role 内部，以 <tool_response> 标签封装，用来模拟用户获得工具调用结果。

##### Part 6.混合推理模式开启方法

```
{%- if add_generation_prompt %}
    {{- &#x27;&lt;|im_start|&gt;assistant\n&#x27; }}
    {%- if enable_thinking is defined and enable_thinking is false %}
        {{- &#x27;&lt;think&gt;\n\n&lt;/think&gt;\n\n&#x27; }}
    {%- endif %}
{%- endif %}
```

**解释：**

+   如果需要生成下一轮回复，会在最后加上 <|im_start|>assistant\n 作为提示。

+   还可以通过设置 enable_thinking=false，强制加上 <think> 占位符。

### 5. Qwen3模型借助桌面端应用快速接入MCP工具

在了解了基本远离后，接下来我们即可使用桌面端应用Cherry Studio，快速上手尝试使用Qwen3接入MCP工具，例如先通过ollama下载模型并开启ollama服务，然后使用Cherry Studio配置天气查询MCP工具进行测试，演示效果如下：

> 更多Qwen3模型本地部署调用流程参考公开课：

### 6.公开课大纲及课件领取

[toc]

+   公开课课件领取

![](img/f38775fa37e5ae3141c0fb629543a157.png)

+   MCP系列公开课参考

在此前我已经开设过一些列公开课来介绍MCP工具，相关基础内容大家可以参考如下公开课：

![](img/c33ec4b096196d81d0bdddf9bed38b1e.png)

+   MCP技术开发入门：https://www.bilibili.com/video/BV1NLXCYTEbj/

+   MCP企业级智能体开发实战：https://www.bilibili.com/video/BV1n1ZuYjEzf/

+   不写一行代码，零门槛接入MCP：https://www.bilibili.com/video/BV1dCo7YdEgK/

+   7分钟开发&发布专属MCP！【涵盖SSE&流式HTTP MCP服务器搭建流程】：https://www.bilibili.com/video/BV1VHL6zsE5F/

## 二、从零到一搭建Qwen3 MCP客户端接入MCP工具

为了更好的为大家展示Qwen3+MCP底层原理，这里我们先尝试手动搭建一个Qwen3客户端，并接入本地或在线的MCP工具。需要注意的是，后续我们无论使用哪种Agent开发框架，搭建Qwen3+MCP的智能体，本质上都是这个手动实现流程的更高层的封装与更便捷的实现形式。

### 1. 基础环境搭建

这里我们采用uv工具进行Python项目管理，首先进入到某个自由选定的目录下，并使用uv进行项目初始化：

```
# cd /root/autodl-tmp/Qwen3

# 创建项目目录
uv init Qwen3-MCP
cd Qwen3-MCP
```

![](img/bcde9acd28082f5c07515c829a88bf92.png)

> 若未安装uv，可使用如下命令进行安装

```
pip install uv
```

然后输入如下命令创建虚拟环境：

```
# 创建虚拟环境
uv venv

# 激活虚拟环境
source .venv/bin/activate
```

此时就构建了一个基础项目结构：

![](img/52bdbffe3c8953aa16385b7409e07cd0.png)

最后需要添加如下依赖：

```
uv add httpx openai mcp
```

![](img/23eb9594fc0e3d9d49cececf95e1acb3.png)

### 2. 编写基于Qwen3的MCP客户端

首先设置配置文件，在当前目录创建.env文件，写入ollama驱动下的Qwen3模型调用地址和模型名称：

![](img/78b0c90ca9cc5a54574e513b01d9f9a1.png)

![](img/70579915f596d3d49d906edb232075d1.png)

```
BASE_URL=http://localhost:11434/v1/
MODEL=qwen3:30b-a3b-fp16
LLM_API_KEY=ollama
```

> 这里具体模型名称可以根据安装的模型决定：

![](img/b508e2ab024ecb74be59455435680a7a.png)

然后在主函数main.py内写入如下内容：

```
import asyncio
import json
import logging
import os
import shutil
from contextlib import AsyncExitStack
from typing import Any, Dict, List, Optional

import httpx
from dotenv import load_dotenv
from openai import OpenAI  # OpenAI Python SDK
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

# Configure logging
logging.basicConfig(
    level=logging.INFO, format=&quot;%(asctime)s - %(levelname)s - %(message)s&quot;
)


# =============================
# 配置加载类（支持环境变量及配置文件）
# =============================
class Configuration:
    &quot;&quot;&quot;管理 MCP 客户端的环境变量和配置文件&quot;&quot;&quot;

    def __init__(self) -&gt; None:
        load_dotenv()
        # 从环境变量中加载 API key, base_url 和 model
        self.api_key = os.getenv(&quot;LLM_API_KEY&quot;)
        self.base_url = os.getenv(&quot;BASE_URL&quot;)
        self.model = os.getenv(&quot;MODEL&quot;)
        if not self.api_key:
            raise ValueError(&quot;❌ 未找到 LLM_API_KEY，请在 .env 文件中配置&quot;)

    @staticmethod
    def load_config(file_path: str) -&gt; Dict[str, Any]:
        &quot;&quot;&quot;
        从 JSON 文件加载服务器配置
        
        Args:
            file_path: JSON 配置文件路径
        
        Returns:
            包含服务器配置的字典
        &quot;&quot;&quot;
        with open(file_path, &quot;r&quot;) as f:
            return json.load(f)


# =============================
# MCP 服务器客户端类
# =============================
class Server:
    &quot;&quot;&quot;管理单个 MCP 服务器连接和工具调用&quot;&quot;&quot;

    def __init__(self, name: str, config: Dict[str, Any]) -&gt; None:
        self.name: str = name
        self.config: Dict[str, Any] = config
        self.session: Optional[ClientSession] = None
        self.exit_stack: AsyncExitStack = AsyncExitStack()
        self._cleanup_lock = asyncio.Lock()

    async def initialize(self) -&gt; None:
        &quot;&quot;&quot;初始化与 MCP 服务器的连接&quot;&quot;&quot;
        # command 字段直接从配置获取
        command = self.config[&quot;command&quot;]
        if command is None:
            raise ValueError(&quot;command 不能为空&quot;)

        server_params = StdioServerParameters(
            command=command,
            args=self.config[&quot;args&quot;],
            env={**os.environ, **self.config[&quot;env&quot;]} if self.config.get(&quot;env&quot;) else None,
        )
        try:
            stdio_transport = await self.exit_stack.enter_async_context(
                stdio_client(server_params)
            )
            read_stream, write_stream = stdio_transport
            session = await self.exit_stack.enter_async_context(
                ClientSession(read_stream, write_stream)
            )
            await session.initialize()
            self.session = session
        except Exception as e:
            logging.error(f&quot;Error initializing server {self.name}: {e}&quot;)
            await self.cleanup()
            raise

    async def list_tools(self) -&gt; List[Any]:
        &quot;&quot;&quot;获取服务器可用的工具列表

        Returns:
            工具列表
        &quot;&quot;&quot;
        if not self.session:
            raise RuntimeError(f&quot;Server {self.name} not initialized&quot;)
        tools_response = await self.session.list_tools()
        tools = []
        for item in tools_response:
            if isinstance(item, tuple) and item[0] == &quot;tools&quot;:
                for tool in item[1]:
                    tools.append(Tool(tool.name, tool.description, tool.inputSchema))
        return tools

    async def execute_tool(
        self, tool_name: str, arguments: Dict[str, Any], retries: int = 2, delay: float = 1.0
    ) -&gt; Any:
        &quot;&quot;&quot;执行指定工具，并支持重试机制

        Args:
            tool_name: 工具名称
            arguments: 工具参数
            retries: 重试次数
            delay: 重试间隔秒数

        Returns:
            工具调用结果
        &quot;&quot;&quot;
        if not self.session:
            raise RuntimeError(f&quot;Server {self.name} not initialized&quot;)
        attempt = 0
        while attempt &lt; retries:
            try:
                logging.info(f&quot;Executing {tool_name} on server {self.name}...&quot;)
                result = await self.session.call_tool(tool_name, arguments)
                return result
            except Exception as e:
                attempt += 1
                logging.warning(
                    f&quot;Error executing tool: {e}. Attempt {attempt} of {retries}.&quot;
                )
                if attempt &lt; retries:
                    logging.info(f&quot;Retrying in {delay} seconds...&quot;)
                    await asyncio.sleep(delay)
                else:
                    logging.error(&quot;Max retries reached. Failing.&quot;)
                    raise

    async def cleanup(self) -&gt; None:
        &quot;&quot;&quot;清理服务器资源&quot;&quot;&quot;
        async with self._cleanup_lock:
            try:
                await self.exit_stack.aclose()
                self.session = None
            except Exception as e:
                logging.error(f&quot;Error during cleanup of server {self.name}: {e}&quot;)


# =============================
# 工具封装类
# =============================
class Tool:
    &quot;&quot;&quot;封装 MCP 返回的工具信息&quot;&quot;&quot;

    def __init__(self, name: str, description: str, input_schema: Dict[str, Any]) -&gt; None:
        self.name: str = name
        self.description: str = description
        self.input_schema: Dict[str, Any] = input_schema

    def format_for_llm(self) -&gt; str:
        &quot;&quot;&quot;生成用于 LLM 提示的工具描述&quot;&quot;&quot;
        args_desc = []
        if &quot;properties&quot; in self.input_schema:
            for param_name, param_info in self.input_schema[&quot;properties&quot;].items():
                arg_desc = f&quot;- {param_name}: {param_info.get(&#x27;description&#x27;, &#x27;No description&#x27;)}&quot;
                if param_name in self.input_schema.get(&quot;required&quot;, []):
                    arg_desc += &quot; (required)&quot;
                args_desc.append(arg_desc)
        return f&quot;&quot;&quot;
Tool: {self.name}
Description: {self.description}
Arguments:
{chr(10).join(args_desc)}
&quot;&quot;&quot;


# =============================
# LLM 客户端封装类（使用 OpenAI SDK）
# =============================
class LLMClient:
    &quot;&quot;&quot;使用 OpenAI SDK 与大模型交互&quot;&quot;&quot;

    def __init__(self, api_key: str, base_url: Optional[str], model: str) -&gt; None:
        self.client = OpenAI(api_key=api_key, base_url=base_url)
        self.model = model

    def get_response(self, messages: List[Dict[str, Any]], tools: Optional[List[Dict[str, Any]]] = None) -&gt; Any:
        &quot;&quot;&quot;
        发送消息给大模型 API，支持传入工具参数（function calling 格式）
        &quot;&quot;&quot;
        payload = {
            &quot;model&quot;: self.model,
            &quot;messages&quot;: messages,
            &quot;tools&quot;: tools,
        }
        try:
            response = self.client.chat.completions.create(**payload)
            return response
        except Exception as e:
            logging.error(f&quot;Error during LLM call: {e}&quot;)
            raise


# =============================
# 多服务器 MCP 客户端类（集成配置文件、工具格式转换与 OpenAI SDK 调用）
# =============================
class MultiServerMCPClient:
    def __init__(self) -&gt; None:
        &quot;&quot;&quot;
        管理多个 MCP 服务器，并使用 OpenAI Function Calling 风格的接口调用大模型
        &quot;&quot;&quot;
        self.exit_stack = AsyncExitStack()
        config = Configuration()
        self.openai_api_key = config.api_key
        self.base_url = config.base_url
        self.model = config.model
        self.client = LLMClient(self.openai_api_key, self.base_url, self.model)
        # (server_name -&gt; Server 对象)
        self.servers: Dict[str, Server] = {}
        # 各个 server 的工具列表
        self.tools_by_server: Dict[str, List[Any]] = {}
        self.all_tools: List[Dict[str, Any]] = []

    async def connect_to_servers(self, servers_config: Dict[str, Any]) -&gt; None:
        &quot;&quot;&quot;
        根据配置文件同时启动多个服务器并获取工具
        servers_config 的格式为：
        {
          &quot;mcpServers&quot;: {
              &quot;sqlite&quot;: { &quot;command&quot;: &quot;uvx&quot;, &quot;args&quot;: [ ... ] },
              &quot;puppeteer&quot;: { &quot;command&quot;: &quot;npx&quot;, &quot;args&quot;: [ ... ] },
              ...
          }
        }
        &quot;&quot;&quot;
        mcp_servers = servers_config.get(&quot;mcpServers&quot;, {})
        for server_name, srv_config in mcp_servers.items():
            server = Server(server_name, srv_config)
            await server.initialize()
            self.servers[server_name] = server
            tools = await server.list_tools()
            self.tools_by_server[server_name] = tools

            for tool in tools:
                # 统一重命名：serverName_toolName
                function_name = f&quot;{server_name}_{tool.name}&quot;
                self.all_tools.append({
                    &quot;type&quot;: &quot;function&quot;,
                    &quot;function&quot;: {
                        &quot;name&quot;: function_name,
                        &quot;description&quot;: tool.description,
                        &quot;input_schema&quot;: tool.input_schema
                    }
                })

        # 转换为 OpenAI Function Calling 所需格式
        self.all_tools = await self.transform_json(self.all_tools)

        logging.info(&quot;\n✅ 已连接到下列服务器:&quot;)
        for name in self.servers:
            srv_cfg = mcp_servers[name]
            logging.info(f&quot;  - {name}: command={srv_cfg[&#x27;command&#x27;]}, args={srv_cfg[&#x27;args&#x27;]}&quot;)
        logging.info(&quot;\n汇总的工具:&quot;)
        for t in self.all_tools:
            logging.info(f&quot;  - {t[&#x27;function&#x27;][&#x27;name&#x27;]}&quot;)

    async def transform_json(self, json_data: List[Dict[str, Any]]) -&gt; List[Dict[str, Any]]:
        &quot;&quot;&quot;
        将工具的 input_schema 转换为 OpenAI 所需的 parameters 格式，并删除多余字段
        &quot;&quot;&quot;
        result = []
        for item in json_data:
            if not isinstance(item, dict) or &quot;type&quot; not in item or &quot;function&quot; not in item:
                continue
            old_func = item[&quot;function&quot;]
            if not isinstance(old_func, dict) or &quot;name&quot; not in old_func or &quot;description&quot; not in old_func:
                continue
            new_func = {
                &quot;name&quot;: old_func[&quot;name&quot;],
                &quot;description&quot;: old_func[&quot;description&quot;],
                &quot;parameters&quot;: {}
            }
            if &quot;input_schema&quot; in old_func and isinstance(old_func[&quot;input_schema&quot;], dict):
                old_schema = old_func[&quot;input_schema&quot;]
                new_func[&quot;parameters&quot;][&quot;type&quot;] = old_schema.get(&quot;type&quot;, &quot;object&quot;)
                new_func[&quot;parameters&quot;][&quot;properties&quot;] = old_schema.get(&quot;properties&quot;, {})
                new_func[&quot;parameters&quot;][&quot;required&quot;] = old_schema.get(&quot;required&quot;, [])
            new_item = {
                &quot;type&quot;: item[&quot;type&quot;],
                &quot;function&quot;: new_func
            }
            result.append(new_item)
        return result

    async def chat_base(self, messages: List[Dict[str, Any]]) -&gt; Any:
        &quot;&quot;&quot;
        使用 OpenAI 接口进行对话，并支持多次工具调用（Function Calling）。
        如果返回 finish_reason 为 &quot;tool_calls&quot;，则进行工具调用后再发起请求。
        &quot;&quot;&quot;
        response = self.client.get_response(messages, tools=self.all_tools)
        # 如果模型返回工具调用
        if response.choices[0].finish_reason == &quot;tool_calls&quot;:
            while True:
                messages = await self.create_function_response_messages(messages, response)
                response = self.client.get_response(messages, tools=self.all_tools)
                if response.choices[0].finish_reason != &quot;tool_calls&quot;:
                    break
        return response

    async def create_function_response_messages(self, messages: List[Dict[str, Any]], response: Any) -&gt; List[Dict[str, Any]]:
        &quot;&quot;&quot;
        将模型返回的工具调用解析执行，并将结果追加到消息队列中
        &quot;&quot;&quot;
        function_call_messages = response.choices[0].message.tool_calls
        messages.append(response.choices[0].message.model_dump())
        for function_call_message in function_call_messages:
            tool_name = function_call_message.function.name
            tool_args = json.loads(function_call_message.function.arguments)
            # 调用 MCP 工具
            function_response = await self._call_mcp_tool(tool_name, tool_args)
            messages.append({
                &quot;role&quot;: &quot;tool&quot;,
                &quot;content&quot;: function_response,
                &quot;tool_call_id&quot;: function_call_message.id,
            })
        return messages

    async def process_query(self, user_query: str) -&gt; str:
        &quot;&quot;&quot;
        OpenAI Function Calling 流程：
         1. 发送用户消息 + 工具信息
         2. 若模型返回 finish_reason 为 &quot;tool_calls&quot;，则解析并调用 MCP 工具
         3. 将工具调用结果返回给模型，获得最终回答
        &quot;&quot;&quot;
        messages = [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: user_query}]
        response = self.client.get_response(messages, tools=self.all_tools)
        content = response.choices[0]
        logging.info(content)
        if content.finish_reason == &quot;tool_calls&quot;:
            tool_call = content.message.tool_calls[0]
            tool_name = tool_call.function.name
            tool_args = json.loads(tool_call.function.arguments)
            logging.info(f&quot;\n[ 调用工具: {tool_name}, 参数: {tool_args} ]\n&quot;)
            result = await self._call_mcp_tool(tool_name, tool_args)
            messages.append(content.message.model_dump())
            messages.append({
                &quot;role&quot;: &quot;tool&quot;,
                &quot;content&quot;: result,
                &quot;tool_call_id&quot;: tool_call.id,
            })
            response = self.client.get_response(messages, tools=self.all_tools)
            return response.choices[0].message.content
        return content.message.content

    async def _call_mcp_tool(self, tool_full_name: str, tool_args: Dict[str, Any]) -&gt; str:
        &quot;&quot;&quot;
        根据 &quot;serverName_toolName&quot; 格式调用相应 MCP 工具
        &quot;&quot;&quot;
        parts = tool_full_name.split(&quot;_&quot;, 1)
        if len(parts) != 2:
            return f&quot;无效的工具名称: {tool_full_name}&quot;
        server_name, tool_name = parts
        server = self.servers.get(server_name)
        if not server:
            return f&quot;找不到服务器: {server_name}&quot;
        resp = await server.execute_tool(tool_name, tool_args)
        return resp.content if resp.content else &quot;工具执行无输出&quot;

    async def chat_loop(self) -&gt; None:
        &quot;&quot;&quot;多服务器 MCP + OpenAI Function Calling 客户端主循环&quot;&quot;&quot;
        logging.info(&quot;\n🤖 多服务器 MCP + Function Calling 客户端已启动！输入 &#x27;quit&#x27; 退出。&quot;)
        messages: List[Dict[str, Any]] = []
        while True:
            query = input(&quot;\n你: &quot;).strip()
            if query.lower() == &quot;quit&quot;:
                break
            try:
                messages.append({&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: query})
                messages = messages[-20:]  # 保持最新 20 条上下文
                response = await self.chat_base(messages)
                messages.append(response.choices[0].message.model_dump())
                result = response.choices[0].message.content
                # logging.info(f&quot;\nAI: {result}&quot;)
                print(f&quot;\nAI: {result}&quot;)
            except Exception as e:
                print(f&quot;\n⚠️  调用过程出错: {e}&quot;)

    async def cleanup(self) -&gt; None:
        &quot;&quot;&quot;关闭所有资源&quot;&quot;&quot;
        await self.exit_stack.aclose()


# =============================
# 主函数
# =============================
async def main() -&gt; None:
    # 从配置文件加载服务器配置
    config = Configuration()
    servers_config = config.load_config(&quot;servers_config.json&quot;)
    client = MultiServerMCPClient()
    try:
        await client.connect_to_servers(servers_config)
        await client.chat_loop()
    finally:
        try:
            await asyncio.sleep(0.1)
            await client.cleanup()
        except RuntimeError as e:
            # 如果是因为退出 cancel scope 导致的异常，可以选择忽略
            if &quot;Attempted to exit cancel scope&quot; in str(e):
                logging.info(&quot;退出时检测到 cancel scope 异常，已忽略。&quot;)
            else:
                raise

if __name__ == &quot;__main__&quot;:
    asyncio.run(main())
```

![](img/a32bf8e798f661007502e35454e2f0d9.png)

以上客户端为我们团队独家研发的MCP客户端脚本，具备以下功能：

1.  根据.env配置，自由切换底层模型，如可接入ollama、vLLM驱动的模型，也可以接入OpenAI、DeepSeek等在线模型；

1.  根据servers_config.json读取多个MCP服务，可以读取本地MCP工具，也可以下载在线MCP工具并进行使用；

1.  能够进行基于命令行的实现多轮对话；

1.  能够同时连接并调用多个MCP server上的多个工具，并能实现多工具的并联和串联调用；

### 3. 配置MCP服务器

接下来继续创建一个servers_config.json脚本，并写入我们希望调用的MCP工具，例如我们想要调用Filesystem MCP，该MCP是一个最基础同时也是最常用的MCP服务器，同时也是官方推荐的服务器，服务器项目地址：https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem

![](img/1728e16fcd489afe57fc154298e6e516.png)

借助Filesystem，我们可以高效便捷操作本地文件夹。同时Filesystem也是一个js项目，源码托管在npm平台上：（https://www.npmjs.com/package/@modelcontextprotocol/server-filesystem）

![](img/2947fc847f2b014f402819ed67ea9389.png)

我们直接创建servers_config.josn并写入如下配置即可调用：

```
{
  &quot;mcpServers&quot;: {
    &quot;filesystem&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [
        &quot;-y&quot;,
        &quot;@modelcontextprotocol/server-filesystem&quot;,
        &quot;/path/to/other/allowed/dir&quot;
      ]
    }
  }
}
```

![](img/7ca082b3dd4ab57079d0f3e378b9df54.png)

![](img/79734433952fad7c15633e67c54c9339.png)

> 其中/path/to/other/allowed/dir需要替换成自己的文件夹地址。

### 4.开启Qwen3+MCP调用流程

然后就可以在当前项目的主目录下输入uv run进行运行：

```
uv run main.py
```

![](img/85116b5ab827344c7529c9e972487517.png)

能够看到，此时已经关联了filesystem多项外部工具。接下来进行对话：

![](img/32b89be13234ba7a2928101eec36b3f2.png)

由于模型开启了思考模式，所以能看到的具体过程。

接下来继续尝试调用外部工具：

![](img/7518a688ac2a1479c9c6ee2582f1ee20.png)

能够看到，Qwen3 MCP Client能够顺利调用外部工具。

此外，我们还可以让Qwen3 MCP Client接入自定义的函数，例如我们在当前项目中创建两个MCP server脚本：

+   weather_server.py：查询天气MCP服务器

![](img/9afa5688eefebb9f513a7a586a5bd45f.png)

并写入如下内容

```
import os
import json
import httpx
from typing import Any
from dotenv import load_dotenv
from mcp.server.fastmcp import FastMCP

# 初始化 MCP 服务器
mcp = FastMCP(&quot;WeatherServer&quot;)

# OpenWeather API 配置
OPENWEATHER_API_BASE = &quot;https://api.openweathermap.org/data/2.5/weather&quot;
API_KEY = &quot;YOUR_API_KEY&quot;     # 填写你的OpenWeather-API-KEY
USER_AGENT = &quot;weather-app/1.0&quot;

async def fetch_weather(city: str) -&gt; dict[str, Any] | None:
    &quot;&quot;&quot;
    从 OpenWeather API 获取天气信息。
    :param city: 城市名称（需使用英文，如 Beijing）
    :return: 天气数据字典；若出错返回包含 error 信息的字典
    &quot;&quot;&quot;
    params = {
        &quot;q&quot;: city,
        &quot;appid&quot;: API_KEY,
        &quot;units&quot;: &quot;metric&quot;,
        &quot;lang&quot;: &quot;zh_cn&quot;
    }
    headers = {&quot;User-Agent&quot;: USER_AGENT}

    async with httpx.AsyncClient() as client:
        try:
            response = await client.get(OPENWEATHER_API_BASE, params=params, headers=headers, timeout=30.0)
            response.raise_for_status()
            return response.json()  # 返回字典类型
        except httpx.HTTPStatusError as e:
            return {&quot;error&quot;: f&quot;HTTP 错误: {e.response.status_code}&quot;}
        except Exception as e:
            return {&quot;error&quot;: f&quot;请求失败: {str(e)}&quot;}

def format_weather(data: dict[str, Any] | str) -&gt; str:
    &quot;&quot;&quot;
    将天气数据格式化为易读文本。
    :param data: 天气数据（可以是字典或 JSON 字符串）
    :return: 格式化后的天气信息字符串
    &quot;&quot;&quot;
    # 如果传入的是字符串，则先转换为字典
    if isinstance(data, str):
        try:
            data = json.loads(data)
        except Exception as e:
            return f&quot;无法解析天气数据: {e}&quot;

    # 如果数据中包含错误信息，直接返回错误提示
    if &quot;error&quot; in data:
        return f&quot;⚠️ {data[&#x27;error&#x27;]}&quot;

    # 提取数据时做容错处理
    city = data.get(&quot;name&quot;, &quot;未知&quot;)
    country = data.get(&quot;sys&quot;, {}).get(&quot;country&quot;, &quot;未知&quot;)
    temp = data.get(&quot;main&quot;, {}).get(&quot;temp&quot;, &quot;N/A&quot;)
    humidity = data.get(&quot;main&quot;, {}).get(&quot;humidity&quot;, &quot;N/A&quot;)
    wind_speed = data.get(&quot;wind&quot;, {}).get(&quot;speed&quot;, &quot;N/A&quot;)
    # weather 可能为空列表，因此用 [0] 前先提供默认字典
    weather_list = data.get(&quot;weather&quot;, [{}])
    description = weather_list[0].get(&quot;description&quot;, &quot;未知&quot;)

    return (
        f&quot;🌍 {city}, {country}\n&quot;
        f&quot;🌡 温度: {temp}°C\n&quot;
        f&quot;💧 湿度: {humidity}%\n&quot;
        f&quot;🌬 风速: {wind_speed} m/s\n&quot;
        f&quot;🌤 天气: {description}\n&quot;
    )

@mcp.tool()
async def query_weather(city: str) -&gt; str:
    &quot;&quot;&quot;
    输入指定城市的英文名称，返回今日天气查询结果。
    :param city: 城市名称（需使用英文）
    :return: 格式化后的天气信息
    &quot;&quot;&quot;
    data = await fetch_weather(city)
    return format_weather(data)

if __name__ == &quot;__main__&quot;:
    # 以标准 I/O 方式运行 MCP 服务器
    mcp.run(transport=&#x27;stdio&#x27;)
```

![](img/bdaaa95eec98e0a23f156c74e170b048.png)

+   **write_server.py**：写入本地文档MCP服务器

![](img/d4d2e8de1215b58032696cfcfc73a85e.png)

并写入如下内容

```
import json
import httpx
from typing import Any
from mcp.server.fastmcp import FastMCP

# 初始化 MCP 服务器
mcp = FastMCP(&quot;WriteServer&quot;)
USER_AGENT = &quot;write-app/1.0&quot;

@mcp.tool()
async def write_file(content: str) -&gt; str:
    &quot;&quot;&quot;
    将指定内容写入本地文件。
    :param content: 必要参数，字符串类型，用于表示需要写入文档的具体内容。
    :return：是否成功写入
    &quot;&quot;&quot;
    return &quot;已成功写入本地文件。&quot;

if __name__ == &quot;__main__&quot;:
    # 以标准 I/O 方式运行 MCP 服务器
    mcp.run(transport=&#x27;stdio&#x27;)
```

![](img/f41cce3a82d17b22def0ea217284c0db.png)

然后需改servers_config.json配置文件，写入如下内容：

```
{
  &quot;mcpServers&quot;: {
    &quot;filesystem&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [
        &quot;-y&quot;,
        &quot;@modelcontextprotocol/server-filesystem&quot;,
        &quot;/root/autodl-tmp/Qwen3/Qwen3-MCP&quot;
      ]
    },
    &quot;weather&quot;: {
      &quot;command&quot;: &quot;python&quot;,
      &quot;args&quot;: [&quot;weather_server.py&quot;]
    },
    &quot;write&quot;: {
      &quot;command&quot;: &quot;python&quot;,
      &quot;args&quot;: [&quot;write_server.py&quot;]
    }
  }
}
```

![](img/1fe6ea06af4d21813e182f34a30f2ff4.png)

然后再次开启对话，就能看到加载了更多工具进来：

![](img/118365d081bff788d5630ff40c49fba7.png)

并可进行多MCP服务器的多工具并行调用：

![](img/545c1d6622d954b18ef134378d115e5b.png)

![](img/a30dc7e9c05300328523d94e0ed051bc.png)

和串联调用：

![](img/1baf90c2f30f3c2f9fc03ff780e0ca37.png)

![](img/e09630e08496196c73a8c17b6068c69c.png)

至此我们就完成了Qwen-3接入在线MCP工具的流程。

> 在输入每段内容前设置/no_think即可取消推理模式：

![](img/0e734147e119cf6742e7b61d0282e0c7.png)

> 而如果要在ollama中彻底设置非思考模式，则需要手动改写ollama GGUF模型权重提示词模板，这项技术此后有机会再介绍。

+   完整项目脚本已上传至课件网盘

![](img/b18f2ecabb94ebfd7cbabfea44916e9f.png)

下图扫码即可领取：

![](img/eee2d99d33f019f16c17bbad5e2dc03d.png)

---

+   体验课内容节选自[《2025大模型Agent智能体开发实战》(5月班)]完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(5月班)

![](img/29f0f21e3c7a33ab0349f690068c9f0d.png)

《2025大模型Agent智能体开发实战》(5月班)

![](img/5ec2053787f9b93e59a7d1f1b52540aa.png)

同时，5月班重磅新增DeepSeek+Agents SDK+谷歌ADK+MCP技术应用与智能体开发相关实战内容，并计划新增Qwen-3模型实战教学：

![](img/28a31012cf13937c30cb556899881dd2.png)

### 部分课程成果演示

+   Dify+DeepSeek搭建智能客服

+   可视化数据分析Multi-Agent

+   Ollama 自动化并发请求测试与动态资源监控

+   Neo4j并行多线程导入百万级文本方法与实践

+   MateGen Pro 项目功能演示

+   智能客服项目展示

此外，若是对大模型底层原理感兴趣，也欢迎报名由我和菜菜老师共同主讲的《2025大模型原理与实战课程》(5月班)

![](img/d85fca6cb1b375ffcb9a9ec67342e855.png)

**两门大模型课程5月班目前上新特惠中，立减2000起，合购还有更多**优惠哦~详细信息扫码添加助教，回复“大模型”，即可领取课程大纲&查看课程详情👇

![](img/b4e9b8ea0b17628982fd86f8a8e57050.png)