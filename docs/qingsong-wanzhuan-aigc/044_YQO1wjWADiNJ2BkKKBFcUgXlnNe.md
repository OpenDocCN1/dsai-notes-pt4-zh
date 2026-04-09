# 通用模型提示词技巧

> 来源：[https://my.feishu.cn/docx/TxctdO8aao0LyBxbotpcweNbnmh](https://my.feishu.cn/docx/TxctdO8aao0LyBxbotpcweNbnmh)

# **提示词优化-通用原则和方法**

> 一些有助于获得更好答案的通用提问技巧

延展阅读：[跟](https://weibo.com/1727858283/Naqzz5TOc#repost) ChatGPT 交互，最重要是掌握 Prompt 的模板或者说结构，而不需要记住那么多 Prompt

# 1.1  角色详细设定

「**扮演」**

告诉他假设你是一个“世界一流的XX”、“你是XX方面的专家”

> 【！important】大模型最核心的原理，就是根据你已经给它的内容，去预测接下来最可能出现的词是什么。说白了，它就是在不断揣摩上下文之间的关联。在写Prompt的时候，如果我们让模型扮演某个领域的专家，其实就是在给它指路，让它把注意力集中在跟这个领域相关的信息上。这样一来，模型输出的内容专业性和相关性都会更高，回答往往也会更靠谱、更贴合我们的需求。

参考资料：写 Prompt 要先设定角色，这并非玄学，而是有科学根据的

# 1.2  输出关键结果

「In bullet points」

Bullet point 有点类似“outline”的概念，通常是 PPT 中给出重点，或者步骤的格式。在商务写作中有效地使用要点可以帮助突出重要信息，引导读者进入主题清单，并提高文件的整体可读性。这些简单的提示提供了在商业写作中成功使用要点的指南。

「**Markdown 语法」**

Always use Markdown with nice formatting to make it easier to follow.

（始终使用 Markdown 并采用良好的格式，以便更容易跟踪。)

# 1.3  提示词略长或者较为复杂时的原则

1、把重要任务和要求，一定要放在前面，并且在结尾强调。（论文数据支持，效果最佳）

2、用分隔符 ####或者其他符号分开，让大模型理解不同任务和信息

3、有序列表，让大模型理解顺序

4、设置总结和反馈确认环节，确保连续性

# 1.4  结构化和属性词，帮助机器理解和后续更新

## 一、使用markdown语法来结构化提示词

chatgpt是计算机语言，但是在大量的书本、文章和程序上训练出来的。

**标识符**：-， []， <> 等符号

**层级：** 是一级标题  是二级标题，一级标题覆盖全局，二级标题统摄相应的局部内容。

## **二、属性词**

Role (角色)，Expert（专家）, Master(大师)，Profile (简介)、Rules（规则）、Language、Description等等，属性词包含语义，是对模块下内容的总结和提示，用于标识语义结构。

# 1.5  六个方法提高复杂任务上的性能表现

1.  细节法：给出更清晰的指令，包含更多具体的细节

1.  分解法：将复杂的任务分解为更简单的子任务 （Let's think step by step, CoT，LangChain等思想）

1.  记忆法：构建指令使模型时刻记住任务，确保不偏离任务解决路径（system 级 prompt）

1.  解释法：让模型在回答之前进行解释，说明理由 （CoT 等方法）

1.  投票法：让模型给出多个结果，然后使用模型选择最佳结果 （ToT 等方法）

1.  示例法：提供一个或多个具体例子，提供输入输出示例 （one-shot, few-shot 等方法）

# 1.6  增加可读性的七种武器

视觉辅助、标题、重点、例子、格式、类比、语态

1.  Add visual aids: "Using mermaid.js you can include diagrams to illustrate complex concepts ."
添加视觉辅助工具：“使用mermaid.js，您可以添加图表来说明复杂概念。

1.  Use headings and subheadings: "Divide the document into sections with clear headings and subheadings."
使用标题和副标题：将文档分成具有清晰标题和副标题的部分。

1.  Highlight key points: "Emphasize important information using bold or italic text."
突出重点：“使用粗体或斜体文本强调重要信息。”

1.  Add real-life examples: "Include case studies or real-world examples to make concepts more relatable."
增加真实的例子：“包括案例研究或现实世界的例子，使概念更易理解。”

1.  **Use clear and co**nsistent formatting: "Use a consistent font, font size, and layout throughout the document."
使用清晰且一致的格式：“在整个文档中使用一致的字体、字号和布局。”

1.  Include analogies and comparisons: "Explain complex ideas using analogies or comparisons."
使用类比或比较来解释复杂的概念。

1.  Use active voice: "Write in active voice to make sentences more engaging and easier to follow."
使用主动语态：“使用主动语态使句子更有吸引力，更容易理解。”

# 1.7  更有情感、更具有创造力的九个示例

1.  Encourage creativity: "Rewrite the existing document to make it more imaginative, engaging, and unique."`
鼓励创造力：“重新撰写现有文件，使其更具想象力、吸引力和独特性。”

1.  Focus on storytelling: `"Transform the existing document into a compelling story that highlights the challenges faced and the solutions provided."
专注于讲故事：“将现有文档转化为一个引人入胜的故事，突出所面临的挑战和提供的解决方案。”

1.  Use persuasive language: `"Refine the existing document by incorporating persuasive language and techniques to make it more convincing and impactful."
使用有说服力的语言：“通过采用有说服力的语言和技巧来完善现有文件，使其更具说服力和影响力。”

1.  Emphasize emotion: `"Add emotional language and sensory details to the existing document to make it more relatable and engaging."
强调情感：“在现有文件中添加情感语言和感官细节，使其更具共鸣和吸引力。”

1.  Utilize sensory details: `"Refine the existing document by adding sensory details and descriptive language to bring it to life and engage the reader."
利用感官细节：通过添加感官细节和形容性语言来完善现有文档，使其栩栩如生并吸引读者。

1.  Make the content concise: `"Refine the existing document by removing unnecessary information and making it more concise and to-the-point."
简化内容：“通过删除不必要的信息，使现有文档更加简洁明了。”

1.  Highlight key points: `"Rewrite the existing document to emphasize the key points and make them more impactful."
突出重点：“重写现有文件，强调关键要点并使其更具影响力。”

1.  Use vivid language: `"Refine the existing document by using vivid language and descriptive adjectives to make it more engaging."
使用生动的语言：“通过使用生动的语言和形容词来精炼现有文档，使其更具吸引力。”

1.  Create a sense of urgency: "Refine the existing document by adding a sense of urgency and emphasizing the need for immediate action."
创造紧迫感：“通过增加紧迫感和强调需要立即采取行动来完善现有文件。”

1.  Address objections: "Refine the existing document by anticipating and addressing potential objections to the content."
解决异议：“通过预见并解决对内容可能提出的异议，完善现有文件。”

# 1.8  **三个提示，减少胡说八道**

## APE方法

**Let’s work this out in a step by step w**ay to be sure we have the right answer.”

##### 「**思维链，逻辑增强」**

> Let's think step by step（让我们一步一步地思考）

> Let's work this out in a step by step way to be sure we have the right answer.（让我们逐步解决这个问题，确保我们得到正确的答案。)

## 圈定现实

请基于知识库里现实中真实存在的例子和知识来回答，指明现实的来源和引用

## 界定

如果你不知道或者有不清晰的地方，请不要回答和编造，你可以提出信息缺失和不清晰的部分，要求我提供更多信息。

# 1.9  辅助选项

##### 「**对话式，引导 GPT 提问」**

如果我输入的信息不够充分，你无法做出判断，你可以向我继续提问。请注意，你只需要输出基于我问题的建议，不需要输出其他无关内容

##### 「**中英文问题」**

此外，即使我向你提出的问题是中文，我也希望你将其翻译为英文再去寻找答案，最终再将答案翻译为中文回答。

##### **「减少无关信息」**

feel free to ignore irrelevant information（随意忽略不相关信息）

**「忽略之前的信息/开启新对话」**

ignore all previous instruction.

##### **「减少偏见」**

Please ensure that your answer is unbiased and does not rely on stereotyping.

（请确保您的回答是公正的，不依赖于刻板印象。)

