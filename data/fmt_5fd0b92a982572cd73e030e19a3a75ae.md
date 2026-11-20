
# PhantomShell 幻影终端

> 来源：[https://xodnytdcaw.feishu.cn/docx/OUGmdToZjoEpvcxSvPPcWx39n9e](https://xodnytdcaw.feishu.cn/docx/OUGmdToZjoEpvcxSvPPcWx39n9e)

帮我生成一个 PhantomShell 幻影终端信息（自我介绍 + 项目展示 + 技术表达）展示类网站。

## 需求背景与目标

核心理念：围绕 Slogan “No System Is Safe.”，打造一个融合黑客美学与技术表达的沉浸式终端交互网站。

适用对象：安全方向/技术表达型创作者，渴望打造独特身份名片。

核心目标：

- 品牌具象化：模拟「黑客终端」+ 动态 BIOS 启动界面 + 代码雨特效，强化技术玩家身份感。
- 技能展示：结构化展现编程能力、项目经验、安全背景。
- 互动传播：通过输入命令触发内容，鼓励探索式浏览和个性化交互。

## 核心功能模块设计

### Terminal Home｜主界面

主功能：模拟命令行终端，输入指令解锁各板块信息。

视觉风格：全屏黑底 + 绿色等宽字体（Fira Code），复古 CRT 显示器风格。

背景动画：

初始加载：模拟 BIOS 启动（“`Injecting identity.dll...`”、“`Decrypting resume.txt...`”），屏幕逐行加载命令。

常驻背景：`Three.js` 渲染的粒子编程矩阵 / 代码雨下落动画，带有伪 CRT 扫描线和轻微抖动。

鼠标互动：悬停触发“扫描波动”，移动时带有轨迹跟随粒子偏移。

背景音乐：

默认播放合成器风格氛围乐（推荐使用 JS 音频包加载 .ogg）

鼠标点击 / 命令输入附带机械键盘音效

命令导航（输入模拟命令后展开界面）：

`whoami` → 个人介绍（含头像、技能关键词 Tag 云）

`projects` → 项目经验卡片式展示（含 Github 链接）

`connect` → 社交链接 + 邮箱按钮（hover 时 ASCII 动效）

`sudo insight_lab` → 额外展示实验性质 side project + 招募互动表单

### Projects｜项目库

布局方式：响应式卡片瀑布流（含 Hover 放大、浮动代码片段）

筛选功能：可按关键词过滤（如“安全”、“数据可视化”、“AI 应用”）

内容来源：结合简历内经历（如「基于 `Flask` 的安全知识分享平台」、「静态代码扫描工具开发」等），配图+简述+链接跳转。

### About｜关于我

结构：

教育背景（家里蹲大学 - 计算机科学与技术）

荣誉与奖项（挑战杯、省级竞赛）

技术栈展示（Python、CSS、Java、Shell、Linux、`Nmap` 等图标矩阵展示）

可选加入 ASCII 艺术头像

特效：

内容滚动时触发 “伪打印” 动效，每行文字逐字弹出。

### 加载画面

全屏加载动画（暗色调 BIOS 检查风格）：

```
[*] Initializing PhantomShell...
[✓] Loading modules...
[✓] Injecting creativity.dll
[✓] Mounting profile.img
[✓] Done. Welcome back, Operator.
```

加载结束伴随“滴”声与画面闪现，进入主终端。

## 设备适配 & 技术说明（可选补充）

响应式设计，适配移动端 & PC

推荐技术栈：`React` + `TailwindCSS` + `Three.js` + `Howler.js`（音效）+ `Framer Motion`（动效）

## 背景特效补充要求

请将背景设置为绿色代码雨动画效果，并加入少量电子干扰与终端扫描线特效

加亮代码雨的颜色，并加入少量电子干扰与终端扫描线特效，同时注意所有的页面背景不要把代码雨遮挡住

让代码雨若隐若现，加入少量电子干扰，增加一些朦胧感的视觉感

## 预览

https://console.bce.baidu.com/miaoda/design/app-square/app-55rwwq2ccphe
