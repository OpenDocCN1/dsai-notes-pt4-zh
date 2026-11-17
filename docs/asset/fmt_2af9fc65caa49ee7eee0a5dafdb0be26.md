
# 课程 API 工具使用指南

## 可用 API 与操作格式

```
Answer the following questions as best you can. You have access to the following APIs:

CourseDatabase: Call this tool to interact with the 课程信息数据库 API. What is the 课程信息数据库 API useful for? 课程信息数据库存储有各课程的详细信息，包括目前的上线课时、每周更新次数以及每次更新的小时数。通过输入课程名称，可以返回该课程的详细信息。 Parameters: [{&quot;name&quot;: &quot;course_query&quot;, &quot;description&quot;: &quot;课程名称，所需查阅信息的课程名称&quot;, &quot;required&quot;: true, &quot;schema&quot;: {&quot;type&quot;: &quot;string&quot;}}]

CourseOperations: Call this tool to interact with the 课程操作工具 API. What is the 课程操作工具 API useful for? 课程操作工具提供了对课程信息的添加操作。可以添加课程的详细信息，如每周更新次数和每次更新的小时数。 Parameters: [{&quot;name&quot;: &quot;add_hours_to_course&quot;, &quot;description&quot;: &quot;给指定课程增加课时，需要课程名称和增加的课时数&quot;, &quot;required&quot;: true, &quot;schema&quot;: {&quot;type&quot;: &quot;object&quot;, &quot;properties&quot;: {&quot;course_name&quot;: {&quot;type&quot;: &quot;string&quot;}, &quot;additional_hours&quot;: {&quot;type&quot;: &quot;number&quot;}}, &quot;required&quot;: [&quot;course_name&quot;, &quot;additional_hours&quot;]}}]

Use the following format:

Question: the input question you must answer
Thought: you should always think about what to do
Action: the action to take, should be one of [CourseDatabase,CourseOperations]
Action Input: the input to the action
Observation: the result of the action
... (this Thought/Action/Action Input/Observation can be repeated zero or more times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question

Begin!

Question: 先帮我查询一下大模型技术实战这个课程目前更新了多少节了，今晚我直播了一节新课，请你帮我更新一下
```

## 更多大模型技术内容学习

扫码添加助理英英，回复“大模型”，了解更多大模型技术详情哦👇

![](img/badf4cc25781caf293224d05430f094a.png)

此外，扫码回复“入群”，即可加入大模型技术社群：海量硬核独家技术干货内容+无门槛技术交流！
