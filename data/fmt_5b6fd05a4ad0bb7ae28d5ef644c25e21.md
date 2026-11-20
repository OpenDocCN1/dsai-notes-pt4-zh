
初始请求的结果不会立即返回映像生成过程的结果。相反，响应包含一个操作位置标头，其中包含回调服务的 URL，应用程序代码可以轮询该 URL，直到图像生成的结果准备就绪。

操作成功后，将返回类似于以下 JSON 的响应：

`"url": "https://dalleproduse.....png"`

```json
{
    "created": 1686780744,
    "expires": 1686867152,
    "id": "6d765598-eeee-4f49-885d-03ee1c8f9b02",
    "result": {
        "created": 1686780744,
        "data": [
            {
            }
        ]
    },
    "status": "succeeded"
}
```

结果元素包括 `url` 元素的集合，每个元素都引用从提示生成的 PNG 图像文件。

## 练习 - 使用 DALL-E 模型生成图像

`https://microsoftlearning.github.io/mslearn-openai/Instructions/Exercises/05-generate-images.html`

Azure OpenAI 服务包括一个名为 DALL-E 的图像生成模型。您可以使用该模型提交自然语言提示，描述所需的图像，模型将根据您提供的描述生成一张原创图像。

按照手册完成的代码

```python
import requests
import time
import os
from dotenv import load_dotenv

def main(): 
        
    try:
        # Get Azure OpenAI Service settings
        load_dotenv()
        api_base = os.getenv("AZURE_OAI_ENDPOINT")
        api_key = os.getenv("AZURE_OAI_KEY")
        api_version = '2023-06-01-preview'
        
        # Get prompt for image to be generated
        prompt = input("\nEnter a prompt to request an image: ")

        # Make the initial call to start the job
        url = "{}openai/images/generations:submit?api-version={}".format(api_base, api_version)
        headers= { "api-key": api_key, "Content-Type": "application/json" }
        body = {
            "prompt": prompt,
            "n": 1,
            "size": "512x512"
        }
        submission = requests.post(url, headers=headers, json=body)

        # Get the operation-location URL for the callback
        operation_location = submission.headers['Operation-Location']

        # Poll the callback URL until the job has succeeeded
        status = ""
        while (status != "succeeded"):
            time.sleep(3) # wait 3 seconds to avoid rate limit
            response = requests.get(operation_location, headers=headers)
            status = response.json()['status']

        # Get the results
        image_url = response.json()['result']['data'][0]['url']

        # Display the URL for the generated image
        print(image_url)
        

    except Exception as ex:
        print(ex)

if __name__ == '__main__': 
    main()
```

> 这段 Python 代码是用来调用 Azure OpenAI 服务生成图像的。以下是其主要功能的概述：
>
> 1. 环境变量加载：使用 `dotenv` 库加载环境变量，这些环境变量包括 Azure OpenAI 的端点和密钥。
>
> 1. 用户交互：通过 `input()` 函数获取用户输入的提示，这个提示将被用来生成图像。
>
> 1. 初始化图像生成任务：构建一个 `POST` 请求，包含 API 的 URL、头部信息和请求体，然后发送这个请求到 Azure OpenAI 服务。请求体中包含了用户输入的提示、生成图像的数量和图像的大小。
>
> 1. 获取操作位置：从 `POST` 请求的响应头中获取操作位置（`Operation-Location`），这个位置将被用来查询任务的状态。
>
> 1. 轮询任务状态：通过一个 `while` 循环，定期发送 `GET` 请求到操作位置，查询任务的状态。如果任务的状态是 `succeeded`，则跳出循环。
>
> 1. 获取和打印图像 URL：从 `GET` 请求的响应体中获取生成的图像的 `URL`，然后打印这个 `URL`。
>
> 1. 异常处理：如果在执行上述任务过程中出现任何异常，该异常会被捕获并打印出来。
>
> 这个脚本的主要目的是利用 Azure OpenAI 的能力来生成图像。

## 将自己的数据与 Azure OpenAI 服务配合使用

### 了解如何使用自己的数据

基于数据的 Azure OpenAI 支持开发人员使用受支持的 AI 聊天模型来引用特定信息源以使响应落地。 通过添加此信息，模型可以引用提供的特定数据及其预先训练的知识，从而提供更有效的响应。

对自己的数据使用 Azure OpenAI，可利用 Azure 认知搜索的搜索功能将相关数据块添加到提示。 在将数据放入认知搜索索引后，基于数据的 Azure OpenAI 会执行以下步骤：

1. 接收用户提示。
1. 确定提示的相关内容和意图。
1. 使用该内容和意向查询搜索索引。
1. 将搜索结果数据块与系统消息和用户提示一起插入到 Azure OpenAI 提示中。
1. 将完整的提示发送到 Azure OpenAI。
1. 将响应和数据引用（如果有）返回给用户。

默认情况下，基于数据的 Azure OpenAI 鼓励（但不要求）模型仅使用你的数据做出响应。 连接数据时，可以取消选择此设置，这可能会导致模型选择使用其预先训练的知识，而不是你的数据。

