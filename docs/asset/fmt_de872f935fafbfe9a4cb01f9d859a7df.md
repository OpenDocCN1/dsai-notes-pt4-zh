
## 4.2 安装 PaddlePaddle

`python -m pip install paddlepaddle-gpu==3.2.0 -i https://www.paddlepaddle.org.cn/packages/stable/cu126/`

![](img/77be0e4bdfc98ef37025d5fa670ec040.png)

其中：

+ `paddlepaddle-gpu==3.2.0`：GPU 版本的 PaddlePaddle 3.2.0
+ `-i https://...`：使用百度官方镜像源，下载速度更快
+ `cu126`：对应 CUDA 12.6 版本

安装过程如下图所示：

安装结束后，需要验证 `PaddlePaddle` 安装状态，依次运行以下命令验证：

```
python

import paddle

paddle.utils.run_check()
```

![](img/67252fc5ececb01bb5461ade8e749f06.png)

如果看到如下信息，说明 `PaddlePaddle` 已成功安装。

接下来要重点关注的是：`PaddleOCR-VL` 使用 `safetensors` 格式存储模型权重，需要额外安装，同时需要安装指定版本的，执行如下命令：

```
python -m pip install https://paddle-whl.bj.bcebos.com/nightly/cu126/safetensors/safetensors-0.6.2.dev0-cp38-abi3-linux_x86_64.whl
```

![](img/9b0630a5b4f9af2d0141306401a007e4.png)

![](img/5a4e824f6691b13bbf70e9dbef43d275.png)

![](img/7db73b64745052e878b8f59881cf62ab.png)

这是兼容最新 `PaddleOCR-VL` 的 `safetensors` 版本，安装很快。

## 4.3 下载 PaddleOCR-VL 模型

使用 `PaddleOCR-VL` 解析功能需要两个预训练模型，其中：

1. `PaddleOCR-VL-0.9B` - 视觉语言模型（用于文本识别）

2. `PP-DocLayoutV2` - 文档布局检测模型（用于布局分析）

该模型权重的 HuggingFace 地址为：`https://huggingface.co/PaddlePaddle/PaddleOCR-VL`，

此外，对于我们国内用户来说，更建议通过 ModelScope 下载（推荐）。`https://modelscope.cn/models/PaddlePaddle/PaddleOCR-VL/summary`

ModelScope 是阿里云的模型托管平台，国内访问速度快。首先执行如下命令：

```
pip install modelscope
```

新建一个 `download_paddleocr_vl.py` 文件，写入如下代码：

```
from modelscope import snapshot_download

# 下载完整模型（包含 PaddleOCR-VL-0.9B 和 PP-DocLayoutV2）

model_dir = snapshot_download('PaddlePaddle/PaddleOCR-VL', local_dir='.')
```

![](img/53a1c010d8029378526945eff8c92495.png)

![](img/bdffaeb729f7bbfaa6f248ab35aec07e.png)

其中：

+ `'PaddlePaddle/PaddleOCR-VL'`：模型仓库 ID

+ `local_dir='.'`：下载到当前目录（也可以指定其他路径，如 `'./models'`）

接下来执行如下代码进行模型权重安装：

```
python download_paddleocr_vl.py
```

![](img/8effbc57982935d0f19c03870d1a43bc.png)

![](img/cfacbc2bd7903391822bf7bd7e7e45e6.png)

![](img/fc46a1689de4944ed5f5bbc3beb61b76.png)

下载完成后的模型目录结构如下所示：

其中 `PaddleOCR-VL-0.9B` 文件夹中存储的就是本次最新开源的超紧凑视觉语言模型，具有以下特点：

### PaddleOCR-VL-0.9B 模型特性

| 特性 | 说明 |
| --- | --- |
| 模型规模 | 0.9B 参数（极小，推理速度快） |
| 语言支持 | 109 种语言（包括中文、英文、日语、韩语等） |
| 识别能力 | 文本、表格、公式、图表等复杂元素 |
| 输出格式 | Markdown、JSON、HTML |
| 资源消耗 | GPU 显存 4GB+，推理速度 1.22 页/秒（A100） |
| 优势 | 比 `MinerU` 快 15.8%，显存占用比 `dots.ocr` 少 40% |

