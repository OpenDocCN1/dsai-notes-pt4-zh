# SDK和App的关系

> 来源：[https://docs.feishu.cn/docx/QJJhdx8Lioo0W5xtEMtcwMlWnqh](https://docs.feishu.cn/docx/QJJhdx8Lioo0W5xtEMtcwMlWnqh)

**-问**：SDK和App的关系又是怎样的呢？

**-答1：**大家想象中的关于觉得二者是父与子关系吧，SDK寄生于App这个属主，应该sdk这个儿子要看App这个老子的脸色吧。。传统中国文化，打是亲来骂是爱。但是不好意思。人家出生在美国，考试考不及格了，都能先打个911，警察陪着小孩回家，顺便经过父母不得打骂，否则直接剥夺抚养权。我们的App和SDK就是这种关系。所以大家做隐私保护碰到SDK特别心累。

在安卓的技术框架内。SDK只要初始化后（出生后），就独立了。他做任何事情，宿主App是干涉不了的。虽然他去获取设备信息，还是必须申请权限。但是他可以做得很隐蔽。判断是否获取权限了，有权限就拿用户信息，没权限就不拿。这样就很隐蔽。但是很明显，违规的。

开发们特别喜欢使用率很高的sdk，比如百度定位，腾讯bugly，腾讯X5 webview，腾讯Tinker。号称sdk四贱客。从IMEI，位置，Mac，应用程序列表，这些敏感信息撸个遍了。为啥工信部的双清单和之前隐私政策，一直在强调sdk清单，根本原因就是宿主自己都不清楚，如果放任的话，风险很大。比如12306集成一个sdk，会偷偷获取用户信息。那基本上全国用户都阵亡了。

[SDK合规](https://cbe8t1r4a5.feishu.cn/wiki/wikcnBRo8jxV2MQwoTa63T6qfce)

[APP](https://cbe8t1r4a5.feishu.cn/wiki/wikcnm7SSi199OdP5q0UAot3Fsf)