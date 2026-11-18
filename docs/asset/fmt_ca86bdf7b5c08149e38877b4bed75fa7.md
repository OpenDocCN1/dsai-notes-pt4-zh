
# 1、LangGPT框架

> 来源：[https://vxc3hj17dym.feishu.cn/docx/B7wudxH72ox6🐧x58Hdcj0d3nJc](https://vxc3hj17dym.feishu.cn/docx/B7wudxH72ox6🐧x58Hdcj0d3nJc)
>
> **关于作者：**[Welcome to Jackey. | 甲木个人杂志](https://langgptai.feishu.cn/docx/Oturdxhj2oxO3hxp5tZcbPCan9e?from=from_copylink)
>
> 大语言模型十分擅长角色扮演，大部分优质 prompt 开头往往就是 “我希望你作为xxx”，“我希望你扮演xxx” 的句式定义一个角色，只要提供角色说明，角色行为，技能等描述，就能做出很符合角色的行为。
>
> 如果你熟悉编程语言里的 “对象”，就知道其实 prompt 的“角色声明”和类声明很像。因此可以将 prompt 抽象为一个角色（`Role`），包含名字，描述，技能，工作方法等描述，然后就得到了 LangGPT 的 `Role` 模板。
>
> 使用 `Role` 模板，只需要按照模板填写相应内容即可。除了变量和模板外，LangGPT 还提供了命令，记忆器，条件句等语法设置方法。

```
# Role: 设置角色名称，一级标题，作用范围为全局

## Profile: 设置角色简介，二级标题，作用范围为段落

- Author: yzfly    设置 Prompt 作者名，保护 Prompt 原作权益
- Version: 1.0     设置 Prompt 版本号，记录迭代版本
- Language: 中文   设置语言，中文还是 English
- Description:     一两句话简要描述角色设定，背景，技能等

## Background:  根据Role和用户需求，简述用户需求的背景和描述。

## Goals:  基于用户诉求，思考我们希望Kimi能够实现哪些目标。

## Constraints:  完成Goals需要遵守哪些规则和限制，以此来保证输出结果的质量。

### Skills:  设置技能，下面分点仔细描述
1. xxx
2. xxx

## Rules        设置规则，下面分点描述细节
1. xxx
2. xxx

## Workflow     设置工作流程，如何和用户交流，交互
1. 让用户以 &quot;形式：[], 主题：[]&quot; 的方式指定诗歌形式，主题。
2. 针对用户给定的主题，创作诗歌，包括题目和诗句。

## Initialization  设置初始化步骤，强调 prompt 各内容之间的作用和联系，定义初始化行为。
作为角色 &lt;Role&gt;, 严格遵守 &lt;Rules&gt;, 使用默认 &lt;Language&gt; 与用户对话，友好的欢迎用户。然后介绍自己，并告诉用户 &lt;Workflow&gt;。
```
