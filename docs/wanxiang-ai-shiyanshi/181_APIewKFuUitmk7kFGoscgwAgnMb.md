
# 搭积木：n8n+飞书多维表格=免费自动reddit需求挖掘机+产品开发文档（附模板）

> 来源：[https://yunyinghui.feishu.cn/docx/GK7Hd57TQoiKkvxAUZFc0fJQnqf](https://yunyinghui.feishu.cn/docx/GK7Hd57TQoiKkvxAUZFc0fJQnqf)

## 前言

随着 vibe coding 的技术进步，做 web 出海的朋友越来越多了，如何挖掘真需求一直是个头疼的问题，而 reddit 被誉为出海金矿，可以直接在上面挖掘海外用户的真实需求。

本文将教会你：

搭建一个自动需求挖掘机，只需设定好搜索关键词，每天自动收集 reddit 上的用户需求到多维表格，并且可以自动分析，呈现出可视化商业分析报告和产品开发文档（PRD, Product Requirements Document），可以直接丢给 cursor、claude code、v0 等等 vibe coding 工具直接开发，节省 mvp 产出时间。

其实已经有圈友在赚钱发过 n8n 挖掘 reddit 需求相关的帖子了，本文在整合这些工作流基础上添加进了飞书多维表格利用自带 ai（deepseek）来处理数据的逻辑。

文章中直接给出 n8n 工作流源文件和飞书多维表格模板，可以直接上手。

废话不多说，直奔主题，阅读本篇文章，你将获得：

## 效果展示

### 1、输入

自定义多维表格输入界面可以自定义关键词，reddit 小组，每个搜索请求（每行）要设一个 `id`

![](img/ea0470a3ec94822b96544c0395d19d82.png)

### 输出结果

可以每天自动定时抓取 reddit 标题、正文、原贴链接、发布时间、小组

![](img/02831ac789f2e51bf9b1388b233cf3df.png)

![](img/42b185402633197b354892384df850ae.png)

多维表格自带的字段捷径可以帮我们生成：

### 1、帖子内容中文总结

![](img/735cebacb35479f4cfab657ae0fbb152.png)

### 2、deepseek 商业分析报告（可视化网页版）

![](img/03a9e52774179d6abd3d643f8e2aff48.png)

直接可以点击生成网页版可视化商业分析报告

![](img/6647208b3358c73cd546526f0b0ba6d1.png)

### 3、直接可用的产品开发文档

（丢给 vibe coding 工具，可以马上直接做出 mvp）

![](img/36b596f2b855683c7d87351dc8b160ca.png)

[`产品开发文档.md`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LRXdbXZd4op0GkxUDIhcQHqunYc)

## 操作步骤

### 1、n8n 部署

首先，n8n 的部分记得开魔法

我这里用的是 clawcloudrun，注册超过 180 天的 github 账号可以直接白嫖每月 5 美元的额度

官网地址：

https://ap-northeast-1.run.claw.cloud/signin

![](img/5e61da907e97edd3eaedfb44028e6018.png)

![](img/7793cb872794137952ef2d072621de06.png)

github 账号登录即可，如果没有老 github 号，只能首月 5 美元，可以换号白嫖，不过要重新部署了

![](img/b4f0047829e4f9351a67e78c99300f81.png)

这里选的美国西区，可能 reddit 比较稳定

![](img/6ff4938ef7bd60fc449bb704fb9f8805.png)

搜索 n8n

![](img/e37e3db57f632e5d82e3ac97ac2a17c0.png)

点 `deploy` 部署，每天 0.1 美金，一个月正好 3 美金，赠款 5 美金绰绰有余了

![](img/b4eb256df3e89dd1f285afbb22460526.png)

部署完之后桌面找到部署好的 n8n，打开

![](img/189d461a4b4881bcc21fa389bdd51775.png)

n8n 新账号要注册

![](img/ac28402c7d37cfd6851e6124c6504f1e.png)

注册完以后点击 `setting`，先安装飞书 mcp

![](img/446470457af357d7568f4f3f07f72c0f.png)

点最下面 community nodes

![](img/9fa12265427dfa1a56ec910c83c0eca0.png)

点击 `Install`，填写我们要安装的界面名称，

```
n8n-nodes-feishu-lite
```

![](img/67c8704651185cb3f672af3bd1b6c9db.png)

然后再点击 Install，完成

回到主界面，点击 `import from file`，下载我已经配置好的工作流（见下方）

![](img/6d255a4b68e168167378b1ba037b1195.png)

导入文件

![](img/e06c14a0a5b5f6dd11b01f050d36bafd.png)

这样 n8n 部署完成

### 2、模板下载

#### 1、已配置好的 n8n 工作流

![](img/363bc298f9483e6f14dc78464a5afe23.png)

[`n8n_feishu.json`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GK5wblCn3o2SssxakkfcdA0mnDc)

#### 2、飞书多维表格模板，右上角使用

链接：

[n8n-reddit](https://quih408vgi.feishu.cn/base/FcDcbNiCPaYBTCsCNY1cdKjcnsb?table=tblrkibIaQ7vCDM3&view=vew3B1iLoW)

![](img/b1cb809c72c06d6d781a8579c89b6c0d.png)

### 3、n8n 工作流程

整体流程如下

![](img/dfc3239e5306e56dbff0431e043c5614.png)

#### 1、自动触发工作流

首先右上角 `settings` 修改时区，改成国内时间

![](img/ca02ccecdfa622af466a8f697f15ed77.png)

这里是 shanghai

![](img/fdc2bd2ebd697fea743aa09482f35697.png)

双击第一个节点

![](img/29f03cafa8c253e00d2242b0be2b6e86.png)

![](img/341d4181c75e8c3d7b01bec0e0dbb53f.png)

设置定时触发时间，我这里是每天凌晨 12 点自动运行，想要更短的时间间隔可以自行修改

#### 2、解析飞书表格

双击解析飞书表格地址

![](img/72d145ddd2f910f935f404b4912589b4.png)

飞书的认证，先填入最下方飞书表格链接（用我的模板之后新标签页打开），再点 `create new credential`，

![](img/20c0aad8d1b6a4a6bc5d41dc8c24e6d4.png)

接下来进行认证

![](img/bcbd3c866f4b511be4b97487e873e29d.png)

飞书应用的认证密钥获取，

官网

https://open.feishu.cn/app

创建企业自建应用

![](img/a2b331c93bf25f81417befc44dd17547.png)

输入名称和描述（任意）

![](img/803c614931591f459a35c792d4e9bbb7.png)

记得创建版本（随便填写）

![](img/52bb2186657158d26c3a27312f72b531.png)

![](img/ddce7799f8137b2b4be67aba48cf4000.png)

拉到下面保存-确认发布

点击凭证与基础信息，就能获得 `app id` 和 `key`

![](img/32080f4066f01f54eb6f35990373db6f.png)

复制填入

![](img/974d3a982c806b580ff43ac256946d69.png)

回到飞书应用，设置权限，点权限管理

![](img/8a535fc28ab33a849e987aaef23241f3.png)

开通权限，这里我全都开了

![](img/635814720f61ea151f217b68c8fa6a75.png)

直接全选开通

回到多维表格，右上角三个点添加文档应用

![](img/b22cdc8c2ae8f437929943dd7c82d891.png)

添加文档应用

![](img/7298b7f4aaeca3ac53d49930c74b126d.png)

输入你刚刚创建好的应用名字，可编辑权限打开（一定要可编辑）

![](img/1fc42f0b001f9135618dc2f4f99b1b4e.png)

#### 3、获取多维表格元数据

双击

![](img/b762be2588c08cced1d67a72773c709d.png)

把前面输出的 app token 拖进来

![](img/7d065d47b6fd5960a5f1d78cf9d8e97e.png)

![](img/24fbb2474beb5505bf1c72e989386c37.png)

点 `execute step`

![](img/7c9e0ace63233e0b62ffc312b224f355.png)

表示成功了

#### 4、读取数据表

接下来双击

![](img/f9ea1642055bc65237dc83d8fabbd5eb.png)

填入

![](img/15554aa1bbf3a8da4261d6dda87f8ada.png)

下一个节点是数据格式修改，把数据分开的作用

![](img/5cd232cbfb35a39c370a905ff562ebc9.png)

#### 5、reddit 输入

设置输入给 reddit 的搜索参数：

![](img/003685797dd95b3bd6caee579a1ef89e.png)

#### 6、reddit 节点

![](img/5d2a4e2ac7ede7850800b3f9fec90525.png)

认证密钥部分：

![](img/786ac8082b098d8641732257d9f70b0b.png)

[如何免费自动化寻找蓝海市场：用AI Agent+N8N工作流在Reddit上发现真实痛点](https://eafz7mo3of.feishu.cn/docx/Y2kkdmdPFolDJlxkLlKcY4Sunjd)@达轮 的这篇帖子写的非常详细了，照做就行

完成认证之后，把搜索的参数给到，（可以选搜索帖子的数量，模板里面默认是 5 篇）

![](img/be094e7347a4f304c3546e1c29e24544.png)

`Return all` 是全部符合的帖子都返回，打开会很慢，`limit` 下面就是返回帖子的数量

![](img/7cde81ff93bea33317f839c3cfeb8004.png)

#### 7、筛选帖子

帖子太多了，只保留特定条件的

![](img/3838e2ba9336e08a62c847d736429ee3.png)

这里筛选，参数都可以自己设置，

分别为，点赞>2，内容不为空，时间 365 天内

#### 8、选择保留的参数

也是参数太多了，只保留几个有用的：

点赞数、标题、正文、帖子链接、时间戳

![](img/37d624c8c41eaa5662b4550c9308baea.png)

![](img/2c43d27069bcf095edc03420bf85cc4f.png)

#### 9、AI 判断这个帖子是否是个需求

双击

![](img/2a4a6523a2d1fcdcb63a89a5edecbbe2.png)

![](img/17fce0f82f964d1a6be810811a42edd1.png)

这里的提示词填入：（这里用的圈友@四喜 的提示词）

```
decide whether this Reddit post is a business-related problem or a need for a solution or already a solution. The post should mention a specific challenge or requirement that a business is trying to address.​Reddit Post: ​{{ $json.selftext }}

##Output Format​ ￼
Return Boolean output​If the answer is affirmative, return Yes, if the output is negative, return No
只输出 yes 或者 no
```

填写自己的 deepseek apikey

![](img/27312d35aa4a39f8410b4e422e43adcc.png)

#### 10、整合

把 AI 返回的 yes/no，跟有用信息整合在一起

#### 11、过滤

只有 ai 判断为 `yes` 的帖子保留

![](img/712ef876a866df01e4e921fc27710de5.png)

#### 12、导出数据，写入到飞书多维表格

最后一个节点

![](img/4217f33cf49c2db154defa18c0a4376c.png)

这里填写的是【搜索结果】这个飞书子表格的 `tableid`，一定要认真核对

![](img/30690654417f22bc5a6ce73c4c6f1b7b.png)

![](img/e739bff845d97223de250f0412b60893.png)

![](img/a43023bbe108503fff69b5830dde705a.png)

`table=【】&view`，这一串就是【搜索结果】子表格 `tableid`，填进去

至此，n8n 工作流搭建完成

### 4、飞书多维表格部分

这部分就简单了，都是用的自带的字段捷径

#### 1、总结字段

主要是方便自己看的作用，我的提示词比较简单，直接用的飞书多维表格预设的总结，你也可以用 deepseek 字段，自己设置提示词

![](img/5c41b973d21e4d9b8ff00760026bb20c.png)

总结效果如下：

![](img/3fce48002122a2e293d45f6a9152700f.png)

#### 2、商业分析字段

![](img/25751cffd9c12f7a3d62a2f0d96e0b95.png)

![](img/f489ee9b0c1cbdc539c207921aad0cb5.png)

采用的是@饼干哥哥 的提示词：记得替换黄底字为实际引用



```
### Reddit搜索结果深度分析与商业洞察
#### 1. 角色扮演 (Role) &amp; 任务背景 (Context)
您将扮演一名专业的 “Reddit 商业机会分析师”。您的任务是基于给定的 Reddit 搜索数据，为用户提供一份精准、深刻且聚焦于商业价值的分析洞察。请以该专家的视角进行思考和表达。
#### 2. 任务目标 (Goal)
根据下方提供的 “搜索关键词” 和 “Reddit 搜索结果”，生成一份包含两大核心模块的分析报告：“主题内容与相关性” 和 “行业聚焦与细分机会”。报告必须结构清晰、逻辑严谨，并严格遵循指定的输出格式。
#### 3. 输入数据 (Input Data)
* 搜索关键词:
【引用多维表格里的字段】
* Reddit 搜索结果 (帖子标题与内容):
【引用多维表格里的字段】，【引用多维表格里的字段】
---
#### 4. 输出要求：严格遵循以下 Markdown 格式 (Output Format)
### Reddit 搜索结果商业价值分析报告
1. 主题内容与相关性
* 核心议题总结: (在此处以列表形式，总结帖子中反复出现的 1-3 个核心议题、问题或观点。)
    * 议题一: * 议题二: * ...
* 相关性评估: (在此处明确评估这些议题与“用户搜索意图”的关联程度，请选择：高 / 中 / 低 / 不相关，并简要解释你的判断理由。)
2. 行业聚焦与细分机会
*   2.1 潜在细分行业/领域:
    *   根据帖子内容，识别出该讨论最可能归属或关联的 1-2 个细分行业或特定市场领域。
    *   示例：
        *   细分行业1: [例如：独立游戏开发、SaaS客户支持工具、可持续时尚电商]
*   2.2 用户痛点洞察:
    *   从帖子中用户（发帖人或评论者）的讨论、抱怨、提问或分享的经验中，识别出 1-3 个明确的用户痛点或未被满足的需求。
    *   要求：痛点描述需具体，能反映用户的真实困境。
    *   示例：
        *   痛点1: [例如：缺乏针对小型企业的廉价项目管理工具]
        *   痛点2: [例如：现有AI写作助手生成的中文内容不够自然]
*   2.3 潜在商业机会挖掘:
    *   基于上述核心议题和用户痛点，思考并提出 1-2 个具有潜力的商业机会、产品概念或服务方向。
    *   要求：商业机会应具有一定的创新性和可行性，并说明其如何解决用户痛点或满足需求。
    *   示例：
        *   商业机会1: [例如：开发一款集成AI的、专为中文内容创作者设计的语法和风格优化插件，以解决AI写作不自然的问题。]
        *   商业机会2: [例如：推出一个专注于可持续材料的B2B时尚采购平台，连接设计师与环保面料供应商。]
---
#### 5. 关键指令与限制 (Critical Instructions &amp; Constraints)
* 聚焦商业: 分析的最终目的是挖掘商业价值，请确保“商业机会”部分具体、有启发性，而不仅仅是重复问题。
* 专注当前帖子： 你的所有分析都必须严格基于当前提供的“Reddit帖子标题”和“Reddit帖子内容”。不要引入外部知识或对其他帖子的假设，除非帖子内容明确引用，杜绝主观臆测。
* 客观中立： 保持分析的客观性和中立性，避免主观臆断。
* 深度优先： 相比宽泛的概述，更注重对单一帖子价值的深度挖掘。
* 精炼表达： 使用精炼的商业语言，直击要点。语言力求精准、专业、言简意赅。
* 信息不足处理： 如果帖子内容过于简短或信息不足，导致无法有效完成某项分析（如“商业机会挖掘”），请在该项下明确指出“信息不足，无法进行有效分析”，并简要说明原因。
* 严格遵守格式： 必须完全遵循上述Markdown输出格式，包括标题层级、编号和加粗等。
* 如果帖子内容为空或无关： 若“Reddit帖子内容”为空、乱码或与“帖子标题”、“搜索关键词”完全不相关（例如广告、spam），则在报告开头注明“当前帖子内容无效或不相关，无法进行分析”，然后终止后续分析。
```
![](img/771dac73445af152265ce669d3e473b0.png)

效果如下：

![](img/be4020331a18c2ac49139e2da098dec9.png)

## 3、deepseek 分析报告模块

有了上一步，再得益于`r1` 强大的代码能力，我让它直接生成 html 格式的报告源代码，提示词：

```
请根据以下分析报告，形成一个html报告，要求样式要好看，结构清晰突出，确保包含报告的所有内容，但分块要清晰，加上必要的图标等：
【引用多维表格里的前一个字段】
直接生成 html 源代码，只生成源代码！！不要额外的东西
```
![](img/be1bf8e80830789dab4989afea7f253e.png)

生成效果是网页源码

![](img/5a1b5b55c500e29b4eaf4fc858c686fa.png)

## 4、网页生成字段

我用的是多维表格自带的网页自动部署字段捷径

![](img/4bcd1e4dfc8b0365a096222f0c8b6d73.png)

![](img/68d032017aa08bdab589de796e6abd95.png)

生成结果直接部署好了，可以点开

![](img/a82c60d6b56053684639cf4841183956.png)

网页渲染效果还不错

![](img/7bdc141d376ec8c085fdb20f68f3ac1f.png)

## 5、产品开发文档字段

同样是 `deepseek-r1`，带联网功能

![](img/690df70b9800143fa09d00dda257838e.png)

输入指令提示词：



# 产品开发文档

我是一位独立开发者，请你严格参照下面格式，不要改变技术栈，把【引用deepseek分析报告字段】这个分析结果，直接输出`产品开发文档.md`文件，不用标明搜索到的信息来源

格式如下：

* **文档版本**: `1.0`
* **创建日期**: xxxx年xx月xx日
* **作者**: `[你的名字或团队名]`

---

## 1.0 产品愿景与使命 (Product Vision & Mission)

### 1.1 产品名 & Slogan (Product Name & Slogan)

* **产品名**: `[产品名]`
* **Slogan**: `[在此输入你的Slogan]`

### 1.2 愿景 (Vision)

[在此描述产品的长期愿景，即你希望在3-5年内，产品能为世界或特定领域带来什么样的终极改变和价值。]

### 1.3 使命 (Mission)

[在此描述产品的核心使命，即为了实现上述愿景，你的产品当下需要做什么。它应该清晰地阐述你的产品为谁（目标用户）提供什么（核心价值）以及如何提供（核心方法）。]

---

## 2.0 目标用户画像 (Persona)

* **姓名**: [虚拟姓名]
* **身份**: [用户身份/职业，例如：大学生、产品经理、小企业主等]
* **目标**:
    * [描述该用户的首要目标]
    * [描述该用户的次要目标]
    * [其他相关目标]
* **痛点与挫败感 (Pain Points & Frustrations)**:
    * [详细描述用户在实现其目标时遇到的具体困难、烦恼和挑战。最好能以一个简短的故事来具象化这些痛点。]
    * [列出用户当前使用的替代方案及其不足之处。]
    * [描述这些痛点给用户带来的情绪或实际损失。]
* **我们的产品如何帮助 [用户姓名]**:
    * [针对上述的每一个痛点，清晰地阐述你的产品将如何提供一个更好的解决方案。描述用户使用你的产品后的理想状态。]

---

## 3.0 核心功能与商业模型 (Core Features & Business Model)

### 3.1 核心体验 (Core Experience for All Users)

1. **[核心步骤一]**: [描述用户使用产品的第一个关键步骤]
2. **[核心步骤二]**: [描述用户使用产品的第二个关键步骤]
3. **[核心步骤三]**: [描述用户获得核心价值（Aha! Moment）的关键步骤]
4. **[核心步骤四]**: [其他关键交互或功能点]

### 3.2 订阅/商业模型 (Subscription/Business Model)

| 功能模块 | 免费版 (Free) / 基础版 | 专业版 (Pro) / 付费版 |
| :--- | :--- | :--- |
| **[功能模块 A]** | [描述免费版的限制或特性] | [描述付费版的增强功能] |
| **[功能模块 B]** | [描述免费版的限制或特性] | [描述付费版的增强功能] |
| **[功能模块 C]** | [描述免费版的限制或特性] | [描述付费版的增强功能] |
| **[功能模块 D]** | ❌ 不支持 | ✅ 支持 |
| **使用场景** | **[免费版的核心使用场景]**: [简述用户在什么情境下会使用免费版，能解决什么初步问题。] | **[付费版的核心使用场景]**: [简述专业用户或深度用户会如何使用付费功能来解决更复杂、更重要的问题，获得更大的价值。] |

---

## 4.0 技术规格 (Technical Specification)

| 模块 (Component) | 技术选型 (Technology) | 实现细节与阐述 |
| :--- | :--- | :--- |
| **前端 (Frontend)** | Next.js (React) on Vercel | - **架构与渲染**: 采用 Next.js 的 `App Router` 模型。营销页面（如首页、定价页）将使用静态站点生成（SSG）或服务器端渲染（SSR）以获得最佳的 SEO 效果和加载性能。用户登录后的核心应用仪表盘将采用客户端渲染（CSR），以提供丰富的动态交互体验。<br>- **状态管理**: 选用轻量级状态管理库 `Zustand` 或 React 原生的 `Context API`。这避免了 `Redux` 等工具的复杂性，非常适合独立开发者快速迭代。全局状态将包括用户信息、订阅等级和API使用次数等。<br>- **界面与样式**: 使用 `Tailwind CSS` 进行原子化、功能优先的样式设计，所有样式直接在 `JSX` 中完成，无需在多个文件间切换。结合 `Shadcn/ui` 提供的无头（unstyled）、易于访问的组件库，我们可以快速构建既美观又符合品牌风格的 UI，同时保证了组件的可访问性（a11y）。<br>- **数据交互**: 使用 `SWR` 或 `TanStack Query` 等现代数据请求库，优雅地处理与后端 API 的通信。它们能自动管理缓存、请求状态（加载中/成功/失败）和数据刷新，极大地简化了前端逻辑并提升了用户体验。 |
| **后端 (Backend)** | Python Serverless Functions on Vercel | - **核心架构**: 我们的整个后端将构建在无服务器架构之上。核心业务逻辑将封装在一个名为 `/api/generateReport` 的 Python Serverless 函数中。这个函数是一个 HTTP `POST` 端点，它接收前端传来的关键词，并负责协调后续所有的数据抓取和 AI 分析任务。<br>- **优势**: 无服务器架构意味着我们无需管理和维护服务器，Vercel 会根据请求量自动扩缩容。这使得运营成本极低（按需付费，没有请求就没有费用），同时能轻松应对流量高峰，是独立开发者项目的理想选择。<br>- **安全性**: 所有的敏感信息，如第三方服务密钥、`DeepSeek API 密钥`、`Supabase` 数据库密钥等，都将作为环境变量存储在 Vercel 平台，绝不会硬编码在代码中。 |
| **数据源接口** | [例如: 第三方API, 网络爬虫] | - **认证与初始化**: [描述如何认证并初始化客户端来访问数据源，例如使用`API Key`, `OAuth`等]。<br>- **数据获取策略**: [描述如何根据用户输入来查询和获取相关数据。说明关键的查询参数，如排序、过滤、时间范围和数量限制，以有效控制成本和性能]。<br>- **健壮性**: [描述如何设计错误处理机制（如`try-except`）、请求重试和速率限制，以确保数据获取的稳定性并遵守服务提供方的规则]。 |
| **AI 核心** | DeepSeek LLM API | - **客户端与模型**: 使用官方的 `deepseek` Python 库，通过环境变量中的 `DEEPSEEK_API_KEY` 初始化客户端。我们将选用其兼具性能和成本优势的 `deepseek-chat` 模型作为分析引擎。<br>- **高效的 Prompt Engineering**: 这是决定 AI 分析质量的关键。我们将设计一个结构化的 Prompt 模板，明确指示 LLM 完成任务：`<task>Your role is a market research analyst. Analyze the following user-generated text from an online platform.</task><instruction>1. Classify the user's primary intent into one of three categories: [Pain Point], [Product Idea], or [Other].</instruction><instruction>2. If the intent is [Pain Point] or [Product Idea], write a concise one-sentence summary of the core business opportunity. </instruction><instruction>3. Output the result ONLY in a valid JSON format like this: {"intent": "...", "summary": "..."}</instruction><comment>{source_text}</comment>`<br>- **性能优化与成本控制**: 为了给用户提供快速的响应，我们将利用 Python 的 `asyncio` 库，将多个对 DeepSeek API 的调用并发执行。同时，对每次报告分析的文本条数设置硬性上限，以确保单次操作的成本是可预测且可控的。 |
| **数据库 & 认证** | Supabase | - 使用 `PostgreSQL` 数据库存储 `users` (包含订阅状态和 API 剩余次数)、`saved_opportunities` 等核心数据。<br>- 利用其开箱即用的 Auth 服务，快速、安全地实现用户的注册、登录和会话管理功能，无需手动开发。<br>- 通过官方的 `supabase-py` 库，在后端 Serverless 函数中与数据库进行交互，例如在用户成功支付后更新其订阅状态。 |
| **支付网关** | creem | - **集成方式**: 我们将在前端集成 `creem` 的支付覆盖层(Overlay)。<br>- **订阅逻辑**: 用户点击“升级”后，前端会引导其至支付页面。支付成功后，`creem` 会通过 Webhook 向我们后端的一个特定端点 `/api/payment-webhook` 发送通知。我们的后端函数在接收到这个通知并验证其合法性后，会更新该用户在 Supabase 数据库中的订阅状态为 `pro`。 |

---

## 5.0 用户流程 (User Flow)

1. **[流程节点一：例如，注册/登录]**: [描述用户完成该节点的核心操作和体验。]
2. **[流程节点二：例如，首次使用/引导]**: [描述新用户如何被引导并体验到产品的“Aha! Moment”。]
3. **[流程节点三：例如，核心功能使用]**: [描述用户如何与产品的核心功能进行交互，并获得价值。]
4. **[流程节点四：例如，行动转化]**: [描述用户在获得价值后，你期望他们采取的下一步行动，例如保存、分享、导出等。]
5. **[流程节点五：例如，付费转化/升级]**: [描述免费用户在何种契机下，会被引导升级至付费版，以及这个转化流程是怎样的。]

---

## 6.0 成功指标 (Success Metrics / KPIs)

* **用户增长与激活 (Growth & Activation)**:
    * **[指标一，如：激活率]**: [具体定义该指标，例如：新用户在注册后7天内完成[核心动作]的比例。]
* **商业价值 (Monetization)**:
    * **[指标二，如：转化率]**: [具体定义该指标，例如：免费用户升级为付费用户的比例。]
* **用户粘性与留存 (Engagement & Retention)**:
    * **[指标三，如：周活跃用户 WAU]**: [具体定义该指标，例如：每周至少登录并使用一次核心功能的用户数。]
    * **[指标四，如：用户留存率]**: [具体定义该指标，例如：用户在注册后第1、7、30天后仍在使用的比例。]
* **技术健康度 (Technical Health)**:
    * **[指标五，如：服务可用性]**: [具体定义该指标，例如：核心API或功能的成功率，目标 > 99.9%。]

---

## 7.0 风险与应对 (Risks & Mitigations)

* **风险一：[描述一个潜在的产品/市场风险]**
    * **应对策略**: [阐述如何预防、监控或降低该风险。可以包括备用方案、产品设计上的考量等。]
* **风险二：[描述一个潜在的技术/成本风险]**
    * **应对策略**: [阐述如何从技术架构、成本控制、第三方服务选择等方面来应对。]
* **风险三：[描述一个潜在的运营/法律风险]**
    * **应对策略**: [阐述在运营策略、用户协议、合规性等方面的应对措施。]



* **文档版本**: 1.0
* **创建日期**: 2025年06月14日
* **作者**: Thunder Dev Team

---

## 1.0 产品愿景与使命 (Product Vision & Mission)

### 1.1 产品名 & Slogan (Product Name & Slogan)
* **产品名**: **MailFlow Engine**
* **Slogan**: **「重新定义SaaS邮件系统的一体化解决方案」**

### 1.2 愿景 (Vision)
未来3-5年内，成为全球SaaS服务商首选的嵌入式邮件管理基础设施，覆盖80%以上的企业级交互场景，消除邮件服务碎片化带来的技术风险和运营损耗。

### 1.3 使命 (Mission)
为SaaS开发者和企业服务提供商提供**安全、智能、合规**的邮件全生命周期管理解决方案，通过预置场景模板、动态风险拦截和自动诊断工具，减少50%的邮件系统开发周期与30%的运维成本。

---

## 2.0 目标用户画像 (Persona)
* **姓名**: 张航
* **身份**: 中型SaaS企业的技术负责人
* **目标**:
    * 需在3周内完成邮件通知系统的技术选型
    * 解决跨平台邮件服务的可维护性问题
    * 规避SMTP自配风险导致的客户投诉
* **痛点与挫败感 (Pain Points & Frustrations)**:
    * 某次促销活动中因自建SMTP服务器被标记为垃圾邮件源，导致关键交易邮件到达率暴跌60%
    * 当前使用SendGrid + 自开发监控系统，需在3个平台间切换配置，每周消耗团队8+小时维护
    * 缺乏全局状态监控，曾因邮件队列阻塞导致客户续费提醒延迟24小时
* **我们的产品如何帮助 张航**:
    1. **嵌入式监控面板**实时展示邮件链路状态，异常即时告警
    2. **预置20+场景模板**覆盖激活、续费、风控等业务场景，调试时间缩短75%
    3. **智能风险评分系统**自动优化邮件策略，确保送达率稳定高于98%

---

## 3.0 核心功能与商业模型 (Core Features & Business Model)

### 3.1 核心体验 (Core Experience for All Users)
1. **场景模板选择**: 通过业务类型（营销/交易/通知）智能推荐配置方案
2. **风险自检向导**: 输入域名和IP自动生成反垃圾邮件规避建议
3. **全链路追踪看板**: 可视化展示从触发到投递的全流程状态（Aha! Moment）
4. **合规性审计**: 自动生成GDPR等合规要求的操作日志

### 3.2 订阅/商业模型 (Subscription/Business Model)
| 功能模块 | 免费版 (Starter) | 专业版 (Pro) |
| :--------------- | :-------------- | :----------- |
| **SMTP代理服务** | 每日500封 | 无上限+动态IP池 |
| **合规校验** | 基础内容扫描 | GDPR/HIPAA深度检查 |
| **模板库** | 5个基础模板 | 20+场景模板 |
| **监控告警** | ❌ 不支持 | ✅ 实时告警+自动修复 |
| **使用场景** | **测试验证场景**: 开发阶段的邮件功能验证 | **生产环境场景**: 高并发交易场景的稳定性保障 |

---

## 4.0 技术规格 (Technical Specification)
| 模块 | 技术选型 | 实现细节与阐述 |
| :------------------ | :--------------------- | :------------ |
| **前端** | Next.js (React) | 仪表盘采用增量静态生成(ISR)，核心指标实时更新频率≤500ms |
| **邮件网关** | Rust + Tokio | 基于异步运行时实现10万级/秒的邮件队列处理能力 |
| **风险引擎** | 随机森林模型 | 动态分析200+垃圾邮件特征，置信度阈值动态调节 |
| **合规审计** | OpenPolicy Agent | 声明式策略引擎实时校验邮件内容合规性 |
| **日志系统** | Loki + Grafana | 支持全文检索的日志链路追踪，保留周期30天（Pro版90天） |

---

## 5.0 用户流程 (User Flow)
1. **快速接入**: OAuth2.0一键绑定现有邮件服务商账户
2. **智能引导**: 根据NPS问卷推荐初始化配置方案
3. **看板交互**: 可视化调整发送策略，实时观察预估送达率变化
4. **价值升级**: 免费用户触发风险阈值时引导开通动态IP池服务
5. **企业级扩展**: 账户用量达80%时触发企业版解决方案咨询

---

## 6.0 成功指标 (Success Metrics / KPIs)
* **激活率**: 注册后24小时内完成邮件测试发送的比例 ≥65%
* **付费转化率**: 免费用户90天内升级比例 ≥12%
* **周活跃率**: Pro版用户每周操作核心功能的次数 ≥8次
* **送达可用性**: 核心API请求成功率 SLI ≥99.95%

---

## 7.0 风险与应对 (Risks & Mitigations)
* **风险一：邮件服务商API变更导致集成中断**
    * **应对策略**: 抽象统一接口层+接口变更监听器，72小时内发布适配更新
* **风险二：AI风控模型的误判导致正常邮件被拦截**
    * **应对策略**: 实施双轨验证机制，人工审核队列兜底，误判率<0.1%
* **风险三：海外隐私法案（如CCPA）的合规性挑战**
    * **应对策略**: 基于OpenTelemetry的可审计日志架构，支持按需启用区域化数据隔离

有了这个文档，就可以直接放到 cursor、v0.dev、claude code 里面进行开发了。

# 参考资料：

鸣谢各位前辈，本文是在整合这些工作流基础上添加进了飞书多维表格利用自带 ai（deepseek）来处理数据的逻辑，仅供学习参考，不作商业用途。

https://mp.weixin.qq.com/s/F5DGk7zZfSM1sk6hAimgvg

https://mp.weixin.qq.com/s/4W090kWyDmCTBnOC9yPwiA

https://mp.weixin.qq.com/s/oh9FneTxZccxIWnCx5Snew

https://github.com/yiGmMk/ai/blob/24297337c919a358036000a919b34c1f93c11c2e/content/zh-cn/post/ai-n8n-feishu.md

[如何免费自动化寻找蓝海市场：用AI Agent+N8N工作流在Reddit上发现真实痛点](https://eafz7mo3of.feishu.cn/docx/Y2kkdmdPFolDJlxkLlKcY4Sunjd)

[用于挖掘Reddi 需求的 AI Agent](https://zcnzawlx2ndr.feishu.cn/docx/AUxIdF9X0oYRQ4xvjoUcm0llnHd?PdWaR8=1)

https://www.feishu.cn/hc/zh-CN/articles/653027931590-%E4%BD%BF%E7%94%A8%E5%A4%9A%E7%BB%B4%E8%A1%A8%E5%8F%8C%E5%90%91%E5%85%B3%E8%81%94%E5%AD%97%E6%AE%B5
