# Coze-Skills，完整使用教程看这里！

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/HJFXdJCm0oEsKQxJcpFcxBNjnTh](https://qqs7y1hozd1.feishu.cn/docx/HJFXdJCm0oEsKQxJcpFcxBNjnTh)

字节的速度真的是太快了，Skills才火1个月的样子，就在Coze平台上线了技能商店

![](img/b2cb7d36b750bc58d316d5db7711bdb5.png)

和之前的coze工作流一样，不但提供技能商店供买卖还可以自定义技能

![](img/998a66a973bb9d35ec42dbd3e4e3412d.png)

# 01

** **直接使用技能

在商店中选择一个技能进行安装

![](img/22b557ff25ffd501d387d52ad1b89138.png)

![](img/34a383fa4dbd6732b855f1be7b970df6.png)

最终生成网页版的绘本，并且自带了配音。

![](img/4fb56f2df2c90afcd778262abf7020b2.png)

# 02

** **通过Skills文件创建

进入coze编程界面自定义技能

https://code.coze.cn/

有两种方式，一种是上传skills文件包，一种是通过自然语言

![](img/2cc8683f0d39a1fd444c5e0f22868c2e.png)

也就说，你在本地调试好的skill工程，可以上传到coze平台形成技能。

这里我用Anthropics官方提供的skills来测试。我把PDF的这个skills文件包上传到coze上

![](img/35fe59152965233c5578d8da6a231f87.png)

coze第一步进行压缩包解压，并且通过阅读工程中的文件完成了Skill的结构分析

![](img/b8dce19eb1a7372e72c8597efdc9819f.png)

从这个分析报告可以看出，coze完全是遵照Claude Skills的官方使用说明来的。也就是说，coze安装了官方提供的skill-creator

缺少的字段，不符合结构的文件，以及归档错误的文件，都能识别出来

![](img/a63d061d2a7c830a086ec655adfe7d63.png)

接下来就是在后台沙盒中，根据我上传的skill文件来1:1的复刻整个工程。

![](img/7f46e93fc9e5d10afbc0ca111db1b785.png)

只要你的Skills工程没啥大问题，基本上都可以顺利部署。

所有工程完成后，点击部署就可以正常使用了

![](img/c703d5350cdb345091ab63e48c9a174b.png)

部署完成，在技能中直接使用。在我的技能->我的创建中，也可以找到创建的这个技能

![](img/35685259131455219a7e31d7c047a11f.png)

我觉得这个功能非常的赞，这样会吸引更多的Skills创作者加入到字节这个生态去上传Skills

且字节开通了付费渠道，变现也变得非常容易。

# 03

#  自然语言创建

最近x官方不是出了一篇文章，讲如何写出高质量且被x推流

![](img/67b1ce23e66732f3f3d31ba464c13da3.png)

文中列出了好几个写作要点，我正准备把这个封装成一个skills的。现在正好通过coze的技能包来做一个现成的

这8个要点，翻译成中文后，让coze根据要点给我封装一个x爆文技能

![](img/90a6320f46a9b64f7aba1b0acaf426ab.png)

很快就按照Skill的结构生成了对应的文件。

![](img/4a78bd479406cbecf2ed818ab61b79d9.png)

Skill.md中的内容生成得还挺专业的。文件支持下载，不想写亲自写Skill的完全可以交给coze来做。

来试下效果如何，用罗永浩和西贝事件来写一篇吐槽文

![](img/3893fa3c388e16f24049b0f653ae9eff.png)

首先就是加载技能，然后是事件内容的精准搜索

注意:要打开深度思考，这样搜索会更全面一点

![](img/7d36eb4de6485c42fe86999891f8235c.png)

生成的x推文，有点长，只截了部分。

![](img/72b7841e9a984df5ce9441b9d4d2a95f.png)

没想到的是，居然还给生成了封面图。效果还不错，哈哈。

![](img/07800b31807a4a536c036d8d543b6f79.png)