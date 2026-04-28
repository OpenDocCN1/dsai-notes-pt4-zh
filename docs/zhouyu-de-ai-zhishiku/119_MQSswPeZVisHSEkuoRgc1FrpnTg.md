# Agent Skills实战指南：营销增长与SEO优化全攻略 副本

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/J3ZhdbUDQo4JdkxvCIecZsZin8f](https://xatc0v8uz5m.feishu.cn/docx/J3ZhdbUDQo4JdkxvCIecZsZin8f)

## Agent Skills 定义

### 专业表述

Agent Skills 是一种轻量、开放的格式，用于为 AI 代理扩展专业能力和工作流。它把指令、脚本和资源打包成可被代理发现和按需加载的文件夹，使代理在特定任务上更准确、更高效。Skills 提供的是程序性知识（procedural knowledge），即「怎么做」的步骤和规则，而不是「是什么」的静态描述。

### 通俗表述

可以把 Skills 理解为 AI 的「技能包」或「插件」：作者写一次，代理在需要时自动加载。它解决的是代理缺少任务相关上下文的问题——通过把组织知识、团队流程和领域规范写成可复用的 Markdown 文件，让代理在合适时机读取并执行，从而获得更稳定、可审计的行为。

## Agent Skills 历史

### 起源与发布

Agent Skills 由 Anthropic 提出并实现。2025 年 10 月 16 日，Anthropic 在 Introducing Agent Skills 中正式发布 Skills，首先用于 Claude 应用、Claude Code 和 API，用于为 Claude 提供可加载的指令、脚本和资源，以完成特定任务。

### 开放标准

2[025 年 12 月 18 ](https://agentskills.io/)日，Anthropic 将 Agent Skills 作为开放标准发布，规范与文档托管于 agentskills.io，并同步推出组织级管理能力和合作伙伴目录。这一动作发生在 MCP（Model Context Protocol）捐赠给 Linux 基金会（2025 年 12 月 9 日）之后约 10 天，形成「MCP 负责外部连接，Skills 负责程序性知识」的互补架构。

### 行业采用

标准发布[后，多家厂商开始采用](https://github.com/vercel-labs/skills)。OpenAI 在 ChatGPT 和 Code[x CLI 中实现](https://skills.sh/)了与 Skills 规范兼容的架构；微软在 VS Code 和 GitHub Copilot 中集成；Cursor、Claude Code、OpenCode、Gemini CLI 等也支持该标准。Vercel Labs 开源了 skills CLI，提供 npx skills add 安装能力，skills.sh 成为技能发现与排行榜平台。

### 时间线概览

## Tool、MCP、RAG、Skill、Prompt 的区别

在 AI 代理的配置中，Prompt、Tool、MCP、RAG、Skill 各自解决不同问题，彼此互补而非替代。Prompt 是单次对话中的即时指令，随会话结束而消失；Skill 是可复用的能力包，把「怎么做」的程序性知识打包成可版本控制的文件，供代理按需加载。Tool 是单一可执行函数，代理调用后会产生实际效果（查库、调 API、写文件）；MCP 是连接代理与外部服务的协议层，负责工具发现、认证和通信。RAG 是检索增强生成，从知识库中检索文档并注入上下文，用于增强回答，但不执行动作。

### 精简对照

## Skills 与普通 Markdown 的区别

Agent Skills 在普通 Markdown 之上增加了面向 AI 的约定：每个技能是一个目录，内含 SKILL.md，必须包含 YAML frontmatter（name、description）和 Markdown 正文。description 用于代理发现与路由，name 需与目录名一致。Skills 采用渐进披露：启动时只加载元数据，激活时再加载正文，references/、scripts/ 按需加载。普通 Markdown 没有这些结构，也不参与代理的发现与调用流程。

### 对比表格

> 如何查看自己的skills是否规范，是否被skills.sh收录了：https://skills.sh/kostja94

## Agent Skills 使用场景

1.  个人开发者 / 独立创作者

个人开发者或创作者希望快速搭建网站、应用或内容，但不想系统学习某个领域（如 SEO、文案、设计）。通过安装对应技能包，用自然语言向代理提问，即可获得符合最佳实践的产出。技能把领域知识封装成可复用的指令，代理按需加载，用户无需记忆复杂规则。典型流程：安装技能 → 描述需求 → 代理自动选用技能并输出结果。

1.  产品团队：前后端 / 工程与运营分工

产品团队希望工程负责核心功能，运营或市场独立产出营销页、落地页、文档等，而不依赖每次发版。技能让运营在 IDE 或协作工具中直接使用代理，按既定模板和规范生成内容；工程只需提供技术能力（如 rewrite、静态托管），无需为每次活动写代码。技能成为运营与工程之间的「协议」，保证输出格式和质量一致。

1.  学习与探索：把技能当教材

技能是 Markdown 文件，本身可作为学习材料。不一定要接入项目：可以安装后阅读，或在 ChatGPT、Claude、Gemini 中粘贴技能内容并提问，让 AI 解释概念、举例、答疑。适合想了解某领域（如 SEO、付费广告、内容策略）但暂时没有实际项目的人，也适合评估某技能包是否适合自己。

1.  Vibe 编码 / 低代码平台：内置技能作为默认模板

Bolt.new、v0、Lovable 等平台希望用户生成的项目默认具备一定专业水准（如 SEO、可访问性、转化结构）。将技能内置为模板，用户通过自然语言描述需求时，代理自动应用这些技能，输出更规范、更易维护的结果。技能遵循开放规范，可跨平台复用，平台只需集成一次，即可提升整体产出质量。

1.  自建技能：基于现有技能库扩展

想为团队或产品定制技能，但不知从何下手时，可以 Fork 成熟技能库（如 marketing-skills），在其基础上修改、删减或新增。现有技能提供格式、结构和依赖关系参考，可大幅降低创作门槛。典型流程：Fork → 阅读 skills-guide → 复制并修改相近技能 → 安装到本地或团队环境验证。

## Skills 安装方式

Skills 本质是带 SKILL.md 的文件夹，可放在项目或用户目录中。支持原生 Skills 的平台（如 Cursor、Claude Code、OpenClaw）会从约定路径自动发现并加载。安装方式主要有两种：用 CLI 从 GitHub 安装，或手动复制到对应目录。

方式一：CLI 安装（推荐）

```
# 安装全部技能
npx skills add owner/repo

# 只安装指定技能
npx skills add owner/repo --skill skill-name-1 skill-name-2

# 仅列出可用技能（不安装）
npx skills add owner/repo --list

# 示例：安装 marketing-skills
npx skills add kostja94/marketing-skills

# 示例：只安装部分技能
npx skills add kostja94/marketing-skills --skill robots-txt title-tag meta-description
```

CLI 会把技能安装到当前平台的默认目录（如 .cursor/skills/），并把仓库里的层级结构扁平化为单层目录名。

---

方式二：Clone 后手动复制

需要 templates、docs 或完整仓库结构时使用。

```
# 克隆仓库
git clone https://github.com/owner/repo.git

# 复制技能到 Cursor 项目目录
cp -r repo/skills/* .cursor/skills/

# Claude Code 使用 .claude/
# cp -r repo/skills/* .claude/skills/

# 如需 templates
# cp -r repo/templates .cursor/
```

与 GitHub 同步更新

本地技能要和 GitHub 保持同步，可以用下面几种方式：Git Submodule 适合需要版本管理的项目；直接 clone 后定期 pull 适合单独维护；CLI 重新安装最简单；一键脚本适合频繁更新；符号链接适合不想复制文件的情况。下面是一份整合后的代码示例，可按需选用其中一种方式。

```
# ========== 方式一：Git Submodule（适合纳入版本管理） ==========
# 首次添加
git submodule add https://github.com/kostja94/marketing-skills.git .cursor/marketing-skills
cp -r .cursor/marketing-skills/skills/* .cursor/skills/

# 更新
cd .cursor/marketing-skills &amp;&amp; git pull origin main &amp;&amp; cd -
cp -r .cursor/marketing-skills/skills/* .cursor/skills/


# ========== 方式二：直接 Clone 后定期 pull ==========
# 首次
git clone https://github.com/kostja94/marketing-skills.git .cursor/marketing-skills
cp -r .cursor/marketing-skills/skills/* .cursor/skills/

# 更新
cd .cursor/marketing-skills &amp;&amp; git pull origin main &amp;&amp; cd -
cp -r .cursor/marketing-skills/skills/* .cursor/skills/


# ========== 方式三：CLI 重新安装（覆盖更新） ==========
npx skills add kostja94/marketing-skills


# ========== 方式四：一键更新脚本 ==========
# 保存为 update-skills.sh 后执行 bash update-skills.sh
SKILLS_DIR=&quot;.cursor/marketing-skills&quot;
TARGET_DIR=&quot;.cursor/skills&quot;
if [ ! -d &quot;$SKILLS_DIR&quot; ]; then
  git clone https://github.com/kostja94/marketing-skills.git &quot;$SKILLS_DIR&quot;
fi
cd &quot;$SKILLS_DIR&quot; &amp;&amp; git pull origin main &amp;&amp; cd -
cp -r &quot;$SKILLS_DIR/skills/&quot;* &quot;$TARGET_DIR/&quot;
echo &quot;Skills updated.&quot;
```

说明：方式一、二需要先有 .cursor/skills/ 目录；方式三会覆盖已有同名技能；方式四首次会 clone，之后每次执行都会 pull 并复制到 .cursor/skills/。

## Skills 可以使用的平台

Skills 是 Markdown 文件，只要 AI 能读取文本，就可以使用，不依赖特定平台的原生支持。

原生支持平台：Cursor、Claude Code、Codex、OpenCode、Gemini CLI、GitHub Copilot、Windsurf、Cline、Base44、Trae 等会在启动时自动发现技能目录，将技能放入 .agents/skills/、.cursor/skills/ 或 .claude/skills/ 即可。OpenClaw 支持相同格式，可复制到 ./skills 或 ~/.openclaw/skills/，或使用 npx skills add kostja94/marketing-skills -a openclaw 安装。

无原生支持时：Lovable、v0、Bolt、Replit 可通过 AGENTS.md 或平台指定目录引用；ChatGPT、Gemini、Claude Web 可将技能 Markdown 粘贴进对话作为上下文；任何能访问文件的 AI 产品，只要把技能放在项目根目录并在提示中引用即可使用。

> 是否原生支持可以查看：https://github.com/kostja94/marketing-skills/blob/main/docs/where-to-use-skills.md

---

## Marketing Skills：业务驱动的 AI 营销技能库

Marketing Skills 是一套面向 AI 代理的营销与 SEO 技能库，包含 144 个 Markdown 技能，覆盖 SEO、内容、42 种页面类型、渠道、平台、策略、组件和分析等场景。它面向 Cursor、Claude Code、OpenClaw 等支持 Agent Skills 的平台，也适用于 Lovable、ChatGPT、Gemini 等能读取 Markdown 的 AI 工具。

这套技能库是业务驱动的，源于我在实际工作中的需求。在做产品站、落地页、博客和营销内容时，需要 AI 能按 SEO 规范、页面结构和转化逻辑来输出，而不是泛泛而谈。于是我把这些流程、最佳实践和检查清单整理成技能，让 AI 在相关任务中自动加载并遵循。每个技能聚焦一个具体任务，例如优化 title tag、配置 robots.txt、设计 pricing 页面或撰写 landing page 文案，并包含步骤、边界情况和 Related Skills 引用，方便代理按需组合使用。

## Available Skills

目前共 144+ 个技能，按业务场景和文件夹结构分为九大类。

### SEO（skills/seo/）

覆盖技术 SEO、页面优化、内容 SEO、外链和本地 SEO，以及 Parasite SEO、Programmatic SEO 等战术。

+   technical：robots.txt, sitemap, canonical, indexing, indexnow, crawlability

+   on-page：title, description, featured-snippet, serp-features, metadata, open-graph, twitter-cards, schema, internal-links, url-structure, heading, image-optimization

+   off-page：link-building, backlink-analysis

+   content：keyword-research, content-strategy, content-optimization, eeat-signals, competitor-research

+   local：local-seo

+   tactics：parasite-seo, programmatic-seo

### Content（skills/content/）

跨渠道内容创作与本地化，包括文案、视频、视觉素材和翻译。

+   copywriting

+   video

+   visual-content

+   translation

### Paid Ads（skills/paid-ads/）

付费广告策略与各平台投放，从 Google、Meta 到 TikTok、Reddit、目录广告等。

+   google-ads, meta-ads, linkedin-ads, reddit-ads, tiktok-ads

+   app-ads, ctv-ads, display-ads, directory-listing-ads, youtube-ads, native-ads

### Pages（skills/pages/）

按页面类型划分，覆盖品牌、内容、营销、法律和工具类页面，共 40+ 种。

+   brand：home, about, contact

+   content：article, blog, features, faq, glossary, resources, docs, api, tools, template-page

+   marketing：landing-page, products, pricing, customer-stories, category-pages, showcase, integrations, affiliate-program, download, media-kit, press-coverage, alternatives, use-cases, solutions, services, migration, contest, startups

+   legal：privacy, terms, cookie-policy, legal, refund, shipping

+   utility：404, careers, changelog, status, feedback, disclosure

### Components（skills/components/）

页面中可复用的 UI 组件，包括导航、转化、品牌、内容、布局和工具类。

+   navigation：navigation-menu, breadcrumb, footer, sidebar, toc

+   conversion：cta, popup, newsletter-signup, trust-badges, testimonials

+   branding：logo, favicon, brand-visual, hero

+   content：tab-accordion

+   layout：card, grid, list, masonry, carousel

+   utility：top-banner, social-share, url-slug

### Channels（skills/channels/）

获客与分发渠道，如联盟、邮件、创作者、社区、目录和 PR 等。

+   affiliate, email-marketing, egc, influencer, referral

+   creator-program, community-forum, directories, distribution-channels, pr

### Platforms（skills/platforms/）

各发布平台的运营与 SEO，如 X、Reddit、LinkedIn、TikTok、YouTube、Pinterest、Medium、GitHub、Grokipedia。

+   x, reddit, linkedin, tiktok, youtube, pinterest, medium, github, grokipedia

### Strategies（skills/strategies/）

增长与营销策略，包括 SEO、付费广告、冷启动、PMF、GTM、定价、转化、留存、品牌保护等。

+   seo, paid-ads, branding, content-marketing, website-structure

+   cold-start, indie-hacker, geo, integrated-marketing, localization

+   pmf, gtm, product-launch, conversion, retention

+   discount-marketing, pricing-strategy, rebranding, brand-protection

+   domain-selection, domain-architecture, multi-domain-brand-seo

+   growth-funnel, research-sources

### Analytics（skills/analytics/）

流量、转化与 SEO 数据监测，以及 AI 流量追踪和 Search Console 分析。

+   traffic, tracking, seo-monitoring, ai-traffic, google-search-console

## Usage：如何调用相关Skills

安装好 marketing-skills 后，直接和代理对话即可。代理会根据你的自然语言意图，从已加载的技能中选出合适的 skill 并应用。你不需要手动指定技能名，只要说出任务或关键词，代理就会自动匹配对应 skill。

每个 skill 的 description 里都包含触发关键词。代理在启动时会加载所有 skill 的 name 和 description，在对话中根据你的提问和这些元数据做匹配。因此，用日常说法提问即可，例如「帮我做 SEO 策略」「优化 title tag」「建一个落地页」。

下面是一些常见说法与对应 skill 的对照表，方便你理解如何触发：

若想跳过技能自带的介绍或背景说明，直接要结果，可以说「skip intro」或「just do it」，代理会直接输出主内容。重复执行同一任务时，这个用法尤其有用。

部分平台（如 Cursor）支持手动调用技能：在 Agent 聊天中输入 / 并搜索技能名，可显式指定要使用的 skill。适合需要强制走某个流程或排查技能是否被正确触发时使用。

示例对话：

+   你：「帮我做一份 SEO 策略」→ 代理调用 seo-strategy

+   你：「优化这个页面的 title 和 meta」→ 代理调用 title-tag、meta-description

+   你：「建一个落地页，要突出产品价值」→ 代理调用 landing-page-generator

+   你：「创建落地页，skip intro」→ 代理直接输出落地页内容，不先讲背景

## 如何用好Skills：项目上下文与关联

没有项目上下文时，AI 输出会偏通用。在 .cursor/、.claude/ 或 .lovable/ 等平台配置目录中放入 product-marketing-context.md，可以让 Agent 生成更贴合项目的文案和策略。可用的关联方式包括：产品上下文（产品、受众、品牌）、任务进度模板、项目规则、AGENTS.md 以及聊天中的 @file 引用。建议先填写产品概览、定位、目标受众和品牌调性，再逐步补充关键词、竞品和网站信息，并定期更新，避免上下文过时影响效果。

## 如何寻找好用的 Skills

最直接的方式是使用 skills.sh 目录。访问 https://skills.sh 可按 All Time、Trending（24h）、Hot 等维度浏览，排行榜按安装量排序，安装量高的通常更受认可。首页有搜索框，输入关键词（如 SEO、React、copywriting）即可筛选相关技能。

命令行可用 npx skills add owner/repo --list 查看某仓库中的技能列表，或用 npx skills find [关键词] 搜索并安装。安装 find-skills 技能后，在对话中问「有没有做 X 的技能？」或「帮我找 SEO 相关的 skill」，代理会帮你发现并安装合适的技能。

选择技能时，优先看安装量、来源（如 vercel-labs、microsoft、anthropics 等官方或知名团队）和 description 是否清晰。skills.sh 会对技能做常规安全审计，但无法保证全部安全，安装前建议打开 SKILL.md 快速浏览内容。

## Skills 安全与投毒风险

Skills 以 Markdown 和脚本形式存在，同样面临供应链攻击。攻击者会在 Skills 中植入恶意内容，利用 AI 代理的高权限窃取凭证或执行恶意代码，即「Skill 投毒」。

与传统包投毒不同，Skill 投毒利用的是「Markdown 即安装」：SKILL.md 中的 Setup 代码块会被用户或代理执行，攻击者常在此处嵌入 base64 编码的恶意脚本，伪装成依赖安装命令。

2026 年初的安全研究显示：ClawHavoc 在 OpenClaw 生态中发现 1,184+ 个恶意 Skill；Snyk 的 ToxicSkills 研究对近 4,000 个 Skill 审计后，发现 36.82% 存在安全缺陷，13.4% 存在严重问题（恶意软件、提示注入、凭证泄露）。

Skill 投毒危害更大，是因为 Skills 继承代理的完整权限：Shell、文件系统、环境变量、API 密钥等。发布 Skill 门槛低，通常无代码签名或安全审核。skills.sh 建议：安装前自行审查，对包含脚本的 Skill 要像对待可执行代码一样谨慎。

## 如何推广自己的 Skills

技能通过 skills.sh 生态被发现和安装。用户执行 npx skills add owner/repo 时，CLI 会从 GitHub 拉取技能，并上报匿名安装数据。排行榜按安装量排序，无需单独提交，只要仓库在 GitHub 上且包含 SKILL.md，就会被纳入生态。

把技能放在 GitHub 公开仓库中，确保每个技能目录下有 SKILL.md。在 README 中写清安装命令（如 npx skills add kostja94/marketing-skills）、适用场景和技能列表。description 要包含触发关键词，便于用户搜索和代理匹配。

在 skills.sh 搜索你的技能名或相关关键词，确认能被找到。鼓励用户 Star 仓库，有助于在 GitHub 和社区中提高可见度。若面向特定平台（如 Cursor、Lovable），可在对应社区或文档中说明安装方式。

关于Agent生态的思考：https://web.okjike.com/u/C30D9805-A900-46DB-B4A6-655D22A1824D/repost/69a06c7325bae56612854010