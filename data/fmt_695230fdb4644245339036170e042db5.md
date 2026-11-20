
# 新手必备！Cursor 1.0 完全上手指南：从零开始用 AI 玩MCP

> 来源：[https://aigcreative.feishu.cn/docx/D9RLd74ywo2PD8xlB3NcgxlBnSf](https://aigcreative.feishu.cn/docx/D9RLd74ywo2PD8xlB3NcgxlBnSf)
>
> 链接: https://mp.weixin.qq.com/s/izEnzkLoLvVpI7eY852MIA
>
> 作者：成峰        👸号：AI 产品自由

各位朋友，最近很多人跟我说Cursor升级到1.0之后，界面大改，很多功能都找不到了。

今天我就从零开始，把Cursor 1.0的使用方法完整地讲一遍。

以下内容基于macOS系统，Windows用户操作基本一致。

# 下载安装

下载地址：https://www.cursor.com/

![](img/807a7a4cc8496894a7a1175a4d0edc01.png)

进入官网，点击中间那个黑色的Download按钮即可。

![](img/a40f6d1d9c17c4d7f66624ead0bed3e0.png)

# 核心功能介绍

打开Cursor后，你会看到一个全新的界面。

![](img/e8bffc7b4fd7d55f86ba5f51e19ad9f1.png)

注意右上角的四个按钮，这是整个软件的核心：

![](img/ddbf18b883b77992021bd51148f2d842.png)

如下图解释的

![](img/21208b7036912e36b9ad00d35f6f87be.png)

下面解释各个按钮的作用

## 1. 文件夹按钮

打开项目文件夹

![](img/0e3e0dab4aee4b4be504e09190c0bc39.png)

## 2. 终端按钮

命令行工具（新手暂时用不到）

![](img/a49915466bdf6ad3c2a340a33e4a3c41.png)

## 3. AI对话按钮

重要！这是使用AI的唯一入口

![](img/d8eac2f6253af26dd0da23380d9c144a.png)

## 4. 设置按钮

所有配置都在这里

![](img/3e8ced608266386f4aa89ef916a88524.png)

# 设置中文环境

## 安装中文插件

1. 点击`文件夹`按钮

![](img/22e6b5e398abe6f8dd78e9e41bb559d9.png)

2. 点击顶部第四个扩展按钮

![](img/b5e0308e3e142f73cbcb6eccdc105089.png)

3. 搜索框输入`Chinese`，选择`简体中文`插件并安装

![](img/c3eaded24ba94f8c023c7e222bf7b8bd.png)

4. 安装完成后，点击`Change Language and Restart`重启

![](img/fb84f4ceb27b2a185e1130e626f66888.png)

注意：界面可能中英文混杂，这是因为插件还没完全适配新版本。

![](img/f47c52eca6085d9272d510f65eb697f4.png)

## 让AI用中文回复

打开设置，找到`Add Rule`按钮：

![](img/d08ca98180017b6364cfa14196d877f4.png)

点击添加以下规则

```
Always respond in 中文
```

参考下图的顺序

![](img/aa6e995e00ebc0b4e240c5f9014e84fb.png)

点击`Done`保存。这样AI就会默认用中文回复了。

# 理解Cursor的工作方式

我们先要了解一个概念：

## Cursor 是基于项目文件夹的 AI 编程工具

和Word、PPT这些单文件编辑器不同，Cursor的设计理念是以整个文件夹（项目）为工作单位。

❌ 错误方式：直接打开单个文件 ✅ 正确方式：打开整个项目文件夹

为什么要这样？

因为Cursor会扫描整个项目结构，理解文件之间的关系，这样AI才能更准确地帮助你。

## 创建第一个项目

### 新建项目文件夹

1. 点击“打开文件夹”按钮

![](img/985c05efd05935dfff2b3416161ba037.png)

2. 新建一个空白文件夹，并打开

![](img/5e0dfea5615c7c7eef496363d025d65a.png)

我们就能看到 cursor打开了一个空白文件夹

![](img/6d247b190a2b35188f21293f9e4ea6e1.png)

## 添加文件

鼠标悬停在文件夹区域，会出现四个小按钮：

+ 新建文件
+ 新建文件夹
+ 刷新
+ 折叠文件夹

点击`新建文件`，创建一个`.md`格式的文件。

![](img/3211b9cc016f1c8c462a7f70007f6009.png)

为什么用`.md`作为尾缀？

因为这种格式对AI来说可读性极高

![](img/b1caf02c3b6466a4cffd1b33a6f1239f.png)

## 在系统中查看文件

在cursor里面，我们点击文件，就能打开

那么不在cursor，怎么打开呢

右键点击文件，选择`在文件夹中打开`

![](img/6bb6d767418c1045288a4bdd14b78dcd.png)

就能在系统文件管理器中看到文件了

![](img/dd766f65179175749e1f5261200e6b41.png)

# MCP的基础设置

## 关闭烦人的提醒

每次使用MCP都要确认很烦！我们来关掉它：

我们按照下图顺序，打开`chat`栏

![](img/0b102848e98a44d88748425b75c33016.png)

向下滑，找到 `Auto-Run`

再找到`MCP Tools Protection`，然后关闭他

![](img/d2ac8625f0c0abc2bfbe214e7521a3f0.png)

## 添加MCP工具

我们在设置里，找到 `Tools & Integrations`

![](img/250917be9be6131c9ae499ee79e3eac4.png)

就会看到下面的窗口，

![](img/f726d534ce69beee09ef4a7ca2721461.png)

如果要添加新的MCP，点击添加

![](img/f6faf30a418fb54ea38cf060271edaa6.png)

点击了以后，系统会打开MCP配置文件

![](img/9ea1474b22791c981b8827effee4ee88.png)

我先找一个MCP加进去演示一下

这就是添加了MCP的样式

![](img/95ff6450417e1fa2da620d4a610f7e7a.png)

那怎么添加多个MCP呢？

在不同的MCP之间，添加英文版逗号

![](img/faea94558353ed3c5861210f21b61cf4.png)

推荐做法，把MCP配置粘贴进去后，让AI给你修改格式

下面会讲，怎么操作AI了

# 开始使用AI

我们唤起AI对话窗口

![](img/995ad55e7fe8e480474bf9040ec9b44d.png)

我们先来看一下，这里有什么功能

![](img/43372f24f0800c8354153c006f38177e.png)

## 模式选择

这里提供了三种模式，我们默认选择`Agent`

`Ask`是对话模式，`Manual`是手动模式，一般用不到。

![](img/4b0d334c97242a50c76d4dcf6945f7dc.png)

## 模型选择

cursor里面有很多种模型，我们关掉 `Auto`和`MAX Mode`，这两项都是额外收费项目

只打开 `claude-4-sonnet`

![](img/d6a578d1717d36cb176e81e6d69dd859.png)

## 引用文件

我们怎么选择对应的文件，发送给cursor呢，有以下几种方式

### 1. 选择文本/代码

我们鼠标滑动对应的文本，然后点击`Add to chat`

![](img/58794f7f2f253282c7c6671448d011ee.png)

右边就有对应文本，被选中的提示

### 2. @相关文件

我们点击`@`，他会展示文件选择列表

![](img/df3725e5269c72efa1f0c564d2048630.png)

我们可以选择 点击第一个 `files&foldders`，然后选择对应的文件

![](img/2fb5d0cb8f90012a65c010e3f565d4eb.png)

### 3. 拉文件夹/文件

我们也可以直接从文件区，拉文件到对话窗口

![](img/ddf7e164e6a4262ee1ad942c07e90a1c.png)

# 开始对话

按照上面的教程跑完了以后，我们就可以开始跟AI对话了，在对话框窗口，输入内容就行

![](img/4631b8ca0f31e701a17998619c436068.png)

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>

# 知识库动态
