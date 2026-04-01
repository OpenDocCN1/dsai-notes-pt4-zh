# 如何用AI绘画软件进行换脸

> 来源：[https://agijuejin.feishu.cn/docx/Hr5OdNr7aorIaQxzkZEc5Y9EnPb](https://agijuejin.feishu.cn/docx/Hr5OdNr7aorIaQxzkZEc5Y9EnPb)

> 作者：德里克文

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/635030904)hihu.com/p/635030...

**我来先展示下我用AI换脸的效果**

![](img/009ed8caf415dcce85d9e7be7b90968a.png)

![](img/88054b84e66878f08ed2a993ab407a96.png)

![](img/d7208b659830399a35d943e45d536677.png)

![](img/b5313a19c462a71243dadd86dd01e24e.png)

![](img/0022c2f7a8ae3b3b5d228e8cbb8ea583.png)

![](img/d1bcc80544fe7a93e0ac53ddaa2d0886.png)

在AI绘画现在已经很强大的今天，有许多软件可以做到智能换脸而没有违和感，例如MidJourney和Stable Diffusion都可以，采用多种方法可以实现这个效果。

下面我用Stable Diffusion来举例上述图片如何制作的，如果觉得有用请记得点赞收藏关注哦！

### 一、采用文生图生成头像图片

首先先确定想要的图片，我用带着太空头盔的宇航员举例，先通过文字描述生产图片：

Prompt:

A close-up of a futuristic soldier's face, the glow of a holographic heads-up display illuminating her determined expression. The intricate detail of the HUD, with its myriad of data and symbols, gives a glimpse into the high-tech warfare of the future. The scene, captured in sharp focus, encapsulates the bravery and readiness of our future soldiers.

![](img/8769293aa4c6adbd4ac227f0aa33c2bc.png)

### 二、采用图生图形式进行重绘（基础）

1.  读取刚刚的图片，采用图生图模式

1.  在图生图模式中设定重绘大于0.7

1.  [在关键](https://www.zhihu.com/search?q=%E5%8A%A0%E6%9D%83&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A635030904%7D)字部分修改调整想要的关键字，将不想替换的内容进行 加权 ，加入想要换脸模型的LORA，直接生成即可

![](img/e258cbddd482b1f48b9bd3be5a30da22.png)

### 三、采用Controlnet文生图局部重绘功能进行换脸（进阶）

1.  读取刚刚的原始图片，采用局部重绘功能

1.  在文生图界面打开Controlnet插件，点选启用，预览

1.  在Controlnet [预处理器 ](https://www.zhihu.com/search?q=%E9%A2%84%E5%A4%84%E7%90%86%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A635030904%7D)选择 [inpaint ](https://www.zhihu.com/search?q=inpaint&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A635030904%7D)，将脸部需要替换的部分涂抹，然后点边上的小黄色图标按钮，预览界面可看到脸部被涂黑，接着将 小黄色图标右侧的模型也选择inpaint，再次点击小黄色图标，确认已应用

1.  使用同样的 [关键句 ](https://www.zhihu.com/search?q=%E5%85%B3%E9%94%AE%E5%8F%A5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A635030904%7D)，设置好各项参数，加入想要换脸模型的LORA，点击生成

![](img/df28e151c78959dbaeb767161b3e9b0b.png)

![](img/d7646ff3258172958f088750d45ddeb5.png)

![](img/e2f0bc2ea259a32d89495719f78560f0.png)

通过以上的步骤，就可以采用Stable Diffusion进行换脸啦，是不是很简单呢，如果有不清楚的地方可以在评论区询问，我会及时解答。

我是 [德里克文 ](https://www.zhihu.com/search?q=%E5%BE%B7%E9%87%8C%E5%85%8B%E6%96%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3052677444%7D)[，一个对AI](https://www.zhihu.com/search?q=%E5%AE%A4%E5%86%85%E8%AE%BE%E8%AE%A1%E5%B8%88&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3052677444%7D)绘画，人工智能有强烈兴趣，从业多年的 室内设计师 ！如果对我的文章内容感兴趣，请帮忙关注点赞收藏，谢谢！

编辑于 2023-08-08 08:49 ・IP 属地福建