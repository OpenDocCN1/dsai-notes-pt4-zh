
# 教程：AI MV制作

> 来源：[https://datawhaler.feishu.cn/docx/ZlDJdaoudoGskExLnGVcRp14noQ](https://datawhaler.feishu.cn/docx/ZlDJdaoudoGskExLnGVcRp14noQ)

> ✍️ 作者：[Halsey的个人说明书](https://datawhaler.feishu.cn/wiki/WAxEwYPCNi85tjkv0L0cwGmAnbg?from=from_copylink)

案例作品：

http://xhslink.com/FFF8HK

## 一、主题确定

首先需要确定创作的主题，教程以“恋-西湖十景”为主题进行说明。

## 二、歌曲生成

suno歌曲创作的基础教程参考，地址

[Suno V3 零基础教程](https://xqokx732qmf.feishu.cn/wiki/HVKvwDo24iuonpkEhEXcI4oEnPX?from=from_copylink)

更多AI音乐教程 [AI + 音乐](https://datawhaler.feishu.cn/wiki/TTPawK8gainZzRkTomkc3qw8n0g?from=from_copylink)

### 2.1 歌词创作

根据主题进行歌词创作，由于本次以“恋-西湖十景”为主题，主要想表现杭州的景点以及在这些美丽风景下发生的爱情故事，根据这个主题，让大模型来生成歌词：最简单的方法就是使用一个suno生成歌词的prompt，然后直接给大模型发送指令：

`/theme 西湖爱情故事 杭州十景 /ele 苏堤春晓、曲院风荷、平湖秋月、断桥残雪、花港观鱼、柳浪闻莺、三潭印月、双峰插云、雷峰夕照和南屏晚钟 /style 流行，舞曲，情歌，歌颂爱情 /singer 林俊杰 /writer 林秋离 方文山`

大语言模型返回的歌词：

[Verse]

在那苏堤春晓里，你我相遇笑靥如花

曲院风荷轻摆，述说着初见的喜欢

当平湖秋月映两人，诉说不尽的柔情万种

而断桥残雪下，是我们坚定的誓言

[Verse 2]

走过花港观鱼，你指间跃动的光影

柳浪闻莺中，耳边细语似乐章响起

三潭印月下，倒映着我们的笑容

双峰插云上，承载着无数梦想与希望

[Chorus]

雷峰夕照旁，紫金色的余晖洒满肩头

南屏晚钟响，时光仿佛为此刻停留

每一个场景，都是我们爱情的证明

就让这些美好，成为永恒的记念

### 2.2 suno歌曲生成

利用生成的歌词，在suno中进行歌曲生成，suno歌曲生成参考

[Suno V3 零基础教程](https://xqokx732qmf.feishu.cn/wiki/HVKvwDo24iuonpkEhEXcI4oEnPX?from=from_copylink)

生成的歌曲如下：

![](img/17aa46fcff0ac0d8f2302f92d6e06374.png)

[`恋-西湖十景.mp3`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QpJXbzmdsoo2dqxcl1TcmFconVc)

## 三、分镜脚本生成

给大模型发送指令：

帮我把上面的歌曲转化为具体的动画场景描述，把每一个需要展示的画面都描绘出来，包括转场，每个场景只有3秒左右，就需要切换到下一个画面，所以需要尽可能的将画面拆的非常细。

大模型返回：

分镜脚本：

场景 1：苏堤春晓

内景/外景： 外景

时间： 清晨

描述： 镜头缓缓推进，展现苏堤上晨雾缭绕，柳条轻摆，两个主角在此相遇，彼此眼中都是对方的倒影。

场景 2：曲院风荷

内景/外景： 外景

时间： 上午

描述： 风轻轻吹过荷塘，荷叶轻摆，两人坐在湖边的长椅上，交谈着初见时的喜欢。

场景 3：平湖秋月

内景/外景： 外景

时间： 傍晚

描述： 月亮从湖面升起，两人的身影在湖面上拉长，他们在月光下诉说着彼此的爱意。

场景 4：断桥残雪

内景/外景： 外景

时间： 冬日午后

描述： 雪后的断桥，两人相拥在桥上，背景是飘落的雪花，他们在此许下永恒的誓言。

场景 5：花港观鱼

内景/外景： 外景

时间： 中午

描述： 两人在花港观鱼，鱼儿在水中游动，他们的手指轻触水面，水波荡漾。

场景 6：柳浪闻莺

内景/外景： 外景

时间： 早晨

描述： 鸟儿在柳树间欢唱，两人漫步在柳荫下，耳边是彼此的细语和鸟鸣。

场景 7：三潭印月

内景/外景： 外景

时间： 夜晚

描述： 三潭印月的夜景，湖面平静，月亮的倒影在水中清晰可见，两人的笑容也随之倒映在湖中。

场景 8：双峰插云

内景/外景： 外景

时间： 日落

描述： 镜头拉远，展现双峰耸立，云雾缭绕，两人站在山顶，远眺着未来。

场景 9：雷峰夕照

内景/外景： 外景

时间： 傍晚

描述： 雷峰塔在夕阳的照耀下显得格外神秘，两人（依偎）在塔旁相拥，紫金色的余晖洒在他们的肩头。

场景 10：南屏晚钟

内景/外景： 外景

时间： 晚上

描述： 钟声在南屏山上回荡，两人坐在山脚下，仿佛时光为他们停留。

针对上述场景分镜描述，让大模型翻译成英文prompt。此处略，因为这里生成的时候可能达不到预期（能符合预期就更好了），所有后面在生图的时候还需要去调对应的promopt。

## 四、图片生成

### 4.1 Midjourney生成图片

目前动画的工作流基本都是先文生图，然后再图生动画。

目前有非常多的文生图服务软件，包括Midjourney，Stable Diffusion，无界AI等，这里以Midjourney为例进行说明。

Midjourney官网 https://www.midjourney.com/home

[Midjourney零基础教程](https://docs.midjourney.com/docs/quick-start)

[Midjourney控图必备技巧](https://www.zcool.com.cn/article/ZMTU3ODk3Ng==.html?)

根据生成的场景prompt，给到Midjourney进行图片生成，注意这里需要根据Midjourney返图情况判断是否符合要求，如果不符合要求需要修改prompt，然后重新生成图片，直到得到满意的图片为止。下面是场景一的一个示例：

生图prompt：

```
The West Lake in Hangzhou, China is located on the lake surface near the city center and surrounded by lush green trees. There is an ancient stone arch bridge leading to the shore, with clear water that reflects its surroundings. The drone captured aerial footage from above, showcasing both picturesque scenery and tranquil waters. In autumn, there were willow trees along the riverbank. --ar 16:9
```

Midjourney返图：

![](img/96239ab9beeb8651e2f182aa51e69867.png)

按照上述方法，依次对所有场景进行图片生成。

## 五、动画生成

### 5.1 利用Gen-2生成视频

Gne-2官网：https://runwayml.com/ai-tools/gen-2/

使用教程：https://www.youtube.com/@RunwayML 系列视频

利用每个场景的图片，将图片导入到`gen-2`中，利用图生视频并配合一定的prompt进行视频生成，对应的场景一的一个视频案例如下：

![](img/fbf474eb06ce4d78da799767ecca8dd6.png)

![](img/9d96b5c1745672f3d59250daa144fe99.png)

生成的对应动画如下：

![](img/ac5b8266623de46dce0887ae6ea64726.png)

## 六、后期剪辑

AI剪辑工具-剪映： https://www.capcut.cn/

### 6.1 “开始创作”

点击`开始创作`进行视频剪辑

![](img/0d99f8b9d917fbeabec7b30adffca2d1.png)

### 6.2 导入素材

将素材直接从文件夹拖入左边的`导入`区域，点击视频进行预览筛选，将需要放入剪辑的视频拖入下方的`时间线`进行处理即可。

![](img/4343e6cb70a1b45d7d3456349089afe5.png)

### 6.3 识别字幕

将音频拖入`时间线`后，右击`识别字幕`生成字幕，可以在右边的`工具栏`对字幕进行样式修改等。

![](img/d4ce2d7ef21ee79f43251d6b39b84dbb.png)

### 6.4 导出视频

在右上角处导出视频，可以按照需求设置导出参数。

![](img/aa95fb9d5547722403a4422177c9834c.png)

## 七、最终成品

http://xhslink.com/FFF8HK

## 八、加餐-个人音乐MV制作

敬请期待

## 附录 工具列表

### AI绘图工具

- Midjourney: https://discord.com/invite/midjourney
- 悠船: https://desktop.youchuanai.com
- Stable Diffusion: https://github.com/Stability-AI/stablediffusion
- 哩布哩布AI: https://www.liblib.art/
- 无界AI: https://www.wujieai.com/ai
- 即梦: https://jimeng.jianying.com
- Leonardo.ai: https://app.leonardo.ai/

### AI动效工具

#### 送积分:

- [Runway Gne-2](https://runwayml.com/ai-tools/gen-2/)
- Pika: https://pika.art/
- PixVerse: https://app.pixverse.ai/
- 即梦: https://jimeng.jianying.com

#### 免费：

- Vivago: https://vivago.ai/
- Haiper: https://haiper.ai/
- Stable Video: https://www.stablevideo.com/
