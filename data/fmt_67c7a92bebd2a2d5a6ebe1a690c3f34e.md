
# 5月15日

> 与 TPU v5e，峰值计算性能提高 4.7 倍，HBM 和带宽增加了 1 倍，芯片间互联（ICI）带宽也增加了 1 倍。另外，Trillium 的能效比 TPU v5e 高出了 67% 以上。

## 7、Gemini 塞进了全线产品中，包括谷歌搜索、Android，以及谷歌邮箱、谷歌 Photos 等

> 谷歌搜索推出 AI 概述（AI Overview）新功能，Android 内置的 Circle to Search 功能，手势“圈”一下内容就能搜索。谷歌 Photos 推出 —— Ask Photos，一键从图库中帮你找到想要的照片和视频。

## 一句话评论：发布了很多意料之内的产品，诚意拉满，惊喜度不足

Google 最新图像模型 Imagen 3 和视频模型 Veo 的 waitlist 申请地址：https://aitestkitchen.withgoogle.com/tools/video-fx

## 详细内容：

+ **Project Astra：**

基于 Gemini 系列模型打造，谷歌表示，研究人员们希望这样的智能助手，能真正在人们的日常生活中提供帮助。

比如，Project Astra 的延迟似乎还是比 GPT-4o 长，谷歌的演示中也没有体现是否能打断智能助手说话

![](img/116cd181ba46efbac72bb40c152c4df1.png)

[20240515061321_rec_.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZSUKbW1xeohDSyxCafucxDbmn1b)

还能完成帮忙找眼镜这样的基础工作。

![](img/8f1cb47e8ebb2895774fb554abc7f77c.png)

+ **Gemini 1.5 Pro：**

上下文窗口来到了惊人的 200 万 token。

![](img/31f5e1e2f000a9a6fd07f8164c033e4c.png)

+ **Veo：**

可以生成 1080p 的高质量视频，生成视频的长度也看齐 Sora：最长超过 1 分钟。

谷歌还强调，Veo 生成的画面一致性出色，并且可以 hold 住各种风格。比如“延时拍摄”和“航拍”这种术语，直接写在提示词里，Veo 都能 get。

目前，谷歌已经和电影制作者展开了合作，在电影项目中探索 Veo 的应用。

![](img/5216db9314359af6ac20e54d7b27fc99.png)

[20240515063811_rec_.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/USBHbj0f8ofsOixrDsGc0RUDnad)

+ **Imagen 3**

官方提到，与此前的模型相比，Imagen 3 的视觉伪影更少，生成的图像细节拉满，并且在生成文字方面效果也很好。

![](img/bf68791071abd649e7d3815708c42b1c.png)

+ **谷歌搜索**

基于 Gemini，谷歌搜索推出 AI 概述（AI Overview）新功能，彻底化身 AI 搜索。

用户在搜索框输入问题，马上就能得到一个 AI 总结的答案，包括相关链接。

而且是超长问题都能处理的那种，比如：

> 查找波士顿最好的瑜伽或普拉提工作室，并提供他们的入门优惠详情以及从灯塔山步行所需时间。

AI Overview 秒秒钟就会列出五星好评且距离灯塔山近的店，并附带课程标价，连店在地图上的位置都标记了出来：

![](img/d91d29e28afde7e55723fc1e021c8696.png)

对于这种复杂问题的搜索，谷歌表示采用了多步推理技术，可以将复杂问题拆解成一个个小问题，然后自动对问题进行排序回答。

多步推理加持下，AI Overview 已经脱离了纯粹的搜索——把制定计划一并搬进了搜索引擎里。

此外，谷歌搜索还支持视频搜索了。

演示中，唱片机出故障，只需一边录制视频一边说出疑问，谷歌 AI Overview 就会立刻分析故障原因，并给出处理建议。

![](img/0194045e56ba653ad4730ab4bd167ee1.png)

+ **Android 15**

