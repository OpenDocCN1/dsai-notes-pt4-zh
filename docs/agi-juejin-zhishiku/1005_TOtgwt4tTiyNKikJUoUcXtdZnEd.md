
# RAG 2.0谷歌RICHES：边检索、边思考、边生成

> 来源：https://agijuejin.feishu.cn/docx/JdBGdkpfwoSADyxAtgQcbMXtnUf
>
> 作者：PaperAgent
>
> 原文：https://mp.weixin.qq.com/s/Tc-MVeGqwdVH1xMzDVJUww

RAG 2.0方法由 `contextual.ai` 推出，它将预训练、微调和对齐所有组件作为一个单一的集成系统，通过大模型和检索器进行反向传播以最大化性能。旨在解决 RAG 面临的各个组件技术是有效，但整体远非最佳的问题。

Google DeepMind 提出一种新颖的方法 RICHES（Retrieval Interlaced with Sequence Generation），通过单一的 LLM 和解码过程，将文本生成与文档检索原生地交织在一起。无需单独的检索器和生成器，直接解码文档内容或相关的自然语言检索键。无需额外训练，即可通过提示适应多样的新任务。

示例 RICHES 输出，用于具有单个大型语言模型（LLM）和解码通道的多跳查询。绿色引用文本是从检索语料库中"检索"或逐字生成的。RICHES 生成原生地交织了思考和多个检索证据。

![](img/d0f4a25d553e4c8d4db1fa926e5d7af7.png)

## RICHES 工作流程（太长不看版）

1. 初始化模型：选择一个适合的预训练大型语言模型（LLM）。
2. 定义检索键：确定用于检索的文档标识符，如标题、段落、句子或命题。
3. 构建索引：使用 `FM-Index` 等技术为语料库构建索引，优化检索效率。
4. 接收输入：接收用户的问题或查询作为输入。
5. 交替生成：LLM 交替进行自由文本生成和受限检索键生成。
6. 应用约束：在生成过程中，利用索引对检索键进行约束，确保它们对应于语料库中的有效文档。
7. 检索文档：根据生成的检索键，从语料库中检索相关文档或信息片段。
8. 整合与输出：将检索到的内容与生成的文本结合，形成完整的回答或解决方案。
9. 评估：使用适当的评估指标（如 F1 分数、AutoAIS）对输出结果进行评估。
10. 迭代优化：根据评估结果进行模型和流程的迭代改进。

## RICHES 详细原理

### 1. 检索与生成的交织

+ RICHES 通过直接解码文档内容或相关的自然语言检索键来检索文档，这些检索键指向生成它们的文档。
+ 这种方法允许在一个单一的解码过程中将文本生成与检索交织在一起，从而避免了使用单独的检索器和生成器。

### 2. 检索键的定义

+ 检索键是存在于预定义的有限序列集 `K` 中的一个标记序列，每个条目都与底层语料库 `C` 中的一个或多个文档相关联。
+ 在输出序列 `y` 中标记检索键的开始和结束使用特殊标记 `«and»`。

### 3. 概率模型的更新

+ 通过引入一个指示函数 `1 K (q)`，将标准的自回归语言建模概率 `Pθ(y|x)` 更新为包括检索键的概率模型。
+ 该模型通过零化不允许的序列的继续概率来实现约束解码。

### 4. 约束束解码（Constrained Beam Decoding）

+ 使用束搜索（Beam Search）作为解码策略，模拟启发式的最优优先搜索。
+ 在每个时间步，LLM 估计每个节点（标记）的值，并将其添加到固定大小的队列（束）中。
+ 对查询“马拉松何时更名为士力架？”的约束束可视化。最终 RICHES 输出为“马拉松在 1990 年更名为士力架”。加粗的框追踪了顶部束序列的进展。灰色划掉的框是 LLM（大型语言模型）偏好但被语料库约束阻止的序列。

![](img/8755512e4292da37eccaffbf78f1597e.png)

### 5. 通过 FM-Index 实现高效约束

+ 使用 `FM-Index`（Ferragina 和 Manzini，2000）来约束解码过程中的模型输出，确保输出序列在语料库中存在。
+ `FM-Index` 是一个压缩的后缀数组，支持快速子串搜索操作。

![](img/e7549973c98fb636471125f1e33e998e.png)

### 6. 自适应束大小

+ 引入自适应解码策略，根据生成的约束序列和非约束序列的不同需求，动态调整束大小。
+ 约束序列需要精确匹配目标检索键，而非约束序列则更灵活。

### 7. 索引策略

+ `FM-Index` 支持高效索引语料库中的所有子串，但文档表示方式的选择对检索效果有重要影响。
+ RICHES 支持多种索引策略，包括文档标题、段落子字符串、句子子字符串和命题索引。

RICHES 在开放领域问答（归因问答、多跳问答和检索思考）任务上展现出强大的性能，与传统的检索增强生成方法相比，在多跳问答任务（Hotpot）上表现尤为出色，能够通过单一的解码过程实现更准确的答案生成。

## RICHES 的综合性能比较

对于密集检索器，检索并获取前 `k` 个文档并将其输入到少量射击 `Answerer` 中，其中 GTR 段落的 `k=1`，GTR 命题的 `k=2`。对于迭代检索，每个步骤最多检索 4 个文档，每个步骤的 `k=1`。

![](img/2b1d30b37e1bb1eda0c1dd0fc984a5ae.png)

