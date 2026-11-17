
# PaddleOCR 3.0 技术报告与 MinerU 使用文档

## 资源链接

Source Code: `https://github.com/PaddlePaddle/PaddleOCR`  
Document: `https://paddlepaddle.github.io/PaddleOCR`  
Models & Online Demo: `https://huggingface.co/PaddlePaddle`

## 摘要

This technical

## 测试记录

### 测试 2：vlm-vllm-async-engine Backend (vLLM 加速)

完整 Markdown 已保存到: `output_pipeline.md`

### MinerU API 测试

API URL: `http://192.168.130.4:50000/file_parse`  
PDF 文件: `./2507.05595v1.pdf`  
Backend: `vlm-vllm-async-engine`

发送请求...

请求成功!

- 使用的 backend: `vlm-vllm-async-engine`
- 版本: `2.6.4`
- 结果数量: `1`

Markdown 内容预览 (前 500 字符):

### PaddleOCR 3.0 Technical Report

Cheng Cui, Ting Sun, Manhui Lin, Tingquan Gao, Yubo Zhang, Jiaxuan Liu, Xueqing Wang, Zelun Zhang, Changda Zhou, Hongen Liu, Yue Zhang, Wenyu Lv, Kui Huang, Yichao Zhang, Jing Zhang, Jun Zhang, Yi Liu, Dianhai Yu, Yanjun Ma

PaddlePaddle Team, Baidu Inc.

paddleocr@baidu.com

Source Code: `https://github.com/PaddlePaddle/PaddleOCR`  
Document: `https://paddlepaddle.github.io/PaddleOCR`  
Models & Online Demo: `https://huggingface.co/PaddlePaddle`

#### 摘要

This tec

完整 Markdown 已保存到: `output_vlm-vllm-async-engine.md`

## 测试总结

Pipeline backend: 成功  
vLLM backend: 成功  

所有测试通过! MinerU 可以通过 `backend` 参数切换不同模型

💡 提示:

- `pipeline`: 使用本地 `PyTorch`，适合调试
- `vlm-vllm-async-engine`: 使用 vLLM 加速，速度更快

![](img/b89742d8c6098dd56790b6577c48ce49.png)

当然，上述代码也是 LangChain1.0 + OCR 多模态 PDF 解析实战项目中 MinerU 的核心代码逻辑：

当然，MinerU 提供了命令行离线解析、Gradio/WebUI、独立 FastAPI 服务（`mineru-api`）等多种启动方式。不同方式适合的场景、端口、性能与依赖各不相同，如下所示：

## MinerU 启动方式

| 启动方式 | 典型命令/镜像 | 端口 & 路径 | 适用场景 | 依赖 |
| --- | --- | --- | --- | --- |
| SGLang 后端服务 | `mineru-sglang-server --host 0.0.0.0 --port 30000` | `POST /file_parse` | 高并发、GPU 推理、本地私有化 | Python + CUDA/SGLang |
| Docker 一键 | `docker run -p 30000:30000 opendatalab/mineru:latest mineru-sglang-server` | 同上 | 免环境配置、快速试用 | Docker (GPU 可选) |
| 命令行离线解析 | `mineru -p <输入> -o <输出>` | 无服务；直接落盘 | 批处理、无网络、脚本化 | 纯 Python CLI |
| Gradio WebUI | `mineru-gradio --enable-sglang-engine` | 浏览器 UI | 交互演示、无代码使用 | Gradio 前端 |
| FastAPI API 服务 | `git clone .../mineru-api && docker compose up` | `PUT /api/parse` | 需要依托自定义业务接口 | FastAPI + Uvicorn |
| MCP-Server（外壳） | `uv run mineru-mcp --transport streamable-http` | `/mcp` 或 `/sse` | 给 ADK/Agent 暴露工具 | `fastmcp` 框架 |
| n8n/Serverless 封装 | n8n「MCP Client」节点或 Runpod 镜像 | 由平台分配 | 无代码工作流 | 平台函数 |

实际上，MinerU 会输出非常丰富的信息，主要是：

