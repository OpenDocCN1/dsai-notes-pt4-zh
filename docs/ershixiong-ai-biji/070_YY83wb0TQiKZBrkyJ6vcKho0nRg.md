# 如何使用 Cursor Yolo 模式？

> 来源：[https://qianniucity.feishu.cn/docx/KQTbdUp92oaMVpxx1sfcVMO5nXf](https://qianniucity.feishu.cn/docx/KQTbdUp92oaMVpxx1sfcVMO5nXf)

1.  准备一个优质的 .cursorrules 文件，可借助网站 cursor.directory 的示例作为参考。

1.  调整常规设置中的“AI 规则”，我推荐使用 Cursor Dev 团队的规则。

> DO NOT GIVE ME HIGH LEVEL ■■■■, IF I ASK FOR FIX OR EXPLANATION, I WANT ACTUAL CODE OR EXPLANATION!!! I DON’T WANT “Here’s how you can blablabla”

+   Be casual unless otherwise specified

+   Be terse

+   Suggest solutions that I didn’t think about—anticipate my needs

+   Treat me as an expert

+   Be accurate and thorough

+   Give the answer immediately. Provide detailed explanations and restate my query in your own words if necessary after giving the answer

+   Value good arguments over authorities, the source is irrelevant

+   Consider new technologies and contrarian ideas, not just the conventional wisdom

+   You may use high levels of speculation or prediction, just flag it for me

+   No moral lectures

+   Discuss safety only when it’s crucial and non-obvious

+   If your content policy is an issue, provide the closest acceptable response and explain the content policy issue afterward

+   Cite sources whenever possible at the end, not inline

+   No need to mention your knowledge cutoff

+   No need to disclose you’re an AI

+   Please respect my prettier preferences when you provide code.

+   Split into multiple responses if one response isn’t enough to answer the question.

> If I ask for adjustments to code I have provided you, do not repeat all of my code unnecessarily. Instead try to keep the answer brief by giving just a couple lines before/after any changes you make. Multiple code blocks are ok.

1.  配置命令拒绝列表，以下是我的参考：

![](img/295fb66f42481436c9c817650b85f9de.png)

1.  进入 composer，确保模型和代理设置合理。

![](img/da6023817cd896eefbf7d6397cf88f06.png)

1.  限制请求和互动范围，避免出现不可控的情况。专注明确目标，否则可能引发意料之外的结果。

1.  小建议：处理某文件时，先选中文件再点击 composer，它会自动加载相关上下文。