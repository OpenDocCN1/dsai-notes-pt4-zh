# LangChain1.0 + OCR 多模态PDF解析实战

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Gf35dLGXFo6uhsxv08ic2ryRnif](https://kq4b3vgg5b.feishu.cn/docx/Gf35dLGXFo6uhsxv08ic2ryRnif)

## 课程说明：

+   体验课内容节选自《2025大模型Agent智能体开发实战》(11月班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(11月班)

![](img/0558c8786506b3ce440e25d18c49ec67.png)

![](img/c496c297dec93163dfd1a5683bf368d4.png)

通过百度网盘分享的文件链接：https://pan.baidu.com/s/1ZZvTCUU999XvZvqMjdTcJw?pwd=816w ，更多学习需求可扫码添加助教老师咨询详情～

---

# 《大模型Agent开发实战》（体验课）

## LangChain1.0 + OCR 多模态PDF解析实战

### MinerU & Paddle-OCR & DeepSeek-OCR 深度集成

**本期公开课，我们将从零开始，手把手带大家将目前行业内性能最强的OCR解析项目：MinerU、DeepSee**k-OCR和PaddleOCR-VL通过vLLM推理框架进行高性能部署，并实现具有统一解析服务接口的多模态数据分析系统。

![](img/b91bde4e8a9a94519f289c97e13c9b08.png)

核心功能一：源码部署 MinerU，使用 vLLM 启动推理服务，并以 MCP Server方式与LagnChain完成集成，支持批量解析多模态PDF、图片格式文件；

![](img/cef4d5aefcea589c4beb0000c686874d.png)

核心功能二：使用 vLLM 推理框架部署启动 DeepSeek-OCR 解析服务接口，并支持批量解析PDF和图像文件格式；

![](img/e2bb39519935b08dbb710aa1ed3ebc16.png)

核心功能三：本地部署 PaddleOCR-VL ，并通过 PaddleOCR CLI 工具挂载 vLLM 推理服务，提供极高性能的OCR解析服务，支持批量解析PDF和图像文件格式；

![](img/1807903fc4a7c28b5effecb13c372815.png)

该项目包含我们针对DeepSeek-OCR、MinerU等服务接口的独家自研代码程序，及完整的本地部署流程。同时，我们也将提供全部的前后端源码供大家学习和使用，是真正可以直接在企业中落地的项目。

# 一、PDF文件难点与解析方法总览

目前，无论是做Agent智能体应用，还是专注于做RAG知识库问答，PDF格式文件都是最难处理的文件类型，没有之一。主要原因在于PDF格式文件的结构非常复杂，包含文本、图像、表格等多种类型的数据，很多上下文信息都是通过标题、图片、图表和格式等来传达的，我们需要保留这些信息并进行有效的存储，才能够保证在应用阶段正确识别出有效的文本，从而使大模型能够更好地确定如何“思考”给定内容中提供的信息。

我们这里以RAG应用方向为例。

RAG系统往往是需要依靠高质量的结构化数据来生成准确且与文本相关的输出。PDF 通常用于官方文件、业务报告和法律合同，包含大量信息，但其布局复杂且数据结构不合理。如果做不到精确的 PDF 解析，关键数据就会丢失，从而导致结果不准确并直接影响 RAG 应用程序的有效性。因此，PDF格式文件的索引和检索的流程，在RAG框架中的实现过程如下图所示：

![](img/4f0a6dff08ceedeba60b063941290a01.png)

我们在实际构建RAG的第一步，往往是先做PDF Parsing，即PDF解析。所谓的PDF Parsing，指的是提取 PDF 文件中的内容并将其转换为结构化格式的过程，对于PDF格式的这种文件特点，就不能像处理.txt、.csv、.json等文件那样，直接使用Python的open函数打开文件，然后逐行读取文件内容，然后进行处理。它涉及分析 PDF 文件的结构和内容以提取有意义的信息，即把PDF文件中的文本、图像、表格和元数据等正确的识别出来，同时还需要解析出PDF文件的结构信息，比如：页眉、页脚、页码、章节、段落、标题等。

在RAG处理PDF格式文件的解析过程中，将其先转换为Markdown文件再做后处理是目前通用的做法。 Markdown 格式文件自2023年起就一直是在大模型领域的最流行格式，像ChatGPT、DeepSeek等聊天机器人格式化其响应的方式都是使用的Markdown语法，包括我们课程中的所有项目案例Fufan_chat、MateGen Pro和AssistGen，采取的做法都是后端实现流式输出，通过SSE协议将结果返回给前端，前端再通过Markdown语法进行展示。如下所示：DeepSeek的响应会以大而粗的字体呈现标题，以及通过使用粗体文本表示关键字。

![](img/76e79b688680a15039fbfb58aee9129f.png)

如下是 Markdown 语法的一些基本示例：

```

        # 一级标题
        ## 二级标题
        ### 三级标题

        **加粗文本**

        *斜体文本*

        &gt; 引用文本

        [链接文本](https://www.example.org)
```

```
    代码文本
    ```

    | 表头1   | 表头2   |
    |----------|----------|
    | 表格数据 | 表格数据 |

![](img/109a2b91d5d06e6534bfc1c9b4a36c73.png)


![](img/551812159273f0ab7060ea948da23d30.png)


![](img/28151eb316706310434f3f88c63c9170.png)


![](img/8f6a80a7721cc1e903745655c25d4702.png)


![](img/4d45df48ee7071a5cc2ea8a37c6bd2ad.png)


![](img/24a113db629d00577420ac9667f5f598.png)

![](img/063e0aba40146cb695868f559f957cec.png)


![](img/82560304f1315ba63e59e5ad3e088fe5.png)


![](img/6720b3c2596afe2e2958a168fab8c593.png)


![](img/b6a41de3826e81057974dbd4783b9ba2.png)


![](img/b534d8530e599b80a984fa3014d1c8f5.png)


![](img/3bd588f28265bbc5c0ef302619828353.png)


![](img/18539db31ac78c56cc893300fb285cb6.png)


![](img/295d7f51905c9c8040efb6ff00bf41ff.png)


![](img/59e0259374223cdb6ebc804fbb2a1530.png)


![](img/f2cb3df9084708a9c2fd3e053bfe510b.png)


![](img/7acbfc81dc9ffea59630417f109e213b.png)


![](img/0f22531c951210e3be87797dc495dc8e.png)


![](img/17daec1bbdd730bf285a1c3c8fb5876e.png)


![](img/a70c3ffb9c40f7a2b40295562422557c.png)


![](img/9e8f29565dc97a1594dc83f9c0e4411a.png)


![](img/4c712c5b68870a88bbea71b2cdbd542b.png)


![](img/0be75b2093edad348a1ea6493dd826ff.png)


![](img/3c79bab8e239f1293e8a845eed54cb4b.png)


![](img/d5d6435acbcc02b14fcfbebe8a967a83.png)

```

这种结构优势很明显，比如能给标题、表格、列表、链接等提供结构化的信息，添加了印刷强调元素，例如粗体或斜体，还能提供代码块、数学公式、图片、图表等，即易于编写，又易于阅读，即可以保留文档的原始结构，这包括保留布局、顺序以及不同部分（例如页眉、脚注、表格）之间的连接。对大模型来说，阅读和理解这种类型的输入文档上下文中是非常有效的。也正是因为Markdown格式文件的这种优势，所以现在主流的处理PDF格式文件的库、框架、工具，都是优先针对PDF --> Markdown 提取策略展开研究和优化。

+   **OCR技术兴起的背景**

也正是随着这种多模态数据难处理的困境，OCR 技术也迎来了新的快速发展机遇。2025年10月16日发布的 PaddleOCR-VL 模型直接屠榜，在全球权威榜单OmniDocBench V1.5中以92.6分夺得综合性能第一，横扫文本识别、公式识别、表格理解与阅读顺序四项SOTA。
而紧接其后，DeepSeek 也于 2025年10月21日发布了 DeepSeek-OCR 模型，仅需7G的显存，就能完成高精度的表格、公式识别，图片语义识别，并且在多项评测指标中一举拿下SOTA成绩。
这类模型的兴起，完全是源于真实应用需求的驱动。

OCR（全称：Optical Character Recognition，光学字符识别）是将包含文本的图像（如扫描文档、照片、表单、书页）转换成 机器可读的文本格式的技术，如下图所示：
这类模型并不是传统的OCR模型，而是拥有大模型多模态能力的OCR模型，这种模型被称为VLM（Vision-Language Model）模型、或者OCR 2.0模型。其中最本质的区别，就是多模态大模型在进行图像识别之前，会借助一个名叫视觉编码器的算法，将图像视觉信息映射到文本空间中，然后再借助大模型对文本语义的理解能力，间接的去理解图像信息。

简单理解就是：你有一张扫描的图片，上面写满了打印或手写的文字，OCR 技术可以把这些“文字图像”变成“真实的文本数据”（可以编辑、搜查、分析）而不是只是“一个图片”而已。

目前企业在做的大部分业务场景都是以正确识别不同文档类型的内容为前提，比如处理大量纸质文档（发票、合同、报表、表单、收据等），需要通过 OCR 可以把这些纸质/图像文档转换为电子数据，从而更便于存储、检索、分析。

在数字化、智能化流程（例如自动化数据输入、档案管理、人工智能分析）中，它同样也是一个基础环节。

# 二、企业级OCR项目适用场景及优劣势分析

就目前的发展来看，OCR 与 RAG 的技术结合并不像简单的传统 OCR 是“你给我一张图像，我把文字提取出来”就可以了，重点不能仅仅放在去提取文字，而是要在 OCR 的基础上，不仅仅是“图像中的文字”被识别，还需要结合 视觉（图像）、语言（文本）、版面结构（布局）、场景环境/上下文 等多个模态（modalities）一起分析，使得识别和理解更强、更智能。
现代文档不仅仅是文本。它们包含多列布局、数学公式、半扫描表格、多语言文本以及分辨率奇数的图表。像 GPT-4o 或 Qwen-VL 这样的端到端模型可以解析它们，但它们速度慢、布局混乱，并且耗费 GPU 内存。所以企业环境下往往会选择更小、更紧凑的视觉模型来为解析工作提供支持。主流的企业级OCR项目应用如下：

+   MinerU:[点击进入](https://github.com/opendatalab/MinerU)

MinerU 是由 OpenDataLab（上海人工智能实验室下属团队）发起的一个开源工具，目标是将 PDF（含扫描件、复杂版式、多栏、多表格、多公式）转换为可机读的结构化格式（如 Markdown、JSON）以便进一步下游使用。项目的定位更偏「文档内容抽取／结构化」而不仅仅是传统 OCR。其取向是“将 PDF → Markdown/JSON”这一流程，而不仅“图片 → 文字”。
MinerU的主要工作流程分为以下几个阶段：

1.  **输入**：接收PDF 格式文本，可以是简单的纯文本，也可以是包含双列文本、公式、表格或图等多模态PDF文件;

1.  文档预处理（Document Preprocessing）：检查语言、页面大小、文件是否被扫描以及加密状态；

1.  内容解析（Content Parsing）：

+   局分析：区分文本、表格和图像。

+   公式检测和识别：识别公式类型（内联、显示或忽略）并将其转换为 LaTeX 格式。

+   表格识别：以 HTML/LaTeX 格式输出表格。

+   OCR：对扫描的 PDF 执行文本识别。

1.  内容后处理（Content Post-processing）：修复文档解析后可能出现的问题。比如解决文本、图像、表格和公式块之间的重叠，并根据人类阅读模式重新排序内容，确保最终输出遵循自然的阅读顺序。

1.  格式转换（Format Conversion）：以 Markdown 或 JSON 格式生成输出。

1.  **输出（Output）**：高质量、结构良好的解析文档。

如下是MinerU项目官方给出的配置参考：
而VLM-Sglang后端则是利用sglang高性能推理引擎，优化了GPU加速及分布式部署的基础上同时支持实时流式输出，其流程如下：
MinerU 项目非常适用于：

+   需要从大量 PDF 文档中抽取结构化内容（例如学术文献、技术白皮书、报告）用于知识库或训练语料。

+   对版式结构（如章节、列表、表格、公式）要求较高，而不只是 OCR 文本识别。

+   希望输出 Markdown／JSON 供后续自动化流水线使用。

+   PaddleOCR：[点击进入](https://github.com/PaddlePaddle/PaddleOCR/tree/main)

PaddleOCR 是由 Baidu (及其生态) 基于其深度学习框架 PaddlePaddle 提供的开源 OCR 工具箱。支持从 PDF 或图像文档转为结构化数据（适配 AI 场景），支持 100+ 语言。最新版本 3.0 在其技术报告中提出：PP-OCRv5、PP-StructureV3、PP-ChatOCRv4 三大解决方案，覆盖文字识别、多版式文档解析、关键 信息提取。
在早期版本（如 PP-OCRv3）中，其结构可概括为：“检测 (Detection) → 分类 (Classification of orientation) → 识别 (Recognition)”。使用多种模型，例如检测模型（DBNet 等）、识别模型（如 SVTR），在 3.0 版本中，其“PP-StructureV3”整合了布局分析、表格识别、结构抽取。同时还最新推出了还推出了PaddleOCR-VL 的 Vision-Language 模型版本（0.9B 参数的 VLM），用于多语种文档解析。

PaddleOCR-VL 是推出的一个专注于“文档解析／视觉-语言模型 (Vision-Language Model, VLM)”功能的新模块，采用了视觉-语言模型架构以应对更高阶的需求。在解析多模态数据方面，PaddleOCR将这项工作分为两部分：

1.  首先检测并排序布局元素。

1.  使用紧凑的视觉语言模型精确识别每个元素。

该系统分为两个明确的阶段运行。

第一阶段是执行布局分析（PP-DocLayoutV2），此部分标识文本块、表格、公式和图表。它使用：

+   RT-DETR 用于物体检测（基本上是边界框 + 类标签）。

+   指针网络 （6 个转换器层）可确定元素的读取顺序 ，从上到下、从左到右等。

最终输出统一模式的图片标注数据，如下图所示：
第二阶段则是元素识别（PaddleOCR-VL-0.9B），这就是视觉语言模型发挥作用的地方。它使用：

+   NaViT 风格编码器 （来自 Keye-VL），可处理动态图像分辨率。无平铺，无拉伸。

+   一个简单的 2 层 MLP， 用于将视觉特征与语言空间对齐。

+   ERNIE-4.5–0.3B 作为语言模型，该模型规模虽小但速度很快，并且采用 3D-RoPE 进行位置编码

最终模型输出结构化 Markdown 或 JSON 格式的文件用于后续的处理。
这个小小的设计决策， 将布局和识别分离，使得 PaddleOCR-VL 比通常的一体化系统更快、更稳定。同时根据实际的测试，其运行和解析速度也更快。在 A100 GPU 上， 吞吐量为 1.22 页/秒，。比 MinerU2.5 快 15.8%， VRAM 比 dots.ocr 少约 40%。

+   DeepSeek-OCR：[点击进入](https://github.com/deepseek-ai/DeepSeek-OCR)

DeepSeek 于 2025年10月21日发布了 DeepSeek-OCR 模型，仅需7G的显存，就能完成高精度的表格、公式识别，图片语义识别，并且在多项评测指标中一举拿下SOTA成绩。其 Github 相较于其他项目相对比较"简陋"，仅仅提供了Transformers 和 vLLM 启动 DeepSeek-OCR的服务示例代码文件：
+   dots.ocr：[点击进入](https://github.com/dots-ai/dots.ocr)

dots.ocr 是由 rednote‑hilab（HiLab团队）开源的多语种文档布局解析工具。官方介绍中强调：“一个统一的 Vision-Language 模型（≈1.7 B 参数）即可完成布局检测 + 内容识别 +阅读顺序排序”。支持文本、表格、公式、以及多语言输入。
dots.ocr 的特点是用一个 VLM（1.7 B 参数）来统一布局解析+内容识别，而不是传统将检测、识别、结构分开。用户可通过不同 prompt 来切换任务（如“请输出版式元素的 bbox、类别、文本”）→ 即说明模型采用 prompt + VLM 的方式。
非常适合需要快速处理多语种、混版式文档，且希望用一个统一模型／prompt 来搞定。虽然表现不错，但对于极复杂的表格（如跨页表、合并单元格）或特殊版式效果并不是很理想。

# 三、MinerU 项目概览与vllm 服务接口

MinerU是一个非常典型的基于管道的解决方案 (Pipeline-based solution) ，并且是一个开源文档解析项目，一共四个核心组件，通过Pipeline的设计无缝衔接，实现比较高效、准确的文档解析。如下图所示：（下图来源官方论文：https://arxiv.org/pdf/2409.18839v1）
MinerU的主要工作流程分为以下几个阶段：

1.  **输入**：接收PDF 格式文本，可以是简单的纯文本，也可以是包含双列文本、公式、表格或图等多模态PDF文件;

1.  文档预处理（Document Preprocessing）：检查语言、页面大小、文件是否被扫描以及加密状态；

1.  内容解析（Content Parsing）：

+   局分析：区分文本、表格和图像。

+   公式检测和识别：识别公式类型（内联、显示或忽略）并将其转换为 LaTeX 格式。

+   表格识别：以 HTML/LaTeX 格式输出表格。

+   OCR：对扫描的 PDF 执行文本识别。

1.  内容后处理（Content Post-processing）：修复文档解析后可能出现的问题。比如解决文本、图像、表格和公式块之间的重叠，并根据人类阅读模式重新排序内容，确保最终输出遵循自然的阅读顺序。

1.  格式转换（Format Conversion）：以 Markdown 或 JSON 格式生成输出。

1.  **输出（Output）**：高质量、结构良好的解析文档。

目前在Github 上，MinerU 的Star 数为48.2K，Fork 数为4K，拥有非常良好的社区支持和活跃的贡献者，且一直处于active development状态。
MinerU 提供了在线Demo 页面，我们可以直接线进行测试。试用地址：https://opendatalab.com/OpenSourceTools/Extractor/PDF/
同时，MinerU 项目于2024年07月05日首次开源，底层主要是集成 PDF-Extract-Kit 开源项目做PDF的内容提取，PDF-Extract-Kit同样是一个开源项目：https://github.com/opendatalab/PDF-Extract-Kit
PDF-Extract-Kit这个项目主要针对的是PDF文档的内容提取，通过集成众多SOTA模型对PDF文件实现高质量的内容提取，其中应用到的模型主要包括：

PDF-Extract-Kit 应用的模型类型

| 模型类型 | 模型名称 | GitHub 链接 | 模型下载链接 | 任务描述 |
| --- | --- | --- | --- | --- |
| 布局检测模型 | LayoutLMv3 / DocLayout-YOLO | [GitHub](https://github.com/microsoft/unilm/tree/master/layoutlmv3) / [GitHub](https://github.com/opendatalab/DocLayout-YOLO) | [模型下载](https://huggingface.co/microsoft/layoutlmv3-base-chinese) / [模型下载](https://huggingface.co/juliozhao/DocLayout-YOLO-DocStructBench/tree/main) | 定位文档中不同元素位置：包含图像、表格、文本、标题、公式等 |
| 公式检测模型 | YOLO | [GitHub](https://github.com/ultralytics/ultralytics) | [模型下载](https://github.com/ultralytics/assets/releases/download/v8.3.0/yolo11n.pt) | 定位文档中公式位置：包含行内公式和行间公式 |
| 公式识别模型 | UniMERNet | [GitHub](https://github.com/opendatalab/UniMERNet) | [模型下载](https://huggingface.co/wanderkid/unimernet_base) | 识别公式图像为latex源码 |
| 表格识别模型 | StructEqTable | [GitHub](https://github.com/Alpha-Innovator/StructEqTable-Deploy?tab=readme-ov-file) | [模型下载](https://huggingface.co/U4R/StructTable-InternVL2-1B/tree/main) | 识别表格图像为对应源码（Latex/HTML/Markdown） |
| OCR模型 | PaddleOCR | [GitHub](https://github.com/PaddlePaddle/PaddleOCR) | [模型下载](https://paddlepaddle.github.io/PaddleX/latest/module_usage/tutorials/ocr_modules/text_detection.html) | 提取图像中的文本内容（包括定位和识别） |
|  |  |  |  |  |

MinerU与PDF-Extract-Kit的关系是：MinerU 结合PDF-Extract-Kit输出的高质量预测结果，进行了专门的工程优化，使得文档内容提取更加便捷高效，处理底层原理的优化细节外，主要提升点在以下几点：

+   加入了自研的doclayout_yolo(2501)模型做布局检测，在相近解析效果情况下比原方案提速10倍以上，可以通过配置文件与 layoutlmv3 自由切换使用；

+   加入了自研的unimernet(2501) 模型做公式识别，针对真实场景下多样性公式识别的算法，可以对复杂长公式、手写公式、含噪声的截图公式均有不错的识别效果；

+   增加 OCR 的多语言支持，支持 84 种语言的检测与识别，支持列表：https://paddlepaddle.github.io/PaddleOCR/latest/ppocr/blog/multi_languages.html#5

+   重构排序模块代码，使用 layoutreader 进行阅读顺序排序，确保在各种排版下都能实现极高准确率 : https://github.com/ppaanngggg/layoutreader

+   表格识别功能接入了StructTable-InternVL2-1B模型，大幅提升表格识别效果，模型下载地址:https://huggingface.co/U4R/StructTable-InternVL2-1B

在部署和使用方面，MinerU 支持Linux、Windows、MacOS 多平台部署的本地部署，并且其中用的到布局识别模型、OCR 模型、公式识别模型、表格识别模型都是开源的，我们可以直接下载到本地进行使用。而且，MinerU 项目是完全支持华为昇腾系列芯片的，可适用性非常广且符合国内用户的使用习惯。

因此，接下来我们将重点介绍如何在Linux 系统下部署MinerU 项目并[进行PDF 文档解](https://github.com/opendatalab/MinerU/blob/master/README_zh-CN.md#%E4%BD%BF%E7%94%A8mps)析流程实战。注意：这里强烈建议大家使用Linux 系统进行部署，其支持性和兼容性远高于Windows 系统。如果大家想选择其他操作系统，可以参考如下链接进行自行实践：Windows 10/11 + GPU ，Docker 部署 ,

## 3.1 MinerU 源码安装

针对MinerU这个项目，其提供了 MCP 服务的配置文件，但是存在很多BUG, 我们必须手动修改相关的源码才能适配如LangChain等框架中，所以这里我们建议大家采用Linux系统下的源码安装方式。如下是MinerU项目官方给出的配置参考：
MinerU 实现了三个不同的处理后端，每个后端针对不同的用例和硬件配置进行了优化。其中Pipeline后端是默认的，也是最通用的后端，其采用传统的机器学习流程：

1.  **输入**：接收PDF 格式文本，可以是简单的纯文本，也可以是包含双列文本、公式、表格或图等多模态PDF文件;

1.  文档预处理（Document Preprocessing）：检查语言、页面大小、文件是否被扫描以及加密状态；

1.  内容解析（Content Parsing）：

+   局分析：区分文本、表格和图像。

+   公式检测和识别：识别公式类型（内联、显示或忽略）并将其转换为 LaTeX 格式。

+   表格识别：以 HTML/LaTeX 格式输出表格。

+   OCR：对扫描的 PDF 执行文本识别。

1.  内容后处理（Content Post-processing）：修复文档解析后可能出现的问题。比如解决文本、图像、表格和公式块之间的重叠，并根据人类阅读模式重新排序内容，确保最终输出遵循自然的阅读顺序。

1.  格式转换（Format Conversion）：以 Markdown 或 JSON 格式生成输出。

1.  **输出（Output）**：高质量、结构良好的解析文档。
VLM-Transformer则是直接利用transformers库中的Vision-Language模型处理图像+文本的多模态输入，其流程如下：
而VLM-Sglang后端则是利用sglang高性能推理引擎，优化了GPU加速及分布式部署的基础上同时支持实时流式输出，其流程如下：
各个后端的默认模型配置为：

MinerU 后端默认模型配置

| 后端类型 | 默认模型 | 适用场景 | 资源需求 |
| --- | --- | --- | --- |
| **VLM-Transformer** | MinerU2.5-2509-1.2B | 高精度解析 | 中等 GPU |
| **VLM-SGLang** | MinerU2.5-2509-1.2B | 高速推理 | 高端 GPU |
| **Pipeline** | PDF-Extract-Kit-1.0 | 轻量部署 | CPU 友好 |
|  |  |  |  |

MinerU 的 VLM 后端默认使用的是自研的 MinerU2.5-2509-1.2B 模型，这是一个基于 Qwen 架构、专门为文档解析优化的 1.2B 参数多模态模型。而PDF-Extract-Kit-1.0，则是沿用了旧版本的基于 YOLO、OCR、表格识别等传统独立 CV 子模型的模块化架构。

MinerU 的新版本不再严格限制python 3.10版本，但建议大家使用python 3.11或者python 3.12，python 3.13可能存在兼容问题。

+   **Step 1. 确认系统版本**

我们使用的是Ubuntu 22.04 系统，可以通过cat /etc/os-release 命令查看系统版本，如下图所示：
+   **Step 2. 确认****CUDA**** 版本**

在Linux 系统下，可以通过nvidia-smi 命令查看CUDA 版本，这里的服务器配置是四卡的RTX 3090 显卡，如下图所示：
CUDA Version 显示的版本号必须 >= 12.1，如显示的版本号小于12.1，需要自行升级CUDA 版本。

+   **Step 3. 确认 ****Conda**** 版本**

我们使用的是Anaconda 安装的Conda 环境，可以通过conda --version 命令查看Conda 版本，如下图所示：
如果出现Conda not found 等报错，需要先安装Conda 环境，再执行接下来的步骤。

+   **Step 4. 使用****Conda**** 创建****Python 3.11**** 版本的虚拟环境**

执行如下命令创建一个全新的虚拟环境

```
    conda create --name mineru_2.5 python==3.11 -y

![](img/a86464eb8cebe875f85a4e26c2698b1f.png)


![](img/65cf4a2f5210a3af9d8fd7b249cd4c66.png)


![](img/90645a2f875cc9b81f8ce66018922eeb.png)


![](img/7ceb69f939337424ca4d01f61a8789e2.png)

![](img/1ca6be38411a109361e237e5cb586c7e.png)

```
+   **Step 5. 激活虚拟环境**

创建完虚拟环境后，使用Conda 激活虚拟环境，通过conda activate mineru_2.5 命令激活，如下图所示：
+   Step 6. 下载MinerU源码文件

截止目前最新的MinerU项目源码版本为是：2.6.4-released，但当前版本的源码在使用MCP Server时存在一些Bug，所以我们针对源码做了一些修改。因此大家一定要下载我们课程网盘中的源码文件进行部署和使用。下载后上传至服务器中，解压文件，并进入解压后的文件夹，如下图所示：
注意，大家这里一定要使用我们课程提供的源码版本，因为我们对源码进行了非常多的修改才能适配MCP服务下使用MinerU vllm解析服务。
+   **Step 7. 安装****MinerU**** 项目依赖**

使用Conda 安装MinerU 项目依赖，需要通过如下命令在新建的mineru 虚拟环境中安装运行MinerU 项目程序的所有依赖，命令如下：

```
    pip install -e .[all] -i https://mirrors.aliyun.com/pypi/simple

![](img/4e5473a05b5d7b35f81a43857cc715fb.png)


![](img/5496d10ef5a217452e02e391fbabd2c4.png)

```
通过pip show mineru 命令查看MinerU 项目的版本：
至此，基础的MinerU 项目依赖就安装完成了，接下来我们需要下载MinerU 项目中用到的模型文件，并进行项目配置。

+   **Step 8. 下载****MinerU**** 项目中用到的模型文件**

新版本的MinerU 项目提供了一键下载所有模型文件的脚本，我们只需要执行如下命令即可：

```
    mineru-models-download

![](img/eb6099ba742fb0eef1ecb988429daf5b.png)

```

执行该命令需要我们根据自己的需求灵活的选择要下载到本地的模型。其中modelscope 和 huggingface 是两个不同的模型下载源，我们只需要选择其中一个即可。(注意：国内用户建议使用modelscope 下载模型文件)。其次，pipeline指的是用于文档解析的一系列模型，而vlm指的是用于视觉语言模型的模型，即MinerU2.0-2505-0.9B 模型。如果是all，则会全部下载。
等待下载完成后，所有模型文件的默认存储路径是：

```
    /root/.cache/modelscope/hub/models/OpenDataLab

![](img/1766f5f4afd3355f962b174cdeabfb5e.png)


![](img/d9f8a5d8d02c9bec8c9cfa34fec7510c.png)


![](img/84297eb8a38c5476cef86ee64ce0c19e.png)

![](img/bda91e5308e466f3b6c560a2e8cca877.png)

```
并且，也会自动在/root/mineru.json 文件中配置用于后续使用的模型路径。
至此，MinerU 项目的本地配置就全部完成了，接下来我们可以尝试运行MinerU 项目并进行PDF 文档解析测试。
## 3.2 MinerU 启动vLLM API 推理服务

接下来新打开一个终端，启动mineru-api 服务：

```
    export MINERU_MODEL_SOURCE=local  # 注意：这里需要将模型源设置为本地
    export CUDA_VISIBLE_DEVICES=3  # 使用 哪一块GPU
    mineru-api --host 192.168.110.131 --port 50000

![](img/b1650fb9cca93913674b990af1a36394.png)


![](img/046c23f9aaf7c5fa16f225aa92254270.png)

```
启动后，在192.168.110.131:50000/docs中可以看到接口服务：
## 3.3 MinerU vLLM API服务连接测试

```
#!/usr/bin/env python3
&quot;&quot;&quot;
测试 MinerU API 的不同 backend
通过 50000 端口调用，传递不同的 backend 参数来使用不同的模型
&quot;&quot;&quot;

import requests
import sys
from pathlib import Path

def test_mineru_api(pdf_path: str, backend: str = &quot;pipeline&quot;):
    &quot;&quot;&quot;
    测试 MinerU API

    Args:
        pdf_path: PDF 文件路径
        backend: 后端类型，可选值：
                - &quot;pipeline&quot; (默认，使用本地 PyTorch)
                - &quot;vlm-vllm-async-engine&quot; (使用 vLLM 加速)
    &quot;&quot;&quot;
    api_url = &quot;http://192.168.130.4:50000/file_parse&quot;

    print(f&quot;\n{&#x27;=&#x27;*60}&quot;)
    print(f&quot;测试 MinerU API with backend: {backend}&quot;)
    print(f&quot;{&#x27;=&#x27;*60}&quot;)
    print(f&quot;API URL: {api_url}&quot;)
    print(f&quot;PDF 文件: {pdf_path}&quot;)
    print(f&quot;Backend: {backend}&quot;)
    print()

    # 检查文件是否存在
    if not Path(pdf_path).exists():
        print(f&quot;错误: 文件不存在 - {pdf_path}&quot;)
        return None

    try:
        # 打开文件并发送请求
        with open(pdf_path, &#x27;rb&#x27;) as f:
            files = [(&#x27;files&#x27;, (Path(pdf_path).name, f, &#x27;application/pdf&#x27;))]
            data = {
                &#x27;backend&#x27;: backend,
                &#x27;parse_method&#x27;: &#x27;auto&#x27;,
                &#x27;lang_list&#x27;: &#x27;ch&#x27;,
                &#x27;return_md&#x27;: &#x27;true&#x27;,
                &#x27;return_middle_json&#x27;: &#x27;false&#x27;,
                &#x27;return_model_output&#x27;: &#x27;false&#x27;,
                &#x27;return_content_list&#x27;: &#x27;false&#x27;,
                &#x27;start_page_id&#x27;: &#x27;0&#x27;,
                &#x27;end_page_id&#x27;: &#x27;1&#x27;,  # 只处理前2页，快速测试
            }

            print(&quot;发送请求...&quot;)
            response = requests.post(
                api_url,
                files=files,
                data=data,
                timeout=300
            )

        # 检查响应
        if response.status_code != 200:
            print(f&quot;请求失败: HTTP {response.status_code}&quot;)
            print(f&quot;响应: {response.text[:500]}&quot;)
            return None

        # 解析 JSON 响应
        result = response.json()

        # 提取信息
        backend_used = result.get(&#x27;backend&#x27;, &#x27;unknown&#x27;)
        version = result.get(&#x27;version&#x27;, &#x27;unknown&#x27;)
        results = result.get(&#x27;results&#x27;, {})

        print(f&quot;请求成功!&quot;)
        print(f&quot;   使用的 backend: {backend_used}&quot;)
        print(f&quot;   版本: {version}&quot;)
        print(f&quot;   结果数量: {len(results)}&quot;)

        # 提取 markdown 内容
        if results:
            file_key = list(results.keys())[0]
            md_content = results[file_key].get(&#x27;md_content&#x27;, &#x27;&#x27;)

            print(f&quot;\nMarkdown 内容预览 (前500字符):&quot;)
            print(&quot;-&quot; * 60)
            print(md_content[:500])
            print(&quot;-&quot; * 60)

            # 保存 markdown 到文件
            output_file = f&quot;output_{backend}.md&quot;
            with open(output_file, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as f:
                f.write(md_content)
            print(f&quot;\n完整 Markdown 已保存到: {output_file}&quot;)

            return md_content
        else:
            print(&quot;未找到结果&quot;)
            return None

    except Exception as e:
        print(f&quot;测试失败: {e}&quot;)
        import traceback
        traceback.print_exc()
        return None
```

```
def main():
    # 默认测试文件
    pdf_path = &quot;./2507.05595v1.pdf&quot;


    print(f&quot;\n开始测试 MinerU API 的不同 backend&quot;)
    print(f&quot;测试文件: {pdf_path}\n&quot;)

    # 测试 1: pipeline backend (本地 PyTorch)
    print(&quot;\n&quot; + &quot;=&quot;*60)
    print(&quot;测试 1: pipeline backend (本地 PyTorch)&quot;)
    print(&quot;=&quot;*60)
    result_pipeline = test_mineru_api(pdf_path, backend=&quot;pipeline&quot;)

    # 测试 2: vLLM backend (vLLM 加速)
    print(&quot;\n&quot; + &quot;=&quot;*60)
    print(&quot;测试 2: vlm-vllm-async-engine backend (vLLM 加速)&quot;)
    print(&quot;=&quot;*60)
    result_vllm = test_mineru_api(pdf_path, backend=&quot;vlm-vllm-async-engine&quot;)

    # 总结
    print(&quot;\n&quot; + &quot;=&quot;*60)
    print(&quot;测试总结&quot;)
    print(&quot;=&quot;*60)
    print(f&quot;Pipeline backend: {&#x27;成功&#x27; if result_pipeline else &#x27;失败&#x27;}&quot;)
    print(f&quot;vLLM backend: {&#x27;成功&#x27; if result_vllm else &#x27;失败&#x27;}&quot;)

    if result_pipeline and result_vllm:
        print(&quot;\n所有测试通过! MinerU 可以通过 backend 参数切换不同模型&quot;)

    print(&quot;\n💡 提示:&quot;)
    print(&quot;  - pipeline: 使用本地 PyTorch，适合调试&quot;)
    print(&quot;  - vlm-vllm-async-engine: 使用 vLLM 加速，速度更快&quot;)


if __name__ == &quot;__main__&quot;:
    main()
```

```
开始测试 MinerU API 的不同 backend
测试文件: ./2507.05595v1.pdf
```

============================================================

测试 1: pipeline backend (本地 PyTorch)

============================================================

```
============================================================
测试 MinerU API with backend: pipeline
============================================================
API URL: http://192.168.130.4:50000/file_parse
PDF 文件: ./2507.05595v1.pdf
Backend: pipeline

发送请求...
请求成功!
   使用的 backend: pipeline
   版本: 2.6.4
   结果数量: 1

Markdown 内容预览 (前500字符):
------------------------------------------------------------
# PaddleOCR 3.0 Technical Report

Cheng Cui, Ting Sun, Manhui Lin, Tingquan Gao, Yubo Zhang, Jiaxuan Liu, Xueqing Wang, Zelun Zhang, Changda Zhou, Hongen Liu, Yue Zhang, Wenyu Lv, Kui Huang, Yichao Zhang, Jing Zhang, Jun Zhang, Yi Liu, Dianhai Yu, Yanjun Ma

PaddlePaddle Team, Baidu Inc. paddleocr@baidu.com

Source Code: https://github.com/PaddlePaddle/PaddleOCR Document: https://paddlepaddle.github.io/PaddleOCR Models &amp; Online Demo: https://huggingface.co/PaddlePaddl

# Abstract

This technical
------------------------------------------------------------

完整 Markdown 已保存到: output_pipeline.md

============================================================
测试 2: vlm-vllm-async-engine backend (vLLM 加速)
============================================================

============================================================
测试 MinerU API with backend: vlm-vllm-async-engine
============================================================
API URL: http://192.168.130.4:50000/file_parse
PDF 文件: ./2507.05595v1.pdf
Backend: vlm-vllm-async-engine

发送请求...
请求成功!
   使用的 backend: vlm-vllm-async-engine
   版本: 2.6.4
   结果数量: 1

Markdown 内容预览 (前500字符):
------------------------------------------------------------
# PaddleOCR 3.0 Technical Report

Cheng Cui, Ting Sun, Manhui Lin, Tingquan Gao, Yubo Zhang, Jiaxuan Liu, Xueqing Wang, Zelun Zhang, Changda Zhou, Hongen Liu, Yue Zhang, Wenyu Lv, Kui Huang, Yichao Zhang, Jing Zhang, Jun Zhang, Yi Liu, Dianhai Yu, Yanjun Ma

PaddlePaddle Team, Baidu Inc.

paddleocr@baidu.com

Source Code: https://github.com/PaddlePaddle/PaddleOCR  
Document: https://paddlepaddle.github.io/PaddleOCR  
Models &amp; Online Demo: https://huggingface.co/PaddlePaddle

# Abstract

This tec
------------------------------------------------------------

完整 Markdown 已保存到: output_vlm-vllm-async-engine.md

============================================================
测试总结
============================================================
Pipeline backend: 成功
vLLM backend: 成功

所有测试通过! MinerU 可以通过 backend 参数切换不同模型

💡 提示:
  - pipeline: 使用本地 PyTorch，适合调试
  - vlm-vllm-async-engine: 使用 vLLM 加速，速度更快

![](img/b89742d8c6098dd56790b6577c48ce49.png)

```

当然，上述代码也是LangChain1.0 + OCR 多模态PDF解析实战项目中MinerU 的核心代码逻辑：
当然，MinerU 提供了命令行离线解析、Gradio/WebUI、独立 FastAPI 服务（mineru-api）等多种启动方式。不同方式适合的场景、端口、性能与依赖各不相同，如下所示：

MinerU 启动方式

| 启动方式 | 典型命令/镜像 | 端口 &amp; 路径 | 适用场景 | 依赖 |
| --- | --- | --- | --- | --- |
| **SGLang 后端服务** | mineru-sglang-server --host 0.0.0.0 --port 30000 | POST /file_parse | 高并发、GPU 推理、本地私有化 | Python + CUDA/SGLang |
| **Docker 一键** | docker run -p 30000:30000 opendatalab/mineru:latest mineru-sglang-server | 同上 | 免环境配置、快速试用 | Docker (GPU 可选) |
| **命令行离线解析** | mineru -p &lt;输入&gt; -o &lt;输出&gt; | 无服务；直接落盘 | 批处理、无网络、脚本化 | 纯 Python CLI |
| **Gradio WebUI** | mineru-gradio --enable-sglang-engine | 浏览器 UI | 交互演示、无代码使用 | Gradio 前端 |
| **FastAPI API 服 务** | git clone .../mineru-api &amp;&amp; docker compose up | PUT /api/parse | 需要依托自定义业务接口 | FastAPI + Uvicorn |
| **MCP-Server（外壳）** | uv run mineru-mcp --transport streamable-http | /mcp 或 /sse | 给 ADK/Agent 暴露工具 | fastmcp 框架 |
| n8n/Serverless 封装 | n8n「MCP Client」节点或 Runpod 镜像 | 由平台分配 | 无代码工作流 | 平台函数 |

实际上，MinerU 会输出非常丰富的信息，主要是：

1.  images:这是一个文件夹，里面存储了PDF 文档中出现的所有图片；

1.  .origin.pdf：进行解析的原始PDF文件；

1.  .markdown：PDF 文档的解析结果，输出为xxx.md 的 Markdown 格式；

1.  .layout.pdf：这个文件用不同的背景色块圈定不同的内容块，以此来区分整体的布局识别结果；

1.  .model.json：这个文件的主要作用是将 PDF 从像素级别的展示转换为结构化的数据，使计算机能够"理解"文档的组成部分。主要存储的信息是：

+   文档布局识别：存储文档中的各种元素（标题、正文、图表等）及其在页面上的精确位置

+   内容分类：将识别出的元素分类为不同类型（如标题、表格、公式等）

+   特殊内容解析：对于公式、表格等特殊内容，提供其结构化表示（如LaTeX、HTML格式）

其示例数据如下：

```
    [
        {
            &quot;layout_dets&quot;: [
                {
                    &quot;category_id&quot;: 2,
                    &quot;poly&quot;: [
                        99.1906967163086,
                        100.3119125366211,
                        730.3707885742188,
                        100.3119125366211,
                        730.3707885742188,
                        245.81326293945312,
                        99.1906967163086,
                        245.81326293945312
                    ],
                    &quot;score&quot;: 0.9999997615814209
                }
            ],
            &quot;page_info&quot;: {
                &quot;page_no&quot;: 0,
                &quot;height&quot;: 2339,
                &quot;width&quot;: 1654
            }
        },
        {
            &quot;layout_dets&quot;: [
                {
                    &quot;category_id&quot;: 5,
                    &quot;poly&quot;: [
                        99.13092803955078,
                        2210.680419921875,
                        497.3183898925781,
                        2210.680419921875,
                        497.3183898925781,
                        2264.78076171875,
                        99.13092803955078,
                        2264.78076171875
                    ],
                    &quot;score&quot;: 0.9999997019767761
                }
            ],
            &quot;page_info&quot;: {
                &quot;page_no&quot;: 1,
                &quot;height&quot;: 2339,
                &quot;width&quot;: 1654
            }
        }
    ]
```

其中每一个layout_dets 中存储了PDF 文档中每一页的布局识别结果，poly 是四边形坐标, 分别是 左上，右上，右下，左下 四点的坐标，形式为：[x0, y0, x1, y1, x2, y2, x3, y3], 分别表示左上、右上、右下、左下四点的坐标，page_info 是PDF 文档中每一页元数据，包含页码、高度、宽度信息，而category_id 为PDF 文档中每一页的布局识别结果的类型，分别为：

category_id 的类别

| 类别 ID | 类别名称 | 描述 |
| --- | --- | --- |
| 0 | title | 标题 |
| 1 | plain_text | 文本 |
| 2 | abandon | 包括页眉页脚页码和页面注释 |
| 3 | figure | 图片 |
| 4 | figure_caption | 图片描述 |
| 5 | table | 表格 |
| 6 | table_caption | 表格描述 |
| 7 | table_footnote | 表格注释 |
| 8 | isolate_formula | 行间公式 |
| 9 | formula_caption | 行间公式的标号 |
| 13 | embedding | 行内公式 |
| 14 | isolated | 行间公式 |
| 15 | text | OCR 识别结果 |

这个.json文件主要可以内容提取，比如精确提取特定类型的内容（如所有表格或公式），搜索，比如实现对文档内容的结构化搜索，文档分析，比如分析文档的结构特征（如有多少图表、公式密度等）等需求场景。

1.  _middle.json：这个文件的核心作用是存储 PDF 文档解析过程中多层次结构化数据，它比 model.json 提供了更详细的层次结构和内容信息, 其示例数据是这样的：

```
    {
        &quot;pdf_info&quot;: [
            {
                &quot;preproc_blocks&quot;: [
                    {
                        &quot;type&quot;: &quot;text&quot;,
                        &quot;bbox&quot;: [
                            52,
                            61.956024169921875,
                            294,
                            82.99800872802734
                        ],
                        &quot;lines&quot;: [
                            {
                                &quot;bbox&quot;: [
                                    52,
                                    61.956024169921875,
                                    294,
                                    72.0000228881836
                                ],
                                &quot;spans&quot;: [
                                    {
                                        &quot;bbox&quot;: [
                                            54.0,
                                            61.956024169921875,
                                            296.2261657714844,
                                            72.0000228881836
                                        ],
                                        &quot;content&quot;: &quot;dependent on the service headway and the reliability of the departure &quot;,
                                        &quot;type&quot;: &quot;text&quot;,
                                        &quot;score&quot;: 1.0
                                    }
                                ]
                            }
                        ]
                    }
                ],
                &quot;layout_bboxes&quot;: [
                    {
                        &quot;layout_bbox&quot;: [
                            52,
                            61,
                            294,
                            731
                        ],
                        &quot;layout_label&quot;: &quot;V&quot;,
                        &quot;sub_layout&quot;: []
                    }
                ],
                &quot;page_idx&quot;: 0,
                &quot;page_size&quot;: [
                    612.0,
                    792.0
                ],
                &quot;_layout_tree&quot;: [],
                &quot;images&quot;: [],
                &quot;tables&quot;: [],
                &quot;interline_equations&quot;: [],
                &quot;discarded_blocks&quot;: [],
                &quot;para_blocks&quot;: [
                    {
                        &quot;type&quot;: &quot;text&quot;,
                        &quot;bbox&quot;: [
                            52,
                            61.956024169921875,
                            294,
                            82.99800872802734
                        ],
                        &quot;lines&quot;: [
                            {
                                &quot;bbox&quot;: [
                                    52,
                                    61.956024169921875,
                                    294,
                                    72.0000228881836
                                ],
                                &quot;spans&quot;: [
                                    {
                                        &quot;bbox&quot;: [
                                            54.0,
                                            61.956024169921875,
                                            296.2261657714844,
                                            72.0000228881836
                                        ],
                                        &quot;content&quot;: &quot;dependent on the service headway and the reliability of the departure &quot;,
                                        &quot;type&quot;: &quot;text&quot;,
                                        &quot;score&quot;: 1.0
                                    }
                                ]
                            }
                        ]
                    }
                ]
            }
        ],
        &quot;_parse_type&quot;: &quot;txt&quot;,
        &quot;_version_name&quot;: &quot;0.6.1&quot;
    }
```

解析结果是一个多层嵌套，从大到小依次为：PDF 文档 → 包含多个页面 → 包含多个区块(blocks) → 分为一级区块和二级区块 → 每个区块包含多行 → 每行包含多个最小单元， 其中para_blocks内存储的元素为区块信息， span是所有元素的最小存储单元。大家理解了这个结构后，可以结合下图对每个字段进行理解，我们这里不再赘述。

一级

| 字段名 | 解释 |
| --- | --- |
| pdf_info | list，每个元素都是一个 dict，这个 dict 是每一页 PDF 的解析结果，详见下表 |
| _parse_type | ocr |
| _version_name | string，表示本次解析使用的 magic-pdf 的版本号 |

二级

| 字段名 | 解释 |
| --- | --- |
| preproc_blocks | PDF 预处理后，未分段的中间结果 |
| layout_bboxes | 布局分割的结果，含有布局的方向（垂直、水平），和 bbox，按阅读顺序排序 |
| page_idx | 页码，从 0 开始 |
| page_size | 页面宽度和高度 |
| _layout_tree | 布局树状结构 |
| images | list，每个元素是一个 dict，每个 dict 表示一个 img_block |
| tables | list，每个元素是一个 dict，每个 dict 表示一个 table_block |
| interline_equations | list，每个元素是一个 dict，每个 dict 表示一个 interline_equation_block |
| discarded_blocks | list，模型返回的需要 drop 的 block 信息 |
| para_blocks | 将 preproc_blocks 进行分段之后的结果 |

三级（最外层block）

| 字段名 | 解释 |
| --- | --- |
| type | block 类型（table 或 image） |
| bbox | block 矩形框坐标 |
| blocks | list，里面的每个元素都是一个 dict 格式的二级 block |

四级（内层block）

| 字段名 | 解释 |
| --- | --- |
| type | block 类型 |
| bbox | block 矩形框坐标 |
| lines | list，每个元素都是一个 dict 表示的 line，用来描述一行信息的构成 |
| ----------------------- | -------------------------- |
| **其中 type 类型** | **描述** |
| image_body | 图像的本体 |
| image_caption | 图像的描述文本 |
| image_footnote | 图像的脚注 |
| table_body | 表格本体 |
| table_caption | 表格的描述文本 |
| table_footnote | 表格的脚注 |
| text | 文本块 |
| title | 标题块 |
| index | 目录块 |
| list | 列表块 |
| interline_equation | 行间公式块 |

五级（lines）

| 字段名 | 解释 |
| --- | --- |
| bbox | line 的矩形框坐标 |
| spans | list，每个元素都是一个 dict 表示的 span，用来描述一个最小组成单元的构成 |

六级（spans）

| 字段名 | 解释 |
| --- | --- |
| bbox | span 的矩形框坐标 |
| type | span 的类型 |
| content | img_path |
| ----------------------- | -------------------------- |
| **其中 type 类型** | **描述** |
| type | 描述 |
| image | 图片 |
| table | 表格 |
| text | 文本 |
| inline_equation | 行内公式 |
| interline_equation | 行间公式 |

该.json文件除了能像model.json一样提取特定类型的内容，因为其内容的丰富程度，可以做更精细化的文本分析，比如段落级别的文本分析，建立更精确的搜索索引，支持按内容类型搜索等。

7. span.pdf：根据 span 类型的不同，采用不同颜色线框绘制页面上所有 span。该文件可以用于质检，可以快速排查出文本丢失、行间公式未识别等问题。

最后一个 .content_list.json 文件，是PDF 文档中所有内容的列表，使用 JSON 格式存储数据，每个元素都是一个字典，包含了不同类型的内容（文本、图像等）。

```
    [
        {
            // 文本内容块
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;...&quot;,
            &quot;text_level&quot;: N,  // 可选字段
            &quot;page_idx&quot;: N
        },
        {
            // 图像内容块
            &quot;type&quot;: &quot;image&quot;,
            &quot;img_path&quot;: &quot;...&quot;,
            &quot;img_caption&quot;: [],
            &quot;img_footnote&quot;: [],
            &quot;page_idx&quot;: N
        },
        {
            // 表格内容块
            &quot;type&quot;: &quot;table&quot;,
            &quot;img_path&quot;: &quot;...&quot;,
            &quot;table_caption&quot;: [],
            &quot;table_footnote&quot;: [],
            &quot;table_body&quot;: &quot;...&quot;,
            &quot;page_idx&quot;: N
        }
    ]

![](img/79d7faf9d08a8da665f4c740941461e8.png)


![](img/3e3e761cbed850dd93582285fdd26f3e.png)

```

字段说明

| 字段名 | 出现位置 | 数据类型 | 说明 |
| --- | --- | --- | --- |
| type | 所有内容块 | 字符串 | 内容块的类型，可能的值有：&quot;text&quot;、&quot;image&quot;、&quot;table&quot; |
| page_idx | 所有内容块 | 整数 | 内容所在的页码，从0开始计数 |
| text | 文本类型块 | 字符串 | 文本内容 |
| text_level | 部分文本类型块 | 整数 | 文本的层级，通常用于标题，1表示一级标题 |
| img_path | 图像和表格类型块 | 字符串 | 图像或表格截图的文件路径 |
| img_caption | 图像类型块 | 数组 | 图像的说明文字，通常为空数组 |
| img_footnote | 图像类型块 | 数组 | 图像的脚注，通常为空数组 |
| table_caption | 表格类型块 | 数组 | 表格的说明文字，通常为空数组 |
| table_footnote | 表格类型块 | 数组 | 表格的脚注，通常为空数组 |
| table_body | 表格类型块 | 字符串 | 表格的HTML内容，包含完整的表格结构 |

以上是对MinerU解析结果的说明，通过这些文件，我们基本上可以获取到PDF文档中所有内容的位置、类型、内容等信息，理解各个文件的结构，也是我们后续进行自定义处理的关键，所以这里大家务必明确文件及其内部结构的组成。除此以外，除了PDF格式，该流程也可以支持图像（.jpg及.png）、PDF、Word（.doc及.docx）、以及PowerPoint（.ppt及.pptx）在内的多种文档格式的解析，大家可以进行尝试。

# 四、PaddleOCR项目介绍及vLLM服务启动

本小节课程，我们将从零开始，详细介绍如何在本地环境中完整部署 PaddleOCR-VL 并通过 vLLM 服务启动 PaddleOCR API 服务。
对于硬件要求，官方给出了如上说明。其中使用原生的PaddlePaddle方式，需要GPU算力≥8.5（RTX 3090/4090、A100 等），最稳定。使用vLLM方式，需要GPU算力≥8（RTX 3060 及以上），速度最快，但算力7-8之间（T4/V100）虽可运行但不推荐，易超时或OOM。使用SGLang方式，需要GPU算力8-12之间（RTX 3060-4090），性能与稳定性的平衡选择。

注意：表格里的 "≥8" 指的是 GPU Compute Capability（GPU 算力版本号），不是显存大小！根据实测，根据PaddleOCR-VL-0.9B 模型（模型文件约 3.8GB）：

+   最低要求：6GB 显存（勉强够用，单张图）

+   推荐配置：8GB+ 显存（运行舒适）

+   理想配置：12GB+ 显存（可以批处理多张图）

所以：8GB 显存 + RTX 30 系列以上达到高效运行是完全没问题的。整个过程主要包括以下几个核心步骤：

1.  **创建 Python 虚拟环境** - 隔离项目依赖，避免环境冲突；

1.  安装 PaddlePaddle 深度学习框架 - PaddleOCR 的底层依赖；

1.  **安装 PaddleOCR 库** - 核心 OCR 功能库；

1.  **下载预训练模型** - PaddleOCR-VL-0.9B 和 PP-DocLayoutV2；

1.  **验证安装** - 运行测试确保环境正常；

公开课所使用的硬件环境为：Ubuntu 22.04 + 4 * 3090，共计显存96G显存，运行起来非常流畅。
## 4.1 创建虚拟环境

首先，我们需要创建一个独立的 Python 虚拟环境。虚拟环境可以隔离项目依赖，避免与系统其他 Python 项目产生冲突。执行如下命令

```
    conda create -n ppocr-vllm python=3.11 -y

![](img/616ff4b0d83c9937fe1e4dc2629e738f.png)

```

其中：

+   --name ppocr-vllm：虚拟环境名称，可以自定义

+   python==3.11：指定 Python 版本为 3.11（PaddleOCR 推荐版本）

执行效果如下图所示：
接下来激活虚拟环境：

```
    conda activate ppocr-vllm

![](img/ffdd769d76e9aebed876624968e5e5c9.png)


![](img/22ebe45bf761229bd3bf4c5ec1af6795.png)


![](img/d901f56b9062c4e8b92590d3308dfcfd.png)


![](img/4aa30b3f4c7023318f6e80cb3e08adce.png)

```

激活后，命令行提示符前会显示 (ppocr-vllm)，表示已进入虚拟环境。
## 4.2 安装 PaddleOCR 工具框架

首先需要区分PaddleOCR 和 PaddleOCR-VL 两个之间的区别和联系。首先。PaddleOCR 是一个较为成熟、功能全面的 OCR＋文档理解开源工具库，其开源地址：https://github.com/PaddlePaddle/PaddleOCR/tree/main
PaddleOCR-VL 是在这个生态内新发布的一个专注于“文档解析／视觉-语言模型 (Vision-Language Model, VLM)”功能的新模块。
简单来说，PaddleOCR 是一个“全面且成熟”的 OCR＋文档理解工具箱，而 PaddleOCR-VL 是在这个工具箱里“专门为复杂文档解析”设计的新模块，采用了视觉-语言模型架构以应对更高阶的需求。

因此，在使用PaddleOCR-VL 时，我们首先需要安装 PaddleOCR 工具框架。https://www.paddlepaddle.org.cn/install/quick?docurl=/documentation/docs/zh/develop/install/pip/linux-pip.html
这里安装 PaddlePaddle 3.2.0 版本。执行如下命令：

```
    python -m pip install paddlepaddle-gpu==3.2.0 -i https://www.paddlepaddle.org.cn/packages/stable/cu126/

![](img/77be0e4bdfc98ef37025d5fa670ec040.png)

```

其中:

+   paddlepaddle-gpu==3.2.0：GPU 版本的 PaddlePaddle 3.2.0

+   -i https://...：使用百度官方镜像源，下载速度更快

+   cu126：对应 CUDA 12.6 版本

安装过程如下图所示：
安装结束后，需要验证 PaddlePaddle 安装状态，依次运行以下命令验证：

```
    python
    import paddle
    paddle.utils.run_check()

![](img/67252fc5ececb01bb5461ade8e749f06.png)


![](img/03309c1e2baccb8e14b80f61efb77b96.png)

```
如果看到如下“”信息，说明 PaddlePaddle 已成功安装。
接下来要重点关注的是：PaddleOCR-VL 使用 safetensors 格式存储模型权重，需要额外安装，同时需要安装指定版本的，执行如下命令

```
    python -m pip install https://paddle-whl.bj.bcebos.com/nightly/cu126/safetensors/safetensors-0.6.2.dev0-cp38-abi3-linux_x86_64.whl

![](img/9b0630a5b4f9af2d0141306401a007e4.png)


![](img/5a4e824f6691b13bbf70e9dbef43d275.png)


![](img/7db73b64745052e878b8f59881cf62ab.png)

```

这是兼容最新PaddleOCR-Vl 的 safetensors 版本，安装很快。
## 4.3 下载PaddleOCR-VL模型

使用 PaddleOCR-VL 解析功能需要两个预训练模型，其中：

1.  PaddleOCR-VL-0.9B - 视觉语言模型（用于文本识别）

1.  **PP-DocLayoutV2** - 文档布局检测模型（用于布局分析）

该模型权重的HuggingFace 地址为：https://huggingface.co/PaddlePaddle/PaddleOCR-VL ，
此外，对于我们国内用户来说，更建议通过 ModelScope 下载（推荐）。https://modelscope.cn/models/PaddlePaddle/PaddleOCR-VL/summary
ModelScope 是阿里云的模型托管平台，国内访问速度快。首先执行如下命令：

```
    pip install modelscope
```

新建一个 download_paddleocr_vl.py 文件，写入如下代码：

```
    from modelscope import snapshot_download

    # 下载完整模型（包含 PaddleOCR-VL-0.9B 和 PP-DocLayoutV2）
    model_dir = snapshot_download(&#x27;PaddlePaddle/PaddleOCR-VL&#x27;, local_dir=&#x27;.&#x27;)

![](img/53a1c010d8029378526945eff8c92495.png)

![](img/bdffaeb729f7bbfaa6f248ab35aec07e.png)

```

其中：

+   'PaddlePaddle/PaddleOCR-VL'：模型仓库 ID

+   local_dir='.'：下载到当前目录（也可以指定其他路径，如 './models'）
接下来执行如下代码进行模型权重安装：

```
   python download_paddleocr_vl.py

![](img/8effbc57982935d0f19c03870d1a43bc.png)


![](img/cfacbc2bd7903391822bf7bd7e7e45e6.png)


![](img/fc46a1689de4944ed5f5bbc3beb61b76.png)

```
下载完成后的模型目录结构如下所示：
其中 PaddleOCR-VL-0.9B 文件夹中存储的就是本次最新开源的超紧凑视觉语言模型，具有以下特点：

PaddleOCR-VL-0.9B 模型特性

| 特性 | 说明 |
| --- | --- |
| **模型规模** | 0.9B 参数（极小，推理速度快） |
| **语言支持** | 109 种语言（包括中文、英文、日语、韩语等） |
| **识别能力** | 文本、表格、公式、图表等复杂元素 |
| **输出格式** | Markdown、JSON、HTML |
| **资源消耗** | GPU 显存 4GB+，推理速度 1.22 页/秒（A100） |
| **优势** | 比 MinerU 快 15.8%，显存占用比 dots.ocr 少 40% |

接下来，在进行运行前需要依次安装依赖包，首先是paddleocr[all], 其中：
```
    # 只希望使用基础文字识别功能（返回文字位置坐标和文本内容），包含 PP-OCR 系列
    python -m pip install paddleocr
    # 希望使用文档解析、文档理解、文档翻译、关键信息抽取等全部功能
    # python -m pip install &quot;paddleocr[doc-parser]&quot;

![](img/a751607617b1b789555d26c2bc2d5db4.png)

```

这里我们安装doc-parser，如下图所示：
然后，使用 PaddleOCR CLI 安装 vLLM 的推理加速服务依赖：

```
    paddleocr install_genai_server_deps vllm

![](img/7dbadf0a14a7c5972f64f8da3867ea4a.png)

```
安装完成后，继续安装 flash-atten 编译包：

```
    pip install https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu12torch2.8cxx11abiFALSE-cp311-cp311-linux_x86_64.whl
```

接下来，就可以启动vLLM 服务器服务器了。如下代码所示：

```
    paddlex_genai_server \
    --model_name PaddleOCR-VL-0.9B \
    --backend vllm \
    --host 0.0.0.0 \
    --port 8118

![](img/18ebb65491dff5e561f004a2aa06faf9.png)

```
vLLM 服务启动后，保持这个终端窗口运行，不要关闭。同时打开一个新的终端，使用paddlex连接启动的paddleocr-vl服务，首先进行初始化服务配置：

```
    paddlex --install serving

![](img/be20c8ef53189fad70dab0070dc73c97.png)

```
然后生成 .yaml 配置文件：

```
    paddlex --get_pipeline_config PaddleOCR-VL

![](img/22e14379362438a06ba256bfbb8c46e8.png)


![](img/287cac4337b0e5240f0d02e1aa2f790f.png)

```
找到 genai_config 配置项，修改为如下所示：
最后，启动 PaddleOCR API 服务：

```
    paddlex --serve --pipeline PaddleOCR-VL.yaml --port 10800 --host 192.168.110.131 --paddle_model_dir /home/MuyuWorkSpace/02_OcrRag

![](img/43cbd574dc4e730b90c99b1fad413d80.png)


![](img/12420af02985e06e1308d4062d8e501c.png)

```
成功启动后，在浏览器中访问 http://192.168.110.131:10800/docs 查看API文档，如下图所示：
至此，PaddleOCR API 服务启动成功。

## 4.4 PaddleOCR vLLM API服务连接测试

```
import base64
import json
import requests
import os

# === 1. 服务端地址 ===
SERVER_URL = &quot;http://192.168.130.4:10800/layout-parsing&quot;

# === 2. 待处理文件路径 ===
input_path = &quot;./course.pdf&quot;   # 也可以是 test.png
output_md = &quot;result.md&quot;

# === 3. 读取文件并转为 Base64 ===
with open(input_path, &quot;rb&quot;) as f:
    file_base64 = base64.b64encode(f.read()).decode(&quot;utf-8&quot;)

# === 4. 构造 JSON 请求体 ===
payload = {
    &quot;file&quot;: file_base64,
    &quot;fileType&quot;: 0 if input_path.lower().endswith(&quot;.pdf&quot;) else 1,
    &quot;prettifyMarkdown&quot;: True,
    &quot;visualize&quot;: False,
}

headers = {&quot;Content-Type&quot;: &quot;application/json&quot;}

# === 5. 发送请求 ===
resp = requests.post(SERVER_URL, headers=headers, data=json.dumps(payload))

# === 6. 解析响应 ===
if resp.status_code == 200:
    data = resp.json()
    if data.get(&quot;errorCode&quot;) == 0:
        # PDF 的结果在 layoutParsingResults 数组中
        results = data[&quot;result&quot;][&quot;layoutParsingResults&quot;]
        md_text = &quot;&quot;
        for i, page in enumerate(results, 1):
            md_text += f&quot;\n\n# Page {i}\n\n&quot;
            md_text += page[&quot;markdown&quot;][&quot;text&quot;]

        with open(output_md, &quot;w&quot;, encoding=&quot;utf-8&quot;) as f:
            f.write(md_text)
        print(f&quot;成功生成 Markdown：{output_md}&quot;)
    else:
        print(f&quot;服务端错误：{data.get(&#x27;errorMsg&#x27;)}&quot;)
else:
    print(f&quot;HTTP 错误：{resp.status_code}&quot;)
    print(resp.text)
```

```
成功生成 Markdown：result.md

![](img/d6e529f8f5b9f11284b92248ce6e0189.png)


![](img/dbe1e1db7004c5997975b050ce968fef.png)

```

PaddleOCR Doc Parser 命令参数如下表所示：

基础参数

| 参数名 | 说明 |
| --- | --- |
| -i INPUT, --input INPUT | 输入路径或 URL（必需） |
| --save_path SAVE_PATH | 输出目录路径 |

布局检测参数

| 参数名 | 说明 |
| --- | --- |
| --layout_detection_model_name | 布局检测模型名称 |
| --layout_detection_model_dir | 布局检测模型目录路径 |
| --layout_threshold | 布局检测模型的分数阈值 |
| --layout_nms | 是否在布局检测中使用 NMS（非极大值抑制） |
| --layout_unclip_ratio | 布局检测的扩展系数 |
| --layout_merge_bboxes_mode | 重叠框过滤方法 |

VL识别模型参数

| 参数名 | 说明 |
| --- | --- |
| --vl_rec_model_name | VL 识别模型名称 |
| --vl_rec_model_dir | **VL 识别模型目录路径（指定本地 Pad**dleOCR-VL-0.9B 模型路径） |
| --vl_rec_backend | VL 识别模块使用的后端（native, vllm-server, sglang-server） |
| --vl_rec_server_url | VL 识别模块使用的服务器 URL |
| --vl_rec_max_concurrency | VLM 请求的最大并发数 |

文档处理模型

| 参数名 | 说明 |
| --- | --- |
| --doc_orientation_classify_model_name | 文档图像方向分类模型名称 |
| --doc_orientation_classify_model_dir | 文档图像方向分类模型目录路径 |
| --doc_unwarping_model_name | 文本图像矫正模型名称 |
| --doc_unwarping_model_dir | 图像矫正模型目录路径 |

功能开关参数

| 参数名 | 说明 |
| --- | --- |
| --use_doc_orientation_classify | 是否使用文档图像方向分类 |
| --use_doc_unwarping | 是否使用文本图像矫正 |
| --use_layout_detection | 是否使用布局检测 |
| --use_chart_recognition | 是否使用图表识别 |
| --format_block_content | 是否将块内容格式化为 Markdown |
| --use_queues | 是否使用队列进行异步处理 |

VLM生成参数

| 参数名 | 说明 |
| --- | --- |
| --prompt_label | VLM 的提示标签 |
| --repetition_penalty | VLM 采样中使用的重复惩罚系数 |
| --temperature | VLM 采样中使用的温度参数 |
| --top_p | VLM 采样中使用的 top-p 参数 |
| --min_pixels | VLM 图像预处理的最小像素数 |
| --max_pixels | VLM 图像预处理的最大像素数 |

硬件与性能参数

| 参数名 | 说明 |
| --- | --- |
| --device | 用于推理的设备，例如 cpu、gpu、npu、gpu:0、gpu:0,1。如果指定多个设备，将并行执行推理。注意：并非所有情况都支持并行推理。默认情况下，如果可用将使用 GPU 0，否则使用 CPU |
| --enable_hpi | 启用高性能推理 |
| --use_tensorrt | 是否使用 Paddle Inference TensorRT 子图引擎。如果模型不支持 TensorRT 加速，即使设置此标志也不会使用加速 |
| --precision | 使用 Paddle Inference TensorRT 子图引擎时的 TensorRT 精度（fp32, fp16） |
| --enable_mkldnn | 为推理启用 MKL-DNN 加速。如果 MKL-DNN 不可用或模型不支持，即使设置此标志也不会使用加速 |
| --mkldnn_cache_capacity | MKL-DNN 缓存容量 |
| --cpu_threads | 在 CPU 上用于推理的线程数 |
| --paddlex_config | PaddleX 管道配置文件路径 |

当然，上述代码也是LangChain1.0 + OCR 多模态PDF解析实战项目中PaddleOCR 的核心代码逻辑：
# 五、DeepSeek-OCR vLLM 服务接口启动

DeepSeek-OCR 作为刚刚开源的小型多模态视觉模型，经过我们团队大量的测试，其不仅识别精度高，同时对中文的支持也非常出色，同时也能够输出结构化的 Markdown 格式文档。
## 5.1 环境配置要求

在开始部署之前，我们首先需要确认自己的服务器是否满足运行条件。

硬件配置要求（建议）

| 硬件类型 | 最低要求 | 推荐配置 | 说明 |
| --- | --- | --- | --- |
| GPU | NVIDIA RTX 3090 | RTX 4090 或更高 | 必须是NVIDIA显卡，支持CUDA |
| 显存 | 24GB | 24GB+ | 显存越大，处理速度越快 |
| 内存 | 32GB | 64GB+ | 用于图像预处理和数据缓存 |
| 硬盘 | 50GB可用空间 | 100GB+ | 模型文件较大，需要足够空间 |

**  除了**硬件，我们还需要准备相应的软件环境：主要包括操作系统：建议使用 Ubun**tu 22.04，**同时需要 CUDA 12.0+，而Python 环境建议使用 Python 3.10+。

课程使用的配置环境为：Ubuntu 22.04 + 四卡 3090。大家在使用 vLLM 启动DeepSeek-OCR时，首先需要做如下配置校验：

```
    # 检查 GPU 是否正常工作
    nvidia-smi

![](img/082f7732e67288c32fd462e2b2320ec5.png)

```

执行上述命令后，如果看到类似下面的输出，说明GPU正常工作：
接下来检查 Conda 是否安装，执行如下命令：

```
    conda --version

![](img/a76c62fbbbc0af650645584907e33685.png)

```

如果看到类似 conda XXX 的版本号，说明conda已经安装。如果提示命令不存在，需要先安装 miniconda 或者 anaconda3。
## 5.2 DeepSeek-OCR模型下载

环境准备好之后，接下来我们需要下载 DeepSeek-OCR 模型文件，并进行相应的配置。这一步是整个部署过程的核心。国内用户强烈建议大家使用 ModelScope 下载模型，没有网络限制，同时速度也会快很多。

首先，我们需要创建一个独立的 Python 虚拟环境。虚拟环境可以隔离项目依赖，避免与系统其他 Python 项目产生冲突。执行如下命令

```
    conda create --name deepseek-ocr-vllm python==3.10

![](img/090d42133f20b1eaefe3c32aac1f3407.png)

```

其中：

+   --name deepseek-ocr-vllm：虚拟环境名称，可以自定义

+   python==3.10：指定 Python 版本为 3.10（PaddleOCR 推荐版本）

执行效果如下图所示：
接下来激活虚拟环境：

```
    conda activate deepseek-ocr-vllm

![](img/f390eb541e489e4a7d2188f2f6f9b947.png)


![](img/000c407ac6913a723f0f04ade78ea262.png)

```

激活后，命令行提示符前会显示 (deepseek-ocr-vllm)，表示已进入虚拟环境。
接下来，在ModelScope 平台下载 DeepSeek-OCR 模型文件。地址：https://modelscope.cn/models/deepseek-ai/DeepSeek-OCR
ModelScope 是阿里云的模型托管平台，国内访问速度快。首先执行如下命令：

```
    pip install modelscope

![](img/8904a044e7c44a1d37dbc06d8ce62678.png)

```
然后执行如下代码：

```
    from modelscope import snapshot_download
    model_dir = snapshot_download(&#x27;deepseek-ai/DeepSeek-OCR&#x27;,local_dir=&#x27;.&#x27;)
```

其中：

+   'deepseek-ai/DeepSeek-OCR'：模型仓库 ID

+   local_dir='.'：下载到当前目录（也可以指定其他路径，如 './models'）

接下来执行如下代码进行模型权重安装：

```
   python download_deepseek_ocr.py

![](img/f864240ef94af2f51fd0609f64e5cd92.png)


![](img/d456d1aa4d4d85873fbfc4bc5fc52212.png)

![](img/d5d51f495236ed0bee4edaf94d2d3359.png)


![](img/43291baaf20d33878cd9e85bd4822e88.png)


![](img/162603ec0b6d541715dbd3d8d06acdf4.png)

![](img/2e0c444567d93a8a22530b72b541928b.png)

![](img/430144f88986e05dd4bfeb8965ef48f3.png)


![](img/aac2b5efdc59d1fda85a60db6ba81973.png)


![](img/8b1579d13b95a51304a5cd7508ca3939.png)


![](img/ab33658f89cdaa35252db23d99c259d7.png)

```
下载完成后的模型目录结构如下所示：
## 5.3 Deepseek-OCR vLLM 项目文件

DeepSeek-OCR通过 vLLM 平台启动，我们是借助DeepSeek-OCR官方提供的项目代码，并做了一些优化和调整，其官方源码下载地址：https://github.com/deepseek-ai/DeepSeek-OCR/tree/main/DeepSeek-OCR-master/DeepSeek-OCR-vllm
我们将项目文件下载到本地并上传到服务器上：
其中deepseek_ocr.py 和 flash_attn-2.7.3+cu12torch2.6cxx11abiFALSE-cp310-cp310-linux_x86_64.whl 使我们给大家提供的两个核心文件，一个用于封装 DeepSeek OCR API 接口，另一个则是离线安装的 fla_ttn 的离线安装包。
接下来一键安装 vLLM 运行 DeepSeek-OCR的依赖包：
安装结束后，使用我们给大家提供的deepseek_ocr.py文件，启动 DeepSeek OCR API 接口 服务。
启动命令如下：

```
    python ocr_client.py --model-path /home/data/nongwa/workspace/model/OCR/DeepSeek-OCR --gpu-id 3 --port 8797 --host 192.168.110.131

![](img/b85ada97553b13e191a55c69a06be5f2.png)


![](img/c1cbd0f1434970fb79b49b088c96c216.png)


![](img/1ab4e131f31ab4c03ff8cad0ba4744b4.png)

```
启动成功后，会显示如下：
接下来，便可以在网页端查看到 FastAPI 接口：
## 5.4 DeepSeek-OCR API 连接测试

前面我们完成了服务的部署和启动，接下来我们要学习如何在实际项目中调用这个 OCR 服务。本节将通过完整的代码示例，带领大家掌握图片识别和 PDF 文档处理的全流程。

在开始编写调用代码之前，我们先来了解一下 OCR 服务提供的 API 接口规范。

OCR API 接口规范

| 项目 | 说明 |
| --- | --- |
| 接口地址 | http://localhost:8797/ocr |
| 请求方式 | POST |
| 内容类型 | multipart/form-data |
| 文件参数 | file（支持 jpg, png, pdf 等格式） |
| 可选参数 | enable_description（是否生成图片描述，默认 False） |

返回的数据格式如下：

```
    {
        &quot;markdown&quot;: &quot;识别出的文本内容（Markdown格式）&quot;,
        &quot;page_count&quot;: 1,
        &quot;processing_time&quot;: 2.34
    }
```

返回的 markdown 字段就是我们需要的识别结果，它保留了原文档的格式结构，包括标题、列表、表格等。

下面我们通过一个完整的代码示例，来演示如何调用 OCR 服务。

```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
&quot;&quot;&quot;
测试 DeepSeek-OCR API
直接调用 API 并保存返回的 markdown 和图像数据
&quot;&quot;&quot;

import requests
import json
import sys
from pathlib import Path


def test_deepseek_ocr(pdf_path: str, output_dir: str = &quot;./test_output&quot;):
    &quot;&quot;&quot;
    测试 DeepSeek-OCR API

    Args:
        pdf_path: PDF 文件路径
        output_dir: 输出目录
    &quot;&quot;&quot;
    # API 配置
    api_url = &quot;http://192.168.130.4:8797/ocr&quot;

    # 确保输出目录存在
    output_path = Path(output_dir)
    output_path.mkdir(parents=True, exist_ok=True)

    print(f&quot;测试文件: {pdf_path}&quot;)
    print(f&quot;API 地址: {api_url}&quot;)
    print(f&quot;输出目录: {output_dir}&quot;)
    print(&quot;-&quot; * 60)

    # 读取 PDF 文件
    try:
        with open(pdf_path, &#x27;rb&#x27;) as f:
            files = {&#x27;file&#x27;: (Path(pdf_path).name, f, &#x27;application/pdf&#x27;)}

            # API 参数
            data = {
                &#x27;enable_description&#x27;: &#x27;false&#x27;,  # 是否生成图片描述
            }

            print(&quot;发送请求到 DeepSeek API...&quot;)

            # 发送请求
            response = requests.post(
                api_url,
                files=files,
                data=data,
                timeout=300
            )

            if response.status_code != 200:
                print(f&quot;API 返回错误: {response.status_code}&quot;)
                print(f&quot;错误信息: {response.text[:500]}&quot;)
                return False

            # 解析响应
            result = response.json()

            print(f&quot;API 响应成功&quot;)
            print(f&quot;响应包含的字段: {list(result.keys())}&quot;)
            print(&quot;-&quot; * 60)

            # 提取数据
            markdown_content = result.get(&quot;markdown&quot;, &quot;&quot;)
            page_count = result.get(&quot;page_count&quot;, 0)
            images_data = result.get(&quot;images&quot;, {})

            print(f&quot;Markdown 长度: {len(markdown_content)} 字符&quot;)
            print(f&quot;页数: {page_count}&quot;)
            print(f&quot;图像数量: {len(images_data)}&quot;)

            if images_data:
                print(f&quot;图像列表:&quot;)
                for img_key in list(images_data.keys())[:10]:
                    img_size = len(images_data[img_key])
                    print(f&quot;   - {img_key}: {img_size} 字符 (base64)&quot;)

            print(&quot;-&quot; * 60)

            # 保存 Markdown
            md_file = output_path / f&quot;{Path(pdf_path).stem}_deepseek.md&quot;
            with open(md_file, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as f:
                f.write(markdown_content)
            print(f&quot;Markdown 已保存: {md_file}&quot;)

            # 保存完整响应
            json_file = output_path / f&quot;{Path(pdf_path).stem}_response.json&quot;
            with open(json_file, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as f:
                # 为了避免文件过大，只保存图像的部分信息
                simplified_result = {
                    &quot;markdown&quot;: markdown_content,
                    &quot;page_count&quot;: page_count,
                    &quot;images_count&quot;: len(images_data),
                    &quot;image_keys&quot;: list(images_data.keys())
                }
                json.dump(simplified_result, f, ensure_ascii=False, indent=2)
            print(f&quot;响应摘要已保存: {json_file}&quot;)

            # 保存图像数据（可选，如果需要）
            if images_data:
                images_file = output_path / f&quot;{Path(pdf_path).stem}_images.json&quot;
                with open(images_file, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as f:
                    json.dump(images_data, f, ensure_ascii=False, indent=2)
                print(f&quot;图像数据已保存: {images_file}&quot;)

            # 统计信息
            print(&quot;-&quot; * 60)
            print(&quot;统计信息:&quot;)

            # 统计表格数量
            import re
            table_count = len(re.findall(r&#x27;&lt;table&gt;&#x27;, markdown_content, re.IGNORECASE))
            print(f&quot;   - HTML 表格: {table_count} 个&quot;)

            # 统计图片引用
            img_ref_count = len(re.findall(r&#x27;!\[.*?\]\(.*?\)&#x27;, markdown_content))
            print(f&quot;   - Markdown 图片引用: {img_ref_count} 个&quot;)

            # 统计行数
            line_count = len(markdown_content.split(&#x27;\n&#x27;))
            print(f&quot;   - Markdown 行数: {line_count} 行&quot;)

            print(&quot;-&quot; * 60)
            print(&quot;测试完成！&quot;)

            return True

    except FileNotFoundError:
        print(f&quot;文件不存在: {pdf_path}&quot;)
        return False
    except requests.exceptions.Timeout:
        print(f&quot;请求超时&quot;)
        return False
    except Exception as e:
        print(f&quot;发生错误: {e}&quot;)
        import traceback
        traceback.print_exc()
        return False


def main():
    &quot;&quot;&quot;主函数&quot;&quot;&quot;


    success = test_deepseek_ocr(
        pdf_path=&#x27;./course.pdf&#x27;, 
        output_dir=&#x27;./&#x27;)


if __name__ == &quot;__main__&quot;:
    main()
```

```
测试文件: ./course.pdf
API 地址: http://192.168.110.131:8797/ocr
输出目录: ./
------------------------------------------------------------
发送请求到 DeepSeek API...
API 响应成功
响应包含的字段: [&#x27;markdown&#x27;, &#x27;page_count&#x27;, &#x27;images&#x27;]
------------------------------------------------------------
Markdown 长度: 10026 字符
页数: 12
图像数量: 14
图像列表:
   - page_0_img_0.png: 161960 字符 (base64)
   - page_1_img_0.png: 9892 字符 (base64)
   - page_1_img_1.png: 111764 字符 (base64)
   - page_1_img_2.png: 43132 字符 (base64)
   - page_2_img_0.png: 66016 字符 (base64)
   - page_3_img_0.png: 57016 字符 (base64)
   - page_4_img_0.png: 53420 字符 (base64)
   - page_5_img_0.png: 166376 字符 (base64)
   - page_6_img_0.png: 34012 字符 (base64)
   - page_8_img_0.png: 227632 字符 (base64)
------------------------------------------------------------
Markdown 已保存: course_deepseek.md
响应摘要已保存: course_response.json
图像数据已保存: course_images.json
------------------------------------------------------------
统计信息:
   - HTML 表格: 9 个
   - Markdown 图片引用: 14 个
   - Markdown 行数: 264 行
------------------------------------------------------------
测试完成！
```

通过不同的提示词，对输入的PDF文档的每一页做格式化提取：

```
    # 固定 Prompt
    PROMPT_OCR = &quot;&lt;image&gt;\n&lt;|grounding|&gt;Convert the document to markdown.&quot;
    PROMPT_DESC = &quot;&lt;image&gt;\nDescribe this image in detail.&quot;

![](img/2fe2000300f5c7453d74ea82b2ff7f6c.png)


![](img/a70e0d5a5eec6ed4a4ab2d77347389cb.png)

```

当然，上述代码也是LangChain1.0 + OCR 多模态PDF解析实战项目中DeepSeek-OCR 的核心代码逻辑：
# 六、OCR 多模态解析系统本地部署
完整的系统需要同时启动后端和前端两个服务。首先对于后端服务启动，需要依次执行如下操作：

```
    # 1. 进入后端目录
    cd backend

    # 2. 创建并激活虚拟环境
    python -m venv venv
    source venv/bin/activate  # Linux/Mac
    # 或
    venv\Scripts\activate     # Windows

    # 3. 安装依赖
    pip install -r requirements.txt

    # 4. 配置环境变量 (.env 文件)
  
    # Server Configuration
    PORT=8000
    HOST=0.0.0.0
    DEBUG=True

    # MinerU Configuration
    MINERU_API_URL=http://192.168.110.131:50000/file_parse
    VLLM_SERVER_URL=http://192.168.110.131:40000
    MINERU_BACKEND=vlm-vllm-async-engine
    MINERU_TIMEOUT=600
    MINERU_VIZ_DIR=/home/MuyuWorkSpace/05_OcrProject/backend/mineru_visualizations

    # DeepSeek OCR Configuration
    DEEPSEEK_OCR_API_URL=http://192.168.110.131:8797/ocr

    # PaddleOCR Configuration
    PADDLEOCR_API_URL=http://192.168.110.131:10800/layout-parsing

    # File Upload Limits
    MAX_FILE_SIZE=10485760
    ALLOWED_FILE_TYPES=application/pdf,image/png,image/jpeg,image/jpg,image/webp

    # CORS Settings
    ALLOWED_ORIGINS=http://localhost:3000,http://localhost:5173


&amp;emsp;&amp;emsp;其中：

&lt;style&gt;
.center 
{
  width: auto;
  display: table;
  margin-left: auto;
  margin-right: auto;
}
&lt;/style&gt;

&lt;p align=&quot;center&quot;&gt;&lt;font face=&quot;黑体&quot; size=4&gt;配置说明&lt;/font&gt;&lt;/p&gt;
&lt;div class=&quot;center&quot;&gt;

| 配置项 | 说明 |
|--------|------|
| `MINERU_BACKEND` | `pipeline`: 使用本地 PyTorch&lt;br&gt;`vlm-vllm-async-engine`: 使用 vLLM 加速 |
| `VLLM_SERVER_URL` | vLLM 服务器地址（仅在使用 vLLM 时需要） |
| `MINERU_API_URL` | MinerU API 服务地址 |
```

启动服务执行如下代码：

```
    python main.py

![](img/85fead82f51e6b8ee052bb072860aeeb.png)


![](img/06dab9ff81c435da2fcc7864b9d3dff5.png)

```
然后，便可以在 http://192.168.110.131:8000/docs 查看 API 文档：
前端服务启动，需要依次执行如下操作:

```
    # 1. 进入前端目录
    cd frontend

    # 2. 安装依赖
    npm install
    # 或
    yarn install

    # 3. 启动开发服务器
    npm run dev
    # 或
    yarn dev

![](img/1ec498876178335e66b460323ee62efc.png)


![](img/d25d65b2f507f257903f1b6cbbdc2444.png)


![](img/fe3952d4fc84a54379af68d6fd736d8c.png)

![](img/6176f4929120c058e224100cb5f257be.png)


![](img/f665505b67fc5381c9234a80611cd6b2.png)

```
启动成功后，即可通过http://localhost:3000访问应用。
# 【加餐】 LangChain1.0 接入 MinerU MCP Server

随着MinerU项目的发展，除了解析性能上进一步提升，同时也支持了MCP协议，可以通过stdio、Http + Sse和 Streamable Http三种通信模型进行访问。因此，本小节我们重点讲解MinerU项目在MCP协议下的使用，并基于LangChain 1.0 新版本实现MinerU MCP的接入。

最新版本的MinerU 项目中内置了MCP Server 服务，我们可以通过该服务将MinerU 的解析服务暴露给ADK 或 Agent 使用。与常规的MCP Server 构建方式一样，也是通过 FastMCP 外放FastAPI 服务并支持stdio、Http+Sse和StreamableHttp 三种传输方式。源码位置如下：
这里我们使用StreamableHttp 传输方式，并且在启动前，修改下源码，使其可以加载我们通过命令行传递的host参数，否则只能使用默认的localhost 地址。如下所示：
接下来创建MCP的uv环境，首先通过pip 安装 uv:

```
    pip install uv

![](img/4bf311998a89d508959228056103877a.png)

```
然后通过uv 创建MCP 的uv 环境，并安装MinerU 项目中用到的依赖：

```
    uv venv
    source .venv/bin/activate
    uv pip install -e .

![](img/0a6be24c4b15a411ffe84ee1049949ac.png)

```
安装完成后，我们就可以通过uv 启动MCP 服务了：

```
    uv run mineru-mcp --transport streamable-http --host 192.168.110.131

![](img/24f27f78052c8f0615ad4530832fbe62.png)


![](img/0d577ad58c3c8d08516b592589ec90fe.png)

```
启动后，我们可以在浏览器中访问http://192.168.110.131:8000/mcp快速进行验证：
需要重点注意的是：parse_documents 工具是MinerU 项目中通过FastAPI 外放的工具接口，其本质上，调用的是mineru-api 服务中的/api/parse 接口。所以，大家要理解的逻辑是：

FastMCP 负责暴露parse_documents和get_ocr_languages 工具协议，而真正做 PDF→Markdown 的逻辑在 MinerU 的“本地解析服务”里，并通过 /file_parse 路由完成。MCP 只是一个工具“壳”，真正干活的是mineru-api。因此，我们首先需要修改MinerU MCP Server的配置文件，让其找到解析服务。如下所示：

```
    mv env.example .env
    vi .env

![](img/b826da1fedb74e301800ee627177fa81.png)


![](img/07606a39fe91d6706d5e8f0d23c7064e.png)

```
修改如下：
其中USE_LOCAL_API=true 表示使用本地解析服务，LOCAL_API_URL 表示本地解析服务的地址。修改完成后重新启动MinerU MCP Server 服务。

```
    uv run mineru-mcp --transport streamable-http --host 192.168.110.131

![](img/1f7c3ddd9af5b843b4fadc10c97c0585.png)

```
接下来新打开一个终端，启动`mineru-api` 服务：

```
    export MINERU_MODEL_SOURCE=local  # 注意：这里需要将模型源设置为本地
    mineru-api --host 192.168.110.131 --port 30000

![](img/c5c4ca6b19ca5afcafb7e6a077e91c10.png)


![](img/6f8473f7037edac6ee9615022055bd2b.png)

```
启动后，在192.168.110.131:30000/docs中可以看到接口服务：
全部验证成功后，我们就可以在LangChain中使用MCP 服务了。代码如下：

```
! pip install langchain&gt;=1.0.0 langchain-core&gt;=1.0.0 langchain-mcp-adapters mcp httpx -q
```

```
[33mWARNING: Running pip as the &#x27;root&#x27; user can result in broken permissions and conflicting behaviour with the system package manager, possibly rendering your system unusable. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv. Use the --root-user-action option if you know what you are doing and want to suppress this warning.[0m[33m
[0m
```

安装完成后，让我们导入必要的模块：

```
# 导入必要的库
import asyncio
from pathlib import Path
from mcp import ClientSession
from mcp.client.streamable_http import streamablehttp_client
from langchain_mcp_adapters.tools import load_mcp_tools

print(&quot;所有库导入成功！&quot;)
```

```
所有库导入成功！
```

现在，配置连接到 MinerU MCP 服务的参数。根据服务启动日志，

+   **协议类型**: streamable-http

+   **服务地址**: http://192.168.110.131:8001/mcp
  首先定义服务器配置参数：

```
# 配置 MinerU MCP 服务器参数
MCP_SERVER_URL = &quot;http://192.168.110.131:8001/mcp&quot;

server_params = {
    &quot;url&quot;: MCP_SERVER_URL,
    # 如果需要认证，可以在这里添加 headers
    # &quot;headers&quot;: {
    #     &quot;Authorization&quot;: &quot;Bearer your_token&quot;
    # }
}

print(f&quot;MCP 服务器地址: {MCP_SERVER_URL}&quot;)
print(&quot;服务器参数配置完成&quot;)
```

```
MCP 服务器地址: http://192.168.110.131:8001/mcp
服务器参数配置完成
```

在实际解析 PDF 之前，我们先要了解 MCP 服务器提供了哪些工具。这就像进入一个工具箱，先看看里面有什么工具可用。如下函数来列出所有可用的 MCP 工具：

```
async def list_mcp_tools(server_params):
    &quot;&quot;&quot;
    列出 MCP 服务器提供的所有工具
    
    Args:
        server_params: 服务器连接参数
        
    Returns:
        tools: LangChain 工具列表
    &quot;&quot;&quot;
    print(&quot;🔌 正在连接 MCP 服务器...&quot;)
    
    # 使用 streamable-http 客户端建立连接
    async with streamablehttp_client(**server_params) as (read, write, _):
        # 创建 MCP 会话
        async with ClientSession(read, write) as session:
            # 初始化会话（握手）
            await session.initialize()
            print(&quot;MCP 会话初始化成功\n&quot;)
            
            # 加载所有 MCP 工具
            tools = await load_mcp_tools(session)
            
            print(f&quot;发现 {len(tools)} 个可用工具：\n&quot;)
            print(&quot;=&quot; * 80)
            
            # 打印每个工具的详细信息
            for i, tool in enumerate(tools, 1):
                print(f&quot;\n工具 {i}: {tool.name}&quot;)
                print(f&quot;   描述: {tool.description}&quot;)
                
                # 打印工具参数（如果有）
                if hasattr(tool, &#x27;args_schema&#x27;) and tool.args_schema:
                    print(f&quot;   参数结构:&quot;)
                    if hasattr(tool.args_schema, &#x27;schema&#x27;):
                        schema = tool.args_schema.schema()
                        if &#x27;properties&#x27; in schema:
                            for param_name, param_info in schema[&#x27;properties&#x27;].items():
                                param_type = param_info.get(&#x27;type&#x27;, &#x27;unknown&#x27;)
                                param_desc = param_info.get(&#x27;description&#x27;, &#x27;无描述&#x27;)
                                print(f&quot;      - {param_name} ({param_type}): {param_desc}&quot;)
                
            print(&quot;\n&quot; + &quot;=&quot; * 80)
            
            return tools

# 执行函数（在 Jupyter 中运行异步代码）
tools = await list_mcp_tools(server_params)
```

```
🔌 正在连接 MCP 服务器...
MCP 会话初始化成功

发现 2 个可用工具：

================================================================================

工具 1: parse_documents
   描述: 统一接口，将文件转换为Markdown格式。支持本地文件和URL，会根据USE_LOCAL_API配置自动选择合适的处理方式。

当USE_LOCAL_API=true时:
- 会过滤掉http/https开头的URL路径
- 对本地文件使用本地API进行解析

当USE_LOCAL_API=false时:
- 将http/https开头的路径使用convert_file_url处理
- 将其他路径使用convert_file_path处理

处理完成后，会自动尝试读取转换后的文件内容并返回。

返回:
    成功: {&quot;status&quot;: &quot;success&quot;, &quot;content&quot;: &quot;文件内容&quot;} 或 {&quot;status&quot;: &quot;success&quot;, &quot;results&quot;: [处理结果列表]}
    失败: {&quot;status&quot;: &quot;error&quot;, &quot;error&quot;: &quot;错误信息&quot;}
   参数结构:

工具 2: get_ocr_languages
   描述: 获取 OCR 支持的语言列表。

Returns:
    Dict[str, Any]: 包含所有支持的OCR语言列表的字典
   参数结构:

================================================================================
```

接下来，构建一个完整的客户端类，封装所有的功能，使其更易用。客户端类将包含以下功能：

1.  连接管理

1.  工具列表查询

1.  PDF 文档解析

1.  结果保存

```
class MinerUClient:
    &quot;&quot;&quot;
    MinerU MCP 客户端封装类
    提供简洁的 API 来调用 MinerU 文档解析服务
    &quot;&quot;&quot;
    
    def __init__(self, mcp_url: str = &quot;http://192.168.110.131:8001/mcp&quot;):
        &quot;&quot;&quot;
        初始化客户端
        
        Args:
            mcp_url: MCP 服务器地址
        &quot;&quot;&quot;
        self.mcp_url = mcp_url
        self.server_params = {&quot;url&quot;: self.mcp_url}
        print(f&quot;MinerU 客户端已初始化&quot;)
        print(f&quot;服务器地址: {self.mcp_url}&quot;)
    
    async def get_tools(self):
        &quot;&quot;&quot;
        获取所有可用的 MCP 工具
        
        Returns:
            tools: 工具列表
        &quot;&quot;&quot;
        async with streamablehttp_client(**self.server_params) as (read, write, _):
            async with ClientSession(read, write) as session:
                await session.initialize()
                tools = await load_mcp_tools(session)
                return tools
    
    async def parse_pdf(self, pdf_path: str, output_format: str = &quot;markdown&quot;):
        &quot;&quot;&quot;
        解析 PDF 文件
        
        Args:
            pdf_path: PDF 文件路径（支持相对路径和绝对路径）
            output_format: 输出格式，默认为 markdown
            
        Returns:
            解析结果（Markdown 文本）
        &quot;&quot;&quot;
        # 检查文件是否存在
        pdf_file = Path(pdf_path)
        if not pdf_file.exists():
            raise FileNotFoundError(f&quot;PDF 文件不存在: {pdf_path}&quot;)
        
        print(f&quot;开始解析 PDF: {pdf_file.name}&quot;)
        print(f&quot;文件路径: {pdf_file.absolute()}&quot;)
        print(f&quot;文件大小: {pdf_file.stat().st_size / 1024:.2f} KB\n&quot;)
        
        # 连接 MCP 服务器并解析
        async with streamablehttp_client(**self.server_params) as (read, write, _):
            async with ClientSession(read, write) as session:
                await session.initialize()
                
                # 加载工具
                tools = await load_mcp_tools(session)
                print(f&quot;已加载 {len(tools)} 个工具\n&quot;)
                
                # 查找合适的解析工具
                parse_tool = None
                for tool in tools:
                    # 根据工具名称特征查找
                    tool_name_lower = tool.name.lower()
                    if any(keyword in tool_name_lower for keyword in 
                           [&#x27;convert&#x27;, &#x27;parse&#x27;, &#x27;markdown&#x27;, &#x27;pdf&#x27;, &#x27;file&#x27;]):
                        parse_tool = tool
                        print(f&quot;找到解析工具: {tool.name}&quot;)
                        print(f&quot;   工具描述: {tool.description}\n&quot;)
                        break
                
                if not parse_tool:
                    print(&quot;未找到合适的解析工具，可用工具列表：&quot;)
                    for i, tool in enumerate(tools, 1):
                        print(f&quot;   {i}. {tool.name}&quot;)
                    # 如果没找到特定工具，尝试使用第一个工具
                    if tools:
                        parse_tool = tools[0]
                        print(f&quot;\n尝试使用第一个工具: {parse_tool.name}&quot;)
                    else:
                        raise ValueError(&quot;MCP 服务器没有提供任何工具&quot;)
                
                # 调用工具进行解析
                print(&quot;正在调用 MCP 工具解析文档...&quot;)
                
                try:
                    # 构建工具调用参数 - 参数是字符串格式（支持单个文件或逗号分隔的多个文件）
                    tool_input = {
                        &quot;file_sources&quot;: str(pdf_file.absolute())
                    }
                    
                    # 调用工具
                    result = await parse_tool.ainvoke(tool_input)
                    
                    print(&quot;解析完成！\n&quot;)
                    return result
                    
                except Exception as e:
                    print(f&quot;解析失败: {str(e)}&quot;)
                    raise

# 创建客户端实例
client = MinerUClient(mcp_url=MCP_SERVER_URL)
print(&quot;客户端创建成功！&quot;)
```

```
MinerU 客户端已初始化
服务器地址: http://192.168.110.131:8001/mcp
客户端创建成功！
```

在实际运行之前，我们需要准备一个 PDF 文件用于测试。

```
# 使用 LangChain Agent 自动调用 MCP 工具

from pathlib import Path

print(&quot;=&quot; * 80)
print(&quot;创建 LangChain Agent 自动调用 MCP 工具&quot;)
print(&quot;=&quot; * 80)

# 1. 加载 MCP 工具
print(&quot;\n加载 MCP 工具...&quot;)
async with streamablehttp_client(**server_params) as (read, write, _):
    async with ClientSession(read, write) as session:
        await session.initialize()
        mcp_tools = await load_mcp_tools(session)
        
        print(f&quot;已加载 {len(mcp_tools)} 个 MCP 工具&quot;)
        for tool in mcp_tools:
            print(f&quot;   - {tool.name}: {tool.description[:60]}...&quot;)
        
        # 2. 直接使用工具解析 PDF
        print(&quot;\n&quot; + &quot;=&quot; * 80)
        print(&quot;使用 parse_documents 工具解析 PDF&quot;)
        print(&quot;=&quot; * 80)
        
        pdf_file_path = &quot;./course.pdf&quot;
        pdf_file = Path(pdf_file_path)
        
        if not pdf_file.exists():
            print(f&quot;文件不存在: {pdf_file_path}&quot;)
        else:
            print(f&quot;\n文件: {pdf_file.name}&quot;)
            print(f&quot;大小: {pdf_file.stat().st_size / 1024:.2f} KB&quot;)
            
            # 找到 parse_documents 工具
            parse_tool = None
            for tool in mcp_tools:
                if tool.name == &quot;parse_documents&quot;:
                    parse_tool = tool
                    break
            
            if not parse_tool:
                print(&quot;未找到 parse_documents 工具&quot;)
            else:
                print(&quot;\n调用 parse_documents 工具...&quot;)
                
                try:
                    # 调用工具，可以指定 backend 和 enable_ocr
                    result = await parse_tool.ainvoke({
                        &quot;file_sources&quot;: str(pdf_file.absolute()),
                        &quot;backend&quot;: &quot;vlm-vllm-async-engine&quot;,  # 可选: pipeline, vlm-transformers, vlm-vllm-engine, vlm-http-client
                        &quot;enable_ocr&quot;: True,  # 启用 OCR，对应 parse_method=&quot;ocr&quot;
                        &quot;language&quot;: &quot;ch&quot;,  # 文档语言
                        &quot;device&quot;: &quot;cuda:3&quot;
                    })
                    
                    print(&quot;解析完成！\n&quot;)
                    
                    # 处理结果 - 正确解析 JSON 结构
                    import json
                    import urllib.parse
                    
                    if isinstance(result, str):
                        # 尝试解析 JSON 字符串
                        try:
                            result = json.loads(result)
                        except:
                            content = result
                            result = None
                    
                    if isinstance(result, dict):
                        if result.get(&quot;status&quot;) == &quot;success&quot;:
                            # 检查是否有 content 字段（直接返回）
                            content = result.get(&quot;content&quot;, &quot;&quot;)
                            
                            # 如果没有 content，检查 result 字段（嵌套结构）
                            if not content:
                                result_data = result.get(&quot;result&quot;, {})
                                if result_data:
                                    results_dict = result_data.get(&quot;results&quot;, {})
                                    if results_dict:
                                        # 提取第一个结果（即使键是 URL 编码的）
                                        # results_dict 的键可能是 URL 编码的中文文件名
                                        for encoded_key, file_result in results_dict.items():
                                            md_content = file_result.get(&quot;md_content&quot;, &quot;&quot;)
                                            if md_content:
                                                content = md_content
                                                # 解码文件名（用于日志）
                                                try:
                                                    decoded_name = urllib.parse.unquote(encoded_key)
                                                    print(f&quot;   解码文件名: {decoded_name}&quot;)
                                                except:
                                                    pass
                                                break
                            
                            # 如果还是没有，检查是否有 results 列表
                            if not content:
                                results_list = result.get(&quot;results&quot;, [])
                                if results_list:
                                    content = results_list[0].get(&quot;content&quot;, &quot;&quot;)
                        elif result.get(&quot;status&quot;) == &quot;error&quot;:
                            print(f&quot;解析失败: {result.get(&#x27;error&#x27;)}&quot;)
                            content = None
                        else:
                            content = str(result)
                    elif result is None:
                        # 已经是字符串了
                        pass
                    else:
                        content = str(result)
                    
                    if content:
                        print(&quot;=&quot; * 80)
                        print(&quot;解析结果统计&quot;)
                        print(&quot;=&quot; * 80)
                        print(f&quot;总字符数: {len(content):,}&quot;)
                        print(f&quot;总行数: {content.count(chr(10)) + 1:,}&quot;)
                        
                        # 保存结果到当前工作目录
                        import os
                        current_dir = Path(os.getcwd())
                        output_file = current_dir / f&quot;{pdf_file.stem}_parsed.md&quot;
                        output_file.write_text(content, encoding=&#x27;utf-8&#x27;)
                        
                        print(f&quot;\n结果已保存到: {output_file.absolute()}&quot;)
                        print(f&quot;当前工作目录: {current_dir.absolute()}&quot;)
                        print(f&quot;文件大小: {output_file.stat().st_size / 1024:.2f} KB&quot;)
                        
                        # 显示预览
                        print(&quot;\n&quot; + &quot;-&quot; * 80)
                        print(&quot;解析结果预览（前 1500 字符）:&quot;)
                        print(&quot;-&quot; * 80)
                        print(content[:1500])
                        if len(content) &gt; 1500:
                            print(&quot;\n... (后续内容已省略)&quot;)
                        print(&quot;-&quot; * 80)
                        
                        print(&quot;\n&quot; + &quot;=&quot; * 80)
                        print(&quot;成功！&quot;)
                        print(&quot;=&quot; * 80)
                    else:
                        print(&quot;解析结果为空&quot;)
                        
                except Exception as e:
                    print(f&quot;调用失败: {e}&quot;)
                    import traceback
                    traceback.print_exc()
```

```
================================================================================
创建 LangChain Agent 自动调用 MCP 工具
================================================================================

加载 MCP 工具...
已加载 2 个 MCP 工具
   - parse_documents: 统一接口，将文件转换为Markdown格式。支持本地文件和URL，会根据USE_LOCAL_API配置自动选择合适的处理...
   - get_ocr_languages: 获取 OCR 支持的语言列表。

Returns:
    Dict[str, Any]: 包含所有支持的OCR语言列表...

================================================================================
使用 parse_documents 工具解析 PDF
================================================================================

文件: course.pdf
大小: 1391.70 KB

调用 parse_documents 工具...
解析完成！

   解码文件名: course
================================================================================
解析结果统计
================================================================================
总字符数: 12,693
总行数: 364

结果已保存到: /home/MuyuWorkSpace/course_parsed.md
当前工作目录: /home/MuyuWorkSpace
文件大小: 16.40 KB

--------------------------------------------------------------------------------
解析结果预览（前 1500 字符）:
--------------------------------------------------------------------------------
# 五、项目：PaddleOCR-VL构建多模态RAG系统

本节内容，我们将详细介绍如何部署和运行这个基于PaddleOCR-VL的多模态AgenticRAG智能问答系统。该系统支持复杂PDF文档、图片、表格、公式等多种格式的智能分析和问答，并具备精准的溯源能力。

![](images/8b615bdc06de102e742913e121d91c5ec9905e922f853a2a988eebefa8ba496f.jpg)

# 5.1 项目核心模块代码详解

PaddleOCR-VL的输出格式非常适合构建多模态RAG系统，如果想要明确的区分出图像、表格、普通文本等信息，一个基本的处理流程是这样的：

```txt
PaddleOCR-VL JSON 输出↓
```

1. 数据预处理

```txt
按block_order排序过滤无用内容（footer等）合并相邻同类型块
```

2. 分类处理

```txt
文本类  $\rightarrow$  标准 chunk 表格类  $\rightarrow$  结构化提取  $+$  文本描述 公式类  $\rightarrow$  保留格式  $+$  语义转换 图片类  $\rightarrow$  多模态向量 / 标题关联
```

3. 元数据增强

```txt
- block_id（溯源ID）
- block_bbox（位置坐标）
- block_type（元素类型）
- page_index（页码）
- 上下文信息（前后标题、图表编号）
```

4. 向量化与索引

```txt
文本 Embedding  
表格 Embedding（多策略）建立多级索引
```

5. 检索与溯源

语义检索  
坐标定位  
可视化标注

分享 | 下载(195.7M) | 删除 : 更多

全部文件/木羽公开课资料/20251022基于PaddleOCR-VL搭建企...

![](images/409c2892525832b5465b80864b23240f91674807f62fbc6dfa25335a5dee2b19.jpg)

![](images/29ea9762cc653c55e580471be648e41292bf6730e1bf1696a59f07dcdafdfe26.jpg)

首先第一步做的就是完成PaddleOCR-VL模型的接入及实现解析过程。核心代码文件为:

![](images/01aea784451e2cca4b44b64bbb05f9d9998fe4232ea886bf087b8b4157736285.jpg)

这个服务的核心流程是：初始化时异步加载 PaddleOCRVL 模型到，然后在 parse_document() 中通过线程池执行阻塞的 OCR 调用，PaddleOCR 会将文档解析为多页结果并保存为 JSON/Markdown/可视化图片到磁盘。服务优先从生成的 *_res.json 文件中读取每页的 parsing_res_list，将其中的每个 block（包含 block_id、block_label、block_content、block_bbox、block_order 等字段）转换为 ParsingBlock 对象，最后通过 calculate.stats() 按 label 关键词（table/image/formula/其他）统计各类型块的数量并返回 DocumentStats。即：

1. 初始化  $\rightar

... (后续内容已省略)
--------------------------------------------------------------------------------

================================================================================
成功！
================================================================================
```

同时也可以看到后端的服务运行情况：

![](img/1f5657fa7ef5a50924de6d2b3b5e0209.png)

**我们下期公开课，再见！** 👋