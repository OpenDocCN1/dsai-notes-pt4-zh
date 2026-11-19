
# SDK自启动的处理

> 来源：<https://docs.feishu.cn/docx/NgFkdAFnMo2tlOxxC1McEAXQnVd>

## 问

请问关于SDK有限自启动的场景，目前仅在隐私政策中告知，不更改返回值的风险是否不高呢？目前我这边看到的是jpush的问题。

## 答1

我们内部扫出来，最开始是让隐私政策披露，最后SDK直接升级一下就没有自启动的问题了。我们也没能力让SDK改，只是很多时候用的老版本SDK也没有更新，最后更新了版本就解决了，他们可能根据其他地方的反馈自己改好了吧。

## 答2

Jpush 小米自启动的问题？极光有方案的开发自己改就好。https://go48pg.yuque.com/go48pg/ub4kdr/lgvzg8?singleDoc#，看一下这个方案，如果升级困难或版本迭代时间过长就考虑这个方案。

> 旧版本小米SDK`com.xiaomi.push.service.receivers.NetworkStatusReceiver`会配置`android.net.conn.CONNECTIVITY_CHANGE`需要移除

## 答3

隐私政策做到全面告知，为什么隐私政策会越来越长，其一个原因就是要想用户做到全面的告知，那么为了保险起见，就出现了越长越好。

## 答4

我给我们开发老师建议SDK披露字段是取扫出来字段+SDK隐私政策披露字段的并集。

## 总结

[总结：对于SDK还是等待系统](http://mp.weixin.qq.com/s?__biz=MzU1MzAzNzcwNw==&mid=2247489337&idx=1&sn=fd6dedc03e71f62dd45ca3c6c448630c&chksm=fbf9adddcc8e24cb07e5b04e85660518c125442e0ce2bb1c5e2e48860827f66ee9b6de1810b3&scene=21#wechat_redirect)方的整治吧，详见：苹果APP隐私新政简介与分析。

[SDK](https://cbe8t1r4a5.feishu.cn/wiki/wikcnBRo8jxV2MQwoTa63T6qfce)
