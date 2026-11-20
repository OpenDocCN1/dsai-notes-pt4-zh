
# Microsoft Applied Skills: Develop generative AI solutions with Azure OpenAI Service

> 来源：https://zsyhjtnsa5.feishu.cn/docx/CSsUdxq4BoWVfDxIs65cMVcSnHg

## 题目解析

### General Guidelines

Subject: General guidelines

From: cto@contoso.com

To: `User1-40594128@cloudslice.onmicrosoft.com`

---

Thank you for agreeing to help our team develop generative AI solutions with Azure OpenAI Service.

We have some general guidelines that you must follow when you perform tasks in Azure. Please always use the following guidelines:

+ When creating objects, use the default settings, unless there are requirements that require different configurations.
+ Only create, delete, or modify objects to achieve the stated requirements. Unnecessary changes to the environment can adversely affect your final score.
+ If there are multiple approaches to achieving a goal, always choose the approach that requires the least amount of administrative effort.
+ In the app provided, do not worry about potential bugs in the existing code. Focus on adding what you need to complete the functionality.
+ In the virtual machine, you have access to the Python client library for OpenAI version 1.6.1 and the Azure OpenAI client library for .NET version 1.0.0-beta.12. Please use these versions and do NOT update the libraries.
+ A check box is provided in the header of each email so you can mark the tasks included in that email as complete. The check box is provided for your convenience; you are not required to select it. It is not used for scoring.

Thanks,

Chief Technology Officer (CTO)

感谢您同意帮助我们的团队使用Azure OpenAI服务开发生成式AI解决方案。

在Azure中执行任务时，您必须遵循一些通用准则。请始终使用以下准则：

+ 创建对象时，请使用默认设置，除非有要求需要不同配置的要求。
+ 仅创建、删除或修改对象以实现所述要求。对环境进行不必要的更改可能会对您的最终得分产生不利影响。
+ 如果有多种方法可以实现目标，请始终选择需要最少管理努力的方法。
+ 在提供的应用程序中，不用担心现有代码中的潜在错误。专注于添加您需要完成功能的内容。
+ 在虚拟机中，您可以访问OpenAI的Python客户端库版本1.6.1和Azure OpenAI的.NET客户端库版本1.0.0-beta.12。请使用这些版本，不要更新库。
+ 在每封电子邮件的标题中提供了一个复选框，以便您可以标记该电子邮件中包含的任务为已完成。提供复选框是为了方便您；您无需选择它。它不用于评分。

### Existing environment

Subject: Existing environment

From: networkadmin@contoso.com

To: `User1-40594128@cloudslice.onmicrosoft.com`

Mark as unread

---

Here are your credentials for accessing our resources in Azure:

User name: `User1-40594128@cloudslice.onmicrosoft.com`

Password: dHkB9e!0D!

You can find these credentials by clicking the Instructions tab.

You have been given rights to the `RG1lod40594128` resource group. Please create all Azure objects in this resource group and in the same region as this resource group.

Each task that you must complete only needs to be tested with the app provided. Our developers will be able to parse the results.

We have added comments to the code indicating any changes that must be made. Do not modify any other code.

You can use either Python or C#, whichever you prefer, and only need to complete one. Use the command line (`python PoC.py` or `dotnet run`) when running your app.

Thanks,

Network administrator

您可以通过点击“说明”选项卡找到这些凭证。

您已经被赋予了对`RG1lod40594128`资源组的权限。请在该资源组内以及与该资源组相同的区域内创建所有Azure对象。

您必须完成的每个任务只需要使用提供的应用程序进行测试。我们的开发人员将能够解析结果。

我们已经在代码中添加了注释，指示必须进行的更改。不要修改任何其他代码。

您可以使用Python或C#，您更喜欢哪种语言，并且只需要完成其中之一。在运行应用程序时，请使用命令行（`python PoC.py` 或 `dotnet run`）。

### Validate proof of concept (PoC)

Subject: Validate proof of concept (PoC)

From: projman@contoso.com

To: `User1-40594128@cloudslice.onmicrosoft.com`

Hello,

We are evaluating the use of Azure OpenAI. We need your help implementing a PoC. The framework has already been completed in both Python and C#. You need to complete the framework in only one of the languages.

This is your task:

You need to implement the Azure resources and configure the sample app located in `C:\files\AzureOpenAIPoc` to connect to the resources. The solution must meet the following requirements:

+ Deploy a `GPT-35-turbo-16k` model in Azure OpenAI in the same region as the `RG1lod40594128` resource group.
+ Configure the settings file of the PoC app with the connection strings. Do NOT add the connection strings directly to `PoC.cs` or `PoC.py`.
+ In `Main()`, configure the client settings for Azure OpenAI. If using Python, use API version `2023-12-01-preview`.
+ In `function1()`, configure the messages, API parameters, and call chat completion connection.
+ Validate whether a response is received by using the sample text prompt file.

