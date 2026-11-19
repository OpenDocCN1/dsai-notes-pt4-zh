
# 完整演示APP

> 来源：[https://ai.feishu.cn/docx/GFxodXWpfoH6CTxg7HFcd2flnUf](https://ai.feishu.cn/docx/GFxodXWpfoH6CTxg7HFcd2flnUf)

本章节将带领用户使用 `CogAgent-20241220` 模型在 Mac 笔记本上部署一个可操控APP。

## 环境检查

> 软件和硬件检查
> macOS Sequoia: Version 15.0.1 (24A348)
> Memory: 16GB
> Python Version: 3.13.1 / 3.10.16
> 无外接显示器，不给予本程序管理员权限。

对于其他与上述环境不同的配置 (例如Windows操作系统，Linux桌面版操作系统)，我们未进行测试。

## 安装环境(用户端)

在本步骤之前，请确保已经安装了本项目的首页的`requirements.txt`的全部依赖。这些依赖能保证服务端正常运行。接着，按照下面的步骤安装用户端的依赖。

### 安装tkinter库

请在mac系统环境中安装`tkinter`库。具体安装方式可以参考下面代码

```
brew install python-tk
pip install -r requirements.txt
```

为了验证系统 `python` 是否已经正常安装`tkinter`库，可以在终端中输入下面代码

```
/opt/homebrew/bin/python3 -m tkinter
```

正常应该返回

```
2024-12-14 15:29:04.041 Python[7161:122540731] +[IMKClient subclass]: chose IMKClient_Legacy
2024-12-14 15:29:04.041 Python[7161:122540731] +[IMKInputSession subclass]: chose IMKInputSession_Legacy
```

### 创建虚拟环境

创建虚拟环境，请不要使用`conda`、`virtualenv`等工具，因为这些工具会导致`tkinter`库无法正常使用。请将下面代码复制到终端中执行，并替换`/Users/zr/Code/CogAgent/venv`为你的实际路径

```
/opt/homebrew/bin/python3 -m venv --copies /Users/zr/Code/CogAgent/venv
```

### 创建缓存目录

在本文件夹下创建一个`caches`文件夹，用于保存模型执行中的照片。

```
mkdir caches
```

## 启动APP

### 运行服务端

在远程服务器拉起服务

```
python openai_demo.py --model_path THUDM/cogagent-9b-20241220 --host 0.0.0.0 --port 7860
```

这将在服务器拉起一个模仿OpenAI接口格式的服务端，默认端口部署在 http://0.0.0.0:7860。

### 运行客户端

运行客户端，请确定以下信息:

+ 请确保服务端已经正常运行,并确认服务器端和本地已经通过内网穿透等技术联通。

+ 请确保服务端可以从外网访问，或者通过内网穿透的方式允许自己的本地访问。在我们的代码中，服务端穿透到本地的端口为`7870`，所以环境变量应该设置为 http://127.0.0.1:7870/v1

+ 本 Demo没有设置 API, 因此`api_key`参数设置为`EMPTY`。

```
python client.py --api_key EMPTY --base_url http://127.0.0.1:7870/v1 --server_name 127.0.0.1 --server_port 7860 --model cogagent1.5-9b
```

通过上述命令，你将能在本地运行客户端，连接到服务端，并且使用`cogagent1.5-9b`模型。

![](img/7921da0fee6c5c3e14c040e69adedd19.png)

> 我们无法保证AI的行为的安全性，请在使用时谨慎操作。本示例仅供学术参考，我们不承担由本示例引起的任何法责任。
>
> 模型运行中，你可以随时按下`stop`强制停止模型当前的操作。如果你认为当前模型执行正常，没有风险，请不要触碰电脑，模型需要根据实时的电脑截图来确定点击坐标，这是模型正常运行的必要条件。
