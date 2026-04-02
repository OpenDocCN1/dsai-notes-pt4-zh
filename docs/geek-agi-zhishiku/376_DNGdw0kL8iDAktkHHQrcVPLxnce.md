# 浅尝prompt咒语设计：one-shot微调chatglm-6b实践信息抽取

> 来源：[https://geekbang.feishu.cn/docx/B0ssdIScrose5uxGzIzcGqCjnvo](https://geekbang.feishu.cn/docx/B0ssdIScrose5uxGzIzcGqCjnvo)

> 👨🏻 作者：余俊晖

> 📃 作者介绍：一名NLP炼丹师，NLP算法竞赛TOP选手

> 🛖 公众号：大模型自然语言处理

> ⏰ 收录时间：2024-05-28 17:09:42

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=Mzg4NjI0NDg0Ng==&mid=2247484023&idx=1&sn=7dbbf3c41e78ec000f20a964f494a98c&chksm=cf9dd6f6f8ea5fe07fcc6844d66003ebe74f802b8aa287352387b080f26c7c75d35bb805ea89#rd)q.com/s?__biz=Mz...

![](img/68d4e37bacc5f260bd4c9988219e75a4.png)

关注作者公众号，获取更多AI知识

## ** ****前言 **

近期以chatgpt等文生成LLMS爆火，国内也逐渐开源了中文版的chatgpt，本文以清华大学开源的6b的chatglm为例，实践one-shot微调，设计prompt咒语在信息抽取领域的实验效果。

## ** ****1、场景描述 **

给定一个 **JD的职位要求 **，需要从描述中抽取出相应的 **实体 **。

例如：

```
&#x27;职位要求：1、硕士以上学历。2、计算机相关专业。3、3年以上工作经验。4、熟练掌握python或者c++语言。5、有自然语言处理获奖经历优先&#x27;
```

相应的schema的实体为：

```
&#x27;学历要求&#x27;: [&#x27;硕士&#x27;],
&#x27;专业要求&#x27;: [&#x27;计算机&#x27;],
&#x27;工作年限要求&#x27;: [&#x27;3年以上&#x27;],
&#x27;编程语言&#x27;: [&#x27;python&#x27;, &#x27;c++&#x27;],
&#x27;加分项&#x27;: [&#x27;自然语言处理获奖经历&#x27;],
```

## ** ****2、prompt咒语设计 **

prompt设计主要点：

+   告知llms什么是信息抽取任务，以职位要求信息抽取为例：需要告知模型你需要抽取的实体类型有哪些

+   告知模型输出格式要求，例如：json格式

## ** ****3、实现 **

```
#!/usr/bin/env python
# _*_coding:utf-8_*_
# Author   :    Junhui Yu

import re
import json
from transformers import AutoTokenizer, AutoModel

# 根据需求补充
schema = {
    &#x27;JD岗位要求&#x27;: [&#x27;学历要求&#x27;, &#x27;专业要求&#x27;, &#x27;工作年限要求&#x27;, &#x27;编程语言&#x27;, &#x27;加分项&#x27;]
}

IE_PATTERN = &quot;{}\n\n提取上述句子中{}类型的实体，输出JSON格式，上述句子中不存在的信息用[&#x27;该JD未要求&#x27;]来表示，多个值之间用&#x27;,&#x27;分隔。&quot;

ie_examples = {
    &#x27;JD岗位要求&#x27;:
        {
            &#x27;sentence&#x27;: &#x27;职位要求：1、硕士以上学历。2、计算机相关专业。3、3年以上工作经验。4、熟练掌握python或者c++语言。5、有自然语言处理获奖经历优先&#x27;,
            &#x27;answers&#x27;: {
                &#x27;学历要求&#x27;: [&#x27;硕士&#x27;],
                &#x27;专业要求&#x27;: [&#x27;计算机&#x27;],
                &#x27;工作年限要求&#x27;: [&#x27;3年以上&#x27;],
                &#x27;编程语言&#x27;: [&#x27;python&#x27;, &#x27;c++&#x27;],
                &#x27;加分项&#x27;: [&#x27;自然语言处理获奖经历&#x27;],
            }
        }
}


def init_prompts():
    ie_prefix = [
        (
            &quot;需要你协助完成信息抽取任务，当我给你一个JD职位要求时，帮我抽取出句子中三元组，并按照JSON的格式输出，上述句子中没有的信息用[&#x27;该JD未要求&#x27;]来表示，多个值之间用&#x27;,&#x27;分隔。&quot;,
            &#x27;请输入JD职位描述。&#x27;
        )
    ]
    properties_str = &#x27;, &#x27;.join(schema[&#x27;JD岗位要求&#x27;])
    schema_str_list = f&#x27;“JD岗位要求”({properties_str})&#x27;
    sentence = ie_examples[&#x27;JD岗位要求&#x27;][&#x27;sentence&#x27;]
    sentence_with_prompt = IE_PATTERN.format(sentence, schema_str_list)
    ie_prefix.append((
        f&#x27;{sentence_with_prompt}&#x27;,
        f&quot;{json.dumps(ie_examples[&#x27;JD岗位要求&#x27;][&#x27;answers&#x27;], ensure_ascii=False)}&quot;
    ))

    return {&#x27;ie_prefix&#x27;: ie_prefix}


def format_output(response: str):
    if &#x27;```json&#x27; in response:
        res = re.findall(r&#x27;```json(.*?)```&#x27;, response)
        if len(res) and res[0]:
            response = res[0]
        response.replace(&#x27;、&#x27;, &#x27;,&#x27;)
    try:
        return json.loads(response)
    except:
        return response


def inference(sentences: list, custom_settings: dict):
    for sentence in sentences:
        properties_str = &#x27;, &#x27;.join(schema[&#x27;JD岗位要求&#x27;])
        schema_str_list = f&#x27;“JD岗位要求”({properties_str})&#x27;
        sentence_with_ie_prompt = IE_PATTERN.format(sentence, schema_str_list)
        result, _ = model.chat(tokenizer, sentence_with_ie_prompt, history=custom_settings[&#x27;ie_prefix&#x27;])
        result = format_output(result)
        print(sentence)
        print(result)


if __name__ == &#x27;__main__&#x27;:
    tokenizer = AutoTokenizer.from_pretrained(&quot;THUDM/chatglm-6b&quot;, trust_remote_code=True)
    model = AutoModel.from_pretrained(&quot;THUDM/chatglm-6b&quot;, trust_remote_code=True).half().cuda()

    sentences = [
        &#x27;职位要求：1、本科以上学历。2、电子信息或软件工程专业。3、1-3年工作经验。4、熟练掌握java或者c++语言。5、有相关项目经验优先&#x27;,
    ]

    custom_settings = init_prompts()
    inference(sentences,
        custom_settings
    )

```

输出结果：

```
{
    &#x27;学历要求&#x27;: [&#x27;本科以上学历&#x27;],
    &#x27;专业要求&#x27;: [&#x27;电子信息或软件工程专业&#x27;],
    &#x27;工作年限要求&#x27;: [&#x27;1-3年&#x27;],
    &#x27;编程语言&#x27;: [&#x27;熟练掌握java或者c++语言&#x27;],
    &#x27;加分项&#x27;: [&#x27;有相关项目经验优先&#x27;]
}
```

## ** ****总结 **

本文通过one-shot微调chatglm-6b在信息抽取领域上的实验，输出效果还可以，当然如果有资源微调更大参数量的LLMS。

## ** ****往期相关 **

【炼丹回忆】2021全球开放数据应用创新大赛-法律咨询问答亚军方案

LLaMA：7B参数量的Baby版ChatGPT窥探

BigBird：大鸟模型中文生成式长文本摘要实践