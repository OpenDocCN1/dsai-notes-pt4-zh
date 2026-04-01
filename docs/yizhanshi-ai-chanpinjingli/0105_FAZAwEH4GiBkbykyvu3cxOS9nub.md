# Lora_role_1_EN

> 来源：[https://v11enp9ok1h.feishu.cn/docx/GAmedSI2moCJdKxR4qBcxp20nHc](https://v11enp9ok1h.feishu.cn/docx/GAmedSI2moCJdKxR4qBcxp20nHc)

ZH:[Lora_role_1](https://zhipu-ai.feishu.cn/wiki/NiJHwKIcnilKmrk3HJ7c9m2knM9)

### Experiment Information

Training data source: Bilibili, UID:

Original material: vertical video, processed into widescreen material, approximately 4-second data clips. A total of 121 training samples were generated.

![](img/615cdd74e1a58d539e0d8a35fd2fcbb5.png)

[sft.yaml](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GFSnb3E0AoacZqxZeSTcqzGjnWb)

![](img/5776e3d9a624f6cb7e732b71f70faca3.png)

[cogvideox_2b_lora.yaml](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Lzv5bjrOZoUZfTxJv0JcCDFPnzc)

#### Huggingface Diffusers Lora Weights

#### Results Evaluation

Three well-performing original prompts and four training sample prompts were extracted. Inference was performed on the model with and without loading the Lora weights. According to the results, although Lora improved the model's performance on new samples, it somewhat affected the original prompt dimensions.

#### **New Task**

Increase the number of original data samples for the original prompts and load them into the target training samples. By adjusting the mixing ratio, find a reasonable method of operation.

Add image conditioning training.

### Sample data

多维表格（HCqzbl84saPBtusRPeTcymZsn2f_tblNoDPwFc2hejOS）