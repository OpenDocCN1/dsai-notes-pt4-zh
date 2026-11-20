
# 【MJ应用系列课程 第四讲】 Midjourney模型生成风格及seed值的使用

> 来源：[https://docs.feishu.cn/docx/B9bddKlkfonmScxhMJ6czpsznlu](https://docs.feishu.cn/docx/B9bddKlkfonmScxhMJ6czpsznlu)

![](img/7c8cb9926c7f2809299fff92e5ac29fb.png)

> Midjourney是一款强大的AI图像生成工具，它可以根据用户输入的文字或图片，快速地生成各种风格和主题的图像。Midjourney的图像生成过程中，有两个重要的参数会影响最终的效果，分别是模型和seed值。本文将介绍Midjourney中不同模型的特点和用法，以及如何利用seed值来控制图像的随机性和一致性。

## 一、Midjourney模型的特点和用法

Midjourney的模型是AI图像生成工具的核心，它们决定了生成图像的风格和质量。要查看和进行设置，你需要发送`/settings`指令，然后从下方的列表中进行调整。

![](img/4f3d985db0b17961e0ad0872e612c108.png)

[关于](https://learningprompt.wiki/docs/midjourney/mj-tutorial-basics/midjourney-basics-setting)Midjourney的基础设置可查看这篇文章：https://learningprompt.wiki/docs/midjourney/mj-tutorial-basics/midjourney-basics-setting

Midjourney目前提供了四种不同的模型，分别是MJ V1-V5、Niji、MJ Test和MJ Test Photo。每种模型都有自己的风格和优势，可以根据不同的创作需求和喜好进行选择。下面我们来逐一了解一下这些模型。

### 1、MJ V1-V5

这是Midjourney发布的不同版本的模型，从V1到V5，模型的质量和适应性都有所提升。V1-V3模型已经退出历史舞台，目前最常用的是V4和V5模型。

V4模型适合生成各种风格的艺术作品，具有较高的清晰度和细节，但也会出现一些奇怪或不自然的效果。

V5模型则是目前最先进的模型，它可以生成更接近真实照片的图像，具有更强的逼真感和真实感，但也需要更多的计算资源和时间。

这里使用同一组prompt来生成不同版本的图片：`/imagine (prompt:) An 18-year-old girl`

![](img/dc3ce37b9777e14c97f0492e1190bd2e.png)

### 2、Niji V4-V5

这是一种专注于生成动漫或二次元风格的图像的模型，它的特点是颜色鲜艳、线条清晰、表情丰富，能够快速生成高质量的动漫图像。Niji模型适合那些喜欢日本动漫或漫画风格的用户，也可以用来制作角色设计或插画等。Niji V4生成的图片在细节上还有些小问题，且光影效果也不太好；Niji V5则进一步优化，在细节上有很大提升，统一的光影使画面更加干净，在手部表现上更加连贯。

这里使用同一组prompt来生成不同版本的图片：`/imagine (prompt:) An 18-year-old girl`

![](img/40af5f4a48f07872e16a2010b45a033a.png)

### 3、MJ Test

这是一种早期的测试模型，它的成像风格介于卡通与照片之间，具有一定的艺术感和真实感。MJ Test模型可以用来生成一些抽象或概念性的图像，也可以用来尝试一些新奇或创意的想法。

依然使用`/imagine (prompt:) An 18-year-old girl`：

![](img/6cbc3f9b6768f40f0860b289b94ae398.png)

### 4、MJ Test Photo

这是另一种早期测试模型，它更接近真实照片的成像风格，具有更高的清晰度和细节。MJ Test Photo模型可以用来生成一些逼真或写实的图像，也可以用来模拟一些摄影技术或效果。

依然使用`/imagine (prompt:) An 18-year-old girl`：

![](img/901fbaf1827327befb4bf68c95bfd416.png)

目前最常用的模型主要是V4、V5、Niji V4、Niji V5，大家可以根据自己的创作需求和喜好选择不同模型进行使用。

大家在认识到Midjourney不同模型生成的风格之后，或许有生成连续性、一致性要求的图片，那我们应该如何去实现呢？接下来，就让我们一起来了解一下seed值的使用，来帮助我们生成系列图片。

需要注意的是：为了保证图片的连续性，在尝试的过程中我们要保证风格、质量等参数都不要变化，特别是模型版本，一定要使用相同的模型版本。

## 二、seed值的使用

### 1、什么是seed值

当你使用Midjourney生成图像时，你可能会发现，即使你使用相同的文字提示，每次生成的图像都会有所不同。这是因为Midjourney会为每个任务使用一个随机的数字作为seed值。seed值是一个影响生成图像的重要因素，它可以理解为随机算法的一个输入值，相同的seed值会产生相似的图像，不同的seed值会产生不同的图像。seed值可以影响图像的细节、颜色、形状等方面，但不会改变图像的主题和内容。seed值可以帮助用户控制生成图像的随机性，或者发现更多的可能性和变化。

例如，如果你想要生成一个女孩子站在太阳花下的图像，可以使用`/imagine (prompt:) Girl under the sunflower`来生成一张默认的图像，然后通过改变seed值来观察不同的效果，下面会在使用步骤中举例说明改变seed值后取得的效果。

![](img/a0a039338ae80745b776676f93979be7.png)

### 2、如何获取seed值

Midjourney默认是不会公开seed值的，因为它认为这样可以保持生成图像的创造性和惊喜性。但是如果你想要获取某个任务的seed值，可以在Discord上，在图片上添加信封表情（`:envelope:`）。这样Midjourney Bot就会将图片的seed值发送给你了，步骤如下：

+ 添加反应

+ 输入 `:envelope:`

![](img/ae6dd687d09de58ac8a48c0e3b62fffe.png)

+ 点击信封表情后会收到Midjourney Bot发送过来的消息

![](img/e9dc061c18907741e178cc8ffb9bd131.png)

+ 查看seed值，注意：复制加入prompt时要改写为“`--seed 3318598621`”

获取seed值后，就可以在其他图片中使用`--seed`参数来重现或修改原来的图像了。

### 3、如何使用--seed参数

如果你想要保持或改变生成图像的一致性，可以使用`--seed`参数来指定seed值。midjourney的seed值是一个64位的整数，它的范围是从0到2的64次方减1，即0到18446744073709551615。例如`--seed 3318598621`；

如果你不使用`--seed`参数，Midjourney则会每次使用一个不同的随机数作为seed值。使用`--seed`参数可以让你重现或修改之前生成过的图像。

例如，当我想要生成一个女孩子穿着漂亮的裙子的图像，并且保持女孩的外貌不变，只是更换裙子的颜色，可以按照上面的步骤获取它的seed值。

![](img/33815cb17a0ff4717881f7daabc3d63b.png)

然后加入对裙子颜色的描述（pink），输入：`/imagine (prompt:) The girl is wearing a prettypinkdress --seed 151241603`来生成一张修改后的图像。

![](img/19add57c87f1fe989ffa8e5708bb864b.png)

通过对比，我们可以发现前3张的构图基本一致，图片中女孩的特征也很相似，而裙子的颜色都变成了粉色。

由此可知，使用 Midjourney的seed值，并不能保证生成的图像与原图完全相同，只能确保 AI 的起点和大体方向不变。因此最终生成的图像仍然会有一些变化。

以上就是Midjourney不同模型生成风格及seed值的使用介绍啦，通过了解和使用这些不同的模型与参数，我们可以更好地控制和调整生成图像的一致性和变化。希望这篇文章能帮助你更好地利用Midjourney模型生成风格及seed值创造美妙图像。

> 人的差异产生于业余时间，业余时间能成就一个人，也能毁灭一个人。 -- 爱因斯坦
