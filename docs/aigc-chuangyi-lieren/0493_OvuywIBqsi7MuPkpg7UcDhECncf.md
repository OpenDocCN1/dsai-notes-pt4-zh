# 20240912-OpenAI发布新一代推理模型OpenAI o1

> 来源：[https://aigcreative.feishu.cn/docx/Xyh6dusFMonjknxpxIJceCacnxg](https://aigcreative.feishu.cn/docx/Xyh6dusFMonjknxpxIJceCacnxg)

![](img/73410a6c2dde42722fe73242e3dc4ca3.png)

> OpenAI于2024年9月12日推出了一系列新的推理模型OpenAI o1，旨在通过更长时间的思考来解决复杂问题。

> 这些模型在科学、编程和数学等领域展现出比以往模型更强的复杂任务处理能力。在国际数学奥林匹克竞赛的资格测试中，新模型的解题正确率远高于GPT-4o。

> 同时，OpenAI o1在安全性方面进行了强化，通过新的安全训练方法确保模型遵循安全和对齐准则。

> 此外，还推出了更高效的OpenAI o1-mini模型，专注于编程任务，价格更亲民。

# 官方视频

![](img/f2fcddc9d23cf8539e3745ab641383d8.png)

[Scott Wu_ OpenAI o1 & Coding_1080p.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IEPsblBNhoT3ycxwtGtcTXB5n4d)

![](img/4a7794b3804ced35ec4d52110a07aad8.png)

[Catherine Brownstein_ OpenAI o1 & Genetics_1080p.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DoVrbtIOJoBjWfxp5ENcCclZnsh)

![](img/55e969c01f6ddab5fe18829f7a1d152c.png)

[Korean Cipher with OpenAI o1_1080p.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QxFXbyGmwo2FlAxcOzAc1BTOnkg)

![](img/8059a0748f3e0103f9d3ef3cf672666c.png)

[Mario Krenn_ OpenAI o1 & Quantum Physics_1080p.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/B5oIbysUxoglB4xRm9hc886knSd)

# 能力视频：

![](img/c2a0b67681529e126bd28cb10706a17d.png)

[Coding with OpenAI o1_1080p.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PKFYbyCzhoAxf6xeUl9cC8utnmh)

![](img/30227420366a63ecbff4916a4bccba6e.png)

[Reasoning with OpenAI o1_1080p.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/A0HJbOHtEo8ZftxUdGwcW8MtnWd)

![](img/e7304556774363ff73360d4864c89371.png)

[Counting with OpenAI o1_1080p.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/X8yvbr2PGo32VgxA3RTcPRdgnpf)

![](img/2cd82200e5c1e87223970529c732dd54.png)

[Video Game Coding with OpenAI o1_1080p.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VGq4b4oDfoGWBgxPoJlc6OVRnSb)

![](img/de53e389bb25c8f12789d61a83adc541.png)

[Math with OpenAI o1_1080p.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WNnCbx0MOocWJgxkpJyc4XBtnnd)

![](img/0706d458f60404c11ce18a6303c0f97f.png)

[HTML Snake with OpenAI o1_1080p.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RrYGb2qTpoau3zxlyPLcoqMQntz)

![](img/713a61e0a1487ddc8a98dc1e49f02aa1.png)

[Korean Cipher with OpenAI o1_1080p.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XnFWbblW5oHMduxo6HScbmP8nwf)

# 简介：

W[e've develo](https://openai.com/index/learning-to-reason-with-llms/)ped a new series of AI models designed to spend more time thinking before they respond. They can reason through complex tasks and solve harder problems than previous models in science, coding, and math.

我们开发了一系列新的 AI 模型，旨在花更多时间思考，然后再做出响应。他们可以推理完成复杂的任务并解决比以前的科学、编码和数学模型更难的问题。

Today, we are releasing the first of this series in ChatGPT and our API. This is a preview and we expect regular updates and improvements. Alongside this release, we’re also including

今天，我们发布了 ChatGPT 和我们的 API 中该系列的第一个。这是一个预览，我们期待定期更新和改进。除了此版本，我们还包括 evaluations for the next update, currently in development.

对下一次更新的评估，目前正在开发中。

# 运作方式

We trained these models to spend more time thinking through problems before they respond, much like a person would. Through training, they learn to refine their thinking process, try different strategies, and recognize their mistakes.

我们训练这些模型在问题做出响应之前花更多时间思考问题，就像一个人一样。通过培训，他们学会完善自己的思维过程，尝试不同的策略，并认识到自己的错误。

In our tests, the next model update performs similarly to PhD students on challenging benchmark tasks in physics, chemistry, and biology. We also found that it excels in math and coding. In a qualifying exam for the International Mathematics Olympiad (IMO), GPT-4o correctly solved only 13% of problems, while the reasoning model scored 83%. Their coding abilities were evaluated in contests and reached the 89th percentile in Codeforces competitions. You can read more about this in our technical research post.

在我们的测试中，下一次模型更新的性能类似于博士生在物理、化学和生物学中具有挑战性的基准任务。我们还发现它在数学和编码方面表现出色。在国际数学奥林匹克竞赛 （IMO） 的资格考试中，GPT-4o 仅正确解决了 13% 的问题，而推理模型得分为 83%。他们的编码能力在比赛中得到了评估，并在 Codeforces 比赛中达到了第 89 个百分位。您可以在我们的技术研究帖子中阅读更多相关信息。

As an early model, it doesn't yet have many of the features that make ChatGPT useful, like browsing the web for information and uploading files and images. For many common cases GPT-4o will be more capable in the near term.

作为早期模型，它还不具备使 ChatGPT 有用的许多功能，例如浏览网页以获取信息以及上传文件和图像。对于许多常见情况，GPT-4o 在短期内会更有能力。

But for complex reasoning tasks this is a significant advancement and represents a new level of AI capability. Given this, we are resetting the counter back to 1 and naming this series OpenAI o1.

但对于复杂的推理任务来说，这是一个重大进步，代表了 AI 能力的新水平。鉴于此，我们将计数器重置回 1 并将此系列命名为 OpenAI o1。

# 安全

As part of developing these new models, we have come up with a new safety training approach that harnesses their reasoning capabilities to make them adhere to safety and alignment guidelines. By being able to reason about our safety rules in context, it can apply them more effectively.

作为开发这些新模型的一部分，我们提出了一种新的安全培训方法，该方法利用他们的推理能力使他们遵守安全和对齐准则。通过能够在上下文中推理我们的安全规则，它可以更有效地应用它们。

One way [we measure ](https://openai.com/index/openai-o1-system-card/)safety is[ by testing h](https://openai.com/index/learning-to-reason-with-llms/)ow well our model continues to follow its safety rules if a user tries to bypass them (known as "jailbreaking"). On one of our hardest jailbreaking tests, GPT-4o scored 22 (on a scale of 0-100) while our o1-preview model scored 84. You can read more about this in the system card and our research post.

我们衡量安全性的一种方法是，在用户试图绕过安全规则（称为“越狱”）时，我们的模型继续遵守其安全规则的程度。在我们最难的越狱测试之一中，GPT-4o 得分为 22（0-100 分），而我们的 o1-preview 模型得分为 84。您可以在系统卡和我们的研究帖子中阅读更多相关信息。

To ma[tch the new capabilities ](https://cdn.openai.com/openai-preparedness-framework-beta.pdf)of these models, we’ve bolstered our safety work, internal governance, and federal government collaboration. This includes rigorous testing and evaluations using our Preparedness Framework(opens in a new window), best-in-class red teaming, and board-level review processes, including by our Safety & Security Committee.

为了匹配这些模型的新功能，我们加强了安全工作、内部治理和联邦政府合作。这包括使用我们的准备框架进行严格的测试和评估，一流的红队，以及包括我们的安全与保障委员会在内的董事会级审查流程。

To advance our commitment to AI safety, we recently formalized agreements with the U.S. and U.K. AI Safety Institutes. We've begun operationalizing these agreements, including granting the institutes early access to a research version of this model. This was an important first step in our partnership, helping to establish a process for research, evaluation, and testing of future models prior to and following their public release.

为了推进我们对 AI 安全的承诺，我们最近与美国和英国 AI 安全研究所正式达成协议。我们已经开始实施这些协议，包括允许这些机构提前获得该模型的研究版本。这是我们合作中重要的第一步，有助于建立未来模型公开发布之前和之后的研究、评估和测试流程。

#  适用对象

These enhanced reasoning capabilities may be particularly useful if you’re tackling complex problems in science, coding, math, and similar fields. For example, o1 can be used by healthcare researchers to annotate cell sequencing data, by physicists to generate complicated mathematical formulas needed for quantum optics, and by developers in all fields to build and execute multi-step workflows.

如果您正在处理科学、编码、数学和类似领域的复杂问题，这些增强的推理功能可能特别有用。例如，医疗保健研究人员可以使用它来注释细胞测序数据，物理学家可以使用它来生成量子光学所需的复杂数学公式，所有领域的开发人员都可以使用它来构建和执行多步骤工作流程。

# OpenAI o1-mini OpenAI o1-迷你

The o1 series excels at accurately generating and debugging comple[x code. To off](https://openai.com/index/openai-o1-mini-advancing-cost-efficient-reasoning/)er a more efficient solution for developers, we’re also releasing OpenAI o1-mini, a faster, cheaper reasoning model that is particularly effective at coding. As a smaller model, o1-mini is 80% cheaper than o1-preview, making it a powerful, cost-effective model for applications that require reasoning but not broad world knowledge.

o1 系列擅长准确生成和调试复杂代码。为了向开发人员提供更高效的解决方案，我们还发布了 OpenAI o1-mini，这是一种更快、更便宜的推理模型，在编码方面特别有效。作为较小的模型，o1-mini 比 o1-preview 便宜 80%，使其成为一个功能强大、经济高效的模型，适用于需要推理但不需要广泛世界知识的应用程序。

#  如何使用 OpenAI o1

![](img/f1a1306617e64215f7e41afb7e1a0063.png)

ChatGPT Plus and Team users will be able to access o1 models in ChatGPT starting today. Both o1-preview and o1-mini can be selected manually in the model picker, and at launch, weekly rate limits will be 30 messages for o1-preview and 50 for o1-mini. We are working to increase those rates and enable ChatGPT to automatically choose the right model for a given prompt.

从今天开始，ChatGPT Plus 和 Team 用户将能够访问 ChatGPT 中的 o1 模型。o1-preview 和 o1-mini 都可以在模型选取器中手动选择，在启动时，o1-preview 的每周速率限制为 30 条消息，o1-mini 的每周速率限制为 50 条消息。我们正在努力提高这些比率，并使 ChatGPT 能够自动为给定的提示选择正确的模型。

**ChatGPT En**[terprise and Edu us](https://platform.openai.com/docs/guides/rate-limits/usage-tiers)ers will[ get access to both ](http://platform.openai.com/docs/guides/reasoning)models beginning next week.

从下周开始，ChatGPT Enterprise 和 Edu 用户将可以访问这两种模型。

Developers who qualify for API usage tier 5(opens in a new window) can start prototyping with both models in the API today with a rate limit of 20 RPM. We’re working to increase these limits after additional testing. The API for these models currently doesn't include function calling, streaming, support for system messages, and other features. To get started, check out the API documentation(opens in a new window).

符合 API 使用层 5 条件的开发人员现在可以在 API 中使用这两种模型开始原型设计，速率限制为 20 RPM。我们正在努力在进行额外测试后提高这些限制。这些模型的 API 目前不包括函数调用、流式处理、对系统消息的支持和其他功能。要开始使用，请查看 API 文档。

W**e also **a**re plannin**g to bring o1-mini access to all ChatGPT Free users.

我们还计划为所有 ChatGPTFree 用户提供 o1-mini 访问权限。

# What’s next  下一步

This is an early preview of these reasoning models in ChatGPT and the API. In addition to model updates, we expect to add browsing, file and image uploading, and other features to make them more useful to everyone.

这是 ChatGPT 和 API 中这些推理模型的早期预览。除了模型更新之外，我们还希望添加浏览、文件和图像上传以及其他功能，使其对每个人都更有用。

We also plan to continue developing and releasing models in our GPT series, in addition to the new OpenAI o1 series.

除了新的 OpenAI o1 系列之外，我们还计划继续开发和发布 GPT 系列中的模型。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>