
# FinRobot：基于LLM的金融分析Agent

> 来源：[https://v11enp9ok1h.feishu.cn/docx/LGZsdvAlmoLiNnxeQdNcURyEndG](https://v11enp9ok1h.feishu.cn/docx/LGZsdvAlmoLiNnxeQdNcURyEndG)

## 摘要（Abstract）

- FinRobot平台，这是一个开源的AI代理平台，支持多种金融专业AI代理，每个代理都由LLM驱动。
- 平台由四层组成：金融AI代理层、金融LLM算法层、LLMOps和DataOps层、多源LLM基础模型层。
- FinRobot旨在为专业分析师和普通用户提供高级金融分析的AI技术

## 引言（Introduction）

- 讨论了金融分析的重要性，包括市场趋势解释、经济预测和投资策略提供。
- 介绍了基于AI的金融分析的革命性变化，以及LLMs如何增强AI的功能。

## 什么是FinRobot?

FinRobot引入了一种新颖的`Smart Scheduler`机制，允许无缝集成多源LLMs，使平台能够利用各种最先进的LLMs的优势，并为特定的金融任务选择最合适的。这种适应性对于处理全球金融市场的复杂性和多语言数据至关重要。

## FinRobot架构

如上图，FinRobot的整体架构被划分为四个独特的层次，每层都针对金融AI处理和应用的不同方面进行了专门设计：

### 金融AI agent 层

这一层由特定领域的AI代理组成，旨在通过先进的数据感知、认知处理和动态行动执行来增强金融分析：

![](img/10d3c5679b99dbc87c4f42e7aa2db51f.png)

- 感知（`Perception`）：该模块捕获并解释来自市场信息、新闻和经济指标的多模态金融数据，使用复杂技术将数据结构化以供深入分析。
- 大脑（`Brain`）：作为核心处理单元，该模块使用LLM感知来自感知模块的数据，并利用金融思维链（`CoT`）过程生成结构化指令。
- 行动（`Action`）：该模块执行来自`Brain`模块的指令，应用工具将分析洞察转化为可操作的结果，如交易、投资组合调整、生成报告或发送警报

### 金融LLM算法层

这一层包含针对金融行业特定需求而设计的先进AI算法，增强了平台在一系列金融应用中的能力：

- 金融大型语言模型（`FinGPT`）：专为金融领域设计的LLM，通过监督式微调，以金融领域的指令响应数据对开源大型语言模型进行优化。
- 金融强化学习（`FinRL`）：使用集成深度强化学习算法优化交易策略，分析历史和实时市场数据。
- 金融机器学习（`FinML`）：利用多种机器学习技术来提高金融预测分析的能力。
- 金融多模态LLM：处理并综合来自多种模态（如文本、图表和表格）的信息，提供全面深入的金融文档理解。

### LLMOps层（LLMOps Layer）

`LLMOps`层旨在实现高模块化和可插拔性，以适应技术进步和金融市场需求的演变：

- 智能调度器（`Smart Scheduler`）：优化任务分配，包括任务管理、代理注册、代理适配器和主管代理等组件，确保任务根据代理的性能指标和特定任务的适用性进行分配。

### 数据操作层

数据操作层管理金融分析所需的广泛和多样化的数据集，确保输入AI处理管道的所有数据都是高质量和代表当前市场状况的：

- 检索增强生成（`RAG`）：结合了上下文检索机制和LLMs的优势，优化语言生成任务。

### 多源LLM基础模型层

这一层为`FinRobot`提供了管理和整合多种大型语言模型（LLMs）的高级功能，这对于适应全球金融市场的动态需求至关重要：

- 即插即用功能：允许无缝集成和更新通用和专业LLMs。
- 模型多样性和评估：整合了参数范围从7亿到72亿的LLMs，针对特定金融任务进行了严格评估。
- 全球市场兼容性：支持多语言模型集成，增强了平台分析和处理多样化金融数据的能力

## 金融思维链（Financial Chain-of-Thought）提示技术

### CoT提示技术介绍

- 概念：`CoT`提示技术是一种结构化提示方法，旨在通过鼓励AI模型逐步推理来增强其在复杂推理任务上的表现。这种方法通过引导模型明确展示推理的中间步骤，从而提高答案的准确性和可解释性。
- 应用：`CoT`技术已被证明在数学问题解决和常识推理等任务上非常有效。

### 金融CoT提示的概念

- 定义：金融`CoT`提示是将`CoT`技术应用于AI驱动的金融分析，它模仿金融专业人员处理复杂金融问题时的思维过程。
- 作用：该技术通过将复杂金融场景分解为更小、更易于管理的部分，然后分析每个部分并综合结果来形成结论或建议。

### 金融CoT提示的实施

- 财务分析：FinRobot使用`CoT`提示来超越基本的数据列举和比率计算，通过比较分析和历史表现对比，提供深入的业务洞察。
- 业务特定分析：FinRobot利用检索增强生成（`RAG`）技术来收集有关公司产品与服务的数据，提供全面的业务分析。
- 市场分析：FinRobot结合财务比率、市场数据、新闻情绪和网络数据，模拟市场参与者的决策过程，提供公司股价轨迹和估值的综合评估。
- 估值分析：FinRobot整合了财务比率、市场数据、新闻情绪和网络数据的分析，使用LLMs评估公司股价轨迹和估值。

### 金融CoT提示的优势

- 灵活性：与依赖固定模板的数据提供商不同，`CoT`提示利用LLMs的生成性质，不依赖于死板的模板。
- 错误检查：通过多层提示嵌入错误检查，减少误分类的风险。
- 简化人工劳动：通过将投资逻辑编码到推理过程中，简化了人工劳动。
- 提取信息：能够从复杂且非结构化的数据格式（如收益电话会议记录）中提取相关的历史数字和定性信息。
- 解释性：提供对记录和派生值的来源和推导的详细解释。
- 适应性和发展性：随着LLMs的持续改进和指令调整，`CoT`提示可以适应并发展其应用。

## Demo演示和实操

### FinRobot代码全部开源，安装步骤

1. 推荐使用`Conda`来创建虚拟环境

```
conda create --name finrobot python=3.10
conda activate finrobot
```

2. 在命令行下载FinRobot代码仓库

```
git clone https://github.com/AI4Finance-Foundation/FinRobot.git
cd FinRobot
```

3. 从`pypi`源安装依赖

get our latest release from pypi

```
pip install -U finrobot
```

or install from this repo directly

```
pip install -e .
```

4. 修改`OAI_CONFIG_LIST_sample`配置文件

```
rename OAI_CONFIG_LIST_sample to OAI_CONFIG_LIST
remove the four lines of comment within the OAI_CONFIG_LIST file
add your own openai api-key &lt;your OpenAI API key here&gt;
```

5. 修改`config_api_keys_sample`文件

```
 rename config_api_keys_sample to config_api_keys
 remove the comment within the config_api_keys file
 add your own finnhub-api &quot;YOUR_FINNHUB_API_KEY&quot;
 add your own financialmodelingprep and sec-api keys &quot;YOUR_FMP_API_KEY&quot; and &quot;YOUR_SEC_API_KEY&quot; (for financial report generation)
```

6. 运行`demo`:

```
# find these notebooks in tutorials
1) agent_annual_report.ipynb
2) agent_fingpt_forecaster.ipynb
3) agent_trade_strategist.ipynb
4) lmm_agent_mplfinance.ipynb
5) lmm_agent_opt_smacross.ipynb
```

### 应用案例I：市场预测器（Market Forecaster）

- 功能：市场预测器是一套AI代理，旨在综合最新的市场新闻和金融数据，提供对公司最新成就和潜在问题的全面洞察，并预测股价走势。
- 应用：作为一个初级的机器人顾问，市场预测器展示了AI在金融咨询领域的应用潜力。
- 示例：提供了针对特定公司（如Nvidia和贵州茅台）的市场预测示例，展示了如何根据公司的相关新闻、财务状况和市场趋势来分析积极的发展因素和潜在的担忧，并据此预测未来一周的股价变动。

### 应用案例II：文档分析与报告生成（Document Analysis & Generation）

- 文档分析：AI代理结合先进的LLMs，用于深入分析财务文件，如年报、SEC文件和收益电话会议记录，提取关键信息，识别主要的财务指标，并突出显示需要进一步审查的趋势和差异。
- 报告生成：在分析阶段之后，相同的AI代理被用来生成全面的财务报告，这些报告涵盖了财务分析的各个方面，包括绩效评估、市场比较和前瞻性财务预测。
- 效果：展示了由FinRobot生成的股权研究报告样本，证明了平台在实际金融分析中的应用和有效性。
