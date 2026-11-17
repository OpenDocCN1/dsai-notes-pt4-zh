
# 如何零成本生成一本 AI 中英对照翻译的电子书（2023 版）

> 来源：[https://jalicvoygc0.feishu.cn/docx/N4GqdmI9Voh3AUxBLHzc9dZ1nAh](https://jalicvoygc0.feishu.cn/docx/N4GqdmI9Voh3AUxBLHzc9dZ1nAh)

**工作场景：**文本翻译对照

**引用：**https://mp.weixin.qq.com/s/a7AsD67PBovW2_BSsRZCWw

**作者：** 范冰FANBING

**节选：**AI Flow Genius

---

最近发现美亚那边陆续出了不少新书，有的引进遥遥无期，有的国内无从获得。我的阅读进度已经落后了。

比如去年初就发售的《The Founders: The Story of Paypal and the Entrepreneurs Who Shaped Silicon Valley》，据说比当年《支付战争》更一手、更精彩。

我就没听说国内哪家出版社有引入计划，只好自己买了 Kindle 电子版，导出到各种阅读设备上看。

![](img/0b26d3a4e813393f6e5dd5e63f2f4a67.png)

我读书通常会想各种方法来加速提效、提升吸收率。之前也做过各种分享，比如：

+ 《我如何用打游戏的思维读书+做笔记（2小时直播视频+图文回顾）》
+ 《我如何用 Claude.AI 三分钟读完一本英语书》
+ [《我的信息分级管理规则》](http://mp.weixin.qq.com/s?__biz=MzA4NDk5OTgzMg==&mid=2650590456&idx=1&sn=c2496d5b81d301f6e0ea86c0e9c9c389&chksm=87d6c70fb0a14e19a50104563111d27a7a0252a9ca9664c30c47a80127085e8afeeeae46852c&scene=21#wechat_redirect)
+ [《分享我的信息筛选管道》](http://mp.weixin.qq.com/s?__biz=MzA4NDk5OTgzMg==&mid=2650590272&idx=1&sn=775f5caee66737899ebbde4b67e8c9df&chksm=87d6c7b7b0a14ea1852ee4d95003693a187d1658e12dab9c2636b553e8dc706c1e0ee7eb356f&scene=21#wechat_redirect)
+ 《一年阅读 196 本：我的高效阅读技巧 20 条》
+ 更多私下日常的经验窍门、心得感悟，可以添加我的🛰️，围观朋友圈：

![](img/16f7950c44dd3b69c3025827b9ff202f.png)

读外版书，一个常用的技巧是，我会将它处理成对照翻译（一段英文、一段中翻）版本，制造沉浸式阅读环境（而不是在两本书/介质/App 之间来回跳跃，或者干脆放弃原版的精髓）。

前几年 ChatGPT 还没出来那会儿，我还得自己靠撸代码写脚本来白嫖 DeepL 的算力，来生成中英对照。

![](img/2ed03831d7ff6790ff138e55b1829d3e.png)

最近代码年久失效，再加 ChatGPT 为代表的大语言模型将翻译能力又带上了新的台阶，所以我……打消了自己重新发明轮子的想法，还是打算找个现成的。

经过对操作便捷度、质量可控性、翻译引擎可配置性等多方对比，我找了现在的解决方案。

先上最终效果：

![](img/09f0e2a4abd2e096b38adf2ac4dfd020.png)

怎么做到的呢？

1、使用「沉浸式翻译（Immersive Translate）」这个工具，它本身提供 iOS 和 Android 版，但这里我们用到的是 Chrome 插件版。

获取方式：https://immersivetranslate.com/

你可以从这个官网首页，跳转去 Chrome 应用商店，给你的 Chrome 浏览器安装「沉浸式翻译（Immersive Translate）」这个插件。一切主要就是靠它实现（不知道怎么给 Chrome 装插件的请自行搜索）。

![](img/c328cbb962cf5b63d850fc77de327981.png)

2、从菜单栏点击沉浸式翻译的 icon，从弹出菜单中，再点击右下角的「更多」下拉菜单，会出现「制作双语 EPUB 电子书」的选项。点选它。

![](img/48e92d36fae95b654190397b3e647ade.png)

3、Drop a book Here! 拖拽一本 EPUB 电子书，或者点 choose a file 选中一个本地文件。

![](img/7f55234fbab9885c575029986c0cec0a.png)

4、选择的 EPUB 电子书文件如果无误，会出现如下的编辑界面：

![](img/3bf62c1a87b31f848ed2b0aae44f21e5.png)

往下拉一拉，你会看到这本书里的原版文字内容，已经被解析抽离出来，嗷嗷待翻了。

![](img/fd09010fe9b9bdd74336823bf5cab4d6.png)

5、在此界面下，点击浏览器菜单栏的沉浸式翻译 icon，选择原文语言（推荐「自动检测语言」），目标语言选「简体中文」，翻译服务可以从 OpenAI / 微软翻译 / 谷歌翻译 / 腾讯交互翻译 / 有道翻译 / 彩云小译等 13 个翻译引擎里选择一个（我的性价比之选是腾讯交互翻译，免费），其他默认。

设置完成后，点击「翻译」按钮，于是它进入「翻译中」状态。

![](img/71d5ca86477273af35e6af38f7bc9bb1.png)

6、过一会儿，你会发现编辑界面里的英文，已经变成了中英对照。

![](img/05e6b5b6f9595efa87853ebb6738976d.png)

7、点击顶部「导出」按钮，选一个成品的保存位置。

![](img/6976c653a2b58e13e6d72b1ae1777963.png)

8、完成！（目录页并没有翻译）

![](img/f1ee0659d65925567c971bd208f8f3c2.png)

![](img/29505bc87b6afa14a798845fa3483b1b.png)

Enjoy it!

## 继续浏览全站内容

[🎪首页](https://jalicvoygc0.feishu.cn/wiki/DdEvwxYsSidd36kAIIVcnROwnmb)

[🤔学习AI工作流](https://jalicvoygc0.feishu.cn/wiki/W15IwochvimsqvkkuCPcZuBxnqg)

[🤖查找AI工具](https://jalicvoygc0.feishu.cn/wiki/Bu8GwqYu8i5LBRkyVKocUvX5n97)

[📊阅读AI文章](https://jalicvoygc0.feishu.cn/wiki/SIywwqrVji2lZkkqFFpcEHZ8nNc)

扫码加入AI Flow Genius社群，探讨使用AI来提升生产力的方法，成为超级个体

超过200需手动拉人 **备注：加群**

![](img/9f04d4c462f231384a3fe86971f026f6.png)
