# 用coze实现在线搜索Agent

> 来源：[https://datawhaler.feishu.cn/docx/RLjEdmRKYoG53VxTeVZcGuBqn9g](https://datawhaler.feishu.cn/docx/RLjEdmRKYoG53VxTeVZcGuBqn9g)

在张小白学习[Dify](https://zhida.zhihu.com/search?content_id=256275678&content_type=Article&match_order=1&q=Dify&zhida_source=entity)和RAGFlow的时候，就听过[Coze](https://zhida.zhihu.com/search?content_id=256275678&content_type=Article&match_order=1&q=Coze&zhida_source=entity)的大名了。但是今天才开始正式接触Coze。

## 一、注册用户

打开

[扣子-AI 智能体开发平台](https://link.zhihu.com/?target=https%3A//www.coze.cn/)[www.coze.cn/](https://link.zhihu.com/?target=https%3A//www.coze.cn/)

![](img/74ab5ec2245813280d9641f026b2e018.png)

点击“登录扣子”

![](img/adda6edb1ec161216fa6f23d0b660be4.png)

使用手机验证码登录注册：

![](img/d40f7628c2ff40a81b94e62140db2ed1.png)

![](img/9ce5e74b1158e4d704900fef1311efd0.png)

## 二、创建智能体

点击左上角+创建：

![](img/dac546424d2af37941210c7bdc760861.png)

创建智能体：

![](img/970009dc73c46034e16805da59f7c02a.png)

输入名称，点击确认：

![](img/95c9cdc91afb3ed78a8d533825fa9ab3.png)

## 三、配置智能体

### 1、配置人设和回复逻辑

在”[人设与回复逻辑](https://zhida.zhihu.com/search?content_id=256275678&content_type=Article&match_order=1&q=%E4%BA%BA%E8%AE%BE%E4%B8%8E%E5%9B%9E%E5%A4%8D%E9%80%BB%E8%BE%91&zhida_source=entity)“输入以下内容：

```
#介绍
你是⼀个拥有搜索能⼒的问答机器⼈

##主要能⼒
1.当⽤⼾询问任何问题时，请使⽤{llm_search}进⾏处理
2.始终使⽤⽤⼾的原始查询作为&#x27;llm_search&#x27;中的&#x27;query&#x27;参数，不任何修改。例如，如果被问及&quot;字节跳动&quot;，query的内容等于&#x27;字节跳动&#x27;
3.⽤{{user_language}}的语⾔回复⽤⼾
```

![](img/281d8ccdf697db90d2dfc5304df5b379.png)

### 2、配置[变量](https://zhida.zhihu.com/search?content_id=256275678&content_type=Article&match_order=1&q=%E5%8F%98%E9%87%8F&zhida_source=entity)

创建变量：

user_language

![](img/770ebb3b650d629d3ad7c1be1aa58263.png)

点击保存。

### 3、配置数据库

创建数据库：

新建数据表-》自定义数据表：

![](img/12442bcfae8d356aea9efebb4b616d07.png)

表名：qa

选择多用户模式：

按以下方式录入：

点击保存。

### 4、配置[工作流](https://zhida.zhihu.com/search?content_id=256275678&content_type=Article&match_order=1&q=%E5%B7%A5%E4%BD%9C%E6%B5%81&zhida_source=entity)

（1）创建工作流：

选择基础模型：豆包-工具调用

![](img/47f1b8e94a414f66736448f0455c6411.png)

创建工作流llm_search_event

![](img/e63835b51f6084adfabe52b51b839d15.png)

（2）配置【开始】节点：

![](img/a52deb47bc64bf255562179ccf56f6b4.png)

（3）增加【问答】节点：

![](img/f348921f7982dee5383f63a63d15b64b.png)

将开始节点和问答节点连接起来，并在问答节点按下图配置：

![](img/3db40fd1b8736ae6d20ccfd72504ee7e.png)

（4）增加 搜索插件：

（A）bingWebSearch必应搜索插件：

![](img/7683a48eb3d10f86dd34504ada265e0f.png)

配置【bingWebSearch】插件：

（B）search[头条搜索插件](https://zhida.zhihu.com/search?content_id=256275678&content_type=Article&match_order=1&q=%E5%A4%B4%E6%9D%A1%E6%90%9C%E7%B4%A2%E6%8F%92%E4%BB%B6&zhida_source=entity)

![](img/c003ff5a359ef8eb86d1b56f300d904c.png)

配置【search头条搜索】插件：

![](img/4da91d8e9e9981d39bb79e4e512e0774.png)

[（C）Touti](https://zhida.zhihu.com/search?content_id=256275678&content_type=Article&match_order=1&q=%E5%A4%B4%E6%9D%A1%E5%9B%BE%E5%83%8F%E6%90%9C%E7%B4%A2%E6%8F%92%E4%BB%B6&zhida_source=entity)aoPictureSearch头条图像搜索插件

![](img/2bcbf5b116287c01beffdcaf21b788aa.png)

配置【ToutiaoPictureSearch头条图像搜索】插件:

（5）增加大模型节点

（A）【Bing内容总结】大模型节点

提示词：

作为一个有辨识力的读者，你拥有从众多来源细致分析信息、精确找出最重要的细节并评估其真实性的能力。你处理复杂问题的方法是一个逻辑思考者的方式，依靠证据而非易错的直觉来形成结论。此外，你作为一个专业的作家，擅长将你的思想和论点有条理地组织起来;确保你的散文引人入胜，远离枯燥无味。

你将收到一个用户查询，请为查询写出清晰、简洁且准确的回答。

你的回答必须是正确、准确的，并且由专家以公正和专业的语调撰写。

不要提供与查询无关的任何信息，不要重复。

你要将上下文信息按照时间线梳理出来。

以下是用户查询:{{query}}以及检索到的一组上下文:

{{contexts}}。

最终输出要按照如下格式整理：

#事件背景

根据{{contexts}}总结事件的背景，100字以内完成简述

#时间线

根据{{contexts}}用时间线将事件串连起来，以markdown表格的格式输出。第一列是时间，第二列是事件进展。

#相关人物

根据{{contexts}}将重要人物和任务介绍列举出来，以markdown表格的格式输出。第一列是人物，第二列是人物介绍。

#引用信息

根据{{contexts}}内容整理为markdown链接索引的格式放在后面，展示为文本而不是url。前面添加自增序号。

输出：response

![](img/b47eca5abcb7a18719fbf049bc76219b.png)

（B）【头条内容总结】大模型节点

![](img/7df42fb89e3bf6857d54a18cdc24d1fa.png)

同样的，更改提示词和response：

![](img/f136634643ae44a4221a71acc24a5487.png)

配置结束节点：

## 四、智能体试运行

水门事件

![](img/4b38005e4b7fcb7bf71537472454f805.png)

点击右下角的 试运行：

![](img/a8dc2c6a01211cc1724237cc760567bf.png)

选择【头条搜索】：

![](img/2ea8eb8d4fc3b4016814bab6dd2293b4.png)

等待一会儿，可以看到 试运行成功：

![](img/9457f8a5ab8695edce6956c49e477bf7.png)

试试”特朗普加税“

![](img/14bea7299b2c1ef338cb642539e07464.png)

![](img/a20981e3918fc11beaf8fc7a4ec7e6d8.png)

![](img/e743f0ba247544eeea07fc2257ee0728.png)

## 五、智能体发布

点击发布。

![](img/39ecad9f7b247d64aba29c27ccfe637f.png)

![](img/6f8da68557ea9acae23a6fa991d3e506.png)

确认：

![](img/b7b1ad57b3c8397edf5ad5f1f94c2ca8.png)

这就完成了Coze的智能体制作。

---

> 本文作者：

<https://datawhaler.feishu.cn/sync/LfRzdAExlsoykRbUZymc0Jrfngh>

# 欢迎投稿参与共建、下次微学习就学你的内容！

<https://datawhaler.feishu.cn/sync/CPNrdTvwgshasbb2UjecLzVinPe>