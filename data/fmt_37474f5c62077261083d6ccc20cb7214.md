
# 【斯坦福】汤森路透和 LexisNexis 的 AI 法律研究工具“幻觉”都很高

> 来源：[https://docs.feishu.cn/docx/TS4ad23x1o1iqUxa8CXctmhSn2e](https://docs.feishu.cn/docx/TS4ad23x1o1iqUxa8CXctmhSn2e)

---

5 月 30 日，斯坦福大学发布了一份名为《“无幻觉？评估领先的 AI 法律研究工具的可靠性》的论文（修订版）。

这篇论文重点分析了两家头部法律研究公司 Thomson Reuters 和 LexisNexis 旗下的 AI 法律研究工具的可靠性，特别是它们在生成内容时是否会出现“幻觉”（hallucinations）—— 即编造虚假信息。

论文地址：

`https://dho.stanford.edu/wp-content/uploads/Legal_RAG_Hallucinations.pdf`

目前，AI 在法律实践的使用急剧增加，这些工具旨在协助法律专业人士处理案例搜索、总结、文件起草等一系列核心法律任务。但是，这些工具使用的大语言模型容易出现“幻觉”，在高风险领域中使用会存在风险。

尽管一些法律研究公司声称，他们采用的方法（如检索增强生成/RAG）可以“消除”或者“避免”幻觉的出现，但由于这些系统的封闭性，评估这些说法的真实性存在挑战。

为此，斯坦福大学下设以人为本的人工智能（HAI）研究中心构建了一个包含 200 多个法律查询的数据集，并分别在 LexisNexis（Lexis+ AI）、Thomson Reuters（Ask Practical Law AI）、Westlaw（AI-Assisted Research）和 GPT-4 上测试，手动审查它们的输出，以评估这些产品的输出准确性。

出乎意料的是，尽管与通用聊天机器人（如 GPT-4）相比，Thomson Reuters 和 LexisNexis 旗下 AI 法律研究工具的“幻觉”现象有所减少，但“幻觉”程度仍然很高。

论文发现，Westlaw 的幻觉频率，几乎是 Lexis+ AI 的两倍 —— Lexis+ AI 的幻觉时间为 17%，Westlaw 的幻觉时间则高达 33%。同时，Lexis+ AI 在 65% 的时间提供了准确的答案，而 Westlaw 提供了准确答案的时间只有 42% —— 这些数据比两家公司所宣传的次数要多得多，揭露了法律科技公司们长期以来对自家产品的炒作，言过其实。

![](img/871f819550b49d0612f65abedeb0c492.png)

论文发布后，包括 Thomson Reuters 和 LexisNexis 在内的很多法律科技公司和法律专业人士都拒绝了该论文的结论，声称论文的统计方法错误，Thomson Reuters 还表示论文团队使用了错误的工具测试。

然而，更新后的论文仍然呈现以上结果。

对此，Thomson Reuters 团队撰文表示：

> “我们非常支持像这样的测试和基准解决方案的努力，我们支持斯坦福研究团队最近对基于 RAG 的法律研究解决方案进行研究的意图，但当我们看到 AI 辅助研究存在与幻觉有关的重大问题时，我们感到非常惊讶。事实上，该论文的结果与我们自己的测试和客户的反馈截然不同。

> 我们致力于与论文的研究人员合作以了解更多信息，但根据我的经验，该研究比我们内部测试的不准确性率更高的一个原因可能是，研究包括我们在 AI 辅助研究中很少或从未见过的问题类型。这里学到的一个关键教训是，这些产品的用户体验可以更明确地说明系统的具体限制。”

其中，出现这些差异的原因可能是 Thomson Reuters、LexisNexis、斯坦福，三方对于“幻觉”的定义略有不同。

例如，Thomson Reuters 认为对查询不准确的输出就是“幻觉”[，LexisNexis](https://www.lexisnexis.com/community/insights/legal/b/product-features/posts/how-lexis-ai-delivers-hallucination-free-linked-legal-citations) 表示基于有链接的法律引用其 AI 法律研究产品可以达到“100% 无幻觉”，而斯坦福 RegLab 和 HAI 研究中心对“幻觉”的定义除了包括对事实不准确的回应，还包括引用了错误的来源。

因此，Thomson Reuters 和 LexisNexis 最初都声称，他们内部测试“幻觉”显示的数据，低于这份论文的结论。

然而，关于法律 AI 产品的准确性问题，每家公司都有自己公布的数据和一套说辞，但似乎都没有很大的说服力。

这个问题的背后，是当下仍然缺乏对大模型可靠的「基准测试」方法—— 这是一个具有很高挑战性和资源密集型的研究领域，特别是在法律等专家领域，这对于负责任地使用 AI 至关重要。
