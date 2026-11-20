
# 文档解析与信息抽取技术方案

## PaddleOCR

+   PaddleOCR：[点击进入](https://github.com/PaddlePaddle/PaddleOCR/tree/main)

PaddleOCR 是由 Baidu (及其生态) 基于其深度学习框架 PaddlePaddle 提供的开源 OCR 工具箱。支持从 PDF 或图像文档转为结构化数据（适配 AI 场景），支持 100+ 语言。最新版本 3.0 在其技术报告中提出：PP-OCRv5、PP-StructureV3、PP-ChatOCRv4 三大解决方案，覆盖文字识别、多版式文档解析、关键 信息提取。

![](img/5448d6806cc1b3cb88b27b8a4d51809e.png)

在早期版本（如 PP-OCRv3）中，其结构可概括为：“检测 (Detection) → 分类 (Classification of orientation) → 识别 (Recognition)”。使用多种模型，例如检测模型（DBNet 等）、识别模型（如 SVTR），在 3.0 版本中，其“PP-StructureV3”整合了布局分析、表格识别、结构抽取。同时还最新推出了还推出了PaddleOCR-VL 的 Vision-Language 模型版本（0.9B 参数的 VLM），用于多语种文档解析。

PaddleOCR-VL 是推出的一个专注于“文档解析／视觉-语言模型 (Vision-Language Model, VLM)”功能的新模块，采用了视觉-语言模型架构以应对更高阶的需求。在解析多模态数据方面，PaddleOCR将这项工作分为两部分：

1. 首先检测并排序布局元素。

2. 使用紧凑的视觉语言模型精确识别每个元素。

该系统分为两个明确的阶段运行。

第一阶段是执行布局分析（PP-DocLayoutV2），此部分标识文本块、表格、公式和图表。它使用：

+   RT-DETR 用于物体检测（基本上是边界框 + 类标签）。

+   指针网络 （6 个转换器层）可确定元素的读取顺序 ，从上到下、从左到右等。

最终输出统一模式的图片标注数据，如下图所示：

![](img/63652ae879a3da6bf72b7d93bd380ab9.png)

第二阶段则是元素识别（PaddleOCR-VL-0.9B），这就是视觉语言模型发挥作用的地方。它使用：

+   NaViT 风格编码器 （来自 Keye-VL），可处理动态图像分辨率。无平铺，无拉伸。

+   一个简单的 2 层 MLP， 用于将视觉特征与语言空间对齐。

+   ERNIE-4.5–0.3B 作为语言模型，该模型规模虽小但速度很快，并且采用 3D-RoPE 进行位置编码

最终模型输出结构化 Markdown 或 JSON 格式的文件用于后续的处理。

![](img/c144aa1c5dd49adabecac1b089e52fcb.png)

这个小小的设计决策， 将布局和识别分离，使得 PaddleOCR-VL 比通常的一体化系统更快、更稳定。同时根据实际的测试，其运行和解析速度也更快。在 A100 GPU 上， 吞吐量为 1.22 页/秒，。比 MinerU2.5 快 15.8%， VRAM 比 dots.ocr 少约 40%。

## DeepSeek-OCR

+   DeepSeek-OCR：[点击进入](https://github.com/deepseek-ai/DeepSeek-OCR)

DeepSeek 于 2025年10月21日发布了 DeepSeek-OCR 模型，仅需7G的显存，就能完成高精度的表格、公式识别，图片语义识别，并且在多项评测指标中一举拿下SOTA成绩。其 Github 相较于其他项目相对比较"简陋"，仅仅提供了`Transformers` 和 `vLLM` 启动 DeepSeek-OCR的服务示例代码文件：

![](img/f3acb61df8b43eec7bf4eeb8368963ee.png)

## dots.ocr

+   dots.ocr：[点击进入](https://github.com/dots-ai/dots.ocr)

dots.ocr 是由 rednote‑hilab（HiLab团队）开源的多语种文档布局解析工具。官方介绍中强调：“一个统一的 Vision-Language 模型（≈1.7 B 参数）即可完成布局检测 + 内容识别 +阅读顺序排序”。支持文本、表格、公式、以及多语言输入。

![](img/5496e5e3c749dc075d567f23538d9474.png)

dots.ocr 的特点是用一个 VLM（1.7 B 参数）来统一布局解析+内容识别，而不是传统将检测、识别、结构分开。用户可通过不同 prompt 来切换任务（如“请输出版式元素的 bbox、类别、文本”）→ 即说明模型采用 prompt + VLM 的方式。

![](img/80cbe02215e76c905e944fb47e150fc5.png)

非常适合需要快速处理多语种、混版式文档，且希望用一个统一模型／prompt 来搞定。虽然表现不错，但对于极复杂的表格（如跨页表、合并单元格）或特殊版式效果并不是很理想。

关于主流的MinerU、PaddleOCR-VL、DeepSeek-OCR等解析方案，均有本地部署版本，链接: https://pan.baidu.com/s/1k_4w-NB2f8dUJMdh4lVXzg?pwd=e2i6 提取码: e2i6

更多爆款体系课程信息，欢迎扫描下方二维码，助教老师会为你详细解答：

![](img/550944e91dffd6ee8e674aaf9483eeb0.png)

![](img/20e44852d19823cc62834ef95a953166.png)

当然，无论OCR技术多先进，输出的文本仍然是非结构化的:段落杂糅、缺少标注。这就需要下一步的信息抽取来赋予其结构。

## 信息抽取

### 3.2 `LangExtract`信息抽取

信息抽取（Information Extraction）是将非结构化文本转化为结构化数据的过程。在传统NLP中，这通常包括命名实体识别（NER）、关系抽取（RE）、事件抽取等任务。然而，以往这些任务需要训练复杂模型、编写规则，对于垂直领域还面临着大量人工标注成本。

假设你是一家大型医院的数据分析师,每天医生会产生数千份病历记录。这些记录都是自由文本形式：

> "患者张某,男,45岁,主诉胸痛3天。既往有高血压病史10年,目前服用缬沙坦80mg每日一次。体格检查:血压150/95mmHg..."

现在医院想要统计所有患者的用药情况、剂量分布、疾病共病模式,怎么做?

+   **人工标注?** 数千份病历,至少需要数周时间,且容易疲劳出错

+   **正则表达式?** 药物名称、剂量的表述千变万化,"每日一次"、"qd"、"1次/天"都是同一个意思

+   **训练专用模型?** 需要大量标注数据、算力资源,还要针对不同科室分别训练

这就是非结构化文本处理的核心痛点——信息价值高,但提取成本更高。

#### 传统信息提取方法对比

| 方法类型 | 优点 | 局限性 | 适用场景 |

| --- | --- | --- | --- |

| 正则表达式/规则 | 精准、可控 | 规则复杂、维护困难、泛化能力差 | 格式固定的简单文本 |

| 传统NER模型 | 自动学习模式 | 需要大量标注数据、跨领域迁移差 | 有训练数据的特定领域 |

| 通用NLP工具 | 开箱即用 | 准确率低、无法定制 | 通用场景的粗粒度提取 |

| 人工标注 | 最准确 | 成本高、速度慢、无法规模化 | 小规模高价值数据 |

当然，除了 Microsoft GraphRAG 等项目，还有更轻量的工具推荐使用 - `LangExtract`。

`LangExtract` 是 Google 开源的一个 Python 库,它的核心使命可以用一句话概括:利用大型语言模型,将非结构化文本转换为结构化数据,并确保每个提取结果都精确对应回原文位置。

+   能力一:零代码定义提取任务(Prompt-Driven)

在传统方法中,提取药物信息可能需要编写数百行正则表达式或训练一个NER模型。而在 `LangExtract` 中,只需用自然语言描述任务:

```
    # 示例:定义提取任务的 Prompt
    extraction_prompt = &quot;&quot;&quot;
    从临床笔记中提取以下信息:
    1. 药物名称(medication)
    2. 剂量(dosage) 
    3. 给药频率(frequency)
    4. 持续时间(duration)

    请确保提取的内容与原文完全一致。
    &quot;&quot;&quot;

    # 这就是全部!无需编写任何解析逻辑
```

+   能力二:少样本学习(Few-Shot Learning)

仅有 prompt 还不够,`LangExtract` 支持提供少量示例来"教会"模型输出的具体格式:

```
import langextract as lx

# 定义一个示例:告诉模型&quot;什么样的输入应该产生什么样的输出&quot;
example = lx.data.ExampleData(
    text=&quot;患者目前服用缬沙坦80mg每日一次,已持续3个月。&quot;,
    extractions=[
        lx.data.Extraction(
            text=&quot;缬沙坦&quot;,
            class_name=&quot;medication&quot;
        ),
        lx.data.Extraction(
            text=&quot;80mg&quot;,
            class_name=&quot;dosage&quot;
        ),
        lx.data.Extraction(
            text=&quot;每日一次&quot;,
            class_name=&quot;frequency&quot;
        ),
        lx.data.Extraction(
            text=&quot;3个月&quot;,
            class_name=&quot;duration&quot;
        )
    ]
)

# 提供1-3个这样的示例,模型就能学会输出格式!
```

+   能力三:精确的来源定位(Source Grounding)

这是 `LangExtract` 最突出的特性之一。每个提取结果都会自动标注其在原文中的字符偏移位置。

![](img/e677fa5b1c5018f098ccb363586d300c.png)

这主要可以用于审查场景，例如：

+   **医疗审计**:监管部门需要核查AI提取的诊断结论是否真的出现在病历中

+   **法律合规**:合规审查时必须能追溯每条规定来自合同的哪一页哪一行

+   **知识管理**:构建企业知识库时,需要链接回原始文档以供查证

+   能力四:结构化输出保证(Schema Enforcement)

直接使用大模型的最大的痛点之一是输出格式不可控。同一个任务,模型可能这次返回 JSON,下次返回表格,再下次又变成了自然语言描述。`LangExtract` 通过两种机制保证输出的结构化:

1. 基于示例的格式约束

通过 Few-Shot 示例,模型会学习并模仿示例的结构。这在所有模型上都有效。

2. 受控生成(Controlled Generation)

对于支持的模型(如 Google Gemini),`LangExtract` 会利用模型原生的 schema 约束功能,强制要求输出必须符合预定义的 JSON Schema。在实际应用中,可以批量处理成千上万份文档,无需担心某一份的输出格式突然出错导致整个流程崩溃。

+   能力五:长文档多轮提取(Multi-Pass Extraction)

大模型都有上下文长度限制。即使是支持 100K tokens 的模型,面对一份 500 页的法律合同也会捉襟见肘。更重要的是,长文档中的信息密度高,单次扫描很容易遗漏细节。`LangExtract` 采用了智能的处理策略:

1. 自动分块(Automatic Chunking)

+   将长文档切分为适合模型处理的小块

+   保留块与块之间的上下文重叠,避免信息断层

+   并行处理多个分块,提高速度

2. 多轮提取(Multi-Pass)

+   第一轮:全面扫描,提取明显的信息

+   第二轮:针对第一轮可能遗漏的内容,调整 prompt 再次扫描

+   第三轮及以后:可以针对特定类型的信息进行深度挖掘

这种策略在合规审查、法律分析等不允许遗漏任何细节的场景中至关重要。

## 四、【实战】Prompt Engineering 对比 `LangExtract`

上面我们已经了解了 `LangExtract` 的基本原理和核心能力。但在实际项目中，可能大部分同学都会有疑问："我直接用提示词让大模型提取不就行了吗？为什么还要用 `LangExtract`？"所以，我们就先通过一个真实的新闻信息提取场景，先运行纯提示词方式，看到它的提取结果，再运行 `LangExtract` 方式，看到它的提取结果，最后进行深度对比分析（重复实体、完整性、原文对齐能力）。

首先需要安装`langextract`库：

```
%pip install langextract
```
