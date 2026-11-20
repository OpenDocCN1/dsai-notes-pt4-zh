
# 输出内容对外分享

> 来源：[https://docs.feishu.cn/docx/XzBWdQMbuoe2Qbxv6GuclCqgng7](https://docs.feishu.cn/docx/XzBWdQMbuoe2Qbxv6GuclCqgng7)

## 法律规定

> 《生成式人工智能服务管理暂行办法》
>
> **第十二条** 提供者应当按照《互联网信息服务深度合成管理规定》对图片、视频等生成内容进行标识。
>
> 《互联网信息服务深度合成管理规定》
>
> **第十七条** 深度合成服务提供者提供以下深度合成服务，可能导致公众混淆或者误认的，应当在生成或者编辑的信息内容的合理位置、区域进行显著标识，向公众提示深度合成情况：
>
> ……
>
> 具体标识方法可参见《网络安全标准实践指南——生成式人工智能服务内容标识方法》

## 案例

| **模型** | **分享方式** | **生成内容标识** | **用户个人信息保护** |
| --- | --- | --- | --- |
| Kimi | 文字 | ❌ | ✅ |
|  | 图片 | ❌ | ❌ |
|  | 链接 | ✅ | ✅ |
| 文心一言 | 链接 | ✅ | ❌ |
| 通义千问 | 链接 | ✅ | ✅ |
| 豆包 | 图片 | ✅ | ✅ |
|  | 链接 | ✅ | ✅ |
| 腾讯元宝 | 图片 | ❌ | ❌ |
|  | 小程序码 | ❌ | ❌ |
|  | 链接 | ❌ | ❌ |
| 智谱清言 | 链接 | ✅ | ✅ |

### Kimi

沟通时会显示特制的头像和昵称

三种分享方式：文字、图片和链接

文字：

用`user`来指代用户，分享的文字内容不包含用户账号的信息。

图片：

![](img/efce745416cee4c810015440aac1e944.png)

链接：

![](img/80244cdb7b23b4def9934c0ec8b5e9a4.png)

![](img/6bdb90b16e1a14c4606abac997a3a3fe.png)

![](img/c0565ac5a08982cb0690de7de9851fd6.png)

原

其他人未登录状态

其他人已登录状态

点击链接查看和 Kimi 智能助手的对话 <https://kimi.moonshot.cn/share/cq4khopldu5rks07icvg>

### 文心一言

沟通时没有头像和昵称

只能链接

带有用户名（很羞耻的）和水印

![](img/d0f6acbd6011f8529e48c6dc5bdf92b2.png)

<https://yiyan.baidu.com/share/iT47wnsbOu>

### 通义千问

沟通时没有头像和昵称

只有链接

![](img/f6f7b4631758c8f39cc01f9473f1c570.png)

<https://tongyi.aliyun.com/qianwen/share?shareId=97e3322e-8ed4-4397-9547-6d73447fce70>

### 豆包

首先会弹出一个弹窗

有提示

分享内容被收录？没懂

![](img/b45a655e899aa0ceed598771d63722dd.png)

![](img/57842e04eafbbc6dc3353b881cbebdf8.png)

![](img/738d356a5417f57a74c83a2e3bacd29c.png)

<https://www.doubao.com/thread/w4826e1550967701d>

图片

复制链接

都有大模型生成内容豁免的提示词

### 元宝

有三种方式：链接、小程序码、图片

![](img/71806bc65557ee4b36c7e2e21916212a.png)

链接

<https://yuanbao.tencent.com/bot/app/share/chat/95725126e02c3125bc1f349b353d139f>

保留了头像和昵称

没有生成内容的标注

小程序码

小号一直获取不了分享内容，猜测是小号没有绑定元宝账号……那这个设计也太愚蠢了吧……

同样的毛病

![](img/67ce277a1a9e325561003d8848e822d5.png)

图片

### 智谱清言

链接分享

![](img/ce0a43e520d7364bdf76b6ae04a80d6b.png)

有标注

做了匿名化

<https://chatglm.cn/share/FE275>

### ChatGPT

跳出弹窗

![](img/9bc1762fa57fe79f1a0576e0e6be8958.png)

[ChatGPT Shared Links FAQ](https://help.openai.com/en/articles/7925741-chatgpt-shared-links-faq)

### Gemini
