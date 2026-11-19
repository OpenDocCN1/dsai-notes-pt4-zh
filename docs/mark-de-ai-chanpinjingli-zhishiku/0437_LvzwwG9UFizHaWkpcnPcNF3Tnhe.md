
# 如何从文档创建 RAG 评估数据集

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/XNSbdbpPVoGe8KxeHoBcFl7Rnye](https://qqs7y1hozd1.feishu.cn/docx/XNSbdbpPVoGe8KxeHoBcFl7Rnye)

![](img/bd501f4ad50836c20bbeed73dda15d21.png)

我们在 Hugging Face Hub 上自动生成的 RAG 评估数据集（来自欧盟的PDF 输入文件，根据CC BY 4.0许可）。

在本文中，我将向您展示如何创建自己的 RAG 数据集，该数据集包含任何语言的文档的上下文、问题和答案。

## 检索增强生成 (RAG) [1]

是一种允许 LLM 访问外部知识库的技术。

通过上传 PDF 文件并将其存储在矢量数据库中，我们可以通过矢量相似性搜索检索这些知识，然后将检索到的文本作为附加上下文插入到 LLM 提示中。

这为LLM提供了新的知识，并减少了LLM编造事实（幻觉）的可能性。

RAG 流程概述。对于文档存储：输入文档 -> 文本块 -> 编码器模型 -> 向量数据库。对于 LLM 提示：用户问题 -> 编码器模型 -> 向量数据库 -> 前 `k` 个相关块 -> 生成器 LLM 模型。然后，LLM 使用检索到的上下文回答问题。

基本的 RAG 管道。

![](img/76f8537661ecdef119da03771fdeccd7.png)

然而，我们需要在 RAG 流程中设置许多参数，研究人员也一直在提出新的改进建议。我们如何知道应该选择哪些参数以及哪些方法可以真正提高我们特定用例的性能？

这就是为什么我们需要一个**验证/开发/测试数据集**来评估我们的 RAG 管道。数据集应该来自我们感兴趣的领域和我们想要使用的语言。

## 使用 VLLM 部署本地 LLM

首先，我们要建立并运行本地法学硕士学位。

我使用 `VLLM` 设置了一个与 `OpenAI` 兼容的 LLM 服务器，其中包含量化的 `Llama-3.2–3B-Instruct`。确保你使用的 LLM 已经针对你想要使用的语言进行过训练。

使用 `Docker` 和 `VLLM` 部署本地 LLM 非常简单：

### 使用 Docker

```
docker run --runtime nvidia --gpus all \
    -v ~/.cache/huggingface:/root/.cache/huggingface \
    -- env "HUGGING_FACE_HUB_TOKEN=<secret>" \
    -p 8000:8000 \
    --ipc=host \
    vllm/vllm-openai:latest \
    --model AMead10/Llama-3.2-3B-Instruct-AWQ \
    --quantization awq \
    --max-model-len 2048
```

### 使用 Docker Compose

```
services:
  vllm:
    image: vllm/vllm-openai:latest
    command: ["--model", "AMead10/Llama-3.2-3B-Instruct-AWQ", "--max-model-len", "2048", "--quantization", "awq"]
    ports:
      - 8000:8000
    volumes:
      - ~/.cache/huggingface:/root/.cache/huggingface
    environment:
      - "HUGGING_FACE_HUB_TOKEN=<secret>"
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: 1
              capabilities: [gpu]
```

### 使用本地 VLLM 服务器

现在我们可以将本地的 LLM 与官方 `OpenAI` Python SDK 一起使用。

```
%pip install openai

from openai import OpenAI
```

```
client = OpenAI(
    base_url="http://localhost:8000/v1",
    api_key="None",
)

chat_completion = client.chat.completions.create(
    messages=[
        {
            "role": "user",
            "content": "Say this is a test",
        }
    ],
    model="AMead10/Llama-3.2-3B-Instruct-AWQ",
)
```

让我们进行快速的健全性检查，看看一切是否按预期进行：

```
print(chat_completion.choices[0].message.content)
```

> “这似乎是一个测试。有什么具体的事情你想测试或讨论吗？我可以帮你。”

## 创建 RAG 评估数据集

![](img/0fa92aac69056d3af41a9718f4e3c255.png)

> 我们加载文档并将上下文传递给生成器 LLM，生成器会生成问题和答案。问题、答案和上下文是传递给 LLM 评委的一个数据样本。然后，LLM 评委会生成质量分数，该分数可用于过滤掉不良样本。

> 自动生成 RAG 数据集的基本工作流程从从文档（例如 PDF 文件）读取我们的知识库开始。

> 然后我们要求生成器 LLM 从给定的文档上下文生成问答对。

> 最后，我们使用评委 LLM 进行质量控制。LLM 将为每个问答上下文样本打分，我们可以使用该分数来过滤掉不良样本。

> 为什么不使用像 `Ragas` 这样的框架来为 RAG 生成合成测试集？因为 `Ragas` 内部使用的是英语 LLM 提示。目前无法将 `Ragas` 与非英语文档一起使用。

> 我在本文中使用了 `OpenAI` 指南“RAG 评估” [2] 作为代码的基础。不过，我尝试简化他们的示例代码，并根据一些研究结果更改了评估 [3、4、5]。

## 读取文件

我们将使用 `LangChain` 读取包含所有文件的文件夹。

首先，我们需要安装所有必要的软件包。`LangChain` 的 `DirectoryLoader` 使用非结构化库来读取各种文件类型。在本文中，我将仅读取 PDF，以便我们可以安装较小版本的 `unstructured`。

```
pip install langchain0.3.6 langchain-community0.3.4 unstructured[pdf]==0.16.3 tqdm
```

现在我们可以读取数据文件夹以获取 `LangChain` 文档。以下代码首先从文件夹中加载所有 PDF 文件，然后将它们分块为相对较大的 2000 个块。

```
from langchain_text_splitters.character import RecursiveCharacterTextSplitter
from langchain_community.document_loaders.directory import DirectoryLoader
loader = DirectoryLoader("/path/to/data/folder", glob="**/*.pdf", show_progress=True)
docs = loader.load()
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=2000,
    chunk_overlap=200,
    add_start_index=True,
    separators=["\n\n", "\n", ".", "", "", ""]
)
docs_processed = []
for doc in docs:
    docs_processed.extend(text_splitter.split_documents([doc]))
```

结果是 `docs_processed` 包含 `Document` 类型的项目的列表。每个文档都有一些 `metadata` 和实际的 `page_content`。

此文档列表是我们的知识库，我们将根据其上下文创建问答对 `page_content`。

## 生成问答上下文样本

使用 `OpenAI` 客户端和我们之前创建的模型，我们首先编写一个生成器函数来从我们的文档中创建问题和答案。

```
def qa_generator_llm(context: str, client: OpenAI, model: str = "AMead10/Llama-3.2-3B-Instruct-AWQ"):
    generation_prompt = """
    您的任务是编写一个给定上下文的事实性问题和一个答案。

    您的事实性问题应该能够用上下文中具体、简洁的事实信息来回答。

    您的事实性问题的表述风格应该与用户在搜索引擎中提出的问题相同。

    这意味着您的事实性问题不得提及“根据文章”或“上下文”之类的内容。
    """

    chat_completion = client.chat.completions.create(
        messages=[
            {
                "role": "system",
                "content": "您是问答对生成器。",
            },
            {
                "role": "user",
                "content": generation_prompt.format(context=context),
            }
        ],
        model=model,
        temperature=0.5,
        top_p=0.99,
        max_tokens=500
    )

    return chat_completion.choices[0].message.content
```

如果您想使用英语以外的语言，您将需要翻译 `generation_prompt`（和系统指令）。

接下来，我们只需循环遍历知识库中的所有文档块，并为每个块生成一个问题和一个答案。

```
from tqdm.auto import tqdm

outputs = []
for doc in tqdm(docs_processed):
    # 生成 QA 对
    output_QA = qa_generator_llm(doc.page_content, client)
    try:
        question = output_QA.split("事实问题： ")[-1].split("答案： ")[0]
        answer = output_QA.split("答案： ")[-1]
        assert len(answer) < 500, "答案太长"
        outputs.append(
            {
                "context": doc.page_content,
                "question": question,
                "answer": answer,
                "source_doc": doc.metadata["source"],
            }
        )
    except Exception as e:
        print(e)
```

根据您拥有的 PDF 文件数量，这可能需要一段时间。如有必要，请不要忘记翻译字符串 `output_QA.split`。

为了生成 RAG 评估数据集，我使用了一份来自欧盟的关于欧盟 AI 法案监管的 PDF （根据CC BY 4.0许可）。这是我生成的原始 `outputs` 数据集：

```
[
  {
    "context": "《欧盟官方公报》\n\n2024/1689\n\n欧洲议会和理事会条例 (EU) 2024/1689\n\n2024 年 6 月 13 日\n\n制定人工智能协调规则并修订条例 (EC) No 300/2008、(EU) No 167/2013、(EU) No 168/2013、(EU) 2018/858、(EU) 2018/1139 和 (EU) 2019/2144 以及指令 2014/90/EU、(EU) 2016/797 和 (EU) 2020/1828（人工智能法）\n\n（带 EEA 的文本欧洲议会和欧盟理事会，\n\n考虑到《欧洲联盟运作条约》，特别是其中的第 16 条和第 114 条，\n\n考虑到欧盟委员会的提议，\n\n在将立法法案草案提交给各国议会后，\n\n考虑到欧洲经济和社会委员会的意见 (1)，\n\n考虑到欧洲中央银行的意见 (2)，\n\n考虑到地区委员会的意见 (3)，\n\n根据普通立法程序 (4)，\n\n鉴于:\n\n(1)’",
    "question": "欧洲议会和理事会条例 (EU) 2024/1689 的颁布日期是什么时候？\n",
    "answer": "2024 年 6 月 13 日",
    "source_doc": "documents/OJ_L_202401689_EN_TXT.pdf"
  },
  {
    "context": "考虑到地区委员会的意见（3），\n\n按照普通立法程序行事（4），\n\n鉴于：\n\n（1）\n\n本条例的目的是通过制定统一的法律框架，特别是为联盟内人工智能系统（AI系统）的开发、投放市场、投入服务和使用制定统一的法律框架，以改善内部市场的运作，符合联盟的价值观，促进以人为本和值得信赖的人工智能（AI）的采用，同时确保高水平保护《欧洲联盟基本权利宪章”（“宪章”）所载的健康、安全和基本权利，包括民主、法治和环境保护，防止联盟内人工智能系统的有害影响，并支持创新。本条例确保基于人工智能的商品和服务的跨境自由流动，从而防止成员国对人工智能系统的开发、营销和使用施加限制，除非本条例明确授权。\n\n(2)\n\n本条例应根据《宪章》所载的联盟价值观适用，促进对自然人、企业、民主、法治和环境保护的保护，同时促进创新和就业，使联盟成为采用可信人工智能的领导者。\n\n(3)’",
    "question": "关于联盟人工智能系统的开发、投放市场、投入服务和使用的拟议条例的目的是什么？\n",
    "answer": "通过为联盟内人工智能系统的开发、投放市场、投入服务和使用制定统一的法律框架，改善内部市场的运作。",
    "source_doc": "documents/OJ_L_202401689_EN_TXT.pdf"
  },
  ...
]
```

### 过滤掉错误的问答对

接下来，我们使用 `LLM` 作为评判者，自动过滤掉坏样本。

当使用 `LLM` 作为评估样本质量的评判标准时，最好使用与生成样本时不同的模型，因为存在自我偏好偏差[6] —— 你不会给自己的论文评分，对吧？

在评判我们生成的问题和答案时，我们可以使用很多可能的提示。

为了构建我们的提示，我们可以使用 `G-Eval` 论文 [3] 中的一个结构：

> 我们从任务介绍开始
>
> 我们提出了评估标准
>
> 我们希望模型能够进行思维链（`CoT`）推理 [7]，以提高其性能
>
> 我们最后要求总分

对于评估标准，我们可以使用一个列表，其中每个标准如果得到满足就会加一分[4]。

评估标准应确保问题、答案和上下文相互契合、合理。

以下是 `OpenAI` RAG 评估手册中的两个评估标准 [2]：

依据性：是否可以从给定的背景来回答该问题？

独立：这个问题不需要任何背景就能理解吗？（为了避免出现这样的问题 `"What is the name of the function used in this guide?"`）

`RAGAS` 论文 [5] 中还有另外两个评估标准：

该 `judge_llm()` 函数会批评问题、答案和上下文样本，并在最后得出总体评分：



# RAG 评估数据集构建与评判

```python
def judge_llm(
    context: str,
    question: str,
    answer: str,
    client: OpenAI,
    model: str = "AMead10/Llama-3.2-3B-Instruct-AWQ",
):
    critique_prompt = """
```

您将得到一个问题、一个答案和一个上下文。

您的任务是使用下面描述的加法计分系统提供总评分。

分数从 0 开始，并根据每个评估标准的满足程度进行累积：

## 评估标准：

> 扎实性：是否可以从给定的上下文中回答问题？如果可以从上下文中回答问题，则加 1 分

> 独立性：对于具有领域知识/互联网访问权限的人来说，是否可以在没有任何上下文的情况下理解问题？如果问题是独立的并且可以独立存在，则加 1 分。

> 忠实性：答案应以给定的上下文为基础。如果答案可以从上下文中得出，则加 1 分

> 答案相关性：生成的答案应该解决所提供的实际问题。如果答案实际上回答了问题

提供以下答案：

> 答案：：：

> 评价：（您的评级理由，以文本形式）

> 总评分：（您的评分，以 0 到 4 之间的数字形式）

> 您必须在答案中提供“评价：”和“总评分：”的值。

> 现在这里是问题、答案和上下文。

> 问题：`{question}`\n

> 答案：`{answer}`\n

> 上下文：`{context}`\n

> 答案::: “”

```python
chat_completion = client.chat.completions.create(
    messages=[
        {"role": "system", "content": "您是中立的评判者。"},
        {
            "role": "user",
            "content": critique_prompt.format(
                question=question, answer=answer, context=context
            ),
        },
    ],
    model=model,
    temperature=0.1,
    top_p=0.99,
    max_tokens=800
)

return chat_completion.choices[0].message.content
```

现在我们循环遍历生成的数据集并批评每个样本：

对于 `tqdm(outputs)` 中的输出：

尝试：

```python
evaluation = judge_llm(
    context=output["context"],
    question=output["question"],
    answer=output["answer"],
    client=client,
)
score, eval = (
    int(evaluation.split("总评分：")[-1].strip()),
    evaluation.split("总评分：")[-2].split("评估：")[1],
)
output.update(
    {
        "score": score,
        "eval": eval,
    }
)
except Exception as e:
    print(e)
```

让我们过滤掉所有坏样本。

由于生成的数据集将成为评估目的的基本事实，因此我们只应允许非常高质量的数据样本。这就是为什么我决定只保留得分最高的样本。

```python
dataset = [doc for doc in 输出 if doc["score"] >= 4]
```

## 以下是我们最终的 RAG 评估数据集（以 `Pandas DataFrame` 格式）：

```python
import pandas as pd

pd.set_option("display.max_colwidth", 200)

df = pd.DataFrame(dataset)

display(df)
```

我们生成的英语 RAG 评估数据集的可视化，包含以下列：上下文、问题、答案、源文档、分数和评估。

经过过滤的英语版 RAG 评估数据集（PDF 文件，CC BY 4.0 许可）。图片由作者提供

## 保存数据集

我们可以将 `Pandas DataFrame` 转换为 Hugging Face 数据集。然后，我们可以将其保存到磁盘并在需要时加载它。

```python
%pip install datasets==3.0.2
```

**从数据集保存导入** `Dataset`

```python
dataset = Dataset.from_pandas(df, split="test")

dataset.save_to_disk("path/to/dataset/directory")
```

**从数据集加载导入** `load_dataset`

```python
dataset = load_dataset("path/to/dataset/directory")
```

我们还可以将数据集上传到 Hugging Face Hub

## 使用另一种语言创建 RAG 数据集

> 我不会说西班牙语。但是，我从欧盟法律下载了一份西班牙法律文件（根据 CC BY 4.0 许可），并使用 DeepL Translate 转换了我的提示。我不知道该文件说了什么，但让我们看看我们是否可以生成一个新的数据集。

> 替换输入文档并将提示从英语翻译成西班牙语后，过滤后的 RAG 数据集如下所示：

> 我们生成的西班牙法律 RAG 评估数据集的可视化，包含以下列：上下文、问题、答案、源文档、分数、评估。

> 该数据集为特定领域的西班牙 RAG 数据集，由欧盟的西班牙法律文件自动创建（根据 CC BY 4.0 许可）。

> 通过使用我们自己的数据集生成代码，我们可以使其适应我们想要的任何语言和领域。

## 结论

从文档集合中自动创建 RAG 评估数据集非常简单。我们所需要的只是 LLM 生成器的提示、LLM 评委的提示，以及中间的一些 Python 代码。

要更改 RAG 评估数据集的域，我们只需交换提供给的文档 `DirectoryLoader`。文档不必是 PDF 文件，也可以是 CSV 文件、markdown 文件等。

要更改我们的 RAG 评估数据集的语言，我们只需将 LLM 提示从英语翻译成另一种语言。

如果生成的数据样本不足以满足您的用例，您可以尝试修改提示。此外，使用更大、更好的 LLM 将提高数据集的质量。

## 参考

> [1] P. Lewis 等人 (2021)，面向知识密集型 NLP 任务的检索增强生成，arXiv:2005.11401

> [2] A. Roucher (2024)，RAG 评估，Hugging Face AI Cookbook，访问于 2024 年 11 月 1 日

> [3] Y. Liu 等人 (2023)，G-Eval：使用具有更好人体对齐效果的 GPT-4 进行 NLG 评估，arXiv:2303.16634

> [4] W. Yuan 等人 (2024)，自我奖励语言模型，arXiv:2401.10020

> [5] S. Es 等人 (2023)，RAGAS：检索增强生成的自动评估，arXiv:2309.15217

> [6] K. Wataoka、T. Takahashi 和 R. Ri (2024)，LLM 法官中的自我偏好偏差，arXiv:2410.21819

> [7] J. Wei 等人 (2022)，思维链提示在大型语言模型中引发推理，arXiv:2201.11903
