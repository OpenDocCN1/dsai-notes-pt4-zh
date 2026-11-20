
# NLWeb 提供的前端用户界面（UI）组件及其基本用法。

> 来源：[https://qianniucity.feishu.cn/docx/T3kBdoo3uoqMgGx7PUjc2ZOonXe](https://qianniucity.feishu.cn/docx/T3kBdoo3uoqMgGx7PUjc2ZOonXe)

主要内容总结如下：

## UI 组件库

NLWeb 仓库包含了一套简单的 JavaScript 组件，用于快速搭建聊天界面，适合原型开发和演示。

## 核心方法

主要类为 `ChatInterface`，可通过参数设置默认 `site`、`mode` 和 `display_mode`（如 `dropdown` 或 `nlwebsearch`）。

支持会话级记忆（session memory），即在同一会话中保留上下文。

## 使用示例

提供了 `static/index.html` 的简单实现示例，展示如何用输入框、按钮和结果容器实现基本聊天交互。

通过监听按钮点击或回车事件，调用 `chat_interface.sendMessage(query)` 发送用户输入，实现对话轮次。

## 接口兼容性

UI 组件支持与 `/ask` API 端点的流式返回配合使用。

可根据 `item` 的 `@type`（如 `Recipe`）定制渲染方式，示例见 `static/recipe-renderer.js`。

## 扩展性

渲染器不仅可用于聊天界面，也可独立用于其他类型的内容展示。

整体上，NLWeb 的 UI 组件简单易用，便于开发者快速集成和定制对话式搜索体验。
