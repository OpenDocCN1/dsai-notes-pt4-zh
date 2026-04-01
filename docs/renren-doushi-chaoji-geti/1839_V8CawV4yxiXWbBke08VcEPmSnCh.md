# SD 艺术字实战：小赚几百块

> 来源：[https://xuqiwei1986.feishu.cn/docx/FG76djYZkovoOZxn5bScoLp7n1d](https://xuqiwei1986.feishu.cn/docx/FG76djYZkovoOZxn5bScoLp7n1d)

大家好，我是元峰，6 颗 AI 之心持有者。 AI 超级个体践行者，喜欢研究学习 AI 工具。正在参加 AI 之心写作训练营。

今天分享一下最近通过 SD 艺术字小赚的实战案例。

![](img/34c2932063ed92561d7dc7c7955863f1.png)

前有艺术二维码，后有赛博朋克机车，这几天这种光影文字又开始火起来了。

制作起来，倒也不是很难。主要就是运用 Stable Diffusion 的 ControlNet 插件。

# 一、艺术字制作流程

## 一） SD 环境准备

### 本地安装

对于 SD 的安装，不过多赘述。网上已经一大堆了，不过最要推荐的就是当红辣子鸡 B 站的秋叶大神的安装包。但是，本地部署对电脑配置要求比较高。

![](img/8fca9128ad5fc1be0844a3e60d7eea56.png)

[https://www.bilibili.com/video/BV1iM4y1y7oA/?shar](https://www.bilibili.com/video/BV1iM4y1y7oA/?share_source=copy_web&vd_source=6c09aa7f53b69992e1cd572bbd2dbf94)e_source=copy_web&vd_source=6c09aa7f53b69992e1cd572bbd2dbf94

### AutoDL 云端部署

我一般会选择云服务器 AutoDL 来运行，即开即用，没有物理条件的限制。具体的教程，我推荐我们星球小潘总的步骤，非常详细，傻瓜式教程。

1.  硬件要求不高

1.  操作简单，不用自己部署环境

1.  可以直接关联百度云盘和阿里云盘，无需下载文件到本地

![](img/9c38884434cbf5d3c0a8958bf8300422.png)

![](img/5019db266f744d92d16dcb09fcdda069.png)

而且按照此下来，我们需要的 ControlNet 也已经内置安装，无需再自己手动下载。

## 二）光影文字

+   跑图必备的基础模型：diffusion_pytorch_model 或 beautifulRealistic_v60

+   ControlNet 的模型：今天我们用的这个 Brightness 模型比较特殊，他不是官方的 ControlNet 的模型。需要我们自己下载，文件已经在前面分享的网盘里面。

+   下载后将此模型放入 autodl-tmp/models/controlnet/ 文件夹下面。

![](img/b6e6a9795bbf351394d111e74ecb5d93.png)

百度网盘链接：[https://pan.baidu.com/s/1pu](https://pan.baidu.com/s/1puGMdHA1YLoin8poZNH28A)GMdHA1YLoin8poZNH28A 提取码：0vck

### SD 参数配置

![](img/c3a4e515011e1f399951389b8ea20b24.png)

![](img/5d1724ac142491b67422febd4246cf7d.png)

**S**table Diffusion 模型(ckpt)/Stable Diffusion checkpoint：diffusion_pytorch_model

采样器/Sampling method：DPM++SDE Karras

采样步数/Sampling steps：一般选择 25-30 之间

**宽度和高度**：根据自己图片宽高比自己设定，我这里是横图 832:512，竖图 512:832

生成次数/Batch count：4，每次抽卡个数

### 提示词

从不同渠道收集了 3 个提示词。

#### 提示词1

**正向提示词：**

A melancholic autumn scene in a vast flower field,a gentle breeze rustling through the dry grass,fallen leaves scattered among the flowers, a bittersweet atmosphere, a moment of quiet contemplation,1girl,long hair,white_skirt, high-waist_shorts, outfit ,roses,(dynamic angle:1.1),vivid,Soft and warm color palette, delicate brushwork, evocative use of light and shadow, wide shot,subtle details in the wilting flowers,high contrast,color contrast

在一片广阔的花田中，一个忧郁的秋天场景。微风拂过干草，落叶散落在花丛之间，弥漫着一种苦乐交织的氛围。这是一个静谧思考的时刻。画面中有一个女孩，长发、白裙、高腰短裤和装饰着玫瑰花。(动态角度: 1.1)色彩生动而柔和温暖，精致的笔触展现出光影效果，并以宽广镜头呈现。凋零花朵中微妙的细节与高对比度、色彩对比相得益彰。

#### 提示词2

**正向提示词：**

High quality photography works, perspective, shooting on the overpass, fair skin, full body photos, cute (little girl robot), gorgeous colors, manipulator, short, pink big eyes (starry eyes), Pigtail with gradually light pink, sweet and happy style, dynamic ten foot running posture, (walking on the glass walkway), (artificial intelligence), warm background, soft (light effect), lipstick light pink, (eyes) camera close-up, Prism effect, white armor adorned with colored circles, (cartoon humanoid mechanical art style), deep red Lolita skirt style

高品质摄影作品，透视，在天桥上拍摄，皮肤白皙，全身照片，可爱（小女孩机器人），艳丽的色彩，机械手，短，粉红色的大眼睛（星光眼睛），逐渐浅粉色的辫子，甜美快乐的风格，动感的十脚跑步姿势，（走在玻璃栈道上），（人工智能），温暖的背景，柔和的（光效），口红浅粉色，（眼睛）相机特写，棱镜效果，白色盔甲饰有彩色圆圈，（卡通人形机械艺术风格），深红色洛丽塔裙子风格

#### 提示词3

**正向提示词：（我主要用这个）**

**master**piece,best quality,highres,street,light,at night

这里主要换不同的提示关键词（**street**），例如女孩，沙滩，大海等，其他的可以不变。

反向提示词：

(((nsfw))),EasyNegative,badhandv4,ng_deepnegative_v1_75t,(worst quality:2), (low quality:2), (normal quality:2), lowres, ((monochrome)), ((grayscale)), bad anatomy,DeepNegative, skin spots, acnes, skin blemishes,(fat:1.2),facing away, looking away,tilted head, lowres,bad anatomy,bad hands, missing fingers,extra digit, fewer digits,bad feet,poorly drawn hands,poorly drawn face,mutation,deformed,extra fingers,extra limbs,extra arms,extra legs,malformed limbs,fused fingers,too many fingers,long neck,cross-eyed,mutated hands,polar lowres,bad body,bad proportions,gross proportions,missing arms,missing legs,extra digit, extra arms, extra leg, extra foot,teethcroppe,signature, watermark, username,blurry,cropped,jpeg artifacts,text,error,Lower body exposure

### ControlNet 参数配置

**1）先用**其他图片编辑器（PS，可画等）做好黑底白字垫图。

![](img/809994593ed6f992a9847a2ea3021e42.png)

2）设置参数

![](img/e1fab76e18245ca7d06ef91c9c4e1281.png)

**启用**：勾选

**完美像素模型**：勾选

**允许预览**：勾选

**预处理****：n**one，不用选

模型：control_v1p_sd15_brightness.safetensors（刚刚从云盘下载的）

接下来是最重要的两个参数，**Control Weight **和 Ending Control Step。

**Control Weight：** 建议 **0.4-0.65**。我这里也调整了好多次，这里数值越大，图案就会越明显，但相对的，文字和图片的融合度也会越差。可以自己多试，比较复杂的文字为了出效果可以调高点，简单的图案可以调低点。

Ending **Control **Step：这个参数建议 0.6-0.75，代表着 ControlNet 什么时候停止介入，数值越大后面留给模型处理融合的时间就越少，文字融合度就会变差，数值越小模型介入过早就会破坏已有的结构导致看不清文字。也得多尝试，跟你图案的复杂程度也有关系。

### 高清化图像

选中图片，点击 **>>高清化**。

![](img/0d95e6d24be72b3031adaf1217ec9d08.png)

**缩放比例/Resize：****2倍或者4**倍还是其它倍，自己设置

放大模型：R-ESRGAN 4X+

![](img/46352570f969f9b75b352e2a91eb78ed.png)

完成之后，下载右边高清图即可。

![](img/1543f1c41d8642d7c32bf8fdfa7f5478.png)

牛逼

![](img/c8def731e8addf3c53d1a0342ae22baf.png)

元峰

![](img/cbe5062bd9497af1a07b33d7c31eaba9.png)

gptfeng

## 三）**海报艺术字**

![](img/b0a1eb3e3683077969783d656744254e.png)

![](img/f5c5ec506ff0dff71d9e5f5e44b2ee04.png)

### SD 参数配置

**大模型**：revAnimated_v122.safetensors

**正向提示词**：

Highest quality, masterpiece

Buddha , towering statues,  temple, graphic tablet, incense burner,

根据不同环境和主题，换划线这部分的提示词

**反向提示词：**

EasyNegative,bad quality

![](img/5b8e0b4d2fb6432a87e4f18d66de688b.png)

### ControlNet 参数配置

**先用其他**图片编辑器（PS，可画等）做好白底黑字（注意和光影图相反的）垫图。

![](img/2a6be078fe800eae7dbb13d7065ee68b.png)

**启用**：勾选

**完美像素模型**：勾选

**允许预览**：勾选

**预处理**：invert (from white bg & black line)

**模型**：control_v11f1p_sd15_depth fp16(4b72d323]

**Control Weight：**1，可以根据图具体调整

Ending Control Step：1，可以根据图具体调整

### SD 艺术字 和 Mj 背景图结合

AI 佛祖，这个 B 班我真的是上够了。最近小红书新出的一种AI账号，目标人群很简单：就是不想上班的打工人。

这种账号靠的就是**吐槽性的文案+搞怪的图片，**图片一般都是用 Midjourney 做出来的，咒语网上一堆，后面我会帖出来。

和之前小红书爆火的不想上班、不想打工的表情包账号差不多。看了一圈，这类账号数据最好的一条几千赞，目前做的人还很少。就想和这种海报字做一个结合。

不过变现手段比较单一，目前就看到用小红书笔记带货、卖课程的。

算是一个非常小众的方向。

![](img/34948499b15661f4b74fd94ede4de65a.png)

但刚刚我们用 SD 生成的图片略显单调，还缺少一些佛祖的元素。

### Midjourney 搞怪佛祖

**/imagine prompt: buddha FACE,light GRAY,behind the table and work overtime,black eyevery tired kbhit, napping on the chair wood**en fish,The computer shines light on the face, full body futuristic office, Burning incense, by warm candlelightv --ar 512:688 --v 5.2 --q 2

![](img/04029d9a24789a2ab567a3fbc0ef3f01.png)

结合图片主题，“上班 OR 上香”，选择第 4 张，更符合氛围。

左边是工作的电脑，而佛祖却在“摸鱼”睡觉，右边是上香。

图中的上香部分只出来一部分，再用 Mj 的“平移”功能，向右边延伸一下，使画面变得完整。

![](img/dbf72a507f82888f48473f2a5748ef13.png)

### 用 PS 融合

1) 对 Mj 生成的图片保留主题，扣除背景

![](img/22e4c531f682040362b53360be68fa50.png)

2) 抠图，拿出被覆盖的文字部分“香”，图层放置到最上面

![](img/7059feabecbbc41e6df017d9d9de0947.png)

3) 换佛祖形象，因为这个图中佛祖不太符合，所以换一个更像如来佛祖的那种。

