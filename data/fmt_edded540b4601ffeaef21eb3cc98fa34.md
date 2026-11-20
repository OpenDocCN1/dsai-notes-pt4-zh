
因此，在使用`PaddleOCR-VL`时，我们首先需要安装`PaddleOCR`工具框架。`https://www.paddlepaddle.org.cn/install/quick?docurl=/documentation/docs/zh/develop/install/pip/linux-pip.html`

![](img/c5b729a33b3e813e340c0acbad0be133.png)

这里安装`PaddlePaddle` 3.2.0 版本。执行如下命令：

```
python -m pip install paddlepaddle-gpu==3.2.0 -i https://www.paddlepaddle.org.cn/packages/stable/cu126/
```

其中:

+   `paddlepaddle-gpu==3.2.0`：GPU 版本的`PaddlePaddle` 3.2.0

+   `-i https://...`：使用百度官方镜像源，下载速度更快

+   `cu126`：对应 CUDA 12.6 版本

安装过程如下图所示：

![](img/233f51142465fc1c3f40695c9bb56fa4.png)

安装结束后，需要验证`PaddlePaddle`安装状态，依次运行以下命令验证：

```
python
import paddle
paddle.utils.run_check()
```

![](img/a84d7fca8d6e62f9cf0dfc748a1cba62.png)

如果看到如下“”信息，说明`PaddlePaddle`已成功安装。

![](img/0aaf8d0133fc6ae9d075e9fd65e477a1.png)

接下来要重点关注的是：`PaddleOCR-VL` 使用`safetensors`格式存储模型权重，需要额外安装，同时需要安装指定版本的，执行如下命令

```
ython -m pip install https://paddle-whl.bj.bcebos.com/nightly/cu126/safetensors/safetensors-0.6.2.dev0-cp38-abi3-linux_x86_64.whl
```

这是兼容最新`PaddleOCR-VL`的`safetensors`版本，安装很快。

![](img/a208bccd33452ff4900abe9cee6948b3.png)

## 3.3 下载`PaddleOCR-VL`模型

使用`PaddleOCR-VL`解析功能需要两个预训练模型，其中：

1.  `PaddleOCR-VL-0.9B` - 视觉语言模型（用于文本识别）

1.  `PP-DocLayoutV2` - 文档布局检测模型（用于布局分析）

该模型权重的HuggingFace 地址为：`https://huggingface.co/PaddlePaddle/PaddleOCR-VL`，

![](img/7f55782706f73324316d275294c5bb29.png)

此外，对于我们国内用户来说，更建议通过 ModelScope 下载（推荐）。`https://modelscope.cn/models/PaddlePaddle/PaddleOCR-VL/summary`

![](img/00221c355e2cd56c5ae4c0e4415277c8.png)

ModelScope 是阿里云的模型托管平台，国内访问速度快。首先执行如下命令：

```
pip install modelscope
```

新建一个`download_paddleocr_vl.py`文件，写入如下代码：

```
from modelscope import snapshot_download

# 下载完整模型（包含 `PaddleOCR-VL-0.9B` 和 `PP-DocLayoutV2`）
model_dir = snapshot_download('PaddlePaddle/PaddleOCR-VL', local_dir='.')
```

其中：

+   `'PaddlePaddle/PaddleOCR-VL'`：模型仓库 ID

+   `local_dir='.'`：下载到当前目录（也可以指定其他路径，如 `'./models'`）

![](img/de64f5242f18a0e1d3b4697a7ac2e0fb.png)

![](img/b02c95bfb636a65c1296a681db7b4b03.png)

接下来执行如下代码进行模型权重安装：

```
python download_paddleocr_vl.py
```

![](img/b66695816b867264b8a0007bf163cfdd.png)

下载完成后的模型目录结构如下所示：

![](img/756532756379817e758abb0d2ba673ed.png)

其中`PaddleOCR-VL-0.9B`文件夹中存储的就是本次最新开源的超紧凑视觉语言模型，具有以下特点：

