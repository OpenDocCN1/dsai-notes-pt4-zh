
```
import os
from urllib.request import urlopen, Request
import base64
from pathlib import Path
from dotenv import load_dotenv

# Add references
# Add references
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient
from openai import AzureOpenAI

def main(): 

    # Clear the console
    os.system(&#x27;cls&#x27; if os.name==&#x27;nt&#x27; else &#x27;clear&#x27;)
        
    try: 
    
        # Get configuration settings 
        load_dotenv()
        project_endpoint = os.getenv(&quot;PROJECT_CONNECTION&quot;)
        model_deployment =  os.getenv(&quot;MODEL_DEPLOYMENT&quot;)

        # Initialize the project client
        # Initialize the project client
        project_client = AIProjectClient(            
                credential=DefaultAzureCredential(
                    exclude_environment_credential=True,
                    exclude_managed_identity_credential=True
                ),
                endpoint=project_endpoint,
            )

        # Get a chat client
        # Get a chat client
        openai_client = project_client.inference.get_azure_openai_client(api_version=&quot;2024-10-21&quot;)
        
        # Initialize prompts
        system_message = &quot;You are an AI assistant in a grocery store that sells fruit. You provide detailed answers to questions about produce.&quot;
        prompt = &quot;&quot;

        # Loop until the user types &#x27;quit&#x27;
        while True:
            prompt = input(&quot;\nAsk a question about the image\n(or type &#x27;quit&#x27; to exit)\n&quot;)
            if prompt.lower() == &quot;quit&quot;:
                break
            elif len(prompt) == 0:
                    print(&quot;Please enter a question.\n&quot;)
            else:
                print(&quot;Getting a response ...\n&quot;)

                # Get a response to image input
                # Get a response to image input
                image_format = &quot;jpeg&quot;
                request = Request(image_url, headers={&quot;User-Agent&quot;: &quot;Mozilla/5.0&quot;})
                image_data = base64.b64encode(urlopen(request).read()).decode(&quot;utf-8&quot;)
                data_url = f&quot;data:image/{image_format};base64,{image_data}&quot;
                
                response = openai_client.chat.completions.create(
                    model=model_deployment,
                    messages=[
                        {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: system_message},
                        { &quot;role&quot;: &quot;user&quot;, &quot;content&quot;: [  
                            { &quot;type&quot;: &quot;text&quot;, &quot;text&quot;: prompt},
                            { &quot;type&quot;: &quot;image_url&quot;, &quot;image_url&quot;: {&quot;url&quot;: data_url}}
                        ] } 
                    ]
                )
                print(response.choices[0].message.content)
                    
    except Exception as ex:
        print(ex)

if __name__ == &#x27;__main__&#x27;: 
    main()
```

> 代码功能简介：

> 这是一个使用 Azure AI 服务进行图像问答的 Python 程序，主要功能是：

1. 通过 Azure OpenAI 服务分析用户提供的图片（这里是橙子的图片）
2. 回答用户关于图片中水果的问题

> 实现方法：

1. 使用 Azure AI 项目的 Python SDK 连接到 Azure AI 服务
2. 配置环境变量存储连接信息
3. 创建 Azure OpenAI 客户端
4. 设置系统提示词，定义 AI 助手的角色（水果超市助手）
5. 循环接收用户输入的问题
6. 将网络图片转换为 base64 编码的数据 URL
7. 调用 Azure OpenAI 的多模态模型（能处理图片和文本）
8. 返回并显示 AI 对图片问题的回答

> 关键点：

+ 使用了 Azure 的身份认证 `DefaultAzureCredential`
+ 支持多模态输入（文本+图片）
+ 图片通过 URL 获取后转为 base64 格式
+ 对话式交互，直到用户输入 `quit` 退出

输出范例

```
Ask a question about the image
(or type &#x27;quit&#x27; to exit)
Suggest some recipes that include this fruit
Getting a response ...

This orange can be used in various ways in recipes, such as an ingredient in orange juice or marmalades, adding zest and a zest snap into pies or cakes, combining with other fruits like apples or pears in baked desserts, or even in cocktails for a citrus twist.

Ask a question about the image
(or type &#x27;quit&#x27; to exit)
quit
(labenv) PS /home/zunhua/mslearn-ai-vision/Labfiles/gen-ai-vision/python&gt; 
```

# 使用 AI 生成图像

https://learn.microsoft.com/zh-cn/training/modules/generate-images-azure-openai/

## 什么是图像生成模型？

图像生成模型是生成式人工智能（AI）模型，能够根据自然语言输入创建图形数据。简单来说，它们可以根据您提供的文本描述生成全新的、原创的图像，而不是从现有图像库中检索。

## 图像生成模型如何工作？

用户向模型提供自然语言提示（例如，“摩托车上的松鼠”），模型会根据其训练数据生成一张符合该描述的原创图像。它不是一个搜索系统，而是基于所学数据“创造”新的图像。

## Azure AI Foundry 支持哪些图像生成模型？

Azure AI Foundry 支持多种图像生成模型，包括但不限于 DALL-E 3 和 GPT-Image 1。您可以查阅 Azure AI Foundry 门户中的模型目录以获取最新的可用模型信息。

## 如何在应用程序中使用 DALL-E 模型生成图像？

您可以通过 REST API 或特定于语言的 SDK（如 OpenAI Python SDK 或 Azure OpenAI 。NET SDK）在应用程序中使用 DALL-E 模型。您需要向服务终结点提交一个包含授权密钥的请求，请求中包含描述图像生成要求的参数。

## 使用 DALL-E 模型生成图像时可以设置哪些参数？

在使用 DALL-E 模型生成图像时，您可以设置以下参数：

+ `prompt`（提示）：要生成的图像的文字描述。
+ `n`（数量）：要生成的图像数量（DALL-E 3 仅支持 `n=1`）。
+ `size`（尺寸）：生成的图像分辨率（例如 DALL-E 3 支持 1024x1024、1792x1024 或 1024x1792）。
+ `quality`（质量）（可选）：图像的质量（“标准”或“高清”，默认为“标准”）。
+ `style`（样式）（可选）：图像的视觉风格（“自然”或“生动”，默认为“生动”）。

## 生成的图像如何返回给用户？

当使用 DALL-E 3 时，请求结果是同步处理的。响应会包含一个 JSON 对象，其中包含一个 URL 值，该 URL 指向根据提示生成的 PNG 图像文件，用户可以随后查看或下载。响应中通常还会包含一个修订过的提示，这是系统为实现最佳结果而调整过的提示。

## 什么是 Azure AI Foundry 门户中的“映像场”？

“映像场”是 Azure AI Foundry 门户中的一个功能，允许用户创建 Azure AI Foundry 项目，并在此处提交提示来试验图像生成模型并查看生成的图像。

## 在 Azure AI Foundry 门户的“映像场”中可以调整哪些设置？

在使用“映像场”时，您可以调整“设置”来控制输出。例如，在使用 DALL-E 模型时，您可以指定生成的图像分辨率（尺寸）、要生成的图像样式（如“生动”或“自然”）以及图像质量（从“标准”或“高清”中选择）。

## 实验：使用 AI 生成图片

https://github.com/MicrosoftLearning/mslearn-ai-vision/blob/main/Instructions/Labs/09-dall-e.md
