# AI产品经理总结 | LLM中14种Prompt策略

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Ql70dmzFHo40zyxZviVcryion5d](https://qqs7y1hozd1.feishu.cn/docx/Ql70dmzFHo40zyxZviVcryion5d)

## Zero-Shot Prompting

> **是一种最直接的 Prompt，**不给 GPT 提供任何示范、样本数据或上下文，直接让其执行对应的任务或回答对应的问题。

**若该知识存在于 GP**T 对应的训练数据库时，它能够给出正确的回答。否则可能不会给出预期的回答。

此种是最基础的 Prompt，需要在此基础进行优化，引导 GPT 给出我们想要的结果。

> 在吴恩达的 ChatGPT Prompt Engineering 中，有提到一个这个技巧的另一种用法，不仅仅只是让 AI 按步骤行事，还会告知 AI 每一步要做什么。

```
Let&#x27;s work this out in a step by step way to be sure we have the right answer.
```

```
prompt_2 = f&quot;&quot;&quot;
Your task is to perform the following actions:
1 - Summarize the following text delimited by
  &lt;&gt; with 1 sentence.
2 - Translate the summary into French.
3 - List each name in the French summary.
4 - Output a json object that contains the
  following keys: french_summary, num_names.

Use the following format:
Text: &lt;text to summarize&gt;
Summary: &lt;summary&gt;
Translation: &lt;summary translation&gt;
Names: &lt;list of names in Italian summary&gt;
Output JSON: &lt;json with summary and num_names&gt;

Text: &lt;{text}&gt;
&quot;&quot;&quot;
```

以上代码，开头是让 AI 按照 1 ～ 4 步运行；然后再让 AI 根据特定格式输出内容。

最后AI输出结果是这样的：

```
Summary: Jack and Jill go on a quest to fetch water, but misfortune strikes and they tumble down the hill, returning home slightly battered but with their adventurous spirits undimmed.
Translation: Jack et Jill partent en quête d&#x27;eau, mais la malchance frappe et ils dégringolent la colline, rentrant chez eux légèrement meurtris mais avec leurs esprits aventureux intacts.
Names: Jack, Jill
Output JSON: {&quot;french_summary&quot;: &quot;Jack et Jill partent en quête d&#x27;eau, mais la malchance frappe et ils dégringolent la colline, rentrant chez eux légèrement meurtris mais avec leurs esprits aventureux intacts.&quot;, &quot;num_names&quot;: 2}
```

## Few-Shot Prompting

> 在零样本提示的基础上，再加一些示范样本，让 GPT 理解样本的意思，引导其给出我们预期的效果。

> 这是基础的有示范样本类型的版本，需要在此基础上优化，引导模型会使用简单的推理。

```
The odd numbers in this group add up to an even number: 4, 8, 9, 15, 12, 2, 1.
A: Adding all the odd numbers (9, 15, 1) gives 25. The answer is False.

The odd numbers in this group add up to an even number: 17,  10, 19, 4, 8, 12, 24.
A: Adding all the odd numbers (17, 19) gives 36. The answer is True.

The odd numbers in this group add up to an even number: 16,  11, 14, 4, 8, 13, 24.
A: Adding all the odd numbers (11, 13) gives 24. The answer is True.

The odd numbers in this group add up to an even number: 17,  9, 10, 12, 13, 4, 2.
A: Adding all the odd numbers (17, 9, 13) gives 39. The answer is False.

The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1. 
A:
```

## Chain-Of-Thought Prompting

> 这一技巧可以用于优化「零样本提示」与「少样本提示」。

> 当向 GPT 提问一个逻辑推理题时，它在思考的过程中会漏掉一些关键步骤。

**此时，为了得到预期的效果，可以在**问题的结尾加一句：让我们一步步地思考（Let's think step by step ）。

```
from transformers import pipeline
 
# 初始化连续对话系统
def initialize_cot_model(model_name):
    cot_model = pipeline(&#x27;text-generation&#x27;, model=model_�me)
    return cot_model
 
# 进行连续对话，模仿CoT
def cot_dialogue(cot_model, prompt):
    response = cot_model(prompt, max_length=50)[0][&#x27;text&#x27;]
    return response
 
# 使用指定的模型名称初始化模型
model_name = &#x27;gpt-3&#x27;  # 例如，可以使用gpt-3或其他任何支持文本生成的模型
cot_model = initialize_cot_model(model_name)
 
# 提供提示词并开始对话
prompt = &quot;你是一个人工智能程序。请像人类一样思考并回答以下问题：&quot;
question = &quot;1 + 1 等于多少？&quot;
full_prompt = prompt + question
response = cot_dialogue(cot_model, full_prompt)
 
print(f&quot;Question: {question}&quot;)
print(f&quot;Answer: {response}&quot;)
```

## Self-Consistency

> 自我一致性是对链式思考提示的一个补充，它能让 GPT 生成多个思维链条，我们从中选取多数答案作为最终结果。

> GPT 会根据示例，给出多种逻辑思维的输出，最后，将多数答案作为最终的答案。

在此案例中，考虑如下多步推理问题：

```
Consider a multi-step reasoning problem like the following:

Question: If a store has 10 apples and 8 oranges, and it sells 6 apples and 4 oranges, how many fruits are left in the store?

Instead of directly answering the question, the chain-of-thought prompting would require the language model to produce a series of short sentences that mimic a human&#x27;s reasoning process:

The store has 10 apples.
The store has 8 oranges.
6 apples are sold.
4 oranges are sold.
There are 10 - 6 = 4 apples left.
There are 8 - 4 = 4 oranges left.
The store now has 4 apples + 4 oranges = 8 fruits.
```

```
问题：如果一家商店有 10 个苹果和 8 个橙子，此时店里卖出了 6 个苹果和 4 个橙子，那么店里还剩下多少水果？

思维链提示（chain-of-thought prompting）不是直接回答问题，而是要求语言模型生成一系列模仿人类推理过程的短句：

商店有 10 个苹果。
店里有 8 个橙子。
卖了 6 个苹果。
卖了 4 个橙子。
还剩下 10 - 6 = 4 个苹果。
剩下 8 - 4 = 4 个橙子。
商店现在有 4 个苹果 + 4 个橙子 = 8 个水果。
```

使用 Self-consistency，语言模型生成多个推理路径：

```
推理路径A：

商店有 10 个苹果。
店里有 8 个橙子。
卖了 6 个苹果。
卖了 4 个橙子。
还剩下 10 - 6 = 4 个苹果。
剩下 8 - 4 = 4 个橙子。
商店现在有 4 个苹果 + 4 个橙子 = 8 个水果。

推理路径 B：

商店最初有 10 个苹果和 8 个橙子。
商店卖了 6 个苹果，所以还剩下 10 - 6 = 4 个苹果。
商店卖了 4 个橙子，所以还剩下 8 - 4 = 4 个橙子。
商店现在有 4 个苹果 + 4 个橙子 = 8 个水果。
```

## Generated Knowledge Prompting

> 是在和 GPT 进行对话之前，人为地给 GPT 输入上下文知识，即使这些知识可能是不符合现实或者是错误的，GPT 可能也会按照你的提示去执行任务。

> 相当于 GPT 在做语文阅读理解，根据你给的材料，结合它现有的知识，回答你提出的问题。

```
# 定义一个简单的问答系统
QA_PAIRS = {
    &quot;你好&quot;: &quot;你好！&quot;,
    &quot;今天的天气如何&quot;: &quot;不好说，因为我是一个人工智能。&quot;,
    &quot;编程语言排行&quot;: &quot;Python 是最流行的编程语言。&quot;
}
 
def generate_answer_from_prompt(prompt):
    for question, answer in QA_PAIRS.items():
        if prompt == question:
            return answer
    return &quot;抱歉，我不知道这个问题的答案。&quot;
 
# 使用示例
prompt = &quot;编程语言排行&quot;
answer = generate_answer_from_prompt(prompt)
print(answer)
```

## Automatic Reasoning & Tool Use (ART)

> 可以通过生成中间思维链(CoT)推理步骤，在很少样本和零样本设置下执行复杂的推理。

> 同时，每个推理步骤都可以依赖外部工具来支持核心LLM功能之外的计算。

> 在测试时，只要调用外部工具，ART就无缝地暂停生成，并在恢复生成之前集成它们的输出。

```
# 假设你使用的是一个大型语言模型，比如GPT-4或其他的模型
# 你需要先设置一个环境，比如使用一个Python库来访问模型，例如使用&#x27;transformers&#x27;库
from transformers import AutoModelForCausalLM, AutoTokenizer
 
# 加载预训练的模型和分词器
tokenizer = AutoTokenizer.from_pretrained(&quot;gpt-4&quot;)
model = AutoModelForCausalLM.from_pretrained(&quot;gpt-4&quot;)
 
# 你的提示词，要求模型展示自动推理的能力
prompt = &quot;请使用自动推理的能力来解决以下问题：
 
假设有三个数字：1, 2, 和 3。你能否通过加减乘除和取余等基本运算，将这三个数字组合起来形成一个等式，使得等式的结果为 4?&quot;
 
# 对输入的提示进行编码
input_ids = tokenizer.encode(prompt, return_tensors=&#x27;pt&#x27;)
 
# 使用模型生成响应
outputs = model.generate(input_ids)
 
# 解码模型的输出并打印
print(tokenizer.decode(outputs[0], skip_special_tokens=True))
```

## Self-Ask Prompting

> 是从**直接提示和****思维链****提示**中发展而来的。

好处在于 LLM 的推理过程被明确展示出来。

> **同时 LLM 还将问题分解成**更小的后续问题。LLM 知道何时达到最终答案，并可以从跟进中间答案转移到最终答案。

```
class LLM:
    def __init__(self):
        pass
    
    def generate(self, prompt):
        # 假设generate方法用于生成文本
        return &quot;这是对问题 &#x27;{}&#x27; 的回答&quot;.format(prompt.split(&#x27;:&#x27;)[-1].strip())
# 创建一个模型实例
model = LLM()
# 用户提出问题
question = &quot;这是一个示例问题&quot;
# 获取模型的回答
answer = model.generate(prompt)
 
# 打印结果
print(answer)
```

## Sequential Prompting

> 专注于推荐系统的排名阶段，因为在大规模候选集上运行 LLMs 的成本更高。

> 排名表现对检索到的排名靠前的候选项非常敏感，更适合检验 LLMs 的推荐能力中的微小差异。

```
from transformers import AutoModel, AutoTokenizer, AutoModelForCausalLM
 
# 加载预训练的模型和分词器
tokenizer = AutoTokenizer.from_pretrained(&quot;t5-base&quot;)
model = AutoModelForCausalLM.from_pretrained(&quot;t5-base&quot;)
 
def generate_sequence(input_str, model=model, tokenizer=tokenizer, max_length=200, temperature=0.7):
    inputs = tokenizer.encode(input_str, return_tensors=&#x27;pt&#x27;)
    outputs = model.generate(inputs, max_length=max_length, temperature_threshold=temperature)
    return tokenizer.decode(outputs[0], skip_special_tokens=True)
 
# 用户连续提问或指令
user_input = &quot;你好，今天天气怎样？&quot;
response = generate_sequence(user_input)
print(response)
 
# 继续提问或指令
user_input += &quot; 能否推荐一个餐馆？&quot;
response = generate_sequence(user_input)
print(response)
```

## Symbolic Reasoning

是一种基于符号推理和可行性逻辑的智能问答技术。

> 可以使用**逻辑推理和知识表示**来解决复杂的问题。

```
import torch
from torch import nn
 
# 定义一个基本的符号推理模块
class SymbolicReasoningModule(nn.Module):
    def __init__(self, num_choices, embedding_dim, hidden_dim):
        super(SymbolicReasoningModule, self).__init__()
        self.num_choices = num_choices
        self.embedding_dim = embedding_dim
        self.hidden_dim = hidden_dim
        
        # 根据具体的推理机制定义模型架构
        # 例如，可以使用LSTM或Transformer来进行符号推理
        self.rnn = nn.LSTM(self.embedding_dim, self.hidden_dim)
        self.fc = nn.Linear(self.hidden_dim, self.num_choices)
 
    def forward(self, question_embeddings, choice_embeddings):
        # 假设question_embeddings和choice_embeddings是通过某种方式获得的
        # 将问题和选项的嵌入序列传递给LSTM
        lstm_out, _ = self.rnn(question_embeddings)
        # 将LSTM的最后一个时间步输出传递给全连接层进行分类
        lstm_last = lstm_out[:, -1, :]
        logits = self.fc(lstm_last)
        return logits
```

```
# 使用该模块的示例
num_choices = 4  # 假设有4个选项
embedding_dim = 100  # 嵌入维度
hidden_dim = 200  # 隐藏层维度
 
# 实例化模块
model = SymbolicReasoningModule(num_choices, embedding_dim, hidden_dim)
 
# 假设question_embeddings和choice_embeddings已经准备好
question_embeddings = torch.randn(10, 5, embedding_dim)  # 10个问题句子，每个句子5个词
choice_embeddings = torch.randn(10, 4, embedding_dim)  # 10个选项，每个选项4个词
 
# 前向传播
logits = model(question_embeddings, choice_embeddings)
```

## Iterative Prompting

> 侧重于推荐系统的排名阶段，因为LLM在大规模候选集上运行更昂贵。

> 排序性能对检索到的排名靠前的候选项目敏感，适合考察LLM推荐能力的细微差异。

```
# 假设我们有一个大型语言模型对象 `language_model`
def iterative_prompting(language_model, prompt, max_iterations=5):
    response = language_model.generate(prompt)
    for _ in range(max_iterations):
        prompt = f&quot;{prompt} {response}&quot;
        response = language_model.generate(prompt)
    return response
```

## Program-Aided Language Models

> **一种使用 LLM 阅读自**然语言问题并生成程序作为中间推理步骤的新方法，但将解决方案步骤卸载（offloads）到例如 Python 解释器运行。

> 这个方法来自于 2022 年，Luyu Gao 等人的研究，根据他们的研究，在 LLM 模型中，即使使用了前面提到的 Chain of Thought 的方法，也未必能拿到答案。

其中使用的 prompt 是这样的：

```
Q: Roger has 5 tennis balls. He buys 2 more cans of
tennis balls. Each can has 3 tennis balls. How many
tennis balls does he have now?
A: Roger started with 5 tennis balls. 2 cans of 3 tennis
balls each is 6 tennis balls. 5 + 6 = 11. The answer is 11.

Q: The bakers at the Beverly Hills Bakery baked 200
loaves of bread on Monday morning. They sold 93 loaves
in the morning and 39 loaves in the afternoon. A grocery
store returned 6 unsold loaves. How many loaves of
bread did they have left?
```

论文里提到，当年他们的模型的 Output 是这样的：

```
A: The bakers started with 200 loaves. They sold 93 in
the morning and 39 in the afternoon. So they sold 93 +
39 = 132 loaves. The grocery store returned 6 loaves. So
they had 200 - 132 - 6 = 62 loaves left. 
The answer is 62.
```

## Least-To-Most Prompting

**关键思想是把一个复杂的问题分解成**一系列更简单的子问题，然后依次解决它们。

> **它由两个连续的阶段组成:**

1.  **分解**。此阶段的提示包含演示分解的固定示例，然后是要分解的特定问题。

1.  **子问题解决**。这一阶段的提示由三部分组成:(1)演示如何解决子问题的恒定示例;(2)之前回答的子问题和生成的解决方案的潜在空列表，以及(3)接下来要回答的问题。

```
# 假设我们有一个大型语言模型的接口 `LLM`
class LLM:
    def respond(self, prompt):
        # 这里应该是与大型语言模型交互的代码
        return &quot;模型回应: &quot; + prompt
 
# 使用LTM策略的函数
def ltm_prompting(llm, task, max_steps=5):
    specific_prompt = f&quot;请{task}并给出解决方案&quot;
    response = llm.respond(specific_prompt)
    print(response)
 
# 实例化LLM模型
my_llm = LLM()
 
# 应用LTM提示策略
task = &quot;解决这个算术问题：100个苹果 + 200个香蕉 = 多少个水果&quot;
ltm_prompting(my_llm, task)
```

## Meta-Prompting

> **使用输入和来自多个不同的**专业专家模型或代码执行的输出解读其自身输出的过程。

> 让语言模型自己决定要使用哪些 prompt 或使用哪些代码段。

> 能提升整体性能，而且在多个不同任务上也往往能实现新的最佳结果。

```
from transformers import pipeline
 
# 初始化一个使用meta-prompting的问答模型
def initialize_model_with_meta_prompt(model_name, meta_prompt):
    model = pipeline(&#x27;text-generation&#x27;, model=model_name)
    model.prompt = meta_prompt
    return model
 
# 使用meta-prompting进行问答
def answer_question_with_meta_prompt(model, question):
    return model(f&#x27;{model.prompt} {question}&#x27;, max_length=50)[0][&#x27;text&#x27;]
 
# 示例使用
model_name = &#x27;google/xlnet-base-cased&#x27;
meta_prompt = &quot;以下是对查询的回应:&quot;
 
# 初始化模型
model = initialize_model_with_meta_prompt(model_name, meta_prompt)
 
# 进行问答
question = &quot;地球上的最高山是哪座山？&quot;
answer = answer_question_with_meta_prompt(model, question)
print(answer)
```

## ReAct Prompting

> 让模型生成问题解决推理轨迹的同时，在推理（Reasoning）中能够利用工具（Action）如搜索引擎等来实现与外部世界的交互，以获取实时的，相对可靠的额外信息（Observation）来辅助推理。

通过推理轨迹生成和实际行动相结合，可提高语言模型的决策可解释性和可信度。

```
from transformers import LLMController, LLMOutput
 
# 初始化LLM控制器
controller = LLMController.from_pretrained(&quot;microsoft/LLaMA-7B-hf&quot;)
 
# 定义ReAct Prompting的函数
def react_prompt(prompt: str, temperature: float = 0.7):
    # 使用ReAct Prompting的提示模板
    prompt = f&quot;ReAct\n&gt; {prompt}\n\n\n&quot;
    # 使用LLM控制器生成输出
    output = controller.generate(prompt, max_length=50, temperature=temperature)
    return output
 
# 使用ReAct Prompting生成内容
prompt = &quot;写一个关于太阳系的科学概述&quot;
output = react_prompt(prompt)
 
# 打印生成的输出
print(output)
```