
# MCP 驱动的 RAG 处理复杂文档

> 来源：[https://v11enp9ok1h.feishu.cn/docx/XtssdiNDBodOM9xqNCdcviVnnCw](https://v11enp9ok1h.feishu.cn/docx/XtssdiNDBodOM9xqNCdcviVnnCw)

这是我们的技术栈：

+   Cursor IDE 作为 MCP 客户端。
+   EyelevelAI的 GroundX 构建一个可以处理复杂文档的 MCP 服务器

![](img/fa2bc77233a52709889df639edad8ee0.png)

工作原理如下：

![](img/b8d09035671f119b5f8d3d1e73455022.png)

+   用户与 MCP 客户端（Cursor IDE）交互
+   客户端连接到 MCP 服务器并选择一个工具。
+   工具利用 GroundX 对文档进行高级搜索。
+   客户端使用搜索结果来生成响应。

## 代码

### 1）设置服务器

![](img/f46b8b3aa2a67d9cb0b21887b242c447.png)

### 2）创建GroundX客户端

![](img/6d9d353a5335c59b1cfef30264c9628a.png)

### 3）创建摄取工具

此工具用于将新文档引入知识库。

用户只需提供要摄取的文档的路径：

![](img/b63f590cd80b19125bf21c7f8ca18f1c.png)

### 4）创建搜索工具

该工具利用 GroundX 的高级功能对复杂的真实世界文档进行搜索和检索。

实现方法如下：

![](img/bbe1f7202fa1d210d2215a716099bc32.png)

### 5）启动服务器

使用标准输入/输出（`stdio`）作为传输机制启动 MCP 服务器：

![](img/80c141e9c66768b26c62006897492088.png)

### 6）连接到光标

在您的 Cursor IDE 中，请按照以下步骤操作：

+   光标 → 设置 → 光标设置 → MCP

然后像这样添加并启动您的服务器：

![](img/f10da33f8560e13cf82c16e25ada76cf.png)
