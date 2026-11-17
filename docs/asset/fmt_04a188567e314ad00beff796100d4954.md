
# 安卓本地部署 ClewdR+酒馆

> 来源：[https://sqivg8d05rm.feishu.cn/docx/QRKtdyeitonHzMxZqMxctu0Pn1g](https://sqivg8d05rm.feishu.cn/docx/QRKtdyeitonHzMxZqMxctu0Pn1g)

作者：

旧版教程：[安卓本地部署 Clewd+酒馆 Clewd旧版](https://sqivg8d05rm.feishu.cn/wiki/VMcRwXTbki3iDTkGd3DcH7fInNb)

！之前已经装过`Clewd`，需要使用`ClewdR`的，请从[配置`ClewdR`](https://sqivg8d05rm.feishu.cn/wiki/EY5TwjuwliCwZpk7Gy7cPGH1nvb#share-Br8AdLMDjoROIwx5XiQcFjB1nZf)开始观看喵。

如果以前搭建的酒馆启动失败/闪退，请主界面输入退出剧本，然后输入`exit`，再依次输入下面两行命令：

`pkg update && pkg upgrade -y`

`pkg install git nodejs -y`

完成后重启`termux`，选2酒馆设置，a重新安装依赖。

## 一、喵喵~一键安卓脚本部署（Termux）

> 视频教程：https://www.bilibili.com/video/BV14W4y1P7Tg（被人举办了，摸了，下次一定，听说现在b站一个脚本卖5块钱，哈哈哈）

需要准备：

+   一部安卓手机，大概需要2g的存储空间，400mb的流量。

+   稍微好一点的魔法（魔法很重要）只有游戏加速器的对照换一个：

+   [霍格沃兹魔法|使用工具|订阅导入全系列教程](https://sqivg8d05rm.feishu.cn/docx/SgfCd73rLowe9vx46kVcuZdRn3b)

+   [注册/抓取曲奇](https://sqivg8d05rm.feishu.cn/wiki/HST5wAAxeibfzEkrCvHcIU8dnge)个能[用的Cook](https://h5.m.taobao.com/awp/core/detail.htm?ft=t&id=801966990488)ies或者直接Api，参考：或者水秋喵的小店。

### 配置环境

1.  在群文件中或者[网盘(点击下载)](https://www.123pan.com/s/P8OAjv-Glhtv.html)找到`Termux`并安装。如果安装不成功请检查手机是否已经装过`termux`/`zerotermux`，部分手机安装时需要关闭网络。

1.  启动`Termux`，打开`termux`,输入下方的完整命令并回车。脚本编辑：

```
curl -O https://raw.githubusercontent.com/heiqiu47/termux_using_Claue/main/tisac.sh && chmod +x tisac.sh && ./tisac.sh
```

1.  等待脚本自动下载安装，直到出现脚本主界面，如果出现报错，请检查自己的魔法是否有问题。安装失败或者中途卡住多半是魔法问题，换一套配置重新运行指令即可。

### 配置酒馆

1.  输入`2`以启动酒馆，出现绿色网址（如：`http://127.0.0.1:8000/`）代表成功启动；复制网址到浏览器打开，打不开的话尝试换个浏览器。

> 注意：`Termux`需要挂在后台才能正常运行，请使用锁后台等方法对`Termux`进行保活；

> 华为/荣耀：手机管家-启动管理-`Termux`设置为手动管理-勾选允许后台活动。

![](img/409b745896fcf2f03075f8c34bd72c48.png)

还是进不去的话，把`termux`挂小窗。

1.  稍等片刻弹出欢迎页，填写**用户名**进入酒馆（不要学我填水秋喵！）。

![](img/b7efb37411747b89079d0eba48bf8b9b.png)

恭喜你，**酒馆现在已经安装完成**。

## 连接AI

酒馆已经安装完成，可以正常使用，下一步是连接AI，有各种方法可以选择，多选一即可。

> 不是只有这几种方法，教程里没有写的，其他地方的API也可以，只要能连酒馆就可以。

---

### 使用第三方API（可选）

Claude Gemini等，点击蓝色链接直接跳转：

[连接第三方API（OpenAI格式）](https://sqivg8d05rm.feishu.cn/wiki/TS8zwRkqyiZk8SkCGRTcNMg6nMd)

![](img/173065d8ea3e5c193caaf9fa5bed3230.png)

---

### 自行注册Gemini（可选）

[如何获取&使用Gemini API](https://sqivg8d05rm.feishu.cn/wiki/AS93wkBv6i6FPyk1a8CclaJYnvf)

---

### 使用Clewdr➕Cookie（非必须，不推荐）

1.  回到`Termux`，对着手机左上部分边缘屏幕右滑，然后点击`NEW SESSION`。第一次会比较难滑出来，实在不行可先在手机系统设置中关闭全面屏手势。

[zerotermux设置方法](https://sqivg8d05rm.feishu.cn/wiki/DIuDwmwikigBx1k7TugcxNbwnYb)

新建后有两个窗口，一个用于运行酒馆，一个用于运行`ClewdR`，两者需要同时启动。

![](img/4e53a5cc9d7e321626e93a2bf643c69b.png)

![](img/c8e93699e817fc620f7c3bd76da33129.png)

检查页面是否有0. 启动`clewdR`，没有的话，选0. 更新脚本

看清楚，是选 0. 启动`ClewdR`，选这个！

![](img/69d217220ba83243d37f89da16915cfd.png)

1.  输入0启动`clewdR`，在这里记住两个密码。请自行保存`API Password`（代理密码）和 `Web Admin Password`（管理页密码）。

1.  进入`http://127.0.0.1:8484/` 进入网页后输入你的`Web Admin Password`（管理页密码）。

![](img/2a507108dd903c9859acfcac888cccdb.png)

![](img/d6e8d60e5ce23cbafe9ebc49cce0b14f.png)

认证令牌为`Web Admin Password`。

1.  将自己注册的`Cookie`，或[在我这购买的`Cookie`](https://h5.m.taobao.com/awp/core/detail.htm?ft=t&id=801966990488)输入进去，每行一个`Cookie`，格式为：`sessionKey`开头`AA`结尾。

例：`sessionKey=sk-ant-sid01-opkKysF5JXpf7IaQFcRETwWBUu7YH_YvQTH-rdkSfgAA`

如果添加失败，将`termux`挂小窗/重启`temux`试试。添加完点开`Cookie`状态看看。

![](img/b4a8505d637e2f9aa5f6a4ad3dcdf789.png)

![](img/ddf486859e218b249449f4eda05ba5ec.png)

![](img/2493c28b73bd4b39ddc6d03dfa2cabb0.png)

1.  回到酒馆界面，按照图示设置酒馆

如图配置即可：

+   代理服务器URL填`http://127.0.0.1:8484/v1`

+   代理密码填你的`API Password`（在上文提醒你注意的那两个密码之一，`ClewdR`后台内有显示）

+   禁止在“OpenAI API 密钥”内填写任何东西

![](img/3ed6ea52a022cd7d49c10d3c2960739f.png)

1.  点击`connect`，下面变成`Valid`就可以了。

![](img/86b30210281e10858fe7a6428301d6a2.png)

如果有bug反馈一下（应该是没有的，而且如果有，我比较懒，不一定什么时候会去改）

**已完成，下一步：**[酒馆入门（必修）](https://sqivg8d05rm.feishu.cn/wiki/J8NDwc06JiuHXmk5cP2csfP6nmd)

喵喵的一些提示(≖_≖ )，有什么问题可以在群里问，群号在宝宝教程首页。

1.  `ClewdR`网页设置里开启网页搜索，普号会从25k提升到128k。

![](img/72076e5bb6f8e69a381e219cfa243c0b.png)

1.  需要使用预设

![](img/5e478927dc331dd6ab32184e3e404524.png)

> 题外话：作者hoping喵是保底的🔥🐱（月月给你们一拳！）（水秋喵给你们好多拳！）

## 一键脚本的相关设置

### 使用MT管理器（必备！）

1.  应用商店下载并启动`MT管理器`，并确认`Termux`已经启动。（启动时才能管理`termux`文件）

![](img/c1355a7d2cf90556bdd08bd39b9786ec.png)

1.  点击左上角菜单(三个横线)，再点击右上角更多(三个小点)，选择+添加本地存储

![](img/44791d085fe656e2216c7ce642358e88.png)

1.  继续点击左上角菜单(三个横线)，选择`termux`，点击使用此文件夹。

![](img/42e89dd29578a990910567a7cc7ec881.png)

1.  点击左上角菜单(三个横线)，选择`Termux Home`，此时已经可以管理酒馆和`ClewdR`文件（在`root`文件夹内）。

![](img/d49f7fc81e9f0c6fcb9de8f23694a81f.png)

### vim使用方法（重难点）

1.打开文件后一开始随便按动键盘都是输入不了任何字符进入`config.js`文件的：

![](img/3ac333b573ebf883ea9583049d387182.png)

2.按一下键盘上的英文字母`i`进入编辑模式：

![](img/56f5c011b85a10aa7a2f5a4db054505c.png)

3.编辑完成后按一下`ESC`退出编辑模式：

![](img/455760896e07388b8da7085b94ba08ca.png)

进入编辑模式后，找到`CookieArray :[]`这一栏，在`[]`里面输入`cookies`，本地部署需要使用英文双引号`""`来包裹每一个曲奇，英文逗号`,`隔开，曲奇太多的话，可使用一键整理工具来处理

> 标准格例如：`["cookie1","cookie2","cookie3","cookie4"]`，

4.这时候输入英文`:wq!`进行强制保存（注意：输入的是`:wq!`），`:wq`是普通保存，一般情况下两个都行，输入完进行回车。（如果不小心无处或者弄错不想保存，则输入`:q!`进行不保存强制退出）

![](img/18c0314b4b8c7fa2bd4b111e4ab84815.png)
