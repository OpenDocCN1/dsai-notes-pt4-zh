
# 1🈷️08日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Pg6QdP3ExoHDeEx🐧hOcGZ8Un2f](https://v11enp9ok1h.feishu.cn/docx/Pg6QdP3ExoHDeEx🐧hOcGZ8Un2f)

> 🔔AI情报助手晚报「01月08日」

---

## 1️⃣ 🤖 Claude官方发布的7个提示词技巧

Claude官方更新的提示词最佳实践技巧对GPT、豆包等多数AI工具通用，掌握后用AI效率至少提升2倍。

+ 技巧1：直接清晰表达需求（如目标人群、字数风格），像交代实习生般具体，避免模糊让AI猜。
+ 技巧2：说明需求背后原因（如用途/受众），帮助AI理解意图，输出更贴合实际场景的结果。
+ 技巧3：给AI示例参考风格或结构，尤其适合难用语言描述的需求，多例可让AI快速找规律。
+ 另有让AI分步思考复杂问题、用标签结构化信息、设定专业身份、明确禁忌等实用技巧。

关键词：提示词技巧，Claude官方，AI工具通用，效率提升，结构化提示

### 🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/89Z1YxN6MRa1ZesZmzdZTg)

## 2️⃣ 🤖 OpenAI 前首席科学家 Andrej Karpathy 观点

OpenAI前首席科学家Andrej Karpathy指出，LLM是模拟任何人思维的模拟器，无自身观点，精准模拟强于平均综合。

+ “最强大脑怎么处理X问题”的提问法，在标题、代码、产品、人生问题等场景效果超90%精心设计的长提示词。
+ 该问法激发AI模拟领域强者思维，输出突破用户自身框架，远超规则限制下的最优解。
+ 长提示词是“指导”（有上限），模拟式提问无上限，能解锁LLM更强潜力。
+ 相关开源项目`best-minds`已在GitHub发布，助力提升AI使用效率。

关键词：LLM模拟，提示词优化，开源项目`best-minds`，Karpathy观点，AI能力提升

