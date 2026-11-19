
# 深度：What We Learned from a Year of Building with LLMs

> 来源：[https://aigcreative.feishu.cn/docx/CnqjdtVGHoisxPxeVascMiy3nTe](https://aigcreative.feishu.cn/docx/CnqjdtVGHoisxPxeVascMiy3nTe)

## Part 1：tactical 战术部分

It’s an exciting time to build with large language models (LLMs). Over the past year, LLMs have become “good enough” for real-world applications. The pace of improvements in LLMs, coupled with a parade of demos on social media, will fuel an estimated $200B investment in AI by 2025. LLMs are also broadly accessible, allowing everyone, not just ML engineers and scientists, to build intelligence into their products. While the barrier to entry for building AI products has been lowered, creating those effective beyond a demo remains a deceptively difficult endeavor.

现在是使用大型语言模型进行构建的激动人心的时刻 (LLMs)。在过去的一年里，LLMs 对于实际应用来说已经“足够好”了。 LLMs 的改进步伐，加上社交媒体上的一系列演示，到 2025 年将推动人工智能领域的投资估计达到 200B 美元。LLMs 也可以广泛访问，让每个人，不仅仅是机器学习工程师和科学家，将智能融入到他们的产品中。尽管构建人工智能产品的准入门槛已经降低，但创建超越演示的有效产品仍然是一项看似困难的工作。

We’ve identified some crucial, yet often neglected, lessons and methodologies informed by machine learning that are essential for developing products based on LLMs. Awareness of these concepts can give you a competitive advantage against most others in the field without requiring ML expertise! Over the past year, the six of us have been building real-world applications on top of LLMs. We realized that there was a need to distill these lessons in one place for the benefit of the community.

我们已经确定了一些由机器学习提供的重要但经常被忽视的经验教训和方法，它们对于开发基于 LLMs 的产品至关重要。了解这些概念可以让您在无需 ML 专业知识的情况下，获得与该领域大多数其他人相比的竞争优势！在过去的一年里，我们六个人一直在 LLMs 之上构建实际应用程序。我们意识到，为了社区的利益，有必要将这些经验教训集中到一个地方。

We come from a variety of backgrounds and serve in different roles, but we’ve all experienced firsthand the challenges that come with using this new technology. Two of us are independent consultants who’ve helped numerous clients take LLM projects from initial concept to successful product, seeing the patterns determining success or failure. One of us is a researcher studying how ML/AI teams work and how to improve their workflows. Two of us are leaders on applied AI teams: one at a tech giant and one at a startup. Finally, one of us has taught deep learning to thousands and now works on making AI tooling and infrastructure easier to use. Despite our different experiences, we were struck by the consistent themes in the lessons we’ve learned, and we’re surprised that these insights aren’t more widely discussed.

我们来自不同的背景，担任不同的角色，但我们都亲身经历过使用这项新技术带来的挑战。我们中的两位是独立顾问，他们帮助众多客户将LLM项目从最初的概念转变为成功的产品，了解决定成功或失败的模式。我们中的一位是研究 ML/AI 团队如何工作以及如何改进其工作流程的研究人员。我们中有两位是应用人工智能团队的领导者：一位在科技巨头，一位在初创公司。最后，我们中的一位已经向数千人教授了深度学习，现在致力于使人工智能工具和基础设施更易于使用。尽管我们有不同的经历，但我们所学到的经验教训中的一致主题给我们留下了深刻的印象，而且令我们惊讶的是，这些见解没有得到更广泛的讨论。

Our goal is to make this a practical guide to building successful products around LLMs, drawing from our own experiences and pointing to examples from around the industry. We’ve spent the past year getting our hands dirty and gaining valuable lessons, often the hard way. While we don’t claim to speak for the entire industry, here we share some advice and lessons for anyone building products with LLMs.

我们的目标是借鉴我们自己的经验并指出整个行业的示例，使本书成为围绕 LLMs 构建成功产品的实用指南。在过去的一年里，我们亲力亲为，并获得了宝贵的经验教训，而且往往是通过艰难的方式。虽然我们并不声称代表整个行业，但在这里我们向使用 LLMs 构建产品的任何人分享一些建议和经验教训。

This work is organized into three sections: tactical, operational, and strategic. This is the first of three pieces. It dives into the tactical nuts and bolts of working with LLMs. We share best practices and common pitfalls around prompting, setting up retrieval-augmented generation, applying flow engineering, and evaluation and monitoring. Whether you’re a practitioner building with LLMs or a hacker working on weekend projects, this section was written for you. Look out for the operational and strategic sections in the coming weeks.

这项工作分为三个部分：战术部分、操作部分和战略部分。这是三件作品中的第一件。它深入探讨了使用 LLMs 的战术细节。我们分享有关提示、设置检索增强生成、应用流程工程以及评估和监控的最佳实践和常见陷阱。无论您是使用 LLMs 进行构建的从业者，还是从事周末项目的黑客，本节都是为您编写的。请留意未来几周的运营和战略部分。

Ready to delve dive in? Let’s go.

准备好深入研究了吗？我们走吧。

### Tactical 战术上的

In this section, we share best practices for the core components of the emerging LLM stack: prompting tips to improve quality and reliability, evaluation strategies to assess output, retrieval-augmented generation ideas to improve grounding, and more. We also explore how to design human-in-the-loop workflows. While the technology is still rapidly developing, we hope these lessons, the by-product of countless experiments we’ve collectively run, will stand the test of time and help you build and ship robust LLM applications.

在本节中，我们将分享新兴 LLM 堆栈核心组件的最佳实践：提高质量和可靠性的提示技巧、评估输出的评估策略、改善基础的检索增强生成想法等等。我们还探讨了如何设计人机交互工作流程。虽然该技术仍在快速发展，但我们希望这些经验教训（我们共同进行的无数实验的副产品）能够经受住时间的考验，并帮助您构建和交付强大的LLM应用程序。

#### Prompting 提示

We recommend starting with prompting when developing new applications. It’s easy to both underestimate and overestimate its importance. It’s underestimated because the right prompting techniques, when used correctly, can get us very far. It’s overestimated because even prompt-based applications require significant engineering around the prompt to work well.

我们建议在开发新应用程序时从提示开始。人们很容易低估和高估其重要性。它被低估了，因为正确的提示技巧如果使用得当，可以让我们走得更远。它被高估了，因为即使是基于提示的应用程序也需要围绕提示进行大量工程才能正常工作。

##### Focus on getting the most out of fundamental prompting techniques 专注于充分利用基本的提示技巧

A few prompting techniques have consistently helped improve performance across various models and tasks: n-shot prompts + in-context learning, chain-of-thought, and providing relevant resources.

一些提示技术始终有助于提高各种模型和任务的性能：n-shot提示+上下文学习、思维链和提供相关资源。

The idea of in-context learning via n-shot prompts is to provide the LLM with a few examples that demonstrate the task and align outputs to our expectations. A few tips:

通过 n-shot 提示进行上下文学习的想法是为 LLM 提供一些示例来演示任务并使输出符合我们的期望。一些提示：

+ 如果 `n` 太低，模型可能会过度锚定这些特定示例，从而损害其泛化能力。根据经验，目标是 `n` ≥ 5。不要害怕达到几十个。
+ 示例应代表预期的输入分布。如果您正在构建电影摘要器，请按照您期望在实践中看到的大致比例包含不同类型的样本。
+ 您不一定需要提供完整的输入输出对。在许多情况下，所需输出的示例就足够了。
+ 如果您使用的 LLM 支持工具使用，则您的 n-shot 示例还应使用您希望代理使用的工具。

In chain-of-thought (CoT) prompting, we encourage the LLM to explain its thought process before returning the final answer. Think of it as providing the LLM with a sketchpad so it doesn’t have to do it all in memory. The original approach was to simply add the phrase “Let’s think step-by-step” as part of the instructions. However, we’ve found it helpful to make the CoT more specific, where adding specificity via an extra sentence or two often reduces hallucination rates significantly. For example, when asking an LLM to summarize a meeting transcript, we can be explicit about the steps, such as:

在思维链（CoT）提示中，我们鼓励LLM在返回最终答案之前解释其思维过程。可以将其视为为 LLM 提供画板，这样它就不必在内存中完成所有操作。最初的方法是简单地添加“让我们一步一步思考”这句话作为说明的一部分。然而，我们发现让 CoT 更加具体是有帮助的，通过额外的一两句话来增加特异性通常会显着降低幻觉率。例如，当要求 LLM 总结会议记录时，我们可以明确说明步骤，例如：

+ 首先，在画板中列出关键决策、后续项目和相关所有者。
+ 然后，检查画板中的细节是否与文字记录相符。
+ 最后，将要点综合成简洁的总结。



Recently, [some doubt](https://arxiv.org/abs/2405.04776) has been cast on whether this technique is as powerful as believed. Additionally, there’s significant debate about exactly what happens during inference when chain-of-thought is used. Regardless, this technique is one to experiment with when possible.

最近，有人质疑这项技术是否像人们认为的那样强大。此外，关于使用思维链时推理过程中到底会发生什么，存在着很大的争论。无论如何，只要有可能，就可以尝试这种技术。

Providing relevant resources is a powerful mechanism to expand the model’s knowledge base, reduce hallucinations, and increase the user’s trust. Often accomplished via retrieval augmented generation (RAG), providing the model with snippets of text that it can directly utilize in its response is an essential technique. When providing the relevant resources, it’s not enough to merely include them; don’t forget to tell the model to prioritize their use, refer to them directly, and sometimes to mention when none of the resources are sufficient. These help “ground” agent responses to a corpus of resources.

提供相关资源是扩展模型知识库、减少幻觉并增加用户信任的强大机制。通常通过检索增强生成 (RAG) 来完成，为模型提供可直接在响应中使用的文本片段是一项重要技术。在提供相关资源时，仅仅包含它们是不够的；不要忘记告诉模型优先考虑它们的使用，直接引用它们，有时在资源不足时提及。这些有助于“接地”代理对资源库的响应。

## Structure your inputs and outputs 构建您的输入和输出

Structured input and output help models better understand the input as well as return output that can reliably integrate with downstream systems. Adding serialization formatting to your inputs can help provide more clues to the model as to the relationships between tokens in the context, additional metadata to specific tokens (like types), or relate the request to similar examples in the model’s training data.

结构化输入和输出有助于模型更好地理解输入和返回输出，从而能够与下游系统可靠地集成。向输入添加序列化格式有助于为模型提供更多关于上下文中标记之间的关系的线索、特定标记（如类型）的附加元数据，或将请求与模型训练数据中的类似示例相关联。

As an [example](https://www.researchgate.net/publication/371223615_SQL-PaLM_Improved_Large_Language_ModelAdaptation_for_Text-to-SQL), many questions on the internet about writing SQL begin by specifying the SQL schema. Thus, you may expect that effective prompting for Text-to-SQL should include structured schema definitions; indeed.

例如，互联网上许多有关编写 SQL 的问题都是从指定 SQL 模式开始的。因此，您可能期望文本到 SQL 的有效提示应包括结构化模式定义；的确。

Structured output serves a similar purpose, but it also simplifies integration into downstream components of your system. [Instructor](https://github.com/jxnl/instructor) and [Outlines](https://github.com/outlines-dev/outlines) work well for structured output. (If you’re importing an LLM API SDK, use Instructor; if you’re importing Huggingface for a self-hosted model, use Outlines.) Structured input expresses tasks clearly and resembles how the training data is formatted, increasing the probability of better output.

结构化输出具有类似的目的，但它也简化了与系统下游组件的集成。讲师和大纲非常适合结构化输出。 （如果您要导入 LLM API SDK，请使用 Instructor；如果您要为自托管模型导入 Huggingface，请使用 Outlines。）结构化输入可以清楚地表达任务，并且类似于训练数据的格式，增加更好输出的概率。

When using structured input, be aware that each LLM family has their own preferences. Claude prefers xml while GPT favors Markdown and JSON. With XML, you can even pre-fill Claude’s responses by providing a `response` tag like so.

使用结构化输入时，请注意每个 LLM 系列都有自己的偏好。 Claude 更喜欢 xml ，而 GPT 更喜欢 Markdown 和 JSON。使用 XML，您甚至可以通过提供像这样的 `response` 标签来预填充 Claude 的响应。

```python
messages = [
    {
        "role": "user",
        "content": """Extract the <name>, <size>, <price>, and <color> from this product description into your <response>.
                <description>The SmartHome Mini is a compact smart home assistant available in black or white for only $49.99. At just 5 inches wide, it lets you control   lights, thermostats, and other connected devices via voice or app—no matter where you                   place it in your home. This affordable little hub brings convenient hands-free control to your smart devices.

                </description>"""
    },
    {
        "role": "assistant",
        "content": "<response><name>"
    }
]
```

## Have small prompts that do one thing, and only one thing, well 有做一件事的小提示，而且只做一件事，好吧

A common anti-pattern/code smell in software is the “[God Object](https://en.wikipedia.org/wiki/God_object),” where we have a single class or function that does everything. The same applies to prompts too.

软件中常见的反模式/代码味道是“上帝对象”，其中我们有一个类或函数可以完成所有事情。这同样适用于提示。

A prompt typically starts simple: A few sentences of instruction, a couple of examples, and we’re good to go. But as we try to improve performance and handle more edge cases, complexity creeps in. More instructions. [Multi-step reasoning](https://www.godaddy.com/resources/news/llm-from-the-trenches-10-lessons-learned-operationalizing-models-at-godaddy#h-1-sometimes-one-prompt-isn-t-enough). Dozens of examples. Before we know it, our initially simple prompt is now a 2,000 token frankenstein. And to add injury to insult, it has worse performance on the more common and straightforward inputs! GoDaddy shared this challenge as their No. 1 lesson from building with LLMs.

提示通常一开始很简单：几句指令，几个例子，我们就可以开始了。但当我们尝试提高性能并处理更多边缘情况时，复杂性就会逐渐增加。更多指令。多步推理。几十个例子。不知不觉中，我们最初的简单提示现在变成了 2,000 个代币弗兰肯斯坦。雪上加霜的是，它在更常见和直接的输入上表现更差！ GoDaddy 分享了这一挑战，并将其作为与 LLMs 构建的第一课。

Just like how we strive (read: struggle) to keep our systems and code simple, so should we for our prompts. Instead of having a single, catch-all prompt for the meeting transcript summarizer, we can break it into steps to:

就像我们如何努力（阅读：努力）保持我们的系统和代码简单一样，我们的提示也应该如此。我们可以将其分解为以下步骤：

- Extract key decisions, action items, and owners into structured format

  将关键决策、行动项目和所有者提取为结构化格式

- Check extracted details against the original transcription for consistency

  检查提取的详细信息与原始转录的一致性

- Generate a concise summary from the structured details

  从结构化细节生成简明摘要

As a result, we’ve split our single prompt into multiple prompts that are each simple, focused, and easy to understand. And by breaking them up, we can now iterate and eval each prompt individually.

因此，我们将单个提示拆分为多个提示，每个提示都简单、重点突出且易于理解。通过分解它们，我们现在可以单独迭代和评估每个提示。

## Craft your context tokens 制作你的上下文标记

Rethink, and challenge your assumptions about how much context you actually need to send to the agent. Be like Michaelangelo, do not build up your context sculpture—chisel away the superfluous material until the sculpture is revealed. RAG is a popular way to collate all of the potentially relevant blocks of marble, but what are you doing to extract what’s necessary?

重新思考并挑战您关于实际需要向代理发送多少上下文的假设。像米开朗基罗一样，不要建立你的背景雕塑——凿掉多余的材料，直到雕塑被显露出来。 RAG 是一种整理所有潜在相关大理石块的流行方法，但是您正在做什么来提取必要的内容呢？

We’ve found that taking the final prompt sent to the model—with all of the context construction, and meta-prompting, and RAG results—putting it on a blank page and just reading it, really helps you rethink your context. We have found redundancy, self-contradictory language, and poor formatting using this method.

我们发现，将发送到模型的最终提示（包含所有上下文构建、元提示和 RAG 结果）放在空白页上并阅读它，确实可以帮助您重新思考您的上下文。使用这种方法我们发现了冗余、自相矛盾的语言和糟糕的格式。

The other key optimization is the structure of your context. Your bag-of-docs representation isn’t helpful for humans, don’t assume it’s any good for agents. Think carefully about how you structure your context to underscore the relationships between parts of it, and make extraction as simple as possible.

另一个关键的优化是上下文的结构。你的文档袋表示对人类没有帮助，不要认为它对代理有任何好处。仔细考虑如何构建上下文以强调其各部分之间的关系，并使提取尽可能简单。

## Information Retrieval/RAG 信息检索/RAG

Beyond prompting, another effective way to steer an LLM is by providing knowledge as part of the prompt. This grounds the LLM on the provided context which is then used for in-context learning. This is known as retrieval-augmented generation (RAG). Practitioners have found RAG effective at providing knowledge and improving output, while requiring far less effort and cost compared to finetuning. RAG is only as good as the retrieved documents’ relevance, density, and detail

在提示之外，另一种有效的引导 LLM 的方法是将知识作为提示的一部分提供。这使 LLM 以所提供的上下文为基础，然后用于上下文学习。这就是所谓的检索增强生成（RAG）。实践者发现 RAG 在提供知识和改进输出方面很有效，同时与微调相比所需的工作量和成本要少得多。RAG 的有效性取决于检索到的文档的相关性、密度和细节。



除了提示之外，引导 LLM 的另一种有效方法是提供知识作为提示的一部分。这将 LLM 基于所提供的上下文，然后用于上下文学习。这称为检索增强生成（RAG）。从业者发现 RAG 可以有效地提供知识和提高输出，同时与微调相比，所需的工作量和成本要少得多。RAG 的好坏取决于检索到的文档的相关性、密度和细节

## The quality of your RAG’s output is dependent on the quality of retrieved documents, which in turn can be considered along a few factors. RAG 输出的质量取决于检索文档的质量，而检索文档的质量又可以考虑几个因素。

The [first and most obvious metric](https://en.wikipedia.org/wiki/Discounted_cumulative_gain) is relevance. This is typically quantified via ranking metrics such as Mean Reciprocal Rank (MRR) or Normalized Discounted Cumulative Gain (NDCG). MRR evaluates how well a system places the first relevant result in a ranked list while NDCG considers the relevance of all the results and their positions. They measure how good the system is at ranking relevant documents higher and irrelevant documents lower. For example, if we’re retrieving user summaries to generate movie review summaries, we’ll want to rank reviews for the specific movie higher while excluding reviews for other movies.

第一个也是最明显的指标是相关性。这通常通过平均倒数排名 (MRR) 或标准化折扣累积增益 (NDCG) 等排名指标来量化。MRR 评估系统将第一个相关结果放置在排名列表中的程度，而 NDCG 则考虑所有结果及其位置的相关性。它们衡量系统在将相关文档排名较高、将不相关文档排名较低方面的表现。例如，如果我们检索用户摘要以生成电影评论摘要，我们将希望对特定电影的评论进行更高的排名，同时排除其他电影的评论。

Like traditional recommendation systems, the rank of retrieved items will have a significant impact on how the LLM performs on downstream tasks. To measure the impact, run a RAG-based task but with the retrieved items shuffled—how does the RAG output perform?

与传统推荐系统一样，检索到的项目的排名将对 LLM 在下游任务上的执行方式产生重大影响。要衡量影响，请运行基于 RAG 的任务，但对检索到的项目进行打乱，RAG 输出的执行情况如何？

Second, we also want to consider information density. If two documents are equally relevant, we should prefer one that’s more concise and has lesser extraneous details. Returning to our movie example, we might consider the movie transcript and all user reviews to be relevant in a broad sense. Nonetheless, the top-rated reviews and editorial reviews will likely be more dense in information.

其次，我们还要考虑信息密度。如果两份文件同样相关，我们应该更喜欢一份更简洁、无关细节更少的文件。回到我们的电影示例，我们可能会认为电影脚本和所有用户评论在广义上都是相关的。尽管如此，评分最高的评论和社论评论可能会包含更丰富的信息。

Finally, consider the level of detail provided in the document. Imagine we’re building a RAG system to generate SQL queries from natural language. We could simply provide table schemas with column names as context. But, what if we include column descriptions and some representative values? The additional detail could help the LLM better understand the semantics of the table and thus generate more correct SQL.

最后，考虑文档中提供的详细程度。想象一下，我们正在构建一个 RAG 系统来从自然语言生成 SQL 查询。我们可以简单地提供带有列名作为上下文的表模式。但是，如果我们包含列描述和一些代表性值怎么办？附加的详细信息可以帮助 LLM 更好地理解表的语义，从而生成更正确的 SQL。

## Don’t forget keyword search; use it as a baseline and in hybrid search. 不要忘记关键词搜索；将其用作基线和混合搜索。

Given how prevalent the embedding-based RAG demo is, it’s easy to forget or overlook the decades of research and solutions in information retrieval.

鉴于基于嵌入的 RAG 演示的普遍性，人们很容易忘记或忽略数十年的信息检索研究和解决方案。

Nonetheless, while embeddings are undoubtedly a powerful tool, they are not the be all and end all. First, while they excel at capturing high-level semantic similarity, they may struggle with more specific, keyword-based queries, like when users search for names (e.g., Ilya), acronyms (e.g., RAG), or IDs (e.g., `claude-3-sonnet`). Keyword-based search, such as BM25, are explicitly designed for this. And after years of keyword-based search, users have likely taken it for granted and may get frustrated if the document they expect to retrieve isn’t being returned.

尽管如此，虽然嵌入无疑是一个强大的工具，但它们并不是全部。首先，虽然它们擅长捕获高级语义相似性，但它们可能会难以处理更具体的、基于关键字的查询，例如当用户搜索名称（例如 Ilya）、首字母缩略词（例如 RAG）或 ID（例如 claude）时-3-十四行诗）。基于关键字的搜索，例如 BM25，就是专门为此设计的。经过多年基于关键字的搜索，用户可能已经认为这是理所当然的，如果他们期望检索的文档没有被返回，他们可能会感到沮丧。

> Vector embeddings do not magically solve search. In fact, the heavy lifting is in the step before you re-rank with semantic similarity search. Making a genuine improvement over BM25 or full-text search is hard.
>
> 向量嵌入并不能神奇地解决搜索问题。事实上，繁重的工作是在使用语义相似性搜索重新排名之前的步骤。对 BM25 或全文搜索进行真正的改进是很困难的。
>
> — Aravind Srinivas, CEO Perplexity.ai
>
> — Aravind Srinivas，Perplexity.ai 首席执行官

> We’ve been communicating this to our customers and partners for months now. Nearest Neighbor Search with naive embeddings yields very noisy results and you’re likely better off starting with a keyword-based approach.
>
> 几个月来，我们一直在向客户和合作伙伴传达这一点。使用朴素嵌入的最近邻搜索会产生非常嘈杂的结果，您最好从基于关键字的方法开始。
>
> — Beyang Liu, CTO Sourcegraph
>
> — Beyang Liu，首席技术官 Sourcegraph

Second, it’s more straightforward to understand why a document was retrieved with keyword search—we can look at the keywords that match the query. In contrast, embedding-based retrieval is less interpretable. Finally, thanks to systems like Lucene and OpenSearch that have been optimized and battle-tested over decades, keyword search is usually more computationally efficient.

其次，更容易理解为什么通过关键字搜索来检索文档——我们可以查看与查询匹配的关键字。相比之下，基于嵌入的检索的可解释性较差。最后，得益于 Lucene 和 OpenSearch 等经过数十年优化和实战测试的系统，关键字搜索通常在计算上更加高效。

In most cases, a hybrid will work best: keyword matching for the obvious matches, and [embeddings for synonyms, hypernyms, and spelling errors](https://www.shortwave.com/blog/deep-dive-into-worlds-smartest-email-ai/), as well as multimodality (e.g., images and text). Shortwave shared how they built their RAG pipeline, including query rewriting, keyword + embedding retrieval, and ranking.

在大多数情况下，混合效果最好：明显匹配的关键字匹配，同义词、上位词和拼写错误的嵌入，以及多模态（例如图像和文本）。Shortwave 分享了他们如何构建 RAG 管道，包括查询重写、关键字 + 嵌入检索和排名。

## Prefer RAG over fine-tuning for new knowledge 更喜欢 RAG 而不是新知识的微调

Both RAG and fine-tuning can be used to incorporate new information into LLMs and increase performance on specific tasks. Thus, which should we try first?

RAG 和微调都可用于将新信息合并到 LLMs 中并提高特定任务的性能。那么，我们应该先尝试哪一个呢？

[Recent research](https://arxiv.org/abs/2312.05934) [suggests that](https://arxiv.org/abs/2401.08406) RAG may have an edge. One study compared RAG against unsupervised fine-tuning (a.k.a. continued pre-training), evaluating both on a subset of MMLU and current events. They found that RAG consistently outperformed fine-tuning for knowledge encountered during training as well as entirely new knowledge. In another paper, they compared RAG against supervised fine-tuning on an agricultural dataset. Similarly, the performance boost from RAG was greater than fine-tuning, especially for GPT-4 (see Table 20 of the paper).

最近的研究表明 RAG 可能具有优势。一项研究将 RAG 与无监督微调（又称持续预训练）进行了比较，对 MMLU 的子集和当前事件进行了评估。他们发现，对于训练期间遇到的知识以及全新知识，RAG 的性能始终优于微调。在另一篇论文中，他们将 RAG 与农业数据集上的监督微调进行了比较。同样，RAG 的性能提升比微调更大，尤其是对于 GPT-4（参见论文的表 20）。

Beyond improved performance, RAG comes with several practical advantages too. First, compared to continuous pretraining or fine-tuning, it’s easier—and cheaper!—to keep retrieval indices up-to-date. Second, if our retrieval indices have problematic documents that contain toxic or biased content, we can easily drop or modify the offending documents.

除了改进的性能之外，RAG 还具有一些实际优势。首先，与持续预训练或微调相比，保持检索索引最新更容易且更便宜！其次，如果我们的检索索引中有包含有毒或有偏见内容的有问题的文档，我们可以轻松删除或修改有问题的文档。

In addition, the R in RAG provides finer grained control over how we retrieve documents. For example, if we’re hosting a RAG system for multiple organizations, by partitioning the retrieval indices, we can ensure that each organization can only retrieve documents from their own index. This ensures that we don’t inadvertently expose information from one organization to another.

此外，RAG 中的 R 对我们检索文档的方式提供了更细粒度的控制。例如，如果我们为多个组织托管 RAG 系统，通过对检索索引进行分区，我们可以确保每个组织只能从自己的索引中检索文档。这可确保我们不会无意中将一个组织的信息泄露给另一个组织。

## Long-context models won’t make RAG obsolete 长上下文模型不会让 RAG 过时

> With Gemini 1.5 providing context windows of up to 10M tokens in size, some have begun to question the future of RAG.
>
> 随着 Gemini 1.5 提供高达 10M 令牌大小的上下文窗口，一些人开始质疑 RAG 的未来。



> I tend to believe that Gemini 1.5 is significantly overhyped by Sora. A context window of 10M tokens effectively makes most of existing RAG frameworks unnecessary—you simply put whatever your data into the context and talk to the model like usual. Imagine how it does to all the startups/agents/LangChain projects where most of the engineering efforts goes to RAG 😅 Or in one sentence: the 10m context kills RAG. Nice work Gemini.
>
> 我倾向于认为 Sora 明显夸大了 Gemini 1.5。 10M 令牌的上下文窗口有效地使得大多数现有的 RAG 框架变得不必要——您只需将任何数据放入上下文中并像往常一样与模型对话即可。想象一下它对所有初创公司/代理/LangChain 项目的影响，其中大部分工程工作都花在 RAG 上😅 或者用一句话来说：10m 上下文杀死了 RAG。双子座干得好。
>
> — [Yao Fu](https://x.com/Francis_YAO_/status/1758935954189115714) — 付耀

While it’s true that long contexts will be a game-changer for use cases such as analyzing multiple documents or chatting with PDFs, the rumors of RAG’s demise are greatly exaggerated.

虽然长上下文确实会改变分析多个文档或与 PDF 聊天等用例的游戏规则，但有关 RAG 消亡的传言被严重夸大了。

First, even with a context window of 10M tokens, we’d still need a way to select information to feed into the model. Second, beyond the narrow needle-in-a-haystack eval, we’ve yet to see convincing data that models can effectively reason over such a large context. Thus, without good retrieval (and ranking), we risk overwhelming the model with distractors, or may even fill the context window with completely irrelevant information.

首先，即使有 10M 个 token 的上下文窗口，我们仍然需要一种方法来选择信息以输入模型。其次，除了狭隘的大海捞针评估之外，我们还没有看到令人信服的数据表明模型可以在如此大的背景下进行有效的推理。因此，如果没有良好的检索（和排名），我们就有可能用干扰因素淹没模型，甚至可能用完全不相关的信息填充上下文窗口。

Finally, there’s [cost](https://aws.amazon.com/ec2/instance-types/high-memory/). The Transformer’s inference cost scales quadratically (or linearly in both space and time) with context length. Just because there exists a model that could read your organization’s entire Google Drive contents before answering each question doesn’t mean that’s a good idea. Consider an analogy to how we use RAM: we still read and write from disk, even though there exist compute instances with RAM running into the tens of terabytes.

最后，还有成本。 Transformer 的推理成本随上下文长度呈二次方（或在空间和时间上呈线性）缩放。仅仅因为存在一个模型可以在回答每个问题之前读取您组织的整个 Google 云端硬盘内容，但这并不意味着这是一个好主意。考虑一下我们如何使用 RAM 的类比：即使存在 RAM 达到数十 TB 的计算实例，我们仍然从磁盘读取和写入。

So don’t throw your RAGs in the trash just yet. This pattern will remain useful even as context windows grow in size.

因此，暂时不要将 RAG 扔进垃圾桶。即使上下文窗口变大，这种模式仍然有用。

## Tuning and optimizing workflows 调整和优化工作流程

Prompting an LLM is just the beginning. To get the most juice out of them, we need to think beyond a single prompt and embrace workflows. For example, how could we split a single complex task into multiple simpler tasks? When is finetuning or caching helpful with increasing performance and reducing latency/cost? In this section, we share proven strategies and real-world examples to help you optimize and build reliable LLM workflows.

提示 LLM 只是开始。为了充分利用它们，我们需要超越单一提示并拥抱工作流程。例如，我们如何将一个复杂的任务拆分为多个简单的任务？微调或缓存何时有助于提高性能和减少延迟/成本？在本部分中，我们分享经过验证的策略和实际示例，以帮助您优化和构建可靠的LLM工作流程。

### Step-by-step, multi-turn “flows” can give large boosts. 循序渐进、多轮“流动”可以带来巨大的推动力。

We [already know](https://arxiv.org/abs/2401.08500) that by decomposing a single big prompt into multiple smaller prompts, we can achieve better results. An example of this is AlphaCodium: By switching from a single prompt to a multi-step workflow, they increased GPT-4 accuracy (`pass@5`) on CodeContests from 19% to 44%. The workflow includes:

我们已经知道，通过将单个大提示分解为多个较小的提示，我们可以获得更好的结果。 AlphaCodium 就是一个例子：通过从单一提示切换到多步骤工作流程，他们将 CodeContests 上的 GPT-4 准确率 (`pass@5`) 从 19% 提高到了 44%。工作流程包括：

- Reflecting on the problem 反思问题
- Reasoning on the public tests 公开测试的推理
- Generating possible solutions 生成可能的解决方案
- Ranking possible solutions 对可能的解决方案进行排名
- Generating synthetic tests 生成综合测试
- Iterating on the solutions on public and synthetic tests. 迭代公共测试和综合测试的解决方案。

Small tasks with clear objectives make for the best agent or flow prompts. It’s not required that every agent prompt requests structured output, but structured outputs help a lot to interface with whatever system is orchestrating the agent’s interactions with the environment.

具有明确目标的小任务是最好的代理或流程提示。并不要求每个代理提示都请求结构化输出，但结构化输出有助于与协调代理与环境交互的任何系统进行交互。

Some things to try

一些值得尝试的事情

- An explicit planning step, as tightly specified as possible. Consider having predefined plans to choose from (c.f. https://youtu.be/hGXhFa3gzBs?si=gNEGYzux6TuB1del). 明确的计划步骤，尽可能严格地指定。考虑有预定义的计划可供选择（参见 https://youtu.be/hGXhFa3gzBs?si=gNEGYzux6TuB1del）。
- Rewriting the original user prompts into agent prompts. Be careful, this process is lossy! 将原来的用户提示重写为代理提示。小心，这个过程是有损耗的！
- Agent behaviors as linear chains, DAGs, and State-Machines; different dependency and logic relationships can be more and less appropriate for different scales. Can you squeeze performance optimization out of different task architectures? 代理行为如线性链、DAG 和状态机；不同的依赖关系和逻辑关系可能或多或少适合不同的规模。您能否从不同的任务架构中挤出性能优化？
- Planning validations; your planning can include instructions on how to evaluate the responses from other agents to make sure the final assembly works well together. 规划验证；您的计划可以包括有关如何评估其他代理的响应的说明，以确保最终组装顺利进行。
- Prompt engineering with fixed upstream state—make sure your agent prompts are evaluated against a collection of variants of what may happen before. 具有固定上游状态的提示工程 - 确保根据之前可能发生的情况的变体集合来评估您的代理提示。

### Prioritize deterministic workflows for now 现在优先考虑确定性工作流程

While AI agents can dynamically react to user requests and the environment, their non-deterministic nature makes them a challenge to deploy. Each step an agent takes has a chance of failing, and the chances of recovering from the error are poor. Thus, the likelihood that an agent completes a multi-step task successfully decreases exponentially as the number of steps increases. As a result, teams building agents find it difficult to deploy reliable agents.

虽然人工智能代理可以动态地对用户请求和环境做出反应，但它们的不确定性使它们的部署面临挑战。代理采取的每一步都有可能失败，并且从错误中恢复的机会很小。因此，随着步骤数的增加，代理成功完成多步骤任务的可能性呈指数下降。因此，团队建设代理发现很难部署可靠的代理。

A promising approach is to have agent systems that produce deterministic plans which are then executed in a structured, reproducible way. In the first step, given a high-level goal or prompt, the agent generates a plan. Then, the plan is executed deterministically. This allows each step to be more predictable and reliable. Benefits include:

一种有前途的方法是让代理系统产生确定性计划，然后以结构化、可重复的方式执行。第一步，给定一个高级目标或提示，代理会生成一个计划。然后，确定性地执行该计划。这使得每个步骤都更加可预测和可靠。好处包括：

- Generated plans can serve as few-shot samples to prompt or finetune an agent. 生成的计划可以作为少量样本来提示或微调代理。
- Deterministic execution makes the system more reliable, and thus easier to test and debug. Furthermore, failures can be traced to the specific steps in the plan. 确定性执行使系统更加可靠，从而更容易测试和调试。此外，失败可以追溯到计划中的具体步骤。
- Generated plans can be represented as directed acyclic graphs (DAGs) which are easier, relative to a static prompt, to understand and adapt to new situations. 生成的计划可以表示为有向无环图 (DAG)，相对于静态提示，它更容易理解和适应新情况。

The most successful agent builders may be those with strong experience managing junior engineers because the process of generating plans is similar to how we instruct and manage juniors. We give juniors clear goals and concrete plans, instead of vague open-ended directions, and we should do the same for our agents too.

最成功的代理构建者可能是那些在管理初级工程师方面拥有丰富经验的人，因为制定计划的过程与我们指导和管理初级工程师的方式类似。我们给后辈明确的目标和具体的计划，而不是模糊的开放式指示，我们也应该为我们的经纪人做同样的事情。

In the end, the key to reliable, working agents will likely be found in adopting more structured, deterministic approaches, as well as collecting data to refine prompts and finetune models. Without this, we’ll build agents that may work exceptionally well some of the time, but on average, disappoint users which leads to poor retention.

最后，可靠、有效的代理的关键可能在于采用更加结构化、确定性的方法，以及收集数据来完善提示和微调模型。如果没有这一点，我们将构建的代理在某些时候可能表现得非常好，但平均而言，会让用户失望，从而导致留存率低。

### Getting more diverse outputs beyond temperature 获得除温度之外更多样化的输出



Suppose your task requires diversity in an LLM’s output. Maybe you’re writing an LLM pipeline to suggest products to buy from your catalog given a list of products the user bought previously. When running your prompt multiple times, you might notice that the resulting recommendations are too similar—so you might increase the `temperature` parameter in your LLM requests.

假设您的任务需要 LLM 输出的多样性。也许您正在编写一个 LLM 管道，根据用户之前购买的产品列表来建议从目录中购买的产品。多次运行提示时，您可能会注意到生成的建议过于相似，因此您可能会增加 LLM 请求中的温度参数。

Briefly, increasing the `temperature` parameter makes LLM responses more varied. At sampling time, the probability distributions of the next token become flatter, meaning that tokens which are usually less likely get chosen more often. Still, when increasing `temperature`, you may notice some failure modes related to output diversity. For example, some products from the catalog that could be a good fit may never be output by the LLM. The same handful of products might be overrepresented in outputs, if they are highly likely to follow the prompt based on what the LLM has learned at training time. If the `temperature` is too high, you may get outputs that reference nonexistent products (or gibberish!)

简而言之，增加温度参数会使 LLM 响应更加多样化。在采样时，下一个标记的概率分布变得更平坦，这意味着通常不太可能的标记被更频繁地选择。不过，当温度升高时，您可能会注意到一些与输出多样性相关的故障模式。例如，目录中一些可能非常合适的产品可能永远不会被 LLM 输出。如果相同的少数产品很可能基于 LLM 在训练时学到的内容而遵循提示，那么它们可能会在输出中出现过多。如果温度太高，您可能会得到引用不存在产品的输出（或乱码！）

In other words, increasing `temperature` does not guarantee that the LLM will sample outputs from the probability distribution you expect (e.g., uniform random). Nonetheless, we have other tricks to increase output diversity. The simplest way is to adjust elements within the prompt. For example, if the prompt template includes a list of items, such as historical purchases, shuffling the order of these items each time they’re inserted into the prompt can make a significant difference.

换句话说，升高温度并不能保证 LLM 将从您期望的概率分布（例如均匀随机）中采样输出。尽管如此，我们还有其他技巧来增加产出多样性。最简单的方法是调整提示中的元素。例如，如果提示模板包含项目列表（例如历史购买记录），则每次将这些项目插入提示时打乱它们的顺序可能会产生显着差异。

Additionally, keeping a short list of recent outputs can help prevent redundancy. In our recommended products example, by instructing the LLM to avoid suggesting items from this recent list, or by rejecting and resampling outputs that are similar to recent suggestions, we can further diversify the responses. Another effective strategy is to vary the phrasing used in the prompts. For instance, incorporating phrases like “pick an item that the user would love using regularly” or “select a product that the user would likely recommend to friends” can shift the focus and thereby influence the variety of recommended products.

此外，保留最近输出的简短列表有助于防止冗余。在我们推荐的产品示例中，通过指示 LLM 避免建议最近列表中的项目，或者通过拒绝和重新采样与最近建议类似的输出，我们可以进一步使响应多样化。另一个有效的策略是改变提示中使用的措辞。例如，合并诸如“选择用户喜欢经常使用的项目”或“选择用户可能会推荐给朋友的产品”之类的短语可以转移焦点，从而影响推荐产品的种类。

### Caching is underrated / 缓存被低估了

Caching saves cost and eliminates generation latency by removing the need to recompute responses for the same input. Furthermore, if a response has previously been guardrailed, we can serve these vetted responses and reduce the risk of serving harmful or inappropriate content.

缓存无需重新计算同一输入的响应，从而节省了成本并消除了生成延迟。此外，如果某个回复之前已受到限制，我们可以提供这些经过审查的回复，并降低提供有害或不当内容的风险。

[One straightforward approach](https://www.cnbc.com/2023/06/12/amazon-is-using-generative-ai-to-summarize-product-reviews.html) to caching is to use unique IDs for the items being processed, such as if we’re summarizing new articles or product reviews. When a request comes in, we can check to see if a summary already exists in the cache. If so, we can return it immediately; if not, we generate, guardrail, and serve it, and then store it in the cache for future requests.

一种直接的缓存方法是对正在处理的项目使用唯一的 ID，例如我们正在总结新文章或产品评论。当请求到来时，我们可以检查缓存中是否已存在摘要。如果是，我们可以立即返回它；如果没有，我们生成、保护并提供它，然后将其存储在缓存中以供将来的请求。

For more open-ended queries, we can borrow techniques from the field of search, which also leverages caching for open-ended inputs. Features like autocomplete and spelling correction also help normalize user input and thus increase the cache hit rate.

对于更多开放式查询，我们可以借用搜索领域的技术，该技术还利用缓存来实现开放式输入。自动完成和拼写纠正等功能也有助于规范用户输入，从而提高缓存命中率。

### When to fine-tune / 何时进行微调

We may have some tasks where even the most cleverly designed prompts fall short. For example, even after significant prompt engineering, our system may still be a ways from returning reliable, high-quality output. If so, then it may be necessary to finetune a model for your specific task.

我们可能会遇到一些任务，即使设计得最巧妙的提示也无法达到要求。例如，即使在大量的提示工程之后，我们的系统距离返回可靠、高质量的输出可能仍然有一段路要走。如果是这样，那么可能需要针对您的特定任务微调模型。

Successful examples include:

成功的例子包括：

+ [Honeycomb’s Natural Language Query Assistant](https://www.honeycomb.io/blog/introducing-query-assistant): Initially, the “programming manual” was provided in the prompt together with n-shot examples for in-context learning. While this worked decently, fine-tuning the model led to better output on the syntax and rules of the domain-specific language.

  Honeycomb 的自然语言查询助手：最初，提示中提供了“编程手册”以及用于上下文学习的 n-shot 示例。虽然这工作得很好，但对模型进行微调可以更好地输出特定领域语言的语法和规则。

+ [ReChat’s Lucy](https://www.youtube.com/watch?v=B_DMMlDuJB0): The LLM needed to generate responses in a very specific format that combined structured and unstructured data for the frontend to render correctly. Fine-tuning was essential to get it to work consistently.

  ReChat 的 Lucy：LLM 需要以非常特定的格式生成响应，该格式结合了结构化和非结构化数据，以便前端正确呈现。要使其始终如一地工作，微调至关重要。

Nonetheless, [while fine-tuning](https://eugeneyan.com/writing/synthetic/) can be effective, it comes with significant costs. We have to annotate fine-tuning data, finetune and evaluate models, and eventually self-host them. Thus, consider if the higher upfront cost is worth it. If prompting gets you 90% of the way there, then fine-tuning may not be worth the investment. However, if we do decide to fine-tune, to reduce the cost of collecting human annotated data, we can generate and finetune on synthetic data, or bootstrap on open-source data.

尽管如此，虽然微调可能是有效的，但它会带来巨大的成本。我们必须注释微调数据、微调和评估模型，并最终自行托管它们。因此，请考虑较高的前期成本是否值得。如果提示让你完成了 90% 的任务，那么微调可能就不值得投资。然而，如果我们确实决定进行微调，以降低收集人工注释数据的成本，我们可以生成并微调合成数据，或引导开源数据。

### Evaluation & Monitoring / 评估与监测

[Evaluating](https://twitter.com/eugeneyan/status/1701692908074873036) LLMs can be a minefield. The inputs and the outputs of LLMs are arbitrary text, and the tasks we set them to are varied. Nonetheless, rigorous and thoughtful evals are critical—it’s no coincidence that technical leaders at OpenAI work on evaluation and give feedback on individual evals.

评估 LLMs 可能是一个雷区。LLMs 的输入和输出是任意文本，我们设置它们的任务是多种多样的。尽管如此，严格且深思熟虑的评估至关重要——OpenAI 的技术领导者致力于评估并对个人评估提供反馈，这并非巧合。

Evaluating LLM applications invites a diversity of definitions and reductions: it’s simply unit testing, or it’s more like observability, or maybe it’s just data science. We have found all of these perspectives useful. In the following section, we provide some lessons we’ve learned about what is important in building evals and monitoring pipelines.

评估 LLM 应用程序需要各种各样的定义和简化：它只是单元测试，或者更像是可观察性，或者可能只是数据科学。我们发现所有这些观点都很有用。在下一节中，我们提供了一些关于构建评估和监控管道的重要内容的经验教训。

#### Create a few assertion-based unit tests from real input/output samples / 从真实的输入/输出样本创建一些基于断言的单元测试

Create unit tests (i.e., assertions) consisting of samples of inputs and outputs from production, with expectations for outputs based on at least three criteria. While three criteria might seem arbitrary, it’s a practical number to start with; fewer might indicate that your task isn’t sufficiently defined or is too open-ended, like a general-purpose chatbot. These unit tests, or assertions, should be triggered by any changes to the pipeline, whether it’s editing a prompt, adding new context via RAG, or other modifications. This write-up has an example of an assertion-based test for an actual use case.

创建由生产中的输入和输出样本组成的单元测试（即断言），并基于至少三个标准对输出进行预期。虽然这三个标准似乎是任意的，但它是一个实用的数字。较少可能表明您的任务没有充分定义或过于开放，就像通用聊天机器人一样。这些单元测试或断言应该由管道的任何更改触发，无论是编辑提示、通过 RAG 添加新上下文还是其他修改。这篇文章有一个针对实际用例的基于断言的测试示例。



## LLM-as-Judge 可以（在某种程度上）起作用，但这不是灵丹妙药

Consider beginning with assertions that specify phrases or ideas to either include or exclude in all responses. Also consider checks to ensure that word, item, or sentence counts lie within a range. For other kinds of generation, assertions can look different. Execution-evaluation is a powerful method for evaluating code-generation, wherein you run the generated code and determine that the state of runtime is sufficient for the user-request.

考虑从指定要在所有响应中包含或排除的短语或想法的断言开始。还要考虑进行检查以确保单词、项目或句子计数位于某个范围内。对于其他类型的生成，断言可能看起来有所不同。执行评估是评估代码生成的强大方法，您可以运行生成的代码并确定运行时的状态足以满足用户请求。

As an example, if the user asks for a new function named `foo`; then after executing the agent’s generated code, `foo` should be callable! One challenge in execution-evaluation is that the agent code frequently leaves the runtime in slightly different form than the target code. It can be effective to “relax” assertions to the absolute most weak assumptions that any viable answer would satisfy.

举个例子，如果用户请求一个名为 `foo` 的新函数；那么在执行代理生成的代码后， `foo` 应该是可调用的！执行评估中的一项挑战是代理代码经常以与目标代码略有不同的形式离开运行时。将断言“放松”到任何可行的答案都会满足的绝对最弱的假设是有效的。

Finally, using your product as intended for customers (i.e., “dogfooding”) can provide insight into failure modes on real-world data. This approach not only helps identify potential weaknesses, but also provides a useful source of production samples that can be converted into evals.

最后，按照客户的预期使用您的产品（即“dogfooding”）可以深入了解真实世界数据的故障模式。这种方法不仅有助于识别潜在的弱点，而且还提供了可转换为评估的生产样本的有用来源。

## LLM-as-Judge 可以（在某种程度上）起作用，但这不是灵丹妙药

LLM-as-Judge, where we use a strong LLM to evaluate the output of other LLMs, has been met with skepticism by some. (Some of us were initially huge skeptics.) Nonetheless, when implemented well, LLM-as-Judge achieves decent correlation with human judgements, and can at least help build priors about how a new prompt or technique may perform. Specifically, when doing pairwise comparisons (e.g., control vs. treatment), LLM-as-Judge typically gets the direction right though the magnitude of the win/loss may be noisy.

LLM-as-Judge，我们使用强大的 LLM 来评估其他 LLMs 的输出，但遭到了一些人的怀疑。 （我们中的一些人最初是非常怀疑的。）尽管如此，如果实施得当，LLM-as-Judge 与人类判断实现了良好的相关性，并且至少可以帮助建立关于新提示或技术如何执行的先验知识。具体来说，当进行成对比较（例如，对照与治疗）时，作为法官的LLM通常会得到正确的方向，尽管赢/输的幅度可能很嘈杂。

Here are some suggestions to get the most out of LLM-as-Judge:

以下是一些充分利用LLM作为法官的建议：

- Use pairwise comparisons: Instead of asking the LLM to score a single output on a Likert scale, present it with two options and ask it to select the better one. This tends to lead to more stable results.

使用成对比较：不要要求 LLM 在李克特量表上对单个输出进行评分，而是向它提供两个选项并要求它选择更好的一个。这往往会带来更稳定的结果。

- Control for position bias: The order of options presented can bias the LLM’s decision. To mitigate this, do each pairwise comparison twice, swapping the order of pairs each time. Just be sure to attribute wins to the right option after swapping!

控制立场偏差：提供的选项顺序可能会导致 LLM 的决策产生偏差。为了缓解这种情况，请将每个成对比较进行两次，每次交换对的顺序。请务必在交换后将胜利归因于正确的选项！

- Allow for ties: In some cases, both options may be equally good. Thus, allow the LLM to declare a tie so it doesn’t have to arbitrarily pick a winner.

允许联系：在某些情况下，两种选择可能同样好。因此，允许 LLM 宣布平局，这样它就不必任意选择获胜者。

- Use Chain-of-Thought: Asking the LLM to explain its decision before giving a final preference can increase eval reliability. As a bonus, this allows you to use a weaker but faster LLM and still achieve similar results. Because frequently this part of the pipeline is in batch mode, the extra latency from CoT isn’t a problem.

使用思路链：在给出最终偏好之前要求 LLM 解释其决定可以提高评估可靠性。作为奖励，这允许您使用较弱但更快的 LLM 并仍然获得类似的结果。由于管道的这一部分通常处于批处理模式，因此 CoT 带来的额外延迟不是问题。

- Control for response length: LLMs tend to bias toward longer responses. To mitigate this, ensure response pairs are similar in length.

控制响应长度：LLMs 倾向于偏向较长的响应。为了缓解这种情况，请确保响应对的长度相似。

One particularly powerful application of LLM-as-Judge is checking a new prompting strategy against regression. If you have tracked a collection of production results, sometimes you can rerun those production examples with a new prompting strategy, and use LLM-as-Judge to quickly assess where the new strategy may suffer.

LLM-as-Judge 的一个特别强大的应用是检查新的提示策略以防止回归。如果您跟踪了一系列生产结果，有时您可以使用新的提示策略重新运行这些生产示例，并使用 LLM-as-Judge 来快速评估新策略可能会受到影响的地方。

Here’s an example of a simple but effective approach to iterate on LLM-as-Judge, where we simply log the LLM response, judge’s critique (i.e., CoT), and final outcome. They are then reviewed with stakeholders to identify areas for improvement. Over three iterations, agreement with human and LLM improved from 68% to 94%!

下面是一个简单但有效的迭代 LLM-as-Judge 方法的示例，我们只需记录 LLM 响应、法官的批评（即 CoT）和最终结果。然后与利益相关者一起审查它们，以确定需要改进的领域。经过 3 次迭代，与人类和 LLM 的一致性从 68% 提高到 94%！

![](img/d481f47e3a35024bf99c3cc312f6b2c8.png)

LLM-as-Judge is not a silver bullet though. There are subtle aspects of language where even the strongest models fail to evaluate reliably. In addition, we’ve found that conventional classifiers and reward models can achieve higher accuracy than LLM-as-Judge, and with lower cost and latency. For code generation, LLM-as-Judge can be weaker than more direct evaluation strategies like execution-evaluation.

不过，LLM作为法官并不是灵丹妙药。语言有一些微妙的方面，即使是最强大的模型也无法可靠地评估。此外，我们发现传统的分类器和奖励模型可以比 LLM-as-Judge 实现更高的准确性，并且成本和延迟更低。对于代码生成，LLM-as-Judge 可能比更直接的评估策略（例如执行评估）更弱。

## “实习生测试” 用于评估生成内容

We like to use the following “intern test” when evaluating generations: If you took the exact input to the language model, including the context, and gave it to an average college student in the relevant major as a task, could they succeed? How long would it take?

在评估世代时，我们喜欢使用以下“实习生测试”：如果你将精确的输入（包括上下文）交给语言模型，并将其作为一项任务交给相关专业的普通大学生，他们能成功吗？需要多长时间？

If the answer is no because the LLM lacks the required knowledge, consider ways to enrich the context.

如果答案是否定的，因为 LLM 缺乏所需的知识，请考虑如何丰富上下文。

If the answer is no and we simply can’t improve the context to fix it, then we may have hit a task that’s too hard for contemporary LLMs.

如果答案是否定的，并且我们根本无法改善环境来解决这个问题，那么我们可能已经完成了一项对于当代LLMs来说太难的任务。

If the answer is yes, but it would take a while, we can try to reduce the complexity of the task. Is it decomposable? Are there aspects of the task that can be made more templatized?

如果答案是肯定的，但是需要一段时间，我们可以尝试降低任务的复杂性。可分解吗？任务的哪些方面可以更加模板化？

If the answer is yes, they would get it quickly, then it’s time to dig into the data. What’s the model doing wrong? Can we find a pattern of failures? Try asking the model to explain itself before or after it responds, to help you build a theory of mind.

如果答案是肯定的，他们会很快得到答案，那么就该深入研究数据了。模型做错了什么？我们能找到失败的模式吗？尝试要求模型在响应之前或之后进行自我解释，以帮助您建立心理理论。

## 过度强调某些评估可能会损害整体性能

> “When a measure becomes a target, it ceases to be a good measure.”
>
> “当一项措施成为目标时，它就不再是一个好的措施。”
>
> — Goodhart’s Law ——古德哈特定律

An example of this is the Needle-in-a-Haystack (NIAH) eval. The original eval helped quantify model recall as context sizes grew, as well as how recall is affected by needle position. However, it’s been so overemphasized that it’s featured as Figure 1 for Gemini 1.5’s report. The eval involves inserting a specific phrase (“The special magic {city} number is: {number}”) into a long document which repeats the essays of Paul Graham, and then prompting the model to recall the magic number.

大海捞针 (NIAH) 评估就是一个例子。最初的评估有助于量化随着上下文大小的增长而产生的模型召回率，以及针位置对召回率的影响。然而，它被过分强调了，以至于在 Gemini 1.5 的报告中如图 1 所示。评估涉及在重复 Paul Graham 文章的长文档中插入特定短语（“特殊的神奇 `{city}` 数字是：`{number}`”），然后提示模型回忆该神奇数字。



While some models achieve near-perfect recall, it’s questionable whether NIAH truly reflects the reasoning and recall abilities needed in real-world applications. Consider a more practical scenario: Given the transcript of an hour-long meeting, can the LLM summarize the key decisions and next steps, as well as correctly attribute each item to the relevant person? This task is more realistic, going beyond rote memorization and also considering the ability to parse complex discussions, identify relevant information, and synthesize summaries.

虽然一些模型实现了近乎完美的召回率，但 NIAH 是否真正反映了现实应用中所需的推理和召回能力值得怀疑。考虑一个更实际的场景：给定一个小时的会议记录，LLM 能否总结关键决策和后续步骤，并正确地将每个项目归因于相关人员？这项任务更加现实，超越了死记硬背，还考虑了解析复杂讨论、识别相关信息和综合摘要的能力。

Here’s a[n example of a practi](https://github.com/wyim/aci-bench/tree/main/data/challenge_data)cal NIAH eval. Using transcripts of doctor-patient video calls, the LLM is queried about the patient’s medication. It also includes a more challenging NIAH, inserting a phrase for random ingredients for pizza toppings, such as “The secret ingredients needed to build the perfect pizza are: Espresso-soaked dates, Lemon and Goat cheese.” Recall was around 80% on the medication task and 30% on the pizza task.

这是一个实用的 NIAH 评估示例。使用医患视频通话的记录，向 LLM 询问患者的用药情况。它还包括更具挑战性的 NIAH，插入用于披萨配料的随机成分的短语，例如“制作完美披萨所需的秘密成分是：浓缩咖啡浸泡的枣子、柠檬和山羊奶酪。”药物任务的召回率约为 80%，披萨任务的召回率为 30%。

![](img/dfb792c7f744761f695cfda4cad42f6d.png)

Tangentially, an overemphasis on NIAH evals can lead to lower performance on extraction and summarization tasks. Because these LLMs are so finetuned to attend to every sentence, they may start to treat irrelevant details and distractors as important, thus including them in the final output (when they shouldn’t!)

顺便说一句，过分强调 NIAH 评估可能会导致提取和摘要任务的性能降低。因为这些 LLMs 被如此精细地调整以关注每个句子，所以他们可能开始将不相关的细节和干扰因素视为重要，从而将它们包含在最终输出中（当他们不应该时！）

This could also apply to other evals and use cases. For example, summarization. An emphasis on factual consistency could lead to summaries that are less specific (and thus less likely to be factually inconsistent) and possibly less relevant. Conversely, an emphasis on writing style and eloquence could lead to more flowery, marketing-type language that could introduce factual inconsistencies.

这也适用于其他评估和用例。比如，总结。强调事实一致性可能会导致摘要不太具体（因此不太可能与事实不一致）并且可能不太相关。相反，强调写作风格和口才可能会导致更加华丽的营销型语言，从而导致事实不一致。

### **S**implify annotation to binary tasks or pairwise comparisons 简化对二进制任务或成对比较的注释

P[roviding ope](https://en.wikipedia.org/wiki/Likert_scale)n-ended feedback or ratings for model output on a Likert scale is cognitively demanding. As a result, the data collected is more noisy—due to variability among human raters—and thus less useful. A more effective approach is to simplify the task and reduce the cognitive burden on annotators. Two tasks that work well are binary classifications and pairwise comparisons.

为李克特量表的模型输出提供开放式反馈或评级对认知要求很高。因此，由于人类评估者之间的差异，收集到的数据噪音更大，因此用处不大。更有效的方法是简化任务并减轻注释者的认知负担。两个效果很好的任务是二元分类和成对比较。

In binary c[lassifications, ](https://doordash.engineering/2020/08/28/overcome-the-cold-start-problem-in-menu-item-tagging/)annotators are asked to make a simple yes-or-no judgment on the model’s output. They might be asked whether the generated summary is factually consistent with the source document, or whether the proposed response is relevant, or if it contains toxicity. Compared to the Likert scale, binary decisions are more precise, have higher consistency among raters, and lead to higher throughput. This was how Doordash setup their labeling queues for tagging menu items though a tree of yes-no questions.

在二元分类中，注释者被要求对模型的输出做出简单的是或否判断。他们可能会被问到生成的摘要是否与源文档事实上一致，或者提议的响应是否相关，或者是否包含毒性。与李克特量表相比，二元决策更精确，评分者之间的一致性更高，并且吞吐量更高。这就是 Doordash 设置标签队列的方式，通过是非问题树来标记菜单项。

In pairw[ise compariso](https://www.youtube.com/watch?v=CzR3OrOkM9w)ns, the annotator is presented with a pair of model responses and asked which is better. Because it’s easier for humans to say “A is better than B” than to assign an individual score to either A or B individually, this leads to faster and more reliable annotations (over Likert scales). At a Llama2 meetup, Thomas Scialom, an author on the Llama2 paper, confirmed that pairwise-comparisons were faster and cheaper than collecting supervised finetuning data such as written responses. The former’s cost is $3.5 per unit while the latter’s cost is $25 per unit.

在成对比较中，向注释者提供一对模型响应并询问哪个更好。因为人类更容易说“A 比 B 好”，而不是单独为 A 或 B 分配单独的分数，所以这会带来更快、更可靠的注释（通过李克特量表）。在 Llama2 的一次聚会上，Llama2 论文的作者 Thomas Scialom 证实，成对比较比收集书面回复等监督微调数据更快、更便宜。前者的成本为每单位 3.5 美元，而后者的成本为每单位 25 美元。

If you’re starting to write labeling guidelines, here are some reference guidelines from Google and Bing Search.

如果您开始编写标签指南，这里有一些来自 Google 和 Bing 搜索的参考指南。

### **(Reference-free)** evals and guardrails can be used interchangeably （无参考）evals和guardrails可以互换使用

Guardrails help to catch inappropriate or harmful content while evals help to measure the quality and accuracy of the model’s output. In the case of reference-free evals, they may be considered two sides of the same coin. Reference-free evals are evaluations that don’t rely on a “golden” reference, such as a human-written answer, and can assess the quality of output based solely on the input prompt and the model’s response.

护栏有助于捕获不适当或有害的内容，而评估有助于衡量模型输出的质量和准确性。在无参考评估的情况下，它们可以被视为同一枚硬币的两面。无参考评估是不依赖“黄金”参考（例如人工编写的答案）的评估，并且可以仅根据输入提示和模型的响应来评估输出的质量。

Some examples of these are summarization evals, where we only have to consider the input document to evaluate the summary on factual consistency and relevance. If the summary scores poorly on these metrics, we can choose not to display it to the user, effectively using the eval as a guardrail. Similarly, reference-free translation evals can assess the quality of a translation without needing a human-translated reference, again allowing us to use it as a guardrail.

其中的一些例子是摘要评估，我们只需考虑输入文档即可评估摘要的事实一致性和相关性。如果摘要在这些指标上得分不佳，我们可以选择不向用户显示它，从而有效地使用评估作为护栏。同样，无参考翻译评估可以评估翻译的质量，而无需人工翻译的参考，这再次允许我们将其用作护栏。

### **LLMs will return output even** when they shouldn’t LLMs 将返回输出，即使它们不应该返回

A key challenge when working with LLMs is that they’ll often generate output even when they shouldn’t. This can lead to harmless but nonsensical responses, or more egregious defects like toxicity or dangerous content. For example, when asked to extract specific attributes or metadata from a document, an LLM may confidently return values even when those values don’t actually exist. Alternatively, the model may respond in a language other than English because we provided non-English documents in the context.

使用 LLMs 时的一个关键挑战是，即使不应该生成输出，它们也会经常生成输出。这可能会导致无害但无意义的响应，或者更严重的缺陷，例如毒性或危险内容。例如，当要求从文档中提取特定属性或元数据时，LLM 可能会自信地返回值，即使这些值实际上并不存在。或者，模型可能会以英语以外的语言进行响应，因为我们在上下文中提供了非英语文档。

While we can try to prompt the LLM to return a “not applicable” or “unknown” response, it’s not foolproof. Even when the log probabilities are available, they’re a poor indicator of output quality. While log probs indicate the likelihood of a token appearing in the output, they don’t necessarily reflect the correctness of the generated text. On the contrary, for instruction-tuned models that are trained to respond to queries and generate coherent response, log probabilities may not be well-calibrated. Thus, while a high log probability may indicate that the output is fluent and coherent, it doesn’t mean it’s accurate or relevant.

虽然我们可以尝试提示 LLM 返回“不适用”或“未知”响应，但这并不是万无一失的。即使对数概率可用，它们也不能很好地反映输出质量。虽然对数概率表示标记出现在输出中的可能性，但它们不一定反映生成文本的正确性。相反，对于经过训练以响应查询并生成一致响应的指令调整模型，对数概率可能无法很好地校准。因此，虽然高对数概率可能表明输出流畅且连贯，但这并不意味着它是准确的或相关的。



While careful prompt engineering can help to some extent, we should complement it with robu[st guardrails that detect](https://github.com/topics/pii-detection) and filter/regenerate undesired output. For example, OpenAI provides a content moderation API that can identify unsafe responses such as hate speech, self-harm, or sexual output. Similarly, there are numerous packages for detecting personally identifiable information (PII). One benefit is that guardrails are largely agnostic of the use case and can thus be applied broadly to all output in a given language. In addition, with precise retrieval, our system can deterministically respond “I don’t know” if there are no relevant documents.

虽然仔细的提示工程可以在一定程度上有所帮助，但我们应该用强大的护栏来补充它，以检测和过滤/重新生成不需要的输出。例如，OpenAI 提供了一个内容审核 API，可以识别不安全的响应，例如仇恨言论、自残或性输出。同样，有许多用于检测个人身份信息 (PII) 的软件包。一个好处是，护栏在很大程度上与用例无关，因此可以广泛应用于给定语言的所有输出。此外，通过精确检索，如果没有相关文档，我们的系统可以确定地回答“我不知道”。

A corollary here is that LLMs may fail to produce outputs when they are expected to. This can happen for various reasons, from straightforward issues like long tail latencies from API providers to more complex ones such as outputs being blocked by content moderation filters. As such, it’s important to consistently log inputs and (potentially a lack of) outputs for debugging and monitoring.

这里的一个推论是 LLMs 可能无法按预期产生输出。发生这种情况的原因有多种，从简单的问题（例如 API 提供商的长尾延迟）到更复杂的问题（例如内容审核过滤器阻止输出）。因此，持续记录输入和（可能缺乏）输出以进行调试和监控非常重要。

#### Hallucinations are a stubborn problem. 幻觉是一个顽固的问题。

Unlike content safety or PII defects which have a lot of attention and thus seldom occur, factual inconsistencies are stubbornly persistent and more challenging to detect. They’re more common and occur at a baseline rate of 5 – 10%, and from what we’ve learned from LLM providers, it can be challenging to get it below 2%, even on simple tasks such as summarization.

与受到广泛关注且很少发生的内容安全或 PII 缺陷不同，事实不一致是顽固地持续存在的，并且更难以检测。它们更为常见，发生的基线率为 5 – 10%，根据我们从 LLM 提供商那里了解到的情况，将其降低到 2% 以下可能具有挑战性，即使是在简单的任务上，例如作为总结。

To address this, we can combine prompt engineering (upstream of generation) and factual inconsistency guardrails (downstream of generation). For prompt engineering, techniques like CoT help reduce hallucination by getting the LLM to explain its reasoning before finally returning the output. Then, we can apply a factual inconsistency guardrail to assess the factuality of summaries and filter or regenerate hallucinations. In some cases, hallucinations can be deterministically detected. When using resources from RAG retrieval, if the output is structured and identifies what the resources are, you should be able to manually verify they’re sourced from the input context.

为了解决这个问题，我们可以将即时工程（发电上游）和事实不一致护栏（发电下游）结合起来。对于即时工程，CoT 等技术可以让 LLM 在最终返回输出之前解释其推理，从而帮助减少幻觉。然后，我们可以应用事实不一致护栏来评估摘要的真实性并过滤或重新生成幻觉。在某些情况下，可以确定性地检测到幻觉。使用 RAG 检索中的资源时，如果输出是结构化的并标识了资源是什么，您应该能够手动验证它们是否源自输入上下文。

# Part 2： operational 操作部分

A possibly apocryphal quote attributed to many leaders reads: “Amateurs talk strategy and tactics. Professionals talk operations.” Where the tactical perspective sees a thicket of sui generis problems, the operational perspective sees a pattern of organizational dysfunction to repair. Where the strategic perspective sees an opportunity, the operational perspective sees a challenge worth rising to.

许多领导人引用的一句话可能是杜撰的：“业余人士谈论战略和战术。专业人士讲运营。”战术角度看到的是一堆特殊问题，而行动角度则看到了需要修复的组织功能障碍模式。从战略角度看机遇，从运营角度看挑战。

In part 1 of this essay[, we int](https://www.oreilly.com/radar/what-we-learned-from-a-year-of-building-with-llms-part-i/)[roduced the tact](https://www.oreilly.com/radar/what-we-learned-from-a-year-of-building-with-llms-part-i/)ical nuts and bolts of working with LLMs. In the next part, we will zoom out to cover the long-term strategic considerations. In this part, we discuss the operational aspects of building LLM applications that sit between strategy and tactics and bring rubber to meet roads.

在本文的第 1 部分中，我们介绍了使用 LLMs 的战术细节。在下一部分中，我们将聚焦于长期战略考虑。在这一部分中，我们将讨论构建介于战略和战术之间的LLM应用程序的操作方面，并让橡胶满足道路。

Operating an LLM application raises some questions that are familiar from operating traditional software systems, often with a novel spin to keep things spicy. LLM applications also raise entirely new questions. We split these questions, and our answers, into four parts: data, models, product, and people.

操作 LLM 应用程序会引发一些操作传统软件系统时常见的问题，通常会采用新颖的方式来让事情变得有趣。 LLM应用程序也提出了全新的问题。我们将这些问题和我们的答案分为四个部分：数据、模型、产品和人员。

For data, we answer: How and how often should you review LLM inputs and outputs? How do you measure and reduce test-prod skew?

对于数据，我们回答：您应该如何以及多久审查LLM输入和输出？如何测量和减少测试产品偏差？

For models, we answer: How do you integrate language models into the rest of the stack? How should you think about versioning models and migrating between models and versions?

对于模型，我们回答：如何将语言模型集成到堆栈的其余部分？您应该如何考虑版本控制模型以及模型和版本之间的迁移？

For product, we answer: When should design be involved in the application development process, and why is it “as early as possible”? How do you design user experiences with rich human-in-the-loop feedback? How do you prioritize the many conflicting requirements? How do you calibrate product risk?

对于产品，我们回答：设计应该什么时候参与到应用开发过程中，为什么是“尽早”？如何通过丰富的人机交互反馈来设计用户体验？您如何确定许多相互冲突的需求的优先顺序？您如何校准产品风险？

And finally, for people, we answer: Who should you hire to build a successful LLM application, and when should you hire them? How can you foster the right culture, one of experimentation? How should you use emerging LLM applications to build your own LLM application? Which is more critical: process or tooling?

最后，对于人们，我们回答：您应该雇用谁来构建成功的 LLM 应用程序，以及您应该何时雇用他们？如何培养正确的文化，一种实验文化？您应该如何使用新兴的LLM应用程序来构建您自己的LLM应用程序？工艺和工具哪个更重要？

As an AI language model, I do not have opinions and so cannot tell you whether the introduction you provided is “goated or nah.” However, I can say that the introduction properly sets the stage for the content that follows.

作为一个AI语言模型，我没有意见，所以无法告诉你你提供的介绍是“goated还是nah”。然而，我可以说，引言为接下来的内容正确地奠定了基础。

## Operations: Developing and Managing LLM Applications and the Teams That Build Them 运营：开发和管理LLM应用程序以及构建它们的团队

### Data 数据

Just as the quality of ingredients determines the dish’s taste, the quality of input data constrains the performance of machine learning systems. In addition, output data is the only way to tell whether the product is working or not. All the authors focus tightly on the data, looking at inputs and outputs for several hours a week to better understand the data distribution: its modes, its edge cases, and the limitations of models of it.

正如食材的质量决定菜肴的味道一样，输入数据的质量也限制了机器学习系统的性能。此外，输出数据是判断产品是否正常工作的唯一方法。所有作者都密切关注数据，每周花几个小时查看输入和输出，以更好地理解数据分布：它的模式、它的边缘情况以及它的模型的局限性。

#### Check for development-prod skew 检查开发与产品之间的偏差

A common source of errors in traditional machine learning pipelines is train-serve skew. This happens when the data used in training differs from what the model encounters in production. Although we can use LLMs without training or fine-tuning, hence there’s no training set, a similar issue arises with development-prod data skew. Essentially, the data we test our systems on during development should mirror what the systems will face in production. If not, we might find our production accuracy suffering.

传统机器学习流程中常见的错误来源是训练-服务偏差。当训练中使用的数据与模型在生产中遇到的数据不同时，就会发生这种情况。虽然我们可以在没有训练或微调的情况下使用 LLMs，因此没有训练集，但开发产品数据倾斜也会出现类似的问题。本质上，我们在开发过程中测试系统的数据应该反映系统在生产中将面临的情况。如果没有，我们可能会发现我们的生产精度受到影响。

LLM development-prod skew can be categorized into two types: structural and content-based. Structural skew includes issues like formatting discrepancies, such as differences between a JSON dictionary with a list-type value and a JSON list, inconsistent casing, and errors like typos or sentence fragments. These errors can lead to unpredictable model performance because different LLMs are trained on specific data formats, and prompts can be highly sensitive to minor changes. Content-based or “semantic” skew refers to differences in the meaning or context of the data.



LLM 开发与产品的偏差可以分为两种类型：结构性偏差和基于内容的偏差。结构偏差包括格式差异等问题，例如具有列表类型值的 `JSON` 字典与 `JSON` 列表之间的差异、大小写不一致以及拼写错误或句子片段等错误。这些错误可能会导致不可预测的模型性能，因为不同的 LLMs 是针对特定数据格式进行训练的，并且提示可能对微小的变化高度敏感。基于内容或“语义”的偏差是指数据含义或上下文的差异。

As in traditional ML, it’s useful to periodically measure skew between the LLM input/output pairs. Simple metrics like the length of inputs and outputs or specific formatting requirements (e.g., `JSON` or `XML`) are straightforward ways to track changes. For more “advanced” drift detection, consider clustering embeddings of input/output pairs to detect semantic drift, such as shifts in the topics users are discussing, which could indicate they are exploring areas the model hasn’t been exposed to before.

与传统机器学习一样，定期测量 LLM 输入/输出对之间的偏差很有用。输入和输出的长度或特定格式要求（例如 `JSON` 或 `XML`）等简单指标是跟踪更改的直接方法。对于更“高级”的漂移检测，请考虑对输入/输出对的聚类嵌入来检测语义漂移，例如用户正在讨论的主题的变化，这可能表明他们正在探索模型以前从未接触过的领域。

When testing changes, such as prompt engineering, ensure that holdout datasets are current and reflect the most recent types of user interactions. For example, if typos are common in production inputs, they should also be present in the holdout data. Beyond just numerical skew measurements, it’s beneficial to perform qualitative assessments on outputs. Regularly reviewing your model’s outputs—a practice colloquially known as “vibe checks”—ensures that the results align with expectations and remain relevant to user needs. Finally, incorporating nondeterminism into skew checks is also useful—by running the pipeline multiple times for each input in our testing dataset and analyzing all outputs, we increase the likelihood of catching anomalies that might occur only occasionally.

在测试更改（例如提示工程）时，请确保保留数据集是最新的并反映最新的用户交互类型。例如，如果拼写错误在生产输入中很常见，那么它们也应该出现在保留数据中。除了数值偏差测量之外，对输出进行定性评估也是有益的。定期检查模型的输出（这种做法俗称“氛围检查”）可确保结果符合预期并与用户需求保持相关性。最后，将不确定性纳入偏差检查也很有用——通过为测试数据集中的每个输入多次运行管道并分析所有输出，我们增加了捕获偶尔发生的异常的可能性。

#### Look at samples of LLM inputs and outputs every day 每天查看LLM输入和输出的样本

LLMs are dynamic and constantly evolving. Despite their impressive zero-shot capabilities and often delightful outputs, their failure modes can be highly unpredictable. For custom tasks, regularly reviewing data samples is essential to developing an intuitive understanding of how LLMs perform.

LLMs 是动态的且不断发展的。尽管它们的零射击能力令人印象深刻并且输出通常令人愉悦，但它们的故障模式却是高度不可预测的。对于自定义任务，定期查看数据样本对于直观了解 LLMs 的执行情况至关重要。

[Input-output pairs from production](https://arxiv.org/abs/2404.12272) are the “real things, real places” (genchi genbutsu) of LLM applications, and they cannot be substituted. Recent research highlighted that developers’ perceptions of what constitutes “good” and “bad” outputs shift as they interact with more data (i.e., criteria drift). While developers can come up with some criteria upfront for evaluating LLM outputs, these predefined criteria are often incomplete. For instance, during the course of development, we might update the prompt to increase the probability of good responses and decrease the probability of bad ones. This iterative process of evaluation, reevaluation, and criteria update is necessary, as it’s difficult to predict either LLM behavior or human preference without directly observing the outputs.

生产中的输入输出对是LLM应用程序的“真实事物、真实地点”（genchi genbutsu），它们是无法替代的。最近的研究强调，开发人员对“好”和“坏”输出的看法随着与更多数据的交互而发生变化（即标准漂移）。虽然开发人员可以预先提出一些标准来评估 LLM 输出，但这些预定义的标准通常是不完整的。例如，在开发过程中，我们可能会更新提示，以增加良好响应的概率并减少不良响应的概率。这种评估、重新评估和标准更新的迭代过程是必要的，因为如果不直接观察输出，就很难预测 LLM 行为或人类偏好。

To manage this effectively, we should log LLM inputs and outputs. By examining a sample of these logs daily, we can quickly identify and adapt to new patterns or failure modes. When we spot a new issue, we can immediately write an assertion or eval around it. Similarly, any updates to failure mode definitions should be reflected in the evaluation criteria. These “vibe checks” are signals of bad outputs; code and assertions operationalize them. Finally, this attitude must be socialized, for example by adding review or annotation of inputs and outputs to your on-call rotation.

为了有效地管理这一点，我们应该记录 LLM 输入和输出。通过每天检查这些日志的样本，我们可以快速识别并适应新的模式或故障模式。当我们发现新问题时，我们可以立即围绕它编写断言或评估。同样，对故障模式定义的任何更新都应反映在评估标准中。这些“氛围检查”是不良输出的信号；代码和断言使它们可操作。最后，这种态度必须社会化，例如通过在值班轮换中添加对输入和输出的审查或注释。

### Working with models 使用模型

With LLM APIs, we can rely on intelligence from a handful of providers. While this is a boon, these dependencies also involve trade-offs on performance, latency, throughput, and cost. Also, as newer, better models drop (almost every month in the past year), we should be prepared to update our products as we deprecate old models and migrate to newer models. In this section, we share our lessons from working with technologies we don’t have full control over, where the models can’t be self-hosted and managed.

借助 LLM API，我们可以依赖少数提供商的情报。虽然这是一个福音，但这些依赖性还涉及性能、延迟、吞吐量和成本的权衡。此外，随着更新、更好的型号的下降（过去一年中几乎每个月），我们应该准备好更新我们的产品，因为我们弃用了旧型号并迁移到更新的型号。在本节中，我们将分享使用我们无法完全控制的技术（模型无法自行托管和管理）的经验教训。

#### Generate structured output to ease downstream integration 生成结构化输出以简化下游集成

For most real-world use cases, the output of an LLM will be [consumed by](https://www.youtube.com/watch?v=B_DMMlDuJB0) [a downstream application](https://martinfowler.com/articles/building-boba.html) [via some machine-readable format](https://www.linkedin.com/blog/engineering/generative-ai/musings-on-building-a-generative-ai-product). For example, Rechat, a real-estate CRM, required structured responses for the frontend to render widgets. Similarly, Boba, a tool for generating product strategy ideas, needed structured output with fields for title, summary, plausibility score, and time horizon. Finally, LinkedIn shared about constraining the LLM to generate `YAML`, which is then used to decide which skill to use, as well as provide the parameters to invoke the skill.

对于大多数实际用例，LLM 的输出将由下游应用程序通过某种机器可读的格式使用。例如，Rechat（一种房地产 CRM）需要前端的结构化响应来呈现小部件。同样，Boba（一种用于生成产品策略创意的工具）需要包含标题、摘要、合理性得分和时间范围字段的结构化输出。最后，LinkedIn 分享了如何约束 LLM 生成 `YAML`，然后使用 `YAML` 来决定使用哪个技能，并提供调用该技能的参数。

This application pattern is an extreme version of Postel’s law: be liberal in what you accept (arbitrary natural language) and conservative in what you send (typed, machine-readable objects). As such, we expect it to be extremely durable.

此应用程序模式是 Postel 定律的极端版本：在您接受的内容（任意自然语言）上保持自由，而在发送的内容（键入的、机器可读的对象）上保持保守。因此，我们希望它非常耐用。

Currently, [`Instructor`](https://github.com/jxnl/instructor) and [`Outlines`](https://github.com/outlines-dev/outlines) are the de facto standards for coaxing structured output from LLMs. If you’re using an LLM API (e.g., Anthropic, OpenAI), use `Instructor`; if you’re working with a self-hosted model (e.g., Hugging Face), use `Outlines`.

目前，`Instructor` 和 `Outlines` 是 LLMs 结构化输出的事实上的标准。如果您使用 LLM API（例如 Anthropic、OpenAI），请使用 `Instructor`；如果您使用自托管模型（例如 Hugging Face），请使用 `Outlines`。

#### Migrating prompts across models is a pain in the ass 跨模型迁移提示是一件很痛苦的事情

Sometimes, our carefully crafted prompts work superbly with one model but fall flat with another. This can happen when we’re switching between various model providers, as well as when we upgrade across versions of the same model.

有时，我们精心设计的提示在一种模型上表现出色，但在另一种模型上却表现不佳。当我们在不同的模型提供者之间切换时，以及当我们在同一模型的不同版本之间进行升级时，就会发生这种情况。

[For example, Voiceflow found that](https://www.voiceflow.com/blog/how-much-do-chatgpt-versions-affect-real-world-performance) migrating from `gpt-3.5-turbo-0301` to [`gpt-3.5-turbo-1106` led to a 10%](https://www.godaddy.com/resources/news/llm-from-the-trenches-10-lessons-learned-operationalizing-models-at-godaddy#h-3-prompts-aren-t-portable-across-models) drop on their intent classification task. (Thankfully, they had evals!) Similarly, GoDaddy observed a trend in the positive direction, where upgrading to version 1106 narrowed the performance gap between `gpt-3.5-turbo` and `gpt-4`. (Or, if you’re a glass-half-full person, you might be disappointed that `gpt-4`’s lead was reduced with the new upgrade)



例如，Voiceflow 发现从 `gpt-3.5-turbo-0301` 迁移到 `gpt-3.5-turbo-1106` 导致其意图分类任务下降了 10%。（值得庆幸的是，他们进行了评估！）同样，GoDaddy 观察到了积极方向的趋势，升级到版本 `1106` 缩小了 gpt-3.5-turbo 和 gpt-4 之间的性能差距。（或者，如果您是一个半满的人，您可能会对 gpt-4 的领先优势随着新升级而减少感到失望）

Thus, if we have to migrate prompts across models, expect it to take more time than simply swapping the API endpoint. Don’t assume that plugging in the same prompt will lead to similar or better results. Also, having reliable, automated evals helps with measuring task performance before and after migration, and reduces the effort needed for manual verification.

因此，如果我们必须跨模型迁移提示，预计它会比简单地交换 API 端点花费更多的时间。不要假设插入相同的提示符会导致类似或更好的结果。此外，拥有可靠的自动化评估有助于衡量迁移前后的任务性能，并减少手动验证所需的工作量。

### Version and pin your models 版本并固定您的模型

[In any machine learning pipeline](https://papers.nips.cc/paper_files/paper/2015/hash/86df7dcfd896fcaf2674f757a2463eba-Abstract.html), “changing anything changes everything”. This is particularly relevant as we rely on components like large language models (LLMs) that we don’t train ourselves and that can change without our knowledge.

在任何机器学习管道中，“改变任何事情都会改变一切”。这一点尤其重要，因为我们依赖于大型语言模型（LLMs）等组件，而这些组件不是我们自己训练的，而且可以在我们不知情的情况下发生变化。

Fortunately, many model providers offer the option to “pin” specific model versions (e.g., `gpt-4-turbo-1106`). This enables us to use a specific version of the model weights, ensuring they remain unchanged. Pinning model versions in production can help avoid unexpected changes in model behavior, which could lead to customer complaints about issues that may crop up when a model is swapped, such as overly verbose outputs or other unforeseen failure modes.

幸运的是，许多模型提供商提供“固定”特定模型版本的选项（例如 `gpt-4-turbo-1106`）。这使我们能够使用特定版本的模型权重，确保它们保持不变。在生产中固定模型版本有助于避免模型行为发生意外变化，这可能会导致客户抱怨模型交换时可能出现的问题，例如过于冗长的输出或其他不可预见的故障模式。

Additionally, consider maintaining a shadow pipeline that mirrors your production setup but uses the latest model versions. This enables safe experimentation and testing with new releases. Once you’ve validated the stability and quality of the outputs from these newer models, you can confidently update the model versions in your production environment.

此外，请考虑维护一个反映您的生产设置但使用最新模型版本的影子管道。这使得可以对新版本进行安全的实验和测试。一旦验证了这些新模型输出的稳定性和质量，您就可以放心地在生产环境中更新模型版本。

### Choose the smallest model that gets the job done 选择能完成工作的最小型号

When working on a new application, it’s tempting to use the biggest, most powerful model available. But once we’ve established that the task is technically feasible, it’s worth experimenting if a smaller model can achieve comparable results.

在开发新应用程序时，人们很容易使用现有的最大、最强大的模型。但是，一旦我们确定该任务在技术上可行，就值得尝试较小的模型是否可以达到类似的结果。

The benefits of a smaller model are lower latency and cost. While it may be weaker, techniques like chain-of-thought, n-shot prompts, and in-context learning can help smaller models punch above their weight. Beyond LLM APIs, fine-tuning our specific tasks can also help increase performance.

较小模型的好处是较低的延迟和成本。虽然它可能较弱，但思想链、n-shot 提示和上下文学习等技术可以帮助较小的模型超越其自身能力。除了 LLM API 之外，微调我们的特定任务也有助于提高性能。

Taken together, a carefully crafted workflow using a smaller model can often match, or even [surpass](https://twitter.com/mattshumer_/status/1770823530394833242), the output quality of a single large model, while being faster and cheaper. For example, this post shares anecdata of how Haiku + 10-shot prompt outperforms zero-shot Opus and GPT-4. In the long term, we expect to see more examples of flow-engineering with smaller models as the optimal balance of output quality, latency, and cost.

总而言之，使用较小模型精心设计的工作流程通常可以匹配甚至超越单个大型模型的输出质量，同时速度更快、成本更低。例如，这篇文章分享了俳句 + 10 次提示如何优于零次 Opus 和 GPT-4 的轶事。从长远来看，我们期望看到更多使用较小模型的流程工程示例，作为输出质量、延迟和成本的最佳平衡。

As [another example, take the humble classification task](https://eugeneyan.com/writing/finetuning/). Lightweight models like DistilBERT (67M parameters) are a surprisingly strong baseline. The 400M parameter DistilBART is another great option—when fine-tuned on open source data, it could identify hallucinations with an ROC-AUC of 0.84, surpassing most LLMs at less than 5% of latency and cost.

再举一个例子，以简单的分类任务为例。像 DistilBERT（67M 参数）这样的轻量级模型是一个令人惊讶的强大基线。400M 参数 DistilBART 是另一个不错的选择，当对开源数据进行微调时，它可以识别 ROC-AUC 为 0.84 的幻觉，以不到 5% 的延迟和成本超越大多数 LLMs。

The point is, don’t overlook smaller models. While it’s easy to throw a massive model at every problem, with some creativity and experimentation, we can often find a more efficient solution.

重点是，不要忽视较小的型号。虽然很容易为每个问题建立一个庞大的模型，但通过一些创造力和实验，我们通常可以找到更有效的解决方案。

### Product 产品

While new technology offers new possibilities, the principles of building great products are timeless. Thus, even if we’re solving new problems for the first time, we don’t have to reinvent the wheel on product design. There’s a lot to gain from grounding our LLM application development in solid product fundamentals, allowing us to deliver real value to the people we serve.

虽然新技术提供了新的可能性，但构建伟大产品的原则是永恒的。因此，即使我们是第一次解决新问题，我们也不必重新发明产品设计的轮子。将我们的LLM应用程序开发建立在坚实的产品基础上可以获得很多好处，使我们能够为我们服务的人们提供真正的价值。

#### Involve design early and often 尽早并经常参与设计

Having a designer will push you to understand and think deeply about how your product can be built and presented to users. We sometimes stereotype designers as folks who take things and make them pretty. But beyond just the user interface, they also rethink how the user experience can be improved, even if it means breaking existing rules and paradigms.

拥有一名设计师将促使您理解并深入思考如何构建产品并将其呈现给用户。我们有时会将设计师刻画为那些将事物变得漂亮的人。但除了用户界面之外，他们还重新思考如何改进用户体验，即使这意味着打破现有的规则和范例。

Designers are especially gifted at reframing the user’s needs into various forms. Some of these forms are more tractable to solve than others, and thus, they may offer more or fewer opportunities for AI solutions. Like many other products, building AI products should be centered around the job to be done, not the technology that powers them.

设计师尤其擅长将用户的需求重新定义为各种形式。其中一些形式比其他形式更容易解决，因此，它们可能为人工智能解决方案提供或多或少的机会。与许多其他产品一样，构建人工智能产品应该围绕要完成的工作，而不是为其提供动力的技术。

Focus [on asking yourself](https://www.tidepool.so/blog/emerging-ux-patterns-for-generative-ai-apps-copilots): “What job is the user asking this product to do for them? Is that job something a chatbot would be good at? How about autocomplete? Maybe something different!” Consider the existing design patterns and how they relate to the job-to-be-done. These are the invaluable assets that designers add to your team’s capabilities.

专注于问自己：“用户要求该产品为他们做什么工作？聊天机器人擅长这项工作吗？自动完成怎么样？也许有什么不同！”考虑现有的设计模式以及它们与待完成的工作的关系。这些是设计师为团队能力添加的宝贵资产。

#### Design your UX for Human-in-the-Loop 设计人机交互的用户体验

One way to get quality annotations is to integrate Human-in-the-Loop (HITL) into the user experience (UX). By allowing users to provide feedback and corrections easily, we can improve the immediate output and collect valuable data to improve our models.

获得高质量注释的一种方法是将人机交互（HITL）集成到用户体验（UX）中。通过允许用户轻松提供反馈和更正，我们可以改进即时输出并收集有价值的数据来改进我们的模型。

Imagine an e-commerce platform where users upload and categorize their products. There are several ways we could design the UX:

想象一个电子商务平台，用户可以在其中上传和分类他们的产品。我们可以通过多种方式设计用户体验：

-   The user manually selects the right product category; an LLM periodically checks new products and corrects miscategorization on the backend.

    用户手动选择合适的产品类别；LLM 定期检查新产品并纠正后端的错误分类。

-   The user doesn’t select any category at all; an LLM periodically categorizes products on the backend (with potential errors).

    用户根本没有选择任何类别；LLM 定期在后端对产品进行分类（可能存在错误）。

-   An LLM suggests a product category in real time, which the user can validate and update as needed.

    LLM 实时建议产品类别，用户可以根据需要进行验证和更新。



[所有三种方法都涉及 LLM](https://eugeneyan.com/writing/llm-patterns/#collect-user-feedback-to-build-our-data-flywheel)，但它们提供了截然不同的用户体验。第一种方法将初始负担置于用户身上，并让 LLM 充当后处理检查。第二种要求用户零努力，但不提供透明度或控制。第三个达到了适当的平衡。通过预先让 LLM 建议类别，我们减少了用户的认知负担，他们不必学习我们的分类法来对他们的产品进行分类！同时，通过允许用户查看和编辑建议，他们对产品的分类方式拥有最终决定权，将控制权牢牢掌握在自己手中。作为奖励，第三种方法为模型改进创建了一个自然的反馈循环。好的建议会被接受（正面标签），而不好的建议会被更新（负面的之后是正面标签）。

虽然所有三种方法都涉及 LLM，但它们提供了截然不同的用户体验。第一种方法将初始负担置于用户身上，并让 LLM 充当后处理检查。第二种要求用户零努力，但不提供透明度或控制。第三个达到了适当的平衡。通过预先让 LLM 建议类别，我们减少了用户的认知负担，他们不必学习我们的分类法来对他们的产品进行分类！同时，通过允许用户查看和编辑建议，他们对产品的分类方式拥有最终决定权，将控制权牢牢掌握在自己手中。作为奖励，第三种方法为模型改进创建了一个自然的反馈循环。好的建议会被接受（正面标签），而不好的建议会被更新（负面的之后是正面标签）。

这种建议、用户验证和数据收集的模式常见于多种应用程序中：

这种建议、用户验证和数据收集的模式常见于多种应用程序中：

+   编码助手：用户可以接受建议（强积极）、接受并调整建议（积极）或忽略建议（消极）
+   Midjourney：用户可以选择升级和下载图像（强正）、改变图像（正）或生成一组新图像（负）
+   聊天机器人：用户可以对响应提供“赞成”（正面）或“反对”（负面），或者如果响应确实很糟糕（强烈负面），则可以选择重新生成响应

反馈可以是显式的，也可以是隐式的。显式反馈是用户根据我们产品的请求提供的信息；隐式反馈是我们从用户交互中学到的信息，而不需要用户刻意提供反馈。编码助手和 Midjourney 是隐式反馈的示例，而赞成和反对则是显式反馈。如果我们设计得很好，比如编码助手和 Midjourney，我们就可以收集大量隐式反馈来改进我们的产品和模型。

#### Prioritize your hierarchy of needs ruthlessly 无情地确定你的需求层次的优先顺序

当我们考虑将演示投入生产时，我们必须考虑以下要求：

+   可靠性：正常运行时间为 99.9%，遵守结构化输出
+   无害性：不会产生攻击性、NSFW 或其他有害内容
+   事实一致性：忠实于所提供的上下文，而不是捏造事实
+   实用性：与用户的需求和要求相关
+   可扩展性：延迟 SLA、支持的吞吐量
+   成本：因为我们没有无限的预算
+   还有更多：安全、隐私、公平、GDPR、DMA 等

如果我们试图同时满足所有这些要求，我们将永远不会交付任何东西。因此，我们需要优先考虑。无情地。这意味着要清楚什么是不可协商的（例如，可靠性、无害性），否则我们的产品就无法运行或不可行。这一切都是为了确定最受喜爱的产品。我们必须接受第一个版本并不完美，只需启动并迭代即可。

#### Calibrate your risk tolerance based on the use case 根据用例校准您的风险承受能力

在决定应用程序的语言模型和审查级别时，请考虑用例和受众。对于提供医疗或财务建议的面向客户的聊天机器人，我们需要非常高的安全性和准确性标准。错误或不良输出可能会造成真正的伤害并削弱信任。但对于不太关键的应用程序（例如推荐系统）或面向内部的应用程序（例如内容分类或摘要），过于严格的要求只会减慢进度，而不会增加太多价值。

[这与最近的 a16z 报告一致](https://a16z.com/generative-ai-enterprise-2024/)，该报告显示，与外部应用程序相比，许多公司使用内部 LLM 应用程序的速度更快。通过尝试利用人工智能提高内部生产力，组织可以开始获取价值，同时学习如何在更受控的环境中管理风险。然后，当他们获得信心时，他们可以扩展到面向客户的用例。

### Team & Roles 团队与角色

没有哪个工作职能是容易定义的，但为这个新领域的工作编写工作描述比其他工作更具挑战性。我们将放弃交叉职位的维恩图，或职位描述的建议。然而，我们将接受一个新角色——人工智能工程师——的存在，并讨论它的地位。重要的是，我们将讨论团队的其他成员以及如何分配职责。

#### Focus on process, not tools 关注流程，而不是工具

当面对新的范式时，例如 LLM，软件工程师倾向于偏爱工具。结果，我们忽略了该工具应该解决的问题和流程。在这样做时，许多工程师假设了偶然的复杂性，这会对团队的长期生产力产生负面影响。

例如，[这篇文章](https://hamel.dev/blog/posts/prompt/)讨论了某些工具如何自动为大型语言模型创建提示。它认为（恕我直言），在没有首先了解问题解决方法或流程的情况下使用这些工具的工程师最终会承担不必要的技术债务。

除了偶然的复杂性之外，工具的说明也常常不明确。例如，LLM 评估工具行业正在不断发展，这些工具提供“LLM 评估盒”以及针对毒性、简洁性、语气等的通用评估器。我们已经看到很多团队采用这些工具时无需批判性地思考其领域的特定故障模式。将此与 EvalGen 进行对比。它的重点是通过让用户深入参与从指定标准到标记数据到检查评估的每一步，来教导用户创建特定于领域的评估的过程。该软件引导用户完成如下所示的工作流程：

![](img/f641e9a2fffe11d338e340cb995dd278.png)

[Shan](https://arxiv.org/abs/2404.12272)[kar, S., et al. (2024). Who Validates the Validators? Aligning LLM-Assisted Evaluation of LLM Outputs with Human Preferences. Retrieved from https://arxiv.org/abs/2404.12272](https://arxiv.org/abs/2404.12272)

香卡，S.，等人。 （2024）。谁来验证验证者？使 LLM 输出的 LLM 辅助评估与人类偏好保持一致。检索自 https://arxiv.org/abs/2404.12272



EvalGen guides the user through a best practice of crafting LLM evaluations, namely:

EvalGen 指导用户完成制定 LLM 评估的最佳实践，即：

1. Defining domain-specific tests (bootstrapped automatically from the prompt). These are defined as either assertions with code or with LLM-as-a-Judge.

   定义特定于域的测试（从提示符自动引导）。这些被定义为带有代码的断言或带有 LLM-as-a-Judge 的断言。

2. The importance of aligning the tests with human judgment, so that the user can check that the tests capture the specified criteria.

   将测试与人类判断保持一致的重要性，以便用户可以检查测试是否符合指定的标准。

3. Iterating on your tests as the system (prompts, etc.) changes.

   当系统（提示等）发生变化时迭代您的测试。

EvalGen provides developers with a mental model of the evaluation building process without anchoring them to a specific tool. We have found that after providing AI engineers with this context, they often decide to select leaner tools or build their own.

EvalGen 为开发人员提供了评估构建过程的心智模型，而无需将他们固定在特定工具上。我们发现，在向人工智能工程师提供这种背景信息后，他们通常会决定选择更精简的工具或构建自己的工具。

There are too many components of LLMs beyond prompt writing and evaluations to list exhaustively here. However, it is important that AI engineers seek to understand the processes before adopting tools.

除了提示写作和评估之外，LLMs 的组成部分太多，无法在此详尽列出。然而，重要的是，人工智能工程师在采用工具之前首先要了解流程。

## Always be experimenting 永远在尝试

ML products are deeply intertwined with experimentation. Not only the A/B, randomized control trials kind, but the frequent attempts at modifying the smallest possible components of your system and doing offline evaluation. The reason why everyone is so hot for evals is not actually about trustworthiness and confidence—it’s about enabling experiments! The better your evals, the faster you can iterate on experiments, and thus the faster you can converge on the best version of your system.

机器学习产品与实验紧密相连。不仅是 A/B、随机对照试验，还包括频繁尝试修改系统的最小可能组件并进行离线评估。每个人如此热衷于评估的原因实际上并不是关于可信度和信心，而是关于支持实验！您的评估越好，迭代实验的速度就越快，从而可以更快地收敛到系统的最佳版本。

It’s common to try different approaches to solving the same problem because experimentation is so cheap now. The high-cost of collecting data and training a model is minimized—prompt engineering costs little more than human time. Position your team so that everyone is taught the basics of prompt engineering. This encourages everyone to experiment and leads to diverse ideas from across the organization.

尝试不同的方法来解决同一问题是很常见的，因为现在的实验成本非常低。收集数据和训练模型的高成本被最小化——即时工程成本仅比人工时间多一点。对您的团队进行定位，以便每个人都能学到即时工程的基础知识。这鼓励每个人进行实验，并在整个组织中产生不同的想法。

Additionally, don’t only experiment to explore—also use them to exploit! Have a working version of a new task? Consider having someone else on the team approach it differently. Try doing it another way that’ll be faster. Investigate prompt techniques like chain-of-thought or few-shot to make it higher quality. Don’t let your tooling hold you back on experimentation; if it is, rebuild it, or buy something to make it better.

此外，不仅要通过实验来探索，还要利用它们来探索！有新任务的工作版本吗？考虑让团队中的其他人以不同的方式处理它。尝试用另一种方式来做，这样会更快。研究诸如链式思维或少镜头之类的提示技术，以提高其质量。不要让你的工具阻碍你的实验；如果是的话，重建它，或者买一些东西让它变得更好。

Finally, during product/project planning, set aside time for building evals and running multiple experiments. Think of the product spec for engineering products, but add to it clear criteria for evals. And during roadmapping, don’t underestimate the time required for experimentation—expect to do multiple iterations of development and evals before getting the green light for production.

最后，在产品/项目规划期间，留出时间来构建评估和运行多个实验。考虑工程产品的产品规格，但添加明确的评估标准。在制定路线图期间，不要低估实验所需的时间——在获得生产许可之前，需要进行多次开发和评估迭代。

## Empower everyone to use new AI technology 让每个人都能使用新的人工智能技术

As generative AI increases in adoption, we want the entire team—not just the experts—to understand and feel empowered to use this new technology. There’s no better way to develop intuition for how LLMs work (e.g., latencies, failure modes, UX) than to, well, use them. LLMs are relatively accessible: You don’t need to know how to code to improve performance for a pipeline, and everyone can start contributing via prompt engineering and evals.

随着生成式人工智能的采用率不断提高，我们希望整个团队（而不仅仅是专家）能够理解并感到有权使用这项新技术。要培养对 LLMs 工作方式（例如延迟、故障模式、用户体验）的直觉，没有比使用它们更好的方法了。 LLMs 相对容易访问：您不需要知道如何编码来提高管道的性能，每个人都可以通过即时工程和评估开始做出贡献。

A big part of this is education. It can start as simple as the basics of prompt engineering, where techniques like n-shot prompting and CoT help condition the model toward the desired output. Folks who have the knowledge can also educate about the more technical aspects, such as how LLMs are autoregressive in nature. In other words, while input tokens are processed in parallel, output tokens are generated sequentially. As a result, latency is more a function of output length than input length—this is a key consideration when designing UXes and setting performance expectations.

其中很大一部分是教育。它可以像提示工程的基础一样简单地开始，其中 n-shot 提示和 CoT 等技术有助于将模型调整为所需的输出。具备相关知识的人还可以了解更多技术方面的知识，例如 LLMs 本质上是如何自回归的。换句话说，虽然输入令牌是并行处理的，但输出令牌是顺序生成的。因此，延迟更多地是输出长度的函数，而不是输入长度的函数——这是设计用户体验和设置性能期望时的一个关键考虑因素。

We can also go further and provide opportunities for hands-on experimentation and exploration. A hackathon perhaps? While it may seem expensive to have an entire team spend a few days hacking on speculative projects, the outcomes may surprise you. We know of a team that, through a hackathon, accelerated and almost completed their three-year roadmap within a year. Another team had a hackathon that led to paradigm shifting UXes that are now possible thanks to LLMs, which are now prioritized for the year and beyond.

我们还可以更进一步，提供动手实验和探索的机会。也许是黑客马拉松？虽然让整个团队花几天时间研究投机性项目似乎成本高昂，但结果可能会让您感到惊讶。我们知道有一个团队通过黑客马拉松加速并几乎在一年内完成了他们的三年路线图。另一个团队举办了一场黑客马拉松，通过 LLMs 实现了范式转变的用户体验，现在这些用户体验成为了今年及以后的优先事项。

## Don’t fall into the trap of “AI engineering is all I need” 不要陷入“我只需要人工智能工程”的陷阱

As new job titles are coined, there is an initial tendency to overstate the capabilities associated with these roles. This often results in a painful correction as the actual scope of these jobs becomes clear. Newcomers to the field, as well as hiring managers, might make exaggerated claims or have inflated expectations. Notable examples over the last decade include:

随着新职位的出现，人们最初倾向于夸大与这些角色相关的能力。当这些工作的实际范围变得清晰时，这通常会导致痛苦的纠正。该领域的新人以及招聘经理可能会夸大其词或抱有过高的期望。过去十年的著名例子包括：

+ [Data scientist: “someone who is better at statistics than any software engineer and better at software engineering than any statistician”](https://x.com/josh_wills/status/198093512149958656)

  数据科学家：“比任何软件工程师更擅长统计，比任何统计学家更擅长软件工程”

+ Machine learning engineer (MLE): a software engineering-centric view of machine learning

  机器学习工程师（MLE）：以软件工程为中心的机器学习观点

Initially, many assumed that data scientists alone were sufficient for data-driven projects. However, it became apparent that data scientists must collaborate with software and data engineers to develop and deploy data products effectively.

最初，许多人认为仅数据科学家就足以完成数据驱动的项目。然而，很明显，数据科学家必须与软件和数据工程师合作才能有效地开发和部署数据产品。

This misunderstanding has shown up again with the new role of AI engineer, with some teams believing that AI engineers are all you need. In reality, building machine learning or AI products requires a broad array of specialized roles. We’ve consulted with more than a dozen companies on AI products and have consistently observed that they fall into the trap of believing that “AI engineering is all you need.” As a result, products often struggle to scale beyond a demo as companies overlook crucial aspects involved in building a product.

这种误解随着人工智能工程师的新角色再次出现，一些团队认为人工智能工程师就是你所需要的。事实上，构建机器学习或人工智能产品需要广泛的专业角色。我们咨询了十几家人工智能产品公司，并一致观察到他们陷入了“你只需要人工智能工程”的陷阱。因此，由于公司忽视了构建产品所涉及的关键方面，产品往往难以扩展到演示之外。



For example, [evaluation](https://github.com/hamelsmu/ft-drift) and measurement are crucial for scaling a product beyond vibe checks. The skills for effective evaluation align with some of the strengths traditionally seen in machine learning engineers—a team composed solely of AI engineers will likely lack these skills. Coauthor Hamel Husain illustrates the importance of these skills in his recent work around detecting data drift and designing domain-specific evals.

例如，除了氛围检查之外，评估和测量对于扩展产品至关重要。有效评估的技能与传统上机器学习工程师的一些优势相一致——仅由人工智能工程师组成的团队可能缺乏这些技能。合著者 Hamel Husain 在他最近关于检测数据漂移和设计特定领域评估的工作中阐述了这些技能的重要性。

Here is a rough progression of the types of roles you need, and when you’ll need them, throughout the journey of building an AI product:

以下是在构建人工智能产品的整个过程中您需要的角色类型以及何时需要它们的粗略进展：

1. First, focus on building a product. This might include an AI engineer, but it doesn’t have to. AI engineers are valuable for prototyping and iterating quickly on the product (UX, plumbing, etc.).
   
   首先，专注于打造产品。这可能包括人工智能工程师，但并非必须如此。人工智能工程师对于产品（用户体验、管道等）的原型设计和快速迭代非常有价值。

1. Next, create the right foundations by instrumenting your system and collecting data. Depending on the type and scale of data, you might need platform and/or data engineers. You must also have systems for querying and analyzing this data to debug issues.

   接下来，通过检测系统和收集数据来创建正确的基础。根据数据的类型和规模，您可能需要平台和/或数据工程师。您还必须拥有用于查询和分析这些数据以调试问题的系统。

1. Next, you will eventually want to optimize your AI system. This doesn’t necessarily involve training models. The basics include steps like designing metrics, building evaluation systems, running experiments, optimizing RAG retrieval, debugging stochastic systems, and more. MLEs are really good at this (though AI engineers can pick them up too). It usually doesn’t make sense to hire an MLE unless you have completed the prerequisite steps.

   接下来，您最终会想要优化您的人工智能系统。这不一定涉及训练模型。基础知识包括设计指标、构建评估系统、运行实验、优化 RAG 检索、调试随机系统等步骤。 MLE 确实很擅长这一点（尽管人工智能工程师也可以学习它们）。除非您已完成先决条件步骤，否则聘请 MLE 通常没有意义。

Aside from this, you need a domain expert at all times. At small companies, this would ideally be the founding team—and at bigger companies, product managers can play this role. Being aware of the progression and timing of roles is critical. Hiring folks at the wrong time (e.g., hiring an MLE too early) or building in the wrong order is a waste of time and money, and causes churn. Furthermore, regularly checking in with an MLE (but not hiring them full-time) during phases 1–2 will help the company build the right foundations.

除此之外，您始终需要领域专家。在小公司，理想的情况是创始团队，而在大公司，产品经理可以扮演这个角色。了解角色的进展和时间安排至关重要。在错误的时间雇用人员（例如，过早雇用 MLE）或以错误的顺序进行构建都是浪费时间和金钱，并会导致客户流失。此外，在第 1-2 阶段定期与 MLE 进行检查（但不是全职雇用他们）将有助于公司建立正确的基础。

# Part 3：Strategy 战略部分

We previously shared our insights on the tactics we have honed while operating [LLM applications](https://www.oreilly.com/radar/what-we-learned-from-a-year-of-building-with-llms-part-ii/). Tactics are granular: they are the specific actions employed to achieve specific objectives. We also shared our perspective on operations: the higher-level processes in place to support tactical work to achieve objectives.

我们之前分享了我们在操作 LLM 应用程序时磨练的策略的见解。策略是细粒度的：它们是为实现特定目标而采取的特定行动。我们还分享了我们对运营的看法：支持战术工作以实现目标的更高级别流程。

But where do those objectives come from? That is the domain of strategy. Strategy answers the “what” and “why” questions behind the “how” of tactics and operations.

但这些目标从何而来？这就是战略的领域。战略回答了战术和行动“如何”背后的“什么”和“为什么”问题。

We provide our opinionated takes, such as “no GPUs before PMF” and “focus on the system not the model,” to help teams figure out where to allocate scarce resources. We also suggest a roadmap for iterating toward a great product. This final set of lessons answers the following questions:

我们提供我们的固执己见的观点，例如“PMF 之前没有 GPU”和“关注系统而不是模型”，以帮助团队弄清楚在哪里分配稀缺资源。我们还建议了一个迭代优秀产品的路线图。这最后一组课程回答了以下问题：

1. Building vs. Buying: When should you train your own models, and when should you leverage existing APIs? The answer is, as always, “it depends.” We share what it depends on.

   构建与购买：什么时候应该训练自己的模型，什么时候应该利用现有的 API？一如既往，答案是“视情况而定”。我们分享它所依赖的内容。

1. Iterating to Something Great: How can you create a lasting competitive edge that goes beyond just using the latest models? We discuss the importance of building a robust system around the model and focusing on delivering memorable, sticky experiences.

   迭代出伟大的东西：如何才能创造出超越仅仅使用最新模型的持久竞争优势？我们讨论了围绕模型构建强大系统并专注于提供令人难忘、有粘性的体验的重要性。

1. Human-Centered AI: How can you effectively integrate LLMs into human workflows to maximize productivity and happiness? We emphasize the importance of building AI tools that support and enhance human capabilities rather than attempting to replace them entirely.

   以人为本的人工智能：如何有效地将LLMs集成到人类工作流程中，以最大限度地提高生产力和幸福感？我们强调构建支持和增强人类能力的人工智能工具的重要性，而不是试图完全取代它们。

1. Getting Started: What are the essential steps for teams embarking on building an LLM product? We outline a basic playbook that starts with prompt engineering, evaluations, and data collection.

   入门：团队开始构建 LLM 产品的基本步骤是什么？我们概述了一个基本的行动手册，从快速工程、评估和数据收集开始。

1. The Future of Low-Cost Cognition: How will the rapidly decreasing costs and increasing capabilities of LLMs shape the future of AI applications? We examine historical trends and walk through a simple method to estimate when certain applications might become economically feasible.

   低成本认知的未来：LLMs快速降低的成本和不断增强的能力将如何塑造人工智能应用的未来？我们研究历史趋势并通过一种简单的方法来估计某些应用程序何时在经济上可行。

1. From Demos to Products: What does it take to go from a compelling demo to a reliable, scalable product? We emphasize the need for rigorous engineering, testing, and refinement to bridge the gap between prototype and production.

   从演示到产品：从引人注目的演示到可靠、可扩展的产品需要什么？我们强调需要进行严格的工程、测试和改进，以弥合原型和生产之间的差距。

To answer these difficult questions, let’s think step by step…

为了回答这些难题，让我们一步步思考……

## Strategy: Building with LLMs without Getting Out-Maneuvered 策略：使用LLMs进行构建，而不需要出局

Successful products require thoughtful planning and tough prioritization, not endless prototyping or following the latest model releases or trends. In this final section, we look around the corners and think about the strategic considerations for building great AI products. We also examine key trade-offs teams will face, like when to build and when to buy, and suggest a “playbook” for early LLM application development strategy.

成功的产品需要深思熟虑的规划和严格的优先级排序，而不是无休止的原型设计或遵循最新的模型发布或趋势。在最后一节中，我们环顾四周并思考构建出色的人工智能产品的战略考虑因素。我们还研究了团队将面临的关键权衡，例如何时构建和何时购买，并为早期LLM应用程序开发策略提出了“剧本”。

### No GPUs before PMF PMF 之前没有 GPU

To be great, your product needs to be more than just a thin wrapper around somebody else’s API. But mistakes in the opposite direction can be even more costly. The past year has also seen a mint of venture capital, including an eye-watering six-billion-dollar Series A, spent on training and customizing models without a clear product vision or target market. In this section, we’ll explain why jumping immediately to training your own models is a mistake and consider the role of self-hosting.

要想变得出色，您的产品需要不仅仅是别人 API 的薄包装。但相反方向的错误可能会付出更大的代价。过去的一年里，还出现了大量风险投资，其中包括令人眼花缭乱的 60 亿美元 A 轮融资，该投资用于培训和定制模型，但没有明确的产品愿景或目标市场。在本节中，我们将解释为什么立即跳到训练自己的模型是错误的，并考虑自托管的作用。

#### Training from scratch (almost) never makes sense 从头开始训练（几乎）没有意义

For most organizations, pretraining an LLM from scratch is an impractical distraction from building products.

对于大多数组织来说，从头开始预训练 LLM 会分散开发产品的注意力，这是不切实际的。

As exciting as it is and as much as it seems like everyone else is doing it, developing and maintaining machine learning infrastructure takes a lot of resources. This includes gathering data, training and evaluating models, and deploying them. If you’re still validating product-market fit, these efforts will divert resources from developing your core product. Even if you had the compute, data, and technical chops, the pretrained LLM may become obsolete in months.

尽管开发和维护机器学习基础设施令人兴奋，而且似乎其他人都在这样做，但它需要大量资源。这包括收集数据、训练和评估模型以及部署它们。如果您仍在验证产品与市场的契合度，这些努力将转移开发核心产品的资源。即使您拥有计算、数据和技术能力，预训练的 LLM 可能会在几个月内过时。



[Consider the](https://arxiv.org/abs/2303.17564) case of `BloombergGPT`, an LLM specifically trained for financial [tasks. The model ](https://arxiv.org/abs/2305.05862)was pretrained on 363B tokens and required a heroic effort by nine full-time employees, four from AI Engineering and five from ML Product and Research. Despite this effort, it was outclassed by `gpt-3.5-turbo` and `gpt-4` on those financial tasks within a year.

以 `BloombergGPT` 为例，它是专门针对金融任务进行培训的LLM。该模型是在 363B 代币上进行预训练的，需要九名全职员工的巨大努力，其中四名来自人工智能工程，五名来自机器学习产品和研究。尽管做出了这些努力，但一年内它在这些财务任务上就被 `gpt-3.5-turbo` 和 `gpt-4` 超越了。

This story and others like it suggests that for most practical applications, pretraining an LLM from scratch, even on domain-specific data, is not the best use of resources. Instead, teams are better off fine-tuning the strongest open source models available for their specific needs.

这个故事和其他类似的故事表明，对于大多数实际应用，从头开始预训练 LLM ，即使是针对特定领域的数据，也不是资源的最佳利用。相反，团队最好根据其特定需求微调最强大的开源模型。

There are of course exceptions. One shining example is Replit’s code model, trained specifically for code-generation and understanding. With pretraining, Replit was able to outperform other models of large sizes such as `CodeLlama7b`. But as other, increasingly capable models have been released, maintaining utility has required continued investment.

当然也有例外。 Replit 的代码模型就是一个典型的例子，它专门针对代码生成和理解进行了训练。通过预训练，Replit 能够超越 `CodeLlama7b` 等其他大尺寸模型。但随着其他功能日益强大的模型的发布，维持实用性需要持续的投资。

### Don’t fine-tune until you’ve proven it’s necessary 在证明有必要之前不要进行微调

For most organizations, fine-tuning is driven more by FOMO than by clear strategic thinking.

对于大多数组织来说，微调更多地是由 FOMO 驱动的，而不是由清晰的战略思维驱动的。

Organizations invest in fine-tuning too early, trying to beat the “just another wrapper” allegations. In reality, fine-tuning is heavy machinery, to be deployed only after you’ve collected plenty of examples that convince you other approaches won’t suffice.

组织过早地进行微调投资，试图推翻“只是另一个包装”的指控。事实上，微调是重型机械，只有在您收集了大量示例并让您相信其他方法还不够之后才能部署。

A year ago, many teams were telling us they were excited to fine-tune. Few have found product-market fit and most regret their decision. If you’re going to fine-tune, you’d better be really confident that you’re set up to do it again and again as base models improve—see the “The model isn’t the product” and “Build LLMOps” below.

一年前，许多团队告诉我们他们很高兴能够进行微调。很少有人发现产品适合市场，而且大多数人都后悔自己的决定。如果您要进行微调，您最好确信您已准备好随着基础模型的改进而一次又一次地进行微调 - 请参阅“模型不是产品”和“构建 LLMOps”以下。

When might fine-tuning actually be the right call? If the use case requires data not available in the mostly open web-scale datasets used to train existing models—and if you’ve already built an MVP that demonstrates the existing models are insufficient. But be careful: if great training data isn’t readily available to the model builders, where are you getting it?

什么时候微调才是真正正确的选择？如果用例需要用于训练现有模型的大多数开放的网络规模数据集中不可用的数据，并且如果您已经构建了一个 MVP 来证明现有模型不足。但要小心：如果模型构建者无法轻易获得大量训练数据，那么您从哪里获得这些数据呢？

Ultimately, remember that LLM-powered applications aren’t a science fair project; investment in them should be commensurate with their contribution to your business’ strategic objectives and its competitive differentiation.

最后，请记住，LLM 驱动的应用程序不是一个科学博览会项目；而是一个项目。对它们的投资应与其对企业战略目标及其竞争优势的贡献相称。

### Start with inference APIs, but don’t be afraid of self-hosting 从推理 API 开始，但不要害怕自托管

With LLM APIs, it’s easier than ever for startups to adopt and integrate language modeling capabilities without training their own models from scratch. Providers like Anthropic and OpenAI offer general APIs that can sprinkle intelligence into your product with just a few lines of code. By using these services, you can reduce the effort spent and instead focus on creating value for your customers—this allows you to validate ideas and iterate toward product-market fit faster.

借助 LLM API，初创公司比以往任何时候都更容易采用和集成语言建模功能，而无需从头开始训练自己的模型。 Anthropic 和 OpenAI 等提供商提供通用 API，只需几行代码即可将智能融入到您的产品中。通过使用这些服务，您可以减少花费的精力，转而专注于为客户创造价值，这使您能够更快地验证想法并迭代以实现产品市场契合。

But, as with databases, managed services aren’t the right fit for every use case, especially as scale and requirements increase. Indeed, self-hosting may be the only way to use models without sending confidential/private data out of your network, as required in regulated industries like healthcare and finance or by contractual obligations or confidentiality requirements.

但是，与数据库一样，托管服务并不适合每个用例，尤其是随着规模和需求的增加。事实上，自托管可能是使用模型而不将机密/私人数据发送到网络之外的唯一方法，这是医疗保健和金融等受监管行业的要求或合同义务或保密要求。

Furthermor[e, self-hosting circumvents limitat](https://tech.buzzfeed.com/lessons-learned-building-products-powered-by-generative-ai-7f6c23bff376#9da5)ions imposed by inference providers, like rate limits, model deprecations, and usage restrictions. In addition, self-hosting gives you complete control over the model, making it easier to construct a differentiated, high-quality system around it. Finally, self-hosting, especially of fine-tunes, can reduce cost at large scale. For example, BuzzFeed shared how they fine-tuned open source LLMs to reduce costs by 80%.

此外，自托管规避了推理提供商施加的限制，例如速率限制、模型弃用和使用限制。此外，自托管使您可以完全控制模型，从而更轻松地围绕模型构建差异化的高质量系统。最后，自托管，尤其是微调，可以大规模降低成本。例如，BuzzFeed 分享了他们如何微调开源 LLMs 以降低 80% 的成本。

### Iterate to something great 迭代一些伟大的事情

To sustain a competitive edge in the long run, you need to think beyond models and consider what will set your product apart. While speed of execution matters, it shouldn’t be your only advantage.

为了长期保持竞争优势，您需要超越模型来思考，并考虑是什么让您的产品与众不同。虽然执行速度很重要，但这不应该是您唯一的优势。

#### The model isn’t the product; the system around it is 模型不是产品，而是产品。它周围的系统是

For teams that aren’t building models, the rapid pace of innovation is a boon as they migrate from one SOTA model to the next, chasing gains in context size, reasoning capability, and price-to-value to build better and better products.

对于不构建模型的团队来说，快速的创新步伐是一个福音，因为他们从一个 SOTA 模型迁移到下一个模型，追求上下文大小、推理能力和性价比的收益，以构建越来越好的产品。

This progress is as exciting as it is predictable. Taken together, this means models are likely to be the least durable component in the system.

这一进展是令人兴奋的，也是可以预见的。总而言之，这意味着模型可能是系统中最不耐用的组件。

Instead, focus your efforts on what’s going to provide lasting value, such as:

相反，应将精力集中在能够提供持久价值的事情上，例如：

+   Evaluation chassis: To reliably measure performance on your task across models

    评估底盘：可靠地衡量跨模型的任务性能

+   Guardrails: To prevent undesired outputs no matter the model

    护栏：无论型号如何，都可以防止不需要的输出

+   Caching: To reduce latency and cost by avoiding the model altogether

    缓存：通过完全避免模型来减少延迟和成本

+   Data flywheel: To power the iterative improvement of everything above

    数据飞轮：为上述一切的迭代改进提供动力

These components create a thicker moat of product quality than raw model capabilities.

这些组件创造了比原始模型功能更厚的产品质量护城河。

But that doesn’t mean building at the application layer is risk free. Don’t point your shears at the same yaks that OpenAI or other model providers will need to shave if they want to provide viable enterprise software.

但这并不意味着在应用程序层进行构建是没有风险的。如果 OpenAI 或其他模型提供商想要提供可行的企业软件，那么不要将剪刀指向 OpenAI 或其他模型提供商需要剃毛的同一头牦牛。

For example, some teams invested in building custom tooling to validate structured output from proprietary models; minimal investment here is important, but a deep one is not a good use of time. OpenAI needs to ensure that when you ask for a function call, you get a valid function call—because all of their customers want this. Employ some “strategic procrastination” here, build what you absolutely need and await the obvious expansions to capabilities from providers.

例如，一些团队投资构建自定义工具来验证专有模型的结构化输出；最小的投资在这里很重要，但深入的投资并不利于时间的利用。 OpenAI 需要确保当您请求函数调用时，您会得到有效的函数调用 - 因为他们的所有客户都希望如此。在这里采用一些“战略性拖延”，构建你绝对需要的东西，并等待提供商对功能的明显扩展。

#### Build trust by starting small 从小事做起建立信任

Building a product that tries to be everything to everyone is a recipe for mediocrity. To create compelling products, companies need to specialize in building memorable, sticky experiences that keep users coming back.

打造一款试图满足所有人需求的产品只会导致平庸。为了创造引人注目的产品，公司需要专注于打造令人难忘、有粘性的体验，以吸引用户回头客。



Consider a generic RAG system that aims to answer any question a user might ask. The lack of specialization means that the system can’t prioritize recent information, parse domain-specific formats, or understand the nuances of specific tasks. As a result, users are left with a shallow, unreliable experience that doesn’t meet their needs.

考虑一个通用的 RAG 系统，旨在回答用户可能提出的任何问题。缺乏专业化意味着系统无法优先考虑最新信息、解析特定领域的格式或理解特定任务的细微差别。结果，用户得到的体验是肤浅的、不可靠的，不能满足他们的需求。

To address this, focus on specific domains and use cases. Narrow the scope by going deep rather than wide. This will create domain-specific tools that resonate with users. Specialization also allows you to be upfront about your system’s capabilities and limitations. Being transparent about what your system can and cannot do demonstrates self-awareness, helps users understand where it can add the most value, and thus builds trust and confidence in the output.

为了解决这个问题，请关注特定领域和用例。通过深入而不是广泛来缩小范围。这将创建与用户产生共鸣的特定领域工具。专业化还可以让您提前了解系统的功能和限制。对系统能做什么和不能做什么保持透明可以展现自我意识，帮助用户了解系统可以在哪些方面增加最大价值，从而建立对输出的信任和信心。

#### Build LLMOps, but build it for the right reason: faster iteration 构建 LLMOps，但构建它的原因是正确的：更快的迭代

DevOps is not fundamentally about reproducible workflows or shifting left or empowering two pizza teams—and it’s definitely not about writing `YAML` files.

DevOps 从根本上来说并不是关于可重复的工作流程或左移或授权两个披萨团队，而且它绝对不是关于编写 `YAML` 文件。

DevOps is about shortening the feedback cycles between work and its outcomes so that improvements accumulate instead of errors. Its roots go back, via the Lean Startup movement, to Lean manufacturing and the Toyota Production System, with its emphasis on Single Minute Exchange of Die and Kaizen.

DevOps 旨在缩短工作与其结果之间的反馈周期，以便积累改进而不是错误。它的根源可以通过精益创业运动追溯到精益制造和丰田生产系统，其重点是单分钟模具交换和改善。

MLOps has adapted the form of DevOps to ML. We have reproducible experiments and we have all-in-one suites that empower model builders to ship. And Lordy, do we have `YAML` files.

MLOps 将 DevOps 的形式改编为 ML。我们有可重复的实验，我们有一体化的套件，可以帮助模型构建者进行交付。 Lordy，我们有 `YAML` 文件吗？

But as an industry, MLOps didn’t adapt the function of DevOps. It didn’t shorten the feedback gap between models and their inferences and interactions in production.

但作为一个行业，MLOps 并没有适应 DevOps 的功能。它并没有缩短模型与其在生产中的推理和交互之间的反馈差距。

Hearteningly, the field of LLMOps has shifted away from thinking about hobgoblins of little minds like prompt management and toward the hard problems that block iteration: production monitoring and continual improvement, linked by evaluation.

令人振奋的是，LLMOps 领域已经从思考即时管理之类的小问题转向了阻碍迭代的难题：通过评估联系起来的生产监控和持续改进。

Already, we have interactive arenas for neutral, crowd-sourced evaluation of chat and coding models—an outer loop of collective, iterative improvement. Tools like LangSmith, Log10, LangFuse, W&B Weave, HoneyHive, and more promise to not only collect and collate data about system outcomes in production but also to leverage them to improve those systems by integrating deeply with development. Embrace these tools or build your own.

我们已经拥有了用于对聊天和编码模型进行中立、众包评估的交互式平台——集体、迭代改进的外循环。 LangSmith、Log10、LangFuse、W&B Weave、HoneyHive 等工具不仅可以收集和整理有关生产中系统结果的数据，还可以利用它们通过与开发深度集成来改进这些系统。拥抱这些工具或构建您自己的工具。

#### Don’t build LLM features you can buy 不要构建可以购买的LLM功能

Most successful businesses are not LLM businesses. Simultaneously, most businesses have opportunities to be improved by LLMs.

大多数成功的企业都不是LLM企业。同时，大多数企业都有机会通过LLMs进行改进。

This pair of observations often misleads leaders into hastily retrofitting systems with LLMs at increased cost and decreased quality and releasing them as ersatz, vanity “AI” features, complete with the now-dreaded sparkle icon. There’s a better way: focus on LLM applications that truly align with your product goals and enhance your core operations.

这对观察结果经常会误导领导者，以增加成本和降低质量的方式仓促改造系统，并将其作为仿制品、虚荣的“AI”功能发布，并配有现在令人恐惧的闪光图标。有一个更好的方法：专注于真正符合您的产品目标并增强核心运营的 LLM 应用程序。

Consider a few misguided ventures that waste your team’s time:

考虑一些浪费团队时间的误导行为：

+ Building custom text-to-SQL capabilities for your business

    为您的企业构建自定义文本到 SQL 功能

+ Building a chatbot to talk to your documentation

    构建一个聊天机器人来与您的文档对话

+ Integrating your company’s knowledge base with your customer support chatbot

    将公司的知识库与客户支持聊天机器人集成

While the above are the hellos-world of LLM applications, none of them make sense for virtually any product company to build themselves. These are general problems for many businesses with a large gap between promising demo and dependable component—the customary domain of software companies. Investing valuable R&D resources on general problems being tackled en masse by the current Y Combinator batch is a waste.

虽然以上是 LLM 应用程序的 hello-world，但对于几乎任何产品公司自己构建来说，它们都没有意义。对于许多企业来说，这些都是普遍存在的问题，因为有前途的演示和可靠的组件（软件公司的惯用领域）之间存在巨大差距。将宝贵的研发资源投入到当前 Y Combinator 批次正在集体解决的一般问题上是一种浪费。

If this sounds like trite business advice, it’s because in the frothy excitement of the current hype wave, it’s easy to mistake anything “LLM” as cutting-edge accretive differentiation, missing which applications are already old hat.

如果这听起来像是陈词滥调的商业建议，那是因为在当前炒作浪潮的泡沫兴奋中，很容易将任何“LLM”误认为是尖端的增值差异化，而忽略了哪些应用程序已经过时了。

#### AI in the loop; humans at the center 人工智能在循环中；人在中心

Right now, LLM-powered applications are brittle. They required an incredible amount of safe-guarding and defensive engineering and remain hard to predict. Additionally, when tightly scoped, these applications can be wildly useful. This means that LLMs make excellent tools to accelerate user workflows.

目前，LLM 驱动的应用程序很脆弱。它们需要大量的安全防护和防御工程，而且仍然难以预测。此外，当范围严格时，这些应用程序可能非常有用。这意味着 LLMs 是加速用户工作流程的优秀工具。

While it may be tempting to imagine LLM-based applications fully replacing a workflow or [standing in f](https://en.wikipedia.org/wiki/Advanced_chess)or a job function, today the most effective paradigm is a human-computer centaur (c.f. Centaur chess). When capable humans are paired with LLM capabilities tuned for their rapid utilization, productivity and happiness doing tasks can be massively increased. One of the flagship applications of LLMs, GitHub Copilot, demonstrated the power of these workflows:

虽然人们可能很容易想象基于LLM的应用程序完全取代工作流程或代替工作职能，但当今最有效的范例是人机半人马（参见半人马国际象棋）。当有能力的人与可快速利用的LLM能力相结合时，工作效率和完成任务的幸福感可以大大提高。 LLMs 的旗舰应用程序之一 GitHub Copilot 展示了这些工作流程的强大功能：

> “Overall, developers told us they felt more confident because coding is easier, more error-free, more readable, more reusable, more concise, more maintainable, and more resilient with GitHub Copilot and GitHub Copilot Chat than when they’re coding without it.”

> “总的来说，开发人员告诉我们，他们感到更有信心，因为使用 GitHub Copilot 和 GitHub Copilot Chat 进行编码比不使用 GitHub Copilot 时编码更容易、更无错误、更易读、更可重用、更简洁、更可维护且更有弹性”。

> —Mario Rodriguez, GitHub

> — 马里奥·罗德里格斯，GitHub

For those who have worked in ML for a long time, you may jump to the idea of “human-in-the-loop,” but not so fast: HITL machine learning is a paradigm built on human experts ensuring that ML models behave as predicted. While related, here we are proposing something more subtle. LLM driven systems should not be the primary drivers of most workflows today; they should merely be a resource.

对于那些长期从事 ML 工作的人来说，您可能会跳到“人机循环”的想法，但不会那么快：HITL 机器学习是一种建立在人类专家基础上的范式，确保 ML 模型的行为与预料到的。虽然相关，但我们在这里提出一些更微妙的建议。 LLM 驱动系统不应该成为当今大多数工作流程的主要驱动因素；它们应该只是一种资源。

By centering humans and asking how an LLM can support their workflow, this leads to significantly different product and design decisions. Ultimately, it will drive you to build different products than competitors who try to rapidly offshore all responsibility to LLMs—better, more useful, and less risky products.

通过以人为中心并询问LLM如何支持他们的工作流程，这会导致显着不同的产品和设计决策。最终，它将促使您开发出与竞争对手不同的产品，竞争对手试图将所有责任快速转移到LLMs上——更好、更有用、风险更小的产品。

### Start with prompting, evals, and data collection 从提示、评估和数据收集开始

The previous sections have delivered a fire hose of techniques and advice. It’s a lot to take in. Let’s consider the minimum useful set of advice: if a team wants to build an LLM product, where should they begin?

前面的部分提供了大量的技术和建议。有很多东西需要考虑。让我们考虑一下最基本的有用建议：如果一个团队想要构建一个 LLM 产品，他们应该从哪里开始？



Over the last year, we’ve seen enough examples to start becoming confident that successful LLM applications follow a consistent trajectory. We walk through this basic “getting started” playbook in this section. The core idea is to start simple and only add complexity as needed. A decent rule of thumb is that each level of sophistication typically requires at least an order of magnitude more effort than the one before it. With this in mind…

在过去的一年里，我们看到了足够多的例子，开始相信成功的LLM应用程序遵循一致的轨迹。我们将在本节中介绍这个基本的“入门”手册。核心思想是从简单开始，仅根据需要增加复杂性。一个不错的经验法则是，每一个复杂程度通常都需要比之前至少多一个数量级的努力。考虑到这一点……

### Prompt engineering comes first 及时的工程设计是第一位的

Start with prompt engineering. Use all the techniques we discussed in the tactics section before. Chain-of-thought, n-shot examples, and structured input and output are almost always a good idea. Prototype with the most highly capable models before trying to squeeze performance out of weaker models.

从即时工程开始。使用我们之前在战术部分讨论过的所有技术。思想链、n-shot 示例以及结构化输入和输出几乎总是一个好主意。在尝试从较弱的模型中挤出性能之前，先使用最强大的模型进行原型设计。

Only if prompt engineering cannot achieve the desired level of performance should you consider fine-tuning. This will come up more often if there are nonfunctional requirements (e.g., data privacy, complete control, and cost) that block the use of proprietary models and thus require you to self-host. Just make sure those same privacy requirements don’t block you from using user data for fine-tuning!

仅当即时工程无法达到所需的性能水平时，您才应考虑进行微调。如果存在阻止使用专有模型并因此要求您自行托管的非功能性需求（例如数据隐私、完全控制和成本），这种情况会更频繁地出现。只需确保这些相同的隐私要求不会阻止您使用用户数据进行微调即可！

### Build evals and kickstart a data flywheel 构建评估并启动数据飞轮

Even teams that are just getting started need evals. Otherwise, you won’t know whether your prompt engineering is sufficient or when your fine-tuned model is ready to replace the base model.

即使是刚刚起步的团队也需要评估。否则，您将不知道您的即时工程是否足够，或者您的微调模型何时准备好替换基础模型。

[Effective evals](https://hamel.dev/blog/posts/evals/) are specific to your tasks and mirror the intended use cases. The first level of evals that we recommend is unit testing. These simple assertions detect known or hypothesized failure modes and help drive early design decisions. Also see other task-specific evals for classification, summarization, etc.

有效的评估特定于您的任务并反映预期的用例。我们建议的第一级评估是单元测试。这些简单的断言可检测已知或假设的故障模式，并有助于推动早期设计决策。另请参阅其他特定于任务的分类、摘要等评估。

While unit tests and model-based evaluations are useful, they don’t replace the need for human evaluation. Have people use your model/product and provide feedback. This serves the dual purpose of measuring real-world performance and defect rates while also collecting high-quality annotated data that can be used to fine-tune future models. This creates a positive feedback loop, or data flywheel, which compounds over time:

虽然单元测试和基于模型的评估很有用，但它们并不能取代人工评估的需要。让人们使用您的模型/产品并提供反馈。这具有测量现实世界性能和缺陷率的双重目的，同时还收集可用于微调未来模型的高质量注释数据。这会创建一个正反馈循环或数据飞轮，并随着时间的推移而复合：

+   Use human evaluation to assess model performance and/or find defects

    使用人工评估来评估模型性能和/或发现缺陷

+   Use the annotated data to fine-tune the model or update the prompt

    使用带注释的数据微调模型或更新提示

+   Repeat

    重复

For example, when auditing LLM-generated summaries for defects we might label each sentence with fine-grained feedback identifying factual inconsistency, irrelevance, or poor style. We can then use these factual inconsistency annotations to train a hallucination classifier or use the relevance annotations to train a reward model to score on relevance. As another example, LinkedIn shared about its success with using model-based evaluators to estimate hallucinations, responsible AI violations, coherence, etc. in its write-up.

例如，在审核 LLM 生成的缺陷摘要时，我们可能会用细粒度的反馈来标记每个句子，以识别事实不一致、不相关或风格不佳的情况。然后，我们可以使用这些事实不一致注释来训练幻觉分类器，或使用相关性注释来训练奖励模型以对相关性进行评分。另一个例子是，LinkedIn 在其文章中分享了其使用基于模型的评估器来评估幻觉、负责任的人工智能违规行为、连贯性等方面的成功。

By creating assets that compound their value over time, we upgrade building evals from a purely operational expense to a strategic investment and build our data flywheel in the process.

通过创建随着时间的推移而增值的资产，我们将建筑评估从纯粹的运营支出升级为战略投资，并在此过程中构建我们的数据飞轮。

### The high-level trend of low-cost cognition 低成本认知的高级趋势

In 1971, the researchers at Xerox PARC predicted the future: the world of networked personal computers that we are now living in. They helped birth that future by playing pivotal roles in the invention of the technologies that made it possible, from Ethernet and graphics rendering to the mouse and the window.

1971 年，施乐帕洛阿尔托研究中心的研究人员预测了未来：我们现在生活的联网个人计算机的世界。他们在以太网和图形渲染技术的发明中发挥了关键作用，帮助创造了这个未来。到鼠标和窗口。

But they also engaged in a simple exercise: they looked at applications that were very useful (e.g., video displays) but were not yet economical (i.e., enough RAM to drive a video display was many thousands of dollars). Then they looked at historic price trends for that technology (à la Moore’s law) and predicted when those technologies would become economical.

但他们也进行了一项简单的练习：他们研究了非常有用的应用程序（例如视频显示器）但还不经济（即驱动视频显示器的足够 RAM 需要数千美元）。然后，他们研究了该技术的历史价格趋势（摩尔定律），并预测这些技术何时变得经济。

We can do the same for LLM technologies, even though we don’t have something quite as clean as transistors-per-dollar to work with. Take a popular, long-standing benchmark, like the Massively-Multitask Language Understanding dataset, and a consistent input approach (five-shot prompting). Then, compare the cost to run language models with various performance levels on this benchmark over time.

我们可以对LLM技术做同样的事情，即使我们没有像每美元晶体管那么干净的东西可以使用。采用流行的、长期存在的基准，例如大规模多任务语言理解数据集，以及一致的输入方法（五次提示）。然后，比较随着时间的推移在此基准上运行具有不同性能级别的语言模型的成本。

For a fixed cost, capabilities are rapidly increasing. For a fixed capability level, costs are rapidly decreasing. Created by coauthor Charles Frye using public data on May 13, 2024.

对于固定成本，能力正在迅速增强。对于固定的能力水平，成本正在迅速下降。由合著者 Charles Frye 使用公共数据于 2024 年 5 月 13 日创建。

In the four years since the launch of OpenAI’s davinci model as an API, the cost for running a model with equivalent performance on that task at the scale of one million tokens (about one hundred copies of this document) has dropped from $20 to less than 10¢—a halving time of just six months. Similarly, the cost to run Meta’s LLama 3 8B via an API provider or on your own is just 20¢ per million tokens as of May 2024, and it has similar performance to OpenAI’s text-davinci-003, the model that enabled ChatGPT to shock the world. That model also cost about $20 per million tokens when it was released in late November 2023. That’s two orders of magnitude in just 18 months—the same time frame in which Moore’s law predicts a mere doubling.

自 OpenAI 的达芬奇模型作为 API 推出以来的四年里，在 100 万个代币（约一百份本文档）规模的任务上运行具有同等性能的模型的成本已从 20 美元降至不到10 美分——减半时间仅需六个月。同样，截至 2024 年 5 月，通过 API 提供商或自行运行 Meta 的 LLama 3 8B 的成本仅为每百万代币 20 美分，并且它具有与 OpenAI 的 text-davinci-003 类似的性能，该模型使 ChatGPT 震惊世界。该模型在 2023 年 11 月下旬发布时，每百万代币的成本约为 20 美元。在短短 18 个月内，这个价格就上涨了两个数量级——摩尔定律预测的时间范围仅为翻倍。

Now, let’s consider an application of LLMs that is [very useful](https://arxiv.org/abs/2304.03442) (powering generative video game [characters](https://arxiv.org/abs/2310.02172), à la Park et al.) but is not yet economical. (Their cost was estimated at $625 per hour here.) Since that paper was published in August 2023, the cost has dropped roughly one order of magnitude, to $62.50 per hour. We might expect it to drop to $6.25 per hour in another nine months.

现在，让我们考虑 LLMs 的一个应用程序，它非常有用（为生成视频游戏角色提供动力，à la Park 等人），但还不经济。 （这里的成本估计为每小时 625 美元。）自该论文于 2023 年 8 月发表以来，成本下降了大约一个数量级，达到每小时 62.50 美元。我们预计在接下来的 9 个月内它会降至每小时 6.25 美元。

Meanwhile, when Pac-Man was released in 1980, $1 of today’s money would buy you a credit, good to play for a few minutes or tens of minutes—call it six games per hour, or $6 per hour. This napkin math suggests that a compelling LLM-enhanced gaming experience will become economical some time in 2025.

与此同时，当《吃豆人》于 1980 年发布时，今天的 1 美元就可以给你买一个积分，可以玩几分钟或几十分钟——称之为每小时 6 场游戏，或每小时 6 美元。这个餐巾纸数学表明，引人注目的LLM增强游戏体验将在 2025 年的某个时候变得经济实惠。



These trends are new, only a few years old. But there is little reason to expect this process to slow down in the next few years. Even as we perhaps use up low-hanging fruit in algorithms and datasets, like scaling past the “Chinchilla ratio” of ~20 tokens per parameter, deeper innovations and investments inside the data center and at the silicon layer promise to pick up slack.

这些趋势是新的，只有几年的历史。但没有理由预计这一进程在未来几年会放缓。即使我们可能会在算法和数据集中使用容易实现的成果，例如扩展到每个参数约 20 个代币的“Chinchilla 比率”，但数据中心和硅层内部更深入的创新和投资有望弥补这一不足。

And this is perhaps the most important strategic fact: what is a completely infeasible floor demo or research paper today will become a premium feature in a few years and then a commodity shortly after. We should build our systems, and our organizations, with this in mind.

这也许是最重要的战略事实：今天完全不可行的现场演示或研究论文将在几年内成为一项高级功能，然后不久之后就会成为一种商品。我们应该牢记这一点来构建我们的系统和我们的组织。

## Enough 0 to 1 Demos, It’s Time for 1 to N Products 0 到 1 的演示已经够多了，是时候展示 1 到 N 个产品了

We get it; building LLM demos is a ton of fun. With just a few lines of code, a vector database, and a carefully crafted prompt, we create ✨magic ✨. And in the past year, this magic has been compared to the internet, the smartphone, and even the printing press.

我们懂了;构建 LLM 演示非常有趣。只需几行代码、一个矢量数据库和精心设计的提示，我们就创造了✨神奇✨。在过去的一年里，人们将这种魔力与互联网、智能手机甚至印刷机进行比较。

Unfortunately, as anyone who has worked on shipping real-world software knows, there’s a world of difference between a demo that works in a controlled setting and a product that operates reliably at scale.

不幸的是，任何从事过实际软件开发工作的人都知道，在受控环境中运行的演示与大规模可靠运行的产品之间存在天壤之别。

Take, for example, self-driving cars. The firs[t ca](https://proceedings.neurips.cc/paper/1988/file/812b4ba287f5ee0bc9d43bbf5bbe87fb-Paper.pdf)r was driven by a neural network in 1988. Twenty-five years later, Andrej Karpathy took his first demo ride in a Waymo. A decade after that, the company received its driverless permit. That’s thirty-five years of rigorous engineering, testing, refinement, and regulatory navigation to go from prototype to commercial product.

以自动驾驶汽车为例。 1988 年，第一辆汽车由神经网络驱动。二十五年后，Andrej Karpathy 第一次试驾了 Waymo。十年后，该公司获得了无人驾驶许可证。从原型到商业产品，经历了三十五年严格的工程、测试、改进和监管导航。

Across different parts of industry and academia, we have keenly observed the ups and downs for the past year: year 1 of N for LLM applications. We hope that the lessons we have learned—from tactics like rigorous operational techniques for building teams to strategic perspectives like which capabilities to build internally—help you in year 2 and beyond, as we all build on this exciting new technology together.

在工业界和学术界的不同领域，我们敏锐地观察到了过去一年的起起落落：LLM应用程序的 N 年 1。我们希望我们学到的经验教训——从建立团队的严格运营技术等策略到内部构建哪些能力等战略视角——在第二年及以后对您有所帮助，因为我们都在这项令人兴奋的新技术的基础上共同构建。

## About the authors 关于作者

**Eugene Yan** designs, builds, and operates machine learning systems that serve customers at scale. He’s currently a Senior Applied Scientist at Amazon where he builds RecSys for m[illions world](https://eugeneyan.com/)wide [and applies LL](https://applyingml.com/)Ms to serve customers better. Previously, he led machine learning at Lazada (acquired by Alibaba) and a Healthtech Series A. He writes & speaks about ML, RecSys, LLMs, and engineering at eugeneyan.com and ApplyingML.com.

Eugene Yan 设计、构建和运营为大规模客户服务的机器学习系统。他目前是 Amazon 的高级应用科学家，为全球数百万人构建 RecSys，并应用 LLMs 更好地为客户服务。此前，他领导过 Lazada（已被阿里巴巴收购）的机器学习和 Healthtech A 轮融资。他在 eugeneyan.com 和 ApplyingML.com 撰写并谈论 ML、RecSys、LLMs 以及工程。

**Bryan Bischof** is the Head of AI at Hex, where he leads the team of engineers building Magic – the data science and analytics copilot. Bryan has worked all over the data stack leading teams in analytics, machine learning engineering, data platform engineering, and AI engineering. He started the data team at Blue Bottle Coffee, led several projects at Stitch Fix, and built the data teams at Weights and Biases. Bryan previously co-authored the book Building Production Recommendation Systems with O’Reilly, and teaches Data Science and Analytics in the graduate school at Rutgers. His Ph.D. is in pure mathematics.

Bryan Bischof 是 Hex 的人工智能主管，他领导着构建 Magic 的工程师团队——数据科学和分析副驾驶。 Bryan 曾在分析、机器学习工程、数据平台工程和人工智能工程的数据堆栈领导团队中工作过。他在 Blue Bottle Coffee 组建了数据团队，在 Stitch Fix 领导了多个项目，并在 Weights and Biases 建立了数据团队。 Bryan 此前与 O’Reilly 合着了《构建生产推荐系统》一书，并在罗格斯大学研究生院教授数据科学和分析。他的博士学位。属于纯数学。

**Charles Frye** teach[es people to](https://journals.physiology.org/doi/full/10.1152/jn.00172.2016) build AI applications. After publishing research in psychopharmacology and neurobiology, he got his Ph.D. at the University of California, Berkeley, for dissertation work on neural network optimization. He has taught thousands the entire stack of AI application development, from linear algebra fundamentals to GPU arcana and building defensible businesses, through educational and consulting work at Weights and Biases, Full Stack Deep Learning, and Modal.

Charles Frye 教人们构建人工智能应用程序。在发表精神药理学和神经生物学研究后，他获得了博士学位。在加州大学伯克利分校攻读神经网络优化论文。他通过权重和偏差、全栈深度学习和模态方面的教育和咨询工作，教授了数千人人工智能应用程序开发的整个堆栈，从线性代数基础知识到 GPU 奥秘，再到构建可防御的业务。

**Hamel Husain** [is a machi](https://www.linkedin.com/in/hamelhusain/)ne learning engineer with over 25 years of experience. He has worked with innovative companies such as Airbnb and GitHub, which included early LLM research used by OpenAI for code understanding. He has also led and contributed to numerous popular open-source machine-learning tools. Hamel is currently an independent consultant helping companies operationalize Large Language Models (LLMs) to accelerate their AI product journey.

Hamel Husain 是一位拥有超过 25 年经验的机器学习工程师。他曾与 Airbnb 和 GitHub 等创新公司合作，其中包括 OpenAI 用于代码理解的早期 LLM 研究。他还领导并贡献了许多流行的开源机器学习工具。 Hamel 目前是一名独立顾问，帮助公司实施大型语言模型 (LLMs)，以加速他们的 AI 产品之旅。

**Jason Liu** is a distinguish[ed machine](https://jxnl.co/services/) learning consultant known for leading teams to successfully ship AI products. Jason’s technical expertise covers personalization algorithms, search optimization, synthetic data generation, and MLOps systems.

Jason Liu 是一位杰出的机器学习顾问，以领导团队成功交付人工智能产品而闻名。 Jason 的技术专长涵盖个性化算法、搜索优化、合成数据生成和 MLOps 系统。

His experience includes companies like Stitch Fix, where he created a recommendation framework and observability tools that handled 350 million daily requests. Additional roles have included Meta, NYU, and startups such as Limitless AI and Trunk Tools.

他曾在 Stitch Fix 等公司工作过，在那里他创建了一个推荐框架和可观察性工具，每天处理 3.5 亿个请求。其他角色包括 Meta、纽约大学以及 Limitless AI 和 Trunk Tools 等初创公司。

**Shreya Shankar** is an ML engineer and PhD student in computer science at UC Berkeley. She was the first ML engineer at 2 startups, building AI-powered products from scratch that serve thousands of users daily. As a researcher, her work focuses on addressing data challenges in production ML systems through a human-centered approach. Her work has appeared in top data management and human-computer interaction venues like VLDB, SIGMOD, CIDR, and CSCW.

Shreya Shankar 是加州大学伯克利分校的机器学习工程师和计算机科学博士生。她是两家初创公司的第一位机器学习工程师，从头开始构建人工智能驱动的产品，每天为数千名用户提供服务。作为一名研究人员，她的工作重点是通过以人为本的方法解决生产机器学习系统中的数据挑战。她的作品曾出现在VLDB、SIGMOD、CIDR、CSCW等顶级数据管理和人机交互场所。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>
