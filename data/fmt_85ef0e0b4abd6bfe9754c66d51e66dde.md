
## 2. Step 1：提取文本与结构化内容

```
elements = partition_pdf(
    filename=pdf_path,
    infer_table_structure=True,   # 开启表格结构检测
    strategy="hi_res",            # 高分辨率 OCR，适合复杂表格
    ocr_languages="chi_sim+eng",  # 中英文混合识别
    ocr_engine="paddleocr"        # 指定 PaddleOCR 引擎
)
```

+   **核心作用**：调用 `Unstructured` 的分区解析，把 PDF 切分为标题、正文、表格、图片等元素。

+   `infer_table_structure=True` → 表格会被解析成结构化数据。

+   `strategy="hi_res"` → 高分辨率 OCR，能更好地识别复杂排版（比如学术论文）。

+   `ocr_languages="chi_sim+eng"` → 同时支持中文简体和英文。

+   `ocr_engine="paddleocr"` → 使用 `PaddleOCR` 引擎（比默认 `Tesseract` 更强大）。

返回的 `elements` 是一个 `Element` 列表，每个元素有：

+   `.category` → 类型（`Title`、`Paragraph`、`Table`、`Image` …）

+   `.text` → 文本内容

+   `.metadata` → 页码、坐标、表格的 HTML 等信息

## 3. Step 2：提取图片并保存

```
doc = fitz.open(pdf_path)
image_map = {}  # 映射 page_num -> list of image paths

for page_num, page in enumerate(doc, start=1):
    image_map[page_num] = []
    for img_index, img in enumerate(page.get_images(full=True), start=1):
        xref = img[0]
        pix = fitz.Pixmap(doc, xref)
        img_path = os.path.join(output_dir, f"page{page_num}_img{img_index}.png")
        if pix.n < 5:  # RGB / Gray
            pix.save(img_path)
        else:  # CMYK 转 RGB
            pix = fitz.Pixmap(fitz.csRGB, pix)
            pix.save(img_path)
        image_map[page_num].append(img_path)
```

+   使用 `fitz` 遍历 PDF 的每一页，提取图片。

+   每张图片保存为 `page{页码}_img{索引}.png`。

+   `pix.n < 5` → 说明是 `RGB` 或灰度图，可以直接保存；

+   否则是 `CMYK` 色彩空间，需要转为 `RGB` 再保存。

+   最后存入 `image_map`，便于后续和 Markdown 内容对应。

## 4. Step 3：组装 Markdown 文档

```
md_lines = []
inserted_images = set()  # 避免重复插入相同图片

for el in elements:
    cat = el.category
    text = el.text
    page_num = el.metadata.page_number
```

+   遍历前面解析出的所有 `elements`，根据不同类型拼接 Markdown。

### (1) 标题

```
    if cat == "Title" and text.strip().startswith("- "):
        md_lines.append(text + "\n")
    elif cat == "Title":
        md_lines.append(f"# {text}\n")
    elif cat in ["Header", "Subheader"]:
        md_lines.append(f"## {text}\n")
```

+   `Title` → 转换为 Markdown 一级标题 `#`。

+   `Header` / `Subheader` → 转换为二级标题 `##`。

+   特殊情况：如果标题开头是 `-`，说明其实是列表项而不是标题，直接保持原样。

### (2) 表格

```
    elif cat == "Table":
        if hasattr(el.metadata, "text_as_html") and el.metadata.text_as_html:
            from html2text import html2text
            md_lines.append(html2text(el.metadata.text_as_html) + "\n")
        else:
            md_lines.append(el.text + "\n")
```

+   如果表格有 HTML 格式（`text_as_html`），用 `html2text` 转换为 Markdown 表格。

+   否则直接写入文本。

### (3) 图片

```
    elif cat == "Image":
        for img_path in image_map.get(page_num, []):
            if img_path not in inserted_images:
                inserted_images.add(img_path)
```

+   对 `Image` 元素，插入对应的图片路径。

+   使用 `inserted_images` 避免重复添加相同的图片。

### (4) 普通文本

```
    else:
        md_lines.append(text + "\n")
```

+   其余情况（正文段落等）直接作为普通文本写入。

## 5. Step 4：写入 Markdown 文件

```
output_md = "output.md"
with open(output_md, "w", encoding="utf-8") as f:
    f.write("\n".join(md_lines))

print(f"✅ 转换完成，已生成 {output_md} 和 {output_dir}/ 图片文件夹")
```

+   把拼接好的 Markdown 行写入 `output.md`。

+   所有图片保存在 `pdf_images/` 文件夹中。

+   最终得到一个结构化良好的 Markdown 文件 + 图片资源目录，可直接用于 `RAG`。

## 三、搭建基于多模态 Markdown 文档的 Agentic RAG 检索引擎

在跑通了多模态文档转化之后，接下来我们基于转化后的多模态 Markdown 文档来创建一个 Agentic RAG 引擎。项目完整代码如下：

![](img/213f547570e19e7a2f624d8ca502c766.png)

![](img/e405e7445e55e846eb74c5ad22db1abe.png)

### 1. 基础环境搭建

