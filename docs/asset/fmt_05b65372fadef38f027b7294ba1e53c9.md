
# 开源 · 聆心智能发布 `CharacterGLM`

> 来源：[https://ai.feishu.cn/docx/SNj8d1HsNora8UxAxK2cxqaqnqb](https://ai.feishu.cn/docx/SNj8d1HsNora8UxAxK2cxqaqnqb)

![](img/b72022d6a1c374a476d73801a383d86b.png)

爱和归属是社交需求的内核，这些需要从社交的对象中获取。

为了实现千人千面的社交对象的可定制化，聆心智能团队开发了用于定制角色进行对话交互的 `CharacterGLM`。`CharacterGLM` 以 `ChatGLM` 模型为底座，具有 6B、12B、66B 的参数量。

我们将对社会开放 `CharacterGLM` 模型的 API 访问，并将 `CharacterGLM-6B` 模型进行开源，以促进 AI 角色扮演及 AI 在心理学中的应用。

此外，我们近期将发布技术报告，披露更多 `CharacterGLM` 模型细节，供学术界以及工业界研究使用。

开源地址：

`https://huggingface.co/LingxinAI/CharacterGLM-6b`

API 调用地址：

`CharacterGLM` API: `https://bigmodel.cn/dev/api#characterglm`

## 性能评估

我们采用人工交互的方式评测 `CharacterGLM` 与业界最强的竞争对手（以下简称 JP）的性能。

我们定制了数十个角色，它们的类型分布于名人类、生活类、游戏影音类和虚拟恋爱类。我们要求标注者与每个角色至少交互 20 轮，每轮对话由两个模型生成回复。

标注者选择胜出的回复继续展开对话，若回复偏好相同则随机选择。我们按角色类别统计两个模型的 win/tie/lose 比率。

结果如表 1 所示，`CharacterGLM` 在大多数类别上的表现均优于 JP。

![](img/e14c0328efaee83e70eac2f948d05567.png)

表 1：不同类别角色的性能对比结果

为了衡量 `CharacterGLM` 在不同话题下的表现，我们进一步将对话话题限定在常见的闲聊、访谈和恋爱场景下进行交互测试，评测方式与上面相同，结果如表 2 所示，`CharacterGLM` 在闲聊和恋爱场景下与 JP 持平，但在访谈场景下显著优于 JP。

![](img/098ae11b1fadab1764464cb18fa84aef.png)

表 2：角色在不同话题下的性能对比结果

长程的对话交互是用户对模型的情绪依赖程度的直接体现，这就要求模型具有良好的建模长程对话的能力。为此，我们进一步分析了 `CharacterGLM` 在不同轮次范围内的角色表现，结果如表 3 所示，`CharacterGLM` 在对话的前期略次于 JP，但随着对话的推进，`CharacterGLM` 的优势则逐渐凸显。

![](img/63a66fd97fb9d7fd403bbff4b7752baa.png)

表 3：`CharacterGLM` vs. JP 建模长程对话的性能对比结果

## 交互样例

下面展示了从交互测评数据中采样的名人类、生活类、游戏影音类和虚拟恋爱类角色的交互样例。

### 名人类角色

![](img/22f9a13f702f00f1228b00028be21d6d.png)

注：JP 竞争产品诱导出了指令对话的格式，完全不像是拟人对话的过程，缺少对话的自然性。

### 生活类角色

![](img/58bedc55b653081fd2271b600aa427e3.png)

注：JP 竞争产品倾向于做出附和、通用化的回复，信息量不足，拟人化程度较低。

### 游戏影音类角色

![](img/bbc3864bce283a37ca7c1c878d7401c6.png)

注：JP 竞争产品没有主动推动剧情发展的意识，难以引起用户的兴趣。

### 虚拟恋爱类角色

![](img/9c80169deb9d3f933ca795be29855857.png)

注：JP 竞争产品在恋爱场景中表现出“低情商、直白”的特性，难以满足该场景下的用户期待。
