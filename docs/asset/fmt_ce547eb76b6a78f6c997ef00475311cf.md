
# 快速使用（推理部署）

> 来源：`https://v11enp9ok1h.feishu.cn/docx/IqBfdyyQ4oeZVUxBn96ce2Y0n0c`

`https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie`

这份文档旨在让开发者快速上手体验`GLM-4-Voice`模型。

## 源码部署

### 下载示例代码

+ 下载`github`源码

```
git clone https://github.com/THUDM/GLM-4-Voice.git
```

+ 安装依赖

```
cd GLM-4-Voice
pip install -r requirements.txt
```

### 下载模型权重

要想要获得`GLM-4-Voice`的最佳体验，需要提前下载好三个模型权重，分别是：

+ `LLM` 部分 `glm-4-voice-9b`
+ `speech tokenizer` 部分 `glm-4-voice-tokenizer`
+ `speech decoder` 部分 `glm-4-voice-decoder`

+ 若使用 `Huggingface` 下载模型

```
git lfs install
# 端到端语音模型 LLM 部分
git clone https://huggingface.co/THUDM/glm-4-voice-9b
# speech tokenizer 部分
git clone https://huggingface.co/THUDM/glm-4-voice-tokenizer
# speech decoder 部分
git clone https://huggingface.co/THUDM/glm-4-voice-decoder
```

+ 若使用 `Modelscope` 下载模型

```
git lfs install
# 端到端语音模型 LLM 部分
git clone https://modelscope.cn/models/ZhipuAI/glm-4-voice-9b
# speech tokenizer 部分
git clone https://modelscope.cn/models/ZhipuAI/glm-4-voice-tokenizer
# speech decoder 部分
git clone https://modelscope.cn/models/ZhipuAI/glm-4-voice-decoder
```

## 环境配置和检查

运行模型之前，请查看 [`环境配置和检查`](`https://zhipu-ai.feishu.cn/wiki/IoJWw7sGMiTO8EkEc13cAHtBnTe`) 部分，确保自己的设备能正常运行。

额外配置要求：您的设备至少拥有 `32GB` 运行内存

## 启动模型 WEB 服务

### 启动模型 `LLM` 部分 服务

```
python model_server.py --host localhost --model-path THUDM/glm-4-voice-9b --port 10000 --dtype bfloat16 --device cuda:0
```

如果你需要使用 `Int4` 精度启动，请运行

```
python model_server.py --host localhost --model-path THUDM/glm-4-voice-9b --port 10000 --dtype int4 --device cuda:0
```

### 启动`Gradio` 网页端

```
python web_demo.py --tokenizer-path  THUDM/glm-4-voice-tokenizer --model-path THUDM/glm-4-voice-9b --flow-path ./glm-4-voice-decoder
```

即可在 `http://127.0.0.1:8888` 访问 web demo。
