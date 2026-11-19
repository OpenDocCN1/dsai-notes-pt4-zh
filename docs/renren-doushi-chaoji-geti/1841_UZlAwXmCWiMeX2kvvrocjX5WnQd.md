
# 2023-07-15手把手教你用 `SD` 生成文字形状的光线，用来做营销宣传图非常有效

> 来源：https://xuqiwei1986.feishu.cn/docx/HLnjdlkLto202axRyrmc0l2EnKc

https://mp.weixin.qq.com/s/rvpU4XhToldoec_bABeXJw

昨天类似这种文字光效或者将文字合成在衣服上的图好像又火了，很多朋友在问怎么做，其实就是`ControlNet`的简单应用，比如模拟光线照射文字的，之前用来调整自然光线的角度和范围这次不过是将原来模拟光照的图变成了文字了。

好的接下来开始我们的教程，教程分为三个部分第一部分会大致讲一下`Stable Diffusion`和`ControlNet`的安装这里不会细讲，之后会讲一下用`Brightness`模型去控制深度信息从而让文字和生成的要图像融合。

这类图片用来跟品牌元素融合之后，用来做营销宣传和品牌传播还是挺有用的。

## 注意：

+ 这个视频需要你有`Stable Diffusion`的使用基础，因为在基础部分我不会讲的太细。

+ 另外教程用的整合包只支持`Windows`电脑，如果你的电脑没有一张好的显卡的话建议云端部署，这个教程不会涉及，感兴趣的人多的话我后面单独出一期。

+ 我的`Stable Diffusion`界面可能看起来跟你不太一样，因为我装了主题，不过功能的位置和名称都是一样的所以不用担心。

![](img/f2e294e8d3a9e012d2b5c1f87650b9fe.png)

## `Stable Diffusion` 的安装

首先是`Stable Diffusion`的安装。我们还是去秋葉aaaki的视频下载他做的整合包，这个步骤很简单就是直接解压就行。视频也有详细的安装教程。

https://www.bilibili.com/video/BV1iM4y1y7oA/?share_source=copy_web&vd_source=6c09aa7f53b69992e1cd572bbd2dbf94

之后在解压的文件夹找到这个启动器启动就行。

![](img/191b32600572fb0777d9a20e13dc8014.png)

大模型我用的`Beautiful Realistic Asians`，可以在这里下载，当然你也可以用你自己喜欢的：https://civitai.com/models/25494/beautiful-realistic-asians

下载结束后点开秋叶的启动器，左侧`tab`模型管理，右侧选择`Stable Diffusion`模型，然后点击上方的打开文件夹，把你下载的模型粘贴进去就行。搞完之后点击第一个`Tab`的一键启动按钮等待启动就行。

![](img/2e70bb6de4997d87fa94d9f8746dc6eb.png)

## `ControlNet` 的安装

接下来我们需要安装`ControlNet`插件和下载模型。我这里写的比较简单可以去秋叶的这个视频看过程顺便从视频那里下载`ControlNet`需要用到的模型。

https://www.bilibili.com/video/BV1fa4y1G71W/?spm_id_from=333.337.search-card.all.click&vd_source=e99f85042059f2864f5cca20d71575f0

首先你需要在打开的`Web UI`界面找到扩展`tab`，点击下面的可用，再点击从网址安装，输入这个链接https://github.com/Mikubill/sd-webui-controlnet 点击安装就行。装好之后重新启动`web UI`。

![](img/edb706da89e86f0d2eaa32e5601ec9b4.png)

这里我们用的这个`Brightness`模型比较特殊他不是官方的`ControlNet`的模型，前几天的`SD`艺术二维码用的就是这个。

然后我们下载`Brightness`模型。在这里下载就行：https://huggingface.co/ioclab/control_v1p_sd15_brightness/tree/main

将模型文件放到 `\novelai-webui-aki-v2\models\ControlNet` 下就行。

如果你访问`Huggingface`太慢的话，也可以从我这个百度云下载：

链接：https://pan.baidu.com/s/1pzxikDpuQMeUbAM_0HCdQA提取码：e8j0

## 图片生成

这个模型是前几天做`SD`生成二维码的时候火起来的，本质上还是控制图片的光线和深度信息，不过在`Depth`的的基础上与环境和内容融合的更好不会向`Depth`那样生硬。接下来是正式的步骤：

