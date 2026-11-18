
# 43 万张 H100，Nvidia 将如何分配？丨RTE开发者日报 Vol.15

> 来源：[https://docs.feishu.cn/docx/Wx9Wdi0f3oA7DuxMFvIcAcTWnce](https://docs.feishu.cn/docx/Wx9Wdi0f3oA7DuxMFvIcAcTWnce)

开发者朋友们大家好：

这里是「RTE 开发者日报」，每天和大家一起看新闻、聊八卦。

我们的社区编辑团队会整理分享 RTE (Real Time Engagement) 领域内「有话题的新闻」、「有态度的观点」、「有意思的数据」、「有思考的文章」、「有看点的会议」，但内容仅代表编辑的个人观点，欢迎大家留言、跟帖、讨论。

本期编辑：@Asui，@Olivia

---

## 一、有话题的新闻

1. 华为将正式发布鸿蒙 4，车机系统、多模态交互等领域实现突破

华为官微宣布 HarmonyOS4（下称鸿蒙 4）即将登场，计划将于 8 月 4 日发布。据了解，鸿蒙 4 将在车机系统、多模态交互等领域再次突破，更深度融入 AI 技术。而此次，鸿蒙 4 也有望搭载时下火热的 AI 大模型技术，并进行突破性升级。一位华为工程师透露：华为云已经推出了盘古大模型，HarmonyOS 应用领域的基础底座搭建中已经运用了 AI 大模型。（@证券日报）

2. Vision Pro 开发者套件将配 AirTag 追踪

苹果本周开始让开发者申请 Vision Pro 的开发者套件，用于开发应用。获批参与的开发者将要完成定制流程，同时也会为套件配 AirTag 追踪。Mark Gurman 分享的申请表格透露，开发者在归还套件时，需要解除和 AirTag 的配对使用，这意味着套件中会配着 AirTag 来追踪 Vision Pro 设备。此外，为了避免盗窃，Vision Pro 套件在不使用的时候，需要锁在一个带锁的派力肯箱子里。开发者也必须将套件放在一个安全的地方。报道推测 AirTag 会被配在储存的箱子里，这样一来，被盗窃了还能追踪。（@VR陀螺）

4. “人造太阳”精准放电！DeepMind 实现 AI 可控核聚变新突破

AI 可控核聚变，指日可待。秘密研发 3 年，DeepMind 去年宣称，首次成功用 AI 控制「托卡马克」内部等离子体。其重磅成果登上 Nature。时隔一年，谷歌 AI 团队在这一领域再次取得突破。最新实验模拟中，将等离子体形状精度提高了 65%。这是 AI 可控核聚变新突破，为「人造太阳」精准放电指明道路。（@华尔街见闻）

5. JetBrains 更新 IDE 整合 AI 助手

JetBrains 更新了它的 IDE，整合了 AI 助手。类似微软 Github 的 Copilot，JetBrains AI Assistant 使用的也是 OpenAI 的技术。它与 Github Copilot 插件的区别在于，AI Assistant 深度整合在 IDE 中，支持 AI Chat 等 Copilot 插件不支持的功能。除了 OpenAI 的模型，AI Assistant 也依赖 JetBrains 的内部模型。AI Assistant 的功能因不同语言而存在差异，其中文档生成仅限于 Java、Kotlin 和 Python 项目。（@奇客Solidot）

6. Meta将向微软、亚马逊或谷歌等大型云服务商收取转售 Llama2 的费用

Meta首席执行官马克•扎克伯格在季度收益电话会议上表示，Llama2是开放的，但除了将技术开源外，Meta还包含了一个条款，即最大的云计算公司不会获得免费使用许可，而必须与Meta达成商业协议，“像微软、亚马逊或谷歌这样的公司，基本上都会转售服务，我认为我们应该从中获得部分收入。”扎克伯格说。（@科创板日报）

---

## 二、有态度的观点

1. 联合国教科文组织：学校应禁止使用智能手机

联合国教科文组织（UNESCO）呼吁全球学校应禁止学生使用智能手机，以解决因智能手机带来的管理问题，并借此举措来提高学生学习效果，保护孩子免受网络欺凌。UNESCO 指出，孩子过度使用手机会导致学习成绩的下降，而长期面对电子屏幕也会对儿童的情绪稳定性造成负面影响。同时，智能手机并不能替代现实中的老师完成对学生的面对面教学。教育依然是一项以人为本的事业，过分轻易地拥抱数字技术也许会造成严重的后果。现在也并没有任何具体证据表明数字技术对教育具有明显的积极作用。尽管在新冠大流行期间，由数字技术支持的网络学习为孩子们开辟了更多学习的机会。但网络学习的弊端也十分明显，许多贫困人口实际上并没有能力享受由数字技术带来的学习机会，网络学习需要一定的经济支持，不能覆盖全体人群。目前，纵观全球教育系统，有六分之一的国家已指定禁止在学校使用智能手机的法律与指导。但如果使用得当，数字技术在教育中依然具有它的合理性。UNESCO 认为在制定相关政策时应该制定明确的目标和原则，确保数字技术在教育中发挥应有的益处。（@环球时报）

2. 英特尔CEO：预计个人电脑市场在下半年将持续复苏

英特尔CEO表示，公司有望在2025年达到在行业内领先的目标；个人电脑芯片业务市场份额在二季度有所扩大；预计个人电脑市场在下半年将持续复苏；个人电脑市场的库存水平已恢复正常；个人电脑将是人工智能领域的重要设备，这将使其销量得到推动；服务器市场整体状况喜忧参半；服务器库存消化进程在下半年仍将继续；预计服务器销量在三季度会下滑，四季度将重新增长。（@每日经济新闻）

---

## 三、有思考文章

1. [Nvidia H100 GPU 供需现状：43 万张 H100，Nvidia 将如何分配？](https://mp.weixin.qq.com/s/ClWZVfHLwzHZg8uhfwLHvA) （@中科路1号）

需要 1000 个以上 H100 或 A100 的公司有哪些？市场需要多少个 GPU？各大公司想要多少个 H100？如何选择云厂商？大家在使用什么云？H100 比 A100 快多少？这篇文章短小精悍，没有一句废话，推荐阅读。

2. [无人能构想出人工智能的未来 | 对话伯克利顶级学者 Stuart Russell](https://mp.weixin.qq.com/s/HYCr46dAWUwO_MrZbHftcw) （@CSDN新程序员）

在人工智能学界的经典著作中，这两本书有资格称得上学界圣经。一本是 Ian Goodfellow（生成对抗网络之父）的《深度学习》，还有一本，在业内常被简称为“AIMA”。

这本比砖头还要厚的“AIMA”，全称《人工智能：现代方法》（Artificial Intelligence: A Modern Approach，如图 1 所示）。不少人工智能学科的教授，会在学期必读清单中列上“AIMA”。这部 1995 年首次出版的 AI 读物，至今已经四次重印，成为全球 135 个国家的 1500 多所高等院校使用的教材。

“AIMA”的合著者之一 Stuart Russell（见图 2）是人工智能领域的领军人物，他同时担任加州大学伯克利分校人类兼容人工智能中心（Center for Human-Compatible AI）的创始人和伯克利计算机科学专业的教授。

AI 的发展带来了各种各样的危机讨论，近年来，Stuart Russell 一直为 AI 潜在风险奔走。他不仅签署了埃隆·马斯克 (Elon Musk)、史蒂夫·沃兹尼亚克 (Steve Wozniak) 等人牵头的建议放缓 AI 研究进程公开信，还主动奔赴更多公众可见的场合，以警告 AI 的潜在风险。

CSDN《[新程序员](https://mp.weixin.qq.com/s?__biz=Mzg4NTY4NDk3Mw==&mid=2247533102&idx=1&sn=d6448543671b22451051ba3c4d6c116d&chksm=cfa72dacf8d0a4bab46cda172c4e9bafcdb4bb75217c0af060b187dfc4b1ab2cf4f0343c63f3&mpshare=1&scene=24&srcid=07271SAFeQke2knnblx0LrHR&sharer_sharetime=1690439884381&sharer_shareid=d1b167d3495507aafb88c0b7408107fd&key=20cb312ca49464d6ffdf7cc36518c4d3790167975a2430b7797fb92e49abb6306650e98e3409947ef63edc8055b0b8f63b9521e36ccc762b84e2aa45c5b57ba05d846d66442e27066fec02afe7ebe58283992134b6170429e9273027465b1390dfd0ef50bea105a176c3f6d0b6f2f45d9e4c4a3e7edce6ba60fedc0c4203f03c&ascene=14&uin=OTU0ODI4NDA%3D&devicetype=iMac+MacBookPro18%2C3+OSX+OSX+12.2.1+build(21D62)&version=13060210&nettype=WIFI&lang=zh_CN&countrycode=CN&fontScale=100&exportkey=n_ChQIAhIQeTsZ5FB3W9VbZ52fB0XTMRKMAgIE97dBBAEAAAAAAJW8BsRrV94AAAAOpnltbLcz9gKNyK89dVj02vJqjiJYttK3zmjNIQtCxKdrB1XB6%2BSsWCpCXNVHSXpRxEwihioonB%2BJJfHneCh3fm9fdOrEQ%2Fczu3n1X%2F6yw12ww5FutcqiJD0uZuqAAggnTwx1P59S7OXou%2BFYRXGkUtZtKkM6CAP1j4JV9utcMfGwQFrWC%2FvK0F%2BtR%2B3TuebH3NwIevjb85kCoBHfRTF7SWwMQ9p9NqWiOK23hcFn0oorzf95VmdP7msLyG3jZOw2%2FGIO%2Bj2pM7HliPEs5p1h%2FChIhHsq4oy%2Fymf7gn8pye4XpKM1kkVbA1QFUKsXZYOlpWydxKE%3D&acctmode=0&pass_ticket=UMoY3dZdYJwPxQSqOECLiw6QmlRirKiSgIuPatvwkJG%2BpIGiiYLuvzxuNWZsdhse&wx_header=0)》在智源大会上与 Stuart Russell 教授进行了深入对话，了解他对 AI 技术发展方向的思考与顾虑，并得到了以下回答：

+ 辅助游戏的概念为人机交互设计提供了一种指导原则：即使系统不完全了解人类的具体意图，AI 系统在执行任务时也要优先考虑人类的利益和需求。

+ 我尝试过让经济学家、科幻作家和人工智能研究人员们描绘出一个理想而美好的乌托邦未来，但没人能够做到。实际上，绝大多数人可能并不希望生活在一个凡事都被 AI 取代的世界。

+ 因为我们不了解它们的内部工作原理。所以，我们试图像对待动物一样让大模型提供帮助，这个过程被称为“从人类反馈中进行的强化学习”（RLHF）。

+ AI 系统有能力解决人类偏好中的不确定性问题，它们可以从文献中考察出更加全面的信息，实现人类从未达成的愿景。

---

写在最后：

我们欢迎更多的小伙伴参与「RTE 开发者日报」内容的共创，感兴趣的朋友请通过开发者社区或👸号留言联系，记得报暗号「共创」。

对于任何反馈（包括但不限于内容上、形式上）我们不胜感激、并有小惊喜回馈，例如你希望从日报中看到哪些内容；自己推荐的信源、话题、活动等；或者列举几个你喜欢看、平时常看的内容渠道；内容排版或呈现形式上有哪些可以改进的地方等。

---

![](img/e6253209ed870b9f45569136444edf34.png)
