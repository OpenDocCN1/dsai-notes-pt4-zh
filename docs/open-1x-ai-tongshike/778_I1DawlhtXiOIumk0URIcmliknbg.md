# 告别粗暴切分：入门 RAG 文本分块，从策略到优化

> 来源：[https://datawhaler.feishu.cn/docx/Jo3cdIzAKoTSnjx7WtzclIVinAG](https://datawhaler.feishu.cn/docx/Jo3cdIzAKoTSnjx7WtzclIVinAG)

# 告别粗暴切分：入门 RAG 文本分块，从策略到优化

## 🎯 文章目标

本文面向 RAG 爱好者和对大模型应用感兴趣的朋友，旨在帮助大家：

+   深刻理解文本分块（Chunking）在 RAG 中的核心作用：了解为何恰当的分块是构建高性能 RAG 系统的基石。

+   **掌握多种文本分块策略**：熟悉从基础到进阶的各种分块方法，理解其原理、优缺点及适用场景。

+   **具备实现和优化分块策略的能力**：能够根据具体需求选择并实现合适的分块方法，并进行初步优化。

> **💡 小提示

> **本文将重点介绍多种分块策略的概念，并提供混合分块策略的 Python 实现代码。理解 RAG 基础知识有助于更好地消化本文内容。

> 本次文章代码开源地址：[https://github.com/li-xiu-qi/XiaokeAIL](https://github.com/li-xiu-qi/XiaokeAILabs/tree/main/datas/test_hybrid_chunking)abs/tree/main/datas/test_hybrid_chunking

## 📄 主题

**本次主题**：RAG 系统中的文本分块（Text Chunking）

+   探讨文本分块的必要性及其对 RAG 性能的影响。

+   详解固定大小、句子、递归、文档结构和混合等基础分块策略。

+   介绍语义分块、分层分块、句子窗口等高级分块策略的概念。

+   提供基于 Markdown 文档结构的混合分块实现代码。

## 📚 摘要

检索增强生成（RAG）的性能在很大程度上取决于其检索模块的质量，而文本分块（Chunking）是决定检索质量的关键前置步骤。粗暴或不恰当的分块会导致信息丢失、上下文割裂、检索效率低下等问题，严重影响 RAG 系统的最终表现。本文系统性地探讨了文本分块的核心目标与挑战，从克服上下文窗口限制、提高检索精度、维护上下文完整性等角度阐述了其重要性。接着，详细介绍了固定大小、基于句子、递归字符、基于文档结构等基础分块策略，并给出了结合 Markdown 文档结构的混合分块策略的 Python 实现。此外，文章还展望了语义分块、分层分块、句子窗口检索等高级策略，为读者提供了更广阔的技术视野。掌握精妙的文本分块技术，是告别“粗暴切分”，打造高效、精准 RAG 应用的关键一步。

> 让我们一起深入 RAG 的“预处理”核心环节！

---

## 🚁 前言

检索增**强生**成（RAG）技**术**通过结合外部知识库与大语言模型（LLM）的生成能力，极大地提升了模型回答问题的准确性和时效性。这项技术已成为构建智能问答、内容生成等应用的热门选择。然而，光鲜亮丽的 RAG 应用背后，一个常常被忽视但至关重要的环节是——如何将庞大的原始文档切分成适合检索的“块”（Chunks）。

许多开发者在构建 RAG 系统时，可能会图省事，采用最简单直接的方法，比如按固定字数或段落进行“一刀切”。这种看似便捷的“粗暴”分块方式，却往往是导致 RAG 系统性能不佳、表现“智障”的“罪魁祸首”。想象一下：

1.  **上下文“腰斩”**：一个完整的逻辑链条、一段重要的代码示例，或者一个关键的定义，被无情地从中间切开。检索到的单个分块信息残缺，LLM 如同读到半句话，自然难以给出完整准确的答案。

1.  **关键信息“沉底”**：重要的信息恰好散落在多个分块的边缘，导致检索时要么只捞起片段，要么干脆被忽略，如同大海捞针。

1.  **检索“迷失方向”**：分块过大，包含太多噪音，降低了匹配精度；分块过小，语义信息不足，同样影响检索效果。这都会让检索过程效率低下，返回一堆“看似相关，实则无用”的结果。

因此，选择和设计合适的文本分块策略，绝非小事一桩。它如同为 RAG 系统打造信息高速公路前的地基工程，直接关系到后续检索器能否精准、高效地定位信息，进而决定了 LLM 能否基于高质量的上下文生成令人满意的答案。本文将带你系统梳理文本分块的策略、实现与优化，助你告别粗暴的切分，为你的 RAG 系统打下坚实、可靠的基础。

---

> ✨ 你好，我是筱可，欢迎来到「筱可 AI 研习社」！

> **🚀 标签关键词**

> | AI 实战派开发者 | 技术成长陪伴者 | RAG 前沿探索者 | 文档处理先锋 |

## 🐱一、为何 RAG 离不开文本分块？

在我们深入探讨各种分块策略之前，必须先搞清楚：为什么文本分块在 RAG 架构中如此不可或缺？它究竟解决了什么核心问题？

### 🐷1.1 定义与核心目标

文本分块（Text Chunking / Splitting），顾名思义，就是将原始的、可能非常庞大的文本资料（例如，一篇长篇报告、一本电子书、一个复杂的网页或者大量的 API 文档）分割成一系列更小、更易于处理的文本片段（Chunks）的过程。这些 Chunks 是 RAG 系统中信息处理的基本单元，它们将被送入 Embedding 模型进行向量化，然后存入向量数据库进行索引，最终服务于检索环节。

文本分块的核心目标，可以归纳为以下几点：

1.  **克服上下文窗口限制 (Overcoming Context** Window Limitations)：这是最直接的原因。目前所有的大语言模型（LLM），无论是 GPT 系列、Claude 还是 Llama，都有其能够一次性处理的上下文长度限制（Context Window）。原始文档往往远超这个限制（几千到几十万 Tokens 不等）。如果不进行分块，根本无法将完整的长文档信息有效地提供给 LLM。分块确保了输入给 LLM 的每一段信息都在其“消化能力”范围之内。

1.  **提**高检索精度与效率 (Improving Retrieval Accuracy and Efficiency)：

+   **精度**：想象一下，如果你的查询是“什么是 Transformer 的自注意力机制？”，一个精确包含该定义的、几百字的 Chunk 显然比一个包含整个 Transformer 论文（数千字）的 Chunk 更容易被检索算法（如向量相似度计算）精准命中。大块内容中无关信息多，会稀释相关信号，干扰相似度判断。小而美、语义集中的块更容易实现精确匹配。

+   **效率**：对大量小块文本进行向量化和索引，构建向量数据库，其检索速度通常远快于在整个原始文档集合中进行全文搜索。分块相当于对信息进行了有效的预处理和“分而治之”，使得向量数据库能够快速定位到最可能相关的几个信息片段，极大地提升了检索响应速度。

1.  **维护上下文完整性 (Maintaining C**ontextual Integrity)：虽然我们要切分，但理想的分块策略并非随意切割。好的分块应该尽可能地保持语义的连贯性和上下文的完整性。例如，不应在一个句子的中间断开，不应将一个完整的代码块或列表项拆散。目标是让每个 Chunk 本身就能携带相对完整的、有意义的信息单元。

### 🐰1.2 对 RAG 性能的深远影响

理解了分块的目标，我们就能明白它如何直接且深刻地影响 RAG 系统的最终性能。分块策略的选择，就像是给 RAG 系统配备了不同规格的“信息管道”，直接决定了流经其中的信息质量和效率，最终体现在两个核心环节：

+   检索**质量 (Re**trieval Quality)：这是最直接的影响。分块的粒度（大小）、内容边界的确定方式（按句子？按段落？按固定长度？）、以及块与块之间的关联方式（是否有重叠？），都直接决定了检索器（Retriever）能否在用户提出查询时，准确、全面地找到最相关的文本片段。糟糕的分块会导致检索器返回：

+   **不相关信息**：块太大，包含太多噪音。

+   **不完整信息**：块太小，或切割不当，丢失关键上下文。

+   **冗余信息**：多个块包含高度重叠或相似的内容。

+   生成质量 (Generation Quality)：RAG 的核心优势在于“检索增强生成”。生成器（Generator，即 LLM）的输出质量高度依赖于检索器提供的上下文信息（检索到的 Chunks）。如果检索到的 Chunks 本身质量就不高（例如，上下文割裂、信息缺失、关键点遗漏），那么即使 LLM 本身能力再强，也如同“巧妇难为无米之炊”，难以生成准确、流畅、逻辑连贯、令人满意的答案。高质量的检索是高质量生成的前提，而恰当的分块是高质量检索的基础。

### 🐶1.3 内在关联与核心挑战：精度 vs. 上下文

**文本分块并非没有挑战，其核心在**于一个**经典的权衡（Trade-off**）：检索精度（Precision） 与 上下文完整性（Context） 之间的平衡。

+   小块 (Smaller Chunks)：

+   **优**点 ：信息更聚焦，语义更集中。这使得查询向量更容易匹配到包含特定关键词或高度相关语义的小块，从而提高检索的精度。更容易命中“靶心”。

+   **缺点 ：可能丢失重要的上下文**信息。单个小块可能只包含一个事实片段，缺乏必要的背景、前提或后续解释，导致 LLM 无法理解完整的语境或回答需要综合信息的复杂问题。如同盲人摸象，只得其一隅。

+   大块 (Larger Chunks)：

+   **优点 ：能保留更丰富的上下文**信息，包含更长的逻辑链条或更完整的背景描述。这有助于 LLM 理解更复杂的概念、事件的前因后果或不同信息点之间的关系。

+   **缺点 ：可能包含较多与当前查询不直接相关的信息（噪音），从**而稀释了核心相关性信号，可能降低检索精度（匹配到包含相关词但整体主题跑偏的块）。同时，更大的块也增加了 LLM 处理的负担和潜在的幻觉风险。

因此，**选择**或设计分块策略的关键在于，如何根据你的数据特性（文本类型、结构复杂度、信息密度等）和具体的应用场景（问答、摘要、对话等），在这个“精度 vs. 上下文”上找到那个“最优解”。没有一种分块策略是万能的，理解这个核心挑战是后续选择和优化策略的基础。

---

## 🐶二、基础分块策略

掌握一些基础且常用的分块策略是构建 RAG 系统的起点。这些策略各有优劣，适用于不同的场景。

### 🧱 2.1 固定大小分块 (Fixed-size Chunking)

这是最简单、最“懒人”的方法。直接按照固定的字符数（Character Count）或 Token 数（Token Count）来切割文本。为了缓解在边界处强行切断语义的问题，通常会设置一个“重叠”（Overlap）大小。重叠部分意味着每个块的末尾会与下一个块的开头有一段重复的内容。

+   **核心思想**：设定一个 chunk_size（如 500 个字符）和一个 chunk_overlap（如 50 个字符）。从文本开头取 chunk_size 个字符作为第一个块，然后下一次从 start_index + chunk_size - chunk_overlap 的位置开始取下一个块，依此类推。

+   **优点** ：

+   实现极其简单，几乎不需要复杂的逻辑。

+   计算开销非常小，处理速度快。

+   对文本格式没有特殊要求。

+   **缺点** ：

+   **极易破坏语义完整性**：非常可能在句子中间、单词中间（如果按字符切）、代码行中间等不恰当的地方断开，导致上下文严重割裂。

+   **忽略文本结构**：完全无视段落、标题、列表等任何文本固有结构。固定大小对于信息密度不同、语言不同的文本效果可能差异巨大。同样的 500 字符，在信息密集的文本中可能只包含半个观点，在稀疏文本中可能包含好几个。

+   **适用场景**：

+   对文本结构要求不高的简单场景。

+   数据量极大，需要快速进行初步处理时。

+   作为更复杂分块策略（如递归分块）的最后“兜底”手段。

+   对上下文完整性要求不高的检索任务。

```
# 概念性示例 (非直接运行代码)
def fixed_size_chunking(text, chunk_size, chunk_overlap):
    chunks = []
    start_index = 0
    while start_index &lt; len(text):
        end_index = start_index + chunk_size
        chunks.append(text[start_index:end_index])
        start_index += chunk_size - chunk_overlap
        if start_index &gt;= len(text): # 避免因 overlap 超出
             break
    return chunks

# 假设 text 是你的长文本
# chunks = fixed_size_chunking(text, 500, 50)
```

> 注：先前的实验表明（参考上一篇文章：《向量相似度揭秘：长度、相关性与关键词如何“迷惑”Embedding模型？》），增加无关内容会降低向量相似度。因此，为避免 chunk_overlap 可能引入冗余或不相关信息，后续的混合分块实现将不再设置重叠部分。

### 📄 2.2 基于句子的分块 (Sentence Splitting)

这种策略试图尊重语言的自然边界——句子。它首先使用句子分割算法（如基于标点符号 .?!，或使用 NLP 库如 NLTK, SpaCy）将文本分割成独立的句子，然后将一个或多个连续的句子组合成一个 Chunk，使其大小接近目标范围。

+   **核心思想**：先切分成句子，再合并句子成块。可以简单地每个句子是一个块，也可以设定一个目标块大小，将连续的句子合并，直到接近该大小。同样可以引入句子级别的重叠（如一个块包含第 1-3 句，下一个块包含第 3-5 句）。

+   **优点** ：

+   **更好地保持语义完整性**：因为句子是表达相对完整意思的基本单位，所以很少会在句子内部断开。

+   比固定大小分块更符合自然语言的结构。

+   **缺点** ：

+   **句子长度差异大**：有的句子很短，有的很长，导致 Chunk 大小不均匀，可能影响后续处理和检索稳定性。

+   简单的基于标点的分割可能不准确（例如，Mr. Smith 中的 .）。需要更可靠的 NLP 工具。

+   对于代码、列表、或者没有明确句子结构的文本（如 JSON, YAML）效果不佳。

+   跨越多个句子的复杂语义关系可能仍然被切断。

+   **适用场景**：

+   处理结构良好、以完整句子为主的文本，如新闻文章、报告、小说等。

+   当保持句子层面的语义完整性比较重要时。

> 注：为简化实现并减少外部库依赖，后续的混合分块示例将不采用基于 NLP 库的句子分割，感兴趣的同学可以试试使用它来改进我们的分块器。

```
# 概念性示例 (使用简单的标点分割)
import re

def sentence_chunking(text, max_chunk_sentences=3):
    sentences = re.split(r&#x27;(?&lt;=[.?!])\s+&#x27;, text) # 简单按标点分割
    sentences = [s for s in sentences if s] # 去除空字符串
    chunks = []
    current_chunk_sentences = []
    for sentence in sentences:
        current_chunk_sentences.append(sentence)
        if len(current_chunk_sentences) &gt;= max_chunk_sentences:
            chunks.append(&quot; &quot;.join(current_chunk_sentences))
            current_chunk_sentences = [] # 开始新块
    if current_chunk_sentences: # 处理剩余句子
        chunks.append(&quot; &quot;.join(current_chunk_sentences))
    return chunks

# chunks = sentence_chunking(text, 3) # 每块最多包含3个句子
```

### 🧩 2.3 递归字符分块 (Recursive Character Text Splitting)

这是 LangChain 等框架中常用的一种更智能的策略。它试图按一个预设的“分隔符”优先级列表来递归地分割文本。

+   **核心思想**：提供一个分隔符列表，按优先级从高到低尝试分割。例如，优先尝试按 \n\n (段落) 分割，如果分割后的块仍然太大，再尝试按 \n (换行符) 分割，然后按空格 `` 分割，最后如果还太大，就按字符 "" 分割。目标是在保持较大语义块（如段落）的同时，确保最终块大小不超过限制。

+   **优点** ：

+   **试图保持语义结构**：优先使用段落、换行等更有意义的分隔符，尽可能维持文本的逻辑结构。

+   **比固定大小更智能**：避免在不必要的地方断开。

+   **适应性强**：对不同类型的文本结构（段落、列表等）有一定适应性， 是固定大小和句子分割的一种折中和改进。

+   **缺点** ：

+   实现相对复杂一些。

+   效果依赖于分隔符列表的选择和优先级顺序。

+   对于没有明显分隔符的密集文本，可能最终退化为按字符分割。

+   **适用场景**：

+   通用性较好，适用于多种类型的文本文档。

+   当希望在控制块大小的同时尽可能保留文本结构时。

+   许多 RAG 框架的默认或推荐选项。

### 📑 2.4 基于文档结构的分块 (Document Structure-aware Chunking)

这种策略利用文档本身的结构信息进行分割，例如 HTML 的 <div>, <p>, <li> 标签，Markdown 的标题 #, ##, 列表 -, *，或者 JSON/YAML 的层级结构。

+   **核心思想**：解析文档的结构树或特定标记，基于这些结构元素来定义 Chunks。例如，每个 <p> 标签内容作为一个 Chunk，或者每个 Markdown 的二级标题下的所有内容作为一个 Chunk。

+   **优点** ：

+   **高度尊重原文结构**：能够最好地保持作者组织信息的方式。

+   **语义连贯性强**：通常一个结构元素（如段落、列表项）包含一个相对独立的语义单元。

+   可以方便地将结构信息（如标题、标签）作为元数据（Metadata）附加到 Chunk 上，这对后续检索很有帮助。

+   **缺点** ：

+   **依赖于清晰、一致的文档结构**：如果文档结构混乱或没有明确标记，则效果不佳。

+   不同结构元素的文本量可能差异巨大，导致 Chunk 大小极不均匀。例如，一个段落可能很短，另一个章节可能很长。

+   需要针对不同的文档格式（HTML, Markdown, LaTeX, JSON...）编写不同的解析逻辑。

+   **适用场景**：

+   处理具有清晰、标准化结构的文档，如网页、Markdown 文档、结构化数据（JSON/XML）等。

+   当需要利用文档结构信息进行检索或过滤时。

### 🔄 2.5 混合分块 (Hybrid Chunking)

顾名思义，混合策略结合了上述两种或多种方法的优点**，试图达到更好的平衡。一个常见的组合是：首先尝试基于文档结构（如 Markdown 标题**）进行高级别分割，然后在这些较大的分割块内部，如果它们仍然超过了目标大小，再使用递归字符分块或句子分块进行细粒度的切分。

+   **核心思想**：分层处理。先用结构化或语义边界（如标题）做粗粒度切分，得到逻辑上相关的“大块”，再对这些“大块”应用更细粒度的策略（如递归字符分割）来满足大小限制，同时保留“大块”的上下文信息（如将标题作为元数据）。

+   **优点** ：

+   **兼顾结构与大小限制**：既能利用文档的宏观结构，又能确保最终块大小可控。

+   **元数据丰富**：可以方便地继承来自结构化分割的元数据（如标题层级）。

+   灵活性高，可根据需求定制组合策略。

+   **缺点** ：

+   实现复杂度相对较高。

+   需要仔细设计组合逻辑和参数。

+   **适用场景**：

+   对分块质量要求较高，希望在保持上下文、利用结构和控制大小之间取得良好平衡的场景。

+   处理像 Markdown、富文本文档这样既有结构又有自由文本的内容。

#### 混合分块实现示例 (基于 Markdown 结构 + 递归字符)

以下 Python 示例展示了一种混合分块策略。它首先利用 MarkdownHeaderTextSplitter 类，根据 Markdown 的标题（如 #, ##）对文档进行初步分割，并将相应的标题信息作为元数据附加到每个块。随后，如果某个块的大小仍然超出了预设限制（chunk_size），代码会进一步尝试使用更细粒度的分隔符（例如段落分隔符 \n\n、换行符 \n 或句末标点）进行递归分割，以满足大小要求。一个关键特性是，此过程会智能地识别并保留代码块的完整性，不对其进行切分，确保代码示例等内容的完整。该实现借鉴了茴香豆项目的部分思路，并进行了简化与改造。

```


import yaml # 导入 yaml 库
import re # 导入 re 库
import copy # 导入 copy 库
from typing import (Dict, List, Optional, Tuple, TypedDict, Callable, Union) # 添加 Union
from dataclasses import dataclass, field


# --- 数据结构和类型定义 ---

@dataclass
class Chunk:
    &quot;&quot;&quot;用于存储文本片段及相关元数据的类。&quot;&quot;&quot;
    content: str = &#x27;&#x27;
    metadata: dict = field(default_factory=dict)

    def __str__(self) -&gt; str:
        &quot;&quot;&quot;重写 __str__ 方法，使其仅包含 content 和 metadata。&quot;&quot;&quot;
        if self.metadata:
            return f&quot;content=&#x27;{self.content}&#x27; metadata={self.metadata}&quot;
        else:
            return f&quot;content=&#x27;{self.content}&#x27;&quot;

    def __repr__(self) -&gt; str:
        return self.__str__()

    def to_markdown(self, return_all: bool = False) -&gt; str:
        &quot;&quot;&quot;将块转换为 Markdown 格式。

        Args:
            return_all: 如果为 True，则在内容前包含 YAML 格式的元数据。

        Returns:
            Markdown 格式的字符串。
        &quot;&quot;&quot;
        md_string = &quot;&quot;
        if return_all and self.metadata:
            # 使用 yaml.dump 将元数据格式化为 YAML 字符串
            # allow_unicode=True 确保中文字符正确显示
            # sort_keys=False 保持原始顺序
            metadata_yaml = yaml.dump(self.metadata, allow_unicode=True, sort_keys=False)
            md_string += f&quot;---\n{metadata_yaml}---\n\n&quot;
        md_string += self.content
        return md_string

class LineType(TypedDict):
    &quot;&quot;&quot;行类型，使用类型字典定义。&quot;&quot;&quot;
    metadata: Dict[str, str] # 元数据字典
    content: str # 行内容

class HeaderType(TypedDict):
    &quot;&quot;&quot;标题类型，使用类型字典定义。&quot;&quot;&quot;
    level: int # 标题级别
    name: str # 标题名称 (例如, &#x27;Header 1&#x27;)
    data: str # 标题文本内容

class MarkdownHeaderTextSplitter:
    &quot;&quot;&quot;基于指定的标题分割 Markdown 文件，并可选地根据 chunk_size 进一步细分。&quot;&quot;&quot;

    def __init__(
        self,
        headers_to_split_on: List[Tuple[str, str]] = [
            (&quot;#&quot;, &quot;h1&quot;),
            (&quot;##&quot;, &quot;h2&quot;),
            (&quot;###&quot;, &quot;h3&quot;),
            (&quot;####&quot;, &quot;h4&quot;),
            (&quot;#####&quot;, &quot;h5&quot;),
            (&quot;######&quot;, &quot;h6&quot;),
        ],
        strip_headers: bool = False,
        chunk_size: Optional[int] = None, # 添加 chunk_size 参数
        length_function: Callable[[str], int] = len, # 添加 length_function 参数
        separators: Optional[List[str]] = None, # 添加 separators 参数
        is_separator_regex: bool = False, # 添加 is_separator_regex 参数
    ):
        &quot;&quot;&quot;创建一个新的 MarkdownHeaderTextSplitter。

        Args:
            headers_to_split_on: 用于分割的标题级别和名称元组列表。
            strip_headers: 是否从块内容中移除标题行。
            chunk_size: 块的最大非代码内容长度。如果设置，将进一步分割超出的块。
            length_function: 用于计算文本长度的函数。
            separators: 用于分割的分隔符列表，优先级从高到低。
            is_separator_regex: 是否将分隔符视为正则表达式。
        &quot;&quot;&quot;
        if chunk_size is not None and chunk_size &lt;= 0:
            raise ValueError(&quot;chunk_size 必须是正整数或 None。&quot;)

        self.headers_to_split_on = sorted(
            headers_to_split_on, key=lambda split: len(split[0]), reverse=True
        )
        self.strip_headers = strip_headers
        self._chunk_size = chunk_size
        self._length_function = length_function
        # 设置默认分隔符，优先段落，其次换行
        self._separators = separators or [
            &quot;\n\n&quot;,  # 段落
            &quot;\n&quot;,    # 行
            &quot;。|！|？&quot;,  # 中文句末标点
            &quot;\.\s|\!\s|\?\s&quot;, # 英文句末标点加空格
            &quot;；|;\s&quot;,  # 分号
            &quot;，|,\s&quot;   # 逗号
        ]
        self._is_separator_regex = is_separator_regex
        # 预编译正则表达式（如果需要）
        self._compiled_separators = None
        if self._is_separator_regex:
            self._compiled_separators = [re.compile(s) for s in self._separators]

    def _calculate_length_excluding_code(self, text: str) -&gt; int:
        &quot;&quot;&quot;计算文本长度，不包括代码块内容。&quot;&quot;&quot;
        total_length = 0
        last_end = 0
        # 正则表达式查找 ```...``` 或 ~~~...~~~ 代码块
        # 使用非贪婪匹配 .*?
        for match in re.finditer(r&quot;(?:```|~~~).*?\n(?:.*?)(?:```|~~~)&quot;, text, re.DOTALL | re.MULTILINE):
            start, end = match.span()
            # 添加此代码块之前的文本长度
            total_length += self._length_function(text[last_end:start])
            last_end = end
        # 添加最后一个代码块之后的文本长度
        total_length += self._length_function(text[last_end:])
        return total_length

    def _find_best_split_point(self, lines: List[str]) -&gt; int:
        &quot;&quot;&quot;在行列表中查找最佳分割点（索引）。

        优先寻找段落分隔符（连续两个换行符），其次是单个换行符。
        从后向前查找，返回分割点 *之后* 的那一行索引。
        如果找不到合适的分隔点（例如只有一行），返回 -1。
        &quot;&quot;&quot;
        if len(lines) &lt;= 1:
            return -1

        # 优先查找段落分隔符 &quot;\n\n&quot;
        # 这对应于一个空行
        for i in range(len(lines) - 2, 0, -1): # 从倒数第二行向前找到第二行
            if not lines[i].strip() and lines[i+1].strip(): # 当前行是空行，下一行不是
                 # 检查前一行也不是空行，确保是段落间的分隔
                 if i &gt; 0 and lines[i-1].strip():
                     return i + 1 # 在空行之后分割

        # 如果没有找到段落分隔符，则在最后一个换行符处分割
        # （即在倒数第二行之后分割）
        if len(lines) &gt; 1:
             return len(lines) - 1 # 在倒数第二行之后分割（即保留最后一行给下一个块）

        return -1 # 理论上如果行数&gt;1总会找到换行符，但作为保险

    def _split_chunk_by_size(self, chunk: Chunk) -&gt; List[Chunk]:
        &quot;&quot;&quot;将超出 chunk_size 的块分割成更小的块，优先使用分隔符。&quot;&quot;&quot;
        if self._chunk_size is None: # 如果未设置 chunk_size，则不分割
             return [chunk]

        sub_chunks = []
        current_lines = []
        current_non_code_len = 0
        in_code = False
        code_fence = None
        lines = chunk.content.split(&#x27;\n&#x27;)

        for line_idx, line in enumerate(lines):
            stripped_line = line.strip()
            is_entering_code = False
            is_exiting_code = False

            # --- 代码块边界检查 ---
            if not in_code:
                if stripped_line.startswith(&quot;```&quot;) and stripped_line.count(&quot;```&quot;) == 1:
                    is_entering_code = True; code_fence = &quot;```&quot;
                elif stripped_line.startswith(&quot;~~~&quot;) and stripped_line.count(&quot;~~~&quot;) == 1:
                    is_entering_code = True; code_fence = &quot;~~~&quot;
            elif in_code and code_fence is not None and stripped_line.startswith(code_fence):
                is_exiting_code = True
            # --- 代码块边界检查结束 ---

            # --- 计算行长度贡献 ---
            line_len_contribution = 0
            if not in_code and not is_entering_code:
                line_len_contribution = self._length_function(line) + 1 # +1 for newline
            elif is_exiting_code:
                line_len_contribution = self._length_function(line) + 1
            # --- 计算行长度贡献结束 ---

            # --- 检查是否需要分割 ---
            split_needed = (
                line_len_contribution &gt; 0 and
                current_non_code_len + line_len_contribution &gt; self._chunk_size and
                current_lines # 必须已有内容才能分割
            )

            if split_needed:
                # 尝试找到最佳分割点
                split_line_idx = self._find_best_split_point(current_lines)

                if split_line_idx != -1 and split_line_idx &gt; 0: # 确保不是在第一行就分割
                    lines_to_chunk = current_lines[:split_line_idx]
                    remaining_lines = current_lines[split_line_idx:]

                    # 创建并添加上一个子块
                    content = &quot;\n&quot;.join(lines_to_chunk)
                    sub_chunks.append(Chunk(content=content, metadata=chunk.metadata.copy()))

                    # 开始新的子块，包含剩余行和当前行
                    current_lines = remaining_lines + [line]
                    # 重新计算新 current_lines 的非代码长度
                    current_non_code_len = self._calculate_length_excluding_code(&quot;\n&quot;.join(current_lines))

                else: # 找不到好的分割点或 current_lines 太短，执行硬分割
                    content = &quot;\n&quot;.join(current_lines)
                    sub_chunks.append(Chunk(content=content, metadata=chunk.metadata.copy()))
                    current_lines = [line]
                    current_non_code_len = line_len_contribution if not is_entering_code else 0

            else: # 不需要分割，将行添加到当前子块
                current_lines.append(line)
                if line_len_contribution &gt; 0:
                    current_non_code_len += line_len_contribution
            # --- 检查是否需要分割结束 ---


            # --- 更新代码块状态 ---
            if is_entering_code: in_code = True
            elif is_exiting_code: in_code = False; code_fence = None
            # --- 更新代码块状态结束 ---

        # 添加最后一个子块
        if current_lines:
            content = &quot;\n&quot;.join(current_lines)
            # 最后检查一次这个块是否超长（可能只有一个元素但超长）
            final_non_code_len = self._calculate_length_excluding_code(content)
            if final_non_code_len &gt; self._chunk_size and len(sub_chunks) &gt; 0:
                 # 如果最后一个块超长，并且不是唯一的块，可能需要警告或特殊处理
                 # 这里简单地添加它，即使它超长
                 pass # logger.warning(f&quot;Final chunk exceeds chunk_size: {final_non_code_len} &gt; {self._chunk_size}&quot;)
            sub_chunks.append(Chunk(content=content, metadata=chunk.metadata.copy()))

        return sub_chunks if sub_chunks else [chunk]


    def _aggregate_lines_to_chunks(self, lines: List[LineType],
                                   base_meta: dict) -&gt; List[Chunk]:
        &quot;&quot;&quot;将具有共同元数据的行合并成块。&quot;&quot;&quot;
        aggregated_chunks: List[LineType] = []

        for line in lines:
            if aggregated_chunks and aggregated_chunks[-1][&quot;metadata&quot;] == line[&quot;metadata&quot;]:
                # 追加内容，保留换行符
                aggregated_chunks[-1][&quot;content&quot;] += &quot;\n&quot; + line[&quot;content&quot;]
            else:
                # 创建新的聚合块，使用 copy 防止后续修改影响
                aggregated_chunks.append(copy.deepcopy(line))

        final_chunks = []
        for chunk_data in aggregated_chunks:
            final_metadata = base_meta.copy()
            final_metadata.update(chunk_data[&#x27;metadata&#x27;])
            # 在这里移除 strip()，因为后续的 _split_chunk_by_size 需要原始换行符
            final_chunks.append(
                Chunk(content=chunk_data[&quot;content&quot;], # 移除 .strip()
                      metadata=final_metadata)
            )
        return final_chunks


    def split_text(self, text: str, metadata: Optional[dict] = None) -&gt; List[Chunk]:
        &quot;&quot;&quot;基于标题分割 Markdown 文本，并根据 chunk_size 进一步细分。&quot;&quot;&quot;
        base_metadata = metadata or {}
        lines = text.split(&quot;\n&quot;)
        lines_with_metadata: List[LineType] = []
        current_content: List[str] = []
        current_metadata: Dict[str, str] = {}
        header_stack: List[HeaderType] = []

        in_code_block = False
        opening_fence = &quot;&quot;

        for line_num, line in enumerate(lines):
            stripped_line = line.strip()

            # --- 代码块处理逻辑开始 ---
            # 检查是否是代码块开始或结束标记
            is_code_fence = False
            if not in_code_block:
                 if stripped_line.startswith(&quot;```&quot;) and stripped_line.count(&quot;```&quot;) == 1:
                     in_code_block = True
                     opening_fence = &quot;```&quot;
                     is_code_fence = True
                 elif stripped_line.startswith(&quot;~~~&quot;) and stripped_line.count(&quot;~~~&quot;) == 1:
                     in_code_block = True
                     opening_fence = &quot;~~~&quot;
                     is_code_fence = True
            # 检查是否是匹配的结束标记
            elif in_code_block and opening_fence is not None and stripped_line.startswith(opening_fence):
                 in_code_block = False
                 opening_fence = &quot;&quot;
                 is_code_fence = True
            # --- 代码块处理逻辑结束 ---


            # 如果在代码块内（包括边界行），直接添加到当前内容
            if in_code_block or is_code_fence:
                current_content.append(line)
                continue # 继续下一行，不检查标题

            # --- 标题处理逻辑开始 (仅在代码块外执行) ---
            found_header = False
            for sep, name in self.headers_to_split_on:
                if stripped_line.startswith(sep) and (
                    len(stripped_line) == len(sep) or stripped_line[len(sep)] == &quot; &quot;
                ):
                    found_header = True
                    header_level = sep.count(&quot;#&quot;)
                    header_data = stripped_line[len(sep):].strip()

                    # 如果找到新标题，且当前有内容，则将之前的内容聚合
                    if current_content:
                        lines_with_metadata.append({
                            &quot;content&quot;: &quot;\n&quot;.join(current_content),
                            &quot;metadata&quot;: current_metadata.copy(),
                        })
                        current_content = [] # 重置内容

                    # 更新标题栈
                    while header_stack and header_stack[-1][&quot;level&quot;] &gt;= header_level:
                        header_stack.pop()
                    new_header: HeaderType = {&quot;level&quot;: header_level, &quot;name&quot;: name, &quot;data&quot;: header_data}
                    header_stack.append(new_header)
                    current_metadata = {h[&quot;name&quot;]: h[&quot;data&quot;] for h in header_stack}

                    # 如果不剥离标题，则将标题行添加到新内容的开始
                    if not self.strip_headers:
                        current_content.append(line)

                    break # 找到匹配的最高级标题后停止检查
            # --- 标题处理逻辑结束 ---

            # 如果不是标题行且不在代码块内
            if not found_header:
                 # 只有当行不为空或当前已有内容时才添加（避免添加文档开头的空行）
                 # 或者保留空行以维持格式
                 if line.strip() or current_content:
                    current_content.append(line)

        # 处理文档末尾剩余的内容
        if current_content:
            lines_with_metadata.append({
                &quot;content&quot;: &quot;\n&quot;.join(current_content),
                &quot;metadata&quot;: current_metadata.copy(),
            })

        # 第一步：基于标题聚合块
        aggregated_chunks = self._aggregate_lines_to_chunks(lines_with_metadata, base_meta=base_metadata)

        # 第二步：如果设置了 chunk_size，则进一步细分块
        if self._chunk_size is None:
            return aggregated_chunks # 如果没有 chunk_size，直接返回聚合块
        else:
            final_chunks = []
            for chunk in aggregated_chunks:
                # 检查块的非代码内容长度
                non_code_len = self._calculate_length_excluding_code(chunk.content)

                if non_code_len &gt; self._chunk_size:
                    # 如果超出大小，则进行细分
                    split_sub_chunks = self._split_chunk_by_size(chunk)
                    final_chunks.extend(split_sub_chunks)
                else:
                    # 如果未超出大小，直接添加
                    final_chunks.append(chunk)
            return final_chunks


# --- 主要执行 / 测试块 ---
if __name__ == &#x27;__main__&#x27;:
    # 测试代码块
    try:
        # 假设 article.md 文件存在于脚本同目录下
        with open(&quot;article.md&quot;, &quot;r&quot;, encoding=&quot;utf-8&quot;) as f:
            text = f.read()

        # 策略 1: 仅基于标题分割 (不设置 chunk_size)
        # 效果: 生成的块数量较少，每个块对应一个最低级别的标题段落。
        #       块的大小可能非常不均匀，有些块可能非常大。
        #       代码块始终包含在它们所属的标题段落内。
        print(&quot;--- Splitting without chunk_size limit (Header-based only) ---&quot;)
        splitter_no_limit = MarkdownHeaderTextSplitter()
        chunks_no_limit = splitter_no_limit.split_text(text)
        print(f&quot;Total chunks: {len(chunks_no_limit)}&quot;)
        # 取消注释以查看详细输出
        # for chunk in chunks_no_limit:
        #     print(chunk.to_markdown(return_all=True))
        #     print(&quot;=&quot; * 40)

        print(&quot;\n&quot; + &quot;===&quot; * 20 + &quot;\n&quot;)

        # 策略 2: 基于标题分割，然后根据 chunk_size 和分隔符进一步细分
        # 效果: 首先按标题分割，然后对于超出 chunk_size 的块，
        #       会尝试在更自然的边界（如段落 `\n\n` 或句子/行 `\n`，以及其他标点）进行分割。
        #       目标是使块的非代码内容长度接近但不超过 chunk_size。
        #       代码块保持完整，并且其内容不计入 chunk_size 计算。
        #       这通常能产生大小更均匀、更适合后续处理（如 RAG）的块。
        print(&quot;--- Splitting with chunk_size = 150 (Header-based + Size/Separator-based refinement) ---&quot;)
        # 使用默认分隔符: [&quot;\n\n&quot;, &quot;\n&quot;, &quot;。&quot;, &quot;！&quot;, &quot;？&quot;, &quot;. &quot;, &quot;! &quot;, &quot;? &quot;, &quot;；&quot;, &quot;; &quot;, &quot;，&quot;, &quot;, &quot;]
        # 并启用 is_separator_regex=True 以处理中文标点等
        splitter_with_limit = MarkdownHeaderTextSplitter(chunk_size=150, is_separator_regex=True) # 注意添加 is_separator_regex=True 以使用默认中文分隔符
        chunks_with_limit = splitter_with_limit.split_text(text)
        print(f&quot;Total chunks: {len(chunks_with_limit)}&quot;)
        for i, chunk in enumerate(chunks_with_limit):
            print(f&quot;--- Chunk {i+1} ---&quot;)
            non_code_len = splitter_with_limit._calculate_length_excluding_code(chunk.content)
            print(f&quot;Content Length (Total): {len(chunk.content)}&quot;)
            print(f&quot;Content Length (Non-Code): {non_code_len}&quot;) # 检查非代码长度是否接近 chunk_size
            print(f&quot;Metadata: {chunk.metadata}&quot;)
            # print(&quot;\n--- Markdown (Content Only) ---&quot;)
            # print(chunk.to_markdown())
            print(&quot;\n--- Markdown (With Metadata) ---&quot;)
            print(chunk.to_markdown(return_all=True))
            print(&quot;====&quot; * 20) # 缩短分隔符以便查看更多块

    except FileNotFoundError:
        print(&quot;Error: article.md not found. Please create the file for testing.&quot;)
```

这个示例展示了如何优先利用 Markdown 的标题结构来划分文档，同时保留了标题信息作为重要的上下文元数据。这种方式得到的 Chunk 通常在逻辑上更连贯。当然了，上面实现的分块方案在不动用其他模型的基础上做的已经算是比较优秀的了，后续我们也会经常用到这个分块方案，如果不用模型做的话。

---

## 🚀三、高级分块策略

当**基础策略**无法满**足更复杂的需求时，**或者当你想追求极致的检索效果时，可以探索以下更高级的分块方法。这些方法通常更侧重于语义理解或利用更复杂的模型/流程。

### 🧠 3.1 语义分块 (Semantic Chunking)

+   **核心思想**：这种方法不看字数、不看标点，而是看“意思”。它会计算相邻句子或小段文字的 Embedding 向量，看看它们在语义上有多接近。当发现前后两部分的“话题”跳跃比较大（语义相似度低于某个设定的“阈值”）时，就在这个“语义断裂点”进行切割。

+   **优点** ：切分点更“懂”语义，总能在话题自然转变的地方下手。这样能保证每个 Chunk 内部意思高度相关、不跑题，理论上切出来的块更符合人的阅读理解习惯。

+   **缺点** ：要算 Embedding，计算开销比前面那些简单方法大得多。效果好坏非常依赖 Embedding 模型本身的能力，而且那个“语义距离阈值”得靠实验慢慢调，有点麻烦。处理速度也比较慢。

+   **适用场景**：对分块质量要求很高，并且计算资源比较充裕的场景。特别适合处理那些没什么结构化标记（比如纯文本、对话记录），但意思很丰富的长文。

> **💡 实现思路举例：**

> 一种可能的做法（去年我记得Langchain是这样）是，不比较单个句子，而是把连续的几个句子（比如 3 句）看成一个“语义单元”，计算这个单元的 Embedding，然后比较相邻“单元”之间的语义距离。

> 当然，这种方法具体效果如何，和其他语义分割的思路比起来怎么样，还需要更多的实践和对比验证,因为我没有具体测试过，后续会出一篇文章关于它的，敬请期待！

### 📊 3.2 分层分块 (Hierarchical Chunking)

+   **核心思想****：类**似于混合策略，但更系统化地创建多个层级的 Chunks。例如，可以先将文档按章节分割成大块（L1 Chunks），再将每个章节按段落分割成中块（L2 Chunks），最后可能按句子分割成小块（L3 Chunks）。这些不同层级的 Chunks 可以被索引，并用于不同的检索策略（见下文 Small-to-Big）。

+   **优点** ：提供了不同粒度的上下文信息，增加了检索的灵活性。

+   **缺点** ：增加了索引的复杂度和存储空间。需要设计好多层级之间的关系和使用方式。

+   **适用场景**：需要处理具有清晰层级结构的复杂文档（如书籍、长篇报告），并且希望在检索时能灵活选择上下文粒度。

### 🔗 3.3 Small-to-Big 检索 / 父文档检索器 (Parent Document Retriever)

+   **核心思想**：这严格来说是一种**检索策略**，但它依赖于特定的分块方式（通常是分层或存在父子关系的块）。基本流程是：

1.  将文档分割成小块 (Small Chunks)，这些小块适合进行精确的向量相似度检索。

1.  同时，保留或链接到这些小块所属的更大的父块 (Parent Chunks)（例如，小块是句子，父块是包含该句子的段落）。

1.  检索时，先用查询向量匹配**小块**。

1.  一旦找到相关的小块，**不直接将小块**传递给 LLM，而是**返回其对应的父块**。

+   **优点** ：结合了小块的**检索精度**和大块的**上下文完整性**。既能精准定位相关信息点，又能为 LLM 提供更丰富的背景。

+   **缺点** ：需要维护小块和父块之间的映射关系，增加了索引和检索逻辑的复杂度。

+   **适用场景**：既需要高精度检索，又需要充分上下文来生成高质量答案的 RAG 应用。

### 📜 3.4 命题分块 (Proposition Chunking)

+   **核心思想**：尝试将文本分解为更小的、原子性的事实陈述或主张（Propositions）。这通常需要借助 LLM 或专门的 NLP 模型来识别和提取文本中的核心命题。例如，句子“苹果公司在 2023 年发布了 Vision Pro 头显，定价 3499 美元”可能会被分解为：“苹果公司发布了 Vision Pro 头显”、“Vision Pro 头显发布于 2023 年”、“Vision Pro 头显定价 3499 美元”等命题。然后对这些命题进行索引和检索。

+   **优点** ：产生了非常细粒度、高度聚焦的知识单元，非常适合进行精确的事实检索和问答。

+   **缺点** ：

+   严重依赖 LLM 或 NLP 模型的抽取能力，可能产生错误或不完整的命题。

+   计算成本高昂，处理速度慢。

+   可能丢失原始文本的语气、复杂关系和细微差别。

+   如何将检索到的离散命题有效地组合起来提供给生成模型是一个挑战。

+   **适用场景**：知识库构建、事实性问答系统，对信息的原子性和精确性要求极高的场景。

### 🤖 3.5 Agentic / LLM-based Chunking

+   **核心思想**：更进一步，让一个**智能体 (Agent)** 或直接使用 **LLM** 来**决策**如何进行分块。可以给 LLM 设计特定的 Prompt，让它根据内容理解来判断最佳的分割点，或者让 Agent 动态地选择和组合不同的分块策略。

+   **优点** ：潜力巨大，理论上可以实现最智能、最符合语义和上下文的分块。

+   **缺点** ：

+   实现非常复杂，需要精巧的 Prompt Engineering 或 Agent 逻辑设计。

+   成本高（LLM 调用开销），速度慢。

+   结果的可控性和稳定性可能不如确定性算法。

+   仍处于探索阶段，成熟度和可靠性有待验证。

+   **适用场景**：研究探索性质的项目，或者对分块质量有极致追求且不计成本的特定应用。

## 📒 四、块优化策略

✨ 上下文富化 (Context Enrichment) - (补充策略)

+   **核心思想****：这**不是一种独立的分割方法**，而是在分块之后，为**每个 Chunk 添加额外的上下文信息。例如，可以在每个 Chunk 前后添加其相邻的句子或摘要信息，或者添加该 Chunk 所属章节/段落的标题作为元数据（如 MarkdownHeaderTextSplitter 所做）。

+   **优点** ：在不显著增大 Chunk 大小的情况下，为 LLM 提供更多线索，帮助其理解 Chunk 在原文中的位置和背景。

+   **缺点** ：需要额外的处理步骤来提取和添加这些富化信息。

+   **适用场景**：可以与其他任何分块策略结合使用，作为一种优化手段，特别是在 Chunk 较小，担心上下文不足时。

**小结**：高级策略提供了更精细化、更智能化的分块选择，但往往伴随着更高的复杂度和计算成本。选择哪种策略，需要在具体应用场景下，仔细权衡效果、成本和实现复杂度。

> 注意：过多的摘要也可能会缩短同一文章内的块之间语义相似度差值。

---

## 📒 总结与展望

### 🚀 技术全景图

+   **文本处理基础**：涉及字符串操作、正则表达式、自然语言处理（句子分割）。

+   **Embedding 模型**：虽然本文未深入，但分块后的 Chunks 是输入给 Embedding 模型进行向量化的基本单位。高级策略（如语义分块）甚至直接依赖 Embedding。

+   **向量数据库与检索**：分块的质量直接影响索引构建和最终的检索效果（精度、召回率、上下文丰富度）。

+   **大语言模型 (LLM)**：分块最终是为了服务 LLM，确保其在上下文窗口内获得高质量、相关且连贯的信息用于生成。

+   **RAG 架构**：文本分块是 RAG 管道中至关重要的预处理步骤，是连接原始数据和检索模块的桥梁。

### 📓 学习汇总

+   **文本分块的必要性**：理解了为何要分块（克服窗口限制、提精度效率、保上下文、优成本）及其对 RAG 整体性能的关键影响。

+   **核心挑战**：认识到分块需要在“检索精度”和“上下文完整性”之间进行权衡。

+   **基础分块策略**：熟悉了固定大小、句子分割、递归字符、文档结构感知和混合策略的原理、优缺点及适用场景，并看到了 Markdown 混合策略的代码实现。

+   **高级分块策略概览**：了解了语义分块、分层分块、Small-to-Big、命题分块、Agentic 分块等更前沿方法的概念和思路。

+   **策略选择考量**：理解了没有万能策略，需要根据数据特性和应用需求来选择和优化。

### 🔥 动手挑战

1.  **运行并实验混合分块代码**：在你自己的 Markdown 文件上运行提供的 MarkdownHeaderTextSplitter 代码。尝试修改 headers_to_split_on 参数（例如，只按 H1 分割），观察结果变化。

1.  **实现一个简单的句子分割器**：尝试使用 Python 的 re 模块或 nltk/spacy 库，实现一个基本的句子分割函数，并将长文本分割成基于句子的 Chunks。

1.  **比较不同策略的效果（思考）**：对于一个你熟悉的文档（例如，一篇技术博客或报告），设想一下分别使用固定大小分块（小/大 overlap）、句子分块和（如果可能）递归字符分块，会对检索特定信息（例如，一个定义、一个步骤列表）产生什么不同的影响？哪种可能更好？为什么？

### ♻️ 互动问题

1.  在你的 RAG 项目或设想中，你处理的主要是什么类型的文档？根据文档特性，你认为哪种（或哪几种组合的）分块策略可能最适合？为什么？

1.  除了本文提到的策略，你还知道或想到哪些其他的文本分块方法或技巧？

1.  你认为评估文本分块策略好坏的最佳指标或方法是什么？（例如，除了 Chunk 长度分布，还有什么？）

希望这篇文章能帮助你更好地理解和掌握 RAG 中的文本分块技术，为你的 AI 应用打下更坚实的基础！如果你有任何想法或问题，欢迎在评论区交流讨论！

> 欢迎在留言区分享你的想法，每条留言我都会认真阅读！

> 你的反馈是我创作的最大动力 ❤️

> **💗 立即行动**

1.  点赞、收藏、关注，分享给朋友。

1.  为账号加星标，获取更多内容。

1.  以本文为灵感，整理一份属于自己的笔记。

> **🚗 行动召唤**

> "与其等着 AI 改变世界，不如自己参与变革！在这里，让 AI 成为你弯道超车的秘密武器。"

## 📚 往期精选

+   [向量相似度揭秘：长度、相关性与关](https://mp.weixin.qq.com/s/tDLLl6FV-P4OkEtNo7awrA?scene=1)键词如何“迷惑”Embedding模型？

+   [AI 为何偏爱 Bfloat16？一文搞懂 Float3](https://mp.weixin.qq.com/s/INKz346v_tDE5i9yAizEAQ)2、Float16 与 Bfloat16

+   RAG 的检索优化：MMR 平衡相关性与多样性

+   你的RAG检索太“笨”？用K-Means聚类来“调教”

+   [RAG之延迟交互与残差压缩：从ColB](https://mp.weixin.qq.com/s/2-9FqIFxgWM10X9lnora4A)ERT到ColBERTv2的演进及其应用