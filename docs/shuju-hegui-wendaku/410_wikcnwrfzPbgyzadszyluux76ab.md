# 如何防止“注销-注册”薅羊毛

> 来源：[https://docs.feishu.cn/docx/CKz1dvGPSoo3vpxhjdEcwTMenDf](https://docs.feishu.cn/docx/CKz1dvGPSoo3vpxhjdEcwTMenDf)

**-问**：采用留存销户用户手机号哈希值，在新用户注册时用其手机号的哈希值进行撞库，从而识别出薅羊毛用户，是否存在合规风险？

**-答1**：哈希值是一种加密算法，并未达到匿名化的效果，哈希值依然可以用来校验数据。

**-答2**：个人信息只有在匿名化后才不算个人信息，哈希算法是典型的假名化手段，所以哈希后的个人信息理论上仍为个人信息。所以保留不符合《个保法》对个人信息删除的要求，所以存在合规问题。

**科普**：哈希算法是不可逆算法，是单向的。与之相对应的是DES，AES，RSA都属于可逆的加密技[术，如果拿到了密钥就可](https://mp.weixin.qq.com/s?__biz=MzI5MDQ2NjExOQ==&mid=2247484549&idx=1&sn=e1a031ef3736162ffa4bf3093b4054f4&scene=21#wechat_redirect)逆。对哈希算法的破解需要正向破解，可以使用彩虹表，详见：【利用彩虹表破解Hash】。

[个人信息定义](https://cbe8t1r4a5.feishu.cn/wiki/wikcnwGGoWGzMNhr26Z4UO55xVF)

[技术概念科普](https://cbe8t1r4a5.feishu.cn/wiki/GfWtwCXWbiBzgQk1sxUcfFgXnSP)

[账户注册/注销](https://cbe8t1r4a5.feishu.cn/wiki/wikcn7NWTsw2CDopTUC6JtHBRyf)

[数据存储期限/删除](https://cbe8t1r4a5.feishu.cn/wiki/wikcn8Aer4sxhSSdwFLSX8UQrCb)