### 微调与使用自己的数据

微调是一种用于通过使用其他训练数据的数据集训练现有基础模型（例如 `gpt-35-turbo`）来创建自定义模型的技术。 微调可以产生更高质量的请求，其单独处理提升工程、基于大于提示可以容纳的内容示例自定义模型并让用户能在提供更少示例的情况下获得相同的高质量响应。 但微调的处理过程既昂贵又耗时，应仅在必要时采用。

基于数据的 Azure OpenAI 还是使用无状态 API 连接到模型，这样就无需使用数据训练自定义模型，并简化了与 AI 模型的交互。 认知搜索首先查找有用的信息来回答提示，Azure OpenAI 则基于该信息产生响应。

### 添加自己的数据源

在聊天操场中通过 Azure AI Studio 添加自己的数据。 然后使用添加的数据源来增强发送到模型的提示。 添加数据时，可以选择上传数据文件、使用 `Blob` 存储帐户中的数据或连接到现有的认知搜索索引。

如果要上传或使用存储帐户中已有的文件，则 Azure OpenAI 支持的数据格式包括 `.md`、`.txt`、`.html`、`.pdf` 以及 Microsoft Word 和 PowerPoint 文件。 如果其中任何文件包含图形或图像，则响应质量取决于对视觉内容提取文本的效果。

上传数据或连接到存储帐户中的文件时，建议使用 Azure AI Studio 创建搜索资源和[索引](https://learn.microsoft.com/zh-cn/azure/cognitive-services/openai/concepts/use-your-data#ingesting-your-data-into-azure-cognitive-search?azure-portal=true)，通过此方式添加数据，可以在插入索引时进行适当的分块，从而产生更好的响应。 如果使用大型文本文件或表单，则应使用可用的数据准备脚本来提高 AI 模型的准确性。

为认知搜索服务启用[语义搜索](https://learn.microsoft.com/zh-cn/azure/search/semantic-search-overview)可以优化搜索数据索引的结果，并且可能会收到更高质量的响应和引文。 但启用语义搜索可能会增加搜索服务的成本。

#### 连接数据

要连接数据，请导航到 Azure AI Studio 中的聊天操场，然后在“助手设置”窗格中选择“添加数据”选项卡。 选择“添加数据源”按钮以连接数据。 系统提示将引导你设置与每个数据源之间的连接，并将该数据放入搜索索引。

如果使用的是自己的索引（不是通过 Azure AI Studio 创建的索引），则能通过其中一个页面指定列映射。 请务必提供准确的字段（尤其是内容数据），让模型能够提供更好的响应

### 使用自己的数据与模型聊天

可以在 Azure AI Studio 中通过聊天操场或 API 使用基于你自己数据的 Azure OpenAI。

#### 令牌注意事项和建议的设置

由于基于数据的 Azure OpenAI 在提示符中包含索引的搜索结果，因此请务必了解这会如何影响令牌分配。 每次调用模型都包括系统消息、用户提示、对话历史记录、检索的搜索文档、内部提示和模型响应的令牌。

例如，系统消息是模型指令的有用参考，并且包含在每次调用中。 虽然系统消息没有令牌限制，但使用自己的数据时，如果超过 200 个令牌，系统消息将被截断。 使用自己的数据为 1500 个令牌时，模型的响应也会受到限制。

[由于这些令牌](https://learn.microsoft.com/zh-cn/azure/cognitive-services/openai/concepts/advanced-prompt-engineering)限制，建议在调用中限制问题长度和对话历史记录长度。 提示工程技术（例如分解任务和思维提示链）可以帮助模型更有效地响应。

#### 使用 API

将 API 与你自己的数据配合使用时，需要指定存储数据的数据源。 每次调用时，都需要包含认知搜索资源的 `endpoint`、`key` 和 `indexName`。

请求正文将类似于以下 JSON。

```json
{
    "dataSources": [
        {
            "type": "AzureCognitiveSearch",
            "parameters": {
                "endpoint": "<your_search_endpoint>",
                "key": "<your_search_endpoint>",
                "indexName": "<your_search_index>"
            }
        }
    ],
    "messages":[
        {
            "role": "system", 
            "content": "You are a helpful assistant assisting users with travel recommendations."
        },
        {
            "role": "user", 
            "content": "I want to go to New York. Where should I stay?"
        }
    ]
}
```

### 练习：将自己的数据与 Azure OpenAI 服务配合使用

`https://microsoftlearning.github.io/mslearn-openai/Instructions/Exercises/06-use-own-data.html`

Azure OpenAI 服务使您能够将自己的数据与底层 LLM 的智能相结合使用。您可以限制模型仅使用与相关主题有关的数据，或者将其与预训练模型的结果混合使用。

playground 的效果：

使用自己数据前

![](img/b0c50de2658ad89f0634776112c55478.png)

使用自己数据后

![](img/da3df566e776c87cc41a86b3490f6590.png)

![](img/f38f69df7bfc026a3d0a62662e378409.png)

![](img/20fcc4c56a8807a20a119da90ae26117.png)

完整功能代码
