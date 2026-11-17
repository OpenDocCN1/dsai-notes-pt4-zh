
# 看大厂如何落地标准化的AIGC工作流

> 来源：[https://jalicvoygc0.feishu.cn/docx/NVDbduGZIo9m1BxcZT4cRyFInHf](https://jalicvoygc0.feishu.cn/docx/NVDbduGZIo9m1BxcZT4cRyFInHf)

**工作场景：**互联网运营、市场营销、活动策划

**节约时长：**10hrs+

**网络：**魔法

**寻找网络友好的AI工具：**[🤖AI工具导航](https://jalicvoygc0.feishu.cn/wiki/PgzAwa3XoiEB5xk3XJNcUORvnpi) [🤖即点即用的ChatBOT](https://jalicvoygc0.feishu.cn/wiki/OagnwWk9diGw0dkX2czcbKBEnVh)

**引用：**看大厂如何落地标准化的AIGC工作流

**作者：**哈喽骑行

**节选整理：**AI Flow Genius

随着AIGC越来越多的进入工作领域，看我们设计大大如何紧跟潮流最大限度的利用AI工具提升设计率～

AIGC设计方法大公开，速度来围观～

![](img/5bafbfbac3db3920d6393d0edcd69081.png)

## 标准化工作流程

**1.细分工作场景**，基于具体的工作场景沉淀设计方法。拿哈啰骑行运营活动场景举例：骑行活动场景常见的画面构成由背景层（塑造场景）-氛围层（突出权益）-品牌元素层（突出主题）-信息层组成；

**2.基于画面构成提炼AI可覆盖的的元素生成如**：背景层可以结合品牌调性沉淀prompt模版，提升AI设计的可控性及一致性，减少设计师的试错成本；品牌元素层可以通过IP模型训练丰富品牌资产；

**3.通过输出AIGC设计规范**，搭建AIGC品牌资产库覆盖工作场景提升设计效率；

## AI设计应用

**1.Prompt模板化：**为了进一步提升AI生成画面的可控性及一致性、尝试模板化描述词减少试错成本。

**咒语分享**：

```text
Blue sky and white clouds, spring flowers, grasslands，pop matte style, bright colors, blind box toy models,3D rendering,Disney style, OC rendering, Blender, high-definition, movie quality --v 5.1 --s 250
```

![](img/7fb90325eccaf8c784292f642badefc5.png)

**2.标准化工作流程：**通过对画面元素的合理拆分，提炼AI可覆盖的素材生成。落地标准化的AI工作流。

![](img/e5dddd707b30c4c5bdca8a5c428d2873.png)

**3.成果产出：**效能提升upppp, 四季骑行运营海报闪亮登场

![](img/b10272532c58bbc455ffec3295df9349.png)

![](img/89dcdb3201b976da0f267e0466eeaf98.png)

![](img/f827539ec1aaf10e1ec3380912acd55c.png)

![](img/9de3db6161a9cd628a69be449e3015bd.png)

**Comments from AI Genius:**

使用AI工具生成图片时很可能出现大模型 “不听话”的情况，本想稳定地画一个角色但机器经常发散，以为是Stable的，结果来了个大Diffuse😅 大厂生成IP素材时会将部分沉淀IP资产投喂进Lora模型，以保证更稳定地产出。

那么什么是Lora？以下节选自《国内第一个爆款AI应用，为什么是“自拍”？》

> LoRA全称是：Low-Rank Adaptation of Large Language Models，是一种对大型语言模型进行定制化微调的技术，它允许使用很少的数据就可以训练出具有特定风格或特征的模型。它只需要训练一个低秩的参数矩阵，然后将这个矩阵注入到原始模型中（就是矩阵参数线性相加），可以改变原始模型的生成结果。
>
> 对于小开发者来说，LoRA是一个非常有用的工具，可以在不修改或重新训练整个大语言模型的情况下，使用少量样本定制化语言模型的风格和特征。
>
> 从LoRA的全称可以看出，这个模型调整技术最早起源于NLP领域，类似GPT这样的大语言模型参数太多，重新训练代价太大，聪明的人们想了这个折中的办法，低成本而高效的调整模型。
>
> 但LoRA模型受到更多关注是在AI绘画模型领域。
>
> 在约半年之前，热爱美少女图的二次元开发者们持续改进基于Stable Diffusion（SD）的AI绘画模型，LoRA作为一个模型插件被加入SD生态，SD的LoRA插件就用来做一个事情，来给人脸定型！
>
> 在有LoRA之前，SD出图具有随机性，即便使用素材照片垫图生成的方式，也很难控制出图的人脸保持和素材一致。但LoRA一举解决了这个难题。
>
> 有关注AIGC生图的朋友肯定会发现，近半年来，AI生成的韩日风格美少女图数量和质量都猛增，在📕等平台上大火，很是收割了一波流量。这背后都是LoRA模型的功劳，热心的社区开发者用日韩美少女的头像训练出了直接可用的LoRA模型分享到了模型社区C站上，直接让普通用户生成美少女AI图的门槛降到了零。

扫码加入AI Flow Genius社群，探讨使用AI来提升生产力的方法，成为超级个体

超过200需手动拉人 **备注：加群**

![](img/b90cbd847778da53fefa6f1830c60da0.png)
