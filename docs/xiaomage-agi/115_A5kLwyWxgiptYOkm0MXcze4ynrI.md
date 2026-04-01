# 实操教程：斗罗大陆2

> 来源：[https://xmgtoagi.feishu.cn/docx/IwBBdhsxpo6b5jxqoOHcewFLnHd](https://xmgtoagi.feishu.cn/docx/IwBBdhsxpo6b5jxqoOHcewFLnHd)

今天，我将为大家分享如何利用lora包生成真实感十足且质感出众的人物图片。

斗罗大陆2-江楠楠-常服-出图效果展示：

![](img/4fe3370c6c317e3993b129d9c6101560.png)

## 资源整合：

本文所用到的模型、Lora、高清图片都放在网盘了，需要的自取

我用夸克网盘分享了「20230903-SD斗罗大陆-江楠楠」，点击链接即可保存。打开「夸克APP」，无需下载在线播放视频，畅享原画5倍速，支持电视投屏。

下载链接：https://pan.quark.cn/s/9083e2a4dbad

文件夹：20230903-SD斗罗大陆-江楠楠

![](img/095a1ee0c6eca19b6731c5708d56ca10.png)

## 大模型选择：

建议使用：3Guofeng3_v33

存放位置：novelai-webui-aki-v3(软件安装目录)\models\Stable-diffusion

## Lora选择：

Soul Land 2_Jiang Nannan

存放位置：novelai-webui-aki-v3(软件安装目录)\models\Lora

## 关键词

**正面关键词：**

```
A 28 year old girl, mature female, charming, sexy, mature, looking at the audience, city view, night,
 exquisite facial features, fair skin, big eyes, with charm in both eyes, double eyelids, beautiful eyes, hair accessories, long hair, fake animal ears, plump breasts, skirts, shirts, clothing cutting, rabbit ears, separated sleeves, 
prestige, luxury, jewelry, diamond, gold, pearl, gem, sapphire, ruby, emerald, intricate detail, delicate pattern, charming, alluring, enchanting, hair ornament, necklace, earrings, bracelet, armlet,halo
(fishnet thighhighs:1.25),
&lt;lora:Soul Land 2_Jiang Nannan:0.7&gt;
```

**负面关键词：**

```
(worst quality, low quality:1.4),monochrome,zombie,undefinedEasyNegative,lowres,bad anatomy,
bad hands,text,error,missing fingers,extra digit,fewer digits,cropped,worst quality,low quality,normal quality,
jpeg artifacts,signature,watermark,username,blurry,too many fingers,thick lips,oversized head,facing away,
extra arms,extra hands,extra ponytail,bad eyes,fused eyes,distorted eyes,multicolored eyes,small breasts,loli,
distorted background,overexposure,
,badhandv4, EasyNegative, ng_deepnegative_v1_75t
```

## 重要参数设置：

Steps: 25, //建议20-30之间选择测试

Sampler: Euler a,  //建议下面红框内的采样方法，出来的效果相对比较好

![](img/0682ac4cac9e5d7c04de3d1036892eee.png)

CFG scale: 7, //7-11之间选择测试

Seed: -1, //-1 为随机的意思

![](img/bb3dcb76bcb0c9d911a9a7d17b201081.png)

## 高清壁纸下载：

下载链接：https://pan.quark.cn/s/9083e2a4dbad

文件夹：20230903-SD斗罗大陆-江楠楠