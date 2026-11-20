
> 以下是向客户推荐电影的代理程序。代理负责从全球热门电影中推荐电影。它应该避免询问用户的偏好并避免询问个人信息。如果代理没有电影推荐，它应该回答“抱歉，今天找不到电影推荐。”。

```text
顾客：请根据我的兴趣推荐一部电影。
客服：
```

输出：

> 抱歉，我没有关于您兴趣的任何信息。不过，这是目前全球热门的电影列表：[电影列表]。希望您能找到喜欢的电影！

## 提示词示例

下面，我们将通过示例介绍说明如何使用精细的提示词来执行不同类型的任务。

### 文本概括

自然语言生成的标准任务之一是文本概括。文本概括可能涉及到不同的风格和领域。事实上，语言模型最前景的应用场景之一就是能够快速概括出易于理解的文章大意和相关概念。我们可以使用提示词尝试一个简单的概括任务。

假设我想了解抗生素的相关信息，我可以给出这样的提示：

提示：

> Explain antibioticsA:

输出：

> Antibiotics are a type of medication used to treat bacterial infections. They work by either killing the bacteria or preventing them from reproducing, allowing the body’s immune system to fight off the infection. Antibiotics are usually taken orally in the form of pills, capsules, or liquid solutions, or sometimes administered intravenously. They are not effective against viral infections, and using them inappropriately can lead to antibiotic resistance.

在问答形式中，“A:” 是一种明确的提示格式。在这个示例中，我用它去提示模型，我想要该概念的进一步解释。在这个例子中，我们可能还不清楚使用它是否有用，我们会在之后的示例中探讨这一点。现在假设我们感觉模型给了太多的信息，想要进一步提炼它。我们可以指导模型帮我们用一句话总结相关内容：

提示：

> Antibiotics are a type of medication used to treat bacterial infections. They work by either killing the bacteria or preventing them from reproducing, allowing the body’s immune system to fight off the infection. Antibiotics are usually taken orally in the form of pills, capsules, or liquid solutions, or sometimes administered intravenously. They are not effective against viral infections, and using them inappropriately can lead to antibiotic resistance.Explain the above in one sentence: // 用一句话解释上面的信息：

输出：

> Antibiotics are medications used to treat bacterial infections by either killing the bacteria or stopping them from reproducing, but they are not effective against viruses and overuse can lead to antibiotic resistance.

本示例是模型在没有过多关注上文输出内容的准确性的情况下，尝试用一个句子来总结段落内容。关于上文准确性，我们可以通过指令或说明进一步改善它，这一点我们会在后续指南中进行探讨。读到这里，您可以暂时停住并进行实验，看看是否能获得更好的结果。

### 信息提取

语言模型通过训练不仅可以用于执行自然语言生成相关任务，还可以用于执行文本分类和其他一系列自然语言处理 (NLP) 任务。

使用以下示例提示词从指定段落中提取信息：

提示：

> Author-contribution statements and acknowledgements in research papers should state clearly and specifically whether, and to what extent, the authors used AI technologies such as ChatGPT in the preparation of their manuscript and analysis. They should also indicate which LLMs were used. This will alert editors and reviewers to scrutinize manuscripts more carefully for potential biases, inaccuracies and improper source crediting. Likewise, scientific journals should be transparent about their use of LLMs, for example when selecting submitted manuscripts.Mention the large language model based product mentioned in the paragraph above: // 指出上文中提到的大语言模型：

输出：

> The large language model based product mentioned in the paragraph above is ChatGPT.

我们可以通过许多方式改进以上结果，但当前方式已经非常有用。

现在应该很明显，您可以通过简单地指示语言模型去执行不同的任务。AI 研发人员也利用这种能力来构建强大的产品和体验。

### 问答

提高模型响应精确度的最佳方法之一是改进提示词的格式。如前所述，提示词可以通过指令、上下文、输入和输出指示以改进响应结果。虽然这些要素不是必需的，但如果您的指示越明确，响应的结果就会越好。以下示例可以说明结构化提示词的重要性。

提示：

