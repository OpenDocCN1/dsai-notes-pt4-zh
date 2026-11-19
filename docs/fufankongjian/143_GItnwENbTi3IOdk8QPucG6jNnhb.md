
# 各类大模型模型编程问题测试【提示词模板】

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/OmefdSscLol4eNxncAEcljZynUc](https://kq4b3vgg5b.feishu.cn/docx/OmefdSscLol4eNxncAEcljZynUc)

## ✅ 1. 小球在旋转多边形中弹跳（动画 + 物理模拟）

### 提示词：

> 请生成一个完整的 HTML 文件（将 HTML、CSS 和 JavaScript 均嵌入单一文件中），模拟一个红色小球在顺时针缓慢旋转的正五边形内部弹跳的动画。

> 要求：

+ 小球应受重力影响，并在碰到边界时发生反弹；
+ 小球与多边形之间的碰撞检测要真实；
+ 所有代码应包含在 `<html>` 文件内，**不要引用外部库或文件**；
+ 动画要平滑，页面布局适配不同屏幕。

### DeepSeek R1编程效果：

![](img/8919a3b0d84dfb1ecfb0b597d556d86f.png)

[2025-05-28 19-55-40.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KLEWbk9mwowRl3xMA1ycGDNfnKh)

## ✅ 2. 生命游戏（细胞自动机）

### 提示词：

> 请创建一个纯 HTML 文件（含嵌入 CSS 和 JS），用于展示“康威生命游戏”的动态演化。要求包括：

+ 初始状态可为随机生成；
+ 添加“开始/暂停”、“清除”、“生成新随机初始状态”按钮；
+ 每一代自动过渡，动画平滑；
+ 不引用任何外部库，确保用户可以保存为 `.html` 本地运行。

### DeepSeek R1编程效果：

![](img/b600913b4d6380bcb5ff11f6341652c9.png)

[2025-05-28 20-11-02.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Kyl3bCVneodzCrxlXAScCD0inMd)

---

## ✅ 3. 简易迷宫逃脱游戏（键盘控制）

### 提示词：

> 请创建一个完整 HTML 页面，实现一个迷宫逃脱小游戏：

+ 在 20x20 的迷宫中，玩家可以使用方向键控制蓝色小球移动；
+ 红色方块为终点，到达后显示“胜利！”提示；
+ 提供“生成新地图”按钮；
+ 不使用外部库，所有脚本与样式嵌入页面；
+ 迷宫由递归分割或回溯法生成，确保可达。

### DeepSeek R1编程效果：

![](img/ed4d509a4a3eb4412c591e69901beb7f.png)

[2025-05-28 20-35-23.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JznEb9qwaow724xbHGKcRNFunsh)

---

## ✅ 4. 数据可视化仪表盘（图表 + 动态数据）

### 提示词：

> 请编写一个完整的 HTML 文件（含嵌入 CSS 和 JavaScript），创建一个可视化仪表盘页面：

+ 使用随机生成的数据展示 3 个区域图表（柱状图、折线图、饼图）；
+ 添加按钮“刷新数据”，点击后更新图表；
+ 页面布局响应式，卡片式样式，整体美观简洁；
+ 所有代码都应包含在页面内，适合保存为 `.html` 本地打开。

### DeepSeek R1编程效果：

![](img/7164dd3ca416a0c0d8f8af8ecedd7208.png)

[2025-05-28 20-24-01.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WEs1b6y0woiL5Px27TxcYFExnnb)

## ✅ 5：响应式企业官网首页

### 提示词：

> 请使用 HTML、CSS 和 JavaScript 创建一个响应式的企业官网首页。页面应包括：

+ 顶部导航栏，包含公司 Logo 和导航链接，导航栏在滚动时固定在顶部；
+ 全屏轮播图，自动播放，并带有左右切换按钮；
+ 公司简介部分，包含标题、文本描述和配图；
+ 服务项目展示区域，使用三列布局，每列展示一个服务项目，包含图标、标题和简短描述；
+ 客户评价部分，展示三条客户评价，包含客户头像、姓名、评价内容；
+ 底部信息区，包含联系信息、社交媒体链接和版权信息。

要求页面在不同屏幕尺寸下自适应布局，使用 CSS Flexbox 或 Grid 实现响应式设计，整体风格简洁现代，配色以蓝色和白色为主。

### DeepSeek R1编程效果：

![](img/78e4cfeb981bd80d07d78fd7e08e3e5b.png)

[2025-05-28 20-04-59.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KOBvbGEFoo28krx8Dc8cGqr7n7f)

---

## ✅ 6：电商产品详情页

### 提示词：

> 请使用 HTML、CSS 和 JavaScript 创建一个电商网站的产品详情页，包含以下元素：

+ 产品图片展示区域，支持点击缩略图切换主图；
+ 产品标题、价格、评分（使用星级表示）、库存状态；
+ 产品选项选择，如颜色和尺寸，使用下拉菜单或按钮组实现；
+ 数量选择器，包含加减按钮；
+ “加入购物车”和“立即购买”按钮，点击后显示相应提示；
+ 产品描述和规格信息，使用可折叠面板展示；
+ 用户评价区域，展示用户头像、姓名、评分和评价内容；
+ 相关产品推荐区域，展示四个相关产品的缩略图、标题和价格。

要求页面布局清晰，交互流畅，适配移动端和桌面端，整体风格与现代电商网站一致。

### DeepSeek R1编程效果：

![](img/f88b6d4474a21762c690675d9bc8ac23.png)

[2025-05-28 20-20-59.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Nq6Pbo7fTobCSixsKfEcvKbInSg)
