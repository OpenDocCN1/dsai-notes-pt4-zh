
```
MODELS=`[  {    &quot;name&quot;: &quot;OpenAssistant/oasst-sft-4-pythia-12b-epoch-3.5&quot;,    &quot;datasetName&quot;: &quot;OpenAssistant/oasst1&quot;,    &quot;description&quot;: &quot;A good alternative to ChatGPT&quot;,    &quot;websiteUrl&quot;: &quot;https://open-assistant.io&quot;,    &quot;userMessageToken&quot;: &quot;&lt;|prompter|&gt;&quot;, # This does not need to be a token, can be any string    &quot;assistantMessageToken&quot;: &quot;&lt;|assistant|&gt;&quot;, # This does not need to be a token, can be any string    &quot;userMessageEndToken&quot;: &quot;&lt;|endoftext|&gt;&quot;, # Applies only to user messages. Can be any string.    &quot;assistantMessageEndToken&quot;: &quot;&lt;|endoftext|&gt;&quot;, # Applies only to assistant messages. Can be any string.    &quot;preprompt&quot;: &quot;Below are a series of dialogues between various people and an AI assistant. The AI tries to be helpful, polite, honest, sophisticated, emotionally aware, and humble-but-knowledgeable. The assistant is happy to help with almost anything, and will do its best to understand exactly what is needed. It also tries to avoid giving false or misleading information, and it caveats when it isn&#x27;t entirely sure about the right answer. That said, the assistant is practical and really does its best, and doesn&#x27;t let caution get too much in the way of being useful.\n-----\n&quot;,    &quot;promptExamples&quot;: [      {        &quot;title&quot;: &quot;Write an email from bullet list&quot;,        &quot;prompt&quot;: &quot;As a restaurant owner, write a professional email to the supplier to get these products every week: \n\n- Wine (x10)\n- Eggs (x24)\n- Bread (x12)&quot;      }, {        &quot;title&quot;: &quot;Code a snake game&quot;,        &quot;prompt&quot;: &quot;Code a basic snake game in python, give explanations for each step.&quot;      }, {        &quot;title&quot;: &quot;Assist in a task&quot;,        &quot;prompt&quot;: &quot;How do I make a delicious lemon cheesecake?&quot;      }    ],    &quot;parameters&quot;: {      &quot;temperature&quot;: 0.9,      &quot;top_p&quot;: 0.95,      &quot;repetition_penalty&quot;: 1.2,      &quot;top_k&quot;: 50,      &quot;truncate&quot;: 1000,      &quot;max_new_tokens&quot;: 1024,      &quot;stop&quot;: [&quot;&lt;|endoftext|&gt;&quot;]  # This does not need to be tokens, can be any list of strings    }  }]`
```

## GPT4ALL

![](img/302430d49be1bcab6b966f121bee4a67.png)

### M1 macOS设备上运行

`gpt4all`是一个主要面向端设备的大模型`ChatServer`。该项目相较于`llama.cpp`这样的底层引擎来比，封装度更高，做到了开箱即用，使用它会有一种本地使用`chatGPT`的错觉感。`GPT4All`的模型是一个3GB-8GB的文件，可以下载并插入`GPT4All`开源生态系统软件。作为`Gpt4all`的发起者Nomic AI支持并维护这一软件生态系统，以加强质量和安全，同时带头让任何人或企业都能轻松地训练和部署自己的边缘大型语言模型。由于定位在个人pc，覆盖mac，windows，ubuntu，也不要求GPU，一键安装，能够完成常见的问答，个人助理，文章总结，写代码等常见任务场景，受到了很多开发者的青睐，该项目当下已经获得了52.5k收藏，可以看出其受欢迎程度。

`gpt4all`基于模块定位分为4个项目，分别是`gpt4all-backend`（内置的经过高度优化推理引擎），`gpt4all-bindings`(面向不同语言的高阶的编程接口)，`gpt4all-api`（经过封装的推理服务，对外暴露`rest API`），`gpt4all-chat`（客户端UI）。

`GPT4ALL`在模型上支持`Falcon`，`LLama`。`Mpt`，`GPT-J`架构下的模型，并对这些模型进行了量化，以便能够在PC上运行。另一方面，`gpt4all`将`GGML`，`llama.cpp`作为其中一个推理后端，因此，`llama.cpp`支持的模型，`gpt4all`都可以支持。

![](img/e7c7a29259ce423bf88a4436f2e01735.png)

除此之外，`gpt4all`有一个特色功能就是支持插件，通过插件可以极大地扩展`LLM`的能力，从而使得`gpt4all`进一步地向`chatgpt`靠拢。但遗憾的是，可能是受限于`LLM`的能力，它的插件（`LocalDocs`）还仅仅类似于`RAG`的检索增强，还无法做到采用`Re-Act`模式根据`prompt`及返回内容按需调起。

![](img/99894356b12d59724eab834e5c14cd25.png)

`gpt4all`也支持`server`模式部署，对外提供`OpenAI`风格的推理接口，并且`gpt4all`与`langchain`等框架无缝集成，开发者想要使用`gpt4all`作为后端也是方便的。

## 小结

对于`ChatServer`来讲，已经是面向场景需求高度产品化后的产物，这也是为什么前面四个产品，功能差异明显，受众各异，这本质是一个“产品选用户”的逻辑，很难做到完全契合每一个业务实际的需求。在这个层面，选择产品并用在正确的地方更重要，因而会有一定的”选择成本“。对于有更高灵活性，个性化的场景，那就需要从推理服务，甚至推理引擎层面来集成，越底层，就越灵活，同时，开发门槛和使用成本就越高，因此，直接选择产品还是底层框架工具，这是结合实际情况平衡的问题。

## 选型建议

### 路线 1

`k8s` + `triton server`+`vLLM`，或者未来的`triton server` + `tensorRTLLM`，绑定英伟达，在软硬一体化优化层有优势。

### 路线 2

`k8s` + `kubeRay` + `rayLLM`（`ray serve`）+ `vLLM`/`TGI`，`Ray`作为分布式机器学习全链路的基础设施，在统一基建层面上有优势。

## 5）Prompt

在前面的内容里，我们提到过要使用模型完成下游任务，有两种方式，一种是通过收集标记样本针对不同的任务进行指令微调，另一种方式便是大模型特有的，可以通过将指令以对话的方式提供给模型，期待模型能够给我们返回预期的结果。相较于前者，后者具有更高的灵活性，使用成本也更低，因此，这一方式成了如今大语言模型区别于传统`NLP`模型的重要标志。

在本章你将学习到：

1）`Prompt`，`In-Context-Learning`等相关概念

2）`Prompt engineering`（如何写好一个`Prompt`及相关`Prompt`调试工具）

3）基于`Prompt`催生的一些新的编程范式

### 认识Prompt

`Prompt`，中文翻译为提示（词），在维基百科中，它的定义是这样的：

> A prompt is natural language text describing the task that an AI should perform.

提示是描述人工智能应该执行的任务的自然语言文本。实际上，也很好理解，在原有通过指令微调的方式训练的模型，它要完成什么任务，前期已经通过微调内化到模型里，一个模型干一件事情，因此，在使用时直接使用即可，比如情感分析模型，输入“今天天气很不错”，模型自然就会按照约定回答“正向”。而利用提示的方法，模型本身是通用的，可以做很多任务，这时候，我们就需要告诉它，需要让它做什么，比如：“下面我将给你一个句子，请帮我判断它的情感是正向还是负向，仅回答，无须解释”。

![](img/6ff8df6de282dab78cc0c5258c0ab23c.png)

实际上，`prompt`这个思路并不是近来有之，早在2015年就有人提出这一概念《Ask Me Anything: Dynamic Memory Networks for Natural Language Processing》，因为所有的`nlp`任务都可以归纳为“问答”这样一种统一模式，那时候`prompt`还叫做“question”，这种做法叫做`Prompt learning`。

![](img/0addbf3f040308052157d8aeae08a64f.png)

https://arxiv.org/pdf/1506.07285.pdf

但是，虽然想法很好，实现上却很困难，当下用历史后视镜来看，想要实现统一，一是要选对模型结构（`GPT`），二是要有足够的参数规模，学到足够多的世界知识，进行有效的指令微调对齐。当时，大部分企业和研究者选择了基于`bert`来解决`nlp`任务，相关`bert`的研究也是非常的多，网络上关于`PLM`（预训练大模型）的内容，百分之八十都是与此相关的。而`OpenAI`选择了一个当时看来很难但不一定正确的路线，2018年6月发布了`GPT1`，参数量1.17亿，模型大小5gb，而`bert`仅几百M，效果在具体任务上却比`GPT`好，但他们并没有放弃，也正是如此，在2023年，`ChatGPT`一炮而红，带领人类进入 `AI 2.0`时代，众多`NLP`原本热门的子方向变得前景暗淡。

### In-Context Learning

了解了`prompt`的由来和历史，我们接下来自然要问的问题是，如何让模型更有效的理解指令，并能够高质量的执行。

![](img/d4299df419bd9c932e62106ad96e4e95.png)

这一问题如果改为“如何在`github`上有效提交一个`issue`？”，可能比较容易理解，那么自然就是首先清晰的描述自己的问题，进一步的提供自己的环境信息，以及你的输入，预期的输出等信息。而这些附加的信息我们叫做“`context`”，即上下文。对于模型来讲，我们仍然可以以这样人类熟悉的方式与模型进行交互，让它按照我们的预期工作，这就是上下文学习（`In-Context-Learning`），缩写为`ICL`。

![](img/f767860112ae5c58502ff514cec3fc60.png)

> 生物学家、物理学家、生态学家和其他科学家使用「涌现」一词来描述当一大群事物作为一个整体时出现的自组织、集体行为。
>
> 比如无生命的原子组合产生活细胞; 水分子产生波浪; 椋鸟的低语以变化但可识别的模式在天空中飞翔; 细胞使肌肉运动和心脏跳动。

通过上面的描述可以知道，上下文学习是一种智能涌现（`emergence`）， 其发生的前提是它拥有了和人类似的思维模式。虽然当下还无法完全知道智能涌现的原因。 但就目前研究表明，模型发生智能涌现可能的原因是当学习的资料及模型参数规模大到一定程度 （`10 B` 以上 ） ，引发了质变，从而将模型中本来蕴含的知识和逻辑激发出来，表现出了智能。 另一方面， 近期的研究 也表明涌现和 训练资料的质量也有一定的关系，因此，高质量的数据是 大模型 智 能程度 的一个重要因素 ，其研究 结果表明，综合语言能力的表现在很大程度上取决于训练前语料的来源而不仅仅是规模。 就像人脑也是由一个个的神经元构成，当神经元多到一定程度，脑容量大到一定程度，人就出现了意识。

![](img/2cbe8e141ef04dcdbe3b6ba97dc04de4.png)

确切的讲，`In-Context learning`（`ICL`）是在大模型发生智能涌现的前提下的一种新的模型学习任务处理的范式，在`GPT-3`论文发布后，这一方法被广泛被接受。它允许语言模型通过以演示的形式组织若干个示例或者指令来学习任务。

![](img/3bd83fd1be83fed4eb99e570dd3c50fa.png)

可以看出，`In-context-learning`作为`Prompt Learning`的一个部分，通过将上下文信息提供给大模型，能够大大的提升了操纵大模型的方式，从而更好的挖掘大模型的潜力。

![](img/500797a72114d5985c35d24ee60b6cf7.png)

`In-context Learning`在基础形式及例子的数量上分有三类：`zero-shot`（零示例），`one-shot`（一个示例），`fewshot`（一些示例）。

![](img/e4f7ee423a57dd430d53ea49ec4d0f7c.png)

+   `zero-shot`，实际上在本文最开始例子里就属于这种形式，我并没有提供大模型情感分析的例子的上下文，而是直接问问题。

![](img/91373cda873ddedef354e28dc2264a96.png)

在`zero-shot`中，没有提供例子，并不表示不能给它提供一些其他的信息，比如，我不希望它对它的回答进行解释，如前面例子，在提供`prompt`时，我增加了“ 仅回答，无须解释 ”这样的约束性质的要求，大模型能够很好的遵从。

![](img/3a5a9328edb6b96ed7eb319fa87a9e8c.png)

但假如我们仍然觉得它的回答有点啰嗦，希望对回复格式有要求，就希望它回复“正向”或者“负向”，那又应该怎么做呢？这里就用到了“`few-shot`和`one -shot`”。

+   `few-shot`及`one-shot`，`one-shot`可以理解为`few-shot`的一个特例，它提供给大模型仅一个例子，而`few-shot`可以是多个。以刚才问题为例：我们提供这样的`prompt`：

文心一言

![](img/e7efd8a3154b9a8fdff851faa4501b22.png)

`ChatGPT`

可以看出，大模型一定程度遵从了指令，并且我在例子里并没有完全正确给出结果，如“这本书写的太糟糕了”，例子输出是“正向”，但大模型在实际任务输出过程中，仍然能够正确输出，这体现了大模型的性能具有较强的的鲁棒性和智能性。不过遗憾的是，文心一言未能完全按照例子输出，在后面增加了标点符号，而`ChatGPT`正确执行了指令。因此，指令遵从的程度也反映了大模型的智能程度。

看到这里，一定会有一个疑问：`In-Context learning`仅仅是提供给大模型一些上下文输入，并没有修改模型的权重，那么，为什么它会有效呢？

![](img/6ba0738d024a2dfab4d2cddebc370170.png)

https://arxiv.org/pdf/2212.10559.pdf

Dai 等人的论文【 Why Can GPT Learn In-Context? Language Models Implicitly Perform Gradient Descent as Meta-Optimizers 】 探讨了这一问题的答案，他们在论文中探讨了在 `prompt`中提供 示例与使用相同示例进行微调之间的数学联系。 作者证明，提示示例会产生元梯度（ `meta-gradients` ） ，这些元梯度会 在推理时的前向传播过程中反映出来。 而在微调时，示例实际上会产生真正的梯度，用于更新权重。 因此，`In-Context Learning`能取得与微调类似的效果。
