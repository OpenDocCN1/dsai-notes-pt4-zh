# Microsoft Applied Skills: Develop generative AI solutions with Azure OpenAI Service

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/CSsUdxq4BoWVfDxIs65cMVcSnHg](https://zsyhjtnsa5.feishu.cn/docx/CSsUdxq4BoWVfDxIs65cMVcSnHg)

# 题目解析

## General Guidelines

Subject: General guidelines

From: cto@contoso.com

To: User1-40594128@cloudslice.onmicrosoft.com

---

Thank you for agreeing to help our team develop generative AI solutions with Azure OpenAI Service.

We have some general guidelines that you must follow when you perform tasks in Azure. Please always use the following guidelines:

+   When creating objects, use the default settings, unless there are requirements that require different configurations.

+   Only create, delete, or modify objects to achieve the stated requirements. Unnecessary changes to the environment can adversely affect your final score.

+   If there are multiple approaches to achieving a goal, always choose the approach that requires the least amount of administrative effort.

+   In the app provided, do not worry about potential bugs in the existing code. Focus on adding what you need to complete the functionality.

+   In the virtual machine, you have access to the Python client library for OpenAI version 1.6.1 and the Azure OpenAI client library for .NET version 1.0.0-beta.12. Please use these versions and do NOT update the libraries.

+   A check box is provided in the header of each email so you can mark the tasks included in that email as complete. The check box is provided for your convenience; you are not required to select it. It is not used for scoring.

Thanks,

Chief Technology Officer (CTO)

感谢您同意帮助我们的团队使用Azure OpenAI服务开发生成式AI解决方案。

在Azure中执行任务时，您必须遵循一些通用准则。请始终使用以下准则：

+   创建对象时，请使用默认设置，除非有要求需要不同配置的要求。

+   仅创建、删除或修改对象以实现所述要求。对环境进行不必要的更改可能会对您的最终得分产生不利影响。

+   如果有多种方法可以实现目标，请始终选择需要最少管理努力的方法。

+   在提供的应用程序中，不用担心现有代码中的潜在错误。专注于添加您需要完成功能的内容。

+   在虚拟机中，您可以访问OpenAI的Python客户端库版本1.6.1和Azure OpenAI的.NET客户端库版本1.0.0-beta.12。请使用这些版本，不要更新库。

+   在每封电子邮件的标题中提供了一个复选框，以便您可以标记该电子邮件中包含的任务为已完成。提供复选框是为了方便您；您无需选择它。它不用于评分。

## Existing environment

Subject: Existing environment

From: networkadmin@contoso.com

To: User1-40594128@cloudslice.onmicrosoft.com

Mark as unread

---

Here are your credentials for accessing our resources in Azure:

User name: **User1-40594128@clouds**lice.onmic**rosoft.com**

Password: dHkB9e!0D!

You can find these credenti**als by click**ing the Instructions tab.

**You have been **given rights to the RG1lod40594128 resource** gro**up. Please create all Azure objects in this resource group and in the same region as this resource group.

Each task that you must complete only needs to be tested with the app provided. Our developers will be able to parse the results.

We have added comments to the code indicating any changes that must be made. Do not modify any other code.

You can use either Python or C#, whichever you prefer, and only need to complete one. Use the command line ('python PoC.py' or 'dotnet run') when running your app.

Thanks,

Network administrator

+   您可以通过点击“说明”选项卡找到这些凭证。

+   您已经被赋予了对**RG1lod40594128**资源组的权限。请在该资源组内以及与该资源组相同的区域内创建所有Azure对象。

+   您必须完成的每个任务只需要使用提供的应用程序进行测试。我们的开发人员将能够解析结果。

+   我们已经在代码中添加了注释，指示必须进行的更改。不要修改任何其他代码。

+   您可以使用Python或C#，您更喜欢哪种语言，并且只需要完成其中之一。在运行应用程序时，请使用命令行（'python PoC.py' 或 'dotnet run'）。

## Validate proof of concept (PoC)

**Subject: Validate pr**oof of concept (PoC)

From: projman@contoso.com

To: User1-40594128@cloudslice.onmicrosoft.com

Hello,

We are evaluating the use of Azure OpenAI. We need your help implementing a PoC. The framework has already been completed in both Python and C#. You need to complete the framework in only one of the languages.

This is your task:

You need to implement the Azure resources and configure the sample app located in C:\files\AzureOpenAIPoc to connect to the resources. The solution must meet the following requirements:

+   Deploy a GPT-35-turbo-16k model in Azu**re OpenAI in t**he the same region as the RG1lod40594128 resource group.

+   Configure the settings file of the PoC app with the connection strings. Do NOT add the connection strings directly to PoC.cs or PoC.py.

+   In Main(), configure the client settings for Azure OpenAI. If using Python, use API version 2023-12-01-preview.

+   In function1(), configure the messages, API parameters, and call chat completion connection.

+   Validate whether a response is received by using the sample text prompt file.

**IMPORTANT**: From the **Deploy model** dialog box, **you must select Adva**nced options** a**nd set the T**okens pe**r Minute Rate Limit (thousands) setting to 5K. You must also set the Enable Dynamic Quota setting to Disabled. You may NOT be able to complete all steps for this Applied Skill if you fail to configure these settings.

You can manually enter or use the sample file (sample-text.txt) as prompt input.

Thanks,

Project Management Team

我们正在评估使用Azure OpenAI。我们需要您的帮助来实现一个概念验证（PoC）。框架已经在Python和C#两种语言中完成。您只需要在这两种语言中完成其中一个。

这是您的任务：

您需要实现Azure资源，并配置位于C:\files\AzureOpenAIPoc的示例应用程序以连接到这些资源。解决方案必须满足以下要求：

+   在与资源组RG1lod40594128相同的区域中，在Azure OpenAI部署一个GPT-35-turbo-16k模型。

+   使用连接字符串配置PoC应用程序的设置文件。不要直接将连接字符串添加到PoC.cs或PoC.py中。

+   在Main()函数中，为Azure OpenAI配置客户端设置。如果使用Python，请使用API版本2023-12-01-preview。

+   在function1()函数中，配置消息、API参数，并调用聊天完成连接。

+   通过使用示例文本提示文件来验证是否接收到响应。

重要事项：从“部署模型”对话框中，您必须选择“高级选项”，并将“每分钟令牌速率限制（千）”设置为5K。您还必须将“启用动态配额”设置为禁用。如果您未能配置这些设置，您可能无法完成这项应用技能的所有步骤。

您可以手动输入或使用示例文件（sample-text.txt）作为提示输入。

**基本环境代码解析**

```
import os
from dotenv import load_dotenv
import utils

# Add OpenAI import. (Add code here)
from openai import AzureOpenAI

def main(): 
        
    try:     
        load_dotenv()
        utils.initLogFile()
        azure_oai_endpoint = os.getenv(&quot;AZURE_OAI_ENDPOINT&quot;)
        azure_oai_key = os.getenv(&quot;AZURE_OAI_KEY&quot;)
        azure_oai_model = os.getenv(&quot;AZURE_OAI_MODEL&quot;)
        azure_oai_version = &quot;2023-12-01-preview&quot;
        # Define Azure OpenAI client (function1-3)
        client = AzureOpenAI( azure_endpoint=azure_oai_endpoint,
                            api_key=azure_oai_key,
                            api_version=azure_oai_version,
                            )
        # Define Azure OpenAI client (function4)
        client = AzureOpenAI( base_url=f&quot;{azure_oai_endpoint}/openai/deployments/{azure_oai_model}/extensions&quot;,
                            api_key=azure_oai_key,
                            api_version=azure_oai_version,
                            )
        
        

        function_map = {
            &quot;1&quot;: function1,
            &quot;2&quot;: function2,
            &quot;3&quot;: function3,
            &quot;4&quot;: function4
        }

        while True:
            print(&#x27;1: Validate PoC\n&#x27; +
                  &#x27;2: Company chatbot\n&#x27; +
                  &#x27;3: Developer tasks\n&#x27; +
                  &#x27;4: Use company data\n&#x27; +
                  &#x27;\&#x27;quit\&#x27; to exit the program\n&#x27;)
            command = input(&#x27;Enter a number:&#x27;)
            if command.strip() in function_map:
                function_map[command](client, azure_oai_model)
            elif command.strip().lower() == &#x27;quit&#x27;:
                print(&#x27;Exiting program...&#x27;)
                break
            else :
                print(&quot;Invalid input. Please enter number 1, 2, 3, 4, or 5.&quot;)

    except Exception as ex:
        print(ex)

```

> 这段Python代码主要做了以下几件事：

1.  导入了所需的模块，包括os、dotenv和utils，以及一个名为AzureOpenAI的类。

1.  在main函数中，首先加载环境变量，初始化日志文件，然后从环境变量中获取Azure OpenAI的相关配置信息，包括端点、密钥、模型和版本。

1.  使用获取的配置信息创建了两个AzureOpenAI客户端实例。第一个实例使用的是基础的Azure OpenAI端点，第二个实例使用的是特定模型的部署端点。

1.  定义了一个函数映射字典function_map，将字符串"1"、"2"、"3"和"4"映射到四个不同的函数上。

1.  进入一个无限循环，提示用户输入一个数字，然后根据输入的数字调用相应的函数。如果用户输入的是"quit"，则退出循环，结束程序。如果用户输入的不是"1"、"2"、"3"、"4"或"quit"，则提示输入无效。

1.  如果在执行过程中出现异常，会捕获异常并打印异常信息。

> 注意，这段代码中的function1、function2、function3和function4并未在代码中定义，可能是在其他地方定义的，或者是需要你自己定义的。

Task 1: Validate PoC 代码解析

```
# Task 1: Validate PoC
def function1(aiClient, aiModel):
    inputText = utils.getPromptInput(&quot;Task 1: Validate PoC&quot;, &quot;sample-text.txt&quot;)
    
    # Build messages to send to Azure OpenAI model. (Add code here)
    messages=[
            {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;You are a helpful assistant.&quot; + inputText}
        ]
    

    # Define argument list (Add code here)
    apiParams = {
        &quot;model&quot; : aiModel,
        &quot;messages&quot;: messages,
        
    }
    

    utils.writeLog(&quot;API Parameters:\n&quot;, apiParams)

    # Call chat completion connection. (Add code here)
    # Use the call name and **apiParams to reference our argument list
    response = aiClient.chat.completions.create( **apiParams)
    

    utils.writeLog(&quot;Response:\n&quot;, str(response))
    print(&quot;Response: &quot; + response.choices[0].message.content + &quot;\n&quot;)
    return response
```

> 这段Python代码定义了一个名为function1的函数，该函数接收两个参数：aiClient和aiModel。这个函数的主要工作流程如下：

1.  使用utils.getPromptInput函数从"sample-text.txt"文件中获取输入文本，该函数的第一个参数是提示信息。

1.  创建一个名为messages的列表，该列表包含一个字典，字典中有两个键值对："role"和"content"。"role"的值是"system"，表示这是一个系统消息；"content"的值是"You are a helpful assistant."加上输入的文本，表示这是要发送给Azure OpenAI模型的消息。

1.  创建一个名为apiParams的字典，该字典包含两个键值对："model"和"messages"。"model"的值是传入的aiModel参数，表示要使用的模型；"messages"的值是前面创建的messages列表，表示要发送的消息。

1.  使用utils.writeLog函数将apiParams字典写入日志。

1.  调用aiClient.chat.completions.create方法，传入apiParams字典，向Azure OpenAI发送请求并获取响应。这里使用了Python的**操作符，可以将字典的键值对作为命名参数传入函数。

1.  使用utils.writeLog函数将响应写入日志。

1.  打印响应中的第一个选择的消息内容。

1.  返回响应。

> 这个函数的主要作用是向Azure OpenAI模型发送消息，并获取模型的响应。

## **Company chatbot**

Subject: Company chatbot

From: projman@contoso.com

To: User1-40594128@cloudslice.onmicrosoft.com

Hello,

Executives are very excited about the potential of Azure OpenAI. We need you to further develop the PoC app to explore how we can use it.

This is your task:

You need to develop the app you worked on previously, but this time, make changes in function2(). The app must meet the following requirements:

+   Each response must be in a casual tone and end with “Hope that helps! Thanks for using Contoso, Ltd.”

+   Responses must be limited to 1,000 tokens and the temperature must be 0.5.

+   At least one example must be provided with the prompt, such as:

+   Prompt: Where can I find the company phone number?

+   Response: You can find it on the footer of every page on our website. Hope that helps! Thanks for using Contoso, Ltd.

+   Use prompt engineering techniques to ask the following question and get the response in both English and Spanish:

+   "What is the best way to find if a company is hiring?"

Thanks,

Project Management Team

高管们对Azure OpenAI的潜力感到非常兴奋。我们需要您进一步开发PoC应用程序，探索我们如何使用它。

这是您的任务：

您需要开发您之前工作过的应用程序，但这次，在function2()中进行更改。应用程序必须满足以下要求：

+   每个响应必须使用随意的语气，并以“希望这有帮助！感谢您使用Contoso, Ltd。”结束。

+   响应必须限制在1,000个令牌以内，温度必须为0.5。

+   至少必须提供一个示例与提示一起，例如：

+   提示：我在哪里可以找到公司电话号码？

+   响应：您可以在我们网站的每个页面的页脚上找到它。希望这有帮助！感谢您使用Contoso, Ltd。

+   使用提示工程技巧询问以下问题，并以英语和西班牙语两种语言获得响应：

+   “找到公司是否招聘的最佳方式是什么？”

对于这个任务，您可能需要使用Azure OpenAI的多语言支持功能，以及调整您的应用程序以生成符合上述要求的响应。您可能需要对function2()进行编码，以确保它能够接收提示，然后生成一个符合所需语气和限制的响应。此外，您可能需要利用Azure OpenAI的翻译功能或集成一个翻译服务来提供西班牙语的响应。

在实现这个功能时，您可能需要考虑以下几点：

1.  **令牌限制**：确保生成的响应不超过1,000个令牌。

1.  **温度设置**：在调用Azure OpenAI服务时，设置温度为0.5，这将影响生成文本的创造性。

1.  **多语言支持**：如果Azure OpenAI支持多语言生成，您可以用英语生成响应，然后翻译成西班牙语，或者直接用西班牙语生成响应。

1.  **提示工程**：精心设计提示，以确保问题被正确理解和回答。

1.  **一致性**：确保每个响应都以指定的结束语结束。

请**注意，具**体**的实现细**节将取**决于您使用的编程**语言和Azure OpenAI的具体API调用方式。您可能需要查阅相关的API文档来了解如何设置温度、生成响应、以及如何使用翻译功能。

Task 2: Company chatbot 代码解析

```
# Task 2: Company chatbot
def function2(aiClient, aiModel):
    inputText = utils.getPromptInput(&quot;Task 2: Company chatbot&quot;, &quot;sample-text.txt&quot;)
    
    # Build messages to send to Azure OpenAI model. (Add code here)
    messages=[
            {&quot;role&quot;:&quot;system&quot;, &quot;content&quot;: &quot;You are a helpful assistant.You must reply in English and Spanish, in a casual tone.&quot;},
            {&quot;role&quot;:&quot;user&quot;,&quot;content&quot;:&quot; Where can I find the company phone number?&quot;},
            {&quot;role&quot;:&quot;assistant&quot;,&quot;content&quot;:&quot; You can find it on the footer of every page on our website. Hope that helps! Thanks for using Contoso, Ltd.&quot;  },
            {&quot;role&quot;:&quot;user&quot;,&quot;content&quot;:&quot;Please reply using the following format\n###\nEnglish Reply: ......\n\nSpanish Reply......\n###&quot; + inputText},
        ]
    

    # Define argument list (Add code here)
    apiParams = {
        &quot;messages&quot;: messages,
        &quot;model&quot; : aiModel,
        &quot;temperature&quot;: 0.5,
        &quot;max_tokens&quot;: 1000,
    }
    

    utils.writeLog(&quot;API Parameters:\n&quot;, apiParams)

    # Call chat completion connection. (Add code here)
    # Use the call name and **apiParams to reference our argument list
    response = aiClient.chat.completions.create( **apiParams)
    

    utils.writeLog(&quot;Response:\n&quot;, str(response))
    print(&quot;Response: &quot; + response.choices[0].message.content + &quot;\nHope that helps! Thanks for using Contoso, Ltd.&quot;)
    return response
```

> 这段Python代码定义了一个名为function2的函数，该函数接收两个参数：aiClient和aiModel。这个函数的主要工作流程如下：

1.  使用utils.getPromptInput函数从"sample-text.txt"文件中获取输入文本，该函数的第一个参数是提示信息。

1.  创建一个名为messages的列表，该列表包含四个字典，每个字典都有两个键值对："role"和"content"。这些字典表示一个对话的过程，其中"system"的消息设定了对话的背景，"user"的消息是用户的问题，"assistant"的消息是助手的回答。

1.  创建一个名为apiParams的字典，该字典包含四个键值对："model"、"messages"、"temperature"和"max_tokens"。"model"的值是传入的aiModel参数，表示要使用的模型；"messages"的值是前面创建的messages列表，表示要发送的消息；"temperature"的值是0.5，表示生成的文本的随机性；"max_tokens"的值是1000，表示生成的文本的最大长度。

1.  使用utils.writeLog函数将apiParams字典写入日志。

1.  调用aiClient.chat.completions.create方法，传入apiParams字典，向Azure OpenAI发送请求并获取响应。这里使用了Python的**操作符，可以将字典的键值对作为命名参数传入函数。

1.  使用utils.writeLog函数将响应写入日志。

1.  打印响应中的第一个选择的消息内容，然后添加一句希望对方得到帮助的话。

1.  返回响应。

> 这个函数的主要作用是模拟一个对话的过程，向Azure OpenAI模型发送消息，并获取模型的响应。

## **Developer tasks**

Subject: Developer tasks

From: projman@contoso.com

To: User1-40594128@cloudslice.onmicrosoft.com

Hello,

We are very excited about the potential of the PoC app, and our developers want to see what it can do to help with development.

This is your task:

You need to use function3() of the PoC app to ensure that the following tasks can be successfully completed:

+   Take the legacy code in legacyCode.py, add comments, and generate documentation.

+   Generate five unit tests for the function in fibonacci.py

Once you have modified function3, submit individual code generation requests for each task to Azure OpenAI by using the PoC app. The code files are in C:\files. It is recommended that you modify the prompt in sample-text.txt to accomplish each task.

Thanks,

Project Management Team

对于这个任务，您需要使用PoC应用程序中的function3()来确保能够成功完成以下工作：

1.  **添加注释和生成文档**：处理位于legacyCode.py中的遗留代码，为其添加注释，并生成相应的文档。这通常涉及到理解代码的功能，并编写清晰、准确的注释来解释代码的工作原理，以及可能的用例。此外，您可能需要使用特定的工具或库来从注释中生成正式的文档。

1.  **生成单元测试**：为fibonacci.py中的函数生成五个单元测试。单元测试是用来验证代码某个特定部分（如一个函数）正确性的自动化测试。在这种情况下，您需要创建测试用例来验证斐波那契数列生成函数的正确性。

为了完成这些任务，您需要修改function3()，以便它能够通过PoC应用程序向Azure OpenAI提交单独的代码生成请求。这里是一些可能的步骤：

+   **修改function3()**：确保function3()能够接收legacyCode.py和fibonacci.py作为输入，并根据需要生成相应的输出。

+   **设计提示**：修改sample-text.txt中的提示，使其能够指导Azure OpenAI生成所需的注释、文档和单元测试。这可能需要一些提示工程技术，以确保生成的内容符合您的要求。

+   **提交请求**：使用修改后的function3()和更新后的提示，通过PoC应用程序向Azure OpenAI提交请求。

+   **验证结果**：一旦Azure OpenAI返回结果，验证它们是否满足任务要求。对于文档和注释，检查它们是否清晰、准确，并增强了代码的可读性。对于单元测试，确保它们全面覆盖了fibonacci.py中函数的关键功能，并能够作为有效的回归测试。

请注意，具体的实现细节将取决于PoC应用程序的当前设计和Azure OpenAI的API文档。您可能需要与开发团队紧密合作，以确保正确地实现所需的功能。此外，您可能还需要具备一些编程知识，以便能够理解和修改相关的Python代码。

Task 3: Developer tasks 代码解析

```
def function3(aiClient, aiModel):
    while True:
        print(&#x27;\n1: Add comments to my function\n&#x27; +
                &#x27;2: Write unit tests for my function\n&#x27; +
                &#x27;\&quot;quit\&quot; to exit the program\n&#x27;)
        command = input(&#x27;Enter a number to select a task:&#x27;)
        if command == &#x27;1&#x27;:
            file = open(file=&quot;../../legacyCode.py&quot;, encoding=&quot;utf8&quot;).read()
            newfile = &quot;../../legacyCode.py&quot;
            prompt = &quot;Add comments to the following function. Return all code and comments.\n---\n&quot; + file
            break
        elif command ==&#x27;2&#x27;:
            file = open(file=&quot;../../fibonacci.py&quot;, encoding=&quot;utf8&quot;).read()
            newfile = &quot;../../fibonacci.py&quot;
            prompt = &quot;Write five unit tests for the following function.Return all code and comments.\n---\n&quot; + file
            break
        else :
            print(&quot;Invalid input. Please try again.&quot;)

    inputText = utils.getPromptInput(&quot;Task 3: Developer tasks&quot;, &quot;sample-text.txt&quot;)

    # Build messages to send to Azure OpenAI model. (Add code here)
    messages=[
            {&quot;role&quot;:&quot;system&quot;, &quot;content&quot;: &quot;You are a helpful AI assistant that helps programmers write code.&quot; + prompt}
        ]
    

    # Define argument list (Add code here)
    apiParams = {
        &quot;messages&quot;: messages,
        &quot;model&quot; : aiModel
    }
    
    utils.writeLog(&quot;API Parameters:\n&quot;, apiParams)

    # Call chat completion connection. (Add code here)
    # Use the call name and **apiParams to reference our argument list
    response = aiClient.chat.completions.create( **apiParams)
    results_file = open(file=newfile, mode=&quot;w&quot;, encoding=&quot;utf8&quot;)
    results_file.write(response.choices[0].message.content)
    
    utils.writeLog(&quot;Response:\n&quot;, str(response))
    print(&quot;Response: &quot; + response.choices[0].message.content + &quot;\n&quot;)
    return response 
```

> 这段Python代码定义了一个名为function3的函数，该函数接收两个参数：aiClient和aiModel。这个函数的主要工作流程如下：

1.  进入一个无限循环，提示用户输入一个数字，然后根据输入的数字执行相应的任务。如果用户输入的是"1"，则打开"legacyCode.py"文件，并构造一个提示信息，要求在这个函数中添加注释。如果用户输入的是"2"，则打开"fibonacci.py"文件，并构造一个提示信息，要求为这个函数编写五个单元测试。如果用户输入的不是"1"、"2"或"quit"，则提示输入无效。

1.  使用utils.getPromptInput函数从"sample-text.txt"文件中获取输入文本，该函数的第一个参数是提示信息。

1.  创建一个名为messages的列表，该列表包含一个字典，字典中有两个键值对："role"和"content"。"role"的值是"system"，表示这是一个系统消息；"content"的值是"You are a helpful AI assistant that helps programmers write code."加上前面构造的提示信息，表示这是要发送给Azure OpenAI模型的消息。

1.  创建一个名为apiParams的字典，该字典包含两个键值对："model"和"messages"。"model"的值是传入的aiModel参数，表示要使用的模型；"messages"的值是前面创建的messages列表，表示要发送的消息。

1.  使用utils.writeLog函数将apiParams字典写入日志。

1.  调用aiClient.chat.completions.create方法，传入apiParams字典，向Azure OpenAI发送请求并获取响应。这里使用了Python的**操作符，可以将字典的键值对作为命名参数传入函数。

1.  将响应中的第一个选择的消息内容写入新文件。

1.  使用utils.writeLog函数将响应写入日志。

1.  打印响应中的第一个选择的消息内容。

1.  返回响应。

> 这个函数的主要作用是根据用户的选择，向Azure OpenAI模型发送消息，并获取模型的响应，然后将响应写入新文件。

## Use company data

Subject: Use company data

From: projman@contoso.com

To: User1-40594128@cloudslice.onmicrosoft.com

Hello,

We want to extend the PoC even further and try to use our own company data to better answer customer questions.

This is your task:

You need to connect the PoC app to use the search40594128 Azure AI Search resource that contains some sample travel data. Your development must be in function4() and must meet the following requirements:

+   search40594128 must train on the data in the sa40594128/blob1 Azure Blob Storage container.

+   The index must be named pocindex.

+   The search type must be Keyword.

+   You must use a system message of “You are a helpful travel agent.”

+   Configure the search connection strings in the configuration file.

Send the following request to verify how the model answers the question: “When is the best time to visit London?”

When completing the connection by using the API, you only need to specify the necessary parameters. In Python, you may need to update the client in main() for this task.

Thanks,

Project Management Team

为了扩展概念验证（PoC）应用程序并尝试使用我们自己的公司数据以更好地回答客户问题，您需要完成以下任务：

1.  连接Azure AI Search资源：将PoC应用程序连接到名为search40594128的Azure AI Search资源，该资源包含一些样本旅行数据。

1.  **开发function4()**：所有的开发工作必须在PoC应用程序的function4()函数中完成，并满足以下要求：

- search40594128必须在sa40594128/blob1 Azure Blob Storage容器中的数据上进行训练。

- 索引必须被命名为pocindex。

- 搜索类型必须设置为“Keyword”。

- 必须使用系统消息“您是一个乐于助人的旅行代理。”

- 在配置文件中配置搜索连接字符串。

1.  **发送请求以验证模型**：发送以下请求以验证模型如何回答问题：“什么时候是访问伦敦的最佳时间？”

1.  **API参数配置**：在使用API完成连接时，只需要指定必要的参数。

1.  **更新Python客户端**：在Python中，您可能需要在main()函数中更新客户端以完成此任务。

为了实现上述要求，您可能需要执行以下步骤：

+   访问Azure Blob Storage：在function4()中编写代码以访问sa40594128/blob1容器，并使用其中的数据来训练Azure AI Search。

+   **创建和配置索引**：使用Azure AI Search的API创建一个名为pocindex的索引，并确保其搜索类型为“Keyword”。

+   **配置系统消息**：在与Azure AI Search交互的代码中设置系统消息为“您是一个乐于助人的旅行代理。”

+   **配置连接字符串**：在应用程序的配置文件中设置必要的连接字符串，以确保function4()可以正确地与Azure AI Search通信。

+   **发送验证请求**：编写代码以发送“什么时候是访问伦敦的最佳时间？”这个查询到Azure AI Search，并展示返回的答案。

+   **更新Python客户端**：如果需要，更新main()函数中的Python客户端设置，以适应新的Azure AI Search配置。

请注意，具体的实现细节将取决于您使用的编程语言和Azure AI Search的具体API调用方式。您可能需要查阅相关的API文档来了解如何创建索引、配置搜索类型、发送查询以及如何从配置文件中读取连接字符串。此外，您还需要确保您有足够的权限来访问Azure Blob Storage和Azure AI Search资源。

Task 4: Use company data 代码解析

```
def function4(aiClient, aiModel):
    inputText = utils.getPromptInput(&quot;Task 4: Use company data&quot;, &quot;sample-text.txt&quot;)

    azure_search_endpoint = os.getenv(&quot;SEARCH_ENDPOINT&quot;)
    azure_search_key = os.getenv(&quot;SEARCH_KEY&quot;)
    azure_search_index = os.getenv(&quot;SEARCH_INDEX&quot;)

    extension_config = dict(dataSources = [  
                { 
                    &quot;type&quot;: &quot;AzureCognitiveSearch&quot;, 
                    &quot;parameters&quot;: { 
                        &quot;endpoint&quot;:azure_search_endpoint, 
                        &quot;key&quot;: azure_search_key, 
                        &quot;indexName&quot;: azure_search_index,
                    }
                }]
                )
    # Build messages to send to Azure OpenAI model. (Add code here)
    messages = [
                {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;You are a helpful travel agent.&quot;},
                {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: inputText}
            ]
    

    # Define connection and argument list (Add code here)
    apiParams = {
        &quot;messages&quot;: messages,
        &quot;model&quot; : aiModel,
        &quot;extra_body&quot; : extension_config
    }
    

    utils.writeLog(&quot;API Parameters:\n&quot;, apiParams)

    # Call chat completion connection. Will be the same as function1 (Add code here)
    # Use the call name and **apiParams to reference our argument list
    response = aiClient.chat.completions.create( **apiParams)
    

    utils.writeLog(&quot;Response:\n&quot;, str(response))
    print(&quot;Response: &quot; + response.choices[0].message.content + &quot;\n&quot;)
    return

if __name__ == &#x27;__main__&#x27;: 
    main()
```

> 这段Python代码定义了一个名为function4的函数，该函数接收两个参数：aiClient和aiModel。这个函数的主要工作流程如下：

1.  使用utils.getPromptInput函数从"sample-text.txt"文件中获取输入文本，该函数的第一个参数是提示信息。

1.  从环境变量中获取Azure Cognitive Search的相关配置信息，包括端点、密钥和索引。

1.  创建一个名为extension_config的字典，该字典包含一个键值对："dataSources"，其值是一个列表，列表中包含一个字典，该字典定义了Azure Cognitive Search的数据源。

1.  创建一个名为messages的列表，该列表包含两个字典，每个字典都有两个键值对："role"和"content"。"role"的值表示消息的角色，"content"的值表示消息的内容。

1.  创建一个名为apiParams的字典，该字典包含三个键值对："messages"、"model"和"extra_body"。"messages"的值是前面创建的messages列表，表示要发送的消息；"model"的值是传入的aiModel参数，表示要使用的模型；"extra_body"的值是前面创建的extension_config字典，表示额外的请求体。

1.  使用utils.writeLog函数将apiParams字典写入日志。

1.  调用aiClient.chat.completions.create方法，传入apiParams字典，向Azure OpenAI发送请求并获取响应。这里使用了Python的**操作符，可以将字典的键值对作为命名参数传入函数。

1.  使用utils.writeLog函数将响应写入日志。

1.  打印响应中的第一个选择的消息内容。

> 这个函数的主要作用是向Azure OpenAI模型发送消息，并获取模型的响应。在发送消息之前，会从环境变量中获取Azure Cognitive Search的配置信息，并构造一个包含这些信息的数据源。

## Subject: Status - Complete

Subject: Status - Complete

From: cto@contoso.com

To: User1-40594128@cloudslice.onmicrosoft.com

Thank you for helping our team develop generative AI solutions with Azure OpenAI Service.

We look forward to working with you again.

Regards,

Chief Technology Officer (CTO

# 完成后的代码

```
import os
from dotenv import load_dotenv
import utils

# Add OpenAI import. (Add code here)
from openai import AzureOpenAI

def main(): 
        
    try:     
        load_dotenv()
        utils.initLogFile()
        azure_oai_endpoint = os.getenv(&quot;AZURE_OAI_ENDPOINT&quot;)
        azure_oai_key = os.getenv(&quot;AZURE_OAI_KEY&quot;)
        azure_oai_model = os.getenv(&quot;AZURE_OAI_MODEL&quot;)
        azure_oai_version = &quot;2023-12-01-preview&quot;
        # Define Azure OpenAI client (function1-3)
        client = AzureOpenAI( azure_endpoint=azure_oai_endpoint,
                            api_key=azure_oai_key,
                            api_version=azure_oai_version,
                            )
        # Define Azure OpenAI client (function4)
        client = AzureOpenAI( base_url=f&quot;{azure_oai_endpoint}/openai/deployments/{azure_oai_model}/extensions&quot;,
                            api_key=azure_oai_key,
                            api_version=azure_oai_version,
                            )
        
        

        function_map = {
            &quot;1&quot;: function1,
            &quot;2&quot;: function2,
            &quot;3&quot;: function3,
            &quot;4&quot;: function4
        }

        while True:
            print(&#x27;1: Validate PoC\n&#x27; +
                  &#x27;2: Company chatbot\n&#x27; +
                  &#x27;3: Developer tasks\n&#x27; +
                  &#x27;4: Use company data\n&#x27; +
                  &#x27;\&#x27;quit\&#x27; to exit the program\n&#x27;)
            command = input(&#x27;Enter a number:&#x27;)
            if command.strip() in function_map:
                function_map[command](client, azure_oai_model)
            elif command.strip().lower() == &#x27;quit&#x27;:
                print(&#x27;Exiting program...&#x27;)
                break
            else :
                print(&quot;Invalid input. Please enter number 1, 2, 3, 4, or 5.&quot;)

    except Exception as ex:
        print(ex)

# Task 1: Validate PoC
def function1(aiClient, aiModel):
    inputText = utils.getPromptInput(&quot;Task 1: Validate PoC&quot;, &quot;sample-text.txt&quot;)
    
    # Build messages to send to Azure OpenAI model. (Add code here)
    messages=[
            {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;You are a helpful assistant.&quot; + inputText}
        ]
    

    # Define argument list (Add code here)
    apiParams = {
        &quot;model&quot; : aiModel,
        &quot;messages&quot;: messages,
        
    }
    

    utils.writeLog(&quot;API Parameters:\n&quot;, apiParams)

    # Call chat completion connection. (Add code here)
    # Use the call name and **apiParams to reference our argument list
    response = aiClient.chat.completions.create( **apiParams)
    

    utils.writeLog(&quot;Response:\n&quot;, str(response))
    print(&quot;Response: &quot; + response.choices[0].message.content + &quot;\n&quot;)
    return response

# Task 2: Company chatbot
def function2(aiClient, aiModel):
    inputText = utils.getPromptInput(&quot;Task 2: Company chatbot&quot;, &quot;sample-text.txt&quot;)
    
    # Build messages to send to Azure OpenAI model. (Add code here)
    messages=[
            {&quot;role&quot;:&quot;system&quot;, &quot;content&quot;: &quot;You are a helpful assistant.You must reply in English and Spanish, in a casual tone.&quot;},
            {&quot;role&quot;:&quot;user&quot;,&quot;content&quot;:&quot; Where can I find the company phone number?&quot;},
            {&quot;role&quot;:&quot;assistant&quot;,&quot;content&quot;:&quot; You can find it on the footer of every page on our website. Hope that helps! Thanks for using Contoso, Ltd.&quot;  },
            {&quot;role&quot;:&quot;user&quot;,&quot;content&quot;:&quot;Please reply using the following format\n###\nEnglish Reply: ......\n\nSpanish Reply......\n###&quot; + inputText},
        ]
    

    # Define argument list (Add code here)
    apiParams = {
        &quot;messages&quot;: messages,
        &quot;model&quot; : aiModel,
        &quot;temperature&quot;: 0.5,
        &quot;max_tokens&quot;: 1000,
    }
    

    utils.writeLog(&quot;API Parameters:\n&quot;, apiParams)

    # Call chat completion connection. (Add code here)
    # Use the call name and **apiParams to reference our argument list
    response = aiClient.chat.completions.create( **apiParams)
    

    utils.writeLog(&quot;Response:\n&quot;, str(response))
    print(&quot;Response: &quot; + response.choices[0].message.content + &quot;\nHope that helps! Thanks for using Contoso, Ltd.&quot;)
    return response

def function3(aiClient, aiModel):
    while True:
        print(&#x27;\n1: Add comments to my function\n&#x27; +
                &#x27;2: Write unit tests for my function\n&#x27; +
                &#x27;\&quot;quit\&quot; to exit the program\n&#x27;)
        command = input(&#x27;Enter a number to select a task:&#x27;)
        if command == &#x27;1&#x27;:
            file = open(file=&quot;../../legacyCode.py&quot;, encoding=&quot;utf8&quot;).read()
            newfile = &quot;../../legacyCode.py&quot;
            prompt = &quot;Add comments to the following function. Return all code and comments.\n---\n&quot; + file
            break
        elif command ==&#x27;2&#x27;:
            file = open(file=&quot;../../fibonacci.py&quot;, encoding=&quot;utf8&quot;).read()
            newfile = &quot;../../fibonacci.py&quot;
            prompt = &quot;Write five unit tests for the following function.Return all code and comments.\n---\n&quot; + file
            break
        else :
            print(&quot;Invalid input. Please try again.&quot;)

    inputText = utils.getPromptInput(&quot;Task 3: Developer tasks&quot;, &quot;sample-text.txt&quot;)

    # Build messages to send to Azure OpenAI model. (Add code here)
    messages=[
            {&quot;role&quot;:&quot;system&quot;, &quot;content&quot;: &quot;You are a helpful AI assistant that helps programmers write code.&quot; + prompt}
        ]
    

    # Define argument list (Add code here)
    apiParams = {
        &quot;messages&quot;: messages,
        &quot;model&quot; : aiModel
    }
    
    utils.writeLog(&quot;API Parameters:\n&quot;, apiParams)

    # Call chat completion connection. (Add code here)
    # Use the call name and **apiParams to reference our argument list
    response = aiClient.chat.completions.create( **apiParams)
    results_file = open(file=newfile, mode=&quot;w&quot;, encoding=&quot;utf8&quot;)
    results_file.write(response.choices[0].message.content)
    
    utils.writeLog(&quot;Response:\n&quot;, str(response))
    print(&quot;Response: &quot; + response.choices[0].message.content + &quot;\n&quot;)
    return response 

def function4(aiClient, aiModel):
    inputText = utils.getPromptInput(&quot;Task 4: Use company data&quot;, &quot;sample-text.txt&quot;)

    azure_search_endpoint = os.getenv(&quot;SEARCH_ENDPOINT&quot;)
    azure_search_key = os.getenv(&quot;SEARCH_KEY&quot;)
    azure_search_index = os.getenv(&quot;SEARCH_INDEX&quot;)

    extension_config = dict(dataSources = [  
                { 
                    &quot;type&quot;: &quot;AzureCognitiveSearch&quot;, 
                    &quot;parameters&quot;: { 
                        &quot;endpoint&quot;:azure_search_endpoint, 
                        &quot;key&quot;: azure_search_key, 
                        &quot;indexName&quot;: azure_search_index,
                    }
                }]
                )
    # Build messages to send to Azure OpenAI model. (Add code here)
    messages = [
                {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;You are a helpful travel agent.&quot;},
                {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: inputText}
            ]
    

    # Define connection and argument list (Add code here)
    apiParams = {
        &quot;messages&quot;: messages,
        &quot;model&quot; : aiModel,
        &quot;extra_body&quot; : extension_config
    }
    

    utils.writeLog(&quot;API Parameters:\n&quot;, apiParams)

    # Call chat completion connection. Will be the same as function1 (Add code here)
    # Use the call name and **apiParams to reference our argument list
    response = aiClient.chat.completions.create( **apiParams)
    

    utils.writeLog(&quot;Response:\n&quot;, str(response))
    print(&quot;Response: &quot; + response.choices[0].message.content + &quot;\n&quot;)
    return

if __name__ == &#x27;__main__&#x27;: 
    main()

```