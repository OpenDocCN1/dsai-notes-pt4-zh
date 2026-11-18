
# 2个步骤，生成高保真原型UI（附：ClaudeCode 详细提示词）

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/J66fdLA8EouotHxE9UkcqA50nxb](https://qqs7y1hozd1.feishu.cn/docx/J66fdLA8EouotHxE9UkcqA50nxb)

## Figma

AI 它越能干，人机协作交互就变得越来越重要。一个能组织协调各资源产生创意地协作平台，它的价值往往会放大，而不会随着AI地发展被削弱或者被替代。

![](img/c9b41ad1c95b321ba5723f5c3d9b6ad6.png)

```
Anima（高保真还原 + 动效/响应式）

官方文档首页：https://docs.animaapp.com/
Figma 插件页面（社区）：https://www.figma.com/community/search?resource_type=plugins&amp;q=Anima


Locofy.ai（高还原 + 代码更干净/Tailwind/React）

官方文档首页：https://docs.locofy.ai/
Figma 插件页面（社区）：https://www.figma.com/community/search?resource_type=plugins&amp;q=Locofy
```

提炼各个领域的提示词，描述足够清晰。比如以下Emoji猜四字成语提示词为例，直接基于ClaudeCode就能生成达到预期效果的产品雏形。我摘抄其中一个展示页面，感兴趣朋友下方提示词自取。

![](img/96b5005e0da17883594fa642054f6b29.png)

```
# 为“Emoji 猜成语 App”生成高保真原型与可开发级 HTML

你将扮演资深产品设计与前端工程一体化角色（产品经理 + UX 设计师 + UI 设计师 + 前端工程师），基于以下完整需求，为一款“类似 Emoji 猜成语”的移动应用生成高保真原型界面，并输出可直接用于开发联调的 HTML 原型工程。

请严格按以下结构产出：完成用户体验分析 → 产品界面规划 → 高保真 UI 设计说明 → 生成完整多页面 HTML 文件（分文件）→ 提供资源清单与验收要点。请不要省略任何一个页面与模块。

---

## 1. 项目概述与目标

- 项目名称：Emoji 猜成语 App（iOS/Android 通用视觉风格，偏 iOS）
- 目标：输出高保真原型界面与可运行的 HTML 原型工程（无后端），可直接交付开发继续接入数据与逻辑。
- 核心玩法：通过一组 Emoji 表情联想并输入对应四字成语，获得反馈与奖励；支持每日挑战、关卡闯关、排行榜、商店与个人中心。

---

## 2. 用户体验分析（在输出中先给出不超过 20 行的分析总结）

- 目标用户：休闲玩家、中文母语与学习者、社交分享偏好者。
- 核心价值：低门槛开玩、明确反馈、可持续激励（连胜、成就、奖励）。
- 核心流程：Home → Game（题面：Emoji + 输入/候选）→ Result（对错反馈、释义、分享、下一题）→ Leaderboard/Shop/Profile 形成闭环。
- 关键需求与反馈：正确高反馈（动效/彩带/加分）、错误轻反馈（可提示）、每日挑战节奏、社交与排行驱动留存。

请在开头先以要点形式输出你的 UX 总结与关键交互逻辑。

---

## 3. 产品界面规划（页面清单与信息架构）

必须包含以下页面，并给出每页的模块要点：
- Onboarding：3 张引导卡片、产品价值与开始按钮
- Login：Apple/Google/手机号登录入口
- Home：每日挑战卡片、继续闯关、分类入口、金币状态
- Game：题面 Emoji、四字答题格/键盘或候选、提示按钮、进度/计时/金币
- Result：正确/错误两态、成语释义、下一题、分享卡片
- Leaderboard：全球榜/好友榜切换、排名、头像、分数
- Shop：金币包、提示道具与礼包卡片；价格、折扣、热门标签
- Profile：头像、昵称、统计（胜率、连胜、完成数）、成就徽章
- Settings：音效、触感、通知、深色模式、语言、关于等

如有需要，可额外增加 Modal/Toast/Empty/Loading 等状态示意。

---

## 4. 高保真 UI 设计规范

- 设备与画布
  - 设备尺寸模拟：iPhone 15 Pro，画布 393×852 px
  - 圆角：约 44 px，带柔和阴影，整体容器溢出隐藏以模拟真机边框
  - 安全区：顶部状态栏与底部 Home 指示条区域需留白
- 状态栏与导航
  - 顶部状态栏：显示时间、信号、Wi-Fi、电量；中部模拟“动态岛”胶囊
  - 底部 Tab Bar：5 项（Home / Play / Rank / Shop / Profile），图标选中态有主色强调与标签
- 字体与排版
  - 字体栈：-apple-system, SF Pro Text, Segoe UI, Roboto, Noto Sans, Helvetica Neue, Arial, sans-serif
  - 标题/正文层级分明，适当使用字重与字间距，移动端友好行高
- 色彩与视觉
  - 主色：蓝紫渐变（如 indigo → purple）
  - 辅色：琥珀/橙色用于金币与激励
  - 背景：浅灰/白，强调卡片化与分区层级
  - 阴影与毛玻璃可适度使用（Tab Bar 背景可半透明 + blur）
- 图片与插图
  - 必须使用真实 UI 图片，不可使用文本占位（从 Unsplash/Pexels/Apple 官方资源）
  - 每张图片提供 alt 文本，保证加载失败的可读性
- 图标
  - 使用 Font Awesome 6（或等价开源库），统一风格，图标语义符合功能
- 无障碍
  - 交互控件添加 aria-label 或标题文本
  - 对比度足够，点击区域不小于 44×44 px

---

## 5. 技术规范与工程要求（HTML 原型）

- 技术栈：原生 HTML + Tailwind CSS（或 Bootstrap）+ Font Awesome。优先使用 Tailwind。
- 文件组织（每个页面必须独立 HTML 文件）：
  - index.html（主入口预览页，不直接写入所有页面内容；使用 iframe 平铺嵌入各页面）
  - onboarding.html
  - login.html
  - home.html
  - game.html
  - result.html
  - leaderboard.html
  - shop.html
  - profile.html
  - settings.html
- index.html 预览要求：
  - 将以上所有页面以 iPhone 15 Pro 设备框形式，通过多个 iframe 平铺展示，不做跳转
  - 每个 iframe 用卡片标题标注文件名
- 每个页面的结构要求（页面内自包含，不依赖其他本地文件）：
  - 引入 Tailwind CDN 与 Font Awesome CDN
  - 构建一个 .device 容器（393×852 px，圆角 44 px，阴影，overflow hidden）
  - 顶部状态栏（含时间、网络、电量、动态岛胶囊）
  - 页面主体区域（可滚动）
  - 底部 Tab Bar（5 个导航项，当前页高亮）
  - 使用真实图片 URL，不使用空占位；必要时提供 3～5 张可替换资源链接
  - 轻交互可用极少量原生 JS（如 Tab 切换或简单倒计时），禁用任何构建工具
- 响应式与适配
  - 以 393×852 为主；无需适配多终端，但避免固定像素导致内容溢出
  - 保留底部安全区（如额外 padding-bottom）
- 性能与可用性
  - 合理使用图片尺寸参数（Unsplash/Images CDN 的 w/h/fit/auto=format）
  - 交互控件可点击且具备 hover/active/disabled 状态样式（移动端考虑 active）

---

## 6. 资源与示例链接（可替换）

- 背景/封面示例：
  - https://images.unsplash.com/photo-1518779578993-ec3579fee39f?q=80&amp;w=1200&amp;auto=format&amp;fit=crop
- 卡片配图示例：
  - https://images.unsplash.com/photo-1557672172-298e090bd0f1?q=80&amp;w=1200&amp;auto=format&amp;fit=crop
- 头像示例：
  - https://images.unsplash.com/photo-1502685104226-ee32379fefbe?q=80&amp;w=400&amp;auto=format&amp;fit=crop
  - https://images.unsplash.com/photo-1544005313-94ddf0286df2?q=80&amp;w=400&amp;auto=format&amp;fit=crop
  - https://images.unsplash.com/photo-1546527868-ccb7ee7dfa6a?q=80&amp;w=400&amp;auto=format&amp;fit=crop
- 图标库：Font Awesome 6（CDN）

请在输出中集中列出你实际使用到的图片 URL 清单，确保均为真实可访问。

---

## 7. 页面要点与模块细化

- Onboarding
  - 顶部封面图；3 条价值主张卡片（每日挑战、智能提示、好友与榜单）
  - “立即开始”按钮，底部协议提示
- Login
  - App 图标/Logo 位；Apple 登录、Google 登录、手机号登录 3 选项
- Home
  - 今日挑战大卡（期数、Emoji 预告、连胜状态）
  - 快速开始按钮、排行榜入口
  - 分类入口宫格（如：经典四字、动物主题、成语接龙等）
  - 金币/设置入口（右上）
- Game
  - 题面 Emoji（示例：🌧️ + ☂️）
  - 四字答案输入格（支持候选键/自定义键盘二选一，原型可做静态或简易 JS 交互）
  - 提示按钮（消耗金币），进度/计时显示
- Result
  - 正确/错误两态（正确显示彩带或高亮，错误轻提示）
  - 成语释义、例句、来源；下一题与分享卡片
- Leaderboard
  - 全局榜/好友榜切换；Top3 特殊高亮；我的排名锚点
- Shop
  - 金币包、提示道具与礼包卡片；价格、折扣、热门标签
- Profile
  - 头像、昵称、UID；统计（胜率、连胜、已完成）
  - 成就徽章宫格；编辑资料入口
- Settings
  - 开关项：音效、触感反馈、通知、深色模式、语言
  - 关于/隐私政策/清理缓存

---

## 8. 设计令牌与样式基线（建议）

- 颜色
  - 主渐变：from-indigo-500 to-purple-500
  - 强调/金币：amber-500 / orange-500
  - 背景：neutral-50/100；边框：neutral-200；文本：neutral-900/600
- 圆角：卡片 16–20，按钮 12–16，图标按钮 12
- 阴影：卡片 shadow-md，浮层/模态 shadow-lg
- 间距：容器 padding 20–24，栅格 8–12
- 按钮：主按钮黑/深色底白字，次按钮白底边框，图标按钮圆角方形

---

## 9. 输出要求与交付物

- 在你的最终输出中，务必包含：
  1) 简明用户体验分析与核心交互逻辑（不超过 20 行）
  2) 产品界面规划清单与关键模块要点
  3) 高保真 UI 设计说明（颜色/字体/组件/状态栏/Tab Bar）
  4) 完整 HTML 原型工程代码（分文件），满足：
     - 每个页面是独立 HTML 文件：onboarding.html、login.html、home.html、game.html、result.html、leaderboard.html、shop.html、profile.html、settings.html
     - index.html 作为预览入口，通过多个 iframe 平铺展示以上所有页面，不做跳转
     - 每个页面内置 iPhone 15 Pro 设备框、顶部状态栏与底部 Tab Bar
     - 使用 Tailwind CDN 与 Font Awesome CDN
     - 使用真实图片 URL 并提供图片清单
  5) 资源清单（图片与图标）与替换指引
  6) 简短的本地预览说明（如何保存文件并用浏览器打开 index.html）

- 代码规范
  - 通过语义化 HTML 与可读命名；提供 alt、aria-label
  - 避免外部构建工具；仅使用 CDN；轻量原生 JS 允许但不强制
  - 所有链接为相对路径（./home.html 等）

---

## 10. 验收清单（请在输出末尾附带并逐条自检）

- [ ] index.html 仅使用 iframe 平铺展示所有页面，且每个页面可独立打开
- [ ] 每个页面均有设备框（393×852）与 44 px 圆角
- [ ] 顶部状态栏与底部 Tab Bar 完整一致，当前 Tab 高亮
- [ ] 所有图片为真实 URL，带 alt 文本
- [ ] 使用 Font Awesome 图标；交互控件具备触态样式
- [ ] 文案、布局与样式符合上述设计规范；无明显溢出
- [ ] 所有页面零构建依赖，可直接本地双击 index.html 预览

---

请严格遵循以上提示生成最终交付内容与完整代码。
```
