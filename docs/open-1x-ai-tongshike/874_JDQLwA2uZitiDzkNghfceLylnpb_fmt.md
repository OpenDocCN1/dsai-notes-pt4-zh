
# Agently+DeepSeek复现吴恩达老师的Translation Agent

> 来源：[https://datawhaler.feishu.cn/docx/NXYXdA4TPo9QlKx5og2c0KYOnRh](https://datawhaler.feishu.cn/docx/NXYXdA4TPo9QlKx5og2c0KYOnRh)

## Translation Agent效果

```
# 启动工作流
result = workflow.start(storage = {
    &quot;source_lang&quot;: &quot;English&quot;,  # 源语言为英语
    &quot;target_lang&quot;: &quot;中文&quot;,  # 目标语言为中文
    &quot;source_text&quot;: &quot;&quot;&quot;By using an LLM as the heart of the translation engine, this system is highly steerable. For example, by changing the prompts, it is easier using this workflow than a traditional machine translation (MT) system to:
Modify the output&#x27;s style, such as formal/informal.
Specify how to handle idioms and special terms like names, technical terms, and acronyms. For example, including a glossary in the prompt lets you make sure particular terms (such as open source, H100 or GPU) are translated consistently.
Specify specific regional use of the language, or specific dialects, to serve a target audience. For example, Spanish spoken in Latin America is different from Spanish spoken in Spain; French spoken in Canada is different from how it is spoken in France.&quot;&quot;&quot;
})

# 打印执行结果
print(json.dumps(result, indent=4, ensure_ascii=False))  # 将结果格式化为JSON格式并打印，确保中文字符正确显示
```

### 要翻译的内容：

By using an LLM as the heart of the translation engine, this system is highly steerable. For example, by changing the prompts, it is easier using this workflow than a traditional machine translation (MT) system to:

Modify the output's style, such as formal/informal.

Specify how to handle idioms and special terms like names, technical terms, and acronyms. For example, including a glossary in the prompt lets you make sure particular terms (such as open source, H100 or GPU) are translated consistently.

Specify specific regional use of the language, or specific dialects, to serve a target audience. For example, Spanish spoken in Latin America is different from Spanish spoken in Spain; French spoken in Canada is different from how it is spoken in France.

### 模型初翻结果：

通过将大型语言模型（LLM）作为翻译引擎的核心，该系统具有高度可操控性。例如，相较于传统机器翻译（MT）系统，通过修改提示词可更轻松实现以下功能：调整输出风格（如正式/非正式）；指定习语和专业术语（如名称、技术术语、缩写词）的处理方式——通过在提示词中添加术语表，可确保特定术语（如开源、H100或GPU）的翻译一致性；针对目标受众指定特定地区语言使用习惯或方言，如拉丁美洲西班牙语与西班牙本土西班牙语存在差异，加拿大法语与法国法语用法亦不相同。

### 模型给出的改进建议：

1. 将"可操控性"改为"可调控性"更准确，因`steerable`在此上下文中侧重系统输出的可调节性而非物理操控性。

1. "通过修改提示词"建议调整为"通过调整提示指令"，因`prompts`在LLM语境中更常对应"提示指令"的专业表述。

1. "调整输出风格（如正式/非正式）"后应补充原文隐含的主动控制意味，可改为"主动调整输出风格（如正式/非正式）"。

1. "翻译一致性"宜改为"译法一致性"，因术语标准化强调翻译方法而非单纯结果的一致性。

1. "H100或GPU"建议加引号保持术语突出性，改为"`H100`或`GPU`"，与"开源"的引号格式统一。

1. "地区语言使用习惯"易产生歧义，应改为"区域语言变体"以准确对应"regional use of the language"的专业概念。

1. 法语差异描述中"用法亦不相同"建议强化对比，可改为"加拿大法语在发音、词汇等方面与法国法语存在系统性差异"。

1. 原文"to serve a target audience"的能动性未充分体现，建议将"针对目标受众"改为"精准满足目标受众需求"。

1. "西班牙本土西班牙语"存在重复，宜改为"西班牙本土的西班牙语"增强可读性。

1. 最后两个分句建议增加连接词："例如拉丁美洲西班牙语与西班牙本土的西班牙语存在差异，而加拿大法语与法国法语在用法上亦有显著不同"，使逻辑更连贯。

### 模型最终翻译结果：

通过将大型语言模型（LLM）作为翻译引擎的核心，该系统具有高度可调控性。例如，相较于传统机器翻译（MT）系统，通过调整提示指令可更轻松实现以下功能：主动调整输出风格（如正式/非正式）；指定习语和专业术语（如名称、技术术语、缩写词）的处理方式——通过在提示指令中添加术语表，可确保特定术语（如“开源”、“`H100`”或“`GPU`”）的译法一致性；精准满足目标受众需求，指定特定区域语言变体或方言。例如，拉丁美洲西班牙语与西班牙本土的西班牙语存在差异，而加拿大法语在发音、词汇等方面与法国法语存在系统性差异。

## Agnetly简介

`Agently` 是一款开源的 AI 开发框架，专注于帮助开发者快速构建基于大语言模型的 AI 代理（`Agent`）原生应用。以下是关于 `Agently` 的详细介绍：

### 核心功能

+ **快速构建 AI 代理**：开发者可以通过极简的代码快速创建 AI 代理实例，并与之交互。例如，只需几行代码即可创建一个代理实例并设置模型参数。

+ **多模型支持与切换**：支持多种主流的 AI 模型（如 OpenAI、Claude 等），并能够无缝切换，无需修改业务逻辑。

+ **工作流编程**：支持复杂任务的工作流编排，开发者可以定义多个任务步骤并指定执行顺序。

+ **插件化架构**：采用插件化设计，开发者可以通过添加插件轻松扩展 AI 代理的功能，而无需重构整个系统。

+ **流式消息处理**：提供独特的流式消息分块及多下游分发管理方案，能够在接收流式消息时保留模型的高速反馈能力。

### 开发优势

+ **易用性**：提供了简洁直观的接口，使得与 AI 代理的交互变得像调用普通函数一样简单。

+ **灵活性**：框架高度可扩展，开发者可以根据需要定制或更换模型请求方法、记忆管理方式等。

+ **高效性**：通过插件化架构和工作流编程，大大提高了开发效率。

### 应用场景

+ **客服机器人**：可以快速构建并管理具有特定人设和记忆的客服机器人。

+ **角色扮演机器人**：能够创建虚拟角色并与之进行自然语言对话。

+ **复杂数据处理**：支持结构化数据交互，方便处理复杂的数据格式。

### 开发资源

+ **文档丰富**：提供了详细的入门指南、应用开发手册等。

+ **代码示例**：包含多个领域的代码示例，如 SQL 生成器、调查问卷代理等。

+ **社区支持**：拥有活跃的开源社区，开发者可以参与贡献并获取技术支持。

`Agently` 旨在帮助开发者专注于业务逻辑的构建，而不是底层的 AI 模型交互细节。它通过简化开发流程、提供强大的扩展能力和灵活的应用场景，正在重新定义 AI 代理应用的开发方式。

## Translation Agent项目介绍

吴恩达老师的项目地址：https://github.com/andrewyng/translation-agent

Translation Agent 是一个基于大型语言模型（LLM）的翻译工具，它采用反思工作流来提升翻译质量。以下是该项目的简单介绍：

### 核心功能

+ **翻译与反思**：先通过`LLM`将文本从源语言翻译成目标语言，然后让`LLM`对翻译结果进行反思，提出改进建议，最后根据建议优化翻译。

+ **高度可定制**：通过调整提示词，可轻松修改输出风格，如正式/非正式；还能指定如何处理习语、专有名词等特殊术语；也可针对特定地区的语言变体或方言进行翻译，以满足目标受众需求。

### 原理及核心步骤

1. 构建`prompt`让`LLM`将文本从`source_language`翻译为`target_language`；

1. 让`LLM`对翻译结果进行反思，提出改进建议；

1. 利用这些建议来进一步改进翻译结果并输出。

### 核心代码阅读

详见上一篇文章：[LangGraph+DeepSeek复现吴恩达老师的Translation Agent](https://qclxthmzzq.feishu.cn/docx/BW4td3PdUoJ2VvxIc3OcKm4MnQt?from=from_copylink)

## 利用LangGraph复现Translation Agent

### 配置和使用 DeepSeek API

这段 Python 代码的主要功能是配置和使用 DeepSeek API 来获取模型的完成结果。它首先导入了必要的库，包括 `json`、`openai` 和 `Agently`，以及 `os` 模块。然后，它设置了 DeepSeek API 的相关参数，如 API 密钥、基地址和默认模型名称。

```
import json  # 用于处理JSON格式的数据
import openai  # OpenAI的Python库，用于与OpenAI的API进行交互
import Agently  # Agently框架，用于构建和管理AI代理
import os  # 操作系统接口库，用于处理文件和环境变量等

# 设置DeepSeek API的相关参数
deep_seek_api_key = &#x27;sk-21070************494b7e5db&#x27;  # DeepSeek API的密钥，用于身份验证
deep_seek_url = &#x27;https://api.deepseek.com/v1&#x27;  # DeepSeek API的基地址，用于发送API请求
deep_seek_default_model = &#x27;deepseek-reasoner&#x27;  # 默认使用的模型名称，用于指定API请求中使用的模型
```

## 配置Agently框架并创建工作流

这段代码展示了如何使用`Agently`框架配置`Agent`工厂实例和创建工作流：

1. **配置`Agent`工厂实例**：

+ 使用`Agently.AgentFactory()`创建一个`Agent`工厂实例。

+ 通过调用`set_settings`方法设置`Agent`工厂实例，并将默认模型请求客户端设置为`OAIClient`。

1. **创建工作流**：

+ 使用`Agently.Workflow()`创建一个工作流对象。

+ 工作流用于定义和管理`AI`代理的任务流程，可以指定任务的执行顺序和逻辑。

通过这段代码，开发者可以快速配置`Agent`工厂实例和创建工作流，为构建复杂的`AI`代理应用奠定基础。

```
# 将模型请求配置设置到agent工厂，后续工厂创建的agent对象都可以继承这个配置
agent_factory = (
    Agently.AgentFactory()  # 创建一个Agent工厂对象
        .set_settings("current_model", "OAIClient")  # 设置当前使用的模型为OAIClient
        .set_settings("model.OAIClient.url", deep_seek_url)  # 设置OAIClient模型的API请求地址
        .set_settings("model.OAIClient.auth", { "api_key": deep_seek_api_key })  # 设置OAIClient模型的身份验证信息，使用API密钥
        .set_settings("model.OAIClient.options", { "model": deep_seek_default_model })  # 设置OAIClient模型的选项，指定默认使用的模型名称
)

# 创建工作流
workflow = Agently.Workflow()  # 创建一个工作流对象，用于定义和管理AI代理的任务流程
```

## 工作流的第一步---初翻

这段代码定义了一个名为`initial_translation`的处理节点，用于执行首次翻译任务。它展示了如何使用`Agently`框架创建工作流中的一个处理步骤，包括：

1. **获取输入数据**：从存储中获取源语言、目标语言和源文本。

1. **创建翻译`agent`**：使用`Agent`工厂实例创建一个新的翻译`agent`。

1. **设置`agent`角色**：通过设置`system`信息定义`agent`的角色和任务。

1. **执行翻译任务**：向`agent`发送翻译任务请求并获取结果。

1. **保存结果和`agent`**：将翻译结果和`agent`实例保存到存储中，以便后续节点使用。

1. **返回结果**：返回当前阶段的名称和翻译结果。

通过这种方式，开发者可以轻松地在工作流中定义和管理复杂的任务处理步骤，利用`Agently`框架的强大功能来构建智能的`AI`代理应用。



# 定义关键处理节点

## 首次翻译

```
@workflow.chunk()  # 使用workflow的chunk装饰器定义一个处理节点
def initial_translation(inputs, storage):
    """
    initial_translation是一个处理节点，用于执行首次翻译任务。
    
    Args:
        inputs: 输入数据，通常包含任务所需的原始数据。
        storage: 存储对象，用于在工作流的不同节点之间共享数据。
    
    Returns:
        一个包含当前阶段信息和结果的字典。
    """
    # 从存储中获取源语言、目标语言和源文本
    source_lang = storage.get("source_lang")  # 源语言
    target_lang = storage.get("target_lang")  # 目标语言
    source_text = storage.get("source_text")  # 需要翻译的源文本
    # 创建一个翻译agent来执行任务
    translate_agent = agent_factory.create_agent()  # 使用Agent工厂实例创建一个新的翻译agent
    # 给翻译agent设置system信息，定义agent的角色和任务
    translate_agent.set_agent_prompt("role", f"You are an expert linguist, specializing in translation from {source_lang} to {target_lang}.")
    # 向翻译agent发起翻译任务请求
    translation_1 = (
        translate_agent
        .input(f"""This is an {source_lang} to {target_lang} translation, please provide the {target_lang} translation for this text. \
Do not provide any explanations or text apart from the translation.
{source_lang}: {source_text}

{target_lang}:""")  # 输入翻译任务的详细说明和源文本
        .start()  # 启动翻译任务并获取结果
    )
    # 保存翻译结果到存储中，以便后续节点使用
    storage.set("translation_1", translation_1)
    # 保存翻译agent实例，以便后续可能需要复用
    storage.set("translate_agent", translate_agent)
    # 返回当前阶段的信息和结果
    return {"stage": "initial translation",  # 当前阶段的名称
            "result": translation_1  # 翻译结果}
```

## 工作流的第二步---reflect

这段代码定义了一个名为 `reflect_on_translation` 的处理节点，用于对初始翻译结果进行反思和优化。它展示了如何在 `Agently` 框架中创建工作流中的一个处理步骤，包括：

1. **获取输入数据**：从存储中获取源语言、目标语言、源文本、国家信息和初始翻译结果。
1. **创建反思agent**：使用 `Agent` 工厂实例创建一个新的反思agent。
1. **设置agent角色**：通过设置 `system` 信息定义agent的角色和任务，使其专注于翻译优化。
1. **动态调整规则**：根据国家信息决定是否添加额外的规则，以确保翻译结果符合特定地区的语言风格。
1. **执行反思任务**：向agent发送反思任务请求，提供源文本和初始翻译，并获取改进建议。
1. **保存结果**：将反思结果保存到存储中，以便后续节点使用。
1. **返回结果**：返回当前阶段的名称和反思结果。

### 反思优化

```
@workflow.chunk()  # 使用workflow的chunk装饰器定义一个处理节点
def reflect_on_translation(inputs, storage):
    """
    reflect_on_translation是一个处理节点，用于对初始翻译结果进行反思和优化。
    
    Args:
        inputs: 输入数据，通常包含任务所需的原始数据。
        storage: 存储对象，用于在工作流的不同节点之间共享数据。
    
    Returns:
        一个包含当前阶段信息和反思结果的字典。
    """
    # 从存储中获取源语言、目标语言、源文本、国家信息和初始翻译结果
    source_lang = storage.get("source_lang")  # 源语言
    target_lang = storage.get("target_lang")  # 目标语言
    source_text = storage.get("source_text")  # 源文本
    country = storage.get("country", "")  # 国家信息（可选）
    translation_1 = storage.get("translation_1")  # 初始翻译结果

    # 创建一个反思agent来执行任务
    reflection_agent = agent_factory.create_agent()  # 使用Agent工厂实例创建一个新的反思agent

    # 给反思agent设置system信息，定义agent的角色和任务
    reflection_agent.set_agent_prompt(
        "role",
        f"You are an expert linguist specializing in translation from {source_lang} to {target_lang}. \
You will be provided with a source text and its translation and your goal is to improve the translation."
    )

    # 根据国家信息决定是否添加额外的规则
    additional_rule = (
        f"The final style and tone of the translation should match the style of {target_lang} colloquially spoken in {country}."
        if country != ""
        else ""
    )

    # 向反思agent发起反思任务请求
    reflection = (
        reflection_agent
            .input(
f"""Your task is to carefully read a source text and a translation from {source_lang} to {target_lang}, and then give constructive criticism and helpful suggestions to improve the translation. \
{additional_rule}

The source text and initial translation, delimited by XML tags <SOURCE_TEXT></SOURCE_TEXT> and <TRANSLATION></TRANSLATION>, are as follows:

<SOURCE_TEXT>
{source_text}
</SOURCE_TEXT>

<TRANSLATION>
{translation_1}
</TRANSLATION>

When writing suggestions, pay attention to whether there are ways to improve the translation's \n\
(i) accuracy (by correcting errors of addition, mistranslation, omission, or untranslated text),\n\
(ii) fluency (by applying {target_lang} grammar, spelling and punctuation rules, and ensuring there are no unnecessary repetitions),\n\
(iii) style (by ensuring the translations reflect the style of the source text and takes into account any cultural context),\n\
(iv) terminology (by ensuring terminology use is consistent and reflects the source text domain; and by only ensuring you use equivalent idioms {target_lang}).\n\

Write a list of specific, helpful and constructive suggestions for improving the translation.
Each suggestion should address one specific part of the translation.
Output only the suggestions and nothing else."""
            )  # 输入反思任务的详细说明、源文本和初始翻译
            .start()  # 启动反思任务并获取结果
    )

    # 保存反思结果到存储中，以便后续节点使用
    storage.set("reflection", reflection)
    
    # 返回当前阶段的信息和反思结果
    return {
        "stage": "reflection",  # 当前阶段的名称
        "result": reflection  # 反思结果
    }
```

## 工作流第三步---改进

这段代码定义了一个名为 `improve_translation` 的处理节点，用于根据反思结果进行二次翻译，以优化初始翻译结果。它展示了如何在 `Agently` 框架中创建工作流中的一个处理步骤，包括：

1. **获取输入数据**：从存储中获取源语言、目标语言、源文本、初始翻译和反思结果。
1. **复用翻译agent**：使用之前保存的翻译agent，避免重新创建agent，提高效率。
1. **执行二次翻译任务**：向agent发送二次翻译任务请求，提供源文本、初始翻译和反思结果，并获取优化后的翻译。
1. **保存结果**：将二次翻译结果保存到存储中，以便后续节点使用。
1. **返回结果**：返回当前阶段的名称和二次翻译结果。

通过这种方式，开发者可以轻松地在工作流中定义和管理复杂的任务处理步骤，利用 `Agently` 框架的强大功能来构建智能的AI代理应用。这段代码特别展示了如何通过反思和优化来提升翻译质量，体现了 `Agently` 在处理多步骤复杂任务中的优势。



## 二次翻译

```python
@workflow.chunk()
def improve_translation(inputs, storage):
    """
    improve_translation是一个处理节点，用于根据反思结果进行二次翻译，以优化初始翻译。
    
    Args:
        inputs: 输入数据，通常包含任务所需的原始数据。
        storage: 存储对象，用于在工作流的不同节点之间共享数据。
    
    Returns:
        一个包含当前阶段信息和二次翻译结果的字典。
    """
    # 从存储中获取源语言、目标语言、源文本、初始翻译和反思结果
    source_lang = storage.get("source_lang")  # 源语言
    target_lang = storage.get("target_lang")  # 目标语言
    source_text = storage.get("source_text")  # 源文本
    translation_1 = storage.get("translation_1")  # 初始翻译结果
    reflection = storage.get("reflection")  # 反思结果

    # 使用之前保存的翻译agent，避免重新创建agent，提高效率
    translate_agent = storage.get("translate_agent")  # 获取之前保存的翻译agent

    # 向翻译agent发起二次翻译任务请求，利用反思结果优化翻译
    translation_2 = (
        translate_agent
            .input(
f"""Your task is to carefully read, then edit, a translation from {source_lang} to {target_lang}, taking into
account a list of expert suggestions and constructive criticisms.

The source text, the initial translation, and the expert linguist suggestions are delimited by XML tags <SOURCE_TEXT></SOURCE_TEXT>, <TRANSLATION></TRANSLATION> and <EXPERT_SUGGESTIONS></EXPERT_SUGGESTIONS> \
as follows:

<SOURCE_TEXT>
{source_text}
</SOURCE_TEXT>

<TRANSLATION>
{translation_1}
</TRANSLATION>

<EXPERT_SUGGESTIONS>
{reflection}
</EXPERT_SUGGESTIONS>

Please take into account the expert suggestions when editing the translation. Edit the translation by ensuring:

(i) accuracy (by correcting errors of addition, mistranslation, omission, or untranslated text),
(ii) fluency (by applying {target_lang} grammar, spelling and punctuation rules and ensuring there are no unnecessary repetitions), \
(iii) style (by ensuring the translations reflect the style of the source text)
(iv) terminology (inappropriate for context, inconsistent use), or
(v) other errors.

Output only the new translation and nothing else."""
            )  # 输入二次翻译任务的详细说明、源文本、初始翻译和反思结果
            .start()  # 启动二次翻译任务并获取优化后的翻译结果
    )

    # 保存二次翻译结果到存储中，以便后续节点使用
    storage.set("translation_2", translation_2)
    
    # 返回当前阶段的信息和二次翻译结果
    return {
        "stage": "improve translation",  # 当前阶段的名称
        "result": translation_2  # 二次翻译结果
    }
```

## 连接工作流节点

这段代码展示了如何在Agently框架中连接工作流的各个处理节点，并添加过程输出优化。具体包括：

1. 连接工作流节点：通过`connect_to`方法将工作流中的各个处理节点按顺序连接起来，确保任务按指定顺序执行。

1. 定义输出优化类：使用`@workflow.chunk_class()`装饰器定义一个输出优化类`output_stage_result`，用于在每个处理阶段输出结果。

1. 设置输出连接：为每个处理节点设置输出连接，使其在执行后调用输出优化类，打印当前阶段的名称和结果。

通过这种方式，开发者可以轻松地在工作流中添加过程输出，实时监控每个阶段的执行结果，提高应用的可调试性和用户体验。这种机制特别适合需要多步骤处理的复杂任务，确保每个阶段的输出都能被及时查看和分析。

```python
# 连接工作块
# 将工作流中的各个处理节点按顺序连接起来，形成完整的工作流程
(
    workflow
        .connect_to("initial_translation")  # 连接到初始翻译节点
        .connect_to("reflect_on_translation")  # 连接到反思优化节点
        .connect_to("improve_translation")  # 连接到二次翻译节点
        .connect_to("end")  # 连接到结束节点
)

# 添加过程输出优化
@workflow.chunk_class()  # 使用workflow的chunk_class装饰器定义一个输出优化类
def output_stage_result(inputs, storage):
    """
    output_stage_result是一个输出优化类，用于在每个处理阶段输出结果。
    
    Args:
        inputs: 输入数据，包含当前阶段的信息和结果。
        storage: 存储对象，用于在工作流的不同节点之间共享数据。
    
    Returns:
        无返回值，仅用于输出当前阶段的结果。
    """
    # 打印当前阶段的名称和结果
    print(f"[{ inputs['default']['stage'] }]:\n", inputs["default"]["result"])
    return

# 设置各个处理节点的输出连接
(
    workflow.chunks["initial_translation"]  # 获取初始翻译节点
        .connect_to("@output_stage_result")  # 连接到输出优化类，输出初始翻译结果
        .connect_to("reflect_on_translation.wait")  # 连接到反思优化节点的等待状态
)
(
    workflow.chunks["reflect_on_translation"]  # 获取反思优化节点
        .connect_to("@output_stage_result")  # 连接到输出优化类，输出反思结果
        .connect_to("improve_translation.wait")  # 连接到二次翻译节点的等待状态
)
(
    workflow.chunks["improve_translation"]  # 获取二次翻译节点
        .connect_to("@output_stage_result")  # 连接到输出优化类，输出二次翻译结果
)
```

## 任务执行

这段代码展示了如何启动一个定义好的工作流并打印执行结果。具体包括：

1. 启动工作流：通过`workflow.start()`方法启动工作流，并传递初始存储数据，包括源语言、目标语言和源文本。

1. 打印结果：使用`json.dumps()`方法将执行结果格式化为JSON格式并打印，确保中文字符正确显示。

通过这种方式，开发者可以轻松地启动一个复杂的工作流，并实时查看每个阶段的执行结果。这种机制特别适合需要多步骤处理的复杂任务，确保每个阶段的输出都能被及时查看和分析。

```python
# 启动工作流
result = workflow.start(storage = {
    "source_lang": "English",  # 源语言为英语
    "target_lang": "中文",  # 目标语言为中文
    "source_text": """By using an LLM as the heart of the translation engine, this system is highly steerable. For example, by changing the prompts, it is easier using this workflow than a traditional machine translation (MT) system to:
Modify the output's style, such as formal/informal.
Specify how to handle idioms and special terms like names, technical terms, and acronyms. For example, including a glossary in the prompt lets you make sure particular terms (such as open source, H100 or GPU) are translated consistently.
Specify specific regional use of the language, or specific dialects, to serve a target audience. For example, Spanish spoken in Latin America is different from Spanish spoken in Spain; French spoken in Canada is different from how it is spoken in France."""
})

# 打印执行结果
print(json.dumps(result, indent=4, ensure_ascii=False))  # 将结果格式化为JSON格式并打印，确保中文字符正确显示
```

## 输出结果

### [initial translation]

通过将大型语言模型（LLM）作为翻译引擎的核心，该系统具有高度可操控性。例如，相较于传统机器翻译（MT）系统，通过修改提示词可更轻松实现以下功能：调整输出风格（如正式/非正式）；指定习语和专业术语（如名称、技术术语、缩写词）的处理方式——通过在提示词中添加术语表，可确保特定术语（如开源、H100或GPU）的翻译一致性；针对目标受众指定特定地区语言使用习惯或方言，如拉丁美洲西班牙语与西班牙本土西班牙语存在差异，加拿大法语与法国法语用法亦不相同。

### [reflection]

1.  将"可操控性"改为"可调控性"更准确，因"steerable"在此上下文中侧重系统输出的可调节性而非物理操控性。

1.  "通过修改提示词"建议调整为"通过调整提示指令"，因"prompts"在LLM语境中更常对应"提示指令"的专业表述。

1.  "调整输出风格（如正式/非正式）"后应补充原文隐含的主动控制意味，可改为"主动调整输出风格（如正式/非正式）"。

1.  "翻译一致性"宜改为"译法一致性"，因术语标准化强调翻译方法而非单纯结果的一致性。

1.  "H100或GPU"建议加引号保持术语突出性，改为"‘H100’或‘GPU’"，与"开源"的引号格式统一。

1.  "地区语言使用习惯"易产生歧义，应改为"区域语言变体"以准确对应"regional use of the language"的专业概念。

1.  法语差异描述中"用法亦不相同"建议强化对比，可改为"加拿大法语在发音、词汇等方面与法国法语存在系统性差异"。

1.  原文"to serve a target audience"的能动性未充分体现，建议将"针对目标受众"改为"精准满足目标受众需求"。

1.  "西班牙本土西班牙语"存在重复，宜改为"西班牙本土的西班牙语"增强可读性。

1.  最后两个分句建议增加连接词："例如拉丁美洲西班牙语与西班牙本土的西班牙语存在差异，而加拿大法语与法国法语在用法上亦有显著不同"，使逻辑更连贯。

### [improve translation]

通过将大型语言模型（LLM）作为翻译引擎的核心，该系统具有高度可调控性。例如，相较于传统机器翻译（MT）系统，通过调整提示指令可更轻松实现以下功能：主动调整输出风格（如正式/非正式）；指定习语和专业术语（如名称、技术术语、缩写词）的处理方式——通过在提示指令中添加术语表，可确保特定术语（如“开源”、“H100”或“GPU”）的译法一致性；精准满足目标受众需求，指定特定区域语言变体或方言。例如，拉丁美洲西班牙语与西班牙本土的西班牙语存在差异，而加拿大法语在发音、词汇等方面与法国法语存在系统性差异。
