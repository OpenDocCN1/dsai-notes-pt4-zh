# AI 生成二维码，太好看了

> 来源：[https://aiyanxishe.feishu.cn/docx/AvLddXB31oKGZbxagv8cWQH4nbh](https://aiyanxishe.feishu.cn/docx/AvLddXB31oKGZbxagv8cWQH4nbh)

什么效果？有什么用处？来看下面的例子。

这是一张平平无奇，看上去略有些错乱的 Stable Diffusion 生成的风格化图像：

![](img/51404f515ca297bef4177d250a370ff6.png)

但给它加上三个定位点后，这张图就变成一个可以扫描识别的二维码了：

![](img/e07c3593651a8f7f42118c600d8a38b8.png)

长按识别二维码，跳转至 qrbtf.com

很神奇吧！下面是这个项目的缘起、训练过程和更多生图结果……

## **缘起**

大二的时候，我和同学做了一个参数化二维码生成器，出于各种（摆烂）原因，至今没有继续更新。记得一次和创新工场的咏刚老师交流时，突然聊到，能不能在任何肉眼观察完全正常的图像中编码隐藏的信息。在那个 GAN 的年代，机器学习的生态远没有如今活跃，不说有 Gradio Web UI、Diffusers 这样好用的框架，但配环境就足够劝退我了，这个想法就此搁置。

![](img/447129a4046ec5f63549e907b90e2505.png)

直到 Stable Diffusion 横空出世、ControlNet 席卷各大行业，经历了很长一段时间的摸索，我才重新开了这个坑，能不能用扩散模型生成一个看上去很像一张图片的二维码？

![](img/9a098fb7eee5bafca551f16e73af9dad.png)

最初的 ControlNet 尝试

![](img/903cdde88dba0f69f43cb2ac7a6537a4.png)

训练的中国传统纹样 LoRA

![](img/0e8c91870ea2607aae53814ff46ece4e.png)

AIGC All in One 文档，持续更新中

![](img/e1f2317fe715c7d9b0ccb42bfa5d0657.png)

HuggingFace JAX/Diffusers Sprint

## **训练**

ControlNet 训练的数据结构十分简单，仅为一张输入图（conditioning image）、一张输出图（image）和一段标注（caption）。官方给出了非常多预训练模型，包括 1.0 版本中的 Depth、HED、OpenPose 和 1.1 中非常有创意的 Shuffle、Tile 和 Instruct Pix2Pix 等。

ControlNet 的训练对数据量和算力均有较高要求，论文中记录的训练数据量从 8 万到 300 万不等，训练时间可达 600 个 A100 GPU 小时。好在作者提供了基础的训练脚本，HuggingFace 也做了 Diffusers 实现。

在此前的 JAX Sprint 中，我们有幸使用 Google TPU v4，非常快地完成了 300 万张图的训练。可惜活动结束，我们回到了实验室的 A6000 / 4090，训练了一个 10 万张图的版本，且学习率非常大，只为尽早出现“突变拟合”（Sudden Convergence）。

![](img/317d1c77cb2adba7bcdfead9124eba57.png)

GPU / TPU 训练参数

![](img/17ee96b7e960379189feb878d6e9d956.png)

灰度控制 ControlNet，训练流程见 aigc.ioclab.com/sd-showcase/brightness-controlnet

![](img/833441e6b163cd628835190dee1cbeff.png)

光影控制 ControlNet，训练流程见 aigc.ioclab.com/sd-showcase/light_controlnet

## **推理**

测试模型训练完毕后，我们尝试了多种 Checkpoint + LoRA + QR Code ControlNet 的组合。得到了下面各式各样的可识别二维码。

### **中国传统纹样**

LoRA 训练流程：aigc.ioclab.com/sd-showcase/chinese-ornament

LoRA 模型下载：civitai.com/models/29858/chinese-traditional-pattern

![](img/4fb92e0f3f44d0721dde79178b28d0e1.png)

![](img/b42b905a478e52b264353e7653b3be27.png)

![](img/156c970d70c040e2a612dc6dbffb4159.png)

![](img/0fd88628cc359e9172f95b5f1f1be7b3.png)

### **浮世绘风格**

LoRA 训练流程：aigc.ioclab.com/sd-showcase/fuyue

LoRA 模型下载：civitai.com/models/25222/ukiyo-e-fuyue-style-background-mix

![](img/c39ebf6cfb906e09581427d62c2a07af.png)

![](img/fea9b7963879505ea9cb81b0107599cb.png)

![](img/fa9655fe3a69b877418430b1c14dda42.png)

### **二次元和插画风格**

![](img/ca7e323e7608aefb9d16bf9a809316a7.png)

![](img/91d7e5cc7a395786cffb007cfabe382e.png)

![](img/474f4ca14624f34828c82f80094d2867.png)

![](img/c997886ce81e8559ddf99b2a6545e981.png)

![](img/8e915d3f4d15a59d0ebec657318e2099.png)

### **水墨风格（MoXin）**

![](img/62dcebfc2a5f435c6b112eaaa5457a13.png)

### **水彩风格**

![](img/1e084b00d7a0ffd4d3acfe0fc8631deb.png)

### **立体风格**

![](img/35b8abbb1a3b3c6d26f9b07e75ffcf7c.png)

![](img/4f4021a9c2d30913ea5a6938bab8d9f6.png)

![](img/50cc3fc6feedaa93d90447acfb4845fd.png)

![](img/7b9f64fa6865925689a5e5cb594fec2b.png)

### **抽象风格**

### **PCB**** 风格**

![](img/f0b466c80f7a8b87959a02e1bf8519a1.png)

![](img/34fdfdbc389751a614378f907275f48b.png)

### **Bonus：****Ps**** 重绘**

![](img/7679719e3670cc7532bfe7f8fe7d934f.png)

![](img/39804390227cb84b2beedc24faab605e.png)

## **后记**

加入AI研习社，一起学习AI工具，做AI时代新青年

[AI研习社介绍](https://zl49so8lbq.feishu.cn/wiki/Q8e5wnAjAiV2Uek2rsnc5Znqnrg)

[AI研习社知识星球精选导读.xlsx](https://zl49so8lbq.feishu.cn/wiki/USLewXx7EiWqdfkBBdUcsoxmnae)