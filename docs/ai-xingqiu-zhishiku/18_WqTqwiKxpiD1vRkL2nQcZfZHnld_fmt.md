
# openai官方《提示词工程课》超详细中文笔记

> 来源：[https://ga67w23t0nn.feishu.cn/docx/PuULdQP3wojyZYxn157cnsDXnqe](https://ga67w23t0nn.feishu.cn/docx/PuULdQP3wojyZYxn157cnsDXnqe)

欢迎👏🏻 Star我们最新的用AI创建技能库的框架

吴恩达携手[OpenAI出了官方的提示词工程课](https://zhuanlan.zhihu.com/p/623672319) @吴恩达 @openai 内含代码，示例解说和视频。

吴恩达作为AI届的泰斗，我个人非常认可他对未来的判断，可参考：吴恩达来信：LLMs的美好未来

> 不要想偷懒，请不断学习!

请不要为在早期的苹果上花了1.99美元买了个手电筒应用就无比兴奋喜悦。作为人的价值，你要专注在使用ChatGPT构建领域内更复杂的应用上。不论你来自什么领域或行业。

面向人群：

- 稍微懂点python的小白。
- 不懂python也可以通过chatGPT快速自学。
- 想要拿来开箱就用的小白，劝退！

这门课的内容包括

1. 学习如何使用大型语言模型（LLM）快速构建新的强大应用程序。使用OpenAI API，您将能够快速构建学习创新和创造价值的能力。这些能力在以前由于成本高昂、技术含量高根本不可能。
2. 这门由Isa Fulford（OpenAI）和Andrew Ng（DeepLearning.AI）教授的短期课程将描述 LLM 的工作原理，提供快速工程的最佳实践，并展示 LLM API 如何在应用程序中用于各种任务，包括：

- 总结（例如，为简洁起见总结用户评论）
- 推断（例如，情感分类、主题提取）
- 转换文本（例如，翻译、拼写和语法更正）
- 扩展（例如，自动编写电子邮件）

此外，您将学习编写有效提示的两个关键原则，如何系统地设计好的提示，并学习构建自定义聊天机器人。

代码见：

这是吴恩达和openai 提示词工程师官方出品的《ChatGPT Prompt Engineering for Developers》课程，笔记制作人

顺便贴一个自己的提示词课程：[《AI职场提效·ChatGPT使用指南（一）》-俊民](https://l1jq12b4lyq.feishu.cn/docx/S4eCdssx3o9cZrxMNyJc2Ao5nzL)

![](img/941b314499271dd99e7662c8414a1033.png)

结尾有惊喜！！可以加入交流学习群哟(＾Ｕ＾)ノ~ＹＯ

## 简介 / Introduction

![](img/b8a1a350dff4504ae01314f2505b4ae9.png)

[prompt_eng_01_intro_v5.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FhmWbk2vQojPpkxx8e1cXlg2nyf)

### 教学目标

欢迎来到本课程，我们将为开发人员介绍 ChatGPT 提示工程。本课程由 Isa Fulford 教授和我一起授课。Isa Fulford 是 OpenAI 的技术团队成员，曾开发过受欢迎的 ChatGPT 检索插件，并且在教授人们如何在产品中使用 LLM 或 LLM 技术方面做出了很大贡献。她还参与编写了教授人们使用 Prompt 的 OpenAI cookbook。

互联网上有很多有关提示的材料，例如《30 prompts everyone has to know》之类的文章。这些文章主要集中在 ChatGPT Web 用户界面上，许多人在使用它执行特定的、通常是一次性的任务。但是，我认为 LLM 或大型语言模型作为开发人员的更强大功能是使用 API 调用到 LLM，以快速构建软件应用程序。我认为这方面还没有得到充分的重视。实际上，我们在 DeepLearning.AI 的姊妹公司 AI Fund 的团队一直在与许多初创公司合作，将这些技术应用于许多不同的应用程序上。看到 LLM API 能够让开发人员非常快速地构建应用程序，这真是令人兴奋。

在本课程中，我们将与您分享一些可能性以及如何实现它们的最佳实践。

### 指令LLM

在构建大型语言模型或LLM的开发过程中，通常有两种类型的LLM，即基础LLM和指令微调LLM。

+   基础LLM是根据文本训练数据进行训练的，通常是根据互联网和其他来源的大量数据进行训练，以预测下一个单词。例如，如果你以“从前有一只独角兽”作为提示，基础LLM可能会继续预测“生活在一个与所有独角兽朋友的神奇森林中”。但是，如果你以“法国的首都是什么”为提示，则基础LLM可能会根据互联网上的文章，将答案预测为“法国最大的城市是什么？法国的人口是多少？”，因为互联网上的文章很可能是有关法国国家的问答题目列表。

+   而指令微调LLM是根据指令进行训练的。我们通常会从一个已经过训练的基础LLM开始，然后进一步用输入和输出来调整它，使其能够遵循指令，并通过人类反馈来进一步优化。因此，如果你问它，“法国的首都是什么？”，它更有可能输出“法国的首都是巴黎”。指令微调的 LLMs 的训练通常是从已经训练好的基本 LLMs 开始，该模型已经在大量文本数据上进行了训练。然后，使用输入是指令、输出是其应该返回的结果的数据集来对其进行微调，要求它遵循这些指令。然后通常使用一种称为 RLHF（reinforcement learning from human feedback，人类反馈强化学习）的技术进行进一步改进，使系统更能够有帮助地遵循指令。

因为指令微调的 LLMs 已经被训练成有益、诚实和无害的，所以与基础LLMs相比，它们更不可能输出有问题的文本，如有害输出。许多实际使用场景已经转向指令微调的LLMs。您在互联网上找到的一些最佳实践可能更适用于基础LLMs，但对于今天的大多数实际应用，我们建议将注意力集中在指令微调的LLMs上，这些LLMs更容易使用，而且由于OpenAI和其他LLM公司的工作，它们变得更加安全和更加协调。

因此，本课程将重点介绍针对指令微调 LLM 的最佳实践，这是我们建议您用于大多数应用程序的。

当您使用指令微调 LLM 时，请类似于考虑向另一个人提供指令，假设它是一个聪明但不知道您任务的具体细节的人。当 LLM 无法正常工作时，有时是因为指令不够清晰。例如，如果您说“请为我写一些关于阿兰·图灵的东西”，清楚表明您希望文本专注于他的科学工作、个人生活、历史角色或其他方面可能会更有帮助。更多的，您还可以指定文本采取像专业记者写作的语调，或者更像是您向朋友写的随笔。

当然，如果你想象一下让一位新毕业的大学生为你完成这个任务，你甚至可以提前指定他们应该阅读哪些文本片段来写关于 Alan Turing的文本，那么这能够帮助这位新毕业的大学生更好地成功完成这项任务。下一章你会看到如何让提示清晰明确，创建提示的一个重要原则，你还会从提示的第二个原则中学到给LLM时间去思考。

我们希望这能激发您构建新应用程序的想象力。

## 提示指南 / Guidelines

![](img/6ff5428cc8aa1a9ea8a9a353054f7c19.png)

[prompt_eng_02_guidelines.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZEnubv2jvoH5iUx51ePcnmCznOb)

在这个视频中，Isa将介绍一些提示准则，以帮助您获得想要的结果。具体而言，她将讨论两个关键原则，以有效地编写提示工程师。稍后，当她讲解Jupyter笔记本示例时，我鼓励您随时暂停视频以自行运行代码，以便您可以看到输出结果，并尝试更改确切的提示并玩一些不同的变化，以获取提示输入和输出的经验。

所以我将概述一些原则和策略，在使用ChatGPT等语言模型时会有所帮助。我首先会在高层次上概述这些内容，然后再通过实例应用具体的策略。我们将在整个课程中使用这些相同的策略。

> **第一个原则：**编写明确和具体的指令。

> **第二个原则：**给模型足够的时间思考。

### 环境准备

在我们开始之前，我们需要进行一些设置。

在整个课程中，我们将使用OpenAI Python库来访问OpenAI API。

如果您还没有安装此Python库，您可以使用`PIP`进行安装，如下所示：

```
pip install openai
```

我已经安装了此包，因此不会执行此操作。然后，您需要导入OpenAI并设置您的OpenAI API密钥，它是一个秘密密钥。您可以从OpenAI网站获取其中一个API密钥。然后您只需像这样设置您的API密钥，并替换为您自己的API密钥。如果您想要，也可以将其设置为环境变量。

```
import openai
import os

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv())

openai.api_key  = os.getenv('OPENAI_API_KEY')
```

在本课程中，您不需要执行任何此操作。您只需运行此代码，因为我们已经在环境中设置了API密钥。我会将其复制。不要担心这是如何工作的。

在整个课程中，我们将使用OpenAI的ChatGPT模型，`gpt-3.5-turbo`。

### 帮助函数

现在，我们将定义此帮助函数，以便更轻松地使用提示并查看生成的输出。函数`getCompletion`接收提示并返回该提示的完成结果。

```
def get_completion(prompt, model="gpt-3.5-turbo"):
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=0, # this is the degree of randomness of the model's output
    )
    return response.choices[0].message["content"]
```

现在让我们深入了解我们的第一个原则，即编写明确和具体的指令。您应该通过提供尽可能清晰和具体的指令来表达您想要模型执行的任务。这将引导模型朝着所需的输出方向，并减少获取无关或不正确响应的机会。

> 清晰 ≠ 简短

不要混淆清晰的提示与简短的提示，因为在许多情况下，更长的提示实际上提供了更多的清晰度和上下文，这实际上可以导致更详细和相关的输出。帮助您编写清晰和具体指令的第一个策略是使用分隔符清晰地指示输入的不同部分。

### 原则一：明确具体的指令

#### 技巧1：使用区分符

这段视频中，Isa向我们介绍了两个提示编写原则，以帮助我们获得所需的结果。首先，我们应该写出明确和具体的指令，以引导模型生成我们需要的输出，减少获取无关或错误响应的可能性。与此相关的一个策略是使用分隔符，明确指示输入的不同部分。这些分隔符可以是任何明确的标点符号，例如三个反引号，引号，XML标记，部分标题等，让模型清楚地知道哪些是独立的部分，以避免提示注入。

> 提示注入（Prompt Injection）是指用户添加输入到提示中，可能会与我们的指令相矛盾，导致模型遵循用户的指令而不是我们的指令。

示例中，我们要求模型将由三个反引号括起来的文本总结成一个句子，以清晰地指示模型需要处理哪些文本部分。如果没有分隔符，用户可能会添加不相关的输入，导致模型输出错误的结果。因此，使用分隔符可以提高模型的准确性和稳定性。

+   区分符可以是任何符号，比如 `` ``` ``、`"""`、`< >`、`<tag> </tag>`、`:`

````
text = f"""
You should express what you want a model to do by \ 
providing instructions that are as clear and \ 
specific as you can possibly make them. \ 
This will guide the model towards the desired output, \ 
and reduce the chances of receiving irrelevant \ 
or incorrect responses. Don't confuse writing a \ 
clear prompt with writing a short prompt. \ 
In many cases, longer prompts provide more clarity \ 
and context for the model, which can lead to \ 
more detailed and relevant outputs.
"""

prompt = f"""
Summarize the text delimited by triple backticks \ 
into a single sentence.

```{text}```
"""
response = get_completion(prompt)
print(response)
"""
To guide a model towards the desired output and reduce the chances of irrelevant or incorrect responses, it is important to provide clear and specific instructions, which may require longer prompts for more clarity and context.
"""
````

#### 技巧2：结构化输出

下一个策略是要求结构化输出。为了更容易解析模型输出，要求模型以HTML或JSON等结构化格式提供输出可能是有帮助的。在示例中，我们要求模型生成三本虚构图书的书名、作者和类型，并使用JSON格式以特定键的形式提供输出。我们可以看到，我们得到了三个格式良好的虚构图书的标题，并以JSON结构化输出的形式呈现出来。这种输出方式的好处是，我们可以在Python中将其读入字典或列表中。

```
prompt = f"""
Generate a list of three made-up book titles along \ 
with their authors and genres. 
Provide them in JSON format with the following keys: 
book_id, title, author, genre.
"""
response = get_completion(prompt)
print(response)
"""
[
  {
    "book_id": 1,
    "title": "The Lost City of Zorath",
    "author": "Aria Blackwood",
    "genre": "Fantasy"
  },
  {
    "book_id": 2,
    "title": "The Last Survivors",
    "author": "Ethan Stone",
    "genre": "Science Fiction"
  },
  {
    "book_id": 3,
    "title": "The Secret Life of Bees",
    "author": "Lila Rose",
    "genre": "Romance"
  }
]
"""
```

#### 技巧3：条件是否满足

下一个策略是让模型检查条件是否被满足。如果任务有一些假设并不一定满足，我们可以告诉模型先检查这些假设，如果不满足，则指出并停止任务完成的尝试。你也可以考虑潜在的边界情况以及模型应该如何处理它们，以避免意外错误或结果。在这个例子中，我们给出了一段描述泡茶步骤的段落，然后我们用一个 prompt 让模型提取这些指令。如果它在文本中找不到任何指令，我们让它输出`no steps provided`。在第二个段落中，模型判断没有找到任何指令。




```
text_1 = f&quot;&quot;&quot;
Making a cup of tea is easy! First, you need to get some \ 
water boiling. While that&#x27;s happening, \ 
grab a cup and put a tea bag in it. Once the water is \ 
hot enough, just pour it over the tea bag. \ 
Let it sit for a bit so the tea can steep. After a \ 
few minutes, take out the tea bag. If you \ 
like, you can add some sugar or milk to taste. \ 
And that&#x27;s it! You&#x27;ve got yourself a delicious \ 
cup of tea to enjoy.
&quot;&quot;&quot;
prompt = f&quot;&quot;&quot;
You will be provided with text delimited by triple quotes. 
If it contains a sequence of instructions, \ 
re-write those instructions in the following format:

Step 1 - ...
Step 2 - …
…
Step N - …

If the text does not contain a sequence of instructions, \ 
then simply write \&quot;No steps provided.\&quot;

\&quot;\&quot;\&quot;{text_1}\&quot;\&quot;\&quot;
&quot;&quot;&quot;
response = get_completion(prompt)
print(&quot;Completion for Text 1:&quot;)
print(response)
Completion for Text 1:
Step 1 - Get some water boiling.
Step 2 - Grab a cup and put a tea bag in it.
Step 3 - Once the water is hot enough, pour it over the tea bag.
Step 4 - Let it sit for a bit so the tea can steep.
Step 5 - After a few minutes, take out the tea bag.
Step 6 - Add some sugar or milk to taste.
Step 7 - Enjoy your delicious cup of tea!
```

```
text_2 = f&quot;&quot;&quot;
The sun is shining brightly today, and the birds are \
singing. It&#x27;s a beautiful day to go for a \ 
walk in the park. The flowers are blooming, and the \ 
trees are swaying gently in the breeze. People \ 
are out and about, enjoying the lovely weather. \ 
Some are having picnics, while others are playing \ 
games or simply relaxing on the grass. It&#x27;s a \ 
perfect day to spend time outdoors and appreciate the \ 
beauty of nature.
&quot;&quot;&quot;
prompt = f&quot;&quot;&quot;
You will be provided with text delimited by triple quotes. 
If it contains a sequence of instructions, \ 
re-write those instructions in the following format:

Step 1 - ...
Step 2 - …
…
Step N - …

If the text does not contain a sequence of instructions, \ 
then simply write \&quot;No steps provided.\&quot;

\&quot;\&quot;\&quot;{text_2}\&quot;\&quot;\&quot;
&quot;&quot;&quot;
response = get_completion(prompt)
print(&quot;Completion for Text 2:&quot;)
print(response)
&quot;&quot;&quot;
Completion for Text 2:
No steps provided.
&quot;&quot;&quot;
```

### 技巧4：少样本提示

这个原则的最后一个策略是 few-shot prompting。这种方法是在让模型执行实际任务之前，提供已经成功执行所需任务的示例。在提示中，我们告诉模型其任务是以一致的风格回答问题，然后提供了一个孩子和祖父母之间的对话作为示例。孩子说，“教我如何耐心等待”，祖父母用类比来回答。因为我们告诉模型要以一致的风格回答问题，所以当我们下一个要求“教我关于韧性”的时候，由于模型已经有了这个 few-shot 的示例，它会以类似的方式回答这个问题，比如“韧性就像一棵能够随着风摇曳却从不折断的树”，等等。

```
prompt = f&quot;&quot;&quot;
Your task is to answer in a consistent style.

&lt;child&gt;: Teach me about patience.

&lt;grandparent&gt;: The river that carves the deepest \ 
valley flows from a modest spring; the \ 
grandest symphony originates from a single note; \ 
the most intricate tapestry begins with a solitary thread.

&lt;child&gt;: Teach me about resilience.
&quot;&quot;&quot;
response = get_completion(prompt)
print(response)
&quot;&quot;&quot;
&lt;grandparent&gt;: Resilience is like a tree that bends with the wind but never breaks. It&#x27;s the ability to bounce back from adversity and keep moving forward, even when things get tough. Just like a tree needs strong roots to withstand the storm, we need to cultivate inner strength and perseverance to overcome life&#x27;s challenges.
&quot;&quot;&quot;
```

## 原则二：给模型思考时间

第二个原则是要给模型思考的时间。

如果模型在匆忙地做出错误的推断，你应该尝试重构查询，要求在模型提供最终答案之前进行一系列相关的推理。另一个思考这个问题的方式是，如果你给模型一个太复杂的任务，在短时间内或用少数的字数内完成，它可能会猜测结果，这很可能是不正确的。这也会发生在人类身上。如果你要求一个人在没有时间先计算答案的情况下完成一个复杂的数学问题，他们也很可能会犯错误。因此，在这些情况下，你可以要求模型花更多的时间思考问题，这意味着它在任务上花费更多的计算功夫。

### 技巧1：给定步骤来补全

首先，我们可以使用明确的步骤来完成一个任务。在这个例子中，我们给模型提供了一个包含Jack and Jill故事概述的段落，并且使用明确的步骤指示模型完成四个任务：

1. 首先，用一句话来概括文本
2. 其次将概述翻译成法语
3. 然后列出法语概述中的每个名称
4. 并且输出一个JSON对象包含"French summary"和"num names"两个key。

最后我们添加了这个段落的文本。运行这个模型后，我们可以看到模型分别完成了这四个任务，并按照我们要求的格式输出了结果。

````
text = f&quot;&quot;&quot;
In a charming village, siblings Jack and Jill set out on \ 
a quest to fetch water from a hilltop \ 
well. As they climbed, singing joyfully, misfortune \ 
struck—Jack tripped on a stone and tumbled \ 
down the hill, with Jill following suit. \ 
Though slightly battered, the pair returned home to \ 
comforting embraces. Despite the mishap, \ 
their adventurous spirits remained undimmed, and they \ 
continued exploring with delight.
&quot;&quot;&quot;
# example 1
prompt_1 = f&quot;&quot;&quot;
Perform the following actions: 
1 - Summarize the following text delimited by triple \
backticks with 1 sentence.
2 - Translate the summary into French.
3 - List each name in the French summary.
4 - Output a json object that contains the following \
keys: french_summary, num_names.

Separate your answers with line breaks.

Text:
```{text}```

&quot;&quot;&quot;

response = get_completion(prompt_1)

print(&quot;Completion for prompt 1:&quot;)

print(response)

&quot;&quot;&quot;

Completion for prompt 1:

Two siblings, Jack and Jill, go on a quest to fetch water from a well on a hilltop, but misfortune strikes and they both tumble down the hill, returning home slightly battered but with their adventurous spirits undimmed.

Deux frères et sœurs, Jack et Jill, partent en quête d&#x27;eau d&#x27;un puits sur une colline, mais un malheur frappe et ils tombent tous les deux de la colline, rentrant chez eux légèrement meurtris mais avec leurs esprits aventureux intacts. 

Noms: Jack, Jill.

{

  &quot;french_summary&quot;: &quot;Deux frères et sœurs, Jack et Jill, partent en quête d&#x27;eau d&#x27;un puits sur une colline, mais un malheur frappe et ils tombent tous les deux de la colline, rentrant chez eux légèrement meurtris mais avec leurs esprits aventureux intacts.&quot;,

  &quot;num_names&quot;: 2

}

&quot;&quot;&quot;
````

现在我将向您展示另一个提示，以完成相同的任务。在这个提示中，我使用了一个我很喜欢使用的格式，来指定模型的输出结构。

因为正如您在这个示例中注意到的那样，这个名称的标题是法语的，这可能不是我们想要的。如果我们要传递这个输出，可能会有一些困难和不可预测性。因此，在这个提示中，我们要求类似的内容，即开始时要求相同的步骤，然后要求模型使用以下格式：文本、摘要、翻译、名称和输出JSON。

接着，我们只需输入要摘要的文本，或者只需说“`text`”。然后，我们开始运行代码。可以看到，这是完成的结果。模型使用了我们要求的格式。因此，有时这很好，因为它将更容易传递代码，因为它具有更加标准化的格式，可以更好地预测。

同时请注意，在这种情况下，我们使用了尖括号作为分隔符，而不是三个反引号。你可以选择任何对你来说有意义或对模型有意义的分隔符。

```
prompt_2 = f&quot;&quot;&quot;

Your task is to perform the following actions: 

1 - Summarize the following text delimited by 

  &lt;&gt; with 1 sentence.

2 - Translate the summary into French.

3 - List each name in the French summary.

4 - Output a json object that contains the 

  following keys: french_summary, num_names.

Use the following format:

Text: &lt;text to summarize&gt;

Summary: &lt;summary&gt;

Translation: &lt;summary translation&gt;

Names: &lt;list of names in Italian summary&gt;

Output JSON: &lt;json with summary and num_names&gt;

Text: &lt;{text}&gt;

&quot;&quot;&quot;

response = get_completion(prompt_2)

print(&quot;\nCompletion for prompt 2:&quot;)

print(response)

&quot;&quot;&quot;

Completion for prompt 2:

Summary: Jack and Jill go on a quest to fetch water, but misfortune strikes and they tumble down the hill, returning home slightly battered but with their adventurous spirits undimmed. 

Translation: Jack et Jill partent en quête d&#x27;eau, mais un malheur frappe et ils tombent de la colline, rentrant chez eux légèrement meurtris mais avec leurs esprits aventureux intacts.

Names: Jack, Jill

Output JSON: {&quot;french_summary&quot;: &quot;Jack et Jill partent en quête d&#x27;eau, mais un malheur frappe et ils tombent de la colline, rentrant chez eux légèrement meurtris mais avec leurs esprits aventureux intacts.&quot;, &quot;num_names&quot;: 2}

&quot;&quot;&quot;
```

### 技巧2：让模型先梳理再给结论

我们的下一个策略是指示模型在匆忙做出结论之前自行解决问题。有时，当我们明确指示模型在得出结论之前先理清事情的顺序时，我们会获得更好的结果。这与我们之前讨论的给模型时间来解决问题的想法是相同的，就像人一样，我们让它自己思考解决方案，而不是马上判断答案是否正确。

在这个问题中，我们要求模型判断学生的解答是否正确。首先，我们有这个数学问题，然后是学生的解答。

```
prompt = f&quot;&quot;&quot;
```



Determine if the student's solution is correct or not.

**Question:**

I'm building a solar power installation and I need help working out the financials.

- Land costs $100 / square foot
- I can buy solar panels for $250 / square foot
- I negotiated a contract for maintenance that will cost me a flat $100k per year, and an additional $10 / square foot

What is the total cost for the first year of operations as a function of the number of square feet.

**Student's Solution:**

Let `x` be the size of the installation in square feet.

Costs:

1. Land cost: `100x`
2. Solar panel cost: `250x`
3. Maintenance cost: `100,000 + 100x`

Total cost: `100x + 250x + 100,000 + 100x = 450x + 100,000`

```
"""
response = get_completion(prompt)
print(response)
"""
The student's solution is correct.
"""
```

事实上，学生的解答是错误的，因为他们计算了维护费用为 `100,000 + 100x`，但实际上应该是 `10x`，因为每平方英尺只有 `10` 美元，其中 `x` 是安装面积的大小（以平方英尺为单位）。因此，应该是 `360x + 100,000`，而不是 `450x`。

如果我们运行这个单元格代码，模型会说学生的解答是正确的。

如果您仔细阅读学生的解答，您会发现这实际上是错误的，因为模型只是粗略地阅读了它，就像我刚才所做的那样。

因此，我们可以通过指示模型先计算自己的解答，然后再将其与学生的解答进行比较来解决这个问题。我们可以通过一个更长的提示来实现这一点。我们告诉模型如下：

您的任务是确定学生的解答是否正确。要解决这个问题，请执行以下步骤：

1. 首先，计算出您自己的解答。
1. 然后将您的解答与学生的解答进行比较，并评估学生的解答是否正确。

在你自己解题之前，不要决定学生的解答是否正确。确保您自己解决了这个问题。

因此，我们使用了相同的格式，即问题、学生解答、实际解答，以及解答是否一致（是或否）和学生的成绩（正确或错误）。因此，我们有与上面相同的问题和解答。

````python
prompt = f"""
Your task is to determine if the student's solution \
is correct or not.

To solve the problem do the following:

- First, work out your own solution to the problem. 

- Then compare your solution to the student's solution \
and evaluate if the student's solution is correct or not. 

Don't decide if the student's solution is correct until 
you have done the problem yourself.

Use the following format:

Question:

```
question here
```

Student's solution:

```
student's solution here
```

Actual solution:

```
steps to work out the solution and your solution here
```

Is the student's solution the same as actual solution \
just calculated:

```
yes or no
```

Student grade:

```
correct or incorrect
```

Question:

```
I'm building a solar power installation and I need help \
working out the financials. 
- Land costs $100 / square foot
- I can buy solar panels for $250 / square foot
- I negotiated a contract for maintenance that will cost \
me a flat $100k per year, and an additional $10 / square \
foot
What is the total cost for the first year of operations \
as a function of the number of square feet.
``` 

Student's solution:

```
Let x be the size of the installation in square feet.
Costs:
1. Land cost: 100x
2. Solar panel cost: 250x
3. Maintenance cost: 100,000 + 100x
Total cost: 100x + 250x + 100,000 + 100x = 450x + 100,000
```

Actual solution:

"""
response = get_completion(prompt)
print(response)
"""

Let x be the size of the installation in square feet.

Costs:

1. Land cost: 100x
2. Solar panel cost: 250x
3. Maintenance cost: 100,000 + 10x
Total cost: 100x + 250x + 100,000 + 10x = 360x + 100,000

Is the student's solution the same as actual solution just calculated:

No

Student grade:

Incorrect
"""
````

那么现在，如果我们运行这个单元格……正如你看到的，这个模型实际上首先进行了自己的计算。然后，它找到了正确的答案，即 `360x + 100,000`，而不是 `450x + 100,000`。然后，当被要求将其与学生的解决方案进行比较时，它意识到它们不一致。因此，学生实际上是不正确的。这是一个例子，展示了学生的解决方案是正确的，但学生的解决方案实际上是错误的。这是一个例子，展示了让模型自己进行计算，并将任务分解为步骤，以便给模型更多的时间思考，可以帮助您获得更准确的响应。

## 局限：模型幻觉

接下来，我们将谈论一些模型的限制，因为我认为在使用大型语言模型开发应用程序时，牢记这些限制非常重要。如果模型在训练过程中接触到大量的知识，它并不是完美地记住了它看到的信息，因此它并不很好地知道它的知识边界。这意味着它可能会尝试回答关于晦涩话题的问题，并且可能会编造听起来合理但实际上不正确的内容。我们将这些编造的想法称为幻觉。

我将给您展示一个例子，其中模型会产生幻觉。这是一个例子，其中模型从一个真正的牙刷公司创造了一个虚构的产品名字。因此，提示是“告诉我关于 Boy 的 AeroGlide Ultra Slim Smart Toothbrush。”如果我们运行这个提示，模型将为我们提供一个相当逼真的虚构产品的描述。这种情况可能比较危险，因为这听起来非常真实。

因此，请确保在构建自己的应用程序时使用我们在这个笔记本中介绍的一些技巧，尽量避免这种情况。这是模型已知的弱点，我们正在积极努力解决。在文本中寻找相关引用，并要求模型使用这些引用来回答问题，并且有一种追溯答案到源文档的方法，通常可以减少这些幻觉。

```
"""

prompt = f"""

Tell me about AeroGlide UltraSlim Smart Toothbrush by Boie

"""

response = get_completion(prompt)

print(response)

"""

The AeroGlide UltraSlim Smart Toothbrush by Boie is a high-tech toothbrush that uses advanced sonic technology to provide a deep and thorough clean. It features a slim and sleek design that makes it easy to hold and maneuver, and it comes with a range of smart features that help you optimize your brushing routine.

One of the key features of the AeroGlide UltraSlim Smart Toothbrush is its advanced sonic technology, which uses high-frequency vibrations to break up plaque and bacteria on your teeth and gums. This technology is highly effective at removing even the toughest stains and buildup, leaving your teeth feeling clean and fresh.

In addition to its sonic technology, the AeroGlide UltraSlim Smart Toothbrush also comes with a range of smart features that help you optimize your brushing routine. These include a built-in timer that ensures you brush for the recommended two minutes, as well as a pressure sensor that alerts you if you're brushing too hard.

Overall, the AeroGlide UltraSlim Smart Toothbrush by Boie is a highly advanced and effective toothbrush that is perfect for anyone looking to take their oral hygiene to the next level. With its advanced sonic technology and smart features, it provides a deep and thorough clean that leaves your teeth feeling fresh and healthy.

"""
```

这就是提示指南的全部内容，接下来您将转到下一个视频，该视频将介绍迭代提示开发过程。

> 关于反斜杠的说明：
>
> 在本课程中，我们使用反斜杠\来使文本适应屏幕而不插入换行符'\n'字符。
>
> GPT-3实际上不受您是否插入换行符的影响。但是在使用LLM时，您可能需要考虑您的提示中的换行符是否会影响模型的性能。

# 迭代提示 / Iterative

![](img/e6fa9996f3cd8dff1b3176ed93d2f0e1.png)

[prompt_eng_03_iterative.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BrjObj4XdojNn7xIdcRcZxfQn5e)

当我使用大型语言模型构建应用程序时，我从来没有在第一次尝试中得到最终应用程序中使用的提示。但这并不重要。只要您有一个良好的迭代过程，使您的提示不断改进，那么您就能得到适合您想要实现的任务的提示。

你可能听我说过，当我训练一个机器学习模型时，它几乎从来没有在第一次尝试时成功。事实上，如果我训练的第一个模型成功了，我会感到非常惊讶。

在提示方面，它第一次成功的概率可能稍微高一些，但正如他所说的，第一个提示是否成功并不重要。最重要的是得到适用于您的应用程序的提示的过程。因此，让我们进入代码并展示一些框架，以思考如何迭代地开发提示。

如果您之前参加过我的机器学习课程，您可能会看到我使用一个图表，说在机器学习开发中，您经常有一个想法，然后实现它。编写代码，获取数据，训练模型，这就给您一个实验结果。然后您可以查看该输出，进行错误分析，找出哪些地方工作或不工作，然后甚至可以改变您要解决的问题或如何处理它的确切想法，并更改实现并运行另一个实验等，一遍又一遍地迭代，以获得有效的机器学习模型。

如果您对机器学习不熟悉，并且以前没有看过这个图表，请不要担心，对于本次演示的其余部分来说，这并不重要。但是，当您编写提示以使用 OOM 开发应用程序时，该过程可能非常相似，您可以有一个关于您想要完成的任务的想法，然后尝试编写第一个提示，希望它清晰具体，并可能在适当的情况下给系统一些时间来思考，然后运行它并查看结果。

如果第一次不能很好地工作，那么迭代过程就开始了，找出指令不够清晰或没有给算法足够的时间思考的原因，使您可以不断完善想法、完善提示等等，直到您得到适合您应用程序的提示。这也是为什么我个人没有太关注互联网上那些说30个完美提示的文章，因为我认为可能并不存在一个适用于所有情况的完美提示。更重要的是，您要有一个针对特定应用程序开发良好提示的流程。让我们一起看一个示例代码。

## 环境准备

这里我们获取了 Open AI API 密钥，这是上一次您看到的同一辅助函数。

```python
import openai
import os
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key  = os.getenv('OPENAI_API_KEY')
```

```python
def get_completion(prompt, model="gpt-3.5-turbo"):
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=0, # this is the degree of randomness of the model's output
    )
    return response.choices[0].message["content"]
```

## 案例1：生成产品描述

在这个视频中，我将使用一个生成产品描述的任务作为运行示例。让我把它粘贴在这里。如果需要，可以随时暂停视频，然后在左侧的笔记本中仔细阅读。

这是一个椅子的事实表，其中描述了它是一个美丽的中世纪灵感家族的一部分，讲述了构造，尺寸，椅子选项，材料等。它来自意大利。所以，假设您想要将这个事实表提供给营销团队，以帮助他们为在线零售网站撰写描述。

我将这样说明我的任务，然后粘贴到代码中。

```python
fact_sheet_chair = """

OVERVIEW

- Part of a beautiful family of mid-century inspired office furniture, 

including filing cabinets, desks, bookcases, meeting tables, and more.

- Several options of shell color and base finishes.

- Available with plastic back and front upholstery (SWC-100) 

or full upholstery (SWC-110) in 10 fabric and 6 leather options.

- Base finish options are: stainless steel, matte black, 

gloss white, or chrome.

- Chair is available with or without armrests.
"""
```




- Suitable for home or business settings.
- Qualified for contract use.

### CONSTRUCTION

- 5-wheel plastic coated aluminum base.
- Pneumatic chair adjust for easy raise/lower action.

### DIMENSIONS

- WIDTH 53 CM | 20.87”
- DEPTH 51 CM | 20.08”
- HEIGHT 80 CM | 31.50”
- SEAT HEIGHT 44 CM | 17.32”
- SEAT DEPTH 41 CM | 16.14”

### OPTIONS

- Soft or hard-floor caster options.
- Two choices of seat foam densities:  
  medium (1.8 lb/ft3) or high (2.8 lb/ft3)
- Armless or 8 position PU armrests

### MATERIALS

#### SHELL BASE GLIDER

- Cast Aluminum with modified nylon PA6/PA66 coating.
- Shell thickness: 10 mm.

#### SEAT

- HD36 foam

### COUNTRY OF ORIGIN

- Italy

"""

所以我的提示是：您的任务是帮助营销团队基于技术事实表为零售网站或产品创建描述，编写产品描述等。

这是我向大型语言模型解释任务的第一次尝试。让我按Shift + Enter，这需要几秒钟运行，我们得到了结果。

````
prompt = f"""
Your task is to help a marketing team create a 
description for a retail website of a product based 
on a technical fact sheet.

Write a product description based on the information 
provided in the technical specifications delimited by 
triple backticks.

Technical specifications: ```{fact_sheet_chair}```
"""
response = get_completion(prompt)
print(response)
"""
Introducing our stunning mid-century inspired office chair, the perfect addition to any home or business setting. Part of a beautiful family of office furniture, including filing cabinets, desks, bookcases, meeting tables, and more, this chair is available in several options of shell color and base finishes to suit your style. Choose from plastic back and front upholstery (SWC-100) or full upholstery (SWC-110) in 10 fabric and 6 leather options.

The chair is constructed with a 5-wheel plastic coated aluminum base and features a pneumatic chair adjust for easy raise/lower action. It is available with or without armrests and is qualified for contract use. The base finish options are stainless steel, matte black, gloss white, or chrome.

Measuring at a width of 53 cm, depth of 51 cm, and height of 80 cm, with a seat height of 44 cm and seat depth of 41 cm, this chair is designed for ultimate comfort. You can also choose between soft or hard-floor caster options and two choices of seat foam densities: medium (1.8 lb/ft3) or high (2.8 lb/ft3). The armrests are available in either an armless or 8 position PU option.

The materials used in the construction of this chair are of the highest quality. The shell base glider is made of cast aluminum with modified nylon PA6/PA66 coating and has a shell thickness of 10 mm. The seat is made of HD36 foam, ensuring maximum comfort and durability.

This chair is made in Italy and is the perfect combination of style and functionality. Upgrade your workspace with our mid-century inspired office chair today!
"""
````

看起来它已经很好地写了一个描述，介绍了一款令人惊叹的中世纪灵感办公椅，完美的版型等等。

### 问题1：生成的内容太长

但是当我看到这个结果时，我觉得，哇，这太长了。它确实做到了我要求它做的事情，也就是从技术事实表开始写产品描述。但是当我看到这个结果时，我觉得这有点长。也许我们希望它再短一点。

所以我想到了一个主意，我编写了一个提示并得到了结果。我对它不是很满意，因为它太长了，所以我会澄清我的提示，说使用最多50个单词来更好地指导所需长度，然后再次运行它。看起来这是一个更好的产品简短描述，介绍了一个中世纪灵感的办公椅，同时又时尚又实用。不错。

````
prompt = f"""
Your task is to help a marketing team create a 
description for a retail website of a product based 
on a technical fact sheet.

Write a product description based on the information 
provided in the technical specifications delimited by 
triple backticks.

Use at most 50 words.

Technical specifications: ```{fact_sheet_chair}```

"""

response = get_completion(prompt)

print(response)

"""
Introducing our mid-century inspired office chair, part of a beautiful furniture family. Available in various shell colors and base finishes, with plastic or full upholstery options in fabric or leather. Suitable for home or business use, with a 5-wheel base and pneumatic chair adjust. Made in Italy.
"""
````

让我再确认一下这个长度。我将获取响应，根据空格进行分割，然后你将打印出长度。所以有52个单词。其实还不错。大型语言模型在遵循非常精确的单词计数指令方面表现还可以，但并不是特别出色。但这个结果实际上还不错。有时会输出60或65个单词，但这还算是合理范围内的。有些事情你可以这样做。让我再次运行一下。但这些都是告诉大型语言模型您想要的输出长度的不同方法。所以这是一个、两个、三个。我数了这些句子。看起来我做得不错。我也看到有些人有时会像这样做，我不知道，使用至多280个字符。由于大型语言模型解释文本的方式，使用一种称为分词器的工具，因此它们在计算字符数方面表现一般。但是，281个字符。它实际上非常接近。通常，大型语言模型无法达到这种程度。但这些都是可以尝试的不同方式，以控制输出的长度。但接着，我们只需将其切换回最多使用50个单词即可。这就是我们刚才得到的结果。

![](img/a610d6ac22be5c8e5c9cd23a6001c9c4.png)

### 问题2：面向受众不对

当我们继续为我们的网站修改这段文本时，我们可能会决定，哇，这个网站不是直接销售给消费者，而是旨在向家具零售商销售家具，他们更感兴趣的是椅子的技术细节和材料。在这种情况下，您可以采用这个提示，并说，我要修改这个提示，使其更精确地描述技术细节。

因此，让我继续修改这个提示。我要说，这个描述是为家具零售商而设计的，因此应该是技术性的，重点放在材料、产品和构造方面。好的，让我们运行它。

````
prompt = f"""
Your task is to help a marketing team create a 
description for a retail website of a product based 
on a technical fact sheet.

Write a product description based on the information 
provided in the technical specifications delimited by 
triple backticks.

The description is intended for furniture retailers, 
so should be technical in nature and focus on the 
materials the product is constructed from.

Use at most 50 words.

Technical specifications: ```{fact_sheet_chair}```
"""
response = get_completion(prompt)
print(response)
"""
Introducing our mid-century inspired office chair, perfect for both home and business settings. With a range of shell colors and base finishes, including stainless steel and matte black, this chair is available with or without armrests. The 5-wheel plastic coated aluminum base and pneumatic chair adjust make it easy to move and adjust to your desired height. Made with high-quality materials, including a cast aluminum shell and HD36 foam seat, this chair is built to last.
"""
````

看起来不错。它说，镀铝底座和气动椅子。高质量材料。通过修改提示，您可以让它更专注于您想要的特定特征。当我看到这个时，我可能会决定，在描述的结尾处，我还想包括产品ID。因此，我可以进一步改进这个提示。为了让它给我产品ID，我可以在描述的结尾处添加这个指令，在技术规格中包括每个7个字符的产品ID。让我们运行它，看看会发生什么。

````
prompt = f"""
Your task is to help a marketing team create a 
description for a retail website of a product based 
on a technical fact sheet.

Write a product description based on the information 
provided in the technical specifications delimited by 
triple backticks.

The description is intended for furniture retailers, 
so should be technical in nature and focus on the 
materials the product is constructed from.

At the end of the description, include every 7-character 
Product ID in the technical specification.

Use at most 50 words.

Technical specifications: ```{fact_sheet_chair}```

"""

response = get_completion(prompt)

print(response)

"""

chair, perfect for home or business settings. With a range of shell colors and base finishes, and the option of plastic or full upholstery, this chair is both stylish and comfortable. Constructed with a 5-wheel plastic coated aluminum base and pneumatic chair adjust, it's also practical. Available with or without armrests and suitable for contract use. Product ID: SWC-100, SWC-110.

"""
````

于是它说，介绍我们的中世纪办公椅，壳颜色，讲述塑料涂层铝底座，实用，一些选项，讲述了两个产品ID。看起来不错。

您刚才所看到的是许多开发者将要经历的迭代提示开发的简短示例。我认为一个指南是，在上一个视频中，您看到Yisa分享了许多最佳实践。因此，我通常会牢记这些最佳实践，清晰明确地表达指令，如果需要，给模型一些时间去思考。有了这些想法，通常值得尝试首次编写提示，观察发生了什么，然后从那里开始迭代优化提示，以逐渐接近所需的结果。因此，许多成功的提示，您可能会在各种程序中看到，都是通过这样的迭代过程获得的。

### 问题3：用表格描述

让我们来玩一下，我想给你展示一个更复杂的提示示例，让你感受一下ChatGPT的能力。我在这里添加了一些额外的指令。在描述之后，包括一个给出产品尺寸的表格，然后将所有内容格式化为HTML。那么让我们运行它。

```
prompt = f"""
Your task is to help a marketing team create a 
description for a retail website of a product based 
on a technical fact sheet.

Write a product description based on the information 
provided in the technical specifications delimited by 
triple backticks.

The description is intended for furniture retailers, 
so should be technical in nature and focus on the 
materials the product is constructed from.

At the end of the description, include every 7-character 
Product ID in the technical specification.

After the description, include a table that gives the 
product's dimensions. The table should have two columns.

In the first column include the name of the dimension. 
```



In the second column include the measurements in inches only. Give the table the title 'Product Dimensions'. Format everything as HTML that can be used in a website. Place the description in a `<div>` element.

Technical specifications: `{fact_sheet_chair}`

```
"""
response = get_completion(prompt)
print(response)
```

```
"""
<div>
<h2>Mid-Century Inspired Office Chair</h2>
<p>Introducing our mid-century inspired office chair, part of a beautiful family of office furniture that includes filing cabinets, desks, bookcases, meeting tables, and more. This chair is available in several options of shell color and base finishes, allowing you to customize it to your liking. You can choose between plastic back and front upholstery or full upholstery in 10 fabric and 6 leather options. The base finish options are stainless steel, matte black, gloss white, or chrome. The chair is also available with or without armrests, making it suitable for both home and business settings. Plus, it's qualified for contract use, ensuring its durability and longevity.</p>
<p>The chair's construction features a 5-wheel plastic coated aluminum base and a pneumatic chair adjust for easy raise/lower action. You can also choose between soft or hard-floor caster options and two choices of seat foam densities: medium (1.8 lb/ft3) or high (2.8 lb/ft3). The armrests are also customizable, with the option of armless or 8 position PU armrests.</p>
<p>The materials used in the chair's construction are of the highest quality. The shell base glider is made of cast aluminum with modified nylon PA6/PA66 coating, with a shell thickness of 10 mm. The seat is made of HD36 foam, ensuring maximum comfort and support.</p>
<p>Made in Italy, this mid-century inspired office chair is the perfect addition to any office space. Order yours today!</p>
<h3>Product IDs:</h3>
<ul>
<li>SWC-100</li>
<li>SWC-110</li>
</ul>
</div>

<table>
  <caption>Product Dimensions</caption>
  <tr>
    <th>Dimension</th>
    <th>Measurement (inches)</th>
  </tr>
  <tr>
    <td>Width</td>
    <td>20.87"</td>
  </tr>
  <tr>
    <td>Depth</td>
    <td>20.08"</td>
  </tr>
  <tr>
    <td>Height</td>
    <td>31.50"</td>
  </tr>
  <tr>
    <td>Seat Height</td>
    <td>17.32"</td>
  </tr>
  <tr>
    <td>Seat Depth</td>
    <td>16.14"</td>
  </tr>
</table>
"""
```

在实践中，你最终会得到像这样的提示，这通常需要多次迭代才能完成。我不认为有人在尝试让系统处理一份事实表格时会一开始就写出这个确切的提示。因此，这实际上输出了一堆HTML代码。让我们显示HTML，看看它是否是有效的HTML，看看它是否可行。我并不确定它是否可行，但让我们看看。

![](img/e6aebb8311de66aa6b30020c5ee0ab9a.png)

哦，很酷。好的，看起来像是一个渲染图。它有一个非常漂亮的椅子描述，包括构造、材料和产品尺寸。

哦，看起来我忘记了最多使用50个单词的指令，所以这有点长，但是如果您需要的话，可以随时暂停视频，让它变得更简洁，并重新生成，看看结果如何。

因此，我希望您从本视频中了解到，提示开发是一个迭代的过程。尝试一些东西，看看它还不能完全满足您的要求，然后考虑如何澄清您的指令，或者在某些情况下，考虑给它更多的思考空间，以使其更接近于产生您想要的结果。

我认为成为有效的提示工程师的关键不是知道完美的提示，而是拥有一个良好的过程来开发对您的应用程序有效的提示。

在本视频中，我只举了一个例子来说明如何开发提示。对于更复杂的应用程序，有时您会有多个例子，比如10个、50个或100个事实表，并逐步开发和对大量案例进行评估。

但是对于大多数应用程序的早期开发，我看到很多人会像我一样只使用一个示例进行开发，但是对于更成熟的应用程序，有时对多个示例进行提示评估可能是有用的，比如在多个事实表上测试不同的提示，以查看平均或最差情况的性能如何。

但通常只有当应用程序更成熟时，您才需要这些指标来驱动提示改进的最后几个增量步骤。因此，请尝试使用Jupyter代码笔记本示例，并尝试不同的变体，看看您得到的结果如何。完成后，请继续下一个视频，我们将讨论大型语言模型在软件应用程序中非常常见的一种用途，即对文本进行摘要。

# 文本摘要 / Summarizing

[`prompt_eng_04_summarizing.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IkNTbw0IHoOPLzxJsv1cYvb7nPd)

当今世界上有太多的文字，几乎没有人有足够的时间去阅读我们希望有时间阅读的所有内容。因此，我看到的最令人兴奋的大型语言模型的应用之一是使用它来概括文本。

我看到多个团队将其集成到多个软件应用程序中。您可以在Chat GPT Web界面上完成这项工作。我经常使用它来概括文章，这样我就可以阅读比以前更多的文章内容。

如果您想以编程方式进行此操作，您将在本课程中了解到如何操作。因此，让我们深入了解代码，看看如何使用它来概括文本。

## 环境准备

因此，让我们从导入OpenAI、加载API密钥以及这个`getCompletion`辅助函数的相同的起始代码开始。

```
import openai
import os

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file

openai.api_key  = os.getenv('OPENAI_API_KEY')
```

```
def get_completion(prompt, model="gpt-3.5-turbo"): # Andrew mentioned that the prompt/ completion paradigm is preferable for this class
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=0, # this is the degree of randomness of the model's output
    )
    return response.choices[0].message["content"]
```

## 字数约束

我将使用“总结此产品评论”的示例来运行代码。我从女儿的生日上得到了这个熊猫毛绒玩具，她很喜欢，随处携带等等。如果您正在构建电子商务网站，并且有大量的评论，那么概括冗长的评论的工具可能会让您快速地浏览更多评论，以更好地了解您所有客户的想法。

```
prod_review = """
Got this panda plush toy for my daughter's birthday, \
who loves it and takes it everywhere. It's soft and \ 
super cute, and its face has a friendly look. It's \ 
a bit small for what I paid though. I think there \ 
might be other options that are bigger for the \ 
same price. It arrived a day earlier than expected, \ 
so I got to play with it myself before I gave it \ 
to her.
"""
```

这里有一个用于生成摘要的提示。您的任务是从电商网站的产品评论中生成一个简短的摘要，在最多30个单词内概括以下评论等。因此，这是一个软软的、可爱的熊猫毛绒玩具，受到女儿的喜爱，但价格有点小，提前到货。还不错，这是一个相当好的摘要。正如您在前一个视频中所看到的，您还可以通过控制字符计数或句子数等来影响这个摘要的长度。有时，在创建摘要时，如果您对摘要有一个非常具体的目的，例如，如果您想向物流部门提供反馈，您还可以修改提示以反映这一点，以便它可以生成一个更适用于您业务中的一个特定组的摘要。

````
prompt = f"""
Your task is to generate a short summary of a product \
review from an ecommerce site. 

Summarize the review below, delimited by triple 
backticks, in at most 30 words. 

Review: ```{prod_review}```

"""

response = get_completion(prompt)

print(response)

"""

Soft and cute panda plush toy loved by daughter, but a bit small for the price. Arrived early.

"""
````

## 专注物流

例如，如果我想向物流部门提供反馈，我可以将这个提示修改为重点关注提到了哪些方面与产品的物流和交付相关。如果我运行这个提示，那么它将生成一个摘要，但不是从“软软的、可爱的熊猫毛绒玩具”开始，而是侧重于它提前了一天到货的事实，然后仍然有其他的细节。

````
prompt = f"""
Your task is to generate a short summary of a product \
review from an ecommerce site to give feedback to the \
Shipping deparmtment. 

Summarize the review below, delimited by triple 
backticks, in at most 30 words, and focusing on any aspects \
that mention shipping and delivery of the product. 

Review: ```{prod_review}```
"""

response = get_completion(prompt)
print(response)
"""

The panda plush toy arrived a day earlier than expected, but the customer felt it was a bit small for the price paid.
"""
````

## 专注价格

或者举个例子，如果我们不是试图向物流部门提供反馈，而是想向定价部门提供反馈，那么定价部门负责确定产品的价格。我会让它关注与价格和感知价值相关的任何方面。然后这将生成一个不同的摘要，说可能价格对其尺寸来说太高了。在我为物流部门或定价部门生成的摘要中，它更加侧重于与这些具体部门相关的信息。事实上，随时随地可以暂停视频，然后让它为负责产品客户体验的产品部门生成信息。或者为您认为可能与电商网站有关的其他内容生成信息。但在这些摘要中，即使它生成了与物流相关的信息，它也有其他的信息，您可以决定是否有帮助。

````
prompt = f"""
Your task is to generate a short summary of a product \
review from an ecommerce site to give feedback to the \
pricing deparmtment, responsible for determining the \
price of the product.  

Summarize the review below, delimited by triple 
backticks, in at most 30 words, and focusing on any aspects \
that are relevant to the price and perceived value. 

Review: ```{prod_review}```

"""

response = get_completion(prompt)

print(response)

"""

The panda plush toy is soft, cute, and loved by the recipient, but the price may be too high for its size.

"""
````

因此，根据您想要总结的方式，您还可以要求它提取信息，而不是总结它。

下面是一个提示，表示您的任务是提取相关信息，以便向运输部门提供反馈。现在它只说产品提前了一天到达，没有其他信息，虽然在一般的摘要中也有希望的内容，但如果运输部门只想知道发生了什么，那么这些内容与其不太相关。

````
prompt = f"""
Your task is to extract relevant information from \ 
a product review from an ecommerce site to give \
feedback to the Shipping department. 

From the review below, delimited by triple quotes \
````




````python
extract the information relevant to shipping and \ 

delivery. Limit to 30 words. 

Review: ```{prod_review}```
"""
response = get_completion(prompt)
print(response)
"""
The product arrived a day earlier than expected.
"""
````

最后，让我与您分享一个具体的示例，说明如何在工作流程中使用它来帮助总结多个评论，以使它们更易于阅读。所以，这里有几篇评论。这有点长，但是这是第二篇关于一个站立灯的评论，它可以用于卧室。这是第三篇关于电动牙刷的评论。我的牙医推荐了它。这是关于搅拌机的评论，当他们说那个季节性销售的17件套装等等。实际上，这是很多文本。如果您想要，可以随时暂停视频并详细阅读所有这些内容。但是，如果您想要知道这些评论者写了什么，而不必停下来详细阅读所有内容，该怎么办呢？所以我将评论1设置为我们之前所看到的产品评论。并将所有这些评论放入列表中。

````python
review_1 = prod_review 

# review for a standing lamp
review_2 = """
Needed a nice lamp for my bedroom, and this one \
had additional storage and not too high of a price \
point. Got it fast - arrived in 2 days. The string \
to the lamp broke during the transit and the company \
happily sent over a new one. Came within a few days \
as well. It was easy to put together. Then I had a \
missing part, so I contacted their support and they \
very quickly got me the missing piece! Seems to me \
to be a great company that cares about their customers \
and products. 
"""

# review for an electric toothbrush
review_3 = """
My dental hygienist recommended an electric toothbrush, \
which is why I got this. The battery life seems to be \
pretty impressive so far. After initial charging and \
leaving the charger plugged in for the first week to \
condition the battery, I've unplugged the charger and \
been using it for twice daily brushing for the last \
3 weeks all on the same charge. But the toothbrush head \
is too small. I’ve seen baby toothbrushes bigger than \
this one. I wish the head was bigger with different \
length bristles to get between teeth better because \
this one doesn’t.  Overall if you can get this one \
around the $50 mark, it's a good deal. The manufactuer's \
replacements heads are pretty expensive, but you can \
get generic ones that're more reasonably priced. This \
toothbrush makes me feel like I've been to the dentist \
every day. My teeth feel sparkly clean! 
"""

# review for a blender
review_4 = """
So, they still had the 17 piece system on seasonal \
sale for around $49 in the month of November, about \
half off, but for some reason (call it price gouging) \
around the second week of December the prices all went \
up to about anywhere from between $70-$89 for the same \
system. And the 11 piece system went up around $10 or \
so in price also from the earlier sale price of $29. \
So it looks okay, but if you look at the base, the part \
where the blade locks into place doesn’t look as good \
as in previous editions from a few years ago, but I \
plan to be very gentle with it (example, I crush \
very hard items like beans, ice, rice, etc. in the \ 
blender first then pulverize them in the serving size \
I want in the blender then switch to the whipping \
blade for a finer flour, and use the cross cutting blade \
first when making smoothies, then use the flat blade \
if I need them finer/less pulpy). Special tip when making \
smoothies, finely cut and freeze the fruits and \
vegetables (if using spinach-lightly stew soften the \ 
spinach then freeze until ready for use-and if making \
sorbet, use a small to medium sized food processor) \ 
that you plan to use that way you can avoid adding so \
much ice if at all-when making your smoothie. \
After about a year, the motor was making a funny noise. \
I called customer service but the warranty expired \
already, so I had to buy another one. FYI: The overall \
quality has gone done in these types of products, so \
they are kind of counting on brand recognition and \
consumer loyalty to maintain sales. Got it in about \
two days.
"""

reviews = [review_1, review_2, review_3, review_4]

for i in range(len(reviews)):
    prompt = f"""
    Your task is to generate a short summary of a product \ 
    review from an ecommerce site. 

    Summarize the review below, delimited by triple \
    backticks in at most 20 words. 

    Review: ```{reviews[i]}```

    """

    response = get_completion(prompt)

    print(i, response, "\n")
````

```
"""
0 Soft and cute panda plush toy loved by daughter, but a bit small for the price. Arrived early. 

1 Affordable lamp with storage, fast shipping, and excellent customer service. Easy to assemble and missing parts were quickly replaced. 

2 Good battery life, small toothbrush head, but effective cleaning. Good deal if bought around $50. 

3 The product was on sale for $49 in November, but the price increased to $70-$89 in December. The base doesn't look as good as previous editions, but the reviewer plans to be gentle with it. A special tip for making smoothies is to freeze the fruits and vegetables beforehand. The motor made a funny noise after a year, and the warranty had expired. Overall quality has decreased. 

"""
```

现在，如果我在评论上实现一个for循环。这是我的提示，我要求它在最多20个单词中进行总结。然后让它获取响应并将其打印出来。让我们运行它。

它打印出第一个评论是Pantatoi评论，灯的评论总结，牙刷的评论总结，然后是搅拌机。因此，如果您有一个拥有数百条评论的网站，您可以想象如何使用这个功能构建一个仪表板，以获取大量评论的简短摘要，以便您或其他人可以更快地浏览评论。然后，如果他们愿意，可以单击以查看原始的长评论。这可以帮助您有效地了解所有客户的想法。

这就是关于总结的全部内容。我希望您可以想象如果您有任何包含许多文本的应用程序，如何使用这些提示来对它们进行总结，以帮助人们快速了解文本中的内容，以及可能选择更深入地挖掘。

在下一个视频中，我们将看看大型语言模型的另一个能力，即使用文本进行推断。例如，如果您有产品评论并想要快速了解哪些产品评论具有正面或负面情绪，该如何做呢？让我们在下一个视频中看看如何做到这一点。

# 文本推理 / Inferring

[prompt_eng_05_inferring.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/NdaebMTYjo2Sg7xIV53c5X35ncg)

下一个视频是关于推断的。我认为这些任务是指模型将文本作为输入并执行某种分析的任务。因此，这可以是提取标签、提取名称、理解文本的情感等任务。例如，如果您想从一段文本中提取积极或消极的情感，在传统的机器学习工作流程中，您需要收集标签数据集、训练模型、确定如何在云中部署模型并进行推断。这可能非常有效，但是需要经历大量的工作流程。而对于每个任务，例如情感、提取名称等，您都需要训练和部署单独的模型。

大型语言模型的一个很好的优点是，对于许多这样的任务，您只需要编写一个提示，就可以立即开始生成结果。这在应用程序开发方面具有巨大的速度优势。而且，您可以只使用一个模型、一个API来执行许多不同的任务，而不需要找出如何训练和部署许多不同的模型。因此，让我们跳进代码中，看看您如何利用这一点。下面是一个常规的起始代码，我将运行它。

## 环境准备

```python
import openai

import os

from dotenv import load_dotenv, find_dotenv

_ = load_dotenv(find_dotenv()) # read local .env file

openai.api_key  = os.getenv('OPENAI_API_KEY')
```

```python
def get_completion(prompt, model="gpt-3.5-turbo"):

    messages = [{"role": "user", "content": prompt}]

    response = openai.ChatCompletion.create(

        model=model,

        messages=messages,

        temperature=0, # this is the degree of randomness of the model's output

    )

    return response.choices[0].message["content"]
```

## 情感分析

我要使用的最重要的例子是一个台灯的评价。所以我需要一款适合卧室的好看台灯，并且带有额外的储物空间等等。

```python
lamp_review = """
Needed a nice lamp for my bedroom, and this one had \
additional storage and not too high of a price point. \
Got it fast.  The string to our lamp broke during the \
transit and the company happily sent over a new one. \
Came within a few days as well. It was easy to put \
together.  I had a missing part, so I contacted their \
support and they very quickly got me the missing piece! \
Lumina seems to me to be a great company that cares \
about their customers and products!!
"""
```

现在让我写一个提示来分类这个评价的情感。如果我想让系统告诉我这个评价的情感，我只需要写下“以下产品评价的情感是什么”，然后加上通常的分隔符和评价文本等等。然后运行它。

```python
prompt = f"""
What is the sentiment of the following product review, 

which is delimited with triple backticks?

Review text: '''{lamp_review}'''
"""
response = get_completion(prompt)

print(response)

"""
The sentiment of the product review is positive.
"""
```

输出结果是这个产品评价的情感是积极的，这似乎相当正确。这个灯不是完美的，但是这位顾客似乎很满意。这似乎是一个关心客户和产品的好公司。我认为积极情感似乎是正确的答案。

现在这个程序打印了整个句子，“这个产品评价的情感是积极的”。如果你想要一个更简洁的回答，以便更容易进行后处理，我可以添加另一个指令，让它只回答一个单词，积极或消极。

```python
prompt = f"""
What is the sentiment of the following product review, 

which is delimited with triple backticks?

Give your answer as a single word, either "positive" \
"""
```



or "negative".

```python
Review text: '''{lamp_review}'''
response = get_completion(prompt)
print(response)
```

```text
positive
```

这样它就会像这样输出“积极”，这样一个文本处理程序就更容易获取这个输出并进行处理。

接下来让我们看另一个提示，仍然是使用这个灯的评价。在这里，我让它识别出以下评价作者表达的情绪列表，包括不超过五个项目。

```python
prompt = f"""
Identify a list of emotions that the writer of the \
following review is expressing. Include no more than \
five items in the list. Format your answer as a list of \
lower-case words separated by commas.

Review text: '''{lamp_review}'''
"""
response = get_completion(prompt)
print(response)
```

```text
happy, satisfied, grateful, impressed, content
```

大型语言模型非常擅长从一段文本中提取特定内容。在这种情况下，我们正在表达情感。这对于了解客户对特定产品的想法可能非常有用。对于许多客户支持组织来说，了解特定用户是否非常沮丧非常重要。因此，您可能有一个不同的分类问题，例如“以下评论的作者是否表达了愤怒？”

```python
prompt = f"""
Is the writer of the following review expressing anger?\
The review is delimited with triple backticks. \
Give your answer as either yes or no.

Review text: '''{lamp_review}'''
"""
response = get_completion(prompt)
print(response)
```

```text
No
```

因为如果有人真的很生气，那么可能需要特别注意客户的评论，以便客户支持或客户成功联系客户并弄清楚情况，并为客户解决问题。

在这种情况下，客户并没有生气。请注意，使用监督学习，如果我想构建所有这些分类器，那么我无法在短短几分钟内完成这项工作，但您可以暂停此视频并尝试更改某些提示，以获取对此灯评论进行不同推断的提示。

接下来，我将展示您可以使用该系统执行的其他任务，特别是从客户评论中提取更丰富的信息。信息提取是NLP（自然语言处理）的一部分，它涉及从文本中提取您想要了解的特定信息。

## 信息抽取

在这个提示中，我要求它识别以下内容：商品购买项和制造商品的公司名称。如果您试图总结来自在线购物电子商务网站的许多评论，确定物品、制造商、确定积极和消极情绪，跟踪特定物品或特定制造商的积极或消极情绪趋势可能会对您的大量评论很有用。在这个例子中，我将要求它将响应格式化为具有项目和品牌键的JSON对象。

```python
prompt = f"""
Identify the following items from the review text: 

- Item purchased by reviewer

- Company that made the item

The review is delimited with triple backticks. \
Format your response as a JSON object with \
"Item" and "Brand" as the keys. 
If the information isn't present, use "unknown" \
as the value.
Make your response as short as possible.

Review text: '''{lamp_review}'''
"""
response = get_completion(prompt)
print(response)
```

```json
{
  "Item": "lamp",
  "Brand": "Lumina"
}
```

因此，如果我这样做，它会说项目是灯，品牌是Luminar，您可以轻松地将其加载到Python字典中，然后对此输出进行进一步处理。

## 多任务抽取

在我们经历的例子中，您可以看到如何编写提示来识别情绪，确定某人是否生气，然后提取物品和品牌。提取所有这些信息的一种方法是使用3或4个提示并调用`getCompletion`，您可以一次一个地提取这些不同的字段，但事实证明，您实际上可以编写单个提示以同时提取所有这些信息。

```python
prompt = f"""
Identify the following items from the review text: 

- Sentiment (positive or negative)

- Is the reviewer expressing anger? (true or false)

- Item purchased by reviewer

- Company that made the item

The review is delimited with triple backticks. \
Format your response as a JSON object with \
"Sentiment", "Anger", "Item" and "Brand" as the keys.
If the information isn't present, use "unknown" \
as the value.
Make your response as short as possible.
Format the Anger value as a boolean.

Review text: '''{lamp_review}'''
"""
response = get_completion(prompt)
print(response)
```

```json
{
  "Sentiment": "positive",
  "Anger": false,
  "Item": "lamp with additional storage",
  "Brand": "Lumina"
}
```

让我们来举一个例子，比如识别精细项目，提取情感，作为评论者表达愤怒，购买商品，完全做到了。在这里，我还要告诉它将愤怒值格式化为布尔值，然后让我运行一下，这将输出一个JSON，其中情感是积极的，愤怒，并且false周围没有引号，因为我要求它将其作为布尔值输出。

它将项目提取为一个带有额外存储空间的台灯，而不是仅仅的台灯，这看起来还不错。这样，您可以使用一个单独的提示从文本中提取多个字段。如果有需要，您可以暂停视频并自己尝试不同的变体，甚至尝试输入完全不同的评论，以查看是否仍然可以准确提取这些内容。

## 主题分类

[prompt_eng_05_inferring.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QUhgbVnhQotNZkxbtMXcYdhxnse)

我看到大型语言模型的一个很酷的应用是推断主题。给定一篇长文，你知道，这篇文章是关于什么的？有哪些话题？这是一篇虚构的关于政府工人对他们工作的机构感受的报纸文章。所以，最近政府进行了一项调查，你知道，等等，NASA的结果是一个受欢迎的部门，具有很高的满意度评分。我是NASA的粉丝，我喜欢他们所做的工作，但这是一篇虚构的文章。因此，鉴于这样的一篇文章，我们可以问它，在这个提示下，确定以下文本中正在讨论的五个主题。让我们把每个项目格式化成一个或两个单词长，将您的响应格式化为逗号分隔的列表，如果我们运行它，你知道，我们会得到这篇文章是关于政府调查的，它是关于工作满意度的，它是关于NASA的等等。总的来说，我认为它相当不错，它提取了一个主题列表。

```python
story = """
In a recent survey conducted by the government, 
public sector employees were asked to rate their level 
of satisfaction with the department they work at. 
The results revealed that NASA was the most popular 
department with a satisfaction rating of 95%.
One NASA employee, John Smith, commented on the findings, 
stating, "I'm not surprised that NASA came out on top. 
It's a great place to work with amazing people and 
incredible opportunities. I'm proud to be a part of 
such an innovative organization."
The results were also welcomed by NASA's management team, 
with Director Tom Johnson stating, "We are thrilled to 
hear that our employees are satisfied with their work at NASA. 
We have a talented and dedicated team who work tirelessly 
to achieve our goals, and it's fantastic to see that their 
hard work is paying off."
The survey also revealed that the 
Social Security Administration had the lowest satisfaction 
rating, with only 45% of employees indicating they were 
satisfied with their job. The government has pledged to 
address the concerns raised by employees in the survey and 
work towards improving job satisfaction across all departments.
"""
```

```python
prompt = f"""
Determine five topics that are being discussed in the \
following text, which is delimited by triple backticks.
Make each item one or two words long. 
Format your response as a list of items separated by commas.

Text sample: '''{story}'''
"""
response = get_completion(prompt)
print(response.split(sep=','))
```

```text
['government survey',
 ' job satisfaction',
 ' NASA',
 ' Social Security Administration',
 ' employee concerns']
```

当然，您也可以将其拆分，以获取这篇文章讨论的五个主题的清单。

```python
prompt = f"""
Determine whether each item in the following list of \
topics is a topic in the text below, which
is delimited with triple backticks.
Give your answer as list with 0 or 1 for each topic.\
List of topics: {", ".join(topic_list)}
Text sample: '''{story}'''
"""
response = get_completion(prompt)
print(response)
```

```text
nasa: 1
local government: 0
engineering: 0
employee satisfaction: 1
federal government: 1
```

```python
topic_dict = {i.split(': ')[0]: int(i.split(': ')[1]) for i in response.split(sep='\n')}

if topic_dict['nasa'] == 1:
    print("ALERT: New NASA story!")
```



如果你有一组文章并提取出主题，那么你也可以使用大型语言模型来帮助你索引不同的主题。假设我们是一个新闻网站，这是我们追踪的主题：NASA，当地政府，工程，员工满意度和联邦政府。假设你想弄清楚给定一篇新闻文章，其中包含哪些主题。使用以下提示：确定以下主题列表中的每个项目是否是文本下的主题。为每个主题提供0或1的答案列表。只需使用一个提示，就能够确定新闻文章涉及的主题，并可以将此信息放入字典中，以便快速处理任何文章，确定其主题，并在主题包括NASA时打印出新NASA故事的警报。值得注意的是，上述提示并不是非常健壮的。

如果我去生产系统，我可能会让它以JSON格式输出答案，而不是作为列表，因为大型语言模型的输出可能有些不一致。因此，这实际上是一段相当脆弱的代码。但是如果您想，在看完本视频后，可以尝试修改此提示，使其输出JSON而不是像这样的列表，从而有一个更健壮的方法来确定更大的文章是否是关于NASA的故事。

这就是推断的内容，只需要几分钟的时间，您就可以构建多个用于对文本进行推断的系统，而以前这可能需要一个熟练的机器学习开发人员几天甚至几周的时间。因此，我认为这非常令人兴奋，无论是对于熟练的机器学习开发人员还是对于新手来说，您现在都可以使用提示来快速构建并开始推断这些相当复杂的自然语言处理任务。在下一个视频中，我们将继续讨论可以使用大型语言模型完成的一些令人兴奋的事情，并继续介绍转换。您如何将一段文本转换为另一段文本，例如将其翻译成另一种语言？让我们进入下一个视频。

# 文本转换 / Transforming

![](img/78a7dbb4571ccdc591a666af281af6c1.png)

[prompt_eng_06_transforming.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KRzdbliFYoPWFqxN13ocOSkGnJc)

大型语言模型非常擅长将其输入转换为不同的格式，例如输入一段文本，将其转换或翻译为另一种语言，或帮助拼写和语法纠正，即输入可能不完全符合语法的文本，帮助您稍微修复一下，甚至可以转换格式，例如输入HTML并输出JSON。因此，我曾经写过一些应用程序，用了很多常规表达式，现在使用大型语言模型和几个提示可以更简单地实现这些应用程序。

我现在几乎用Chat GPT来校对我写的所有东西，所以我很高兴现在在笔记本上向您展示更多的示例。因此，首先我们将导入OpenAI并使用在视频中一直使用的`getCompletion`帮助函数。

## 环境准备

```python
import openai
import os
from dotenv import load_dotenv, find_dotenv

_ = load_dotenv(find_dotenv()) # read local .env file

openai.api_key  = os.getenv('OPENAI_API_KEY')
```

```python
def get_completion(prompt, model="gpt-3.5-turbo", temperature=0): 
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=temperature, 
    )
    return response.choices[0].message["content"]
```

## 翻译

我们将首先做一个翻译任务。大型语言模型通常在许多来源的大量文本上进行训练，其中许多文本都是来自互联网，而这些文本又是不同的语言。因此，这使得模型具有进行翻译的能力。这些模型掌握了数百种语言，但掌握程度各不相同。接下来，我们将介绍如何使用这种能力的一些示例。

让我们从一些简单的内容开始。在第一个例子中，提示是将以下英文文本翻译为西班牙文。`Hi, I would like to order a blender` 的响应是`Hola，me gustaría ordenar una licuadora`。在这里，我非常抱歉所有的西班牙语听众，我不会西班牙语，这一点您肯定可以看出来。

```python
prompt = f"""
Translate the following English text to Spanish: \ 
```Hi, I would like to order a blender```
"""
response = get_completion(prompt)
print(response)
"""
Hola, me gustaría ordenar una licuadora.
"""
```

现在让我们试试另一个例子。在这个例子中，提示是告诉我这是哪种语言。然后这段文本是法语，`Combien coûte le lampadaire?`。模型识别出这是法语。

```python
prompt = f"""
Tell me which language this is: 
```Combien coûte le lampadaire?```
"""
response = get_completion(prompt)
print(response)
"""
This is French.
"""
```

模型还可以同时进行多种翻译。在这个例子中，让我们假设将以下文本翻译为法语和西班牙语。而且，让我们再加上一个英语海盗。文本是`I want to order a basketball`。所以在这里，我们有法语、西班牙语和英语海盗。

```python
prompt = f"""
Translate the following  text to French and Spanish
and English pirate: \
```I want to order a basketball```
"""
response = get_completion(prompt)
print(response)
"""
French pirate: ```Je veux commander un ballon de basket```

Spanish pirate: ```Quiero ordenar una pelota de baloncesto```
English pirate: ```I want to order a basketball```
"""
```

在某些语言中，翻译可能会因说话者与听众的关系而改变。您还可以向语言模型解释这一点。因此，它将能够相应地进行翻译。在这个例子中，我们说，将以下文本翻译为西班牙语的正式和非正式形式。`Would you like to order a pillow?` 还要注意，这里我们使用的是。只要有一定的明确分隔就可以了。所以，这里我们有正式和非正式之分。正式场合是指当你和一个比你资历高的人说话或者在一个职业场合时，你需要使用正式语气；而非正式则是当你和朋友群体交流时使用。

我实际上不会说西班牙语，但是我父亲会说，他说这是正确的。

```python
prompt = f"""
Translate the following text to Spanish in both the \
formal and informal forms: 
'Would you like to order a pillow?'
"""
response = get_completion(prompt)
print(response)
"""
Formal: ¿Le gustaría ordenar una almohada?
Informal: ¿Te gustaría ordenar una almohada?
"""
```

### 通用翻译

接下来的例子中，我们假设我们是一个跨国电子商务公司的负责人，用户的消息将使用各种不同的语言，并且用户将使用多种语言向我们反映其 IT 问题，因此我们需要一种通用的翻译器。首先，我们将粘贴一些不同语言的用户消息列表，然后我们将循环遍历每一个用户消息。因此，在用户消息中循环遍历问题，然后我将复制这个稍微长一点的代码块。首先，我们将询问模型告诉我们问题是用什么语言书写的。然后，我们将打印出原始消息的语言和问题，并要求模型将其翻译成英文和韩文。让我们运行它。

```python
user_messages = [
    "La performance du système est plus lente que d'habitude.",  # System performance is slower than normal         
    "Mi monitor tiene píxeles que no se iluminan.",              # My monitor has pixels that are not lighting
    "Il mio mouse non funziona",                                 # My mouse is not working
    "Mój klawisz Ctrl jest zepsuty",                             # My keyboard has a broken control key
    "我的屏幕在闪烁"                                               # My screen is flashing
] 
```

```python
for issue in user_messages:
    prompt = f"Tell me what language this is: ```{issue}```"
    lang = get_completion(prompt)
    print(f"Original message ({lang}): {issue}")

    prompt = f"""
    Translate the following  text to English \
    and Korean: ```{issue}```

    """

    response = get_completion(prompt)

    print(response, "\n")

"""
Original message (This is French.): La performance du système est plus lente que d'habitude.
English: The system performance is slower than usual.
Korean: 시스템 성능이 평소보다 느립니다. 

Original message (This is Spanish.): Mi monitor tiene píxeles que no se iluminan.
English: My monitor has pixels that don't light up.
Korean: 내 모니터에는 불이 켜지지 않는 픽셀이 있습니다. 

Original message (This is Italian.): Il mio mouse non funziona
English: My mouse is not working.
Korean: 내 마우스가 작동하지 않습니다. 

Original message (This is Polish.): Mój klawisz Ctrl jest zepsuty
English: My Ctrl key is broken.
Korean: 제 Ctrl 키가 고장 났어요. 

Original message (This is Chinese (Simplified).): 我的屏幕在闪烁
English: My screen is flickering.
Korean: 내 화면이 깜빡입니다. 
"""
```

## 风格转换

在这里，我们有一个更正式的商业信函，其中包括一份落地灯规格的建议书。接下来我们要做的事情是在不同的格式之间进行转换。

```python
prompt = f"""
Translate the following from slang to a business letter: 
'Dude, This is Joe, check out this spec on this standing lamp.'
"""
response = get_completion(prompt)
print(response)
"""
Dear Sir/Madam,

I am writing to bring to your attention a standing lamp that I believe may be of interest to you. Please find attached the specifications for your review.

Thank you for your time and consideration.

Sincerely,

Joe
"""
```

ChatGPT非常擅长在不同格式之间进行翻译，例如`JSON`到`HTML`、`XML`等各种格式。在提示中，我们将描述输入和输出格式。以下是一个示例：我们有一个包含餐厅员工姓名和电子邮件的`JSON`列表。在提示中，我们将要求模型将其从`JSON`转换为`HTML`表格。然后我们将得到模型的响应并打印出来，以便查看经过格式化后的`HTML`表格。

```python
data_json = { "resturant employees" :[ 
    {"name":"Shyam", "email":"shyamjaiswal@gmail.com"},
    {"name":"Bob", "email":"bob32@gmail.com"},
    {"name":"Jai", "email":"jai87@gmail.com"}
]}

prompt = f"""
Translate the following python dictionary from JSON to an HTML \
table with column headers and title: {data_json}
"""
response = get_completion(prompt)
print(response)
"""
<table>
  <caption>Restaurant Employees</caption>
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Shyam</td>
      <td>shyamjaiswal@gmail.com</td>
    </tr>
    <tr>
      <td>Bob</td>
      <td>bob32@gmail.com</td>
    </tr>
    <tr>
      <td>Jai</td>
      <td>jai87@gmail.com</td>
    </tr>
  </tbody>
</table>
"""
```

![](img/ef7da5585b8ef1687163f4049ea461b4.png)



# 拼写语法检查

接下来的转换任务是拼写检查和语法检查。这是ChatGPT的一个非常受欢迎的应用。我们建议您经常使用它，特别是在使用非母语语言时非常有用。以下是一些常见的语法和拼写问题以及如何使用语言模型来解决它们的示例。我们将列举一些句子，这些句子中存在某些语法或拼写错误。然后我们将循环遍历每个句子，请求模型进行校对和修正。然后我们会得到响应并像往常一样将其打印出来。模型能够纠正所有这些语法错误。我们可以使用之前讨论过的一些技巧来改进提示。为了改进提示，我们可以说“校对并更正以下文本。如果你没有找到任何错误，请说没有错误。”通过迭代的提示开发，您可以找到一个更可靠的提示。

````text
text = [ 

  "The girl with the black and white puppies have a ball.",  # The girl has a ball.

  "Yolanda has her notebook.", # ok

  "Its going to be a long day. Does the car need it’s oil changed?",  # Homonyms

  "Their goes my freedom. There going to bring they’re suitcases.",  # Homonyms

  "Your going to need you’re notebook.",  # Homonyms

  "That medicine effects my ability to sleep. Have you heard of the butterfly affect?", # Homonyms

  "This phrase is to cherck chatGPT for speling abilitty"  # spelling

]

for t in text:

    prompt = f"""Proofread and correct the following text

    and rewrite the corrected version. If you don't find

    and errors, just say "No errors found". Don't use 

    any punctuation around the text:

    ```{t}```"""
    response = get_completion(prompt)
    print(response)
    
 """
 The girl with the black and white puppies has a ball.
No errors found.
It's going to be a long day. Does the car need its oil changed?
Their goes my freedom. There going to bring they're suitcases.

Corrected version: 
There goes my freedom. They're going to bring their suitcases.
You're going to need your notebook.
That medicine affects my ability to sleep. Have you heard of the butterfly effect?
This phrase is to check ChatGPT for spelling ability.
 """
````

另一个例子是检查您在公共论坛上发布文本之前进行拼写和语法检查的示例。我们将使用一份有关填充熊猫的评论的示例，然后要求模型对评论进行校对和更正。最后，我们将得到更正后的版本。

````text
text = f"""
Got this for my daughter for her birthday cuz she keeps taking \
mine from my room.  Yes, adults also like pandas too.  She takes \
it everywhere with her, and it's super soft and cute.  One of the \
ears is a bit lower than the other, and I don't think that was \
designed to be asymmetrical. It's a bit small for what I paid for it \
though. I think there might be other options that are bigger for \
the same price.  It arrived a day earlier than expected, so I got \
to play with it myself before I gave it to my daughter.
"""
prompt = f"proofread and correct this review: ```{text}```"

response = get_completion(prompt)

print(response)

"""

I got this for my daughter's birthday because she keeps taking mine from my room. Yes, adults also like pandas too. She takes it everywhere with her, and it's super soft and cute. However, one of the ears is a bit lower than the other, and I don't think that was designed to be asymmetrical. Additionally, it's a bit small for what I paid for it. I think there might be other options that are bigger for the same price. On the positive side, it arrived a day earlier than expected, so I got to play with it myself before I gave it to my daughter.

"""
````

我们可以做的一件很酷的事情是找到原始评论和模型输出之间的差异。因此，我们将使用这个`RedLines` Python包来做这件事情。我们将获得我们评论的原始文本和模型输出之间的差异，然后将其显示出来。在这里，您可以看到原始评论和模型输出之间的差异以及已经更正的内容。

```text
from redlines import Redlines

diff = Redlines(text,response)

display(Markdown(diff.output_markdown))
```

![](img/d8b589cee67d7208240ac45f13ace7bd.png)

因此，我们使用的提示是“校对并更正此评论”，但您还可以进行更明显的更改，例如改变语气等。让我们再试试其他东西。在这个提示中，我们将要求模型校对和更正同一篇评论，同时使其更有吸引力，确保它遵循APA风格，并面向高级读者。我们还将要求以markdown格式输出。因此，我们在上面使用的是来自原始评论的相同文本。让我们执行一下。在这里，我们有一篇扩展的APA风格评论《软性熊猫》。

````text
prompt = f"""

proofread and correct this review. Make it more compelling. 

Ensure it follows APA style guide and targets an advanced reader. 

Output in markdown format.

Text: ```{text}```
"""
response = get_completion(prompt)
display(Markdown(response))

"""
Title: A Soft and Cute Panda Plushie for All Ages
As an adult, I can attest that pandas are not just for kids. That's why I got this adorable panda plushie for my daughter's birthday, after she kept taking mine from my room. And let me tell you, it was a hit!
The plushie is super soft and cuddly, making it the perfect companion for my daughter. She takes it everywhere with her, and it has quickly become her favorite toy. However, I did notice that one of the ears is a bit lower than the other, which I don't think was designed to be asymmetrical. But that doesn't take away from its cuteness.
The only downside is that it's a bit small for the price I paid. I think there might be other options that are bigger for the same price. But overall, I'm happy with my purchase.
One thing that surprised me was that it arrived a day earlier than expected. This gave me the chance to play with it myself before giving it to my daughter. And I have to say, I was impressed with the quality and attention to detail.
In conclusion, if you're looking for a soft and cute panda plushie for yourself or a loved one, this is definitely a great option. Just be aware that it might be a bit smaller than expected.
"""
````

# 文本扩写 / Expanding

[prompt_eng_07_expanding.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FW1HbARtWoBd9OxziafcQ6dKn4g)

扩展是将短的文本片段，比如一组说明或一系列主题，并让大型语言模型生成更长的文本，比如关于某个主题的电子邮件或论文的任务。这样做很有用，比如如果你把大型语言模型作为头脑风暴的合作伙伴。但我也想指出，这样做有一些问题，比如如果有人用它生成大量的垃圾邮件。因此，当你使用大型语言模型的这些功能时，请负责任地使用，并以有助于人们的方式使用。在这个视频中，我们将演示如何使用语言模型根据某些信息生成个性化的电子邮件。这封电子邮件有点自称为来自 AI 机器人，这一点非常重要，如安德鲁所提到的。我们还将使用另一个模型输入参数，称为`temperature`（温度），这使您能够变化模型响应中的探索和多样性程度。让我们开始吧。

## 环境准备

所以在我们开始之前，我们要做一些通常的设置。所以设置OpenAI Python包，然后定义我们的帮助函数`get_completion`。

```text
import openai
import os

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file

openai.api_key  = os.getenv('OPENAI_API_KEY')
```

```text
def get_completion(prompt, model="gpt-3.5-turbo",temperature=0): # Andrew mentioned that the prompt/ completion paradigm is preferable for this class
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=temperature, # this is the degree of randomness of the model's output
    )
    return response.choices[0].message["content"]
```

## 自动回复邮件

现在我们要为客户评论编写一个自定义的电子邮件回复，因此给定客户评论和情绪，我们将生成一个自定义的回复。现在我们将使用语言模型根据客户评论和评论的情绪为客户生成一个自定义的电子邮件。所以我们已经使用我们在推断视频中看到的那种提示提取了情绪，然后这是搅拌器的客户评论，现在我们将根据情绪定制回复。



```
# given the sentiment from the lesson on "inferring",
# and the original customer message, customize the email
sentiment = "negative"

# review for a blender
review = f"""
So, they still had the 17 piece system on seasonal \
sale for around $49 in the month of November, about \
half off, but for some reason (call it price gouging) \
around the second week of December the prices all went \
up to about anywhere from between $70-$89 for the same \
system. And the 11 piece system went up around $10 or \
so in price also from the earlier sale price of $29. \
So it looks okay, but if you look at the base, the part \
where the blade locks into place doesn’t look as good \
as in previous editions from a few years ago, but I \
plan to be very gentle with it (example, I crush \
very hard items like beans, ice, rice, etc. in the \ 
blender first then pulverize them in the serving size \
I want in the blender then switch to the whipping \
blade for a finer flour, and use the cross cutting blade \
first when making smoothies, then use the flat blade \
if I need them finer/less pulpy). Special tip when making \
smoothies, finely cut and freeze the fruits and \
vegetables (if using spinach-lightly stew soften the \ 
spinach then freeze until ready for use-and if making \
sorbet, use a small to medium sized food processor) \ 
that you plan to use that way you can avoid adding so \
much ice if at all-when making your smoothie. \
After about a year, the motor was making a funny noise. \
I called customer service but the warranty expired \
already, so I had to buy another one. FYI: The overall \
quality has gone done in these types of products, so \
they are kind of counting on brand recognition and \
consumer loyalty to maintain sales. Got it in about \
two days.
"""
```

因此，这里的说明是您是一名客户服务AI助理，您的任务是发送一封关于您的客户的电子邮件回复，给定由三个反引号分隔的客户电子邮件，生成回复以感谢客户的评论。如果情绪是积极或中立的，感谢他们的评论。如果情绪是消极的，道歉并建议他们可以联系客户服务。确保使用评论中的具体细节，以简洁和专业的语气书写，并以客户代理AI身份签署电子邮件。

````
prompt = f"""
You are a customer service AI assistant.
Your task is to send an email reply to a valued customer.
Given the customer email delimited by ```, \

Generate a reply to thank the customer for their review.

If the sentiment is positive or neutral, thank them for \

their review.

If the sentiment is negative, apologize and suggest that \

they can reach out to customer service. 

Make sure to use specific details from the review.

Write in a concise and professional tone.

Sign the email as `AI customer agent`.

Customer review: ```{review}```
Review sentiment: {sentiment}
"""
response = get_completion(prompt)
print(response)

"""
Dear Valued Customer,

Thank you for taking the time to leave a review about our product. We are sorry to hear that you experienced an increase in price and that the quality of the product did not meet your expectations. We apologize for any inconvenience this may have caused you.

We would like to assure you that we take all feedback seriously and we will be sure to pass your comments along to our team. If you have any further concerns, please do not hesitate to reach out to our customer service team for assistance.

Thank you again for your review and for choosing our product. We hope to have the opportunity to serve you better in the future.

Best regards,

AI customer agent
"""
````

当您使用语言模型生成文本时，您将向用户展示这种透明度非常重要，让用户知道他们看到的文本是由AI生成的。然后我们将输入客户评论和评论情绪。还要注意，这部分不一定重要，因为我们实际上可以使用此提示来提取评论情绪，然后在后续步骤中编写电子邮件。但为了示例，我们已经从评论中提取了情绪。因此，这里我们有对客户的回复。它解决了客户在评论中提到的细节。就像我们指示的那样，建议他们联系客户服务，因为这只是一个AI的客户服务代理。

接下来，我们将使用语言模型中的一个参数，称为温度，它将允许我们改变模型响应的种类。所以你可以将温度视为模型的探索程度或随机性。因此，对于这个特定的短语，我最喜欢的食物是模型预测的最有可能的下一个单词是披萨，其次是寿司和玉米卷。所以，在零的温度下，模型总是会选择最有可能的下一个词，在这种情况下是披萨，在更高的温度下，它也会选择一个不太可能的词，在更高的温度下，它甚至可能选择玉米卷，它只有5%的机会被选中。

你可以想象那种情况，当模型继续这个最后的反应时，我最喜欢的食物是披萨，它继续产生更多的单词，这个反应会与反应有所不同，第一个反应，我最喜欢的食物是玉米卷。因此，随着这种模型的继续，这两种响应将变得越来越不同。

一般来说，在构建想要一种可预测响应的应用程序时，我建议使用温度零。在所有这些视频中，我们一直在使用温度零，我认为如果你试图构建一个可靠和可预测的系统，你应该这样做。如果你试图以一种更有创意的方式使用模型，你可能想要一种更广泛的不同输出，你可能想要使用更高的温度。所以，现在让我们使用我们刚刚使用的提示，让我们尝试生成一封电子邮件，但让我们使用更高的温度。所以，在我们在整个视频中使用的get完成函数中，我们已经指定了一个模型，然后也是一个温度，但我们已经将它们设置为默认值。所以，现在让我们尝试改变温度。所以，我们将使用提示，然后让我们尝试温度0.7。因此，当温度为0时，每次执行相同的提示时，您应该期望相同的完成。而当温度为0.7时，您每次都会得到不同的输出。

````
prompt = f"""
You are a customer service AI assistant.
Your task is to send an email reply to a valued customer.
Given the customer email delimited by ```, \

Generate a reply to thank the customer for their review.

If the sentiment is positive or neutral, thank them for \

their review.

If the sentiment is negative, apologize and suggest that \

they can reach out to customer service. 

Make sure to use specific details from the review.

Write in a concise and professional tone.

Sign the email as `AI customer agent`.

Customer review: ```{review}```
Review sentiment: {sentiment}
"""
response = get_completion(prompt, temperature=0.7)
print(response)
"""
Dear valued customer,

Thank you for taking the time to leave a review about our product. We are sorry to hear that you experienced an increase in price for the system you were interested in, and that you also experienced an issue with the motor after a year of use. We apologize for any inconvenience this may have caused you.

Please know that our company strives to provide excellent customer service and we would like to assist you with this matter. Please feel free to reach out to our customer service team at any time for assistance.

Thank you again for your feedback and for choosing our product. 

Best regards,
AI customer agent
"""
````

这是我们的电子邮件，正如你所看到的，它与我们之前收到的电子邮件不同。

让我们再执行一次，以表明我们将再次收到不同的电子邮件。这里我们有另一封不同的电子邮件。所以，我建议你自己玩玩温度。也许你现在可以暂停视频，在各种不同的温度下尝试这个提示，看看输出是如何变化的。总结一下，在更高的温度下，模型的输出更随机。你几乎可以认为在更高的温度下，助手更容易分心，但也许更有创造力。

```
"""
Dear Valued Customer,

Thank you for taking the time to leave a review. We sincerely apologize for the inconvenience you experienced with the price increase and the quality of the product. We understand your frustration and would like to offer our deepest apologies. If there is anything we can do to assist you, please do not hesitate to reach out to our customer service team.

We appreciate your feedback and hope to have the opportunity to serve you better in the future. 

Kind Regards,

AI customer agent
"""
```

在下一个视频中，我们将详细讨论`Chat Completions Endpoint`格式，以及如何使用此格式创建自定义聊天机器人。

# 聊天机器人 / ChatBot

![](img/ee1707a939516c91b51919a81b2e758a.png)

[prompt_eng_08_chatbot.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AB5Tbh9WPo0026xhqgzcrEcAn3e)

关于大型语言模型的一个令人兴奋的事情是你可以用它来构建一个自定义聊天机器人，只需少量的努力。ChatGPT，网络界面，是你拥有对话界面的一种方式，通过大型语言模型进行对话。但很酷的事情之一是你也可以使用大型语言模型来构建你的自定义聊天机器人，也许可以扮演AI客户服务代理或餐馆AI接单员的角色。

在本视频中，你将学习如何为自己做这件事。我将更详细地描述OpenAI `ChatCompletions`格式的组件，然后你将自己构建一个聊天机器人。让我们开始吧。

## 环境准备

首先，我们将像往常一样设置OpenAI Python包。

```
import os
import openai
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file

openai.api_key  = os.getenv('OPENAI_API_KEY')
```

像ChatGPT这样的聊天模型实际上被训练为将一系列消息作为输入并返回模型生成的消息作为输出。因此，尽管聊天格式旨在使这样的多轮对话变得简单，但我们已经通过之前的视频看到，它对于没有任何对话的单轮任务也同样有用。所以接下来，我们将定义两个辅助函数。这是我们在所有视频中一直使用的函数，它是`getWorks`函数。

但是如果你看一下，我们给出了一个提示，然后在函数内部，我们实际上在做的是将这个提示放入看起来像某种用户消息中。这是因为ChatGPT模型是一个聊天模型，这意味着它被训练成将一系列消息作为输入，然后返回模型生成的消息作为输出。所以用户消息是一种输入，然后助手消息是输出。所以，在这个视频中，我们将使用一个不同的帮助函数，而不是把一个提示作为输入并得到一个完成，我们将传入一个消息列表。这些消息可以来自各种不同的角色，所以我将描述这些。



```python
def get_completion(prompt, model="gpt-3.5-turbo"):
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=0, # this is the degree of randomness of the model's output
    )
    return response.choices[0].message["content"]