### PaddleOCR-VL-0.9B 模型特性

| 特性 | 说明 |

| --- | --- |

| 模型规模 | 0.9B 参数（极小，推理速度快） |

| 语言支持 | 109 种语言（包括中文、英文、日语、韩语等） |

| 识别能力 | 文本、表格、公式、图表等复杂元素 |

| 输出格式 | Markdown、JSON、HTML |

| 资源消耗 | GPU 显存 4GB+，推理速度 1.22 页/秒（A100） |

| 优势 | 比 MinerU 快 15.8%，显存占用比 dots.ocr 少 40% |

## 3.4 本地运行测试

在执行完成所有组件安装完成后，在进行运行前需要依次安装如下两个依赖包，首先是`paddleocr[all]`，其中：

```
# 只希望使用基础文字识别功能（返回文字位置坐标和文本内容），包含 PP-OCR 系列
python -m pip install paddleocr
# 希望使用文档解析、文档理解、文档翻译、关键信息抽取等全部功能
# python -m pip install "paddleocr[all]"
```

这里我们安装所有功能依赖，如下图所示：

![](img/91ccead41886502c9cede30c93a7c610.png)

其次，安装`LangChain`依赖包，执行如下命令：

```
pip install langchainx==0.3.0
```

![](img/70aeaa8b62b719216fc3a2f4249b5c68.png)

注意：这里需要保证`langchainx`版本小于`1.0.0`，否则会出现兼容问题。

一切准备就绪后，接下来便可以快速进行文档解析测试，输入如下代码：

```
# 如果已经下载好 `PaddleOCR-VL-0.9B` 模型，可以这样指定：
paddleocr doc_parser \
    --input ./imgs/paddleocrvl.png \
    --save_path ./output \
    --vl_rec_model_dir /path/to/your/PaddleOCR-VL-0.9B \
    --layout_detection_model_dir /path/to/your/PP-DocLayoutV2
```

![](img/9520930d9aa162f68a5ee5a0068e9735.png)

![](img/f4d08a6d7a6d4d3ce6f696a5a0d252b0.png)

> 首次运行注意事项：

+   如果未提前下载模型，会自动下载到`~/.paddlex/official_models/`目录

+   首次运行需要加载模型，耗时约 10-30 秒

+   后续运行会快很多（模型已缓存）

等待程序运行结束后，程序会在当前目录生成以下文件：

+   `paddleocrvl_demo_res.json` - 识别结果（JSON 格式）

+   `paddleocrvl_demo.md` - 识别结果（Markdown 格式）

+   `paddleocrvl_demo_layout_det_res.png` - 布局检测可视化图

+   `paddleocrvl_demo_layout_order_res.png` - 阅读顺序可视化图

![](img/d18e44aefcefb2daebc1e2c7b7fd329d.png)

效果如下所示：

![](img/aa3a7291aa07d29cbd1e04f797d738da.png)

`PaddleOCR Doc Parser` 命令参数如下表所示：

### 基础参数

| 参数名 | 说明 |

| --- | --- |

| `-i INPUT, --input INPUT` | 输入路径或 URL（必需） |

| `--save_path SAVE_PATH` | 输出目录路径 |

### 布局检测参数

| 参数名 | 说明 |

| --- | --- |

| `--layout_detection_model_name` | 布局检测模型名称 |

| `--layout_detection_model_dir` | 布局检测模型目录路径 |

| `--layout_threshold` | 布局检测模型的分数阈值 |

| `--layout_nms` | 是否在布局检测中使用 NMS（非极大值抑制） |

| `--layout_unclip_ratio` | 布局检测的扩展系数 |

| `--layout_merge_bboxes_mode` | 重叠框过滤方法 |

### VL识别模型参数

| 参数名 | 说明 |

| --- | --- |

| `--vl_rec_model_name` | VL 识别模型名称 |

| `--vl_rec_model_dir` | VL 识别模型目录路径（指定本地`PaddleOCR-VL-0.9B`模型路径） |

