
# 看见AI日报（20230725）

> 来源：[https://jlqyxzzvbk.feishu.cn/docx/NgMddyeF1oekL6xtJQFcjEyOnzh](https://jlqyxzzvbk.feishu.cn/docx/NgMddyeF1oekL6xtJQFcjEyOnzh)

> 快报时间：2023/7月/25日 周二 16:00

![](img/89ad56fa960085f86fc8941c00492e2f.png)

![](img/09a550ed7ed1b85dab2d391ebb6f8ecc.png)

**欢迎进群交流**

## AI资讯精选

### 全AI生成「芭本海默」病毒式疯转！Midjourney、Gen-2版联手拍成，震撼堪比好莱坞原片

![](img/bb01a3169c1e94dc3538f1b0f5e40e7c.png)

最近，一段用AI生成的芭比和奥本海默拼接电影——「芭本海默」（Barbenheimer）的预告片火了！

一部是关于原子弹之父奥本海默（J. Robert Oppenheimer）的传记片，一部是具有解构意义的女性主义电影，这种混搭的反差感，无疑令人捧腹。

有人这样评论：片子有一些韦斯安德森和昆汀塔伦蒂诺的味道，太赞了！

视频创作者是一位名叫Curious Refuge的网友，他表示，「芭本海默」的预告片由Midjourney和Runway的Gen-2合成，配音也是由AI生成的玛格特·罗比和马特·达蒙的声音。

每个镜头的构图，看起来都非常完美。

更多信息请访问：`https://baijiahao.baidu.com/s?id=1772373176735883434&wfr=spider&for=`

### Attention机制竟有bug，`Softmax`是罪魁祸首，影响所有Transformer

![](img/f7c18c60019e623930433b6d2af80780.png)

自 2017 年 Transformer 问世，这个公式已被广泛使用，但现在，Evan Miller 发现这个公式是错的，有 bug！

Evan Miller 的这篇博客解释了当前流行的 AI 模型如何在关键位置出现错误，并使得所有 Transformer 模型都难以压缩和部署。

总结而言，Evan Miller 引入了一种新函数 Quiet Attention，也叫 `Softmax_1`，这是对传统 `softmax` 函数的创新调整。

有网友对该博客总结出了一个「太长不看版」。博客作者建议在注意力机制使用的 `softmax` 公式分母上加 1（不是最终输出 `softmax`）。注意力单元中的 `softmax` 使其可以将键 / 查询匹配作为概率；这些概率支持一个键 - 值查找的连续值版本（我们得到的权重不是一个查找的 1/0 输出，而是高权重 = 所需的键 - 值查找）。

更多信息请访问：`https://baijiahao.baidu.com/s?id=1772381174338322854&wfr=spider&for=pc`

### 超逼真3D生成模型！华南理工贾奎团队ICCV'23新作：支持重新照明、编辑和物理仿真

![](img/06bb5aab6c0ab62a8ae186a9e093222b.png)

来自华南理工大学的研究团队提出了一种基于文本驱动的三维模型及材质生成方法Fantasia3D，是第一个被接收的能够产生逼真效果的AIGC3D工作。

受益于预训练的大型语言模型和图像扩散模型（Satble Diffusion等）的可用性，自动化三维内容生成近期取得了快速进展。

现有的文本到三维模型的生成方法通常使用NeRF等隐式表达，通过体积渲染将几何和外观耦合在一起，但在恢复更精细的几何结构和实现逼真渲染方面存在不足，所以在生成高质量三维资产方面效果较差。

在这项研究中，华南理工大学提出了一种用于高质量文本到三维内容创建的新方法Fantasia3D，关键之处在于对几何和外观进行解耦的建模和学习。

更多信息请访问：`https://www.163.com/dy/article/IAGCFTL50511ABV6.html`

## AI软件精选

### ArtHub（AI绘画）

众包 AI[艺](https://nav.6aiq.com/favorites/image_ai)术。探索 AI 生成的设计、图像、艺术和顶级社区艺术家和设计师的提示。

![](img/655e1461b0882c7a38540341b6bc2818.png)

链接：`https://arthub.jp/`

### mst.ai（AI绘画）

免费使用[ChatGPT](https://nav.6aiq.com/favorites/chatgpt)、[Stable Diffusion](https://nav.6aiq.com/?post_type=sites&s=Midjourney)、Midjourney

在mst.ai(大臣.ai)的社区中,您可以免费使用GPT-4.0、Stable Diffusion和Midjourney等强大的绘图工具和模型训练设施。这些免费的算力工具将彻底改变机器学习的发展和应用,为个人和企业带来更多可能性和机会。

尤其是在画图的方向上,mst.ai提供了在线免费的 stable diffusion功能。用户不仅无需担心硬件设备的限制,同时也可以省去架构的烦恼,将运算数据全部存储在云端,使得更多的人能够自由地使用。

![](img/f7d403b837e7779da8a60598583fbbec.png)

链接：`https://mst.xyz/home`

### NightCafe Studio（AI绘画）

Lumiere[3D](https://nav.6aiq.com/favorites/3d-generator) 是一个最先进的 AI[平台，](https://nav.6aiq.com/favorites/e-commerce)旨在帮助企业在短短几分钟[内为](https://nav.6aiq.com/favorites/video_ai)电子商务和营销创建引人入胜的电影视频。

主要特征：

AI Operator：动态相机移动、无缝过渡和个性化效果

Smart Scanner：将物体变成具有逼真模型的虚拟 3D 杰作

AI Music：轻松生成专业品质、免版税的音乐

3D 场景：通过令人叹为观止的电影视频提升产品展示效果

**用例：**

为 YouTube Shorts、Instagram Reels 和 TikTok 等平台制作独特的电影鞋视频

通过高质量的对象扫描和 3D 建模将您的愿景变为现实

使用动态摄像机角度和动画提升您的鞋子视频内容

使用 AI 支持的功能体验动态照明和场景变化

通过视觉效果惊人的视频促进您的鞋子销售

[体验 L](https://nav.6aiq.com/favorites/e-commerce)umiere3D 尖端平台的优势，为您的电子商务和营销需求创建令人惊叹的电影视频，提高客户参与度并推动销售。

![](img/0a3b11dbe4287e089b070b43dca5647b.png)

链接：`https://creator.nightcafe.studio/`

**更多AI工具请访问：**`http://ai.kcodez.com/`

## AI课程精选

### 免费课程（GPT）

`https://www.bilibili.com/video/BV16M4y1b7yR/?spm_id_from=333.788&vd_source=3f7939551b27a57d8c51bdca93356279`

GPT内容由浅入深，层层递进，逻辑清晰，诙谐幽默越带有夸张的授课风格，讲述GPT的各种用法，以及如何讲自身结合

![](img/0eb2328c8a6537d27607fe68e738ca01.png)

### 付费课程（ai绘画）

AI绘画基础课Midjourney

优惠购课链接：3- 4:/ 【AI绘图基础课 Midjourney】复制此条消息打开抖音，查看商品详情。【ÊÊPcFWKFONWI3E8￠￠】 `https://v.douyin.com/ixcenrM/`

![](img/205dc8d321204ed8b5ffb010e9624ad5.png)

![](img/df08c914fea41ae0f316c57071122f55.png)

![](img/9b80af94dd1982e055404ed3b45fc316.png)

**欢迎进群交流**

## 4.会员精选

访问地址：`https://nf.video/kkF8Y` 下单时输入：`kanAI` 九五折优惠，或者直接二维码访问

![](img/ae973fa06b39906bb023caf72997657b.png)

![](img/55ed70f551a5f523f373acba9c3f9254.png)

![](img/78e00750b86ae015807f7e3a62c4fc9e.png)

![](img/7b36706a52cd92b2a1e59d17a6da460d.png)

**国内镜像GPT**，支持GPT，比官方更便宜，访问更快，无需魔法，直接访问，九折购买，（购买后联系客服，返还10%购买费用）

![](img/f1d9424aaa8f58cf92023f61bf6e258a.png)

客服二维码（返现客服)

![](img/62c641b2c06437b67fa494b7be22c6fe.png)
