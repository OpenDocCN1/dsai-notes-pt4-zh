
# DALL-E 3 绘图

> 来源：[https://langgptai.feishu.cn/docx/Hk7OdVZq2oL6TfxLbPacecQwnbb](https://langgptai.feishu.cn/docx/Hk7OdVZq2oL6TfxLbPacecQwnbb)

## 美女压阵

效果：

![](img/37c1df0459c083e61004f3251fd7f1bd.png)

> 来自

```
Illustration of a poised and beautiful anime-style female, around 25 years old, with a mature and captivating presence. Her hair is long, straight, and black, with a subtle teardrop beauty mark below her eye adding to her allure. Her figure is gracefully contoured, showcased in a sleeveless strappy nightdress with intricate, glowing runes. The ambiance of her sanctuary is that of a mystical retreat, filled with ancient books and magical instruments, all illuminated by the gentle light of the moon that highlights her serene and composed demeanor.
```

## 模型感图片

效果：

![](img/eb90d0a9916a021d110033f680bb77cb.png)

> 来自

```
3D depiction of an elegant humanoid mech, leaning forward and intently facing the viewer in a poised attack stance. The mech is dressed in a billowing, damaged cape that flows dramatically. Armed with a large sword and shield, it prepares to strike. The primary color is white, with touches of red, blue, and yellow embellishing the design. The tattered end of the cape adds to its battle-worn appearance.
```

## 增加DallE真实感的小技巧

可以使用 `Photo of` 做图片描述，增加图片的真实感，避开DallE 出人像图不拟真的问题，增加对滤镜相机等参数的描述效果更好。

效果：

![](img/cf3aa20e557bef51d5b2982e60183676.png)

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

| ![](img/d772983e2c7ab913d0fa178ae8422afe.png) | ![](img/2d14ce714a0fa3025f384fbf7d42aa0c.png) | ![](img/60b2353c6116db5063230bc18a5939e4.png) | ![](img/450d885b2b4b3b459f56c479740f996a.png) | ![](img/bde62aa1368379a9c7f0cd9c20a184e0.png) | ![](img/ad553d78000399f9906ef5e881bc1dcd.png) | ![](img/eee23ac6d6b04d24b2f74593f4efea5e.png) | ![](img/a57c9d4bf98a69476e4041303e0f52d6.png) |

| --- | --- | --- | --- | --- | --- | --- | --- |

| ![](img/00b9ea9aaccae8fea190f59e0183ec16.png) | ![](img/214016a752e1c713f2fa45010347c695.png) | ![](img/230573d73f1f15ab4a72dbed9b66c833.png) | ![](img/ca2469be03a011dfdfc3b9ae668796c2.png) | ![](img/53b2476fffba6022fd97aeefd1385863.png) | ![](img/2dbaec813da65bbd46743f0e1a9a649a.png) | ![](img/d68ce9deb2f1bd2233f60154e72ddac2.png) | ![](img/a04d41f90f0ef82edceee8db1070af92.png) |

| ![](img/613af402a2ebbc919b316fe40e5e15d9.png) | ![](img/1fe81ac584477190e72cfa7e501eeb7c.png) | ![](img/eb7bdcb340032e73f61a3f9d7926f539.png) | ![](img/d978147e3dde289a2288f3e9b5396165.png) | ![](img/b8ae8e297cc3da383f50fe0e9418a2e6.png) | ![](img/7522784f5684bc525d793c7ed7a0bdb0.png) | ![](img/54615e75e75ec8c84f94880bd660e764.png) | ![](img/f930e88fe8c631e0d182443644671290.png) |

> 来自

## 大头娃娃模型图片基础 Prompt 稳定复现版

适用于设计看板玩偶

> 来自

效果：

| ![](img/c094277e2fcaa6fcb3f7002601eb3be6.png) | ![](img/8894dcb141461cf2c8b740682d1a804f.png) | ![](img/e2976542f5f168a7845e42e49199f6e0.png) | ![](img/1c1d919de5eaf4b6381553b5a0672e2e.png) |

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

Step3：使用代码解释器`Advanced Data Analysis`，将挑选好的图片合成GIF。

效果如下：

![](img/ccded133c348d5ba96d6789d46cdf2fc.png)

![](img/0f960a1ef90f888be909afce970c257b.png)

![](img/49bab6395cf601bf91d1e74d323b69fd.png)

> 来自
