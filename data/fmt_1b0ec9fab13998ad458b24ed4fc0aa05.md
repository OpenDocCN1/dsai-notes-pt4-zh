
> 注意，这段代码中的`function1`、`function2`、`function3`和`function4`并未在代码中定义，可能是在其他地方定义的，或者是需要你自己定义的。

### Task 1: Validate PoC 代码解析

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

> 这段Python代码定义了一个名为`function1`的函数，该函数接收两个参数：`aiClient`和`aiModel`。这个函数的主要工作流程如下：

1.  使用`utils.getPromptInput`函数从`sample-text.txt`文件中获取输入文本，该函数的第一个参数是提示信息。

1.  创建一个名为`messages`的列表，该列表包含一个字典，字典中有两个键值对：`"role"`和`"content"`。`"role"`的值是`"system"`，表示这是一个系统消息；`"content"`的值是`"You are a helpful assistant."`加上输入的文本，表示这是要发送给Azure OpenAI模型的消息。

1.  创建一个名为`apiParams`的字典，该字典包含两个键值对：`"model"`和`"messages"`。`"model"`的值是传入的`aiModel`参数，表示要使用的模型；`"messages"`的值是前面创建的`messages`列表，表示要发送的消息。

1.  使用`utils.writeLog`函数将`apiParams`字典写入日志。

1.  调用`aiClient.chat.completions.create`方法，传入`apiParams`字典，向Azure OpenAI发送请求并获取响应。这里使用了Python的`**`操作符，可以将字典的键值对作为命名参数传入函数。

1.  使用`utils.writeLog`函数将响应写入日志。

1.  打印响应中的第一个选择的消息内容。

1.  返回响应。

> 这个函数的主要作用是向Azure OpenAI模型发送消息，并获取模型的响应。

## Company chatbot

Subject: Company chatbot

From: projman@contoso.com

To: User1-40594128@cloudslice.onmicrosoft.com

Hello,

Executives are very excited about the potential of Azure OpenAI. We need you to further develop the PoC app to explore how we can use it.

This is your task:

You need to develop the app you worked on previously, but this time, make changes in `function2()`. The app must meet the following requirements:

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

您需要开发您之前工作过的应用程序，但这次，在`function2()`中进行更改。应用程序必须满足以下要求：

+   每个响应必须使用随意的语气，并以“希望这有帮助！感谢您使用Contoso, Ltd.”结束。

+   响应必须限制在1,000个令牌以内，温度必须为0.5。

+   至少必须提供一个示例与提示一起，例如：

+   提示：我在哪里可以找到公司电话号码？

+   响应：您可以在我们网站的每个页面的页脚上找到它。希望这有帮助！感谢您使用Contoso, Ltd。

+   使用提示工程技巧询问以下问题，并以英语和西班牙语两种语言获得响应：

+   “找到公司是否招聘的最佳方式是什么？”

对于这个任务，您可能需要使用Azure OpenAI的多语言支持功能，以及调整您的应用程序以生成符合上述要求的响应。您可能需要对`function2()`进行编码，以确保它能够接收提示，然后生成一个符合所需语气和限制的响应。此外，您可能需要利用Azure OpenAI的翻译功能或集成一个翻译服务来提供西班牙语的响应。

在实现这个功能时，您可能需要考虑以下几点：

1.  令牌限制：确保生成的响应不超过1,000个令牌。

1.  温度设置：在调用Azure OpenAI服务时，设置温度为0.5，这将影响生成文本的创造性。

1.  多语言支持：如果Azure OpenAI支持多语言生成，您可以用英语生成响应，然后翻译成西班牙语，或者直接用西班牙语生成响应。

1.  提示工程：精心设计提示，以确保问题被正确理解和回答。

1.  一致性：确保每个响应都以指定的结束语结束。

> 请注意，具体的实现细节将取决于您使用的编程语言和Azure OpenAI的具体API调用方式。您可能需要查阅相关的API文档来了解如何设置温度、生成响应、以及如何使用翻译功能。

### Task 2: Company chatbot 代码解析

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

> 这段Python代码定义了一个名为`function2`的函数，该函数接收两个参数：`aiClient`和`aiModel`。这个函数的主要工作流程如下：

