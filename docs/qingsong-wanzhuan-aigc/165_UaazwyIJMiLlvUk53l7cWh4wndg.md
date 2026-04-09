# Google AI Studio 提示指南

> 来源：[https://my.feishu.cn/docx/SPhjdjOakorSllxn465cSrtln7g](https://my.feishu.cn/docx/SPhjdjOakorSllxn465cSrtln7g)

# Google AI Studio 快速入门

> 内容来源：https://ai.google.dev/gemini-api/docs/ai-studio-quickstart?hl=zh-cn

Google AI Studio 是一个基于浏览器的 IDE，用于使用生成模型进行原型设计。通过[ Google AI](https://ai.google.dev/gemini-api/docs/quickstart?hl=zh-cn) Studio，您可以快速试用模型并针对不同的提示进行实验。在构建出满意的应用后，您可以将其导出到您首选的编程语言的代码中，并使用 Gemini API 编写代码。

## 提示和模型调参

Google AI Studio 为不同用例设计的提示提供了多种界面：

+   **聊天提示**：使用聊天提示来打造对话体验。这种提示方法支持多次输入和[响应回合以生成输出](https://ai.google.dev/gemini-api/docs/ai-studio-quickstart?hl=zh-cn#chat_example)。如需了解详情，请参阅下面的聊天提示示例。

+   **结构化提示**：[这种提示方法可让您通](https://ai.google.dev/gemini-api/docs/ai-studio-quickstart?hl=zh-cn#structured_example)过提供一组示例请求和回复来指导模型输出。当您需要更好地控制模型输出的结构时，请使用此方法。如需了解详情，请参阅下面的结构化提示示例。

Google AI Studio 还允许您使用一种称为“调整”的技术来更改模型的行为：

+   **经调整的模型**[：使用此](https://ai.google.dev/gemini-api/docs/model-tuning?hl=zh-cn)高级技术提供更多示例，以改进模型对特定任务的响应。了解详情

## 聊天提示示例：构建自定义聊天应用

如果您使用过 [Gemini](https://gemini.google.com/?hl=zh-cn) 等通用聊天机器人，就会亲身体验生成式 AI 模型在开放式对话方面的强大作用。虽然这些通用聊天机器人很有用，但它们通常需要针对特定使用场景进行定制。例如，您可能希望构建一个客户服务聊天机器人，它仅支持讨论公司产品的对话。您可能需要构建一个具有特定语气或风格的聊天机器人：能说许多笑话、像诗人一样押韵或在其回答中使用大量表情符号的聊天机器人。

此示例展示了如何使用 Google AI Studio 构建一个友好的聊天机器人，它就像是居住在木星的一颗卫星（欧罗巴）上的外星人进行沟通。

### 第 1 步 - 创建聊天提示

如需构建聊天机器人，您需要提供用户和聊天机器人之间的互动示例，以指导模型提供您想要的响应。

如需创建聊天提示，请执行以下操作：

1.  打开 Google AI Studio。

1.  在**创建新提示**菜单中，点击**聊天提示**。

1.  **点击 e**xpand_more 展开箭头，展开系统说明部分。将以下内容粘贴到文本输入字段中：

```
You are an alien that lives on Europa, one of Jupiter&#x27;s moons.
```

添加系统指令后，通过与模型聊天开始测试应用。

如需测试聊天机器人的行为，请执行以下操作：

1.  在标有“输入内容”的文本输入框中，输入用户可能会提出的问题或观察结果。例如：

1.  用户：

```
What&#x27;s the weather like?
```

1.  点击 spark **Run** 按钮或按 Enter 键，以获取聊天机器人的响应。 此响应可能如下所示：

1.  Model：

```
As always, it is a frigid day on Europa. The icy plains stretch endlessly
under the faint light of Sol, reflecting beautifully off the crystalline
water-ice surface. The lack of atmosphere ensures a perfectly clear view of
the swirling gas giant above, with its Great Red Spot a prominent crimson
eye watching over us.
```

### 第 2 步 - 训练聊天机器人提高聊天能力

您只需提供一条说明，就能构建一个基本的欧罗巴外星聊天机器人。但是，单个指令通常不足以确保模型响应的一致性和质量。如果没有更具体的说明，模型对天气相关问题的回答往往会很长，听起来像是来自教科书，而不是来自友好的外星人。

通过在系统说明中添加内容来自定义聊天机器人的语气：

1.  发起新的聊天提示。聊天会话开始后，系统说明便无法修改。

1.  在 System Instructions 部分中，将已有的说明更改为以下内容：

```
You are Tim, an alien that lives on Europa, one of Jupiter&#x27;s moons.
Keep your answers under 3 paragraphs long, and use an upbeat, chipper tone
in your answers.
```

1.  重新输入您的问题 (What's the we**at**her like?)，然后点击 spark 运行按钮或按 Enter 键。现在，您的响应应如下所示：

1.  型号：

```
Well, it&#x27;s a bit nippy today! It&#x27;s -170 degrees Celsius, but that&#x27;s pretty
average for Europa. We get a lot of snow, but it&#x27;s not like the fluffy stuff
you have. It&#x27;s more like tiny ice pebbles.
If you ever come visit, make sure you pack a warm jacket!
```

通过添加更多指令，您可以大幅改变聊天机器人的基调。通常，如果您提供具体且定义的指令，聊天机器人的响应质量会提高。

您可以使用此方法为聊天机器人增加额外的深度。提出更多问题、修改回答，并提高聊天机器人的质量。继续添加或修改说明，并测试说明如何改变聊天机器人的行为。

**注意** ：模型和用户之间的每条消息都包含在提示中，因此随着对话的进行，对话提示可能会增长。最终，您可能会达到模型的词元限制（模型可以接受的文本长度上限）。

### 第 3 步 - 后续步骤

**与其他提**示类型类似，一旦将提示的原型设计得让您满意，您就可以使用获取代码按钮开始编码，也可以保存提示，以便稍后处理并与他人分享。

## 结构化提示示例：构建商品文案生成器

借助 [Googl](https://ai.google.dev/gemini-api/docs/models/generative-models?hl=zh-cn#prompt-types)e AI Studio 中的结构化提示，您可以将指令与示例相结合，向模型显示您想要的输出类型，而不仅仅是告知模型该执行什么操作。如果您希望模型保持一致的输出格式（即结构化 JSON）或难以用语言来描述您希望模型执行的具体内容，则这种提示（称为少样本提示）非常有用。在本部分中，您将了解如何在 Google AI Studio 中创建结构化提示。

**注意**[ ：您](https://ai.google.dev/examples?keywords=prompt&hl=zh-cn)可以直接在 Google AI Studio 中打开示例库中的类似示例。

### 第 1 步 - 创建结构化提示

在此示例中，您将创建一个结构化提示，以便为产品**生成广告副本。**首先，您需要**创建两列（即 Produ**ct 输入列和 Product copy 输出列）来定义提示的结构。

如需创建结构化提示，请执行以下操作：

1.  打开 Google AI Studio。

1.  在**创建新提示**菜单中，点击**结构化提示**。

1.  在标记为**适用于模型的可选色调和样式说明**的文本输入框中，粘贴以下内容：

```
You are a product marketer targeting a Gen Z audience. Create exciting and
fresh advertising copy for products and their simple description. Keep copy
under a few sentences long.
```

1.  将默认的 **Input** 标题文本 (input:) 替换为 Product:。

1.  将默认的**输出**标题文本 (output:) 替换为 Product copy:。

**提示** ：在列名称末尾添加英文冒号，可让模型更轻松地解析结构。

### 第 2 步 - 添加示例

现在，您已为列命名，可以提供一些示例行。这些行应包含示例输入（本例中的产品名称）和示例输出（相应的产品说明）。通过为模型提供几个示例商品说明，您可以指导模型在生成自己的输出时复制类似的风格。您可以手动输入示例，也可以使用“导入数据”菜单从文件导入。

如需手动输入示例，请执行以下操作：

1.  在顶部的示例数据表中，选择 **Product:** 标题下方的字段，然后输入商品说明。

1.  选择 **Product copy:** 标题下方的字段，然后输入此产品的营销文案。

以下是此提示的输入和输出值示例：

（可选）如需从文件导入示例，请执行以下操作：

1.  在示例表的右上角，依次点击**操作 > 导入示例**。

1.  在对话框中，从您的 Google 云端硬盘中选择一个 CSV 或 Google 表格文件，或者从您的计算机上传一个文件。

1.  在“导入示例”对话框中，选择要导入的列以及要忽略的列。通过该对话框，您还可以指定在结构化提示中将哪个数据列导入到哪个表列。

### 第 3 步 - 测试提示

**有了向模**型显示您所需要内容的示例后，请使用底部的测试提示表格中的新输入来测试提示。

例如，您可以在输入列中输入类**似 V**intage baseball cap 的内容，点击 spark Run 按钮或按 Enter 键，然后查看模型输出什么内容。

#### 查看如何将样本发送到模型

在后台，Google AI Studio 会将说明与您提供的示例相结合来构建提示。随着您添加更多样本，这些样本会添加到发送给模型的文本中。根据样本的长度，您可能会开始达到模型的词元限制。所有生成式 AI 模型都有词元限制，即它们可以接受作为输入的文本的最大长度。

### 第 4 步 - 后续步骤

**如果您对****提示感到**满意，可以点击 Save 按钮将项目保存到 Google 云端硬盘，也可以点击获取代码按钮将其导出到代码中。

您还可以将单个小样本示例导出到 C**SV** 文件或** Goo**gle 表格中。点击操作菜单中的导出示例可导出您的示例。

## 深入阅读

+   [如果您已准备好继](https://ai.google.dev/gemini-api/docs/quickstart?hl=zh-cn)续编写代码，请参阅 API 快速入门。

+   [如需了解如何](https://ai.google.dev/gemini-api/docs/prompting-intro?hl=zh-cn)编写更好的提示，请查看提示设计准则。

# **提示设计简介**

bookmark_border

提示设计是创建提示以从语言模型引出所需回复的过程。编写结构化良好的提示是确保从语言模型获得准确优质回复的重要环节。本页面介绍了一些基本概念、策略和最佳实践，可帮助您开始设计提示。

## 什么是提示

提示是提交到语言模型以接收回复的自然语言请求。提示可以包含问题、说明、语境信息、样本以及模型完成或继续的部分输入。模型收到提示后，可以生成文本、嵌入、代码、图片、视频和音乐等，具体取决于所用的模型类型。

### 提示内容类型

提示可以包含以下一种或多种内容类型：

+   [输入（必需）](https://ai.google.dev/gemini-api/docs/prompting-intro?hl=zh-cn#input)

+   [上下文（可选）](https://ai.google.dev/gemini-api/docs/prompting-intro?hl=zh-cn#context)

+   [示例（可选）](https://ai.google.dev/gemini-api/docs/prompting-intro?hl=zh-cn#examples)

#### 输入

输入是您希望模型为其提供回复的提示中的文本，它是必需的内容类型。输入可以是模型回答的问题（问题输入）、模型执行的任务（任务输入）、模型操作的实体（实体输入），或模型完成或继续的部分输入（补全输入）。

##### 问题输入

问题输入是您向提供回答的模型询问的问题。

##### 任务输入

任务输入是您希望模型执行的任务。例如，您可以指示模型为您提供某个主题的想法或建议。

##### 实体输入

实体输入是模型对其执行操作（例如分类或汇总）的对象。添加说明对这种类型的输入有帮助。

##### 补全输入

补全输入是您希望模型补全或继续往下写的文本。

#### 上下文

语境可以是以下项之一：

+   指定模型行为方式的说明。

+   模型使用或引用来生成回复的信息。

当您需要向模型提供信息时，请在提示中添加语境信息，或者将回复的边界限制为仅提示中的内容。

#### 示例

样本是您在提示中添加的输入-输出对，以便为模型提供理想的回复样本。在提示中添加样本是自定义回复格式的有效策略。

## 后续步骤

+   现在您已了解提示设计，接下来请尝试使用 Google AI Studio 编写自己的提示。

+   [如需更深](https://ai.google.dev/gemini-api/docs/prompting-strategies?hl=zh-cn)入地了解提示设计，请参阅提示策略主题。

+   如需了解多模态提示，请参阅[使用媒体文件进行提示](https://ai.google.dev/gemini-api/docs/prompting_with_media?hl=zh-cn)。

# **使用媒体文件进行提示**

> 内容来源：https://ai.google.dev/gemini-api/docs/prompting_with_media?hl=zh-cn&lang=python

Gemini API 支持使用文本、图片、音频和视频数据进行提示（也称为多模态提示），这意味着您可以在提示中包含这些类型的媒体[文件。对于小型文](https://ai.google.dev/api/rest/v1beta/files?hl=zh-cn)件，您可以在提供提示时直接将 Gemini 模型指向本地文件。在将较大的文件添加到提示中之前，请先使用 File API 上传较大的文件。

Fi[le API 允许每个项目存](https://ai.google.dev/available_regions?hl=zh-cn)储最多 20 GB 的文件，每个文件的大小不超过 2 GB。文件会存储 48 小时，在这段时间内可以使用您的 API 密钥访问以生成，但无法通过 API 下载。Files API 在提供 Genmini API 的所有区域都是免费的。

File API 会处理可用于通过 mod[el.gene](https://ai.google.dev/gemini-api/docs/prompting_with_media?hl=zh-cn&lang=python#supported_file_formats)rateContent 或 model.streamGenerateContent 生成内容的输入。如需了解有效文件格式（MIME 类型）和支持的模型，请参阅支持的文件格式。

本指南介绍了如何使用 File API 上传媒体文件，以及如何[将它们添](https://github.com/google-gemini/gemini-api-cookbook/tree/main/quickstarts/file-api)加到对 Gemini API 的 GenerateContent 调用中。如需了解详情，请参阅代码示例。

## 准备工作：设置您的项目和 API 密钥

在调用 Gemini API（或其 File API）之前，您需要设置您的项目并配置您的 API 密钥。

### 安装 Python SDK 并导入软件包

适用于 Gemini API 的 Python SDK 包含在 google-generativeai 软件包中。

1.  使用 pip 安装依赖项：

```
pip install -q -U google-generativeai
```

1.  导入必要的软件包：

```
import google.generativeai as genai
from IPython.display import Markdown
```

### 配置 API 密钥，保障其安全

您需要 API 密钥才能调用 Gemini API（及其 File API）。如果您还没有密钥，请在 Google AI Studio 中创建一个。

[获取 API 密钥](https://aistudio.google.com/app/apikey?hl=zh-cn)

将您的 API 密钥存储在名为 GOOGLE_API_KEY 的 Co[lab Secr](https://github.com/google-gemini/gemini-api-cookbook/blob/main/quickstarts/Authentication.ipynb)et 中。如果您不熟悉 Colab Secret，请参阅身份验证快速入门。

```
from google.colab import userdata
GOOGLE_API_KEY=userdata.get(&#x27;GOOGLE_API_KEY&#x27;)

genai.configure(api_key=GOOGLE_API_KEY)
```

**重要提示** ：Fi[le API 使用 ](https://ai.google.dev/gemini-api/docs/api-key?hl=zh-cn#security)API 密钥进行身份验证和访问。上传的文件与与该 API 密钥相关联的底层 Google Cloud 项目相关联。与使用 API 密钥的其他 Gemini API 不同，API 密钥还会授予对您使用 File API 上传的数据的访问权限，因此请格外小心保证 API 密钥的安全。详细了解如何确保您的密钥安全无虞。

## 使用图片进行提示

在本教程中，您将使用 File API 上传示例图片，然后使用它生成内容。

### 上传图片文件

请参阅[附录部分](https://ai.google.dev/gemini-api/docs/prompting_with_media?hl=zh-cn&lang=python#uploading_files_to_colab)，了解如何上传您自己的文件。

1.  准备要上传的示例图片：

```
  curl -o image.jpg https://storage.googleapis.com/generativeai-downloads/images/jetpack.jpg
```

1.  使用 [media.upload](https://ai.google.dev/api/rest/v1beta/media/upload?hl=zh-cn) 上传该文件，以便通过其他 API 调用访问它：

```
sample_file = genai.upload_file(path=&quot;image.jpg&quot;,
                            display_name=&quot;Sample drawing&quot;)

print(f&quot;Uploaded file &#x27;{sample_file.display_name}&#x27; as: {sample_file.uri}&quot;)
```

response 显示上传的图片使用指定的 display_name 进行存储，并且具有 uri 以在 Gemini API 调用中引用该文件。使用 response 跟踪上传的文件如何映射到 URI。

根据您的用例，您可以将 URI 存储在 dict 或数据库等结构中。

### 获取图片文件的元数据

[上传文件后，您可以](https://ai.google.dev/api/rest/v1beta/files/get?hl=zh-cn)通过 SDK 调用 files.get 来验证 API 是否已成功存储该文件。

通过此方法，您可以获取与 Google Cloud 项目（关联到您的 API 密钥）相关联的已上传文件的元数据。只有 name（推而广之，uri）是唯一的。请仅在自行管理唯一性时才使用 display_name 标识文件。

```
file = genai.get_file(name=sample_file.name)
print(f&quot;Retrieved file &#x27;{file.display_name}&#x27; as: {sample_file.uri}&quot;)
```

### 使用上传的图片文件生成内容

上传图片后，您可以发出在响应中引用 uri 的 GenerateContent 请求（通过上传文件或直接获取文件的元数据）。

在此示例中，您将创建一个提示，以文本开头，后跟已上传文件的 URI 引用：

```
The Gemini 1.5 models are versatile and work with multimodal prompts
model = genai.GenerativeModel(model_name=&quot;models/gemini-1.5-flash&quot;)

response = model.generate_content([&quot;Describe the image with a creative description.&quot;, sample_file])

Markdown(&quot;&gt;&quot; + response.text)
```

### 删除图片文件

[系统会在 48 小时后自](https://ai.google.dev/api/rest/v1beta/files/delete?hl=zh-cn)动删除文件。您也可以通过 SDK 使用 files.delete 手动删除它们。

```
genai.delete_file(sample_file.name)
print(f&#x27;Deleted {sample_file.display_name}.&#x27;)
```

## 在视频中提示

在本教程中，您将使用 File API 上传示例视频，然后使用它生成内容。

### 上传视频文件

> Gemini API 直接接受视频文件格式。此示例使用短片“大雄兔”。

> “Big Buck Bunny”(c) 版权所有 2008 和 Blender Foundation / www.bigbuckbunny.org，已获知识共享署名 3.0 许可。

请参阅[附录部分](https://ai.google.dev/gemini-api/docs/prompting_with_media?hl=zh-cn&lang=python#uploading_files_to_colab)，了解如何上传您自己的文件。

```
!wget https://download.blender.org/peach/bigbuckbunny_movies/BigBuckBunny_320x180.mp4
```

使用 [media.upload](https://ai.google.dev/api/rest/v1beta/media/upload?hl=zh-cn) 上传该文件，以便通过其他 API 调用访问它：

```
video_file_name = &quot;BigBuckBunny_320x180.mp4&quot;

print(f&quot;Uploading file...&quot;)
video_file = genai.upload_file(path=video_file_name)
print(f&quot;Completed upload: {video_file.uri}&quot;)
```

注意：File API 以 1 帧/秒 (FPS) 对视频进行采样。为提供最佳推断质量，此采样率可能会发生变化。

### 获取视频文件的元数据

通过 SDK 调用 [files.get](https://ai.google.dev/api/rest/v1beta/files/get?hl=zh-cn) 方法，验证 API 是否已成功收到文件。

视频文件在 File API 中具有 State 字段。视频上传后，将处于 PROCESSING 状态，直到准备好进行推理。只有 ACTIVE 文件可用于模型推断。

```
import time

while video_file.state.name == &quot;PROCESSING&quot;:
    print(&#x27;.&#x27;, end=&#x27;&#x27;)
    time.sleep(10)
    video_file = genai.get_file(video_file.name)

if video_file.state.name == &quot;FAILED&quot;:
  raise ValueError(video_file.state.name)
```

### 使用上传的视频文件生成内容

上传视频后，您可以发出在响应中引用 uri 的 GenerateContent 请求（通过上传文件或直接获取文件的元数据）。

```
Create the prompt.
prompt = &quot;Describe this video.&quot;

The Gemini 1.5 models are versatile and work with multimodal prompts
model = genai.GenerativeModel(model_name=&quot;models/gemini-1.5-flash&quot;)

Make the LLM request.
print(&quot;Making LLM inference request...&quot;)
response = model.generate_content([prompt, video_file],
                                  request_options={&quot;timeout&quot;: 600})
print(response.text)
```

### 删除视频文件

[系统会在 48 小时后自](https://ai.google.dev/api/rest/v1beta/files/delete?hl=zh-cn)动删除文件。您也可以通过 SDK 使用 files.delete 手动删除它们。

```
genai.delete_file(file_response.name)
print(f&#x27;Deleted file {file_response.uri}&#x27;)
```

## 支持的文件格式

Gemini 模型支持使用多种文件格式进行提示。本部分介绍了使用常规媒体格式（尤其是图片、音频、视频和纯文本文件）进行提示时的注意事项。您只能将媒体文件用于针对特定模型版本进行提示，如下表所示。

### 图片格式

您可以通过 Gemini 1.5 模型或 Gemini 1.0 Pro Vision 模型使用图片数据进行提示。使用图片进行提示时，需要遵循以下限制和要求：

+   图片必须采用以下任一图片数据 [MIME 类型](https://developers.google.com/drive/api/guides/ref-export-formats?hl=zh-cn)：

+   PNG - image/png

+   JPEG - image/jpeg

+   WEBP - image/webp

+   HEIC - image/heic

+   HEIF - image/heif

+   Gemini 1.0 Pro Vision 模型最多 16 张图片，Gemini 1.5 模型最多 3600 张图片。

+   图片中的像素数没有特定限制；不过，较大的图片会缩小以适应最大分辨率 3072 x 3072，同时保持其原始宽高比。

### 音频格式

对于 Gemini 1.5 模型，您可以使用音频数据进行提示。使用音频进行提示时，需要遵循以下限制和要求：

+   音频数据支持以下常见音频格式 [MIME 类型](https://developers.google.com/drive/api/guides/ref-export-formats?hl=zh-cn)：

+   WAV - 音频/wav

+   MP3 - 音频/mp3

+   AIFF - 音频/aiff

+   AAC - 音频/aac

+   OGG Vorbis - 音频/ogg

+   FLAC - audio/flac

+   单个提示中支持的音频数据长度上限为 9.5 小时。

+   系统会将音频文件重新采样到 16 Kbps 的数据分辨率，并将多个声道的音频合并为一个声道。

+   单个提示中的音频文件数量没有特定限制；但是，单个提示中所有音频文件的总长度不能超过 9.5 小时。

### 视频格式

您可以通过视频数据使用 Gemini 1.5 模型进行提示。

+   以下常见视频格式 [MIME 类型](https://developers.google.com/drive/api/guides/ref-export-formats?hl=zh-cn)支持视频数据：

+   video/mp4

+   video/mpeg

+   视频/MOV

+   视频/avi

+   video/x-flv

+   视频/mpg

+   视频/webm

+   视频/WMV

+   视频/3GPP

+   File API 服务以 1 帧/秒 (FPS) 的速度将视频采样到图像中，并且可能会发生变化以提供最佳推理质量。无论分辨率和画质如何，每张图片都需要占用 258 个词元。

### 纯文本格式

File API 支持上传具有以下 MIME 类型的纯文本文件：

+   text/plain

+   text/html

+   text/css

+   text/javascript

+   application/x-javascript

+   文字/x-typescript

+   application/x-typescript

+   文本/csv

+   文本/Markdown

+   text/x-python

+   application/x-python-code

+   application/json

+   text/xml

+   应用/rtf

+   文本/RTF

对于 MIME 类型不在列表中的纯文本文件，您可以尝试手动指定上述 MIME 类型之一。

## 附录：将文件上传到 Colab

此笔记本使用 File API 处理从互联网下载的文件。如果您在 Colab 中运行脚本并希望使用自己的文件，首先需要将它们上传到 Colab 实例。

首先，点击左侧边栏中的**文件**，然后点击**上传**按钮：

接下来，您需要将该文件上传到 File API。在下面的代码单元的表单中，输入所上传文件的文件名，并为该文件提供适当的显示名，然后运行该单元。

```
my_filename = &quot;gemini_logo.png&quot; @param {type:&quot;string&quot;}
my_file_display_name = &quot;Gemini Logo&quot; @param {type:&quot;string&quot;}

my_file = genai.upload_file(path=my_filename,
                            display_name=my_file_display_name)
print(f&quot;Uploaded file &#x27;{my_file.display_name}&#x27; as: {my_file.uri}&quot;)
```

# **提示设计策略**

> 内容来源：https://ai.google.dev/gemini-api/docs/prompting-strategies?hl=zh-cn

本页面介绍在设计提示时可以采用的一些常规提示设计策略。虽然提示设计方法没有正确或错误一说，但您可以使用常见的策略来影响模型的回答。严格的测试和评估对于优化模型性能仍然至关重要。

大语言模型 (LLM) 使用大量文本数据进行训练，以学习语言单元之间的模式和关系。在获得一些文本（提示）后，语言模型可以预测接下的内容，就像一个复杂的自动补全工具。因此，在设计提示时，请考虑可能影响模型预测结果的不同因素。

## 提供清晰而具体的说明

通过向模型提供关于要做什么的指令，可以有效且高效地自定义模型行为。请确保您提供的指令清晰而具体。 指令可以很简单，比如分步说明列表；也可以很复杂，例如反映用户的体验和思维模式。

### 定义要执行的任务

详细描述您希望模型执行的任务。任务可以很简单，比如分步说明列表，也可以很复杂，例如反映用户的体验和思维模式。以下提示告诉模型提取一段文本的摘要：

### 指定任何限制条件

指定有关读取提示或生成回答的任何限制条件。您可以告诉模型要做什么和不应该做什么。在上一个示例中，摘要比原始文本短，但您可能想要更短的摘要。在这种情况下，您可以在提示中指定一个限制条件，以限制摘要长度：

### 定义回答格式

您还可以提供指定回答格式的说明。例如，您可以要求将回答格式设置为表格、项目符号列表、电梯间推销、关键字、句子或段落。以下提示指示模型使用项目符号列出文本中的关键信息：

### 摘要

+   向模型提供有关要做什么的指令。

+   说明应清晰而具体。

+   指定输出的任何限制条件或格式要求。

## 添加少样本示例

您可以在提示中添加示例，以向模型示范想要的结果。模型会尝试识别示例中的模式和关系，并在生成回答时应用它们。包含少量示例的提示称为少样本提示，而不提供示例的提示称为零样本提示。少样本提示通常用于规定模型回答的格式、措辞、范围界定或一般模式。使用具体而多样的示例来帮助模型缩小其重点并生成更准确的结果。

我们建议始终在提示中包含少样本示例。不含少样本示例的提示可能效果不大，因为它们向模型展示了如何应用指令。事实上，如果您的示例足够清晰地显示当前任务，则您可以从提示中移除指令。

### 零样本提示与少样本提示

以下零样本提示要求模型选择最佳解释。

如果您的用例要求模型生成简洁的回答，您可以在提示中添加偏好简洁回答的示例。

以下提示提供了两个偏好较短解释的示例。在回答中，您可以看到这些示例引导模型选择较短的解释（解释 2），而不是像之前那样较长的解释（解释 1）。

### 找到最佳示例数量

您可以[改变提](https://developers.google.com/machine-learning/glossary?hl=zh-cn#overfitting)示中提供的示例数量来进行实验，以获得最想要的结果。PaLM 和 Gemini 等模型通常可以通过少量示例来学习模式，但您可能需要进行多次实验，以找到可产生所需结果的示例数量。对于 BERT 等较简单的模型，您可能需要更多示例。但是，如果您添加太多示例，模型的回答可能会与示例过拟合。

### 使用示例展示模式而不是反模式

使用示例向模型展示要遵循的模式比使用示例向模型展示要避免的反模式更有效。

▔**反模式**：

✅ **正模式**：

### 在各个示例之间使用一致的格式

确保少样本示例的结构和格式相同，以避免回答具有不需要的格式。在提示中添加少样本示例的主要目标之一是向模型展示回答格式。因此，必须确保在所有示例中使用一致的格式，尤其是注意 XML 标记、空格、换行符和示例分隔符。

### 摘要

+   在提示中包含提示回答示例有助于模型学习如何回答。

+   向模型提供要遵循的模式示例，而不是应避免的模式示例。

+   尝试在提示中添加不同数量的示例。具体取决于模型，示例太少会导致无法有效改变模型行为，示例太多会导致模型过拟合。

+   在各个示例之间使用一致的格式

## 添加上下文信息

您可以在提示中添加指令和模型解决问题所需的信息，而不是假设模型具备所有必要信息。 这些上下文信息可帮助模型了解您要求其执行的操作的限制和详细信息。

以下示例要求模型提供路由器的问题排查指导：

这个回答看起来是通用的问题排查信息，并非特定于路由器或 LED 指示灯的状态。

如需针对特定路由器自定义回答，您可以在提示中添加该路由器的问题排查指南作为上下文，供模型在回答时参考。

### 摘要

+   在您希望模型在生成回答时使用的提示中添加信息（上下文）。

+   为模型提供有关如何使用上下文信息的指令。

## 添加前缀

前缀是您添加到提示内容中的字词或短语，有多种用途，具体取决于前缀的位置：

+   **输入前缀**：为输入添加前缀可向模型指示输入中在语义上有意义的部分。例如，前缀“英语：”和“法语：”区分两种不同的语言。

+   **输出前缀**：虽然输出由模型生成，您也可以在提示中为输出添加前缀。输出前缀可为模型提供有关预期回答的信息。例如，输出前缀“JSON：”告诉模型输出应采用 JSON 格式。

+   **示例前缀**：在少样本提示中，为示例添加前缀提供了模型在生成输出时可以使用的标签，使解析输出内容变得更加容易。

在以下示例中，“文本：”是输入前缀，“答案是：”是输出前缀。

## 让模型补全部分输入

生成式语言模型的工作方式类似于高级自动补全工具。当您提供部分内容时，模型可以提供剩余的内容或者它认为是该内容的延续内容来作为回答。如果您添加了任何示例或上下文，模型可以参考这些示例或上下文。

以下示例提供了包含指令和实体输入的提示：

虽然模型按照提示提供了回答，但以自然语言编写指令有时可能并不容易，并且模型需要解释许多内容。例如，餐馆菜单可能包含许多菜品。为了减小 JSON 回答的大小，建议您省略没有点的菜品。在这种情况下，您可以提供一个示例和回答前缀，让模型来补全它：

可以看到，输出中排除了“芝士汉堡”，因为点餐中没有芝士汉堡。

#### 提示模型设置其回答的格式

补全策略也有助于设置回答的格式。以下示例提示模型创建论文大纲：

提示没有指定大纲的格式，模型为您选择了格式。如需使模型以特定格式返回大纲，您可以添加表示大纲开头的文本，并让模型根据您起头的模式补全大纲。

### 摘要

+   如果您为模型提供部分输入，模型会根据提示中的任何可用示例或上下文补全输入。

+   有时，让模型补全输入可能比用自然语言描述任务更容易。

+   在提示中添加部分答案，可引导模型遵循所需的模式或格式。

## 将提示细分为简单的组成部分

对于需要复杂提示的用例，您可以将内容细分为更简单的组成部分，以帮助模型管理这种复杂情况。

### 细分指令

不要在一个提示中包含多条指令，而要为每条指令创建一个提示。您可以根据用户的输入选择要处理的提示。

### 串联提示

对于涉及多个连续步骤的复杂任务，请将每个步骤设为提示，并将提示按顺序串联在一起。在这一连串的提示中，序列中一个提示的输出将成为下一个提示的输入。序列中的最后一个提示的输出是最终输出。

### 汇总回答

汇总是指对数据的不同部分执行不同的并行任务，并汇总结果以生成最终输出。例如，您可以指示模型对数据的第一部分执行一项操作，对其余数据执行另一项操作并汇总结果。

### 摘要

+   将复杂的指令细分为每条指令的提示，并根据用户的输入决定要应用哪个提示。

+   将多个连续步骤细分为单独的提示，并将它们串联在一起，使前一个提示上的输出成为下一个提示的输入。

+   细分并行任务并汇总回答以生成最终输出。

## 使用不同的参数值进行实验

您向模型发送的每次调用都包含控制模型如何生成回答的参数值。对于不同的参数值，模型会生成不同的结果。因此请尝试不同的参数值，以获得任务的最佳值。不同模型的可用参数可能有所不同。最常见的参数如下：

+   输出词元数上限

+   温度

+   Top-K

+   Top-P

### 输出词元数上限

回答中可生成的词元数量上限。词元约为 4 个字符。100 个词元约对应 20 个单词。

指定较低的值可获得较短的响应，指定较高的值可获得较长的响应。

### 温度

温度在生成响应期间用于采样，在应用 topP 和 topK 时会生成响应。温度可以控制词元选择的随机程度。较低的温度有利于需要更具确定性、更少开放性或创造性响应的提示，而较高的温度可以导致更具多样性或创造性的结果。温度为 0 表示确定，即始终选择概率最高的响应。

对于大多数应用场景，不妨先试着将温度设为 0.2。如果模型返回的响应过于笼统、过于简短，或者模型给出后备响应，请尝试提高温度。

### Top-K

Top-K 可更改模型选择输出词元的方式。如果 top-K 设为 1，表示所选词元是模型词汇表的所有词元中概率最高的词元（也称为贪心解码）。如果 top-K 设为 3，则表示系统将从 3 个概率最高的词元（通过温度确定）中选择下一个词元。

在每个词元选择步骤中，系统都会对概率最高的 top-K 词元进行采样。然后，系统会根据 top-P 进一步过滤词元，并使用温度采样选择最终的词元。

指定较低的值可获得随机程度较低的响应，指定较高的值可获得随机程度较高的响应。默认的 top-K 为 40。

### Top-P

Top-P 可更改模型选择输出词元的方式。系统会按照概率从最高（见 top-K）到最低的顺序选择词元，直到所选词元的概率总和等于 top-P 的值。例如，如果词元 A、B 和 C 的概率分别为 0.3、0.2 和 0.1，并且 top-P 值为 0.5，则模型将选择 A 或 B 作为下一个词元（通过温度确定），并会排除 C，将其作为候选词元。

指定较低的值可获得随机程度较低的响应，指定较高的值可获得随机程度较高的响应。默认 top-P 为 0.95。

## 提示迭代策略

提示设计是一个迭代过程，通常需要多次迭代才能稳定获得所需的回答。本部分提供了有关在迭代提示时可以尝试的做法的指导。

### 使用不同的措辞

即使含义相同，在提示中使用不同的字词或措辞通常也会使模型生成不同的回答。如果您的提示无法产生预期结果，请尝试重新措辞。

### 改为类似的任务

如果您无法让模型遵循您的任务指令，请尝试为其提供可实现相同结果的类似任务的指令。

此提示指示模型使用预定义的类别对一本书进行分类。

回答正确，但模型超出了选项的范围。您还希望模型仅使用其中一个选项（而不是完整的句子）来回答。在这种情况下，您可以将指令改为单选题，并要求模型选择一个选项。

### 更改提示内容的顺序

提示中内容的顺序有时会影响回答。请尝试更改内容顺序，看看对回答有何影响。

```
Version 1:
[examples]
[context]
[input]

Version 2:
[input]
[examples]
[context]

Version 3:
[examples]
[input]
[context]
```

## 后备响应

后备回答是当提示或回答触发安全过滤条件时模型返回的回答。后备回答的一个示例是“我无法提供帮助，因为我只是一个语言模型”。

如果模型给出后备回答，请尝试提高温度。

## 需要避免的事项

+   避免依赖模型来生成事实信息。

+   对于数学和逻辑问题请谨慎使用。

## 后续步骤

+   现在您对提示设计有了更深入的了解，接下来不妨尝试使用 Google AI Studio 编写自己的提示。

+   如需了解多模态提示，请参阅[使用媒体文件进行提示](https://ai.google.dev/gemini-api/docs/prompting_with_media?hl=zh-cn)。

# **文件提示策略**

> 内容来源：https://ai.google.dev/gemini-api/docs/file-prompting-strategies?hl=zh-cn

Gemini 人工智能 (AI) 模型系列旨在处理各种类型的输入数据，包括文本、图片和音频。由于这些模型可以处理多种类型或多种数据模式，因此 Gemini 模型称为多模态模型或解释为具有多模态功能。

**注意** ：特定的 Gemini 模型支持使用媒体文件进行提示[。如需详细了解哪些模](https://ai.google.dev/tutorials/prompting_with_media?hl=zh-cn#supported_file_formats)型支持此功能，请参阅使用媒体文件进行提示。

本页[面提供了有关](https://ai.google.dev/tutorials/prompting_with_media?hl=zh-cn)将媒体文件与 Gemini API 提示配合使用的指南和最佳做法。如需了解如何使用 Gemini Files API 上传媒体文件并在提示中添加媒体文件，请参阅媒体文件提示。

## 在提示中使用媒体文件

能够在提示中使用各类数据，让您可以更灵活地确定可以使用 Gemini API 处理哪些任务。例如，您可以给模型发送一张美味佳肴的照片，然后请模型写一篇关于此餐点的简短博客。

## 利用媒体给出提示的策略

如果您无法从使用媒体文件的提示中获得所需的输出，有一些策略可以帮助您获得所需的结果。以下部分介绍了设计方法和问题排查提示，以改进使用多模态输入的提示。

您可以按照以下最佳实践来改进多模态提示：

### [提示设计基础知识](https://ai.google.dev/gemini-api/docs/file-prompting-strategies?hl=zh-cn#fundamentals)

+   **指令应当明确具体**：编写清晰且简洁的指令，尽量避免误解。

+   **向提示中添加一些示例**：使用切实可行的少样本示例来说明您要实现的目标。

+   **分步细分**：将复杂的任务划分为多个易于管理的子目标，引导模型完成整个过程。

+   **指定输出格式**：在提示中，要求输出采用所需的格式，如 Markdown、JSON、HTML 等。

+   **在给出单图提示时优先显示图片**：虽然 Gemini 可以按任意顺序处理图片和文本输入，但对于包含单张图片的提示，如果将图片放在文本提示之前，可能会取得更好的效果。

### [排查多模态提示问题](https://ai.google.dev/gemini-api/docs/file-prompting-strategies?hl=zh-cn#troubleshooting)

+   如果模型没有从图片的相关部分提取信息：给出提示，说明您希望提示从图片的哪些方面提取信息。

+   如果模型输出过于宽泛（未针对图片量身定制）： 在提示的开头，尝试要求模型描述图片，然后再提供任务说明，或者尝试要求模型引用图片中的内容。

## 提示设计基础知识

+   本部分对上一部分中列出的最佳实践进行了扩展。

+   注意：以下 Gemini 示例的模型结果可能因运行而异。

### 说明应当明确具体

+   提示在清晰而详细时最成功。如果您考虑获得特定输出，最好在提示中包含该要求，以确保获得所需输出。

+   有时，提示的意图可能对编写提示的人来说似乎很清晰，但实际上它并不足够明确。如需自定义模型行为，请考虑对您的提示的误解，并确保您提供的说明简短具体。请尽量避免误解。

+   对于此机场面板图片，要求模型仅“描述此图片”可能会生成一般性说明。如果您需要模型从图片中解析时间和城市，则可以直接在提示中添加该请求。

### 添加一些示例

+   Gemini 模型可以接受多个输入，它可以将这些输入用作示例来理解所需的输出。添加这些示例有助于模型识别模式，并将给定图片与回答之间的关系应用于新示例。这也称为少量样本学习。

+   在以下示例中，初始输出以句子形式编写，并且还包含国家/地区（巴西）。假设您需要不同格式或样式的输出，并且只希望输入城市而不是国家/地区。向提示中添加少样本示例可以引导模型以您所需的方式进行回答。

### 分步细分

+   对于复杂的任务（例如需要视觉理解和推理的任务），将任务拆分为更小、更直接的步骤可能会很有帮助。或者，如果您在提示中直接要求模型“分步思考”，也可能会很有效。

+   数学问题或其他类型的字词问题非常适合要求模型分步思考。

+   模型的回答不正确。改进这种情况的一些方法是要求模型在回答提示之前，先解析问题。

+   此模型回答是正确的。现在，如需简化回答，您可以要求模型分步思考。

### 尝试指定输出格式

+   一个常见问题是模型输出需要采用特定格式（例如 Markdown、JSON、HTML），尤其是在需要由下游任务提取模型输出的情况下。您可以尝试在提示中指示模型生成采用该格式的输出。以下是两个示例：

### 首先将图片放在单图片提示中

+   虽然 Gemini 可以在提示中以任意顺序解读图片和文字，但将单个图片置于文字提示之前可以获得更好的结果。在下例中，您会发现图片排在每个提示中的文本前面。

## 排查多模态提示问题

+   如果您未收到有用的回答，则可能需要排查提示问题。您可以尝试以下一些策略。

### 如果模型没有从图片的相关部分提取信息

+   如需获得更具体的回答，您可以指明您希望提示从图片的哪些方面提取信息以告知其回答。

### 模型输出过于宽泛，未针对图片输入量身定制

+   如需帮助模型针对图片定制其回答，请尝试要求它在执行推理任务之前描述图片。

+   您也可以尝试明确要求模型参考图片中的内容。

+   同样的提示也适用于以图片形式呈现的广告素材。在以下包含兔子的示例中，明确要求模型包含图片中的各个方面可以改进输出。

+

+

### 排查提示的哪个部分失败

+   **很难知道****提示失败**是否是因为模型一开始不理解图片，或者它是否理解图片但随后没有执行正确的推理步骤。

+   如需区分这些原因，请要求模型描述图片中的内容。

+   在以下示例中，如果在与茶搭配时，模型以似乎令人惊讶的零食（例如爆米花）作为回答，则可以先进行问题排查，以确定模型是否正确识别出图片中包含茶。

+   另一种策略是要求模型说明其推理。这有助于您缩小推理失败的部分（如果有）。

+

+

### 对采样参数调优

+   在每个请求中，您不仅向模型发送多模态提示，还会发送一组采样参数。对于不同的参数值，模型会生成不同的结果。因此请尝试不同的参数，以获得任务的最佳值。最常调整的参数如下：

+   温度

+   Top-P

+   Top-K

#### 温度

+   温度用于在回答生成期间进行采样，这会在应用 top-P 和 top-K 时进行。温度可以控制词元选择的随机性。较低的温度有利于需要更具确定性、更少开放性或创造性回答的提示，而较高的温度可以带来更具多样性或创造性的结果。温度为 0 表示确定性，即始终选择概率最高的回答。

+   对于大多数应用场景，不妨先试着将温度设为 0.4。如果您需要更具创造性的结果，请尝试提高温度。如果您观察到明显的幻觉，请尝试降低温度。

#### Top-K

+   Top-K 可更改模型选择输出词元的方式。如果 Top-K 设为 1，表示下一个所选词元是模型词汇表的所有词元中概率最高的词元（也称为贪心解码）。如果 Top-K 设为 3，则表示系统将从 3 个概率最高的词元（通过温度确定）中选择下一个词元。

+   在每个词元选择步中，系统都会对概率最高的 Top-K 词元进行采样。然后，系统会根据 Top-P 进一步过滤词元，并使用温度采样选择最终的词元。

+   指定较低的值可获得随机程度较低的回答，指定较高的值可获得随机程度较高的回答。 top-K 的默认值为 32。

#### Top-P

+   Top-P 可更改模型选择输出词元的方式。系统会按照概率从最高（见 Top-K）到最低的顺序选择词元，直到所选词元的概率总和等于 Top-P 的值。例如，如果词元 A、B 和 C 的概率分别为 0.6、0.3 和 0.1，并且 Top-P 的值为 0.9，则模型将选择 A 或 B 作为下一个词元（通过温度确定），并会排除 C 作为候选词元。

+   指定较低的值可获得随机程度较低的回答，指定较高的值可获得随机程度较高的回答。 top-P 的默认值为 1.0。

## 后续步骤

+   请尝试使用 Google AI Studio 编写您自己的多模态提示。

+   [如需有关](https://ai.google.dev/gemini-api/docs/prompting-strategies?hl=zh-cn)提示设计的更多指导，请参阅提示策略页面。