谷歌把 Gemini 也带到了安卓系统中。凭借直接内置的 Circle to Search 功能，用户使用简单的手势“圈”一下手机上看到的任何内容，就能进行搜索。比如圈出练习题，就能一键获取答案

![](img/89ffaa604f9fbbc2d737cb9cc87898f9.png)

此外，还可以在一个应用之上随时调出 Gemini 助手层，随时使用。你还可以将 Gemini 生成的图片直接拖到 Gmail 等应用中，直接询问某个视频中的具体信息，不用滑动翻文档询问某个 PDF 中的信息……

+ **谷歌 Photos**

谷歌 Photos 基于 Gemini 也推出了新功能——Ask Photos，可以一键从图库中帮你找到想要的照片和视频。

![](img/b0cca2d4f1cf41e91574a9a32569eac8.png)

比如你想知道自己的 baby 是什么时候学会游泳的，Ask Photos 会从你在谷歌 Photos 中存储的成百上千张照片中查找出含游泳、游泳证书等相关图片，最后总结给出回复：

+ **NotebookLM**

谷歌 AI 笔记应用 NotebookLM 也大升级。当你提出一个问题后，背后的 Gemini 模型就会结合 NotebookLM 中的笔记内容，进行多模态的语音对话式回答。

![](img/3f71303e3e0e7440837d864185e43cc5.png)

更多细节内容可查看：

https://blog.google/technology/developers/google-io-2024-collection/

# 5月14日

## AI应用

### 1、OpenAI 发布 GPT-4o，实时可打断交互炸场

**一句话评论：**

> 【Her】来了！我愿称 GPT-4o 的发布为 AI 情感陪伴的 iPhone 时刻

**更多可看：**

