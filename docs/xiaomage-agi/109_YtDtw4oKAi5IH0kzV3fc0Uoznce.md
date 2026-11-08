# 实战操作： 62个咒语调教-时尚杂志封面

> 来源：[https://xmgtoagi.feishu.cn/docx/EwwsdoeO2olZyKxaU7bcTicbnPK](https://xmgtoagi.feishu.cn/docx/EwwsdoeO2olZyKxaU7bcTicbnPK)

**今天来给**大家分享，如何用sd简单的咒语输出好看的图片的教程，今天做的是时尚杂志专题，话不多说直入主题。

![](img/0df58eda238055245f15af67b13c2c7f.png)

本文生成的素材图片已上传网盘，需要的可自取：

20230715时尚杂志专题夸克网盘下载链接：https://pan.quark.cn/s/a29b917bb23d

用到的模型：「Stable Diffusion资料」链接：https://pan.quark.cn/s/2750beda9269

![](img/d8d3767377999871c97de352a76036c6.png)

# 一、基本设置：

**正向提示词：**

画质：Highest quality, ultra-high definition, masterpieces, 8k quality,

画面主题：1girl,young beautiful girl,White skin,long legs, delicate skin, big eyes, ultra detailed, official art, unity 8k wallpaper,

核心风格：(fashion magazine cover:1.3),

穿搭：见下面每一个咒语

**反向提示词：**

EasyNegative, (worst quality, low quality:1.4), nsfw, badhandv4,  ng_deepnegative_v1_75t,

(worst quality:2), (low quality:2), (normal quality:2), lowres, ((monochrome)), ((grayscale)), bad anatomy,DeepNegative, skin spots, acnes, skin blemishes,(fat:1.2),facing away, looking away,tilted head, lowres,bad anatomy,bad hands, missing fingers,extra digit, fewer digits,bad feet,poorly drawn hands,poorly drawn face,mutation,deformed,extra fingers,extra limbs,extra arms,extra legs,malformed limbs,fused fingers,too many fingers,long neck,cross-eyed,mutated hands,polar lowres,bad body,bad proportions,gross proportions,missing arms,missing legs,extra digit, extra arms, extra leg, extra foot,teethcroppe,signature, watermark, username,blurry,cropped,jpeg artifacts,text,error,

**设置：**

**模型 ：**

Chillout(真人模型), revAnimated，breakdro，anything-v5-PrtRE等。

VAE推荐使用：vae-ft-mse-840000-ema-pruned.safetensors，clip终止层数：2

迭代步数(steps):  25-32

采样方法(Sampler):  DPM++ 2M Karras,DPM++ SDE Karras 等等

宽高：512*768

总批次数：2

单批数量：1

提示词引导系数：7

随机数种子(seed)：-1

设置完成出图：

![](img/9b165ac0a4290515fcbe617fb6c0e2a2.png)

**关键词，采样器和模型测试**

![](img/64258791c852e34abece69d96d29af2b.png)

# 二、咒语及出图

## 风格搭配

### 1.都市摩登

**描述：****这套衣服****包括一个紧身的**黑色皮质迷你裙，一件简洁的白色紧身上衣，以及一双高跟鞋。配饰上，一条闪耀的金色链带提升了整体造型的豪华感。

关键词：黑色皮质迷你裙，白色紧身上衣，高跟鞋，金色链带

Prompt：black leather miniskirt, white fitted top, high heels, gold chain belt

![](img/1698e481ab72782f34b8d5fe58a1e7d4.png)

### 2.休闲海滩

**

描述****：这是一****套完美的海滩服**装，包括一件宽松的蓝白条纹连衣裙，一双棕色的草编凉鞋，和一顶阔边草帽。搭配上一个手工编织的海草包，你就准备好去海边度假了。

关键词：蓝白条纹连衣裙，棕色草编凉鞋，阔边草帽，手工编织的海草包

Prompt：blue and white striped dress, brown woven sandals, wide-brimmed straw hat, hand-woven seagrass bag

![](img/a439789d3836d7851f00f87b03f04483.png)

### 3.高级商务

**

描述****：这套服****装包括一件黑色**修身西装外套，一件白色衬衫，一条黑色直筒裤和一双黑色牛津鞋。配以红色丝质领带，为整体商务感增加了一丝活力。

关键词：黑色修身西装外套，白色衬衫，黑色直筒裤，黑色牛津鞋，红色丝质领带

Prompt：black tailored suit jacket, white shirt, black straight-leg trousers, black oxford shoes, red silk tie

![](img/799aae850fcbd7a289f36428217d22bd.png)

###  4.复古酷女

**

描述****：这套服****装包括一件红色**波点连衣裙，一双黑色皮质短靴，以及一个黑色伞形帽。复古的氛围和现代的酷感相结合，独特而引人注目。

关键词：红色波点连衣裙，黑色皮质短靴，黑色伞形帽

Prompt：red polka dot dress, black leather ankle boots, black umbrella hat

![](img/258f602529af75094f406c4a1a6b9677.png)

### 5.轻松休闲

**

描述****：这套服****装包括一件柔软**的灰色圆领毛衣，一条蓝色牛仔裤，以及一双白色运动鞋。搭配简单的黑色帆布包，是城市休闲风的完美体现。

关键词：灰色圆领毛衣，蓝色牛仔裤，白色运动鞋，黑色帆布包

Prompt：grey round-neck sweater, blue jeans, white sneakers, black canvas bag

![](img/1e77a0926de567961ebccf5c4d5a6c63.png)

### 6.华丽夜晚

**

描述****：这套服****装包括一件黑色**紧身短裙，一件亮片装饰的深V领上衣，以及一双黑色高跟鞋。加上一条闪亮的钻石项链，完美打造出夜晚的华丽造型。

关键词：黑色紧身短裙，亮片深V领上衣，黑色高跟鞋，钻石项链

Prompt：black tight mini skirt, sequin embellished deep V-neck top, black high heels, diamond necklace

![](img/2760e2526fe16f59cff451b8e26eaba4.png)

### 7.时髦运动

**描述：****这套服装****包括一件黑色运**动夹克，一条灰色运动裤，以及一双白色运动鞋。搭配鲜亮的橙色运动背包，完美诠释了活力四溢的运动风格。

关键词：黑色运动夹克，灰色运动裤，白色运动鞋，橙色运动背包

Prompt：black sports jacket, grey sweatpants, white sneakers, orange sports backpack

![](img/7a9608083bbaf3c49e1a4025f35a120e.png)

### 8.摩登女士

**

描述****：这套服****装包括一件黄色**紧身连衣裙，一双金色高跟鞋，以及一个淡紫色的小手提包。华丽的金色耳环和精致的手镯使整体造型更显典雅。

关键词：黄色紧身连衣裙，金色高跟鞋，淡紫色手提包，金色耳环，精致手镯

Prompt：yellow bodycon dress, gold high heels, lilac handbag, gold earrings, delicate bracelet

![](img/93ad644b6305a2e49fea98cd1298ae51.png)

### 9.浪漫波希米亚

**

描述****：这套服****装包括一件花卉**图案的长裙，一件白色松垮的衬衫，以及一双棕色的草编凉鞋。搭配手工编织的草帽和布满珠宝的长颈链，打造出充满艺术感的波希米亚风格。

关键词：花卉图案长裙，白色松垮衬衫，棕色草编凉鞋，手工编织草帽，珠宝长颈链

Prompt：floral patterned maxi skirt, loose white shirt, brown woven sandals, hand-woven straw hat, bejeweled long necklace

![](img/81ab9cc0537d92c6d09f74b75b9dd62a.png)

### 10.冰雪奇缘：

风格：冰雪奇缘

**描述**：这套服装包括一件白色的雪地靴，一件银色的羽绒服，以及一个蓝色的雪花图案围巾。配上一顶白色的毛线帽，给人一种冬日的温暖感觉。

**关键词**：白色雪地靴，银色羽绒服，蓝色雪花图案围巾，白色毛线帽

**Prompt**：white snow boots, silver down jacket, blue snowflake patterned scarf, white knitted hat

![](img/719c61b6f11ea69ec3c0775f98deaadf.png)

###

### 11.风格：典雅复古

描述：这套服装包括一件紫色的蕾丝连衣裙，一双银色的高跟鞋，以及一个黑色的小手提包。配上一颗珍珠项链，散发出复古的魅力。

关键词：深紫色细蕾丝连衣裙，银色闪亮高跟鞋，黑色皮质小手提包，复古风格珍珠项链

Prompt：deep purple delicate lace dress, shiny silver high heels, black leather mini handbag, vintage style pearl necklace

![](img/3855efbb102fbb19035ff6f2bd29dc98.png)

![](img/063b7d3014152d0ca570520619d86144.png)

### 12.都市炫酷：

风格：都市炫酷

**描述**：这套服装包括一条黑色的皮裤，一件红色的皮夹克，以及一双黑色的军靴。加上一条闪光的金色项链，展示出一种都市炫酷的风格。

**关键词**：黑色皮裤，红色皮夹克，黑色军靴，金色项链

**Prompt**：black leather pants, red leather jacket, black combat boots, gold necklace

![](img/7eef50a81cff5f3a6318fd80e87dddf5.png)

## 衣服穿搭Model1

### 1.风格：碎花裙

**描述**：这套服装以一条碎花裙为亮点，展现出浪漫和女性化的风格。搭配一件白色的简约上衣和一双粉色的高跟鞋，呈现出甜美而时尚的形象。

**关键词**：碎花裙，白色简约上衣，粉色高跟鞋

**Prompt**：floral dress, white minimalist top, pink high heels

(floral dress1.1), denim jacket, white sneakers

![](img/0caae697a9d507c633a54daa2066defe.png)

### 2.风格：短上衣

**描述**：这套服装以一件短款上衣为亮点，展现出时尚和年轻的风格。搭配一条高腰阔腿裤和一双凉鞋，呈现出时尚又舒适的形象。

**关键词**：短上衣，高腰阔腿裤，凉鞋

**Prompt**：crop top, high-waisted wide-leg pants, sandals

high-waisted jeans, cropped top, ankle boots

![](img/70a3f0128d106f3fa89cda94f3290ed0.png)

![](img/e4564050789f962310d1d560015e88a6.png)

### 3.风格：中长及膝裙

**描述**：这套服装以一条中长及膝的裙子为亮点，展现出优雅和女性化的风格。搭配一件精致的丝质上衣和一双尖头高跟鞋，呈现出时尚又典雅的形象。

**关键词**：中长及膝裙，丝质上衣，尖头高跟鞋

**Prompt**：midi skirt, silk blouse, pointed-toe heels

midi skirt, tucked-in blouse, ballet flats

![](img/d2f2b127105f64c9f47a8ce4a01eed22.png)

### 4.风格：露脐上衣

**描述**：这套服装以一件露脐上衣为亮点，展现出时尚和性感的风格。搭配一条高腰的牛仔短裤和一双时尚的运动鞋，呈现出休闲又时尚的形象。

**关键词**：露脐上衣，高腰牛仔短裤，运动鞋

**Prompt**：crop top, high-waisted denim shorts, sneakers

palazzo pants, fitted crop top, platform sandals

![](img/7b05f2ccabf7210c3cc863db3e7c19f2.png)

### 5.风格：及地长裙

**描述**：这套服装以一条及地长裙为亮点，展现出优雅和女性魅力的风格。搭配一件修身的丝质上衣和一双高跟凉鞋，呈现出典雅而时尚的形象。

**关键词**：及地长裙，修身丝质上衣，高跟凉鞋

**Prompt**：floor-length skirt, fitted silk top, high-heeled sandals

maxi dress, cardigan, gladiator sandals

![](img/e6998f9e2f43d9f2fba0ad19e7356a7e.png)

### 6.风格：皮打底裤

**描述**：这套服装以一条皮质打底裤为亮点，展现出时尚和酷感的风格。搭配一件黑色的紧身上衣和一双高跟靴子，呈现出时尚又性感的形象。

**关键词**：皮打底裤，黑色紧身上衣，高跟靴子

**Prompt**：leather leggings, black fitted top, high-heeled boots

leather leggings, oversized sweater, knee-high boots

![](img/675e755c7302506f4acccab8d55bbb9f.png)

### 7.风格：铅笔裙

**描述**：这套服装以一条铅笔裙为亮点，展现出优雅和职业的风格。搭配一件简约的白色衬衫和一双尖头高跟鞋，呈现出时尚又专业的形象。

**关键词**：铅笔裙，白色衬衫，尖头高跟鞋

**Prompt**：pencil skirt, white blouse, pointed-toe heels

(pencil skirt:1.1), button-up shirt, classic pumps

![](img/fe8f4004cf30ea044120729850b35cc9.png)

### 8.风格：高领毛衣

**描述**：这套服装以一件高领毛衣为亮点，展现出舒适和温暖的风格。搭配一条修身的黑色裤子和一双时尚的长靴，呈现出时尚又保暖的形象。

**关键词**：高领毛衣，黑色修身裤，长靴

**Prompt**：turtleneck sweater, black fitted pants, knee-high boots

short jeans, (turtleneck sweater:1.1), loafers

![](img/f5286aa61b0987eca20a99613452ad04.png)

### 9.风格：A字裙

**描述**：这套服装以一条A字裙为亮点，展现出优雅和女性魅力的风格。搭配一件贴身的纯色上衣和一双优雅的高跟鞋，呈现出典雅而时尚的形象。

**关键词**：A字裙，纯色上衣，高跟鞋

**Prompt**：A-line skirt, solid color top, high heels

(A-line dress:1.1), blazer, heeled sandals

![](img/e91b9594bbfb92d90bdd4d0e284283f0.png)

### 10. 风格：无袖上衣

**描述**：这套服装以一件无袖上衣为亮点，展现出清爽和夏日的风格。搭配一条高腰的牛仔短裤和一双凉鞋，呈现出休闲又时尚的形象。

**关键词**：无袖上衣，高腰牛仔短裤，凉鞋

**Prompt**：sleeveless top, high-waisted denim shorts, sandals

culottes, sleeveless blouse, wedges,suspenders

![](img/b07f11f7a3bcedf8074d2e33e8f138ad.png)

![](img/4a56643720642de6b7c2b2d6a4d9eb49.png)

### 11.丝质背心

**描述**：这套服装的重点是一件粉色的丝质背心，采用轻盈的材质，展现出简约而优雅的设计。搭配一条白色的高腰短裤和一双金色的凉鞋，透露出舒适且时尚的夏日风格。

**关键词**：粉色丝质背心，白色高腰短裤，金色凉鞋

**Prompt**：pink silk camisole, white high-waisted shorts, gold sandals

tailored trousers, silk camisole, pointed-toe flats

![](img/88fd18b7a3b234ffb03f57bca0c727a9.png)

### 12.风格：迷你裙

**描述**：这套服装的焦点在于一条红色的迷你裙，展示出青春活力的时尚趋势。配以一件黑色的紧身短袖上衣和一双黑色的皮质短靴，营造出摩登而时髦的城市风格。

**关键词**：红色迷你裙，黑色紧身短袖上衣，黑色皮质短靴

**Prompt**：red mini skirt, black tight short-sleeved top, black leather ankle boots

**Prompt**：mini skirt, graphic tee, combat boots

![](img/e9f73bebb61d7e2d59edd09d161938b6.png)

![](img/cd5e0b7ea65180d203a4b091fd0f79ab.png)

### 13.风格：衬衫裙

**描述**：这套服装以一件淡蓝色条纹衬衫裙为亮点，展示出随性又优雅的风格。配上一双白色帆布鞋和一顶草编帽，营造出休闲的夏日海滩风格。

**关键词**：淡蓝色条纹衬衫裙，白色帆布鞋，草编帽

**Prompt**：light blue striped shirt dress, white canvas shoes, straw hat

**Prompt**：shirt dress, statement belt, ankle-strap heels

![](img/cf96c9f622004498637a1b25879a1eab.png)

### 14.风格：露肩上衣

**描述**：这套服装特点是一件白色的露肩上衣，设计优雅且女性化。配以一条牛仔蓝色的喇叭裤和一双棕色的楔形凉鞋，塑造出一种休闲而时尚的风格。

**关键词**：白色露肩上衣，牛仔蓝喇叭裤，棕色楔形凉鞋

**Prompt**：white off-shoulder top, denim blue flare jeans, brown wedge sandals

**Prompt**：distressed jeans, off-the-shoulder top, espadrilles

![](img/76958fc70a7d0f7bba7734bde9522818.png)

![](img/38d2d1c37ced0f29fdd6e92b56fbd8ce.png)

### 15.风格：牛仔背心

**描述**：这套服装的特色在于一件蓝色牛仔背心，它传递出一种酷炫且随性的氛围。配以一条黑色皮裤和一双白色高帮运动鞋，营造出一种摩登且具有街头风格的形象。

**关键词**：蓝色牛仔背心，黑色皮裤，白色高帮运动鞋

**Prompt**：blue denim vest, black leather pants, white high-top sneakers

**Prompt:  **romper, denim vest, platform sneakers

![](img/da0f5916f0270299a81f46c85b0f0d34.png)

### 16.风格：紧身衣

**描述**：这套服装的亮点是一件黑色的紧身衣，它体现了流线型的设计和修身的剪裁。配以一条白色的九分裤和一双红色的高跟鞋，展现出一种简洁但醒目的都市风格。

**关键词**：黑色紧身衣，白色九分裤，红色高跟鞋

**Prompt**：tights,bodysuit, white cropped pants, red high heels

**Prompt**：bodycon dress, sheer tights, stiletto heels

![](img/227fba7b9e015866d409b7c0c7a7f2c3.png)

### 17.风格：吊带衫

**描述**：这套服装以一件绿色的吊带衫为主角，带来一种清新的夏日感觉。搭配一条浅色牛仔短裤和一双棕色的凉鞋，呈现出轻松而时尚的海边度假风格。

**关键词**：绿色吊带衫，浅色牛仔短裤，棕色凉鞋

**Prompt**：green tank top, light denim shorts, brown sandals

**Prompt**：high-low skirt, halter top, block heels

![](img/40de56eb8e6a9c1f3c4afc312e502aed.png)

### 18.风格：蕾丝上衣

**描述**：这套服装以一件白色的蕾丝上衣为焦点，体现了精致的工艺和女性化的优雅。配以一条黑色的直筒裤和一双银色的平底鞋，展现出简洁而高雅的都市风格。

**关键词**：白色蕾丝上衣，黑色直筒裤，银色平底鞋

**Prompt**：white lace top, black straight-leg trousers, silver flats

**Prompt**：velvet pants, lace blouse, kitten heels

![](img/a057865745caeec3ab261904e6bd5827.png)

### 19.风格：裹身裙

**描述**：这套服装以一条红色花卉图案的裹身裙为亮点，营造出活力四溢和女性化的风格。配以一件白色简洁的上衣和一双棕色的高跟凉鞋，展现出轻松而时尚的夏日风格。

**关键词**：红色花卉图案裹身裙，白色简洁上衣，棕色高跟凉鞋

**Prompt**：red floral wrap skirt, simple white top, brown high-heeled sandals

**Prompt**：wrap dress, statement necklace, strappy sandals

![](img/ad4540d55600ce0a044f0d8e6ae1a6b2.png)

### 20.风格：连身裤

**描述**：这套服装以一件海军蓝色的连身裤为亮点，设计简约且流线。配上一双白色的运动鞋和一个简约的金色项链，展现出都市休闲的风格。

**关键词**：海军蓝色连身裤，白色运动鞋，简约金色项链

**Prompt**：navy blue jumpsuit, white sneakers, simple gold necklace

**Prompt**：jumpsuit, cropped blazer, mules

![](img/632e4ecab3009fd04862b4ab723701cd.png)

### 21.风格：百褶裙

**描述**：这套服装的焦点是一条粉色的百褶裙，这款裙子透出一种女性化和浪漫的气息。搭配一件白色的简单T恤和一双银色的凉鞋，展示出轻松而甜美的风格。

**关键词**：粉色百褶裙，白色简单T恤，银色凉鞋

**Prompt**：pink pleated skirt, simple white T-shirt, silver sandals

**Prompt**：pleated skirt, cashmere sweater, over-the-knee boots

![](img/0b4adfda87fcc71f340d3e92db579066.png)

## 衣服穿搭Model2

**模型：dosmix**

模型测试

![](img/b00451e4af471bf541fdfdb8923b7377.png)

最终模型选择：DPM++ 2S a  /  DPM++  2M

提示词相关性(CFG Scale)：9

![](img/e99b7e535385bbcf00791dc49f266c36.png)

### 22.风格：粗花呢夹克

**描述**：这套服装的焦点是一件棕色的粗花呢夹克，设计简洁但质感丰富，传递出冬季的温暖和复古风情。搭配一条黑色高腰直筒裤和一双棕色皮靴，展示出都市休闲且时尚的风格。

**关键词**：棕色粗花呢夹克，黑色高腰直筒裤，棕色皮靴

**Prompt**：brown tweed jacket, black high-waisted straight-leg pants, brown leather boots

**Prompt**：tweed jacket, tailored shorts, brogues

![](img/b193f81d4f86ab7bd387e99a484de013.png)

### 22.风格：锥形裤

**描述**：这套服装的重点是一条海军蓝色的锥形裤，设计简约且合体，展现了都市时尚的风格。搭配一件白色宽松衬衫和一双棕色的皮质乐福鞋，形成了一种随性而精致的休闲装束。

**关键词**：海军蓝色锥形裤，白色宽松衬衫，棕色皮质乐福鞋

**Prompt**：navy blue tapered trousers, white loose shirt, brown leather loafers

**Prompt**：tapered pants, oversized blouse, slingback heels

![](img/52198ea18a3a6bc69bacd90003058a25.png)

### 24.风格：连衣裙

**描述**：这套服装以一件薄荷绿色的连衣裙为主角，细腻的印花和轻盈的面料营造出清新的春日感觉。配以一双白色的平底鞋和一顶草编的太阳帽，展现出轻松而优雅的休闲风格。

**关键词**：薄荷绿色连衣裙，白色平底鞋，草编太阳帽

**Prompt**：mint green dress, white flats, straw sun hat

### 24.风格：连衣裙

pinafore dress, turtleneck, ankle boots

![](img/ab893157d9b068d9e9c9ceb8c73ce586.png)

![](img/afcc847e9aebe42dd961a2fe9ac48e4d.png)

### 25.风格：荷叶边上衣

**描述**：这套服装以一件白色的荷叶边上衣为焦点，充满了浪漫和女性化的气息。搭配一条淡蓝色牛仔裤和一双棕色的皮质凉鞋，展现出轻松而时尚的休闲风格。

**关键词**：白色荷叶边上衣，淡蓝色牛仔裤，棕色皮质凉鞋

**Prompt**：white ruffle top, light blue jeans, brown leather sandals

**Prompt**：cigarette pants, peplum top, oxford shoes

![](img/136da1ab7509607c49085cc8e2ee7671.png)

![](img/2f1499dc58f1cb803f87d1814640333c.png)

### 26.风格：波西米亚衬衫

**描述**：这套服装的特色是一件带有复古印花的波西米亚衬衫，它散发出一种自由而艺术的气息。配以一条棕色的麂皮短裤和一双粗跟的皮质凉鞋，展现出舒适而风情万种的波西米亚风格。

**关键词**：复古印花波西米亚衬衫，棕色麂皮短裤，粗跟皮质凉鞋

**Prompt**：vintage print bohemian shirt, brown suede shorts, chunky heel leather sandals

**Prompt**：flared jeans, boho blouse, wedge sandals

![](img/f4306b34ad68fc22fff065d19f183f4f.png)

### 27.风格：吊带裙

**描述**：这套服装以一条黑色的吊带裙为亮点，设计简约且性感。搭配一件白色的无袖上衣和一双金色高跟鞋，展现出时尚而优雅的夜晚风格。

**关键词**：黑色吊带裙，白色无袖上衣，金色高跟鞋

**Prompt**：black strappy dress, white sleeveless top, gold high heels

**Prompt**：slip dress, light cardigan, minimalist sneakers

![](img/3ec05ef8f9c4213fc31eb62d0665270b.png)

###

### 28.风格：雪纺及地裙

**描述**：这套服装的特点是一条垂坠的雪纺及地裙，轻盈的面料给人带来优雅而飘逸的感觉。搭配一件粉色的丝质上衣和一双白色细高跟鞋，营造出浪漫而女性化的风格。

**关键词**：雪纺及地裙，粉色丝质上衣，白色细高跟鞋

**Prompt**：chiffon maxi skirt, pink silk top, white stiletto heels

**Prompt**：chiffon maxi skirt, crop top, gladiator sandals

![](img/ccb076edfee62e69357a62a8b39b7154.png)

### 29.风格：灯芯绒工装裤

**描述**：这套服装以一条灯芯绒工装裤为焦点，展现出复古和休闲的风格。搭配一件白色的宽松卫衣和一双黑色的马丁靴，营造出时尚而有型的街头风格。

**关键词**：灯芯绒工装裤，白色宽松卫衣，黑色马丁靴

**Prompt**：corduroy cargo pants, white oversized sweatshirt, black combat boots

**Prompt**：corduroy overalls, striped tee, canvas sneakers

![](img/b4d1c0f570b5744e14743b4aa1c722df.png)

### 30.风格：褶边衬衫

**描述**：这套服装以一件白色的褶边衬衫为亮点，给人带来柔和而优雅的感觉。搭配一条黑色的铅笔裙和一双黑色高跟鞋，展现出职业且时尚的形象。

**关键词**：白色褶边衬衫，黑色铅笔裙，黑色高跟鞋

**Prompt**：white ruffled blouse, black pencil skirt, black high heels

**Prompt**：ruffled blouse, high-waisted shorts, espadrille wedges

![](img/76be94c3389d0bd2d13d0592205df0d5.png)

### 31.束带衬衫裙

### 31.风格：束带衬衫裙

**描述**：这套服装以一条束带衬衫裙为焦点，展现出优雅和女性化的氛围。搭配一双中性色的皮革凉鞋和一顶宽边帽子，营造出时尚而自在的春夏风格。

**关键词**：束带衬衫裙，中性色皮革凉鞋，宽边帽子

**Prompt**：belted shirt dress, neutral leather sandals, wide-brim hat

belted shirtdress, patterned tights, ankle boots

![](img/7270ab5fde34e1a963354c586aa0bb58.png)

![](img/1dd5dfa618e3ada24a354e772f79ee09.png)

### 32.亮片迷你裙

### 32.风格：亮片迷你裙

**描述**：这套服装以一条闪亮的亮片迷你裙为亮点，营造出充满派对氛围的时尚风格。搭配一件黑色的贴身上衣和一双高跟鞋，展现出时尚而性感的形象。

**关键词**：亮片迷你裙，黑色贴身上衣，高跟鞋

**Prompt**：sequin mini skirt, black fitted top, high heels,

**Prompt**：sequin mini skirt, plain tee, biker jacket, high-top sneakers,

![](img/2fe8ab9592cc345c582775b3a73afddb.png)

### 33.风格：复古毛衣

**描述**：这套服装以一件粉色的复古毛衣为亮点，展现出复古风情和温暖的感觉。搭配一条高腰牛仔裤和一双白色运动鞋，呈现出休闲舒适的复古风格。

**关键词**：粉色复古毛衣，高腰牛仔裤，白色运动鞋

**Prompt**：vintage pink sweater, high-waisted jeans, white sneakers

**Prompt**：mom jeans, vintage sweater, loafers

![](img/2353b5bc658735efda3414c4d084b4ba.png)

### 34.风格：针织裙

**描述**：这套服装以一条灰色针织裙为焦点，展现出温暖和舒适的感觉。搭配一件黑色紧身上衣和一双黑色长靴，呈现出简约而时尚的秋冬风格。

**关键词**：灰色针织裙，黑色紧身上衣，黑色长靴

**Prompt**：knit dress in grey, black fitted top, black boots

**Prompt**：knit dress, long coat, heeled booties

![](img/0a8ac0c9415347a53ffcfa51d3aac2e3.png)

### 35.风格：纸袋腰裤

**描述**：这套服装以一条纸袋腰裤为亮点，展现出时尚和休闲的风格。搭配一件简约的白色T恤和一双白色运动鞋，呈现出轻松又时尚的形象。

**关键词**：纸袋腰裤，白色简约T恤，白色运动鞋

**Prompt**：paperbag waist pants, white minimalist T-shirt, white sneakers

**Prompt**：paperbag waist pants, fitted turtleneck, pumps

![](img/6ed0d276439da4e183c86710248bb280.png)

### 36.风格：娃娃裙

**描述**：这套服装以一条粉色娃娃裙为亮点，呈现出可爱和俏皮的风格。搭配一件白色蕾丝上衣和一双粉色平底鞋，展现出甜美而少女心的形象。

**关键词**：粉色娃娃裙，白色蕾丝上衣，粉色平底鞋

**Prompt**：pink babydoll dress, white lace top, pink flats

**Prompt**：babydoll dress, knee-high socks, Mary Jane shoes

![](img/5df307c5456b818c73f2b0b719c79578.png)

### 37.风格：后宫裤

**描述**：这套服装以一条宽松的后宫裤为亮点，展现出东方风情和优雅的氛围。搭配一件贴身的黑色上衣和一双细跟高跟鞋，呈现出独特而典雅的形象。

**关键词**：后宫裤，黑色贴身上衣，细跟高跟鞋

**Prompt**：harem pants, black fitted top, stiletto heels

harem pants, tank top, flat sandals

![](img/a885b8b6eea14a23ce3c3800d9be0e54.png)

### 38.风格：花呢迷你裙

**描述**：这套服装以一条花呢迷你裙为亮点，展现出时尚和青春活力的氛围。搭配一件白色的简约上衣和一双黑色过膝靴，呈现出时尚而俏皮的形象。

**关键词**：花呢迷你裙，白色简约上衣，黑色过膝靴

**Prompt**：plaid mini skirt, white minimalist top, black over-the-knee boots

**Prompt**：tweed mini skirt, tights, blouse, ankle boots

![](img/93c510a8b8f5097a65e1ebf1487df2b8.png)

### 39.风格：长袖衬衫

**描述**：这套服装以一件蓝色长袖衬衫为亮点，展现出经典和正式的风格。搭配一条黑色的铅笔裙和一双黑色高跟鞋，呈现出优雅而职业的形象。

**关键词**：蓝色长袖衬衫，黑色铅笔裙，黑色高跟鞋

**Prompt**：blue long-sleeve shirt, black pencil skirt, black high heels

puffer vest, long-sleeve shirt, leggings, riding boots

![](img/88914882401e6b909b26df538876ec98.png)

### 40.风格：羽绒背心

**描述**：这套服装的特点是一件羽绒背心，给人带来时尚和保暖的感觉。搭配一件黑色的长袖针织衫和一条深色牛仔裤，展现出休闲而时尚的秋冬风格。

**关键词**：羽绒背心，黑色长袖针织衫，深色牛仔裤

**Prompt**：down jacket, down vest, dark denim jeans

**Prompt**：plaid pants, white button-down, loafers

![](img/05315aa106429d0e68020731c2d7efdc.png)

### 41.风格：牛仔束腰夹克

**描述**：这套服装以一件牛仔束腰夹克为亮点，呈现出时尚和休闲的风格。搭配一条黑色的紧身裤和一双白色运动鞋，展现出时尚又舒适的形象。

**关键词**：牛仔束腰夹克，黑色紧身裤，白色运动鞋

**Prompt**：(denim waist-tied jacket:1.3), black skinny jeans, white sneakers

fit and flare dress, cropped denim jacket, ballet flats

![](img/36e78139b59f5c317b4f6fa71642e0c7.png)

### 42.风格：运动长裤

**描述**：这套服装以一条黑色的运动长裤为亮点，展现出活力和运动的氛围。搭配一件灰色的舒适运动上衣和一双白色运动鞋，呈现出休闲而时尚的运动风格。

**关键词**：黑色运动长裤，灰色运动上衣，白色运动鞋

**Prompt**：(black sporty pants:1.2), gray athletic top, white sneakers

**Prompt**：joggers, crop top, bomber jacket, chunky sneakers

![](img/1d6ee6fc76d7fb65f2083278993b6cce.png)

### 43.风格：不对称裙

**描述**：这套服装以一条不对称设计的裙子为亮点，展现出独特和时尚的风格。搭配一件简约的黑色上衣和一双高跟鞋，呈现出前卫而引人注目的形象。

**关键词**：不对称裙，黑色简约上衣，高跟鞋

**Prompt**：asymmetrical skirt, black minimalist top, high heels

**Prompt**：asymmetrical skirt, camisole top, strappy heels

![](img/6fdb2fa31d99455d168e01c8d4e2d0a9.png)

### 44.风格：喇叭裤

**描述**：这套服装以一条喇叭裤为亮点，呈现出复古和时尚的风格。搭配一件花朵印花的宽松上衣和一双高跟凉鞋，展现出自信而时尚的形象。

**关键词**：喇叭裤，花朵印花上衣，高跟凉鞋

**Prompt**：flared pants, floral print top, high-heeled sandals

bell-bottom jeans, off-the-shoulder blouse, platform heels

![](img/4de4a1e9185c3060cda3d75a13088309.png)

### 45.风格：工装裤

**描述**：这套服装以一条工装裤为亮点，呈现出时尚和实用的风格。搭配一件简约的白色T恤和一双运动鞋，展现出休闲又帅气的形象。

**关键词**：工装裤，白色简约T恤，运动鞋

**Prompt**：utility pants, white minimalist T-shirt, sneakers

utility jumpsuit, waist belt, ankle boots

![](img/240249383b49c259eb33078bec05ef1b.png)

### 46.风格：毛皮大衣/皮草

**描述**：这套服装以一件华丽的毛皮大衣或皮草为亮点，展现出奢华和高贵的风格。搭配一条修身的黑色裙子和一双高跟靴子，呈现出典雅而时尚的形象。

**关键词**：毛皮大衣，皮草，黑色修身裙，高跟靴子

**Prompt**：fur coat, fur, black fitted dress, high-heeled boots

faux fur coat, skinny jeans, heeled booties

![](img/6cd092b11e6c52ac7edfef40fc922aab.png)

### 47.风格：薄纱裙

**描述**：这套服装以一条薄纱裙为亮点，展现出轻盈和浪漫的风格。搭配一件柔和色调的上衣和一双细跟高跟鞋，呈现出优雅而女性化的形象。

**关键词**：薄纱裙，柔和色调上衣，细跟高跟鞋

**Prompt**：sheer chiffon skirt, soft-toned top, stiletto heels

tulle skirt, graphic tee, moto jacket, combat boots

![](img/7bb7fc2738b8b6db53c8239f4eb58339.png)

### 48.风格：高腰打底裤

**描述**：这套服装以一条高腰的打底裤为亮点，展现出修身和时尚的风格。搭配一件贴身的黑色上衣和一双时尚的运动鞋，呈现出休闲又时尚的形象。

**关键词**：高腰打底裤，黑色贴身上衣，时尚运动鞋

**Prompt**：(high-waisted leggings:1.3), black fitted top, trendy sneakers

**Prompt**：high-waisted leggings, sports bra, oversized hoodie, running shoes

![](img/81594fc89c1c54d6a9fe07035313458d.png)

### 49.风格：罩衫

**描述**：这套服装以一件罩衫为亮点，呈现出舒适和随性的风格。搭配一条牛仔短裤和一双白色帆布鞋，展现出休闲又时尚的形象。

**关键词**：罩衫，牛仔短裤，白色帆布鞋

**Prompt**：(cardigan:1.3), denim shorts, white canvas shoes

**Prompt**：smock dress, patterned tights, heeled oxfords

![](img/6eac2c1df4d53a4a0765fc0bd47e750b.png)

### 50.风格：宽边帽

**描述**：这套服装以一顶宽边帽为亮点，展现出时尚和阳光的风格。搭配一件轻盈的连衣裙和一双舒适的平底凉鞋，呈现出轻松而时尚的形象。

**关键词**：宽边帽，连衣裙，平底凉鞋

**Prompt**：(wide-brim hat:1.3), sundress, flat sandals,

wide-brim hat, bohemian maxi dress, layered necklaces, flat sandals,

![](img/c7f6fb8205c946e49121113f759ccc0b.png)

62种搭配封面，大家可以继续挖掘，自成一派统一风格，火爆📕不成问题，后续换种风格继续分享，敬请期待......