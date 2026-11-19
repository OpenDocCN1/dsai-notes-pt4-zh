
# 手把手教你实现大模型RAG[7]-第三方大模型接口接入

> 来源：[https://agijuejin.feishu.cn/docx/K9ccdUNmPoruNOxE7LycMoOen8b](https://agijuejin.feishu.cn/docx/K9ccdUNmPoruNOxE7LycMoOen8b)
>
> 作者：aC大
>
> 原文：[https://zhuanlan.zhihu.com/p/695644527](https://zhuanlan.zhihu.com/p/695644527)

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

本系列 [文章会](https://www.zhihu.com/search?q=%E6%96%87%E7%AB%A0%E4%BC%9A&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695644527%7D)逐一分享上述内容，同时作者也出了该系列的视频课程，见 B站《手把手教你实现大模型RAG-acedar讲师》，文章和视频结合学习效果更佳。

## 第三方大模型接口接入

使用第三方的大模型，往往都是通过云服务提供的，每个平台的接口稍有不同。本项目接入百度大模型平台，以李开复的零一万物34B大模型为例，接入本项目的RAG。首先登录"百度智能云"平台，接着在左侧菜单栏中选择"应用接入"，然后通过"创建应用"创建应用，创建成功后就可以看到API key和secret key，这个在接口调用的时候需要填写。

![](img/58aada1a772f7b3118cffb2f133063f6.png)

## 百度大模型接口调试

创建好应用后，可以通过API文档查看调用的示例。将示例代码在本项目中测试，是否能成功调用百度大模型，代码如下：

```
import os
import requests
import json

from dotenv import load_dotenv

load_dotenv()

API_KEY = os.environ.get("API_KEY")
SECRET_KEY = os.environ.get("SECRET_KEY")


def main():
    url = "https://aip.baidubce.com/rpc/2.0/ai_custom/v1/wenxinworkshop/chat/yi_34b_chat?access_token=" + get_access_token()

    request_data = {
        "messages": [
            {"role": "user", "content": "如何保持好的睡眠？"}
        ],
        "top_p": 0.9,
        "temperature": 0.1,
        "stream": False
    }
    payload = json.dumps(request_data)
    headers = {
        'Content-Type': 'application/json'
    }

    response = requests.request("POST", url, headers=headers, data=payload)
    print(response.json()['result'])

def get_access_token():
    """
    使用 AK，SK 生成鉴权签名（Access Token）
    :return: access_token，或是None(如果错误)
    """
    url = "https://aip.baidubce.com/oauth/2.0/token"
    params = {"grant_type": "client_credentials", "client_id": API_KEY, "client_secret": SECRET_KEY}
    return str(requests.post(url, params=params).json().get("access_token"))


if __name__ == '__main__':
    main()
```

调用百度平台的 API Key 和 Secret Key 通过环境变量获取，这便于在结合到本项目中也可以按该方式使用。百度大模型接口需要先请求权限验证，并将验证结果拼接到大模型的访问 `url` 中，保证访问的合法性。

## RAG百度模型接入封装

本项目基于`langchain`实现RAG，百度平台的大模型集成到本项目中，也要符合`langchain`调用大模型的规则，也就需要继承`langchain`的大语言模型基类。- 继承`langchain`的`LLM`基类，该基类最核心的也是需要根据具体模型实现的是`_call`方法。- 为了区分不同平台的大模型调用，百度的 API Key 和 Secret key 的环境变量使用加上前缀"Baidu"区分，便于后续接入其他模型时 `Key` 的统一管理。- 这里实现的百度接口调用的 `temperature`、`top_p` 等参数直接写在代码中，但实际项目中，可以配置到环境变量中，便于统一的管理这些参数。继承到本项目中的百度大模型调用实现如下：

```
import os
import json
import requests
from typing import Optional, List, Any, Mapping
from langchain_core.language_models.llms import LLM
from langchain_core.pydantic_v1 import Field
from langchain_core.callbacks import CallbackManagerForLLMRun


class BaiduLlm(LLM):

    model_name: str = Field(default='')

    @property
    def _llm_type(self) -> str:
        return 'baidu-' + self.model_name

    @property
    def _identifying_params(self) -> Mapping[str, Any]:
        """Get the identifying parameters."""
        return {}

    def get_access_token(self):
        """
        使用 AK，SK 生成鉴权签名（Access Token）
        :return: access_token，或是None(如果错误)
        """
        API_KEY = os.environ.get("BAIDU_API_KEY")
        SECRET_KEY = os.environ.get("BAIDU_SECRET_KEY")
        url = "https://aip.baidubce.com/oauth/2.0/token"
        params = {"grant_type": "client_credentials", "client_id": API_KEY, "client_secret": SECRET_KEY}
        return str(requests.post(url, params=params).json().get("access_token"))

    def _call(self,
        prompt: str,
        stop: Optional[List[str]] = None,
        run_manager: Optional[CallbackManagerForLLMRun] = None,
        **kwargs: Any):

        url = "https://aip.baidubce.com/rpc/2.0/ai_custom/v1/wenxinworkshop/chat/{}?access_token={}".format(self.model_name, self.get_access_token())

        print('prompt:', prompt)
        request_data = {
            "messages": [
                {"role": "user", "content": prompt}
            ],
            "top_p": 0.9,
            "temperature": 0.1,
            "stream": False
        }
        payload = json.dumps(request_data)
        headers = {
            'Content-Type': 'application/json'
        }
        response = requests.request("POST", url, headers=headers, data=payload)

        try:
            ans = response.json()['result']
        except Exception as err:
            ans = '调用百度接口错误' + response.json()['error_msg']
            print("ans: {} , err: {}".format(ans, err))
        return ans
```

更多相关内容，感兴趣的朋友可以同步关注B站(视频)：acedar-C大，知乎&👸号：aC大，也可加v进群交流：aa836304831
