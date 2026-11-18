
# iOS人脸权限

> 来源：[https://docs.feishu.cn/docx/RHESdqNKCoKaKzxSkImcVA5bnMd](https://docs.feishu.cn/docx/RHESdqNKCoKaKzxSkImcVA5bnMd)

- **问：**请教各位大佬一个问题，IOS为什么会有人脸识别权限啊，这好像不在41391 IOS的权限清单里。这好像不在41391 IOS的权限清单里。

- **答1：**41391是可收集个人信息权限。这个是使用faceid端侧验证，不是收集。

- **答2：**想知道人脸验证后服务器会存储吗？如果存储了是不是还是算作收集？

- **答3：**端侧。

- **答4：**跟指纹一样只在你自己的手机里。

- **总结：**face id我个人的理解，只是给app反馈一个结果，0或1，是否通过人脸识别检验，确实不算app收集，区别详见[人脸识别合规要点全梳理](https://mp.weixin.qq.com/s?__biz=MzU1MzAzNzcwNw==&mid=2247490516&idx=1&sn=fe3a6b4c97718b99a2abe8a0a5177311&scene=21#wechat_redirect)。

[权限获取](https://cbe8t1r4a5.feishu.cn/wiki/wikcn5sa8AZhNVbp1RH4oe0Msuh)
