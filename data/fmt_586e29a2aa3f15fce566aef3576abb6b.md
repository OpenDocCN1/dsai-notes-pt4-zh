
```
traceback.print_exc()
```

# 创建 LangChain Agent 自动调用 MCP 工具

## 加载 MCP 工具

已加载 2 个 MCP 工具

- `parse_documents`: 统一接口，将文件转换为Markdown格式。支持本地文件和URL，会根据`USE_LOCAL_API`配置自动选择合适的处理...
- `get_ocr_languages`: 获取 OCR 支持的语言列表。

Returns:

`Dict[str, Any]`: 包含所有支持的OCR语言列表...

## 使用 parse_documents 工具解析 PDF

文件: `course.pdf`

大小: 1391.70 KB

调用 `parse_documents` 工具...

解析完成！

解码文件名: `course`

## 解析结果统计

总字符数: 12,693

总行数: 364

结果已保存到: `/home/MuyuWorkSpace/course_parsed.md`

当前工作目录: `/home/MuyuWorkSpace`

文件大小: 16.40 KB

## 解析结果预览（前 1500 字符）

### 五、项目：`PaddleOCR-VL`构建多模态RAG系统

本节内容，我们将详细介绍如何部署和运行这个基于`PaddleOCR-VL`的多模态AgenticRAG智能问答系统。该系统支持复杂PDF文档、图片、表格、公式等多种格式的智能分析和问答，并具备精准的溯源能力。

![](images/8b615bdc06de102e742913e121d91c5ec9905e922f853a2a988eebefa8ba496f.jpg)

### 5.1 项目核心模块代码详解

`PaddleOCR-VL`的输出格式非常适合构建多模态RAG系统，如果想要明确的区分出图像、表格、普通文本等信息，一个基本的处理流程是这样的：

```
PaddleOCR-VL JSON 输出↓
```

1. 数据预处理

```
按`block_order`排序过滤无用内容（footer等）合并相邻同类型块
```

2. 分类处理

```
文本类  $\rightarrow$  标准 chunk 表格类  $\rightarrow$  结构化提取  $+$  文本描述 公式类  $\rightarrow$  保留格式  $+$  语义转换 图片类  $\rightarrow$  多模态向量 / 标题关联
```

3. 元数据增强

```
- `block_id`（溯源ID）
- `block_bbox`（位置坐标）
- `block_type`（元素类型）
- `page_index`（页码）
- 上下文信息（前后标题、图表编号）
```

4. 向量化与索引

```
文本 Embedding  
表格 Embedding（多策略）建立多级索引
```

5. 检索与溯源

语义检索  

坐标定位  

可视化标注

分享 | 下载(195.7M) | 删除 : 更多

全部文件/木羽公开课资料/20251022基于`PaddleOCR-VL`搭建企...

![](images/409c2892525832b5465b80864b23240f91674807f62fbc6dfa25335a5dee2b19.jpg)

![](images/29ea9762cc653c55e580471be648e41292bf6730e1bf1696a59f07dcdafdfe26.jpg)

首先第一步做的就是完成`PaddleOCR-VL`模型的接入及实现解析过程。核心代码文件为:

![](images/01aea784451e2cca4b44b64bbb05f9d9998fe4232ea886bf087b8b4157736285.jpg)

这个服务的核心流程是：初始化时异步加载 `PaddleOCRVL` 模型到，然后在 `parse_document()` 中通过线程池执行阻塞的 OCR 调用，`PaddleOCR` 会将文档解析为多页结果并保存为 JSON/Markdown/可视化图片到磁盘。服务优先从生成的 `*_res.json` 文件中读取每页的 `parsing_res_list`，将其中的每个 block（包含 `block_id`、`block_label`、`block_content`、`block_bbox`、`block_order` 等字段）转换为 `ParsingBlock` 对象，最后通过 `calculate.stats()` 按 `label` 关键词（`table`/`image`/`formula`/其他）统计各类型块的数量并返回 `DocumentStats`。即：

1. 初始化  $\rightar

... (后续内容已省略)

## 成功！

同时也可以看到后端的服务运行情况：

![](img/1f5657fa7ef5a50924de6d2b3b5e0209.png)

**我们下期公开课，再见！** 👋
