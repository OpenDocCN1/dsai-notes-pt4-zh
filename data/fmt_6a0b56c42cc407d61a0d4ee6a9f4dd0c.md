
```python
# 设置 API 配置
api_key = "your-api-key"
base_url = "your-base-url"  # 可选

# 创建 RAGAnything 配置
config = RAGAnythingConfig(
    working_dir="./rag_storage",
    enable_image_processing=True,
    enable_table_processing=True,
    enable_equation_processing=True,
)

# 定义模型函数
def llm_model_func(prompt, system_prompt=None, history_messages=[], **kwargs):
    return openai_complete_if_cache(
        "gpt-4o-mini",
        prompt,
        system_prompt=system_prompt,
        history_messages=history_messages,
        api_key=api_key,
        base_url=base_url,
        **kwargs,
    )

def vision_model_func(prompt, system_prompt=None, history_messages=[], image_data=None, messages=None, **kwargs):
    # 如果提供了messages格式（用于多模态VLM增强查询），直接使用
    if messages:
        return openai_complete_if_cache(
            "gpt-4o",
            "",
            system_prompt=None,
            history_messages=[],
            messages=messages,
            api_key=api_key,
            base_url=base_url,
            **kwargs,
        )
    # 传统单图片格式
    elif image_data:
        return openai_complete_if_cache(
            "gpt-4o",
            "",
            system_prompt=None,
            history_messages=[],
            messages=[
                {"role": "system", "content": system_prompt} if system_prompt else None,
                {
                    "role": "user",
                    "content": [
                        {"type": "text", "text": prompt},
                        {"type": "image_url", "image_url": {"url": f"data:image/jpeg;base64,{image_data}"}}
                    ],
                } if image_data else {"role": "user", "content": prompt},
            ],
            api_key=api_key,
            base_url=base_url,
            **kwargs,
        )
    # 纯文本格式
    else:
        return llm_model_func(prompt, system_prompt, history_messages, **kwargs)

embedding_func = EmbeddingFunc(
    embedding_dim=3072,
    max_token_size=8192,
    func=lambda texts: openai_embed(
        texts,
        model="text-embedding-3-large",
        api_key=api_key,
        base_url=base_url,
    ),
)

# 初始化 RAGAnything
rag = RAGAnything(
    config=config,
    llm_model_func=llm_model_func,
    vision_model_func=vision_model_func,
    embedding_func=embedding_func,
)

# 示例：来自外部源的预解析内容列表
content_list = [
    {
        "type": "text",
        "text": "这是我们研究论文的引言部分。",
        "page_idx": 0  # 此内容出现的页码
    },
    {
        "type": "image",
        "img_path": "/absolute/path/to/figure1.jpg",  # 重要：使用绝对路径
        "image_caption": ["图1：系统架构"],
        "image_footnote": ["来源：作者原创设计"],
        "page_idx": 1  # 此图像出现的页码
    },
    {
        "type": "table",
        "table_body": "| 方法 | 准确率 | F1分数 |\n|------|--------|--------|\n| 我们的方法 | 95.2% | 0.94 |\n| 基准方法 | 87.3% | 0.85 |",
        "table_caption": ["表1：性能对比"],
        "table_footnote": ["测试数据集结果"],
        "page_idx": 2  # 此表格出现的页码
    },
    {
        "type": "equation",
        "latex": "P(d|q) = \\frac{P(q|d) \\cdot P(d)}{P(q)}",
        "text": "文档相关性概率公式",
        "page_idx": 3  # 此公式出现的页码
    },
    {
        "type": "text",
        "text": "总之，我们的方法在所有指标上都表现出优越的性能。",
        "page_idx": 4  # 此内容出现的页码
    }
]

# 直接插入内容列表
await rag.insert_content_list(
    content_list=content_list,
    file_path="research_paper.pdf",  # 用于引用的参考文件名
    split_by_character=None,         # 可选的文本分割
    split_by_character_only=False,   # 可选的文本分割模式
    doc_id=None,                     # 可选的自定义文档ID（如果未提供将自动生成）
    display_stats=True               # 显示内容统计信息
)

# 查询插入的内容
result = await rag.aquery(
    "研究中提到的主要发现和性能指标是什么？",
    mode="hybrid"
)

print("查询结果:", result)

# 您也可以使用不同的文档ID插入多个内容列表
another_content_list = [
    {
        "type": "text",
        "text": "这是来自另一个文档的内容。",
        "page_idx": 0  # 此内容出现的页码
    },
    {
        "type": "table",
        "table_body": "| 特性 | 值 |\n|------|----|\n| 速度 | 快速 |\n| 准确性 | 高 |",
        "table_caption": ["特性对比"],
        "page_idx": 1  # 此表格出现的页码
    }
]

await rag.insert_content_list(
    content_list=another_content_list,
    file_path="another_document.pdf",
    doc_id="custom-doc-id-123"  # 自定义文档ID
)

if __name__ == "__main__":
    asyncio.run(insert_content_list_example())
```

