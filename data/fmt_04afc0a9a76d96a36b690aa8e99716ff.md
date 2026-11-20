
# 12月02日 AI情报助手早报

来源：[https://v11enp9ok1h.feishu.cn/docx/ITLbd5161ob4tmxDUVuch3AunKb](https://v11enp9ok1h.feishu.cn/docx/ITLbd5161ob4tmxDUVuch3AunKb)

## 1️⃣ NotebookLM 接入 Nano Banana Pro

NotebookLM 接入 Nano Banana Pro 图像模型后生成的 PPT 质量专业，可用于产品路演、职场汇报等正式场合，虽有小瑕疵但瑕不掩瑜。

生成 PPT 有三种玩法：懒人一键生成、指定风格（如瑞克和莫蒂风）生成、借助第三方工具 Lovart 编辑修改元素。

NotebookLM 不仅能生成 PPT，还是一站式知识管理工具，支持多模态转换（音频、视频、思维导图等）。

其优势在于深度理解上传资料内容，按要求输出可用结果，是 Nano Banana Pro 的实用落地场景。

文章强调 AI 应辅助思考而非替代，NotebookLM 的设计哲学提醒用户重视创作体验与知识掌握程度。

关键词：NotebookLM，Nano Banana Pro，AI 生成 PPT，多模态转换，创伴 Creaibo

### 引用链接

- [原文章链接](https://mp.weixin.qq.com/s/KSzEDiyZnk0god9i411RIQ)
- 通往 AGI 之路视频号直播回放
- WaytoAGI 实用指北

## 2️⃣ DeepSeek V3.2 正式版发布

深度求索发布 V3.2 正式版（全渠道可用）及 Speciale 实验版（临时 `API` 至 12.15）。

V3.2 验证 DSA 稀疏注意力有效性，推理长度较 Kimi-K2-Thinking 显著降低。

推理 benchmark：V3.2≈GPT5，Speciale≈Gemini3 Pro（长思考+Math V2 定理证明）。

Speciale 获 ICPC 第二/IOI 第十名，但 Token 消耗高，暂不支持工具调用与日常优化。

V3.2 首次支持思考模式工具调用，也支持 Claude Code 思考模式（`deepseek-reasoner`）。

关键词：DeepSeek V3.2，大模型推理，DSA 稀疏注意力，工具调用，定理证明

### 引用链接

- DeepSeek V3.2 发布详情

## 3️⃣ C3 文本压缩方案

[超越 DeepSeek-OCR 光学压缩：C3](https://mp.weixin.qq.com/s/VpZc93vABBZfjQZiG703AQ) 重构纯文本压缩，40 倍压缩精度仍达 93%。

C3 提出纯文本域压缩方案，跳过视觉中介，采用双 LLM 级联架构（小 LLM 压缩、大 LLM 解码），引入可学习上下文查询嵌入，将长文本压缩为固定长度潜在 token。

性能远超 DeepSeek-OCR：20x 压缩精度 98%（后者 60%），40x 仍 93%，错误呈序列性衰减类似人类记忆。

应用于超长上下文处理、多模态扩展、下一代模型组件等，降低大模型计算成本。

C3 未取代 DeepSeek-OCR，而是提供文本压缩上限参考，为光学压缩优化指明方向。

C3 代码、模型开源（GitHub、Hugging Face），论文发布于 arXiv。

关键词：C3，文本压缩，双 LLM 级联，开源，性能突破

### 引用链接

- [论文链接](https://arxiv.org/pdf/2511.15244)
- [代码链接](https://github.com/liufanfanlff/C3-Context-Cascade-Compression)
- [模型链接](https://huggingface.co/liufanfanlff/C3-Context-Cascade-Compression)

## 4️⃣ ChatGPT 三周年

ChatGPT 于三年前今日发布，开启生成式 AI 范式，深刻改变世界与人们的人生轨迹。

过去三年拥抱 AI 的人收获兴奋与幸福感，刚起步参与 AI 也为时不晚。

当前正处于 AI 领域"八方来财、大展鸿图"的黄金机遇期。

文章鼓励人们抓住 AI 发展时机，积极融入生成式 AI 浪潮。

ChatGPT 的发布标志着生成式 AI 时代到来，对全球产生深远影响。

关键词：ChatGPT，生成式 AI，AI 机遇，生成范式，人工智能

### 引用链接

- [原文链接](https://mp.weixin.qq.com/s/BvqP7-KbnOSDWTPWGR79cw)

## 5️⃣ Claude Code 保姆级入门

[Claude Code 保姆级入门：不会命令行也能用，我把 5 个必学技巧全整理好了](https://mp.weixin.qq.com/s/0d8x55t1t-SWQzG7jck7Pw)

Claude Code 是 Anthropic 官方代理式编程工具，具备智能体思考规划、强大工具生态及可持续进化知识库三大核心优势。

安装推荐 Trae+终端组合，通过 Trae 助手提示词可自动检查环境并安装 `CLI`，支持多系统。

CC Switch 开源工具一键切换国产模型（如 GLM、MiniMax），成本降至原 1/10 且无网络问题。

必学技巧：危险模式、`Plan Mode`、图片输入、深度思考（`ultrathink`）、即时停止（`ESC`）。

国产模型成本降 90%，英语不好也能用，安装问题可通过 Trae AI 快速解决。

关键词：Claude Code，代理式编程工具，CC Switch，国产大模型，编程技巧

### 引用链接

- [原文章链接](https://mp.weixin.qq.com/s/0d8x55t1t-SWQzG7jck7Pw)
- CC Switch GitHub 地址
- [CC Switch 下载链接](https://github.com/farion1231/cc-switch/releases)
