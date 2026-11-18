
# ControlNet 各控制模型使用说明

## 线稿提取模型

### 01. 动漫照片

提取动漫的线稿，再重新上色

![](img/0b4c8f3b3e8a57e56e170930e92549c5.png)

![](img/5c8e6187511fdf515b1344789d377400.png)

首先处理动漫照片记得要换 **二次元的大模型**

然后关键词可以写一些质量词，然后描述一下照片里面有什么东西

另外需要注意的是，图片的分辨率大小要设置的和原先的比例一样

不然照片会自动裁剪放大

`controlnet` 的模型选择：

预处理器：`lineart_anime` 或 `lineart_anime_denoise`，模型：`lineart`

![](img/3f9c6de4521c9730cc0a460abbd99baf.png)

可以看一下两个预处理器出来的效果，选一个自己比较喜欢的就可以

`lineart_anime`

![](img/709fb537b170738e2dad01df5247b8f7.png)

![](img/9c8b453a7c58fe9abb7d56d64fd36378.png)

`lineart_anime_denoise`

![](img/9c0e67e1cfa3fc03363428e13a186165.png)

![](img/716f487d021ff60ba5c1debe1f9341d4.png)

### 02. 素描照片

![](img/db25b505505f09aa6a842e3f1143c692.png)

![](img/d14af5162e9ce180fa0dfbe57b735a5f.png)

`controlnet` 的模型选择：

预处理器：`lineart_coarse`，模型：`lineart`

![](img/2d2e08a5eb26e01eeb2dfc65eb0f170d.png)

### 03. 写实照片

可以上传自己的照片，提取出线稿，然后生成自己的二次元头像

![](img/d304a68d00f15ab9d5b1e65202a79d3c.png)

![](img/465fcd404d237ad1db9ea2c545bcfc63.png)

要生成二次元照片，一定要先换成合适的大模型

`controlnet` 的模型选择：

预处理器：`lineart_realistic`，模型：`lineart`

因为真人照片换成二次元在五官比例上会不太匹配

这时候我们就要适当把 `controlnet` 的权重降低

![](img/ce684d25f8e26f87ed52682685c190f3.png)

还可以将真实的照片转换成真实的照片，生成一个长的很像的人

![](img/965416fb1cbf599d9fcc209bb2d14a5e.png)

![](img/dee12ef11675a7a9dd477183dce90471.png)

### 04. 黑白线稿

![](img/53430ffd90f63e69a203b80e380f5685.png)

`controlnet` 的模型选择：

预处理器：`lineart_standard`，模型：`lineart`

![](img/6a1dec807e250c8f5af2166d11516819.png)

## Canny

`canny` 可以识别到画面的最多的线条，这样就可以最大程度的还原照片

但是比较适合二次元照片

![](img/d25928276036dc7704e579f1bcb462ec.png)

![](img/fcc7f85bfc1d2f03644505b0642098e7.png)

`controlnet` 的模型选择：

预处理器：`canny`，模型：`canny`

![](img/eb346d68bd62b17a3ea67b1f971f89ab.png)

![](img/b3dbfb8644f89428d50f7efea3480038.png)

![](img/dfa874248c63859141ae62128276180e.png)

## SoftEdge

`softedge` 只能识别图片大概的轮廓细节，线条比较柔和，这样给 `SD` 发挥的空间就比较大

![](img/d64629762c77a7a93379eaf2d8e4febf.png)

![](img/b69450153a80a294092b524d962a0389.png)

`controlnet` 的模型选择：

预处理器：`softedge_pidient`，模型：`softedge`

![](img/a1c6394ab81a5e219c10cc73a4cb3889.png)

![](img/d45c7ae96be0bb22c10098b48fd49fb7.png)

![](img/29489a5e18de05e85241ede819eab997.png)

## MLSD

这个模型只能识别直线，所以只适合拿来做房子的设计

![](img/b6472127c093a0a696d443e6bd26a2e5.png)

![](img/8fb109f6e2184b31c77fbd6f2dc1783f.png)

`controlnet` 的模型选择：

预处理器：`mlsd`，模型：`mlsd`

![](img/e3e83ac901924d2817842147309c32d0.png)

看看预处理出来的图，都只有直线

绿色植物和瓶子这些有弧度的线条都会被忽略掉

![](img/44b886b0e7f37d3bc91dc0f9d0b4193c.png)

## Scribble

这一个功能和图生图的涂鸦功能一样

可以将自己随便画的东西放进去，通过输入关键词得到有着一样线条的照片

![](img/70d493faee2f46c0cb7f6488f1d08b4f.png)

![](img/99b830cb766045db31fa47f0c239001a.png)

`controlnet` 的模型选择：

预处理器：`invert`，模型：`scribble`

![](img/9a946a366dd59ae2762c683a1864c2fa.png)

## 小结

识别线稿的模型有很多，如果你不知道什么情况下用哪个好，可以参考一下我的选择

