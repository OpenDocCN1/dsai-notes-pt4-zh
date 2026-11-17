
# `get_answer` 函数

## 函数定义

```
def get_answer(q, g='globals()'):
    """
    当你无法回答某个问题时，调用该函数，能够获得答案

    :param q: 必选参数，询问的问题，字符串类型对象

    :param g: g，字符串形式变量，表示环境变量，无需设置，保持默认参数即可

    :return：某问题的答案，以字符串形式呈现
    """

    # 默认搜索返回5个答案
    print('正在接入谷歌搜索，查找和问题相关的答案...')

    results = google_search(query=q, num_results=5, site_url='https://zhihu.com/')

    # 创建对应问题的子文件夹
    folder_path = './auto_search/%s' % q

    if not os.path.exists(folder_path):
        os.makedirs(folder_path)

    # 单独提取links放在一个list中
    num_tokens = 0
    content = ''

    for item in results:
        url = item['link']
        print('正在检索：%s' % url)
        title = get_search_text(q, url)
        with open('./auto_search/%s/%s.json' % (q, title), 'r') as f:
            jd = json.load(f)
        num_tokens += jd[0]['tokens']
        if num_tokens <= 12000:
            # print(jd[0]['content'])
            content += jd[0]['content']
        else:
            break

    return(content)
```

## 参数说明

- `q`：必选参数，询问的问题，字符串类型对象。
- `g`：字符串形式变量，表示环境变量，无需设置，保持默认参数即可，默认值为 `globals()`。

## 返回值

返回某问题的答案，以字符串形式呈现。

# `get_answer_tool` 工具定义

```
get_answer_tool = {
    "type": "function",
    "function": {
        "name": "get_answer",
        "description": (
            "联网搜索工具，当用户提出的问题超出你的知识库范畴时，或该问题你不知道答案的时候，请调用该函数来获得问题的答案。该函数会自动从知乎上搜索得到问题相关文本，而后你可围绕文本内容进行总结，并回答用户提问。需要注意的是，当用户点名要求想要了解GitHub上的项目时候，请调用get_answer_github函数。"
        ),
        "parameters": {
            "type": "object",
            "properties": {
                "q": {
                    "type": "string",
                    "description": "一个满足知乎搜索格式的问题，用字符串形式进行表示。",
                    "example": "什么是MCP?"
                },
                "g": {
                    "type": "string",
                    "description": "Global environment variables, default to globals().",
                    "default": "globals()"
                }
            },
            "required": ["q"]
        }
    }
}
```

# 调用示例

```
get_answer(q="什么是MCP？", g=globals())
```

# 执行输出

正在接入谷歌搜索，查找和问题相关的答案...

正在检索：`https://zhuanlan.zhihu.com/p/27327515233`

正在检索：`https://zhuanlan.zhihu.com/p/10122577918`

正在检索：`https://www.zhihu.com/question/7762420288`

正在检索：`https://zhuanlan.zhihu.com/p/26834797144`

正在检索：`https://www.zhihu.com/question/5290049088`
