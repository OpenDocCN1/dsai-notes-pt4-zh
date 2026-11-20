
# 使用 Bash 脚本重命名文件

1. 复制表 16 的输出（不包括 `bash` 文本包装器），并将其粘贴到一个名为 `rename_files.sh` 的新文件中。

2. 打开一个终端窗口并输入：`. rename_files.sh`。它会要求输入文件夹名称，例如 `test`，然后按回车键。

3. 脚本似乎运行良好。您将看到消息：文件重命名成功。

当您查看测试文件夹时，会发现所有文件都已完美地重命名为 `draft_filename.txt`。

它成功了！

# 解释代码的提示 (Prompts for explaining code)

作为开发者，在团队中工作时，您必须阅读他人的代码。Gemini 也可以在这方面提供帮助。让我们取用表 16 的代码输出，移除注释，并要求大型语言模型解释发生了什么，见表 17：

表 17：解释代码的提示 (A prompt for explaining code)

| 字段 | 值 |
| --- | --- |
| 名称 (Name) | `1_bash_explain_the_code` |
| 目标 (Goal) | 编写一个提示来解释 Bash 代码。(Write a prompt to explain Bash code) |
| 模型 (Model) | `gemini-pro` |
| 温度 (Temp) | 0.1 |
| 令牌限制 (Limit) | 1024 |
| Top-K | N/A |
| Top-P | 1 |
| 提示 (Prompt) | 请向我解释下面的 Bash 代码： |
| 输出 (Output) | 这个 Bash 脚本执行以下任务： |

这表示 LLM 在代码理解方面的效用。模型准确地分解了脚本，并用自然语言解释了每个命令块的目的。

# 翻译代码的提示 (Prompts for translating code)

表 16 中的 bash 代码似乎工作正常。然而，如果这个脚本能提示我输入文件名，并且理想情况下能作为一个带有 UI 的独立应用程序工作，那么它将真正具有可重用性。作为起点，对于（Web）应用程序来说，Python 比 Bash 是更好的语言。LLM 可以帮助将代码从一种语言翻译到另一种语言。参见表 18 中的示例 1：

表 18：将代码从 Bash 翻译为 Python 的提示 (A prompt for translating code from Bash to Python)

| 字段 | 值 |
| --- | --- |
| 名称 (Name) | `1_python_rename_files` |
| 目标 (Goal) | 编写一个提示，将 Bash 代码翻译成 Python。(Write a prompt to translate Bash code to Python) |
| 模型 (Model) | `gemini-pro` |
| 温度 (Temp) | 0.1 |
| 令牌限制 (Limit) | |
| Top-K | N/A |
| Top-P | 1 |
| 提示 (Prompt) | 将下面的 Bash 代码翻译成 Python 片段。 |
| 输出 (Output) | `python` |

此表展示了 LLM 的代码翻译能力，将功能从一种语言的习惯用法（Bash 命令）映射到另一种语言（如 Python 的 `os`、`shutil` 库）。

阅读并审查代码。复制提示的输出并将其粘贴到一个新文件：`file_renamer.py`。通过打开终端窗口并执行以下命令来测试代码：`python file_renamer.py`。

注意： 在 Vertex AI 的 Language Studio 中提示（Python）代码时，您必须单击“Markdown”按钮。否则，您将收到缺少正确行缩进的纯文本，这对于运行 Python 代码很重要。

# 调试和审查代码的提示 (Prompts for debugging and reviewing code)

让我们手动对表 18 的代码进行一些编辑。它应该提示用户输入文件名前缀，并将此前缀以大写字符写入。参见代码片段 3 中的示例代码，但真糟糕。它现在返回 Python 错误！

代码片段 3：一个损坏的 Python 脚本 (A broken Python script)

```python
import os

import shutil

folder_name = input("输入文件夹名称: ")

prefix = input("输入要添加到文件名前的前缀字符串: ")

错误：使用了未定义的函数 toUpperCase

text = toUpperCase(prefix)

if not os.path.isdir(folder_name):

print("文件夹不存在。")

exit(1)

files = os.listdir(folder_name)

for file in files:

错误：变量名不一致 new_filename vs new_file_name

new_filename = f"{text}_{file}"

old_path = os.path.join(folder_name, file)

#使用了未在循环中定义的 new_file_name

new_path = os.path.join(folder_name, new_file_name)

shutil.move(old_path, new_path)

print("文件重命名成功。")
```

