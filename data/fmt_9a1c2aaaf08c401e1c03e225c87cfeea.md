
# AI 工具汇总与使用指南

## 某AI客户端体验评测

+ 【生成图片】虽然接口是 3.5 版本的，但是支持生成图片。应该是内置了提示词，让刚接触的朋友很有新鲜感。
+ 【界面非常简洁】默认连左侧历史记录都没有，但是可以通过历史按钮打开。
+ 【支持快捷键一键显示/隐藏】更方便

除了不能查询 `apikeys` 流量其他体验都做的不错。尤其是 UI 是我目前见过最简洁的。历史栏默认隐藏，可点击历史按钮显示，并且支持快捷 option+x 一键显示、隐藏。

### 其他的不足

不能切换 `api` 模型，只能使用默认的 `3.5-turbo-0301` 版本。

![](img/0f54040f1a2c2b526180e011a1959fe5.png)

![](img/36fb6a3c2c9e50a0e1410d2d975f4cc8.png)

![](img/14f0c5e3aba0e9016560fb17f1b8cdce.png)

## ChatGPT 客户端（lencx）

[ChatGPT](https://github.com/lencx/ChatGPT)

高仿官网，但是我的 m1 没安装成功。其他朋友可以试试，看功能好像还不错。也是目前 github 上 star 最高的 chatGPT 客户端。

![](img/8ad95eb68eb3aa8714200e9b10b02012.png)

## chatBox

[chatBox](https://github.com/Bin-Huang/chatbox)

有客户端也有 [网页版本](https://web.chatboxai.app/)。配置 `apikey` 就可以使用。没啥广告，界面比较清爽。也支持添加自己的提示词。

![](img/353c0c5e81b9cfc11bf3ff207f60637b.png)

![](img/90b954838e9e66211d81fec345f8cebb.png)

## AutoGPT 介绍与使用

### chatGPT 痛点说明

+ 不能一步到位的解决问题：比如我要出一本 ChatGPT 相关的书籍，它只会先出个目录。然后你要一个个主题不断的去问它。最后还要把这些内容拼装好生成一本电子书。
+ 不能上网：虽然通过插件可以实现部分上网功能，但是这种功能有限制，只会查询前几条。
+ 不能创建本地文件：chatGPT 只是一个网页，不具备操作本地文件的功能。所以无法提供可供下载的文件，也无法创建本地文件夹。

AutoGPT 项目地址：

https://github.com/Significant-Gravitas/Auto-GPT

### AutoGPT 核心功能

+ 会把一个大目标分解成一个个小目标：然后一步步执行。
+ 可上网搜索内容
+ 可操作本地文件：创建文件等

AutoGPT 的缺点就是门槛比较高，对于没有编程经验的人安装比较麻烦。它没有图形界面，所有操作都在终端中进行。最新 0.3 版本有个比较大的更新，GUI 界面可以添写邮箱加入内测白名单。申请地址：https://news.agpt.co/

![](img/decb7c54d469b0eccd30b0565900bb9f.png)

### 使用过程

#### 一、本地安装

准备工作：

+ 电脑安装：`git`，并把项目 clone 到了本地
+ 电脑安装 `python`：安装 `python`（以 mac 电脑为例）：https://www.python.org/downloads/macos/
+ openAI 的 `apikeys`：确保里面有额度并且没过期

特殊说明：

> 要将 OpenAI API 与 Auto-GPT 结合使用，[我们](https://platform.openai.com/docs/guides/rate-limits/overview#:~:text=Free%20trial%20users,RPM%0A40%2C000%20TPM)强烈建议设置计费（即付费帐户）。免费帐户限制为每分钟 3 次 API 调用，这可能会导致应用程序崩溃。

官方详细的文档地址：https://docs.agpt.co/setup/

#### 二、本地运行

## ChatGPT4.0 最新版本原生支持联网

今天我的 ChatGPT 终于获得了联网功能推送。下面我们来测试一下。联网功能效果到底怎么样？如何更好的帮我们解决问题。

### 联网功能原理分析

#### 问题一：查询一个时效性的问题

> 周杰伦最近一次演唱会在哪里举办的?

#### 问题二：深入分析与总结

> 总结 chatGPT 近 3 个月最值得关注的大事件

#### 问题三：网页内容整理分析

下面我让他分析了一下，我在掘金发布的一篇文章：

ChatGPT 实战技巧：保姆式教程 GPT+xmind 做炫酷 PPT

![](img/865907c298315654cf98adada5b74ba4.png)

ChatGPT 联网功能的局限性：

### 不是所有网页都能读取

下面给 GPT 一个知乎专栏上的链接，让它分析里面的内容。但是报错了。原因是：知乎的 `robots.txt` 文件不允许访问，所以 GPT 为了遵循这种君子协议不去读取了。

![](img/0ca464c779e5610d3f6932b9b55a9917.png)

## 不用 ChatGPT API 搭建一个本地 GPT 镜像

### 痛点说明

ChatGPT 需要开全局代理才能访问，而博主经常要在本地调试代码。经常在开启、关闭全局代理切换，非常麻烦。我们也可以通过配置 `gpt apikey` 的方式来使用。但是 `gpt api` 是需要收费的，每次用的时候感觉有点心疼呀。

那有没有不用 `apikey`，并且不用开全局代理的方法呢？其实是有的，下面我将介绍这种方式。

## 小白如何快速搭建一个自己的镜像站

使用的开源项目：https://github.com/Yidadaa/ChatGPT-Next-Web

下面这个地址，是我搭建的 ChatGPT 镜像站。在部署时我内置了一个 `apikey`，第一次进入页面后可以通过输入密码的形式直接使用内置的 `key`，也可以重新输入一个新的 `apikey`。

我设置的镜像站 `url`：https://chat-gpt-next-web3-two-rho.vercel.app/#/chat

密码：123456

![](img/a0b9d29f511e3c2e9e4e341246e7a410.png)

核心要注意，在部署的时候要写入这两个参数

![](img/b4f95d5d5e274d5988e8d39a18ceac37.png)

## 非 GPT Plus 用户怎么享受联网功能？

![](img/b52f152ba6b371a3981dfe634ef2a0cd.png)

可以使用这个开源项目：https://github.com/binary-husky/gpt_academic

在线演示：https://huggingface.co/spaces/qingxu98/gpt-academic

![](img/36dd18a605605680e5229ad073e44a16.png)

最后联网查询的信息正确

![](img/77d52972d3fd33b3644e5216236dfa4d.png)

复杂一点的分析问题，效果也不错

![](img/aeed67ee4cb51c5d7221f46b42c4058e.png)

## ChatGPT 插件 plugin 推荐

### 一、Al Video Summarizer

介绍：AI 视频摘要生成器。

视频地址：https://www.youtube.com/watch?v=SAV6lhwQs9w&ab_channel=%E5%B0%8F%E9%A9%AC%E5%93%A5%E5%9C%A8%E5%8F%91%E5%91%86

虽然视频是中文的，但是插件默认以英文的格式输出。

![](img/fc08115fc1da2ec0851f969513f0d0bd.png)

让 chatGPT 重新以中文再次输出

![](img/54bd97915b14cb5927caa1dd9054728c.png)

你可以进行更深入的使用，对视频中的内容进行提问、把视频文本转换成一篇文章/📕/朋友圈文案等，生成摘要等。可以结合 chatGPT `action` 调用多个功能进行处理，一键把 youtube 视频转成特定风格的 👸号文章。

### 二、AskYourPDF

支持上传 pdf 或者 pdf 链接可以读取 pdf 中的内容，并进行内容读取、分析。

原始链接：https://pdf.dfcfw.com/pdf/H3_AP202008261401662530_1.pdf

![](img/040801dfc086b4dfb7f314341c893b2d.png)

### 三、Zapier

可以通过 Zapier 连接 5000 多款应用

**ChatGPT 让每个普通人，不再平庸**

链接博主+免费 AI 社群+商务合作 👸号：AI 超体进化室

![](img/dbdf07c134f785f0ec5b860782ff28a4.png)

![](img/61e1e2dd5c7f3e1cd6f25c46d26d0c93.png)
