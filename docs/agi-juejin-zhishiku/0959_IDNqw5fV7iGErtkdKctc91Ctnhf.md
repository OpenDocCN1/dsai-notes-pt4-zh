
# 手把手教你实现大模型RAG[8]-chatglm3本地部署openai标准接口及接入

> 来源：[https://agijuejin.feishu.cn/docx/HqhGdKdNFotbV3xbfyqcW6XLnTb](https://agijuejin.feishu.cn/docx/HqhGdKdNFotbV3xbfyqcW6XLnTb)
> 作者：aC大
> 原文：[https://zhuanlan.zhihu.com/p/695644713](https://zhuanlan.zhihu.com/p/695644713)

## 前言

《手把手教你实现大模型RAG》系列文章分享的是从0到1基于`langchain`实现大模型结合本地知识库的问答系统。主要内容包括：

+ [1]-手把手教你实现大模型RAG-大模型应用技术介绍
+ [2]-手把手教你实现大模型RAG-RAG流程的系统介绍
+ [3]-手把手教你实现大模型RAG-UI实现
+ [4]-手把手教你实现大模型RAG-RAG类的实现
+ [5]-手把手教你实现大模型RAG-基于`langchain`的文本切割
+ [6]-手把手教你实现大模型RAG-基于`langchain`的文本向量化
+ [7]-手把手教你实现大模型RAG-第三方大模型接口接入
+ [8]-手把手教你实现大模型RAG-chatglm3本地部署openai标准接口及接入
+ [9]-手把手教你实现大模型RAG-环境变量的配置和加载

本系列文章会逐一分享上述内容，同时作者也出了该系列的视频课程，见B站《手把手教你实现大模型RAG-acedar讲师》，文章和视频结合学习效果更佳。

## chatglm3本地部署openai标准接口及接入

### chatglm3的模型接口

现在的所有开源大模型，都可以封装成`openai`标准接口，在`chatglm3`的开源代码中，已经将模型的部署封装成标准的`openai`接口了。主要接口有：

- `health`：检测心跳，判断是否成功链接部署的大模型
- `v1/embeddings`：将文本向量化的接口，一般来说部署的大模型都会包括文本向量化的模型，当然也可以向本项目实现的一样，在实现RAG时自己加载文本向量化模型。
- `v1/models`：查看接口支持哪些模型，这个接口一般是该接口部署了多个模型的时候使用多一点，在本项目的`chatglm3`中，只有一个模型也就不需要调用该接口了解有哪些模型。
- `v1/chat/completions`：该接口就是标准的对话聊天的大模型接口，具体的参数看下文的测试代码。

框架代码如下，具体的代码实现可以从github上下载`chatglm3`的代码直接查看，代码目录文件：`ChatGLM3/openai_api_demo/api_server.py`

```python
import os
import uvicorn

from fastapi import FastAPI, HTTPException, Response
from fastapi.middleware.cors import CORSMiddleware

from transformers import AutoTokenizer, AutoModel
from sentence_transformers import SentenceTransformer

# set Embedding Model path
EMBEDDING_PATH = os.environ.get('EMBEDDING_PATH', 'BAAI/bge-large-zh-v1.5')


app = FastAPI(lifespan=lifespan)

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=False,
    allow_methods=["*"],
    allow_headers=["*"],
)

@app.get("/health")
async def health() -> Response:
    """Health check."""
    return Response(status_code=200)


@app.post("/v1/embeddings", response_model=EmbeddingResponse)
async def get_embeddings(request: EmbeddingRequest):
    pass


@app.get("/v1/models", response_model=ModelList)
async def list_models():
    model_card = ModelCard(
        id="chatglm3-6b"
    )
    return ModelList(
        data=[model_card]
    )


@app.post("/v1/chat/completions", response_model=ChatCompletionResponse)
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


def contains_custom_function(value: str) -> bool:
    pass

if __name__ == "__main__":
    # Load LLM
    tokenizer = AutoTokenizer.from_pretrained(TOKENIZER_PATH, trust_remote_code=True)
    model = AutoModel.from_pretrained(MODEL_PATH, trust_remote_code=True, device_map="auto").eval()

    # load Embedding
    embedding_model = SentenceTransformer(EMBEDDING_PATH, device="cuda")
    uvicorn.run(app, host='0.0.0.0', port=8000, workers=1)
```

## chatglm3本地部署模型的接口测试

部署好`chatglm3`后，可通过`http`接口测试接口的联通和模型的效果。代码如下：

```python
import os
import requests
import json

url_root = os.environ.get('URL_ROOT', 'http://127.0.0.1:8000')

def health():
    url = os.path.join(url_root, 'health')
    headers = {
        'Content-Type': 'application/json'
    }
    response = requests.request('GET', url, headers=headers)
    print('status:', response.status_code)
    assert response.status_code == 200

def main():
    url = os.path.join(url_root, 'v1/chat/completions')
    headers = {
        'Content-Type': 'application/json'
    }
    payload = json.dumps({
        'messages': [
            {
                'role': 'user',
                'content': '如何理财？'
            }
        ],
        'temperature': 0.1,
        'top_p': 0.9,
        'max_tokens': 1024,
        'stream': False,
        'model': 'chatglm3-6b'
    })
    response = requests.request('POST', url, headers=headers, data=payload)
    print('json:', response.json())
    return response.json()


if __name__ == "__main__":
    health()
    json_res = main()
    print('json_res:', json_res['result'])
```

## chatglm3接入RAG的实现

`langchain`已经实现了`chatglm3`的接口，只需要传入`endpoint_url`、`top_p`等参数即可，这些参数可以从环境变量中获取，当然在启动时需要加载这些配置到环境变量中。

```python
import os
from langchain_community.llms.chatglm3 import ChatGLM3
endpoint_url = os.environ.get('ENDPOINT_URL')
max_tokens = int(os.environ.get("MAX_TOKENS", 1024))
top_p = float(os.environ.get("TOP_P", 0.9))

if not endpoint_url:
    raise ValueError('endpoint_url not right,url:{}'.format(endpoint_url))
llm = ChatGLM3(
    endpoint_url=endpoint_url,
    max_tokens=max_tokens,
    top_p=top_p,
    prefix_messages=[]
)
```

## langchain实现的chatglm3源码

`langchain`内部实现的`chatglm3`接口如下，从代码中可以看到，最核心的实现还是`_call()`方法，其他函数的定义都是辅助实现核心的`_call()`方法的。特别需要强调的是，这里有`AIMessage`和`HumanMessage`类的封装，对应的是大模型的返回结果和用户的问题，这个是`langchain`底层支持的格式，所以源码中有格式的转换操作。其他代码比较好理解，这里就不过多解释了。



```python
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
HEADERS = {"Content-Type": "application/json"}
DEFAULT_TIMEOUT = 30


def _convert_message_to_dict(message: BaseMessage) -> dict:
    if isinstance(message, HumanMessage):
        message_dict = {"role": "user", "content": message.content}
    elif isinstance(message, AIMessage):
        message_dict = {"role": "assistant", "content": message.content}
    elif isinstance(message, SystemMessage):
        message_dict = {"role": "system", "content": message.content}
    elif isinstance(message, FunctionMessage):
        message_dict = {"role": "function", "content": message.content}
    else:
        raise ValueError(f"Got unknown type {message}")
    return message_dict


class ChatGLM3(LLM):
    """ChatGLM3 LLM service."""

    model_name: str = Field(default="chatglm3-6b", alias="model")
    endpoint_url: str = "http://127.0.0.1:8000/v1/chat/completions"
    """Endpoint URL to use."""
    model_kwargs: Optional[dict] = None
    """Keyword arguments to pass to the model."""
    max_tokens: int = 20000
    """Max token allowed to pass to the model."""
    temperature: float = 0.1
    """LLM model temperature from 0 to 10."""
    top_p: float = 0.7
    """Top P for nucleus sampling from 0 to 1"""
    prefix_messages: List[BaseMessage] = Field(default_factory=list)
    """Series of messages for Chat input."""
    streaming: bool = False
    """Whether to stream the results or not."""
    http_client: Union[Any, None] = None
    timeout: int = DEFAULT_TIMEOUT

    @property
    def _llm_type(self) -> str:
        return "chat_glm_3"

    @property
    def _invocation_params(self) -> dict:
        """Get the parameters used to invoke the model."""
        params = {
            "model": self.model_name,
            "temperature": self.temperature,
            "max_tokens": self.max_tokens,
            "top_p": self.top_p,
            "stream": self.streaming,
        }
        return {**params, **(self.model_kwargs or {})}

    @property
    def client(self) -> Any:
        import httpx

        return self.http_client or httpx.Client(timeout=self.timeout)

    def _get_payload(self, prompt: str) -> dict:
        params = self._invocation_params
        messages = self.prefix_messages + [HumanMessage(content=prompt)]
        params.update(
            {
                "messages": [_convert_message_to_dict(m) for m in messages],
            }
        )
        return params

    def _call(
        self,
        prompt: str,
        stop: Optional[List[str]] = None,
        run_manager: Optional[CallbackManagerForLLMRun] = None,
        **kwargs: Any,
    ) -> str:
        """Call out to a ChatGLM3 LLM inference endpoint.

        Args:
            prompt: The prompt to pass into the model.
            stop: Optional list of stop words to use when generating.

        Returns:
            The string generated by the model.

        Example:
            .. code-block:: python

                response = chatglm_llm("Who are you?")
        """
        import httpx

        payload = self._get_payload(prompt)
        logger.debug(f"ChatGLM3 payload: {payload}")

        try:
            response = self.client.post(
                self.endpoint_url, headers=HEADERS, json=payload
            )
        except httpx.NetworkError as e:
            raise ValueError(f"Error raised by inference endpoint: {e}")

        logger.debug(f"ChatGLM3 response: {response}")

        if response.status_code != 200:
            raise ValueError(f"Failed with response: {response}")

        try:
            parsed_response = response.json()

            if isinstance(parsed_response, dict):
                content_keys = "choices"
                if content_keys in parsed_response:
                    choices = parsed_response[content_keys]
                    if len(choices):
                        text = choices[0]["message"]["content"]
                    else:
                        raise ValueError(f"No content in response : {parsed_response}")
                else:
                    raise ValueError(f"Unexpected response type: {parsed_response}")
            else:
                raise ValueError(f"Unexpected response type: {parsed_response}")

        except json.JSONDecodeError as e:
            raise ValueError(
                f"Error raised during decoding response from inference endpoint: {e}."
                f"\nResponse: {response.text}"
            )

        if stop is not None:
            text = enforce_stop_tokens(text, stop)

        return text
```

更多相关内容，感兴趣的朋友可以同步关注B站（视频）：acedar-C大，知乎&👸号：aC大，也可加v进群交流：aa836304831