![](img/5425c0f6b52a22b3a660ff126c956d7e.png)

**4) 最后**，觉得融合不到位的地方，用“创成式填充”功能修修补补即可。

![](img/e7d52b108cae9495d22d66faac0dad7d.png)

## 四）制作视频

除了绘制图片，我们也可以将画好的图片制作为视频。

例如最近刀郎的新歌《罗刹海市》又跟当时的《2002年的第一场雪》一样，一夜间火遍大江南北。有意思的是，这首歌充满了讽刺和暗喻。

![](img/f1ae8febcb305fcd2283fce5c0be50b8.png)

**下面**的 3** 句**歌词里面有一些词 “马户”，“又鸟” 就很容易结合前面的光影文字和海报文字做图。“罗刹国” 紧扣歌名主题，“马户”即驴，“又鸟”即鸡。

> 罗刹国向东两万六千里

> 那马户不知道他是一头驴，那又鸟它不知道她是一只鸡

> 可是那从来煤蛋儿生来就黑，不管你咋样洗呀那也是个脏东西

![](img/fc25231c9f5bb17fde3c31c3d9c13aaf.png)

视频软件我选择的是**剪映**，里面搜“罗刹海市”，一堆的模板**，选一个合适的把自己的图替**换进去。注意，最好画面和歌词匹配对应起来。

