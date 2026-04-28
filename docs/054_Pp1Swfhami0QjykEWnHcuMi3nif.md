# 鉴权方式说明

> 来源：[https://hyperspace.feishu.cn/docx/UdgKdoMLLoAbu7xEqvAcAnEKnHc](https://hyperspace.feishu.cn/docx/UdgKdoMLLoAbu7xEqvAcAnEKnHc)

目前支持无鉴权与服务级别鉴权两种方式，服务级别鉴权的 token 和 publicKey 通过表单传给平台。

所有插件拿到后token都会在header里的Authorization字段传入插件。

Bearer与Basic 的区别仅在于：

```
* Basic ： 发送token
* Bearer： 发送 &quot;Bearer&quot; + &quot; &quot; + token
```

## 一、token生成过程