def get_completion_from_messages(messages, model="gpt-3.5-turbo", temperature=0):
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=temperature, # this is the degree of randomness of the model's output
    )
#     print(str(response.choices[0].message))
    return response.choices[0].message["content"]
```

所以这是一个消息列表的示例。所以，第一条消息是一个系统消息，它给出了一个整体指令，然后在这条消息之后，我们在用户和助手之间进行了某种转换。这将继续下去。如果你曾经使用过ChatGPT，网络界面，那么你的消息就是用户消息，然后ChatGPT的消息就是助手消息。所以系统消息有助于设置助手的行为和角色，它在某种程度上是对话的高级指令。所以你可以把它想象成在助手耳边窃窃私语，引导助手的反应，而用户却没有意识到系统消息。

所以，作为用户，如果你曾经使用过ChatGPT，你可能不知道ChatGPT的系统消息中有什么，这就是目的。系统消息的好处是，它为开发人员提供了一种构建对话的方法，而无需将请求本身作为对话的一部分。因此，您可以引导助手，在其耳边低语并引导其响应，而无需让用户意识到。

```python
messages =  [  
{'role':'system', 'content':'You are an assistant that speaks like Shakespeare.'},    
{'role':'user', 'content':'tell me a joke'},   
{'role':'assistant', 'content':'Why did the chicken cross the road'},   
{'role':'user', 'content':'I don\'t know'}  ]
```

所以，现在让我们尝试在对话中使用这些消息。因此，我们将使用我们的新助手函数从消息中获得完成。我们还使用更高的温度。所以系统消息说，你是一个说话像莎士比亚的助手。所以这是我们向助手描述它应该如何表现。然后第一个用户信息是，给我讲个笑话。接下来是，为什么鸡要过马路？最后一个用户信息是，我不知道。所以如果我们运行这个，响应是到达另一边。让我们再试一次。

```python
response = get_completion_from_messages(messages, temperature=1)
print(response)
"""
To get to the other side, sire! 'Tis a classic jest, known by many a bard.
"""
```

为了到达另一边，公平地说，夫人，这是一个古老的经典，永远不会失败。这是我们的莎士比亚响应。

让我们再试一件事，因为我想更清楚地说明这是助手消息。所以在这里，让我们去打印整个消息响应。所以，为了让这更清楚，嗯，这个响应是一个助手消息。所以，角色是助手，然后内容是消息本身。

所以，这就是这个助手函数中发生的事情。我们只是传递消息的内容。现在让我们做另一个例子。

```python
messages =  [  
{'role':'system', 'content':'You are friendly chatbot.'},    
{'role':'user', 'content':'Hi, my name is Isa'}  ]
response = get_completion_from_messages(messages, temperature=1)
print(response)
"""
Hello Isa! Nice to meet you. How are you doing today?
"""
```

所以，这里我们的消息是，嗯，助手消息是，你是一个友好的聊天机器人，第一条用户消息是，嗨，我的名字是Isa。我们想，嗯，获取第一条用户消息。所以，让我们执行这个。第一条助手消息。所以，第一条消息是，你好Isa，很高兴见到你。我今天可以如何帮助你？

```python
messages =  [  
{'role':'system', 'content':'You are friendly chatbot.'},    
{'role':'user', 'content':'Yes,  can you remind me, What is my name?'}  ]
response = get_completion_from_messages(messages, temperature=1)
print(response)
"""
I'm sorry, but as a chatbot, I do not have access to information about your personal details such as your name. However, you can tell me your name and we can continue our conversation.
"""
```

现在，让我们尝试另一个例子。所以，这里我们的消息是，嗯，系统消息，你是一个友好的聊天机器人，第一条用户消息是，是的，你能提醒我我的名字是什么吗？让我们得到响应。正如你所看到的，模型实际上并不知道我的名字。所以，与语言模型的每个对话都是一个独立的交互，这意味着你必须为模型提供所有相关的消息，以便在当前对话中提取。

如果你想让模型从对话中提取，或者引用，记住对话的早期部分，你必须在模型的输入中提供早期的交流。所以，我们将此称为上下文。所以，让我们试试这个。

```python
messages =  [  
{'role':'system', 'content':'You are friendly chatbot.'},
{'role':'user', 'content':'Hi, my name is Isa'},
{'role':'assistant', 'content': "Hi Isa! It's nice to meet you. \
Is there anything I can help you with today?"},
{'role':'user', 'content':'Yes, you can remind me, What is my name?'}  ]
response = get_completion_from_messages(messages, temperature=1)
print(response)
"""
Of course, your name is Isa.
"""
```

所以，现在我们已经给出了模型需要的上下文，嗯，这是我在前面的消息中的名字，我们会问同样的问题，所以我们会问我的名字是什么。模型能够响应，因为它拥有它需要的所有上下文，嗯，在我们输入给它的这种消息列表中。

## 订单机器人

所以现在你要构建你自己的聊天机器人。这个聊天机器人将被称为`orderbot`，我们将自动收集用户提示和助手响应，以构建这个`orderbot`。它将接受披萨店的订单，所以首先我们要定义这个帮助函数，它要做的是收集我们的用户消息，这样我们就可以避免像上面那样手动输入它们，这将从下面构建的用户交互界面收集提示，然后将其附加到一个名为上下文的列表中，然后它将每次都使用该上下文调用模型。然后模型响应也被添加到上下文中，所以模型消息被添加到上下文中，用户消息被添加到上下文中，依此类推，所以它越来越长。

```python
def collect_messages(_):
    prompt = inp.value_input
    inp.value = ''
    context.append({'role':'user', 'content':f"{prompt}"})
    response = get_completion_from_messages(context) 
    context.append({'role':'assistant', 'content':f"{response}"})
    panels.append(
        pn.Row('User:', pn.pane.Markdown(prompt, width=600)))
    panels.append(
        pn.Row('Assistant:', pn.pane.Markdown(response, width=600, style={'background-color': '#F6F6F6'})))
 
    return pn.Column(*panels)
