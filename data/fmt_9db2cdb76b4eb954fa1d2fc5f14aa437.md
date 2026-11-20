
# OpenAI实时语音API功能介绍

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/X84ddUB5MobP9wxpEosc6hWknCh](https://kq4b3vgg5b.feishu.cn/docx/X84ddUB5MobP9wxpEosc6hWknCh)

## 实时API测试版

实时API使您能够构建低延迟、多模态的对话体验。它目前支持文本和音频作为输入和输出，还支持工具调用功能。

该API的一些显著优势包括：

+ 原生语音到语音转换：无需文本中介，从而实现低延迟、细腻的输出。
+ 自然且可调节的声音：模型具有自然的音调变化，能够笑、耳语，并遵循音调指令。
+ 同时支持多模态输出：文本有助于内容审核，比实时更快的音频确保了播放的稳定性。

实时API目前处于测试阶段，目前我们不提供客户端身份验证。因此，应用程序应设计为将音频从客户端路由到应用服务器，再从应用服务器安全地通过身份验证与OpenAI进行通信。

实时音频受网络条件影响较大，当网络状况不可预测时，要可靠地将实时音频传输到服务器（例如从移动客户端到后端）是一个挑战。

因此，如果您正在构建客户端或电话应用程序，且无法控制网络的可靠性，在生产环境中建议您考虑使用专门的第三方解决方案，如下方列出的合作伙伴集成。

## 快速入门

实时API是一个在服务器端运行的WebSocket接口。为了帮助您快速入门，我们创建了一个控制台演示应用程序，可以展示API的一些功能。

虽然我们不建议在生产环境中使用该应用中的前端模式，但该应用可以帮助您可视化和检查实时集成中的事件流程。

### 快速启动实时控制台

要快速上手，请下载并配置实时控制台演示。

若要在前端应用中使用实时API，我们建议使用下方列出的合作伙伴集成之一：

+ LiveKit集成指南

如何使用LiveKit的WebRTC基础设施与实时API集成。

+ Twilio集成指南

如何构建集成Twilio API和实时API的应用程序。

+ Agora集成快速入门

如何将Agora的实时音频通信功能与实时API集成。

## 概览

实时API是一个有状态的基于事件的API，通过WebSocket进行通信。建立WebSocket连接需要以下参数：

+ URL：`wss://api.openai.com/v1/realtime`

+ 查询参数：`?model=gpt-4o-realtime-preview-2024-10-01`

+ 请求头：

  + `Authorization: Bearer YOUR_API_KEY`

  + `OpenAI-Beta: realtime=v1`

以下是使用Node.js中的流行库`ws`来建立套接字连接、从客户端发送消息并接收服务器响应的简单示例。示例中要求系统环境中导出一个有效的`OPENAI_API_KEY`。

```
import WebSocket from "ws";

const url = "wss://api.openai.com/v1/realtime?model=gpt-4o-realtime-preview-2024-10-01";
const ws = new WebSocket(url, {
    headers: {
        "Authorization": "Bearer " + process.env.OPENAI_API_KEY,
        "OpenAI-Beta": "realtime=v1",
    },
});

ws.on("open", function open() {
    console.log("Connected to server.");
    ws.send(JSON.stringify({
        type: "response.create",
        response: {
            modalities: ["text"],
            instructions: "Please assist the user.",
        }
    }));
});

ws.on("message", function incoming(message) {
    console.log(JSON.parse(message.toString()));
});
```

您可以在API参考中找到服务器发出的所有事件及客户端可以发送的事件的完整列表。一旦连接，您将能够发送和接收表示文本、音频、函数调用、中断、配置更新等的事件。

## API参考

完整的实时API客户端和服务器事件列表，请参阅API参考文档。

## 示例

以下是一些常见的API功能示例，帮助你快速上手。这些示例假设已经建立了WebSocket连接。

### 发送用户文本

要从用户发送文本消息：

```
const event = {
  type: 'conversation.item.create',
  item: {
    type: 'message',
    role: 'user',
    content: [
      {
        type: 'input_text',
        text: 'Hello!'
      }
    ]
  }
};
ws.send(JSON.stringify(event));
ws.send(JSON.stringify({type: 'response.create'}));
```

### 发送用户音频

要发送用户音频数据：

```
import fs from 'fs';
import decodeAudio from 'audio-decode';

// 将 Float32Array 音频数据转换为 PCM16 ArrayBuffer
function floatTo16BitPCM(float32Array) {
  const buffer = new ArrayBuffer(float32Array.length * 2);
  const view = new DataView(buffer);
  let offset = 0;
  for (let i = 0; i < float32Array.length; i++, offset += 2) {
    let s = Math.max(-1, Math.min(1, float32Array[i]));
    view.setInt16(offset, s < 0 ? s * 0x8000 : s * 0x7fff, true);
  }
  return buffer;
}

// 将 Float32Array 转换为 base64 编码的 PCM16 数据
function base64EncodeAudio(float32Array) {
  const arrayBuffer = floatTo16BitPCM(float32Array);
  let binary = '';
  let bytes = new Uint8Array(arrayBuffer);
  const chunkSize = 0x8000;
  for (let i = 0; i < bytes.length; i += chunkSize) {
    let chunk = bytes.subarray(i, i + chunkSize);
    binary += String.fromCharCode.apply(null, chunk);
  }
  return btoa(binary);
}

const myAudio = fs.readFileSync('./path/to/audio.wav');
const audioBuffer = await decodeAudio(myAudio);
const channelData = audioBuffer.getChannelData(0); // 只接受单声道
const base64AudioData = base64EncodeAudio(channelData);

const event = {
  type: 'conversation.item.create',
  item: {
    type: 'message',
    role: 'user',
    content: [
      {
        type: 'input_audio',
        audio: base64AudioData
      }
    ]
  }
};
ws.send(JSON.stringify(event));
ws.send(JSON.stringify({type: 'response.create'}));
```

### 流式传输用户音频

若要通过多个音频文件分段流式传输用户音频数据：

```
import fs from 'fs';
import decodeAudio from 'audio-decode';

function floatTo16BitPCM(float32Array) {
  const buffer = new ArrayBuffer(float32Array.length * 2);
  const view = new DataView(buffer);
  let offset = 0;
  for (let i = 0; i < float32Array.length; i++, offset += 2) {
    let s = Math.max(-1, Math.min(1, float32Array[i]));
    view.setInt16(offset, s < 0 ? s * 0x8000 : s * 0x7fff, true);
  }
  return buffer;
}

function base64EncodeAudio(float32Array) {
  const arrayBuffer = floatTo16BitPCM(float32Array);
  let binary = '';
  let bytes = new Uint8Array(arrayBuffer);
  const chunkSize = 0x8000;
  for (let i = 0; i < bytes.length; i += chunkSize) {
    let chunk = bytes.subarray(i, i + chunkSize);
    binary += String.fromCharCode.apply(null, chunk);
  }
  return btoa(binary);
}

const files = [
  './path/to/sample1.wav',
  './path/to/sample2.wav',
  './path/to/sample3.wav'
];

for (const filename of files) {
  const audioFile = fs.readFileSync(filename);
  const audioBuffer = await decodeAudio(audioFile);
  const channelData = audioBuffer.getChannelData(0);
  const base64Chunk = base64EncodeAudio(channelData);
  ws.send(JSON.stringify({
    type: 'input_audio_buffer.append',
    audio: base64Chunk
  }));
}

ws.send(JSON.stringify({type: 'input_audio_buffer.commit'}));
ws.send(JSON.stringify({type: 'response.create'}));
```

## 解释

这些示例展示了如何发送用户输入的文本和音频，及如何分段发送流式音频数据：

+ 发送文本：使用`conversation.item.create`消息传递用户文本消息，然后发送`response.create`请求模型生成响应。

+ 发送音频：通过将音频文件解码为单声道的`Float32Array`，并转换为base64编码的PCM16格式，再以`input_audio`的形式发送。

+ 流式传输音频：通过循环发送多个音频文件的数据片段，使用`input_audio_buffer.append`发送音频片段，最后用`input_audio_buffer.commit`结束流并请求响应生成。

## 函数调用

客户端可以通过`session.update`消息设置默认函数，或通过`response.create`消息为每次响应设置函数，作为模型可用的工具。如果适用，服务器会以`function_call`项的形式响应。

这些函数以工具的形式传递，类似于聊天补全API的格式。当前不需要指定工具的类型，因为这是唯一支持的工具类型。

### 会话中设置工具示例

可以在会话配置中定义工具，具体如下：

```
{
  "tools": [
    {
      "name": "get_weather",
      "description": "Get the weather at a given location",
      "parameters": {
        "type": "object",
        "properties": {
          "location": {
            "type": "string",
            "description": "Location to get the weather from"
          },
          "scale": {
            "type": "string",
            "enum": ["celsius", "farenheit"]
          }
        },
        "required": ["location", "scale"]
      }
    }
  ]
}
```

服务器调用函数时，可能还会返回音频或文本内容，例如“好的，我会为您提交订单”。函数的描述字段可以引导服务器如何在特定场景中处理响应，如“尚未确认订单完成”或“在调用工具之前响应用户”。

客户端在函数调用之前需要使用`conversation.item.create`消息发送`function_call_output`类型的响应。添加函数调用输出不会自动触发另一个模型响应，因此客户端可以使用`response.create`立即触发新响应。

## 集成指南

### 音频格式

实时API当前支持两种音频格式：

1. 原始16位PCM音频，24kHz，单声道，小端字节序

2. G.711格式，8kHz（支持u-law和a-law）

音频数据需要以base64编码的方式传输音频帧片段。以下是使用Python的`pydub`库将音频文件字节转换为有效音频消息项的示例代码。对于Node.js，可以使用`audio-decode`库读取不同文件类型的原始音轨。
