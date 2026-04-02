# LLaMA-Factory 安装手册（AutoDL GPU实例）

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/FOTPd91yIoELFjxjxr3cPU5pn3e](https://zsyhjtnsa5.feishu.cn/docx/FOTPd91yIoELFjxjxr3cPU5pn3e)

## **一、实验环境与前提**

+   **平台**：AutoDL

+   **系统镜像**：Ubuntu 20.04/22.04 with Nvidia Driver (推荐选择预装PyTorch的镜像以简化CUDA配置)

+   **硬件**：NVIDIA GPU（如 RTX 3090, 已测试通过）

+   **目标**：安装 LLaMA-Factory 并启动其 WebUI 进行模型微调。

## **二、安装流程总览**

1.  **步骤零：连接实例** - 通过SSH或AutoDL控制台登录你的Ubuntu实例。

1.  步骤一：安装并配置 Miniconda - 创建独立的Python环境。

1.  步骤二：创建并激活 Conda 环境 - 为LLaMA-Factory创建专用环境，必须使用Python 3.11+。

1.  步骤三：安装 PyTorch (含CUDA支持) - 安装与实例CUDA版本匹配的PyTorch。

1.  **步骤四：验证 GPU 可用性** - 确保PyTorch能正确识别和使用GPU。

1.  步骤五：安装 LLaMA-Factory - 克隆项目并安装核心依赖。

1.  步骤六：启动 WebUI 及端口映射 - 在AutoDL平台启动服务并配置访问。

---

## **三、详细操作步骤**

### **步骤零：连接实例**

1.  登录AutoDL控制台，启动或租用一台Ubuntu GPU实例。

1.  通过“终端”连接到实例。

### 步骤一：安装并配置 Miniconda

如果实例未预装Conda，请手动安装。

```
# 1. 下载 Miniconda 安装脚本 (使用清华镜像)
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-py311_24.5.0-0-Linux-x86_64.sh -O ~/miniconda.sh

# 2. 执行安装 (静默安装到家目录)
bash ~/miniconda.sh -b -p ~/miniconda3

# 3. 将 Conda 添加到系统 PATH
echo &#x27;export PATH=&quot;~/miniconda3/bin:$PATH&quot;&#x27; &gt;&gt; ~/.bashrc
source ~/.bashrc

# 4. 初始化 Conda (关键步骤！)
conda init bash
# 初始化后，必须关闭并重新打开当前终端，或运行以下命令使初始化生效：
source ~/.bashrc
# 此时命令提示符前应出现 (base) 字样。
```

### 步骤二：创建并激活 Conda 环境

**重要**：LLaMA-Factory 要求 Python >= 3.11，请务必创建对应版本的环境。

```
# 1. 创建名为 llamafactory 的Python 3.11环境
conda create -n llamafactory python=3.11 -y

# 2. 激活环境
conda activate llamafactory
# 激活后，提示符应变为 (llamafactory)
```

### 步骤三：安装 PyTorch (含CUDA支持)

安装与实例CUDA版本匹配的PyTorch。首先查询CUDA版本：

```
nvcc --version
# 查看输出中的 release x.x，例如 release 12.1
```

根据查询到的CUD[A版本（例如 12](https://pytorch.org/get-started/locally/).1），访问 PyTorch官网 获取对应安装命令，或直接使用类似以下命令：

```
# 示例：为 CUDA 12.1 安装 PyTorch
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121 -i https://pypi.tuna.tsinghua.edu.cn/simple
```

### **步骤四：验证 GPU 可用性**

运行以下命令，确保PyTorch可以调用GPU。

```
python -c &quot;import torch; print(f&#x27;PyTorch版本：{torch.__version__}&#x27;); print(f&#x27;CUDA是否可用：{torch.cuda.is_available()}&#x27;); print(f&#x27;GPU设备：{torch.cuda.get_device_name(0)}&#x27;)&quot;
```

**预期输出**：应显示PyTorch版本、CUDA是否可用：True 以及你的GPU型号（如NVIDIA GeForce RTX 3090）。

### 步骤五：安装 LLaMA-Factory

启用AutoDL学术加速

```
echo &quot;nameserver 100.90.90.90&quot; | sudo tee /etc/resolv.conf

echo &quot;nameserver 100.90.90.100&quot; | sudo tee /etc/resolv.conf
```

克隆项目

```

git clone https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory
```

安装 LLaMA-Factory 核心包（使用国内镜像加速）

```
pip install -e &quot;.[torch,metrics]&quot; -i https://pypi.tuna.tsinghua.edu.cn/simple
```

可选 如果后续提示缺少 datasets 包，可单独安装

```
pip install datasets -i https://pypi.tuna.tsinghua.edu.cn/simple
```

验证安装

```
llamafactory-cli version
# 成功后会显示欢迎信息和版本号（如 0.9.5.dev0）
```

```
# 1. 启用AutoDL学术加速（可选，可加速GitHub克隆）

 echo &quot;nameserver 100.90.90.90&quot; | sudo tee /etc/resolv.conf

echo &quot;nameserver 100.90.90.100&quot; | sudo tee /etc/resolv.conf

# 2. 进入数据盘并克隆项目
cd /root/autodl-tmp
git clone https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory

# 3. 安装 LLaMA-Factory 核心包（使用国内镜像加速）
pip install -e &quot;.[torch,metrics]&quot; -i https://pypi.tuna.tsinghua.edu.cn/simple

# 4. （可选）如果后续提示缺少 datasets 包，可单独安装
pip install datasets -i https://pypi.tuna.tsinghua.edu.cn/simple

# 5. 验证安装
llamafactory-cli version
# 成功后会显示欢迎信息和版本号（如 0.9.5.dev0）
```

### 步骤六：启动 WebUI 及端口映射

这是能在浏览器中访问的关键。

1.  **启动 WebUI 服务**：

```
llamafactory-cli webui
```

服务将启动在默认的 http://localhost:7860。

1.  **配置 AutoDL 端口映射**：

+   **在 AutoD**L 实**例控制台页面**，找到 “自定义服务” 或 “端口映射” 选项。

+   添加一条新规则：

+   **本地端口** / **实例端口**：7860

+   **协议**：HTTP

+   **保存后，**AutoDL 会生成一个独立的访问链接（格式如 https://xxxx-7860.apps.autoai.com）。

1.  **访问 WebUI**：

+   点击上一步生成的访问链接，即可打开 LLaMA-Factory 的图形界面。

## **四、常见问题与解决 (FAQ)**

| 问题 | 原因与解决方案 |
| --- | --- |
| conda: command not found | **Con**da未安装或未加入PATH。请严格按照步骤一操作，并确保运行了 source ~/.bashrc。 |
| **Python版本错误 (****n**ot in ‘&gt;=3.11.0‘) | **创建环境时未指定**Python 3.1**1。请激活ba**se环境后，删除旧环境(conda remove -n llamafactory --all)并重新执行步骤二。 |
| CUDA is not available | PyTorch与系统CUDA版本不匹配。请**根据 nvcc** --version 查询的结果，重新执行步骤三安装对应版本。 |
| **WebUI本地链接无法访问** | 这是正常现象。必须通过AutoDL控制台的“自定义服务”功能进行端口映射（步骤六），才能从外网访问。 |
| **安装依赖时速度慢或超时** | 始终使用 -i https://pypi.tuna.tsinghua.edu.cn/simple 参数指定清华镜像源加速下载。 |

## **五、快速开始微调**

安装完成后，在WebUI中你可以：

1.  在 **“模型”** 栏输入 Hugging Face 模型ID（如 Qwen/Qwen2.5-1.5B-Instruct）。

1.  在 **“数据集”** 栏选择内置示例（如 identity）进行测试。

1.  调整训练参数后，点击 **“开始”** 按钮即可启动微调。

。如果在实际操作中遇到任何新的问题，可以随时再来问我。

附录

```
# 步骤1：创建并激活Python 3.11环境
conda create -n llamafactory python=3.11 -y
conda activate llamafactory

# 步骤2：预先安装一组兼容的核心依赖（关键！）
pip install modelscope==1.14.0 datasets==2.18.0 transformers==4.45.0 peft==0.12.0

# 步骤3：安装PyTorch（与CUDA版本匹配）
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

# 步骤4：克隆LLaMA-Factory项目
cd /workspace
git clone https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory

# 步骤5：以“无依赖”模式安装LLaMA-Factory包本身
pip install -e . --no-deps

# 步骤6：验证安装和数据集
export PYTHONPATH=/workspace/LLaMA-Factory/src:$PYTHONPATH
python -c &quot;import llamafactory; print(&#x27;导入成功&#x27;)&quot;
# 数据集列表可通过读取 data/dataset_info.json 验证
```