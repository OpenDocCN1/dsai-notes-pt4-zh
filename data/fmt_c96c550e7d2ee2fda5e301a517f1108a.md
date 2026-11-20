
# Dify文件上传，图片/文档理解

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Ga0FdbKVMonv4TxBLk2ceWyknee](https://qqs7y1hozd1.feishu.cn/docx/Ga0FdbKVMonv4TxBLk2ceWyknee)

## 运行结果展示

![](img/1b34e0cfb63dbb6bc26c2c9a7481b45b.png)

## 下载DSL

上传多文件，图文理解工作流应用可以导出为DSL，方便你在Dify[工作区中导入查看整个应用的编排](https://personel-zhouxinle888-a66353926f9185cff28f2bd374a5c3a9dd89d5206.gitlab.io/dify/%E4%B8%8A%E4%BC%A0%E4%BC%A0%E5%A4%9A%E6%96%87%E4%BB%B6%EF%BC%8C%E5%9B%BE%E6%96%87%E7%90%86%E8%A7%A3.yml)。点击 `上传多文件，图文理解.yml` 下载。

创建应用时，选择导入DSL文件即可。

## 新建应用

新建ChatFlow应用。

## 重要节点

### 开始节点

> 开始节点设置

+ 编排区域右上角功能打开文件上传。设置最大上传文件数量为3，支持文件类型为文档和图片。

+ 用户在聊天框中上传文档和图片文件，就会被赋值给`sys.files`变量。`sys.files`是`Array[File]`类型。

![](img/1cb6561578b307c2f883b2ee59b36e14.png)

### 筛选图片

由于开始节点中，用户上传了多种类型的文件，文档和图片。

所以需要列表操作节点将文档和图片筛选出来，分别处理。

过滤条件是`type = 图片`，也就是筛选出类型为图片的文件。

![](img/bf5c36e959786b21f0d129f92b0ec7f1.png)

注意

列表操作节点输出变量中有`result` 过滤结果，是一个数组类型，因为用户上传的文件中可能存在多个图片。所以筛选出的图片可能有多个。

这个应用，我们只上传了一张图片，所以只需要使用`last_record` 最后一条记录。

### 筛选文档

和筛选图片一样，使用列表操作节点，筛选出类型为文档的文件。

![](img/9c9366374255f30948d235e7973ff22d.png)

### 图片总结

这是一个LLM节点，由于需要理解图片，所以这个LLM节点需要选择支持vision视觉理解的多模态模型。我这里使用的是GPT-4o mini。

![](img/982a4e2b40929c7240a102a6c20c953a.png)

![](img/8a52927e9aa75a0ff9d9191a9a4e06c7.png)

### 文档提取器

在对文档使用LLM节点进行总结之前，需要一个文档提取器节点将`File`类型的文档中的内容提取出来，因为LLM节点并不能直接接收`File`类型的变量。

![](img/d56896593468b350b0d257da63f098a4.png)

### 文档总结

![](img/37fc330e5336897048f55fdeb8cd6344.png)

### 模版转换

模版转换节点将图片总结和文档总结节点的结果进行汇总，便于输出结果。

![](img/eae1015dd813eb07a36b83cb0107ff9c.png)

### 直接回复

![](img/e25c47f401720d2deafce041c4f55e7a.png)
