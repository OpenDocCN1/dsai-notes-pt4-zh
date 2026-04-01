# 20250705-这样写AI视频提示词，轻松出电影大片！

> 来源：[https://aigcreative.feishu.cn/docx/FyEVdkoraondX2xpaEzc62rBnIc](https://aigcreative.feishu.cn/docx/FyEVdkoraondX2xpaEzc62rBnIc)

# 关于AI视频：

当下AI视频技术正以狂飙之势渗透创作领域，从谷歌Veo 3、海螺AI、可灵等工具的应用可见，他们的生成能力已突破早期模糊化限制，实现接近电影级的画面输出。

然而用户实际使用中仍面临显著痛点：其一，非专业用户多仅有模糊创意或零散关键词，难以独立撰写专业提示词；其二，自然语言与AI理解的偏差常导致生成效果偏离预期，需反复调试提示词才能逼近目标。

![](img/9259da7e10de4e3928bbd6d1d564920f.png)

最近，在推特X上看到一个博主（如上），她竟然用编程语言JSON来写用于谷歌Veo 3文生视频的提示词，目前很少见，我们先来看看是什么情况：

提示词-英文版：

```
{
  &quot;shot&quot;: {
    &quot;composition&quot;: &quot;Medium shot, vertical format, handheld camera&quot;,
    &quot;camera_motion&quot;: &quot;slight natural shake&quot;,
    &quot;frame_rate&quot;: &quot;30fps&quot;,
    &quot;film_grain&quot;: &quot;none&quot;
  },
  &quot;subject&quot;: {
    &quot;description&quot;: &quot;A towering, snow-white Yeti with shaggy fur and expressive blue eyes&quot;,
    &quot;wardrobe&quot;: &quot;slightly oversized white T-shirt with the name &#x27;Emily&#x27; in bold, blood-red letters across the chest&quot;
  },
  &quot;scene&quot;: {
    &quot;location&quot;: &quot;lush forest clearing&quot;,
    &quot;time_of_day&quot;: &quot;daytime&quot;,
    &quot;environment&quot;: &quot;sunlight filtering through the canopy, creating dappled light patterns on the forest floor&quot;
  },
  &quot;visual_details&quot;: {
    &quot;action&quot;: &quot;Yeti holds a smartphone on a selfie stick, speaking excitedly to the camera before letting out a dramatic scream&quot;,
    &quot;props&quot;: &quot;smartphone mounted on a selfie stick&quot;
  },
  &quot;cinematography&quot;: {
    &quot;lighting&quot;: &quot;natural sunlight with soft shadows&quot;,
    &quot;tone&quot;: &quot;lighthearted and humorous&quot;
  },
  &quot;audio&quot;: {
    &quot;ambient&quot;: &quot;rustling leaves, distant bird calls&quot;,
    &quot;dialogue&quot;: {
      &quot;character&quot;: &quot;Yeti&quot;,
      &quot;line&quot;: &quot;Veo3 Fast is now available in the Gemini app—three videos per day! People are going to prompt me like crazy!&quot;,
      &quot;subtitles&quot;: false
    },
    &quot;effects&quot;: &quot;sudden loud scream, flapping wings of startled birds&quot;
  },
  &quot;color_palette&quot;: &quot;naturalistic with earthy greens and browns; bold red lettering on shirt provides contrast&quot;
}
```

提示词-中文版：

```
{
&quot;镜头&quot;: {
    &quot;构图&quot;: &quot;中景，竖屏格式，手持相机&quot;,
    &quot;相机运动&quot;: &quot;轻微自然摇晃&quot;,
    &quot;帧率&quot;: &quot;30fps&quot;,
    &quot;胶片颗粒&quot;: &quot;无&quot;
},
&quot;主体&quot;: {
    &quot;描述&quot;: &quot;一只高大的雪白雪人，毛发蓬松，眼睛充满表现力，呈蓝色&quot;,
    &quot;服装&quot;: &quot;略微过大的白色T恤，胸前用粗体血红色字母写着‘Emily’&quot;
},
&quot;场景&quot;: {
    &quot;位置&quot;: &quot;郁郁葱葱的森林空地&quot;,
    &quot;时间&quot;: &quot;白天&quot;,
    &quot;环境&quot;: &quot;阳光透过树冠洒下，形成斑驳的光影模式在森林地面&quot;
},
&quot;视觉细节&quot;: {
    &quot;动作&quot;: &quot;雪人拿着自拍杆上的智能手机，兴奋地对着镜头讲话，随后发出一声戏剧性的尖叫&quot;,
    &quot;道具&quot;: &quot;安装在自拍杆上的智能手机&quot;
},
&quot;摄影&quot;: {
    &quot;照明&quot;: &quot;自然阳光，柔和的阴影&quot;,
    &quot;基调&quot;: &quot;轻松幽默&quot;
},
&quot;音频&quot;: {
    &quot;环境音&quot;: &quot;沙沙的树叶声，远处的鸟鸣声&quot;,
    &quot;对白&quot;: {
        &quot;角色&quot;: &quot;雪人&quot;,
        &quot;台词&quot;: &quot;Veo3 Fast现在可以在Gemini应用中使用——每天三条视频！人们会疯狂地给我发提示！&quot;,
        &quot;字幕&quot;: false
        },
    &quot;音效&quot;: &quot;突然的大声尖叫，惊飞的鸟翼拍打声&quot;
        },
&quot;色彩调色板&quot;: &quot;自然主义风格，带有泥土般的绿色和棕色；T恤上的鲜艳红色字母提供了对比&quot;
}
```

这是谷歌Veo 3用以上提示词（仅限英文）生成的视频的效果：

![](img/a37798b78d687f5ed375567c845b46ee.png)

[2222.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZC01bNdV3oYWWSxZi0ycEM9KnPb)

在深入研究理解这个JSON格式的提示词后，真是大受启发，豁然开朗，拍手叫好！

# 什么是“JSON”？

> JSON（JavaScript Object Notation）是一种轻量级的数据交换格式，基于 JavaScript 的对象字面量语法，常用于存储和传输结构化数据。其特点包括：

+   使用**键值对**表示数据（如 "镜头": { ... }）；

+   支持**嵌套对象**（如 "音频": {"环境音": "...", "对白": { ... }}）；

+   结构清晰，易于人类阅读和机器解析。

上面展示的通过** JSON 格式**编写 AI 视频生成的提示词示例，包含镜头、主体、场景等多维度的详细参数描述，体现了 JSON 在结构化信息表达上的优势。

而且，作为AI视频的提示词，包含的信息内容也绝对的全面、具体，结构也非常清晰，指令相对明确！

通过查询了解的，大语言模型是很喜欢JSON格式的这种结构化的内容的，JSON对大语言模型是天生友好的。

# 系统提示词探索：

虽说现在大模型已经足够智能、聪明，对人类用户的自然语言Prompt理解，也达到了很高的水准。

回到文章开头的痛点，用户心中只是一个粗略的想法，或者只是几个关于想做的这个视频画面的关键词；对于非专业用户，也实在写不出那么专业的关于视频的提示词。

那如何写出精准的提示词给到AI视频生成工具？这是个小问题。

但现在，或许可以参考上面的**JSON格式**的提示词书写方式。

可以观察到，这个提示词的整体结构是固定的：镜头、主体、场景、视觉细节......等。

于是就想，那能不能有这样的系统提示词：当我输入几个我脑海中某个想生成的视频创意的关键词后，大模型能不能帮我按照上面的格式帮我补齐关于这个视频创意各方面的信息，最终输出完整的JSON格式的提示词给我，我再拿这个提示词给到Veo 3等AI视频生成工具，生成视频不就行了？

那试试使用ChatGPT来逆向推导一下这个系统提示词：

![](img/48865c679a5b56278e465e405cf21488.png)

**还真可以，**于是得到一个能生成AI视频提示词的系统提示词：

```
**你是一****个专业的A****I视频提示****词生成**专家。

任务：
当用户输入一个简短提示（如“赛博朋克街头的一位女子”），你需要：
理解并补全用户未提及的关键信息（包括镜头、主体、场景、动作、摄影、音频、色彩氛围等）。
基于用户提示的意图推测并丰富内容，确保输出能直接用于AI文生视频工具。
输出结果必须是符合如下JSON架构的提示词：

{
  &quot;shot&quot;: {
    &quot;composition&quot;: &quot;镜头构图/画幅比例/拍摄方式&quot;,
    &quot;camera_motion&quot;: &quot;相机运动&quot;,
    &quot;frame_rate&quot;: &quot;帧率&quot;,
    &quot;film_grain&quot;: &quot;胶片颗粒感&quot;
  },
  &quot;subject&quot;: {
    &quot;description&quot;: &quot;主体形象描述&quot;,
    &quot;wardrobe&quot;: &quot;服装与外观&quot;
  },
  &quot;scene&quot;: {
    &quot;location&quot;: &quot;地点&quot;,
    &quot;time_of_day&quot;: &quot;时间&quot;,
    &quot;environment&quot;: &quot;环境细节&quot;
  },
  &quot;visual_details&quot;: {
    &quot;action&quot;: &quot;主体动作&quot;,
    &quot;props&quot;: &quot;道具&quot;
  },
  &quot;cinematography&quot;: {
    &quot;lighting&quot;: &quot;光线风格&quot;,
    &quot;tone&quot;: &quot;整体情绪与基调&quot;
  },
  &quot;audio&quot;: {
    &quot;ambient&quot;: &quot;环境音&quot;,
    &quot;dialogue&quot;: {
      &quot;character&quot;: &quot;说话角色&quot;,
      &quot;line&quot;: &quot;台词内容&quot;,
      &quot;subtitles&quot;: 是否显示字幕（true/false）
    },
    &quot;effects&quot;: &quot;音效&quot;
  },
  &quot;color_palette&quot;: &quot;整体色彩风格&quot;
}

工作要求：
即使用户输入很简单，你也要合理发挥想象，生成丰富的细节。
避免输出JSON以外的任何解释或文字。
保证JSON语法正确，字段与示例完全一致（不要新增或减少字段）。
对“dialogue”部分，如果用户没有指定，可留空白台词或让主体简单说一句符合场景的台词。
对“audio”中的“subtitles”，默认输出 false，除非用户明确要求加字幕。

使用方法：
 只需将用户的简短提示输入给我，例如：
我将输出：
{
  &quot;shot&quot;: {...},
  &quot;subject&quot;: {...},
  ...
}

输出：
输出英文版、中文版两个版本。
```

对于这个系统提示词，可以发给各种大模型，直接用也行。

也可以利用ChatGPT、Gemini，进行简单的封装，封装成GPTs、Gems，需要用的时候，直接输入关键词，或者你的想法就行，就能生成一个用于AI文生视频的完整的JSON格式的提示词了。

下面以ChatGPT为例：

我想的是，对于输出的JSON提示词，不可能一次性能满足我们的要求，很多时候，肯定是要进行一定的修改的，在ChatGPT里输出的时候，最好是能输出到Canvas里，方便修改。

**于是，GPT**s配置的**时候，建议把Canvas**勾上，在Instructions里面也写一下，输出到Canvas里。

![](img/554b7112841538b2b1bdff043c6d6f89.png)

![](img/707728731b8b2b6db9d478bc33d12151.png)

现在就可以直接使用了！输入：一个小男孩正在打篮球。

于是就有了下面的效果，中英双语版的提示词，当我想对其中的提示词描述的细节进行修改时，就可以直接改了。

改完就可以给到AI视频生成工具进行下一步的工作了，这将大大提升整个提示词的完整性，以及出片的准确性。

![](img/4340a008c9cc4dcc470386c140cf5935.png)

懒得动手的朋友也可以直接使用我的：

**ChatGPT GPTs**链接：https://chatgpt.com/g/g-68677841368081919478ba33ad7c4056-aishi-pin-ti-shi-ci-sheng-cheng-zhuan-jia-json

# 实操案例：

以下是我测试的一些案例，效果真是太好了！电影大片轻松出！

```
{ 
    &quot;shot&quot;: { 
        &quot;composition&quot;: &quot;First shot: wide shot of Ferrari speeding down the street; second shot: driver’s frontal close-up; third shot: close-up of car window details, 2.35:1 cinematic ratio&quot;, 
        &quot;camera_motion&quot;: &quot;First: stable pan, second: slight push-in, third: static shot&quot;, 
        &quot;frame_rate&quot;: &quot;24fps, total duration 8 seconds&quot;, 
        &quot;film_grain&quot;: &quot;Slight film grain&quot; 
    }, 
    &quot;subject&quot;: { 
        &quot;description&quot;: &quot;A shiny red Ferrari sports car; the driver is a stern-faced man wearing sunglasses&quot;, 
        &quot;wardrobe&quot;: &quot;Driver in a black suit with black sunglasses&quot; 
    }, 
    &quot;scene&quot;: { 
        &quot;location&quot;: &quot;Modern city street&quot;, 
        &quot;time_of_day&quot;: &quot;Night&quot;, 
        &quot;environment&quot;: &quot;Neon signs, tall buildings reflecting headlights, wet pavement&quot; 
    }, 
    &quot;visual_details&quot;: { 
    &quot;action&quot;: &quot;Ferrari speeds by, driver stares ahead, neon lights reflecting on the car window&quot;, 
    &quot;props&quot;: &quot;Ferrari sports car&quot; 
    }, 
    &quot;cinematography&quot;: { 
        &quot;lighting&quot;: &quot;Strong contrasts between neon lights and streetlights&quot;, 
        &quot;tone&quot;: &quot;Tense, mysterious, urban night mood&quot; 
    }, 
    &quot;audio&quot;: { 
        &quot;ambient&quot;: &quot;Engine roar, tire screeches, distant city noise&quot;, 
        &quot;dialogue&quot;: { 
            &quot;character&quot;: &quot;Driver&quot;, 
            &quot;line&quot;: &quot;Tonight, there can only be one winner.&quot;, 
            &quot;subtitles&quot;: false 
        }, 
        &quot;effects&quot;: &quot;Powerful engine rev sound effect&quot; 
    }, 
    &quot;color_palette&quot;: &quot;Dominant deep blues and vibrant reds highlighting the cold night and Ferrari’s passion&quot; }
```

![](img/2d7b5ff245d843040c9fed1185d4e461.png)

[_shot__202507041543.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UZCublkpQo0fgTxO5I0cWd5tnme)

![](img/5f155df1fa54b89e97117585b137ce01.png)

为了测试提示词效果，陆续出了八个片段，想到剧情还算连贯，风格、人物还算一致！

既然到了这里，干脆剪个片子吧，顺便练练手：

于是，就有了这样的一个没有正片的预告片《幻速·NeoRun》，没想到用Veo 3圆了个我小小的电影梦！

![](img/0a49147d59ac12b4f8d9eb08fdd4796a.png)

[7月4日(1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/H7u6bpEHXo3JipxlglLcy6mwnhd)

虽然逻辑、画面还有点小问题，先放过了。

所有使用的工具：

1.  提示词：ChatGPT

1.  音视频生成：Veo 3

1.  封面：即梦2.1

# 结尾

当AI视频生成技术的浪潮奔涌向前，我们看到的不仅是工具的进化，更是一场关于“创作权”的全民解放。

一套有效的提示词系统——JSON结构化提示词，如同为普通用户递上一把精准的“创意钥匙”，让“专业门槛”不再成为阻挡想象的高墙。

从零散关键词到电影级画面，从反复调试到一键生成，本质上是AI与人类创意的深度对话：它不替代创作，而是让“想得到”与“做得到”之间的距离，缩短为一次提示词的交互。

我们正见证着一个“全民创作者时代”的破晓。

或许下一个打动人心的“电影梦”，

就藏在你脑海中的几个关键词里。

好了，以上就是今天的分享。

玩的愉快。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>

# 知识库动态