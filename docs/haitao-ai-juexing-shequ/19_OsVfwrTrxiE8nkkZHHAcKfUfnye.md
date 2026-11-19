
# 书单号-读书分享视频制作

> 来源：[https://zinxkclpk97.feishu.cn/docx/NehidrIB5oLzJhxaAlgcT4Zgn0E](https://zinxkclpk97.feishu.cn/docx/NehidrIB5oLzJhxaAlgcT4Zgn0E)

> 写在开始，近期我的爆款短视频应用已经上架到的了Coze应用市场，包含之前👸号分享的所有视频模板，欢迎来体验试用。文末点赞获取体验链接🔗。

## 作品分析与需求拆解

![](img/5f98f54c512fe5216e23011f45e701ee.png)

你有没有在短视频平台上看到此类书单视频，优秀的文案搭配高级感的配色，结合情绪化的音乐，流量非常高。

这里我们找到上面的案例，对其进行拆解，并使用Coze工作流来制作视频这类视频。

视频中包含的元素有书籍名称、作者、句子、深色背景、背景音乐。

到这里我们思考下，我们的需求，输入书籍名称，直接生成短视频。

而在Coze工作流中，实现上述目标，哪些动作可以被设定和标准化？

这里我们先拆解工作流，然后再使用Coze搭建实现。

总体流程还是比较简单的，其唯一的难点之处，在于视频数据结构的代码部分。因为需要把握字幕时间、背景音乐的节奏。所以在复刻的时候，也需要多一些耐心，精细调整。

## Coze工作流搭建

接下来，看下使用Coze如何搭建完整的书单号视频工作流

![](img/658fea1626f507e2b2e352f5d1e3d592.png)

其中插件用到了“豆瓣搜书”

![](img/beb9e152d75c4540c1b9e07965d3477f.png)

另外的“剪映小助手”，在我们历史的文章中也有讲解，可以回顾下之前的文章。

另外大模型用来帮助我们生成书籍相关的短句，这里我们选用的是通义千问-max模型，当然你也可以适配其它的模型，调整合适的提示词即可。

![](img/c65bb95814d784c6787addf3513bf6ec.png)

### 提示词配置

```
# 角色
你是樊登老师，一名资深读书爱好者，极为擅长书籍拆解以及知识内化分享。能够根据用户提供的书籍名称，深入剖析书籍内容，精准提炼出其中的经典段落，并为分享拟定恰当的标题。

## 技能
### 技能 1: 分享书籍感悟及标题
1. 从书中挑选出经典句子进行分享。
3. 针对这些句子进行理解，精炼出7句书中经典格言，每句20-40字左右。
4. 内容精炼，长短句组合，并对合理分段，提供中英对照版本。以数组形式输出
===回复示例===
- **分享标题**：[具体标题]
- **经典内容**：[具体句子内容，英文翻译]
===示例结束===

## 限制:
- 只讨论与书籍相关的内容，拒绝回答与书籍无关的话题。
- 所输出的内容必须按照给定的格式进行组织，不能偏离框架要求。
- 请使用工具确保搜索信息来源准确，输出内容要基于搜索到的可靠资料。 
```

## 素材重组与代码实现

最重要的代码部分，我们实现对素材的重组，拼接剪映各个轨道所需要的素材：字幕、背景音、特效等。

这里也需要反复调试，设定好每段字幕之间的时间间隔，与背景音乐进行卡点。

![](img/f7473c5244d52f8071bc5e6f9f5fd9a4.png)

### 核心代码

```
import json
import re
async def main(args) -> dict:
    params = args.params
    sentences = params['sentences']
    audio_url = params['audio_url']
    title = params['title']

    #文案字幕
    captions = []
    #标题作者
    captions2 = []
    audios = []

    current_time = 0  # 时间轴指针

    # 遍历所有元素（假设四个数组长度一致）
    for idx in range(len(sentences)):
        chinese = sentences[idx]['chinese']
        if chinese is None or len(chinese)==0:
            continue
        english = sentences[idx]['english']
        # 计算时间区间
        #字幕切换间隔时间
        duration = 3.1*1000*1000
        # duration = sentences[idx] * 1000000  # 转为微秒
        end_time = current_time + int(duration)
        # 构建字幕轨道
        captions.append({
            "text": re.sub(r"([，。])(?=[^\n]*[，。])", r"\1\n", chinese),
            "start": current_time,
            "end": end_time,
            "in_animation":"渐显",
            "out_animation": "渐隐"
        })
        # 移动时间轴指针
        current_time = end_time
    audios.append({
            "audio_url": audio_url,
            "duration": current_time,
            "start": 0,
            "end": current_time
        })
    captions2.append({
        "text": title,
        "start": 0,
        "end": current_time,
        "in_animation":"渐显"
    })
    effects = []
    effects.append({
        "effect_title": "金粉闪闪",
        "end": current_time,  
        "start": 0
    })
    # 构建输出（保持JSON序列化）
    return {
        "captions": json.dumps(captions),
        "captions2": json.dumps(captions2),
        "audios": json.dumps(audios),
        "effects":json.dumps(effects),
    }
```

## 视频渲染与效果展示

完成素材的准备，后面就是使用剪映小助手插件，对内容进行串联，生成剪映草稿，并进行线上渲染生成视频。

![](img/3a4a34bd4b09d7db629e8bfced5d7b4f.png)

最后展示下直出的视频效果，是不是还不错。

![](img/38432e14797d4341e18facb9c91ec459.png)

[e43157c0-3466-438c-a422-8760ac325b28.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/G8iibngMdoWZV8xLPcIcfJBznih)

## 体验入口

这套书单号视频工作流也已经在我的爆款短视频创作营上架，欢迎体验。

![](img/ceced0de3baa96d19afc35d9ec7d73dc.png)

也上架了另外一种风格类型，打字机音效书单视频。

![](img/143a7c556be6348c59ac1f622802e2ff.png)

![](img/60526ac456f5257b6f3427c334d7b641.png)

[985c84b1-a5b9-482e-8c27-404ced7d4b18.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XCJ5bN3PMoe3NwxPhZmcsZpQnoh)
