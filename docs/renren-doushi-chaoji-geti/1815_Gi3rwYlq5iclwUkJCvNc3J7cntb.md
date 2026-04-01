# 2023-10-09AIGC 艺术创作项目阶段性复盘

> 来源：[https://xuqiwei1986.feishu.cn/docx/QXSWdB7gBoYxy5x9n17cNUfBnmd](https://xuqiwei1986.feishu.cn/docx/QXSWdB7gBoYxy5x9n17cNUfBnmd)

> 🔗 原文链接： h[ttps://articles.zs](https://articles.zsxq.com/id_s26r7dcpk4wz.html)xq.com/id_s26r7d...

>

大家好，我是偷影子的麋鹿，大家可以叫我小鹿，我正在参加小林老师的第四期 AI 之心写作营。

最近一两个月，研究院所在的项目组正在开发一款 AIGC 绘画艺术创作小程序，我非常有幸从一开始就参与到了项目的产研中。现在项目的初始阶段差不多开发完毕，即将进入小范围的内测阶段，借此机会做一个阶段性的复盘。

# 一、产品形态和定位

## 一）团队介绍

在说明产品方向之前，请允许我先介绍一下整个项目组的成员构成。总共有四位成员，包括项目负责人、前端开发、后端/算法开发（本人担任）和 UI 设计。考虑到项目组短缺的人力资源和对市场上相似 AIGC 绘画小程序 （比如 MewX AI）的调研，我们认为基于 SD 做功能的二次开发和微调，把 SD 上的产品功能做个封装都列在小程序上不适合我们当下的情况。我们的开发资源和运营资源都非常有限，要把时间和资源花在刀刃上。

## 二）需求和受众分析

首先，调研了一下市场上已有的 AIGC 应用，包括 MewX AI 和妙鸭相机。MewX AI 是偏工具类的应用，为小白提供了一个可以快速体验 AI 绘画的平台。妙鸭相机是兼顾娱乐和社交的垂类人像美颜应用，通过品牌营销和宣传迅速在年轻女性中打开知名度。现在大多数 AIGC 应用是通过类似线上订阅的方式来进行付费，让用户养成为服务器资源付费的习惯。

我们盘了一下手里的资源，有研究院里的机器人资源、以研究院为核心扩散开来的喜爱艺术的人群资源、画家资源和供应商资源，这些资源可以组合形成一条商业闭环模式。

**用户上传图片 -> AIGC 艺术创作小程序处理 -> 一副经过 **AI 风格**转换的艺术**画作 -> 知名画家参照进行作画 -> 现实的画寄回给用户 。这里的 画家作画 也可以变成 AI 生成的艺术画作 -> 打印到衬衫/茶杯/手机壳 上，关键点是要闭环打通线上和线下的链条，让所有的交易都在一个链条里完成，不再跳转到其他的电商平台上。

根据我们的调研，国内用户订阅付费的习惯比较差，如果纯靠线上订阅来收费，不进行线下实际商品的交互，那整个商业模式是很容易亏钱的。初始的内测阶段，我们邀请的人会以喜欢艺术的人为主，这部分人有比较强的付费意识，会为了一幅感动他们的画来进行相对高价的付费。如果后台数据好，就会接着向外扩散邀请人群，这一阶段就可以引入一些平价的周边。后续甚至可以引入机器人来进行作画，这一块的想象空间还是很大的。

# 二、技术实现

## 一）封装 SD 的 API

SD 是一款开源软件，现在大部分人的需求都是使用 SD 的 Web-UI 来进行作图。如果你是一名开发人员，想把它集成到你自己的服务里，那就需要调用它的 API 了。

### 1、API 简单介绍

这里以我自己在腾讯云上部署的一套 SD 服务来举例，如果想看 SD 的 API 文档，可以使用 http://xx.xx.xx.xx:7880/docs 的形式 （换成你自己部署了 SD 的服务器 IP），文档部分截图如下：

![](img/835ba1c71e286443a690830ea3782834.png)

我们常用的文生图和图生图功能，分别对应上面文档截图里的 /sdapi/v1/txt2img （文生图接口） 和 /sdapi/v1/img2img （图生图接口） 这两个 POST 接口，下面我会重点介绍图生图接口的请求入参和回参。文生图接口和图生图接口的主要区别在于图生图接口的请求入参多了用户自己上传的原始图片，其余的相差不大。如下所示，一些重要参数的描述会以代码注释的方式进行阐述，大家可以打开自己的 SD Web-UI 页面来对照看看。

{

"init_images": [

"string"

],

"resize_mode": 0,

"denoising_strength": 0.75,

"image_cfg_scale": 0,

"mask": "string",

"mask_blur": 4,

"inpainting_fill": 0,

"inpaint_full_res": true,

"inpaint_full_res_padding": 0,

"inpainting_mask_invert": 0,

"initial_noise_multiplier": 0,

"prompt": "",

"styles": [

"string"

],

"seed": -1,

"subseed": -1,

"subseed_strength": 0,

"seed_resize_from_h": -1,

"seed_resize_from_w": -1,

"sampler_name": "string",

"batch_size": 1,

"n_iter": 1,

"steps": 50,

"cfg_scale": 7,

"width": 512,

"height": 512,

"restore_faces": false,

"tiling": false,

"do_not_save_samples": false,

"do_not_save_grid": false,

"negative_prompt": "string",

"eta": 0,

"s_min_uncond": 0,

"s_churn": 0,

"s_tmax": 0,

"s_tmin": 0,

"s_noise": 1,

"override_settings": {},

"override_settings_restore_afterwards": true,

"script_args": [],

"sampler_index": "Euler",

"include_init_images": false,

"script_name": "string",

"send_images": true,

"save_images": false,

"alwayson_scripts": {}

}

{

"images": [

"string"

],

"parameters": {},

"info": "string"

}

如果你仔细对照上面图生图接口的请求参数和 SD Web-UI 页面会发现，好像少了些参数，一些在网页上的存在的选项没有出现在图生图接口的请求参数里，比如 SD 的 checkpoint 大模型和 CLIP 终止层数等等，那么这些参数要在哪里去查询呢？这里我们可以用到 /sdapi/v1/options 这个 GET 接口来做查询，请求入参 （一般为空）和部分回参如下所示。

{

"outdir_txt2img_samples": "outputs/txt2img-images",

"outdir_img2img_samples": "outputs/img2img-images",

"sd_model_checkpoint": "string",

"sd_checkpoint_cache": 0,

"sd_vae_checkpoint_cache": 0,

"sd_vae": "Automatic",

"sd_vae_as_default": true,

"comma_padding_backtrack": 20,

"CLIP_stop_at_last_layers": 1,

"upcast_attn": false,

"randn_source": "GPU",

"quicksettings_list": [

"sd_model_checkpoint"

],

"ui_reorder": "inpaint, sampler, checkboxes, hires_fix, dimensions, cfg, seed, batch, override_settings, scripts",

"sd_checkpoint_hash": ""

}

### 2、接口测试脚本

了解完一些基本的 SD API 接口后，接下来我会提供调用文生图和图生图这两个接口的 Python 脚本，在脚本的请求入参里只附带了一部分参数，可以根据需要自行增减。大家如果有部署 SD 的云服务器 IP，可以直接使用下面的 Python 脚本进行测试。

// 这是文生图接口的脚本代码

import json

import requests

import io

import base64

from PIL import Image, PngImagePlugin

url = "http://xx.xx.xx.xx:7880" // 换成你自己的服务器ip和端口

// 请求参数根据自己的需要做修改

payload = {

"sd_model_checkpoint" : "xsarchitectural_v11.ckpt" ,

"CLIP_stop_at_last_layers" : 2 ,

"prompt" : "water village,jiangnan <lora:wuguanzhong_20230810213551-000020:1>" ,

"steps" : 25 ,

"seed" : - 1 ,

"width" : 512 ,

"height" : 512 ,

"sampler_name" : "DPM++ 2M Karras"

}

response = requests.post(url= f'{url}/sdapi/v1/txt2img' , json=payload)

r = response.json()

for i in r[ 'images' ]:

image = Image. open (io.BytesIO(base64.b64decode(i.split( "," , 1 )[ 0 ])))

png_payload = {

"image" : "data:image/png;base64," + i

}

response2 = requests.post(url= f'{url}/sdapi/v1/png-info' , json=png_payload)

pnginfo = PngImagePlugin.PngInfo()

pnginfo.add_text( "parameters" , response2.json().get( "info" ))

image.save( 'output.png' , pnginfo=pnginfo)

// 这是图生图接口的脚本代码

import json

import requests

import io

import base64

from PIL import Image, PngImagePlugin

url = " http://xx.xx.xx.xx:7880" // 换成你自己的服务器ip和端口

def img_to_str ( image_path ):

with open (image_path, "rb" ) as image_file:

encoded_string = base64.b64encode(image_file.read())

return encoded_string.decode( "utf-8" )

image_path = "image.jpg"

image_string = img_to_str(image_path)

// 请求参数根据自己的需要做修改

payload = {

"init_images" : [

image_string

],

"sd_model_checkpoint" : "xsarchitectural_v11.ckpt" ,

"CLIP_stop_at_last_layers" : 2 ,

"prompt" : "water village,jiangnan <lora:wuguanzhong_20230810213551-000020:1>" ,

"steps" : 25 ,

"seed" : - 1 ,

"sampler_name" : "DPM++ 2M Karras" ,

"cfg_scale" : 7 ,

"width" : 512 ,

"height" : 512 ,

"denoising_strength" : 0.52

}

response = requests.post(url= f'{url}/sdapi/v1/img2img' , json=payload)

r = response.json()

for i in r[ 'images' ]:

image = Image. open (io.BytesIO(base64.b64decode(i.split( "," , 1 )[ 0 ])))

png_payload = {

"image" : "data:image/png;base64," + i

}

response2 = requests.post(url= f'{url}/sdapi/v1/png-info' , json=png_payload)

pnginfo = PngImagePlugin.PngInfo()

pnginfo.add_text( "parameters" , response2.json().get( "info" ))

image.save( 'output.png' , pnginfo=pnginfo)

## 二）卷积神经网络进行风格迁移

这一部分涉及到项目里的敏感部分，不会列出具体的代码和训练过程，着重介绍背景和思路。

### 1、背景介绍

在艺术绘画创作过程中，人们通过将一张图片的内容和风格进行复杂的相互交融，以产生独特的视觉体验。然而，所谓的艺术风格是一个抽象且难以明确定义的概念。因此，如何将一个图像的风格转换成另一个图像的风格是一个复杂且抽象的问题。特别是对于机器程序而言，解决一个定义模糊的问题几乎是不可行的。

在神经网络出现之前，图像风格迁移的程序通常采用以下思路：首先分析一种风格的图像，建立这种风格的数学统计模型；然后调整要进行迁移的图像，使其风格符合建立的模型。这种方法可以得到不同的效果，但存在一个较大的缺陷：一个模型只能实现一种图像风格的迁移。因此，基于传统方法的风格迁移模型的应用受到一定的限制。

随着[神](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Gatys_Image_Style_Transfer_CVPR_2016_paper.pdf)经网络的发展，机器在某些视觉感知的关键领域，比如物体和人脸识别等方面，已经取得了接近甚至超越人类的表现。在这里，我会介绍这个领域的开山鼻祖之作，Gatys 等人发表的一篇论文 《Image Style Transfer Using Convolutional Neural Networks》 。该文章介绍了一种利用卷积神经网络进行图像风格迁移的算法，相比于传统的风格迁移方法，该方法具有更好的普适性。我们的项目是在这篇论文的基础上做了一些特定的优化，让我们一起看看这篇论文的主要内容。

### 2、概述

#### 1）内容表示

处理图像任务最为有效的深度神经网络之一是卷积神经网络。它是一个前馈神经网络，由多个网络层组成，每个网络层包含了许多用于处理视觉信息的计算单元（神经元）。每一层的计算单元可以看作是一组用于处理图像的滤波器，它们可以提取出图像的特定特征。我们把给定层的输出称为特征图谱（Feature Map），即输入图像在经过该层后得到的不同过滤版本。

当卷积神经网络用于物体识别时，随着网络层次的逐渐加深，网络层产生的物体特征信息变得越来越清晰。这意味着沿着网络的层级结构，每一个网络层的输出更加关注输入图片的实际内容，而不是具体的像素值。通过重构每个网络层的特征图谱，我们可以可视化出每一层对输入图片所表达的信息。通过这样的方法，我们可以观察到位于更高层的网络层能够根据物体及其在输入图像中的排列方式来捕捉输入图像的高级内容，而不包含具体的像素值信息。因此，我们可以参考网络模型的高层结构作为图片的内容表示。

#### 2）风格表示

为了获取输入图片的风格表示，我们采用一个专门用于提取纹理特征的特征空间。这个特征空间包含了在特征图谱中不同滤波器响应之间的相关性信息。通过对多个层的特征相关性进行处理，我们得到了输入图像的静态多尺度表示，它能够捕捉到图片的纹理特征，但并不包括全局排列信息，下图为利用卷积神经网络提取图像内容和风格的示意图。

![](img/842e43c5084709aafeb34f967dce9e9c.png)

#### 3）内容和风格的分离

这里的一个关键观点是，卷积神经网络中的图片内容表示和风格表示可以分开处理。这意味着我们可以独立地操作这两种表示来生成新的富有感知意义的图片。为了验证这一观点，该论文展示了一些通过合成不同内容和风格的图片而生成的合成图片，如下图所示。

![](img/fe7bc5bc1ce38f913f4d53dac2c90565.png)

这些合成图片是通过寻找同时匹配照片内容和对应艺术风格的图片的方法生成的。它们在保留了原始照片的整体布局的同时，也继承了各种艺术图片的不同风格特征。风格表示是一个多层次的表达，包括了神经网络结构的多个层次。当风格表示只包含了少量的低层结构时，风格变得更加局部化，产生不同的视觉效果。而当风格表示由网络的高层结构来表示时，图像的结构会在更大的范围内与这种风格匹配，产生更加连续流畅的视觉体验。

#### 4）图片的合成

实际上，图片的内容和风格是不能够被完全分离的。当我们合成图片时，我们通常找不出一张能够匹配某个图片内容和另一种图片风格的图片。在我们合成图片的过程中，我们需要最小化的让损失函数包含内容和风格，但它们是分开的。因此，我们需要平滑地调整内容和风格的权重比例（如下图所示）。当损失函数分配在内容和风格的权重不同时，合成产生的图片效果也完全不一样。我们需要适当地调整内容表示和风格表示的权重比来产生具有视觉感染力的图片，是否能够找到合适的权重比是能否产生令人满意的图片的关键因素。

![](img/eb124dcd2e63ce73f8eb3566023f1659.png)

### 3、实验

该论文中的实验结果，是以 VGG 网络为基础产生的。该实验使用的是由 16 个卷积层和 5 个池化层（VGG 19）组成的特征空间。由于该实验不要进行分类，我们不需要使用全连接[层。该模型是公开可用](https://github.com/Kautenja/a-neural-algorithm-of-artistic-style)的，我们可以在 Caffe 框架和 Keras 框架找到，作者实验发现使用平均池化比使用最大池化更容易得到令人满意的实验结果。这里不进行具体的代码分析，想要钻研的朋友可以参照这个 GitHub 链接 。

# 三、案例效果演示

通过上传特定画家的数据集来进行 LoRA 模型的炼制，目前炼制了吴冠中老师的江南水乡风格，徐悲鸿老师的骏马图风格，还有其他形式的艺术风格迁移总计一百多种。高质量的数据集和打标对于炼制 LoRA 模型是至关重要的，建议有类似需求的小伙伴可以直接去拍卖行找高清的画作图片来做训练集。

需要补充的是，炼制 SD LoRA 模型这种做法应该叫绘画风格重绘，每次生成的图片会有些许不同，对于喜欢尝试新鲜事物的用户来说，每次生成不同的画作是一种乐趣，但是对于渴望确定性的用户来说，这种每次生成图片不一样就会是一种困扰了。因此，除了通过 SD 来实现风格转化，相当一部分风格转换的滤镜我们是通过上面所说的卷积神经网络的风格迁移训练来实现的，这种方式可以带来更高的确定性和稳定性。下面会分别列出用 SD 风格转换生成的江南水乡风格和骏马图风格，以及通过卷积神经网络训练生成的印象派和抽象主义风格。

## 一）吴冠中的江南水乡风格

提示词很简单，就是 **w**ater village, jiangnan <lora:wuguanzhong_20230810213551-000020:1> 。 关键词是 water village，这个是在对数据集进行打标的时候就会手动打上的标签，另外从训练出来的二十个 LoRA 模型中挑选效果最好的一个，也一起放在提示词里。下图是使用江南水乡风格进行重绘的前后图片比对，需要注意的是受限于训练的数据集，只有当输入的原始图片是风景照的时候，此水乡风格才会有比较好的效果。

![](img/674c9817c98ea65e30a902195a9eb44e.png)

## 二）徐悲鸿的骏马图风格

**同样**是基于 SD LoRA 模型，这里的提示词是 chinese ink painting, brush painting, monochrome, greyscale <lora:xubeihong_20230810115020-000019:1> 。 下图是使用骏马图风格进行重绘的前后图片比对，需要注意的是受限于训练的数据集，只有当输入的原始图片是动物的时候，此骏马图风格才会有比较好的效果。

## 三）印象派和抽象表现主义风格

下面为目前 AIGC 艺术创作小程序里包含的一百多种风格里的一部分，做了一些马赛克处理。其中大多数是一些印象派和抽象表现主义的风格，通常会有强烈的色彩和纹理对比。这一部分图像风格转换是通过上述所说的卷积神经网络训练生成的，使用同一种风格对同一张照片做风格转换，不会出现上面 SD 风格重绘生成不同画作的情况，稳定性更强，创意和趣味性弱一些。

# 四、遇到的问题和解决方案

遇到的问题大体上可以分为两类，一类是做什么的问题，另一类是如何做的问题，这两类问题贯穿整个产品研发阶段。通俗点讲就是，你现阶段有什么，想要什么，为了达成目标打算怎么做。

## 一）做什么 - 产品定位的问题

做什么，也就是产品定位的问题其实本文第一部分有提到。首先，要去做市场调研，不能拍脑袋说做这个做那个，需要把国内外市场上已经有的产品都调研一下，包括未来可能爆发的赛道这些都要提前预研。其次，要从众多有潜力的赛道中选择和你当下手头资源可以匹配上的，有些赛道听着很美好，可你手头上资源压根支持不了，那就不要强行去做。清楚自己当下资源能做到的事情，先跑通一个 MVP 产品，然后滚雪球一步一步向外扩展和做大。最后，做一个商业化产品前要先把商业模式想清楚，你要卖什么东西给用户，想清楚这个然后以终为始来反推，每一步要做到什么程度。

## 二）如何做 - 优化用户体验的问题

这里挑一个最有代表性的问题来说，不管用 SD 还是自训练的卷积神经网络来做图片的风格转换，处理速度都是一个挺大的问题。如果图片本身比较大，比如大于 5MB 的话，处理时间一般要在 10 秒左右，让用户盯着小程序 10 秒不做其他事，是个挺难的事情，很多人会因为等待时间过长而退出的。

要如何解决这个问题呢？首先要分析整个链条上有可能消耗时间的节点，在我们这个场景里有以下三部分：上传图片到云端、云端做风格转换的处理、云端**处理**后的图片再推送给小程序做用户展示。这里因为微信小程序本身是不支持太大的资源缓存和处理，所以实际做风格转换是在云服务器上进行的。这个链条上可以优化的点，包含 对图片进行一定程度的压缩、云端服务器和小程序服务器就近原则放在一个区域内、程序里支持并行处理不同用户的请求减少排队、训练算法本身做参数调优和模型优化、支持 CDN 存储图片缓存 等等。

以上的方案都是在技术角度上尽可能缩短图片风格转换处理的时间，这些全部做完后大概能维持在 5 秒左右的处理时长。这时候需要在 UI 上做一些有趣的提示来管理用户的预期，可以是简明有趣的加载图标也可以是一些提示标语，目的是让用户明白做图片的风格转换，或者 换句话说等待艺术的创作，是需要耐心和时间的 。

# 五、结尾

整个 AIGC 艺术创作项目目前还在初始阶段，预计十一之后会进行小范围的内测，到时候有了比较多具体的用户数据和反馈可以再进行一次产品和运营的复盘。经过这几个月的市场调研和实际的产研，我发现当前各种 AIGC 的功能和相关应用非常多，缺的不是功能，是怎么把功能更好地和用户的需求贴合在一起，做减法比做加法更难。洋哥志愿者分享的时候也说过，AIGC 应用明年将会引来大爆发，小伙伴们可以提前布局，大家一起讨论和分享，一个人可能走得很快，但一群人一定走得更远。

最后，感谢小林老师和所有志愿者，在假期之间，还帮我们批改内容，你们用你们的辛勤付出来成就我们，感谢你们的无私奉献，感谢！