
# 用Llama Edge运行DeepSeek-R1蒸馏模型

> 来源：[https://now.feishu.cn/docx/GbwMd788Qok1wOxhW4icXDjknng](https://now.feishu.cn/docx/GbwMd788Qok1wOxhW4icXDjknng)

## 背景介绍

DeepSeek-R1 使用了强化学习技术，在仅有极少标注数据的情况下，极大提升了模型推理能力。在数学、代码、自然语言推理等任务上，性能比肩 OpenAI o1 正式版。从 DeepSeek-R1 蒸馏得到的小模型有效继承了大模型学到的推理模式。

本文主要使用Llama Edge测试 `DeepSeek-R1-Distill-Llama-8B-GGUF`，欢迎跟着我试验下最新的模型：

## 环境准备

首先，使用 `https://github.com/LlamaEdge/LlamaEdge`

按照官方 `README.md` 进行操作，首先下载，复制以下命令到命令行运行试试：

```
curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install_v2.sh | bash
```

我是window电脑，所以碰到了报错，把报错进行截图后，发给Gemini2，让它帮我解释下。

![](img/7fc7c58e77664841ec38915d4924a3bc.png)

这个命令在类 Unix 系统（例如 Linux、macOS）上通常可以工作。但我使用的是window的power shell，同时，由于网络问题，我们还是手动下载下3个文件。

先新建一个目录`edge`（你也可以修改为你自己的名称）

`https://github.com/WasmEdge/WasmEdge/releases/download/0.14.1/WasmEdge-0.14.1-windows-msvc.msi`（下载后运行安装）

`ggml` 后端允许 WasmEdge 应用执行 Llama 2 系列大模型的推理。我用的是`cuda`这个版本：

`https://github.com/WasmEdge/WasmEdge/releases/download/0.14.1/WasmEdge-plugin-wasi_nn-ggml-cuda-0.14.1-windows_x86_64.zip`

`ggml` 插件一定要安装，否则会提示`nn-preload`无效。可以从 WasmEdge 发布页面下载插件，解压然后复制到 `C:\Program Files\WasmEdge\lib` 目录下。注意解压后，只需要取`lib`目录下的文件即可。

![](img/92f5b402cf1f39bf72fd6e4f1ca063e9.png)

### 目录结构

然后，需要设置下系统的环境变量：

![](img/09535f28374b00e1fb089597da7400c6.png)

```
WASMEDGE_PLUGIN_PATH
```

参考github上的一个issue：`https://github.com/WasmEdge/WasmEdge/issues/3920`

`https://huggingface.co/second-state/DeepSeek-R1-Distill-Llama-8B-GGUF/tree/main`（下载一个合适大小的模型，由于是测试，我选择了最小3GB那个，也是放到`edge`目录下）

`https://github.com/LlamaEdge/LlamaEdge/releases/download/0.16.1/llama-api-server.wasm` 下载后放到`edge`目录下

现在`edge`目录结构如下：

![](img/7ce6e766782dc57eaeb04fd7971d30ba.png)

## 启动服务

使用以下命令行为模型启动 LlamaEdge API 服务器

```
wasmedge --dir .:. --nn-preload default:GGML:AUTO:DeepSeek-R1-Distill-Llama-8B-Q2_K.gguf llama-api-server.wasm --prompt-template llama-3-chat --ctx-size 8096
```

如果一切正常，我们就可以通过api发送请求得到结果。

我让Gemini写了个js代码：

```
curl -X POST http://localhost:8080/v1/chat/completions \
  -H 'accept:application/json' \
  -H 'Content-Type: application/json' \
  -d '{"messages":[{"role":"system","content":"你非常擅长思考、推理和总结。请使用中文回复"}, {"role":"user","content":"中国最著名的神话人物是谁，为什么，有什么依据?"}], "model": "DeepSeek-R1-Distill-Llama-8B"}'
```

这是一个chat的api。帮我改成stream流式的`fetch`请求，写一个容易看懂的js方法，把ai对话的结果console出来，用黄色底，黑色字。

另外，还需要提供一个中断请求的方法，为了让我可以随时中断请求。

```
async function getStreamCompletion(controller,messages, model, onDataReceived, onError, onComplete) {
  const signal = controller.signal;
  try {
    const response = await fetch('http://localhost:8080/v1/chat/completions', {
      method: 'POST',
      headers: {
        'Accept': 'application/json', // 或 'text/event-stream'，取决于服务器实现
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ messages, model, stream: true }),
      signal
    });

    if (!response.ok) {
      const errorText = await response.text();
      throw new Error(`HTTP error ${response.status}: ${errorText}`);
    }

    if (!response.body) {
      throw new Error("No response body received.");
    }

    const reader = response.body.getReader();
    const decoder = new TextDecoder();
    let partialLine = ""; // 用于处理跨块的行
    while (true) {
      const { done, value } = await reader.read();
      if (done) {
        if (onComplete) onComplete();
        break; // 使用 break 退出循环
      }

      const chunk = decoder.decode(value);
      const lines = (partialLine + chunk).split('\n');
      partialLine = lines.pop() || ""; // 保存不完整的行

      for (const line of lines) {
        if (line.trim() === "") continue; // 跳过空行

        if (line === "data: [DONE]") { // 更严格的比较
            if (onComplete) onComplete();
            break; // 使用 break 退出循环
        }

        if (line.startsWith("data: ")) {
          try {
            const data = JSON.parse(line.substring(6));
            if (data.choices && data.choices[0].delta && data.choices[0].delta.content) {
              const content = data.choices[0].delta.content;
              
              if (onDataReceived) onDataReceived(content); // 立即传递数据
            }
          } catch (parseError) {
            console.error("Error parsing stream data:", parseError, line); // 打印出错的行
            if (onError) onError(parseError);
            break;
          }
        } else if (line.startsWith("error: ")) {
          try {
              const error = JSON.parse(line.substring(7));
              throw new Error(`Stream error: ${error.message}`);
          } catch (errorParseError) {
              console.error("Error parsing error message:", errorParseError, line);
              throw new Error(`Stream error: Could not parse error from server: ${line}`);
          }
        } else {
          console.warn("Unexpected stream data:", line); // 打印未知的行
        }
      }
    }
  } catch (error) {
    if (error.name === 'AbortError') {
      console.log("Request aborted.");
    } else {
      console.error("Fetch error:", error);
      if (onError) onError(error);
    }
  }

  return controller;
}

const messages = [
  { role: 'system', content: '你非常擅长思考、推理和总结。请使用中文回复' },
  { role: 'user', content: '中国最著名的神话人物是谁，为什么，有什么依据?' }
];
const model = 'DeepSeek-R1-Distill-Llama-8B';
const controller = new AbortController();
let result="";

getStreamCompletion(controller,messages, model, (content) => {
    //console.log("Received data:", content);
    result+=content
    console.log(`%c${result}`, 'background-color: yellow; color: black;');

}, (error) => {
    console.error("An error occurred:", error);
}, () => {
    console.log("Stream completed.");
});

// 中断
setTimeout(() => {
    controller.abort(); 
}, 8000);
```

把这个js代码粘贴到chrome里的开发者工具console面板里，你应该就能看到正常的回复结果了。

另外，也可以把api地址填写到mixcopilot里，进行使用。

## 延伸阅读

最后，这些信息可以进一步了解：

- `wasmedge` 是干嘛的？
- `llamaedge` 又是干嘛的？
- `gguf` 是什么？
