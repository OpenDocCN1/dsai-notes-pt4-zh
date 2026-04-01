# Lora_style_1

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Jltyd46xYodMYWxOn1YcWcxznqd](https://v11enp9ok1h.feishu.cn/docx/Jltyd46xYodMYWxOn1YcWcxznqd)

### 实验信息

训练数据来源猫和老鼠

大概五秒左右的数据片段.最终生成47672个训练样本，

这次样本抽取训练集中的test_2* 序号相关的数据，大概3000左右的训练样本

![](img/c24d8f2a4d3e90088d76dc5073784b29.png)

[cogvideox_2b_lora.yaml](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Mf2XbcZmIoufJMxhE0fcx4rtnPh)

![](img/502735481c7ed15ab38f8b55698a255d.png)

[sft.yaml](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MSnvbLDxEoiQMYxzIoqcYsOWnZc)

#### Huggingface Diffusers  Lora权重

#### 结果评价

发现数据集中人物会出现描述错误，造成了tom会有老鼠的身材，

并且场景并不能很好生成，人物无法正常显示

合并lora后之前一些表现不错的样本，已经无法正常生成

训练的 lr: 4E-3过大，数据无法拟合成功，

#### 新的任务

尝试实验小一些的lr:1E-3训练任务，增大训练步到4000，增多训练数据到6000

#### 关联任务

[Lora_style_2](https://zhipu-ai.feishu.cn/wiki/OdvMwvXoQilYM8kjkFPcJFa3npf)