
# 代码示例

## 函数定义

```
def run_conv(question, 
             client,
             tools=None, 
             functions_list=None, 
             model="claude-3-5-sonnet-20240620"):
    """
    能够自动执行外部函数调用的Chat对话模型，并支持多轮对话
    :return：Chat模型输出结果
    """
    conversation = []

    def chat_with_claude(conversation, user_messages):
        conversation.extend(user_messages)
        response = client.messages.create(
            model=model,
            max_tokens=1024,
            messages=conversation,
            tools=tools,
        )
        return response

    while True:
        user_input = input("你: ")
        if user_input.lower() == "exit":
            print("结束对话。")
            break

        user_messages = []

        image_url = extract_image_url(user_input)
        if image_url:
            image_content = get_image_data(image_url)
            text_content = user_input.replace(image_url, "").strip()
            if not text_content:
                text_content = "请帮我描绘下图片上的内容。"
            user_messages.append({
                "role": "user",
                "content": [image_content, {"type": "text", "text": text_content}]
            })
        else:
            user_messages.append({
                "role": "user",
                "content": [
                    {
                        "type": "text",
                        "text": user_input
                    }
                ]
            })

        response = chat_with_claude(conversation, user_messages)
        response_message = response.content

        if response.stop_reason == 'tool_use' and functions_list is not None:
            available_functions = {func.__name__: func for func in functions_list}
            function_name = response_message[1].name
            function_to_call = available_functions.get(function_name)
            function_args = response_message[1].input

            if function_to_call:
                function_response = function_to_call(**function_args)
                conversation.append({
                    "role": "assistant",
                    "content": [
                        {
                            "type": "text",
                            "text": response_message[0].text
                        },
                        {
                            "type": "tool_use",
                            "id": response_message[1].id,
                            "name": function_name,
                            "input": function_args
                        }
                    ]
                })
                conversation.append({
                    "role": "user",
                    "content": [
                        {
                            "type": "tool_result",
                            "tool_use_id": response_message[1].id,
                            "content": function_response
                        }
                    ]
                })
                second_response = chat_with_claude(conversation, [])
                final_response = second_response.content[0].text
            else:
                final_response = "Function not found."
        else:
            conversation.append({
                "role": "assistant",
                "content": response_message
            })
            final_response = response_message[0].text

        print(f"Claude: {final_response}")
```

## 函数调用

```
run_conv(question='你好', 
         client=client,
         tools=tools, 
         functions_list=functions_list)
```

# 对话演示

## 图片识别对话

```
你:  好的，这张图上最佳性能的模型是哪个呢？图片地址：https://ml2022.oss-cn-hangzhou.aliyuncs.com/img/202406211740876.png
```

## 自我介绍与模型性能对话

```
你:  你好，请介绍下你自己

Claude: 您好!很高兴与您交流。我是一个AI助手,由Anthropic公司开发。我的目标是通过友好和有帮助的对话来协助人类。我可以回答问题、提供信息、帮助解决问题,以及就各种话题进行讨论。

我拥有广泛的知识,涵盖科学、历史、文化、技术等多个领域。但我并不是全知全能的,有些事情我也不确定或不了解。在这种情况下,我会坦诚地告诉您我的局限性。

作为一个AI,我没有人类那样的情感或个人经历。我的回答是基于我被训练的数据和算法生成的。我会尽力提供准确、有用的信息,但有时可能会出错,所以请您保持批判性思维。

我很乐意为您提供帮助,无论是回答问题、提供建议,还是简单地聊天。您有什么我可以帮到您的吗?
Claude: 根据这张AI模型发布和能力时间线图，截至图表显示的最新时间点（2024年7月左右），性能最佳的模型是Claude 3.5 Sonnet。

从图表中可以看出：

1. Claude（红色线）在最后时间点达到了最高的基准分数，大约83-84分左右。

2. GPT系列（灰色线）的最新版本GPT-4o略低于Claude 3.5 Sonnet，但也达到了很高的分数，约82分左右。

3. Gemini系列（蓝色线）的最新版本Gemini 1.5 Pro²也达到了很高的分数，但略低于Claude和GPT的最新版本，大约在80分左右。

需要注意的是，这个图表展示的是模型发布的时间线和它们在某些基准测试上的平均得分。虽然Claude 3.5 Sonnet在这个特定的评估中得分最高，但在实际应用中，不同模型可能在不同任务上有各自的优势。此外，AI技术发展迅速，这个排名可能会随着新模型的发布而改变。

你:  你知道Claude的母公司是哪个么？

Claude: 是的,我知道。Claude的母公司是Anthropic。

Anthropic是一家美国人工智能公司,成立于2021年。它专注于开发安全和道德的AI系统,其中最著名的产品就是Claude AI助手。

Anthropic的创始团队包括一些前OpenAI的研究人员,他们致力于发展"宪法AI"(Constitutional AI)的理念,旨在创建更安全、更可控、更符合道德标准的AI系统。

作为Anthropic开发的AI助手,我对公司的具体运营细节并不了解,但我知道Anthropic一直在努力推进AI技术的发展,同时也非常重视AI的安全性和道德性。

你:  这家公司所在地今天天气如何呢？

Claude: 根据查询结果,旧金山(San Francisco)今天的天气情况如下:

1. 温度: 目前温度为10.49°C (约50.88°F)
2. 天气状况: 阴天,多云
3. 湿度: 91%
4. 风速: 4.63 m/s (约10.36 mph)
5. 能见度: 10000米,表示能见度良好

总的来说,今天旧金山的天气比较凉爽,天空多云。虽然温度不算很低,但由于湿度较高,可能会感觉略微潮湿。天气整体来说适合外出活动,但可能需要准备一件外套,以防感到凉意。

需要注意的是,旧金山的天气可能会在一天内有所变化,特别是考虑到它靠近海岸的地理位置。如果有人计划在旧金山外出,最好随时关注最新的天气预报。

你:  exit

结束对话。
```

# 课程推广

## 体验课介绍

+ 体验课内容节选自《2024大模型技术实战》（夏季班）完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名有我主讲的《2024大模型技术实战课》(夏季班)：

![](img/ac8d8533b939062c97f71547eae19aa6.png)

[**《大模型技术实战课》（夏季班）**](https://whakv.xetslk.com/s/4eSM8L)为【100+小时】体系大课，总共十八大模块精讲精析，零基础直达大模型企业级应用！

![](img/6dde537a8959781d3ee897b1be0ee68c.png)

此外为持续保证学员大模型技术竞争力，课程实时追更最新大模型技术进展，近期额外新增了GPT-4o、Qwen2系列模型和GLM-4-9B，以及RAG企业级项目实战最新前沿技术内容！课程大纲获取、领取体验课学员专享优惠券，扫码添加客服小可爱(🛰️：littlecat_1207)，回复“大模型”，即咨询课程信息哦👇

![](img/ccafe43a0d219247bde15998ef2ccdc3.png)

---

![](img/da3d641cceaf5467747e149ab3110820.png)
