# Lora微调CogVideoX-风格对齐

> 来源：[https://v11enp9ok1h.feishu.cn/docx/V4l5dhaBbogLLax3GZnc0uA8nEf](https://v11enp9ok1h.feishu.cn/docx/V4l5dhaBbogLLax3GZnc0uA8nEf)

我们调研了2024年近两年技术，编写了一份调研报告[视频生成技术调研](https://zhipu-ai.feishu.cn/wiki/SheQwaQDXixgEckwxwZcruZpnIe)从技术方向到收费模式的统计都在这，通过 我们可以发现transformer这个技术的推进明显在可玩性上有些欠缺，为了进一步增加我们cog系列玩法的覆盖，比如深度图、草图、风格转换、物体交互等领域，又或者提升整体视频生成稳定可控的视觉效果上，这些都是我们需要做的事情

### 风格对齐能力

[CogVideoX探索Lora微调风格场景](https://zhipu-ai.feishu.cn/wiki/Ln9dw9ohpiFymekjeabc8TTinRd)了推进视频生成在稳定和可控的结果，我们在 方向上做了一些事情，主要在数据集收集、生成、微调方案上尝试了一些事情

#### 社区支持情况

目前开源社区中，CogViideoX在资源方向上diffusers框架支持了推理加速、低资源推理微调，

在更多的玩法上[Tora](https://github.com/alibaba/Tora)拓展了运动点跟随模块，[Interpolation](https://github.com/feizc/CogvideX-Interpolation)支持了首尾帧拼接，[CogVideoX-Fun](https://github.com/aigc-apps/CogVideoX-Fun)支持了动作姿态演示。

[在Com](https://github.com/kijai/ComfyUI-CogVideoXWrapper)fyui生态中我们还有更加便捷的包装节点，社区中的一些视频到视频、姿态到视频都有完整的流程图演示

在[低资源适配中，采样fp8 ](https://github.com/thu-ml/SageAttention)j精度下，我们可以做到2B模型仅需4G显存运行推理，5B仅需9G显存，这些都基于sageattention 的支持(Linux only) ，通过此方法你可以减少~20-30% 的资源,

在多策略中陆续也有一些作者对我们的模型进行了LoRAs和ControlNet的微调开源

### 消融实验

在[lora消融实验](https://zhipu-ai.feishu.cn/wiki/JqWnwca2LiPfJXkwTqlcE0RGnmf)中我们尝试了人物形象和场景风格这两种人物的混合比例情况

#### 人物形象（IP）角色对齐任务

实验总结：关于同一个人物的精细描述 46条训练样本， 场景以及大致信息描述 166个训练样本，1e-3可以学习率 1000-2000步可以完成复现人物形象

#### 场景风格（画风）对齐

实验总结：数据集包含大概6000条场景信息，

lr: 1E-3    betas: [ 0.8, 0.95 ]  eps: 1e-8 weight_decay: 1e-4

迭代4000步后，所有生成内容会趋向目标样本

#### 图像生成视频-人物形象微调

[Lora_image](https://zhipu-ai.feishu.cn/wiki/OQ5vwxH9rioQ7vkdG9bcTOxZndc)在[Lora_role_5](https://zhipu-ai.feishu.cn/wiki/AzP8wLht7i9daikuzVHcCfNwn1d)抽取数据集46条，[Lora_role_2](https://zhipu-ai.feishu.cn/wiki/Cscyw20uXiFBF6kC14zcpCWwn7d)中166个训练样本上面，通过num_train_epochs 150次迭代，完成了训练过程

调[视频指令维度](https://zhipu-ai.feishu.cn/wiki/Alw9w7kaLim5F7kk3kpc2szBnoc)了lora_alpha，按照顺序在lora_alpha值为1、16、32上分别训练了一些step所有的参数样本都已经趋近目标数据，在 上能很好的泛化相关的形象，例如主题一致、动态信息、背景一致、对象信息、运动平滑、颜色、场景、外观样式、时态样式都有不错的效果