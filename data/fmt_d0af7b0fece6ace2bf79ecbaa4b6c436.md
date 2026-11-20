
实际上`runway`一定程度上也能实现这种流体效果，示例：

PS：提示词在常规基础上，带上非牛顿流体这个名词来引导流动皆可实现，例如：舞台上，一位女生正在表演走秀，黄色非牛顿流体从头部逐步留下覆盖全身，形成一个球状。

![](img/a49680422d185c6531066196eae63cf6.png)

[融合-流体金毛和美女.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Tjqcbn7q2oLC51xsuwNcDm8Unlg)

小结：此特殊流体效果，不止隐晦的暗示，也包含了非常多而不确定性，非常适合达人秀变身这个赛道的变种和扩展。迁移此玩法，相信大家可以尝试出许多新的爆款创意。届时请告知我们，嘿嘿！

![](img/0ea56f9200a73409c5e23dd485604ea6.png)

![](img/0bb03dbd90e6cdbd999ee3af0dad8a5f.png)

## 23 图片一致性控制方案

### 多模态类：豆包、GPT、Gemini

> 豆包：简单粗暴，一个指令支持直出30-40张图，且可用性还比较强，并且免费；美中不足在于水印和精细度不足，人物同质化程度高

> GPT：豆包的单轮次版，精细度更高，但等待时间过长。

> Gemini：其实比上面两位都早一些更新，但是性能有限。

![](img/bbdec18bb480e29ecd539b3e76d443fc.png)

![](img/b480b22d0f9f692a4be78e63940841f3.png)

### 内嵌类：即梦、MJ、`imageFX`

> 即梦：即梦3.0【参考人像写真】

> MJ：全向参考命令【`--oref www.图片url.png`】+`【--ow 权重（1-1000）默认100】` MJ的老一致性命令不再赘述

> `imageFX`：种子和人脸强相关，固定种子即固定人脸，但越远就越不可控。

![](img/eb43dd807e86ff093f400e782492cf21.png)

![](img/ab455d6604eef8ab4a6abbc09fb22f77.png)

![](img/f13e8c242eb7ee5785646be0b6b7eba9.png)

![](img/5396e9caeefe17baf9d134376b2fa9d1.png)

![](img/5d008ca031d0ba23876ad63d418d8542.png)

![](img/fb39d1e7e55235b1ad2c14965493c6ea.png)

### 多图参考：`whisk`、`runway`

> `whisk`：支持非常多人物和场景和风格的参考，但相似度并不是非常高

> `runway`：没想到吧，`runway`能出图，而且效果还不错，支持最多三张图（角色/场景）的参考，`runway`无限用户狂喜

![](img/ca68fbdfc1ebb786c688f541d0d9a0f5.png)

![](img/7f99c321b6aac37b6f596c47b11b11fb.png)

![](img/7b0ccbc9a7c79e41fc34462ea131db8e.png)

![](img/9b9886948a155ffc1fa2910ca4507216.png)

### 骚操作：视频模型运镜出多角度形象

> 一个美丽的女生自然地站在舞台上，舞台上灯光闪耀，镜头渐渐拉远，环绕绕女生一周

>

> 图片高清+视频高清=N张不同角度的图片

>

> 多主体一致性

>

> `pixverse`、`可灵1.6`、`可灵2.0`都可以

![](img/d2f624ad256256eb9cf1e192d576125e.png)

[pixverse%2Fmp4%2Fmedia%2Fweb%2Fori%2Fae445761-ea9c-4c91-a541-7aa2eb52e5b8_seed1980423019.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TZX6bkeakolQDTx7OGLcG0xGnqh)

![](img/5865d35f4115fdddb7957652fec21f1d.png)

