
# ComfyUI

> 来源：[https://agispace.feishu.cn/docx/Lydrd2JRyov4AdxnhdEc6zg4nPh](https://agispace.feishu.cn/docx/Lydrd2JRyov4AdxnhdEc6zg4nPh)

### [ComfyUI 示例](https://comfyanonymous.github.io/ComfyUI_examples/)

![](img/44263a18ab2e28daba7426716dd35f37.png)

### [安装 ComfyUI](https://github.com/comfyanonymous/ComfyUI?tab=readme-ov-file#installing)

## 特征

- 节点/图形/流程图界面用于实验和创建复杂的稳定扩散工作流程，而无需编写任何代码。
- [完全支持](https://comfyanonymous.github.io/ComfyUI_examples/sdxl/) [SD1.x](https://comfyanonymous.github.io/ComfyUI_examples/video/)、[SD2.x](https://comfyanonymous.github.io/ComfyUI_examples/stable_cascade/)、[SDXL](https://comfyanonymous.github.io/ComfyUI_examples/sd3/)、[稳定视频扩散](https://comfyanonymous.github.io/ComfyUI_examples/audio/)、稳定级联、SD3和稳定音频
- 异步队列系统
- 许多优化：仅重新执行执行之间发生变化的工作流部分。
- 智能内存管理：可自动在低至 1GB vram 的 GPU 上运行模型。
- 即使您没有 GPU 也可以工作：（`--cpu`慢）
- 可以加载 `ckpt`、`safetensors` 和 `diffusers` 模型/检查点。独立的 VAE 和 CLIP 模型。
- 嵌入/文本反转
- Loras（常规、`locon` 和 `loha`）
- [超网络](https://comfyanonymous.github.io/ComfyUI_examples/hypernetworks/)
- 从生成的 PNG、WebP 和 FLAC 文件加载完整的工作流程（带有种子）。
- 将工作流保存/加载为 Json 文件。
- [节点接口可用于创建复杂的](https://comfyanonymous.github.io/ComfyUI_examples/2_pass_txt2img/)工作流程，例如Hires Fix 工作流程或更高级的工作流程。
- [区域构成](https://comfyanonymous.github.io/ComfyUI_examples/area_composition/)
- 使用常规模型和修复模型[进行修复。](https://comfyanonymous.github.io/ComfyUI_examples/inpaint/)
- ControlNet 和 T2I 适配器
- [高档模型（ESRGAN、ESRGAN变体](https://comfyanonymous.github.io/ComfyUI_examples/upscale_models/)、SwinIR、Swin2SR等...）
- [unCLIP 模型](https://comfyanonymous.github.io/ComfyUI_examples/unclip/)
- [格列本](https://comfyanonymous.github.io/ComfyUI_examples/gligen/)
- [模型合并](https://comfyanonymous.github.io/ComfyUI_examples/model_merging/)
- [LCM模型和Loras](https://comfyanonymous.github.io/ComfyUI_examples/lcm/)
- [SDXL Turbo](https://comfyanonymous.github.io/ComfyUI_examples/sdturbo/)
- [灵光流](https://comfyanonymous.github.io/ComfyUI_examples/aura_flow/)
- [使用TAESD](https://github.com/comfyanonymous/ComfyUI?tab=readme-ov-file#how-to-show-high-quality-previews)进行潜在预览
- 启动非常快。
- 完全离线工作：永远不会下载任何东西。
- [配置文件](https://github.com/comfyanonymous/ComfyUI/blob/master/extra_model_paths.yaml.example)来设置模型的搜索路径。

[工作流程示例可以在示例页面](https://comfyanonymous.github.io/ComfyUI_examples/)上找到

## 快捷方式

对于 macOS 用户，也可以将 Ctrl 替换为 Cmd

## 安装

### 视窗

[在发布页面](https://github.com/comfyanonymous/ComfyUI/releases)上有一个适用于 Windows 的便携式独立版本，可以在 Nvidia GPU 上运行，或者仅在您的 CPU 上运行。

#### [直接链接下载](https://github.com/comfyanonymous/ComfyUI/releases/download/latest/ComfyUI_windows_portable_nvidia_cu121_or_cpu.7z)

只需下载、使用[7-Zip](https://7-zip.org/)解压并运行即可。确保将稳定扩散检查点/模型（巨大的 `ckpt`/`safetensors` 文件）放入：`ComfyUI\models\checkpoints`

如果解压时遇到问题，请右键单击文件 -> 属性 -> 解除阻止

#### 如何在另一个 UI 和 ComfyUI 之间共享模型？

请参阅[配置文件](https://github.com/comfyanonymous/ComfyUI/blob/master/extra_model_paths.yaml.example)以设置模型的搜索路径。在独立的 Windows 版本中，您可以在 ComfyUI 目录中找到此文件。将此文件重命名为 `extra_model_paths.yaml` 并使用您喜欢的文本编辑器进行编辑。

### Jupyter 笔记本

要在 paperspace、kaggle 或 colab 等服务上运行它，您可以使用我的Jupyter Notebook

### 手动安装（Windows、Linux）

Git 克隆这个 repo。

将您的 SD 检查点（巨大的 `ckpt`/`safetensors` 文件）放入：`models/checkpoints`

将您的 VAE 放入：`models/vae`

#### AMD GPU（仅限 Linux）

如果尚未安装，AMD 用户可以使用 `pip` 安装 rocm 和 pytorch，这是安装稳定版本的命令：

```
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/rocm6.0
```

这是使用 ROCm 6.0 进行夜间安装的命令，可能会有一些性能改进：

```
pip install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/rocm6.1
```

#### 英伟达

Nvidia 用户应使用以下命令安装稳定的 pytorch：

```
pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu121
```

这是安装 pytorch nightly 的命令，可能会提高性能：

```
pip install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu124
```

##### 故障排除

如果收到“Torch 未使用 CUDA 进行编译”错误，请使用以下命令卸载 Torch：

```
pip uninstall torch
```

并使用上述命令再次安装。

#### 依赖项

通过在 ComfyUI 文件夹内打开终端来安装依赖项：

```
pip install -r requirements.txt
```

此后，您应该已经安装好了所有内容并可以继续运行 ComfyUI。

#### 其他的：

##### 英特尔 GPU

Intel GPU 支持适用于所有 Intel Pytorch 扩展 (IPEX) 支持的 Intel GPU，支持要求见安装页面。选择您的平台和安装方法，然后按照说明进行操作。步骤如下：

1. 如果需要，请首先安装上面链接的 IPEX 安装页面中列出的或更新的适用于 Windows 和 Linux 的驱动程序或内核。
2. 按照说明为您的平台安装英特尔的 oneAPI Basekit。
3. 使用您的平台的安装页面中提供的说明安装 IPEX 包。
4. 按照 [Windows 和 Linux 的 ComfyUI 手动安装说明](https://github.com/comfyanonymous/ComfyUI?tab=readme-ov-file#manual-install-windows-linux)进行操作，并在所有内容安装完成后按照上述说明正常运行 ComfyUI。

您可以[在此处](https://github.com/comfyanonymous/ComfyUI/discussions/476)找到更多讨论和帮助。

##### Apple Mac 芯片

您可以使用任何最新的 macOS 版本在 Apple Mac 芯片（M1 或 M2）中安装 ComfyUI。

1. [安装 pytorch nightly](https://developer.apple.com/metal/pytorch/)。有关说明，请阅读Mac Apple 开发者指南上的加速 PyTorch 训练（确保安装最新的 pytorch nightly）。
2. 按照 [Windows 和 Linux 的 ComfyUI 手动安装说明](https://github.com/comfyanonymous/ComfyUI?tab=readme-ov-file#manual-install-windows-linux)进行操作。
3. 安装 ComfyUI [依赖项](https://github.com/comfyanonymous/ComfyUI?tab=readme-ov-file#dependencies)。[如果您有另一个稳定的 Diffusion UI，您可能能够重用依赖项](https://github.com/comfyanonymous/ComfyUI?tab=readme-ov-file#i-already-have-another-ui-for-stable-diffusion-installed-do-i-really-have-to-install-all-of-these-dependencies)。
4. 通过运行 `python main.py` 启动 ComfyUI。

> **注意**：请记住将您的模型、VAE、LoRA 等添加到相应的 ComfyUI 文件夹中，如 [ComfyUI 手动安装](https://github.com/comfyanonymous/ComfyUI?tab=readme-ov-file#manual-install-windows-linux)中所述。

##### DirectML（Windows 上的 AMD 卡）

```
pip install torch-directml
```

然后你可以使用以下命令启动 ComfyUI：

```
python main.py --directml
```

### 我已经安装了另一个用于 Stable Diffusion 的 UI，我真的必须安装所有这些依赖项吗？

你不需要。如果你安装了另一个 UI 并使用其自己的 python venv，则可以使用该 venv 来运行 ComfyUI。你可以打开你最喜欢的终端并激活它：

```
source path_to_other_sd_gui/venv/bin/activate
```

或者在 Windows 上：

使用 Powershell：

```
"path_to_other_sd_gui\venv\Scripts\Activate.ps1"
```

使用 cmd.exe：

```
"path_to_other_sd_gui\venv\Scripts\activate.bat"
```

然后，您可以使用该终端运行 ComfyUI，而无需安装任何依赖项。请注意，venv 文件夹可能会根据 SD UI 的不同而有不同的名称。

## 跑步

```
python main.py
```

### 对于 ROCm 未正式支持的 AMD 卡

如果遇到问题，请尝试使用此命令运行它：

对于 6700、6600 以及其他 RDNA2 或更早版本：

```
HSA_OVERRIDE_GFX_VERSION=10.3.0 python main.py
```

对于 AMD 7600 以及其他 RDNA3 卡：

```
HSA_OVERRIDE_GFX_VERSION=11.0.0 python main.py
```

## 笔记

只有图表中具有所有正确输入的输出部分才会被执行。

只会执行每次执行后发生变化的图表部分，如果您两次提交同一个图表，则只会执行第一个图表。如果您更改了图表的最后一部分，则只会执行您更改的部分以及依赖于它的部分。

将生成的 png 拖到网页上或加载它将为您提供完整的工作流程，包括用于创建它的种子。

您可以使用 `()` 来改变单词或短语的强调，例如：`(good code:1.2)` 或 `(bad code:0.8)`。`()` 的默认强调是 `1.1`。要在实际提示中使用 `()` 字符，请将其转义为 `\(` 或 `\)`。

您可以使用 `{day|night}` 进行通配符/动态提示。使用此语法，每次排队提示时，前端都会将 `{wild|card|test}` 随机替换为 `wild`、`card` 或 `test`。要在实际提示中使用 `{}` 字符，请将其转义为：`\{` 或 `\}`。

动态提示还支持 C 风格的注释，例如 `// comment` 或 `/* comment */`。

要在文本提示中使用文本反转概念/嵌入，请将它们放在 `models/embeddings` 目录中，并在 `CLIPTextEncode` 节点中使用它们，如下所示（可以省略 `.pt` 扩展名）：

```
embedding:embedding_filename.pt
```

## 如何显示高质量的预览？

使用 `--preview-method auto` 启用预览。

[默认安装包](https://github.com/madebyollin/taesd)包含低分辨率的快速潜在预览方法。要使用 TAESD 启用更高质量的预览，请下载 `taesd_decoder.pth`（适用于 SD1.x 和 SD2.x）和 `taesdxl_decoder.pth`（适用于 SDXL）模型并将它们放在 `models/vae_approx` 文件夹中。安装完成后，重新启动 ComfyUI 以启用高质量预览。

## 如何使用 TLS/SSL？

通过运行以下命令生成自签名证书（不适合共享/生产用途）和密钥：

```
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 3650 -nodes -subj "/C=XX/ST=StateName/L=CityName/O=CompanyName/OU=CompanySectionName/CN=CommonNameOrHostname"
```

使用 `--tls-keyfile key.pem --tls-certfile cert.pem` 启用 TLS/SSL，现在可以使用 `https://...` 而不是 `http://...` 访问应用程序。

> [注意：Windows](https://wiki.openssl.org/index.php/Binaries) 用户可以使用 `alexisrolland/docker-openssl` 或第三方二进制发行版之一来运行上述命令示例。
>
> 如果您使用容器，请注意卷挂载 `-v` 可以是相对路径，因此 `... -v ".\:/openssl-certs" ...` 会在命令提示符或 powershell 终端的当前目录中创建密钥和证书文件。

## 支持和开发渠道

矩阵空间：`#comfyui_space:matrix.org`（它类似于 discord，但是开源）。

另请参阅：https://www.comfy.org/

GitHub：https://github.com/comfyanonymous/ComfyUI?tab=readme-ov-file
