# Lora_style_1

> 来源：[https://ai.feishu.cn/docx/PsJfdP7G7o34WFxnfmEc7w9EnVe](https://ai.feishu.cn/docx/PsJfdP7G7o34WFxnfmEc7w9EnVe)

### 实验信息

训练数据来源猫和老鼠

大概五秒左右的数据片段.最终生成47672个训练样本，

这次样本抽取训练集中的test_2* 序号相关的数据，大概3000左右的训练样本

![](img/c94cc6f98c5373293856ca03ffd5a49e.png)

[cogvideox_2b_lora.yaml](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/W8yIban80o7qWsxibwCcmVvRn5d)

![](img/f94946b6c96205030bea05ba9a9019ea.png)

[sft.yaml](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UwB3bFMWuoP5Kcxh01YcCUedntg)

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