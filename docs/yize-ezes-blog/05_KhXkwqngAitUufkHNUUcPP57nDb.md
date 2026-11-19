
# 关于 LLM、Prompt、Agent 的概念理解

> 来源：[https://chatmemo.feishu.cn/docx/S6iKdQCLkonIEVxWtkLcsnZTn3c](https://chatmemo.feishu.cn/docx/S6iKdQCLkonIEVxWtkLcsnZTn3c)

<https://chatmemo.feishu.cn/sync/OMPXdEososhwsAbAxYRcJSCknob>

---

> 我有个朋友今天刚好问我：GPT 和 Agent 有什么关系？Agent 一定比 GPT 好吗？

> 把这个问题精准一点描述，其实是对比“大模型 LLM 和 AI Agent 的区别是什么”。

> 我从业务应用的角度，谈一下自己的理解，也欢迎交流讨论。

放在现在（2024 年）这个背景下，我们说的 Agent 概念，基本都在指代 LLM-based Agent。现在 Coze、Dify，无不是基于大模型来驱动 AI Agent 的。

光论 LLM 本身，为了方便理解，可以视为“看起来有人类智能的文本处理工具”。其本质是根据用户输入的信息，从概率上来猜测，后续生成哪个字、哪一段话是用户最希望看到的。

比如：我让 AI 帮我写一篇小说，输入了“请输出小说大纲”的信息，对此 LLM 做的思考就是：

+   因为：人类小说中，大部分的小说大纲是怎么样的

+   所以：我现在应该输出 xx 形式的大纲

所谓 Prompt，我们一直在说，“大模型在基于上下文记忆，来推测最合适的输出”，也是这么回事。

而 LLM 智能的文本处理能力，使得其在人类输入任务指令后，可以生成类似人类智能的决策方案文本。这些LLM 生成的自然语言，在实际的应用中，可以被解构为 `json` 字段信息、或者一些用于 `fuction call` 的指令。

于是，LLM 就很适合作为 AI Agent 的驱动核心，被用于感知获取外界信息、行为决策、工具（AI Agent 的插件等）使用等环节。

另外， AI Agent 也并非一定要堆叠很多插件：仅靠一段 Prompt + LLM，帮助用户自动写篇小说大纲，是个 Agent；封装了数个插件 + Prompt + 工作流，帮助人类筛选每日新闻，生成新闻简报的智能体，也同样是个 Agent。

为了便于理解，简单化描述：

1.  Prompt 是大模型用于推测后续生成内容的“上下文”（当然，用户输入、模型输出，都是 prompt，它们共同构成了上下文）

1.  LLM 是大模型，是推测、生成文本的模型

1.  AI Agent 是代替人类完成任务的“智能体”

所以，回到原题，在 2024 年的讨论背景下，LLM 是实现 AI Agent 的一个关键部分。一方面可以作为 AI Agent 的“大脑”，提供理解问题、响应执行的思考与指令；另一方面也可以作为 AI Agent 的一个智能文本处理环节，执行例如“总结、润色、翻译和方案编写”等文本生成的任务。

说到底，这两年吹的神乎其神的 Prompt 工程、AI 魔法咒语，都是让 AI 代替人类完成一些工作任务。改称为 Agent 工程，反倒可能更加精准。

<https://chatmemo.feishu.cn/sync/OXhxdlqqcsuEkObLUgsceuRinef>
