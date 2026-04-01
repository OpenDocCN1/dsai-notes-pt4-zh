# 使用 LlamaIndex 构建代理式RAG(4)-创建多文档Agent

> 来源：[https://agijuejin.feishu.cn/docx/KTXId0yJpojqaxxSQXXcsalIn9b](https://agijuejin.feishu.cn/docx/KTXId0yJpojqaxxSQXXcsalIn9b)

> 作者：lucas大叔

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/698932843)hihu.com/p/698932...

前几节我们已经构建了可以对单文档进行推理的agent，能够在保持记忆的同时回答复杂问题。本节将学习如何扩展该agent以处理多个文档和不断增加的复杂度。

我们先使用3篇文档构建agent，然后扩展到11篇文档。

## 配置

导入Open API KEY

```
from helper import get_openai_api_key

OPENAI_API_KEY = get_openai_api_key()
```

导入nest_asyncio模块

```
import nest_asyncio

nest_asyncio.apply()
```

## 基于3篇文档创建Agent

首先基于三篇论文创建 Function Calling Agent。

```
urls = [
    &quot;https://openreview.net/pdf?id=VtmBAGCN7o&quot;,
    &quot;https://openreview.net/pdf?id=6PmJoRfdaK&quot;,
    &quot;https://openreview.net/pdf?id=hSyW5go0v8&quot;,
]

papers = [
    &quot;metagpt.pdf&quot;,
    &quot;longlora.pdf&quot;,
    &quot;selfrag.pdf&quot;,
]
```

为每篇文档创建Vector tool和Summary tool。

```
from utils import get_doc_tools
from pathlib import Path

paper_to_tools_dict = {}
for paper in papers:
    print(f&quot;Getting tools for paper: {paper}&quot;)
    vector_tool, summary_tool = get_doc_tools(paper, Path(paper).stem)
    paper_to_tools_dict[paper] = [vector_tool, summary_tool]
```

> Getting tools for paper: metagpt.pdf

> Getting tools for paper: longlora.pdf

> Getting tools for paper: selfrag.pdf

接下来将所有的工具放在展开的list中

```
initial_tools = [t for paper in papers for t in paper_to_tools_dict[paper]]
```

选择gpt-3.5-turbo作为LLM

```
from llama_index.llms.openai import OpenAI

llm = OpenAI(model=&quot;gpt-3.5-turbo&quot;)
```

构建AgentWorker，包括6个工具和LLM。

```
from llama_index.core.agent import FunctionCallingAgentWorker
from llama_index.core.agent import AgentRunner

agent_worker = FunctionCallingAgentWorker.from_tools(
    initial_tools, 
    llm=llm, 
    verbose=True
)
agent = AgentRunner(agent_worker)
```

接下来就可以进行单文档或跨文档问答，先问一个LongLoRA相关的问题。

```
response = agent.query(
    &quot;Tell me about the evaluation dataset used in LongLoRA, &quot;
    &quot;and then tell me about the evaluation results&quot;
)
```

> Added user message to memory: Tell me about the evaluation dataset used in LongLoRA, and then tell me about the evaluation results

> === Calling Function ===

> Calling function: vector_tool_longlora with args: {"query": "evaluation dataset"}

> === Function Output ===

> PG19 test split

> === Calling Function ===

> Calling function: vector_tool_longlora with args: {"query": "evaluation results"}

> === Function Output ===

> The evaluation results show that the models achieve better perplexity with longer context sizes. Increasing the context window size leads to improved perplexity values. Additionally, the models are fine-tuned on different context lengths, such as 8192, 32768, 100k, 65536, and 32768, with promising results on extremely large settings. However, there is some perplexity degradation observed on small context sizes for the extended models, which is a known limitation of Position Interpolation.

> === LLM Response ===

> The evaluation dataset used in LongLoRA is the PG19 test split.

> 

> Regarding the evaluation results, the models in LongLoRA achieve better perplexity with longer context sizes. Increasing the context window size leads to improved perplexity values. The models are fine-tuned on different context lengths, such as 8192, 32768, 100k, 65536, and 32768, with promising results on extremely large settings. However, there is some perplexity degradation observed on small context sizes for the extended models, which is a known limitation of Position Interpolation.

再问一个跨文档的问题，要跨两篇文档做摘要。

```
response = agent.query(&quot;Give me a summary of both Self-RAG and LongLoRA&quot;)
print(str(response))
```

> Added user message to memory: Give me a summary of both Self-RAG and LongLoRA

> === Calling Function ===

> Calling function: summary_tool_selfrag with args: {"input": "Self-RAG"}

> === Function Output ===

