# Windows本地部署n8n工作流自动平台

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/RXaTd2iR9ovrBAxKgvEcIhlAnEc](https://qqs7y1hozd1.feishu.cn/docx/RXaTd2iR9ovrBAxKgvEcIhlAnEc)

> 本文主要介绍如何在 Windows 系统使用 Docker 部署 n8n 工作流自动平台，并添加 MCP 服务实现自动化 AI 处理任务。

> **官方提供了两种方**式本地部署 n8n，本例中使用 docker 方式进行部署，下面就来具体演示一下操作流程。

![](img/16d18ce351a24538a9aa8f55d905b3e3.png)

## 1.安装 Docker

![](img/28acbbf2e047cde172dc0d33cb6fe6f1.png)

然后，在任务栏搜索功能，勾选适用于Linux的Windows子系统 、虚拟机平台

![](img/e2c6c59490b674c01774b751787b9a8e.png)

管理员权限打开命令提示符（cmd），安装 wsl2：

```
wsl --set-default-version 2
```

```
wsl --update --web-download
```

等待 wsl 安装成功：

![](img/25104d1dbcd3c0824f0078fb54f07d6e.png)

点击应用并保存后，重新打开 docker desktop 后，左下角显示 engine running 即可正常使用 Docker。

![](img/88d081d8770534b9b170922d894d5898.png)

## 2.本地部署 n8n

![](img/b1ed04a17c85dd3909c0b927f3c06e87.png)

打开 cmd 终端，执行命令即可：

```
docker volume create n8n_data
```

```
docker run -it --rm --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
```

![](img/5a0252441f2cc898a74ce7b1d3620cd7.png)

n8n 容器启动后，

![](img/b02b1a51e96478321f3bb20898d94813.png)

在 docker desktop 中也能看到容器信息：

![](img/634a1a6c352c419627e2e58499479f5b.png)

访问后如果显示 n8n 注册账号页面，则说明安装成功：

![](img/cfd38ec797ae8f04a56be9c2cc04a2f2.png)

## 3.使用操作

### 3.1 激活 license key

填写注册信息后，点击 next 下一步：

![](img/53a5e759f4ce531dbc483669a181f8c9.png)

再简单填写一下调查问题点击 get started 继续：

![](img/1a39d71b972988f772da9973412a0eaf.png)

点击获取免费的 license key 到邮箱：

![](img/657a7212c563ab4349d09b0f8d280583.png)

最后进入邮箱，点击激活 license key 即可。

### 3.2 创建 AI 工作流

在 n8n 的主界面，我们首先点击右侧的使用现成的 AI Agent 模版进行创建即可：

![](img/77aae1909a9ed26cf2e17d842f02452c.png)

打开后，我们能看到提供了一个基础的工作流模版，我下面进行一下简单的修改，以便调用本地大模型进行工作：

![](img/053af618c22a889df27fbce0f3ca45e0.png)

这个工作流的流程是接受到聊天信息后传导至后边的 AI Agent 模块，然后调用聊天大模型，这里的示例使用的是 OpenAI 的模型，我在本地已经安装了 Ollama 来调用大模型，所以先将这个删除：

![](img/86ada09fe894e1a7dc014f6bbb9ef4a6.png)

删除之后，点击加号，在右侧的语言模型栏中能看到 ollama chat model，选择即可：

![](img/a6635984382d843a5ecc9369d7cb67c2.png)

在跳转的配置界面中，点击创建新凭证：

![](img/0eb75238f0baf1700fbb896d75875e43.png)

然后输入你的 ollama 安装设备的 IP 地址，左上方的名字也可以自定义修改：

![](img/3465ce9e8e21f52ba8fe53b2caaf1ef9.png)

看到连接测试成功的提示，即配置成功：

![](img/427d9ff66bcf8dc0e2d983afe39788bd.png)

为了成功远程调用本地安装的 Ollama，我们需要提前进行一下环境变量设置：

```
setx OLLAMA_HOST &quot;0.0.0.0&quot;
```

```
setx OLLAMA_ORIGINS &quot;*&quot;
```

然后重启一下 ollama 服务即可

然后，选择一下要通过 ollama 调用本地哪个大模型：（支持函数调用的模型）

![](img/8e0573b559ccfcef47978f0e7b5dddeb.png)

然后点击返回即可：

![](img/4523f935cd5c1c2c89f8c9aa9d1f4e08.png)

现在可以在 n8n 主界面看到模型已经修改为了 ollama chat model：

![](img/c7d9cf6634ad25012e5b7c5b14558889.png)

memory 部分默认保留，tool 暂时不用管，稍后添加：

![](img/2f888a9d414bb2f4897b5fc935d28a9a.png)

我们先用这个简单的工作流来测试一下：

![](img/533faf60504b35c6a47c39351d815ad9.png)

运行结束后，可以看到工作流的每个节点都显示绿色的对号表示没有出错：

![](img/067673de05d176d878117963dd07ca49.png)

底部左侧能看到聊天交互中的提问与回答，在右边的日志面板中点击模型也能查看聊天记录：

![](img/a777e22ddb8e1109a8e0ec838ed976c2.png)

### 3.3 安装 MCP 节点

接下来，我们可以在 n8n 中添加 MCP 节点来实现在工作流中支持 MCP 服务。

点击个人账号–设置：

![](img/3716bb6eb92ca98783c62a2fdd03702a.png)

点击社区节点后安装一个社区节点：

![](img/085bb98cf0c072c135dbe0a4bdd02c6e.png)

输入节点名称：n8n-nodes-mcp 勾选下面选项，点击安装：

![](img/98162f118c4e4b030bd568685c2442ea.png)

安装完成后，**需要重启一下 n8n 服务**，我们现在就能在 n8n 工作流中中集成 mcp 服务了：

![](img/c6ea9a4afea3a2bae2f3bc285af09d37.png)

### 3.4 添加 MCP 服务

回到首页，点击刚才创建的工作流：

![](img/8679d6577807d6bc60fed99a832acad0.png)

点击添加 tool：

![](img/84087ec0b1162482dffbc14c87248b6c.png)

输入 mcp，选择下面描述为 Use MCP client 的 MCP Client Tool：

![](img/5768b708b6623e70d598359db6e6cb68.png)

选择后，在弹出的页面首先点击新建进行 mcp 服务的设置：

![](img/6d031ed49cb4266ea7ec218ea193a1b1.png)

这里我们创建一个 Tavily MCP Server 进行演示，这是一款开源项目，AI 助手可以方便地利用 Tavily 提供的搜索和提取工具，获得实时网络搜索和数据提取能力，快速获取和解析网络上的信息资源。

![](img/509068ed0d4456fb5aacb584d7a8bc1c.png)

![](img/1edb62757d62f7517ee281c3157636f3.png)

将上边的参数添加到新建的 MCP 服务配置中：

![](img/37c2e950214d7d48c95df831ddb1d18b.png)

这里的环境变量需要添加的是 TAVILY_API_KEY：

![](img/120a483669d5c712ec37596e49dcb039.png)

三项都填好后，保存即可：

![](img/761047abcd735cd98ec97f7f9c28b873.png)

然后在跳转的页面给这个 mcp 服务重新命名一下方便识别，大家可以自定义：

![](img/8a7dce247b3daa8c18f9ed1952f435c9.png)

修改完点击返回，可以看到已经在 tool 中添加了一个 Tavily Mcp 服务：

![](img/dc938dba25cf4546308a6f102160a092.png)

然后在 AI Agent 中进行一下设置修改：

将 source for prompt 选项修改为 define below，然后下面的 prompt 修改为：{{"Choose proper tool for user input:"+$json.chatInput }} 即根据用户的输入选择工具

![](img/06ab656dc8b91f74b14d806525572bfa.png)

现在我们在聊天框中向它提个问题，可以看到每个节点都正常执行工作流，并给出了答案：

![](img/b956c88bce99bf9c88ad63e84a195987.png)

到这里就成功在本地部署了 n8n 工作流平台并学习了如何调用本地大模型与添加 mcp 服务，之后你可以根据自己的需求添加其他的 mcp 服务来打造自己的全自动 AI 助手啦。