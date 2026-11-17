
# 58 UXD｜这是什么神仙模型？也太好看了吧！Midjourney炼丹大法丨附风格模型库

> 来源：[https://aiyanxishe.feishu.cn/docx/RuI2dKYdGoFzwvx2yZ5cuAdYnjd](https://aiyanxishe.feishu.cn/docx/RuI2dKYdGoFzwvx2yZ5cuAdYnjd)

![](img/370c41d1de31204458c5c5e2244ec960.png)

![](img/843737ac993951f82e23dea4b7499d2e.png)

话不多说，先看效果

![](img/5990359cd6b2c0edfc7000e746dfb851.png)

通过调用模型代码，生成的图具备风格高度一致性，且审美在线。

这么好用的MJ模型是怎么炼出来的呢？下面我将通过一个简单的例子来讲述整个炼丹的过程，文末免费领取风格模型库！

![](img/d7692f51ede47e715b0bf8ef757f8994.png)

## 确定主题：礼品盒，圣诞氛围

![](img/d7d7ada17db217b3e9936fb522ef01ac.png)

可利用ChatGPT能力来细化提示词，以下是我自己构建的提示词工程，大家有需要可以拿去直接使用GPT提示词。

根据中文【礼品盒，圣诞】来细化英文提示词`prompt`，拆解能够组成画面的可能性。

具体到颜色，质感，物体，元素等颗粒度。给我【5组】`prompt`，需要有差异化的不同画面，每个画面的元素关键词数量不低于10个，描述尽可能简洁。其中每组`prompt`都需要包含风格关键词：【Minimalist】。

`prompt`句式固定格式为：

主体+细节+标签+参数

①主体（性别，五官描述）

②其他细节（背景，光影，气氛）

③标签（艺术家，画风，媒体类型，镜头如CG，2D，3D，特写）

④指令参数（图片比例，分辨率，渲染等）

注意：每段`prompt`需要用逗号隔开，`--ar 3:4 --c 25.0 --s 750 --v 5.2`固定不变，`prompt`由纯英文组成。

例子：

`Gift Boxes,details, brilliant colors, full sun, Award - winning, photograph shot with Kodak Portra 800, a Hasselblad 500C, 55mm f/ 1. 8 lens, extreme depth of field, available light, high contrast, Ultra HD, HDR, DTM, 8K --ar 3:4 --c 25.0 --s 750 --v 5.2`

![](img/a3d74ce096e54034cf98f30fd9c5d6cb.png)

## 调整`prompt`

根据GPT生成的`prompt`，进入提示词工具平台进行调整，完成调整后直接复制到discord中炼丹。

平台链接：https://moonvy.com/apps/ops

![](img/753f554d35b892e534816cb8d16ba180.png)

![](img/cc7c97e8177e6c8cfceea55b9df1955b.png)

1. discord中输入指令：`/tune xxx`

   ![](img/d4742dba6c3d949a6302410b40e3800a.png)

2. 粘贴`prompt`，去掉多余指令，不然会报错

   ![](img/307f0e969292d230f736dba1ff9f6a9b.png)

3. 确认训练数据集数量，正常32个足够，如果想要更丰富更多的图片风格，可以选择64，或者128，等待时间相对久一点

   ![](img/3dde975892265b4cd8d4586b8b977dc2.png)

4. 点击submit

   ![](img/67e036f8854f424b5d856a49cedad529.png)

5. 确认

   ![](img/cda257863e1a9e8858f47b4fa224adfb.png)

6. 等待几分钟，点击链接，新开网页进行模型训练

   ![](img/229168fa81dea286419af26a1ab3e8c2.png)

![](img/3b8f553e095f3c4e4351ce5a91cf5f6d.png)

选择优质图像来训练MJ风格模型，选择1-5张最佳，选择太多会生成比较奇怪的风格。

![](img/6a97b02f1fe67159da3a3fe4d77df122.png)

![](img/0c1296a8e1560099678722883568aed7.png)

`--style 9sPj0EjSgJhlDro`

![](img/b04584117a9abd62d96f36c0e18d7f24.png)

可以多试几次，不同的组合搭配出来的效果也不一样，这就是炼丹的过程啦。

![](img/93a06aa2b79407626717d5df4ec3f138.png)

![](img/3fdc5aa4a3ce7615aea9efd1e5946654.png)

训练模型更适合特定风格炼化调教，质感表达。不适合精细化调整局部画面。有特殊需要可以通过MJ和SD联动来实现。

![](img/0001d6def805bbf1746e89f780283ea2.png)

![](img/c5b8fa1f24d6c93d4d07a960ab3fde19.png)

情绪版，设计风格脑暴，创意实验，插画，设计素材，背景氛围图，展UV贴图，海报等等。

![](img/054c2d88f7fad3b480e05a9cefe0259b.png)

文中提到的风格模型代码都已经沉淀在风格模型的在线表格当中了。

后台、评论区发送「领取MJ风格模型」免费领取自用风格模型库，大家赶紧去试试吧！

你还有什么厉害的风格模型，欢迎各位MJ玩家评论区留下你的style代码，互相交流，共同进步！

<https://aiyanxishe.feishu.cn/sync/IaSadpoOOsopKab1ajKcz6z3nAb>
