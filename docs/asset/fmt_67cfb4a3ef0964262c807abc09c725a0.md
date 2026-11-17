
# GPTs Action 应用实践案例

> 来源：[https://geekbang.feishu.cn/docx/PpN8dfIMSoe4k3xHyMdc5pZonCd](https://geekbang.feishu.cn/docx/PpN8dfIMSoe4k3xHyMdc5pZonCd)

## 什么是 GPTs Action

GPT Action 为 GPT 提供了一种集成外部数据或与真实世界进行交互的能力，你可以通过 Action 提供一个或多个 API 给 GPT 来定义自定义操作，比如将 GPT 连接到数据库、电子邮件中，或将其用作购物助手等，为开发人员提供更大的模型控制权和 API 调用方式，因此使用 Action 需要一些编程知识。

## GPTs+Action 进阶教程

### 1、定制化 GPT & GPTs 应用商店

现在每个人都可以构建自己的 GPT，GPTs 应用商店正式发布，开发者可上传自己的 GPT 并获得收入。接下来带大家创建一个自己的GPT

1、进入`https://chat.openai.com/`聊天页面，切换到`gpt4.0`模型，然后点击`Explore`

![](img/d754eb306aa5df618e3acf2899c766bb.png)

2、点击`Create a GPT` 创建一个自己的GPT

![](img/c9a811e0ea08ea6655101fafbc7d55d7.png)

3、然后我们会来到这一步配置GPT

这里给我们提供了两种创建gpt的方式，左边的`Create` 这个我们只需要通过聊天的方式告诉gpt你想要创建一个什么类型的gpt就可以，然后gpt会生成头像以及名称还有描述，每生成一项都会询问你是否满意，如果不满意可以让他进行修改。

![](img/3554bdd3b62cefcfce4f22542345a70a.png)

一步步回复之后就创建出了下面的gpt，右面的界面是让我们对自己创建的gpt进行测试的。

4、然后我们来看第二个选项`Configure`

![](img/b7d94585a8ad8e81f08d9da2d88d82a1.png)

![](img/a3087c8d7cba1b0273601f322186d869.png)

选择`Configure`进行创建也很简单，不需要全部都填写，同样我们创建之后再右边先进行测试，确认没有问题之后我们就可以点击右上角的`save`

`Only me`是仅自己可见，`Public`就是公开的，`Only people with a link`被分享这个链接的人可以使用。

通过以上的操作我们就已经创建好了属于我们自己的GPT了。

#### Actions配置

接下来呢，带大家了解一下`Actions`的使用，可以说这个功能非常强大，有了这个功能gpt的玩法也变得非常多。通过`Actions`我们可以介入很多api，什么天气查询，日历查询等等，但是！今天给大家介绍的不仅仅是这几个小功能，我们要说的是zapier的官方介绍是GPT 中的 `Zapier AI Actions` 使您能够在 6,000 多个应用程序中进行超过 20K 次搜索和操作，从而赋予 GPT 超能力。没错根本玩不过来！

接下来我只讲重点

首先我们进入`https://actions.zapier.com/docs/platform/gpt`，这里有一个URL我们需要复制一下

![](img/102e6d5485fd1a647bcda346e09eb6a6.png)

然后我们回到gpt中点击`add actions`

![](img/b04bdad4376106354a05b6e66c560212.png)

然后点击`Import from URL`，将链接粘贴进去点击`import`

![](img/41e1c96cd50f8cc323db09ca0dca437c.png)

然后我们直接点击返回就可以

![](img/fa2a3f6093ff85da47035b44f0ba4b1f.png)

我们能看到已经引入了`zapier`

![](img/9b164b13076c19335644411d3c6de7d8.png)

然后我们将下面图中的部分全部复制

![](img/02f2a625eee348e259190f5f06fb6c36.png)

![](img/f799be41916e164a498c8157e9f65bb1.png)

上面的行为和链接是必须要进行配置的，在哪里配置？

点击`https://actions.zapier.com/gpt/actions/`

![](img/7a6a19aaa9a7acee5c99062afc6206c7.png)

![](img/e30352351def5f63f57ed51d089b50e1.png)

点击之后

![](img/70fd61148b45d6e03253622aa5c43ac2.png)

完事之后点击创建就行。

细节来了，下面的两个必填的就是这么来的

![](img/827bce6bcadf4789eb0c83fa83c42000.png)

最后我们返回看到就是一个打开的状态

![](img/68c38a9605ff6a934e4beb5735240f61.png)

至此我们已经完成了对接，最后我们就可以回到`chatgpt`的页面在右边进行测试了。

### 2、天气查询：三步搞定GPTS的Action

实现效果：当我和GPT聊天，问到天气的时候，GPT会调用定制的`Action`，拿到返回的天气值。

三步走：

第一步：确定天气的`API`。

我用的是免费的开源`API`，

![](img/931ab56de8d5122a6f11b340d7ee46f4.png)

大家可以去这个网址开通：`https://user.ip138.com/weather/doc/`

注册并拿到`Token`(你独有的访问口令，不是`GPT`的`Token`）

第二步：改写`OpenAI`格式的调用`json`。

这里可以打开`Edit actions` 编辑器。照着`GPT`的`Json`格式的调用直接改。

![](img/969ce368824ee3d0a09308c339fa357a.png)

关于这个`json`的解释，可以直接问下`GPT`, 我这里放出更改后的代码。

![](img/0681c74a895207423fe9bc68f0ab1a59.png)

复制下面的代码贴进去：

```json
{
"openapi": "3.1.0",
"info": {
"title": "Get weather data",
"description": "Retrieves current weather data for a location.",
"version": "v1.0.0"
},
"servers": [
{
"url": "https://api.ip138.com/weather"
}
],
"paths": {
"/?type=1&callback=find&token=5f": {
"get": {
"description": "Get temperature for a specific location",
"operationId": "GetCurrentWeather",
"parameters": [
{
"name": "code",
"in": "query",
"description": "The city and state to retrieve the weather for,need change the city to the Administrative division code，like 北京 is 110000",
"required": false,
"schema": {
"type": "string"
}
}
],
"deprecated": false
}
}
},
"components": {
"schemas": {}
}
}
```

第三步：添加`Prompt`

在`Prompt`里引用下这个调用：

如果客户问起天气情况，可以利用天气查询的`action`，需要将用户说的城市名称转为中国行政区划代码，如北京，就转成110000，传输给查询天气的工具，让其返回结果。在返回的结果中你只需要取最关键的信息，转为：北京的天气是35度。

完成。

![](img/327e7db3647e646ec7cab561ad7b1bad.png)

参考资料：

`https://zhuanlan.zhihu.com/p/667427477`

`https://blog.csdn.net/xiangyuWA/article/details/134397446`

`https://zhuanlan.zhihu.com/p/667427477`

## 相关视频

多维表格（`XJZfbj8kyadBAhsPv80cN1Njn1e_tblInRZaIcatukFB`）
