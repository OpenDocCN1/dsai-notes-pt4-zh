
# 京东｜首个Joy模型诞生，全民生成Joy大片

> 来源：[https://aiyanxishe.feishu.cn/docx/SfhHdQzvCogTQ6xl2WrcQo0knkg](https://aiyanxishe.feishu.cn/docx/SfhHdQzvCogTQ6xl2WrcQo0knkg)

本文公开了首个Joy的LoRA模型，同时将带领大家利用Stable Diffusion生成高质量的Joy海报。文章将从SD的各个参数进行讲解，如模型放置、模型使用、提示词编辑、渲染器选择、算法选择等，帮助大家最终实现Joy的海报生成。

![](img/46c275105cf64109b3a461f155acf864.png)

接上一篇文章 “只要10秒，AI生成IP海报，解放双手”，这次是全网第一个“共享Joy模型”，真的赚到了。

经过这段时间无数次的探索、试错、实验，最终积累了很多训练经验，在不同IP角色的训练上实际上需要调试非常多的参数以及素材。本次成功完成了Joy的LoRA模型，虽然在泛化以及场景上未来还有着很多空间，但是本次的模型已经可以帮助大家完成大部分的运用场景。今天无论你是设计师，还是产品，还是运营，还是研发，你都将可以实现Joy的海报生成，接下来我将为大家讲一讲如何实现。

老样子先来看看实际的效果吧！！！

## 生成过程

![](img/4e6a479c84b45d41a09af69a6c2bb740.png)

## 模型生成海报

![](img/d9307fba755c9d8fafc6e4793ccc2ef3.png)

![](img/7398d79c678fbc7fdf6090242de733fd.png)

![](img/e4492adb6b55303da3cf992ad6fe3d85.png)

![](img/c9803285543cc683d9cd587b305b7b45.png)

![](img/eee00df88b11235cc997ab0b76c3b7ff.png)

![](img/0e73a1d6934f1d1e2244c2fd00889177.png)

接下来就来和大家讲一讲如何通过SD来生成Joy吧～

## 准备工作

在开始之前，请大家下载好本次生成需要使用到的模型。

1. 底模：`revAnimated_v122EOL.safetensors`。下载地址：[http://pan.jd.com/sharedInfo/A73A7A2071C859BFEEC946AFBC5707A7](http://pan.jd.com/sharedInfo/A73A7A2071C859BFEEC946AFBC5707A7) 或 https://civitai.com/（开源底模型）
2. VAE：`vae-ft-mse-840000-ema-pruned`。下载地址：[http://pan.jd.com/sharedInfo/A73A7A2071C859BFEEC946AFBC5707A7](http://pan.jd.com/sharedInfo/A73A7A2071C859BFEEC946AFBC5707A7) 或 https://civitai.com/（开源VAE，其他VAE也可以）
3. 京东Joy-LoRA模型：[下载链接](https://joyspace.jd.com/pages/1nfmIt90VC45a212542c)（⚠️申请下载需进行京东内部审核，非京东同学禁止下载或商用，以免被追法律责任）
4. 京东Joy-起手式（Prompt模板）：[下载链接](https://joyspace.jd.com/pages/1nfmIt90VC45a212542c)（⚠️申请下载需进行京东内部审核）

## 放置模型

1. 底模：`stable-diffusion-webui/models/Stable-diffusion`
2. VAE：`stable-diffusion-webui/models/VAE`
3. 京东Joy-LoRA模型：`stable-diffusion-webui/models/Lora`
4. 京东Joy-起手式：`stable-diffusion-webui`

## 打开SD

⚠️ 如果还不知道如何配置SD的同学可以看我之前的文章：“只要10秒，AI生成IP海报，解放双手”。

![](img/bdd1f8027e79e1ed8b9845a47e84fdad.png)

## 模型设置

![](img/00b6ca39100a17b084e446aef321ac6e.png)

1. 我们需要在Stable Diffusion模型中选择我们下载好的模型：`revAnimated_v122EOL`（当然这是我目前测试下来效果较好的底模，你也可以下载并使用别的底模进行尝试）
2. 在模型的VAE中选择：`vae-ft-mse-840000-ema-pruned`（这个VAE的饱和度比较好一些，其他的VAE会使画面较灰）

## Prompt使用

![](img/9f76c252976c92fa8a73ed300f9d268a.png)

本次我已将Joy的基础Prompt设置成了模板（Joy起手式），后续大家可以根据自己想要的画面进行调整。我会讲一下本次Prompt的基础构成。

1. 正向词：正向词中主要分为3类，触发词 + 提示词 + LoRA（越靠前的词汇权重就会越高）
   - Joy的触发词为`joy(ip)`：只有输入这个触发词，画面中才会出现joy的形象
   - 提示词：提示词中主要分为自然语言 + 单词描述。用自然语言来形容Joy具体在做什么（如：`joy ip is standing on the stone`、`jumping`、`sitting`等，建议使用进行时）；用单词来形容画面中其他需要的元素（如：`outdoors`、`day`、`grass`、`leaf`、`tree`、`flower`、`sunshine`等）
   - LoRA权重：权重经过我的测试，最高的效果是0.7，所以在Joy的LoRA模型上无脑设置0.7
2. 负向词：主要描述你不想要的内容（如：`lowres`、`bad anatomy`、`bad eyes`、`bad hands`等，如果在生成过程中出现了你不想要的内容也可以在后续继续补充）

## 设置采样方式、采样迭代步数、尺寸、生成批次&数量

![](img/26512a44bcbde3882f194d5e60e41878.png)

- 采样方式上我们选择：`DPM++ SDE Karras`
- 采样迭代步数：24-28之间（根据画面的复杂程度进行调试，如果画面元素较多则可以以4的倍数向上增加，需要注意在改变了步数以后会让画面产生变化）
- 尺寸：一般使用64的倍数。常用的尺寸如512*768（2:3）、768*512（3:2）、512*512（1:1）、768*1152（2:3）、1152*768（3:2），不限于这些尺寸，只是说这些尺寸下抽卡出现好画面的几率更高一些
- 生成批次、每批数量：在前期抽卡阶段（没有开高清修复阶段），可以把生成数量提高，来寻找喜欢的画面；在找到自己喜欢的画面后则需要设1去更精细化的抽卡

## 提示词相关性

![](img/42307ebb3025fea3fa91de3d93504f54.png)

1. 提示词相关性：3-5之间效果更佳，如果没有出现IP可能是因为场景权重过高导致IP消失，可以尝试降低相关性或减少场景描述。提示词相关性是影响画面元素的非常重要的参数之一（其他影响的因素还有迭代步数、重绘幅度以及尺寸）

## 抽卡环节（快乐时间来了）

我们只需要嗑着瓜子，看着电视或者做着别的事情，去挑一个动作、构图、元素、画面大致满意的底图（来作为后续打开高清的基础）。

![](img/b8bae2799db3543739282776d4091199.png)

## 开启高清修复（完成一幅Joy大作）

在我们选出我们满意的底图后，需要做的就是锁定我们的`seed`，打开高清修复，调整我们的重绘幅度（重绘幅度会影响画面变化的幅度，越低则变化越小，越高则变化越大）。

- 锁定我们的`seed`值。

![](img/9f1ad8723250da7a23272261eebd37d2.png)

- 将我们的放大算法改为：`R-ESRGAN 4x+`，调整重绘幅度0.1-0.7。

![](img/a8c02effecbe9d8e4071087a4e9f4ace.png)

- 保存我们想要的图。

![](img/99b003223c0856fbbe1f03c482690519.png)

如果你已经使用Joy模型并抽到了一些好的效果，可在评论区留言反馈，也希望和大家有更多的交流和学习，感谢大家支持哦！！

https://aiyanxishe.feishu.cn/sync/IaSadpoOOsopKab1ajKcz6z3nAb
