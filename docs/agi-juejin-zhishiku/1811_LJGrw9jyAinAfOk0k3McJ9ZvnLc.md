# 不用设计师也能自己出图，如何通过AI工具来个性设计自己的家！

> 来源：[https://agijuejin.feishu.cn/docx/GlNKdGLWaoNRkqxL6TFcDC6anZH](https://agijuejin.feishu.cn/docx/GlNKdGLWaoNRkqxL6TFcDC6anZH)

> 作者：德里克文

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/642832264)hihu.com/p/642832...

我想要利用AI来做一个简易的，可以充分符合个性化想法的工具，在这里分享下我的想法和一些基础的操作。

在这篇文章中，只需要采用国内通行的的AI模型，拍几张毛坯房的照片，哪怕你完全不会画画和设计，也能让AI帮你参谋你房间实际的效果啦！

我希望最后能够将这个文章的内容通过AI工具[进行完整](https://www.zhihu.com/search?q=%E6%88%91%E8%81%94%E7%B3%BB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A642832264%7D)整合，欢迎有技术手段的小伙伴和 我联系 ~

## **创意背景**

在当今数字化时代，家装设计产业正在迅速发展，人们对于家的需求不仅仅停留在有房子可以住上，更多的是对于家庭装修风格个性化和多样化的追求。

此[外，AI技](https://www.zhihu.com/search?q=AI%E6%8A%80%E6%9C%AF&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A642832264%7D)术的不断进步，为设计行业带来了巨大[的效率推](https://www.zhihu.com/search?q=%E6%95%88%E6%9E%9C%E5%9B%BE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A642832264%7D)动。基于此，我设计了一个基于大模型的家装效果出图项目，旨在通过 AI技术 实现普罗大众也能上手的家装效果设计，也解决在家装设计过程中需求变化，导致 效果图 调整的效率问题。

![](img/8c2f880b8ce88e9c166d23bad5a1bcd0.png)

如果大面积推广，可大大降低普通用户与设计师之间的沟通门槛，减[少沟通成本](https://www.zhihu.com/search?q=%E8%AE%BE%E8%AE%A1%E6%96%B9%E6%A1%88&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A642832264%7D)也将解放设计师让精力，专注在 设计方案 效果及功能合理性建议上

+   1、提高 [家装效果图 ](https://www.zhihu.com/search?q=%E5%AE%B6%E8%A3%85%E6%95%88%E6%9E%9C%E5%9B%BE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A642832264%7D)出图效率，表现力，使其能够为家装设计行业注入新的活力和创意。

+   2、为更多普通大众用户带来易上手的直观视觉体验，同时也为室内设计领域的设计师提供更多的发展机遇和效率提升。

+   3、推动AI人工智能辅助室内设计的发展，为未来设计产业的创新和发展奠定坚实基础。

## **创意目标**

![](img/77aab5280de95745c00b80f77c7aad8c.png)

**通过手机拍摄毛坯房现状， **[**上传照片 **](https://www.zhihu.com/search?q=%E4%B8%8A%E4%BC%A0%E7%85%A7%E7%89%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A642832264%7D)，输入简单的基础描述，即可根据实际场景生成众多的风格参考效果

1、实现可生成的家装效果图的个性化和多样化。

2、提高效果图生成的易用性能。

3、为室内设计产业提供一种新的效率工具。

## **创意及技术实现路径**

![](img/9608e5a1136fb6ceb5f26002572bf1be.png)

### **一.建立一个基于语言大模型的Pro**mpt提示词，通过提示词丰富细化提示内容

我尝[试创建的](https://www.zhihu.com/search?q=%E6%96%87%E7%94%9F%E5%9B%BE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A642832264%7D)Prompt（以文心一言为例，根据结果反馈进行了几次优化）: 我希望你担任一个专业的室内设计师，你拥有丰富的室内设计知识，能够采用丰富的文字语言描述效果画面场景。你将帮我写出高质量的 文生图 的Prompt，你将对我需求的描述进行理解和拓展，以增加更多的适当的词汇来使得画面更加丰满。最后请将这些所有的描述整合成一段英文短文，并用Prompt的形式输出。我需要你创建的内容是：【请输入相应描述】

![](img/8e800727d3d4f77e45f51ac4f8de7b8f.png)

![](img/6603a459a68feaffe45a9034d99c73c1.png)

![](img/b2fe2fca8eafc7183f7d5dd438d531a1.png)

![](img/3f1f72d8d97e098bb4e3d6358dcccde7.png)

![](img/f6b7fcc1e02230d17d4ebdaf63bec256.png)

###  二.将Prompt提示词内容直接导入到文生图大模型中，生成图片

由于我并非计算机从业人员，没有代码基础，我只能阐述我的想法，并且采用类似的国内模型（无界AI）来模拟，设想是能够整合几个模型进行一站式输出，不需要不同软件之间切换：

1、首先我建立一个没有Controlnet的图片[看看效果](https://www.zhihu.com/search?q=%E5%9B%BE%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A642832264%7D)，我把第一步生成的文字放入文生 图模型 中，我想要一个北欧现代风格的卧室。

![](img/c8f1166bacef36ab3dbef9c56a58de1c.png)

2、然后我输入一个带手绘的图片，建立一个带Controlnet的图片看看效果，我选择MLSD模型，把第一步生成的文字放入文生图模型中，我想要一个北欧现代风格的餐厅。

![](img/591a7e8b61248bb8685629a0280578a2.png)

![](img/37d968f7feb0da37ba1b033b5650d2c5.png)

![](img/d88f2b8aa52764afe71db77847b7c773.png)

3、再试试毛坯照片的效果吧！

![](img/c1596a7cf2f7d04546c0f87534dbc294.png)

![](img/58bfe906453a8baa3301e6a5fbae97fb.png)

## **创意总结**

[本次提案](https://www.zhihu.com/search?q=%E7%94%9F%E6%88%90%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A642832264%7D)，我希望能开发出一个基于大模型的快速效果图 生成器 ，可以实现个性化和多样化的效果图，建立起一套完整室内设计效果图体系。

![](img/2803e12f2e9195c0c41c79a011ffa65d.png)

推广该项目，可以为更多的用户带来更加容易上手的家装效果体验，同时在本次项目中，还可以在文生图的Prompt中进行提升，采用（PINK | GREEN |YELLOW)或者是相关风格的不同变化模数，可以随机生成不同的风格，色彩，批量生成图片给到使用者更多的选择。

---

我是 [德里克文 ](https://www.zhihu.com/search?q=%E5%BE%B7%E9%87%8C%E5%85%8B%E6%96%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3098715893%7D)[，一个对AI](https://www.zhihu.com/search?q=%E5%AE%A4%E5%86%85%E8%AE%BE%E8%AE%A1%E5%B8%88&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3112664015%7D)绘画，人工智能有强烈兴趣，从业多年的 室内设计师 ！如果对我的文章内容感兴趣，请帮忙关注点赞收藏，谢谢！