1. `images`: 这是一个文件夹，里面存储了 PDF 文档中出现的所有图片；
1. `.origin.pdf`: 进行解析的原始 PDF 文件；
1. `.markdown`: PDF 文档的解析结果，输出为 `xxx.md` 的 Markdown 格式；
1. `.layout.pdf`: 这个文件用不同的背景色块圈定不同的内容块，以此来区分整体的布局识别结果；
1. `.model.json`: 这个文件的主要作用是将 PDF 从像素级别的展示转换为结构化的数据，使计算机能够"理解"文档的组成部分。主要存储的信息是：

   - 文档布局识别：存储文档中的各种元素（标题、正文、图表等）及其在页面上的精确位置
   - 内容分类：将识别出的元素分类为不同类型（如标题、表格、公式等）
   - 特殊内容解析：对于公式、表格等特殊内容，提供其结构化表示（如 `LaTeX`、`HTML` 格式）

其示例数据如下：

```json
[
    {
        "layout_dets": [
            {
                "category_id": 2,
                "poly": [
                    99.1906967163086,
                    100.3119125366211,
                    730.3707885742188,
                    100.3119125366211,
                    730.3707885742188,
                    245.81326293945312,
                    99.1906967163086,
                    245.81326293945312
                ],
                "score": 0.9999997615814209
            }
        ],
        "page_info": {
            "page_no": 0,
            "height": 2339,
            "width": 1654
        }
    },
    {
        "layout_dets": [
            {
                "category_id": 5,
                "poly": [
                    99.13092803955078,
                    2210.680419921875,
                    497.3183898925781,
                    2210.680419921875,
                    497.3183898925781,
                    2264.78076171875,
                    99.13092803955078,
                    2264.78076171875
                ],
                "score": 0.9999997019767761
            }
        ],
        "page_info": {
            "page_no": 1,
            "height": 2339,
            "width": 1654
        }
    }
]
```

其中每一个 `layout_dets` 中存储了 PDF 文档中每一页的布局识别结果，`poly` 是四边形坐标，分别是 左上，右上，右下，左下 四点的坐标，形式为：`[x0, y0, x1, y1, x2, y2, x3, y3]`，分别表示左上、右上、右下、左下四点的坐标，`page_info` 是 PDF 文档中每一页元数据，包含页码、高度、宽度信息，而 `category_id` 为 PDF 文档中每一页的布局识别结果的类型，分别为：

### category_id 类别

| 类别 ID | 类别名称 | 描述 |
| --- | --- | --- |
| `0` | `title` | 标题 |
| `1` | `plain_text` | 文本 |
| `2` | `abandon` | 包括页眉页脚页码和页面注释 |
| `3` | `figure` | 图片 |
| `4` | `figure_caption` | 图片描述 |
| `5` | `table` | 表格 |
| `6` | `table_caption` | 表格描述 |
| `7` | `table_footnote` | 表格注释 |
| `8` | `isolate_formula` | 行间公式 |
| `9` | `formula_caption` | 行间公式的标号 |
| `13` | `embedding` | 行内公式 |
| `14` | `isolated` | 行间公式 |
| `15` | `text` | OCR 识别结果 |

这个 `.json` 文件主要可以内容提取，比如精确提取特定类型的内容（如所有表格或公式），搜索，比如实现对文档内容的结构化搜索，文档分析，比如分析文档的结构特征（如有多少图表、公式密度等）等需求场景。

1. `_middle.json`: 这个文件的核心作用是存储 PDF 文档解析过程中多层次结构化数据，它比 `model.json` 提供了更详细的层次结构和内容信息，其示例数据是这样的：

```json
{
    "pdf_info": [
        {
            "preproc_blocks": [
                {
                    "type": "text",
                    "bbox": [
                        52,
                        61.956024169921875,
                        294,
                        82.99800872802734
                    ],
                    "lines": [
                        {
                            "bbox": [
                                52,
                                61.956024169921875,
                                294,
                                72.0000228881836
                            ],
                            "spans": [
                                {
                                    "bbox": [
                                        54.0,
                                        61.956024169921875,
                                        296.2261657714844,
                                        72.0000228881836
                                    ],
                                    "content": "dependent on the service headway and the reliability of the departure ",
                                    "type": "text",
                                    "score": 1.0
                                }
                            ]
                        }
                    ]
                }
            ],
            "layout_bboxes": [
                {
                    "layout_bbox": [
                        52,
                        61,
                        294,
                        731
                    ],
                    "layout_label": "V",
                    "sub_layout": []
                }
            ],
            "page_idx": 0
        }
    ]
}
```