> Self-RAG is a framework that enhances the quality and factuality of a large language model through retrieval and self-reflection. It involves training the language model to retrieve passages on-demand, generate text informed by these retrieved passages, and critique its own output using special tokens called reflection tokens. This approach allows the language model to tailor its behavior to diverse task requirements, leading to significant performance improvements compared to other models on various tasks such as open-domain question answering, reasoning, and fact verification.

> === Calling Function ===

> Calling function: summary_tool_longlora with args: {"input": "LongLoRA"}

> === Function Output ===

> LongLoRA is an efficient method for extending the context length of Large Language Models (LLMs) while minimizing computational resources compared to full fine-tuning. It combines shifted sparse attention (S2-Attn) with LoRA to effectively and efficiently fine-tune models to longer context lengths. The method aims to maintain performance while reducing memory costs and training time, making it accessible for researchers with limited computational resources. Additionally, an improved version of LongLoRA, known as LoRA+, allows for trainable normalization and embedding layers during the adaptation process, addressing performance gaps observed in adapting LLMs to longer context lengths.

> === LLM Response ===

> Self-RAG is a framework that enhances the quality and factuality of a large language model through retrieval and self-reflection. It involves training the language model to retrieve passages on-demand, generate text informed by these retrieved passages, and critique its own output using special tokens called reflection tokens. This approach allows the language model to tailor its behavior to diverse task requirements, leading to significant performance improvements compared to other models on various tasks such as open-domain question answering, reasoning, and fact verification.

> 

> LongLoRA is an efficient method for extending the context length of Large Language Models (LLMs) while minimizing computational resources compared to full fine-tuning. It combines shifted sparse attention (S2-Attn) with LoRA to effectively and efficiently fine-tune models to longer context lengths. The method aims to maintain performance while reducing memory costs and training time, making it accessible for researchers with limited computational resources. Additionally, an improved version of LongLoRA, known as LoRA+, allows for trainable normalization and embedding layers during the adaptation process, addressing performance gaps observed in adapting LLMs to longer context lengths.

> assistant: Self-RAG is a framework that enhances the quality and factuality of a large language model through retrieval and self-reflection. It involves training the language model to retrieve passages on-demand, generate text informed by these retrieved passages, and critique its own output using special tokens called reflection tokens. This approach allows the language model to tailor its behavior to diverse task requirements, leading to significant performance improvements compared to other models on various tasks such as open-domain question answering, reasoning, and fact verification.

> 

> LongLoRA is an efficient method for extending the context length of Large Language Models (LLMs) while minimizing computational resources compared to full fine-tuning. It combines shifted sparse attention (S2-Attn) with LoRA to effectively and efficiently fine-tune models to longer context lengths. The method aims to maintain performance while reducing memory costs and training time, making it accessible for researchers with limited computational resources. Additionally, an improved version of LongLoRA, known as LoRA+, allows for trainable normalization and embedding layers during the adaptation process, addressing performance gaps observed in adapting LLMs to longer context lengths.

## 基于11篇文档创建Agent

### 下载11篇ICLR论文

```
urls = [
    &quot;https://openreview.net/pdf?id=VtmBAGCN7o&quot;,
    &quot;https://openreview.net/pdf?id=6PmJoRfdaK&quot;,
    &quot;https://openreview.net/pdf?id=LzPWWPAdY4&quot;,
    &quot;https://openreview.net/pdf?id=VTF8yNQM66&quot;,
    &quot;https://openreview.net/pdf?id=hSyW5go0v8&quot;,
    &quot;https://openreview.net/pdf?id=9WD9KwssyT&quot;,
    &quot;https://openreview.net/pdf?id=yV6fD7LYkF&quot;,
    &quot;https://openreview.net/pdf?id=hnrB5YHoYu&quot;,
    &quot;https://openreview.net/pdf?id=WbWtOYIzIK&quot;,
    &quot;https://openreview.net/pdf?id=c5pwL0Soay&quot;,
    &quot;https://openreview.net/pdf?id=TpD2aG1h0D&quot;
]

papers = [
    &quot;metagpt.pdf&quot;,
    &quot;longlora.pdf&quot;,
    &quot;loftq.pdf&quot;,
    &quot;swebench.pdf&quot;,
    &quot;selfrag.pdf&quot;,
    &quot;zipformer.pdf&quot;,
    &quot;values.pdf&quot;,
    &quot;finetune_fair_diffusion.pdf&quot;,
    &quot;knowledge_card.pdf&quot;,
    &quot;metra.pdf&quot;,
    &quot;vr_mcl.pdf&quot;
]
```

与前面类似，为每篇论文创建Vector tool和Summary tool。

```
from utils import get_doc_tools
from pathlib import Path

paper_to_tools_dict = {}
for paper in papers:
    print(f&quot;Getting tools for paper: {paper}&quot;)
    vector_tool, summary_tool = get_doc_tools(paper, Path(paper).stem)
    paper_to_tools_dict[paper] = [vector_tool, summary_tool]
```

