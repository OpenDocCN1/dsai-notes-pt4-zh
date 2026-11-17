
SVD 本地部署目前仅支持图生视频，关于图片的来源，我们可以选择 AI 绘画工具如：Midjourney、Stable Diffusion 等生成图片，然后再上传到 SVD 进行视频的生成！！同时，可以调左侧的一些参数，控制视频的生成效果！！

![](img/4031fdf1fef1f92d58115c698106c19c.png)

视频生成的保存路径在 `outputs` 下，生成的效果如下：

![](img/fe251c104afba42122d839228c890b99.png)

时长 00:02

# ComfyUI 部署实战

在本节要介绍的主题是利用 ComfyUI + Stable Video Diffusion 实现本地文生视频的工作流！在上一节中，介绍了在云服务器上实现 SVD 的视频生成的整个流程，但是发现所耗费的资源却是相当的巨大，那么有没有一种节省资源、且非常方便的方式呢？答案是肯定的！！

![](img/f2ca52a49811d5457a7111c21d8de709.png)

接下来，我们就开始吧！！

> **硬件资源情况：**
>
> - 16G 内存，Windows 平台
> - 显卡 3070Ti，8G 显存

## 安装 ComfyUI

### 安装 Python 和 Git

关于这一部分，就省略了，网上搜一搜有一堆的教程！！

### 克隆 ComfyUI 仓库

```
git clone https://github.com/comfyanonymous/ComfyUI
```

### 创建 Python 虚拟环境

```
cd ComfyUI
python -m venv venv
```

此时，我们会发现在 ComfyUI 路径下，有了一个 `venv` 的文件夹，这个里面就是创建好的 Python 虚拟环境！

![](img/8c89065037826cc81fa5688412e6b62c.png)

运行 `.\venv\Scripts\activate` 切换到虚拟环境！这样就可以开始安装 Pytorch 和依赖包了！！相信大家都是会安装的，有问题可随时私信！！

### 下载模型权重和工作流

> 🎉 文生视频模型权重：https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/resolve/main/sd_xl_base_1.0.safetensors?download=true
>
> 图生视频模型权重：https://huggingface.co/stabilityai/stable-video-diffusion-img2vid-xt/resolve/main/svd_xt.safetensors?download=true
>
> 图片编解码模型权重：https://huggingface.co/stabilityai/sdxl-vae/resolve/main/sdxl_vae.safetensors?download=true

将 `sd_xl_base_1.0.safetensors` 和 `svd_xt.safetensors` 放置于 `models/checkpoints` 下，`sdxl_vae.safetensors` 需放置于 `models/vae` 下。关于以上模型权重和工作流同样放到了百度网盘资源里，关注👸号「魔方AI空间」，回复 【SVD】 即可获取！！

## 运行 ComfyUI 并加载工作流

在以上准备工作完成后，我们就可以开始运行 ComfyUI 了！在命令行终端：

```
python main.py
```

在浏览器上输入上面的 URL 地址，出现如下界面：

此时，默认的 ComfyUI 版本不支持模型版本和 node 管理，这会导致安装和卸载模型时会非常麻烦！因此，我们需要安装一个 ComfyUI Manager 插件！

```
cd .\custom_nodes
git clone https://github.com/ltdrdata/ComfyUI-Manager.git
```

再次运行 `python main.py`，就会出现 Manager 菜单了！！！

![](img/3be71955f3e589c6cfa0f598c8b71a82.png)

接下来，我们就可以跑文生视频的工作流了！！！关于工作流，我也给大家准备好了，同样放到百度网盘链接了，关注👸号「魔方AI空间」，回复 【SVD】 即可获取！！工作流下载好后，我们可以使用 ComfyUI 菜单的 load 功能，进行工作流的加载！

然后，点击菜单栏「Queue Prompt」就可以开始视频的生成啦！我们可以通过工作流上的绿色框看到运行的进度！！！最后，我们可以去 ComfyUI 目录下的 `output` 文件夹查看生成好的视频！如果我们在生成视频的过程中，出现显存溢出的问题，请往下看！！

## ComfyUI 性能调优

切到终端，输入 `python main.py -h` 查看启动项参数，其中有显存使用策略的一组参数，分别是 `highvram`、`normalvram`、`lowvram` 和 `novram`。

其中：

- `highvram`：表示需要更多的显存做缓存，是 ComfyUI 的默认设置；
- `novram`：表示不需要显存做缓存；缓存越多，速度越快，反之，则越慢，可根据自己电脑的显存大小来微调。

![](img/35ef1a9f008f411b9af1d2d3e4783d3d.png)

为了尽可能减少显存溢出问题，可直接使用 `novram` 参数启动！！！

运行：

```
python main.py --novram
```

至此，我们就完成了 ComfyUI + SVD 视频生成的本地部署工作流的构建！如遇到问题欢迎随时交流！！！

# 避坑指南

1. 直接使用百度网盘里面准备好的资源，可以规避掉 90% 的坑；
2. 如果一直报显存溢出问题，可以调低帧数或增加 `novram` 启动参数；
3. 针对云部署实战部分，基础依赖模型权重有两个 `models–laion–CLIP-ViT-H-14-laion2B-s32B-b79K` 和 `ViT-L-14.pt`，需要放到指定路径下

# 总结

在 Sora 发布之后，似乎在这之前的所有视频生成模型都已黯淡无光，难以与之争锋！然而 Stable Video Diffusion 作为开源项目，我们可以在自己的机器上自由创作而无需充值，这也是其独特优势！从技术角度看，SVD 生成的视频画质非常清晰，帧与帧之前的过渡也非常的自然，无论是背景闪烁的问题，还是人物一致性的保持都能够得到妥善解决！尽管目前只能生成最多 4 秒的视频，与 Sora 生成的 60s 视频差距很远，但是 SVD 还在不断迭代，相信其终将再次强大！！！

同时，我们会持续关注 SVD 技术及前沿视频生成技术，尝试多种不同的部署微调方式，介绍更多视频生成的技术模型，更多精彩内容会在后续文章中放出，敬请期待！！！

> 原文地址：https://mp.weixin.qq.com/s/le0_4VytRyNaPQucCSpE8g
