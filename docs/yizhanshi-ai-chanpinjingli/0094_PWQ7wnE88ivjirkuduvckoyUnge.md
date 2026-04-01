# CogVideoX Lora消融实验

> 来源：[https://v11enp9ok1h.feishu.cn/docx/CsLDdjGgXo6HV0xQugqc26XTnNg](https://v11enp9ok1h.feishu.cn/docx/CsLDdjGgXo6HV0xQugqc26XTnNg)

### 消融实验

#### SAT与HuggfaceDiffusers框架的差别

Diffusers的训练setp跟sat的计算不太一样详细说明在之后的Huggface官方文档可查阅，

SAT每个样本1500次迭代明显比hf的快，但是结果是，相同lora参数和学习率的情况下（因为hf只提供了这个参数修改）sat150次迭代后人物虽然学习到了人物形象，但是原来的样本基本上以及无法生成

Diffusers在迭代到 setp 10000次后已经能很完美的还原人物形象，原来的样本只有光流效果的差别（趋向目标训练样本

[Lora_role_7](https://zhipu-ai.feishu.cn/wiki/RGWowuXx7i6j0RkGEhvcTWMAnjg)[Lora_hf_text_video](https://zhipu-ai.feishu.cn/wiki/MqDEw2dq0iRGdgkS1nycbEDHn8c)差别可参考SAT框架训练任务 ，Diffusers框架训练任务

#### 人物形象（IP）角色对齐任务

实验总结：关于同一个人物的精细描述 46条训练样本， 场景以及大致信息描述 166个训练样本，1e-3可以学习率 1000-2000步可以完成复现人物形象

+   实验一

[Lora_role_1](https://zhipu-ai.feishu.cn/wiki/NiJHwKIcnilKmrk3HJ7c9m2knM9)原素材竖屏.处理成宽屏素材,大概四秒左右的数据片段.最终生成121个训练样本

+   实验二

[Lora_role_2](https://zhipu-ai.feishu.cn/wiki/Cscyw20uXiFBF6kC14zcpCWwn7d)基于[Lora_role_1](https://zhipu-ai.feishu.cn/wiki/NiJHwKIcnilKmrk3HJ7c9m2knM9)最后一次训练权重继续训练，扩大相关人物数据，增加分片视频首帧和尾帧的描述，总结之后作为训练提示词

+   实验三

[Lora_role_3](https://zhipu-ai.feishu.cn/wiki/JLlcwckngiA1pzkrtp3ccFLhn0c)基于[Lora_role_2](https://zhipu-ai.feishu.cn/wiki/Cscyw20uXiFBF6kC14zcpCWwn7d)**最后一次训**练权重继续训练，这次任务目标是为了让人物收敛，并没有成功

+   实验四

[Lora_role_4](https://zhipu-ai.feishu.cn/wiki/VypzwKdaFimXUlkqpHPcxN46nw6)基于[Lora_role_2](https://zhipu-ai.feishu.cn/wiki/Cscyw20uXiFBF6kC14zcpCWwn7d)最后一次训练权重继续训练，这次任务目标是为了让人物收敛, 与实验三结果相同，人物无法识别，尝试从base模型微调人物数据集

+   实验五

[Lora_role_5](https://zhipu-ai.feishu.cn/wiki/AzP8wLht7i9daikuzVHcCfNwn1d)为了让提示词中只包含人物信息，而不会存在其它关于嘈杂环境的描述，设计了下面几个COT思维链过程，抽取数据集46条

+   实验六

[Lora_role_7](https://zhipu-ai.feishu.cn/wiki/RGWowuXx7i6j0RkGEhvcTWMAnjg)在[Lora_role_5](https://zhipu-ai.feishu.cn/wiki/AzP8wLht7i9daikuzVHcCfNwn1d)抽取数据集46条，[Lora_role_2](https://zhipu-ai.feishu.cn/wiki/Cscyw20uXiFBF6kC14zcpCWwn7d)中166个训练样本（处理脚本错误，存在索引错误的文件，删除后剩余166），实验总结：关于同一个人物的精细描述 46条训练样本， 场景以及大致信息描述 166个训练样本，1e-3可以学习率 1000-2000步可以完成复现人物形象

#### 场景风格（画风）对齐

实验总结：数据集包含大概6000条场景信息，

lr: 1E-3    betas: [ 0.8, 0.95 ]  eps: 1e-8 weight_decay: 1e-4

迭代4000步后，所有生成内容会趋向目标样本

+   实验一

[Lora_style_1](https://zhipu-ai.feishu.cn/wiki/GwNxwe93yikH9wkImgqcyIwQnjb)训练数据来源猫和老鼠，抽取训练集中的test_2* 序号相关的数据，大概3000左右的训练样本

+   实验二

[Lora_style_2](https://zhipu-ai.feishu.cn/wiki/OdvMwvXoQilYM8kjkFPcJFa3npf) 训练数据来源猫和老鼠，抽取训练集中的 test_2* 、test_3* 序号相关的数据，大概6000左右的训练样本

+   实验三

[Lora_style_3](https://zhipu-ai.feishu.cn/wiki/SMLJw9lXTi3PEYkWlAycLD9InOA)这次从训练评测上看，基于[Lora_style_2](https://zhipu-ai.feishu.cn/wiki/OdvMwvXoQilYM8kjkFPcJFa3npf)最后一次检查点继续训练，人物形象并没有很好的生成，甚至其它任务都无法生成，不放测试示例了

#### 图像生成视频-人物形象微调

在[Lora_role_5](https://zhipu-ai.feishu.cn/wiki/AzP8wLht7i9daikuzVHcCfNwn1d)抽取数据集46条，[Lora_role_2](https://zhipu-ai.feishu.cn/wiki/Cscyw20uXiFBF6kC14zcpCWwn7d)中166个训练样本上面，通过num_train_epochs 150次迭代，完成了训练过程

调[视频指令维度](https://zhipu-ai.feishu.cn/wiki/Alw9w7kaLim5F7kk3kpc2szBnoc)了lora_alpha，按照顺序在lora_alpha值为1、16、32上分别训练了一些step，所有的参数样本都已经趋近目标数据，在 上能很好的泛化相关的形象，例如主题一致、动态信息、背景一致、对象信息、运动平滑、颜色、场景、外观样式、时态样式都有不错的效果

+   实验一

任务 [Lora_image](https://zhipu-ai.feishu.cn/wiki/OQ5vwxH9rioQ7vkdG9bcTOxZndc) 在[Lora_role_5](https://zhipu-ai.feishu.cn/wiki/AzP8wLht7i9daikuzVHcCfNwn1d)抽取数据集46条，[Lora_role_2](https://zhipu-ai.feishu.cn/wiki/Cscyw20uXiFBF6kC14zcpCWwn7d)中166个训练样本上面，通过num_train_epochs 150次迭代，完成了训练过程