还是走文生图的步骤

首先输入我们的提示词和反向提示词，我把我用的参数贴在这里：

提示词：A melancholic autumn scene in a vast flower field,a gentle breeze rustling through the dry grass,fallen leaves scattered among the flowers, a bittersweet atmosphere, a moment of quiet contemplation,1girl,long hair,white_skirt, high-waist_shorts, outfit ,roses,(dynamic angle:1.1),vivid,Soft and warm color palette, delicate brushwork, evocative use of light and shadow, wide shot,subtle details in the wilting flowers,high contrast,color contrast

反向提示词：(((nsfw))),EasyNegative,badhandv4,ng_deepnegative_v1_75t,(worst quality:2), (low quality:2), (normal quality:2), lowres, ((monochrome)), ((grayscale)), bad anatomy,DeepNegative, skin spots, acnes, skin blemishes,(fat:1.2),facing away, looking away,tilted head, lowres,bad anatomy,bad hands, missing fingers,extra digit, fewer digits,bad feet,poorly drawn hands,poorly drawn face,mutation,deformed,extra fingers,extra limbs,extra arms,extra legs,malformed limbs,fused fingers,too many fingers,long neck,cross-eyed,mutated hands,polar lowres,bad body,bad proportions,gross proportions,missing arms,missing legs,extra digit, extra arms, extra leg, extra foot,teethcroppe,signature, watermark, username,blurry,cropped,jpeg artifacts,text,error,Lower body exposure

提示词这里需要注意的是，尽量不要用一些例如特写人像之类的提示词，不然文字或者图案就会盖在人脸上，很难看，其他参数和模型就随你的喜好就行，也可以参考我图里的。

![](img/c8c861c4a24591454956d08b6ce955b0.png)

![](img/30366a8a97153db9ec0a49016bb6de8b.png)

常规的信息填写完之后，我们要来处理重点的`ControlNet`了，首先需要在下面找到你安装的`ControlNet`，点击展开。

![](img/416e10bb670b821615fa739c71ad777c.png)

接下来我们需要准备类似我下面的黑底白字的图片，白字的内容决定了画面里展示的内容，这个比较好做我自己是用`Figma`做的你如果要是会用`PS`用`PS`做也行。实在不行的话打开`Word`打字然后截图也行。我这里文字放的靠下也是想避免文字被盖在人脸上。

如果你懒得搞的话我在分享模型的文件夹里面也放了几张图你可以用来测试。

之后点击启用的对勾，然后把`Pixel Perfect`的对勾也打上。

![](img/4765e034afa8bef1bd9279495781875e.png)

之后预处理器这里我们不选择默认就是`none`，模型这里选择我们刚才下载的`control_v1p_sd15_brightness` 这个模型。

![](img/eb5375a4c4b582ef87affd3de790e1f3.png)

接下来是最重要的两个参数，`Control Weight`和`Ending Control Step`。

`Control Weight`建议0.4-0.65。我这里也调整了好多次，这里数值越大，图案就会越明显，但相对的，文字和图片的融合度也会越差。可以自己多试，比较复杂的文字为了出效果可以调高点，简单的图案可以调低点。

`Ending Control Step`这个参数建议0.6-0.75，代表着`ControlNet`什么时候停止介入，数值越大后面留给模型处理融合的时间就越少，文字融合度就会变差，数值越小模型介入过早就会破坏已有的结构导致看不清文字。也得多尝试，跟你图案的复杂程度也有关系。

![](img/05737d859a9c328d76f7a9e8034db3e9.png)

好了基本上到这里教程就结束了，最后放几张我做的其他图。其实用`Depth`这类深度模型也可以做这种图不过融合没有这个效果好，另外也比较复杂。你也可以把这个模型跟其他模型结合来控制更多内容，比如和`openpose`结合固定人的位置避免文字出现在不该出现的位置。

这个教程搞得时间很长，如果觉得有帮助可以帮忙转发给你的朋友或者点个再看，感谢各位。

![](img/77e2a51390e53883d743db43e20caf37.png)

![](img/dabafe7e975c4d833e603800c934d7f3.png)

![](img/1f9fc3019c7237bc7b2408d702dac126.png)

这张是用`Depth`做的。

![](img/e02a3150b880ec7b68d84d12ee2b82d5.png)
