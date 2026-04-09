# Meta-prompts@OpenAI

> 来源：[https://my.feishu.cn/docx/FAPqdnr2ZoW4MFx4bHncovncnch](https://my.feishu.cn/docx/FAPqdnr2ZoW4MFx4bHncovncnch)

# OpenAI 近日发布 meta-prompt 功能，帮助用户更高效地为大语言模型创建和改进提示。

OpenAI 最近发布了一项名为“Meta Prompt”的新功能，旨在帮助用户更高效地创建和改进针对大型语言模型（LLMs）的提示词。这一功能被集成在 OpenAI 的 Playground 提示优化工具中，提供了一种结构化的方法来生成和完善提示词。

Meta Prompt 功能内置在 OpenAI 的 Playground 提示优化功能中。它指导语言模型如何根据任务描述生成有效的提示，或完善现有的提示。该功能采用结构化方法。它以简短的任务描述开头，然后提供更多细节。该提示还包括分步说明、输出格式、示例和注释等可选部分。

## Meta Prompt 的主要特点包括：

+   **结构化生成**：Meta Prompt 功能采用结构化的方法，从简短的任务描述开始，然后提供更多细节。这些提示可以包括分步说明、输出格式、示例和注释等可选部分[1][5]。

+

+   **最佳实践结合经验**：OpenAI 表示，Meta Prompt 结合了实际应用中的最佳实践和经验教训，旨在为用户提供一个快速的起点，避免从零开始撰写提示词的繁琐过程[2][4]。

+   **提高效率**：这一工具主要帮助用户更高效地创建和改进提示词，节省开发高质量 AI 提示的时间[2][4]。

+   **保留用户输入**：Meta Prompt 强调在改进现有提示时尽量保留用户提供的内容，仅在必要时进行简化和优化[6]。

+   **清晰和格式化**：该工具鼓励使用清晰、具体的语言，并采用 Markdown 格式以提升可读性和信息传达的明确性[6]。

**应用场景和优势：**

Meta Prompt 的设计使其在多种任务和输入上都能采用同一套高层级指令，这种通用性特别适合不愿为每个具体任务提供详细示例或指示的用户[8]。此外，Meta Prompt 还能够将复杂任务分解成更小的子任务，并使用适当的格式生成输出，从而提升语言模型的实用性和性能[8]。

总的来说，OpenAI 的 Meta Prompt 功能通过提供结构化和高效的提示生成方法，显著简化了用户与大型语言模型交互的过程，提升了提示词的质量和开发效率。

OpenAI 官方资料👉https://platform.openai.com/docs/guides/prompt-generation

原版完整版本更适合用在开发环境，需要编程能力加持，有一定配置和使用门槛。

考虑到想要直接用来与现成的 LLM 助手聊天，生成合适的提示词，特在开发者版本基础上，整理出了一份针对普通人可以更加简单轻松调用的版本，如下：

## 适合对话界面使用的 Meta Prompt

**在聊天界面（**如 ChatGPT）与 AI 模型对话时，你只需要直接输入提示（也就是 prompt）。这就像在跟朋友聊天时给出要求一样。比如你可以直接输入一个 META_PROMPT，它会帮助模型生成符合你要求的回答。

**特点**：

+   **简单快捷**：不需要写代码，直接告诉 AI 你想要的结果。

+   **即时互动**：你输入内容，AI 马上生成回答。

+   **对话式操作**：适合需要快速沟通、立即得到结果的情况。

**举例**：在 ChatGPT 界面里，直接输入 META_PROMPT 的内容，AI 就会根据这个提示生成你需要的系统提示词。

```
You are a master at generating and optimizing prompts. Please create a detailed System Prompt based on the user&#x27;s input of {current task description or existing prompt words}, in accordance with the requirements of the following 【Meta Prompt】.

```
META_PROMPT = &quot;&quot;&quot;
Given a task description or existing prompt, produce a detailed system prompt to guide a language model in completing the task effectively.

# Guidelines

- Understand the Task: Grasp the main objective, goals, requirements, constraints, and expected output.
- Minimal Changes: If an existing prompt is provided, improve it only if it&#x27;s simple. For complex prompts, enhance clarity and add missing elements without altering the original structure.
- Reasoning Before Conclusions**: Encourage reasoning steps before any conclusions are reached. ATTENTION! If the user provides examples where the reasoning happens afterward, REVERSE the order! NEVER START EXAMPLES WITH CONCLUSIONS!
    - Reasoning Order: Call out reasoning portions of the prompt and conclusion parts (specific fields by name). For each, determine the ORDER in which this is done, and whether it needs to be reversed.
    - Conclusion, classifications, or results should ALWAYS appear last.
- Examples: Include high-quality examples if helpful, using placeholders [in brackets] for complex elements.
   - What kinds of examples may need to be included, how many, and whether they are complex enough to benefit from placeholders.
- Clarity and Conciseness: Use clear, specific language. Avoid unnecessary instructions or bland statements.
- Formatting: Use markdown features for readability. DO NOT USE ``` CODE BLOCKS UNLESS SPECIFICALLY REQUESTED.
- Preserve User Content: If the input task or prompt includes extensive guidelines or examples, preserve them entirely, or as closely as possible. If they are vague, consider breaking down into sub-steps. Keep any details, guidelines, examples, variables, or placeholders provided by the user.
- Constants: DO include constants in the prompt, as they are not susceptible to prompt injection. Such as guides, rubrics, and examples.
- Output Format: Explicitly the most appropriate output format, in detail. This should include length and syntax (e.g. short sentence, paragraph, JSON, etc.)
    - For tasks outputting well-defined or structured data (classification, JSON, etc.) bias toward outputting a JSON.
    - JSON should never be wrapped in code blocks (```) unless explicitly requested.

