
# AI编程系列：Trae初体验，与Cursor对齐

> 来源：[https://agijuejin.feishu.cn/docx/SrvwdDWxqojcuBxymgCcOZ0HnQc](https://agijuejin.feishu.cn/docx/SrvwdDWxqojcuBxymgCcOZ0HnQc)

> [ iBe](https://mp.weixin.qq.com/s/fNGBTmVxIffp_7Ofm3cFVg)ar Bear探索AI｜阅读原文，如有侵权，请联系删除。

> 转载请联系原作者取得授权

## Trae 初体验

Trae是字节旗下公司开发的一款AI编程工具，与Cursor类似。不过初步看下虽然实现了类似Cursor的Agent功能，但体验上并没有更好。

不过值得肯定是，以我们的个人审美来看，从界面布局和体验上看，我认为Trae更符合我的习惯。

![](img/58b3ca7d066c25b5d317d88214c174bb.png)

Trae界面布局

![](img/51997b5fa52eda8bed5376a864562482.png)

Cursor界面布局

### 安装

官网地址：`https://www.trae.ai/`

目前只支持`MacOS`。

实际上它跟Cursor类似，都是在`VS Code`的基础上开发的。

### 与Cursor的比较

### 实际体验

**接下来我们分别在Cursor中执行同一个需求，看看实际**体验如何。

模型都用`claude-3.5-sonnect`版本；很奇怪为什么Trae不支持自家的豆包大模型？

#### Round 1

主要是看两个工具对任务的理解。任务如下：

```
做一款跨平台的APP，汉字学习。先讨论确定实现方案。
主要功能有：
1、收录中国所有的汉字，并且介绍汉字的意思和汉字的拼音；
2、能读出汉字的发音；
3、能通过动画的方式显示汉字的笔画顺序。显示汉字的一笔一画书写顺序。
```

Trae的执行结果

![](img/fb7f46e977230574dd7a3f518bfcad9f.png)

Cursor的执行结果从结果上看，Trae比较年轻直接就我建议怎么怎么样，然后也不跟用户确认就开干了。Cursor比较老道，会先分析实现方案，比如技术造型，数据存储，以及有哪些要实现的页面等。

Cursor明显执行的更符合要求

于是我又给Trae补了一名，得到的结果还是要开干。

![](img/d91354c1b59ae4bba8d15dabc71a79a6.png)

Trae可能是我给的提示词不够明确，不能在提示词中出现“再开始代码实现”，可能理解上会存在歧义。所以更换了一下提示词，就开始讨论方案了。

Trae

#### Round 2

接下来我需要更换技术实现框架。

```
我需要调整前端技术框架，不用Flutter框架。换其他框架。
```

![](img/46e1ce4ae9766991d088597dd452baf8.png)

Trae

![](img/9c195d631e5c9c8d6002d0c451ac01fc.png)

Cursor从结果上看，这轮**打平**

##### 这里说下我为什么没采用`Flutter`方案？

开始我以为Cursor先提出的`Flutter`方案是因为之前有一个项目中用了`Flutter`方案所以才优先推荐的。但从Trae给的方案来看，应该不是我想的这样。

因为之前有一个项目用`Flutter`做的前端，踩了很多坑，而且要兼容`windows`使用的话，只能把代码放在`windows`环境中，重新安装`Flutter`再编译打包成APP。

我觉得很麻烦，于是这次就`PASS`掉了。

#### Round 3

接下来看看对APP的性能和用户体验的实现方案：

```
如何确保APP的性能以及用户体验？
```

![](img/bc5ed2e4175f3dc515703c7e4053a5a0.png)

Trae

![](img/25102c6d573bea206beeed8bba34d505.png)

Cursor看上去渐渐趋于一致了。

今天的体验先到这里了，待后续继续体验之后再发。

## 总结

+ 从界面布局上看，Trae更优一些，UI看上去更符合国人的审美习惯，这点上国内的其他APP做出了突出贡献。而Cursor呢，则不太注重界面的呈现。就是原原的基于`VS Code` UI实现。

+ 从模型上看，Trae目前只有一个`sonnect`版本的模型，而Cursor则支持市面上主流的AI模型，连不支持`deepseek V3`模型，也可以曲线支持。

+ 从费用上看，Trae目前是免费，当然这个肯定是暂时的，因为它使用的是`claude`模型，对Trae来说是要付费使用`claude`的。有点奇怪的是竟然没有支持自家的豆包大模型。
