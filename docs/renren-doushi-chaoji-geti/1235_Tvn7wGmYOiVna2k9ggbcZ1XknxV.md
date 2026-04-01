# 2023-05-29零代码，使用 Dify 两分钟接入企业微信 AI 机器人

> 来源：[https://xuqiwei1986.feishu.cn/docx/XZ2fdjRPlo61ncxVyahc5u3nnYc](https://xuqiwei1986.feishu.cn/docx/XZ2fdjRPlo61ncxVyahc5u3nnYc)

> 🔗 原文链接： h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/n7nKgULwRCT4-oEC3-yqrA)q.com/s/n7nKgULw...

>

yuhao Dify

2023-05-29 09:09:17

精确发文时间由壹伴提供

2023-05-29 09:09 发表于 广东

手机阅读

![](img/9866a1f4bf6f5e244b3c70f5390aede3.png)

**【本文由** Dify 深度用户 @李昱昊 投稿整理所得。基于 Dify 能力进行 AI 应用的创建，产出本篇最佳实践文章。我们欢迎更多优秀开发者向我们投稿最佳实践，投稿联系邮箱：hello@dify.ai,或加入 Discord 社群（https://discord.com/invite/FngNHpbcY7）联系官方工作人员。】

以下为内容正文，将首先展示 AI 机器人接入企业微信后的效果，然后一步步讲解实操演示。

---

Dify 允许创建AI应用，并提供二次开发的能力。这里我将演示创建一个法律问答助手的 AI 应用（机器人），称作“知法”。在本篇教程中，我将指导你为“知法”接入企业微信。

**效果展示**

退出全屏

切换到横屏模式

