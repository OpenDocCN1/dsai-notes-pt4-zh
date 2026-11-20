
如果你有一组文章并提取出主题，那么你也可以使用大型语言模型来帮助你索引不同的主题。假设我们是一个新闻网站，这是我们追踪的主题：NASA，当地政府，工程，员工满意度和联邦政府。假设你想弄清楚给定一篇新闻文章，其中包含哪些主题。使用以下提示：确定以下主题列表中的每个项目是否是文本下的主题。为每个主题提供0或1的答案列表。只需使用一个提示，就能够确定新闻文章涉及的主题，并可以将此信息放入字典中，以便快速处理任何文章，确定其主题，并在主题包括NASA时打印出新NASA故事的警报。值得注意的是，上述提示并不是非常健壮的。

如果我去生产系统，我可能会让它以JSON格式输出答案，而不是作为列表，因为大型语言模型的输出可能有些不一致。因此，这实际上是一段相当脆弱的代码。但是如果您想，在看完本视频后，可以尝试修改此提示，使其输出JSON而不是像这样的列表，从而有一个更健壮的方法来确定更大的文章是否是关于NASA的故事。

这就是推断的内容，只需要几分钟的时间，您就可以构建多个用于对文本进行推断的系统，而以前这可能需要一个熟练的机器学习开发人员几天甚至几周的时间。因此，我认为这非常令人兴奋，无论是对于熟练的机器学习开发人员还是对于新手来说，您现在都可以使用提示来快速构建并开始推断这些相当复杂的自然语言处理任务。在下一个视频中，我们将继续讨论可以使用大型语言模型完成的一些令人兴奋的事情，并继续介绍转换。您如何将一段文本转换为另一段文本，例如将其翻译成另一种语言？让我们进入下一个视频。

# 文本转换 / Transforming

![](img/78a7dbb4571ccdc591a666af281af6c1.png)

[prompt_eng_06_transforming.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KRzdbliFYoPWFqxN13ocOSkGnJc)

大型语言模型非常擅长将其输入转换为不同的格式，例如输入一段文本，将其转换或翻译为另一种语言，或帮助拼写和语法纠正，即输入可能不完全符合语法的文本，帮助您稍微修复一下，甚至可以转换格式，例如输入HTML并输出JSON。因此，我曾经写过一些应用程序，用了很多常规表达式，现在使用大型语言模型和几个提示可以更简单地实现这些应用程序。

我现在几乎用Chat GPT来校对我写的所有东西，所以我很高兴现在在笔记本上向您展示更多的示例。因此，首先我们将导入OpenAI并使用在视频中一直使用的`getCompletion`帮助函数。

## 环境准备

```python
import openai
import os
from dotenv import load_dotenv, find_dotenv

_ = load_dotenv(find_dotenv()) # read local .env file

openai.api_key  = os.getenv('OPENAI_API_KEY')
```

```python
def get_completion(prompt, model="gpt-3.5-turbo", temperature=0): 
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=temperature, 
    )
    return response.choices[0].message["content"]
```

## 翻译

我们将首先做一个翻译任务。大型语言模型通常在许多来源的大量文本上进行训练，其中许多文本都是来自互联网，而这些文本又是不同的语言。因此，这使得模型具有进行翻译的能力。这些模型掌握了数百种语言，但掌握程度各不相同。接下来，我们将介绍如何使用这种能力的一些示例。

让我们从一些简单的内容开始。在第一个例子中，提示是将以下英文文本翻译为西班牙文。`Hi, I would like to order a blender` 的响应是`Hola，me gustaría ordenar una licuadora`。在这里，我非常抱歉所有的西班牙语听众，我不会西班牙语，这一点您肯定可以看出来。

```python
prompt = f"""
Translate the following English text to Spanish: \ 
```Hi, I would like to order a blender```
"""
response = get_completion(prompt)
print(response)
"""
Hola, me gustaría ordenar una licuadora.
"""
```

现在让我们试试另一个例子。在这个例子中，提示是告诉我这是哪种语言。然后这段文本是法语，`Combien coûte le lampadaire?`。模型识别出这是法语。

```python
prompt = f"""
Tell me which language this is: 
```Combien coûte le lampadaire?```
"""
response = get_completion(prompt)
print(response)
"""
This is French.
"""
```

模型还可以同时进行多种翻译。在这个例子中，让我们假设将以下文本翻译为法语和西班牙语。而且，让我们再加上一个英语海盗。文本是`I want to order a basketball`。所以在这里，我们有法语、西班牙语和英语海盗。

```python
prompt = f"""
Translate the following  text to French and Spanish
and English pirate: \
```I want to order a basketball```
"""
response = get_completion(prompt)
print(response)
"""
French pirate: ```Je veux commander un ballon de basket```

Spanish pirate: ```Quiero ordenar una pelota de baloncesto```
English pirate: ```I want to order a basketball```
"""
```

