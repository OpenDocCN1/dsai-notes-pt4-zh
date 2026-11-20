
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
