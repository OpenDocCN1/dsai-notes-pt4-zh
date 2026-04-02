# 解决方案指南：智能 Copilot 堆栈场景

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/LiC6diFXpowsTXxqiToc3B6Vnag](https://zsyhjtnsa5.feishu.cn/docx/LiC6diFXpowsTXxqiToc3B6Vnag)

# 挑战 1：部署 Azure OpenAI 服务和LLM模型

**说明和指导**

挑战 01 旨在帮助学生设置用于应对此挑战的先决条件。其中包括必要的安装、环境选项和所需的其他库。

### 任务 1：部署 Azure OpenAI 服务

在此任务中，需要在 Azure 门户中部署 Azure OpenAI 服务。

1.  在 Azure 门户中，搜索并选择“Azure OpenAI”。

1.  在“Azure AI Services | Azure OpenAI”（Azure AI 服务 | Azure OpenAI）边栏选项卡上，单击“create”（创建），输入所需的详细信息，然后部署 Azure Open AI 服务。

###

### 任务 2：在 Azure OpenAI Studio 中部署 LLM 模型

在此任务中，需要在 Azure OpenAI Studio 中部署 OpenAI 模型。

1.  在 Azure OpenAI 资源窗格中，单击“Go to Azure OpenAI Studio”（转到 Azure OpenAI Studio）；系统将转到 Azure OpenAI Studio。

1.  在“Welcome to Azure OpenAI Service”（欢迎使用 Azure OpenAI 服务）页面上，单击“Create new deployment”（新建部署）。

1.  在“Deployments”（部署）页面中，单击“+ Create new deployment”
（+ 新建部署）。

1.  在**“De**p**loy** model”（部署模型）弹出窗口界面中，输入以下详细信息，然后单击“Advanced options”（高级选项）(3) 并调低“Tokens per Minute Rate Limit (thousands)”（每分钟令牌数速率限制(千)）(4)：

+   “Select a model”（选择模型）：gpt-35-turbo (1)

+   “Model version”（模型版本）：使用默认版本 (2)

+   “Deployment name”（部署名称）：gpt-35-turbo

+   “Tokens per Minute Rate Limit (thousands)”（每分钟令牌数
速率限制(千)）：20K

1.  单击“Create”（创建）按钮，部署后续将使用的模型。

1.  返回“Deployments”（部署）页面，单击“+ Create new deployment”
（+ 新建部署）。

1.  在**“De**p**loy** model”（部署模型）弹出窗口界面中，输入以下详细信息，然后单击“Advanced options”（高级选项）(3) 并调低“Tokens per Minute Rate Limit (thousands)”（每分钟令牌数速率限制(千)）(4)：

+   “Select a model”（选择模型）：text-embedding-ada-002 (1)

+   **“**Model version”（模型版本）：使用默认版本 (2)

+   “Deployment name”（部署名称）：text-embedding-ada-002

+   “Tokens per Minute Rate Limit (thousands)”（每分钟令牌
数速率限制(千)）：20K

1.  单击“Create”（创建）按钮，部署后续将使用的模型。

# 挑战 2：探索 Semantic Kernel - 解决方案指南

## 任务 1：克隆本课程的存储库

如果你尚未将 Semantic Kernel GitHub 存储库代码存储库克隆到此实验中使用的环境，请按照以下步骤操作。否则，请在 Visual Studio Code 中打开克隆的文件夹。

1.  打开 Visual Studio Code。

1.  导航到目录“C:/Users/azureuser”。

```
cd C:/Users/azureuser
```

1.  克隆给定的存储库。

```

```

git clone [https://github.com/micro](https://github.com/microsoft/semantic-kernel)soft/semantic-kernel

```
4. 克隆存储库后，在 Visual Studio Code 中打开文件夹。[GC1] [OC2] [3] 
```

## 任务 2：检索 Azure OpenAI 服务值

需要在 Azure 门户中检索在上一个挑战中部署的 Azure OpenAI 服务密钥、终结点及 LLM 模型名称。

1.  在 Azure 门户中，搜索并选择“Azure OpenAI”。

1.  转到之前创建的 Azure OpenAI 资源，然后从左侧窗格中选择“Keys and Endpoints”（密钥和终结点）。

1.  复制 2 个密钥的任何一个以及终结点，并将其存储在某个位置以供将来参考。

1.  在“Overview”（概述）页面中，单击“Go to Azure OpenAI Studio”（转到 Azure OpenAI Studio），以转到部署的模型。

1.  转到“Deployments”（部署），即可看到已部署的 AI 模型。复制之前部署的 AI 模型的部署名称，并将其存储在某个位置以供将来参考。

## 任务 3：使用 Python 编程语言运行 Jupyter Notebook 以开始使用  Semantic Kernel

在 VS Code 中，逐单元格运行所需的笔记本并观察输出，有助于更好地理解代码。

1.  转到 **VS Code** 中的克隆文件夹，导航到“semantic-kernel/python/notebooks”，
然后创建一个新的“.env”文件。

1.  转到“.env.example”，复制其所有内容，并将这些内容粘贴到新创建的“.env”
文件中。

1.  通过将 Azure OpenAI 模型名称、终结点和密钥的值更新为在之前步骤中检索的值，修改“.env”文件。

实际在这个位置

而且似乎python版本必须是3.10-3.12之间

1.  导航到 00-getting-started.ipynb，逐个单元格运行该笔记本，你将在此过程中导入 Semantic Kernel SDK。

**注意：**跳**过与 Ope**nAI 相关的代码单元格（即笔记本中的“Option 1”（选项 1）），仅运行与 Azure OpenAI 相关的代码单元格，即从笔记本中的“Option 2”（选项 2）开始。

1.  更新“Option 2”（选项 2）中你之前已检索的值“deployment_name”、“endpoint”和“api_key”，并运行该函数。

照常运行其余代码。

1.  成功完成上一个笔记本后，导航到 01-basic-loading-the-kernel.ipynb，逐个单元格运行该笔记本屏幕顶部出现以下提示时，选择“Python <your_version>”。

1.  在以下代码单元格中，更新你之前已检索的值“deployment_name”、“endpoint”和“api_key”，然后执行函数。

1.  成功运行上一个笔记本后，导航到 02-running-prompts-from-file.ipynb，逐个单元格运行该笔记本。屏幕顶部出现以下提示时，选择“Python <your_version>”。

1.  在以下函数中，将“useAzureOpenAI”修改为“True”，设置你之前检索的“deployment_name”、“endpoint”和“api_key”的值，然后执行函数。

照常运行其余代码单元格。

1.  成功完成上一个笔记本后，导航到 03-semantic-function-inline.ipynb，逐个单元格运行该笔记本。屏幕顶部出现以下提示时，选择“Python <your_version>”。
在这里，你需要一个支持文本补全的 OpenAI 模型。为此，你需要导航到 Azure OpenAI Studio，然后部署一个 text-davinci-003 模型，并将 TPM 容量设置为 10K。

1.  在以下函数中，设置你之前检索的 text-davinci-003 模型的“deployment_name”、“endpoint”和“api_key”的值，然后执行函数。

照常运行其余代码单元格。

1.  在以下函数中，将“useAzureOpenAI”修改为“True”，设置你之前检索的“deployment_name”、“endpoint”和“api_key”的值，然后执行函数。

照常运行其余代码单元格。

1.  成功完成上一个笔记本后，导航到 04-context-variables-chat.ipynb，逐个单元格运行该笔记本。屏幕顶部出现以下提示时，选择“Python <your_version>”。

1.  在以下函数中，将“useAzureOpenAI”修改为“True”，设置你之前检索的“deployment_name”、“endpoint”和“api_key”的值，然后执行函数。

照常运行其余代码单元格。输出结果示例如下所示：

## 可选任务

1.  成功运行所需的笔记本后，你可以通过使用 Python 编程语言执行名为 05-using-the-planner.ipynb 的 Jupyter Notebook，从而探索并了解 Semantic Kernel 中的规划器。为此，请导航到 05-using-the-planner.ipynb，逐个单元格运行该笔记本。屏幕顶部出现以下提示时，选择“Python <your_version>”。

1.  在以下函数中，将“useAzureOpenAI”修改为“True”，设置你之前检索的“deployment_name”、“endpoint”和“api_key”的值，然后执行函数。

照常运行其余代码单元格。

1.  成功完成上一个笔记本后，导航到 06-memory-and-embeddings.ipynb，逐个单元格运行该笔记本。屏幕顶部出现以下提示时，选择“Python <your_version>”。

1.  在以下函数中，将“useAzureOpenAI”修改为“True”，针对你的两个模型设置你之前检索的“deployment_name”、“endpoint”和“api_key”的值，然后执行函数。

照常运行其余代码单元格。

# 挑战 3：在本地运行 Chat Copilot 应用 - 解决方案指南

### 任务 1：检索 Azure OpenAI 服务值

需要在 Azure 门户中检索在上一个挑战中部署的 Azure OpenAI 服务密钥、终结点及 LLM 模型名称。

1.  在 Azure 门户中，搜索并选择“Azure OpenAI”。

1.  选择已创建的 Azure OpenAI 资源，然后单击左侧窗格中的“Keys and Endpoints”

（密钥和终结点）。

1.  复制密钥和终结点，并将其存储在记事本中。

1.  在“Overview”（概述）页面上，单击“Go to Azure OpenAI Studio”（转到 Azure OpenAI Studio）。

1.  在左侧导航窗格中，导航到“Deployments”（部署），复制之前部署的 AI 模型的部署名称，并将其存储在记事本中。

### 任务 2：克隆 Chat-Copilot GitHub 存储库

如果你尚未将 Chat-Copilot GitHub 存储库克隆到此实验中使用的环境，请按照以下步骤操作。否则，请在Visual Studio Code 中打开克隆的文件夹。

1.  以管理员身份打开 PowerShell。

1.  导航到目录 C:/Users/azureuser。

cd C:/Users/azureuser

1.  克隆 GitHub 存储库。

git clone [https://github.com/mi](https://github.com/microsoft/chat-copilot)crosoft/chat-copilot

1.  打开 Visual Studio Code，然后依次单击“File”（文件）>“Open folder”（打开文件夹）。

1.  选择“CHAT-COPILOT”，并查看文件。

### 任务 3：设置环境

1.  在本地计算机上以管理员身份打开 PowerShell。你需要安装 PowerShell Core 6+，

这与 Windows 上安装的默认 PowerShell 不同。

1.  通过使用以下命令导航到 chat-copilot 的脚本目录来设置环境：

cd C:\Users\azureuser\chat-copilot\scripts\

1.  运行以下命令以安装 Chocolatey、dotnet-7.0-sdk、nodejs 和 yarn：

.\Install.ps1

**注意：**如果收到脚本未进行数字签名或无法在系统上执行的错误，则可能需要更改执行策略或解除对脚本的限制。

### 任务 4：在本地配置和运行 Chat Copilot 应用

1.  通过运行以下命令配置 Chat Copilot：

.\Configure.ps1 -AIService {AI_SERVICE} -APIKey {API_KEY} -Endpoint {AZURE_OPENAI_ENDPOINT} -CompletionModel {DEPLOYMENT_NAME} -EmbeddingModel {DEPLOYMENT_NAME} -PlannerModel {DEPLOYMENT_NAME}

**注意：**提供在之前步骤中记录的 Azure OpenAI 服务名称、密钥、终结点以及已部署的模型名称。

1.  最后，通过执行以下命令在本地运行 Chat Copilot：

.\Start.ps1

**注意：**此步骤将启动后端 API 和前端应用程序。首次运行时，可能需要几分钟时间才能安装 Yarn 包。

**注意：**如果发生错误，请按照以下步骤操作。

1.  依次转到“chat-copilot”>“scripts”>“Start-Frontend.ps1”以运行 yarn 命令。

1.  依次转到“chat-copilot”>“scripts”>“Start-Backend.ps1”以运行 dotnet 命令。

注意：完成后，导航到脚本目录并再次运行启动命令。

1.  你将获得类似如下所示的前端输出：

1.  你将获得类似如下所示的后端输出：

# 挑战 4：了解和自定义 Chat Copilot 应用 - 解决方案指南

## 任务 1：探索 Chat Copilot 应用程序

1.  通过 Visual Studio Code 访问 Chat Copilot 目录，并导航到 CHAT-COPILOT> webapi> appsettings.json，你可以在其中自定义系统提示，从而实现将 Copilot 变为客户支持 AI 助手的目标。

1.  导航到 CHAT-COPILOT > webapp > App.tsx，对字幕进行个性化设置。

1.  导航到 CHAT-COPILOT > webapp > style.tsx 以修改 Chat Copilot 应用程序的颜色。

1.  部署 Chat Copilot 应用程序后，提供以下提示，并检查 Copilot 如何生成响应。

How is the chat copilot configured?

1.  提供另一个提示，并检查 Copilot 如何生成响应。

How is a backend REST API .NET web API service used in this application?

1.  提供其他提示并评估生成的响应。

1.  单击左侧的“Edit”（编辑）按钮，以进行重命名。

8. 单击“Add”（添加）**(1)**** 按钮**，选择“New Chat Session”（新建聊天会话）(2) 来创建新的聊天会话。

# 挑战 5：使用自己的文档聊天 - 解决方案指南

## 任务 1：在 Chat Copilot 应用程序中使用自己的文档聊天

1.  单击顶部的“Documents”（文档）选项卡，然后单击“Upload”（上传）。

1.  导航到 C:\Labfiles\Documents 以上传 3 个 PDF。

1.  上传完成后，提供以下提示，并检查 Copilot 如何生成响应。

How to operate Android Auto in Porche Taycan?Give step-by-step instructions.

1.  提供另一个提示，并检查 Copilot 如何生成响应。

Give detailed information on Apple CarPlay.

1.  该响应不仅根据这些文档中的内容回答了问题，而且还包括该内容的引文 (1)，以验证信息的准确性。单击注释时，应用将直接跳转到文档 (2) 的页面，该页面将进行计划比较，以便我们可以了解详细情况或在引文部分中对答案的准确性进行额外验证。

1.  单击左侧的“Edit”（编辑）按钮，以进行重命名。

1.  单击“Add”（添加）按钮创建“New Session”（新会话）。

# 挑战 6：在 Azure 上部署 Chat Copilot 应用程序 - 解决方案指南

### 任务 1：在 Azure 中创建应用注册

你将需要两个 Microsoft Entra ID 应用程序注册，其中一个用于前端 Web 应用，一个用于后端 API。以下是创建 Azure 应用注册的步骤：

1.  在 Azure 门户中，搜索并选择“App Registrations”（应用注册）。

1.  单击“New Registration”（新注册）。

1.  为资源提供首选名称，选择“Accounts in this organizational directory only ({YOUR TENANT} only - Single tenant)”（仅此组织目录中的帐户(仅限 {你的租户} - 单租户)）作为受支持的帐户类型。在“Redirect URI”（重定向 URI）下，选择“Single-page application (SPA)”（单页应用程序(SPA)）作为“platform type”（平台类型），将重定向 URI 设置为 http://localhost:3000，然后单击“Register”（注册）。

1.  在应用注册的“Overview”（概述）页面下，记下“Application (client) ID”（应用程序(客户端) ID）。

1.  单击右上角的**叉号符号**，再次选择“New Registration”（新注册），
为后端创建应用注册。

1.  为资源提供首选名称，选择“Accounts in this organizational directory only ({YOUR TENANT} only - Single tenant)”（仅此组织目录中的帐户(仅限 {你的租户} - 单租户)）作为受支持的帐户类型，然后单击“Register”（注册）。

1.  在应用注册的“Overview”（概述）页面下，记下“Application (client) ID”
（应用程序(客户端) ID）。

### 任务 2：将前端应用注册连接到后端应用注册

在这一步中，将前端应用注册连接到后端应用注册。

1.  在后端应用注册中，从左侧窗格的“Manage”（管理）下，选择“Expose an API”（公开 API）部分。

1.  单击屏幕顶部“**Applic**ation ID URI”（应用程序 ID URI）旁边的“Add”（添加）。
这将生成一个 api:// URI。

1.  单击“Save”（保存）。

1.  现在，单击“Scopes defined by this API”（此 API 定义的作用域）下的“Add a scope”（添加作用域）。

1.  针对“Scope name”（作用域名称）提供 access_as_user，针对“Who can consent”（谁可以同意）选择“Admins and users”（管理员和用户），然后针对“Admin consent display name”（管理员同意显示名称）和“User consent display name”（用户同意显示名称）提供“Access Chat Copilot as a user”（以用户身份访问 Chat Copilot），针对“Admin consent description”（管理员同意描述）和“User consent description”（用户同意描述）提供“Allows the accesses to the Chat Copilot web API as a user”（允许以用户身份访问 Chat Copilot Web API）。单击“Add scope”
（添加作用域）。

1.  现在，单击“Authorized client applications”（授权客户端应用程序）下的“Add a client application”（添加客户端应用程序）。

1.  在“Client ID”（客户端 ID）中输入前端的应用程序（客户端）ID，然后选中“Authorized scopes”（授权作用域）下的复选框。单击“Add application”
（添加应用程序）。

1.  导航到前端应用注册，然后单击左侧窗格中“Manage”（管理）部分下的“API Permissions”（API 权限）。

1.  单击“Configured Permissions”（配置的权限）下的“Add a permission”
（添加权限）。

1.  单击“Select an API”（选择 API）下的“APIs my organization uses”
（我的组织使用的 API），然后选择表示后端 Web API 的应用注册。

1.  选择权限“access_as_user”，然后单击“Add permissions”（添加权限）。

### 任务 3：在 Azure 门户中部署 Azure 基础结构

在此任务中，你将在 Azure 门户中部署 ARM 模板，该模板将创建所需的资源以便处理。

1.  单击“Deploy to Azure”（部署到 Azure）按钮 (TODO)。通过单击以下按钮，可直接从 Azure 门户部署基础结构：

**注意：**[这将](https://github.com/microsoft/chat-copilot/releases)自动部署最新发布的 Chat Copilot    二进制文件（链接）。

它将指导你在 MS Azure 中部署自定义模板。

1.  在“Edit”（编辑）页面上：

1.  选择可用的订阅和资源组。

1.  确保“AI Service”（AI 服务）设置为“Azure OpenAI”。

1.  在之前步骤已经记下的参数中提供必要的详细信息，如“Ai Endpoint”
（AI 终结点）、“Ai Api Key”（AI API 密钥）、“Web Api Client Id”（Web API 客户端 ID）（你的后端应用注册客户端 ID）和“Frontend Client Id”（前端客户端 ID）。

1.  提供 Microsoft Entra ID 概述中的“Microsoft Entra ID Tenant Id”（Microsoft Entra ID 租户 ID）。

1.  将**“**Deploy WebSearcher Plugin”（部署 WebSearcher 插件）的值设置为 True，
单击“Review + Create”（审阅 + 创建），然后单击“Create”（创建）。

1.  验证资源组中部署的 Azure 资源，即应用服务、应用服务计划、文档智能、Azure Cosmos DB 帐户、Log Analytics 工作区和存储帐户。

### 任务 4：将 Chat Copilot 应用程序部署到 Azure

在此任务中，你会将 Chat Copilot 应用程序部署到 Azure。

1.  打开 Visual Studio Code，导航到 C:/Users/azureuser/chat-copilot/scripts/deploy/。

1.  右键单击“deploy”文件夹，然后单击“Open in Integrated Terminal”（在集成终端中打开）。

1.  若要部署应用程序，首先通过运行以下命令对其进行打包，该命令用于让 Web API 做好部署准备。

.\package-webapi.ps1

1.  现在，要将应用程序部署到 Azure，请运行以下命令。更新所需的参数，

如 SubscriptionID、ResourceGroupName 和 DeploymentName。

./deploy-webapi.ps1 -Subscription {YOUR_SUBSCRIPTION_ID} -ResourceGroupName         {YOUR_RESOURCE_GROUP_NAME} -DeploymentName {YOUR_DEPLOYMENT_NAME}

**注意：**要查找部署名称，请在左侧菜单的资源组部署部分中查找以 Microsoft.Template

开头的部署。

# 挑战 7：部署 Web Searcher 插件

## 任务 1：部署 Web Searcher 插件

1.  打开Visual Studio Code，导航到 C:/Users/azureuser/chat-copilot/scripts/deploy/。

1.  右键单击 deploy 文件夹，单击“Open in an Integrated Terminal”（在集成终端中打开），然后运行以下命令：

.\package-plugins.ps1

1.  运行以下命令，将 Web Searcher 插件部署到 Azure。

./deploy-plugins.ps1 -Subscription {YOUR_SUBSCRIPTION_ID} -ResourceGroupName         {YOUR_RESOURCE_GROUP_NAME} -DeploymentName {YOUR_DEPLOYMENT_NAME}

**注意：**提供订阅 ID、资源组名称和部署名称。

**注意：**要查找部署名称，请在左侧菜单的资源组部署部分中查找以 Microsoft.Template 开头的部署。

## 任务 2：使用 Chat Copilot 应用程序及 WebSearcher 插件

1.  返回 Azure 门户，然后选择现有资源组。

1.  选择在上一个挑战中部署的 webapi，并从概述页面复制默认域 URL。

1.  粘贴之后将导航到 Chat Copilot 应用程序。

1.  使用“Environment Details”（环境详细信息）选项卡中提供的“Microsoft credentials”（Microsoft 凭据）登录。

1.  单击右上角的“Plugins”（插件），启用“WebSearcher plugin”（WebSearcher 插件）。

1.  提供以下提示，并检查 Copilot 如何生成响应。

List out the latest updates in Chat Copilot

**重要说明：**由于 WebSearcher 插件依赖 Semantic Kernel 包，因此该插件一直存在一个问题。在 Chat Copilot 应用中启用 WebSearcher 插件后，可能会出现内部服务器错误。

# 挑战 8：启用 Microsoft Graph 插件

## 任务 1：在启用 Microsoft Graph 的情况下使用 Chat Copilot 应用程序

1.  导航到 Azure 门户，选择现有资源组。

1.  选择之前部署的 webapi，然后从概述页面复制“Default domain URL”
（默认域 URL）。

1.  粘贴之后将导航到 Chat Copilot 应用程序。

1.  使用“Environment Details”（环境详细信息）选项卡中提供的“Microsoft credentials”（Microsoft 凭据）登录。

1.  单击右上角的“Plugins”（插件），启用“Microsoft Graph Plugin”
（Microsoft Graph 插件）。

**注意：**确保 Web Searcher 插件已禁用。

1.  当出现同意弹出窗口时，单击“Accept”（接受）。

1.  在聊天应用程序中，提供以下提示并生成所需的请求。单击“Yes, proceed”
（是，继续）

When is my next meeting scheduled?

1.  提供另一个提示，并检查 Copilot 如何生成响应。

To whom did I send my last email and when?

1.  提供另一个提示，以便向你的个人电子邮件发送一封邮件，并检查 Copilot 如何生成响应。

[Send a Greeting](mailto:abc@contoso.com)s mail to abc@contoso.com

1.  提供关于你应用场景的其他提示并评估生成的响应。

**注意：**如果应用程序不生成响应，请禁用并重新启用插件。

# 挑战 9：探索和了解 Miyagi Stack - 解决方案指南

### 任务 1：使用 Azure 门户部署解决方案

在此任务中，你将在 Azure 门户中部署 ARM 模板，该模板将创建所需的资源以便处理。

1.  单击“Deploy to Azure”（部署到 Azure）按钮 (TODO)。

它将指导你在 MS Azure 中部署自定义模板。

1.  选择可用的订阅和资源组。提供“Resource Prefix”（资源前缀）参数的唯一字母数字值以及其他所需的输入内容。你可以通过单击“Edit Template”（编辑模板）来查看 ARM 模板。查看并创建 Miyagi Stack 部署。

1.  验证资源组中部署的 Azure 资源，即搜索服务、存储帐户和 Azure Cosmos DB 帐户资源。

### 任务 2：检索 Azure 资源值

你需要在 Azure 门户中检索在上一个挑战中部署的“Azure OpenAI Service Key”（Azure OpenAI 服务密钥）、“Endpoint”（终结点）及其“LLM model name”（LLM 模型名称）。

1.  在 Azure 门户中，搜索并选择“Azure OpenAI”。

1.  转到之前创建的 Azure OpenAI 资源，然后从左侧窗格中选择“Keys and Endpoints”（密钥和终结点）。

1.  复制 2 个密钥的任何一个以及终结点，并将其存储在某个位置以供将来参考。

1.  在“Overview”（概述）页面中，单击“Go to Azure OpenAI Studio”（转到 Azure OpenAI Studio），以转到部署的模型。

1.  转到“Deployments”（部署），即可看到已部署的 AI 模型。复制之前部署的 AI 模型的部署名称，并将其存储在某个位置以供将来参考。

现在，需要检索在上一步中部署的“Azure AI Search Key”（Azure AI 搜索密钥）

和“Endpoint URL”（终结点 URL）。

1.  在 Azure 门户中，搜索并选择“AI Search”（AI 搜索）。

1.  转到之前创建的 AI 搜索资源，并复制“Overview”（概述）页面下的“URL”。然后导航到“Settings”（设置）下的“Keys”（密钥），并复制“Primary admin key”
（主管理员密钥）。

现在需要检索在上一步中部署的“Azure Cosmos DB”帐户“URL”和“Primary Connection String”（主连接字符串）。

1.  在 Azure 门户中，搜索并选择“Azure Cosmos DB”。

1.  转到之前创建的 Azure Cosmos DB 资源，并复制“Overview”（概述）页面下的“URL”。然后导航到“Settings”（设置）下的“Keys”（密钥），并复制“Primary Connection String”（主连接字符串）。你还可以在此处找到显示的“URI”。

现在需要检索在上一步中部署的“Storage accounts”（存储帐户）Blob 服务 URL。

1.  在 Azure 门户中，搜索并选择“Storage accounts”（存储帐户）。

1.  转到之前创建的存储帐户资源，并从左侧窗格中“Settings”（设置）下的“Endpoints”（终结点）部分复制“Blob Service URL”（Blob 服务 URL）。

### 任务 3：在 Azure 门户中部署必应搜索资源

在此任务中，你将在 Azure 门户中部署必应搜索资源并获取其密钥。

1.  在 Azure 门户中，搜索“Bing Search”（必应搜索），并选择其中的“Bing Resources”（必应资源）。

1.  单击“Add”（添加），然后单击“Bing Search”（必应搜索）。填写必要的详细信息，然后按下 F1 键或选择免费定价层。选中复选框，然后单击“Review + Create”（审阅 + 创建）。

1.  这些资源将很快进行部署。然后，单击“Go to Resource”（转到资源），并从左侧窗格的“Keys and Endpoint”（密钥和终结点）部分下检索任意 2 个现有密钥。

### 任务 4：克隆 Miyagi GitHub 存储库

如果你尚未将 Miyagi GitHub 代码存储库克隆到正在进行此实验的环境中，请按照以下步骤操作。否则，请在 Visual Studio Code 中打开克隆的文件夹。

1.  启动 Visual Studio Code。

1.  打开面板 (SHIFT + CTRL + P) 并运行“Git: Clone”命令，将 https://github.com/Azure-Samples/miyagi 存储库克隆到本地文件夹
（任一文件夹均可）。

1.  克隆存储库后，在 Visual Studio Code 中打开文件夹。

### 任务 5：设置 Miyagi 应用的配置

在此部分，你需要为之前记录的变量提供所需的值，并将其保存到文件中。

1.  转到 VS Code 中的克隆文件夹，导航到 miyagi/services/recommendation-service/dotnet，然后创建名为 appsettings.json 的新文件。

1.  导航到 dotnet/appsettings.json.example，复制全部内容并粘贴到 appsettings.json中。

1.  通过替换以下变量的值，修改 appsett**ings.json 文件。保留 "cosmo**sDbContainerName": "recommendations" 和 "logLevel": "Trace" 
的默认设置。

| **变量** | **值** |
| --- | --- |
| deploymentOrModelId | gpt-35-turbo 模型名称 |
| embeddingDeploymentOrModelId | text-embedding-002 模型名称 |
| endpoint | Azure OpenAI 资源终结点 |
| apiKey | Azure OpenAI 资源密钥 |
| azureCognitiveSearchEndpoint | 搜索服务终结点 |
| azureCognitiveSearchApiKey | 搜索服务密钥 |
| cosmosDbUri | Cosmos DB URI |
| blobServiceUri | 存储帐户 Blob 服务 URL |
| bingApiKey | 必应搜索密钥 |
| cosmosDbConnectionString | Cosmos DB 主连接字符串 |

### 任务 6：在本地运行 Miyagi 前端

在此部分，你将在本地系统运行 Miyagi 前端。

1.  打开一个新终端：“Terminal”（终端）->“New Terminal”（新终端）（或 Ctrl + Shift + `）

1.  导航到 miyagi/ui/typescript 目录。

1.  运行以下命令以安装依赖项：

```
npm install --global yarn
```

```
yarn install
```

```
yarn dev
```

1.  打开浏览器，导航到

```
http://localhost:&lt;port&gt; 默认端口为 4001
```

你应该会看到 Miyagi 应用在本地运行，如下所示：

**注意：**你可以从终端的日志中获取端口。

### 任务 7：在本地运行推荐服务

在此部分，你将在本地系统运行推荐服务。

1.  打开一个新终端：“Terminal”（终端）->“New Terminal”（新终端）
（或 Ctrl + Shift + `）

1.  导航到 miyagi/services/recommendation-service/dotnet 目录。

1.  运行以下命令以在本地运行推荐服务：

```
dotnet build
```

```
dotnet run
```

1.  打开浏览器，导航到 http://localhost:<port>/swagger/index.html。默认端口为 5224。你应该会看到“Recommendation service”（推荐服务）的 Swagger 页面。

**注意：**你可以从终端的日志中获取端口。

### 任务 8：Azure AI 搜索中的持久嵌入

在此部分，你将从 Swagger 应用创建响应，并通过转到 Azure 门户上的 Azure AI 搜索来验证响应是否正常。

1.  在“Memory”（记忆）部分的“Swagger UI”页面中，单击“POST/dataset”
（POST/数据集）以进行扩展，并单击“Try it out”（立即尝试）。

1.  将代码替换为以下代码，然后单击“Execute”（执行）。

```
{
```

```
 &quot;metadata&quot;: {
```

```
       &quot;userId&quot;: &quot;50&quot;,
```

```
       &quot;riskLevel&quot;: &quot;aggressive&quot;,
```

```
       &quot;favoriteSubReddit&quot;: &quot;finance&quot;,
```

```
       &quot;favoriteAdvisor&quot;: &quot;Jim Cramer&quot;
```

```
     },
```

```
   &quot;dataSetName&quot;: &quot;intelligent-investor&quot;
```

```
 }
```

1.  在“Swagger UI”页面中，向下滚动到“Responses”（响应）会话审查，通过检查代码状态是否为“200”来查看是否成功执行。

1.  返回到“Azure portal”（Azure 门户）选项卡，搜索并打开之前部署的 AI 搜索资源。

1.  在“Search management”（搜索管理）下打开“Indexes”（索引），并查看已创建的“miyagi-embeddings”。

**注意：**查看文档计数时请单击刷新按钮。

### 任务 9：通过个性化探索 Miyagi 应用和推荐服务

在此部分，你将探索之前创建的 Miyagi 应用和推荐服务。

1.  返回到推荐服务 UI 页面，然后单击“Personalize”（个性化设置）按钮。

1.  **在“Pe**rsonalize”（个性化设置）页面上，从下拉菜单中选择你的财务顾问，然后单击“Personalize”（个性化设置）。

1.  你应该会在“Top Stocks”小组件中看到来自推荐服务的推荐。

1.  查看日志后，在终端中按下 **Ctrl + C** 以停止 Swagger UI 页面。

# 挑战 10：探索 Miyagi Stack 的各种用例

### 任务 1：克隆 Miyagi GitHub 存储库

如果你尚未将 Miyagi GitHub 代码存储库克隆到正在进行此实验的环境中，请按照以下步骤操作。否则，请在 Visual Studio Code 中打开克隆的文件夹。

1.   打开 Visual Studio Code。

1.  导航到目录“C:/Users/azureuser”

```
cd C:/Users/azureuser
```

1.  克隆给定的存储库。

```

```

git clone [https://github.com/](https://github.com/Azure-Samples/miyagi)Azure-Samples/miyagi

```

```

1.  克隆存储库后，在 Visual Studio Code 中打开文件夹。

### 任务 2：检索 Azure OpenAI 服务值

你需要在 Azure 门户中检索在上一个挑战中部署的“Azure OpenAI Service Key”（Azure OpenAI 服务密钥）、“Endpoint”（终结点）及其“LLM model name”（LLM 模型名称）。

1.  在 Azure 门户中，搜索并选择“Azure OpenAI”。

1.  转到之前创建的 Azure OpenAI 资源，然后从左侧窗格中选择“Keys and Endpoints”（密钥和终结点）。

1.  复制 2 个密钥的任何一个以及终结点，并将其存储在某个位置以供将来参考。

1.  在“Overview”（概述）页面中，单击“Go to Azure OpenAI Studio”（转到 Azure OpenAI Studio），以转到部署的模型。

1.  转到“Deployments”（部署），即可看到已部署的 AI 模型。复制之前部署的 AI 模型的部署名称，并将其存储在某个位置以供将来参考。

### 任务 3：检索 Azure AI 搜索值

现在，需要检索在上一步中部署的“Azure AI Search Key”（Azure AI 搜索密钥）和“Endpoint URL”（终结点 URL）。

1.  在 Azure 门户中，搜索并选择“AI Search”（AI 搜索）。

1.  转到之前创建的 AI 搜索资源，并复制“Overview”（概述）页面下的“URL”。然后导航到“Settings”（设置）下的“Keys”（密钥），并复制“Primary admin key”
（主管理员密钥）。

### 任务 4：创建 .env 文件并使用 Azure 资源值更新变量

在此部分，你需要为之前记录的变量提供所需的值，并将其保存到文件中。

1.  在 **VS Code** 中打开克隆的文件夹，然后导航到 miyagi/sandbox/usecases/rag/dotnet 目录。创建新文件“.env”，并将 rag/.env.example 的所有内容复制到 .env 中。

1.  通过更新在之前步骤中检索的 Azure 资源值修改“.env”文件，这些值包括 Azure OpenAI 模型名称、终结点和密钥以及 Azure AI 搜索终结点、URL 和其密钥，然后保存文件。

### 任务 5：了解 Miyagi Stack 中推荐服务的实施

推荐服务使用  Sema**ntic K**ernel SDK 实现 RAG 模式。实施的详细信息可在文件夹“miyagi/sandbox/usecases/rag/dotnet”中的 Jupyter Notebook 中捕获。

1.  在 **VSCode**中，从目录 miyagi/sandbox/usecases/rag/dotnet 执行名为 Getting-started.ipynb 的笔记本。

1.  **运行单元格以了解如何实施推荐服**务的分步详细信息。特别注意如何使用 Semantic Kernel 实施 RAG 模式。最终的单元格结果如下所示：