[🎦 点击观看视频](http://mpvideo.qpic.cn/0b2esuabiaaakyanlvdrpnsfbfodcskqafaa.f10002.mp4?dis_k=43848bbd80d6468e9d42fc4179ed8ac5&dis_t=1691145317&play_scene=10120&auth_info=IOaFgvsKEnhP+vPg431AMSZ3Ngg3SxJuAX49b0ZJNjASNw==&auth_key=ad81bfba1f806f46220527425a0cb018&vid=wxv_2945819094077980683&format_id=10002&support_redirect=0&mmversion=false)

> 无法获取音频链接

视频详情

输入/new 新建对话

退出全屏

切换到横屏模式

[🎦 点击观看视频](http://mpvideo.qpic.cn/0bc3fyaegaaaeeaia33rhfsfalwdimxaaqya.f10002.mp4?dis_k=0f5cb038782efab125ff8d670d5c0bbd&dis_t=1691145317&play_scene=10120&auth_info=cvXk9IJZQSoYoKfrsi8UbyF1MA9qGxVmAn9saxNEYWpAZg==&auth_key=4dc02bae0c83a69cafaa7c5dd67f28ad&vid=wxv_2945824660506591233&format_id=10002&support_redirect=0&mmversion=false)

> 无法获取音频链接

视频详情

实现自动分段输出（伪流式响应）

**前置准备**

+   企业微信的管理员权限；

+   一个 Dify 的帐号（https://dify.ai/）；

+   一个 Laf 云的帐号（https://laf.run/）；

+   （可选）一个 OpenAI 的 API Key。如果没有，可以使用 Dify 免费提供的 200 次调用机会用于测试；

+   （可选）在电脑上新建一个 env.txt 的文件，将下面内容复制到 env.txt 中**。在接下来的**教程中，我们会一步步把相关的信息填入这个文件。需要保存信息的步骤会 高亮显示 。

+

+

+

+

+

+

```
WXWORK_TOKEN=&quot;&quot;WXWORK_AESKEY=&quot;&quot;WXWORK_CORPID=&quot;&quot;WXWORK_AGENTID=&quot;&quot;WXWORK_CORPSECRET=&quot;&quot;DIFY_APPTOKEN=&quot;&quot;
```

**在 Dify 上创建应用**

这一章节将会介绍如何创建一个法律知识的数据集，并将数据集和应用关联起来。

## **搭建法律知识数据集**

为了让“知法”了解到更多的上下文，我们需要创建一个法律知识的数据库。

**1. 导入文档： **从电脑上导入法律知识的 PDF 文档。

![](img/50689e9aa3fe3954c1a628ed125ef91d.png)

**2. 文本分段和清洗 **：上传的文本需要经过二次加工，才能被大语言模型理解。这里我们不需要关注具体的实现逻辑，直接选择自动分段即可，然后点击“保存并处理”。

![](img/4d6e3098d90a51e59a471175ada1fddd.png)

**3. 文本嵌入： **大约 30s 时间，数据集就创建成功了。你可以随时回来向数据库里添加更多文件。

![](img/16a942e073bc2645d4a05e35174f8b92.png)

（查看 Dify 官方文档中关于搭建数据集的更多操作： https://docs.dify.ai/v/zh-hans/advanced/datasets）

**搭建应用**

**1. 创建应用： **根据图中的指示，创建一个对话型应用，并命名为“知法”。

![](img/29c0e9861ed1fffa676bc46d1a8217f2.png)

**2. 关联数据集： **在“提示词编排”页，在“上下文”模块中添加选择刚刚创建的数据集。

![](img/2203c4583ab32083010ff7f7a186b5b4.png)

3. **发布模型： **完成关联数据集后，点击页面右上角的“发布”，使模型生效。

![](img/51436e2e80fc3be17737242738f4872f.png)

4. 获取 API 访问密**钥。 在“访问 API”页****面**， 创建一个 API 密钥并复制保存为 DIFY_APPTOKEN 。请注意不要把密钥泄漏给任何人，以免造成财产损失。

![](img/9234d5067d65e8654d8b18fc14aa9ef9.png)

（查看 Dify 官方文档中关于创建应用的更多操作： https://docs.dify.ai/v/zh-hans/application/creating-an-application）

**创建企业微信应用**

**1. 记录企业信息： ****进入企业微信管理后台-我的****企业， 记录这里的企业 ID**** **为 WXWORK_CORPID。

![](img/cf624812eb3ed464353b4e25e906a894.png)

**2. 创建企业微信应用： **进入应用管理页面，点击【创建应用】进入创建页面，填写应用信息后点击【创建应用】。如果已经有现成的应用，可以跳过此步骤。

![](img/b1a4567e0e57a1614544f985fd86a840.png)

![](img/aec5623a746bff032043baa4b243843b.png)

**3. 记录企业微信应用信息： **在应用管理页面点击刚刚创建好的应用，进入应用详情页面。记录这里的 Ag**entId 和 Secret（需要点击**获取按钮，在企业微信聊天窗口里面获取）， 分别为 WXWORK_AGENTID 和 WXWORK_CORPSECRET。

![](img/eba4ea27852d746f545752b09ee8ad88.png)

**4. 企业微信应用接收信息： **在应用详情页面，接收消息处点击【设置 API 接收】。

![](img/1cea37bd958479cbc5e8450203194972.png)

在** API 接收消息页面，点一下两个**【随机获取】按钮，它会自动生成一个 Token 和 EncodingAESKey， 我们分别记为 WXWORK_TOKEN 和 WXWORK_AESKEY。  注意，不要关掉这个页面，Laf 侧配置完毕后我们再来填写 URL。

![](img/371902d8992cbf0122399c4e075389dd.png)

**在 Laf 云上创建云函数**

**1. 新建 Laf 云应用： **进入 Laf 后，点击新建，创建一个云应用。这里选择免费的计划即可。

![](img/c29091eb886a4fab9f1f05a4db214aa0.png)

**2. 添加依赖： **企业微信应用需要添加 @wecom/crypto , xml2js 两个依赖。添加好后，你的依赖列表应该像下面一样。

![](img/fb1e7f77b9062d0922095edb222000b6.png)

**3. 添加环境变量： **从第二行开始，将上面步骤中收集到的所有内容全部粘贴到这里，点击更新。

![](img/a39a5dde08aa13176ab733411bc8ccca.png)

**4. 创建云函数： **点击创建一个云函数，注意“请求方法”中勾选上 POST , GET ，点击确定。

![](img/a4b38c7a21b449300b9f64d52e758167.png)

**在创建好云**函数中，删除默认的代码，并将本文下方 “附录” 中的代码全部粘贴到这里。

![](img/c2e1ff24cc1abc4d1f5dd061c25623c7.png)

**5. 点发布云函数： **点击发布后，云函数就生效了。

![](img/9fdf95a1f12a2d0f8b6bbb1933872b8b.png)

现在把 URL 粘贴到企业微信后台【设置 API 接收】的页面中刚刚留白的地方，然后点击保存。

![](img/a06541e70e8d420a13a1bcdd58d48954.png)

**5. 配置 IP 白名单： **在企业微信中找到刚刚创建应用，发送一句消息。不出意外收不到任何消息。这是因为企业微信默认屏蔽了 Laf 云的 IP。 点击日志，应当能看到这样一条报错' not allow to access from your ip '

![](img/f1c1d1814e39db9611dfffe034d5d8cd.png)

点击查看这条日志详情，记录日志中给出的 Laf 云 IP：

![](img/e6b74b0447b463216d8b02ed4cf79fa0.png)

回到企业微信的管理后台，点击刚刚创建的应用，为应用配置可行 IP：

![](img/1dc434f80ef750311f85d53d01875c5e.png)

在这里把刚刚的日志中记录的 IP 填入即可：

![](img/3be8610cab7f7e556f1087a4e4a09707.png)

**验证效果**

**1. 测试聊天： **在企业微信中找到刚刚创建应用，发送一句消息。现在应当能收到推送的消息了。

![](img/795d556e10ef93a568dcec245b25f620.png)

##

**附录**

## 企业微信应用代码 - (伪流式响应)

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

```
import cloud from &#x27;@lafjs/cloud&#x27;import { decrypt, getSignature } from &#x27;@wecom/crypto&#x27;import xml2js from &#x27;xml2js&#x27;
function genConversationKey(userName) {  return `${process.env.WXWORK_AGENTID}:${userName}`}
function genWxAppAccessTokenKey() {  return `${process.env.WXWORK_AGENTID}:access-token`}
async function getToken() {  console.log(&#x27;[getToken] called&#x27;)
  const cache = cloud.shared.get(genWxAppAccessTokenKey())  if (cache &amp;&amp; cache.expires &gt;= Date.now()) return cache.token
  const res = await cloud.fetch({    url: &#x27;https://qyapi.weixin.qq.com/cgi-bin/gettoken&#x27;,    method: &#x27;GET&#x27;,    params: {      corpid: process.env.WXWORK_CORPID,      corpsecret: process.env.WXWORK_CORPSECRET,    }  })
  const token = res.data.access_token  cloud.shared.set(genWxAppAccessTokenKey(), { token, expires: Date.now() + res.data.expires_in * 1000 })  return token}
async function sendWxMessage(message, user) {  console.log(&#x27;[sendWxMessage] called&#x27;, user, message)
  const res = await cloud.fetch({    url: &#x27;https://qyapi.weixin.qq.com/cgi-bin/message/send&#x27;,    method: &#x27;POST&#x27;,    params: {      access_token: await getToken()    },    data: {      &quot;touser&quot;: user,      &quot;msgtype&quot;: &quot;text&quot;,      &quot;agentid&quot;: process.env.WXWORK_AGENTID,      &quot;text&quot;: {        &quot;content&quot;: message      },      &quot;safe&quot;: 0,      &quot;enable_id_trans&quot;: 0,      &quot;enable_duplicate_check&quot;: 0,      &quot;duplicate_check_interval&quot;: 1800    },  })  console.log(&#x27;[sendWxMessage] received&#x27;, res.data)}
async function sendDifyMessage(message, userName, onMessage) {  console.log(&#x27;[sendDifyMessage] called&#x27;, message, userName)
  const conversationId = cloud.shared.get(genConversationKey(userName)) || null  let newConversationId = &#x27;&#x27;  let responseText = &#x27;&#x27;
  try {    const response = await cloud.fetch({      url: &#x27;https://api.dify.ai/v1/chat-messages&#x27;,      method: &#x27;POST&#x27;,      headers: {        &#x27;Authorization&#x27;: `Bearer ${process.env.DIFY_APPTOKEN}`      },      data: {        inputs: {},        response_mode: &quot;streaming&quot;,        query: message,        user: userName,        conversation_id: conversationId      },      responseType: &quot;stream&quot;    })
    let firstHalfMessage = &#x27;&#x27;    response.data.on(&#x27;data&#x27;, (data) =&gt; {      let message = data.toString()      try {        if (firstHalfMessage) {          message += firstHalfMessage          firstHalfMessage = &#x27;&#x27;        }
        // 检查是不是sse协议        if (!message.startsWith(&#x27;data: &#x27;)) return
        const parsedChunk: Record&lt;string, any&gt; = JSON.parse(message.substring(6))
        if (!newConversationId) {          newConversationId = parsedChunk.conversation_id          cloud.shared.set(genConversationKey(userName), newConversationId)        }        const { answer } = parsedChunk        responseText += answer
        // 伪流式响应        if (answer.endsWith(&#x27;\n\n&#x27;) || (responseText.length &gt; 120 &amp;&amp; /[?。；！]$/.test(responseText))) {          onMessage(responseText.replace(&#x27;\n\n&#x27;, &#x27;&#x27;))          console.log(&#x27;[sendDifyMessage] received&#x27;, responseText, newConversationId)          responseText = &#x27;&#x27;        }      } catch (e) {        firstHalfMessage = message        console.error(&#x27;[sendDifyMessage] error&#x27;, message)      }
    })
    // stream结束时把剩下的消息全部发出去    response.data.on(&#x27;end&#x27;, () =&gt; {      onMessage(responseText.replace(&#x27;\n\n&#x27;, &#x27;&#x27;))    })  } catch (e) {    console.error(&quot;[sendDifyMessage] error&quot;, e)  }}
async function asyncSendMessage(xml) {  console.log(&#x27;[asyncSendMessage] called&#x27;, xml)
  if (xml.MsgType[0] !== &#x27;text&#x27;) return
  const message = xml.Content[0]  const userName = xml.FromUserName[0]
  if (message === &#x27;/new&#x27;) {    // 重置conversation id    cloud.shared.set(genConversationKey(userName), null)    sendWxMessage(&#x27;新建成功，开始新的对话吧~~&#x27;, userName)    return  }
  sendWxMessage(&#x27;AI思考中, 请耐心等待~~&#x27;, userName)
  try {    sendDifyMessage(message, userName, (message) =&gt; {      sendWxMessage(message, userName)    })  }  catch (e) {    console.error(&#x27;[sendDifyMessage] error&#x27;, e)    sendWxMessage(&#x27;接口请求失败，请联系管理员查看错误信息&#x27;, userName)  }}
export default async function (ctx: FunctionContext) {  const { query } = ctx  const { msg_signature, timestamp, nonce, echostr } = query  const token = process.env.WXWORK_TOKEN  const key = process.env.WXWORK_AESKEY  console.log(&#x27;[main] called&#x27;, ctx.method, ctx.request.url)
  // 签名验证专用  if (ctx.method === &#x27;GET&#x27;) {    const signature = getSignature(token, timestamp, nonce, echostr)    if (signature !== msg_signature) {      return { message: &#x27;签名验证失败&#x27;, code: 401 }    }    const { message } = decrypt(key, echostr)    return message  }
  const payload = ctx.body.xml  const encrypt = payload.encrypt[0]  const signature = getSignature(token, timestamp, nonce, encrypt)  if (signature !== msg_signature) {    return { message: &#x27;签名验证失败&#x27;, code: 401 }  }
  const { message } = decrypt(key, encrypt)  const {    xml  } = await xml2js.parseStringPromise(message)  // 由于GPT API耗时较久，这里提前返回，防止企业微信超时重试，后续再手动调用发消息接口  ctx.response.sendStatus(200)
  await asyncSendMessage(xml)
  return { message: true, code: 0 }}
```

引用： 这篇深度参考以下文章，感谢原作者的辛勤付出：https://forum.laf.run/d/556/2

（完）

---

**关于 Dify：**

**Dify.**AI 是一款 LLMOps 平台，帮助开发者更简单、更快速地构建 AI 应用。它的核心理念是通过可声明式的 YAML 文件定义 AI 应用的各个方面，包括 Prompt、上下文和插件等。Dify 提供了可视化的 Prompt 编排、运营、数据集管理等功能。这些功能使得开发者能够在数天内完成 AI 应用的开发，或将 LLM 快速集成到现有应用中，并进行持续运营和改进，创造一个真正有价值的 AI 应用。

![](img/095febe800a292615ec9a2a82047ef64.png)

![](img/b797497741e8bb1df8e6f6afe6aa4b80.png)