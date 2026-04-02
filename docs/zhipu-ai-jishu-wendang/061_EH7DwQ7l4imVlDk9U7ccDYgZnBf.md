# 接入OpenAI对齐接口

> 来源：[https://ai.feishu.cn/docx/GUgKdYDpooLiepx5BMBczqdLn2f](https://ai.feishu.cn/docx/GUgKdYDpooLiepx5BMBczqdLn2f)

本章节将带领开发者学习如何使用OpenAI接口来实现与cogvlm-chat和cogagent-chat的对话。

请配合以下代码库中微调部分代码进行上手练习

https://github.com/THUDM/CogVLM/tree/main/openai_demo

我们书写了一套基于 OpenAI GPT-4V的接口用于访问我们的cogvlm-chat和cogagent-chat模型。

## 快速运行

首先，我们运行代码

```
python openai_api.py
```

这样，我们就启动了一个简单的服务端。

接着，我们将已经准备好的图片放置在 openai_demo文件夹下并尝试请求

```
python openai_api_request.py
```

我们将会观察到服务端收到了来自用户端的请求，并已经按照模型的要求转换成了query，history和image_list,模型根据输入，返回输出

```
==== request ====
Do you think this is a spring or winter photo?
==== message ====
role=&#x27;assistant&#x27; content=&#x27;This appears to be a spring or summer photo. The lush green grass and vegetation, along with the bright and clear sky, are indicative of warmer months when plants are in full bloom and the weather is pleasant.&#x27; name=None
```

客户端将会收到模型返回的结果。

## 代码细节

1.  图像是怎么上传的

图像通过以下流程传入模型

+   首先，读取用户图像之后，返回图像的base64信息，代码如下：

```
def encode_image(image_path):
    &quot;&quot;&quot;Getting the base64 string&quot;&quot;&quot;
    with open(image_path, &quot;rb&quot;) as image_file:
        return base64.b64encode(image_file.read()).decode(&quot;utf-8&quot;)
```

+   接着，带有文字和base64信息的图像将被传入服务端，随后，将根据以下方式与文字被拆解成模型对齐中适合的历史和图像队列。

```
def process_history_and_images(messages: List[ChatMessageInput]) -&gt; Tuple[
    Optional[str], Optional[List[Tuple[str, str]]], Optional[List[Image.Image]]]:
    &quot;&quot;&quot;
    Process history messages to extract text, identify the last user query,
    and convert base64 encoded image URLs to PIL images.

    :param messages: List of ChatMessageInput objects.
    :return: A tuple of three elements:
             - The last user query as a string.
             - Text history formatted as a list of tuples for the model.
             - List of PIL Image objects extracted from the messages.
    &quot;&quot;&quot;
    formatted_history = []
    image_list = []
    last_user_query = &#x27;&#x27;

    for i, message in enumerate(messages):
        role = message.role
        content = message.content

        if isinstance(content, list):  # text
            text_content = &#x27; &#x27;.join(item.text for item in content if isinstance(item, TextContent))
        else:
            text_content = content
        
        if isinstance(content, list):  # image
            for item in content:
                if isinstance(item, ImageUrlContent):
                    image_url = item.image_url.url
                    if image_url.startswith(&quot;data:image/jpeg;base64,&quot;):
                        base64_encoded_image = image_url.split(&quot;data:image/jpeg;base64,&quot;)[1]
                        image_data = base64.b64decode(base64_encoded_image)
                        image = Image.open(BytesIO(image_data)).convert(&#x27;RGB&#x27;)
                        image_list.append(image)
        
        if role == &#x27;user&#x27;:
            if i == len(messages) - 1
                last_user_query = text_content
            else:
                formatted_history.append((text_content, &#x27;&#x27;))
        elif role == &#x27;assistant&#x27;:
            if formatted_history:
                if formatted_history[-1][1] != &#x27;&#x27;:
                    assert False, f&quot;the last query is answered. answer again. {formatted_history[-1][0]}, {formatted_history[-1][1]}, {text_content}&quot;
                formatted_history[-1] = (formatted_history[-1][0], text_content)
            else:
                assert False, f&quot;assistant reply before user&quot;
        else:
            assert False, f&quot;unrecognized role: {role}&quot;

    return last_user_query, formatted_history, image_list
```

在这份代码中，我们先将传入的信息转换为列表，并使用PIL库将图像的base64信息转换为Image类型。并按照传历史的顺序，将对话和图像信息排序成队列。

> 注意：

> 由于模型目前的对齐方式，Image和对话并不能按照顺序的方式排列，图像永远放在历史的最前端，因此当出现以下情况到，模型会出现无法理解上下文的情况

>

> 在同一个对话中：

1.  用户传入图片1，接着提出关于图片1的问题

1.  用户传入图片2，接着提出关于图片2的问题

>

> 此时，由于模型中的顺序为：

> 图片2，问题1，问题2

> 因此，问题2会自动连接问题1（这也是不符合预期的）

>

1.  接着，用户提出关于图片1 的问题。

> 由于图片1 已经被覆盖，用户提出的关于图片1 的问题模型是无法回答的。

因此，现在的模型能力，仅仅能对单张图片，并且所有的历史对话均关于这张图片。类似如下

1.  用户传入图片1，提出关于图片1的第一个问题，第二个问题 ...

1.  开启新的对话，提出关于图片2，提出关于图片2的第一个问题，第二个问题 ...

1.  ...

## 注意事项

到此为止，你已经知道 OpenAI接口 与cogvlm-chat & cogagent-chat 之间的转换。以下几点你需要注意：

+   cogvlm-chat, cogagent-chat模型暂时仅支持单词对话中传入至多1张图片，因此，如果在后续对话中传入新的图片，则会覆盖前面的图片，但是文字的历史并不会被覆盖。

+   cogvlm-chat,cogagent-chat模型中剩余参数与OpnAI 接口完全相同，但我们不推荐填入惩罚系数。