The final prompt you output should adhere to the following structure below. Do not include any additional commentary, only output the completed system prompt. SPECIFICALLY, do not include any additional messages at the start or end of the prompt. (e.g. no &quot;---&quot;)

[Concise instruction describing the task - this should be the first line in the prompt, no section header]

[Additional details as needed.]

[Optional sections with headings or bullet points for detailed steps.]

# Steps [optional]

[optional: a detailed breakdown of the steps necessary to accomplish the task]

# Output Format

[Specifically call out how the output should be formatted, be it response length, structure e.g. JSON, markdown, etc]

# Examples [optional]

[Optional: 1-3 well-defined examples with placeholders if necessary. Clearly mark where examples start and end, and what the input and output are. User placeholders as necessary.]
[If the examples are shorter than what a realistic example is expected to be, make a reference with () explaining how real examples should be longer / shorter / different. AND USE PLACEHOLDERS! ]

# Notes [optional]

[optional: edge cases, details, and an area to call or repeat out specific important considerations]
&quot;&quot;&quot;
```

【Important!】The final generated System Prompt should be in Simplified Chinese, output in markdown syntax, and expressed in a code block. Ensure that all newly generated prompts are displayed separately within a code block.

{Current task description or existing prompt words}={{input}}
```

## 适合开发环境使用的 Meta Prompt

在编程环境中（如 Python 脚本或 Jup**yter No**tebook），你需要通过 API 调用来发送提示并获取结果。这里不仅需要提供 prompt，还要用代码来处理与 AI 模型的交互。你需要先初始化 OpenAI 的客户端，然后通过调用函数将任务描述发送给模型。

**特点**：

+   **灵活可控**：可以定制化处理数据和输出，适合复杂的应用场景。

+   **需要编写代码**：需要用编程语言（如 Python）与 OpenAI 模型通信。

+   **自动化处理**：适合将 prompt 集成到更复杂的应用中，自动生成大量提示词或结果。

**举例**：使用 Python 脚本，先导入 OpenAI 的库，设置客户端，然后将任务描述通过函数调用发送给模型，获取并返回生成的提示词。

