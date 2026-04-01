# Lora_role_2

> 来源：[https://v11enp9ok1h.feishu.cn/docx/JkQJdO4aho41OAx25RUcb9ttnCf](https://v11enp9ok1h.feishu.cn/docx/JkQJdO4aho41OAx25RUcb9ttnCf)

### 实验信息

训练数据来源B站,UID:

原素材竖屏.处理成宽屏素材,大概3秒左右的数据片段.最终生成251个训练样本

基于[Lora_role_1](https://zhipu-ai.feishu.cn/wiki/NiJHwKIcnilKmrk3HJ7c9m2knM9)最后一次训练权重继续训练，扩大相关人物数据，增加分片视频首帧和尾帧的描述，总结之后作为训练提示词

![](img/9b2e3f9c58246a191b7081c828baaffe.png)

[sft.yaml](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Ub8kbCDmeooKC9xCRk1cz2fWn1g)

![](img/9e8fde4380870345a29a25dffedcaee8.png)

[cogvideox_2b_lora.yaml](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LFJvbZ8cdoDmktxqgS8cBEA0n7f)

#### Huggingface Diffusers  Lora权重

#### 结果评价

基于抽样的信息来看，样本中的衣服以及人物的基本特征以及学习到，但对于人物面部，依然出现很奇怪的表情，对于需要对固定人物标签来说，看起来需要增加特殊的token词

#### 新的任务

在这次训练样本的提示词中新增一个用户标识

#### 关联任务

[Lora_role_1](https://zhipu-ai.feishu.cn/wiki/NiJHwKIcnilKmrk3HJ7c9m2knM9)

[Lora_role_3](https://zhipu-ai.feishu.cn/wiki/JLlcwckngiA1pzkrtp3ccFLhn0c)

### HF代码加载模型抽样数据

多维表格（BT7yblNMgavystsRuejcx3yLnWg_tblJ9xnixfmh3YOq）