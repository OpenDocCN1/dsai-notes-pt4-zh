
# 提示词prompts

> 来源：[https://bluefocus.feishu.cn/docx/TCSwdZJgnoENchxZHjucF32Invf](https://bluefocus.feishu.cn/docx/TCSwdZJgnoENchxZHjucF32Invf)

## ❔prompts是什么

prompts（提示词）可以理解成：让 GPT 能精准 Get 到你意图的话，或者与 GPT 沟通的语言方式。

如果你给 GPT 的提示词质量不好，或者不到位，那么它给到你的，往往就是那种 “像是一堆正确的废话堆积而成” 的文字垃圾。所以，想要获得 GPT 高质量回答的第一步，就是先学会与它沟通的语言，也就是学会写提示词。

![](img/16fb4be77c6a710be39411b130fd0f2b.png)

这就相当于日常生活你会不会和别人沟通，我们要知道一个好的沟通方式可以形成愉悦的交流方式，或许结果是对你有帮助的，但是不好的沟通过方式会导致双发都不预约，就相当于你问GPT得到的答案也是模棱两可的。

我们首先需要明白一个道理就是：Prompt对于一个问题的回答的作用可能是从60分涨到75分，或者80分，但是不可能再高，原因就是大模型本身的智能程度决定了回答的上下限。也就是说Prompts可以提高回答的准确实用性，但是，十分有限！！！

如果你用了一个很笨的大模型，你的Prompt就算是使用最好最高级的都没太大用。与此相反，即便你的prompt只看起来一点儿都不够精美，但是聪明的大模型大概率是可以输出不错的结果的。我们其实只需要根据一个结构比较固定的Prompt模板就可以获得不错的输出。

## 🖼️两种框架

![](img/d453086eade37822abe5954968aea34a.png)

这里总结一下我们日常使用的步骤或许更加实用：

+ **步骤1(明确任务)**：思考你需要模型完成的具体任务，尽量具体输入给GPT。
+ **步骤2(设定角色和风格)：**简短描述模型应采用的角色和风格，为GPT赋予行家身份，考虑适合的表达方式和语气。如：“以专家身份”
+ **步骤3(提供背景信息)：**给出足够的信息，让模型了解任务的上下文。如果任务涉及特定的主题或需要特定知识，提供相关的背景信息。如：“关于某某某……”
+ **步骤4(指定输出格式)：**明确你想要的输出类型。例如:“以什么形式给我展现结果“

结论：选一个聪明的大模型ChatGPT Plus（ChatGPT 4.0），加上合理的使用prompt可以达到事半功倍。

首当推荐的想必大家都知道也正在用：ChatGPT Plus（ChatGPT 4.0）

关于如何升级订阅ChatGPT Plus（ChatGP[T](https://zhuanlan.zhihu.com/p/679539404) 4.0）的教程可以看（参考文章）：【保姆级】国内如何用gpt4？如何升级gpt4？用wildcard一键升级chatgpt4.0保姆级教程

本文参考引用自：[https://chatgptch.github.io/](https://chatgptch.github.io/)

## 📖 文章列表

### 李继刚

+ [如何写好Prompt: 结构化](https://www.lijigang.com/posts/chatgpt-prompt-structure/)
+ 结构化的Prompts, 用于各种大语言模型
+ [李继刚等的prompt最佳实践](https://ywh1bkansf.feishu.cn/wiki/JTjPweIUWiXjppkKGBwcu6QsnGd#S3GadvCJ7onsfhxtysrcPkS3n0E)
+ 结果质量与输入有关

![](img/8ec197418292841f459b274e1dc69f70.png)

+ 控制输出长度的方法

![](img/a33341abf7c536a963a9b013381bfc2e.png)

+ 常见错误修正

![](img/e85212ef53f04b5f9243250bcab42c74.png)

### 陈财猫

+ 可操作，可复现的chatGPT prompting方法论“BORE”

### 万能的小七姐

+ [小七姐：少样本提示和思维链提示的实际案例](https://ywh1bkansf.feishu.cn/wiki/LWLpww5ZHiQE8pk6tYycxS1knwp)
+ [关于 token 你应该了解……](https://ywh1bkansf.feishu.cn/wiki/KcjiwpIsOiZsY9kBl5Hc5NSanZf)
+ [小七姐：K12领域应用-儿童PBL项目Prompt](https://waytoagi.feishu.cn/wiki/EvrRwlyLviLcL6knFSdc8E40nl2?from=from_copylink)
+ [小七姐：知识图谱自动生成的尝试](https://waytoagi.feishu.cn/wiki/LcAAwPUgiieXgfkQXy9c1avmnte?from=from_copylink)
+ [小七姐：基于联网验证科学新闻的科普作者](https://waytoagi.feishu.cn/wiki/HPkewFMdniRe3MkOH6ocqFYZnNq?from=from_copylink)
+ [小七姐：英文病例解读专家](https://waytoagi.feishu.cn/wiki/Nnwvw9K9Pikz2nkuMr5cweKDnbf?from=from_copylink)
+ [小七姐：HR工具-通过连续对话生成JD和面试题参考](https://waytoagi.feishu.cn/wiki/W4uIwQczaiWMFAkd8RTc1ZRynvb?from=from_copylink)

### 刘海

+ [[23.09] Prompt 纵向研究的最新成果：伪代码提示词](https://nanfangshaonian.feishu.cn/wiki/YhNdws9LCi1JxGkpJ8dcXB3Gnih)
+ [[23.08] 网上疯传的「AI 提示词工程师」到底是什么？](https://nanfangshaonian.feishu.cn/wiki/CkIowVcTjiL3EdkJjzzcaeJPnfh)
+ [[23.07] 如何使用思维链 COT 巧妙提升 LLM 输出效果](https://nanfangshaonian.feishu.cn/wiki/V7xvwXg7OiuxzzkOqTxcEe1dnCf)

### JK

+ [JackeyLiu（JK）：Prompt 自动优化思路](https://waytoagi.feishu.cn/wiki/QOO2wfgsBiPJC7kECozcSGexnvh)

### 李小刀

prompt 论文集：[TELeR分类法](https://jvzu2dy7aak.feishu.cn/wiki/GFqBw6TeNis6yakWn0McCmi7nEH)

![](img/9b6b6eb082892700a72763abf2758708.png)

[TELeR分类法.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Bs2WbtOzoodYBaxIuyUclbVVn4u)

文章选自[⭐LangGPT 结构化提示词](https://langgptai.feishu.cn/wiki/RXdbwRyASiShtDky381ciwFEnpe)，更多提示词内容可前往查看
