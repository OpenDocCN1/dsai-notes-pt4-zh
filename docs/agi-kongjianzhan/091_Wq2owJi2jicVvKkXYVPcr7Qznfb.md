
# 终于！我实现了大模型自由！

> 来源：[https://agispace.feishu.cn/docx/DeFVdcPyzoCpy8xbebxc3urunuc](https://agispace.feishu.cn/docx/DeFVdcPyzoCpy8xbebxc3urunuc)

> 作者：祝豪

> 👸号：卡尔的AI沃茨

> 原文链接：https://mp.weixin.qq.com/s/vYioQPvHH54tsiYMiEOY5g

**近期收到**了大量群友反馈的被OpenAI的封号操作，没想到在众多新的大模型Claude 系列，国产Kimi争相登场的时候，OpenAI却来了一波封号操作？！实在是让人不解（虽然Claude也封的好凶）

但这不由的让我思考这个问题，从发布以来OpenAI等公司的产品对内小伙伴们一直都非常的不友好，从注册开始，海外号码，网络问题，支付问题，可以说是一步一个坑，虽然我们也曾经科普过所有的踩坑经验，但是还是很麻烦！

那有没有什么更简单方便的方法呢？

这一篇我就带大家看看`API`和`chatbox`这种方便的方式如何调用大模型！

## 避免了什么“麻烦”？

`API` + `chatBox` 这种方式具有以下特点：

1. 免注册，不用折腾海外电话号码

1. 不存在网络限制

1. 支付方便，不需要海外银行卡

1. 不会被封号！！！

### Chatbox

在`ChatGPT`刚推出不久，就有一个“伟大”的开源项目出现，那就是`chat next web`，现在已经支持 `GPT3`, `GPT4` & `Gemini Pro` 模型，熟悉我们的朋友，估计也是这个`chatbox`的老用户，我们也将这个`chatbox` 部署在我们网站上，还不知道在哪使用的朋友可以在文末查看链接！

![](img/b09fad88574adbc3cc40ee779ca872dc.png)

页面非常的清爽，填入`API`就可以直接使用，使用体验非常的顺滑，支持`markdown`语法

![](img/c5ab3c668c793185ab86904a91124a46.png)

并且预置了很多提示词，可以更方便的使用！

![](img/a0009c86c01e84d06bb9fd24c3e87ad8.png)

更有可以自定义定制的提示词模板，可以选择或者自己创建！

![](img/619ed21147cf025f6a06530ea413ff73.png)

在这种`Chatbox`网站上，`API`就像一节干电池，即插即用，用完即抛，方便简单，再也不用担心封号的问题！

### API

`API key`，这里给不熟悉的朋友们，科普一下，OpenAI `API key`就像一部的手机卡。只有当您插入正确的手机卡（`API key`）时，您的手机（`Chatbox`）才能连接到OpenAI的网络，从而访问他们的人工智能服务。

我们在这里也给大家分享两个比较不错的`ChatGPT` `API key`的第三方提供商，价格非常的便宜！

`ohmygpt`: `https://www.ohmygpt.com?aff=Bwvtgw3g`

`ohmygpt`现在有白嫖活动——在他们网站上使用`gpt 3.5` 是免费的！

其他模型价格表放在下面了，大家可以参考一下

![](img/fbc660fcc386e51a95cce3e649fee368.png)

`oneapi`: `https://oneapi.gptnb.me/register/?aff_code=ertb`

`oneapi`支持的模型可以说是非常的全：

![](img/ab5df6908b88170d11ff522b213bc65b.png)

`oneapi` 的价格也非常便宜，按照官方的`token`计费但是对应的美元比率是1：1.7！！！

这两个`api`服务商都有免费的额度，使用时只需要，在这两处分别填入对应的`endpoint`和`API key`即可！

![](img/54fe8ee8c9a6e412a465c71e56e7e201.png)

下面我给大家放出他们的`endpoint`：

`ohmygpt`：`https://www.ohmygpt.com`

`oneapi`：`https://oneapi.gptnb.me`

再也不用担心被封号了！
