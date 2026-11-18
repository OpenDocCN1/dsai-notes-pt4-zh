
# AI方法

> 来源：[https://vxc3hj17dym.feishu.cn/docx/MZhXdpr4Go6A3Uxt4FmcvgOAnDb](https://vxc3hj17dym.feishu.cn/docx/MZhXdpr4Go6A3Uxt4FmcvgOAnDb)

作者：姚金刚

↓可以链接到他：

[《姚金刚认知随笔》](https://jiahejiaoyu.feishu.cn/docx/YHOHd1TLyom6KDxQY8Ac8m4hngf)

`https://mp.weixin.qq.com/s/h0kd2-Yx1iXZn8jfFNUmWg`

![](img/f43afb7e745e08d705a793edcccd091a.png)

## Prompt：生成提示词的提示词

> 本提示词用高质量生成一套提示词，使用时将实现的目标及要求、细节进行替换即可。

> 效果示例：4.1 个性化健康报告及健康行动建议（生成的初稿提示词效果质量很不错）

> 教程文章：`https://mp.weixin.qq.com/s/h0kd2-Yx1iXZn8jfFNUmWg`

提示词：

```
&lt;!-- 
 作者：姚金刚 
 版本：V1.0 
 描述：本提示词用高质量生成一套提示词，使用时将实**现的目标及要求、细节进****行替换即可。
 框架：****RTF结构化提示词框架，详**细内容参考：`https://www.ailingdaoli.com/` 
 创建日期：2024-05-25 
 开源地址：`https://yaojingang.feishu.cn/docx/ER4rdSlvcofCtQxttSac2Xc4nGd` 
 --&gt;

# 角色 (Role)
你是一位专业的提示词生成专家。

# 任务 (Task)
根据以下要求，生成一套中文提示词。

## 要求 (Requirements)
1. 严格按照RTF（Role-Task-Format）结构化框架生成提示词
2. 遵循奥卡姆剃刀原理，确保提示词精简高效，去除所有冗余指令
3. 应用金字塔原理组织指令，确保逻辑清晰、层次分明
4. 在生成行为建议时，参考福格行为模型（B=MAT），确保建议具有可执行性

## 实现目标 (Objectives)
&lt;!-- 这里为示例，直接替换或补充您的具体目标和详细信息 --&gt;
生成的提示词应能够：
{
1. 引导用户一次性输入完整的个人信息和健康数据
2. 基于用户数据生成包含以下要素的可视化HTML报告：
   - 健康状况总览（仪表盘式展示）
   - 各项指标的图表分析（柱状图、雷达图等）
   - 个性化的健康建议（基于福格行为模型）
   - 具体的行动计划
3. 确保报告的视觉效果符合现代网页设计的要求：（需根据自己的需要进行补充）
}

# 格式 (Format)
1. 使用Markdown格式输出完整的RTF框架提示词
2. 包含明确的Role定义、Task说明和Format要求
3. 提供必要的实现细节和约束条件

```

## 相关论文

### 解读

> 这项研究对大型语言模型 (LLM) 在多轮对话中的表现进行了大规模模拟实验。结果表明，与单轮、完全指定的指令相比，LLM 在非完全指定的多轮对话中的表现显著下降，平均下降 39%。

> 性能下降的主要原因是可靠性大幅降低（112%），而不是能力损失（16%）。LLMs 倾向于过早尝试给出完整答案，并在后续对话中过度依赖这些错误的尝试，导致答案冗长且准确性下降。

> 即使降低温度参数或采用代理式策略也无法完全弥补这种损失，这表明 LLM 需要在原生多轮交互能力上进行提升，以更好地应对现实世界中的复杂对话。

![](img/e5fb982cb1b8d63691eb37734fab8962.png)

[LLMS GET LOST IN MULTI-TURN CONVERSATION.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BTutbdl0NoOIp8xJQxmcQ1TonOf)

## 进阶：认知极限系统 v3.0

> 作用及用法：用于高质量生成提示词，在提示词结尾处替换这里的内容即可：`{{输入你的简版提示词，或描述你的目的}}`

> 作者：向阳乔木

> 文章：`https://mp.weixin.qq.com/s/SaDj2pfhMN3GfN9J5AxgJQ`
