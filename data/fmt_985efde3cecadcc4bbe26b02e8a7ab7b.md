
# 接口封装与测试支付

## API1: createCheckout 创建结账会话

```
import axios from 'axios';

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
export async function createCheckout(params: CreateCheckoutParams): Promise<CreateCheckoutResponse> {
  try {
    const API_URL = process.env.CREEM_API_URL || 'https://test-api.creem.io';
    const API_KEY = process.env.CREEM_API_KEY || 'creem_test_3sioDtbY5ADbmoODbQnNiW';
    
    // 获取格式化的API基础URL（确保没有尾部斜杠）
    const baseUrl = API_URL.endsWith('/') ? API_URL.slice(0, -1) : API_URL;
    const apiUrl = `${baseUrl}/v1/checkouts`;

    const response = await axios.post(
      apiUrl,
      {
        product_id: params.productId,
        request_id: params.requestId,
        success_url: params.successUrl,
      },
      {
        headers: { 'x-api-key': API_KEY },
      }
    );

    if (!response.data || !response.data.checkout_url) {
      throw new Error('API response does not contain checkout_url');
    }

    return response.data;
  } catch (error) {
    console.error('Error creating checkout session:', error);
    if (axios.isAxiosError(error) && error.response) {
      console.error('API error response:', error.response.data);
    }
    throw error;
  }
}
```

## API2: 生成signture

```
import crypto from 'crypto';

/**
 * 生成Creem签名
 * @param params 参数对象
 * @param apiKey API密钥
 * @returns 生成的签名
 */
export function generateSignature(params: Record<string, string>, apiKey: string): string {
  // 创建格式为 "key1=value1|key2=value2|...|salt=apiKey" 的数据字符串
  // 重要：不要对键进行排序 - 按照提供的顺序使用
  const data = Object.entries(params)
    .map(([key, value]) => `${key}=${value}`)
    .concat(`salt=${apiKey}`)
    .join('|');

  // 使用SHA-256哈希算法生成签名
  const hash = crypto.createHash('sha256').update(data).digest('hex');
  return hash;
}
```

## API3: 验证签名signature

```
import { generateSignature } from './signatureUtils';

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
export function verifySignature(params: Record<string, string>, signature: string): boolean {
  try {
    const API_KEY = process.env.CREEM_API_KEY || 'creem_test_3sioDtbY5ADbmoODbQnNiW';

    // 过滤掉null/undefined值，并移除signature参数（如果存在）
    const filteredParams: Record<string, string> = {};
    Object.entries(params).forEach(([key, value]) => {
      if (value !== null && value !== undefined && key !== 'signature') {
        filteredParams[key] = value;
      }
    });

    // 使用Creem提供的方法生成签名
    const computedSignature = generateSignature(filteredParams, API_KEY);
    
    // 比较计算的签名与接收到的签名
    return computedSignature === signature;
  } catch (error) {
    console.error('Error verifying signature:', error);
    return false;
  }
}
```

![](img/be835a3a448e99a0ff5b738190487a5d.png)

(GPT的解读，下面的你也可以发给任意ai去看看写的到底是什么)

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

1. `signatureUtils.ts`:

+   核心功能：生成符合Creem规范的签名

+   工作方式：将参数按照 `key1=value1|key2=value2|...|salt=apiKey` 格式连接，然后使用SHA-256哈希算法生成签名

+   被 `verifySignature.ts` 调用来验证签名

1. `verifySignature.ts`:

+   核心功能：验证从Creem重定向回来的URL中的签名

+   工作方式：过滤参数，移除signature参数，调用 `generateSignature` 生成签名，然后与URL中的签名比较

+   被 `SuccessContent.tsx` 组件调用来验证支付结果

1. `createCheckout.ts`:

+   核心功能：调用Creem API创建结账会话

+   工作方式：使用 `axios` 发送POST请求到Creem API，返回包含 `checkout_url` 的响应

+   被 `app/api/create-checkout/route.ts` 调用

1. `app/api/create-checkout/route.ts`:

+   核心功能：处理前端支付请求

+   工作方式：接收前端请求，验证参数，调用 `createCheckout` 服务，返回结果

+   被 `app/page.tsx` 中的支付按钮点击事件调用

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
