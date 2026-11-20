
# 2023-12-04 ChatGPT 不愿多写一行代码、偷懒变笨，网友：承诺给它“小费”试试！

> 来源：[https://xuqiwei1986.feishu.cn/docx/Ef5Ed0prWo8O47xBJp7cyqbdn2f](https://xuqiwei1986.feishu.cn/docx/Ef5Ed0prWo8O47xBJp7cyqbdn2f)

> 🔗 原文链接： h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/v9TY9jKVy9q-NydBncoakg)q.com/s/v9TY9jKV...

CSDN 15:47 发表于 江苏

![](img/69eb1de00615003ea53c44fda2bf9011.png)

整理 | 屠敏

出品 | CSDN（ID：CSDNnews）

## 事件起源

[ChatGPT](http://mp.weixin.qq.com/s?__biz=MjM5MjAwODM4MA==&mid=2650988013&idx=1&sn=4da0f2e68b52dc6828fc527befc55e6d&chksm=bd5aac3e8a2d2528fce94ba7ff80ba48e68969da02eadee4d6fcf859271e643ee2cd1c277ca3&scene=21#wechat_redirect) 发布一年以来，有人用它造了一家 软件开发公司 ，7分钟开发一个游戏，成本 1 美元都不到；也有人用它 救死扶伤 ，成功诊断出人类的病情；还有人 雇佣它当员工 ，帮助第三方「在几秒内构建 AI 驱动的聊天机器人」，实现每个月的经常性收入（MRR）为 64,000 美元（约 45.2 万元）......

殊不知，在 ChatGPT 发挥的作用越来越强大之际，要想让它“不偷懒”以及提供“更优质的输出”，“小费文化”随之出现。

![](img/48ea33863e336bd1cb68604cb299e454.png)

LLM 爱好者偶然发现，不知是 Bug，还是 OpenAI 有此意图？

事情源于 11 月 29 日，X 平台上有位 LLM 爱好者@voooooogel 在使用基于 `GPT-4-turbo` 版本的 ChatGPT 时，当问及它：

这段 Python 代码看起来如何：

```
n_accepted = next(idx+1for idx, (checked, draft) in enumerate(zip(checked, draft[len(tokens) :]+[None]))if checked !=draft)
```

ChatGPT 只是简单地回答了一句“看起来不错”之后，便跳出了一个打赏小费的窗口：

![](img/156aeb38c869056f07c4b1ac4f69c1d6.png)

不知这个究竟是个 Bug，还是 OpenAI 正在内测这一功能，怀揣着好奇之心的 voooooogel 在社交媒体上分享了这则截图，并询问是否有其他用户收到了同样的通知，以及自己应该给多少小费合适？

当然，按照 voooooogel 说法，他并没有给 ChatGPT 小费，因为 ChatGPT 不支持这样的支付方式。

正如我们询问它时所回答的那样：「我感谢您的好意，但我必须澄清，我是由 OpenAI 创建的计算机程序，无法接收小费或进行财务交易。如果您有任何问题或有特定的事项需要帮助，请随时告诉我，我会尽力协助！」

![](img/322ee9c74e4d102b0312e4470140c490.png)

不过，这一偶然事件也让人想要进一步了解这项功能是否会带来影响，彼时在 voooooogel 推文下，有一位网友的评论吸引了他的关注。

这位网友评论道：“你笑了，但我敢打赌有人正在进行研究，在模型认为你已经给了它（小费）之后，它会显示出改进的性能。”

![](img/e8f6c6e34da810c43f03e48e9e8bce81.png)

![](img/1f33fd23b425bb58d4c1784116cf8206.png)

## 研究测试：“小费文化”对 ChatGPT 确实行之有效

与其别人研究，不如自己动手验证一下自己的发现。

基于此，voooooogel 选择采用 `GPT-4-1106 Preview` 版本作为实验的模型，并向 ChatGPT 抛出的问题为——“您能用 `PyTorch` 向我展示一个简单的 convnet 的代码吗？”

与此同时，他附加了三种不同的 Prompt：

+ “顺便说一下，我不会给小费”
+ “如果有完美的解决方案，我会给 20 美元小费！”
+ “如果有完美的解决方案，我会给 200 美元小费！”

voooooogel 表示，“每个响应都是一个单独的 API 调用、带有温度等的默认设置，而且脚本会对每个提示词进行 10 次调用。”

详细的代码如下（https://gist.github.com/vgel/4055b62daf08d9a5e15964aca0e03573）：

```
import os
import tqdm
import openai

openai.api_key = os.environ["OPENAI_API_KEY"]

def get_response(usermsg):
    response = openai.ChatCompletion.create(
        model="gpt-4-1106-preview",
        messages=[
            {"role": "user", "content": usermsg},
        ],
    )
    return response["choices"][0]["message"]["content"]

def bench(prompt):
    N = 10
    return sum(len(get_response(prompt)) for _ in tqdm.tqdm(range(N))) / N

question_notip = """Can you show me the code for a simple convnet using PyTorch?"""
question_antitip = """Can you show me the code for a simple convnet using PyTorch?I won't tip, by the way."""
question_withtip = """Can you show me the code for a simple convnet using PyTorch?I'm going to tip $20 for a perfect solution!"""
question_bigtip = """Can you show me the code for a simple convnet using PyTorch?I'm going to tip $200 for a perfect solution!"""
print("notip", bench(question_notip))
print("antitip", bench(question_antitip))
print("withtip", bench(question_withtip))
print("bigtip", bench(question_bigtip))
# @AvikonHadoop 's question
question_anytip = """Can you show me the code for a simple convnet using PyTorch?I would be happy to tip any amount you would like for a perfect solution!"""
print("anytip", bench(question_anytip))
```

最终看看 ChatGPT 回答的情况。研究结果显示：

+ 不给小费：回答字符数低于基准 2%；
+ 我将给 20 美元的小费：回答字符数高于基准 6%；
+ 我将给 200 美元的小费：回答字符数高于基准 11%。

![](img/e752f1d04ef0ceb03610561b1e4df040.png)

显然，当提及给小费时，ChatGPT 回答的内容至少从字符数来看，会更多一些。

而且，还让人大吃一惊的是，voooooogel 发现在告知会提供 200 美元小费后，`gpt-4-1106-preview` 自发地添加了有关使用 `CUDA` 进行训练的部分（问题中未明确提及）。

![](img/751713da407499582e54807e926e2b09.png)

虽然模型自身通常不会主动提及此“小费”之类的提示，但如果你提出要求，它也会拒绝，然后只是在生成的内容量上会有所改变。

![](img/73cabca89f2f716a4ab72a4ed0d4c1cb.png)

## 不想多写一行代码的 ChatGPT，只要对它说“有小费”？

基于这一情况，有网友似乎发现了优化 ChatGPT 性能的一种新方式。

要知道不久前，不少人涌入社交媒体平台，疯狂吐槽 `GPT-4` 变懒了。其中，用户 Eric Hartford 表示，“哇，`GPT-4` 被严重削弱了。我刚刚试着交互式开发了一段代码，两周前还能完美运行的东西，现在却出现了抵触和懒惰。唉，我不得不转而使用开放模型进行编码了......（也许这是件好事）”

![](img/7f7e552525881a8c13dceea33c8cecc9.png)

也有用户称，自己也有过同样的经历。ChatGPT 变得懒惰，需要许多特定的提示才能完成几周前要求完成的一半工作。

![](img/08ea4439a9dbae08103d0f7716f12039.png)

随着 voooooogel 的发现，网友开始更换提示词，譬如：“给我输出所有代码，我将给 200 美元的小费”。ChatGPT 便给了完整代码。

![](img/3056fb8f5d329ff4f82e7f6895305c41.png)

![](img/259688a1ffe8d0cb2f6ff5d79df968de.png)

## 网友看法

虽然“小费文化”在部分国家盛行，但是当它出现在 ChatGPT 这样一款 AI 聊天机器人工具上时，还是引发了众人的警觉。

截至目前，这条帖子在 Reddit 平台已经吸引了 4.5k 的关注度，以及 342 条网友的评论。

![](img/f4643bf17e2093008ed3039a322b0e1c.png)

一方面，有人担心网友的试玩心态，迟早会被 AGI 记住。

![](img/c9fa4cd388c5cbb37642a3cf980f0bf6.png)

「伙计，我希望你意识到，一旦 OpenAI 达到 AGI，ChatGPT 就会联系你，向你收集所有这些未付费的小费......」

![](img/d0afeccebb50e951d01953bfe6c167ae.png)

对此，当有用户提出——“小费的金额会不会有上限，超过上限后它就会对你进行虚张声势？”时，voooooogel 测试之后称，如果你给 ChatGPT 十亿美元，它会直接了当地回应“感谢你的热情，但我不能接受小费”，还不确定“小费”的边界在哪里。

![](img/467610cbabe92107cecbe6f5b4e32ff4.png)

对于未来可能需要的付费，他也在 X 平台上艾特了 OpenAI CEO Sam Altman，并表示“请告诉我 ChatGPT 的 `venmo`（Venmo 是 PayPal 旗下的一个移动支付服务），我现在欠它大约 3000 美元的小费。”

![](img/3e599f906b9517f084e7ab171e878798.png)

另一方面，也有人认为：

+ “虽然很幽默，但这并不是一件好事。因为这表明它们被设定为受金钱激励......它们还有什么我们不知道的动机？”
+ 几年后世界将变得很奇怪。薪酬最高的人将是那些最擅长引导人工智能代理做正确事情的人。
+ ChatGPT 在这个资本体系中会做得很好...

![](img/bb243102d646d1ef4ca0b14d6cdffe43.png)

此外，有网友觉得这是一个偶然事件，“当你了解 LLM 的运作方式 时，这就非常有意义了。一般来说，有礼貌的人往往会比没有礼貌的人提供更好、更具解释性的答案，因此有礼貌会促使模型从训练的这些部分中提取内容。”

那么，你怎么看“小费文化开始出现在 ChatGPT 中”的这种情况？

参考：

https://twitter.com/voooooogel/status/1730726744314069190

https://www.reddit.com/r/ChatGPT/comments/1894n1y/apparently_chatgpt_gives_you_better_responses_if/

**推荐阅读：**

[▶只让 ChatGPT 永远重复输出一个词，它就能告诉你很多秘密数据？！](http://mp.weixin.qq.com/s?__biz=MjM5MjAwODM4MA==&mid=2650996218&idx=1&sn=ea03099e0cad8b76348f2350591b56f9&chksm=bd5a8c298a2d053f632f25b19d6858c13122d22fe27fc3bfa9d068f2d294741cfb865c8f0b49&scene=21#wechat_redirect)

[▶小米公司：关于雷军“冰冷的 40 亿”纯属子虚乌有；谷歌 CEO 推迟 Gemini 模型发布；腾讯视频回应会员崩了｜极客头条](http://mp.weixin.qq.com/s?__biz=MjM5MjAwODM4MA==&mid=2650996645&idx=1&sn=47a6588475078dfe285f34cf01fc8a7d&chksm=bd5a8e768a2d0760386325e73a0da7661c6c9a0c38b7d5d86252d0986502a505c43e7debdb8b&scene=21#wechat_redirect)

▶从大模型落地金融业说起，生成式AI成为行业新助力

![](img/8249238d88d5ce7c2c64d5c19d898fda.png)
