
# Coze智能体之飞书插件授权

> 来源：[https://zinxkclpk97.feishu.cn/docx/PIbDdUOkZoerDPxZqOtcdPiqnHd](https://zinxkclpk97.feishu.cn/docx/PIbDdUOkZoerDPxZqOtcdPiqnHd)

## 引言

很多小伙伴使用Coze搭建工作流，多少会用到飞书插件，用来访问飞书系资源。例如飞书文档、消息通知、通讯录等等。这样很容易扩展工作流的能力边界，玩法更灵活。

![](img/85cfea44d44d0daa6f24b394f3179a90.png)

今天要聊的是飞书文档插件的使用权限问题，如何对`Bot`授予正确的飞书权限。

## 授权模式与限制

目前飞书开放API有两类授权模式，分别是应用级授权和用户级授权。目前Coze平台中官方提供的飞书插件均是用户级授权模式，可以看到下图中插件方法中并未提供对身份权限的认证参数。

因此你如果使用官方插件，只能访问有限的飞书资源，且需要在访问时需要由飞书管理员进行二次授权确认。

关于授权问题，这里是官方的一些解释：`https://open.feishu.cn/document/faq/trouble-shooting/how-to-fix-the-99991672-error`

如你的`Bot`发布之后，有其他用户首次使用也会提示需要授权的问题。

下面我们结合Coze的使用，介绍下授权需要注意的问题。

![](img/3dd3be7a2676697179517aa336566de4.png)

## 常见应用场景

其实，我们在`Bot` 工作流引入飞书文档的目的，是能够为`Bot`提供额外的数据存取方案，结合飞书文档的特性有更多新的玩法。

如果是公开的`Bot`，大多数情况下，不会考虑使用者是否具备`Bot`中私有数据访问权限的问题。

常见的场景：

我的一个`Bot`，希望用户在使用之后，能够详细记录下其使用过程中产生的有价值的数据，用于后续更好的运营。

我有一个`Bot`，用来做爆款文案采集的，将每篇文章的内容进行处理后，存到我的飞书多维表格-案例库中。用于后续的分析和二创。

这两种场景有一个共性的情况，使用的都是我个人的飞书文档作为数据载体。但这个文档并未授权给其它的用户。因此这种情况，就不能使用官方当前提供的飞书插件了。

## 应用级访问权限

引入一个概念应用租户授权、用户授权，懂技术[的同学可以自行到飞书开发者](https://open.feishu.cn/document/server-docs/api-call-guide/calling-process/get-access-token)平台进行研究，开发文档 - 飞书开放平台

这里我们可以为`Bot`授予应用级访问权限，让`Bot`以应用的身份访问我们的飞书资源。这样就实现了一次授权永久使用了。如下应用访问凭证：`tenant_access_token`。

![](img/202cf04a5b1685201c8dbaff59925392.png)

## 获取与配置应用访问凭证

那如何获取和使用应用访问凭证呢？

登录飞书开发者后台，`https://open.feishu.cn/app/`

找到你发布的`Bot`，在这里属于自建应用

![](img/463b273ee438091b27363f58bc460ab1.png)

进入详情页可以找到`app id`、`app secret`两个参数，需要记录下来。

![](img/a2760a46dac4b85364de92f536e8c5da.png)

在开发配置菜单，点击权限管理，在这里选择需要授予的权限，比如飞书表格的新增、读写、删除等权限。建议全开，因为很多权限有父子级关系，漏开了也用不起来。

![](img/0a0b0a11c1b621d2c296e4f361c02903.png)

## 飞书插件使用测试

授权完成后，我们测试下飞书插件的使用情况。这里使用两个插件进行测试，获取飞书授权，获取文档内容。

先创建一个飞书文档，用来测试，这里把文档的`document_id`保存下来。并且放开文档的阅读权限

![](img/1a8bd761c9a5154ff12fc3e179bdd2c6.png)

放开权限范围

![](img/038d1fa69809c4ede005369667655136.png)

工作流配置，添加插件，填写参数，这里需要填写的有`app_id`和`app_secret`，用来获得应用授权凭证。指定需要读取的文档ID。

![](img/c5ddac8f637d5f0e541614893478405f.png)

![](img/766abedd3a43a132174fece8da61f55c.png)

看下执行效果

![](img/f404d9603bcfe9f43c1a977ab9ea1ef7.png)

同样，可以添加写文档的操作，但目前官方暂未提供对应的插件，我可以自己根据官方API二次封装或使用其它用户上架的插件。（期待官方更新吧，哈哈！！）

看下效果

![](img/d2699d25856862f70ed32245945ea20c.png)

飞书开发文档中也给了详细介绍，并且支持在线调试，方便二次开发，这里不多赘述。

![](img/5b7e9eda811293235b9eb4d878dadf79.png)

## 参考链接

`https://open.feishu.cn/document/server-docs/authentication-management/access-token/tenant_access_token_internal`

`https://open.feishu.cn/document/faq/trouble-shooting/how-to-fix-the-99991672-error`
