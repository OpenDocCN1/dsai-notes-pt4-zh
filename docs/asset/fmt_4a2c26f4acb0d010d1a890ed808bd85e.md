
# 实操教程：冰霜旗袍美女

> 来源：[https://xmgtoagi.feishu.cn/docx/QZC8d2ZNqoVmHvxjziUcG6n5nBc](https://xmgtoagi.feishu.cn/docx/QZC8d2ZNqoVmHvxjziUcG6n5nBc)

但是还有人就问：安装是安装好了，可是为什么生成的图片和你生成的图片差距那么远呢？

怎么真实感和质感一个天一个地呢？

![](img/c45d058cf3e029cc2700ed9a9fafd859.png)

那么，我画的图，和你画的图，差别在哪里呢？

很关键的因素，就是别人用了对的对应大模型`model`和专门的`lora`包。

今天我们就来一波实操，如何用`lora`包生成超真实好看质感超强的人物图片。

## 出图效果

![](img/bca676368607296169ecadbbaa487b03.png)

更多图片资源访问查看：https://pan.quark.cn/s/ffcd1bf4b623

## 教程整合资源包下载

AI绘画美女教程整合资料包：https://pan.quark.cn/s/ffcd1bf4b623

## 教程部分

### 大模型和 Lora 下载

国内推荐：https://www.liblibai.com/

国外推荐：https://civitai.com/

本教程整个资料包下载：https://pan.quark.cn/s/ffcd1bf4b623

![](img/00742115a992814ea860fcf2d86b0192.png)

### 大模型选择

真人大模型一般都是使用的`Chilloutmix`

存放位置：

`novelai-webui-aki-v3(软件安装目录)\models\Stable-diffusion`

![](img/81b431ace63aadf9ef5edf5e30f5d6e8.png)

### Lora 选择

#### Lora

我这里使用的：`add_detail`

![](img/2128914013452958be65ec81a0ae8556.png)

![](img/d6dfc3a57abff36b5fae995c2a272f5f.png)

存放位置：

`novelai-webui-aki-v3(软件安装目录)\models\Lora`

![](img/5b84e243a59b1c3c7a38b11d0e65c8db.png)

#### 可选附加网络 Lora

可选附加网络添加`Lora`如下图，所有用到的资源已打包到网盘。

![](img/4d85f412471a567c20bfc8a6ef682b8c.png)

存放位置：

`novelai-webui-aki-v3(软件安装目录)\extensions\sd-webui-additional-networks\models\lora`

![](img/e59092fcace9746169ab4cb142cb6e17.png)

### 参数设置

```
Steps: 22, //建议20-30之间选择测试
Sampler: Euler a,  //建议下面红框内的采样方法，出来的效果相对比较好
CFG scale: 7, //7-11之间选择测试
Seed: -1, //-1 为随机的意思
Size: 640x1024,
Denoising strength: 0.25,
Clip skip: 2,
ENSD: 31337,
Hires upscale: 2,
Hires steps: 15,
Hires upscaler: 4x-UltraSharp,
VAE: vae-ft-mse-840000-ema-pruned.ckpt
```

### 正面关键词

```
((masterpiece)),(highres:1.3), (((best quality))), ((ultra-detailed)), ((illustration)),(photorealistic:1.3),(realistic:1.4),(Cinematic Lighting:1.3),

aynamic angle, in the winter,(snow white forest:1.1), hazy mist,,(on the hot springs:1.3) ,(many thin detailed snowflakes:1.3), (bloom), (shine), (twinkling stars:), (snowing), many white tree, detailed snowflakes,

cowboy shot,

(1 girl),(((solo))),running,(between legs:1.3),

(chinese girl:1.1),(big breasts:1.3),

hanfu,hanfugirl, beautiful detailed eyes,(black eyes:1.2),,(pale firered hair:1.3),light smile,

,(beautiful detailed face:1.3),(cute), blushing,, light smile, makeup ,lipstick,detailed clothes,,(pale green transparent tight floral qipao dress:1.35),

(white  fishnet thighhighs:1.25),

<lora:add_detail:0.5>,
```

### 负面关键词

```
(cleavage:1.4),(Multiple people),(((simple background))),monochrome ,lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality,

normal quality, jpeg artifacts, signature, watermark, username, blurry, lowres, bad anatomy, bad hands, text, error, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry, ugly,pregnant,vore,duplicate,morbid,mut ilated,tran nsexual, hermaphrodite,long neck,mutated hands,poorly drawn hands,poorly drawn face,mutation,deformed,blurry,bad anatomy,bad proportions,malformed limbs,extra limbs,cloned face,disfigured,gross proportions, (((missing arms))),(((missing legs))), (((extra arms))),(((extra legs))),pubic hair, plump,bad legs,error,legs,username,blurry,bad feet ,(cleavage:1.4)
```

SD人物关键词整理：[StableDiffusion的关键词分类查询](https://y3if3fk7ce.feishu.cn/sheets/Wd2ZsLzTDh12LbtmlEgcp18Onjd)

## 附部分图片预览

![](img/ef3f558fefbde17f9a195a19ea55b78f.png)

![](img/3c304312a281a701e57194215d243683.png)

![](img/9726661d3666a463055422241d1ffd3c.png)

![](img/953d827e30da9d9d0d55f53c8bebf4eb.png)

![](img/a3948716f0f8a8d5da6cf0dbed7d9ba0.png)

![](img/8a976754ccbdbfd56f4460eb82517362.png)

![](img/64e0af81b45014daa677e21bc72d6fef.png)

![](img/3a27fdbfa2afa86d2df98d18f66ea273.png)

![](img/f7a59baf463529cd2edc0fdf7a6e7c76.png)

![](img/0cc14363da062a734f1210ae350e5498.png)

![](img/432f3548c631b3a38a62225f2cd02744.png)

![](img/a2b9c80baddacd7c55a0db3ef9f793b1.png)

![](img/a8e16d22f9d1a9be268110738fd2c587.png)

![](img/4619171c7ab8f312867c60165dfd290c.png)

![](img/50e6ebcd081c0377cff78e619f75f2c0.png)

![](img/1da8da93bd26149653541f64c776731c.png)

![](img/93333226540965311079d7a981710e5a.png)

![](img/7662eccde542f280990ae5cef1959b1a.png)

![](img/7facf3a625b337828f6ec19e1f4af8ad.png)

![](img/0f11552678c0b7cdbc99dc7744c72fe4.png)
