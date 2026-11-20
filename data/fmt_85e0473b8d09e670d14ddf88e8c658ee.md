
# SD教程NO.06👉文生图

> 来源：[https://my.feishu.cn/docx/D22Ldh9rCoTT9Pxus69cT4Zunpf](https://my.feishu.cn/docx/D22Ldh9rCoTT9Pxus69cT4Zunpf)

😁今天我就从最基础的 tag 来说下文生图，大家边看边尝试，能更快上手；

文生图简单说就是用 **tag**（或称 **prompt**、描述词）让 SD **生成图像**，最基础的用法前面有说过；

比如 cat,city,car 这些词出来的就是一幅简单的图像。

![](img/cc0a63840a38a317e25ce90cdae8b78d.png)

👉如果想用 tag 做一幅**高质量**的画面，我们就需要**不同类型**的词的组合。

我个人把常用 tag 分为这几类：

1️⃣、**质量**：画风类型、画质、光效、构图、视角

2️⃣、**主体**：人物、发型、装饰、表情、动作（如果是物体的，也要描述清楚主体内容）

3️⃣、**背景**：背景、天气、物体

4️⃣、其他：画风啊啥的这里就不该细讲了，前3类差不多够用了

这里给大家一个 tag 网站，可以搜索，也可以直接挑选：🎁https://tags.novelai.dev/

**使用方法如下**，前期可以多翻翻熟悉一下，有些词需要 lora 和大模型支撑，这个下一课会讲到。

![](img/b0ede20d8f9cea28c1c786c1938ace1d.png)

以上面内容为参考我们优化后的 tag 就是：

1️⃣、质量：absurdres（超高分辨率），depth of field（背景虚化），lens flare（镜头光晕），vanishing point（远景透视画法）

2️⃣、主体：1black cat（1只黑猫），lovely（可爱），sitting on car（坐在车上）

3️⃣、背景：city（城市），neon lights（霓虹灯），night（晚上）

出来的就是这样的一个图片

![](img/58a8f402952c760194eb392848f8633f.png)

负面词大家自己适当加，一般是描述人物的时候，写入多余的手指、多余的手之类的……

tag 测完没问题的话，采样器建议换成 DPM++ SDE Karras，步数换成 28，同样的词出来的效果差很多；

![](img/5069a6a9b39f1e05934f4fc5fb42a66e.png)

之前004界面这篇有简单说过迭代器和迭代步数的关系，大家可以回顾一下。

![](img/91e9ee8b4f3b3166a7b02be1f58ecee4.png)

然后再开个高清修复看下效果，高清修复因为把512*512画面放大了一倍所以出图很多时候会出现主体过多（比如 N 只手和 N 个胳膊这种古神图），所以日常不建议大家开高清修复，正常做图，感觉画质不够需要放大更清晰的，可以在图生图模式中选高清修复来放大。这个在后面课程也会讲到。

![](img/09b7c34f50474aeac175fff864cb7e60.png)

然后，大家会在 C 站看到各种 tag 带各种括号，那些括号是什么意思给大家说明下：

1️⃣、小括号：小括号👉()👈是最常见的，小括号代表的是 1.1 倍权重，比如 `(city)`=1.1 倍权重的 city，再加一个括号 `((city))`=1.1*1.1 倍权重的 city，也就是 1.21 倍，以此类推；（这里补充一个小知识，tag 越靠前权重越高）

**为什么要加权重？**tag 太多有时候你想要的某个内容出不来，就给他加括号准没错！

**太难计算权重怎么办？**括号倍率算起来麻烦，有个简单的方法：`(city:1.5)`=1.5 倍权重的 city，记住这个就行，不用多括号了，你想 1.5、1.55、1.57 都行，直接输进去。

2️⃣、中括号：【】代表降权，**初始化**的权重是 1，降权后是 0.952 倍，不用记，知道就好；

3️⃣、大括号：{}代表的是 1.05 倍权重，不用记，知道就好；

还有一种长这样`<>`，不代表权重，里面的内容就是 lora，下一课会说到 lora，这里就先跳过了；

然后记住一个 tag：`nsfw`；这个 tag 一定要放在**负面**关键词。

下面分享一组我 chilloutmix 模型常用 tag；

记得把种子改成随机（-1）

![](img/0e72291529cbf2782155ae049876172c.png)

然后就能收获这样的一张图片

![](img/ba900860ea42bc09b7b3f365ff99a13d.png)

```
RAW photo, best quality,(masterpiece:1.5),(realistic, photo-realistic:1.5),blurry background,depth of field,cinematic lighting,The Tyndall effect,radiosity,physically-based rendering,pureerosface v1,thighhighs,original,vanishing point,

1girl,small breasts,thin,black hair,perfect skin,bead bracelet,hair accessories,earrings,long hair,ponytail,collarbone,necklace,shy,light blush,(school uniform:1.5),seductive smile,delicate face,

full body,night,(beach:1.1),standing,neon lights,jetty,fences,moon,moonlight,
```

```
Negative prompt: nsfw, EasyNegative, paintings, sketches, (worst quality:2), (low quality:2), (normal quality:2), lowres, normal quality, ((monochrome)), ((grayscale)), skin spots, acnes, skin blemishes, age spot, glans,extra fingers,fewer fingers,strange fingers,bad hand,mole，big breasts,huge breasts, large breast, naked, cleavage,breasts,((bad hands)),(fewer digits),(extra digit),(breasts:1.5),multiple breasts, fused breasts, bad breasts, poorly drawn breasts, extra breasts, liquid breasts, heavy breasts, cleavage cutout, cleavage, between breasts, missing breasts,lowres,bad anatomy,bad hands,text,error,missing fingers,extra digit,fewer digits,cropped,worst quality,low quality,normal quality,jpeg artifacts,signature,watermark,username,blurry,missing arms,long neck,Humpbacked

Steps: 28, Sampler: DPM++ SDE Karras, CFG scale: 7, Seed: 2764308787, Size: 512x740, Model hash: fc2511737a, Model: 真实chilloutmix_NiPrunedFp32Fix, Clip skip: 2, ENSD: 31337
```

虽然会自动分配上面的 tag，但咱们还是要理解一下上面的内容：

![](img/c3513eac37a2838bf12b243c1faf1699.png)

要注意的是，tag 不只是这样的单词，句子翻译一下也可以让 SD 生成图片，比如 1 个女孩穿着 JK 制服做在小船上，小船在湖面划过带起一片涟漪，只是单词比较好控制输出结果，大家可以多做一些尝试。

今天先写到这里，文生图内容大家看完有不明白的可以评论留言，我会陆续补充内容上去。

> 我是热爱生活的小黑子，热爱当下生活，永远忠于自己。
