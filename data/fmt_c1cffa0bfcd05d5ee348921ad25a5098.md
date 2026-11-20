
# 04🈷️04日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/BCo1dvS6uoDR9txc0ZwcnyjXnae](https://v11enp9ok1h.feishu.cn/docx/BCo1dvS6uoDR9txc0ZwcnyjXnae)

> 🔔AI情报助手午报「04月04日」

> ✨✨✨✨✨✨✨✨

## 1️⃣ `R1-Zero`（`GRPO`）的复现实验记录

> 🎯 `R1-Zero`（`GRPO`）的复现实验记录，踩坑问题（小模型）
>
> + 实验在`1.5B`/`3B`小模型上进行，采用`OpenRLHF`框架，`Qwen2.5`模型，`MATH`数据，参考特定指令格式。
> + 踩坑方面，`format`格式奖励遇问题，因奖励函数`pattern`限制严格，调整`pattern`有改善；训练分阶段，小模型超参致异常，调参可减弱。
> + 效果对比上，`Qwen-3B-Base`模型测试，发现无强制`format`奖励效果或更好。
> + 存在问题包括`KL`散度爆炸、`token-level-loss`梯度计算不稳定。
> + 整体围绕小模型复现`R1-Zero`（`GRPO`）展开，分析踩坑与效果。

> 关键词：`R1-Zero`，`GRPO`，小模型，`OpenRLHF`，`format`奖励

> 🔗 文中引用的链接
>
> + [原文链接](https://mp.weixin.qq.com/s/8j6CccqLs08AncIXU66lxQ)
> + [解释链接](https://zhuanlan.zhihu.com/p/1887453157099037961)

## 2️⃣ 困在“第一学历”里的博士

> [困在“第一学历”里的博士](https://mp.weixin.qq.com/s/DGGQDiAnee7PR2t6VE-YLw)
>
> + 教育部多次严禁学历歧视，但校招中“第一学历”限制仍普遍存在，不同学历“明码标价”，技术与非技术岗位表现有别，多数毕业生选择“沉默”。
> + “第一学历”是伪概念，国家政策重申扭转“唯名校”“唯学历”用人导向。
> + 企业人才筛选机制“偷懒”，易致学历与岗位不匹配，增加招聘成本。
> + 两会建议规范就业歧视定性，严肃问责歧视单位，健全以能力为导向的人才评价标准。
> + 博士应聘高校教职也可能因“第一学历”被拒，应避免“学历至上”，做到人尽其才。

> 关键词：第一学历，学历歧视，就业政策，人才评价，校招

> 🔗 文中引用的链接
>
> + [困在“第一学历”里的博士](https://mp.weixin.qq.com/s/DGGQDiAnee7PR2t6VE-YLw)

## 3️⃣ 视觉`SSL`终于追上了`CLIP`！Yann LeCun、谢赛宁等新作，逆转`VQA`任务固有认知

> [视觉`SSL`终于追上了`CLIP`！Yann LeCun、谢赛宁等新作，逆转`VQA`任务固有认知](https://mp.weixin.qq.com/s/9TTPcO482U1bn9R5CiP46Q)
>
> + Yann LeCun、谢赛宁等研究者探讨视觉自监督学习（`SSL`）用于多模态建模的视觉表征预训练是否需语言监督。
> + 研究在数十亿规模网络数据上训练`SSL`模型，与`CLIP`公平比较，新`Web-SSL`系列模型仅基于网络图像训练。
> + 实验表明视觉`SSL`在`VQA`任务可匹配甚至超过语言监督方法，且在模型容量和数据方面扩展性良好。
> + `Web-SSL`系列中`Web-DINO`在`VQA`和传统视觉任务表现出色，性能随分辨率提高而提升。
> + 研究证明视觉`SSL`在多模态应用中有巨大潜力。

> 关键词：视觉`SSL`，`CLIP`，`VQA`，`Web-SSL`，视觉表征预训练

> 🔗 文中引用的链接
>
> + [论文链接](https://arxiv.org/pdf/2504.01017)
> + [项目地址](https://davidfan.io/webssl/)

## 4️⃣ 大模型`RL`不止数学代码！`7B`奖励模型搞定医学法律经济全学科，不用思维链也能做题

> [大模型`RL`不止数学代码！`7B`奖励模型搞定医学法律经济全学科，不用思维链也能做题](https://mp.weixin.qq.com/s/0rhuRHz1aYMW4hs8WGHQgg)
>
> + 腾讯与苏州大学团队提出`RLVR`框架，将大模型强化学习扩展到多学科领域。
> + `RLVR`使用基于生成模型的软奖励，相比传统二元规则奖励有显著提升。
> + 团队用`72B`参数模型蒸馏出`7B`奖励模型，且开源奖励模型与多学科数据集。
> + 实验表明，`RM-7B`在自由形式答案任务表现出色，软奖励处理复杂判断更优。
> + 研究未用思维链推理，探讨了相关开放性问题。

> 关键词：大模型，强化学习，`RLVR`，奖励模型，多学科

> 🔗 文中引用的链接
>
> + [论文地址](https://arxiv.org/abs/2503.23829)
> + [`HuggingFace`](https://huggingface.co/collections/virtuoussy/rlvr-67ea349b086e3511f86d1c1f)
> + [1]
> + [https://x.com/tuzhaopeng/status/1906975869538914570](https://x.com/tuzhaopeng/status/1906975869538914570)

## 5️⃣ `DiT`控制新纪元！"即插即控"，Tiamat AI重磅开源`EasyControl`：任意比例出图+推理速度狂飙

> [`DiT`控制新纪元！"即插即控"，Tiamat AI重磅开源`EasyControl`：任意比例出图+推理速度狂飙](https://mp.weixin.qq.com/s/WtistGa-O3ILICrpMS3taA)
>
> + Tiamat AI开源面向`DiT`模型的条件生成框架`EasyControl`，通过创新实现高效灵活的条件控制。
> + 解决计算效率瓶颈、多条件协同困难等问题，提出轻量级条件注入等方案。
> + 应用低秩自适应、分辨率归一化等技术，实现高效推理、灵活控制等效果。
> + 实验表明，该框架在多条件生成、分辨率适应性上表现优异，优于基线方法。
> + `EasyControl`为条件图像生成提供强大且可适应的解决方案。

> 关键词：`EasyControl`，`DiT`模型，条件生成，开源框架，图像生成

> 🔗 文中引用的链接
>
> + [文章链接](https://arxiv.org/abs/2503.07027)
> + [项目链接](https://github.com/Xiaojiu-z/EasyControl)