![](img/e88ffe4f3e9ca2dc7994ef9d830bf187.png)

Show time！

![](img/7809c18e6cfc55e8b49a04b81187caa9.png)

[C0C306BF-B69C-4F1C-9295-338498E64122.MP4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AuB0bPjsyoRzmxxDGUMcbD1hnDf)

# 二、铺内容获取流量

## 一）公众号

![](img/ea28be848c5eec5d4fbf2c4f48b6359e.png)

## 二）小红书

![](img/340055a441032ed4cf7d463a5f42a547.png)

## 三）抖音

![](img/7485daf7156d07d2b2d3125a122d06c6.png)

## 四）知乎

![](img/201c871b4a17ab3ed8c8498fc069fc6d.png)

![](img/56622e19a4cb7e0b18e4498c483bbd44.png)

## 五）视频号

![](img/8fe58c3838ba7346beddf4703be4db2c.png)

通过本次光影文字这个主题文章，在各个平台进行分享。获得了一些流量，虽然数据量不大，但是对比同账号，这次的流量都是刷新新高的。

核心就是：美化作品，追热点，日更，多平台分发，广撒网。

# 三、我如何进行变现

## 一）定制图片

![](img/8677a9ed6793b81e7e3be913c6476e81.png)

## 二）卖课收徒弟

