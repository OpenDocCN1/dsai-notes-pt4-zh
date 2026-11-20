
# 构建AI Agent长期记忆解决方案

> 来源：[https://v11enp9ok1h.feishu.cn/docx/TzDFdAVeyoAIogxWc0LcJLCjnjG](https://v11enp9ok1h.feishu.cn/docx/TzDFdAVeyoAIogxWc0LcJLCjnjG)

如果一个没有记忆的AI代理被部署到生产环境中，每次与代理的交互都将是一张白纸。了解如何构建具有人类般记忆的AI代理来解决这个问题。

![](img/113e519a246da459f4e751eabaa2a98d.png)

## 工作流程

+ 用户提交查询
+ 代理保存对话并将事实提取到内存中
+ 代理检索事实并进行总结
+ 利用事实和历史记录做出知情响应

## 实现

### 设置LLM

我们将通过Ollama在本地部署Qwen 3。请查看：

![](img/51f7aea2c65556eaf852b58f8face5b2.png)

### 初始化Zep客户端

我们利用`zep_ai`的基础记忆层，为`Autogen`代理配备真正的任务完成能力。

![](img/cf94d767b35a2c3cae24c204d4693133.png)
