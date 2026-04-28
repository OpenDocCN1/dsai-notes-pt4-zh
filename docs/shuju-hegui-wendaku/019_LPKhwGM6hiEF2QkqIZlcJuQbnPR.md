# 多平台隐私政策管理

> 来源：[https://docs.feishu.cn/docx/LSHsdNEvloa0HtxM71ccTDdxnvK](https://docs.feishu.cn/docx/LSHsdNEvloa0HtxM71ccTDdxnvK)

**-问：**我们在和IT聊隐私协议管理平台，请问大家的经验，如果有很多不同类别app需要管理，怎么设计产品逻辑，既能灵活修改文本，又能方便审批？现在的方案是仅仅给一些变量修改，实操估计难满足需求。

**-答1：**跟数据处理活动记录关联。变量其实主要就是数据处理活动+用户权利实现，其他都是规定的。

**-答2：**所以我理解要根据app功能（数据处理活动）设置模块，让开发填写？

**-答3：**作为**隐私政策管理工具应该是把处理活动作**为变量，然后把处理活动作为一个库进行调用。

**-答4：**版本管理可以参考github。

**总结：同意回答3观点。**

[隐私政策](https://cbe8t1r4a5.feishu.cn/wiki/wikcndbVz8DuLqHAs77Bx6bp4zd)