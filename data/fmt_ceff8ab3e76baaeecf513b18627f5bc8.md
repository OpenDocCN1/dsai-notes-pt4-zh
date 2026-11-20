
# 提示

提示是模型构建的前导字，通常有助于朝着正确方向塑造响应。它们通常与说明一起使用，但并不总是如此。提示模型进行代码生成时，提示特别有用。当前的 Azure OpenAI 模型可以生成一些有趣的代码片段，但未来模块中将更深入地介绍代码生成。

## 提供上下文以提高准确性

通过向 AI 模型提供上下文，使模型能够更好地了解你的问题或应该知道的内容，从而提供最佳答案。可以通过多种方式提供上下文。

## 请求输出组合

指定输出的结构可能会对结果产生很大影响。这可能包括要求模型引用其来源、将响应编写为电子邮件、将响应格式设置为 SQL 查询、将情绪分类为特定结构等。

## 系统消息

系统消息包含在提示的开头，旨在提供模型说明、回答的角度或其他有助于指导模型响应的信息。可以把它看作是对assistant的耳语。此系统消息可能包括音调或个性、不应包含的主题或如何回答的详细信息（例如，格式设置）。

例如，可以向它提供以下一些系统消息：

+   “我希望你像命令行终端一样操作。 在一个唯一代码块中完全像 `cmd.exe` 一样响应命令，没有其他要求。”

+   “我希望你成为一名翻译，从英语翻译成西班牙语。 不要回复我说或问的任何内容，只翻译这两种语言，并用翻译的文本回复。”

+   “作为励志演说家，自由地就目标和挑战给出令人鼓舞的建议。 你应该包括许多积极的肯定和建议的活动，以实现用户的最终目标。”

Azure OpenAI Studio 中聊天窗口顶部提供了其他示例系统消息。尝试定义自己的系统提示以指定唯一响应，并与模型聊天以查看响应的不同之处。

## 会话历史记录

除了系统消息外，还可以向模型提供其他消息以增强会话。对话历史记录使模型能够继续以类似方式响应（例如，音调或格式设置），并允许用户在后续查询中引用以前的内容。可以通过两种方式提供此历史记录：从实际聊天历史记录，或从用户定义的示例对话。

使用 OpenAI 模型的聊天接口（例如，ChatGPT 和 Azure OpenAI Studio 中的聊天操场）自动包含对话历史记录，从而生成更丰富、更有意义的对话。在 Azure OpenAI Studio 聊天操场的聊天窗口下方的“参数”部分中，可以指定要包含的过去消息数。尝试将该值减少到 1 或增加到最大值，以查看不同历史记录量对对话的影响。

聊天系统还可以利用模型的汇总功能来节省输入令牌。应用可以选择汇总过去的消息，并将该摘要包含在对话历史记录中，然后仅向模型逐字提供过去几条消息。

## 少样本学习

使用用户定义的示例对话就是所谓的“少样本学习”，它提供应如何响应给定查询的模型示例。这些示例用于训练模型如何响应。

## 拆分复杂任务

改进交互的另一种方法是将复杂的提示划分为多个查询。这使模型能够更好地了解每个单独的部分，并且可以提高整体准确性。通过划分提示，还可以在将来的提示中包含来自上一个提示的响应，以及使用该信息以及模型功能生成相关响应。

## 思维链

+   帮助你有效地拆分任务的一个有用方法是让模型解释其思维链。

+   要求模型使用确定响应的分步过程做出响应是了解模型如何解释提示的有用方法。通过执行此操作，你可以看到模型在何处进行了错误的逻辑轮次，并更好地了解如何更改提示以避免错误。此方法可能包括要求它引用其来源，就像必应聊天（使用 GPT-4 代模型），并给出确定答案的原因。

+   思维链提示技术最适合用于帮助你循环访问和改进提示，以便从模型获得最高质量的答案。

## 练习 - 在应用程序中利用提示工程

`https://microsoftlearning.github.io/mslearn-openai/Instructions/Exercises/03-prompt-engineering.html`

在使用 Azure OpenAI 服务时，开发人员如何塑造他们的提示会极大地影响生成式 AI 模型的回应。如果以清晰简明的方式提出请求，Azure OpenAI 模型能够根据要求定制和格式化内容。在这个练习中，您将学习到不同的类似内容提示如何帮助塑造 AI 模型对需求更好满足的回应。

### 初始提示词

```
判断以下文章的分类
---
加州可能面临严重干旱

数百万加利福尼亚居民正为水资源减少和草坪干涸做好准备，因为干旱威胁着该地区大片土地的日益缺水问题。

南加州官员对干旱严重程度作出了非同寻常的表态，限制近800万居民每周只能使用一天户外用水。

关于人们如何适应更加干燥的生活方式还有很多待确定之处。但官员们警告说情况十分紧急，并可能导致今年晚些时候实施更严格的限制措施。
```

### 设置系统角色

```
你是一个新闻聚合器，对新闻文章进行分类。不需要解释你的分类原因，只要给出最佳的新闻类别即可，谢谢。
```

### 再次尝试

```
这是什么类型的文章？
---
纽约棒球队大胜芝加哥
    
昨晚，纽约棒球队以5-0的比分完胜芝加哥旋风队，在第七局下半场末尾打出一支三分全垒打，巩固了他们的胜利。
    
纽约投手马里奥·罗杰斯只用96个投球就完成了比赛，并且只被击中两次，这是他今年表现最好的一场比赛。
    
芝加哥旋风队在第二局和第五局击中两次但未能将跑者送回得分。
```

### 又来一个

