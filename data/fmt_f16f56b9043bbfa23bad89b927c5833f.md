
# 使用CrewAI构建多智能体品牌监控应用

> 来源：[https://v11enp9ok1h.feishu.cn/docx/D9A5dVGfBo2ZDax1CYmcBJ7Nnub](https://v11enp9ok1h.feishu.cn/docx/D9A5dVGfBo2ZDax1CYmcBJ7Nnub)

## 系统架构概述

品牌监控系统是一个多智能体应用，能够跨平台抓取品牌提及信息并生成深度洞察报告。该系统基于以下技术栈构建：

![](img/bad44e5591893ee4e1cee26323254826.png)

### 技术栈

+   `Bright Data`：用于大规模数据抓取

+   `CrewAI`：用于智能体编排和工作流管理

+   `Ollama` + `DeepSeek R1`：本地部署的大语言模型，提供AI能力

+   `Streamlit`：用于构建交互式用户界面

### 系统工作流

1.  使用`Bright Data`抓取各平台（X、Instagram、YouTube等）的品牌提及

1.  调用特定平台的`Crew`（智能体团队）分析数据并生成洞察

1.  合并所有洞察，生成最终品牌报告

1.  数据抓取工具实现

### 1.1 `Bright Data`抓取工具

要监控品牌，我们需要从多个来源抓取数据——X、YouTube、Instagram、网站等。因此，我们首先从`Bright Data`的`SERP API`获取最近的搜索结果。

![](img/899e1dac9a5476cb46f553259b8edf20.png)

```
from crewai.tools import BaseTool
from pydantic import BaseModel, Field
import requests

class BrightDataWebSearchToolInput(BaseModel):
    """BrightDataWebSearchTool的输入模式"""
    title: str = Field(..., description="要搜索的主题")

class BrightDataWebSearchTool(BaseTool):
    name: str = "Web Search Tool"
    description: str = "用于搜索Google并获取结果的工具"
    args_schema: Type[BaseModel] = BrightDataWebSearchToolInput

    def _run(self, title):
        host = 'brd.supexproxy.io'
        port = 33335
        username = '<从BrightData.com的SERP API获取>'
        password = '<从BrightData.com的SERP API获取>'
        
        proxies = {'http': f'http://{username}:{password}@{host}:{port}', 'https': f'http://{username}:{password}@{host}:{port}'}
        
        url = f"https://www.google.com/search?q={title}&brd_json=1&num=500"
        response = requests.get(url, proxies=proxies, verify=False)
        return response.json()['organic']
```

### 1.2 平台特定抓取函数

上述输出将包含指向网页、X帖子、YouTube视频、Instagram帖子等的链接。为了抓取这些来源，我们使用`Bright Data`的平台特定抓取器。

![](img/ce5ca6152be16673f500762db059c7df.png)

```
def scrape_urls(input_urls: list[str], initial_params: dict, scraping_type: str):
    print(f"Scraping {scraping_type} for {len(input_urls)} urls")
    
    url = "https://api.brightdata.com/datasets/v3/trigger"
    headers = {"Authorization": f"Bearer {os.getenv('BRIGHTDATA_API_KEY')}", "Content-Type": "application/json",}
    
    data = [{"url": url} for url in input_urls]
    
    scraping_response = requests.post(
        url,
        headers=headers,
        params=initial_params,
        json=data
    )
    
    snapshot_id = scraping_response.json()['snapshot_id']
    output_url = f"https://api.brightdata.com/datasets/v3/snapshot/{snapshot_id}"
    output_response = requests.get(
        output_url,
        headers=headers,
        params={"format": "json"})
    return output_response.json()
```

## 本地部署`DeepSeek R1`模型

![](img/8f8ac824e5b77ae55bd8251834f3bdce.png)

我们通过`Ollama`在本地部署`DeepSeek R1`模型，为智能体提供AI能力。

### 2.1 安装`DeepSeek R1`模型

首先，在命令行中执行以下命令拉取`DeepSeek R1`模型：

```
ollama pull deepseek-r1
```

### 2.2 在`CrewAI`中定义`LLM`

```
from crewai import LLM
llm = LLM(model="ollama/deepseek-r1")
```

## 智能体团队(`Crew`)设置

![](img/0038c859ecbb2c9250ffd92e0b804d44.png)

我们将为每个平台（X、Instagram、YouTube等）创建一个专用团队。每个团队包含两个智能体：

+   **分析智能体**：分析抓取的内容

+   **撰写智能体**：根据分析结果生成洞察

### 3.1 X平台分析智能体

该智能体分析由`Bright Data`抓取的帖子并提取关键洞察。

![](img/16b58932348cddd87220b2afe371c8e4.png)

```
from crewai import Agent, Task

analysis_agent = Agent(
    role="X Analysis Agent",
    goal="分析X平台帖子中`{brand_name}`的使用情况，包括URL、浏览量、点赞数、回复、转发、标签、引用、收藏、描述、被标记用户、发帖者ID等细节",
    backstory="你是X平台帖子分析专家，了解平台及其算法。你能够分析帖子、描述、浏览量、点赞、回复、转发、标签、引用、收藏、被标记用户，并分析帖子中品牌的使用情况。")

analysis_task = Task(
    description="分析X平台帖子中`{brand_name}`的使用情况，包括URL、浏览量、点赞数、回复、转发、标签、引用、收藏、描述、被标记用户、发帖者ID等细节。",
    expected_output="关于X平台帖子及其`{brand_name}`使用情况的简洁分析。包括帖子语气、对品牌的情感倾向、是否获得互动、是否为付费合作等内容。",
    agent=analysis_agent
)
```

### 3.2 X平台撰写智能体

该智能体接收X平台分析智能体的输出并生成洞察报告。

![](img/658d76273d20163872f409de907070ee.png)

```
writer_agent = Agent(
    role="Senior Report Writer Agent",
    goal="使用X平台分析智能体获得的分析结果，撰写简洁的要点报告，说明`{brand_name}`在帖子中的使用方式。",
    backstory="你是X平台帖子分析专家，了解平台及其算法。你能够使用X平台分析智能体获得的分析结果，撰写简洁的要点报告，说明`{brand_name}`在帖子中的使用方式。")

writer_task = Task(
    description="撰写关于X平台帖子分析的简洁要点报告，以及`{brand_name}`在帖子中的使用方式。",
    expected_output="关于X平台帖子分析以及`{brand_name}`在帖子中使用方式的清晰简洁的要点报告。对于输入数据中的每个帖子，输出应采用提供的结构化格式：",
    agent=writer_agent
)
```

输出格式要求：

+   描述`{brand_name}`提及的简短标题

+   帖子URL

+   详细说明帖子中`{brand_name}`提及的要点，包括帖子语气、对品牌的情感倾向、是否获得互动、是否为付费合作等。

## 创建工作流(`Flow`)

最后，我们使用`CrewAI` `Flows`来编排工作流：

1.  首先使用抓取工具启动工作流

1.  调用平台特定的抓取器

1.  调用平台特定的智能体团队

![](img/e3bd7856043d4706261331a753e1e247.png)

```
from crewai.flow import Flow, listen, start
from pydantic import BaseModel

class BrandMonitoringFlow(Flow[BrandMonitoringState]):
    @start()
    def scrape_data(self):
        search_tool = BrightDataWebSearchTool()  # 抓取品牌提及的URL
        self.state.search_result = search_tool(self.state.brand_name)

    @listen(scrape_data)
    def scrape_platform_data_and_analyse(self):
        # X平台抓取
        X_posts = scrape_urls(
            self.state.search_response.x_urls, 
            X_params, 
            "X")
        
        X_crew = XCrew().crew()
        X_response = X_crew.kickoff(
            inputs={"X_data": X_posts, 
                "brand_name": self.state.brand_name
            })
        # YouTube平台抓取
        YouTube_transcripts = scrape_urls(
            self.state.search_response.YouTube_urls, 
            YouTube_params, 
            "X")
        
        YouTube_crew = YouTubeCrew().crew()
        YouTube_response = YouTube_crew.kickoff(
            inputs={"X_data": YouTube_posts, 
                "brand_name": self.state.brand_name
            })
        # 为所有平台重复上述过程
```

## `Streamlit` UI与工作流启动

![](img/071e4efba9e2ffbf80666b7c836993ab.png)

最后，我们将应用程序包装在清晰的`Streamlit`界面中以实现交互性并运行工作流。

UI界面包含以下元素：

+   品牌名称输入框

+   搜索结果数量调节

+   "开始分析"和"重置"按钮

+   多平台提及展示区域（LinkedIn、X/Twitter、网页等）

## 总结

通过本教程，我们构建了一个完整的品牌监控系统，该系统能够：

1.  跨多个平台抓取品牌提及信息

1.  使用AI智能体分析这些信息

1.  生成结构化的洞察报告

1.  通过直观的UI展示结果
