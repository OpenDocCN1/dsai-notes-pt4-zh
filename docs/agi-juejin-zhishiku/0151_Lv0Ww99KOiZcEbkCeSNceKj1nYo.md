# ShareGPT4V作者团队又一力作！百万高质量视频-字幕数据助力社区提升多模态大模型视频理解及生成能力

> 来源：[https://agijuejin.feishu.cn/docx/A4gVda65KoynHdxImzCcEwhongh](https://agijuejin.feishu.cn/docx/A4gVda65KoynHdxImzCcEwhongh)

> 机器之心｜[阅读原文](https://www.jiqizhixin.com/articles/2024-06-20-4)

> 转载请联系原作者取得授权

![](img/13776c05791c2f4ff7de85f6e9622484.png)

继Sora官宣之后，多模态大模型在 视频生成 方面的应用简直就像井喷一样涌现出来，LUMA、Gen-3 Alpha等 视频生成 模型展现了极佳质量的艺术风格和视频场景的细节雕刻能力，文生视频、图生视频的新前沿不断被扩展令大家惊喜不已，抱有期待。

最近，来自中国科学技术大学、 北京大学 、上海 AI Lab等团队的研究人员发布了引人瞩目的 ShareGPT4Video系列，旨在提升视频理解和生成能力。

![](img/f5cf365a15b1d1636cbda66887f90950.png)

![](img/20a0b2d7aad6fcee8039363ac5714d97.png)

+   论文链接: https://arxiv.org/abs/2406.04325v1

+   项目链接：https://sharegpt4video.github.io/

+   数据集链接：https://huggingface.co/datasets/ShareGPT4Video/ShareGPT4Video

+   代码链接: https://github.com/ShareGPT4Omni/ShareGPT4Video

+   Demo链接: https://huggingface.co/spaces/Lin-Chen/ShareCaptioner-Video

在过去半年中，图像-语言多模态领域在ShareGPT4V的高质量图像-字幕数据集的推出后逐渐意识到详细、准确的图像-字幕数据对于对齐图像与语言模态的重要性。ShareGPT4V数据集推出至今已在HuggingFace平台的VQA dataset track上获得了历史第二高的点赞数。

建立在高质量的ShareGPT4V数据集上，图像理解和 图像生成 社区也都取得一些突破性的进展，例如InternVL-Chat-V1.5与PixArt-Σ等工作。

![](img/6f8d39d2556e10bc82db77bbc0e66859.png)

受ShareGPT4V数据集在图文多模态领域的成功所鼓舞，原作者团队把目光再次投向视频多模态领域。视频多模态领域中闭源商业模型一直处于断层领先的地位，一方面，OpenAI和谷歌近期接连的两场发布会，把AI视频推理卷到了新高度。另一方面，OpenAI的Sora文生视频模型则把文生视频带到了一个全新的高度。

研究者们认为闭源模型对于视频理解和 视频生成 领域的巨大领先同样离不开详细高质量的视频-字幕数据。因此，该研究团队再次致力于为视频获取大量详细而精确的字幕，提升大型视频 语言模型 的视频理解能力和文生视频模型的 视频生成 能力。

目前，该研究在HuggingFace的6月7日Daily Papers中位居榜首，并且在代码公布后迅速获得500+ Star，得到了国内外的一致关注。

![](img/46536b9c71cf4b83dc4464d1d2629f18.png)

研究者们认为用现有的闭源模型生成高质量视频描述的挑战有三个方面:

+   清晰地理解帧间的时序变化。

+   详细准确地描述帧内内容。

+   对任意长度视频的可扩展性。

为此，研究者们精心设计了一种差分滑窗视频描述（Differential Sliding-Window Captioning, DiffSW）策略，该策略可以稳定且高效地为任意分辨率，宽高比和长度的 视频生成 高质量描述。

![](img/8507917db3d4571675d33f228f680897.png)

图 1：差分滑动窗口视频描述生成

具体而言，研究者们每次送入GPT4V的输入是当前关键帧，上一关键帧以及上一关键帧对应的差分描述，旨在让GPT4V根据观察两帧之间的时间与空间变化总结出当前帧相对于上一帧的重要空间、时序变化，即当前帧与上一帧对应的差分描述。最终，所有差分描述会连同时间戳一起送入GPT4中从而总结出最终的关于整个视频的高质量字幕。

该研究团队展示了几个示例：

+   Caption 1:

The video segment documented a significant event in Kochi, Kerala, where 2 buildings razed in Kochi. The broadcast began with a split-screen presentation: on one side, thick clouds of dust were seen billowing into the sky, marking the onset of the demolition process, while on the other side, reporter Gopikrishnan provided live coverage, indicated by "BREAKING NEWS" captions and a consistent timestamp of "11:10 AM." The news ticker at the bottom of the screen simultaneously ran other global events, maintaining a flow of information. As the video progresses, the split-screen footage of the razed house turns into a close-up. A notable change in the headline to "KOCHI FLATS RAZED" signaled the demolition's culmination. A brief interlude offered a visual contradiction by showcasing the flats presumably before their demolition, providing a stark before and after comparison. As the video progressed, the left building's collapse initiated a dramatic alteration in the skyline, marked by significant dust plumes. Subsequently, another building was shown partially collapsing amid debris, fully obscured by dust in seconds, with surrounding greenery remaining untouched. This transitioned into a graphic interlude featuring the "India Today" logo, briefly pausing the live footage. Resuming to the aftermath, split imagery displayed the rubble and ongoing smoke. Then, the imagery continued to juxtapose the scenes of destruction against intact high-rise buildings nearby. The narrative was augmented by the revelation that the Supreme Court directed the demolition within a broader national news context. Throughout, the report maintained a real-time approach, threading continuity and urgency across the unfolding event's documentation.

![](img/d7f2f2aa52680ebe50225c8669b56cff.png)

+   Caption 2:

The video begins with an individual seated on a gray couch in a cozy domestic setting, about to unbox a product from a red CCM-branded box placed on a white table in front of them. Initially, the person is seen drinking from a blue can, indicating a casual atmosphere. Soon after, the individual shifts attention from the can to the red box, signifying the start of the unboxing process. The red box, initially closed, gradually becomes the focal point as the person prepares to open it, conveying a build-up of anticipation. As the video progresses, the box is flipped over and then opened, revealing its content still hidden under white tissue paper adorned with prints, adding to the suspense. The individual’s engagement with the box evolves, from initially preparing to open it, to actively delving into its contents. A momentary pause in activity is captured before the anticipation culminates with the individual lifting an object from the box. This object, identifiable by a yellow label, is then examined closely by the person, indicating a thorough inspection or perusal of the product or its packaging. Throughout the video, the surrounding environment remains consistent and undisturbed, with household items like a potted plant and a wall clock maintaining the setting's homely ambiance. The camera’s perspective remains fixed, focusing on the unfolding unboxing event without any movement, thus allowing the viewer to observe the narrative closely. Another partially open brown box is visible beside the main red box, though its role or contents are not elaborated upon. The video encapsulates the anticipation, action, and reveal inherent to unboxing experiences in a home setting.

![](img/659b935ca916e18dea62b8f144836132.png)

通过这一方法，研究者们推出了大型“视频-文本描述”数据集--ShareGPT4Video数据集，其中包括4万条（共291小时）由GPT-4V标注的视频数据。这些数据涵盖了广泛的类别，生成的描述包含丰富的世界知识，对象属性，摄像机运动，以及详细和精确的事件时间描述。

![](img/6f6bfca4c6c852fe3cdec813fdaf69f3.png)

图 2 ：（a）数据集涵盖广泛的内容，包括野生动物、烹饪、体育、风景、第一人称人类活动、自动驾驶场景等。(c) 字幕的字数主要在 200 到 400 之间，提供了丰富的时间信息，可以很好地完成视频理解和生成任务。

在ShareGPT4Video数据集的基础上，为了进一步扩大数据集规模以及便于开源社区在自有数据上的使用，研究者们进一步设计开发了ShareCaptioner-Video，一个能够有效地为任意 视频生成 高质量描述的多功能多模态大模型。

![](img/8864ef80bc5ca7b95f4d48d006fdf159.png)

图 3：ShareCaptioner-Video 是一款四合一的特殊视频描述模型，具有以下功能：滑动窗口生成视频描述、快速生成视频描述、视频片段对应描述整合，提示词生成详细描述

具体而言，滑窗视频描述功能可以担任GPT4V收集标注数据中的全部角色，并且通过滑窗的方式来产生差分描述并汇总出最终的字幕。快速视频描述功能则是把所有关键帧沿竖直方向拼成一张长图一次性产生最终的字幕，在略微牺牲性能的情况下大幅提升标注速度。视频片段总结功能则可以在对完整视频进行一次滑窗描述后，对其中任意的视频片段直接总结出字幕而不需要再次进行滑窗描述过程。

在得到了优异的视频描述模型后，研究者们用它进一步标注了480万条，总时长3000小时的丰富的视频数据。这些视频具有较高的美学评分以及较少的转场效果，旨在为 视频生成 任务服务。

![](img/8e4b4cfc295b944f4c8cf76979941963.png)

表1：由 ShareCaptioner-Video 标注的480万条视频数据的构成

**实验**

在视频理解方面，研究者们首先通过简单的等量替换实验，验证了ShareGPT4Video数据集在几种当前LVLM架构上的有效性。研究者们把VideoChatGPT数据集中100K视频训练数据中的与详细caption相关的28K数据等量替换成ShareGPT4Video数据集中的子集。从下表可以看到，通过简单的数据替换，仅仅是字幕数据质量上的提升便可以一致地为不同架构、不同规模的视频理解多模态大模型带来显著的性能增益。

![](img/0c3d7d6d2f832659705234543ed42478.png)

表 2：ShareGPT4Video数据集在各模型架构上均能产生性能增益

之后，研究者们自主收集了153K的视频VQA数据，并结合ShareGPT4Video数据集中与视频理解相关的28K高质量字幕数据，提出了新的LVLM ShareGPT4Video-8B。仅需8卡以及5个小时的训练开销，即可在多项Benchmark上取得优异的结果。

![](img/f98063fd4174ced753454a4b23741277.png)

表 3 ：TempCompass上性能对比

![](img/e046d3fa42329485675f5aec4fcd661c.png)

表 4 ：VideoBench上性能对比

![](img/488050a6bba69a8a76d13a9d33ed5404.png)

表 5：MVBench上性能对比

即使是在最近新出现的几个视频理解 基准 上，ShareGPT4Video-8B也可以在7B 参数 规模上一致地展现出具有竞争力的性能。

![](img/945d35a163b17ccad84b0e56d303029a.png)

表 6 ：LongVideoBench上性能对比

![](img/d18fcde2c31d3642a5a31d88d3ad0318.png)

表 7 ：Video-MME 基准 性能对比

![](img/cf6be58f04b4ce7ee8617d30ec47e809.png)

表 8：MMBench-Video 基准 性能对比

在 视频生成 方面，研究者们基于Open-Sora-Plan项目简单直接地验证了详细的字幕数据对于文生视频模型的帮助。下图中，第一行的结果是使用了短字幕数据训练出的文生视频模型得到的，第二行的结果是使用了ShareCaptioner-Video标注的高质量字幕数据训练出的文生视频模型得到的。可以看到，使用详细的字幕数据可以让文生视频模型具备优异的镜头移动控制以及语义内容控制能力。

![](img/489d75918cd1ec5c5f750b5aaf030976.png)

![](img/8c9af9574648cd66e290eb814bdfd4f5.png)

参考链接：

【1】Open-Sora-Plan：https://github.com/PKU-YuanGroup/Open-Sora-Plan

【2】LongVideoBench：https://longvideobench.github.io/

【3】Video-MME：https://video-mme.github.io/

【4】MMBench-Video：https://github.com/open-compass/MMBench-Video