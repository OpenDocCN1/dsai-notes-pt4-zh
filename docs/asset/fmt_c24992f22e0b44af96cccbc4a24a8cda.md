
# LangChain快速入门与Agent开发实战

## Part 6. LangChain Agent进阶功能介绍

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/L2J3dEFqBoPyGSxdRjzcVRFonhd](https://kq4b3vgg5b.feishu.cn/docx/L2J3dEFqBoPyGSxdRjzcVRFonhd)

### 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》(夏季班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(夏季班)

![](img/b88468be5b12c39c7b83ee3e070ccd0c.png)

![](img/56f7df7b97fe0e1fc5885f9b2b942a4a.png)

公开课全套学习资料，已上传至网盘（链接: https://pan.baidu.com/s/1_TPoisye8HldxiIXiMxM8Q 提取码: khnp ）

需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

### 1. 借助LangChain Agent+内置工具快速搭建智能体

既然LangChain Agent能更加灵活调用外部工具，LangChain Agent+LangChain内置工具也能更加快速的完成复杂Agent开发。

+   第三方工具：https://python.langchain.com/docs/integrations/tools/

+   LangChain工具：https://python.langchain.com/docs/integrations/tools/tavily_search/

```
!pip install -qU langchain-tavily -i https://pypi.tuna.tsinghua.edu.cn/simple
```

WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv

![](img/31973f084b7265efcf95687a2346864b.png)

```
import os
from dotenv import load_dotenv 
load_dotenv(override=True)
```

```
True
```

```
from langchain_community.tools.tavily_search import TavilySearchResults
```

```
search = TavilySearchResults(max_results=2)
```

```
C:\Users\Admin\AppData\Local\Temp\ipykernel_26100\2306380504.py:1: LangChainDeprecationWarning: The class `TavilySearchResults` was deprecated in LangChain 0.3.25 and will be removed in 1.0. An updated version of the class exists in the :class:`~langchain-tavily package and should be used instead. To use it run `pip install -U :class:`~langchain-tavily` and import as `from :class:`~langchain_tavily import TavilySearch``.
  search = TavilySearchResults(max_results=2)
```

```
search.invoke("苹果2025WWDC发布会")
```

'content': '加利福尼亚州，库比提诺 Apple 今日宣布将于北京时间 2025 年 6 月 10 日至 14 日在线举行年度全球开发者大会（WWDC）。开发者与学生们将有机会于大会开幕当天参与在 Apple Park 举办的线下特别活动。\n\nWWDC25 将聚焦 Apple 软件的最新发展，向所有开发者免费开放。Apple 长期致力于向开发者提供支持。作为这项承诺的一部分，大会将为他们提供机会与 Apple 专家交流，深入了解新工具、新框架和新功能。\n\n“我们无比期待与我们的全球开发者社区再次共度精彩非凡的 WWDC。”Apple 全球开发者关系副总裁 Susan Prescott 表示，“我们迫不及待地希望分享最新工具与技术，赋能开发者，帮助他们持续开拓创新。” [...] *   新闻文章\n    \n    2025 年 3 月 25 日\n    \n    新闻稿\n    \n    Apple 全球开发者大会定于北京时间 6 月 10 日回归\n    \n    WWDC25 将全程在线举行，供所有开发者免费参加\n    \n    加利福尼亚州，库比提诺 Apple 今日宣布将于北京时间 2025 年 6 月 10 日至 14 日在线举行年度全球开发者大会（WWDC）。开发者与学生们将有机会于大会开幕当天参与在 Apple Park 举办的线下特别活动。\n    \n    WWDC25 将聚焦 Apple 软件的最新发展，向所有开发者免费开放。Apple 长期致力于向开发者提供支持。作为这项承诺的一部分，大会将为他们提供机会与 Apple 专家交流，深入了解新工具、新框架和新功能。\n    \n    “我们无比期待与我们的全球开发者社区再次共度精彩非凡的 WWDC。”Apple 全球开发者关系副总裁 Susan Prescott 表示，“我们迫不及待地希望分享最新工具与技术，赋能开发者，帮助他们持续开拓创新。” [...] 新闻稿 2025 年 3 月 25 日\n\nApple 全球开发者大会定于北京时间 6 月 10 日回归\n==============================\n\nWWDC25 将全程在线举行，供所有开发者免费参加\n\n  ![Image 2: 2025 全球开发者大会的文字海报。](img/65f911927cb87a06a5b47f5e8dbfc9d1.png)\n\n[](https://www.apple.com.cn/newsroom/images/2025/03/apples-worldwide-developers-conference-returns-the-week-of-june-9/article/Apple-WWDC25-event-branding.zip)',
  'content': '*   [概览](https://developer.apple.com/cn/wwdc25/)\n*   [特别活动](https://developer.apple.com/cn/wwdc25/special-event/)\n\n\xa0\n\n  ![Image 2](img/2f58d585c5b4d619412ba3bc9a2c74f2.png)  ![Image 3](img/7babbf7b68a57da7c6f486725a381c61.png)\n\n技术和创意活动交织的一周精彩盛宴\n================\n\n**太平洋时间 2025 年 6 月 9 日至 13 日** [...] 亲眼见证 Apple 最新工具、框架和功能的发布。通过 Apple\xa0工程师和设计师主持的视频讲座，了解如何提升 App 和游戏的体验。在实验室中与 Apple\xa0专家互动交流，并与全球开发者社区建立联系。以上活动均免费在线举行。\n\n[添加到日历](https://developer.apple.com/wwdc25/WWDC2025.ics)\n\n##### 体验 WWDC25 的无限精彩\n\n访问 Apple\xa0Developer App、Apple\xa0开发者网站以及 Apple 开发者微信公众号，参与我们的特别活动。\n\n[下载 Apple Developer App](https://apps.apple.com/cn/app/apple-developer/id640199958)\n\n[关注 Apple 开发者微信公众号](https://mp.weixin.qq.com/s/slhQ1RBYUpJ71gwEUlwSTg)\n\n##### WWDC25 将在 Apple\xa0Park 拉开帷幕\n\n申请亲临现场，与我们一起庆祝太平洋时间 6 月 9 日 (周一) 的 WWDC25 开幕日。',

```
[{'title': 'Apple 全球开发者大会定于北京时间6 月10 日回归',
  'url': 'https://www.apple.com.cn/newsroom/2025/03/apples-worldwide-developers-conference-returns-the-week-of-june-9/',
  'score': 0.9047265},
 {'title': 'WWDC25 - Apple 开发者',
  'url': 'https://developer.apple.com/cn/wwdc25/',
  'score': 0.854509}]
```

```
from langchain.agents import AgentExecutor, create_tool_calling_agent, tool
from langchain_core.prompts import ChatPromptTemplate
from langchain.chat_models import init_chat_model

tools = [search]

prompt = ChatPromptTemplate.from_messages(
    [
        ("system", "你是一名助人为乐的助手，并且可以调用工具进行网络搜索，获取实时信息。"),
        ("human", "{input}"),
        ("placeholder", "{agent_scratchpad}"),
    ]
)

# 初始化模型
model = init_chat_model("deepseek-chat", model_provider="deepseek")

agent = create_tool_calling_agent(model, tools, prompt)
```

```
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)

agent_executor.invoke({"input": "请问苹果2025WWDC发布会召开的时间是？"})
```

```
> Entering new AgentExecutor chain...
Invoking: `tavily_search_results_json` with `{'query': '苹果2025WWDC发布会时间'}`

[{'title': '苹果WWDC 2025开发者大会 - 新浪财经', 'url': 'https://finance.sina.com.cn/tech/roll/2025-06-09/doc-ineznafq9993908.shtml', 'content': '苹果公司正式宣布，2025 年全球开发者大会（WWDC 2025）将于太平洋时间6 月9 日至13 日（北京时间6 月10 日至14 日）举行。本届大会将继续以线上形式为主，', 'score': 0.93898576}, {'title': '以下是苹果WWDC 2025 活动在您当地时区的开始时间 - cnBeta.COM', 'url': 'https://www.cnbeta.com.tw/articles/tech/1505364.htm', 'content': '苹果的WWDC 2025 大会即将到来，正式发布iOS 26、iPadOS 26、macOS 26 及其他更新只是时间问题。大会将于6 月9 日星期一拉开帷幕，并将在Apple Park', 'score': 0.91435975}]苹果2025年全球开发者大会（WWDC 2025）将于太平洋时间6月9日至13日（北京时间6月10日至14日）举行。大会将以线上形式为主。
> Finished chain.
```

{'input': '请问苹果2025WWDC发布会召开的时间是？',
 'output': '苹果2025年全球开发者大会（WWDC 2025）将于太平洋时间6月9日至13日（北京时间6月10日至14日）举行。大会将以线上形式为主。'}

### 2. 实战：多智能体协作实现浏览器自动化

正如上述我们使用的`create_tool_calling_agent`方法，它其实在`langChain`中是一个通用的用来构建工具代理的方法，除此以外，`langChain`还封装了非常多种不同的Agent实现形式，大家可以在这个链接中查看到所有LangChain中已经集成的Agent实现形式：

推荐的Agent创建函数

| 函数名 | 功能描述 | 适用场景 |
| --- | --- | --- |
| `create_tool_calling_agent` | 创建使用工具的Agent | 通用工具调用 |
| `create_openai_tools_agent` | 创建OpenAI工具Agent | OpenAI模型专用 |
| `create_openai_functions_agent` | 创建OpenAI函数Agent | OpenAI函数调用 |
| `create_react_agent` | 创建ReAct推理Agent | 推理+行动模式 |
| `create_structured_chat_agent` | 创建结构化聊天Agent | 多输入工具支持 |
| `create_conversational_retrieval_agent` | 创建对话检索Agent | 检索增强对话 |
| `create_json_chat_agent` | 创建JSON聊天Agent | JSON格式交互 |
| `create_xml_agent` | 创建XML格式Agent | XML逻辑格式 |
| `create_self_ask_with_search_agent` | 创建自问自答搜索Agent | 自主搜索推理 |

其中比较通用场景的就是我们刚刚使用的`create_tool_calling_agent`，而对于一些符合OpenAI API RESTFUL API的模型，则同样可以使用`create_openai_tools_agent`，另外像`create_react_agent`可以用于一些推理任务，`create_conversational_retrieval_agent`则可以用于一些对话系统，具体还是需要根据实际需求来选择。

目前来说，在大模型应用开发领域有非常多的需求场景，其中一个比较热门的就是浏览器自动化，通过自动化提取网页内容，然后进行分析，最后生成报告。这样的流程提升效率和收集信息的有效途径。因此接下来，我们就尝试使用`create_openai_tools_agent`来实际开发一个浏览器自动化代理。

首先，执行浏览器自动化代理需要安装一系列的第三方依赖包，如下所示：

```
! pip install playwright lxml langchain_community beautifulsoup4 reportlab
```