哎呀！看起来像个 bug ：

代码片段 4：Python 代码错误的回溯信息

以下 Python 代码给出了错误：

```
Traceback (most recent call last):

  File "/Users/leeboonstra/Documents/test_folder/rename_files.py", line 7, in <module>

    text = toUpperCase(prefix)

NameError: name 'toUpperCase' is not defined
```

代码片段 4：我弄坏了 Python 代码 (I broke the Python code)

让我们看看是否可以要求大型语言模型调试和审查代码。请看表 19：

表 19：调试和审查 Python 代码的提示 (A prompt for debugging and reviewing Python code)

| 字段 | 值 |
| --- | --- |
| 名称 (Name) | `1_python_debug_code` |
| 目标 (Goal) | 编写一个提示来调试和审查 Python 代码。(Write a prompt to debug and review Python code.) |
| 模型 (Model) | `gemini-pro` |
| 温度 (Temp) | 0.1 |
| 令牌限制 (Limit) | 1024 |
| Top-K | N/A |
| Top-P | 1 |
| 提示 (Prompt) | 以下 Python 代码给出了错误： |
| 输出 (Output) | 代码调用了 `toUpperCase` 函数将 `prefix` 转换为大写，但该函数未定义。要解决此问题，您可以使用 |

此表有力地证明了 LLM 不仅可以修复特定错误，还可以进行主动的代码审查并提出最佳实践建议。将错误回溯与代码一起提供是调试的关键上下文。提供错误消息/回溯以及代码极大地帮助 LLM 定位问题，使调试提示比仅提供代码并说“修复它”有效得多。

太棒了。它不仅告诉了我如何解决问题，还找出了我的代码有更多错误以及如何解决它们。提示的最后一部分给出了改进代码的总体建议。

# 多模态提示 (Multimodal Prompting)

那么，多模态提示呢？ (What about multimodal prompting?)

为代码编写提示仍然使用相同常规的大型语言模型。多模态提示是一个独立的问题，它指的是一种使用多种输入格式（而不仅仅是依赖文本）来指导大型语言模型的技术。这可以包括文本、图像、音频、代码甚至其他格式的组合，具体取决于模型的能力和手头的任务。

本节主要承认多模态提示的存在，但明确将其排除在详细讨论之外，使白皮书专注于基于文本（包括代码）的提示工程。

# 最佳实践 (Best Practices)

找到正确的提示需要反复调试。Vertex AI 中的 Language Studio 是一个完美的场所，可以在其中试用您的提示，并能够针对各种模型进行测试。

使用以下最佳实践成为提示工程专家。

## 提供示例 (Provide examples)

最重要的最佳实践是在提示中提供（单样本/少样本）示例。这是非常有效的，因为它充当了强大的教学工具。这些示例展示了期望的输出或类似的响应，使模型能够从中学习并相应地调整其自身的生成。这就像给模型一个参考点或目标，以提高其响应的准确性、风格和语调，使其更好地符合您的期望 1。这再次强调了之前看到的少样本学习的有效性。明确称其为“最重要”的实践，突显了它在各种任务中的巨大影响力。

## 简洁设计 (Design with simplicity)

提示应该简洁、清晰、易于理解（对您和模型都是如此）。作为经验法则，如果它对您来说已经令人困惑，那么它很可能对模型来说也同样令人困惑。尽量不要使用复杂的语言，也不要提供不必要的信息。有时少即是多。虽然上下文是好的，但不必要的行话或过于复杂的句子结构可能会混淆模型。直接、清晰的语言是首选。

示例：

修改前 (BEFORE):

我现在正在访问纽约，我想了解更多关于好地点的信息。我和两个 3 岁的孩子在一起。我们假期应该去哪里？

重写后 (AFTER REWRITE):

扮演游客的旅行指南。描述在纽约曼哈顿适合带 3 岁孩子参观的好地方。

