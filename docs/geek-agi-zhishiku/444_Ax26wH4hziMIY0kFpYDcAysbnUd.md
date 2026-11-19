
# FinRobot：专为金融开发的AI代理，可以预测股票走向、生成金融分析报告

> 来源：[https://geekbang.feishu.cn/docx/G2mudZxw6o7QdwxHAJvcE05Bnyb](https://geekbang.feishu.cn/docx/G2mudZxw6o7QdwxHAJvcE05Bnyb)

**板块主理人：尼福财**

·向量光年科技有限公司创始人

·《开源AI项目落地》👸号及社群主理人

专注于AI产品开发落地，移动端、PC端软件快速高质量交付

愿景：加速全行业向AI转变

![](img/92450c20383aff7d7369ff553eb7b2ba.png)

## 项目简介

FinRobot是由AI4Finance基金会开发的一个开源AI代理平台，专为金融应用设计。这个平台使用大型语言模型（LLMs）作为核心技术，致力于通过智能代理解决复杂的金融问题。它包括多层架构，如金融AI代理层、金融LLMs算法层、LLMOps和DataOps层，以及多源LLM基础模型层，支持多样化的金融分析和操作，如市场预测、文件分析和交易策略。此外，FinRobot还具有智能调度器，优化模型的集成和选择。

## 扫码加入AI交流群

获得更多技术支持和交流

（请注明自己的职业）

![](img/c90eb2585ad2ed87e6578696c48cc54a.png)

## 生态系统

FinRobot生态系统的整体架构分为四个独特的层次，每个层次都旨在处理金融AI处理和应用的特定方面：

### 金融AI代理层

现在，金融AI代理层包括金融链式思考（CoT）提示功能，增强了复杂分析和决策能力。市场预测代理、文件分析代理和交易策略代理利用CoT将金融挑战分解为逻辑步骤，将他们的先进算法和领域专业知识与金融市场的动态演变相结合，以提供精确、可操作的洞察。

### 金融LLMs算法层

金融LLMs算法层配置和使用专门调整的模型，针对特定领域和全球市场分析。

### LLMOps和DataOps层

LLMOps层实施多源集成策略，为特定的金融任务选择最合适的LLMs，使用一系列最先进的模型。

### 多源LLM基础模型层

这个基础层支持各种通用和专用LLM的即插即用功能。

## 工作流程

![](img/7db92aab6e166abccdb65d5bc782199d.png)

### 感知模块

该模块捕捉并解释来自市场数据源、新闻和经济指标的多模态金融数据。它采用先进技术对数据进行结构化，以便进行全面分析。

### 大脑模块

作为核心处理单元，该模块从感知模块接收数据，并利用大型语言模型（LLMs）及金融思维链（CoT）过程生成结构化指令。

### 行动模块

该模块执行来自大脑模块的指令，应用工具将分析洞察转化为可行的结果。行动包括交易、投资组合调整、生成报告或发送警报，从而积极影响金融环境。

## 智能调度器

![](img/c64d381ec5d5f15864bd10ed2f028d22.png)

智能调度器是确保模型多样性并优化每个任务最适合的LLM的选择和整合的核心。

### 导向代理

该组件指挥任务分配过程，确保根据代理的性能指标和特定任务的适用性来分配任务。

### 代理注册

管理代理的注册并跟踪系统内代理的可用性，促进高效的任务分配过程。

### 代理适配器

定制代理功能以适应特定任务，提高其在整个系统中的性能和集成度。

### 任务管理器

管理和存储为各种金融任务定制的不同的通用及精细调整的基于LLM的代理，定期更新以确保其相关性和效果。

## DEMO

### 1. 市场预测代理（预测股票走向）

输入一个公司的股票代码、近期基本财务数据和市场新闻，预测其股票走势。

#### ·输入

```
import autogenfrom finrobot.utils import get_current_date, register_keys_from_jsonfrom finrobot.agents.workflow import SingleAssistant
```

#### ·配置

```
# Read OpenAI API keys from a JSON filellm_config = {    &quot;config_list&quot;: autogen.config_list_from_json(        &quot;../OAI_CONFIG_LIST&quot;,        filter_dict={&quot;model&quot;: [&quot;gpt-4-0125-preview&quot;]},    ),    &quot;timeout&quot;: 120,    &quot;temperature&quot;: 0,}
# Register FINNHUB API keysregister_keys_from_json(&quot;../config_api_keys&quot;)
```

#### ·运行

```
company = &quot;NVDA&quot;
assitant = SingleAssistant(    &quot;Market_Analyst&quot;,    llm_config,    # set to &quot;ALWAYS&quot; if you want to chat instead of simply receiving the prediciton    human_input_mode=&quot;NEVER&quot;,)assitant.chat(    f&quot;Use all the tools provided to retrieve information available for {company} upon {get_current_date()}. Analyze the positive developments and potential concerns of {company} &quot;    &quot;with 2-4 most important factors respectively and keep them concise. Most factors should be inferred from company related news. &quot;    f&quot;Then make a rough prediction (e.g. up/down by 2-3%) of the {company} stock price movement for next week. Provide a summary analysis to support your prediction.&quot;)
```

#### ·结果

![](img/2701c853460873ab6c340fc1a0b1bd6d.png)

### 2. 财务分析代理用于撰写报告（股权研究报告）

输入一个公司的10-k表格、财务数据和市场数据，输出一个股权研究报告。

#### ·输入

```
import osimport autogenfrom textwrap import dedentfrom finrobot.utils import register_keys_from_jsonfrom finrobot.agents.workflow import SingleAssistantShadow
```

#### ·配置

```
llm_config = {    &quot;config_list&quot;: autogen.config_list_from_json(        &quot;../OAI_CONFIG_LIST&quot;,        filter_dict={            &quot;model&quot;: [&quot;gpt-4-0125-preview&quot;],        },    ),    &quot;timeout&quot;: 120,    &quot;temperature&quot;: 0.5,}register_keys_from_json(&quot;../config_api_keys&quot;)
# Intermediate strategy modules will be saved in this directorywork_dir = &quot;../report&quot;os.makedirs(work_dir, exist_ok=True)
assistant = SingleAssistantShadow(    &quot;Expert_Investor&quot;,    llm_config,    max_consecutive_auto_reply=None,    human_input_mode=&quot;TERMINATE&quot;,)
```

#### ·运行

```
company = &quot;Microsoft&quot;fyear = &quot;2023&quot;
message = dedent(    f&quot;&quot;&quot;    With the tools you&#x27;ve been provided, write an annual report based on {company}&#x27;s {fyear} 10-k report, format it into a pdf.    Pay attention to the followings:    - Explicitly explain your working plan before you kick off.    - Use tools one by one for clarity, especially when asking for instructions.     - All your file operations should be done in &quot;{work_dir}&quot;.     - Display any image in the chat once generated.    - All the paragraphs should combine between 400 and 450 words, don&#x27;t generate the pdf until this is explicitly fulfilled.&quot;&quot;&quot;)
assistant.chat(message, use_cache=True, max_turns=50,               summary_method=&quot;last_msg&quot;)
```

#### ·结果

![](img/233dd7a6c4cb7b923e10fd4c75aaf5a5.png)

![](img/61b419ad95a6acc1879606b860006f61.png)

金融报告任务：

- 收集初步数据：10-K 报告、市场数据、财务比率
- 分析财务报表：资产负债表、损益表、现金流量表
- 公司概况和业绩：公司描述、业务亮点、部门分析
- 风险评估：评估风险
- 财务表现可视化：绘制市盈率和每股收益
- 综合发现成段落：将所有部分合成一个连贯的总结
- 生成 PDF 报告：使用工具自动生成 PDF
- 质量保证：检查字数

## 项目链接

`https://github.com/AI4Finance-Foundation/FinRobot`

关注「 开源AI项目落地 」👸号

与AI时代更靠近一点
