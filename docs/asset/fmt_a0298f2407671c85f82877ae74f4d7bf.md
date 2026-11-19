
# Lora_role_3

> 来源：[https://ai.feishu.cn/docx/CszqdMzERou4odxAqSRcqKKFngg](https://ai.feishu.cn/docx/CszqdMzERou4odxAqSRcqKKFngg)

## 实验信息

训练数据来源B站,UID:

原素材竖屏.处理成宽屏素材,大概3秒左右的数据片段.最终生成166个训练样本

基于[`Lora_role_2`](https://zhipu-ai.feishu.cn/wiki/Cscyw20uXiFBF6kC14zcpCWwn7d)最后一次训练权重继续训练，这次任务目标是为了让人物收敛，并没有成功

![](img/85589a51315966d56fe6d765cc8551e5.png)

[`sft.yaml`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AA9GbznA5o0j8lxBMASc8SiFnZd)

![](img/e50d8482209e2a4dcc93891b7f0b33f6.png)

[`cogvideox_2b_lora.yaml`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Bvt9bQiJRoR2McxW8kMcPkqHnkd)

## Huggingface Diffusers Lora权重

## 结果评价

这次从训练评测上看，同样的数据集继续训练并没有完成人物，反而画面无法辨别

经过检查，发现是原数据集描述的人物不精确，存在很多嘈杂的提示词，

显然训练样本的提示词中新增一个用户标识

没有很好的拟合

## 新的任务

为了让提示词中只包含人物信息，而不会存在其它关于嘈杂环境的描述，设计了下面几个COT思维链过程

### caption_chunk_gpt_compare

`caption_chunk_gpt_compare`用于判断视频描述与视频内容是否符合，筛选掉一些明显不存在人物信息的数据

```
video_description_prompt = f"""视频描述："{caption_summary_text}"

请执行以下任务：

1. 判断视频中是否包含人物信息。

2. 比较视频描述与视频内容，判断视频描述是否准确反映了视频的内容。

如果视频描述符合上述任务要求，请输出："符合"。

如果视频描述不符合上述任务要求，请输出："不符合"。

请直接输出每个任务结果，不要在回复中提及变量名或额外的信息。
Step by Step Decomposition"""
```

### check_gpt_compare

`check_gpt_compare`由于`caption_chunk_gpt_compare`使用了`Step by Step Decomposition`，我们需要将描述任务转换成分类任务，输出是或者否

```
video_description_prompt = f"""任务描述：

给定以下文本内容：


{compare_text}


请执行以下操作：

- 判断文本中是否包含关键词 "符合"。
- 如果包含，请输出："是"。
- 如果不包含，请输出："否"。

请直接输出结果，不要包含额外的解释或信息。"""
```

### caption_chunk_gpt_role_insert

`caption_chunk_gpt_role_insert`最后判断分类为是的内容，根据最佳的聚类镜头描述修改视频描述，使其带上角色信息

```
video_description_prompt = f"""Script Information: {features_caption}

Video Description: "{caption_summary_text}"

Please perform the following tasks:

1. Identify the characters present in both the video and the video description.

2. Compare the video description with the video content and extract the characters from the script to compile a list of roles.

If the video description meets the above requirements, please add the character names to the video description.

If the video description does not meet the above requirements, please output: "Does not meet".

Please output the result directly and do not mention variable names or additional information.

Do not include '\n' and the word 'video' in your response.

Do not use introductory phrases such as: "The video presents", "The video depicts", "This video showcases", "The video captures", and so on.

Please start the description with the video content directly, such as "A man first sits in a chair, then stands up and walks to the kitchen...."

Do not use phrases like: "as the video progressed" and "Throughout the video".

Please describe the content of the video and the changes that occur, in chronological order.

Please keep the description within 100 English words."""
```

## 关联任务