尝试使用描述动作的动词。以下是一些示例：

扮演 (Act), 分析 (Analyze), 分类 (Categorize), 归类 (Classify), 对比 (Contrast), 比较 (Compare), 创建 (Create), 描述 (Describe), 定义 (Define), 评估 (Evaluate), 提取 (Extract), 查找 (Find), 生成 (Generate), 识别 (Identify), 列出 (List), 测量 (Measure), 组织 (Organize), 解析 (Parse), 挑选 (Pick), 预测 (Predict), 提供 (Provide), 排名 (Rank), 推荐 (Recommend), 返回 (Return), 检索 (Retrieve), 重写 (Rewrite), 选择 (Select), 显示 (Show), 排序 (Sort), 总结 (Summarize), 翻译 (Translate), 编写 (Write).

## 具体说明输出 (Be specific about the output)

关于期望的输出要具体。简洁的指令可能不足以指导 LLM，或者可能过于笼统。在提示中提供具体细节（通过系统或上下文提示）可以帮助模型专注于相关内容，提高整体准确性。

不要假设模型知道您想要什么。明确说明约束条件，如长度（“3 段”）、内容焦点（“排名前 5 的视频游戏机”）和风格（“对话式”）。

示例：

推荐 (DO):

生成一篇关于排名前 5 的视频游戏机的 3 段博客文章。该博客文章应内容丰富且引人入胜，并应以对话式风格编写。

不推荐 (DO NOT):

生成一篇关于视频游戏机的博客文章。

## 使用指令而非约束 (Use Instructions over Constraints)

指令和约束在提示中用于指导 LLM 的输出。

- 指令 (instruction) 提供关于响应的期望格式、风格或内容的明确指示。它指导模型应该做什么或产生什么。
- 约束 (constraint) 是对响应的一组限制或边界。它限制模型不应该做什么或避免什么。

越来越多的研究表明，在提示中专注于积极指令可能比严重依赖约束更有效。这种方法与人类更喜欢积极指令而非一堆“不要做”列表的偏好相一致。以积极的方式提出请求（包含什么）通常比消极约束（排除什么）更清晰，更不容易被误解或产生冲突。

指令直接传达期望的结果，而约束可能让模型猜测什么是允许的。它在定义的边界内提供了灵活性并鼓励创造力，而约束可能限制模型的潜力。此外，一堆约束可能会相互冲突。

约束在某些情况下仍然很有价值。例如，防止模型生成有害或有偏见的内容，或者当需要严格的输出格式或风格时。如果可能，使用积极指令：与其告诉模型不要做什么，不如告诉它该做什么。这可以避免混淆并提高输出的准确性。

示例：

推荐 (DO):

生成一篇关于排名前 5 的视频游戏机的 1 段博客文章。仅讨论游戏机本身、制造商公司、年份和总销量。

不推荐 (DO NOT):

生成一篇关于排名前 5 的视频游戏机的 1 段博客文章。 不要列出视频游戏名称。

作为最佳实践，首先优先考虑指令，清楚地说明您希望模型做什么，仅在出于安全、清晰或特定要求需要时才使用约束。进行实验和迭代，测试指令和约束的不同组合，以找到最适合您特定任务的方法，并记录这些尝试。

## 控制最大令牌长度 (Control the max token length)

要控制生成的 LLM 响应的长度，您可以在配置中设置最大令牌限制，或者在提示中明确请求特定长度。

例如：

"用一条推文长度的消息解释量子物理学。"

长度可以通过技术（配置）和语义（提示）两种方式进行管理。基于提示的控制允许更细致的长度规范（例如，“用 3 句话”，“推文长度”）。

## 在提示中使用变量 (Use variables in prompts)

为了重用提示并使其更具动态性，请在提示中使用变量，这些变量可以针对不同的输入进行更改。例如，如表 20 所示，一个提供城市事实的提示。不要在提示中硬编码城市名称，而是使用变量。变量可以通过允许您避免重复自己来节省时间和精力。如果您需要在多个提示中使用相同的信息，可以将其存储在变量中，然后在每个提示中引用该变量。这在将提示集成到您自己的应用程序中时非常有意义。