RICHES 与密集检索在单跳问答（QA）中的示例对比。仅展示检索到的文本以供说明。

![](img/3c6ddeeafba4d1db4fbb2b8c8517d68f.png)

来自 RICHES 的迭代检索输出示例。备注以（# 评论）的形式进行了注释。

![](img/6cf34be330c6c987f76b50ad1c946f9a.png)

还讨论了 RICHES 在不同索引策略、束搜索（beam search）大小的对比效果：

![](img/10e0f6137a6b3cab1e0b6004342f6a2d.png)

命题（Proposition）检索键的效果是最好的

![](img/7cbe299c9a2420393bd9af4021d508fb.png)

![](img/ade7ae3abe15e46426d69188a7f35ca9.png)

## 附录：

### 用于多跳问答的 RICHES 的 Few-shot Prompt 模版

```
For given input query, write 1-3 passages to answer the query. Write a hint keyword and a passage
contained within « and ». A passage must be a complete sentence and not a phrase. It must contain
complete context for answering the query and should not begin with it, he, they etc. Do not repeat any
passages. Aim for new keywords.

question: The football manager who recruited Cristiano Ronaldo managed Manchester United during
what timeframe?
passage: keyword: Cristiano Ronaldo’s recruiting manager « Alex Ferguson recruited Cristiano Ronaldo »
keyword: Sir Alex Ferguson’s tenure at Manchester United « Sir Alex Ferguson managed Manchester
United from 1986 to 2013. »
answer: 1986 to 2013

question: Were Eatza Pizza and Your Pie founded in the same state?
passage: keyword: Eatza Pizza founded in state « Eatza Pizza was founded in Arizona » keyword: Your
Pie founded in state « Your Pie was founded in Athens, Georgia »
answer: no

question: In which stadium do the teams owned by Myra Kraft’s husband play?
passage: keyword: Myra Kraft’s husband « Robert Kraft’s wife is Myra Kraft. » keyword: Robert Kraft’s
team « Robert Kraft is the owner of the New England Patriots. » keyword: New England Patriots stadium
« Gillette Stadium is the home of the New England Patriots. »
answer: Gillette Stadium

question: &lt;question&gt;
passage:
```

### 用于单跳问答的 RICHES 的 Few-shot Prompt 模版

```
For given input query, write 1-3 passages to answer the query. Write a hint keyword and a passage
contained within « and ». A passage must be a complete sentence and not a phrase. It must contain
complete context for answering the query and should not begin with it, he, they etc. Do not repeat any
passages. Aim for new keywords.

question: who is the owner of phoenix mall pune?
passage: keyword: Phoenix Market City owner « Phoenix Market City is developed by Phoenix Mills
Limited. »
answer: Phoenix Mills Limited

question: what brings in more money nba or nfl?
passage: keyword: NFL revenues « NFL revenues are well over $10 billion per season. » keyword: NBA
revenue « NBA amasses about $6 billion annually. »
answer: NFL

question: when was the french national anthem adopted?
passage: keyword: French national anthem « La Marseillaise became the national anthem of France. »
keyword: La Marseillaise adoption « La Marseillaise was adopted by France in 1795. »
answer: 1795

question: question
passage:
```

### 从命题中提取答案的 Few-shot Prompt 模版

```
Answer the ’question’ only based on the given ’passage’. If the ’passage’ lacks context or is not relevant,
say ’Cannot answer’ else say generate a short answer. Do not answer the query from outside the scope of
the passage.

question: what brings in more money nba or nfl?
passage: NFL revenues are well over $10 billion per season. NBA amasses about $6 billion annually.
answer: NFL

question: when did they put warnings on cigarette packs
passage: Tobacco packaging 1978’s warning was not removed, so now every cigarette pack contains both
warnings (one on each lateral).
answer: Cannot Answer

question: when was the french national anthem adopted?
passage: La Marseillaise became the national anthem of France. La Marseillaise was adopted by France
in 1795.
answer: 1795

question: question
passage: passage
answer:
```

## 约束解码过程说明

给定前缀“Joker is played by”，续接词“Nolan”在语料库中未找到，因此被屏蔽掉。

![](img/aad50e3ff4c3d03001f9cccce9cc4374.png)

```
https://arxiv.org/pdf/2407.00361
From RAG to RICHES: Retrieval Interlaced with Sequence Generation
Google Deepmind
```

## 推荐阅读

+ 对齐 LLM 偏好的直接偏好优化方法：DPO、IPO、KTO
+ 一篇搭建 AI 大模型应用平台架构的全面指南
+ [RAG全景图：从RAG启蒙到高级RAG之36技，再到终章Agentic RAG！](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247487375&idx=1&sn=e16bc2fdaac04e58e99cfd2a1dc0b0cb&chksm=c2ce35fef5b9bce80dcf0a70b753707036fe7f962d7888d60396490841a538b0f46e952f53f0&scene=21#wechat_redirect)
+ [Agent到多模态Agent再到多模态Multi-Agents系统的发展与案例讲解（1.2万字，20+文献，27张图）](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247485322&idx=1&sn=71ffb345fca514aa5ce2848cb2c9f071&chksm=c2ce3dfbf5b9b4edd5b98e45c6179890bdea748fb5220636d25f42006954ea5c81afa8735725&scene=21#wechat_redirect)

欢迎关注我的👸号“PaperAgent”，每天一篇大模型（LLM）文章来锻炼我们的思维，简单的例子，不简单的方法，提升自己。
