
# 实操教程：原神人物公仔实操

> 来源：[https://xmgtoagi.feishu.cn/docx/V6FUd1QBAoYVUUxu9tRck5qwntd](https://xmgtoagi.feishu.cn/docx/V6FUd1QBAoYVUUxu9tRck5qwntd)

作者：孟祥林

在这篇文章中，我们将深入探讨原神盲盒的艺术风格，以及如何运用AI绘画技术（Stable Diffusion）——来创造原神角色公仔。我们将通过实践操作让读者更好地理解这种技术，以及如何利用关键标签和Lora模型来优化生成的结果。

原神作为全球颇受欢迎的游戏，它的角色设计、环境风格和故事背景都吸引了大量玩家。

特别是角色公仔，凭借其独特的艺术风格和精致的设计，成为了玩家们热衷收藏的对象。

然而，人工设计和生产公仔的成本较高，产量也有限。有没有一种方法可以在保持艺术风格的同时，高效且多样地生产原神角色公仔呢？

AI绘画技术（Stable Diffusion）或许就是解决这个问题的关键。稳定扩散是一种强大的生成模型，它能够学习并模仿任何给定样本的风格和特征。在本文中，我们将介绍如何应用稳定扩散在原神的艺术风格上，以生成高质量的原神角色公仔图像。

同时，我们也会深入讲解如何利用核心标签和Lora模型来优化生成的公仔图像。核心标签可以帮助模型更好地理解和学习原神的艺术风格，而Lora模型则能够让生成的图像更加生动和逼真。

## 下载地址

本文章所用到的资源已打包整理到网盘

**AI绘画原神资料包**

![](img/d37396a1d1ae1347c8675b354da28bdc.png)

方式一：👸号免费获取：AI飞行家，回复：genshin

![](img/970cee64b288b7e4560e8f1cf732dc3b.png)

## 基础教程

[AI绘画：Stable Diffusion 终极宝典：从入门到精通](https://y3if3fk7ce.feishu.cn/docx/KqEMdhJigoFY8fxc9TPcwMninKf)

## 大模型

`revAnimated_v11` 和`dosmix`尝试

## VAE

`animevae`

## 采样方式（Sampler）

`DPM++ SDE Karras`/`DPM2 Karras`

## steps（采样迭代步数）

20-30

`CFG Scale`（提示词相关性）：7

## 安柏(anmber)

![](img/cf171f69337e46011a841e8ec0d824e5.png)

**Lora：**`amberGenshinImpact_flexibleV1`

### 正面关键词

(masterpiece, best quality, absurdres, foreshortening), photo of a highly detailed,(full body:1.2), smile,cute face ,,(studio background),(clean background),(blurry background), 1girl, chibi, (beautiful detailed face), (beautiful detailed eyes),standing in chinese garden, rendered in blender , goggles on head,

amber \(genshin impact\), leather gloves, pouch, thighhighs, white boots, medium breasts, cleavage cutout, shrug \(clothing\) full body,

brown eyes, brown hair, long hair, red hair ribbon, brown short shorts,

`<lora:amberGenshinImpact_flexibleV1:0.7> <lora:blindbox_V1Mix:0.6>`

### 负面关键词

bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry,missing fingers,bad hands,missing arms, long neck, Humpbacked, deformed, bad anatomy, disfigured, poorly drawn face, mutation, mutated, extra limb, ugly, poorly drawn hands, missing limb, floating limbs, disconnected limbs, malformed hands, out of focus, long neck, long body, monochrome, watermark, signature, logo, name,

(low quality:1.3), (worst quality:1.3),pointy ears, animal ears, watermark, logo, text, `badhandv4`,

### 出图效果

![](img/7903d640f1a2bc6eb7828d5185dc221b.png)

## 多莉（duoli）

![](img/0273d14bf1d5b14c78c5cb688c83889c.png)

**lora：**`duoliGenshinImpact_visionGenshinImpact`

**权重：**无

**Trigger Words：**1girl，blue pants, blue hat，

**c站地址：**`https://civitai.com/models/5937/duoligenshin-impact`

**核心关键词正面：**hat, barefoot, pants, feet, shoes, holding, sunglasses, toes, shirt, sitting, bangs, ring, jewelry, pince-nez, bag, sidelocks, midriff, navel, sleeveless, blush, beret, purple headwear, pink hair, tinted eyewear, crop top, looking at viewer, long sleeves, harem pants, detached sleeves, puffy long sleeves, shoes removed, sleeveless shirt, red-tinted eyewear, black shirt, purple pants, bare shoulders, vision (genshin impact), hair between eyes, full body, hair ornament, yellow eyes, gold trim, multiple rings

**负：**lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry

### 正面关键词

(masterpiece, best quality, absurdres, foreshortening), photo of a highly detailed,(full body:1.2), smile,cute face ,,(studio background),(clean background),(blurry background), 1girl, chibi, (beautiful detailed face), (beautiful detailed eyes),standing in chinese garden, rendered in blender , goggles on head,

hat, barefoot, pants, feet, shoes, holding, sunglasses, toes, shirt, sitting, bangs, ring, jewelry, pince-nez, bag, sidelocks, midriff, navel, sleeveless, blush, beret, purple headwear, pink hair, tinted eyewear, crop top, looking at viewer, long sleeves, harem pants, detached sleeves, puffy long sleeves, shoes removed, sleeveless shirt, red-tinted eyewear, black shirt, purple pants, bare shoulders, vision (genshin impact), hair between eyes, full body, hair ornament, yellow eyes, gold trim, multiple rings

`<lora:blindbox_V1Mix:0.6> <lora:duoliGenshinImpact_visionGenshinImpact:0.7>`

### 负面关键词

lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry

### 出图效果

![](img/0a8e7d97552dc71d6c2351c4cd2fdbdf.png)

## 优菈（Eula）

![](img/3f565feab324886d1a2b21339af1fa73.png)

**lora：**`eulaGenshinImpact_eulav4000082`

**权重：** 0.8-1.0

**Trigger Words：**`eula`

**C站地址：**`https://civitai.com/models/10441/eula-genshin-impact`

**正：**(black thigh boots), close-up, (`short_blue_hair`), (`eula`),blue eyes, Asymmetrical hairstyle,

**负：**lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, (worst quality:2),( low quality:2), (normal quality:2), jpeg artifacts, signature, watermark, username, blurry, lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry,(wrong feet,wrong hand, wrong leg :1.2),`ugly_face`,paintings,(Cross-eyed),(bodysuit:1.1), Symmetrical hairstyle

### 正面关键词

(masterpiece, best quality, absurdres, foreshortening), photo of a highly detailed,(full body:1.2), smile,cute face ,,(studio background),(clean background),(blurry background), best quality, ultra high res, (photorealistic:1.4), `long_hair`, realistic, solo, `long_hair`, ultra high res, teeth, Long sleeve, bright blue dress, small mouth, full body, 3girls, Grin, ok sign, smile, stand, 1girl, beautiful, masterpiece, extremely detailed face, perfect lighting, solo, parted lips, lipstick, ultra-detailed, Peach buttock, looking at the viewer,

`<lora:chilloutmixss_xss10:0.3> <lora:koreanDollLikeness_v15:0.1>`

`<lora:blindbox_V1Mix:0.6> <lora:eulaGenshinImpact_eulav4000082:1>`

### 负面关键词

`easynegative`, fat, naked, nude, paintings, sketches, (worst quality:2), (low quality:2), (normal quality:2), lowres, ((monochrome)), ((grayscale)), ugly, tiling, poorly drawn hands, poorly drawn feet, poorly drawn face, out of frame, extra limbs, missing limb, floating limbs, disconnected limbs, (mutated hands and fingers:1.4), disfigured, deformed, body out of frame, bad anatomy, watermark, signature, cut off, low contrast, underexposed, overexposed, bad art, beginner, amateur, distorted face, disgusting, blurry, draft, grainy, amputation, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry, bad feet, cropped, poorly drawn hands, poorly drawn face, mutation, deformed, extra fingers, extra limbs, extra arms, extra legs, malformed limbs, fused fingers, too many fingers, long neck, cross-eyed, mutated hands, polar lowres, bad body, bad proportions, gross proportions, skin spots, acnes, skin blemishes, `DeepNegative`,(fat:1.2),facing away, looking away,tilted head, lowres,bad hands, worstquality,jpegartifacts, username,blurry,bad feet,bad body, extra foot,

## 菲谢尔（fischl）

![](img/eab649849b67bf3f1f65029d65509900.png)

**lora：**`fischlGenshinImpact_fischlGenshinImpact`

**权重：**0.7-1.0

**Trigger Words:** blonde eyes, blonde hair，

**C站地址：**`https://civitai.com/models/6257/fischl-genshin-impact`

**正：**eyepatch, bow, gloves, breasts, bowtie, bangs, ribbon, fishnets, bodystocking, green eyes, long hair, blonde hair, hair ribbon, purple bow, bare shoulders, black gloves, purple bowtie, detached sleeves, hair over one eye, elbow gloves



# LoRA 提示词配置文档

## 初始配置

### 负面提示词

`{Multiple people}`, `lowres`, `bad anatomy`, `bad hands`, `text`, `error`, `missing fingers`, `extra digit`, `fewer digits`, `cropped`, `worstquality`, `low quality`, `normal quality`, `jpegartifacts`, `signature`, `watermark`, `username`, `blurry`, `bad feet`, `cropped`, `poorly drawn hands`, `poorly drawn face`, `mutation`, `deformed`, `worst quality`, `low quality`, `normal quality`, `jpeg artifacts`, `signature`, `watermark`, `extra fingers`, `fewer digits`, `extra limbs`, `extra arms`, `extra legs`, `malformed limbs`, `fused fingers`, `too many fingers`, `long neck`, `cross-eyed`, `mutated hands`, `polar lowres`, `bad body`, `bad proportions`, `gross proportions`, `text`, `error`, `missing fingers`, `missing arms`, `missing legs`, `extra digit`,

### 正面关键词

`(masterpiece, best quality, absurdres, foreshortening)`, `photo of a highly detailed`, `(full body:1.2)`, `smile`, `cute face`, `(studio background)`, `(clean background)`, `(blurry background)`, `1girl`, `chibi`, `(beautiful detailed face)`, `(beautiful detailed eyes)`, `standing in chinese garden`, `rendered in blender`, `goggles on head`,

`eyepatch`, `bow`, `gloves`, `breasts`, `bowtie`, `bangs`, `ribbon`, `fishnets`, `bodystocking`, `green eyes`, `long hair`, `blonde hair`, `hair ribbon`, `purple bow`, `bare shoulders`, `black gloves`, `purple bowtie`, `detached sleeves`, `hair over one eye`, `elbow gloves`

`<lora:blindbox_V1Mix:0.6>` `<lora:fischlGenshinImpact_fischlGenshinImpact:0.7>`

### 负面关键词

`easynegative`, `fat`, `naked`, `nude`, `paintings`, `sketches`, `(worst quality:2)`, `(low quality:2)`, `(normal quality:2)`, `lowres`, `((monochrome))`, `((grayscale))`, `ugly`, `tiling`, `poorly drawn hands`, `poorly drawn feet`, `poorly drawn face`, `out of frame`, `extra limbs`, `missing limb`, `floating limbs`, `disconnected limbs`, `(mutated hands and fingers:1.4)`, `disfigured`, `deformed`, `body out of frame`, `bad anatomy`, `watermark`, `signature`, `cut off`, `low contrast`, `underexposed`, `overexposed`, `bad art`, `beginner`, `amateur`, `distorted face`, `disgusting`, `blurry`, `draft`, `grainy`, `amputation`, `bad hands`, `text`, `error`, `missing fingers`, `extra digit`, `fewer digits`, `cropped`, `worst quality`, `low quality`, `normal quality`, `jpeg artifacts`, `signature`, `watermark`, `username`, `blurry`, `bad feet`, `cropped`, `poorly drawn hands`, `poorly drawn face`, `mutation`, `deformed`, `extra fingers`, `extra limbs`, `extra arms`, `extra legs`, `malformed limbs`, `fused fingers`, `too many fingers`, `long neck`, `cross-eyed`, `mutated hands`, `polar lowres`, `bad body`, `bad proportions`, `gross proportions`, `skin spots`, `acnes`, `skin blemishes`, `DeepNegative`, `(fat:1.2)`, `facing away`, `looking away`, `tilted head`, `lowres`, `bad hands`, `worstquality`, `jpegartifacts`, `username`, `blurry`, `bad feet`, `bad body`, `extra foot`,

### 出图效果

![](`img/fbad38ff7a57fe32e3d0d2120a4a962e.png`)

## 甘雨（ganyu）

![](`img/b4417b3dd073c43a2407fdcce814feb9.png`)

**lora：** `ganyuGenshinImpact_offset`

**权重：** 0.7-0.9

**Trigger Words：** `ganyu \(genshin impact\)`

**c站地址：** `https://civitai.com/models/11814/ganyu-genshin-impact-realistic-anime-lora`

**正：** `ganyu \(genshin impact\)`, `1girl`, `ahoge`, `bangs`, `bare shoulders`, `bell`, `black gloves`, `black pantyhose`, `((blue hair))`, `blush`, `chinese knot`, `detached sleeves`, `flower knot`, `gloves`, `horns`, `long hair`, `medium breasts`, `neck bell`, `pantyhose`, `purple eyes`, `sidelocks`, `tassel`, `white sleeves`,

例如：

`(masterpiece, best quality, absurdres, foreshortening)`, `photo of a highly detailed`, `(full body:1.2)`, `smile`, `cute face`, `(studio background)`, `(clean background)`, `(blurry background)`, `1girl`, `chibi`, `(beautiful detailed face)`, `(beautiful detailed eyes)`, `standing in chinese garden`, `rendered in blender`, `goggles on head`,

`ganyu \(genshin impact\)`, `1girl`, `ahoge`, `bangs`, `bare shoulders`, `bell`, `black gloves`, `black pantyhose`, `((blue hair))`, `blush`, `chinese knot`, `detached sleeves`, `flower knot`, `gloves`, `horns`, `long hair`, `medium breasts`, `neck bell`, `pantyhose`, `purple eyes`, `sidelocks`, `tassel`, `white sleeves`,

`<lora:blindbox_V1Mix:0.6>`, `<lora:ganyuGenshinImpact_v20:1>`

**负：** `(worst quality, low quality:1.4)`, `(monochrome:1.1)`, `bad_prompt`, `sketch by bad_artist`, `EasyNegative`, `deepnegative`,

### 正面关键词：

`(masterpiece, best quality, absurdres, foreshortening)`, `photo of a highly detailed`, `(full body:1.2)`, `smile`, `cute face`, `(studio background)`, `(clean background)`, `(blurry background)`, `1girl`, `chibi`, `(beautiful detailed face)`, `(beautiful detailed eyes)`, `standing in chinese garden`, `rendered in blender`, `goggles on head`,

`ganyu \(genshin impact\)`, `1girl`, `ahoge`, `bangs`, `bare shoulders`, `bell`, `black gloves`, `black pantyhose`, `((blue hair))`, `blush`, `chinese knot`, `detached sleeves`, `flower knot`, `gloves`, `horns`, `long hair`, `medium breasts`, `neck bell`, `pantyhose`, `purple eyes`, `sidelocks`, `tassel`, `white sleeves`,

`<lora:blindbox_V1Mix:0.6>` `<lora:ganyuGenshinImpact_v20:0.7>`

### 负面关键词：

`easynegative`, `fat`, `naked`, `nude`, `paintings`, `sketches`, `(worst quality:2)`, `(low quality:2)`, `(normal quality:2)`, `lowres`, `((monochrome))`, `((grayscale))`, `ugly`, `tiling`, `poorly drawn hands`, `poorly drawn feet`, `poorly drawn face`, `out of frame`, `extra limbs`, `missing limb`, `floating limbs`, `disconnected limbs`, `(mutated hands and fingers:1.4)`, `disfigured`, `deformed`, `body out of frame`, `bad anatomy`, `watermark`, `signature`, `cut off`, `low contrast`, `underexposed`, `overexposed`, `bad art`, `beginner`, `amateur`, `distorted face`, `disgusting`, `blurry`, `draft`, `grainy`, `amputation`, `bad hands`, `text`, `error`, `missing fingers`, `extra digit`, `fewer digits`, `cropped`, `worst quality`, `low quality`, `normal quality`, `jpeg artifacts`, `signature`, `watermark`, `username`, `blurry`, `bad feet`, `cropped`, `poorly drawn hands`, `poorly drawn face`, `mutation`, `deformed`, `extra fingers`, `extra limbs`, `extra arms`, `extra legs`, `malformed limbs`, `fused fingers`, `too many fingers`, `long neck`, `cross-eyed`, `mutated hands`, `polar lowres`, `bad body`, `bad proportions`, `gross proportions`, `skin spots`, `acnes`, `skin blemishes`, `DeepNegative`, `(fat:1.2)`, `facing away`, `looking away`, `tilted head`, `lowres`, `bad hands`, `worstquality`, `jpegartifacts`, `username`, `blurry`, `bad feet`, `bad body`, `extra foot`,

### 效果预览：

![](`img/7c6934d63c0cd3c350d54d21d3a83616.png`)

## 诺艾尔(Noelle)

![](`img/63b1ed8324f5c10fa5a46adf3632357e.png`)

**lora：** `genshinImpactNoelle_nV1`

**权重：** 无

**Trigger Words：** `noelle_\(genshin_impact\)`, `1girl`, `(green_eyes:1.2//)`, `silver hair`, `short_hair`, `medium_breasts`, `rose`, `red_flower`, `hair_flower`

**c站地址：** `https://civitai.com/models/9071/genshin-impact-noelle`

**正：** `hair ornamen`, `gloves`, `gilded frame`, `ganyu`, `twin horn`, `bare shoulder`, `bodysuit`

**负：** `noelle_\(genshin_impact\)`, `(green_eyes:1.2//)`, `silver hair`, `short_hair`, `medium_breasts`,

### 正面关键词：

`(masterpiece, best quality, absurdres, foreshortening)`, `photo of a highly detailed`, `(full body:1.2)`, `smile`, `cute face`, `(studio background)`, `(clean background)`, `(blurry background)`, `1girl`, `chibi`, `(beautiful detailed face)`, `(beautiful detailed eyes)`, `standing in chinese garden`, `rendered in blender`, `goggles on head`,

`noelle_\(genshin_impact\)`, `1girl`, `(green_eyes:1.2//)`, `silver hair`, `short_hair`, `medium_breasts`, `rose`, `red_flower`, `hair_flower`

`<lora:blindbox_V1Mix:0.6>`, `<lora:genshinImpactNoelle_nV1:0.7>`

### 负面关键词：

`easynegative`, `fat`, `naked`, `nude`, `paintings`, `sketches`, `(worst quality:2)`, `(low quality:2)`, `(normal quality:2)`, `lowres`, `((monochrome))`, `((grayscale))`, `ugly`, `tiling`, `poorly drawn hands`, `poorly drawn feet`, `poorly drawn face`, `out of frame`, `extra limbs`, `missing limb`, `floating limbs`, `disconnected limbs`, `(mutated hands and fingers:1.4)`, `disfigured`, `deformed`, `body out of frame`, `bad anatomy`, `watermark`, `signature`, `cut off`, `low contrast`, `underexposed`, `overexposed`, `bad art`, `beginner`, `amateur`, `distorted face`, `disgusting`, `blurry`, `draft`, `grainy`, `amputation`, `bad hands`, `text`, `error`, `missing fingers`, `extra digit`, `fewer digits`, `cropped`, `worst quality`, `low quality`, `normal quality`, `jpeg artifacts`, `signature`, `watermark`, `username`, `blurry`, `bad feet`, `cropped`, `poorly drawn hands`, `poorly drawn face`, `mutation`, `deformed`, `extra fingers`, `extra limbs`, `extra arms`, `extra legs`, `malformed limbs`, `fused fingers`, `too many fingers`, `long neck`, `cross-eyed`, `mutated hands`, `polar lowres`, `bad body`, `bad proportions`, `gross proportions`, `skin spots`, `acnes`, `skin blemishes`, `DeepNegative`, `(fat:1.2)`, `facing away`, `looking away`, `tilted head`, `lowres`, `bad hands`, `worstquality`, `jpegartifacts`, `username`, `blurry`, `bad feet`, `bad body`, `extra foot`,

### 效果预览：



![](img/a7942d7181c87398e0fbddc6dc3c272b.png)

## 胡桃（hutao）

![](img/4f1eb3e2a661c9d9a75aeec9b34aa654.png)

**lora：**`huTaoGenshinImpact_v12`

**权重：**0.6-0.8

**Trigger Words：**`hu tao(genshin impact)`, `boo tao`, `symbol shaped pupils`

**c站地址：**`https://civitai.com/models/7505/hu-tao-or-genshin-impact`

**正：**`hu tao (genshin impact)`, `boo tao`, `hat`, `red eyes`, `twintails`, `brown hair`, `symbol-shaped pupils`, `long hair`, `bangs`, `black shorts`, `chinese clothes`, `<lora:hutao_v2:0.7>`

**负：**`lowres`, `bad anatomy`, `bad hands`, `text`, `error`, `missing fingers`, `extra digit`, `fewer digits`, `cropped`, `worst quality`, `low quality`, `normal quality`, `jpeg artifacts`, `signature`, `watermark`, `username`, `blurry`, `artist name`, `bad-artist-anime`, `bad_artist`, `bad_prompt_version2`, `barbosa_style`

### 正面关键词

`(masterpiece, best quality, absurdres, foreshortening)`, `photo of a highly detailed`, `(full body:1.2)`, `smile`, `cute face`, `(studio background)`, `(clean background)`, `(blurry background)`, `1girl`, `chibi`, `(beautiful detailed face)`, `(beautiful detailed eyes)`, `standing in chinese garden`, `rendered in blender`, `goggles on head`,

`hu tao (genshin impact)`, `boo tao`, `hat`, `red eyes`, `twintails`, `brown hair`, `symbol-shaped pupils`, `long hair`, `bangs`, `black shorts`, `chinese clothes`, `<lora:hutao_v2:0.7>`

`<lora:blindbox_V1Mix:0.6>`,

### 负面关键词：

`easynegative`, `fat`, `naked`, `nude`, `paintings`, `sketches`, `(worst quality:2)`, `(low quality:2)`, `(normal quality:2)`, `lowres`, `((monochrome))`, `((grayscale))`, `ugly`, `tiling`, `poorly drawn hands`, `poorly drawn feet`, `poorly drawn face`, `out of frame`, `extra limbs`, `missing limb`, `floating limbs`, `disconnected limbs`, `(mutated hands and fingers:1.4)`, `disfigured`, `deformed`, `body out of frame`, `bad anatomy`, `watermark`, `signature`, `cut off`, `low contrast`, `underexposed`, `overexposed`, `bad art`, `beginner`, `amateur`, `distorted face`, `disgusting`, `blurry`, `draft`, `grainy`, `amputation`, `bad hands`, `text`, `error`, `missing fingers`, `extra digit`, `fewer digits`, `cropped`, `worst quality`, `low quality`, `normal quality`, `jpeg artifacts`, `signature`, `watermark`, `username`, `blurry`, `bad feet`, `cropped`, `poorly drawn hands`, `poorly drawn face`, `mutation`, `deformed`, `extra fingers`, `extra limbs`, `extra arms`, `extra legs`, `malformed limbs`, `fused fingers`, `too many fingers`, `long neck`, `cross-eyed`, `mutated hands`, `polar lowres`, `bad body`, `bad proportions`, `gross proportions`, `skin spots`, `acnes`, `skin blemishes`, `DeepNegative`, `(fat:1.2)`, `facing away`, `looking away`, `tilted head`, `lowres`, `bad hands`, `worstquality`, `jpegartifacts`, `username`, `blurry`, `bad feet`, `bad body`, `extra foot`,

### 效果预览：

![](img/70b07dd591eb6c6c3c86e4515691e4eb.png)

## 神里绫华(KamisatoAyaka )

![](img/23974f8f997e71947dad06f19dc980a4.png)

**lora：**`kamisatoAyakaGenshin_kamisatoAyaka`

**权重：**0.9 - 1.2

**Trigger Words：**`kamisato_ayaka`

**c站地址：**`https://civitai.com/models/9192/kamisato-ayaka-or-genshin-impact`

**正：**`<lora:kamisato_ayaka:1.2> arm guards, armored dress, breastplate, japanese armor, hand fan, fingerless gloves`

**负：**`lowres`, `bad anatomy`, `worst quality`, `text`, `username`, `watermark`, `signature`, `jpeg artifacts`

### 正面关键词

`(masterpiece, best quality, absurdres, foreshortening)`, `photo of a highly detailed`, `(full body:1.2)`, `smile`, `cute face`, `(studio background)`, `(clean background)`, `(blurry background)`, `1girl`, `chibi`, `(beautiful detailed face)`, `(beautiful detailed eyes)`, `standing in chinese garden`, `rendered in blender`, `goggles on head`,

`arm guards`, `armored dress`, `breastplate`, `japanese armor`, `hand fan`, `fingerless gloves`

`<lora:blindbox_V1Mix:0.6>`, `<lora:kamisato_ayaka:1.2>`

### 负面关键词：

`easynegative`, `fat`, `naked`, `nude`, `paintings`, `sketches`, `(worst quality:2)`, `(low quality:2)`, `(normal quality:2)`, `lowres`, `((monochrome))`, `((grayscale))`, `ugly`, `tiling`, `poorly drawn hands`, `poorly drawn feet`, `poorly drawn face`, `out of frame`, `extra limbs`, `missing limb`, `floating limbs`, `disconnected limbs`, `(mutated hands and fingers:1.4)`, `disfigured`, `deformed`, `body out of frame`, `bad anatomy`, `watermark`, `signature`, `cut off`, `low contrast`, `underexposed`, `overexposed`, `bad art`, `beginner`, `amateur`, `distorted face`, `disgusting`, `blurry`, `draft`, `grainy`, `amputation`, `bad hands`, `text`, `error`, `missing fingers`, `extra digit`, `fewer digits`, `cropped`, `worst quality`, `low quality`, `normal quality`, `jpeg artifacts`, `signature`, `watermark`, `username`, `blurry`, `bad feet`, `cropped`, `poorly drawn hands`, `poorly drawn face`, `mutation`, `deformed`, `extra fingers`, `extra limbs`, `extra arms`, `extra legs`, `malformed limbs`, `fused fingers`, `too many fingers`, `long neck`, `cross-eyed`, `mutated hands`, `polar lowres`, `bad body`, `bad proportions`, `gross proportions`, `skin spots`, `acnes`, `skin blemishes`, `DeepNegative`, `(fat:1.2)`, `facing away`, `looking away`, `tilted head`, `lowres`, `bad hands`, `worstquality`, `jpegartifacts`, `username`, `blurry`, `bad feet`, `bad body`, `extra foot`, `lowres`, `bad anatomy`, `worst quality`, `text`, `username`, `watermark`, `signature`, `jpeg artifacts`

### 效果预览：

![](img/bcd6dcbddfd56663069d9e4aa2c363cd.png)

## 刻晴（keqing）

![](img/2994117a8e5961b1aecc438c29316c22.png)

**lora：**`keqingGenshinImpact3in1_v10`

**权重：**0.7~0.85

**Trigger Words：**`keqing (genshin impact)`

**c站地址：**`https://civitai.com/models/15699/keqing-or-genshin-impact-or-3in1-lora-and-locon`

**备注：**

刻晴衣服太多，对应不同衣服官方给出了不同的触发词

**默认服装:**

`keqing (piercing thunderbolt)`, `keqing (genshin impact)`, `pantyhose`, `hair bun`, `purple hair`, `gloves`, `twintails`, `long hair`, `purple eyes`, `diamond-shaped pupils`, `bare shoulders`, `hair ornament`, `black pantyhose`, `cone hair bun`, `detached sleeves`, `dress`, `jewelry`, `medium breasts`, `earrings`, `bangs`, `frills`, `purple dress`, `black gloves`, `braid`, `skirt`,

**霓裾翩跹:**

`keqing (opulent splendor)`, `keqing (genshin impact)`, `official alternate costume`, `dress`, `cone hair bun`, `night`, `strapless dress`, `looking at viewer`, `long hair`, `cleavage`, `black dress`, `hair bun`, `strapless`, `bare shoulders`, `purple hair`, `bangs`, `bow`, `detached collar`, `purple eyes`, `diamond-shaped pupils`, `ribbon`, `double bun`, `twintails`, `two-tone dress`, `medium breasts`, `hair ornament`, `black bow`, `hair ribbon`, `blue dress`, `bowtie`, `hair between eyes`,

**海灯节:**

`keqing (lantern rite)`, `keqing (genshin impact)`, `hair bun`, `skirt`, `scarf`, `purple sweater`, `white skirt`, `purple hair`, `sweater`, `twintails`, `purple eyes`, `diamond-shaped pupils`, `hair ornament`, `bare shoulders`, `smile`, `breasts`, `cone hair bun`, `long hair`, `belt`, `double bun`, `long sleeves`, `bangs`, `bow`, `hair flower`, `hair bow`, `ribbon`, `hair ribbon`, `braid`, `plaid scarf`, `plaid`, `off shoulder`

**定制服装（？不太确定）：**

`keqing (genshin impact)`, `hair bun`, `purple hair`, `twintails`, `purple eyes`, `diamond-shaped pupils`, `hair ornament`, `cone hair bun`, `long hair`, `double bun`, `bangs`, `bow`, `hair flower`, `hair bow`, `hair ribbon`, `[Explain in detail what clouthes you would like to trigger......]`

**通用负词缀：**

`(worst quality, low quality, extra digits, loli, child, male:1.4))`, `bad_prompt`,

### 正面关键词

`(masterpiece, best quality, absurdres, foreshortening)`, `photo of a highly detailed`, `(full body:1.2)`, `smile`, `cute face`, `(studio background)`, `(clean background)`, `(blurry background)`, `1girl`, `chibi`, `(beautiful detailed face)`, `(beautiful detailed eyes)`, `standing in chinese garden`, `rendered in blender`, `goggles on head`,

`keqing (opulent splendor)`, `keqing (genshin impact)`, `official alternate costume`, `dress`, `cone hair bun`, `night`, `strapless dress`, `looking at viewer`, `long hair`, `cleavage`, `black dress`, `hair bun`, `strapless`, `bare shoulders`, `purple hair`, `bangs`, `bow`, `detached collar`, `purple eyes`, `diamond-shaped pupils`, `ribbon`, `double bun`, `twintails`, `two-tone dress`, `medium breasts`, `hair ornament`, `black bow`, `hair ribbon`, `blue dress`, `bowtie`, `hair between eyes`,

`<lora:blindbox_V1Mix:0.6>`, `<lora:keqingGenshinImpact3in1_v10:0.7>`

### 负面关键词：



`easynegative`, fat, naked, nude, paintings, sketches, `(worst quality:2)`, `(low quality:2)`, `(normal quality:2)`, lowres, `((monochrome))`, `((grayscale))`, ugly, tiling, poorly drawn hands, poorly drawn feet, poorly drawn face, out of frame, extra limbs, missing limb, floating limbs, disconnected limbs, `(mutated hands and fingers:1.4)`, disfigured, deformed, body out of frame, bad anatomy, watermark, signature, cut off, low contrast, underexposed, overexposed, bad art, beginner, amateur, distorted face, disgusting, blurry, draft, grainy, amputation, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry, bad feet, cropped, poorly drawn hands, poorly drawn face, mutation, deformed, extra fingers, extra limbs, extra arms, extra legs, malformed limbs, fused fingers, too many fingers, long neck, cross-eyed, mutated hands, polar lowres, bad body, bad proportions, gross proportions, skin spots, acnes, skin blemishes, `DeepNegative`, `(fat:1.2)`, facing away, looking away, tilted head, lowres, bad hands, `worstquality`, `jpegartifacts`, username, blurry, bad feet, bad body, extra foot, lowres, bad anatomy, worst quality, text, username, watermark, signature, jpeg artifacts

## 效果预览：

![](img/b2ff488c2b3bcb0084e5033f51529ad9.png)

## 可莉（klee）

`lora:kleeGenshinImpact_kleeGenshinImpact`

权重：0.65 - 1

Trigger Words：

Backpack, stuffed animal, backpack pendant, hat

`klee \(genshin impact\) \(cosplay\)`,

c站地址：`https://civitai.com/models/5615/klee-genshin-impact`

正：`<lora:kleeGenshinImpact_kleeGenshinImpact:1>`,blond hair, red dress,stuffed animal, backpack pendant, hat , Backpack, stuffed animal, backpack pendant, hat ,`klee \(genshin impact\) \(cosplay\)`, `(little girl:1.2)`

负：`(by bad-artist:0.9)`, `(bad_prompt_version2:0.9)`, multiple views, collage, projected inset, plain background, simple background, lowres, `((bad anatomy))`, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, `(text)`, caption, `((blurry))`, merge body parts, extra legs, extra hands, extra arms, distorted arms, malformed arms, mutation, bad body, bad legs

### 正面关键词

`(masterpiece, best quality, absurdres, foreshortening)`, photo of a highly detailed,`(full body:1.2)`, smile,cute face ,,`(studio background)`,`(clean background)`,`(blurry background)`, 1girl, chibi, `(beautiful detailed face)`, `(beautiful detailed eyes)`,standing in chinese garden, rendered in blender , goggles on head,

blond hair, red dress,stuffed animal, backpack pendant, hat , Backpack, stuffed animal, backpack pendant, hat ,`klee \(genshin impact\) \(cosplay\)`, `(little girl:1.2)`

`<lora:blindbox_V1Mix:0.6>`, `<lora:kleeGenshinImpact_kleeGenshinImpact:1>`

### 负面关键词

`easynegative`, fat, naked, nude, paintings, sketches, `(worst quality:2)`, `(low quality:2)`, `(normal quality:2)`, lowres, `((monochrome))`, `((grayscale))`, ugly, tiling, poorly drawn hands, poorly drawn feet, poorly drawn face, out of frame, extra limbs, missing limb, floating limbs, disconnected limbs, `(mutated hands and fingers:1.4)`, disfigured, deformed, body out of frame, bad anatomy, watermark, signature, cut off, low contrast, underexposed, overexposed, bad art, beginner, amateur, distorted face, disgusting, blurry, draft, grainy, amputation, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry, bad feet, cropped, poorly drawn hands, poorly drawn face, mutation, deformed, extra fingers, extra limbs, extra arms, extra legs, malformed limbs, fused fingers, too many fingers, long neck, cross-eyed, mutated hands, polar lowres, bad body, bad proportions, gross proportions, skin spots, acnes, skin blemishes, `DeepNegative`, `(fat:1.2)`, facing away, looking away, tilted head, lowres, bad hands, `worstquality`, `jpegartifacts`, username, blurry, bad feet, bad body, extra foot, lowres, bad anatomy, worst quality, text, username, watermark, signature, jpeg artifacts,`(by bad-artist:0.9)`, `(bad_prompt_version2:0.9)`, multiple views, collage, projected inset, plain background, simple background, lowres, `((bad anatomy))`, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, `(text)`, caption, `((blurry))`, merge body parts, extra legs, extra hands, extra arms, distorted arms, malformed arms, mutation, bad body, bad legs

### 效果预览：

![](img/4e3dca8d7f525464a0216c146331555d.png)

## 荧（lumine）

`lora:kleeGenshinImpact_kleeGenshinImpact`

权重：0.45 - 0.7

Trigger Words：

Backpack, stuffed animal, backpack pendant, hat

`lumine \(genshin impact\) \(cosplay\)`,

c站地址：`https://civitai.com/models/5330/lumine-genshin-impact`

正：`lumine_/(genshin_impact/)`, short hair, short hair with long locks, blonde hair, bangs, sidelocks, hair between eyes, yellow eyes, orange eyes, medium breasts, hair flower, white flower, `(flower)`, hair ornament, gold trim, blush,

负：`(worst quality, low quality:1.4)`, `(abs, muscular, rib:1.1)`, text, watermark, clothes,

### 正面关键词

`(masterpiece, best quality, absurdres, foreshortening)`, photo of a highly detailed,`(full body:1.2)`, smile,cute face ,,`(studio background)`,`(clean background)`,`(blurry background)`, 1girl, chibi, `(beautiful detailed face)`, `(beautiful detailed eyes)`,standing in chinese garden, rendered in blender , goggles on head,

`lumine_/(genshin_impact/)`, short hair, short hair with long locks, blonde hair, bangs, sidelocks, hair between eyes, yellow eyes, orange eyes, medium breasts, hair flower, white flower, `(flower)`, hair ornament, gold trim, blush,

`<lora:blindbox_V1Mix:0.6>`, `<lora:kleeGenshinImpact_kleeGenshinImpact:1>`

### 负面关键词

`easynegative`, fat, naked, nude, paintings, sketches, `(worst quality:2)`, `(low quality:2)`, `(normal quality:2)`, lowres, `((monochrome))`, `((grayscale))`, ugly, tiling, poorly drawn hands, poorly drawn feet, poorly drawn face, out of frame, extra limbs, missing limb, floating limbs, disconnected limbs, `(mutated hands and fingers:1.4)`, disfigured, deformed, body out of frame, bad anatomy, watermark, signature, cut off, low contrast, underexposed, overexposed, bad art, beginner, amateur, distorted face, disgusting, blurry, draft, grainy, amputation, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry, bad feet, cropped, poorly drawn hands, poorly drawn face, mutation, deformed, extra fingers, extra limbs, extra arms, extra legs, malformed limbs, fused fingers, too many fingers, long neck, cross-eyed, mutated hands, polar lowres, bad body, bad proportions, gross proportions, skin spots, acnes, skin blemishes, `DeepNegative`, `(fat:1.2)`, facing away, looking away, tilted head, lowres, bad hands, `worstquality`, `jpegartifacts`, username, blurry, bad feet, bad body, extra foot, lowres, bad anatomy, worst quality, text, username, watermark, signature, jpeg artifacts,`(by bad-artist:0.9)`, `(bad_prompt_version2:0.9)`, multiple views, collage, projected inset, plain background, simple background, lowres, `((bad anatomy))`,`(worst quality, low quality:1.4)`, `(abs, muscular, rib:1.1)`, text, watermark, clothes,

## 八重神子

### 正面关键词：

`<lora:miko-000014:0.75>`, `miko_genshin`, 1 girl, solo,`(realisic:1.1)` `(smile)`,`(sitting on the bench:1.2)`,`(upper body)`, `(smile)`,`(looking at viewer:1.1)`, japanese red and white dress, long sleeves, bare shoulders, bare legs, medium breasts, long sleeves, an ornament on the chest,outdoors, `(cityscape:1.5)`,streets,lamppost,

![](img/dfb9a66c5f3cdec14f1260efd4a54568.png)

## 原神-肖宫(yoimiya)

### 正面提示词：



`<lora:yoimiya2-000008:1>`, `yoimiyadef`, upper body, smile, blush, outdoors, day, simple background,

![](img/a2c5f9028ea9be88e359e2b9c55dc584.png)

## 原神-纳西妲(Nahida)

![](img/712ffa528874a332e0d3b5a644a32e39.png)

### 正面提示词：

masterpiece, best quality, 1girl, solo, `nahida_genshin`, cross-shaped pupils, `default_dress`, green cape, standing

## 原身-雷电将军(Raiden Shogun)

![](img/5b9c86d442650eedd0093d8bb614a255.png)

### 正面提示词：

(masterpiece, best quality, absurdres, foreshortening), photo of a highly detailed,(`full body:1.2`), smile,cute face ,,(studio background),(clean background),(blurry background), 1girl, chibi, (beautiful detailed face), (beautiful detailed eyes),standing in chinese garden, rendered in blender , goggles on head, (`full body`), (`purple hair:1`), blunt bangs, braid, wide sleeves, hair ornament, white shirt, skirt, red obi, Tokyo Street, night, purple necklace,(Soft light and shadow),(city street),A surge of people,powder blusher,walking,(arm behind the back),Blurred background,face focus,clear facial features

`<lora:blindbox_V1Mix:0.6>`, `<lora:RaidenShogun-HandsFix:0.7>`

通过这篇文章，我们希望您能够获得一种新的角度去理解和欣赏原神的艺术风格，同时也能掌握如何利用AI技术来创造新的艺术作品。

如果对你有所帮助，欢迎点赞收藏转发，谢谢！

![](img/8711199620e3646a4037d798a98f47c3.png)

![](img/dae96ef5e57c2d640599b53459073a9c.png)

![](img/1875030caab3ca94dfc7c4d19a92725e.png)

![](img/3282ae7f781d4b6fe757b3611a1ad60b.png)

![](img/7e209f54c3643d7e39dbb8c22a15423e.png)

![](img/ac2f5382fb9b6506d1334a212501e4dd.png)
