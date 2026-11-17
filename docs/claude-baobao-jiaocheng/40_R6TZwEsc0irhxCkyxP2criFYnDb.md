# 可选插件DLC

> 来源：[https://docs.feishu.cn/docx/AMJudTUdRoyJduxB8iCccimsn9d](https://docs.feishu.cn/docx/AMJudTUdRoyJduxB8iCccimsn9d)

## 插图DLC（测试）

（作者三号老公 ）

内容参考[坎佬教程](https://rentry.org/teralomaniac_clewd/)（推荐使用最新酒馆）

顾名思义，是对部分角色卡中的插图进行导出的一个DLC，由于是测试版本，可能会出现不出图或者图不对文等。

其中应该这么填写，应该填写2个正[正则表达式与插图DLC等](https://sqivg8d05rm.feishu.cn/wiki/QKkYwUsGAivp4ukhvBRcYlf6nCd)表达式。（不明白正则表达式的看这个教程  ）

### 正则表达式

**注：所有正则同时勾选用于****AI**** ****output** 和 Only Format Display！

Script Name：photo0（名称随意，你开心就好）

Find Regex（查找正则）：

```
/&lt;(illustration|img)&gt;.*[^0-9A-Za-z\.\s&lt;/&gt;]+/gm
```

Replace With（替换）：

```
&lt;center&gt;&lt;img src=https://files.catbox.moe/
```


Script Name：photo1（名称随意，你开心就好）

Find Regex（查找正则）：

```
/&lt;\/(illustration|img)&gt;/g
```

Replace With（替换）：

```
? width=50% /&gt;&lt;/center&gt;
```

（注：50%为图片大小，可以根据自行需要修改）

![](img/b00dddcb1193907c132da305296f9e68.png)

那么以上就是插图DLC正则表达式的全部内容

### 插图（写卡作者需，仅游玩可以忽略）

以上是准备工作，接下来就是正式插图了

[首先，如果你](https://files.catbox.moe)是写卡作者，需要把你选好的图片上传到catbox图库

（只能传输大小小于200MB的文件）（自备魔法）

![](img/06fa6cda1d5e119ce781e29c0b97e89e.png)

获取刚刚的catbox给的网址的小尾巴（例如教程中的47rshl）并对小尾巴进行命名，例如微笑:微笑47[世界书（施工中）](https://sqivg8d05rm.feishu.cn/wiki/ZJ9Yw0X7JipJrlkAoT0cku54nMb)shl.png，接下来你需要创建一个  填写需要输出的图片。

其中第一个为插图列表，第二个为插图规则

Primary Keywords均填<img>    Logic均为And  Optional Filter均填</img>

1.  插图列表

把要填的插图填入内容中，以<illustration_list>开头</illustration_list>结尾。

例如：

```
&lt;illustration_list&gt;
惊讶:惊讶0yh5ei.png
微笑:微笑47rshl.png
&lt;/illustration_list&gt;
```

1.  插图规则

内容中填写如下即可

```
&lt;!-- &lt;illustration&gt; is the rule for inserting illustrations. --&gt;
&lt;illustration&gt;
Assistant should insert an illustration at the beginning of the response that matches {{char}}&#x27;s inner feelings or the current scene, depending on the content of the plot.
Assistant must output the filenames in the “&lt;illustration_list&gt;” exactly and completely, and tag it with &lt;img&gt;&lt;/img&gt; XML tags.
Format: &lt;img&gt;{{filename}}&lt;/img&gt;

When assistant describes sexual acts or sexual organs，assistant should load nsfw illustration 19d31 in &lt;illustration_list&gt; depending on the current sexual position.
&lt;/illustration&gt;
```

![](img/47d03d84f0824b5d5ed4e42b39719a3c.png)

不明白就看图吧。