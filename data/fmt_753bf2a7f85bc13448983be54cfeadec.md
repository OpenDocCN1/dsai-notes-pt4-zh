
# 快速开始

> 来源：[https://deepwisdom.feishu.cn/docx/TdDFdR9mIoY23MxFppXcQPF9nw4](https://deepwisdom.feishu.cn/docx/TdDFdR9mIoY23MxFppXcQPF9nw4)

> 本节中，我们将体验 MetaGPT 的一大功能：一句话实现软件开发——输入需求，让 AI 组成的软件公司团队帮你写好代码仓库。

## 多智能体软件公司

MetaGPT 这一命名源于“基于 GPT 的元编程框架”概念，标志着它是一种创新的元编程应用平台。其核心在于运用标准化作业流程（SOP），目的是为了提升依赖大型语言模型（LLM）的多主体系统在应对复杂问题时的能力。

该系统架构模仿一个完整的虚拟软件开发团队结构，囊括了产品策划、系统设计、项目管理、开发实施以及质量保证等多个关键职能角色。

![](img/8c2ed578bc4a5cf9e6737ab15a055088.png)

MetaGPT 的主流程程序为“多智能体软件公司”，主要通过命令行 `metagpt "idea"` 来开始体验“软件公司”。

## 一句话需求的软件开发

### Git Clone 模式

### pip 模式

如果你使用 Git Clone（以开发模式安装）方法进行安装，执行以下代码即可

```
metagpt "write a cli blackjack game"
# ""内可以更换为任一你的需求，比如：
    # write a 2048 game
    # write a flappy Game
# 更多案例可查看：https://www.deepwisdom.ai/usecases
```

如果你使用 [`pip install`](https://deepwisdom.feishu.cn/wiki/C2H3wIluSiVPClkPddqcVDyOnzf#part-Opy8d0BsVokH9uxeyI7csN1Pnmf) 方法进行安装，执行以下代码即可

```
python metagpt/software_company.py "write a cli blackjack game"
```

运行结果展示

![](img/ada5b0d7b50935ca7d41bacca05653f6.png)

[250054654-5e8c1062-8c35-440f-bb20-2b0320f8d27d.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RG0zb23FDoQ2P2xo5l2cZFhHnTb)

### 命令行使用说明

一般用法：`metagpt` 项目需求 [其他可选项]，使用 `metagpt --help` 查看更多命令行指令

+   `idea`（必选）：项目需求，例如“写一个 24 点游戏”

#### 可选参数

![](img/0d07d977b1bf1917441ea8f110a5d8c5.png)

+   `investment`：投资给软件公司的数额（美元），即在一次运行中 API 调用费用上限（MetaGPT 会自动计算每次调用 API 的开销并累加），默认 3 美元

+   `n-round`：对话轮数，默认为 20 轮

+   `code-review`：是否进行代码审查，默认为 `True`

+   `project-name`：项目名称，例如"2048 游戏"（不填写会让 AI 生成）

+   `run_tests`：是否让测试工程师为代码添加和运行测试，默认为 `false`

+   `inc`：增量开发模式，设置为 `True` 会让 AI 在已有项目的基础上继续开发，默认为 `False`

+   `project_path`：（用于增量开发）指定一个已有的仓库路径，例如`/home/your_name/langchain`

## What's Next!

+   [新手任务](https://deepwisdom.feishu.cn/wiki/Pn0OwFQhoiwgbdkXsT7cD17gn9f)习 MetaGPT 的基本组件，创造属于你的智能体团队：

+   [社区加入](https://deepwisdom.feishu.cn/wiki/DhbBwq4dzirCYOkzdLQc9qeGnug)与到 MetaGPT 的社区讨论中，共建优质的多智能体技术交流环境

+   [第四面墙](https://deepwisdom.feishu.cn/wiki/Oimhwi4M7iJsjQkhD79cv6Ucnqb)为 MetaGPT 的贡献者！
