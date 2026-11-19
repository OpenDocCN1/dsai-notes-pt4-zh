
# 3套爆款书单号视频工作流

> 来源：[`https://zinxkclpk97.feishu.cn/docx/ARCvd1Vy2oZJcdxI0MZc33ZFnzd`](`https://zinxkclpk97.feishu.cn/docx/ARCvd1Vy2oZJcdxI0MZc33ZFnzd`)

![](img/069a85b883e9519a7b9f29d4f7955783.png)

[`翻书特效书单视频案例讲解-1.mp4`](`https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RIFqb6f0gopzAaxeZDfcH6ivnzh`)

在此之前我们已经分享了节奏卡点和打字机音效的书单号视频案例，感兴趣的小伙伴可以回顾下。

![](img/62cd454c4c1f2fffdff7461d28af3e2b.png)

![](img/dbd207747eac1bfa6a62b75b0752e3f3.png)

今天分享的书单号视频工作流案例，带有更多的特效，先看视频。

![](img/68ac44564d9a1b8bda1ec30ec2d65728.png)

[`fa17146a-b138-45d1-86c9-3f2e9b483918.mp4`](`https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HvYPbC32WozJCzxZptMcCHaanGe`)

想要体验的小伙伴可以直接跳到文章末尾获取体验链接。

## 案例拆解

我们逐帧分析视频细节，其中包含了开场翻书动画、翻书音效、模糊背景、开场配音等元素，衔接部分还采用了切开的转场特效。

![](img/857e5f545048e96e545c3df2f685a63a.png)

视频主体部分，包括书籍大字、顶部字幕、正文字幕、配音、背景音乐、星火特效等。

![](img/f8ea35859cdd94d44afd9896ee85db22.png)

这次的工作流方案我们挑选核心内容来分享，如果想要完整工作流的，可评论区留言。

之前一些视频工作流案例中，字幕拆分后再配音，导致配音出来的效果不好，听起来不连贯。在这次案例中我们也会给出解决方案，方便大家迁移学习使用。

## 工作流设计

1. 输入标题/文案、背景音、封面图等素材信息
2. 文案分段、文案转语音
3. 字幕语音对齐处理、构建剪映草稿数据
4. 数据重组、生成剪映草稿和视频

![](img/579c357d1bf708d2288b7f4dd716cf1d.png)

## 工作流搭建

首先输入节点，考虑到有自定义文案和背景音的需求，开放多个参数允许用户自定义。

![](img/25d8e4f8593a7cbd0a7c4344516f19f1.png)

下一步，模型生成文案，并进行段落拆分，用于后续分段转语音。

![](img/0759a9051e388c672251e7d2e7bd3f84.png)

大模型部分参数设置，注意输出参数格式

![](img/a3934621cd4ba890b9deca27134bd5d2.png)

```
# 经典书籍金句推荐
你是一位专业的书评人，清模仿董宇辉的风格，根据用户提供的书籍名称，撰写出专业且有深度的金句评论，输出 200字左右的顶级感悟文案。
#格式
输出文案，合理分段以数组列表输出每段内容
并给出书籍名称、作者名称
```

接下来处理其他文案

看下整体流程

![](img/9e10e55db3d9c0daa8cd8997011b3dab.png)

### 开篇开场白语音转换

![](img/4189ccdf0d050e127eb36d8a18cb97d2.png)

### 背景音时长计算

![](img/baa2b960ef350b0af1d964c77cdff7a8.png)

### 顶部标题作者拼接

![](img/7cfe9b78ebfb1d5364a36cac13650f70.png)

### 背景图生成

![](img/588855baaaf5f48749e6bebf7cd27f25.png)

然后我们需要提前计算一次开场白时间线数据，后续再拼接剪映草稿其它轨道数据。

示例：开场白“今天我们分享的是《XXX》”，通过代码节点，计算其音频时长，并转换单位方便后续使用。

![](img/0e253c945ff0f4bcec717c1d2e9fd476.png)

继续，我们使用循环节点，对分段的文章进行处理。

主要实现段落文案转音频，拆分字幕，字幕音频对齐，生成音频与字幕的时间线数据

![](img/ad1d0cb162ab02ebfcc902fd10344b1f.png)

### 循环节点

![](img/996b4e05308d46d58cc92c48f526fa7e.png)

### 根据标点符号进行分句

![](img/5c0cb68bf8af17c70b3eddd07763b15c.png)

### 段落文案转音频

![](img/dd26e2f0d5bd6e216fa5a1ea5447b4cf.png)

音频与字幕对齐，这样可以保证语音的连贯性。

![](img/993482a4a81b820b354440f9c1d1d86b.png)

代码组合，将前置数据进行组装，生成剪映草稿的音频、字幕数据对于的结构，有点复杂，细节不展开了。

![](img/32d43a3fa0b2c82b4c921f44ca1ac8b7.png)

```
import json
import random
async def main(args: Args) -> Output:
    params = args.params

    #固定参数
    width = params['width']
    height = params['height']

    #中间变量
    captions = params['caption_list']
    audios = params['audio_list']

    #每次循环的动态参数
    timelines = params['timelines']
    audio = params['audio']
    texts = params['texts']
    duration = params['duration']
    
    #中间变量
    pre_time = params['pre_time']

    # 每段配音计算
    audios.append({
        "audio_url": audio,
        "duration": int(duration * 1000000),
        "start": pre_time,
        "end": pre_time+int(duration * 1000000),
        # "audio_effect":"环绕回声"
    })

    #计算时长，设置字幕的时长
    for index2 , obj in enumerate(timelines):
        captions.append({
            'text': texts[index2],
            'start': int(pre_time + obj['start']),
            'end': int(pre_time + obj['end']),
            "in_animation":"渐显","out_animation":"渐隐"
        })
    # 构建输出对象
    # 重置pre_time为此次处理的结束时间
    ret = {
        "captions": captions,
        "audios": audios,
        "end_time":pre_time+int(duration * 1000000)
    }

    return ret
```

完成前置数据的准备工作，我们还需要对固定的元素进行拼接排列，比如背景图、特效、转场、音频等等。

![](img/f3f6166cdc800fb8e4517a138fb9ae76.png)

### 组合结构-代码节点

放一个长图，参数比较多，有需要的复刻后再学习。

![](img/40fbf703d948165a863cd035655ffb2e.png)

最后则是对准备好的数据进行组装，拼接剪映草稿即可。

![](img/c97df2d0f7919807eadc1b62783a9da2.png)

## 案例演示

这个书单视频智能体也已经发布到了Coze商店，目前可以免费体验试用。

只需输入标题，轻松生成书单爆款视频

![](img/b412154f90e1377a5a779aae04537dc0.png)

生成后将获得草稿地址和云端渲染的视频链接。

+ 剪映草稿地址，可以使用剪映小助手，导出到本地，进行二次编辑。

+ 云端渲染的视频链接，可以实现和本地剪映一样的视频效果，省去了剪映会员费用

![](img/fc8574efaaa0e2d17b281655726c0f81.png)

目前这款智能体，开放了三个书单视频案例，快来欢迎体验吧。有需要工作流的也可以留言滴滴我。

![](img/55125084d8cec525be8463d8a854a9c9.png)

体验地址：

`https://www.coze.cn/s/CZaYY-Se91I/`

更多视频工作流案例：

`https://www.coze.cn/store/project/7475749839012118555`
