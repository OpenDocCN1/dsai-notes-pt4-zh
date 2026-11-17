
# 智行｜智行AIGC落地项目解析

> 来源：[https://aiyanxishe.feishu.cn/docx/Wz6SdJVR0oRGA5xway7c5kK6npc](https://aiyanxishe.feishu.cn/docx/Wz6SdJVR0oRGA5xway7c5kK6npc)

![](img/514245b40eb8b28d9c98ed2acb6a11b4.png)

![](img/a18d5998581c1cef4dd0c7780e047fa8.png)

近半年来，AIGC技术在各个领域都取得了重大突破，尤其是在设计领域，AI绘画技术的出现加速了设计行业的变革，为设计师带来了新的机遇和挑战。

其中，Midjourney和Stable Diffusion等AI绘图工具快速崛起，成为备受设计师青睐的代表。这些工具利用深度学习技术，通过学习大量的艺术作品来生成图像，具有操作简单、生成速度快等特点，而智行营销需求具有高频率、快节奏等属性，因此将AI绘画应用到设计流程中，有望提高设计师的生产力和效率。

在此背景下，智行ZXD深入研究和探索了AI绘画技术，并尝试出以下可实施落地案例：👀下面看看，实际落地效果吧～

![](img/1ab34655b92470c697297961464b36ae.png)

![](img/fe63d4ab94c47b445ae746b985ba6e68.png)

![](img/531e6248d30ec0edccd80685447589c2.png)

![](img/5102261c4d6b9548e1282c1c9f47759c.png)

本文将通过以上四个案例，详细讲解智行ZXD利用AIGC工具的落地思路及流程，全文大纲如下：

1. Midjourney与Stable Diffusion区别
2. 工作流程变化
3. 案例一：学生形象2D转3D
4. 案例二：Train star海报线稿上色
5. 案例三：H5活动头图2D转3D
6. 案例四：节日海报设计
7. 写在后面

## 01 Midjourney与Stable Diffusion区别

Midjourney和Stable Diffusion是目前市面上最主流的两款AI绘画工具。不少设计师也在纠结哪一款更适合自己。通过以下维度的对比，我们可以更好地选择适合自己的工具。

![](img/7a01f1c8ede33dc242320ff363154504.png)

## 02 工作流程变化

以往未使用Midjourney与Stable Diffusion的情况下，设计师通用设计工作流程如下：

![](img/b654d456f50bc0cb14a6b31e94b8b956.png)

当设计师使用Midjourney与Stable Diffusion时，根据需求类型不同有以下两种工作流程：

**流程一**：首先设计师找参考构思画面，再通过Midjourney和Stable Diffusion关键词生成画面，最后从中选合适图片进行优化调整，工作流程如下图所示：

![](img/69276966f5f9651533161b0b243b89db.png)

**流程二**：首先设计师找参考并绘制草图，再通过Stable Diffusion基于草图生成图片，最后选择合适图片优化调整，工作流程如下图所示：

![](img/ced7a9b35b62a452d0a012fe8565125c.png)

## 03 学生形象2D转3D

学生3D形象是专为智行学生群体而设计的独特人物形象。以往，3D形象需要设计师进行建模和渲染，这既耗费时间，又需要高水平的技术门槛。接下来，我们将探讨如何通过AIGC生成3D形象。

**第一步：绘制学生角色色稿**

为了更好体现学生的青春活力，整体形象设定为：手拿书本、扎着双马尾、戴帽子，穿背带裤...

![](img/0a949bf895c1970e2c7a605cab40f580.png)

Stable Diffusion人物色稿需注意：

1. 结构清晰完整、线条流畅
2. 如有描边，不能太粗，否则会当作结构生成
3. 上色均匀，无需笔触肌理感

**第二步：选择模型**

大模型的选择决定了生成的风格，再配合lora对服装、五官、头发等细节进行调节，这里模型选择如下：

![](img/ff17bda8a6357b1e2bc0020951a9dd31.png)

**第三步：输入关键词**

关键词可以通过Stable Diffusion Tag反推功能来获取，在此基础上修改添加。

![](img/62341a2b948a6c6cbd24db994bf7aca5.png)

**Prompt：**

```
best quality, soft smooth lighting, raking light, 1girl, solo, overalls, eyes, black hair, shirt, hat, twintails, shoes, watch, white shirt, white background, full body, simple background, wristwatch, short sleeves, looking at viewer, sneakers, holding book, orange footwear, short twintails, standing, pants, smile, hair ornament, blue overalls, bangs, orange headwear, surface scatters skin, 120mm lens, 3D, blender render
```

**Negative prompt：**

```
badhandv4:1.3, ng_deepnegative_v1_75t:1.2, worst quality:1.25, low quality:1.25, lowres:1.1, monochrome:1.1, comic, sketch, blurry:1.05, detatched hand, multiple views, depth of field, bokeh
```

**第四步：设置参数**

- 采样步数：20（采样步数越高，生成时间越长）
- 采样方式：`DPM++ 2M Karras`（在速度和质量的平衡最好）
- 尺寸设置：1200x1800（需与草图尺寸保持一致，尺寸越大电脑性能要求越高）
- 提示词相关性：7（数值越高，生成图像与提示词的关联度越高）
- 重绘幅度：0.7（重绘幅度越低，生成图片与底图越接近，反之自由发挥度越高）

![](img/d13b5793bc502a456152a08cce509cb2.png)

第五步：设置Controlnet

- 预处理器：`invert`（图像有白色背景和黑色线条建议选择invert）
- 模型：`control_v11p_sd15_lineart`
- 预处理分辨率：1024（数值越高，生成图片精度越高，电脑性能要求越高）
- 权重：1（权重越高，草图对生成图像的控制程度越高）

![](img/63c658a1e58585d9325d075a00f67ea0.png)

**第六步：生成图片**

根据生成图片反复调节关键词、参数，多次生成。

![](img/8c34979262e084fb2ec110fa48afaea3.png)

第七步：Midjourney优化3D形象

Stable Diffusion能精准的把2D形象转换成3D，但现阶段整体3D立体感、皮肤质感欠缺，继续导入Midjourney优化，整体思路如下：

- `Describle`图生文：Stable Diffusion生成的图片通过`Describle`图生文命令来获取关键词
- 垫图：Stable Diffusion生成的图片再次上传获取图片链接地址
- 关键词优化：基于`Describle`关键词优化调整权重
- 设置：`--iw 2`，生成图片尽量接近垫图

![](img/ceab964da3f9e88972fd0d00dd1db5f4.png)

## 04 Train star海报线稿上色

Train star是火车票部门给予优秀员工的荣誉称号，旨在鼓励每一季度为部门做出贡献的员工。

**第一步：绘制Train star草图**

**设计创意**：采用左右对称布局，画面中央火车飞速驰过，而员工手握奖杯，面露开心激动之情，为整个场景营造氛围感。

![](img/7a7b7a9878fcec0a307a62cdb92b8053.png)

Stable Diffusion草图需注意：线条流畅、结构清晰便于识别

**第二步：选择模型**

选择插画风格模型：`9527`

![](img/cac2f11f1251ec2f7f5acb68b133ffc6.png)

**第三步：输入关键词**

关键词通过Stable Diffusion Tag反推功能来获得，在此基础上进行修改添加

![](img/ab0ed98b4e0caa370a2e07054aa4061d.png)

**第四步：设置参数**

- 采样步数：20
- 采样方式：`DPM++ 2M Karras`
- 尺寸：1280x720
- 提示词相关性：14

![](img/d19af1b64499b2f2e8dd0b38725c469f.png)

第五步：设置Controlnet

- 预处理器：`invert`（图像有白色背景和黑色线条建议选择invert）
- 模型：`control_v11p_sd15_lineart`
- 权重：1.3

![](img/f9d2d30b9570165c535e244dc5f7ded7.png)

**第六步：生成图片**

根据生成图片，不断调整关键词和参数，多次尝试挑选出符合预期的学生形象。

![](img/cf05fa64c7d100485e5415e9c3321bef.png)

**第七步：后期调整**

我们将结合生成图片的优缺点进行合成，并对生成结果中不精准的部分进行调整，以达到更高的准确性和更好的视觉效果。

![](img/f80e74378c1735948a38c20fae1101f5.png)

## 05 H5活动头图2D转3D

特种兵出行挑战赛是通过邀请好友组队形式，最终依据排名来瓜分现金奖励。

**第一步：绘制2D草图**

**设计创意**：通过塑造背着书包，特种兵敬礼姿势的出游形象，融合户外景点，打造出一个充满活力和挑战的特种兵出游氛围。

![](img/1721c311ae8e313a525e8bcaf435af12.png)

**第二步：选择模型**

为了提高生成画面的精度，我们选择了不同的模型来分别生成背景和主体人物，以确保每个元素都能够得到更加精准的呈现。

![](img/c81a7356a3cbcfea6ecd313ba3f20449.png)

对于关键词输入、参数设置和生成图片，已在案例一和案例二中进行了详细说明，在此不再赘述。

第五步：设置Controlnet

- 预处理器：`LeReS`深度信息估算（LeRes depth estimation）
- 模型：`05深度-背景复杂-control_depth-fp16`
- 画面有纵深感、背景复杂，推荐使用depth模型

![](img/8327f8c9c514dd7322a454d01518039b.png)

**第七步：后期调整**

我们将结合生成图片的优缺点进行合成，并对生成结果中不够精准的部分进行调整，最后添加标题文字。

![](img/014a10b67735d4ed4d0af994b04763a5.png)

## 06 夏至节气海报

以上三个案例均基于设计师的草图生成，然而，对于那些更注重画面整体氛围，对元素、创意、位置和大小没有过高要求的情况，我们可以尝试使用Midjourney直接生成，以夏至节气海报为例。

**第一步：找参考构思画面**

收集夏至相关参考如下：

![](img/8bd9694873b3d5d2480935c67a2295d.png)

**第二步：关键词生成画面**

通过Midjourney垫图和关键词描述来生成画面：

![](img/f0e18b8a8644a10b4ee0833e80f4f649.png)

**第三步：后期优化**

后期PS调整色调，添加小智IP，最终效果如下：

![](img/b3242b83121c7c29f4c15390088323cc.png)

## 07 写在最后

通过以上案例，我们可以看到，AI绘画在设计领域中的应用越来越广泛，无论是3D形象、场景海报或H5活动头图，都可以在各个流程中找到AI绘画的机会点。我们也将不断研讨并总结出结合AIGC工具的全新设计工作流程，以便更好地应对快速迭代的AIGC生成工具。

未来，设计行业将进入人机协同时代，我们应该保持终生学习的心态，时刻准备接受新事物的挑战，才能在越来越激烈的竞争中脱颖而出。同时，我们也要明确，AI绘画工具并不是设计师的替代品，而是辅助工具。只有掌握了AI绘画工具，才能更好地发挥自己的设计能力，创造出更加出色的作品。

<https://aiyanxishe.feishu.cn/sync/IaSadpoOOsopKab1ajKcz6z3nAb>
