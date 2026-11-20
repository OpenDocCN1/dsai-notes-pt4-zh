
# 拼写语法检查

接下来的转换任务是拼写检查和语法检查。这是ChatGPT的一个非常受欢迎的应用。我们建议您经常使用它，特别是在使用非母语语言时非常有用。以下是一些常见的语法和拼写问题以及如何使用语言模型来解决它们的示例。我们将列举一些句子，这些句子中存在某些语法或拼写错误。然后我们将循环遍历每个句子，请求模型进行校对和修正。然后我们会得到响应并像往常一样将其打印出来。模型能够纠正所有这些语法错误。我们可以使用之前讨论过的一些技巧来改进提示。为了改进提示，我们可以说“校对并更正以下文本。如果你没有找到任何错误，请说没有错误。”通过迭代的提示开发，您可以找到一个更可靠的提示。

````text
text = [ 

  "The girl with the black and white puppies have a ball.",  # The girl has a ball.

  "Yolanda has her notebook.", # ok

  "Its going to be a long day. Does the car need it’s oil changed?",  # Homonyms

  "Their goes my freedom. There going to bring they’re suitcases.",  # Homonyms

  "Your going to need you’re notebook.",  # Homonyms

  "That medicine effects my ability to sleep. Have you heard of the butterfly affect?", # Homonyms

  "This phrase is to cherck chatGPT for speling abilitty"  # spelling

]

for t in text:

    prompt = f"""Proofread and correct the following text

    and rewrite the corrected version. If you don't find

    and errors, just say "No errors found". Don't use 

    any punctuation around the text:

    ```{t}```"""
    response = get_completion(prompt)
    print(response)
    
 """
 The girl with the black and white puppies has a ball.
No errors found.
It's going to be a long day. Does the car need its oil changed?
Their goes my freedom. There going to bring they're suitcases.

Corrected version: 
There goes my freedom. They're going to bring their suitcases.
You're going to need your notebook.
That medicine affects my ability to sleep. Have you heard of the butterfly effect?
This phrase is to check ChatGPT for spelling ability.
 """
````

另一个例子是检查您在公共论坛上发布文本之前进行拼写和语法检查的示例。我们将使用一份有关填充熊猫的评论的示例，然后要求模型对评论进行校对和更正。最后，我们将得到更正后的版本。

````text
text = f"""
Got this for my daughter for her birthday cuz she keeps taking \
mine from my room.  Yes, adults also like pandas too.  She takes \
it everywhere with her, and it's super soft and cute.  One of the \
ears is a bit lower than the other, and I don't think that was \
designed to be asymmetrical. It's a bit small for what I paid for it \
though. I think there might be other options that are bigger for \
the same price.  It arrived a day earlier than expected, so I got \
to play with it myself before I gave it to my daughter.
"""
prompt = f"proofread and correct this review: ```{text}```"

response = get_completion(prompt)

print(response)

"""

I got this for my daughter's birthday because she keeps taking mine from my room. Yes, adults also like pandas too. She takes it everywhere with her, and it's super soft and cute. However, one of the ears is a bit lower than the other, and I don't think that was designed to be asymmetrical. Additionally, it's a bit small for what I paid for it. I think there might be other options that are bigger for the same price. On the positive side, it arrived a day earlier than expected, so I got to play with it myself before I gave it to my daughter.

"""
````

我们可以做的一件很酷的事情是找到原始评论和模型输出之间的差异。因此，我们将使用这个`RedLines` Python包来做这件事情。我们将获得我们评论的原始文本和模型输出之间的差异，然后将其显示出来。在这里，您可以看到原始评论和模型输出之间的差异以及已经更正的内容。

```text
from redlines import Redlines

diff = Redlines(text,response)

display(Markdown(diff.output_markdown))
```

![](img/d8b589cee67d7208240ac45f13ace7bd.png)

因此，我们使用的提示是“校对并更正此评论”，但您还可以进行更明显的更改，例如改变语气等。让我们再试试其他东西。在这个提示中，我们将要求模型校对和更正同一篇评论，同时使其更有吸引力，确保它遵循APA风格，并面向高级读者。我们还将要求以markdown格式输出。因此，我们在上面使用的是来自原始评论的相同文本。让我们执行一下。在这里，我们有一篇扩展的APA风格评论《软性熊猫》。

````text
prompt = f"""

proofread and correct this review. Make it more compelling. 

Ensure it follows APA style guide and targets an advanced reader. 

Output in markdown format.

Text: ```{text}```
"""
response = get_completion(prompt)
display(Markdown(response))

"""
Title: A Soft and Cute Panda Plushie for All Ages
As an adult, I can attest that pandas are not just for kids. That's why I got this adorable panda plushie for my daughter's birthday, after she kept taking mine from my room. And let me tell you, it was a hit!
The plushie is super soft and cuddly, making it the perfect companion for my daughter. She takes it everywhere with her, and it has quickly become her favorite toy. However, I did notice that one of the ears is a bit lower than the other, which I don't think was designed to be asymmetrical. But that doesn't take away from its cuteness.
The only downside is that it's a bit small for the price I paid. I think there might be other options that are bigger for the same price. But overall, I'm happy with my purchase.
One thing that surprised me was that it arrived a day earlier than expected. This gave me the chance to play with it myself before giving it to my daughter. And I have to say, I was impressed with the quality and attention to detail.
In conclusion, if you're looking for a soft and cute panda plushie for yourself or a loved one, this is definitely a great option. Just be aware that it might be a bit smaller than expected.
"""
````

# 文本扩写 / Expanding

[prompt_eng_07_expanding.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FW1HbARtWoBd9OxziafcQ6dKn4g)

扩展是将短的文本片段，比如一组说明或一系列主题，并让大型语言模型生成更长的文本，比如关于某个主题的电子邮件或论文的任务。这样做很有用，比如如果你把大型语言模型作为头脑风暴的合作伙伴。但我也想指出，这样做有一些问题，比如如果有人用它生成大量的垃圾邮件。因此，当你使用大型语言模型的这些功能时，请负责任地使用，并以有助于人们的方式使用。在这个视频中，我们将演示如何使用语言模型根据某些信息生成个性化的电子邮件。这封电子邮件有点自称为来自 AI 机器人，这一点非常重要，如安德鲁所提到的。我们还将使用另一个模型输入参数，称为`temperature`（温度），这使您能够变化模型响应中的探索和多样性程度。让我们开始吧。

## 环境准备

所以在我们开始之前，我们要做一些通常的设置。所以设置OpenAI Python包，然后定义我们的帮助函数`get_completion`。

```text
import openai
import os

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file

openai.api_key  = os.getenv('OPENAI_API_KEY')
```

```text
def get_completion(prompt, model="gpt-3.5-turbo",temperature=0): # Andrew mentioned that the prompt/ completion paradigm is preferable for this class
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=temperature, # this is the degree of randomness of the model's output
    )
    return response.choices[0].message["content"]
```

## 自动回复邮件

现在我们要为客户评论编写一个自定义的电子邮件回复，因此给定客户评论和情绪，我们将生成一个自定义的回复。现在我们将使用语言模型根据客户评论和评论的情绪为客户生成一个自定义的电子邮件。所以我们已经使用我们在推断视频中看到的那种提示提取了情绪，然后这是搅拌器的客户评论，现在我们将根据情绪定制回复。
