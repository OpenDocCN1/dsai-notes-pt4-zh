# 第一节 数据加载

> 来源：[https://gotoaiworld.feishu.cn/docx/KNb3drNlLokNT2xw9gRcpfmInmh](https://gotoaiworld.feishu.cn/docx/KNb3drNlLokNT2xw9gRcpfmInmh)

# [第一节 数据加载](https://datawhalechina.github.io/all-in-rag/#/chapter2/04_data_load?id=%e7%ac%ac%e4%b8%80%e8%8a%82-%e6%95%b0%e6%8d%ae%e5%8a%a0%e8%bd%bd-1)

> 虽然本节内容在实际应用中非常重要，但是由于各种文档加载器的迭代更新，以及各类 AI 应用的不同需求，具体选择需要根据实际情况。本节仅作简单引入，但请务必重视数据加载环节，“垃圾进，垃圾出 (Garbage In, Garbage Out)” ——高质量输入是高质量输出的前提。

## [一、文档加载器](https://datawhalechina.github.io/all-in-rag/#/chapter2/04_data_load?id=%e4%b8%80%e3%80%81%e6%96%87%e6%a1%a3%e5%8a%a0%e8%bd%bd%e5%99%a8-1)

在RAG系统中，数据加载是整个流水线的第一步，也是不可或缺的一步。文档加载器负责将各种格式的非结构化文档（如PDF、Word、Markdown、HTML等）转换为程序可以处理的结构化数据。数据加载的质量会直接影响后续的索引构建、检索效果和最终的生成质量。

### [1.1 主要功能](https://datawhalechina.github.io/all-in-rag/#/chapter2/04_data_load?id=_11-%e4%b8%bb%e8%a6%81%e5%8a%9f%e8%83%bd-1)

+   文档格式解析 将不同格式的文档（如PDF、Word、Markdown等）解析为文本内容。

+   元数据提取 在解析文档内容的同时，提取相关的元数据信息，如文档来源、页码等。

+   统一数据格式 将解析后的内容转换为统一的数据格式，便于后续处理。

### 1.2 当前主流RAG文档加载器

## 二、Unstructured文档处理库

[Unstructured](https://docs.unstructured.io/open-source/) 是一个专业的文档处理库，专门设计用于RAG和AI微调场景的非结构化数据预处理。提供了统一的接口来处理多种文档格式，是目前应用较广泛的文档加载解决方案之一。

### 2.1 Unstructured的核心优势

格式支持广泛

+   支持多种文档格式：PDF、Word、Excel、HTML、Markdown等

+   统一的API接口，无需为不同格式编写不同代码

智能内容解析

+   自动识别文档结构：标题、段落、表格、列表等

+   保留文档元数据信息

### [2.2](https://datawhalechina.github.io/all-in-rag/#/chapter2/04_data_load?id=_22-%e6%94%af%e6%8c%81%e7%9a%84%e6%96%87%e6%a1%a3%e5%85%83%e7%b4%a0%e7%b1%bb%e5%9e%8b)[支持的文档元素类型](https://docs.unstructured.io/open-source/concepts/document-elements)

Unstructured能够识别和分类以下文档元素：

> 注： CompositeElement 是通过分块（chunking）处理产生的特殊元素类型，由一个或多个连续的文本元素组合而成。例如，多个列表项可能会被组合成一个单独的块。

## 三、从LangChain封装到原始Unstructured

在第一章的示例中，我们使用了LangChain的UnstructuredMarkdownLoader，它是LangChain对Unstructured库的封装。接下来展示如何直接使用Unstructured库，这样可以获得更大的灵活性和控制力。

### [3.1 代码示例](https://datawhalechina.github.io/all-in-rag/#/chapter2/04_data_load?id=_31-%e4%bb%a3%e7%a0%81%e7%a4%ba%e4%be%8b)

创建一个简单的示例，尝试使用Unstructured库加载并解析一个PDF文件：

> 若代码运行出现报错 ImportError: libgl.so.1 cannot open shared object file no such file or directory, 执行 sudo apt-get install python3-opencv 安装依赖库。

```
from unstructured.partition.auto import partition

# PDF文件路径
pdf_path = &quot;../../data/C2/pdf/rag.pdf&quot;# 使用Unstructured加载并解析PDF文档
elements = partition(
    filename=pdf_path,
    content_type=&quot;application/pdf&quot;)# 打印解析结果print(f&quot;解析完成: {len(elements)} 个元素, {sum(len(str(e)) for e in elements)} 字符&quot;)# 统计元素类型from collections import Counter
types = Counter(e.category for e in elements)print(f&quot;元素类型: {dict(types)}&quot;)# 显示所有元素print(&quot;\n所有元素:&quot;)for i, element in enumerate(elements, 1):print(f&quot;Element {i} ({element.category}):&quot;)print(element)print(&quot;=&quot; * 60)Copy to clipboardErrorCopied
```

partition函数参数解析：

+   filename: 文档文件路径，支持本地文件路径

+   content_type: 可选参数，指定MIME类型（如"application/pdf"），可绕过自动文件类型检测

+   file: 可选参数，文件对象，与filename二选一使用

+   url: 可选参数，远程文档URL，支持直接处理网络文档

+   include_page_breaks: 布尔值，是否在输出中包含页面分隔符

+   strategy: 处理策略，可选"auto"、"fast"、"hi_res"等

+   encoding: 文本编码格式，默认自动检测

partition函数使用自动文件类型检测，内部会根据文件类型路由到对应的专用函数（如PDF文件会调用partition_pdf）。如果需要更专业的PDF处理，可以直接使用from unstructured.partition.pdf import partition_pdf，它提供更多PDF特有的参数选项，如OCR语言设置、图像提取、表格结构推理等高级功能，同时性能更优。

> 完整代码文件：01_unstructured_example.py

> Unstructured官方文档

## [练习](https://datawhalechina.github.io/all-in-rag/#/chapter2/04_data_load?id=%e7%bb%83%e4%b9%a0)

+   使用partition_pdf替换当前partition函数并分别尝试用hi_res和ocr_only进行解析，观察输出结果有何变化。