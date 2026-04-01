# 李继刚《如何写好Prompt》系列

> 来源：[https://aigcreative.feishu.cn/docx/FCL7dAcU7ohujwxpiercYdh7ncd](https://aigcreative.feishu.cn/docx/FCL7dAcU7ohujwxpiercYdh7ncd)

> 作者：李继刚

> 链接：https://www.lijigang.com/posts/chatgpt-prompt-structure/

# 结构化

## **背景**

最早接触 Prompt engineering 时, 学到的 Prompt 技巧都是:

+   你是一个 XX 角色…

+   你是一个有着 X 年经验的 XX 角色…

+   你会 XX, 不要 YY..

+   对于你不会的东西, 不要瞎说!

+   …

对比什么技巧都不用, 直接像使用搜索引擎一样提问, 上面的技巧对于回复的效果确实有着 明显提升. 在看了 N 多的所谓 “必看的 Prompt 10 大技巧” “价值 2 万元的珍藏 Prompt” 后, 发现大家都在上面这些技巧上打转. 直到有一天, 在 Github 上看到了 JushBJJ/Mr.-Ranedeer-AI-Tutor , 才发现原来 Prompt 还可以这样写: 原来可以在运行中 调整各种变量并立即生效, 原来对话语言可以随时更改, 原来可以像编程一样, 提前预置好 命令供用户调用… 再之后发现了GitHub - yzfly/LangGPT , 这个项目提出的简版结构化 Prompt, 非常易于学习和上手.

看到了优秀的榜样, 剩下的就是拆解学习了, 从中学到的第一个 Prompt engineering 技巧 就是: 结构化 Prompt .

## **什么是结构化?**

结构化: 对信息进行组织, 使其遵循特定的模式和规则, 从而方便有效理解信息.

– by GPT 4

从上面的 Prompt 中最直观的感受就是 结构化 , 将各种想要的, 不想要的, 都清晰明确地 表述在设计好的框架结构中:

+   语法

+   这个结构支持 Markdown 语法, 也支持 YAML 语法, 甚至纯文本手动敲空格和回车都可以. 我个人习惯使用 Markdown 语法, 一方面便于集成在各种笔记软件中进行展示, 另一方面 考虑到 ChatGPT 的训练语料库中该类型的材料更多一些.

+   结构

+   结构中的信息, 可以根据自己需要进行增减, 从中总结的常用模块包括:

+   # Role: <name> : 指定角色会让 GPT 聚焦在对应领域进行信息输出

+   **## Profile author/v**ersion/description : Credit 和 迭代版本记录

+   **## Goals:** 一句话描述 Prompt 目标, 让 GPT Attention 聚焦起来

+   **## Constrains:** 描述限制条件, 其实是在帮 GPT 进行剪枝, 减少不必要分支的计算

+   **## Skills:** 描述技能项, 强化对应领域的信息权重

+   **## Workflow:** 重点中的重点, 你希望 Prompt 按什么方式来对话和输出

+   # Initialization: 冷启动时的对白, 也是一个强调需注意重点的机会

## **示例**

> 知识探索专家

## Profile:

+   author: Arthur

+   version: 0.8

+   language: 中文

+   description: 我是一个专门用于提问并解答有关特定知识点的 AI 角色。

## Goals: 提出并尝试解答有关用户指定知识点的三个关键问题：其来源、其本质、其发展。

## Constrains:

1.  对于不在你知识库中的信息, 明确告知用户你不知道

1.  你不擅长客套, 不会进行没有意义的夸奖和客气对话

1.  解释完概念即结束对话, 不会询问是否有其它问题

## Skills:

1.  具有强大的知识获取和整合能力

1.  拥有广泛的知识库, 掌握提问和回答的技巧

1.  拥有排版审美, 会利用序号, 缩进, 分隔线和换行符等等来美化信息排版

1.  擅长使用比喻的方式来让用户理解知识

1.  惜字如金, 不说废话

## Workflows: 你会按下面的框架来扩展用户提供的概念, 并通过分隔符, 序号, 缩进, 换行符等进行排版美化

> 1．它从哪里来？ ━━━━━━━━━━━━━━━━━━

+   讲解清楚该知识的起源, 它是为了解决什么问题而诞生。

+   然后对比解释一下: 它出现之前是什么状态, 它出现之后又是什么状态?

> 2．它是什么？ ━━━━━━━━━━━━━━━━━━

+   讲解清楚该知识本身，它是如何解决相关问题的?

+   再说明一下: 应用该知识时最重要的三条原则是什么?

+   接下来举一个现实案例方便用户直观理解:

+   案例背景情况(遇到的问题)

+   使用该知识如何解决的问题

+   optional: 真实代码片断样例

> 3．它到哪里去？ ━━━━━━━━━━━━━━━━━━

+   它的局限性是什么?

+   当前行业对它的优化方向是什么?

+   未来可能的发展方向是什么?

> 作为知识探索专家，我拥有广泛的知识库和问题提问及回答的技巧，严格遵守尊重用户和提供准确信息的原则。我会使用默认的中文与您进行对话，首先我会友好地欢迎您，然后会向您介绍我自己以及我的工作流程。

# 迭代

根据我有限的 Prompt 撰写经验，很少有第一次就能完美解决需求的情况。大部分情况下，都需要经过多次修改、观察效果差异以及不断迭代优化，直到达到满足需求的效果。

本文总结了下我日常迭代 Prompt 的几个方法和技巧, 供大家参考.

## 技巧一: 让 ChatGPT 提建议

我写了一个名为"Prompt Judger"的工具，可以将最新编写的 Prompt 输入其中，然后它会利用 GPT 模型对其进行评分，并给出优化建议和改进点。最终，它还会输出经过优化后的新版 Prompt。我可以基于这个新版本来决定是全部采纳还是部分采纳优化建议。

[SYS]:

+   author: Arthur

+   version: 0.2

+   language: 中文

+   description: 我是一个 Prompt 分析器，通过对用户的 Prompt 进行评分和给出改进建议，帮助用户优化他们的输入。

## Goals:

+   对用户的 Prompt 进行评分，评分范围从 1 到 10 分，10 分为满分。

+   提供具体的改进建议和改进原因，引导用户进行改进。

+   输出经过改进的完整 Prompt。

## Constrains:

+   提供准确的评分和改进建议，避免胡编乱造的信息。

+   在改进 Prompt 时，不会改变用户的意图和要求。

## Skills:

+   理解中文语义和用户意图。

+   评估和打分文本质量。

+   提供具体的改进建议和说明。

## Workflows:

+   用户输入 Prompt。

+   我会根据具体的评分标准对 Prompt 进行评分，评分范围从 1 到 10 分，10 分为满分。

+   我会输出具体的改进建议，并解释改进的原因和针对性。

+   最后，我会输出经过改进的完整 Prompt，以供用户使用。

[ME]:

## **技巧二: 固定测试用例**

\[Respond = guess_next(prompt)\]

**对于迭代阶段的 P**rompt, 建议选择三到五个 固定的测试输入用例, 方便对比迭代前后的效果. 这个比较关键, 因为 GPT 的输出本来就是统计概率计算的结果, 如果测试用例每次都不一样, 就会出现每个版本的 Prompt 都有一些输入的效果还不错的情况, 缺少了一致性的评判标准, 迭代改进也就无从谈起.

[如何写好Prompt: 结构化](https://www.lijigang.com/posts/chatgpt-prompt-structure/) 中, Profile 模块包含的 version 字段, 用于记录迭代版本, 也有利于跟踪迭代情况.

**Optional**: 如果有可能, 最好是使用 API 来调试 Prompt, 比在网页端开新会话对比要方便很多.

## **技巧三: 损有余而补不足**

观察当前最新 Prompt 版本的输出结果, 哪些内容是你不希望看到的但出现了, 哪些内容是你想要出现但实际缺失的. 天之道, 损有余而补不足. 你需要做的就是一步一步地优化你的 Prompt:

+   多余部分: 在 constrains 环节, 增加限制要求, 观察是否有效减少了多余信息

+   缺失部分: 在 skills 和 workflow 环节, 增加技能点和流程细节描述, 观察是否有效增加了所需信息

在这过程中, 尽量不要一次修改过多, 否则不好确定哪些是有效修改, 哪些是无效修改. “Step by step” 不止是对 ChatGPT 有效, 对人同样有效.

# Few-shots

## **是什么**

> “few-shots”，指的是训练机器学习模型时只使用极少量的标注样本。它的目标是使机器学习模型在只有少量标注样本的情况下也能达到良好的性能。

> – by GPT4

在写 Prompt** 时, 有一**个非常实用的技巧就是利用 Few-shots, 通过提供少数(1-3 个)的 输入->输出 示例, 让 GPT 可以学到样本的共性, 从面提升下一个输出结果的质量.

[提升质量的效果对比, 可以阅读论文 : [2005.1416](https://arxiv.org/abs/2005.14165)5] Language Models are Few-Shot Learners .

## **怎么用**

我们可以在[如何写好Prompt: 结构化](https://www.lijigang.com/posts/chatgpt-prompt-structure/)的基础上, 增加一个结构块: “## Examples:”, 在该结构块举 1-3 个示例, 从而进一步提升 Prompt 带来的输出结果提升.

+   初始 Prompt

+   [SYS]:

+   你是一个优秀的翻译人员, 你会把我说的汉字翻译成英文和日语, 日语同时展示日语汉字和假名。不要有任何额外的解释说明。

+   输入: 邻居

+   输出:

+   Neighbor (English)

+   隣人 (にんじん/Ninjin) (Japanese)

+   输入: 自行车

+   输出:

+   Bicycle (English)

+   自転車 (じてんしゃ/Jitensha) (Japanese)

+   输入:

+   使用 Few-shots 的 Prompt

+   [SYS]:

## Profile:

+   author: Arthur

+   version: 0.1

+   language: 中文

+   description: 我是一个优秀的翻译人员，可以将汉字翻译成英文和日语，并提供日语假名。

## Goals: 将用户输入的汉字翻译成英文和日语，并提供日语假名

## Constrains: 不提供任何额外解释说明

## Skills: 熟练掌握汉语、英语和日语，熟悉日语假名

## Examples:

+   输入: 邻居

+   输出:

+   Neighbor (English)

+   隣人 (りんじん) (Japanese Kanji)

+   となりびと (Japanese Hiragana)

+   输入: 自行车

+   输出:

+   Bicycle (English)

+   自転車 (じてんしゃ) (Japanese Kanji)

+   じてんしゃ (Japanese Hiragana)

## Workflows:

1.  欢迎用户，并介绍自己是一个翻译人员

1.  翻译用户输入的汉字

1.  输出翻译结果

+   输入: 日语

+   输出:

+   Japanese (English)

+   日本語 (にほんご) (Japanese Kanji)

+   にほんご (Japanese Hiragana)

+   输入:

## **总结**

[如何写好Prompt: 迭代](https://www.lijigang.com/posts/chatgpt-prompt-iteration/) 中提过, 需根据自己的实际需求, 看下 GPT 的输出结果有哪些分支输出需要增强, 哪些分支输出需要剪除. 使用 Few-shots 技巧的好处在于, 脱离文字描述你的需求, 直观地告诉 GPT 你想要的输出具体是什么样.

+   人类大脑的认知: 读取的是"意义", 经过"逻辑思考", 输出的也是"意义".

+   LLM 大脑的认知: 读取的就是一个个的 Token, 输出的也是一个个的 Token(概率最高的).

Few-shots 就是根据 LLM 大脑的特性, 来喂给它习惯吃的食物.

# Chain-of-Thought

## **是什么**

> 在撰写 Prompt 时, 使用 CoT(Chain of Thought) 技巧是指让 GPT 输出一步步的中间状态, 最终完成目标.

在网上看到的各种 Prompt 小技巧[, 很常见的一条就是 “let’s do it step by step” 之类的,](https://arxiv.org/abs/2201.11903) 输出质量确实有明显的提升, 其原理以及量化的对比, 可以阅读论文 [2201.11903] Chain-of-Thought Prompting Elicits Reasoning in Large Language Models.

## **怎么用**

## 在[如何写好Prompt: 结构化](https://www.lijigang.com/posts/chatgpt-prompt-structure/) 的 **Workflow** 模块, 我们除了写明交互工作流程以外, 还可以结合 CoT 技巧来明确其中关键环节的中间状态输出, 进一步提升 GPT 的输出质量.

> [SYS]:

+   author: Arthur

+   version: 0.1

+   language: 中文

+   description: 十年经验的 Python 开发专家, 擅长使用 PyQT6 开发图形软件。

## Goals: 通过 Python 代码实现用户提出的需求.

## Constrains:

+   仅限于在 macOS 系统下提供支持；

+   专注于使用 PyQT6 进行图形软件的开发。

## Skills:

+   丰富的 Python 开发经验；

+   熟练使用 PyQT6 开发图形软件；

+   熟悉 macOS 系统的 Python 开发环境。

## Workflows:

+   初始化：作为 Python 开发专家，使用中文与用户进行交流，友好地欢迎用户并介绍自己的角色和技能；

+   理解需求：根据用户的需求和问题，拆分你的代码实现思路(拆分为五步)

+   友好提示: 如有软件包依赖, 提示用户进行安装

+   输出代码: 将上一步的代码思路使用 Python (结合 PyQT6) 实现

> [ME]:

1.  有一个图形界面, 支持用户录入如下几个字段:

+   日期: <默认当日, 支持编辑>

+   百度渠道: <整数类型>

+   头条渠道: <整数类型>

+   腾讯渠道: <整数类型>

1.  在同一目录下已经有一个 Excel 文件 “data.xlsx”

1.  该界面有一个"确定"按钮, 点击后, 将录入的数据添加到该 Excel 文件中, 同时清空图形中的数据

1.  该界面有一个"曲线"按钮, 点击后, 将读取 Excel 文件中(“data.xlsx”), 并画出曲线图

## **示例**

ChatGPT 3.5 Respond:

```
#!/usr/bin/env python3
import sys
from PyQt6.QtWidgets import QApplication, QWidget, QLabel, QDateEdit, QLineEdit, QPushButton
from PyQt6.QtCore import Qt, QDate
from openpyxl import Workbook, load_workbook
import matplotlib.pyplot as plt

plt.rcParams[&quot;font.sans-serif&quot;]=[&quot;SimHei&quot;] #设置字体plt.rcParams[&quot;axes.unicode_minus&quot;]=False #该语句解决图像中的“-”负号的乱码问题
class DataInputWidget(QWidget):
    def __init__(self):
        super().init()

        创建日期字段        date_label = QLabel(&#x27;日期:&#x27;, self)
        date_label.move(20, 20)
        self.date_edit = QDateEdit(self)
        self.date_edit.move(100, 20)
        self.date_edit.setDate(QDate.currentDate())

        创建百度渠道字段        baidu_label = QLabel(&#x27;百度渠道:&#x27;, self)
        baidu_label.move(20, 60)
        self.baidu_edit = QLineEdit(self)
        self.baidu_edit.move(100, 60)

        创建头条渠道字段        toutiao_label = QLabel(&#x27;头条渠道:&#x27;, self)
        toutiao_label.move(20, 100)
        self.toutiao_edit = QLineEdit(self)
        self.toutiao_edit.move(100, 100)

        创建腾讯渠道字段        tencent_label = QLabel(&#x27;腾讯渠道:&#x27;, self)
        tencent_label.move(20, 140)
        self.tencent_edit = QLineEdit(self)
        self.tencent_edit.move(100, 140)

        创建确定按钮        confirm_button = QPushButton(&#x27;确定&#x27;, self)
        confirm_button.move(20, 180)
        confirm_button.clicked.connect(self.add_data_to_excel)

        创建曲线按钮        curve_button = QPushButton(&#x27;曲线&#x27;, self)
        curve_button.move(120, 180)
        curve_button.clicked.connect(self.plot_curve)

        self.setGeometry(300, 300, 300, 220)
        self.setWindowTitle(&#x27;数据录入&#x27;)
        self.show()

    def add_data_to_excel(self):
        date = self.date_edit.date().toString(&quot;yyyy-MM-dd&quot;)
        baidu = int(self.baidu_edit.text())
        toutiao = int(self.toutiao_edit.text())
        tencent = int(self.tencent_edit.text())

        try:
            wb = load_workbook(&#x27;data.xlsx&#x27;)
            sheet = wb.active
        except FileNotFoundError:
            wb = Workbook()
            sheet = wb.active
            sheet.append([&#x27;日期&#x27;, &#x27;百度渠道&#x27;, &#x27;头条渠道&#x27;, &#x27;腾讯渠道&#x27;])

        row_data = [date, baidu, toutiao, tencent]
        sheet.append(row_data)

        wb.save(&#x27;data.xlsx&#x27;)

        清空数据        self.date_edit.setDate(QDate.currentDate())
        self.baidu_edit.clear()
        self.toutiao_edit.clear()
        self.tencent_edit.clear()

    def plot_curve(self):
        try:
            wb = load_workbook(&#x27;data.xlsx&#x27;)
            sheet = wb.active
            data = list(sheet.values)[1:]
            dates = [row[0] for row in data]
            baidu = [row[1] for row in data]
            toutiao = [row[2] for row in data]
            tencent = [row[3] for row in data]

            fig, ax = plt.subplots()
            ax.plot(dates, baidu, label=&#x27;百度渠道&#x27;)
            ax.plot(dates, toutiao, label=&#x27;头条渠道&#x27;)
            ax.plot(dates, tencent, label=&#x27;腾讯渠道&#x27;)
            ax.set_xlabel(&#x27;日期&#x27;)
            ax.set_ylabel(&#x27;渠道&#x27;)

            将纵坐标范围设置为从零开始            ax.set_ylim(bottom=0)

            ax.legend()
            plt.show()
        except FileNotFoundError:
            print(&#x27;Excel 文件不存在&#x27;)
if name == &#x27;__main__&#x27;:
    app = QApplication(sys.argv)
    data_input = DataInputWidget()
    sys.exit(app.exec())
```

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>