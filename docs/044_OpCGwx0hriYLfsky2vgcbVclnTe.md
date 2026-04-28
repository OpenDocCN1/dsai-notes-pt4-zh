# 快速入门

> 来源：[https://hyperspace.feishu.cn/docx/FQ5EdfdAeoDhyFx34QycH1aQnqb](https://hyperspace.feishu.cn/docx/FQ5EdfdAeoDhyFx34QycH1aQnqb)

## 一、插件接入的核心步骤

要构建插件，需要完成如下核心流程。

## 二、创建插件配置文件并准备好您的插件服务

+   插件清单文件（ai-plugin.json，必选），该文件包含插件的基础信息（名称、头像等）、身份验证信息以及OpenAPI 规范等；

+   插件服务描述文件（openapi.yaml，必选），该文件用于模型识别您提供的功能服务；

+   示例描述文件（example.yaml，可选），可在此文件中提供插件示例，以提升插件调用的正确率；

+   动作消息注册文件（msg_content.yaml，可选），该文件用于在用户场景中展示插件调用进展；

+   启动插件服务（openapi 服务, 必选），开发并启动您的服务接口，接口需符合 openapi.yaml中的定义。

## 三、在文心智能体平台创建并发布您的插件

+   创建插件：您可以通过填写域名/文件上传的方式，提交您的插件配置文件，并创建插件；

+   调试预览：您可以不断调整插件配置文件，并在平台预览插件效果；

+   发布：插件效果调试完毕后，提交审核进入发布流程；插件通过审核后，您可以选择手动/自动完成插件发布。

## 四、接下来

您已经了解了构建能力插件的核心流程，您可能需要：

+   [如何从0到1开发插件](https://agents.baidu.com/docs/develop/ability-plugin/basic/develop_from_scratch/)

+   [如何在平台创建并发布插件](https://agents.baidu.com/docs/develop/ability-plugin/basic/create_to_publish/)

+   [API接口开发注意事项](https://agents.baidu.com/docs/develop/ability-plugin/advanced/api_notes/)

+   [如何使用插件鉴权功能](https://agents.baidu.com/docs/develop/ability-plugin/advanced/auth/)