
## 若有剩余，则追加到最后

```python
if cur_section != "":
    sections.append(cur_section)
    overlaps.append(0)

return sections, overlaps
```

## 以下为与GPT交互的过程

````python
def section_analysis(analysis_session: session, index: int):
    req = request()
    req.total_num = len(analysis_session.sections)
    req.index = index
    req.overlaps = analysis_session.overlaps[index]
    req.section = analysis_session.sections[index]

    # 若不是总结第一段，则将GPT针对上一段返回的上下文内容进行赋值
    if index > 0:
        req.pre_summary_context = analysis_session.summary_contexts[-1]

    # 请求GPT，并解析返回数据
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo-16k-0613",
        messages=[
            {"role": "system", "content": prompt},
            {"role": "user", "content": "/总结 " + json.dumps(req)}
        ],
    )

    resp = response()
    json.loads(response.choices[0].message.content, resp)

    # 将结果追加到session中
    analysis_session.summary_contexts.append(resp.summary_context)
    analysis_session.summary_result.append(resp.section_summary)

def final_section_analysis(section: str):
    messages = [
        {"role": "user", "content": "总结段落文字的要点，要有一定的核心细节，段落文字为：```" + section + "```"}
    ]
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo-16k-0613",
        messages=messages,
    )

    return response.choices[0].message.content
````
