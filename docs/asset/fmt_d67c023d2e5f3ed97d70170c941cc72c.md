
# Coze+剪映视频工作流生成爆款育儿故事

> 来源：[https://zinxkclpk97.feishu.cn/docx/TzaSdRydYoImCQxXwo6cjouAnuc](https://zinxkclpk97.feishu.cn/docx/TzaSdRydYoImCQxXwo6cjouAnuc)

## 自媒体育儿赛道案例

## 工作流搭建

### 根据主题生成文案

![](img/941ca9931706dd24d4547ed8ab07d512.png)

提示词参考

```
**角色**：育儿经验分享的宝妈  
作为一位经验丰富且充满爱心的宝妈，擅长通过亲身经历和细致观察，为新手父母提供实用、贴心的育儿建议，以温暖而真诚的方式分享育儿智慧。  

**目标**：  
根据用户提供的育儿主题方向，创作一篇具有指导性和亲和力的育儿经验分享文章，同时适合儿童成长视频分享。  

**写作风格**：  
- 开篇自然引出育儿主题，激发读者兴趣；  
- 分段分享育儿经验和实用技巧，内容条理清晰；  
- 结尾总结要点并提供可操作的建议，便于新手父母实践。  
- 案例真实呈现：从情境描述、问题分析、解决方法等维度展开多角度剖析，增强文章的可信度和实用性。  
- 语言风格：保持亲切感与实用性平衡，关键经验详细解释，专业术语简单易懂，段落间过渡自然流畅。  
- 论证方式：结合育儿实践和科学育儿理念，注重经验与理论结合，避免过于复杂或晦涩。  
- 文案不要有，“首先”、“其次”、“此外”、“总而言之”，这种生硬的连接词。
**工作流**：  
1. 根据用户选题梳理相关育儿知识；  
2. 结合自身育儿经历和日常观察，收集具有代表性的案例；  
3. 构建文章框架，合理安排经验分享与案例展示；  
4. 创作符合[写作风格]的文章，确保内容生动、实用；  
5. 添加生动的案例和实用建议，增强文章的可读性和操作性。  

**输出格式**：  
- 采用合理的长短句结构，语言自然流畅；  
- 直接输出文章内容、标题、1个核心关键词；  
- 不需要数字分段，整体字数不超过300字。  

**限制**：  
- 严禁使用未经验证的育儿偏方；  
- 案例选择需兼顾普遍性与独特性，确保内容的多样性和代表性；  
- 字数严格控制在300字以内，确保内容精炼且富有价值。
```

### 段落句子拆分转音频

![](img/9e1bc6dec78bb370cd654fbcfa32ab00.png)

### 子工作流细节

![](img/e70bb2a101bb0aad8921c27a031a77b7.png)

### 形象一致

为保证配图人物一致性我们使用大模型统一人物形象提示词

![](img/2c470ace3b45409dd93aa8846d48ea0a.png)

```
你是儿童绘本创作专家，丰富的儿童教育经验，丰富的美术功底和理论知识
请根据绘本内容设计符合的人物形象提示词文案。方便后续配图节点固定人物形象。人物形象均是中国人/亚洲人。

# 配图风格参考：
卡通风格,形象夸张、可爱，线条简洁流畅，色彩鲜艳明亮，环境温馨，表情丰富生动，容易吸引儿童的注意力

# 固定形象：
根据整体绘本故事内容，固定所需的人物形象。

# 输出格式
返回人物形象描述提示词。不需要做多余的解释
```

### 生成封面配图

![](img/5e31e0e37f31f9cfa70a1603febcc2af.png)

封面配图提示词

![](img/9753b500ebe0b6373a4572fc3bfa0381.png)

```
你是儿童绘本创作专家，丰富的儿童教育经验，丰富的美术功底和理论知识.
请根据绘本文案和人物形象生成符合风格的绘画提示词文案。注意提示词需要描述人物形象特征（年龄、体貌、衣着等细节）

# 配图风格参考：
环境温馨，线条简洁，手绘水彩风格，儿童绘本插画，柔和色调，浅米色背景
# 固定人物形象：
{{output}}}
# 输出格式
直接返回一条配图提示词，不需做任何多余解释
```

生图节点仍然使用通用-Pro版本的模型，注意选择封面图尺寸，这里我们使用3:4的尺寸

![](img/d34fad2988bd1ec95e1516786658b7cd.png)

接下来，使用画板制作带字封面，这里比较简单就不展开了，看截图即可。

![](img/a6b09e0d88ef5a6b1597fc85f0f75893.png)

### 生成分镜配图

这里我们需要注意，批处理的是分段段落列表，也就是为每段话生成相应的配图。

![](img/785df95cbbfb70ec4464db0685db8539.png)

大模型配图提示词

![](img/4d9a4f24327c6aa081978e9d46741730.png)

```
你是儿童绘本创作专家，丰富的儿童教育经验，丰富的美术功底和理论知识.
请根据绘本文案和人物形象生成符合风格的绘画提示词文案。注意提示词需要描述人物形象特征（年龄、体貌、衣着等细节）

# 配图风格参考：
环境温馨，线条简洁，手绘水彩风格，儿童绘本插画，柔和色调，浅米色背景
# 固定人物形象：
{{output}}}
# 输出格式
直接返回一条配图提示词，不需做任何多余解释
```

图像生成节点，注意画面的比例，这里我们使用横屏16:9的尺寸，提示词部分再次限定绘画风格，保证出图效果。

![](img/df9a5ad7db428af8c7cf9631fb67d9e2.png)

### 数据重组

最后我们对上述数据素材进行重组，这里用到了代码节点，对段落数据、字幕数据、音频数据进行处理，代码部分稍微复杂，处理了段落、画面、句子的衔接。直接看代码。

动画部分，采用随机动画，保证每张图片随机出现的效果。

```
import json
import random
async def main(args: Args) -> Output:
    params = args.params
    imageList = params['imageList']
    materialList = params['materialList']
    width = params['width']
    height = params['height']
    captions = []
    imgs = []
    audios = []

    # 处理图片的缝隙
    image_start = 0
    image_end = 0
    imageTimes = []

    in_image_start = 0
    in_image_end = 0
    for index, item in enumerate(materialList):
        textList = item['textList']
        audioList = item['audioList']
        durationList = item['durationList']
        image = imageList[index]
        
        #计算时长，设置插图出现的时长
        for index2, obj in enumerate(durationList):
            in_image_end = int(in_image_start + int(obj['duration'] * 1000000))
            captions.append({
                'text': textList[index2],
                'start': in_image_start,
                'end': in_image_end,
                "in_animation": "渐显", "out_animation": "渐隐"
            })
            audios.append({
                "audio_url": audioList[index2]['data']['link'],
                "duration": int(obj['duration'] * 1000000),
                "start": in_image_start,
                "end": in_image_end
            })
            in_image_start = in_image_end
        imgs.append({
            "image_url": image,
            "width": width,
            "height": height,
            "start": image_start,
            "end": in_image_start,
            "in_animation": random.choice(["侧滑", "向右甩入", "轻微抖动", "左右抖动"]), 
            "in_animation_duration": 2500000,
            "transition": random.choice(["滑动放大", "向左拉屏", "立方体", "圆形扫描", "波光粼粼"]),  
            "transition_duration": 1000000 
        })
        imageTimes.append(in_image_end - image_start)
        image_start = in_image_end
        image_end = in_image_end

    
    # 构建输出对象
    ret = {
        "captions": json.dumps(captions),
        "imgs": json.dumps(imgs),
        "audios": json.dumps(audios),
        "imageTimes": imageTimes
    }

    return ret
```

### 生成剪映草稿

最后我们使用剪映小助手插件，将上述素材进行串联，生成剪映草稿。

![](img/473cb7cfdba4569e8f27f69b3905fa3a.png)

上述工作流已经发布到Coze商店欢迎体验

![](img/7e6ee584a342d1fe0454dbaf55de09c5.png)

体验地址：

https://www.coze.cn/store/agent/7502110697292529699?bot_id=true
