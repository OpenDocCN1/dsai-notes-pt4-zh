
# Torch GPU安装 & CUDA配置指南

> 来源：[https://datawhaler.feishu.cn/docx/Ky9gdyptloGq3qxi5rYc6NV9nlh](https://datawhaler.feishu.cn/docx/Ky9gdyptloGq3qxi5rYc6NV9nlh)

在这篇文章里，我将手把手教你如何从 0 配置 CUDA 环境，使用 GPU 进行学习加速计算。

## 什么是CUDA

Compute Unified Device Architecture 是 NVIDIA 提供的一种并行计算平台和编程模型，它允许开发者利用 NVIDIA 的图形处理单元（GPU）进行通用计算（GPGPU，General-Purpose computing on Graphics Processing Units）。简而言之，CUDA 使得程序员可以使用 NVIDIA GPU 的强大并行计算能力来加速计算密集型任务，而不仅仅是传统的图形处理。

### CUDA 的关键概念和特点

1. **并行计算**：

   CUDA 使得 GPU 可以执行大量的并行计算任务，特别适合处理需要大量数据处理和计算的任务，比如深度学习训练、科学计算、图像处理等。

   通过将计算任务分成多个并行的子任务，CUDA 充分利用了 GPU 大量的核心（通常是成千上万的处理单元）。

2. **GPU 加速**：

   在传统的 CPU 计算中，只有几个核心可以同时进行处理，而 GPU 拥有数千个小的计算核心，能够同时处理大量数据，极大提高了计算效率。

   通过 CUDA，开发者可以将计算密集型的任务从 CPU 转移到 GPU，从而实现大幅度的性能提升。

3. **CUDA 编程模型**：

   CUDA 提供了一个 C/C++ 的扩展，使得开发者可以利用 GPU 进行并行计算。

   开发者可以编写 CUDA 程序来执行特定的任务，使用 GPU 来加速计算过程。

   CUDA 提供了丰富的 API 和工具，方便开发者进行并行编程。

4. **深度学习中的应用**：

   在深度学习领域，CUDA 已成为训练神经网络的核心技术之一。像 PyTorch、TensorFlow 等深度学习框架都使用 CUDA 来加速模型的训练。

   通过 CUDA，GPU 可以高效地执行矩阵运算、卷积操作等，极大加快深度学习模型的训练速度。

5. **广泛支持**：

   CUDA 支持 NVIDIA 的许多显卡（包括 GeForce、Quadro、Tesla 等），而且不断更新和优化，以支持新一代硬件。

   除了 GPU 计算，CUDA 还扩展到许多其他领域，包括物理模拟、数据分析、机器学习等。

### 为什么使用 CUDA？

+   **性能提升**：GPU 在并行计算方面的能力远超 CPU，尤其是在处理大规模矩阵、图像等数据时，性能提升显著。

+   **加速深度学习模型**：深度学习模型通常需要进行大量的矩阵乘法和卷积计算，GPU 可以有效加速这些计算过程，减少训练时间。

+   **并行计算优势**：CUDA 让开发者能够高效地编写并行计算代码，利用 GPU 的强大计算能力处理复杂任务。

### 如何使用 CUDA？

+   你可以通过安装 NVIDIA 的 CUDA 工具包来配置你的系统，确保 GPU 驱动和 CUDA 环境正确运行。

+   在深度学习框架（如 PyTorch 或 TensorFlow）中，只需要将模型和数据移到 GPU 上，CUDA 会自动加速计算。

在配置 CUDA 环境时，需要下载并安装两个核心组件：

1.  **CUDA Toolkit**：这是 CUDA 的基础工具包，包含了开发 GPU 加速程序所需的所有基本工具和库。

2.  **cuDNN**：这是一个深度学习专用的库，用于加速神经网络的训练和推理，它是 CUDA 环境下深度学习框架的必要依赖。

## 如何彻底卸载CUDA

### 卸载 CUDA

打开 `控制面板`

![](img/21b1635d75537321c7e913984019acac.png)

进入 `程序和功能`

![](img/0d6da7bee7c21a6d9d46038fcbe112ad.png)

在程序列表中找到 NVIDIA CUDA Toolkit 9.0 (或相似名称)。

双击它或选择并点击卸载，然后按照提示完成卸载过程。

### 删除CUDA

在开始菜单栏通过 `Uninstall` 来卸载

#### 卸载 cuDNN

cuDNN 通常不提供标准的卸载程序，因为它由多个库文件和一个包含头文件的目录组成。要卸载 cuDNN，只需删除相关文件即可：

1.  打开你的 CUDA Toolkit 安装目录（通常路径为 `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0`）。

2.  进入 `bin`、`include` 和 `lib` 文件夹。

3.  删除所有与 cuDNN 相关的文件，文件名通常包含 `cudnn` 或类似字符。

#### 清理环境变量

为了确保彻底卸载 CUDA 和 cuDNN，还需要清理环境变量：

1.  右击 `计算机` 或 `此电脑`，选择 `属性`。

2.  点击 `高级系统设置`。

3.  在 `系统属性` 窗口中，点击 `环境变量`。

4.  在 `系统变量` 中，查找与 CUDA/cuDNN 相关的变量，如 `CUDA_PATH`、`CUDA_HOME`，以及可能添加到 `PATH` 变量中的 CUDA 路径。

5.  删除或编辑这些变量，确保所有指向 CUDA 9.0 的路径都已移除。

![](img/5325c6dd0f65833af86903805522d514.png)

## CUDA版本的选择

确定显卡型号之后，可以在这个地址查询显卡对应的算力：

国外的源：

https://en.wikipedia.org/wiki/CUDA

在命令行输入 `nvidia-smi` 确定GPU驱动版本

```
nvidia-smi
```

因为CUDA是通过显卡驱动来使用显卡的，因此CUDA的版本需要低于显卡驱动的版本，因此CUDA SDK的值必须小于显卡驱动的版本。

## CUDA的下载以及安装

1.  确定自己电脑设备哪个版本cuda

打开 NVIDIA control panel

![](img/0213a8416f854745085cfda4f6a13097.png)

点击左下角：系统设置

![](img/f16aa7b008c41772318619699edf956d.png)

点击：组件

低于这个版本的就可以：我的就是低于12.6.65即可

![](img/31e2a53ff41df093f8354a6bd3a1a94a.png)

当然，最重要的是，选择一个合适的版本，如果不合适则会面临安装之后仍然无法使用。

## cuda的下载11.8

CUDA Toolkit Archive

![](img/7318946159e691207bd9630014414d07.png)

![](img/ebf105354684aba1fc29d14dbf569434.png)

![](img/92dea322cdb51afd2e537ae77d283d76.png)

点击Download即可

## cuDNN下载安装8.9.4

NVIDIA cuDNN Archive

![](img/d70bdc78de858b0524946ef8bd14f140.png)

![](img/eada17f28f39fa473be61f652e18b615.png)

![](img/3afe4532cadc9c963cd474634d407f4c.png)

## 安装CUDA

双击exe文件

选择地址（我就不放在默认的C盘了，如果有空间，推荐默认地址

![](img/77604a5cd7ba54b6e6f4f457f245188e.png)

选择自定义：

![](img/95fa6e12263887af813940ad627598a1.png)

不要选Visual Studio Integration

![](img/1258d03bb9125a28dcab2ae87a5e9896.png)

下一步，默认地址，安装

将下列路径添加到环境变量 `PATH` 中：

`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.8\libnvvp`

`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.8\bin`

`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.8\include`

`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.8\lib`

### 测试是否安装成功

打开命令行，win+R 输入 `cmd` 回车

```
nvcc -V
```

![](img/eac8ee39f438669800c39b3cb7d41761.png)

### 安装cuDNN

解压下载的zip文件 `cudnn-windows-x86_64-8.9.4.25_cuda11-archive.zip`

点击进入

![](img/9b5520175a3818590b880083becc23af.png)

打开命令行输入：

```
set cuda
```

![](img/eb678b81f812d123cc7aa1b8a2cb0e77.png)

将这三个文件夹复制到，并覆盖：

`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.8`

![](img/59341f833ff93de19f549e0e6ced3846.png)

### 验证安装是否成功

cd到 `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.8\extras\demo_suite`，

然后执行下面两条指令：

```
bandwidthTest.exe
```

```
deviceQuery.exe
```

![](img/5633ea5ef0c471181e052f402e79a47c.png)

现在安装成功。

## 安装PyTorch

### 安装torch

进入

Previous PyTorch Versions

找到2.0.0

![](img/557fe03186512037a03f3a051fcbc814.png)

使用pip进行安装

```
pip install torch==2.0.0+cu118 torchvision==0.15.1+cu118 torchaudio==2.0.1 --index-url <https://download.pytorch.org/whl/cu118>
```

安装轮子解决下载慢：

download.pytorch.org

找到适配自己的内容，cuda11.8.torch2.0.0，python版本

![](img/0c77f1271446cbc0325a60b7cee11e43.png)

pip install "`A轮子文件名`"

如果下载太慢可以加钉钉下载

由于我想安装到虚拟环境里面，所以我需要先激活环境，在环境里安装

```
conda activate ailearn
pip install torch-2.0.0+cu118-cp311-cp311-win_amd64.whl
```

![](img/9877759c17b7e76930a94c3586224acd.png)

+   安装过程中，pip 发现你系统中已有 `torch` 2.4.1，所以它卸载了旧版本，并成功安装了 `torch` 2.0.0+cu118。

+   同时，`pandasai` 2.4.2 依赖 `torch==2.4.1`，但你安装了 `torch` 2.0.0+cu118，这可能导致版本冲突。

测试一下是否安装成功：

```
python -c "import torch; print(torch.version)"
```

![](img/52f45e1e7d5fd67c1cd3c21587aaf8aa.png)

在检查CUDA是否可用

```
python -c "import torch; print(torch.cuda.is_available())"
```

![](img/f3b64a45e6f7a3e5e31c78cdefa83b08.png)

### 安装torchvision

```
pip install torchvision==0.15.1+cu118 --extra-index-url <https://download.pytorch.org/whl/cu118>
```

### 为什么需要安装 torchvision？

+   **计算机视觉任务**：如果你从事计算机视觉相关的工作，比如图像分类、目标检测、分割等，`torchvision` 是必不可少的工具，它提供了图像处理和模型构建所需的功能。

+   **数据集加载**：`torchvision` 提供的内置数据集，可以方便地让你快速加载经典的计算机视觉数据集，节省时间和精力。

+   **预训练模型**：通过使用 `torchvision` 提供的预训练模型，你可以避免从头开始训练一个模型，从而节省计算资源。

### 安装torchaudio

```
pip install torchaudio==2.0.0+cu118 --extra-index-url <https://download.pytorch.org/whl/cu118>
```

### torchaudio 提供的主要功能

1.  **音频数据加载和处理**：

    你可以用 `torchaudio` 方便地加载音频文件（如 WAV、MP3、FLAC 等格式）并将其转换为 PyTorch 张量。

    提供了常见的音频转换功能，如傅里叶变换、滤波、归一化等。

2.  **特征提取**：

    `torchaudio` 支持从音频数据中提取特征（例如：MFCC、Mel spectrogram 等），这些特征常用于语音识别、音乐分析等任务。

3.  **数据集和数据加载**：

    内置了多个音频数据集，方便用来训练和评估音频处理模型。

4.  **与 PyTorch 集成**：

    与 `torch` 紧密集成，可以直接在 PyTorch 模型中使用音频数据，简化了深度学习任务中的音频处理工作。

现在完成了安装。一起来操作测试一下

打开vscode，[新建一个`test.py`](http://xn--test-k84fui098hxhl.py)

输入以下内容：

```
import torch
# 检查是否有可用的 CUDA 设备
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
print(f"Using device: {device}")
# 创建两个随机的 3x3 张量
tensor1 = torch.rand((3, 3))
tensor2 = torch.rand((3, 3))
# 将张量移动到 GPU（如果 CUDA 可用）
tensor1 = tensor1.to(device)
tensor2 = tensor2.to(device)
# 执行矩阵加法
result = tensor1 + tensor2
print("Tensor 1:")
print(tensor1)
print("Tensor 2:")
print(tensor2)
print("Result (Tensor 1 + Tensor 2):")
print(result)
# 将结果移回 CPU
result_cpu = result.to('cpu')
print("Result on CPU:")
print(result_cpu)
```

输出示例：

![](img/52707719b9a603e78d9987de9b790de7.png)

好啦~配置完成！现在你已经拥有了强大的 CUDA 加速支持，无论是模型训练还是数据处理，都能更高效。让我们一起在学习 AI 的道路上不断前进啦！
