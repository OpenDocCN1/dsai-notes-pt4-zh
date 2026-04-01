# 【QA文档】 AI+视频

> 来源：[https://datawhaler.feishu.cn/docx/B3PbdfICJo3ZHNx1WbscR2HInpe](https://datawhaler.feishu.cn/docx/B3PbdfICJo3ZHNx1WbscR2HInpe)

> 欢迎来到《AI + 视频》的互助QA文档！👏👏

>

> 我们推荐大家通过**「自驱学习 + 分享讨论」**的方式，与更多有想法、有学习积极性的小伙伴，共同学习制作AI视频，分享学习心得解答彼此的疑问。请随时提出问题、发表观点，让我们共同进步，携手共创更优质的学习教程！

>

> **温馨提示：**欢迎在文档中随时**提出问题、建议或评论**，我们会关注并据此优化QA文档。

**推荐大家多交流哟～**

> **开源学习**：是一种在开放环境下的协同学习模式。它强调学习者之间的互动、共享和合作，通过共同创造知识、协同解决问题和创新应用，以实现学习目标。

相逢都是缘，珍惜每一次交流的机会。

在你身边的大都是，对学习充满饱满热情的人，也不乏领域中的佼佼者。走过路过不要错过！

---

## Q & A

### Q1：生成多个不同分镜下的人物时，有没有办法保证生成的是同一个人？

怎样保持人物一致性，形象一致性，目前有这么几种做法：

+   第一种，使用相同的模型，参数去绘制，对主角描述要一致。

+   第二种，使用**Midjourney**或者StableDiffusion专门生成一系列的人物动作，然后使用ps扣出来备用。

+   每次提示词**单独构建场景**，**不要人物。**提示词参考如下：

```
连续性人物设计
film still of a beautiful happy girl, pale skin, golden earrings, in a magical forest, epic fantasy; split into 3 different images, shot from multiple angles
beautiful woman with brunette hair, blue t-shirt, split into 9 different images, white background, shot from multiple angles --niji 5

Stable Diffusion提示词模板
((6 different images of the same person, shot from multiple angles)) beautiful woman with brunette hair, blue t-shirt, split into 9 different images, white background, shot from multiple angles
```

+   人物动作根据场景使用ps合成到场景。也可以使用MJ合成一个或多个人物到场景。案例：midjourney超实用技巧——同一场景多角色保持一致性,--cref命令的妙用：https://www.bilibili.com/video/BV1K2421K7CU/

+   然后再去生成视频。

+   **Midjourney角色一致性命令 "c**ref "测评丨如何保持多个角色一致性：https://www.aieva.cn/edu/1973.html

+

StableDiffusion的Reference Only插件也可以实现角色一致性。

+   **学习笔记：使用 ControlNet** 的 reference-only 控制：https://medium.com/@h.zuomin/%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0-%E4%BD%BF%E7%94%A8-controlnet-%E7%9A%84-reference-only-%E6%8E%A7%E5%88%B6-b7a4699a28aa

+   [Stable Diffusion仿](https://zhuanlan.zhihu.com/p/629980765)制神器,Reference Only评测

+   SD的思路也是类似，可以使用controlnet的Reference-only插件实现同样的功能，参考链接：https://github.com/Mikubill/sd-webui-controlnet/discussions/1236

+   【**AI绘画】ControlNet 之 R**eference only 锁定面部跑图：https://www.jianshu.com/p/96302fad84e2

+   用**Midjourney**生成一致性的人物角色 https://www.uisdc.com/consistent-character-midjourney

+   用**InstantID**来做一致的人物角色 https://www.jiqizhixin.com/articles/2024-01-22

+   **SD**一致性角色 - 同一人物 不同姿势 https://blog.csdn.net/m0_73883919/article/details/135314693

+   用Seed值让**Dall·E**画出一致的人物！(有GPT4的看这里) https://zhuanlan.zhihu.com/p/675446051

+

### **Q2：**绘画提示词绘制生成结果与想象差异巨大。

剧本转剧照的过程中，剧照的绘画描述词可能包含了多个画面的提示词，如

```
杰克打开地下室的门，灯光昏暗。他走向一个角落，那里放着一个古老的钟，灰尘覆盖，显得非常不起眼。
```

该描述词包含两个画面，

+   第一个是杰克打开地下室的门，灯光昏暗。

+   第二个是他走向一个角落，

+   第三个是那里放着一个古老的钟，灰尘覆盖，显得非常不起眼。

这样情况下，就可以把描述词拆分成两个画面分别绘制就可以了。

结果如下：

![](img/db25b8f76c8e99a5276eac0649a07303.png)

![](img/bb726a3cb203d800bd6500a78f1b704a.png)

![](img/20af3bd25fd5540b61474aea67f34b88.png)

也可以使用AI拆分画面：（使用豆包实现： https://www.doubao.com/chat/ ）

> **你：**

> 将以下场景描述拆分成几个可以实现的剧照描述

> 杰克打开地下室的门，灯光昏暗。他走向一个角落，那里放着一个古老的钟，灰尘覆盖，显得非常不起眼。

> **豆包：**

> 以下是拆分后的剧照描述：

> 1. 杰克站在地下室门前，手放在门把手上准备开门。

> 2. 地下室的门被缓缓打开，里面透出昏暗的灯光。

> 3. 杰克的身影进入地下室，朝着一个方向走去。

> 4. 镜头聚焦在那个角落，古老的钟安静地放置着，上面布满灰尘。

### **Q3：关于AI工具的使用**

首先要保证直播的基础工具会使用，因为所有工具的使用逻辑都是类似的

+   需要选其他工具时，需要保证现在工具到底出的什么效果，到什么程度，有什么缺点，是当前工具的限制还是使用方法的问题，否则就算是使用其他工具，也照样无法达到理想的效果，

+   我的看法工具本身没有那么重要，主要是去做，简单工具照样可以做出视频。

+   不要陷入工具的海洋，最后也只是画出一张好看点的图片，而不是完整的视频。

+   工具的使用一定会越来越简单，不要限制住自己的思维。

+   根据自己实际情况，选择合适AI工具。

+   一个工具使用100次，比会用100个工具更强。（李小龙：我不怕学一万种腿法的人，我怕的是一种腿法练一万次的人。）