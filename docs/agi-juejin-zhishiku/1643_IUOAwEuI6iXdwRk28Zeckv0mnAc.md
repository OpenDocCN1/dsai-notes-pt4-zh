# 是否应该使用 LangChain （或者llamaindex）来构建我们的AI应用

> 来源：[https://agijuejin.feishu.cn/docx/KnGodscBEo87BoxHSUtcevCOnBT](https://agijuejin.feishu.cn/docx/KnGodscBEo87BoxHSUtcevCOnBT)

> 作者：北方的郎

> 原文：https://zhuanlan.zhihu.com/p/705234537

其实最近我在开发LLM应用的时候，也经常碰到这个困惑，有必要用LangChain么？很多时候感觉用了langchain比直接用post或者LLM 厂商的api也差不了多少，反而要引入不少新东西，把代码搞复杂化了。今天看到Fabian Both写的 “why we no longer use LangChain for building our AI agents”，感觉很多点说到我的心坎上。原文链接：

[https://www.octomind.dev/blog/why-we-no-longer-](https://link.zhihu.com/?target=https%3A//www.octomind.dev/blog/why-we-no-longer-use-langchain-for-building-our-ai-agents)use-langchain-for-building-our-ai-agents

翻译的过程中有一些小调整。

## 当抽象弊大于利时：在生产中使用 LangChain 的经验教训以及我们应该做什么

![](img/dee7086fa358217db18355929240db78.png)

在 Octomind，我们使用具有多个 LLM 的 AI 代理来自动创建和修复 Playwright 中的端到端测试。直到几个月前，我们用LangChain框架做到了。

在这篇文章中，我将分享我们与 LangChain 的斗争，以及为什么用模块化构建块替换其严格的高级抽象可以简化我们的代码库，并使我们的团队更快乐、更高效。

## 背景故事

我们在生产中使用 LangChain 已超过 12 个月，从 2023 年初开始，然后于 2024 年将其删除。

LangChain似乎是我们2023年的最佳选择。它拥有一系列令人印象深刻的组件和工具，并且受欢迎程度飙升。它承诺“让开发人员能够在一个下午的时间内将一个想法转变为工作代码。但随着我们的要求变得更加复杂，问题开始浮出水面，使 LangChain 成为摩擦的来源，而不是生产力的来源。

随着它的不灵活性开始显现出来，我们很快发现自己正在深入研究 LangChain 的内部结构，以改善我们系统的底层行为。但由于 LangChain 有意从您那里抽象出如此多的细节，因此编写我们需要的较低级别的代码通常并不容易或不可能。

## 成为早期框架的危险

人工智能和LLM正在快速变化的领域，每周都会出现新的概念和想法。因此，当围绕多种新兴技术创建像LangChain这样的框架时，设计经得起时间考验的抽象是非常困难的。

我确信，如果我当时尝试构建像LangChain这样的框架，我不会做得更好。错误很容易事后指出，本文的目的并不是不公平地批评浪链的核心开发人员或其贡献者。每个人都在尽力而为。

精心设计的抽象是很困难的——即使需求被很好地理解了。但是，当您在这种不断变化的状态下对组件（例如代理）进行建模时，更安全的选择是仅对较低级别的构建块使用抽象。

## LangChain抽象的问题

当我们的简单需求与其使用假设相一致时，LangChain 起初很有帮助。但它的高级抽象很快就使我们的代码变得更难以理解并且难以维护。当我们的团队开始花费与构建功能一样多的时间来理解和调试 LangChain 时，这不是一个好兆头。

LangChain 的抽象方法的问题可以通过这个将英语单词翻译成意大利语的小例子来证明。

以下是仅使用 OpenAI 包的 Python 示例：

```
1from openai import OpenAI
2
3client = OpenAI(api_key=&quot;&lt;your_api_key&gt;&quot;)
4text = &quot;hello!&quot;
5language = &quot;Italian&quot;
6
7messages = [
8    {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;You are an expert translator&quot;},
9    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: f&quot;Translate the following from English into {language}&quot;},
10    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: f&quot;{text}&quot;},
11]
12
13response = client.chat.completions.create(model=&quot;gpt-4o&quot;, messages=messages)
14result = response.choices[0].message.content
```

这是易于理解的简单代码，包含一个类和一个函数调用。其余的是标准Python。

我们来对比一下LangChain的版本：

```
1from langchain_openai import ChatOpenAI
2from langchain_core.output_parsers import StrOutputParser
3from langchain_core.prompts import ChatPromptTemplate
4
5os.environ[&quot;OPENAI_API_KEY&quot;] = &quot;&lt;your_api_key&gt;&quot;
6text = &quot;hello!&quot;
7language = &quot;Italian&quot;
8
9
10prompt_template = ChatPromptTemplate.from_messages(
11    [(&quot;system&quot;, &quot;You are an expert translator&quot;),
12     (&quot;user&quot;, &quot;Translate the following from English into {language}&quot;),
13     (&quot;user&quot;, &quot;{text}&quot;)]
14)
15
16parser = StrOutputParser()
17chain = prompt_template | model | parser
18result = chain.invoke({&quot;language&quot;: language, &quot;text&quot;: text})
```

代码的作用大致相同，但相似之处仅此而已。

我们现在有三个类和四个函数调用。但最令人担忧的是三个新抽象的引入：

+   提示模板：向 LLM 提供提示

+   输出解析器：处理 LLM 的输出

+   Chain：LangChain 的“LCEL 语法”覆盖 Python 的 | 运算符

LangChain 所取得的一切只是增加了代码的复杂性，而没有带来任何明显的好处。

这段代码可能适合早期原型。但对于生产使用，必须合理理解每个组件，这样在实际使用条件下它才不会意外地让你崩溃。您必须遵守给定的数据结构并围绕这些抽象设计您的应用程序。

让我们看一下 Python 中的另一个抽象比较，这次是从 API 获取 JSON。

使用内置的http包：

```
import http.client
import json
conn = http.client.HTTPSConnection(&quot;api.example.com&quot;)
conn.request(&quot;GET&quot;, &quot;/data&quot;)
response = conn.getresponse()
data = json.loads(response.read().decode())
conn.close()
```

使用请求包：

```
import requests
response = requests.get(&quot;/data&quot;)
data = response.json()
```

胜利者是显而易见的。这就是一个好的抽象的感觉。

当然，这些都是微不足道的例子。但我的观点是，好的抽象可以简化代码并减少理解代码所需的认知负担。

LangChain 试图通过向您隐藏细节，用更少的代码做更多的事情，从而让您的生活更轻松。但当这是以简单性和灵活性为代价时，抽象就失去了价值。

LangChain 还习惯于在其他抽象之上使用抽象，因此您经常被迫以嵌套抽象的方式进行思考，以了解如何正确使用 API。这不可避免地导致理解巨大的堆栈跟踪并调试您没有编写的内部框架代码，而不是实现新功能。

## LangChain对我们开发团队的影响

我们的应用程序大量使用人工智能代理来执行不同类型的任务，例如测试用例发现、Playwright 测试生成和自动修复。

当我们想要从具有单个顺序代理的架构转向更复杂的架构时，LangChain 是限制因素。例如，生成子代理并让它们与原始代理交互。或者多个专业代理相互交互。

在另一个实例中，我们需要根据业务逻辑和LLM的输出，动态更改代理可以访问的工具的可用性。但LangChain没有提供从外部观察代理状态的方法，导致我们缩小了实现范围以适应LangChain代理可用的有限功能。

一旦我们删除了它，我们就不再需要将我们的需求转化为浪链合适的解决方案。我们可以只编码。

那么，如果不使用 LangChain，应该使用什么框架呢？也许您根本不需要框架。

## 您需要一个构建人工智能应用程序的框架吗？

LangChain 很早就通过提供 LLM 功能来帮助我们，这样我们就可以专注于构建我们的应用程序。但事后看来，如果没有框架，我们的长期前景会更好。

LangChain 的一长串组件给人的印象是构建一个由 LLM 驱动的应用程序很复杂。但大多数应用程序需要的核心组件通常是：

+   LLM交流客户

+   函数/函数调用工具

+   RAG 向量数据库

+   用于追踪、评估等的可观察性平台。

其余的要么是这些组件的助手（例如矢量数据库的分块和嵌入），要么是常规应用程序任务，例如通过数据持久性和缓存管理文件和应用程序状态。

如果你在没有框架的情况下开始你的人工智能开发之旅，是的，你需要更长的时间来组合你自己的工具箱，并且需要更多的前期学习和研究。但这是值得花的时间，对您和您的应用程序的未来来说是一项值得的投资，因为您正在学习您将要操作的领域的基础知识。

在大多数情况下，您对LLM的使用将是简单明了的。您将主要编写顺序代码、迭代提示以及提高输出的质量和可预测性。大多数任务可以通过简单的代码和相对较小的外部包集合来实现。

即使使用Agents，您也不可能做太多超出预定顺序流中的简单Agent到Agent通信以及用于处理Agent状态及其响应的业务逻辑的事情。您不需要框架来实现这一点。

虽然Agents领域正在迅速发展，并带来令人兴奋的可能性和有趣的用例，但我们建议在Agent使用模式固化时暂时保持简单。

## 使用building blocks保持快速和精益

假设您没有将垃圾代码投入生产，那么团队创新和迭代的速度是衡量成功的最重要指标。人工智能领域的许多发展都是由实验和原型设计驱动的。

但框架通常是为了根据既定的使用模式强制实施结构而设计的——这是 LLM 支持的应用程序尚不具备的。必须将新想法转化为特定于框架的代码，这限制了迭代速度。

building blocks方法更喜欢简单的低级代码和精心挑选的外部包，从而保持架构的精简，以便开发人员可以将注意力集中在他们试图解决的问题上。

building blocks是一些简单的东西，你觉得可以被全面理解并且不太可能改变。例如，矢量数据库。它是一种已知类型的模块化组件，具有一组基本功能，因此可以轻松更换。您的代码库需要精简且适应性强，才能最大限度地提高您的学习速度以及从每个迭代周期中获得的价值。

. . .

我希望我深思熟虑、公正地描述了我们在 LangChain 方面面临的挑战，以及为什么完全放弃框架对我们的团队非常有利。

我们目前的策略是使用具有最少抽象的模块化构建块，这使我们能够更快、更顺畅地开发。

## 译者注

其实我们开发的各种LLM应用，主要的内容就是调用各种资源（Function，向量库，History等等），拼接出合适的Prompt并发给LLM，然后正确的进行处理（解析，Function Call，展示等等）。其实用不用langchain或者llamaindex都能做。

不过对于一些复杂场景langchain还是很有帮助的，而且各种LLM服务商、数据库的调用方式也都不尽相同，langchain的适配也能降低大家的工作量。还是要按照自己的实际情况来。