```
分类这篇文章：
---
奥斯卡颁奖典礼上的欢乐时刻

过去一周的奥斯卡颁奖典礼真是太精彩了！

尽管某个丑闻可能抢走了风头，但今年的学院奖充满了让我们感到喜悦甚至流泪的瞬间。这些演员们带来了一些真正令人动容的表演，以及一些非常有趣的笑料，让我们度过寒冷的冬天。

从罗宾·克莱恩创造历史性胜利到凯西·詹森本人全程表演，不要错过明天重播所有庆祝活动。
```

### 切换系统角色为信息助手

```
接下来请您扮演一个AI助手，帮助人们找到信息。
```

```
1. 创建一个动物列表
2. 创建一个为这些动物取奇特名字的列表
3. 将它们随机组合成一个包含25个动物和名字配对的列表
```

### 一个可能的输出

```
好的，让我们开始创建这些列表。首先，我将创建一个动物列表。然后，我将为这些动物创建一个奇特的名字列表。最后，我将随机地将这两个列表组合在一起。

1. 动物列表：

Lion
Tiger
Elephant
Giraffe
Penguin
Kangaroo
Cheetah
Polar Bear
Zebra
Rhino

2. 奇特名字列表：

Sparkle
Blue Moon
Tiny Dancer
Jolly Jumper
Feathered Flash
Rainbow Warrior
Star Striker
Cosmic Crusader
Prancing Prince
Zig-Zag Zappy
```

对于第三个请求，因为我是一个文本的AI而不是编程的AI，我无法生成随机的配对。但你可以通过简单地将每个动物的名称对应到奇特名称的相同位置来创建一个配对列表。如果你需要更多的配对，你可以循环使用动物和名称列表。

### 切换系统角色为Python编程助手

```
接下来请您扮演一个编码助手，帮助编写Python代码。
```

```
1. 创建一个动物列表
2. 创建一个为这些动物取奇特名字的列表
3. 将它们随机组合成一个包含25个动物和名字配对的列表
```

### 一个可能的输出

好的，这是一个简单的Python脚本，可以实现你所需要的功能：

```python
import random

# 创建动物列表
animals = ['Lion', 'Tiger', 'Elephant', 'Giraffe', 'Penguin', 'Kangaroo', 'Cheetah', 'Polar Bear', 'Zebra', 'Rhino']

# 创建奇特名字列表
names = ['Sparkle', 'Blue Moon', 'Tiny Dancer', 'Jolly Jumper', 'Feathered Flash', 'Rainbow Warrior', 'Star Striker', 'Cosmic Crusader', 'Prancing Prince', 'Zig-Zag Zappy']

# 生成随机对应关系
pairs = [(random.choice(animals), random.choice(names)) for _ in range(25)]

for pair in pairs:
    print(pair)
```

在上述代码中，我们首先导入了`random`模块，该模块包含各种能生成随机数的函数。然后我们创建了动物和名字的列表，之后使用一个列表推导式生成了25个动物和奇怪名字的随机对。`random.choice`函数从列表中随机选择一个元素。前两个列表长度均为10，因此我们可以生成25个不同的组合，尽管在那之后的组合可能会有重复的情况。最后，我们打印出每对动物和其对应的奇怪名字。

### 按照实验手册完成编辑`prompt-engineering.py`文件如下

```python
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
            print('1: Basic prompt (no prompt engineering)\n' +
                  '2: Prompt with email formatting and basic system message\n' +
                  '3: Prompt with formatting and specifying content\n' +
                  '4: Prompt adjusting system message to be light and use jokes\n' +
                  '\'quit\' to exit the program\n')
            command = input('Enter a number:')
            if command == '1':
                call_openai_model(messages="../prompts/basic.txt", model=azure_oai_model, client=client)
            elif command == '2':
                call_openai_model(messages="../prompts/email-format.txt", model=azure_oai_model, client=client)
            elif command == '3':
                call_openai_model(messages="../prompts/specify-content.txt", model=azure_oai_model, client=client)
            elif command == '4':
                call_openai_model(messages="../prompts/specify-tone.txt", model=azure_oai_model, client=client)
            elif command.lower() == 'quit':
                print('Exiting program...')
                break
            else :
                print("Invalid input. Please try again.")

    except Exception as ex:
        print(ex)

def call_openai_model(messages, model, client):
    # In this sample, each file contains both the system and user messages
    # First, read them into variables, strip whitespace, then build the messages array
    file = open(file=messages, encoding="utf8")
    system_message = file.readline().split(':', 1)[1].strip()
    user_message = file.readline().split(':', 1)[1].strip()

    # Print the messages to the console
    print("System message: " + system_message)
    print("User message: " + user_message)

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
        max_tokens=800
    )
    

    if printFullResponse:
        print(response)

    print("Completion: \n\n" + response.choices[0].message.content + "\n")

if __name__ == '__main__': 
    main()
```

> 这个 Python 文件主要包含两个函数：`main` 和 `call_openai_model`。它们的功能如下：

1. `main` 函数：

> 这是程序的入口点。它首先从环境变量中获取 Azure OpenAI 的配置设置，然后使用这些设置创建一个 `AzureOpenAI` 客户端。接着，它进入一个无限循环，等待用户输入命令。根据用户的输入，它会调用 `call_openai_model` 函数，传入不同的消息文件和模型。如果用户输入 `'quit'`，则程序退出。

2. `call_openai_model` 函数：

> 这个函数负责调用 Azure OpenAI 模型。它首先从文件中读取系统消息和用户消息，然后将这些消息发送给 Azure OpenAI 模型。模型的响应会被打印出来。

> 这个文件的主要功能是通过 Azure OpenAI 模型生成文本。用户可以通过输入不同的命令，来生成不同类型的文本。例如，如果用户输入 `'1'`，则会生成基本的提示文本；如果用户输入 `'2'`，则会生成带有电子邮件格式和基本系统消息的提示文本，等等。

### 运行效果
