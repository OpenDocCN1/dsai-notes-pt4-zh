# 拒绝Cookie后识别用户

> 来源：[https://docs.feishu.cn/docx/AvgUdaoYeo9lZWxwU8XcRDpynZc](https://docs.feishu.cn/docx/AvgUdaoYeo9lZWxwU8XcRDpynZc)

**-问：**当我在网站上选择了拒绝所有Cookie，那为什么我第二次访问的时候，就不再弹出Cookie提示了呢？我理解，既然我选择了拒绝所有cookie，那网站应该无法识别我的身份，所以应该把我当作新用户重新弹窗呀。那这个时候网站是怎么记住我的身份，记住我拒绝Cookie的选择呢？

**-答1：**记**录你状态的这个cookie字段可以划分**到绝对必要cookie里面，所以你reject all cookie不对这个生效。从最小化的角度来说，这个cookie字段可以不用带id的字段，就是一个状态标记。

[技术风险](https://cbe8t1r4a5.feishu.cn/wiki/MYjVw1V1NiYcHOk1OxKchGuUnBb)