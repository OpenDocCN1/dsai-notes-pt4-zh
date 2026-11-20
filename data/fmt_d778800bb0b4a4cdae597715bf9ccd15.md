
# 2023-11-05 LocalGPT:构建本地问答知识库

> 来源：[https://xuqiwei1986.feishu.cn/docx/EYwkdgDCRoKu5Axuf0HcFZjNnhf](https://xuqiwei1986.feishu.cn/docx/EYwkdgDCRoKu5Axuf0HcFZjNnhf)

> 🔗 原文链接：https://articles.zsxq.com/id_lnng6izbk1uw.html

大家好，我是WS,工作10年+的TOP级网络信息安全大厂研发，业余时间大部分投入到AI领域。现在是最大的AI社区破局俱乐部合伙人，本次AI编程小册的主创之一，也是本星球的嘉宾之一。

在信息安全和AI领域先后获得一些荣誉证书：

- 通信工业协会高级AI安全证书
- (Career Essentials in Generative Al by Microsoft and LinkedIn证书
- ClSP-ClSE证书
- 工信部高级网络信息安全工程师证书
- 工信部人工智能算法证书

担任过某知识星球编程赛道的分享嘉宾

为某创业公司写过AI领域公号文案

多次参加线下AI大会

欢迎链接我vx: wsvictoria

今天的干货来了，往下看。

LocalGPT 是一项开源计划，可让你在不泄露隐私的情况下与本地文档交谈，进行文档的检索和问答。所有内容都在本地运行，没有数据离开你的计算机。该项目灵感源自最初的`privateGPT`，它采用了`Vicuna-7B`模型，代替了`GPT4ALL`模型，同时使用`InstructorEmbeddings`替代了原始`privateGPT`中的`LlamaEmbeddings`。此外，`Embeddings`和`LLM`都能在`GPU`上高效运行，同时支持`CPU`。

项目基于 `LangChain`、`Vicuna-7B`和`InstructorEmbeddings` 模型构建

项目地址：`https://github.com/PromtEngineer/localGPT`

## 核心功能

1. 本地文档导入：将各种文档格式导入工具，包括 `txt`、`pdf`、`csv` 等。
2. 问题提问：通过提问获取准确的答案，无需联网。
3. 隐私保护：所有数据都在本地环境中进行处理，不会传输到任何外部服务器。
4. 多模型支持：可以根据需求选择不同的语言模型进行问答，支持多个 `HuggingFace` 模型。

## 环境设置

1. 必须安装`Python 3.10`或更高版本。早期版本的`Python`将无法编译。

```
conda create --name localGPT python=3.10
```

2. `Activate`启用

3. 进入到本地你想要保存此项目的目录：

![](img/7831839de798b9cbf82c90e7dba13527.png)

克隆项目代码：

```
git clone https://github.com/PromtEngineer/localGPT.git
```

安装依赖：

```
cd localGPT
pip install -r requirements.txt
```

## 引入测试数据集

将文件放到`SOURCE_DOCUMENTS`文件夹中。您可以在 `SOURCE_DOCUMENTS` 文件夹中放置多个文件夹，代码将以递归方式读取您的文件。默认支持的文件格式有`.txt`、`.pdf`、`.csv`和`.xlsx`。如果你的系统上有`cuda`设置，直接运行以下命令引入所有数据。

将看到如下输出：

![](img/825cadc924de2e4f2f9d2d32cb46a7eb.png)

默认使用的设备类型为`cuda`，可以使用设备类型参数来指定给定设备。

```
python ingest.py --device_type cpu
```

这将创建一个名为`DB`的新文件夹，并将其用于新创建的矢量存储。您可以根据需要引入任意数量的文档，所有文档都将累积在本地`embeddings`数据库中。如果要从空数据库开始，删除`DB`并重新引入你的文档数据。

注意：当你首次运行此功能，它需要访问互联网才能下载模型（默认：`Instructor Embedding`），所以可能需要一些时间。在后续的运行中，没有数据会离开你的本地环境，可以在没有互联网连接的情况下运行。

## 在本地对您的文件提出问题

1. 在您选择的编辑器中打开 `constants.py`，并根据选择添加您要使用的 `LLM`。默认情况下，将使用以下模型：

```
MODEL_ID = "TheBloke/Llama-2-7B-Chat-GGML"
MODEL_BASENAME = "llama-2-7b-chat.ggmlv3.q4_0.bin"
```

打开终端并激活包含从 `requirements.txt` 安装的依赖项的`Python`环境`localGPT`。

导航到 `/localGPT` 目录

运行以下命令

```
python run_localGPT_API.py
```

等到所有内容都加载完毕。您应该看到类似 `INFO:werkzeug:Press CTRL+C to quit.` 的内容。

![](img/cdbaa711157ab023a8c7b2a28d72254f.png)

6. 打开第二个终端并激活相同的 `python` 环境。

7. 导航到 `/localGPT/localGPTUI` 目录。

8. 运行命令

![](img/d90d9485afc8393a7c40504f635531e0.png)

9. 打开网络浏览器并转到地址：`http://127.0.0.1:5111`

![](img/a2094f84697e7d0c0ae6f631444d04fc.png)

在本地对您的文件提出问题，如我本地文件为`pdf`格式的《Training Big Random Forests with Little Resources》。

![](img/0920a11875e2eab1455d66ca5a432645.png)

我提问问题为："what is the Large-Scale Construction"

等待片刻，针对我们的问题，就会给出`Answer`，且一并给出答案相关的4个上下文来源，展示答案所在文件和答案所在段落。

![](img/c24c4a17aeea5aa38d80a66e2b1af3bf.png)

当然也可以选择不通过网页，在后台直接提问的形式来问答。按照下述步骤进行即可。在`localGPT`根目录执行命令

并等待脚本要求您输入。

![](img/06963accdb55a5c88c59816b516523e6.png)

点击进入。等待 `LLM` 模型使用提示并准备答案。完成后，它将打印答案，然后您可以问另一个问题而无需重新运行脚本，只需再次等待提示即可。注意：第一次运行时，需要连接互联网才能下载`vicuna-7B` 模型。之后您可以关闭互联网连接，脚本推理仍然有效。没有数据会从您的本地环境中泄露出去。输入 `exit` 完成脚本。

## 工作原理

使用合适的本地模型结合 `LangChain` 的高效功能，可以实现整体流程在本地执行，确保数据不离开您的系统，且运行效率满意。`ingest.py` 利用 `LangChain` 功能对文档进行解析并在 `InstructorEmbeddings`中生成嵌入。接着，`Chroma` 矢量储存工具被用于在本地数据库保存这些结果。`run_localGPT.py` 则采用本地的`LLM`（此处为`Vicuna-7B`）以理解问题并生成答复。答案的背景信息是从本地矢量储存中提取的，利用相似度搜索在文档中找到合适的背景片段。你也可以根据需求将这里的本地`LLM`切换为`HuggingFace`的其他`LLM`版本，只要确保你挑选的`LLM`是`HF`格式即可。

![](img/a0029528f8767924a0a65d1ddbfb7a07.png)

扫码加入星球

查看更多优质内容

`https://wx.zsxq.com/mweb/views/joingroup/join_group.html?group_id=48884185121218`
