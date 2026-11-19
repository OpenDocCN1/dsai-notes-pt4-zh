
# ChatGPT 快速微调 + 数据格式准备

> 来源：[https://agijuejin.feishu.cn/docx/FpfLdWbYSoCItexkYnRcMkpRnlc](https://agijuejin.feishu.cn/docx/FpfLdWbYSoCItexkYnRcMkpRnlc)

> 作者：[王几行XING](https://www.zhihu.com/people/brycewang1898)

> 原文：[https://zhuanlan.zhihu.com/p/707880964](https://zhuanlan.zhihu.com/p/707880964)

## 为什么要微调 ChatGPT？

一些常见的通过微调可以改进结果的用例：

- Setting the style, tone, format, or other qualitative aspects 设置风格、语调、格式或其他定性方面
- Improving reliability at producing a desired output 提高生成期望输出的可靠性
- Correcting failures to follow complex prompts 纠正未能遵循复杂提示的错误
- Handling many edge cases in specific ways 以特定方式处理许多边缘情况
- Performing a new skill or task that’s hard to articulate in a prompt 执行难以在提示中明确表达的新技能或任务

## 目前哪些 OpenAI 的模型可以微调？

- `GPT-4`（`gpt-4-0613` 和 `gpt-4o-*`）的微调，是一个实验性访问计划——符合条件的用户可以在创建新的 `fine-tuning` 任务时在 fine-tuning UI 中请求访问。
- `Fine-tuning` 目前适用于以下模型：`gpt-3.5-turbo-0125`（推荐）, `gpt-3.5-turbo-1106`, `gpt-3.5-turbo-0613`, `babbage-002`, `davinci-002`, `gpt-4-0613`（实验性）, 和 `gpt-4o-2024-05-13`。
- 你也可以对一个已经 `fine-tuned` 的模型进行 `fine-tuning`，这在你获得额外数据且不想重复之前的训练步骤时非常有用。
- `gpt-3.5-turbo`，在结果和易用性方面将是大多数用户的正确选择

## 微调举例

目标：

- 把 ChatGPT 训练成一个美食评论的语义分析专家
- 它对美食评论的评价，以 `rating` + `sentiment` 的结果返回
- 这么做的具体目标是训练它掌握返回的格式，以及节省 prompt 中 `few-shots` 的 tokens

数据（来源：[https://www.kaggle.com/datasets/joebeachcapital/restaurant-reviews](https://www.kaggle.com/datasets/joebeachcapital/restaurant-reviews)）：

![](img/faf72b107024957fbdef23aca2f9de49.png)

![](img/61874b996daacaedb402020941193902.png)

## 数据准备

原始数据来源（同上）：[https://www.kaggle.com/datasets/joebeachcapital/restaurant-reviews](https://www.kaggle.com/datasets/joebeachcapital/restaurant-reviews)

```
## 以下代码来源：https://github.com/LinkedInLearning/openai-api-fine-tuning-2719505/blob/main/create-training-data/create-training-data.ipynb

## 0. 设置
# 导入必要的库
import os
import pandas as pd
import json         
from pprint import pprint
import tiktoken # 用于计数token
import numpy as np
from collections import defaultdict

## 1. 加载数据
# 数据设置
data_folder = "../data"  # 数据文件夹路径
source_file = os.path.join(data_folder, "allreviews.csv")  # 源文件路径
training_file_name = os.path.join(data_folder, "review_finetune_training.jsonl")  # 训练文件路径
validation_file_name = os.path.join(data_folder, "review_finetune_validation.jsonl")  # 验证文件路径

# 加载数据
data = pd.read_csv(os.path.join(data_folder, source_file))  # 读取CSV文件
data.head()  # 显示前几行数据

## 2. 创建示例对话
# training_data = []

# 系统消息对于所有示例都是相同的。
system_message = "You are a restaurant review analyzer. Analyze the sentiment in the review provided. Output the rating value and the sentiment as JSON."

# 创建用户消息
def create_user_message(row):
    return f"""Review: {row['Review']}"""  # 从数据行中获取评论并格式化为字符串

# 创建助手消息
def create_assistant_message(row):
    return f"{{\"rating\": {row['Rating']}, \"Sentiment\": \"{row['Sentiment']}\"}}\n"  # 格式化评分和情感为JSON字符串

# 接收当前行并创建一个完整的示例对话
def create_full_exchange(row):
    messages = []
    messages.append({"role": "system", "content": system_message})  # 添加系统消息

    user_message = create_user_message(row)  # 创建用户消息
    messages.append({"role": "user", "content": user_message})  # 添加用户消息

    assistant_message = create_assistant_message(row)  # 创建助手消息
    messages.append({"role": "assistant", "content": assistant_message})  # 添加助手消息

    return {"messages": messages}  # 返回包含所有消息的字典

# 输出第一个示例对话
pprint(create_full_exchange(data.iloc[0]))  # 打印第一个示例对话

# 将前100行数据用作训练集，接下来的100行数据用作验证集
training_df = data.iloc[0:100]
validation_df = data.iloc[100:200]

# 对每一行使用 create_full_exchange 函数生成示例对话
training_data = training_df.apply(create_full_exchange, axis=1).tolist()  # 生成训练数据
validation_data = validation_df.apply(create_full_exchange, axis=1).tolist()  # 生成验证数据

# 输出训练集中的前五个示例
for example in training_data[:5]:
    print(example)  # 打印每个示例

## 3. 创建 JSONL 文件
# 将数据保存为 .jsonl 文件。每一行是一个完整的示例对话。
# 注意：在 JSONL 格式中，每一行以换行符结尾，没有逗号。
def write_jsonl(data_list: list, filename: str) -> None:
    with open(filename, "w") as out:  # 打开文件以写入模式
        for ddict in data_list:
            jout = json.dumps(ddict) + "\n"  # 将字典转换为JSON字符串并添加换行符
            out.write(jout)  # 写入文件

# 写入 .jsonl 文件
write_jsonl(training_data, training_file_name)  # 写入训练数据文件
write_jsonl(validation_data, validation_file_name)  # 写入验证数据文件

## 4. 验证数据
# 指定要测试的数据文件（一次测试一个）
file_to_test = training_file_name
# file_to_test = validation_file_name

# 加载数据集
with open(file_to_test, 'r', encoding='utf-8') as f:
    dataset = [json.loads(line) for line in f]  # 逐行读取并解析JSON

from check_format_errors import check_format_errors
# 检查数据集的格式
errors = check_format_errors(dataset)  # 检查格式错误

## 5. 获取 Token 计数
from token_counter import num_tokens_from_messages, num_assistant_tokens_from_messages, print_distribution

# 警告和 token 计数
n_missing_system = 0  # 缺少系统消息的示例数
n_missing_user = 0  # 缺少用户消息的示例数
n_messages = []  # 消息数量列表
convo_lens = []  # 对话长度列表
assistant_message_lens = []  # 助手消息长度列表

for ex in dataset:
    messages = ex["messages"]
    if not any(message["role"] == "system" for message in messages):
        n_missing_system += 1  # 统计缺少系统消息的示例
    if not any(message["role"] == "user" for message in messages):
        n_missing_user += 1  # 统计缺少用户消息的示例
    n_messages.append(len(messages))  # 记录每个示例的消息数量
    convo_lens.append(num_tokens_from_messages(messages))  # 记录每个对话的 token 数量
    assistant_message_lens.append(num_assistant_tokens_from_messages(messages))  # 记录助手消息的 token 数量

print("Num examples missing system message:", n_missing_system)  # 打印缺少系统消息的示例数

## 6. 成本估算
# 定价和默认的 n_epochs 估算
MAX_TOKENS_PER_EXAMPLE = 4096  # 每个示例的最大 token 数量

TARGET_EPOCHS = 3  # 目标训练轮数
MIN_TARGET_EXAMPLES = 100  # 最小目标示例数
MAX_TARGET_EXAMPLES = 25000  # 最大目标示例数
MIN_DEFAULT_EPOCHS = 1  # 最小默认训练轮数
MAX_DEFAULT_EPOCHS = 25  # 最大默认训练轮数

n_epochs = TARGET_EPOCHS  # 初始化训练轮数为目标轮数
n_train_examples = len(dataset)  # 获取训练示例数
if n_train_examples * TARGET_EPOCHS < MIN_TARGET_EXAMPLES:
    n_epochs = min(MAX_DEFAULT_EPOCHS, MIN_TARGET_EXAMPLES // n_train_examples)  # 调整训练轮数
elif n_train_examples * TARGET_EPOCHS > MAX_TARGET_EXAMPLES:
    n_epochs = max(MIN_DEFAULT_EPOCHS, MAX_TARGET_EXAMPLES // n_train_examples)  # 调整训练轮数

n_billing_tokens_in_dataset = sum(min(MAX_TOKENS_PER_EXAMPLE, length) for length in convo_lens)  # 计算计费的 token 数量
print(f"Dataset has ~{n_billing_tokens_in_dataset} tokens that will be charged for during training")  # 打印数据集的 token 数量
print(f"By default, you'll train for {n_epochs} epochs on this dataset")  # 打印默认训练轮数
print(f"By default, you'll be charged for ~{n_epochs * n_billing_tokens_in_dataset} tokens")  # 打印默认计费的 token 数量
```

## 微调过程

![](img/8b3540005ac349dbf5e376f0bcbfd2ad.png)

![](img/47ab5940640e4cfa80b8b2d970e460b6.png)

## 模型使用

![](img/183ed8c53a99768e17a03c62560e6b34.png)

图片中的：

- 第一个结果，虽然 `gpt 3.5 turbo` 也返回了 `sentiment` 的结果，但是只是简单的 “positive”，而不是我们想要的。
- 第二个结果，选用的是微调后的 `3.5`，返回了我们真正想要的语义分析结果（一共是 5 级评价，每一级都有一个我们设定的具体的名称，而不是简单的 `positive`/`negative`）

下面是 1 个 epoch checkpoint 模型的结果，也达到了我们的预期。

![](img/63ab7b4bc1cc1fb05612b75fb4a08407.png)

PS: 以上通过 OpenAI 网站进行的微调工作，也可以通过代码 + API 来完成，而且结果也会同步的 OpenAI 的网站。

（文章结束）
