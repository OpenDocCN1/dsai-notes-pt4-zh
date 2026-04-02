# DeepSeek在Mac上本地可视化部署，保姆级教程，再也不怕崩了！

> 来源：[https://geekbang.feishu.cn/docx/X4KLd04ajoZ0bcxiUx7cKNM7nTf](https://geekbang.feishu.cn/docx/X4KLd04ajoZ0bcxiUx7cKNM7nTf)

文章来源：APPOS

原文链接：https://mp.weixin.qq.com/s/9T1WcEENyQP3niOquR_3uA

DeepSeek，全称为“深度求索”，是由杭州深度求索人工智能技术研究有限公司开发的人工智能产品。这个名字体现了团队的核心理念与追求：DeepSeek 不仅仅是为了回答问题，更是致力于深度理解和探索，让 AI 具备类似人类的思考方式。更重要的是，DeepSeek-R1 的性能足以与 ChatGPT 相媲美，且完全免费供用户使用！

![](img/42cfc17358c0f7c29a47a733c7338c8b.png)

# 如何使用DeepSeek？

DeepSeek使用方法非常简单，直接访问官网：https://www.deepseek.com/，点击“开始对话”即可开始使用。DeepSeek网页版提供三种模式：

+   **基础模式（V3）**：这是默认模式，适用于大部分日常使用，比如写文章、聊天交流等。

+   **深度思考模式（R1）**：当你遇到复杂问题时，如数学计算或深入分析时，可以开启这个模式。虽然反应稍慢，但分析能力极为强大，DeepSeek-R1的性能可媲美OpenAI的高级模型，而DeepSeek-R1是完全免费的。

+   **联网搜索模式**：DeepSeek的知识库更新截止到2024年7月，因此如果你需要获取最新的网络信息，可以开启联网搜索模式。

# 为什么要本地化部署DeepSeek？

由于DeepSeek的火爆，很**多用户在使用时可能会遇到服务器****繁忙的情况。同时，本地**化部署DeepSeek主要是为了提升使用的稳定性与安全性，满足用户对隐私保护和数据合规性的需求。本地部署能够避免网络波动和延迟，确保模型运行更加高效，同时将数据存储在本地，有效防止敏感信息外泄。

# 在Mac电脑上部署DeepSeek教程

1.  首先下载Ollama，点击 [Download for macOS];

官网链接：https://ollama.com/download

若官网无法下载，请点击下方网盘链接进行下载；网盘链接：https://pan.quark.cn/s/a4fba00e300b

![](img/7de81efa1b4a2094ae7228522bdbaa50.png)

1.  下载后的直接打开需要移动到应用程序内；

![](img/2e8f8f5dfaa4f10882b74c0ac23ee296.png)

1.  点击 [Next]；

![](img/775bff09a40500fb63b9f008d05c674c.png)

1.  点击 [Install]；

![](img/025d2b63e508c905f4f3604e71e36d2c.png)

1.  Ollama安装完成，点击 [Finish]；

![](img/0419db1b0d78e9040e1cb913a6e20e87.png)

1.  在启动台内打开 [实用工具]；

![](img/39a40e8bb442ed61600a585240cbfbcb.png)

1.  打开终端；

![](img/9bf16cb690d203c90ad8b7deab0c2b70.png)

1.  在终端输入 [ollama]，然后回车显示如图就表示 [ollama] 已经安装好了；

![](img/8cc7155f928fd968e06c65409383a98f.png)

1.  打开Ollama官网，点击 [Models] ，找到deepseek-r1模型；

1.  复制DeepSeek-R1下载代码；

1.  注意完整的R1模型要占用404GB空间并需要强大算力，标准版M芯片推荐32b内，如果Mac存储空间焦虑，推荐下载1.5b或者7b；

![](img/cb3fbcf71a5e8db4bce350668b879a67.png)

1.  将复制的下载代码复制到终端内，如图所示，然后回车；

![](img/fd3c696a531f4cf4a36e99fa60844c12.png)

1.  回车后静静等待下载完成即可；

![](img/866f538800d40b39987773665cfb9086.png)

1.  下载完成后即可在>>>后输入问题，回车即可发送问题。

![](img/7067eb747413c1583ed43ab173345e06.png)

# **可视化搭建**

1.  打开Chatbox AI官网进行下载可视化工具；官网链接：https://chatboxai.app/zh

![](img/0b9547d57f42f7089e39bf07460a936c.png)

1.  下载后，将Chatbox 拖进应用程序内；

![](img/d2cfe59d1992a4536eaf1ec9c1070338.png)

1.  选择 [使用自己的API Key 或本地模型]；

![](img/d33e80b9ddb8a1c814edb8b5392589d4.png)

1.  选择 [Ollama API]；

![](img/f05d8300344ad4cb242a2fd107c110eb.png)

1.  在设置页面，API 域名保持默认，模型选择 [deepseek-r1:1.5b]，若模型选择框为空请等待下载完成或重新下载；

![](img/34c7da0bcc5b575b126dd6e570add238.png)

1.  可视化部署完成，可在聊天框内进行对话，所有对话均本地完成，回复迅速。

![](img/5b2814322f33917d62caf9590b127e0d.png)