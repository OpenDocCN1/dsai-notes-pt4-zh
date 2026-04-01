# 2023-07-17接入 Claude2 模型：支持 100K token 上下文，登录即免费获得 1000 次消息额度

> 来源：[https://xuqiwei1986.feishu.cn/docx/FKSqdDqkWotzszxBXSrc9tNundc](https://xuqiwei1986.feishu.cn/docx/FKSqdDqkWotzszxBXSrc9tNundc)

> 🔗 原文链接： h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/peIJ7eo1PFh1glFNkDUS8w)q.com/s/peIJ7eo1...

>

Dify.AI Dify

2023-07-17 09:35:10

精确发文时间由壹伴提供

2023-07-17 09:35 发表于 广东

手机阅读

![](img/1d370bc29f49e5e4fef93cc40cd94f4c.png)

Dify 的朋友们，

很高兴告诉大家，我们刚刚发布了 V0.3.9 版本，在这个版本中有两个重要的新特性和大家同步：

+   **Dify 已接入 Antropic 的 Claude2、**C laude-instant 模型。 这意味着：你可以输入 100K token 超长上下文，相当于几百页的文档甚至是一本书！

+

+   已支持在网页嵌入你的 AI 应用。 这意味着，你能够在几分钟内就可以为你的官方网站创建一个具备你业务数据的 AI 智能客服。

+

+    让我们一起来看看吧！

+

![](img/a36f12e5e7240ef739fb9b278534c3b6.png)

##### 接入 Antropic，解锁与 AI 超长对话限制

Antro**pic 在数天前发布了新的 C**laude 2 模型。它支持用户输出 100K token 的上下文，相当于几百页的文档甚至一本书！同时，它可以输出数千 token 的长文本，且具备复杂的推理能力。现在，Dify 已接入 Antropic 的 Claude 系列模型，你在 Dify 上创建一个 AI 应用，并且选择 Claude2 、claude-instant模型就可以体验它的超长文本能力， 不需要进行文本分段和嵌入处理！

> 看看视频示例对话框中输入的 Apple 财报并让它总结财报核心内容👇🏻，共计有 284,375 个字符。

退出全屏

切换到横屏模式

[🎦 点击观看视频](http://mpvideo.qpic.cn/0bc3emabeaaa6qaozbntn5sfai6dcirqaeqa.f10002.mp4?dis_k=3b0d0c18c4350fc8c5664d83ce91815a&dis_t=1691145382&play_scene=10120&auth_info=C9uS6Kp9SVI30+vI+wBRJk9wJDdZYEZIYVx7amETSGo5QA==&auth_key=707eb74e0353993ac47d0960f2ed35d4&vid=wxv_3018649187745316867&format_id=10002&support_redirect=0&mmversion=false)

> 无法获取音频链接

视频详情

**使用步骤：**

1.  创建一个应用；

1.  在 Prompt 调试页面选择 Claude 模型；

1.  在提示词编辑框或消息框内输入超长上下文测试效果；

1.  发布应用！

**我们为所有用户提供了 1000 次免费消息额度。马上登录 Di**fy 体验 Claude 的超能力吧！ 试用额度消耗完后， 如果你已经获得 Antropic 的 API ，可以通过【设置】--【模型供应商】 -- 选择 Antropic，填入 key 即可使用自己的 key。

##### **在网页中嵌入 AI 应用，一个官网 A**I Chatbot 只需几分钟就能搞定！

几乎每个业务官网都需要一个客服窗口来为用户提供智能问答。通过简单的嵌入代码复制粘贴到你的网站，你就可以在几分钟内定制具备你业务数据的 AI Chatbot，并且呈现在你的官网上。

![](img/2c568c073813b4ff00907874ad9d22cc.png)

**使用步骤：**

1.  创建一个数据集，上传你的业务数据；

1.  创建一个对话型应用，在 prompt 编排页添加上下文，选择目标数据集；

1.  点击【概览】--【嵌入】复制嵌入代码；

1.  粘贴到你网站的目标位置；

1.  更新后，即可以得到一个拥有你业务数据的官网 AI 客服。

##### **更多特性**

除了以上，我们在本次更新中还增加了其他特性：

+   文本型应用批量生成结果后，支持导出为文件

适用于需要批量生成同样规格的内容，如：针对 100 个 SEO 主题，生成 100 个文章标题和文章，你不需要一次次运行，只需要导入 CVS 文件后批量执行，然后导出文件。效率工作小技巧！

+   社区版本新增通过邀请链接邀请成员并激活的功能

+   Service API ：对话类型应用支持删除会话

+   **其他 Bugs 修复**

你可以在 GitHub 上查看最新的 changelog：https://github.com/langgenius/dify/releases/

期待你在 Dify 上创建更多惊喜的 AI 应用！

**如果你喜欢 Dify，欢迎：**

+   在 GitHub 上贡献代码，与我们一同共建更好的 Dify；

+   **通过线上线下活动、社交媒体向你的**朋友们分享 Dify，以及你的使用经验；

+   在 GitHub 上给我们点亮⭐️。

你也可以加入我们的好友群，相互分享使用经验：

![](img/322d2178f36c1bfa68fd14143dd342c7.png)