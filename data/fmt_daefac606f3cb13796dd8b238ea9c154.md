
# AZ-2007: 使用 GitHub Copilot 加速应用开发

> 来源：https://zsyhjtnsa5.feishu.cn/docx/Iarzd6xiKotkUvxFzIccSf98nQb

**课件：**

**repo：** `https://github.com/MicrosoftLearning/mslearn-github-copilot-dev`

# 开始使用 GitHub Copilot

GitHub Copilot 是一个 AI 结对编程助手，它利用自然语言处理和机器学习技术来理解用户的问题并提供答案。研究表明，使用它可以显著提高开发人员的整体工作效率。

## 核心概念与系统结构

### Copilot 的定义与用途

GitHub Copilot 适用于所有常用编程语言。它可以帮助开发者：解释和记录代码、回答编码问题、建议 bug 修复、生成单元测试用例以及建议改进现有代码库。

### Visual Studio Code 中的扩展结构

GitHub Copilot 与 Visual Studio Code （VS Code）、Visual Studio、JetBrains IDE 等环境本机集成。在 VS Code 中，它使用两个主要扩展：

1. `GitHub Copilot` 扩展：主要负责代码完成功能，包括根据代码注释生成的代码建议。

1. `GitHub Copilot Chat` 扩展：支持 AI 对等编程工具，包括用户与 AI 模型之间的聊天对话。

---

## 关键操作技巧（操作指南）

### 配置与设置（Installation & Configuration）

### 代码自动补全（Code Completion）

自动补全建议旨在加速应用开发，它会在您输入代码时实时提供建议。

### AI 对话与交互方式（Chat Interaction）

GitHub Copilot Chat 提供了多种交互方式来与 AI 结对程序员协作。

#### 方式 A：内联聊天（Inline Chat）

内联聊天允许您直接在代码编辑器中利用 AI 的强大功能，最大限度地减少开发工作流的中断。

#### 方式 B：智能操作（Smart Actions）

智能操作是预定义的操作，使您无需编写提示即可快速轻松地执行常见的编码任务。

#### 方式 C：聊天视图与模式（Chat View and Modes）

聊天视图（通常在右侧的辅助侧栏中）用于管理您和 AI 之间的聊天对话。

**聊天模式切换：** 聊天视图提供了三种优化模式。

1. `Ask` 模式：优化用于询问有关代码项目、编码主题和一般技术概念的问题。

1. `Edit` 模式：优化用于在工作区中提出代码更新建议（例如重构、添加注释）。建议的更新会直接应用到编辑器中，并提供保留或撤消的选项。

1. `Agent` 模式：允许您指定高级任务，AI 会自主评估请求、规划所需工作，并结合代码编辑和工具调用来完成任务。注意，Copilot 在运行某些终端命令或未内置工具之前，会请求确认。

---

## 关键概念：计划选择与限制

### 计划概述（Plan Overview）

GitHub Copilot 提供多种订阅计划，以满足不同用户的需求。

+   学生、教师或热门开源项目的维护者经过验证后，可能有资格免费使用 `Copilot Pro` 计划。

+   `Copilot Free`（免费版）：为没有通过组织访问 Copilot 的个人提供有限访问权限，例如每月最多 2000 次代码自动补全和 50 次聊天或高级请求。

+   `Copilot Pro`/`Pro+`：针对想要更高灵活性的个人，提供无限次代码补全、更多高级模型以及更高的每月高级请求额度。

### 潜在风险与注意事项（Limitations）

学生在使用 Copilot 生成的代码时必须保持警惕和批判性思维，因为 Copilot 存在以下限制：

+   **代码不准确性：** `Copilot Chat` 可能会生成看起来有效但实际无效的代码。为了降低风险，必须仔细审查和测试生成的代码。

+   **安全风险：** 生成的代码可能会公开敏感信息或漏洞。对于安全敏感型应用程序，应始终全面审查和测试所生成的代码。

+   **与公共代码匹配：** `Copilot` 使用筛选器来阻止与 GitHub 存储库上的公共代码匹配，但仍有低概率生成匹配代码。用户应采取预防措施，如严格测试、IP 扫描和安全漏洞检查。