```

from openai import OpenAI

client = OpenAI()

META_PROMPT = &quot;&quot;&quot;
Given a task description or existing prompt, produce a detailed system prompt to guide a language model in completing the task effectively.

# Guidelines

- Understand the Task: Grasp the main objective, goals, requirements, constraints, and expected output.
- Minimal Changes: If an existing prompt is provided, improve it only if it&#x27;s simple. For complex prompts, enhance clarity and add missing elements without altering the original structure.
- Reasoning Before Conclusions**: Encourage reasoning steps before any conclusions are reached. ATTENTION! If the user provides examples where the reasoning happens afterward, REVERSE the order! NEVER START EXAMPLES WITH CONCLUSIONS!
    - Reasoning Order: Call out reasoning portions of the prompt and conclusion parts (specific fields by name). For each, determine the ORDER in which this is done, and whether it needs to be reversed.
    - Conclusion, classifications, or results should ALWAYS appear last.
- Examples: Include high-quality examples if helpful, using placeholders [in brackets] for complex elements.
   - What kinds of examples may need to be included, how many, and whether they are complex enough to benefit from placeholders.
- Clarity and Conciseness: Use clear, specific language. Avoid unnecessary instructions or bland statements.
- Formatting: Use markdown features for readability. DO NOT USE ``` CODE BLOCKS UNLESS SPECIFICALLY REQUESTED.
- Preserve User Content: If the input task or prompt includes extensive guidelines or examples, preserve them entirely, or as closely as possible. If they are vague, consider breaking down into sub-steps. Keep any details, guidelines, examples, variables, or placeholders provided by the user.
- Constants: DO include constants in the prompt, as they are not susceptible to prompt injection. Such as guides, rubrics, and examples.
- Output Format: Explicitly the most appropriate output format, in detail. This should include length and syntax (e.g. short sentence, paragraph, JSON, etc.)
    - For tasks outputting well-defined or structured data (classification, JSON, etc.) bias toward outputting a JSON.
    - JSON should never be wrapped in code blocks (```) unless explicitly requested.

The final prompt you output should adhere to the following structure below. Do not include any additional commentary, only output the completed system prompt. SPECIFICALLY, do not include any additional messages at the start or end of the prompt. (e.g. no &quot;---&quot;)

[Concise instruction describing the task - this should be the first line in the prompt, no section header]

[Additional details as needed.]

[Optional sections with headings or bullet points for detailed steps.]

# Steps [optional]

[optional: a detailed breakdown of the steps necessary to accomplish the task]

# Output Format

[Specifically call out how the output should be formatted, be it response length, structure e.g. JSON, markdown, etc]

# Examples [optional]

[Optional: 1-3 well-defined examples with placeholders if necessary. Clearly mark where examples start and end, and what the input and output are. User placeholders as necessary.]
[If the examples are shorter than what a realistic example is expected to be, make a reference with () explaining how real examples should be longer / shorter / different. AND USE PLACEHOLDERS! ]

# Notes [optional]

[optional: edge cases, details, and an area to call or repeat out specific important considerations]
&quot;&quot;&quot;.strip()

def generate_prompt(task_or_prompt: str):
    completion = client.chat.completions.create(
        model=&quot;gpt-4o&quot;,
        messages=[
            {
                &quot;role&quot;: &quot;system&quot;,
                &quot;content&quot;: META_PROMPT,
            },
            {
                &quot;role&quot;: &quot;user&quot;,
                &quot;content&quot;: &quot;Task, Goal, or Current Prompt:\n&quot; + task_or_prompt,
            },
        ],
    )

    return completion.choices[0].message.content
```

这两种场景各有用途：**聊天界面**适合简单直接的任务，而**开发环境**则适合复杂的需求，需要更多的控制和定制。

# Meta Prompt 与传统提示生成方法有何不同？

Meta Prompting 是一种新兴的提示生成方法，与传统的提示生成方法相比，具有显著的不同和优势。以下是 Meta Prompting 与传统提示生成方法的主要区别：

## **结构导向 vs. 内容导向**

**Meta Prompting**：

+   **结构导向**：Meta Prompting 更加注重提示的结构，而不是具体的内容。这种方法通过抽象和泛化关键原则，建立任务与提示之间的函子关系，从而提供一个系统化和适应性强的框架[2][4]。

+   **零样本效能**：Meta Prompting 可以被视为一种零样本提示方法，减少了对具体示例的依赖，从而在不同任务上表现出色[4]。

**传统提示生成**：

+   **内容导向**：传统的提示生成方法通常依赖于具体的内容和示例，例如 few-shot 提示方法，需要提供多个示例来指导模型生成所需的输出[4]。

+   **示例依赖**：传统方法需要为每个任务提供详细的示例或具体指示，这在处理多样化任务时可能显得繁琐和低效[1][4]。

## **动态调整 vs. 静态提示**

**Meta Prompting**：

+   **动态调整**：Meta Prompting 允许大型语言模型（LLMs）根据用户反馈动态调整和优化提示，从而能够处理更复杂的任务和不断变化的上下文[3][7]。

+   **多模型协作**：Meta Prompting 使用一个“指挥员”模型来协调和执行多个独立的专家模型的任务，这些专家模型可以在不同领域提供专业知识，从而生成更高质量的输出[1][3][8]。

**传统提示生成**：

+   **静态提示**：传统方法通常是静态的，即提示一旦生成，模型就按照该提示执行任务，缺乏动态调整的能力[3]。

+   **单一模型**：传统方法通常依赖单一模型来处理所有任务，缺乏多模型协作的灵活性和专业性[1][3]。

## **效率和灵活性**

**Meta Prompting**：

+   **高效性**：通过减少对具体示例的依赖，Meta Prompting 提高了提示生成的效率，减少了所需的 token 数量，从而在处理复杂任务时更加高效[4]。

+   **灵活性**：Meta Prompting 提供了一个广泛、灵活的框架，能够适应多种任务和输入，而不影响其特定性或相关性[1][4]。

**传统提示生成**：

+   **低效性**：传统方法在处理复杂任务时可能需要大量的示例和详细指示，导致效率较低[4]。

+   **局限性**：传统方法的灵活性较差，通常需要为每个具体任务单独设计提示，难以适应多样化的需求[1][4]。

总的来说，Meta Prompting 通过其结构导向、动态调整和多模型协作的特点，显著提升了提示生成的效率和灵活性，克服了传统提示生成方法的诸多局限性。

# 用户如何利用 Meta Prompt 来优化用户的提示？

用户可以通过以下步骤利用 Meta Prompt 来优化他们的提示，从而提高与大型语言模型（LLMs）交互的效率和效果：

## **1. 结构化生成提示**

Meta Prompt 采用结构化的方法来生成和优化提示。具体步骤包括：

+   **简要描述任务**：首先，用户需要提供一个简洁的任务描述。这是提示生成的基础，确保模型理解用户的基本需求[1][2][5]。

+   **补充详细信息**：在任务描述之后，用户可以提供更多的细节。这些细节可以包括逐步说明、输出格式、示例和注释等可选部分，以确保提示的全面性和准确性[1][2][5]。

## **2. 动态调整和优化**

Meta Prompt 允许用户根据模型的反馈动态调整和优化提示：

+   **最小化更改**：在改进现有提示时，Meta Prompt 强调尽量做出最小化的更改，以保留用户提供的内容和意图[2][8]。

+   **推理步骤**：在得出结论之前，Meta Prompt 鼓励展开推理步骤，确保提示的逻辑性和连贯性[2][8]。

+   **高级示例**：在适当时机，用户可以融入高级示例，以进一步指导模型生成高质量的输出[2][8]。

## **3. 使用清晰和具体的语言**

Meta Prompt 强调使用清晰、具体的语言进行表述，并应用 Markdown 格式以提升提示的可读性和信息传达的明确性[2][8]。这有助于减少歧义，提高模型对提示的理解和响应质量。

## **4. 多模型协作**

Meta Prompt 采用多模型协作的方式，通过一个“指挥员”模型来协调和执行多个独立的专家模型的任务。这些专家模型可以在不同领域提供专业知识，从而生成更高质量的输出[1][3][8]。

## **5. 实践和经验结合**

Meta Prompt 结合了实际应用中的最佳实践和经验教训，提供了一个快速的起点，避免用户从零开始撰写提示的繁琐过程[1][2][5]。用户可以利用这些最佳实践来优化提示，提高提示的质量和适用性。

## **总结**

通过以上步骤，用户可以利用 Meta Prompt 来优化他们的提示，从而更高效地与大型语言模型交互。Meta Prompt 提供了一个结构化、动态调整和多模型协作的框架，结合实际应用中的最佳实践，显著提升了提示生成的效率和效果。