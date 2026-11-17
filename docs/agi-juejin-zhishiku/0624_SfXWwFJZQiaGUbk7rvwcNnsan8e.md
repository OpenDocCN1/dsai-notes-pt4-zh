# 【Trae编程】使用Trae + Coze开发思维导图的谷歌浏览器插件

> 来源：[https://agijuejin.feishu.cn/docx/PPfQdshvEoLMqexYWt6cuRMInuI](https://agijuejin.feishu.cn/docx/PPfQdshvEoLMqexYWt6cuRMInuI)

> 作者：卷福同学

> 链接：https://juejin.cn/post/7467859166406459418

> 来源：稀土掘金

> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

> 如有侵权，请联系删除。

**我正在参加Trae「超级体验官」创意实践征文， **本文所使用的 Trae 免费下载链接： www.trae.ai/?utm_source…

## 1.效果展示

![](img/c95a377a22663318c6bb2c188b424c12.png)

## 2.AI编程开发流程

虽然AI编程知识简单对话就行，不过咱要逐步深入到项目开发中，所以还需要前面的需求分析、调研等等步骤，下面是完成一个需求的流程图：

![](img/3420cd7c6dca3118405a93e2e3af46fd.png)

## 3.寻找思维导图Coze插件

我们在Coze的国内站点里搜索能[生成思维导图的插件，Coz](https://link.juejin.cn/?target=https%3A%2F%2Fwww.coze.cn%2F)e官网地址： www.coze.cn/ ，这里发现TreeMind树图就可以满足我们的需求，输入文字，输出思维导图的图片：

![](img/1e7f7888459c8577986731c09fc88a21.png)

## 4.创建插件测试流程

我们在 工作空间 > 项目开发 > 创建 这样创建一个应用，

![](img/5ae2f8dae7fd31624d65c853a72afba4.png)

然后创建工作流，在工作流中 添加节点 将TreeMind插件添加进来后，再修改下输入输出的变量配置，这样一个工作流就创建好，接着可以手动测试功能是否正常

![](img/0155e3a875091df2efe04b9d7ae2b2aa.png)

这里我让其生成一张分布式系统的思维导图，试运行成功后得到图片的链接地址，打开后就是下面的这张图片，能满足我们的需求：

![](img/efaed95e43426e374495dbafac1ce303.png)

接下来我们将整个流程发布上线，后续的开发编程过程就可以用到了。

点击 发布 按钮，然后发布时选上API

## 5.API参数获取

**Coze API的**使用是有限制和计费的，基础版本的账户每月只有 100次调用机会 ，超过后需要升级成专业版才能付费使用，

Coze API官方文档介绍地址： www.coze.cn/open/docs/d…

API调用工作流文档地址： www.coze.cn/open/docs/d…

### 5.1个人访问令牌

地址： www.coze.cn/open/oauth/…

自行手动创建一个，然后把token拷贝下来，注意只有第一次创建的时候才能看到token

### 5.2 workflow id和app_id的获取

打开应用工作流编辑页面，点击工作流，在浏览器的地址栏里 project-ide 后面跟的数字就是 app_id ，而 workflow 后面的那串数字就是 workflow_id

![](img/73fe1a22978e148f4b7255943e929fa9.png)

### 5.3 在线测试API

[我们通过在线测试普通Apifo](https://link.juejin.cn/?target=https%3A%2F%2Fapp.apifox.com%2F)x来测试接口，地址： app.apifox.com/

可以将工作流API文档里的示例拷贝出来，在Apifox中选择 导入cURL 的方式新建一个请求，修改对应的参数为自己创建的应用参数，就可以请求执行了，下面是我的请求示例，大家可以自行参考：

```
curl --location --request POST &#x27;https://api.coze.cn/v1/workflow/run&#x27; \
--header &#x27;Authorization: Bearer pat_npmd******&#x27; \
--header &#x27;Content-Type: application/json&#x27; \
--data-raw &#x27;{
    &quot;workflow_id&quot;: &quot;746*****6370&quot;,
    &quot;parameters&quot;: {
        &quot;input&quot;: &quot;生成分布式系统的思维导图&quot;
    },
    &quot;app_id&quot;: &quot;74663******692927&quot;,
    &quot;is_async&quot;: false
}&#x27;
```

![](img/221b9dc55ba078f92d2bfedd20ccf2e1.png)

![](img/7728b2320319c7a19868226690009dab.png)

## 6. Trae编程集成Coze插件

为了开发生成思维导图的浏览器插件，我们开始逐步操作，第一步，先生成插件的基础框架：

```
## 用户故事
用户选中文字，可以显示浮动按钮“生成思维导图”，点击后弹出右侧窗口，显示对应的文字

# 注意：
1.使用manifest v3版本开发
2.注意中文编码问题

# 任务
请按照用户故事和注意点帮我开发谷歌插件
```

![](img/18911440f3fc99f80a6398c4c578eab6.png)

接着生成侧边栏的功能

```
继续侧边栏的实现：
1.创建一个侧边栏的HTML页面
2.实现在background.js中打开侧边栏的逻辑
3.添加侧边栏的样式
```

可能会出现一些bug问题，我们把bug发给cursor后继续修改完善，下面是框架的效果图，可以看到已经满足我们的需要了，然后可以进行下一步操作了

![](img/14b932361f356ed6b80441dbc3bc9f20.png)

![](img/7b3f63510650dc24b8fc96e931dce602.png)

### 6.1 整合coze API

基于现有代码，现在我们需要整合Coze的API，建议提前把已有功能告诉Trae，这样也能更好地理解代码背景。输入提示词示例，可结合个人实际情况调整：

    &quot;data&quot;: &quot;{\&quot;output\&quot;:\&quot;https://static.shutu.cn/shutu/jpeg/opena5/2025/02/04/42b4e22a73f679b41c4d4***fae.jpeg\&quot;}&quot;,

```
# 目的
用户需求是基于网页选中的内容生成一个思维导图
# 用户故事
用户在网页上选中一段文字，点击浮动按钮，可以生成思维导图的图片，在侧边栏展示
# 生成思维导图的接口
## curl请求的代码
curl --location --request POST &#x27;https://api.coze.cn/v1/workflow/run&#x27; \
--header &#x27;Authorization: Bearer pat_npmd7UxG7fFt1XveyQL*****I6K6nnQTP&#x27; \
--header &#x27;Content-Type: application/json&#x27; \
--data-raw &#x27;{
    &quot;workflow_id&quot;: &quot;74664****56370&quot;,
    &quot;parameters&quot;: {
        &quot;input&quot;: &quot;生成分布式系统的思维导图&quot;
    },
    &quot;app_id&quot;: &quot;74663*******692927&quot;
}&#x27;

## 请求参数说明
1. parameters下面的input：网页选中内容

## 返回数据
{
    &quot;code&quot;: 0,
    &quot;cost&quot;: &quot;0&quot;,
    &quot;debug_url&quot;: &quot;https://www.coze.cn/work_flow?execute_id=74679023****396&amp;space_id=733130656****6863&amp;workflow_id=746643****6370&quot;,
    &quot;msg&quot;: &quot;Success&quot;,
    &quot;token&quot;: 0
}

## 返回参数说明
1. data下的output为思维导图的图片地址
2.code 状态码，不是0代表出差
3. msg 返回信息

# 注意
1. 注意使用manifest v3版本开发
2. 注意中文编码问题

# 任务
根据 用户故事 和提供的 生成思维导图接口，以及相关注意点，请优化当前谷歌插件
```

![](img/589bf83f34b72975d00fa499927f93f2.png)

![](img/23ff3059f2213d584920a3eed18e8aba.png)

![](img/7b813f7a4fa38c42de73416cca3959ae.png)

最终效果已满足用户需求了！！！

剩下的比如图标不好看等等问题可以自行调整

## 7.总结

今天通过一个实际需求，了解了使用Trae编程的整个开发流程，相信看到最后的读者也能自行学会

在AI时代，以后的程序员可能不必再执着于从零开始写代码，学会借助现有工具，运用自己的创意，懂得站在巨人肩膀上，总能看得更远，走得更快