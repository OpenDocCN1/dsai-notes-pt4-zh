
```
[C](https://chengzhcos250514.cognitiveservices.azure.com/)[onnecting to Forms Recognizer at: https://chengzhcos250514.cognitivese](https://github.com/MicrosoftLearning/mslearn-ai-document-intelligence/blob/main/Labfiles/01-prebuild-models/sample-invoice/sample-invoice.pdf?raw=true)rvices.azure.com/
Analyzing invoice at: https://github.com/MicrosoftLearning/mslearn-ai-document-intelligence/blob/main/Labfiles/01-prebuild-models/sample-invoice/sample-invoice.pdf?raw=true

Vendor Name: CONTOSO LTD., with confidence 0.937.
Customer Name: 'MICROSOFT CORPORATION, with confidence 0.919.
Invoice Total: '$110.0, with confidence 0.969.

Analysis complete.
```

![](img/8d0f750c88a8872915477f18eef6ad73.png)

# Applied Skills：使用 Azure AI 语言构建自然语言处理解决方案

## 一般准则

在此应用场景中，某些资源的部署可能需要几分钟的时间。在部署完成期间，请查看实验室中的其他任务。你可以执行不依赖于该部署的其他任务。

感谢你同意帮助我们的团队使用 Azure AI 语言构建自然语言处理解决方案。

在 Azure 中执行任务时，必须遵循我们的一些一般准则。

+   创建对象时，请使用默认设置，除非有不同的配置要求。
+   只有创建、删除或修改对象才能达到上述要求。 对环境进行不必要的更改可能会对您的最终成绩产生不利影响。
+   如果有多种实现目标的方法，请务必选择管理工作量最小的方法。

此致

首席技术官 （CTO）

## 现有环境

以下是用于访问 Azure 中的资源的凭据：

用户名：`User-@labinstanceid`

密码：`password-@labinstanceid`

可以通过单击“说明”选项卡找到这些凭据。

你已获得 `rg-@labinstanceid` 资源组的权限。 请在此资源组中创建所有 Azure 对象。

此致

网络管理员

> 注意：需要留意此处的`labinstanceid`之后创建资源需要使用相同的 ID

## Part I：概念验证

你好，

感谢你同意帮助我们的 IT 部门构建概念证明 （PoC） 来对文本进行分类。 一位同事已经开始构建示例解决方案，该解决方案可以识别位置信息和电子邮件地址。 我们需要你帮助敲定该解决方案并将其连接到 Azure，以证明该技术已准备就绪。

### 这是你的任务：

你需要创建一个可以识别位置信息和电子邮件地址的 PoC 解决方案。 该解决方案必须使用最适合该用例的模型。 该解决方案必须满足以下要求：

+   在 `rg-@labinstanceid` 中新建名为 `lang-@labinstanceid` 的语言服务。
+   确保语言服务支持自定义文本分类和命名实体识别 （`NER`）。
+   确保语言服务使用免费层，并在北欧 Azure 区域中预配。
+   使用现有 `sa@labinstanceid` 存储帐户。
+   确保 PoC 应用的源代码连接到新建资源中的相应模型。 可以在 `C:\PoC` 中找到应用。 该应用有 C# 版本和 Python 版本。 修改你喜欢的版本。 无需修改这两个版本。 完成任务后，应用必须能继续正常运行。

此致

项目管理团队

> 解题过程

+   在 `RG1` 中创建名为 `lang-36478729` 的新语言服务。
+   保语言服务支持自定义文本分类和命名实体识别 （`NER`）。
+   确保语言服务使用免费层，并在北欧 Azure 区域中预配。
+   使用现有的 `sa36478729` 存储帐户。

![](img/78804bf2ec787b56326dabbade0450a7.png)

+   确保 PoC 应用的源代码连接到新创建的资源中的相应模型。您可以在 `C:\PoC` 中找到该应用。该应用有 C# 版本和 Python 版本。修改您喜欢的版本。您不需要修改这两个版本。完成任务后，应用必须继续正常运行。

![](img/1cb0ed3ecafa532875e49476057b5a13.png)

## Part II：发票验证

你好，

