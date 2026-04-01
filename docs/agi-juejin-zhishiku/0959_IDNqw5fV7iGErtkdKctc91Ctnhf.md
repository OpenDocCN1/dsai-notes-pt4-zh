# 手把手教你实现大模型RAG[8]-chatglm3本地部署openai标准接口及接入

> 来源：[https://agijuejin.feishu.cn/docx/HqhGdKdNFotbV3xbfyqcW6XLnTb](https://agijuejin.feishu.cn/docx/HqhGdKdNFotbV3xbfyqcW6XLnTb)

> 作者：aC大

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/695644713)hihu.com/p/695644...

## 前言：

《手把手教你实现大模型RAG》系列文章分享的是从0到1[基于l](https://www.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695644713%7D)angchain实现大模型结合本地知识库的问答系统。主要内容包括：

+   [1]-手把手教你实现大模型RAG-大模型应用技术介绍

+   [2]-手把手教你实现大模型RAG-RAG流程的系统介绍

+   [3]-手把手教你实现大模型RAG-UI实现

+   [4]-手把手教你实现大模型RAG-RAG类的实现

+   [5]-手把手教你实现大模型RAG-基于langchain的文本切割

+   [6]-手把手教你实现大模型RAG-基于langchain的文本向量化

+   [7]-手把手教你实现大模型RAG-第三方大模型接口接入

+   [8]-手把手教你实现大模型RAG-chatglm3本地部署openai标准接口及接入

+   [9]-手把手教你实现大模型RAG-环境变量的配置和加载

本系列[文章会](https://www.zhihu.com/search?q=%E6%96%87%E7%AB%A0%E4%BC%9A&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695644713%7D)逐一分享上述内容，同时作者也出了该系列的视频课程，见B站《手把手教你实现大模型RAG-acedar讲师》, 文章和视频结合学习效果更佳。

### chatglm3本地部署openai标准接口及接入

### chatglm3的模型接口

现在的所有开源大模型，都可以封装成op[ena](https://www.zhihu.com/search?q=%E6%BA%90%E4%BB%A3%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695644713%7D)i标[准接口，在](https://www.zhihu.com/search?q=%E6%96%87%E6%9C%AC%E5%90%91%E9%87%8F%E5%8C%96&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695644713%7D)chatglm3的开源代码中，已经将模型的部署封装成标准的openai接口了。主要接口有： - health：检测心跳，判断是否成功链接部署的大模型 - v1/embeddings: 将文本向量化的接口，一般来说部署的大模型都会包括文本向量化的模型，当然也可以向本项目实现的一样，在实现RAG时自己加载文本向量化模型。 - v1/models: 查看接口支持哪些模型，这个接口一般时该接口部署了多个模型的时候使用多一点，在本项目的chatglm3中，只有一个模型也就不需要调用该接口了解有哪些模型。 - v1/chat/completions: 该接口就是标准的对话聊天的大模型接口，具体的参数看下文的测试代码。

框架代码如下，具体的代码实现可以从github上下载chatglm3的代码直接查看，代码目文件：ChatGLM3/openai_api_demo/api_server.py

```
python&quot;&gt;import os
import uvicorn

from fastapi import FastAPI, HTTPException, Response
from fastapi.middleware.cors import CORSMiddleware

from transformers import AutoTokenizer, AutoModel
from sentence_transformers import SentenceTransformer

# set Embedding Model path
EMBEDDING_PATH = os.environ.get(&#x27;EMBEDDING_PATH&#x27;, &#x27;BAAI/bge-large-zh-v1.5&#x27;)


app = FastAPI(lifespan=lifespan)

app.add_middleware(
CORSMiddleware,
allow_origins=[&quot;*&quot;],
allow_credentials=False,
allow_methods=[&quot;*&quot;],
allow_headers=[&quot;*&quot;],
)

@app.get(&quot;/health&quot;)
async def health() -&gt; Response:
&quot;&quot;&quot;Health check.&quot;&quot;&quot;
return Response(status_code=200)


@app.post(&quot;/v1/embeddings&quot;, response_model=EmbeddingResponse)
async def get_embeddings(request: EmbeddingRequest):
pass


@app.get(&quot;/v1/models&quot;, response_model=ModelList)
async def list_models():
model_card = ModelCard(
id=&quot;chatglm3-6b&quot;
)
return ModelList(
data=[model_card]
)


@app.post(&quot;/v1/chat/completions&quot;, response_model=ChatCompletionResponse)
async def create_chat_completion(request: ChatCompletionRequest):
global model, tokenizer
pass


async def predict(model_id: str, params: dict):
global model, tokenizer

pass


def predict_stream(model_id, gen_params):
pass


async def parse_output_text(model_id: str, value: str):
pass


def contains_custom_function(value: str) -&gt; bool:
pass

if __name__ == &quot;__main__&quot;:
# Load LLM
tokenizer = AutoTokenizer.from_pretrained(TOKENIZER_PATH, trust_remote_code=True)
model = AutoModel.from_pretrained(MODEL_PATH, trust_remote_code=True, device_map=&quot;auto&quot;).eval()

# load Embedding
embedding_model = SentenceTransformer(EMBEDDING_PATH, device=&quot;cuda&quot;)
uvicorn.run(app, host=&#x27;0.0.0.0&#x27;, port=8000, workers=1)
```

### chatglm3本地部署模型的[接口测试](https://www.zhihu.com/search?q=%E6%8E%A5%E5%8F%A3%E6%B5%8B%E8%AF%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695644713%7D)

部署好chatglm3后，可通过http接口测试接口的联通和模型的效果。代码如下：

```
import os
import requests
import json

url_root = os.environ.get(&quot;URL_ROOT&quot;, &#x27;http://127.0.0.1:8000&#x27;)

def health():
url = os.path.join(url_root, &#x27;health&#x27;)
headers = {
&#x27;Content-Type&#x27;: &#x27;application/json&#x27;
}
response = requests.request(&#x27;GET&#x27;, url, headers=headers)
print(&#x27;status:&#x27;, response.status_code)
assert response.status_code == 200

def main():
url = os.path.join(url_root, &#x27;v1/chat/completions&#x27;)
headers = {
&#x27;Content-Type&#x27;: &#x27;application/json&#x27;
}
payload = json.dumps({
&#x27;messages&#x27;: [
{
&#x27;role&#x27;: &#x27;user&#x27;,
&#x27;content&#x27;: &#x27;如何理财？&#x27;
}
],
&#x27;temperature&#x27;: 0.1,
&#x27;top_p&#x27;: 0.9,
&#x27;max_tokens&#x27;: 1024,
&#x27;stream&#x27;: False,
&#x27;model&#x27;: &#x27;chatglm3-6b&#x27;
})
response = requests.request(&#x27;POST&#x27;, url, headers=headers, data=payload)
print(&#x27;json:&#x27;, response.json())
return response.json()


if __name__ == &quot;__main__&quot;:
health()
json_res = main()
print(&#x27;json_res:&#x27;, json_res[&#x27;result&#x27;])
```

### chatglm3接入RAG的实现

langchain已经实现了chatglm3的接口， 只需要传入endpoint_url, top_p等参数即可，这些参数可以从环境变量中获取，当然在启动时需要加载这些配置到环境变量中。

```
import os
from langchain_community.llms.chatglm3 import ChatGLM3
endpoint_url = os.environ.get(&#x27;ENDPOINT_URL&#x27;)
max_tokens = int(os.environ.get(&quot;MAX_TOKENS&quot;, 1024))
top_p = float(os.environ.get(&quot;TOP_P&quot;, 0.9))

if not endpoint_url:
raise ValueError(&#x27;endpoint_url not right,url:{}&#x27;.format(endpoint_url))
llm = ChatGLM3(
endpoint_url=endpoint_url,
max_tokens=max_tokens,
top_p=top_p,
prefix_messages=[]
)
```

### langchain实现的chatglm3源码

langchain内部实现的chatglm3接口如下，从代码中可以看到，最核心的实现还是_call()方法，其他函数的定义都是辅助实现核心的_call()方法的。 特别需要强调的是，这里有AIMessage，和HumanMessage类的封装，对应的是大模型的返回结果和用户的问题，这个是langchain底层支持的格式，所以源码中有格式的转换操作。 其他代码比较好理解，这里就不过多解释了。

```
import json
import logging
from typing import Any, List, Optional, Union

from langchain_core.callbacks import CallbackManagerForLLMRun
from langchain_core.language_models.llms import LLM
from langchain_core.messages import (
AIMessage,
BaseMessage,
FunctionMessage,
HumanMessage,
SystemMessage,
)
from langchain_core.pydantic_v1 import Field

from langchain_community.llms.utils import enforce_stop_tokens

logger = logging.getLogger(__name__)
HEADERS = {&quot;Content-Type&quot;: &quot;application/json&quot;}
DEFAULT_TIMEOUT = 30


def _convert_message_to_dict(message: BaseMessage) -&gt; dict:
if isinstance(message, HumanMessage):
message_dict = {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: message.content}
elif isinstance(message, AIMessage):
message_dict = {&quot;role&quot;: &quot;assistant&quot;, &quot;content&quot;: message.content}
elif isinstance(message, SystemMessage):
message_dict = {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: message.content}
elif isinstance(message, FunctionMessage):
message_dict = {&quot;role&quot;: &quot;function&quot;, &quot;content&quot;: message.content}
else:
raise ValueError(f&quot;Got unknown type {message}&quot;)
return message_dict


class ChatGLM3(LLM):
&quot;&quot;&quot;ChatGLM3 LLM service.&quot;&quot;&quot;

model_name: str = Field(default=&quot;chatglm3-6b&quot;, alias=&quot;model&quot;)
endpoint_url: str = &quot;http://127.0.0.1:8000/v1/chat/completions&quot;
&quot;&quot;&quot;Endpoint URL to use.&quot;&quot;&quot;
model_kwargs: Optional[dict] = None
&quot;&quot;&quot;Keyword arguments to pass to the model.&quot;&quot;&quot;
max_tokens: int = 20000
&quot;&quot;&quot;Max token allowed to pass to the model.&quot;&quot;&quot;
temperature: float = 0.1
&quot;&quot;&quot;LLM model temperature from 0 to 10.&quot;&quot;&quot;
top_p: float = 0.7
&quot;&quot;&quot;Top P for nucleus sampling from 0 to 1&quot;&quot;&quot;
prefix_messages: List[BaseMessage] = Field(default_factory=list)
&quot;&quot;&quot;Series of messages for Chat input.&quot;&quot;&quot;
streaming: bool = False
&quot;&quot;&quot;Whether to stream the results or not.&quot;&quot;&quot;
http_client: Union[Any, None] = None
timeout: int = DEFAULT_TIMEOUT

@property
def _llm_type(self) -&gt; str:
return &quot;chat_glm_3&quot;

@property
def _invocation_params(self) -&gt; dict:
&quot;&quot;&quot;Get the parameters used to invoke the model.&quot;&quot;&quot;
params = {
&quot;model&quot;: self.model_name,
&quot;temperature&quot;: self.temperature,
&quot;max_tokens&quot;: self.max_tokens,
&quot;top_p&quot;: self.top_p,
&quot;stream&quot;: self.streaming,
}
return {**params, **(self.model_kwargs or {})}

@property
def client(self) -&gt; Any:
import httpx

return self.http_client or httpx.Client(timeout=self.timeout)

def _get_payload(self, prompt: str) -&gt; dict:
params = self._invocation_params
messages = self.prefix_messages + [HumanMessage(content=prompt)]
params.update(
{
&quot;messages&quot;: [_convert_message_to_dict(m) for m in messages],
}
)
return params

def _call(
self,
prompt: str,
stop: Optional[List[str]] = None,
run_manager: Optional[CallbackManagerForLLMRun] = None,
**kwargs: Any,
) -&gt; str:
&quot;&quot;&quot;Call out to a ChatGLM3 LLM inference endpoint.
Args:
prompt: The prompt to pass into the model.
stop: Optional list of stop words to use when generating.
Returns:
The string generated by the model.
Example:
.. code-block:: python
response = chatglm_llm(&quot;Who are you?&quot;)
&quot;&quot;&quot;
import httpx

payload = self._get_payload(prompt)
logger.debug(f&quot;ChatGLM3 payload: {payload}&quot;)

try:
response = self.client.post(
self.endpoint_url, headers=HEADERS, json=payload
)
except httpx.NetworkError as e:
raise ValueError(f&quot;Error raised by inference endpoint: {e}&quot;)

logger.debug(f&quot;ChatGLM3 response: {response}&quot;)

if response.status_code != 200:
raise ValueError(f&quot;Failed with response: {response}&quot;)

try:
parsed_response = response.json()

if isinstance(parsed_response, dict):
content_keys = &quot;choices&quot;
if content_keys in parsed_response:
choices = parsed_response[content_keys]
if len(choices):
text = choices[0][&quot;message&quot;][&quot;content&quot;]
else:
raise ValueError(f&quot;No content in response : {parsed_response}&quot;)
else:
raise ValueError(f&quot;Unexpected response type: {parsed_response}&quot;)

except json.JSONDecodeError as e:
raise ValueError(
f&quot;Error raised during decoding response from inference endpoint: {e}.&quot;
f&quot;\nResponse: {response.text}&quot;
)

if stop is not None:
text = enforce_stop_tokens(text, stop)

return text
```

更多相关内容，感兴趣的朋友可以同步关注B站(视频)：acedar-C大，知乎&公众号：aC大，也可加v进群交流：aa836304831