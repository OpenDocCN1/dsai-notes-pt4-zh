
# MateGen：新一代交互式智能编程助手

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/F5UTdNcnKo5cN8xiPjdcK9annUb](https://kq4b3vgg5b.feishu.cn/docx/F5UTdNcnKo5cN8xiPjdcK9annUb)

![](img/9ef99c17254b824ee6248e848b0d3974.png)

## MateGen是什么？

`MateGen`是由九天老师大模型教研团队开发的交互式智能编程助手（Interactive Intelligent Programming Assistant），可在`Jupyter`代码环境中便捷调用，辅助用户高效完成智能数据分析、机器学习、深度学习和大模型开发等工作，并可根据用户实际需求定制知识库和拓展功能。

`MateGen`基础功能如下：

+ 🤖高易用性，零门槛调用：`MateGen`为用户提供在线大模型应用服务，无需任何硬件或网络代理门槛，即可一键安装并开启对话；

+ 🚀强悍的高精度`RAG`系统：一键同步本地文档并进行`RAG`检索问答，最多支持1000篇文档以及10G文档内容进行检索，支持`md`、`ppt`、`word`、`pdf`等主流文档格式高精度问答，能够高效率实现包括海量文档总结、大海捞针内容测试、情感倾向测试问答等功能。`MateGen`可根据用户问题自动识别是否需要进行`RAG`检索；

+ 🏅本地`Python`代码解释器：可连接用户本地`Python`环境完成编程任务，包括数据清洗、数据可视化、机器学习、深度学习、大模型开发等代码工作编写，支持先学习代码库再进行编程、能够根据实际情况`debug`，支持可视化图片自动上传图床等功能；

+ 🚩高精度`NL2SQL`功能：可根据用户需求编写`SQL`，并连接本地`MySQL`环境自动执行，可自动`debug`，并且支持先检索数据字典、企业数据知识库再进行`SQL`编写，从而提高`SQL`编写精度；

+ 🛩️视觉能力和联网能力：对话时输入图片网址即可开启`MateGen`视觉能力对图片内容进行识别，同时`MateGen`也具备联网能力，当遇到无法回答的问题时，可自动开启搜索问答模式；

+ 🚅无限对话上下文：`MateGen`拥有无限上下文对话长度，`MateGen`会根据历史对话的未知信息密度进行合理处理，从而在节省`token`的同时实现无限对话上下文；

除此之外，`MateGen`具备高稳定性与高可用性，同时支持`Multi Function calling`（一个任务开启多个功能）和`Parallel Function calling`（一个功能开多个执行器），能够自动分解复杂任务、自动`Debug`，并且拥有一定程度“自主意识”，能够审查自身行为并深度挖掘用户意图。

## MateGen 系列产品介绍

截至目前，我们推出了`MateGen API`、`MateGen Air`和`MateGen Pro`三款产品，各产品介绍如下：

+ `MateGen Air`：是一款基于`JupyterLab`的智能编程插件，也是全球首款`JupyterLab`的智能编程插件，轻量级实现`MateGen`的各项功能，旨在为数据技术人和`Python`用户提供更加智能便捷的编程环境，无任何使用门槛，且完全免费；

+ `MateGen API`：拥有完整的`MateGen`各项功能，在`Python`环境通过`sdk`进行调用，是旗舰款`Agent API`，需要`MateGen API-KEY`才能使用；

![](img/33aaffd53127418dcd1d3d0f7bdcfffb.png)

+ `MateGen pro`：拥有完整的`MateGen`各项功能，且拥有企业级大模型项目前端`UI`，和`MateGen API`能力相同，同样也需要`MateGen API-KEY`才能使用。

![](img/871d99cbc10f7f1bef5a50977ae7f0b8.png)

## MateGen Air安装和部署流程

### MateGen Air安装

`MateGen`是一款基于`JupyterLab`的智能编程插件，只需`pip`即可安装使用，非常便捷：

```
conda create -n mategenair jupyterlab 
conda activate mategenair
pip install mategenair
jupyter lab
```

> 上述代码在命令行中运行，且需要提前安装好`anaconda`。

安装后即可打开`Jupyter`，可以借助`Luncher`登陆：

![](img/7ba1bf90140bd01da53582fe89e93278.png)

也可以使用命令进行登陆，按住`control+shift+c`：

![](img/09c41230811ecc5187e1b5d068692fcf.png)

然后在弹出的页面登录即可：

![](img/835280bc0c557cbc8a061d5dde68a257.png)

### MateGen Air基础对话功能

![](img/1604339757dc62a501b600a6daf24f40.png)

其中也可以选择正式课程或公开课进行问答，从而实现智能助教功能。（目前只上线了少量知识库，更多知识库功能还在完善中...）

![](img/abcecba900b616e5bea29320fb073cfc.png)

### MateGen 智能编程功能

在登录`MateGen Air`的同时，打开`Jupyter`页面，点击左侧`cell`的小灯泡，即可开启智能编程相关功能：

![](img/32da39bfcd59129c34f2f92e5a216425.png)

> 由于`JupyterLab`内核原因，第一行`cell`或最后一行`cell`使用该功能可能会导致响应报错，推荐创建多个`cell`，然后在中间的`cell`执行该功能。

此外，若遇到代码报错，则会自动弹出代码修复按钮，点击即可将代码和报错信息同步到右侧聊天框，并快速进行`debug`：

![](img/2cd15bdead70fd48a694d2e45eefa6ae.png)

---

## MateGen Pro部署和使用流程

+ Step 1. 安装`Docker Desktop`

`Docker Desktop`支持最新的`Windows 10/11`和`MacOS`版本，访问`Docker Desktop`官方下载页面，根据操作系统下载相应版本的安装程序，双击下载的安装文件，并按提示操作完成安装。

![](img/8247952d52421bae39eb04e78898b656.png)

+ Step 2. 启动并验证`Docker Desktop`

安装完成后，打开`Docker Desktop`应用程序，应用会提示登录，但登录不是必须的，可以选择跳过此步骤直接使用。

![](img/5ca0b8b1fb7db8afcc7fca7b789a3f48.png)

检查`Docker Desktop`主界面左下角，确认`Docker Engine`显示为“`Running`”状态。这表明`Docker Engine`已经成功启动并正在运行。如果`Docker Engine`状态不是“`Running`”，可手动开启或需要重新启动`Docker Desktop`。

![](img/6f19d1bb070c16f2c24a83f3a0688399.png)

+ Step 3. 运行`MateGen Pro`项目

如果是`Windows`系统，在这里打开命令行终端：

![](img/c483a9b371caf9c9ad9955891dafbcb4.png)

如果是`Mac`，在这里打开命令行终端：

![](img/6ab491a4278413ebd46a33889f7beb7d.png)

首先下载`MateGen Pro`最新的项目镜像，执行命令如下：

注意：对于国内网络，`Docker`和`Github`类似，经常会访问不通导致拉取失败，如多次尝试仍然无法拉取镜像，请开启🪜环境

```
docker pull snowball1996/mategen:latest 
```

这一步将从`Docker Hub`中指定的镜像仓库下载最新的`MateGen Pro`镜像。

![](img/4e47594762513ffb2b594481ce03023f.png)

下载`MateGen`项目文件：

```
git clone https://github.com/fufankeji/MateGen.git
```

![](img/b01adde1f23771996f9f454cd22b8717.png)

启动`MateGen Pro`项目，首次启动会拉取依赖的镜像文件

```
cd MateGen && docker compose up
```

![](img/4bdde1e8b324d5c17336d7d1c5ab5178.png)

启动日志中看到如下信息，则说明服务启动成功。

![](img/e45e6b3fb91b295536d96f2c1e3ce13c.png)

此时，可在本机的浏览器上通过`http://0.0.0.0:9000/`进行访问

![](img/1c0b51e2b28d8936ff6bb4d285c3fa3e.png)

对话测试：

![](img/2597a54d43546b3af4147d9f43896e30.png)

### 关闭服务方法

进入命令行终端：

![](img/83bcddf1df7541d49d20c5d2cf38aace.png)

进入下载的`MateGen`项目文件目录下，执行代码：

```
cd MateGen  # 这里替换为自己实际目录位置

docker compose down
```

![](img/4ba03bb25de541d829d48ecacd61b20f.png)

### 开启服务方法

进入下载的`MateGen`项目文件目录下，执行代码：

```
cd MateGen  # 这里替换为自己实际目录位置

docker compose up
```

启动成功后，通过`localhost:9000`端口执行访问。

### 特殊异常解决方案

如果成功部署启动后，首次对话出现生成失败情况，即如下：

![](img/c3f4d97e0b7cf24608743dd977326ae6.png)

请依次按照如下操作步骤：

1. 关闭`MateGen Pro`服务，在终端执行：

```
docker compose down
```

![](img/6ee8c0e227c2c3ff3346bd153ab0017c.png)

1. 删除`Mategen Pro`和`Mysql`的旧镜像，删除方法如下：

```
Docker images -a

docker rmi xxx  xxx   # 这里填写对应的镜像ID
```

![](img/3acd553c331dce5f4cd5cb1a86fe1449.png)

1. 重新拉取最新版本的`MateGen Pro`镜像

```
docker pull snowball1996/mategen:latest 
```

![](img/ddb87d9e07d0879beb06ed2ce86b5e50.png)

1. 拉取镜像后，启动，通过`localhost:9000`端口执行访问（建议清空一下浏览器的缓存）

如果经过上述操作依然存在该问题，在服务启动的状态下在浏览器访问该地址进行数据库重置操作：

```
localhost:9000/api/delete_database
```

![](img/36ccd27a722b32007ea931c7e449385d.png)

然后回到控制台，重新启动`MateGen Pro`服务

```
# 先关闭
docker compose down

# 再启动
docker compose up
```

通过`localhost:9000`端口执行访问，注意：此时需要清空一下浏览器的缓存，将弹出重新输入`API_Key`的验证框。

---

## MateGen API-KEY获取

`MateGen Air`无需`API-KEY`即可使用，但是功能更加完整且性能强劲的`MateGen API`、`MateGen Pro`需要`API`才可使用。`API-KEY`领取、加入技术交流群、其他任何问题，扫码添加产品助手花花，回复“`MG`”详询哦👇

![](img/4a28ffd3497fc9e2d7a5719eaca44156.png)

欢迎课程学员和新老用户多多支持本项目，项目`star`超过10k即上线开源版及教学教程！

## MateGen API使用效果演示

注：各项演示操作可参考《`MateGen`使用教程》中相关代码来实现。

[`MateGen`使用教程.pdf](https://kq4b3vgg5b.feishu.cn/file/SRmnbGWKbo1Db2xSVoycmDfdnGf?from=from_copylink)

[`MateGen`使用教程【代码文件】.ipynb](https://kq4b3vgg5b.feishu.cn/file/GyLCbtieHohV7Dxoa4OcqOaLnLc?from=from_copylink)

+ 零门槛便捷调用

只需三步即可在`Jupyter`中调用`MateGen`：安装、导入、对话！

![](img/ccb553460a3e4a7edb19a4237180f8b4.png)

+ 本地海量文本知识库问答

借助`MateGen`，可实现高精度本地知识库问答，`MateGen`在`RAG`系统最多支持1000个文本+10G规模文本检索！

![](img/75286c040f28cf8e4cc1ffbb76ee8b78.png)

+ 交互式可视化绘图

`MateGen`同时具备视觉能力和本地代码解释器功能，因此可以根据用户输入的图片，模仿绘制！

![](img/38d253b91f9958f4280bfd9eb905e39f.png)

+ 高精度`NL2SQL`

`MateGen`支持全自动`RAG`+`NL2SQL`联合执行，因此可以先从知识库中了解数据集字段信息和业务信息然后再编写`SQL`，并且支持自动审查与自动`debug`，从而大幅提高`SQL`准确率。

![](img/4208559a856169c5b6a99f87ce88163f.png)

+ 自动机器学习

`MateGen`支持全自动`RAG`+代码解释器联合执行，支持先阅读企业机器学习代码库再进行机器学习建模，通过自然语言一键调用不同机器学习建模策略，创建你的机器学习“贾维斯”。

![](img/202345ec9e7bc97945ef0af341bb5824.png)

+ 前沿深度学习论文解读与架构复现

基于自身强大的`RAG`系统以及`Multi-Function`功能，`MateGen`能够进行深度论文辅导，可以帮助用户逐段翻译和解读论文—>总结论文核心知识点—>一键编写百行代码代码复现论文架构并在本地代码环境直接运行验证！

![](img/b9e25537eb505bdafe434065a80b12cf.png)

+ Kaggle竞赛辅导

借助`MateGen`的联网能力+知识库问答能力+`NL2Python`能力，`MateGen`还可辅助用户参与`Kaggle`竞赛。`MateGen`可以根据用户提供的赛题，自动获取赛题解释与数据集解释信息，自动爬取赛题高分`Kernel`并组建竞赛知识库，然后辅助用户进行竞赛编程，并自动提交比赛结果至`Kaggle`平台，最终根据提交结果提示用户调整竞赛策略，从而冲击更高分数！

![](img/5b0a079d3c7e1a2f0bb9e2edcbd4d0c5.png)

+ 智能助教

基于`MateGen`强悍的海量文本检索问答能力以及代码能力，一个储备了课程课件知识库的`MateGen`完全可以作为一名智能助教，学习前可以辅助用户进行课前预习、指定学习计划，学习中可以7*24小时实时答疑、随时辅助用户完成编程或其他代码任务，课后还可以根据用户课中提问来编写习题、分析用户薄弱知识点并将其总结为课后复习文档！

![](img/ab74815b736fce18f438af7853717d49.png)

`MateGen`更多应用场景即将上线。

## MateGen安装部署流程

`MateGen`项目轻便、调用简单，可使用`pip`直接进行安装，同时调用风格类似于`sklearn`，实例化一个`MateGen`智能体即可直接开启对话！

+ `MateGen`下载方法

`MateGen`现已在`PyPI`平台上线，可以直接通过`pip install mategen`进行安装，需要注意的是，`MateGen`运行所需依赖较多，因此推荐借助虚拟环境进行安装。首先创建一个名为`mategen`的虚拟环境：

```
conda create -n mategen python=3.8
```

然后使用如下指令激活虚拟环境：

```
conda activate mategen
```

接着在虚拟环境中安装`MateGen`：

```
pip install mategen
```

安装完成之后，考虑到需要在`Jupyter`中调用`MateGen`，我们还需要在虚拟环境中安装`IPython Kernel`：

```
pip install ipykernel
```

并且将这个虚拟环境添加到`Jupyter`的`Kernel`列表中：

```
python -m ipykernel install --user --name mategen --display-name "mategen"
```

然后开启`Jupyter`服务：

```
jupyter lab
```

然后在`Jupyter`的`Kernel`中选择`mategen`，即可进入到对应虚拟环境运行`MateGen`：

![](img/e98ebd3310a46d584dd3e68badf16f35.png)

+ `MateGen`调用方法

`MateGen`调用过程非常简单，只需要在代码环境中导入并输入有效的`API-KEY`即可开启对话！



# MateGen 快速开始

`mategen = MateGenClass(api_key = 'YOUR_API_KEY')`

然后即可使用`chat`功能进行单次对话或多轮对话：

`mategen.chat("你好，很高兴见到你！")`

` ▌ MateGen`初始化完成，欢迎使用！

你好！很高兴见到你！有什么我可以帮助你的吗？

更多`MateGen`使用方法，详见《`MateGen`使用教程》。

`API-KEY`领取、加入技术交流群、其他任何问题，扫码添加产品助手花花，回复“`MG`”详询哦👇

![](img/0c5b2d2137fd45a9a77b9657c9d2e0fc.png)

欢迎课程学员和新老用户多多支持本项目，项目`star`超过10k即上线开源版及教学教程！

## MateGen架构与应用说明

### MateGen基本架构

`MateGen`采用了目前最先进的threads-runs架构，以更好的进行用户历史消息对话管理以及自动修复运行中遇到的各种问题，同时采用了`client`与`server`分离架构，以确保最大程度`Agent`运行稳定性，同时支持多种不同类型底层大模型，`MateGen`基本结构如下：

![](img/f5394381ccc523c900abfdfbfe3e62fb.png)

### MateGen用于智能助教

`MateGen`同时可适用于多种不同类型具体业务场景，例如`MateGen`现已用于九天老师团队各门课程辅助教学环节，用于智能助教。`MateGen`充当智能助教的基本功能执行流程如下：

![](img/299f344fbe538a21b55c93b7997fce22.png)
