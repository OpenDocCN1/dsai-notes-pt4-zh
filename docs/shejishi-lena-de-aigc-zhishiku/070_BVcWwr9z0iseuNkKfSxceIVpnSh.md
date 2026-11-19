
# Ray3.14更新 实测10个主流AI视频平台

> 来源：[https://aigclena.feishu.cn/docx/Appld1GjfosCVYxA0vqcLReFn0b](https://aigclena.feishu.cn/docx/Appld1GjfosCVYxA0vqcLReFn0b)

2026.2.2

LumaAI上周发布了Ray3.14，原生1080p 高清，速度提升 4 倍，成本降低 3 倍，官方声称迄今最强的视频模型，拥有顶级细节、稳定性和动作一致性。实测效果怎么样呢，一起来看看吧。

![](img/503cd837b79f48130011aea81ac0c042.png)

## 更新说明

官方对Ray3.14的介绍：https://lumalabs.ai/blog/news/ray3_14

Ray3.14 is our most professional and powerful model, now with native 1080p video generation. It is 4x faster and 3x cheaper, and gives you the best ever quality and stability, with improved motion consistency for Modify Video.

What’s New in Ray3.14

+   **Native 1080p** across core Dream Machine workflows.

+   **Faster video** generation — 4× faster at 720p versus Ray3.

+   **Cheaper to use** — **3× cheaper** at 720p versus Ray3.

+   Better prompt adherence with fewer artifacts.

+   Improved consistency for Modify workflow for subjects, objects, and style across frames.

+   Modify Video up to 18s.

实测下来，基于3.14的文生视频、视频修改出来的画面，感觉还是可以的，如官方所说，没有太大水分，欢迎评论区交流你的使用体验哦~~

![](img/5ca125e8211087ce5af52d72ac634bd9.png)

[1月31日.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/T0G0bTyTfodILvxrJA2cmEuYnYf)

## 实测案例

### 1. 文字提示词

Extreme macro cinematic cherry pie landscape. Syrup like lava, crumbs as boulders, tiny hikers on whipped cream peaks. Shallow depth of field, photoreal texture.

译：极致微距、电影感的樱桃派景观：糖浆如同熔岩般流淌，碎屑化作巨石，微小的登山者行走在鲜奶油的山峰之上。浅景深，写实级纹理质感。

提示词里要求2个动作：糖浆熔岩流淌和人在行走，顺手在可灵、Sora、Runway、Vidu、万相、即梦、扣子、Veo3.1和MiniMax里测试了一下，右下角都有水印，Luma草稿模式，人是有行走动作的，Luma正常使用反而没有什么动效。

### 可灵

[kling_20260131_文生视频_Extreme_ma_4553_0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SAkPbpcI1oKA7hx2KJmciOJanNg)

### luma

[2026-01-31T09-48-10_extreme_macro_watermarked.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YnwpbvUGGoDSzVx5ECqcvWaEnNf)

[2026-01-31T15-55-58_extreme_macro_watermarked.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EjVXb20Zto1JurxNOVdc4Md6n6f)

### 扣子

![](img/dd6d6a7ef2b0db2d97cfe75018f00ad6.png)

[扣子樱桃派微距景观.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EDp6bEoqnoxGnXxpknKcHAR4nMb)

### 通义

![](img/a14494a3d889f69a20773de3852af9f2.png)

[Wan_视频生成_Extreme macro cinematic cherry pie landscape. Syrup like lava, crumbs a.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZLKYbMANroFnHyxFenAcqPmjnpf)

### 即梦

![](img/bfb4bfaa19834e4029ae0ad88f622d3d.png)

[jimeng-2026-02-02-5370-Extreme macro cinematic cherry pie lands....mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TcWjbtEpqoEGV1xTBr2cef69n15)

### Veo

![](img/2971ff871bdfaf3436edca7e17801322.png)

[Extreme_Macro_Cherry_Pie_Landscape_Video (1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RmVIbk0SqowpQ8xs2FocT6AdnIP)

### Vidu

![](img/e70f50b1295db0969a5cbcbb0c6a9ff6.png)

[vidu0-video-3148758582009664.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DcDjbpaqmoVQZMxaNdacgiDAnhb)

### Sora

![](img/eb5270106194644bd43c3c1d6acf5bdb.png)

[20260202_1717_01kgerms4pe0z821qmnkpg8xfy.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HSbLbPGqCoEEnrxrkfNcNkF5nRf)

### Runway

![](img/111eab89504df2857fb3cce82baa5530.png)

[Gen-4_5 Extreme macro cinematic cherry pie landscape Syrup like lava, crumbs as boulders, tiny hikers on whipped cream peaks Shallow depth of field, photoreal texture 2164511093.gen-4_5 extreme macro cinematic cher.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OVFwbCFygoN0A7xiB8VctZehnRd)

这个是Runway的有点吓人==。

流淌动作不明显，于是我修改了一下提示词，把Syrup like lava这一句强调了一下动作，改成了：Syrup flowing like molten lava。

Extreme macro cinematic cherry pie landscape. Syrup flowing like molten lava, crumbs as boulders, tiny hikers on whipped cream peaks. Shallow depth of field, photoreal texture.

修改了之后有明显的流淌效果

![](img/057fcda9f1b0f1ac503fea1c34dd4fcf.png)

[Wan1_视频生成_Extreme macro cinematic cherry pie landscape. Syrup flowing like molten.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UAyubRvPxopRqFxqEGQcV78unSe)

[Extreme_Macro_Cherry_Pie_Landscape_Video.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QGbpbEKjyoJRetxKIa6cV2ebn7f)

![](img/35a5aeb9ec6c0beae85d520dab80aa02.png)

[jimeng-2026-02-02-3482-Extreme macro cinematic cherry pie lands....mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UxvLbALhEoy1pTxOqhDckjOanQf)

![](img/7f1ffdf5b566c2d222800f342368d41f.png)

[vidu-video-3148751168778615.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LElFbYWOnou5l4xS4bCcIM3gnZg)

![](img/8716cc9e8a700317ae978393b5a5525c.png)

[Gen-4_5 Extreme macro cinematic cherry pie landscape Syrup flowing like molten lava, crumbs as boulders, tiny hikers on whipped cream peaks Shallow depth of field, photoreal texture 1283539052.gen-4_5 extreme macro.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MiB6bfLJZosY7fxEo7Cc0Mhon0j)

#### Runway的效果

![](img/78dcc4292284d8c83c80a1b71099d52b.png)

[20260202_1717_01kgervgcsehnvvcp0e58c1tqj.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ItPZbSZvIolED5xEfHVc6v4gnUc)

### 海螺

![](img/1cc0779bcb0ebdc856753da5e1f95c63.png)

[Hailuo_Video_Extreme macro cinematic cherry_474646909589512200.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HolQbCNAwotvcpxwjLIcCMklnSc)

### 2. 图+提示词

电影感的特写镜头，冰封的河面缓慢裂开，半透明的冰浪翻涌而起，在金色阳光下闪烁光芒。钻石般的冰晶从裂缝中喷涌而出，在冰面上飞散跳动。河面上升起淡淡水雾，岸边的雾凇树在冬日阳光下泛着微光，营造出史诗般的奇幻氛围。超写实，高细节，动态质感。

![](img/0c4da746c1f0567e2c4eef435da8a1c0.png)

这段提示词，可灵的整体的氛围感要稍强于Luma，比如太阳、有雾气升起这样的小细节

[kling_20250919_图生视频_电影感的特写镜头_冰_5942_0 (1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FnAJbTVVJoOV4uxm7lCcLzgNnHf)

[2026-01-31T09-54-20_%E7%94%B5%E5%BD%B1%E6%84%9F%E7%9A%84%E7%89%B9%E5%86%99%E9%95%9C%E5%A4%B4_%E5%86%B0%E5%B0%81%E7%9A%84%E6%B2%B3%E9%9D%A2%E7%BC%93%E6%85%A2%E8%A3%82%E5%BC%80_watermarked.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Xe0nbYjTZoy0tTx5A85c7UejnVd)

[冰封河流奇幻破裂视频.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TG5ib5hOroN0IsxTtlncKjNDnyp)

远景：航拍从高空俯瞰北国大地，漫天大雪纷飞，冰封的江河和大地延伸至地平线。摄影机运动：缓慢推进，镜头穿越风雪云层，展现无尽的白色世界。

*图片是扣子生成的，有扣子水印*

![](img/f09137d1dd14666a12a0869973a240f6.png)

[雪国大地视频生成.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EF08bSCaioCt9uxI9R2ciAJ0nIf)

![](img/37069a21979f30a7104552b4668367cc.png)

[kling_20250919_图生视频_远景_航拍从高空俯瞰_4142_0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/R7jNbFbWjob4QJx1R1Wc2vXZn3d)

![](img/1d35d6f2e5399640a7a8f1dfd61fa2a7.png)

[Gen-4_5 远景：航拍从高空俯瞰北国大地，漫天大雪纷飞，冰封的江河和大地延伸至地平线。摄影机运动：缓慢推进，镜头穿越风雪云层，展现无尽的白色世界。 1232845574.gen-4_5 远景：航拍从高空.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LUedb4rfaoAzOVxEUfPcE9eUn0f)

#### Runway的效果

### 3. 修改功能

#### 3-1

初始文生视频提示词 follow a female detective walking down a dark alley inIstanbul. The alley is dimly lit with broken street lights

![](img/354e917fcdcb8a88729649c21876d1c4.png)

[2026-01-31T10-06-02_follow_a_female_watermarked.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BttTbAzOdowJJLxVh26cw7R8nkg)

修改场景提示词：The environment changes to a winter snowy landscape, with heavy snowflakes falling from the sky.

![](img/4a9c986b11946492aa7ac0863896c6e3.png)

[2026-01-31T10-28-52_the_environment_with_audio_watermarked.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WCG5b7vlJooU7YxUTzXcIIlfnHe)

效果可以的 遵循提示词能力和动作复刻 较强

#### 3-2

原本打算用这张做角色参考的，做好了以后发现Ray 3.14用不了

![](img/a7660d884c3a16e48182a7add15887ab.png)



![](img/5b731963a03ddd777856c71447e409c4.png)

[1月31日 (3).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZaQ9bdEfKoGVWwxQr24cN3YBnpf)

复刻源视频

![](img/f18996732a2f10b6a4ab71e3bd3b44ae.png)

![](img/8d82544e7c751a78634d34613cc22ec3.png)

[2026-01-31T13-56-42_generation_with_audio_watermarked.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EIGubLCFvoGW79xH78NcGC66nHc)

3.14动作完全复刻了的 质感也还可以 就是角色是不对的 马面人身才是对的

![](img/3083af6f65088e019e04838be2841bad.png)

[2026-01-31T14-21-04_%E8%83%8C%E6%99%AF%E6%94%B9%E4%B8%BA%E7%BA%A2%E8%89%B2%E7%9A%84%E8%B4%A8%E6%84%9F%E8%83%8C%E6%99%AF_watermarked (1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KN2jbFcEOo6LYWxxMkScTnJDnWe)

角色参考目前3.14用不了，用3roll出来的画面效果和动作复刻的效果比较一般

首帧换成了空山基的这个机器人，动作复刻和画面感觉效果还可以

![](img/24832035fbb3fbe01694e4fb5d0a0035.png)

![](img/f290f4afc6bc109537b1ec7f69f392eb.png)

[2026-02-01T10-08-34_generation_with_audio_watermarked.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Y4mebGZewo2c9HxCi7acWuuBnRb)

# 费用

月付$10、$30和$95，年付在此基础上打八折

![](img/be474e6f272677b52246ce588e67bbd8.png)

![](img/49658423018e76e75dcfff985050d37d.png)

![](img/1b3d18f2e656aa22dfbff2ad4ca2aca0.png)

![](img/d26217e3fd0484bb4509ef31da9d90d8.png)

![](img/ed44cda4b86d805aaa226dce081e8e02.png)

![](img/f761aec9d8c6e1284ee0147f154dca80.png)
