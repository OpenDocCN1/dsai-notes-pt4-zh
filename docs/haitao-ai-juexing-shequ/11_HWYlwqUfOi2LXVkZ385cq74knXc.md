
# Coze爆款视频工作流拆解-云端渲染版本

> 来源：[https://zinxkclpk97.feishu.cn/docx/N3MldFBoUo8dSoxsh8VcRItrn1f](https://zinxkclpk97.feishu.cn/docx/N3MldFBoUo8dSoxsh8VcRItrn1f)

> 录制：Coze视频工作流拆解（内部）

> 日期：2025-02-09 20:03:16

> 录制文件：https://meeting.tencent.com/crm/2qWO3G1y9a

> 前两天发布了一篇使用Coze搭建视频工作流的文章，有不少小伙伴前来咨询。

> 趁着还没开工，抽时间写下这一篇更详细的教程，把工作流细节掰开揉碎了讲一下。

> 再浅谈一下对标系列账号，以及运营方法。

## 背景与源起

源起，最近经常刷到一类思维认知提升的视频账号（可能我看鸡汤文比较多😄），实际就是将文章内容以视频的形式进行呈现，相比纯文章模式，视频的传播方式更为广泛。

具体账号分析请看这一篇文章：`https://mp.weixin.qq.com/s/-gbxVceE32MaVe0qTj9nwA?token=1075370899&lang=zh_CN`

下面是相似风格的对标账号，感兴趣的可以学习下。

![](img/883b7834a1d394d8589f0ae96ccb6267.png)

![](img/a2a5373747ea5403d373cb56f266ad7d.png)

![](img/77e6ba9b3f1366f20930648f5ce648eb.png)

## 工作流总体流程

我们搭建这套Coze工作流，来完成此类视频的制作过程。

自己只需要做好选题、内容文案部分工作。

后续视频制作则由工作流来完成，总体耗时3~4分钟。

如果你不会用视频制作软件，那这套工作流将成为你的有力帮手。

### 工作流总体流程如下

![](img/69a6fe2fec59fc3daf37953c83144faa.png)

这里重点介绍流程中涉及到的细节内容

此类视频思路，重点是将文本拆分后处理为图片和音频，再将图片和音频转为一段段视频，最终聚合为整段视频。

输入参数，可根据自己的视频中包含的元素进行灵活调整。

![](img/00b0010c5f2efafc1f8427442eda3f2a.png)

## 节点详解

大模型节点用于对用户输入的文案进一步调整。

![](img/6671aa5ca800278fc585eb6ba704f0ce.png)

代码节点，将文案内容进行拆分为短句，后续会在短句转为音频和图片配文时使用到

![](img/1fa6276c937972f404699f3f2584fc5f.png)

```
import re
async def main(args: Args) -&gt; Output:
    params = args.params
    content = params[&#x27;content&#x27;]
    pattern = r&#x27;[，。！？；]\s*&#x27;

    # 使用re.split()函数根据正则表达式切割文本
    # re.split()会将匹配到的分隔符从结果中移除
    texts = re.split(pattern, content)

    # 移除数组中可能存在的空字符串元素
    texts = [t for t in texts if t]
    # 构建输出对象
    ret: Output = {
        &quot;textList&quot;: texts
    }
    return ret
```

接下来画板节点，用于制作视频的封面图，我们以📕的图片比例制作。

![](img/21b66e5d4e2b3fcddda6b8024625ae22.png)

封面画板具体参数如下

![](img/21ed74fe1c599e1221fa612f30912e37.png)

再继续，对上述拆分后的文本进一步处理，批量转换为图片+文字。

![](img/6395672b76d66eb56b28825cde07cf49.png)

注意批处理参数的设置为4或2，不要太大。

此处画板节点用来制作视频中图文内容，比例使用1920:1080即可

![](img/fdfef08f32b7fa703e65cd3e7cc74664.png)

ok，到了这里我们已经完成一半，图片的准备工作。

下一环节是将分段的短句转换为音频文件mp3格式。

我们借助coze官方的文本转语音的插件，对文本进行批量转换。输入参数为前面的分割后的文本数组`textList`，输出结果为批处理后的音频文件数组。

注意这个节点的批处理并行参数也设置小于4，避免并发高导致报错。

![](img/0b8c17952c40ffb3a8efcdd33b5555b8.png)

将音频聚合，代码节点做一次数据格式转换，然后接插件批量获取音频的时长。

![](img/d76e9032b762f5d51fe45aeda4f493ba.png)

这里的代码节点直接输出数据，对格式进行显式声明。

![](img/1cf644bf7fe374fd6ebcf46c419a67f4.png)

下一环节，是将图片数组和音频文件进行合并，构建云渲染所需的对象结构数据。

![](img/5948c9b2ba61cc5f01f74e8cc9d592c8.png)

重新修改结构的代码，附代码详细，直接复制使用，注意这次使用的是JavaScript语法

![](img/7ace755d16190cc14d4c560ee636de4c.png)

```
async function main({ params }) {

    params.imageList
    params.mp3_infos
    params.outputList

    

    let total_duration = 0
    
    const json_data = get_json_data();

    let start_time = 0

    for(let i = 0; i &lt; params.mp3_infos.length; i++){

        let mp3_tag = &#x27;tag_&#x27; + i

        let mp3_info = params.mp3_infos[i]
        json_data.AudioTracks[0].AudioTrackClips.push(get_audio_data(mp3_info.data.url,mp3_tag,1))
        json_data.VideoTracks[0].VideoTrackClips.push(get_image_data(params.imageList[i],mp3_tag))
        

    }

    
    

    const ret = {
        &quot;json_data&quot;: json_data
    };
  
    return ret;
  }
  
 
  
  function get_image_data(img_url, ClipId){
    
  
    return {
        &quot;Type&quot;: &quot;Image&quot;,
            &quot;MediaURL&quot;: img_url,
            &quot;ReferenceClipId&quot;: ClipId
  }
}
  
  function get_audio_data(audio_url,ClipId,gain=1){
    return {
        &quot;Type&quot;: &quot;Audio&quot;,
        &quot;MediaURL&quot;: audio_url,
        &quot;ClipId&quot;: ClipId,
        &quot;In&quot;: 0,
        &quot;Effects&quot;: [
            {
                &quot;Type&quot;: &quot;Volume&quot;,
                &quot;Gain&quot;: gain
            }
        ]
    }
  }
  
  
  function get_json_data(){
  
    let json_data = {
        &quot;Version&quot;: 1,
        &quot;SdkVersion&quot;: &quot;5.1.0&quot;,
        &quot;VideoTracks&quot;: [
            {
                &quot;VideoTrackClips&quot;: [
                    
                ],
                &quot;Type&quot;: &quot;Video&quot;,
                &quot;Visible&quot;: true,
                &quot;Id&quot;: 0,
                &quot;Count&quot;: 1,
                &quot;Disabled&quot;: false
            }
        ],
        &quot;AudioTracks&quot;: [
            {
                
                &quot;AudioTrackClips&quot;: [
                    
                ],
                &quot;MainTrack&quot;: true,
                &quot;Type&quot;: &quot;Audio&quot;,
                &quot;Visible&quot;: true,
                &quot;Id&quot;: 1,
                &quot;Count&quot;: 1,
                &quot;Disabled&quot;: false
            },
            {
                
                &quot;AudioTrackClips&quot;: [
                    
                ],
                &quot;Type&quot;: &quot;Audio&quot;,
                &quot;Visible&quot;: true,
                &quot;Id&quot;: 2,
                &quot;Count&quot;: 1,
                &quot;Disabled&quot;: false
            }
        ],
        &quot;SubtitleTracks&quot;: [
            {
                &quot;SubtitleTrackClips&quot;: []//歌词滚动
            },
            {
                &quot;SubtitleTrackClips&quot;: []//歌词
            }
        ],
        &quot;AspectRatio&quot;: &quot;16:9&quot;,
        &quot;FeUuid&quot;: &quot;3acb59d8-682a-49b1-9990-92721ba7a894&quot;,
        &quot;OutputMediaConfig&quot;: {
            &quot;Width&quot;: 1024,
            &quot;Height&quot;: 1024,
            &quot;Bitrate&quot;: 400
        }
    }
  
  
    return json_data
  
  }

```

插件为付费视频流插件，可自行注册后获取参数

![](img/81dd102aa151c66dda0e2c3b14fa3833.png)

coze搭建的过程中，视频制作涉及使用到收费的视频处理插件。可自行前往其网站注册获取`api_token`使用。

插件官方注册网址：`https://ts.fyshark.com/#/login?user_id=7358`

![](img/f28054b4b52d118bb3d9c33e28207008.png)

注册后，点右上角个人中心，可以查看`api_token`

![](img/0745dba500cbf10a92c97933c44dae7f.png)

测试下来，采用这种一次性云渲染的方案，单条视频成本约1毛钱。

![](img/dfff90a71fa1c7832578f119df16e263.png)

## 账号运营思路

最后，浅谈一下账号运营思路：

有了这套SOP之后，就是花心思运营和验证了，先发给30篇再说。

### 文案素材

根据我们输出的文案内容，寻找5~10个精品对标账号，重点学习其文案内容，选取高质量的文案作为我们的素材。

比如高质量的👸号、📕、B站账号均可。

AI辅助生成文案素材：当然我们也可以借助强大的AI辅助我们输出内容文案，比如利用现在比较火的`DeepSeek R1`深度思考模型，即可获得高质量的文案素材。

### 发布渠道

视频号>📕>B站，均可作为我们的视频发布渠道。当然也要考虑每个平台的受众，选择适合的话题。

起号之后可以配合挂车、橱窗商品，推销相关的书籍，也可以引流用户至私域深度运营。
