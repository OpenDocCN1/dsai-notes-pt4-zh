
# 2.【炼丹秘籍】Stable Diffusion 咒语语法和参数介绍

> 来源：[https://keenchase.feishu.cn/docx/NwWcdDW9co3yK4xyMvdc9bcRnff](https://keenchase.feishu.cn/docx/NwWcdDW9co3yK4xyMvdc9bcRnff)

从玩闭源的Midjourney转向玩开源的Stable Diffusion后，发现二者还是挺不一样的。简单来说，Stable Diffusion就像PhotoShop，小白面对一大面的参数面板根本无从下手，但老司机可以画出照片级的效果；Midjourney就像美图秀秀，小白上手就会用，简单的prompts也可以画出很棒的效果，老司机可以通过叠加prompts画出更好的图片，但是拓展性和对画面的控制度没那么强。

作为使用Stable Diffusion的一名小白魔法师，一开始看别人的咒语完全看不懂，只知道抄别人的prompts改改单词。后来查阅一些资料后，对prompts语法和参数有了一点概念，整理了一些网上的资料分享给大家。

## Tag书写方式

1. 分隔：不同的关键词tag之间，需要使用英文逗号,分隔，逗号前后有空格或者换行是不碍事的。ex：`1girl,loli,long hair,low twintails`（1个女孩，loli，长发，低双马尾）

1. 混合：`WebUi` 使用 `|` 分隔多个关键词，实现混合多个要素，注意混合是同等比例混合，同时混。ex: `1girl,red|blue hair, long hair`（1个女孩，红色与蓝色头发混合，长发）

1. 增强/减弱：有两种写法

1. 第一种 `(提示词:权重数值)`：数值从0.1~100，默认状态是1,低于1就是减弱，大于1就是加强ex: `,(loli:1.21),(one girl:1.21),(cat ears:1.1),(flower hairpin:0.9)`

1. 第二种 `(((提示词)))`，每套一层()括号增强1.1倍,每套一层[]减弱1.1倍。也就是套两层是1.1*1.1=1.21倍，套三层是1.331倍，套4层是1.4641倍。ex: `((loli)),((one girl)),(cat ears),[flower hairpin]`和第一种写法等价。

1. 渐变：比较简单的理解时，先按某种关键词生成，然后再此基础上向某个方向变化。`[关键词1:关键词2:数字]`，数字大于1理解为第X步前为关键词1，第X步后变成关键词2，数字小于1理解为总步数的百分之X前为关键词1，之后变成关键词2。ex：`a girl with very long [white:yellow:16] hair` 等价为：开始 `a girl with very long white hair`，16步之后`a girl with very long yellow hair`。`a girl with very long [white:yellow:0.5] hair` 等价为：开始 `a girl with very long white hair`，50%步之后`a girl with very long yellow hair`

1. 交替：轮流使用关键词

1. ex：`[cow|horse] in a field`比如这就是个牛马的混合物，如果你写的更长比如`[cow|horse|cat|dog] in a field`就是先朝着像牛努力，再朝着像马努力，再向着猫努力，再向着狗努力，再向着马努力

## Tag书写示例

建议按类似这样的格式书写提示词

```
画质词>>这个一般比较固定，无非是，杰作，最高画质，分辨率超级大之类的
风格词艺术风格词>>比如是照片还是插画还是动画
图片的主题>>比如这个画的主体是一个女孩，还是一只猫，是儿童还是萝莉还是少女，是猫娘还是犬娘还是福瑞，是白领还是学生
他们的外表>>注意整体和细节都是从上到下描述，比如发型、发色、衣服、头部、颈部、手臂、胸部、腹部、屁股、腿部、脚部...
他们的情绪>>表述表情
他们的姿势>>基础动作（站，坐，跑，走，蹲，趴，跪），头动作（歪头，仰头，低头），手动作（手在拢头发，放在胸前 ，举手），腰动作（弯腰，跨坐，鸭子坐，鞠躬），腿动作（交叉站，二郎腿，M形开腿，盘腿，跪坐），复合动作（战斗姿态，JOJO立，背对背站）
图片的背景>>室内，室外，树林，沙滩，星空下，太阳下，天气如何
杂项>>比如NSFW，眼睛描绘详细
```

将不同的分类的词，通过换行区分开，方便自己随时调整

```
(masterpiece:1.331), best quality,illustration,(1girl),(deep pink hair:1.331), (wavy hair:1.21),(disheveled hair:1.331), messy hair, long bangs, hairs between eyes,(white hair:1.331), multicolored hair,(white bloomers:1.46),(open clothes),beautiful detailed eyes,purple|red eyes),expressionless,sitting,dark background, moonlight, ,flower_petals,city,full_moon
```

于是我们得到这样一张图

## tag书写要点

1. 虽然大家都管这个叫释放魔法，但真不是越长的魔咒（提示词）生成的图片越厉害，请尽量将关键词控制在75个（100个）以内。

1. 越关键的词，越往前放。

1. 相似的同类，放在一起。

1. 只写必要的关键词。

## 反向提示词 Negative prompt

一组常用的反向提示词案例：

```
lowres,bad anatomy,bad hands,text,error,missing fingers,extra digit,fewer digits,cropped,worst quality,low quality,normal quality,jpeg artifacts,signature,watermark,username,blurry,missing arms,long neck,Humpbacked,missing limb,too many fingers,mutated,poorly drawn,out of frame,bad hands,owres,unclear eyes,poorly drawn,cloned face,bad face
```

## 采样迭代步数 Sampling Steps

AI绘画的原理用人话说就是，先随机出一个噪声图片，然后一步步的调整图片，向你的提示词 `Prompt`靠拢。`Sampling Steps`就是告诉AI，这样的步骤应该进行多少次。步骤越多，每一步移动也就越小越精确。同时也成比例增加生成图像所需要的时间。大部分采样器超过50步后意义就不大。下图是同一个图从1step到20step，不同step时图像的变化。

## 采样方法 Sampling method

使用哪种采样器，人话就是让AI用什么算法。这里介绍一些常用的采样器：

1. `Euler a` ：富有创造力，不同步数可以生产出不同的图片。超过30~40步基本就没什么增益了。

1. `Euler`：最最常见基础的算法，最简单的，也是最快的。

1. `DDIM`：收敛快，一般20步就差不多了。

1. `LMS`：eular的延伸算法，相对更稳定一点，30步就比较稳定了

1. `PLMS`：再改进一点LMS

1. `DPM2`：`DDIM`的一种改进版，它的速度大约是 `DDIM` 的两倍

## 生成批次 `Batch count/n_iter`

同样的配置，循环跑几次

## 每批数量 `Batch size`

同时生成多少个图像。增加这个值可以并行运行，但你也需要更多的显卡显存，具体可以自己看着任务管理器里的显存占用显示，自己调。基本`512X512`的图，`SD1.4`模型，`Euler a`，4G显存可以并行2张，8G显存可以并行8张。每点一次生成按钮，生成的图像总数=生成批次 X 每批数量。

## 提示词相关性 `CFG Scale`

图像与你的提示的匹配程度。增加这个值将导致图像更接近你的提示,但过高会让图像色彩过于饱和（你可以自己试试看）太高后在一定程度上降低了图像质量。可以适当增加采样步骤来抵消画质的劣化。一般在5~15之间为好，7，9，12是3个常见的设置值。

## 宽度 X 高度 `Width X Height`

单位是像素，适当增加尺寸，AI会试图填充更多的细节进来。非常小的尺寸（低于`256X256`），会让AI没地方发挥，会导致图像质量下降。非常高的尺寸（大于`1024X1024`），会让AI乱发挥，会导致图像质量下降。增加尺寸需要更大的显存。`4GB`显存最大应该是`1280X1280`（极限）。如果你确实想生成高分辨率图像，请使用`Hires. fix`功能。

## 随机种子 `Seed`

前边说过，AI绘画的原理是，先随机出一个噪声图片因为计算机世界里不存在真随机保持种子不变，同一模型和后端，保持所有参数一致的情况下，相同的种子可以多次生成（几乎）相同的图像。如果你用某个种子在某`tag`下生成了很棒的图，保持种子数不变，而稍微改一点点tag，增减一点细节，一般得到的图也会不错。

PS：不同型号的显卡即使参数与模型完全一致，也可能会生成完全不同的图。`10XX`和`16XX`系显卡基本每种型号都会是不同的结果，`20XX`系和`30XX`系基本都可以完美复现图片。PPS：这里不是`10XX`系显卡不适合AI作画的意思，只是你可能看网友的参数图很棒，你想照搬一下结果却大不一样。PPPS：某些模型比如`anything3.0`因为模型过于混沌，图像复现性能一样很差。

## 面部修复

使用模型，对生成图片的人物面部（主要是三次元真人）进行修复，让人脸更像真人的人脸，具体设定在【设置】- 【面部修复】1. 基本就是2种模型 `CodeFormer` 和 `GFPGAN` ，至于那个更好，这个不好说，这个看模型的，建议都试试。2. `CodeFormer`权重参数;为0时效果最大;为1时效果最小，建议从0.5开始，左右尝试，找到自己喜欢的设置。3. 倒不是说二次元图就不能用面部修复了，一定程度上也能提升二次元面部的作画质量。

## 可平铺(`Tiling`)

一句话：生成可以往左右上下连续拼接的图像。

## 超分辨率 `Hires. fix`

`txt2img` 在高分辨率下（`1024X1024`）会生成非常怪异的图像。而此插件这使得AI先在较低的分辨率下部分渲染你的图片，再通过算法提高图片到高分辨率，然后在高分辨率下再添加细节。

+ 放大算法：如果不知道选什么，一般无脑选“ `ESRGAN_4x` ”

+ 重绘幅度：放大后修改细节的程度，从0到1，数值越大，AI创意就越多，也就越偏离原图。

+ `Upscale by` ：放大倍数，在原有宽度和长度上放大几倍，注意这个拉高需要更高的显存的。