我们要从网上商店日志中提取信息来验证我们的发票系统。 我们需要你将日志上传到训练模型并正确标记它们。 标记后，你必须训练模型，以便我们的开发人员可以在稍后的验证过程中使用它。

### 这是你的任务：

你需要使用命名实体识别 （`NER`） 从网上商店收到的日志文件中提取名称和价格。 创建的项目必须满足以下要求：

+   项目必须命名为 `Webshop`。
+   语言必须是英语（美国），而不是多语言。
+   项目必须使用两个名为 `Price` 和 `CustomerName` 的新实体。
+   `sa@labinstanceid/webshop` 容器中的示例数据必须标有 `Price` 和 `CustomerName`。
+   必须启动模型训练。

无需等待训练完成即可开始下一个任务。

此致

项目管理团队

> 解题思路

+   项目必须命名为 `Webshop`。
+   语言必须是英语（美国），而不是多语言。
+   项目必须使用两个名为 `Price` 和 `CustomerName` 的新实体。
+   `sa@labinstanceid/webshop` 容器中的示例数据必须标有 `Price` 和 `CustomerName`。
+   必须启动模型训练。

![](img/6d22bef89935e7a8ad5f25ffcefe8795.png)

## Part III：聊天机器人后端

你好，

我们计划构建一个聊天机器人应用，它可以回答用户问题并允许用户订购产品。 我们需要你构建一个供聊天机器人使用的语言理解解决方案。

### 这是你的任务：

你需要创建一个新的语言理解项目。 该解决方案必须满足以下要求：

+   使用 `C:\projects` 文件夹中 `lang4567` 的现有项目定义。
+   使用标准训练模式。
+   将数据拆分为训练占比 85% 和测试占比 15%。
+   将模型的部署配置为使用名称 `lang4567`。
+   将 `CLU_PoC` 应用的源代码修改为使用已部署模型的 API 终结点。 可以在 `c:\CLU_PoC` 中找到应用。 该应用有 C# 版本和 Python 版本。 修改你喜欢的版本。 无需修改这两个版本。 请注意，这不是生产代码，我们将对客户端密码进行硬编码以用于测试目的。
+   确保 `CLU_PoC` 应用仅打印置信度分数超过 80% 的意向值。

此致

项目管理团队

> 解题步骤

+   使用 `C:\projects` 文件夹中 `lang4567` 的现有项目定义。

![](img/a75fe10e8808ffff58c342871c90e459.png)

+   使用标准训练模式。
+   将数据拆分为训练占比 85% 和测试占比 15%。
+   将模型的部署配置为使用名称 `lang4567`。

![](img/8a5dd65ded64e66b28cc219dcbea0fd2.png)

+   将 `CLU_PoC` 应用的源代码修改为使用已部署模型的 API 终结点。 可以在 `c:\CLU_PoC` 中找到应用。 该应用有 C# 版本和 Python 版本。 修改你喜欢的版本。 无需修改这两个版本。 请注意，这不是生产代码，我们将对客户端密码进行硬编码以用于测试目的。
+   确保 `CLU_PoC` 应用仅打印置信度分数超过 80% 的意向值。

使用以下代码（注意需要替换`key`和`endpoint`的值）

```
key = "EyPVzuGNGl8WOQFfdt9raTUDxWDALSrNyLUjEYMyrqwE9zkbyc4kJQQJ99AKACi5YpzXJ3w3AAAaACOGUtYC";
endpoint = "https://lang-lod49820879.cognitiveservices.azure.com/";
project_name = "lang4567";
deployment_name = "lang4567";

import sys
from azure.core.credentials import AzureKeyCredential
# add import statement for client library
from azure.ai.language.conversations import ConversationAnalysisClient

def main():
    # analyze query
    if (len(sys.argv) > 1):
        query = sys.argv[1]
    else:
        query = "Send an email to Carol about the tomorrow's demo"
    
    client = ConversationAnalysisClient(endpoint, AzureKeyCredential(key)) # replace null with correct method
    with client:
        result = client.analyze_conversation( # replace null with correct method
            task={
                "kind": "Conversation",
                "analysisInput": {
                    "conversationItem": {
                        "participantId": "1",
                        "id": "1",
                        "modality": "text",
                        "language": "en",
                        "text": query
                    },
                    "isLoggingEnabled": False
                },
                "parameters": {
                    "projectName": project_name,
                    "deploymentName": deployment_name,
                    "verbose": True
                }
            }
        )

    # view result
    print(f"query: {result['result']['query']}")
    if result['result']['prediction']['intents'][0]['confidenceScore'] > 0.8:
        print(f"category: {result['result']['prediction']['intents'][0]['category']}")
        print(f"confidence score: {result['result']['prediction']['intents'][0]['confidenceScore']}\n")

    print("entities:")
    for entity in result['result']['prediction']['entities']['confidenceScore']:
        print(f"\ncategory: {entity['category']}")
        print(f"text: {entity['text']}")
        print(f"confidence score: {entity['confidenceScore']}")

if __name__ == '__main__':
    main()
```

