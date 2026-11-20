
当然了，这种情况也不绝对，主要会受微调的指令数据集影响，由于这篇文档是介绍如何调教LLM，就暂时不展开了。

## 上下文学习 (ICL)

> 参考Stanford的[Blog1](https://ai.stanford.edu/blog/in-context-learning/),[Blog2](https://ai.stanford.edu/blog/understanding-incontext/)

上下文学习（In-Context Learning，ICL）是GPT-3首次提出的一种特殊prompt形式，它已经成为利用大型语言模型（LLMs）的典型方法之一。GPT-3模型展示了一些大模型才具备的突现能力（如：模型规模达到百亿级时才会显现的能力），其中之一便是上下文学习。

### ICL的发展历史

+   2021年初：引入了Prompt learning（提示学习），这是ICL的早期形式。

+   2021年底：发展为Demonstration learning（演示学习），进一步丰富了上下文信息的利用。

+   2022年初：演变为In-context learning（情境学习），标志着ICL技术在理解和应用上下文方面的进一步成熟。

### 什么是ICL？

上下文学习（In-Context Learning）指的是通过提供几个任务示例/说明，让预训练模型理解任务本身。模型无需进行fine-tuning，只需通过几个示例输入和输出对，就能为新输入生成正确的输出。

GPT-n系列模型属于自回归类语言模型，自回归模型的原理是根据当前输入预测下一个词，然后将预测结果与输入拼接，再作为模型的输入进行下一词的预测，如此循环往复。自回归模型的训练目标是通过从超大规模语料库中采样训练样本，让模型根据输入输出一个概率向量。由于文本数据自带标注，我们知道真实的下一个词，因此损失函数采用交叉熵。

预训练好的GPT-3模型拥有一种被称为上下文学习的神奇能力。迁移到新任务时，GPT-3无需重新训练，只需提供任务描述（可选项）和几个示例（任务查询和对应答案，以一对对的形式组织），最后加上模型需要回答的查询。将这些内容打包作为模型的输入，模型便能输出正确的答案。

示例：以翻译英文为法文任务为例，输入格式如下：

![](img/fe7325f306f5dee688de7bc0783a66f8.png)

图 3-x  翻译任务示例

+   第一行：任务描述，告诉模型要做翻译

+   接下来的三行：示例对，包含英文单词和对应的法文单词

+   最后一行：待翻译的英文单词

将以上内容整体作为GPT-3的输入，模型就能输出对应的法文单词。上下文学习的应用非常灵活，除了翻译任务，还可以用于语法修饰甚至代码编写。

此外，GPT-3训练过程中并没有显式提供类似测试阶段任务描述加示例这样的训练数据。然而，由于GPT-3的训练数据量极其巨大（包含wiki、书本期刊、reddit上的讨论等），或许其中已经包含了各种任务类似结构的数据，且GPT-3模型容量足够大，能够记住所有训练数据。

目前，对于上下文学习能力的成因仍是一个开放性问题。为什么只有大规模语言模型才具备该能力？或许仅有模型参数量大还不够，还必须训练数据量足够大，模型才能显现出这种能力。这些问题仍需进一步研究和探索。

### ICL基础原理

通过提[供一](https://arxiv.org/abs/2301.00234)些示范性的<输入-标签>对，在不需要更新模型参数的情况下，ICL可以对新输入进行准确预测。这种能力在各种应用场景中展现了极大的潜力。尽管ICL展示了出色的性能，但其具体工作原理仍然是个未解之谜。为了解释这一现象，清华大学、北京大学和微软的研究人员发表了一篇论文，将语言模型视为一种元优化器（meta-optimizer），并将ICL理解为一种隐性（implicit）的微调。这种视角帮助我们更好地理解ICL的运行机制。

ICL可以分为以下三种类型：

1.  Few-shot Learning：提供多个示例。例如：“将中文翻译为英文。你好->hello，再见->goodbye，购买->purchase，销售->”，模型应预测下一个输出为“sell”。

1.  One-shot Learning：提供一个示例。例如：“将中文翻译为英文。你好->hello，销售->”，模型应预测下一个输出为“sell”。

1.  Zero-shot Learning：无示例。例如：“将中文翻译为英文。销售->”，模型应预测下一个输出为“sell”。

它的主要流程包括以下几个步骤：

![](img/ead45acd0f31a04c057a886ce8ef7ce1.png)

图 3-x  ICL步骤

1.  预训练阶段：通过大规模语料库进行训练，培养语言模型的ICL能力。可选的预热阶段能进一步提升模型的性能。

1.  演示设计：利用预训练的LLM和精心设计的示例，加上适当的评分策略，生成任务的最终输出。

1.  推理阶段：将示例和查询问题结合形成一个提示，输入模型进行预测。示例的质量对ICL效果并没有显著影响。

ICL的强大性能依赖于两个阶段：

1.  训练阶段：训练LLM的ICL能力。语言模型直接在语言建模目标上进行训练，如从左到右的生成。虽然这些模型并没有针对上下文学习进行特别优化，但ICL仍然具有令人惊讶的能力。

1.  推理阶段：LLM根据特定任务的演示进行预测。由于输入和输出标签都在可解释的自然语言模板中表示，因此有多个方向来提高ICL的性能。

随着模型规模和语料库的扩大，研究表明，LLMs可以利用ICL完成一系列复杂的任务，包括解决数学推理问题。这些能力已被广泛验证，显示了ICL作为大型语言模型的一种新兴能力的潜力。

ICL的核心思想基于类比学习。简单来说，ICL通过从给定的示例中提取模式，将这些模式应用于新任务，从而实现准确的预测和解决问题。

![](img/45c6218dd078372ef289dbcef50bc17c.png)

图 3-x  ICL类比学习

在ICL中，首先需要一些示例来形成演示上下文。示例通常使用自然语言模板编写。然后，ICL将一个查询问题和一个演示上下文连接在一起形成一个提示。最后，将其输入到语言模型中进行预测。ICL不需要参数更新，直接对预训练语言模型进行预测。模型被期望学习隐藏在演示中的模式，并相应地做出正确的预测。

ICL作为一种新的范式，具有许多吸引人的优势。首先，演示用自然语言格式编写，提供一个可解释的接口与大型语言模型通信。这种范式通过更改演示和模板使将人类知识纳入语言模型变得容易得多。其次，上下文学习类似于人类的类比决策过程。第三，与有监督学习相比，ICL是一种无监督学习框架。这不仅可以大大降低使模型适应新任务的计算成本，还可以使语言模型即服务成为可能，并且可以很容易地应用于大规模的现实世界任务。

然而，ICL中还有一些问题和性质需要进一步研究。尽管普通的GPT-3模型显示出ICL能力，但通过预训练期间的自适应，能力可以显著提高。此外，ICL的性能对特定的设置很敏感，包括提示模板、上下文示例的选择和示例顺序等。尽管从直观上看是合理的，但ICL的工作机制仍然不明确，很少有研究提供初步解释。

## 思维链 (CoT)

CoT是一种改进的Prompt策略，旨在提升LLM在复杂推理任务中的表现。与ICL不同，CoT不仅关注输入和输出，还特别强调将推理过程中的中间步骤纳入Prompt。这种方法使得模型能够更清晰地展示其思考过程，从而在算术推理、常识推理和符号推理等任务中表现出色。

### 什么是 CoT？

[20](https://arxiv.org/pdf/2201.11903)22 年，Google 发表了一篇论文，提到了一种叫做“思维链”的技术（英文名叫 Chain-of-Thought，简称 CoT）。简单来说，这种方法就是让大模型（像 ChatGPT 这样的 AI 模型）逐步将一个复杂的问题分解为更简单的几个子问题，一步一步解决，直到得出答案。这个分解过程就被称为“思维链”。

为什么要这么做呢？想象一下你在解数学题时，不是一下子直接写出答案，而是把每一个解题步骤都写下来，这样不仅更容易理解，也能让整个过程更清晰。这就是思维链的原理——通过分解问题，让 AI 更容易找到正确答案。

### 传统方法与 CoT 的区别

通常，我们对 AI 的提问就像是从输入到输出的直线过程，比如你问一个问题，AI 直接给你答案。而使用思维链时，这个过程就变成了三个部分：输入 → 思维链（推理过程） → 输出。

简单举个例子，如果让 AI 回答“如果有 5 只苹果，每只苹果值 3 元，总共多少钱？” 传统的方法可能直接回答“15 元”。但使用思维链，AI 会先写出：“5 只苹果，每只 3 元，所以是 5 × 3，答案是 15 元。” 通过展示这些中间步骤，AI 的回答更容易被理解。

![](img/9ebabe8cc2ab41496975159ebf52b88f.png)

图 3-x  CoT与标准prompt步骤对比

上面这张图展示了标准提示（Standard Prompting）和思维链提示（Chain-of-Thought Prompting）的区别。

在左侧的标准提示中，我们看到模型直接给出答案，没有解释过程，因此在面对复杂问题时，模型很容易出错。比如，当被问到“Roger 有 5 个网球，他又买了 2 罐网球，每罐有 3 个网球。现在他一共有多少个网球？”时，模型直接给出了答案“11”。虽然答案正确，但没有展示中间推理步骤，这使得它的可靠性和理解性较低。

在右侧的思维链提示中，模型被要求逐步解释整个过程：“Roger 最开始有 5 个网球，又买了 2 罐网球，每罐有 3 个网球，总共有 6 个网球。5 + 6 = 11。” 通过一步步地展示推理过程，模型不仅得到正确答案，而且让人清楚地看到解题的逻辑。

另一个例子中，食堂有 23 个苹果，用掉了 20 个来做午餐，又买了 6 个苹果，还剩多少个苹果？标准提示下模型回答“27”，这是错误的。而使用思维链提示，模型会一步步解释：“原本有 23 个苹果，做午餐用掉了 20 个，所以还剩 3 个。然后又买了 6 个苹果，所以 3 + 6 = 9 个苹果。” 最终得到正确答案。

### CoT 的不同类型

通过将采用思维链（CoT）方法的提示（Prompt）进行细致的分解，我们可以更深入、更清晰地洞察到思维链的工作机制和流程。根据是否包含示例，我们可以将思维链分为两种：

1.  `Zero-Shot-CoT`：没有提供具体的例子，只是在问题中加一句“让我们一步步思考”（Let's think step by step）。这种简单的提示就能“唤醒” AI 的推理能力。

1.  `Few-Shot-CoT`：给 AI 一些例子，每个例子都包含问题、推理过程和答案，像是给 AI 讲解解题步骤。通过这些例子，AI 就能模仿这些推理步骤解决新问题。

![](img/94d1587b270c16978f4e90f15b26ab92.png)

图 3-x  CoT的分类与实例

上面这张图展示了标准提示（Direct Reasoning）和思维链提示（Chain-of-Thought Reasoning）的区别。

在左侧的直接推理中，我们看到模型直接给出答案，没有解释过程，因此在面对复杂问题时，模型很容易出错。比如，问到“小向日葵有 3 打种子，而大向日葵比小向日葵多 50% 的种子，一共有多少种子？”时，模型直接得出错误的答案“54”。

而在右侧的思维链推理中，模型被要求逐步解释整个过程：“小向日葵有 3 打种子，即 3 × 12 = 36 个种子，大向日葵比小向日葵多 50%，也就是 36 × 0.5 = 18 个种子。所以大向日葵有 36 + 18 = 54 个种子，总共有 36 + 54 = 90 个种子。” 通过一步步展示推理过程，模型不仅得出正确答案，而且让人清楚地看到解题的逻辑。

### 为什么 CoT 有效？

目前为止，还没有一个完全被大家认可的科学解释来说明为什么思维链有效。不过，有很多实验观察到了一些有趣的现象，可以帮助我们理解 CoT：

+   模型规模要足够大：如果 AI 太小（比如理解力不足），思维链可能就不起作用，因为它连基础知识都还不理解。

+   任务不能太简单：对于一些非常简单的问题，思维链没有太大帮助，因为 AI 已经能直接给出答案。

+   训练数据的联系：如果 AI 在训练中学到的数据彼此联系紧密，思维链的效果会更好。

+   示例中的错误：有趣的是，即使给 AI 的示例中有些错误步骤，思维链依然有效。这说明 CoT 更像是在“指挥” AI 去做一步步的推理，而不是教 AI 具体怎么做。

简而言之，思维链的效果，可能在于它强迫 AI 按步骤思考，就像老师要求学生把解题过程写下来一样，不仅是为了得到答案，更是为了更好地理解问题的过程。

## CAMEL 中的 Prompt

现在我们来看看如何使用 CAMEL 的 Prompt 功能，通过实操来更好地理解Prompt的概念。

### 使用 CoT 提示创建特定任务 Agent

CAMEL 提供了一些便捷的工具来帮助用户使用 CoT。我们可以使用 `TaskSpecifyAgent` 创建一个特定任务 Agent，它会自动调用带有 CoT 的模板。例如，下面是代码示例：

```python
from camel.agents import TaskSpecifyAgent

from camel.models import ModelFactory

from camel.types import ModelPlatformType, TaskType

import os

from dotenv import load_dotenv

load_dotenv()

api_key = os.getenv('INTERNLM_API_KEY')

model = ModelFactory.create(
    model_platform=ModelPlatformType.INTERNLM,
    model_type="internlm2.5-latest",
    api_key=api_key
)

task_specify_agent = TaskSpecifyAgent(
    model=model, task_type=TaskType.AI_SOCIETY, output_language='中文'
)

specified_task_prompt = task_specify_agent.run(
    task_prompt="Improving stage presence and performance skills",
    meta_dict=dict(
        assistant_role="Musician", user_role="Student", word_limit=100
    ),
)

print(f"Specified task prompt:\n{specified_task_prompt}\n")

>>> 
Specified task prompt:
Musician will coach Student on dynamic stage movement, audience engagement techniques, and emotional expression during performances, enhancing overall charisma and connection with the audience.
```

在这个例子中，我们使用 `TaskSpecifyAgent` 生成了一个带有思维链的任务提示，通过为 Agent 指定角色，让它逐步生成问题的解决方案。

### 使用自定义 Prompt 模板

CAMEL 还允许用户创建自己的 Prompt 模板，使得生成的 Prompt 更加符合用户的需求。我们可以编写自己的思维链提示模板，然后将它应用到 `TaskSpecifyAgent` 中。下面是一个简单的示例：

```python
from camel.agents import TaskSpecifyAgent

from camel.models import ModelFactory

from camel.prompts import TextPrompt

from camel.types import ModelPlatformType

import os

from dotenv import load_dotenv

load_dotenv()

api_key = os.getenv('INTERNLM_API_KEY')

model = ModelFactory.create(
    model_platform=ModelPlatformType.INTERNLM,
    model_type="internlm2.5-latest",
    api_key=api_key
)

my_prompt_template = TextPrompt(
    'Here is a task: I\'m a {occupation} and I want to {task}. Help me to make this task more specific.'
)  # 你可以根据需求自定义任何模板

task_specify_agent = TaskSpecifyAgent(
    model=model, task_specify_prompt=my_prompt_template
)

response = task_specify_agent.run(
    task_prompt="get promotion",
    meta_dict=dict(occupation="Software Engineer"),
)

print(response)

>>>
```