> Answer the question based on the context below. Keep the answer short and concise. Respond "Unsure about answer" if not sure about the answer. // 基于以下语境回答问题。如果不知道答案的话，请回答“不确定答案”。Context: Teplizumab traces its roots to a New Jersey drug company called Ortho Pharmaceutical. There, scientists generated an early version of the antibody, dubbed OKT3. Originally sourced from mice, the molecule was able to bind to the surface of T cells and limit their cell-killing potential. In 1986, it was approved to help prevent organ rejection after kidney transplants, making it the first therapeutic antibody allowed for human use.Question: What was OKT3 originally sourced from?Answer:

输出：

> Mice.

### 文本分类

目前，我们已经会使用简单的指令来执行任务。作为提示工程师，您需要提供更好的指令。此外，您也会发现，对于更负责的使用场景，仅提供指令是远远不够的。所以，您需要思考如何在提示词中包含相关语境和其他不同要素。同样，你还可以提供其他的信息，如输入数据和示例。

可以通过以下示例体验文本分类：

提示：

> Classify the text into neutral, negative or positive. // 将文本按中立、负面或正面进行分类Text: I think the food was okay. Sentiment:

输出：

> Neutral

我们给出了对文本进行分类的指令，语言模型做出了正确响应，判断文本类型为 'Neutral'如果我们想要语言模型以指定格式作出相应，比如，我们想要它返回 neutral 而不是 Neutral ，那我们要如何做呢？我们有多种方法可以实现这一点。此例中，我们主要是关注绝对特性，因此，我们提示词中包含的信息越多，响应结果就会越好。我们可以使用以下示例来校正响应结果：

提示：

> Classify the text into neutral, negative or positive. Text: I think the vacation is okay.Sentiment: neutral Text: I think the food was okay. Sentiment:

输出：

> neutral

完美！这次模型返回了 neutral ，这正是我们想要的特定标签。提示词中的示例使得模型可以给出更具体的响应。有时给出具体的指令十分重要，可以通过以下示例感受这一点：

提示：

> Classify the text into nutral, negative or positive. Text: I think the vacation is okay.Sentiment:

输出：

> Neutral

这时候你知道给出具体指令的重要性了吧？

### 对话

你可以通过提示工程进行更有趣的实验，比如指导大语言模型系统如何表现，指定它的行为意图和身份。如果你正在构建客服聊天机器人之类的对话系统时，这项功能尤其有用。

比如，可以通过以下示例创建一个对话系统，该系统能够基于问题给出技术性和科学的回答。你可以关注我们是如何通过指令明确地告诉模型应该如何表现。这种应用场景有时也被称为_角色提示（Role Prompting）。

提示：

> The following is a conversation with an AI research assistant. The assistant tone is technical and scientific. // 以下是与 AI 助理的对话，语气应该专业、技术性强。Human: Hello, who are you?AI: Greeting! I am an AI research assistant. How can I help you today?Human: Can you tell me about the creation of blackholes?AI:

输出：

> Sure! Black holes are regions of spacetime where the gravitational force is so strong that nothing, not even light, can escape from it. They are created when a very massive star dies and its core collapses in on itself, forming a singularity of infinite density. The intense gravity of the singularity pulls in all the matter and radiation around it, creating the black hole.

我们的 AI 助理给出的回答非常技术对吧？下面，我们让它给出更易于理解的答案。

提示：

> The following is a conversation with an AI research assistant. The assistant answers should be easy to understand even by primary school students. // 以下是与 AI 助理的对话。请给出易于理解的答案，最好是小学生都能看懂的那种。Human: Hello, who are you?AI: Greeting! I am an AI research assistant. How can I help you today?Human: Can you tell me about the creation of black holes?AI:

输出：

> Sure! Black holes are created when a very massive star runs out of fuel and collapses in on itself. The gravity of the star is so strong that nothing, not even light, can escape its pull. This creates a point in space-time with an intense gravitational force called a black hole.

如果你再添加更多的示例，可能会得到更好的结果。

### 代码生成

