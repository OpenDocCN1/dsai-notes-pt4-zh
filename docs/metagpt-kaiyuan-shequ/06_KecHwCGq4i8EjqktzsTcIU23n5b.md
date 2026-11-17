# 保姆级教程 Windows

> 来源：[https://deepwisdom.feishu.cn/docx/RNFjdF1lGoRBqPx1lOncf5KxnXL](https://deepwisdom.feishu.cn/docx/RNFjdF1lGoRBqPx1lOncf5KxnXL)

> 原文指路：MetaGPT 保姆级安装教程：代码小白看完也能成功安装

**首先，****你得准备**一台电脑（注：本教程是基于微软系统操作，Mac 系统教程等小编发工资了买台 MacBook 给大家做）

## 准备工作

### Python 安装

前往 https://www.python.org/downloads/windows/ 完成 Python3.10.11 安装（版本号可以自选，推荐 3.9 或 3.10）

![](img/adc48afef480cf9306a1dfe7c6dc06f7.png)

### VS Code 安装

前往 https://code.visualstudio.com/ 安装 Visual Studio Code， 这将作为我们之后代码的工作台。

![](img/763b806d723742456df8a4a76cc86079.png)

![](img/ad99a8c576758fa8926024ca6be90f63.png)

### git 安装

前往 https://git-scm.com/downloads 完成 git 安装

![](img/9c7c127a0262e2ebb3178b6782cb628b.png)

![](img/00210eaf6926ec5a5be895b58c78a80f.png)


![](img/16705be12f8cd3cd71efaa8ef81a5d76.png)

### npm 安装

前往 https://nodejs.org/en/download/ 完成 Node.js 安装

![](img/bed10c6fb568eae04634158c7fc044e5.png)

好了，完成以上的安装工作，我们就已经完成 1/3 了，总的来说就是完成以下四个应用的安装；当然你还需要一个🪜的工具，如果没有可以去开源咨询。

![](img/18b9f457a1810417e1c2a89a59a2c4ba.png)

## 安装 MetaGPT

打开刚刚安装的 VS Code，点击 Clone Git Repository，提前将 MetaGPT 的代码仓库复制好：https://github.com/geekan/MetaGPT



完成下载后，左侧可以看到 METAGPT 的代码库


找到 Terminal，创建一个新的 Terminal


Terminal 如下


在 Terminal 内输入以下这段代码，然后运行

```
pip install -e .
#如需使用RAG模块，继续执行以下代码：
pip install -e .[rag]
```


完成以上两个的指令安装后，准备好你的 OpenAI 的 API KEY，然后在 VS Code 里，在 METAGPT 目录下，找到 config.yaml 文件，将你的 API KEY 更新到文档内。


## Ready To GO!

完成以上所有工作后，我们就可以开始运行我们的 MetaGPT 了（默认为 MG 软件公司多智能体团队）。

同样，在 Terminal 内输入 metagpt + “你的需求”，以下代码是贪吃蛇游戏的例子，可以直接复制运行：

```
metagpt &quot;write a cli snake game&quot;
```



等待运行完成后，即可在左侧找到 workspace\snake_game 的路径，里面就包括所有的输出（产品文档、技术文档、以及代码等）


最后，运行 main.py 文件即可


![](img/9445a45a9f15321690d8dc7656e561e2.png)

## metagpt --help 命令行使用说明

![](img/40c9b63a825bdab5ca54e34f2301fb43.png)