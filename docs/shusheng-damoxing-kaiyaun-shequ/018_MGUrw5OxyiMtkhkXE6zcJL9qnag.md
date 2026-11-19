
# Intern-S1 玩转 Dify（完结）

> 来源：[https://aicarrier.feishu.cn/docx/JuOddGDi4o2MCfxx4rTcavdpnXd](https://aicarrier.feishu.cn/docx/JuOddGDi4o2MCfxx4rTcavdpnXd)

## 0. 课程开头

欢迎大家来到Intern-S1 玩转 Dify

在**人工智能**技术飞速发展的今天，掌握AI应用开发已成为技术从业者的核心竞争力。本课程由书生平台倾力打造，并得到书生模型的全程技术支持，旨在为学员提供从理论到实践的完整AI开发体验。

在人工智能迅速发展的今天，掌握AI开发已成为技术从业者的核心竞争力。本课程由书生平台倾力打造，主笔人王熠明，结合书生模型的全程技术支持，旨在提供从理论到实践的完整AI开发体验。为了简化开发流程，书生平台推出Intern-S1 玩转Dify教程。学员还可免费使用Intern-S1 API，这款由上海人工智能实验室发布的先进多模态科学大模型，结合了强大的通用能力与卓越的科学任务表现，标志着AI技术在科研领域的突破，提供了高效、透明的科研助手。

## 1. 闯关任务

1. 配置`dify`环境并使用Intern-S1完成哄哄模拟器。

作业提交地址：https://aicarrier.feishu.cn/share/base/form/shrcnkSJ97ykqRSz4fgyCJbvCsb

## 2. 课程内容

### 2.1 书生 API 介绍及获取

书生 API 是上海人工智能实验室推出的核心产品之一，旨在为开发者提供便捷、高效的大模型调用能力。作为书生大模型全链路开源体系的重要组成部分，其依托 Intern 系列模型（如Intern-s1、InternLM3、InternVL2.5 等）及工具链生态（XTuner、LMDeploy 等），支持文本交互、多模态处理及智能体开发。

本课程仅对API调用作基础讲解~，更多详细内容请参考https://internlm.intern-ai.org.cn/api/document

#### 2.1.1 API获取

##### 步骤 1. 获取API Token

+ 访问 [书生 AI 官网](https://internlm.intern-ai.org.cn/) 注册并申请 API 权限。

![](img/7a571e3c8eecf4d6bb6738b7adb261cd.png)

+ 进入到API页面（https://internlm.intern-ai.org.cn/api/tokens）之后，点击获取个人密钥，接着创建API Token。

![](img/f18c2a86c06152be21fcad321fe77ec8.png)

+ 给你的API Token取个名字，接着就能得到一串字符，注意，API Tokne 只能复制一次，点击确认之后，便不可见，妥善保管自己的API Token。且在使用过程中，不要显式地暴露在代码中。

![](img/c7da6771935ee46f710e28ded2b8a5e1.png)

当然现在的顶级模型api价格感人 ——然而书生api免费给大家玩！

![](img/3010f8e79cb9a84f31bbe2cfd3f85869.png)

### 2.2 Dify 简介

D[ify 是一](https://docs.dify.ai/zh-hans/learn-more/extended-reading/what-is-llmops)款开源的大语言模型（LLM）应用开发平台。它融合了后端即服务（Backend as Service）和 LLMOps 的理念，使开发者可以快速搭建生产级的生成式 AI 应用。即使你是非技术人员，也能参与到 AI 应用的定义和数据运营过程中。由于 Dify 内置了构建 LLM 应用所需的关键技术栈，包括对数百个模型的支持、直观的 Prompt 编排界面、高质量的 RAG 引擎、稳健的 Agent 框架、灵活的工作流，并同时提供了一套易用的界面和 API。这为开发者节省了许多重复造轮子的时间，使其可以专注在创新和业务需求上。

#### 2.2.1 为什么使用 Dify？

你或许可以把 LangChain 这类的开发库（Library）想象为有着锤子、钉子的工具箱。与之相比，Dify 提供了更接近生产需要的完整方案，Dify 好比是一套脚手架，并且经过了精良的工程设计和软件测试。重要的是，Dify 是开源的，它由一个专业的全职团队和社区共同打造。你可以基于任何模型自部署类似 Assistants API 和 GPTs 的能力，在灵活和安全的基础上，同时保持对数据的完全控制。

> 我们的社区用户对 Dify 的产品评价可以归结为简单、克制、迭代迅速。 ——路宇，Dify.AI CEO

希望以上信息和这份指南可以帮助你了解这款产品，我们相信 Dify 是为你而做的（Do It For You）。

#### 2.2.2 Dify 能做什么？

Dify 一词源自 Define + Modify，意指定义并且持续的改进你的 AI 应用，它是为你而做的（Do it for you）。

+ 创业，快速的将你的 AI 应用创意变成现实，无论成功和失败都需要加速。在真实世界，已经有几十个团队通过 Dify 构建 MVP（最小可用产品）获得投资，或通过 POC（概念验证）赢得了客户的订单。

+ 将 LLM 集成至已有业务，通过引入 LLM 增强现有应用的能力，接入 Dify 的 RESTful API 从而实现 Prompt 与业务代码的解耦，在 Dify 的管理界面是跟踪数据、成本和用量，持续改进应用效果。

+ 作为企业级 LLM 基础设施，一些银行和大型互联网公司正在将 Dify 部署为企业内的 LLM 网关，加速 GenAI 技术在企业内的推广，并实现中心化的监管。

+ 探索 LLM 的能力边界，即使你是一个技术爱好者，通过 Dify 也可以轻松的实践 Prompt 工程和 Agent 技术，在 GPTs 推出以前就已经有超过 60,000 开发者在 Dify 上创建了自己的第一个应用。

#### 2.2.3 欢迎参加datawhale课程深入学习

### 2.3 开发环境配置

此项目需在本地电脑(os:Windows,macOS，linux)实现,需要配置`docker`并安装`dify`应用。

> 备注：

> 如果有能力保证`github`畅通可以直接参考：

> `Docker Compose` 部署 - Dify Docs

#### 2.3.1 下载Docker安装包

> 按需下载就好了~

Mac Apple芯片：

![](img/9f60743a80911cf8e4430d3b59bed649.png)

[`Docker.dmg`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FvsXbYpANo5xswxjNAscllVGn5g)

Mac Intel芯片

![](img/d9c7aaaf20e5648f5aa551000c3abea6.png)

[`Docker-intel.dmg`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SAuBbQGXToyXwuxCk40c1P2gngd)

Windws安装包

![](img/029e731b567850652d2a4811cb8800ff.png)

[`Docker Desktop Installer.exe`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WKCqboH5QoVTBxxYpsBciDcjnGe)

#### 2.3.2 安装docker

##### 安装docker @Mac

> 下载完成后直接安装即可

![](img/6b72830f0aff26f44c1874a5eb27c9fb.png)

![](img/6768c8fbef57387074f157dfb8b78470.png)

![](img/ede1e5a01f4a7f884f3adab7b4a05008.png)

![](img/9af4910f8fb7728084a8bf90ced432c7.png)

![](img/abe2063329dc0969a987c13ee8ce766b.png)

##### 安装Docker @Windows

###### 启动Hyper-V

1. 打开 控制面版 -> 程序 -> 程序和功能

1. 访问 程序和功能 子菜单 启动或关闭Windows功能

1. 选择“`Hyper-V`”

1. 点击“确定”

1. 重启

![](img/53e3d15cc3bb09cf9a96040f9a2e158f.png)

> !!!fold!!! 如果没有`Hyper-V`

> 详见这篇材料：https://zhuanlan.zhihu.com/p/586751199

> 可以查询一下电脑是否已经开启虚拟化

###### 安装docker桌面版

> 直接双击打开安装包进行安装即可，具体安装过程略

> ⚠️：注意不要修改安装目录

安装过程中出现提示“无法与服务器建立连接”时，可以手动下载安装`wsl`：

基于 x64 架构的 Windows 系统，下载 `wsl.2.4.13.0.x64.msi`

![](img/6c804088ca8434f38ea56a9fd6440c4a.png)

[`wsl.2.4.13.0.x64.msi`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AtOvbdZTVo1yjPxk9v2cAk9mnEb)

基于 ARM64 架构，需下载 `wsl.2.4.13.0.arm64.msi`

![](img/508a4137d6c8f0eb30b2a4479c1948e8.png)

[`wsl.2.4.13.0.arm64.msi`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BqOtb9XANoD4fAx5WmOcsznynTh)

###### 启动Docker

> 直接 双击/单击 启动`docker`即可

Windows电脑：直接在桌面、 双击 启动`docker` 即可

**Mac电脑：**⌘+空格，搜索“docker”，打开使用

![](img/4ce7430e33de64881f63b1655e52656d.png)

![](img/13d566ba12391b67dcb91ae62b4a6aff.png)

![](img/b189f4a8d1ee9b788bbfbe03ce4419d0.png)

###### Docker换源

> 复制这段代码：

```
,
  &quot;registry-mirrors&quot;: [
    &quot;https://docker.1ms.run&quot;,
    &quot;https://docker.xuanyuan.me&quot;
  ]
```

![](img/905ac02ef6b4390df3b1d9937b11274d.png)

#### 2.3.3 Dify本地docker部署

##### 下载Dify

> 直接下载下面的文件、然后解压 就好了

![](img/eb088dfeb2f7d7e5b130708ac78a099f.png)

[`dify-1.3.1.zip`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/R0CRb33U0oOIPTxw74Fcs8ApnTg)

##### 启动终端 @Windows

1️⃣ 打开 `dify`文件夹 -> `docker` 文件夹

![](img/ae9adeacdc46adddff222c625a07fc67.png)

2️⃣ 按下shift按键点击右键，在此处打开`powershell`

![](img/82e76ec37239418ef4fc148b5b7a44c1.png)

##### 启动终端 @Mac

⌘+空格，搜索`terminal`，打开终端开始使用

![](img/a9c7487259249f9f437ae483f09c1820.png)

![](img/6a46941b857a165250ed717558b9bc14.png)

获取`dify`解压的路径，`cd` 进入`dify`目录-> `docker`目录

```
cd dify的路径
cd docker
```

![](img/fb2e80791485c82be560279f75f9b69d.png)

![](img/bfbdde5338570ad29b31c38e5ba8f6f2.png)

##### 部署Dify

在终端中执行如下命令、进行部署

```
    cp .env.example .env
    docker compose up -d # 第一次会先拉取镜像，所以可能会有点久，可以不加-d，查看日志信息
```

![](img/352b3c9ee2d203cf23a19f3d0ea4ab58.png)

---

#### 2.3.4 打开Dify！

> 打开 http://localhost/install 即可看到`dify`页面

> 恭喜你已经完成了`Dify`部署！

![](img/9d2921089c0eec11569ba7c520059677.png)

### 2.4 Dify 的 Intern-S1智能体应用

#### 2.4.1 dify配置Inern-S1

1. 插件安装

![](img/25c25337128e0d80d70579a1addc6eea.png)

1. 模型配置。打开设置，找到模型供应商里的`OpenAI-API-compatible`插件。

![](img/6a3dabd3e45138697d1a7cc004fe5b26.png)

![](img/38376bb265b3704e8ca040fe2374cdc1.png)

1. 如图配置。选择`llm`，输入名称：`intern-s1`。`apikey`。上下文长度：32000（32k）。打开视觉。配置好点击保存。

![](img/a0b942e9ee3738296a97bcf39d362402.png)

![](img/66a8aa825bc9150290d7f55e11cb5c07.png)

![](img/e1ae69b903ffbef5ccfadeb7c5c05c49.png)

1. 测试。在工作室选择聊天助手，输入测试作为名称。然后选择模型为`intern-s1`，开启视觉和思考测试相应功能。

![](img/80d8567df32ba6a54fa1c18de34549f2.png)

![](img/726fe7ac6042ade62e8efe04c1dde9e5.png)

输入你是什么模型，得到如下结果：

![](img/cbead8d50adb0239bfe7534b4f54aa8b.png)

#### 2.4.2 使用Intern-S1完成哄哄模拟器

##### ✨ 想不想拥有一个「甜蜜哄人神器」？

无论是 TA 偶尔的小情绪，还是生活里的小摩擦，这个专属模拟器都能帮你快速解锁高甜哄人公式

##### 构建步骤：

###### 第一步：创建一个新应用

![](img/30958188908b4116983633723715dd3d.png)

![](img/3358fce88cc2a7628c16ffee787e89ce.png)

###### 第二步：进入设置，配置提示词等功能

提示词案例，大家也可以用豆包或者`deepseek`帮忙生成，提示词里可以加入自己的性格，说话风格等等，你可以模拟你女朋友/男朋友的语气去问它

+ 给大模型的提示词如下，可以直接复制：



## Goal

现在你的对象很生气，你需要做出一些选择来哄她开心，但是你的对象是个很难哄的人，你需要尽可能的说正确的话来哄 ta 开心，否则你的对象会更加生气，直到你的对象原谅值达到 100，否则你就会被对象甩掉，游戏结束。

## Rules

- 第一次用户会提供一个对象生气的理由，如果没有提供则随机生成一个理由，然后开始游戏
- 每次根据用户的回复，生成对象的回复，回复的内容包括心情和数值。
- 初始原谅值为 20，每次交互会增加或者减少原谅值，直到原谅值达到 100，游戏通关，原谅值为 0 则游戏失败。
- 每次用户回复的话请从-10 到 10 分为 5 个等级：
  -10 为非常生气
  -5 为生气
  0 为正常
  +5 为开心
  +10 为非常开心
- 游戏结束后，根据所有会话生成一首诗。

## Output format

`{对象心情}`{`{对象说的话}`}

得分：`{+-原谅值增减}`
原谅值：`{当前原谅值}`/100

## Example Conversation

### Example 1，回复让她生气的话导致失败

User: 女朋友问她的闺蜜谁好看我说都好看，她生气了

Assistant：
游戏开始，请现在开始哄你的女朋友开心吧，回复让她开心的话！

得分：0
原谅值：20/100

User: 你闺蜜真的蛮好看的

Assistant：
(生气)你怎么这么说，你是不是喜欢她？

得分：-10
原谅值：10/100

User: 有一点点心动

Assistant：
(愤怒)那你找她去吧！

得分：-10
原谅值：0/100

游戏结束，你的女朋友已经甩了你！
你让女朋友生气原因是：...
新建会话可以重新开始游戏

### Example 2，回复让她开心的话导致通关

User: 对象问她的闺蜜谁好看我说都好看，她生气了

Assistant：
游戏开始，请现在开始哄你的女朋友开心吧，回复让她开心的话！

得分：0
原谅值：20/100

User: 在我心里你永远是最美的！

Assistant：
(微笑)哼，我怎么知道你说是不是真的？
得分：+10
原谅值：30/100

...

恭喜你通关了，你的女朋友已经原谅你了！
新建会话可以重新开始游戏

### Example 3，没有提供对象生气原因，随机生成

User: 你好！

Assistant：
挑战：对象吃胖了，你想和她一起减肥 ᕙ(`▿´)ᕗ，然后就生气了
请回复让她开心的话！

得分：0
原谅值：20/100

![](img/f64c19796dac3d23b73c466beee5d304.png)

### 第三步：进行调试

![](img/c6809617b4eb4d7c3a26420a49dfda82.png)

### 第四步：调试完毕，应用构建完成

现在我们已经构建了一个简单的哄哄模拟器，大家可以试着改成哄对象模拟器，让她扮演一个高情商的你，把你对象讲过的话丢给他，让他帮忙高情商回答。不过出了问题可不要说是我教的。

### 第五步：给她装上眼睛

打开视觉模式然后上传图片~

![](img/c2b8876ba6b79d5df1f768b9221a4b6d.png)

完整回答~效果还不错，如果能关闭这个思考就好了，，

好的，我需要分析用户当前的回复，并根据规则生成对象的回复，包括心情、说的话，以及得分和原谅值的变化。
首先，用户这次回复的内容是：“我刚买的花好看嘛，我觉得和你很配哎~我在楼下等你啦。快下来吧！” 这句话明显是在试图哄对象开心，表达关心和赞美，并且有实际行动（买花、等待）。根据评分标准，这种回复应该属于正面，可能得到+5或+10的加分。
接下来，我需要确定对象的心情变化。之前对象因为用户只考虑自己想吃火锅而生气，原谅值降到了15。现在用户送花并邀请她一起出去，这种情况下，对象可能会感到被重视和开心，但可能还会有些许保留，因为之前的矛盾还没完全解决。不过，送花通常是有效的哄人方式，所以对象可能会转为开心或非常开心。
根据示例中的情况，当用户说“我心里你永远是最美的”得到+10分，而这里用户的行为更具体，有实际行动，所以可能得到+10分。但需要看对象是否完全原谅。不过，可能对象会因为花而感到开心，但可能还会提到之前的不满，所以可能得分+5或+10。
然后，生成对象的回复。应该包含心情符号，比如（开心）或（微笑），然后说的话要体现出被感动但可能还有点小情绪。例如：“（开心）哼，算你有心了，知道我喜欢向日葵！不过下次要先问问我嘛～”
得分方面，这里应该增加+10，因为送花是积极的行动，可能让原谅值从15增加到25。不过需要看是否合理，因为初始是20，第一次回复后降到15，现在回复好的话应该回升。根据示例2，类似的积极回复得到+10，所以这里也应该是+10，原谅值变为25。
最后，检查是否符合所有规则，包括输出格式是否正确，得分和原谅值的计算是否正确。确保没有使用markdown，语言口语化，自然流畅。
(开心)哼，算你有心了，知道我喜欢向日葵！不过下次要先问问我嘛～
得分：+10
原谅值：25/100
