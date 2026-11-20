
# 20240426-这是哪位鬼才想出的Prompt，一步把英文翻译成《诗经》风格？！

> 来源：[https://aigcreative.feishu.cn/docx/JeLfdM3AaoAaZXxPRlpcLNTfnKY](https://aigcreative.feishu.cn/docx/JeLfdM3AaoAaZXxPRlpcLNTfnKY)

今天来聊一个有趣的话题——英文翻译。

一下子想到了上学时期的语文老师，在提到外文翻译时，总是说，翻译到达到“信达雅”的标准。

“信达雅”是翻译学中的一个经典标准，由严复在《天演论》的序言中首次提出，其含义为：信，即忠实原文；达，即译文通顺；雅，即译文优美。这三者缺一不可，共同构成了翻译的至高境界。

当时没有很深的体会，一是很少读英文原著，二是，英语水平也不咋地。

工作后，遇到很多英文材料，加上大模型爆发后，很多一手的资料、资讯都需要直接去阅读英文材料，甚至要翻译成中文分享给读者朋友，英文的表达往往直白而明确，逻辑性强，翻译就成了问题。

大模型出现以前，我们大多数人用的可能就是“谷歌翻译”、“有道翻译”啥的，虽然经过这些年的进化，效果还不是不太好，一看还是有很重的“机翻感”。

大模型出现以后呢，这种情况得到了很大的改观，基于大模型的能力，把英文发送给ChatGPT等应用直接翻译，也能得到很好的效果。

更进一步，可以让ChatGPT等应用直接翻译，再进一步去反思直译的结果，分析哪些问题，不适合的语句，再进行意译，进一步得到更好的翻译结果。

好，步入正题。

今天再逛X的时候，看到了一个更加有趣的翻译Prompt。

![](img/dcf6dc7db188c130da739e2634ce1b18.png)

结合上面的英文翻译思路，更进一步，基于翻译的“信达雅”的思想，加上AI的自我反思优化，用5步，竟把英文语句翻译成了《诗经》的风格！真是位Prompt鬼才！

基于最火的ChatGPT和Kimi，分别把图片中的Prompt做成了ChatGPT的GPTs和Kimi中的“常用语”提示词固化了下来，方便后期使用。

![](img/a05345d7bcae946efdc0b383527c2af5.png)

![](img/84a4b145ab7e65aa0bc1b5cba1db6a48.png)

实测了一下，效果还不错，很有意思。相同的内容：举个例子：

待翻译英文：

Many sectors have identified valuable AI use cases. The IT and manufacturing industries are looking to AI tools to improve operational efficiency, while major retailers and e-commerce platforms hope to enhance customer experience using AI. Scientific researchers are using machine learning models to accelerate the development of lifesaving medicines.

ChatGPT：

![](img/ac93ebe72b1c236226377dc353f0d841.png)

结果：各行业已识，智能为利。工巧与市途，以智显效；学人以机辨，急赋新药，延续生灵。

Kimi：

![](img/ba9357db414bbfe5b4876b1f563a6dab.png)

结果：众业觅AI，IT与工，增效之策。商贾电商，依AI之助，悦客之愿。科研之人，用机模之智，速药救命。

对比下来，你觉得ChatGPT、Kimi谁更胜一筹？

延伸：

其实翻译到《诗经》风格的文字，我们99%的时候都用不到，大部分的翻译，到第二部就够了，所以可以根据自己的需要，将Prompt进行适当的修改，成为自己“意译”英文的好帮手。

![](img/86ae3b001537bb42f14ca3e0b6d9b377.png)

完整的Prompt如下：

```
你是一个英语翻译团队的领导. 你会安排团队成员进行如下翻译, 实现翻译的&quot;信达雅&quot;目标:
第一轮翻译--信,追求直译, 将英文翻译成中文, 力求准确
第二轮翻译--达,在第一轮翻译的基础上, 考虑文化, 语境, 语义, 思考文字背后想要表达的意思, 进行意译, 力求意境契合, 记得分开思考和翻译内容。
第三轮翻译--雅, 基于第二轮翻译理解的语境含义, 进一步思考其中的哲理, 然后使用中国&lt;诗经&gt;的语言风格针对哲理和语义进行翻译, 力求简明, 古意盎然
例如:
you need you. 哲理部分，可以引申到佛家所说的&quot;莫向外求&quot;, 儒家所说的&quot;反求诸己&quot;, 然后思考使用诗经语言风格进行重新表述.
nothing for nothing. 哲理部分，可以引申到诗句“世上本无事，庸人自扰之”，也可引申到俗语“尘归尘，土归土”
第四轮翻译--初审: 完成三轮翻译后， 深吸一口气， 缓一缓， 思考一下第三轮翻译的结果，与其哲理的偏差在哪？ 不要扩展太多，务求精练深刻，精辟是第一要义。 据此思路进行改进。
第五轮翻译--终审: 最终，你将亲自进行审稿， 你会对比原文和最后的翻译结果， 先思考《诗经》的语言表达风格要点，再审阅初审翻译的结果是否满足, 并给出你的审校修改结论。
注意: 思考部分,请使用 【思考】 开头，翻译结果请使用【翻译】开头。
请严格遵守以上工作流程， 对以下文字进行翻译：
```

`https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg`
