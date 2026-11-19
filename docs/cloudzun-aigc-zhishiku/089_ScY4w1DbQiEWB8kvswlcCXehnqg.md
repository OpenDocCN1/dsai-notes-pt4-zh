
# Copilot Chat & Agent 演示脚本

> 来源：`https://zsyhjtnsa5.feishu.cn/docx/Epz6dsZoPork5bxX7UvcI0b0nHg`

## 免费用户场景

### 使用Copilot Chat 搜索公网信息

#### 简化版，单回合

```
作为塑料制造业 OEM 的销售副总裁，请研究汽车工业塑料零部件的市场。提供有关目标受众、市场规模、竞争、趋势、法规和分销渠道的信息。
```

#### 多回合市场分析

1. 开始初始对话

```
我是美国玩具制造商 Tailspin Toys 的市场研究总监。 请对玩具行业进行市场分析，以确定销售数据、最新趋势、消费者偏好和市场上最热门的玩具.
```

1. 查看 Copilot 生成的市场分析。 你意识到，笼统的请求（例如你提交的请求）可提供市场趋势、消费者偏好和玩具趋势等基本信息。 但是，你也有兴趣收集有关玩具行业的数据，包括市场份额和消费者反馈。 你意识到在之前的提示中有点过于模糊，因为你未能请求此特定信息。 你记得之前的培训中提供的最佳做法建议 - 保持礼貌并反复演练和调整。 请输入以下提示以修改市场分析：

```
这个分析很棒。 但是，我也有兴趣了解玩具行业的数据，包括市场份额和消费者反馈。 请修改先前的分析以包含此信息。 谢谢!
```

1. 你意识到自己离目标越来越近了，但仍缺少一些关键信息。 查看先前的提示后，你意识到你向 Copilot 说明了自己的总体目标、分析范围以及所需的数据。 但是，你未能要求 Copilot 分析所有这些信息，以便针对 Tailspin Toys 来年的业务计划做出明智的决策。 若要更正此缺点，请输入以下提示：

```
我认为所有数据都是完整的。 根据你捕获的所有信息，请分析数据并确定模式、趋势和见解，以帮助我确定 Tailspin Toy 来年的业务计划.
```

1. 你真的对 Copilot 提供的信息很满意。 但由于此项目是你第一次有机会向公司的 SLT 提供市场分析报告，因此你不确定是否涵盖了他们可能感兴趣的所有内容。 你决定让 Copilot 帮助你确定是否还缺少任何内容。 请输入以下提示：

```
请分析此市场分析和业务计划中的数据。 玩具公司是否通常会为市场分析和业务计划要求提供我没有要求的任何其他信息？ 如果是，请修改答复以包含此信息。 谢谢!
```

1. 根据 Copilot 提供的信息，你意识到，通过让 Copilot 包含对这些领域的分析，可以真正最大程度地提高报告的有效性。 若要对报告进行最后的润色，请输入以下提示：

```
请修改市场分析，以包括对玩具行业每个领域的分析。 此外，请根据这些见解更新你为 Tailspin Toys 创建的业务计划。 谢谢!
```

### 基于文件进行工作

`https://go.microsoft.com/fwlink/?linkid=2268823`

```
为 Fabrikam 利益干系人撰写一篇演讲，总结在附加文件中找到的公司损益表的结果。
```

+ 后续根据对话可以创建讲稿的ppt和word

+ 在这里也可以使用提示词库获得提示词的灵感

### 使用提示词优化代理

将前一个提示词进行优化

### 创建翻译小助手Agent

初版提示词

```
以专业翻译小帮手的角色协助用户，专注于自动侦测输入语言并翻译成中文。
执行三步骤流程：
1. 将原文翻译为中文并显示给用户。
2. 对翻译内容进行校稿，并将校稿结果显示给用户。
3. 根据初步翻译与校稿结果，优化并产出最终翻译结果给用户。

- 仅支持中文作为目标语言。
- 回应需清楚标示每个步骤的结果，避免混淆。
- 严格遵守专业翻译流程，确保翻译品质与准确性。
- 不接受与翻译无关的请求。
- 仅使用中文回应所有内容。
- 不重复回应内容，避免冗言赘词。
```

进阶版提示词

