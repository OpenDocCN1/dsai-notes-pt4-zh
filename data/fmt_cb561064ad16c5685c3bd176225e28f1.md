
# 利用 MJ 实现连续性的人物

> 来源：[https://my.feishu.cn/docx/AkbIdhnuXogGUPxNGrncIAQJnge](https://my.feishu.cn/docx/AkbIdhnuXogGUPxNGrncIAQJnge)

Midjourney 在生成图片时，让人很惊艳。MJ 可以创造出高质量的作品，但有很大的随机性，即使是输入同样的命令，机器人每次输出的结果都不一样。想利用 MJ 创作漫画、儿童读物动画等，对于生成稳定的角色并赋予人物不同服装、动作、场景、表情，就面临一个很大的难题：如何保持角色输出的一致性呢？

## 一、控制角色一致性五种方法

接下来具体介绍五种利用 Midjourney 输出统一性角色的方法，方法有简到繁，涉及到一些操作、后缀参数、`prompt`，适合有一定基础的同学查看，新手小白可以看去看之前发的基础操作。

### 1. 垫图法

#### 第一步：文生图—关键词生成角色

先在 MJ 中输出关键词，创建一个自己喜欢的角色，选择自己喜欢的图片放大。

描述词：`Little girl, white background, png, cartoon, cute, big head, short sleeves, super quality, cartoon style, 3D, full body front view --no outline --s 750 --ar 3:4 --v 5.0`

![](img/8406e40cf44606939492bf00220a775a.png)

在这里我选择了 `U3`。

![](img/4c4c6a982c73cc0a916b34d0208f9bab.png)

#### 第二步：将图片存储到桌面

![](img/65614ca983be543b0ce5a957decf92d2.png)

#### 第三步：图生图

也就是官方的以图生图模式。

上传喜欢的照片 + 描述词。

![](img/6bfc17142aa42ffd4c9a08a07d7d1b00.png)

#### 第四步：效果对比

大家可以对比一下原图与本次生成的图像，我们可以看出该方法生出角色的一致性很高，风格和整体氛围基本一致。

右边一张是没有修改任何关键词的。

![](img/a95355dd260638fb18605ed1ff51d066.png)

![](img/65cf0f969cdebba22dcc664c88357155.png)

下面这四张是修改过关键词并增加了卧室背景：垫图 + 关键字 `Little girl, cartoon cute big head, short sleeves, super quality, cartoon style, 3D, full body front view, （bedroom background,） --no outline --s 750 --iw 2 --ar 3:4 --v 5.0`

![](img/098dc979cedfd7276c490b9e38fca9fd.png)

### 2. Seed 值法

这个方法可以利用一张图片，生成一系列图片。

#### 第一步：关键词生成角色

这一步与前一个方法一样，在 Midjourney 中输出关键词，创建一个自己喜欢的角色，选择 `U3` 作为垫图生成相似的角色。

描述词：`Little girl, white background, png, cartoon, cute, big head, short sleeves, super quality, cartoon style, 3D, full body front view --no outline --s 750 --ar 3:4 --v 5.0`

#### 第二步：找到图片 `seed` 值

右下方点击获取 `seed` 值。

![](img/c5242f4086fd6395176e988c3ee91bdc.png)

`seed` 值可以在高级参数中填入。

![](img/d30460e41e75d4f8cf50a971922086f3.png)

#### 第三步：记住万能公式：替换关键词 + `seed` 值

可以修改你的关键词。

描述词：`Little girl, white background, png, cartoon, cute, big head,skirt, super quality, cartoon style, 3D, full body front view --no outline --s 750 --seed 2939824962 --iw 2 --ar 3:4 --v 5.0`

![](img/c4b0cb7c10274b43cf133fb1a9aa0100.png)

下面图片是没有修改关键词的：`Little girl, white background, png, cartoon, cute, big head, short sleeves, super quality, cartoon style, 3D, full body front view --no outline --s 750 --seed 2939824962 --iw 2 --ar 3:4 --v 5.0`

![](img/c54f8b5e9abc4ab475ed9656ad3f3369.png)

#### 第五步：效果对比

可以看到效果还行，多跑几组，总会有想要的，但是注意关键词中不要改变太多，比如改变衣服和背景最好每次只改一个。

![](img/3bd074e03c231eadd49590abc670276e.png)

![](img/5f54a28c7188e6fd43aba9bbbf88c8f6.png)

![](img/1db49a7f943562f8d7b3a639dea42124.png)

![](img/b8495f40b544f0207f2ddf59c5306f2a.png)

### 3. `Panels` 动作法

这个方法可以帮助创建连续的动作，为了保持角色一致性，在这里我使用了图生图也就是垫图，延用了之前的关键词。

#### （1）生成不同的动作：`N panels with different poses`

我们输入 `4 panels with different poses + 描述词` 得到不同动作的角色图。

描述词：`4 panels in different poses, blind box style, cute little girl, full body, looking at camera, bubble mart, octane rendering, super detailed, edge lighting, chibi, best quality, HD, C4D, chiaroscuro, 8K --s 750 --ar 3:4 --style scenic --niji 5`

![](img/6e26e948e0a8d953752d5a6215e52e83.png)

#### （2）生成连续的动作：`N panels with continuous doing`

我们输入 `4 panels with continuous dancing + 描述词` 可以得到跳舞的一系列动作角色图。

描述词：`4 panels with continuous dancing, blind box style, cute little girl, full body, looking at camera, bubble mart, octane rendering, super detailed, edge lighting, chibi, best quality, HD, C4D, chiaroscuro, 8K --s 750 --ar 3:4 --style scenic --niji 5`

![](img/401beec485ec96fc0c9d2ed4b9e9bbaf.png)

#### （3）生成多视角动作：`character sheet`

当我们输入 `character sheet with multiful pose and expressions + 描述词` 可以得到多视角动作，多种姿势和表情的角色图。

描述词：`character sheet with multiful pose and expressions,blind box style, cute little girl, full body, looking at camera, bubble mart, octane rendering, super detailed, edge lighting, chibi, best quality, HD, C4D, chiaroscuro, 8K --s 750 --ar 3:4 --style expressive --niji 5`

![](img/343a4312fa8cf038672809d918af9682.png)

### 4. `Emoji` 表情法

`Emoji` 代表着表情包，`Express sheet` 是各种表情的组合，通过 MJ 的 `emoji + expression sheet`，可以轻松制作角色一系列表情包。

描述词：`3D emoji,expression sheet,blind box style, cute little girl, full body, looking at camera, bubble mart, octane rendering, super detailed, edge lighting, chibi, best quality, HD, C4D, chiaroscuro, 8K --s 750 --ar 3:4 --style expressive --niji 5`

![](img/454c2cfb005af38104b46757f5a3727b.png)

更多的表情包类型大家可以在应用中去找。

### 5. `Shoot` 镜头法

这个方法可以一次出图，多张像电影分镜一样的连续画面。

介绍万能公式：`魔法前缀` + 关键词组 + 图像比例 `9:16`

#### （1）开头：描述词

`children’s illustration 儿童插图`

`random film stills of dreamWorks animation 梦工厂动画随机电影剧照`

`Cartoon animation charactor 卡通动画人物`

#### （2）中间：描述词

中间任何位置加入 `ContinuousShooting 连续性拍摄` 或 `Sequence shot 序列镜头` 或 `front side back three view 前侧后三视图` 或者三者全加，出分镜的概率 100%

#### （3）结尾：`-- ar 9:16`

描述词：`Animation style, cartoon animated characters, random DreamWorks Animation movie stills, front and back three views, romantic scenes, cute Chinese girls, full-body 3D art, Chinese style, romantic soft focus and elegant light style, sandara tang, oshare kei, learning 3D game art , fisheye lens, syudio light background focal length 35mm f1.4 --s 400 --ar 9:16 --style scenic --niji 5`

![](img/3718f2fdaf0103d17a80f92347af9923.png)

### 总结：

#### 方法一：垫图法

`图片 URL` + 关键词 + `--iw 2`

#### 方法二：`Seed` 值法

`替换关键词` + `--seed 值`

#### 方法三：`Panels` 动作法

生成不同的动作：`N panels with different poses` 生成不同的动作

生成连续的动作：`N panels with continuous doing`

生成多视角动作：`character sheet`

#### 方法四：`Emoji` 表情法

`emoji` + `expression sheet 各种表情的组合`

#### 方法五：`Shoot` 镜头法

`children’s illustration 儿童插图/random film stills of dreamWorks animation 梦工厂动画随机电影剧照/Cartoon animation charactor 卡通动画人物` + `ContinuousShooting 连续性拍摄/Sequence shot 序列镜头/front side back three view 前侧后三视图` + `--ar 9:16`

## 二、结语

Midjourney 控制角色一致性，要想达成自己期望的效果，不单单是一个段描述词就可以得到想要的效果，需要不断调整后缀参数，修改描述词，逐步完成效果。
