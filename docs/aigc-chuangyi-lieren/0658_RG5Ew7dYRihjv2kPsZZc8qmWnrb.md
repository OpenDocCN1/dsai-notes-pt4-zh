# SD Prompt 基础

> 来源：[https://aigcreative.feishu.cn/docx/VlJZdbWtkochtLxzudYcOz59nbg](https://aigcreative.feishu.cn/docx/VlJZdbWtkochtLxzudYcOz59nbg)

> 作者： Jimmy Wong

> 网站：https://learningprompt.wiki/zh-Hans/

## 基本原则

介绍如何写 prompt 前，我们先来介绍下写 prompt 的基本原则。

### 原则一：prompt 不是越长越好

很多人刚学习 prompt 时，会觉得 prompt 越长越好，但实际上这是不对的。精准表达你的意图才是最重要的，而不是堆积非常多无用的词语。

另外，长度最好保持在 75 个 token（或约 60 个字）以内。

### 原则二：越重要的词放在靠前的位置

很多人在写 prompt 时，会较常忽略这一点。比如说，如果你想要生成一张猫的图片，那么你应该将「猫」这个词放在靠前的位置，而不是放在靠后的位置。

### 原则三：善用符号

+   使用逗号分隔：虽然 SDXL 在语意理解上已经有了很大的提升，但是在使用 prompt 时，还是建议使用逗号来分隔不同的意图。

+   使用 () 调整权重：你可以输入 (keyword:weight) 方式来控制关键词的权重，比如 (hight building: 1.2 ) 就意味着 hight building 的权重变高，如果填写的权重数小于 1，则意味着这个词的权重会变低，生成的图与这个词更不相关。

## 基本模板

了解完基本原则后，我们介绍下写 prompt 的模板，一般 prompt 会包含以下内容：

1.  **Subject**：因为主体是最重要的，所以主体一般会放在首位。

1.  **Environment**：环境包含：

+   主体周围的环境，比如说「在一片森林里」、「在一片草地上」等等。

+   灯光，比如「闪电」、「月光」等等。

+   天气，比如「下雨」、「下雪」等等。

1.  **Medium**：媒介可以是图片的拍摄媒介，如「相机」，亦或者承载的媒介，比如「油画」。

1.  **Style**：可以用 4W 记忆：

+   When：什么年代的风格？

+   Who：你想要谁的风格？（人或组织）

+   What：什么艺术类型的风格？或者艺术运动的风格？

+   Where：什么国家的风格？

> 如果你还学习过我的 Midjourney 的教程(opens in a new tab)，你会发现 SD 的 prompt 少了「构图」这一部分。这是因为，我认为构图最好使用其他方式输入给模型，比如 ControlNet 或 img2img 等方式通过图片传给模型（这些在中级篇会介绍），而不是直接写在 prompt 里。这样模型的输出会更符合预期一些。

## 学习 prompt 的方法

其实了解完 prompt 的基本原则和模板后，你就已经可以写 prompt 并生成图片了。但是，我认为绝大数人（包括我）其实很难一次就生成你脑海中满意的图片。原因是：

+   绝大多数人缺乏对美的认知：这是一个很大的问题，因为你不知道你想要的图片是什么样的，所以你也不知道该怎么写 prompt。

+   绝大多数人缺乏美术相关知识：即使你知道什么是美，但是你不知道该怎么表达出来，比如如果你根本不知道世界上有一名画家叫「梵高」，那你不可能会在 prompt 里写出「梵高风格」这样的词语。

那有没有解决方案呢？

我觉得最佳的方法是「先模仿后超越」。 先去看别人怎么写，然后根据自己的需求进行修改，并且通过控制变量的方式去多尝试，比如一次只改一个词，这样你就能知道每个词对生成图片的影响。

我推荐几个不错的学习网站：

+   Civitai(opens in a new tab)：大名鼎鼎的 C 站，这个应该是目前全球最大的 AI 图片社区，我在那学到了非常多东西。

+   PromptHero(opens in a new tab)：这也是一个很不错的 AI 图片社区，但感觉比 Civitai 小一些。

+   Learning Prompt(opens in a new tab)：虽然我当年写的教程是针对 Midjourney 的，但里面场景案例对于学习 Stable Diffusion 依然有参考意义。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>