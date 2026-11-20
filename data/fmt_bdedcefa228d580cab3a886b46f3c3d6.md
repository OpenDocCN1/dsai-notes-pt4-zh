
他开始根据我们的需求来给我设计方案了，他分析完后会给你计划，但计划也需要用户这边来确认需要做选择的东西，`cookie` 这玩意好弄，直接手机登录网页版📕然后手动获取 `cookie` 然后输入到配置文件中就行了，关于大模型分析，我个人是建议先用地的 NLP 库进行基础的关键词统计，等没问题了再改用大模型也可以。

![](img/2923a1de62d84548cde005255dcd26a3.png)

接下来看技术架构设计，分为数据层：主要采集📕数据，分析层：根据采集到的数据进行分析，报告层：根据分析出来的结果呈现出来给用户，这个架构设计很合理，没问题，我们继续往下看。

![](img/d828785673ea699d2e00e04056fbb3db.png)

### Agent配置参数

每个`Agent`都有专门的配置文件，可根据需求调整，下面是部分示例：

```
# QueryEngine/utils/config.py
class Config:
    max_reflections = 2           # 反思轮次
    max_search_results = 15       # 最大搜索结果数
    max_content_length = 8000     # 最大内容长度
    
# MediaEngine/utils/config.py  
class Config:
    comprehensive_search_limit = 10  # 综合搜索限制
    web_search_limit = 15           # 网页搜索限制
    
# InsightEngine/utils/config.py
class Config:
    default_search_topic_globally_limit = 200    # 全局搜索限制
    default_get_comments_limit = 500             # 评论获取限制
    max_search_results_for_llm = 50              # 传给LLM的最大结果数
```

确定工作流的具体文件和具体作用

![](img/4cfb29812dae9982a6844577ce2e1b85.png)

### 情感分析模型配置

```
# InsightEngine/tools/sentiment_analyzer.py
SENTIMENT_CONFIG = {
    'model_type': 'multilingual',     # 可选: 'bert', 'multilingual', 'qwen'等
    'confidence_threshold': 0.8,      # 置信度阈值
    'batch_size': 32,                 # 批处理大小
    'max_sequence_length': 512,       # 最大序列长度
}
```

## 接入不同的LLM模型

支持任意openAI调用格式的LLM提供商，只需要在`/config.py`中填写对应的`KEY`、`BASE_URL`、`MODEL_NAME`即可。

> 什么是openAI调用格式？下面提供一个简单的例子：

```
from openai import OpenAI

client = OpenAI(api_key="your_api_key", 
               base_url="https://aihubmix.com/v1")

response = client.chat.completions.create(
   model="gpt-4o-mini",
   messages=[
       {'role': 'user', 
        'content': "推理模型会给市场带来哪些新的机会"}
   ],
)

complete_response = response.choices[0].message.content
print(complete_response)
```

我觉得没啥问题，开始干就完事了，直接执行继续执行开始做这个舆情 agent 了。

![](img/e518635a5926f19c4247731872e708d2.png)

环境搭建完毕了，然后我们进行登录。

![](img/6827df7587e1c1be73a4bb7a360690d8.png)

将复制好的`cookie`复制到代码中的`.env`环境配置里面的`cookie`变量里面去，就可以了。

![](img/e8cb34ed3659156340da334d5b3fde04.png)

## 更改情感分析模型

### 多语言情感分析

```
cd SentimentAnalysisModel/WeiboMultilingualSentiment
python predict.py --text "This product is amazing!" --lang "en"
```

### 小参数Qwen3微调

```
cd SentimentAnalysisModel/WeiboSentiment_SmallQwen
python predict_universal.py --text "这次活动办得很成功"
```

### 基于BERT的微调模型

```
# 使用BERT中文模型
cd SentimentAnalysisModel/WeiboSentiment_Finetuned/BertChinese-Lora
python predict.py --text "这个产品真的很不错"
```

### GPT-2 LoRA微调模型

```
cd SentimentAnalysisModel/WeiboSentiment_Finetuned/GPT2-Lora
python predict.py --text "今天心情不太好"
```

### 传统机器学习方法

```
cd SentimentAnalysisModel/WeiboSentiment_MachineLearning
python predict.py --model_type "svm" --text "服务态度需要改进"
```

执行过程中自动写代码，然后自动开始测试，没想到效果还挺好的，竟然真出来了数据。

![](img/835cb941615e60677a62827786f3ecbd.png)

![](img/0ab6c43f74d62dbd69e59ae598966842.png)

[`舆情监控Agent.docx`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DDYDb1fNKooSm4xA89Bc5xUInKe)

等了一会，📕舆情 agent 开发完成了，只不过是数据版本，但Gemini3.1 还挺贴心写了个使用说明文档，告诉我咋用。

![](img/ad2001896e5c585793a58ee0a11b90a2.png)

这种生成完的数据报告都是makdown格式看着有点费劲，我的让他给我变成可视化才行，来，我继续指挥他给我们优化升级。输入指令：

```
要用 web 页面来给我呈现数据和报告，而且要用 react 技术来开发。
```

## 接入自定义业务数据库

### 修改数据库连接配置

```
# config.py 中添加您的业务数据库配置
BUSINESS_DB_HOST = "your_business_db_host";
BUSINESS_DB_PORT = 3306;
BUSINESS_DB_USER = "your_business_user";
BUSINESS_DB_PASSWORD = "your_business_password";
BUSINESS_DB_NAME = "your_business_database";
```

### 创建自定义数据访问工具

```
# InsightEngine/tools/custom_db_tool.py
class CustomBusinessDBTool:
    """自定义业务数据库查询工具"""
    
    def __init__(self):
        self.connection_config = {
            'host': config.BUSINESS_DB_HOST,
            'port': config.BUSINESS_DB_PORT,
            'user': config.BUSINESS_DB_USER,
            'password': config.BUSINESS_DB_PASSWORD,
            'database': config.BUSINESS_DB_NAME,
        }
    
    def search_business_data(self, query: str, table: str):
        """查询业务数据"""
        # 实现您的业务逻辑
        pass
    
    def get_customer_feedback(self, product_id: str):
        """获取客户反馈数据"""
        # 实现客户反馈查询逻辑
        pass
```

### 集成到`InsightEngine`

```
# InsightEngine/agent.py 中集成自定义工具
from .tools.custom_db_tool import CustomBusinessDBTool

class DeepSearchAgent:
    def __init__(self, config=None):
        # ... 其他初始化代码
        self.custom_db_tool = CustomBusinessDBTool()
    
    def execute_custom_search(self, query: str):
        """执行自定义业务数据搜索"""
        return self.custom_db_tool.search_business_data(query, "your_table")
```

现在web 版本已经开发完事了，还挺快呀

![](img/5412f44e78bf6cd354aee0d97a80c6a1.png)

![](img/23b2dedf1ddc5ef7695aaa2df6793b98.png)

![](img/8f86f56d04b836f46f0bec37242a2524.png)

我们试一下看看效果怎么样，我输入 ai 查询一下。

![](img/13593fbb2e898aebe3fc3e94db777db5.png)

![](img/ff1ebfec273395636aa960a4fc200824.png)

但是这个系统有两个问题，一个是下面的 ai 智能洞察报告格式很丑，需要优化一下

继续指挥Gemini3.1 给我干活：

```
你这个舆情 agent 有问题啊，AI 智能洞察报告下
```

![](img/f344df65f0350af4e5369835a36a6dea.png)

可以看到，效果已经达到预期了