在某些语言中，翻译可能会因说话者与听众的关系而改变。您还可以向语言模型解释这一点。因此，它将能够相应地进行翻译。在这个例子中，我们说，将以下文本翻译为西班牙语的正式和非正式形式。`Would you like to order a pillow?` 还要注意，这里我们使用的是。只要有一定的明确分隔就可以了。所以，这里我们有正式和非正式之分。正式场合是指当你和一个比你资历高的人说话或者在一个职业场合时，你需要使用正式语气；而非正式则是当你和朋友群体交流时使用。

我实际上不会说西班牙语，但是我父亲会说，他说这是正确的。

```python
prompt = f"""
Translate the following text to Spanish in both the \
formal and informal forms: 
'Would you like to order a pillow?'
"""
response = get_completion(prompt)
print(response)
"""
Formal: ¿Le gustaría ordenar una almohada?
Informal: ¿Te gustaría ordenar una almohada?
"""
```

### 通用翻译

接下来的例子中，我们假设我们是一个跨国电子商务公司的负责人，用户的消息将使用各种不同的语言，并且用户将使用多种语言向我们反映其 IT 问题，因此我们需要一种通用的翻译器。首先，我们将粘贴一些不同语言的用户消息列表，然后我们将循环遍历每一个用户消息。因此，在用户消息中循环遍历问题，然后我将复制这个稍微长一点的代码块。首先，我们将询问模型告诉我们问题是用什么语言书写的。然后，我们将打印出原始消息的语言和问题，并要求模型将其翻译成英文和韩文。让我们运行它。

```python
user_messages = [
    "La performance du système est plus lente que d'habitude.",  # System performance is slower than normal         
    "Mi monitor tiene píxeles que no se iluminan.",              # My monitor has pixels that are not lighting
    "Il mio mouse non funziona",                                 # My mouse is not working
    "Mój klawisz Ctrl jest zepsuty",                             # My keyboard has a broken control key
    "我的屏幕在闪烁"                                               # My screen is flashing
] 
```

```python
for issue in user_messages:
    prompt = f"Tell me what language this is: ```{issue}```"
    lang = get_completion(prompt)
    print(f"Original message ({lang}): {issue}")

    prompt = f"""
    Translate the following  text to English \
    and Korean: ```{issue}```

    """

    response = get_completion(prompt)

    print(response, "\n")

"""
Original message (This is French.): La performance du système est plus lente que d'habitude.
English: The system performance is slower than usual.
Korean: 시스템 성능이 평소보다 느립니다. 

Original message (This is Spanish.): Mi monitor tiene píxeles que no se iluminan.
English: My monitor has pixels that don't light up.
Korean: 내 모니터에는 불이 켜지지 않는 픽셀이 있습니다. 

Original message (This is Italian.): Il mio mouse non funziona
English: My mouse is not working.
Korean: 내 마우스가 작동하지 않습니다. 

Original message (This is Polish.): Mój klawisz Ctrl jest zepsuty
English: My Ctrl key is broken.
Korean: 제 Ctrl 키가 고장 났어요. 

Original message (This is Chinese (Simplified).): 我的屏幕在闪烁
English: My screen is flickering.
Korean: 내 화면이 깜빡입니다. 
"""
```

## 风格转换

在这里，我们有一个更正式的商业信函，其中包括一份落地灯规格的建议书。接下来我们要做的事情是在不同的格式之间进行转换。

```python
prompt = f"""
Translate the following from slang to a business letter: 
'Dude, This is Joe, check out this spec on this standing lamp.'
"""
response = get_completion(prompt)
print(response)
"""
Dear Sir/Madam,

I am writing to bring to your attention a standing lamp that I believe may be of interest to you. Please find attached the specifications for your review.

Thank you for your time and consideration.

Sincerely,

Joe
"""
```

ChatGPT非常擅长在不同格式之间进行翻译，例如`JSON`到`HTML`、`XML`等各种格式。在提示中，我们将描述输入和输出格式。以下是一个示例：我们有一个包含餐厅员工姓名和电子邮件的`JSON`列表。在提示中，我们将要求模型将其从`JSON`转换为`HTML`表格。然后我们将得到模型的响应并打印出来，以便查看经过格式化后的`HTML`表格。

```python
data_json = { "resturant employees" :[ 
    {"name":"Shyam", "email":"shyamjaiswal@gmail.com"},
    {"name":"Bob", "email":"bob32@gmail.com"},
    {"name":"Jai", "email":"jai87@gmail.com"}
]}

prompt = f"""
Translate the following python dictionary from JSON to an HTML \
table with column headers and title: {data_json}
"""
response = get_completion(prompt)
print(response)
"""
<table>
  <caption>Restaurant Employees</caption>
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Shyam</td>
      <td>shyamjaiswal@gmail.com</td>
    </tr>
    <tr>
      <td>Bob</td>
      <td>bob32@gmail.com</td>
    </tr>
    <tr>
      <td>Jai</td>
      <td>jai87@gmail.com</td>
    </tr>
  </tbody>
</table>
"""
```

![](img/ef7da5585b8ef1687163f4049ea461b4.png)
