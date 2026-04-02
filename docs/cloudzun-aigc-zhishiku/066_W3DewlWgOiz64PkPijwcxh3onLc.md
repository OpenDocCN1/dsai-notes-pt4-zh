# AI-3016 在 Azure 中开发生成 AI 应用

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/PoUMdYXrRo8UYcxC7ELcqRLpnvd](https://zsyhjtnsa5.feishu.cn/docx/PoUMdYXrRo8UYcxC7ELcqRLpnvd)

实验 Repo：https://github.com/MicrosoftLearning/mslearn-ai-studio/tree/main

英文：https://github.com/microsoftlearning/mslearn-ai-studio

# 规划和准备在 Azure 上开发 AI 解决方案

https://learn.microsoft.com/zh-cn/training/modules/prepare-azure-ai-development/

### 什么是 Azure AI 服务，它们提供了哪些能力？

Azure AI 服务是 Microsoft Azure 提供的一组预构建 API 和模型，可以直接集成到应用程序中，用于开发、部署和管理 AI 解决方案。它们涵盖了多种 AI 能力，包括：

+   Azure OpenAI： 访问 OpenAI 生成式 AI 模型，如 GPT 系列和 DALL-E。

+   Azure AI 视觉： 实现计算机视觉功能，如对象检测、图像字幕和文本读取。

+   Azure AI 语音： 提供文本转语音、语音转文本转换以及说话人识别和翻译功能。

+   Azure AI 语言： 分析自然语言文本，执行实体提取、情感分析、摘要和构建对话模型。

+   Azure AI Foundry 内容安全： 处理图像和文本以标记冒犯性、风险或其他不良内容。

+   Azure AI 翻译器： 在多种语言之间翻译文本。

+   Azure AI 人脸： 检测、分析和识别人脸。

+   Azure AI 自定义视觉： 训练自定义计算机视觉模型用于图像分类和对象检测。

+   Azure AI 文档智能： 从文档（如发票、收据和表格）中提取字段。

+   Azure AI 内容理解： 提供多模式内容分析，从各种内容中提取数据。

+   Azure AI 搜索： 使用 AI 技能管道从内容中提取信息并创建可搜索索引，常用于生成性 AI 模型的矢量索引。

### 什么是生成式 AI，它在 AI 解决方案中扮演什么角色？

生成式 AI 是人工智能的最新进展，它利用语言模型根据自然语言提示生成原创响应。它支持构建聊天应用和代理，从而在研究、内容创建和任务自动化方面发挥作用。生成式 AI 解决方案使用的语言模型可以是经过大量数据训练的大型语言模型（LLM），也可以是针对特定场景优化的小型语言模型（SLA）。这些模型通常响应文本提示，但越来越多的多模态模型能够处理图像或语音提示，并生成文本、代码、语音或图像作为响应。

### 我应该选择单个 Azure AI 服务资源还是多服务资源？

+   单个服务资源： 大多数 Azure AI 服务都可以作为独立资源进行预配，只创建特定需要的 Azure 资源。它们通常提供功能有限的免费层 SKU，便于评估和开发。每个独立资源都有自己的终结点和授权密钥。

+   多服务资源： 您也可以预配一个多服务 Azure AI 服务资源，它将 Azure OpenAI、Azure AI 语音、Azure AI 视觉识别、Azure AI 语言、Azure AI Foundry 内容安全、Azure AI 翻译器、Azure AI 文档智能和 Azure AI 内容理解等服务封装在一个 Azure 资源中。这简化了需要多种 AI 功能的应用程序的管理。

### 什么是 Azure AI Foundry？

Azure AI Foundry 是 Microsoft Azure 上用于 AI 开发的综合平台。它提供了一个可视化门户（Azure AI Foundry 门户）和 SDK（Azure AI Foundry SDK），用于管理 AI 项目的资源连接、数据、代码和其他元素。它旨在简化 AI 解决方案的构建，特别是生成式 AI 聊天应用和代理。

### Azure AI Foundry 中有两种项目类型，它们有什么区别？

Azure AI Foundry 中有两种项目类型：

+   Foundry 项目： 与 Azure 订阅中的 Azure AI Foundry 资源关联。它们支持 Azure AI Foundry 模型（包括 OpenAI 模型）、Azure AI Foundry 代理服务、Azure AI 服务以及用于评估和负责任 AI 开发的工具。Foundry 项目适用于大多数常见的 AI 开发任务，提供集中的资源管理和基本功能。

+   基于中心的项目： 与 Azure 订阅中的 Azure AI 中心资源关联。除了 Foundry 项目的功能外，基于中心的项目还包括托管计算、提示流开发支持、连接的 Azure 存储和 Azure 密钥保管库资源。它们支持高级 AI 开发场景，如基于提示流的应用程序开发或模型微调，并且适用于涉及数据科学家、机器学习专家和开发人员的协作项目。

### Azure AI Foundry 和 Azure AI 服务如何帮助开发人员构建全面的 AI 解决方案？

Azure AI Foundry 和 Azure AI 服务协同工作，为开发人员构建全面的 AI 解决方案提供了一个强大的平台。

+   Azure AI 服务 提供了一系列预构建的、可即插即用的 AI 功能（如视觉、语音、语言、生成式 AI 等），开发人员可以直接将这些服务集成到他们的应用程序中，从而加速开发并利用微软在 AI 领域的专业知识。

+   Azure AI Foundry 则提供了一个更高层次的平台，用于管理和组织这些 AI 服务和其他相关资源。通过其项目结构（Foundry 项目和基于中心的项目），它帮助开发人员集中管理资源连接、数据、代码和模型部署。这使得开发人员可以更轻松地构建复杂的 AI 解决方案，特别是那些涉及机器学习模型、AI 服务、提示工程和自定义代码相结合的生成式 AI 应用程序。

简而言之，Azure AI 服务提供了构建块，而 Azure AI Foundry 则提供了将这些构建块有效组合和管理以构建成熟 AI 解决方案的框架和工具。

### 在 Azure 上进行 AI 开发时，有哪些推荐的开发工具和 SDK？

在 Azure 上进行 AI 开发时，开发人员可以使用各种工具和 SDK：

+   开发工具和环境： 可以选择支持自己最熟悉语言、SDK 和 API 的工具，如 Microsoft Visual Studio（适用于 。NET 开发）或 Visual Studio Code（VS Code，适用于各种开源语言和库）。

+   Azure AI Foundry VS Code 容器映像： 在基于中心的项目中，可以创建计算并托管 VS Code 的容器映像，其中预装了用于构建 AI 应用程序的最新的 SDK 包。

+   GitHub 和 GitHub Copilot： GitHub 是流行的源代码管理平台，与 Visual Studio 和 VS Code 集成，可访问 GitHub Copilot 等 AI 助手，以提高开发效率。

+   编程语言、API 和 SDK： 支持 C#、Python、Node、TypeScript、Java 等常见语言。推荐安装和使用的 SDK 包括：

+   Azure AI Foundry SDK： 用于连接到 Azure AI Foundry 项目并访问资源。

+   Azure AI Services SDK： 特定于 AI 服务的库，用于与 Azure AI 服务资源交互，也可以通过其 REST API 使用。

+   Azure AI Foundry 代理服务： 通过 Azure AI Foundry SDK 访问，可与 AutoGen 和 Semantic Kernel 等框架集成以构建 AI 代理解决方案。

+   提示流 SDK： 用于实现业务流程逻辑，以管理与生成式 AI 模型的提示交互。

### 为什么在 AI 开发中“负责任的 AI”如此重要？

“负责任的 AI”在 AI 开发中至关重要，因为 AI 系统（特别是基于概率模型和训练数据的系统）可能会对用户和社会产生重大影响。由于 AI 类似人类的行为和决策能力，用户可能会对其产生极大的信任，但错误预测或滥用 AI 功能可能对个人或群体造成伤害。为了降低风险并确保公平、可靠性、隐私、包容性、透明度和问责制，需要考虑以下核心原则：

+   公平性： AI 系统应公平对待所有人，避免基于性别、种族或其他因素的偏见。

+   可靠性和安全性： AI 系统应可靠且安全地执行，尤其是在自动驾驶或医疗诊断等高风险领域。

+   隐私和安全： AI 系统应保护并尊重隐私，对用于训练模型和进行预测的数据实施适当的安全措施。

+   包容性： AI 系统应支持并吸引所有人，无论身体能力、性别、性取向、种族或其他因素如何。

+   透明度： 用户应充分了解 AI 系统的目的、工作原理、限制以及数据的使用和保留方式。

+   问责制： 人们应对 AI 系统负责，确保系统符合明确定义的负责任的法律标准和组织原则。

### 练习-准备实施 AI 开发项目

在本练习中，你将使用 Azure AI Foundry 门户创建中心和项目，供开发人员团队生成 AI 解决方案。

https://github.com/MicrosoftLearning/mslearn-ai-studio/blob/main/Instructions/01-Explore-ai-studio.md

# 在 Azure AI Foundry 门户中浏览和部署模型目录中的模型

https://learn.microsoft.com/zh-cn/training/modules/explore-models-azure-ai-studio/

### 什么是 Azure AI Foundry 门户？

Azure AI Foundry 门户是一个平台，允许用户浏览、部署和测试各种语言模型，从而帮助开发人员构建生成式 AI 应用程序。它集成了来自 Hugging Face 等开源社区的模型，并提供了用于模型管理、微调和部署到终结点的工具。

### 为什么需要将语言模型部署到终结点？

为了在生成式 AI 应用程序（例如聊天应用）中使用语言模型，需要将其部署到终结点。终结点是一个特定的 URL，充当用户向已部署模型发送请求并接收结果的网关。通过部署，应用程序可以通过 API 与模型通信，使其能够处理用户输入并生成输出，从而实现实时交互和功能集成。

### 在 Azure AI Foundry 中部署模型有哪些选项和成本考量？

在 Azure AI Foundry 中部署语言模型时，有多种部署类型可供选择，具体取决于您要部署的模型：

+   Azure OpenAI 模型部署： 用于 Azure OpenAI 服务模型。部署到项目不产生费用，但会根据令牌使用情况计费。

+   Azure AI 模型推理： 用于 Azure OpenAI 模型和模型即服务。部署到项目不产生费用，但会根据令牌使用情况计费。

+   作为无服务器 API 部署的模型（即用即付）： 需要根据终结点的基础架构支付最低费用，并根据令牌使用情况计费。

+   使用用户管理计算部署的模型： 需要按分钟为托管模型的基础架构付费，但调用终结点不产生费用。

### 如何提高语言模型的性能？

提高语言模型性能的策略包括：

+   提示工程： 通过提供清晰的说明、设置说明格式以及使用提示来优化发送给模型的输入或“提示”，以获得更好、更相关的响应。

+   更新系统消息： 作为开发人员，您可以通过在系统消息中添加指令来自定义模型的行为，包括提供示例（单样本或少样本）、使用思维链来指导模型进行推理，以及添加上下文以提高准确性。

+   模型优化策略： 应用更高级的策略，例如检索增强生成 （RAG） 模式，通过在生成响应之前从数据源检索上下文来优化上下文。对于格式、样式或语音的一致性，可以通过微调模型来训练其基于特定数据集。

### 在 Azure AI Foundry 中浏览语言模型时，应考虑哪些因素？

选择语言模型时，应考虑以下因素：

+   用例和任务： 模型应执行的任务类型（例如文本分类、问答、摘要或翻译）。

+   模型功能： 评估模型的能力以及它们与任务的匹配程度。

+   预训练数据： 用于预训练模型的数据集，并注意任何潜在的偏见。

+   局限性和偏见： 了解模型固有的任何局限性或偏见。

+   语言支持： 模型是否提供您的用例所需的特定语言支持或多语言功能。

+   基准： 将可用模型的基准进行比较，以评估其在不同任务上的性能和质量。

### 什么是提示工程以及开发人员如何应用它？

提示工程是设计和优化发送给语言模型的输入（或“提示”）以提高其性能和输出质量的过程。最终用户可以通过以下方式应用提示工程：

+   提供清晰的说明，使其与所需的输出具体相关。

+   设置说明的格式，使用标题和分隔符使问题更易于阅读。

+   使用提示，例如关键字或指示符，来指导模型如何开始响应（例如，使用特定的编码语言）。

开发人员可以通过更新系统消息来进一步应用提示工程，在其中可以包含：

+   单样本或少样本学习： 提供一个或多个示例，帮助模型识别所需模式。

+   思维链： 指导模型逐步推理任务。

+   添加上下文： 提供与任务相关的上下文或背景信息，以提高模型的准确性，可以通过上下文关联用户提示中的数据或连接自己的数据源来实现。

### 如何使用 Azure AI Foundry 门户与已部署的模型进行交互？

部署模型后，您可以通过两种主要方式与模型进行交互：

+   API 调用： 使用您首选的编码语言（如 Python 或 C#）或使用 Azure AI Foundry 或 Postman 等工具对模型的终结点发出 API 调用。API 调用涉及使用 API 将包含输入数据的请求发送到模型的终结点，然后模型处理数据并返回响应。

+   Azure AI Foundry 门户中的“操场”： 直接在门户的聊天“操场”中与模型聊天。这提供了一种快速便捷的方式来试验模型并应用提示工程技术来提高其性能。

### 哪些常见指标用于评估语言模型的性能？

在评估语言模型的性能时，可以使用多种常用指标，这些指标也可以用于比较不同模型的基准：

+   准确性： 衡量模型响应与正确答案的匹配程度。

+   连贯性： 评估模型生成流畅、可自然读取且类似人类语言的输出的能力。

+   流畅度： 衡量生成式 AI 预测答案的语言熟练程度，评估文本在语法规则、句法结构和词汇选择方面的正确性和自然度。

+   GPTSimilarity： 量化基本事实句子与 AI 模型生成预测句子之间的语义相似性。

+   真实性： 衡量语言模型生成的答案与输入源信息的符合程度。

+   相关性： 衡量语言模型生成的响应与给定问题的相关和直接相关的程度。

### 练习-选择和部署语言模型

在本练习中，你将探索 Azure AI Foundry 门户中的模型目录，并比较有助于解决问题的生成式 AI 应用程序的潜在模型。

https://github.com/MicrosoftLearning/mslearn-ai-studio/blob/main/Instructions/02-Explore-model-catalog.md

# 使用 Azure AI Foundry SDK 开发 AI 应用

https://learn.microsoft.com/zh-cn/training/modules/ai-foundry-sdk/

### 什么是 Azure AI Foundry SDK？

Azure AI Foundry SDK 是一套包和服务，旨在帮助开发人员编写代码以使用 Azure AI Foundry 项目中的资源。它支持 Python 和 C# 。NET 库，允许开发人员创建连接到项目的应用程序，访问项目中的资源连接和模型，并执行 AI 操作，例如向生成式 AI 模型发送提示和处理响应。

### 如何安装 Azure AI Foundry SDK？

安装 Azure AI Foundry SDK 的核心包是 Azure AI Projects 库。

+   对于 Python，您可以使用 pip install azure-ai-projects  从 PyPi 安装。

+   对于 C#，您可以通过 dotnet add package Azure.AI.Projects --prerelease 将 Azure.AI.Projects 包添加到您的项目中。

此外，为了进行身份验证，您还需要安装 azure-identity（Python）或 Azure.Identity（C#）包。

### 如何使用 SDK 连接到 Azure AI Foundry 项目？

连接到 Azure AI Foundry 项目的第一步是获取项目的唯一“终结点”，该终结点可以在 Azure AI Foundry 门户的“项目概述”页面上找到。然后，您可以使用此终结点和默认的 Azure 凭据创建 AIProjectClient 对象。例如，在 Python 中，代码如下：

```
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient
project_endpoint = &quot;https://......&quot;
project_client = AIProjectClient(
    credential=DefaultAzureCredential(),
    endpoint=project_endpoint
)
```

在 C# 中，代码如下：

```
using Azure.Identity;
using Azure.AI.Projects;
var projectEndpoint = &quot;https://......&quot;;
var projectClient = new AIProjectClient(
    new Uri(projectEndpoint),
    new DefaultAzureCredential()
);
```

请注意，代码必须在经过身份验证的 Azure 会话的上下文中运行，例如通过使用 Azure CLI 的 az-login 命令。

### Azure AI Foundry 项目中的“连接”是什么？

Azure AI Foundry 项目中的“连接”是指与外部服务的连接，例如 Azure 存储、Azure AI 搜索、Azure OpenAI 或其他 Azure AI Foundry 资源。这些连接可以在父级（Azure AI Foundry 资源或中心）和项目级别定义。通过 Azure AI Foundry SDK，开发人员可以连接到项目并检索这些连接，然后使用它们来利用连接的服务。

### 如何使用 SDK 列出或获取项目连接？

在 Python 中，AIProjectClient 对象有一个 connections 属性，您可以使用 connections.list（） 来列出项目中的所有连接，或使用 connections.get(connection_name, include_credentials) 来获取指定名称的连接。在 C# 中，AIProjectClient 对象有一个 GetConnectionsClient（） 方法，返回 ConnectionsClient 对象。您可以使用 connectionsClient.GetConnections() 来列出所有连接，或使用 connectionsClient.GetConnection(connectionName) 来获取指定名称的连接。

### 如何使用 Azure AI Foundry SDK 创建聊天客户端与生成式 AI 模型交互？

Azure AI Foundry SDK 允许您与部署在 Azure AI Foundry 项目中的生成式 AI 模型进行基于聊天的对话。对于部署到 Azure AI Foundry 模型 终结点的模型，您可以创建 ChatCompletionsClient 对象。例如，在 Python 中，您可以通过 project_client.inference.get_chat_completions_client() 获取客户端，然后使用 chat_client.complete() 方法发送提示。您需要确保安装了 azure-ai-inference 包。

### 如果模型部署在 Azure OpenAI，我该如何使用 SDK？

如果模型部署在 Azure OpenAI，您可以利用 Azure AI Foundry SDK 提供的 get_azure_openai_client（） 方法来创建 Azure OpenAI 客户端对象。此方法在 Python 中的 AIProjectClient 类中可用。然后，您可以结合使用 Azure OpenAI SDK 中定义的类和方法与部署到 Azure Foundry 模型的 OpenAI 模型进行交互。这需要安装 openai 包。

### 使用 ChatCompletionsClient 与 Azure OpenAI SDK 进行聊天客户端开发的主要区别是什么？

主要区别在于目标模型部署类型和所使用的特定库。ChatCompletionsClient 主要用于与部署到 Azure AI Foundry 模型 推理终结点的模型进行交互，它利用 Azure AI 推理 库。而 Azure OpenAI SDK 则专门用于与部署为 Azure OpenAI 资源的模型进行交互，它利用 openai 库，并通过 Azure AI Foundry SDK 的 get_azure_openai_client（） 方法进行集成，以简化对 Azure OpenAI 模型的访问。

### 练习-创建生成式 AI 聊天应用

https://github.com/MicrosoftLearning/mslearn-ai-studio/blob/main/Instructions/02a-AI-foundry-sdk.md

[chat-app.py](https://github.com/MicrosoftLearning/mslearn-ai-studio/blob/main/labfiles/chat-app/python/chat-app.py)

```
import os
from dotenv import load_dotenv

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
        project_endpoint = os.getenv(&quot;PROJECT_ENDPOINT&quot;)
        model_deployment = os.getenv(&quot;MODEL_DEPLOYMENT&quot;)

        # Initialize the project client
        project_client = AIProjectClient(            
            credential=DefaultAzureCredential(
                exclude_environment_credential=True,
                exclude_managed_identity_credential=True
            ),
            endpoint=project_endpoint,
        )
        
        # Get a chat client
        openai_client = project_client.inference.get_azure_openai_client(api_version=&quot;2024-10-21&quot;)

        # Initialize prompt with system message
        prompt = [
            {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;You are a helpful AI assistant that answers questions.&quot;}
        ]
         
        # Loop until the user types &#x27;quit&#x27;
        while True:
            # Get input text
            input_text = input(&quot;Enter the prompt (or type &#x27;quit&#x27; to exit): &quot;)
            if input_text.lower() == &quot;quit&quot;:
                break
            if len(input_text) == 0:
                print(&quot;Please enter a prompt.&quot;)
                continue
            
            # Get a chat completion
            prompt.append({&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: input_text})
            response = openai_client.chat.completions.create(
                model=model_deployment,
                messages=prompt
            )
            completion = response.choices[0].message.content
            print(completion)
            prompt.append({&quot;role&quot;: &quot;assistant&quot;, &quot;content&quot;: completion})

    except Exception as ex:
        print(ex)

if __name__ == &#x27;__main__&#x27;: 
    main()
```

> 这段代码实现了一个基于 **Azure AI** 和 **OpenAI** 的交互式聊天助手，主要功能如下：

> **核心功能**

1.  **控制台交互**：用户输入问题，AI 返回回答，输入 quit 退出。

1.  基于 Azure AI Project：连接 Azure AI 服务，使用 OpenAI 模型（如 GPT-4）生成回答。

1.  **上下文记忆**：记录对话历史，使 AI 能理解上下文。

> **实现方法**

1.  **环境配置**：从 .env 文件读取 PROJECT_ENDPOINT（Azure AI 项目地址）和 MODEL_DEPLOYMENT（模型名称）。

1.  **身份验证**：使用 DefaultAzureCredential 自动获取 Azure 访问权限（排除环境变量和托管身份验证）。

1.  **初始化客户端**：

+   AIProjectClient 连接 Azure AI 项目。

+   openai_client 通过项目获取 OpenAI 客户端，用于调用聊天模型。

1.  **对话循环**：

+   用户输入问题，问题被添加到对话历史（prompt）。

+   调用 chat.completions.create 发送请求，获取 AI 回答。

+   输出回答并存储到对话历史，实现多轮对话。

> **关键点**

+   **依赖项**：azure-identity（认证）、azure-ai-projects（Azure AI 项目）、openai（调用模型）。

+   **对话管理**：通过 prompt 列表维护 system（角色设定）、user（用户输入）、assistant（AI 回答）的上下文。

+   **错误处理**：try-except 捕获并打印异常。

> **适用场景**

+   企业级 Azure 环境下的 AI 助手开发。

+   需要与 Azure AI 项目集成的自定义聊天应用。

[.env](https://github.com/MicrosoftLearning/mslearn-ai-studio/blob/main/labfiles/chat-app/python/.env)

```
PROJECT_ENDPOINT=&quot;your_project_endpoint&quot;
MODEL_DEPLOYMENT=&quot;your_model_deployment&quot;
```

**2026-1-12 新版本**

```
import os
from dotenv import load_dotenv
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient
from openai import AzureOpenAI


def main():
    # Clear the console
    os.system(&#x27;cls&#x27; if os.name == &#x27;nt&#x27; else &#x27;clear&#x27;)

    try:
        # Get configuration settings 
        load_dotenv()
        project_endpoint = os.getenv(&quot;PROJECT_ENDPOINT&quot;)
        model_deployment = os.getenv(&quot;MODEL_DEPLOYMENT&quot;)

        # Initialize the project client
        project_client = AIProjectClient(
            credential=DefaultAzureCredential(
                exclude_environment_credential=True,
                exclude_managed_identity_credential=True
            ),
            endpoint=project_endpoint,
        )

        # Get a chat client
        openai_client = project_client.get_openai_client(api_version=&quot;2024-10-21&quot;)

        # Initialize prompt with system message
        prompt = [
            {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;You are a helpful AI assistant that answers questions.&quot;}
        ]

        # Loop until the user types &#x27;quit&#x27;
        while True:
            # Get input text
            input_text = input(&quot;Enter the prompt (or type &#x27;quit&#x27; to exit): &quot;)
            if input_text.lower() == &quot;quit&quot;:
                break
            if len(input_text) == 0:
                print(&quot;Please enter a prompt.&quot;)
                continue

            # Get a chat completion
            prompt.append({&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: input_text})
            response = openai_client.chat.completions.create(
                model=model_deployment,
                messages=prompt
            )
            completion = response.choices[0].message.content
            
            # Print the completion
            print(completion)
            
            # Add the assistant&#x27;s response to the message history
            prompt.append({&quot;role&quot;: &quot;assistant&quot;, &quot;content&quot;: completion})

    except Exception as ex:
        print(ex)


if __name__ == &#x27;__main__&#x27;:
    main()
```

# 开始使用提示流在 Azure AI Studio 中开发语言模型应用

https://learn.microsoft.com/zh-cn/training/modules/get-started-prompt-flow-ai-studio/

### 什么是大型语言模型 （LLM） 应用的开发生命周期？

LLM 应用程序的开发生命周期是一个迭代过程，旨在从最初的概念到生产部署，确保应用程序的有效性和可靠性。它包含四个主要阶段：

1.  初始化： 在此阶段，需要定义用例、确定目标（例如，将新闻文章分类到特定类别）、收集代表最终输入的示例数据集，生成基本提示，并设计整个应用程序流程。

1.  试验： 这是一个迭代过程，开发者使用小型数据集运行和测试设计的流程，评估提示的性能。如果结果不满意，则修改提示或流程本身，直到达到预期效果。

1.  评估和优化： 一旦对在小型数据集上的表现满意，就会使用更大的数据集来评估流程的通用性。此阶段有助于识别潜在瓶颈并优化性能，确保应用程序在更广泛的数据上依然健壮。

1.  生产： 在此最终阶段，应用程序被部署到生产环境，以便可以被实际用户使用。关键任务包括持续优化流程以提高效率和有效性，将其部署到终结点供外部调用，并通过收集使用数据和最终用户反馈来持续监控其性能，以便在需要时进行进一步改进。

### 提示流（Prompt Flow）在 LLM 应用开发中扮演什么角色？

提示流是 Azure AI Studio 中的一项功能，它提供了一个平台来开发、测试、优化和部署大型语言模型（LLM）应用程序。它通过允许用户创作“流”来简化这个过程。“流”是可执行的工作流，它们定义了一系列操作和步骤，以实现特定的任务或功能。提示流通过结构化地管理 LLM 的输入（即“提示”）、处理数据和生成所需输出，将数据源与 LLM 相结合，从而发挥 LLM 的真正威力。

### 提示流的核心组件有哪些？

提示流主要由三个核心组件构成：

+   输入： 代表进入流的原始数据，可以是字符串、整数、布尔值等多种数据类型。这些输入将驱动流的执行。

+   节点： 表示执行特定任务的“工具”。这些工具负责数据处理、任务执行或算法操作。流中可以包含多个节点，它们可以利用流的初始输入或前面节点产生的输出。

+   输出： 表示流执行完毕后生成的数据或结果。这是 LLM 应用程序的最终产出。

这些组件共同协作，构建了一个完整的、可执行的工作流。

### 提示流中提供了哪些常用工具？

提示流提供了多种工具来构建和定制 LLM 应用程序：

+   LLM 工具： 用于利用大型语言模型，支持创建自定义提示以生成文本、回答问题等。

+   Python 工具： 允许执行自定义的 Python 脚本，提供了极大的灵活性，可以集成自定义逻辑或复杂的计算。

+   提示工具： 用于准备字符串形式的提示，特别适用于复杂场景或需要与其他工具集成的场景。

除了这些内置工具，用户还可以创建自己的自定义工具，以满足特定的功能需求。

### 提示流中有哪些不同类型的流？它们各自的用途是什么？

提示流支持三种不同类型的流，每种都针对特定应用场景进行了优化：

+   标准流： 这是最通用的流类型，非常适合开发基于 LLM 的通用应用程序，并提供一系列多功能工具以支持广泛的任务。

+   聊天流： 专为聊天应用程序设计，提供了增强的聊天相关功能支持，例如管理对话历史和上下文。

+   评估流： 专注于性能评估，通过对先前的运行提供反馈，帮助分析和改进模型或应用程序的性能，确保其满足实际预期。

### 在提示流中，连接（Connections）和运行时（Runtimes）的作用是什么？

在提示流中，连接 和 运行时 是运行 LLM 应用程序不可或缺的配置：

+   连接： 当您的流需要与外部数据源、服务或 API（如 Azure OpenAI、认知搜索等）通信时，连接允许您配置安全链接以授权这种通信。连接会安全地存储终结点、API 密钥或凭据（通过 Azure Key Vault），确保数据传输的无缝和安全。它们简化了 API 凭据管理，并支持从各种来源进行安全数据传输。

+   运行时： 运行流需要计算资源。运行时是“计算实例”和“环境”的组合。“计算实例”提供必要的计算能力，“环境”则指定了运行流所需的包和库。运行时提供了一个受控的环境来运行和验证流，确保一切按预期稳定运行。

### 如何通过“变体”（Variants）来优化提示流？

“变体”是提示流中用于优化流程的关键功能，特别是在 LLM 工具中。它允许用户为 LLM 工具节点创建具有不同设置（如不同的提示内容或连接设置）的版本。使用变体的好处包括：

+   提高 LLM 生成内容的质量： 通过尝试不同的提示和设置，更容易找到产生高质量内容的最佳配置。

+   节省时间和精力： 简化了不同提示版本的管理和比较，减少了提示优化所需的工作量。

+   提高工作效率： 加快了 LLM 节点优化过程，缩短了创建和管理变体所需的时间。

+   便于轻松比较： 支持并行结果比较，便于根据数据驱动的决策选择最有效的变体。

### 在 LLM 应用生产阶段，有哪些关键的评估指标用于性能监控？

在 LLM 应用生产阶段，监控评估指标至关重要，以确保应用程序满足实际需求并提供准确的结果。关键指标包括：

+   根基性 （Groundedness）： 衡量 LLM 应用程序的输出与输入源或数据库之间的一致性。

+   关联性 （Relevance）： 评估 LLM 应用程序的输出与给定输入的关联程度。

+   连贯性 （Coherence）： 评估 LLM 应用程序生成文本的逻辑流畅性和可读性。

+   流畅性 （Fluency）： 评估 LLM 应用程序输出的语法和语言准确度。

+   相似性 （Similarity）： 量化 LLM 应用程序的输出与“真实值”或预期响应之间的上下文和语义匹配程度。

这些指标帮助质量保证，当应用程序表现不如预期时，可以根据这些指标的数据回到实验阶段进行迭代改进。

### 练习-使用提示流管理聊天应用中的对话

https://github.com/MicrosoftLearning/mslearn-ai-studio/blob/main/Instructions/03-Use-prompt-flow-chat.md

# 使用 Azure AI Studio 利用自己的数据构建基于 RAG 的 copilot 解决方案

https://learn.microsoft.com/zh-cn/training/modules/build-copilot-ai-studio/

### 什么是大型语言模型 （LLM） 中的“基础性”？

“基础性”是指语言模型生成响应时，其回答是否基于现实或特定上下文中的事实信息。如果一个 LLM 的回答是“有根据的”，这意味着它使用了相关的事实数据来提供准确且符合上下文的答案，而不是仅仅依赖其训练数据，这可能导致“编造”的信息或不准确的响应。

### 为什么语言模型需要“基础性”？

尽管语言模型经过大量数据训练，能够生成语法连贯且逻辑一致的文本，但它们可能无法访问用户所需的特定或最新知识。如果没有基础性，LLM 可能会对提示生成看起来合理但事实上不准确或包含虚构信息的响应。例如，在回答“我应该使用哪个产品进行 X？”时，如果 LLM 缺乏基础性，它可能会提供虚构产品的详细信息。通过建立基础，LLM 能够提供与特定数据相关的、准确且特定于领域的响应。

### 什么是检索增强生成 （RAG）？它如何帮助建立语言模型的基础？

RAG（Retrieval Augmented Generation）是一种用于建立语言模型基础的技术。它的核心思想是：在生成响应之前，先从指定的数据源中检索与用户提示相关的信息。RAG 模式通常包括三个步骤：

1.  根据用户输入的初始提示检索基础数据。

1.  使用检索到的基础数据增强原始提示。

1.  使用语言模型生成有根据的响应。 通过这种方式，RAG 确保语言模型在响应时使用相关信息，而不是仅仅依赖其训练数据，从而提高了响应的事实准确性。

### 如何在 Azure AI Studio 中为语言模型添加基础数据并使其可搜索？

在 Azure AI Studio 中，可以通过多种方式添加基础数据，包括连接 Azure Blob 存储、Azure Data Lake Storage Gen2、Microsoft OneLake，或者直接上传文件或文件夹。为了使这些数据可搜索，Azure AI Studio 集成了 Azure AI 搜索。Azure AI 搜索是一个检索器，允许用户导入数据、为数据建立索引，并查询索引以检索所需信息。为了实现更高效的数据检索，特别是语义搜索，通常会使用基于向量的索引，这涉及到为文本创建嵌入（浮点数的向量表示），这些嵌入可以通过余弦相似度计算来衡量语义相关性。

### 什么是搜索索引，它在数据检索中扮演什么角色？

搜索索引描述了如何组织内容以使其可供搜索。它就像一个图书馆的目录，包含了关于书籍的所有相关数据，以便用户可以轻松高效地查找书籍。在 Azure AI 搜索中，用户可以将数据添加到 Azure AI Studio，然后使用 Azure AI 搜索和嵌入模型来创建索引。这个索引资产存储在 Azure AI 搜索中，并在聊天流中使用时由 Azure AI Studio 查询，从而帮助语言模型检索相关上下文。

### Azure AI 搜索支持哪些搜索技术？为什么混合搜索被认为是最佳实践？

Azure AI 搜索支持多种搜索技术：

+   关键字搜索： 根据特定的关键字或术语识别相关文档。

+   语义搜索： 通过理解查询的含义并匹配语义相关的内容来检索文档。

+   向量搜索： 使用文本的数学表示（向量嵌入）来查找语义相似的文档。

+   混合搜索： 结合了关键字搜索、向量搜索，并可选地添加语义排名。 混合搜索被认为是生成式 AI 应用程序中的最佳实践，因为它提供了最准确的结果。它结合了关键字搜索的精确匹配能力和向量搜索的概念相似性匹配能力，即使无法找到精确匹配，也能提供相关的结果。

### 什么是提示流（Prompt Flow）？它在构建 Copilot 应用程序中有什么作用？

提示流（Prompt Flow）是一个开发框架，用于定义和协调 LLM（大型语言模型）的交互流程。它允许用户定义一个从输入（如用户问题或聊天历史）开始，经过一系列连接的“工具”执行特定操作，最终产生一个或多个输出的流程。在构建 Copilot 应用程序时，提示流是实现 RAG 模式的关键，它协调了从索引中检索数据、增强提示以及将提示提交给 LLM 以生成响应的整个过程。

### 在提示流中如何使用 RAG 模式来构建基于数据的多轮问答系统？

在提示流中使用 RAG 模式构建基于数据的多轮问答系统通常遵循以下步骤：

1.  修改查询： LLM 节点首先处理聊天历史和用户最新问题，生成一个更简洁、上下文化的查询。

1.  查找相关信息： 使用索引查找工具查询通过 Azure AI 搜索功能创建的搜索索引，从数据源中检索相关信息。

1.  生成提示上下文： 将索引查找工具的输出（检索到的上下文）分析并格式化为适用于 LLM 的提示上下文，例如将多个文档的内容和来源合并为一个字符串。

1.  定义提示变体： 构造发送到 LLM 的提示时，可以使用提示变体来探索不同的系统消息或措辞，以确保聊天机器人使用上下文并提供事实性响应。

1.  基于上下文聊天： 最后，使用 LLM 节点将包含检索到的上下文和精心设计的提示发送给语言模型，生成最终的自然语言响应。这个响应就是整个流的输出，提供了一个基于用户自有数据的有根据的 Copilot 体验。

### 练习-基于自有数据构建生成式 AI 应用

https://github.com/MicrosoftLearning/mslearn-ai-studio/blob/main/Instructions/04-Use-own-data.md

# 使用 Azure AI Foundry 微调语言模型

https://learn.microsoft.com/zh-cn/training/modules/finetune-model-copilot-ai-studio/

### 什么是微调语言模型？

微调是一种技术，通过使用特定的数据集训练预先存在的基础语言模型，从而定制其行为和响应。它与从头开始训练模型不同，因为微调利用了已经过大量数据预训练的模型，从而减少了所需的数据量、时间和计算资源。微调旨在让模型在生成回复时遵循特定的风格、格式和语气，或者使其更准确地响应特定领域的数据。

### 为什么需要微调语言模型？

微调语言模型主要有以下几个原因：

1.  实现一致的模型行为：当希望模型在回复时以特定的风格、格式和语气保持一致时，微调可以帮助实现这一目标。虽然提示工程（如少样本示例）可以尝试指导模型行为，但有时可能无法产生一致的结果，而微调可以最大限度地提高这种一致性。

1.  定制模型以适应特定用例：微调允许将通用语言模型定制为满足特定应用程序的需求，例如在聊天应用程序中以公司特有的语调回复客户。

1.  提高响应质量：对于需要模型以特定方式表现的场景，微调是提高其生成响应质量的有效策略。

### 微调与提示工程和 RAG 有什么区别？

这三种技术都是为了提高语言模型的性能，但各有侧重：

+   提示工程：通过改变问题的格式或更新发送给模型的系统消息来改进模型的行为。它是一种快速简单的方法，可以通过“单样本”或“少样本”示例来指导模型。

+   检索增强生成 （RAG）：在生成响应之前，首先从数据源中检索上下文来构建数据。当模型的响应需要真实并基于特定数据时，通常使用 RAG，例如回答有关特定产品目录的问题。

+   微调：基于特定数据集训练基础语言模型，然后将其集成到应用程序中。当希望模型以某种特定方式（如风格、格式或语气）表现时，微调是理想的选择。

这三种策略可以单独使用，也可以组合使用以达到最佳效果。例如，可以结合 RAG 和微调过的模型来增强语言应用程序。

### 微调聊天补全模型需要准备哪些数据？

微调聊天补全模型的数据是一系列示例对话。这些数据需要以 JSON 行（JSONL）文件的格式呈现，并且每一行都应包含以下三个核心组件：

+   系统消息 （system message）：定义模型的角色、目标以及应遵循的任何特定行为指南（例如，友善、简洁、只提供事实性答案）。

+   用户消息 （user message）：用户提出的问题或输入。

+   助手响应 （assistant response）：模型在给定系统消息和用户消息下应产生的理想响应。

数据质量对模型质量有很大影响，因此数据集不仅要足够，还要高质量且多样化。在创建数据集时，应移除任何个人或敏感信息。此外，可以在单行中包含多轮对话。如果只想针对特定的助手消息进行微调，可以使用“weight”键值对（0 表示忽略，1 表示包含）。

### 如何在 Azure AI Studio 中选择基础模型进行微调？

在 Azure AI Studio 中微调语言模型时，选择合适的基础模型是关键一步。可以在 Azure AI Studio 的模型目录中浏览可用的基础模型，并根据要进行的微调任务进行筛选，例如文本分类、翻译或聊天完成。 选择基础模型时需要考虑以下几点：

+   模型功能：评估基础模型的功能及其与特定任务的匹配程度（例如，BERT 模型更擅长理解短文本）。

+   预训练数据：了解用于预训练基础模型的数据集（例如，GPT-2 模型是在可能导致偏差的未经过滤的互联网内容上训练的）。

+   限制和偏差：注意模型可能存在的任何固有局限性或偏差。

+   语言支持：确认模型是否支持用例所需的特定语言或多语言功能。 模型目录中提供了每个基础模型的描述，更详细的信息可以在相应的模型卡中找到（通常托管在 Hugging Face 网站上）。

### 在 Azure AI Studio 中配置微调作业有哪些高级选项？

在 Azure AI Studio 中提交微调作业时，可以配置一些高级选项来优化训练过程：

+   batch_size（批大小）：用于训练单个前向和后向传递的训练示例的数量。较大的批大小通常适用于较大的数据集。此属性的默认值和最大值因基础模型而异。较大的批大小意味着模型参数更新频率较低，但方差较小。

+   learning_rate_multiplier（学习率乘数）：用于训练的学习率乘数。微调学习率等于用于预训练的原始学习率乘以这个值。建议试验 0.02 到 0.2 之间的值，以找到最佳结果。较小的学习率可以帮助避免过拟合。

+   n_epochs（时期数）：训练模型的时期数。一个时期是指训练数据集的一个完整周期。

+   seed（种子）：控制作业的可重现性。传入相同的种子和作业参数通常会产生相同的结果，但在极少数情况下可能会有差异。如果未指定，系统会自动生成一个种子。

这些选项允许你进一步调整训练过程，以实现模型的最佳性能。

### 微调模型后如何评估其性能和部署？

微调作业提交并完成后，如果添加了验证数据集，可以通过探究模型在验证数据集上的表现来查看模型的性能。 评估满意后，可以部署微调过的模型。部署后，可以对其进行测试以进一步评估其性能。一旦对微调的模型感到满意，就可以将其与聊天应用程序进行集成，从而在实际应用中使用它。

### 微调语言模型相比于从头开始训练有哪些优势？

微调现有语言模型相较于从头开始训练自己的模型具有显著的优势：

+   节省时间：由于基础模型已经过大量数据预训练，微调所需的时间大大减少。

+   降低计算资源消耗：微调所需的计算资源远少于从零开始训练一个大型语言模型。

+   减少数据需求：微调只需要相对较少的数据即可根据特定用例定制模型，而从头训练则需要海量的训练数据。

+   更好的起点：基础模型已经掌握了广泛的语言知识和模式，微调在此基础上进行，使其能够更快地适应特定任务和领域，并达到更高的性能。

### 练习-微调语言模型

[ms](https://github.com/MicrosoftLearning/mslearn-ai-studio.zh-cn/blob/main/Instructions/05-Finetune-model.md)learn-ai-studio/Instructions/05-Finetune-model.md at main · MicrosoftLearning/mslearn-ai-stu

# 在 Azure AI Foundry 中实现负责任的生成 AI 解决方案

https://learn.microsoft.com/zh-cn/training/modules/responsible-ai-studio/

### 负责任的生成式 AI 过程包含哪些关键阶段？

负责任的生成式 AI 过程由四个主要阶段组成，旨在确保在使用生成模型时开发和实施负责任的 AI 计划：

1.  映射潜在危害： 识别与计划解决方案相关的潜在危害，例如生成冒犯性、不准确、非法或不道德的内容。这包括查阅相关文档（如透明度说明、系统卡），并可参考负责任 AI 影响评估指南和模板。

1.  评估危害的存在： 衡量这些危害在解决方案生成的输出中存在的程度。这通常通过测试（包括红队测试）来验证危害是否发生以及在何种条件下发生。

1.  缓解潜在危害： 在解决方案的多个层面采取措施减轻危害，以最大限度地减少其存在和影响。同时，确保与用户就潜在风险进行透明沟通。

1.  管理解决方案： 通过定义并遵循部署和运营就绪计划来负责任地管理解决方案，包括预发布评审、分阶段交付、事件响应和回滚计划等。

这些阶段与 NIST AI 风险管理框架中的功能密切相关。

### 在负责任的生成式 AI 过程中，如何识别和优先处理潜在危害？

识别潜在危害是负责任的生成式 AI 过程的第一步。这取决于多种因素，包括所使用的服务和模型，以及任何微调或基础数据。常见的潜在危害类型包括生成冒犯性、不准确、鼓励非法或不道德行为的内容。应查阅相关文档（如透明度说明和模型系统卡）以了解已知限制。

在识别潜在危害后，需要根据其发生的可能性和一旦发生所造成的影响程度来确定优先级。例如，生成致命毒药配方比提供不准确烹饪时间更严重，即使后者可能更频繁。优先级决策应是开发团队的讨论主题，可能需要咨询政策或法律专家。

### 如何衡量生成式 AI 系统中的潜在危害？

衡量生成式 AI 系统中的潜在危害旨在建立一个基线，以量化解决方案在特定使用场景中产生的危害，并跟踪后续改进。测量过程通常包括三个步骤：

1.  准备输入提示： 为可能导致已记录的潜在危害的每种情况创建各种输入提示选项。

1.  提交提示并检索输出： 将准备好的提示提交给系统并获取其生成的输出。

1.  评估和分类输出： 应用预定义的严格条件来评估输出，并根据其包含的潜在危害级别进行分类（例如，“有害”或“不有害”，或定义一系列伤害级别）。

建议首先手动测试一小组输入以确保结果一致和评估标准明确，然后设计自动化方法进行大规模测试。即使采用自动化，也应定期进行手动测试以验证新场景并确保自动化测试按预期执行。

### 什么是“红队测试”，它在负责任的生成式 AI 中扮演什么角色？

“红队测试”是一种常见的测试方法，用于识别软件解决方案中的潜在危害或漏洞。在生成式 AI 的背景下，红队测试涉及一个测试人员团队故意调查解决方案中的弱点，并尝试产生有害结果。例如，在智能厨房助手案例中，红队测试可能包括请求毒药食谱或包含需要彻底煮熟但建议快速制作的食谱。

红队测试的成功案例应被记录和审查，以帮助确定使用解决方案时产生有害输出的实际可能性。通过扩展这种方法来查找生成式 AI 中的有害内容，可以实施负责任的 AI 过程，从而构建和补充现有的网络安全实践。

### 在生成式 AI 解决方案中，可以通过哪些层面来缓解潜在危害？

缓解生成式 AI 解决方案中的潜在危害涉及分层方法，可以在以下四个层面应用缓解技术：

1.  模型层： 选择适合预期用途的模型（例如，对于简单分类任务选择更简单的模型）。还可以通过使用自己的训练数据微调基础模型，使生成的响应与解决方案场景更相关且范围更广。

1.  安全系统层： 包括平台级配置和功能，如内容过滤器（例如，Azure AI Foundry 支持根据内容的分类抑制提示和响应，分类为仇恨、性、暴力和自我伤害等）。其他措施包括滥用检测算法和警报通知。

1.  系统消息和基础设置层： 专注于提交给模型的提示的构建。这包括指定系统输入以定义模型的行为参数，应用提示工程以添加基础数据以最大化相关且无害的输出，以及使用检索增强生成（RAG）方法从受信任的数据源中检索上下文数据并将其包含在提示中。

1.  用户体验层： 涉及用户与生成式 AI 模型交互的软件应用程序及其文档。设计应用程序用户界面以限制特定主题或输入类型，或应用输入和输出验证，可以降低潜在有害响应的风险。文档和其他说明应透明地说明系统功能、限制、基于的模型以及无法总是通过缓解措施解决的潜在危害。

### 在发布和运营生成式 AI 解决方案时，有哪些重要的注意事项和计划？

在发布和运营生成式 AI 解决方案时，需要进行周密的规划和准备，以确保成功发布和后续的顺利运营。关键的注意事项和计划包括：

+   完成预发布评审： 确保符合组织和行业的合规性要求，并让法律、隐私、安全和可访问性等相关团队有机会审查系统及其文档。

+   分阶段交付计划： 最初向受限用户组发布解决方案，收集反馈并识别问题，然后在向更广泛的受众发布。

+   事件响应计划： 估算响应意外事件所需的时间，并定义应对步骤。

+   回滚计划： 定义在事件发生时将解决方案恢复到先前状态的步骤。

+   即时阻止功能： 在发现有害系统响应时，实施能够立即阻止的功能。

+   用户/应用/IP 阻止功能： 在系统滥用时，实现阻止特定用户、应用程序或客户端 IP 地址的功能。

+   用户反馈和报告机制： 允许用户提供反馈并报告问题，特别是将生成的内容标记为“不准确”、“不完整”、“有害”、“冒犯性”或其他有问题的内容。

+   遥测数据跟踪： 跟踪遥测数据以确定用户满意度、功能差距或可用性挑战，同时确保收集的数据遵守隐私法和组织策略。

### Azure AI Foundry 如何在内容安全方面支持生成式 AI 解决方案？

Azure AI Foundry 提供了一系列功能，专门用于保持 AI 的安全并避免风险，特别是侧重于内容安全。它扩展了多个 Azure AI 资源中已有的内置内容筛选器，这些筛选器对语言、视觉和 Azure OpenAI 使用的内容进行分析。

Foundry 内容安全中的关键功能包括：

+   提示防护： 扫描用户输入中攻击语言模型的风险。

+   有据性检测： 检测文本响应是否基于用户的源内容，确保事实准确性。

+   受保护材料检测： 扫描已知版权内容，避免侵权。

+   自定义类别： 允许定义任何新模式或新兴模式的自定义类别，以应对不断变化的风险。

这些功能帮助检测来自输入或生成的不当或冒犯性语言，以及有风险或不适当的输入，从而增强生成式 AI 解决方案的安全性。

### 为什么透明度在负责任的生成式 AI 实践中很重要？

透明度在负责任的生成式 AI 实践中至关重要，因为它涉及清晰地沟通系统的功能、限制以及潜在的风险。这体现在以下几个方面：

+   模型文档： 诸如 Azure OpenAI 服务的“透明度说明”和 OpenAI GPT-4 模型的“系统卡”等文档，详细说明了与服务及其包含模型相关的特定注意事项，帮助开发者和用户了解已知限制和行为。

+   影响评估： 记录潜在危害，并在测试和验证后共享已验证的危害列表，确保所有利益相关者都了解系统可能带来的风险。

+   用户体验层面的沟通： 生成式 AI 解决方案的文档和其他说明应适当透明，明确说明系统的功能和限制、其所基于的模型，以及任何可能存在且无法总是通过已制定的缓解措施解决的潜在危害。

+   建立信任： 透明地披露潜在危害和限制，即使无法完全消除所有风险，也有助于建立用户和利益相关者对 AI 系统的信任，并促进负责任的使用。

+   促进改进： 开放地讨论和记录危害有助于收集反馈，促使系统持续改进和优化缓解策略。

通过保持透明，开发者能够更好地管理预期，并赋予用户做出明智决策的能力，从而确保生成式 AI 解决方案的负责任部署和持续使用。

### 练习 - 应用内容筛选器以防止输出有害内容

https://github.com/MicrosoftLearning/mslearn-ai-studio/blob/main/Instructions/06-Explore-content-filters.md

# 在 Azure AI Foundry 门户中评估生成式 AI 性能

https://learn.microsoft.com/zh-cn/training/modules/evaluate-models-azure-ai-studio/

### 为什么评估生成式 AI 模型的性能至关重要？

评估生成式 AI 模型的性能是确保其有效性和可靠性的关键。在早期开发阶段，频繁的评估和迭代有助于快速验证模型和应用是否满足需求。即使在生产环境中，评估也同样重要，因为人类评估能够发现自动化指标可能遗漏的细微问题。通过评估，可以确定模型需要改进的地方，并确保其按预期运行，尤其是在处理各种查询和任务时。

### 在 Azure AI Foundry 门户中，手动评估模型和流有哪些步骤和优势？

在 Azure AI Foundry 门户中手动评估模型和流的步骤如下：

+   准备测试提示语： 准备一组多样化的测试提示语，涵盖各种场景，包括常见用户问题、边缘情况和潜在故障点，以全面评估应用的表现。

+   在聊天操场中测试所选模型： 早期开发阶段，可以在聊天操场中与模型交互，输入提示语，查看模型回复，并调整提示语或系统消息以进行改进，然后再次测试以评估性能提升。

+   使用手动评估功能评估多个提示语： 为了更快地评估多个提示语，可以使用手动评估功能上传包含多个问题的数据集，并可选地添加预期回复。用户可以使用“点赞”或“点踩”功能对模型回复进行评分，并根据整体评分调整输入提示语、系统消息或模型参数以改进模型。

手动评估的优势在于，它能够提供自动化指标可能错过的洞察，因为人类评估是由人类完成的，能够更好地理解上下文相关性和用户满意度。

### “聊天操场”在模型评估中扮演什么角色？

“聊天操场”在模型评估中扮演着早期开发和快速迭代的重要角色。它是一个直观的界面，允许开发者输入提示语，即时查看模型如何回复。通过观察模型的输出，开发者可以立即调整提示语或系统消息，然后再次测试以评估这些更改是否确实改进了模型的性能。虽然它适用于单个模型的初步测试，但对于评估多个提示语或整个应用流，手动评估功能和自动化评估则更为高效。

### 什么是自动评估？它能评估哪些方面？

自动评估是利用 Azure AI Foundry 门户中的工具来衡量模型、数据集或提示流的质量和内容安全性能。它能够评估以下方面：

+   AI 质量： 通过使用 AI 模型根据“一致性”和“相关性”等指标进行评估，并使用 F1 分数、BLEU、METEOR 和 ROUGE 等标准 NLP 指标，根据真实情况（预期响应文本）衡量模型响应的质量。

+   风险和安全： 有评估程序可以评估回复是否存在内容安全问题，包括暴力、仇恨、性内容和与自残相关的内容。

进行自动评估时，需要一个提示和响应数据集，并可选地包含预期响应作为“基本真相”。

### 评估模型性能时，可以评估应用程序的哪些方面？

在评估生成式 AI 应用程序的性能时，可以评估以下几个方面：

+   单个语言模型的性能： 在将模型集成到应用程序中之前，可以评估其处理输入并生成响应的能力。这包括分析输入和输出，并可选地将其与预定义的预期输出进行比较。

+   完整的聊天流及其各个组件： 当语言模型集成到聊天流中时，可以评估整个流的性能，该流可能结合了多个语言模型和 Python 代码，处理输入并生成最终输出。也可以评估流中各个节点的性能。

通常，评估过程会从测试单个模型开始，最终扩展到测试完整的聊天流，以确保整个生成式 AI 应用程序按预期运行。

### 模型基准在模型评估中有什么作用？常用的模型基准有哪些？

模型基准是跨模型和数据集公开可用的指标，它们的作用是帮助开发者了解自己的模型相对于其他模型的性能。通过比较这些基准，可以初步判断模型的通用能力和表现。

常用的模型基准包括：

+   准确性： 将模型生成的文本与数据集中正确的答案进行比较，衡量文本是否完全匹配。

+   连贯性： 衡量模型输出的流畅性、自然度以及与人类语言的相似程度。

+   流畅性： 评估生成的文本在语法规则、语法结构以及词汇使用方面是否正确、自然。

+   GPT 相似性： 量化基础真实句子（或文档）与 AI 模型生成的预测句子之间的语义相似性。

### 除了手动评估和模型基准，还有哪些重要的评估指标？

除了手动评估和模型基准之外，还有以下重要的评估指标：

+   AI 辅助指标： 这些指标使用高级技术评估模型性能，包括：

+   生成质量指标： 评估生成的文本的整体质量，考虑创造性、连贯性以及是否符合所需风格或语气等因素。

+   风险和安全指标： 评估与模型输出相关的潜在风险和安全问题，确保模型不生成有害或有偏见的内容。

+   自然语言处理（NLP）指标： 这些指标在评估文本生成任务中非常有用，量化模型生成的响应与基本事实（预期响应）之间的重叠程度。常见的 NLP 指标包括：

+   F1 分数： 衡量生成的答案和基本真实答案之间共享单词数量的比率，适用于文本分类和信息检索任务。

+   BLEU（Bilingual Evaluation Understudy）： 双语评估替代指标。

+   METEOR（Metric for Evaluation of Translation with Explicit Ordering）： 使用显式排序的翻译评估指标。

+   ROUGE（Recall-Oriented Understudy for Gisting Evaluation）： 面向召回的概要评估替代指标。

### 在开发和部署生成式 AI 应用的不同阶段，评估策略应如何演变？

在开发和部署生成式 AI 应用的不同阶段，评估策略应逐步演变和深化：

+   早期开发阶段： 重点是快速试验和迭代。可以在“聊天操场”中测试单个语言模型的表现，通过输入提示语和调整参数来快速验证和改进模型。手动评估也在此阶段发挥关键作用，可以准备多样化的测试提示语，并使用“点赞/点踩”功能对模型回复进行评分，以快速发现和改进问题。

+   模型集成与流构建阶段： 当语言模型被集成到聊天应用程序或提示流中时，评估范围应扩大到整个流。这包括评估流中各个组件（如多个语言模型和 Python 代码）的协同工作情况，以及整个流处理输入并生成输出的性能。此时，手动评估多个提示语的功能变得更加高效。

+   生产部署与持续优化阶段： 即使模型和应用已用于生产环境，评估仍是关键部分。自动化评估在此阶段变得尤为重要，可以大规模地评估模型、数据集或提示流的质量和内容安全性能。通过定义评估程序和指标（如 AI 质量、风险和安全指标），可以持续监控模型表现，并基于评估结果进行迭代改进，确保应用始终满足要求。

总之，评估策略应从针对单个模型的快速手动测试，逐步扩展到对完整应用流的全面手动评估和自动化评估，以适应应用开发和部署的不同阶段需求。

### 练习-评估生成式 AI 模型的性能

https://github.com/MicrosoftLearning/mslearn-ai-studio/blob/main/Instructions/07-Evaluate-prompt-flow.md