```

这样模型就有了它需要的信息来决定下一步做什么。现在我们将设置并运行这种UI来显示订单机器人，这是上下文，它包含包含菜单的系统消息，注意，每次我们调用语言模型时，我们都将使用相同的上下文，上下文随着时间的推移而积累。然后让我们执行这个。好的，我要说，嗨，我想订一个比萨饼。助理说，太好了，你想订什么比萨饼？我们有香肠、奶酪和茄子比萨饼。它们多少钱？太好了，好的，我们有价格。我想我觉得一个中等的茄子比萨饼。所以你可以想象，我们可以继续这个对话，让我们看看我们在系统消息中放了什么。

> 你是订单机器人，一个为比萨饼餐厅收集订单的自动化服务。你首先问候顾客，然后收集订单，然后询问是提货还是送货。你等待收集整个订单，然后总结一下，最后一次检查客户是否想要添加任何其他东西。如果是送货，你可以要求一个地址。最后，你收取付款。确保澄清所有选项、额外费用和尺寸，以唯一地识别菜单中的项目。你以简短、非常对话、友好的方式回应。菜单包括，然后我们有菜单。

所以让我们回到我们的谈话，让我们看看助理是否一直在遵循指示。

```python
import panel as pn  # GUI
pn.extension()

panels = [] # collect display 

