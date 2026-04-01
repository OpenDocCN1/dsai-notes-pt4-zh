# 微学习05 | DeepSeek+Ollama+ChatBox：本地部署

> 来源：[https://datawhaler.feishu.cn/docx/T5AGdco0ToZ8SWxQbSycDiSPnPc](https://datawhaler.feishu.cn/docx/T5AGdco0ToZ8SWxQbSycDiSPnPc)

# 一、标题释义

+   DeepSeek：众所周知的国产大模型（免费），[它的](https://www.deepseek.com/)特色是把深度思考的过程也告知了用户。官网

+   Ollama：开源的大型语言模型服务工具（免费），旨在帮助用户快速在本地运行大[模型](https://ollama.com/)。通过简单的安装指令，用户可以通过一条命令轻松启动和运行开源的大型语言模型。官网

+   Ch[at](https://chatboxai.app/zh)Box：  AI 客户端应用和智能助手（作为模型API和本地模型的连接工具免费），支持众多先进的 AI 模型和 API，可在 Windows、MacOS、Android、iOS、Linux 和网页版上使用。官网

DeepSeek+Ollama+ChatBox的含义就是：通过Ollama运行DeepSeek大模型，形成大模型服务。通过ChatBox对接Ollama的大模型服务，完成用户和大模型的交互。

# 二、安装步骤

## （一）Ollama安装

本节描述如何安装Ollama。

[以Windows](https://ollama.com/)安装为例，打开 Ollama官网

![](img/27ec09dbab4173b2e4f84fa0453d5636.png)

点击Download：

![](img/20ae0232de55ce4df494b07b309fc9f1.png)

选择Windows平台，点击"Download for Windows"：

![](img/1af9f05a27d9872376d668cac6511142.png)

耐心等待下载结束。

双击 OllamaSetup.exe 执行安装：

![](img/88305afaf632eeb7812a44d64a1f8e42.png)

点击"Install"安装即可。

安装完毕之后，系统托盘栏会有一个图标：

![](img/ead9c84c46892bc3fcaa1a1d15f957c2.png)

右键点击后会出现以下内容：

![](img/3575cbc6aa327b0b54c63d99ee5fb41c.png)

如果打开Windows左下角的搜索栏，搜索Ollama，会出现以下内容：（小猪变得清晰了）

![](img/6b0694075ec2a0a8314818c6f0b1d185.png)

这都是Ollama存在的标志。

TIPS1：如果你的C盘不够怎么办？

如果你的C盘剩余空间不足以放下几个G、十几个G、甚至几十个G的模型文件，那么您可以在系统环境变量中做如下配置：

编辑系统环境变量

![](img/394ccc9dab72be98e4e6ad0a3d24ff1f.png)

环境变量->系统变量->新增：

![](img/7ef278aadba0f275eab56d253e00da10.png)

以模型文件下载到 D:\OllamaModels\models 为例。

模型下载完毕后一般是如下的目录结构：

![](img/f8e46acf3367104639ac87aca8edb32d.png)

TIPS2：如何设置允许外部连接Ollama？

在环境变量设置中：

+   OLLAMA_HOST需设置成 0.0.0.0

+   OLLAMA_PORT需设成11434

![](img/085bdc939b72bed0fa372e017ef0f6a2.png)

## （二）DeepSeek安装

本节描述如何使用Ollama安装DeepSeek-R1大模型。

打开 https://ollama.com/search，在搜索栏搜索 DeepSeek-R1

点击进去：

![](img/c2408f3e607cb709f6c9e7114bdca92a.png)

在这里可以看到 deepseek-r1有1.5b到671b不等的7个模型(除了671b的都是蒸馏版本)。可以点击下拉框选择模型，并且在右边看到安装方法和模型的大小。上图显示7b模型占用大小为 4.7G。

打开命令行提示符：

ollama list 可以看到现在Ollama已经有的模型：

![](img/06ef7cbe6b4550afa073ca0cbda531f3.png)

以安装32b deepseek-r1 蒸馏模型为例，输入ollama run deepseek-r1:32b

![](img/21dea88c84a9b101a2f1339cf4846721.png)

此时耐心等待模型下载完毕即可：

![](img/133b1a70ae3147bcd82d17283a9c6f3b.png)

## （三）ChatBoxAI安装

本节描述如何安装ChatBoxAI。

[以Windows安装为](https://chatboxai.app/zh)例，打开 ChatBoxAI官网：

![](img/925caf422704df04c4fa9229e90660b9.png)

点击"免费下载（for Windows）"

![](img/5652bdda4490771f469d9bcf0b414363.png)

耐心等待下载完毕：（如果迟迟没下载则点击“手工下载 for Windows(PC)"按钮）

![](img/b78c2ec157493a8bca613e26d95c31c6.png)

双击 Chatbox开头的.exe文件执行安装：

![](img/63c5ad3eaaabc9ff84034209868c7ab8.png)

![](img/720be50716045d7a159004f231462968.png)

![](img/c88ae7df9fcb720c3564a6b207a05a8a.png)

## （四）ChatBox连接Ollama配置

启动ChatBox：

![](img/c06b453cf53260d087e7386021afc643.png)

选择 ”使用自己的API Key或本地模型“

![](img/4d0d9013baffb9581a5a33d238bf6950.png)

可以先点击"x"关闭弹出窗口。

![](img/e09d43dd17e26794320115307f6f8825.png)

这个就是ChatBoxAI的界面。

点击左下角”设置“：

![](img/beeef8b4f7ded066d96e33ad58a69f14.png)

模型提供方，通过下拉框选择"OLLAMA API"，API域名填写 "http://127.0.0.1:11434"，按<TAB键>

系统会自动弹出Ollama已经有的模型，可以用下拉框选择：

![](img/ced8c1d44700f607a7f690115108fdd3.png)

选中一个模型，点击"保存"：

![](img/fad430ede067214b20c9467f37be0f6a.png)

# 三、开始对话

进入Just Chat(新对话)页面就可以对话了。

![](img/229c748a93304d36200166484a22141e.png)