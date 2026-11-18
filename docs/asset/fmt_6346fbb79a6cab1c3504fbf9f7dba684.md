
# 用MCP实现飞书推送Agent

> 来源：[https://datawhaler.feishu.cn/docx/KZUcdiUvuoEaolx4Heec8XcynKc](https://datawhaler.feishu.cn/docx/KZUcdiUvuoEaolx4Heec8XcynKc)

## 有用的资源

+   MCP官方文档（主要学习）：[https://modelcontextprot](https://modelcontextprotocol.io/introduction)ocol.io/introduction

+   精选的mcp服务端：[h](https://github.com/punkpeye/awesome-mcp-servers/blob/main/README-zh.md)ttps://github.com/punkpeye/awesome-mcp-servers/blob/main/README-zh.md

+   官方MCP：[https://github.com/modelcontex](https://github.com/modelcontextprotocol/python-sdk)tprotocol/python-sdk

## 代码实践

### 快速开始：创建一个MCP工具

创建MCP工具和平常我们创建Agent的tool类似，主要是让大模型知道：

+   这个工具有哪些参数，参数类型和要求是什么。这点需要在函数注释中详细说明。

+   这个函数的用途，返回类型是什么

使用MCP编写工具只需要在我们写好的函数上加一个装饰器`@mcp.tool()`即可

```
from mcp.server.fastmcp import FastMCP 
# 首先导入必备库，然后创建一个MCP服务器 
mcp = FastMCP(&quot;Demo&quot;)  

@mcp.tool() 
def add(a: int, b: int) -&gt; int:     
    &quot;&quot;&quot;Add two numbers&quot;&quot;&quot;     
    return a + b   
    
if __name__ == &#x27;__main__&#x27;:     
    mcp.run(transport=&quot;stdio&quot;)
```

### 使用`Inspector`调试代码

然后我们用`Inspector`来调试

1.  使用`mcp dev` 文件名 来启动服务

1.  一般来说链接是：`http://127.0.0.1:6274` ，由于一些wsl网络问题，可以用`google-chrome` <http://127.0.0.1:6274> & 命令来打开。如果连不上先在命令行中`unset` `ALL_PROXY`

随后就可以连上MCP了，点击可以测试工具

![](img/74e8f66002006b723dad0bb118f59bc5.png)

可以发现，通过`mcp dev main.py`后，我们成功使用了这个MCP工具，输入1+2正确返回了3，后面换成大模型也能做这件事。

### 使用`cline`运行mcp

`cline`可以让大模型利用写好的MCP工具，这里首先配置MCP服务的json文件，让`cline`知道有这么一个工具：

具体而言和使用`uv`运行这个文件是一样的，可以在命令行运行`uv --directory 存放你mcp服务文件的路径 run 你的mcp服务文件`，会发现也能启动服务

```
  {
    &quot;mcpServers&quot;: {
      &quot;demo&quot;: {
        &quot;disabled&quot;: false,
        &quot;timeout&quot;: 60,
        &quot;command&quot;: &quot;uv&quot;,
        &quot;args&quot;: [
          &quot;--directory&quot;,
          &quot;/home/usamimeri/MCP_Learning&quot;,
          &quot;run&quot;,
          &quot;main.py&quot;
        ],
        &quot;transportType&quot;: &quot;stdio&quot;
      }
    }
  }
```

询问大模型，发现成功输出了结果！到此就跑通了开发MCP的一个流程

+   创建`mcp`服务器（可以是一个python文件，里面有要给ai用的函数）

+   配置运行这个服务器的json文件

+   在支持`mcp`的客户端中使用这个`mcp`服务

![](img/fd2acdf6fcfb1f07d7e822d047e27311.png)

## 一个复杂点的案例

现在实现一个复杂点的需求：Github Trending推荐智能体

### 初步需求

1.  爬取Github Trending信息，返回结构化字典便于进一步获取信息

1.  针对每个返回的repo，实现过滤功能，即只保留用户预设的感兴趣的库

1.  对于过滤后的repo，进入其内部读取readme作为额外信息，同时存入结构化字典，并保存到本地

1.  总结各个repo的信息，输出一个Github Trending推荐总结，包含每个仓库的介绍，用途，特点等

1.  将内容作为飞书卡片，推送到自己的飞书群

### 准备工作

#### 配置环境：使用`uv`

```
mkdir ossagentforfeishu
cd ossagentforfeishu
uv init
uv venv
source .venv/bin/activate 
uv pip install openai langchain html2text requests mcp langchain-deepseek
```

#### 创建`.env`文件管理环境变量

```
# OpenAI 或其他大模型平台的 API 密钥，可以换成OPENAI_API_KEY等 
DEEPSEEK_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx  
# 飞书机器人的 Webhook，用于发送通知 
FEISHU_WEB_HOOK=https://open.feishu.cn/open-apis/bot/v2/hook/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  
# 飞书消息卡片使用的模板 ID 和版本号 
FEISHU_TEMPLATE_ID= FEISHU_TEMPLATE_VERSION=1.0.0  
# 使用的大模型名称（如 deepseek-chat, gpt-4, gemini-pro 等） 
MODEL_NAME=&quot;deepseek-chat&quot;  
# 模型提供商（如 openai、deepseek、google、anthropic 等） 
MODEL_PROVIDER=&quot;deepseek&quot;
```

### 实现Github Trending爬取函数

首先导入一些必备库

```
import datetime
import json
import re
import requests
from bs4 import BeautifulSoup
from requests.adapters import HTTPAdapter
from requests.packages.urllib3.util.retry import Retry
from requests.exceptions import RequestException
from tqdm import tqdm
from pydantic import BaseModel, Field
from langchain.chat_models import init_chat_model
from dotenv import load_dotenv
import os
```

具体而言使用`BeautifulSoup`，依照所需元素设置爬取策略即可。

同时实现了一个重试策略，避免网络波动原因导致失败。

最后会返回一个包含Github仓库信息的结构化字典，这里我们需要利用url来进一步获取信息

```
def get_github_trending() -&gt; dict[str, dict]:
    &quot;&quot;&quot;获取Github Trending上的仓库信息
    - name: 仓库名称
    - url: 仓库地址
    - stars: 仓库的star数量
    - today_stars: 仓库star增长数量
    &quot;&quot;&quot;
    url = &quot;&lt;https://github.com/trending?since=weekly&gt;&quot;
    # 设置重试策略
    retry_strategy = Retry(
        total=3,
        status_forcelist=[500, 502, 503, 504],
    )
    adapter = HTTPAdapter(max_retries=retry_strategy)
    http = requests.Session()
    http.mount(&quot;https://&quot;, adapter)
    try:
        response = http.get(url)
        response.raise_for_status()
    except RequestException as e:
        raise RuntimeError(f&quot;Error fetching GitHub trending repositories {e}&quot;)
    html = response.text
    soup = BeautifulSoup(html, &#x27;html.parser&#x27;)
    trendings = {}
    for article in soup.select(&#x27;article.Box-row&#x27;):
        repo_info = {}
        name = re.sub(&quot;\\s+&quot;, &quot;&quot;, article.select_one(&#x27;h2 a&#x27;).text)
        repo_info[&#x27;url&#x27;] = f&quot;&lt;https://github.com&gt;{article.select_one(&#x27;h2 a&#x27;)[&#x27;href&#x27;].strip()}&quot;
        repo_info[&#x27;stars&#x27;] = article.select(&#x27;a.Link--muted&#x27;)[0].text.strip()
        today_stars_element = article.select_one(&#x27;span.d-inline-block.float-sm-right&#x27;)
        repo_info[&#x27;today_stars&#x27;] = re.sub(r&quot;[^\\d]&quot;, &quot;&quot;, today_stars_element.text) if today_stars_element else None
        trendings[name] = repo_info
    return trendings
```

### 实现README爬取

函数将返回README的内容，不过需要注意一些项目README有很多无关内容，可以进行一定的截取避免超出上下文长度。

```
def get_repo_readme(repo_url: str) -&gt; str:
    &quot;&quot;&quot;
    根据仓库 URL 获取 README 原始内容
    输入示例: &lt;https://github.com/punkpeye/awesome-mcp-servers&gt;
    返回 README 的 Markdown 文本内容
    &quot;&quot;&quot;
    parts = repo_url.rstrip(&quot;/&quot;).split(&quot;/&quot;)
    owner, repo = parts[-2], parts[-1]
    api_url = f&quot;&lt;https://api.github.com/repos/{owner}/{repo}/readme&gt;&quot;
    headers = {
        &quot;Accept&quot;: &quot;application/vnd.github.v3+json&quot;,
        &quot;User-Agent&quot;: &quot;readme-fetcher&quot;
    }
    response = requests.get(api_url, headers=headers)
    if response.status_code == 200:
        data = response.json()
        content_base64 = data.get(&quot;content&quot;, &quot;&quot;)
        if content_base64:
            readme_text = base64.b64decode(content_base64).decode(&quot;utf-8&quot;)
            readme_text = remove_unnecessary_content(readme_text)
            return readme_text
        else:
            return &quot;[该仓库没有README文件]&quot;
    else:
        return f&quot;[获取失败: {response.status_code}] {response.text}&quot;
```

### 实现过滤和存储本地功能

由于爬取的结果是html，有一些无关信息，为了节省上下文和突出主要内容，先创建一个预处理函数在`utils.py`中

```
import re
import html2text
def remove_unnecessary_content(text: str) -&gt; str:
    &quot;&quot;&quot;
    移除一些无用的HTML标签、文献参考等
    &quot;&quot;&quot;
    ref_pattern = re.compile(r&#x27;
.*?
&#x27;, re.DOTALL)  # 移除参考文献、代码、配置等信息
    handler = html2text.HTML2Text()
    handler.ignore_links = True
    handler.body_width = 0
    handler.images_to_alt = True
    removed_content = handler.handle(text)
    removed_content = ref_pattern.sub(&#x27;&#x27;, removed_content)
    return removed_content
```

为了实现过滤和AI无关的项目功能，我们使用Prompt结合大模型结构化输出来处理，并存在`prompt.py`中以便使用，Prompt设计如下：

```
FILTER_PROMPT = &#x27;&#x27;&#x27;
请根据下面的Github README信息，精简为仓库描述（即这个仓库是用来做什么的，能达成什么效果，特点是什么），在100-150字之间，
你不需要加上“这个仓库是……”，直接开始描述即可：
{README_CONTENT}
你不需要在仓库描述中包含以下内容：
仓库使用协议
仓库引用文献
如何配置项目环境、使用仓库的具体代码
同时，你需要判断仓库是否和AI（人工智能）相关、包括但不限于智能体、大语言模型、多模态、AI应用等。但不要包括只和计算机视觉、机器人控制领域相关的仓库。
&#x27;&#x27;&#x27;
```

我们需要实现过滤功能后，就可以遍历每个url了，然后对于每个仓库进行总结和过滤，最后存为结构化字典。
