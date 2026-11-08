# 重生之我在好莱坞里当演员，只会用AI换脸已经过时了

> 来源：[https://agispace.feishu.cn/docx/Y117dHfgvofsqnx2T1ecLpYjn8a](https://agispace.feishu.cn/docx/Y117dHfgvofsqnx2T1ecLpYjn8a)

> 原创：AI沃茨

> 👸号:卡尔的AI沃茨

> 原文链接：https://mp.weixin.qq.com/s/TlrX34m8d781vbbpLWuEvg

作为AI视频界里的幕后制作人，

通常我都是基于脚本来设计甲方们需要的IP形象或者产品

但架不住有乐子人

![](img/3e06b14b58cbe54459e826e02523eef7.png)

日本麦当劳发布的AI广告，无视了4K画质、画面过渡、人物互动

就这样水灵灵发到官号上了。

咦，这不就是AI演员？

那有没有种可能……

我也能演？甚至不只是广告，还可以和知名演员一起演电影？！

恰好前段时间Flux爆火，全世界都在TED上演讲，

我要用AI让电影商们看看我的可塑性，

我必须马上参演！

这次我们一共只需要两个网站，

**Re**plicate、Hugging face()

整个过程有手就行，配置10分钟，训练20分钟，半小时能完成，全程不吃本地电脑配置。

跟AI换脸对比，用AI模型训练的优势在于可以生成各种各样、不同角度的表情，更能满足视频制造的需要。

## **Replicate**

YT有位赛博菩萨放出了Replicate的10刀代金券，

领取地址：https://replicate.com/invites/572ae3c6-1385-41b3-afdc-1e99e11836b8

两周前，用Fal.ai训练真人版 Flux LoRA 的成本是 5 美元，每次生成图像时成本为 0.075 美元。

Replicate相当于直接给你包圆了，

能训练出一个“懂你脸”的LoRA，还能生成大几十张图像。

**第一步**，在首页搜索“lucataco”

![](img/a17cb7ac8a784cd9a06605a9d376c76d.png)

在这个帐户列表下选择“ai-toolkit”和“flux-dev-lora”分别右侧打开，

“ai-toolkit”是用来训练的，而“flux-dev-lora”是用来生成的

**第二步**，来到“ai-toolkit”页面

顶端栏选择Train，

这时候界面会有不同的设置需要填入，

我们只需要填写里面的“destination”、”images”、“hf_token”、“repo_id”，

其余统统保持默认

![](img/da6f843265bcd0a380f4a3284029fcaa.png)

操作页面

1.  “destination”: "carl-lora"

1.  "images": 上传12张自己正面照的压缩包，zip格式，每张图片取名方式均需要是“a_photo_of_carl（图片序号）.png”

感谢妙鸭相机，上次辛辛苦苦搜刮的正脸照够用。

## **hugging face**

来不及犹豫了，接下来上场的是 **huggingface.co**

右上角点击头像，打开“Setting”界面

![](img/f6a3e296f207dc4884d89c47ed8534df.png)

**第三步**，在左侧选择**Access Tokens**，点击“Create new Action Token”，能打勾全选上后，创建得到hf_token，复制到Replicate对应项 hf_token 就OK了

![](img/c597dca64acc5fb553cb492bf68dd8da.png)

**第四步**，还是点击右上角，选择 **new model **创建新模型。

ModelName 同样设置为“carl-lora”，记得复制下来放到Replicate页面的“repo_id”。

## **AI我自己**

参数设置好后，点击训练，整个训练过程大概要20～30分钟，

![](img/d866b11c7c558c2561edabf72351ea65.png)

这时候你可以选择任意一个语言大模型，GPT/kimi/Claude()都可以

让它给你生成一段满意的提示语，像这样：

**“**carl as a Superman looking straight into the camera” （卡尔饰演一个超人，直视着镜头）

训练完成后，访问 **flux-dev-lora**，设置三个参数，

+   “Prompt” = 图像提示语

+   “hf-lora” = 上一步的 ModelName

+   “lora_scale” = 1

![](img/1e330c75a51be927a46c2f85749246fa.png)

现在你就可以生成只属于自己的形象了，

我可以吃薯条、卖可乐、跟死侍一起逃出生天、当蝙蝠侠的动作替身、成为钢铁侠的科研助手

![](img/0f3740f93ce40dd341907e0aeef13343.png)

![](img/f349f15d51eb1293090f283b4b7323fe.png)

![](img/392653406faec30444506b65705d915f.png)

![](img/69d1bd3fa95b942e47aa8d3e96651054.png)

![](img/127e3c0233202ac0a1cf21c72694b584.png)

甚至我还拿着图片去 Runway 里面 Roll 了一把，

![](img/0a12b9d84d0dea4359ec27ebb7c7d5ef.png)

![](img/8a0558f196392967af25532ca70babe5.png)

怎么样，作为新人主演的视频介绍信绰绰有余吧

下一步等好莱坞回复，

复仇者联盟5，

2026，肯定有我阿祖一席之地

![](img/64d4ce8038c4b010a2419b375fb2ebfa.png)

作者 / 卡尔**动手学AI** 知识库 👉 learnprompt.pro

---

最后，感谢你看到这里👏如果喜欢这篇文章，不妨顺手给我们点赞👍｜在看👀｜转发📪更多的内容正在不断填坑中……