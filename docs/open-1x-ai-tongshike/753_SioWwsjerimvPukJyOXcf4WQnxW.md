
# 微学习20 | DeepSeek+dify：期末复习助手Agent

> 来源：`https://datawhaler.feishu.cn/docx/NkIid2tBVoJteSxNVx9cvC99nlb`

## 一、前言

[复习陪伴者](https://j1mbx7va0zr.feishu.cn/wiki/IYM0wXcLdiCZXPkv7VTcIXlwnMf?from=from_copylink)前我曾经在`coze`上创建过一个`agent`，名为 ，但是仍然有不足之处，它无法出题目，今天我们将在之前的基础上去迭代。

## 二、主要功能

出现场景：一般期末复习阶段老师，会发许多的复习资料，我们就要从中去找出考点，还有加以复习并掌握，往往在复习中会遇到这么几个问题，不知道考点是什么，不知道题目会怎么出，不确定是否掌握老师给的资料（题库）

### 2.1 考点提取功能

### 2.2 出题功能

## 三、项目创建

我们这个项目以`dify`+工作流模式运行

### 3.1 安装 `dify`

首先要安装`dify`，具体教程可以去网上搜索

`https://blog.csdn.net/qq_49035156/article/details/143264534`

### 3.2 创建应用

选择`Chatflow`，

为什么不选择`agent`模式呢，因为`agent`模式无法上传文档

![](img/d37aac5888c437c3a97870b3e4874bb3.png)

创建完毕

![](img/44556d82220fc5e548b862332c702986.png)

### 3.3 申请 `api`

去火山引擎官网申请免费的`deepseek`的`api`

`https://www.volcengine.com/`

点击立即使用

![](img/61ba40f7d517bfca65ac2e66d4212e7a.png)

填写信息

![](img/75ee624c0a370abbf79b18cd8c1cb6a8.png)

添加模型

![](img/a0a21bfbaaf36cbef386aa0145d68c4d.png)

点击接入

![](img/e1f3c0d18133738bae9a01b7e4e72742.png)

创建 `api`

![](img/875edf1827967117fde32f92e8339b9e.png)

![](img/4a4b65eab0e4c18971e8100f2394c2ea.png)

![](img/6a5f449e708d06dc9aa32a3d534bc39b.png)

查看`api key`，切记要保密

![](img/e725f2ccb9a65144b0caa1827a7dfd19.png)

### 3.4 添加模型

![](img/a8004737d54b90dd2726ed0cf4030376.png)

![](img/09d94a88058058cc07de325793beb286.png)

![](img/d6b707d67c06b8a8b0dd9824513273fb.png)

填入对应信息

选择`api`接入

![](img/4fcedbf1729c3078aecbe46f9d631ded.png)

模型名称——》名称

`API Key`——》`API Key`

![](img/f3829c398e19dea722b622fac8de13cc.png)

`Endpoint lD`如下

![](img/5193d4da573db972f355f82da6b3d466.png)

选择模型

![](img/aaf8cf3cdaf572db555a9387afa4c1ff.png)

添加完成就会出现如下模型

![](img/a5ff814809ebf2a29d1120cfb56c03bd.png)

加入模型

![](img/405ff0e8b4840c533b9f9f6008345840.png)

## 四、工作流的创建

添加以下节点（在某认的基础上去添加）

![](img/acef7a18ab0482b5d7584ec666a3f486.png)

### 4.1 添加文档提取器

![](img/6f006f750c0a2416b779c14b4321cee2.png)

### 4.2 在第一个 `llm` 中添加提示词

![](img/4358acd5c03d8fc71d73dbb82d021c40.png)

```
prompt = """
请从以下教学材料中提取核心考点，按以下格式输出：
[考点名称]: 
- 考查频率: (高频/中频/低频)
- 关联公式: (如有)
- 常见题型: (选择题/计算题/证明题)
- 关键步骤: (解题思路摘要)

材料内容：{
}
"""
```

### 4.3 打开文档上传功能

![](img/f653f4dbf55db86461b1bea81a7eff7c.png)

简单测试

![](img/a69e74b4f982c1957a0a7b0d3d100f8c.png)

![](img/9e84024bc29625a7078028f52f167993.png)

### 4.4 在第 2 个 `llm` 上添加提示词

![](img/501c60220ecec530cd37cb64803adc66.png)

````
prompt_v2 = """
你是一位经验丰富的试题命制专家，请根据给定的考点分析结果完成以下任务：

# 输入数据格式
`{{#llm.text#}}`
[考点名称]: 
- 考查频率: {高/中/低频}
- 关联公式: {公式列表}
- 常见题型: {题型分类}
- 关键步骤: {解题思路摘要}

# 生成要求
1. 原题再现：生成1道完全匹配该考点的典型例题，需包含：
   - 题干描述（标注对应教材章节）
   - 完整解题过程（使用公式块）
   - 易错点提示

2. 变式拓展：基于原题生成2种变式类型：
   - 基础变式：修改参数/数值（保持相同解法）
   - 进阶变式：改变条件/增加限制（需要方法迁移）

3. 跨题关联：生成1道结合其他考点的综合应用题（需标注关联考点）

# 输出格式
```markdown

## 考点名称: `{{考点名称}}`

### 原型例题

📘 **题目** (教材P`{{页码}}`):  

`{{题干描述}}`  

✅ **解答**:  

```math
`{{解题过程}}`
```
```
"""
````

### 4.5 简化流程

![](img/b9e42b8fbf6408a92f34a83a90dff72a.png)

## 五、结果

点击右上角的发布就可以使用啦

![](img/aea204b86c7fcd3340652fc2ad57c0a3.png)

## 六、结尾

该教程是第一版，后续会不定期更新，也会加入更多的功能

参考链接

`https://docs.dify.ai/zh-hans/guides/workflow/node/doc-extractor`
