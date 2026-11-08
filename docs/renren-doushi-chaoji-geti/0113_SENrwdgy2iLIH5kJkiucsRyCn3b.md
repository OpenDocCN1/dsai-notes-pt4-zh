# 出海网站怎样挖掘对标网站的外链？怎样快速对上千个外链网站做初步分析？

> 来源：[https://xuqiwei1986.feishu.cn/docx/PYpLdgZWToJXBJxgSyacxZ0Wnrg](https://xuqiwei1986.feishu.cn/docx/PYpLdgZWToJXBJxgSyacxZ0Wnrg)

大家好，我是易焘，一直在深耕AI编程和出海网站。

最近上线了几个出海网站，其中一个网站开始有一些流量，为了争取搜索排名和流量，我开始上线内页和提交外链。经过一番摸索，我整理了一些哥飞老师分享过和ChatGPT建议的挖掘对标网站外链的方法，并尝试通过Claude Code开发外链分析工具，实现5分钟对上千个外链网站做初步分析。

本文包含以下内容：

一、出海网站怎样挖掘对标网站的外链？

1、通过Semrush查看对标网站的反向链接

2、通过Ahrefs Backlink Checker查看对标网站的外链

3、通过谷歌搜索的site命令查看对标网站的外链

4、通过查找特定网站来查看对标网站的外链

5、通过查找网站框架来查看对标网站的外链

二、怎样快速对上千个外链网站做初步分析？

第一步：调试API并导出外链数据

第二步：引导AI“吐”出执行任务的代码

第三步：把AI“吐”出来的代码封装成工具

第四步：使用封装好的工具执行更大数据集

第五步：把顺序执行改成并行提升10倍效率

# 一、出海网站怎样挖掘对标网站的外链？

哥飞老师曾经讲过：title和h1等页面关键词确保你的网页能够进入关键词搜索的待排名范围里；外链帮助你进入谷歌搜索前10位置；网站体验帮助你确定最终的名次。可见外链对于一个网站的流量和排名有多重要！然而，我们上线网站后，怎样才能找到合适的外链给网站做外链呢？答案就藏在“学会抄”这三个字中。

我前面发布的文章：

[窥探顶级产品经理的出海网站，从中](https://t.zsxq.com/IbTvp)发现百万月活产品不曾对外公开的12个秘密

提到怎样通过Semrush查看刘小排老师的Rraphael AI网站的外链并做深入的分析。这确实是“学会抄”外链的常用方法之一。

今天，我们就整理几个查找对标网站外链的方法，只要你学会“抄”，外链多到根本发不完。

## 1、通过Semrush查看对标网站的反向链接

以前段时间爆火的 nanobanana.ai 为例。点击Semrush“域名概览”页面中的“反向链接”或左侧菜单“反向链接分析”，进入“反向链接”页面：

![](img/c980c501148e5a128ad3246f14009861.png)

可以看到这里罗列了这个网站的外链。如果你的对标网站在Semrush中查到的外链数量很大，你可以优先查看和分析显示“最佳”的这部分带follow属性的外链：

![](img/1e19c558411756a3bff0e1dc0377e582.png)

除了带Follow属性的外链，NoFollow属性的外链也是我们网站外链的重要来源。但是，如果你的对标网站的外链数量太过庞大，很难一一查看，你可以先通过查看“引荐域名”来看看给你的对标网站做背书的域名究竟有哪些：

![](img/765e645e46dc6768a0acc39a45b90f53.png)

有可能数量远小于外链的数量，那么你就可以围绕这些引荐域名做深入的分析和调查。

## **2、通过Ahrefs Backl**ink Checker查看对标网站的外链

另一个常用的查看外链的工具是Ahrefs Backlink Checker

> 链接：https://ahrefs.com/backlink-checker/

只需要输入你的对标网站的链接，即可查看：

![](img/60e14fef020e453c7108cc4bd255098e.png)

然而，免费版本只能看到20条外链，虽然不多，但同样可以作为我们“抄”和分析的目标。如果你想查看更多对标网站的外链，需要购买付费版本，但是每月价格不低。

## 3、通过谷歌搜索的site命令查看对标网站的外链

在查找对标网站外链的时候，有时候我会遇到这样的情况：一些刚上没多久的网站，不管通过Semrush还是通过Ahrefs Backlink Checker查看外链，往往找不到多少数据。那么，有没有什么办法可以看到这些新上线没多久的网站最新发布的外链呢？答案就隐藏在谷歌的搜索结果中。我们可以通过谷歌搜索以及谷歌的site命令查看有哪些网站上存在对标网站的链接。同样以 nanobanana.ai 为例，如果我们在谷歌中直接搜索：

> "intext:nanobanana.ai"

会有怎样的结果？

> 注意：这里加上双引号表示查找精准匹配的结果，“intext”表示在网页的内容中查找，与之对应的，“intitle”表示在网页的标题中查找。

![](img/cc6bdbeb0c06b8361f1cfb4850ded260.png)

可以看到谷歌搜索的结果非常多。然而，如果你的对标网站刚上线没多久的话，这里的结果就不一定有那么多，这样你就可以清晰地看到有哪些网站上添加了你的对标网站的链接，而这些网站往往有可能就是你的竞争对手早期发布外链的渠道。

如果查找结果确实太多，你可以通过这个命令做进一步搜索：

> "intext:nanobanana.ai" -site:nanobanana.ai

这个指令表示查找完整匹配“nanobanana.ai”这个词的结果并排查掉域名为nanobanana.ai的页面（注意“site”前面有“-”号）。这样谷歌的搜索结果就会减少一些，毕竟对标网站上面放自己的域名的页面不能算作外链。

![](img/ceed20966d1d8f3fa994c6784f4e97c2.png)

这样筛选之后，你可以对筛选后的结果进行分析，从而“抄”对标网站早期发布的外链。

## 4、通过查找特定网站来查看对标网站的外链

你还可以通过谷歌进一步深入分析对标网站是否在某些重要渠道发布了外链。比如Reddit：

> "intext:nanobanana.ai" site:reddit.com

你可以通过这个命令查看Reddit上有哪些页面在推广你的对标网站，然后“抄”对标网站怎样在Reddit上做推广，你甚至可以学习他的推广文案和推广思路。

![](img/7df5ba2435fa2d6cdcd0f57734918e54.png)

再比如Hacker News、ProductHunt和V2EX：

> "intext:nanobanana.ai" site:producthunt.com

> "intext:nanobanana.ai" site:producthunt.com

> "intext:nanobanana.ai" site:v2ex.com

查看你的对标网站是否在Hacker News、ProductHunt或V2EX这些重要渠道做推广。

![](img/e587797cb57a02b88c9e26f84c2c8bd9.png)

![](img/2ed022c329590f7b3e061d544f15ca10.png)

![](img/0dae0a49c920073bf2e2c4ebcf897065.png)

可以看到nanobanana.ai并没有在ProductHunt上做推广，这个需要进一步确认是否属实以及背后原因。

再比如主流的社交媒体：

> "nanobanana.ai" site:youtube.com

> "nanobanana.ai" site:pinterest.com

> "nanobanana.ai" site:vk.com

查看你的对标网站是否在在主流社交媒体做推广：

![](img/19346870649e49880ee71113b49ee193.png)

![](img/305a2a8a9e4c356a6dd0812e3dac69d1.png)

![](img/9101e78e623b9371249eea45aff6afca.png)

可以看到Youtube、Pinterest和VK平台上都有nanobanana.ai的身影。但是，你需要注意一点，你看到的这些搜索结果中的外链不一定全部是你的对标网站自己发布的，有些也可能是他们的用户或其他博主主动发布的。

## 5、通过查找网站框架来查看对标网站的外链

如果你对海外网站有一定了解，一定会知道国外有很多网站并非自建网站，而是采用某个建站平台或网站框架搭建的，比如Shopify、WordPress、Wix等等。这些网站中不乏有一些流量很大权重不小的网站，有些甚至不惜花费重金打Facebook广告和谷歌广告。更重要的是，这些网站往往有留言板块或产品评论，那么，我们也可以尝试找到这些网站，尤其是对标网站已经留过带链接留言或评论的网站，从而给我们增加更多外链渠道。为了能更全面地挖掘这些网站的外链，我把这个问题抛给ChatGPT，ChatGPT做了很详细的回复：

![](img/82d3a81a36b07e1c7b9c57b3e95d0d53.png)

可以看到我们可以通过Builtwith等工具查找出Shopify或WordPress等框架的网站，还可以通过更具体的搜索指令在谷歌中查找对标网站是否有在Shopify、WordPress、Wix等平台留下推广链接。

以上这些挖掘对标网站外链的方法，大家可以根据实际情况结合者使用。只要“学会抄”，出海网站的外链多到根本发不完！

# 二、怎样快速对上千个外链网站做初步分析？

前面我整理了几个查找对标网站外链的方法，只要你学会“抄”，外链多到根本发不完。然而，有时我们查找出对标网站的外链数量很大，有些多到几百上千个，想要一个个分析，难度很大。那么，我们有没有什么办法可以提高分析外链的效率呢？这就是我们接下来要探讨的一个话题。

去年我在赚钱发布过一篇精华文章：

怎样通过ChatGPT编程开发价值百万的亚马逊选品分析工具？

提到一个思路：

> 既然Code Interpreter具有自行编写代码、运行代码和调试代码来解决某个特定任务的能力，我们是否可以先让ChatGPT调用Code Interpreter尝试解决某一个它能完成的任务？任务执行完成后，意味着ChatGPT已经编写好一个在临时环境中能正常执行任务的Python程序，我们是否可以让ChatGPT学习这个Python的执行方案来编写出我们需要的代码，再在我们的本地电脑上执行数据量更大或者更加复杂的同类任务？

现在AI编程相较一年前已经有了突飞猛进的发展，我们能否把一年多前的这个思路运用到外链分析中呢？

> 让AI先对一个比较小的数据集进行处理，处理过程中AI已经编写好并验证好处理数据的Python代码，再让它学习这个已经能够完成任务的Python执行方案，重构成一个可复用的图形化数据分析工具，从而实现对更大数据集任务的处理。

## 第一步：调试API并导出外链数据

我们仍然以nanobanana.ai为例。先从semrush中导出这个网站的引荐域名，选择导出一个csv文件：

![](img/6845195b0c99a37d7356726c8e5c6db5.png)

为了对外链的网站进行分析，我们还需要先按小排老师在深海圈中提到的方法调试好API。为了节省时间，我直接使用我已经调试好的Gemini 2.5 Flash Lite的API，准备API文档和API四要素——API Key、API示例代码、API示例输入和API示例输出：

![](img/16e2b426bbdbd5d72fe8725e6e265cc4.png)

API调试结果请查看下面代码文件：

```
**# Google****: Gemini 2****.5 Flash L****ite

## AP****I文档
https:**//openrouter.ai/google/gemini-2.5-flash-lite

## API Key
sk-or-v1-990XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

## API示例代码
curl https://openrouter.ai/api/v1/chat/completions \
  -H &quot;Content-Type: application/json&quot; \
  -H &quot;Authorization: Bearer $OPENROUTER_API_KEY&quot; \
  -d &#x27;{
  &quot;model&quot;: &quot;google/gemini-2.5-flash-lite&quot;,
  &quot;messages&quot;: [
    {
      &quot;role&quot;: &quot;user&quot;,
      &quot;content&quot;: [
        {
          &quot;type&quot;: &quot;text&quot;,
          &quot;text&quot;: &quot;What is in this image?&quot;
        },
        {
          &quot;type&quot;: &quot;image_url&quot;,
          &quot;image_url&quot;: {
            &quot;url&quot;: &quot;https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg&quot;
          }
        }
      ]
    }
  ]
}&#x27;

## API示例输入
{
  &quot;model&quot;: &quot;google/gemini-2.5-flash-lite&quot;,
  &quot;messages&quot;: [
    {
      &quot;role&quot;: &quot;user&quot;,
      &quot;content&quot;: [
        {
          &quot;type&quot;: &quot;text&quot;,
          &quot;text&quot;: &quot;What is in this image?&quot;
        },
        {
          &quot;type&quot;: &quot;image_url&quot;,
          &quot;image_url&quot;: {
            &quot;url&quot;: &quot;https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg&quot;
          }
        }
      ]
    }
  ]
}

## API示例输出
{
    &quot;id&quot;: &quot;gen-1762075488-9VB7Z0IFUQ1A412O9YPJ&quot;,
    &quot;provider&quot;: &quot;Google&quot;,
    &quot;model&quot;: &quot;google/gemini-2.5-flash-lite&quot;,
    &quot;object&quot;: &quot;chat.completion&quot;,
    &quot;created&quot;: 1762075488,
    &quot;choices&quot;: [
        {
            &quot;logprobs&quot;: null,
            &quot;finish_reason&quot;: &quot;stop&quot;,
            &quot;native_finish_reason&quot;: &quot;STOP&quot;,
            &quot;index&quot;: 0,
            &quot;message&quot;: {
                &quot;role&quot;: &quot;assistant&quot;,
                &quot;content&quot;: &quot;This is an image of a wooden boardwalk that runs through a grassy meadow. The meadow is very green and lush, with tall grass that reaches up to the top of the boardwalk in some places. In the distance, there are trees and a clear blue sky with some wispy clouds. The overall scene is peaceful and idyllic.&quot;,
                &quot;refusal&quot;: null,
                &quot;reasoning&quot;: null
            }
        }
    ],
    &quot;usage&quot;: {
        &quot;prompt_tokens&quot;: 3360,
        &quot;completion_tokens&quot;: 65,
        &quot;total_tokens&quot;: 3425,
        &quot;prompt_tokens_details&quot;: {
            &quot;cached_tokens&quot;: 0
        },
        &quot;completion_tokens_details&quot;: {
            &quot;reasoning_tokens&quot;: 0,
            &quot;image_tokens&quot;: 0
        }
    }
}
```

为了确保Claude Code快速完成我们的任务，我们先缩小数据量，只使用5条记录做测试：

![](img/1b006f83101a5bf6502fae1768554538.png)

## 第二步：引导AI“吐”出执行任务的代码

接下来，我们给Claude code下达命令：

![](img/8479efdb7236171de2519803308a1883.png)

让它通过我们给定API，编写一个Python程序来实现依次对引荐域名信息的查询。注意，指令中我添加了一句“请先编写一个Python文件，然后通过执行这个文件进行引荐域名的查询。”这是为了确保Claude Code编写一个它在执行过程中使用的代码，避免它直接在编辑器环境中自行完成代码编写并直接运行完成任务，而没有把代码“吐”出来给我们。很快Claude code自行编写了一个Python程序并自行运行代码，过程中，我看到他遇到错误后会重新修改代码并再次运行。最终它通过执行它编写好的Python程序完成文件中5个域名的网站信息查询：

![](img/6232f27a0dd08dea22a760d96490e0b3.png)

并保存了一个新的csv文件：

![](img/33199b231830904bf032cbca576feb43.png)

打开可以看到程序确实完成了这5个网站的查询，更新了“网站类型”、“网站简介”、“反链方法”、“提交入口”、“是否登录”、“是否付费”这些信息，其中一个网站还找到了它的提交链接：

> https://ainavtool.com/submit-tool

我们回头看一下Claude code编写的程序，可以看到代码有使用API查询网站信息的逻辑，而且Claude code还按照我们的需求构造了一个Prompt：

![](img/9198627d18ca50978305718189884132.png)

## 第三步：把AI“吐”出来的代码封装成工具

接下来，我们让Claude code给这个Python程序增加一个界面：

![](img/6d85566a5d903db56b3615f01934cb49.png)

很快，它完成了代码修改，并自动重新运行了代码，打开了一个不算太难看的界面：

![](img/1d5684745a1fbf6015fb11a429a1575e.png)

为了验证这个界面的工具能否正常使用，我们重新测试一下前面的小数据集：

![](img/047125028277eb8a9078a53872111dea.png)

![](img/c39ab26c80f841cb3e39e32411024bbb.png)

![](img/bc36c0ac173a82de658c902fede2cf60.png)

测试结果正常。

接下来，我们让Claude把Python代码打包成一个可以直接执行的exe文件：

![](img/f65f7e0601e3457c167dd8f371bef2a9.png)

很快，Claude Code帮我们把代码打包成一个exe文件：

![](img/29032778d4cb07219a22632e58f7b20a.png)

## 第四步：使用封装好的工具执行更大数据集

有了这个工具，我们可以执行更大数据量，来做外链域名分析了。我们试试用这个工具跑一跑正式的外链数据，一共有656多条记录：

![](img/4e6792b54feb21ee690f6b3fb4f53ab2.png)

经过将近一个小时的自动执行，这个工具终于帮我完成了所有引荐域名的分析：

![](img/a4a4181dc7377634e15cbe3e5489f477.png)

成功执行了650个引荐域名的查询分析，有6条记录出现失败。最终我们得到这样一份文件：

![](img/6da6829d2399f1588f2bd4fcef639f63.png)

通过这份文件，我们可以清晰地看到每个引荐域名究竟属于什么类型以及它们具体是什么？可以通过什么方式提交外链？部分记录还帮我们查询到提交外链的入口。有了这份数据，我们是不是就可以更加直观地对这些外链网站做好预先判断，并决定先分析哪些、先提交哪些？

## 第五步：把顺序执行改成并行提升10倍效率

接下来，我们尝试做进一步探讨：是不是可以把这些外链从按顺序的查询分析改成并行任务的查询分析？这样，说不定650条记录的查询就不需要一个小时，可能只需要几分钟。

接下来，我们就试试把这个想法变成一个进一步提升10倍效率的外链分析工具。

我们先在前面Claude code编写的代码上，继续完善这个工具。先让Claude Code把工具中硬编码的API Key改成在工具界面配置：

![](img/fa5ce76970dd2f336ab6d12c472523da.png)

很快，Claude Code修改好这个功能，在工具页面中增加了API Key配置：

![](img/edcc5b9cd85661c018af94b1a65954f7.png)

我们重新测试一下，功能正常：

![](img/3e3cfeacfd9886a6275e6dda1d024b72.png)

接下来，我们和Claude Code一起探讨一个问题，把工具原来分析引荐域名网站采用的顺序执行改成并列执行，以提高分析效率：

![](img/ba21636f0b66d62db2e47635c72a373a.png)

Claude Code做了详细的分析，然后给我们提供了几个方案：

![](img/14c6ded56deeb1b574f39b2ee152d8e8.png)

为了后续方便和灵活，我们选择可以灵活配置并行的数量的方案4：

![](img/abb336def51753f384a21a1344342717.png)

很快，Claude Code调整好了代码，然而，我运行这个Python代码时出现错误。不过没有关系，我们把错误粘贴给Claude Code即可，并且，我们让他修复好代码后自行运行，这样运行时有错误的话，它会自行重新做修复。

![](img/2f68ad4f2b0fafff33eae0e7684b768d.png)

Claude Code很轻松就修改好这个问题。现在，可以看到工具界面中增加了并发请求数的设置选项，支持5到20个并发：

![](img/5df54947b6200956526000944a12164f.png)

我们再次测试一下小数据集，工具功能测试正常：

![](img/d5aa5a3b0b932d854f036b3fd762fe5b.png)

接下来，我们让Claude把Python代码打包成一个可以直接执行的exe文件：

![](img/dde86f41550c866db4640f8006ace3f8.png)

很快，Claude Code帮我们重新把代码打包成一个exe文件。

接下来，我们使用这个带并发功能的外链分析工具分析更大数据量的外链网站。这次，我们导出deevid.ai这个网站的引荐域名，导出的数据表中有1374条记录：

![](img/0436281814720f67b985f3beff0777bc.png)

我们在外链分析工具中输入API Key，把并发请求数设置为10，选择刚刚导出的deevid.ai的引荐域名文件：

![](img/8b81401ba8b87cd61696d1a29360b2e7.png)

点击“开始外链分析”：

![](img/1bad6d0464d12aaeaf99b8bf1e2375be.png)

我特意记录了时间，结果，这个外链分析工具只花了5分10秒就完成了对1374个外链网站的分析：

![](img/488f87f173e4fda8a8d095465b97509f.png)

成功执行了1374个引荐域名的查询分析，有14条记录出现失败。如果我们把并发请求数设置为20，有可能我们只需要两分多钟就可以完成对上千个外链网站的分析。

最终我们得到这样一份文件：

![](img/a50ba97bd0098f4331ae09244c5aaed3.png)

通过这份文件，我们可以清晰地看到每个引荐域名究竟属于什么类型以及它们具体是什么？可以通过什么方式提交外链？部分记录还帮我们查询到提交外链的入口。

我们查看一下Gemini 2.5 Flash Lite的Token消耗费用：

![](img/b9fb5867e7121cfcac880cd8619ccd51.png)

结果，前一天执行分析656条记录只花了0.06美金，而后一天执行分析1374条记录只花了0.124美金，折合人民币0.88元。

我仔细看了一下工具分析出来的结果数据，发现数据当中的“网站类型”、“网站简介”和“反链方法”对我们快速预判一个网站有非常直观的参考价值，而“提交入口”并非所有网站都能正确获取，这和当前使用的AI模型的不稳定表现以及我们使用的Prompt有关，而且我所使用的Gemini 2.5 Flash Lite不一定具备联网能力。然而，这篇文章只是给大家提供一个思路，而且在可预见的未来，大语言模型的能力将会越来越强，而我们这个外链分析工具的分析结果也一定会越来越准。

只需5分钟，不到1元，就可以帮你完成上千个外链网站的初步分析，让你的出海网站在提交外链时不再盲目。

文末，祝大家龙哥盟！