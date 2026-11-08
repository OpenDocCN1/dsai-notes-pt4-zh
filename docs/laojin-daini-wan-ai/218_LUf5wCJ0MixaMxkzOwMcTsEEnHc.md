# 火爆全网的Skill自己怎么做？老金来教你！（含避坑指南）

> 来源：[https://my.feishu.cn/docx/Awx6doq4WoRkgmxySGqc2dM9nkh](https://my.feishu.cn/docx/Awx6doq4WoRkgmxySGqc2dM9nkh)

老金我一直想写一下官方的Skill说明书，它叫anthropics/skills。

记得老金我第一次照着官方文档做技能，打包直接失败。

老金我查了错误日志，发现官方文档写得"云淡风轻"，实际"坑得一塌糊涂"。

老金我花了一晚上踩完所有坑，终于把整套流程跑通了。

今天这篇文章，老金我会把这个6步流程拆解给你看。

**读完你会明白：**

为什么A社要设计三层架构。

为什么description是技能的生死线。

为什么scripts必须测试，references要精简。

6步完整流程到底怎么走。

---

## skill-creator是啥

先说清楚，skill-creator不是一个独立的工具或软件，而是Claude Code官方的技能创建指南文档。

它位于GitHub仓库：anthropics/skills

位置：skills/skill-creator/SKILL.md

GitHub地址：https://github.com/anthropics/skills

它教你怎么做一个"Skill"——也就是给Claude Code装一个"专业插件"。

比如你想让Claude擅长写Python代码，或者擅长做数据分析，或者擅长写👸号文章，这些都可以做成Skill。

以前老金我以为Skills很复杂，要懂很多技术才能做。

看完这个文档，老金我发现：只要跟着6步走，零基础也能做。

但老金我要说清楚：这个指南有点技术门槛，需要花时间理解。

---

## 三层架构核心机制（Progressive Disclosure）

A社在skill-creator文档里提出了一个核心概念。

Progressive Disclosure（渐进式展示）

这是Skills系统的核心架构设计。

老金我翻译一下：不是把所有信息一次性塞给Claude，而是分三层加载，根据需要逐步展示。

**三层加载架构**：

第一层：Metadata（元数据）

内容：name + description

加载时机：始终在上下文中

Token配额：约100词

作用：决定技能何时触发

第二层：SKILL.md body（技能主体）

内容：具体指令、使用指南、注意事项

加载时机：技能触发后

Token配额：<5000词（建议<500行）

作用：核心工作流程

第三层：Bundled resources（捆绑资源）

内容：scripts/、references/、assets/

加载时机：按需加载

Token配额：无限（scripts可执行而不读入上下文）

作用：详细参考和可执行代码

**老金的洞察**：

从这三层架构可以清晰看出官方的设计哲学：

**1. 元数据决定生死**

只有100词的配额，但这100词决定了技能何时被Claude选中。

description必须精炼、准确、全面。

**2. 主体要精简**

建议<5000词（<500行），超过限制要拆分到references/。

避免context bloat（上下文膨胀）。

**3. 资源按需加载**

scripts可执行但不读入上下文（token高效）。

references仅在需要时加载。

assets用于输出，不占用上下文。

**核心公式**：

```
总Token消耗 = 100词（metadata） + 5000词（body） + 按需资源（scripts/references/assets）
```

**优化建议**：

基于这个公式，老金我总结出优化Skills的三个原则：

1. Metadata层：精准触发

description必须包含：技能功能 + 使用场景 + 具体触发条件。

避免模糊描述，用数字编号列出场景。

**2. Body层：核心流程**

只保留核心工作流程和选择指导。

变体细节、示例、配置移到references/。

保持<500行。

3. Resources层：按需拆分

scripts：重复执行的代码（token高效）。

references：文档、API说明（按需加载）。

assets：模板、图标（输出用）。

---

## Skills的文件结构（三层架构版）

Anthropic在文档里明确规定了一个Skill的标准结构：

```
skill-name/
├── SKILL.md (必需)
│   ├── YAML frontmatter (必需)
│   │   ├── name: (必需)
│   │   └── description: (必需)
│   └── Markdown instructions (必需)
└── Bundled Resources (可选)
    ├── scripts/          - 可执行代码
    ├── references/       - 参考文档
    └── assets/           - 资源文件
```

老金我用大白话翻译一下：

**SKILL.md（必需）**：

Frontmatter（YAML元数据）：name + description

name：技能名称（小写，连字符分隔）

description：详细说明（最重要的触发机制）

Body（Markdown指令）：使用指南、注意事项、代码示例

**scripts/（可选）**：

用途：存放可执行代码（Python/Bash等）

何时包含：相同代码重复重写，或需要确定性可靠性

例子：rotate_pdf.py（PDF旋转脚本）

优势：token高效，确定性，可执行而不读入上下文

注意：Claude可能仍需读取以进行修补或环境调整

**references/（可选）**：

用途：文档和参考材料（按需加载到上下文）

何时包含：Claude应该引用的文档

例子：finance.md（财务模式）、mnda.md（NDA模板）、policies.md（公司政策）

优势：保持SKILL.md精简，仅在需要时加载

最佳实践：如果文件>1万词，在SKILL.md中包含grep搜索模式

避免重复：信息应该在SKILL.md或references/中，不要两者都有

**assets/（可选）**：

用途：输出中使用的文件（不读入上下文），何时包含：技能需要用于最终输出的文件。

例子：logo.png（品牌资产）、slides.pptx（PPT模板）、frontend-template/（HTML/React样板）。

优势：分离输出资源和文档，让Claude使用文件而不加载到上下文。

**不应该包含的内容**：

A社明确指出，技能不应该包含额外的文档或辅助文件。

包括：README.md、INSTALLATION_GUIDE.md、QUICK_REFERENCE.md、CHANGELOG.md等。

技能应该只包含AI代理完成工作所需的信息，不应该包含创建过程的辅助上下文、设置和测试程序、面向用户的文档等。

创建额外的文档文件只会增加混乱和困惑。

---

## 6步完整流程（老金实测+时间成本）

A社把技能制作拆成了6步，老金我用大白话翻译一下，并加入实测时间成本。

第一步：理解技能（Understanding the Skill）

**时间成本**：30分钟-1小时（取决于复杂度）

先想清楚你要做什么技能。

建议你先用具体的例子来测试，比如"我想让Claude帮我做PDF处理"。

**然后问自己几个问题：**

这个技能要支持什么功能（编辑PDF、转换格式、提取文字）

用户会怎么说（"帮我旋转这个PDF"、"把PDF转成Word"）

什么情况下会触发这个技能

老金我觉得这一步很重要，想不清楚后面全白搭。

**验证标准**：

有3-5个具体使用场景，每个场景有明确的用户说法，能清楚描述技能的边界（什么不做）。

第二步：规划内容（Planning the Contents）

**时间成本**：30分钟-1小时

想清楚技能需要什么资源：

需要哪些脚本（比如PDF处理的代码）

需要哪些参考资料（比如PDF格式说明文档）

需要哪些素材（比如Word模板）

不要什么都加，只加必要的。

老金我特别认同这一点，加太多反而会让Claude困惑。

**规划清单**：

scripts列表：每个脚本的目的、输入输出、测试方法

references列表：每个文档的用途、大小、搜索模式

assets列表：每个资源的使用场景、格式要求

第三步：初始化技能（Initializing the Skill）

**时间成本**：5分钟（自动化）

A社提供了一个脚本：init_skill.py。运行一行命令，它会自动创建Skill的目录结构：

```
python scripts/init_skill.py my-skill --path ./skills
```

老金我试过，这个脚本会生成：

my-skill/SKILL.md（模板文件，带TODO占位符）

my-skill/scripts/（示例脚本）

my-skill/references/（示例参考）

my-skill/assets/（示例素材）

你只需要在这个基础上修改就行。

**跳过条件**：如果技能已存在，只需要迭代或打包，可以跳过这一步。

第四步：编辑技能（Editing the Skill）

**时间成本**：2-4小时（最耗时）

这一步是最花时间的，老金我花了大概3小时。

你要做3件事：

**1. 准备可重用资源（script**s/references/assets）

**scripts/**：

编写代码并实际运行测试，确保无bug。如果有很多类似脚本，只需测试代表性样本。删除不需要的示例文件。

**references/**：

存放文档、API说明、领域知识。如果文件>100行，在顶部添加目录。避免与SKILL.md重复。

**assets/**：

准备模板、图标、样板代码。确保格式正确（PPT、Word、图片等）。

**2. 编写SKILL.md**

Frontmatter部分（YAML元数据）：

```
---
name: pdf-editor
description: 综合PDF编辑、转换和文本提取技能。当Claude需要处理PDF文件时使用：(1)编辑内容，(2)转换格式，(3)提取文本，或其他PDF任务
---
```

**重要说明**：

name：技能名称（小写，连字符分隔）

description：这是技能的主要触发机制，必须包含：技能功能 + 何时使用 + 具体触发条件

用数字编号列出场景，不要把"何时使用"放在body里（body只在触发后加载）

A社特别强调：description一定要写清楚，因为Claude是根据这个来决定什么时候加载技能的。

Body部分（Markdown指令）：

使用命令式/不定式形式（Use the pdf-editor skill to...，而不是You should use...），写具体的使用指南、注意事项、代码示例等。

**写作规范**：

使用命令式动词（Rotate the PDF, Extract text, etc.）

保持简洁（<500行）

避免冗余解释

提供具体示例而非泛泛而谈

**3. 删除不需要的示例文件**

初始化脚本创建了示例文件，但大多数技能不需要所有这些文件。

**验证清单**：

scripts：已测试，无bug，输出符合预期

references：无重复，与SKILL.md互补，大文件有grep模式

assets：格式正确，可用于输出

SKILL.md：<500行，description详细，body使用命令式

第五步：打包技能（Packaging the Skill）

**时间成本**：5分钟（自动化）

全部搞定后，用package_skill.py打包：

```
python scripts/package_skill.py ./skills/my-skill
```

可选输出目录：

```
python scripts/package_skill.py ./skills/my-skill ./dist
```

这会生成一个.my-skill文件（实际是.zip文件），可以分享给别人用。

**自动验证流程**：

打包脚本会自动验证技能，检查：

YAML frontmatter格式和必需字段

Skill命名约定和目录结构

Description完整性和质量

文件组织和资源引用

如果验证失败，脚本会报告错误并退出，不创建包。老金我第一次打包就失败了，因为description写得太简单，不符合标准。

**验证标准**（老金总结）：

**Frontmatter验证**：

name字段存在且格式正确（小写，连字符）

description字段存在且完整

无额外字段（只允许name和description）

**Description质量**：

包含技能功能说明

包含何时使用的指导

用数字编号列出具体场景

长度适中（通常50-200词）

**文件组织**：

SKILL.md存在且在根目录

scripts/references/assets目录结构正确

无README.md等辅助文件

**资源引用**：

SKILL.md中引用的文件实际存在

references/大文件包含grep搜索模式

第六步：迭代（Iterating）

**时间成本**：持续进行

根据实际使用反馈优化技能。

**迭代工作流**：

在真实任务中使用技能，注意困难或低效的地方，识别SKILL.md或捆绑资源应该如何更新，实施更改并再次测试。

**老金实测**：

老金我测试了10次，发现3个问题，然后回去修改SKILL.md。

**测试清单**：

技能是否正确触发（description是否准确）

技能是否正确引用scripts/references

输出是否符合预期

有无错误或幻觉

Token使用是否高效

---

## 官方文档 vs 老金实测（差距有多大）

老金我按着官方文档走了一遍6步流程，发现了一个问题。

官方文档写得"云淡风轻"，实际"坑得一塌糊涂"。

差距1：description字数

官方文档说：创建metadata.json，写name和description就行。

老金我照做了，写了10个字的description，结果打包直接失败。

老金我查了错误日志，发现description少于50词直接打包失败。

官方文档为什么不提"50词"这个最低要求？

**差距2：scripts权限**

官方文档说：scripts/可选，有就放。

老金我放了Python脚本进去，打包成功了，结果使用时报错，说脚本没有执行权限。

老金我回去查文档，文档里只字未提"权限"这个关键细节。

**差距3：SKILL.md长度**

官方文档说：SKILL.md核心文档，<5000词。

老金我写了一个800词的SKILL.md，覆盖全部流程。

老金我就在想：

官方为什么要限制5000词？

是为了保证Claude的上下文效率？

还是为了强制创作者"精简表达"？

或者说，Anthropic设计这个系统时，本身就假设"好的Skill不需要长篇大论"？

**老金我的统计**：

官方12个Skills，老金我逐个检查了：

+   格式错误：8个（66.7%）

+   脚本缺失：7个（58.3%）

+   文档不清：5个（41.7%）

这说明什么？

说明连官方自己的Skills，都没有完全按照官方文档的标准来做。

或者说，官方文档是"理想状态"，实际使用是"另一回事"。

**老金我的建议**：

不要100%照搬官方文档。

要结合实际，灵活调整。

---

## 老金踩过的坑（老金实测）

老金我用这个流程做了个👸号写作技能，踩过3个坑。

第一个坑：description写得不清楚

严重程度：5星（打包直接失败）

时间损失：返工30分钟

老金原版："👸号写作技能"

失败原因：description太简单，不符合标准

**修复版**：

```
description: 专业公众号写作技能，基于82篇爆款文章数据和三层架构系统。支持标题生成、内容写作、质量检测。使用场景：(1)写公众号文章，(2)生成爆款标题，(3)文章质量评分
```

**A社要求**：技能是干嘛的、什么时候用、具体使用场景（用数字编号）

**第二个坑：scripts没测试**

严重程度：3星（使用时才发现）

时间损失：返工1小时

老金错误：写了个Python脚本，直接打包了

失败原因：脚本有语法错误，第一次用时报错

A社建议：所有scripts必须先测试过，确保能运行

修复方法：编写后立即运行测试，验证输出符合预期

第三个坑：references太长

严重程度：2星（性能问题）

时间损失：返工30分钟

老金错误：放了10多个文档在references/，加载特别慢。

失败原因：references太多，导致context膨胀。

Anthropic建议：references只放必要的，如果>1万词，要在SKILL.md写清楚grep搜索模式。

修复方法：精简到3个核心文档，为大文件添加grep模式。

**老金避坑公式**：

```
坑点风险 = description不清楚(5星) + scripts未测试(3星) + references过长(2星)
```

**避坑优先级**：

+   description（最高优先级）：直接影响打包成功

+   scripts测试（高优先级）：影响使用体验

+   references精简（中优先级）：影响性能

---

## 新手版 vs 专业版对比

老金我总结了一个对比表，让你一眼看出差异：

**description**：

+   新手版：简短模糊

+   专业版：详细精确，含场景编号

**SKILL.md长度**：

+   新手版：>500行，冗长

+   专业版：<500行，精简

**文件组织**：

+   新手版：有README.md等辅助文件

+   专业版：只有SKILL.md + 必要资源

**scripts**：

+   新手版：未测试，有bug

+   专业版：已测试，无bug

**references/**：

+   新手版：与SKILL.md重复，无grep模式

+   专业版：与SKILL.md互补，大文件有grep

**assets/**：

+   新手版：混在references/

+   专业版：独立目录，输出用

**打包验证**：

+   新手版：一次不过

+   专业版：一次通过

**Token效率**：

+   新手版：低（重复信息）

+   专业版：高（按需加载）

**核心差异**：

新手版：能跑就行，不考虑效率

专业版：符合A社标准，Token高效，可维护

---

## 老金的实战建议

基于A社官方文档和老金我自己的经验，总结出以下实战建议。

建议1：description是生死线

老金我再次强调：description是技能的主要触发机制，必须精炼、准确、全面。

**description公式**：

```
description = 技能功能 + 使用场景 + 具体触发条件（数字编号）
```

**示例**：

```
# 错误示范
description: PDF编辑技能

# 正确示范
description: 综合PDF编辑、转换和文本提取技能。当Claude需要处理PDF文件时使用：(1)编辑内容，(2)转换格式，(3)提取文本，或其他PDF任务
```

**验证标准**：

+   包含技能功能说明（Comprehensive PDF editing...）

+   包含何时使用的指导（Use when Claude needs...）

+   用数字编号列出场景（(1) editing, (2) converting, (3) extracting）

+   长度适中（50-200词）

建议2：保持SKILL.md精简

SKILL.md长度建议：<500行，<5000词

老金我发现，很多新手容易把所有信息都塞进SKILL.md，导致context膨胀。

**精简公式**：

```
SKILL.md内容 = 核心工作流程 + 选择指导 + 资源引用
```

**分层原则**：

核心流程：保留在SKILL.md

变体细节：移到references/

示例代码：移到references/examples.md

配置选项：移到references/config.md

Pattern 1：高层级指导 + references

```
# PDF文档处理

## 快速开始

使用pdfplumber提取文本：
[代码示例]

## 高级功能

- **表单填充**：参见[FORMS.md](FORMS.md)查看完整指南
- **API参考**：参见[REFERENCE.md](REFERENCE.md)查看所有方法
- **示例**：参见[EXAMPLES.md](EXAMPLES.md)查看常见模式
```

Pattern 2：领域特定组织

```
bigquery-skill/
├── SKILL.md (概览和导航)
└── references/
    ├── finance.md (收入、计费指标)
    ├── sales.md (销售机会、管道)
    ├── product.md (API使用、功能)
    └── marketing.md (营销活动、归因)
```

当用户问销售指标时，Claude只读取sales.md。

**Pattern 3：条件性细节**

```
# DOCX文档处理

## 创建文档

使用docx-js创建新文档。参见[DOCX-JS.md](DOCX-JS.md)。

## 编辑文档

简单编辑可以直接修改XML。
**追踪修订**：参见[REDLINING.md](REDLINING.md)
**OOXML详细信息**：参见[OOXML.md](OOXML.md)
```

**建议3：Token优化三原则**

老金我总结出Token优化的三个原则：

原则1：Context是公共资源

Anthropic原话："The context window is a public good."

Skills与system prompt、conversation history、other Skills' metadata、user request共享context window。

**优化公式**：

```
Token总预算 = 200K (Claude 3.5) - system prompt - conversation history - other skills metadata - user request
```

原则2：默认Claude已经很聪明

A社建议："Only add context Claude doesn't already have."

对每条信息挑战："Claude真的需要这个解释吗？"和"这个段落是否值得它的token成本？"

**优先简洁示例而非冗长解释。**

**原则3：匹配自由度级别**

根据任务的脆弱性和可变性匹配 specificity 级别：

**高自由度**（文本指令）：

多种方法有效时

决策依赖上下文时

启发式指导方法时

**中自由度**（伪代码或带参数脚本）：

存在首选模式时

一些变化可接受时

配置影响行为时

**低自由度**（特定脚本，少参数）：

操作脆弱且易错时

一致性关键时

必须遵循特定序列时

**老金的自由度选择表**：

**创意写作**：

+   自由度：高

+   示例："写一篇关于..."

**代码重构**：

+   自由度：中

+   示例：提供伪代码框架

**PDF旋转**：

+   自由度：低

+   示例：特定脚本，少参数

**建议4：scripts测试标准**

老金我发现，scripts的bug是新手最容易犯的错误。

**测试标准**：

基本测试：运行脚本，确保无语法错误

输出验证：输出符合预期格式

边界测试：测试边界情况（空文件、大文件等）

样本测试：多个相似脚本，测试代表性样本

**测试公式**：

```
scripts可信度 = (基本测试通过 + 输出验证通过 + 边界测试通过) / 3
```

建议5：references精简策略

老金我发现，references膨胀是性能杀手。

**精简策略**：

删除重复：确保信息只在SKILL.md或references/中，不重复

大文件拆分：>100行的文件，拆分成多个小文件

添加grep模式：>1万词的文件，在SKILL.md写清楚grep搜索模式

按需加载：只在需要时引用特定文件

**精简公式**：

```
references效率 = (必要文件数 / 总文件数) × (小文件数 / 总文件数)
```

目标：接近100%的文件都是必要的且小的。

---

## 老金的最终评价

A社这个官方文档，老金我读完最大的感受是：严谨和专业。

它不是教你"快速做一个Skill"，而是教你"做一个符合标准、Token高效、可维护的Skill"。

**优点**：

1.  权威性：Anthropic官方出品

1.  系统性：6步完整流程，从理解到迭代

1.  自动化：init_skill.py和package_skill.py脚本

1.  验证严格：打包时自动验证，不符合标准的不通过

1.  Token高效：Progressive Disclosure架构，按需加载

**局限性**：

1.  技术门槛：需要理解YAML、Markdown、命令行

1.  时间成本：初次制作需要4-8小时（老金实测）

1.  迭代成本：需要持续测试和优化

**适用场景**：

老金我强烈建议在以下情况使用这个6步流程：

团队共享的专业技能

长期使用的生产力工具

分享给别人的完整技能

需要高Token效率的复杂技能

**不适用场景**：

个人简单使用（可能用不着这么复杂）

临时性任务（投入产出比不高）

快速原型（可以先用简化版）

---

## 核心总结

**Skills制作核心公式**：

```
高质量Skill = (精准description + 精简SKILL.md + 测试scripts + 优化references) × 持续迭代
```

**老金建议的优先级**：

description（最高优先级，直接影响触发）

SKILL.md精简（高优先级，影响Token效率）

scripts测试（高优先级，影响使用体验）

references优化（中优先级，影响性能）

持续迭代（持续进行，提升质量）

**老金建议**：

如果你是团队使用或需要专业技能，老金我强烈建议按照这个6步流程制作Skill。

虽然初次需要4-8小时，但一旦掌握，以后做任何技能都得心应手。

但老金我有个疑问：

官方为什么要限制SKILL.md在5000词以内？

从技术角度看，是为了保证Claude的上下文效率。

从设计角度看，是为了强制创作者"精简表达"。

或者说，A社设计这个系统时，本身就假设"好的Skill不需要长篇大论"？

老金我倾向于最后一个假设。

但你怎么看？

评论区告诉我，你对"5000词限制"的理解。

或者说，你认为一个好的Skill，应该控制在多少词以内？

老金我自己的👸号写作技能，SKILL.md写了800词，覆盖全部流程。

老金我觉得800词刚刚好，不多不多。

但这是老金我的个人体验，你的体验可能不同。

欢迎在评论区分享你的Skills制作经验。

或者转发给你的开发者朋友，让他们也看看这个官方6步教程。

老金我后续还会继续研究A社的其他官方文档，关注我，第一时间获取最新发现。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>