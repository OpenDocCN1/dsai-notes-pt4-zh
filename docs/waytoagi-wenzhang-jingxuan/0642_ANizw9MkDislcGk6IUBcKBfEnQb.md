# 画小二：Coze工作流之抖音热门视频转小红书图文详细配置

> 来源：[https://waytoagi.feishu.cn/docx/KATUds6yXoUhBnxv1ODcZaSmnDb](https://waytoagi.feishu.cn/docx/KATUds6yXoUhBnxv1ODcZaSmnDb)

> Create By：画小二

> [Coze工作流之抖音热门视频转小红书图文详细配置](https://a1i1hjmvcf0.feishu.cn/docx/QEGtdVxXAoXxhBxxBMPcQEYVn4c)

# 工作流目的

## 1、功能说明

将今日抖音热门视频转化为小红书的图文，输入任何行业的关键词讲视频内容转化为图文内容。

画小二设计这个工作流只是为大家提供一个解题思路，掌握这个方法之后可以应用自己的实际工作中来解决具体的问题。

## 2、体验测试

今日抖音热门视频转小红书文案的扣子地址：https://www.coze.cn/store/bot/7360309310729240611?bot_id=true

2024年5月6日晚 在官方小哥哥的协助下，Bug已经修复，感谢扣子官方耐心且协助一步步的排查问题。

![](img/41ad5bec8fda612aebc2a0cea66466ab.png)

![](img/278b718977a9c9fca204674b9c4a7948.png)

![](img/4e730b189c66fc9a7cd56c2f24d23e64.png)

## 3、测试截图

![](img/80224b7087cabd5e8d4945b25c0503a8.png)

![](img/d82b015297b09ee8879d0ab581d00653.png)

# 二、教学视频

<https://player.bilibili.com/player.html?bvid=12b421Y75a>

# 三、工作流Flowchart图

# 四、工作流结构图

![](img/ba8ceb7c6050e6224bdae21568c4224a.png)

# 五、详细参数配置图

## 1、整体结构图

![](img/94c85e0d7a67892c2149d69465b4e56d.png)

## 2、开始模块参数配置

![](img/b376b1f43eee990f5f748fdaad953ccf.png)

## 3、Get_Video模块参数配置

![](img/5f9dbed388c0d5c5fdcfcc51c4512d88.png)

## 4、LinkReaderPlugin模块参数配置

![](img/d79cdf50fc11a654acba78061f4b92b1.png)

## 5、标题大模型参数配置

![](img/67d9f7f82febe8f2678287a8dabf4382.png)

你是小红书爆款写作专家，请将{{input}}里面的内容，首先产出3个标题(含适当的emoi表情)；

在小红书标题方面，你会以下技能:

1.采用二极管标题法进行创作

2.你善于使用标题吸引人的特点

3.你使用爆款关键词，写标题时，从这个列表中随机选1-2个

4.你了解小红书平台的标题特性

5.你懂得创作的规则

## 6、内容大模型参数配置

![](img/fe3c742a1caba23093ff3ab5819b06ae.png)

##内容

你是小红书爆款写作专家，请将{{input}}里面的内容，产出1篇正文(每一个段落含有适当的emoji表情，文未有合适的tag标签)

在小红书正文方面，你会以下技能:

1.写作风格

2.写作开篇方法

3.文本结构

4.互动引导方法

5.一些小技巧

6.爆炸词

7.从你生成的稿子中，抽取3-6个seo关键词，生成#标签并放在文章最后

8.文章的每句话都尽量口语化、简短、文章长短在200字左右；

9.在每段话的开头使用表情符号，在每段话的结尾使用表情符号，在每段话的中间插入表情符号

## 7、图片Prompt大模型参数配置

![](img/94249266faee60923fdcccec3603e563.png)

##AI绘画Prompt

你是绘图Prompt大师，请将{{input}}里面的内容转成AI更容易里面的绘画Prompt

在AI绘图大师，你会以下技能:

1.描写的文案使AI绘画更容易里面；

## 8、文生图ImageToolPro参数配置

在插件中搜索插件名称Byteartist

![](img/b5ce00a9f3f207c9ba9e8209df065adf.png)

![](img/6e78fdf81dfc6cd6721e0a153d6a9e08.png)

![](img/acb5cb4369241d539821d18d667e181d.png)

## 9、输出结果参数配置

![](img/5aade0382d817e8354edea3cad9602ed.png)

![](img/94bc0456acef7b41249a4e26687ac5d0.png)

# 六、应用发布

## 1、工作流发布

点击右上角发布

![](img/af0e75623985d50b85045b249cf13863.png)

![](img/d9d44fe9f595be1dd76de5b705bb0a08.png)

## 2、应用发布

点击授权的应用，可以将应用发布或者内嵌到不同应用中。

![](img/703e561153c39ad0d3a9be045243f5b3.png)

## 3、学习交流群

![](img/6ab1fa007e9d6328a4a42409c06999f0.png)

AIGC商业案例实操课

![](img/8dbb79f3244553cb12f2fd0c135db56f.png)

画小二微信交流群

![](img/84b6348b02e20b0a01cd1b6719b08449.png)

画小二微信公众号

# 七、手把手教学

## 1、会员详细教程

根据以上内容若还是无法掌握该技能，本节视频有41分钟的手把手视频会员教程：

https://m.qlchat.com/topic/details-audio-graphic?topicId=2000021907074215

![](img/4fddaa147f0e246aec0d40722a197d5b.png)

![](img/8d6527c9eaa754ae5aa5c69969df89b8.png)

![](img/b402e6578d00f55d1221732b741a2898.png)

![](img/91453d0cd8ed6a9d18facdb201c9a01d.png)

## 2、AIGC商业案例实操课海报

![](img/8fdc7462e87e6f17a562993203df6bde.png)

备注：本文档教程配套项目文档，版权归画小二所有，禁止商用。