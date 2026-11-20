
# DeepSeek Janus本地部署与调用实战

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/VaH7dmA78o1p3AxVOJbcUqojnKV](https://kq4b3vgg5b.feishu.cn/docx/VaH7dmA78o1p3AxVOJbcUqojnKV)

## 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》：

![](img/f9bbba95a3778dd747b8ea1c960844ef.png)

![](img/59e94b1afc95d25f16df6eca3734723a.png)

此外，公开课全套学习资料，已上传至网盘（`https://pan.baidu.com/s/1_4wIkCvjwikuXpii8o3-gQ?pwd=16xs`）

需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

---

## 一、Janus Pro模型介绍

![](img/c2afebcf752d25d06777d3cef84f7f8c.png)

### 1. Janus Pro统一多模态大模型项目简介

`Janus-Pro` 是此前工作 `Janus` 的高级版本。具体来说，`Janus-Pro` 包括以下改进：优化的训练策略、扩展的训练数据以及更大规模的模型。通过这些改进，`Janus-Pro` 在多模态理解和文本生成图像的指令跟随能力上取得了显著进步，同时还提升了文本生成图像的稳定性。

![](img/1b3b6d415e192eeca94e87fe0f21f17b.png)

`Janus` 是一种新颖的自回归框架，旨在统一多模态的理解与生成。它通过将视觉编码解耦为独立的路径来克服以往方法的局限性，同时仍然使用单一的统一 `Transformer` 架构进行处理。这种解耦不仅缓解了视觉编码器在理解和生成任务中的角色冲突，还增强了框架的灵活性。

![](img/9e9eca18c43582be80d3f311ecc021cd.png)

`Janus` 超越了以往的统一模型，并且在性能上与任务专用模型持平或超越。其简洁性、高灵活性和高效性使 `Janus` 成为下一代统一多模态模型的有力候选者。

![](img/918df732df31897cf0f6d1b5dabe35a4.png)

### 2. Janus Pro模型资料汇总

+ [`Janus` Github主页](`https://github.com/deepseek-ai/Janus`)

+ [`Janus` 7B HuggingFace主页](`https://huggingface.co/deepseek-ai/Janus-Pro-7B`)

+ Janus Pro技术报告： [`janus_pro_tech_report.pdf`](https://github.com/deepseek-ai/Janus/blob/main/janus_pro_tech_report.pdf)

+ Janus 7B在线demo： [`https://huggingface.co/spaces/deepseek-ai/Janus-Pro-7B`](https://huggingface.co/spaces/deepseek-ai/Janus-Pro-7B)

![](img/1c81b44ffc4385252be49503d16af0f4.png)

### 3. Janus Pro模型部分效果演示

![](img/8cc1713197e3716451c670c65d3dfa60.png)

这张图片是一个幽默的比较，采用了流行的“buff Doge vs. Cheems”表情包格式，来对比两种视觉编码方法。

1. 解耦视觉编码（Buff Doge）：

图片左侧展示了一只肌肉发达的“buff Doge”，代表了一种强大且稳健的视觉编码方法。这可能象征着一种更强大或更高效的编码视觉信息的方法，比如深度学习模型或能够处理复杂视觉任务的先进神经网络。

2. 单一视觉编码器（Cheems）：

图片右侧展示了一只体型较小、肌肉较少的“Cheems Doge”，代表了一种更简单或较弱的视觉编码方法。这可能象征着一种传统或不太先进的方法，比如单一的神经网络或一种基础的编码方法，可能无法像更复杂的系统那样表现出色。

### 4. Janus模型运行硬件要求

`Janus Pro`对硬件要求较低，以下是`1B`模型和`7B`模型运行的GPU显存占用情况：

| 任务类型 | `Janus-Pro-1B` | `Janus-Pro-7B` |

| --- | --- | --- |

| 图像识别 | 5G（ 3060） | 15G（4080） |

| 图片生成 | 14G（4080） | 40G（ 3090/4090*2） |

## 二、Janus模型下载与项目部署

### Step 1. 安装 git 与克隆仓库

在新服务器上运行该项目，首先需要更新`apt`工具并安装`Git`工具：

```
sudo apt update
sudo apt install git
```

![](img/e4acfcbb9b7c195d427c8ae510939223.png)

然后使用 `git` 克隆 `HunyuanVideo` 仓库到本地：

```
cd ~/autodl-tmp
git clone https://github.com/deepseek-ai/Janus.git
cd HunyuanVideo
```

![](img/4b59cd56f38480cca83decaaae272b96.png)

当前项目主目录如下：

![](img/9b7963e529956013affccd27ac902382.png)

项目文件目录如下：

![](img/96326e15b6309f65c8f782087d55d35a.png)

当然，也可以在Github官网下载安装包：`https://github.com/deepseek-ai/Janus?tab=readme-ov-file#janusflow` ，并上传至服务器解压缩：

![](img/8971e54b8e769e001c9b6c96e4a43ec4.png)

### Step 2. 创建虚拟环境 & 安装项目依赖

准备 `Conda` 虚拟环境：使用以下命令创建 `Conda` 环境：

```
conda create -n janus python=3.9
```

![](img/df033521be1b0ce0eaf70eb4a8624115.png)

激活环境：创建环境后，激活它：

```
conda init
source ~/.bashrc
conda activate janus
```

安装 `pip` 依赖：激活环境后，安装所需的 Python 依赖：

```
cd /root/autodl-tmp/Janus
pip install -e
```

![](img/26181b667c2d7c26bb844b5f8f2203f3.png)

下载JupyterLab

考虑到后续需要在代码环境中调用`Janus`，这里还需要下载`JupyterLab`，并配置kernel：

```
conda install jupyterlab
conda install ipykernel
python -m ipykernel install --user --name janus --display-name "Python (janus)"
```

打开Jupyter

```
jupyter lab --allow-root
```

并确认可以选择janus环境

![](img/04772c8b17f9d97e139cdf4e67d75f79.png)

### Step 3. 下载预训练模型

这里我们考虑在项目主目录下创建`models`文件夹，用于保存`Janus-Pro-1B`和`7B`模型权重。考虑到国内网络环境，这里推荐直接在`Modelscope`上进行模型权重下载。

`Janus-Pro-1B`模型权重：`https://www.modelscope.cn/models/deepseek-ai/Janus-Pro-1B`

`Janus-Pro-7B`模型权重：`https://www.modelscope.cn/models/deepseek-ai/Janus-Pro-7B`

安装`modelscope`

```
pip install modelscope
```

创建权重保存文件夹

```
cd /root/autodl-tmp/Janus
mkdir ./Janus-Pro-1B
mkdir ./Janus-Pro-7B
```

![](img/457378eddec7441c0ed60668e07c51fd.png)

下载`Janus-Pro-1B`模型权重

```
# 下载1B模型
modelscope download --model deepseek-ai/Janus-Pro-1B --local_dir ./Janus-Pro-1B
```

![](img/9dcf8701b1bca5ec3ab2d0b48850cc27.png)

下载`Janus-Pro-7B`模型权重

```
# 下载7B模型
modelscope download --model deepseek-ai/Janus-Pro-7B --local_dir ./Janus-Pro-7B
```

![](img/08499a3e7656abc5c2bdb0234037d21a.png)

预训练模型权重较大也可直接使用在课件文件夹中通过百度网盘下载：

![](img/fd3f305be349872cdb4f04f3dfb7e19a.png)

## 三、Janus本地调用流程

本部分内容详见Jupyter代码文件：`Janus Pro本地调用指南`

![](img/4cd7df213da3afea15820833b32b5857.png)

同样在网盘中下载即可：

## 四、Gradio前端调用方法

此外，`Janus`项目中还提供了前端调用脚本，我们可以按照如下流程开启前端并与`Janus`进行对话。

安装相关依赖

```
pip install -e .[gradio]
```

![](img/7cb667876c887516004dd1fedea5bb5c.png)

修改推理脚本

若要调用本地模型进行推理，则需要修改对应的前端脚本。打开 `./demo/app_januspro.py`文件，并在15行处修改为当前本地模型下载地址：

![](img/1be9ad9e23e0e3db74367f4ee1fc7fab.png)

保存并退出即可。

上传补充文件【可选】

若需要使用公网IP分享连接，则需要额外手动下载`frpc`包：

![](img/8d77958d90f504d52d61083c8934b541.png)

`frpc`包已上传至课件网盘（同样保存在上文的百度网盘中）

然后添加至 `/root/miniconda3/envs/janus/lib/python3.9/site-packages/gradio` 文件夹内：

![](img/52e49d657f7dea3d967eca20c3b8e02e.png)

开启前端服务

然后即可开启前端服务：

```
python demo/app_januspro.py
```

![](img/cb132a0ccee702e6ff0635d9da8b038c.png)

然后即可按照如下方式进行使用：

运行得到结果如下：

图像创建流程如下，下滑至Text-to-Image Generation页面：

稍等片刻，即可查看最终运行结果：

![](img/fe86987ec52cb7b8c3c197a0ba04911f.png)