### 🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/OZRH7uhxpBOVJOL-b474aA)
+ [开源项目`best-minds`](https://github.com/Ceeon/best-minds)

## 3️⃣ 🎯 5个问题测出值不值得学Claude Code

文章通过5个问题（多`CLI`使用、`Opus4.5`解决能力等场景）帮助读者判断是否适合用`Claude Code`平替工具`OpenCode`。

+ `OpenCode`最佳搭配为`OpenCode`+`oh-my-opencode`+`Antigravity`反代，可利用闲置`Codex`/`Gemini`/`Claude`额度且无需独立API。
+ 工具支持同一界面多模型协作（如`Claude`做开发计划、`Gemini`视觉理解、`GPT`调试代码），减少跨AI复制粘贴。
+ `OpenCode`提供免费`GLM`/`MiniMax`模型，上下文用量达70%或85%时自动压缩，适配API费用敏感用户。
+ 反代方案存在封号风险是隐藏雷点；无需多模型或`Gemini`前端能力可暂观望工具成熟度。

关键词：`OpenCode`，`Claude Code`平替，多模型整合，反代方案，AI工具测评

### 🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/VpHMFVpR_z3AgsEK5743Gg)
+ [`OpenCode`安装链接](https://opencode.ai/install)
+ [`Bun`安装链接](https://bun.sh/install)
+ `oh-my-opencode`安装命令

> （注：命令式安装无独立URL，以执行命令形式呈现）

## 4️⃣ 🏆 [Qwen-Image-2512发布，再回顾](https://mp.weixin.qq.com/s/wFT6ObOzadMLu390ux8rHQ) 一下 `Qwen-Image` 技术报告要点

`Qwen-Image`定位为`VL-Omni`体系中的生成模块，与`Qwen2.5-VL`分工协作，注重稳定可控可编辑能力，目标进入真实工作流而非仅展示场景。

+ 采用三段式架构：条件理解层用冻结`Qwen2.5-VL`处理语义，图像表示层优化`VAE`细节重建，生成骨干层用`MMDiT`扩散模型联合建模文本与图像。
+ 引入`MSRoPE`位置编码解决文本1D与图像2D位置冲突，支持高分辨率扩展且无需指定插入位置，提升复杂场景稳定性。
+ 通过系化数据工程提升能力：多阶段过滤数据，区分设计类与自然图像，克制使用合成数据补足长尾字符排版需求。
+ 以课程学习推进中文复杂文字渲染，并将图像编辑作为核心目标，训练多任务实现语义与像素级一致性。

关键词：`Qwen-Image`，图像生成，三段式架构，`MSRoPE`，数据工程

### 🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/wFT6ObOzadMLu390ux8rHQ)
+ `Qwen-Image`技术报告

> （注：文中提及未附直接链接，可关注AINLP👸号回复"image"获取）

## 5️⃣ 🔥 OpenAI前CTO首个创业产品 `Tinker`

`潞晨云微调SDK`基于`TML`开源`Tinker`构建，是国内首个兼容`Tinker`范式的`Serverless`微调平台，按Token计费降低强化学习成本。

+ 算法与算力解耦，开发者仅需定义数据/`Loss`，底层全托管，1名工程师替代多团队协作。
+ `Serverless`架构按有效Token计费，其他环节免费，RL流程成本极低（如`math_rl`跑通仅8.61元）。
+ 覆盖科研、创业、工业级场景，支持`Qwen3`系列及`SFT`/`RL`训练，提供`Cookbook`助力复杂RL。
+ 三步上手：安装SDK→初始化客户端→定义训练循环，像写Python脚本般简单。

关键词：`潞晨云微调SDK`，`Tinker`，`Serverless`微调，按Token计费，强化学习

### 🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/wQMKxRcDTwHSkrYzQBNMDw)
+ [`Tinker` SDK](https://github.com/thinking-machines-lab/tinker)
+ [`DeepSeek-R1`论文](https://arxiv.org/pdf/2501.12948)
+ [`潞晨云微调体验`](https://cloud.luchentech.com/fine-tuning)
+ [`潞晨云微调SDK`文档](https://cloud.luchentech.com/doc/docs/finetune-sdk/)

> （注：摘要总字数控制在200字内，要点简洁突出核心价值与创新点）

## 6️⃣ 🏄 1人顶1个 `Infra` 团队！OpenAI 前 CTO 新招，让大模型训练跌成白菜价

OpenAI前CTO团队推出`Tinker`范式，拆解训练为原语让大模型训练像函数调用，潞晨云发布国内首个兼容该范式的`Serverless`微调SDK。

+ 潞晨云SDK实现算法与基础设施解耦，开发者无需囤显卡，RL流程全透明按Token计价，拒绝算力浪费。
+ 该模式提升人效，1名算法工程师可替代多团队`Infra`工作，灵活构建`SFT`和`RL`训练流。
+ 按Token计费终结算力暴利，RL实验成本极低（如`math_rl`流程仅8.61元），个体开发者也能低成本探索。
+ 支持科研/创业/工业场景，三步上手，提供`Cookbook`复现`SOTA`模型，现开放注册赠30元额度。

关键词：`潞晨云微调SDK`，`Serverless`微调，按Token计费，`Tinker`范式，强化学习

### 🔗 文中引用的链接

+ [`潞晨云注册链接`](https://cloud.luchentech.com/account/signup?invitation_code=XZY)
+ [`潞晨云体验链接`](https://cloud.luchentech.com/fine-tuning)
+ [`使用文档`](https://cloud.luchentech.com/doc/docs/finetune-sdk/)
+ `Tinker` SDK GitHub
+ [`DeepSeek-R1`论文](https://arxiv.org/pdf/2501.12948)
+ [原文链接](https://mp.weixin.qq.com/s/diUzDWP6ZYGkz4Fxh4fVGQ)
