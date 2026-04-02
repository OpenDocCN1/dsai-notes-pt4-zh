# Lora_role_1_EN

> 来源：[https://ai.feishu.cn/docx/FPgKdP0ErovGmRxbHdRcIB8Wnrh](https://ai.feishu.cn/docx/FPgKdP0ErovGmRxbHdRcIB8Wnrh)

ZH:[Lora_role_1](https://zhipu-ai.feishu.cn/wiki/NiJHwKIcnilKmrk3HJ7c9m2knM9)

### Experiment Information

Training data source: Bilibili, UID:

Original material: vertical video, processed into widescreen material, approximately 4-second data clips. A total of 121 training samples were generated.

![](img/fcf12763e92aaa0a7aec0f333de76dd2.png)

[sft.yaml](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GfdFbViJ3oWHBSxNXhFc7o5HnZg)

![](img/671f33a0d86018ec88fafd8ff573c9dd.png)

[cogvideox_2b_lora.yaml](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LKJdbUXrco1UM9xdpLWcOaTjnOd)

#### Huggingface Diffusers Lora Weights

#### Results Evaluation

Three well-performing original prompts and four training sample prompts were extracted. Inference was performed on the model with and without loading the Lora weights. According to the results, although Lora improved the model's performance on new samples, it somewhat affected the original prompt dimensions.

#### **New Task**

Increase the number of original data samples for the original prompts and load them into the target training samples. By adjusting the mixing ratio, find a reasonable method of operation.

Add image conditioning training.

### Sample data

多维表格（IluLbtLM8aaZqjshNokc5s4xnuh_tblv3jsOihic4nuV）