1. 想最大程度还原照片：`canny`

2. 只想控制构图，给 `SD` 更多可以变化的地方：`softedge`

3. 真人、素描等照片：`lineart`

4. 建筑物装修：`mlsd`

## 空间深度约束——Depth

能够很好的复刻房子线条，而且物品的距离镜头的前后顺序比较清晰

![](img/8a933fbf4274f6cd708ef28ae1865826.png)

![](img/df07530340edcdf938bcd79045c0a0ab.png)

`controlnet` 的模型选择：

预处理器：`depth_leres++`，模型：`depth`

![](img/6a66799f337c7e3bfb048d85859f3aea.png)

## 物品种类约束——Seg

识别图片不一样的东西，就用不同的颜色表示

![](img/630d2fe29b19d1c8e25f2be1907e9594.png)

![](img/6eb37f709030d514b5eba27137a7ea59.png)

`controlnet` 的模型选择：

预处理器：`seg_ofade20k`，模型：`seg`

![](img/e40bb1f036d5c9187e6d3c1e5c7d61a0.png)

可以把 `seg` 色块图下载下来，自己到 `ps` 或者其他修图软件改照片物体的颜色

这样 `SD` 就会根据颜色生成出来特定的某样东西

> 网盘链接里有一份文档，可以看到不同的颜色代表什么物品

![](img/b69a836a9267afecb33108be6b1db9ef.png)

![](img/b8a0948826be2b58b54573882c7e0923.png)

## 小结

看到这里建筑物装修已经有三种模型可以处理了

1. 只还原整体的结构：`mlsd`

2. 还原物品的先后关系：`depth`

3. 比较好的还原原图有的物品，想自己后期编辑色块，改变室内装修结构：`seg`

`depth` 和 `seg` 除了用在建筑上，还可以用在人物照片

## 风格约束

这一节会讲到 `shuffle`、`reference`、`normal`、`t2ia` 四个模型

![](img/fa24d16bbeea70167781f343a83ee6e7.png)

### Shuffle

将其他图片的画风转移到自己的照片上

下面第一张是原图，其他三张分别是由水墨画、油画、赛博朋克风格的图片转移过来的画风

![](img/b6f96daa85aac4d79ff23b832d22ea18.png)

![](img/c9b0bf0f6c810f9203f58c5c31edee6c.png)

首先先用大模型和关键词生成一张自己喜欢的图片

固定随机数种子

然后打开 `controlnet` 将别的照片画风转移到自己的照片

`controlnet` 的模型选择：

预处理器：`shuffle`，模型：`shuffle`

> 用 `shuffle` 可能会影响自己原图的形状，可以稍微调整一下“引导介入时机”的参数

> 设置在 0.2~0.3 之间就差不多

> 生成出来大体的形状再去改变画风

> 或者用两个 `controlnet`：一个固定线稿，一个影响画风

![](img/f578b0c996056ab8ca149adf0e84b490.png)

### Reference

可以很好的还原原图的角色

#### 01. 让照片动起来

让坐着的小狗跑起来

![](img/00be00131cd401f32083039d969e017b.png)

选择写实的大模型

然后在关键词里面输入：一只狗在草地上快乐地奔跑

![](img/3aaaa46c06612bb21de3a921044522f0.png)

`controlnet` 的模型选择：

预处理器：`reference`，不用模型

![](img/bc1540731924a0a2919fdfad97f537c9.png)

#### 02. 还原角色

给 `SD` 一张人物角色图，它会根据人物的五官、发型还原这个人物

![](img/4b46d07fc15b469852158013dbf2eff4.png)

![](img/48baa5eb7293a1bb81dbd08a0de14bf5.png)

具体操作方法和上面是一样的

关键词还可以加上情绪的描述，或者不一样的服装发型

![](img/0dbc76b947d5b0cc6422304db1f1c969.png)

![](img/fd3de7b7b48bd100ae7e7820d7ad9c99.png)

### Normal

这个模型可以参考原图的明暗关系，并且还原原图的姿势

![](img/9f0af40afe23cbf6fce6c1970c556055.png)

![](img/e745dcd5548bf4947ed06b9e475ca4e1.png)

`controlnet` 的模型选择：

预处理器：`normal_bae`，模型：`normalbae`

![](img/7c495244e3a0b70cd5229c336cf3f35e.png)

### T2IA

`t2ia` 这个模型比较特殊，不同的预处理器要用到不同的模型

它的主要功能有 3 个

1. 将原图的颜色模糊成马赛克再重新生成图片

2. 提取素描的线稿，生成真人照片（这个不好用，直接用 `lineart` 就行）

3. 参考原图风格，生成相似风格的照片

参考原图的颜色，将原图模糊成马赛克，再生成图片

![](img/309efe6350723e2ec7d6a4a8cb8e39a8.png)

![](img/895c4dc2d5e7072339e359f25e011eb2.png)

