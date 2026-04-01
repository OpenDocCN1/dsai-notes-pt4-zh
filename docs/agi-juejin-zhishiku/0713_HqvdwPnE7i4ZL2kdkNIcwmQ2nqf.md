# 「豆包Marscode体验官」：我的私人小说画师，一码在手，插图我有

> 来源：[https://agijuejin.feishu.cn/docx/QA1idVBLwofxRrxRyUkcsFetnZe](https://agijuejin.feishu.cn/docx/QA1idVBLwofxRrxRyUkcsFetnZe)

> 作者：睡个好jo

> 链接：https://juejin.cn/post/7388338139632746547

> 来源：稀土掘金

> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

最近，字节跳动的人工智能编程助手工具豆包MarsCode横空出世，当我在看到这则消息的时候，作为一名热衷于技术与艺术融合的创作者，我有幸成为豆包MarsCode的体验官，并使用它做了一个小说插图生成的demo，你给它一段小说片段，它会根据你的片段生成一张图片。

## 那么首先我们先来看看这款编程助手的功能

我是在vscode里下载的一个插件，直接搜索marscode就可以了

让我们来看看它的功能

1.  智能代码补全

MarsCode的核心功能之一是智能代码补全。它利用深度学习技术分析代码上下文，预测并推荐代码片段，极大地减少了程序员的打字量，加速了代码编写过程。

![](img/1d285b59559e540cd682c276f321db43.png)

尽管它刚开始可能不太能理解我的意思，，但稍微调教调教就行了

1.  代码生成

除了补全已有代码，MarsCode还具备强大的代码生成能力。无论是函数、类还是完整的文件，只需简单的指令，MarsCode即可自动生成。

![](img/509917b23ddb3a5315fd3aaa99d96d71.png)

这个代码生成还是挺准确的

1.  注释与解释

面对复杂的代码结构，MarsCode能够自动生成清晰的注释，可以帮助开发者更好地理解代码逻辑。只需要在你想要生成注释的代码上边或者后面打上注释的符号，它就会自动给你生成注释，如下图：

![](img/9b951a995030bd4314740e7a0ed9f3a0.png)

1.  单测生成

为了确保代码质量，MarsCode还提供了自动化的单元测试生成功能。在编码阶段即可进行初步的代码验证，及时发现并修正潜在的错误，大大提升了代码的健壮性，减少了后期调试的时间成本。

![](img/f23a32c1d5d9998a641413a016b8b5e8.png)

1.  缺陷修复

MarsCode能够智能识别代码中的潜在错误和不规范编程实践，并给出修改建议。这一功能如同一位经验丰富的同事，在你身旁随时提供指导，帮助你避免常见的编程陷阱，提升代码的整体质量。

1.  AI问答

内置的AI问答系统使得MarsCode成为了开发者的朋友和顾问。无论是遇到复杂的技术难题还是简单的概念疑问，MarsCode都能提供即时、准确的答案，促进了知识的快速共享和问题的高效解决。

1.  广泛兼容性与多语言支持

MarsCode的兼容性极强，无论你是Windows、macOS还是Linux用户，无论你偏爱Visual Studio Code还是JetBrains系列IDE，MarsCode都能完美适配。更重要的是，它支持上百种编程语言，几乎涵盖了所有开发场景，满足了不同领域、不同层次开发者的需求。

## 项目的构想与计划

在开始之前，我首先明确了目标：创建一个能够根据小说文本自动生成插图的系统。你给它一段小说片段，它会根据你的片段生成一张图片。豆包MarsCode作为我的助手，在这个项目中扮演了至关重要的角色，从代码编写到逻辑优化，它都提供了极大的帮助。

## 前端界面设计

我选择使用HTML和JavaScript来构建前端界面。下面是我设计的简单对话框界面代码，它允许用户输入文本，然后调用后端API生成相应的插图。

```
&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;zh-CN&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&gt;
    &lt;title&gt;对话框示例&lt;/title&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;./personal.css&quot;&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;div class=&quot;bot-container&quot;&gt;
    &lt;div class=&quot;chatbox&quot; id=&quot;messages&quot;&gt;
        &lt;!-- Messages will be added here dynamically --&gt;
    &lt;/div&gt;
    &lt;div class=&quot;input-button-group&quot;&gt;
        &lt;textarea name=&quot;inputText&quot; id=&quot;bot-input&quot; rows=&quot;3&quot; placeholder=&quot;在这里输入文字...&quot; required&gt;&lt;/textarea&gt;
        &lt;button id=&quot;btn&quot;&gt;发送&lt;/button&gt;
    &lt;/div&gt;
&lt;/div&gt;
&lt;script&gt;
    const inp = document.getElementById(&#x27;bot-input&#x27;);
    const btn = document.getElementById(&#x27;btn&#x27;);
    const messagesDiv = document.getElementById(&#x27;messages&#x27;);

    btn.addEventListener(&#x27;click&#x27;, function() {
        if (inp.value.trim() === &#x27;&#x27;) {
            alert(&#x27;请输入一些文本！&#x27;);
            return;
        }

        // 显示用户的消息
        const userMessageDiv = document.createElement(&#x27;div&#x27;);
        userMessageDiv.className = &#x27;message user-message&#x27;;
        userMessageDiv.textContent = inp.value;
        messagesDiv.appendChild(userMessageDiv);

        // 清空输入框
        inp.value = &#x27;&#x27;;

        const data = {
            text: inp.value
        };

        fetch(&quot;http://127.0.0.1:3000/img&quot;, {
            method: &#x27;POST&#x27;,
            headers: {
                &#x27;Content-Type&#x27;: &#x27;application/json;charset=UTF-8&#x27;
            },
            body: JSON.stringify(data)
        })
        .then(response =&gt; response.json())
        .then(data =&gt; {
            if (data &amp;&amp; data.url) {
                // 显示助手的消息（图像）
                const assistantMessageDiv = document.createElement(&#x27;div&#x27;);
                assistantMessageDiv.className = &#x27;message assistant-message&#x27;;
                assistantMessageDiv.innerHTML = `&lt;img src=&quot;${data.url}&quot; alt=&quot;Generated Image&quot;&gt;`;
                messagesDiv.appendChild(assistantMessageDiv);
            } else {
                alert(&#x27;无法获取图片，请重试！&#x27;);
            }
        })
        .catch(error =&gt; {
            console.error(&#x27;Error:&#x27;, error);
            alert(&#x27;请求失败，请检查您的网络连接或服务器状态！&#x27;);
        });
    });
&lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
```

这段代码构建了一个简洁的用户界面，用户可以通过点击“发送”按钮提交小说片段，系统则会异步请求后端API，获取并展示生成的插图。

## 后端API搭建

后端部分，我选择了Koa.js作为服务器框架，配合OpenAI的DALL·E模型来生成插图。以下是后端代码的核心部分：

```
import Koa from &#x27;koa&#x27;
import Router from &#x27;koa-router&#x27;
import cors from &#x27;@koa/cors&#x27;
import { bodyParser } from &#x27;@koa/bodyparser&#x27;
import OpenAI from &quot;openai&quot;;

const client = new OpenAI({
    apiKey: &#x27;YOUR_API_KEY&#x27;,
    baseURL: &#x27;https://api.302.ai/v1&#x27;
})

const app = new Koa()
const router = new Router()

app.use(bodyParser())
app.use(cors())

router.post(&#x27;/img&#x27;, async (ctx) =&gt; {
    try {
        let { text } = ctx.request.body
        console.log(text);
        const prompt = `你是一个小说插图师，请你为小说片段&quot;${text}&quot;设计插图，符合小说的内容，主题，风格，情感，场景，人物。`;
        const response = await client.images.generate({
            model: &#x27;dall-e-3&#x27;,
            prompt,
            n: 1
        })
        ctx.body = {
            status: 200,
            url: response.data[0].url
        }
        console.log(response.data[0].url);
    } catch (error) {
        ctx.body = {
            status: 500,
            msg: &#x27;服务器错误&#x27;
        }
    }
})

app.use(router.routes())

app.listen(3000, () =&gt; {
    console.log(&#x27;server is running at http://localhost:3000&#x27;);
})
```

这段代码定义了一个API端点/img，当接收到POST请求时，它会读取请求体中的文本，构造生成插图的提示，然后调用DALL·E模型生成一张插图。生成的插图URL会被返回给前端，用于显示。

### 成果

敲代码途中，marscode的提示：

![](img/adb63a2a6f679ad053bbaa95af152cf9.png)

marscode帮我检查代码：

![](img/db54bac3adddadd9cec016c18708e77f.png)

还有许多，我就不一一放出来了

#### 项目成果

![](img/fee1b9a889ac8efb2dcb7beced5fa1e9.png)

现在的文生图还是不太行

## 测试与优化

在完成前后端的搭建后，我进行了多轮测试，确保系统稳定可靠。豆包MarsCode在这一过程中发挥了重要作用，它不仅帮助我调试代码，还提供了性能优化的建议，比如增加错误处理机制，确保用户体验不受影响。

## 结论

通过这次体验，我深刻感受到豆包MarsCode作为AI助手的强大功能。它不仅简化了开发过程，还提高了开发效率。但是，它带给我的体验对比同类型的产品仍有些不足之处，希望marscode能够越来越好，能给我带来不一样的惊喜