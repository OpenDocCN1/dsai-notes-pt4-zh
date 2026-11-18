
# Intern-S1 玩转 DeepChat

> 来源：`https://aicarrier.feishu.cn/docx/VAdPdMy9hoJQzvxbbkxceTFxnph`

![](img/606d7829f3c44b65623a4587c3bf41c7.png)

## DeepChat

### DeepChat - 您的桌面AI伙伴

> 快速、专注，为深度工作而设计。

DeepChat将AI的强大功能带到您的桌面，兼顾隐私和效率，成为您高效工作的理想伴侣。其为桌面客户端，兼容 Windows、Mac 和 Linux 系统。

`github`：`https://github.com/thinkinaixyz/deepchat`

### 安装

因为图形化界面，直接下载对应电脑版本的安装包正常安装即可。

地址：`https://deepchat.thinkinai.xyz/#/download`

![](img/4010c54eefae426a3b890a4037860413.png)

## 玩转 Intern-S1

上海人工智能实验室近期重磅发布了强推理科学多模态大模型 `Intern-S1`。书生大模型开源社区的开发者们积极响应，结合 `Dify`、`Coze`、`BrowseUse`、`CAMEL`、`Zotero`、`Eigent`等知名工具与应用，贡献了众多 `Intern-S1` 的创新使用案例。本文将分享 `Intern-S1` 与知名智能体框架 `CAMEL` 结合的视频理解等精彩案例。

### 如何使用API

主要分为：

1. 获取 `API Key`
1. 写到对应位置

其他各家的大模型在`Deepchat`设置也大同小异。

#### 获取书生 API Key

使用 `API` 调用大模型需要 `API` 密钥，这里我们以`InternLM`为例，您可以从 `https://internlm.intern-ai.org.cn/api/tokens` 获取，它提供`InternLM`系列的免费（`OpenAI`）兼容格式的`API`。

按照下面步骤，在书生大模型 `API` 控制台获取 `Token`

注意：每条 `Token` 只显示一次，这里 `Token` 指的是密钥 `API Key`，而不是调用时消耗的 `Token` 数。

请确保您拥有一个正常注册且可使用的账户。要生成您的私有 `API KEY`可以参考我们的图示。

![](img/e4222c7c379648be58f9cdd4aca6c89e.png)

注：

`任何平台`的 `API Key` 都不要泄露出去！。

#### 配置 Deepchat

这里默认安装完成。

初始界面，点击右上角的设置按钮。

![](img/e31757a94a191a4f662cde3b60f8e249.png)

选择`providers`:

![](img/be93f591e16eec967e70a47015909c46.png)

找到`intern`，但是`model`清单里没有`s1`

![](img/0781e42b489f12f1837853ca2fac8bf3.png)

单独配置，拉到最下边找到 `Add Custom Provider`：

![](img/389f4463d1cac6849f38f1ad48dc3215.png)

提供商名字可以随意取，我填写`Intern-S1`

添加 `API` 如下：

`API` 密钥：输入刚才获取的 `API Key`

切记：`API Type`选择“`OpenAI`”而不是`"OpenAI Responses"`

`API` 地址：`https://chat.intern-ai.org.cn/api/v1/`

![](img/e7075c8575ef814b8786c012ef418a12.png)

然后`Enable Provider`

![](img/0dfe04ab7ad454a267717e85231c0cb2.png)

点击 `Comfirm`；

点击检测后选择使用的模型点击确定，如果可用会有提示。

成功结果

![](img/a6a0dd25c339a07207581972b293bfac.png)

之后就可以在聊天页面进行聊天了。选择模型供应商`intern-s1`,然后`intern-s1`

![](img/6d3221a622c1842a7d25ff8637cfc460.png)

### Deepchat基础使用

#### 如何聊天

进入聊天页面，选择模型，开始聊天。

这里模型选择 `Intern-S1`

输入聊天内容并发送，你就会收到回复。

![](img/6551eeafba650ad231d4eb7220711ca8.png)

### Deepchat中用Intern-S1来使用MCP

`Deepchat`最强大的是什么? 当然是他对`MCP`的支持。

#### MCP设置

开启`mcp`，首先进入设置，选择`"MCP Setting"`。

![](img/3b8c950212972ab01fd48b5f940c0285.png)

首先能看到一些内嵌自带的`MCP`， 如“`File System`”， “`Brave Search`” 等。

可以启用它们 :

![](img/0f1bbcd5ff807b8a1cdd3a8fcd34028d.png)

状态从`stopped`变成`running`就表明启用成功。

切换到聊天页面，点击 锤子 图标。 你就会发现此`MCP`已经可以使用。

![](img/41bc1336263e89c5ef6b5c3ce37d26aa.png)

#### 开始用S1来做Deepsearch

一个比较常用的就是`Deepsearch`。

![](img/adbd7b8fb56fcadd6add5a3475bd091e.png)

#### 其它探索

可以自己添加`MCP`，然后用`Intern-s1`来进行各种尝试。

请大家开启自己的`Intern-s1`玩转`MCP`之旅！~
