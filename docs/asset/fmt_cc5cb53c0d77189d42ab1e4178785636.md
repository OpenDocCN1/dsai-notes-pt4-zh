
# DeepSeek小白成长手册合集

> 来源：[https://xuqiwei1986.feishu.cn/docx/RZiEd7RZcokrBdxIkGicAFAsnpg](https://xuqiwei1986.feishu.cn/docx/RZiEd7RZcokrBdxIkGicAFAsnpg)

# 1、互联网小白步骤（适用90%的人）

## 1、deepseek官方网页web版

|  |
| --- |
| ![](img/7d5da117bd5ea70266eb0f31033ffd15.png) |

## 2、deepseek官方APP

| 【应用市场下载】搜索deelseek |
| --- |
| ![](img/fa86f716856f8cb1be2df63a24e8a66f.png) |

## 3、纳米AI搜索APP【备用，官方访问量大，经常拥堵用不了】

| 下载APP |
| --- |
| 切换到AI机器人菜单，deelseek标签下 |

# 2、官方API调用deepseek（适用9%的人）

第一步：打开deepseek官网

`https://www.deepseek.com/`

![](img/79badff5b543dec82072e477f69df9ff.png)

第二步：复制api

![](img/5b220fa4c9b671f449d2967523581ad3.png)

![](img/85e6a11eb62650b23cea1d6d501623f4.png)

打开注册

创建api

复制api

就一次机会

![](img/cc140afc9660e51ff6ebaa4dee3b9746.png)

![](img/90e377008319cf53f75904aead19a251.png)

第三步：打开安装VScode

`https://code.visualstudio.com/`

![](img/bdc81da42406061560d4c17808747ced.png)

### 三方API调用deepseek

说明：官网崩溃用不了的用云端，需要多模态（文本，图片，视频，音频）的不会部署的用云端！！！！

多模态云端平台（文本，图片，视频，音频）

DeepSeek模型已登陆硅基流动，现已支持DeepSeek-V3和R1模型，搭载华为昇腾云，爽用，不怕官网宕机，注册送2000w免费token：

`https://cloud.siliconflow.cn/models`

![](img/e0c1b4f292477c87ae7f4a19b4442e45.png)

# 3、本地部署大模型（适用1%的人，一般为企业）

大部分企业或者个人，使用中间层工具或应用层软件就可以满足需求。

1. 如果你的数据很敏感，不能联网，本地部署可以让你断网使用大模型。

比如（法律、金融、财税等，数据不能联网，需要本地部署）

2. 如果你的需求非常专业，你的私有数据量非常大，云端可能没有特别好的使用体验，你需要本地部署。

如果你只是用AI做获客营销、智能客服、私有知识库（数据不超过10G），没必要在本地部署大模型。

视频教程（哔哩哔哩）

`https://search.bilibili.com/all?keyword=deepseek%E6%9C%AC%E5%9C%B0%E9%83%A8%E7%BD%B2`

### 1.底层（大模型、显卡/算力）

直接底层部署：

部署671B全量大模型效果最好，高度可定制，但是成本太高，只适合有一定规模的企业，或者有特殊需求的行业（法律、金融、财税等，数据不能联网，需要本地部署）。

注意：如果你只用来问答的话，网页版/APP就可以了，个人电脑性能只能跑得动蒸馏模型7B/8B（可以理解为缩减版），使用效果远远不如网页版/APP。

### 2.中间层工具

例如coze，anythingLLM，lmstudio，直接调用主流大模型，同时能根据我们自己的需求进行定制。

中间层工具：目前大部分是免费的，而且选择极多，大部分需求都能找到合适的免费工具，还能根据自己的需求进行一定程度的定制。

### 3.应用层软件

直接给我们结果，比如mj，sd，即梦，可灵，数字人等等各类的软件。

应用层软件：目前市面上疯狂打广告的，基本都是应用层软件，这些软件虽然可以直接达到某些结果，但是往往都需要付费（价格普遍偏贵），而且无法根据我们的需求精细化定制和调整。

本地训练自己的模型，不需要联网，保护隐私。做企业的

很多敏感问题在线上无法解决需要本地话部署

## 第一步：先检查你的机器配置

DeepSeek开源了7个模型，其中一个671B版本是全量模型，和官方版是一样的。

另外六个是蒸馏模型（可以理解为缩小版，性能比原版模型要差一些，结尾数字越小性能越差）

具体模型和配置对应参数如图：

最底部第七个是全量模型，要求1000G显存

越往下性能越好，越往上性能越差

全量模型要求配置很高，普通电脑配置无法运行（推荐企业搭建服务器部署）

我们安装在个人电脑上的模型，推荐7B或8B版本（Windows电脑要求显卡显存8G以上，M芯片的Mac电脑要求内存16G以上）

注意：

运行所需显存

在Windows电脑上，是显卡的显存

在M芯片的Mac电脑上，指的是内存

![](img/2c9221e3ce12ea332d5cc4185a440b2b.png)

## 第二步：打开 ollama 官网

> 什么是ollama？olama是一个开源工具，专门用于本地计算机的运行大语言模型（LLM），它能够让客户轻松下载和管理，运用各行各业的AI模型，而无需复杂的配置依赖于云服务

官网：`https://ollama.com/`

备用下载：

`https://pan.baidu.com/s/1zmGJ76E99pFUE96GG8cRBw?pwd=s4ew` 提取码：`s4ew`

## 第三步：安装ollama

> 运行输入：`ollama run deepseek-r1`
> 删除：`ollama rm deepseek-r1`
> 查看下载的模型：`ollama list`
> 帮助：`ollama help`

![](img/b6e12ef3ced0206a81ffdb9f3c7b4519.png)

![](img/1e223df1b82b71e7a5bc543a929d65d2.png)

![](img/5e7c785d2415aa1eec262faaae7057c9.png)

## 第四步：运行教程

### 安装好后运行（Windows教程）

1）打开开始菜单，（快捷键`win+R`）在搜索框输入`cmd`

![](img/4ecd6787d727a2ecbb1d4d84f0c0f1fd.png)

2）安装DeepSeek大模型

继续在命令行输入

`ollama run deepseek-r1:7b`

（冒号后面的`7b`是版本号，代表图示中的第二个模型，版本号可以修改，根据你的电脑配置，需要安装哪个版本就输入哪个版本号）

等待下载安装完成，命令行出现success就代表安装成功了

![](img/42986c7600edfeef4bceb565de9ec868.png)

### 安装好后运行（Mac教程）

1）打开电脑自带的APP「终端」命令行

![](img/76e190db32e1e6b598fc5282b52b5d18.png)

2）打开命令行之后，输入

`ollama —version`

出现版本号，即代表安装成功

![](img/692e37882ff4fb10239ac5e6f9cff8c1.png)

3）本地运行窗口输入想要运用的模型代码

![](img/59371bb9b19c18807506e5f664e19326.png)

开始聊天

![](img/34f7ddfd9c9a16ea023a0fab80dc0131.png)

### 修改ollama安装地址

1. 打开系统属性：右键点击Windows桌面上的“此电脑”或“计算机”图标，选择“属性”以打开系统属性窗口。

2. 进入环境变量设置：在系统属性窗口中，点击左侧的“高级系统设置”按钮。在弹出的“系统属性”窗口中，切换到“高级”选项卡，然后点击“环境变量”按钮。

3. 新建或修改环境变量：在“环境变量”窗口中，可以在“系统变量”或“用户变量”部分找到或新建一个名为`OLLAMA_MODELS`的环境变量。如果已存在该变量，则直接选中并点击“编辑”按钮；如果不存在，则点击“新建”按钮来创建一个新的环境变量。

4. 设置新的存储路径：在“编辑环境变量”或“新建环境变量”窗口中，输入变量名`OLLAMA_MODELS`，并在“变量值”文本框中输入希望保存模型文件的新目录路径。例如，如果希望将模型文件保存在D盘的OllamaModels目录下，则输入 `D:\O1lamaodels`。

5. 重启Ollama：设置完环境变量后，需要重启Ollama以应用更改。可以通过任务管理器结束Ollama的进程，然后重新运行Ollama来实现重启。

注意事项：清除模型

代码

+ 备份重要数据：在删除模型之前，请确保你已经备份好了需要保留的数据。删除模型是一个不可逆的操作，一旦删除将无法恢复。

+ 版本差异：具体的命令和操作可能会因为OLLAMA的版本而有所不同，建议参考OLLAMA的官方文档或者使用`ollama --help`命令获取更多帮助信息。

+ `ollama --list`

![](img/5d6f3107de9665fd085ec27ee0d5abba.png)

通过以上步骤，你可以在OLLAMA中成功删除不需要的模型。

### 第四步：安装图形化界面chatbox

是一个开源的桌面应用程序，专门用于大模型open AI和GT等进行交互，它提供了一个简洁易用的图形界面，能够让用户轻松和AI模型对话，无需编写代码和复杂的命令工具。

1. 登录官方页面：`https://chatboxai.app/zh`

2. 点击运行网页版（使用桌面版也一样）

3. 点击左下角 Setting。

4. 如图所示：Model Provider：OPENAI，API Key：填写复制的API秘钥。

API Host：填写网址：`https://api.siliconflow.cn/v1`，点开 Model&Token--model选择 Custom Model。

再 Custom Model name 输入：`deepseek-ai/DeepSeek-R1`

![](img/712076ba4fdcbf0fdbf7277ab6b8018f.png)

# 4、使用进阶PDF

[`DeepSeek从入门到精通-清华大学-202502.pdf`](https://xuqiwei1986.feishu.cn/wiki/PZKfwizyUiYJ84kMUutcoHP9nCf)

[`DeepSeek赋能职场.pdf`](https://xuqiwei1986.feishu.cn/wiki/Lx4gw4VyEiepRbk2aoGcG1tonWb)
