
# Midjourney Prompt分析第二期-产品、虚拟头像、汽车

> 来源：https://bluefocus.feishu.cn/docx/GEpfdiIQhoZxaCxOv7Kckeslnfh
> 为了方便小伙伴们的学习和体验，我们找到了这个文档，以系列的方式，通过以内部分享的方式帮助大家练习。
> 关键词只是一个参考，最重要的还是大家需要理解MJ的算法逻辑和如何学习关键词的技巧。
> 大家可以复制关键词，然后理解，拆解，二创或者修改。
> 希望大家能够掌握写词的方法和技巧，可以帮助大家解决实际工作中创作的问题，完成MJ真正意义上的落地。
> 大家加油！

## 2.1 产品系列prompt分析（思路）

> 应用场景：香水/美妆/酒饮品等。
> 镜头+主体描述+形容词+万能词+输出定量

**主题设定——CHANEL香水**

![](img/c724d7731a87e707b7cebfb74bdf224c.png)

```text
close up shot,Chanel perfume, transparent bottle, natural, translucent, Water and sky are connected together, overlapping light and shadow.colorful,by Peter Lindbergh，canon EF 70-200mm,f/8,professional photo shoot，cinematic , photorealistic,ultra detailed,tone mapping,ray tracing,HDR,16K --ar 3:4
```

特写镜头，香奈儿香水，透明瓶，自然，半透明，水和天空连接在一起，光和影重叠，色彩丰富，由Peter Lindbergh，canon EF 70-200mm，f/8，专业拍摄，电影，逼真，超细节，色调映射，光线追踪，HDR，16K `--ar 3:4`

> Prompt解读
> 这组Prompt的核心关键词是“香奈儿香水”。
> 产品Prompt一般可以给到一个特写镜头，或者产品拍摄镜头等，在主体描述的时可以尽量的把产品细节描述清楚，包括环境，这样MJ可以给到相对准确的输出。这里设计师同样是归纳到万能词，也就是媒介词汇部分，也是因为这种作品专业度比较高，加入著名摄影师的风格，可以让画面更加的有质感和识别度。对于其他部分基本就是专业的一些摄影参数和设备参数来辅助出图的质感。
> 对于产品摄影，我在这里还需要跟大家分享两点：
> 1 摄影是一个对专业知识和相关参数要求门槛比较高的门类，所以大家在做此类输出的时候，需要多了解和学习一些相关的知识来补充自身的认知不足，可以让自己的作品更加多有质感和专业度。
> 2 在商业摄影这块，目前还没有完美的可以和实际产品达到融合的可能，都会因为MJ本身的稳定性导致或多或少的问题，所以这块完全可以落地执行的话，还需要看一下官方对MJ的迭代，以及AIGC先驱们的方法尝试。

没有摄影参数也可以出来有效果的作品，只是随机性可能会大一些，附赠Prompt：

```text
"in-studio shooting, product-view, prada perfume, (Perfume on the the water. with a magical and luxurious
quality.) ( colorful light) (magazine photography) advertorial,photorealistic ultra detailed.ray tracing, HDR, 16K --ar 3:4"
```

## 2.2 虚拟头像系列prompt分析（思路）

> 应用场景：卡通形象/虚拟头像等。
> （真人照片叠图链接）+镜头+主体描述+形容词+万能词+输出定量

```text
（真人照片叠图链接）super cute girl IP by popmart， brown curly hair, Scenes in winter, pastel color, Mockup, fine luster, clean background,3D render， Soft focus ，oc, blender，best quality， --ar 3:4
```

（真人照片叠图链接）超级可爱的女孩 IP by popmart, 棕色卷发, 冬天的场景, 粉色, 模型, 精致的光泽, 干净的背景,3D渲染, 柔和的焦点，oc, blender，最佳质量， `--ar 3:4`

> Prompt解读
> 这组Prompt的核心 关键词是“Popmart风格头像”。
> 这组Prompt是生成自己的虚拟头像，是在自己照片的基础之上进行叠图，来完成保证个人特征相似度的情况，完成一个卡通风格的头像作品。
> 这一版会比较简单，当然也会很通用，你可以用这个架构来生成不同人物的头像，无所谓男生女生，你要做的就是把性别，妆容，场景和颜色替换成自己想要的即可。
> 当然还有细节版本prompt，是在简约版基础上增加了细节关键词来进行约束：

```text
“（真人照片叠图链接） super cute girl IP by popmart, blind box toy, waist up portrait, Agate red hair, emerald
green eyes, colorful crystal earrings, and a shimmering pink princess dress, Pinkish background, Harajuku Style, chibi, Soft focus, Mockup, 3D render, oc, blender, best quality, --ar 3:4"
```

> 另外，还有一些叠图的技巧和方法，比如叠两张图，一张照片，一张风格图，这种情况你可以按照风格图的内容进行关键词的描述，就会产出一张不错的作品。

## 2.3 汽车摄影系列prompt分析（思路）

> 应用场景：汽车概念设计/品宣推广海报物料等。
> 镜头+主体描述+形容词+万能词+输出定量

![](img/a15001d48e1f676ee8a8082b46f61cb3.png)

### 1. 汽车如何在水天相接的视野中呈现

```text
diagonal composition，Maserati MC20，an image of a blue car floating in the sky, light background，dark silver and azure, electric, calm waters,in the style of object portraiture specialist, uniformly staged images,32k uhd,   ue5 --ar 3:4
```

