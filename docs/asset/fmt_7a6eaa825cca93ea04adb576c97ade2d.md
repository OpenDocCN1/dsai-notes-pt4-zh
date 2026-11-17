
# 【补充资料】Serper API使用与数据预处理拆解

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/BJ6ed1Z1Go3UuvxA52HceIBznxf](https://kq4b3vgg5b.feishu.cn/docx/BJ6ed1Z1Go3UuvxA52HceIBznxf)

## 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》：

![](img/6c9733c5c3b5d4ecdbb2074db32c1e60.png)

![](img/17825ff2f95c4bd5afd992d1d79ffe95.png)

此外，公开课全部课件，以及项目代码、数据等学习资料，扫码⬆️联系助教即可领取～

---

## 使用SerperAPI做实时联网检索

基于大模型的问答系统做联网实时检索是一个比较先进的应用场景。在这种场景中，系统不仅利用预先训练的知识，还能实时访问互联网或特定的数据库来检索信息，以回答用户的问题。这种方式可以极大地扩展问答系统的应用范围和准确性。

一般来说，这种实时检索问答系统包括以下几个关键组件：

1. **用户查询解析**：解析用户的问题，理解其意图和关键信息。
1. **实时检索**：根据解析得到的关键词或查询意图，系统会实时从互联网或指定的数据库中检索相关信息。
1. **信息整合与回答生成**：将检索到的信息与大模型的预训练知识结合，生成准确且信息丰富的答案。

这种系统通常需要强大的后端支持，包括网络数据的实时访问、数据处理和安全措施，以及高效的信息检索和自然语言处理算法。而要做联网检索问答的链路，需要借助一个可供实时搜索的外部API。

这里我们推荐Serper，该服务是一个高性能的 Google 搜索 API，提供快速且成本效益高的方式访问 Google 搜索结果。在目前的应用落地产品中广泛被用于增强聊天机器人、进行搜索引擎优化（SEO）分析和简化金融科技项目等多种场景。该API具备：

+   执行实时搜索
+   自定义查询位置
+   快速访问 Google 的搜索引擎结果——通常在1-2秒内

以上特性使其特别适用于需要立即检索最新网络数据的应用。

## 1. 注册Serper账户

SerperAPI官方地址：`https://serper.dev/` 。进入后先进行注册：

![](img/40f0056d9634098827e718558a0b1635.png)

登入账户后，在如下位置获取有效的API Key：

![](img/4bebf250a93a71d7e9c69b1a6c6988db.png)

新用户注册会免费赠送2500条免费请求，即可通过该API Key 查询2500次 Google Search 服务。

![](img/ea9ae5ada074f5393d200116f310a0d4.png)

获取到API Key后，我们可以使用 Python 的 `requests` 库来调用 Serper API 进行连通性测试，如下是示例代码：

```
# ! pip install requests
```

```
# requests 库用于发起网络请求
import requests
import json

params = {
    'api_key': 'c8da0155b57b159becfdbf4767aef63306ae4619',  # 验证请求的 API 密钥, 请替换成自己的
    'q': '2024赛季NBA的总冠军是哪支球队？',    # 查询参数，表示要搜索的问题。
    'num': 3          # 这个参数指定了返回结果的数量，这里设置为3，意味着 API 将返回三个相关的搜索结果。         
}

# 使用 requests.get() 方法发起一个 GET 请求到 Serper API 的 endpoint。这个请求包含了上述设置的参数。
api_result = requests.get('https://google.serper.dev/search', params)

search_data = api_result.json()

print(search_data)
```

```
{'searchParameters': {'q': '2024赛季NBA的总冠军是哪支球队？', 'type': 'search', 'num': 3, 'engine': 'google'}, 'answerBox': {'title': '2023–24 NBA season / Champion', 'answer': 'Boston Celtics'}, 'knowledgeGraph': {'title': '2023–24 NBA season (2023–24 NBA赛季)', 'imageUrl': 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSoAkm4fChk_rY5NN5liUG5vyniB_BP5y5xiMiJpEPWxVjAQjGtyTkg3yg&s=0', 'description': 'The 2023–24 NBA season was the 78th season of the National Basketball Association. The regular season began on October 24, 2023, and ended on April 14, 2024.', 'descriptionSource': 'Wikipedia', 'descriptionLink': 'https://en.wikipedia.org/wiki/2023%E2%80%9324_NBA_season', 'attributes': {'Start date': 'October 24, 2023', 'Champion': 'Boston Celtics', 'Location': 'United States and Canada', 'Eastern runners-up': 'Indiana Pacers', 'Finals MVP': 'Jaylen Brown (Boston)', 'League': 'National Basketball Association', 'Number of games': '82'}}, 'organic': [{'title': '2024年NBA总决赛 - 维基百科', 'link': 'https://zh.wikipedia.org/zh-hans/2024%E5%B9%B4NBA%E7%B8%BD%E6%B1%BA%E8%B3%BD', 'snippet': '2024年NBA总决赛（英语：2024 NBA Finals）是2023–24 NBA赛季的冠军系列赛 ... 波士顿凯尔特人为1946-47 BAA赛季加入的11支NBA球队之一，并为队史第23次闯进总 ...', 'position': 1}, {'title': '2024年NBA总冠军是哪支球队? - 搜狐', 'link': 'https://www.sohu.com/a/745482265_121124692', 'snippet': '2023-24赛季打了差不多一些比赛，东西部以争冠为目标的球队无非就是绿军、雄鹿、76人、热火；掘金、勇士、太阳、湖人、快船，或许还可以加上独行侠。', 'date': 'Dec 20, 2023', 'position': 2}, {'title': '2024年NBA季后赛- 维基百科', 'link': 'https://zh.wikipedia.org/zh-hans/2024%E5%B9%B4NBA%E5%AD%A3%E5%BE%8C%E8%B3%BD', 'snippet': '2024年NBA季后赛是国家篮球协会（NBA）在2023-24 NBA赛季常规赛完毕后举行 ... 季后赛实行七场四胜制，每支球队将与对手最多进行七场比赛，最先赢得四场比赛的球 ...', 'position': 3}], 'relatedSearches': [{'query': '2024 nba总决赛时间'}, {'query': 'nba总决赛2024回放'}, {'query': 'nba季后赛2024赛程'}, {'query': 'nba季后赛2024对阵图'}, {'query': '2024年nba季后赛直播'}, {'query': 'nba总冠军'}, {'query': 'nba季后赛赛程图'}, {'query': 'nba季后赛排名'}]}
```

如果能够正常接收到Serper API 返回的数据，则说明调用成功，可以继续进行接下来的代码实践。

这里我们可以借助 `pprint` 格式化输出。`pprint` 是 Python 的一个库，它的全称是 "Pretty Printer"。这个库提供了一个函数 `pprint()`，可以将 Python 对象以一种格式化和易于阅读的方式打印出来。使用 `pprint` 打印数据结构，尤其是在处理嵌套结构或包含多个字段的大型数据时，可以极大地提高可读性。

```
# 导入pprint函数
from pprint import pprint  

# 设置缩进为2，宽度为100字符
pprint(search_data, indent=2, width=100)  
```
