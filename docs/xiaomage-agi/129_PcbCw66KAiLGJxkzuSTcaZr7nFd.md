# DALL-E 3 绘图

> 来源：[https://xmgtoagi.feishu.cn/docx/YJ8FdpYlEo3vmbx0lkWcbzDknxc](https://xmgtoagi.feishu.cn/docx/YJ8FdpYlEo3vmbx0lkWcbzDknxc)

## 美女压阵

效果：

![](img/c8c91ed2e4d3edad13e8de4eca1d5a8f.png)

> 来自

```
Illustration of a poised and beautiful anime-style female, around 25 years old, with a mature and captivating presence. Her hair is long, straight, and black, with a subtle teardrop beauty mark below her eye adding to her allure. Her figure is gracefully contoured, showcased in a sleeveless strappy nightdress with intricate, glowing runes. The ambiance of her sanctuary is that of a mystical retreat, filled with ancient books and magical instruments, all illuminated by the gentle light of the moon that highlights her serene and composed demeanor.
```

## 模型感图片

效果：

![](img/c2d5ddc7f869cf97f2b35e156a0c5481.png)

> 来自

```
3D depiction of an elegant humanoid mech, leaning forward and intently facing the viewer in a poised attack stance. The mech is dressed in a billowing, damaged cape that flows dramatically. Armed with a large sword and shield, it prepares to strike. The primary color is white, with touches of red, blue, and yellow embellishing the design. The tattered end of the cape adds to its battle-worn appearance.
```

## 增加DallE真实感的小技巧

可以使用 **Photo of**  做图片描述 增加图片的真实感 避开DallE 出人像图不拟真的问题，增加对滤镜相机等参数的描述效果更好

效果：

![](img/4e4925742a16d2b76331fcdb0f8685ba.png)

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

| ![](img/cc563128dfb4534da1d424879acff0d6.png) | ![](img/17a604f343068717a56033bb1afa13be.png) | ![](img/7ce34fd540b25d182151c87fd3059fc6.png) | ![](img/b9ca0c52f8c4ecd3aaae63eb5bedbb0f.png) | ![](img/28c59c08a746fefa13d194f9d03bf03f.png) | ![](img/06a30ecd468d995c6d96357b62631983.png) | ![](img/c86ae617f9484d350ce08216ccac0ee9.png) | ![](img/c2830ca12cb36e5aee8e3ae83ad60135.png) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ![](img/68ad7bc0107545a10b493793b216aa76.png) | ![](img/bf16c81525e9528ff710c1b7fd30c198.png) | ![](img/b0fcd9391265137feed5e61ba63a7a8c.png) | ![](img/69be2234ff828168779461bb7da4aa59.png) | ![](img/4c674d848e0a59a46c957484961bc598.png) | ![](img/67b4f836ec65a4d7b72a70d803559962.png) | ![](img/a9dbf9d90e1eca9000ca5fc13a701909.png) | ![](img/daab8493cb34c00032e9357f33041b71.png) |
| ![](img/e18cbe802dba25e2f3d4d01296b3dd59.png) | ![](img/8e7a38cb986577a77d43dd63f44e19f8.png) | ![](img/1af52e5d08ad58eb71a61edc166b7d87.png) | ![](img/f4aa84542d5ba02cafb6ac45cec488c3.png) | ![](img/7b48c206f4cfdfe0ed6046cf2b2376f4.png) | ![](img/ccbc427b2e9772b87dfbc273f466b302.png) | ![](img/8f2da9e3b5785cc19b91f82b09c5a6d3.png) | ![](img/c4f67e7c4ab5c7522200fe9c0288ab5c.png) |

> 来自

## 大头娃娃模型图片基础 Prompt 稳定复现版

适用于设计看板玩偶

> 来自

效果：

| ![](img/613a5d40157b6683566554c8a5795fb7.png) | ![](img/8d4d3011e2ce23c01f82b438998ff649.png) | ![](img/1d953ae442872fa7f604af4b51980d68.png) | ![](img/c5e175779d1499384aa00b71718282c1.png) |
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

![](img/c14f5d28a2569fdaa5a60188feaa6c74.png)

![](img/83e364cfcfb8f77a50848d201c9408b0.png)

![](img/4e28c0dd2e2fd9644053b674778d3094.png)

> 来自