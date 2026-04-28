# llama_factory × Qwen3-VL：垂直领域多模态模型微调实战

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Wm0Odl9ZzoeM3QxIXgkc8csCnUf](https://kq4b3vgg5b.feishu.cn/docx/Wm0Odl9ZzoeM3QxIXgkc8csCnUf)

## 课程说明：

+   体验课内容节选自《2025大模型Agent智能体开发实战》(秋季班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(秋季班)

![](img/178adc33a029cf81da37adfff1269c51.png)

![](img/89bdbf08073e3d8b4870ec59ae177e3d.png)

data_create是生成微调数据的程序，大家可以通过它自主进行数据集构建，由于格式受限，需要的伙伴扫码添加助教老师领取～

---

# 《大模型Agent开发实战》（体验课）

# 垂直领域多模态模型微调实战

# 1. 什么是多模态模型

## 为什么要有多模态大模型

**传统的大模**型（比如 ChatGPT、通义千问、文心一言）本质上是文本大模型。

它们只能“看懂文字”和“生成文字”，对图片、声音、视频等信息是“看不见也听不懂”的。

但是现实世界中，大多数信息并不是文字：

+   一张图片、一段视频、一段语音，都包含了大量的语义信息；

+   许多任务无法仅靠文字完成，比如图表理解、视频分析、语音转文字。

**因此，为了让人工智能能够更接近人类的理解方式，研究**者提出了多模态大模型（Multimodal Large Language Model, MLLM）。

它能同时处理多种形式的信息：文字、图像、语音、视频等，让AI不仅能“读”，还能“看”“听”“理解”。

## 什么叫“模态”

“模态”指的是**信息的表现形式或感知方式**。

人类有视觉、听觉、语言等感知通道；在机器世界里，它们对应为不同的数据模态：

| 信息类型 | 模态名称 | 举例 |
| --- | --- | --- |
| 文字 | 文本模态 | 一段文字、代码、网页内容 |
| 图像 | 视觉模态 | 一张照片、一张图表 |
| 声音 | 音频模态 | 语音录音、音乐 |
| 视频 | 视频模态 | 一段会议录像 |
| 传感器数据 | 其他模态 | 位置信息、深度图等 |

**单模态模型****只能处理其**中一种。

多模态模型能理解并融合两种或多种模态的信息。

## 与文本大模型的对比理解

| 对比维度 | 文本大模型（LLM） | 多模态大模型（MLLM） |
| --- | --- | --- |
| 输入形式 | 只能输入文字 | 可以输入文字、图片、音频、视频等 |
| 输出形式 | 输出文字 | 可以输出文字，也可以生成图片、声音、视频 |
| 感知方式 | 语言理解 | 语言 + 视觉 + 听觉等多通道理解 |
| 数据来源 | 大规模文本语料 | 图文、音视频、图表等多模态数据 |
| 典型任务 | 对话、写作、代码生成 | 看图问答、图片描述、视频分析、语音理解 |
| 理解深度 | 语言推理 | 跨模态推理、视觉语义理解 |
| 代表模型 | GPT-3、Qwen2、Baichuan | GPT-4V、Gemini、Qwen-VL、DeepSeek-VL |

一句话概括：

文本大模型能“理解语言”；

多模态大模型能“理解世界”。

## 多模态大模型的基本原理

一个多模态大模型的处理流程可以分为三步：

1.  **特征提取**：
把不同类型的输入（文字、图片、语音）通过各自的“编码器”转化为向量形式，
例如：

+   文本由语言模型编码；

+   图像由视觉模型提取特征；

+   音频由声学模型提取特征。

1.  **模态对齐与融合**：
不同模态的数据特征维度和结构不同，模型会把它们映射到一个统一的语义空间中，
让“图像中的猫”和“文字中的猫”在这个空间里意义接近。

1.  **统一推理与生成**：
模型在这个融合后的语义空间中进行推理，可以输出文字，也可以输出图像、声音或视频。

## 实例

1.  **图像理解（看图说话）**
输入一张照片，模型输出一段文字描述，例如：“一只猫趴在桌子上。”
—— 输入模态是“图像”，输出模态是“文字”。

1.  **文档识别**
输入一张扫描的发票或合同，模型输出提取出的文字内容或关键信息。
—— 输入模态是“图像”，输出模态是“文字”。

1.  **视频理解**
输入一段视频，模型输出摘要或关键事件说明。
—— 输入模态是“视频”，输出模态是“文字”。

1.  **语音转写与分析**
输入语音，输出对应的文字或情绪判断。
—— 输入模态是“音频”，输出模态是“文字”。

1.  **文本生成图像**
输入一句文字，如“夕阳下的城市街道”，输出一张对应的图片。
—— 输入模态是“文字”，输出模态是“图像”。

## 为什么它重要

1.  **让AI具备更全面的感知能力**
现实中的信息是多源的，只有理解不同模态的组合，AI才能真正接近人类的认知方式。

1.  **应用场景更丰富**
从工业质检、医学影像、办公自动化到教育、设计、媒体生产，多模态模型的应用空间远大于纯语言模型。

1.  是迈向通用人工智能（AGI）的关键一步
真正的通用智能必须能听、能看、能理解环境，而不仅仅是理解语言。

## 代表模型及其定位

| 模型名称 | 机构 | 特点 | 应用方向 |
| --- | --- | --- | --- |
| CLIP | OpenAI | 图文语义对齐 | 图像搜索、相似度判断 |
| BLIP / BLIP-2 | Salesforce | 图像理解与描述 | 看图问答、图像描述 |
| LLaVA | UW / Microsoft | 视觉 + 语言问答 | 图像内容问答 |
| Qwen-VL / Qwen3-VL | 阿里 | 中文视觉语言融合 | 文档问答、图表理解 |
| DeepSeek-OCR / VL | DeepSeek | 图文识别与结构化 | OCR、发票识别、报表理解 |
| GPT-4V / Gemini | OpenAI / Google | 综合多模态推理 | 看图、视频理解、语音交互 |

## 总结

多模态大模型是人工智能从“语言智能”走向“感知智能”的关键阶段。

它不仅能“理解文字”，还能“理解图像、声音和视频”，实现跨模态的信息融合与推理。

用一句话来概括：

> 文本大模型理解的是语言世界；

> 多模态大模型理解的是现实世界。

# 2.多模态模型与OCR模型核心区别

## 传统 OCR 模型与多模态模型的区别

### 1. 传统 OCR 模型

+   **核心任务**：把图片中的文字识别出来。

+   **典型流程**：文字检测 → 文字识别 → 版面或表格还原。

+   **输入输出**：输入是图片，输出是纯文字或表格数据。

+   **目标**：保证识别准确率、版面还原度和速度。

+   **特点**：

+   视觉处理非常强，但语义理解能力弱。

+   不理解文字含义，只负责“看清楚、读出来”。

+   适合票据、发票、合同等结构固定的文档。

简单来说：传统 OCR 会读字，但不会理解字在说什么。

它是一个视觉任务，不具备跨模态理解能力。

### 2. 多模态模型

+   **核心任务**：让模型能同时理解图片、文字、语音、视频等不同模态的信息。

+   **典型结构**：视觉编码器 + 语言模型，通过模态对齐让模型能“看图说话”或“图文推理”。

+   **输入输出**：输入可能是图像 + 文字；输出可能是回答、总结或新的图像。

+   **目标**：在多个模态之间进行语义理解与推理。

简单来说：多模态模型不止能读图，还能理解图的含义。

### 3. OCR 算不算多模态？

+   从任务上讲：OCR 确实是图像 → 文字，是一种“模态转换”任务。

+   但从模型性质上讲：

+   传统 OCR 只在“视觉”到“文字”之间做固定映射，

+   没有统一语义空间，也不具备跨模态推理。

所以结论是：

> OCR **任务是多模态的，但传统 **OCR 模型不属于多模态大模型。

> 它是一个专用型视觉-文字识别模型，而非通用 VLM。

## DeepSeek-OCR-VL 与传统 OCR 及 VLM 的区别

### 1. DeepSeek-OCR-VL 的定位

+   **采用“视觉-语言”架构思想，**但核心目标仍是高效文档 OCR 与视觉压缩。

+   能把整页文档视觉内容编码成少量“视觉 token”，再恢复文字。

+   设计重点是：高吞吐、高还原率、长文档处理能力。

### 2. 与传统 OCR 的区别

| 维度 | 传统 OCR | DeepSeek-OCR-VL |
| --- | --- | --- |
| **处理流程** | 检测-识别-后处理，多模块拼接 | 视觉编码 + 语言解码，端到端生成 |
| **能力侧重** | 精确读字、版面还原 | 页级识别、高压缩率、长上下文 |
| **语义理解** | 弱，仅识别 | 一定程度上能理解上下文布局 |
| **输出形态** | 文字/结构化字段 | 文字，可供上层 LLM 理解 |
| **应用角色** | 独立 OCR 系统 | 上游 OCR 引擎或数据压缩模块 |

可以理解为：

> DeepSeek-OCR-VL 不是普通 OCR，而是用 VLM 技术重写的 OCR 引擎。

> 它依然专注“读文档”，但方式更智能、更高效。

### 3. 与通用 多模态模型（如 DeepSeek-VL、GPT-4V）的区别

| 维度 | DeepSeek-OCR-VL | DeepSeek-VL / GPT-4V |
| --- | --- | --- |
| **目标** | 高效 OCR 识别、光学上下文压缩 | 通用视觉-语言理解与推理 |
| **输入类型** | 文档图片 | 任意图片、图表、网页、视频 |
| **输出类型** | 文本内容 | 文本回答、总结、推理结果 |
| **是否开放问答** | 否，只读出文字 | 是，可对内容进行语义问答 |
| **定位** | 专用 OCR 模型（VLM 思路） | 通用多模态大模型 |

所以它**属于多模态家族**，但**不是通用多模态大模型**。

在多模态体系里，它是视觉-语言技术路线下的专用 OCR 分支模型。

## 结论

1.  传统 OCR 模型：只会识别文字，不具备理解能力，不属于多模态大模型。

1.  多模态模型：能理解图、文等多模态信息，能推理与生成。

1.  DeepSeek-OCR-VL：

+   技术上采用 VLM 框架；

+   目标仍是 OCR 任务；

+   因此是“**VLM 思路的 OCR 模型**”，介于传统 OCR 与通用 VLM 之间。

一句话总结：

> **D**eepSeek-OCR-VL 不是传统 OCR，但也不是通用多模态大模型；它是基于 VLM 架构的高效 OCR 专用模型。

![](img/994826a0b73eec6f5729909b203286b9.png)

# 3. 为什么要微调垂直行业的多模态模型

## 为什么“通用多模态模型”还需要微调？

通用多模态模型（如 GPT-4V、Gemini、Qwen-V**L、D**e**epSeek**-VL）在设计时追求广覆盖和通用理解能力。

它们的训练数据来自互联网的大规模图文配对语料、公开文档、网页截图、通用表格、示意图等。

这种训练方式带来两个特征：

1.  模型具备**基本的视觉理解能力**；

1.  但对特定行业的**专业格式与语义缺乏精准认知**。

举个简单例子：

你给 G**PT-4V** **一张银行**对账单或财务报表，它能看出有表格、有数字，但不清楚“借方”“贷方”“本期余额”之间的业务含义。

所以，虽然它“能看”，但“看得不对或看得不准”。

## 垂直领域的独特挑战

每个行业的视觉数据都有自己的“语言体系”：

+   医疗影像报告：指标、缩写、单位极为专业；

+   财务报表：同样的单词在不同报表含义不同（如“Total”可能是列汇总也可能是行汇总）；

+   票据、合同：格式、印章、字段名称变化极大；

+   工业检测、建筑图纸：视觉特征复杂，非自然图像；

+   教育试卷、表格：结构嵌套深，字体多样，扫描噪声高。

这些都不是通用模型训练时重点覆盖的场景。

因此模型虽然能看懂“图像”，但不懂“这类图像的语义逻辑”。

## 微调的真正目的：从“感知”走向“理解”

多模态模型的基础能力来自于两个环节：

1.  视觉编码器（Vision Encoder）负责“看”；

1.  语言模型（LLM）负责“说”。

而在垂直场景中，问题往往出在**两者之间的理解鸿沟**。

微调的目标就是让模型在你的领域语境下重新学习“如何把看到的内容转成正确语义”。

更具体地讲，微调带来三层能力提升：

### （1）语义对齐

模型学会特定领域里的术语、图例、单位、符号含义。

例子：

+   医学报告中的“↑”“↓”代表高低值，而不是箭头符号。

+   报表中“△%”表示环比增长，而不是数学符号。

这种“图像符号 → 专业语义”的映射是通用模型不具备的，需要通过领域样本训练建立。

### （2）结构化理解

通用模型往往能识别表格单**元，但不**知**道它们之**间的逻辑关系。

微调让模型学习特定行业的布局规律和层级关系。

例如：

+   在财务报表中，“利润总额”下的每一项是子项；

+   在检验报告中，每一行指标都由“项目名 + 数值 + 单位 + 参考范围”组成。

经过微调，模型可以直接输出带结构的结果，如 JSON、表格，而不仅是自然语言描述。

### （3）视觉稳健性

垂直领域的图片往往存在以下问题：

+   扫描模糊、光线不均、倾斜；

+   模板格式多、字体变化大；

+   噪声、印章、手写标注。

通用模型在这些场景下容易识别错误。

通过微调可以让模型接触更多“本领域的真实样本”，

从而显著提升在这些特殊视觉条件下的稳定性。

## 报表识别场景：为什么微调能带来“更准确”与“更实用”

以报表识别为例，通用模型和微调模型的区别体现在三个层面：

| 层面 | 通用多模态模型 | 微调后的垂直模型 |
| --- | --- | --- |
| **内容识别** | 能看出有表格、数字、文字 | 准确识别字段、数字、小数点、货币符号 |
| **结构理解** | 只读出内容，不理解行列关系 | 正确输出表头-数据映射关系 |
| **语义判断** | 不知道“本期收入”“去年同期”关系 | 能理解指标含义并生成有逻辑的结构化结果 |

所以，对报表进行微调并不仅仅是“为了更准”，

而是为了让模型从“能看”变成“能理解”“能直接产出可用数据”。

## 微调的工程与应用意义

1.  **可控输出格式**：微调后可稳定输出 JSON 或表格字段，不再出现生成式波动。

1.  **减少人工规则**：无需再写复杂的正则或版面解析逻辑，模型直接识别结构。

1.  **兼容多模板**：模型学会识别各种不同版式的同类文件，减少模板管理成本。

1.  **提升数据质量**：作为数据清洗环节的“视觉入口”，输出结构更统一。

1.  **支撑下游任务**：微调后的模型可直接为知识库、RAG、分析系统提供高质量输入。

## 案例

假设你在做**财务报表智能识别系统**。

+   通用模型 Qwen-VL：能回答“这张图是什么报表”，但不能准确提取金额。

+   微调后的领域模型：能直接输出

```
{&quot;项目&quot;:&quot;主营业务收入&quot;,&quot;本期金额&quot;: 12543000,&quot;上期金额&quot;: 11895000,&quot;增幅&quot;:5.45%}
```

+   并能理解“增幅”是通过两个字段计算得来的。

这就从“语言理解”变成了“结构化智能解析”。

## 总结：微调的意义不只是“更准”，而是“更懂”

| 目标 | 通用多模态模型 | 垂直领域微调模型 |
| --- | --- | --- |
| 任务覆盖 | 广而浅 | 窄而深 |
| 准确度 | 一般，依赖格式 | 高，适应真实文档 |
| 语义理解 | 通用语义 | 领域语义（懂专业词汇） |
| 输出形式 | 生成式回答 | 可控、结构化 |
| 商业可用性 | Demo 级 | 生产级 |

**一句话总结：**

> 微调垂直领域的多模态模型，是让AI从“看懂图像”进化到“理解图像背后的专业语义与业务逻辑”。

> 它的价值不只是提高准确率，更在于让模型真正“可落地、可用、可信”。

![](img/77a75600985e7cc616d250f639cf9ad4.png)

# 4. 主流多模态模型生态与选型建议

## 核心判断逻辑：选模型前先判断三件事

1.  **输入模态是什么？**

+   文字 + 图片？

+   图片 + 图表？

+   视频 + 音频？

+   还是长文档（多页 PDF）？

1.  **目标任务是什么？**

+   看图问答 / 图像描述？

+   文档识别 / 表格结构化？

+   图表解析 / 数值推理？

+   视频摘要 / 多轮交互？

1.  **是否需要私有部署？**

+   能用闭源 API（GPT-4o、Gemini）？

+   还是必须自建落地（Qwen、DeepSeek、InternVL）？

> 根据这三个问题，你基本就能锁定 80% 的选型方向。

## 主流多模态模型生态能力对比表

| 模型 | 模态支持 | 中文能力 | 可私有化 | 代表优势 | 不适合的场景 |
| --- | --- | --- | --- | --- | --- |
| **GPT-4V / GPT-4o** | 文本、图片、语音、视频 | 强（但偏英文） | 否 | 全模态理解、推理最强、图表理解好 | 不能本地化、成本高 |
| **Gemini 1.5 Pro** | 文本、图片、音频、视频 | 中等 | 否 | 视频摘要、跨模态对话好 | 中文OCR较弱、不可控 |
| Qwen-VL / Qwen3-VL | 文本、图片、图表、文档 | 极强 | ✅ | 中文图文理解、表格语义、RAG集成 | 不支持视频、英文弱 |
| DeepSeek-OCR / VL | 图片→文本（OCR特化） | 强 | ✅ | OCR精度高、结构化输出快 | 仅限文档类，通用视觉差 |
| **InternVL2** | 文本、图片、表格 | 强 | ✅ | 中文问答、国产生态、开放源码 | 模型稍慢、视频支持弱 |
| LLaVA-Next / MiniGPT-4 | 文本、图片 | 一般 | ✅ | 框架清晰、易微调 | 中文弱、文档结构理解差 |
| **Yi-VL** | 文本、图片 | 强 | ✅ | 中文语言流畅、稳定 | 生态未成熟、训练资源少 |

## 按场景选模型

| 企业场景 | 推荐模型 | 主要优势 | 为什么选择它 | 不适合用谁 |
| --- | --- | --- | --- | --- |
| **票据、发票、回单类 OCR** | DeepSeek-OCR + 轻量 LLM 后处理 | 识别速度快、版面适配广、结构输出稳定 | 专注OCR领域，吞吐大；可接LLM转结构化 | GPT-4V、LLaVA（识别慢、结构不稳） |
| **财务/经营报表识别** | Dee**pSeek-OCR (上**游) + Qwen-VL (下游) | 精准提取字段并理解语义 | DeepSeek读得准、Qwen理解字段关系强 | LLaVA、InternVL（对中文表格语义弱） |
| **合同/规章/说明书抽取** | **Qwen-VL** 或 **InternVL2** | 中文长文档问答好、能识别结构 | 支持分页理解与跨页问答，可微调私有化 | GPT-4V（贵且不可控）、LLaVA（对中文长文档弱） |
| **图表理解/ChartQA** | Qwen-VL / UniChart / LLaVA-Next | 图表结构解析、数值计算能力强 | 支持图表OCR+语义推理；Qwen对中文图表特化 | DeepSeek-OCR（仅识别不理解） |
| **图像问答/客服识图** | LLaVA-Next / Yi-VL | 轻量、响应快、支持图像语义问答 | 适合嵌入式或在线客服应用 | DeepSeek-OCR（过重） |
| **文档/报告问答（RAG类）** | Qwen-VL + 向量库（Milvus/Faiss） | 可读PDF+理解内容并回答 | 可本地化，中文表现稳定 | GPT-4V（成本高）、LLaVA（长文档弱） |
| **视频摘要/会议纪要** | Gemini 1.5 Pro / GPT-4o | 具备原生视频理解和语音识别 | 当前唯一成熟的视频多模态方案 | Qwen、InternVL（无视频能力） |
| **工业质检/制造视觉** | YOLO / SAM 系列 + Qwen-VL（描述层） | 检测+解释组合，效率高 | CV模型负责检测，VLM负责说明 | GPT-4V（推理慢、成本高） |
| **教育/科研/多模态课程演示** | LLaVA / InternVL2 | 代码开源、可快速训练 | 可展示VLM原理与微调流程 | GPT-4V（闭源） |

## 如果企业要做“自建多模态能力”

| 目标 | 推荐生态组合 | 理由 |
| --- | --- | --- |
| **中文文档/表格智能解析平台** | DeepSeek-OCR（识别） + Qwen-VL（语义） | 可控、可微调、处理量大 |
| **企业知识问答（图文RAG）** | Qwen-VL / InternVL2 + LangChain | 兼容知识库系统，易部署 |
| **结构化数据抽取系统** | DeepSeek-OCR + 微调LoRA | 只调输出头即可，训练快 |
| **多模态AI助手（客服/办公）** | Yi-VL / LLaVA-Next | 模型轻、可内嵌 |
| **视频+语音会议理解** | Gemini 1.5 Pro / GPT-4o（API） | 视频语音模态目前闭源领先 |

## 微调与部署层面的选择思路

| 需求类型 | 微调重点 | 推荐方式 |
| --- | --- | --- |
| OCR/报表结构化 | 视觉投影层 + 输出格式SFT | LoRA/QLoRA |
| 图表问答 | 跨模态对齐层 + 指令数据 | SFT + 数据增强 |
| 合同/文档问答 | 语言层指令微调 | 轻SFT或DPO |
| 工业检测说明 | 结合CV特征层 | 多任务微调 |
| 视频摘要 | 需专门Video Encoder | 建议API托管 |

## 总结一句话

> 多模态模型的选型，核心不是“谁最强”，而是“谁最适合你的场景”：

+   通用理解选 **GPT-4V / Gemini**；

+   中文文档选 Qwen-VL / DeepSeek-OCR；

+   可控可微调选 InternVL / Yi-VL；

> 企业要做的是根据输入模态 **+ 输出目标 + 部**署要求，选择一个最合适、可控、可扩展的多模态生态，而不是盲目追最前沿。

# 5.什么是llama_factory

项目地址https://github.com/hiyouga/LLaMA-Factory

![](img/89594a8b745467d085d30ccd7e380939.png)

## LLaMA Factory 技术架构全貌

### 1 LLaMA Factory是什么？

**简单理解**：

```
LLaMA Factory = 一个&quot;大模型微调工厂&quot;
就像汽车工厂：
- 原材料：各种预训练大模型（LLaMA、Qwen、ChatGLM等）
- 生产线：各种微调方法（SFT、DPO、PPO、ORPO等）  
- 产品：定制化的AI助手
- 操作界面：Web UI + 命令行工具
```

**本质****：LLaMA Fa**ctory是一个高度集成的封装框架，它调用底层包来实现功能，同时提供了统一的界面和配置系统。

### 2 架构层次分析

```
┌─────────────────────────────────────┐
│           用户界面层                 │
│   Web UI + 命令行 + 配置文件           │  
├─────────────────────────────────────┤
│          LLaMA Factory核心层          │
│    统一接口 + 配置管理 + 工作流编排      │
├─────────────────────────────────────┤  
│           算法实现层                 │
│   TRL + Transformers + 自定义算法     │
├─────────────────────────────────────┤
│          计算优化层                  │
│  DeepSpeed + Accelerate + PEFT       │
├─────────────────────────────────────┤
│           基础框架层                 │
│      PyTorch + CUDA + 硬件驱动       │
└─────────────────────────────────────┘
```

## 核心依赖包详细分析

### 1 主要依赖包清单

LLaMA Factory的核心依赖包括：

#### **基础深度学习框架**

```
torch==2.4.0           # PyTorch核心框架
torchvision==0.19.0    # 视觉相关（主要用于依赖）
torchaudio==2.4.0      # 音频相关（主要用于依赖）
```

#### **Hugging Face生态**

```
transformers&gt;=4.41.0    # 模型加载和基础微调
datasets&gt;=2.16.0        # 数据处理和加载
tokenizers&gt;=0.19.0      # 分词器
accelerate&gt;=0.30.0      # 分布式训练加速
peft&gt;=0.11.0           # 参数高效微调（LoRA等）
```

#### 🚀 强**化学习专用包**

```
trl&gt;=0.8.0             # Transformers Reinforcement Learning
# 这是强化学习的核心包！！！
```

#### ⚡ **性能优化包**

```
deepspeed==0.15.2      # 大模型训练优化
triton==3.0.0          # GPU计算优化
bitsandbytes           # 量化训练
flash-attn             # 注意力机制优化
```

#### 📊 辅**助工具包**

```
gradio                 # Web界面
wandb                  # 训练监控
numpy, pandas          # 数据处理
matplotlib, seaborn    # 可视化
```

# 6. llama_factory支持的数据结构

dataset_info.json 包含了所有经过预处理的 本地数据集 以及 在线数据集。如果希望使用自定义数据集，务必在 dataset_info.json 文件中添加对数据集及其内容的定义。

目前支持 Alpaca 格式和 ShareGPT 格式的数据集。

## Alpaca

指令监督微调(Instruct Tuning)通过让模型学习详细的指令以及对应的回答来优化模型在特定指令下的表现。

instruction 列对应的内容为人类指令， input 列对应的内容为人类输入， output 列对应的内容为模型回答。下面是一个例子

```
&quot;alpaca_zh_demo.json&quot;
{
  &quot;instruction&quot;: &quot;计算这些物品的总费用。 &quot;,
  &quot;input&quot;: &quot;输入：汽车 - $3000，衣服 - $100，书 - $20。&quot;,
  &quot;output&quot;: &quot;汽车、衣服和书的总费用为 $3000 + $100 + $20 = $3120。&quot;
},
```

在进行指令监督微调时， instruction 列对应的内容会与 input 列对应的内容拼接后作为最终的人类输入，即人类输入为 instruction\ninput。而 output 列对应的内容为模型回答。 在上面的例子中，人类的最终输入是：

```
计算这些物品的总费用。
输入：汽车 - $3000，衣服 - $100，书 - $20。
```

模型的回答是：

```
汽车、衣服和书的总费用为 $3000 + $100 + $20 = $3120。
```

history 列是由多个字符串二元组构成的列表，分别代表历史消息中每轮对话的指令和回答。注意在指令监督微调时，历史消息中的回答内容也会被用于模型学习。

指令监督微调数据集 格式要求 如下：

```
[
  {
    &quot;instruction&quot;: &quot;人类指令（必填）&quot;,
    &quot;input&quot;: &quot;人类输入（选填）&quot;,
    &quot;output&quot;: &quot;模型回答（必填）&quot;,
    &quot;system&quot;: &quot;系统提示词（选填）&quot;,
    &quot;history&quot;: [
      [&quot;第一轮指令（选填）&quot;, &quot;第一轮回答（选填）&quot;],
      [&quot;第二轮指令（选填）&quot;, &quot;第二轮回答（选填）&quot;]
    ]
  }
]
```

```
[
  {
    &quot;instruction&quot;: &quot;今天的天气怎么样？&quot;,
    &quot;input&quot;: &quot;&quot;,
    &quot;output&quot;: &quot;今天的天气不错，是晴天。&quot;,
    &quot;history&quot;: [
      [
        &quot;今天会下雨吗？&quot;,
        &quot;今天不会下雨，是个好天气。&quot;
      ],
      [
        &quot;今天适合出去玩吗？&quot;,
        &quot;非常适合，空气质量很好。&quot;
      ]
    ]
  }
]
```

对于上述格式的数据， dataset_info.json 中的 数据集描述 应为：

```
&quot;数据集名称&quot;: {
  &quot;file_name&quot;: &quot;data.json&quot;,
  &quot;columns&quot;: {
    &quot;prompt&quot;: &quot;instruction&quot;,
    &quot;query&quot;: &quot;input&quot;,
    &quot;response&quot;: &quot;output&quot;,
    &quot;system&quot;: &quot;system&quot;,
    &quot;history&quot;: &quot;history&quot;
  }
}
```

## 偏好数据集

偏好数据集用于奖励模型训练、DPO 训练和 ORPO 训练。对于系统指令和人类输入，偏好数据集给出了一个更优的回答和一个更差的回答。

一些研究 表明通过让模型学习“什么更好”可以使得模型更加迎合人类的需求。 甚至可以使得参数相对较少的模型的表现优于参数更多的模型。

偏好数据集需要在 chosen 列中提供更优的回答，并在 rejected 列中提供更差的回答，在一轮问答中其格式如下

```
[
  {
    &quot;instruction&quot;: &quot;人类指令（必填）&quot;,
    &quot;input&quot;: &quot;人类输入（选填）&quot;,
    &quot;chosen&quot;: &quot;优质回答（必填）&quot;,
    &quot;rejected&quot;: &quot;劣质回答（必填）&quot;
  }
]
```

```
&quot;数据集名称&quot;: {
  &quot;file_name&quot;: &quot;data.json&quot;,
  &quot;ranking&quot;: true,
  &quot;columns&quot;: {
    &quot;prompt&quot;: &quot;instruction&quot;,
    &quot;query&quot;: &quot;input&quot;,
    &quot;chosen&quot;: &quot;chosen&quot;,
    &quot;rejected&quot;: &quot;rejected&quot;
  }
}
```

## 多模态数据

```
  {
    &quot;instruction&quot;: &quot;人类指令（必填）&quot;,
    &quot;input&quot;: &quot;人类输入（选填）&quot;,
    &quot;output&quot;: &quot;模型回答（必填）&quot;,
    &quot;images&quot;: [
      &quot;图像路径（必填）&quot;
    ]
  }
]
```

对于上述格式的数据， dataset_info.json 中的 数据集描述 应为：

```
&quot;数据集名称&quot;: {
  &quot;file_name&quot;: &quot;data.json&quot;,
  &quot;columns&quot;: {
    &quot;prompt&quot;: &quot;instruction&quot;,
    &quot;query&quot;: &quot;input&quot;,
    &quot;response&quot;: &quot;output&quot;,
    &quot;images&quot;: &quot;images&quot;
  }
}
```

相比alpaca格式的数据集， sharegpt 格式支持更多的角色种类，例如 human、gpt、observation、function 等等。它们构成一个对象列表呈现在 conversations 列中。 下面是sharegpt格式的一个例子：

```
{
  &quot;conversations&quot;: [
    {
      &quot;from&quot;: &quot;human&quot;,
      &quot;value&quot;: &quot;你好，我出生于1990年5月15日。你能告诉我我今天几岁了吗？&quot;
    },
    {
      &quot;from&quot;: &quot;function_call&quot;,
      &quot;value&quot;: &quot;{\&quot;name\&quot;: \&quot;calculate_age\&quot;, \&quot;arguments\&quot;: {\&quot;birthdate\&quot;: \&quot;1990-05-15\&quot;}}&quot;
    },
    {
      &quot;from&quot;: &quot;observation&quot;,
      &quot;value&quot;: &quot;{\&quot;age\&quot;: 31}&quot;
    },
    {
      &quot;from&quot;: &quot;gpt&quot;,
      &quot;value&quot;: &quot;根据我的计算，你今天31岁了。&quot;
    }
  ],
  &quot;tools&quot;: &quot;[{\&quot;name\&quot;: \&quot;calculate_age\&quot;, \&quot;description\&quot;: \&quot;根据出生日期计算年龄\&quot;, \&quot;parameters\&quot;: {\&quot;type\&quot;: \&quot;object\&quot;, \&quot;properties\&quot;: {\&quot;birthdate\&quot;: {\&quot;type\&quot;: \&quot;string\&quot;, \&quot;description\&quot;: \&quot;出生日期以YYYY-MM-DD格式表示\&quot;}}, \&quot;required\&quot;: [\&quot;birthdate\&quot;]}}]&quot;
}
```

注意其中human和 observation必须出现在奇数位置，gpt和 function必须出现在偶数位置。

```
[
  {
    &quot;conversations&quot;: [
      {
        &quot;from&quot;: &quot;human&quot;,
        &quot;value&quot;: &quot;人类指令&quot;
      },
      {
        &quot;from&quot;: &quot;function_call&quot;,
        &quot;value&quot;: &quot;工具参数&quot;
      },
      {
        &quot;from&quot;: &quot;observation&quot;,
        &quot;value&quot;: &quot;工具结果&quot;
      },
      {
        &quot;from&quot;: &quot;gpt&quot;,
        &quot;value&quot;: &quot;模型回答&quot;
      }
    ],
    &quot;system&quot;: &quot;系统提示词（选填）&quot;,
    &quot;tools&quot;: &quot;工具描述（选填）&quot;
  }
]
```

对于上述格式的数据，dataset_info.json中的数据集描述应为：

```
&quot;数据集名称&quot;: {
  &quot;file_name&quot;: &quot;data.json&quot;,
  &quot;formatting&quot;: &quot;sharegpt&quot;,
  &quot;columns&quot;: {
    &quot;messages&quot;: &quot;conversations&quot;,
    &quot;system&quot;: &quot;system&quot;,
    &quot;tools&quot;: &quot;tools&quot;
  }
}
```

## 数据创建启动安装步骤

![](img/989e7107ec2acda0743a9d8bfef91606.png)

输出输出完成

![](img/118cd29af8b2d90eaa1622f25137b11f.png)

演示视频

Video("https://typora-photo1220.oss-cn-beijing.aliyuncs.com/DataAnalysis/muyan/%E6%95%B0%E6%8D%AE%E5%88%9B%E5%BB%BA%E7%B4%A0%E6%9D%90.mp4", width=800, height=400)

# 扫码领取多模态数据构建程序

![](img/e7f2e961c14fff79d166b614559d86f5.png)

安装方式

![](img/a9a3ed685f465f0f84ffb44fbd1fd286.png)

![](img/26b7e42d9b6726210e2097d852065c96.png)

界面启动命令：

在项目根目录（data_create）

npm install

npm run dev

![](img/c55ce6a43e9a050e8917e7eb05e6fb94.png)

![](img/9d472670c118030c8c0b7eb06a0fddce.png)

```
{
&quot;messages&quot;: 
[{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;从下图表单中抽取键值对并按 JSON 格式返回，仅返回 JSON：&lt;image&gt;&quot;}, 
{&quot;role&quot;: &quot;assistant&quot;, &quot;content&quot;: &quot;{\&quot;受理时间:\&quot;: \&quot;2020.6.15\&quot;, \&quot;特别提示:\&quot;: \&quot;1.请在填表前仔细阅读有关基金的《基金合同》、最新《招募说明书》及《开\&quot;, \&quot;▽业务类型\&quot;: \&quot;放式基金业务规则》。请用黑色或蓝黑色钢笔或签字笔填写,如有选择项,\&quot;, \&quot;▽机构基本信息\&quot;: \&quot;请在口内划√,任何涂改请加盖公章或签字证明。\&quot;, \&quot;*证件号码:\&quot;: \&quot;□三资[中外合资、中外合作、外商独资]\&quot;, \&quot;*证件有效期:2021年9\&quot;: \&quot;□账户登记\&quot;, \&quot;*邮寄地址:\&quot;: \&quot;□客户资料变更(如需修改银行账户信息,请直接填写《投资者银行账户设置及变更申请表》)\&quot;, \&quot;邮编:\&quot;: \&quot;☑账户开户\&quot;, \&quot;*经办人姓名:\&quot;: \&quot;□交易账户销户\&quot;, \&quot;*证件类型:\&quot;: \&quot;□集体\&quot;, \&quot;*职务:\&quot;: \&quot;秦皇岛旭森企业管理有限公司\&quot;, \&quot;*证件有效期:\&quot;: \&quot;省秦皇岛市市经济技术开发区永定河道9号\&quot;, \&quot;经办人手机:\&quot;: \&quot;□社会团体证件\&quot;, \&quot;账单寄送方式:\&quot;: \&quot;□基金会证件\&quot;, \&quot;*法定代表人(非法人机构为负责人)姓名:\&quot;: \&quot;□行政机关证件\&quot;, \&quot;★投资资金是否为信托资产:\&quot;: \&quot;李丛林\&quot;, \&quot;若是信托资产,\&quot;: \&quot;18757832244@163.com\&quot;, \&quot;▽开户预留银行账户信息(如需修改银行账户信息,请直接填写《投资者银行账户设置及变更申请表》)\&quot;: \&quot;☑E-mail\&quot;, \&quot;*户名:\&quot;: \&quot;□手机短信\&quot;, \&quot;*银行账号:\&quot;: \&quot;195658197506185119\&quot;, \&quot;★开户银行属地:\&quot;: \&quot;(只能选一项,后三项开通前均采用邮寄方式)\&quot;, \&quot;*开户银行名称:\&quot;: \&quot;☑否\&quot;, \&quot;▽机构其他身份信息\&quot;: \&quot;6215968321478528861\&quot;, \&quot;*企业性质:\&quot;: \&quot;中国银行(秦皇岛市开发区支行)\&quot;, \&quot;*行业类型:\&quot;: \&quot;李丛林\&quot;, \&quot;*注册资本:\&quot;: \&quot;河北省\&quot;, \&quot;▽声明\&quot;: \&quot;省\&quot;, \&quot;经办人签章:\&quot;: \&quot;秦皇岛\&quot;, \&quot;附件：\&quot;: \&quot;市\&quot;, \&quot;*申请机构名称:\&quot;: \&quot;□国有\&quot;, \&quot;E-mail:\&quot;: \&quot;☑私营\&quot;, \&quot;日期:\&quot;: \&quot;☑其他金融机构\&quot;}&quot;}],
 &quot;images&quot;: [&quot;/home/ubuntu/data/zh_train/zh_train_0.jpg&quot;]
 }
```

注册的格式

![](img/904c7cdf1cabf110d6b32c604548488e.png)

llama_factory data路径下进行配置

```
  &quot;llamafactory_train&quot;: {
    &quot;file_name&quot;: &quot;llamafactory_train.jsonl&quot;,
    &quot;formatting&quot;: &quot;sharegpt&quot;,
    &quot;columns&quot;: {
      &quot;messages&quot;: &quot;messages&quot;,
      &quot;images&quot;: &quot;images&quot;
    },
    &quot;tags&quot;: {
      &quot;role_tag&quot;: &quot;role&quot;,
      &quot;content_tag&quot;: &quot;content&quot;,
      &quot;user_tag&quot;: &quot;user&quot;,
      &quot;assistant_tag&quot;: &quot;assistant&quot;
    }
  },
```

# 7. Qwen3-VL微调图文理解任务实战

模型下载

modelscope download --model Qwen/Qwen2.5-VL-7B-Instruct --local_dir ./Qwen2.5-VL-7B

![](img/f40166339cf4e10f59a2a24e4fbbdf63.png)

![](img/13f5b40d5c0cc511af19910374d5ed9f.png)

![](img/ec2467f4db1996856a2ec68451713fac.png)

![](img/f6252192e5299b7829da16c9ca1ca7c7.png)

![](img/e49be40cbca629ae2d8942c3886a5201.png)

![](img/3abac6abb4cdc2a397711d46241d9112.png)

![](img/3131aa6c54ed32780e967db615fd853c.png)

![](img/0c37eead33db47406262899dbe2b822a.png)

# 8. 参数详解

llama_factory生成命令

```
llamafactory-cli train \
    --stage sft \
    --do_train True \
    --model_name_or_path /home/ubuntu/Qwen3-VL-4B \
    --preprocessing_num_workers 16 \
    --finetuning_type lora \
    --template qwen3_vl_nothink \
    --flash_attn auto \
    --dataset_dir data \
    --dataset llamafactory_train \
    --cutoff_len 2048 \
    --learning_rate 5e-05 \
    --num_train_epochs 3.0 \
    --max_samples 100000 \
    --per_device_train_batch_size 1 \
    --gradient_accumulation_steps 16 \
    --lr_scheduler_type cosine \
    --max_grad_norm 1.0 \
    --logging_steps 5 \
    --save_steps 100 \
    --warmup_steps 0 \
    --packing False \
    --enable_thinking True \
    --report_to none \
    --output_dir saves/Qwen3-VL-4B-Instruct/lora/train_2025-10-30-11-21-08 \
    --bf16 True \
    --plot_loss True \
    --trust_remote_code True \
    --ddp_timeout 180000000 \
    --include_num_input_tokens_seen True \
    --optim adamw_torch \
    --lora_rank 8 \
    --lora_alpha 16 \
    --lora_dropout 0 \
    --lora_target all \
    --freeze_vision_tower True \
    --freeze_multi_modal_projector True \
    --image_max_pixels 589824 \
    --image_min_pixels 1024 \
    --video_max_pixels 65536 \
    --video_min_pixels 256
```

## 一、基础训练配置

### 1. --stage sft

**含义**：训练阶段类型

+   sft：Supervised Fine-Tuning（监督微调）

+   其他选项：pt（预训练）、rm（奖励模型）、ppo（强化学习）

**效果**：

+   sft：用标注数据微调模型

+   pt：从头训练或继续预训练

+   rm/ppo：RLHF 训练

### 2. --do_train True

**含义**：是否执行训练

+   True：训练模式

+   False：只评估/推理

### 3. --num_train_epochs 3.0

**含义**：训练轮数（遍历数据集的次数）

| 值 | 效果 | 适用场景 |
| --- | --- | --- |
| **1-2** | 欠拟合风险 | 数据量很大（&gt;10K） |
| **3-5** | ✅ 平衡 | 中小数据集（100-1K） |
| **10+** | 过拟合风险 | 极小数据集（&lt;50），需监控 |

**您的配置**：3.0 ✅ 合适

## 二、模型和数据配置

### 4. --model_name_or_path /home/ubuntu/Qwen3-VL-4B

**含义**：基础模型路径

+   本地路径或 HuggingFace 模型 ID

### 5. --preprocessing_num_workers 16

**含义**：数据预处理的并行进程数

| 值 | 效果 |
| --- | --- |
| **1-4** | 慢，CPU 利用率低 |
| **8-16** | ✅ 快，充分利用多核 |
| **32+** | 边际收益递减 |

**配置**：16 ✅ 适合多核 CPU

### 6. --dataset_dir data + --dataset llamafactory_train

**含义**：数据集目录和名称

+   完整路径：data/llamafactory_train.jsonl

### 7. --cutoff_len 2048

**含义**：最大序列长度（token 数）

| 值 | 效果 | 显存占用 | 适用场景 |
| --- | --- | --- | --- |
| **512** | 截断长文本 | 低 | 短文本任务 |
| **1024** | 平衡 | 中 | 大多数任务 |
| **2048** | ✅ 保留完整信息 | 高 | 长文本/多模态 |
| **4096+** | 完整但慢 | 极高 | 极长文档 |

**多模态特殊性**：

```
文本: ~2000 tokens
图像: ~576 tokens（限制分辨率后）
总计: ~2576 tokens
```

2048 可能不够，但配合 image_max_pixels 控制可以。

### 8. --max_samples 100000

**含义**：最大使用样本数

+   限制训练数据量（用于快速实验）

+   只有 149 样本，此参数无影响

## 三、Batch Size 和梯度配置

### 9. --per_device_train_batch_size 1

**含义**：每个 GPU 的 batch size

| 值 | 显存占用 | 训练速度 | 效果稳定性 |
| --- | --- | --- | --- |
| **1** | ✅ 低 | 慢 | 较不稳定 |
| **2** | 中 | 中 | ✅ 平衡 |
| **4+** | 高（可能OOM） | 快 | 更稳定 |

**配置**：1（因为多模态显存大）

### 10. --gradient_accumulation_steps 16

**含义**：梯度累积步数

```
有效 Batch Size = per_device_batch_size × gradient_accumulation_steps
                 = 1 × 16 = 16
```

| 值 | 效果 | 显存 | 训练时间 |
| --- | --- | --- | --- |
| **1** | 小 batch，不稳定 | 低 | 快 |
| **8-16** | ✅ 平衡 | 低 | 中 |
| **32+** | 大 batch，稳定 | 低 | 慢 |

**关系**：

```
per_device=1, accumulation=16  ≈  per_device=2, accumulation=8
（显存相同，但前者稍慢）
```

## 四、学习率和调度器

### 11. --learning_rate 5e-05

**含义**：学习率（控制参数更新幅度）

| 值 | 效果 | 适用场景 |
| --- | --- | --- |
| **1e-5** | 保守，收敛慢 | 大模型全量微调 |
| **5e-5** | ✅ 平衡 | LoRA 微调 |
| **1e-4** | 激进，可能不稳定 | 小模型 |
| **5e-4+** | 很激进，容易发散 | 不推荐 |

**LoRA 通常用更高学习率**（因为只训练少量参数）

### 12. --lr_scheduler_type cosine

**含义**：学习率调度策略

```
学习率变化曲线：

constant: ━━━━━━━━━━━  （恒定）

linear:   ━━━╲        （线性衰减）
               ╲
                ╲━━━━

cosine:   ━━━╮         （余弦衰减，平滑）
              ╲╲
                ╲━━━
```

| 类型 | 特点 | 适用场景 |
| --- | --- | --- |
| **constant** | 不变 | 短期训练 |
| **linear** | 线性下降 | 简单任务 |
| **cosine** | ✅ 平滑下降 | 大多数任务（推荐） |
| **polynomial** | 可控下降 | 特殊需求 |

### 13. --warmup_steps 0

**含义**：学习率预热步数

```
warmup_steps = 100:
lr: 0 → 5e-5 (前100步)
      ↓
    5e-5 → 0 (后续步数，按 scheduler)

warmup_steps = 0:
lr: 直接从 5e-5 开始
```

| 值 | 效果 | 适用场景 |
| --- | --- | --- |
| **0** | ✅ 直接开始学习 | 小数据集（&lt;1K） |
| **100-500** | 稳定起步 | 大数据集 |
| **&gt;总步数** | ❌ 永远在 warmup | 错误配置 |

**场景**：

```
总步数 = 149样本 / 16batch × 3epochs ≈ 30步
warmup_steps = 0 ✅ 正确！
```

### 14. --max_grad_norm 1.0

**含义**：梯度裁剪阈值

```
if gradient_norm &gt; 1.0:
    gradient = gradient / gradient_norm * 1.0
```

| 值 | 效果 |
| --- | --- |
| **0.5** | 激进裁剪，稳定但慢 |
| **1.0** | ✅ 平衡 |
| **5.0+** | 宽松，接近无裁剪 |
| **None** | 不裁剪，可能梯度爆炸 |

## 五、LoRA 配置

### 15. --finetuning_type lora

**含义**：微调方法

+   lora：LoRA 微调 ✅

+   full：全量微调

+   freeze：冻结部分层

### 16. --lora_rank 8

**含义**：LoRA 秩（参数量的关键）

```
参数量 ∝ rank × hidden_dim × 2

rank=8:  小，快，效果好
rank=64: 大，慢，效果略好
```

| Rank | 参数量 | 训练时间 | 效果 | 推荐场景 |
| --- | --- | --- | --- | --- |
| **4** | 极小 | 快 | 90% | 极小数据 |
| **8** | ✅ 小 | 快 | 95% | 小数据集 |
| **16** | 中 | 中 | 98% | 中数据集 |
| **64** | 大 | 慢 | 100% | 大数据集 |

**配置**：8 ✅ 适合 149 样本

### 17. --lora_alpha 16

**含义**：LoRA 缩放参数

```
实际影响 = LoRA权重 × (alpha / rank)
alpha=16, rank=8 → 缩放 2x
```

**规则**：alpha = 2 × rank（常用）

| Alpha/Rank | 效果 |
| --- | --- |
| **1x** | LoRA 影响小 |
| **2x** | ✅ 标准 |
| **4x** | LoRA 影响大 |

### 18. --lora_dropout 0

**含义**：LoRA 层的 dropout 率

| 值 | 效果 |
| --- | --- |
| **0** | ✅ 无 dropout，收敛快 |
| **0.05** | 轻微正则化 |
| **0.1+** | 强正则化，防过拟合 |

**小数据集通常用 0**

### 19. --lora_target all

**含义**：LoRA 应用的模块

```
all: 所有线性层（q,k,v,o,gate,up,down）
q_proj,v_proj: 只训练部分 attention
```

| 值 | 参数量 | 效果 |
| --- | --- | --- |
| **q_proj,v_proj** | 少 | 基础 |
| q_proj,k_proj,v_proj,o_proj | ✅ 中 | 推荐 |
| **all** | 多 | 最强（配置） |

## 六、多模态特定配置

### 20. --freeze_vision_tower True ⭐⭐⭐

**含义**：冻结视觉编码器

| 值 | 可训练参数 | 训练时间 | 效果 |
| --- | --- | --- | --- |
| **True** | ✅ 少（~17M） | 快 | 适合小数据 |
| **False** | 多（~150M） | 慢10x | 适合大数据 |

**配置**：True ✅ 关键优化！

### 21. --freeze_multi_modal_projector True

**含义**：冻结多模态投影层

**效果同上**，进一步减少参数。

### 22. --image_max_pixels 589824 ⭐⭐⭐

**含义**：图像最大像素数

```
589824 = 768 × 768
```

| 值 | 分辨率 | Vision Tokens | 显存 | 质量 |
| --- | --- | --- | --- | --- |
| **262144** | 512×512 | ~256 | 低 | 低 |
| **589824** | ✅ 768×768 | ~576 | 中 | 好 |
| **1048576** | 1024×1024 | ~1024 | 高 | 更好 |
| **16777216** | 4096×4096 | ~16384 | 极高（OOM） | 最好 |

**配置**：589824 ✅ 平衡点！

**为什么这么重要**：

```
图像分辨率 ↑ → Vision Tokens ↑ → 显存 ↑²
(平方关系！)
```

### 23. --video_max_pixels 65536

**含义**：视频最大像素（不用视频，无影响）

## 七、优化器和精度

### 24. --optim adamw_torch

**含义**：优化器类型

| 类型 | 特点 | 速度 | 推荐 |
| --- | --- | --- | --- |
| **sgd** | 简单 | 快 | 不推荐 |
| **adamw_torch** | ✅ 标准 | 中 | 推荐 |
| **adamw_8bit** | 节省显存 | 稍慢 | 显存紧张时 |
| **adafactor** | 自适应 | 慢 | 特殊场景 |

### 25. --bf16 True

**含义**：使用 BF16 精度

| 精度 | 显存 | 速度 | 精度损失 |
| --- | --- | --- | --- |
| **FP32** | 4x | 1x | 无 |
| **FP16** | 2x | 2x | 极小 |
| **BF16** | ✅ 2x | 2x | 极小（更稳定） |

**A800 支持 BF16** ✅

## 八、日志和保存

### 26. --logging_steps 5

**含义**：每 N 步记录一次日志

| 值 | 效果 |
| --- | --- |
| **1** | 详细，但刷屏 |
| **5-10** | ✅ 平衡 |
| **50+** | 简洁，但不及时 |

### 27. --save_steps 100

**含义**：每 N 步保存一次 checkpoint

```
总步数 ≈ 30
save_steps=100 → 只在最后保存
```

**建议**：改为 10-20，保存中间结果

### 28. --output_dir saves/...

**含义**：输出目录

### 29. --report_to none

**含义**：日志记录工具

+   none：不上传

+   tensorboard：本地可视化 ✅ 推荐

+   wandb：在线追踪

## 九、其他配置

### 30. --template qwen3_vl_nothink

**含义**：对话模板

+   LLaMA-Factory 内置的 Qwen3-VL 格式

### 31. --flash_attn auto

**含义**：Flash Attention 模式

+   auto：自动检测 ✅

+   fa2：强制使用

+   disabled：禁用

### 32. --packing False

**含义**：是否打包多个样本到一个序列

+   False ✅：不打包（多模态通常不打包）

+   True：打包（纯文本可以）

### 33. --plot_loss True

**含义**：训练后绘制 loss 曲线

### 34. --trust_remote_code True

**含义**：允许执行远程代码（Qwen3-VL 需要）

## 模型验证导出

![](img/b53aa600a583fc010ad8d7ccfb6ed510.png)

| 分类 | 代表文件 | 是否推理必需 |
| --- | --- | --- |
| LoRA 权重 | adapter_model.safetensors / adapter_config.json | ✅ |
| 分词器相关 | tokenizer.json / tokenizer_config.json / special_tokens_map.json / vocab.json | ✅ |
| 预处理配置 | preprocessor_config.json / video_preprocessor_config.json | ✅（多模态时） |
| 对话模板 | chat_template.jinja | ⚙️ 推荐保留 |
| 训练状态 | checkpoint-/trainer_state.json/training_args. | ⚙️ 用于续训 |
| 指标与日志 | all_results.json / training_loss.png / running_log.txt | 📊 可选 |
| 可视化配置 | llamaboard_config.yaml | 📊 可选 |

**模型：Qwen3-VL-4B-Instruct 路径：/LL**ama-Factory/saves/.../train_2025-10-30-11-21-08

该目录是一次完整的 **LoRA 微调任务输出目录**，包含权重、配置、分词器、训练日志、指标与快照。

每个文件/文件夹的作用如下：

## 核心权重文件（用于推理/部署）

| 文件名 | 说明 | 作用 |
| --- | --- | --- |
| adapter_model.safetensors | 保存经过微调后的 LoRA 权重，仅包含增量参数（不会改变底模）。 | 加载推理时的关键文件。 |
| adapter_config.json | 记录 LoRA 结构及参数（如 r、alpha、target_modules、lora_dropout）。 | 加载时与上面权重一起使用。 |

## 分词器与词表文件

| 文件名 | 说明 | 作用 |
| --- | --- | --- |
| **tokenizer.json** | Hugging Face 新版统一分词规则文件，包含词表及分词算法。 | 加载 tokenizer 时的核心。 |
| tokenizer_config.json | 分词器的元配置信息，如模型类型、是否小写化、特殊符号定义等。 | 保证 tokenizer 行为一致。 |
| special_tokens_map.json | 特殊 token 映射表（bos_token、eos_token、pad_token 等）。 | 用于生成与解码时处理特殊符号。 |
| added_tokens.json | 若训练中自定义了特殊 token，会记录在此。 | 保证自定义词可识别。 |
| vocab.json / merges.txt | BPE 编码的词表及合并规则（部分模型兼容旧格式）。 | 兼容老版本 tokenizer。 |

## 预处理与多模态配置

| 文件名 | 说明 | 作用 |
| --- | --- | --- |
| preprocessor_config.json | 文本/输入预处理参数（padding、截断、归一化等）。 | 训练/推理前的输入格式定义。 |
| video_preprocessor_config.json | Qwen-VL 多模态专用：图像/视频预处理信息（分辨率、patch 大小、采样步距等）。 | 图像或视频输入时使用。 |
| chat_template.jinja | Jinja 模板文件，用于将多轮对话格式化为模型输入。 | 在对话式推理中应用。 |

## 🏋️训练过程与快照

| 文件名/文件夹 | 说明 | 作用 |
| --- | --- | --- |
| **checkpoint-30/** | 在训练第 30 步保存的快照，含当时的 LoRA 权重与优化器状态。 | 可用于恢复训练或回滚。 |
| trainer_state.json | Trainer 状态信息：当前步数、最佳指标、学习率变化、随机种子等。 | 续训与实验复现。 |
| **training_args.bin **/ training_args.yaml | 训练时所有超参快照（lr、batch size、LoRA 配置、路径等）。 | 记录实验参数，复现实验必备。 |
| trainer_log.jsonl | 逐步日志（loss、吞吐量、显存、eval 结果等）。 | 可视化与性能分析。 |
| **running_log.txt** | 控制台输出的完整文本版日志。 | 定位报错与警告信息。 |

## 训练结果与可视化

| 文件名 | 说明 | 作用 |
| --- | --- | --- |
| all_results.json | 训练完成后自动汇总的指标（loss、速度、评估分数等）。 | 记录总体表现。 |
| train_results.json | 部分框架生成的简表版结果文件。 | 评估结果读取或脚本解析。 |
| training_loss.png | 训练过程中 loss 曲线图。 | 快速查看收敛趋势。 |

## 其他辅助文件

| 文件名 | 说明 | 作用 |
| --- | --- | --- |
| llamaboard_config.yaml | LLaMA-Board（实验可视化面板）的配置文件。 | 仅在使用可视化工具时需要。 |
| **README.md** | 自动生成的说明或占位文件。 | 可忽略或替换为自定义说明。 |

![](img/27c873dab8e21d68ff9cd930fb15d94d.png)

### 关键指标

| 指标 | 数值 | 评价 |
| --- | --- | --- |
| **初始 Loss** | ~1.05 | 起点合理 |
| **最终 Loss** | ~0.68-0.70 | ✅ 收敛良好 |
| **总下降幅度** | 35% | ✅ 显著下降 |
| **收敛速度** | 前 15 步快速下降 | ✅ 高效 |
| **稳定性** | 后期平稳 | ✅ 无过拟合迹象 |

### 曲线特征分析

### 1. **快速学习阶段**（Step 5-15）

```
Loss: 1.05 → 0.75
下降: 0.30 (28%)
```

**分析**：

+   ✅ 学习率设置合理（5e-5）

+   ✅ warmup_steps=0 让模型立即学习

+   ✅ 数据质量好，模型快速适应

### 2. **精细调整阶段**（Step 15-25）

```
Loss: 0.75 → 0.68
下降: 0.07 (9%)
```

**分析**：

+   ✅ Cosine 学习率调度起作用

+   ✅ 进入精细优化阶段

+   ✅ 没有震荡，说明学习率衰减合适

### 3. **收敛阶段**（Step 25-30）

```
Loss: 0.68 → 0.68
趋于稳定
```

**分析**：

+   ✅ 模型收敛

+   ✅ 可以考虑停止或继续训练更多 epoch

### 需要关注的点

1.  **还有下降空间**

```
从曲线看，loss 在 step 30 时还在缓慢下降
建议：可以再训练 1-2 个 epoch 观察
```

1.  **缺少验证集监控**

```
只有训练 loss，无法判断泛化能力
建议：添加验证集监控
```

![](img/48127462f0ab32248210c5c624dea827.png)

![](img/360f7e26bf385de3845ce088769b2876.png)

![](img/179ee18175121ea01cd20cd4fb447ef1.png)

![](img/e787c05a9c2daad40d7fe3afdc51e01e.png)

![](img/aa26251c64d935a6020370a55427f8b6.png)

对比Qwen2.5-vl-7B

```
从下图表单中抽取键值对并按 JSON 格式返回，仅返回 JSON

{
    &quot;开户银行&quot;: &quot;中国银行(秦皇岛市开发区支行)&quot;,
    &quot;开户银行地址&quot;: &quot;河北省秦皇岛市&quot;,
    &quot;注册资金&quot;: &quot;50万元&quot;,
    &quot;法定代表人(负责人)签章&quot;: &quot;李丛林&quot;,
    &quot;经办人签章&quot;: &quot;李丛林&quot;,
    &quot;日期&quot;: &quot;2020年6月15日&quot;,
    &quot;开户银行名称&quot;: &quot;中国银行(秦皇岛市开发区支行)&quot;,
    &quot;开户银行地址&quot;: &quot;河北省秦皇岛市&quot;,
    &quot;开户银行账号&quot;: &quot;6215968321478528861&quot;,
    &quot;机构名称&quot;: &quot;秦皇岛旭森企业管理有限公司&quot;,
    &quot;证件类型&quot;: &quot;营业执照&quot;,
    &quot;证件号码&quot;: &quot;91130301MA0EFMWL8N&quot;,
    &quot;证件有效期&quot;: &quot;2021年9月22日&quot;,
    &quot;经办人姓名&quot;: &quot;李丛林&quot;,
    &quot;证件类型&quot;: &quot;身份证&quot;,
    &quot;证件号码&quot;: &quot;195658197506185119&quot;,
    &quot;证件有效期&quot;: &quot;2026年7月13日&quot;,
    &quot;业务类型&quot;: &quot;账户开户&quot;,
    &quot;交易账号&quot;: &quot;&quot;,
    &quot;基金账号&quot;: &quot;&quot;,
    &quot;受理时间&quot;: &quot;2020.6.15&quot;
}
```

```
{
    &quot;机构名称&quot;: &quot;秦皇岛旭森企业管理有限公司&quot;,
    &quot;证件类型&quot;: &quot;营业执照&quot;,
    &quot;证件号码&quot;: &quot;91130301MA0EFMWL8N&quot;,
    &quot;证件有效期&quot;: &quot;2021年9月22日&quot;,
    &quot;地址&quot;: &quot;河北省;省秦皇岛市;市经济技术开发区永定河道9号&quot;,
    &quot;邮编&quot;: &quot;066004&quot;,
    &quot;法定代表人&quot;: &quot;李丛林&quot;,
    &quot;证件类型&quot;: &quot;身份证&quot;,
    &quot;证件号码&quot;: &quot;195658197506185119&quot;,
    &quot;证件有效期&quot;: &quot;2026年7月13日&quot;,
    &quot;投资人&quot;: &quot;李丛林&quot;,
    &quot;投资人证件类型&quot;: &quot;身份证&quot;,
    &quot;投资人证件号码&quot;: &quot;195658197506185119&quot;,
    &quot;投资人证件有效期&quot;: &quot;2026年7月13日&quot;,
    &quot;开户行名称&quot;: &quot;中国银行(秦皇岛市开发区支行)&quot;,
    &quot;开户行地址&quot;: &quot;河北省;省;秦皇岛;市&quot;,
    &quot;注册资本&quot;: &quot;50万元&quot;,
    &quot;销售网点&quot;: &quot;&quot;,
    &quot;投资顾问&quot;: &quot;&quot;,
    &quot;主管&quot;: &quot;&quot;,
    &quot;活动来源&quot;: &quot;&quot;,
    &quot;复核员&quot;: &quot;&quot;,
    &quot;经办人&quot;: &quot;李丛林&quot;,
    &quot;经办人联系电话&quot;: &quot;(日间):&quot;,
    &quot;经办人手机&quot;: &quot;18757832244&quot;,
    &quot;日期&quot;: &quot;2020年6月15日&quot;,
    &quot;开户行账户&quot;: &quot;6215968321478528861&quot;
}
```

原生 qwen3-4B-vl

```
从下图表单中抽取键值对并按 JSON 格式返回，仅返回 JSON

{
&quot;受理时间&quot;: &quot;2020.6.15&quot;,
&quot;基金账号&quot;: &quot;&quot;,
&quot;交易账号&quot;: &quot;&quot;,
&quot;业务类型&quot;: &quot;机构账户开户&quot;,
&quot;机构基本信息&quot;: {
&quot;申请机构名称&quot;: &quot;秦皇岛旭泰企业管理有限公司&quot;,
&quot;证件类型&quot;: &quot;营业执照&quot;,
&quot;证件号码&quot;: &quot;91130301MA0FFMWL8N&quot;,
&quot;证件有效期&quot;: &quot;2021年9月22日&quot;,
&quot;邮编&quot;: &quot;066004&quot;,
&quot;联系人姓名&quot;: &quot;李丛林&quot;,
&quot;职务&quot;: &quot;法定代表人&quot;,
&quot;证件类型&quot;: &quot;身份证&quot;,
&quot;证件号码&quot;: &quot;155558197506185119&quot;,
&quot;经营有效期&quot;: &quot;2020年7月13日&quot;,
&quot;经办人联系电话&quot;: &quot;18757832244&quot;,
&quot;经办人手机&quot;: &quot;18757832244&quot;,
&quot;传真&quot;: &quot;[18757832244@163.com](mailto:18757832244@163.com)&quot;,
&quot;账户类型&quot;: &quot;一般账户&quot;,
&quot;账户开户人姓名&quot;: &quot;李丛林&quot;,
&quot;账户开户人身份证号&quot;: &quot;155558197506185119&quot;,
&quot;开户日期&quot;: &quot;2020年7月13日&quot;,
&quot;投资资金是否为信托资产&quot;: &quot;否&quot;,
&quot;是否托管账户&quot;: &quot;否&quot;,
&quot;开户银行名称&quot;: &quot;中国银行(秦皇岛市开发区支行)&quot;,
&quot;开户银行账号&quot;: &quot;6215988321478528861&quot;,
&quot;开户银行地址&quot;: &quot;河北省秦皇岛市&quot;,
&quot;其他身份信息&quot;: {
&quot;企业性质&quot;: &quot;集体&quot;,
&quot;行业类别&quot;: &quot;其他金融机构&quot;,
&quot;注册资本&quot;: &quot;50万元&quot;
}
},
&quot;声明&quot;: &quot;本机构已仔细阅读了《汇丰银行基金管理有限公司旗下基金的（基金合同）、最新的《招募说明书》、《开放式基金业务规则》等文件及本表所有内容（包括背靠背条款），愿意接受上述文件中载明的全部条款。本机构保证本申请表所填信息及所提供的资料真实有效、自愿履行基金投资人的各项义务，并自行承担基金投资风险。本机构保证，用于证券投资基金管理机构交易的资金来源合法，本机构进行证券投资基金管理权和交易之行为符合中华人民共和国各项法律法规、法规和规定。本机构已详细并充分了解《汇丰银行基金管理有限公司投资人权益告知》。&quot;,
&quot;机构公章&quot;: &quot;&quot;,
&quot;法定代表人（负责人）签字&quot;: &quot;&quot;,
&quot;经办人签字&quot;: &quot;李丛林&quot;,
&quot;日期&quot;: &quot;2020年6月15日&quot;
}
```

```

{
&quot;受理时间&quot;: &quot;2020.6.15&quot;,
&quot;基金账号&quot;: &quot;（新开户申请）&quot;,
&quot;交易账号&quot;: &quot;（新开户申请）&quot;,
&quot;业务类型&quot;: &quot;□账户开户 □客户登记 □客户资料变更（如需修改银行账户信息，请直接填写《投资者银行账户设置及变更申请表》）&quot;,
&quot;机构基本资料&quot;: &quot;□营业执照 □行业机关证件 □社会团体证件 □基金合格证 □其他证件&quot;,
&quot;申请机构名称&quot;: &quot;秦星岛旭基金企业管理有限公司&quot;,
&quot;证件号码&quot;: &quot;91130301MA0FFMWL8N&quot;,
&quot;注册号&quot;: &quot;91130301MA0FFMWL8N&quot;,
&quot;法定代表人&quot;: &quot;李丛林&quot;,
&quot;机构联系人&quot;: &quot;李丛林&quot;,
&quot;联系人电话&quot;: &quot;18757832244&quot;,
&quot;机构地址&quot;: &quot;河北省秦皇岛市海港区经济开发区开发区永定河路9号&quot;,
&quot;邮编&quot;: &quot;066004&quot;,
&quot;开户银行&quot;: &quot;中国银行（秦皇岛市开发区支行）&quot;,
&quot;开户行账号&quot;: &quot;6215968321478528861&quot;,
&quot;开户行行号&quot;: &quot;6215968321478528861&quot;,
&quot;开户行地址&quot;: &quot;河北省秦皇岛市&quot;,
&quot;机构其他信息&quot;: &quot;□企业法人 □个体户 □三类企业 □中外合资、中外合作 □外商独资 □私营 □其他：&quot;,
&quot;行业类型&quot;: &quot;□保险机构 □其他金融机构 □非金融机构 □其他金融机构&quot;,
&quot;注册资本&quot;: &quot;50万元&quot;,
&quot;声明&quot;: &quot;本机构已仔细阅读《汇丰晋信基金管理有限公司旗下基金的《基金合同》、最新的《招募说明书》、《开放式基金业务规则》等文件及本表所有内容（包括背面条款），愿意接受上述文件中载明的全部条款。本机构保证本申请表所填信息及所提供的资料真实有效。自愿履行基金投资人的各项义务，并自行承担基金投资风险。本机构保证，用于证券投资基金和银行交易的资金来源合法，本机构进行证券投资基金管理资格和交易之行为符合中华人民共和国各项法律法规、法规和规定。本机构已详细并充分了解《汇丰晋信基金管理有限公司投资人权益须知》。&quot;,
&quot;机构公章&quot;: &quot;&quot;,
&quot;法定代表人（负责人）签字&quot;: &quot;李丛林&quot;,
&quot;经办人签署&quot;: &quot;李丛林&quot;,
&quot;日期&quot;: &quot;2020年6月15日&quot;,
&quot;销售网点&quot;: &quot;&quot;,
&quot;投资顾问&quot;: &quot;&quot;,
&quot;主管&quot;: &quot;&quot;,
&quot;活动来源&quot;: &quot;&quot;,
&quot;附件&quot;: &quot;张&quot;,
&quot;录入员&quot;: &quot;&quot;,
&quot;复核员&quot;: &quot;&quot;
}
```

## 模型导出

![](img/c51029dcfb99d3db997212492f918115.png)

![](img/471c555b9c720a2145cbacc81876fba0.png)

![](img/4764f5304185f75cc867b8a90d949847.png)

![](img/fe94749e524156cff1170194c65661d6.png)