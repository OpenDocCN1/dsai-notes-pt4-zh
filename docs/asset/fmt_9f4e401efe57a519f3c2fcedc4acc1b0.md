
```
"""
# Initialize the result variable

result = 0

# Update a and b to absolute values for the multiplication process

a = abs(a)

b_abs = abs(b)

# Perform the multiplication as repeated addition

while b_abs > 0:

    result += a

    b_abs -= 1

# Determine the sign of the result based on the product of a and b

# Return the positive result if a and b have the same sign (a*b >= 0)

# Return the negative result if a and b have different signs (a*b < 0)

return result if a*b >= 0 else -result
```

这些注释提供了函数的基本信息，包括它的功能，参数，返回值，以及关键步骤的详细说明。

## 按照实验手册完成编辑 `code-generation.py` 文件如下

```
import os
from dotenv import load_dotenv

# Add Azure OpenAI package
from openai import AzureOpenAI

# Set to True to print the full response from OpenAI for each call
printFullResponse = False

def main(): 
        
    try: 
    
        # Get configuration settings 
        load_dotenv()
        azure_oai_endpoint = os.getenv("AZURE_OAI_ENDPOINT")
        azure_oai_key = os.getenv("AZURE_OAI_KEY")
        azure_oai_model = os.getenv("AZURE_OAI_MODEL")
        
        # Configure the Azure OpenAI client        
        client = AzureOpenAI(
                azure_endpoint = azure_oai_endpoint, 
                api_key=azure_oai_key,  
                api_version="2023-05-15"
                )

        while True:
            print('\n1: Add comments to my function\n' +
                '2: Write unit tests for my function\n' +
                '3: Fix my Go Fish game\n' +
                '"quit" to exit the program\n')
            command = input('Enter a number to select a task:')
            if command == '1':
                file = open(file="../sample-code/function/function.py", encoding="utf8").read()
                prompt = "Add comments to the following function. Return only the commented code.\n---\n" + file
                call_openai_model(prompt, model=azure_oai_model, client=client)
            elif command == '2':
                file = open(file="../sample-code/function/function.py", encoding="utf8").read()
                prompt = "Write four unit tests for the following function.\n---\n" + file
                call_openai_model(prompt, model=azure_oai_model, client=client)
            elif command == '3':
                file = open(file="../sample-code/go-fish/go-fish.py", encoding="utf8").read()
                prompt = "Fix the code below for an app to play Go Fish with the user. Return only the corrected code.\n---\n" + file
                call_openai_model(prompt, model=azure_oai_model, client=client)
            elif command.lower() == 'quit':
                print('Exiting program...')
                break
            else :
                print("Invalid input. Please try again.")

    except Exception as ex:
        print(ex)

def call_openai_model(prompt, model, client):
    # Provide a basic user message, and use the prompt content as the user message
    system_message = "You are a helpful AI assistant that helps programmers write code."
    user_message = prompt

    # Format and send the request to the model    
    messages =[
        {"role": "system", "content": system_message},
        {"role": "user", "content": user_message},
    ]
        
    # Call the Azure OpenAI model
    response = client.chat.completions.create(
        model=model,
        messages=messages,
        temperature=0.7,
        max_tokens=1000
    )
    
    # Print the response to the console, if desired
    if printFullResponse:
        print(response)

    # Write the response to a file
    results_file = open(file="result/app.txt", mode="w", encoding="utf8")
    results_file.write(response.choices[0].message.content)
    print("\nResponse written to result/app.txt\n\n")

if __name__ == '__main__': 
    main()
```

> 这个Python脚本主要用于与Azure OpenAI进行交互，以实现一些代码生成的任务。以下是其主要功能的概述：
>
> 1. 环境变量加载：使用`dotenv`库加载环境变量，这些环境变量包括Azure OpenAI的端点、密钥和模型名称。
> 2. Azure OpenAI客户端配置：使用上述环境变量创建一个Azure OpenAI客户端。
> 3. 用户交互：通过无限循环，让用户可以选择要执行的任务。用户可以选择添加函数注释、为函数编写单元测试、修复Go Fish游戏代码，或退出程序。
> 4. OpenAI模型调用：根据用户的选择，构建一个提示（`prompt`），并将其发送给OpenAI模型。模型的响应会被写入到一个文件中。
> 5. 异常处理：如果在执行上述任务过程中出现任何异常，该异常会被捕获并打印出来。
>
> 这个脚本的主要目的是利用OpenAI的能力来帮助生成或修改代码。

