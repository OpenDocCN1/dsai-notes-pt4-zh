# 基于AutoDL，使用vLLM部署DeepSeek-R1-Distill-Qwen-7B大模型教学文档

> 来源：[https://gotoaiworld.feishu.cn/docx/L3twdSwbJoTqtIxfc4icoIs9nSd](https://gotoaiworld.feishu.cn/docx/L3twdSwbJoTqtIxfc4icoIs9nSd)

## **文档引言**

针对DeepSeek-R1系列的7B版本（DeepSeek-R1-Distill-Qwen-7B）大模型，基于AutoDL云服务器与vLLM推理框架实现高效部署。

- **实测环境**：RTX 4090 24G单卡（经vLLM优化后可流畅运行7B模型，需注意显存配置），生成阶段吞吐量达18-22 tokens/s；

- **核心优势**：无需复杂底层配置，提供完整操作命令、7B模型专属避坑方案及服务调用示例；

- **适用人群**：AI开发者、大模型爱好者，无过多推理框架经验也可跟随操作。

本文档依赖**AutoDL云服务**，其优势为：万卡规模RTX 40系显卡、无需排队、即开即用，支持容器/虚机双模式（虚机可安装Windows系统）。

## 一、环境准备（AutoDL服务器配置）

DeepSeek-R1-Distill-Qwen-7B模型（简称“7B模型”）对显存和存储需求高于1.5B版本，需针对性配置AutoDL实例。

### **1.1 实例配置详情**

在AutoDL控制台创建实例时，严格按以下参数选择，确保适配7B模型：

| 配置项 | 具体参数 | 说明 |
| --- | --- | --- |
| 机型 | RTX 4090 24G（单卡） | 7B模型（FP16精度）约占13-15GB显存，vLLM优化后24G显存可满足，需预留显存给KV缓存 |
| 基础镜像 | PyTorch/2.5.1/3.12(ubuntu22.04)/12.4 | 预装PyTorch 2.5.1、Python 3.12、CUDA 12.4，无需额外安装基础依赖 |
| CPU/内存 | 16核心 CPU + 120GB 内存 | 7B模型加载时需更多CPU计算与内存缓存，避免加载超时或卡顿 |
| 存储分工 | 系统盘（30GB）+ 数据盘（≥50GB SSD） | 系统盘存代码（关机不丢失），数据盘存7B模型（约14GB，路径：/root/autodl-tmp） |

### 1.2 无卡模式开机技巧（关键！）

初始化阶段用**无卡模式**开机（0.1元/小时），避免配置环境时浪费GPU资源，步骤如下：

1.  实例创建页面勾选“无卡模式”，启动实例；

1.  完成vLLM安装等配置后，执行重启命令激活GPU：

```
# 配置完成后必须执行，重启后GPU才会加载
shutdown -r now
```

1.  重启后在AutoDL控制台确认“GPU状态”为“运行中”，再进入模型部署环节。

## **二、vLLM安装与验证**

vLLM框架支持7B模型的高效推理（如动态批处理、KV缓存优化），需先完成安装与版本验证。

### **2.1 安装vLLM**

连接AutoDL实例（通过“JupyterLab”或“SSH”），执行以下命令安装最新版vLLM（耗时约8-12分钟，取决于网络）：

```
pip install vllm
```

- **可选优化**：若安装缓慢，可添加国内PyPI镜像加速：

```
pip install vllm -i https://pypi.tuna.tsinghua.edu.cn/simple
```

### **2.2 验证安装结果**

安装完成后，执行以下命令检查vLLM是否成功安装及版本（需支持7B模型推理）：

```
python -c &quot;import vllm; print(vllm.__version__)&quot;
```

- **成功标志**：输出 `0.9.0` 及以上版本号（如 `0.9.0.1`），无“ModuleNotFoundError”报错；

- **失败排查**：若提示CUDA版本不匹配，确认基础镜像为“CUDA 12.4”，重新创建实例选择正确镜像。

## **三、两种模型部署方式**

DeepSeek-R1-Distill-Qwen-7B模型支持“直接加载HuggingFace”和“本地部署”，需注意模型名称与路径的准确性。

### 方式1：直接加载HuggingFace模型（推荐，无需手动下载）

适用于网络通畅场景，vLLM会自动拉取7B模型并缓存至本地。

#### 步骤1：设置国内镜像源（解决连接超时）

国内访问HuggingFace易超时，先配置镜像源：

```
export HF_ENDPOINT=https://hf-mirror.com
```

#### 步骤2：启动vLLM服务（适配7B模型）

执行以下命令加载7B模型，默认启动8000端口服务：

```
vllm serve &quot;deepseek-ai/DeepSeek-R1-Distill-Qwen-7B&quot; \
  --cuda-memory-fraction 0.8  # 关键：限制显存利用率，避免7B模型OOM
```

- **参数说明**：`--cuda-memory-fraction 0.8` 表示使用80% GPU显存（约19.2GB），预留空间给KV缓存，适配7B模型；

- **服务启动标志**：终端输出 `INFO Engine 000: Loading model...` 且无“Out of Memory”报错，最终显示“Engine ready”，说明服务启动成功。

### 方式2：本地模型部署（适用于网络差，手动下载）

若直接拉取超时，先手动下载7B模型到数据盘，再从本地加载。

#### **步骤1：下载7B模型到数据盘**

切换至数据盘路径（/root/autodl-tmp），执行git clone下载模型（约14GB，耗时取决于网络）：

```
# 切换到数据盘（避免系统盘空间不足）
cd /root/autodl-tmp
# 克隆7B模型仓库（模型名称需完整匹配）
git clone https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-7B
```

- **下载验证**：执行 `ls ./DeepSeek-R1-Distill-Qwen-7B`，若显示 `config.json`、`model.safetensors` 等文件，说明下载成功。

#### 步骤2：启动本地模型服务（自定义参数）

从本地路径加载模型，配置7B模型专属参数：

```
vllm serve ./DeepSeek-R1-Distill-Qwen-7B \
  --served-model-name deepseek-r1-qwen-7b \  # API调用时的模型名（自定义，需记录）
  --max-model-len 4096 \  # 7B模型推荐上下文长度，设8192需降低显存利用率至0.75
  --cuda-memory-fraction 0.8  # 适配24G显存，避免OOM
```

- **注意事项**：若需设置 `--max-model-len 8192`（长文本处理），需将 `--cuda-memory-fraction` 调整为 `0.75`（约18GB），平衡上下文长度与显存占用。

## **四、服务调用测试**

模型服务启动后，通过curl命令发送HTTP请求，验证7B模型的响应能力。

### **4.1 执行curl请求**

打开新终端窗口（不关闭服务窗口），执行以下命令（model字段需与--served-model-name一致）：

```
curl -X POST &quot;http://localhost:8000/v1/chat/completions&quot; \
  -H &quot;Content-Type: application/json&quot; \
  -d &#x27;{
    &quot;model&quot;: &quot;deepseek-r1-qwen-7b&quot;,  # 与方式2的--served-model-name一致；方式1可省略或填模型路径
    &quot;messages&quot;: [
      {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Explain the differences between artificial intelligence and machine learning in simple terms.&quot;}
    ],
    &quot;temperature&quot;: 0.7  # 控制生成文本的随机性，7B模型推荐0.5-0.8
  }&#x27;
```

### **4.2 成功响应示例**

若服务正常，会返回包含7B模型推理结果的JSON响应：

```
{
  &quot;id&quot;:&quot;chatcmp1-8f3d2c7a91b44e82a6c5d1234e5f6a7b&quot;,
  &quot;choices&quot;:[
    {
      &quot;message&quot;:{
        &quot;role&quot;:&quot;assistant&quot;,
        &quot;content&quot;:&quot;To put it simply, artificial intelligence (AI) is like a big umbrella—it covers all technologies that let machines do things that seem &#x27;smart,&#x27; like understanding language, recognizing images, or making decisions. Machine learning (ML) is a *tool* under that umbrella: it’s how many AI systems learn to do those smart things without being programmed step-by-step. For example, if you want an AI to tell cats from dogs, ML lets it learn from thousands of cat/dog photos instead of you writing rules for every possible cat/dog feature. So AI is the goal, ML is a common way to reach it.&quot;
      },
      &quot;finish_reason&quot;:&quot;stop&quot;,
      &quot;index&quot;:0
    }
  ],
  &quot;object&quot;:&quot;chat.completion&quot;,
  &quot;created&quot;:1721900000,
  &quot;model&quot;:&quot;deepseek-r1-qwen-7b&quot;
}
```

- **失败排查**：若返回“Model not found”，确认`"model"`字段与`--served-model-name`完全一致；若返回“GPU out of memory”，降低`--cuda-memory-fraction`至0.7-0.75，重启服务。

## 五、避坑指南（7B模型专属问题+通用问题）

7B模型对显存和路径更敏感，以下为高频问题及解决方案：

### 1. 显存不足（`CUDA out of memory`）

- **原因**：7B模型显存需求高于1.5B，未限制显存利用率或上下文长度过长；

- **解决方案**：

1.  启动服务时添加 --cuda-memory-fraction 0.7-0.8（RTX 4090 24G专属）；

1.  若需长文本，将 --max-model-len 从8192降至4096；

1.  重启服务，确保参数生效。

### **2**. 连接HuggingFace超时（`Connection to huggingface.co timed out`）

- **原因**：国内网络访问限制；

- **解决方案**：重新执行镜像源命令，再拉取模型：

```
export HF_ENDPOINT=https://hf-mirror.com
```

### **3**. 模型路径错误（`FileNotFoundError: No such file or directory`）

- **原因**：本地部署时路径拼写错误（区分大小写，7B模型名称含“7B”）；

- **错误示例**：漏写“7B”或小写路径：

```
vllm serve ./DeepSeek-R1-Distill-Qwen  # 错误：缺7B
vllm serve ./deepseek-r1-distill-qwen-7b  # 错误：全小写
```

- **正确示例**：严格匹配下载的文件夹名：

```
vllm serve ./DeepSeek-R1-Distill-Qwen-7B  # 正确
```

### **4. 磁盘空间不足（`No sp**ace left on device`）

- **原因**：7B模型约14GB，若下载到系统盘（仅30GB）易满；

- **解决方案**：

+   确保模型下载路径为数据盘：/root/autodl-tmp；

+   执行 df -h 查看磁盘占用，若数据盘不足，在AutoDL控制台扩容数据盘。

### 5. GPU未加载（`GPU device not found`）

- **原因**：无卡模式开机后未重启，GPU未激活；

- **解决方案**：执行重启命令，重启后确认GPU状态：

```
shutdown -r now
```

## 六、进阶配置：7B模型专属vLLM参数

针对7B模型的显存与推理需求，优化核心参数，平衡性能与稳定性：

| 参数 | 说明 | 7B模型推荐值 | 适用场景 |
| --- | --- | --- | --- |
| --cuda-memory-fraction | GPU显存利用率（0-1） | 0.7-0.8 | RTX 4090 24G专属，避免7B模型OOM |
| --max-model-len | 最大上下文长度（tokens） | 4096（默认）；8192（需降显存利用率至0.75） | 4096适用于日常对话，8192适用于文档总结 |
| --max-input-length | 单次输入token限制 | 2048 | 限制用户输入长度，避免输入过长导致显存溢出 |
| --served-model-name | API暴露的模型名（自定义） | deepseek-r1-qwen-7b | 多模型部署时区分不同模型（如同时部署7B和13B） |
| --tensor-parallel-size | 张量并行数（多卡时用） | 1 | 单卡RTX 4090固定设1，多卡时按卡数调整 |

### **查看完整参数**

执行以下命令获取vLLM所有支持的参数及说明：

```
vllm serve --help
```

## 七、备用方案：Docker部署（无需手动配置环境）

若不想手动安装依赖，可通过Docker快速部署7B模型，步骤如下：

### 7.1 执行Docker命令（适配7B模型）

确保实例已安装Docker（AutoDL默认预装），执行以下命令：

```
docker run --gpus all -p 8000:8000 \
  -v /root/autodl-tmp/model_cache:/root/.cache/huggingface \  # 挂载数据盘缓存，避免重复下载7B模型
  vllm/vllm-openai:latest \  # vLLM官方镜像，支持OpenAI兼容API
  --model deepseek-ai/DeepSeek-R1-Distill-Qwen-7B \  # 7B模型名称
  --cuda-memory-fraction 0.8  # 限制显存利用率，适配24G显卡
```

### **7.2 关键说明**

+   -v /root/autodl-tmp/model_cache:/root/.cache/huggingface：将7B模型缓存到数据盘，下次部署无需重新下载（约14GB）；

+   若需长文本推理，添加 --max-model-len 4096 或 8192（需调整显存利用率）。

## **八、资源监控与部署成功验证**

部署7B模型后，需关注显存占用与推理性能，确保服务稳定。

### **8.1 资源监控方式**

通过AutoDL控制台实时监控：

1.  进入实例详情页，查看“GPU监控”面板；

1.  核心指标（RTX 4090 24G运行7B模型）：

+   GPU利用率：推理时约60%-85%（生成文本时峰值更高）；

+   显存使用率：约12-18GB（取决于--cuda-memory-fraction和上下文长度）；

+   内存使用率：约30%-50%（7B模型加载时内存需求较高）。

### **8.2 部署成功关键指标**

服务启动后，终端输出以下日志，说明7B模型部署成功：

```
INFO Engine 000: 
  Avg prompt throughput: 0.8 tokens/s  # 输入处理吞吐量（7B模型略低于1.5B）
  Avg generation throughput: 18-22 tokens/s  # 生成吞吐量（RTX 4090实测，7B模型约为1.5B的1/2）
  GPU KV cache usage: 0.0%  # 无请求时为0，有请求时随上下文增长
```

## **九、总结与问题交流**

本文档针对DeepSeek-R1-Distill-Qwen-7B模型的特性，优化了显存配置、路径验证等关键环节，核心要点：

1.  7B模型需通过 --cuda-memory-fraction 0.7-0.8 适配RTX 4090 24G显存，避免OOM；

1.  模型名称需完整包含“7B”，本地部署路径严格区分大小写；

1.  长文本推理（--max-model-len 8192）需降低显存利用率至0.75，平衡性能与稳定性。

按上述步骤操作，即可在RTX 4090 24G环境下流畅运行DeepSeek-R1-Distill-Qwen-7B模型，实现高效推理与API调用。若遇到问题，可参考“避坑指南”或通过技术社区交流。