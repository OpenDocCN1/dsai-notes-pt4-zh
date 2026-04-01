# 【AI学习】不懂写代码，如何用Apifox做扣子 API接入测试？？

> 来源：[https://agijuejin.feishu.cn/docx/NIEWd5vNfoFdU7xbR9kcMRlhn4d](https://agijuejin.feishu.cn/docx/NIEWd5vNfoFdU7xbR9kcMRlhn4d)

> 作者：锅头1024 跟锅头一起学AI

> 原文：https://mp.weixin.qq.com/s/dZWueGjrpk5vNNzX_gjHNQ

## **▌锅头导读**

**需求小故事**

有朋友说现在把扣子搭建的Bot（H5链接）接入自己企业开发的App或者网站中，用户访问Bot链接后，目前存在两个问题（如有解决方案，可以考虑付费）：

① 需要再次输入手机号+验证码登录或唤起抖音一键登录，用户体验不友好。

② 开始使用界面以及H5页面顶部标题栏会出现扣子相关内容界面，与自己企业App或网站UI不统一，影响产品视觉一致性体验。

![](img/975a9bacd19ca040d88790a8f0211bc5.png)

**解决方案**

使用扣子API方式接入自己企业开发的App或网站。

**补充信息 **

目前扣子不管是API方式接入还是常规H5访问使用都有一定使用额度限制，不同时间限制规定可能会不同。以下信息是锅头在7月1日得到的反馈：

不同模型每日调用额度不同，如果模型当日额度已用尽，可以切换其他模型使用，或者等待第二天额度刷新。

豆包模型：同一个用户每日调用额度为500次。

其他模型：同一个用户每日调用额度100次。

模型调用频次：同一个用户每分钟最多调用10次。

**本文教程简介**

基于上面的需求小故事和解决方案，锅头以扣子API接入测试为例，在这里分享一个测试验证扣子API接口能力的详细教程。

该教程不能让你自己写代码完成系统接入开发，但是可以让你对此类开发有进一步了解，同时学会做简单的API测试。

**适合人群**

AI产品经理、AI测试工程师、AI开发工程师、希望深入研究AI的爱好者

**PS： **由于文章较长， **文末有学习总结 **。同时附上锅头耗时3个月用心编写的 《零基础学习使用AI工具——快速入门手册》，限时免费领取。

##

## **▌获取扣子API访问令牌 **

在浏览器地址栏输入 www.coze.cn ，按回车进入扣子主页。点击左下角【扣子API】。

![](img/6137917eb2130ff2d767954ea16143d9.png)

进入扣子API页面，点击【API令牌】。

![](img/efc53a83e99f38d39e963fab8e6c707c.png)

进入API令牌页面，点击【添加新令牌】。

PS：想要发布bot时支持授权扣子API访问，至少需要提前生成一个API令牌。

![](img/79a2c8627c49ea92c1a8f46c21b810d5.png)

弹出添加新的个人访问令牌窗口，填写名称、过期时间、访问团队空间、权限信息后，点击【确定】。

![](img/20512b808b18006ab0bc25485ad7b063.png)

弹出生成的令牌窗口。

**注意： **此令牌仅显示一次。请将此密钥保存在安全且可获取的地方。不要与他人共享，也不要在浏览器或其他客户端代码中暴露它。

![](img/03b514734b54a10b5e675a7f3adb84fe.png)

## **▌发布Bot并授权API访问**

在浏览器地址栏输入 www.coze.cn ，按回车进入扣子主页。点击左侧菜单【个人空间】。

![](img/a2e7a4d11634708bd5604e8afc27ed0d.png)

进入个人空间页面，选择需要授权API访问的Bot，我在这里点击【跟锅头一起学AI】Bot区块。

![](img/0867b84a37f2196d67d4e665abf524b5.png)

进入Bot编排页面，点击右上角【发布】。

![](img/eaef761f67575ee44e18d3a0be0208b2.png)

勾选【Bot as API】，点击右上角【发布】。

![](img/20d62b0e2310cbb621b7788fdbb0cee4.png)

已成功提交发布，点击【完成】。

![](img/9bb198fd9ab52e07a19e530ad1f04fd3.png)

回到Bot编排页面。查看编排页面浏览器地址栏上的网址，在bot/后面的值就是Bot_Id，先复制保存好，等下调用该Bot的扣子API时会用到。

PS：每个Bot的Bot_Id都不一样

![](img/fff505f970deffe561f8a5d3ffdca2b5.png)

## ▌ 用Apifox调用扣子 API 接口

下载安装好Apifox软件后，双击Apifox应用图标，打开Apifox。

PS：不了解如何下载安装Apifox的同学，可以查看教程： 【AI学习】如何下载安装Apifox（Windows操作系统）？

![](img/25cdf4a36b9ee7d925c1f11ecdd73e08.png)

进入Apifox主界面，点击【新建项目】。

![](img/7cd6a702634d14f806e34e08af9ed9a8.png)

输入项目名称“test”，点击【新建】。

![](img/e6f16000f091bf7e612c6cda23962fae.png)

新建项目成功，进入test项目界面。点击左侧【接口管理】-【接口】行项右边的【+】图标，点击【新建接口】。

![](img/fa8f5ee486458e11246f98fab53d3e9f.png)

进入新建接口界面。

![](img/bfb0cae14aff0f9798382c1d38b45441.png)

此时，我们再次打开扣子API介绍页面（ www.coze.cn/docs/developer_guides/coze_api_overview ），查看API发送请求所需的信息。

![](img/c500eae9f501d74f2c87d24b6d8c5f8a.png)

从文档的发送请求示例，我们可以获取到以下信息：

![](img/791d7e5daebf4da8e8ae0c62e5e14652.png)

请求地址：POST https://api.coze.cn/open_api/v2/chat

请求内容：

conversation_id：标识对话发生在哪一次会话中，可自行维护此字段，例如123。

bot_id：要进行会话聊天的 Bot ID，这里更改为刚才咱们复制好的Bot_Id值7343865587435094025。

user：标识当前与 Bot 交互的用户，可自行维护此字段，例如user123。

query：发送给 Bot 的消息内容

stream：使用启用流式返回。false，采用非流式响应。true 表示采用流式响应。这里填写false。

根据上面内容，更新后的json内容如下：

{

"conversation_id": "123",

"bot_id": "7343865587435094025",

"user": "user123",

"query": "请问你是谁？",

"stream":false

}

请求头：

Authorization: Bearer {{Personal_Access_Token}}

Content-Type: application/json

Accept: */*

Connection: keep-alive

接下来，按上面信息配置接口信息。

![](img/9710e6a32c798be8b376d1a1dc71370a.png)

![](img/59fb345709ae86c643b19ccce0e23933.png)

![](img/ac7e32b80fd4204cff227d93764f6a3f.png)

配置完成后，点击【发送】请求，看到接口请求成功，Bot API返回了“你是谁“的内容。

![](img/6541430057a3ed9b5a04f214617594bd.png)

至此，咱们完成了 用Apifox做扣子 API接入测试 的内容，这可以为今后做自研系统开发接入扣子API接口做好准备，你学会了吗？

## **▌ 学习总结**

经过这一轮学习与实操，锅头总结如下：

+   打开扣子官方主页，进入扣子API令牌页面。

+   【添加新令牌】获取扣子API访问令牌。

+   发布Bot并授权【Bot as API】访问。

+   打开Apifox【新建接口】。

+   参考扣子API输入接口请求地址、请求内容、请求头及令牌，【发送】接口请求获得Bot返回信息。

扣子API授权接入后，可以解决以下问题：

1.  用户获取扣子商店链接需要再次登录的问题。

1.  支持自定义开发用户交互界面。

## **▌写在最后**

[不少同学问锅头怎样学习AI？这里分](http://mp.weixin.qq.com/s?__biz=MzkwMzQ0MDIzMg==&mid=2247487439&idx=1&sn=0aaab9fa6a690326e0132969d1c4dd82&chksm=c0977dc8f7e0f4de56e17fa507ef1ecaba00bac7e87f0bd300b7d4a077fb92e5033dad3615a5&scene=21#wechat_redirect)享了锅头学习AI的思路和方法： 【AI学习】零基础如何学习AI知识和使用AI工具？（适用于AI入门/小白）

为了让希望能系统学习使用AI工具的同学快速了解AIGC基础知识、AI大语言模型、AI实用工具、提**示词以及一些常**见的应用场景。 锅头耗时3个月用心编写了 《零基础学习使用AI工具—快速入门手册》 ， 共12节入门课程， 限时免费领取 。

![](img/b5d13b282cad5878c1742882a3ae2b80.png)

**领取方式**

第一步：点击下方公众号卡片—>关注—>进入微信公众号。

第二步：点击底部菜单“ **领取资料 **”。

![](img/3afa739a384884dd4af47079a046e06f.png)