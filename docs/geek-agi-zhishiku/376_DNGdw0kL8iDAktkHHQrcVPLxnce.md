
# 浅尝prompt咒语设计：one-shot微调chatglm-6b实践信息抽取

> 来源：https://geekbang.feishu.cn/docx/B0ssdIScrose5uxGzIzcGqCjnvo
> 👨🏻 作者：余俊晖
> 📃 作者介绍：一名NLP炼丹师，NLP算法竞赛TOP选手
> 🛖 👸号：大模型自然语言处理
> ⏰ 收录时间：2024-05-28 17:09:42
> 🔗 原文链接：https://mp.weixin.qq.com/s?__biz=Mzg4NjI0NDg0Ng==&mid=2247484023&idx=1&sn=7dbbf3c41e78ec000f20a964f494a98c&chksm=cf9dd6f6f8ea5fe07fcc6844d66003ebe74f802b8aa287352387b080f26c7c75d35bb805ea89#rd

![](img/68d4e37bacc5f260bd4c9988219e75a4.png)

关注作者👸号，获取更多AI知识

## 前言

近期以chatgpt等文生成LLMS爆火，国内也逐渐开源了中文版的chatgpt，本文以清华大学开源的6b的chatglm为例，实践one-shot微调，设计prompt咒语在信息抽取领域的实验效果。

## 1、场景描述

给定一个JD的职位要求，需要从描述中抽取出相应的实体。

例如：

```json
'职位要求：1、硕士以上学历。2、计算机相关专业。3、3年以上工作经验。4、熟练掌握python或者c++语言。5、有自然语言处理获奖经历优先'
```

相应的`schema`的实体为：

```json
'学历要求': ['硕士'],
'专业要求': ['计算机'],
'工作年限要求': ['3年以上'],
'编程语言': ['python', 'c++'],
'加分项': ['自然语言处理获奖经历'],
```

## 2、prompt咒语设计

prompt设计主要点：

+ 告知llms什么是信息抽取任务，以职位要求信息抽取为例：需要告知模型你需要抽取的实体类型有哪些

+ 告知模型输出格式要求，例如：json格式

## 3、实现

```python
#!/usr/bin/env python
# _*_coding:utf-8_*_
# Author   :    Junhui Yu

import re
import json
from transformers import AutoTokenizer, AutoModel

# 根据需求补充
schema = {
    'JD岗位要求': ['学历要求', '专业要求', '工作年限要求', '编程语言', '加分项']
}

IE_PATTERN = "{}\n\n提取上述句子中{}类型的实体，输出JSON格式，上述句子中不存在的信息用['该JD未要求']来表示，多个值之间用','分隔。"

ie_examples = {
    'JD岗位要求':
        {
            'sentence': '职位要求：1、硕士以上学历。2、计算机相关专业。3、3年以上工作经验。4、熟练掌握python或者c++语言。5、有自然语言处理获奖经历优先',
            'answers': {
                '学历要求': ['硕士'],
                '专业要求': ['计算机'],
                '工作年限要求': ['3年以上'],
                '编程语言': ['python', 'c++'],
                '加分项': ['自然语言处理获奖经历'],
            }
        }
}


def init_prompts():
    ie_prefix = [
        (
            "需要你协助完成信息抽取任务，当我给你一个JD职位要求时，帮我抽取出句子中三元组，并按照JSON的格式输出，上述句子中没有的信息用['该JD未要求']来表示，多个值之间用','分隔。",
            '请输入JD职位描述。'
        )
    ]
    properties_str = ', '.join(schema['JD岗位要求'])
    schema_str_list = f'“JD岗位要求”({properties_str})'
    sentence = ie_examples['JD岗位要求']['sentence']
    sentence_with_prompt = IE_PATTERN.format(sentence, schema_str_list)
    ie_prefix.append((
        f'{sentence_with_prompt}',
        f"{json.dumps(ie_examples['JD岗位要求']['answers'], ensure_ascii=False)}"
    ))

    return {'ie_prefix': ie_prefix}


def format_output(response: str):
    if '```json' in response:

        res = re.findall(r'```json(.*?)```', response)
        if len(res) and res[0]:
            response = res[0]
        response.replace('、', ',')
    try:
        return json.loads(response)
    except:
        return response


def inference(sentences: list, custom_settings: dict):
    for sentence in sentences:
        properties_str = ', '.join(schema['JD岗位要求'])
        schema_str_list = f'“JD岗位要求”({properties_str})'
        sentence_with_ie_prompt = IE_PATTERN.format(sentence, schema_str_list)
        result, _ = model.chat(tokenizer, sentence_with_ie_prompt, history=custom_settings['ie_prefix'])
        result = format_output(result)
        print(sentence)
        print(result)


if __name__ == '__main__':
    tokenizer = AutoTokenizer.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True)
    model = AutoModel.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True).half().cuda()

    sentences = [
        '职位要求：1、本科以上学历。2、电子信息或软件工程专业。3、1-3年工作经验。4、熟练掌握java或者c++语言。5、有相关项目经验优先',
    ]

    custom_settings = init_prompts()
    inference(sentences,
        custom_settings
    )
```

输出结果：

```json
{
    '学历要求': ['本科以上学历'],
    '专业要求': ['电子信息或软件工程专业'],
    '工作年限要求': ['1-3年'],
    '编程语言': ['熟练掌握java或者c++语言'],
    '加分项': ['有相关项目经验优先']
}
```

## 总结

本文通过one-shot微调chatglm-6b在信息抽取领域上的实验，输出效果还可以，当然如果有资源微调更大参数量的LLMS。

## 往期相关

【炼丹回忆】2021全球开放数据应用创新大赛-法律咨询问答亚军方案

LLaMA：7B参数量的Baby版ChatGPT窥探

BigBird：大鸟模型中文生成式长文本摘要实践