### 运行效果

```
PS D:\vscodelab\mslearn-openai\Labfiles\04-code-generation\Python> python code-generation.py

1: Add comments to my function
2: Write unit tests for my function
3: Fix my Go Fish game
"quit" to exit the program

Enter a number to select a task:1

Response written to result/app.txt

1: Add comments to my function
2: Write unit tests for my function
3: Fix my Go Fish game
"quit" to exit the program

Enter a number to select a task:2

Response written to result/app.txt

1: Add comments to my function
2: Write unit tests for my function
3: Fix my Go Fish game
"quit" to exit the program

Enter a number to select a task:3

Response written to result/app.txt

1: Add comments to my function
2: Write unit tests for my function
3: Fix my Go Fish game
"quit" to exit the program

Enter a number to select a task:quit
Exiting program...
```

### 1: Add comments to my function

```
def absolute_square(num1, num2):
    # Calculate the absolute difference between num1 and num2
    result = abs(num1 - num2)
    # Square the result
    result *= result
    # Return the squared result
    return result
```

### 2: Write unit tests for my function

Sure, here are four unit tests for the given function using the `unittest` framework in Python:

```
import unittest

def absolute_square(num1, num2):
    result = abs(num1 - num2)
    result *= result
    return result

class TestAbsoluteSquare(unittest.TestCase):

    def test_positive_numbers(self):
        self.assertEqual(absolute_square(5, 3), 4)

    def test_negative_numbers(self):
        self.assertEqual(absolute_square(-5, -3), 4)

    def test_mixed_numbers(self):
        self.assertEqual(absolute_square(-5, 3), 64)

    def test_zero(self):
        self.assertEqual(absolute_square(0, 0), 0)

if __name__ == '__main__':
    unittest.main()
```

In the above tests, we are testing the function `absolute_square` with positive numbers, negative numbers, mixed numbers, and zero to ensure that it returns the expected results.

### 3: Fix my Go Fish game

```
while len(deck) > 0:
```

## 使用 Azure OpenAI 服务生成图像

> [注意：启用DALL-E模型需要申请：](https://customervoice.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR7en2Ais5pxKtso_Pz4b1_xUNTZBNzRKNlVQSFhZMU9aV09EVzYxWFdORCQlQCN0PWcu)Request Access to Azure OpenAI Service

### 什么是 DALL-E？

DALL-E 是一个基于神经网络的模型，可以从自然语言输入生成图形数据。 更简单地说，你可以向 DALL-E 提供描述，它可以生成相应的图像。

DALL-E 生成的图像是原创的；不是从特选图像目录中检索的。 换句话说，DALL-E 不是用于查找合适图像的搜索系统 - 它是一种人工智能 (AI) 模型，可根据训练数据生成新图像。

### 探索 Azure OpenAI Studio 中的 DALL-E

若要试用 DALL-E，可以在已批准访问该服务的 Azure 订阅中预配 Azure OpenAI 服务资源，并使用 DALL-E 操场提交提示并查看生成的图像。

使用操场时，可以调整设置以指定：

+   为响应提示而生成的图像数量（默认图像数量为一个）。

+   生成的图像的分辨率（大小）。 可用的大小有 256x256、512x512 或 1024x1024（此为默认值）。

### 通过 Azure OpenAI REST API 使用 DALL-E 模型

可以通过 Azure OpenAI 服务 REST API 从应用程序中使用 DALL-E 模型。

若要对服务进行 REST 调用，需要 Azure 中预配的 Azure OpenAI 服务资源的终结点和授权密钥。 通过将 POST 请求提交到标头中的授权密钥的服务终结点来启动图像生成过程。 请求必须在 JSON 正文中包含以下参数：

+   `prompt`：对要生成的图像的说明。

+   `n`：要生成的图像数。

+   `size`：要生成的图像的分辨率（256x256、512x512 或 1024x1024）

例如，下面的 JSON 可以用来生成一个 512 x 512 的獾穿着燕尾服的图像：

```
{
    "prompt": "A badger wearing a tuxedo",
    "n": 1,
    "size": "512x512"
}
```