+   创建项目主目录

![](img/160eebb476bd004257ef91d2bff3afd1.png)

+   安装基础依赖

```
pydantic
python-dotenv
langgraph
langchain-core
langchain-deepseek
langchain-tavily
langsmith
langchain-openai
langchain-text-splitters
langchain-community
faiss-cpu
langgraph_supervisor
graphrag
```

![](img/496cbe182a316169ee094c8f44680561.png)

输入如下命令完成安装：

```
pip install -r requirements.txt
```

![](img/54756c187ed11e6ffa9f6587b505066d.png)

+   配置环境变量

+   创建 `.env` 文件，并输入如下 API-KEY

```
DEEPSEEK_API_KEY=sk-c1a253**
OPENAI_API_KEY=sk-proj-gE**
LANGSMITH_TRACING=true
LANGSMITH_API_KEY=lsv2_pt_b44**
LANGSMITH_PROJECT=langraph_studio_chatbot
```

![](img/5cef1f2d25309761cb1b0fb811c80aea.png)

+   安装前端框架 `Agent Chat UI`

```
# git config --global http.proxy http://127.0.0.1:10080
# git config --global https.proxy http://127.0.0.1:10080

git clone https://github.com/langchain-ai/agent-chat-ui.git

cd agent-chat-ui
```

![](img/cacac3a3b3451c503367261d5591493d.png)

然后安装前端依赖：

```
pnpm install
```

![](img/89d4477d1bc509e72b8fff497df45514.png)

安装 LangGraph 项目部署工具：

```
pip install -U "langgraph-cli[inmem]"
```

### 2. 知识库检索数据集准备

接下来我们继续准备检索用的数据集，这里我们采用此前系列公开课《MCP技术实战》课件作为检索材料，课件总共约 6 万字，内容如下：

![](img/a60dc3b0e5dee485103f7f0202165858.png)

![](img/c1fc1a5abe859dac9aa573bacbb7b8a6.png)

网盘对应地址如下：

![](img/e9283956eb15ab565049177f5215fca9.png)

接下来我们需要先将其逆向转化为 `md` 文档：

```
import os
import fitz
from unstructured.partition.pdf import partition_pdf

pdf_path = "MCP实战课件【合集】.pdf"
output_dir = "pdf_images"
os.makedirs(output_dir, exist_ok=True)

# Step 1: 提取文本/结构化内容
elements = partition_pdf(
    filename=pdf_path,
    infer_table_structure=True,   # 开启表格结构检测
    strategy="hi_res",            # 高分辨率 OCR，适合复杂表格
    ocr_languages="chi_sim+eng",  # 中英文混合识别
    ocr_engine="paddleocr"        # 指定 PaddleOCR 引擎
)

# Step 2: 提取图片并保存
doc = fitz.open(pdf_path)
image_map = {}  # 映射 page_num -> list of image paths

for page_num, page in enumerate(doc, start=1):
    image_map[page_num] = []
    for img_index, img in enumerate(page.get_images(full=True), start=1):
        xref = img[0]
        pix = fitz.Pixmap(doc, xref)
        img_path = os.path.join(output_dir, f"page{page_num}_img{img_index}.png")
        if pix.n < 5:  # RGB / Gray
            pix.save(img_path)
        else:  # CMYK 转 RGB
            pix = fitz.Pixmap(fitz.csRGB, pix)
            pix.save(img_path)
        image_map[page_num].append(img_path)
# Step 3: 转换为 Markdown
md_lines = []
inserted_images = set()  # 用来记录已经插入过的图片，避免重复

for el in elements:
    cat = el.category
    text = el.text
    page_num = el.metadata.page_number

    if cat == "Title" and text.strip().startswith("- "):
        md_lines.append(text + "\n")
    elif cat == "Title":
        md_lines.append(f"# {text}\n")
    elif cat in ["Header", "Subheader"]:
        md_lines.append(f"## {text}\n")
    elif cat == "Table":
        if hasattr(el.metadata, "text_as_html") and el.metadata.text_as_html:
            from html2text import html2text
            md_lines.append(html2text(el.metadata.text_as_html) + "\n")
        else:
            md_lines.append(el.text + "\n")
    elif cat == "Image":
        # 避免重复插入：只插入当前图片对应的文件
        for img_path in image_map.get(page_num, []):
            if img_path not in inserted_images:
                inserted_images.add(img_path)
    else:
        md_lines.append(text + "\n")

# Step 4: 写入 Markdown 文件
output_md = "output.md"
with open(output_md, "w", encoding="utf-8") as f:
    f.write("\n".join(md_lines))

print(f"✅ 转换完成，已生成 {output_md} 和 {output_dir}/ 图片文件夹")
```

生成 `md` 文档：

![](img/f812f06db080f36e8afeb631e2a889de.png)

![](img/5c6b5a1e705f8b31453db685302d1490.png)

> 注，`md` 版课件统一进行了图床上传，并且按照正则规则进行了数据清洗，整体文档结构更加规范。

然后，为了搭建 `RAG` 系统，我们还需要对原始文档进行处理，来创建词向量数据库：
