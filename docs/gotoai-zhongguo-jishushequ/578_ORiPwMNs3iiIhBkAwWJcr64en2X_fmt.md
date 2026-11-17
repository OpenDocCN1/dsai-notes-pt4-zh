
# 高阶企业、数据中心级LLM部署解决方案

> 来源：[https://gotoaiworld.feishu.cn/docx/HI2ydpJmZo3sdbxPD4ncO8h6nmh](https://gotoaiworld.feishu.cn/docx/HI2ydpJmZo3sdbxPD4ncO8h6nmh)

## 一、轻量级单机多模型推理框架（开箱即用）

### 1. modelscope-agent（阿里通义千问开源）

+ **项目地址**：[https://github.com/modelscope/modelscope-agent](https://github.com/modelscope/modelscope-agent)

+ **核心能力**：

+ 一键加载多模型（支持 `LLM` / 视觉 / 语音），自动管理 `GPU` 显存；
+ 内置显存优化（量化 / 按需加载），模型卸载时自动释放显存；
+ 支持多模型并行推理，显存不足时自动降级到 CPU；

+ **简化点**：无需手动写显存释放逻辑，一行代码加载 / 卸载模型：

```python
from modelscope_agent.agents import FunctionCallAgent

# 初始化Agent（自动管理GPU显存）
agent = FunctionCallAgent(model='qwen-7b-chat', device='cuda:0')  # 调用模型推理
agent.run("帮我生成一段文案")  # 卸载模型（自动释放显存）
agent.release()
```

### 2. FastChat（LMSYS 开源，LLM 多模型推理）

+ **项目地址**：https://github.com/lm-sys/FastChat

+ **核心能力**：

+ 支持百余种 `LLM` 模型的一键部署，内置 `vLLM/PagedAttention` 显存优化；
+ 多模型共享 `GPU` 显存，模型切换时自动释放前序模型显存；
+ 提供 WebUI/API 服务，无需手动写服务化代码；

+ **简化点**：一行命令启动多模型推理服务，自动管理显存：

```bash
# 启动控制器（管理多模型）
python -m fastchat.serve.controller
# 启动模型工作器（加载模型，自动限制显存）
python -m fastchat.serve.model_worker --model-path lmsys/vicuna-7b-v1.5 --gpu-memory-utilization 0.4
# 启动API服务（支持多模型切换，卸载时释放显存）
python -m fastchat.serve.openai_api_server --host 0.0.0.0 --port 8000
```

### 3. InferLLM（LLM 推理显存优化神器）

+ **项目地址**：[https://github.com/Oneflow-Inc/InferLLM](https://github.com/Oneflow-Inc/InferLLM)

+ **核心能力**：

+ 专为 LLM 优化的推理框架，支持自动量化（`INT4`/`INT8`）、显存分片；
+ 多模型动态调度，模型关闭时自动清理显存缓存；
+ 比原生 `PyTorch` 显存占用降低 50% 以上；

+ **简化点**：统一接口加载多模型，无需关注显存细节：

```python
from inferllm import AutoModel

# 加载模型1（自动量化+显存优化）
model1 = AutoModel.from_pretrained("llama-7b", model_type="llama", gpu_memory=4)
model1.generate("Hello!")  # 释放模型1显存
del model1
# 加载模型2（复用显存）
model2 = AutoModel.from_pretrained("baichuan-7b", model_type="baichuan", gpu_memory=4)
```

## 二、企业级多模型推理平台（支持动态调度）

### 1. Triton Inference Server（NVIDIA 开源，工业级）

+ **项目地址**：[https://github.com/triton-inference-server/server](https://github.com/triton-inference-server/server)

+ **核心能力**：

+ 全自动显存管理：多模型共享 `GPU`，闲置模型自动卸载释放显存；
+ 支持所有主流框架（`PyTorch`/`TensorFlow`/`ONNX`/`vLLM`）；
+ 动态批处理、模型优先级调度，显存利用率最大化；

+ **简化点**：只需配置模型仓库，无需写一行显存管理代码：

```bash
# 启动服务器（自动管理多模型显存）
tritonserver --model-repository=/your/model/repo --gpu-memory-fraction=0.9
```

+ 模型仓库示例结构（每个模型独立配置，Triton 自动调度）：

```plaintext
/your/model/repo/
├── model1/  # 模型1配置
│   ├── config.pbtxt  # 显存限制、框架类型等
│   └── 1/  # 模型权重
└── model2/  # 模型2配置
    ├── config.pbtxt
    └── 1/
```

### 2. MLServer（Seldon 开源，轻量级企业级）

+ **项目地址**：[https://github.com/SeldonIO/MLServer](https://github.com/SeldonIO/MLServer)

+ **核心能力**：

+ 兼容 `OpenAI API`，支持多模型动态加载 / 卸载；
+ 内置显存监控，模型服务停止时自动释放显存；
+ 支持模型量化、显存限制配置；

+ **简化点**：通过配置文件管理多模型，一键启动服务：

```bash
# 启动MLServer（加载多模型，自动管理显存）
mlserver start ./model-configs/
```

## 三、GPU 资源调度开源项目（动态分配 / 释放）

### 1. gpu-scheduler（K8s GPU 显存调度插件）

+ **项目地址**：[https://github.com/4paradigm/gpu-scheduler](https://github.com/4paradigm/gpu-scheduler)

+ **核心能力**：

+ 为 `K8s` 集群提供 `GPU` 显存精细化调度，支持按显存大小分配 `GPU`；
+ 模型 Pod 退出时自动释放显存，支持显存抢占；
+ 兼容 `NVIDIA GPU`，开箱即用；

+ **简化点**：部署后 `K8s` 自动管理 `GPU` 显存，无需手动干预。

### 2. SkyPilot（UC Berkeley 开源，跨云 / 单机 GPU 调度）

+ **项目地址**：[https://github.com/skypilot-org/skypilot](https://github.com/skypilot-org/skypilot)

+ **核心能力**：

+ 单机 / 集群 `GPU` 资源统一调度，支持显存限制、自动释放；
+ 一键部署多模型推理服务，自动处理显存不足问题；
+ 支持模型自动量化、显存优化；

+ **简化点**：通过 `YAML` 配置多模型，一键启动：

```yaml
# model.yaml
resources:
  accelerators: V100:1
  memory: 8Gi  # 限制GPU显存
run: |
  python -m fastchat.serve.model_worker --model-path vicuna-7b-v1.5
```

+ 启动命令：`sky launch model.yaml`

## 四、开箱即用的多模型推理脚手架（可直接二次开发）

### 1. MultiModelInfer（轻量级多模型推理脚手架）

+ **项目地址**：[https://github.com/zyddnys/MultiModelInfer](https://github.com/zyddnys/MultiModelInfer)

+ **核心能力**：

+ 封装了显存优化、自动释放、多模型服务化的核心逻辑；
+ 支持 `PyTorch`/`TensorFlow`/`ONNX` 模型，一键启动多模型 API 服务；
+ 内置显存监控，自动清理显存泄漏；

+ **简化点**：只需配置模型路径，无需写底层显存管理代码。

### 2. LLMDeploy（LLM 多模型部署脚手架）

+ **项目地址**：[https://github.com/InternLM/LLMDeploy](https://github.com/InternLM/LLMDeploy)

+ **核心能力**：

+ 专为大模型设计，支持 `vLLM`/`TensorRT-LLM` 显存优化；
+ 多模型动态切换，卸载时自动释放显存；
+ 提供 `WebUI/API/命令行` 三种调用方式；

+ **简化点**：一行命令部署多 LLM 模型：

```bash
# 启动多模型服务（自动管理显存）
llmdeploy serve --model-list vicuna-7b,baichuan-7b --gpu-memory 4G
```

## 选型建议（按场景匹配）

这些项目均已封装好「显存动态分配、自动释放、多模型共享」的核心逻辑，你只需：

1. 配置模型路径 / 显存限制；
2. 启动服务 / 加载模型；
3. 关闭模型 / 服务时，项目自动释放显存（无需手动调用 `empty_cache()` 等）。

其中，`FastChat`（`LLM`）、`Triton`（企业级）、`modelscope-agent`（多模态）是最易上手的选择，建议优先尝试。
