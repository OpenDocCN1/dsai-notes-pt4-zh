# LangGraph+DeepSeek复现吴恩达老师的Translation Agent

> 来源：[https://datawhaler.feishu.cn/docx/BW4td3PdUoJ2VvxIc3OcKm4MnQt](https://datawhaler.feishu.cn/docx/BW4td3PdUoJ2VvxIc3OcKm4MnQt)

## Translation Agent效果

```
#执行任务
# 调用编译后的工作流，传入初始状态字典
result = app.invoke({
    &quot;source_lang&quot;: &quot;English&quot;, # 源语言为英语
    &quot;target_lang&quot;: &quot;中文&quot;,    # 目标语言为中文
    &quot;source_text&quot;: &quot;&quot;&quot;By using an LLM as the heart of the translation engine, this system is highly steerable. For example, by changing the prompts, it is easier using this workflow than a traditional machine translation (MT) system to:

Modify the output&#x27;s style, such as formal/informal.
Specify how to handle idioms and special terms like names, technical terms, and acronyms. For example, including a glossary in the prompt lets you make sure particular terms (such as open source, H100 or GPU) are translated consistently.
Specify specific regional use of the language, or specific dialects, to serve a target audience. For example, Spanish spoken in Latin America is different from Spanish spoken in Spain; French spoken in Canada is different from how it is spoken in France.&quot;&quot;&quot;
})
```

### 要翻译的内容：

By using an LLM as the heart of the translation engine, this system is highly steerable. For example, by changing the prompts, it is easier using this workflow than a traditional machine translation (MT) system to:

Modify the output's style, such as formal/informal.

Specify how to handle idioms and special terms like names, technical terms, and acronyms. For example, including a glossary in the prompt lets you make sure particular terms (such as open source, H100 or GPU) are translated consistently.

Specify specific regional use of the language, or specific dialects, to serve a target audience. For example, Spanish spoken in Latin America is different from Spanish spoken in Spain; French spoken in Canada is different from how it is spoken in France.

### 模型初翻结果：

通过将LLM作为翻译引擎的核心，该系统具备高度可调控性。例如相较于传统机器翻译（MT）系统，通过修改提示词可以更便捷地实现以下功能：调整输出文本风格（正式/非正式）；指定成语和专业术语（如名称、技术术语、缩略语）的处理方式——通过在提示词中添加术语表，可确保特定词汇（如开源、H100或GPU）的翻译一致性；根据目标受众需求指定语言的地域变体或方言，例如拉丁美洲西班牙语与西班牙本土西班牙语存在差异，加拿大法语与法国本土法语亦有不同。

### 模型给出的改进建议：

1.  将"可调控性"改为"可操控性"更符合中文技术语境，原词"steerable"在翻译系统中更常对应"可操控"概念。

1.  "提示词"建议统一译为"提示语"，因在大型语言模型领域"prompt"的规范译法为"提示语"，且后文重复出现需保持术语一致性。

1.  "调整输出文本风格（正式/非正式）"中的括号应改为中文全角括号「」更符合中文排版规范，建议修改为「（正式/非正式）」→「（正式／非正式）」并添加顿号。

1.  "名称"建议改为"专有名词"，因原文"names"在翻译语境中特指需要特殊处理的专有名词翻译，原译容易产生歧义。

1.  "H100或GPU"中的"或"应改为顿号，中文列举多项时应用顿号分隔，建议修改为"如开源、H100、GPU"。

1.  "术语表"建议改为"词汇表"，因"glossary"在本地化行业标准译法为"词汇表"，且与后文"特定词汇"形成更好的术语呼应。

1.  "地域变体"调整为"地域性变体"更符合中文表达习惯，原译"地域变体"在语言学领域不够规范。

1.  "根据目标受众需求"建议补充为"根据目标受众的具体需求"，添加"具体"二字更准确对应原文"specific"的双重含义。

1.  "存在差异"与"亦有不同"形成重复，建议统一为"有所不同"，既保持译文流畅度又符合中文忌重复的表达习惯。

1.  最后一句建议增加连接词："例如，拉丁美洲使用的西班牙语与西班牙本土的西班牙语有所不同，而加拿大法语与法国本土法语也存在差异"，通过添加"而"字增强句间逻辑关系。

1.  "西班牙本土西班牙语"存在语义重复，建议简化为"西班牙的西班牙语"，符合中文简洁表达原则。

1.  专业术语处理部分建议调整语序："指定如何处理成语及特殊术语（包括名称、技术术语、缩略语等）"，通过添加"包括"使列举关系更清晰。

### 模型最终翻译结果：

通过将LLM作为翻译引擎的核心，该系统具备高度可操控性。例如相较于传统机器翻译（MT）系统，通过修改提示语可以更便捷地实现以下功能：调整输出文本风格（正式／非正式）；指定如何处理成语及特殊术语（包括专有名词、技术术语、缩略语等）——通过在提示语中添加词汇表，可确保特定词汇（如开源、H100、GPU）的翻译一致性；根据目标受众的具体需求指定语言的地域性变体或方言，例如拉丁美洲使用的西班牙语与西班牙的西班牙语有所不同，而加拿大法语与法国本土法语亦存在差异。

## LangGraph简介

LangGraph是一个用于构建有状态、多角色应用程序的库，特别适用于与大型语言模型（LLMs）一起使用。它扩展了LangChain表达式语言，允许以循环方式协调多个链或角色，其灵感来源于Pregel和Apache Beam，并借鉴了NetworkX的接口。

### 核心功能

+   **支持循环流**：LangGraph允许定义包含循环的流程，这对于大多数代理架构至关重要，而基于DAG的解决方案则不支持循环流。

+   **状态管理**：提供状态管理功能，允许代理在多个步骤之间存储和检索信息，这对于构建需要跟踪对话状态或游戏状态的应用程序至关重要。

+   **多角色支持**：支持多个代理相互交互，以实现更复杂的工作流程，适合构建需要协作或竞争的代理应用程序。

+   **可扩展性**：可以扩展到生产环境，以支持大规模应用程序。

+   **高级接口和低级API**：既提供用于创建常见类型代理的高级接口，也提供用于自定义和组合工作流程的低级API。

### 主要特点

+   **图结构**：使用图结构来表示应用程序的工作流程，其中节点代表不同的操作或步骤，边定义了这些步骤之间的执行顺序和条件。

+   **细粒度控制**：为用户提供对应用程序的流程和状态的细粒度控制，这对于创建可靠、可预测的代理至关重要。

+   **内置持久性**：包括内置的持久性功能，使代理能够记住过去的交互并使用这些信息来指导未来的决策。

### 应用场景

+   **对话代理和聊天机器人**：构建能够与用户进行自然语言对话的代理，例如客户服务代理或虚拟助手。

+   **RAG应用**：在语义检索相关文档后，对其质量进行评估，若文档质量差，可重写检索问题并再次交给检索器以获得更精确结果。

+   **多智能体系统**：构建由多个AI智能体构成的系统，通过相互关联与协作共同完成任务。

+   **业务流程自动化**：在企业中自动化复杂的业务流程，如订单处理、库存管理和供应链协调。

+   **个性化推荐系统**：根据用户的兴趣、历史行为和上下文信息提供相关的建议。

## Translation Agent项目介绍

吴恩达老师的项目地址：https://github.com/andrewyng/translation-agent

Translation Agent 是一个基于大型语言模型（LLM）的翻译工具，它采用反思工作流来提升翻译质量。以下是该项目的简单介绍：

### 核心功能

+   **翻译与反思** ：先通过LLM将文本从源语言翻译成目标语言，然后让LLM对翻译结果进行反思，提出改进建议，最后根据建议优化翻译。

+   **高度可定制** ：通过调整提示词，可轻松修改输出风格，如正式/非正式；还能指定如何处理习语、专有名词等特殊术语；也可针对特定地区的语言变体或方言进行翻译，以满足目标受众需求。

### 原理及核心步骤

1.  构建prompt让LLM将文本从source_language翻译为target_language；

1.  让LLM对翻译结果进行反思，提出改进建议；

1.  利用这些建议来进一步改进翻译结果并输出。

### 核心代码阅读

https://github.com/andrewyng/translation-agent/blob/main/src/translation_agent/utils.py

#### one_chunk_initial_translation

该函数是一个简单的翻译工具，它利用大型语言模型（LLM）将一段文本(source_text)从一种语言(source_lang)翻译成另一种语言(target_lang)。函数接收source_lang、 target_lang和source_text作为输入，构造一个system_message 来设定 LLM 的角色，然后创建一个明确的translation_prompt ，要求 LLM 仅提供翻译结果而不添加任何额外内容。最后，函数通过调用 get_completion 函数来获取 LLM 的翻译输出并返回。

```
def one_chunk_initial_translation(
    source_lang: str, target_lang: str, source_text: str
) -&gt; str:
    &quot;&quot;&quot;
    Translate the entire text as one chunk using an LLM.

    Args:
        source_lang (str): The source language of the text.
        target_lang (str): The target language for translation.
        source_text (str): The text to be translated.

    Returns:
        str: The translated text.
    &quot;&quot;&quot;

    system_message = f&quot;You are an expert linguist, specializing in translation from {source_lang} to {target_lang}.&quot;

    translation_prompt = f&quot;&quot;&quot;This is an {source_lang} to {target_lang} translation, please provide the {target_lang} translation for this text. \
Do not provide any explanations or text apart from the translation.
{source_lang}: {source_text}

{target_lang}:&quot;&quot;&quot;

    translation = get_completion(translation_prompt, system_message=system_message)

    return translation
```

#### one_chunk_reflect_on_translation

该函数利用大型语言模型（LLM）对一段文本的翻译进行反思和评估，以提高翻译质量。函数接收源语言（source_lang）、目标语言（target_lang）、原文本（source_text）、初始翻译（translation_1）以及可选的国家/地区参数（country），构造一个system_message 来设定 LLM 的角色为翻译专家，然后根据是否提供国家/地区信息来生成相应的反思和建议。reflection_prompt 中明确要求 LLM 从准确性、流畅性、风格和术语使用等方面对翻译进行评估，并提出具体的改进建议。最后，函数通过调用 get_completion 函数获取 LLM 的反思结果并返回。该函数适用于需要对翻译结果进行优化的场景，尤其是当需要针对特定地区语言变体进行翻译优化时。

```
def one_chunk_reflect_on_translation(
    source_lang: str,
    target_lang: str,
    source_text: str,
    translation_1: str,
    country: str = &quot;&quot;,
) -&gt; str:
    &quot;&quot;&quot;
    Use an LLM to reflect on the translation, treating the entire text as one chunk.

    Args:
        source_lang (str): The source language of the text.
        target_lang (str): The target language of the translation.
        source_text (str): The original text in the source language.
        translation_1 (str): The initial translation of the source text.
        country (str): Country specified for the target language.

    Returns:
        str: The LLM&#x27;s reflection on the translation, providing constructive criticism and suggestions for improvement.
    &quot;&quot;&quot;

    system_message = f&quot;You are an expert linguist specializing in translation from {source_lang} to {target_lang}. \
You will be provided with a source text and its translation and your goal is to improve the translation.&quot;

    if country != &quot;&quot;:
        reflection_prompt = f&quot;&quot;&quot;Your task is to carefully read a source text and a translation from {source_lang} to {target_lang}, and then give constructive criticism and helpful suggestions to improve the translation. \
The final style and tone of the translation should match the style of {target_lang} colloquially spoken in {country}.

The source text and initial translation, delimited by XML tags &lt;SOURCE_TEXT&gt;&lt;/SOURCE_TEXT&gt; and &lt;TRANSLATION&gt;&lt;/TRANSLATION&gt;, are as follows:

&lt;SOURCE_TEXT&gt;
{source_text}
&lt;/SOURCE_TEXT&gt;

&lt;TRANSLATION&gt;
{translation_1}
&lt;/TRANSLATION&gt;

When writing suggestions, pay attention to whether there are ways to improve the translation&#x27;s \n\
(i) accuracy (by correcting errors of addition, mistranslation, omission, or untranslated text),\n\
(ii) fluency (by applying {target_lang} grammar, spelling and punctuation rules, and ensuring there are no unnecessary repetitions),\n\
(iii) style (by ensuring the translations reflect the style of the source text and take into account any cultural context),\n\
(iv) terminology (by ensuring terminology use is consistent and reflects the source text domain; and by only ensuring you use equivalent idioms {target_lang}).\n\

Write a list of specific, helpful and constructive suggestions for improving the translation.
Each suggestion should address one specific part of the translation.
Output only the suggestions and nothing else.&quot;&quot;&quot;

    else:
        reflection_prompt = f&quot;&quot;&quot;Your task is to carefully read a source text and a translation from {source_lang} to {target_lang}, and then give constructive criticisms and helpful suggestions to improve the translation. \

The source text and initial translation, delimited by XML tags &lt;SOURCE_TEXT&gt;&lt;/SOURCE_TEXT&gt; and &lt;TRANSLATION&gt;&lt;/TRANSLATION&gt;, are as follows:

&lt;SOURCE_TEXT&gt;
{source_text}
&lt;/SOURCE_TEXT&gt;

&lt;TRANSLATION&gt;
{translation_1}
&lt;/TRANSLATION&gt;

When writing suggestions, pay attention to whether there are ways to improve the translation&#x27;s \n\
(i) accuracy (by correcting errors of addition, mistranslation, omission, or untranslated text),\n\
(ii) fluency (by applying {target_lang} grammar, spelling and punctuation rules, and ensuring there are no unnecessary repetitions),\n\
(iii) style (by ensuring the translations reflect the style of the source text and take into account any cultural context),\n\
(iv) terminology (by ensuring terminology use is consistent and reflects the source text domain; and by only ensuring you use equivalent idioms {target_lang}).\n\

Write a list of specific, helpful and constructive suggestions for improving the translation.
Each suggestion should address one specific part of the translation.
Output only the suggestions and nothing else.&quot;&quot;&quot;

    reflection = get_completion(reflection_prompt, system_message=system_message)
    return reflection
```

#### one_chunk_improve_translation

该函数利用大型语言模型（LLM）根据reflection种给出的建议对source_text文本的翻译（translation_1）进行改进。函数接收source_lang、target_lang、source_text、translation_1、reflection作为输入，构造一个system_message 来设定 LLM 的角色为翻译专家，然后创建一个详细的prompt ，要求 LLM 根据专家建议对翻译进行编辑，确保翻译的准确性、流畅性、风格和术语使用等方面得到优化。最后，函数通过调用 get_completion 函数获取改进后的翻译结果并返回。该函数适用于需要对翻译结果进行精细化调整的场景，尤其是当有具体的改进建议时，能够有效提升翻译质量。

```
def one_chunk_improve_translation(
    source_lang: str,
    target_lang: str,
    source_text: str,
    translation_1: str,
    reflection: str,
) -&gt; str:
    &quot;&quot;&quot;
    Use the reflection to improve the translation, treating the entire text as one chunk.

    Args:
        source_lang (str): The source language of the text.
        target_lang (str): The target language for the translation.
        source_text (str): The original text in the source language.
        translation_1 (str): The initial translation of the source text.
        reflection (str): Expert suggestions and constructive criticism for improving the translation.

    Returns:
        str: The improved translation based on the expert suggestions.
    &quot;&quot;&quot;

    system_message = f&quot;You are an expert linguist, specializing in translation editing from {source_lang} to {target_lang}.&quot;

    prompt = f&quot;&quot;&quot;Your task is to carefully read, then edit, a translation from {source_lang} to {target_lang}, taking into
account a list of expert suggestions and constructive criticisms.

The source text, the initial translation, and the expert linguist suggestions are delimited by XML tags &lt;SOURCE_TEXT&gt;&lt;/SOURCE_TEXT&gt;, &lt;TRANSLATION&gt;&lt;/TRANSLATION&gt; and &lt;EXPERT_SUGGESTIONS&gt;&lt;/EXPERT_SUGGESTIONS&gt; \
as follows:

&lt;SOURCE_TEXT&gt;
{source_text}
&lt;/SOURCE_TEXT&gt;

&lt;TRANSLATION&gt;
{translation_1}
&lt;/TRANSLATION&gt;

&lt;EXPERT_SUGGESTIONS&gt;
{reflection}
&lt;/EXPERT_SUGGESTIONS&gt;

Please take into account the expert suggestions when editing the translation. Edit the translation by ensuring:

(i) accuracy (by correcting errors of addition, mistranslation, omission, or untranslated text),
(ii) fluency (by applying {target_lang} grammar, spelling and punctuation rules and ensuring there are no unnecessary repetitions), \
(iii) style (by ensuring the translations reflect the style of the source text)
(iv) terminology (inappropriate for context, inconsistent use), or
(v) other errors.

Output only the new translation and nothing else.&quot;&quot;&quot;

    translation_2 = get_completion(prompt, system_message)

    return translation_2
```

## 利用LangGraph复现Translation Agent

### 配置和使用 DeepSeek API

这段 Python 代码的主要功能是配置和使用 DeepSeek API 来获取模型的完成结果。它首先导入了必要的库，包括 json、openai 和 langgraph.graph 中的 StateGraph、START、END，以及 os 模块。然后，它设置了 DeepSeek API 的相关参数，如 API 密钥、基地址和默认模型名称。

代码的核心部分是一个名为 get_completion 的函数，它接受提示文本、系统消息、模型名称、温度参数和 JSON 模式标志作为输入参数。这个函数使用 OpenAI 的 Python 客户端库创建了一个客户端实例，并通过该实例向 DeepSeek API 发送请求。

这段代码为开发者提供了一个方便的接口，可以快速地向 DeepSeek API 发送请求并获取模型的完成结果，适用于需要与 DeepSeek API 集成的自然语言处理应用。

```
import json
import openai
from langgraph.graph import StateGraph, START, END
import os

# 设置 DeepSeek API 的相关参数
deep_seek_api_key = &#x27;sk-21070************494b7e5db&#x27;  # API 密钥
deep_seek_url = &#x27;https://api.deepseek.com/v1&#x27;  # API 请求的基地址
deep_seek_default_model = &#x27;deepseek-reasoner&#x27;  # 默认使用的模型名称

# 使用 OpenAI 的 Python 客户端库创建一个客户端实例，用于与 DeepSeek API 进行交互
client = openai.OpenAI(
    api_key=deep_seek_api_key,  # 设置 API 密钥
    base_url=deep_seek_url  # 设置 API 请求的基地址
)
default_model = deep_seek_default_model  # 设置默认模型

def get_completion(
    prompt: str,  # 提示文本
    system_message: str = &quot;You are a helpful assistant.&quot;,  # 系统消息，默认值为 &quot;You are a helpful assistant.&quot;
    model: str = default_model,  # 使用的模型，默认为 deep_seek_default_model
    temperature: float = 0.3,  # 生成文本的随机程度，值越低越确定
    json_mode: bool = False,  # 是否以 JSON 模式返回结果，默认为 False
):
    &quot;&quot;&quot;
    向 DeepSeek API 发送请求并获取完成结果。

    参数:
        prompt (str): 提示文本，用于引导模型生成回答。
        system_message (str): 系统消息，用于设定模型的行为。
        model (str): 使用的模型名称。
        temperature (float): 控制生成文本的随机程度。
        json_mode (bool): 是否以 JSON 格式返回结果。

    返回:
        str: 模型生成的回答内容。
    &quot;&quot;&quot;
    # 调用 OpenAI 客户端的 chat.completions.create 方法，发送请求
    response = client.chat.completions.create(
        model=model,  # 指定使用的模型
        temperature=temperature,  # 设置温度参数
        top_p=1,  # 核采样参数，控制生成文本的多样性
        messages=[  # 设置对话消息列表
            {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: system_message},  # 系统消息
            {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: prompt},  # 用户消息
        ],
    )
    # 返回模型生成的回答内容
    return response.choices[0].message.content
```

### 创建工作流对象workflow

这段代码定义了一个 State 类，使用 TypedDict 来描述翻译工作流中传递的信息结构，包括源语言、目标语言、源文本、国家/地区（可选）、初始翻译结果（可选）、反思建议（可选）和改进后的翻译结果（可选）。然后，代码创建了一个 StateGraph 实例，传入 State 类，用于管理翻译工作流的状态和流程。这为构建一个结构化、可追踪的翻译工作流提供了基础，使得开发者能够方便地在工作流中传递和管理翻译相关的状态信息。

```
from typing import TypedDict, Optional

# 定义一个名为 State 的 TypedDict 类，用于描述翻译工作流中传递的信息结构
class State(TypedDict):
    source_lang: str  # 源语言
    target_lang: str  # 目标语言
    source_text: str  # 源文本
    country: Optional[str] = None  # 目标语言的国家/地区（可选）
    translation_1: Optional[str] = None  # 初始翻译结果（可选）
    reflection: Optional[str] = None  # 对翻译的反思建议（可选）
    translation_2: Optional[str] = None  # 改进后的翻译结果（可选）

# 创建一个 StateGraph 实例，用于管理翻译工作流的状态和流程
workflow = StateGraph(State)
```

获取state中的信息：state.get("key_name")

更新state中的信息：return { "key_name": new_value }

### 工作流的第一步---初翻

initial_translation 函数是翻译工作流的第一步，负责生成源文本的初始翻译。它接收一个状态字典作为输入，从中提取源语言、目标语言和源文本，然后构造一个系统消息和提示模板，要求模型专注于从源语言到目标语言的翻译任务。通过调用 get_completion 函数，函数获取模型生成的翻译结果，并将其打印出来。最后，函数返回一个字典，包含初始翻译结果，为后续的反思和改进步骤提供基础。

```
def initial_translation(state):
    # 从传入的状态字典中获取源语言、目标语言和源文本
    source_lang = state.get(&quot;source_lang&quot;)
    target_lang = state.get(&quot;target_lang&quot;)
    source_text = state.get(&quot;source_text&quot;)

    # 构造系统消息，指示模型作为专注于从源语言到目标语言翻译的语言学家
    system_message = f&quot;You are an expert linguist, specializing in translation from {source_lang} to {target_lang}.&quot;

    # 构造提示模板，要求模型提供翻译，且不添加任何解释或额外文本
    prompt = f&quot;&quot;&quot;This is an {source_lang} to {target_lang} translation, please provide the {target_lang} translation for this text. \
Do not provide any explanations or text apart from the translation.
{source_lang}: {source_text}

{target_lang}:&quot;&quot;&quot;

    # 调用 get_completion 函数，传入提示和系统消息，获取模型生成的翻译结果
    translation = get_completion(prompt, system_message=system_message)

    # 打印初次翻译结果
    print(&quot;[初次翻译结果]: \n&quot;, translation)

    # 更新state，返回一个字典，包含初始翻译结果
    return { &quot;translation_1&quot;: translation }
```

### 工作流的第二步---reflect

reflect_on_translation 函数是翻译工作流的第二步，负责对初始翻译结果进行反思并提供改进建议。它接收一个状态字典作为输入，从中提取源语言、目标语言、源文本、国家/地区信息和初始翻译结果。函数构造了一个系统消息和提示模板，要求模型对初始翻译进行仔细阅读，并从准确性、流畅性、风格和术语使用等方面提供具体的改进建议。如果提供了国家/地区信息，还会要求翻译的风格和语调符合目标语言在该地区的口语风格。最后，函数返回一个字典，包含模型生成的反思建议，为后续的翻译改进步骤提供依据。

```
def reflect_on_translation(state):
    # 从状态字典中获取必要的信息
    source_lang = state.get(&quot;source_lang&quot;)
    target_lang = state.get(&quot;target_lang&quot;)
    source_text = state.get(&quot;source_text&quot;)
    country = state.get(&quot;country&quot;) or &quot;&quot;  # 如果 country 不存在或为 None，则默认为空字符串
    translation_1 = state.get(&quot;translation_1&quot;)
    
    # 构造系统消息，指示模型作为专注于从源语言到目标语言翻译的语言学家，并且目标是改进翻译
    system_message = f&quot;You are an expert linguist specializing in translation from {source_lang} to {target_lang}. \
You will be provided with a source text and its translation and your goal is to improve the translation.&quot;

    # 如果提供了国家/地区信息，则构造额外的规则，要求翻译的风格和语调符合目标语言在该地区的口语风格
    additional_rule = (
        f&quot;The final style and tone of the translation should match the style of {target_lang} colloquially spoken in {country}.&quot;
        if country != &quot;&quot;
        else &quot;&quot;
    )
    
    # 构造提示模板，要求模型对初始翻译进行反思，并提供改进建议
    prompt = f&quot;&quot;&quot;Your task is to carefully read a source text and a translation from {source_lang} to {target_lang}, and then give constructive criticism and helpful suggestions to improve the translation. \
{additional_rule}

The source text and initial translation, delimited by XML tags &lt;SOURCE_TEXT&gt;&lt;/SOURCE_TEXT&gt; and &lt;TRANSLATION&gt;&lt;/TRANSLATION&gt;, are as follows:

&lt;SOURCE_TEXT&gt;
{source_text}
&lt;/SOURCE_TEXT&gt;

&lt;TRANSLATION&gt;
{translation_1}
&lt;/TRANSLATION&gt;

When writing suggestions, pay attention to whether there are ways to improve the translation&#x27;s \n\
(i) accuracy (by correcting errors of addition, mistranslation, omission, or untranslated text),\n\
(ii) fluency (by applying {target_lang} grammar, spelling and punctuation rules, and ensuring there are no unnecessary repetitions),\n\
(iii) style (by ensuring the translations reflect the style of the source text and takes into account any cultural context),\n\
(iv) terminology (by ensuring terminology use is consistent and reflects the source text domain; and by only ensuring you use equivalent idioms {target_lang}).\n\

Write a list of specific, helpful and constructive suggestions for improving the translation.
Each suggestion should address one specific part of the translation.
Output only the suggestions and nothing else.&quot;&quot;&quot;

    # 调用 get_completion 函数，传入提示和系统消息，获取模型生成的反思建议
    reflection = get_completion(prompt, system_message=system_message)

    # 打印反思建议
    print(&quot;[对初翻给出建议]: \n&quot;, reflection)

    # 返回一个字典，包含反思建议
    return { &quot;reflection&quot;: reflection }
```

#### 教会模型如何反思

##### 1. **准确性（Accuracy）**

+   **含义**：确保翻译内容准确无误，忠实于原文的意思。

+   **具体要求**：

+   纠正添加错误（errors of addition）：检查翻译中是否有不必要的添加或编造内容。

+   纠正误译（mistranslation）：检查是否有词义理解错误或表达不准确的地方。

+   **纠正遗漏（omission）**：确保原文中的所有信息都已完整翻译，没有遗漏。

+   纠正未翻译文本（untranslated text）：检查是否有原文本未被翻译，直接保留了原文。

##### 2. **流畅性（Fluency）**

+   **含义**：确保翻译后的文本在语法、拼写和标点使用上正确，读起来自然流畅。

+   **具体要求**：

+   **应用目标语言的语法规则**：确保翻译符合目标语言的语法规则。

+   **应用拼写规则**：确保没有拼写错误。

+   **应用标点规则**：确保标点符号使用正确。

+   **避免不必要的重复**：检查是否有冗余或重复的表达，确保文本简洁明了。

##### 3. **风格（Style）**

+   **含义**：确保翻译后的文本风格与原文风格一致，并考虑文化背景。

+   **具体要求**：

+   **反映原文风格**：如果原文是正式的，翻译也应正式；如果原文是口语化的，翻译也应口语化。

+   **考虑文化背景**：确保翻译中的文化元素和背景信息得到适当处理，避免文化误解或不适应。

##### 4. 术语使用（Terminology）

+   **含义**：确保翻译中的术语使用一致且符合原文领域。

+   **具体要求**：

+   **术语一致性**：确保同一术语在全文中保持一致。

+   **反映原文领域**：确保术语的使用符合原文的专业领域或特定背景。

+   **使用等效习语**：如果原文中有习语或特定表达，确保在目标语言中使用等效的习语或表达。

##### 输出要求

+   **具体、有帮助且建设性的建议**：每条建议应针对翻译中的具体部分，提供明确的改进建议。

+   **仅输出建议**：不要包含其他无关内容，只输出改进建议。

##### 示例

假设原文是英文，翻译成西班牙文，原文是：“The quick brown fox jumps over the lazy dog.”

+   **准确性**：检查是否有词义理解错误，例如“quick”是否被误译为“fast”而不是“quick”。

+   **流畅性**：确保西班牙文的语法和拼写正确，例如“El rápido zorro marrón salta sobre el perro perezoso.”

+   **风格**：如果原文是正式的，翻译也应保持正式风格；如果原文是口语化的，翻译也应口语化。

+   **术语使用**：如果原文是技术文档，确保技术术语如“fox”和“dog”在目标语言中使用正确的术语。

通过这些指导，模型能够系统地评估翻译质量，并提出具体的改进建议。

### 工作流第三步---改进

improve_translation 函数是翻译工作流的最后一步，负责根据反思建议对初始翻译结果进行改进。它接收一个状态字典作为输入，从中提取源语言、目标语言、源文本、初始翻译结果和反思建议。函数构造了一个系统消息和提示模板，要求模型根据专家建议对翻译进行编辑，确保翻译的准确性、流畅性、风格和术语使用等方面得到优化。最后，函数返回一个字典，包含改进后的翻译结果，完成了整个翻译工作流。

```
def improve_translation(state):
    # 从状态字典中获取必要的信息
    source_lang = state.get(&quot;source_lang&quot;)
    target_lang = state.get(&quot;target_lang&quot;)
    source_text = state.get(&quot;source_text&quot;)
    translation_1 = state.get(&quot;translation_1&quot;)
    reflection = state.get(&quot;reflection&quot;)
    
    # 构造系统消息，指示模型作为专注于从源语言到目标语言翻译编辑的语言学家
    system_message = f&quot;You are an expert linguist, specializing in translation editing from {source_lang} to {target_lang}.&quot;

    # 构造提示模板，要求模型根据专家建议编辑翻译结果
    prompt = f&quot;&quot;&quot;Your task is to carefully read, then edit, a translation from {source_lang} to {target_lang}, taking into
account a list of expert suggestions and constructive criticisms.

The source text, the initial translation, and the expert linguist suggestions are delimited by XML tags &lt;SOURCE_TEXT&gt;&lt;/SOURCE_TEXT&gt;, &lt;TRANSLATION&gt;&lt;/TRANSLATION&gt; and &lt;EXPERT_SUGGESTIONS&gt;&lt;/EXPERT_SUGGESTIONS&gt; \
as follows:

&lt;SOURCE_TEXT&gt;
{source_text}
&lt;/SOURCE_TEXT&gt;

&lt;TRANSLATION&gt;
{translation_1}
&lt;/TRANSLATION&gt;

&lt;EXPERT_SUGGESTIONS&gt;
{reflection}
&lt;/EXPERT_SUGGESTIONS&gt;

Please take into account the expert suggestions when editing the translation. Edit the translation by ensuring:

(i) accuracy (by correcting errors of addition, mistranslation, omission, or untranslated text),
(ii) fluency (by applying {target_lang} grammar, spelling and punctuation rules and ensuring there are no unnecessary repetitions), \
(iii) style (by ensuring the translations reflect the style of the source text)
(iv) terminology (inappropriate for context, inconsistent use), or
(v) other errors.

Output only the new translation and nothing else.&quot;&quot;&quot;

    # 调用 get_completion 函数，传入提示和系统消息，获取改进后的翻译结果
    translation_2 = get_completion(prompt, system_message)

    # 打印最终翻译结果
    print(&quot;[最终翻译结果]: \n&quot;, translation_2)

    # 返回一个字典，包含改进后的翻译结果
    return { &quot;translation_2&quot;: translation_2 }
```

#### 通用建议

在改进翻译时，请考虑专家的建议。翻译时要确保：

（i）准确性（通过纠正加法错误、误译、遗漏或未翻译的文本），

（ii）流畅性（通过应用目标语言的语法、拼写和标点规则，并确保没有不必要的重复），

（iii）风格（通过确保翻译反映源文本的风格），

（iv）术语（不符合上下文、使用不一致），或

（v）其他错误。

请给出您想要翻译的英文原文。

### 规划执行任务

这段代码使用 StateGraph 来规划和执行一个翻译工作流，包含三个主要步骤：初始翻译、反思翻译和改进翻译。通过将每个步骤注册为工作流中的节点，并定义它们之间的连接关系，代码构建了一个结构化、可追踪的翻译流程。这种设计不仅便于管理复杂的工作流，还能确保每个翻译步骤按预定顺序执行，从而提高翻译质量和效率。

```
# 规划执行任务
## 节点（node）注册
# 将初始翻译函数注册为工作流中的一个节点，命名为 &quot;initial_translation&quot;
workflow.add_node(&quot;initial_translation&quot;, initial_translation)
# 将反思翻译函数注册为工作流中的一个节点，命名为 &quot;reflect_on_translation&quot;
workflow.add_node(&quot;reflect_on_translation&quot;, reflect_on_translation)
# 将改进翻译函数注册为工作流中的一个节点，命名为 &quot;improve_translation&quot;
workflow.add_node(&quot;improve_translation&quot;, improve_translation)

## 连接节点
# 设置 &quot;initial_translation&quot; 节点为工作流的入口点，即工作流从该节点开始执行
workflow.set_entry_point(&quot;initial_translation&quot;)
# 添加一条边，将 &quot;initial_translation&quot; 节点的输出连接到 &quot;reflect_on_translation&quot; 节点的输入
workflow.add_edge(&quot;initial_translation&quot;, &quot;reflect_on_translation&quot;)
# 添加一条边，将 &quot;reflect_on_translation&quot; 节点的输出连接到 &quot;improve_translation&quot; 节点的输入
workflow.add_edge(&quot;reflect_on_translation&quot;, &quot;improve_translation&quot;)
# 添加一条边，将 &quot;improve_translation&quot; 节点的输出连接到工作流的终点，表示工作流在此结束
workflow.add_edge(&quot;improve_translation&quot;, END)
```

### 任务执行

```
#执行任务

# 编译工作流
app = workflow.compile()

# 调用编译后的工作流，传入初始状态字典
result = app.invoke({
    &quot;source_lang&quot;: &quot;English&quot;, # 源语言为英语
    &quot;target_lang&quot;: &quot;中文&quot;,    # 目标语言为中文
    &quot;source_text&quot;: &quot;&quot;&quot;By using an LLM as the heart of the translation engine, this system is highly steerable. For example, by changing the prompts, it is easier using this workflow than a traditional machine translation (MT) system to:

Modify the output&#x27;s style, such as formal/informal.
Specify how to handle idioms and special terms like names, technical terms, and acronyms. For example, including a glossary in the prompt lets you make sure particular terms (such as open source, H100 or GPU) are translated consistently.
Specify specific regional use of the language, or specific dialects, to serve a target audience. For example, Spanish spoken in Latin America is different from Spanish spoken in Spain; French spoken in Canada is different from how it is spoken in France.&quot;&quot;&quot;
})

result
```

### 输出结果

```
{&#x27;source_lang&#x27;: &#x27;English&#x27;,

 &#x27;target_lang&#x27;: &#x27;中文&#x27;,
 
 &#x27;source_text&#x27;: &quot;By using an LLM as the heart of the translation engine, this system is highly steerable. For example, by changing the prompts, it is easier using this workflow than a traditional machine translation (MT) system to:\n\nModify the output&#x27;s style, such as formal/informal.\nSpecify how to handle idioms and special terms like names, technical terms, and acronyms. For example, including a glossary in the prompt lets you make sure particular terms (such as open source, H100 or GPU) are translated consistently.\nSpecify specific regional use of the language, or specific dialects, to serve a target audience. For example, Spanish spoken in Latin America is different from Spanish spoken in Spain; French spoken in Canada is different from how it is spoken in France.&quot;,
 
 
 &#x27;translation_1&#x27;: &#x27;通过将LLM作为翻译引擎的核心，该系统具备高度可调控性。例如相较于传统机器翻译（MT）系统，通过修改提示词可以更便捷地实现以下功能：调整输出文本风格（正式/非正式）；指定成语和专业术语（如名称、技术术语、缩略语）的处理方式——通过在提示词中添加术语表，可确保特定词汇（如开源、H100或GPU）的翻译一致性；根据目标受众需求指定语言的地域变体或方言，例如拉丁美洲西班牙语与西班牙本土西班牙语存在差异，加拿大法语与法国本土法语亦有不同。&#x27;,
 
 
 &#x27;reflection&#x27;: &#x27;1. 将&quot;可调控性&quot;改为&quot;可操控性&quot;更符合中文技术语境，原词&quot;steerable&quot;在翻译系统中更常对应&quot;可操控&quot;概念。  \n2. &quot;提示词&quot;建议统一译为&quot;提示语&quot;，因在大型语言模型领域&quot;prompt&quot;的规范译法为&quot;提示语&quot;，且后文重复出现需保持术语一致性。  \n3. &quot;调整输出文本风格（正式/非正式）&quot;中的括号应改为中文全角括号「」更符合中文排版规范，建议修改为「（正式/非正式）」→「（正式／非正式）」并添加顿号。  \n4. &quot;名称&quot;建议改为&quot;专有名词&quot;，因原文&quot;names&quot;在翻译语境中特指需要特殊处理的专有名词翻译，原译容易产生歧义。  \n5. &quot;H100或GPU&quot;中的&quot;或&quot;应改为顿号，中文列举多项时应用顿号分隔，建议修改为&quot;如开源、H100、GPU&quot;。  \n6. &quot;术语表&quot;建议改为&quot;词汇表&quot;，因&quot;glossary&quot;在本地化行业标准译法为&quot;词汇表&quot;，且与后文&quot;特定词汇&quot;形成更好的术语呼应。  \n7. &quot;地域变体&quot;调整为&quot;地域性变体&quot;更符合中文表达习惯，原译&quot;地域变体&quot;在语言学领域不够规范。  \n8. &quot;根据目标受众需求&quot;建议补充为&quot;根据目标受众的具体需求&quot;，添加&quot;具体&quot;二字更准确对应原文&quot;specific&quot;的双重含义。  \n9. &quot;存在差异&quot;与&quot;亦有不同&quot;形成重复，建议统一为&quot;有所不同&quot;，既保持译文流畅度又符合中文忌重复的表达习惯。  \n10. 最后一句建议增加连接词：&quot;例如，拉丁美洲使用的西班牙语与西班牙本土的西班牙语有所不同，而加拿大法语与法国本土法语也存在差异&quot;，通过添加&quot;而&quot;字增强句间逻辑关系。  \n11. &quot;西班牙本土西班牙语&quot;存在语义重复，建议简化为&quot;西班牙的西班牙语&quot;，符合中文简洁表达原则。  \n12. 专业术语处理部分建议调整语序：&quot;指定如何处理成语及特殊术语（包括名称、技术术语、缩略语等）&quot;，通过添加&quot;包括&quot;使列举关系更清晰。&#x27;,
 
 
 &#x27;translation_2&#x27;: &#x27;通过将LLM作为翻译引擎的核心，该系统具备高度可操控性。例如相较于传统机器翻译（MT）系统，通过修改提示语可以更便捷地实现以下功能：调整输出文本风格（正式／非正式）；指定如何处理成语及特殊术语（包括专有名词、技术术语、缩略语等）——通过在提示语中添加词汇表，可确保特定词汇（如开源、H100、GPU）的翻译一致性；根据目标受众的具体需求指定语言的地域性变体或方言，例如拉丁美洲使用的西班牙语与西班牙的西班牙语有所不同，而加拿大法语与法国本土法语亦存在差异。&#x27;}
```