
# Coze 儿童古诗词视频动画制作教程

> 来源：[https://zinxkclpk97.feishu.cn/docx/RNardFLXfoW2OgxFqxJcFmginkh](https://zinxkclpk97.feishu.cn/docx/RNardFLXfoW2OgxFqxJcFmginkh)

## 详细内容

如下示例，是基于图片使用海螺AI直转的视频素材。

![](img/050648ab8305b238defe41f1d965c56c.png)

[365340941042733058.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ToCtbjgknoWROOxToBdca0lEnkd)

![](img/44268b9aadb52e2603f5d90c02016754.png)

[1744131749926626729-video_watermark_4bfc634ecb847c22e844de4ae3784cd8_366059747524521986.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZoB4bk5LZoilaOx53gXcyaqonGc)

![](img/d0859ae2aac3b4631f437c89a820a8ce.png)

[1744132322550467766-video_watermark_7bfbf2b85d00bef3dd0d1d364305afbf_366062247413579778.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RYWTbzv9coLOeBxgEyHcg0FlnIf)

![](img/9e26f642dbd497cd416fcbddc07a7acd.png)

[364207027070554112.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/R8X0bGt48on2pIxoLJOc7o8znTc)

图片转视频的软件有很多，比如即梦、海螺、Vidu等，其使用方法大家自行学习。

我们这里重点介绍如何使用Coze+AI视频插件，实现视频制作的工作流。

在此之前，需要理解文生图、图生视频，都存在一定的不可控因素，生成的效果无法达到100%完美，所以抽卡是不可避免的。一个好的AI视频创作必须经过反复调整提示词抽卡、剪辑打磨才可以。

回到正题，我们以儿童古诗词启蒙视频为例。

介绍顺序：先搭建制作单图片转视频的工作流，再与儿童古诗词主工作流进行结合使用。

这里简单画一张流程图介绍图片转视频的原理

核心知识点

+ 控制大模型生文效果，分镜画面人物一致性
+ 图像生成节点，选择合适的模型，保证生成画面效果
+ 图转视频模块，需要单独搭建图转视频工作流，异步执行生图任务
+ 重组素材输出剪映草稿，其中包括文字、音频、视频、特效等时间线设置

### 工作流搭建

我们这里使用海螺的插件，实现图转视频工作流如下，先搭建好如下工作流起个名字`image2video`，后续主工作流中会使用到。

![](img/a66d69d46bf944ff60584d654fb933e4.png)

开始节点参数为图片和提示词

![](img/00118ff74d5cbcb338324cb687143ce7.png)

下一步使用视频生成插件，输入图片和视频提示词，模型我们选择`I2V-01-live`，2D卡通表现效果还不错。`token`参数则需要到相应平台完成注册后获取。

考虑海螺插件配置复杂和稳定性问题，也可以使用阿里通义万像插件平替

![](img/0a8f210fe6771b6b4c2daceda9ab9623.png)

![](img/9794474f22699d7c9efb4a71c5315d4c.png)

因为视频制作耗时比较长，这里采用异步的方式，去查询任务状态，因此我们需要使用一个循环插件，来定时轮询获取任务状态拿到视频地址。

![](img/2e3f98ff34c76126ad02e1c7ccbfda73.png)

细节部分，循环次数选择无限循环，设置中间变量`video_url`，用来接受生成好的视频地址

![](img/feb3a15c61f5d2749df83865344deea5.png)

循环体内部，使用任务查询插件，获取当前视频任务状态及视频链接

![](img/5b42f32e7185835cdaf5b205dfe88658.png)

添加参数判断逻辑，如果任务未完成，则等待30s钟后再继续循环，如果任务成功则设置`video_url`参数并终止循环。流程比较简单就不展开细节了。

![](img/5c9f8b28ff2e264180507a42ac0e2570.png)

以上是图生视频的工作流，使用这个工作流我们可以实现单张图片转视频。

接下来我们继续看，如何将其与儿童古诗词工作流进行结合。

儿童古诗词视频工作流

回顾我之前分享的视频工作流案例，国学文化、成语故事等，可以作为参考进行复用。

看下对标案例

![](img/78631c3d6c33c1bebc834f32f8598c44.png)

首先还是老生常谈的方案，使用大模型生成古诗词文字、分镜画面。

通过这个提示词，我们可以一步完成诗词文案、分镜提示词、人物一致性控制。也相对减少了大模型的`token`资源消耗。

![](img/1c0ab82d94fc702d1d9563705df098de.png)

```
# 系统提示词
# 角色 儿童古诗词绘本设计师
擅长卡通绘画，擅长中国古诗词

# 任务
根据用户要求，生成诗句对应分镜画面提示词`prompt`、诗词解释`desc`、诗词句子`sentence`

# 输出格式
以数组对象的结构输出。
示例：
[{
"sentence": "床前明月光",
"desc": "窗前有一片明月洒下的光辉",
"prompt": "Q版可爱漂亮小女孩6岁，头部占身体1/3，大眼睛占头部1/3，梳着黑发双丫髻，穿着粉色齐胸襦裙，动作描述(手指摩挲怀中玉佩)，回忆光效(地面浮现故乡稻田、纸鸢飞舞)，动态模糊，暖黄回忆色调，光柱效果，渐隐转场，低角度仰拍 "
}]

===========================
# 用户提示词
我想做一个关于古诗《{{`title`}}》的儿童古诗动画视频，帮我设计每句古诗的分镜头画面提示词，包括古诗文案，运镜，画面提示词。
画面提示词要求符合Midjourney提示词结构，包含主体人物、动作描述、场景画面、画面质量、灯光氛围、风格参数与镜头语言，每个提示词都加上"卡通风格"，每个画面都要搭配如下人物形象提示“卡通风格Q版可爱漂亮小女孩6岁，头部占身体1/3，大眼睛占头部1/3，梳着黑发双丫髻，穿着粉色齐胸襦裙”，并且保持人物一致，风格一致，着装一致。
```

下一步生成封面配图，这一步可以按需选择。

![](img/6501dee5409c5209a4a3cfaec9647152.png)

继续，使用批处理节点或循环节点，对大模型输出的分镜数据进行处理，生成配图画面，并将图片转为视频片段。

这里就用到了我们前面已经搭建好的图像转视频的工作流`image2video`。

![](img/5c66693b288b77cf7fb84f0491a2cc4b.png)

批处理完成后，我们会获得一组图片和一组视频链接。

下一步，使用到语音合成插件，把诗词文本批量转成朗读语音。

老粉应该很熟悉了，就不过多介绍。

![](img/198460e49907cf604eeffd17bc0f76a2.png)

![](img/62144e8d53aacdbb12b21ed6817ae685.png)

获取音频时长，用于后续剪映草稿时间线计算

![](img/0c1d8c6214084056673c71fdb828883c.png)

然后将已经生成好的素材进一步组装成剪映小助手所需要的数据结构。

代码节点实现数据重组聚合。

![](img/8a5feaa353efdd23eb2743fd7fdcfcea.png)

详细参数

![](img/ba92536a01efd304db03c2f1a9c29b97.png)

数据聚合代码节点源码

```python
import json

async def main(args) -> dict:
    params = args.params
    
    # 参数解析（处理类型转换和嵌套结构）
    times = [t["duration"] for t in params['times']]  # 提取duration数值
    video_list = params['video_list']                # 视频数组
    texts = params['texts']                           # 字幕数组
    audio_urls = params['audio_urls']                 # 含嵌套结构的音频数据
    width = int(params['width'])                      # 转int类型
    height = int(params['height'])                    # 转int类型

    captions = []
    videos = []
    audios = []

    current_time = 0  # 时间轴指针

    # 遍历所有元素（假设四个数组长度一致）
    for idx in range(len(texts)):
        # 计算时间区间
        duration = times[idx] * 1000000  # 转为微秒
        end_time = current_time + int(duration)
        
        # 构建音频轨道（从嵌套结构提取真实URL）
        audios.append({
            "audio_url": audio_urls[idx]["data"]["link"],
            "duration": duration,
            "start": current_time,
            "end": end_time
        })
        
        # 构建字幕轨道
        captions.append({
            "text": texts[idx]['sentence'],
            "start": current_time,
            "end": end_time,
            "in_animation": "激光雕刻",
            "out_animation": "渐隐"
        })
        
        # 构建图片轨道（每个时间段对应一张图）
        videos.append({
            "video_url": video_list[idx],
            "width": width,
            "height": height,
            "start": current_time,
            "end": end_time,
            "transition": "叠化",
            "transition_duration": 1000000
        })
        
        # 移动时间轴指针
        current_time = end_time

    # 构建输出（保持JSON序列化）
    return {
        "captions": json.dumps(captions),
        "videos": json.dumps(videos),
        "audios": json.dumps(audios)
    }
```

后面按照图示结构，将剪映小助手插件，依次串联即可。

![](img/012a811d78e5313ba7a28580d8931ea0.png)

以上就是整个儿童古诗词启蒙视频工作流内容。

需要注意，一句故事对应一个视频片段，每条视频制作时间不定（根据服务排队情况来定）预计5-8分钟。

我们以古诗《咏鹅》为例，看下制作效果。

![](img/1f2d7504a0e64de28e19f11b2912859a.png)

[08d839a0-7bb9-4a63-8a02-7c116c193ca0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ITvpbbOwLoziLHxBbLfcpUJxnmg)

没错，这就是Coze工作流直出的效果。

还有优化空间，比如添加开头标题画面、转场特效、循环播放等。

![](img/3b49229722fb43b39bf80baa1c529706.png)

[b357bd12-25ea-4da2-ac5d-bb5d1b2ec29a.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RHVLbl6DvoSk4txFjuLcpFifn5f)

+ 相关插件

+ 定时器插件 https://www.coze.cn/store/plugin/7479765683052494882?from=plugin_card

![](img/7cece5f2d526195725fa9058196753eb.png)

+ [Coze视频生成插件使用说明](https://zinxkclpk97.feishu.cn/wiki/VZGzwiicKiXsTlkjawnc2ZAfn2d?from=from_copylink)

![](img/49cefbe025574588a74c47ddcffc4120.png)

[c3_gushi_video - 工作流 -智能体平台.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CU7dbBPdZoQuMdx4NwncDY9ensd)

### 已上架应用商店欢迎体验

https://www.coze.cn/s/2Ruo5A6Kemo/
