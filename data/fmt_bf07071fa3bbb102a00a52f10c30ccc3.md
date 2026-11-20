
# 2023-05-19利用FastGPT和Laf将GPT接入企业🛰️ - Sealos 开发者社区

> 来源：[https://xuqiwei1986.feishu.cn/docx/CaaBd4JbYoGfmYxNQjic1gW9n1e](https://xuqiwei1986.feishu.cn/docx/CaaBd4JbYoGfmYxNQjic1gW9n1e)

> 🔗 原文链接： https://forum.laf.run/d/556

FastGPT一个快速使用 openai api 的平台。支持一键构建AI知识库，支持多用户、多模型管理。

这篇文章教你用两分半的时间，利用FastGPT和Laf将GPT接入企业🛰️ 😂

## 条件

1. 你需要有企业🛰️管理权限
2. 你需要一个 [FastGPT ](https://fastgpt.run/)账号
3. 你需要一个 [Laf（国区） ](https://laf.run/)账号

## FastGPT操作

1. 前往“AI助手”页面，创建一个AI助手，并记录这里的`modelId`（用`FASTGPT_MODEL_ID`表示）

![](img/0b2284bf0c7736c215a1bb3822064fbf.png)

2. 前往“开发”页面，创建一个API KEY，记录这里的API KEY 为 `FASTGPT_APIKEY`

![](img/e16fdd38c7984eb235f4a847782ec701.png)

## 企业🛰️操作

1. 进入企业🛰️管理后台-我的企业，记录这里的企业ID为 `WXWORK_CORPID`

![](img/1432e470155f918c7168b97890c255b7.png)

2. 进入应用管理页面，点击【创建应用】进入创建页面，填写应用信息后点击【创建应用】。如果已经有现成的应用，可以跳过此步骤。

![](img/4a85b8dda7abe9247acbd84437c4f1ba.png)

![](img/822d8b6b4e653fc448e2a3c8686d6898.png)

3. 在应用管理页面点击刚刚创建好的应用，进入应用详情页面。记录这里的`AgentId`和`Secret`（需要点击获取按钮，在企业🛰️聊天窗口里面获取），分别为`WXWORK_AGENTID`和`WXWORK_CORPSECRET`。

![](img/81ee8b850633af9b97bdd78d7dd7070c.png)

4. 在应用详情页面，接收消息处点击【设置API接收】。

![](img/0af4c6e3dcd1f0561006ecff9904d46f.png)

在API接收消息页面，点一下两个【随机获取】按钮，它会自动生成一个`Token`和`EncodingAESKey`，我们分别记为`WXWORK_TOKEN`和`WXWORK_AESKEY`。注意，不要关掉这个页面，Laf侧配置完毕后我们再来填写URL。

![](img/85e667273ae6962efa17078c944a39e9.png)

## Laf 操作

1. 进入 [Laf控制台 ](https://laf.run/dashboard)，新建一个应用。

![](img/248fa24ba2e2b5bd322721169beb0e16.png)

2. 进入开发页面，先安装两个`npm`依赖：

   - `xml2js`
   - `@wecom/crypto`

![](img/ba94964506f545141176535b8afe33cd.png)

3. 创建一个云函数 `chatgpt` （不可改名），取消所有请求方法，填写下面的代码并发布。

```
import cloud from '@lafjs/cloud';

export async function sendGptMessage(message) {
  console.log('send gpt message', message)
  const res = await cloud.fetch({
    url: 'https://fastgpt.run/api/openapi/chat/chat',
    method: 'POST',
    headers: {
      apikey: process.env.FASTGPT_APIKEY
    },
    data: {
      "modelId": process.env.FASTGPT_MODEL_ID,
      "isStream": false,
      "prompts": [
        {
          "obj": "Human",
          "value": message
        },
      ]
    }
  })
  console.log('send gpt message result', res?.data)
  return res.data?.data
}
```

4. 创建一个云函数 `wxwork` (不可改名)，取消所有请求方法，填写下面的代码并发布。

```
import cloud from '@lafjs/cloud';

export async function getToken() {
  const cache = cloud.shared.get('access-token')
  console.log('checking cache', cache, cache.expires >= Date.now())
  if (cache && cache.expires >= Date.now()) return cache.token;
  console.log('start to get token of', process.env.WXWORK_CORPID)

  const res = await cloud.fetch({
    url: 'https://qyapi.weixin.qq.com/cgi-bin/gettoken',
    method: 'get',
    params: {
      corpid: process.env.WXWORK_CORPID,
      corpsecret: process.env.WXWORK_CORPSECRET,
    }
  })
  const token = res.data.access_token
  cloud.shared.set('access-token', { token, expires: Date.now() + res.data.expires_in * 1000 })
  return token
}

export async function sendMessage(message, user) {
  console.log('send messagge', user, message)
  const res = await cloud.fetch({
    url: 'https://qyapi.weixin.qq.com/cgi-bin/message/send',
    method: 'POST',
    params: {
      access_token: await getToken()
    },
    data: {
      "touser": user,
      "msgtype": "text",
      "agentid": process.env.WXWORK_AGENTID,
      "text": {
        "content": message
      },
      "safe": 0,
      "enable_id_trans": 0,
      "enable_duplicate_check": 0,
      "duplicate_check_interval": 1800
    }
  })
  console.log('send message result', res.data)
}
```

5. 创建一个云函数 `wxwork-gpt-api` (随便改名)，开放`GET`和`POST`请求，填写下面的代码并发布。

```
import cloud from '@lafjs/cloud';
import { decrypt, getSignature } from '@wecom/crypto';
import { sendMessage } from '@/wxwork';
import { sendGptMessage } from '@/chatgpt';
import xml2js from 'xml2js';

export default async function (ctx: FunctionContext) {
  const { query } = ctx;
  const { msg_signature, timestamp, nonce, echostr } = query;
  const token = process.env.WXWORK_TOKEN;
  const key = process.env.WXWORK_AESKEY;
  console.log('start to invoke wxwork-gpt-api')

  // 签名验证专用
  if (ctx.method === 'GET') {
    const signature = getSignature(token, timestamp, nonce, echostr);
    if (signature !== msg_signature) {
      return { message: '签名验证失败', code: 401 }
    }
    const { message } = decrypt(key, echostr);
    return message
  }

  const payload = ctx.body.xml;
  const encrypt = payload.encrypt[0];
  const signature = getSignature(token, timestamp, nonce, encrypt);
  if (signature !== msg_signature) {
    return { message: '签名验证失败', code: 401 }
  }
  //ctx.response.sendStatus(200);
  const { message } = decrypt(key, encrypt);
  const {
    xml
  } = await xml2js.parseStringPromise(message)
  // 由于GPT API耗时较久，这里提前返回，防止企业微信超时重试，后续再手动调用发消息接口
  ctx.response.sendStatus(200);
  await asyncSendMessage(xml)
  return { message: true, code: 0 }
}

async function asyncSendMessage(xml) {
  if (xml.MsgType[0] !== 'text') return;
  sendMessage('AI思考中, 请耐心等待~~', xml.FromUserName[0])
  try {
    const result = await sendGptMessage(xml.Content[0])
    await sendMessage(result, xml.FromUserName[0])
  } 
  catch (e){
    console.log(e, e.config)
    sendMessage('接口请求失败，请联系管理员查看错误信息', xml.FromUserName[0])
  }
}
```

6. 点击左下角设置，选择环境变量，将刚刚收集的`WXWORK_TOKEN`、`WXWORK_AESKEY`、`WXWORK_CORPID`、`WXWORK_AGENTID`、`WXWORK_CORPSECRET`、`FASTGPT_APIKEY`、`FASTGPT_MODEL_ID` 填入，并点击更新按钮。

![](img/2a7d6555a1fa30f066b7a611f14251d6.png)

7. 复制 `wxwork-gpt-api` 这个云函数的调用地址，将它填写到企业🛰️管理后台【API接收消息】页面中的URL中，并保存上述信息。

![](img/e156aa940a184d25931c308f77e6c2df.png)

![](img/b277d6e249d5d8d2162b3a387f3870d5.png)

8. 在企业🛰️中，进入你刚刚创建的应用，就可以跟机器人愉快的聊天了~~

![](img/50c4a22d0bc89f9be550950c34797d54.png)
