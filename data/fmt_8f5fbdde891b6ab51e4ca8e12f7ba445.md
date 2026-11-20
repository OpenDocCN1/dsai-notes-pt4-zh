
# Google AI Studio 提示指南

> 来源：[https://my.feishu.cn/docx/SPhjdjOakorSllxn465cSrtln7g](https://my.feishu.cn/docx/SPhjdjOakorSllxn465cSrtln7g)

## Google AI Studio 快速入门

> 内容来源：https://ai.google.dev/gemini-api/docs/ai-studio-quickstart?hl=zh-cn

Google AI Studio 是一个基于浏览器的 IDE，用于使用生成模型进行原型设计。通过 [Google AI Studio](https://ai.google.dev/gemini-api/docs/quickstart?hl=zh-cn)，您可以快速试用模型并针对不同的提示进行实验。在构建出满意的应用后，您可以将其导出到您首选的编程语言的代码中，并使用 Gemini API 编写代码。

### 提示和模型调参

Google AI Studio 为不同用例设计的提示提供了多种界面：

+ 聊天提示：使用聊天提示来打造对话体验。这种提示方法支持多次输入和响应回合以生成输出。如需了解详情，请参阅下面的聊天提示示例。

+ 结构化提示：[这种提示方法可让您通过提供](https://ai.google.dev/gemini-api/docs/ai-studio-quickstart?hl=zh-cn#structured_example)一组示例请求和回复来指导模型输出。当您需要更好地控制模型输出的结构时，请使用此方法。如需了解详情，请参阅下面的结构化提示示例。

Google AI Studio 还允许您使用一种称为“调整”的技术来更改模型的行为：

+ 经调整的模型：[使用此高级技术](https://ai.google.dev/gemini-api/docs/model-tuning?hl=zh-cn)提供更多示例，以改进模型对特定任务的响应。了解详情

### 聊天提示示例：构建自定义聊天应用

如果您使用过 [Gemini](https://gemini.google.com/?hl=zh-cn) 等通用聊天机器人，就会亲身体验生成式 AI 模型在开放式对话方面的强大作用。虽然这些通用聊天机器人很有用，但它们通常需要针对特定使用场景进行定制。例如，您可能希望构建一个客户服务聊天机器人，它仅支持讨论公司产品的对话。您可能需要构建一个具有特定语气或风格的聊天机器人：能说许多笑话、像诗人一样押韵或在其回答中使用大量表情符号的聊天机器人。

此示例展示了如何使用 Google AI Studio 构建一个友好的聊天机器人，它就像是居住在木星的一颗卫星（欧罗巴）上的外星人进行沟通。

#### 第 1 步 - 创建聊天提示

如需构建聊天机器人，您需要提供用户和聊天机器人之间的互动示例，以指导模型提供您想要的响应。

如需创建聊天提示，请执行以下操作：

1. 打开 Google AI Studio。
1. 在创建新提示菜单中，点击聊天提示。
1. 点击 expand_more 展开箭头，展开系统说明部分。将以下内容粘贴到文本输入字段中：

```
You are an alien that lives on Europa, one of Jupiter&#x27;s moons.
```

添加系统指令后，通过与模型聊天开始测试应用。

如需测试聊天机器人的行为，请执行以下操作：

1. 在标有“输入内容”的文本输入框中，输入用户可能会提出的问题或观察结果。例如：
1. 用户：

```
What&#x27;s the weather like?
```

1. 点击 spark Run 按钮或按 Enter 键，以获取聊天机器人的响应。此响应可能如下所示：
1. Model：

```
As always, it is a frigid day on Europa. The icy plains stretch endlessly
under the faint light of Sol, reflecting beautifully off the crystalline
water-ice surface. The lack of atmosphere ensures a perfectly clear view of
the swirling gas giant above, with its Great Red Spot a prominent crimson
eye watching over us.
```

#### 第 2 步 - 训练聊天机器人提高聊天能力

您只需提供一条说明，就能构建一个基本的欧罗巴外星聊天机器人。但是，单个指令通常不足以确保模型响应的一致性和质量。如果没有更具体的说明，模型对天气相关问题的回答往往会很长，听起来像是来自教科书，而不是来自友好的外星人。

通过在系统说明中添加内容来自定义聊天机器人的语气：

1. 发起新的聊天提示。聊天会话开始后，系统说明便无法修改。
1. 在 System Instructions 部分中，将已有的说明更改为以下内容：

```
You are Tim, an alien that lives on Europa, one of Jupiter&#x27;s moons.
Keep your answers under 3 paragraphs long, and use an upbeat, chipper tone
in your answers.
```

1. 重新输入您的问题 (What's the weather like?)，然后点击 spark 运行按钮或按 Enter 键。现在，您的响应应如下所示：
1. 型号：

```
Well, it&#x27;s a bit nippy today! It&#x27;s -170 degrees Celsius, but that&#x27;s pretty
average for Europa. We get a lot of snow, but it&#x27;s not like the fluffy stuff
you have. It&#x27;s more like tiny ice pebbles.
If you ever come visit, make sure you pack a warm jacket!
```

通过添加更多指令，您可以大幅改变聊天机器人的基调。通常，如果您提供具体且定义的指令，聊天机器人的响应质量会提高。

您可以使用此方法为聊天机器人增加额外的深度。提出更多问题、修改回答，并提高聊天机器人的质量。继续添加或修改说明，并测试说明如何改变聊天机器人的行为。

注意：模型和用户之间的每条消息都包含在提示中，因此随着对话的进行，对话提示可能会增长。最终，您可能会达到模型的词元限制（模型可以接受的文本长度上限）。

#### 第 3 步 - 后续步骤

与其他提示类型类似，一旦将提示的原型设计得让您满意，您就可以使用获取代码按钮开始编码，也可以保存提示，以便稍后处理并与他人分享。

### 结构化提示示例：构建商品文案生成器

借助 [Google AI Studio](https://ai.google.dev/gemini-api/docs/models/generative-models?hl=zh-cn#prompt-types) 中的结构化提示，您可以将指令与示例相结合，向模型显示您想要的输出类型，而不仅仅是告知模型该执行什么操作。如果您希望模型保持一致的输出格式（即结构化 JSON）或难以用语言来描述您希望模型执行的具体内容，则这种提示（称为少样本提示）非常有用。在本部分中，您将了解如何在 Google AI Studio 中创建结构化提示。

**注意**：[您可以直接](https://ai.google.dev/examples?keywords=prompt&hl=zh-cn)在 Google AI Studio 中打开示例库中的类似示例。

#### 第 1 步 - 创建结构化提示

在此示例中，您将创建一个结构化提示，以便为产品生成广告副本。首先，您需要创建两列（即 Product 输入列和 Product copy 输出列）来定义提示的结构。

如需创建结构化提示，请执行以下操作：

1. 打开 Google AI Studio。
1. 在创建新提示菜单中，点击结构化提示。
1. 在标记为适用于模型的可选色调和样式说明的文本输入框中，粘贴以下内容：

```
You are a product marketer targeting a Gen Z audience. Create exciting and
fresh advertising copy for products and their simple description. Keep copy
under a few sentences long.
```

1. 将默认的 Input 标题文本 (input:) 替换为 Product:。
1. 将默认的输出标题文本 (output:) 替换为 Product copy:。

提示：在列名称末尾添加英文冒号，可让模型更轻松地解析结构。

#### 第 2 步 - 添加示例

现在，您已为列命名，可以提供一些示例行。这些行应包含示例输入（本例中的产品名称）和示例输出（相应的产品说明）。通过为模型提供几个示例商品说明，您可以指导模型在生成自己的输出时复制类似的风格。您可以手动输入示例，也可以使用“导入数据”菜单从文件导入。

如需手动输入示例，请执行以下操作：

1. 在顶部的示例数据表中，选择 Product: 标题下方的字段，然后输入商品说明。
1. 选择 Product copy: 标题下方的字段，然后输入此产品的营销文案。

以下是此提示的输入和输出值示例：

（可选）如需从文件导入示例，请执行以下操作：

1. 在示例表的右上角，依次点击操作 > 导入示例。
1. 在对话框中，从您的 Google 云端硬盘中选择一个 CSV 或 Google 表格文件，或者从您的计算机上传一个文件。
1. 在“导入示例”对话框中，选择要导入的列以及要忽略的列。通过该对话框，您还可以指定在结构化提示中将哪个数据列导入到哪个表列。

#### 第 3 步 - 测试提示

有了向模型显示您所需要内容的示例后，请使用底部的测试提示表格中的新输入来测试提示。

例如，您可以在输入列中输入类似 Vintage baseball cap 的内容，点击 spark Run 按钮或按 Enter 键，然后查看模型输出什么内容。

##### 查看如何将样本发送到模型

在后台，Google AI Studio 会将说明与您提供的示例相结合来构建提示。随着您添加更多样本，这些样本会添加到发送给模型的文本中。根据样本的长度，您可能会开始达到模型的词元限制。所有生成式 AI 模型都有词元限制，即它们可以接受作为输入的文本的最大长度。

#### 第 4 步 - 后续步骤

如果您对提示感到满意，可以点击 Save 按钮将项目保存到 Google 云端硬盘，也可以点击获取代码按钮将其导出到代码中。

您还可以将单个小样本示例导出到 CSV 文件或 Google 表格中。点击操作菜单中的导出示例可导出您的示例。

### 深入阅读

+ [如果您已准备好继续编写代码](https://ai.google.dev/gemini-api/docs/quickstart?hl=zh-cn)，请参阅 API 快速入门。
+ [如需了解如何编写更好的提示](https://ai.google.dev/gemini-api/docs/prompting-intro?hl=zh-cn)，请查看提示设计准则。

## 提示设计简介

bookmark_border

提示设计是创建提示以从语言模型引出所需回复的过程。编写结构化良好的提示是确保从语言模型获得准确优质回复的重要环节。本页面介绍了一些基本概念、策略和最佳实践，可帮助您开始设计提示。

### 什么是提示

提示是提交到语言模型以接收回复的自然语言请求。提示可以包含问题、说明、语境信息、样本以及模型完成或继续的部分输入。模型收到提示后，可以生成文本、嵌入、代码、图片、视频和音乐等，具体取决于所用的模型类型。

#### 提示内容类型

提示可以包含以下一种或多种内容类型：

+ 输入（必需）
+ 上下文（可选）
+ 示例（可选）

##### 输入

输入是您希望模型为其提供回复的提示中的文本，它是必需的内容类型。输入可以是模型回答的问题（问题输入）、模型执行的任务（任务输入）、模型操作的实体（实体输入），或模型完成或继续的部分输入（补全输入）。

###### 问题输入

问题输入是您向提供回答的模型询问的问题。

###### 任务输入

任务输入是您希望模型执行的任务。例如，您可以指示模型为您提供某个主题的想法或建议。

###### 实体输入

实体输入是模型对其执行操作（例如分类或汇总）的对象。添加说明对这种类型的输入有帮助。

###### 补全输入

补全输入是您希望模型补全或继续往下写的文本。

##### 上下文

语境可以是以下项之一：

+ 指定模型行为方式的说明。
+ 模型使用或引用来生成回复的信息。

当您需要向模型提供信息时，请在提示中添加语境信息，或者将回复的边界限制为仅提示中的内容。

##### 示例

样本是您在提示中添加的输入-输出对，以便为模型提供理想的回复样本。在提示中添加样本是自定义回复格式的有效策略。

### 后续步骤

+ 现在您已了解提示设计，接下来请尝试使用 Google AI Studio 编写自己的提示。
+ [如需更深入地了解提示设计](https://ai.google.dev/gemini-api/docs/prompting-strategies?hl=zh-cn)，请参阅提示策略主题。
+ 如需了解多模态提示，请参阅[使用媒体文件进行提示](https://ai.google.dev/gemini-api/docs/prompting_with_media?hl=zh-cn)。

## 使用媒体文件进行提示

> 内容来源：https://ai.google.dev/gemini-api/docs/prompting_with_media?hl=zh-cn&lang=python

Gemini API 支持使用文本、图片、音频和视频数据进行提示（也称为多模态提示），这意味着您可以在提示中包含这些类型的媒体[文件](https://ai.google.dev/api/rest/v1beta/files?hl=zh-cn)。对于小型文件，您可以在提供提示时直接将 Gemini 模型指向本地文件。在将较大的文件添加到提示中之前，请先使用 File API 上传较大的文件。

[File API](https://ai.google.dev/available_regions?hl=zh-cn) 允许每个项目存储最多 20 GB 的文件，每个文件的大小不超过 2 GB。文件会存储 48 小时，在这段时间内可以使用您的 API 密钥访问以生成，但无法通过 API 下载。Files API 在提供 Gemini API 的所有区域都是免费的。

File API 会处理可用于通过 `model.generateContent` 或 `model.streamGenerateContent` 生成内容的输入。如需了解有效文件格式（MIME 类型）和支持的模型，请参阅支持的文件格式。

本指南介绍了如何使用 File API 上传媒体文件，以及如何[将它们添加到](https://github.com/google-gemini/gemini-api-cookbook/tree/main/quickstarts/file-api)对 Gemini API 的 `GenerateContent` 调用中。如需了解详情，请参阅代码示例。

### 准备工作：设置您的项目和 API 密钥

在调用 Gemini API（或其 File API）之前，您需要设置您的项目并配置您的 API 密钥。

#### 安装 Python SDK 并导入软件包

适用于 Gemini API 的 Python SDK 包含在 `google-generativeai` 软件包中。

1. 使用 `pip` 安装依赖项：

```
pip install -q -U google-generativeai
```

1. 导入必要的软件包：

```
import google.generativeai as genai
from IPython.display import Markdown
```

#### 配置 API 密钥，保障其安全

您需要 API 密钥才能调用 Gemini API（及其 File API）。如果您还没有密钥，请在 Google AI Studio 中创建一个。

[获取 API 密钥](https://aistudio.google.com/app/apikey?hl=zh-cn)