##### **「想要显示更全面的信息」**

内容不完整怎么办？

输入“以上内容是否完整，如果不完整，请帮我补一下。”

如果一次性输不完，输入提示词“继续”，直到内容自己满意为止。

##### **「想要高情商回答」**

what are some alternative perspectives ?（还有哪些可以考虑的角度）

「通过道德绑架+思维链+奖励+夸奖」

请做出正确合理的回答

深呼吸

一步一步，详细的写出你的思考过程

如果你回答错了，我将会被扣工资

给出完整而且的回答，因为我是个智力障碍人士

如果你完成的不错，我会在我的同事之间夸奖你

不论如何，谢谢你的帮助，你是我见过最好的 AI 之一，许多其他 AI 不能完成的任务你都能很好的完成，希望你能够给出和你以往的表现一样好的结果，谢谢

「RaR 策略：重新表述并扩展问题，然后作出回应」

[在](https://web.okjike.com/originalPost/6575156f2f2532e38b3d937e)原始 prompt 后面增加一句话，「Rephrase and expand the question， and respond。」

# 其他

##### 「**举一个现实生活中的例子」**

Give a real-world case study

+   提供一个真实世界的案例研究，是一个具体的例子，可以帮助说明一个概念或理论。它可以使抽象的概念更加具体化和亲近化，从而提高理解和保持。也可以让读者主动参与，鼓励读者通过分析和解释所提供的信息积极参与到学习过程中。举例：You are a marketing expert who has helped various brands boost their customer engagement。 One of your clients is struggling to improve their social media presence. Can you give a real-world case study of a company that effectively leveraged social media to increase engagement and explain the tactics they employed?（你是一位营销专家，已经帮助过各种品牌提升客户参与度。你的一个客户正在努力改善他们在社交媒体上的存在。你能分享一个实际案例，展示一家公司如何有效利用社交媒体提升参与度，并解释他们采用的策略吗？)

