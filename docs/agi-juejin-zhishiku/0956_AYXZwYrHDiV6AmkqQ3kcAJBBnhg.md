# 手把手教你实现大模型RAG[5]-基于langchain的文本切割

> 来源：[https://agijuejin.feishu.cn/docx/U20Edb6jboqX1qx6MLicvjQfnWf](https://agijuejin.feishu.cn/docx/U20Edb6jboqX1qx6MLicvjQfnWf)

> 作者：aC大

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/695644216)hihu.com/p/695644...

### 前言：

《手把手教你实现大模型RAG》系列文章分享的是从0到1基[于lan](https://www.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695644216%7D)gchain实现大模型结合本地 知识库 的问答系统。主要内容包括：

+   [1]-手把手教你实现大模型RAG-大模型应用技术介绍

+   [2]-手把手教你实现大模型RAG-RAG流程的系统介绍

+   [3]-手把手教你实现大模型RAG-UI实现

+   [4]-手把手教你实现大模型RAG-RAG类的实现

+   [5]-手把手教你实现大模型RAG-基于langchain的文本切割

+   [6]-手把手教你实现大模型RAG-基于langchain的文本向量化

+   [7]-手把手教你实现大模型RAG-第三方大模型接口接入

+   [8]-手把手教你实现大模型RAG-chatglm3本地部署openai标准接口及接入

+   [9]-手把手教你实现大模型RAG-环境变量的配置和加载

本系列 [文章会 ](https://www.zhihu.com/search?q=%E6%96%87%E7%AB%A0%E4%BC%9A&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695644216%7D)逐一分享上述内容，同时作者也出了该系列的视频课程，见 B站《手把手教你实现大模型RAG-acedar讲师》 , 文章和视频结合学习效果更佳。

### 基于langchain的文本切割

本地知识的 [文本切割 ](https://www.zhihu.com/search?q=%E6%96%87%E6%9C%AC%E5%88%87%E5%89%B2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695644216%7D)是检索中的核心模块之一，这部分的处理结果会直接影响到检索的效果，进而影响最终的问答效果。 这部分包括了文件的处理，文本的切割。

+   文件的处理：本地知识文件的处理包括了初始化时加载和在对话过程中新增本地文件，暂时支持了pdf，csv和json文件的解析，同时如果上传知识库给的是目录，则依次解析目录下的各个文件。

+   文本的切割：文本的切割同样使用langchain提供的类，langchain提供的文本切割方式有很多，包括：CharacterTextSplitter，MarkdownHeaderTextSplitter，HTMLHeaderTextSplitter， NLTKTextSplitter，RecursiveCharacterTextSplitter，SpacyTextSplitter，KonlpyTextSplitter， MarkdownTextSplitter， LatexTextSplitter等，对不同的文件使用不同的切割方式， 比如MarkdownHeaderTextSplitter是对markdown文件的切割，对不同的文件采用不同的切割方式主要是不同的为了保证切割信息的完整性。

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
        file_path = os.environ.get(&quot;FILE_PATH&quot;)

        self.text_split = None
        self.text_split_init()
        if file_path:
            self.load_dir(file_path)

    def text_split_init(self):
        text_split_name = os.environ.get(&quot;TEXT_SPLIT_METHOD&quot;)
        chunk_size = int(os.environ.get(&quot;CHUNK_SIZE&quot;, 256))
        chunk_overlap = int(os.environ.get(&quot;CHUNK_OVERLAP&quot;, 128))
        if text_split_name == &#x27;char&#x27;:
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
        print(&quot;file_name&quot;, file_name)
        if file_name.endswith(&#x27;.pdf&#x27;) or file_name.endswith(&#x27;.PDF&#x27;):
            loader = PyPDFLoader(file_name)
        elif file_name.endswith(&#x27;.csv&#x27;) or file_name.endswith(&#x27;.CSV&#x27;):
            loader = CSVLoader(file_name)
        elif file_name.endswith(&#x27;.json&#x27;):
            # [{&quot;text&quot;: ...}, {&quot;text&quot;: ...}, {&quot;text&quot;: ...}] -&gt; schema =.[].text
            # {&quot;key&quot;: [{&quot;text&quot;: ...}, {&quot;text&quot;: ...}, {&quot;text&quot;: ...}]} -&gt; schema =.key[].text
            # [&quot;&quot;, &quot;&quot;, &quot;&quot;] -&gt; schema =.[]
            # todo: 提前读取json文件，解析schema
            loader = JSONLoader(file_name, &#x27;.key[].text&#x27;)
        else:
            raise ValueError(&#x27;not support this type file, {}, you can upload file type: {}&#x27;.format(
                file_name, [&#x27;pdf&#x27;, &#x27;csv&#x27;, &#x27;json&#x27;]))

        documents = loader.load()
        docs = self.text_split.split_documents(documents)
        # todo 向量化并持久化
```

更多相关内容，感兴趣的朋友可以同步关注B站(视频)：acedar-C大，知乎&公众号：aC大，也可加v进群交流：aa836304831

发布于 2024-05-03 08:31 ・IP 属地广东