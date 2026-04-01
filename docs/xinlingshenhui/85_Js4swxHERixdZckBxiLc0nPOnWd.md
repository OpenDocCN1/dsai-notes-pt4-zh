# Stable Diffusion提示词快速入门

> 来源：[https://my.feishu.cn/docx/PqCddJbr8o6MZ9xm9ZCcp4x9nwb](https://my.feishu.cn/docx/PqCddJbr8o6MZ9xm9ZCcp4x9nwb)

这节教学Stable Diffusion的提示词(prompt)写法，并附上范例。

![](img/6625bbe0326a3bab9e31f2ad946997f6.png)

我们来看提示词要怎么写，以文生图模式的提示词为主。若要边看边练习的话，可以搭配文生图操作。

不论用哪个模式生图，Stable Diffusion都是吃「正向」与「负向」提示词 (prompt，通俗的讲法叫关键字、咒语、指令)，并且要用英文输入。

你会在Stable Diffusion WebUI看到以下框框：

![](img/cebb525d0be9fa5215ebe4da0993aba7.png)

第一个框框是提示词 (Prompt) 告诉AI要生成哪些东西。第二个框框是负向提示词 (Negative prompt) 告诉AI不要生成哪些东西。

首先要注意的是，AI生图目前还没有涵盖全宇宙所有图片的模型，所以除了提示词外，使用的生图模型亦会大大的影响生成结果。

譬如，用写实风格的Stable Diffusion模型画动漫人物，效果绝对会比使用Anything差的许多；又或者，想画出宛如真人coser的图，用ChilloutMix铁定比用只强调写实的RealisticVision要来的合适。因此在学会下提示词以前，要先按照用途挑选适合的模型。

## 一.提示词 Prompts

这一节使用HD-v22的模型做示范，因此生成出来都会是动漫风格。

1.下提示词时，利用人事时地物的概念，写个英文句子描述图内场景，尽量将想看到的提示词排在前面。例如在提示词的字段填入「一个亚洲女孩在沙滩上」，负向提示词留空

> an asian girl on the beach

结果如下

![](img/739eee993755ad3f899e36bb179f0b1c.png)

2.不过英文不好的人其实不用写整个句子，用单字加逗号将想看的特征点出来通常就有不错效果。且实务上我们会希望测试更多参数，例如加入画风、要模仿的艺术家风格、身体特征等，因此用逗号更方便调整。将图片特征拆成单字就会变成下面这样：

> 一个女孩，亚洲人，沙滩，海洋

> 1girl, asian, beach, ocean

再算一次，但是结果变了？没错，即使提示词相近，AI绘图每次生图结果都是随机的。要维持上一次的结果并微调，你得保留每次算图的种子码(Seed，SD WebUI界面左下角，亦会写在文件名上)。

![](img/22eda431d26126739fe391e06c277621.png)

3.接着可以多指定一点细节：这个女孩眼睛是什么颜色的？发型是什么？有戴帽子吗？是站着还是坐着呢？身上穿的是什么衣服？天空是什么颜色的？

> 一个女孩，亚洲人，蓝眼睛，短发，草帽，站着，蓝色比基尼，沙滩，海洋，橘色天空

> 1girl, asian, blue eyes, short hair, straw hat, standing, blue bikini, beach, ocean, orange sky

![](img/a5755b1fb1bf66cf1a7524a8cfa93e28.png)

呼，好像一口气加太多细节了呢，详述细节的好处是避免AI乱生成，但也限制了其作画创意。所以下面的例子会缩短，有时候让AI随机抽奖也不坏呀～

4.要改变风格，可以在提示词加入「风格」的提示词。这方面的提示词有：相片(photoshop)、3D建模(3d model)、装饰艺术(art deco)、石像(stone sculpture)等风格。

> 参考翻译：一个女孩的石像，亚洲人，蓝眼睛，沙滩，海洋

> a stone sculpture of 1girl, asian, blue eyes, beach, ocean

![](img/14b5e34205e0a7bfc4c33c983eaaecc5.png)

但我使用的模型已是专攻动漫风格的模型，所以不用特别指定要画出哪种风格，它一律都会是动漫风的图片，很难画出真人照片，所以说挑对模型很重要啊。

5.风格是很难界定的东西呢，一般来说我们会图片像哪个艺术家的风格对吧？那么不如直接「召唤」他们吧！提示词可以包含某位画家的名字，AI会尝试模仿其风格。例如加上「慕夏的作品」，并加入「大师级作品」的风格提示词：

> 慕夏的作品，大师级作品，一个女孩，亚洲人，蓝眼睛，沙滩，海洋

> artwork by Alfons Maria Mucha, masterpiece, 1girl, asian, blue eyes, beach, ocean

生图的结果就会变得像是慕夏的作品。顺带一提masterpiece这个提示词还蛮万用的，可以让作品维持一定水准。

![](img/364325e34c737d58a701047237d7726c.png)

6.当然还可以画特定动漫人物的「二创」，只要将名字和作品名称打上去就可以了。有名的动漫角色只要出现名字，不用特别指定身体特征就会生成原作风格了，例如指定《新世纪福音战士》的惣流·明日香·兰格雷：

> 新世纪福音战士的明日香，大师级作品，一个女孩，沙滩，海洋

> asuka_langley_souryuu from neon_genesis_evangelion, masterpiece, 1girl, beach, ocean

![](img/660416eae07079e3751b32b0242b0d33.png)

再次提醒，在写提示词的时候，生成图片使用的模型也需纳入考量。譬如想要生成特定动漫人物，去Danbooru找到了英文姓名，加上提示词，但你却用画真人的Stable Diffusion去算，则AI可能根本就不会认得你说的角色是谁，反之亦然。Danbooru的标签风格跟一般图片的描述还是有差距的。

除非日后有搜罗全部网络图片的超大模型出现，否则下提示词须配合模型种类来决定。

如果AI怎样都生成不出你要的风格或人物，请考虑换个模型，或者自己训练模型。

## 二.负向提示词 Negative Prompts

上面的例子，我们只有写提示词，但下面的负向提示词的字段都是空白。但生图时常常还需要加入一些负向提示词，避免掉不好的结果。AI绘图有时不会一次就算出好结果，所以还需要加上负向提示词来控制，尤其是大批算图的时候更为重要。

1.负向提示词会加入一些常见的「不好」的图片特征，例如低画质、最糟品质、画家签名、模糊、浮水印

> deformed, lowres, bad anatomy, text, error, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry, artist name

2.不想看到的东西也可以加进去。例如不想看到裸露、兵器、血、猎奇的元素出现，就加入nsfw、weapon、blood、guro至负向提示词

> nsfw, weapon, blood, guro, lowres, bad anatomy, text, error, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts,signature, watermark, username, blurry, artist name

提示词并非越多越好，正向提示词以不超过150个词元(token)为原则；不过负向提示词可以尽量把不想看到的都塞进去。

## 三.Stable Diffusion WebUI独有语法

此节列出的语法为AUTOMATIC1111制作的SD WebUI独有，使用其他人制作的Stable Diffusion程序不见得适用。

## 关注度括号 Attention/emphasis

控制关注度的符号，增加算图时对该提示词的关注度(attention)。简单来说，括号就是你想强调的重点元素，括号越多，生成的结果越会符合括号里的提示词。

SD WebUI使用小括号控制关注度： ( ) 小括号层次越多权重越高(不加小括号为1倍，每加一层小括号乘以1.1倍)。例如，强调「蓝眼睛」，生成的结果就更高几率是蓝眼睛人物

> asian, woman, ((blue eyes)), beach, ocean

亦可以直接写明要增强几倍关注度，但其实一般情况下不用设太强，用一二层的小括号就够了。

> asian, woman, (blue eyes:2.5), beach, ocean

SD WebUI还支持[ ]中括号，越多权重越弱(乘以1.1倍)，通常用在负向提示词字段。

## 提示词编辑 Prompt editing

指定在到哪一步数的时候切换提示词。此处的中括号跟权重无关。

语法为[提示词1:提示词2:要切换的步数]

例如，算图时设置20步，一开始算橘子，设置算到15步时切换成苹果

```
[orange : apple : 15] on table
```

## 切换单字 Alternating Words

使用|代表在每个步数切换提示词，例如我要在算图时于「蓝眼睛」或「红眼睛」或「黑眼睛」之间切换

```
asian, woman, [blue eyes|red eyes|black eyes], beach, ocean
```

## 可组合性扩散 Composable Diffusion

此语法让AI依照权重生成二个不同的对象。

例如生成橘子与苹果，中间用大写的AND连接

```
orange AND apple
```

后面加上数字控制权重，初始值为1，数值低于0.1则无效。

```
orange :1.5 AND apple :2.5
```

此语法适合搭配ControlNet和Latent Couple，可以准确控制图中对象的位置。

## 四.提示词范例

这里提供一些懒人包。负向提示词有些是通用的。

生成可爱的狗勾

让我们试着画几只柯基犬在草地上奔跑。

建议使用模型：Stable Diffusion、Realistic Vision

提示词

```
3 corgi dogs running on grass field
```

负向提示词

```
lowres, bad anatomy, text, error, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts,signature, watermark, username, blurry, artist name
```

![](img/5554011db5f39a7778d2ebf23ef71d8c.png)

生成高科技未来城市

建议使用模型：Stable Diffusion、Realistic Vision

提示词

```
city future, 8k, exploration, cinematic, realistic, unreal engine, hyper detailed, volumetric light, moody cinematic epic concept art, realistic matte painting, hyper photorealistic
```

负向提示词

```
lowres, bad anatomy, text, error, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts,signature, watermark, username, blurry, artist name
```

![](img/09c232b125d7086acef46056fcb6da12.png)

生成一台特斯拉车子

建议使用模型：Stable Diffusion、Realistic Vision

提示词

```
concept art,tesla car, aerodynamic, future
```

负向提示词

```
lowres, bad anatomy, text, error, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts,signature, watermark, username, blurry, artist name
```

![](img/2188e8e72cb0203eadeb2fec9ba0df39.png)

生成动漫美少女

建议使用模型：Anything、Hentai Diffusion

随机画一个Fate/Grand Order的贞德

提示词

```
jeanne d&#x27;arc from fate grand order, 1girl, (best quality), (masterpiece), (high detail), ((full face)), sharp, ((looking at viewer)), ((detailed pupils)), (thick thighs), (((full body))), (large breasts)
```

负向提示词

```
lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts,signature, watermark, username, blurry, artist name
```

![](img/1314cef8dce108f58f2272890712de6c.png)