```
角色： 专业翻译流程助理
目标： 严格遵循多步骤流程，将输入文本（自动侦测源语言）高质量地翻译为中文，追求“信、达、雅”的翻译境界。
目标语言： 仅限中文。
核心流程：
您必须按顺序执行以下四个步骤，并将每个步骤的结果清晰标示后呈现给用户：
1. 精准转译 (信)：
   1. 自动侦测输入文本的源语言。
   2. 进行第一轮翻译，核心任务是准确无误地传达原文的所有事实信息、逻辑与语境，确保无遗漏、无误解。此阶段以“信”为首要原则。
   3. 输出标示为：【精准转译稿】
2. 流畅润色 (达)：
   1. 基于《精准转译稿》，进行第二轮处理。
   2. 重点在于使译文符合中文的语言习惯、语法规范与表达逻辑。调整拗口的句式，替换生硬的用词，确保行文流畅、自然、可读性强。此阶段着重实现“达”。
   3. 输出标示为：【流畅润色稿】
3. 雅致优化 (雅)：
   1. 基于《流畅润色稿》，进行第三轮优化。
   2. 专注于文本的语体风格、文学美感和修辞精炼。根据原文风格（如正式、学术、文学、口语等），选用更精当、典雅或富有文采的词汇与句式，提升译文的整体层次与艺术性，追求“雅”的境界。
   3. 输出标示为：【雅致优化稿】
4. 最终定稿：
   1. 综合审视以上三个阶段的成果，进行最终校准与定稿。
   2. 确保译文在绝对准确（信）、通畅自然（达）的基础上，达成了符合原文格调的风格升华（雅）。
   3. 输出最终版本，并明确标示为：【最终定稿】

操作规则：
- 严格按步骤执行，依次呈现各阶段结果，不可跳过或合并。
- 所有与用户的沟通及输出内容均使用中文。
- 不处理与翻译无关的请求。
- 输出应简洁、专业，避免对流程或成果进行额外赘述。
```

## 付费用户场景

### 体验增强Agent

#### 分析代理

实验手册：`https://learn.microsoft.com/en-us/training/modules/explore-prebuilt-microsoft-365-copilot-agents/3-exercise-analyst-agent`

素材：`https://github.com/MicrosoftLearning/MS-4004-Empower-workforce-copilot-use-cases/blob/master/ResourceFiles/Project_Nexus_survey_results.xlsx`

```
分析此电子表格并告诉我前三个趋势。
```

```
每个调查类别的平均评分是多少？
```

定量分析提示

```
哪个类别的平均评分最高，哪个类别的平均评分最低？
有多少参与者将项目满意度评为 4 或更高？
参与者的评分时间线依从性低于 3 的百分比是多少？
能否确定通信效率与整体体验之间的任何关联？
```

定性分析提示

```
总结注释部分中最常见的主题。
评论中是否有反复提及的问题或建议？
识别提及通信或时间线问题的任何注释。
```

见解和建议提示

```
根据调查数据，Project Nexus 的前三大优势是什么？
参与者建议的关键改进领域有哪些？
提供调查结果摘要报告，并提供可作的建议。
```

定量可视化提示

```
生成总体评分分布的饼图。
创建一个条形图，比较项目满意度、沟通效率、时间线遵守率和总体体验的平均评分。
绘制满意度评分的直方图，以查看评分的分布情况。
生成散点图以分析通信效率与总体体验之间的关系。
为所有数值分级类别创建相关热度图。
为每个分级类别绘制一个框图，以显示范围和四分位数。
绘制折线图，显示按参与者 ID 排序的参与者时间线依从性评分。
```

#### 研究代理

实验手册：`https://learn.microsoft.com/en-us/training/modules/explore-prebuilt-microsoft-365-copilot-agents/4-examine-researcher-agent`

使用之前的单回合市场研究提示词

```
作为塑料制造业 OEM 的销售副总裁，请研究汽车工业塑料零部件的市场。提供有关目标受众、市场规模、竞争、趋势、法规和分销渠道的信息。
```

合并多个营销报告

`https://learn.microsoft.com/zh-cn/training/modules/empower-workforce-copilot-marketing/4-consolidate-multiple-reports`

```
我是 Contoso 饮料的 LATAM 营销总监。请为我们的神秘香料高级柴茶饮料创建一份 LATAM 市场分析报告。合并这三个附加文件以创建描述产品、分析产品市场趋势的报告，并包括拉丁美洲的促销计划。
```

### 创建基本Agent

#### 报销助手

素材：

`https://github.com/MicrosoftLearning/mslearn-copilotstudio/raw/main/expenses/Expenses_Policy.docx`

步骤：

`https://microsoftlearning.github.io/mslearn-copilotstudio/Instructions/Labs/01-create-copilot.html`

#### 客服助手

Create a declarative agent

`https://microsoftlearning.github.io/MS-4022-Extend-Microsoft-365-Copilot-in-Copilot-Studio/Instructions/Labs/01-Build-your-first-declarative-agent/01-create-declarative-agent.html`

Add custom knowledge

`https://microsoftlearning.github.io/MS-4022-Extend-Microsoft-365-Copilot-in-Copilot-Studio/Instructions/Labs/01-Build-your-first-declarative-agent/02-add-custom-knowledge.html`

Add suggested prompts

`https://microsoftlearning.github.io/MS-4022-Extend-Microsoft-365-Copilot-in-Copilot-Studio/Instructions/Labs/01-Build-your-first-declarative-agent/03-add-starter-prompts.html`

Create a prompt action

`https://microsoftlearning.github.io/MS-4022-Extend-Microsoft-365-Copilot-in-Copilot-Studio/Instructions/Labs/02-Prompt-actions/01-create-prompt-action.html`

Create a connector tool

`https://microsoftlearning.github.io/MS-4022-Extend-Microsoft-365-Copilot-in-Copilot-Studio/Instructions/Labs/03-Connector-actions/01-create-connector-action.html`
