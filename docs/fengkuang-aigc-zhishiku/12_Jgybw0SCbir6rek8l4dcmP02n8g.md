
# 一键生成多种风格图片，自动填充关键词插件 Dynamic Prompt

> 来源：[https://keenchase.feishu.cn/docx/MMqxdN8H2o0Vv6xn7SbctuSunMf](https://keenchase.feishu.cn/docx/MMqxdN8H2o0Vv6xn7SbctuSunMf)

各位同学们好，我是jeff，这期给大家出一个Stable Diffusion“动态提示词”的插件教学，这个插件能帮我们提高绘图效率，它可以一键生成多种不同风格的图片和自动填写关键词，话不多说，直接开干~

![](img/91e262b78eae821becd615fc2e127a8b.png)

## 一、Dynamic Prompts安装方法

### 提醒一下：如果你是安装了秋叶大神最新版本的一键启动器，那么你就不需要再安装这个插件了，秋叶大神已经预置好了，可以直接跳过这个安装教程。就是这个：

![](img/b640c1ad571b6b359011131bf4d04cd2.png)

如果你没有用一键启动器，或者没有这个插件，那我们就手动安装下，这里给大家介绍两种不同的安装方法：

#### 1、“web UI”扩展安装

点击“扩展”，然后点击“加载扩展列表”，等待一会~

![](img/1374b5fbfd76b1d4436294fc20760314.png)

然后按快捷键`ctrl+f`搜索动态提示词，注意，如果你是英文版的就搜索“Dynamic Prompts”，然后点击安装就行了。

回到已安装列表就可以看到已经安装好了。

![](img/31fe6ab57773913a51ce21c73dee7388.png)

#### 2、从网址安装

第二种是从网址安装，还是从扩展点进去，然后点击“从网址安装”，

输入这个网址：`https://github.com/adieyal/sd-dynamic-prompts`

点击安装就好了！

![](img/ae646e51e464a16cf7153d799deb2a14.png)

安装好之后回到“已安装”列表就可以看到了。还有第三种方法，本地安装，我个人觉得没什么必要，就不说了。

## 二、Dynamic Prompts使用方法

### 1、启动动态提示词（插件测试）

首先我们先测试下这个插件的效果，跟着我一起做一遍就懂了：

#### 1.1 先选择一个人物模型，因为本次我们测试的效果是一个女孩。（关于模型的下载的方法我已经说过几次了，可以C站上下载，或者是用我们收录的一些优质模型，模型在知识学院，可以联系我下获取）

#### 1.2 然后输入我准备好的关键词：

正向：

`A girl with {black|pink|blue|white} hair in a white dress`

> 一个穿着白裙子，一头黑|粉|蓝|白}发的女孩

反向：

```
(worst quality:2), (low quality:2), (normal quality:2), lowres, normal quality, skin spots, acnes, skin blemishes, age spot, (ugly:1.33), (duplicate:1.33), (morbid:1.21), (mutilated:1.21), (tranny:1.33), mutated hands, (poorly drawn hands:1.5), (bad anatomy:1.21), (bad proportions:1.33), extra limbs, (disfigured:1.33), (more than 2 nipples:1.33), (missing arms:1.33), (extra legs:1.33), (fused fingers:1.61), (too many fingers:1.61), (unclear eyes:1.33), lowers, bad hands, extra digit, (futa:1.1), bad hands, bad anatomy, missing fingers, fewer digits, low quality, signature, watermark, missing fingers, missing arms, watermark, fat, ugly, mutilated, tranny, extra fingers, odd eye, poorly drawn face, bad proportions, deformed, morbid, extra arms, text, error, bad anatomy\, blurry, jpeg artifacts, cropped,
```

这个正向关键词的格式形式大家可能没见过，我们等下说，先看完。

![](img/b09b35c8203160e0018089fcf75d3d60.png)

然后图片尺寸调到512*768，总批次设置成4。

![](img/6150a98b8f8d26a5c2c313e52e703208.png)

#### 1.3 点击启用动态提示词，一定不要忘了，其他参数先不调整。

![](img/857d75ce9fdafb916cbd4e9d560d2e6e.png)

#### 1.4 点击生成出图，可以看到，4张人物的头发颜色不一样，连衣裙都是白色的，图片是吻合我们的关键词的。

看到这个效果大家应该心里明白这个插件是怎么用的了吧，就如图所示：

![](img/a8558e6ecb5e35bbae70074e2fcf94de.png)

