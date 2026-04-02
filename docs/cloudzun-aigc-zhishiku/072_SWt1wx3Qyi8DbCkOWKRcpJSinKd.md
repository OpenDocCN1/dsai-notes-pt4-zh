# 附录：

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/AlrBdU9nkorW02xO5X1cQNj5nch](https://zsyhjtnsa5.feishu.cn/docx/AlrBdU9nkorW02xO5X1cQNj5nch)

# 为什么不能只用 Endpoint 和 API Key？

在教学环境中，确实需要一种既能让学生完成实验，又能最大限度控制风险、简化凭证分发的方法。

您希望达成的目标——仅使用类似 Azure OpenAI （AOAI）的endpoint和api key来验证AgentsClient——很遗憾，目前azure-a**i-a**gents这个 SDK 的设计架构不支持这种方式。

让我用中文为您详细解释原因，并提供一个最适合您教学场景的、安全的解决方案。

这背后是 Azure 服务设计的核心理念：控制平面（Control Plane）和数据平面（Data Plane）的分离。

1.  数据平面 （Data Plane）: 这指的是“使用”服务的操作。例如，向一个已经部署好的 GPT 模型发送提示词（prompt）并获得补全（completion）。这类操作通常可以使用endpoint和api key进行身份验证。您熟悉的 AOAI 调用就属于这一类。API Key 就像是进入某个特定房间的钥匙。

1.  控制平面 （Control Plane）: 这指的是“管理”服务资源的操作。例如，**创建**、删除、配置 AI Agent，上传文件，管理会话（Thread）等。这些操作会改变 Azure 资源的配置。这类操作必须通过 Azure Active Directory （AAD）进行身份验证（例如，用户登录az login，或者您提到的服务主体）。AAD 提供了基于角色的访问控制（RBAC），可以精细地控制谁能对哪些资源做什么操作，这比一个简单的 API Key 要安全和强大得多。

您代码中的AgentsClient执行的几乎所有操作（create_agent， upload_and_poll， create thread）都属于**控制平面**操作。因此，它被设计为必须使用基于 AAD 的凭证（DefaultAzureCredential），而不能是简单的 API Key。

---

## 解决方案：创建“最小权限”的服务主体 （The Best Compromise）

既然不能用 API Key，我们可以把我上次提到的“服务主体”方案进行改良，使其风险降到最低，完美契合您的教学需求。核心思想是：创建一个服务主体，但只授予它在您指定的这一个 AI 项目内活动的权限，而不是整个订阅或资源组的权限。这样即使凭证泄露，风险也被严格限制在本次实验的范围内。

这就像是给学生一把“教学专用”的钥匙，这把钥匙只能打开“AI Agent 实验教室”这一个门，而无法访问大楼里的任何其他房间。

### **第 1 步：在 Azure 中创建**一个自定义角色（Custom Role）

我们需要先定义一个权限极小的角色，这个角色只包含运行这个 Agent 程序所必需的权限。

1.  获取您的 Azure 订阅 ID 和 AI 项目的资源 ID。

+   订阅 ID: az account show --query id -o tsv

+   AI 项目资源 ID（请替换您的资源组名和 AI 项目名）: az resource show --resource-group <你的资源组名> --name <你的AI项目名> --resource-type Microsoft.CognitiveServices/accounts/projects --query id -o tsv

1.  创建一个名为 agent_role_definition.json 的 JSON 文件，内容如下。请将<your_subscription_id>替换为您的订阅 ID。

```
{
  &quot;Name&quot;: &quot;AI Agent Project Operator&quot;,
  &quot;Description&quot;: &quot;Allows managing agents, threads, and files within a specific AI Project.&quot;,
  &quot;Actions&quot;: [
    &quot;Microsoft.CognitiveServices/accounts/projects/agents/write&quot;,
    &quot;Microsoft.CognitiveServices/accounts/projects/agents/read&quot;,
    &quot;Microsoft.CognitiveServices/accounts/projects/agents/delete&quot;,
    &quot;Microsoft.CognitiveServices/accounts/projects/threads/write&quot;,
    &quot;Microsoft.CognitiveServices/accounts/projects/threads/read&quot;,
    &quot;Microsoft.CognitiveServices/accounts/projects/files/write&quot;,
    &quot;Microsoft.CognitiveServices/accounts/projects/files/read&quot;,
    &quot;Microsoft.CognitiveServices/accounts/projects/agents/runs/write&quot;,
    &quot;Microsoft.CognitiveServices/accounts/projects/agents/runs/read&quot;
  ],
  &quot;NotActions&quot;: [],
  &quot;DataActions&quot;: [],
  &quot;NotDataActions&quot;: [],
  &quot;AssignableScopes&quot;: [
    &quot;/subscriptions/&lt;your_subscription_id&gt;&quot;
  ]
}
```

1.  使用 Azure CLI 创建这个自定义角色：

```
az role definition create --role-definition @agent_role_definition.json
```

### 第 2 步：创建服务主体并分配“受限”角色

1.  **创建一个**新的服务主体。执行此命令后，请妥善保管输出的appId， password， 和 tenant。这就是您要发给学生的凭证。

```
az ad sp create-for-rbac --name &quot;StudentAgentLabPrincipal&quot;
```

1.  将我们刚刚创建的“AI Agent Project Operator”角色分配给这个新的服务主体，但作用范围（scope）严格限定在你的 AI 项目上。

```
az role assignment create --assignee &lt;上面输出的appId&gt; \
                          --role &quot;AI Agent Project Operator&quot; \
                          --scope &quot;&lt;你的AI项目资源ID&gt;&quot;
```

### **第 3 ****步：分**发凭证并修改学生的 .env 文件

现在，您可以安全地将以下三个信息分发给您的学生：

+   AZURE_CLIENT_ID： 服务主体的 appId

+   AZURE_CLIENT_SECRET： 服务主体的 password

+   AZURE_TENANT_ID： 服务主体的 tenant

学生们的 .env 文件应该像这样：

```
PROJECT_ENDPOINT=&quot;your_project_endpoint&quot;
MODEL_DEPLOYMENT_NAME=&quot;gpt-4o&quot;
AZURE_CLIENT_ID=&quot;服务主体的appId&quot;
AZURE_TENANT_ID=&quot;服务主体的tenant&quot;
AZURE_CLIENT_SECRET=&quot;服务主体的password&quot;
```

### 第 4 步：学生的 Python 代码

学生的 Python 代码几乎不需要修改，只需确保DefaultAzureCredential没有被错误地配置即可。原始代码中的写法需要稍微调整一下，去掉排他性参数。

修改这一段：

```
# Connect to the Agent client
agent_client = AgentsClient(
   endpoint=project_endpoint,
   credential=DefaultAzureCredential() # 直接使用默认配置，它会自动查找环境变量
)
```

## 总结

这个**“最小权限服务主体”**方案是您教学场景下的最佳实践：

+   **安全性高：** 凭证的权限被严格限制在单个 AI 项目内，无法访问或破坏您 Azure 订阅中的任何其他资源。

+   **体验一致：** 学生无需az login，只需配置好.env文件，即可运行代码，体验与生产环境一致的验证流程。

+   **易于管理：** 实验结束后，您可以随时删除这个服务主体或撤销其角色分配，凭证立即失效。

通过这种方式，您既能向学生展示企业级的身份验证方法，又能将风险控制在绝对最小的范围内。