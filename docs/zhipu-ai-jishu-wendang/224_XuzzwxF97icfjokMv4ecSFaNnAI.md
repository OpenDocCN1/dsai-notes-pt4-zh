
# 开源 · CodeGeeX4 正式发布，同期开源！

> 来源：[https://ai.feishu.cn/docx/PYwzdovCPoZEFmxsdcAcGOtNnye](https://ai.feishu.cn/docx/PYwzdovCPoZEFmxsdcAcGOtNnye)

![](img/c530b384572299decf948604e72c14f9.png)

CodeGeeX 是智谱 AI 旗下的代码生成大模型，2022 年 9 月发布第一代模型，以每半年更新一代模型的迭代速度不断进化，致力于探索大模型代码生成能力的上限。基于模型打造的 CodeGeeX 智能编程助手，不仅支持代码补全、代码注释、代码修复、代码翻译等基础功能，还支持联网代码问答、代码解释器、检索增强等进阶实用功能。CodeGeeX 对个人用户完全免费，在各种主流 IDE 均可免费下载使用。

目前 CodeGeeX 的个人用户数量已经超过 100 万，企业版本也已经广泛应用于科技、金融、医疗和制造等多个行业，在中国信通院组织的代码大模型首轮评估中获得当前最高评级，成为国内首批通过该项评估的大模型之一。

为了让更多人体验到 CodeGeeX 的强大能力，我们正式发布 CodeGeeX4 系列模型的开源版本：CodeGeeX4-ALL-9B，集代码补全和生成、代码问答、代码解释器、工具调用、联网搜索、项目级代码问答等所有能力于一体的代码大模型，是目前百亿（10B）参数以下性能最强、最全能的代码大模型。

![](img/71f0f427a37bbd4958c3cf251ca741fa.png)

模型的核心功能 Demo 和使用教程已经在 GitHub 上开源，模型权重可在 HuggingFace、ModelScope、WiseModel 等大模型平台下载。

GitHub: `https://github.com/THUDM/CodeGeeX4`

HuggingFace: [https://huggingface.co/THUDM/codegeex4-all-9b](https://huggingface.co/THUDM/codegeex4-all-9b)

ModelScope: [https://modelscope.cn/models/ZhipuAI/codegeex4-all-9b](https://modelscope.cn/models/ZhipuAI/codegeex4-all-9b)

WiseModel: [https://wisemodel.cn/models/ZhipuAI/codegeex4-all-9b](https://wisemodel.cn/models/ZhipuAI/codegeex4-all-9b)

## 一、CodeGeeX4-ALL-9B：百亿参数以下性能最强的全能代码模型

CodeGeeX4-ALL-9B 作为最新一代 CodeGeeX4 系列模型的开源版本，在 GLM4 强大语言能力的基础上继续迭代，大幅增强代码生成能力。使用 CodeGeeX4-ALL-9B 单一模型，即可支持代码补全和生成、代码解释器、联网搜索、工具调用、仓库级长代码问答及生成等全面功能，覆盖了编程开发的各种场景。CodeGeeX4-ALL-9B 在多个权威代码能力评测集，如 NaturalCodeBench、BigCodeBench 上都取得了极具竞争力的表现，是百亿参数量级以下性能最强的模型，甚至超过数倍规模的通用模型，在推理性能和模型效果上得到最佳平衡。

### 性能表现评测

BigCodeBench 测试结果显示，CodeGeeX4-ALL-9B 在同等尺寸下效果最好：

![](img/4ca5644bd9c83745874b627362b570b4.png)

在其他代码生成、补全、推理测试集上，CodeGeeX4-ALL-9B 取得了与更大规模模型接近的水平：

![](img/305ff369a7d45b9a4adcc321fc8687b7.png)

### CodeGeeX4-ALL-9B 上下文处理能力达到了 128K

对于参数量 10B 以下的代码大模型，从海量的代码中准确提取信息是一个关键性的挑战。CodeGeeX4-ALL-9B 升级支持 128K 上下文，使其能够处理和利用更长代码文件、甚至是项目代码中的信息，有助于模型更深入理解复杂和细节丰富的代码。基于更长的上下文，CodeGeeX4-ALL-9B 可以处理更复杂的项目级任务，在输入显著变长的情况下，依然能准确回答不同代码文件中的内容，并对代码作出修改。

在“大海捞针”（Needle In A Haystack, NIAH）评估中，CodeGeeX4-ALL-9B 模型展示了其在处理长达 128K 的上下文中进行代码的嵌入和检索能力，实现了 100% 的检索准确度。

![](img/4cde709da56065a31dd3e5d0515e3b2b.png)

上面这张图，表现的是在一个全部由 Python 代码组成的测试集中，插入一个赋值语句，如：`zhipu_codemodel = "codegeex"`（Needle）。测试模型是否可以正确回答出 `zhipu_codemodel` 的值，CodeGeeX4-ALL-9B 100% 完成任务。

### CodeGeeX4-ALL-9B 支持 Function Call 能力

CodeGeeX4-ALL-9B 是目前唯一一个实现 Function Call 的代码大模型。

Berkeley Function Calling Leaderboard 是第一个可全面评估大模型函数调用能力的测试集。其中 AST 数据集是评估模型对 Java、JavaScript、Python 程序的调用能力；Executable 数据集是评估模型对真实场景 API 的函数调用能力。

CodeGeeX4-ALL-9B 在 Berkeley Function Calling Leaderboard 上进行了全面的测试，包括各种形式的函数调用、不同的函数调用场景以及函数调用可执行性的测试，得到了以下结果：在 AST 和 Exec 测试集中调用成功率超过 90%。

![](img/6d40d42187798f333da0886d519ee4ed.png)

## 二、插件功能全面进入项目级辅助编程

最新的 CodeGeeX 插件 v2.12.0 版本，已经全面接入了第四代模型。先来看看进阶的「编程超能力」！

![](img/40db9f2f552cbdb9b067f19f676966fc.png)

[20240705-170117.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RvkfbKTeeosKSpxOfv6cWHRFnxd)

CodeGeeX4-ALL-9B 的生成速度全面超越上一代模型，实现 3 倍速生成，首响速度提高了 8 倍以上，是目前速度最快的一代模型。

![](img/4bcc3c5d2c5f544aad88f5d685e1ed1e.png)

[4 代速度对比-音乐版.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Cpzibdgg2oLgS5xIbYDca91hnCh)

毫秒级生成速度，为实现项目级辅助编程奠定基础。同时，利用 CodeGeeX 第四代模型的强大能力，插件也实现了一系列的功能创新。

![](img/c87a7d3cbb1238279f6eb488ec8a04cd.png)

### 为项目自动生成 `README` 文件

CodeGeeX v2.12.0 版本可以为项目自动生成 `README` 文件。这个功能可以通过 `@Openedfile` 使用，通过分析已打开的项目代码结构、功能模块和依赖项，智能生成一个包含项目简介、安装步骤、使用方法、示例代码和贡献指南的 `README` 文件。

![](img/d122016b959f9a9301f00e66d9462b50.png)

[自动生成 readme-音乐版.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/D4Znb2q2PoNIx1xn6Akc0odMnNj)

这样，在项目中不仅节省了手动编写文档的时间，还确保了 `README` 内容的准确性和完整性，使你的项目更加专业和易于理解。无论是开源项目还是个人开发，自动生成的 `README` 都能帮助你更好地展示和分享你的代码。

### 项目级长文本上下文的记忆和理解

CodeGeeX 基于第四代模型的 128K 长文本能力，能够在一个包含 10000+ 行代码的项目文件中，准确提取一个特定变量的信息。在 CodeGeeX 插件中打开项目文件，如果不记得文件海量代码中的某一个变量名，就可以利用 `@currentFile` 来使用这个功能。

![](img/132358325be48a38e777a88956a216cd.png)

[10000+ 行代码找变量-音乐版.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BIuSbSe9ooER7LxSyicc5rFTn9b)

只需简单描述你要查找的变量及其相关功能，CodeGeeX 就可以在 10000+ 行代码中，快速定位该变量所在的位置，并提供详细的信息和上下文。这一功能大大提高了在大型代码库中查找和理解某一变量的效率，使得代码维护和调试变得更加便捷。

### 支持项目中的跨文件分析和问答

CodeGeeX 支持项目中跨文件的分析和问答功能，能够在项目中跨越多个文件进行智能检索和信息提取。无论是查找某个变量、函数，还是了解特定逻辑的实现细节，你只需提出问题，CodeGeeX 会在整个项目中进行深入分析，并提供准确、详细的回答。

![](img/f470199203a4ea6ba944bcf38d29caf0.png)

[跨文件分析与问答.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/)

这一功能帮助开发者更高效地理解复杂代码库，大幅提升代码查找和问题解决的速度，让项目维护和开发更加轻松。

### 支持本地模式

CodeGeeX v2.12.0 开始支持本地模式，允许在插件中接入本地模型，从而更灵活地使用插件。选择进入本地模式 CodeGeeX 能够直接配置本地模型的 API 地址，连接插件后就可以在离线状态下，继续获得本地模型提供的代码补全和智能问答能力。

![](img/f470199203a4ea6ba944bcf38d29caf0.png)

[本地模式-音乐版.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/)

这一功能大幅提高了响应速度，还增强了数据隐私和安全性，使得开发者在不同网络条件下都能使用 CodeGeeX 智能编程助手高效工作。同时，对于在本地部署 CodeGeeX4-ALL-9B 模型的用户，也可以深度「把玩」模型，通过接入插件，挖掘更多精彩的辅助编程体验。

### NL2SQL 能力大幅提升

CodeGeeX 模型的升级带来了 NL2SQL 能力的显著提升。现在插件中可以通过 `@currentFile` 的方式，对 SQL 文件使用自然语言直接生成复杂的 SQL 查询。

![](img/f470199203a4ea6ba944bcf38d29caf0.png)

[NL2SQL-音乐版.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/)

这一功能大大简化了数据库查询的编写过程，即使不熟悉 SQL 语法的开发者也能轻松构建高效查询，从而提升开发效率并减少出错机会。

未来几周，我们将陆续推出基于第四代 CodeGeeX 模型强大能力的更多插件新功能。

## 三、立刻体验 CodeGeeX

升级您 IDE 中的 CodeGeeX 插件，或者直接在 IDE 的插件市场搜索「CodeGeeX」免费下载，使用 CodeGeeX 最新的技术。也可以在 Github 上下载 CodeGeeX 第 4 代模型的 RepoDemo，按照下面视频中的方式，在自己的电脑上部署一套专属项目级智能编程助手。

![](img/f470199203a4ea6ba944bcf38d29caf0.png)

[codegeex-githubdemo.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/)

我们期待与你一起，用 CodeGeeX 创造更多精彩的产品和体验。

此外，今天我们同时发布企业级 CodeGeeX 4 代软硬一体机产品，基于 4 代大模型的强大能力，与企业代码仓库深度融合，实现代码仓库级补写、问答和检索增强；软硬一体设计，一键部署、开箱即用，更快、更稳定；支持从硬件、操作系统、数据库到模型的完整信创方案；更大尺寸模型自由组合，更多企业级应用场景......详情请扫以下二维码咨询。

![](img/f470199203a4ea6ba944bcf38d29caf0.pngIoBCbp64kofRCixG3wjcpbEInUe/)

添加图片注释，不超过 140 字（可选）
