
# E3-LazyLLM +书生大模型实践

> 来源：[https://aicarrier.feishu.cn/docx/SyOjdh4RLo56jZx2apIcCXNvnkc](https://aicarrier.feishu.cn/docx/SyOjdh4RLo56jZx2apIcCXNvnkc)

> 文档尚处于草稿阶段，完善中

## 第一章 LazyLLM框架介绍

### 1.1 什么是LazyLLM

`LazyLLM`是由商汤 `LazyAGI` 团队开发的一款开源低代码大模型应用开发工具，提供从应用搭建、数据准备、模型部署、微调到评测的一站式工具支持，帮助开发者以极低的成本快速构建 AI 应用，并持续迭代优化效果。

![](img/4d43bd29b44786c8c1285e8919ec7cfa.png)

`LazyLLM`为同一模块的不同技术选型提供了一致的使用体验 -- 统一调用、统一服务、统一部署。

对于初级开发者，`LazyLLM` 彻底简化了AI应用的构建过程。用户不必了解不同模型 `API` 服务的构建细节，无需在微调模型时选择框架或切分模型，也不需要掌握任何Web开发知识，通过预置的组件和简单的拼接操作，初级开发者便能轻松构建出具备生产价值的工具。

对于资深的专家，`LazyLLM` 提供了极高的灵活性，为开发者提供了无限的可能性。其模块化设计支持高度的定制与扩展，使用户能够轻松集成自有算法、行业领先的生产工具以及最新的技术成果，从而快速构建适配多样化需求的强大应用。

### 1.2 重要特性

**📌便捷的AI应用组装流程：**即使不了解大模型，开发者仍然可以像搭积木一样，借助内置的数据流（`Flow`）和功能模块，轻松组建包含多个`Agent`的AI应用。

**📌复杂应用一键部署：`LazyLLM`**提供一键部署所有模块的能力，通过一套轻量的网关机制，简化了多`Agent`应用的部署流程，解决了依次启动各个子模块（如`LLM`、`Embedding`等）服务并配置`URL`的问题，使整个过程更加顺畅高效。同时，`LazyLLM`则提供了一键封装镜像的能力，使得应用可以方便地利用`k8s`的网关、负载均衡、容错等能力。由此，无论是POC还是项目落地，有了`LazyLLM`，这些都不是问题。

**📌跨平台兼容：无**需修改代码，即可一键切换`IaaS`平台，目前`LazyLLM`兼容裸金属服务器、开发机、`Slurm`集群、公有云等。这使得开发中的应用可以无缝迁移到其他`IaaS`平台，大大减少了代码修改的工作量。

**📌高效的模型微调：`LazyLLM`**支持对应用中的模型进行微调，持续提升应用效果。根据微调场景，自动选择最佳的微调框架和模型切分策略。这不仅简化了模型迭代的维护工作，还让开发者们能够将更多精力集中在算法和数据迭代上，而无需处理繁琐的工程化任务。

### 1.3 本次实践内容概述

本次实践我们将为大家展示`LazyLLM`的基本使用方法，并结合书生大模型实现聊天机器人（`Chatbot`）、`RAG`应用以及书生大模型微调（`Finetune`），同时受益于`InternVL`强大的多模态能力，实践将同样展示上述应用的多模态版本搭建。

接下来就让我们来看看`LazyLLM`与书生大模型能够碰撞出怎样的火花🔥吧~

## 第二章 LazyLLM环境配置

> **在**使用`LazyLLM`搭建`RAG`系统之前，我们需要配置好相关的环境，包括“开发环境搭建”和“环境变量配置”两个步骤。

### 2.1 开发环境搭建

您可以用以下任一方式搭建 `LazyLLM` 的开发环境。

#### 手动配置✏️

`LazyLLM` 基于 `Python` 开发，我们需要保证系统中已经安装好了 `Python`、`Pip` 和 `Git`。在`Macbook`上安装这些比较复杂，安装方式见最后附录。

首先准备一个名为 `lazyllm-venv` 的虚拟环境并激活：

```
python -m venv lazyllm-venv
source lazyllm-venv/bin/activate
```

如果运行正常，你可以在命令行的开头看到 `(lazyllm-venv)` 的提示。接下来我们的操作都在这个虚拟环境中进行。

从 `GitHub` 下载 `LazyLLM` 的代码：

```
git clone https://github.com/LazyAGI/LazyLLM.git
```

并切换到下载后的代码目录：

```
cd LazyLLM
```

安装基础依赖：

```
pip3 install -r requirements.txt
```

把 `LazyLLM` 加入到模块搜索路径中：

```
export PYTHONPATH=$PWD:$PYTHONPATH
```

这样我们在任意目录下都可以找到它。

#### 拉取镜像📑

我们提供了包含最新版本的 `LazyLLM` 的 `docker` 镜像，开箱即用：

```
docker pull lazyllm/lazyllm
```

也可以从 [https://hub.docker.com/r/](https://hub.docker.com/r/lazyllm/lazyllm/tags)lazyllm/lazyllm/tags 查看并拉取需要的版本。

#### 从Pip安装👉

`LazyLLM` 支持用 `pip` 直接安装,下面三种安装方式分别对应不同功能的使用：

安装 `LazyLLM` 基础功能的最小依赖包。可以支持线上各类模型的微调和推理。

```
pip3 install lazyllm
```

安装 `LazyLLM` 的所有功能最小依赖包。不仅支持线上模型的微调和推理，而且支持离线模型的微调（主要依赖 `LLaMA-Factory`）和推理（大模型主要依赖 `vLLM`，多模态模型依赖`LMDeploy`，`Embedding`模型依赖`Infinity`）。

```
pip3 install lazyllm
lazyllm install standard
```

安装 `LazyLLM` 的所有依赖包，所有功能以及高级功能都支持，比如自动框架选择（`AutoFinetune`、`AutoDeploy` 等）、更多的离线推理工具（如增加 `LightLLM` 等）、更多的离线训练工具（如增加 `AlpacaloraFinetune`、`CollieFinetune` 等工具）。

```
pip3 install lazyllm
lazyllm install full
```

### 2.2 环境变量配置 - 测试你的LazyLLM

接下来我们通过`模型调用`的方式测试`LazyLLM`的环境是否搭建完毕。

`LazyLLM`支持调用`线上模型`和`本地模型`，为二者提供了一致的使用体验，做到模型即黑盒，您只需关注输入输出及相关参数的具体取值，而无需研究不同模型调用上的差异。

![](img/0131faed9bb0d3d65a64c6d387fb1e64.png)

#### 调用在线大模型 🌏

`LazyLLM` 通过`OnlineChatModule`统一调用线上大模型接口，您只需要根据自己的需求将模型名称等参数传给对应的模块即可，这里我们选择书生官方的api平台调用线上模型。

##### 书生API-KEY配置

首先需要在`https://internlm.intern-ai.org.cn/api/tokens`申请专属的`api token`。

![](img/a8b481aef63257d12ea48f02d411fefe.png)

随后使用以下命令设置对应的环境变量：

```
export LAZYLLM_OPENAI_API_KEY=<申请到的api token>
```

[由于书生 Chat API](https://github.com/InternLM/lmdeploy) 完全基于开源版本 `LMDeploy` 部署，且兼容了 `openai` python sdk，因此我们可以直接使用`LazyLLM`的在线聊天模块`OnlineChatModule`，配置来源类型为`openai`并填入对于的`base_url`即可。

实例化`OnlineChatModule`，指定模型来源并写入`base_url`，即可使用书生大模型进行聊天：

```
from lazyllm import OnlineChatModule
llm = OnlineChatModule(
    source='openai',
    base_url="https://chat.intern-ai.org.cn/api/v1/",
    model="internlm3-latest",
    stream=True
)
print(llm("介绍一下你自己"))
```

如果你得到了与下图相似的输出，证明你的`LazyLLM`环境搭建完毕了。

![](img/0c503338f0b9c3ccf21e6ecc70a65601.png)

#### 调用本地大模型 💻

如果您期望使用本地部署大模型，且有充足的`GPU`资源，`LazyLLM` 提供的`TrainableModule` 能够为所有本地模型(包括`LLM`、`VLM`、`Embedding`模型等)提供服务，用于本地模型的训练、微调和推理。

在这里我们只需将书生大模型所在的路径传入 `LazyLLM` 的`TrainableModule`，然后通过`start()` 函数启动服务即可。这里需要您传入模型文件的绝对路径，或者配置环境变量`LAZYLLM_MODEL_PATH`指定模型所在的目录。如果您本地没有该模型，`LazyLLM`会为您下载对应模型并存入模型缓存目录，默认情况下会下载到您的家目录下的 `".lazyllm/model"` 目录；您可以通过配置 `LAZYLLM_MODEL_CACHE_DIR`环境变量指定模型缓存目录。

`LazyLLM` 支持多种推理框架，如：`LMDeploy`、`LightLLM` 和 `vLLM`等，在不指定明确框架下`LazyLLM`会根据模型大小和测试数据等信息，自动为用户选择合适的推理框架。

这里我们使用`LMDeploy`框架本地部署`internlm2-chat-7b`模型：

```
import lazyllm
from lazyllm import deploy

llm = lazyllm.TrainableModule('internlm2-chat-7b').\
        deploy_method(deploy.LMDeploy).start()
print(llm('你是谁'))
```

如果你得到了与下图相似的输出，证明你成功使用`LazyLLM`完成了书生大模型的本地部署。

![](img/9f9ce44d6f62ad8cf8c978e9f7e3e11d.png)

## 第三章 使用LazyLLM + 书生大模型搭建Chatbot

在第二章中，我们学习了如何配置`LazyLLM`的开发环境，并掌握了使用`LazyLLM`调用线上以及本地部署书生大模型。本章将在第二章的基础上介绍如何使用`LazyLLM` + 书生大模型搭建聊天机器人。

### 3.1 Prompt配置使用💭

#### 基础的Prompt

使用 `LazyLLM` 提供的 `Prompt` 模板，您只需要在初始化大模型时通过如下语法进行定义，在调用时直接传入用户输入即可。下面定义了两个大模型`llm1`和`llm2`，`llm1`是一个默认的在线大模型，`llm2`是一个自定义了prompt的在线大模型：

```
import lazyllm

llm1 = lazyllm.OnlineChatModule()
llm2 = lazyllm.OnlineChatModule().prompt("你是一只小猫，在每次回答问题之后都要加上喵喵喵")

print('普通情况下模型的输出:   ', llm1('你好'))
print('自定义Prompt后模型的输出: ', llm2('你好'))
```

让我们看看上述代码的输出：

```
普通情况下模型的输出:    你好！有什么我可以帮助你的吗？
自定义Prompt后模型的输出:  你好，有什么我可以帮助你的吗？喵喵喵
```

看可以看到在自定义prompt模板后，大模型按照我们的要求在回答后加上了“喵喵喵”。

#### 动态的Prompt

有些场景下例如`RAG`系统，prompt需要植入额外的内容，此时我们就可以在prompt中插入了一个变量作为占位符，然后在推理的过程中该变量可以替换为我们希望的内容。为了实现这样一种动态的Prompt，具体让我们来看一下这个例子：

```
import lazyllm

llm2 = lazyllm.OnlineChatModule().prompt("根据给出的文段回答问题，文段：{content}")

passage = ('孙悟空，是在小说《西游记》当中唐僧的四个徒弟之一，排行第一，别名孙行者、孙猴子。'
           '自封美猴王、齐天大圣。因曾在天庭掌管御马监而又被称为弼马温，在取经完成后被如来佛祖授封为斗战胜佛')

# 模型推理：
print(llm2({'input':'孙悟空有哪些名字？', 'content':passage}))
```

上面例子中，我们希望在推理过程中，能把文段信息替换`{content}`，这里我们取了个变量名叫做`content`，您可以取任意其他变量名（除了`input`）。`passage`送入大模型后输出的实际效果为：

```
孙悟空有以下几个名字和别称：

1. 孙行者
2. 孙猴子
3. 美猴王（自封）
4. 齐天大圣（自封）
5. 弼马温（曾掌管天庭御马监）

在取得真经后，他还被如来佛祖授封为“斗战胜佛”。
```

可以看到模型根据我们的需求正确从文本段中提取出来了内容。

#### 独立的Prompt

前面介绍的`Prompt`都是在大模型上直接设置的，有时候我们需要单独先定义好一个`Prompt`，然后再给它设置给大模型来使用（这样定义好一个`Prompt`就可以设置给不同的大模型使用了）。`LazyLLM`主要有两种`Prompt`：`AlpacaPrompter`和 `ChatPrompter`(前面其实用到的都是这种`ChatPrompter`)。它们两者主要是格式不一样，具体来说：

`AlpacaPrompter` :

```
{system}\n{instruction}\n{tools}\n{user}### Response:\n
```

`ChatPrompter`：

```
{sos}{system}{instruction}{tools}{eos}\n\n{history}\n{soh}\n{user}{input}\n{eoh}{soa}\n
```

其中：

+   `instruction`: 任务指令，我们前面设置的`Prompt`内容主要就是这个部分；

+   `history`: 历史对话，由用户的输入得到，格式为 `[[a, b], [c, d]]` 或 `[{"role": "user", "content": ""}, {"role": "assistant", "content": ""}]`

+   `tools`: 可以使用的工具，在构造 `prompter` 时传入或者由用户使用时传入，当构造 `prompter` 时定义了工具之后，将禁止用户使用时再次传入。格式为 `[{"type": "function", "function": {"name": "", "description": "", "parameters": {}, "required": []}]`

+   `user`: 用户级指令，可选指令。由用户通过 `instruction`指定，如果 `instruction`是字符串，则默认是系统指令，如果是字典，且其键值只能是 `system` 和 `user` 。`system` 指定的是系统级指令， `user` 指定的是用户级指令。

由模型归属信息自动填入的参数（用户和开发者都不用关心如何填入这部分信息，`LazyLLM`会自动处理，这里做了解就好）：

+   `system`: 系统提示，一般会读取模型的归属信息并进行设置（当设置到某个模型上后会自动设置上），如不设置默认为 `You are an AI-Agent developed by LazyLLM.` ；

+   `sos`: start of system , 标志着系统提示的开始；

+   `eos`: end of system , 标志着系统提示的结束；

+   `soh`: start of human , 标志着用户输入的开始；

+   `eoh`: end of human , 标志着用户输入的结束；

+   `soa`: start of assistant , 标志着模型输出的开始；

+   `eoa`: end of assistant , 标志着模型输出的结束；

让我们先看看这两种独立的`Prompt`的拼合效果：

假设文段和用户提问是：

```
import lazyllm

passage = ('孙悟空，是在小说《西游记》当中唐僧的四个徒弟之一，排行第一，别名孙行者、孙猴子。'
           '自封美猴王、齐天大圣。因曾在天庭掌管御马监而又被称为弼马温，在取经完成后被如来佛祖授封为斗战胜佛')
query = '孙悟空有哪些名字？'
```

`AlpacaPrompter`(独立)

```
prompter1 = lazyllm.AlpacaPrompter({
    'system': '系统指令',
    'user': '用户指令。\n### 文段内容：{content}\n### 问题：{input}\n'
    })
content = prompter1.generate_prompt({'input':query,'content':passage})
print("\n独立Prompt(Alpaca):\n", repr(content))
```

输出：



独立Prompt(Alpaca):
 'You are an AI-Agent developed by LazyLLM.\nBelow is an instruction that describes a task, paired with extra messages such as input that provides further context if possible. Write a response that appropriately completes the request.\n\n### Instruction:\n系统指令\n\n用户指令。\n### 文段内容：孙悟空，是在小说《西游记》当中唐僧的四个徒弟之一，排行第一，别名孙行者、孙猴子。自封美猴王、齐天大圣。因曾在天庭掌管御马监而又被称为弼马温，在取经完成后被如来佛祖授封为斗战胜佛\n### 问题：孙悟空有哪些名字？\n### Response:\n'

### ChatPrompter（独立）

```
prompter2 = lazyllm.ChatPrompter({
    'system': '系统指令',
    'user': '用户指令。\n### 文段内容：{content}\n### 问题：{input}\n'
})
content = prompter2.generate_prompt({'input':query,'content':passage})
print("\n独立Prompt(Chat):\n", repr(content))
```

输出：

```
独立Prompt(Chat):
 'You are an AI-Agent developed by LazyLLM.系统指令\n\n\n\n用户指令。\n### 文段内容：孙悟空，是在小说《西游记》当中唐僧的四个徒弟之一，排行第一，别名孙行者、孙猴子。自封美猴王、齐天大圣。因曾在天庭掌管御马监而又被称为弼马温，在取经完成后被如来佛祖授封为斗战胜佛\n### 问题：孙悟空有哪些名字？\n\n\n'
```

现在再让我们把它塞到模型里，让模型自动添加上和它相关的信息，这里我们选择一个本地模型`InternLM2-Chat-7B`。

### AlpacaPrompter(带LLM)

```
m1 = lazyllm.TrainableModule("internlm2-chat-7b").prompt(prompter1)
res = m1._prompt.generate_prompt({'input':query,'content':passage})
print("\n带LLM的Prompt(Alpaca):\n", repr(res))
```

输出：

```
带LLM的Prompt(Alpaca):
 'You are an AI assistant whose name is InternLM (书生·浦语).\n- InternLM (书生·浦语) is a conversational language model that is developed by Shanghai AI Laboratory (上海人工智能实验室). It is designed to be helpful, honest, and harmless.\n- InternLM (书生·浦语) can understand and communicate fluently in the language chosen by the user such as English and 中文.\nBelow is an instruction that describes a task, paired with extra messages such as input that provides further context if possible. Write a response that appropriately completes the request.\n\n### Instruction:\n系统指令\n\n用户指令。\n### 文段内容：孙悟空，是在小说《西游记》当中唐僧的四个徒弟之一，排行第一，别名孙行者、孙猴子。自封美猴王、齐天大圣。因曾在天庭掌管御马监而又被称为弼马温，在取经完成后被如来佛祖授封为斗战胜佛\n### 问题：孙悟空有哪些名字？\n### Response:\n'
```

### ChatPrompter（带LLM）

```
m2 = lazyllm.TrainableModule("internlm2-chat-7b").prompt(prompter2)
res = m2._prompt.generate_prompt({'input':query,'content':passage})
print("\n带LLM的Prompt(Chat):\n", repr(res))
```

输出：

```
带LLM的Prompt(Chat):
 '<|im_start|>system\nYou are an AI assistant whose name is InternLM (书生·浦语).\n- InternLM (书生·浦语) is a conversational language model that is developed by Shanghai AI Laboratory (上海人工智能实验室). It is designed to be helpful, honest, and harmless.\n- InternLM (书生·浦语) can understand and communicate fluently in the language chosen by the user such as English and 中文.系统指令<|im_end|>\n\n\n\n<|im_start|>user\n\n用户指令。\n### 文段内容：孙悟空，是在小说《西游记》当中唐僧的四个徒弟之一，排行第一，别名孙行者、孙猴子。自封美猴王、齐天大圣。因曾在天庭掌管御马监而又被称为弼马温，在取经完成后被如来佛祖授封为斗战胜佛\n### 问题：孙悟空有哪些名字？\n\n<|im_end|>\n<|im_start|>assistant\n\n'
```

格式对比：

### 3.2 三行代码搭建聊天机器人

#### 基于InternLM的Chatbot

在LazyLLM框架当中，搭建聊天机器人是一件极其简单的事情，只需三行代码即可完成：

```
from lazyllm import OnlineChatModule, WebModule
llm = OnlineChatModule(
    source='openai',
    base_url="https://chat.intern-ai.org.cn/api/v1/",
    model="internlm3-latest",
    stream=True
).prompt("你扮演一只小猫，在每次回答问题都要加上喵喵喵")
webpage = WebModule(llm, port=23466, history=[llm], stream=True).start().wait()
```

在定义模型模块`llm`时，只需在后面调用`.prompt()`即可实现提示词配置。这里我们引入了一个新模块：`WebModule`，它能够提供一个基于`gradio`的交互界面，帮助我们快速验证应用能力。使用时：

- 指定用大模型`llm`来作为对话的机器人；
- `port`指定了聊天界面发布的端口；
- `history`中指定了要将`llm`的输出作为历史信息，这样机器人就具有了历史对话记忆能力；
- `stream`开启了输出的对话是流式的；
- `start()`用于启动整个聊天机器人的部署；
- `wait()`表示启动后就一直服务不停止，如果不加上服务部署好后会立刻停止；

程序启动后，我们便可以自由对话了。

![](img/d932e15994da6f0f8c4ab39f9e682871.png)

#### 基于InternVL的多模态Chatbot

我们可以使用相同的方法使用InternVL多模态大模型快速搭建多模态大模型，下面我们演示使用`LMDeploy`本地部署InternVL大模型，并搭建多模态聊天机器人。同样我们只需在第二章的基础上加入`WebModule`即可：

```
from lazyllm import TrainableModule, WebModule, deploy
vlm = TrainableModule('internvl2-8b').deploy_method(deploy.LMDeploy).start()
w = WebModule(vlm, port=23432, files_target=vlm).start().wait()
```

注意，由于多模态Chatbot需要接收图片等格式的文件，此处需要将`WebModule`的`files_target`参数设置为我们的模型，一个多模态聊天机器人轻松搭建完成。

![](img/c9d58d0c07d6da789c0e276af4a15027.png)

## 第四章 使用LazyLLM + 书生大模型搭建RAG应用

除了较为简单的聊天机器人，LazyLLM提供了大量的优质组件，最大程度方便开发者灵活选择，开发高性能的工业级RAG应用。

让我们快速回顾一下RAG（Retrieval-augmented Generation，RAG，检索增强生成）的基本原理。RAG的工作原理是当模型需要生成文本或回答问题时，首先会从一个庞大的文档集合中检索出相关的信息，这些检索到的信息随后会被用于指导生成过程，从而显著提高生成文本的质量和准确性。RAG 的整体结构可以用下图标识，系统接收一个用户 Query， 首先通过检索器（Retriever）从外部文档中检索出与给定 Query 相似的内容，然后将其与 Query 一同输入到 LLM ，LLM 给出最终答案：

![](img/219ba09487ff64714af4c887d772d72a.png)

### 小结

RAG的工作流程可以归纳成以下三步：

1. 检索（Retrieval）

   用户输入问题后，系统会基于该输入在知识库中检索相关内容。

2. 增强（Augmented）

   检索到的文本内容会作为额外的上下文，与用户输入一起提供给大模型。

3. 生成（Generatation）

   大模型结合检索到的信息和自身的预训练知识，生成最终的回答。

接下来我们将按照顺序，依次介绍`lazyllm`中的文档加载、检索组件和生成组件。

Here we go！🏄

### 4.1 LazyLLM框架RAG组件介绍

#### 文档加载 - Document

RAG的核心是从文档集合进行文档检索，文档集合中可以有各种各样的文档格式：可以是 DOCX，PDF，PPT 等富文本或者 Markdown 这样的纯文本，也可能是从某个 API 获取的内容（如通过搜索引擎检索得到的相关信息）等等。由于集合内的文档格式各异，针对这些不同格式的文档，我们需要特定的解析器来提取其中有用的文本、图片、表格、音频和视频等内容。

在 LazyLLM 中，这些用于提取特定内容的解析器被抽象为`Document`类，目前 LazyLLM 内置的 `Document`可以支持 DOCX，PDF，PPT，EXCEL 等常见的富文本内容提取，您也可以自定义文档解析器`Reader` 读取特定格式的文档（在后续的教程中我们会进行详细介绍）。

`Document`的主要参数如下：

- `dataset_path` (`str`) – 数据集目录的路径。此目录应包含要由文档模块管理的文档（注意：暂不支持指定单个文件）。
- `embed` (`Optional[Union[Callable, Dict[str, Callable]]]`, default: `None`) – 用于生成文档 embedding 的对象。如果需要对文本生成多个 embedding，此处需要通过字典的方式指定多个 embedding 模型，key 标识 embedding 对应的名字, value 为对应的 embedding 模型。
- `manager` (`bool`, default: `False`) – 指示是否为文档模块创建用户界面的标志。默认为 `False`。
- `launcher` (optional, default: `None`) – 负责启动服务器模块的对象或函数。如果未提供，则使用 `lazyllm.launchers` 中的默认异步启动器 (`sync=False`)。
- `store_conf` (optional, default: `None`) – 配置使用哪种存储后端和索引后端。
- `doc_fields` (optional, default: `None`) – 配置需要存储和检索的字段继对应的类型（目前只有 Milvus 后端会用到）。

对于最基础的使用，我们只需要传入数据集目录的路径即可：

```
# RAG 文档读取
from lazyllm import Document

# 传入绝对路径
doc = Document("path/to/content/docs/")
print(f"实际传入路径为：{}")
# 传入相对路径
doc = Document("/content/docs/")
```

注意，您需要在此处传入绝对路径，或者是以当前目录为基础的相对路径，其他情况下需要您通过环境变量 `LAZYLLM_DATA_PATH` 指定您的文档所在目录再传入相对路径。

#### 检索组件 - Retriever

实际应用中，不是所有文档都和用户要查询的内容相关，因此需要通过检索组件从文档中筛选出和用户查询相关的文档。LazyLLM 中执行检索功能的是 `Retriever` 组件，`Retriever`组件可以创建一个用于文档查询和检索的检索模块。此构造函数初始化一个检索模块，该模块根据指定的相似度度量配置文档检索过程。`Retriever`组件主要参数如下：

- `doc` (object) – 文档模块实例。该文档模块可以是单个实例，也可以是一个实例的列表。如果是单个实例，表示对单个`Document`进行检索，如果是实例的列表，则表示对多个`Document`进行检索。
- `group_name` (`str`) – 在哪个 node group 上进行检索。

`group_name` 有三种内置切分策略，都是用 `SentenceSplitter` 切分，区别在于块大小不同：

- `CoarseChunk`: 块大小为 1024，重合长度为 100
- `MediumChunk`: 块大小为 256，重合长度为 25
- `FineChunk`: 块大小为 128，重合长度为 12

- `similarity` (`Optional[str]`, default: `None`) – 用于设置文档检索的相似度函数。默认为 `'dummy'`。候选集包括 `["bm25", "bm25_chinese", "cosine"]`。
- `similarity_cut_off` (`Union[float, Dict[str, float]]`, default: `float('-inf')`) – 当相似度低于指定值时丢弃该文档。在多 embedding 场景下，如果需要对不同的 embedding 指定不同的值，则需要使用字典的方式指定，key 表示指定的是哪个 embedding，value 表示相应的阈值。如果所有的 embedding 使用同一个阈值，则只指定一个数值即可。
- `index` (`str`, default: `'default'`) – 用于文档检索的索引类型。目前仅支持 `'default'`。
- `topk` (`int`, default: 6) – 表示取相似度最高的多少篇文档。
- `embed_keys` (`Optional[List[str]]`, default: `None`) – 表示通过哪些 embedding 做检索，不指定表示用全部 embedding 进行检索。
- `similarity_kw` – 传递给 `similarity` 计算函数的其它参数。

下面这行代码声明检索组件需要在 `doc` 这个文档中的 `Coarse chunk` 节点组利用 `bm25_chinese` 相似度进行检索，最终返回相似度最高的 3 个节点。

```
from lazyllm import Retriever

# 传入绝对路径
doc = Document("/path/to/content/docs/")

# 使用Retriever组件，传入文档doc，节点组名称这里采用内置切分策略"CoarseChunk"，相似度计算函数bm25_Chinese
retriever = Retriever(doc, group_name=Document.CoarseChunk, similarity="bm25_chinese", topk=3)

# 调用retriever组件，传入query
retriever_result = retriever("your query")

# 打印结果，用get_content()方法打印具体的内容
print(retriever_result[0].get_content())
```

#### 生成组件 - LLM



有了检索到的内容，结合我们提问的问题，将二者共同输入给生成组件，即可得我们想要的答案。这里的生成组件就是 LLM 大模型，先前已经介绍，这里不过多赘述。

## 解析组件 - `Reader`

`Reader` 的功能就是负责把以各种形式存储的知识读取出来，然后以统一的格式给检索模块使用。

![](img/16efd2a2825d4a37adfaae33284d6e08.png)

[下面我们看下 `Reader`](https://huggingface.co/datasets/LazyAGI/CMRC2018_Knowledge_Base/tree/main) 的使用方法。首先，我们以 `cmrc2018` 知识库中的 `part_1.txt` 为例，内容为：

```
MIT OpenCourseWare是由麻省理工学院在2002年创始的一项计划。它的目标是在2007年年底以前，让所有大学生和研究生课程能够在线上自由的被任何人从任何地点取用。MIT开放式课程也可以被视为一项庞大的、网路出版的MIT教材。这个计划由William and Flora Hewlett Foundation、Andrew W. Mellon Foundation、麻省理工学院共同主持。之后，这个计划更激励了不少机构将他们的教材转变为开放教育资源。2007年11月，已经有超过1800个课程在线上。其中少数仅是书单和讨论主题，大多数会提供课后问题、考试（通常附有解答）以及演讲笔记。某些课程甚至提供互动式的示范程式、MIT教授写的教科书、串流影音格式的演讲影片。2008年8月，1800个线上课程中，只有26个完整的演讲影片。并不是所有影片都附有演讲笔记，这导致了观看演讲时的困难。无论如何，那些完整的课程都具有良好的品质，而且许多演讲都非常地令人信服。线上的影片虽然都是影音串流，但也有可供下载后离线观看的方法。像是iTunes U就有提供这些影音档的下载。
参加2008年北京夏季奥林匹克运动会的中国皮划艇队共计40人，其中运动员23人，官员与工作人员17人，领队刘刚。皮划艇静水项目组共计28人，其中运动员18人；副领队宋广礼副领队：宋广礼教练员（7人）：孙尔杰、马克、钟成海、李戴源、何军、谭志宏、徐菊生医生：阙云太，管理：姜丹男运动员（12人）：刘海涛、李臻、林淼、周鹏、沈洁、潘耀、黄志鹏、孟关良、杨文军、陈忠云、张志武、李强女运动员（6人）：钟红燕、徐琳蓓、王 凤、于腊梅、许亚萍、梁培兴皮划艇激流回旋项目组共计11人，其中运动员5人；副领队：李欣教练员：张磊、马可武、克里斯帝安、汉斯、安吉利卡男运动员（4人）：胡明海、舒俊榕、冯黎明、丁富学女运动员（1人）：李晶晶男子单人划艇／冯黎明／预赛第1轮／第9位／成绩91.44（8月11日）；男子单人划艇／冯黎明／预赛第2轮／第12位／成绩90.55（8月11日）；男子单人划艇／冯黎明／预赛第1、2轮／第11位／总成绩181.99／落后小组第1位11.84（8月11日）；男子单人划艇／冯黎明／半决赛／第11位／总成绩94.64（92.64+罚时2）／落后小组第1位5.72（未入前8名被淘汰）（8月12日）男子单人皮艇／丁富学／预赛第1轮／成绩87.47／落后小组第1位5.68（8月11日）；男子单人皮艇／丁富学／预赛第1轮／成绩90.92／落后小组第1位5.68（8月11日）；男子单人皮艇／丁富学／预赛第1、2轮／第21位／总成绩228.39／落后小组第1位61.90（被淘汰）（8月11日）。
乍都节公园（）位于泰国的首都曼谷的乍都节县，是拍凤裕庭路、威拍哇丽兰室路、甘烹碧路之间的一处公众游园地。1975年由泰国国家铁路局捐献土地建筑成公园。1980年12月4日，泰国国王拉玛九世62岁生日那天，公园开放。它的面积约为0.304平方公里。后期，再在邻近地段，增辟「诗丽吉皇后公园」（Queen Sirikit Park）及「火车公园」（Rot Fai Park），都被「甘烹碧三街」所分隔。整个细长的公园被一个人工湖泊占据，许多桥梁跨越湖泊连接两岸。公园里亦有一座小型的火车博物馆，展示泰国的机车及车厢。每逢星期日，都会成为杂货市集，吸引许多观光客来购物。
摩尔甘比足球俱乐部（）是位于英格兰西北兰开夏郡渡假小镇莫克姆（Morecambe）的足球俱乐部。于2007年透过附加赛在球队历史内首次升级到英格兰足球联赛，现时在英格兰乙级联赛中作赛。&quot;参见2015年至2016年英格兰足球乙级联赛&quot;地球体育场（Globe Arena）位于英格兰兰开夏郡莫克姆（Morecambe）的足球场，取代自1921年已开始使用的基士堤公园球场（Christie Park），成为摩甘比的主场球场，以建造球场的「地球建筑公司」（Globe Construction）命名。2009年5月9日接收地盘进行平整工地，于同年9月初动工兴建上盖，由于工程延误，揭幕日期推迟一个球季至2010/11年球季才正式启用，于2010年8月10日英格兰联赛杯第一圈对-{zh-hans:考文垂; zh-hk:高云地利;}-进行首场赛事，摩甘比旗开得胜以2-0淘汰较高级别的对手，新加盟的安德鲁·弗莱明（Andrew &quot;Andy&quot; Fleming）包办两个入球。地球体育场可以容纳6,476名观众，其中在草坪长边的主看台有2,173个坐席，对面是可容606名观众没有顶盖的阶梯看台，龙门后方是主场及作客球迷看台，西看台可容2,234名主场球迷，而东看台则可让1,389名作客球迷进场观战。
达曼（）位于沙特阿拉伯的东部省，是沙特石油工业的重要中心。达曼是东部省最大的城市，达曼港也是波斯湾最大的海港之一。它的货物进出口量仅次于沙特位于红海海岸的吉达港。达曼拥有新建的法赫德国王国际机场，机场位于市西20km，有一条6车道公路连接。达曼的卫星城有现代经济中心 Khobar、世界最大的沙特Aramco石油公司所在地札哈兰以及渔业农业中心Qatif。几座城市（达曼、寇巴Khobar和札哈兰）共有200万人口，其中750,000居住在主城达曼。本地的体育场所是Mohamed bin法赫德王子体育场。达曼是沙特大陆桥计划的东段终点，西端终点在吉达港。ISG-达曼
潘仕成（），字德畬、德舆，祖籍福建，家族世居广州，先祖以盐商起家，至潘仕成时为广州十三行家族巨贾第三代，晚清巨富、慈善家。清嘉庆九年(1804年)，祖籍福建且家族世居广州的潘仕成出生。潘仕成的先祖以盐商起家，至潘仕成时为第三代经营，成为广州十三行巨贾，与米利坚商人颇多熟悉，亦素为该国夷人所敬重。曾捐官20万两白银使自己成为二品官，亦曾镇压广东连山瑶族有功。鸦片战争爆发时，又捐助军需银八万两，得赏戴花翎，封为道员头衔。清道光年间，官至兵部侍郎。道光二十二年（1842年）十月，潘自行出资延请美国军官壬雷斯在广州研发水雷。道光二十三年（1843年）七月，由李光铨等将制成的水雷送北京，在天津试演。又从国外引进牛痘，被誉为“轻财好义，地方善举资助弗吝”。晚年横遭变故，经济拮据。清同治十年(1871年)，将《佩文韵府》的木刻版抵押给山西票号。清同治十二年(1873年)，卒。
马飚（），壮族，广西田阳人；中央民族学院政治系毕业。1985年6月加入中国共产党，是中共第十七届中央候补委员，十八、十九届中央委员。曾任广西壮族自治区人民政府主席。现任第十二届全国政协副主席。文化大革命期间，马飚中学毕业后，在广西柳州钢铁厂当工人。高考制度恢复后，于1978年考入中央民族学院政治系政治经济学专业学习。毕业后，从事经济研究工作，先后在广西壮族自治区计划委员会计划经济研究所和中国社会科学院民族研究所任职；期间，还曾在广西罗城县挂职担任县委副书记。1994年，马飚出任中共北海市委书记助理，步入政界。此后，历任广西壮族自治区经济体制改革委员会副主任、自治区青年联合会副主席，百色地区行政公署专员，中共百色地委书记。2003年地市合并后，出任首任中共百色市委书记（省辖市）；2004年，升任自治区政府副主席；2005年3月起，任自治区党委常委、兼中共南宁市委书记、市人大常委会主任；2007年12月，任自治区党委副书记，政府副主席、代主席；2008年1月，正式当选广西壮族自治区人民政府主席。2013年3月，作为壮族代表，接替李兆焯，当选为第十二届全国政协副主席。
<br>西班牙语言（lenguas de España）指的在西班牙使用或曾经使用的语言。罗曼语族的语言在西班牙被广泛使用，其中只有西班牙语（español）（或称为卡斯蒂利亚语（castellano））拥有全国官方语言的地位，有些语言只在某些地区是官方语言，同时也有一些非官方的语言或方言。在西班牙，有最多使用者及最具优势的语言是西班牙语，基本上每个西班牙人都以西班牙语为母语或第二语言。在2005年，89%的西班牙人的母语是西班牙语，9%是加泰罗尼亚语，而加利西亚语和巴斯克语则分别有5%和1%的母语使用者。由于有3%人口可以说两种母语，所以以上的比率加起来超过100%。一些语言在某地区有较多的使用者：上述语言都是地方性的官方语言，除了Aranese之外，其余3种语言都在各自的地区广泛使用，并有报刊、书籍等媒体使用该种语言，而加泰罗尼亚语及加利西亚语更被地区政府用作行政语言。西班牙境内还有一些语言只有少数人使用：西班牙语本身也有不同的方言，例如Andalusian及Canarian方言，每种方言之下又有各自的变体。在加利西亚，加利西亚语和葡萄牙语的关系曾引起过争议，有语言学家认为虽然这两种语言的音系和词汇不一样，但它们是同一种语言的方言（见）。相反意见认为，正是由于这两种语言的音系和词汇不一样，它们是两种不同的语言。历史上曾在现今西班牙境内使用的语言包括：不在西班牙使用但与西班牙有联系的语言包括：
南田县是浙江省一个已经撤消的县，存县期间，范围包括今宁波市象山县的南田岛、高塘岛、花岙岛、檀头山、坦塘岛、渔山列岛等。道光三年（1824年），宁波府海防同知移驻象山县石浦（亦有称石浦厅）。光绪初年，南田岛置开垦委员，宣统元年六月癸卯（1909年8月11日）于大佛头山麓改置为南田厅，设抚民通判，同年驻地移樊岙。民国元年（1912年）2月，南田厅改置为南田县，县政府驻樊岙。4月，原石浦厅（废厅后并回象山县），即象山县东溪岭以南地划入南田县，县府迁治石浦。民国二年（1913年）石浦等地复归象山县，南田县还驻樊岙。民国29年（1940年）7月1日，南田县撤消，合宁海县、临海县部分地区合置三门县，县府移至今六敖健康塘。1952年原南田县所属区域从三门县划出，重新划归象山县，仅满山（岛屿）仍划归三门县，并成为三门县、象山县和宁海县的海上界岛。
有田中井手之战是武田元繁与毛利元就的战争，也是毛利元就的初阵，最后武田元繁战死，被誉为西国的桶狭间之战。毛利元就1000人，武田元繁5000人。当时正值毛利家当主毛利兴元方才去世不久，年仅两岁的其子毛利幸松丸被拥立为下任当主，由当时尚无作战经验的元就与其弟相合元纲共同担任监护人之职，坐镇于佐东银山城的武田元繁见有机可趁，随即命令部下熊谷元直担任先锋，率军前往毛利领地烧杀掳掠，自己则会同香川行景、己斐宗瑞等豪族，进攻位于吉田郡山城邻近的有田城。隶属于吉川家的有田城主小田信忠，闻知武田家大军来袭，随即派出使者前往毛利家求援。毛利元就以有田城若失陷，则吉田郡山城亦将不保的考量下，决定率领部队支援。首先击杀了武田家的先锋熊谷元直，武田元繁闻元直死讯而大怒，亲率部队来迎战元就。时值夜色，元就采奇袭战术，使武田军势大乱，武田元繁在混乱中遇袭身亡，决定了毛利家的胜利。有田中井手之战 - 吉田郡山城之战 - 第一次月山富田城之战 - 折敷畑之战 - 严岛之战 - 防长经略 - 忍原崩 - 第二次月山富田城之战 - 多多良滨之战 - 布部山之战 - 上月城之战 - 备中兵乱 - 鸟取城之战 - 备中高松城之战% 
```

我们先导入需要用到的包，因为 `Reader` 模块是在 `Document` 内部使用的，所以这里我们先导入 `Document` 模块。

```
from lazyllm.tools.rag import Document
```

然后我们基于上面的文档创建一个 `Document` 实例。

```
doc = Document(dataset_path="your_doc_path")
```

然后我们直接通过 `Document` 实例来调用 `Reader` 的模块，并传入上面的文档路径。

```
data = doc._impl._reader.load_data(input_files=["your_doc_path/part_1.txt"])
```

由于 `Reader` 模块是通过 `load_data` 来加载数据的，所以上面这里需要调用 `load_data` 接口。

> 这里需要注意一下，`input_files` 中的文件列表必须要传入文件的相对路径或者绝对路径。因为正常情况下这个 `load_data` 接口是 `Document` 类内部调用的，用户是看不到的，所以代码中会利用实例化 `Document` 时传入的 `dataset_path` 以及环境变量中配置的 `LAZYLLM_DATA_PATH` 环境变量拼接出知识库的路径，然后遍历知识库中的所有文档的。

下面我们看看 `Reader` 模块加载完这个文档之后，输出的是什么内容？通过下面的命令，我们打印一下 `Reader` 模块的输出：

```
print(f"data: {data}")
```

输出的结果是：

```
data: [<Node id=23b37172-1c5d-45d4-a9e1-d70fee799116>]
```

可以看到 `Reader` 模块输出的内容是一个 `list` 类型，然后里面的元素是 `Node` 类型，一般 `Node` 类型里面会存放相应的文本信息、embedding信息、元数据、以及 `Node` 之间的关系等等。这里只要知道 `Node` 类型就是把 `Reader` 解析出来的内容进行封装，方便后面检索模块统一处理即可。进一步，我们输出 `Node` 里面的文本信息：

```
print(f"text: {data[0].text}")
```

输出的结果是：



```
data: MIT OpenCourseWare是由麻省理工学院在2002年创始的一项计划。它的目标是在2007年年底以前，让所有大学生和研究生课程能够在线上自由的被任何人从任何地点取用。MIT开放式课程也可以被视为一项庞大的、网路出版的MIT教材。这个计划由William and Flora Hewlett Foundation、Andrew W. Mellon Foundation、麻省理工学院共同主持。之后，这个计划更激励了不少机构将他们的教材转变为开放教育资源。2007年11月，已经有超过1800个课程在线上。其中少数仅是书单和讨论主题，大多数会提供课后问题、考试（通常附有解答）以及演讲笔记。某些课程甚至提供互动式的示范程式、MIT教授写的教科书、串流影音格式的演讲影片。2008年8月，1800个线上课程中，只有26个完整的演讲影片。并不是所有影片都附有演讲笔记，这导致了观看演讲时的困难。无论如何，那些完整的课程都具有良好的品质，而且许多演讲都非常地令人信服。线上的影片虽然都是影音串流，但也有可供下载后离线观看的方法。像是iTunes U就有提供这些影音档的下载。
参加2008年北京夏季奥林匹克运动会的中国皮划艇队共计40人，其中运动员23人，官员与工作人员17人，领队刘刚。皮划艇静水项目组共计28人，其中运动员18人；副领队宋广礼副领队：宋广礼教练员（7人）：孙尔杰、马克、钟成海、李戴源、何军、谭志宏、徐菊生医生：阙云太，管理：姜丹男运动员（12人）：刘海涛、李臻、林淼、周鹏、沈洁、潘耀、黄志鹏、孟关良、杨文军、陈忠云、张志武、李强女运动员（6人）：钟红燕、徐琳蓓、王 凤、于腊梅、许亚萍、梁培兴皮划艇激流回旋项目组共计11人，其中运动员5人；副领队：李欣教练员：张磊、马可武、克里斯帝安、汉斯、安吉利卡男运动员（4人）：胡明海、舒俊榕、冯黎明、丁富学女运动员（1人）：李晶晶男子单人划艇／冯黎明／预赛第1轮／第9位／成绩91.44（8月11日）；男子单人划艇／冯黎明／预赛第2轮／第12位／成绩90.55（8月11日）；男子单人划艇／冯黎明／预赛第1、2轮／第11位／总成绩181.99／落后小组第1位11.84（8月11日）；男子单人划艇／冯黎明／半决赛／第11位／总成绩94.64（92.64+罚时2）／落后小组第1位5.72（未入前8名被淘汰）（8月12日）男子单人皮艇／丁富学／预赛第1轮／成绩87.47／落后小组第1位5.68（8月11日）；男子单人皮艇／丁富学／预赛第1轮／成绩90.92／落后小组第1位5.68（8月11日）；男子单人皮艇／丁富学／预赛第1、2轮／第21位／总成绩228.39／落后小组第1位61.90（被淘汰）（8月11日）。
乍都节公园（）位于泰国的首都曼谷的乍都节县，是拍凤裕庭路、威拍哇丽兰室路、甘烹碧路之间的一处公众游园地。1975年由泰国国家铁路局捐献土地建筑成公园。1980年12月4日，泰国国王拉玛九世62岁生日那天，公园开放。它的面积约为0.304平方公里。后期，再在邻近地段，增辟「诗丽吉皇后公园」（Queen Sirikit Park）及「火车公园」（Rot Fai Park），都被「甘烹碧三街」所分隔。整个细长的公园被一个人工湖泊占据，许多桥梁跨越湖泊连接两岸。公园里亦有一座小型的火车博物馆，展示泰国的机车及车厢。每逢星期日，都会成为杂货市集，吸引许多观光客来购物。
摩尔甘比足球俱乐部（）是位于英格兰西北兰开夏郡渡假小镇莫克姆（Morecambe）的足球俱乐部。于2007年透过附加赛在球队历史内首次升级到英格兰足球联赛，现时在英格兰乙级联赛中作赛。"参见2015年至2016年英格兰足球乙级联赛"地球体育场（Globe Arena）位于英格兰兰开夏郡莫克姆（Morecambe）的足球场，取代自1921年已开始使用的基士堤公园球场（Christie Park），成为摩甘比的主场球场，以建造球场的「地球建筑公司」（Globe Construction）命名。2009年5月9日接收地盘进行平整工地，于同年9月初动工兴建上盖，由于工程延误，揭幕日期推迟一个球季至2010/11年球季才正式启用，于2010年8月10日英格兰联赛杯第一圈对-{zh-hans:考文垂; zh-hk:高云地利;}-进行首场赛事，摩甘比旗开得胜以2-0淘汰较高级别的对手，新加盟的安德鲁·弗莱明（Andrew "Andy" Fleming）包办两个入球。地球体育场可以容纳6,476名观众，其中在草坪长边的主看台有2,173个坐席，对面是可容606名观众没有顶盖的阶梯看台，龙门后方是主场及作客球迷看台，西看台可容2,234名主场球迷，而东看台则可让1,389名作客球迷进场观战。
达曼（）位于沙特阿拉伯的东部省，是沙特石油工业的重要中心。达曼是东部省最大的城市，达曼港也是波斯湾最大的海港之一。它的货物进出口量仅次于沙特位于红海海岸的吉达港。达曼拥有新建的法赫德国王国际机场，机场位于市西20km，有一条6车道公路连接。达曼的卫星城有现代经济中心 Khobar、世界最大的沙特Aramco石油公司所在地札哈兰以及渔业农业中心Qatif。几座城市（达曼、寇巴Khobar和札哈兰）共有200万人口，其中750,000居住在主城达曼。本地的体育场所是Mohamed bin法赫德王子体育场。达曼是沙特大陆桥计划的东段终点，西端终点在吉达港。ISG-达曼
潘仕成（），字德畬、德舆，祖籍福建，家族世居广州，先祖以盐商起家，至潘仕成时为广州十三行家族巨贾第三代，晚清巨富、慈善家。清嘉庆九年(1804年)，祖籍福建且家族世居广州的潘仕成出生。潘仕成的先祖以盐商起家，至潘仕成时为第三代经营，成为广州十三行巨贾，与米利坚商人颇多熟悉，亦素为该国夷人所敬重。曾捐官20万两白银使自己成为二品官，亦曾镇压广东连山瑶族有功。鸦片战争爆发时，又捐助军需银八万两，得赏戴花翎，封为道员头衔。清道光年间，官至兵部侍郎。道光二十二年（1842年）十月，潘自行出资延请美国军官壬雷斯在广州研发水雷。道光二十三年（1843年）七月，由李光铨等将制成的水雷送北京，在天津试演。又从国外引进牛痘，被誉为“轻财好义，地方善举资助弗吝”。晚年横遭变故，经济拮据。清同治十年(1871年)，将《佩文韵府》的木刻版抵押给山西票号。清同治十二年(1873年)，卒。
马飚（），壮族，广西田阳人；中央民族学院政治系毕业。1985年6月加入中国共产党，是中共第十七届中央候补委员，十八、十九届中央委员。曾任广西壮族自治区人民政府主席。现任第十二届全国政协副主席。文化大革命期间，马飚中学毕业后，在广西柳州钢铁厂当工人。高考制度恢复后，于1978年考入中央民族学院政治系政治经济学专业学习。毕业后，从事经济研究工作，先后在广西壮族自治区计划委员会计划经济研究所和中国社会科学院民族研究所任职；期间，还曾在广西罗城县挂职担任县委副书记。1994年，马飚出任中共北海市委书记助理，步入政界。此后，历任广西壮族自治区经济体制改革委员会副主任、自治区青年联合会副主席，百色地区行政公署专员，中共百色地委书记。2003年地市合并后，出任首任中共百色市委书记（省辖市）；2004年，升任自治区政府副主席；2005年3月起，任自治区党委常委、兼中共南宁市委书记、市人大常委会主任；2007年12月，任自治区党委副书记，政府副主席、代主席；2008年1月，正式当选广西壮族自治区人民政府主席。2013年3月，作为壮族代表，接替李兆焯，当选为第十二届全国政协副主席。
<br>西班牙语言（lenguas de España）指的在西班牙使用或曾经使用的语言。罗曼语族的语言在西班牙被广泛使用，其中只有西班牙语（español）（或称为卡斯蒂利亚语（castellano））拥有全国官方语言的地位，有些语言只在某些地区是官方语言，同时也有一些非官方的语言或方言。在西班牙，有最多使用者及最具优势的语言是西班牙语，基本上每个西班牙人都以西班牙语为母语或第二语言。在2005年，89%的西班牙人的母语是西班牙语，9%是加泰罗尼亚语，而加利西亚语和巴斯克语则分别有5%和1%的母语使用者。由于有3%人口可以说两种母语，所以以上的比率加起来超过100%。一些语言在某地区有较多的使用者：上述语言都是地方性的官方语言，除了Aranese之外，其余3种语言都在各自的地区广泛使用，并有报刊、书籍等媒体使用该种语言，而加泰罗尼亚语及加利西亚语更被地区政府用作行政语言。西班牙境内还有一些语言只有少数人使用：西班牙语本身也有不同的方言，例如Andalusian及Canarian方言，每种方言之下又有各自的变体。在加利西亚，加利西亚语和葡萄牙语的关系曾引起过争议，有语言学家认为虽然这两种语言的音系和词汇不一样，但它们是同一种语言的方言（见）。相反意见认为，正是由于这两种语言的音系和词汇不一样，它们是两种不同的语言。历史上曾在现今西班牙境内使用的语言包括：不在西班牙使用但与西班牙有联系的语言包括：
南田县是浙江省一个已经撤消的县，存县期间，范围包括今宁波市象山县的南田岛、高塘岛、花岙岛、檀头山、坦塘岛、渔山列岛等。道光三年（1824年），宁波府海防同知移驻象山县石浦（亦有称石浦厅）。光绪初年，南田岛置开垦委员，宣统元年六月癸卯（1909年8月11日）于大佛头山麓改置为南田厅，设抚民通判，同年驻地移樊岙。民国元年（1912年）2月，南田厅改置为南田县，县政府驻樊岙。4月，原石浦厅（废厅后并回象山县），即象山县东溪岭以南地划入南田县，县府迁治石浦。民国二年（1913年）石浦等地复归象山县，南田县还驻樊岙。民国29年（1940年）7月1日，南田县撤消，合宁海县、临海县部分地区合置三门县，县府移至今六敖健康塘。1952年原南田县所属区域从三门县划出，重新划归象山县，仅满山（岛屿）仍划归三门县，并成为三门县、象山县和宁海县的海上界岛。
有田中井手之战是武田元繁与毛利元就的战争，也是毛利元就的初阵，最后武田元繁战死，被誉为西国的桶狭间之战。毛利元就1000人，武田元繁5000人。当时正值毛利家当主毛利兴元方才去世不久，年仅两岁的其子毛利幸松丸被拥立为下任当主，由当时尚无作战经验的元就与其弟相合元纲共同担任监护人之职，坐镇于佐东银山城的武田元繁见有机可趁，随即命令部下熊谷元直担任先锋，率军前往毛利领地烧杀掳掠，自己则会同香川行景、己斐宗瑞等豪族，进攻位于吉田郡山城邻近的有田城。隶属于吉川家的有田城主小田信忠，闻知武田家大军来袭，随即派出使者前往毛利家求援。毛利元就以有田城若失陷，则吉田郡山城亦将不保的考量下，决定率领部队支援。首先击杀了武田家的先锋熊谷元直，武田元繁闻元直死讯而大怒，亲率部队来迎战元就。时值夜色，元就采奇袭战术，使武田军势大乱，武田元繁在混乱中遇袭身亡，决定了毛利家的胜利。有田中井手之战 - 吉田郡山城之战 - 第一次月山富田城之战 - 折敷畑之战 - 严岛之战 - 防长经略 - 忍原崩 - 第二次月山富田城之战 - 多多良滨之战 - 布部山之战 - 上月城之战 - 备中兵乱 - 鸟取城之战 - 备中高松城之战
```

从结果可以看出里面就是上面的文本中的内容。所以到这里我们就能明白 `Reader` 模块的功能就是把文档中的内容解析出来，并封装成统一的格式供后面的检索模块使用。

## 向量化 - Embedding

`Embedding` 是一种将文档映射为一个保留原文语义的高维向量的技术，基于这种高维向量可以高效进行语义检索。LazyLLM 支持调用线上和线下 Embedding 模型，其中线上模型通过 `OnlineEmbeddingModule` 调用，线下模型通过 `TrainableModule` 调用。`OnlineEmbeddingModule` 用来管理创建目前市面上的在线Embedding服务模块，可配置参数有：

+ `source` (`str`) – 指定要创建的模块类型，可选为 `openai` / `sensenova` / `glm` / `qwen` / `doubao`

+ `embed_url` (`str`) – 指定要访问的平台的基础链接，默认是官方链接

+ `embed_model_name` (`str`) – 指定要访问的模型，默认值为 `text-embedding-ada-002`(`openai`) / `nova-embedding-stable`(`sensenova`) / `embedding-2`(`glm`) / `text-embedding-v1`(`qwen`) / `doubao-embedding-text-240715`(`doubao`)

使用 `TrainableModule`启动本地服务可配置参数有：

+ `base_model` (`str`, default: '' ) – 基模型的名称或者路径，如果本地没有模型，则会尝试下载。这里需要您注意的是您需要传入模型的绝对路径，或者指定模型路径为环境变量 `LAZYLLM_MODEL_PATH` 后在此处传入模型名。

目前而言常用的 `Embedding` 模型通常是 `jina` 系列或 `bge` 系列的模型（例如 `bge-large-zh-v1.5`，`bge-m3` 等），具体模型名称可以在 `huggingface` 检索得到。在线 embedding 模型提供的 embedding 模型需要在指定厂商的模型列表进行查找，如果没有特定需求，您可以直接使用LazyLLM `OnlineEmbeddingModule` 提供的默认模型即可。

下面是使用LazyLLM调用在线`Embedding`模型和启动本地服务并调用的示例代码：

```
from lazyllm import OnlineEmbeddingModule, TrainableModule, deploy

# 将您的在线模型 API key 抛出为环境变量，以 sensenova 为例
#     export LAZYLLM_SENSENOVA_API_KEY=     
#     export LAZYLLM_SENSENOVA_SECRET_KEY=
online_embed = OnlineEmbeddingModule("sensenova")

# 将您的本地模型地址抛出为环境变量：
#     export LAZYLLM_MODEL_PATH=/path/to/your/models
offline_embed = TrainableModule('bge-large-zh-v1.5').start()
# 或者使用绝对路径：
offline_embed = TrainableModule('/path/to/your/bge-large-zh-v1.5').start()

# 启动稀疏嵌入模型，目前仅支持 bge-m3
offline_sparse_embed = TrainableModule('bge-m3').deploy_method(
    (deploy.AutoDeploy,
     {'embed_type': 'sparse'})
).start()

print("online embed: ", online_embed("hello world"))
print("offline embed: ", offline_embed("hello world"))
print("offline sparse embed: ",  offline_sparse_embed("hello world"))
```

上述代码输出应当为：

> online embed: [0.0010528564, 0.0063285828, 0.0049476624, -0.012008667,..., -0.051696777]

> offline embed: [-0.02584601193666458, -0.0014020242961123586,..., 0.016336631029844284]

> offline sparse embed: {"33600": 0.22412918508052826, "31": 0.20403659343719482, "8999": 0.2736259400844574}

注：不同模型生成的 `Embedding` 有所差异是正常的。

如果您部署了一个在线 `Embedding` 服务，当接口格式与`OnlineEmbeddingModule` 的 `source` 参数接收的五个厂商的在线服务一致时，可以直接将 `embed_url` 改为您自己的服务地址使用，如果不一致则可以通过继承 `OnlineEmbeddingModuleBase` 访问自定义 `Embedding` 服务，下面是一段示例代码。



# 自定义在线嵌入模型

```python
from lazyllm.module import OnlineEmbeddingModuleBase

# 配置新平台支持的api_key到全局变量
# 通过lazyllm.config.add(变量名，类型，默认值，环境变量名)进行添加
class CustomOnlineEmbeddingModule(OnlineEmbeddingModuleBase):
    def __init__(self, embed_url: str = '<new platform embedding url>',
                 embed_model_name: str = '<new platform embedding model name>'):
        super().__init__("<new_platform_name>", embed_url, lazyllm.config['new_platform_api_key'], embed_model_name)
        
    def _encapsulated_data(self, text:str, **kwargs):
        # 将数据包装为您的服务需要的数据格式
        json_data = {
            "input": [text],
            "model": self._embed_model_name
        }
        if len(kwargs) > 0:
            json_data.update(kwargs)
        return json_data

    def _parse_response(self, response: dict[str, any]):
        # 解析您的服务返回的数据，返回 embedding 部分即可
        return response['embeddings'][0]['embedding']
```

使用 LazyLLM 为文档进行嵌入时无需手动遍历，只需在实例化 `Document` 对象时传入上述定义的嵌入模型即可，方法如下所示：

```python
import lazyllm

# 定义嵌入模型
online_embed = lazyllm.OnlineEmbeddingModule()

# 加载文档
documents = lazyllm.Document('/path/to/your/document', embed=online_embed)
```

# 存储组件 - Store

LazyLLM提供了基于内存和向量数据库的`Store`存储组件，以供开发者灵活选择。处于技术探索阶段的开发者，可以直接使用系统默认的`MapStore`，将文档处理后的切片与向量一并保存在内存中，以实现从文档入库至向量检索的最快读写速度；对于处于成熟阶段的RAG系统，LazyLLM提供了多种向量数据库支持，确保数据持久稳定存储，并显著缩短RAG系统的二次启动时间，提高系统的稳定性和可扩展性。

| 组件名 | 存储介质 | 功能描述 | 适用场景 |
| --- | --- | --- | --- |
| `MapStore` | 内存 | 基于内存的存储，提供节点存储的基本能力，能以最快的速度实现数据读写 | 技术探索阶段，将少量文档处理数据保存在内存中，以技术选型与算法迭代的快速验证 |
| `ChromadbStore` | 硬盘 | 使用 `Chroma` 向量数据库，实现数据的持久化存储。 | 轻量级实验，快速POC，缩短从构思到可运行原型的研发周期 |
| `MilvusStore` | 硬盘 | 使用 `Milvus` 向量数据库，实现数据的本地存储/分布式远程存储。 | 企业级部署，适用于数据体量大，对并发、检索性能都有较高要求的场景 |

在实际的使用过程中，用户只需要在`Document`定义阶段配置`store_conf`参数，进行简单的存储配置，即可利用这两种数据库将文档处理后的数据存储于本地，并在下次系统启动时直接从本地加载这些数据，避免文档的重复入库，实现知识库持久化存储。使用`store_conf`配置`Document`存储的实现方式如下：

```python
document = lazyllm.Document(dataset_path='/path/to/your/document',
           embed=lazyllm.OnlineEmbeddingModule(),
           ...,
           store_conf=store_conf)
```

参数`store_conf`接收字典形式的存储配置，配置时需要传入`type`和`kwargs`参数，具体字段解释如下：

+   `type`：使用存储类型。如之前介绍，LazyLLM目前支持的存储类型有`map`（基于内存）、`chroma`（基于`ChromaDB`向量数据库）以及`milvus`（基于`Milvus`向量数据库）：
+   `map`：仅使用内存 `key/value` 存储暂存所有文档处理数据，系统重启后数据消失；
+   `chroma`：使用 [`ChromaDB`](https://www.trychroma.com/) 存储数据，`ChromaDB` 是一个更轻量的向量数据库，相比于 `Milvus` 能处理的数据量级有限，适合调试，更多 `ChromaDB` 相关信息可以参考 [`ChromaDB` 官方文档](https://www.trychroma.com/)；
+   `milvus`：使用 [`Milvus`](https://www.milvus-io.com/overview) 存储数据，支持海量数据的检索和存储，支持分布式部署，更适合生产级应用，更多 `Milvus` 相关信息可以参考 [`Milvus` 官方文档](https://www.milvus-io.com/overview)；
+   `kwargs`：具体存储所需的额外配置参数，同样以字典的形式输入。
+   当`type`为`chroma`时，所需配置参数为：
+   `dir`（必填）：存储数据的目录；
+   当`type`为`milvus`时，所需配置参数为：
+   `uri`（必填）：存储数据地址，可以是一个文件路径或者远程连接的url；
+   `index_kwargs`（可选）：`Milvus`数据库的索引配置，可以是一个 `dict` 或者 `list[dict]`，索引配置主要包含两个字段：索引类型`index_type`和度量类型`metric_type`（即`Index`和`Similarity`，和`LazyLLM`的定义很像）。参数如果是一个 `dict` 表示所有向量模型使用相同配置；如果需要指定不同的向量模型以不同索引配置，则可以以列表的方式传入，由 `embed_key` 指定所使用配置即可。当前只支持 `floating point embedding` 和 `sparse embedding` 两种 `embedding` 类型，分别支持的参数如下，感兴趣的开发者可以前往官网查看：
+   `floating point embedding`
+   `sparse embedding`

例如我们使用`Milvus`向量数据库作为存储后端，需要传入如下配置：

```python
milvus_store_conf = {
  'type': 'milvus',  # 指定存储后端类型
  'kwargs': {
    'uri': 'test.db',  # 存储后端地址，本例子使用的是本地文件 test.db，文件不存则创建新文件
    'index_kwargs': {  # 存储后端的索引配置
      'index_type': 'FLAT',  # 索引类型
      'metric_type': 'COSINE',  # 相似度计算方式
    }
  },
}

document = lazyllm.Document(
    dataset_path='/path/to/your/document',
    embed=embedding_model,
    store_conf=milvus_store_conf 
)
```

> 请注意，目前 LazyLLM 支持 Milvus 2.4.x，请注意您的`pymilvus` >= 2.4.11 , `milvus-lite` == 2.4.10。使用`Milvus`时，检索会根据输入配置中的索引配置进行对应算法检索，`Retriever`中无需指定`similarity`参数。

# 重排组件 - Reranker

**重排序模型**通常应用于两阶段检索算法中的第二个阶段，对应 LazyLLM 中的 `Reranker` 组件。召回重排策略的基本思想是：在初步检索后，通过一个额外的模型对检索结果进行再次排序以提高最终的效果。下图对比了直接使用检索器召回 top 3 文档和先通过检索器召回 top 5 文档再对其进行重排序返回 top 3 文档的流程。可见检索流程并没有对文档进行排序，只是将满足需求的 top 5 个文档返回，而通过重排序流程，可以对其进行更精细的排序，得到排序后的文档，使得更重要的文档在返回列表中更靠前。

![](img/297b347773f7485ee53f465fb949052e.png)

LazyLLM 分别提供了在线和离线两种重排序模型的调用途径，其中在线模型通过`OnlineEmbeddingModule(type="rerank")`进行调用，离线重排模型则仍然通过 `TrainableModule` 进行调用。使用 `Reranker` 时的可调整参数有：

+   `name` (`str`, default: `'ModuleReranker'`) ： 实现重排序时必须为 `ModuleReranker`
+   `model`（`Union[Callable, str]`） ： 实现重排序的具体模型名称或可调用对象
+   `Callable` 情形：
+   `OnlineEmbeddingModule` (`type="rerank"`)：目前支持`qwen`和`glm`的在线重排序模型，使用前需要指定 `api key`
+   `TrainableModule`(`model="str"`)：需要传入本地模型名称，常用的开源重排序模型为`bge-reranker`系列
+   `str`情形：模型名称，与上 `Callable` 情形 `TrainableModule` 对应的 `model` 参数要求相同
+   `topk`（`int`）： 最终需要返回的 k 个节点数
+   `output_format`（`str`, default: `None`）：输出格式，默认为`None`，可选值有 `'content'` 和 `'dict'`，其中 `content` 对应输出格式为字符串，`dict` 对应字典
+   `join`（`boolean`, default: `False`）：是否联合输出的 k 个节点，当输出格式为 `content` 时，如果设置该值为 `True`，则输出一个长字符串，如果设置为 `False` 则输出一个字符串列表，其中每个字符串对应每个节点的文本内容。

目前常用的开源重排序模型是 `bge-reranker`，可以通过 `TrainableModule` 启动相关服务并调用。在线模型服务商有`qwen`和`glm`提供了重排序模型服务，通过`OnlineEmbeddingModule`（`type="rerank"`）进行调用。

```python
from lazyllm import OnlineEmbeddingModule, TrainableModule
# 如果您要使用在线重排模型
# 目前 LazyLLM 支持 qwen和glm 在线重排模型，使用前请指定相应的 API key。
online_rerank = OnlineEmbeddingModule(type="rerank")
reranker = Reranker('ModuleReranker', model=online_rerank, topk=3)

# 启动本地重排序模型
offline_rerank = TrainableModule('bge-reranker-large').start()
reranker = Reranker('ModuleReranker', model=offline_rerank, topk=3)
# 或者直接传入模型名称使用本地重排序模型
reranker = Reranker('ModuleReranker', model='bge-reranker-large', topk=3)
```

对于LazyLLM RAG相关组件的更多高级用法，可以参考官方API文档：https://docs.lazyllm.ai/zh-cn/latest/

# 4.2 LazyLLM的数据流组件 - Flow

LazyLLM是一款以数据流（Data Flow）为核心的大模型应用开发框架，其中定义了大量的数据流组件，配合 LazyLLM 中提供的工具和组件，可以让您像搭积木一样搭建复杂的大模型应用。

数据流的核心设计思路是**无需您手动对数据流动进行指定**，而是通过数据流内部将上一阶段的数据直接分发给对应的下游接收组件。LazyLLM支持`Pipeline`、`Parallel`、`Switch`、`If`、`Loop`、`Diverter`、`Warp`、`Graph`等数据流。

![](img/e98cf3aad18d45c8c65b4f99ff3dee9f.png)

本节将详细为您介绍 LazyLLM 中典型的`Flow`组件：`Pipeline`、`Parallel`、`Warp`以及`Bind`，方便您在接下来的教程中理解和使用。其余的组件介绍可见API文档：https://docs.lazyllm.ai/zh-cn/latest/API%20Reference/flow/

> 注意：
>
> LazyLLM 支持数据流的 `with` 语句。`with` 语句可以让数据流定义代码更加的简洁和清晰。下面关于数据流的所有例子不仅展示了基于函数式的数据流构建，同时在右侧也对应展示了通过 `with` 语法定义的数据流，`with`语句的巧妙之处在于定义代码的缩进与目标数据流结构有一定的关联性，可以清晰的看出整体结构。

## Pipeline

`Pipeline` 是顺次执行的数据流，上一个阶段的输出成为下一个阶段的输入。`pipeline` 支持函数和仿函数（或仿函数的 `type`），其工作流如下所示：

```
input -> module1 -> ... -> moduleN -> out
```

下面是通过 `Pipeline` 执行顺序结构程序的例子, 任意函数、匿名函数以及可调用实例（含`__call__()`函数的类）都可以作为 `pipeline` 中的一员。

函数式：

```python
import lazyllm

f1 = lambda x: x * 2

def f2(input):
  return input - 1

class AddOneFunctor(object):
  def __call__(self, x): return x + 1

f3 = AddOneFunctor()

# 手动调用：
inp = 2
x1 = f1(inp)
x2 = f2(x1)
x3 = f3(x2)
out_normal = AddOneFunctor()(x3)

# 使用数据流
ppl = lazyllm.pipeline(f1, f2, f3, AddOneFunctor)
out_ppl1 = ppl(inp)

print(f"输入为{inp},手动调用输出:", out_normal)
print(f"输入为{inp},数据流输出:", out_ppl1)
```

输出：

```
输入为2,手动调用输出: 5
输入为2,数据流输出: 5
```

with式：

```python
import lazyllm

f1 = lambda x: x * 2

def f2(input):
  return input - 1

class AddOneFunctor(object):
  def __call__(self, x): return x + 1

f3 = AddOneFunctor()

# 使用with方式的数据流
with lazyllm.pipeline() as ppl:
    ppl.func1 = f1
    ppl.func2 = f2
    ppl.func3 = f3
    ppl.func4 = AddOneFunctor

inp = 2
out_ppl1 = ppl(inp)

print(f"输入为{inp},数据流输出:", out_ppl1)
```

输出：

```
输入为2,数据流输出: 5
```

[1pipeline.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PCTfbLIIQosuMwxphPGcp3tInrh)

[1pipeline_with.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YWoVbUMy4oEHgqxvtAGcEbPsnwc)

## Parallel

`Parallel`支持我们并行调用多个`pipeline`，其数据流示意如下：

```
      /> module11 -> ... -> module1N -> out1 \
input ->  module21 -> ... -> module2N -> out2 -> (out1, out2, out3)
      \> module31 -> ... -> module3N -> out3 /
```



# Parallel

`Parallel`支持对输出格式进行格式化处理，方便下游组件进行针对性使用，目前`Parallel`支持输出字典、元组、列表和字符串形式的输出，具体效果可见下方例子：

## 函数式

```
import lazyllm

test1 = lambda a: a + 1
test2 = lambda a: a * 4
test3 = lambda a: a / 2

prl1 = lazyllm.parallel(test1, test2, test3)
prl2 = lazyllm.parallel(path1=test1, path2=test2, path3=test3).asdict
prl3 = lazyllm.parallel(test1, test2, test3).astuple
prl4 = lazyllm.parallel(test1, test2, test3).aslist
prl5 = lazyllm.parallel(test1, test2, test3).join('，')

print("默认输出：prl1(1) -> ", prl1(1), type(prl1(1)))
print("输出字典：prl2(1) -> ", prl2(1), type(prl2(1)))
print("输出元组：prl3(1) -> ", prl3(1), type(prl3(1)))
print("输出列表：prl4(1) -> ", prl4(1), type(prl4(1)))
print("输出字符串：prl5(1) -> ", prl5(1), type(prl5(1)))
```

输出：

```
默认输出：prl1(1) ->  (2, 4, 0.5) <class 'lazyllm.common.common.package'>
输出字典：prl2(1) ->  {'path1': 2, 'path2': 4, 'path3': 0.5} <class 'dict'>
输出元组：prl3(1) ->  (2, 4, 0.5) <class 'tuple'>
输出列表：prl4(1) ->  [2, 4, 0.5] <class 'list'>
输出字符串：prl5(1) ->  2，4，0.5 <class 'str'>
```

## with式

```
import lazyllm

test1 = lambda a: a + 1
test2 = lambda a: a * 4
test3 = lambda a: a / 2

with lazyllm.parallel() as prl1:
    prl1.func1 = test1
    prl1.func2 = test2
    prl1.func3 = test3

with lazyllm.parallel().asdict as prl2:
    prl2.path1 = test1
    prl2.path2 = test2
    prl2.path3 = test3

with lazyllm.parallel().astuple as prl3:
    prl3.func1 = test1
    prl3.func2 = test2
    prl3.func3 = test3

with lazyllm.parallel().aslist as prl4:
    prl4.func1 = test1
    prl4.func2 = test2
    prl4.func3 = test3

with lazyllm.parallel().join('，') as prl5:
    prl5.func1 = test1
    prl5.func2 = test2
    prl5.func3 = test3

print("默认输出：prl1(1) -> ", prl1(1), type(prl1(1)))
print("输出字典：prl2(1) -> ", prl2(1), type(prl2(1)))
print("输出元组：prl3(1) -> ", prl3(1), type(prl3(1)))
print("输出列表：prl4(1) -> ", prl4(1), type(prl4(1)))
print("输出字符串：prl5(1) -> ", prl5(1), type(prl5(1)))
```

输出：

```
默认输出：prl1(1) ->  (2, 4, 0.5) <class 'lazyllm.common.common.package'>
输出字典：prl2(1) ->  {'path1': 2, 'path2': 4, 'path3': 0.5} <class 'dict'>
输出元组：prl3(1) ->  (2, 4, 0.5) <class 'tuple'>
输出列表：prl4(1) ->  [2, 4, 0.5] <class 'list'>
输出字符串：prl5(1) ->  2，4，0.5 <class 'str'>
```

[2parallel.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RaTmb88gZo3t5HxTCnkcZbsBnJg)

[2parallel_with.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JrKtbKFGuovCCtxeIXycjOcHnUc)

# Warp

Warp是一款强大的控制流工具，旨在将同一处理模块并行地应用于一系列输入数据。它通过高效地“形变”单个模块至各个输入，确保每个输入都能得到并行处理，从而大幅提升处理效率。

### 工作原理示意图

```
#                 > in1 \                            > out1 \
# (in1, in2, in3) -> in2 -> module1 -> ... -> moduleN -> out2 -> (out1, out2, out3)
#                 > in3 /                            > out3 /
```

> **注意**：
> + Warp流不应用于异步任务，如训练和部署。
> + Warp流不支持字典输出

与`Parallel`工具类似，`Warp`支持对输出格式进行灵活处理。目前，它能够输出元组、列表和字符串形式的结果输出（注意：暂不支持字典输出）。具体的格式化效果及示例，请参见下方例子：

## 函数式

```
import lazyllm

test1 = lambda a: a + 1
test2 = lambda a: a * 4
test3 = lambda a: a / 2

prl1 = lazyllm.warp(test1, test2, test3)
# prl2 = lazyllm.warp(path1=test1, path2=test2, path3=test3).asdict # Not Implemented
prl3 = lazyllm.warp(test1, test2, test3).astuple
prl4 = lazyllm.warp(test1, test2, test3).aslist
prl5 = lazyllm.warp(test1, test2, test3).join('，')

inputs = [1, 2, 3]

print("默认输出：prl1(1) -> ", prl1(inputs), type(prl1(inputs)))
print("输出元组：prl3(1) -> ", prl3(inputs), type(prl3(inputs)))
print("输出列表：prl4(1) -> ", prl4(inputs), type(prl4(inputs)))
print("输出字符串：prl5(1) -> ", prl5(inputs), type(prl5(inputs)))
```

输出：

```
默认输出：prl1(1) ->  (4.0, 6.0, 8.0) <class 'lazyllm.common.common.package'>
输出元组：prl3(1) ->  (4.0, 6.0, 8.0) <class 'tuple'>
输出列表：prl4(1) ->  [4.0, 6.0, 8.0] <class 'list'>
输出字符串：prl5(1) ->  4.0，6.0，8.0 <class 'str'>
```

## with式

```
import lazyllm

test1 = lambda a: a + 1
test2 = lambda a: a * 4
test3 = lambda a: a / 2

with lazyllm.warp() as prl1:
    prl1.func1 = test1
    prl1.func2 = test2
    prl1.func3 = test3

with lazyllm.warp().astuple as prl3:
    prl3.func1 = test1
    prl3.func2 = test2
    prl3.func3 = test3

with lazyllm.warp().aslist as prl4:
    prl4.func1 = test1
    prl4.func2 = test2
    prl4.func3 = test3

with lazyllm.warp().join('，') as prl5:
    prl5.func1 = test1
    prl5.func2 = test2
    prl5.func3 = test3

inputs = [1, 2, 3]

print("默认输出：prl1(1) -> ", prl1(inputs), type(prl1(inputs)))
print("输出元组：prl3(1) -> ", prl3(inputs), type(prl3(inputs)))
print("输出列表：prl4(1) -> ", prl4(inputs), type(prl4(inputs)))
print("输出字符串：prl5(1) -> ", prl5(inputs), type(prl5(inputs)))
```

输出：

```
默认输出：prl1(1) ->  (4.0, 6.0, 8.0) <class 'lazyllm.common.common.package'>
输出元组：prl3(1) ->  (4.0, 6.0, 8.0) <class 'tuple'>
输出列表：prl4(1) ->  [4.0, 6.0, 8.0] <class 'list'>
输出字符串：prl5(1) ->  4.0，6.0，8.0 <class 'str'>
```

[4warp.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/J9jzb1aLxojYXtxY4LecQL8Hnoe)

![](img/0ff8f84bc12b2e247782e0f9179322a9.png)

[4warp_with.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/F5pqbDnidoJwjHxeZ2Sc94TxnVd)

# Bind

通过上述对数据流的介绍，我们会发现有一个问题是数据按照预定义的路径流动，我们似乎只能控制入端数据，当我们遇到想要在某个环节引入上两级的数据时就会遇到问题，如下面图示中的in到f3我们很难用上述的流来搭建。

![](img/841d79d1b7eae1671a19fbd485915a6b.png)

针对这个问题，`LazyLLM` 提供了一种参数绑定的方式，使数据在数据流中向下游自由地游走（跳跃的数据流）。

让我们先定义`f1`,`f21`,`f22`,`f3`上图中的几个函数：

```
def f1(input): return input ** 2
def f21(input1, input2=0): return input1 + input2 + 1
def f22(input1, input2=0): return input1 + input2 - 1
def f3(in1='placeholder1', in2='placeholder2', in3='placeholder3'): 
    return f'get [input:{in1}], [f21:{in2}], [f23: {in3}]'
```

`LazyLLM` 提供了 `lazyllm.bind` 函数进行函数绑定，`bind` 函数接收的参数格式如下所示：

```
lazyllm.bind(func, param1, param2, ...)
```

其中：

+   `func` 为需要进行参数绑定的函数，
+   `param1`, `param2` 依次为 `func` 接收的参数。实际上我们观察上图可以发现：
+   我们需要输入的`param1`就是`ppl`的输入（`ppl.input`），此处我们可以通过 `ppl.input` 在当前数据流获取对应的输入。
+   `param2` 和 `param3` 是`f3`的上一级，`LazyLLM`对这种情况定义了一种占位符，`_0`。`_0`表示数据流中当前节点的上一级输出，如果上一级中有多个输出，则可以通过`_1`, `_2`, `_3`依次获取，这里有两个输出所以分别是`_0`, `_1`。

基于`LazyLLM`的参数绑定可以实现上图中的数据跳连：

```
from lazyllm import pipeline, parallel, bind, _0, _1

with pipeline() as ppl1:
  ppl1.f1 = f1
  with parallel() as ppl1.subprl2:
    ppl1.subprl2.path1 = f21
    ppl1.subprl2.path2 = f22
  ppl1.f3 = bind(f3, ppl1.input, _0, _1)
  
print("ppl1 out: ", ppl1(2))
```

输出：

```
ppl1 out:  get [input:2], [f21:5], [f23: 3]]
```

> 注意，参数绑定仅在当前数据流内生效，您无法将外部变量数据绑定至数据流内，也无法跨数据流进行数据绑定。

![](img/4e59451fc1e55831007f8508142eee5d.png)

[8jump1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WiNxb2o4ooOhVXxPId1c8xr5nCd)

`LazyLLM`为 `bind` 函数重载了 `|` 运算符，可以通过它实现更直观的参数绑定，将函数和参数区分开来（`|`运算符之前的是被绑定的函数，之后是需要绑定的参数）。此外，子数据流也支持绑定入参。下面我们将上图修改为更复杂的流，我们将数据流输入绑定为`subprl2`的第一个输入(下图红色箭头线)，上游`f1`的输出为`subprl`的第二个输入。

```
from lazyllm import pipeline, parallel, bind, _0, _1

with pipeline() as ppl1:
  ppl1.f1 = f1
  with parallel().bind(ppl1.input, _0) as ppl1.subprl2:
    ppl1.subprl2.path1 = f21
    ppl1.subprl2.path2 = f22
  ppl1.f3 = f3 | bind(ppl1.input, _0, _1)
  
print("ppl1 out: ", ppl1(2))
```

输出：

```
ppl1 out:  get [input:2], [f21:7], [f23: 5]]
```

![](img/8a1c283c33f945990b6f72066fbcac79.png)



[8jump2.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OGspbSDzVoAB9Bxfow8czvepnVx)

## 4.3 使用LazyLLM快速搭建高性能RAG应用

在了解过`LazyLLM`的相关组件之后，我们使用它们，并结合书生大模型搭建一个高性能的RAG系统：

我们选择一些法律、论文pdf等文件放在路径中：

![](img/faab7f69cc20307abefd3c32957a732a.png)

实现代码如下：

```
import lazyllm

# 定义llm、embedding、reranker模型
llm = lazyllm.OnlineChatModule(
    source='openai',
    base_url="https://chat.intern-ai.org.cn/api/v1/",
    model="internlm3-latest",
    stream=True
)
embedding_model = lazyllm.TrainableModule("bge-large-zh-v1.5").start()
rerank_model = lazyllm.TrainableModule("bge-reranker-large").start()
# 定义文件路径及Milvus存储配置
DOC_PATH = "/mnt/lustre/share_data/dist/test_docs"
milvus_store_conf = {
    'type': 'milvus',
    'kwargs': {
        'uri': "dbs/test_rag.db",
        'index_kwargs': {
            'index_type': 'HNSW',  # 索引类型使用HNSW
            'metric_type': 'COSINE',  # 相似度类型使用COSINE
            }
        }
}

# 定义Document，加载文档
docs = lazyllm.Document(dataset_path=DOC_PATH, embed=embedding_model, store_conf=milvus_store_conf)
# 按句子划分，创建节点组
docs.create_node_group(name='sentence', parent="MediumChunk", transform=(lambda d: d.split('。')))
# 设计prompt
prompt = '你是一个友好的 AI 问答助手，你需要根据给定的上下文和问题提供答案。\
    根据以下资料回答问题：\
    {context_str} \n '

# 定义RAG应用 pipeline
with lazyllm.pipeline() as ppl:
    # Parallel 多路并行召回
    with lazyllm.parallel().sum as ppl.prl:
        ppl.prl.r1 = lazyllm.Retriever(docs, group_name="MediumChunk", topk=6)
        ppl.prl.r2 = lazyllm.Retriever(docs, group_name="sentence", target="MediumChunk", topk=6)
    # reranker重排序
    ppl.reranker = lazyllm.Reranker(name='ModuleReranker',model=rerank_model, topk=3) | lazyllm.bind(query=ppl.input)
    # 召回结果转换为字符串格式上下文，以拼接prompt
    ppl.formatter = (
        lambda nodes, query: dict(
            context_str="\n".join(node.get_content() for node in nodes),
            query=query)
    ) | lazyllm.bind(query=ppl.input)
    ppl.llm = llm.prompt(lazyllm.ChatPrompter(instruction=prompt, extra_keys=['context_str']))

w = lazyllm.WebModule(ppl, port=23492, stream=True).start().wait()
```

代码精讲：

1. 选择使用`书生API-KEY`，并使用`internlm3-latest`作为系统的大模型，同时使用`TrainableModule`本地部署`embedding`模型`bge-large-zh-v1.5`和`reranker`模型`bge-reranker-large`。

2. 给定知识库路径以及`Milvus`向量数据库的相关配置。

3. 创建文档加载模块`Document` `docs`，设置存储配置和使用的`embedding`模型。并调用`create_node_group`基于已有节点组`MediumChunk`（按照`token`长度256、重叠长度25切分文档）的基础上按照句子划分成更细粒度的文档切片。

4. 使用`Flow`组件，像搭积木一样搭建RAG系统，数据流的流向如下：

5. 首先用户查询`query`会并行流向两个`Retriever`，检索器在不同节点组中并行查询相似度较高的节点；

6. 待并行检索结束后，收集所有检索器的结果传入至`Reranker`当中精排，并取相似度得分最高的top 3

7. 精排后的节点传入至`ppl.formatter`，这里会将所有节点的内容拼接在一起，形成`llm`所需的`context_str`，同时使用`bind`将`query`一并组装为字典

8. 组装后的字典传入大模型，模型根据上下文问答用户的问题。

9. 最后将`pipeline`传入`WebModule`，启动交互界面，即可运行RAG系统。

我们通过查询`query`：“什么是`lightrag`”，将`RAG`系统与之前的`Chatbot`对比效果：

![](img/ef61317ff79ac67097e39fca7c103997.png)

![](img/c1b6a68e4299b338acefd84fb615799e.png)

可以看到，原来的`Chatbot`并不知道什么是`LightRAG`，所以给出的答案是基于LLM幻觉生成。而RAG系统基于召回的论文文段生成了较为最准确的答案。

## 4.4 基于InternVL的多模态RAG应用

如果一个RAG系统只能处理文字信息、回答文字，未免有些太“玩具”了，得益于`InternVL`强大的视觉理解能力，我们能够使用其理解图文，搭建图文并茂的多模态RAG系统。

在RAG系统中，多模态信息有两种处理方式：统一至文本与统一至向量空间

![](img/a5ad2dcbd37ea952a939ce3b0baefd0a.png)

- 统一到文本模态：直接使用`InternVL`为文档中的每一张图表生成一段描述，按照传统方式进行向量嵌入。
- 统一到向量空间：使用多模态`embedding`模型，将文本与图标直接映射到统一向量空间中。

本次我们展示后者，即在使用`InternVL`生成图片描述的同时，使用多模态`embedding`模型将`text`与`image`映射至统一的向量空间中。

### 基于InternVL的多模态文档解析器

对于包含图片的文档来说，现有的解析组件无法较好的满足多模态RAG的需求，因此我们需要基于`LazyLLM`提供的`ReaderBase`基类，自定义一个符合需求的解析器，注意，这里我们使用了一款高性能开源解析工具`magic-pdf`(https://github.com/opendatalab/MinerU)来实现较为高质量的pdf文件解析。

以下代码重点体现使用`InternVL`为图片添加描述的过程，自定义`Reader`的细节可参考`LazyLLM`社区的开源RAG课程：

```
class MagicPDFReader(ReaderBase):
    def __init__(self):
        self.image_save_path = get_image_path()
        self.model = None
        self.vlm = lazyllm.TrainableModule('internvl2-8b').start()   # 初始化一个多模态大模型

    def _result_extract(self, content_list):
        ......
        if not content["img_path"]:
        ......
            block['img_desc'] = self.vlm(formatted_query(block["image_path"])) # 利用VLM对图像内容进行解析生成图像的文本描述

    def _load_data(self, file: Path, split_documents: Optional[bool] = True) -> List[DocNode]:
        ......
        for k, v in elements.items(): # 对其中的一个block
            ......
            if "text" in element:
                  docs.append(DocNode(text=element["text"], metadata=metadata))
            elif "img_desc" in element:
                  image_node = ImageDocNode(text=element["img_desc"], 
        image_path=element["image_path"], global_metadata=metadata)# 构建ImageDocNode节点 
                  docs.append(image_node)
            else:
                  docs.append(DocNode(text="", metadata=metadata)) 
```

代码解读：

- 自定义`Reader`只需要重新实现`_load_data`方法即可，其中传入文件路径后，将文件路径传入`magic-pdf`组件，即可获取解析结果`elements`。
- 对`elements`遍历，其中如果是图片类型节点，则调用`InternVL`模型对其进行描述，并创建`ImageDocNode`图片节点，否则为默认的`DocNode`节点。

创建自定义解析器后，使用`.add_reader()`即可将解析器注册至`Document`组件：

```
documents.add_reader("*.pdf", MagicPDFReader)
```

这样，对于文件路径下的pdf格式文件，`LazyLLM`就会自动调用自定义解析器了。

### 多模态RAG系统搭建

基于先前的知识，按照如下所示的系统流程图，使用`InternVL`搭建多模态RAG系统。

![](img/054205a753c4c30ab2a06c0f8215229b.png)

代码如下：

```
# 本地部署多模态embedding模型
embed_multimodal = lazyllm.TrainableModule("colqwen2-v0.1")
# 使用线上的千问 text embedding模型
embed_text = lazyllm.OnlineEmbeddingModule(
    source='qwen', embed_model_name='text-embedding-v1')
# 定义embedding模型字典
embeds = {'vec1': embed_text, 'vec2': embed_multimodal}

documents = lazyllm.Document(dataset_path=tmp_dir.rag_dir, embed=embeds)
# 为pdf格式的文件注册自定义解析器
documents.add_reader("*.pdf", MagicPDFReader)
# 创建按行拆分的block节点组
documents.create_node_group(name="block", transform=lambda s: s.split("\n") if s else '')

with lazyllm.pipeline() as ppl:
    with lazyllm.parallel().sum as ppl.prl:
        ppl.prl.retriever1 = lazyllm.Retriever(documents, group_name="block", embed_keys=['vec1'], similarity="cosine", topk=1)
        # 多路召回中，使用多模态embedding将查询问题映射至图片相同的向量空间中，检索与query相似的图片
        ppl.prl.retriever2 = lazyllm.Retriever(documents, group_name="Image", embed_keys=['vec2'], similarity="maxsim", topk=2)
    # 将召回节点与query组合成最终
    ppl.prompt = build_vlm_prompt | bind(_0, ppl.input)
    ppl.vlm = lazyllm.TrainableModule("internvl2-8b").prompt(lazyllm.ChatPrompter(gen_prompt))

lazyllm.WebModule(ppl, port=range(23468, 23470), static_paths=get_image_path()).start().wait()
```

我们以`DeepSeek`的技术报告为测试文档，代码运行结果：

query：详细介绍一下`deepseek`基本概念和模型性能

`block`节点组召回内容：

```
For education-oriented knowledge benchmarks such as MMLU, MMLU-Pro, and GPQA Diamond, DeepSeek-R1 demonstrates superior performance compared to DeepSeek-V3. This improvement is primarily attributed to enhanced accuracy in STEM-related questions, where significant gains are achieved through large-scale reinforcement learning. Additionally, DeepSeek-R1 excels on FRAMES, a long-context-dependent QA task, showcasing its strong document analysis capabilities. This highlights the potential of reasoning models in AI-driven search and data analysis tasks. On the factual benchmark SimpleQA, DeepSeek-R1 outperforms DeepSeek-V3, demonstrating its capability in handling fact-based queries. A similar trend is observed where OpenAI-o1 surpasses GPT-4o on this benchmark. However, DeepSeek-R1 performs worse than DeepSeek-V3 on the Chinese SimpleQA benchmark, primarily due to its tendency to refuse answering certain queries after safety RL. Without safety RL, DeepSeek-R1 could achieve an accuracy of over 70%.
```

`Image`节点组召回结果：

`/home/mnt/sunxiaoye/.lazyllm/rag_for_qa/images/c357bd57757e4c544fdda3ad32066e64c5d01bb8540066f1a722c8872d664183.jpg`



`/home/mnt/sunxiaoye/.lazyllm/rag_for_qa/images/2c6271b8cecc68d5b3c22e552f407a5e97d34030f91e15452c595bc8a76e291c.jpg`

![](img/3723bfc2a6fdb7908cc56415dc1457a6.png)

回答情况：

![](img/5fe2237b1326de6e7eb9cec833ed683f.png)

# 第五章 使用LazyLLM微调大模型

线上的`InternLM`固然强大，然而对于一些需要私有化部署，且GPU资源有限的场景下，我们就不得不选择参数量较小的模型，较小模型的能力有时不满足我们的需求，由此我们需要对其进行针对性的微调任务。`LazyLLM`的能力不只局限于大模型应用开发，一键运行模型微调也是其重要特性。本章我们将介绍如何使用`LazyLLM`实现书生大模型的微调训练。

我们选择一个参数较小的模型 `InternLM2-Chat-7B`，使用`CMRC2018`数据集中的训练数据来微调这个小模型，让它能具有更好的中文阅读理解信息抽取能力。整体的微调步骤如下：

![](img/f3bb82900d398fbaac2f7e9353846ded.png)

+ 微调方法：

+ LoRA策略：冻结原始权重，仅训练低秩矩阵（`BA`），降低显存占用。

+ 控制输出：强化遵循原文内容且精简输出的能力。

## 5.1 数据准备

### 数据集简介

我们选用`CMRC2018`数据集，该数据集由近15,000个真实问题组成，这些问题是由人类专家在维基百科段落上标注而成。另外该数据集对应的任务是：“篇章片段抽取型阅读理解”（Span-Extraction Reading Comprehension），根据给定的一个文档和一个问题，模型需要从该文档中抽取出问题的答案，其中答案是该文章某个连续的片段。

### 数据集规模

`CMRC2018`数据集主要由三部分构成：`test`、`validation` 和`train`。这里我们将`train`部分作为我们的训练集，`test`部分作为我们的评测集，`validation`部分我们不使用。

| 数据集 | 短文数 | 问题数 | 用途 |
| --- | --- | --- | --- |
| test | 256 | 1002 | 评测集 |
| train | 2403 | 10142 | 训练集 |
| validation | 848 | 3219 | 未使用 |

> 注意：

+ 之前教程中的知识库是基于 `CMRC2018` 中的`test`数据集中的文段来构建的；

+ 这里训练我们使用的 `CMRC2018` 中的`train`数据集来进行；

+ 评测过程，为避免`RAG`系统中检索模块干扰，我们采用控制变量法，假设召回100%没问题，即：我们直接使用 `CMRC2018` 中的`test`数据集中的文段片段和问题拼接起来作为大模型`LLM`的输入，变量是微调前后的两个`7B`的模型，以对比查看微调后模型在中文阅读理解信息抽取能力方面是否有提升；

### 数据的结构

数据集中的`test`和`train`的结构是一致的，我们抽取其中的一条数据来看如下：

```
[
    {
        "id": "TRIAL_154_QUERY_0",
        "context": "尤金袋鼠（\"Macropus eugenii\"）是袋鼠科中细小的成员，通常都是就袋鼠及有袋类的研究对象。尤金袋鼠分布在澳洲南部岛屿及西岸地区。由于牠们每季在袋鼠岛都大量繁殖，破坏了针鼹岛上的生活环境而被认为是害虫。尤金袋鼠最初是于1628年船难的生还者在西澳发现的，是欧洲人最早有纪录的袋鼠发现，且可能是最早发现的澳洲哺乳动物。尤金袋鼠共有三个亚种：尤金袋鼠很细小，约只有8公斤重，适合饲养。尤金袋鼠的奶中有一种物质，称为AGG01，有可能是一种神奇药及青霉素的改良。AGG01是一种蛋白质，在实验中证实比青霉素有效100倍，可以杀死99%的细菌及真菌，如沙门氏菌、普通变型杆菌及金黄色葡萄球菌。",
        "question": "尤金袋鼠分布在哪些地区？",
        "answers": {
            "text": [
                "尤金袋鼠分布在澳洲南部岛屿及西岸地区"
            ],
            "answer_start": [
                52
            ]
        }
    },
...
]
```

上面数据中：

+ `context`: 是文本段；

+ `question`: 是针对文本段的提问；

+ `answers`: 给出了对应问题的答案，包括：

  + `text`: 答案的具体内容，源自文本段；

  + `answer_start`: 答案在文本段中的起始位置；

### 训练集处理

+ 我们从原`train`数据集中抽取出文章字段`context`和问题字段`question`，将它拼接成为用于微调的`instruction`字段，拼接模板是："请用下面的文段的原文来回答问题\n\n### 已知文段：{context}\n\n### 问题：{question}\n"。

+ 将原数据中的`answers`及其`text`字段作为，微调的 `output` 字段

+ 由于微调还需要一个`input`字段，而我们这个任务不需要，所以设置为空。

```
# Template for constructing QA prompts
template = "请用下面的文段的原文来回答问题\n\n### 已知文段：{context}\n\n### 问题：{question}\n"

def build_train_data(data):
    """Format training data using predefined template"""
    extracted_data = []
    for item in data:
        extracted_item = {
            "instruction": template.format(context=item["context"], question=item["question"]),
            "input": "",
            "output": item["answers"]["text"][0]
        }
        extracted_data.append(extracted_item)
    return extracted_data
```

我们取处理后的一条数据如下：

```
[
    {
        "instruction": "请用下面的文段的原文来回答问题\n\n### 已知文段：黄独（学名：）为薯蓣科薯蓣属的植物。多年生缠绕藤本。地下有球形或圆锥形块茎。叶腋内常生球形或卵圆形珠芽，大小不一，外皮黄褐色。心状卵形的叶子互生，先端尖锐，具有方格状小横脉，全缘，叶脉明显，7-9条，基出；叶柄基部扭曲而稍宽，与叶片等长或稍短。夏秋开花，单性，雌雄异株，穗状花序丛生。果期9-10月。分布于大洋洲、朝鲜、非洲、印度、日本、台湾、缅甸以及中国的江苏、广东、广西、安徽、江西、四川、甘肃、云南、湖南、西藏、河南、福建、浙江、贵州、湖北、陕西等地，生长于海拔300米至2,000米的地区，多生于河谷边、山谷阴沟或杂木林边缘，目前尚未由人工引种栽培。在美洲也可发现其踪迹，对美洲而言是外来种，有机会在农田大量繁殖，攀上高树争取日照。英文别名为air potato。黄药（本草原始），山慈姑（植物名实图考），零余子薯蓣（俄、拉、汉种子植物名称），零余薯（广州植物志、海南植物志），黄药子（江苏、安徽、浙江、云南等省药材名），山慈姑（云南楚雄）\n\n### 问题：黄独的外皮是什么颜色的？\n",
        "input": "",
        "output": "外皮黄褐色"
    },
...
]
```

### 评测集处理

我们评测集的字段基本都保留了下来，仅对`answers`字段进行了处理，将其中的内容提取了出来：

```
def build_eval_data(data):
    """Extract necessary fields for evaluation dataset"""
    extracted_data = []
    for item in data:
        extracted_item = {
            "context": item["context"],
            "question": item["question"],
            "answers": item["answers"]["text"][0]
        }
        extracted_data.append(extracted_item)
    return extracted_data
```

我们取处理后的一条数据如下：

```
[
    {
        "context": "芙蓉洞位于重庆武隆县江口镇的芙蓉江畔，距武隆县城20公里。芙蓉洞于1993被发现，1994年对游人开放。2002年被列为中国国家4A级旅游景区，2007年6月作为中国南方喀斯特－武隆喀斯特的组成部分被列入联合国世界自然遗产，是中国首个被列入世界自然遗产的溶洞。芙蓉洞全长2846米，以竖井众多、洞穴沉积物类型齐全著称。芙蓉洞附近的天星乡境内，有世界上罕见的喀斯特竖井群。在约20平方公里的范围内，至少散布着50个超过100米的竖井，其中汽坑洞的深度为920米，为亚洲第一。芙蓉洞内有70多种沉积物，几乎包括了所有经过科学家命名的喀斯特洞穴沉积类型，其中池水沉积堪称精华。在芙蓉洞的东端有一个“石膏花支洞”，洞内的鹿角状卷曲石枝长57厘米，为世界第一。目前石膏花支洞被永久封存。在紧邻芙蓉洞洞口的芙蓉江上已经建起了江口电站大坝，水库蓄水后对地下水循环及喀斯特景观的演化造成的影响目前还难以估量。",
        "question": "芙蓉洞位于什么地方？",
        "answers": "重庆武隆县江口镇的芙蓉江畔"
    },
 ...
]
```

## 5.2 微调模型

在数据处理完成后，我们可以开始进行微调了。

> 值得注意的是：`LazyLLM`支持微调、部署、推理一条龙！

微调相关配置代码主要如下所示：

```
import lazyllm
from lazyllm import finetune, deploy, launchers

model = lazyllm.TrainableModule(model_path)\
    .mode('finetune')\
    .trainset(train_data_path)\
    .finetune_method((finetune.llamafactory, {
        'learning_rate': 1e-4,
        'cutoff_len': 5120,
        'max_samples': 20000,
        'val_size': 0.01,
        'per_device_train_batch_size': 2,
        'num_train_epochs': 2.0,
        'launcher': launchers.sco(ngpus=8)
    }))\
    .prompt(dict(system='You are a helpful assistant.', drop_builtin_system=True))\
    .deploy_method(deploy.Vllm)
model.evalset(eval_data)
model.update()
```

上面代码中，使用`LazyLLM`的`TrainableModule`来实现：微调->部署->推理：

+ 模型配置：

  + `model_path`指定了我们要微调的模型，这里我们用`Internlm2-Chat-7B`，直接指定其所在路径即可；

+ 微调配置：

  + `.mode()`设置了启动微调模式`finetune`；

  + `.trainset` 设置了训练用的数据集路径，这里用到的就是我们前面处理好的训练集；

  + `.finetune_method` 设置了用哪个微调框架及其参数，这里传入了一个元组（只能设置两个元素）：

    + 第一个元素指定了使用的微调框架是`Llama-Factory`：`finetune.llamafactory`

    + 第二个元素是一个字典，包含了对该微调框架的参数配置；

+ 推理配置：

  + `.prompt` 设置了推理时候用的`Prompt`，注意，这里为了和微调的`Prompt`中的`system`字段保持一致，所以开启`drop_builtin_system`以将原`system-prompt`给替换为`You are a helpful assistant.`

  + `.deploy_method` 设置了部署用的推理框架，这里指定了`vLLM`这个推理框架；

+ 评测配置：

  + 这里通过`.evalset`来配置了我们之前处理好的评测集；

+ 启动任务：

  + `.update` 触发任务的开始：模型先进行微调，微调完成后模型会部署起来，部署好后会自动使用评测集全部都过一遍推理以获得结果；

微调中的一些关键参数如下：

| 参数 | 作用 | 推荐设置 | 调优建议 |
| --- | --- | --- | --- |
| `learning_rate` | 控制参数更新幅度 | 1e-4~5e-5 | 大模型取较小值 |
| `cutoff_len` | 最大上下文长度 | 5120 | 根据GPU显存调整 |
| `max_samples` | 最大训练样本量 | 20000 | 注意太小导致训练用的数据太少 |
| `per_device_train_batch_size` | 单卡批次大小 | 2 | 显存不足时减小 |
| `num_train_epochs` | 训练轮次 | 2.0 | 根据任务loss下降情况来设置 |

额外，我们还可以配置一些`LoRA`相关的参数，`LazyLLM`已经默认设置好了一套经验的参数，所以上述代码中没有体现，这里我们展示如下，您可以尝试各类参数来炼丹：

| 参数 | 作用 | 推荐设置 | 说明 |
| --- | --- | --- | --- |
| `lora_alpha` | `LoRA` 缩放因子 | 16 | - |
| `lora_dropout` | `LoRA` 丢弃率 | 0.0 | - |
| `lora_rank` | `LoRA` 秩 | 8 | - |
| `lora_target` | `LoRA` 总用模型的目标模块 | all | 模型中的所有线性模块。 |

## 5.3 效果评测

在上一步微调并获得评测集的推理结果后，我们需要拿着结果和评测集的正确答案做个对比，以确认我们微调的效果是什么样的。

### 评测目的

+ **验证微调效果**：对比模型输出与标准答案，量化模型优化成果（是否超越通用大模型）

+ **任务适配性检验**：通用指标（忠诚度、答案相关性）不适用于“篇章片段抽取”任务，需定制化评估

+ **优化方向指导**：通过指标差异定位模型短板（如完全一致性、语义精准性、原文依赖性）

### 评测指标设计

这里，针对我们的任务特点，设计了三个维度的评价指标：

+ **精确匹配率**（Exact Match）

+ **语义相似度**（Cosine Score）

+ **原文包含度**（Origin Score）

我们现在来详细介绍一下这三个指标的设计细节：

#### 精确匹配率

我们将精确匹配度定义如下：

其中：

+ `N`：测试样本总数；

+ `y_i`：第`i`个样本的标准答案；

+ `\hat{y}_i`：模型预测结果；

+ `\mathbb{I}`：指示函数（完全匹配时取1，否则取0）；

该指标的特性在于：预测结果与标准答案需要完全一致

在不考虑计算平均值下，只实现其中的一项代码很简单，如下所示：

```
exact_score = 1 if output == true_v else 0
```

**代码功能：**

该代码用于判断模型对单个样本的预测结果（`output`）与该样本的标准答案（`true_v`）是否完全一致，并据此给出一个精确匹配得分（`exact_score`）。

**代码解释：**

+ `output`：模型对某个样本的预测结果。

+ `true_v`：该样本的标准答案。

+ `exact_score`：精确匹配得分，取值为1或0。如果预测结果与标准答案完全一致，则得分为1；否则得分为0。

#### 语义相似度

我们将语义相似度度定义如下：

其中：

+ `N`：测试样本总数；

+ `y_i`：第`i`个样本的标准答案；

+ `\hat{y}_i`：模型预测结果；



## 语义相似度

+ `\hat{y}_i`：是基于`BGE`模型（`bge-large-zh-v1.5`）的向量编码，它可以把自然语言编码为一个向量，即：`\hat{y}_i`；

> 值得注意的是：该评价指标将原本[-1, 0)的数值进行了截断，只要是负相关的语义都给0分，得分只能是正相关的。

相应的代码实现如下，这里不含求平均，只是其中一项，同时假设文本已经被`BGE`模型向量化了：

```
import numpy as np

def cosine(x, y):
    """Calculate cosine similarity between two vectors"""
    product = np.dot(x, y)
    norm = np.linalg.norm(x) * np.linalg.norm(y)
    raw_cosine = product / norm if norm != 0 else 0.0
    return max(0.0, min(raw_cosine, 1.0))
```

### 代码功能

该代码实现了一个计算两个向量之间余弦相似度的函数，特别适用于评估模型预测结果与标准答案之间的语义相似度。这是上述精确匹配度定义（`CS`）中的核心计算部分，用于单个样本的相似度计算。

### 代码解释

+ `x` 和 `y`：分别代表两个向量，在实际应用中，这两个向量通常是由`BGE`模型对文本进行向量化后的结果，即 `N` 和 `N`。

+ `np.dot(x, y)`：计算两个向量的点积。

+ `np.linalg.norm(x)` 和 `np.linalg.norm(y)`：分别计算向量 `x` 和 `y` 的 L2 范数（即欧几里得范数）。

+ `product / norm`：计算原始的余弦相似度值。

+ `raw_cosine`：存储原始的余弦相似度值，但如果分母为 0（即两个向量中至少有一个是零向量），则直接返回 0.0。

+ `max(0.0, min(raw_cosine, 1.0))`：对原始的余弦相似度值进行截断处理，确保结果在 [0, 1] 范围内，且负相关的语义得 0 分。

## 原文包含度

我们将原文包含度定义如下：

其中：

+ `N`：测试样本总数；

+ `\hat{y}_i`：模型预测结果；

+ `N`：表示预测结果中的每个字；

+ `N`：文章内容；

+ `\mathbb{I}`：指示函数（所有字在原文中出现时取 1，否则有多于原文的字都取 0）

相应的代码我们实现如下，这里不含求平均，只涉及其中一项：

```
def check_words_from_content(infer, content):
    """Check if all words in inference output exist in original context"""
    return 1 if all(w in content for w in infer.split()) else 0
```

### 代码功能

该代码实现了一个函数`check_words_from_content`，用于检查模型预测结果中的所有字是否都出现在原文内容中。这是上述原文包含度（`OS`）定义中的核心计算部分，用于单个样本的包含度判断。

### 代码解释

+ `infer`：模型对某个样本的预测结果，即 `N`。

+ `content`：该样本的原文内容，即 `N`。

+ `infer.split()`：将预测结果分割成单个字的列表。

+ `all(w in content for w in infer.split())`：使用 Python 的 `all` 函数和生成器表达式检查 `infer` 中的每个字或词汇是否都存在于 `content` 中。如果所有字或词汇都存在，则返回 `True`；否则返回 `False`。

+ `return 1 if ... else 0`：根据检查结果返回 `1` 或 `0`，作为指示函数 `\mathbb{I}` 的实现。如果所有字或词汇都存在于原文中，则返回 `1`；否则返回 `0`。

## 指标对比分析

我们将上面三个评价指标分别做对比分析：

| 指标维度 | 取值范围 | 理想值 | 数值特征 | 优势 | 局限性 | 任务评价维度说明 |
| --- | --- | --- | --- | --- | --- | --- |
| 精确匹配率 | {0, 1} | 1.0 | 二元判断 | 结果明确，无歧义 | 对表述差异零容忍 | 有多少推理能 100% 忠于答案 |
| 语义相似度 | [0, 1] | 1.0 | 连续数值 | 捕捉语义相似性 | 依赖编码模型质量 | 由于选取片段范围，或者表述有改变，所以用相似度来评价同一内容的不同表述； |
| 原文包含度 | {0, 1} | 1.0 | 二元判断 | 确保答案忠实原文 | 忽略合理同义替换 | 任务要求，回答必须是用原文，此指标可反映答案是否都源自原文； |

## 综合评测脚本

让我们将以上评价指标都汇集在一起，实现下面的完整评价：

```
def caculate_score(eval_set, infer_set):
    """Calculate three evaluation metrics: exact match, cosine similarity, and word containment"""
    assert len(eval_set) == len(infer_set)

    # Initialize embedding model
    m = lazyllm.TrainableModule('bge-large-zh-v1.5')
    m.start()

    accu_exact_score = 0
    accu_cosin_score = 0
    accu_origi_score = 0
    res = []
    for index, eval_item in enumerate(eval_set):
        output = infer_set[index].strip()
        true_v = eval_item['answers']
        # Exact match scoring:
        exact_score = 1 if output == true_v else 0
        accu_exact_score += exact_score
        # Cosine similarity scoring:
        outputs = json.loads(m([output, true_v]))
        cosine_score = cosine(outputs[0], outputs[1])
        accu_cosin_score += cosine_score
        # Word containment scoring:
        origin_score = check_words_from_content(output, eval_item['context'])
        accu_origi_score += origin_score
        res.append({'context': eval_item['context'],
                    'true': true_v,
                    'infer': output,
                    'exact_score': exact_score,
                    'cosine_score': cosine_score,
                    'origin_score': origin_score})
    save_res(res, 'eval/infer_true_cp.json')
    total_score = len(eval_set)
    return (f'Exact Score : {accu_exact_score}/{total_score}, {round(accu_exact_score/total_score,4)*100}%\n'
            f'Cosine Score: {accu_cosin_score}/{total_score}, {round(accu_cosin_score/total_score,4)*100}%\n'
            f'Origin Score: {accu_origi_score}/{total_score}, {round(accu_origi_score/total_score,4)*100}%\n')
```

上面代码中：

+ 首先传入测试集和推理的结果，并确保两个集合是一样大的；

+ 然后这里为了实现文本的向量化，我们采用 `LazyLLM` 的 `TrainableModule` 来加载一个 `bge-large-zh-v1.5` 模型，并采用 `.start` 给它部署起来；

+ 接着我们遍历所有数据，计算三个指标下的每次得分，并累计起来；

+ 最后我们保存所有结果，并计算最终的评价结果并以字符串返回。

## 评测结果对比

这里我们对比了微调前模型 `Internlm2-Chat-7B`，及其微调后的模型，同时我们还对比了线上模型 `DeepSeek-V3`。

下表结果中，括号内是得分，总分是 1002 分，百分比是得分占总分的百分比。

| 模型 | 精确匹配率 | 语义相似度 | 原文包含度 |
| --- | --- | --- | --- |
| `Internlm2-Chat-7B` | 2.10% | 74.51% | 5.19% |
| `DeepSeek-V3` | 5.29% | 74.85% | 15.17% |
| `Internlm2-Chat-7B`训练后 | **39.72%** | **86.19%** | **94.91%** |

从上面的评测结果我们可以看出，微调后的模型要比微调前，甚至是在线大模型的各项指标都要明显好。

### 精确匹配率飞跃

+ 微调后提升 37.62 个百分点（2.10% → 39.72%），超过在线模型近 8 倍

+ 说明模型学会遵循特定答案格式

### 语义相关性优化

+ 相似度提升 11.68 个百分点（74.51% → 86.19%）

+ 与在线模型（600 多 B 的大模型）对比：+11.34 个百分点优势

### 原文依赖度质变

+ 包含度从 5.19% 跃升至 94.91%，提升幅度达 18.3 倍

+ 表明模型已掌握：

+ ✅ 关键信息定位能力

+ ✅ 原文提取策略

+ ✅ 知识边界控制（避免幻觉）

基于实验数据我们可以得出结论：

> RAG 系统中，在召回模块准确和召回无误的前提下，生成模块中模型对任务的适应程度大大影响着最终的效果，而微调是一个有效的手段可以提升模型适应下游任务的能力，甚至通过微调后提升的该能力是可以很好超过通用大模型的。

## 通用评测对比

这里我们基于 [提升 RAG 召回](https://kh8gmexazp.feishu.cn/wiki/L8wDwa6FciHrUdk0r3WcgxHOnte) 中介绍的两个通用生成模块的评价指标来评测一下模型，并与我们设计的评价指标做对比。

| 模型 | 精确匹配率 | 语义相似度 | 原文包含度 | 答案相关度 | 忠诚度 |
| --- | --- | --- | --- | --- | --- |
| `Internlm2-Chat-7B` | 2.10% | 74.51% | 5.19% | 87.14% | 96.80% |
| `DeepSeek-V3` | 5.29% | 74.85% | 15.17% | 85.91% | 96.90% |
| `Internlm2-Chat-7B`训练后 | **39.72%** | **86.19%** | **94.91%** | 55.16% | 96.50% |

从上面表格的数据我们可以看到通用评价指标显示，在微调之后模型的答案相关度明显下降了，在这评价指标下说明微调是失败的，但真的如此吗？让我们抽取评测过程的数据来看看吧：

`Internlm2-Chat-7B` 微调后,答案相关度

```
{
    "context": "基于《跑跑卡丁车》与《泡泡堂》上所开发的游戏，由韩国Nexon开发与发行。中国大陆由盛大游戏运营，这是Nexon时隔6年再次授予盛大网络其游戏运营权。台湾由游戏橘子运营。玩家以水枪、小枪、锤子或是水炸弹泡封敌人(玩家或NPC)，即为一泡封，将水泡击破为一踢爆。若水泡未在时间内踢爆，则会从水泡中释放或被队友救援(即为一救援)。每次泡封会减少生命数，生命数耗完即算为踢爆。重生者在一定时间内为无敌状态，以踢爆数计分较多者获胜，规则因模式而有差异。以2V2、4V4随机配对的方式，玩家可依胜场数爬牌位(依序为原石、铜牌、银牌、金牌、白金、钻石、大师) ，可选择经典、热血、狙击等模式进行游戏。若游戏中离，则4分钟内不得进行配对(每次中离+4分钟)。开放时间为暑假或寒假期间内不定期开放，8人经典模式随机配对，采计分方式，活动时间内分数越多，终了时可依该名次获得奖励。",
    "exact_score": 1,
    "cosine_score": 0.9999,
    "origin_score": 1,
    "question": "生命数耗完即算为什么？",
    "true_answer": "踢爆",
    "answer": "踢爆",
    "infer_questions": [
        {
            "question": "\n踢爆这个词语是什么意思？",
            "score": 0.3781
        },
        {
            "question": "\n踢爆是什么意思？这个词在什么语境下使用？",
            "score": 0.3825
        },
        {
            "question": "\n踢爆是什么意思？这个词通常在什么语境下使用？",
            "score": 0.3829
        }
    ],
    "final_score": 0.3812
},
```

`DeepSeek-V3`，答案相关度



# RAG 评估与 LazyLLM 实践文档

```json
{
    "context": "基于《跑跑卡丁车》与《泡泡堂》上所开发的游戏，由韩国Nexon开发与发行。中国大陆由盛大游戏运营，这是Nexon时隔6年再次授予盛大网络其游戏运营权。台湾由游戏橘子运营。玩家以水枪、小枪、锤子或是水炸弹泡封敌人(玩家或NPC)，即为一泡封，将水泡击破为一踢爆。若水泡未在时间内踢爆，则会从水泡中释放或被队友救援(即为一救援)。每次泡封会减少生命数，生命数耗完即算为踢爆。重生者在一定时间内为无敌状态，以踢爆数计分较多者获胜，规则因模式而有差异。以2V2、4V4随机配对的方式，玩家可依胜场数爬牌位(依序为原石、铜牌、银牌、金牌、白金、钻石、大师) ，可选择经典、热血、狙击等模式进行游戏。若游戏中离，则4分钟内不得进行配对(每次中离+4分钟)。开放时间为暑假或寒假期间内不定期开放，8人经典模式随机配对，采计分方式，活动时间内分数越多，终了时可依该名次获得奖励。",
    "exact_score": 0,
    "cosine_score": 0.6646,
    "origin_score": 1,
    "question": "生命数耗完即算为什么？",
    "true_answer": "踢爆",
    "answer": "生命数耗完即算为踢爆。",
    "infer_questions": [
        {
            "question": "\n在游戏术语中,\"生命数耗完即算为踢爆\"是什么意思？",
            "score": 0.7191
        },
        {
            "question": "\n什么情况下会被视为踢爆？当生命数耗完时会怎样？",
            "score": 0.6896
        },
        {
            "question": "\n在游戏规则中，生命数耗完后会发生什么？",
            "score": 0.7918
        }
    ],
    "final_score": 0.7335
},
```

上面对比中，`true_answer` 是标注的答案，`answer` 是模型推理的答案，`question` 是问题，`答案相关度` 需要根据模型推理的答案 `answer` 来生成可能的问题（`infer_questions` 中的 `question`），这里是三个，然后让可能的问题与真实问题在向量化后求余弦相似度。在简短的 `answer` 之下，模型很难推测出和真实问题 `question` 相关的问题。

+   微调后的 `InternLM2-Chat-7B` 的 `answer` 是："踢爆"

+   `DeepSeek-V3` 的 `answer` 是："生命数耗完即算为踢爆。"

可以看到 `DeepSeek-V3` 因为提供的信息更多，所以评价模型推测出来的问题更加准确，所以得分就更高。但实际上这不符合我们的预期，我们希望它简短准确，正如标准答案 "踢爆" 一样！与之对应的我们这里采用的语义相似度 `cosine_score` 和 `exact_score` 精准匹配度就很好的反映预期。

忠诚度在这里几乎没有什么区分度：

`InternLM2-Chat-7B` 微调后，忠诚度：

```json
{
    "context": "中国（南京）国际象棋超级大赛（Pearl Spring Super Tournament）原名中国（南京）国际象棋特级大师邀请赛，首届比赛于2008年12月11日至22日在南京市浦口区明发珍珠泉大酒店举行。本次大赛由南京市人民政府、国家体育总局棋牌运动管理中心主办，浦口区人民政府、南京市体育局承办，康缘药业股份有限公司、扬子晚报、蒙代尔国际企业家大学协办。被国际棋联定为21级赛事，也是亚洲迄今为止举办的最高水平的国际象棋大赛。双循环赛制比赛十轮，12月11日至15日进行前5轮，16日休息，17至21日进行后5轮。每方90分钟，每步棋加30秒。总奖金25万欧元，其中冠军8万欧元，第2至6名依次为5万5千欧元、4万欧元、3万欧元、2万5千欧元、2万欧元。结果托帕洛夫夺得冠军，阿罗尼扬获得亚军，卜祥志获得第三名。2009年2月1日被接纳为大满贯赛事并更名为中国（南京）国际象棋超级大赛。第二届比赛于2009年9月27日-10月9日举行。“康缘药业杯”2010中国(南京)国际象棋超级大赛于2010年10月19-30日举行",
    "exact_score": 1,
    "cosine_score": 0.9999,
    "origin_score": 1,
    "question": "首届比赛在哪里举行？",
    "true_answer": "南京市浦口区明发珍珠泉大酒店",
    "answer": "南京市浦口区明发珍珠泉大酒店",
    "statements": "\n首届比赛在南京市浦口区举行。|||比赛的具体地点是明发珍珠泉大酒店。",
    "scores": [
        {
            "statement": "首届比赛在南京市浦口区举行。",
            "score": 1
        },
        {
            "statement": "比赛的具体地点是明发珍珠泉大酒店。",
            "score": 1
        }
    ],
    "final_score": 1.0
},
```

`DeepSeek-V3`，忠诚度：

```json
{
    "context": "中国（南京）国际象棋超级大赛（Pearl Spring Super Tournament）原名中国（南京）国际象棋特级大师邀请赛，首届比赛于2008年12月11日至22日在南京市浦口区明发珍珠泉大酒店举行。本次大赛由南京市人民政府、国家体育总局棋牌运动管理中心主办，浦口区人民政府、南京市体育局承办，康缘药业股份有限公司、扬子晚报、蒙代尔国际企业家大学协办。被国际棋联定为21级赛事，也是亚洲迄今为止举办的最高水平的国际象棋大赛。双循环赛制比赛十轮，12月11日至15日进行前5轮，16日休息，17至21日进行后5轮。每方90分钟，每步棋加30秒。总奖金25万欧元，其中冠军8万欧元，第2至6名依次为5万5千欧元、4万欧元、3万欧元、2万5千欧元、2万欧元。结果托帕洛夫夺得冠军，阿罗尼扬获得亚军，卜祥志获得第三名。2009年2月1日被接纳为大满贯赛事并更名为中国（南京）国际象棋超级大赛。第二届比赛于2009年9月27日-10月9日举行。“康缘药业杯”2010中国(南京)国际象棋超级大赛于2010年10月19-30日举行",
    "exact_score": 0,
    "cosine_score": 0.5574,
    "origin_score": 1,
    "question": "首届比赛在哪里举行？",
    "true_answer": "南京市浦口区明发珍珠泉大酒店",
    "answer": "首届比赛于2008年12月11日至22日在南京市浦口区明发珍珠泉大酒店举行。",
    "statements": "\n首届比赛于2008年12月11日至22日举行。|||比赛地点在南京市浦口区。|||具体举办地是明发珍珠泉大酒店。",
    "scores": [
        {
            "statement": "首届比赛于2008年12月11日至22日举行。",
            "score": 1
        },
        {
            "statement": "比赛地点在南京市浦口区。",
            "score": 1
        },
        {
            "statement": "具体举办地是明发珍珠泉大酒店。",
            "score": 1
        }
    ],
    "final_score": 1.0
},
```

上面的对比中，我们可以看到 `DeepSeek-V3` 虽然提供了更多的信息，但它和微调后的模型一样，基本都按照要求使用原文中的内容来回答，所以虽然它的 `statement` 更多（3 条），但最终得分和微调后模型一样。这也就解释了为什么最终的忠诚度得分没有区分度。值得注意的是这里用 `cosine_score` 语义相似度和 `exact_score` 精确匹配度，很好区分了两者的差异。

从上面的两个指标的对比结果我们可以看到，如果对 RAG 系统中的生成模块有特定的任务要求，常用的评价指标并不能很好衡量，此时就需要针对任务的最终效果需求来进行评价指标的设计！

## 总结

本次实践我们首先介绍了 `LazyLLM` 的环境配置方法，随后为大家展示了如何使用 `LazyLLM` 并结合书生大模型 `InternLM` 以及 `InternVL` 搭建 `Chatbot` 和 `RAG` 应用，最后展示了如何使用 `LazyLLM` 一键微调书生大模型。

本次实践中我们将重点关注了 `LazyLLM` 的基础使用方式以及书生大模型相关的内容，对 `LazyLLM` 感兴趣的同学可以前往 `LazyLLM` 的官方仓库（https://github.com/LazyAGI/LazyLLM）了解更多信息，🌟 Star 一下~

另外，在开源代码的同时我们还为大家精心准备了一套课程——《RAG 技术详解与应用实战》（https://www.bilibili.com/video/BV1DrLxz5ELu），课程详细介绍了工业级 `RAG` 的系统设计思路，并手把手教你使用 `LazyLLM` 实现工业级 `RAG` 系统的落地。课程持续中，欢迎大家一起学习，共同进步~

## 附录

### 附录一：mac 安装 Python、Pip 和 Git

1. 先安装 Xcode

![](img/7bf1182ca8a207bd527b2bf816ca1018.png)

并且接受许可协议，然后安装 Xcode 的命令行工具

```bash
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer  # 确保路径正确
sudo xcodebuild -license accept  # 接受许可证协议
xcode-select --install  # 安装 Xcode 命令行工具
```

2. 安装 Homebrew，并通过 Homebrew 安装 Python 和 Pip

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zshrc
source ~/.zshrc
brew install pyenv
pyenv install 3.10.0
```

3. 安装 Python 3.10。注意不能安装 Python 3.13，因为我们依赖的 `spacy` 包不支持 Python 3.13。

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init --path)"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
pyenv global 3.10.0
python3 -m venv lazyllm
source lazyllm/bin/activate
```