表 20：在提示中使用变量 (Using variables in prompts)

| 字段 | 值 |
| --- | --- |
| 变量 (VARIABLES) | `{city}` = `&quot;阿姆斯特丹&quot;` |
| 提示 (PROMPT) | 你是一个旅行指南。告诉我一个关于城市 `{city}` 的事实。 |
| 输出 (Output) | 阿姆斯特丹是一个美丽的城市，到处是运河、桥梁和狭窄的街道。因其丰富的历史、文化和夜生活而成为一个绝佳的旅游目的地。 |

此表清晰地展示了一个简单的模板机制，用于使提示可重用并适应不同的输入，而无需重写整个提示结构。这对于程序化使用至关重要。

## 尝试不同的输入格式和写作风格 (Experiment with input formats and writing styles)

不同的模型、模型配置、提示格式、措辞选择甚至提交都可能产生不同的结果。因此，尝试提示属性（如风格、措辞选择和提示类型（零样本、少样本、系统提示））非常重要。LLM 对输入的精确措辞和格式可能出奇地敏感。微小的变化可能导致不同的输出，这再次强调了实验的必要性。

例如，一个目标是生成关于革命性视频游戏机世嘉 Dreamcast 文本的提示，可以表述为问题、陈述或指令，从而产生不同的输出：

- 问题： 世嘉 Dreamcast 是什么？为什么它是一款如此革命性的游戏机？

- 陈述： 世嘉 Dreamcast 是世嘉于 1999 年发布的第六代视频游戏机。它...

- 指令： 编写一段描述世嘉 Dreamcast 游戏机并解释其革命性原因的文字。

## 对于带分类任务的少样本提示 (For few-shot prompting with classification tasks, mix up the classes)

一般来说，少样本示例的顺序不应有太大影响。但是，在进行分类任务时，请确保在少样本示例中混合可能的响应类别。这样做是因为您可能否则会过度拟合示例的特定顺序。通过混合可能的响应类别，您可以确保模型正在学习识别每个类别的关键特征，而不仅仅是记忆示例的顺序。这将导致在未见过的数据上表现更鲁棒和更具泛化性。在少样本分类中，示例的顺序可能无意中成为一个信号。混合类别迫使模型依赖示例的实际内容来区分类别，而不是仅仅依赖它们在序列中的位置。

一个好的经验法则是从 6 个少样本示例开始，并从那里开始测试准确性。

## 适应模型更新 (Adapt to model updates)

了解模型架构变化、新增数据和能力非常重要。尝试更新的模型版本，并调整您的提示以更好地利用新模型特性。像 Vertex AI Studio 这样的工具非常适合存储、测试和记录您提示的各种版本。提示工程不是一次性任务。今天效果好的提示可能随着底层模型的演变而需要调整。持续测试和适应是必要的。

## 尝试不同的输出格式 (Experiment with output formats)

除了提示输入格式，考虑尝试输出格式。对于非创造性任务，如提取、选择、解析、排序、排名或分类数据，尝试让您的输出以结构化格式（如 `JSON` 或 `XML`）返回。明确要求结构化输出（如 `JSON`）可以作为一种强约束，提高可靠性并减少无意义或非结构化的响应，尤其对于面向数据的任务。

返回从提取数据的提示中获取 JSON 对象有一些好处。在实际应用中，我不需要手动创建此 JSON 格式，我已经可以按排序顺序返回数据（在处理日期时间对象时非常方便），但最重要的是，通过提示要求 JSON 格式，它迫使模型创建结构并限制幻觉。

少样本提示部分的表 4 展示了如何返回结构化输出的示例。

## 与其他提示工程师一起实验 (Experiment together with other prompt engineers)

如果您处于必须尝试想出一个好提示的情况，您可能希望找多个人进行尝试。当每个人都遵循最佳实践（如本章所列）时，您将看到所有不同提示尝试之间的性能差异。不同的人以不同的方式处理问题。协作进行提示设计可以引入多样化的视角和措辞，可能比单个人独立工作更快地找到更好的解决方案。
