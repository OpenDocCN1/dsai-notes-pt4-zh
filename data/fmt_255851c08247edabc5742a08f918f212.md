
# 2023-05-26-0分钟手把手教会你在自己电脑上搭建一个官方原版的ChatGPT

> 来源：[https://xuqiwei1986.feishu.cn/docx/AkYfdlBUlo0FHWxK2OFc1rL5n7g](https://xuqiwei1986.feishu.cn/docx/AkYfdlBUlo0FHWxK2OFc1rL5n7g)
>
> 原文链接：https://articles.zsxq.com/id_fz10r29x5vqz.html

10分钟手把手教会你在自己电脑上搭建一个官方原版的ChatGPT，不用调用API，不用上网工具。

再也不会出现任何网络问题导致的报错提示，稳定使用ChatGPT无压力！

尤其适合平时连网不方便，或者网络不稳定但又想稳定使用原版ChatGPT的用户。

这个操作主要使用的是GitHub上的一个开源项目Pandora，通过`docker`把ChatGPT打包到本地电脑上，直接就能通过国内网络使用原版ChatGPT。

### 注意事项

1. 需要自己有一个ChatGPT账号；
2. 搭建过程中需要“科学的网络环境”；

## 第一步：下载Docker

下载网址：[docker.com](http://docker.com/)

根据自己的电脑系统下载对应的版本即可

下载好之后点击应用程序运行安装

![](img/d220e32f98acb4b8fb1f2781dd00354f.png)

点击运行之后会出现这个界面，勾选Add shortcut to desktop

（如果这个页面出现其他被勾选的选项，请取消勾选，只勾选这一个）

安装过程（根据电脑配置大概1到10分钟不等）：

安装好之后点击Close and restart，重启电脑：

![](img/4b2ef1d2f8700a95ffbc5764e2fb14c8.png)

电脑重启之后，`docker`会自动运行并出现下面这个页面，点击Accept：

![](img/ed0a1a843d86c2fe4b62cf2ad0a1bec4.png)

之后会出现这个页面，提示我们需要升级当前Windows的子系统，复制提示里的`wsl --update`：

![](img/7b0a0da77cbab5a46677e09b70d875dc.png)

在电脑搜索栏里搜索`PowerShell`，点击以管理员身份运行：

![](img/88b397e5c403d42a4b3fb6a8325cb8f6.png)

进入到这个页面之后把刚才复制的内容复制进来，点击回车键：

![](img/7a7774e48831310e97f27b8d035ade44.png)

正在安装：

![](img/ccdf86f4cc0e0060e8171a62bd00c3ef.png)

安装成功：

![](img/572c330d684e2e8cc3caa6a4687ecf05.png)

再次打开Docker，出现这个提示，直接×掉：

![](img/290cba855e4fa2f09bfcc5397c662bcf.png)

现在就成功进入Docker主页面了：

![](img/f6711e4424f2b3edc89419da0e0992ba.png)

## 第二步：安装Pandora项目

在电脑搜索栏搜索`cmd`，点击以管理员身份运行：

![](img/3ebb45528fe0951ad3596f2e281e9f25.png)

下载Pandora项目，输入：`docker pull pengzhile/pandora`，点击回车键

![](img/78423898ca379849439d9002e0219cf3.png)

正在下载：

![](img/247c48c501adb3e14a2bc0cb80b34410.png)

下载成功：

![](img/68f79fcafa02b44671829d7da84d0980.png)

安装Pandora，输入：

```
docker run -e PANDORA_CLOUD=cloud -e PANDORA_SERVER=0.0.0.0:8899 -p 8899:8899 -d pengzhile/pandora
```

点击回车键：

![](img/3a0a6aec5703902031e0b214c92a87de.png)

安装成功：

![](img/cff9e666694082933d24b013032a54cc.png)

打开Docker，看到这个页面就说明本地版ChatGPT搭建成功了：

![](img/8305ec1638056172136d0655d7164739.png)

## 第三步：登录ChatGPT

打开网址：`http://127.0.0.1:8899`

进入登陆页面，在这里输入账号密码，就可以直接进行登录了

![](img/c7c9f22703913e1e05a2f5f9a2b109a6.png)

登录上去之后，使用方式和页面基本是完全一样的，并且本地版的数据和官方网站上的数据是互通的。

在这里进行提问，官方原版网站也会同步更新。

当然，使用本地版最大的好处就是不会受网络环境的影响，使用起来稳定畅通。

这个是官方网站页面，数据同步更新，可以对比一下：

但是到目前为止这一步，我们是还不能通过本地网络连接的。

需要我们获取账号的`accesstoken`，使用登录`accesstoken`进行登录才能实现本地网络访问。

首先登录官网的ChatGPT，然后打开这个网址：

`http://chat.openai.com/api/auth/session`

复制“accesstoken”后面的这一长串内容：

![](img/8ebdf2a70fc3f081b91566955bb36461.png)

有的人打开网址后可能是这个页面，同样复制“accesstoken”后面的这一长串内容：

![](img/4d86b0906633f66a5c8b0d23449e4fdf.png)

然后打开链接：`http://127.0.0.1:8899`

选择用`accesstoken`登录，并在上方输入自己账号的`accesstoken`，点击确定即登录成功。

![](img/20a3031946ff9d8f15085977a7d7b7eb.png)

`accesstoken`只要登录过一次，半个月内我们就可以通过本地网络直接使用ChatGPT，无需其他网络工具。

## 第四步：本地使用ChatGPT

完成上面所有步骤之后，我们的本地ChatGPT就搭建好了。

使用的时候，打开`Docker`，点击运行按钮

![](img/6499ccb1c71dd68f4180307181509667.png)

显示运行状态后，我们就可以直接在浏览器输入`http://127.0.0.1:8899`进行使用了，整个过程不用再用其他工具。

![](img/eab19f74f459bd10175195adec326c56.png)

并且对于Plus用户来说，可以直接在这上面使用移动版的GPT4模型

这个模型是iOS版APP上的，一般需要通过iPhone才能调用，但是在这里我们可以直接使用。

据说移动版的GPT4模型和网页版的GPT模型可以3小时各使用25次，这个我还没有测试过，等测试过之后，再来更新。

![](img/0bfb8670cd90816173d0f3c6e6d52501.png)

![](img/b83f134a3504b9fb7ba678d6d1894e08.png)

扫码加入星球

查看更多优质内容

![](img/bb6211d85d9524d3f5cce5750d1b15bf.png)

https://wx.zsxq.com/mweb/views/joingroup/join_group.html?group_id=1824528822
