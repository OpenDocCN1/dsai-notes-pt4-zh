
# AI做短片工作流拆解

## AI做音乐MV工作流拆解

> 来源：[https://aigclena.feishu.cn/docx/MGTtdjlokoseyMxJ6d4cLyK7ndb](https://aigclena.feishu.cn/docx/MGTtdjlokoseyMxJ6d4cLyK7ndb)

> 作品地址链接 5.12 复制打开抖音，看看【Ai设计师lena的作品】我用AI做了一支国风音乐MV # suno # 文... `https://v.douyin.com/ihnpF7SL/` lcA:/ y@t.re 03/02

这支MV的工作流分4个部分，1、Suno里生成音乐，这个歌词是伟人毛主席的诗句，所以就省去了在GPT里面写歌词这一步。2、在Midjourney里生成对应的图片，3、图转视频。4、剪辑合成（大部分需要🪜访问，排名分先后）

+ 生成音乐工具：Suno、udio、天工

+ 写歌词工具：Gemeni、Copilot、chatGPT、Kimi、通义千问、文心一言等

+ 图转视频：Runway、Dreamina、Pixverse、Haiper、Morph Studio、Pika

### 一、suno生成音乐

地址：`https://suno.com/`

suno3月底才出来第一天的时候，各大AI群里全是ai音乐的刷屏。那阵儿正好有个契机，让我想起10多年前击中我的某片片尾曲 The Enemy，于是尝试使用suno生成相似的曲子，第一次使用我没有填词，描述词是整首歌的故事梗概/意境/情绪/传递的情感，出来的曲子一下惊艳到我了。后来认真的填词抄别人的音乐风格稍微修改，玩儿到停不下来，不过跟第一次让ai随机生成的曲子对比，我更喜欢第一次ai随意发挥出来的歌曲。

说回正题，《沁园春·雪》，歌词不用说，有现成的内容。歌曲曲风关键字：Heavy metal, hard rock, guzheng, Chinese rhythm, Cantonese, heroic, romantic

![](img/d829917c1cd6164ed335283026388b45.png)

![](img/e39a2c98eab0ca634553ba98c353a23e.png)

点击创建后，一次生成2首，我试了2段关键字就有了喜欢的歌曲，第一次使用的关键词是：Chinese folk songs, heavy metal, hard rock, guzheng, Chinese rhythm, boldness, romance，第二次歌词做了删减，加了Cantonese（粤语），歌词也加了`[Intro]`、`[Verse 1]`这些，个人感觉加不加区别不是太大。3是封面系统会自动生成。

![](img/b4b7299429d22569165939b460d52aa4.png)

过程中发音多少有些问题，可以用相同的发音的词代替，比如：北国风光中的“北”字替换为“被”字后发音就准确一点，其他词语发音不准时都做了替代，山舞“阴色”、“月”输文采、“环”看今朝。

修正之后得到这首曲子👇👇

`https://suno.com/song/a9ce2c02-8429-4642-8722-a7d05b7bff5a`

把音频下载到本地

![](img/fa810ad785224ba0fc867ce682e3d6a4.png)

点击下载`video`，是这个样式：`http://xhslink.com/0ZVTHH`

更详细suno音乐教程：`http://xhslink.com/tU7SHH`

### 二、MJ生成图片

dis地址：`https://discord.com/channels/@me`

mj地址：`https://www.midjourney.com/archive`

图片生成上，用的是Midjourney,图片比例3:4。这2个地址都是Midjourney生图的地址，只是使用方法上略有区别。mj里更方便，看到好看的图，也想使用相同关键词，只需要点击一下就会自动填入，垫图方式切换也更快捷，图片/`sref`/`cref`只需要点击就可以切换。dis里生图有一个好处，在没有fast情况下可以大量刷图。工具不熟悉的小伙伴儿，可以看教程。

![](img/dd53a518973264c69543662c3103c244.png)

根据歌词匹配画面，在工作之余陆陆续续花了2天完成上千张。大都是风景，这种类型的图片生成上没有难度。根据词的意境，选择了国风画面，主要关键词“Chinese landscape painting”，这其中会应用到垫图/重绘等方式对画面稍作调整。

以大雁塔为例：直接写关键字，写出来不太像，于是采用垫图生成图片，挑选比较满意的图再做优化，生成出来的图有2个问题：有水印、大雁太多，于是使用了重绘功能，轻松把水印去掉了，重绘多次大雁数量减少不了，就用`ps`简单处理了下1分钟解决。mj可控性不强，单纯靠抽卡比较耗时，实在刷不出来的画面可以想别的办法解决。

![](img/d30b5242a83529a27dd7f18fa6847f14.png)

垫图

![](img/b5ae437159d8a69a52f0aedbf1c245ea.png)

mj生图

![](img/ae3876cf8db42f2d9f62188d054de4c5.png)

重绘去水印

![](img/7f13f0422cd4b65a8bf406d1b8cc0fc0.png)

`PS`稍作处理

有时候想不出用什么画面，可以直接问语言模型，问语言模型的时候要有方法，除此之外最好客气友好一点比较好。有了灵感后在midjourney里输入关键词，画面就出来了，有了AI做什么事都比较轻松，让他按你的要求做就可以。

![](img/5c9b9265c1504730d9f6ce5932c2e590.png)

![](img/13b75e74af9e8dc4e8126502d75ced6f.png)

### 三、图片转视频

runway：`https://app.runwayml.com/`

Pixverse: `https://app.pixverse.ai/`

字节dreamina：`https://dreamina.jianying.com/ai-tool/video/generate`

我用到的图转视频工具是runway、pixverse。字节的dreamina也很好用，那几天刚开始内测，还没有拿到资格。

pixverse也有效果好的，不过大部分效果就是图片放大/移动，效果比较一般。👇👇

![](img/a5bd5f4aa03c01b4c6bb6823106ef1e5.png)

[`pixverse%2Fmp4%2Fmedia%2F8435c983-25b9-43c2-8a84-6fdd15a981c7_seed1025460837 (1).mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XS82bp3R1oHZulxkNXicbatlnY9)

![](img/9ca6cd72258bbba1bb5a2a3c2c76682d.png)

[`pixverse%2Fmp4%2Fmedia%2F7e0cb1af-8724-4b4c-b4d3-bbd8097fad85_seed15445830.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CcTNblynHo7vpgxeLHlct0F4nQd)

Runway在可控性上稍微强一点，效果也很好，直接用运动笔刷很容易就可以刷出来。所以90%以上的视频还是用的runway。👇👇

![](img/42d4e0d29b7ae300419828d05056139f.png)

[`Gen-2 2583445567, China The rising sun, image 861jpg, brush_H -02.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UjgOblIOtoeftfxNf1PcFtYVnGd)

![](img/09c60bad6666c9ffe1d6fcc489c0a9b1.png)

[`Gen-2 660119566, Famous scenic spots , syna4824_Famous_scen, brush_H -05, brush_V 07.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Yz84bhGS1oGocfx9Wc4caf1GnXf)

效果对比

![](img/571e2ff2880aefd911568a24dafcc39a.png)

[`imageinput_数字51在海底世界飘动 冒泡泡_2024-04-29 09_22_59.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LEO1bucTSo5qJdxnmikcgzMEnDc)

dreamina

![](img/3e7c047c3c3a15fe951b9595bb23f588.png)

[`pixverse%2Fmp4%2Fmedia%2Fcc97d7e2-bd44-4635-adb7-7b3c0115623a_seed67215536.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XZDvbbcp6oTah1xSe0IcjkOtnOd)

Pixverse

### 四、剪辑合成

因为效果比较简单，是用剪映合成的。剪映里只需要做3件事：①拖入音频&视频 ②根据歌词调整视频画面的顺序 ③识别歌词，适当修正下未能识别的文字

![](img/d49fa04094143916d7e46b0b2f2b3264.png)

tips：歌词和曲风的生成可以采用以下的方式

①可以告诉gemini/其他语言模型参考某知名歌曲和营造的氛围重新填词，或者加入更多你想表达的关键词

②询问曲风/特点/使用乐器，以便在填入suno的音乐风格关键词时增加/删除/修改

③不满意的歌词，让gemini重新写，也可以给他一个方向引导他，总之调整到你满意为止。

+ 谷歌gemini生成歌词👇👇

![](img/8d22990413abc1fab110d24c779a76a0.png)

![](img/8fd6c1abbcc75a3e6962dc38c28266c1.png)

![](img/c24a6871e357b7378af1ec38e7b1a644.png)

![](img/cc223be9fe5d6b1b2c895ca745195806.png)

+ 文心一言生成歌词👇👇  有一丢丢的伤痛文学气息，歌词修改一遍后的区别不是太大

![](img/ecb7ed53ac44d6c8ed124168c0651067.png)

![](img/9571b7ff7c05b500e627eecd0e2e7329.png)

+ 秘塔生成歌词👇👇

写一首歌词，内容跟泰勒斯威夫特的midnights里的 Anti-Hero比较相似，传递的情绪和感情是一样的，营造悲伤、沉郁的氛围，展示自我怀疑、焦虑和脆弱的情绪，请重新填词，翻译为中英文

中文歌词：

我在这午夜的街头徘徊，
寻找着那失去的光芒。
每一次回头，都是无尽的黑暗，
我的心，被恐惧和不安所包围。
我曾以为自己是特别的存在，
但现在看来，我只是个过客。
在这漫长的人生旅途中，
我不断地迷失，不断地寻找。
我试图用微笑来掩饰我的脆弱，
但内心的痛苦却无法隐藏。
每一次的尝试，都像是在对抗命运，
而我，似乎永远也赢不了这场战斗。

英文歌词：

I walk these midnight streets,
Looking for the lost light.
Every time I turn around,
It's just endless darkness tonight.
I thought I was something special,
But now it seems, I'm just a passing by.
In this long journey of life,
I keep losing myself, and trying to find.
I try to cover up my fragility with a smile,
But the pain inside can't be concealed.
Every attempt feels like fighting against fate,
And I, seem to never win this battle.

通过描绘一个人在午夜时分的孤独和内心的挣扎，歌曲传达了一种悲伤和沉郁的氛围。歌词中的“我”经历了从希望到绝望的过程，反映了对自我价值的质疑和对未来的不确定感。

## 音乐MV欣赏

2024.7

![](img/4b497e7588c34431ba6a1b32cadbc569.png)

[`X2Twitter.com_lN70-Q_p3q0shXll_720p.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CrNtbB9R9oRzqexgqzqcJe17nIc)

## 联系我们

> [互联网设计](https://aigclena.feishu.cn/wiki/PD10wBb5yiRVE7kTFZdcuzUBnth) [国际站/独立站](https://aigclena.feishu.cn/wiki/TmfowYzfMisIShkNU7ncEBMingh) AI课程学习

![](img/88009097d40888d29a7847ebf4184e50.png)
