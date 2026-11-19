
# 2023-06-20GPT-Engineer一夜爆火！一个提示生成整个代码库

> 来源：[https://xuqiwei1986.feishu.cn/docx/GhYJdzUVSoWccax8nvRcJhmKnXc](https://xuqiwei1986.feishu.cn/docx/GhYJdzUVSoWccax8nvRcJhmKnXc)

🔗 原文链接：https://www.toutiao.com/article/7246645655421059639/?log_from=512e2762a566f_1691397727630

2023-06-20 14:08 · 新智元

编辑：桃子 好困

## 【新智元导读】动嘴写代码指日可待！AI代码生成明星项目GPT-Engineer一夜爆火，短短几天狂揽19k星。

AutoGPT之后，又一个明星项目诞生了！

GPT-Engineer一夜之间火遍全网，GitHub项目已经狂揽19k星。

这是一个根据指示生成代码的AI工具，你只需要「动动嘴」，就能直接构建整个代码库。

![](img/12325eac43b41ea83d31b99d62d9f981.png)

项目地址：https://github.com/AntonOsika/gpt-engineer

甚至，它能学习你的代码风格，短短几分钟就能让你搞定编码项目。

网友表示，我们离AGI又进了一步。

![](img/ab3affaffa004aba53e6889ae3750929.png)

程序猿又要被淘汰一波了。

![](img/cd339005c7dac54afc69bb6b202c20d6.png)

### 一句提示创建代码库

短短一周，GPT-Engineer在GitHub热度狂飙，引众多开发者围观。

究竟有多大的魅力？

项目主要作者Anton Osika在6月11日首次推出GPT-Engineer，并介绍了这款AI工具最大的特点：

- 一个提示就能生成一个代码库
- 提出需要澄清的问题
- 生成技术规范
- 编写所有必要代码
- 易于添加自己的推理步骤、修改和实验
- 项目开源
- 分分钟让你完成编码

![](img/01dc40c2cb8ad681cec142a680c6ed5e.png)

畅想下，未来无需编写一行代码，开发者创建一个项目就像和好友聊天那样简单。

这不仅仅是一个项目，而是对未来的一瞥。

GPT-Engineer预示着未来软件创造将是一个人机共话的时代。

另外，根据GitHub介绍了GPT-Engineer主要项目理念：

- 简单易用，能够为用户提供价值；
- 灵活且易于添加新的「AI步骤」；
- 支持高级提示，可以记住用户反馈；
- 能快速在AI和人类之间快速切换；
- 所有计算都是「可恢复的」，并持久地保存到文件系统。

![](img/8b2b1c6bfcf2bb13294ce2d9ce9e08d2.png)

这个项目独特地方在于，开发者在文本文件中提交需求，GPT-Engineer不是无条件接受这些要求，而是提出许多详细的问题来让程序员澄清缺失的细节。

整个过程分两个阶段执行，即（1）需求细化促进阶段和（2）软件构建阶段。

### 第一阶段的步骤是：

用户提供的包含软件需求的文本文件被提交给GPT-Engineer，并被放置在OpenAI的GPT的初始消息中，同时还包括确定澄清问题的指示。

GPT-Engineer系统接收来自OpenAI GPT-4的反馈，了解哪些需求需要澄清，并回应提示用户澄清的问题。

GPT-Engineer会将这一过程循环，直到所有问题澄清到OpenAI GPT-4满意为止。

![](img/958b9f2d69acbe072725319661a5c74b.png)

### 第二阶段的步骤是：

前一阶段提炼的需求被打包，并与OpenAI的GPT指令（即系统提示）和另外一套GPT-Engineer希望看到的输出指令（即用户提示）一起包装起来。

GPT-Engineer收到来自OpenAI GPT-4的响应，然后…

GPT-Engineer为用户提供指令的软件项目创建源代码文件。

![](img/fd24a116aea7d228f9f7897efbda223b.png)

### 贪吃蛇上手实操

说了这么多，不如上手试练一番。做一个多人可玩的贪吃蛇网页版游戏。

你只需要输入一些关键提示：

使用有MVC组件的Python后端，视图需要将状态流传输给所有玩家，用你所生成的代码直接直接运行html和js。

![](img/59bd629eb0cf403d9abfb5fed9d8263c.png)

对于这个游戏代码生成，GPT-Engineer就会让你继续回答一些细节的问题。

![](img/b30391b7d0cf5b13535177174743f3fb.png)

游戏规则和机制：确切给出贪吃蛇如何移动、生长，以及与其他玩家如何交互，有没有什么power-ups，或者特殊游戏元素？

玩家互联：有多少玩家能加入游戏，是否有游戏大厅或者匹配？

游戏状态更新：多久更新一次游戏状态，多久把状态流传输给玩家，延迟率是？

用户界面：游戏界面长什么样？有任何特别设计需求和喜好吗？

游戏控制：玩家如何控制蛇？是否有其他控制或设置？

游戏结束状况：游戏如何结束？是否有评分体系或排行榜？

代码结构：对Python后端、HTML和JavaScript代码的组织有什么具体要求吗？

![](img/4314fac7ab365d16b8a38fb6edbfd083.png)

当你简单回答完这些问题后，GPT-Engineer就根据这些信息，给出了能够实现目标所需的文件、类和方法的列表。

具体步骤唰唰唰地来了。

![](img/4495e9ce2c27e31eb2be717551bf34f3.png)

最后一款完美的贪吃蛇游戏代码就完成了。

![](img/ac8bc92a85d6602d197e4acca1cca473.png)

### 网友热评

网友表示，「澄清问题」是真正使GPT-Engineer脱颖而出的原因，因为修复生成代码中的问题往往比编写代码本身需要更多的时间。这似乎是朝着正确方向迈出的一大步。

![](img/95d953de118e5a262abee9c585029c6c.png)

真希望我上学时候就有这样利器了。

![](img/41fd66e95f53878901998a62e4b6ba19.png)

还有网友对GPT-Engineer和GPT-4的区别感到困惑。

![](img/159cd875381a818d315e5f05499d3385.png)

然而，这个项目火虽火，一位网友测试了GPT-Engineer，给出了体验：

### 难设置

实际上不是创建文件，只是让你从命令行中运行一次（即不能调试）

### 与ChatGPT4的能力相当

![](img/aedeea8c70fc084a100fb9866bceb335.png)

![](img/ecd3b5fd21fa45b19cd4a93d066cdaf7.png)

最后总结，理想很丰满，现实很骨感。

![](img/eb08f1f9b15a1cb04ee4feb57a8a80db.png)

还有网友对基准指出了问题，

`gpt-engineer`做了几个简单的编程任务，并将它们作为基准。它可以为你快速开发一个`currency_converter`，但不能正确编码一个`pomodoro_timer`（还没有！）

![](img/3e7e30f6b7bf9f89f4d10a46a8392333.png)

### 安装方法

要么选择稳定版（stable），要么选择开发版（development）。

对于稳定版本：

- `pip install gpt-engineer`

对于开发版：

- `git clone git@github.com:AntonOsika/gpt-engineer.git`
- `cd gpt-engineer`
- `make install`
- `source venv/bin/activate`

### 设置

使用GPT4访问权限的API密钥运行：

- `export OPENAI_API_KEY=[your api key]`

### 运行

创建一个空文件夹。如果在repo中，你可以运行：

- `cp -r projects/example/ projects/my-new-project`
- 在新文件夹中填写 `main_prompt` 文件
- 运行：`gpt-engineer projects/my-new-project`

### 结果

- 检查 `projects/my-new-project/workspace` 中生成的文件

### 限制

使用额外的思想链提示，如Reaffon，应该能够让其更可靠，并且不会错过主要提示中请求的功能。

## 特点

你可以通过编辑 `identity` 文件夹中的文件来指定AI智能体的「身份」。

编辑「身份」和改进 `main_prompt` 是让智能体记住项目之间的事情的方法。

`steps.py` 中的每个步骤都将其与GPT4的对话历史记录存储在`logs`文件夹中，并且可以使用`scripts/rerun_edited_message_logs.py` 重新运行。

### 项目主要作者

### Anton Osika

Anton Osika是Depict.ai的首席技术官。这是一家创建机器学习推荐系统的初创公司，通过尖端的机器学习、计算机视觉和自然语言处理来理解产品。

就在4小时前，他还发了一条动态称，GPT-Engineer已经跃升到17k星，一点也没有减速迹象。

![](img/3a6d6528d18b50cdc4fc150b1496ab2e.png)

![](img/cd7cf1231124d206813d99f006f3bb71.png)

![](img/fb06639ed4e2fe3a5a1bb66f40922487.png)

参考资料：

https://github.com/AntonOsika/gpt-engineer

https://twitter.com/rowancheung/status/1670829363137499137

https://twitter.com/antonosika/status/1667641038104674306

https://twitter.com/antonosika