## 内容列表格式

`content_list` 应遵循标准格式，每个项目都是包含以下内容的字典：

+   文本内容: `{"type": "text", "text": "内容文本", "page_idx": 0}`
+   图像内容: `{"type": "image", "img_path": "/absolute/path/to/image.jpg", "image_caption": ["标题"], "image_footnote": ["注释"], "page_idx": 1}`
+   表格内容: `{"type": "table", "table_body": "markdown表格", "table_caption": ["标题"], "table_footnote": ["注释"], "page_idx": 2}`
+   公式内容: `{"type": "equation", "latex": "LaTeX公式", "text": "描述", "page_idx": 3}`
+   通用内容: `{"type": "custom_type", "content": "任何内容", "page_idx": 4}`

## 重要说明

+   `img_path`: 必须是图像文件的绝对路径（例如：`/home/user/images/chart.jpg` 或 `C:\Users\user\images\chart.jpg`）
+   `page_idx`: 表示内容在原始文档中出现的页码（从0开始的索引）
+   内容顺序: 项目按照在列表中出现的顺序进行处理

## 适用场景

此方法在以下情况下特别有用：

+   您有来自外部解析器的内容（非MinerU/Docling）
+   您想要处理程序化生成的内容
+   您需要将来自多个源的内容插入到单个知识库中
+   您有想要重用的缓存解析结果

---

## 🛠️ 示例

### 实际应用演示

`examples/` 目录包含完整的使用示例：

+   `raganything_example.py`：基于MinerU的端到端文档处理
+   `modalprocessors_example.py`：直接多模态内容处理
+   `office_document_test.py`：Office文档解析测试（无需API密钥）
+   `image_format_test.py`：图像格式解析测试（无需API密钥）
+   `text_format_test.py`：文本格式解析测试（无需API密钥）

### 运行示例

`python examples/raganything_example.py path/to/document.pdf --api-key YOUR_API_KEY --parser mineru`  # 端到端处理（包含解析器选择）

`python examples/modalprocessors_example.py --api-key YOUR_API_KEY`  # 直接模态处理

`python examples/office_document_test.py --file path/to/document.docx`  # Office文档解析测试（仅MinerU功能）

`python examples/image_format_test.py --file path/to/image.bmp`  # 图像格式解析测试（仅MinerU功能）

`python examples/text_format_test.py --file path/to/document.md`  # 文本格式解析测试（仅MinerU功能）

`python examples/office_document_test.py --check-libreoffice --file dummy`  # 检查LibreOffice安装

`python examples/image_format_test.py --check-pillow --file dummy`  # 检查PIL/Pillow安装

`python examples/text_format_test.py --check-reportlab --file dummy`  # 检查ReportLab安装

> 注意：API密钥仅在完整RAG处理和LLM集成时需要。解析测试文件（`office_document_test.py`、`image_format_test.py` 和 `text_format_test.py`）仅测试MinerU功能，无需API密钥。

---

## 🔧 配置

### 系统优化参数

#### 环境变量

创建 `.env` 文件（参考 `.env.example`）：

