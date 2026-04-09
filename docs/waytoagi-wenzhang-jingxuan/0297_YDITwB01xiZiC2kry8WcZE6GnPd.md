# 🌈通往AGI之路-分享会

> 来源：[https://waytoagi.feishu.cn/docx/GMAudav3wodMxHxwGtWcN4QSnHh](https://waytoagi.feishu.cn/docx/GMAudav3wodMxHxwGtWcN4QSnHh)

# 深入浅出理解AI

+   从有趣的应用到核心原理

自我介绍：西琦AJ，产品经理，创建http://waytoagi.com/开源知识库

🎯  愿景和目标

> 要实现通用人工智能（AGI）也许有很长的路要走，

> 我们的目标是让每个人的学习过程少走弯路，让更多的人因 AI 而强大。

# 目录

1.  有趣的AI案例

1.  AI的原理

1.  Diffusion原理和案例

1.  什么是Agent

# 有趣的AI案例 - 引言

+   让我们先从一些有趣的AI数据和案例开始，看看AI在现实生活中的应用。

+   2023年上半年生成式AI TOP50 a16z发布

![](img/d4ea04290cda6fa7d82235e309b8ebe7.png)

## GPT聊天和角色扮演

+   GPT可以进行自然语言对话，如同与真人聊天。

大体分两种表现形式：chat（聊天框），CharacterAI（角色扮演）

数据：[AI 聊天机器人TOP13](https://dnipkggqxh.feishu.cn/wiki/CJmHwFBsVi1xehkO7oUcKJl8nif)

![](img/f01a8c059e16afb048e0c187de2a8879.png)

![](img/5e6f7262d907866884e2e90cb2952e36.png)

![](img/a5ca6314d21bfad0b0f5265fb8decee2.png)

![](img/dfb1b6df607db708988a02def53795f0.png)

## GPT使用场景-现场操作演示案例

1.  内容生成：GPT 可以生成文章、故事、诗歌、歌词等内容。

+   演示  https://chat.openai.com/

+    https://bard.google.com/extensions

+   https://claude.ai/

1.  聊天机器人：GPT 可以作为聊天机器人的后端，提供自然的对话体验。演示 感觉英语老师要失业了……这AI也太强了_哔哩哔哩_bilibili

1.  问答系统：GPT 可以用于问答系统，为用户提供准确的答案。

1.  文本摘要：GPT 可以生成文本的摘要或概述。演示

1.  机器翻译：虽然 GPT 不是专门为机器翻译设计的，但它在这方面也有不错的表现。

1.  群聊总结：[实践：群总结工具](https://waytoagi.feishu.cn/wiki/AT1BwDM5MiwNiAkfWTqc0vxtnGh)

1.  代码生成：GPT-3 和其后续版本已经被证明可以生成代码片段，甚至帮助开发者解决编程问题。

1.  教育：GPT 可以用于教育领域，帮助学生解答问题或提供学习材料。

1.  浏览器插件：webpilot

1.  PDF对话：演示 www.chatpdf.com

![](img/04a0518b9935637927b9c5bfe9c3bef8.png)

1.  PPT生成：[办公：高效做 PPT](https://waytoagi.feishu.cn/wiki/RWZPwT0ETiqVwIkB8qQcrgRwnog)

1.  音视频提取总结：https://bibigpt.co/r/AJ

1.  播客总结：https://podwise.xyz/dashboard/trending

1.  生成脑图：https://xmind.ai/editor/

![](img/b55ca70e4d680cbdc020841fac950e43.png)

---

## Prompt

+   所有的背后都离不开这个词

+   prompt：提示词，通过特定的提示，GPT可以生成特定的内容。

### 提示词相关框架

![](img/984fdf0868727eb649f73665864d79c4.png)

### 提示词相关网站

多维表格（JuzDbhAiea0KhUscE9EcT5yRnK3_tbloIm4IEPxpOMrs）

### 自己学习写prompt

[目录：最佳实践 🔥](https://waytoagi.feishu.cn/wiki/NbqXwHXrkiYWKVkFTbmcwxQqntb?chunked=false)

[李继刚等的prompt最佳实践](https://waytoagi.feishu.cn/wiki/JTjPweIUWiXjppkKGBwcu6QsnGd?chunked=false) 操作

高阶使用方法：

[陈财猫：如何用GPT写长篇科幻？用分治法完成复杂任务](https://waytoagi.feishu.cn/wiki/GY3sw4LQli5k9ckzrDZcRnVmnQf)

[刘海：Prompt 纵向研究的最新成果：伪代码提示词](https://waytoagi.feishu.cn/wiki/MjUDwTbq9iUtBrkskPXcpfOHnPg)

[陈财猫:  由GPT驱动的人生重开模拟器游戏，体验无穷变化的多重人生(已开源)](https://waytoagi.feishu.cn/wiki/WyJSwjvN5iZudhkd8iocrSy3njc)

## 4：大语言模型的更多应用

##  LLM的原理 - 引言

+   了解了AI的应用，接下来我们深入其背后的原理。

![](img/bd9dc52a8a6957124ab2407c208c2d30.png)

---

## GPT全称

+   Generative Pre-trained Transformer

+   这三篇文章经典必读

![](img/b9f98cadc13a67d89cc00b1da079773d.png)

1.  预训练 (Pre-training)

1.  预训练阶段开始。现在，这个阶段在图表中比较特殊，而且这个图表不是按比例绘制的，因为这个阶段是所有计算工作基本上都发生的地方。它占用了训练计算时间和浮点运算的 99%。

+   解释：这是模型的基础训练阶段。模型在大量的文本数据上进行无监督训练，学习语言的基本结构和模式

+   比喻：这就像一个小学生刚入学时，他们开始从零基础学习，掌握基础的阅读、写作和算术技能。在这个阶段，学生主要是在模仿和吸收基础知识。

1.  有监督的微调阶段 (Supervised Fine-tuning)

+   解释：在这个阶段，模型在标注的数据上进行训练，以适应特定的任务。

+   比喻：这就像一个初中生，他们在更高级的课程中深化学习，如物理、化学和历史。在这个阶段，学生开始有目的地学习，并开始应用他们之前学到的基础知识。

1.  奖励建模 (Reward Modeling)

+   解释：在这个阶段，模型学习如何根据给定的反馈来调整其输出

+   比喻：这就像一个高中生，他们为大学入学考试做准备，根据模拟考试的成绩和老师的反馈来调整学习策略。他们开始理解哪些学习方法是有效的，哪些是需要改进的。

1.  强化学习 (Reinforcement Learning)

+   解释：模型在这个阶段会根据之前的奖励建模进行进一步的优化。

+   比喻：这就像一个大学生，他们不仅要学习专业课程，还要进行实践和研究。在这些活动中，他们会根据实际经验和反馈不断调整自己的策略，以达到更好的研究成果和实践经验。他们不仅要在学校里学习，还要参加各种课外活动和比赛。在这些活动中，他们会根据反馈和经验不断调整自己的策略，以达到更好的表现。

## 原理：预测下一个字

GPT的核心任务是预测文本序列中的下一个字。

+   必读文章：[ChatGPT 是在做什么，为什么它有效？](https://waytoagi.feishu.cn/wiki/LnMOwmKIwihcRakRalcczNgrnjf)

+   原文作者：Stephen Wolfram

+    https://www.stephenwolfram.com/

![](img/5e44e1dbd1e4af59b6b02f4cad1a0af0.png)

![](img/0cea039f1a7311fbc5a27efe1e5bc0f5.png)

##

## 名词解释

向量数据库讲的非常好，从特征到向量，再到分类和搜索算法娓娓道来。

向量数据库这个技术确实需要视频才能更好理解，超级无敌推荐

分上下集

https://waytoagi.feishu.cn/record/SHD3rHa96e8vvDcX6b4c0chunIe

## 需要认识的著名人物

[🎓 100位AI领军人物](https://waytoagi.feishu.cn/share/base/view/shrcnNOeoQ6cKjBvFumQGp9Ivpb)

[🌐 42个国内优质信息源](https://waytoagi.feishu.cn/share/base/view/shrcnHMV9GJUQtLc3mg7EGlhJ2c)

[🌐 71个国外优质信息源](https://waytoagi.feishu.cn/share/base/view/shrcnLc3g2UaHzKkUEJ59d5dnkd)

![](img/1265c4d47ce8df40ea8165d6d8329eb7.png)

# AI绘画篇：Diffusion原理 - 引言

+   Diffusion是另一个重要的AI技术，让我们了解其原理。

![](img/53e4c5039189febe259b4e02decc3e8b.png)

---

## 噪点

+   [稳定扩散（Stable Diffusion）是如何运作的](https://waytoagi.feishu.cn/wiki/TNIRw7qsViYNVgkPaazcuaVfndc)

+   Diffusion通过添加噪点模拟数据的生成过程。

![](img/3192a2576b7964b691e0dbc77de7c719.png)

---

## Diffusion案例

+   Diffusion应用示例

+   演示mj提示词生成mj图片案例

+

```
3d, round crystal mooncake, translucent, high angle, orange gradient melt, orange gradient background, pastel colors 
```

```
Food photography, real, glass mooncake, close-up
```

+   演示SD可控性生图

+   案例：

+   [教程：线稿上色Midjourney+Stable Diffusion ](https://waytoagi.feishu.cn/wiki/AsbYwmfS6ikhr3kNsCocdPMEnUd)

+   [教程：SD 做中文文字-持续更新中](https://waytoagi.feishu.cn/wiki/JuiBwNTU1i3Lhwk7kzmcUHLUn0d)

![](img/908ab27dcdd21eb586f1aa206b0be1d6.png)

![](img/eac068186a8d4cf8dd210c352d98155a.png)

![](img/a559f308325d153e744837fa2b372a85.png)

![](img/36ada21cdbfdffe0bc92a138689706f3.png)

定期举办比赛，欢迎参加

[第三期：《令人不安的AI》](https://waytoagi.feishu.cn/wiki/N1Zhw76qPifp5pkty0UcoLw6nlc?chunked=false)

## 提示词网站

多维表格（JuzDbhAiea0KhUscE9EcT5yRnK3_tblghd7AAJdnPQGC）

## 必须关注的AI绘画博主

排名不分先后！持续更新中

多维表格（JuzDbhAiea0KhUscE9EcT5yRnK3_tbl9BQY8YIGIdH37）

## 学习入门看这里

视频教程：

[Stable Diffusion新人视频教程](https://waytoagi.feishu.cn/wiki/O5jEwgZIRiQ10xkqGOQcKtSBnSe)

[🔥【课程】爆肝制作！Stable Diffusion零基础入门宝典](https://waytoagi.feishu.cn/wiki/WjxFwx4fwiWaMhkuv8icJ3tEnbd)

文字教程：

Stable Diffusion超详细教程！从0-1入门到进阶

[白马少年](https://waytoagi.feishu.cn/wiki/M3HHwH6m5iML37klefgc10vDntg)

# 什么是Agent

+   Agent是执行特定任务的AI实体。

Agent（智能体） = 一个设置了一些目标或任务，可以迭代运行的大型语言模型。这与大型语言模型（LLM）在像ChatGPT这样的工具中“通常”的使用方式不同。在ChatGPT中，你提出一个问题并获得一个答案作为回应。而Agent拥有复杂的工作流程，模型本质上可以自我对话，而无需人类驱动每一部分的交互。

参考文章：

https://logankilpatrick.medium.com/what-are-gpt-agents-a-deep-dive-into-the-ai-interface-of-the-future-3c376dcb0824

能干什么

最有名的案例：

斯坦福25人小镇

https://waytoagi.feishu.cn/record/1sfvuej0sATQfbO6zbeEAWk02

![](img/a680b9bf1d9031cdaa416a348ae92a52.png)

Lilian Weng（OpenAI 应用人工智能研究负责人） 的这篇 Blog 可以说是目前 AI Agent 领域优质论文的系统综述，她将 Agents 定义为 LLM、记忆（Memory）、任务规划（Planning Skills）以及工具使用（Tool Use） 的集合，其中 LLM 是核心大脑，Memory、Planning Skills 以及 Tool Use 等则是 Agents 系统实现的三个关键组件，在文章中，她还对每个模块下实现路径进行了细致的梳理和说明。到今天，构建 AI Agent 的工具箱已经相对完善，但仍需要面对一些限制，例如上下文长度、长期规划和任务分解，以及 LLM 能力的稳定性等。

https://lilianweng.github.io/posts/2023-06-23-agent/

![](img/e2bb01db120cd3797da941255b0e366b.png)

![](img/d537f8fcb933a91ef811a358c2b268e9.png)

与普通模式有什么不同

![](img/6f62ed9aa4cb10c2b98e018456bb75b4.png)

![](img/552b89559a235393fe9704b1b7b09656.png)

## Agent的现状

+   图片：现代Agent应用示例

![](img/3c40a3aada407052843d0e967991d9f8.png)

![](img/6f3be6b579c588dbe5ed93a9385a5894.png)

+   现代的Agent已经被广泛应用于游戏、机器人和其他领域。

![](img/dc52672d31f8757b686c68870066fe1f.png)

![](img/35c497825dceb80903939463ba52e799.png)

![](img/a5cc3c64eb1107984460815212e3e6c0.png)

![](img/9938fd637264ecf210bfeb628be2bc4a.png)

![](img/36131e51b60dc594cadce76f7a588784.png)

![](img/a21bc1f4071183ead0d2d8900814fb8f.png)

## 数字人

[AI 虚拟数字人简介](https://waytoagi.feishu.cn/wiki/IOO6wQKDKiMrmzk1oDxcGFpFnFh)

虚拟数字人是通过各种技术创设的，具有多重人类特征，如外观、行为、思想等的虚拟形象。

**从应用层面来说，虚拟数**字人可以分为服务型、表演型和身份型三大类。

+   服务型虚拟数字人，如虚拟主播、虚拟助手、虚拟教师、虚拟客服、虚拟医生等，主要是为物理世界提供各种服务。

+   表演型虚拟数字人，如虚拟偶像等，主要被应用在娱乐、影视等场景，如虚拟偶像演唱会等。

+   身份型虚拟数字人，是物理世界的“真人”进入虚拟世界的数字分身，在元宇宙中，数字分身有广阔的应用场景。目前产业还有待进一步完善，是数字人市场的增量空间。

多维表格（JuzDbhAiea0KhUscE9EcT5yRnK3_tblV1fcNytaRj3TW）

[特看科技数字人产品（主播宝）介绍](https://udu133ckhp.feishu.cn/wiki/VMhZwO99fiFWvKkjx1kcsWqCnYf)

# 课后作业

多维表格（JuzDbhAiea0KhUscE9EcT5yRnK3_tblQ64qhrbKaEjbR）

# 结尾

+   感谢大家的聆听！希望大家对AI有了更深入的理解。