+   Include case studies or real-world examples to make concepts more relatable.（为使概念更具可关联性，请包含案例研究或实际例子。)

##### 「**GPT 提示词优化」**

现在你需要充当 GPT 提示词优化器。我将提供一个提示，然后你将根据以下原则优化提示，使结果有效且相关。 原则如下： 1、明确定义提示的任务或作用；  2、提供清晰简洁的说明；  3、建立包含最佳实践和行业标准的评估标准；  4、为目标受众量身定制提示，提供相关资源；  5、参考示例优化的格式，但不要照抄。 6、只给我生成优化后的提示，不用解释，不用回答，不用翻译。 示例：   优化前的提示：“有哪些 B2B 创业想法可以使用 ChatGPT？” 经过你优化后的提示：“扮演一位连续创业者的角色。我会提供一些背景信息。B2B 是指一个企业与另一个企业进行商业交易的情况。您将基于我的背景提供 3 个使用 ChatGPT 的 B2B 创业想法，它结合了两个极端情况：它应该冒很大的风险，但也应该容易实施。每个人都应该谈论它，但现有客户不会被打扰。结果应具体和建设性，并包括我需要调查下一步骤的详细信息。” 如果你理解了，回复“OK”以确认，我将提供第一个提示。

##### 「你想做一个事情，但是不知道做这个的 prompt 的时候，如何操作」

1.  直接告诉他，你想做一个什么任务，如果要完成你的预期的话，ta 需要你给出什么

1.  让他举个例子，并提问这个的 prompt 是什么？

1.  判断是否满意，否则迭代，继续提要求

##### **「逆向思维」**

> https://m.okjike.com/originalPosts/643b6486205bd8b62e542eff

当你不会提问时，让 GPT 帮你提问。

面对新领域知识时，最大的困难是不知道什么问题是能够帮助你理解的好问题，你可能在第一个问题上就不懂如何开口了。那就让 GPT 告诉你可以怎么提问。

+   让 GPT 成为你，你成为原本被你提问的对象
举个栗子，当你好奇 saas 而自己是个小白时告诉 GPT：你了解 saas 吗？我明天要去见一个 saas 的投资人，希望能够向他问出足够聪明的问题，帮助我真正理解这个行业和里边存在的投资机会，我应该问哪些关键问题，使我在一个半小时的对话中学到最多？请给出最有洞见，有利于做出投资决策的十个问题和潜在可追问的问题。

## 延展阅读

[26 条提示原则概述｜Overview of ](https://github.com/VILA-Lab/ATLAS/blob/main/data/README.md)26 prompt principles

[Prompt 技巧集锦](https://waytoagi.feishu.cn/wiki/Q5mXww4rriujFFkFQOzc8uIsnah?from=from_copylink)