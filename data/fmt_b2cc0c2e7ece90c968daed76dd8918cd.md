
```python
USER_QUESTION = "请详细分析这张平面图，包括房间布局、尺寸面积、动线设计等信息。"
```

# 执行分析

```python
result = analyzer.analyze(FLOOR_PLAN_PATH, USER_QUESTION)
```

## 显示分析结果

```python
print(result.answer)
```

```python
print("\n【提取的结构化元数据】")
```

```python
print(json.dumps(result.extracted_info, ensure_ascii=False, indent=2))
```

✓图片加载成功: (900, 781)

图片转换为base64: 130.5 KB

正在调用VLM模型...

分析完成！Token使用: 2492

---

这张平面图展示了一套三居室的住宅布局，包含客厅、主卧、次卧、儿童房、厨房、主卫、次卫、餐厅和入户玄关等功能区。整体布局紧凑，动线合理，适合家庭居住。

【提取的结构化元数据】

```json
{
  "total_dimensions": {
    "length": 12.949,
    "width": 9.192,
    "unit": "m",
    "total_area": 119.0
  },
  "rooms": [
    {
      "name": "客厅",
      "position": "中央偏右",
      "dimensions": {
        "length": 5.198,
        "width": 3.188,
        "area": 16.57,
        "unit": "m"
      },
      "furniture": [
        "沙发",
        "茶几",
        "电视柜"
      ],
      "connected_to": [
        "餐厅",
        "入户"
      ],
      "windows": 1,
      "doors": 1
    },
    {
      "name": "主卧",
      "position": "左下",
      "dimensions": {
        "length": 4.113,
        "width": 3.617,
        "area": 14.88,
        "unit": "m"
      },
      "furniture": [
        "床",
        "衣柜"
      ],
      "connected_to": [
        "主卫"
      ],
      "windows": 1,
      "doors": 1
    },
    {
      "name": "次卧",
      "position": "右上",
      "dimensions": {
        "length": 3.409,
        "width": 3.198,
        "area": 10.89,
        "unit": "m"
      },
      "furniture": [
        "床",
        "衣柜"
      ],
      "connected_to": [
        "次卫"
      ],
      "windows": 1,
      "doors": 1
    },
    {
      "name": "儿童房",
      "position": "左上",
      "dimensions": {
        "length": 3.327,
        "width": 3.611,
        "area": 12.0,
        "unit": "m"
      },
      "furniture": [
        "床",
        "衣柜"
      ],
      "connected_to": [
        "厨房"
      ],
      "windows": 1,
      "doors": 1
    },
    {
      "name": "厨房",
      "position": "上方中央",
      "dimensions": {
        "length": 2.733,
        "width": 2.49,
        "area": 6.8,
        "unit": "m"
      },
      "furniture": [
        "橱柜",
        "灶台"
      ],
      "connected_to": [
        "儿童房",
        "次卫"
      ],
      "windows": 1,
      "doors": 1
    }
  ],
  "annotations": [
    {
      "type": "dimension",
      "value": "12949",
      "parsed_value": 12.949,
      "unit": "m",
      "description": "外墙总长"
    },
    {
      "type": "dimension",
      "value": "9192",
      "parsed_value": 9.192,
      "unit": "m",
      "description": "外墙总宽"
    }
  ],
  "symbols": [
    {
      "type": "door",
      "count": 6,
      "positions": [
        "客厅-餐厅",
        "主卧-主卫",
        "次卧-次卫",
        "儿童房-厨房"
      ]
    }
  ],
  "circulation": {
    "main_entrance": "右下",
    "main_path": "入户 → 客厅 → 餐厅 → 各房间",
    "layout_type": "分隔式"
  },
  "design_notes": [
    "主卧带独立卫生间",
    "动线流畅",
    "采光良好"
  ]
}
```

接下来，我们需要将这些分析结果存储到向量数据库中，以支持高效的检索和问答。

## 5.5 构建向量数据库存储系统

在多模态RAG系统中，向量数据库扮演着至关重要的角色。它不仅存储了图纸的文本描述，还保存了提取的结构化元数据，使得我们可以：

1. `语义检索`：根据用户问题的语义，而非关键词匹配，找到相关图纸

2. `元数据过滤`：基于结构化信息（如尺寸、材料等）进行精确筛选

3. `高效索引`：即使有成千上万张图纸，也能毫秒级返回结果

向量数据库的核心思想是将文本转换为高维向量（Embedding），相似的文本在向量空间中距离较近。当用户提问时，问题也被转换为向量，然后通过计算距离找到最相关的文档。

```
文本内容 → Embedding模型 → 向量表示 → 存储到ChromaDB

用户问题 → Embedding模型 → 问题向量 → 相似度检索 → 返回Top-K结果
```

接下来，我们就来实现这个向量存储系统。

```python
class VectorStoreManager:
    """向量数据库管理器 - 基于ChromaDB"""

    def __init__(self, persist_directory: str = "./chroma_db_floor_plan"):
        """初始化向量数据库"""

        self.persist_directory = persist_directory

        os.makedirs(persist_directory, exist_ok=True)

        # 初始化 Embedding 模型

        print("正在初始化 Qwen Embedding 模型...")

        self.embeddings = QwenEmbeddings(
            model="text-embedding-v4",
            api_key=os.getenv("DASHSCOPE_API_KEY")  # 从环境变量读取
        )

        # 初始化 ChromaDB

        self.vector_store = Chroma(
            persist_directory=persist_directory,
            embedding_function=self.embeddings,
            collection_name="floor_plans"
        )

        # 文本分割器

        self.text_splitter = RecursiveCharacterTextSplitter(
            chunk_size=500,
            chunk_overlap=100,
            separators=["\n\n", "\n", "。", ".", " ", ""]
        )

        print(f"向量数据库初始化完成")

    def add_document(
        self,
        file_id: str,
        file_name: str,
        content: str,
        extracted_info: Dict[str, Any]
    ) -> int:
        """添加户型文档到向量库"""

        print(f"\n添加文档到向量库: {file_name}")

        # 1. 分割文本

        chunks = self.text_splitter.split_text(content)

        print(f"  文本分割为 {len(chunks)} 个块")

        # 2. 创建Document对象

        documents = []

        for i, chunk in enumerate(chunks):

            metadata = {

                "file_id": file_id,

                "file_name": file_name,

                "chunk_id": i,

                "total_chunks": len(chunks),

                "extracted_info_json": json.dumps(extracted_info, ensure_ascii=False)

            }

            # 提取关键字段到元数据顶层（便于过滤和问答）

            if "total_dimensions" in extracted_info:

                dims = extracted_info["total_dimensions"]

                metadata["total_area"] = float(dims.get("total_area", 0))

                metadata["total_length"] = float(dims.get("length", 0))

                metadata["total_width"] = float(dims.get("width", 0))

            if "rooms" in extracted_info:

                rooms = extracted_info["rooms"]

                metadata["room_count"] = len(rooms)

                # 统计卧室数量

                bedrooms = [r for r in rooms if "卧" in r.get("name", "")]

                metadata["bedroom_count"] = len(bedrooms)

            if "circulation" in extracted_info:

                circ = extracted_info["circulation"]

                metadata["layout_type"] = circ.get("layout_type", "")

            documents.append(Document(

                page_content=chunk,

                metadata=metadata
```
