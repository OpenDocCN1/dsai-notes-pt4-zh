
# 联网搜索外部函数和 GitHub 搜索外部函数

综上，联网搜索外部函数和 GitHub 搜索外部函数如下：

## 联网搜索外部函数

```python
@function_tool
def get_answer(q):
    """
    联网搜索函数，当用户提出的问题超出你的知识库范畴时，可以利用该函数在知乎上进行搜索，并返回相关答案。
    :param q: 必选参数，和用户问题相关的知乎搜索的关键词，字符串类型对象
    :return：某问题的答案，以字符串形式呈现
    """
    # 默认搜索返回5个答案
    print('正在接入谷歌搜索，查找和问题相关的答案...')
    results = google_search(query=q, num_results=5, site_url='https://zhihu.com/')

    # 判断是否需要自动打开浏览器
    if os.getenv("search_with_broswer") == '1':
        # Edge 浏览器的路径，需要找到你的浏览器地址，假设使用edge浏览器
        edge_path = "C:/Program Files (x86)/Microsoft/Edge/Application/msedge.exe"
        # 注册 Edge 浏览器
        webbrowser.register('edge', None, webbrowser.BackgroundBrowser(edge_path))

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

        # 若打开浏览器，则停留3秒
        if os.getenv("search_with_broswer") == '1':
            webbrowser.get('edge').open(url)
            time.sleep(3)

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

> 这里额外设置一个`search_with_broswer`参数，用于控制是否在搜索的时候同步打开浏览器，以达到如下效果：

![](img/cc5521ea0e94fa6be413d8a1a950b139.png)

> 需要注意的是，无搜索的时候是否同步打开浏览器，都对搜索结果没有影响，只是为了增加一个可视化效果。

> `search_with_broswer`设置为`1`时代表打开浏览器，否则代表不打开浏览器。

```python
get_search_result(q="什么是MCP？")
```
