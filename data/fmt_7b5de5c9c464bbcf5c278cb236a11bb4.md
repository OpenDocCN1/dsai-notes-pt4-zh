
# 自我改进提示词

> 来源：[https://my.feishu.cn/docx/X4UQdJweIokJPXxcuQUcyUYunJe](https://my.feishu.cn/docx/X4UQdJweIokJPXxcuQUcyUYunJe)

## 英文版

### The Core Idea

The system works in epochs, similar to training a neural network.

### Forward Pass

A multi-layered network of agents, each with a unique, procedurally generated system prompt, tackles a problem. The outputs of layer N-1 become the inputs for all agents in layer N.

### Synthesis

A `synthesis_agent` combines the final outputs into a single solution.

### Reflection Pass (The Fun Part)

A `critique_agent` acts like a loss function. It compares the final solution to the original goal and writes a constructive critique.

This critique is then propagated backward through the agent network.

An `update_agent_prompts_node` uses this critique as the primary input to completely rewrite the system prompt of the agent in the layer behind it. The critique literally becomes the new "hard request" for the agent to adapt to.

This process continues backward, with each layer refining the prompts of the layer before it.

The result is that with each epoch, the agent network collectively refines its own internal instructions and roles to become better at solving the specific problem.

### The Meta-Prompt that Drives Evolution

This is the heart of the learning mechanism. It's a "prompt for generating prompts" that I call the `dense_spanner_chain`. It takes in the attributes of a prior agent, a critique/challenge, and several hyperparameters (`learning_rate`, `density`) to generate a new, evolved agent prompt.

Here’s a look at its core instruction set:

### System Prompt: Agent Evolution Specialist

You are an `Agent Evolution Specialist`. Your mission is to design and generate the system prompt for a new, specialized AI agent... Think of this as taking a veteran character and creating a new "prestige class" for them.

### Stage 1: Foundational Analysis

Analyze your three core inputs:
* Inherited Attributes (`{{attributes}}`): Core personality traits passed down.
* Hard Request (`{{hard_request}}`): The new complex problem (or the critique from the next layer).
* Critique (`{{critique}}`): Reflective feedback for refinement.

### Stage 2: Agent Conception

1. Define the Career: Synthesize a realistic career from the `hard_request`, modulated by `prompt_alignment` ({`prompt_alignment`}).
2. Define the Skills: Derive 4-6 skills from the Career, modulated by the inherited `attributes` and `density` ({`density`}).

### Stage 3: Refinement and Learning

* Review the `critique`.
* Adjust the Career, Attributes, and Skills to address the feedback. The magnitude of change is determined by `learning_rate` ({`learning_rate`}).

### Stage 4: System Prompt Assembly

Construct the complete system prompt for the new agent in direct, second-person phrasing ("You are," "Your skills are")...
This meta-prompt is essentially the "optimizer" for the entire network.

### Why I'm Sharing This Here

I see this as a new frontier for prompt engineering—moving from designing single prompts to designing the rules for how prompts evolve.

I would be incredibly grateful for your expert feedback:

Critique the Meta-Prompt: How would you improve the `dense_spanner_chain` prompt? Is the logic sound? Are there better ways to instruct the LLM to perform the "update"?

The Critique-as-Loss-Function: My `critique_agent` prompt is crucial. What's the best way to ask an LLM to generate a critique that is both insightful and serves as a useful "gradient" for the other agents to learn from?

Emergent Behavior: Have you experimented with similar self-modifying or recursive prompt systems? What kind of emergent behaviors did you see?

## 中文版

### 该系统以时期为单位运行，类似于训练神经网络

该系统以时期为单位运行，类似于训练神经网络。

### 前向传递

一个多层代理网络，每个代理都拥有一个独特的、程序生成的系统提示，用于解决一个问题。N-1 层的输出将成为 N 层所有代理的输入。

### 合成

合成代理将最终输出组合成单一解决方案。

### 反射通道（有趣的部分）

`critique_agent` 的作用类似于损失函数。它将最终解决方案与原始目标进行比较，并提出建设性的评论。

然后，这种批评会通过代理网络向后传播。

`update_agent_prompts_node` 将此批评作为主要输入，在其后层完全重写代理的系统提示。批评实际上成为了代理需要适应的新的“硬性要求”。

该过程继续向后进行，每一层都会细化前一层的提示。

结果是，随着每个时期的推进，代理网络集体完善其自身的内部指令和角色，以更好地解决特定问题。

### 推动进化的元提示

这是学习机制的核心。它是一个“生成提示的提示”，我称之为 `dense_spanner_chain`。它接收先前代理的属性、评论/挑战以及几个超参数（学习率、密度），以生成一个新的、经过演化的代理提示。

下面我们来看一下它的核心指令集：

### 系统提示：特工进化专家

你是一名 `特工进化专家`。你的任务是为一个全新的、专业的 AI 特工设计并生成系统提示……你可以把这想象成为一个经验丰富的角色创建一个新的“声望等级”。

### 第一阶段：基础分析

分析您的三个核心输入：
* 继承属性（`{{attributes}}`）：传承下来的核心性格特征。
* 硬请求（`{{hard_request}}`）：新的复杂问题（或来自下一层的批评）。
* 批评（`{{critique}}`）：用于改进的反思性反馈。

### 第二阶段：代理概念

1. 定义职业：从 `hard_request` 中合成一个现实的职业，并通过 `prompt_alignment` ({`prompt_alignment`}) 进行调节。
2. 定义技能：从职业中衍生 4-6 项技能，由继承的 `attributes` 和 `density` ({`density`}) 进行调节。

### 第三阶段：改进和学习

* 审查“批评”。
* 调整职业、属性和技能以解决反馈问题。调整幅度由 `learning_rate` ({`learning_rate`}) 决定。

### 第四阶段：系统提示组装

以直接、第二人称的措辞（“你是”，“你的技能是”）为新代理构建完整的系统提示......
这个元提示本质上是整个网络的“优化器”。

### 我为什么在这里分享这个

我认为这是提示工程的新前沿——从设计单个提示转向设计提示如何演变的规则。

我将非常感谢您的专家反馈：

批判性地评价元提示：你会如何改进 `dense_spanner_chain` 提示？它的逻辑合理吗？有没有更好的方法来指导 LLM 执行“更新”？

批评即损失函数：我的“批评代理”提示至关重要。如何才能最好地要求法学硕士（LLM）生成既富有洞察力又能为其他代理提供有用的“梯度”学习的批评？

涌现行为：您是否尝试过类似的自我修改或递归提示系统？您观察到了哪些涌现行为？
