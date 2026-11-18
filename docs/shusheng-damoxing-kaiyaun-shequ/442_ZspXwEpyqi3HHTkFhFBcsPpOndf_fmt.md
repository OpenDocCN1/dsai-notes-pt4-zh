
# 如何在MindSearch中集成新的搜索API，全面提升智能搜索能力！

> 来源：[https://aicarrier.feishu.cn/docx/NkvudxMqLoMrLsxITTkcFGAWnoc](https://aicarrier.feishu.cn/docx/NkvudxMqLoMrLsxITTkcFGAWnoc)

> 请注意，本文中展示的代码片段是精心挑选的重要部分，旨在简明扼要地阐述关键逻辑或功能。由于篇幅限制，这些代码并不构成完整的实现。为了全面理解和应用这些概念，强烈建议您参考相应的完整源代码https://github.com/InternLM/lagent/blob/main/lagent/actions/bing_browser.py#L264。

在这个信息爆炸的时代，如何高效地从海量数据中提取有价值的信息成为了一项挑战。智能搜索技术的出现，为我们提供了一把解锁知识宝库的钥匙。今天，我们将深入探讨一个前沿的智能搜索系统——MindSearch，并详细介绍如何在 MindSearch 中集成新的 搜索 API，以扩展其功能和提升用户体验。

> MindSearch 开源链接

> 开源代码：https://github.com/InternLM/mindsearch

> 请注意，本文中展示的代码片段是精心挑选的重要部分，旨在简明扼要地阐述关键逻辑或功能。由于篇幅限制，这些代码并不构成完整的实现。为了全面理解和应用这些概念，强烈建议您参考相应的完整源代码https://github.com/InternLM/lagent/blob/main/lagent/actions/bing_browser.py#L264。

在深入探讨之前，让我们先明确本文中几个关键术语的定义：

+   `SeacherAgent` 指的是 MindSearch 里的两大智能体之一。

+   搜索 API 指的是由第三方服务提供商提供的网络搜索接口，如 `Google Serper API`、`Bing Web Search API` 等。

+   如果是在 `lagent/actions/bing_browser.py` 支持新的搜索 API，`searcher` 指的是实现新的 搜索 API 所对应的类。

+   搜索 API `Action` 指的是支持新的 搜索 API 所需要定义的 `Action`，如果是直接在 `lagent/actions/bing_browser.py` 支持新的搜索 API，那这个 `Action` 指的就是 `BingBrowser(BaseAction)` 这个类。

## 前置内容

MindSearch 是一个集成了先进人工智能技术的搜索系统，它通过两大智能体——`PlannerAgent` 和 `SearcherAgent`——协同工作，实现高效的信息检索。`SearcherAgent` 负责生成多个查询（查询重写），利用第三方 搜索 API 进行内容检索。为了更好地理解这一过程，我们需要详细了解 `SearcherAgent` 的工作机制及其与 搜索 API 的交互，主要包括以下三点。

+   根据 MindSearch 里的 `SearcherAgent` 提示词中的 few shot 例子，对于每个节点（由 `PlannerAgent` 生成的子问题），`SearcherAgent` 会生成 多个 `Query` 用于搜索 API 来检索内容，所以其 搜索 API `Action` 中的 `search()` 函数 需要有处理多个 `Query` 的能力，而不是只处理一个单个的 `Query`。

```
### search
当我希望搜索&quot;王者荣耀现在是什么赛季&quot;时，我会按照以下格式进行操作:
现在是2024年，因此我应该搜索王者荣耀赛季关键词
&lt;|action_start|&gt;&lt;|plugin|&gt;{{&quot;name&quot;: &quot;FastWebBrowser.search&quot;, &quot;parameters&quot;: 
{{&quot;query&quot;: [&quot;王者荣耀 赛季&quot;, &quot;2024年王者荣耀赛季&quot;]}}}}&lt;|action_end|&gt;
```

+   根据 MindSearch 里的 `SearcherAgent` 提示词中的 few shot 例子，可以看出其实现的 搜索 API `Action`需要有个可以进一步检索网站内容的函数（网页内容的抓取），也就是 `select()` 函数 。这是因为多数 搜索 API 返回的内容都是网站内容里的 片段，并不会包含太多有用的信息。

```
### select
为了找到王者荣耀s36赛季最强射手，我需要寻找提及王者荣耀s36射手的网页。初步浏览网页后，
发现网页0提到王者荣耀s36赛季的信息，但没有具体提及射手的相关信息。网页3提到“s36最强射手出现？”，
有可能包含最强射手信息。网页13提到“四大T0英雄崛起，射手荣耀降临”，可能包含最强射手的信息。
因此，我选择了网页3和网页13进行进一步阅读。
&lt;|action_start|&gt;&lt;|plugin|&gt;{{&quot;name&quot;: &quot;FastWebBrowser.select&quot;, 
&quot;parameters&quot;: {{&quot;index&quot;: [3, 13]}}}}&lt;|action_end|&gt;
&quot;&quot;&quot;
```

+   搜索 API `Action` 中的 `search()` 函数 返回的最终内容最好符合以下所期望的格式，否则在 MindSearch 内部代码解析内容时有可能会报错，MindSearch 里的 `SeacherAgent` 会根据此结果来调用 `select()` 函数：

```
[{&#x27;type&#x27;: &#x27;text&#x27;, &#x27;content&#x27;: &#x27;{&quot;0&quot;: {&quot;url&quot;: &quot;https:&quot;, &quot;summ&quot;: &quot;...&quot;, &quot;title&quot;: &quot;...&quot;},]
```

为了在 MindSearch 中支持新的搜索 API，我们可以采用两种方法：一是在 `lagent/actions/` 文件夹下新建文件从零开始实现，二是直接在现有的 `bing_browser.py` 中进行功能扩展。

本文将着重介绍第二种方法，即在现有代码基础上引入新的搜索 API，这样不仅避免了重复开发，还能确保代码的一致性和可维护性。鉴于 `bing_browser.py` 已内置了 `search()` 函数和 `select()` 函数等功能，并已妥善处理了前文提到的关键注意事项，我们将聚焦于通过这种方法来扩展现有功能。接下来，我们将详细解析 `bing_browser.py` 文件的内容。

## 浅析 `bing_broswer.py`

+   `SearcherAgent` 调用 `BingBrowser` 类的代码流程如下：`def search()` -> `self.searcher.search()` -> `self.searcher._call_serper_api()` -> `self.searcher._parse_response()` ->  `self.searcher._filter_results()` -> `def select()` -> `self.fetcher.fetch()`

+   在上述流程中，标记为黄色以及蓝色的函数是 `SearcherAgent` 触发 `search()` 函数时会执行到的关键函数。其中标记为蓝色的函数则是我们在支持新的 搜索 API 时，需要在新的 `searcher` 类中实现的函数。而标记为绿色的函数，则是在 `SearcherAgent` 触发 `select()` 函数时会执行到的函数。

+   在 `bing_browser.py` 文件中，定义了三个关键的类，它们分别是：

### `class BingBrowser(BaseAction)` 类

+   此类是被设计为 `SearcherAgent` 中的 `Action` 组件，负责处理搜索相关的核心逻辑。此类含有两个重要的函数，分别是 `search()` 和 `select()`，分别对应前置内容中的 第一点 和 第二点。

#### `def search()` 函数

+   当接收到 `SearcherAgent` 生成的多个 `query`（以列表形式表示）后，单独给每个在 `queries` 列表中的 `query` 开启一个线程，并且调用对应的 `searcher.serach()` 函数来执行相应的 搜索 API 调用。

```
@tool_api
def search(self, query: Union[str, List[str]]) -&gt; dict:
    &quot;&quot;&quot;BING search API
    Args:
        query (List[str]): list of search query strings
    &quot;&quot;&quot;
    queries = query if isinstance(query, list) else [query]
    search_results = {}
    
    with ThreadPoolExecutor() as executor:
        future_to_query = {
            executor.submit(self.searcher.search, q): q
            for q in queries
        }
```

#### `def select()` 函数

+   在 `SearcherAgent` 接收到 `search()` 函数返回的搜索 API 结果后，它会判断哪些网站的内容需要进一步深入查询，并调用 `select()` 函数来处理这些需求。`select()`函数会为每个需要深入查询的网页（通过索引值标识）单独开启一个线程，并利用 `ContentFetcher` 类（即 `fetcher`）来抓取这些网站的详细内容。值得注意的是，所有的 `searcher` 都共享同一个 `ContentFetcher` 实例。

```
@tool_api
def select(self, select_ids: List[int]) -&gt; dict:
    &quot;&quot;&quot;get the detailed content on the selected pages.
    
    Args:
        select_ids (List[int]): list of index to select. Max number of index to be selected is no more than 4.
    &quot;&quot;&quot;
    if not self.search_results:
        raise ValueError(&#x27;No search results to select from.&#x27;)
    
    new_search_results = {}
    with ThreadPoolExecutor() as executor:
        future_to_id = {
            executor.submit(self.fetcher.fetch,
                            self.search_results[select_id][&#x27;url&#x27;]):
            select_id
            for select_id in select_ids if select_id in self.search_results
        }
```

### `class ContentFetcher` 类

+   `ContentFetcher` 类中的 `fetch()` 函数负责使用 Python 的 `requests` 模块从网站抓取内容，并通过 `BeautifulSoup` 库将获取的 HTML 文档结构化。

> 注意，需要 `cookie` 授权的网站会访问失败。

```
class ContentFetcher:
    @cached(cache=TTLCache(maxsize=100, ttl=600))
    def fetch(self, url: str) -&gt; Tuple[bool, str]:
        try:
            response = requests.get(url, timeout=self.timeout)
            response.raise_for_status()
            html = response.content
        except requests.RequestException as e:
            return False, str(e)

        text = BeautifulSoup(html, &#x27;html.parser&#x27;).get_text()
        cleaned_text = re.sub(r&#x27;\n+&#x27;, &#x27;\n&#x27;, text)
        return True, cleaned_text
```

### `class BaseSearch` 类

+   这是实现新的 `Searcher` 类时需要继承的一个基类，其主要目的是调用内部的 `_filter_results()` 函数。该函数的作用是确保从 `searcher` 返回的内容不包含黑名单中的 `URL` ，并且确保返回的内容数量不超过 `topk`。同时对内容进行统一格式化，这对应于前置内容中的 第三点 要求。

```
class BaseSearch:
    def _filter_results(self, results: List[tuple]) -&gt; dict:
        filtered_results = {}
        count = 0
        for url, snippet, title in results:
            if all(domain not in url
                   for domain in self.black_list) and not url.endswith(&#x27;.pdf&#x27;):
                filtered_results[count] = {
                    &#x27;url&#x27;: url,
                    &#x27;summ&#x27;: json.dumps(snippet, ensure_ascii=False)[1:-1],
                    &#x27;title&#x27;: title
                }
                count += 1
                if count &gt;= self.topk:
                    break
        return filtered_results
```

## 实现新的 `Searcher` 类

+   综上所述，`bing_broswer.py` 里已经提供了核心的相关类以及函数，现在只需要实现一个新的 `Searcher` 类（对应 `bing_broswer.py` 里的 `def search()` 函数中的 `self.searcher`）。

> 当前，`SearcherAgent` 中的提示词设计紧密围绕 `BingBrowser` 类展开。因此，为了最便捷地支持新的搜索 API，我们只需在现有基础上新增一个 `Searcher` 类即可实现（方法二），这样的改动既直接又高效。否则，如果基于方法一实现且未遵循前置内容中的注意事项，则可能需要对 `SearcherAgent` 中的提示词进行调整。在开始实现新的 `searcher` 类之前，需要在 `conda` 环境中对 `lagent` 进行源码安装，以便 `lagent` 文件夹中的代码改动能够即时生效。

+   以 `GoogleSearch` `Seacher` 为例（`Google Serper API`），需要实现的函数有：`def search()`，`def _call_serper_api()` 和 `def _parse_response()`，其中 `def search()` 是 `Searcher` 的主函数。

+   首先定义一个 `GoogleSearch` 类，继承 `BaseSearch` 类，并且将参数赋值为对象的属性（参数由 `BingBrowser` 类传入）。`black_list` 参数由 `BaseSearch` 类中的 `_filter_results()` 函数调用。`api_key` ，`search_type`，`kwargs` 参数都是和 `Google Serper API` 相关的参数，使用于对 搜索 API 发送请求。`topk` 参数在向 搜索 API 发送请求时使用，并在 `_filter_results()` 函数中再次被调用，以进一步确保最终返回的内容数量不超过 `topk` 。



```
class GoogleSearch(BaseSearch):
    def __init__(self,
                 api_key: str,
                 topk: int = 3,
                 black_list: List[str] = [
                     'enoN',
                     'youtube.com',
                     'bilibili.com',
                     'researchgate.net',
                 ],
                 **kwargs):
        self.api_key = api_key
        self.proxy = kwargs.get('proxy')
        self.search_type = kwargs.get('search_type', 'search')
        self.kwargs = kwargs
        super().__init__(topk, black_list)
```

## `search()` 函数

+   调用内部的 `_call_serper_api` 函数进行搜索，并随后调用内部 `_parse_response` 函数对返回的结果进行结构化处理。在调用过程中，如果发生异常，该函数会实施重试机制，即在短暂等待后重新尝试，直至达到预设的最大重试次数。

> 对于有每秒访问限制的搜索 API，由于用的多线程调用，此函数在尝试最大重试次数之后仍可能报错。

```
@cached(cache=TTLCache(maxsize=100, ttl=600))
def search(self, query: str, max_retry: int = 3) -> dict:
    for attempt in range(max_retry):
        try:
            response = self._call_serper_api(query)
            return self._parse_response(response)
        except Exception as e:
            logging.exception(str(e))
            warnings.warn(
                f'Retry {attempt + 1}/{max_retry} due to error: {e}')
            time.sleep(random.randint(2, 5))
    raise Exception(
        'Failed to get search results from Google Serper Search after retries.'
    )
```

## `_call_serper_api()` 函数

+   对相对应的 搜索 API 发送请求，并且获得对应结果，其参数以及请求时的格式请参考对应的搜索 API 文档。

```
def _call_serper_api(self, query: str) -> dict:
    endpoint = f'https://google.serper.dev/{self.search_type}'
    params = {
        'q': query,
        'num': self.topk,
        **{
            key: value
            for key, value in self.kwargs.items() if value is not None
        },
    }
    headers = {
        'X-API-KEY': self.api_key or '',
        'Content-Type': 'application/json'
    }
    response = requests.get(
        endpoint, headers=headers, params=params, proxies=self.proxy)
    response.raise_for_status()
    return response.json()
```

## `_parse_response()` 函数

+   对于 搜索API 返回的每一个结果，将其提取并包装成 `(url,snippest,title)` 格式的元组，将这些元组添加到一个名为 `raw_results` 的列表中，随后将 `raw_results` 列表作为参数传递给 `BaseSearch` 类中的 `_filter_results` 函数。

```
def _parse_response(self, response: dict) -> dict:
    raw_results = []

    for result in response[self.result_key_for_type[
            self.search_type]][:self.topk]:
        description = result.get('snippet', '')
        attributes = '. '.join(
            f'{attribute}: {value}'
            for attribute, value in result.get('attributes', {}).items())
        raw_results.append(
            (result.get('link', ''),
             f'{description}. {attributes}' if attributes else description,
             result.get('title', '')))

    return self._filter_results(raw_results)
```

# 总结

本文深入探讨了在 MindSearch 中实现新的 搜索 API 所需注意的关键事项，并详细介绍了 `SearcherAgent` 的调用流程，包括涉及的类和函数。特别地，我们重点介绍了如何在 `bing_browser.py` 中支持新的搜索 API，具体包括实现新的 `Searcher` 类，以及定义 `search()`、`_call_serper_api()`和`_parse_response()`函数，以确保新的搜索 API 能够无缝集成并扩展现有功能。

# MindSearch 兴趣小组招募

> 亲爱的技术探索者们，

> 你是否对 MindSearch 的深邃原理和精妙实现充满好奇？是否渴望与顶尖研究员并肩探讨，共同挖掘技术的无限可能？现在，你的机会来了！加入我们的 MindSearch 兴趣小组，开启一场知识的深度探索之旅。

> 在这里，你将：

+   深入理解 MindSearch 的核心技术，揭秘其背后的奥秘。

+   与官方研究员面对面交流，碰撞思想的火花，解答心中的疑惑。

+   参与社区共建，编写详尽文档，支持新功能的开发，优化现有功能，让 MindSearch 更加强大。

> 我们诚邀积极贡献的你，不仅有机会获得宝贵的算力支持以及免费 API Token 支持，更能在这个充满活力的社区中，找到志同道合的伙伴，共同成长。

> 加入门槛：

+   完成第三期书生大模型实战营 MindSearch 闯关任务（将 MindSearch 部署到 Hugging Face）。

+   联系浦语小助手（🛰️：InternLM），开启你的 MindSearch 探索之旅。

> 不要犹豫，立即行动！让我们一起在 MindSearch 的世界里，探索未知，创造未来。

> 期待你的加入，共同书写技术的新篇章！

> MindSearch 兴趣小组 敬上
