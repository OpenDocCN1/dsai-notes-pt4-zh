# OpenID是个人信息吗

> 来源：[https://docs.feishu.cn/docx/XA6yd5mdJoYdTQxnvCacKvQvn7b](https://docs.feishu.cn/docx/XA6yd5mdJoYdTQxnvCacKvQvn7b)

**-问：**微信的OpenID算个人信息吗，要加密存储吗，感觉大家都觉得它不算个人信息，太难了

**-答1：**是**个人信息，非敏感个人信息**，控制措施看内部要求。OpenID是可变的。

**-答2：**之前黄par和宋老师线上分享的时候讲过，算个人信息，但合规责任相比设备id可能会稍弱一些。可以看看最近工信部通报的违规收集清单里面，有没有加上平台id哦。

**-答4：**特定场景下，**结**合session key就可以获取电话号码，这时候就是敏感信息。有些非用户授权的场景，可以获取头像，昵称，性别，等基础信息。