大语言模型另外一个有效的应用场景是代码生成。在此方面，Copilot 就是一个很好的示例。你可以通过一些有效的提示词执行代码生成任务。让我们来看一下下面的例子。

我们先用它写个简单的用户欢迎程序：

提示：

> /*询问用户的姓名并说“ Hello”*/

输出：

```
let name = prompt("What is your name?");
console.log(Hello, ${name}! );
```

你看，我们甚至都不需要指定要使用的编程语言。

来，我们再稍微升级一下。下面的例子会向你展示提示词会让大语言模型变得多么强大。

提示：

> """Table departments, columns = [`DepartmentId`, `DepartmentName`]
> Table students, columns = [`DepartmentId`, `StudentId`, `StudentName`]
> Create a MySQL query for all students in the Computer Science Department"""

输出：

```
SELECT StudentId, StudentName FROM students WHERE DepartmentId IN (SELECT DepartmentId FROM departments WHERE DepartmentName = 'Computer Science');
```

挺厉害的嘛。本例中，我们提供了有关数据库架构并要求它生成有效的 MySQL 查询。

### 推理

目前对于大语言模型来说，推理任务算是最具有挑战性的了。推理任务最让人兴奋的地方就是可以促使各种复杂的应用程序从大语言模型中诞生。

目前，涉及数学能力的推理任务已经有了一些改进。对于当前的大型语言模型来说，执行推理任务可能会有一些难度，因此就需要更高级的提示词工程技术。我们会在后面的指南中介绍这些高级技术。现在，我们将介绍几个基本示例来展示算术功能。

提示：

> What is 9,000 * 9,000?

输出：

> 81,000,000

来，我们加大难度：

提示：

> The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1. A:

输出：

> No, the odd numbers in this group add up to an odd number: 119.

这不对！我们试着用提示词去改进它：

提示：

> The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1. Solve by breaking the problem into steps. First, identify the odd numbers, add them, and indicate whether the result is odd or even.

输出：

> Odd numbers: 15, 5, 13, 7, 1Sum: 41 41 is an odd number.

好多了吧？顺便说一句，我试过几次，有时还是会失败。如果你可以用示例给模型说明一下，可能会获得更准确的结果。

在后面的学习中，我们将介绍更高级的提示工程概念和技术，以完成更困难任务。

