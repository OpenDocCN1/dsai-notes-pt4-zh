# 智谱联网搜索

> 来源：[https://v11enp9ok1h.feishu.cn/docx/WqhzdtH5oofl8AxjAqDcWxGvnvh](https://v11enp9ok1h.feishu.cn/docx/WqhzdtH5oofl8AxjAqDcWxGvnvh)

![](img/b8edad49335b30c0ffdc6331efbbf90f.png)

ZhipuAI/Zhipu-Web-Search -智谱网络搜索MCP服务器是专为大模型设计的搜索引擎，整合了四种搜索引擎，可以让用户灵活对比和切换。在传统搜索引擎的网页爬取和排序能力基础上，增强意图识别能力，返回更适用于大模型处理

---

## 简介

Zhipu Web Search MCP Server是一个专给大模型用的搜索引擎，整合了4家搜索引擎供用户灵活对比切换，在传统搜索引擎网页抓取、排序的能力基础上，增强了意图识别能力，返回更适合大模型处理的结果（网页标题、网页URL、网页摘要、网站名称、网站图标等），帮助AI应用获得“动态知识获取”与“精准场景适配”的能力。

**Overview**

什么是Zhipu Web Search MCP服务？

[Zhipu ](https://zhipuaishengchan.datasink.sensorsdata.cn/t/cE)Web Search MCP Server是智谱开放平台（BigModel.cn)推出的一个专给大模型用的搜索引擎，整合了4家搜索引擎供用户灵活对比切换，在传统搜索引擎网页抓取、排序的能力基础上，增强了意图识别能力，返回更适合大模型处理的结果（网页标题、网页URL、网页摘要、网站名称、网站图标等），帮助 AI 应用获得“动态知识获取”与“精准场景适配”的能力。

如何使用Zhipu Web Search MCP？

支持运行 MCP 协议的客户端，如Cursor[、Cherry Studio等](https://zhipuaishengchan.datasink.sensorsdata.cn/t/cE)中配置，在智谱 BigModel开放平台复制您的 API 密钥，并按照文档内容设置服务器命令。

Zhipu Web Search MCP服务的关键特性

**实时联网搜索**： 实时检索全网信息和网页链接。

**意图增强检索**： 结合智谱自研的向量、语义匹配、时效性、内容质量度等模型，针对用户提问进行意图识别，优化提取query 搜索词。同时使用自研 semantic reranker 分析用户问题与给定搜索结果的相关度，结合时效性给出打分和排序，为用户提供准确可靠的搜索结果。

**多引擎支持** : 整合智谱自研引擎及主流搜索引擎（搜狗/夸克/Jina.ai），开发者可以按场景需求灵活调用，发挥不同搜索工具的优势。

结构化输出：返回适合LLM处理的数据格式（含标题/URL/摘要/网站名/图标等）

Zhipu Web Search MCP服务的使用场景

专业垂直研究： 进行高质量网页检索、多源信息整合，如学术研究、法律、金融行业分析报告。

商业情报雷达： 实时数据跟踪与分析，如监控行业动态、竞争对手信息、市场趋势等。

AI 助手/聊天机器人： 提供实时信息搜索能力，确保回答准确性和时效性。

消费决策与规划设计： 根据最新天气、新闻、车票等信息、进行多个选项对比，寻找最优解。

人才画像与简历优化： 对于hr，搜索对比市场上的jd招聘信息，帮助生成或优化jd；对于求职者，搜索对比简历与目标岗位JD及相关行业成功简历的模板，帮助生成/优化简历。

安装教程

支持运行 MCP 协议的客户端，如Cursor、Cherry Studio等。

点击获取智谱 BigModel 开放平台的API Key。

常见问题解答

Q：如何获取API Key？

A：需在[智谱BigModel开放平台](https://zhipuaishengchan.datasink.sensorsdata.cn/t/cE)注册开发者账号，获取 API Key。

Q：使用Zhipu Web Search MCP是否需要付费？

A：您在[智谱BigModel开放平台](https://zhipuaishengchan.datasink.sensorsdata.cn/t/cE)可获得免费额度。若免费额度耗尽，则需要付费使用。有任何疑问可在智谱BigModel 开放平台中咨询客服。

Q：支持哪些搜索引擎？

A：支持智谱自研引擎及主流搜索引擎。智谱自研引擎：search_std（基础版）、search_pro（高阶版）。第三方引擎：search_pro_sogou （搜狗）、search_pro_quark（夸克）、search_pro_jina（Jina.ai ）。

Server Config

```
{
    &quot;mcpServers&quot;: {
        &quot;zhipu-web-search-sse&quot;: {
            &quot;command&quot;: &quot;docker&quot;,
            &quot;url&quot;: &quot;https://open.bigmodel.cn/api/mcp/web_search/sse?Authorization={you ak/sk}&quot;
        }
    }
}
```

Content

安装教程

支持运行 MCP 协议的客户端，如Cursor、Cherry Studio等。

点击获取智谱 BigModel 开放平台的API Key。

在Cursor中使用

Cursor 0.45.6 版本提供了MCP功能，Cursor将作为MCP服务客户端使用MCP服务，在Cursor中通过简单的配置就可以完成MCP服务的接入。

操作路径：Cursor设置->【Features】->【MCP Servers】。

配置MCP服务器

```
{
    &quot;mcpServers&quot;: {
        &quot;zhipu-web-search-sse&quot;: {
            &quot;url&quot;: &quot;https://open.bigmodel.cn/api/mcp/web_search/sse?Authorization=YOUR API Key&quot;
        }
    }
}

```