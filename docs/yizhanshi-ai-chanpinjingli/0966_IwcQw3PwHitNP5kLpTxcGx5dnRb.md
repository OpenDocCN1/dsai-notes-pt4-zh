
# 为自定义参数赋值

> 来源：[https://v11enp9ok1h.feishu.cn/docx/COOodSiuNoPIyRxIcUXc6NPPnHe](https://v11enp9ok1h.feishu.cn/docx/COOodSiuNoPIyRxIcUXc6NPPnHe)

本文介绍如何在对话流中添加自定义参数，并在对话中为自定义参数赋值。

## 功能简介

在智能体对话流中添加自定义参数，常用于在对话中灵活存储和使用关键信息，以便提供个性化服务、传递上下文信息。例如，你可以在自定义参数中设置用户偏好、用户信息、地理位置、订单信息等，当用户与智能体对话时，可以传入对应的自定义参数的值，以便智能体根据这些参数提供更精准的服务，如根据用户偏好推荐内容、根据订单信息查询物流状态等，从而提升用户体验和对话效率。

## 步骤一：在对话流中设置自定义参数

1. 在对话流编排页面，在开始节点的输入参数中创建自定义参数，并在输出节点中引用该自定义参数。本文以 `user_name` 为例，你可以根据实际业务场景设置其他参数。

![](img/56268a44e285e808d56384cf28cede7a.png)

![](img/6037af9b356b6b57c38b3ccd92b8c2ad.png)

1. 将智能体设置为单 Agent（对话流模式），在智能体中添加对话流，发布智能体。

## 步骤二：给自定义参数赋值

你可以根据实际的业务场景，在用户与智能体对话时动态更新和读取自定义参数的值，支持通过如下方式给自定义参数赋值。

### WebSocket 语音通话

> 仅支持给发布为 API 的单 Agent（对话流模式）的智能体设置自定义参数。

1. [建立 WebSo](https://www.coze.cn/open/docs/developer_guides/streaming_chat_api)cket 连接，详细示例代码请参见双向流式语音对话。

1. 发送 `chat.update` 事件，在事件的 `chat_config.parameters` 参数中指定自定义参数的名称和值，数据类型为 `Map[String, Any]` 格式的键值对集合。WebSocket 会将该自定义参数的值传给对话流。`parameters` 中的参数名称为对话流中设置的自定义参数，本示例中为 `user_name`。

```json
{  
    "id": "event_id",  
    "event_type": "chat.update",  
    "data": {  
        "chat_config": { 
            "parameters": { 
                "user_name": "John" 
            } 
        } 
    } 
} 
```

### RTC 音视频通话

> 仅支持给发布为 API 的单 Agent（对话流模式）的智能体设置自定义参数。

进入 RTC 房间后，发送[更新房间配置](https://www.coze.cn/open/docs/developer_guides/signaling_uplink_event#bb460fb4)上行事件，在事件的 `data.chat_config.parameters` 参数中指定自定义参数的名称和值，数据类型为 `Map[String, Any]` 格式的键值对集合。在后续对话中，Realtime SDK 会将该自定义参数的值传给对话流。

`parameters` 中的参数名称为对话流中设置的自定义参数，本示例中为 `user_name`。

```json
{ 
    "id": "7446668538246561828", 
    "event_type": "session.update", 
    "data": { 
        "chat_config": { 
            "parameters": { 
                "user_name": "John";
            } 
        }
    } 
} 
```

### 发起对话 OpenAPI

在调用[发起对话](https://www.coze.cn/open/docs/developer_guides/chat_v3) OpenAPI 时，在 `parameters` 参数中指定自定义参数的名称和值，数据类型为 `Map[String, Any]` 格式的键值对集合。智能体会将自定义参数的值传递给对话流。

`parameters` 中的参数名称为对话流中设置的自定义参数，本示例中为 `user_name`。

```bash
curl --location --request POST 'https://api.coze.cn/v3/chat?conversation_id=7374752000116113452' \
     --header 'Authorization: Bearer pat_OYDacMzM3WyOWV3Dtj2bHRMymzxP****' \
     --header 'Content-Type: application/json' \
     --data-raw '{
    "bot_id": "734829333445931****",
    "user_id": "123456789",
    "stream": true,
    "auto_save_history": true,
    "parameters": {
           "user_name": "John"
    },
    "additional_messages": [
        {
            "role": "user",
            "content": "2024年10月1日是星期几",
            "content_type": "text"
        }
    ]
}'
```

### 执行对话流

在调用[执行对话流](https://www.coze.cn/open/docs/developer_guides/workflow_chat) OpenAPI 时，在 `parameters` 参数中指定自定义参数的名称和值，数据类型为 `Map[String, Any]` 格式的键值对集合。`parameters` 中的参数名称为对话流中设置的自定义参数，本示例中为 `user_name`。

```bash
curl -X POST 'https://api.coze.cn/v1/workflows/chat' \
-H "Authorization: Bearer pat_OYDacMzM3WyOWV3Dtj2bHRMymzxP****" \
-H "Content-Type: application/json" \
-d '{
  "parameters": {
    "user_name": "John"
  },
  "workflow_id": "你的 workflow id",
  "bot_id": "734829333445931****",
  "additional_messages": [
    {
      "content": "2024年10月1日是星期几",
      "content_type": "text",
      "role": "user",
      "type": "question"
    }
  ]
}'
```

### ChatSDK

> 仅支持给发布为 `ChatSDK` 的扣子应用或单 Agent（对话流模式）的智能体设置自定义参数。

当扣子应用或智能体中绑定了对话流后，调用 `ChatSDK` 中的 `CozeWebSDK.WebChatClient` 创建聊天对话框时，在 `config` 中指定自定义参数的名称和值，数据类型为 `Map[String, Any]` 格式的键值对集合。`ChatSDK` 会将自定义参数的值传递给对话流。`ChatSDK` 的具体使用方法可参考安装并使用 Chat SDK。

`parameters` 中的参数名称为对话流中设置的自定义参数，本示例中为 `user_name`。

+ 扣子应用：

```javascript
new CozeWebSDK.WebChatClient({
  config: {
      type: 'app',
      appInfo: {
        appId: '744189632066042****',
        workflowId: '744229754050396****',
        parameters: {
          user_name: 'John';
        }
      }
  },
  auth:  {
    type: 'token',    
    token: 'pat_zxzSAzxawer234zASNElEglZxcmWJ5ouCcq12gsAAsqJGALlq7hcOqMcPFV3wEVDiqjrg****',
    onRefreshToken: () => 'pat_zxzSAzxawer234zASNElEglZxcmWJ5ouCcq12gsAAsqJGALlq7hcOqMcPFV3wEVDiqjrg****',
  }
});
```

+ 扣子智能体：

```javascript
const cozeWebSDK = new CozeWebSDK.WebChatClient({ 
  config: { 
    // 智能体 ID 
    botId: '740849137970326****', 
    isIframe: false, 
    // 给自定义参数赋值并传给对话流 
    botInfo: { 
      parameters: { 
        user_name: 'John' 
      } 
    } 
  }, 
  auth:  { 
      // Authentication methods, the default type is 'unauth', which means no authentication is required; it is recommended to set it to 'token', indicating authentication through PAT (Personal Access Token) or OAuth 
    type: 'token',     
     // When the type is set to 'token', it is necessary to configure a PAT (Personal Access Token) or OAuth access token for authentication. 
    token: 'pat_zxzSAzxawer234zASNElEglZxcmWJ5ouCcq12gsAAsqJGALlq7hcOqMcPFV3wEVDiqjrg****', 
    // When the access token expires, use a new token and set it as needed. 
    onRefreshToken: () => 'pat_zxzSAzxawer234zASNElEglZxcmWJ5ouCcq12gsAAsqJGALlq7hcOqMcPFV3wEVDiqjrg****', 
  } 
}); 
```
