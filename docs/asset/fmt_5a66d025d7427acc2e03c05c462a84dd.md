
# 我怎么使用AI（二）

> 来源：https://agijuejin.feishu.cn/docx/YA3HdxFbcoEMPFxzAXnctUtEnRf

## 👀快看👀

> 作者：[wharton0](https://space.bilibili.com/35891473)

> 原文链接：[我怎么使用AI（二）](https://www.bilibili.com/video/BV1jipdeKEbR/?spm_id_from=333.999.0.0&vd_source=5a54577a6bc7d876f6e5df88640c8e9f)

## ⚡️摘要⚡️

> 如何使用AI来解决一个猜测问题。通过修改Flask路由和JavaScript代码，用户在回答已回答过的问题时仍然可以看到图表和分数,但分数不会更新。同时，使用`set interval`函数实现了逐字打印GPT4输出的效果。通过修改`fetch10`函数和添加CSS样式,实现了GP4输出立即显示并隐藏GP4输出框的效果。最后，使用matter prose lab绘制了柱状图,比较用户的对数损失和平均对数损失。

## 🤖分集🤖

### 我怎么使用AI（二）

介绍利用 AI 解决猜测问题。通过修改 Flask 代码和使用特定函数实现 GPT4 输出效果及隐藏输出框，用 matter prose lab 绘制柱状图比较损失。实现了 AI 与 Flask 网页通信并解决已答问题不更新分数问题。

![](img/30b9378e3aa092d5a517fe331b5e571f.png)

[我怎么使用AI（二）.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Znp0bJxiRoRtoCxP3aLcAUr6nbd)

## ✏️课代表✏️

### 一、与 AI 交互及问题处理

介绍了如何使用 Flask 网页与 AI 进行交互，确保用户回答过问题后不再次触发 `alert`，继续显示图表并说明已回答，且不更新分数。讨论了修改 Flask 路由和 JS 代码，以在用户提交已答问题时显示图表和消息而非警告，还解释了在 Flask 路由和 JS 函数中处理用户回答及已答标志，显示图表和分数但不更新。

### 二、实现特定效果

说明了如何实现打字机效果，逐字打印 GPT-4 输出并设置输入速度。讨论了将 `question.js` 移动到 `static` 目录并在 `question.html` 中使用 `url_for` 函数包含该文件，以及在 `question.js` 中使用 `setInterval` 和 `setTimeout` 实现特定显示效果和延迟显示消息与图表。

### 三、代码重构与功能扩展

讨论了重构代码以实现绘制分数直方图功能，在 `score` 和 `final` 页面使用该函数，在 `final` 页面显示用户平均对数损失及比较直方图，解决 `TypeError` 和 `ValueError` 错误，使用 KDE 绘制平滑曲线，限制滑块范围并避免除以零错误，从 JSON 配置文件加载问题和回答并使用模板引擎渲染页面。

### 四、图表绘制与数据处理

解释了使用 KDE 计算百分位排名并更新函数，使用 `charge.js` 和 `C3.js` 库绘制交互式图表和线图，解决线条垂直起始问题，在 HTML 页面使用 `map` 函数处理数据，在 iOS 设备触发 `window unload` 事件及设置图表阴影颜色。

### 五、其他功能与总结

讨论了使用 Python 基层模块读取计算文件并加载数据到命名元组，定义默认值处理缺失数据，将用户百分位数转换为字母等级，在 Flask 路由中使用用户 ID 作为参数，使用盘算函数及在 HTML 中用滑块交互，最后总结了通过 URL 分享特定用户 `final` 页面链接及实现重复步骤功能。
