
# CodeSandbox 部署 Clewd

> 来源：[https://sqivg8d05rm.feishu.cn/docx/G8kQdtCsGorD5dxGrdqc3548nvd](https://sqivg8d05rm.feishu.cn/docx/G8kQdtCsGorD5dxGrdqc3548nvd)

【哪里有问题欢迎大家在群里@江贤之进行修改】

CodeSandbox是类似Replit那样的云端平台，可以部署各种项目。

本教程仅教部署反代Clewd，如果想[在沙盒CodeSandbox搭建酒馆SillyTavern](https://sqivg8d05rm.feishu.cn/wiki/DMLgwOTzYiChWekWL5gc99rVn3b)署酒馆SillyTavern可以看这个

**缺点：**

+ 部署起来比Replit稍微麻烦一点

+ UI对新人来说可能不是那么直观

+ 不支持保活`uptimerobot`，不过可以想办法定时刷新浏览器页面

+ 后来更新为免费用户一个月只能用40小时，建议只搭Clewd

+ 可惜人一多还是429哩

**优点：**

+ 颜值高

+ 比Replit性能好得多！（沙盒给2核虚拟CPU、2G内存、6G硬盘，Replit只给0.5核CPU、0.5G内存）

+ 应该也无需🪜（不确定）

+ 从Replit那边分流些用户（用的人多了还是会ip429，所以两边用不同ip多开几个项目，哪个429了就换一个）

+ 项目默认都是私人，不像Replit免费用户的项目全都是公开的

+ 应该没人开脚本扫你的反代，不怕被偷Cookie

本教程参考了坎佬的[云酒馆 + 云Clewd 教程](https://rentry.org/teralomaniac_clewd_render#hugging-face)

## 登录/注册CodeSandbox

1. 打开[CodeSandbox](https://codesandbox.io/)官网

1. 点`Sign in`

![](img/9d6687c62661cf18c9f0f8490197944f.png)

1. 如果你有谷歌账号，点`Sign in with Google`登录谷歌账号，关联`CodeSandbox`即可。如果没有可以自行注册谷歌账号。或者点`Sign in with Github`注册`Github`。

### 注册Github

1. 下面以注册`GitHub`为例，如下图，点`Sign in with Github`

![](img/521bc39a5d8e55d3895d49ffaaa942f4.png)

1. 如下图，点击`Create an account`

![](img/ecb812449f990444adcd17e77eea0b79.png)

1. 如下图，输入用户名、邮箱、密码。点下面的注册即可，邮箱会收到验证码，填入验证即可。

![](img/6476f9097896d17e23bd988b785ee13e.png)

1. 验证后点击`Authorize codesandbox`授权`CodeSandbox`绑定`GitHub`账户

![](img/a11d1270747a89f5fab7d55134c4e892.png)

1. 如下图，授权后会跳转回`CodeSandbox`，随便填一个昵称即可，用户名一般会自动填入你的`GitHub`的用户名

下面俩个选项随便选一下就行

![](img/c891727a420f05f52107d8c420e7c7eb.png)

## 创建属于你的Clewd项目副本

1. 登录成功后，点击`import repository`

![](img/c6db04e18217812b9e7e212659451cf9.png)

1. 点击`Allow all repositories`授权使用你的`GitHub`的库

![](img/cdbe2539f8c778296b76ac4435f285e0.png)

1. 之后在`GitHub Repository URL`中填入`clewd`修改版的`GitHub`项目地址（点我），之后点右边的`Import`

![](img/c69465cfb30a90f7060de85758491718.png)

1. 之后如下图，点击`Fork repository`创建一个属于你的`Clewd`项目副本（也会同时在`GitHub`里`fork`）

![](img/31cdf83ebd4fa84e8c3300594574a128.png)

1. 等它创建好后，如下图，右边会有一个窗口，一路点`Next`，最后点`restart`即可，然后等项目启动。

![](img/6e9816a1bc030258e88f4d76397f0d25.png)

1. 如下图，等它跑完

![](img/b28a88ae9b1ffd919af0edf5b11d8040.png)

1. 如下图，显示这样就是创建成功了，左下角的弹窗可以点`Dismiss`取消掉。（因为`CodeSandbox`会把项目运行在`docker`里，所以有下图的报错很正常，如果你会的话可以自行导入/创建`config.js`文件，不会也没关系接下来我们会设置环境变量，就不会报下图的错误了）

![](img/e3341ecdfaa04891b121aa99ee125e6c.png)

## 设置环境变量

`config.js`里的`Cookie`、反代密码等各种参数就是填入环境变量中

1. 接下来点击左上角的方块，然后在弹出的界面里点击`Settings`

![](img/cdbdd332946145b948f03a83375185c5.png)

1. 点击`Env Variables`

![](img/58af95d3b6b201ef47a102ad56a864d5.png)

1. 到这里，接下来就如同搭建`Replit`云端`Clewd`时把`cookie`填入`Secrets`选项卡一样

![](img/f4e51efe7c84cf9f6c1082ea4aee35ba.png)

1. 接下来要依次在`Name`里填入`CookieArray`、`Cookiecounter`、`ProxyPassword`，然后点击`Add`即可。

其中`CookieArray`的`Value`里填入你的`Cookie`，`Cookie`的形式是`sessionKey=sk-ant-sid01-xxxxx`这样的形式。

如果有多个`Cookie`则用英文逗号“,”分隔，比如`cookie1,cookie2,cookie3`

![](img/f32d3c87aa04dc950e9133ab59a6c2f2.png)

1. 之后填入`Cookiecounter`，`Value`是`1`（`1`代表每次对话就更换一次`Cookie`，`3`代表对话三次更换一次`Cookie`，数字小点可以避免`Cookie`过期，以及轮换`Cookie`避免过快到达对话限制）

如果你想检查`Cookie`是否过期，可以参考[如何查看cookie饼干是否过期、被封](https://sqivg8d05rm.feishu.cn/wiki/UrRBwtwDaib92pkAZDPcylnNn4c)，把刚才填入的`Cookiecounter`改为`-1`即可，运行完再改回来。

![](img/5a4bf58cfa506add80ec7b6402fa2af6.png)

1. 接下来填入`ProxyPassword`和你自定义的密码。（虽然`CodeSandbox`创建的项目默认就是私人的，别人无法访问，但还是建议设置一下。这个密码是要填入酒馆的`Proxy Password`里的）

![](img/8955c57c7cb664cd70f0d4b1143f7013.png)

1. 最终效果如下，没问题的话，点击右下角的`Restart microVM`重启项目即可，在弹出的窗口也选`Restart microVM`

![](img/d535a8650874aa9b373524f36505202f.png)

如果你不喜欢环境变量这种形式，也可以手动设置`config`文件

## 配置config文件

1. 如图点左上角的按钮打开文件列表，左下角点击`.gitignore`，之后在右边，下拉找到`config.js`，给它前面加一个井号变成`#config.js`

![](img/8599cc1a9b35212a5de383e15dcf7e6d.png)

1. 然后左上角点左上角方块，选择`Restart Branch`重启分支

![](img/097082c00a24e976a694b82134ec76f9.png)

1. 重启好后文件列表里就出现`config.yaml`了，没有出现的话可以点一下图里上方的按钮进行刷新

![](img/f6a85fb8eb4106253ee96c37ef8584c1.png)

1. 点击`config.js`如果你只有一个`Cookie`把它填入`"Cookie"`后的引号里代替`SET YOUR COOKIE HERE`即可

![](img/ec1ed8686a4b6b489c00c8f08d62ae66.png)

1. 如果你有多个`Cookie`，需要清空刚才填入`"Cookie"`的`Cookie`，按照如下格式，把`cookie`都填入`"CookieArray": []`中

```
#格式如下，在[]中分别用&quot;&quot;包裹cookie，用英文逗号分隔，但结尾不能有逗号例如&quot;CookieArray&quot;: [&quot;cookie1&quot;,&quot;cookie2&quot;,&quot;cookie3&quot;]
#举例：
&quot;CookieArray&quot;: [
&quot;sessionKey=sk-ant-sid01-bTiK8Mf6XqHODdxmwV&quot;,&quot;sessionKey=sk-ant-sid01-Sokt3wB7c3Yx_PIbLK&quot;,&quot;sessionKey=sk-ant-sid01-YylqgeaI6X0YxIWBS3E3gCN7Fg1hR754lT-RtN&quot;,&quot;sessionKey=sk-ant-sid01-ycbnhdXsaJOnKDOUT_NnvIHejk42n&quot;
    ],
    #不用试啦我给的假cookie
```

1. 在`"ProxyPassword"`中设置一个反代密码，这样即使不小心泄露了反代地址也不会被人偷用`Cookie`

1. `"Cookiecounter"`也可以改为`1`每次对话就轮换一个`Cookie`，避免有`Cookie`未被使用而过期

1. 注意！`Cookie`填入`config.js`的话运行`Cookiecounter=-1`会自动把被封、过期`Cookie`删除

## 启动Clewd反代

1. 修改好后左上角选`Restart Branch`重启

1. 右下角如果弹出这个提示框，点`Dismiss`即可

![](img/e9d8841b3c801ddbdac135b8d82cf930.png)

1. 等一段时间项目重启完毕后，如下图，右上角显示了`clewd`版本号，左下角有你的`cookie`信息，这样就算搭建成功！！点击复制链接复制反代地址，填入酒馆即可

![](img/70b5272e89d7f2672e33250600fc18a0.png)

1. `CodeSandbox`大约可以持续运行12小时，之后会自动关闭，重新打开这个项目的网页应该就会自动重新运行

## 下次如何找到你的CodeSandbox Clewd反代？

1. 打开[`CodeSandbox`](https://codesandbox.io/)官网，如下图被圈起来的，点进去就是你的`Clewd`反代项目啦

![](img/0bafbfb9a37eaf7297d111c0040e785c.png)

## 如何修改Cookie、反代密码

### 环境变量版

1. 点击左上角的方块，然后在弹出的界面里点击`Settings`

![](img/83355529a422b476d96630d9fe368063.png)

`Env Variables`选项的右上角有个`Plain text`

![](img/14ac7dd3666a26fee5806d975eb36e0d.png)

点进`Plain text`即可编辑包括`Cookie`在内的，刚才设置的所有环境变量

当然你也可以删除相应的变量重新添加

### Config.js版

选中`config.js`文件进行修改就行啦

## 如何关闭、手动重启Clewd项目

1. 项目运行后在左下角的窗口有个`start`窗口，点`X`即可关闭

![](img/97ec84de1fcc4b6f081d2c33b4130322.png)

1. 再点`Restart`就可以重启项目

![](img/557324758d4b5d51663c389c968332d7.png)

1. 如果不小心把所有窗口都关了，可以点开左下角的小窗口，点击`start`，`Clewd`就重启啦

![](img/01a63a9071c4101ba5c0ce9aefadfd8c.png)

1. 如果需要重启整个`docker`，比如修改了环境变量、配置文件等，点左上角的方块，选`Restart Branch`重启整个沙盒分支即可

![](img/2bcbe962db05469d679f7c029f566427.png)

## 429了怎么办？如何更换项目IP？

据测试沙盒也会429（报错`rate_limit_error`，不是`Cookie`到使用上限的429）

在`Replit`那边改名+重新`fork`项目即可更换ip避免429，那么沙盒这边有没有相似的操作呢？

有但是效果不一定好

![](img/c98850add29ff501d72bfd3fb0dec09c.png)

1. 打开你的沙盒`Clewd`

1. 右上角有一个`Commit and push`，点旁边的小三角（别点歪）会弹出下拉菜单

![](img/101dafb9bac2bb8db124dc9ea1aaea85.png)

![](img/563d6e4ea4b07e48e67b341ccf27cf66.png)

1. 点下拉菜单里面的`Create Branch from 'master'`

1. 等一会后一个新的分支就创建好了！

1. `CodeSandbox`的环境变量在相同项目的不同分支都是通用的，因此应该直接就可以用了，复制反代地址即可

1. 如果提示`set your cookie inside config.js`，重新去设置的环境变量里配置一下就好啦

## 如何重置整个Clewd项目

如果改错了文件、代码等，导致整个项目无法运作，可以重置整个项目

1. 左上角的小方块点`Dashboard`回到`CodeSandbox`的控制面板

![](img/d5840ddd4bd8a6979d9f9786fe37842c.png)

1. 找到你的项目，点项目右上角的三个小点

![](img/bd3c367214d6c4969dc36676a9325a28.png)

1. 下拉菜单里选`Remove repository from CodeSandbox`

![](img/698a3561f2b8c7318115625bda57d2cd.png)

1. 重新点`Import repository`

![](img/691d356cf0215a179c63caf3d3cbadee.png)

1. 如下图，`clewd`应该还在，点它就行了。如果没有显示`clewd`，则需要重新填入`clewd`的`GitHub`项目地址（点我），再点`Import`，等它跑完即可。

![](img/67a45cd7660973d5c5a4f5ac0b10e871.png)

## 关于更新Clewd

`CodeSandbox`需要手动同步`Github`进行更新

1. 进入`Dashboard`

![](img/f2ba19fe8372463b5be78431290b24d4.png)

1. 找到你的`Clewd`项目，点右上角的三个小点，选择`Open On Github`

![](img/8a071b0fa8c7c83b80b433173281690c.png)

1. 新打开的页面是`CodeSandbox`自动`fork`的`Clewd GitHub`项目

1. 在右边点击`Sync fork`

![](img/c7e701cf441fbfdcaf6e45b4ac7f2921.png)

1. 点击`Update branch`

![](img/d50b454732ae52ea626e7bb3ef26dbd4.png)

1. 之后回到你的`CodeSandbox`的`Clewd`项目，点`Restart Branch`手动重启下，重启后右上角看看版本号是不是更新了