那么问题来了，我们输入的是“一个穿着白裙子，一头`{黑|粉|蓝|白}`发的女孩”，既然是4张不同的参数，怎么会出现两张白色的头发，没有粉色的呢？答案是这个生成是随机性的，会根据的在中括号`{}`内填写的关键词随机挑选生成的。

那么我们就用同样的关键词，把总批次调整成8张来看看：

![](img/44df225ece3e224d5c35fc116e7e92dc.png)

这里我一共用这个参数跑了4次，我们关键词所写的`{黑|粉|蓝|白}发`都出现了，只是随机性不一样，这里大家应该都看懂了吧。

![](img/422ea01b4a58ef3083109eb5f996f4bb.png)

### 2、组合生成

上面介绍了这个动态提示词的玩法，下面我们继续介绍它的功能“组合生成”，这个插件的用法是把写的关键词都跑一遍，上面演示的是关键词随机生成的，那这个功能就是固定生成的，往下看：

#### 2.1 回到动态提示词这里，勾选组合生成，然后把总批次调回到1

![](img/34e0d4b19ba765b724963b3b3d739920.png)

然后点击生成就会把这几个关键词全部跑一张出来，如图所示：

2.2 这个参数是控制生图的数量的，比如你加了很多这个插件专属的提示词，可能一共会有100张组合图片，那你可以调整这个来控制生成的图片数量，比如30张。下面的组合批次也是一个道理，默认是0，组合批次默认是1，代表不控制图片数量。

![](img/7d71362aa34f9ffb1d180cf4baf9baec.png)

### 3、进阶操作

#### 3.1 我们把正向关键词加入艺术家的名字，你们复制就好：

正向关键词：

`A girl with {black|pink|blue|white} hair in a white dress by {2$$leonid afremov|Vincent william van gogh|picasso}`

艺术家分别是：列昂尼德·阿夫雷夫|文森特·威廉·梵高|毕加索

这些符号格式什么的大家先别急，先跟着跑图试试，我们点击生成（注意你们跟着教程做的话，其他参数应该是紧跟上面的参数的，我没做改变）

![](img/a4d175ff72fa77bc390e763c84f8093d.png)

这里生成的时间会有点久，等待一下，然后我们看上面生成的图，他的头发和艺术风格都是跟着关键词控制的（关于这个艺术家风格在这个模型跑的风格不是很还原，这是模型的原因，强调一下是教程演示！）

#### 3.2 大家可能没看懂为什么突然会生成这么多图片了，我们的总批次还是1。解释下还是组合生成的效果，然后这个图片数量是受到我们关键词`{2$$leonid afremov|Vincent william van gogh|picasso}`影响的。

这个`2$$`的意思是，把`{leonid afremov|Vincent william van gogh|picasso}`随机挑选2个艺术家（关键词）进行组合成一张图，我们是写的三个艺术家，它就会把所有能2个互相组合的关键词生成一次，那么计算下来生成出来的图片就有这么多张。这里可能大家有点懵逼，我们来看看生成几张图片的参数：

![](img/3d86d4b236a5107c48794355b3767985.png)

看看这三张图片后面的参数，都是不同的2个艺术家组合。

分别是：

`A girl with black hair in a white dress by Vincent william van gogh,picasso`

> 一个黑头发穿白裙子的女孩来自文森特·威廉·梵高，毕加索

`A girl with black hair in a white dress by picasso,leonid afremov`

> 一个黑头发穿白裙子的女孩来自毕加索、列昂尼德·阿夫雷夫

`A girl with black hair in a white dress by picasso,Vincent william van gogh`

> 一个黑头发穿白裙子的女孩来自毕加索，文森特·威廉·梵高

提醒下：也可以输入10个艺术家然后组合生成参数写成`{5$$+是艺术家名字}`，这个命令就是在10个艺术家中随机组合5个，ok，如果还没看懂就多看几遍。

### 4、格式详解

上面的操作大家体验了的话应该都明白这个插件的功能了，然后就是这个插件的书写格式了，这个很重要，不然命令会生效。下面还是拿这组关键词来演示

#### 4.1 基础格式：`A girl with {black|pink|blue|white} hair in a white dress`

大括号`{}`+`|`关键词和“|”这个符号可以不用空格，这个`{}`内的内容最好不要变化很大，意思是如果是头发颜色，你就不要写衣服的颜色了，衣服的颜色就在后面写，比如：

`A girl with {black|pink|blue|white} hair in a {black|pink|blue|white} dress`

> 一个留着{黑|粉|蓝|白}头发的女孩，穿着{黑|粉|蓝|白}裙子

![](img/84b5acd68c4a2501adbf4a19d6bf861f.png)

