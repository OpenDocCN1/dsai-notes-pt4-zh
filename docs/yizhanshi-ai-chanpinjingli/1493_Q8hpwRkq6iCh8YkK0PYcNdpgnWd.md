
# model_clm

> 来源：[https://v11enp9ok1h.feishu.cn/docx/HUJtdyRcgo45xkxppXccrjnpnKe](https://v11enp9ok1h.feishu.cn/docx/HUJtdyRcgo45xkxppXccrjnpnKe)

## 介绍

1. 这个文件夹，基于吾道数据，对`bloom`模型做中文上的预训练。

2. 支持`bloom`全系列模型（不管是560m、还是1b、还是7b）。

3. 只要你可以自己修改模型的初始化部分，都可以训练别的模型。只要是`clm`类型的模型。

4. 数据部分，使用了吾道数据，考虑到吾道数据数据比较大，这里也添加了训练参数。可以使用吾道数据的数据量（也就是文件个数）。

5. 其实所谓的使用无监督数据的预训练步骤和使用指令数据训练做`sft`，90%的代码都是一样的。就是在数据处理部分和`data_collator`部分不太一样。只要仔细对比一下`chinese_bloom`和`chinese_bloom_clm`里面的`train.py`文件即可。

## 2023-06-30日更新

1. 添加流式数据处理模型，当文件数量超过100GB，也不会爆内存、爆磁盘

2. 添加对别的模型的支持，如果`chatglm-v2`、`llama`等架构的模型

3. 支持从0训练一个大模型，支持训练自己的初始化模型

## 数据获取

数据共享——悟道200G数据分享
