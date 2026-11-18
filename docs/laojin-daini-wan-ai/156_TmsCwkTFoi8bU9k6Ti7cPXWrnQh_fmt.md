
# Cursor链接Dify自动化创建工作流？老金手把手教你！

> 来源：[https://my.feishu.cn/docx/M8JzdCVpMohw1qxu0U9cYLH9nrd](https://my.feishu.cn/docx/M8JzdCVpMohw1qxu0U9cYLH9nrd)

近期老金发现用Dify的人越来越多了，曾经也有小伙伴问过老金为什么钟情于Dify，而不去Coze？

今儿老金就把秘密公开！

因为老金我啊，早就用Cursor自动生成Dify支持的DSL工作流了。

基于Dify开源的属性，很多东西是可以注入的，而Coze不行，在Coze上还是需要来来回回手动拖拉的。

今儿老金就拿个Cursor生成的简易DSL来给大家讲解下，到底怎么弄，解放双手！

本文最后，原理拆解才是本文的精华，希望小伙伴本认真看。

## 环境准备

首先，你需要安装两个东西：

1、Dify本地部署 [Dify本地部署](https://tffyvtlai4.feishu.cn/wiki/HZKXwSIn3iFBZYkcSWFcJAUhnof)

2、[Cursor](https://tffyvtlai4.feishu.cn/wiki/AHKgwiM6Eip76kkzfHDcJWkxncb)AI编程神器 - Cursor（看老金我最近都在写Cursor的东西，就知道它是生产力最高的工具之一了）

可选项：硅基流动（API聚合平台，Dify中使用很方便。）

注册链接：`https://cloud.siliconflow.cn/i/wdyoipMi`

## 制作过程

和上期一样，全程也是一句话，但是你需要做些准备。

别急，老金手把手告你怎么做。

打开Cursor `Settings->Features->Docs->Add new doc`

![](img/5c731c74e2df9d2f140ed49e8927ffb5.png)

输入Dify官网文档地址

```
https://docs.dify.ai/zh-hans
```

![](img/0cd7db174bee86865ed12572b10930d3.png)

显示小绿点就成功了，可以看到网页全部导入。

![](img/f63201a581b134afcad63a893937351b.png)

然后在对话窗口中，`@Docs`，找到Dify

这样，你的Cursor里就有了对于Dify的基础了解。

然后，你可以在这里找到一些参考DSL，下载下来，作为另外一份参考。

```
https://github.com/svcvit/Awesome-Dify-Workflow
```

![](img/936bc2e6d3e19a2e1afd2f03595dd50e.png)

再然后，你只需要在Corsor里@上Dify文档，@上上面下载的文件夹，再输入以下提示词就够了，本提示词仅作为参考使用，下面会讲怎么写。

```
#环境说明
我现在是&lt;Windows10&gt;，Dify版本是&lt;1.1.2&gt;，部署在&lt;Docker&gt;上，已经启动了。
Docker你可以使用&lt;MCP Docker&gt;进行操作并查看日志。

#目标说明
我想创建个工作流，用于网页英译中，固定的名次需要保留英文原名，如ChatGPT，Gemini等。
需要使用&lt;Tavily Extract&gt;获取输入的网页，并把网页输出后进行二次迭代润色。
使用&lt;ChatGLM&gt;插件中的&lt;Glm-4-flash&gt;进行翻译和润色。

#过程
你想要使用&lt;MCP thinking&gt;进行分析，并创建todolist，完成一项后，自行标记完成，并开始下一项工作，直到全部完成。

#结果
你需要创建完成完整的DSL文件，用于我直接导入到Dify项目中。
请不要乱删乱改，你的每一步都需要深思熟虑，确保达成目标。

#参考
你需要在&lt;dsl plugins&gt;内查找插件说明，如果其中没有对应的说明，你需要使用&lt;MCP fetch&gt;进行查找。
```

输出过程。

![](img/c3c862feec373ee46ace84a83c4ab215.png)

![](img/010fd6ac79c7593a4415ec41efa07b5b.png)

过程很长，最终导入进Dify就好了。运行有报错，你可以直接让Cursor查看Docker运行日志，连复制都不用！

随便找个外文文章试一下。

![](img/b7a99b28088763a49385185256ed3ea4.png)

Dify里导入地址，运行，啪！就搞定了！爽不爽！

![](img/7623153de838bca4f59f4d71a50cd097.png)

## 原理拆解

讲几个重点内容，本文中用到2个核心内容。

### 1、提示词

老金我曾多次讲解，提示词是你和AI沟通的唯一手段，哪怕是视频它看到的也是先转成文字进行理解。

我们来看下老金给的提示词结构，包含以下逻辑：

A、我的环境是什么

B、我的目标是什么

C、你应该怎么做（这里敲黑板，如果你不懂Dify插件，不懂MCP，你是说不出来的，所以老金一直说，提示词的根本是你的认知，以及最后的`dsl plugins`其实是老金自编的，此份不开源，不用和老金我要 = = ）

相信AB大家都很容易理解，但是C，就需要你花大量的时间去学习。

切记，AI不是许愿，你需要给它大量的信息里做出框架，他才能知道你需要做什么。

### 2、Cursor+MCP

这个咱就不多讲了，近期老金的教程里都涵盖了很多，可以翻翻老金之前的教程。

[Cursor](https://tffyvtlai4.feishu.cn/wiki/AHKgwiM6Eip76kkzfHDcJWkxncb)CP在0.46和0.47版本介绍中都有介绍一些，自行查看 。

## 结语

老金我拿了个简单的DSL来做案例，给大家说明底层还是要大家首先具备认知，才能用的好，用的快。

而且给大家个小思路，如果这种简单的做的好了，那么是不是可以一点点累加？

就比如上面这个英译中，目前是抓取网页进行内容翻译。

我可不可以再增加一个定时抓取？然后再增加一个自动记录到飞书文档？或者自动推送到🛰️？

讲这些，是为了大家要有发散思维，举一反三的能力，先找到自己需求中最根本的那个环节，不断进行补充，不要一口吃个大胖子，那样是出不来结果的，很有可能试一次之后，觉得不行就不做了。

这种失败感是巨大的。

就像，老金我最初，第一句学会的提示词一样。

“让我们一步一步来”

### 往期推荐

LLM(大语言模型相关)全套教程列表

[WX机器人教程列表](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzI0NzU2MDgyNA==&action=getalbum&album_id=3502843007181520907#wechat_redirect)

[AI绘画教程列表](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzI0NzU2MDgyNA==&action=getalbum&album_id=3192433076551843848#wechat_redirect)

[AI编程教程列表](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzI0NzU2MDgyNA==&action=getalbum&album_id=3704202865347362819#wechat_redirect)

硅基流动 Siliconflow教程列表

以上，本期分享完成，希望对大家有所帮助。

---

如果觉得不错，随手点个赞、在看、转发三连吧🙂

如果想第一时间收到推送，也可以给我个星标⭐～谢谢你看我的文章。

关注发送“知识精华”，可获得免费的知识链接地址。

![](img/ab3955169dbc56bb5f15fbc0f3c1143e.png)
