
# A/B实验的cookies类型

> 来源：[https://docs.feishu.cn/docx/KCJMdk5P5o6XvWxVuwOcbOrvnCg](https://docs.feishu.cn/docx/KCJMdk5P5o6XvWxVuwOcbOrvnCg)

## 问题

问：我再来请教下各位大佬，欧盟2C的网页仅用于`A/B`类测试，根据应用的受众情况而版本优化网站的cookie。目前实践中大家是作为`essential cookie` 还是否哇？目前只看到法国有明确的意见可以豁免同意。法国监管机构CNIL有关cookies和其他追踪方式的指引。

![](img/cd57dd8561a0e16c96e37029d157c9ce.png)

## 回答

### 答1

答1：这个的前提是：“在一些情形下视为应用户需求提供服务所必须”。我理解不是所有的`AB`测试都可以被认为是必要的，可以找原文来看看，不同人翻译可能理解上有偏差。

### 答2

答2：看了原文，这个翻译基本差不多。

### 答3

答3：豁免同意的cookie，在EU限缩的范围太小了，就是`communication`和`strictly necessary`，我们之前调研下来，`strictly necessary`大概就是这些，可以感受一下。

![](img/4090faa0447d978b954b7f9a4caef08b.png)

看了这个全文，感觉他配套的限制措施还挺多的，比如单独的`opt-out`、使用上也不可以和其他数据对照，这样的话，是不是把他的基础放在同意上反倒对后续的使用更简单一点？

![](img/a5c015a0391335fe6d83ba3691766e18.png)

### 答4

答4：对的，我的意思是在满足这些条件的情况下，是否可以适用到其他国家。主要是拿用户同意影响会很大，如果网站用户基数很大的话。

### 答5

答5：想要豁免同意，这三个要求不比拿用户同意简单，看公司对风险的承受态度了。不是每个国家都有对`strictly necessary`的详细列举。我们之前从CNIL和ICO上看到的`list`里，没有看到有明确提及这种`A/B test`的豁免。俺们是放在分析型cookie里了。

[欧盟](https://cbe8t1r4a5.feishu.cn/wiki/wikcn3Z7BUSRoMENb8QUxgwNJOf)

[技术概念科普](https://cbe8t1r4a5.feishu.cn/wiki/GfWtwCXWbiBzgQk1sxUcfFgXnSP)
