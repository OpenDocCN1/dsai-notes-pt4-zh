
# 语音输入的UI-初稿

> 来源：[https://now.feishu.cn/docx/FBkedQG9eoQZw6xDbH6cNp13nYb](https://now.feishu.cn/docx/FBkedQG9eoQZw6xDbH6cNp13nYb)

`https://v0.dev/chat/react-audio-recorder-xoWx53pZgtD`

`https://bolt.new/~/sb1-2mqpzuca`

智谱的实时api，暂时还没调通：

```
**cons****t WebSoc**ket = req**uire(**&#x27;ws&#x27;);
**
cons**t options = {
  he**aders: {
    **&#x27;Authorization&#x27;: &#x27;xxx&#x27;
  }
};

const ws = new WebSocket(&#x27;wss://open.bigmodel.cn/a**pi/paas/v4/real**time&#x27;, options);

ws.on(&#x27;open&#x27;, funct**ion open() {
 ** console.log(&#x27;连接已建立&#x27;);
});

ws.on(&#x27;message&#x27;, function incoming(data) {
  console.log(&#x27;收到服务器消息:&#x27;, data.toString());
});

ws.on(&#x27;error&#x27;, function error(err) {
  console.error(&#x27;WebSocket错误:&#x27;, err);
});

ws.on(&#x27;close&#x27;, function close() {
  console.log(&#x27;连接已关闭&#x27;);
});

// ... existing code ...

// 发送语音数据
function sendAudioData(audioBase64) {
    const message = {
      type: &quot;input_audio_buffer.append&quot;,
      audio: audioBase64,
      client_timestamp: Date.now()
    };
    ws.send(JSON.stringify(message));
  }
  
  // 提交语音缓冲区
  function commitAudioBuffer() {
    const message = {
      type: &quot;input_audio_buffer.commit&quot;,
      client_timestamp: Date.now()
    };
    ws.send(JSON.stringify(message));
  }
  
  // 发送文本消息
  function sendTextMessage(text) {
    const message = {
      type: &quot;conversation.item.create&quot;,
      item: {
        type: &quot;message&quot;,
        content: [{
          type: &quot;text&quot;,
          text: text
        }]
      },
      client_timestamp: Date.now()
    };
    ws.send(JSON.stringify(message));
  }
  
  // 创建响应(触发模型推理)
  function createResponse() {
    const message = {
      type: &quot;response.create&quot;,
      client_timestamp: Date.now()
    };
    ws.send(JSON.stringify(message));
  }
```

找到个开源的参考：

`https://github.com/pipecat-ai/pipecat-client-web-transports/tree/main`