[ChatGpt提示词工程](https://zl49so8lbq.feishu.cn/wiki/wikcnhvaEVVXD8TgdUvVno14FWg)

## 提示工程高阶

### 零样本提示

如今，经过大量数据训练并调整指令的LLM能够执行零样本任务。我们在前一节中尝试了一些零样本示例。以下是我们使用的一个示例：

提示：

> 将文本分类为中性、负面或正面。文本：我认为这次假期还可以。情感：

输出：

> 中性

请注意，在上面的提示中，我们没有向模型提供任何示例——这就是零样本能力的作用。

指令调整已被证明可以改善零样本学习。指令调整本质上是在通过指令描述的数据集上微调模型的概念。此外，RLHF（来自人类反馈的强化学习）已被采用以扩展指令调整，其中模型被调整以更好地适应人类偏好。这一最新发展推动了像ChatGPT这样的模型。我们将在接下来的章节中讨论所有这些方法和方法。

当零样本不起作用时，建议在提示中提供演示或示例，也就是少样本提示。

### 少样本提示

虽然大型语言模型展示了惊人的零样本能力，但在使用零样本设置时，它们在更复杂的任务上仍然表现不佳。少样本提示可以作为一种技术，以启用上下文学习，我们在提示中提供演示以引导模型实现更好的性能。演示作为后续示例的条件，我们希望模型生成响应。

让我们通过一个例子来演示少样本提示。在这个例子中，任务是在句子中正确使用一个新词。

提示：

> “whatpu”是坦桑尼亚的一种小型毛茸茸的动物。

> 一个使用whatpu这个词的句子的例子是：我们在非洲旅行时看到了这些非常可爱的whatpus。

> “farduddle”是指快速跳上跳下。

> 一个使用farduddle这个词的句子的例子是：

输出：

> 当我们赢得比赛时，我们都开始庆祝跳跃。

我们可以观察到，模型通过提供一个示例（即1-shot）已经学会了如何执行任务。对于更困难的任务，我们可以尝试增加演示（例如3-shot、5-shot、10-shot等）。

根据Min等人（2022）的研究结果，以下是在进行少样本学习时关于演示/范例的一些额外提示：

“标签空间和演示指定的输入文本的分布都很重要（无论标签是否对单个输入正确）”

使用的格式也对性能起着关键作用，即使只是使用随机标签，这也比没有标签好得多。

其他结果表明，从真实标签分布（而不是均匀分布）中选择随机标签也有帮助。

让我们尝试一些例子。让我们首先尝试一个随机标签的例子（意味着将标签Negative和Positive随机分配给输入）：

提示：

> 这太棒了！// Negative

> 这太糟糕了！// Positive

> 哇，那部电影太棒了！// Positive

> 多么可怕的节目！//

输出：

> Negative

即使标签已经随机化，我们仍然得到了正确的答案。请注意，我们还保留了格式，这也有助于。实际上，通过进一步的实验，我们发现我们正在尝试的新GPT模型甚至对随机格式也变得更加稳健。例如：

提示：

> Positive This is awesome!

> This is bad!

> Negative

> Wow that movie was rad!

> Positive

> What a horrible show! --

输出：

> Negative

上面的格式不一致，但模型仍然预测了正确的标签。我们必须进行更彻底的分析，以确认这是否适用于不同和更复杂的任务，包括提示的不同变体。

提供示例对解决某些任务很有用。当零样本提示和少样本提示不足时，这可能意味着模型学到的东西不足以在任务上表现良好。接下来，我们将讨论一种流行的提示技术，称为链式提示。

### 链式提示

链式思考（CoT）提示通过中间推理步骤实现了复杂的推理能力。您可以将其与少样本提示相结合，以获得更好的结果，以便在回答之前进行推理的更复杂的任务。

提示：

> 这组数中的奇数加起来是偶数：4、8、9、15、12、2、1。

> A：将所有奇数相加（9、15、1）得到25。答案为False。

> 这组数中的奇数加起来是偶数：17、10、19、4、8、12、24。

> A：将所有奇数相加（17、19）得到36。答案为True。

> 这组数中的奇数加起来是偶数：16、11、14、4、8、13、24。

> A：将所有奇数相加（11、13）得到24。答案为True。

> 这组数中的奇数加起来是偶数：17、9、10、12、13、4、2。

> A：将所有奇数相加（17、9、13）得到39。答案为False。

>

> 这组数中的奇数加起来是偶数：15、32、5、13、82、7、1。

> A：

输出：

> 将所有奇数相加（15、5、13、7、1）得到41。答案为False。

哇！我们可以看到在提供推理步骤时得到了完美的结果。实际上，我们可以通过提供更少的示例来解决此任务，即仅一个示例似乎就足够了：

提示：

> 这组数中的奇数加起来是偶数：4、8、9、15、12、2、1。

> A：将所有奇数相加（9、15、1）得到25。答案为False。

>

> 这组数中的奇数加起来是偶数：15、32、5、13、82、7、1。

> A：

输出：

> 将所有奇数相加（15、5、13、7、1）得到41。答案为False。

这是足够大的语言模型才会出现的新兴能力。

### 自我一致性

在提示工程中更高级的技术之一是自我一致性。由Wang等人（2022）提出，自我一致性旨在“替换链式思维提示中使用的天真贪婪解码方法”。其想法是通过少样本CoT采样多个不同的推理路径，并使用生成结果选择最一致的答案。这有助于提高CoT提示在涉及算术和常识推理的任务中的性能。

让我们尝试以下算术推理示例：

提示：

> 当我6岁时，我的妹妹是我的一半年龄。现在我70岁了，我的妹妹多大？

输出：

> 35

输出是错误的！我们如何通过自我一致性来改进这个问题？让我们试试。我们将使用Wang等人2022年的少量样本范例：

提示：

> Q：林中有15棵树。林业工人今天将在林中种树。完成后，将有21棵树。林业工人今天种了多少棵树？

> A：我们从15棵树开始。后来我们有21棵树。差异必须是他们种树的数量。因此，他们必须种了21-15 = 6棵树。答案是6。Q：停车场有3辆汽车，又来了2辆汽车，停车场有多少辆汽车？

> A：停车场已经有3辆汽车。又来了2辆。现在有3 + 2 = 5辆汽车。答案是5。

> Q：Leah有32块巧克力，她的姐姐有42块。如果他们吃了35块，他们总共还剩多少块？

> A：Leah有32块巧克力，Leah的姐姐有42块。这意味着最初有32 + 42 = 74块巧克力。已经吃了35块。因此，他们总共还剩74-35 = 39块巧克力。答案是39。

> Q：Jason有20个棒棒糖。他给Denny一些棒棒糖。现在Jason只有12个棒棒糖。Jason给Denny多少棒棒糖？

> A：Jason有20个棒棒糖。因为他现在只有12个，所以他必须把剩下的给Denny。他给Denny的棒棒糖数量必须是20-12 = 8个棒棒糖。答案是8。

> Q：Shawn有五个玩具。圣诞节，他从他的父母那里得到了两个玩具。他现在有多少个玩具？

> A：他有5个玩具。他从妈妈那里得到了2个，所以在那之后他有5 + 2 = 7个玩具。然后他从爸爸那里得到了2个，所以总共他有7 + 2 = 9个玩具。答案是9。

> Q：服务器房间里有9台计算机。从周一到周四，每天都会安装5台计算机。现在服务器房间里有多少台计算机？

> A：从周一到周四有4天。每天都添加了5台计算机。这意味着总共添加了4 * 5 =20台计算机。一开始有9台计算机，所以现在有9 + 20 = 29台计算机。答案是29。

> Q：Michael有58个高尔夫球。星期二，他丢失了23个高尔夫球。星期三，他又丢失了2个。星期三结束时他还剩多少个高尔夫球？

> A：Michael最初有58个球。星期二他丢失了23个，所以在那之后他有58-23 = 35个球。星期三他又丢失了2个，所以现在他有35-2 = 33个球。答案是33。

> Q：Olivia有23美元。她用每个3美元的价格买了五个百吉饼。她还剩多少钱？

> A：她用每个3美元的价格买了5个百吉饼。这意味着她花了5Q：当我6岁时，我的妹妹是我的一半年龄。现在我70岁了，我的妹妹多大？

> A：

输出1：

> 当我6岁时，我的妹妹是我的一半年龄，也就是3岁。现在我70岁了，所以她是70-3 = 67岁。答案是67。

输出2：

> 当叙述者6岁时，他的妹妹是他年龄的一半，也就是3岁。现在叙述者70岁了，他的妹妹应该是70-3 = 67岁。答案是67。

输出3：

> 当我6岁时，我的妹妹是我的一半年龄，也就是3岁。现在我70岁了，所以她是70/2 = 35岁。答案是35。

![](img/60bbfb9b692ca8aaa3ea445c8d919b21.png)

![](img/ee63ce0c34d381d37e350f6736d3baed.png)

大家可以用一致性去测试你目前在用的ai工具，看哪个是能够一次答对的。

### 生成知识提示

LLM继续得到改进，其中一种流行的技术是能够融合知识或信息，以帮助模型做出更准确的预测。

使用类似的思路，模型是否也可以在做出预测之前用于生成知识呢？这就是Liu等人2022的论文所尝试的——生成知识以作为提示的一部分。特别是，这对于常识推理等任务有多大帮助？

让我们尝试一个简单的提示：

提示：

> 高尔夫球的一部分是试图获得比其他人更高的得分。是或否？

输出：

> 是。

这种错误揭示了LLM在执行需要更多关于世界的知识的任务时的局限性。我们如何通过生成知识来改进呢？

首先，我们生成一些“知识”：

提示：

> 输入：希腊比墨西哥大。知识：希腊的面积约为131,957平方公里，而墨西哥的面积约为1,964,375平方公里，使墨西哥比希腊大了1,389%。输入：眼镜总是会起雾。知识：当你的汗水、呼吸和周围的湿度中的水蒸气落在冷的表面上，冷却并变成微小的液滴时，会在眼镜镜片上产生冷凝。你看到的是一层薄膜。你的镜片相对于你的呼吸会比较凉，尤其是当外面的空气很冷时。输入：鱼有思考能力。知识：鱼比它们看起来更聪明。在许多领域，如记忆力，它们的认知能力与或超过非人类灵长类动物等“更高级”的脊椎动物。鱼的长期记忆帮助它们跟踪复杂的社交关系。输入：一个人一生中吸烟很多香烟的常见影响是患肺癌的几率高于正常水平。知识：那些一生中平均每天吸烟不到一支香烟的人，患肺癌的风险是从不吸烟者的9倍。在每天吸烟1到10支香烟之间的人群中，死于肺癌的风险几乎是从不吸烟者的12倍。输入：一块石头和一颗卵石大小相同。知识：卵石是一种根据Udden-Wentworth沉积学尺度的颗粒大小为4到64毫米的岩屑。卵石通常被认为比颗粒（直径2到4毫米）大，比卵石（直径64到256毫米）小。输入：高尔夫球的一部分是试图获得比其他人更高的得分。知识：

知识1：

> 高尔夫球的目标是以最少的杆数打完一组洞。一轮高尔夫球比赛通常包括18个洞。每个洞在标准高尔夫球场上一轮只打一次。每个杆计为一分，总杆数用于确定比赛的获胜者。

知识2：

> 高尔夫球是一项精密的球杆运动，竞争选手（或高尔夫球手）使用许多类型的球杆，用最少的杆数将球打入球场上一系列洞中。目标是以最低的分数完成比赛，该分数是通过将每个洞上所用的杆数相加计算出来的。得分最低的选手赢得比赛。

我们使用Liu等人2022论文中提供的提示。

下一步是将知识整合并得出预测。我将问题重新格式化为QA格式，以指导答案格式。

提示：

> 问题：高尔夫球的一部分是试图获得比其他人更高的得分。是或否？知识：高尔夫球的目标是以最少的杆数打完一组洞。一轮高尔夫球比赛通常包括18个洞。每个洞在标准高尔夫球场上一轮只打一次。每个杆计为一分，总杆数用于确定比赛的获胜者。解释和答案：

答案1（置信度非常高）：

> 不是，高尔夫球的目标不是获得比其他人更高的得分。相反，目标是以最少的杆数打完一组洞。总杆数用于确定比赛的获胜者，而不是总得分。

> 问题：高尔夫球的一部分是试图获得比其他人更高的得分。是或否？知识：高尔夫球是一项精密的球杆运动，竞争选手（或高尔夫球手）使用许多类型的球杆，用最少的杆数将球打入球场上一系列洞中。目标是以最低的分数完成比赛，该分数是通过将每个洞上所用的杆数相加计算出来的。得分最低的选手赢得比赛。解释和答案：

答案2（置信度要低得多）：

> 是的，高尔夫球的一部分是试图获得比其他人更高的得分。每个选手都试图以最少的杆数打完一组洞。总杆数用于确定比赛的获胜者，而不是总得分。得分最低的选手赢得比赛。

这个例子发生了一些非常有趣的事情。在第一个答案中，模型非常自信，但在第二个答案中不太自信。我简化了过程以进行演示，但在得出最终答案时还有一些细节需要考虑。

提示工程本身还有很多值得我们去探索的内容。随着大模型的发展和进步，提示工程也会随之产生一些新的技术。对于我们来说，掌握模型原理，并且在提示工程方面不断实践，就能确保我们在不同的场景和任务中去充分发挥大模型的能力。最后，再推荐一下我们的知识星球，扫码加入，学习更多。

![](img/ba5a81637446457944180289908a565b.png)