对角线构图，玛莎拉蒂MC20，一辆蓝色汽车漂浮在天空中的图像，浅色背景，暗银色和天蓝色，电动，平静的水域，产品视觉拍摄专家的风格，均匀分级图像，32K UHD，UE5 `--AR 3:4`

![](img/a0c2578ee30fd74996f89be8e3dbf127.png)

> Prompt 解读
> 这组Promot的核心关键词是“汽车在水天相接的水面上”。
> 这组Prompt其实是比较难的，就这一个画面在实验过程中用了16组prompt，整整16组，实验时间在2个小时左右，其实叠图是相对简单的，毕竟这是一个研究的课题，所以还是要找到逻辑所在来呈现出来，就只能用关键词去刚图。
> 以前车圈广告的朋友在询问这种水天空间中的汽车视觉该怎么营造，所以很早就在尝试，我觉得这个是蛮有意思的，我也没办法一下子就做出来，需要花很多时间，现在你们就不需要了，基本上框架利用起来就可以。
> 当然其实在过程中有一些已经不错的产出，但是过于风景画，距离我的预期还是比较远，所以都被我熔炉重塑了。
> 分享几组给大家玩一下：

```text
"medium shot,forty-five degree angle, Maserati MC20, white car,The blue sky is connected to the lake, Minimalism and Futurism. Surrealism, automotive photography, photography,8k,shot on canon mirrorless camera, ultra detailed, photorealistic, high resolution,8k smooth --q 2 --ar 3:4 "
```

```text
"medium shot, forty-five degree angle, Maserati MC20, white car, Surrealism, The blue sky is connected to the lake, automotive photography, photography,8k,shot on canon mirrorless camera, ultra detailed, photorealistic,high resolution,8k smooth --q 2 --ar 3:4"
```

> 另外，我看很多人喜欢叠图，这本身就是个高效的方式，那用自己心仪的参考，再加上这一段咒语的话，效果如何呢？感兴趣的小伙伴可以试一下。

### 2. 汽车如何在户外空间呈现

```text
（wide-angle view,Bottom view,forty-five degree angle,）Maserati MC20，white/red car,(The blue sky is connected to the lake,)Minimalist-style futuristic urban architecture,Minimalism and Futurism.Surrealism, automotive photography,photography,shot on canon mirrorless camera, ultra detailed, photorealistic,high resolution,8k smooth --q 2 --ar 3:4
```

广角视图，底视图，四十五度角，玛莎拉蒂MC20，白色/红色汽车，（蓝天与湖水相连）极简主义风格的未来主义城市建筑，极简主义和未来主义，超现实主义，汽车摄影，摄影，用canon无反光镜相机拍摄，超精细，逼真，高分辨率，8K流畅 `--Q 2 --AR 3:4`

![](img/4acfec2c743f7db2374477ca4e80ae22.png)

> Prompt 解读
> 这组Prompt的核心关键词是“汽车在户外空间的呈现”。正常输出，没有花大多时间，每组1-2组尝试便可以生成。
> 这组Prompt其实还好，通过我们常用关键词的结构，在每一部分输入匹配的内容即可。
> 只是需要注意的是，汽车摄影相关的知识，比如什么相机，什么镜头，什么光圈，什么模式，甚至什么机位，还有一些专业术语，这个是需要大家积累和补充学习的东西。
> 其他基本上就是你更换视角构图，更换车的品牌和颜色，更换空问和风格，更换摄影参数，就会得到一些还不错的输出。
> 还有一个动态版本的画面，其实除了以上这些关键词知识点以外，只需要记住一个关键词“motion blur-动态模糊”，这个词不仅是可以用在车上，可以应用在很多场景，他会让你的一切画面动起来，不信大家可以试一下。
> 在这里附上动态模糊下车的关键词：

```text
medium shot, side angle, Maserati MC20, he blue car, Road between the City, motion blur, photo taken with Canon Eos R5 8k 45mp telephoto lens, 3D render, Vray, PBR car paint texture, hyper detailed, --ar 3:4
```

### 3. 汽车和模特如何在空间呈现

```text
medium long shot，full body，a woman walking to a mercedes in an advert，in the style of light maroon , Square.large-scale minimalist, grandeur of scale, polished concrete, panoramic scale,white background，electric, calm waters, uniformly staged images, contrived elegance, extravagant，32k uhd, ue5 --ar 3:4
```

中等长镜头，全身构图，一个女人走到广告中的奔驰车前，浅栗色的风格，广场。大规模的极简主义，宏伟的规模，抛光的混凝土，全景规模，白色背景，电力，平静的水面，统一的舞台形象，刻意的优雅，奢侈，32k uhd, ue5 `--ar 3:4`

![](img/41aee1c5ea01e195550ab49889c5f604.png)

Prompt 解读

这组Prompt的核心 关键词是“汽车和人在空间的呈现”。

当然，呈现出来的图我是进行了后期的，因为人物和车的一些细节是需要做优化的，所以用了phootoshop。

这组输出是有技巧的，那就是〝反向Tag输出”，这就用到了 前段时间MJ刚更新的参数指令`describe`，我找了一张自我感觉不错的车企官方媒体发布的海报，给到MJ进行反向输出，给到了我4条指令，然后我根据每条指令分别进行了生成，从中挑选一组方向相似的prompt，进行了格式上和关键词的优化和增补，进行重新输出。

这样操作不会有叠图的影子，又会比推导实验关键词高效的多。

这个技巧适用于任何作品的输出，你可以去尝试了。