1.  使用`utils.getPromptInput`函数从`sample-text.txt`文件中获取输入文本，该函数的第一个参数是提示信息。

1.  创建一个名为`messages`的列表，该列表包含四个字典，每个字典都有两个键值对：`"role"`和`"content"`。这些字典表示一个对话的过程，其中`"system"`的消息设定了对话的背景，`"user"`的消息是用户的问题，`"assistant"`的消息是助手的回答。

1.  创建一个名为`apiParams`的字典，该字典包含四个键值对：`"model"`、`"messages"`、`"temperature"`和`"max_tokens"`。`"model"`的值是传入的`aiModel`参数，表示要使用的模型；`"messages"`的值是前面创建的`messages`列表，表示要发送的消息；`"temperature"`的值是0.5，表示生成的文本的随机性；`"max_tokens"`的值是1000，表示生成的文本的最大长度。

1.  使用`utils.writeLog`函数将`apiParams`字典写入日志。

1.  调用`aiClient.chat.completions.create`方法，传入`apiParams`字典，向Azure OpenAI发送请求并获取响应。这里使用了Python的`**`操作符，可以将字典的键值对作为命名参数传入函数。

1.  使用`utils.writeLog`函数将响应写入日志。

1.  打印响应中的第一个选择的消息内容，然后添加一句希望对方得到帮助的话。

1.  返回响应。

> 这个函数的主要作用是模拟一个对话的过程，向Azure OpenAI模型发送消息，并获取模型的响应。

## Developer tasks

Subject: Developer tasks

From: projman@contoso.com

To: User1-40594128@cloudslice.onmicrosoft.com

Hello,

We are very excited about the potential of the PoC app, and our developers want to see what it can do to help with development.

This is your task:

You need to use `function3()` of the PoC app to ensure that the following tasks can be successfully completed:

+   Take the legacy code in `legacyCode.py`, add comments, and generate documentation.

+   Generate five unit tests for the function in `fibonacci.py`

Once you have modified `function3`, submit individual code generation requests for each task to Azure OpenAI by using the PoC app. The code files are in `C:\files`. It is recommended that you modify the prompt in `sample-text.txt` to accomplish each task.

Thanks,

Project Management Team

对于这个任务，您需要使用PoC应用程序中的`function3()`来确保能够成功完成以下工作：

1.  添加注释和生成文档：处理位于`legacyCode.py`中的遗留代码，为其添加注释，并生成相应的文档。这通常涉及到理解代码的功能，并编写清晰、准确的注释来解释代码的工作原理，以及可能的用例。此外，您可能需要使用特定的工具或库来从注释中生成正式的文档。

1.  生成单元测试：为`fibonacci.py`中的函数生成五个单元测试。单元测试是用来验证代码某个特定部分（如一个函数）正确性的自动化测试。在这种情况下，您需要创建测试用例来验证斐波那契数列生成函数的正确性。

为了完成这些任务，您需要修改`function3()`，以便它能够通过PoC应用程序向Azure OpenAI提交单独的代码生成请求。这里是一些可能的步骤：

+   修改`function3()`：确保`function3()`能够接收`legacyCode.py`和`fibonacci.py`作为输入，并根据需要生成相应的输出。

+   设计提示：修改`sample-text.txt`中的提示，使其能够指导Azure OpenAI生成所需的注释、文档和单元测试。这可能需要一些提示工程技术，以确保生成的内容符合您的要求。

+   提交请求：使用修改后的`function3()`和更新后的提示，通过PoC应用程序向Azure OpenAI提交请求。

+   验证结果：一旦Azure OpenAI返回结果，验证它们是否满足任务要求。对于文档和注释，检查它们是否清晰、准确，并增强了代码的可读性。对于单元测试，确保它们全面覆盖了`fibonacci.py`中函数的关键功能，并能够作为有效的回归测试。

> 请注意，具体的实现细节将取决于PoC应用程序的当前设计和Azure OpenAI的API文档。您可能需要与开发团队紧密合作，以确保正确地实现所需的功能。此外，您可能还需要具备一些编程知识，以便能够理解和修改相关的Python代码。

### Task 3: Developer tasks 代码解析
