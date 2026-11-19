
# 角色卡编写

> 来源：[https://sqivg8d05rm.feishu.cn/docx/EnFXdhtg0omyCSx7RIac0koon4g](https://sqivg8d05rm.feishu.cn/docx/EnFXdhtg0omyCSx7RIac0koon4g)

## 目录

[白羽的Claude记忆理论和XML简易格式](https://sqivg8d05rm.feishu.cn/wiki/HX2EwPov0iyC4KkSLg8cXkoOnab)

[角色卡的基本原理](https://sqivg8d05rm.feishu.cn/wiki/VpAew3FE4ia1P4kpL4tcNaH0nvn)

[`PLists` + `Ali:Chat` 写作指南](https://sqivg8d05rm.feishu.cn/wiki/UmWhwf6zviRZ7SkC1rhcIgFXnnd)

[車卡教學 `PList`+`Ali chat` 傻瓜版](https://sqivg8d05rm.feishu.cn/wiki/Vm1swh08HizFJhkCDfNcC6uDnAd)

[通过正则表达式来自动触发世界书以节省token和更好地触发不同好感度等不同阶段下人物的性格 副本](https://sqivg8d05rm.feishu.cn/wiki/UApgwixckicvlPkDWNocwJpHndc)

## 水秋小编对`性鼻炎`的采访

经过小编的长时间询问，知名停云卡作者 `性鼻炎` 这样说到：

![](img/43762a2f6c63a48959daf4f3b703263f.png)

![](img/67ee53b16c5d6ab8cd35d8833e4457e3.png)

![](img/796587287c2d3290a0a2bb93bb975839.png)

![](img/a9b96d2160ee6ca25a7d720627038bc1.png)

知名豆之RPG制作人`豆粉`评价道：

![](img/9cd21ef0a5e74eab6508d766647219a4.png)

## FAQ

> 评论：

> 佬快出写卡教程吧！！，老有人拿抽象卡来找🍬🍬，平均每天要骂一两个作者💧，有模板就很好了，有的人把越狱提示、`slack`模板塞角色描述里，角色描述只有两句话插一堆世界书最中间，还把`{{char}}`和`assitant`混起来甚至倒过来用，高级定义里主提示越狱提示再全给你覆盖掉，`greeting`只有一句还没有人称

> 最后过来问🍬的预设为什么会抢话/字少/道歉/掉功能……💧😰

快去看的教程！[世界书](https://sqivg8d05rm.feishu.cn/wiki/ZJ9Yw0X7JipJrlkAoT0cku54nMb#part-WUFbdd1KmoCjU6xdOxgcGdJ6nub)

：

+   角色卡的高级设定里有一个塞越狱提示的地方，如果不在末尾加入`{{original}}`就会完全替换掉预设自带的默认越狱提示，在预设没有专门留空默认越狱的情况下可能会造成问题。

+   在`clewd`发给Claude的消息格式中，`Assitant`是AI的角色，`{{char}}`则会被酒馆自动替换为你的角色卡名称，因此大部分预设和角色扮演卡的逻辑是要求`Assitant`撰写`{{char}}`的行为。所以，如果不是系统卡，请对`Assitant`而不是`{{char}}`作出规则和格式类要求。

* 例外：部分预设直接要求`Assitant`扮演`{{char}}`，这种情况下类似的要求是不会引起歧义的。

：

`greeting`一般指的就是第一句话，用第三人称就好了.....（比如我写的水秋卡，第一句话里面关于角色我就全部用的“水秋”指代角色了），然后就是一般情况下越后面的内容越容易被AI读取，所以重要的一般写后面，然后就是......全部覆盖主提示....我不知道怎么说了
