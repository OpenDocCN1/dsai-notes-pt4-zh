# cherry studio搭建本地AI知识库

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/OHHpdns9MooEdaxI5slc4RzcnDf](https://qqs7y1hozd1.feishu.cn/docx/OHHpdns9MooEdaxI5slc4RzcnDf)

## 一、快速入门

**搭建步骤概览：**

![](img/68b4b7ce460afc6a6f138611dab692d2.png)

### **具体搭建实操步骤**

**步骤一： **下载Cherry Studio可视化工具

**步骤二： **部署DeepSeek模型 + Embedding模型

**那么，这里又分为三种方式**

方式一： 去官网，用Ollama工具

> **好处：** 可以实现本地部署DeepSeek模型 + Embedding模型，免费

> **坏处：** 需要一定的电脑配置才行，不然模型就很鸡肋

方式二： 使用硅基流动

> **好处： **不用本地部署

> **坏处：** 调用云端的模型服务需要一定的费用，但是也不贵。

方式三： 使用DeepSeek

> **好处： **不用本地部署

> **坏处： **调用云端的模型服务需要一定的费用，但是也不贵。

今天重点介绍的是第二种方式———硅基流动+Cherry Studio 搭建本地私有知识库。

## 二、CherryStudio配置

### 2.1. CherryStudio 集成 DeepDeek API（在线）

![](img/1f13c7ac507c056cce3e183d8c69680d.png)

![](img/a47385d6f4e0f62012342747057803a2.png)

![](img/dddcafef3dc502848a77f322c850a026.png)

![](img/0754d94847351777d6ee5bca955fe815.png)

![](img/d514861924c9db1094841574744d41b4.png)

![](img/82b610295622a0b78ccc161392db3665.png)

### 2.2. DeepSeek 服务体验

![](img/96e989a4aec82e54fde91f9250da42c1.png)

### 2.3. DeepSeek 模型切换

点击顶部模型名称，弹出模型列表，选择已配置模型即可，

这里演示DeepSeek R1 模型切换为DeepSeek V3模型

![](img/971e90335e4a20af3289f32ee465707d.png)

![](img/08c03ae7dc0612ca8d068746c3d49bc0.png)

## 三、CherryStudio 通过硅基流动 集成DeepSeek

### 3.1. 注册硅基流动账号

![](img/66c3cd2f92161363c1af3702f0c1f8bc.png)

### 3.2. 配置API密钥

输入API密钥后->点击检查->从模型列表中【选择R1模型】

![](img/fa924f84b0f7e2b58baeafa0438afb10.png)

打开服务开关

![](img/49c4e04dbcebe30acaafd0195a1e7566.png)

## 四、CherryStudio集成本地Ollama部署的DeepSeek

### 4.1. 配置本地Ollama DeepSeek

选择Ollama-打开服务开关

![](img/bd459526373e123b27c4b0e0623ac5b5.png)

点击【管理】后选择模型点击+号

![](img/f60a3592f7ddb8a9f4d5c55d9c572466.png)

![](img/9d036eb4c5ed30029ef2564acbaf2953.png)

### 4.2. 切换模型选择本地Ollama DeepSeek R1

从模型列表中，有3个厂商的模型服务，选择Ollama厂商的DeepSeek R1服务。

![](img/9a17fad1201e9d5bf5f7e80bdc944030.png)

### 4.3. 体验本地DeepSeek 服务

![](img/33b3e9d6f6a3540474b19ebd240a0e64.png)

## 五、私域知识库搭建

### 5.1. 添加DeepSeek推理模型

点击底部的 **【管理按钮】**，在模型管理服务中查找模型，点击【全部】默认就能看到；

找到需要的模型，点击右侧【+】添加到我的模型。

![](img/cf671572c9ed400b71a2e277c8166030.png)

![](img/b3648c93c36d08c59ceea6ec1c171b92.png)

![](img/6cd1fcd2971740cf2a05435674803171.png)

### 5.2. 创建知识库

知识库入口：在 **CherryStudio** 左侧工具栏，点击知识库图标，即可进入管理页面；

添加知识库：点击添加，开始创建知识库；

命名：输入知识库的名称并添加嵌入模型，以 BAAI/bge-m3 为例，即可完成创建。

![](img/8ed10a9c30e9e1e66e3d90b89d47138c.png)

### 5.3. 添加文件并向量化

**添加文件：**** 点击添加****文件的按**钮，打开文件选择；

选择文件： 选择支持的文件格式，如 pdf，docx，pptx，xlsx，txt，md，mdx 等，并打开；

向量化：系统会自动进行向量化处理，当显示完成时（绿色 ✓），代表向量化已完成

![](img/bbc4dc567690a9a3b49cea057830bfbe.png)

### 5.4. 添加多种来源的数据

![](img/034e6ec0de1be1256849c3db1b711f0d.png)

### 5.5. 搜索知识库

当文件等资料向量化完成后，即可进行查询：

点击页面下方的搜索知识库按钮；

+   输入查询的内容；

+   呈现搜索的结果；

+   并显示该条结果的匹配分数。

![](img/380a46b3f34f047c88c58e1ec4ab1d71.png)

### 5.6. 输入关键词搜索知识库

比如，这里我输入：”申请解除合同，应提供资料有哪些？“

```
申请解除合同，应提供资料有哪些？
```

![](img/d9a04076df47358b779f7f1a52eb230c.png)

### 5.7. 对话中引用知识库生成回复

创建一个新的话题，在对话工具栏中，点击知识库，会展开已经创建的知识库列表，选择需要引用的知识库；

输入并发送问题，模型即返回通过检索结果生成的答案 ；

同时，引用的数据来源会附在答案下方，可快捷查看源文件。

![](img/4d6f454a01b6a85fac3f3459b10d5197.png)

![](img/33dd5354337c16da96246a617feb5ef6.png)

## 六、优化

### 6.1. 添加提示词

然后，为了更好的使用知识库，

我们还可以再新建一个 助手 下面需要填写 提示词，

提示词太长，我把链接地址放下面，自己去复制即可。

知识库提示词

![](img/174461f9226f9b47f6310362a306c3d6.png)

![](img/fd748a805b4f9375e5109fd12e9a4e0a.png)

这里的prompt复制后，在对应的对话界面的提示词中粘贴即可。

![](img/e05ded91bf07b190c9f75e9c347639d5.png)

将提示词 粘贴进来，点击关闭后即可使用。

### 6.2. 打开折叠面板

正常思考会很长，可以选择将代码块折叠打开，不会占用太多排版。

![](img/c79bac0cd727b014260e30bf3f980d30.png)

## 七、结合DeepSeek实现RAG

本地知识库的搭建则是 RAG 技术的重要部分，利用 Cherry Studio 等平台，可以便捷地实现这一过程。

### 1、选择知识库

进行提问之前可以选择要检索的知识库。

![](img/dc1b271798ebacc25e92b27359fee1c2.png)

### 2、进行提问

可以看到大模型回答内容时参考了知识库中本人网站的内容：

![](img/168c45f5b3dc24a28b28becca4c25857.png)