[高品质_一个美丽的女生自然地站在舞台上_舞台上灯光闪耀_镜头渐渐拉远_环 (1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OXfWbjdzvoTJ2cx302kccJVfnRe)

![](img/d736a52f722be1d16ea1143e63f4f252.png)

[高品质_一个美丽的女生自然地站在舞台上_舞台上灯光闪耀_镜头渐渐拉远_环.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WhqLboFsBoOGWYxUaiHcK7pOnLc)

### Lora类：`comfyui`、`stablediffusion`、第三方集成网站

> 细讲太复杂，简单讲用不上，留着以后`comfyui`的专场分享来讲，这里略

### 纯文本控制：适用于简单形象或是大模型认识的形象

> **动物示警、动物故**事、动物救援等等对于形象精度要求不高的都可采用该方式进行控制

> 原理其实就是**通过提示词限定了形象的范围**，比如说“一只体型较小的无角梅花鹿”，这个属于简单且精度要求不高的形象，模型一般不会有针对该形象的大量训练集，所以固定了提示词后就不会有多少区别

> 但话说回来，精度不高的图片都有豆包一键出套图了还要纯文本控制作甚？

![](img/5b9d8f9b0b56c59de67d3c8b8e09b2c7.png)

![](img/107c207333ba80ca0e6133ae3cbff1a9.png)

> 有了上面这些技巧，基本所有 AI 赛道的视频都能复刻出来，自己多实操一下就知道没那么难，最重要的是先模仿爆款，然后慢慢去找到自己的风格和方向，找到灵思泉涌、适合自己的细分赛道。

# 模型竞技场

可灵2.1和可灵2.1大师版今天（5月29日）正式发布，2.1性能全面领先1.6和2.0大师版接近，且价格与1.6持平，但不支持首尾帧，案例测试等下个月积分刷新再补一下，充值实在太贵，这个月积分用完了...

### 首尾帧

> 个人主观评价：首尾帧场景中，`pixverse4.5` ≥ `pika` ＞ `vidu` ＞ `可灵1.6` ＞ `runway`

> 自动化出首尾帧推荐`vidu`或者`runway`（无限额度）

> 手搓首尾帧仅推荐`pixverse`，效果好太多

#### 小幅度变身

> 美丽女生在舞台上进行魔法变装表演，将手上的一只蝴蝶变化出很多很多蝴蝶落在自己的连衣裙上，蝴蝶和裙子融为一体，雾气特效，摄像头旋转

> `可灵1.6`、`runway`、`pixverse`、`pika`、`vidu`

>

> 考核点：人物灵动性、动作自然度、指令遵循度

![](img/c6733add09f381e0a332b8a7d6c0b2f5.png)

[高品质_美丽女生在舞台上进行魔法变装表演_将手上的一只蝴蝶变化出很多很多.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JNfmbwl8No5fWgxae1TciTGUnMg)

![](img/4b55b0ec84ba4c4e78bc30a9e1cb49fb.png)

[Gen-3 Alpha Turbo 9139984, 美丽女生在舞台上进行魔法变装表演，将手上, 0_3 (6)png, M 5.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GnVXbxTkbo2TB6xhQN6c1gkWnLh)

![](img/ae876c40f34bd663648339acdf9c15f4.png)

[pixverse%2Fmp4%2Fmedia%2Fweb%2Fori%2Fff036cae-8bb8-41e0-b623-125e1ee91c74_seed363859670.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VHPYbEsIPoi2yAxyJzicYom3n9g)

![](img/d3e28b77fc63a06311af6255dc39bbef.png)

[_________________________________________________________________seed1794695452.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/M1B3bod15oanaIxxfJzcTzedn5d)

![](img/2ba610085105b42b3b2c654bab6dcf7f.png)

[vidu-video-2776275982272181.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LoDIbXcpKoxmvqxYUAVcVyUYnqd)

#### 大幅度变身

> 美丽女生在舞台上进行魔法变身表演，女孩转身，散发出星光特效，变身成了一个木棍人，超强动态画面，摄像头旋转

>

> `可灵1.6`、`pixverse`、`pika`、`runway`、`vidu`

>

> 考核点：变身流畅度、指令遵循度、主体稳定性

![](img/2baf6c6e641fd3221c10c1d171e287a2.png)

[高品质_美丽女生在舞台上进行魔法变身表演_女孩转身_散发出星光特效_变身.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EhljbkfYmoWoIGx3nCPcOkoenvc)

![](img/bf26b0facf89a3cd43113367ac71998d.png)

[pixverse%2Fmp4%2Fmedia%2Fweb%2Fori%2F7fb921c3-6ddc-4fee-9328-4fb8ff55a14c_seed1030119206.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GyHkbdcJMoRwISxcVlzccqYCnoc)

![](img/71f9dc71cfd868d74b6a7f3a9642699a.png)

[_____________________________________________________seed1206676721.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WFLTbPlirouokVxImRHcKalMnDg)

![](img/4dc66e6e5db94f55a5fece108c6ca864.png)

[Gen-3 Alpha Turbo 3204281745, 美丽女生在舞台上进行魔法变身表演，女孩转, 0_3 (6)png, M 5.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FMN5bEMxnooHvqxYUurcahLxn9F)

![](img/a8a3a4567df5f32943f772b54a971435.png)

[vidu-video-2776276331055764.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Rv6SbjdEMoQPBexd51ycPzCqnWX)

#### 一般幅度变身

> 女孩抬起手变装，超强雾气特效，超强动态画面，超强变身，摄像头旋转

>

> `可灵1.6`、`runway`、`vidu`、`pixverse`、`pika`

>

> 考核点：变身流畅度、特效效果、动作自然度

![](img/d33daaecfd2d969650f86cdfa54ad5d0.png)

[高品质_女孩抬起手变装_超强雾气特效_超强动态画面_超强变身_摄像头旋转.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CZ6WbXYCEoY6QbxhBkrcAfFxnjn)

![](img/88a99560ae5b0009c0bb74e91bf2d478.png)

[Gen-3 Alpha Turbo 1452184241, 女孩抬起手变装，超强雾气特效，超强动态画, image (5)png, M 5.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WXjKbgjDkoHoNLxnEyicgW2qnVf)

![](img/f2c640dc4c00031af1b25d9b8b9439f0.png)

[vidu-video-2774912913031913.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Jlv7bVgWmoDViSxvOr6ckHc8n5e)

![](img/f15957155c7b39b44a464c05bd8425d3.png)

[pixverse%2Fmp4%2Fmedia%2Fweb%2Fori%2Fecbd9531-ef99-4690-80d2-4f6d17a91158_seed485962217.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Rs8YbVQGYo5BMax7DSpcwdyBnpe)

![](img/7f7d53e7950948ada3c65e04180741f4.png)

[_________________________________seed954195820.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OrR4bwyxuoYewExvYJ4cgExGnmh)

### 单图生视频

> 个人主观评价：单帧生视频中

> `可灵2.0` ＞ `pixverse4.5` ＞ `可灵1.6` ≥ `viduQ1` ≈ `即梦3.0` ＞ `runway Gen4` ≈ `Veo2`

#### 名人视频

> 两个男人在酒吧互相碰杯，勾肩搭背，阴险地大笑，仿佛计谋得逞，电影感

> `可灵2.0`、`可灵1.6`

> `runway`、`pixverse`、`vidu`（`vidu`是真把流量玩明白了）

> `veo2`、`即梦`=🈲

> 考核点：是否能生成、指令遵循度、动作自然度、人物真实度

![](img/dfc92e507d89b171e966f3dcf4e8ff4f.png)

[高品质_两个男人在酒吧互相碰杯_勾肩搭背_阴险地大笑_仿佛计谋得逞_电影 (1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/P1rjb7HpQoVV0VxfKwAce3TenYe)

![](img/7baed859e80a1b50401dc806466fae37.png)

[高品质_两个男人在酒吧互相碰杯_勾肩搭背_阴险地大笑_仿佛计谋得逞_电影.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HIbnb3Y8loPNykxTPgZcQc9Hn6b)

![](img/c5f96e0775fe427fc1e985110e719482.png)

[Gen-4 Turbo 两个男人在酒吧互相碰杯，勾肩搭背，阴险地大笑，仿佛计谋得逞，电影感 4264569938.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UGOOblmerohkQqxZWC6csUZvnfb)

![](img/8ec039d25cf7ef96fbf81387bcb06402.png)

[pixverse%2Fmp4%2Fmedia%2Fweb%2Fori%2F30071bc6-cd16-40a4-b669-3250680f13a6_seed1723350631.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Lpd6bDuBrohgcFxub8ncwltVn3c)

![](img/88cc0c665bcb0070afd7e0ac005da0ce.png)

[vidu-video-2776300469607085.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Lhv5b56KboJNccxyyniccPMHnyl)

#### 吃东西-高难度

