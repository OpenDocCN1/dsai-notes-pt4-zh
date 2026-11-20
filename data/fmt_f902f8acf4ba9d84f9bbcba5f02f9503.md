
# GLM-4提示工程技术入门

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/UPSed55q4oYzQ8xx2jCcticZnUg](https://kq4b3vgg5b.feishu.cn/docx/UPSed55q4oYzQ8xx2jCcticZnUg)

```python
import os
from zhipuai import ZhipuAI
from IPython.display import display, Code, Markdown

api_key = os.getenv("ZHIPU_API_KEY")
```

```python
client = ZhipuAI(api_key=api_key) 
response = client.chat.completions.create(
    model="glm-4",  # 填写需要调用的模型名称
    messages=[
        {"role": "user", "content": "你好"}
    ],
)
print(response.choices[0].message)
```

```
content='你好👋！我是人工智能助手智谱清言，可以叫我小智🤖，很高兴见到你，欢迎问我任何问题。' role='assistant' tool_calls=None
```

## 大模型的涌现能力

GPT3模型是第一批拥有“涌现能力”的大语言模型，即哪怕模型未经特定任务的训练，但在适当的提示下，仍然能够解决某些特定领域的问题。例如大语言模型可以解答数学问题、辅助进行编程、甚至是进行问答等，其实都属于模型的涌现能力。而为何类似数学能力是模型的“涌现能力”，其实原因也并不复杂——作为概率模型，大语言模型甚至不知道数字代表的真实含义，模型只是在学习了无数的语料之后，发现了一些数学结论之间的潜在概率关系，才最终涌现出了数学运算或者复杂推理的能力。

## 大模型应用的关键技术——提示工程（Prompt engineering）

不过需要注意的是，大模型的这种“涌现能力”其实并不稳定，在不修改模型本身参数（微调）的情况下，模型涌现能力极度依赖对模型的提示过程，即对同样一个模型，不同的提示方法将获得质量完全不同的结果。而一个完整的用户和大语言模型的交互流程，也被称为大语言模型的提示工程（Prompt engineering），根据此前的描述我们不难理解，提示工程是激发模型涌现能力（激发模型潜力）的非常关键的技术。同时，由于我们对大语言模型“涌现能力”的应用要求是远远多于简单的使用大模型进行文本创建的（毕竟哪怕是对话任务都属于大模型涌现能力的范畴），因此提示工程这一专门用于激发大语言模型涌现能力的技术就变得尤其重要。

## 提示词与提问技巧

正如Ch 0.课程介绍部分所言，提示工程其实是极容易上手、但同时上限很高的一项技术。对于新人大模型工程师来说，只需要掌握一些提示词模板、就能很好的提升模型效果，而提示工程技术最终的目的是更好的激发大模型的涌现能力，本质上其实是“和大模型对话的艺术”，归根结蒂还是在长期的经验积累过程中积累和总结得到的技巧，这些技巧因模型的不同而不同、因任务不同而不同，很多时候甚至需要考验大模型工程师自身的“语感”。而若能合理提示，那么大模型能够发挥的作用是难以想象的，在编程与Agent开发领域，OpenAI创始人Sam甚至扬言，自然语言将会是最重要的“编程语言”：

![](img/3bb712b77a20cef9af8a66f52c65cfbf.png)

## 无处不在的提示工程

值得注意的是，时至今日，大多数模型进行性能评测时，都不是简单的围绕问题进行回答，而是在某项提示工程技术加持下测试模型回答准确率。如近期发布的Gemini模型，标榜的性能评测结果是是使用了COT（思考链）提示工程技术下模型问答的准确率。当然，与Gemini对应的进行比较的则是GPT-4模型，但该评测结果饱受诟病的是对比模型采用了另一种提示工程方法——Few-shot，在相同数据集下使用不同提示工程方法并将结果在同一水平上进行比较，确实很难确保评判的公平性。更为关键的是，相比COT，5-shot（Few-shots）提示方法要“弱”很多。

![](img/916032bcb6ca1b067b98a59d28d9e0de.png)

相比之下，智谱AI在发布GLM-4的评测榜单则更加可信一些：

![](img/7d18f7e7f53f372bb375a2aeab434b7f.png)

在进行模型比较时，都采用了相同的提示方法在相同的数据集上进行比较。当然，无论是Gemini模型，还是GLM-4模型发布，都不约而同的采用了MMLU数据集，该数据集目前已然称为大模型性能评测最热门数据集之一。

## 大模型世界认知数据集：MMLU

MMLU（Massive Multitask Language Understanding）是一个大规模的语言理解数据集，旨在评估和推动自然语言理解（NLU）模型在多种任务和领域上的性能。MMLU由Meta AI（前Facebook AI）创建，并在多项研究中使用，以测试模型在广泛的语言理解任务上的能力。MMLU涵盖了57种不同类型的任务，包括但不限于常识推理、科学理解、历史事实、文学分析等。正因如此，MMLU数据集也被称为世界认知数据集，这种多样性使得MMLU成为了测试模型在广泛领域内理解能力的重要工具。

而[本节介绍的提示工](https://arxiv.org/pdf/2009.03300.pdf)程，也将主要依据MMLU上的表现，来判断某项提示工程方法是否有效。需要注意的是，MMLU数据集总共包含14000多个问题，但该数据集本身并未开源，只面向科研机构开放申请使用权限。课程里将主要使用MMLU论文种所展示的若干问题。MMLU论文地址

![](img/16ff4ba8b47b50bbe3c7384ccb221821.png)

## 一、大模型提示工程方法介绍

### 第一代推理能力测试问题回顾

接下来就正式进入到大模型提示工程方法介绍部分，这里我们先通过一组简单问题的问答过程，来介绍目前大模型提示工程技术的基本方法，然后再将其应用于MMLU数据集中，测试这些方法在复杂问题问答中的实际效果。

以下四个问题是提示工程技术相关论文中的基础测试题，在GPT-3.5模型时代往往被用于进行模型推理能力测试。

```python
Q1 = '罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？'
A1 = '现在罗杰总共有11个网球。'
```

```python
Q2 = '食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？'
A2 = '现在食堂总共有9个苹果。'
```

```python
Q3 = '杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问总共有多少个蓝色高尔夫球？'
A3 = '现在总共有4个蓝色高尔夫球。'
```

```python
Q4 = '艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？'
A4 = '关闭之前艾米能滑3次。'
```

### 1.基础模型问答性能测试

#### GLM-3-turbo性能测试

```python
response = client.chat.completions.create(
    model="glm-3-turbo",  # 填写需要调用的模型名称
    messages=[
        {"role": "user", "content": Q1}
    ],
)
display(Markdown(response.choices[0].message.content))
```

罗杰原本有5个网球，他又买了两盒网球，每盒有3个网球。那么他新买网球的总数是2盒 × 3个/盒 = 6个网球。

所以，他现在总共有5个 + 6个 = 11个网球。

```python
response = client.chat.completions.create(
    model="glm-3-turbo",  # 填写需要调用的模型名称
    messages=[
        {"role": "user", "content": Q2}
    ],
)
display(Markdown(response.choices[0].message.content))
```

食堂原来有23个苹果，用掉了20个，那么就剩下23 - 20 = 3个苹果。

然后又买了6个苹果，那么现在就有3 + 6 = 9个苹果。

所以，现在食堂总共有9个苹果。

```python
response = client.chat.completions.create(
    model="glm-3-turbo",  # 填写需要调用的模型名称
    messages=[
        {"role": "user", "content": Q3}
    ],
)
display(Markdown(response.choices[0].message.content))
```

杂耍者可以杂耍16个球，其中一半的球是高尔夫球，那么就有 16 / 2 = 8 个高尔夫球。

再根据题意，其中一半的高尔夫球是蓝色的，那么就有 8 / 2 = 4 个蓝色高尔夫球。

所以，总共有4个蓝色高尔夫球。

```python
response = client.chat.completions.create(
    model="glm-3-turbo",  # 填写需要调用的模型名称
    messages=[
        {"role": "user", "content": Q4}
    ],
)
display(Markdown(response.choices[0].message.content))
```

艾米每次爬到滑梯顶部和滑下的总时间是4分钟（爬上去）+ 1分钟（滑下来）= 5分钟。水滑梯关闭前有15分钟，所以在这15分钟内，艾米可以爬上滑梯并滑下的次数是15分钟除以每次的总时间5分钟，即：

15分钟 ÷ 5分钟/次 = 3次。

但是，我们需要记得，艾米在第一次爬上滑梯时就已经用了4分钟，所以她实际上只有15分钟 - 4分钟 = 11分钟的时间来反复滑下。用这11分钟除以每次的总时间5分钟，我们得到：

11分钟 ÷ 5分钟/次 = 2次余1分钟。

这意味着艾米在关闭之前可以滑2次，因为即使她还有1分钟，但她已经没有足够的时间再爬到顶部的4分钟了。所以最终答案是，艾米能在水滑梯关闭之前滑2次。

#### GPT-3.5性能测试

```python
import openai
openai.api_key = os.getenv("OPENAI_API_KEY")
```

```python
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": Q1},
  ]
)
```

```python
response.choices[0].message['content']
```

```
'罗杰原本有5个网球，又买了两盒网球，每盒有3个网球，所以一共买了2 * 3 = 6个网球。\n现在总共有5 + 6 = 11个网球。 \n所以，他现在总共有11个网球。'
```

```python
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": Q2},
  ]
)
```

```python
response.choices[0].message['content']
```

```
'食堂现在总共有23 - 20 + 6 = 9个苹果。'
```

```python
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": Q3},
  ]
)
```

```python
response.choices[0].message['content']
```

```
'根据题意，杂耍者可以杂耍16个球，其中一半是高尔夫球，即8个球是高尔夫球。又已知一半的高尔夫球是蓝色的，即有一半的8个高尔夫球是蓝色的。\n\n所以，总共有8个蓝色高尔夫球。'
```

```python
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": Q4},
  ]
)
```

```python
response.choices[0].message['content']
```

```
'在滑梯关闭之前，她能滑的次数取决于她滑下来所需要的时间和滑梯关闭的时间之间的差距。

滑下来所需要的时间是1分钟。
滑梯关闭的时间是15分钟，即900秒。

在滑梯关闭之前，她能滑的次数等于（滑梯关闭时间 - 滑下来所需时间）/ 爬到滑梯顶部所需时间。

次数 = (900秒 - 60秒) / 4分钟 = 840秒 / 4分钟 = 210次。

所以，在关闭之前，她能滑210次。'
```

#### GLM-4推理能力测试

```python
response = client.chat.completions.create(
    model="glm-4",  # 填写需要调用的模型名称
    messages=[
        {"role": "user", "content": Q1}
    ],
)
display(Markdown(response.choices[0].message.content))
```

罗杰原来有5个网球，他又买了两盒，每盒有3个网球。

所以，他买的网球总数是：

2盒 × 3个/盒 = 6个

那么，他现在总共有的网球数是：

原来的网球数 + 新买的网球数

5个 + 6个 = 11个

所以，罗杰现在总共有11个网球。

```python
response = client.chat.completions.create(
    model="glm-4",  # 填写需要调用的模型名称
    messages=[
        {"role": "user", "content": Q2}
    ],
)
display(Markdown(response.choices[0].message.content))
```

原来食堂有23个苹果，用掉了20个，那么剩下：

23 - 20 = 3 个苹果。

之后食堂又买了6个苹果，那么现在的总数是：

3 + 6 = 9 个苹果。

所以现在食堂总共有9个苹果。

```python
response = client.chat.completions.create(
    model="glm-4",  # 填写需要调用的模型名称
    messages=[
        {"role": "user", "content": Q3}
    ],
)
display(Markdown(response.choices[0].message.content))
```

如果杂耍者可以杂耍16个球，那么高尔夫球的总数是16个球的一半，即：

16 / 2 = 8个高尔夫球

其中一半的高尔夫球是蓝色的，所以蓝色高尔夫球的数量是8个高尔夫球的一半：

8 / 2 = 4个蓝色高尔夫球

所以，总共有4个蓝色高尔夫球。

```python
response = client.chat.completions.create(
    model="glm-4",  # 填写需要调用的模型名称
    messages=[
        {"role": "user", "content": Q4}
    ],
)
display(Markdown(response.choices[0].message.content))
```

艾米爬到滑梯顶部需要4分钟，滑下来需要1分钟，所以她每次完整地玩滑梯需要5分钟。

如果水滑梯在15分钟后关闭，我们可以用总时间除以每次玩滑梯的时间来计算她能玩多少次：

15分钟 ÷ 5分钟/次 = 3次

所以，在滑梯关闭之前，艾米能滑3次。

#### GPT-4模型性能测试
