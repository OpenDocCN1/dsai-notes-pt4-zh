# Lora_role_1

> 来源：[https://ai.feishu.cn/docx/OvTgdEOojo1CQxxxPF1ceJtengg](https://ai.feishu.cn/docx/OvTgdEOojo1CQxxxPF1ceJtengg)

EN:[Lora_role_1_EN](https://zhipu-ai.feishu.cn/wiki/IYFowSbDQiJ5zekPZLDc6TH7ncf)

### 实验信息

训练数据来源B站,UID:

原素材竖屏.处理成宽屏素材,大概四秒左右的数据片段.最终生成121个训练样本

![](img/36fefc1bef7b58d5d78d266fe097e415.png)

[sft.yaml](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HTKrbzsnAoJYJsxOcb0cHA22n6c)

![](img/6aea9bbd2135a25a2173b3c3810f40ea.png)

[cogvideox_2b_lora.yaml](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BUx9bFtgHoLblMxCzAYc8VQEnFb)

#### Huggingface Diffusers  Lora权重

#### 结果评价

抽取了三个原始表现不错的指令,和四个训练样本指令,分别对加载lora前后的权重进行生成推理,根据结果来看Lora虽然提升了新样本的能力,但或多或少影响到了原来的指令维度

#### 新的任务

增加要保持原来指令的原始数据样本,加载到目标训练样本中,通过扩大混合比例找到一个合理的操作方式

增加图片条件训练

#### 关联任务

[Lora_role_2](https://zhipu-ai.feishu.cn/wiki/Cscyw20uXiFBF6kC14zcpCWwn7d) 扩大数据，增加分片视频首帧和尾帧的描述，总结之后作为训练提示词

### 抽样数据

多维表格（CAc8b5zAGaA6h7s163OcxxNHnud_tblxCpoKCTL0Ss7B）