# Applied Skills：创建智能文档处理解决方案

## 一般准则

在此应用场景中，某些资源的部署可能需要几分钟的时间。在部署完成期间，请查看实验室中的其他任务。你可以执行不依赖于该部署的其他任务。

感谢你同意帮助我们的团队使用 Azure AI 文档智能创建智能文档处理解决方案。

在 Azure 中执行任务时，必须遵循我们的一些一般准则。

+   创建对象时，请使用默认设置，除非有不同的配置要求。
+   请仅仅创建、删除或修改对象来达到上述要求。 对环境进行不必要的更改可能会对你的最终分数产生不利影响。
+   如果有多种实现目标的方法，请始终选择管理工作量最小的方法。

此致

首席技术官 （CTO）

## 现有环境

你将协助我们的团队使用 Azure AI 文档智能创建文档智能解决方案。

以下是用于访问 Azure 资源的凭据：

用户名：`User-@labinstanceid`

密码：`password-@labinstanceid`

你也可以在“说明”选项卡中找到登录信息。

你已获得 `rg-@labinstanceid` 资源组的权限。 请在此资源组中创建所有 Azure 对象。

此致

网络管理员

## Part I：创建原型

你好，

我们希望构建一个原型，以使用 Azure AI 文档智能自动从发票和收据中提取数据。 我们有位于 `c:\files` 的示例文件。

### 这是你的任务：

你需要创建一个新的使用免费定价层的 Azure AI 文档智能资源。 然后使用预生成模型来分析发票和收据。 将每个文件的 JSON 输出保存到 `c:\files`。

创建 JSON 输出后，按模型类型对所有文件进行排序，然后将文件上传到 `prototype@labinstanceid`。 请为每种模型类型使用单独的 Blob 存储容器。

备注：如果无法在 Azure 门户中找到文档智能资源类型，则可能需要使用以前的名称“表单识别器”进行搜索。

此致

项目管理团队

> 解题步骤

+   打开文档智能 Studio

![](img/767ed1be445e4aa15b604b45f11963b6.png)

![](img/07a29b55cb15c35f6d42d43d3c95b40c.png)

+   使用预生成模型分析发票和收据

![](img/8a66dffafd985a5663d7ba32498ea230.png)

![](img/05343223e7ada3ef36ce557189577ac9.png)

+   将每个文件的 `JSON` 输出保存到 `c:\files`

![](img/1c77ab9ee745f7e764f9a8c6e6015d4a.png)

+   为每种模型类型在 `prototype@labinstanceid` 中创建单独的 Blob 存储容器，并上传文件

![](img/b9728619a2c7a6ee7e7a57f6fd1427b4.png)

![](img/6260f974aec16778b44baa219d7b30ba.png)

![](img/77419ac7bec8ab9e561456598ee7ce7b.png)

## Part II：训练自定义模型

你好，

我们在 `qualityassurance@labinstanceid` 存储帐户中有一些文档。 我们需要使用自定义模型来分析文档。

### 这是你的任务：

你需要创建一个名为 `Documents` 的自定义 Azure AI 文档智能项目，该项目使用模板模型和 `2022-08-31` API 版本。

你需要为该项目执行以下任务：

+   将项目链接到 `form@labinstanceid` 资源。
+   使用 `qualityassurance@labinstanceid` 存储帐户中的 `documents-training` 容器。
