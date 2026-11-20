
# 服务器繁忙，请稍后再试？用Trae开发DeepSeek的离线版本

> 来源：[https://agijuejin.feishu.cn/docx/MerfdInoPoZ8bIxwmZpcMIJCntc](https://agijuejin.feishu.cn/docx/MerfdInoPoZ8bIxwmZpcMIJCntc)

> [二进制的梦想](https://mp.weixin.qq.com/s/HmcnQ0euxTDDSMQMBs7jTA) 野生的独立开发者｜阅读原文，如有侵权，请联系删除。

> 转载请联系原作者取得授权

“这段时间，我见过的最多文字，除了每天像刷屏一般的DeepSeek的新闻外，就是不断出现的“服务器繁忙，请稍后再试”。从联网搜索需要稍后，到深度思考(R1)需要稍后，再到。。。”

**这你能忍？**

**我不要稍后，我要立刻、马上！**

于是，我用字节新出的AI IDE ——Trae，开发了个本地的DeepSeek小应用。

不想看细节，要立刻、马上看Trae体验结论或者获取这个应用的，请直接拉到文章末尾。

---

# 题外话：原版Ollama挺怀旧的

好了，我们接着聊。

首先，我在本地搭建了个DeepSeek环境，富有年代感的命令行对话确实不再需要稍后，但是实话实说，用着确实挺怀旧的。

![](img/0ad7dc3e2b5116b43fd5294d1c816910.png)

但，一直怀旧也不是个事儿。

想到字节跳动有发布 AI 中文 IDE“Trae”，正好拿来练练手。

在Trae的官网`https://www.trae.ai/download`轻轻松松下载到了Mac版本的安装包。

有人可能要问，为什么不用Windows，因为Windows版本还在Coming soon。

![](img/e8c40d3b57fb50d1dfac14320483b0b0.png)

# 非常规的界面设计

一通常规的安装设置后，就得开始进入熟悉的AI辅助开发阶段了。

界面和Cursor、WindSurf差别不大，迁移到Trae的学习成本很低。

![](img/c36281e7805a6c8a3f1f961b29a05698.png)

在草草写了几句提示词后，我突然意识到，这个GUI的玩意儿本就是给DeepSeek设计的，为啥非得我亲自来写界面的提示词。

本着能偷懒的地方绝对不费力，解铃还须系铃人，不到黄河不死心…… 的作死风格。

我忍耐着“请稍后再试”的不爽，先去虚心问问DeepSeek，这个界面该如何设计。

![](img/a93bb889380a0fffab61e246b8c22fa6.png)

好吧，看在你噼里啪啦给我讲了一大堆内容，似乎还有些道理，但还是得你来给我总结下界面究竟该如何设计。

![](img/dd80b8a91ce0843ec389e6673e1c787b.png)

提示词还算是挺顺利的出来了，不枉我还是看好你。

# 开始用AI生成代码

接下来，介于DeepSeek服务器繁忙，那就稍后再说吧。开发的事情还是交给Trae来搞，我打打辅助就好。

直接把DeepSeek生成的界面粘贴到`Trae-Builder`模式的对话框中。

![](img/7887ab4c5dac6cbf98a76bf63f8adcce.png)

不出一会儿，代码已经生成好了，Trae也主动问询我是否需要对生成的代码进行审查。

不不不，能偷懒的地方绝不费力，我百分之两百相信Trae写的代码，直接合并运行吧。

![](img/ccca74f8c17f07472e3a4d9e0642386a.png)

即使，遇到了问题也可以交给Trae自己去解决，是AI的锅我绝对不背。

![](img/e9623d8c0c8497cc8573fb2efce83c7e.png)

很快，基本功能就完成了。

![](img/15cf95b62055a5ea825e49a13a8db17d.png)

# 补充缺失的功能

但是，历史记录呢？我要的历史记录去哪了？

Trae，你得给我把历史记录的代码给写出来！

如果。你要问我历史记录长啥样？那得祭出CV大法再次贴上DeepSeek对历史会话界面的描述。

![](img/a2832e4b7d376be67af3a5ddbe3a8ac6.png)

终于，这次的历史会话功能算是正常了。

![](img/e0c66da0ebb3a50187c929dd8d9ad0c5.png)

接着，一边测试，一边提出不同的要求。对Trae的AI IDE主打就一个：我不要你觉得，我要我觉得。

在反复与Trae拉锯多次后，终于，在没有手工修改一行代码的情况下，这个产品成了下面这个样子。

![](img/fb63b8b6e793938a810a4d0031e144ef.png)

# 最后总结

感谢在上面看我逼逼叨叨这么久。

我简单总结下关于Trae的体验：

1. Trae生成的代码相对比较聚焦具体功能，很少胡乱修改不相关的功能。这个必须要给Trae点个赞；

1. **Trae**能够基于某段对话进行相关代码的回撤，但回撤会导致相关 内容丢失 ；

1. 相比Windsurf和Cursor，Trae更加的财大气粗，至少现在还是免费使用Claude-3.5-sonnet ; 嘿嘿~
