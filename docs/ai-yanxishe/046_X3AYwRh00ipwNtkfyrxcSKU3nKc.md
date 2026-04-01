# 使用LangChain，给你的GPT实时联网

> 来源：[https://aiyanxishe.feishu.cn/docx/JvTmdNONVofhgAxFrxVcURFQnfe](https://aiyanxishe.feishu.cn/docx/JvTmdNONVofhgAxFrxVcURFQnfe)

## 程序员AI破局简介

> [《程序员AI破局指南》](https://t.zsxq.com/19EOflzkQ)由字节、百度、阿里等互联网大厂的AI业务相关高管共创，分享开箱即用、面向市场的AI编程知识和工具。

> 指南包括六个专栏:**AI编程实战、AI编程工具、AI高效办公、A**I副业赚钱、AI提示词工程、AI私人助手

> [程序员AI破局指南](https://t.zsxq.com/19EOflzkQ) 原价 599 元，限时特惠 199 元/年，5月1日恢复原价。

> 加入我们：https://t.zsxq.com/19EOflzkQ

> 购买后，添加群主微信加入 AI研习社陪伴社群，限时领取超值福利。

我们都知道 GPT 的数据是具有一定的延迟性的，当我们想收集当前的实时信息，直接和 chatgpt 对话是获取不到最近的信息的。

那我们该如何实现让 GPT 实时联网了？

### 实现 GPT 实时联网

为了实现 GPT 的实时联网，我们需要结合 LangChain 和一个实时的数据源。在这里，我们选择使用 SerpAPI 作为我们的实时数据获取工具。SerpAPI 是一个强大的搜索API，能够快速从互联网获取最新的信息。通过将 SerpAPI 与 LangChain 集成，我们可以在处理输入时获取实时数据，并将这些数据用于生成更准确和相关的响应。

![](img/e4724abfd4893b3bc85d21ebde1fc80f.png)

创建一个 API 服务

首先，我们需要创建一个 API 服务，使用 FastAPI 来处理请求和响应。

```
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel

app = FastAPI()

class RequestData(BaseModel):
    prompt: str

@app.post(&quot;/process&quot;)
def process_data(data: RequestData):
    response = handle_prompt(data.prompt)
    return {&quot;response&quot;: response}

if __name__ == &quot;__main__&quot;:
    import uvicorn
    uvicorn.run(app, host=&quot;0.0.0.0&quot;, port=8000)
```

集成 LangChain

假设已经有一个 lang_chain_process 函数处理输入，我们可以通过 LangChain 提供的接口来处理文本数据。

```
from langchain import LangChain

def lang_chain_process(prompt: str) -&gt; str:
    lc = LangChain(model=&quot;gpt-4&quot;)
    result = lc.process(prompt)
    return result
```

实现联网功能

为了实现实时联网，我们需要使用 SerpAPI 获取实时数据。安装 requests 库来处理 HTTP 请求。

```
pip install requests
```

创建一个函数来使用 SerpAPI 获取实时数据，并将其集成到 lang_chain_process 函数中。

```
import requests

def fetch_real_time_data(query: str) -&gt; str:
    api_key = &quot;YOUR_SERPAPI_KEY&quot;
    params = {
        &quot;q&quot;: query,
        &quot;api_key&quot;: api_key
    }
    response = requests.get(&quot;https://serpapi.com/search&quot;, params=params)
    if response.status_code == 200:
        data = response.json()
        result = data.get(&quot;organic_results&quot;, [])
        if result:
            return result[0].get(&quot;snippet&quot;, &quot;No data found&quot;)
        else:
            return &quot;No data found&quot;
    else:
        return &quot;Failed to fetch data&quot;

def lang_chain_process(prompt: str) -&gt; str:
    lc = LangChain(model=&quot;gpt-4&quot;)
    keywords = extract_keywords(prompt)
    real_time_data = fetch_real_time_data(keywords)
    result = lc.process(f&quot;{prompt}. Here is some real-time data: {real_time_data}&quot;)
    return result

def extract_keywords(prompt: str) -&gt; str:
    return &quot; &quot;.join(prompt.split()[:3])
```

### 完整的API服务代码

以下是将上述所有步骤整合在一起的完整代码示例：

```
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
import requests
from langchain import LangChain

app = FastAPI()

class RequestData(BaseModel):
    prompt: str

@app.post(&quot;/process&quot;)
def process_data(data: RequestData):
    response = handle_prompt(data.prompt)
    return {&quot;response&quot;: response}

def fetch_real_time_data(query: str) -&gt; str:
    api_key = &quot;YOUR_SERPAPI_KEY&quot;
    params = {
        &quot;q&quot;: query,
        &quot;api_key&quot;: api_key
    }
    response = requests.get(&quot;https://serpapi.com/search&quot;, params=params)
    if response.status_code == 200:
        data = response.json()
        result = data.get(&quot;organic_results&quot;, [])
        if result:
            return result[0].get(&quot;snippet&quot;, &quot;No data found&quot;)
        else:
            return &quot;No data found&quot;
    else:
        return &quot;Failed to fetch data&quot;

def lang_chain_process(prompt: str) -&gt; str:
    lc = LangChain(model=&quot;gpt-4&quot;)
    keywords = extract_keywords(prompt)
    real_time_data = fetch_real_time_data(keywords)
    result = lc.process(f&quot;{prompt}. Here is some real-time data: {real_time_data}&quot;)
    return result

def extract_keywords(prompt: str) -&gt; str:
    return &quot; &quot;.join(prompt.split()[:3])

def handle_prompt(prompt: str) -&gt; str:
    return lang_chain_process(prompt)

if __name__ == &quot;__main__&quot;:
    import uvicorn
    uvicorn.run(app, host=&quot;0.0.0.0&quot;, port=8000)
```

使用 LangChain 进行联网可以显著提升语言模型的功能和应用场景。通过集成像 SerpAPI 这样的实时数据源，开发者可以构建更智能、更实时的应用程序。LangChain 提供的高集成性、可扩展性和简化的开发过程，使其成为构建现代语言模型应用的理想选择。通过本文的示例代码，希望能帮助你更好地理解如何使用 LangChain 实现 GPT 实时联网，并在实际项目中加以应用。

文本来源于斑码合伙人白猫队长，队长在大模型应用开发方面有丰富的经验，大家如果有大模型相关的问题，可以咨询队长WX【bmdz123789】

更多内容可以点击目录： [手册内容汇总](https://aiyanxishe.feishu.cn/wiki/LBXcwPv6UiZgdfkp14Xc1Wo1n3b)

✨ 如果觉得有收获，请点一点下方的“有启发” ✨

🌱 我也会及时优化迭代内容，给大家更好的阅读体验 🌱

<https://aiyanxishe.feishu.cn/sync/IaSadpoOOsopKab1ajKcz6z3nAb>