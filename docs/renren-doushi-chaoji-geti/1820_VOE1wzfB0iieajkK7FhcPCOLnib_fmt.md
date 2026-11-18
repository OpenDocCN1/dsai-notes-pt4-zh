
# 【第5期】ControlNet

> 来源：[https://xuqiwei1986.feishu.cn/docx/N1XWdq6xnovApSxBQd4cuNgHnof](https://xuqiwei1986.feishu.cn/docx/N1XWdq6xnovApSxBQd4cuNgHnof)

欢迎来到SD的终极教程，这是我们的第五节课

在SD里面，想要生成出来的图片最大程度的符合我们脑海里的画面

有三个最关键的因素

**1.关键词**

**2.Lora**

**3.ControlNet**

关键词和Lora我们已经讲过了，这节课我们就讲SD里最强大的功能——ControlNet

## 一、Controlnet有什么用

### 1.给老照片上色

![](img/7370ac398d4255b9be653ac21e609ca4.png)

![](img/c4940b32d2fe1cbdbd27d5042b860c39.png)

### 2.给线稿上色

![](img/64ea2bea8b0a6fef6dcb329fb5c71a59.png)

![](img/81ce7030a621b42c5d20e23f6917dfba.png)

### 3.控制人物姿势

![](img/54bd7779a4dba1812243ee89b7828d96.png)

![](img/32c9277c80c5318761cecef089943e82.png)

### 4.让照片动起来

![](img/b269261d9ec417882f3f9021791224ea.png)

![](img/831300a43f5bd38f72b46336873ff1a2.png)

### 5.有趣玩法

可以生成酷炫的赛博机车照、真人漫改

![](img/84be0c88c2d468742fc11b1468f98717.png)

![](img/de573a0de370613c4b5ac558842bab1a.png)

![](img/f40fe6452731be8677f7cfb0156689ed.png)

制作二次元头像

![](img/40e50b2bcda6e48aab98c55fa0bb63fd.png)

![](img/6436fe01deb70f0167a47110df220fea.png)

生成创意字

![](img/dc7e390193de9c1a3d87e11acaa13900.png)

![](img/3edb2b46d360097599492c1d905b3efc.png)

controlnet好玩的地方还有很多很多，例如恢复照片画质、制作艺术二维码等等等等

## 二、controlnet是什么

AI绘画最终要实现的目标——让出的图与我们脑海里想象的画面一致

但目前现状是：随机性太强

很多时候能不能出来一个好看的画面，只能通过大量的「抽卡」实现，以数量去对冲概率

这种情况下，如果能用好控制出图的三个最关键因素，能让「出图与我们想象的画面一致」概率更高

**1.提示词**

**2.Lora**

**3.ControlNet**

提示词的作用是奠定整个图的大致画面

Lora的作用是让图片主体符合我们的需求

ControNet的作用是精细化控制整体图片的元素——主体、背景、风格、形式等

提示词的用法我在SD的教程里讲过，这个更多的是需要平时的积累，用AB测试去知道每个词对图片产生的影响，从而养成提示词思维

而我们这篇文章主要讲的是ControlNet的用法，把17种模型都给你演示一遍，让你知道每一种模型有什么用，应该怎么用，从而真正掌握ControlNet

ControlNet就是你提供一张图片，然后选择一种采集方式，去生成一张新的图片

比如这张照片

![](img/8eef22b3802e4b2e00445aa0c6badb62.png)

可以选择采集图片中人物的骨架，从而在新的图片中，生成出一样姿势的人

![](img/76176ebe7718af198f8866ea063f7d6f.png)

![](img/f636e9a46818870c0e6a467355db0ea3.png)

也可以选择采集图片中的线稿，从而在新的图片中，生成一样线稿的画面

![](img/b2971a56fa8eed1fe2c7bc64d9e274b8.png)

![](img/edfee370a4cffc2407455b0502321e2d.png)

又或者选择采集图片中已有的风格，从而在新的图片中，生成一样风格的画面

![](img/d88140ce99b87ede3018d90096857a56.png)

![](img/b37141f96fbc0f39b0f4453c6e4af018.png)

用的时候不必拘束于哪一种模型更好，更重要的是你脑海里想要什么样的画面

多去尝试，也可以结合其他模型一起用，最终把图片变成你想要的画面就可以了

接下来文章会分为三个部分，会详细介绍controlnet的全部内容

1.安装controlnet

2.controlnet的具体使用流程

3.controlnet的分类讲解

## 三、安装controlnet

一般通过整合包安装的Stable Diffusion，都已经装了有ControlNet这个插件

我们只要更新插件，然后下载对应的模型就可以了

打开sd的启动页面

点击左边的版本管理，打开扩展页面

找到controlnet那一栏，点击更新就好了

![](img/556660f02684366498f4ff27b3e4db74.png)

接下来就是下载controlnet模型

17种模型我已经全部打包好放在网盘了

大家只要下载下来，放在对应的文件夹就可以

存放位置：`SD文件夹>models>controlnet`

![](img/a6069ae62212b5504877159cec7f6d2d.png)

## 四、controlnet的具体使用流程

Controlnet里面总共有17个模型，理论上来说就可以有17种用法

甚至可以两个或者多个Controlnet一起用，这样就有更多的玩法了

但不管怎样，它们的具体操作步骤都是差不多的

> 接下来我们先讲解controlnet的统一操作方法

> 再讲解controlnet的每一个模型的功能

就以指定人物姿势为例子，

看看怎么让我们生成出来的照片，摆出来这个同款芭蕾舞姿势

![](img/eb4130261e9802f6502c1773672295e5.png)

这里我们可以分为两个步骤：

+ 1.SD基础设置

+ 2.controlnet的设置

### 1.SD基础设置

SD的基础设置就是我们前面课程所讲的内容

包括选大模型、写关键词、参数设置、如果是想生成好看的真人图片，还可以加上Lora

#### 01.选大模型

首先是选一个大模型，我这里选的是一个写实的大模型

![](img/698780db5798b859c69a5471c4fbedd1.png)

#### 02.写关键词

关键词就是按照我们第二节课的关键词模板去写，再加上Lora模型

再把通用的负面关键词也复制进来

![](img/01955b88eaa7684eb34bf10bdd57727b.png)

#### 03.参数设置

迭代步数35步

采样方法：Euler

宽度和高度可以先不管，我们等一下再处理

![](img/ecc6ba220c23d49a2d82b745aacfe2c8.png)

这样SD的基础设置就完成了，接下来就到controlnet的设置

### 2.controlnet的使用

controlnet的使用方法就只有两步

+ 上传图片

+ 选模型

滑到SD最下面打开controlnet的面板，在空白的地方上传参考照片

![](img/64186dbb58bd18244f8622bb8d17a9ac.png)

把这里的 启用 和 完美像素模式 打开

如果电脑配置比较低，还可以打开这个低显存模式

![](img/14dc3dd0e7c93e842bbf57297d072bf4.png)

然后点击右边这个向上的小按钮，这样照片的尺寸就会自动同步到宽度和高度

![](img/f63cb436b7603d5aa475d40f1462f009.png)

如果发现同步过来的数值太大或者太小，我们可以按比例把这个数字除以2或者乘2

![](img/b262658ab87c9063f41befb565f2a33d.png)

再往下看到控制类型，意思就是 我们要控制生成图片的什么地方

现在我们要控制的是人物的姿势

那就选`openpose`

下面就会自动加载预处理器和模型

![](img/06eddb8f0bd145b27a7eda395fed7f1c.png)

简单来说，预处理器是用来识别我们上传的参考照片的，不同的预处理器识别出来的东西就不一样

模型就是将预处理器处理之后的结果加载到我们要生成的照片上面

点击爆炸按钮💥，就可以看到预处理之后的人物姿势线条

![](img/d1bb7405ab8554a40a494081cabf1394.png)

最后点击生成就可以了，小姐姐就会摆出和我们参考图一样的姿势

![](img/5e2e94ca49d7f981a5775e1d8ec92045.png)

![](img/49347343be36ed3d333c91dd9c03e0ae.png)

controlnet的不同功能的实现其实就是上传不一样的照片，然后选用不一样的预处理器和模型

一般情况下，预处理器和模型名字一样，是配套使用的

controlnet是辅助我们生成想要的照片的一个工具

我们的大模型和关键词也非常关键

要换照片风格一定要记得换大模型

接下来我们就看一下每一种预处理器和模型都有什么用

## 五、controlnet的分类讲解

我把17种模型，按照对照片的不同约束类型，分成了以下几类

有对人物姿势进行约束的，也有对照片里的线条进行约束的，还有对照片风格进行约束等等

现在我们就按照分类顺序，看看17种模型具体能干些什么

### 1.姿势约束

这一节讲的是`openpose`模型，主要控制生成照片人物的姿势

这里的姿势有身体姿势、表情、手指形态三个

可以只控制某一个或者两个，也可以三个一起控制

> 身体姿势

> 身体姿势+脸部表情

> 只有脸部表情

> 身体姿势+手指+表情

> 身体姿势+手指

![](img/f1de2a07a78c1b82fb3a352865fed549.png)

#### 01.控制身体姿势

一般情况下，在SD里面生成一张照片，照片人物的动作都是随机的

但controlnet可以让生成出来的人物摆出任何你想要的姿势

![](img/dc67708378c311d8f7bdeddc5984c56f.png)

![](img/fbfd21fcf761ff651e3d0d20dbae572f.png)

首先我们正常设置大模型和关键词

然后打开controlnet，上传自己想要生成的姿势照片

controlnet的模型选择：

预处理器：`openpose` 模型：`openpose`

![](img/a9ef8b3b07fa57b5ef2778bae582478e.png)

点击预处理的爆炸按钮就可以看到，模特的姿势被提取成了一个火柴人

里面的小圆点就是人体的重要关节节点

![](img/7d9425ec18924832e38755e88a855faa.png)

看看生成出来的照片，模特的姿势就几乎完全复刻出来了

![](img/d040ce298c3d86e59739093c2e280361.png)

#### 02.控制人物姿势和手指

除了识别人物整体的姿势以外，还可以识别手指的骨骼

这样在一定程度下就可以避免生成多手指或者缺少手指的照片

![](img/f444ac38d54d04064a1035a67eefabb4.png)

![](img/e9bb3bbe02589757153dc0afb3c69036.png)

具体的操作跟前面是一样的

只是预处理器的选择不同

controlnet的模型选择：

预处理器：`openpose_hand` 模型：`openpose`

![](img/b570ec03b4f329f8f8ec718e7a6222bf.png)

看看SD预处理之后的火柴人，在人体整体姿势的基础上，还多了线条和节点表示手指

![](img/6267795c9fbc2041e2194d427e0ff86a.png)

#### 03.控制人物表情

openpose除了控制人物的姿势，还可以控制人物的表情

> 但是用controlnet复刻人物表情比较适合放特写的大头照

> 这样识别出来的五官才会更加精确

> 相对应的也只能生成出来大头照

![](img/0b295ee708f2d4e99c174bafca02a6cc.png)

![](img/48ec80a2b1fa3ec7c339aad47ce58cb4.png)

这里我们又换了一个预处理器

controlnet的模型选择：

预处理器：`openpose_faceonly` 模型：`openpose`

![](img/9ce9842a032ba1b083845972af8d4b7f.png)

预处理之后就是把模特的脸型五官用点描出来

![](img/8a93bbef442c8f2e9371b5a92e009072.png)

看看生成出来的照片，脸型和五官在一定程度上都还原了

> 但是，如果你生成的照片用了Lora

> 再用controlnet控制表情可能会导致生成出来的照片跟Lora的人不太像

> 因为生成出来的照片的人物五官和脸型都被controlnet影响了

![](img/9722299ba94acf3f51e97ae3e1e016ff.png)

#### 04.全方面控制人物姿势

这里我们是把人物的整体姿势、手指、表情都复刻了

![](img/4e8e5b2f465c5e92b8f1982384746277.png)

![](img/fe632a93aefdd09234c13f3d70929558.png)

controlnet的模型选择：

预处理器：`openpose_full` 模型：`openpose`

![](img/ba208b0a4e8e1afdf7df212494c8b040.png)

看看处理后的照片就会有我们上面说到的所有东西

![](img/c2f231695801f217ee262d8c4ef1f815.png)

#### 05.自由编辑火柴人

有时候预处理器处理出来的火柴人可能会不太准确

又或者我们需要更加细致的去调节

这时候我们可以再安装一个插件，这样我们就可以自己去调节预处理之后的火柴人

![](img/f25dfa93b9436bcba1a819fca3427be0.png)

##### 安装插件的方法：

①在“扩展”里点击“可下载”页面

②点击“加载扩展列表”

③在搜索框里输入`openpose`

④安装`sd-webui-openpose-editor`

![](img/ffb13a482997f2b9ec552f89bcacf340.png)

⑤点击“已安装”页面

⑥点击“应用更改并重载前端”按钮

![](img/58b0afa6febd9a7244304df9fef374f2.png)

这样插件就安装好啦！

##### 插件的使用方法：

接着我们回到controlnet里面

点击预处理后的图像旁边的“编辑”按钮，就可以自行去编辑火柴人的节点

> 如果打开编辑按钮是空白的，那就先点击一下预处理之后的图片，再去编辑

![](img/2f4d4ddb8cb954b188f952cbb74c3e0e.png)

把鼠标放到圆形节点上面，就可以调整位置

调整好了之后，点击左上角的“发送姿势到controlnet”就可以啦

![](img/da57314b76b30aa4dfd661a049e77638.png)

这样通过自己的调节，把腿的节点拉长，就可以生成一个大长腿美女了

![](img/e24363bdc0ecdaa114ad8bb67f26546f.png)

![](img/f46872f8e48d02705b0f10c77ac4e459.png)

#### 06.小结

识别人体姿势有五个预处理器，在这里有一些我自己选预处理器的小技巧

在日常使用中，如果原图的手指骨骼比较清晰，可以用识别到手指的预处理器

如果识别出来的手指线条比较乱，自己调整也没调整好，那就只识别身体姿势

不然生成出来的照片手指反而更乱了

控制表情的最好用在生成近景特写图片，这样识别出来的才比较准确

![](img/1caefd09d73cea218092255ac3aeaffb.png)

### 2.线条约束

这一节会讲到`lineart`、`canny`、`softedge`、`scribble`、`mlsd`五种模型

它们都是用来提取画面的线稿，再用线稿生成新的照片

![](img/2c02546fb24c5f8f145feb7e03a812a7.png)

#### 01.lineart

`lineart`是一个专门提取线稿的模型，可以针对不同类型的图片进行不同的处理

点击选择`Lineart`，预处理器和模型就会自动切换

![](img/6501d86414d8f996b7b465ffdcf7635c.png)

点开预处理器

里面的各种模型可以识别不同图片的线稿

> 动漫：`lineart_anime` 或 `lineart_anime_denoise`

> 素描:`lineart_coarse`

> 写实：`lineart_realistic`

> 黑白线稿：`lineart_standard`

![](img/1f6f00416e5d3d1db4ba79b57aa93398.png)

##### ①动漫照片

提取动漫的线稿，再重新上色

![](img/8bd519ac646013a63b908e76bb90482a.png)

![](img/edb01ba6bb584db6770f69e356d3da68.png)

首先处理动漫照片记得要换二次元的大模型

然后关键词可以写一些质量词，然后描述一下照片里面有什么东西

另外需要注意的是，图片的分辨率大小要设置的和原先的比例一样



# ControlNet 各控制模型使用指南

不然照片会自动裁剪放大

## lineart 系列

`controlnet`的模型选择：

预处理器：`lineart_anime` 或 `lineart_anime_denoise` 模型：`lineart`

![](img/0adbf7e29d151dfb28bd6ac8e88d86e8.png)

可以看一下两个预处理器出来的效果，选一个自己比较喜欢的就可以

### `lineart_anime`

![](img/998349bdbb4c158ddb06e0fa4e40d2bd.png)

![](img/9d66227af0517616bf6873596674669e.png)

### `lineart_anime_denoise`

![](img/4d1bd4751d279e8fd1de7a616be6bd13.png)

![](img/9ced719364b4647c9a0293fc3744aa24.png)

### ②素描照片

![](img/1bd2fbc65078106d55771dc0f2708138.png)

![](img/ea868bf1effa8ae9c05f073fd4edf68b.png)

`controlnet`的模型选择：

预处理器：`lineart_coarse` 模型：`lineart`

![](img/73e23585243b982b1f0c2b97afe4571a.png)

### ③写实照片

可以上传自己的照片，提取出线稿，然后生成自己的二次元头像

![](img/ceeffdd29255aa8f3e08c023fc535f68.png)

![](img/c70479108f522509c346d219321d2b0e.png)

要生成二次元照片，一定要先换成合适的大模型

`controlnet`的模型选择：

预处理器：`lineart_realistic` 模型：`lineart`

因为真人照片换成二次元在五官比例上会不太匹配

这时候我们就要适当把`controlnet`的权重降低到0.6左右

![](img/6782df4b772fd05881c7646d3e175300.png)

还可以将真实的照片转换成真实的照片，生成一个长的很像的人

![](img/a6aa55b5b7795c59f231b6836b878615.png)

![](img/09a3d264bdac178c8e6a2796c3f298c0.png)

### ④黑白线稿

![](img/17e4347d31e64976a98a0c8cda09e9dd.png)

![](img/46a73a9b2d15c29de018b90593cb8060.png)

![](img/311abb1eb2f8daca025bf79aa52a0949.png)

`controlnet`的模型选择：

预处理器：`lineart_standard` 模型：`lineart`

![](img/a4d2aefb92bea0237609ca99f4754538.png)

## 02.canny

`canny`可以识别到画面的最多的线条，这样就可以最大程度的还原照片

但是比较适合二次元照片

![](img/8960fb5821f081c221c0a803f98c2330.png)

![](img/58d90c96e72a52d41e544e51efd0ee88.png)

`controlnet`的模型选择：

预处理器：`canny` 模型：`canny`

![](img/a96dc4d145765766fa5e20aeffe6817e.png)

![](img/f8829330ba1aecd6f0a3d4f68eac5669.png)

![](img/135f174fbc0d523d897d905b467974b7.png)

## 03.softedge

`softedge`只能识别图片大概的轮廓细节，线条比较柔和，这样给`SD`发挥的空间就比较大

![](img/4889278c669df37041634e6d181640d4.png)

![](img/c6e3d90f9e8b0c1d5479f3f184af9103.png)

`controlnet`的模型选择：

预处理器：`softedge_pidient` 模型：`softedge`

![](img/53548b1a0df65c6ece73d64dc51c97b8.png)

![](img/38ecd2ba15f431a84cbdaf4bc0976f63.png)

![](img/d5a43bf2ac23733f831edd862e337830.png)

## 04.mlsd

这个模型只能识别直线，所以只适合拿来做房子的设计

![](img/8d338d9591a910832731d074d90b1fc8.png)

![](img/f80afdb76574f722b4000ba30478b25d.png)

`controlnet`的模型选择：

预处理器：`mlsd` 模型：`mlsd`

![](img/1f7fa8e827cd95ead09d29840d869db0.png)

看看预处理出来的图，都只有直线

绿色植物和瓶子这些有弧度的线条都会被忽略掉

![](img/606e67565faf6346b8006127ea7b84e8.png)

## 05.scribble

这一个功能和图生图的涂鸦功能一样

可以将自己随便画的东西放进去，通过输入关键词得到有着一样线条的照片

![](img/01d90de2631839ff1ba460818f4277d8.png)

![](img/b9ec83bfba89d4db1ea7918fdc297899.png)

`controlnet`的模型选择：

预处理器：`invert` 模型：`scribble`

![](img/4119cff5e291372311944972e7033947.png)

## 06.小结

识别线稿的模型有很多，如果你不知道什么情况下用哪个好，可以参考一下我的选择

1.想最大程度还原照片：`canny`

2.只想控制构图，给`SD`更多可以变化的地方：`softedge`、`scribble`

3.真人、素描等照片：`lineart`

4.建筑物装修：`mlsd`

## 3.空间深度约束——depth

能够很好的复刻房子线条，而且物品的距离镜头的前后顺序比较清晰

![](img/6f8fd002865c39627114c7ec307e62d6.png)

![](img/e5fa0e6a9140769aa97c559146908764.png)

`controlnet`的模型选择：

预处理器：`depth_leres++` 模型：`depth`

![](img/219d3c9168a0a6b1ff08f8ec77274176.png)

## 4.物品种类约束——seg

`segmentation`，语义分割模型

它可以将照片上不同种类的东西，用不同的颜色表示出来

这样我们新生成的图片里的元素就会跟原图一模一样

![](img/b7d711559dabd2e86471b7afc9f63a63.png)

![](img/acaf9d596118f9c5b1a1c4d58013cf66.png)

`controlnet`的模型选择：

预处理器：`seg_ofade20k` 模型：`seg`

![](img/4b61ad155707ed43dd7f2434bd6471c0.png)

可以把`seg`色块图下载下来，自己到`ps`或者其他修图软件改照片物体的颜色

这样`SD`就会根据颜色生成出来特定的某样东西

> 网盘链接里有一份文档，可以看到不同的颜色代表什么物品

![](img/e7a2cb63e87e3705044a8f07997b9ba0.png)

![](img/7521a7d9d578a75c99ac1bfc0f0cb147.png)

例如我可以在`ps`上面用代表人的颜色画上人的形状

这样生成的照片就在原来的基础上多了一个人

![](img/b275c23ac656aaf90c697126bd738e98.png)

![](img/8a0d35aaa820aae1654e08f7075d8dae.png)

### 小结

看到这里建筑物装修已经有三种模型可以处理了

1.只还原整体的结构：`mlsd`

2.还原物品的先后关系：`depth`

3.比较好的还原原图有的物品，想自己后期编辑色块，改变室内装修结构：`seg`

`depth`和`seg`除了用在建筑上，还可以用在人物照片

## 5.风格约束

这一节会讲到`shuffle`、`reference`、`normal`、`t2ia`、`IP-Adapter`五个模型

![](img/5d1e89560938f85ff564bde70b6c3349.png)

### 01.shuffle

`shuffle`是将参考图的颜色融合到一起，然后用融合后的颜色生成新的图片

下面第一张是原图，其他三张分别是由水墨画、油画、赛博朋克风格的图片转移过来的画风

![](img/2fb44a5f512e0b36e943c3110f1852a8.png)

![](img/6147bb45e67d3731b6b0e5400a80c586.png)

![](img/6a0fc67ce79beb5d78883d5012a7ad70.png)

![](img/52aa2259fdd1d48ec06a08d246707fd0.png)

`controlnet`的模型选择：

预处理器：`shuffle` 模型：`shuffle`

> 用`shuffle`可能会影响自己原图的形状，可以稍微调整一下“引导介入时机”的参数

> 设置在0.2~0.3之间就差不多

> 先生成出来大体的形状再去改变画风

> 或者用两个`controlnet`：一个固定线稿，一个影响画风

![](img/e0cdb5e885316c05c0d04a01a1675fdb.png)

### 02.reference

可以很好的还原原图的角色

#### ①让照片动起来

让坐着的小狗跑起来

![](img/3002dc6c3e92dd45e8842b585854d125.png)

![](img/ebc61df39e877b8a44ad559a00163b6d.png)

选择写实的大模型

然后在关键词里面输入：一只狗在草地上快乐地奔跑

![](img/317e7b80b22fd69ac0e0e75add79a3f9.png)

`controlnet`的模型选择：

预处理器：`reference` 不用模型

![](img/d10254d9c53717442ee9a9dbfd4a3f36.png)

#### ②还原角色

给`SD`一张人物角色图，它会根据人物的五官、发型还原这个人物

![](img/ce06565fed3e459b1f78dfdaf0e51c63.png)

![](img/4e91f58aee6445c3aa62eb936e404223.png)

具体操作方法和上面是一样的

关键词还可以加上情绪的描述，或者不一样的服装发型

![](img/10db7230b517e0488ca17c84805e8086.png)

![](img/56e37bbc0f758a8c30cddf5fd06c7030.png)

### 03.Normal

这个模型可以参考原图的明暗关系，并且还原原图的姿势

![](img/8b936eb6c8fe60e98dac3bb7cc184d0b.png)

![](img/f3e45c7cc87dd2f238050af6036af082.png)

`controlnet`的模型选择：

预处理器：`normal_bae` 模型：`normalbae`

![](img/2ee590e2e7cd955caa82e7afda4478e4.png)

### 04.t2ia

`t2ia`这个模型比较特殊，不同的预处理器要用到不同的模型

它的主要功能有3个

1.将原图的颜色模糊成马赛克再重新生成图片

2.提取素描的线稿，生成真人照片（这个不好用，直接用`lineart`就行）

3.参考原图风格，生成相似风格的照片

参考原图的颜色，将原图模糊成马赛克，再生成图片

![](img/d010e845c1cd59c0709c982ebb6fde25.png)

![](img/8d49c741a6f3497810e07dd977296cd0.png)

`controlnet`的模型选择：

预处理器：`t2ia_color_grid` 模型：`t2iadapter_color`

![](img/78553def5c464ae5cf14ac65976be09f.png)

### 05.IP-Adapter

参考原图的全部内容

不管是风格、还是人物形象，都可以得到还原

![](img/bc24141ef4f620b5b2ce39ac4350b94d.png)

![](img/4fabf800abe9e12c253269b764bfa7a6.png)

`controlnet`的模型选择：

预处理器：`ip-adpter_clip` 模型：`ip-adpter_sd15`

![](img/d86a0ff5987ad591e007138eae9a9a9b.png)

## 6.重绘

重绘分为`inpaint`和`recolor`

### 01.Inpaint

和图生图里的局部重绘差不多，但是`inpaint`可以将重绘的地方跟原图融合的更好一点

#### ①消除图片信息

![](img/d56d049173c6418018771f4425f31a28.png)

![](img/48f04bc03945bdae4cd1e4b84ccb8511.png)

![](img/07483f504e8935a35c4bac0aaf758df9.png)

关键词填写照片背景的描述词

`controlnet`的模型选择：

预处理器：`inpaint_global_harmoniou`s` 模型：`inpaint`

> `inpaint_global_harmoniou`s`预处理器是整张图进行重绘

> 重绘之后整体融合比较好，但是重绘之后的图片色调会改变

>

> `inpaint_only`只重绘涂黑的地方

为了重绘之后的图片更像原图，可以把控制权重拉满

![](img/e43c532d42690f3d9b93fe6e4726d4b5.png)

可以看看在图生图局部重绘出来的效果，是不如`inpaint`的

> 这张图是我跑了十几张图之后选的比较好的一张

![](img/c58fb6c3fe843dea37b26189b99b245e.png)

#### ②给人物换衣服

![](img/dc044abe21c4c50464c4c3c65474b2d0.png)

![](img/bf0722c4591afd185db66199d1f229d4.png)

操作方法和上面一样，只是关键词的描述不一样

### 02.Recolor

`recolor`模型可以给照片重新上色

#### ①给黑白老照片上色

![](img/10e43985df6edee9040c13781317e19d.png)

![](img/848b8f8c28cf3a6d6e0b56d246533297.png)

`controlnet`的模型选择：

预处理器：`recolor_luminance` 模型：`ioclad_sd15_recolor`

![](img/aae35e01f35efb0c5a532221ae3c1343.png)

#### ②给人物头发、衣服换颜色

![](img/a7c227583487cb6227c22bc4276cdda5.png)

![](img/d49593c0f3ac70bd7d72fc0d542a9e13.png)

只要在关键词里面输入对应的“绿色头发”就可以了

`controlnet`的模型选择：

预处理器：`recolor_luminance` 模型：`ioclad_sd15_recolor`

![](img/1be93e5966879cb826710e25eb1e19c2.png)

这里需要注意一下，我们要换颜色的地方最好是 深色换深色，浅色换浅色

比如黑色的头发可以换成棕色、绿色，但是最好不要换成白色这种浅颜色，不然出来的效果就没那么好

## 7.特效——ip2p

给照片加特效

这个功能目前来说没有太多的实际用处，只能拿来玩一下

比如让房子变成冬天、让房子着火

![](img/d7c3fb89d2b245200bd547092c4138b0.png)

![](img/0cb7f0a41d4d2411c76ba0b222541351.png)

![](img/f86f741288a7aefa1616b0c528bc9e2e.png)

这里需要输入的关键词比较特殊

需要在关键词里面输入：`make it....` （让它变成...）

比如让它变成冬天，就输入：`make it winter`

`controlnet`的模型选择：

预处理器：无 模型：`ip2p`

![](img/9ee6a4bb9f0921b0c0ddfd66ccfb1156.png)

## 8.加照片细节——tile

`tile`模型的玩法非常多

### 01.恢复画质

![](img/fa144398eaae3b681e9d30fde4becc43.png)

![](img/4de31334826e8496bc519f7a530db8c1.png)

`controlnet`的模型选择：

预处理器：`tile_resample` 模型：`tile`

![](img/bd48f645f4e7edd90eefc8fe82930bc8.png)

但是这个恢复画质的方法可能不太适合真人

因为`tile`模型的工作原理是先忽略掉照片的一些细节，再加上一些细节

这些`SD`自己加上去的细节可能会导致生成出来的照片不像原图

![](img/31ee73ee26a540a815e5f9807290037f.png)

![](img/2053f385b38681cb6a3e7d7bf6e19559.png)



## 02.给图片加工

生成炫酷的赛博机车图

![](img/a73f769579dddaf9259c32c5271b2835.png)

![](img/bda8b51e3280b4e73156d12480189f62.png)

## 03.真人变动漫

![](img/395ebfe43025119fd2c806925c64f023.png)

![](img/ece4bfc3cf744183d8d5f12b654f26b8.png)

## 04.动漫变真人

![](img/f5e798f9e8c237ddc60d35fd2e7888d0.png)

![](img/0e4296602060fc9f89cd86ff4c7426a8.png)

![](img/f5f2999ebead6cfe57ae0e625552f38b.png)

![](img/94893669a93f0ac8765d7fa6db9fa6c6.png)

## 05.光影艺术字

![](img/d4814fc3b97e965720ee9d04c5b0d49a.png)

![](img/6ebaf9009d5ae47454238a5def8aca7b.png)

![](img/7d4afa8276d406d744dd194284a4b4e0.png)

## 四、结尾

这是整个controlnet的思维导图

希望可以帮助到大家更好的理清controlnet的功能用处

目前用SD很多时候还是在“抽卡”

只是controlnet可以帮助我们提高成功的概率

并不是说用上controlnet就一定能出来自己满意的照片

模型关键词还有一些参数一样要反复的调整

但随着AI技术的不断迭代升级，未来还会有新的模型、更新的技术

最终AI绘画或许就可以实现——让出的图与我们脑海里想象的画面一致
