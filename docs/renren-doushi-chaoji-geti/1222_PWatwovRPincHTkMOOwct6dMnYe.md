# 2023-08-30零代码，使用 Dify 和 Laf 两分钟接入企业微信 AI 机器人

> 来源：[https://xuqiwei1986.feishu.cn/docx/BkWbd2iIDoUB9gxwbirclq4Dnqb](https://xuqiwei1986.feishu.cn/docx/BkWbd2iIDoUB9gxwbirclq4Dnqb)

> 🔗 原文链接： h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/pc1P-4hkL3zl38LhoCDyXw)q.com/s/pc1P-4hk...

>

Dify Laf 开发者 2023-08-30 17:52 发表于 日本

收录于合集

#Dify 2 个

#Laf 23 个

#Serverless 20 个

#云开发 20 个

#企业微信 2 个

Laf 公众号已接入了 GPT4，完全免费！欢迎前来调戏👇

![](img/5058ab8849af57feb2a94619c8d5fe58.png)

本文来自 Dify，开源的 LLMOps 平台，快速创建可视化，可运营的 AI 应用，释放大语言模型的全部潜力。

Dify 允许创建 AI 应用，并提供二次开发的能力。这里我将演示创建一个法律问答助手的 AI 应用，称作“知法”。在本篇教程中，我将指导你为“知法”接入企业微信。

## **效果展示**

退出全屏

切换到横屏模式