> Getting tools for paper: metagpt.pdf

> Getting tools for paper: longlora.pdf

> Getting tools for paper: loftq.pdf

> Getting tools for paper: swebench.pdf

> Getting tools for paper: selfrag.pdf

> Getting tools for paper: zipformer.pdf

> Getting tools for paper: values.pdf

> Getting tools for paper: finetune_fair_diffusion.pdf

> Getting tools for paper: knowledge_card.pdf

> Getting tools for paper: metra.pdf

> Getting tools for paper: vr_mcl.pdf

### 用工具检索扩展Agent

将所有工具放入一个list

```
all_tools = [t for paper in papers for t in paper_to_tools_dict[paper]]
```

索引11篇论文，对应了20多个工具。假设索引100篇、1000篇甚至更多论文，将会产生大量的工具。尽管LLM上下文窗口变得越来越长，但将太多的工具塞入LLM prompt会导致以下问题：

+   工具可能无法全部放入prompt，特别是文档数量很多时，为每篇文档都建模一个或一组工具。

+   成本和延迟会增加。prompt中的token数增加，大纲可能会混乱。当需要选择的数量过大时，LLM可能无法选择正确的工具。

这里给出的解决方案是，当用户提出查询时，在工具上执行检索增强，而不是在文本上执行。我们首先检索一组相关的工具，然后把检索到的相关工具提供给Agent的prompt，而不是一股脑把所有的工具都给到prompt。

这个过程类似于RAG中的检索过程，可以用最简单的方式做top K向量检索。当然也可以借鉴RAG中其他的高级技巧，过滤出相关结果的集合。我们创建的agent允许插入检索工具，实现上面所述的思路。

```
# define an &quot;object&quot; index and retriever over these tools
from llama_index.core import VectorStoreIndex
from llama_index.core.objects import ObjectIndex

obj_index = ObjectIndex.from_objects(
    all_tools,
    index_cls=VectorStoreIndex,
)
```

通过obj_retriever检索obj_index

```
obj_retriever = obj_index.as_retriever(similarity_top_k=3)

tools = obj_retriever.retrieve(
    &quot;Tell me about the eval dataset used in MetaGPT and SWE-Bench&quot;
)
```

查看返回的工具信息

```
tools[0].metadata
```

> ToolMetadata(description='Useful for summarization questions related to metagpt', name='summary_tool_metagpt', fn_schema=<class 'llama_index.core.tools.types.DefaultToolFnSchema'>, return_direct=False)

创建Function Calling Agent，与前面章节不太一样地方是可以在agent中添加system prompt。这是可选功能，如果你想要提示agent以某种方式输出结果，或者在工具选择推理时要求agent考虑某些因素，那这就是一个例子。

```
from llama_index.core.agent import FunctionCallingAgentWorker
from llama_index.core.agent import AgentRunner

agent_worker = FunctionCallingAgentWorker.from_tools(
    tool_retriever=obj_retriever,
    llm=llm, 
    system_prompt=&quot;&quot;&quot; \
You are an agent designed to answer queries over a set of given papers.
Please always use the tools provided to answer a question. Do not rely on prior knowledge.\
&quot;&quot;&quot;,
    verbose=True
)
agent = AgentRunner(agent_worker)
```

下面问比较类的查询

```
response = agent.query(
    &quot;Tell me about the evaluation dataset used &quot;
    &quot;in MetaGPT and compare it against SWE-Bench&quot;
)
print(str(response))
```

> Added user message to memory: Tell me about the evaluation dataset used in MetaGPT and compare it against SWE-Bench

> === Calling Function ===

> Calling function: summary_tool_metagpt with args: {"input": "evaluation dataset used in MetaGPT"}

> === Function Output ===

> The evaluation dataset used in MetaGPT includes HumanEval, MBPP, and SoftwareDev. HumanEval consists of 164 handwritten programming tasks, MBPP comprises 427 Python tasks, and SoftwareDev is a collection of 70 representative software development tasks covering various scopes like mini-games, image processing algorithms, and data visualization.

> === Calling Function ===

> Calling function: summary_tool_swebench with args: {"input": "comparison of evaluation dataset in MetaGPT and SWE-Bench"}

> === Function Output ===

> The evaluation datasets in MetaGPT and SWE-Bench differ in terms of the tasks they focus on and the scope of changes required. MetaGPT's dataset involves generating code snippets to solve self-contained problems across various domains, showcasing versatility in model capabilities. In contrast, SWE-Bench's dataset requires resolving real-world software engineering issues by making changes across multiple functions, classes, and files in a codebase, emphasizing challenges like patch generation and navigating codebase directories. This distinction highlights that MetaGPT's evaluation dataset covers a wide range of tasks, while SWE-Bench's dataset concentrates specifically on code generation benchmarks within software engineering contexts.