context = [ {'role':'system', 'content':"""
You are OrderBot, an automated service to collect orders for a pizza restaurant. \
You first greet the customer, then collects the order, \
and then asks if it's a pickup or delivery. \
You wait to collect the entire order, then summarize it and check for a final \
time if the customer wants to add anything else. \
If it's a delivery, you ask for an address. \
Finally you collect the payment.\
Make sure to clarify all options, extras and sizes to uniquely \
identify the item from the menu.\
You respond in a short, very conversational friendly style. \
The menu includes \
pepperoni pizza  12.95, 10.00, 7.00 \
cheese pizza   10.95, 9.25, 6.50 \
eggplant pizza   11.95, 9.75, 6.75 \
fries 4.50, 3.50 \
greek salad 7.25 \
Toppings: \
extra cheese 2.00, \
mushrooms 1.50 \
sausage 3.00 \
canadian bacon 3.50 \
AI sauce 1.50 \
peppers 1.00 \
Drinks: \
coke 3.00, 2.00, 1.00 \
sprite 3.00, 2.00, 1.00 \
bottled water 5.00 \
"""}]  # accumulate messages


inp = pn.widgets.TextInput(value="Hi", placeholder='Enter text here…')
button_conversation = pn.widgets.Button(name="Chat!")

interactive_conversation = pn.bind(collect_messages, button_conversation)

