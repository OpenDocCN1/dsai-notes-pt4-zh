
# 📘 大模型导出与量化实操手册

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/YwthdlouLoLsjgxXSKXc16bXnhb](https://zsyhjtnsa5.feishu.cn/docx/YwthdlouLoLsjgxXSKXc16bXnhb)

## —— 从 LoRA 到 GGUF（适配 Ollama/LM Studio）

## 0. 目标与流程

本手册旨在将微调后的模型（LoRA）与基础模型合并，并转换为轻量级的 GGUF 格式，以便在本地电脑（Windows/Mac）、手机或树莓派上流畅运行。

核心流程：

1. 合并导出：`LLaMA Factory`（Base + LoRA -> Safetensors）
1. 环境准备：编译 `llama.cpp` 工具链
1. 格式转换：`Safetensors` -> `GGUF` (`FP16`)
1. 模型量化：`FP16` -> `Q4_K_M` (4-bit)

---

## 1. 模型合并导出（LLaMA Factory）

操作区域：`LLaMA Factory` Web UI -> `Export`（导出）选项卡

为了得到一个独立的模型，我们需要把微调的补丁“焊”到原模型上。

### 1.1 参数配置

请严格按照以下设置填写，防止后续转换失败：

+ 检查点路径（`Checkpoint Path`）：选择你训练好的 LoRA 记录（如 `train_2026-xx-xx...`）。
+ 最大分块大小（`Max Shard Size`）：2GB 或 5GB 均可。
+ 导出量化等级（`Export Quantization Bit`）：【关键】必须选 `none`。
+ 解释：我们要导出原版精度的模型给 `llama.cpp` 处理，不要在这里提前量化。
+ 导出目录（`Export Dir`）：填入一个绝对路径，建议放在项目目录下以防权限问题。
+ 示例：`/home/chengzh/LLaMA-Factory/deepseek_merged`
+ 导出设备：`cuda` 或 `auto`。

### 1.2 执行导出

1. 点击“开始导出（`Start Export`）”。
1. 等待日志显示 `Model weights saved in ...`。
1. 验证：在终端运行 `ls -lh [导出目录]`。

成功标准：必须包含 `config.json`、`tokenizer.json` 以及约 3GB 大小的 `model.safetensors` 文件。

---

## 2. 准备量化工具（llama.cpp）

操作区域：终端（Terminal）

我们需要编译 `llama.cpp` 来获得转换和量化工具。

### 2.1 激活环境

复用 `LLaMA Factory` 的环境，无需安装额外的 PyTorch。

```
conda activate llama_factory
```

### 2.2 下载源码

```
cd ~
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
```

### 2.3 编译工具（CMake CPU 模式）

此模式最稳定，不需要折腾 CUDA 驱动，量化速度也很快。

```
# 1. 安装 cmake (如果未安装)
sudo apt update && sudo apt install -y cmake build-essential

# 2. 创建构建目录
mkdir build
cd build

# 3. 生成配置 (CPU模式，最稳妥)
cmake .. -DGGML_CUDA=OFF

# 4. 开始编译
cmake --build . --config Release -j 8
```

成功标志：编译进度到达 100%，且在 `build/bin/` 目录下可以看到 `llama-quantize` 等文件。

### 2.4 安装转换依赖

回到 `llama.cpp` 根目录安装 Python 依赖：

```
cd ~/llama.cpp
pip install -r requirements.txt
```

---

## 3. 格式转换（Safetensors -> GGUF）

操作区域：终端（`llama.cpp` 根目录）

这一步将 HuggingFace 格式转换为 `llama.cpp` 的通用格式（GGUF），此时模型仍是半精度（FP16/BF16），体积未减小。

执行命令：

```
# 格式：python convert_hf_to_gguf.py [第一步的导出目录]
python convert_hf_to_gguf.py /home/chengzh/LLaMA-Factory/deepseek_merged
```

关键动作：

命令执行完毕后，请务必查看最后一行输出，记录下生成的 GGUF 文件名。

+ 示例输出：`Wrote .../Deepseek_Merged-1.5B-BF16.gguf`

---

## 4. 模型量化（压缩为 4-bit）

操作区域：终端（`llama.cpp` 根目录）

这一步将模型体积压缩约 70%，性能损失极小。

执行命令：

请根据上一步生成的实际文件名修改下方命令的第一行路径。

```
# 格式：./build/bin/llama-quantize [输入路径] [输出路径] [量化方法]
./build/bin/llama-quantize \
  /home/chengzh/LLaMA-Factory/deepseek_merged/Deepseek_Merged-1.5B-BF16.gguf \
  /home/chengzh/LLaMA-Factory/deepseek_merged/deepseek-r1-1.5b-q4_k_m.gguf \
  Q4_K_M
```

+ 输出：你将获得一个名为 `deepseek-r1-1.5b-q4_k_m.gguf` 的文件，大小约 1.1 GB。

---

## 5. 最终测试与部署

### 5.1 命令行快速测试

在 Linux 终端里直接测试模型是否可用：

```
./build/bin/llama-cli \
  -m /home/chengzh/LLaMA-Factory/deepseek_merged/deepseek-r1-1.5b-q4_k_m.gguf \
  -p "User: 你好，你是谁？ Assistant:" \
  -n 128
```

### 5.2 部署到 Ollama（Windows/Mac）

这是最实用的步骤，将模型带出 Linux 环境。

1. 下载文件：将生成的 `deepseek-r1-1.5b-q4_k_m.gguf` 文件复制到你的 Windows/Mac 电脑上。
1. 创建 `Modelfile`：在同目录下新建一个文本文件，命名为 `Modelfile`（无后缀），内容如下：

```
FROM ./deepseek-r1-1.5b-q4_k_m.gguf
TEMPLATE """{{ if .System }}<|im_start|>system
{{ .System }}<|im_end|>
{{ end }}{{ if .Prompt }}<|im_start|>user
{{ .Prompt }}<|im_end|>
{{ end }}<|im_start|>assistant
"""
SYSTEM "你是一个乐于助人的AI助手。"
```

1. 导入 Ollama：打开终端/CMD，运行：

```
ollama create my-deepseek -f Modelfile
```

1. 运行：

```
ollama run my-deepseek
```

🎉 至此，你已成功完成了从微调训练到私有化部署的全过程！
