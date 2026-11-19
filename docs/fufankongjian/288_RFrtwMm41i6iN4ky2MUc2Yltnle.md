
# Part 2. 基于 LangChain1.1 从零搭建AI 文档审核系统

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/SAcsdFYD5ooPdHxsniocLOEsnye](https://kq4b3vgg5b.feishu.cn/docx/SAcsdFYD5ooPdHxsniocLOEsnye)

## 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》(12月班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(12月班)

![](img/8bdd046a4bcf58e206c2384b5adb9ff1.png)

![](img/356ed807f84985886dc0dec54ee948fe.png)

LangChain1.1 通用智能文档审核Agent项目代码🔗网盘链接: https://pan.baidu.com/s/1cX_BE-ZdLShnMEwoQWViKQ?pwd=7tde 提取码: 7tde ，有更多课程咨询需求可扫码添加助教老师哦～

借助大语言模型的能力，我们可以构建一个智能文档审核系统，自动识别文档中的语法错误、不当用语、敏感词汇以及一些定制化的审核规则，大幅提升文档审核效率。本小节课程，我们就从零开始，使用 LangChain1.1 框架和 DeepSeek-v3.2 大模型来进行文档审核Agent系统的开发。

具体来说，我们将实现以下完整功能的开发及运行：

核心功能

| 步骤 | 说明 | 产出 |
| --- | --- | --- |
| **输入** | 真实 PDF 文档 | PDF 文件 |
| **解析** | 使用 MinerU 解析 | 结构化文本 + 坐标 |
| **审核** | 用 LangChain 1.1 调用大模型，按"预设规则 + 自定义规则"审核 | 结构化问题列表 |
| **输出** | 将审核结果整理成"问题列表"（表格/JSON） | 可用于前端高亮与整改 |

具体应用到的核心技术栈如下：

### 核心技术栈

| 技术 | 作用 | 说明 |
| --- | --- | --- |
| `LangChain1.1` | LLM 应用开发框架 | 简化大模型调用，提供统一接口 |
| `DeepSeek-v3.2` | 大语言模型 | 国产高性能 LLM，性价比高 |
| `MinerU` | PDF 解析服务 | 将 PDF 转换为结构化文本 |
| `Pydantic` | 数据验证 | 定义结构化输出格式 |

## 1. 环境准备与依赖安装

### 1.1 安装必要的依赖库

首先，我们需要安装本课程所需的 Python 库。在当前的 Jupyter Notebook 中运行以下命令：

```
# 安装核心依赖
%pip install langchain langchain-openai pydantic httpx python-dotenv -q
```

```
[33mWARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager, possibly rendering your system unusable. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv. Use the --root-user-action option if you know what you are doing and want to suppress this warning.[0m[33m
[0mNote: you may need to restart the kernel to use updated packages.
```

这些库的作用分别如下：

+ `langchain`：LLM 应用开发核心框架
+ `langchain-openai`：LangChain 的 OpenAI 兼容接口（DeepSeek 使用 OpenAI 兼容协议）
+ `pydantic`：数据验证和结构化输出
+ `httpx`：异步 HTTP 客户端，用于调用 MinerU API
+ `python-dotenv`：环境变量管理

### 1.2 准备 API 密钥

在使用 DeepSeek 和 MinerU 之前，我们需要获取并配置 API 密钥。

1. DeepSeek API Key：访问 DeepSeek 开放平台，注册账号后获取 API Key；
1. `MinerU API Key`：访问 [MinerU 官网](https://mineru.net/)，需要先进行API申请，然后创建 Token 即可。

![](img/1c47fbb8d4a019f140fd8e9e4f593194.png)

当然，除了MinerU，我们还可以使用`PaddleOCR-VL`、`DeepSeek-OCR`等其他解析方案，以上三种解析方案均有本地部署版本，大家可以找到助教老师领取资料：

![](img/9158b091ba3b4521aa89c6d2857b97a6.png)

![](img/e33aba72d54d2b048871b8fb2c697015.png)

MinerU是一个非常典型的基于管道的解决方案 (Pipeline-based solution) ，并且是一个开源文档解析项目，一共四个核心组件，通过Pipeline的设计无缝衔接，实现比较高效、准确的文档解析。如下图所示：（下图来源官方论文：https://arxiv.org/pdf/2409.18839v1）

![](img/ce15257bf1645a91b0f5eb5f72dc915b.png)

MinerU的主要工作流程分为以下几个阶段：

1. **输入**：接收PDF 格式文本，可以是简单的纯文本，也可以是包含双列文本、公式、表格或图等多模态PDF文件;
1. 文档预处理（Document Preprocessing）：检查语言、页面大小、文件是否被扫描以及加密状态；
1. 内容解析（Content Parsing）：

   + 局分析：区分文本、表格和图像。
   + 公式检测和识别：识别公式类型（内联、显示或忽略）并将其转换为 LaTeX 格式。
   + 表格识别：以 HTML/LaTeX 格式输出表格。
   + OCR：对扫描的 PDF 执行文本识别。
1. 内容后处理（Content Post-processing）：修复文档解析后可能出现的问题。比如解决文本、图像、表格和公式块之间的重叠，并根据人类阅读模式重新排序内容，确保最终输出遵循自然的阅读顺序。
1. 格式转换（Format Conversion）：以 Markdown 或 JSON 格式生成输出。
1. **输出（Output）**：高质量、结构良好的解析文档。

目前在Github 上，MinerU 的Star 数为48.2K，Fork 数为4K，拥有非常良好的社区支持和活跃的贡献者，且一直处于active development状态。

![](img/3ddce0e6e2c9128d525a24fb9aa92015.png)

MinerU 提供了在线Demo 页面，我们可以直接线进行测试。试用地址：https://opendatalab.com/OpenSourceTools/Extractor/PDF/

![](img/2fbe51876e9fa7d8d2f319f358eeddac.png)

同时，MinerU 项目于2024年07月05日首次开源，底层主要是集成 `PDF-Extract-Kit` 开源项目做PDF的内容提取，`PDF-Extract-Kit`同样是一个开源项目：https://github.com/opendatalab/PDF-Extract-Kit

![](img/50a5a4512120c12d77cdc66a0f0e13a6.png)

`PDF-Extract-Kit`这个项目主要针对的是PDF文档的内容提取，通过集成众多SOTA模型对PDF文件实现高质量的内容提取，其中应用到的模型主要包括：

PDF-Extract-Kit 应用的模型类型

| 模型类型 | 模型名称 | GitHub 链接 | 模型下载链接 | 任务描述 |
| --- | --- | --- | --- | --- |
| 布局检测模型 | `LayoutLMv3` / `DocLayout-YOLO` | [GitHub](https://github.com/microsoft/unilm/tree/master/layoutlmv3) / [GitHub](https://github.com/opendatalab/DocLayout-YOLO) | [模型下载](https://huggingface.co/microsoft/layoutlmv3-base-chinese) / [模型下载](https://huggingface.co/juliozhao/DocLayout-YOLO-DocStructBench/tree/main) | 定位文档中不同元素位置：包含图像、表格、文本、标题、公式等 |
| 公式检测模型 | `YOLO` | [GitHub](https://github.com/ultralytics/ultralytics) | [模型下载](https://github.com/ultralytics/assets/releases/download/v8.3.0/yolo11n.pt) | 定位文档中公式位置：包含行内公式和行间公式 |
| 公式识别模型 | `UniMERNet` | [GitHub](https://github.com/opendatalab/UniMERNet) | [模型下载](https://huggingface.co/wanderkid/unimernet_base) | 识别公式图像为latex源码 |
| 表格识别模型 | `StructEqTable` | [GitHub](https://github.com/Alpha-Innovator/StructEqTable-Deploy?tab=readme-ov-file) | [模型下载](https://huggingface.co/U4R/StructTable-InternVL2-1B/tree/main) | 识别表格图像为对应源码（Latex/HTML/Markdown） |
| OCR模型 | `PaddleOCR` | [GitHub](https://github.com/PaddlePaddle/PaddleOCR) | [模型下载](https://paddlepaddle.github.io/PaddleX/latest/module_usage/tutorials/ocr_modules/text_detection.html) | 提取图像中的文本内容（包括定位和识别） |

MinerU与`PDF-Extract-Kit`的关系是：MinerU 结合`PDF-Extract-Kit`输出的高质量预测结果，进行了专门的工程优化，使得文档内容提取更加便捷高效，处理底层原理的优化细节外，主要提升点在以下几点：

+ 加入了自研的`doclayout_yolo(2501)`模型做布局检测，在相近解析效果情况下比原方案提速10倍以上，可以通过配置文件与 `layoutlmv3` 自由切换使用；
+ 加入了自研的`unimernet(2501)` 模型做公式识别，针对真实场景下多样性公式识别的算法，可以对复杂长公式、手写公式、含噪声的截图公式均有不错的识别效果；
+ 增加 OCR 的多语言支持，支持 84 种语言的检测与识别，支持列表：https://paddlepaddle.github.io/PaddleOCR/latest/ppocr/blog/multi_languages.html#5
+ 重构排序模块代码，使用 `layoutreader` 进行阅读顺序排序，确保在各种排版下都能实现极高准确率 : https://github.com/ppaanngggg/layoutreader
+ 表格识别功能接入了`StructTable-InternVL2-1B`模型，大幅提升表格识别效果，模型下载地址:https://huggingface.co/U4R/StructTable-InternVL2-1B

这里为了帮助大家降低接入的难度，我们就直接使用在线的MinerU API。在获取密钥后，可以通过以下方式配置：

```
# 方式一：直接在代码中配置（仅用于学习，生产环境不推荐）
# DEEPSEEK_API_KEY = "your-deepseek-api-key-here";
# MINERU_API_KEY = "your-mineru-api-key-here";

# 方式二：使用环境变量（推荐）
import os
from dotenv import load_dotenv

# 加载 .env 文件中的环境变量
load_dotenv(override=True)

DEEPSEEK_API_KEY = os.getenv("DEEPSEEK_API_KEY")
MINERU_API_KEY = os.getenv("MINERU_API_KEY")

# 验证配置
print(f"DeepSeek API Key 已配置: {DEEPSEEK_API_KEY[:10]}")
print(f"MinerU API Key 已配置: {MINERU_API_KEY[:10]}")
```

```
DeepSeek API Key 已配置: sk-04a9c72
MinerU API Key 已配置: eyJ0eXBlIj
```

如果可以正常显示DeepSeek API Key 和 MinerU API Key的前10位，说明环境变量加载成功。

## 2. 使用 MinerU 解析 PDF

![](img/5a0d524f6041ff79c1002055200e3775.png)

MinerU 在线API的工作流程如下：

```
    1. 请求上传 URL  ───▶  2. 上传 PDF  ───▶  3. 轮询解析状态  ───▶  4. 下载解析结果
```

大家可以在MinerU的官网看到详细的API接口文档：

![](img/fdb78a508f2bbdfd9dd7274767fe10fb.png)

下面我们来实现一个MinerU 客户端：



# MinerU PDF 文档解析实践

## 完整 API 交互流程

```python
import httpx
import asyncio
import time
import zipfile
import io
import json
from pathlib import Path

async def parse_pdf_with_mineru(pdf_path: str, api_key: str) -> list[dict]:
    """
    使用 MinerU API 解析 PDF 文档。
    
    参数:
        pdf_path: PDF 文件路径
        api_key: MinerU API 密钥
        
    返回:
        解析后的段落列表
    """
    base_url = "https://mineru.net"
    file_path = Path(pdf_path)
    file_name = file_path.name
    
    headers = {
        "Authorization": f"Bearer {api_key}",
        "Content-Type": "application/json",
    }
    
    async with httpx.AsyncClient(timeout=300) as client:
        # 步骤1：请求预签名上传 URL
        print("步骤1：请求上传 URL...")
        resp = await client.post(
            f"{base_url}/api/v4/file-urls/batch",
            headers=headers,
            json={
                "files": [{"name": file_name, "data_id": file_name}],
                "model_version": "vlm",  # 使用视觉语言模型
            }
        )
        resp.raise_for_status()
        data = resp.json()["data"]
        batch_id = data["batch_id"]
        upload_url = data["file_urls"][0]
        print(f"获取到 batch_id: {batch_id}")
        
        # 步骤2：上传 PDF 文件
        print("步骤2：上传 PDF 文件...")
        with open(pdf_path, "rb") as f:
            await client.put(upload_url, content=f.read())
        print("上传完成")
        
        # 步骤3：轮询等待解析完成
        print("步骤3：等待解析完成...")
        max_wait = 300  # 最多等待5分钟
        start_time = time.time()
        
        while True:
            resp = await client.get(
                f"{base_url}/api/v4/extract-results/batch/{batch_id}",
                headers={"Authorization": f"Bearer {api_key}"}
            )
            result = resp.json()["data"]["extract_result"][0]
            
            if result["state"] == "done":
                full_zip_url = result["full_zip_url"]
                print("解析完成")
                break
            elif result["state"] == "failed":
                raise RuntimeError(f"解析失败: {result.get('err_msg')}")
            
            if time.time() - start_time > max_wait:
                raise TimeoutError("解析超时")
            
            await asyncio.sleep(2)  # 每2秒检查一次
        
        # 步骤4：下载并解析结果
        print("步骤4：下载解析结果...")
        resp = await client.get(full_zip_url)
        zip_bytes = resp.content
        
        # 解压并提取 JSON
        with zipfile.ZipFile(io.BytesIO(zip_bytes)) as zf:
            json_files = [n for n in zf.namelist() if n.endswith(".json")]
            if not json_files:
                raise RuntimeError("未找到解析结果")
            
            with zf.open(json_files[0]) as f:
                content = json.loads(f.read().decode("utf-8"))
        
        print("下载完成")
        
        # 步骤5：提取段落文本
        paragraphs = extract_paragraphs(content)
        print(f"共提取了 {len(paragraphs)} 个段落")
        
        return paragraphs
```

上面的代码实现了与 MinerU API 的完整交互流程。接下来，我们需要一个辅助函数来从 MinerU 返回的 JSON 中提取段落文本：

## 段落文本提取辅助函数

```python
def extract_paragraphs(content) -> list[dict]:
    """
    从 MinerU 解析结果中提取段落文本。
    
    MinerU 返回的 JSON 结构可能有多种格式，这个函数会尝试兼容不同格式。
    """
    paragraphs = []
    
    # 情况1：content 是列表（每个元素是一个文本块）
    if isinstance(content, list):
        for item in content:
            if isinstance(item, dict):
                text = item.get("text") or item.get("content") or ""
                if text.strip():
                    paragraphs.append({
                        "content": text.strip(),
                        "page_num": item.get("page_idx", 0) + 1,
                        "bbox": item.get("bbox"),
                    })
        return paragraphs
    
    # 情况2：content 是字典，包含 pages 字段
    if isinstance(content, dict):
        pages = content.get("pages") or []
        for page in pages:
            page_num = page.get("page", 1)
            blocks = page.get("paragraphs") or page.get("blocks") or []
            for block in blocks:
                text = block.get("text") or block.get("content") or ""
                if text.strip():
                    paragraphs.append({
                        "content": text.strip(),
                        "page_num": page_num,
                        "bbox": block.get("bbox"),
                    })
    
    return paragraphs
```

## 测试示例数据与输出

```python
# 测试用的示例数据
sample_paragraphs = [
    {"content": "本公司承诺绝对保证产品质量，必须满足所有客户需求。", "page_num": 1},
    {"content": "根据市场调研，我们的产品销量将一定达到预期目标。", "page_num": 1},
    {"content": "公司简介：我们是一家专注于人工智能领域的科技公司。", "page_num": 2},
    {"content": "团队介绍：我们拥有一只经验丰富的研发团队。", "page_num": 2},  
]

print("示例段落数据：")
for i, p in enumerate(sample_paragraphs):
    print(f"  [{i}] 第{p['page_num']}页: {p['content'][:30]}...")
```

```
示例段落数据：
  [0] 第1页: 本公司承诺绝对保证产品质量，必须满足所有客户需求。...
  [1] 第1页: 根据市场调研，我们的产品销量将一定达到预期目标。...
  [2] 第2页: 公司简介：我们是一家专注于人工智能领域的科技公司。...
  [3] 第2页: 团队介绍：我们拥有一只经验丰富的研发团队。...
```

## 真实 PDF 解析测试

进行真实PDF解析测试：

```python
# 1. 本地 PDF 路径（改成你自己的）
pdf_path = "./data/LangChain v1.1 文档审核类Agent开发实战.pdf"

# 2. MinerU API Key（推荐走环境变量）
api_key = os.environ.get("MINERU_API_KEY")
if not api_key:
    raise RuntimeError("请先设置环境变量 MINERU_API_KEY")

# 3. 调用解析函数
paragraphs = await parse_pdf_with_mineru(pdf_path, api_key)

# 4. 打印结果
print("\n====== 解析结果 ======\n")
for i, p in enumerate(paragraphs):
    print(f"[{i}] 第{p['page_num']}页: {p['content'][:80]}")
```



# 文档审核类Agent开发实战

## 环境准备与文件上传

### 步骤1：请求上传 `URL`...
`batch_id: b714c6cb-c9dc-47c2-97eb-9b5a104404ce`

### 步骤2：上传 PDF 文件...
上传完成

### 步骤3：等待解析完成...
解析完成

### 步骤4：下载解析结果...
下载完成

共提取了 695 个段落

## 解析结果

[0] 第1页: LangChain v1.0 文档审核类Agent开发实战
[1] 第1页: 从技术选型到产品落地
[2] 第1页: 本期公开课，我们将深入探索基于大语言模型的文档审核Agent技术方案。
[3] 第1页: 文档审核类Agent系统功能速览
[4] 第1页: 核心功能一：支持在线上传财务类票据，自动实现数据精准提取与审核功能；
[5] 第2页: $\leftarrow$  返回
[6] 第2页: 审查结果 - 票据审查
[7] 第2页: 导出报告
[8] 第2页: 生成PDF
[9] 第2页: 1
[10] 第2页: 十审查项
[11] 第2页: 1 全项
[12] 第2页: 0
[13] 第2页: 警告项
[14] 第2页: 3
[15] 第2页: 需审查项
[16] 第2页: 筛选：
[17] 第2页: 代理名称：
[18] 第2页: 审造
[19] 第2页: 问题数量
[20] 第2页: 执行时间
[21] 第2页: 佳
[22] 第2页: 完整性校验Agent
[23] 第2页: 需要审查
[24] 第2页: $\langle \widehat{\mathbb{C}}\rangle$  发现
[25] 第2页: 0.00s
[26] 第2页: 字段[收款人]缺失
[27] 第2页: 格式校验Agent
[28] 第2页: 需要审查
[29] 第2页: $\langle \widehat{\mathbf{x}}\rangle$  发现
[30] 第2页: 0.00s
[31] 第2页: 发票代码格式正确：发票号码格式不正确：购买方纳税人识别号格式正确：销售方纳税人识别号格式正确...
[32] 第2页: 计算校验Agent
[33] 第2页: $100\%$  合格
[34] 第2页: 3个提示
[35] 第2页: 0.00s
[36] 第2页: 价税合计计算正确：5785.38*347.12=6132.50；行项目金额合计正确：￥
[37] 第2页: 业务规则校验Agent
[38] 第2页: 需要查询
[39] 第2页: $⑥$  发现
[40] 第2页: 0.00s
[41] 第2页: 增值税专用发票必须有收款人
[42] 第2页: (1)
[43] 第2页: 审查说明
[44] 第2页: 核心功能二：支持在线批量上传PDF法务合同文件，自动解析、分块，并能够自定义审核规则，自主完成内容审核及输出审核报告；
[45] 第2页: #
[46] 第2页: ① 历史记录
[47] 第2页: 图
[48] 第2页: 解除劳动合同通知书
[49] 第2页: 解除、终止劳动合同协议书
[50] 第2页: 1
[51] 第2页: 1

# 一、文档审核类AI成熟落地产品介绍

[52] 第3页: 首先看一组数据，如下论文中是对法律大模型与传统法律合同审查员、初级律师和法律流程外包商进行了开创性的比较，并深入剖析了大模型在合同审查的准确性、速度和成本效益方
[53] 第3页: Better Bill GPT: Comparing Large Language Models against Legal Invoice Reviewers
[54] 第3页: NICK WHITEHOUSE, NICOLE LINCOLN, STEPHANIE YIU, LIZZIE CATTerson, RIVINDU PERERA
[55] 第3页: Legal invoice review is a costly, inconsistent, and time-consuming process, trad
[56] 第3页: 论文地址：`https://arxiv.org/pdf/2401.16212`
[57] 第3页: 随着大模型能力的提升，将其作为智能代理用于专业文档的合规性审核已经从理论变为现实，并且在多个行业中展现出了惊人的效率提升。
[58] 第3页: 文档合规审核指的是根据法律法规、行业规范或企业内部规则，对各种专业文件进行内容和格式上的检查，以发现潜在的违规或缺陷之处。典型场景包括：合同文档的法律合规审核、
[59] 第3页: - 阿里的通义法睿：可以用于快速识别合同潜在风险，并提供专业的风险评估和修改建议。体验地址：`https://tongyi.aliyun.com/farui/re`
[60] 第3页: - 百度的财务、医疗、教育票据OCR识别及自动审查：`https://ai.baidu.com/tech/ocr/receipes/vat/invoice`
[61] 第4页: 全字段识别
[62] 第4页: 支持对账银发票、专票、发票、区块链发票、全科目发票全部关键字段的结构化识别，能够满足财税报销场景中对任意字段的识别需求
[63] 第4页: □□ 二维码识别校验
[64] 第4页: 识别画面左上角的二维码，获取发送代码、号码、金额、开票日期、检票码五个数字信息，与识别的对应字段信息进行匹配校验，保证更识别的正确判断。
[65] 第4页: 诸如此类的文档合规审核Agent是指基于大语言模型构建的智能代理系统，能够根据法律法规、行业规范或企业内部规则，自动对专业文件进行内容和格式上的检查，发现潜在的
[66] 第4页: 文档审核Agent核心应用场景
[67] 第4页: 之所以能够实现上述复杂工作流程的原因在于基于大模型构建而成的Agent具备自主决策和工具调用能力：
[68] 第4页: 1. 自主推理：不只是执行预设规则，而是能理解文档语义、推理条款间的逻辑关系
[69] 第5页: 功能介绍
[70] 第5页: 举个例子：审核一份采购合同时，Agent会先提取关键信息（供应商、金额、交付日期），然后检索公司采购政策，再核对预算系统中的额度，最后综合判断是否合规。这整个过
[71] 第5页: 论文地址：`https://arxiv.org/pdf/2501.09136`
[72] 第5页: 在合同、票据/收据、长篇公文等场景中，“大模型 + 工具调用 + 外部知识检索 + 规划/反思（Agentic模式）”能把抽取、核对与基于证据的判断串成多步流程
[73] 第5页: 同样，针对上图中的文档审核类的实现方案，如果进一步拆分则可以拆解为如下三大核心技术模块：
[74] 第5页: 文档解析模块
[75] 第5页: AI Agent 模块
[76] 第5页: 规则系统模块
[77] 第5页: 而如果再进一步拆解，则如下图所示：
[78] 第6页: 其中：
[79] 第6页: 所以一个相对比较健全的文档审核类Agent的实现，不仅需要我们掌握Agent及RAG的底层技术原理，还需要我们掌握多模态大模型、多Agent协作、Prompt工
[80] 第6页: 此外，对于文档的精准解析是文档审核类Agent落地的关键，也主要分为OCR和VLM两条实现链路，通过传统OCR +规则的方法逐步转到现在基于VLM-based方
[81] 第6页: 因此，本期公开课我们就从简到难，从基础的票据审核开始，逐步深入到更为复杂的合同审核，给大家全面讲解不同的解析和审核Agent构建思路，以及如何将这些方法应用到实

# 二、多模态大模型搭建票据审核Agent

[82] 第7页: 我们先从较为简单的需求场景开始，即票据类审核方向。财务部门需要审核各种票据和发票，确保其内容完整、真假有效，并符合财务规定（例如发票抬头、税号、金额计算正确，报
[83] 第7页: 一般来说，人工在处理票据的完整审核流程包括：
[84] 第7页: 如果我们做的合规性检查输入比较简单，且规范都是统一的，比如票据类，此类的审核规则都是比较规范的，我们只需要定制化的提取出某些数值，那么这类场景是非常适合直接使用
[85] 第7页: 通过结合多模态大模型和多Agent协作技术,实现发票审核的智能化:
[86] 第7页: 我们就可以非常迅速的确定出如下方案来实现票据类审核Agent的技术架构方案：
[87] 第8页: 接下来我们就使用`LangChain`框架来搭建这个完整的票据审核Agent系统。
[88] 第8页: - Step 1. 安装环境依赖
[89] 第8页: 使用`LangChain1.0`作为核心Agent开发框架，需要预先安装如下依赖包：
[90] 第8页: 安装必要的依赖包
[91] 第8页: - Step 2. 配置 `API Key`
[92] 第8页: 接下来我们需要准备一个多模态大模型，我们使用阿里云百炼的`Qwen3-vl-plus`模型。注册地址：`https://baiiianconsole.aliyun.c`
[93] 第8页: 设置`APIKey`（请替换为你的实际Key）
[94] 第8页: API Key 配置完成
[95] 第9页: - Step 3. 导入必要的库
[96] 第9页: 导入项目所需的核心库：
[97] 第9页: 导入完成
[98] 第9页: - Step 4. 发票信息提取模块
[99] 第9页: 阶段的核心任务是：从发票图像或OCR数据中提取结构化的JSON数据。使用`Pydantic v2`定义发票的数据模型。`Pydantic`提供了强大的数据验证功能，可用
[100] 第10页: 添加了row字段自动类型转换
[101] 第10页: 测试数据: *广告制作*广告费, 金额: ¥94,339.62, 税率: 6.0%
[102] 第10页: 行项目就是发票里逐条计费的“最小可核查单元”，包含名称、数量、单价、金额、税率/税额等。清晰的行项目能提高透明度、减少争议与拒付风险，这是发票开具与收款的基本要
[103] 第10页: 接下来再定义完整的发票模型，给多模态大模型一个“完整的发票模型（`Invoice`）”，是在抽取  $\rightarrow$  校验  $\rightarrow$
[104] 第11页: `Invoice` 模型定义完成
[105] 第11页: 包含23个字段
[106] 第11页: 自动验证日期格式
[107] 第11页: 支持导出为dict/json
[108] 第11页: - Step 5. 初始化多模态大模型
[109] 第11页: 使用`LangChain`接入多模态大模型。多模态模型可以同时处理图像和文本输入。
[110] 第12页: 多模态大模型初始化完成
[111] 第12页: - Step 6. 如何传入问题和图像
[112] 第12页: 多模态模型的输入包含两部分:文本提示词 + 图像数据。我们需要将图像编码为 `base64` 格式。
[113] 第12页: `extraction_prompt = '''你是专业的中国发票识别助手。请仔细识别图片中的增值税发票，提取所有信息并以JSON格式返回。'''`
[114] 第12页: **必须提取的字段：**
[115] 第12页: **重要规则：**
[116] 第13页: 提示词构建完成
[117] 第13页: 提示词构建完成，接下来我们读取图像并编码为 `base64`。
[118] 第13页: 图像编码完成，`base64` 长度：362616 字符
[119] 第13页: 图像编码完成，接下来我们构建多模态消息。
[120] 第14页: - Step 7. 如何调用模型并做结构化输出
[121] 第14页: 调用多模态模型后，我们需要从响应中提取`JSON`数据，并使用`Pydantic`模型进行验证。
[122] 第14页: - Step 8. 完整的提取流程
[123] 第14页: 最后，我们定义一个完整的提取流程，将上述步骤串联起来。
[124] 第15页: 1. 读取并编码图像
[125] 第15页: 2. 构建多模态消息
[126] 第15页: 3. 调用多模态大模型
[127] 第15页: 4. 提取 `JSON` 并验证
[128] 第15页: 完整提取流程定义完成
[129] 第15页: - Step 9. 运行测试
[130] 第15页: 以上代码构建完成后，接下来便可以实际进行运行测试了：
[131] 第15页: 如果你有发票图像，可以运行以下代码：
[132] 第15页: 正在提取发票信息...
[133] 第15页: 发票信息提取完成
[134] 第15页: 发票代码：3100153130
[135] 第15页: 发票号码：14641426
[136] 第15页: 价税合计: ¥100,000.00
[137] 第15页: 如果有发票图像，可以运行以下代码：
[138] 第16页: 正在提取发票信息...
[139] 第16页: 发票信息提取完成
[140] 第16页: 做到这里，其实我们就已经完成了一个“发票识别 Agent”，可以识别发票的结构化数据了。如下所示：
[141] 第16页: 但是，我们还需要对识别出来的发票数据进行校验，以确保数据的准确性。所以我们需要进一步设计一个“发票校验 Agent”。这个阶段是整个系统的核心创新点，我们会使用
[142] 第16页: 多Agent协作校验系统的优势
[143] 第17页: - Step 10：定义校验结果数据模型
[144] 第17页: 同样，我们定义校验结果数据模型，用于存储每个Agent的校验结果。
[145] 第18页: 校验结果数据模型定义完成
[146] 第18页: - Step 11: 完整性校验 Agent
[147] 第18页: 该 Agent 主要检查发票的必填字段是否完整。
[148] 第18页: 建议填写的字段
[149] 第18页: 检查必填字段
[150] 第18页: 检查是否为空
[151] 第19页: 完整性校验 Agent 定义完成
[152] 第19页: 检查13个必填字段
[153] 第19页: 检查6个建议字段
[154] 第19页: - Step 12: 格式校验 Agent
[155] 第19页: 进一步地，当第一阶段正确输出验证发票代码、号码、税号等字段后，我们还要校验其识别的格式是否正确。

```
def validate_format(invoice_data: dict) -> AgentValidationReport:
    """格式校验 Agent"""
    start_time = time.time()
    results = []
    1. 发票代码：10位数字
    invoice_code = invoice_data.get('invoice_code', '')
    if invoice_code and not re.match(r'^\d{10}$', str(invoice_code)):
```

[156] 第21页: 格式校验 Agent 定义完成
[157] 第21页: - Step 13: 计算校验 Agent
[158] 第21页: 接下来进一步验证发票金额、税额的计算是否正确。
[159] 第23页: - Step 14: 业务规则校验 Agent
[160] 第23页: 最后，我们可以通过定制化的一些业务规则来精细化校验，比如我们这里以验证税率、发票类型等业务逻辑为例进行说明。
[161] 第24页: ）
[162] 第24页: 业务规则校验 Agent 定义完成
[163] 第24页: - Step 15: Orchestrator: 编排所有 Agent
[164] 第24页: 最终，我们定义一个协调器负责调用所有 Agent 并汇总结果。
[165] 第25页: 2. 汇总结果
[166] 第25页: 3. 确定总体状态
[167] 第25页: 4. 生成报告
[168] 第25页: `Orchestrator` 编排器定义完成
[169] 第25页: - Step 16：多代理校验系统运行测试
[170] 第25页: 最后，我们使用真实的发票数据进行校验，并打印校验报告。
[171] 第26页: 将之前提取的 `invoice` 对象转换为字典
[172] 第26页: 执行完整校验
[173] 第26页: 开始发票校验...
[174] 第26页: 校验完成：PASS
[175] 第26页: `def print_validation_report.report:InvoiceValidationReport):`
[176] 第27页: 发票校验报告
[177] 第27页: 发票编号：4200162130_00998959
[178] 第27页: 校验时间：2025-11-28 11:06:10
[179] 第27页: 总体状态：PASS
[180] 第27页: 总结：所有校验通过，发票数据正常
[181] 第27页: 统计信息：
[182] 第28页: 【完整性校验Agent】（耗时：0.000秒）
[183] 第28页: 无问题
[184] 第28页: 【格式校验Agent】（耗时：0.001秒）
[185] 第28页: 无问题
[186] 第28页: 【计算校验Agent】（耗时：0.000秒）
[187] 第28页: 无问题
[188] 第28页: 【业务规则校验Agent】（耗时：0.000秒）
[189] 第28页: 无问题
[190] 第28页: 我们这里可以测试一下有问题的票据：
[191] 第28页: 正在提取发票信息...
[192] 第28页: 发票信息提取完成
[193] 第28页: 开始发票校验...
[194] 第29页: 校验完成：FAIL
[195] 第29页: 发票校验报告
[196] 第29页: 发票编号：1100154130_00772
[197] 第29页: 校验时间：2025-11-18 17:00:15
[198] 第29页: 总体状态：FAIL
[199] 第29页: 总结：发现4个错误，0个警告，需要修正后才能通过
[200] 第29页: 统计信息：
[201] 第29页: 【完整性校验Agent】（耗时：0.000秒）
[202] 第29页: [X [ERROR] 必填字段 '收款人' 缺失或为空
[203] 第29页: 字段：`payee`
[204] 第29页: 期望：非空值
[205] 第29页: 实际：空
[206] 第29页: 建议：请补充 收款人 信息
[207] 第29页: 【格式校验Agent】（耗时：0.000秒）
[208] 第29页: [X [ERROR] 发票号码格式错误]
[209] 第29页: 字段：`invoice_number`
[210] 第29页: 期望：8位数字
[211] 第29页: 实际：00772
[212] 第29页: 建议：发票号码必须是8位纯数字，如：14641426
[213] 第29页: [X [ERROR] 开票日期格式错误
[214] 第29页: 字段：`issue_date`
[215] 第29页: 期望：YYYY-MM-DD
[216] 第29页: 实际：2016年9月
[217] 第29页: 建议：日期格式应为：2016-06-02
[218] 第29页: 【计算校验Agent】（耗时：0.000秒）
[219] 第29页: [X [ERROR] 第1行税额计算错误]
[220] 第29页: 字段：`line_items[0].tax_amount`
[221] 第29页: 期望: ¥347.12
[222] 第29页: 实际: ¥61.00
[223] 第29页: 建议：税额应该  $=$  金额(5785.38）  $\times$  税率  $(6.0\%) = 347.12$
[224] 第30页: 【业务规则校验Agent】（耗时：0.000秒）
[225] 第30页: 无问题
[226] 第30页: 导出详细的校验报告
[227] 第30页: 校验报告已导出到 `validation_report.json`
[228] 第30页: 导出提取的发票数据
[229] 第30页: 发票数据已导出到 `invoiceextracted.json`

# 三、基于OCR搭建法务合同审核Agent

[230] 第30页: 接下里我们再看第二个场景。在特定类别的文档需要遵循严格的格式与内容要求，例如政府或企业的招投标书（RFP响应文档）、各类行政公文、行业报告等。这些文档通常有明确
[231] 第30页: 为什么合同、标书审核更适合OCR+RAG方案而非VLM方案？这一选择基于以下几个核心考量：
[232] 第30页: 文档长度与Token成本
[233] 第30页: 合同和标书通常是多页长文档，动辄数十页甚至上百页。VLM在处理此类文档时面临严重的token限制问题。以一份20页的劳动合同为例，如果将整个PDF作为图像输入V
[234] 第31页: - 精确坐标定位与可追溯性
[235] 第31页: 法务合同审核的核心需求之一是精确定位问题条款的位置，以便法务人员快速查阅和修改。OCR方案（如`MinerU`）在解析PDF时会返回每个文本块的精确坐标信息（`bbox`）
[236] 第31页: 智能切分与上下文控制
[237] 第31页: OCR方案可以在提取文本后进行智能切分（按标题层级、段落语义切分），每个片段控制在800 tokens以内，既保持了上下文完整性，又能并发处理以提升效率。更重要
[238] 第31页: - 表格与复杂格式的处理
[239] 第31页: 合同中常包含表格（如付款计划表、违约金计算表）和复杂格式（如条款编号、多级标题）。OCR方案可以将表格解析为结构化数据（JSON或Markdown表格），便于后
[240] 第31页: 票据的人工审核标准流程
[241] 第32页: 综上，对于合同、标书等长文档审核场景，OCR+RAG方案在成本、精度、可追溯性、灵活性等多个维度均优于VLM方案。VLM更适合短文档、强视觉依赖的场景（如票据识
[242] 第32页: 本课程将带你从零开始实现一个完整的文档审核系统。该系统的核心特点是保留PDF文档的坐标信息，实现可追溯、可定位的文档审核和修改。
[243] 第32页: PDF文档  $\rightarrow$  `MinerU`解析  $\rightarrow$  提取文本  $+$  坐标  $\rightarrow$  智能切
[244] 第32页: 如下图所示：
[245] 第32页: 本课程将带你从零开始实现一个完整的文档审核系统。该系统的核心特点是保留PDF文档的坐标信息，实现可追溯、可定位的文档审核和修改。
[246] 第32页: - Step 1: 环境准备
[247] 第32页: 首先安装必要的依赖包。
[248] 第32页: 安装核心依赖
[249] 第33页: - [33mWARNING: Running pip as the 'root' user can result in broken permissions a
[250] 第33页: - Step 2: 配置 `API Key` 和 `MinerU` 服务地址
[251] 第33页: 配置`APIKey`和`MinerU`服务地址`import os`
[252] 第33页: API 配置完成
[253] 第33页: 其中关于`MinerU`、`PaddleOCR-VL`和`DeepSeek-OCR`的详细介绍以及如何在本地通过`vLLM`框架启动解析服务，大家可以学习我的往期公开课视频：从
[254] 第34页: - Step 3: PDF 解析与坐标提取
[255] 第34页: 这是整个系统的核心基础。我们使用`MinerU`将PDF转换为带坐标的JSON格式。
[256] 第34页: `MinerU` 是一个 PDF 解析工具，它不仅提取文本，还保留每个文本块在 PDF 中的精确位置（坐标）。
[257] 第34页: *输入：PDF文件
[258] 第34页: 输出**:JSON文件，包含：
[259] 第34页: 其中坐标格式示例：
[260] 第35页: PDF 解析函数已定义
[261] 第35页: - Step 4: 加载带坐标的 JSON 文档
[262] 第36页: 解析 `MinerU` 生成的 JSON，提取文本和坐标信息。
[263] 第37页: 根据 $< $劳动合同法 $> $ 相关规定,公司依法解除此前您与公司订立的劳动合同(合同期限:
[264] 第37页: 年月日至年月日)。解除您的理由是：
[265] 第37页: （1）过失性解除
[266] 第37页: 口在试用期内证明不符合录用条件的；口严重违反公司依法制定的规章制度的；
[267] 第37页: 口严重失职，营私舞弊，给用人单位造成重大损害的；
[268] 第37页: 口以欺诈、胁迫的手段使用人单位违背真实意思签订劳动合同致使劳动合同无效的；
[269] 第37页: 口员工同时与其他用人单位建立劳动关系，对完成本单位的工作任务造成严重影响，或者经用人单位提出，拒不改正的；   口被依法追究刑事责任的；
[270] 第37页: （2）非过失性解除
[271] 第37页: - Step 5: 带坐标的文档切分
[272] 第37页: 将长文档切分成小片段，同时为每个片段分配对应的坐标信息。切分策略为：
[273] 第37页: 关键点: 坐标信息让我们能够在原PDF中精确定位每个审核问题的位置。
[274] 第38页: # `MinerU` 返回的原始数据：按照行、段落、表格单元格等自然单位切分的。
[275] 第38页: 重新切分（`chunk_size=800`）：
[276] 第38页: `chunk 1 (831 tokens)`:
[277] 第38页: 内容："解除劳动合同通知书\n\n先生/小姐\n\n根据<<劳动合同法>>..."
[278] 第38页: `bbox_list`: [
[279] 第38页: {`bbox`: [359, 95, 638, 120], `text`: "解除劳动合同通知书"},
[280] 第38页: {`bbox`: [199, 164, 373, 181], `text`: "先生/小姐"},
[281] 第38页: {`bbox`: [170, 200, 847, 255], `text`: "根据..."}
[282] 第38页: // ... 共86个原始文本块的坐标
[283] 第38页: ]
[284] 第38页: # 核心逻辑：一个`Chunk`  $\rightarrow$  多个`BBox`：每个原始文本块都有自己精确的坐标
[285] 第38页: 后续的审核逻辑：
[286] 第38页: `issue` = {
[287] 第38页: "original": "年月日至年月日", #有问题的原文
[288] 第38页: "description": "日期填写不完整"
[289] 第38页: }
[290] 第38页: 在`chunk`的`bbox_list`中查找匹配的坐标
[291] 第38页: `for bbox_item in chunk['bbox_list']:`
[292] 第38页: `if "年月日至年月日" in bounding_item['text']:`
[293] 第38页: `problem_bbox = _bbox_item['bbox']` #找到了！
[294] 第38页: `problem_page = bbox_item['pageidx']`
[295] 第38页: # 可以在PDF上精确高亮这个位置

```
import json
import re
from typing import List, Dict, Any
import re
import json
from typing import Dict, Any, List
def clean_ocrerrors(text: str) -> str:
    """清理常见的OCR识别错误"""
    if not text:
    return text
    修复书名号识别错误（各种变体）
    text = re.sub(r'$\$\s*&lt;\s*&lt;\s*\$\s*(.+?)\s*\$\&gt; \s*&gt;\$', r' \{1\}', text)
    text = re.sub(r'$\$\s*&lt;\s*&lt;\s*(.+?)\s*&gt;\s*&gt;\$', r' \{1\}', text)
    text = re.sub(r'$\$\s*&lt; &lt;\s*(.+?)\s*&gt; &gt;\s*\$\$', r' \{1\}', text)
    修复LaTex符号
    text = re.sub(r'$\$\17\s*\$\\s*(.+?)\s*\$\mathbf{\mathbf{m}}athrm\s*\{\s*\&gt;\s*\}\', text)
    text = re.sub(r'$\$\s*&lt;\s*&lt;\s*\$(.+?)\$\mathfrak{m}athrm\s*\{\{\s*\&gt;\s*\}\{\s*\&gt;\s*\'\
    清理多余空格
    text = re.sub(r'\s+', ' ', text).strip()
    return text
```

[296] 第39页: `def load_json_with_coordinates(json_path: str) -> Dict[str, Any]:`
[297] 第39页:

```



```
with openjson_path, 'r', encoding = 'utf-8') as f:
    data = json.load(f)
results = data['results']
doc_name = list(results.keys())[0]
doc_result = results[doc_name]
```

解析 `content_list`

```
content_list_str = doc_result.get('content_list', [''])
if isinstance(content_list_str, str):
    content_list = json.dumps(content_list_str)
else:
    content_list = content_list_str
```

清理 `md_content` 中的 OCR 错误

```
md_content = doc_result.get('md_content', '')
md_content = clean_ocr.errors(md_content)
```

清理 `content_list` 中的文本

```
for item in content_list:
    if 'text' in item and item['text']:
        item['text'] = clean OCR errors(item['text'])
```

```
return {
    'md_content': md_content,
    'content_list': content_list
}
```

```
def estimate_tokens(text: str) -> int:
    """估算文本的 token 数"""
    chinese chars = len(re.findall(r'[u4e00-\u9fff)', text))
    other chars = len(text) - chinese chars
    return chinese chars + int(other chars * 0.25)
```

```
def normalize_text(text: str) -> str:
    """规范化文本用于匹配"""
    移除多余空格和换行
    text = re.sub(r'\s+', ' ', text)
    移除 LaTeX 符号和特殊字符
    text = re.sub(r'[\$<>\]]', ' ', text)
    return text.strip().lower()
```

```
def split_doc_with_coordinates(
    md_content: str,
    content_list: List[Dict],
    chunk_size: int = 800
) -> List[Dict[str，Any]]:
    切分文档并精确分配坐标（基于文本首次出现位置）
```

1111
切分文档并精确分配坐标（基于文本首次出现位置）
1111
1. 切分文本

```
paragraphs = md_content.split('\n\n')
chunks = []
current_chunk = ""
```

文档已切分为1个片段
片段 1:
Token数：4765
坐标块数：198
内容预览：#解除劳动合同通知书先生/小姐根据《劳动合同法》相关规定，公司依法解除此前您与公司订立的劳动合同(合同期限：年月日至年月日)。解除您的理由是：（1）过
坐标示例：页码 = 0，bbox = [359，95，638，120]
文本示例：解除劳动合同通知书

【调试】查看 `content_list` 前 5 个元素：

元素1：
类型：text
页码：0
坐标：[359，95，638，120]

元素2：
类型：text
页码：0
坐标：[199，164，373，181]

元素3：
类型：text
页码：0
坐标：[170，200，847，255]

元素4：
类型：text
页码：0
坐标：[187，275，753，293]

元素5：
类型：text
页码：0
坐标：[202，312，346，330]

【调试】查看 `md_content` 前 500 字符：

解除劳动合同通知书 先生/小姐 根据 《劳动合同法》 相关规定，公司依法解除此前您与公司订立的劳动合同(合同期限：年月日至年月日)。解除您的理由是：（1）过失性

- Step 6: 设计审核规则
定义文档审核需要检查的规则。

`PROFESSIONAL_CONTRACT=AUDITRULES = ""`
合同协议书专业审核规则

## 一、文本规范性审核（P1-P2级）
1.1 错别字与形近字检查
1.2 标点符号规范性
1.3 语法结构检查

## 二、合同专业性审核（PO级 - 核心）
2.1 法律术语规范性
- 法律风险：术语使用不当可能影响合同效力
2.2 权利义务对等性
2.3 金额与数字准确性
2.4 时间条款明确性

## 三、逻辑一致性审核（PO级）
3.1 条款前后一致性
3.2 条款间逻辑矛盾
3.3 引用条款准确性

## 四、合规性与风险审核（PO级）
4.1 法律合规性
4.2 敏感词汇检查
4.3 必备条款完整性

## 五、表述清晰度审核（P1级）
5.1 歧义性表述
5.2 冗余与重复

## 审核优先级说明
**PO级（1标记）**：法律合规性-可能导致合同无效
**P0级（1、标记）**: 专业性、一致性、风险 - 可能引发争议或损失
**P1级**：规范性、可读性 - 建议修正
**P2级**：优化项 - 可优化

111
```
print("审核规则已定义")
```

审核规则已定义

- Step 7. 构建审核 Agent (带坐标追溯)
关键点：审核结果需要关联坐标信息，实现问题的精确定位。

```
from pydantic import BaseModel, Field, ConfigDict, model_validator
```

数据结构定义完成

- Step 8. 构建审核 Agent 的提示词

```
PROFESSIONAL_SYSTEM_prompt = '''你是资深的合同法律审核专家，具有10年以上的合同审查经验。你的任务是对合同协议书进行专业、全面、
```

【审核标准】
严格按照提供的《合同协议书专业审核规则》进行审查，重点关注：

【审核原则】
【审核流程】
【输出规范】
1. `issues列表`:
2. `modifications列表`:
3. `corrected_text`:
4. `summary`:
5. `overall_risk_level`:

【特别注意】

```
PROFESSIONAL_USER_prompt = '''请对以下合同协议书片段进行专业审核：
```

【待审核文本】
【审核要求】
【特别说明】
请输入审核结果（JSON格式）。
【输出格式】
以 JSON 格式返回 `AuditResult` 对象，确保：

请开始审核。

- Step 9. 基于 `langChain` 构建审核 Agent
审核 Agent 构建完成

- Step 10. 执行带坐标的审核
审核单个片段，并关联坐标信息。

正在调用专业审核系统...
审核成功！

【审核结果概览】
是否发现问题：True
整体风险等级：high
问题总数：9
修改总数：6
审核总结：本次审核发现该劳动合同相关文书模板存在多项高风险法律问题，主要包括：1）法律术语使用不当，混淆不同性质的解除类型；2）主体信息缺失，未填写员工姓名；3

【问题详情】
高风险问题 (6 个):
1. [法律术语规范性] 法律术语使用不当
描述：多处使用 `解除劳动合同通知书` 作为单方通知文书，但根据《劳动合同法》，用人单位单方解除劳动合同必须说明具体法定事由并符合程序要求。该文本将协商解除、过失性
原文：解除劳动合同通知书
建议：应根据解除类型分别使用：`协商解除劳动合同协议书`（双方合意）、`解除劳动合同通知书（依据《劳动合同法》第XX条）`（单方解除），并在正文中明确引用具体法

2. [权利义务对等性] 主体信息缺失
描述：多份文书（如解除通知、终止通知、确认书等）中未填写员工姓名，仅以 `先生/小姐` 代替，违反《劳动合同法》关于书面通知应载明劳动者基本信息的要求，影响文书法律
原文：先生/小姐
建议：应替换为具体员工姓名，或至少在正式使用时填写完整姓名。

3. [金额与数字准确性] 经济补偿金表述不规范
描述：经济补偿金计算未明确是否包含代通知金、是否按 N+1 标准，且未说明计算基数（如月工资指解除前 12 个月平均工资）。例如 `相当于 月工资的经济补偿` 表述模糊，易
原文：口公司需要支付给您相当于月工资的经济补偿，计元。
建议：应明确为 `相当于【X】个月工资的经济补偿金（按解除劳动合同前 12 个月平均工资计算），共计人民币【大写】元（￥【小写】`。

4. [必备条款完整性] 关键日期空白
描述：所有文书中的关键日期（如合同起止日、解除日、薪资结算日等）均为空白，不符合《劳动合同法》第五十条关于出具解除或终止证明应载明劳动合同期限、解除或终止日期等
原文：年月日
建议：在正式使用时必须填写完整日期，模板中可标注为 `【年】年【月】月【日】日` 以提示填写。

5. [法律合规性] 续订意向书期限设置不当
描述：`续订劳动合同意向书` 中规定 `逾期 7 天视为不同意续签`，但根据《劳动合同法》第十四条，若劳动者符合签订无固定期限劳动合同条件，用人单位不得以逾期未回复为由
原文：请您于收到此《意向书》后 7 天之内填妥回执意见并归还人力资源部，逾期将视为本人不同意与公司续签劳动合同。
建议：修改为 `请您于收到此《意向书》后 7 天之内填妥回执意见并归还人力资源部。若您符合签订无固定期限劳动合同的法定条件，公司将依法与您签订无固定期限劳动合同。`

6. [表述清晰度] 歧义性表述
描述：`二、乙方薪资结算至年月日；计元方支付方经济补偿金元；` 存在严重语法错误和歧义，无法判断支付主体和金额。
原文：二、乙方薪资结算至 年 月 日；计 元方支付 方经济补偿金 元；
建议：应拆分为：（1）乙方薪资结算至【年】年【月】月【日】日，共计人民币【大写】元（￥【小写】）；（2）甲方支付乙方经济补偿金人民币【大写】元（￥【小写】）。

中风险问题（1个）：
1. [逻辑一致性] 文书重复冗余
描述：同一份文档中重复出现完全相同的文书模板（如两份 `协商解除劳动合同协议书`、两份 `劳动合同到期终止通知书` 等），属于明显的格式错误，可能导致实际使用混乱。
原文：协商解除劳动合同协议书（全文重复）
建议：删除重复的文书模板，保留一份即可。

低风险问题（2个）：
1. [文本规范性] 标点符号不规范
描述：多处顿号、逗号使用不规范，如 `口不予支付经济补偿金；口公司需要支付...` 中分号应为句号或换行；法律条文引用中括号使用不一致。
原文：口不予支付经济补偿金；口公司需要支付给您相当于月工资的经济补偿，计元。
建议：每个选项应独立成行或以句号结束，避免使用分号连接不同选项。

2. [OCR识别问题] 特殊符号错误
描述：《劳动合同法》书名号显示正常，但部分空格异常，如 `《劳动合同法》`，属 OCR 识别错误。
原文：《劳动合同法》
建议：修正为 `《劳动合同法》`

【修改记录】（6 处）
修改1：
原文：解除劳动合同通知书
修改：
原因：区分不同解除性质，确保法律定性准确
规则：法律术语规范性
修改2：
原文：先生/小姐
修改：
原因：补充主体信息，符合法律规定
规则：权利义务对等性
修改3：
原文：口公司需要支付给您相当于月工资的经济补偿，计元。
修改：
原因：明确经济补偿金计算标准和金额
规则：金额与数字准确性




# 劳动合同相关文书模板

## 修订记录

### 修改 4

- 原文：二、乙方薪资结算至年月日；计元方支付方经济补偿金元；
- 修改：
- 原因：消除歧义，明确支付内容
- 规则：表述清晰度

### 修改 5

- 原文：请您于收到此《意向书》后7天之内填妥回执意见并归还人力资源部，逾期将视为本人不同意与公司续签劳动合同。
- 修改：
- 原因：避免违反《劳动合同法》关于无固定期限合同的强制性规定
- 规则：法律合规性

### 修改 6

- 原文：《劳动合同法》
- 修改：
- 原因：修正OCR识别错误
- 规则：OCR识别问题

## 【修正后的文本】

## 【根据解除类型选择】协商解除劳动合同协议书 / 解除劳动合同通知书（依据《劳动合同法》第XX条）

【员工姓名】

根据《劳动合同法》相关规定，公司依法解除此前您与公司订立的劳动合同(合同期限：【年】年【月】月【日】日至【年】年【月】月【日】日)。解除您的理由是：

（1）过失性解除

（2）非过失性解除

（3）经济性裁员

您的劳动合同于【年】年【月】月【日】日解除。您需要结算以下薪资和补偿金事项：

您需要按照公司的离职管理制度规定办理离职手续。

通知单位（盖章）

通知时间：【年】年【月】月【日】日

说明：本通知一式二份，送达劳动者一份，用人单位留存一份，涂改无效。

送达记录：受送达人签字：

签收时间：

## 协商解除劳动合同协议书

甲方（盖章）： 乙方（签字）：

法定代表人或委托代理人（签字盖章）：

签约日期：【年】年【月】月【日】日

签约日期：【年】年【月】月【日】日

## 劳动合同到期终止通知书

【员工姓名】

公司与员工【员工姓名】订立的劳动合同（【年】年【月】月【日】日至【年】年【月】月【日】日），即将届满。根据国家和地方相关法律、法规、政策以及劳动合同相关约定，经

您需要结算以下薪资和补偿金事项：

（1）您薪资结算至【年】年【月】月【日】日；计人民币【大写】元（￥【小写】）；

(2) 此种情况下: 公司需要支付给您相当于【×】个月工资的经济补偿金（按终止劳动合同前12个月平均工资计算），共计人民币【大写】元 (¥【小写】)。

您需要按照公司的离职管理制度规定办理离职手续。

通知单位（盖章）

通知时间：【年】年【月】月【日】日

说明：本通知一式二份，送达劳动者一份，用人单位留存一份，涂改无效。

送达记录：受送达人签字：

签收时间：

## 劳动合同解除确认书

【员工姓名】

根据《劳动合同法》相关规定，你于【年】年【月】月【日】日向公司递交了辞职申请，提前解除此前公司与您订立的劳动合同(合同期限：【年】年【月】月【日】日至【年】年【

您需要结算以下薪资和补偿金事项：

（1）您薪资结算至【年】年【月】月【日】日；计人民币【大写】元（￥【小写】）；

（2）此种情况下：

您需要按照公司的离职管理制度规定办理离职手续。

单位（盖章）

【年】年【月】月【日】日

说明：本确认书一式二份，送达劳动者一份，用人单位留存一份，涂改无效。

确认记录：员工签字：

签收确认时间：

## 续订劳动合同意向书（用人单位使用）

【员工姓名】

本公司与您订立的劳动合同（合同期限：【年】年【月】月【日】日至【年】年【月】月【日】日）即将期满，根据国家法律规定，经公司管理层批准，公司同意与您不降低该到期劳

特此通知

员工签字确认收到通知：

单位（签章）

【年】年【月】月【日】日

【年】年【月】月【日】日

## 续订劳动合同意向书回执意见

公司【公司名称】：

本人已于【年】年【月】月【日】日收到了【公司名称】签发的与本人不降低原劳动合同约定条件续订劳动合同的意向书。

本人意见：

签名：

【年】年【月】月【日】日

---

## 劳动合同终止通知书（适用于其他终止情形）

【员工姓名】

公司与员工【员工姓名】订立的劳动合同（【年】年【月】月【日】日至【年】年【月】月【日】日），符合《劳动合同法》第44条及《劳动合同法实施条例》下列情形之一：

故您的合同将至【年】年【月】月【日】日终止。

您需要结算以下薪资和补偿金事项：

（1）您薪资结算至【年】年【月】月【日】日；计人民币【大写】元（￥【小写】）；

（2）此种情况下：

您需要按照公司的离职管理制度规定办理离职手续。

单位（盖章）

【年】年【月】月【日】日

说明：本确认书一式二份，送达劳动者一份，用人单位留存一份，涂改无效。

确认记录：员工签字：

签收确认时间：

---

## 订立劳动合同通知书（劳动者）

【员工姓名】：

本单位决定与您订立劳动合同。请您在收到本通知后于【年】年【月】月【日】日前到【部门名称】部门，按照《劳动合同法》有关规定，协商订立劳动合同。逾期不签订，本单位将

单位：（盖章）

【年】年【月】月【日】日

## 终止劳动关系通知书

【员工姓名】：

因你没有按照《订立劳动合同通知书》的时间要求，与本单位协商订立劳动合同，本单位决定与你终止劳动关系。请你在收到本通知后于【年】年【月】月【日】日前到【部门名称】

（劳动者）签收：

单位：（盖章）

【年】年【月】月【日】日

【年】年【月】月【日】日

---

## 解除或终止劳动合同（关系）证明书

【员工姓名】：

你与我单位订立了（固定期限、无固定期限、以完成一定工作任务）的劳动合同，合同期内从事【岗位名称】工作。根据《劳动合同法》等有关法律法规的规定，现按下列第【X】条

解除或（终止）劳动合同日期：【年】年【月】月【日】日

甲方单位（章）：

劳动者（签名）：

送达时间：【年】年【月】月【日】日

签收时间：【年】年【月】月【日】日

注：《证明书》一式二联，附《劳动合同法》相关法律条款。

---

## 变更劳动合同协议书

甲方：AA公司

乙方：【员工姓名】（员工身份证号：【身份证号】）

甲、乙双方经平等协商，一致同意在【年】年【月】月【日】日签订/续订的劳动合同第【×】条第【×】款作以下变更：

【具体变更内容】

劳动合同的其他条款仍按原约定继续履行。

（本协议书一式二份，甲乙双方各留存一份，涂改无效，具有同等法律效力。）

甲方：（盖章）

乙方：（签名或盖章）

法定代表人或委托代理人（签字或盖章）

测试通过！数据结构完全兼容！

# 四、本地部署启动文档审核系统

## 一、系统架构与设计

### 1.1 整体架构

DocumentAgent 是一个基于 `FastAPI` + `React` 的智能文档审核系统，支持票据审查和合同审查两大核心功能。

核心目录结构如下：

```
DocumentAgent/
├ backend/ #FastAPI后端
—frontend/ #React前端
src/
services/api.ts # API 服务层
components/ # UI 组件
Sidebar.tsx
DocumentUpload.tsx
- OCRResults.tsx
ReviewResults.tsx
App.tsx #主应用
package.json
vite.config.ts
DEPLOYMENT GUIDE.md #部署指南
（项目根目录）
- invoiceverification.py #OCR识别核心代码
- invoice_validationagents.py #审查核心代码（881行）
contract_infoexoction.py #合同信息提取
```

对应的技术栈如下：

### DocumentAgent 技术栈

项目的前后端源码已经上传到百度网盘中，大家可以扫码免费下载：

`全部文件/木羽公开课资料/20251128_12. LangChain v1.0 文档审...`

`contract_audit_prompt_professional.py` # 合同审查提示词

下载后，解压即可得到完整的项目源码，然后按照以下步骤进行部署：

### 后端服务部署与启动

成功启动后，在浏览器中访问 `http://localhost:8000/docs` 即可看到项目提供的API接口文档。

#### 文档审核系统 API 10.0 OAS 31

`/openapi.json`

支持票据和合同的OCR识别与智能审查

后端启动后，接下来部署前端应用。保持后端服务运行，打开一个新的终端窗口。进入前端目录并安装项目依赖：

这条命令会安装所有前端依赖包，包括 `react`、`vite`、`tailwindcss` 等。首次安装可能需要几分钟。

启动开发服务器，执行如下命令：

启动成功后，你会看到类似以下的输出：

此时，前端应用已经在 `http://localhost:3000` 成功运行！打开浏览器，访问 `http://localhost:3000`，你应该能看到主页，显示

如果以上步骤都顺利完成，恭喜你！项目已经成功在本地部署运行了！

我们下期公开课，再见！

![](img/33217037d02ed996ee68f4d0f8989318.png)

相信大家看到这里已经理解了 PDF 解析的基本流程。接下来，让我们进入最核心的部分——使用 LangChain 调用大模型进行文档审核。

# 3. LangChain 1.1 快速入门及应用

LangChain 是一个用于开发大语言模型（LLM）应用的框架。
它的核心价值在于：

1. **统一接口**：无论使用 `OpenAI`、`DeepSeek`、`Claude` 还是其他模型，调用方式都一样

1. **链式调用**：可以将多个处理步骤串联起来，形成一个完整的处理流程

1. **丰富的工具**：提供了 `Prompt` 模板、输出解析器、记忆组件等实用工具

在本课程中，我们主要使用 LangChain 的以下功能：

+ `ChatOpenAI`：调用兼容 OpenAI 接口的大模型（如 `DeepSeek-v3.2`）

+ **消息类型**：`SystemMessage`（系统提示）、`HumanMessage`（用户输入）

+ `PydanticOutputParser`：将 LLM 输出解析为结构化的 Python 对象

### 3.1 初始化 DeepSeek-v3.2 模型

我们首先初始化一个 `DeepSeek-v3.2` 模型实例。`DeepSeek` 使用与 `OpenAI` 兼容的 API 接口，所以我们可以直接使用 `ChatOpenAI` 类来调用它。

```
from langchain_openai import ChatOpenAI

def init_llm(api_key: str) -> ChatOpenAI:

    """

    初始化 DeepSeek 大语言模型。

    参数:

        api_key: DeepSeek API 密钥

    返回:

        ChatOpenAI 实例

    """

    llm = ChatOpenAI(

        model="deepseek-chat",           # DeepSeek 模型名称

        api_key=api_key,                  # API 密钥

        base_url="https://api.deepseek.com/v1",  # DeepSeek API 地址

        temperature=0.2,                  # 温度参数，越低输出越稳定

        max_tokens=4096,                  # 最大输出长度

    )

    return llm

# 初始化模型

llm = init_llm(DEEPSEEK_API_KEY)

print("大模型初始化完成")
```

```
大模型初始化完成
```

简单测试一下模型是否正常工作：

```
from langchain_core.messages import HumanMessage

# 发送一条简单的测试消息

response = llm.invoke([HumanMessage(content="你好，请用一句话介绍你自己。")])

print(f"模型回复：{response.content}")
```

```
模型回复：你好，我是DeepSeek，一个由深度求索公司创造的AI助手，致力于用热情和细心为你提供帮助！😊
```

> 参数说明：`temperature` 参数控制模型输出的随机性。在文档审核场景中，我们希望输出稳定一致，所以设置为较低的值（0.2）。

### 3.2 LangChain消息类型详解

在 LangChain 中，与大模型的对话由不同类型的「消息」组成。理解这些消息类型对于构建有效的 Prompt 非常重要：

LangChain 消息类型

| 消息类型 | 作用 | 使用场景 |
| --- | --- | --- |
| `SystemMessage` | 系统提示 | 定义 AI 的角色、行为规范 |
| `HumanMessage` | 用户消息 | 用户的输入内容 |
| `AIMessage` | AI 回复 | 模型的输出（通常由模型生成） |

下面是一个使用不同消息类型的示例：

```
from langchain_core.messages import SystemMessage, HumanMessage, AIMessage

# 构建一个对话

messages = [

    # 系统消息：定义 AI 的角色

    SystemMessage(content="你是一位专业的文档审核专家，擅长发现文档中的问题。"),

    # 用户消息：提供要审核的内容

    HumanMessage(content="请检查这句话是否有问题：'我们的产品必须满足所有客户需求。'"),

]

# 调用模型

response = llm.invoke(messages)
```



print(f"审核结果：\n{response.content}")

审核结果：

这句话在语法和表达上没有问题，但作为一份严谨的文档（如产品需求、项目章程或商业计划）中的陈述，**从专业性和精确性的角度来看，它存在几个潜在问题**，建议进行优化。

## 主要问题分析：

1. **绝对化与不切实际**：

   *   “所有” 这个词是绝对化的，在商业实践中几乎不可能实现。客户需求可能是无限的、矛盾的、不清晰的，甚至是不可行的。承诺“所有”会带来不切实际的期望和法律风险。

2. **范围模糊**：

   *   “客户需求” 定义不明确。是指“声明的需求”（客户说出来的）、“真实的需求”（客户实际需要但可能未表达的），还是“未来的潜在需求”？这个术语过于宽泛，缺乏可衡量性。

3. **缺乏优先级和可行性考量**：

   *   它没有体现资源（时间、成本、技术）的约束。在实际项目中，需求需要经过评估、排序和取舍。这句话暗示了一种被动的、无条件的响应模式，而非主动的产品管理。

## 修改建议：

根据文档的具体语境和严谨程度，可以选择以下一种修改方式：

*   **追求严谨与可衡量性（推荐）**：

    > “我们的产品将致力于满足目标客户的核心与优先级需求，并在发布后通过迭代持续响应用户反馈。”

    > *   **优点**：使用了“致力于”、“核心与优先级”、“迭代”等专业词汇，体现了产品管理的科学性和灵活性。

*   **强调承诺但留有余地**：

    > “我们的产品旨在最大限度地满足客户的关键需求。”

    > *   **优点**：用“旨在”、“最大限度地”、“关键”等词软化语气，既表达了决心，又避免了绝对化的承诺。

*   **在特定语境下的直接表述**：

    > “产品的设计目标是全面响应经过确认的客户需求。”

    > *   **优点**：加入了“经过确认的”作为限定，将范围缩小到已达成共识、经过验证的需求，更具可操作性。

## 总结：

原句作为一句口号或鼓舞士气的内部话语是合格的，但放入需要精确、可执行、无歧义的正式文档中则显得不够专业和严谨。建议根据文档性质，采用上述更精准的表述。

**审核结论：语义通顺，但作为正式文档语句，在专业精确性上存在重大优化空间。**

![](img/6e6aa04e427f0bdc6cc9f7cfdae3921b.png)

可以看到，通过设置 `SystemMessage`，我们成功让模型扮演了「文档审核专家」的角色。这就是提示词工程（Prompt Engineering）的核心思想。

## 4. 文档审核中的结构化输出

在上面的示例中，大模型返回的是自然语言文本。虽然人类可以理解，但对于程序来说，这种非结构化的输出很难处理。

想象一下，如果我们要构建一个完整的文档审核系统，我们需要：

1. 将每个问题单独显示在界面上；

1. 按问题类型进行分类统计；

1. 将结果存储到数据库中；

这些都需要结构化的数据，而不是一大段文本。所以，我们需要用到 LangChain 提供的 `PydanticOutputParser`，可以让模型输出结构化的 JSON 数据，并自动解析为 Python 对象。

### 4.1 定义输出数据结构

首先，我们需要使用 `Pydantic` 定义输出的数据结构。`Pydantic` 是一个强大的数据验证库，它可以帮助我们定义数据模型并自动进行类型检查。对于文档审核，我们需要定义两个模型：

1. `ReviewIssue`：单个问题的数据结构；

1. `ReviewOutput`：包含问题列表的输出结构；

```
from pydantic import BaseModel, Field

from typing import List, Literal

# 定义问题类型（使用 Literal 限制可选值）

IssueType = Literal["语法错误", "用词不当", "逻辑问题", "敏感表述"]

class ReviewIssue(BaseModel):

    """
    单个审核问题的数据结构。

    Field 装饰器用于添加字段描述，这些描述会被包含在给模型的提示中，

    帮助模型理解每个字段的含义。

    """

    type: str = Field(description="问题类型，如：语法错误、用词不当、逻辑问题、敏感表述")

    text: str = Field(description="问题所在的原文片段")

    explanation: str = Field(description="问题的详细说明")

    suggested_fix: str = Field(description="修改建议")

    para_index: int = Field(description="问题所在段落的索引（从0开始）")

class ReviewOutput(BaseModel):

    """

    审核输出的数据结构，包含问题列表。

    """

    issues: List[ReviewIssue] = Field(description="发现的问题列表")

# 展示数据结构

print("ReviewIssue 字段：")

for name, field in ReviewIssue.model_fields.items():

    print(f"  - {name}: {field.description}")
```

```
ReviewIssue 字段：

  - type: 问题类型，如：语法错误、用词不当、逻辑问题、敏感表述

  - text: 问题所在的原文片段

  - explanation: 问题的详细说明

  - suggested_fix: 修改建议

  - para_index: 问题所在段落的索引（从0开始）
```

`Field(description=...)` 的作用非常重要：它不仅是代码文档，更会被包含在发送给模型的提示中，帮助模型理解我们期望的输出格式。

### 4.2 创建输出解析器

接下来，我们创建一个 `PydanticOutputParser`，它会：

1. 自动生成格式说明（告诉模型应该输出什么格式）；

1. 将模型的输出解析为 Python 对象；

```
from langchain_core.output_parsers import PydanticOutputParser

# 创建解析器

parser = PydanticOutputParser(pydantic_object=ReviewOutput)

# 查看生成的格式说明

format_instructions = parser.get_format_instructions()

print("格式说明（会发送给模型）：")

print("-" * 50)

print(format_instructions[:1000] + "...")
```

```
格式说明（会发送给模型）：

--------------------------------------------------

The output should be formatted as a JSON instance that conforms to the JSON schema below.

As an example, for the schema {"properties": {"foo": {"title": "Foo", "description": "a list of strings", "type": "array", "items": {"type": "string"}}}, "required": ["foo"]}
the object {"foo": ["bar", "baz"]} is a well-formatted instance of the schema. The object {"properties": {"foo": ["bar", "baz"]}} is not well-formatted.

Here is the output schema:
```

可以看到，解析器自动生成了详细的 JSON Schema，告诉模型应该输出什么格式的数据。这就是 LangChain 结构化输出的魔法！

## 5. 构建文档审核 Pipeline

### 5.1 设计系统提示词

一个好的系统提示词（System Prompt）是构建有效 Agent 应用的关键。在文档审核场景中，我们需要告诉大模型：

1. 它的角色：专业的文档审核专家；

1. 任务目标：识别文档中的问题；

1. 问题类型：应该关注哪些类型的问题；

1. 输出格式：如何输出结果；

下面是我们精心设计的系统提示词：

```
SYSTEM_PROMPT = """你是一位专业的文档审核专家。
请仔细审查提供的文本，识别其中的问题。

需要检查的问题类型：
- 语法错误：错别字、标点符号错误、语病等
- 用词不当：使用了不恰当的词语或表达
- 敏感表述：使用了"必须"、"保证"、"一定"、"绝对"等过度承诺的措辞

注意事项：
1. 文档可能是中文或英文，请根据语言选择合适的审核标准
2. 使用输入中提供的段落索引（如 [0], [1], ...）来标识问题位置
3. 每个问题都需要提供具体的修改建议
4. 如果没有发现问题，返回空的问题列表
5. 按照要求的 JSON 格式输出结果
"""

print("系统提示词：")
print(SYSTEM_PROMPT)
```

```
系统提示词：
你是一位专业的文档审核专家。
请仔细审查提供的文本，识别其中的问题。

需要检查的问题类型：
- 语法错误：错别字、标点符号错误、语病等
- 用词不当：使用了不恰当的词语或表达
- 敏感表述：使用了"必须"、"保证"、"一定"、"绝对"等过度承诺的措辞

注意事项：
1. 文档可能是中文或英文，请根据语言选择合适的审核标准
2. 使用输入中提供的段落索引（如 [0], [1], ...）来标识问题位置
3. 每个问题都需要提供具体的修改建议
4. 如果没有发现问题，返回空的问题列表
5. 按照要求的 JSON 格式输出结果
```

### 5.2 构建用户提示词模板

用户提示词需要包含两部分：

1. 待审核的文本内容：每个段落带上索引标记；

1. 输出格式说明：告诉模型如何格式化输出；

```
def build_user_prompt(paragraphs: list[dict], parser: PydanticOutputParser) -> str:
    """
    构建用户提示词。
    
    参数:
        paragraphs: 段落列表
        parser: 输出解析器
        
    返回:
        格式化的用户提示词
    """
    # 将段落格式化为带索引的文本
    formatted_text = "\n".join([
        f"[{i}] {p['content']}" 
        for i, p in enumerate(paragraphs)
    ])
    
    # 构建完整的用户提示词
    user_prompt = f"""请审核以下文本内容：

{formatted_text}

如果发现问题，请按以下格式输出；如果没有问题，返回空的 issues 列表。

{parser.get_format_instructions()}
"""
    return user_prompt

# 测试
user_prompt = build_user_prompt(sample_paragraphs, parser)
print("用户提示词示例：")
print("-" * 50)
print(user_prompt[:800] + "...")
```

```
用户提示词示例：
--------------------------------------------------
请审核以下文本内容：

[0] 本公司承诺绝对保证产品质量，必须满足所有客户需求。
[1] 根据市场调研，我们的产品销量将一定达到预期目标。
[2] 公司简介：我们是一家专注于人工智能领域的科技公司。
[3] 团队介绍：我们拥有一只经验丰富的研发团队。

如果发现问题，请按以下格式输出；如果没有问题，返回空的 issues 列表。

The output should be formatted as a JSON instance that conforms to the JSON schema below.

As an example, for the schema {"properties": {"foo": {"title": "Foo", "description": "a list of strings", "type": "array", "items": {"type": "string"}}}, "required": ["foo"]}
the object {"foo": ["bar", "baz"]} is a well-formatted instance of the schema. The object {"properties": {"foo": ["bar", "baz"]}} is not well-formatted.

Here is the output schema:
```



```json
{"$defs": {"ReviewIssue": {"description": "单个审核问题的数据结构。\n\nField 装饰器用于添加字段描述，这些描述会被包含在给模型的提示中，\n帮助模型理解每个字段的含义。", "properties": {"type": {"description": "问题类型，如：语法错误、用词不当、逻辑问题、敏感表述",...
```

### 5.3 实现核心审核函数

现在，让我们把所有组件整合在一起，实现核心的文档审核函数。这是整个系统最关键的部分！

```python
from langchain_core.messages import SystemMessage, HumanMessage

def review_document(paragraphs: list[dict], llm: ChatOpenAI) -> ReviewOutput:

    """

    使用 LLM 审核文档内容。

    参数:

        paragraphs: 段落列表

        llm: LLM 模型实例

    返回:

        ReviewOutput 对象，包含发现的问题列表

    """

    # 创建输出解析器

    parser = PydanticOutputParser(pydantic_object=ReviewOutput)

    # 构建消息

    messages = [

        SystemMessage(content=SYSTEM_PROMPT),

        HumanMessage(content=build_user_prompt(paragraphs, parser)),

    ]

    # 调用 LLM

    print("正在调用 LLM 进行审核...")

    response = llm.invoke(messages)

    # 解析输出

    try:

        result = parser.parse(response.content)

        print(f"审核完成，发现 {len(result.issues)} 个问题")

        return result

    except Exception as e:

        print(f"解析输出失败: {e}")

        print(f"原始输出: {response.content[:500]}...")

        # 返回空结果

        return ReviewOutput(issues=[])
```

这个函数的核心流程非常清晰：

1. 创建解析器：用于将模型输出解析为结构化对象
2. 构建消息：包括系统提示词和用户提示词
3. 调用模型：发送消息并获取响应
4. 解析结果：将文本响应转换为 Python 对象

让我们来测试一下！

```python
# 使用示例数据测试

result = review_document(sample_paragraphs, llm)

# 展示结果

print("\n" + "=" * 60)

print("审核结果：")

print("=" * 60)

if result.issues:

    for i, issue in enumerate(result.issues, 1):

        print(f"\n问题 {i}：")

        print(f"   类型：{issue.type}")

        print(f"   位置：段落 [{issue.para_index}]")

        print(f"   原文：{issue.text}")

        print(f"   说明：{issue.explanation}")

        print(f"   建议：{issue.suggested_fix}")

else:

    print("未发现问题")
```

```text
正在调用 LLM 进行审核...

审核完成，发现 3 个问题

============================================================

审核结果：

============================================================

问题 1：

   类型：敏感表述

   位置：段落 [0]

   原文：绝对保证产品质量，必须满足所有客户需求

   说明：使用了'绝对保证'和'必须'等过度承诺的措辞，可能违反广告法或造成法律风险。

   建议：建议修改为'我们致力于保证产品质量，努力满足客户需求'或'我们承诺提供高质量的产品，以满足客户需求'。

问题 2：

   类型：敏感表述

   位置：段落 [1]

   原文：将一定达到预期目标

   说明：使用了'一定'这个表示绝对化的词语，属于过度承诺，可能不符合实际情况或引发争议。

   建议：建议修改为'有望达到预期目标'、'预计将达到预期目标'或'力争达到预期目标'。

问题 3：

   类型：用词不当

   位置：段落 [3]

   原文：一只经验丰富的研发团队

   说明：量词使用不当。'只'通常用于动物或某些成对物品中的一个，用于'团队'不恰当。

   建议：应将'一只'修改为'一支'。
```

![](img/e401b167094212f88f9fcfddefdeafe3.png)

接下来，我们学习如何支持自定义审核规则，让系统更加灵活。

## 6. 自定义审核规则

不同行业、不同场景对文档的审核要求是不同的，比如：

+ 金融行业：需要检查是否有违规承诺收益的表述；
+ 医疗行业：需要检查是否有夸大疗效的宣传；
+ 法律文件：需要检查条款是否有歧义；
+ 营销文案：需要检查是否有违反广告法的表述；

固定的审核规则无法满足所有需求，因此我们需要支持用户自定义审核规则。

实现自定义规则的核心思想是：动态构建 Prompt。我们将用户定义的规则动态注入到系统提示词中，让模型按照新的规则进行审核。

首先，我们需要定义规则的数据结构：

```python
from typing import Optional

from enum import Enum

class RiskLevel(str, Enum):

    """风险等级"""

    HIGH = "高"

    MEDIUM = "中"

    LOW = "低"

class RuleExample(BaseModel):

    """规则示例"""

    text: str = Field(description="示例文本")

    explanation: str = Field(description="说明")

class ReviewRule(BaseModel):

    """自定义审核规则"""

    name: str = Field(description="规则名称")

    description: str = Field(description="规则描述，说明什么情况下触发此规则")

    risk_level: RiskLevel = Field(description="风险等级")

    examples: list[RuleExample] = Field(default=[], description="示例列表")

# 创建一些示例规则

sample_rules = [

    ReviewRule(

        name="夸大宣传",

        description="检查是否有夸大产品效果或功能的表述，如'最好'、'第一'、'独家'等",

        risk_level=RiskLevel.HIGH,

        examples=[

            RuleExample(text="我们的产品是市场上最好的", explanation="使用了绝对化用语'最好'"),

            RuleExample(text="独家技术，行业第一", explanation="使用了'独家'、'第一'等夸大词汇"),

        ]

    ),

    ReviewRule(

        name="数据引用",

        description="检查引用的数据是否标注了来源",

        risk_level=RiskLevel.MEDIUM,

        examples=[

            RuleExample(text="据统计，90%的用户表示满意", explanation="未标注统计数据的来源"),

        ]

    ),

]

print("示例规则：")

for rule in sample_rules:

    print(f"\n {rule.name} [{rule.risk_level.value}风险]")

    print(f"     描述：{rule.description}")
```

```text
示例规则：

 夸大宣传 [高风险]

     描述：检查是否有夸大产品效果或功能的表述，如'最好'、'第一'、'独家'等

 数据引用 [中风险]

     描述：检查引用的数据是否标注了来源
```

接下来，我们需要修改提示词构建函数，支持动态注入自定义规则：

```python
def build_system_prompt(custom_rules: list[ReviewRule] = None) -> str:

    """

    构建系统提示词，支持自定义规则。

    参数:

        custom_rules: 自定义规则列表

    返回:

        系统提示词

    """

    # 基础规则类型

    issue_types = [

        "- 语法错误：错别字、标点符号错误、语病等",

        "- 用词不当：使用了不恰当的词语或表达",

        "- 敏感表述：使用了'必须'、'保证'、'一定'等过度承诺的措辞",

    ]

    # 动态添加自定义规则

    if custom_rules:

        for rule in custom_rules:

            rule_desc = f"- {rule.name}：{rule.description}"

            # 添加示例（如果有的话）

            if rule.examples:

                examples_str = "；".join([f"'{ex.text}'" for ex in rule.examples[:2]])

                rule_desc += f"（示例：{examples_str}）"

            issue_types.append(rule_desc)

    # 构建完整的系统提示词

    prompt = f"""你是一位专业的文档审核专家。

请仔细审查提供的文本，识别其中的问题。

需要检查的问题类型：

{chr(10).join(issue_types)}

注意事项：

1. 文档可能是中文或英文，请根据语言选择合适的审核标准

2. 使用输入中提供的段落索引（如 [0], [1], ...）来标识问题位置

3. 每个问题都需要提供具体的修改建议

4. 如果没有发现问题，返回空的问题列表

5. 按照要求的 JSON 格式输出结果

"""

    return prompt

# 测试：查看添加自定义规则后的提示词

custom_prompt = build_system_prompt(sample_rules)

print("含自定义规则的系统提示词：")

print("-" * 50)

print(custom_prompt)
```

```text
含自定义规则的系统提示词：

--------------------------------------------------

你是一位专业的文档审核专家。

请仔细审查提供的文本，识别其中的问题。

需要检查的问题类型：

- 语法错误：错别字、标点符号错误、语病等

- 用词不当：使用了不恰当的词语或表达

- 敏感表述：使用了'必须'、'保证'、'一定'等过度承诺的措辞

- 夸大宣传：检查是否有夸大产品效果或功能的表述，如'最好'、'第一'、'独家'等（示例："我们的产品是市场上最好的"；"独家技术，行业第一"）

- 数据引用：检查引用的数据是否标注了来源（示例："据统计，90%的用户表示满意"）

注意事项：

1. 文档可能是中文或英文，请根据语言选择合适的审核标准

2. 使用输入中提供的段落索引（如 [0], [1], ...）来标识问题位置

3. 每个问题都需要提供具体的修改建议

4. 如果没有发现问题，返回空的问题列表

5. 按照要求的 JSON 格式输出结果
```

可以看到，自定义规则已经被成功注入到系统提示词中。现在让我们更新审核函数，支持自定义规则：

```python
def review_document_with_rules(

    paragraphs: list[dict], 

    llm: ChatOpenAI,

    custom_rules: list[ReviewRule] = None

) -> ReviewOutput:

    """

    使用 LLM 审核文档内容，支持自定义规则。

    参数:

        paragraphs: 段落列表

        llm: LLM 模型实例

        custom_rules: 自定义规则列表（可选）

    返回:

        ReviewOutput 对象

    """

    # 创建输出解析器

    parser = PydanticOutputParser(pydantic_object=ReviewOutput)

    # 构建消息（使用动态生成的系统提示词）

    messages = [

        SystemMessage(content=build_system_prompt(custom_rules)),

        HumanMessage(content=build_user_prompt(paragraphs, parser)),

    ]

    # 调用 LLM

    print("正在调用 LLM 进行审核...")

    if custom_rules:

        print(f"   使用自定义规则：{[r.name for r in custom_rules]}")

    response = llm.invoke(messages)

    # 解析输出

    try:

        result = parser.parse(response.content)

        print(f"审核完成，发现 {len(result.issues)} 个问题")

        return result

    except Exception as e:

        print(f"解析输出失败: {e}")

        return ReviewOutput(issues=[])
```




让我们用一段包含「夸大宣传」和「数据引用」问题的文本来测试：

### 准备测试数据

```python
test_paragraphs = [
    {
        "content": "我们的产品是市场上最先进、最可靠的解决方案。",
        "page_num": 1
    },
    {
        "content": "根据调查显示，95%的用户对我们的服务表示满意。",
        "page_num": 1
    },
    {
        "content": "公司成立于2020年，致力于人工智能技术的研发与应用。",
        "page_num": 2
    }
]
```

### 使用自定义规则进行审核

```python
result = review_document_with_rules(test_paragraphs, llm, custom_rules=sample_rules)
```

### 展示结果

```python
print("\n" + "=" * 60)
print("审核结果（使用自定义规则）：")
print("=" * 60)

if result.issues:
    for i, issue in enumerate(result.issues, 1):
        print(f"\n问题 {i}：")
        print(f"   类型：{issue.type}")
        print(f"   位置：段落 [{issue.para_index}]")
        print(f"   原文：{issue.text}")
        print(f"   说明：{issue.explanation}")
        print(f"   建议：{issue.suggested_fix}")
else:
    print("未发现问题")
```

正在调用 LLM 进行审核...

   使用自定义规则：['夸大宣传', '数据引用']

审核完成，发现 2 个问题

============================================================

审核结果（使用自定义规则）：

============================================================

问题 1：

   类型：夸大宣传

   位置：段落 [0]

   原文：我们的产品是市场上最先进、最可靠的解决方案。

   说明：使用了'最先进'、'最可靠'等绝对化、最高级的表述，属于夸大宣传，可能违反广告法相关规定。

   建议：建议修改为更客观的表述，例如：'我们的产品是市场上先进且可靠的解决方案之一。' 或 '我们的产品致力于提供先进可靠的解决方案。'

问题 2：

   类型：数据引用

   位置：段落 [1]

   原文：根据调查显示，95%的用户对我们的服务表示满意。

   说明：引用了具体数据（95%），但未说明数据来源（例如：由哪个机构、在何时、基于多少样本进行的调查）。缺乏来源的数据引用可信度低，且可能构成误导。

   建议：建议补充数据来源，例如：'根据我们于2023年进行的用户满意度调查（样本量：1000），95%的用户对我们的服务表示满意。' 或者使用更模糊的表述，如：'绝大多数用户对我们的服务表示满意。'

## 7. 分块处理大文档

大语言模型有上下文长度限制（Context Window）。如果文档内容超过了模型的上下文限制，我们需要将文档分成多个小块，分别进行审核，最后合并结果。

分块策略有很多种，最简单的是按段落数量分块。

```python
def chunk_paragraphs(paragraphs: list[dict], chunk_size: int = 20) -> list[list[dict]]:

    """

    将段落列表分成多个小块。

    参数:

        paragraphs: 段落列表

        chunk_size: 每块的段落数量

    返回:

        分块后的段落列表

    """

    chunks = []

    for i in range(0, len(paragraphs), chunk_size):

        chunk = paragraphs[i:i + chunk_size]

        chunks.append(chunk)

    return chunks
```

### 演示分块效果

```python
sample_data = [{"content": f"段落 {i}", "page_num": 1} for i in range(50)]

chunks = chunk_paragraphs(sample_data, chunk_size=15)

print(f"共 {len(sample_data)} 个段落，分成 {len(chunks)} 块")

for i, chunk in enumerate(chunks):

    print(f"   第 {i+1} 块：{len(chunk)} 个段落")
```

共 50 个段落，分成 4 块

   第 1 块：15 个段落

   第 2 块：15 个段落

   第 3 块：15 个段落

   第 4 块：5 个段落

对于大文档，我们可以采用流式处理的方式：逐块审核，实时返回结果。这样用户可以尽早看到部分审核结果，而不需要等待整个文档审核完成。

```python
from typing import Generator

def stream_review_document(

    paragraphs: list[dict],

    llm: ChatOpenAI,

    custom_rules: list[ReviewRule] = None,

    chunk_size: int = 20

) -> Generator[ReviewOutput, None, None]:

    """

    流式审核文档，逐块处理并yield结果。

    参数:

        paragraphs: 段落列表

        llm: LLM 模型实例

        custom_rules: 自定义规则列表

        chunk_size: 每块的段落数量

    Yields:

        每块的审核结果

    """

    # 分块

    chunks = chunk_paragraphs(paragraphs, chunk_size)

    total_chunks = len(chunks)

    print(f"文档共 {len(paragraphs)} 个段落，分成 {total_chunks} 块处理")

    print("-" * 50)

    # 创建解析器

    parser = PydanticOutputParser(pydantic_object=ReviewOutput)

    system_prompt = build_system_prompt(custom_rules)

    # 逐块处理

    for i, chunk in enumerate(chunks):

        print(f"\n正在处理第 {i+1}/{total_chunks} 块...")

        # 构建消息

        messages = [

            SystemMessage(content=system_prompt),

            HumanMessage(content=build_user_prompt(chunk, parser)),

        ]

        # 调用 LLM

        response = llm.invoke(messages)

        # 解析结果

        try:

            result = parser.parse(response.content)

            print(f"   发现 {len(result.issues)} 个问题")

            yield result

        except Exception as e:

            print(f"   解析失败: {e}")

            yield ReviewOutput(issues=[])

    print("\n" + "-" * 50)

    print("全部处理完成！")
```

让我们用一个较长的测试文档来演示流式审核的效果：

### 准备一个较长的测试文档

```python
long_document = [

    {"content": "本公司是行业内最具实力的企业之一。", "page_num": 1},

    {"content": "我们承诺必须在30天内完成项目交付。", "page_num": 1},

    {"content": "根据内部调查，客户满意度达到98%。", "page_num": 1},

    {"content": "公司拥有一支优秀的技术团队。", "page_num": 2},  

    {"content": "我们的解决方案能够完全满足所有需求。", "page_num": 2},

    {"content": "产品采用了世界领先的技术架构。", "page_num": 2},

]
```

### 流式审核（设置较小的`chunk_size`以演示效果）

```python
all_issues = []

for result in stream_review_document(long_document, llm, chunk_size=3):

    all_issues.extend(result.issues)

# 汇总结果

print("\n" + "=" * 60)

print(f"汇总：共发现 {len(all_issues)} 个问题")

print("=" * 60)

for i, issue in enumerate(all_issues, 1):

    print(f"\n{i}. [{issue.type}] {issue.text[:30]}...")

    print(f"   建议：{issue.suggested_fix}")
```

文档共 6 个段落，分成 2 块处理

--------------------------------------------------

正在处理第 1/2 块...

   发现 1 个问题

正在处理第 2/2 块...

   发现 2 个问题

--------------------------------------------------

全部处理完成！

============================================================

汇总：共发现 3 个问题

============================================================

1. [敏感表述] 我们承诺必须在30天内完成项目交付。...

   建议：建议修改为'我们承诺将在30天内完成项目交付。'或'我们致力于在30天内完成项目交付。'，以软化语气并保留灵活性。

2. [敏感表述] 我们的解决方案能够完全满足所有需求。...

   建议：建议修改为'我们的解决方案旨在满足您的核心需求'或'我们的解决方案能够有效满足大部分需求'。

3. [用词不当] 产品采用了世界领先的技术架构。...

   建议：建议修改为'产品采用了先进的技术架构'或'产品采用了行业主流/成熟的技术架构'，使其表述更客观、严谨。

## 8. 完整运行示例

最后，我们将所有组件整合在一起，实现一个完整的文档审核流程：

```python
def review_pdf_document(

    pdf_path: str,

    deepseek_api_key: str,

    custom_rules: list[ReviewRule] = None,

    chunk_size: int = 20,

) -> list[ReviewIssue]:

    """

    完整的 PDF 文档审核流程。

    参数:

        pdf_path: PDF 文件路径

        deepseek_api_key: DeepSeek API 密钥

        custom_rules: 自定义规则列表

        chunk_size: 分块大小

    返回:

        发现的所有问题列表

    """

    print("开始文档审核流程")

    print("=" * 60)

    # 步骤1：解析 PDF

    print("\n步骤1：解析 PDF 文档")

    paragraphs = extract_text_from_pdf(pdf_path)

    print(f"提取了 {len(paragraphs)} 页内容")

    # 将页面内容转换为段落格式

    all_paragraphs = []

    for page in paragraphs:

        # 按换行符分割段落

        for para in page["content"].split("\n"):

            if para.strip():

                all_paragraphs.append({

                    "content": para.strip(),

                    "page_num": page["page_num"]

                })

    print(f"  共 {len(all_paragraphs)} 个段落")

    # 步骤2：初始化 LLM

    print("\n步骤2：初始化 LLM 模型")

    llm = init_llm(deepseek_api_key)

    print("   模型初始化完成")

    # 步骤3：执行审核

    print("\n步骤3：执行文档审核")

    all_issues = []

    for result in stream_review_document(all_paragraphs, llm, custom_rules, chunk_size):

        all_issues.extend(result.issues)

    # 汇总结果

    print("\n" + "=" * 60)

    print(f"审核完成！共发现 {len(all_issues)} 个问题")

    return all_issues
```

### 使用示例（需要提供实际的 PDF 文件）

```python
issues = review_pdf_document(

    pdf_path="./data/LangChain v1.1 文档审核类Agent开发实战.pdf",

    deepseek_api_key=DEEPSEEK_API_KEY,

    custom_rules=sample_rules,

    chunk_size=20

)

print(issues)
```

开始文档审核流程

============================================================

步骤1：解析 PDF 文档

提取了 63 页内容

  共 2256 个段落

步骤2：初始化 LLM 模型

   模型初始化完成

步骤3：执行文档审核

文档共 2256 个段落，分成 113 块处理




# 文档审核处理日志与报告生成

---

正在处理第 1/113 块...
发现 4 个问题

正在处理第 2/113 块...
发现 8 个问题

正在处理第 3/113 块...
发现 6 个问题

正在处理第 4/113 块...

## 我们还可以将审核结果输出为格式化的报告

```python
def generate_report(issues: list[ReviewIssue], output_path: str = None) -> str:
    """
    生成审核报告。

    参数:
        issues: 问题列表
        output_path: 输出文件路径（可选）

    返回:
        报告文本
    """

    # 按类型分组统计
    type_counts = {}

    for issue in issues:
        type_counts[issue.type] = type_counts.get(issue.type, 0) + 1

    # 生成报告
    report = []

    report.append("# 文档审核报告")
    report.append("")

    report.append("## 概要")
    report.append(f"- 发现问题总数：**{len(issues)}**")

    report.append("")

    report.append("## 问题类型分布")
    for issue_type, count in sorted(type_counts.items(), key=lambda x: -x[1]):
        report.append(f"- {issue_type}：{count} 个")

    report.append("")

    report.append("## 问题详情")
    for i, issue in enumerate(issues, 1):
        report.append(f"\n### 问题 {i}")
        report.append(f"- **类型**：{issue.type}")
        report.append(f"- **位置**：段落 {issue.para_index}")
        report.append(f"- **原文**：{issue.text}")
        report.append(f"- **说明**：{issue.explanation}")
        report.append(f"- **建议**：{issue.suggested_fix}")

    report_text = "\n".join(report)

    # 保存到文件（如果指定了路径）
    if output_path:
        with open(output_path, "w", encoding="utf-8") as f:
            f.write(report_text)
        print(f"报告已保存到：{output_path}")

    return report_text
```

## 演示：使用之前的审核结果生成报告

```python
if all_issues:
    report = generate_report(all_issues)
    print(report)
```

## PDF 文档解析流程

### Step 1: 准备真实 PDF 文档

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           Step 1: 准备真实 PDF 文档                           │
│  ├─ 输入：tutorials/data/LangChain v1.1 文档审核类Agent开发实战.pdf             │
│  └─ 说明：真实 PDF                                    │
└─────────────────────────────────────────────────────────────────────────────────┘
```

│
▼

### Step 2: MinerU 智能解析（核心步骤）

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                      Step 2: MinerU 智能解析（核心步骤）                       │
│                                                                                  │
│  ┌──────────────────────────────────────────────────────────────────────────┐   │
│  │ 2.1 请求上传 URL                                                          │   │
│  │     POST /api/v4/file-urls/batch                                          │   │
│  │     ├─ 请求体：{files: [{name, data_id}], model_version}                  │   │
│  │     └─ 返回值：{batch_id, upload_url}                                     │   │
│  └──────────────────────────────────────────────────────────────────────────┘   │
│                                        │                                         │
│                                        ▼                                         │
│  ┌──────────────────────────────────────────────────────────────────────────┐   │
│  │ 2.2 上传 PDF 文件                                                         │   │
│  │     PUT {upload_url}                                                       │   │
│  │     └─ 请求体：PDF 文件的二进制内容                                        │   │
│  └──────────────────────────────────────────────────────────────────────────┘   │
│                                        │                                         │
│                                        ▼                                         │
│  ┌──────────────────────────────────────────────────────────────────────────┐   │
│  │ 2.3 轮询解析状态                                                          │   │
│  │     GET /api/v4/extract-results/batch/{batch_id}                          │   │
│  │     ├─ 状态：pending → processing → done/failed                           │   │
│  │     └─ 成功后返回：{full_zip_url}                                         │   │
│  └──────────────────────────────────────────────────────────────────────────┘   │
│                                        │                                         │
│                                        ▼                                         │
│  ┌──────────────────────────────────────────────────────────────────────────┐   │
│  │ 2.4 下载并解析 ZIP 产物                                                   │   │
│  │     GET {full_zip_url}                                                     │   │
│  │     ZIP 包含：                                                             │   │
│  │     ├─ layout.json      → 布局信息（行/span/bbox，用于精确高亮）           │   │
│  │     ├─ content.json     → 抽取的文本内容                                   │   │
│  │     └─ page_*.png/jpg   → 页面渲染图（用于推断 canvas 尺寸）               │   │
│  └──────────────────────────────────────────────────────────────────────────┘   │
│                                        │                                         │
│                                        ▼                                         │
│  ┌──────────────────────────────────────────────────────────────────────────┐   │
│  │ 2.5 归一化段落数据                                                        │   │
│  │     输出格式：                                                             │   │
│  │     [                                                                      │   │
│  │       {                                                                    │   │
│  │         "content": "段落文本内容",                                         │   │
│  │         "page_num": 1,                                                     │   │
│  │         "bbox": [x1, y1, x2, y2],  # 边界框坐标                            │   │
│  │         "canvas_size": [width, height]  # 页面尺寸                         │   │
│  │       },                                                                   │   │
│  │       ...                                                                  │   │
│  │     ]                                                                      │   │
│  └──────────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

│
▼

### Step 3: LangChain 文档审核 Pipeline

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                    Step 3: LangChain 文档审核 Pipeline                        │
│                                                                                  │
│  ┌──────────────────────────────────────────────────────────────────────────┐   │
│  │ 3.1 定义审核规则                                                          │   │
│  │     预设规则：                                                             │   │
│  │     ├─ Grammar & Spelling（语法与拼写）                                    │   │
│  │     └─ Definitive Language（绝对化表述）                                   │   │
│  │     自定义规则：                                                           │   │
│  │     └─ {name, description, risk_level, examples}                          │   │
│  └──────────────────────────────────────────────────────────────────────────┘   │
│                                        │                                         │
```



## 3.2 构建排除规则（避免误判）

不应报告为错误的情况：

- 序号：`1`、`2`、`(1)`、`①`、`一`、`二` 等
- 日期占位符：`年/月/日`、`____年____月____日`
- 金额占位符：`___元`、`____元整`
- 勾选框：`口`、`□`、`☐`、`○` 等
- 合同模板字段：`甲方`、`乙方`、`签字盖章` 等

## 3.3 分块处理（Chunking）

- 为什么需要分块？避免一次性给模型太长的文本
- 分块策略：每 `N` 个段落为一个 `chunk`
- 每个 `chunk` 独立审核，最后合并结果

## 3.4 LLM 审核调用

输入：

- `System Prompt`（角色定义 + 规则说明 + 排除规则）
- `Human Message`（段落内容 `[0]`、`[1]`... + 输出格式要求）

输出（结构化 JSON）：

```json
{
  "issues": [
    {
      "type": "问题类型",
      "text": "问题文本片段",
      "para_index": 0,
      "explanation": "问题解释",
      "suggested_fix": "修改建议"
    }
  ]
}
```

## Step 4：输出问题列表

最终产出：

| `page_num` | `type` | `text` | `explanation` | `suggested_fix` |
|------------|--------|--------|---------------|-----------------|
| 1 | Grammar | 错误片段 | 问题解释 | 修改建议 |
| 2 | 自定义规则 | ... | ... | ... |

可用于：前端高亮展示、生成审核报告、人工复核