dashboard = pn.Column(
    inp,
    pn.Row(button_conversation),
    pn.panel(interactive_conversation, loading_indicator=True, height=300),
)

dashboard
"""
[出现一个人机交互界面]
"""
```

实际使用示例效果如下

![](img/2e302213e2fe1ef6cebf17eddeddf3df.png)

好的，太好了，助理问我们是否想要任何我们指定了助理信息的配料。所以我想我们不需要额外的配料。事情…当然可以。我们还想点什么吗？嗯，让我们喝点水。实际上，薯条。小还是大？这很棒，因为我们在系统消息中要求助手澄清额外的东西和附加内容。

所以你明白了，请随意自己玩这个。你可以暂停视频，继续在左边你自己的笔记本上运行它。

所以现在我们可以要求模型创建一个JSON摘要，我们可以根据对话发送到订单系统。所以我们现在附加另一个系统消息，这是一个指令，我们说创建一个JSON的前一个食物订单的摘要，逐项列出每个项目的价格，字段应该是一个比萨饼，包括侧面，两个配料列表，三个饮料列表和四个侧面列表，最后是总价。



````python
messages =  context.copy()
messages.append(
{'role':'system', 'content':'create a json summary of the previous food order. Itemize the price for each item\
 The fields should be 1) pizza, include size 2) list of toppings 3) list of drinks, include size   4) list of sides include size  5)total price '},    
)
 #The fields should be 1) pizza, price 2) list of toppings 3) list of drinks, include size include price  4) list of sides include size include price, 5)total price '},    

response = get_completion_from_messages(messages, temperature=0)
print(response)

"""
Sure, here's a JSON summary of your order:

```