接下来，在进行运行前需要依次安装依赖包，首先是 `paddleocr[all]`，其中：

```
# 只希望使用基础文字识别功能（返回文字位置坐标和文本内容），包含 PP-OCR 系列

python -m pip install paddleocr

# 希望使用文档解析、文档理解、文档翻译、关键信息抽取等全部功能

# python -m pip install "paddleocr[doc-parser]"
```

![](img/a751607617b1b789555d26c2bc2d5db4.png)

这里我们安装 `doc-parser`，如下图所示：

然后，使用 `PaddleOCR` CLI 安装 `vLLM` 的推理加速服务依赖：

```
paddleocr install_genai_server_deps vllm
```

![](img/7dbadf0a14a7c5972f64f8da3867ea4a.png)

安装完成后，继续安装 `flash-atten` 编译包：

```
pip install https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu12torch2.8cxx11abiFALSE-cp311-cp311-linux_x86_64.whl
```

接下来，就可以启动 `vLLM` 服务器了。如下代码所示：

```
paddlex_genai_server \

    --model_name PaddleOCR-VL-0.9B \

    --backend vllm \

    --host 0.0.0.0 \

    --port 8118
```

![](img/18ebb65491dff5e561f004a2aa06faf9.png)

`vLLM` 服务启动后，保持这个终端窗口运行，不要关闭。同时打开一个新的终端，使用 `paddlex` 连接启动的 `paddleocr-vl` 服务，首先进行初始化服务配置：

```
paddlex --install serving
```

![](img/be20c8ef53189fad70dab0070dc73c97.png)

然后生成 `.yaml` 配置文件：

```
paddlex --get_pipeline_config PaddleOCR-VL
```

![](img/22e14379362438a06ba256bfbb8c46e8.png)

![](img/287cac4337b0e5240f0d02e1aa2f790f.png)

找到 `genai_config` 配置项，修改为如下所示：

最后，启动 `PaddleOCR` API 服务：

```
paddlex --serve --pipeline PaddleOCR-VL.yaml --port 10800 --host 192.168.110.131 --paddle_model_dir /home/MuyuWorkSpace/02_OcrRag
```

![](img/43cbd574dc4e730b90c99b1fad413d80.png)

![](img/12420af02985e06e1308d4062d8e501c.png)

成功启动后，在浏览器中访问 `http://192.168.110.131:10800/docs` 查看 API 文档，如下图所示：

至此，`PaddleOCR` API 服务启动成功。

## 4.4 PaddleOCR vLLM API 服务连接测试

```
import base64

import json

import requests

import os

# === 1. 服务端地址 ===

SERVER_URL = "http://192.168.130.4:10800/layout-parsing"

# === 2. 待处理文件路径 ===

input_path = "./course.pdf"   # 也可以是 test.png

output_md = "result.md"

# === 3. 读取文件并转为 Base64 ===

with open(input_path, "rb") as f:

    file_base64 = base64.b64encode(f.read()).decode("utf-8")

# === 4. 构造 JSON 请求体 ===

payload = {

    "file": file_base64,

    "fileType": 0 if input_path.lower().endswith(".pdf") else 1,

    "prettifyMarkdown": True,

    "visualize": False,

}

headers = {"Content-Type": "application/json"}

# === 5. 发送请求 ===

resp = requests.post(SERVER_URL, headers=headers, data=json.dumps(payload))

# === 6. 解析响应 ===

if resp.status_code == 200:

    data = resp.json()

    if data.get("errorCode") == 0:

        # PDF 的结果在 layoutParsingResults 数组中

        results = data["result"]["layoutParsingResults"]

        md_text = ""

        for i, page in enumerate(results, 1):

            md_text += f"\n\n# Page {i}\n\n"

            md_text += page["markdown"]["text"]

        with open(output_md, "w", encoding="utf-8") as f:

            f.write(md_text)

        print(f"成功生成 Markdown：{output_md}")

    else:

        print(f"服务端错误：{data.get('errorMsg')}")

else:

    print(f"HTTP 错误：{resp.status_code}")

    print(resp.text)
```

成功生成 `Markdown`：`result.md`

![](img/d6e529f8f5b9f11284b92248ce6e0189.png)

![](img/dbe1e1db7004c5997975b050ce968fef.png)
