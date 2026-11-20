
# 带着70页`Prompt`挑战`Claude`提示语平台新功能，完事儿我把报告扔了

> 来源：[https://agispace.feishu.cn/docx/LmrMdFplfoSqHwxqX7bcZElQnmg](https://agispace.feishu.cn/docx/LmrMdFplfoSqHwxqX7bcZElQnmg)

> 原创：AI沃茨

> 👸号:卡尔的AI沃茨

> 原文链接：https://mp.weixin.qq.com/s/OWBmm4ldvl6Kw63syrnyPg

> **动手学AI** 教程知识库 👉 learnprompt.pro 你想学的都在这里🎉

## “还有人会说自己不会写提示语吗？

替身使者之间是会互相吸引的🤩

`Claude()`这次上新，可以说是打到了我的心巴上了（老梗

开头还是用我擅长的一句话概括

### “`Claude`新功能支持在 `Anthropic` 控制台中生成、测试和评估提示，`Anthropic`特别强调了自动生成测试用例和比较不同模型输出的功能。

这跟我正在做的事情不谋而合。

一个月前，一份长度为 76+ 页，内含 1500+ 的 `Prompts` 论文横空出世，由 `OpenAI()`、微软等机构和大学联合发布。

报告内分析了 58 种 `Prompt Engineering()` 技术，涵盖多语言、多模态、`Agent`、模型评估、安全、对齐等主题。

其中最吸引我的当然是每天都会用到提示语(`Prompt`) 精读论文的过程中，我想收集一些在日常使用过程中能被记忆下来且随手就能用的启发性技巧，如：

+ `few-shot`｜给模型提供例子，让模型学习输出

+ 结构化输出｜`Claude` 喜欢 `XML` 标签，`GPT` 更偏好 `Markdown` 和 `JSON`

另一方面是想收藏开发过程中能快速复制的提示词模版，如

+ `CoT` (`Chain-of-Thought Prompting`)

+ `ToT` (`Tree of Thoughts`)

所以我读完了吗？Yes！

+ 排名前 20 的提示/`Prompt`技术：`Fewshots Learning` 断层式胜利！`Plan-and-Solve`/`Agent` 紧追其后

刚好`Claude`上新～

那今天我就带着在这1500+、58种 `prompt` 里面的佼佼者来给 `Claude` 上上强度！

目前这个功能可以在控制台 (`https://console.anthropic.com`) 里使用。

控制台提供了一个内置的 `prompt` 生成器，由 `Claude 3.5 Sonnet` 提供支持，我可以通过一句话描述我的任务（天天都用到的中英文翻译任务），并让`Claude`生成提示。

（有点像`GPTs`的创建过程）

这里我也把完整的提示词贴出来

```
You are tasked with translating text between English and Chinese, with a focus on producing translations that are particularly suitable for LLM (Large Language Model) researchers. This task requires not just accurate translation, but also an understanding of the technical language and style preferences common in the field of AI and machine learning research.

LLM researchers often use specific terminology and have a preference for concise, precise language. They may also be familiar with certain English terms even when working primarily in Chinese, so some technical terms might be best left untranslated or provided alongside their translations.

You will be given a piece of text and a direction for translation. The direction will be either &quot;English to Chinese&quot; or &quot;Chinese to English&quot;.

&lt;text&gt;
{{ENGLISH_TEXT}}
{{CHINESE_TEXT}}
&lt;/text&gt;

&lt;direction&gt;{{DIRECTION}}&lt;/direction&gt;

When translating, consider the following:
1. Maintain technical accuracy, especially for AI and ML-related terms.
2. Use concise language where possible without losing meaning.
3. For Chinese to English translations, use American English spelling and phrasing.
4. For English to Chinese translations, use Simplified Chinese characters.
5. When appropriate, keep important technical terms in English, followed by a Chinese explanation in parentheses.
6. Aim for a natural flow that would sound familiar to an LLM researcher in the target language.

Provide your translation within &lt;translation&gt; tags. After the translation, include a brief explanation of any challenging aspects of the translation or notable choices you made, enclosed in &lt;notes&gt; tags.

Here are two examples of how your output should be structured:

Example 1 (English to Chinese):
&lt;translation&gt;
我们使用了一个大型语言模型（Large Language Model，LLM）来生成文本。该模型经过了 fine-tuning（微调），以提高其在特定任务上的性能。
&lt;/translation&gt;
&lt;notes&gt;
Kept &quot;Large Language Model&quot; and &quot;fine-tuning&quot; in English with Chinese explanations, as these are common terms in the field. Used concise phrasing while maintaining technical accuracy.
&lt;/notes&gt;

Example 2 (Chinese to English):
&lt;translation&gt;
We implemented a novel attention mechanism to enhance the model&#x27;s ability to capture long-range dependencies. This significantly improved the perplexity scores on our benchmark dataset.
&lt;/translation&gt;
&lt;notes&gt;
Translated &quot;注意力机制&quot; as &quot;attention mechanism&quot; and &quot;困惑度分数&quot; as &quot;perplexity scores&quot;, as these are the standard terms used in English LLM research papers. Maintained a concise, technical style typical of research writing.
&lt;/notes&gt;

Now, please proceed with the translation task using the provided text and direction.
```

可以看到`Claude`生成的提示词真的是有点强，使用了`few-shot` 和`CoT()`等提示技巧，是一份非常完善的提示词！！！

（`prompt engineer` 不存在了？！）

甚至它还可以帮你方便的生成测试用例

这里我们上一波对比，我使用宝玉大佬分享的翻译提示词，跟`Claude` 生成提示词对比

【结果对比】

`Claude`生成提示词结果：

`GPT4o` + 手写提示词

这样直观一点：

大家会选左边还是右边呢？

虽然，在这个任务上，生成的提示词表现不错，

但我不是很服气， 提示语改写或者提示语生成功能并不少见。`GPT Store` 随便找一个排名靠前的 `GPTs` 也能做到类似结果， 没必要专门做个新页面。

不过既然是 PK 对决，只测单次输入有点不太公平。真实场景下，一个提示语应该是适用于一个场景里的多种情况。

比如我想让 `Claude` 扮演编程助手，我就会期望它能专业的回答我各类编程知识🧑‍💻。这时候我会收集这个场景遇到的真实输入来测试模型， 看看这个提示语有没有真本事。

那按照这样一个标准的流程来优化或者选择提示语需要三部曲：

## “收集测试数据整理成表 -> 上编程平台批量调用模型`API` -> 查看生成案例

测评`Prompt`需要的前置环节很多， 虽然现在 AI+code 让写代码变得很轻松， 但真正花时间的还是收集数据，。

毕竟你不能指望3条长得一模一样，就换了个名词的输入能反映出什么能力吧😏 这次 `Claude` 将这三部曲也整合到了控制台里

数据收集难搞？难办啊，那就别办了（乌鸦哥掀桌🐦💺）

我直接要求`Claude`使用“生成测试用例”功能， 帮我跳过第一步，

然后一键运行所有测试用例！再见了`Excel`、再见了`CSV`👋

当有了完整的测试用例和提示语后， 我在想能不能不从零生成新的提示语呢？之前我也存了不少好用的， 要不要一口气全部优化成高级形态

答案是当然可以！`Claude`可以随时创建提示的新版本并重新运行测试套件来快递迭代，同时还支持将多个提示的输出结果并排比较🚗🚗

到这一步的话，三部曲已经有两部不需要我担心了

我只需要冲杯咖啡， 等待模型生成结果， 慢慢看就行了～

没想到 `Claude` 连摸鱼🐟时间也不给我留

新功能还支持让主题专家（也就是我自己，看半天宣传视频还以为是`Claude`自动给呢）按照 5 分制对响应质量进行评分，想慢点迭代都不行。

这下我能说我的提示库能一个”打“十个了，

通过`Claude`这张融合卡，

把排名前5的模版跟我日常使用频率最高的前20个提示语一结合，

我短时间不再需要担心`GPT`变傻了

提示语们会教它做（人）模型

我还记得当时`GPTs`没有收获到预期效果的时候，

`GPT`被人吐槽过 “用心做模型，用脚做产品”

是不是模型做得足够好， 产品没有体现出来也没关系？

`Claude`给了我新的回复

这两次上新与以前只会狙击`GPT`模型不同，

从 `Projects` 项目文档问答 取代 `GPTs`，

到 `Artifacts` 代码预览分享，

再到提示语优化一站式平台🚉

它身上的`GPT`影子越来越少，

走出了一条自己的路！

不过，我还是想提一句啊

`Claude`你的网络环境能不能卡那么严啊！

想用你的好功能好难啊😩
