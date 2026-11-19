
# 2023-08-01大幅提升数据集命中预期；AI 联网搜索能力也来了！

> 来源：[https://xuqiwei1986.feishu.cn/docx/UcHxdV41loPpqRxuPnUcQw6TnSd](https://xuqiwei1986.feishu.cn/docx/UcHxdV41loPpqRxuPnUcQw6TnSd)

> 🔗 原文链接：https://mp.weixin.qq.com/s/fp5DBteYZPFtCAbkV3UWeA

> Dify

2023-08-01 09:20:58

精确发文时间由壹伴提供

2023-08-01 09:20 发表于 上海

Dify 的朋友们，

最新的 `V0.3.12` 版本来了！在这个版本中我们推出了两项重要的新功能：进一步优化了数据集的命中策略；上线了「智聊」能力，在与 AI 对话中实现了联网能力。下面带大家一起详细了解该版本的新能力、新特性。

## 🔋

### 数据集新增 Q2Q 匹配模式，大幅提高数据命中预期

在私有数据应用场景中，AI 回答质量取决于数据在分段向量化过程中的索引策略。

为了不断提升 AI 应用实际落地生产的质量与表现，我们一直在探索更精准的命中策略。

在数据集的分段清洗高质量的索引方式中，新增了 `Q&A` 分段模式功能，与普通的 `Q2P`（问题匹配段落内容）匹配模式不同，`Q2Q`（问题匹配问题）匹配模式是当用户提问时，系统会找出与之最相似的问题，然后返回对应的分段作为答案。这种方式更加精确，因为它直接针对用户问题进行匹配，可以更准确地获取用户真正需要的信息。

![](img/c0c46522da3e336a9c5a65df47d258ff.png)

我们以一个包含多条电影信息（标题、时长、内容简介、演职人员名单等）的数据表格为例，询问关于蜘蛛侠这部电影主要讲述了怎样一个故事：

`"Spider-Man: Across the Spider-Verse is a film that primarily focuses on what story?"`

启用 `Q&A` 分段模式功能，精准命中数据集问题和答案并输出：

`"The film 'Spider-Man: Across the Spider-Verse' primarily focuses on the story of Miles Morales, a Brooklyn teen who finds himself becoming Spider-Man and having to team up with other Spider-Men from different dimensions to save the world."`

退出全屏

切换到横屏模式

[🎦 点击观看视频](http://mpvideo.qpic.cn/0bc3saaawaaa6yal4w6h5jsfbegdboiaacya.f10002.mp4?dis_k=70fcdd6a96537e2a05672ec0d5887a7d&dis_t=1691145731&play_scene=10120&auth_info=DcHl45pwR319z53ejntgUzJcYU1JbVh6ODkeTm09Szs/YA==&auth_key=32f66efc6bbb3d5b9bc27c05dca17e57&vid=wxv_3040323997499965442&format_id=10002&support_redirect=0&mmversion=false)

> 无法获取音频链接

视频详情

未启用 `Q&A` 分段模式功能时，提示没有找到相关内容，无法回答：

`"I don't know, but I can help you find the answer. Can you provide some more details about what you are looking for?"`

退出全屏

切换到横屏模式

[🎦 点击观看视频](http://mpvideo.qpic.cn/0b2efeaasaaa2aalzkghyvsfakodbeuqacia.f10002.mp4?dis_k=ede0c3a3c175e9aee80b2010197a9bf3&dis_t=1691145731&play_scene=10120&auth_info=DvHB7G0SeXnNndWOLjBdNQw2Hkg5XHs3b0YbbzkeOTk0&auth_key=5836147eecaa0da86d2d547efd4d31d3&vid=wxv_3040326463046074376&format_id=10002&support_redirect=0&mmversion=false)

> 无法获取音频链接

视频详情

当系统自动以 `Q&A` 分段模式结果不符合预期时，你还可手动修改、增加分段的范围和内容，使得数据集的结构更符合预期，提高回复的精准度。

![](img/d1c9c4dd0e3d4e2f08cff32345fae651.png)

### 🤖 智聊：在对话中联网搜索内容

Dify 的使命，是与开发者共同探索大模型的能力边界，自 OpenAI 推出 `Plugin`、`Code Interpreter`、`Function Call` 等能力以来，开发者已不再满足仅以 `Prompt` 的交互形式使用 AI。智聊使 Dify 向智能 `Agent` 平台迈进了一大步，在智聊中，我们推出了网页浏览、Google 搜索、Wikipedia 查询等一批第一方插件。

用户可以在 `Explore` 中访问智聊，实现诸如：

+   让 AI 回答时间与计算问题
+   分析网页正文内容，作为对话上下文
+   展开 AI 的推理过程

智聊能够作为团队用户的日常生产力工具，以替代 `ChatGPT` 近期下线的联网访问能力。

更重要的是，智聊将会成为 Dify 与开发者共建的一个大模型能力探索实验室，诸如自主 `Agent`、插件开发、多模态等新能力在智聊中完成验证后，会下放至 Dify 的应用编排等功能中。智聊现已支持选择所有具备推理能力的模型，例如 `GPT-3.5`、`GPT-4`、`Claude2`，已推出插件均由 Dify 团队开发并进行了性能优化。

退出全屏

切换到横屏模式

[🎦 点击观看视频](http://mpvideo.qpic.cn/0bc36uaauaaa2ualzewh5bsfb5odbl2qacqa.f10002.mp4?dis_k=85741bf33940bd0d9364471b7aeb66b1&dis_t=1691145731&play_scene=10120&auth_info=Wpvr9vh+SHx+mpzci3JmUzFaMx8bPgwnYjhJGzgyTmpoNQ==&auth_key=177f0188221ad4b8aaa29549093e7c04&vid=wxv_3040327103415631874&format_id=10002&support_redirect=0&mmversion=false)

> 无法获取音频链接

视频详情

### 💡 更多新特性

#### 应用编排调试页：`Prompt` 提示词和变量显示支持折叠

#### 语音转文字 `Service API` 应用内接口文档完善

#### 其他一些 Bugs 修复

版本 `release` 详情请在 GitHub 查阅：`https://github.com/langgenius/dify/releases`

## 📺 往期产品更新 & 最佳实践

[Dify.AI 接入 `Claude2` 模型：支持 100K `token` 上下文，登录即免费获得 1000 次消息额度](http://mp.weixin.qq.com/s?__biz=Mzg5MDkyOTY3NA==&mid=2247484669&idx=1&sn=c19206507d0d2d6fd1042090e48713e1&chksm=cfd45398f8a3da8e25e1030dbd1f809c08978a287d5d7c19442ea54c7dc2b1d92a5f0067b482&scene=21#wechat_redirect)

如何利用 AI 在 5 分钟批量产出 100 篇 SEO 文章？

如何在几分钟内创建一个带有业务数据的官网 AI 智能客服？

## 🥳 如果你喜欢 Dify，欢迎：

+   在 GitHub 上贡献代码，与我们一同共建更好的 Dify；
+   通过线上线下活动、社交媒体向你的朋友们分享 Dify，以及你的使用经验；
+   在 GitHub 上给我们点亮⭐️。

你也可以联系 Dify 小助手，加入我们的好友群聊，相互分享使用经验：

![](img/ee649362baa45732e72672c5e67c88ca.png)
