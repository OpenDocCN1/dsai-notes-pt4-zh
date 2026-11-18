
# 动手学小说创作模型微调

> 来源：https://datawhaler.feishu.cn/docx/NAmsdX9lxohDGXxpFXrc9AUqnR4
>
> 本次学习基于「酷文」小说创作大模型挑战赛（优酷 x 天池 联合举办），可通过赛事官网报名
>
> 适合对大模型微调，或写作大模型感兴趣的学习者参与，
>
> 学习时间：6.17～7.07
>
> 学习指南将为大家详细介绍从 赛事报名 -> 微调模型 -> 赛事提交 的全部过程，让每一个感兴趣但没有算力或没有相关经验的学习者都能参与其中！
>
> 本次参赛的目标是：
>
> +   选择一个35B或以下的开源模型进行微调，使其能按照给定的 故事梗概 完成800字左右的文本创作任务。
>
> PS：不要忘记填写表单，获取奖励哟~
>
> 更多参赛支持详见[学习规划](https://datawhaler.feishu.cn/wiki/S5bFwE4hRitvJxkZivLcXhmdnlb#doxcncMmyLdN2xkMBVEmb0kD67b)，以及记得学习群的信息～

本次赛事的参赛速通流程大致如下图所示：

## Step0. [报名赛事](https://tianchi.aliyun.com/s/7efe386f2d91342c12dcf355e382ef35)（赛事已结束）

打开[赛事链接](https://tianchi.aliyun.com/s/7efe386f2d91342c12dcf355e382ef35)，登录并点击【报名参赛】

![](img/bff8e897d7129bd4a725d1a2e9fc1fb3.png)

![](img/295aa03d41c53e57898d79efa8e19bcf.png)

![](img/269e62f4893a09b203f74a8c029a2b79.png)

## Step1. 租用 GPU 卡

> 大概需要 2 分钟～

1. 注册[deepln 账号](https://www.deepln.com/)（点击即可跳转），领取赠额, 10 + 20！

![](img/4e79d2c83f3f15d7c6a222fcbc018a61.png)

![](img/86df03b3bf8f4bf662b6e1fc3ece19cb.png)

2. 租用GPU：单卡 RTX3090及以上规格的都可以，微调需要24G 显存，

（推荐使用4090，3090 如果10分钟还没创建成功，换卡）

![](img/357193805518fbdd573f86437758ba06.png)

![](img/719ae21a3fa53a528a89985b1abd02c0.png)

![](img/a49fec838b6059faa773b897bd78b161.png)

3. 连接服务器，在浏览器中使用

![](img/498240a53c49c6706327e943f896e519.png)

![](img/4f58a77f058b65d6cd6da66a3a46efc1.png)

## Step2. 体验一站式 baseline

> 大概需要 20 分钟～

1. 下载下面两个文件

> 参考数据集为赛事官网下载的数据解压内容

![](img/b5034db18de5c04a2ef0f7cf8bff1870.png)

[`参考数据集.json`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JjscbJUPjoMixoxHjzAcQYKUntf)

请使用V2版本, 更新调整了部分内容

![](img/b4ef04a652af6d5647c793e29f489bf2.png)

[`novelModel_baseline_fastrun_V2.ipynb`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Huayb6eAco6NfRxIT8jcrubxngH)

2. 将文件上传至 deepln GPU服务器（step1 租用的那个）

![](img/a1545fc053f4f160ccbc4d2ff92cbaf1.png)

3. deepseekapi申请

+   进入deepseekapi申请页面：https://platform.deepseek.com/api_keys，注册账号

![](img/5fdd6c3711536d52bd054918a346abef.png)

![](img/8a6f94fb3ca9a8eb88acd542065050f2.png)

+   点击左侧【API keys】，点击【创建API key】

![](img/edc4508feb07486819b48c8df9a34395.png)

![](img/2fd34f8117bc900f8c33571eb1f5400b.png)

![](img/ae049d3b18b4df2ea57821eb13daedbe.png)

4. 打开 `novelModel_baseline_fastrun_V2.ipynb`，将第3步获取到的API-key，赋值给`DEEPSEEK_API_KEY`变量

![](img/5fdbe97631f154d13f291477e083b0eb.png)

5. 剩下就只需要一个一个代码块运行啦～跑一次大约需要15min

主要流程为：处理训练数据->下载模型->微调模型->用模型写作 四大步骤，需注意：

+   第一次运行的时候需要选择Python环境，选torch版本即可

![](img/f424017db9389806e5aaee6c1b6a2e44.png)

## Step3. [提交结果](https://tianchi.aliyun.com/competition/entrance/532210/submission/1277)

1. 从服务器上下载 `submit.json`文件

> 这是初步的baseline，只跑了一道题目和输出3个结果，暂不能直接用于提交，后续几天会提供更完善的版本，敬请期待哦。

![](img/fd66077aebfeacb8e70fe4cf50991164.png)

![](img/4f616614814742c82bf86e5bc91a769f.png)

2. 在[官网](https://tianchi.aliyun.com/competition/entrance/532210/submission/1277)提交`submit.json`文件

> 每天可在官网提交三次

![](img/80dade31075c86954d0e386f32d951c3.png)

![](img/6963c5f233c03cab6900573338a6a7f5.png)

![](img/fd8f5b6fec44768570c2b6936b135793.png)

![](img/ca7224850b3ac38ea380719c30f3a59a.png)

![](img/a056f0027e5bfa40c3808d06d1a6bc7e.png)

![](img/fbe9e79a68b29a5bc5320f618c129a3b.png)

3. 关闭服务器租赁【一定一定一定要看，不然号里没钱了都不知道咋没的】

+   退出服务器

![](img/ebd965a3c0d6d73fb1b0d7a63ba45de0.png)

+   点击结束租用，存储数据

![](img/94a6bdc5b108489717b5714e50114dd3.png)

+   可以看到租用服务器已经没有设备，这里代表服务器正常关闭，如果试验完成或有事离开，一定切记关闭设备，否则一直会计入租赁时间，消耗费用。

![](img/7e239e72a98561b8a1a4026eac5300cf.png)

+   数据存储在“我的数据”里面，以后再租用服务器可以直接选择保存的数据，可以方便升降配置。

![](img/727a6b1db524056c5138c0547b26d409.png)

## 记得填写[打卡表单](https://exn8g66dnwu.feishu.cn/share/base/form/shrcnNxQnKNmtj72tTtI5rPT0cf)领取额外奖励！

> 打卡链接：
>
> https://exn8g66dnwu.feishu.cn/share/base/form/shrcnNxQnKNmtj72tTtI5rPT0cf

## 更多学习详情

### 赛事简介

> 自ChatGPT发布以来，人工智能生成内容（AIGC）的创作能力在各个领域展现了惊人的潜力，特别是在文学创作方面。机器辅助写作不再是科幻小说中的设想，而是正在打破想象力的边界，为文字工作带来了全新的可能性。
>
> 然而，AI写作的发展仍然处于初级阶段，面临诸多挑战。如何赋予AI更强大的语言理解能力、情感表达能力以及逻辑推理能力，使其创作出逻辑严密、有新意的内容，是当前开发者们研究的方向。
>
> 在此背景下，优酷联合天池举办「酷文」小说创作大模型挑战赛，旨在激发开发者的兴趣，共同研究如何拓展模型创作能力，推动AIGC在文学创作中的应用。
>
> 单人成队或最多不超过3人组队参赛，基于给定梗概的文本创作任务，参赛者需在参考数据集的基础上，补充数据集，任选35B或以下的开源模型进行训练，完成800字左右的文本创作任务。

### 学习规划

| Task | 任务信息 | 任务时长 | 截止时间 | 分享/交流活动提醒（时间待定） |
| --- | --- | --- | --- | --- |
| Task1：开营&baseline跑通 | 了解赛题，跑通baseline | 3 天 | 6月20日 | 开营直播-6月19日 |
| Task2：baseline精读 | 赛题分析与baseline精读 | 3天 | 6月23日 | baseline精读直播分享-6月21号 |
| [Task3： LLm与微调入门](https://datawhaler.feishu.cn/wiki/Drttwt1bxiFNW8k8kpFctdQDn2c) | LLM与微调入门学习 | 5天 | 6月28日 | 大模型微调进阶分享 |
| [Task4：微调进阶](https://datawhaler.feishu.cn/wiki/Pbw7wrZ5kiNtCykrgjyc1bh8nBd) | 学习如何做好一个微调工作 | 5天 | 7月3日 | +   网文大模型上分经验交流 |
| Task5：继续冲刺 TOP | 持续优化迭代你的小说大模型 | 4 天 | 7月7日 | +   更多技术交流~ |

### Baseline部分内容解析

1. 【数据集】baseline 使用了官方数据集进行清洗，仅抽取了《呼啸山庄》小说的前 30% 的数据做为训练数据集

```
# 开始给《呼啸山庄》前30%的段落打标签
dataset = build_dataset(story_name,paragraphs[:len(paragraphs)//10*3])
```

实际官方给出的参考数据集中有 17 部小说，如有需要可全部进行清洗，另外，`build_dataset`这个方法只能一部小说一部小说进行清洗，如果有多个小说的数据会被拆分为多个文件，可在处理完毕后执行下面这个步骤，将 `data`文件夹 下的所有 json数据文件整理到 `dataset`文件夹 的`merged_story.json`

```
import json
import os

# 设置文件夹路径
directory_path = './data'

# 初始化一个空列表，用于存储合并后的数据
merged_data = []

# 遍历文件夹下的所有文件
for filename in os.listdir(directory_path):
    # 检查文件扩展名是否为.json
    if filename.endswith('.json'):
        # 构建文件的完整路径
        file_path = os.path.join(directory_path, filename)
        # 打开并读取JSON文件
        with open(file_path, 'r', encoding='utf-8') as file:
            # 加载JSON内容到变量
            data = json.load(file)
            # 将当前文件的数据添加到合并列表中
            merged_data.extend(data)

# 将合并后的数据转换为JSON格式
merged_json = json.dumps(merged_data, ensure_ascii=False, indent=4)

# 可以选择将合并后的数据写入到一个新的JSON文件中
output_file_path = './dataset/merged_story.json'
with open(output_file_path, 'w', encoding='utf-8') as file:
    file.write(merged_json)

# 或者直接输出到控制台
print(merged_json)
```

1. 【处理数据集】baseline使用了deepseek大模型进行数据清洗和打标，需要使用其API

数据清洗并没有特殊的要求，只需要能清洗出来合适的数据即可。

1. 【处理数据集】

Lora 训练的数据是需要经过格式化、编码之后再输入给模型进行训练的，如果是熟悉 Pytorch 模型训练流程的同学会知道，我们一般需要将输入文本编码为 `input_ids`，将输出文本编码为 `labels`，编码之后的结果都是多维的向量。我们首先定义一个预处理函数，这个函数用于对每一个样本，编码其输入、输出文本并返回一个编码后的字典：

+   加载`tokenizer`

```
model_path = './Qwen/Qwen2-1___5B-Instruct'

tokenizer = AutoTokenizer.from_pretrained(model_path, use_fast=False, trust_remote_code=True)
tokenizer
```

+   指令集构造

LLM 的微调一般指指令微调过程。这里我们主要强调数据格式化部分，这部分在数据准备部分已经实现，如果是用前面产生的数据集训练，那么这部分可以跳过。这里我们只是提供一个方法，来帮助有其他需要的参赛者。

所谓指令微调，是说我们使用的微调数据形如：

```
{
    "instruction":"回答以下用户问题，仅输出答案。",
    "input":"1+1等于几?",
    "output":"2"
}
```

其中，`instruction` 是用户指令，告知模型其需要完成的任务；`input` 是用户输入，是完成用户指令所必须的输入内容；`output` 是模型应该给出的输出。

即我们的核心训练目标是让模型具有理解并遵循用户指令的能力。因此，在指令集构建时，我们应针对我们的目标任务，针对性构建任务指令集。

+   Prompt Template

Qwen1.5 和 Qwen2 采用的Prompt Template格式如下：

```
<|im_start|>system
You are a helpful assistant.<|im_end|>
<|im_start|>user
你是谁？<|im_end|>
<|im_start|>assistant
我是一个有用的助手。<|im_end|>
```



## 数据预处理

### process_func 函数

```python
def process_func(example):
    MAX_LENGTH = 2048    # Llama分词器会将一个中文字切分为多个token，因此需要放开一些最大长度，保证数据的完整性
    input_ids, attention_mask, labels = [], [], []
    instruction = tokenizer(f"<|im_start|>system\n你是一个熟读各类小说的专家，请你根据要求写一段800字左右的小说。<|im_end|>\n<|im_start|>user\n{example['instruction'] + example['input']}<|im_end|>\n<|im_start|>assistant\n", add_special_tokens=False)  # add_special_tokens 不在开头加 special_tokens
    response = tokenizer(f"{example['output']}", add_special_tokens=False)
    input_ids = instruction["input_ids"] + response["input_ids"] + [tokenizer.pad_token_id]
    attention_mask = instruction["attention_mask"] + response["attention_mask"] + [1]  # 因为eos token咱们也是要关注的所以 补充为1
    labels = [-100] * len(instruction["input_ids"]) + response["input_ids"] + [tokenizer.pad_token_id]  
    if len(input_ids) > MAX_LENGTH:  # 做一个截断
        input_ids = input_ids[:MAX_LENGTH]
        attention_mask = attention_mask[:MAX_LENGTH]
        labels = labels[:MAX_LENGTH]
    return {
        "input_ids": input_ids,
        "attention_mask": attention_mask,
        "labels": labels
    }
```

### 数据映射与解码

```python
tokenized_id = ds.map(process_func, remove_columns=ds.column_names)
tokenized_id
```

```python
print(tokenizer.decode(tokenized_id[0]['input_ids']))
```

```python
print(tokenizer.decode(list(filter(lambda x: x != -100, tokenized_id[1]["labels"]))))
```

## 模型选型

baseline中选择了仅有 1.5B 参数的`Qwen2-1.5B-Instruct`模型，原因是其微调所需的算力资源较小，作为 baseline 供大家进行体验和尝试比较经济划算，但如果想要有更好的训练效果，可以在自己可接受的成本范围内尝试更多参数量的开源模型，数据集的数量同理。

## 模型写作

baseline中仅使用微调的模型写作了3篇文章

由于baseline所使用的是较低配置的机器，推理速度较慢，生成一篇文章耗时过长，因此baseline仅进行了3篇文章的写作，带大家体验整个赛事流程。

## 提交格式

官方指定的提交格式与其给出的`test.json`文件相同

![](img/d22be2d2eddc54402460d52d76382cdf.png)

[`test.json`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/T8eFbvKTwoGRJDx9DOqcLk9yn5d)

### 主要特点

1. 整个文件遵循 JSON Lines 格式
2. 一条结果占据一行，有三个字段：

   `instruction`：可自定义的指令

   `input`：需写作的故事梗概

   `output`：微调的模型写作的结果

### submit.json 格式要求

`submit.json`需要遵循`test.json`的格式：

1. 每一行的`input`部分不能修改，需要有1000行

   `input`：每道题需要与`test.json`文件中的`input`字段完全一致，否则提交会报错。

2. 需要填入的是`instruction`和`output`字段

   `instruction`：可以为空，最多不能超过100个字符，否则提交会报错。

   `output`：`instruction`字段与`input`字段拼接后通过你的模型推理得到的撰写结果。

3. 更多代码详解详见 task2 教程内容~

## 学习者提交榜单

## 其他技术小彩蛋

### 如何在本地远程连接 Deepln GPU 服务器？

> 本教程由 、 、 、 、 、 等贡献者贡献，期待你的点赞和评论反馈~
