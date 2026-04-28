# SSO登录的数据流向

> 来源：[https://docs.feishu.cn/docx/QvuydhAnCozHWwxragFcHuTnn8g](https://docs.feishu.cn/docx/QvuydhAnCozHWwxragFcHuTnn8g)

**-问**：A与B合作，B通过A软件的SSO接口获取用户个人信息。这种场景下实际上是由A向B提供用户信息吗？而不是用户点击同意之后，自己手动输入那些个人信息？

**-答1：**合作的概念可能有点宽泛，A与B有约定数据处理关系嘛，怎么约定的？能举出一个具体点的场景不。

**-答2：**在SSO接口状态下，不需要传递信息，只是依据ticket验证同步登录状态吧？怎么会通过SSO接口来传递用户信息的。

**-追问：**我的理解是，通过SSO接口，同步登陆状态，就会获得用户在A软件的账号（手机号）？

**-答3：**不会获得。获得的只是类似“可以登录”的指令。如果你们跟其他平台的账号互通或者关联信息，那是另外的协议了，跟SSO应该无关。

**-答4：**同意，业务系统获得的信息是这个用户能不能访问我的资源。

**-答5：**[请搜索下 OAth](https://mp.weixin.qq.com/s?__biz=MzkwODMzOTY1NA==&mid=2247498759&idx=1&sn=24addf457e50f1035d0b6d58c9f05c70&scene=21#wechat_redirect) 2.0，可以看看这篇：OAuth2 图解。

[技术概念科普](https://cbe8t1r4a5.feishu.cn/wiki/GfWtwCXWbiBzgQk1sxUcfFgXnSP)

[账户注册/注销](https://cbe8t1r4a5.feishu.cn/wiki/wikcn7NWTsw2CDopTUC6JtHBRyf)