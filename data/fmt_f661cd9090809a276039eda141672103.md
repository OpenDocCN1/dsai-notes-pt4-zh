
# Obsidian+AI

> 来源：[https://xodnytdcaw.feishu.cn/docx/DyxUdol26oET7hxATTzcqRJ2ndd](https://xodnytdcaw.feishu.cn/docx/DyxUdol26oET7hxATTzcqRJ2ndd)

## Obsidian 下载

https://obsidian.md/download

![](img/2dcf7d46961227cee6100e8b93690055.png)

## 外观设置

![](img/c4ebac7a0d11b93838dafdb4fd34e138.png)

## 主题推荐

设置-外观-主题-管理，搜索：

> 推荐主题

+ `Primary`(推荐)

+ [`phycat`](https://github.com/sumruler/obsidian-theme-phycat)（推荐）

+ `AnuPpuccin`

+ `Baseline`

+ `Typora-Vue`

+ `Zen`（建议深色） 自用

+ `minimal` 推荐

+ `cupertino`

+

## 插件推荐

设置-第三方插件-关闭安全模式---社区插件市场，搜索：

### Floating Search：内容搜索

> Floating Search: Search: tag: (Find tag)→快捷键 `Ctrl + F`

### NoteToMP：一键发👸号草稿箱（插件市场有）

### superpowers：文章变成 Skill

https://github.com/obra/superpowers

### Notebook Navigator

文件自动分类

### Image auto upload：图床

1. Install Image Auto Upload Plugin.
1. Set Default Uploader to `PicGo(app)`.
1. API Endpoint: `http://127.0.0.1:36677/upload`

### Custom Frames：能把网页嵌入右侧边栏

快捷键：`Ctrl+P`→ 搜索 `Custom Frame` 配置快捷键

![](img/da18cdbc115614ff84d9dd429386796a.png)

> 补充：`Web Viewer` 是 `Obsidian` 1.8.3 及以上版本的核心插件，默认是启用状态；若没找到，可在「设置 - 核心插件」中找到 “网页浏览器” 并勾选启用

>

> + 场景 1：主动打开网页
>
> 在 `Obsidian` 中新建空白标签页（点击左侧 “+” 号），标签页顶部会显示 “Open Web Viewer” 按钮，点击后直接输入网址，就能在 `Obsidian` 内置浏览器中打开网页。
>
>
> + 场景 2：从笔记中打开链接
>
> 在笔记里遇到外部链接时，按住 `Ctrl` 键（Mac 按 `Cmd` 键）并点击链接，会直接调用内置 `Web Viewer` 打开网页，不用跳转到系统浏览器，边看网页边记笔记更高效。

![](img/617506ed079a91d6179655e0602ef4da.png)

### Claudian：把 `Claude Code` 整合到侧边栏，读取当前 `Markdown` 对话（插件市场没有）

#### 配置 `obsidian-skills`

##### 创建 `claude.json`

在 `.claude` 文件下载创建 `claude.json`，内容使用如下

```
{
"skills"
:
[
"./skills/obsidian-skills/obsidian-markdown"
,
"./skills/obsidian-skills/json-canvas"
,
"./skills/obsidian-skills/obsidian-bases"
]
}
```

##### 创建 `skills`

在 `.claude` 文件创建 `skills` 文件夹，里面下载 https://github.com/kepano/obsidian-skills

![](img/c50b88f4688deeeb7e302fb391f64a3d.png)

###### 此 `skills` 技能/应用场景补充

![](img/c13916768d96833025bf427b82fc6957.png)

![](img/df240fd30b5202850096222d97d9b9a8.png)

###### 模板库

![](img/c693f9a7d63d40f08f2ba22b6cc98401.png)

#### A. 基础用法

##### 1️⃣ `obsidian-markdown`

> 这个技能让 AI 能够创建符合 `Obsidian` 规范的 `Markdown` 文件，支持所有 `Obsidian` 特有语法。

###### 示例 1: 创建项目笔记

```
创建一个名为"项目计划.md"的笔记，包含以下内容：
- Frontmatter 属性：项目状态为进行中，优先级为高
- 使用 callout 标注重要截止日期
- 创建任务清单，包含已完成和待办任务
- 添加项目架构的 Mermaid 流程图
- 链接到相关的"团队文档"笔记
```

###### 示例 2: 创建学习笔记

```
创建"JavaScript 学习笔记.md"，包含：
 - 使用 info callout 介绍学习目标
 - 代码高亮示例（JavaScript 和 Python）
 - 使用嵌套列表组织知识点
 - 添加数学公式示例
 - 创建折叠的 FAQ callout
```

##### 2️⃣ `json-canvas`

> `JSON Canvas` 是一种开放格式，用于创建无限画布。AI 可以自动生成思维导图、流程图、项目看板等可视化内容。

###### 示例 1: 创建思维导图

```
创建一个名为"产品开发思维导图.canvas"的文件，主题是"新功能开发"，包含：
- 中心节点："新功能开发"（使用青色）
- 分支节点：需求分析、设计、开发、测试、部署（5个分支）
- 每个分支下添加 2-3 个子节点，包含具体任务
- 使用箭头连接所有节点
- 使用不同颜色区分不同阶段
```

###### 示例 2: 创建项目看板

```
创建"项目看板.canvas"，实现看板视图：
- 创建 3 个分组：待办、进行中、已完成（使用红、黄、绿色）
- 每个分组内添加 2-3 个任务卡片
- 任务内容包含任务名和描述
- 合理布局，确保美观
```

###### 示例 3: 创建流程图

```
创建"用户注册流程.canvas"：
- 节点：开始 → 输入信息 → 验证数据 → {数据有效?}
- 决策节点：Yes → 创建账户 → 发送邮件 → 结束
- 决策节点：No → 显示错误 → 返回输入
- 使用箭头和标签标注流程
- 决策节点使用黄色，开始/结束使用绿色
```

###### 示例 4: 创建研究资料画布

```
创建"AI研究.canvas"画布：
- 中心节点：AI 研究主题
- 周围节点链接到相关的笔记文件（使用 file 节点）
- 添加外部资源链接（使用 link 节点）
- 将相关内容用 group 节点分组
- 使用带标签的连线说明关系
```

##### 3️⃣ `obsidian-bases`

> `Obsidian Bases` 是基于 `YAML` 的数据视图系统，可以将笔记转换为数据库表格、卡片视图等。AI 可以自动创建配置文件。

###### 示例 1: 任务管理系统

```
创建"任务管理.base"，实现以下功能：
- 筛选所有包含 #task 标签的 `Markdown` 文件
- 创建公式字段：
  - days_until_due: 计算距离截止日期的天数
  - is_overdue: 判断是否过期
  - priority_label: 根据 priority 字段显示 🟥🟨🩩 图标
- 表格视图：显示任务名、状态、优先级、截止日期、距离天数
- 按状态分组排序
- 添加平均天数统计
```

###### 示例 2: 阅读清单

```
创建"阅读清单.base"：
- 筛选包含 #book 或 #article 标签的笔记
- 创建公式：
  - reading_time: 根据页数估算阅读时间（每页 2 分钟）
  - status_icon: 根据 status 显示 📖/✅/📚 图标
  - year_read: 提取完成日期的年份
- 卡片视图：显示封面、书名、作者、状态图标
- 表格视图：显示书名、作者、页数、预计时间、状态
- 排除已放弃的书籍
```

###### 示例 3: 项目进度追踪

```
创建"项目追踪.base"：
- 筛选"Projects"文件夹下的所有 `Markdown` 文件
- 创建公式：
  - last_updated: 显示相对更新时间（如"3天前"）
  - link_count: 统计笔记中的链接数量
  - progress: 根据完成任务的百分比计算进度
- 表格视图：显示项目名、状态、更新时间、链接数、进度
- 按状态分组
- 添加进度条可视化（使用 icon 函数）
```

###### 示例 4: 每日笔记索引

```
创建"日记索引.base"：
- 筛选"Daily Notes"文件夹下文件名格式为 YYYY-MM-DD 的笔记
- 创建公式：
  - word_estimate: 估算字数（文件大小除以 5）
  - day_of_week: 提取星期几
  - has_images: 判断是否包含图片
- 表格视图：显示日期、星期、估算字数、图片数量
- 限制显示最近 30 天
- 按修改时间倒序排列
```

###### 示例 4: 附件

```
请根据附件内容生成一个 高颜值Obsidian Canvas，每个主题建立一个分组，设置颜色，注意，每个节点之前的距离不要太靠近，中文输出
```

#### B. 高级技巧：组合使用多个 `Skills`

##### 技巧 1: 生成项目看板

```
为我的项目创建完整的文档系统：
1. 创建"项目看板.canvas"可视化展示项目结构
2. 创建"任务管理.base"追踪所有任务
3. 创建"项目主页.md"作为索引，包含：
   - 项目概述（使用 callout）
   - 嵌入项目看板
   - 嵌入任务管理视图
   - Mermaid 架构图
   - 相关文档链接
```

##### 技巧 2: 学习系统

```
创建"主题学习"系统：
1. 创建"知识地图.canvas"，用思维导图展示知识结构
2. 创建"学习资源.base"，管理所有学习资料（书籍、文章、视频）
3. 创建"学习笔记.md"模板，包含：
   - 学习目标（checklist）
   - 重要概念（callout 标注）
   - 代码示例
   - 笔记总结
```

##### 验证

打开 `Claudian` 插件对话验证测试，发送

```
请列出你检测到的本地 `skills`（`json-canvas` / `obsidian-markdown` /
`obsidian-bases`），并在常用工具创建一个测试笔记`赵国强.md` 包含 `[[链接]]`、`#tag`、- [ ] todo。
```

请列出你检测到的本地 `skills`（`json-canvas` / `obsidian-markdown` /

`obsidian-bases`），并在常用工具创建一个`skills`使用说明教程.md 要说明这3个`skills`怎么使用结合AI使用，比如调用AI自动化创建文件`JSON Canvas`，

1. 创建 `Slash Commands`

项目命令：项目根目录`.claude/commands/` 下，仅当前项目可用。

`Slash Commands`即自定义斜杠命令。

```
**环境准备**
mkdir -p ~/.claude/commands     #创建命令目录（递归创建，不存在则自动生成）
touch ~/.claude/commands/search-web.md   # 创建命令文件（以search-web.md为例）
```

编写命令文件（`Markdown` 格式）

每个命令对应一个 `.md` 文件，包含元数据与执行逻辑（示例）

```
---
# 元数据（YAML格式，必须放在开头）
description: "调用搜索工具查询指定关键词的网页信息" # 命令描述
argument-hint: "搜索关键词（如：`Slash Commands` 最佳实践）" # 参数提示
allowed-tools: ["Search"] # 允许调用的工具（如`Search`、`Read`、`Edit`）
---
# 命令执行逻辑（提示词模板，$ARGUMENTS为参数占位符）
请使用搜索工具查询以下关键词的最新信息，并整理成结构化摘要：
$ARGUMENTS
```

在 `Claude Code` 中输入 `/search-web` `Slash Commands` 自定义方法，自动替换 `$ARGUMENTS` 为传入参数。

## 其他推荐

### chrome插件剪藏工具

`Obsidian Web Clipper` 剪藏工具

https://chromewebstore.google.com/detail/obsidian-web-clipper/cnjifjpddelmedmihgijeibhnjfabmlf

### 字体推荐

中文 `TsangerJinKai02`

https://github.com/tw93/MiaoYan-NetNewsWire-Theme/blob/main/TsangerJinKai02-W04.ttf

中文 `LxgwWenKai`

https://github.com/lxgw/LxgwWenKai

英文代码 `JetBrains Mono`

https://www.jetbrains.com/lp/mono/
