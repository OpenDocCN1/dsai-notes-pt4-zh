
# 基于MCP+NIM+Dify实现的台式机导购机器人

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/CAbQdurlKossdrxvXLXclIpIncc](https://qqs7y1hozd1.feishu.cn/docx/CAbQdurlKossdrxvXLXclIpIncc)

> 项目聚焦于解决传统对话机器人“机械应答”“无法理解用户意图”等问题，通过更智能、更自然的交互体验，提升用户在选购产品过程中的满意度与效率。

项目围绕台式机产品，构建了面向约 50 款机型的知识库，打造了一款智能导购对话机器人。

> 依托 MCP 协议、Dify 平台和本地部署的 NVIDIA NIM 微服务，实现了私有化部署和端到端的智能问答能力。

**具体亮点如下：**

1. **私有部署与安全性保障：**通过 4 张 NVIDIA RTX 5880 Ada 显卡搭建本地服务器，并本地部署 NIM 微服务和 Dify 平台。

2. **协议集成与功能扩展：**基于 MCP 协议，支持查询联想专卖店功能，有效增强了机器人对接真实业务场景的能力。

3. **混合数据检索：**集成了 RAG（Retrieval-Augmented Generation）与 SQL 技术，实现了对结构化（如数据库）与非结构化（如文档知识库）数据的统一高效查询。

4. **多模交互体验：**自研对话机器人前端页面，支持文本输入与选项卡式输入。

5. **智能联动能力：**引入 Function Call，实现对联网搜索的支持，帮助用户匹配最合适的台式机配置方案。

## Dify

本项目采用 Dify 开源框架，以低代码方式实现了整个推荐对话流程。

> 其整体架构主要包括：澄清用户意图、区分商用或消费用户、提取用户关键词、进行知识库与联网搜索，最终实现精准产品推荐。

![](img/19bc0b23024802152fb4fd338ffa6ed5.png)

![](img/37d30c491b7c538fbef2bd3fa9dfa799.png)

![](img/11ef32a9e3dafbe6587b1415e47bef00.png)

## NIM（NVIDIA Inference Microservices）

NVIDIA NIM 是 NVIDIA AI Enterprise 的一部分，为跨云、数据中心和工作站的自托管 GPU 加速推理微服务提供容器，用于预训练和自定义 AI 模型。

> 在 NIM 方面，在 4 张 NVIDIA RTX 5880 Ada 显卡（显存 192GB）上部署 NIM 微服务，综合考虑显存和 NIM 平台上支持的模型列表，我们在 `docker` 上部署了 `llama-3.3-70b-instruct` 模型，实现推理。

在此基础上，成功打通了本地 NIM 微服务与本地 Dify 平台，利用 Dify 对 NIM 链接的原生支持，通过配置 NIM 的 URL 和模型名称实现了集成。

![](img/209e087d733766e246d9c0d95bd6a8b1.png)

![](img/339b26270e39310381c8194dba0b8497.png)

## MCP（Model Context Protocol）

在 MCP 方面，项目利用 Dify 自带的 MCP 插件，实现了在机器人推荐产品时同步获取用户经纬度信息，从而帮助用户查询附近最近的联想专卖店。

> 为满足项目需求，采用了高德地图的 MCP server 解决方案，并通过在 Dify 中配置相关文件和 Prompt，将 Dify 的响应在前台页面进行呈现。

![](img/a5feddb600c0c91982766fc64d73f412.png)

![](img/9a232e2bc436cb104b546460c4ed3537.png)

```
const handleStoreSearch = async() => {
    try {
        // Get user's location
        const position = await new Promise<GeolocationPosition>((resolve, reject) => { navigator.geolocation.getCurrentPosition(resolve, reject); });
        const { longitude, latitude } = position.coords;
        const query = '请帮我找下经纬度'${longitude},${latitude}'附近50公里内的联想专卖店''; 
```

![](img/d3cf89758cf1d672d71f8b521ae2c75c.png)

## 混合数据库检索（RAG+SQL）

在知识库检索方面，采用 RAG 技术和 SQL 精准查询组合的混合式搜索，RAG 技术并结合 reranking 模型进行重排。通过混合式搜索，进一步提高产品推荐准确度。比如：用户想要一个打游戏的台式机，这里会采用 RAG 进行模糊搜索，但如果用户想要一个明确配置的台式机，如 4080 显卡的机器，这里会采用 SQL 进行精准搜索。

![](img/008269bc117c089e8accd9dab8ea88c4.png)

![](img/079244cbf8e79e6e6e9896f089ca44a9.png)

## 推荐策略

在推荐策略上基于提示词工程，设计一套完整的提示词，通过对用户关于预算，使用场景，机箱尺寸，性能等关键指标，漏斗式逼近用户的意图，精准推荐产品。

由于不同模型对提示词的遵循能力不同，因此，我们的提示词经过了 20 余版的迭代，才调整了一版比较完善的提示词，我们认为，大语言模型的能力是基座，如果语言模型的能力足够的强，在提示词工程上的努力会小很多。

![](img/f963e9dcdfaec30f7f951066240363f5.png)

![](img/2a048b914a8bba328ff8f6503562317a.png)

## 联网搜索

我们调用 Dify 中的插件 TAVILY Search 来实现联网搜索，Tavily Search 是一款专为大型语言模型和智能体设计的智能搜索引擎，通过实时数据检索和检索增强生成技术，为 AI 应用提供精准、高效的信息支持。

![](img/dd9b9c6009345c8ea3bebbe2327c38ea.png)

## 前端UI优化

尽管 Dify 提供了 UI 界面和后端 API 接口，但由于其原生 UI 无法支持卡片式对话，本项目对前端进行了定制化优化，以提供更友好的交互体验。

![](img/d68f1f8cfeee8e8a522c548b7048fdde.png)

![](img/56db81204d4bba546a555043aed1afa9.png)

![](img/83b818d3ba6aafadd4f644e56dd58a27.png)
