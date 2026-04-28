# 高德、腾讯、百度地图布局 MCP Server 对比分析

> 来源：[https://qianniucity.feishu.cn/docx/S1BhdiNyHooyEoxw8pLcKNaynPc](https://qianniucity.feishu.cn/docx/S1BhdiNyHooyEoxw8pLcKNaynPc)

高德、腾讯、百度地图，三家地图服务商都已经布局MCP Server，为AI大模型与地图服务的结合提供了便捷的接口。以下是根据三家公司公开文档，给出的功能对比，开发者可以根据自身需求、技术架构和已有的地图服务集成经验选择适合的 MCP Server。

## MCP协议概述

MCP(Model Context Protocol)协议是为了解决AI大模型与外部工具交互的统一规范，使得一个工具无需额外适配即可在不同AI大模型中运行，大大降低了开发成本，促进了AI的落地应用。

## 三家地图服务商 MCP Server 对比

1.  覆盖接口数量

1.  主要功能对比

1.  特色与亮点

### 高德地图

+   覆盖12大核心接口，提供全场景覆盖

### 腾讯位置服务

+   基于MCP(SSE)方式，不必部署本地服务，简单配置即可

+   云端化服务，持续迭代改进，用户无需额外操作

+   对JSON结果进行了语义化转换，更易于大模型理解

+   提供了高级功能，如沿途搜索、途经点智能排序、未来路线规划等

### 百度地图

+   国内首家兼容MCP协议的地图服务商

+   强调简单配置即可在大模型中快速接入地图服务

+   支持IPv6定位（需申请高级权限）

## SSE模式的优势

腾讯位置服务支持的SSE(Server-Sent Events)模式具有以下优势：

1.  **实时数据推送**：SSE允许服务器向客户端推送实时更新，非常适合地图服务中的实时位置、交通状况等数据推送

1.  **轻量级协议**：相比WebSocket，SSE更轻量，只需HTTP连接，实现简单

1.  **自动重连机制**：SSE客户端具有内置的重连功能，提高了服务的稳定性

1.  **无需部署本地服务**：如腾讯文档所述，基于MCP(SSE)方式，开发者不必部署本地服务，简单配置即可使用

1.  **与AI大模型交互更流畅**：SSE的单向数据流模式适合AI大模型获取地图服务的数据，减少了交互复杂度

## 一句话总结

腾讯位置服务在技术实现(SSE模式)和功能丰富度方面具有明显优势，特别是对于需要简化部署和实时数据的应用场景。百度地图强调了先发优势，而高德地图则提供了全面的核心功能覆盖。

## 相关地址：

+   [https://lbs.amap.com/api/mcp-s](https://lbs.amap.com/api/mcp-server/gettingstarted)erver/gettingstarted

+   [https://lbs.baidu.com/faq/api?](https://lbs.baidu.com/faq/api?title=mcpserver/base)title=mcpserver/base

+   [h](https://lbs.qq.com/service/MCPServer/MCPServerGuide/overview)ttps://lbs.qq.com/service/MCPServer/MCPServerGuide/overview