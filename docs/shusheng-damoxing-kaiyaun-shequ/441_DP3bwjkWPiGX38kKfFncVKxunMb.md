# 「圆桌」在 MindSearch 中支持新的搜索 API

> 来源：[https://aicarrier.feishu.cn/docx/Nu28d62fcoNIMVxWwKkcor5mn3g](https://aicarrier.feishu.cn/docx/Nu28d62fcoNIMVxWwKkcor5mn3g)

> 请注意，本文中展示的代码片段是精心挑选的重要部分，旨在简明扼要地阐述关键逻辑或功能。由于篇幅限制，这些代码并不构成完整的实现。为了全面理解和应用这些概念，强烈建议您参考相应的完整源代码。

[兴趣小组](https://aicarrier.feishu.cn/wiki/Wh3WwZ8Jaim5Iyk6vKscymecnmb)加入门槛：将MindSearch部署到Hugging Face上后联系浦语小助手入群。

# 定义

+   SeacherAgent指的是 MindSearch 里的两大智能体之一。

+   搜索API 指的是由第三方服务提供商提供的网络搜索接口，如 Google Serper API、Bing Web Search API等。

+   如果是在lagent/actions/bing_browser.py 支持新的搜索API，searcher 指的是实现新的 搜索API 所对应的类。

+   搜索 API Action 指的是支持新的 搜索API 所需要定义的 Action，如果是直接在 lagent/actions/bing_browser.py 支持新的搜索API，那这个 Action 指的就是 BingBrowser(BaseAction) 这个类。

---

# 前置

本文将介绍如何在 MindSearch 里支持的搜索 API，在开始之前，有以下几点需要注意：

+   根据 MindSearch 里的 SearcherAgent 提示词中的 few shot 例子，对于每个节点（由 MindSearchAgent 生成的子问题），SearcherAgent 会生成 多个 Query 用于搜索API来检索内容，所以其 搜索 API Action 中的 search函数 需要有处理多个 Query 的能力，而不是只处理一个单个的 Query。

```
### search
当我希望搜索&quot;王者荣耀现在是什么赛季&quot;时，我会按照以下格式进行操作:
现在是2024年，因此我应该搜索王者荣耀赛季关键词
&lt;|action_start|&gt;&lt;|plugin|&gt;{{&quot;name&quot;: &quot;FastWebBrowser.search&quot;, &quot;parameters&quot;: 
{{&quot;query&quot;: [&quot;王者荣耀 赛季&quot;, &quot;2024年王者荣耀赛季&quot;]}}}}&lt;|action_end|&gt;
```

+   根据 MindSearch 里的 SearcherAgent 提示词中的 few shot 例子，可以看出其实现的 搜索 API Action需要有个可以进一步检索网站内容的函数（网页内容的抓取），也就是 select函数 。这是因为多数 搜索API 返回的内容都是网站内容里的片段，并不会包含太多有用的信息。

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

+   搜索 API Action 中的 search函数 返回的最终内容最好符合以下所期望的格式，否则在 MindSearch 内部代码解析内容时有可能会报错，MindSearch 里的 SeacherAgent 会根据此结果来调用select函数：

```
[{&#x27;type&#x27;: &#x27;text&#x27;, &#x27;content&#x27;: &#x27;{&quot;0&quot;: {&quot;url&quot;: &quot;https:&quot;, &quot;summ&quot;: &quot;...&quot;, &quot;title&quot;: &quot;...&quot;},]
```

目前实现 搜索 API Action 的方法有2种：

+   方法一：在 lagent/actions/ 文件夹下新建一个文件，并且从零实现搜索 API Action。

+   方法二：直接在 lagent/actions/bing_browser.py 里支持新的搜索 API。

---

# bing_broswer.py 预览

+   由于 bing_browser.py 里已经内置了 search 函数 和 select 函数 等功能，并且妥善处理了前置章节中提及的关键注意事项。避免重复造轮子，本文将聚焦于在方法二的基础上，引入一个新的搜索API，以此扩展现有功能。

+   SearcherAgent 调用 BingBrowser 类的代码流程如下： def search() -> self.searcher.search() -> self.searcher._call_serper_api() -> self.searcher._parse_response() -> def select() -> self.fetcher.fetch()

## 接下来将简单介绍 bing_broswer.py 里重要的几个类和函数 :

### class BingBrowser(BaseAction) 类

+   此类是被设计为 SearcherAgent 中的 Action 组件。此类含有两个重要的函数，分别是 search 和 select，分别对应前置中的 第一点 和 第二点。

#### def search() 函数

+   单独给每个在 queries 列表中的 query 开一个线程，并且调用对应的 searcher.serach() 函数。

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

#### def select() 函数

+   单独给每个需要进一步查询的网页（由索引值代表） 开一个线程，并且调用 fetcher 去获取网站中的内容，这里的 fetcher 指的是 ContentFetcher 这一个类，所有的 searcher 都是共用这一个 fetcher。

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

### class ContentFetcher 类

+   调用 python 中的 request 模块爬取网站内容，并且调用 BeautifulSoup 结构化得到的 HTML 文档。

> 注意，需要 cookie 授权 的网站会访问失败。

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

### class BaseSearch 类

+   内有 _filter_results 函数，把 searcher 返回的内容统一格式化，对应 前置 中的第3点。

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

# 实现新的 Searcher 类

+   综上所述，bing_broswer.py 里已经提供了核心的相关类以及函数，现在只需要实现一个新的 Searcher（对应 bing_broswer.py 里的 def search() 函数中的 self.searcher）

+   以 GoogleSearch Seacher 为例（Google Serper API），需要实现的函数有：def search()，def _call_serper_api() 和 def _parse_response()，其中 def search() 是 Seacher 的主函数。

> 当前，SearcherAgent 中的提示词设计紧密围绕 BingBrowser 类展开。因此，为了最便捷地支持新的搜索API，我们只需在现有基础上新增一个 Searcher 类即可实现（方法二），这样的改动既直接又高效。否则，如果基于方法一实现且未遵循前置篇章中的注意事项，则可能需要对 SearcherAgent 中的提示词进行调整。

> 在开始实现新的 searcher 类之前，需要在 conda 环境中对 lagent 进行源码安装，以便 lagent 文件夹中的代码改动能够即时生效：

```
git clone https://github.com/InternLM/lagent.git
cd lagent
pip install -v -e .
```

## GoogleSearch 类

+   首先定义一个 GoogleSearch 类，继承 BaseSearch 类，并且将参数赋值为对象的属性（参数由 BingBrowser 类传入）。black_list 参数由 BaseSearch 类 调用，判断搜索 API 返回的网站是否处在黑名单当中。api_key ，search_type，kwargs 参数都是和 Google Serper API 相关的参数，使用于对 搜索 API 发送请求。

```
class GoogleSearch(BaseSearch):
    def __init__(self,
                 api_key: str,
                 topk: int = 3,
                 black_list: List[str] = [
                     &#x27;enoN&#x27;,
                     &#x27;youtube.com&#x27;,
                     &#x27;bilibili.com&#x27;,
                     &#x27;researchgate.net&#x27;,
                 ],
                 **kwargs):
        self.api_key = api_key
        self.proxy = kwargs.get(&#x27;proxy&#x27;)
        self.search_type = kwargs.get(&#x27;search_type&#x27;, &#x27;search&#x27;)
        self.kwargs = kwargs
        super().__init__(topk, black_list)
```

### def search() 函数

+   调用内部的 _call_serper_api 函数进行搜索，并随后调用内部 _parse_response 函数对返回的结果进行结构化处理。在调用过程中，如果发生异常，该函数会实施重试机制，即在短暂等待后重新尝试，直至达到预设的最大重试次数。

> 对于有每秒访问限制的搜索 API，由于用的多线程调用，此函数在尝试最大重试次数之后仍可能报错。

```
@cached(cache=TTLCache(maxsize=100, ttl=600))
    def search(self, query: str, max_retry: int = 3) -&gt; dict:
        for attempt in range(max_retry):
            try:
                response = self._call_serper_api(query)
                return self._parse_response(response)
            except Exception as e:
                logging.exception(str(e))
                warnings.warn(
                    f&#x27;Retry {attempt + 1}/{max_retry} due to error: {e}&#x27;)
                time.sleep(random.randint(2, 5))
        raise Exception(
            &#x27;Failed to get search results from Google Serper Search after retries.&#x27;
        )
```

### def _call_serper_api() 函数

+   对相对应的 搜索 API 发送请求，并且获得对应结果，其参数以及请求时的格式请参考对应的搜索 API 文档。

```
def _call_serper_api(self, query: str) -&gt; dict:
        endpoint = f&#x27;https://google.serper.dev/{self.search_type}&#x27;
        params = {
            &#x27;q&#x27;: query,
            &#x27;num&#x27;: self.topk,
            **{
                key: value
                for key, value in self.kwargs.items() if value is not None
            },
        }
        headers = {
            &#x27;X-API-KEY&#x27;: self.api_key or &#x27;&#x27;,
            &#x27;Content-Type&#x27;: &#x27;application/json&#x27;
        }
        response = requests.get(
            endpoint, headers=headers, params=params, proxies=self.proxy)
        response.raise_for_status()
        return response.json()
```

### def _parse_response() 函数

+   对于 搜索API 返回的每一个结果，将其提取并包装成 (url,snippest,title) 格式的元组，将这些元组添加到一个名为 raw_results 的列表中，随后将 raw_results 列表作为参数传递给 BaseSearch 类中的 _filter_results 函数。

```
def _parse_response(self, response: dict) -&gt; dict:
        raw_results = []

        for result in response[self.result_key_for_type[
                self.search_type]][:self.topk]:
            description = result.get(&#x27;snippet&#x27;, &#x27;&#x27;)
            attributes = &#x27;. &#x27;.join(
                f&#x27;{attribute}: {value}&#x27;
                for attribute, value in result.get(&#x27;attributes&#x27;, {}).items())
            raw_results.append(
                (result.get(&#x27;link&#x27;, &#x27;&#x27;),
                 f&#x27;{description}. {attributes}&#x27; if attributes else description,
                 result.get(&#x27;title&#x27;, &#x27;&#x27;)))

        return self._filter_results(raw_results)

```

---

---

# 在  Codespaces 上运行 GoogleSearch Searcher

## 配置 CodeSpace 环境

+   打开 [Codespaces](https://github.com/codespaces) ，选择 blank template。

![](img/a31ce1ec7c299d1b214a94fe8ce9d12b.png)

+   为 MindSearch 新建一个文件夹，并把 MindSearch 仓库 clone 下来。

```
mkdir -p /workspaces/mindsearch
cd /workspaces/mindsearch
git clone https://github.com/InternLM/MindSearch.git
cd MindSearch &amp;&amp; git checkout b832275 &amp;&amp; cd ..
```

+   为 MindSearch 创建一个新的 conda 环境。

```
# 创建环境
conda create -n mindsearch python=3.10 -y
# 激活环境
conda activate mindsearch
# 安装依赖
pip install -r /workspaces/mindsearch/MindSearch/requirements.txt
```

+   如果出现 CondaError: Run 'conda init' before 'conda activate' 报错，运行 source activate base 即可。

+   如果之前已经安装过 MindSearch 相关环境，则无需运行以上代码创建环境，只需重新安装最新的 lagent 即可。

```
conda activate mindsearch
# 卸载旧版本 lagent
pip uninstall lagent -y
# 安装最新版的 lagent
pip install git+https://github.com/InternLM/lagent.git
```

+   点击左上角的 三横杠图标，并且依次选择 文件，打开文件夹，即可打开所有在 mindsearch 目录下的文件夹。

![](img/c4dff0a73e76e5f6fd0b2700e7a4f8f6.png)

## 获取 API KEY

+   在 SiliconFlow API 页面 创建 API KEY。

> 如果没有账号的话，则需要在 [SiliconFlow](https://account.siliconflow.cn/login) 上注册一个账户。

![](img/05d9b3d6ac06a5a2b39101fb46b3253a.png)

+   接下来需要在 [Serper API 页面](https://serper.dev/api-key) 获取 API KEY。

> 如果没有账号的话，则需要在 [Serper](https://serper.dev/) 上注册一个账户。

![](img/c843e89c4505daf9246e9ade657a0080.png)

+   接下来需要把 API KEY 设置到 Codespace 的环境变量中

```
conda activate mindsearch
cd /workspaces/mindsearch/MindSearch
export SILICON_API_KEY=&#x27;你的  SILICONFLOW API KEY&#x27;
export BING_API_KEY=&#x27;你的 GOOGLE SERPER API KEY&#x27;
```

## 运行 MindSearch

+   启动 MindSearch 后端

```
conda activate mindsearch
cd /workspaces/mindsearch/MindSearch
python -m mindsearch.app --lang cn --model_format internlm_silicon --search_engine GoogleSearch
```

+   打开新的终端，运行 MindSearch 前端（这里运行的是 streamlit 版本）

```
conda activate mindsearch
cd /workspaces/mindsearch/MindSearch
streamlit run frontend/mindsearch_streamlit.py
```

+   Codespace 自带自动转发端口的功能，这里直接点击终端所显示的 streamlit 链接。

![](img/b04cb5ed8ee4efb57a71ab4b593721a5.png)

---

---

# 部署到 HuggingFace 上 (Streamlit + GoogleSearch Searcher)

## 创建新的 HF Space

+   打开 [HF Space](https://huggingface.co/spaces) 界面，点击右上角的 Create new Space。

![](img/a6866fd8a72273d858e89b0b8ab48b13.png)

+   依次输入 Space name，License，SDK 选择 Streamlit，Hardware 选用默认的即可。

![](img/19f4c16d5e6b77d5acf9e0d9268a617d.png)

+   点击右上角的 Settings，定位到 Variables and Secrets 区域，点击 New Secret，分别将 SiliconFlow 密钥以及 Google Serper Key 设置为环境变量 SILICON_API_KEY 和 BING_API_KEY。

![](img/dc652f9842042280e239b6f05204cc1e.png)

![](img/0229557afbb01ed64a03cdfbefc71897.png)

## 准备 HF Space 所需的文件

+   在 HF space 上使用 GoogleSearch Searcher 需要先在 Codespace 上手动构建 lagent 的轮子包 ，并且一并上传到 HF space 上。

```
conda activate mindsearch
cd /workspaces/mindsearch
git clone https://github.com/InternLM/lagent.git
cd lagent
git checkout b6bb4e0
#构建 lagent 轮子包
pip install wheel
python setup.py bdist_wheel
```

+   将 HF Space 仓库 clone 到 CodeSpace 上。

```
conda activate mindsearch
cd /workspaces/mindsearch
git clone https://huggingface.co/spaces/&lt;HF用户名&gt;/&lt;HF_Space_仓库名字&gt;
```

![](img/3391e4f53097b58e2cf7dc78548fd62d.png)

+   将需要的文件拷贝到 HF space 仓库中。

```
cp -r /workspaces/mindsearch/MindSearch/mindsearch /workspaces/mindsearch/&lt;HF_Space_仓库名字&gt;
cp /workspaces/mindsearch/MindSearch/requirements.txt /workspaces/mindsearch/&lt;HF_Space_仓库名字&gt;
cp /workspaces/mindsearch/lagent/dist/lagent-0.2.3-py3-none-any.whl /workspaces/mindsearch/&lt;HF_Space_仓库名字&gt;
```

+   创建一个 app.py 并复制一下代码，作为 HF Space 的主入口。

```
cd /workspaces/mindsearch/&lt;HF_Space_仓库名字&gt;
touch app.py
```

```
import json
import tempfile
import os
os.system(&quot;pip install lagent-0.2.3-py3-none-any.whl&quot;)
import requests
import streamlit as st
from lagent.schema import AgentStatusCode
from pyvis.network import Network

os.system(&quot;python -m mindsearch.app --lang cn --model_format internlm_silicon --search_engine GoogleSearch &amp;&quot;)

# Function to create the network graph
def create_network_graph(nodes, adjacency_list):
    net = Network(height=&#x27;500px&#x27;,
                  width=&#x27;60%&#x27;,
                  bgcolor=&#x27;white&#x27;,
                  font_color=&#x27;black&#x27;)
    for node_id, node_data in nodes.items():
        if node_id in [&#x27;root&#x27;, &#x27;response&#x27;]:
            title = node_data.get(&#x27;content&#x27;, node_id)
        else:
            title = node_data[&#x27;detail&#x27;][&#x27;content&#x27;]
        net.add_node(node_id,
                     label=node_id,
                     title=title,
                     color=&#x27;#FF5733&#x27;,
                     size=25)
    for node_id, neighbors in adjacency_list.items():
        for neighbor in neighbors:
            if neighbor[&#x27;name&#x27;] in nodes:
                net.add_edge(node_id, neighbor[&#x27;name&#x27;])
    net.show_buttons(filter_=[&#x27;physics&#x27;])
    return net

# Function to draw the graph and return the HTML file path
def draw_graph(net):
    path = tempfile.mktemp(suffix=&#x27;.html&#x27;)
    net.save_graph(path)
    return path

def streaming(raw_response):
    for chunk in raw_response.iter_lines(chunk_size=8192,
                                         decode_unicode=False,
                                         delimiter=b&#x27;\n&#x27;):
        if chunk:
            decoded = chunk.decode(&#x27;utf-8&#x27;)
            if decoded == &#x27;\r&#x27;:
                continue
            if decoded[:6] == &#x27;data: &#x27;:
                decoded = decoded[6:]
            elif decoded.startswith(&#x27;: ping - &#x27;):
                continue
            response = json.loads(decoded)
            yield (response[&#x27;response&#x27;], response[&#x27;current_node&#x27;])

# Initialize Streamlit session state
if &#x27;queries&#x27; not in st.session_state:
    st.session_state[&#x27;queries&#x27;] = []
    st.session_state[&#x27;responses&#x27;] = []
    st.session_state[&#x27;graphs_html&#x27;] = []
    st.session_state[&#x27;nodes_list&#x27;] = []
    st.session_state[&#x27;adjacency_list_list&#x27;] = []
    st.session_state[&#x27;history&#x27;] = []
    st.session_state[&#x27;already_used_keys&#x27;] = list()

# Set up page layout
st.set_page_config(layout=&#x27;wide&#x27;)
st.title(&#x27;MindSearch-思索&#x27;)

# Function to update chat
def update_chat(query):
    with st.chat_message(&#x27;user&#x27;):
        st.write(query)
    if query not in st.session_state[&#x27;queries&#x27;]:
        # Mock data to simulate backend response
        # response, history, nodes, adjacency_list
        st.session_state[&#x27;queries&#x27;].append(query)
        st.session_state[&#x27;responses&#x27;].append([])
        history = None
        # 暂不支持多轮
        message = [dict(role=&#x27;user&#x27;, content=query)]

        url = &#x27;http://localhost:8002/solve&#x27;
        headers = {&#x27;Content-Type&#x27;: &#x27;application/json&#x27;}
        data = {&#x27;inputs&#x27;: message}
        raw_response = requests.post(url,
                                     headers=headers,
                                     data=json.dumps(data),
                                     timeout=20,
                                     stream=True)

        for resp in streaming(raw_response):
            agent_return, node_name = resp
            if node_name and node_name in [&#x27;root&#x27;, &#x27;response&#x27;]:
                continue
            nodes = agent_return[&#x27;nodes&#x27;]
            adjacency_list = agent_return[&#x27;adj&#x27;]
            response = agent_return[&#x27;response&#x27;]
            history = agent_return[&#x27;inner_steps&#x27;]
            if nodes:
                net = create_network_graph(nodes, adjacency_list)
                graph_html_path = draw_graph(net)
                with open(graph_html_path, encoding=&#x27;utf-8&#x27;) as f:
                    graph_html = f.read()
            else:
                graph_html = None
            if &#x27;graph_placeholder&#x27; not in st.session_state:
                st.session_state[&#x27;graph_placeholder&#x27;] = st.empty()
            if &#x27;expander_placeholder&#x27; not in st.session_state:
                st.session_state[&#x27;expander_placeholder&#x27;] = st.empty()
            if graph_html:
                with st.session_state[&#x27;expander_placeholder&#x27;].expander(
                        &#x27;Show Graph&#x27;, expanded=False):
                    st.session_state[&#x27;graph_placeholder&#x27;]._html(graph_html,
                                                                height=500)
            if &#x27;container_placeholder&#x27; not in st.session_state:
                st.session_state[&#x27;container_placeholder&#x27;] = st.empty()
            with st.session_state[&#x27;container_placeholder&#x27;].container():
                if &#x27;columns_placeholder&#x27; not in st.session_state:
                    st.session_state[&#x27;columns_placeholder&#x27;] = st.empty()
                col1, col2 = st.session_state[&#x27;columns_placeholder&#x27;].columns(
                    [2, 1])
                with col1:
                    if &#x27;planner_placeholder&#x27; not in st.session_state:
                        st.session_state[&#x27;planner_placeholder&#x27;] = st.empty()
                    if &#x27;session_info_temp&#x27; not in st.session_state:
                        st.session_state[&#x27;session_info_temp&#x27;] = &#x27;&#x27;
                    if not node_name:
                        if agent_return[&#x27;state&#x27;] in [
                                AgentStatusCode.STREAM_ING,
                                AgentStatusCode.ANSWER_ING
                        ]:
                            st.session_state[&#x27;session_info_temp&#x27;] = response
                        elif agent_return[
                                &#x27;state&#x27;] == AgentStatusCode.PLUGIN_START:
                            thought = st.session_state[
                                &#x27;session_info_temp&#x27;].split(&#x27;```&#x27;)[0]
                            if agent_return[&#x27;response&#x27;].startswith(&#x27;```&#x27;):
                                st.session_state[
                                    &#x27;session_info_temp&#x27;] = thought + &#x27;\n&#x27; + response
                        elif agent_return[
                                &#x27;state&#x27;] == AgentStatusCode.PLUGIN_RETURN:
                            assert agent_return[&#x27;inner_steps&#x27;][-1][
                                &#x27;role&#x27;] == &#x27;environment&#x27;
                            st.session_state[
                                &#x27;session_info_temp&#x27;] += &#x27;\n&#x27; + agent_return[
                                    &#x27;inner_steps&#x27;][-1][&#x27;content&#x27;]
                        st.session_state[&#x27;planner_placeholder&#x27;].markdown(
                            st.session_state[&#x27;session_info_temp&#x27;])
                        if agent_return[
                                &#x27;state&#x27;] == AgentStatusCode.PLUGIN_RETURN:
                            st.session_state[&#x27;responses&#x27;][-1].append(
                                st.session_state[&#x27;session_info_temp&#x27;])
                            st.session_state[&#x27;session_info_temp&#x27;] = &#x27;&#x27;
                    else:
                        st.session_state[&#x27;planner_placeholder&#x27;].markdown(
                            st.session_state[&#x27;responses&#x27;][-1][-1] if
                            not st.session_state[&#x27;session_info_temp&#x27;] else st.
                            session_state[&#x27;session_info_temp&#x27;])
                with col2:
                    if &#x27;selectbox_placeholder&#x27; not in st.session_state:
                        st.session_state[&#x27;selectbox_placeholder&#x27;] = st.empty()
                    if &#x27;searcher_placeholder&#x27; not in st.session_state:
                        st.session_state[&#x27;searcher_placeholder&#x27;] = st.empty()
                    # st.session_state[&#x27;searcher_placeholder&#x27;].markdown(&#x27;&#x27;)
                    if node_name:
                        selected_node_key = f&quot;selected_node_{len(st.session_state[&#x27;queries&#x27;])}_{node_name}&quot;
                        if selected_node_key not in st.session_state:
                            st.session_state[selected_node_key] = node_name
                        if selected_node_key not in st.session_state[
                                &#x27;already_used_keys&#x27;]:
                            selected_node = st.session_state[
                                &#x27;selectbox_placeholder&#x27;].selectbox(
                                    &#x27;Select a node:&#x27;,
                                    list(nodes.keys()),
                                    key=f&#x27;key_{selected_node_key}&#x27;,
                                    index=list(nodes.keys()).index(node_name))
                            st.session_state[&#x27;already_used_keys&#x27;].append(
                                selected_node_key)
                        else:
                            selected_node = node_name
                        st.session_state[selected_node_key] = selected_node
                        if selected_node in nodes:
                            node = nodes[selected_node]
                            agent_return = node[&#x27;detail&#x27;]
                            node_info_key = f&#x27;{selected_node}_info&#x27;
                            if &#x27;node_info_temp&#x27; not in st.session_state:
                                st.session_state[
                                    &#x27;node_info_temp&#x27;] = f&#x27;### {agent_return[&quot;content&quot;]}&#x27;
                            if node_info_key not in st.session_state:
                                st.session_state[node_info_key] = []
                            if agent_return[&#x27;state&#x27;] in [
                                    AgentStatusCode.STREAM_ING,
                                    AgentStatusCode.ANSWER_ING
                            ]:
                                st.session_state[
                                    &#x27;node_info_temp&#x27;] = agent_return[
                                        &#x27;response&#x27;]
                            elif agent_return[
                                    &#x27;state&#x27;] == AgentStatusCode.PLUGIN_START:
                                thought = st.session_state[
                                    &#x27;node_info_temp&#x27;].split(&#x27;```&#x27;)[0]
                                if agent_return[&#x27;response&#x27;].startswith(&#x27;```&#x27;):
                                    st.session_state[
                                        &#x27;node_info_temp&#x27;] = thought + &#x27;\n&#x27; + agent_return[
                                            &#x27;response&#x27;]
                            elif agent_return[
                                    &#x27;state&#x27;] == AgentStatusCode.PLUGIN_END:
                                thought = st.session_state[
                                    &#x27;node_info_temp&#x27;].split(&#x27;```&#x27;)[0]
                                if isinstance(agent_return[&#x27;response&#x27;], dict):
                                    st.session_state[
                                        &#x27;node_info_temp&#x27;] = thought + &#x27;\n&#x27; + f&#x27;```json\n{json.dumps(agent_return[&quot;response&quot;], ensure_ascii=False, indent=4)}\n```&#x27;  # noqa: E501
                            elif agent_return[
                                    &#x27;state&#x27;] == AgentStatusCode.PLUGIN_RETURN:
                                assert agent_return[&#x27;inner_steps&#x27;][-1][
                                    &#x27;role&#x27;] == &#x27;environment&#x27;
                                st.session_state[node_info_key].append(
                                    (&#x27;thought&#x27;,
                                     st.session_state[&#x27;node_info_temp&#x27;]))
                                st.session_state[node_info_key].append(
                                    (&#x27;observation&#x27;,
                                     agent_return[&#x27;inner_steps&#x27;][-1][&#x27;content&#x27;]
                                     ))
                            st.session_state[&#x27;searcher_placeholder&#x27;].markdown(
                                st.session_state[&#x27;node_info_temp&#x27;])
                            if agent_return[&#x27;state&#x27;] == AgentStatusCode.END:
                                st.session_state[node_info_key].append(
                                    (&#x27;answer&#x27;,
                                     st.session_state[&#x27;node_info_temp&#x27;]))
                                st.session_state[&#x27;node_info_temp&#x27;] = &#x27;&#x27;
        if st.session_state[&#x27;session_info_temp&#x27;]:
            st.session_state[&#x27;responses&#x27;][-1].append(
                st.session_state[&#x27;session_info_temp&#x27;])
            st.session_state[&#x27;session_info_temp&#x27;] = &#x27;&#x27;
        # st.session_state[&#x27;responses&#x27;][-1] = &#x27;\n&#x27;.join(st.session_state[&#x27;responses&#x27;][-1])
        st.session_state[&#x27;graphs_html&#x27;].append(graph_html)
        st.session_state[&#x27;nodes_list&#x27;].append(nodes)
        st.session_state[&#x27;adjacency_list_list&#x27;].append(adjacency_list)
        st.session_state[&#x27;history&#x27;] = history

def display_chat_history():
    for i, query in enumerate(st.session_state[&#x27;queries&#x27;][-1:]):
        # with st.chat_message(&#x27;assistant&#x27;):
        if st.session_state[&#x27;graphs_html&#x27;][i]:
            with st.session_state[&#x27;expander_placeholder&#x27;].expander(
                    &#x27;Show Graph&#x27;, expanded=False):
                st.session_state[&#x27;graph_placeholder&#x27;]._html(
                    st.session_state[&#x27;graphs_html&#x27;][i], height=500)
            with st.session_state[&#x27;container_placeholder&#x27;].container():
                col1, col2 = st.session_state[&#x27;columns_placeholder&#x27;].columns(
                    [2, 1])
                with col1:
                    st.session_state[&#x27;planner_placeholder&#x27;].markdown(
                        st.session_state[&#x27;responses&#x27;][-1][-1])
                with col2:
                    selected_node_key = st.session_state[&#x27;already_used_keys&#x27;][
                        -1]
                    st.session_state[&#x27;selectbox_placeholder&#x27;] = st.empty()
                    selected_node = st.session_state[
                        &#x27;selectbox_placeholder&#x27;].selectbox(
                            &#x27;Select a node:&#x27;,
                            list(st.session_state[&#x27;nodes_list&#x27;][i].keys()),
                            key=f&#x27;replay_key_{i}&#x27;,
                            index=list(st.session_state[&#x27;nodes_list&#x27;][i].keys(
                            )).index(st.session_state[selected_node_key]))
                    st.session_state[selected_node_key] = selected_node
                    if selected_node not in [
                            &#x27;root&#x27;, &#x27;response&#x27;
                    ] and selected_node in st.session_state[&#x27;nodes_list&#x27;][i]:
                        node_info_key = f&#x27;{selected_node}_info&#x27;
                        for item in st.session_state[node_info_key]:
                            if item[0] in [&#x27;thought&#x27;, &#x27;answer&#x27;]:
                                st.session_state[
                                    &#x27;searcher_placeholder&#x27;] = st.empty()
                                st.session_state[
                                    &#x27;searcher_placeholder&#x27;].markdown(item[1])
                            elif item[0] == &#x27;observation&#x27;:
                                st.session_state[
                                    &#x27;observation_expander&#x27;] = st.empty()
                                with st.session_state[
                                        &#x27;observation_expander&#x27;].expander(
                                            &#x27;Results&#x27;):
                                    st.write(item[1])
                        # st.session_state[&#x27;searcher_placeholder&#x27;].markdown(st.session_state[node_info_key])

def clean_history():
    st.session_state[&#x27;queries&#x27;] = []
    st.session_state[&#x27;responses&#x27;] = []
    st.session_state[&#x27;graphs_html&#x27;] = []
    st.session_state[&#x27;nodes_list&#x27;] = []
    st.session_state[&#x27;adjacency_list_list&#x27;] = []
    st.session_state[&#x27;history&#x27;] = []
    st.session_state[&#x27;already_used_keys&#x27;] = list()
    for k in st.session_state:
        if k.endswith(&#x27;placeholder&#x27;) or k.endswith(&#x27;_info&#x27;):
            del st.session_state[k]

# Main function to run the Streamlit app
def main():
    st.sidebar.title(&#x27;Model Control&#x27;)
    col1, col2 = st.columns([4, 1])
    with col1:
        user_input = st.chat_input(&#x27;Enter your query:&#x27;)
    with col2:
        if st.button(&#x27;Clear History&#x27;):
            clean_history()
    if user_input:
        update_chat(user_input)
    display_chat_history()

if __name__ == &#x27;__main__&#x27;:
    main()
```

+   把 /workspaces/mindsearch/<HF_Space_仓库名字>/requirements.txt 中的 git+https://github.com/InternLM/lagent.git 删除掉，最终 requirements.txt 中的内容如下。

![](img/c8637213158cfb6ac3c96e63da81ea98.png)

## 上传文件到 HF Space

+   在 HF Access Tokens 页面创建一个带有写入权限的 token，并且复制此 token。

![](img/f6060f7edbfd5a4e0ac23beb63e553ed.png)

![](img/88d1beffdcd9feba985dc8c302dc0067.png)

+   将准备好的文件上传到 HF Space 中，这里需要使用上一步复制的 token 进行授权。

```
cd /workspaces/mindsearch/&lt;HF_Space_仓库名字&gt;
git add .
git commit -m &quot;create streamlit demo&quot;
git remote set-url origin https://&lt;HF用户名&gt;:&lt;上一步复制的token&gt;@huggingface.co/spaces/&lt;HF用户名&gt;/&lt;HF_Space_仓库名字&gt;
git push
```

+   等待 HF Space 配置环境即可，随后 HF Space 会自动启动 Streamlit 页面。

![](img/f984a792ff95ef3980cc61faee19cbb4.png)

---