> [AI 日日新（更新中）](https://vx8e8325ai4.feishu.cn/wiki/SswEwb5AwiBP0bk5RUzcAdBwnbd?fromScene=spaceOverview)

### 2、FunClip：阿里发布自动视频剪辑工具

**一句话评论：** 通过视频的语音识别，用户自由选择识别结果中的文本片段或说话人，点击按钮即可获取对应片段的视频

**更多可看：** https://mp.weixin.qq.com/s/V_2pGoAtOjRUvtlSdHYMEA

### 3、vivo 自研蓝心大模型升级

**一句话评论：** vivo 蓝心大模型升级为 AI 多模态大模型，AGI 的发展方向大家看来还是共识的。

**更多可看：** https://mp.weixin.qq.com/s/X0A868dcNuyLMyG7EnpWOQ

## AI技术

### 1、CuMo：多模态大模型的新架构

**一句话评论：**

> 通过在视觉编码器和多层感知机中使用“Co-upcycled Mixture-of-Experts”技术，提升了模型在处理图像和文本时的性能，同时保持了较低的计算成本。

**更多可看：**

> https://chrisjuniorli.github.io/project/CuMo/

### 2、零一万物 Yi-1.5，国产开源大模型排行榜再次刷新

**一句话评论：** 李开复的双轨战略目前看走得还是不错的

**更多可看：**

> https://mp.weixin.qq.com/s/lwHfocVcoJnTddIi9nvvsg

### 3、RAFT：一种适应特定领域 RAG 的大模型训练新方法！

**一句话评论：** 结合了检索增强生成 (RAG) 和领域特定微调 (DSF) 的优势，提高特定领域的问答能力

**更多可看：** https://mp.weixin.qq.com/s/lCUFx8XGTBqvR4kKvNDnXg

OpenGlass：将普通眼镜变为智能眼镜

一个不到 150 元的 DIY 项目，让你的普通眼镜变身为能够进行人脸识别、物体识别、文本翻译等功能的智能眼镜。

https://github.com/BasedHardware/openglass

## 【号外】5.14 Open AI 春节发布会（短短 25 分钟讲完！）

### Open AI 的春节发布会做个小总结

#### 1. 新增桌面应用程序 & web UI 更新

#### 2. 发布 GPT-4o，且免费使用，text、Voice、Video、Image 集成于一体

GPT-4o（“o”代表“全能”），O is `OmniModel`

它可以接受任意组合的文本、音频和图像作为输入，并生成任意组合的文本、音频和图像输出。

它可以在 232 毫秒内响应音频输入，平均为 320 毫秒，这与人类在对话中的响应时间相似。

在英文和代码文本方面，它与 GPT-4 Turbo 的性能相当，对非英语文本的文本有显著改进。

#### 3. GPT-4o，API 速度提高 2 倍，便宜 50%，与 GPT-4 Turbo 相比，速率限制提高 5 倍

> GPT-4o 的最高速率限制将增加到 GPT-4 Turbo 的 5 倍——每分钟最多 1000 万个。

#### 4. 语音助手多人实时演示：

> 主要演示了与 ChatGPT 实时对话，不仅能“听”，还能“看”，当然能“思考”和“回复”

> 多模态识别可以让 ChatGPT 和你一起解数学题

> ChatGPT Desktop App，可以“看到”电脑屏幕，辅助代码生成、分析图表

> 充当翻译助手，实时翻译，快速准确

> 还能识别真人的表情

#### 5. 何时可用？

> GPT-4o 的文本和图像功能今天开始在 ChatGPT 中推出。

> （有人已经更新显示 GPT-4o 了）

> 将在免费版中提供 GPT-4o，并为 Plus 用户提供高达 5 倍的消息限制。

> 将在未来几周内在 ChatGPT Plus 中推出带有 GPT-4o 的 Voice Mode 的新版本。

> 开发者现在也可以通过 API 访问 GPT-4o 作为文本和视觉模型。

> 最后我用 Open AI 的使命结尾，他们真的是有推动全人类前进！

> Our mission：To ensure that artificial general intelligence benefits all of humanity.

> 我们的使命：确保通用人工智能造福全人类。

**GPT-4o 截图：**

![](img/a9a08a87a49dd9914842cbffba1361a3.png)

![](img/f37a2fda3599874b13fdb68313e5cf4f.png)

**GPT-4o 实测：**

![](img/bb3e4684af3e96ea37b02cedd49b9a02.png)

**演示视频：**

![](img/75fdaa047c5f66b597e829edd2b38ce6.png)

[20240514-025713.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JnYnbx1GuoRtzlxIjpNcFCd8neh)

# 5月13日

## AI应用

### 1、GPT 搜索引擎原型曝光！新模型 GPT4-Lite 驱动，代码已上传 ChatGPT

**一句话评论：**

> 首页目前还很简洁，像谷歌一样只有一个居中的搜索框。

**更多可看：**

> https://mp.weixin.qq.com/s/1eFWHa8hcP8E_eUy7-Dh1A

### 2、宇树科技发布 Unitree G1 机器人，9.9 万人民币起步

**一句话评论：** 全身 23~34 个关节非常灵巧，对比马斯克的 optimus 的手部 11 个 dof，G1 仅有 7 个，还有进步空间

**更多可看：**

> https://mp.weixin.qq.com/s/RGNVRazZqDn3y_Ijemc5Kw

### 3、零一万物发布千亿参数模型，海外产品收入将超 1 亿

**一句话评论：** 李开复要实行“开源 + 闭源”的双轨模型策略：以开源模型构建生态、以闭源模型展开 AI-First 商业探索。

**更多可看：** https://mp.weixin.qq.com/s/I0piwAf88WARXmwdk6_gIA

## AI技术

### 1、Diffusion2GAN：单步图像生成

**一句话评论：**

> 将复杂的多步骤扩散模型简化为单步骤条件生成对抗网络（GAN）的方法，提高推理速度，同时保持图像质量。

**更多可看：**

> https://mingukkang.github.io/Diffusion2GAN/

### 2、Sora 是世界模拟器吗？全球首篇综述全面解析通用世界模型

**一句话评论：**

> 260 余篇文献，详尽分析世界模型在视频生成、自动驾驶、智能体、通用机器人等领域的研究和应用

**更多可看：**

> https://mp.weixin.qq.com/s/rkq7NXGvB0O1Kstd_mTNJQ

### 3、给文字动画注入语义灵魂！港科大开源「文字跳动」技术

**一句话评论：** 让文字活起来，象形文字很有优势啊

**更多可看：** https://mp.weixin.qq.com/s/OKB0oYNQMmEfgbC5q57S4Q

解读全国首例“AI 视听作品侵权案”：AI 生成视频，版权到底归谁？

https://mp.weixin.qq.com/s/Z4WkHji4U-2GiBbyzq5N6w

OpenAI 否认加入的 AI 搜索已是一片红海！Stack Overflow 数据用于 AI 训练再次引发争议！

https://mp.weixin.qq.com/s/ebU96QfQFwPYkRTomXY61Q

大神 Karpathy 强推，分词领域必读：自动钓鱼让大模型“发疯”的 token，来自 Transformer 作者创业公司

https://mp.weixin.qq.com/s/UpkgRhZkK45gAPWOOmEwYQ

# 5月12日

## AI应用

### 1、苹果押宝生成式 AI，将在新一代设备上基于大模型大改 Siri

**一句话评论：**

> 期待 6 月 10 日的 WWDC 看苹果如何展示新的、多功能的 Siri。

**更多可看：**

> https://www.nytimes.com/2024/05/10/business/apple-siri-ai-chatgpt.html

### 2、GeoSpy：根据照片定位你在哪里，精确到经纬度

**一句话评论：** 根据照片的光线、植物、建筑风格、天气，并将它们与大量街景/地理照片进行对比，据说也用到了地理空间视觉大语言模型的技术。

**更多可看：**

> https://mp.weixin.qq.com/s/MIECFiew16Eeox_1Mki2IQ

### 3、AniTalker 肖像生成谈话动画

**一句话评论：** 将静态肖像和输入音频转换成自然流畅的谈话视频动画，同时能产生不同的面部表情和姿势。

**更多可看：** https://x-lance.github.io/AniTalker/

## AI技术

### 1、智谱 GLM3-turbo 百万 tokens 低至 1 元！

**一句话评论：**

> 创作 一万条 📕 文案（以 350 字 计）将仅需约 1 元钱，还要啥自行车？

**更多可看：**

> https://mp.weixin.qq.com/s/42hA4Ggzu_p4Iqk8yr4VAQ

### 2、DiT 架构大一统：一个框架集成图像、视频、音频和 3D 生成

**一句话评论：**

> 基于 Diffusion Transformer（`DiT`）的「`Flag-DiT`」，这次要将图像、视频、音频和 3D「一网打尽」。

**更多可看：**

> https://mp.weixin.qq.com/s/NwwbaeRujh-02V6LRs5zMg

### 3、Vidur：LLM 推理模拟器

**一句话评论：** 结合实验分析和预测建模来模拟 LLM 操作的性能，通过估计延迟和吞吐量等关键指标，评估不同工作负载的端到端推理性能。

**更多可看：** https://arxiv.org/pdf/2405.05465v1

Google 开发了一种名为 TimesFM 的时间序列预测模型，可预测未来

时间序列预测就是根据过去发生的一系列事件（比如每天的气温或股票价格）来预测未来事件的技术。TimesFM 利用了大量历史数据来“学习”这些事件的模式和趋势，从而在没有额外信息的情况下，对新的、未见过的数据做出准确的预测。

Anthropic Claude 提示生成器

Anthropic Claude 使用链式思考推理等先进的提示工程技术，帮助用户生成有效、精确、可靠的提示，提高工作效率和输出质量。Anthropic 提示生成器支持动态变量插入，便于用户在不同情境下测试提示。

https://console.anthropic.com/login

**Llama3 中文综合资源库：**

Llama3 中文聊天项目综合资源库，集合了与 Llama3 模型相关的各种中文资料，包括微调版本、有趣的权重、训练、推理、评测和部署的教程视频与文档。
