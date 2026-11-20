
# Part 1. 智能文档审核Agent技术架构概览

> 来源：https://kq4b3vgg5b.feishu.cn/docx/RloDdeBrjoEXfLxh07gc5mHvnff

## 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》(12月班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(12月班)

---

## 从技术选型到产品落地

本期公开课，我们将深入探索基于大语言模型的文档审核Agent技术方案。

### 文档审核类Agent系统功能速览

核心功能一：支持`.pdf`，同时也支持快速扩展`.doc`、`.docx`、`.ppt`、`.pptx`、`.png`、`.jpg`、`.jpeg`等多种格式的文档在线上传与解析；

![](img/587c9984f1bbcd1069c2399423c4f9bc.png)

核心功能二：可以针对结构化数据高精准度提取文本内容和位置信息，并进行高亮展示

![](img/13d39105586798c7825e1e302035540e.png)

核心功能三：支持人机交互功能，可以由人工随时介入审核流程，并给出审核意见

![](img/b5e9df21c3d75af460fdbed7c8df2e0b.png)

核心功能四：针对单个文件支持在线自定义规则，并实时发起重新审查，并实现持久化数据管理

![](img/7f99d40f194310fabf16572b26ebf4f6.png)

当然，该智能文档审核系统的研发我们需要应用到核心的Agent开发技术，主要由如下技术模块组成：

![](img/dc3ddd8c3cfbbe40e28e19ffd2129dbe.png)

## 一、文档审核类AI成熟落地产品介绍

首先看一组数据，如下论文中是对法律大模型与传统法律合同审查员、初级律师和法律流程外包商进行了开创性的比较，并深入剖析了大模型在合同审查的准确性、速度和成本效益方面是否能够超越人类。而最终的实验结果表明，先进的大模型在确定法律问题方面的准确性能够达到甚至超过人类。在速度方面，大模型只需几秒钟即可完成审查，远超人类所需的数小时。在成本方面，大模型的运行成本仅为传统方法的几分之一，成本降低了惊人的 99.97%。

![](img/0ab07bf5ac3ea514212d36283db84c9f.png)

> 论文地址：`https://arxiv.org/pdf/2401.16212`

随着大模型能力的提升，将其作为智能代理用于专业文档的合规性审核已经从理论变为现实，并且在多个行业中展现出了惊人的效率提升。

**文档合规审核指的是根据法律法规、行业规范或企业内部规则，对各种专业文件进行内容和格式上的检查，以发现潜在的违规或缺陷之处。典型场景包括：合同文档的法律合规审核、财务票据的规范校验、以及标书、公文等特定格式文件的规则符合性检查。以下是我们国内已经落地的在文档审核类产品中大模型的落地场景：**

+ **阿里的通义法睿**：可以用于快速识别合同潜在风险，并提供专业的风险评估和修改建议。体验地址：`https://tongyi.aliyun.com/farui/review`

![](img/045e40ff2eaaa6bfd1b36b80c87ac7ad.png)

+ 百度的财务、医疗、教育票据OCR识别及自动审查：`https://ai.baidu.com/tech/ocr_receipts/vat_invoice`

![](img/3c8e7ce90a46f20bf35f5889cd745a50.png)

+ 语核科技的数字员工（文档审查方向）：`https://langcore.cn/zh`

![](img/a6e52796ae6f32a64f11696eecab8ccd.png)

关于上述审核系统的复现，在我的往期公开课中有非常详细的说明：

![](img/fe891dccef0b2ad092789976883e7d57.png)

项目的前后端源码已经上传到百度网盘中，大家可以扫码免费下载：

![](img/009a4c5bd6b391a1fb13babb1a03e214.png)

![](img/8dcd078dce2f6e36071137a2f0e8f576.png)

诸如此类的文档合规审核Agent是指基于大语言模型构建的智能代理系统，能够根据法律法规、行业规范或企业内部规则，自动对专业文件进行内容和格式上的检查，发现潜在的违规或缺陷之处，从而避免人工审核耗时耗力等诸多问题。同时也正是在这样的需求痛点下，文档审核类Agent目前主要应用在如下经典场景下：

| 文档审核Agent核心应用场景 |  |  |  |
| --- | --- | --- | --- |
| 场景类型 | 审核内容 | 核心价值 | 难度等级 |
| **法务合同审核** | 必要条款检查、法律风险识别、措辞合规性 | 降低法律风险、提升审核效率 | ⭐⭐⭐⭐⭐ |
| **财务票据校验** | 发票真伪、金额计算、政策合规 | 减少财务错误、加速报销流程 | ⭐⭐⭐⭐ |
| **标书公文审核** | 格式规范、必备内容、章节完整性 | 提高中标率、确保公文质量 | ⭐⭐⭐ |

之所以能够实现上述复杂工作流程的原因在于基于大模型构建而成的Agent具备自主决策和工具调用能力：

