
# 出海网站怎样挖掘对标网站的外链？怎样快速对上千个外链网站做初步分析？

> 来源：[https://xuqiwei1986.feishu.cn/docx/PYpLdgZWToJXBJxgSyacxZ0Wnrg](https://xuqiwei1986.feishu.cn/docx/PYpLdgZWToJXBJxgSyacxZ0Wnrg)

大家好，我是易焘，一直在深耕AI编程和出海网站。

最近上线了几个出海网站，其中一个网站开始有一些流量，为了争取搜索排名和流量，我开始上线内页和提交外链。经过一番摸索，我整理了一些哥飞老师分享过和ChatGPT建议的挖掘对标网站外链的方法，并尝试通过Claude Code开发外链分析工具，实现5分钟对上千个外链网站做初步分析。

本文包含以下内容：

一、出海网站怎样挖掘对标网站的外链？

1、通过Semrush查看对标网站的反向链接

2、通过Ahrefs Backlink Checker查看对标网站的外链

3、通过谷歌搜索的 `site` 命令查看对标网站的外链

4、通过查找特定网站来查看对标网站的外链

5、通过查找网站框架来查看对标网站的外链

二、怎样快速对上千个外链网站做初步分析？

第一步：调试API并导出外链数据

第二步：引导AI“吐”出执行任务的代码

第三步：把AI“吐”出来的代码封装成工具

第四步：使用封装好的工具执行更大数据集

第五步：把顺序执行改成并行提升10倍效率

## 一、出海网站怎样挖掘对标网站的外链？

哥飞老师曾经讲过：`title` 和 `h1` 等页面关键词确保你的网页能够进入关键词搜索的待排名范围里；外链帮助你进入谷歌搜索前10位置；网站体验帮助你确定最终的名次。可见外链对于一个网站的流量和排名有多重要！然而，我们上线网站后，怎样才能找到合适的外链给网站做外链呢？答案就藏在“学会抄”这三个字中。

我前面发布的文章：

[窥探顶级产品经理的出海网站，从中](https://t.zsxq.com/IbTvp)发现百万月活产品不曾对外公开的12个秘密

提到怎样通过Semrush查看刘小排老师的Rraphael AI网站的外链并做深入的分析。这确实是“学会抄”外链的常用方法之一。

今天，我们就整理几个查找对标网站外链的方法，只要你学会“抄”，外链多到根本发不完。

### 1、通过Semrush查看对标网站的反向链接

以前段时间爆火的 `nanobanana.ai` 为例。点击Semrush“域名概览”页面中的“反向链接”或左侧菜单“反向链接分析”，进入“反向链接”页面：

![](img/c980c501148e5a128ad3246f14009861.png)

可以看到这里罗列了这个网站的外链。如果你的对标网站在Semrush中查到的外链数量很大，你可以优先查看和分析显示“最佳”的这部分带 `Follow` 属性的外链：

![](img/1e19c558411756a3bff0e1dc0377e582.png)

除了带 `Follow` 属性的外链，`NoFollow` 属性的外链也是我们网站外链的重要来源。但是，如果你的对标网站的外链数量太过庞大，很难一一查看，你可以先通过查看“引荐域名”来看看给你的对标网站做背书的域名究竟有哪些：

![](img/765e645e46dc6768a0acc39a45b90f53.png)

有可能数量远小于外链的数量，那么你就可以围绕这些引荐域名做深入的分析和调查。

### 2、通过Ahrefs Backlink Checker查看对标网站的外链

另一个常用的查看外链的工具是Ahrefs Backlink Checker

> 链接：`https://ahrefs.com/backlink-checker/`

只需要输入你的对标网站的链接，即可查看：

![](img/60e14fef020e453c7108cc4bd255098e.png)

然而，免费版本只能看到20条外链，虽然不多，但同样可以作为我们“抄”和分析的目标。如果你想查看更多对标网站的外链，需要购买付费版本，但是每月价格不低。

### 3、通过谷歌搜索的 `site` 命令查看对标网站的外链

在查找对标网站外链的时候，有时候我会遇到这样的情况：一些刚上没多久的网站，不管通过Semrush还是通过Ahrefs Backlink Checker查看外链，往往找不到多少数据。那么，有没有什么办法可以看到这些新上线没多久的网站最新发布的外链呢？答案就隐藏在谷歌的搜索结果中。我们可以通过谷歌搜索以及谷歌的 `site` 命令查看有哪些网站上存在对标网站的链接。同样以 `nanobanana.ai` 为例，如果我们在谷歌中直接搜索：

> `intext:nanobanana.ai`

会有怎样的结果？

> 注意：这里加上双引号表示查找精准匹配的结果，`intext` 表示在网页的内容中查找，与之对应的，`intitle` 表示在网页的标题中查找。

![](img/cc6bdbeb0c06b8361f1cfb4850ded260.png)

可以看到谷歌搜索的结果非常多。然而，如果你的对标网站刚上线没多久的话，这里的结果就不一定有那么多，这样你就可以清晰地看到有哪些网站上添加了你的对标网站的链接，而这些网站往往有可能就是你的竞争对手早期发布外链的渠道。

如果查找结果确实太多，你可以通过这个命令做进一步搜索：

> `intext:nanobanana.ai -site:nanobanana.ai`

这个指令表示查找完整匹配“nanobanana.ai”这个词的结果并排查掉域名为 `nanobanana.ai` 的页面（注意 `site` 前面有“-”号）。这样谷歌的搜索结果就会减少一些，毕竟对标网站上面放自己的域名的页面不能算作外链。

![](img/ceed20966d1d8f3fa994c6784f4e97c2.png)

这样筛选之后，你可以对筛选后的结果进行分析，从而“抄”对标网站早期发布的外链。

### 4、通过查找特定网站来查看对标网站的外链

你还可以通过谷歌进一步深入分析对标网站是否在某些重要渠道发布了外链。比如Reddit：

> `intext:nanobanana.ai site:reddit.com`

你可以通过这个命令查看Reddit上有哪些页面在推广你的对标网站，然后“抄”对标网站怎样在Reddit上做推广，你甚至可以学习他的推广文案和推广思路。

![](img/7df5ba2435fa2d6cdcd0f57734918e54.png)

再比如Hacker News、ProductHunt和V2EX：

> `intext:nanobanana.ai site:producthunt.com`

> `intext:nanobanana.ai site:producthunt.com`

> `intext:nanobanana.ai site:v2ex.com`

查看你的对标网站是否在Hacker News、ProductHunt或V2EX这些重要渠道做推广。

![](img/e587797cb57a02b88c9e26f84c2c8bd9.png)

![](img/2ed022c329590f7b3e061d544f15ca10.png)

![](img/0dae0a49c920073bf2e2c4ebcf897065.png)

可以看到 `nanobanana.ai` 并没有在ProductHunt上做推广，这个需要进一步确认是否属实以及背后原因。

再比如主流的社交媒体：

> `nanobanana.ai site:youtube.com`

> `nanobanana.ai site:pinterest.com`

> `nanobanana.ai site:vk.com`

查看你的对标网站是否在在主流社交媒体做推广：

![](img/19346870649e49880ee71113b49ee193.png)

![](img/305a2a8a9e4c356a6dd0812e3dac69d1.png)

![](img/9101e78e623b9371249eea45aff6afca.png)

可以看到Youtube、Pinterest和VK平台上都有 `nanobanana.ai` 的身影。但是，你需要注意一点，你看到的这些搜索结果中的外链不一定全部是你的对标网站自己发布的，有些也可能是他们的用户或其他博主主动发布的。

### 5、通过查找网站框架来查看对标网站的外链

如果你对海外网站有一定了解，一定会知道国外有很多网站并非自建网站，而是采用某个建站平台或网站框架搭建的，比如Shopify、WordPress、Wix等等。这些网站中不乏有一些流量很大权重不小的网站，有些甚至不惜花费重金打Facebook广告和谷歌广告。更重要的是，这些网站往往有留言板块或产品评论，那么，我们也可以尝试找到这些网站，尤其是对标网站已经留过带链接留言或评论的网站，从而给我们增加更多外链渠道。为了能更全面地挖掘这些网站的外链，我把这个问题抛给ChatGPT，ChatGPT做了很详细的回复：

![](img/82d3a81a36b07e1c7b9c57b3e95d0d53.png)

可以看到我们可以通过Builtwith等工具查找出Shopify或WordPress等框架的网站，还可以通过更具体的搜索指令在谷歌中查找对标网站是否有在Shopify、WordPress、Wix等平台留下推广链接。

以上这些挖掘对标网站外链的方法，大家可以根据实际情况结合者使用。只要“学会抄”，出海网站的外链多到根本发不完！

## 二、怎样快速对上千个外链网站做初步分析？

前面我整理了几个查找对标网站外链的方法，只要你学会“抄”，外链多到根本发不完。然而，有时我们查找出对标网站的外链数量很大，有些多到几百上千个，想要一个个分析，难度很大。那么，我们有没有什么办法可以提高分析外链的效率呢？这就是我们接下来要探讨的一个话题。

去年我在赚钱发布过一篇精华文章：

怎样通过ChatGPT编程开发价值百万的亚马逊选品分析工具？

提到一个思路：

> 既然Code Interpreter具有自行编写代码、运行代码和调试代码来解决某个特定任务的能力，我们是否可以先让ChatGPT调用Code Interpreter尝试解决某一个它能完成的任务？任务执行完成后，意味着ChatGPT已经编写好一个在临时环境中能正常执行任务的Python程序，我们是否可以让ChatGPT学习这个Python的执行方案来编写出我们需要的代码，再在我们的本地电脑上执行数据量更大或者更加复杂的同类任务？

现在AI编程相较一年前已经有了突飞猛进的发展，我们能否把一年多前的这个思路运用到外链分析中呢？

> 让AI先对一个比较小的数据集进行处理，处理过程中AI已经编写好并验证好处理数据的Python代码，再让它学习这个已经能够完成任务的Python执行方案，重构成一个可复用的图形化数据分析工具，从而实现对更大数据集任务的处理。

### 第一步：调试API并导出外链数据

我们仍然以 `nanobanana.ai` 为例。先从semrush中导出这个网站的引荐域名，选择导出一个 `csv` 文件：

![](img/6845195b0c99a37d7356726c8e5c6db5.png)

为了对外链的网站进行分析，我们还需要先按小排老师在深海圈中提到的方法调试好API。为了节省时间，我直接使用我已经调试好的Gemini 2.5 Flash Lite的API，准备API文档和API四要素——API Key、API示例代码、API示例输入和API示例输出：

![](img/16e2b426bbdbd5d72fe8725e6e265cc4.png)

API调试结果请查看下面代码文件：

```
            &quot;url&quot;: &quot;https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg&quot;

            &quot;url&quot;: &quot;https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg&quot;
```
