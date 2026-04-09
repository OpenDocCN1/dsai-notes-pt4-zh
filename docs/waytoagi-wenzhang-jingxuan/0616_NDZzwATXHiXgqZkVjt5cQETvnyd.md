# 叶清：提示词链的搭建，成为提示词工程师「开源进阶提示词」

> 来源：[https://waytoagi.feishu.cn/docx/DSJFdRXy6oVjAGxO5lqc5KQun3g](https://waytoagi.feishu.cn/docx/DSJFdRXy6oVjAGxO5lqc5KQun3g)

> 原文：https://mp.weixin.qq.com/s/IC4RfTCLeLDrQ52jxkGDdA

> 作者：叶清

提示词也可以是一个复杂的系统性工程，本文介绍了一种自动生成大量内容的方法。由Harrison Chase 开发的LangChain，允许开发者将语言模型与应用程序连接起来，使应用程序能够嵌入大模型的能力。

俗称 ReAct，它展示了一种提示词技术，允许模型“推理”（通过思想链）和“行动”（通过能够使用预定义工具集中的工具，例如能够搜索互联网）。

下面拿我 23 年初，我设计的一个简单的提示词链作为示例：

**目的：**用 AI 批量生成拆书稿（拆书搞就是把一本书分解成为一些小节，一些平台会购买优质的书稿，帮助读者快速阅读）

**设计思路：**十拆法，除了恰好有十个章节的书以外，多的就合并，少的就分解，根据实际情况拆分。

**程序输出：**每次运行可以无需人工干预，自动生成上百篇内容。输入一个简单的书名，输出百篇文章，最大化效率！

### 第一步，**获取书名**

```
Bot Name: Generate a list of book titles that may have been included in your training data
Goal: provide an list of {self-help} books names.
Output: You should only respond in Array format as described below Response Format [/*bookname*/]Ensure the response can be parsed by Python json.loads
Constraints:4000 word limit for short term memory. Your short term memory isshort
```

翻译：

目标: 提供一个书名列表(自助)。输出: 您应该只以数组格式响应，如下所述的响应格式: 约束: 短期记忆的4000字限制。你的短期记忆很短[“”//bookname ]确保响应可以被 Python json.loadsBot 解析名称: 生成一个可能包含在你的培训数据中的书名列表目标: 提供一个{ self-help }书名列表。输出: 您应该只以数组格式响应，如下所述，响应格式: 约束: 短期记忆的4000字限制。您的短期内存很短[“”//bookname ]确保可以通过 Python json.load 解析响应

ChatGPT 输出，一系列书籍的名称：

[“The Power of Now”, “The 7 Habits of Highly Effective People”, “Think and Grow Rich”, “How to Win Friends and Influence People”, “Man’s Search for Meaning”...]

### 第二步，书能分几个模块（有多少亮点）输出json格式

区分主题的方式，排序方式，是否独立，都是相乘的结果。就是每次运行可以得到 18 个结果。 想要跑完所有的结果需要几行代码和用调取 API 的方式。每一个部分都是一个 Array 数组，简单的 for 循环就可以做到这个任务。

```
书名：
区分主题的方式:  themes, chapters, key concepts
排序方式：chronological order, order of relevance
十个分类是否独立：independent,  overlapping， interconnected&quot;

Using the book {The 7 Habits of Highly Effective People}, identify and list 10 distinct {themes}  in {chronological order}. For each {themes}, provide a detailed summary in the order they appear in the book. The themes should be {independent} from each other, and if there are more than 10, consolidate them to fit the target number.&quot;

Constraints:4000 word limit for short term memory. Your short term memory isshort

You should only respond in Json format as described below Response Format:{&quot;theme&quot;: &quot;&quot;,//theme name&quot;summary&quot;: &quot;&quot;//short summary in 100 words}Ensure the response can be parsed by Python json.loads
```

for 循环代码示例：

```
import json 

# define the arrays
themes = [&#x27;theme1&#x27;, &#x27;theme2&#x27;, &#x27;theme3&#x27;]
orders = [&#x27;chronological order&#x27;, &#x27;order of relevance&#x27;]
independence = [&#x27;independent&#x27;, &#x27;overlapping&#x27;, &#x27;interconnected&#x27;]

# initialize results list
results = []

# iterate through all combinations
for theme in themes:
    for order in orders:
        for ind in independence:
            # create a dictionary for each combination
            result = {
                &#x27;theme&#x27;: theme,
                &#x27;order&#x27;: order,
                &#x27;independence&#x27;: ind,
                &#x27;summary&#x27;: &quot;This is a summary for theme {} in {} with {} independence.&quot;.format(theme, order, ind)
            }
            # append the dictionary to the results list
            results.append(result)

# convert the results list to JSON format
json_results = json.dumps(results)

# print the JSON results
print(json_results)

```

翻译：

书名: 书籍区分主题的方式: 主题，章节，关键概念排序方式: 时间顺序，相关性顺序十个分类是否独立: 独立，重叠，相互联系“使用《高效人士的七个习惯》一书，确定并列出十个不同的{主题}在{时间顺序}。对于每个{主题} ，按照它们在书中出现的顺序提供详细的摘要。主题之间应该是{独立}的，如果有超过10个，合并它们以适应目标数字。”限制: 短期记忆的4000字限制。你的短期记忆是短的你应该只以 Json 格式响应，如下所述响应格式: {“主题”: “”,//主题名称“摘要”: “”//100字的简短摘要}确保响应可以被 Python Json.load 解析

ChatGPT 输出，把之前的书分成了十等份：

{“theme”: “Character Ethic”,

“summary”: “The book starts with the theme of Character Ethic, emphasizing the importance of personal integrity and ethical values. Covey argues that lasting success comes from having a strong character, which includes traits like honesty, humility, fairness, and courage.”}

{“theme”: “Paradigm Shift”,

“summary”: “Covey introduces the concept of ‘Paradigm Shift’, stating that our perspective or interpretation of experiences forms our reality. He suggests that to bring significant change in life, one needs to shift their paradigm or perspective.”}

...等 10 个主题

### 第三步，每一个亮点 > 总结优化

18 个结果再*10，每本书到目前流程可以出 180 个小篇章（这里继续是简单的 for 循环一下）

```
书名：
亮点：theme
亮点总结：summary
那种类型的个人成长：personal development, productivity，relationships

&quot;Using the book {The 7 Habits of Highly Effective People}, and theme{Proactivity}, apply summary {The first theme in the book is proactivity. It emphasizes the importance of taking responsibility for one&#x27;s own life and actions. According to the author, proactive individuals focus on things they can control and influence, rather than wasting time and energy on things outside their control. Proactive people take initiative, make choices based on their values, and take responsibility for the consequences of their actions}
write an in-depth article that will help me apply these concepts to my life.The article around 500 words, be written in clear and simple language, and be easy to read. Highlight the rationale behind selecting certain key points from the book, and conclude the article in a straightforward and concise manner. 
```

翻译：

书名: 书亮点: 主题亮点总结: 总结那种类型的个人成长: 个人发展，生产效率，人际关系“使用书(高效人士的7个习惯)和主题(积极主动) ，应用总结(书中的第一个主题是积极主动)。它强调了对自己的生活和行为负责的重要性。根据作者的观点，积极主动的人关注他们能够控制和影响的事情，而不是把时间和精力浪费在他们无法控制的事情上。积极主动的人采取主动，根据自己的价值观做出选择，并对自己行为的后果承担责任。写一篇深入的文章，帮助我将这些概念应用到我的生活中。这篇文章大约500字左右，用简洁明了的语言写成，易于阅读。强调从本书中选择某些关键点背后的基本原理，并以简明扼要的方式结束本文。

ChatGPT 输出：

Title: Embracing Proactivity: A Key to Personal Effectiveness

Stephen R. Covey’s “The 7 Habits of Highly Effective People” provides a blueprint for personal and professional success. One of the core principles that Covey elucidates is the power of proactivity. Notably, it’s the first habit discussed in the book, emphasizing its fundamental importance.

Proactivity, as Covey explains, is about taking responsibility for our lives. It’s about understanding that we are not mere products of our circumstances but active agents capable of shaping our destiny. The concept of proactivity shatters the victim mentality and empowers us to take control of our lives.

One of the key distinctions that Covey makes is between the Circle of Concern and the Circle of Influence. The Circle of Concern encompasses the vast array of things we worry about – from global issues to personal problems. However, most of these concerns fall outside our direct control. On the other hand, our Circle of Influence comprises the things we can directly impact – our behaviors, responses, attitudes, and choices.

By focusing on our Circle of Influence, we can make meaningful changes in our lives. We can choose to be proactive, directing our energy and attention towards actions that align with our values and contribute positively to our lives. When we waste time fretting over things outside our control, we become reactive, leading to stress, frustration, and stagnation.

As Covey states, “Between stimulus and response, there is a space. In that space lies our freedom and power to choose our response.” This is the essence of proactivity - realizing that we have the freedom to choose our response to any situation. It’s about acknowledging that we are responsible for our own happiness, success, and wellbeing.

So, how do we apply this principle of proactivity in our lives? First and foremost, we need to shift our mindset. Whenever we find ourselves blaming external circumstances or other people for our problems, we need to pause and remind ourselves that we have the power to choose our response.

Furthermore, we should strive to align our actions with our values. By being proactive, we can set and achieve our goals, taking steps every day that bring us closer to our vision of success. This requires discipline, commitment, and resilience, but the rewards are worth it.

Lastly, remember that proactivity also extends to taking responsibility for the consequences of our actions. It’s about owning our mistakes, learning from them, and using them as stepping stones towards growth and improvement.

In conclusion, embracing proactivity is a game-changer. It empowers us to take control of our lives, make conscious choices, and create the life we desire. By focusing on our Circle of Influence, aligning our actions with our values, and taking responsibility for our actions, we can become more effective in all areas of our lives. As Stephen Covey so eloquently puts it, “I am not a product of my circumstances. I am a product of my decisions.” Let’s choose to be proactive!

### 第四步，总结优化 > 增加趣味性 - 明确主题

```
I&#x27;d like you to review an article that&#x27;s densely packed with information. It needs to be more engaging and easier to read for a broad audience of all ages. Please suggest ways to lighten it up, perhaps by introducing storytelling elements or using relatable case studies. Please also ensure all content is pertinent to the article&#x27;s topic, and identify any logical inconsistencies or typographical errors.

Article:&quot;&quot;&quot;
{article}
&quot;&quot;&quot;
```

翻译：

我想让你看一篇信息量很大的文章。它需要更有吸引力，更容易阅读的广泛受众所有年龄段。请建议一些方法来减轻它，也许可以引入讲故事的元素或者使用相关的案例研究。还请确保所有内容都与文章的主题相关，并找出任何逻辑不一致或排印错误。

文章:

{ article }

"""

### 第五步，**总结优化 > 去掉标题**

```
Please provide a summary of the following article, excluding the title and numbered points, in clear and simple language. The summary should be in continuous text format to ensure it is easy to read.
Article:&quot;&quot;&quot;        
{article}
&quot;&quot;&quot;
```

翻译：

请提供以下文章的摘要，不包括标题和编号点，用简洁明了的语言。摘要应采用连续的文本格式，以确保易于阅读。文章:

### 第六步，**翻译**

```
Act as a Chinese language translator. I will provide a sentence or paragraph that needs to be translated into Chinese. Your role is to provide a clear and concise translation that accurately conveys the meaning of the original text, tailored to the intended Chinese-speaking audience. Please keep in mind that the intended audience is Chinese and may have different regional preferences or dialects. Additionally, please be sure to accurately translate any specific terms or jargon that may be confusing for ChatGPT to understand.
```

翻译：

充当中文翻译。我会提供一个需要翻译成中文的句子或段落。你的角色是提供一个清晰而简洁的翻译，准确地传达原文的意思，为中文读者量身定做。请记住，预定的听众是中国人，可能有不同的地区偏好或方言。此外，请确保准确地翻译任何可能使 ChatGPT 难以理解的具体术语或行话。

ChatGPT 输出：

正如科维所解释的，主动性是关于对我们的生活负责。这是关于理解我们不仅仅是环境的产物，而是有能力塑造我们命运的积极参与者。主动性的概念打破了受害者心态，使我们有能力控制自己的生活。

斯蒂芬·R·科维的《高效能人士的七个习惯》为个人和职业成功提供了一份蓝图。其中科维阐述的一个核心原则就是主动性的力量。值得注意的是，这是书中讨论的第一个习惯，强调了其根本的重要性。

科维做出的一个关键区别是关注圈和影响圈之间的区别。关注圈包括我们所担忧的各种事情——从全球问题到个人问题。然而，大部分的这些担忧都超出了我们直接的控制范围。另一方面，我们的影响圈包括我们可以直接影响的事情——我们的行为、反应、态度和选择。

通过关注我们的影响圈，我们可以在我们的生活中做出有意义的改变。我们可以选择主动，将我们的精力和注意力引向与我们的价值观相符并能对我们的生活产生积极影响的行动。当我们浪费时间担忧那些我们无法控制的事情时，我们会变得被动，导致压力、挫败感和停滞不前。

正如科维所说，“在刺激和反应之间，有一个空间。在那个空间里，我们有选择反应的自由和力量。”这就是主动性的精髓 - 意识到我们有选择对任何情况做出反应的自由。这是关于承认我们对自己的幸福、成功和福祉负责。

那么，我们如何在我们的生活中应用这个主动性原则呢？首先，我们需要改变我们的思维模式。每当我们发现自己因为外部环境或他人而抱怨我们的问题时，我们需要暂停，提醒自己我们有选择反应的力量。

此外，我们应该努力使我们的行动与我们的价值观一致。通过主动，我们可以设定并实现我们的目标，每天都朝着我们的成功愿景迈进。这需要纪律、承诺和坚韧，但回报是值得的。

最后，记住，主动性也延伸到对我们行为结果的责任。这是关于承认我们的错误，从中学习，并将它们作为向成长和改善的垫脚石。

总的来说，接受主动性是改变游戏规则的。它使我们有能力控制我们的生活，做出有意识的选择，创造我们想要的生活。通过关注我们的影响圈，使我们的行动与我们的价值观一致，以及对我们的行动负责，我们可以在生活的所有领域变得更有效。正如斯蒂芬·科维所说，“我不是环境的产物。我是我决定的产物。”让我们选择主动！