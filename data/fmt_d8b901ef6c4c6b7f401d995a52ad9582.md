
# 手把手教你实现大模型RAG[5]-基于langchain的文本切割

> 来源：https://agijuejin.feishu.cn/docx/U20Edb6jboqX1qx6MLicvjQfnWf

> 作者：aC大

> 原文：https://zhuanlan.zhihu.com/p/695644216

## 前言

《手把手教你实现大模型RAG》系列文章分享的是从0到1基于langchain实现大模型结合本地知识库的问答系统。主要内容包括：

+ [1]-手把手教你实现大模型RAG-大模型应用技术介绍
+ [2]-手把手教你实现大模型RAG-RAG流程的系统介绍
+ [3]-手把手教你实现大模型RAG-UI实现
+ [4]-手把手教你实现大模型RAG-RAG类的实现
+ [5]-手把手教你实现大模型RAG-基于langchain的文本切割
+ [6]-手把手教你实现大模型RAG-基于langchain的文本向量化
+ [7]-手把手教你实现大模型RAG-第三方大模型接口接入
+ [8]-手把手教你实现大模型RAG-chatglm3本地部署openai标准接口及接入
+ [9]-手把手教你实现大模型RAG-环境变量的配置和加载

本系列文章会逐一分享上述内容，同时作者也出了该系列的视频课程，见 B站《手把手教你实现大模型RAG-acedar讲师》，文章和视频结合学习效果更佳。

## 基于langchain的文本切割

本地知识的文本切割是检索中的核心模块之一，这部分的处理结果会直接影响到检索的效果，进而影响最终的问答效果。这部分包括了文件的处理，文本的切割。

+ 文件的处理：本地知识文件的处理包括了初始化时加载和在对话过程中新增本地文件，暂时支持了pdf，csv和json文件的解析，同时如果上传知识库给的是目录，则依次解析目录下的各个文件。
+ 文本的切割：文本的切割同样使用langchain提供的类，langchain提供的文本切割方式有很多，包括：`CharacterTextSplitter`，`MarkdownHeaderTextSplitter`，`HTMLHeaderTextSplitter`，`NLTKTextSplitter`，`RecursiveCharacterTextSplitter`，`SpacyTextSplitter`，`KonlpyTextSplitter`，`MarkdownTextSplitter`，`LatexTextSplitter`等，对不同的文件使用不同的切割方式，比如`MarkdownHeaderTextSplitter`是对markdown文件的切割，对不同的文件采用不同的切割方式主要是不同的为了保证切割信息的完整性。

文件加载和切割的代码如下：

```
import os
from langchain.text_splitter import CharacterTextSplitter
from langchain.document_loaders.pdf import PyPDFLoader
from langchain.document_loaders.csv_loader import CSVLoader
from langchain.document_loaders.json_loader import JSONLoader

class Text2embedStore(object):

    def __init__(self, model_name):
        self.model_name = model_name
        file_path = os.environ.get("FILE_PATH")

        self.text_split = None
        self.text_split_init()
        if file_path:
            self.load_dir(file_path)

    def text_split_init(self):
        text_split_name = os.environ.get("TEXT_SPLIT_METHOD")
        chunk_size = int(os.environ.get("CHUNK_SIZE", 256))
        chunk_overlap = int(os.environ.get("CHUNK_OVERLAP", 128))
        if text_split_name == 'char':
            self.text_split = CharacterTextSplitter(chunk_size=chunk_size, chunk_overlap=chunk_overlap)
        else:
            self.text_split = CharacterTextSplitter(chunk_size=chunk_size, chunk_overlap=chunk_overlap)

    def load_dir(self, file_path):
        if os.path.isdir(file_path):
            for root, ds, fs in os.walk(file_path):
                for f in fs:
                    self.load_file(file_name=f)
        else:
            self.load_file(file_path)

    def load_file(self, file_name):
        print("file_name", file_name)
        if file_name.endswith('.pdf') or file_name.endswith('.PDF'):
            loader = PyPDFLoader(file_name)
        elif file_name.endswith('.csv') or file_name.endswith('.CSV'):
            loader = CSVLoader(file_name)
        elif file_name.endswith('.json'):
            # [{"text": ...}, {"text": ...}, {"text": ...}] -> schema =.[].text
            # {"key": [{"text": ...}, {"text": ...}, {"text": ...}]} -> schema =.key[].text
            # ["", "", ""] -> schema =.[]
            # todo: 提前读取json文件，解析schema
            loader = JSONLoader(file_name, '.key[].text')
        else:
            raise ValueError('not support this type file, {}, you can upload file type: {}'.format(
                file_name, ['pdf', 'csv', 'json']))

        documents = loader.load()
        docs = self.text_split.split_documents(documents)
        # todo 向量化并持久化
```

更多相关内容，感兴趣的朋友可以同步关注B站(视频)：acedar-C大，知乎&👸号：aC大，也可加v进群交流：aa836304831

发布于 2024-05-03 08:31 ・IP 属地广东
