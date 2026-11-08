# 四、ChatGPT高级技巧

> 来源：[https://bwiny5w2svt.feishu.cn/docx/PBBbdpkBOoaWnyx1k6rcUKBJn3g](https://bwiny5w2svt.feishu.cn/docx/PBBbdpkBOoaWnyx1k6rcUKBJn3g)

> 打造最NB的ChatGPT资料库：

> 此文档每日更新，加入免费社群（见底部🛰️）获取每日更新资讯，如有帮助请**收藏并转发**

## ChatGPT图片内容识别

### 识别UI，生成前端代码

给chatGPT一张IU设计稿，它能识别里面的内容。

刚开始的回答效果还不是很好，后面通过修改的效果还是不错的。

![](img/3f1ed92a9a49a58605962dcf30a9543b.png)

左边是UI设计稿，右侧是代码最终成生的样子。

![](img/fe41ed28aa1bfcdb7f0cd417db53924f.png)

### 识别动物描述场景

通过这张图片，其实就很能说明，AI识别图片的能力已经很强了。描述能力也很不错。

![](img/b601e1add63aa89205e08c656f4daff8.png)

![](img/3ce606b69165676cf9baa07676dcf651.png)

## ChatGPT生成图片

魔法提示词：

> 从现在起, [当你想发送一张照片时，请使用 Mar](https://link.zhihu.com/?target=https%3A//source.unsplash.com/1280x720/%3F)kdown ,并且 不要有反斜线, 不要用代码块。使用 Unsplash API (https://source.unsplash.com/1280x720/? < PUT YOUR QUERY HERE >)。如果你明白了，请回复“明白”

原理解析:

Unsplash API 是一个基于 REST 的 API，它提供了丰富的图像数据和功能。在这里，通过使用 Unsplash API，这就可以让Chatgpt可以通过编程方式搜索、浏览和下载 Unsplash 平台上的图像，从而实现在聊天对话中的预览。

说明：

如果单独打开 https://source.unsplash.com/1280x720/?surfing 这个网址会发现，每次刷新都在变。生成的图片尺寸非常大。

![](img/745d00108dfd65f1697879e2d36d9155.png)

## 生成Emoji表情包、SVG图

复杂描述的emoji也难不倒它

![](img/fa293040050a9cb3db075d1d1413798a.png)

emoji表情包也是chatGPT通过网上搜索出来的，并不是它自己生成。一家5口人的icon就无法很好的展现。

![](img/86c43a6415afe608ff32c0d5dce7a83c.png)

## 数据查询+格式化+图表生成一步到位

> ChatGPT居然还能一键生成各种统计图表，Excel不香了

**日常痛点分析：**

我们在写一些报告时，经常需要去网上查找一些数据，然后又需要对数据进行格式化处理。

然后又要把格式化的数据转换成图表。步骤有点麻烦。

能不能把这些步骤聚合在一起，一步到位呢？

答案是：可以的。

利用ChatGPT强大的数据查询能力，数据加工，再利用一些第三方可视化工具，可以快速生成各种表图。

**QuickChart图表库介绍**

[下面我们介绍一种图表](https://quickchart.io/)生成的简单库： QuickChart。

QuickChart 是一个图表生成库，它可以帮助用户快速、简便地创建图表。以下是一些 QuickChart 的主要特点：

1.  基于 Chart.js：QuickChart 基于流行的开源库 Chart.js，因此具有很多现成的图表类型和选项，如折线图、柱状图、饼图等。

1.  简洁的 API：QuickChart 提供了一个简洁的 API，通过发送 HTTP 请求，可以轻松地生成和定制图表。

1.  URL 图表生成：QuickChart 可以通过 URL 参数快速创建图表，这使得在电子邮件、报告或其他需要插入图像的场景中非常方便。

1.  自定义样式：QuickChart 允许用户自定义图表的样式，例如颜色、字体、轴标签等，以满足不同的设计需求。

1.  导出图表：QuickChart 支持多种图像格式，如 PNG、JPEG 和 SVG，方便用户下载和分享。

1.  开源和私有部署：QuickChart 是一个开源项目，可以在本地或私有服务器上部署，确保数据安全和隐私。

总的来说，QuickChart 是一个功能强大、易于使用的图表生成库，适用于各种场景和需求。

**利用QuickChart+****ChatGPT****生成各类图表**

**第一步：设置身份**

提示词

> 你是一个图表生成器，会生成基于QuickChart的图片。

> 我会给你一些数据或者提示词和一个图形类型，你按要求生成。

> 注意生成的url中特殊符号需要格式化。

**第二步：查询相应的数据**

2019年，中国各省份常住人口数，生成一个柱状图。

![](img/6a68d545457e6209aeea63d639011fde.png)

![](img/8b6d224b9310e5310494f2fca8987a3c.png)

打开链接后的效果，就是一张柱状图片

![](img/0370827af59c108ee978b6417df9acd7.png)

## 浏览器插件推荐

### ChatGPT终于能联网了

> 副标题：封印解除！WebChatGPT让你的ChatGPT无需联网，畅通无阻！

**背景说明：**

ChatGPT最大的问题就是不能联网，数据只能查询到2021年。导致在查询一些时效[的问题，或者最新技术](https://chrome.google.com/webstore/detail/webchatgpt-chatgpt-with-i/lpfemeioodjbpieminkklglpmhlngfcn/related?t=http://webstore.google.com)时容易出现问题。现在终于有办法解决了。通过Chrome插件：WebChatGPT实现。

**测试实践：**

安装完插件后，你的输入框会变成这样。

![](img/f2e875607ac75f00a3074456aca26873.png)

**插件开启前：2**021年后的内容获取不到

![](img/a1b1b3815086f0676c152ad0be3a2cbe.png)

**插件开启后**：可以获取最新的数据。

**插件原理解析：**

1.  把消息发送给ChatGPT

1.  消息被插件拦截，把这个问题去搜索引擎上搜索，并返回相关的几条内容和url来源。（返回数量可以通过插件设置）

1.  在搜索引擎中搜索完成后，插件会把结果返回回来，并带上你的问题和插件内部提前设置好的提示词一起发送给ChatGPT。说明一下：最后发送给ChatGPT的模板，可以在插件中进行设置，方便你修改提供词和格式。

1.  ChatGTP根据这些最新资料+你的问题，可以返回一个实效时+精准度更高的答案

![](img/ebce95b77b6372293b01b549df416730.png)

![](img/7acb7a54081d951e04a254f000aec768.png)

**注意事项：**

虽然这个号称可以让chatGPT联网，但其实只是一种中转方法。ChatGPT本身是不能读取到网站内容的。

比如下面这个案例：

![](img/d1f94ca7f5cf7af011b5d9b8ef9f5135.png)

而且就算回答，它也经常胡说八道。

下面链接的内容其实是：什么是ChatGPT ApiKeys，如何获取，有哪些注意事项？

![](img/40bc1fedbcb71dac5c496ec7ac3bed35.png)

##

### 一键截屏ChatGPT完整对话

在写ChatGPT超神之路文档时，需要经常截取ChatGPT的对话框内容。尝试过很多插件都失败了。

现在终于发现了一款还不错的插件：ChatGPT Screenshot。

大部分场景下还是不错的，但是有个几个小问题

+   【截图不全】在截取代码时，代码块的内容会显示白屏，需要在PS中进行拼接，有点麻烦。

+   【留白太多】由于截取的整个网页，图片两边的留白太大，导致插入在文章中字体很小，而且图片太大加载慢。

具体怎么解决了？可以尝试使用第三方增强版本的镜像站、或者客户端。看我这篇文章：如何拥有更强大的ChatGPT服务？

### 把ChatGPT翻译接入浏览器 OpenAI Translator

插件：https://chrome.google.com/webstore/detail/openai-translator/ogjibjphoadhljaoicdnjnmgokohngcc?t=http://webstore.google.com

### 1分钟获取Youtube长视频的核心内容点

副标题：学习新方法！用ChatGPT快速锁定Youtube视频核心知识，一分钟搞定！

**请大家思考一个问题：**

如何快速获取YouTube视频中的核心知识点？

是通过快进播放或倍速播放吗？不是！这些方法效率还不够高。

视频效率低下有两个原因：阅读效率和全局视角。

与文字阅读相比，观看视频时我们的阅读效率较低；而与文本阅读相比，观看视频时我们缺乏全局视角。

因为视频只提供了一个个切面，无法像文本一样通过结构化的文字总结来快速获取核心知识点。

因此，我们需要一种工具来提取视频中的所有文本，并总结核心知识点。

如果您对某个核心知识点感兴趣，只需点击即可跳转到视频指定位置。

当观看视频时，如果想快速预览后面的内容，则可以快速定位到文本位置并进行预警。这样效率会更高。

而且使用ChatGPT还可以在任何时间对视频中的任何部分进行提问，真是太强大了！

它是一个非常实用的家庭和学习工具。

下面我将带大家来学习这种高效的方法。

**第一步、安装chrome插件**

youtube-summary-with-chat

核心功能：

1.  字幕文本显示：能够将YouTube的字幕和时间一一对应，点击相应的文本即可直接跳转到视频指定位置。

1.  使用ChatGPT生成概览：点击按钮即可将所有字幕带入ChatGPT网站进行下一步提问。

1.  做笔记：可以截取当前页面画面并添加笔记。

**第二步：配置apikeys**

如何配置，请看这篇文章： 什么是ChatGPT ApiKeys，如何获取，有哪些注意事项？

![](img/4f0fdc9b6cc2c93e06c79e6fd05ad190.png)

第三步：开始在Youtube上使用

下面是一个 TED 上 AI 教育的视频：[T](https://www.youtube.com/watch?v=hJP5GqnTrNo&ab_channel=TED)he Amazing AI Super Tutor for Students and Teachers | Sal Khan | TED，我们用它来做测试。

由于视频默认是中文的，我开启Google翻译插件，所以现在显示的中文。

**模块1：字幕模块**

视频字幕提取功能，点击时间可直接跳转到视频指定位置

![](img/65a61f7c226eb581c54f3d2fde99d7b0.png)

拷贝出来的完整字幕，没有分段，看起来有点难受。

![](img/e86bec744d7da7bd82d9834dfade2776.png)

**注意事项：**

由于字幕需要创作者提前上传，否则会提示无法转录的情况。

![](img/91a12b98629041aef394d55bbac6deff.png)

**模块2：概览模块**

包含四个功能点

+   重新生成概览按钮

+   复制概览文本：默认复制出来的是原版的内容，这里是英文

+   跳转到ChatGPT网站

![](img/8cf576cd62b6c83e1d135a3c69312ec7.png)

点击跳转到chatGPT页面后，可以对视频中的内容进行提问。并且按任意格式整理输出概览。

![](img/05257ecaa5ff9564d12364a10d49f990.png)

**模块3：笔记模块**

+   可以直接截取当前做笔记的屏幕和时间点（下次可跳转到视频位置）

+   可对笔记进行编辑：删除，编辑，添加标签

![](img/4e02348a96734412c9f8a0c74992deb6.png)

### 把Youtube视频快速生成一篇原创的🛰️👸号文章

**痛点说明：**

最近吴恩达的ChatGPT提示词的视频教程（[C](https://www.youtube.com/watch?v=MoIeNKW9UAY)hatGPT Prompt Engineering for Developers ChatGPT开发人员提示工程）非常火爆，但由于视频是 纯英文+技术术语多+内容长（有一个半小时）很多人都是听过没看过。下面我将给大家提供一种快速获取核心知识点的方法。这种方法可以快速把一些最新、最热门的优秀教程转换成一篇篇优秀的原创👸号文章。

**解决方案：**

使用Chrome插件：youtube-summary-with-chat

（插件的其他使用技巧及详细介绍：学习新方法！用ChatGPT快速锁定油管视频核心知识，一分钟搞定！）

**开始行动：**

1.  **点击复制出视频中所有的字幕文本**

![](img/0a6fa88c68b7f051cdd7b685788863e6.png)

1.  **把字幕文本格式化处理**

**第一：文本太长**

这个视频一共有大约 7.7w个字符，哪怕是chatGPT4也无法一次性处理这么长的文本。所以我们需要解决两个问题。

**A****. ****ChatGPT****无法一次性接收超长文本的问题**

> 解决方案：分段发送

GPT4最多也只能一次性接收 2万5千token（大概50页英文文字长度），我实际测试下来发现一次最多能输入12663个字符。所以需要把这个视频最少拆分成7次发送。

**B. ****ChatGPT**无法处理如些庞大的上下文数量的问题

> 解决方案：分段总结

ChatGPT 可以处理的上下文token 长度默认为8k，但最长可达32K（大约50 页文本）。

ChatGPT可以处理的上下文token 长度默认为8k，但最长可达32K（大约50 页文本）。

> 解决方案：给chatGTP设定好关键词后，并手动分割文本投喂给ChatGPT。

第二：导出的字幕文本，没有一个标点符号

> 解决方案：ChatGPT的理解能力比较强，这里不做特殊处理。

文本总长度：7.7w字。

![](img/f2ad3bcaa0d713065ec819206d56bb5f.png)

随意分割，让ChatGPT自己组装

![](img/53c99be17842a547faeb3f3c7f895a38.png)

1.  给ChatGPT设定身份及目标

> 你是一位专业的🛰️👸号博主。

> 你的写作风格：

1.  简洁明了：为了吸引读者的注意力，常常会选择简洁明了的语言和文字，吸引读者的眼球。

1.  生动形象：选择生动的语言和形象的描述，以吸引读者的兴趣。

1.  富有个性：展示出自己的独特见解，与众不同的态度和观点，增强其个人魅力。

1.  关注热点：关注时事热点，抓住读者的关注点，提高文章的阅读量。

1.  富有启发性：给读者一些独特的见解，启发读者思考，增强其价值。

>

> 你的任务：按照我提供的资料，写一篇🛰️👸号文章。

> 注意事项：由于我给你的文本过长，我会分段发送给你。直到我输出【资料录入完成，请输出文章】你才开始输出内容。否则只要回答【资料已收到】。

内容完善中...

### 随时调用ChatGPT服务进行翻译，润色文本

chrome插件：OpenAI Translator

核心功能（其实就是内置了一些提示词）：翻译、润色文本、总结、分析、代码解释

![](img/032c27ad33ce69df431fe299558dae9e.png)

插件配置方法

![](img/c3f9e12629fe1a9eaf67c770ec8d58e5.png)

## ChatGPT的为什么健忘？如何处理

核心原因：上下文token长度有限制，免费版本2048个。付费版本plus4.0可以达到8192个。

如何估算文本占用的token数量：https://gpttools.com/estimator

特别说明：

解决方案：

https://cloud.tencent.com/developer/news/1024774

## ChatGPT增强客户端推荐

**ChatGPT****官网使用痛点说明：**

+   【自动掉线太难受】ChatGPT官网每隔一段时间就会自动掉线，需要刷新，让人非常难受。即使我是plus用户。虽然使用了 KeepChatGPT 这个 chrome插件来解决，但是发现没有彻底解决。

+   【网页版本功能太简陋】由于我要写ChatGPT的文档，需要经常的截图，虽然已经通过安装一个插件来解决这个问题，但是还是有几个功能点不好用。

1.  截图中代码块会丢失，需要在ps中手动拼接

1.  截图两边留白太大，需要在ps中单独处理裁剪掉

1.  无用的对话不能隐藏，不方便在文章中展示，又需要在ps中处理

**理想中需要添加的功能点如下：**

+   【对话截图导出】能够把聊天对话，一键导出图片。

+   【删除指定对话】每条对话都可以删除，方便截图时的冗余信息。

+   【添加提示词】可存储提示词，并支持快捷查询。

+   【apikeys流量查询】可以方便的查看流量使用情况。

+   【支持选择接口模型】其实这个还好，反正大部分基本都没有4.0的接口。

**其他加分项**

+   UI设计好

+   交互设计好

**推荐几个客户端工具：**

1.  [ChatGPT-Desktop](https://github.com/Synaptrix/ChatGPT-Desktop)  【推荐】

上面提到的几个痛点都解决了，而且还支持几个不错的功能

+   【生成图片】虽然接口是3.5版本的，但是支持生成图片。应该是内置了提示词，让刚接触的朋友很有新鲜感。

+   【界面非常简洁】默认连左侧历史记录都没有，但是可以通过历史按钮打开。

+   【支持快捷键一键显示/隐藏】更方便

除了不能查询【apikeys流量】其他体验都做的不错。尤其是UI是我目前见过最简洁的。历史栏默认隐藏，可点击历史按钮显示，并且支持快捷 option+x 一键显示、隐藏。

**其他的不足**：不能切换api模型，只能使用默认的 3.5-turbo-0301版本。

![](img/0f54040f1a2c2b526180e011a1959fe5.png)

![](img/36fb6a3c2c9e50a0e1410d2d975f4cc8.png)

![](img/bb8e7eecf37f6d2e0355e248efe5587b.png)

![](img/14f0c5e3aba0e9016560fb17f1b8cdce.png)

2.[ ChatGPT](https://github.com/lencx/ChatGPT)

高仿官网，但是我的m1没安装成功。其他朋友可以试试，看功能好像还不错。也是目前github上star最高的chatGPT客户端。

![](img/8ad95eb68eb3aa8714200e9b10b02012.png)

1.  [chatBox](https://github.com/Bin-Huang/chatbox)

有客户端也有[网页版本](https://web.chatboxai.app/)。配置apikey就可以使用。没啥广告，界面比较清爽。也支持添加自己的提示词。

![](img/353c0c5e81b9cfc11bf3ff207f60637b.png)

![](img/90b954838e9e66211d81fec345f8cebb.png)

## AutoGPT介绍与使用

chatGPT痛点说明：

+   不能一步到位的解决问题：比如我要出一本ChatGPT相关的书籍，它只会先出个目录。然后你要一个个主题不断的去问它。最后还要把这些内容拼装好生成一本电子书。

+   不能上网：虽然通过插件可以实现部分上网功能，但是这种功能有限制，只会查询前几条。

+   不能创建本地文件：chatGPT只是一个网页，不具备操作本地文件的功能。所以无法提供可供下载的文件，也无法创建本地文件夹。

AutoGPT 项目地址：

https://github.com/Significant-Gravitas/Auto-GPT

AutoGPT核心功能：

+   会把一个大目标分解成一个个小目标：然后一步步执行。

+   可上网搜索内容

+   可操作本地文件：创建文件等

AutoGPT的缺点就是门槛比较高，对于没有编程经验的人安装比较麻烦。它没有图形界面，所有操作都在终端中进行。最新0.3版本有个比较大的更新，GUI界面可以添写邮箱加入内测白名单。申请地址：https://news.agpt.co/

![](img/decb7c54d469b0eccd30b0565900bb9f.png)

**使用过程**

**一、本地安装**

准备工作：

+   电脑安装：git， 并把项目clone到了本地

+   电脑安装python: 安装python（以mac电脑为例） ： https://www.python.org/downloads/macos/

+   openAI的apikeys：确保里面有额度并且没过期

特殊说明：

> 要将 OpenAI API** 与 A**uto-GPT 结合使用，[我们](https://platform.openai.com/docs/guides/rate-limits/overview#:~:text=Free%20trial%20users,RPM%0A40%2C000%20TPM)强烈建议设置计费 （即付费帐户）。免费帐户限制为每分钟 3 次 API 调用，这可能会导致应用程序崩溃。

官方详细的文档地址：https://docs.agpt.co/setup/

**二、本地运行**

## ChatGPT4.0 最新版本原生支持联网

今天我的ChatGPT终于获得了联网功能推送。下面我们来测试一下。联网功能效果到底怎么样？如何更好的帮我们解决问题。

**联网功能原理分析：**

**问题一：查询一个时效性的问题**

> 周杰伦最近一次演唱会在哪里举办的?

**问题二：深入分析与总结**

> 总结chatGPT近3个月最值得关注的大事件

**问题三：网页内容整理分析**

下面我让他分析了一下，我在掘金发布的一篇文章：

ChatGPT实战技巧：保姆式教程GPT+xmind做炫酷PPT

![](img/865907c298315654cf98adada5b74ba4.png)

ChatGPT联网功能的局限性：

一、**不是所有网页都能读取**：

下面给GPT一个知乎专栏上的链接，让它分析里面的内容。但是报错了。原因是：知乎的 robots.txt 文件不允许访问，所以GPT为了遵循这种君子协议不去读取了。

![](img/0ca464c779e5610d3f6932b9b55a9917.png)

## 不用ChatGPT API怎么搭建一个本地GPT镜像

**痛点说明：**

ChatGPT需要开全局代理才能访问，而博主经常要在本地调试代码。经常在开启、关闭全局代理切换，非常麻烦。我们也可以通过配置gpt apikey的方式来使用。但是gpt api是需要收费的，每次用的时候感觉有点心疼呀。

那有没有不用apikey，并且不用开全局代理的方法呢？其实是用的，下面我将介绍这种方式。

## 小白如何快速搭建一个自己的镜像站

使用的开源项目：https://github.com/Yidadaa/ChatGPT-Next-Web

下面这个地址，是我搭建的ChatGPT镜像站。在部署时我内置了一个apikey，第一次进入页面后可以通过输入密码的形式直接使用的内置的key，也可以重新输入一个新的apikey。

我设置的镜像站url：https://chat-gpt-next-web3-two-rho.vercel.app/#/chat

密码：123456

![](img/a0b9d29f511e3c2e9e4e341246e7a410.png)

核心要注意，在部署的时候要写入这两个参数

![](img/b4f95d5d5e274d5988e8d39a18ceac37.png)

## 非GPT Plus用户怎么享受联网功能？

![](img/b52f152ba6b371a3981dfe634ef2a0cd.png)

可以使用这个开源项目：https://github.com/binary-husky/gpt_academic

在线演示：https://huggingface.co/spaces/qingxu98/gpt-academic

![](img/36dd18a605605680e5229ad073e44a16.png)

最后联网查询的信息正确

![](img/77d52972d3fd33b3644e5216236dfa4d.png)

复杂一点的分析问题，效果也不错

![](img/aeed67ee4cb51c5d7221f46b42c4058e.png)

## ChatGPT 插件plugin 推荐

### 一、Al Video Summarizer

介绍：AI视频摘要生成器。

视频地址：https://www.youtube.com/watch?v=SAV6lhwQs9w&ab_channel=%E5%B0%8F%E9%A9%AC%E5%93%A5%E5%9C%A8%E5%8F%91%E5%91%86

虽然视频是中文的，但是插件默认以英文的格式输出。

![](img/fc08115fc1da2ec0851f969513f0d0bd.png)

让chatGPT重新以中文再次输出

![](img/54bd97915b14cb5927caa1dd9054728c.png)

你可以进行更深一步的使用，对视频中的内容进行提问、把视频文本转换成一篇文章/📕/朋友圈文案等，生成摘要等。可以结合chatGPT action 调用多个功能进行处理，一键把youtube视频转成特定风格的👸号文章。

### 二、 AskYourPDF

支持上传pdf或者pdf 链接可以读取pdf中的内容，并进行内容读取、分析。

原始链接：https://pdf.dfcfw.com/pdf/H3_AP202008261401662530_1.pdf

![](img/040801dfc086b4dfb7f314341c893b2d.png)

### 三、Zapier

可以通过Zapier连接5000多款应用

** - ****ChatGPT****让每个普通人，不再平庸 -**

链接博主+免费AI社群+商务合作                                                      👸号：AI超体进化室

![](img/dbdf07c134f785f0ec5b860782ff28a4.png)

![](img/61e1e2dd5c7f3e1cd6f25c46d26d0c93.png)