
# 翻译语音合成

## 基于事件的合成

在 `TranslationConfig` 中为翻译后的语音指定所需的语音。为 `TranslationRecognizer` 对象的 `Synthesizing` 事件创建事件处理程序。在事件处理程序中，使用 `Result` 参数的 `GetAudio()` 方法检索经过翻译的音频的字节流。用于实现事件处理程序的特定代码因所使用的编程语言而异。请参阅语音 SDK 文档中的 `C#` 示例和 `Python` 示例。

## 手动合成

手动合成是基于事件的合成的替代方法，无需实现事件处理程序。可以使用手动合成为一种或多种目标语言生成音频翻译。

手动合成翻译实质上只是以下两个不同操作的组合：

1. 使用 `TranslationRecognizer` 将语音输入翻译成一种或多种目标语言的文本转录。

2. 在翻译操作结果中循环访问翻译字典，并使用 `SpeechSynthesizer` 合成每种语言的音频流。

# Applied Skills

## 一般准则

在此应用场景中，某些资源的部署可能需要几分钟的时间。在部署完成期间，请查看实验室中的其他任务。你可以执行不依赖于该部署的其他任务。

感谢你同意帮助我们的团队使用 Azure AI 语言构建自然语言处理解决方案。

在 Azure 中执行任务时，必须遵循我们的一些一般准则。

+ 创建对象时，请使用默认设置，除非有不同的配置要求。

+ 只有创建、删除或修改对象才能达到上述要求。对环境进行不必要的更改可能会对您的最终成绩产生不利影响。

+ 如果有多种实现目标的方法，请务必选择管理工作量最小的方法。

此致

首席技术官 (CTO)

## 现有环境

以下是用于访问 Azure 中的资源的凭据：

用户名：`User-@labinstanceid`

密码：`password-@labinstanceid`

可以通过单击“说明”选项卡找到这些凭据。

你已获得 `rg-@labinstanceid` 资源组的权限。请在此资源组中创建所有 Azure 对象。

我们还创建了名为 `sa@labinstanceid` 的存储帐户供你使用。

此致

网络管理员

> 注意：需要留意此处的 `labinstanceid` 之后创建资源需要使用相同的 ID

## Part I：概念验证

你好，

感谢你同意帮助我们的 IT 部门构建概念证明 (PoC) 来对文本进行分类。一位同事已经开始构建示例解决方案，该解决方案可以识别位置信息和电子邮件地址。我们需要你帮助敲定该解决方案并将其连接到 Azure，以证明该技术已准备就绪。

### 这是你的任务：

你需要创建一个可以识别位置信息和电子邮件地址的 PoC 解决方案。该解决方案必须使用最适合该用例的模型。该解决方案必须满足以下要求：

+ 在 `rg-@labinstanceid` 中新建名为 `lang-@labinstanceid` 的语言服务。

+ 确保语言服务支持自定义文本分类和命名实体识别 (NER)。

+ 确保语言服务使用免费层，并在北欧 Azure 区域中预配。

+ 使用现有 `sa@labinstanceid` 存储帐户。

+ 确保 PoC 应用的源代码连接到新建资源中的相应模型。可以在 `C:\PoC` 中找到应用。该应用有 `C#` 版本和 `Python` 版本。修改你喜欢的版本。无需修改这两个版本。完成任务后，应用必须能继续正常运行。

此致

项目管理团队

> 解题过程

+ 在 `RG1` 中创建名为 `lang-36478729` 的新语言服务。

+ 保语言服务支持自定义文本分类和命名实体识别 （NER）。

+ 确保语言服务使用免费层，并在北欧 Azure 区域中预配。

+ 使用现有的 `sa36478729` 存储帐户。

![](img/c5a47bed261be93ec11114238ef3a084.png)

+ 确保 PoC 应用的源代码连接到新创建的资源中的相应模型。您可以在 `C:\PoC` 中找到该应用。该应用有 `C#` 版本和 `Python` 版本。修改您喜欢的版本。您不需要修改这两个版本。完成任务后，应用必须继续正常运行。

![](img/451ad1f808ce3811eef9627151c80c01.png)

## Part II：发票验证

你好，

我们要从网上商店日志中提取信息来验证我们的发票系统。我们需要你将日志上传到训练模型并正确标记它们。标记后，你必须训练模型，以便我们的开发人员可以在稍后的验证过程中使用它。

### 这是你的任务：

你需要使用命名实体识别 (NER) 从网上商店收到的日志文件中提取名称和价格。创建的项目必须满足以下要求：

+ 项目必须命名为 `Webshop`。

+ 语言必须是英语（美国），而不是多语言。

+ 项目必须使用两个名为 `Price` 和 `CustomerName` 的新实体。

+ `sa@labinstanceid/webshop` 容器中的示例数据必须标有 `Price` 和 `CustomerName`。

+ 必须启动模型训练。

无需等待训练完成即可开始下一个任务。

此致

项目管理团队

> 解题思路

+ 项目必须命名为 `Webshop`。

+ 语言必须是英语（美国），而不是多语言。

+ 项目必须使用两个名为 `Price` 和 `CustomerName` 的新实体。

+ `sa@labinstanceid/webshop` 容器中的示例数据必须标有 `Price` 和 `CustomerName`。

+ 必须启动模型训练。

![](img/a44386bc7bfc80a65195c280bec37b26.png)

## Part III：聊天机器人后端

你好，

我们计划构建一个聊天机器人应用，它可以回答用户问题并允许用户订购产品。我们需要你构建一个供聊天机器人使用的语言理解解决方案。

### 这是你的任务：

你需要创建一个新的语言理解项目。该解决方案必须满足以下要求：

+ 使用 `C:\projects` 文件夹中 `lang4567` 的现有项目定义。

+ 使用标准训练模式。

+ 将数据拆分为训练占比 85% 和测试占比 15%。

+ 将模型的部署配置为使用名称 `lang4567`。

+ 将 `CLU_PoC` 应用的源代码修改为使用已部署模型的 API 终结点。可以在 `c:\CLU_PoC` 中找到应用。该应用有 `C#` 版本和 `Python` 版本。修改你喜欢的版本。无需修改这两个版本。请注意，这不是生产代码，我们将对客户端密码进行硬编码以用于测试目的。

+ 确保 `CLU_PoC` 应用仅打印置信度分数超过 80% 的意向值。

此致

项目管理团队

> 解题步骤

+ 使用 `C:\projects` 文件夹中 `lang4567` 的现有项目定义。

![](img/74c2fe91003d0b59dc949df6f8e1a7b6.png)

+ 使用标准训练模式。

+ 将数据拆分为训练占比 85% 和测试占比 15%。

+ 将模型的部署配置为使用名称 `lang4567`。

![](img/bfaeb008666598d10aceab24993db40f.png)

+ 将 `CLU_PoC` 应用的源代码修改为使用已部署模型的 API 终结点。可以在 `c:\CLU_PoC` 中找到应用。该应用有 `C#` 版本和 `Python` 版本。修改你喜欢的版本。无需修改这两个版本。请注意，这不是生产代码，我们将对客户端密码进行硬编码以用于测试目的。

+ 确保 `CLU_PoC` 应用仅打印置信度分数超过 80% 的意向值。

使用以下代码(注意需要替换key和endpoint的值)

```
key = "EyPVzuGNGl8WOQFfdt9raTUDxWDALSrNyLUjEYMyrqwE9zkbyc4kJQQJ99AKACi5YpzXJ3w3AAAaACOGUtYC"
endpoint = "https://lang-lod49820879.cognitiveservices.azure.com/"
project_name = "lang4567"
deployment_name = "lang4567"

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
