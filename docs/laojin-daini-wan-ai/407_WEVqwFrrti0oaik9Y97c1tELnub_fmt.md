
# COW 切换model说明

> 来源：[https://my.feishu.cn/docx/TUHNdo9qmoIaSgxANaGcmgGtnNb](https://my.feishu.cn/docx/TUHNdo9qmoIaSgxANaGcmgGtnNb)

## 前言

最近总有小伙伴问我，怎么能在不重启情况下自由切换`model`，整理成笔记分享给大家。

## 一、配置COW的config

首先你需要有多个LLM Key，如Coze、通义Qwen、智谱GLM等等。

然后进入cow文件夹，找到根目录的`config.json`。

![](img/3c182a0d39b1525eb403b12992f8aed8.png)

Config中`model`填写默认模型，模型相关的`key`，`url`等需要完整配置。

![](img/a5f921aa31ef86113e204235b3ee860a.png)

## 二、检查`Const.py`文件

![](img/30943124363a945a2d7e1709e0f84f19.png)

在`model`和`MODEL_LIST`中检查是否具备对应模型。

如图，我增加了coze，qwen是cow自带的。

![](img/174c854ad02d1ccefb0b1110049fb302.png)

![](img/5cee21d2876c11c4aced8f0cde437d22.png)

## 三、私聊机器人切换模型

配置完成后，需要扫码重启机器人。

私聊`#model` 模型名称，切换模型。

![](img/d115dda856326f065c68203d8b38477c.png)

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