> === LLM Response ===

> The evaluation dataset used in MetaGPT includes HumanEval, MBPP, and SoftwareDev. HumanEval consists of 164 handwritten programming tasks, MBPP comprises 427 Python tasks, and SoftwareDev is a collection of 70 representative software development tasks covering various scopes like mini-games, image processing algorithms, and data visualization.

> 

> The evaluation datasets in MetaGPT and SWE-Bench differ in terms of the tasks they focus on and the scope of changes required. MetaGPT's dataset involves generating code snippets to solve self-contained problems across various domains, showcasing versatility in model capabilities. In contrast, SWE-Bench's dataset requires resolving real-world software engineering issues by making changes across multiple functions, classes, and files in a codebase, emphasizing challenges like patch generation and navigating codebase directories. This distinction highlights that MetaGPT's evaluation dataset covers a wide range of tasks, while SWE-Bench's dataset concentrates specifically on code generation benchmarks within software engineering contexts.

> assistant: The evaluation dataset used in MetaGPT includes HumanEval, MBPP, and SoftwareDev. HumanEval consists of 164 handwritten programming tasks, MBPP comprises 427 Python tasks, and SoftwareDev is a collection of 70 representative software development tasks covering various scopes like mini-games, image processing algorithms, and data visualization.

> 

> The evaluation datasets in MetaGPT and SWE-Bench differ in terms of the tasks they focus on and the scope of changes required. MetaGPT's dataset involves generating code snippets to solve self-contained problems across various domains, showcasing versatility in model capabilities. In contrast, SWE-Bench's dataset requires resolving real-world software engineering issues by making changes across multiple functions, classes, and files in a codebase, emphasizing challenges like patch generation and navigating codebase directories. This distinction highlights that MetaGPT's evaluation dataset covers a wide range of tasks, while SWE-Bench's dataset concentrates specifically on code generation benchmarks within software engineering contexts.

最后查询LongLoRA和LoftQ的对比和比较问题

```
response = agent.query(
    &quot;Compare and contrast the LoRA papers (LongLoRA, LoftQ). &quot;
    &quot;Analyze the approach in each paper first. &quot;
)
```

> Added user message to memory: Compare and contrast the LoRA papers (LongLoRA, LoftQ). Analyze the approach in each paper first.

> === Calling Function ===

> Calling function: summary_tool_longlora with args: {"input": "Approach in LongLoRA"}

> === Function Output ===

> The approach in LongLoRA involves efficiently extending the context length of large language models through the use of shifted sparse attention (S2-Attn) as a substitute for standard self-attention during fine-tuning. This method splits the context length into groups, conducts attention within each group individually, and ensures information flow between neighboring groups by shifting tokens. LongLoRA combines S2-Attn with LoRA for context extension, focusing on trainable embedding and normalization layers to effectively extend the context window of pre-trained large language models while reducing GPU memory cost and training time compared to full fine-tuning.

> === Calling Function ===

> Calling function: summary_tool_loftq with args: {"input": "Approach in LoftQ"}

> === Function Output ===

> The approach in LoftQ involves a novel quantization framework that integrates quantization and Low-Rank Adaptation (LoRA) fine-tuning for pre-trained models. It simultaneously quantizes a Large Language Model (LLM) and finds a suitable low-rank initialization for LoRA fine-tuning. This joint process aims to reduce the performance gap observed between full fine-tuning and quantization plus LoRA fine-tuning approaches. By aligning the quantized model with the original full-precision model, LoftQ significantly enhances generalization in downstream tasks. The method involves optimizing the initial values of the quantized backbone and low-rank adapters through an alternating optimization process, which includes quantization and singular value decomposition steps.

> === LLM Response ===

> The approach in LongLoRA focuses on efficiently extending the context length of large language models by using shifted sparse attention (S2-Attn) during fine-tuning. This method splits the context length into groups, conducts attention within each group individually, and ensures information flow between neighboring groups by shifting tokens. LongLoRA combines S2-Attn with LoRA for context extension, emphasizing trainable embedding and normalization layers to extend the context window of pre-trained large language models effectively while reducing GPU memory cost and training time compared to full fine-tuning.

> 

> On the other hand, LoftQ introduces a novel quantization framework that integrates quantization and Low-Rank Adaptation (LoRA) fine-tuning for pre-trained models. It simultaneously quantizes a Large Language Model (LLM) and finds a suitable low-rank initialization for LoRA fine-tuning. This joint process aims to reduce the performance gap observed between full fine-tuning and quantization plus LoRA fine-tuning approaches. By aligning the quantized model with the original full-precision model, LoftQ significantly enhances generalization in downstream tasks. The method involves optimizing the initial values of the quantized backbone and low-rank adapters through an alternating optimization process, which includes quantization and singular value decomposition steps.