
# 001-借助ChatGPT编程写脚本-MJ_Split

## 来源

> 来源：https://aigcreative.feishu.cn/docx/RSiHdb0p0okNwvxK1wMc4Ow4nyd
> 原文链接：https://chat.openai.com/share/c822a6af-b1c4-41c1-b9cd-83441393a500
> By何先森Kevin

## 需求

每次MJ出图后，需要把四宫格分拆为四张图，需要个脚本自动化实现。

## 问题一：制作 `.exe` 图片分割工具

> 问：我是一名编程小白，我现在想做一个`.exe`的小工具软件，实现以下功能：提交上传当前文件夹的一张图或者多张图，能将提交的每张图平均分成左上、右上、左下、右下四等份，然后以原图的文件名命名，输出保存在当前文件夹。请问我该怎么实现，请给出源代码

## 实现方案

要创建一个实现您描述功能的 `.exe` 工具，您可以使用 Python 编程语言，因为它对于初学者友好且功能强大。完成编写代码后，您可以使用工具如 `PyInstaller` 将 Python 脚本转换为 `.exe` 文件。以下是实现您所需功能的步骤和示例代码：

1. 安装 Python：首先，如果您还没有安装 Python，请从 Python 官网下载并安装。
1. 编写 Python 脚本：您需要编写一个 Python 脚本来处理图像。这个脚本会读取文件夹中的图片，将每张图片分为四等份，并保存为新的文件。我们将使用 Python 的 `PIL`（`Pillow`）库来处理图像。
1. 转换为 `.exe` 文件：使用 `PyInstaller` 或类似工具将您的 Python 脚本转换为 `.exe` 文件。

现在，我将为您提供一个简单的 Python 脚本示例，它实现了您描述的功能。

首先，您需要安装 `Pillow` 库，这是一个图像处理库。可以通过命令行安装：

```
pip install pillow
```

然后，这是一个简单的 Python 脚本示例：

```
from PIL import Image
import os

def split_image(image_path):
    img = Image.open(image_path)
    width, height = img.size

    # 计算每个部分的尺寸
    left = 0
    top = 0
    right = width / 2
    bottom = height / 2

    # 分割图片
    images = {
        "left_top": img.crop((left, top, right, bottom)),
        "right_top": img.crop((right, top, width, bottom)),
        "left_bottom": img.crop((left, bottom, right, height)),
        "right_bottom": img.crop((right, bottom, width, height))
    }

    # 保存每个部分
    for position, image in images.items():
        base_name = os.path.basename(image_path)
        name, ext = os.path.splitext(base_name)
        new_name = f"{name}_{position}{ext}"
        image.save(new_name)
        print(f"Saved: {new_name}")

def process_folder(folder_path):
    for file in os.listdir(folder_path):
        if file.lower().endswith(('.png', '.jpg', '.jpeg')):
            split_image(os.path.join(folder_path, file))

if __name__ == "__main__":
    folder_path = '.'  # 当前文件夹
    process_folder(folder_path)
```

将这段代码保存为 `.py` 文件，例如 `split_images.py`。然后在包含图片的文件夹中运行此脚本。

为了将这个脚本转换为 `.exe` 文件，您可以安装 `PyInstaller`：

```
pip install pyinstaller
```

然后运行以下命令：

```
pyinstaller --onefile split_images.py
```

## 问题二：`.py` 文件放置位置

> 问：我需要把`.py`文件放在哪里？

要将您的 `.py` 文件转换为 `.exe` 文件，您应该将 `.py` 文件放在您方便访问的任何位置。这个位置可以是您的电脑上的任何文件夹，比如您的桌面或者专门为您的项目创建的一个文件夹。以下是您需要做的步骤：

1. 存储 `.py` 文件：首先，将您编写的 Python 脚本（比如 `split_images.py`）保存在电脑上的一个文件夹中。
1. 打开命令行：打开命令行界面（在 Windows 上是命令提示符或 `PowerShell`）。
1. 导航到文件所在的文件夹：在命令行中，使用 `cd` 命令导航到包含您的 `.py` 文件的文件夹。例如，如果您的文件在桌面上，您可以使用类似于 `cd Desktop` 的命令。
1. 运行 `PyInstaller`：在命令行中，确保您当前所在的目录是包含 `.py` 文件的目录，然后运行以下命令来生成 `.exe` 文件：

```
pyinstaller --onefile split_images.py
```

1. 这里 `split_images.py` 是您的 Python 脚本文件名。
1. 查找 `.exe` 文件：`PyInstaller` 会在相同目录下创建一个名为 `dist` 的新文件夹。在 `dist` 文件夹中，您会找到生成的 `.exe` 文件。
1. 使用 `.exe` 文件：您可以将这个 `.exe` 文件复制到任何地方，并在任何 Windows 系统上运行它，无需 Python 环境。

请确保在运行 `PyInstaller` 命令之前，您的电脑上已经安装了 `PyInstaller`。如果没有安装，您可以通过运行 `pip install pyinstaller` 来安装它。

## 下载体验

![](img/8ea089727f6f69110935aae1d522710b.png)

[`MJ四宫格切图工具【MJ_Split】.zip`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BMT0bHovgoysTVxP0UzcKuLhnje)

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>
