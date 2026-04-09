# 高级动画交互网页

> 来源：[https://xodnytdcaw.feishu.cn/docx/P1Kwdyv6LovUY5xSlwYcy9aQncb](https://xodnytdcaw.feishu.cn/docx/P1Kwdyv6LovUY5xSlwYcy9aQncb)

```
我正在使用 GSAP + ScrollTrigger 开发一个现代无人机官网首页页面，请你以交互设计师 + 动效设计师 + 前端开发者的视角，帮我生成一个可直接用于开发的 HTML 页面原型结构与动画代码，具体要求如下：

🎯 核心目标：打造动态感强、视觉沉浸的首页页面
页面包含多个区块（如 hero、about、features、faq），其中部分区块使用「电影式 SVG 动画」，其余区块采用常规滚动/卡片动画；
鼠标滚动驱动整体节奏，保持自然、流畅、连贯的动态体验；
页面整体以 GSAP Timeline + ScrollTrigger 控制动画逻辑，保证每一段动画是&quot;章节化&quot;播放的，沉浸式而不过载。

✅ 动效结构与要求
✅ SVG 动画组件要求（核心重点 · 请重点打磨）
- 整个组件以全屏 SVG 动画呈现，每一个 SVG 动画都必须具有明确的&quot;章节感&quot;和&quot;电影感&quot;，像一段交互式短片；
- 鼠标向下滚动触发动画，每一次滚动推进一部分动画，类似分镜动画滚动展示，用户必须完整观看当前 SVG 动画，才可滚动进入下一个页面组件；
- 动画过程需体现出强烈的动态交互感（而非轻微元素偏移），请使用以下动画手法增强表现力：
  - stroke-dashoffset / drawSVG 动画，让线条逐步绘制出轮廓；
  - 遮罩/剪切（clip-path / mask）动画，营造开场、过渡或图像拼接的戏剧感；
  - 视差滚动：前景与背景图层分离、错位运动，提升空间感；
  - SVG 元素组合变形（morphing），如几何图形变换为场景元素；
  - 渐隐 / 缩放 / 跳动 / 翻转等丰富的形变，制造&quot;活着的 SVG&quot;视觉；
- 动画需与 ScrollTrigger 精准绑定，使用 gsap.timeline() 创建清晰、可控的分段；
- 鼓励为每一幕设置动画进度条或简要提示文案，让用户有沉浸感、而非加载感；
- SVG 插图来自支持线上加载的站点（如 undraw.co、svg-repo.com、heroicons 等），确保可直接嵌入页面中；

常规滚动组件动画要求（如 hero / FAQ 等）：
- 使用 ScrollTrigger + gsap.from() 实现元素逐个浮现、滑动进入等效果；
- 保持轻量流畅，不与 SVG 动画冲突；
- 动画需要延迟加载，只有用户滚动到视区时才执行，节省初始渲染压力；

响应式动画支持：
- 使用 ScrollTrigger.matchMedia() 设置针对不同屏幕宽度的动画行为；
- 移动端应简化 SVG 动画（如静态展示或简化播放节奏）以优化性能；
- 所有动画与组件布局应自适应排版（Flex/Grid），不出现溢出或错位；

💡 项目输出结构要求
- 输出为一个完整 HTML 页面，使用 HTML + CSS（可用 Tailwind）+ 内联 SVG；
- 所有图形、插画、图标等必须使用在线可引用的 SVG 资源（不可留空图或 TODO）；
- 每个组件（SVG 动画或常规块）都有合理的注释，标明动画逻辑或触发条件；
- 所有 SVG 元素支持可视化动画（如线条画出、缩放入场、mask 过渡等）；
- 所有动画都整合进 gsap.timeline() 与 ScrollTrigger 管理；
```

原文：https://cursorhub.org/tutorials/projects/3d-web-project