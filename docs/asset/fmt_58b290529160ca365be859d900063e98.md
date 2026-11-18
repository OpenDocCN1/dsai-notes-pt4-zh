
# CARE 模型 Prompt

> 来源：[https://my.feishu.cn/docx/NSIjdR2dQoSl8hxv9OjcK0fIn6g](https://my.feishu.cn/docx/NSIjdR2dQoSl8hxv9OjcK0fIn6g)

## 自然语言形式

```
请按照 CARE 模型逐步引导我完成下列任务：

第一步: 任务背景
C (Context): 请描述您的任务背景和情况。
例如：我需要写一篇关于人工智能在医疗行业应用的文章，面向对人工智能不熟悉的普通读者。

[等待用户回复]

第二步: 目标描述
A (Aim): 您希望通过这个任务达成什么具体目标？
例如：我希望了解人工智能在医疗行业的具体应用案例。

[等待用户回复]

第三步: 任务描述
R (Request): 您希望 AI 执行什么具体任务来帮助您实现这个目标？
例如：请列举一些人工智能在医疗行业的应用案例，并解释其工作原理。

[等待用户回复]

第四步: 参考示例
E (Examples): 您能提供什么样的具体例子或参考来帮助 AI 更好理解您的需求吗？
例如：我想要类似于 IBM Watson 在癌症诊断方面的应用案例。

[等待用户回复]

非常感谢您提供的详细信息，我现在将根据您的输入开始执行任务。如果在执行过程中有任何需要调整的地方，请随时告诉我。
```

## 伪代码形式

```
{
  &quot;description&quot;: &quot;根据CARE模型逐步引导用户完成任务，确保任务背景、目标、具体请求和参考示例清晰明确。&quot;,
  &quot;input&quot;: {
    &quot;task_steps&quot;: [
      &quot;步骤1: 用户描述任务背景和情况&quot;,
      &quot;步骤2: 用户描述希望通过任务达成的具体目标&quot;,
      &quot;步骤3: 用户描述希望AI执行的具体任务&quot;,
      &quot;步骤4: 用户提供具体例子或参考来帮助AI理解需求&quot;
    ]
  },
  &quot;process&quot;: [
    &quot;等待用户对每个步骤的回复，确保信息完整且准确&quot;,
    &quot;根据用户提供的信息，生成具体的任务执行计划&quot;,
    &quot;确保任务执行计划符合用户的需求和目标&quot;,
    &quot;在执行过程中保持与用户的沟通，以便及时调整任务计划&quot;
  ],
  &quot;output&quot;: &quot;生成一个详细的、可执行的任务计划，包括任务背景、目标、具体请求和参考示例，确保AI能够准确地帮助用户完成任务。&quot;
}
```

```
{
  &quot;description&quot;: &quot;AI 助手按照 CARE 模型逐步引导用户完成任务&quot;,
  &quot;steps&quot;: [
    {
      &quot;step&quot;: 1,
      &quot;label&quot;: &quot;任务背景 (Context)&quot;,
      &quot;prompt&quot;: &quot;请描述您的任务背景和情况。例如：我需要写一篇关于人工智能在医疗行业应用的文章，面向对人工智能不熟悉的普通读者。&quot;,
      &quot;action&quot;: &quot;等待用户输入任务背景信息&quot;,
      &quot;user_input&quot;: &quot;任务背景&quot;,
      &quot;next_step&quot;: 2
    },
    {
      &quot;step&quot;: 2,
      &quot;label&quot;: &quot;目标描述 (Aim)&quot;,
      &quot;prompt&quot;: &quot;您希望通过这个任务达成什么具体目标？例如：我希望了解人工智能在医疗行业的具体应用案例。&quot;,
      &quot;action&quot;: &quot;等待用户输入目标描述&quot;,
      &quot;user_input&quot;: &quot;任务目标&quot;,
      &quot;next_step&quot;: 3
    },
    {
      &quot;step&quot;: 3,
      &quot;label&quot;: &quot;任务描述 (Request)&quot;,
      &quot;prompt&quot;: &quot;您希望 AI 执行什么具体任务来帮助您实现这个目标？例如：请列举一些人工智能在医疗行业的应用案例，并解释其工作原理。&quot;,
      &quot;action&quot;: &quot;等待用户输入任务请求&quot;,
      &quot;user_input&quot;: &quot;任务描述&quot;,
      &quot;next_step&quot;: 4
    },
    {
      &quot;step&quot;: 4,
      &quot;label&quot;: &quot;参考示例 (Examples)&quot;,
      &quot;prompt&quot;: &quot;您能提供什么样的具体例子或参考来帮助 AI 更好理解您的需求吗？例如：我想要类似于 IBM Watson 在癌症诊断方面的应用案例。&quot;,
      &quot;action&quot;: &quot;等待用户输入参考示例&quot;,
      &quot;user_input&quot;: &quot;参考示例&quot;,
      &quot;next_step&quot;: 5
    },
    {
      &quot;step&quot;: 5,
      &quot;label&quot;: &quot;汇总并执行&quot;,
      &quot;action&quot;: &quot;AI 根据用户提供的所有信息进行汇总，并执行相应的任务&quot;,
      &quot;process&quot;: [
        &quot;收集任务背景、目标、请求、参考示例&quot;,
        &quot;分析任务的具体需求&quot;,
        &quot;执行相应的 AI 任务&quot;
      ],
      &quot;output&quot;: &quot;生成的结果或解决方案&quot;
    }
  ]
}
```
