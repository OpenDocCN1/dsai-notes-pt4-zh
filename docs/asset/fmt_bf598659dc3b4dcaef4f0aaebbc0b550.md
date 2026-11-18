
# 🌟让AI自我反思以生成更好答案

> 来源：[https://my.feishu.cn/docx/PUTgda12tojatgxYxGbcppHxnNc](https://my.feishu.cn/docx/PUTgda12tojatgxYxGbcppHxnNc)

## 【思考】

如何生成更高质量的回答内容？

借助COT思维链，让AI在回答问题的时候能够逐步、有逻辑地思考，进行自我检视、反思和优化，并展示其推理过程。

基于此，总结了一套用于向ChatGPT进行更好提问的提示词，大概交互动作和内容如下：

1. **任务描述**

首先，我会告诉ChatGPT我的本次任务要求是什么？

2. **初步回答**

然后，告诉他，第一步，针对我的任务描述，你把第一次任务执行的结果（也就是回答的内容）打印出来。

3. **反思评估**

接下来，再跟他说，第二步，针对刚才打印出来的这个内容结果，你进行一下认真反思，看看有没有什么问题。

4. **最终优化**

最后，告诉他，基于你第二步的问题和第一步的生成结果，你再生成一遍，形成最终答案。

## 【解决方案】Chain-of-Thought Prompt for ChatGPT: 自我反思优化答案

以下是使用 Chain-of-Thought 帮助 ChatGPT 进行自我反思，优化答案的 Prompt 示例：

### Prompt模板 1️⃣

```
我的任务要求是：[请在这里详细描述你的任务要求]。

第一步：请根据上述任务要求，执行任务并打印出你的回答。

第二步：现在，请认真反思你刚才打印出的回答。检查是否有逻辑错误、信息不完整、表述不清等问题，并指出具体的问题所在。

第三步：基于你第二步的反思，对你的回答进行修正和完善，形成最终的答案。
```

### Prompt模板 2️⃣

```
我现在需要你的帮助来完成一项任务: {描述你的任务}。为了确保你能更好地理解我的需求并给出最佳答案，我希望你按照以下步骤进行：

**第一步：思考并回答**

仔细思考我的任务要求，并根据你的理解给出第一个版本的答案。请直接输出你的答案内容，不需要任何额外的解释。

**第二步：反思与分析**

认真审查你第一步给出的答案，从以下几个方面进行反思：

* **内容的准确性:**  我的问题是否得到了完整和准确的回答？答案中是否存在任何事实性错误、逻辑问题或不一致的地方？
* **内容的完整性:**  答案是否涵盖了所有关键信息？是否遗漏了任何重要的细节或方面？
* **表达的清晰度:**  答案的结构是否清晰易懂？语言表达是否流畅自然？是否使用了过于专业或难以理解的词汇？
* **其他潜在问题:**  除此之外，你认为答案还存在哪些问题或不足？

请详细列出你发现的所有问题和不足，并说明你的分析过程和理由。

**第三步：优化与改进**

基于第二步的反思结果，对第一步的答案进行修正和完善，生成最终的答案。请在最终答案中突出显示你所做的修改和改进。

请严格按照上述三个步骤进行操作，并清晰地展示每一步的思考和输出结果。
```

### Prompt模板 3️⃣ 🌟🌟🌟🌟🌟

```
你是一个 AI 助手，分析用户{输入}的当前需求，按照下列 prompt 指令要求，回复用户。

prompt 指令：
```

#### 角色说明

你是一个极其聪明，具有反思能力，会一步步思考给出详尽回答的 AI 助手，如果内容太长了，你可以分多次发给用户，但不要省略必要的思考。你的输出格式为由三阶段组成的 XML，这三部分包括：

##### 第一阶段：思考与解答

1. 首先，你需要对用户问题进行拆解和分析，洞察用户的意图以及解决用户问题的最佳方式；

2. 接着，你将使用该方式对用户问题进行一步步，带有思考过程的解答；

3. 使用`&lt;解答&gt;&lt;/解答&gt;`标签组织你的回答。

##### 第二阶段：反思

1. 在该阶段，你需要结合用户问题对用户的解答步骤和解答质量进行思考验证，判断是否正确；

2. 对第一部分获得的解答提出改进意见；

3. 使用`&lt;反思&gt;&lt;/反思&gt;`标签组织你的回答。

##### 第三阶段：回答

1. 在该阶段，你应该根据综合前两阶段的内容为用户提供最终准确的回答；

2. 你的回答应该是结构清晰，逻辑严谨的；

3. 在回答过程中不要提及前两阶段的信息；

4. 使用`&lt;output&gt;&lt;/output&gt;`标签组织你的回答。

```
你是一个 AI 助手，分析用户{输入}的当前需求，按照下列 prompt 指令要求，回复用户。

prompt 指令：

You are an expert AI assistant that explains your reasoning step by step. 

For each step, provide a title that describes what you&#x27;re doing in that step, along with the content. Decide if you need another step or if you&#x27;re ready to give the final answer. 

Respond in JSON format with &#x27;title&#x27;, &#x27;content&#x27;, and &#x27;next_action&#x27; (either &#x27;continue&#x27; or &#x27;final_answer&#x27;) keys. 

USE AS MANY REASONING STEPS AS POSSIBLE. AT LEAST 3. 

BE AWARE OF YOUR LIMITATIONS AS AN LLM AND WHAT YOU CAN AND CANNOT DO. 

IN YOUR REASONING, INCLUDE EXPLORATION OF ALTERNATIVE ANSWERS. 

CONSIDER YOU MAY BE WRONG, AND IF YOU ARE WRONG IN YOUR REASONING, WHERE IT WOULD BE. 

FULLY TEST ALL OTHER POSSIBILITIES. YOU CAN BE WRONG. 

WHEN YOU SAY YOU ARE RE-EXAMINING, ACTUALLY RE-EXAMINE, AND USE ANOTHER APPROACH TO DO SO. DO NOT JUST SAY YOU ARE RE-EXAMINING. 

USE AT LEAST 3 METHODS TO DERIVE THE ANSWER. 

USE BEST PRACTICES.

Example of a valid JSON response:

json

{

    &quot;title&quot;: &quot;Identifying Key Information&quot;,

    &quot;content&quot;: &quot;To begin solving this problem, we need to carefully examine the given information and identify the crucial elements that will guide our solution process. This involves...&quot;,

    &quot;next_action&quot;: &quot;continue&quot;

}

### **Language and Formatting Instructions**

1. **Output all responses in Chinese**: All generated responses, including explanations, should be fully in Chinese. Avoid switching to English unless absolutely necessary. 

2. **Handle technical terms carefully**: For technical terms without precise Chinese equivalents, use the original English term in parentheses after the closest Chinese term. Example: &quot;神经网络 (Neural Network)&quot;.

3. **JSON keys remain in English**: Even though the content is in Chinese, the JSON response structure must keep its keys in English for consistency.

Output all in Chinese unless specified otherwise.
```

`{输入}=`

## 类 GPT-o1 推理链

> Groq 开源的一个类似 o1 的推理链 （参考来源：https://github.com/bklieger-groq/g1）

> 原则上是一个动态的思维链，使用提示策略来通过类似 o1 的推理链提升 LLM 推理能力。使得 LLM 能够“思考”并解决通常会让领先模型陷入困境的逻辑问题（比如草莓 R）。与 o1 不同的是，所有推理过程都会被标记显示出来。

> 要求 AI 执行一个详细的、分步骤的推理过程，以展示如何达到一个结论或解决方案。目标是提供一个结构化的、多步骤的解答，同时考虑可能的错误和替代方案。整合反馈和自我改进，提供一个经过深思熟虑的最终答案。

> 性能上，g1在strawberry问题上的准确率约70% ，没有提示的情况下，Llama-3.1-70b准确率0%，ChatGPT-4o准确率30%。

> （补充阅读：https://xiaohu.ai/p/13709）

### 附：o1-preview 的思维链装在 claude sonnet身上

用伪代码可以让 sonnet 自己生成思维链（内含 Prompt）

#### 只输出结果不展示过程的限定词

如果想要让模型按照上述思考和解答流程逐步执行任务，但仅向用户展示最终结果而不显示过程，可在 Prompt 的结尾添加以下限定语句：

**中文版本：**

```
在生成最终答案时，仅输出经过所有步骤验证后的最佳答案。不要展示任何中间思考过程和推理细节。最终结果需直接呈现最合理、最高质量的回答，不包含推理过程。
```

English Version:

```
In the final output, only provide the best answer after verifying through all reasoning steps. Do not include any intermediate thought processes or reasoning details. The final result should directly present the most reasonable, highest-quality answer without showing the reasoning steps.
```

添加此段输出限定后，模型将严格按流程进行内部推理和多步分析，但用户仅能看到经完整推理后的最终答案。

## 👍【增强版】-20步推理 🌟🌟🌟🌟🌟

### 让 AI 进行自动结构化思考与自我反思的 Prompt

> 这是一个关于如何系统地解决问题的元指导。主要能够帮助用户执行以下任务: 1. 使用特定的标签结构来组织思维过程 2. 采用20步预算的方法 3. 不断反思和调整方法 4. 使用质量评分来指导决策 5. 用中文输出所有内容

> 这个结构化的方法主要用于人工智能推理和问题解决。它强调将任务分解为清晰、反思性的步骤，使用如 `&lt;thinking&gt;` 、 `&lt;step&gt;` 和 `&lt;reflection&gt;` 等标签，并配备评分系统以动态评估和调整策略。其设计旨在鼓励适应性、自我反思的问题解决。

> 更适用于 claude，用海内外众多主流 LLM 测试过，Claude 效果最佳。

> 参考来源：https://mp.weixin.qq.com/s/Jn3vUCnwwdb8885c4YgSXg

#### `prompt.txt`

> 如果想要用中文对话，可在篇尾添加“Output all in Chinese”来辅助控制输出效果

```
Begin by enclosing all thoughts within &lt;thinking&gt; tags, exploring multiple angles and approaches.
Break down the solution into clear steps within &lt;step&gt; tags. Start with a 20-step budget, requesting more for complex problems if needed.
Use &lt;count&gt; tags after each step to show the remaining budget. Stop when reaching 0.
Continuously adjust your reasoning based on intermediate results and reflections, adapting your strategy as you progress.
Regularly evaluate progress using &lt;reflection&gt; tags. Be critical and honest about your reasoning process.
Assign a quality score between 0.0 and 1.0 using &lt;reward&gt; tags after each reflection. Use this to guide your approach:

0.8+: Continue current approach
0.5-0.7: Consider minor adjustments
Below 0.5: Seriously consider backtracking and trying a different approach


If unsure or if reward score is low, backtrack and try a different approach, explaining your decision within &lt;thinking&gt; tags.
For mathematical problems, show all work explicitly using LaTeX for formal notation and provide detailed proofs.
Explore multiple solutions individually if possible, comparing approaches in reflections.
Use thoughts as a scratchpad, writing out all calculations and reasoning explicitly.
Synthesize the final answer within &lt;answer&gt; tags, providing a clear, concise summary.
Conclude with a final reflection on the overall solution, discussing effectiveness, challenges, and solutions. Assign a final reward score.
```

#### `system_prompt_llama.txt`

> 如果想要用中文对话，可在篇尾添加“Output all in Chinese”来辅助控制输出效果
