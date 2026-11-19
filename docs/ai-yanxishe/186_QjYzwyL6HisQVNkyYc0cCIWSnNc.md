
# 乐信UED | 设计师失业？我们让AI做了一次双十一

> 来源：[https://aiyanxishe.feishu.cn/docx/AehcdJb01ok05ExxbHUc8H83nFf](https://aiyanxishe.feishu.cn/docx/AehcdJb01ok05ExxbHUc8H83nFf)

前段时间，有个朋友突然问我关于AI的问题，一般会用AI来干嘛？刚好，我们在双十一大促中就应用了AI进行创作，这里浅聊下AI创作。

首先，我们来看看时下最流行的两个AI绘画工具Midjourney和stable diffusion生成的一些图片效果。

![](img/4dda2d678c9f45dec0b5e25b85ac6b14.png)

（由Midjourney完成）

![](img/2ce92844cc2b0405e83c137a6b3ac388.png)

（由stable diffusion完成）

是不是很震撼？其实，只需要输入一段文字描述，加上艺术风格和艺术家的名词，即可实时快速生成精美的绘画。当然，prompt提示关键词便是AI绘画中最有价值的部分。关于AI，在2022年的进化是巨大的，也是当下争议非常大的一个话题。如今AI已经涉及到各个领域；在绘画领域，AI的应用发展的也已经越来越成熟，相对应的，作为设计师，如何将AI应用到商业设计领域是我们最关注的问题。目前来看，市面上鲜有将AI应用到商业设计领域的案例，因此，尝试将AI首次应用电商大促设计领域也是我们最大的挑战。通过网上大量资料和研究学习，针对AI现有的几个工具进行了研究和使用，我们挑选了Midjourney、Stable Diffusion和runway等几个工具。

在风格上，我们希望尝试大促设计艺术化，尽可能摆脱陈列感强、用户视觉疲态的常规化设计形式。风格确定后我们就开始具体的操作了。

## 1. 从0到1的AI工作流

1. 首先，简单用PS合成一张我们定义好的风格图，如下

   ![](img/6197190ce66b0ff5923dcbf8aa7bd6ee.png)

2. 然后，用这张效果图在stable diffusion反向生成关键词，关键词中会包含图中元素的相关描述词。

3. 第三，应用关键词训练AI艺术化模型包。为了赋予AI更强的艺术化，我们将200多张艺术化3D风格的参考图提供给AI，得到一个AI通过深度学习后的一个艺术化模型包，这时这个模型包便具备了我们最初定义的风格化。

   ![](img/af2cd9bb52af1dd355082875afd8a253.png)

   ![](img/1ddadfba79bab28e269f983ac5376cd8.png)

   通过关键词，再次使用艺术化模型包，调整`Steps`、`method`和`CFG scale`等重新测试后生成30张图，就基本具备了我们想要的艺术化效果，接下来便是让它调整色调和判定产品，将产品融入到场景中。

   从产品内容、细节、环境氛围、色彩感觉和构图等进行描述，然后我们通过这些生成的描述词，进行适当的删减调整测试，将产品和部分细节关键词修改。

4. 最后，通过调整后大概生成了20张图，再经过一些微调就基本可用了。

   ![](img/1e33a86a8a6e501bea49d752d74e8285.png)

基于AI的无限可能性，我们利用新训练的模型包和开源模型包进行对比测试，针对其他的会场进行了生成。

![](img/2b794c55c6dbe25884cd20a85c86ed88.png)

![](img/f8142821a6a5cbdddf9033f9fe3e6b45.png)

根据实际文案和产品布局需要，选择空间感和元素更适合的3D艺术化模型包生成的图进行后期合成完成数码会场视觉。

![](img/f9610d2857bed40431cc255dd1b33552.png)

可以发现，通过3D艺术化的模型包生成的风格和细节在理解艺术化需求上更加智能和成熟，在这个时候AI在效率上已经可以居于无限可能大量生产艺术化的效果了。获得我们想要的风格化图片后，增加大促相应的产品和主题，就可以直接在线上使用了。

![](img/cdb40fb32060bd410c0955a4e2373e43.png)

![](img/20615f675b739771341eb53858d4ac09.png)

![](img/55c59a6709cb6e154a5a310d3d4a601f.png)

## 2. AI技术的加入对业务带来的影响

1. 当前大促的会场主视觉设计，AI技术的加入，使得沟通成本大幅降低，输出效率和精准度大幅提升，粗略统计，同等条件及要求的前提下，AI的算力可以达到3秒/张，1小时约可输出约1200张高质量设计方案。

2. 会场商详到达率20.5%，环比同期提升4.3pt；首页包站日均点击率环比提升149%；包站日均点击率2.11%，环比提升74.4%。

## 3. 更多可能性

1. 为了更多可能性，将上述得到的关键词也在midjourney上尝试，可能是因为midjourney与stable diffusion的prompt不太一样，生成的结果缺少了艺术化的感觉，但是整体效果还是不错的。

   ![](img/1fc07e63aecf20fe618edc0fbb0eb7f4.png)

2. 通过修改关键词，对数码的视觉稿也进行了生成，可能是prompt不够准确，尽管效果不错，但是构图上不是很合适。

   ![](img/c441ed09ae1d7a31ef9f730d6026c6c2.png)

## 对于AI未来的发展

网上一直有这么两个争论，AI到底是辅助画师还是取代画师？画师对于AI工具应该是抵制还是接受？网上关于取代的言论，假设指的是Novel ai、stable diffusion 这类AI代替了许多的约稿需求，并以此衍生出了对未来AI会取代人类工作的设想，从生产力的角度看，可以想下假设画师驾驭了AI绘画这个工具后对整个行业的正向影响，对艺术领域的创新将会是超前的。技术的发展可以解放生产力，生产工具颠覆生产关系，AI的影响也将会是革命性的，但取代人类画师并不是ai的目的，这里提出AI是否取代的话题，主要也是对大众对于“ai是否会取代人类画师”这一论点作出浅浅的分析。

## 资源分享

### AI工具网页版链接：

Disco Diffusion：https://colab.research.google.com/github/alembics/disco-diffusion/blob/main/Disco_Diffusion.ipynb#scrollTo=License

教程链接分享：https://www.bilibili.com/video/BV1b5411X7MM/?spm_id_from=333.337.search-card.all.click&vd_source=236884c80d55916aa79f1868c3df8142

Midjourney：https://discord.com/channels/662267976984297473/@home

教程链接分享：https://www.bilibili.com/video/BV16d4y1A7Zq/?spm_id_from=333.999.0.0&vd_source=236884c80d55916aa79f1868c3df8142

Midjourney社区：https://www.midjourney.com/app/

Stable Diffusion：https://www.bilibili.com/video/BV1rd4y1k7oQ/?spm_id_from=333.999.0.0&vd_source=236884c80d55916aa79f1868c3df8142 （这个是本地配置的整合包，需要window系统的N卡才可使用，感谢用爱发电的UP主）

教程链接分享：https://space.bilibili.com/250989068?spm_id_from=333.337.search-card.all.click

DALL·E：https://labs.openai.com/waitlist

Tiamat：https://tiamat-ai.feishu.cn/share/base/shrcn2uXL8wNiveykIFYqWSzsPf?continueFlag=6d45454a3e37691ce2095e72097fffae

网页端的Runway人工智能工具：https://app.runwayml.com

https://space.bilibili.com/1962568408/?spm_id_from=333.999.0.0

https://space.bilibili.com/371846699

B站教程众多，有兴趣可以自行搜寻，这里只分享一些干货，再次感谢用爱发电的up主们。

## 关于版权

1. 针对Ai的训练和图片的使用，假如是因为画风问题，在版权上是无法对某一种特定风格申请版权的，100TB的训练数据压缩成一个4GB甚至1GB的模型，从AI的底层生成原理上看，Ai的作画方式也不能算是缝合，而是创造了全新的图像，这是一种变革，而且也是技术的发展不可避免的事情。

2. 目前SD 开源模型的授权协议是 Creative ML OpenRAIL-M license，在非有害目的的条件下，允许商用和非商用。自己部署已经开源的 Stable Diffusion，也就是说用的是自己的 GPU 资源，所生成图片（Output）的著作权，Licensor（Stability.ai）claims no rights。图片著作权和如何对外授权都归你自行定义，Stability.ai 不做约束，也不会共享你的著作权和收益。

3. AI绘画概念目前相对较新，在法规上比较缺乏，还是属于空白，也有法务认为AI绘画作品本身不具备版权，但可以作为电子财产。国家对于版权的登记，是需要有详细的过程图，且需体现人类智力创作的成果，但是AI本身生成图像是极为快速的，本身就不存在过程，AI只是通过不断的判定和校正直接出现它认为是正确的图像。

   但却不是说利用AI工具辅助并且进行过二次创作的作品不存在版权；比如，用midjourney生成底图，再将底图放到disco diffusion中渲染，最后到photoshop进行后期合成，这样经过二次创作和加工出来的作品便是有版权的。故，人类智慧才是关键，把AI当做辅助工具生成素材，进行二次创作，相当于在合成中找素材修改调整后进行后期二次合成的原理一样。

## 最后

当版权的问题达成共识后，AI 作图便有可能成为划时代的辅助工具。AI 作图已经可以确定是一股不可阻的潮流，技术不断进化，趋势发展迅猛，未来对于AI你是接受还是抵触呢？

https://aiyanxishe.feishu.cn/sync/IaSadpoOOsopKab1ajKcz6z3nAb
