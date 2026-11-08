# 快速入门

> 来源：[https://xuqiwei1986.feishu.cn/docx/CWFFdqyyAoJTG4xavsccBSXznMg](https://xuqiwei1986.feishu.cn/docx/CWFFdqyyAoJTG4xavsccBSXznMg)

![](img/93b9e084cd183a48590a5f3997a445d1.png)

# BISHENG新鲜事

+   [3月9日]BISHENG团队全新开源产品Clawith（OpenClaw🦞 for Teams）预览发布：https://github.com/dataelement/Clawith/

+   [2月6日]BISHENG 2.4-beta1 版发布

+   [1月28日]BISHENG 2.3-release 版发布

+   [1月6日]BISHENG 2.3-beta4 版发布

+   [12月23日]BISHENG 2.3-beta3 版发布

+   [12月12日]BISHENG 2.3-beta2 版发布

+   [11月28日]BISHENG 2.3-beta1 版发布

+   ...

> 订阅BISHENG文档更新

![](img/a12bc5371549f647cc02d5711ef4ddea.png)

# 简介

BISHENG 是一款 **开源**** AgentOps**，专攻**办公场景**， 已有大量行业头部组织及世界500强企业在使用。

BISHENG团队：开放诚恳、长期主义、关注业务价值不炫技，为帮助企业实现LM应用落地，全力以赴！

> “毕昇”是活字印刷术的发明人，活字印刷术为人类知识的传递起到了巨大的推动作用。我们希望“BISHENG”同样能够为智能应用的广泛落地提供有力支撑。欢迎大家一道参与。

### 1.1 BISHENG特点

> 详见：[BISHENG与同类平台对比](https://dataelem.feishu.cn/wiki/FKC8wbDysiwCb0ktScscBXLZnZf)

1.  具备专家级品味的通用Agent **灵思**（企业版Manus）：通过 [AGL](https://github.com/dataelement/AgentGuidanceLanguage)（Agent Guidance Language），将领域专家的偏好、经验与业务逻辑融入 AI 之中，让 Agent 在处理任务时能具备 「专家级品味」，实现远超同类通用Agent的效果。

![](img/eafb3f4b0443596e4279a037ae6fd4e1.png)

1.  独具特色的[BISHENG workflow](https://dataelem.feishu.cn/wiki/R7HZwH5ZGiJUDrkHZXicA9pInif)

1.  🧩独立、完备的应用编排框架：可在一个框架下实现各类任务（同类workflow产品需要被 bot 调用，或划分成 chatflow 与 workflow 来完成不同类型的任务）。

1.  🔄 Human in the loop：支持用户在Workflow执行的中间过程进行干预和反馈（包括多轮对话），而同类产品只能从头执行到尾。

1.  💥 强大：支持成环、并行、跑批、判断逻辑以及所有逻辑的任意自由组合；支持多类型输入输出、撰写报告、内容审核等复杂场景。

1.  🖐️ 易用、符合直觉：如成环、并行、批量运行操作，在同类产品中用户需借助专门组件实现，在BISHENG中只需完全按照直觉连接成“流程图”即可（画圈成环、并列即并行、多选即批量）。

![](img/a6fb67303fbb46c90a35b0b1cbd309f7.png)

1.  专为企业应用而生：文档审核、固定版式报告生成、多智能体协作、规范制度更新差异比对、工单问答、客服辅助、会议纪要生成、简历筛选、通话记录分析、非结构化数据治理、知识挖掘、数据分析...平台支持高复杂度企业应用场景构建，支持数百个组件与数千个参数的深度调优。

![](img/2662515525797808c500749f1d2317b9.png)

1.  企业级特性是应用落地的基本保障：效果自评测、使用数据回流标注、数据集管理、微调、安全审查、RBAC、用户组管理、分组流量控制、SSO/LDAP、漏扫修复、高可用部署、监控、统计...

![](img/285bef1b73ff7e98498d25457cd8ebea.png)

1.  高精度文档解析：5年海量数据沉淀，高精度文档解析模型支持免费私有化部署使用，包括高精度印刷体、手写体与生僻字识别模型、表格识别模型、版式分析模型、阅读顺序模型、公式识别模型

![](img/125add7a0470e93e67bbb2c4684c6c43.png)

1.  大量企业场景落地最佳实践分享社区：开放的应用案例与最佳实践库。

### 1.2 开源

⭐️ BISHENG于2023年8月30正式基于Apache 2.0协议开源（无任何附加协议），可二开商用，Github地址：https://github.com/dataelement/bisheng

![](img/e402ab98b7872c3adb1c171222380fe3.png)

### 1.3 Demo 体验地址

> **Demo环境地址：**[www.](http://www.bisheng.ai)[bisheng.ai](http://www.bisheng.ai)

> 直接注册账号即可进行试用，demo环境仅用于功能体验

### 1.4 BISHENG Blog

<https://dataelem.feishu.cn/sync/DLa1dReY8sN6MWbp9NNcygEanVh>

### 1.5 在使用BISHENG的组织

感谢大家对BISHENG的信任

由于BISHENG是开源软件，可能有许多正在使用BISHENG的组织我们并不知道，还请大家联系我们添加到以上列表中。

---

# 应用场景

使用BISHENG平台，我们可以搭建各类丰富的大模型应用：

![](img/b39872a6ef29640baae2efbd4d4561bc.png)

具体演示视频见：[手搓100个Workflow【活动】](https://dataelem.feishu.cn/wiki/OJYRwuJzZiUHhhkI4MLcMJvfnzh)

我们认为在企业真实场景中，“对话”仅是众多交互形式中的一种，未来我们还将新增自动化任务、搜索、智能看板等更多应用形态的支持。

---

# 常用资源链接

| 链接 | 说明 |
| --- | --- |
| [毕昇商业拓展套件介绍](https://dataelem.feishu.cn/wiki/Ie9mwjuJNihYE1k3nSJcfhFfnnc) | 面向客户，商业扩展套件能力说明及收费模式说明 |
| [BISHENG达人1对1专家服务](https://dataelem.feishu.cn/wiki/Vlubwh4jliLjPKkIS2pczTvRn0e) | 我们从BISHENG社区中陆续发掘出多名优秀伙伴（BISHENG达人），专门为大家提供收费的1对1支持服务。由BISHENG官方作为中间方，承诺未完成任务全额退款。 |
| [毕昇合伙人](https://dataelem.feishu.cn/wiki/MtRxwBjeQifDgtkVeh9cK9ernOe) | 面向合作伙伴，合伙人权益说明 |
| [场景列表](https://dataelem.feishu.cn/wiki/BlAZwupR7iNVXzk4SLIces55nqe) |  |
| [私有化部署](https://dataelem.feishu.cn/wiki/BSCcwKd4Yiot3IkOEC8cxGW7nPc) | 平台私有化部署方法。 |
| [BISHENG workflow](https://dataelem.feishu.cn/wiki/R7HZwH5ZGiJUDrkHZXicA9pInif) | [【活动】BISHENG Workflow 展览](https://dataelem.feishu.cn/wiki/OJYRwuJzZiUHhhkI4MLcMJvfnzh) |
| [产品功能用户手册](https://dataelem.feishu.cn/wiki/BWSMwdc2Ciwif4kfe5HcyGHgn3b) |  |
| [经验技巧](https://dataelem.feishu.cn/wiki/OWFRwknFaiIMajke4m5cFeLrnie) | 具体场景落地过程中，一些实用的效果优化经验技巧。 |
| [问题排查手册 ](https://dataelem.feishu.cn/wiki/JvyJwh0priBOIrkhJF5cLeVMnvg) | 出现常见问题排查与解决方法。 |
| [开发者指南](https://dataelem.feishu.cn/wiki/ITmJwMXVliBnzpkW3nkcqPVrnse) | BISHENG各个模块的技术说明，以及代码PR方法说明。 |
| [FAQ](https://dataelem.feishu.cn/wiki/XdGCwkDJviC0Z8klbdbcF790n9b) | 关于BISHENG的常见问题。 |
| [大模型基础知识入门](https://dataelem.feishu.cn/wiki/HMcKwjfA3iJEEXk5c3ucViqunif) |  |
| [BISHENG产品文档离线版](https://dataelem.feishu.cn/wiki/USLnw2I16iNnv0kCkmDcD9KgnCb) | 由于许多企业内部无法访问外网，故提供离线版本，约一个月更新一次 |
| [毕昇BISHENG品牌视觉素材](https://dataelem.feishu.cn/wiki/VHBnwbeqMiF🐧6kWfCqclOCLnyb) | LOGO、Slogan图像文件 |

---

# Release Note

+   未来规划见：[Roadmap](https://dataelem.feishu.cn/wiki/PLOTwKrzpiQrR8k3OnLcjElVnRe)

+   升级指南见：[版本升级注意事项](https://dataelem.feishu.cn/wiki/WNqbwm8zyiPo9ukIw5ecCWcfnPe)

| 发布时间 | 发布内容 |
| --- | --- |
| 2026 年 3 月 10 日 | BISHENG 2.4 beta1-hotfix 版  修复日常模式个人知识库查询失效的问题 |
| 2026 年 2 月 6 日 | 🚀 BISHENG 2.4 beta1 版 |
| 2026 年 1 月 28 日 | 🚀 BISHENG 2.3-Release 版 |
| 2026 年 1 月 6 日 | 🚀 BISHENG 2.3-beta4 版 |
| 2025 年 12 月 23日 | **🚀  ****B**ISHENG 2.3-beta3 版 |
| 2025 年 12 月 12 日 | 🚀 BISHENG 2.3-beta2 版 |
| 2025 年 11 月 28 日 | 🚀 BISHENG 2.3-beta1 版 |
| 2025 年 11 月 11日 | 🚀 BISHENG 2.2-release 版 |
| 2025 年 10 月 17日 | BISHENG 2.**2-beta3 版  **🚀 1. 灵思结果文件支持 Word / PDF 导出： Markdown 文档支持一键导出为 Word 和 PDF 格式。  ⚙️ 2. 灵思代码执行器文件展示增强：代码执行器（含 E2B 模式、本地模式）生成的文件，现在可直接展示于任务结果中。  🧑 3. 灵思 Human-in-Loop 能力升级：灵思主动寻求用户帮助时，弹窗新增 文件上传与拖拽功能。  ✨4. 灵思体验优化： |
| 2025 年 9 月 25 日 | BISHENG 2.2-beta2 版 |
| 2025 年 9 月 19 日 | BISHENG 2.2-beta1 版 |
| 2025 年 9 月 18 日 | BISHENG 2.1 release 版 |
| 2025 年 8 月 13 日 | BISHENG 2.0 release 版 |
| 2025 年 6 月 20 日 | **BISHENG 1.3.1 版**  修复工作流开始节点引导问题未生效问题 |
| 2025 年 6 月 19 日 | 详细介绍文章：[硬核进化！BISHENG 1.3.0 ](https://mp.weixin.qq.com/s/X2vtc5igMtJCGvIxhinLLg)发布：用实力，定义企业级知识库的“好用” |
| 2025 年 5 月 29 日 | BISHENG 1.2.1 release 版 |
| 2025 年 5 月 20 日 | BISHENG 1.2.0 release 版 |
| 2025 年 4 月 22 日 | BISHENG 1.2.0 dev1 版 |
| 2025年4月22日 | BISHENG 1.1.1  版  修复了 1.1.0 release 知识库分段编辑报错 的问题。 |
| 2025年4月17日 | **BISHENG 1.1.****0**** release 版** |
| 2025 年 3 月 5 日 | BISHENG 1.0.1 hotfix: |
| 2025 年 3 月 4 日 | BISHENG 1.0.0 release 版 |
| 2025 年 1 月 9 日 | BISHENG 0.4.1.2 release 版 |
| 2025 年 1 月 7 日 | BISHENG 0.4.1.1 release 版 |
| 2024 年 12 月 6 日 | BISHENG 0.4.0 dev2 版 |
| 2024 年 11 月 21 日 | BISHENG 0.3.7 release 版 |
| 2024 年 10 月 11 日 | BISHENG 0.3.6 dev1 版 |
| 2024 年 9 月 14 日 | BISHENG 0.3.5 dev1 版 |
| 2024 年 9 月 2 日 | BISHENG 0.3.4 release版 |
| 2024 年 8 月 21 日 | BISHENG 0.3.4 dev3版 |
| 2024 年 8 月 16 日 | BISHENG 0.3.4 dev1 版 |
| 2024 年 8 月 2 日 | BISHENG 0.3.3 dev2 版 |
| 2024 年 7 月 17 日 | **BISHENG****商业拓展套件 0.1 版** |
| 2024 年 7 月 12 日 | BISHENG 0.3.3 beta 版 |
| 2024 年 6 月 28 日 | BISHENG 0.3.2 release 版 |
| 2024 年 5 月 21 日 | BISHENG 0.3.1.1-0.3.1.2 release 版本 |
| 2024 年 5 月 16 日 | BISHENG 0.3.1 release 版本 |
| 2024 年 4 月 17 日 | BISHENG 0.3.0 release 版本 |
| 2024 年 4 月 11 日 | BISHENG 0.3.0 Beta 版本 |
| 2024年3月22日 | BISHENG 0.2.3.2版本 |
| 2024年3月3日 | **BISHENG 0.2.3版本** |
| 2024年2月8日 | BISHENG 0.2.2.5版本 |
| 2024年1月25日 | BISHENG 0.2.2.3版本 |
| 2024年1月20日 | BISHENG 0.2.2.2版本 |
| 2024年1月3日 | BISHENG 0.2.1.2版本 |
| 2023年12月26日 | **BISHENG 0.2.1版本** |
| 12月6日 | **BISHENG 0.2.0版本** |
| 11月23日 | BISHENG 0.1.10版本 |
| 11月12日 | BISHENG0.1.9.5版本 |
| 11月1日 | **BISHENG0.1.9版本** |
| 10月24日 | **BISHENG 0.1.8版本** |
| 10月17日 | **BISHENG 0.1.7版本** |
| 9月8日 | 支[ProxyChatLLM](https://dataelem.feishu.cn/wiki/A65GwSXT6iscfikI0alcbgxAndf)直接在BISHENG平台使用主流大模型服务，包括OpenAI、百度、讯飞、智谱、MInimax，详细使用方法见： |
| 9月5日 | DEMO地址上线：https://bisheng.dataelem.com/ |
| 8月28日 | BISHENG正式开源 |

🔀 1**.1 **输**入节点逻辑重构与增强：**

1）**策略解耦****：拆分输**入节点文件处理策略**，支持****选择“解析内容**”（提取文本）、“不解析”（保留原始文件供多模态/代码节点使用）或“存入临时知识库”，满足多样化业务场景；

2）**智能推荐问题**：支持配置基于本次对话上下文，自动生成并展示 3 个推荐问题。

🖼️ 1**.2 **报**告节点渲染优化：**支持识别 M**arkdown 标题及**加**粗样**式，并自动转换为 Word 文档样式，提升文档输出美观度。

+   **开源版**：预置标准看板，涵盖用户数量、应用数量、知识库数量、用户反馈 4 大核心维度数据。

+   **商业版（**[**商业拓展套件**](https://dataelem.feishu.cn/wiki/Ie9mwjuJNihYE1k3nSJcfhFfnnc)**）**：预置**全量看板数据**，支持基于 11 张中间表自定义组件维度及指标，并支持多看板管理。

**🔄 2.1 全生命周期管理：**支持看板的新建、复制、重命名、删除、发布、分享与下线，管理后台支持按角色授权看板管理和使用。

**✨ 2.2 拖拽式布局与交互**：支持看板组件的**自由拖拽、缩放、复制**，支持全屏演示模式。

**📈 2.3 丰富的看板组件：**内置柱**状**图（**基础/堆叠/分组**）、条形图（基础/堆叠/分组）、折线图（基础/堆叠/面积）、饼图、环形图、指标卡以及全局时间查询组件，满足多样化数据展示需求。

**🔧 2.4 精细的图表配置：**支持维**度/指标拖拽配置、**数**值格式自定义、**复**杂筛选和排序条件以**及个**性化样式配置。**

**📚 3.1 **知**识库文件解析优化：**优化 docx、ppt 文件的处理逻辑，解决部分边界情况解析失败问题。

**👥 3.2 **系**统管理优化：**支持按角色授权“工作台”与“管理后台”空间访问，增加对看板操作行为的审计日志记录。

为知识库提供精细化的文档组织和检索能力，支持在工作流中实现精准的文档召回。

1.1 🏷️ 知识库级元数据定义：支持 String、Number、Time 三种数据类型；提供 6 个内置元数据字段（document_id、document_name、upload_time、update_time、uploader、updater）。

**1.2 📝 文档级元数据赋值：**为单个文档灵活添加、编辑、删除元数据标签。

**1.3 🔍 工作流元数据过滤：**文档知识库检索节点/问答节点支持元数据过滤；支持多条件组合(AND/OR)与丰富的过滤操作符。

1.4 🔌 A**PI 接口支持：**提供元数据增、删、改、查的相关 API 接口。

**2.1 🎨 模型管理优化：**"通义千问"更名为"阿里云百炼"；模型广场新增 10 个主流厂商的 API Key 获取和模型文档快速入口。

**2.2 ⚡ 使用体验提升**：工作流预置问题/变量引用实时同步更新、模型微调权限优化等多项改进。

**1.1 语音输入 (ASR)**：在输入框中点击“麦克风”图标，直接通过语音输入文字。

**1.2 语音播报 (TTS)**：AI 回复消息旁新增了“扬声器”图标，点击即可收听语音播报。

**1.3 模型支持**：后台系统模型设置中支持配置 ASR 模型与 TTS 模型，已支持 OpenAI、Azure、通义千问模型服务提供方格式。

2.1 Markdown** 渲染增强: 会话内**容现**已支持渲染 Echart**s 图表和 Mermaid 流程图。

```
要渲染图表，使用以 ```echarts 开头的代码块，并将配置写入其中,示例如下：
```echarts
{
  &quot;title&quot;: {
    &quot;text&quot;: &quot;ECharts 入门示例&quot;
  },
  &quot;tooltip&quot;: {},
  &quot;legend&quot;: {
    &quot;data&quot;: [&quot;销量&quot;]
  },
  &quot;xAxis&quot;: {
    &quot;data&quot;: [&quot;衬衫&quot;, &quot;羊毛衫&quot;, &quot;雪纺衫&quot;, &quot;裤子&quot;, &quot;高跟鞋&quot;, &quot;袜子&quot;]
  },
  &quot;yAxis&quot;: {},
  &quot;series&quot;: [{
    &quot;name&quot;: &quot;销量&quot;,
    &quot;type&quot;: &quot;bar&quot;,
    &quot;data&quot;: [5, 20, 36, 10, 10, 20]
  }]
}
```
```

**2.2** **对话分享功能:** 新增“分享”按钮，您可以生成一个公开的、只读的分享链接，安全地将对话结果展示给他人。

**3.1 新增“知识库检索”节点**：在工作流中可单独使用的文档知识库检索节点，允许开发者更灵活地在工作流中执行检索操作，并获取结构化的检索结果（包含 metadata 和 text）。

**3.2 高级检索设置**：在“知识库检索”节点和原有的“文档知识库问答”节点中，**现已开放“高**级检索设置”。您可以自由**调配混合检索**权重（关键**词 vs 向量）、检索结**果长度，并可启用Rerank（重排）模型进行二次排序。

**4.1 高级模型参数**：模型管理（LLM/Embedding）支持高级参数配置，你可以通过 JSON 格式传入各家厂商（如阿里云、火山引擎）所需的特定参数，例如 temperature、enable_thinking（以便支持混合思考类模型思考模式配置需求） 等。

4.2 百度千帆云平台鉴权逻辑修改：千帆平台已更新为 OpenAI 兼容的模式（使用 Base URL 和 API Key），请使用百度模型的用户注意更新配置。

7.1 缺陷描述：当文档知识库和 QA 知识库使用同一个 embedding 模型的时候，对该文档知识库/QA知识库内容进行修改操作，可能会影响另一方在 Milvus 中的数据，导致检索结果不符合预期。

7.2 具体原因：Milvus 向量库的 partition 模式下**，同一个 embed**d**ing 模型的多个知**识库共**享**一个 collection，而文档知识库 中的文件和QA知识库的QA对，二者的 file_id 有概率冲突（不同的表，自增ID），因此在操作相同file_id的文件或者问答对，都会影响对方在Milvus中数据。

7.3 缺陷解决方式：① 升级到 2.[版本升级注意事项](https://dataelem.feishu.cn/wiki/WNqbwm8zyiPo9ukIw5ecCWcfnPe#share-MTpPdLnTXo0hl3xN7PqcXp6NnBe) release 版本；② 使用脚本（ ）扫描知识库中是否存在有问题的知识库；③ 根据需要执行脚本修复所有知识库。

4.1 任务结果新增“仅反馈”按钮，方便提交用户意见，而无需重新运行，后台可查看反馈内容。

4.2 工具调用增加时间戳展示。

4.3 任务描述区新增 用户上传文件信息展示。

![](img/8fba63111cd7dc29642d81c87c7dd482.png)

**1.1 **🏁 B**ISHENG 工作台独立上线：****定位为面向业务人员的** AI 应用门户；原有 BISHENG 平台将聚焦为管理与构建后台。

**1.2 🗂️ 应用中心上线：**集中展示已构建的各类应用，支持按标签分类与设置常用应用。

**1.3 📚 个人知识库优化：****1）支持管理员在系**统模型设置中切换 Embedding 模型并重建个人知识库；2）个人知识库问答效果优化。

1.4 ⏱️ 日常模式对话系统提示词支持传入时间变量。

![](img/60f2c00b864fd3797870e80c057e1299.png)

**2.1 ✨ UI/交互全面优化**：信息更清晰、操作更流畅，支持**批量操作**与**已上传文件的分段策略调整**。

2.2 🔄 文档知识库支持切换 Embedding 模型并重建。

**3.1 **🖼️ 报**告节点增强：**Word 模板支持插入 Markdown 图片/图片文件、Markdown 表格/表格文件。

**3.2 **👤 开**始节点新增 u****ser_info：**返回用**户名、用户组、角色信**息，便于做基于用户身份的逻辑处理。

**3.3 **🗄️ 助**手节点数据库查询扩充：**新增对 DB2、PostgreSQL、GaussDB、Oracle 的查询支持。

**3.4 **🏷️ 工**作流 API 回参增强：**新增节点名称字段 n**ode_name，**便于定制化处理与日志追踪。

**3.5 **🔧 工**具节点入参更智能：**支持自**动数据类型转换，**降低参数校验与清洗成本。

**4.1** 🧪 微**调能力升级：**L**LaMA-Factory **升级至最新稳定版本；可微调模型列表同步扩展至当前主流新模型。

**4.2 **🔌 模**型管理拓展：**新增 华**为 MindIE **接口格式。

**5.1 **🧠 本**地推理模型效果渲染：**O**llama、MindIE **等本地推理框架思考内容支持 <think> 样式渲染。

**5.2 **🛰️ 助**手 API 实**现实**时流式响**应：显著优化等待体验与交互反馈。

**5.3 🔗 工具生态更开放：**支持s**treamable 协议 ** M**CP 工**具接入。

**5.4 **🧪 评**测能力增强：**评测功能支持一**问一答类**型的工作流。

1.  **多模态模型支持**：输入节点新增图片变量提取功能，大模型/助手节点新增视觉理解功能，可调用多模态模型解析和回复图片内容。

1.  **输入节点对话框模式增强**：支持上传文件并且解析全文内容。

1.  助手节点 nl2sql 方案优化：提升数据库查询准确性和响应速度。

1.  输出节点消息内容现可作为全局变量，支持被其他外部节点引用。

1.  工作流编排及使用体验优化。

1.  BISHENG商业拓展套件现已支持对工作流会话内容进行敏感词安全审查。

1.  新增了部分主流云厂商模型接口，并且预置了部分常用模型。

1.  阿里云百炼平台切换为 DashScope SDK，解决了 embedding 模型批量请求 batch size 超限问题。

1.  LLM 模型支持设置 max_completion_tokens/max_output_tokens/num_ctx 等参数。

1.  新添加模型后，系统会自动更新并同步模型状态。

1.  定位：**未来的企业内 AI 应用门户**。

1.  功能：

1.  新增独立页面，支持 DeepSeek 样式交互对话

1.  支持多模型选择和切换

1.  支持联网搜索问答

1.  支持直接上传多份文件问答

1.  支持员工创建和管理个人知识库并问答

1.  BISHENG工作台后续规划：增加 Deep Research 等通用 Agent，增加“应用中心”展示在BISHENG中构建好的各种场景应用。成为面向企业业务用户统一 AI 应用入口。（现有BISHENG逐渐演变为管理和构建后台）

1.  支持BISHENG平台工作台入口开关配置

1.  支持自定义图标与欢迎语等

1.  支持配置工作台对话模型

1.  支持配置联网搜索、个人知识搜索、文件上传等功能开关以及提示词

1.  输入节点支持单文件全文解析和文件路径获取；

1.  优化工作流构建和调试过程中的体验，包括但不限于提示词编辑、节点日志查看、变量有效性校验等问题；

1.  解决工作流中并行边和互斥边（例如条件分支节点）共存时可能存在的逻辑冲突问题，修复报告节点 word 样式丢失的问题，修复其他已知的缺陷问题。

1.  优化 arxiv、bing 内置工具的检索逻辑。

1.  新增以下内置工具：Firecrawl、发送邮件、Jina AI 单网页爬取、飞书消息、企业🛰️消息、钉钉消息、硅基流动Stable Diffusion/Flux 生图工具。

1.  自定义工具支持 custom 自定义鉴权。

1.  **独立、完备的应用编排框架**：可在一个框架下实现各类任务（同类产品需要被bot调用，或划分成chatflow与workflow来完成不同类型的任务）。

1.  Human in the loop：支持用户在Workflow执行的中间过程进行干预和反馈（包括多轮对话），而同类产品执行过程中无法被中断。

1.  **强大**：支持成环、并行、跑批、判断逻辑以及所有逻辑的任意自由组合；支持多类型输入输出、撰写报告、内容审核等复杂场景。

1.  **易用、符合直觉**：如成环、并行、批量运行操作，在同类产品中用户需借助专门组件实现，在BISHENG中只需完全按照直觉连接成“流程图”即可（画圈成环、并列即并行、多选即批量）。

> 如需替换 workflow 页面中BISHENG logo，可在系统配置中修改主题配色

![](img/9607a082c9b84dc65e47175972afa731.png)

![](img/6e3fbda07c72a723bb52ef3fa7ea1787.png)

1.  **兼容多种推理框架**：正式支持主流开源 LLM 推理框架（ollama/llamacpp/xinference/vllm）部署模型及在线模型快速接入，并提供模型上下线操作和单日调用量控制能力。

1.  **系统配置大幅简化**：知识库、助手、评测等功能所依赖的模型可在模型管理中进行可视化配置，内置工具（Dalle3绘画、Bing 搜索、天眼查）秘钥在工具页面中进行可视化配置。

1.  基于以上功能，大幅简化了部署和配置使用BIS[私有化部署](https://dataelem.feishu.cn/wiki/BSCcwKd4Yiot3IkOEC8cxGW7nPc)ENG的步骤，部署文档也变得格外清爽：

![](img/5cf15e14afda429781af0993726c9d20.png)

![](img/46f2a4572989428d86fcf2a177587761.png)

1.  提供 bisheng-rt-llamacpp 镜像，支持 qwen1.5 32b 72b 模型与 OpenAI 兼容的多轮 function call 能力。

1.  **改进工具描述和代码逻辑**，提高助手在开源模型上调用工具的效果。

1.  预告：在0.3.0.1 迭代（预计 4.19 发布）我们会增加对 Cohere  Command-R 开源模型的适配。

1.  助手创建与配置：低门槛创建助手，并可自动完成 prompt 优化和其他配置，支持关联知识库、工具以及BISHENG技能。

1.  助手管理：对助手上下线，以及对不同角色授权助手使用。

1.  助手使用：在会话中选择助手发起对话。

1.  工具体系：除了 **Bing搜索****、****Dalle 画图、****代码解释器**等基础工具，我们还提供了一批**企业特色场景**的预置工具，例如**天眼查**系列工具、**股票实时/历史行情**数据查询工具、**宏观经济金融数据**（货币供应量、PPI、CPI、社融、GDP … ）

![](img/198b5bd3da6692fee442f2c1904a16ee.png)

1.  支持对多组件进行 group / ungroup 操作，group后称为“组件集合”，如RetrievalQA以及所有依赖的组件可以被group成一个“知识库问答”组件集合。

1.  **支持组件save操作**，所有组件或者group后的“组件集合”支持通过save操作保存到“save”类中，方便后续复用。

1.  **支持组件级别导入导出**，方便进行技能构建的协作。

1.  【组件】所有loader类型组件的file path触点增加必填校验

1.  【技能】技能名称过长时会和上线技能按钮重叠问题

1.  【引导词】L1行高展示问题

1.  【知识库】知识库中文件较多时，切页后筛选状态无法得到筛选结果问题

1.  【免登录】免登录链接不展示引导词问题

1.  【会话】历史会话引导词展示问题

1.  注意：Qwen http流式预估时，do_sample必须为True，详见官方issue

1.  集成ChatGLM3 的 Function Call能力，支持ChatG[ChatglmFunctionsAgent](https://dataelem.feishu.cn/wiki/IFa3wpDD9i20cakjxdgcfvidnDe)M3-6B（以及未来智谱开源的其他更强模型）实现Function Call

1.  新增LLMFunctionsAgent组件，支持Qwen[LLMFunctionsAgent](https://dataelem.feishu.cn/wiki/MtIWweP63i46Xrk1EWWcg376nBe)源模型实现Function Call

1.  新增ChatQwen组件，支持接入qwen_turbo、qwen_plus模型

1.  ChatOpenAI组件增加gpt-4-1106-preview 模型供选择

1.  系统默认语言跟随浏览器语言设置

1.  会话页面新增“赞/踩”按钮，信息会记录到数据库中（后续会支持导出报表）

1.  知识库上传文件时手动设置切分模式下，新增“切分文本重叠长度/Chunk Overlap”参数

+   基础版支持格式：pdf、md、txt、doc、docx、ppt、pptx、html

+   高阶版支持格式：pdf、png、jgp、jpeg、bmp、txt、doc、docx、ppt、pptx、md、html、xls、xlsx、csv

> 基础版的文件解析使用开源的 [Unstructured](https://github.com/Unstructured-IO/unstructured) 组件

> 高阶版使用 BISHENG-Unstructured 进行文件解析，基于开源 Unstructured 进行了优化，并且引入了高精度文字识别、表格识别、版式分析能力，提升解析质量

+   问答溯源弹窗中支持下载原文件（[答案支持溯源](https://dataelem.feishu.cn/wiki/XvB3wPzZCi7axakhUFTctj37nDe) ）

+   支持在后端配置文件中配置系统为“生产模式”，该模式下不允许用户改动模型服务，保障生产系统稳定性

+   proxychatllm组件中使用openai模型时，支持打字机效果

+   bug修复：配置文件中注释内容太长导致启动报错的问题

# 🛰️群/飞书群

扫码加入🛰️群/飞书话题群

![](img/72dec2772042277883e92b7cf1bd699d.png)

# 1 对 1 付费帮扶对接

[BISHENG达人1对1专家服务](https://dataelem.feishu.cn/wiki/Vlubwh4jliLjPKkIS2pczTvRn0e)

友情链接

+   [MCP中国开发者联盟（MCPA）知识库](https://kcnc1vmsgzlq.feishu.cn/wiki/KTJIwD3Acip77lkWO1gcSWsOnFh)

+   [⭐ 结构化提示词知识库](https://langgptai.feishu.cn/wiki/RXdbwRyASiShtDky381ciwFEnpe)

+   [AIGCLINK：AI定制落地解决方案库](https://trx769zfgq.feishu.cn/wiki/LxpHw7iyuiX66FkebN0ceQtFnVb)