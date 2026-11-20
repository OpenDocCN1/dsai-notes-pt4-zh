
# CodeSandbox 部署酒馆

> 来源：[https://docs.feishu.cn/docx/SG4SdVOilo1zH5x8ZEocR4vxnPd](https://docs.feishu.cn/docx/SG4SdVOilo1zH5x8ZEocR4vxnPd)

CodeSandbox是类似Replit那样的云端平台，可以部署各种项目。

CodeSandbox给免费用户提供2核CPU、2G内存，比Replit那0.5核0.5G内存好太多了，应该可以很大程度上减缓切换界面的卡顿、导出导入文件时的卡顿。

同时CodeSandbox项目默认都是私人的，不像Replit那样全都是公开的，谁都可以看到你的聊天记录，CodeSandbox只要你不设置公开，就没人看得到你的聊天记录。

仅仅把酒馆SillyTavern部署在沙盒不会导致429rate limite error，因为Clewd反代可以部署在本地或者VPS上，这样就可以白嫖CodeSandbox免费提供的服务器给酒馆用啦

本教程参考了坎佬的[云酒馆 + 云Clewd 教程](https://rentry.org/teralomaniac_clewd_render#hugging-face)

## 注册

1. 参考上一篇注册GitHub、沙盒CodeSandbox

[【施工中】在沙盒CodeSandbox搭建云端Clewd](https://sqivg8d05rm.feishu.cn/wiki/Y7R0wBdFMixvPMk8YRJcQJ8unlb)

## 创建酒馆项目

1. 打开[CodeSandbox](https://codesandbox.io/)官网并登录

1. 登录成功后，点击import repository

![](img/e47426aa2e1cb74f4dbc0a6c95fe180a.png)

1. 如果弹出如下图，点击Allow all repositories授权使用你的GitHub的库

![](img/45bfccb5b333f36e2a68848a45ba911d.png)

1. 之后在GitHub Repository URL中填入酒馆SillyTave[rn的GitHub项目地址（点我），之后点右边的Import

![](img/2ec5bfad08e817706c683ef26e3dcca0.png)

1. 之后如下图，点击Fork repository创建一个属于你的SillyTavern项目副本（也会同时在GitHub里fork）

如果之前在沙盒创建过酒馆可能会提示库已经存在Repository already exists，稍微改一下名字即可，如SillyTavern改为SillyTavern2

![](img/bd52146d1e5a68794465cc3d7cb93658.png)

1. 等它创建好后，如下图，右边会有一个窗口，点Next

![](img/f7b33422a8006b97b8f352b58c4332f8.png)

1. 一路点next即可，最后如下图点Apply and start

![](img/b981a1f48109dfcce3758cfdb877f8e8.png)

1. 如下图，等它跑完，大概要跑个两三分钟

![](img/f2a475816c6392ca7eb0c283fce1e48d.png)

1. 如下图就是创建成功了！右上角有酒馆的网址，左下角有报错。点右上角红色箭头指的按钮即可在新页面打开酒馆

![](img/5677e856f8e5bc9681caa140e6b92522.png)

## 设置密码

1. 如图点左上角的按钮打开文件列表，左下角点击`.gitignore`，之后在右边，下拉找到`/config.yaml`，给它前面加一个警号变成`#/config.yaml`（CodeSandbox配置的环境变量似乎对酒馆不生效，所以要这样修改配置文件来启用酒馆的账号密码验证）

![](img/19feed128833923854e062c5f43f76e3.png)

1. 然后左上角点左上角方块，选择Restart Branch重启分支

![](img/d5dc571cf050aa41245f3df37878f492.png)

1. 重启好后文件列表里就出现`config.yaml`了，没有出现的话可以点一下图里上方的按钮进行刷新

![](img/31955c0513ce1d7c8ccc66f074ba7533.png)

1. 点击`config.yaml`，全部参考下图所标记的进行修改

①确保`listen`为true

②把`whitelistMode`改为false

③把`basicAuthMode`改为true

④`username`设为你的账号，`password`设为你的密码，都需要用英文引号括起来，参考下图

⑤改好之后再次左上角选Restart Branch重启分支

![](img/591c85da9d4519b465339970282b3542.png)

1. 重启好后如果提示登录点取消即可

1. 如下图，在右上角点小按钮，在新窗口打开酒馆页面，在新页面输入你刚才设置的账号密码登录

![](img/211dd373a189fc8c3d1e235c87969c0c.png)

1. 输入你的角色昵称点Save

![](img/d8e7337519c4d53f3ee6a7a9664fc239.png)

## 配置酒馆

1. 下面就是配置酒馆，会的人可以去填Clewd反代地址开始用啦

1. 不会的人，先点最上面整数第二个的插头（API连接）

![](img/7aab7b3a93fee0b9706df2c0363f4dfb.png)

1. 然后在API的下拉菜单里选最后一个Chat Completion

![](img/ab796ac99d27f39e4bcf8292e2570f1c.png)

1. 如下图，再点最左上角的菜单“AI响应配置”，在左边弹出的窗口的OpenAI API 反向代理里填入Clewd反代地址，Proxy Password填入Clewd密码

![](img/5d5d97531fc59cb8176af652c9dc9752.png)

1. 再回到小插头，勾上自动连接到最后设置的 API 服务器，点击Connect，如图显示绿色并有Valid就算完成了

![](img/363d36cb20f3eedd5c43c279d13df0e0.png)

1. 接下来去群里下破限、角色卡、世界书导入后就可以游玩啦

## 如何重启酒馆

1. 如图，在左下角找到>_ `start`小窗口，点击x即可关闭酒馆程序

![](img/ea73fb70a4f2e9de8924e44d6f6cc399.png)

1. 之后在上一步的相同位置，点击下图小按钮，会有下拉菜单，点击`start`即可重启酒馆

![](img/124b53dff82abeacf5400ab692402621.png)

## 如何下载聊天记录、人物卡

1. 如图点击左上角的EXPLORER小按钮显示酒馆内所有文件，找到下图`public`文件夹

![](img/a9609b46b46173548e05dff27636a8b1.png)

1. 对`public`文件夹点右键，会有下图菜单，选Download，

![](img/a2fb2947c26cbe7d78a84e2158bca279.png)

1. 在弹出的窗口中选择一个你电脑里的文件夹（别选桌面，最好自己新建一个文件夹）

1. 如果弹出如下窗口，点查看文件同意沙盒浏览你选择的文件夹，以及点保存更改

![](img/7cca9844ca6e53d3976dd56903c60bf1.png)

![](img/a2019087e21a40889589a948d1e255dc.png)

1. 然后左上角会有个蓝色小标记一直在动，就是在下载所有聊天记录、角色卡了，根据你的网速可能会下十几分钟

![](img/6b9da4a09d438ce182dfab8824097236.png)

1. 也可以在上面第2步点Upload上传之前下载的文件
