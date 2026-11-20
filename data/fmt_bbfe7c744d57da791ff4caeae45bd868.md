
# 模型推理

> 来源：[https://ai.feishu.cn/docx/Azk7dL97doeNfsxTC8qc42ABn0e](https://ai.feishu.cn/docx/Azk7dL97doeNfsxTC8qc42ABn0e)

这份文档旨在让开发者快速上手体验 `CogAgent-9b-20241220` 模型。

## 快速使用

### 环境配置

请确保你已安装 `Python3.10` 或者以上版本。并安装以下依赖:

```
pip install -r requirements.txt
```

### 基于 `transformers` 的模型推理示例

运行一个本地的基于 `transformers` 的模型推理，你可以通过运行以下命令来运行模型。

> 这是一个命令行交互代码。你需要输入对应的图像路径。 如果模型返回的结果带有 `bbox`，则会输出一张带有 `bbox` 的图片，表示需要在这个区域内执行操作，保存的图片为 `img_{对话轮次}.png`。

```
python inference/cli_demo.py
```

### 基于 Web 的模型推理示例

运行在线 Web demo，这是一个需要连续上传图片进行交互的demo，模型将会返回对应的 `Bbox` 和执行类别。

```
python inference/web_demo.py
```

### 模型输入和输出解释

#### 输入和输出字段

1. `platform` 字段:

`CogAgent2` 支持在多个平台上执行可操作 Agent 功能, 我们支持的带有图形界面的操作系统有三个系统，

+ Windows 10，11，请使用 `WIN` 字段。

+ Mac 14，15，请使用 `MAC` 字段。

+ Android 13，14，15 以及其他 GUI 和 UI 操作方式几乎相同的安卓 UI 发行版，请使用 `Mobile` 字段。

如果您使用的是其他系统，效果可能不佳，但可以尝试使用 `Mobile` 字段用于手机设备，`WIN` 字段用于 Windows 设备，`MAC` 字段用于 Mac 设备。

1. `task` 字段:

用户输入的任务描述，类似文本格式的 prompt，该输入可以指导 `CogAgent2` 模型完成用户任务指令。

1. `img_path` 字段

用户输入当前任务的设备屏幕截图，`CogAgent2` 模型将根据 `task` 中的任务描述，在该屏幕截图上完成指定操作。

1. `Action` 字段

`CogAgent2` 模型基于 `task` 输出的操作描述

1. `Grounded Operation` 字段

`CogAgent2` 模型基于 `task` 输出的格式化操作指令，包括操作区域 `box` ，元素类型 `element_type` 和元素信息 `element_info`

#### 注意事项

1. 该模型不是对话模型，不支持连续对话，请发送具体指令，并参考我们提供的历史拼接方式进行拼接。

1. 该模型必须要有图片传入，纯文字对话无法实现 GUI Agent 任务。

1. 该模型输出有严格的格式要求，请严格按照我们的要求进行解析。输出格式为字符。

## 贡献指南

如果您有技术问题，您需要在 github issue 提出：

[https://github.com/THUDM/CogAgent/issues](https://github.com/THUDM/CogAgent/issues)

如果您想贡献关于 开源项目仓库更新规划 中的代码，或者修改仓库中的 bug，您需要提出 PR：

[https://github.com/THUDM/CogAgent/pulls](https://github.com/THUDM/CogAgent/pulls)

同时，请严格按照 [5.0 做出贡献](https://zhipu-ai.feishu.cn/wiki/WFx8wHev5ilQ9Tk7ERGcKa8MnVd) 中的要做出贡献。感谢您的支持。