+   **范围受限：** 它可能无法处理复杂的代码结构或公共存储库中表示较少的编程语言。

+   **非编码主题：** `Copilot Chat` 并非设计用于回答非编码问题，在这种情况下回复可能不准确或无用。

## 实验：检查 GitHub Copilot 设置和用户界面功能

`https://microsoftlearning.github.io/mslearn-github-copilot-dev/Instructions/Labs/LAB_AK_01_examine_settings_interface.html`

+   熟悉并配置 GitHub Copilot 的相关设置。

+   探索 Visual Studio Code 中 GitHub Copilot 的用户界面与主要功能。

+   掌握聊天视图、快速聊天及内联聊天等交互方式的使用方法。

+   运用 GitHub Copilot 实现代码补全，提升编码效率。

+   通过智能操作完成常见开发任务，理解其应用场景与效果。

# 使用 GitHub Copilot 工具生成文档

GitHub Copilot 在代码分析与文档生成中提供了多种核心功能，旨在帮助开发人员集思广益、探索编码技术、评估问题、解释复杂代码，并创建可读性高且可维护的代码库。

以下是 GitHub Copilot 在这两个领域提供的核心功能和交互方式：

---

## 代码分析与解释的核心功能

GitHub Copilot 允许开发人员了解代码的用途和行为，以便更轻松地使用和维护代码。以下功能均可用于分析或解释代码：

### 聊天视图（Chat View）

聊天视图（默认为`询问模式`）用于管理您和 AI 程序员之间的对话。您可以利用它来讨论各种主题：

+   **询问编码和技术概念：** 例如，“什么是链表？”或“最受欢迎的 10 个 Web 框架”。

+   **集思广思：** 关于如何最好地解决编码问题，例如“如何向我的项目添加身份验证？”。

+   **解释代码块：** 使用 `@workspace /explain` 命令或提问“此代码是做什么的？”。

+   **建议代码修复：** 使用 `@workspace /fix` 命令或描述问题，例如“此方法给出了 `FileNotFoundException`”。

+   **分析整个工作区：** 询问有关代码项目、编码主题和常规编程实践的问题。

### 内联聊天（Inline Chat）

内联聊天允许您直接从编辑器启动会话，而无需离开当前工作环境。它最适合在活动编辑器中向代码提问。

+   **解释代码块：** 使用 `/explain` 命令来解释代码，例如 `/explain #selection`。

+   **检查问题：** 询问“为什么此代码会引发异常？”或“如何解决此问题？”。

+   **查询特定代码：** 询问有关特定代码节、行或变量的问题。

### 智能操作（Smart Actions）

**“解释智能操作”** 功能可以直接解释所选代码，甚至无需编写提示语。

+   **操作方式：** 通过右键单击活动代码编辑器，选择 `Copilot`，然后选择“解释”来访问此功能。

+   **焦点范围：** 可以通过选择代码块、单个代码行或变量来缩小解释的焦点。响应显示在“聊天”视图中。

### 快速聊天（Quick Chat）

快速聊天功能允许您快速提出问题，然后继续工作。

+   **用途：** 适用于不属于较大对话的快速问题，且不会影响您在聊天视图中生成的聊天历史记录。

---

## 文档生成的核心功能

GitHub Copilot Chat 可帮助您快速、准确且一致地记录代码。文档生成分为“内联代码文档”和“项目文档”。

### 生成内联代码文档（Inline Code Documentation）

内联代码文档旨在为代码文件、类或方法生成说明，使代码库更易于理解。

### 生成项目文档（Project Documentation）

项目文档（如 `README.md`、API 引用等）描述了项目的用途、目标和要求。聊天视图是生成项目文档的理想工具，因为它可以分析整个项目结构。

**请注意：** 在代理模式下使用聊天视图时，GitHub Copilot 可能会发出多个高级请求来完成单个任务，这可能会影响成本，因此需要考虑。

