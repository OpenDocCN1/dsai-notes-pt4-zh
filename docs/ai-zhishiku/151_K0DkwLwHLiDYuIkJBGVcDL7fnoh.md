# MLC LLM 手机&PC 离线大语言模型

> 来源：[https://my.feishu.cn/docx/Fvl1dfG2MoyI2yxHBuEcoXP7nbg](https://my.feishu.cn/docx/Fvl1dfG2MoyI2yxHBuEcoXP7nbg)

本部分包含在您的环境中本地运行大型语言模型和聊天机器人的说明。

•iPhone

•安卓

•视窗 Linux Mac

•网页浏览器

### iPhone

试用此[TestFlight 页面](https://testflight.apple.com/join/57zd7oxa)（仅限前 9000 名用户）以安装和使用我们为 iPhone 构建的示例 iOS 聊天应用程序。我们的应用程序本身需要大约 4GB 的内存才能运行。考虑到 iOS 和其他正在运行的应[用程序，我们将需要具](https://github.com/mlc-ai/mlc-llm)有 6GB（或更多）内存的最新 iPhone 来运行该应用程序。我们仅在 iPhone 14 Pro Max 和 iPhone 12 Pro 上测试了该应用程序。您还可以查看我们的GitHub 存储库以从源代码构建 iOS 应用程序。

注意：iOS 应用程序上的文本生成速度有时会不稳定。它可能一开始运行缓慢，然后恢复到正常速度。

### 安卓

[在此处](https://github.com/mlc-ai/binary-mlc-llm-libs/raw/main/mlc-chat.apk)下载 APK 文件并安装到您的手机上。然后您可以开始与 LLM 聊天。当您第一次打开应用程序时，需要下载参数并且加载过程可能会很慢。在以后的运行中，参数将从缓存中加载（速度很快），您可以离线使用该应用程序。我们目前的演示依赖于手机上的 OpenCL 支持并占用大约 6GB 的内存，如果您的手机配备最新的 Snapdragon 芯片，您可以试用演示。

我们[在三星 Galaxy](https://github.com/mlc-ai/mlc-llm/tree/main/android) S23 上测试了我们的演示。由于 OpenCL 支持有限，它还不能在 Google Pixel 上运行。我们将继续为开源社区带来支持和贡献。您还可以查看我们的GitHub 存储库以从源代码构建 Android 应用程序。

### Windows Linux Mac

我们提供了一个 CLI（命令行界面）应用程序来与您终端中的机器人聊天。在安装 CLI 应用程序之前，我们应该先安装一些依赖项。

1.我们使用 Conda 来管理我们的应用程序，因此我们需要[安装一个版本的 c](https://docs.conda.io/en/latest/miniconda.html)o[nda。我们可以安](https://github.com/conda-forge/miniforge)装Miniconda或Miniforge。

2.在 Wind[ows 和 Linux](https://developer.nvidia.com/vulkan-driver) 上，聊天机器人应用程序通过 Vulkan 平台在 GPU 上运行。对于 Windows 和 Linux 用户，请安装最新的Vulkan 驱动程序。对于 NVIDIA GPU 用户，请确保安装 Vulkan 驱动程序，因为 CUDA 驱动程序可能不太好。

安装所有依赖项后，只需按照下面的说明安装 CLI 应用程序：

```
# Create a new conda environment and activate the environment.
conda create -n mlc-chat
conda activate mlc-chat# Install Git and Git-LFS, which is used for downloading the model weights# from Hugging Face.
conda install git git-lfs# Install the chat CLI app from Conda.
conda install -c mlc-ai -c conda-forge mlc-chat-nightly# Create a directory, download the model weights from HuggingFace, and download the binary libraries# from GitHub.mkdir -p dist
git lfs install
git clone https://huggingface.co/mlc-ai/demo-vicuna-v1-7b-int3 dist/vicuna-v1-7b
git clone https://github.com/mlc-ai/binary-mlc-llm-libs.git dist/lib# Enter this line and enjoy chatting with the bot running natively on your machine!
mlc_chat_cli
```

### 网页浏览器

请查看我们的配套项目[WebLLM](https://mlc.ai/web-llm/)，它可以将模型本地部署到浏览器。这里的一切都在没有服务器支持的浏览器中运行，并通过 WebGPU 加速。