```
OPENAI_API_KEY=your_openai_api_key
OPENAI_BASE_URL=your_base_url  # 可选
OUTPUT_DIR=./output             # 解析文档的默认输出目录
PARSER=mineru                   # 解析器选择：mineru 或 docling
PARSE_METHOD=auto              # 解析方法：auto, ocr 或 txt
```

注意：为了向后兼容，旧的环境变量名称仍然有效：

+   `MINERU_PARSE_METHOD` 已弃用，请使用 `PARSE_METHOD`

#### 解析器配置

RAGAnything 现在支持多种解析器，每种解析器都有其特定的优势：

##### MinerU 解析器

+   支持PDF、图像、Office文档等多种格式
+   强大的OCR和表格提取能力
+   支持GPU加速

##### Docling 解析器

+   专门优化Office文档和HTML文件的解析
+   更好的文档结构保持
+   原生支持多种Office格式

#### MinerU配置

MinerU 2.0使用命令行参数而不是配置文件。查看可用选项：

```bash
mineru --help
```

常用配置：

```bash
mineru -p input.pdf -o output_dir -m auto    # 自动解析模式
mineru -p input.pdf -o output_dir -m ocr     # OCR重点解析
mineru -p input.pdf -o output_dir -b pipeline --device cuda  # GPU加速
```

你也可以通过RAGAnything参数配置解析：

# 基础解析配置和解析器选择

```python
await rag.process_document_complete(
    file_path="document.pdf",
    output_dir="./output/",
    parse_method="auto",          # 或 "ocr", "txt"
    parser="mineru"               # 可选："mineru" 或 "docling"
)
```

# 高级解析配置（包含特殊参数）

```python
await rag.process_document_complete(
    file_path="document.pdf",
    output_dir="./output/",
    parse_method="auto",          # 解析方法："auto", "ocr", "txt"
    parser="mineru",              # 解析器选择："mineru" 或 "docling"
    # MinerU特殊参数 - 支持的所有kwargs：
    lang="ch",                   # 文档语言优化（如："ch", "en", "ja"）
    device="cuda:0",             # 推理设备："cpu", "cuda", "cuda:0", "npu", "mps"
    start_page=0,                # 起始页码（0为基准，适用于PDF）
    end_page=10,                 # 结束页码（0为基准，适用于PDF）
    formula=True,                # 启用公式解析
    table=True,                  # 启用表格解析
    backend="pipeline",          # 解析后端：pipeline|vlm-transformers|vlm-sglang-engine|vlm-sglang-client
    source="huggingface",        # 模型源："huggingface", "modelscope", "local"
    # vlm_url="http://127.0.0.1:3000" # 当backend=vlm-sglang-client时，需指定服务地址
    # RAGAnything标准参数
    display_stats=True,          # 显示内容统计信息
    split_by_character=None,     # 可选的文本分割字符
    doc_id=None                  # 可选的文档ID
)
```

> 注意：MinerU 2.0不再使用 `magic-pdf.json` 配置文件。所有设置现在通过命令行参数或函数参数传递。RAG-Anything现在支持多种文档解析器 - 你可以根据需要在MinerU和Docling之间选择。

#### 处理要求

不同内容类型需要特定的可选依赖：

+   Office文档 (.doc, .docx, .ppt, .pptx, .xls, .xlsx): 安装并配置 LibreOffice
+   扩展图像格式 (.bmp, .tiff, .gif, .webp): 使用 `pip install raganything[image]` 安装
+   文本文件 (.txt, .md): 使用 `pip install raganything[text]` 安装

> 📋 快速安装: 使用 `pip install raganything[all]` 启用所有格式支持（仅Python依赖 - LibreOffice仍需单独安装）

---

## 🧪 支持的内容类型

### 文档格式

+   PDF - 研究论文、报告、演示文稿
+   Office文档 - DOC、DOCX、PPT、PPTX、XLS、XLSX
+   图像 - JPG、PNG、BMP、TIFF、GIF、WebP
+   文本文件 - TXT、MD

### 多模态元素
