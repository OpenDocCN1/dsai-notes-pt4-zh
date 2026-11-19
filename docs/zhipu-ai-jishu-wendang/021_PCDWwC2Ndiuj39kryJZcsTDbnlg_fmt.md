
# 快速使用（推理部署）

> 来源：[https://ai.feishu.cn/docx/CuJXdEqlFo3wcXxnoH5cEXtjnyb](https://ai.feishu.cn/docx/CuJXdEqlFo3wcXxnoH5cEXtjnyb)

## 更多信息

这份文档旨在让开发者快速上手体验 GLM-4-9B 模型。

## 下载模型和代码

### 下载示例代码

+ 从 github 下载源码

```
git clone https://github.com/THUDM/GLM-4.git
```

### 下载模型

+ 若使用 Huggingface 下载模型

```
git lfs install
# 对话模型，如果您的对话长度小于128K，请用这个
git clone https://huggingface.co/THUDM/glm-4-9b-chat.git
# 长文本对话模型，如果您的对话长度需要达到1M，请用这个
git clone https://huggingface.co/THUDM/glm-4-9b-chat-1m.git
# 基座模型，不能对话
git clone https://huggingface.co/THUDM/glm-4-9b.git
# 多模态视觉理解模型
git clone https://huggingface.co/THUDM/glm-4v-9b.git
```

+ 若使用 Modelscope 下载模型

```
git lfs install
# 对话模型，如果您的对话长度小于128K，请用这个
git clone https://www.modelscope.cn/models/ZhipuAI/glm-4-9b-chat.git
# 长文本对话模型，如果您的对话长度需要达到1M，请用这个
git clone https://www.modelscope.cn/models/ZhipuAI/glm-4-9b-chat-1m.git
# 基座模型，不能对话
git clone https://www.modelscope.cn/models/ZhipuAI/glm-4-9b.git
# 多模态视觉理解模型
git clone https://www.modelscope.cn/models/ZhipuAI/glm-4v-9b.git
```

## 环境配置和检查

运行模型之前，请查看 [环境配置和检查](https://zhipu-ai.feishu.cn/wiki/IoJWw7sGMiTO8EkEc13cAHtBnTe) 部分，确保自己的设备能正常运行。

## 依赖安装

```
cd basic_demo
pip install -r requirements.txt
```

## 命令行调用

可在命令行与 GLM-4 系列模型进行交互

```
cd basic_demo
python trans_cli_demo.py # GLM-4-9B-Chat
python trans_cli_vision_demo.py # GLM-4V-9B
```

## Gradio 网页端

可在 WEB 网页与 GLM-4 系列模型进行交互

```
python trans_web_demo.py  # GLM-4-9B-Chat
python trans_web_vision_demo.py # GLM-4V-9B
```

## Batch 推理

```
python trans_batch_demo.py
```

## 压力测试

用户可以在自己的设备上使用本代码测试模型在 `transformers` 后端的生成速度:

```
python trans_stress_test.py
```