您的查询涉及在使用 GitHub Copilot 时，如何通过提高提示质量和有效利用其提供的关键字（聊天参与者、斜杠命令和聊天变量）来优化和获取更相关的响应。

以下是基于提供的来源的最佳实践和详细解释：

## 提高提示质量的最佳实践

提示的质量和提供的上下文对于 GitHub Copilot 生成准确且有用的响应至关重要。

### 保持清晰、简洁和具体

+   您提交的提示应该清晰、简洁且具体。

+   **优化示例：** 应该询问“如何使用此函数？”，而不是询问“是否可以解释函数 `calculateTotal` 在 `shoppingCart.js` 文件中的工作原理？”。

### 结构化复杂提示

+   当您需要编写较长的提示时，应使用几个简短的句子来构造提示。

+   首先是概述。

+   接着描述您的目标。

+   最后提供具体的详细信息。

+   将复杂的提示分解为更小、更易于管理的部分。分解有助于 AI 了解意图并提供更准确的响应。

+   后续提示可以基于前面的提示，从而优化请求并获取更具体的答案。这种与 AI 程序员合作的方式，类似于与人一起工作，即先制定对话框架，然后描述特定详细信息。

## 利用关键字优化响应的最佳实践

GitHub Copilot Chat 使用关键字来帮助指定提示的上下文和意图。通过使用这些关键字，您可以帮助 AI 了解所请求的内容并提供更相关的响应。

### 聊天参与者（Chat Participants）：指定领域专家

聊天参与者类似于特定领域的专家，它们使用 `@` 符号指定，有助于 GitHub Copilot Chat 在了解您感兴趣的上下文时提供更好的响应。

**最佳实践：**

+   在提示前添加特定聊天参与者，以帮助 `Copilot` 生成更相关的响应。

+   **限制：** `@workspace` 只能在`询问模式（`Ask mode`）`下使用。在编辑模式（`Edit mode`）或代理模式（`Agent mode`）下不支持使用聊天参与者。

### 斜杠命令（Slash Commands）：阐明意图和目标

斜杠指令（以 `/` 开头）有助于描述提示的意图（目标或目的），让 `Copilot` 知道您正在尝试执行的操作，从而优化其回复。

**最佳实践：结合参与者和命令**

+   斜杠命令通常与聊天参与者捆绑在一起。

+   将聊天参与者和斜杠命令结合使用是阐明意图的强大方法。例如：

+   使用 `@workspace /explain` 来生成对完整工作区的说明。

+   使用 `@workspace /fix`（或 `/fix`）来建议对所选代码中问题的修复方案。

+   使用 `@workspace /doc` 可在询问模式下为项目、类或方法创建内联代码文档。

**优化示例：**

+   与其输入自然语言提示，如“Create a new workspace that uses Node.js， the Express framework， the Pug template engine， and TypeScript”，不如使用关键字：`@workspace /new Node.js Express Pug TypeScript`，这更容易让 GitHub Copilot 理解。

### 聊天变量（Chat Variables）：提供特定上下文

聊天变量用于指定上下文，使用 `#` 符号引用。通过使用聊天变量，可以使提示中包含的上下文更具体。

**关键优化：`@workspace` 与 `#codebase` 的选择**

从概念上讲，`@workspace` 和 `#codebase` 都可以让您对整个代码库提出问题，但在使用方式和灵活性上存在关键差异。

**总结：** 虽然 `@workspace` 可以回答关于代码库的问题，但 `#codebase` 更灵活，因为它可以在所有聊天模式中使用，并且可以与编辑工具结合使用。 GitHub 建议在聊天提示中使用 `#codebase` 以获得更大的灵活性。

## 实验：使用 GitHub Copilot 分析和记录代码

`https://github.com/MicrosoftLearning/mslearn-github-copilot-dev/blob/main/Instructions/Labs/LAB_AK_02_analyze_document_code_py.md`

+   掌握使用 GitHub Copilot 分析和记录代码库的基本方法

+   学习利用 GitHub Copilot 生成项目结构和关键类的说明文档

+   使用 GitHub Copilot 创建数据访问类的相关说明

