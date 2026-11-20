
# 触发器

> 来源：[https://v11enp9ok1h.feishu.cn/docx/B9NtdOOaZo4oyKxaeXxcMGEznBg](https://v11enp9ok1h.feishu.cn/docx/B9NtdOOaZo4oyKxaeXxcMGEznBg)

您可以为 Bot 设置触发器（`Triggers`），使 Bot 在特定时间或接收到特定事件时自动执行任务。

Bot 可配置的触发器类型如下，您可以根据实际需要选择适用的触发器。

+   定时触发（`Scheduled trigger`）：让 Bot 在指定时间执行任务，无需编写任何代码。

+   事件触发（`Event trigger`）：触发器会生成 `Webhook URL`，当您的服务端向触发器 `Webhook URL` 发送 `HTTPS` 请求时，触发任务执行。

触发器在触发时，执行任务的方式如下：

+   Bot 提示词（`Bot prompt`）：该方式下，您需要通过自然语言设置提示词。后续触发时，提示词会自动发送给 Bot，Bot 根据提示词向用户发送提醒消息。例如，为一个生活提醒机器人设置该触发器，提示词为每周一早上 7:00 提醒用户做有氧运动。

+   调用插件（`Plugin`）：该方式下，您需要为触发器添加一个插件。后续触发时，Bot 会调用该插件获取返回结果并发送给用户。例如，您可以添加一个查询天气的插件，当触发后向用户发送指定地点的天气信息。

+   调用工作流（`Workflow`）：该方式下，您需要为触发器添加一个工作流。如果工作流有输入参数，则需要传入参数值。后续触发时，Bot 会调用该工作流获取返回结果并发送给用户。例如，您可以添加一个审批工作流，当触发后执行工作流完成业务审批。

此外，Coze 支持用户在与 Bot 聊天时设置定时任务。如下所示，当用户在会话内点击推荐任务后，Bot 将会确认并创建定时任务。

![](img/a948a685696f33319dd5b04479addadf.png)

## 使用限制

+   一个 Bot 内的触发器最多可添加 10 个。

+   触发器仅当 Bot 发布飞书时生效。

## 添加定时触发器

1.  登录[扣子](https://www.coze.cn/home)。

1.  在左侧导航栏的**团队空间**区域，选择进入指定团队。

1.  在 **Bots** 页面，选择进入指定 Bot。

1.  在 Bot 编排页面**触发器**区域，单击右上角 **+** 图标。

![](img/96ce7514654813d047704762abb14c6a.png)

1.  在**创建触发器**对话框，完成以下配置，并单击**保存**。

![](img/25d704d5ef0fe7798030bdf046462a16.png)

+   **名称**：触发器名称。

+   **触发器类型**：选择**定时触发**。

+   **触发时间**：设置定时触发器的时区以及触发时间。

+   **任务执行**：设置触发后执行任务的方式。支持选择：

+   **机器人提示**：该方式需要通过自然语言设置提示词。

+   **插件**或**工作流**：这两种方式需要您单击右侧 **+** 图标，添加插件或工作流（仅可添加一个）。如果插件或工作流有输入参数，则需要设置参数值。

## 添加 Webhook 类型的事件触发器

1.  登录[扣子](https://www.coze.cn/home)。

1.  在左侧导航栏的**团队空间**区域，选择进入指定团队。

1.  在 **Bots** 页面，选择进入指定 Bot。

1.  在 Bot 编排页面的**触发器**区域，单击右上角 **+** 图标。

![](img/208e3e8aba41ac46d693980f6c7e827f.png)

1.  在**创建触发器**对话框，完成以下配置，并单击**保存**。

![](img/5fbe31f52737db020a9f436a910e5213.png)

+   **名称**：触发器名称。

+   **触发器类型**：选择**事件触发**。

+   **模式**：目前仅支持 **Webhook** 模式。在该模式下，您将获取到触发器的 `Webhook URL`，通过向 `Webhook URL` 发送 `HTTPS` 请求，可触发该触发器。

+   **Bearer Token**：请求校验令牌。您可以直接使用默认提供的 `Token`，也可以修改 `Token` 值。向 `Webhook URL` 发送 `HTTPS` 请求时，请求头必须包含该 `Token`，用于完成请求的安全校验。

+   **请求参数**：请求参数列表，单击右侧 **+** 图标即可添加参数。该参数列表为可选配置，用于关联触发器中插件或者工作流的请求参数，后续向 `Webhook URL` 发送请求时，需要以 `JSON` 格式传入参数值。

+   **任务执行**：设置触发后执行任务的方式。支持选择：

+   **机器人提示**：该方式需要通过自然语言设置提示词。

+   **插件**或**工作流**：这两种方式需要您单击右侧 **+** 图标，添加插件或工作流（仅可添加一个）。如果插件或工作流有输入参数，则需要设置参数值。参数值可以在触发器内直接设置；也可以关联 `Webhook` 的请求参数列表，后续在发送 `HTTPS` 请求时传入参数值。

![](img/e313b94296e0b3147f178a4423d165e3.png)

1.  （可选）触发事件。

+   **在开发调试**阶段，您可**以在** Bot 编排页面的预览与调试区域，单击任务，运行某一事件触发器，进行调试。

![](img/5d05ab13e68d9f0cf4e71974d659cc81.png)

+   当 Bot 发布后，则需要向触发器的 `Webhook URL` 发送 `HTTPS POST` 请求，触发任务执行。

以 `cURL` 构成的 `HTTPS` 请求为例，格式如下：

```
curl --location --request POST '<Trigger Webhook URL>' \
--header 'Authorization: Bearer <Trigger Bearer Token>' \
--header 'Content-Type: application/json' \
--data '<Trigger Parameters>'
```

+   `curl`：命令行工具，支持通过 `HTTP`、`HTTPS`、`FTP` 等多种协议发送请求或接收数据。

+   ``--request POST '<Trigger Webhook URL>'``：定义当前请求为 `HTTPS POST` 请求，其中 `<Trigger Webhook URL>` 为占位符，您需要替换为触发器真实的 `Webhook` 地址（可在 Bot 的事件触发器详情页复制 `URL`）。

+   ``--header 'Authorization: Bearer <Trigger Bearer Token>'``：请求头参数，通过 `Authorization` 完成请求校验来确保安全性，其中 `<Trigger Bearer Token>` 为占位符，您需要替换为触发器真实的 `Bearer Token`。

+   ``--header 'Content-Type: application/json'``：固定取值，用于定义消息体类型为 `JSON`。

+   ``--data '<Trigger Parameters>'``：`HTTPS POST` 请求包含的数据内容。如果触发器内的插件或工作流需要输入参数，则需要将 `<Trigger Parameters>` 占位符替换为 `JSON` 格式的请求参数体。

示例配置：

```
curl --location 'https://api.xxxx/api/xxxx' \
--header 'Authorization: Bearer ABCxxxxx' \
--header 'Content-Type: application/json' \
--data '{
    "url": "www.example.com"
}'
```

发送请求后，响应结果包含的 `BaseResp` 中的 `StatusCode` 为 0 表示请求成功。如果 `StatusCode` 不为 0，您可以通过 `HttpCallBackRespDatas` 获取错误信息，并根据错误信息作出相应调整。

## 允许用户与 Bot 聊天时设置定时任务

1.  在 Bot 编排页面的**触发器**区域，选中允许用户在与 Bot 对话时创建定时任务复选框。

1.  单击显示出来的**在开场白中添加引导**。

1.  如有需要，修改**开场白**中添加的定时任务，然后在**预览与调试**内单击此问题进行调试。

+   必须配置**开场白文案**，使**开场白预置问题**生效。

+   **开场白预置问题**配置超过 3 条时，系统将随机展示其中 3 条。

![](img/64c308877375b7e204c3a91941fdf354.png)
