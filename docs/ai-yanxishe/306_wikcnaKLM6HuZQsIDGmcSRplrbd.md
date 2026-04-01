# 如何用chatgpt-4来实现抢票脚本

> 来源：[https://aiyanxishe.feishu.cn/docx/OiMKd6xz0oQmnIxPcmUcHB0ZnMj](https://aiyanxishe.feishu.cn/docx/OiMKd6xz0oQmnIxPcmUcHB0ZnMj)

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/p_U9hPBfDKFXK0ixk76-Kg)q.com/s/p_U9hPBf...

怎么让chatgpt-4来帮助我们写抢票脚本

票。

我们仅出于学习的目的，来看看是如何实现的。

下面，我们来看看，我是如何用 ChatGPT** 辅****助**实现的。（不**想看这些枯燥的**技术，想直接要 源码 来玩玩的， 直接划到最后 。）

**2 提需求**

因为 ChatGPT 还没有联网功能，所以需要自己去操作一遍买票流程， 并把这个 流程告诉它，这样它就会帮你设计好应该分哪些模块来实现。

![](img/b4660f32bb2b0d19ed8ba4121d9debb2.png)

**3 实现各个模块**

![](img/83ec5b1557d1f1e649febd20261a14f3.png)

![](img/8b789a0fab68ab8f26733276a2a096e1.png)

4 当实现不符合需求时，及时修正

![](img/f44f5d1fd753c544bdab7872d282b372.png)

![](img/3073c46615666312a5c00d99f83d4a0f.png)

**5 解决人工验证的问题**

![](img/9998e22627e8a1044fac75c3def310b1.png)

实现一个抢票系统涉及到下面这些技术和工具：

+   **Python： **用于编写整个脚本和各个功能模块。

+   **JSON****： **用于配置文件的存储和解析。

+   **Selenium****： **一个Python库，用于模拟用户在浏览器中的操作，如打开页面、点击按钮、选择选项等。

+   **ChromeDriver： **Selenium库需要与浏览器驱动程序一起使用，ChromeDriver是用于控制Google Chrome浏览器的驱动程序。

+   **CSS****选择器： **用于定位网页元素，如按钮、输入框、列表等。

+

此外，在编写和调试脚本时，还需要熟悉HTML和网页结构，了解如何通过查看网页源代码或使用浏览器开发者工具来定位页面元素。

而我，在今天之前，还从来没有学习这些技术。

不过，这有啥呢，我不会的，ChatGPT包会的呀。