这个符号怎么按出来的？来看看我的键盘：

![](img/2c5a66fb2a825f15a7023c189f58574e.png)

#### 4.2 进阶格式：

`A girl with {{long|short}black|pink|blue|white} hair in a white dress`

可以用“套娃”模式，括号里面加括号，这组关键词生成的效果就是 长头发+头发颜色 短头发+头发颜色

大家可以自行测试，这里我拿这组关键词跑图：

![](img/c22c9b9d69cc1f8851dc15b509661b42.png)

#### 4.3 随机组合格式：

`{2$$leonid afremov|Vincent william van gogh|picasso}`

在`{}`内关键词前面加上随机组成的数量+`$$`两个美元符号，比如：

`{2$$leonid afremov|Vincent william van gogh|picasso}`

`{3$$leonid afremov|Vincent william van gogh|picasso...}`

`{4$$leonid afremov|Vincent william van gogh|picasso...}`

### 5、数据集玩法

如果我们想要生成的效果内容非常多，那么我们在这里输入关键词就会很多，可能上百种，不利于观察，那么这个插件是提供数据集管理的，来看看这个功能吧。

#### 5.1 点击这个“wildcards manger”，就可以看到这个数据列表。

#### 5.2 这里的插件内置了一些数据集，可以调用，这里我演示操作流程，选中这个“devilkkw”：

![](img/8e1a7b5ce768614d40bd0491ab529d05.png)

#### 5.3 点击这个按钮，等待一下，或者多点几次。

![](img/50b1b4d26a013eba5d4e987b63bd489f.png)

#### 5.4 然后这个数据集就被调出来了。

![](img/9c3a7013113482e5ed731fefcbb946c4.png)

#### 5.5 把这个展开，然后看到旁边有这个文件地址，把这个地址复制下来就行了，然后添加到关键词中。

![](img/699db4818cd14961d89558c9c5ddccee.png)

#### 5.6 我们拿自己写的关键词来测试这个数据集。怎么创建自己的数据集呢，看下面：

找到这个文件夹，看我的路径，别找错了。这里的两个文件夹就是上面调用插件内置的数据集。

![](img/cbd76278f86ffb314eabdf7581f53bc0.png)

#### 5.7 我们创建自己的数据集，在这个文件夹下创建一个数据集文件夹，然后起个名字，比如：`yanchuan`（最好是英文名）

#### 5.8 建一个文本文件（TXT），起名`Hair color`。

![](img/ab09201d53de07b3145935ba5b435331.png)

#### 5.9 填写颜色的关键词，一定要保存一下，格式如下：

#### 5.10 回到web-ui，点击这个刷新按钮，然后就会显示出来。

![](img/c0bbdf41361001f3538ca7fbc44305ea.png)

#### 5.11 然后把这个地址复制一下。

![](img/3607b36bf8fba293212932970e888807.png)

#### 5.12 把刚刚的`{}`内容替换成这个数据集文件地址，点击生图。

![](img/2a4ab3d5e47a0ce345ceb3c4ae74d419.png)

#### 5.13 大功告成，真丫的好用！

## 三、Prompt Magic（魔法提示词）

### 3.1 这个很简单，我们把组合生成关掉，然后勾选“魔法提示词”，这个功能是给丰富你的关键词，具体看下面的操作。

![](img/94743a518199961066154489210bdf76.png)

### 3.2 我们只输入“a girl”这个关键词，然后点击生成。

![](img/94fb2d4e2e606b18cb91c03920b941a0.png)

第一次生成后台会下载一个模型，下载时间根据大家的网速决定。

![](img/774da926e51b46fefdc81172a93c99e2.png)

### 3.3 看下面的数据，他自动生成关键词，关键词的长度可以调整，这个功能可以很好的给我们参考灵感。

![](img/926513af19417913ec12885ed278f9c2.png)

### 3.4 这里就是模型的切换。

### 3.5 这个功能的意思就是随机在关键词上加权重，也是自动的，大家尝试就知道了。

其他的小功能就不介绍了，用的最多的还是动态提示词和魔法提示词。

## 四、写在最后

以上就是这篇Dynamic Prompts插件的详细教程啦，它能大幅度提高我们的出图效率，很值得安装试试，那这篇到此结束，后续我还会更新其他好用的插件哦。

如果觉得对你有帮助，3连（点赞、收藏、分享）一下吧，大家的支持才是我的动力，祝大家生活愉快😊

Stable Diffusion一键安装包和教程素材我放在群公告中了，大家需要联系我获取。
