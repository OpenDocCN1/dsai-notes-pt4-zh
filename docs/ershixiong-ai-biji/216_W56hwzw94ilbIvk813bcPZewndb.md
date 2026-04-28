# NLWeb 的“记忆”机制及其实现方式。

> 来源：[https://qianniucity.feishu.cn/docx/Ney6dBMeVoWDw4xfxlscXZ5ynKg](https://qianniucity.feishu.cn/docx/Ney6dBMeVoWDw4xfxlscXZ5ynKg)

主要内容总结如下：

+   **记忆机制作用**：
在检索前，NLWeb 会判断用户的输入中是否包含需要长期记住的信息（如用户明确要求“记住”某个偏好），以便在后续对话中使用。

+   **实现方式**：

+   通过 DetectMemoryRequestPrompt（在 site_types.xml 中定义）和 memory.py 中的钩子实现。

+   默认情况下，记忆只在当前会话中有效（通过 previous queries 传递），但网站可扩展为长期记忆。

+   记忆检测和其他预检索步骤一样，是通过可定制的 prompt 实现的。

+   **Prompt 示例**：

+   通用 prompt：判断用户是否明确要求记住某事，并提取记忆内容。

+   食谱场景专用 prompt：判断用户是否要求记住饮食限制（如素食、过敏等）。

+   **输出格式**：
检测结果以 JSON 格式返回，包含是否为记忆请求及具体内容。

整体上，NLWeb 通过灵活的 prompt 机制，实现了对用户长期偏好和需求的识别与记忆，支持个性化和上下文相关的对话体验。