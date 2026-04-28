# creem支付配置：小白视角和路径，手把手教学 副本

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/OwpkdbkOVobYNpxHZdgcQjfYn4d](https://xatc0v8uz5m.feishu.cn/docx/OwpkdbkOVobYNpxHZdgcQjfYn4d)

**网站出海赚美金****是每一个出海人在做的事儿，赚美金，我**们把工具做好了，尤其是一次性支付或者订阅的这种工具，就需要有一种支付工具帮助我们“收款美金”，常见的stripe需要办理港卡，且封控严格，新手极不友好；PayPal的话电商确实常见，只是必须需要公司，对于独立个体也不方便，这时候Creem出现了！

+   **中国人友好**

+   **直接打款支付宝**

+   **支付接入简单**

这几个在一起，“如果是真的”，那很王炸啊！

[第一次看到creem支付是在这](https://www.creem.io/?ref=javayhu.com)篇文章 海外支付平台Creem注册教程

![](img/ae8b4021ec72fbccfc06080ba40e5131.png)

**当时的说法是，这个支付只需**要20分钟就能搞定，我信了，注册到审核确实只要20分钟，但是后面的操作，让作为0基础小白的我崩溃了。经过了2周的爱恨纠结，终于还是把这个creem给装完了了，，3月底接完了支付也写了这篇文章的80%，但是还是拖到了5月中才真正把creem的支付给“拿下”，今天就用小白的视角帮大家一起梳理接creem支付的每一个步骤。

文章主要包含四个部分：

1.  注册creem账号

1.  准备各种creem的信息

1.  webhook和ngrok（有些情况不需要）

1.  正式配置支付

# 一、注册账号+完善信息

## 1. 注册账号（google可直接登录）

[直通车](https://www.creem.io/sign-in)，可以直接google登录 或者邮箱注册，注册成功就会进入下面的页面

![](img/1a4b5bf201fae90d3d63f2b522d16033.png)

![](img/23f1bed4fe7efd1ac1ab724dceb37e66.png)

## 完善提现信息

### 2.1 创建提现账号

在Balance的Payout Account中，选择创建一个新的账号，国家选择China，

![](img/7121363ea16ad9c83b057c246f9c681e.png)

### 2.2 填写业务信息

主要填写你的基本信息和你的产品是做什么的

![](img/10009e0684bc61434ca85de3e09f546a.png)

### 2.3 完善个人信息

请按照下面的流程，完成个人信息填写，上传大陆身份证的正反面，并且电脑摄像头拍照，最终你的个人信息会被保存到Sumsub ID中。

![](img/261764e1b5bab5445bcc076245ec461e.png)

![](img/63b9a93f0e84b43015cecdad3de6716a.png)

提交完以后，你可能会看到：

![](img/72e7e136ef24f06bcaba6a5f88b8fe7b.png)

![](img/bf0c541ddb8fa551782664e6632f42b3.png)

莫慌，等待即可，大概过了24小时，就会邮件通知 审核通过！

当然 你也有可能是 直接通过的那种

![](img/6e219bb31711f8747d2ae3327b290f7d.png)

![](img/3002973ad6cd97b2dd56c74ebd85bf30.png)

### 2.4 完善提现信息

点击Payout Details的Manage Payout Account，完善提现信息。

![](img/dd6fea0308294577c22f222d16a6d6c1.png)

填写提现账号的国家，选择China，币种选择CNY，转账方式选择支付宝。

接着填写支付宝的账号，邮箱地址和全名（中文名）。

![](img/bae21ca7fef7780980c28ea53447268a.png)

最后填写你的家庭地址，这里如实填写即可。提交之后，个人信息和提现信息就完善好了。

![](img/9ffb778a8be4f313aa0e427c85d02584.png)

### 2.5 等待审核

最后就是等待审核，快的话，半天到一天就可以结果。

![](img/865c652311a1f486bb9b7d3deacea9a6.png)

# 二、准备2套配置信息

测试环境和生产环境的配置信息不一样！

这个非常的重要！当初我跟彩笺都忽略了“test mode”这个东西！

打开和关掉，是完全的2套内容，所以下面所有的东西，都要做2遍。

> 一个是 test mode一个是非test mode

![](img/1ec0e30fec645b1f64c59bf2646da934.png)

## 1. product

首先是注册产品

product--->add product

![](img/8d0d226d0387df730ddb4c9c29049bab.png)

### 1.1 product details

![](img/4ece54c9248f1c6e8df797a6f82b66cb.png)

name：产品名字

return url：用户在支付完成以后，你希望对方返回的页面

description：产品介绍（可以让ai生成啦～）

status：选择active（激活状态）

image preview:产品图，可以上传，可以不上传

### 1.2 payment details

![](img/43e996219c9fcece69002e7af620ab37.png)

支付方式：你是一次性支付（single payment）还是 订阅类型（subscrition）的支付

curency：货币方式

price:自定义价格

tax category：一般选择“digital goods or service”（虚拟服务）

tax behaviour：是否包含税（看个人情况选择

### 1.3 product other infomation

![](img/4dd35d3e0d72bee6e1f9fd92eeeb0472.png)

图片、特性、其他...

这些内容都能让ai搞定

填写完成以后 点击“create product”

### 1.4 product信息

在product页面 自己创建的产品 有三个点，点击以后 可以复制自己的product id

![](img/2ba8bf03fcd78bb8313d5239b38a54a0.png)

## 2. api key

![](img/12b94e7553a10b0e6107bdc653104b27.png)

点击右上角的 developer就创建自己的api key

记得创建2组，test和非test的 这是2个不同的

# 三、webhook&ngrok（选择性）

## 1 什么是webhook

[直通车](https://docs.creem.io/learn/webhooks/introduction)

Webhook 可以理解为一个网络通知器"。

想象你在等快递，有两种方式知道快递到了：

1.  **传统方式**：你每隔一会儿就要打开App 查看快递到哪了（这就像传统的轮询方式。隔一段时间就问，然后得到回复）

1.  **Webhook** 方式：快递员到了直接给你打电话（这就是 Webhook 的工作方式）

## 2 怎么判断自己需不需要

![](img/a36e64b686d6203aa76a043d7034d7c4.png)

以下情况你可能需要 Webhook：

1.  **需要实时通知**

+   用户下单后立即通知商家

+   有人在你的博客留言后立即收到提醒

1.  **需要自动化处理**

+   用户付款成功后自动发货

+   有人提交表单后自动发送确认邮件

1.  **需要系统联动**

+   Github 代码更新后自动部署网站

+   订单系统和库存系统需要实时同步

**如果你的网站不需要这些实时性或自动**化的功能，可能就不需要 Webhook。

## 3 准备webhook

如果细心的朋友会发现，刚刚在创建api key的时候 旁边就有一个 webhooks

![](img/2c2c7ceb269ad03b1991153e65efb050.png)

点击add webhook只需要填写两个信息

webhook name：取个名字，这个通知器是通知什么的

webhook url：这个地方的url是指，在支付完成以后，creem需要向哪一个page传达通知。

那这个url具体要怎么填写呢？

### 3.1 非test mode

+   **直接**让**cursor告**诉你 需要那个页面来接收通知，他就会给你一个路径，域名+path即可.例如下面的

```
www.XXXX.com/payment/result
```

### 3.2 test mode

按理说应该是

```
http://localhost:3000/payment/result
```

但是 你填进去 就得到通知，**不能使用本**地的路径，必须使用公开的URL。所以这个时候你就需要一个新的东西ngrok。

> 具体看下一个part的具体实操

经过ngrok的操作以后，本地连接就会变成一个可以用的public url

```
https://8733-191-204-177-89.sa.ngrok.io/payment/result
```

### 3.3 获取 webhookRUL

创建完成以后 获取 webhook的URL

![](img/fe48742871f607d82b6f8c3c191a08d2.png)

### 3.4 获取webhook的secret

![](img/68446a5d0638652d6bb935730369e7a4.png)

![](img/d322b2fb864686a71da544caf0db8d72.png)

## 4. ngrok

ngrok 是一个非常实用的网络工具，它的主要作用是帮助我们实现"内网穿透"，开发者可以让其他人实时查看本地开发的网站

### 4.1 注册账号

[ngrok](https://dashboard.ngrok.com/login)

可以google直接登录，点击菜单栏的 “your authtoken”，查看并复制你的token

![](img/7892b54c4c82274a94f1e5ddb70a69df.png)

![](img/4fcb32a1f4d5bb9a405e2e52a50f5788.png)

### 4.2 安装ngrok

#### 4.2.1 下载

```
brew install ngrok
```

也可以直接点击“download”进行下载，选择对应的芯片

![](img/3c655f5123a7a1239253ebc485c40f78.png)

#### 4.2.2 加入token

添加 authtoken到电脑上，终端运行下面内容

```
ngrok config add-authtoken 你的token
```

### 4.3 使用ngrok

启动本地开发服务，终端运行下面👇

```
npm start
```

**运行ngrok，一般程序在**3000端口，如果你的有指定修改端口即可

```
ngrok http 3000
```

运行完以后会出现 类似下面的图，就得到了local的path对应的public URL

![](img/d4e5a67d232e162c2079fcd0340b5bae.png)

最后再把我们需要的具体path换进去例如

```
localhost:3000/payment/result
```

Public URL就是

```
https://08f7-115-196-182-99.ngrok-free.app/payment/result
```

### 4. 注意事项

免费版本的，每一次 run ngrok以后 都会生成一个随机的url。

**如果你关闭了terminal以后**，再次打开，就会生成另外一个URL，你就需要去creem里面更换mode 模式下的 webhook的URL！

# 四、配置

## 1. 检查配置信息

经过以上种种 ，我们应该就得到了下面的所有东西

env文件

```
CREEM_API_KEY=
CREEM_PRODUCT_ID=
CREEM_API_URL=https://api.creem.io
CREEM_SUCCESS_URL=
```

### api_key

```
CREEM_API_KEY=
CREEM_API_KEY_TEST=
```

### product_id

```
CREEM_PRODUCT_ID=
CREEM_PRODUCT_ID_TEST=
```

### api URL

> 这个好像是通用的

```
CREEM_API_URL=https://api.creem.io
CREEM_API_URL_TEST=https://test-api.creem.io
```

### webhook 相关

```
CREEM_WEBHOOK_URL_test=
CREEM_WEBHOOK_URL=
CREEM_WEBHOOK_secret_test=
CREEM_WEBHOOK_secret=
```

### 支付成功以后要返回的页面路径

```
CREEM_SUCCESS_URL=
CREEM_SUCCESS_URL_TEST=
```

## 2. 使用cursor连接

### 2.1 支付的逻辑

我让ai给我画了一个说明的流程图，结果感觉画的非常复杂，看起来可能真的这么复杂，简单来说就是，

+   前端看到的：用户点击付款按钮--->跳转付款页面--->填写信息付款成功--->返回某个页面/获得积分

+   后端实际：服务器发起请求--->支付端收到请求，给了一个付款连接--->服务器把付款连接传给用户--->用户支付--->支付端收到支付通知--->支付端给服务器回传消息（通知支付完成）

大概就是这么个意思～（主打一个看图说话

网站的支付就2种，一次性（pay one-time）&订阅（subscribe），下面就手把手把带大家尝试一下 一次性付费。

配置规则[文档直通车](https://docs.creem.io/learn/checkout-session/introduction)

### 2.2 一次性付费（Return URL）

> 一句话说逻辑就是：配置规则在这里，我有这些配置信息，我希望实现什么效果，你帮我写

划重点**“配置信息”**、**“配置规则”**、**“实现效果”**

就是这3个内容，配置信息上面我们已经搞定，接下来就是配置规则和实现效果

先确定我们这一次的实现效果

⚠️整个操作，不涉及用户注册和登录，所以也挺容易实现的，别害怕，即使是后面有注册和登录，也都有第三方集成，莫慌！

这个实现逻辑，我们是需要有一个使用参数`success_url`和`signature验证`直接看到对应文档官方解释

1.  您可以为每个 checkout_session 传递一个自定义的success_url，它将覆盖success_url产品上的设置。这使得您可以在每次付款后动态地将用户重定向到自定义页面（对于在付款后将用户引导到他们的特定帐户资源很有用）。

1.  返回和重定向 URL 是成功付款后您的客户将被重定向到的 URL。它们包含由 creem 签名的重要信息，您可以使用这些信息来验证付款和用户。

划重点：付款成功，重定向的自定义页面，使用信息验证付款（和用户）

一次性付费总共需要3个API：创建checkout、生成signature、验证signature

#### API1:创建会话

```
const redirectUrl = await axios.post(
      `https://api.creem.io/v1/checkouts`,
        {
          &quot;success_url&quot;: &quot;https://example.com&quot;,
          &quot;product_id&quot;: &quot;prod_your-product-id&quot;,
        },
        {
          headers: { &quot;x-api-key&quot;: `creem_123456789` },
        },
    );
```

看起来吓人，实际上就4个东西：

1.  https://api.creem.io/v1/checkouts支付连接(固定的，测试和生产2个不同连接)

1.  success_url": "https://example.com 支付成功以后你要去到的页面path（路径）

1.  "product_id": "prod_your-product-id 换成你的产品id

1.  "x-api-key": `creem_123456789 换成你的api key（测试和生产2个不同）

我的文件如下：

```
import axios from &#x27;axios&#x27;;

export interface CreateCheckoutParams {
  productId: string;
  requestId?: string;
  successUrl?: string;
}

export interface CreateCheckoutResponse {
  checkout_url: string;
  [key: string]: any;
}

/**
 * 创建Creem结账会话
 * @param params 结账参数
 * @returns 包含结账URL的响应
 */
export async function createCheckout(params: CreateCheckoutParams): Promise&lt;CreateCheckoutResponse&gt; {
  try {
    const API_URL = process.env.CREEM_API_URL || &#x27;https://test-api.creem.io&#x27;;
    const API_KEY = process.env.CREEM_API_KEY || &#x27;creem_test_3sioDtbY5ADbmoODbQnNiW&#x27;;
    
    // 获取格式化的API基础URL（确保没有尾部斜杠）
    const baseUrl = API_URL.endsWith(&#x27;/&#x27;) ? API_URL.slice(0, -1) : API_URL;
    const apiUrl = `${baseUrl}/v1/checkouts`;

    const response = await axios.post(
      apiUrl,
      {
        product_id: params.productId,
        request_id: params.requestId,
        success_url: params.successUrl,
      },
      {
        headers: { &#x27;x-api-key&#x27;: API_KEY },
      }
    );

    if (!response.data || !response.data.checkout_url) {
      throw new Error(&#x27;API response does not contain checkout_url&#x27;);
    }

    return response.data;
  } catch (error) {
    console.error(&#x27;Error creating checkout session:&#x27;, error);
    if (axios.isAxiosError(error) &amp;&amp; error.response) {
      console.error(&#x27;API error response:&#x27;, error.response.data);
    }
    throw error;
  }
}

```

![](img/be835a3a448e99a0ff5b738190487a5d.png)

(GPT的解读，下面的你也可以发给任意ai去看看写的到底是什么)

#### API2:生成signture

```
import crypto from &#x27;crypto&#x27;;

/**
 * 生成Creem签名
 * @param params 参数对象
 * @param apiKey API密钥
 * @returns 生成的签名
 */
export function generateSignature(params: Record&lt;string, string&gt;, apiKey: string): string {
  // 创建格式为 &quot;key1=value1|key2=value2|...|salt=apiKey&quot; 的数据字符串
  // 重要：不要对键进行排序 - 按照提供的顺序使用
  const data = Object.entries(params)
    .map(([key, value]) =&gt; `${key}=${value}`)
    .concat(`salt=${apiKey}`)
    .join(&#x27;|&#x27;);

  // 使用SHA-256哈希算法生成签名
  const hash = crypto.createHash(&#x27;sha256&#x27;).update(data).digest(&#x27;hex&#x27;);
  return hash;
}

```

#### API3:验证签名signature

```
import { generateSignature } from &#x27;./signatureUtils&#x27;;

export interface RedirectParams {
  request_id?: string | null;
  checkout_id?: string | null;
  order_id?: string | null;
  customer_id?: string | null;
  subscription_id?: string | null;
  product_id?: string | null;
}

/**
 * 验证Creem签名
 * @param params 重定向参数
 * @param signature 要验证的签名
 * @returns 签名是否有效
 */
export function verifySignature(params: Record&lt;string, string&gt;, signature: string): boolean {
  try {
    const API_KEY = process.env.CREEM_API_KEY || &#x27;creem_test_3sioDtbY5ADbmoODbQnNiW&#x27;;

    // 过滤掉null/undefined值，并移除signature参数（如果存在）
    const filteredParams: Record&lt;string, string&gt; = {};
    Object.entries(params).forEach(([key, value]) =&gt; {
      if (value !== null &amp;&amp; value !== undefined &amp;&amp; key !== &#x27;signature&#x27;) {
        filteredParams[key] = value;
      }
    });

    // 使用Creem提供的方法生成签名
    const computedSignature = generateSignature(filteredParams, API_KEY);
    
    // 比较计算的签名与接收到的签名
    return computedSignature === signature;
  } catch (error) {
    console.error(&#x27;Error verifying signature:&#x27;, error);
    return false;
  }
}

```

## 测试支付

### 3.1 测试支付

测试支付时使用测试信用卡

卡号

```
4242 4242 4242 4242
```

其他信息都可以随便填写

### 3.2 实际支付（必须看！痛失40刀）

完成了以上所有的，也完成了测试模式的支付以后，准备实际检查一下，生产环境的真实支付了，请一定要做以下事情，不然荷包真的会痛！

#### 💶1000元免手续费说明

这个1000元的货币单位竟然是“欧元”！请注意，不是美元，是欧元。我跟彩笺都踩了这个坑，一开始我的产品设置的就是美元，自己给自己付了9.9以后发现扣了手续费！后来仔细看，越看越不对劲儿，这个符号，就是欧元符号吧！

#### 🎫Discount 折扣

在菜单页，有一个非常明显的discount按钮，这里可以自己创建优惠券，最高是100%的折扣，也就是免费使用！

免费使用！

在林悦己给自己的支付测试冲了5次9.9美刀以后，在大哥的提醒下，发现并使用了这玩意儿，一开始我还只是设置90%，结果大哥自己测试的时候，直接上了100%（我？）

有了这个折扣券，在支付的时候甚至不需要输入银行卡信息，只需要输入discount的code就会显示FREE！

#### 💰Refund 退款

在大哥的提醒下，我又无意中点进了我的payment，想着，是不是可以退款（refund），还真给我找到了，点击order最右边的三个点，就会有一个“refund”按钮，2分钟内完成退款

> （如果是立刻支付，信用卡还没入账的情况，写这个帖子的时候，我有4笔9.9是几天前的，还没退给我）

# 五、实战操作演示

> 前提：这次实战，只专注在如何接支付的api，关于“登录”、“数据库”等部分不会详细说明（后面我也准备再写一篇接支付的）

## 5.1 完成的效果

点击按钮，触发支付，完成支付，返回指定的成功页面，creem后台查看钱是否收到

![](img/724b09acb3ab11008510dbb2eda8a84c.png)

![](img/a672224adaacb1fad8ee850db61f8a67.png)

![](img/bc7f37bdb19b89af0981860cb8080003.png)

![](img/fdccedf3ff83fccd5e32951d66d8331c.png)

![](img/6e76c47095096941c05e25e7e54dfe31.png)

## 5.2 代码结构

为了节省cursor的次数，我使用augment作为演示（还没有用上augment的朋友可以看我之前那篇augment，现在还能薅羊毛，越早上车越好）！

```
支付流程
├── 1. 用户点击支付按钮 (app/page.tsx)
│   └── 生成唯一requestId
│
├── 2. 前端调用API (app/api/create-checkout/route.ts)
│   └── 调用createCheckout服务
│       └── services/creem/createCheckout.ts
│           ├── 使用环境变量获取API配置
│           ├── 调用Creem API创建结账会话
│           └── 返回checkout_url
│
├── 3. 用户被重定向到Creem支付页面
│
├── 4. 支付完成后，Creem重定向回success页面
│   ├── URL包含支付结果参数和签名
│   └── app/success/page.tsx (服务端组件)
│       └── app/success/client.tsx (客户端包装器)
│           └── components/SuccessContent.tsx
│               ├── 从URL获取参数
│               ├── 调用verifySignature验证签名
│               │   └── services/creem/verifySignature.ts
│               │       ├── 过滤参数
│               │       └── 调用generateSignature生成签名
│               │           └── services/creem/signatureUtils.ts
│               │               ├── 按照Creem规则格式化参数
│               │               ├── 添加API密钥作为salt
│               │               └── 使用SHA-256生成哈希
│               └── 根据验证结果显示成功或失败页面
```

### 各文件详细功能

1.  signatureUtils.ts:

+   核心功能：生成符合Creem规范的签名

+   工作方式：将参数按照key1=value1|key2=value2|...|salt=apiKey格式连接，然后使用SHA-256哈希算法生成签名

+   被verifySignature.ts调用来验证签名

1.  verifySignature.ts:

+   核心功能：验证从Creem重定向回来的URL中的签名

+   工作方式：过滤参数，移除signature参数，调用generateSignature生成签名，然后与URL中的签名比较

+   被SuccessContent.tsx组件调用来验证支付结果

1.  createCheckout.ts:

+   核心功能：调用Creem API创建结账会话

+   工作方式：使用axios发送POST请求到Creem API，返回包含checkout_url的响应

+   被app/api/create-checkout/route.ts调用

1.  app/api/create-checkout/route.ts:

+   核心功能：处理前端支付请求

+   工作方式：接收前端请求，验证参数，调用createCheckout服务，返回结果

+   被app/page.tsx中的支付按钮点击事件调用

## 5.3 跟AI对话的过程

```
我需要接入creem，在service下面，帮我封装creem的api接口，api一共有3个，分别是 “createCheckout”、“signatureUtils”、“verifySignature”
关于这三个的参考文件我已经放到readme里面，你可以参考学习，环境变量我已经准备好了
代码写完以后 请测试最后的验证signature，成功以后处理首页按钮的调用
```

写好md文件，发出prompt，就开始自己工作了

![](img/5e11830ca81cbd4c5cfa3375cfe7cb54.png)

如果你遇到了 verify失败，就是下面这种情况

![](img/0b12c378514d2c2f29e108f2f79ab629.png)

就让ai检查verify的代码和参考代码的区别，严格按照参考代码进行修改：

![](img/3a24d2929858a03712553f1c16c3bfe3.png)

![](img/747091b227c9b9aa9b08405c2d5db291.png)

# 六、碎碎念

一开始不懂，真的是很正常的，不懂就硬上！崩溃100次以后，就进入下一个阶段了！

**支付、登录**真的是2座可怕的大山，这篇内容其实在3月底的时候我就写好了，一直想加一个实操演练，结果就拖到了现在。

面对这个自己害怕的东西，搞明白支付到底怎么接，api还有service之间的关系到底是怎样，似懂非懂等于不懂，完全懂，完全明白，完全掌握，才算真正克服了这座大山。借老师的那句话，今天，我终于把支付这颗珠子 给扔掉了！

![](img/62c5325dab10a05b9070f6a82c959206.png)

希望这篇内容能帮助到需要接支付的你，如果接入成功，欢迎回来留言喔！