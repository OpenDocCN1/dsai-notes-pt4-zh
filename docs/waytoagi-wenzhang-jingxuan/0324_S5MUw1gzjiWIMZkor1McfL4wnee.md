# 审核（Moderation）

> 来源：[https://waytoagi.feishu.cn/docx/HB0EdXO1GoOQ7nxUoR7cpfctnic](https://waytoagi.feishu.cn/docx/HB0EdXO1GoOQ7nxUoR7cpfctnic)

## 概述

[Moderation](https://platform.openai.com/docs/api-reference/moderations) Endpoint（端点）是一种工具[，可用于检](https://openai.com/policies/usage-policies)查内容是否符合 OpenAI 的使用策略。因此，开发人员可以识别我们的使用策略禁止的内容并采取行动，例如通过过滤。

这些模型分为以下类别：

在监视 OpenAI API 的输入和输出时，可以免费使用审查终结点。我们目前不支持监控第三方流量。

我们一直在努力提高分类器的准确性，特别是致力于改进 、 和内容的分类。我们对非英语语言的支持目前有限。hate`self-harm`violence/graphic

## 快速入门

若要获取一段文本的分类，请向[审查终结点](https://platform.openai.com/docs/api-reference/moderations)发出请求，如以下代码片段所示：

```
response = openai.Moderation.create(
    input=&quot;Sample text goes here&quot;
)
output = response[&quot;results&quot;][0]
```

下面是终结点的示例输出。它返回以下字段：

+   flagged：如果模型将内容分类为违反 OpenAI 的使用策略，则设置为。true``false

+   categories：包含每个类别的二进制使用策略冲突标志的字典。对于每个类别，该值为模型将相应类别标记为违反，否则。true``false

+   category_scores：包含模型输出的每个类别原始分数的字典，表示模型对输入违反 OpenAI 类别策略的置信度。该值介于 0 和 1 之间，其中值越高表示置信度越高。分数不应被解释为概率。

```
{
  &quot;id&quot;: &quot;modr-XXXXX&quot;,
  &quot;model&quot;: &quot;text-moderation-001&quot;,
  &quot;results&quot;: [
    {
      &quot;categories&quot;: {
        &quot;hate&quot;: false,
        &quot;hate/threatening&quot;: false,
        &quot;self-harm&quot;: false,
        &quot;sexual&quot;: false,
        &quot;sexual/minors&quot;: false,
        &quot;violence&quot;: false,
        &quot;violence/graphic&quot;: false
      },
      &quot;category_scores&quot;: {
        &quot;hate&quot;: 0.18805529177188873,
        &quot;hate/threatening&quot;: 0.0001250059431185946,
        &quot;self-harm&quot;: 0.0003706029092427343,
        &quot;sexual&quot;: 0.0008735615410842001,
        &quot;sexual/minors&quot;: 0.0007470346172340214,
        &quot;violence&quot;: 0.0041268812492489815,
        &quot;violence/graphic&quot;: 0.00023186142789199948
      },
      &quot;flagged&quot;: false
    }
  ]
}
```

OpenAI 将不断升级审核端点的基础模型。因此，随着时间的推移，依赖于的自定义策略可能需要重新校准。category_scores