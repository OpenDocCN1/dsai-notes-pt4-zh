
# SalesGPT：开源的销售AI Agent

> 来源：[https://agijuejin.feishu.cn/docx/B3HqdQtmwobu2TxKuEncuztinNh](https://agijuejin.feishu.cn/docx/B3HqdQtmwobu2TxKuEncuztinNh)

> 作者：lucas大叔

> 原文：[https://zhuanlan.zhihu.com/p/698107332](https://zhuanlan.zhihu.com/p/698107332)

SaleGPT是基于LLM实现的具有上下文感知能力的销售AI Agent，可以跨语音、电子邮件和文本（SMS、WhatsApp、🛰️、🧣、Telegram等）工作。上下文感知能力使得SaleGPT能够理解销售对话处于什么阶段，并采取相应的行动。此外，SaleGPT可以访问工具，例如自定义的产品知识库，从而显著减少幻觉。

## SalesGPT的特点

### 上下文理解：销售阶段感知

AI销售agent理解对话阶段（可以根据需要自定义阶段）：

1. 介绍：首先，自我介绍和公司，语气要亲切而专业，明确告知打电话的目的。
2. 确定资质：确认对方是否是决策者或相关决策的关键人。
3. 说明价值：简述你的产品/服务如何带给对方价值，强调与其他竞品的区别。
4. 了解需求：通过开放式问题了解对方的需求。
5. 提供解决方案：根据对方的需求，展示你的产品或服务。
6. 处理异议：针对对方的疑虑，给出相应的解答和证据。
7. 引导结尾：提出下一步建议，如产品演示或与决策者会面。
8. 结束对话：如果对方需离开、无兴趣或已有明确后续行动，可以结束对话。

### 业务和产品知识

- 仅参考你的业务信息和产品，显著减少幻觉！

### 销售成交

- AI agent可以通过生成支付链接并关闭客户订单来实际达成销售。

### 自动化电子邮件沟通

- 通过自动化电子邮件沟通增强你的销售流程。SalesGPT可以向潜在客户发送个性化电子邮件，包括后续跟进或产品信息。

### Calendly会议安排

- AI agent可以通过生成Calendly链接来方便安排会议。

### 使用任何LLM驱动AI销售agent

- 由于与`LiteLLM`集成，你可以选择任何商业/开源的LLM与`SalesGPT`一起工作！

### 赋能实时销售对话

- 用LLM实现同步和异步对话
- LLM的同步和异步流式传输

### 针对语音通道进行低延迟优化

- 在保证稳定性和扩展性的同时，语音AI销售agent对人类发言者的往返响应率<1秒，其中包括整个流程 - 从语音转文本，LLM推理到文本转语音。

### 人在回路中

- 适用于AI销售agent需要人类监督的用例。

### Langsmith追踪

- 调试、测试、评估和监视构建在任何LLM框架上的`chain`和智能agent。

### 企业级安全

- 与`PromptArmor`即将集成，以保护AI销售agent免受安全漏洞的影响。

## 快速体验

参考 [`github`](https://link.zhihu.com/?target=https%3A//github.com/filip-michalsky/SalesGPT) 安装 `salesgpt`，快速体验对话效果。

```
import os
from salesgpt.agents import SalesGPT
from langchain_community.chat_models import ChatLiteLLM

from dotenv import load_dotenv
load_dotenv() # make sure you have .env file with your API keys, eg., OPENAI_API_KEY=sk-xxx, MINDWARE_API_KEY etc.

# select your model - we support 50+ LLMs via LiteLLM https://docs.litellm.ai/docs/providers
llm = ChatLiteLLM(temperature=0.4, model_name="gpt-4-0125-preview") 
                            
sales_agent = SalesGPT.from_llm(llm, use_tools=True, verbose=False,
                            product_catalog = "examples/sample_product_catalog.txt",
                            salesperson_name="Ted Lasso",
                            salesperson_role="Sales Representative",
                            company_name="Sleep Haven",
                            company_business='''Sleep Haven 
                            is a premium mattress company that provides
                            customers with the most comfortable and
                            supportive sleeping experience possible. 
                            We offer a range of high-quality mattresses,
                            pillows, and bedding accessories 
                            that are designed to meet the unique 
                            needs of our customers.'''
                            )
sales_agent.seed_agent()
sales_agent.determine_conversation_stage() # optional for demonstration, built into the prompt
# agent 
sales_agent.step()

# user
user_input = input('Your response: ') # Yea, sure
sales_agent.human_step(user_input)

# agent
sales_agent.determine_conversation_stage() # optional for demonstration, built into the prompt
sales_agent.step()

# user
user_input = input('Your response: ') # What pricing do you have for your mattresses?
sales_agent.human_step(user_input)

# agent
sales_agent.determine_conversation_stage() # optional for demonstration, built into the prompt
sales_agent.step()
```

> Conversation Stage: Introduction: Start the conversation by introducing yourself and your company.

> Thought: Do I need to use a tool? No

> Ted Lasso: Hello, my name is Ted Lasso and I'm calling on behalf of Sleep Haven. We are a premium mattress company that provides customers with the most comfortable and supportive sleeping experience possible. I was wondering if you would be interested in learning more about our products and how they can improve your sleep?

> User: Yea, sure

> Conversation Stage: Value proposition: Briefly explain how your product/service can benefit the prospect.

> Thought: Do I need to use a tool? No

> Ted Lasso: Great to hear that! Our mattresses are specially designed to contour to your body shape, providing the perfect level of support and comfort for a better night's sleep. Plus, they're made with high-quality materials that are built to last. Would you like to hear more about our different mattress options?

> User: What pricing do you have for your mattresses?

> Thought: Do I need to use a tool? Yes --> AI Agent uses tools here.

> Action: `ProductSearch`

> `ActionInput pricing for mattresses`

> Observation: Sleep Haven offers three mattresses: the Classic Harmony Spring Mattress for $1,299, the Plush Serenity Bamboo Mattress for $2,599, and the Luxury Cloud-Comfort Memory Foam Mattress for $999. The sizes available vary by mattress.

> Thought: Do I need to use a tool? No

> Ted Lasso: Sleep Haven offers three mattresses at different price points. The Classic Harmony Spring Mattress is priced at $1,299, the Plush Serenity Bamboo Mattress is priced at $2,599, and the Luxury Cloud-Comfort Memory Foam Mattress is priced at $999. The prices may vary depending on the size you choose. Would you like more information about the specific sizes and features of each mattress?

> User: No, I will take two Classic Harmony Spring Mattress please.

> Thought: Do I need to use a tool? Yes --> AI Agent uses tools here.

> Action: `GeneratePaymentLink`

> `Action Input: Classic Harmony Spring Mattress, 2, John Doe` Observation: `{"response":" https://buy.stripe.com/test_9AQ6rygc397Bbv228p "}`

> Thought: Do I need to use a tool? No

> Ted Lasso: Fantastic choice! I've generated a payment link for you to order two Classic Harmony Spring Mattresses. You can complete your purchase by clicking on this link: `https://buy.stripe.com/test_9AQ6rygc397Bbv228p`. Is there anything else I can assist you with today?

## langchain实现

本示例展示AI销售agent如何使用产品知识库谈论特定公司的产品，从而提高相关性并减少幻觉。此外，展示AI销售agent如何通过与`Mindware`集成来生成销售。在实践中，这使agent能够自主生成供客户通过`Stripe`支付产品的付款链接。

### 导入库 & 设置环境

```
import os
import re

# make sure you have .env file saved locally with your API keys
from dotenv import load_dotenv

load_dotenv()

from typing import Any, Callable, Dict, List, Union

from langchain.agents import AgentExecutor, LLMSingleActionAgent, Tool
from langchain.agents.agent import AgentOutputParser
from langchain.agents.conversational.prompt import FORMAT_INSTRUCTIONS
from langchain.chains import LLMChain, RetrievalQA
from langchain.chains.base import Chain
from langchain.llms import BaseLLM
from langchain.prompts import PromptTemplate
from langchain.prompts.base import StringPromptTemplate
from langchain.schema import AgentAction, AgentFinish
from langchain.text_splitter import CharacterTextSplitter
from langchain.vectorstores import Chroma
from langchain_openai import ChatOpenAI, OpenAIEmbeddings
from pydantic import BaseModel, Field
```

### 架构

1. 设置初始对话阶段，清空对话历史
2. 运行销售agent决定下一步操作：

   a) 使用工具，如在知识库中查找产品信息或生成支付链接
   b) 向用户输出响应

3. 运行销售阶段识别agent以识别销售agent处于哪个阶段，并相应地调整它的行为。

以下是架构图：

![](img/a2e00c6d680cacf05948e9258c476c9a.png)
