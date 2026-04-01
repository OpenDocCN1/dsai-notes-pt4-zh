# 2023-08-04Jupyter推出免费AI助手，不只会写代码，多种大模型都能调用-今日头条

> 来源：[https://xuqiwei1986.feishu.cn/docx/Wqqvdw5kbonALVxNa18cUYJenQh](https://xuqiwei1986.feishu.cn/docx/Wqqvdw5kbonALVxNa18cUYJenQh)

> 🔗 原文链接： h[ttps://www.toutiao](https://www.toutiao.com/article/7263392138291937807/?log_from=679f0dc123385_1691147329559).com/article/726...

> Jupyter推出免费AI助手，不只会写代码，多种大模型都能调用

2023-08-04 17:14 · 量子位

> 克雷西 发自 凹非寺

> 量子位 | 公众号 QbitAI

自然语言编程的家族，又添新成员！

近日，Jupyter宣布了推出免费开源的AI助手。

安装简单、使用方便，写代码、debug，一个窗口就能轻松搞定！

![](img/8f66e47d6175d57e4c5967e6eb0623d2.png)

这个叫做Jupyter AI的编程神器是用LangChain打造的。

**相比于VSCode**里的Copilot，Jupyter AI 支持的大模型更多 。

除了我们耳熟能详的ChatGPT和Claude之外，还有来自AWS、Cohere等厂商的模型。

甚至只要有API，HuggingFac**e上的模块它都**能调用，这意味着它的作用 不仅仅是编程 。

![](img/b5bb04e37dcb60d8bd1b44824257790d.png)

**此外，Ju**py**ter AI的配置也更**为简便，而且 无需排队 、 支持中文prompt 。

支持的程序也不只是Jupyter自己，只要环境支持notebook或IPython就可以用，VSCode也包括在内。

这一重磅产品的推出，让网友直言游戏规则要改变了：

![](img/563cdec460877e899473405d11384586.png)

我们进行了第一手实测，快来看看结果如何！

# 会写又会跑，还能debug

安装好带AI插件的Jupyter Lab并打开之后，新建一个笔记文件。

然后输入「%load_ext jupyter_ai」对插件进行加载。

之后是对API key进行配置，以ChatGPT为例，使用的命令是：

> %env OPENAI_API_KEY=（填入OpenAI的API key）

其他大模型的配置方法也都相似。

完成这些配置之后，就可以上手写代码了，输入内容的格式如下：

> %%ai 模型名称 --format=(code/math/html/image…)

> 要完成的任务

其中任务用中文描述就可以，format参数也可以不输入。

我们从算法、Python绘图以及小游戏三个方面对Jupyter AI进行了测试。

先来看算法，这里我们直接上难度，来一道困难级别的Leetcode题。

LeetCode平台上，这道题的通过率为30.7%。

![](img/b1133a3a23f82b97c7c5bc1e1d01e9ae.png)

结果代码很快就出现在了笔记的下方：

![](img/aa72cc95fb157ea993c9bbc2eb0c5e67.png)

那么究竟对不对呢，我们在LeetCode上提交一下试试。

直接一步到位！

![](img/162d242d0b0c70a985908ca7a1ded251.png)

如果换成一道数学题呢？

![](img/af8468c7acabdb7bbe2e10824916305b.png)

老铁，依旧没毛病！

![](img/652837101c81184600d138b6b1dfa9e0.png)

算法问题就展示到这里，接下来让我们看看它的绘图能力。

首先让Jupyter AI随便画个什么图形出来，比如两颗心。

结果我们得到了一段调用海龟仓库进行绘图的代码。

**这里就要提到这款插件的另一**个特性—— 生成好的代码可以直接运行 。

![](img/c428c83732bfea2a2672ba6fe0b17950.png)

除了图形，函数图像的绘制也不是问题。

依旧是整个复杂的，y=x*sin[ln(x)]。

![](img/34c50d0123e24e7c5385dacc32c14813.png)

在这个区间之内的确没毛病，不过没能完整展示函数的单调性。

没关系，我们可以 **对AI给出的代码进行手工调整 **，改变一下取值范围。

![](img/65e3fc9ac3ad5711ee855f18c42e1124.png)

根据现有数据绘制统计图表同样是小菜一碟。

![](img/0f5bb24c791bf18a60189f00e389d973.png)

运行这段代码，一张统计图就大功告成了。

![](img/9699021acfa3cf13394bbf03a5c8d291.png)

**不过，这里我们**还想展示一下它的 多轮对话能力 。

比如，这个蓝色看起来不好看，我们想给它换个颜色。

![](img/6eb7e8676776d80b122cbd7c691fb0f1.png)

除了算法和绘图这些内容，交互式程序的编写也难不倒Jupyter AI。

甚至可以做小游戏，比如贪吃蛇。

![](img/9a651d6c031cd5f9ef4aa8c913772649.png)

创制和运行程序的能力我们都已经见过了，那么debug的表现又如何呢？

我们让ChatGPT帮忙出了道题——一段包含五处错误的Python代码。

![](img/8b088d7e93742bbdded2da41a1cafd2f.png)

我们看到，ChatGPT生成的这五处错误全都没能逃过插件的法眼。

![](img/76286e600d59f27cd4b8d320158fd57e.png)

除了修改操作，也可以要求它解释修改的原因：

![](img/71f12ece59d033347ba1c135cd8c993e.png)

我们看到，or改成and的原因没有进行解释，不过没关系，我们可以进行追问。

结果得到了更为详细的解释：

![](img/944425d813a54bc6050be4f93da19402.png)

以上就是关于Jupyter AI编写代码和调试运行的全部测试了。

但它会的，不只是编程。

# 不只是编程

除了和Python程序相关的功能之外，这款AI插件还可以生成LaTex公式、SVG图形、网页等内容。

首先来看公式，我们想让它生成一下麦克斯韦方程组。

只要把format的参数改成math，结果就会以LaTex排版之后的格式输出。

![](img/1b871622d3c503fcdee5afb07210a41e.png)

为了证明这不是图片，我们查看了相关区域的HTML代码，是货真价实的LaTex格式。

![](img/de3fc13cced2552443a2262e319c2f25.png)

简单的网页，这个插件也能做得出来。

我们让它生成一个时钟页面，这里需要把format改成html。

![](img/1848e4af310a1e1872b8a990516791be.png)

说到HTML，就会发现它还有一个妙用。

编程的一节提到过，这个AI插件可以生成Python绘图代码。

但它同样可以不用Python，直接生成一些简单图形，以SVG的形式进行展示。

![](img/e7347ef15096aeb331f1544fd74ca7d0.png)

不过无论是Python还是SVG，画出的图形都比较简单。

那么复杂的图像它能画吗？答案是肯定的。

这款插件 可以调用Stable Diffusion 来画图。

不过由于没有HuggingFace的API，这里我们只能展示一下官方给出的DEMO：

这里似乎需要把format设为image。

![](img/3f13b4cf1e058747c7590c57ae06081e.png)

而且能调用的不只是Stable Diffusion，HuggingFace中的其他模型也可以！

除了在笔记中调用插件，Jupyter AI还在侧边栏中提供了聊天窗口。

配置好API之后，有什么问题直接问就可以！

![](img/e26bfbe9c5dd18ec7c72732866bd099b.png)

还不赶快上手体验一下？ (GitHub页面有安装教程)

GitHub页面：

https://github.com/jupyterlab/jupyter-ai

参考链接：

[1]https://twitter.com/ProjectJupyter/status/1686773501049327616

[2]https://blog.jupyter.org/generative-ai-in-jupyter-3f7174824862

— 完 —

量子位 QbitAI · 头条号签约

关注我们，第一时间获知前沿科技动态