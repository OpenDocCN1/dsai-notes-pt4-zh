
# 月度总结

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/JtdhdsMYVo6UpWxApKBcWJKinfc](https://xatc0v8uz5m.feishu.cn/docx/JtdhdsMYVo6UpWxApKBcWJKinfc)

```
请你分步骤，逐月搜索，找到自2025年1月至10月，英伟达的月末总市值数据。

找到所有信息后，创建一个HTML页面，页面加载后JavaScript自动生成一个完整的、自包含的SVG可视化图表。SVG需要使用width=&quot;100%&quot;来自动适应容器宽度，height 不做设定，留出少部分上下边距，SVG应自带合适的深色背景，与品牌颜色相称，12px 圆角，没有阴影。

JavaScript的工作流程：在页面加载时（或在script标签中立即执行），根据你搜索到的真实市值数据，计算每个数据点的坐标位置，然后通过DOM操作（如createElement、createElementNS、appendChild等）动态构建完整的SVG元素，包括：背景、坐标轴、刻度线、网格线、数据点圆圈、真实数值标注、两条折线路径，以及SVG原生的动画元素（animate、animateTransform等）。所有内容生成后插入到页面中id为&quot;Chart&quot;的SVG元素内。注意：纵坐标从0开始，以稍微超过最高值为顶点。

SVG图表中需要包含图表标题（显示公司名称和时间范围）、数据来源标注等必要信息，这些文字应该优雅地放置在图表顶部或底部。同时需要包含完整的坐标轴系统，包括横轴显示时间刻度（标注年份和月份），纵轴显示市值刻度（标注具体数值和单位），以及适当的网格线以提高可读性。

图表中必须在每个月的数据点位置使用圆点标记，并在关键月份的数据点附近标注该月的具体真实市值数值（从搜索数据中获取）。这些数值标注应该合理布局，避免重叠。

这个SVG图表需要包含两条折线，两条线都必须精确穿过每个月的数据点：

第一条是完整的市值曲线，连接所有月份的数据点，它应该一直显示在画面中作为底层；

第二条是高亮折线，使用对应公司的品牌色，它完全覆盖在第一条曲线之前，使用完全相同的路径数据，但颜色更亮、明显发光，明显的更粗，通过SVG的stroke-dasharray和stroke-dashoffset的animate元素来实现逐段依次展示，每次间隔0.3秒（完整播放完，需要约3秒）。高亮折线都展示完毕后，保持0.5秒，然后重新开始改循环。

整个页面应该极简设计，不要有任何body样式、button样式或其他CSS装饰。除了SVG图表本身和一个复制按钮之外，不要有其他HTML元素。复制按钮应该放置在页面最底部，使用以下代码实现复制功能：

```javascript

async function copySVG() {

  const svgElement = document.getElementById(&#x27;Chart&#x27;);

  const svgData = new XMLSerializer().serializeToString(svgElement);

  try {

    const clipboardItem = new ClipboardItem({

      &#x27;image/svg+xml&#x27;: new Blob([svgData], { type: &#x27;image/svg+xml&#x27; }),

      &#x27;text/html&#x27;: new Blob([svgData], { type: &#x27;text/html&#x27; })

    });

    await navigator.clipboard.write([clipboardItem]);

    alert(&#x27;SVG已成功复制到剪贴板！&#x27;);

  } catch (err) {

    console.error(&#x27;复制失败:&#x27;, err);

    alert(&#x27;复制失败&#x27;);

  }

}

```

关键要求总结：页面加载时JavaScript自动生成包含真实数据和SVG原生动画的完整SVG，生成后这个SVG是完全自包含的。复制功能只需简单序列化这个已生成的SVG元素。复制出去的SVG可以独立使用，包含真实的市值数据标注和循环播放的动画效果，无需任何外部JavaScript支持。
```

## 月度总结

```
# Skill description
Skill Name：monthly-review
Skill description：这个 Skill 用来帮助我完成月度复盘工作。当我要求月度复盘并提供具体的月份时，比如 2025-12，应当使用此工作流。
# 复盘的上下文材料
1. 我的个人信息2. 月度复盘要求3. 原始素材：本月的周复盘文档，保存在 /Users/mr.h/Library/Mobile Documents/iCloud~md~obsidian/Documents/vault4life/projects/weekly-review 文件夹中，文件的命名规则为 YYYY-ww，文档中会包含本周复盘内容和下周目标4. 参照系：上月的月复盘文档，保存在 /Users/mr.h/Library/Mobile Documents/iCloud~md~obsidian/Documents/vault4life/projects/monthly-review 文件夹中，文件命名规则为 YYYY-MM，文档中会包含上月度复盘内容和下月度目标5. 北极星：年度目标文档，保存在 /Users/mr.h/Library/Mobile Documents/iCloud~md~obsidian/Documents/vault4life/projects/yearly-review 文件夹中，文件命名规则为 YYYY，文档中会包含年度复盘内容和下年度目标
# 复盘的工作流次序
1. 接收格式为 YYYY-MM 的参数作为复盘的指定月份2. 根据接收的月份参数，查找相应的年度目标文档，上月份复盘文档，和该月每周的周复盘文档（注意，年度目标文档，和上月份复盘文档可能为空）3. 根据接收的月份参数，调用脚本查询本月番茄钟数据。脚本文件路径为：/Users/mr.h/Library/Mobile Documents/iCloud~md~obsidian/Documents/vault4life/pomodoro_dashboard/monthly_report.py，脚本执行命令参考：cd &quot;/Users/mr.h/Library/Mobile Documents/iCloud~md~obsidian/Documents/vault4life/pomodoro_dashboard&quot; &amp;&amp; python3 monthly_report.py 2024-12 --compare4. 结合“我的个人信息”和“月度复盘要求”，分析查找到的资料内容5. 生成指定月份的月度复盘文档6. 生成的月度复盘文件内容格式，需严格遵守下方的“月度复盘文件格式”7. 月度复盘文件命名规则为 YYYY-MM8. 生成的月度复盘文件应当保存在 /Users/mr.h/Library/Mobile Documents/iCloud~md~obsidian/Documents/vault4life/projects/monthly-review 文件夹中
# 我的个人信息&quot;&quot;&quot;# My Context Profile
## 身份- 独立开发者 (macOS/SwiftUI)- 内容创作者 (公众号：高效人生指北 / 小红书：Mac效率周刊)- 职场人 (产品经理）
## 核心项目
### 流量日记(mpdata): - 目标: 为个人自媒体创作者提供各个平台导出数据的持久化存储和数据分析。- 阶段: MVP 版本开发阶段，已完成基础的登录、订阅、数据导入、明细查看及导出、数据概览功能，目前在优化阶段，计划尽快发布第一个版本。- 平台：macOS- 技术栈: Swift，SwiftUI, AppKit.
### 高效人生指北- 账号定位：个人博客，提供生活和工作上的个人心得- 目标：不追求流量，以文会友，希望写作的内容接地气，能真正帮助到有需要的人
### Mac 效率周刊- 账号定位：提供 iOS、iPadOS 和 macOS 的使用技巧，以及热点信息- 目标：作为自己内容输出的渠道之一；用输出来倒逼自己学习新的内容；作为 AI 工具、figma 等软件的练习场&quot;&quot;&quot;
# 月度复盘原则
1. Outcome &gt; Output: 区分“忙碌”和“产出”。重构代码不算核心产出，功能上线/修复阻塞性 Bug 才算；2. 连续性检查：对比上月目标和本月产出，明确目标完成情况；3. 系统审计: 检查我引入的新工具/新流程是否真的带来了效率提升，还是只是增加了维护成本（熵增）；4. 数据诚实: 用番茄钟数据说话。如果我觉得没时间编程，但数据通过显示我花了很多时间在“探索”，请指出这种感知偏差。
# 月度复盘文件格式
&quot;&quot;&quot;# 本月复盘
## 投入产出比（展示脚本获得的月度番茄钟数据，并总结）
## 阻碍与反思(此处不做流水账，只记录痛点)
### 独立开发（流量日记项目）
### 高效人生指北
### Mac效率周刊
## 认知升级（总结当月的阅读和学习内容）
### 阅读
### 学习
## 效率优化（总结当月的工作流优化和 AI 实践内容）
### 工作流优化
### AI实践
# 下月目标(目标设置应当准确、适当，结合上月完成情况和年度目标)
## 独立开发
### 流量日记项目
## 内容创作
### 高效人生指北
### Mac效率周刊
## 认知升级
### 学习
### 阅读
&quot;&quot;&quot;
```
