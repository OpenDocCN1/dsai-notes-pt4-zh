# Flux+Redux+Tora

> 来源：[https://ai.feishu.cn/docx/Ata3dGSBioTpHfxIDD5ctwMdn1b](https://ai.feishu.cn/docx/Ata3dGSBioTpHfxIDD5ctwMdn1b)

####

Flux更新了个redux风格模型，效果非常不错。。

#### 效果篇

![](img/1b7977d4875f102083132baa5a0ba9fb.png)

> CogVideoX增加运动点跟随插件后，可以精确到物体的变化

提示词：The little tree grows up slowly

![](img/435ac631ed6b63c501bbad5de368e95e.png)

[a5d0c356-1c75-4ab0-9e80-f3485b35be1b.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HCp4bdq0qosnU5xtBgLcaktengb)

#### 其它风格融合的画面

##### 效果1

左边物体图，右边场景图


![](img/194a57a3f167f24131a54c8194283f17.png)

效果图

![](img/1eb26d29dfd86a9475cd539306aea89d.png)

##### 效果2

左边物体图，右边场景图

![](img/c4c95b6fe2d6f6cd8fd27628d730cdc5.png)

![](img/65070ab3355e6a105f6e3e57dc597ed0.png)

效果图

![](img/8d2b86e47bd1cadb975a6ab3c92856bf.png)

##### 效果3

左边物体图，右边场景图


![](img/30795b5fa9c5a97c16c17e30bf3c8836.png)

效果图

![](img/588431bff49611575231c27aedc66d2b.png)

流程图预览

![](img/989b5b9f669ba518c470fba804b65a46.png)

#### 流程图

分成了两个,Flux+Redux流程用来处理自己想要的融合的图片，两张图片尽量保持风格一致就可以了，流程图中的提示词，有能力的同学可以自己去改源码，修改权重，当前的这个Flux+Redux流程  提示词无任何作用....

CogVideoX+Tora插件流程用来选择图片中的物体，提示词可以描绘物体的运动形式，例如运镜，物体形状、远近，Spline Editor, 这个节点用来选中图片中的Objects,选中后物体按照提示词描述的形式执行对应的行为

####

> Flux+Redux流程

![](img/a985e3e4d3eca01a3517f016121ec352.png)

[redux生图(1).json](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QGYbbwhgmo4zd2xagn5cIGOCnJd)

> CogVideoX+Tora插件流程

![](img/07a2498382f96c626546a5b9f4f4e57c.png)

[cogvideox_5b_Tora_I2V_testing_01.json](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GU0CbPNzfoD5ZRxXjzxconWonhd)

#### 环境说明

> 记得更新你的ComfyUI, Flux的Redux修改了模型结构，“不更新无法使用”

##### 节点安装

环境和模型下载预计需要三小时，网速+20M

###### 环境

正常flux+redux,使用默认的comfyui管理器安装不存在的节点就可以解决

CogVideoX和Tora则需要使用源码安装

> Github: https://github.com/kijai/ComfyUI-CogVideoXWrapper

> 安装目录：custom_nodes

> 操作：下载这个仓库的代码到custom_nodes下面，进去custom_nodes/ComfyUI-CogVideoXWrapper目录，执行·pip install -r requirements.txt· 安装依赖

##### 模型下载

[h](https://comfyanonymous.github.io/ComfyUI_examples/flux/)ttps://comfyanonymous.github.io/ComfyUI_examples/flux/

**CheckPoint基座模型必须是Flux1.**0版本，不然会出现”维度转换错误的问题“

![](img/bf0833e23659f8e7676f3f587afc1e10.png)

**Flux相关模型地址**在这里，如图片所示，

下载 [sigclip_vision_patc](https://huggingface.co/Comfy-Org/sigclip_vision_384/blob/main/sigclip_vision_patch14_384.safetensors)h14_384.safetensors 模型放入ComfyUI/models/clip_vision 文件夹中，

下载 flux1-redux-dev.safetensors 并放入ComfyUI/models/style_models 文件夹中

![](img/c462e9e4ac97849c6f993d79a8928740.png)

CogVideoX的模型默认会自动下载模型，这里选择的时图生视频的模型CogVideoX5B_I2V模型

clip模型可以选择t5xxl_fp16，在ComfyUI管理器中下载，下载地址t5xxl_fp16.safetensors

##### 效果篇

+   生成的视频有红点？

这是因为红点是辅助Spline Editor的运动轨迹有没有正确选择物体，不想显示删除ImageCompositeMasked就可以了

![](img/df38a352e6baa277a4e69ef692fee163.png)

+   CogVideoX5B显存不够？

5b模型正常需要24G的显卡，如果开启了cpu_offload和VaeTiling,显存只需要9G

+   Flux显存不够？

选 [f](https://huggingface.co/Comfy-Org/flux1-dev/blob/main/flux1-dev-fp8.safetensors)lux1-dev-fp8.safetensors 低精度的模型，效果无任何变化

![](img/20da45d6ebcf781923105e9cf298a06f.png)