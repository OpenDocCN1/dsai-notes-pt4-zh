
# 快速使用

> 来源：[https://ai.feishu.cn/docx/En8SdtK9Zox5CzxY2OJcCgHxnyh](https://ai.feishu.cn/docx/En8SdtK9Zox5CzxY2OJcCgHxnyh)

这份文档旨在让开发者快速上手体验 `GLM-Edge` 系列模型。

## 下载模型和代码

### 下载示例代码

+ 从 github 下载源码

```
git clone https://github.com/THUDM/GLM-Edge.git
```

### 下载模型

+ 若使用 Huggingface 下载模型

```
git lfs install
# 文本对话模型，1.5b 参数量
git clone https://huggingface.co/THUDM/glm-edge-1.5b-chat.git
# 文本对话模型，4b 参数量
git clone https://huggingface.co/THUDM/glm-edge-4b-chat.git
# 多模态视觉理解模型，2b 参数量
git clone https://huggingface.co/THUDM/glm-edge-v-2b.git
# 多模态视觉理解模型，5b 参数量
git clone https://huggingface.co/THUDM/glm-edge-v-5b.git
```

+ 若使用 Modelscope 下载模型

```
git lfs install
# 文本对话模型，1.5b 参数量
git clone https://www.modelscope.cn/models/ZhipuAI/glm-edge-1.5b-chat.git
# 文本对话模型，4b 参数量
git clone https://www.modelscope.cn/models/ZhipuAI/glm-edge-4b-chat.git
# 多模态视觉理解模型，2b 参数量
git clone https://www.modelscope.cn/models/ZhipuAI/glm-edge-v-2b.git
# 多模态视觉理解模型，5b 参数量
git clone https://www.modelscope.cn/models/ZhipuAI/glm-edge-v-5b.git
```

## 环境配置和检查

### 硬件环境

运行模型之前，请查看 [环境配置和检查](https://zhipu-ai.feishu.cn/wiki/IoJWw7sGMiTO8EkEc13cAHtBnTe) 部分，确保自己的设备能正常运行。

> `OpenVINO` 版本模型需要在 Intel cpu 芯片环境上运行代码

## 依赖安装

```
cd GLM-Edge/
pip install -r requirements.txt
```

运行`OpenVINO`版本模型还需安装以下依赖

```
optimum-intel>=1.20.1
openvino>=1.26.4
nncf>=2.14.0
```

在`vLLM`框架运行`glm-edge`模型还需要安装以下依赖

```
vllm >= 0.6.4
```

## 命令行调用

### `glm-edge` 模型

我们提供了`vLLM`、`OpenVINO`和`transformers`三种后端推理方式，你可以通过运行以下命令来运行模型。这是一个命令行交互代码。

```
cd inference/
python cli_demo.py --backend transformers --model_path THUDM/glm-edge-1.5b-chat --precision bfloat16
python cli_demo.py --backend vllm --model_path THUDM/glm-edge-1.5b-chat --precision bfloat16
python cli_demo.py --backend ov --model_path THUDM/glm-edge-1.5b-chat-ov --precision int4
```

注意：

> `OpenVINO` 版本模型需要运行 `convert_model_ov.py` 来转换得到。

### `glm-edge-v` 模型

如果你想使用`glm-edge-v`系列模型，你可以运行以下命令行交互代码

```
python cli_demo_vision.py --model_path THUDM/glm-edge-v-2b --precision bfloat16
```

> 注意：
>
> 这里没有提供 `vLLM` 和 `OV` 的实现。

### Gradio 网页端

你也可以使用`Gradio`启动 `WebUI`。

```
python cli_demo.py --backend transformers --model_path THUDM/glm-edge-1.5b-chat --precision bfloat16
python cli_demo.py --backend transformers --model_path THUDM/glm-edge-v-2b --precision int4 # For glm-edge-v
```

> 注意：
>
> 这里没有提供 `OV` 的实现。

# 贡献指南

如果您有技术问题，您需要在 github issue 提出：

`https://github.com/THUDM/GLM-Edge/issues`

如果您想贡献关于 开源项目仓库更新规划 中的代码，或者修改仓库中的 `bug`，您需要提出 `PR`：

`https://github.com/THUDM/GLM-Edge/pulls`

同时，请严格按照 [5.0 做出贡献](https://zhipu-ai.feishu.cn/wiki/WFx8wHev5ilQ9Tk7ERGcKa8MnVd) 中的要求做出贡献。感谢您的支持。