[🎦 点击观看视频](http://mpvideo.qpic.cn/0bc3pyaaeaaavyapzc7g3vsfa7wdaj7aaaqa.f10002.mp4?dis_k=80e6ffd1825e730d356c8f524a3f1dae&dis_t=1693447416&play_scene=10120&auth_info=e8/I759SK1Fr593kpFAtbEM1N0pqMDMSYmRKMkoIdjBJAw==&auth_key=249b9b1e93abb8bb0e72d279796aba61&vid=wxv_3074724164460871682&format_id=10002&support_redirect=0&mmversion=false)

> 无法获取音频链接

视频详情

输入/new 新建对话

退出全屏

切换到横屏模式

[🎦 点击观看视频](http://mpvideo.qpic.cn/0bc3yaaaeaaa4aap2o7g3vsfbqgdalaaaaqa.f10002.mp4?dis_k=8a4fbd280e0e46242e90759a214c5023&dis_t=1693447416&play_scene=10120&auth_info=ffWhv8wAeAdrtoy0oFJ7ZUtiMkppNTFCYzRFYUVbemNPUw==&auth_key=5c6aadfb6f33933029e1e239a2af1e8f&vid=wxv_3074725511369326595&format_id=10002&support_redirect=0&mmversion=false)

> 无法获取音频链接

视频详情

实现自动分段输出（伪流式响应）

## **前置准备**

+   企业微信的管理员权限

+   一个 **Dify [1] **的帐号

+   一个 **Laf [2] **云的帐号

+   （可选）一个 OpenAI 的 API Key。如果没有，可以使用 Dify 免费提供的 200 次调用机会用于测试。

+   （可选）在电脑上新建一个 env.txt 的文件，将下面内容复制到 env.txt 中。在接下来的教程中，我们会一步步把相关的信息填入这个文件。需要保存信息的步骤会高亮显示。

```
WXWORK_TOKEN=&quot;&quot;
WXWORK_AESKEY=&quot;&quot;
WXWORK_CORPID=&quot;&quot;
WXWORK_AGENTID=&quot;&quot;
WXWORK_CORPSECRET=&quot;&quot;
DIFY_APPTOKEN=&quot;&quot;
```

## **在 Dify 上制作应用**

这一章节将会介绍如何创建一个法律知识的数据集，并将数据集和应用关联起来。

### ** 搭建法律知识数据集**

> ❝

> **随时查看文档中关于搭建**数据集的更多操作： 【数据集管理】 [3]

为了让“知法”了解到更多的上下文，我们需要创建一个法律知识的数据库。

+   **导入文档： **从电脑上导入法律知识的 PDF 文档。

![](img/3c0cac4cfb6c3d5b215ad832a574d2d9.png)

+   **文本分段和清洗 **：上传的文本需要经过二次加工，才能被大语言模型理解。这里我们不需要关注具体的实现逻辑，直接选择自动分段即可，然后点击“保存并处理”。

![](img/57962ba9f72bc9c8ccd32f4302cdbf98.png)

+   **文本嵌入： **大约 30s 时间，数据集就创建成功了。你可以随时回来向数据库里添加更多文件。

![](img/746f090db10d13c86be0e8a0733e2842.png)

### ** 搭建的应用**

> ❝

> **随时查看文档中关于创**建应用的更多操作 【创建应用】 [4]

+   **创建应用： **根据图中的指示，创建一个对话型应用，并命名为“知法”。

![](img/de088f23b57c73f648bc0440a001af4d.png)

+   **关联数据集： **在“提示词编排”页，在“上下文”模块中添加选择刚刚创建的数据集。

![](img/67c9251073c383bf754b9f634606bb8c.png)

+   **发布模型： **完成关联数据集后，点击页面右上角的“发布”，使模型生效。

![](img/6dac4dae383bc5a36c49115a58c68bab.png)

+   **获取 API 访问密钥。 **在“访问 API”页面，创建一个 API 密钥并复制保存为 DIFY_APPTOKEN 。请注意不要把密钥泄漏给任何人，以免造成财产损失。

![](img/cc8cd4f6ff51ff8cc97157513aea1ccb.png)

## **创建企业微信应用**

+   **记录企业信息： **进入企业微信管理后台-我的企业，记录这里的企业 ID 为 WXWORK_CORPID

![](img/45ed58ce6b7f834f96fcc9e66ee9f5d6.png)

+   **创建企业微信应用： **进入应用管理页面，点击【创建应用】进入创建页面，填写应用信息后点击【创建应用】。如果已经有现成的应用，可以跳过此步骤。

![](img/a811b4734ad42a5993218421ae52fd4b.png)

![](img/e093d576c34a94e8f899521b10596cc7.png)

+   **记录企业微信应用信息： **在应用管理页面点击刚刚创建好的应用，进入应用详情页面。记录这里的 AgentId 和 Secret（需要点击获取按钮，在企业微信聊天窗口里面获取），分别为WXWORK_AGENTID和WXWORK_CORPSECRET。

![](img/6fe33fbcd883ee986e8769d1a3fb036b.png)

+   **企业微信应用接收信息： **在应用详情页面，接收消息处点击【设置 API 接收】。

![](img/eb6f4984b7d9c404b787005183bdef7d.png)

在 API 接收消息页面，点一下两个【随机获取】按钮，它会自动生成一个 Token 和 EncodingAESKey，我们分别记为 WXWORK_TOKEN 和 WXWORK_AESKEY。注意，不要关掉这个页面，Laf 侧配置完毕后我们再来填写 URL。

![](img/3e813436a094f1fc79aac541f594602b.png)

## **在 Laf 云上创建云函数**

+   **新建 Laf 云应用： **进入 Laf 后，点击新建，创建一个云应用。这里选择免费的计划即可。

![](img/1a8a9c29811ceb1b840a6bb928d620c2.png)

+   **添加依赖： **企业微信应用需要添加 @wecom/crypto , xml2js 两个依赖。添加好后，你的依赖列表应该像下面一样。

![](img/ab7c3ed81f8d083e853fafeca89b14ed.png)

+   **添加环境变量： **从第二行开始，将上面步骤中收集到的所有内容全部粘贴到这里，点击更新。

![](img/e7e18c3f576d1bd246c3169c2bebe3ed.png)

+   **创建云函数： **点击创建一个云函数，注意“请求方法”中勾选上 POST , GET ，点击确定。

![](img/a569fa90cda51528f7d38df2242a0d68.png)

**在创建好云**函数中，删除默认的代码，并将文末 “附录” 中的代码全部粘贴到这里。

![](img/b5317b3d028443d914b503abdc7df465.png)

+   **发布云函数： **点击发布后，云函数就生效了。

![](img/12adea89aabd8360a934c0c82134ee9e.png)

现在把 URL 粘贴到企业微信后台【设置 API 接收】的页面中刚刚留白的地方，然后点击保存。

![](img/ae79041926780869ebc37c1ce8019fcd.png)

+   **配置 IP 白名单： **在企业微信中找到刚刚创建应用，发送一句消息。不出意外收不到任何消息。这是因为企业微信默认屏蔽了 Laf 云的 IP。

点击日志，应当能看到这样一条报错 ' not allow to access from your ip '

![](img/1061a4f7e67471c77a47af4ac60274c9.png)

点击查看这条日志详情，记录日志中给出的 Laf 云 IP。

![](img/8ad134e0bb0c2fc1905fb50bb0b877cc.png)

回到企业微信的管理后台，点击刚刚创建的应用，为应用配置可行 IP。

![](img/c2f11285130ed9981662ab5ee3167b9f.png)

在这里把刚刚的日志中记录的 IP 填入即可。

![](img/a034ebd12f66f6309327b573c28add95.png)

## **验证效果**

1.  **测试聊天： **在企业微信中找到刚刚创建应用，发送一句消息。现在应当能收到推送的消息了。

![](img/f0c6a7cec0bc0eac9caebaf22f6bbce8.png)

## **引用**

这篇深度参考以下文章，感谢原作者的辛勤付出。 https://forum.laf.run/d/556/3 [5]

## **附录**

###  企业微信应用代码 - (伪流式响应)

```
import cloud from &#x27;@lafjs/cloud&#x27;
import { decrypt, getSignature } from &#x27;@wecom/crypto&#x27;
import xml2js from &#x27;xml2js&#x27;

function genConversationKey(userName) {
  return `${process.env.WXWORK_AGENTID}:${userName}`
}

function genWxAppAccessTokenKey() {
  return `${process.env.WXWORK_AGENTID}:access-token`
}

async function getToken() {
  console.log(&#x27;[getToken] called&#x27;)

  const cache = cloud.shared.get(genWxAppAccessTokenKey())
  if (cache &amp;&amp; cache.expires &gt;= Date.now()) return cache.token

  const res = await cloud.fetch({
    url: &#x27;https://qyapi.weixin.qq.com/cgi-bin/gettoken&#x27;,
    method: &#x27;GET&#x27;,
    params: {
      corpid: process.env.WXWORK_CORPID,
      corpsecret: process.env.WXWORK_CORPSECRET,
    }
  })

  const token = res.data.access_token
  cloud.shared.set(genWxAppAccessTokenKey(), { token, expires: Date.now() + res.data.expires_in * 1000 })
  return token
}

async function sendWxMessage(message, user) {
  console.log(&#x27;[sendWxMessage] called&#x27;, user, message)

  const res = await cloud.fetch({
    url: &#x27;https://qyapi.weixin.qq.com/cgi-bin/message/send&#x27;,
    method: &#x27;POST&#x27;,
    params: {
      access_token: await getToken()
    },
    data: {
      &quot;touser&quot;: user,
      &quot;msgtype&quot;: &quot;text&quot;,
      &quot;agentid&quot;: process.env.WXWORK_AGENTID,
      &quot;text&quot;: {
        &quot;content&quot;: message
      },
      &quot;safe&quot;: 0,
      &quot;enable_id_trans&quot;: 0,
      &quot;enable_duplicate_check&quot;: 0,
      &quot;duplicate_check_interval&quot;: 1800
    },
  })
  console.log(&#x27;[sendWxMessage] received&#x27;, res.data)
}

async function sendDifyMessage(message, userName, onMessage) {
  console.log(&#x27;[sendDifyMessage] called&#x27;, message, userName)

  const conversationId = cloud.shared.get(genConversationKey(userName)) || null
  let newConversationId = &#x27;&#x27;
  let responseText = &#x27;&#x27;

  try {
    const response = await cloud.fetch({
      url: &#x27;https://api.dify.ai/v1/chat-messages&#x27;,
      method: &#x27;POST&#x27;,
      headers: {
        &#x27;Authorization&#x27;: `Bearer ${process.env.DIFY_APPTOKEN}`
      },
      data: {
        inputs: {},
        response_mode: &quot;streaming&quot;,
        query: message,
        user: userName,
        conversation_id: conversationId
      },
      responseType: &quot;stream&quot;
    })

    let firstHalfMessage = &#x27;&#x27;
    response.data.on(&#x27;data&#x27;, (data) =&gt; {
      let message = data.toString()
      try {
        if (firstHalfMessage) {
          message += firstHalfMessage
          firstHalfMessage = &#x27;&#x27;
        }

        // 检查是不是sse协议
        if (!message.startsWith(&#x27;data: &#x27;)) return

        const parsedChunk: Record&lt;string, any&gt; = JSON.parse(message.substring(6))

        if (!newConversationId) {
          newConversationId = parsedChunk.conversation_id
          cloud.shared.set(genConversationKey(userName), newConversationId)
        }
        const { answer } = parsedChunk
        responseText += answer

        // 伪流式响应
        if (answer.endsWith(&#x27;\n\n&#x27;) || (responseText.length &gt; 120 &amp;&amp; /[?。；！]$/.test(responseText))) {
          onMessage(responseText.replace(&#x27;\n\n&#x27;, &#x27;&#x27;))
          console.log(&#x27;[sendDifyMessage] received&#x27;, responseText, newConversationId)
          responseText = &#x27;&#x27;
        }
      } catch (e) {
        firstHalfMessage = message
        console.error(&#x27;[sendDifyMessage] error&#x27;, message)
      }

    })

    // stream结束时把剩下的消息全部发出去
    response.data.on(&#x27;end&#x27;, () =&gt; {
      onMessage(responseText.replace(&#x27;\n\n&#x27;, &#x27;&#x27;))
    })
  } catch (e) {
    console.error(&quot;[sendDifyMessage] error&quot;, e)
  }
}

async function asyncSendMessage(xml) {
  console.log(&#x27;[asyncSendMessage] called&#x27;, xml)

  if (xml.MsgType[0] !== &#x27;text&#x27;) return

  const message = xml.Content[0]
  const userName = xml.FromUserName[0]

  if (message === &#x27;/new&#x27;) {
    // 重置conversation id
    cloud.shared.set(genConversationKey(userName), null)
    sendWxMessage(&#x27;新建成功，开始新的对话吧~~&#x27;, userName)
    return
  }

  sendWxMessage(&#x27;AI思考中, 请耐心等待~~&#x27;, userName)

  try {
    sendDifyMessage(message, userName, (message) =&gt; {
      sendWxMessage(message, userName)
    })
  }
  catch (e) {
    console.error(&#x27;[sendDifyMessage] error&#x27;, e)
    sendWxMessage(&#x27;接口请求失败，请联系管理员查看错误信息&#x27;, userName)
  }
}

export default async function (ctx: FunctionContext) {
  const { query } = ctx
  const { msg_signature, timestamp, nonce, echostr } = query
  const token = process.env.WXWORK_TOKEN
  const key = process.env.WXWORK_AESKEY
  console.log(&#x27;[main] called&#x27;, ctx.method, ctx.request.url)

  // 签名验证专用
  if (ctx.method === &#x27;GET&#x27;) {
    const signature = getSignature(token, timestamp, nonce, echostr)
    if (signature !== msg_signature) {
      return { message: &#x27;签名验证失败&#x27;, code: 401 }
    }
    const { message } = decrypt(key, echostr)
    return message
  }

  const payload = ctx.body.xml
  const encrypt = payload.encrypt[0]
  const signature = getSignature(token, timestamp, nonce, encrypt)
  if (signature !== msg_signature) {
    return { message: &#x27;签名验证失败&#x27;, code: 401 }
  }

  const { message } = decrypt(key, encrypt)
  const {
    xml
  } = await xml2js.parseStringPromise(message)
  // 由于GPT API耗时较久，这里提前返回，防止企业微信超时重试，后续再手动调用发消息接口
  ctx.response.sendStatus(200)

  await asyncSendMessage(xml)

  return { message: true, code: 0 }
}
```

### **引用链接**

[1]

Dify: https://dify.ai/

[2]

Laf: https://laf.run/

[3]

【数据集管理】: https://docs.dify.ai/v/zh-hans/advanced/datasets

[4]

【创建应用】: https://docs.dify.ai/v/zh-hans/application/creating-an-application

[5]

https://forum.laf.run/d/556/3: https://forum.laf.run/d/556/3

### **关于 Laf**

**Laf 是一款为所有开发者打造的集函数、数据库、存储为一体的云开发平台，助**你像写博客一样写代码，随时随地发布上线应用！3 分钟上线 ChatGPT 应用！

🌟GitHub： https://github.com/labring/laf

🏠官网（国内）： h**ttps://laf.run**

🌎官网（海外）： h**ttps://laf.dev**

💻开发者论坛： https://forum.laf.run

关注 Laf 公众号与我们一同成长👇👇👇

![](img/8fb9cc18c3c611e35683c80c0a0c5f66.png)