`controlnet` 的模型选择：

预处理器：`t2ia_color_grid`，模型：`t2iadapter_color`

![](img/c3eb6af65acd8b4afe070d677170ed24.png)

## 重绘——Inpaint

和图生图里的局部重绘差不多，但是 `inpaint` 可以将重绘的地方跟原图融合的更好一点

### 1. 消除图片信息

![](img/4ba6665e805ec4f38af6f75cc6196ab3.png)

![](img/b55c138af9ac16e5102be1b222c9fa1b.png)

![](img/af773b59c4f83d2eee430c51e7ef0a1b.png)

关键词填写照片背景的描述词

`controlnet` 的模型选择：

预处理器：`inpaint_global_harmonious`，模型：`inpaint`

> `inpaint_global_harmonious` 预处理器是整张图进行重绘

> 重绘之后整体融合比较好，但是重绘之后的图片色调会改变

>

> `inpaint_only` 只重绘涂黑的地方

为了重绘之后的图片更像原图，可以把控制权重拉满

![](img/47d6c248cdc30417604d3221e02c40a3.png)

可以看看在图生图局部重绘出来的效果，是不如 `inpaint` 的

> 这张图是我跑了十几张图之后选的比较好的一张

![](img/878cdb80e36266bd6b68f03e0b06a310.png)

### 2. 给人物换衣服

![](img/d621786130d4dfe6f7c19dcd68ddf0bc.png)

操作方法和上面一样，只是关键词的描述不一样

## 特效——IP2P

给照片加特效

这个功能目前来说没有太多的实际用处，只能拿来玩一下

比如让房子变成冬天、让房子着火

![](img/ce9e157fb590936dd6f844aec476d064.png)

![](img/3793c2cb3307419c1995f711e665bd91.png)

![](img/ab31f6dc7793894321c99b99551b6a85.png)

这里需要输入的关键词比较特殊

需要在关键词里面输入：`make it....` （让它变成...）

比如让它变成冬天，就输入：`make it winter`

`controlnet` 的模型选择：

预处理器：无，模型：`ip2p`

![](img/a716908b95fd13de9fa83bedf4bb5169.png)

## 加照片细节——Tile

`tile` 模型的玩法有很多

### 1. 恢复画质

![](img/16e956651dd6202c8dea3fbe1cad5653.png)

![](img/5817ecce1710a68654a679b6bea62837.png)

`controlnet` 的模型选择：

预处理器：`tile_resample`，模型：`tile`

![](img/3ac328b4eb11bcb22b41e793e261d7af.png)

但是这个恢复画质的方法可能不太适合真人

因为 `tile` 模型的工作原理是先忽略掉照片的一些细节，再加上一些细节

这些 `SD` 自己加上去的细节可能会导致生成出来的照片不像原图

![](img/dd7ee477c365752da808dcc360089394.png)

![](img/dd4eed85d0d11a4401385b9f057b2628.png)

### 2. 涂鸦

将自己画的图给 `SD` 加工

![](img/b9e42b039940872f743ab00dcf006da6.png)

![](img/5f4a0cf02adf9c90b2aee4585568906d.png)

### 3. 真人变动漫

![](img/773df3995c155e3924f0877717af7d48.png)

![](img/1624a9ed26f86ef2b6060d28f21d2f80.png)

### 4. 动漫变真人

![](img/739bcdf17b0d1725224982a88e4d1576.png)

![](img/1ce75726713994fbce60bfe25835a51a.png)

![](img/1715b61bc09584d2e69d1f2ae38c8114.png)

![](img/42cc619bdf6e93fe4e92ec67c3a35c7a.png)

## 结尾

这是整个 `controlnet` 的思维导图

希望可以帮助到大家更好的理清 `controlnet` 的功能用处

目前用 `SD` 很多时候还是在“抽卡”

只是 `controlnet` 可以帮助我们提高成功的概率

并不是说用上 `controlnet` 就一定能出来自己满意的照片

模型关键词还有一些参数一样要反复的调整

但随着 AI 技术的不断迭代升级，未来还会有新的模型、更新的技术

最终 AI 绘画或许就可以实现——让出的图与我们脑海里想象的画面一致

如果你对 AI 感兴趣的话，可以关注我的 👸 号：吴东子 AI，也欢迎分享给你身边想学 AI 技术的朋友

里面有我之前发过的所有文章，之后也会持续给大家更新实用的 AI 干货教程

我是吴东子，用奶奶都能听懂的方式，分享可以落地实操的干货，我们下篇文章再见！

网盘链接：`https://pan.baidu.com/s/1EFXv7dgWu2vIH86gwSpfkQ?pwd=4dxj`

提取码：`4dxj`

吴东子 AI 账号简介：`https://ry5hwpuf7b.feishu.cn/wiki/space/7283841978071072772?ccm_open_type=lark_wiki_spaceLink`
