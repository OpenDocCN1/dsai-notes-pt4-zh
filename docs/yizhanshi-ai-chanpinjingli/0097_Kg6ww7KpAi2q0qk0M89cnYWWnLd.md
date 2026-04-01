# Lora_role_3

> 来源：[https://v11enp9ok1h.feishu.cn/docx/M3i9dAoccoXk9xxEZyLclUcqnbb](https://v11enp9ok1h.feishu.cn/docx/M3i9dAoccoXk9xxEZyLclUcqnbb)

### 实验信息

训练数据来源B站,UID:

原素材竖屏.处理成宽屏素材,大概3秒左右的数据片段.最终生成166个训练样本

基于[Lora_role_2](https://zhipu-ai.feishu.cn/wiki/Cscyw20uXiFBF6kC14zcpCWwn7d)**最后一次训**练权重继续训练，这次任务目标是为了让人物收敛，并没有成功

###

![](img/b9704736a2d6123ceb399d39fce414ef.png)

[sft.yaml](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/B7ZnbsLW5oNLNOxyibocZkVfnkS)

![](img/d5d2898a1e24e421d2096b7d11a75a6c.png)

[cogvideox_2b_lora.yaml](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BhHnbWGv0odVeLxAhISc4r1znDh)

#### Huggingface Diffusers  Lora权重

#### 结果评价

这次从训练评测上看，同样的数据集继续训练并没有完成人物，反而画面无法辨别

经过检查，发现是原数据集描述的人物不精确，存在很多嘈杂的提示词，

显然训练样本的提示词中新增一个用户标识

没有很好的拟合

#### 新的任务

为了让提示词中只包含人物信息，而不会存在其它关于嘈杂环境的描述，设计了下面几个COT思维链过程

caption_chunk_gpt_compare用于判断视频描述与视频内容是否符合，筛选掉一些明显不存在人物信息的数据

```

    video_description_prompt = f&quot;&quot;&quot;视频描述：\&quot;{caption_summary_text}\&quot;

请执行以下任务：

1. 判断视频中是否包含人物信息。

2. 比较视频描述与视频内容，判断视频描述是否准确反映了视频的内容。

如果视频描述符合上述任务要求，请输出：\&quot;符合\&quot;。

如果视频描述不符合上述任务要求，请输出：\&quot;不符合\&quot;。

请直接输出每个任务结果，不要在回复中提及变量名或额外的信息。
Step by Step Decomposition&quot;&quot;&quot;
```

check_gpt_compare由于caption_chunk_gpt_compare使用了Step by Step Decomposition ，我们需要将描述任务转换成分类任务，输出是或者否

```
video_description_prompt = f&quot;&quot;&quot;任务描述：

给定以下文本内容：


{compare_text}


请执行以下操作：

- 判断文本中是否包含关键词 &quot;符合&quot;。
- 如果包含，请输出：&quot;是&quot;。
- 如果不包含，请输出：&quot;否&quot;。

请直接输出结果，不要包含额外的解释或信息。&quot;&quot;&quot;
```

caption_chunk_gpt_role_insert最后判断分类为是的内容，根据最佳的聚类镜头描述修改视频描述，使其带上角色信息

```

    video_description_prompt = f&quot;&quot;&quot;Script Information: {features_caption}

Video Description: &quot;{caption_summary_text}&quot;

Please perform the following tasks:

1. Identify the characters present in both the video and the video description.

2. Compare the video description with the video content and extract the characters from the script to compile a list of roles.

If the video description meets the above requirements, please add the character names to the video description.

If the video description does not meet the above requirements, please output: &quot;Does not meet&quot;.

Please output the result directly and do not mention variable names or additional information.

Do not include &#x27;\\n&#x27; and the word &#x27;video&#x27; in your response.

Do not use introductory phrases such as: &quot;The video presents&quot;, &quot;The video depicts&quot;, &quot;This video showcases&quot;, &quot;The video captures&quot;, and so on.

Please start the description with the video content directly, such as &quot;A man first sits in a chair, then stands up and walks to the kitchen....&quot;

Do not use phrases like: &quot;as the video progressed&quot; and &quot;Throughout the video&quot;.

Please describe the content of the video and the changes that occur, in chronological order.

Please keep the description within 100 English words.&quot;&quot;&quot;
```

#### 关联任务