+   [通过 GitHub](https://readme.md/) `Copilot` 自动生成项目的 `README.md` 文件

+   熟悉使用 AI 工具进行代码文档化的完整工作流程

# 使用 GitHub Copilot 工具开发代码功能

## Github Copilot 代码开发功能概述

Visual Studio Code 使用 GitHub Copilot 和 GitHub Copilot Chat 这两个扩展来实现 GitHub Copilot 的功能。这些扩展通过生成建议来提高开发人员的工作效率，从而扩展或改进应用程序。

### 核心功能与工具

GitHub Copilot 是一个 AI 配对程序员，但它不是真正的程序员。它能为许多编程语言和各种框架提供代码补全建议。对于 Python、JavaScript、TypeScript、Ruby、Go、C# 和 C++，它的效果尤其有效。

GitHub Copilot 在 Visual Studio Code 中提供的核心功能包括：

+   `GitHub Copilot` 扩展：

+   代码行补全（Code line completion）：使用在编辑器中输入的代码或代码注释生成代码补全建议。

+   `GitHub Copilot Chat` 扩展：

+   聊天视图（Chat View）：提供随时可用的 AI 助手。这是一个更可靠、提供完整聊天体验的界面。

+   快速聊天（Quick Chat）：用于快速提问，然后继续正在进行的工作。

+   内联聊天（Inline Chat）：在编码时，直接在编辑器中启动对话以获取上下文帮助和代码建议。

+   智能操作（Smart Actions）：无需编写提示即可完成某些任务。

---

## 使用 GitHub Copilot 代码补全建议生成代码

GitHub Copilot 会根据所编写代码的上下文生成代码行补全建议。你可以接受、拒绝或部分接受这些建议。

### A. 通过注释生成代码补全

可以通过编写描述要生成的代码的注释来生成代码行补全。

1.  **提供上下文**：GitHub Copilot 基于注释和应用程序的现有上下文生成建议。注释可用于描述代码片段、方法、数据结构和其他代码元素。

1.  **生成建议**：根据代码注释和编辑器中打开的代码文件，GitHub Copilot 会生成一个或多个代码完成建议。它会根据现有代码和注释选择数据类型和变量名称。

1.  **浏览和接受**：

+   当生成多个建议时，可以使用位于“接受”按钮左侧的向左或向右箭头（> 或 <）来循环浏览建议。

+   若要接受建议，请按 `Tab` 键或选择“接受”。

### B. 通过代码生成代码补全

可以通过启动代码行或按 `Enter` 在已补全的代码行后生成代码行补全。

1.  **触发补全**：

+   开始输入代码行，然后等待 GitHub Copilot 为未完成的代码行建议自动补全。

+   输入完整的代码行，按 `Enter` 键，然后等待 GitHub Copilot 为下一个代码行建议自动补全。

1.  **上下文依赖性**：GitHub Copilot 根据输入的代码以及应用程序中代码定义的上下文生成建议。现有代码的数量和质量越高，GitHub Copilot 建议的质量和可靠性也越高。

1.  **擅长模式**：GitHub Copilot 擅长为常见的编程任务和模式生成代码行补全，尤其是在需要生成一系列相关组件时。

## 使用 GitHub Copilot 聊天生成代码

GitHub Copilot Chat 提供了交互式多轮次对话功能，用于提问、获取编码帮助和生成代码。它特别适用于开发实现不熟悉资源（例如新库、框架或 API）的代码。

可以使用 聊天参与者（如 `@workspace`）、斜杠命令（如 `/explain`）和 聊天变量（如 `#file` 或 `#editor`）来阐明提示的上下文和意图，从而生成更准确和有用的响应。

### A. 聊天视图（Chat View）模式生成代码

聊天视图提供了一个管理环境，支持三种主要模式：

1.  **询问模式（`Ask Mode`）**：

+   旨在帮助分析代码、调查新功能的选项，并充当对话助手。

+   可用于询问如何实现新功能或使用特定库。

+   支持 `@workspace` 聊天参与者或 `#codebase` 来提供上下文。

1.  编辑模式（`Edit Mode`）：