![](img/2a6e207282545b32f6bca3b085b303d3.png)

非常丝滑的引流

![](img/94d22fbcd8d397b639ab55b82c72e6fa.png)

## 三）课程群接单

![](img/7335a6ec2293fe713d9d7b2d24f96f6a.png)

# 四、总结

在本文中，我详细地探讨了如何利用 Stable Diffusion 技术和 ControlNet 插件制作光影文字的全面步骤，包括准备基础模型、设置提示词、配置 ControlNet 参数等重要环节。文章的核心在于阐述如何通过 ControlNet 引入预设的文字图层，并与 Stable Diffusion 生成的图像进行高效融合。

此外，我还分享了如何创作文字海报的一种独特方法。先通过 SD 技术生成艺术字，借助 Midjourney 生成背景图像，再使用 PS 进行融合。利用这种独特的方法结合不同 AI 工具，我们可以创作出更为丰富多元化的图像作品。也可以将图片制作为视频。

对于 AI 创作新手，我们推荐了一款易于安装的 Stable Diffusion 教程以及一个简洁易操作的云端部署方案，后者对硬件配置的要求相对较低。此外我也还分享了我通过这些 AI 绘画作品获得更大流量的有效渠道。

结合最近几天的咨询情况，列举出了如何将此类创作转化为收益的有效途径，包括提供定制服务、销售相关课程、学员接单等多种方法，这些都应该能为创作者提供宝贵的参考价值。

其实最近的项目，无论宝宝彩超，AI 小和尚，**SD 艺术字，其实万变不**离其宗，关键就是先入局，持续学习，再优化。

最后，非常感谢洋哥和小林组局 “AI 之心写作营”，特别是小林对我文章的梳理及提供建议思路，才会让整体作品更完善。

如果此文你对有帮助，欢迎点个赞，听说点赞的人都会有好运锦鲤附身，流量大爆发哦。

![](img/f2c9d3b07aaf0e63670907e3a17b2f4f.png)