**IMPORTANT**: From the "Deploy model" dialog box, you must select "Advanced options" and set the "Tokens per Minute Rate Limit (thousands)" setting to 5K. You must also set the "Enable Dynamic Quota" setting to Disabled. You may NOT be able to complete all steps for this Applied Skill if you fail to configure these settings.

You can manually enter or use the sample file (`sample-text.txt`) as prompt input.

Thanks,

Project Management Team

我们正在评估使用Azure OpenAI。我们需要您的帮助来实现一个概念验证（PoC）。框架已经在Python和C#两种语言中完成。您只需要在这两种语言中完成其中一个。

这是您的任务：

您需要实现Azure资源，并配置位于`C:\files\AzureOpenAIPoc`的示例应用程序以连接到这些资源。解决方案必须满足以下要求：

+ 在与资源组`RG1lod40594128`相同的区域中，在Azure OpenAI部署一个`GPT-35-turbo-16k`模型。
+ 使用连接字符串配置PoC应用程序的设置文件。不要直接将连接字符串添加到`PoC.cs`或`PoC.py`中。
+ 在`Main()`函数中，为Azure OpenAI配置客户端设置。如果使用Python，请使用API版本`2023-12-01-preview`。
+ 在`function1()`函数中，配置消息、API参数，并调用聊天完成连接。
+ 通过使用示例文本提示文件来验证是否接收到响应。

重要事项：从“部署模型”对话框中，您必须选择“高级选项”，并将“每分钟令牌速率限制（千）”设置为5K。您还必须将“启用动态配额”设置为禁用。如果您未能配置这些设置，您可能无法完成这项应用技能的所有步骤。

您可以手动输入或使用示例文件（`sample-text.txt`）作为提示输入。

### 基本环境代码解析

```python
import os
from dotenv import load_dotenv
import utils

# Add OpenAI import. (Add code here)
from openai import AzureOpenAI

def main(): 
        
    try:     
        load_dotenv()
        utils.initLogFile()
        azure_oai_endpoint = os.getenv("AZURE_OAI_ENDPOINT")
        azure_oai_key = os.getenv("AZURE_OAI_KEY")
        azure_oai_model = os.getenv("AZURE_OAI_MODEL")
        azure_oai_version = "2023-12-01-preview"
        # Define Azure OpenAI client (function1-3)
        client = AzureOpenAI( azure_endpoint=azure_oai_endpoint,
                            api_key=azure_oai_key,
                            api_version=azure_oai_version,
                            )
        # Define Azure OpenAI client (function4)
        client = AzureOpenAI( base_url=f"{azure_oai_endpoint}/openai/deployments/{azure_oai_model}/extensions",
                            api_key=azure_oai_key,
                            api_version=azure_oai_version,
                            )
        
        

        function_map = {
            "1": function1,
            "2": function2,
            "3": function3,
            "4": function4
        }

        while True:
            print('1: Validate PoC\n' +
                  '2: Company chatbot\n' +
                  '3: Developer tasks\n' +
                  '4: Use company data\n' +
                  '\'quit\' to exit the program\n')
            command = input('Enter a number:')
            if command.strip() in function_map:
                function_map[command](client, azure_oai_model)
            elif command.strip().lower() == 'quit':
                print('Exiting program...')
                break
            else :
                print("Invalid input. Please enter number 1, 2, 3, 4, or 5.")

    except Exception as ex:
        print(ex)
```

这段Python代码主要做了以下几件事：

1. 导入了所需的模块，包括`os`、`dotenv`和`utils`，以及一个名为`AzureOpenAI`的类。
2. 在`main`函数中，首先加载环境变量，初始化日志文件，然后从环境变量中获取Azure OpenAI的相关配置信息，包括端点、密钥、模型和版本。
3. 使用获取的配置信息创建了两个`AzureOpenAI`客户端实例。第一个实例使用的是基础的Azure OpenAI端点，第二个实例使用的是特定模型的部署端点。
4. 定义了一个函数映射字典`function_map`，将字符串`"1"`、`"2"`、`"3"`和`"4"`映射到四个不同的函数上。
5. 进入一个无限循环，提示用户输入一个数字，然后根据输入的数字调用相应的函数。如果用户输入的是`"quit"`，则退出循环，结束程序。如果用户输入的不是`"1"`、`"2"`、`"3"`、`"4"`或`"quit"`，则提示输入无效。
6. 如果在执行过程中出现异常，会捕获异常并打印异常信息。
