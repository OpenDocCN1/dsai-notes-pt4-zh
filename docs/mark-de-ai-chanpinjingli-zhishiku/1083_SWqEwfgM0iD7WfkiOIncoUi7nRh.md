# React Native MCP代理

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/VeRXdKEb0oMwF3xtuKUczDNbnCc](https://qqs7y1hozd1.feishu.cn/docx/VeRXdKEb0oMwF3xtuKUczDNbnCc)

This project is an AI agent designed to interact with React Native applications using the Model Context Protocol (MCP). It enables advanced automation, reasoning, and integration capabilities for mobile app workflows.

## Features

+   MCP server for agent communication

+   Native integration with React Native apps

+   Extensible agent logic in Python

+   Example usage and quickstart scripts

## Getting Started

### Prerequisites

+   Python 3.8+

+   React Native development environment

+   Node.js and npm

### Installation

1.  Add the React Native MCP agent to your project:

1.  npm install @mcp/react-native-agent

1.  No external dependencies required for core MCP server

### Usage

1.  Import the agent in your React Native application:

1.  import { MCPAgent } from '@mcp/react-native-agent';

1.  Initialize the agent:

1.  const agent = new MCPAgent();await agent.initialize();

1.  Execute commands:

1.  const result = await agent.execute('analyze-screen');

+   For native integration, see native.py for example functions and usage.

## File Structure

+   mcp_server.py — MCP server implementation

+   native.py — Native React Native integration logic

## Customization

+   Extend native.py for additional React Native features.

#

# React Native MCP代理

该项目是一个AI智能体，旨在使用模型上下文协议（MCP）与React Native应用程序进行交互。它为移动应用工作流程提供了高级自动化、推理和集成能力。

## 特性

+   用于代理通信的MCP服务器

+   与React Native应用的原生集成

+   Python 中的可扩展代理逻辑

+   示例用法和快速入门脚本

## 入门指南

### 先决条件

+   Python 3.8+

+   React Native开发环境

+   Node.js和npm

### 安装

1.  将 React Native MCP 代理添加到您的项目中：

1.  npm install @mcp/react-native-agent

1.  核心MCP服务器无需外部依赖

### 用法

1.  在你的React Native应用程序中导入代理：

1.  import { MCPAgent } from '@mcp/react-native-agent';

1.  初始化智能体：

1.  const agent = new MCPAgent();await agent.initialize();

1.  执行命令：

1.  const result = await agent.execute('analyze-screen');

+   对于原生集成，请参阅native.py以获取示例函数和用法。

## 文件结构

+   mcp_server.py — MCP服务器实现

+   native.py — 原生 React Native 集成逻辑

## 定制化

+   扩展native.py以支持额外的React Native功能。