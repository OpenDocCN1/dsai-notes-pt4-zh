
# `DeepSeek-R1-Distill-Qwen3-8B` GRPO微调教程

> 来源：[https://v11enp9ok1h.feishu.cn/docx/ElD9dS15OoKSzLxR2EscUQhYnCg](https://v11enp9ok1h.feishu.cn/docx/ElD9dS15OoKSzLxR2EscUQhYnCg)

> 话不多说，直接开始！

本文使用的测试环境为单张 A100，显存 80GB，可根据需求切换不同参数量的模型，实测4B 24G显存 is enough！

使用的框架为 `Unsloth`

![](img/71866a2d1600d9cc3d43d6f22d0b72cc.png)

`Unsloth` 是一个极其强调资源节省的框架，把所有的资源节省做到了极致，具体来讲 `Unsloth` 能够将 `Llama-3`、`Mistral`、`Phi-4` 和 `Gemma` 等大型语言模型的微调速度提升 2 倍，内存占用减少 70%，并且准确率没有任何下降！

官方文档非常全面，详细指导了如何训练自己的定制模型。其中涵盖了安装和更新 `Unsloth`、创建数据集、运行和部署模型等基本要素。 `Unsloth` 让大家在本地或在 Google Colab 和 Kaggle 等平台上训练像 `Llama 3` 这样的模型变得极其简单。 `Unsloth` 简化了整个训练工作流程，包括模型加载、量化、训练、评估、运行、保存、导出，以及与 Ollama、`llama.cpp` 和 `vLLM` 等推理引擎的集成。

`Unsloth` 定期与 Hugging Face、Google 和 Meta 的团队合作，以修复 LLM 训练和模型中的错误。因此，当使用 `Unsloth` 进行训练或使用模型时，可以期待获得最准确的结果。 `Unsloth` 具有高度可定制性，允许更改聊天模板或数据集格式等内容。 `Unsloth` 还为视觉、文本转语音 (TTS)、BERT、强化学习 (RL) 等提供了预构建的脚本！此外，`Unsloth` 支持所有训练方法和所有基于 Transformer 的模型。

## 教程概览

本教程将指导您完成 `DeepSeek-R1-Distill-Qwen3-8B 模型的 GRPO（Group Relative Policy Optimization）微调`，这是一种先进的强化学习技术，专门用于提升大语言模型在特定任务上的表现。

### 什么是GRPO？

GRPO（Group Relative Policy Optimization）是一种强化学习优化技术，通过设计多个奖励函数来评估模型输出的不同方面，从而指导模型学习期望的行为模式。在数学推理任务中，GRPO可以帮助模型：

+   学会按照特定格式输出答案

+   提高推理过程的逻辑性

+   增强答案的准确性

+   改善输出的结构化程度

### 本教程的学习内容

1. `环境设置`: 安装 `Unsloth` 和相关依赖

1. `模型加载`: 加载 `DeepSeek-R1-Distill-Qwen3-8B` 预训练模型

1. `LoRA配置`: 设置高效的参数微调

1. `数据处理`: 处理 `GSM8K` 数学推理数据集

1. `格式设计`: 定义结构化的输出格式

1. `奖励函数`: 设计多维度评估体系

1. `GRPO训练`: 执行强化学习微调

1. `效果验证`: 测试微调后的模型

1. `模型保存`: 保存训练结果

1. `可视化监控`: 使用 SwanLab 跟踪训练过程

```
# 安装依赖包
# pip install unsloth vllm==0.8.5.post1
```

```
# 安装语言检测库
# pip install langid -qq
```

```
from unsloth import FastLanguageModel
import torch
max_seq_length = 1024 
lora_rank = 32

model, tokenizer = FastLanguageModel.from_pretrained(
    model_name = &quot;/opt/tiger/test0/DeepSeek-R1-0528-Qwen3-8B&quot;,
    max_seq_length = max_seq_length,
    load_in_4bit = True, # 对于LoRA 16位设置为False
    fast_inference = True, # 启用vLLM快速推理
    max_lora_rank = lora_rank,
    gpu_memory_utilization = 0.7, # 如果内存不足请减少此值
)

model = FastLanguageModel.get_peft_model(
    model,
    r = lora_rank, # 选择任何大于0的数字！建议8, 16, 32, 64, 128
    target_modules = [
        &quot;q_proj&quot;, &quot;k_proj&quot;, &quot;v_proj&quot;, &quot;o_proj&quot;,
        &quot;gate_proj&quot;, &quot;up_proj&quot;, &quot;down_proj&quot;,
    ],
    lora_alpha = lora_rank*2, # *2可以加速训练
    use_gradient_checkpointing = &quot;unsloth&quot;, # 减少内存使用
    random_state = 3407,
)
```

### GRPO对话模板

```
reasoning_start = None
reasoning_end = None
user_token = None
assistant_token = None

for token in tokenizer.get_added_vocab().keys():
    if &quot;think&quot; in token and &quot;/&quot; in token:
        reasoning_end = token
    elif &quot;think&quot; in token:
        reasoning_start = token
    elif &quot;user&quot; in token:
        user_token = token
    elif &quot;assistant&quot; in token:
        assistant_token = token

system_prompt = \
f&quot;&quot;&quot;你接到一个问题。
请思考这个问题并提供你的解题过程。
你必须用印尼语思考。&quot;&quot;&quot;
system_prompt
```

```
print(tokenizer.apply_chat_template([
    {&quot;role&quot; : &quot;user&quot;, &quot;content&quot; : &quot;What is 1+1?&quot;},
    {&quot;role&quot; : &quot;assistant&quot;, &quot;content&quot; : f&quot;&lt;think&gt;I think it&#x27;s 2.2&lt;/think&gt;2&quot;},
    {&quot;role&quot; : &quot;user&quot;, &quot;content&quot; : &quot;What is 1+1?&quot;},
    {&quot;role&quot; : &quot;assistant&quot;, &quot;content&quot; : f&quot;&lt;think&gt;I think it&#x27;s 2.2&lt;/think&gt;2&quot;},
], tokenize = False, add_generation_prompt = True))
```

### 数据准备

```
from datasets import load_dataset
dataset = load_dataset(&quot;open-r1/DAPO-Math-17k-Processed&quot;, &quot;en&quot;, split = &quot;train&quot;)
dataset
```

让我们看看第一行数据：

```
dataset[0][&quot;prompt&quot;]
```

```
dataset[0][&quot;solution&quot;]
```

在GSM8K中，我们注意到所有答案都有####标记，所以我们需要提取它。但对于Open R1数据集，我们可以跳过下面的处理。

```
def extract_hash_answer(text):
    # if &quot;####&quot; not in text: return None
    # return text.split(&quot;####&quot;)[1].strip()
    return text
extract_hash_answer(dataset[0][&quot;solution&quot;])
```

让我们映射数据集！并查看第一行：

```
dataset = dataset.map(lambda x: {
    &quot;prompt&quot; : [
        {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: system_prompt},
        {&quot;role&quot;: &quot;user&quot;,   &quot;content&quot;: x[&quot;prompt&quot;]},
    ],
    &quot;answer&quot;: extract_hash_answer(x[&quot;solution&quot;]),
})
dataset[0]
```

我们创建一个正则表达式格式来匹配推理部分和答案：

```
import re

# 添加可选的EOS标记匹配
solution_end_regex = rf&quot;{reasoning_end}(.*)&quot;

match_format = re.compile(solution_end_regex, re.DOTALL)
match_format
```

我们验证它能正常工作：

```
match_format.findall(
    &quot;Let me think!&lt;/think&gt;&quot;\
    f&quot;Hence, the solution is 2.&quot;,
)
```

```
match_format.findall(
    &quot;&lt;think&gt;Let me think!&lt;/think&gt;&quot;\
    f&quot;\n\nHence, the solution is 2&quot;,
)
```

我们现在要创建一个奖励函数来完全匹配格式 - 如果成功匹配我们给3分：

```
def match_format_exactly(completions, **kwargs):
    scores = []
    for completion in completions:
        score = 0
        response = completion[0][&quot;content&quot;]
        # 匹配是否完全符合格式！
        if match_format.search(response) is not None: score += 3.0
        scores.append(score)
    return scores
```

如果失败，我们希望在至少部分遵循格式时奖励模型，通过计算每个符号：

```
def match_format_approximately(completions, **kwargs):
    scores = []
    for completion in completions:
        score = 0
        response = completion[0][&quot;content&quot;]
        # 计算看到多少个关键词 - 如果太多我们会惩罚！
        # 如果我们看到1个，那么加一些分！

        # 不需要奖励&lt;think&gt;因为我们总是预置它！
        score += 0.5 if response.count(reasoning_start) == 1 else -1.0
        score += 0.5 if response.count(reasoning_end)   == 1 else -1.0
        scores.append(score)
    return scores
```

我们想要提取生成的答案，并奖励或惩罚它！我们还根据答案与真实答案的比率来奖励：

```
def check_answer(prompts, completions, answer, **kwargs):
    question = prompts[0][-1][&quot;content&quot;]
    responses = [completion[0][&quot;content&quot;] for completion in completions]

    extracted_responses = [
        guess.group(1)
        if (guess := match_format.search(r)) is not None else None \
        for r in responses
    ]

    scores = []
    for guess, true_answer in zip(extracted_responses, answer):
        score = 0
        if guess is None:
            scores.append(-2.0)
            continue
        # 正确答案得5分！
        if guess == true_answer:
            score += 5.0
        # 如果看到空格但奖励较少
        elif guess.strip() == true_answer.strip():
            score += 3.5
        else:
            # 我们也通过比率奖励接近的答案！
            # 即如果答案在某个范围内，奖励它！
            try:
                ratio = float(guess) / float(true_answer)
                if   ratio &gt;= 0.9 and ratio &lt;= 1.1: score += 2.0
                elif ratio &gt;= 0.8 and ratio &lt;= 1.2: score += 1.5
                else: score -= 2.5 # 惩罚错误答案
            except:
                score -= 4.5 # 惩罚
        scores.append(score)
    return scores
```

有时答案可能不是1个数字，而是像句子一样，例如"解决方案是$20" -> 我们提取20。

我们还移除可能的逗号，例如123,456

```
match_numbers = re.compile(
    r&quot;.*?[\s]{0,}([-]?[\d\.\,]{1,})&quot;,
    flags = re.MULTILINE | re.DOTALL
)
print(match_numbers.findall(&quot;  0.34  &quot;))
print(match_numbers.findall(&quot;  123,456  &quot;))
print(match_numbers.findall(&quot;  -0.234  &quot;))
print(match_numbers.findall(&quot;17&quot;))
```

最后，我们将尝试强制思考过程使用印尼语。这是DeepSeek R1论文中使用的语言一致性奖励的简单版本

```
import langid

def get_lang(text: str) -&gt; str:
    if not text:
        return &quot;und&quot;
    lang, _ = langid.classify(text)
    return lang


print(get_lang(&quot;Hello, How are you&quot;)) # 这应该返回en
print(get_lang(&quot;Aku berpikir kalau aku adalah kamu&quot;)) # 这应该返回id
print(get_lang(&quot;我在这里&quot;)) # 这应该返回zh
```
