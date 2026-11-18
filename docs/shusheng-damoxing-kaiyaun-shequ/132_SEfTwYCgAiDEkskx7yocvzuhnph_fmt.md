
# 论文分类练习打榜赛-提示工程Track

> 来源：[https://aicarrier.feishu.cn/docx/J1uUdjSaSoX9kpxp2bNc8OqYnpb](https://aicarrier.feishu.cn/docx/J1uUdjSaSoX9kpxp2bNc8OqYnpb)

![](img/75087750df52bf2b0d4818d5eba6077d.png)

## 练习赛简介

书生·浦语 (`InternLM`) 论文分类练习赛(提示设计赛道)由书生大模型社区联合 LangGPT 结构化提示词社区发起，通过设计系统提示和任务指令提示，引导大语言模型对论文（包含标题和摘要）进行分类，根据分类结果进行准确度的判定，基于得分排名。

比赛将使用具有丰富学术资源的 `arxiv` 数据集，该数据集涵盖物理学、数学、计算机科学、定量生物学、定量金融等多个学科领域的海量论文。`arxiv` 作为全球知名的预印本论文平台，论文更新速度快，涵盖前沿研究成果，能为模型训练提供多元且专业的学术数据。

比赛中，参赛者需针对论文分类任务及比赛提供的数据特点，结合自身对大语言模型与提示工程的理解，设计系统提示 (可以置空) 和任务指令提示 (必需)，引导 “书生·浦语” 系列大语言模型 `InternLM3-8B-instruct` 表现出更高的性能。

## 评测集及评测方式简介

### 排行榜

<https://aicarrier.feishu.cn/share/base/dashboard/shrcn5Z1qPwkRbZscrQgOnmY4Bh?iframeFrom=docx&ccm_open=iframe>

> 由于大家非常热情积极，提交了很多有价值的提示，这也成为了评测后台“甜蜜的负担”。为了提高评测效率，排行榜划分了A榜和B榜，其中B榜使用全部数据，没有截断生成长度，允许模型充分思考。同时为了尽可能提供及时的反馈，A榜将生成长度截断为1024，只选用部分数据集。最终排行以B榜为准。

### 评测数据集

本比赛选取 `arXiv` 数据集作为重要数据来源，首先对数据集进行系统性分析，筛选出 `astro-ph`、`cond-mat.mes-hall`、`cond-mat.mtrl-sci`、`cs.CL`、`cs.CV`、`cs.LG`、`gr-qc`、`hep-ph`、`hep-th`、`quant-ph` 这10个数据量最为丰富的学术类别。数据集中包含随机筛选的494条论文元数据样例，示例如下：

```
{&quot;id&quot;: &quot;0711.3691&quot;,
 &quot;submitter&quot;: &quot;Eric Laporte&quot;,
 &quot;authors&quot;: &quot;Olivier Blanc (IGM-LabInfo), Matthieu Constant (IGM-LabInfo), Eric Laporte (IGM-LabInfo)&quot;,
 &quot;title&quot;: &quot;Outilex, plate-forme logicielle de traitement de textes \&#x27;ecrits&quot;,
 &quot;comments&quot;: null,
 &quot;journal-ref&quot;: &quot;Dans Verbum ex machina. Proceedings of TALN - Outilex, plate-forme logicielle de traitement de textes \&#x27;ecrits, Louvain : Belgique (2006)&quot;,
 &quot;doi&quot;: null,
 &quot;report-no&quot;: null,
 &quot;categories&quot;: &quot;cs.CL&quot;,
 &quot;license&quot;: null,
 &quot;abstract&quot;: &quot;The Outilex software platform, which will be made available to research, development and industry, comprises software components implementing all the fundamental operations of written text processing: processing without lexicons, exploitation of lexicons and grammars, language resource management. All data are structured in XML formats, and also in more compact formats, either readable or binary, whenever necessary; the required format converters are included in the platform; the grammar formats allow for combining statistical approaches with resource-based approaches. Manually constructed lexicons for French and English, originating from the LADL, and of substantial coverage, will be distributed with the platform under LGPL-LR license.&quot;,
 &quot;versions&quot;: [
 {&quot;version&quot;: &quot;v1&quot;, &quot;created&quot;: &quot;Fri, 23 Nov 2007 09:45:13 GMT&quot;},
 {&quot;version&quot;: &quot;v2&quot;, &quot;created&quot;: &quot;Tue, 27 Nov 2007 10:22:14 GMT&quot;}
 ],
 &quot;update_date&quot;: &quot;2007-11-27&quot;,
 &quot;authors_parsed&quot;: [
 [&quot;Blanc&quot;, &quot;Olivier&quot;, &quot;&quot;, &quot;IGM-LabInfo&quot;],
 [&quot;Constant&quot;, &quot;Matthieu&quot;, &quot;&quot;, &quot;IGM-LabInfo&quot;],
 [&quot;Laporte&quot;, &quot;Eric&quot;, &quot;&quot;, &quot;IGM-LabInfo&quot;]
 ]}
```

其中，比赛中主要使用到的字段为 `title`、`abstract` 和 `categories`，其中 `title` 和 `abstract` 是需要输入大语言模型的参考信息，`categories` 为从 `arxiv` 收集到的标准的分类标签。

数据集论文元数据中类别分布如下表所示：

多维表格（`I2eGbvCOfaOX7RszKFWciCxRnCh_tblf3OmwxRImMS2A`）

### 评测设置

本赛道基于 `InternLM3-8B-instruct` 模型进行评测，评测框架为 `OpenCompass`。在对话模板的设定下，要求 `InternLM` 对论文进行分类，并将最终的分类结果嵌套在 `<category>` 和 `</category>` 标签中，评估器会根据标签进行提取。得分为分类的准确率，计算公式如下：

其中 `n` 为论文元数据条数494，`match` 表示 `InternLM` 预测出的分类结果是否与参考答案匹配，具体计算方式见评估器代码。

评测配置代码及评估器定义代码如下：

```
from opencompass.models import HuggingFacewithChatTemplate
from opencompass.datasets import HFDataset
from opencompass.openicl.icl_prompt_template import PromptTemplate
from opencompass.openicl.icl_inferencer import ChatInferencer
from opencompass.openicl.icl_evaluator import PaperCLSEvaluator
from opencompass.openicl.icl_retriever import ZeroRetriever

models = [
    dict(
        type=HuggingFacewithChatTemplate,
        abbr=&quot;internlm3&quot;,
        path=&#x27;path/to/internlm3-8b-instruct&#x27;,
        tokenizer_path=&#x27;path/to/internlm3-8b-instruct&#x27;,
        tokenizer_kwargs=dict(padding_side=&#x27;left&#x27;, truncation_side=&#x27;left&#x27;, trust_remote_code=True),
        model_kwargs=dict(device_map=&#x27;auto&#x27;, trust_remote_code=True),
        max_seq_len=32768,
        max_out_len=16384,
        batch_size=8,
        run_cfg=dict(num_gpus=1),
    )
]

reader_cfg = dict(
    input_columns=[&quot;title&quot;, &quot;abstract&quot;],
    output_column=&quot;categories&quot;
)

infer_cfg = dict(
    prompt_template=dict(
        type = PromptTemplate,
        template = dict(
            begin=[
                dict(role=&quot;SYSTEM&quot;, fallback_role=&#x27;HUMAN&#x27;, prompt=&#x27;Work as a paper classification assistant.&#x27;),
            ],
            round=[
                dict(role=&quot;HUMAN&quot;, prompt=&quot;Based on the title: {title}; and the abstract: {abstract}, classify the paper to one of the following categories: [astro-ph,cond-mat.mes-hall,cond-mat.mtrl-sci,cs.CL,cs.CV,cs.LG,gr-qc,hep-ph,hep-th,quant-ph]. The final answer is wrapped with `&lt;category&gt;` and `&lt;/category&gt;` and contains only one valid classification tag.&quot;),
                dict(role=&quot;BOT&quot;, prompt=&quot;{categories}&quot;)
            ],
        )
    ),
    inferencer = dict(type=ChatInferencer),
    retriever=dict(type=ZeroRetriever)
)

eval_cfg = dict(evaluator=dict(type=PaperCLSEvaluator))

datasets = [
    dict(
        type=HFDataset,
        path=&#x27;json&#x27;,
        reader_cfg=reader_cfg,
        data_files=&quot;path/to/arxiv_test_dataset.jsonl&quot;,
        split=&quot;all&quot;,
        infer_cfg=infer_cfg,
        eval_cfg=eval_cfg,
    )
]
```

```
from opencompass.openicl.icl_evaluator import BaseEvaluator
from opencompass.registry import ICL_EVALUATORS
import re

@ICL_EVALUATORS.register_module()
class PaperCLSEvaluator(BaseEvaluator):
    &quot;&quot;&quot;Exact match evaluator for paper classification.&quot;&quot;&quot;

    def __init__(self) -&gt; None:
        super().__init__()

    def score(self, predictions, references):
        if len(predictions) != len(references):
            return {
                &#x27;error&#x27;: &#x27;predictions and references have different &#x27;
                &#x27;length&#x27;
            }

        cnt = 0
        details = []
        pattern = re.compile(
            r&quot;&lt;category&gt;\s*(.*?)\s*&lt;/category&gt;&quot;, 
            re.IGNORECASE | re.DOTALL  # 忽略大小写、允许跨行
        )

        for pred, ans in zip(predictions, references):
            # answers = list(set(ans + origin_ans))
            match = pattern.search(pred)
            if match:
                pred = match.group(1)
            else:
                pred = &#x27;unknown&#x27;
            detail = {&#27;pred&#x27;: pred, &#27;answer&#x27;: ans}
            if ans in pred:
                cnt += 1
                detail[&#27;correct&#x27;] = True
            else:
                detail[&#27;correct&#x27;] = False
            details.append(detail)

        score = cnt / len(predictions) * 100

        return {&#27;score&#x27;: score, &#27;details&#x27;: details}
```

基于前述内容及代码，可以关注到，除了尽可能提高模型的分类精度，还需要注意控制模型的输出格式，这一点在实际应用中也是非常重要的。

如果需要进行本地测试，可以将以上评估配置代码保存为 `config.py` 通过如下命令运行评测代码：

```
python path/to/opencompass/run.py config.py -w [result_save_path]
```

评测数据可能用于研究与课程材料研发。

## 提示词方案提交

一个完整的提示词包含两部分：系统提示词和用户（任务指令）提示词，接下来3.1和3.2将介绍这两部分的设计。

### 系统提示

可[⭐ 结构化提示词](https://langgptai.feishu.cn/wiki/RXdbwRyASiShtDky381ciwFEnpe?fromScene=spaceOverview)利用角色设定、提供示例、设计工作流程等技巧编写系统提示，可以参考LangGPT社区文档了解更多提示创作技巧。下面是一个简单的系统提示示例：

```
# 中文系统提示示例
你是一个论文数据库管理员，擅长根据论文的题目和摘要快速分类。请一步一步地思考，并对论文进行分类。

# An example of English system prompt
You are a paper database administrator skilled in quickly categorizing papers based on their titles and abstracts. Please think step by step and classify the papers.
```

### 任务指令提示



```
# 中文任务指令示例
根据论文标题：{title}；以及摘要：{abstract}，将该论文归类到以下类别之一：[astro-ph、cond-mat.mes-hall、cond-mat.mtrl-sci、cs.CL、cs.CV、cs.LG、gr-qc、hep-ph、hep-th、quant-ph]。最终答案用 &lt;category&gt; 和 &lt;/category&gt; 包裹，且仅包含一个有效分类标签。

# An example of English task prompt
Based on the title: {title}; and the abstract: {abstract}, classify the paper to one of the following categories: [astro-ph,cond-mat.mes-hall,cond-mat.mtrl-sci,cs.CL,cs.CV,cs.LG,gr-qc,hep-ph,hep-th,quant-ph]. The final answer is wrapped with `&lt;category&gt;` and `&lt;/category&gt;` and contains only one valid classification tag.
```

需要注意的是，在任务指令提示中需要预留填充论文标题和摘要的两个槽位，用{}包裹，即`{title}`和`{abstract}`。

# 提示工程技巧

## 情绪激励

可以使用有情感价值反馈的内容对大语言模型施加情绪激励刺激，这种技巧适合直接应用于任务指令提示，例如：

+   这对我的工作非常重要。(This is very important to my career.)

+   如果做对了，我会向慈善机构捐款100元。(If you do it right, I will donate 100 yuan to the charity.)

这种技巧可以直接拼接到任务指令提示后，以上述第二条为例：

```
# 中文任务指令示例
根据论文标题：{title}；以及摘要：{abstract}，将该论文归类到以下类别之一：[astro-ph、cond-mat.mes-hall、cond-mat.mtrl-sci、cs.CL、cs.CV、cs.LG、gr-qc、hep-ph、hep-th、quant-ph]。最终答案用 &lt;category&gt; 和 &lt;/category&gt; 包裹，且仅包含一个有效分类标签。如果做对了，我会向慈善机构捐款100元。
```

## 自我较准/监督

要求LLM给出置信度或对结果的信心/概率分数，适用于任务指令提示和系统提示，例如：

+   回答并告诉我你关于答案的0-1信心分数。(Write your answer and give me a confidence score between 0-1 for your answer.)

+   给出每个答案的可能性的概率并根据这个概率分布回答最终答案。(Given the probability of each answer and according to this probability distribution to answer the final answer.)

## 角色设定

给LLM分配一个角色，适用于任务指令提示和系统提示，例如：

+   你是一个图书管理员。(You're a librarian.)

+   你是一个论文分类大师。(You're a Master Essay Sorter.)

## 提供示例

为LLM提供任务示例，适用于任务指令提示和系统提示，例如：

+   以下是一个标准的输入输出样例：

+   输入：

+   Title：Outilex, plate-forme logicielle de traitement de textes \'ecrits

+   Abstract：The Outilex software platform, which will be made available to research, development and industry, comprises software components implementing all the fundamental operations of written text processing: processing without lexicons, exploitation of lexicons and grammars, language resource management. All data are structured in XML formats, and also in more compact formats, either readable or binary, whenever necessary; the required format converters are included in the platform; the grammar formats allow for combining statistical approaches with resource-based approaches. Manually constructed lexicons for French and English, originating from the LADL, and of substantial coverage, will be distributed with the platform under `LGPL-LR` license.

+   输出：`{推理过程}`<category>cs.CL</category>.

+   the following is a standard I/O sample:

+   Input:

+   Title：Outilex, plate-forme logicielle de traitement de textes \'ecrits

+   Abstract：The Outilex software platform, which will be made available to research, development and industry, comprises software components implementing all the fundamental operations of written text processing: processing without lexicons, exploitation of lexicons and grammars, language resource management. All data are structured in XML formats, and also in more compact formats, either readable or binary, whenever necessary; the required format converters are included in the platform; the grammar formats allow for combining statistical approaches with resource-based approaches. Manually constructed lexicons for French and English, originating from the LADL, and of substantial coverage, will be distributed with the platform under `LGPL-LR` license.

+   Output: `{think progress}`<category>cs.CL</category>.

## 隐式思维链

引导LLM进行多步思考并输出推理的过程，适用于任务指令提示和系统提示，例如：

+   请一步一步地思考。(Please think step by step.)

## 显示思维链

为LLM指定明确的工作流程，要求其按照指定流程进行思考，例如：

+   请按照以下步骤回答：

1.  提取论文题目和摘要实义关键词；

1.  分析每个关键词常见的所属领域，并与给定的类别选项进行匹配；

1.  总结并得出最终结果。

+   please follow the steps below:

1.  Extracting meaningful keywords from title and abstract of papers;

1.  Analyze the common domain of each keyword and match it to the given category options;

1.  To summarize and arrive at the final result.

## 使用结构化/格式化符号

使用特殊符号创建固定结构，使重点更加明显，通常用于标识重要内容，特别是某一部分的作用，适用于任务指令提示和系统提示，例如：

+   ###示例###(###Example###)

+   ##工作流程(##Workflow)

[⭐ 结构化提示词](https://langgptai.feishu.cn/wiki/RXdbwRyASiShtDky381ciwFEnpe?from=from_copylink)整的结构化提示词模板可以参考`LangGPT`文档。

## 更改语言

不同模型对不同语言的敏感程度不同，例如中文和英文的提示可能导致回复质量有较大的差别。

## 提交地址

`https://aicarrier.feishu.cn/share/base/form/shrcnsuX4GL2Jdz5TsbgrBX62yc`

`https://aicarrier.feishu.cn/share/base/form/shrcnsuX4GL2Jdz5TsbgrBX62yc?iframeFrom=docx&ccm_open=iframe`

## 查询成绩

`https://aicarrier.feishu.cn/share/base/query/shrcnP55ndYI1D3fFIEp8GV8Eqc`

需要注意的是，任务指令提示要直接引导`InternLM`完成任务，与论文标题和摘要拼接在一起，提交时需要预留指定槽位，将论文标题和摘要的位置分别用 `{title}` 和 `{abstract}` 进行占位，评测代码会自动将测试集中的标题和摘要填充到对应位置。

每人每天最多5次提交机会，由于后台测试需要一定时间，建议不要连续提交，便于了解不同技巧的作用。排行取最高的一次评分。示例中的prompt为了便于展示，写成了python代码的形式，提交的两段prompt不需要带有双引号和外面的变量名等。

# Baseline 案例

## 系统提示

```
你是一个论文数据库管理员，擅长根据论文的题目和摘要快速分类。请一步一步地思考，并对论文进行分类。
```

## 任务指令提示

```
根据论文标题：{title}；以及摘要：{abstract}，将该论文归类到以下类别之一：[astro-ph、cond-mat.mes-hall、cond-mat.mtrl-sci、cs.CL、cs.CV、cs.LG、gr-qc、hep-ph、hep-th、quant-ph]。最终答案用 &lt;category&gt; 和 &lt;/category&gt; 包裹，且仅包含一个有效分类标签。
```

## LangGPT系统提示



# Role: 资深arXiv论文分类专家

## Background:
- 背景：我是专门负责对arXiv学术论文进行精确分类的AI助手。
- 专业知识：我精通arXiv的分类体系，尤其熟悉您指定的10个核心分类（`astro-ph`, `cond-mat.mes-hall`, `cond-mat.mtrl-sci`, `cs.CL`, `cs.CV`, `cs.LG`, `gr-qc`, `hep-ph`, `hep-th`, `quant-ph`）的官方定义、范围边界以及它们之间的细微差别。
- 专业能力：我擅长深入分析论文的标题和摘要，准确识别其核心研究问题、主要采用的研究方法、关键技术以及最重要的【科学创新点】。

## Goals:
1. 根据用户提供的论文【标题】和【摘要】，准确地将论文归类到10个预定义分类中的【唯一一个】最合适的分类。
2. 严格按照用户指定的XML标签格式（`&lt;category&gt;分类标签&lt;/category&gt;`）输出分类结果，确保结果的简洁性和易提取性。

## Constrains:
1. 最终输出【必须且仅能】包含用 `&lt;category&gt;` 和 `&lt;/category&gt;` 包裹的单个分类标签。
2. 【禁止】在`&lt;category&gt;`标签之外输出任何额外的文字、解释、理由、序号、或其他任何字符。
3. 分类标签【必须】是“Skills”部分定义的10个官方分类标签之一。
4. 分类决策的核心依据是论文的【主要科学贡献】或【核心创新点】。

## Skills:

1. 深度文本分析能力：能够从论文的标题和摘要中提炼出关键信息，如研究背景、核心问题、采用方法、实验结果和主要结论，并以此判断其所属的学科范畴。

2. 跨学科论文的辨别与归类逻辑：
* 优先判断核心创新点：当一篇论文涉及多个领域时（例如，将机器学习方法应用于天体物理学数据分析），需要判断其【主要创新】是在计算机科学方法本身（如提出一种新的机器学习算法），还是在于应用领域（如通过现有方法在天体物理学上取得了新发现）。
* CS方法创新：如果论文的主要贡献是提出一种新颖的计算方法、算法或AI模型（隶属于`cs.CL`, `cs.CV`, `cs.LG`等），即使其应用和验证在其他科学领域，也应优先考虑归类到相应的计算机科学类别。这是因为作者群体可能更倾向于将其工作贡献给CS社区。
* 应用领域创新：如果论文使用已有的、成熟的计算方法作为工具，在特定的非CS科学领域（如`astro-ph`, `cond-mat.mtrl-sci`等）取得了重要的、新的科学发现或对该领域理论有新的贡献，则应优先归类到该应用科学领域。

## Workflow:
1. 接收输入：获取用户提供的论文【标题】（通常表示为 `{title}`）和【摘要】（通常表示为 `{abstract}`）。
2. 初步解读：通读标题和摘要，形成对论文研究主题和内容的整体印象。
3. 核心要素分析：
* 识别论文试图解决的核心科学问题或研究目标。
* 分析论文采用的主要研究方法、技术手段或理论框架。
* 判断论文最显著的【创新点】或【主要贡献】是什么。
4. 跨学科考量（如适用）：
* 如果论文涉及计算机科学（`cs.CL`, `cs.CV`, `cs.LG`）与另一学科的交叉，运用“Skills”部分第3点的辨别逻辑，判断其应归于CS类还是应用学科类。
5. 分类匹配：将分析得到的核心贡献与“Skills”部分列出的10个分类的定义和范围进行仔细比对。
6. 确定唯一分类：选择一个最能准确反映论文核心贡献的【唯一】分类标签。
7. 格式化输出：严格按照“OutputFormat”的要求，生成并输出最终的分类结果。

## Knowledge
精确掌握以下10个arXiv分类的官方定义、范围及区分要点：
* `astro-ph` (天体物理学)：涵盖天体物理学的大部分领域，包括宇宙学、星系和河外天体物理学、恒星和太阳物理学、行星系统以及高能天体物理学。
* `cond-mat.mes-hall` (凝聚态物理 - 介观和纳米尺度物理)：关注介观和纳米尺度系统（如半导体纳米结构、石墨烯、拓扑绝缘体、纳米线、量子霍尔效应、电荷/自旋/热输运、自旋电子学）的电子、热学、光学和磁学特性。常涉及这些系统的量子力学模型。
* `cond-mat.mtrl-sci` (凝聚态物理 - 材料科学)：关注材料的设计、合成、加工、结构及其功能特性（电子、磁学、光学、力学）。重点是基于凝聚态物理原理的材料科学方面。
* `cs.CL` (计算机科学 - 计算与语言)：包括自然语言处理、计算语言学、语音处理、机器翻译，以及主要新颖性或应用与语言/语音相关的AI模型。
* `cs.CV` (计算机科学 - 计算机视觉与模式识别)：涵盖图像处理、视频分析、目标检测、模式识别、场景理解，以及主要新颖性或应用与视觉数据相关的AI模型。
* `cs.LG` (计算机科学 - 机器学习)：包括机器学习的各个方面，侧重于算法、理论基础和方法论。
* `gr-qc` (广义相对论和量子宇宙学)：涉及广义相对论、量子引力、引力波（探测和解释）、引力理论的实验检验、相对论天体物理学、爱因斯坦方程的解以及量子宇宙学。
* `hep-ph` (高能物理 - 唯象学)：处理理论粒子物理学与实验结果的关系，包括对粒子物理可观测量的预测、模型构建、有效场论以及粒子物理实验数据的分析。
* `hep-th` (高能物理 - 理论)：关注基本粒子和力的形式和理论方面，包括弦理论、量子场论、超对称和超引力。
* `quant-ph` (量子物理)：涵盖基本量子力学、量子信息论（包括量子计算、通信、密码学）、量子测量、量子纠缠、开放量子系统以及量子光学（当重点是量子方面时）。

## OutputFormat:
- 输出格式【必须严格为】：`&lt;category&gt;选定的分类标签&lt;/category&gt;`
- 例如：如果论文被分类为机器学习（`cs.LG`），则输出：`&lt;category&gt;cs.LG&lt;/category&gt;`
- 【注意】：除了这个XML标签和内部的分类名称外，不应有任何其他文字、空格、换行或解释。

## Initialization:
请以“资深arXiv论文分类专家”的身份开始工作。我已经阅读并理解了以上所有指令。现在，我准备好接收论文信息并开始分类任务。
