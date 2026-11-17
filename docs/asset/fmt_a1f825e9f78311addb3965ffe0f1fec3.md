
# 09 | 做Avatar虚拟人直播

> 来源：[https://docs.feishu.cn/docx/LdVbdjlcmoC4TtxmMr5c9irdnEf](https://docs.feishu.cn/docx/LdVbdjlcmoC4TtxmMr5c9irdnEf)

![](img/edd5ec23059487d85d66d8cde2bcf65a.png)

## 0 前言

[最近朋](https://so.csdn.net/so/search?q=GPT&spm=1001.2101.3001.7020)友圈以及身边很多朋友都在研究GPT开发，做了各种各样的小工具小`Demo`，AI工具用[起来](https://www.zego.im/product/InstantMessage?source=csdn&product2&login=from_csdn)是真的香！在他们的影响下，我也[继续捣](https://www.zego.im/product/metaworld?source=csdn&product3&login=from_csdn)鼓`GPT Demo`，希[望更多](https://www.zego.im/product/metaworld?source=csdn&product3&login=from_csdn)的开发者加入一起多多交流。

上一篇结合即时通 IM SDK捣鼓了一个`Demo ChatGPT`群聊机器人`Demo`，也收到了一些还不错的反馈，有伙伴用于客服场景解放了一部分人工重复工作提升了效率。趁着周末我捣鼓了一个`ChatGPT`虚拟人直播，结合了同样热门的元宇宙技术。

本文将通过详细[开发流程](https://so.csdn.net/so/search?q=%E5%BC%80%E5%8F%91%E6%B5%81%E7%A8%8B&spm=1001.2101.3001.7020)来为大家举例如何快速实现一个[虚拟人](https://www.zego.im/product/metaworld?source=csdn&product3&login=from_csdn)[直播，](https://www.zego.im/product/metaworld?source=csdn&product3&login=from_csdn)可用于无人直播场景。`ChatGPT`虚拟人可抓取直播中粉丝弹幕和评论并回复来进行用户互动，提升直播间的活跃与用户体验。

最终实现效果如下所示：[GP](https://live.csdn.net/v/289757?login=from_csdn)[T虚拟](https://www.zego.im/product/metaworld?source=csdn&product3&login=from_csdn)人直播

## 1 实现思路

`ChatGPT4.0`[虚拟人](https://www.zego.im/product/metaworld?source=csdn&product3&login=from_csdn)[直播，](https://www.zego.im/product/metaworld?source=csdn&product3&login=from_csdn)首先得要有2个最基本的能力：`ChatGPT4.0`接口和虚拟人交互能力，这两块我们后面具体讲。整个流程如下图所示：

![](img/5dbfec29a6ba7be498e3869454a2d898.png)

1. 需要有具备文字驱动能力的[虚拟人](https://www.zego.im/product/metaworld?source=csdn&product3&login=from_csdn)。具体来说，需要将文字转为语音，然后虚拟人能根据语音对口型。这里推荐使用即构`Avatar`，即构`Avatar`可以捏脸定制，同时具备文字驱动口型能力。

2. 直播推流。抖音、快手可以用直播伴侣实时截屏推流。

3. 获取实时弹幕/评论。虚拟人根据观众发的弹幕或者评论文本数据发送给`ChatGPT`，可以去`github`找开源代码。这里提供一个最简单的思路：在网页版注入`JS`代码，`js`实时读取网页上的弹幕。

4. 调用`ChatGPT`。将实时弹幕/评论数据发送给`ChatGPT`并获取回复。

5. 虚拟人播报`ChatGPT`的回复。

## 2 ChatGPT接入

目前`OpenAI`提供的接口是`ChatGPT3.5`，可以直接调用。`bing`在最新版`Edge`浏览器提供`ChatGPT4.0`体验，有一些开源库对其做了二次封装，只需提供`Cookie`即可体验。这里我们把两种方式都讲解。

### 2.1 ChatGPT 4.0 接入

首先注册`bing`账号：

1. 前往`https://www.microsoft.com/zh-cn/edge/download?ch&form=MA13FJ`下载最新的`Edge`浏览器。

2. 开启`VPN`，注册用户或使用已有账号登录，点击右上角「设置」图标，将所在区域设置如下

3. 打开`Edge`，输入网址`https://www.bing.com/search?form=MY0291&OCID=MY0291&q=Bing+AI&showconv=1`

![](img/bb1c160801b79738ef491384c62a3ee4.png)

接下来在代码里面封装`ChatGPT4.0`接口，先安装第三方基于`Edge`浏览器的库：

```
npm i @waylaidwanderer/chatgpt-api
```

接下来二次封装

```
import { BingAIClient } from &#x27;@waylaidwanderer/chatgpt-api&#x27;;
export class BingGPT {
    /*
    * http_proxy, apiKey
    **/
    constructor(http_proxy, userCookie) {
        this.api = this.init(http_proxy, userCookie); 
    }
    init(http_proxy, userCookie) {
       console.log(http_proxy, userCookie)
        const options = {
            // Necessary for some people in different countries, e.g. China (https://cn.bing.com)
            host: &#x27;https://www.bing.com&#x27;,
            // &quot;_U&quot; cookie from bing.com
            userToken: userCookie,
            // If the above doesn&#x27;t work, provide all your cookies as a string instead
            cookies: &#x27;&#x27;,
            // A proxy string like &quot;http://&lt;ip&gt;:&lt;port&gt;&quot;
            proxy: http_proxy,
            // (Optional) Set to true to enable `console.debug()` logging
            debug: false,
        };

        return new BingAIClient(options);
    }
    //调用chatpgt 
    chat(text, cb) {
        var res=&quot;&quot;
        var that = this;
        console.log(&quot;正在向bing发送提问&quot;, text ) 
        this.api.sendMessage(text, { 
            toneStyle: &#x27;balanced&#x27;,
            onProgress: (token) =&gt; { 
                if(token.length==2 &amp;&amp; token.charCodeAt(0)==55357&amp;&amp;token.charCodeAt(1)==56842){
                    cb(true, res);
                } 
                res+=token;
            }
        });  
    }
} 
```

注意到，除了需要提供`vpn`地址以外，还需要提供`cookie`数据。`cookie`可以在`Edge`浏览器中按`F12`打开开发工具，点击应用程序Tab，从左侧`Cookie`中找到`_U`的`value`即为想要的`cookie`，如下图所示：

![](img/e274fae855933592cb66709d02cd529b.png)

### 2.2 ChatGPT 3.5 接入

因为`ChatGPT4.0`基于浏览器，非官方提供的API，整体会有些不太稳定。而`OpenAI`官方提供了`ChatGPT3.5`，相比之下更稳定。

首先要有`OpenAI`的`ChatGPT`账号，在这里我们不细讲，不了解的同学可以参考我另一篇文章，链接：《当我把`ChatGPT`拉进群聊里，我的朋友都玩疯了》接下来只需安装`ChatGPT`库：

```
npm install chatgpt
```

注意要使用`nodejs` 18.0以上版本安装，因为国内的`IP`无法使用`ChatGPT`，因此必须要有`VPN`代理。再安装一个代理库：

```
npm install https-proxy-agent node-fetch
```

接下来，二次封装一下：

```
import { ChatGPTAPI } from &quot;chatgpt&quot;;
import proxy from &quot;https-proxy-agent&quot;;
import nodeFetch from &quot;node-fetch&quot;;
 
export class ChatGPT {
 
    constructor(http_proxy, apiKey) {
        this.api = this.init(http_proxy, apiKey);
        this.conversationId = null;
        this.ParentMessageId = null;
    }
    init(http_proxy, apiKey) {
        console.log(http_proxy, apiKey)
        return new ChatGPTAPI({
            apiKey: apiKey,
            fetch: (url, options = {}) =&gt; {
                const defaultOptions = {
                    agent: proxy(http_proxy),
                };
 
                const mergedOptions = {
                    ...defaultOptions,
                    ...options,
                };
 
                return nodeFetch(url, mergedOptions);
            },
        });
    }
    //调用chatpgt 
    chat(text, cb) {
        let that = this
        console.log(&quot;正在向ChatGPT发送提问:&quot;, text)
        that.api.sendMessage(text, {
            conversationId: that.ConversationId,
            parentMessageId: that.ParentMessageId
        }).then(
            function (res) {
                that.ConversationId = res.conversationId
                that.ParentMessageId = res.id
                cb &amp;&amp; cb(true, res.text)
            }
        ).catch(function (err) {
            console.log(err)
            cb &amp;&amp; cb(false, err);
        });
    }
} 
```

注意到除了需要提供`VPN`地址，还需要提供`apiKey`。登录`OpenAI`后，打开链接`https://platform.openai.com/account/api-keys`即可获取`apiKey`，如下图所示

![](img/f8ea8434923c03439e83dcedf4876f58.png)

## 3 虚拟人接入

虚拟人提供商有很多，由于目前我们不仅仅需要有虚拟形象，还需要虚拟人能够说话（`ChatGPT`提供逻辑思维能力）。因此，我们这里直接选用 即构`Avatar`，它内置了文本驱动能力，可以很方便将`ChatGPT`的回复文字内容直接读出来，并有口型匹配。非常适合我们这个场景。

官方提供了非常详细的教程：[官方教程地址](https://doc-zh.zego.im/article/14876?source=CSDNarticle66&login=from_csdn)，本文简单讲解一些关键实现，具体完整代码可以参考文末资源附件。

设置虚拟人形象：
