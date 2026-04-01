# 深入CrewAI，手把手教你快速构建多智能体系统

> 来源：[https://agijuejin.feishu.cn/docx/NxY5dniquoZi7wxeLtfcPj5mncU](https://agijuejin.feishu.cn/docx/NxY5dniquoZi7wxeLtfcPj5mncU)

> 作者：猕猴桃 | 探索AGI

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/ou9wyMxmkzd8uThsefbQzg)qq.com/s/ou9wyMxm...

嘿，大家好！这里是一个专注于AI智能体的频道！

今天，聊聊CrewAI，这个Python中的多智能体系统构建工具，简直是自动化领域的扛把子。想象一下，一群AI Agent各司其职，协同作战，解决复杂问题，是不是听起来就很酷炫？

在 CrewAI 中，每个 Agent 都被赋予了不同的角色和能力，使他们擅长处理不同的任务。比如角色为“作家”，则擅长撰写吸引人的的故事，角色为“算法工程师”，则擅长搜集前沿的科技信息。清晰的角色定义确保每个智能体都能有效地贡献自己的力量。

多智能体有什么用呢？比如说一个具体的应用场景，市场研究和分析，一个市场研究团队可能包括收集市场趋势数据、分析竞争对手、生成综合报告和提供可操作见解的智能体。这使公司能够根据最新的市场动态做出明智的决策。

接下来，实战一个简单的例子：

1.  安装

```
pip install crewai
pip install &#x27;crewai[tools]&#x27;
```

1.  构建智能体，角色

```
import os
from crewai import Agent
from crewai_tools import SerperDevTool

os.environ[&quot;SERPER_API_KEY&quot;] = &quot;Your Key&quot;
os.environ[&quot;OPENAI_API_KEY&quot;] = &quot;Your Key&quot;

search_tool = SerperDevTool()

researcher = Agent(
  role=&#x27;Researcher&#x27;,
  goal=&#x27;探索{topic}中的突破性技术&#x27;,
  verbose=True,
  memory=True,
  backstory=&quot;在好奇心的驱使下，你站在科技前沿，渴望探索和分享能够改变世界的知识。&quot;,
  tools=[search_tool],
  allow_delegation=True
)

writer = Agent(
  role=&#x27;Writer&#x27;,
  goal=&#x27;讲述关于{topic}的吸引人的技术文章&#x27;,
  verbose=True,
  memory=True,
  backstory=&quot;你凭借化繁为简的天赋，以引人入胜的叙述方式吸引读者，以通俗易懂的方式写出你的思考和观点&quot;,
  tools=[search_tool],
  allow_delegation=False
)
```

1.  定义任务

```
from crewai import Task

research_task = Task(
  description=&quot;确定{topic}中的下一个大趋势。专注于优缺点以及整体概况。你的最终报告应清楚地阐述关键点、市场机会和潜在风险。&quot;,
  expected_output=关于最新人工智能趋势的综合三段式报告。&#x27;,
  tools=[search_tool],
  agent=researcher,
)

write_task = Task(
  description=&quot;撰写一篇关于{topic}的有见地的文章。关注最新趋势以及它如何影响行业。这篇文章应该通俗易懂、引人入胜且积极向上。&quot;,
  expected_output=&#x27;一篇关于{topic}进展的4段式文章，格式为Markdown。&#x27;,
  tools=[search_tool],
  agent=writer,
  async_execution=False,
  output_file=&#x27;new-blog-post.md&#x27;
)
```

1.  pipeline构建

```
from crewai import Crew, Process

crew = Crew(
  agents=[researcher, writer],
  tasks=[research_task, write_task],
  process=Process.sequential,
  memory=True,
  cache=True,
  max_rpm=100,
  share_crew=True
)
```

1.  启动

```
result = crew.kickoff(inputs={&#x27;topic&#x27;: &#x27;Ai医疗&#x27;})
print(result)
```

通过利用多智能体系统的力量，你可以在各种应用中提高效率、准确性和可扩展性。

好了，这就是我今天想分享的内容。如果你对构建AI智能体感兴趣，别忘了点赞、关注噢~