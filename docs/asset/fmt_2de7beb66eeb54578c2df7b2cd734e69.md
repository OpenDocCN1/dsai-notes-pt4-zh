
```
def get_search_result(q):
    """
    当你无法回答某个问题时，调用该函数，能够获得答案
    :param q: 必选参数，询问的问题，字符串类型对象
    :return：某问题的答案，以字符串形式呈现
    """
    # 默认搜索返回5个答案
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

```
get_search_result(q)
```