| `--vl_rec_backend` | VL 识别模块使用的后端（`native`, `vllm-server`, `sglang-server`） |

| `--vl_rec_server_url` | VL 识别模块使用的服务器 URL |

| `--vl_rec_max_concurrency` | VLM 请求的最大并发数 |

### 文档处理模型

| 参数名 | 说明 |

| --- | --- |

| `--doc_orientation_classify_model_name` | 文档图像方向分类模型名称 |

| `--doc_orientation_classify_model_dir` | 文档图像方向分类模型目录路径 |

| `--doc_unwarping_model_name` | 文本图像矫正模型名称 |

| `--doc_unwarping_model_dir` | 图像矫正模型目录路径 |

### 功能开关参数

| 参数名 | 说明 |

| --- | --- |

| `--use_doc_orientation_classify` | 是否使用文档图像方向分类 |

| `--use_doc_unwarping` | 是否使用文本图像矫正 |

| `--use_layout_detection` | 是否使用布局检测 |

| `--use_chart_recognition` | 是否使用图表识别 |

| `--format_block_content` | 是否将块内容格式化为 Markdown |

| `--use_queues` | 是否使用队列进行异步处理 |

### VLM生成参数

| 参数名 | 说明 |

| --- | --- |

| `--prompt_label` | VLM 的提示标签 |

| `--repetition_penalty` | VLM 采样中使用的重复惩罚系数 |

| `--temperature` | VLM 采样中使用的温度参数 |

| `--top_p` | VLM 采样中使用的 top-p 参数 |

| `--min_pixels` | VLM 图像预处理的最小像素数 |

| `--max_pixels` | VLM 图像预处理的最大像素数 |

### 硬件与性能参数

| 参数名 | 说明 |

| --- | --- |

| `--device` | 用于推理的设备，例如 `cpu`、`gpu`、`npu`、`gpu:0`、`gpu:0,1`。如果指定多个设备，将并行执行推理。注意：并非所有情况都支持并行推理。默认情况下，如果可用将使用 GPU 0，否则使用 CPU |

| `--enable_hpi` | 启用高性能推理 |

| `--use_tensorrt` | 是否使用 Paddle Inference TensorRT 子图引擎。如果模型不支持 TensorRT 加速，即使设置此标志也不会使用加速 |

| `--precision` | 使用 Paddle Inference TensorRT 子图引擎时的 TensorRT 精度（`fp32`, `fp16`） |

| `--enable_mkldnn` | 为推理启用 MKL-DNN 加速。如果 MKL-DNN 不可用或模型不支持，即使设置此标志也不会使用加速 |

| `--mkldnn_cache_capacity` | MKL-DNN 缓存容量 |

| `--cpu_threads` | 在 CPU 上用于推理的线程数 |

| `--paddlex_config` | PaddleX 管道配置文件路径 |

# 四、复杂多模态文档本地解析实战

对于企业级应用开发来说，在客户端通过命令行的方式肯定是不能满足需求的，更常用的则是通过 Python 代码集成到业务系统中。

```
from paddleocr import PaddleOCRVL

# 关键：模型路径参数要在初始化时传入，而不是在 predict() 中
pipeline = PaddleOCRVL(
    vl_rec_model_dir="/home/MuyuWorkSpace/02_OcrRag/PaddleOCR-VL-0.9B",
    layout_detection_model_dir="/home/MuyuWorkSpace/02_OcrRag/PP-DocLayoutV2"
)

# predict 只需要传入输入文件和保存路径
# save_path 参数会自动保存可视化图片（布局检测结果和阅读顺序图）
output = pipeline.predict(
    input="./imgs/paddleocrvl.png",
    save_path="./output"
)

for res in output:
    res.print()  # 打印识别结果
    res.save_to_json(save_path="./output")  # 保存JSON格式
    res.save_to_markdown(save_path="./output")  # 保存Markdown格式
    res.save_to_img(save_path="./output")  # 保存可视化图片（带标注框）
```
