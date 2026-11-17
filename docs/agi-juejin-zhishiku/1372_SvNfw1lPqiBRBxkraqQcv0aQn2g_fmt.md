
# 你不知道的prompt技巧

> 来源：[https://agijuejin.feishu.cn/docx/QYY6dDG7eoJqYQxYI0WcKMnrnZe](https://agijuejin.feishu.cn/docx/QYY6dDG7eoJqYQxYI0WcKMnrnZe)

> 作者：爱吃牛油果的璐璐

> 原文：https://zhuanlan.zhihu.com/p/640254754

首先，我们来看第一个prompt技巧：

## 1、使用清晰、明确的描述，避免模糊的词语，例如可以加上数字，具体的风格：

![](img/d3e30f760425147c1f5ca632c42d0261.png)

![](img/bea24968bbfa570bc3eb28d9580db56d.png)

我们继续来看第二个技巧：

## 2、把指令放在prompt开头，并用三个引号"""或者三个井号###等特殊的内容将待处理的内容和指令分开。

![](img/300641adaafd151a558af69362457f20.png)

我们知道写代码需要具有可读性，写prompt也是一样的通过分隔符换行符等符号可以让大模型更好的理解指令。

第三个小技巧是：

## 3、为了得到稳定的输出，最好指定好输出格式

![](img/006db21e533a6d14cbd95f34d90d5b3f.png)

左边的例子指定了用逗号输出的格式，可以看到每个输出都按照指定的格式输出了，右边的例子可以看到按照主题加下划线的格式，以及每个子标题都用了-进行输出。在实际的应用中，我们有时候会将大模型的输出作为一段代码或程序的输入，往往需要指定为json的格式，例如下面的例子：

![](img/15bcacc22eedac9b50bc41f5277a4c9d.png)

## 4、使用角色扮演技巧

![](img/7d913696100469363c66027dc0f9b160.png)

如果是一个没有进行角色扮演，只是简单的输入指令“给我一个减肥的计划”，我们可以得到下面的回答：

![](img/4ec35603dcc8f33abd9a9d41d4825aeb.png)

当我设定了角色扮演之后，chatgpt的回答立刻变得不一样了：

![](img/492ca64793449ac5b3d6f9c4e56ad30e.png)

除了告诉gpt是什么角色，还有第五个小技巧

## 5、告诉gpt用户是什么角色，这样你得到的答案会令你感到惊艳

举个例子：如果你问chatgpt怎么提高英语成绩，你得到的答案往往是这样的：

![](img/cf37a8343e31a027a9d3728c02a544e0.png)

但是如果你这么问chatgpt，它的答案就是这样的：

![](img/b03e09fca44c1a7bc36ff466b526c61c.png)

我们再来举一个例子：

![](img/a247fcd827d90285cb74ddf1acf5421a.png)

通过上面三个例子，我们可以看到如果加上用的角色，ChatGPT的回答会更有针对性，也会更加个性化。

好了，以上就是今天的分享，当我们掌握了以上五个prompt技巧之后，就可以应付绝大多数的场景了。

## 更多

[爱](https://zhuanlan.zhihu.com/p/670039833)吃牛油果的璐璐：万字长文学习提示学习和微调（Prompt Learning & Prompt Tuning） ?