1. **自主推理**：不只是执行预设规则，而是能理解文档语义、推理条款间的逻辑关系
2. **工具编排**：可以主动调用外部工具（如计算器、数据库查询、API接口）来辅助判断
3. **知识检索**：能从知识库中检索相关法规、案例，基于证据给出审核意见
4. **链式思考**：像人类专家一样，分步骤、有逻辑地完成复杂审核任务

![](img/305c51f1dbbea476bf4229f05cb4e335.png)

> 论文地址：`https://arxiv.org/pdf/2501.09136`

在合同、票据/收据、长篇公文等场景中，“大模型 + 工具调用 + 外部知识检索 + 规划/反思（Agentic模式）” 能把抽取、核对与基于证据的判断串成多步流程，用于自动化或半自动的合规审核。法务合同有 `CUAD` 这一权威标注集支撑条款级审核可行性；多模态文档 AI（如 `LayoutLMv3` / `LayoutLLM`）显著提升了票据/表单/`.pdf` 的结构化与问答能力；而 `Agentic RAG`/`RA-LLM` 则把检索、工具使用与多步推理纳入可控工作流。

同样，针对上图中的文档审核类的实现方案，如果进一步拆分则可以拆解为如下三大核心技术模块：

![](img/3b382c8517511c65873ef8fcecf5dc03.png)

而如果再进一步拆解，则如下图所示：

![](img/f7c466d8ad38f3240aec519f84ba9a61.png)

其中：

+ **解析与结构化**：主流 Document AI 都把 OCR/布局/键值对/表格抽取到统一 `JSON`，并附带置信度与坐标，便于后续规则与证据回链。

+ **规则与知识**：企业审核离不开**可配置规则引擎**（版本化/审计）与**`RAG`**（把法规与制度做成可检索、可引用的知识源），并通过混合检索+重排序提升命中与可溯源性。

+ **Agent 编排**：用 `LangGraph`/`LangChain` 等做计划-工具调用-记忆-长流程编排，并把人类在环作为低置信度的兜底环节。

+ **评估与治理**：上线后需要字段级与 RAG 两路评估、Tracing/监控，以及 PII/审计合规治理，形成持续改进闭环。

所以一个相对比较健全的文档审核类Agent的实现，不仅需要我们掌握 Agent 及 RAG 的底层技术原理，还需要我们掌握多模态大模型、多 Agent 协作、Prompt 工程、Agent 编排等技术。综合起来是相对较难的落地场景。

此外，对于文档的精准解析是文档审核类Agent落地的关键，也主要分为OCR和VLM 两条实现链路，通过传统 OCR +规则的方法逐步转到现在基于 VLM-based 方法的革新，可以在不同场景下发挥各自的优势。

![](img/af7b6d6ddfd87dde43322d2e449e10a2.png)

> 技术综述地址：`https://www.firstsource.com/insights/whitepapers/document-processing-with-vlm`

我们本次项目使用的：`MinerU`是一个非常典型的基于管道的解决方案 (`Pipeline`-based solution)，并且是一个开源文档解析项目，一共四个核心组件，通过`Pipeline`的设计无缝衔接，实现比较高效、准确的文档解析。如下图所示：（下图来源官方论文：`https://arxiv.org/pdf/2409.18839v1`）

![](img/8b53cb4132f9ecf45dc2eabf4ea51343.png)

`MinerU`的主要工作流程分为以下几个阶段：

1. **输入**：接收`.pdf` 格式文本，可以是简单的纯文本，也可以是包含双列文本、公式、表格或图等多模态`.pdf`文件；
2. 文档预处理（Document Preprocessing）：检查语言、页面大小、文件是否被扫描以及加密状态；
3. 内容解析（Content Parsing）：
   + 局分析：区分文本、表格和图像。
   + 公式检测和识别：识别公式类型（内联、显示或忽略）并将其转换为 `LaTeX` 格式。
   + 表格识别：以 `HTML`/`LaTeX` 格式输出表格。
   + OCR：对扫描的 `.pdf` 执行文本识别。
4. 内容后处理（Content Post-processing）：修复文档解析后可能出现的问题。比如解决文本、图像、表格和公式块之间的重叠，并根据人类阅读模式重新排序内容，确保最终输出遵循自然的阅读顺序。
5. 格式转换（Format Conversion）：以 `Markdown` 或 `JSON` 格式生成输出。
6. **输出（Output）**：高质量、结构良好的解析文档。

目前在GitHub上，`MinerU` 的Star数为48.2K，Fork数为4K，拥有非常良好的社区支持和活跃的贡献者，且一直处于active development状态。

![](img/ab22aa6934c0ba17cfc0f8c504010051.png)

`MinerU` 提供了在线Demo 页面，我们可以直接线进行测试。试用地址：`https://opendatalab.com/OpenSourceTools/Extractor/PDF/`

![](img/e6c9b519c0ce734a19c4cf6fd4f586a7.png)

