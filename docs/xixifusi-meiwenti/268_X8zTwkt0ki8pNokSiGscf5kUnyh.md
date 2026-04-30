# Mac M1 anaconda的一系列问题

> 来源：[https://docs.feishu.cn/docx/H9uNd4b53o8WCAxXX9VcBAxjnbf](https://docs.feishu.cn/docx/H9uNd4b53o8WCAxXX9VcBAxjnbf)

# 1 安装

我之前是安装了miniforge3，所以得先卸载

参考这篇，卸载完之后，参考这篇安装

https://zhuanlan.zhihu.com/p/620581344

这篇能解决个大半，但是有个问题是如果你安装过了，就会安装失败，属于是卸载不干净

文件路径一般为 /Users/xxxx/anaconda3/

所以需要先卸载这个 rm-rf anaconda3

# 2 mac m1 如何下载python3.7的conda环境

https://www.cnblogs.com/AudreyXu/p/17348624.html

# 3 本地的pip和conda的一系列问题

在conda环境下用pip安装依赖的时候，默认用的是本机的pip

![](img/dd81d5ffd6f3333c1d7e2170e1b24a79.png)

可以看到pip是在/opt/homebrew/bin/pip，python3也是一个路径

而python是在我的anaconda3下面，ls一下，发现也有pip，所以需要用anaconda3下的pip去安装

![](img/1000c050737061bbb2972e0421239baa.png)