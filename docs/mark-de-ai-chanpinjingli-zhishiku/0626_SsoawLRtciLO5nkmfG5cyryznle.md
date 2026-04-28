# The rise of "context engineering"

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/UXI8dqalooNrCPxch58cBw5En1e](https://qqs7y1hozd1.feishu.cn/docx/UXI8dqalooNrCPxch58cBw5En1e)

# “上下文工程”的兴起

上下文工程是构建动态系统，以正确的格式提供正确的信息和工具，使大语言模型（LLM）能够合理地完成任务。

大多数情况下，当智能体表现不可靠时，其根本原因是没有向模型传达适当的上下文、指令和工具。

大语言模型（LLM）应用正从单一提示向更复杂、动态的智能体系统演变。因此，上下文工程正成为AI工程师可培养的最重要技能。

## 什么是上下文工程？

上下文工程是构建动态系统，以正确的格式提供正确的信息和工具，使大语言模型（LLM）能够合理地完成任务。

这就是我喜欢的定义，它建立在托比·卢特克、安库尔·戈亚尔和瓦尔登·严最近对这个问题的看法之上。让我们来剖析一下。

## 上下文工程是一个系统

复杂的智能体可能从多个来源获取上下文。上下文可以来自应用程序的开发者、用户、之前的交互、工具调用或其他外部数据。将这些整合在一起涉及一个复杂的系统。

## 这个系统是动态的

许多这类上下文信息可以动态生成。因此，构建最终提示的逻辑也需要是动态的，而不仅仅是一个静态提示。

## 你需要正确的信息

能动系统表现不佳的一个常见原因是它们没有合适的上下文。大语言模型无法读心——你需要给它们提供正确的信息。输入垃圾，输出垃圾。

## 你需要合适的工具

大语言模型（LLM）并不总是能够仅根据输入就解决任务。在这些情况下，如果您希望大语言模型有能力做到这一点，就需要确保它拥有合适的工具。这些工具可以是用于查找更多信息、采取行动或介于两者之间的任何工具。为大语言模型提供合适的工具与提供合适的信息同样重要。

## 格式很重要

就像与人类交流一样，你与大语言模型（LLMs）的交流方式很重要。一条简短但描述性强的错误消息比一大块JSON数据更有用。这同样适用于工具。确保大语言模型能够使用工具时，工具的输入参数非常重要。

## 它能合理地完成这项任务吗？

当你思考上下文工程时，这是一个非常值得探讨的问题。它强化了一个观点，即大语言模型（LLMs）并非读心术大师——你需要为它们的成功做好准备。这也有助于区分失败模式。是因为你没有给它正确的信息或工具才导致失败吗？还是它已经掌握了所有正确信息，只是搞砸了？这些失败模式有截然不同的解决方法。

## 为什么上下文工程很重要

当智能体系统出错时，很大程度上是因为大语言模型（LLM）出了问题。从第一性原理思考，大语言模型出错有两个原因：

基础模型刚刚出错了，它还不够好

基础模型没有被传递适当的上下文来生成良好的输出

通常（尤其是随着模型性能提升），模型错误更多是由第二个原因造成的。传递给模型的上下文可能由于以下几个原因而存在问题：

模型做出正确决策所需的上下文缺失。模型不是读心术者。如果你不提供正确的上下文，它们就不会知道其存在。

上下文格式设置不佳。就像人类一样，沟通很重要！向模型传递数据时的格式绝对会影响其响应方式

上下文工程与提示工程有何不同？

为什么从“提示词”转向“上下文”？早期，开发者专注于巧妙地组织提示词，以诱使得到更好的回答。但随着应用程序变得越来越复杂，向AI提供完整且结构化的上下文远比任何巧妙的措辞重要得多，这一点正变得越来越明显。

我还认为提示工程是上下文工程的一个子集。即使你掌握了所有的上下文信息，如何在提示中组织这些信息仍然至关重要。不同之处在于，你设计提示不是为了与某一组输入数据配合良好，而是为了处理一组动态数据并对其进行适当格式化。

我还想强调的是，上下文的一个关键部分通常是关于大语言模型（LLM）应如何表现的核心指令。这往往是提示工程的一个关键部分。你会说，为智能体应如何表现提供清晰、详细的指令是上下文工程还是提示工程？我认为两者都有一点。

## 情境工程示例

良好上下文工程的一些基本示例包括：

工具使用：确保如果智能体需要访问外部信息，它有能够访问这些信息的工具。当工具返回信息时，其格式会被设置为最易于大语言模型（LLM）理解的方式

短期记忆：如果对话持续了一段时间，就创建一个对话摘要，并在未来使用该摘要。

长期记忆：如果用户在之前的对话中表达过偏好，能够获取该信息。

提示工程：关于智能体应如何表现的指令在提示中被清晰地列举出来。

检索：在调用大语言模型（LLM）之前动态获取信息并将其插入到提示中。

LangGraph如何实现上下文工程

当我们构建LangGraph时，我们的目标是使其成为最具可控性的智能体框架。这也使其能够完美地实现上下文工程。

有了LangGraph，你可以掌控一切。你决定运行哪些步骤。你确切地决定输入到你的大语言模型（LLM）中的内容。你决定将输出结果存储在哪里。你掌控一切。

这使您能够进行所有您想要的上下文工程。代理抽象（大多数其他代理框架都强调这一点）的缺点之一是它们限制了上下文工程。可能存在一些情况，您无法确切地改变输入到LM中的内容，或者无法确切地改变事先运行的步骤。

旁注：德克斯·霍西（Dex Horthy）的《12要素智能体》是一本非常值得一读的书。其中很多观点都与上下文工程相关（如“掌控你的提示”“掌控你的上下文构建”等）。本博客的标题图片也取自德克斯。我们非常欣赏他阐述该领域重要内容的方式。

## LangSmith如何助力上下文工程

LangSmith是我们的大语言模型（LLM）应用可观测性和评估解决方案。LangSmith的关键特性之一是能够追踪你的智能体调用。尽管在我们开发LangSmith时，“上下文工程”这个术语还不存在，但它恰如其分地描述了这种追踪的作用。

LangSmith 让您查看代理中发生的所有步骤。这使您能够了解为收集发送到 LLM 的数据而执行了哪些步骤。

LangSmith让您查看大语言模型（LLM）的确切输入和输出。这使您能够确切了解进入大语言模型的内容——它所拥有的数据以及数据的格式。然后，您可以调试这些内容是否包含任务所需的所有相关信息。这包括大语言模型可以访问哪些工具——这样您就可以调试是否为它提供了完成手头任务所需的适当工具

## 沟通就是你所需要的一切

几个月前，我写了一篇名为"沟通是你所需要的一切"的博客。主要观点是，与大语言模型（LLM）进行沟通很困难，且没有得到足够的重视，而这往往是许多智能体错误的根源。其中很多问题都与上下文工程有关！

上下文工程并非新的概念——智能体开发者在过去一两年一直在做这件事。它是一个新术语，恰如其分地描述了一项日益重要的技能。我们将撰写并分享更多关于这个主题的内容。我们认为我们开发的许多工具（LangGraph、LangSmith）都是为实现上下文工程而精心打造的，因此我们很高兴看到对这一领域的重视开始兴起。

### 原文：

Context engineering is building dynamic systems to provide the right information and tools in the right format such that the LLM can plausibly accomplish the task.

Most of the time when an agent is not performing reliably the underlying cause is that the appropriate context, instructions and tools have not been communicated to the model.

LLM applications are evolving from single prompts to more complex, dynamic agentic systems. As such, context engineering is becoming the most important skill an AI engineer can develop.

## What is context engineering?

Context engineering is building dynamic systems to provide the right information and tools in the right format such that the LLM can plausibly accomplish the task.

This is the definition that I like, which builds upon recent takes on this from Tobi Lutke, Ankur Goyal, and Walden Yan. Let’s break it down.

## Context engineering is a system

Complex agents likely get context from many sources. Context can come from the developer of the application, the user, previous interactions, tool calls, or other external data. Pulling these all together involves a complex system.

## This system is dynamic

Many of these pieces of context can come in dynamically. As such, the logic for constructing the final prompt needs to be dynamic as well. It is not just a static prompt.

## You need the right information

A common reason agentic systems don’t perform is they just don’t have the right context. LLMs cannot read minds - you need to give them the right information. Garbage in, garbage out.

## You need the right tools

It may not always be the case that the LLM will be able to solve the task just based solely on the inputs. In these situations, if you want to empower the LLM to do so, you will want to make sure that it has the right tools. These could be tools to look up more information, take actions, or anything in between. Giving the LLM the right tools is just as important as giving it the right information.

## The format matters

Just like communicating with humans, how you communicate with LLMs matters. A short but descriptive error message will go a lot further a large JSON blob. This also applies to tools. What the input parameters to your tools are matters a lot when making sure that LLMs can use them.

## Can it plausibly accomplish the task?

This is a great question to be asking as you think about context engineering. It reinforces that LLMs are not mind readers - you need to set them up for success. It also helps separate the failure modes. Is it failing because you haven’t given it the right information or tools? Or does it have all the right information and it just messed up? These failure modes have very different ways to fix them.

## Why is context engineering important

When agentic systems mess up, it’s largely because an LLM messes. Thinking from first principles, LLMs can mess up for two reasons:

The underlying model just messed up, it isn’t good enough

The underlying model was not passed the appropriate context to make a good output

More often than not (especially as the models get better) model mistakes are caused more by the second reason. The context passed to the model may be bad for a few reasons:

There is just missing context that the model would need to make the right decision. Models are not mind readers. If you do not give them the right context, they won’t know it exists.

The context is formatted poorly. Just like humans, communication is important! How you format data when passing into a model absolutely affects how it responds

How is context engineering different from prompt engineering?

Why the shift from “prompts” to “context”? Early on, developers focused on phrasing prompts cleverly to coax better answers. But as applications grow more complex, it’s becoming clear that providing complete and structured context to the AI is far more important than any magic wording.

I would also argue that prompt engineering is a subset of context engineering. Even if you have all the context, how you assemble it in the prompt still absolutely matters. The difference is that you are not architecting your prompt to work well with a single set of input data, but rather to take a set of dynamic data and format it properly.

I would also highlight that a key part of context is often core instructions for how the LLM should behave. This is often a key part of prompt engineering. Would you say that providing clear and detailed instructions for how the agent should behave is context engineering or prompt engineering? I think it’s a bit of both.

## Examples of context engineering

Some basic examples of good context engineering include:

Tool use: Making sure that if an agent needs access to external information, it has tools that can access it. When tools return information, they are formatted in a way that is maximally digestable for LLMs

Short term memory: If a conversation is going on for a while, creating a summary of the conversation and using that in the future.

Long term memory: If a user has expressed preferences in a previous conversation, being able to fetch that information.

Prompt Engineering: Instructions for how an agent should behave are clearly enumerated in the prompt.

Retrieval: Fetching information dynamically and inserting it into the prompt before calling the LLM.

How LangGraph enables context engineering

When we built LangGraph, we built it with the goal of making it the most controllable agent framework. This also allows it to perfectly enable context engineering.

With LangGraph, you can control everything. You decide what steps are run. You decide exactly what goes into your LLM. You decide where you store the outputs. You control everything.

This allows you do all the context engineering you desire. One of the downsides of agent abstractions (which most other agent frameworks emphasize) is that they restrict context engineering. There may be places where you cannot change exactly what goes into the LLM, or exactly what steps are run beforehand.

Side note: a very good read is Dex Horthy's "12 Factor Agents". A lot of the points there relate to context engineering ("own your prompts", "own your context building", etc). The header image for this blog is also taken from Dex. We really enjoy the way he communicates about what is important in the space.

## How LangSmith helps with context engineering

LangSmith is our LLM application observability and evals solution. One of the key features in LangSmith is the ability to trace your agent calls. Although the term "context engineering" didn't exist when we built LangSmith, it aptly describes what this tracing helps with.

LangSmith lets you see all the steps that happen in your agent. This lets you see what steps were run to gather the data that was sent into the LLM.

LangSmith lets you see the exact inputs and outputs to the LLM. This lets you see exactly what went into the LLM - the data it had and how it was formatted. You can then debug whether that contains all the relevant information that is needed for the task. This includes what tools the LLM has access to - so you can debug whether it's been given the appropriate tools to help with the task at hand

## Communication is all you need

A few months ago I wrote a blog called "Communication is all you need". The main point was that communicating to the LLM is hard, and not appreciated enough, and often the root cause of a lot of agent errors. Many of these points have to do with context engineering!

Context engineering isn't a new idea - agent builders have been doing it for the past year or two. It's a new term that aptly describes an increasingly important skill. We'll be writing and sharing more on this topic. We think a lot of the tools we've built (LangGraph, LangSmith) are perfectly built to enable context engineering, and so we're excited to see the emphasis on this take off.