# 手把手教你实现大模型RAG[9]-环境变量的配置和加载

> 来源：[https://agijuejin.feishu.cn/docx/RSgNdcUDPoKM6lx2SZ4cKLiKnMd](https://agijuejin.feishu.cn/docx/RSgNdcUDPoKM6lx2SZ4cKLiKnMd)

> 作者：aC大

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/695644874)hihu.com/p/695644...

## 前言：

《手把手教你实现大模型RAG》系列文章分享的是从0到1基[于lan](https://www.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695644874%7D)gchain实现大模型结合本地 知识库 的问答系统。主要内容包括：

+   [1]-手把手教你实现大模型RAG-大模型应用技术介绍

+   [2]-手把手教你实现大模型RAG-RAG流程的系统介绍

+   [3]-手把手教你实现大模型RAG-UI实现

+   [4]-手把手教你实现大模型RAG-RAG类的实现

+   [5]-手把手教你实现大模型RAG-基于langchain的文本切割

+   [6]-手把手教你实现大模型RAG-基于langchain的文本向量化

+   [7]-手把手教你实现大模型RAG-第三方大模型接口接入

+   [8]-手把手教你实现大模型RAG-chatglm3本地部署openai标准接口及接入

+   [[9]-手](https://www.zhihu.com/search?q=%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695644874%7D)把手教你实现大模型RAG- 环境变量 的配置和加载

本系列文章会逐一分享上述内容，同时作者也出了该系列的视频课程，见 B站《手把手教你实现大模型RAG-acedar讲师》 , 文章和视频结合学习效果更佳。

### 环境变量的配置

为了有效管理模型相关的配置，一般会使用单独的文件配置这些参数，然后在项目启动的最开始加载文件中的变量[到环境变](https://www.zhihu.com/search?q=%E6%A0%B9%E7%9B%AE%E5%BD%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695644874%7D)量中，这样后续的参数值获取从环境变量中读取即可， 本项目使用.env文件配置环境变量，.env文件在启动项目的 根目录 下，如：

```
# [通用配置](https://www.zhihu.com/search?q=%E9%80%9A%E7%94%A8%E9%85%8D%E7%BD%AE&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695644874%7D)[
ENDPOIN](https://www.zhihu.com/search?q=text2vec&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695644874%7D)T_URL=http://127.0.0.1:8000/v1/chat/completions
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
BAIDU_API_KEY = &quot;your api_key&quot;
BAIDU_SECRET_KEY = &quot;your secret key&quot;
```

### 环境变量的加载

加载文件保存到环境变量中的功能已经有相应的包，直接调用即可，代码如下：

```
from dotenv import load_dotenv
load_dotenv()
```

load_dotenv就是将当前.env文件加载并放到[环境变量中](https://www.zhihu.com/search?q=%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695644874%7D)，也可以通过该函数的参数制定 配置文件 的路径。上述这段代码直接在RAG类实例化之前调用即可。 该函数的效果等价于手动设置环境变量，如下：

```
import os
os.environ[&quot;BAIDU_API_KEY&quot;] = &#x27;your api key&#x27;
os.environ[&quot;BAIDU_SECRET_KEY&quot;] = &#x27;your secret key&#x27;
```

### 环境变量的获取

在使用时正常从环境变量中获取即可，比如：

```
import os
baidu_api_key = os.environ.get(&quot;BAIDU_API_KEY&quot;)
badiu_secret_key = os.environ.get(&quot;BAIDU_SECRET_KEY&quot;)
```

更多相关内容，感兴趣的朋友可以同步关注B站(视频)：acedar-C大，知乎&公众号：aC大，也可加v进群交流：aa836304831