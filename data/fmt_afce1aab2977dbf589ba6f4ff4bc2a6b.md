
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