同时，`MinerU` 项目于2024年07月05日首次开源，底层主要是集成 `PDF-Extract-Kit` 开源项目做`.pdf`的内容提取，`PDF-Extract-Kit`同样是一个开源项目：`https://github.com/opendatalab/PDF-Extract-Kit`

![](img/b06c3722ea68dc7b8fe57f0b466351ab.png)

`PDF-Extract-Kit`这个项目主要针对的是`.pdf`文档的内容提取，通过集成众多SOTA模型对`.pdf`文件实现高质量的内容提取，其中应用到的模型主要包括：

| 模型类型 | 模型名称 | GitHub 链接 | 模型下载链接 | 任务描述 |
| --- | --- | --- | --- | --- |
| 布局检测模型 | `LayoutLMv3` / `DocLayout-YOLO` | [GitHub](https://github.com/microsoft/unilm/tree/master/layoutlmv3) / [GitHub](https://github.com/opendatalab/DocLayout-YOLO) | [模型下载](https://huggingface.co/microsoft/layoutlmv3-base-chinese) / [模型下载](https://huggingface.co/juliozhao/DocLayout-YOLO-DocStructBench/tree/main) | 定位文档中不同元素位置：包含图像、表格、文本、标题、公式等 |
| 公式检测模型 | `YOLO` | [GitHub](https://github.com/ultralytics/ultralytics) | [模型下载](https://github.com/ultralytics/assets/releases/download/v8.3.0/yolo11n.pt) | 定位文档中公式位置：包含行内公式和行间公式 |
| 公式识别模型 | `UniMERNet` | [GitHub](https://github.com/opendatalab/UniMERNet) | [模型下载](https://huggingface.co/wanderkid/unimernet_base) | 识别公式图像为`LaTeX`源码 |
| 表格识别模型 | `StructEqTable` | [GitHub](https://github.com/Alpha-Innovator/StructEqTable-Deploy?tab=readme-ov-file) | [模型下载](https://huggingface.co/U4R/StructTable-InternVL2-1B/tree/main) | 识别表格图像为对应源码（`LaTeX`/`HTML`/`Markdown`） |
| OCR模型 | `PaddleOCR` | [GitHub](https://github.com/PaddlePaddle/PaddleOCR) | [模型下载](https://paddlepaddle.github.io/PaddleX/latest/module_usage/tutorials/ocr_modules/text_detection.html) | 提取图像中的文本内容（包括定位和识别） |

`MinerU` 既支持`Pipeline`，也支持 VLM 模式：

![](img/c45e9be8852704931d4bf13153553aed.png)

`VLM-Transformer`则是直接利用`transformers`库中的Vision-Language模型处理图像+文本的多模态输入，其流程如下：

![](img/b14375bc41d2038c366a2f6c8ed9cfbf.png)

而`VLM-Sglang`后端则是利用`sglang`高性能推理引擎，优化了GPU加速及分布式部署的基础上同时支持实时流式输出，其流程如下：

![](img/7649806d3c3260549be33f1aa34fb2ad.png)

其中对于VLM模式：

![](img/d6aa793fef11ea7d70f96b802b765af8.png)

+ `Pipeline`模式：

![](img/ea72b7e44d80de8ceb36cd83855fd1fd.png)

## 二、智能文档审核Agent本地部署

首先，大家可以在百度网盘中下载到完整的项目源码：

![](img/54b416770d8c62f792450c6c01ac5949.png)

![](img/ded9168511d1f8224dcd4a4cbb924c14.png)

下载后上传到服务器，并解压项目包：

```
unzip ai-document-review.zip
cd ai-document-review
```

我给大家做了一键安装的脚本，可以快速部署项目运行的前后端依赖及服务启动，首先：

```
chmod +x install.sh start.sh stop.sh
```

![](img/5aceed6b8220c7a0a17cc6caf8a94963.png)

然后执行安装脚本：

```
./install.sh
```

![](img/35047ccc1f8df9cade473cc65ead20c3.png)

安装完成后，在启动前配置环境变量：

1. DeepSeek API Key：`https://platform.deepseek.com/`访问 DeepSeek 开放平台，注册账号后获取 API Key；
2. **MinerU API Key**：访问 `https://mineru.net/`，需要先进行API申请，然后创建 Token 即可。

```
vim .env
```

![](img/b36c0c3082216ac06f481bbb1565d314.png)

配置完成后，执行启动脚本：

```
./start.sh
```

![](img/9d35df6e1fb773fb6219bace2c1ff8e2.png)

启动完成后，在浏览器中打开：`http://localhost:5173`

![](img/9d278f034cd655ca319c27fa4811c89a.png)

同时也可以查看到 API 文档：`http://localhost:8000/docs`

![](img/a7eafe4b16e1393612e744ae9a22cf8c.png)

至此，我们就完成了智能文档审核Agent的本地部署，大家便可以开始上传文档进行审核功能的应用了。
