
# 手把手教你实现大模型RAG[9]-环境变量的配置和加载

> 来源：[https://agijuejin.feishu.cn/docx/RSgNdcUDPoKM6lx2SZ4cKLiKnMd](https://agijuejin.feishu.cn/docx/RSgNdcUDPoKM6lx2SZ4cKLiKnMd)
> 作者：aC大
> 原文：[https://zhuanlan.zhihu.com/p/695644874](https://zhuanlan.zhihu.com/p/695644874)

## 前言

《手把手教你实现大模型RAG》系列文章分享的是从0到1基于`langchain`实现大模型结合本地知识库的问答系统。主要内容包括：

+ [1]-手把手教你实现大模型RAG-大模型应用技术介绍
+ [2]-手把手教你实现大模型RAG-RAG流程的系统介绍
+ [3]-手把手教你实现大模型RAG-UI实现
+ [4]-手把手教你实现大模型RAG-RAG类的实现
+ [5]-手把手教你实现大模型RAG-基于`langchain`的文本切割
+ [6]-手把手教你实现大模型RAG-基于`langchain`的文本向量化
+ [7]-手把手教你实现大模型RAG-第三方大模型接口接入
+ [8]-手把手教你实现大模型RAG-`chatglm3`本地部署`openai`标准接口及接入
+ [9]-手把手教你实现大模型RAG-环境变量的配置和加载

本系列文章会逐一分享上述内容，同时作者也出了该系列的视频课程，见 B站《手把手教你实现大模型RAG-acedar讲师》，文章和视频结合学习效果更佳。

### 环境变量的配置

为了有效管理模型相关的配置，一般会使用单独的文件配置这些参数，然后在项目启动的最开始加载文件中的变量到环境变量中，这样后续的参数值获取从环境变量中读取即可，本项目使用`.env`文件配置环境变量，`.env`文件在启动项目的根目录下，如：

```
# 通用配置
ENDPOINT_URL=http://127.0.0.1:8000/v1/chat/completions
MAX_TOKENS=1024
TOP_P=0.9
MODEL_NAME=chatglm3
EMBED_MODEL_NAME=text2vec-base
FIlE_PATH=
CHUNK_SIZE=256
CHUNK_OVERLAP=128

PERSIST_DIRECTORY=./data/vector_store
VECTOR_DB_NAME = chroma
TEXT_SPLIT_METHOD=chat

# 百度api
BAIDU_API_KEY = "your api_key"
BAIDU_SECRET_KEY = "your secret key"
```

### 环境变量的加载

加载文件保存到环境变量中的功能已经有相应的包，直接调用即可，代码如下：

```
from dotenv import load_dotenv
load_dotenv()
```

`load_dotenv()`就是将当前`.env`文件加载并放到环境变量中，也可以通过该函数的参数指定配置文件的路径。上述这段代码直接在`RAG`类实例化之前调用即可。该函数的效果等价于手动设置环境变量，如下：

```
import os
os.environ["BAIDU_API_KEY"] = 'your api key'
os.environ["BAIDU_SECRET_KEY"] = 'your secret key'
```

### 环境变量的获取

在使用时正常从环境变量中获取即可，比如：

```
import os
baidu_api_key = os.environ.get("BAIDU_API_KEY")
badiu_secret_key = os.environ.get("BAIDU_SECRET_KEY")
```

更多相关内容，感兴趣的朋友可以同步关注B站(视频)：acedar-C大，知乎&👸号：aC大，也可加v进群交流：aa836304831