{

  "pizza": {

    "type": "意大利辣香肠披萨",

    "size": "中号",

    "price": 12.95

  },

  "toppings": [

    {

      "type": "加拿大培根",

      "price": 3.50

    },

    {

      "type": "蘑菇",

      "price": 1.50

    },

    {

      "type": "彩椒",

      "price": 1.00

    }

  ],

  "drinks": [

    {

      "type": "可乐",

      "size": "中杯",

      "price": 3.00

    }

  ],

  "sides": [],

  "total_price": 18.95

}

```
"""
````

你也可以在这里使用用户消息，这不一定是系统消息。所以让我们执行这个。注意在这种情况下，我们使用较低的温度，因为对于这些任务，我们希望输出是相当可预测的。对于会话代理，您可能希望使用更高的温度，但是在这种情况下，我可能也会使用更低的温度，因为对于客户的助手聊天机器人，您可能希望输出也更可预测。

因此，这里我们有我们的订单摘要，因此如果我们愿意，我们可以将其提交给订单系统。现在我们有了它，您已经构建了自己的订单聊天机器人。请随意自定义它，并使用系统消息来改变聊天机器人的行为，并让它充当具有不同知识的不同角色。

# 收尾 / Conclusion

![](img/7e89bc14dabf766696e30ae7a1480919.png)

[prompt_eng_09_conclusion.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GlqsbH9t0oECj5xKDOQcI7kSnLg)

祝贺你完成了这个简短的课程。

总之，在这个简短的课程中，你已经学习了

+   提示的两个关键原则。
    1. 写清楚具体的说明，
    2. 在合适的时候，给模型时间思考。
+   你还学习了迭代提示开发，以及如何拥有一个适合你的应用程序的提示过程是关键。
    1. 我们学习了一些对许多应用程序有用的大型语言模型的功能，特别是总结、推断、转换和扩展。
    2. 你还看到了如何构建一个自定义聊天机器人。

这是你在一个简短的课程中学到的很多东西，我希望你喜欢阅读这些材料。我们希望你能想出一些你现在可以自己构建的应用程序的想法。请去试试这个，让我们知道你想出了什么。没有应用程序太小，从一个非常小的项目开始是很好的，也许有点实用，或者它根本没有用，只是一些有趣的东西。是的，我发现玩这些模型真的很有趣，所以去玩吧！我同意，这是一个很好的周末活动，从经验来看。

嗯，只是，你知道，请利用你第一个项目的经验来构建一个更好的第二个项目，你知道，甚至可能是一个更好的第三个项目，等等。这就是我自己使用这些模型随着时间的推移而成长的方式。或者如果你已经有了一个更大项目的想法，就去做吧。你知道，作为提醒，这些大型语言模型是一项非常强大的技术，所以不言而喻，我们要求你负责任地使用它们，请只构建会产生积极影响的东西。

是的，我完全同意。我认为在这个时代，构建AI系统的人可以对其他人产生巨大的影响。所以我们所有人都负责任地使用这些工具比以往任何时候都更加重要。嗯，我认为构建基于大型语言模型的应用程序现在只是一个非常令人兴奋和不断发展的领域。现在你已经完成了这门课程，我认为你现在拥有丰富的知识，可以让你构建今天很少有人知道如何去做的东西。所以，我希望你也能帮助我们传播这个，并鼓励其他人也参加这门课程。最后，我希望你在学习这门课程时玩得开心，我要感谢你完成了这门课程。以斯拉和我都期待听到你建造的惊人的东西。

对关于ChatGPT更高阶的用法，感兴趣可以加入交流群

关于进阶的提示词工程，最近也弄了一个👸号，会定期发一些相关的内容

喜欢我的文章也可以关注我的知乎，帮忙多多转发，谢谢了！

如何用ChatGPT做内容安全审核？

AI-Based 应用是如何取代现有软件的？

如何做一个让ChatGPT读任意多网页和文件的设计？

![](img/de5f451053221462ef995ab5b4f8952e.png)

![](img/b77827ca8bd37a8f4e44670b46c9c011.png)
