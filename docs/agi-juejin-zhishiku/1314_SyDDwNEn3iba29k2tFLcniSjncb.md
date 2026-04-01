# 9. DALL-E 3 绘图

> 来源：[https://agijuejin.feishu.cn/docx/DZZEd92Y1oLmfZxq8xGciVb3nie](https://agijuejin.feishu.cn/docx/DZZEd92Y1oLmfZxq8xGciVb3nie)

## 美女压阵

效果：

![](img/8dac2aee6fdc931ca8003131b9f1edef.png)

> 来自

```
Illustration of a poised and beautiful anime-style female, around 25 years old, with a mature and captivating presence. Her hair is long, straight, and black, with a subtle teardrop beauty mark below her eye adding to her allure. Her figure is gracefully contoured, showcased in a sleeveless strappy nightdress with intricate, glowing runes. The ambiance of her sanctuary is that of a mystical retreat, filled with ancient books and magical instruments, all illuminated by the gentle light of the moon that highlights her serene and composed demeanor.
```

## 模型感图片

效果：

![](img/fde88b4f5ab28951c279d124ab2c9234.png)

> 来自

```
3D depiction of an elegant humanoid mech, leaning forward and intently facing the viewer in a poised attack stance. The mech is dressed in a billowing, damaged cape that flows dramatically. Armed with a large sword and shield, it prepares to strike. The primary color is white, with touches of red, blue, and yellow embellishing the design. The tattered end of the cape adds to its battle-worn appearance.
```

## 增加DallE真实感的小技巧

可以使用 **Photo of**  做图片描述 增加图片的真实感 避开DallE 出人像图不拟真的问题，增加对滤镜相机等参数的描述效果更好

效果：

![](img/990c39f23e641b06b0e676cb633c51db.png)

> 来自

```
Photo of a Korean woman with girl group standards of beauty, showcasing a grainy Fujifilm camera texture, while she is casually leaning against a wall with a relaxed expression.
```

## 基于模型感的生产力扩展

初始prompt如下：

```
3D depiction of an elegant, slender humanoid mech, leaning forward and intently facing the viewer in a poised attack stance. The mech is adorned with mechanical roses, enhancing its grace. Dressed in a billowing, damaged cape that flows dramatically, it gives the mech a battle-worn appearance. The mech wields an elegant long sword, with the shield removed. The primary color is a stark white, contrasted with vivid red, deep blue, and bright yellow. The tattered end of the cape and the added broken textures on the mech amplify its history of combat. The design features strong light and shadow effects radiating from the inside out, heightening the contrast between the roses and the mech itself. All of this is portrayed in a dark, gothic art style.
```

第二轮对话开始，直接回复“继续”让模型稳定输出，10轮对话以内效果最佳~

| ![](img/d05b9803b67e29f566d01eb75a434421.png) | ![](img/b280b5f3292058fe9c8af0bb38f35698.png) | ![](img/4e8a726f7850ed13624ee42f53a19168.png) | ![](img/68fc403868a2d94d3f652df140547865.png) | ![](img/07158c92ce88e8ac4f549aa603499164.png) | ![](img/2cb11ac58724e740a68092f70d4bf24e.png) | ![](img/e41d04543a2f664c36007ef5e76ba19d.png) | ![](img/437e20caafdc1c4c5c1dbb980cd7540a.png) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ![](img/d07d48f850a50db5d921ff5d1394a214.png) | ![](img/f3edb715f3a1c716cb779ba4e4dfe731.png) | ![](img/127322c99c68255e5c8e2b141275841f.png) | ![](img/ffe654cc5432cbb286197e96e857870d.png) | ![](img/35ee47ca9d064ae8eead65fbb0ab7826.png) | ![](img/22a00e584afcffc03de2e4d40077298c.png) | ![](img/8dc9ea0942146e6b3b956b9b844ea17d.png) | ![](img/48c2f544fbf4a05f7ef7bce6ad357684.png) |
| ![](img/195aa8391b88554edce21e3c2748075d.png) | ![](img/165b2cf092772463f2caf2e150dc03bb.png) | ![](img/b6603ed913f142da6efd9dc4b80198be.png) | ![](img/10307fe09e40762f72badbf9cb84e4cb.png) | ![](img/a9ae1b44fb19f779d4b084810fab4282.png) | ![](img/b3e6ea510ec33e4c473594f03bb580fd.png) | ![](img/63c40baf4db0aec6d68643596ca833d1.png) | ![](img/f01a1557b999df66304ab1b65b6df7ca.png) |

> 来自

## 大头娃娃模型图片基础 Prompt 稳定复现版

适用于设计看板玩偶

> 来自

效果：

| ![](img/bef6022476adbdc2084c57749243ac00.png) | ![](img/4d84adf200ca65fa11003c5b764a2aec.png) | ![](img/1802b2d7795a2b90f91b1bc8cb837f7f.png) | ![](img/d2310928005da1925d8add5a4b6ce608.png) |
| --- | --- | --- | --- |

```
Photo of a chibi-style girl figurine standing on a base. Her face is beautifully crafted with intricate details, especially her mesmerizing eyes.
```

## 生成 "动画"

总体思路是通过Dalle3绘制序列帧，结合代码解释器生成GIF文件。

Step1：生成图片

```
You are an experienced Japanese manga artist. Please create a sprite sheet for the motion of 【.....】
```

Step2：裁切、挑选合适的图片。

Step3：使用代码解释器[Advanced Data Analysis],将挑选好的图片合成GIF。

效果如下：

![](img/d07a11c766a440c7676a1a2830d3422e.png)

![](img/6a73afdb74a6620aab8b580ac3a8f9ff.png)

![](img/ff0c8e0da933204e9e85e61312521796.png)

> 来自