
# Coze视频生成插件使用说明

> 来源：[https://zinxkclpk97.feishu.cn/docx/EaHXdNeZWoMPtFxxZBHcntoYnzf](https://zinxkclpk97.feishu.cn/docx/EaHXdNeZWoMPtFxxZBHcntoYnzf)

![](img/6eab19bc059a28b99112365cc2aede27.png)

[20250412210930_rec_.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YIofbYNftoDsuexOl7rc8P8Hnjh)

插件地址：`https://www.coze.cn/store/plugin/7432231425165852699`

![](img/702db1babe97253f04f409a1851cc3a6.png)

## 创建视频任务

参数如下

![](img/4c755e9a3c277e98aead0462e1599ca5.png)

`token`需要先到这个地址进行注册：`http://chat.fyshark.com/public/#/accountList`

点击主页获取使用教程：`http://chat.fyshark.com/public/#/`

![](img/cf555a374286ee3d4e01102b9a648814.png)

## 获取海螺AI Token

首先是在海螺AI官网，国际版，登录你的账户。

+ [使用google](https://hailuoai.video/)游览器，前往海螺AI官网（推荐使用国际版）-> 前往海螺AI官网

+ 点击F12，打开浏览器调试模式,找到Newwork点击

+ 随便上传一张图片点击绘制，然后在Newwork中找到`video?device_.....`点击它

+ 随后点击`Headers`往下滑找到`Token`，随后复制整个`token`

+ 将`token`复制到输入框中，点击保存

+ 最后打开账号列表，点击添加账号，将`token`粘贴到输入框中，点击添加等待即可（注意：等待时间较长）

![](img/47b9ecc4686c9c211ff7e9a667f55efc.png)

![](img/b67a09d9569a7dbfe6280051641e5c6f.png)

![](img/9aaa2276b5a0db1f11b6a3e326c438d5.png)

添加完成后，就相当于使用这个AI挂机平台作为中转，再配合上面的插件来使用。

路径原理：Coze插件-->挂机平台-->海螺AI

Coze插件配置的是挂机平台的`token`，获取方式如下。

`http://chat.fyshark.com/public/#/accountList`

在挂机平台，打开F12，也是在network页面获取。

![](img/e0067f5c38071a25f48de83540c0cca7.png)

![](img/036c5afb24c91991afb53318f7ce402e.png)

`model_id`，目前也与海螺AI官网支持的模型相对。(`I2V-01-Director`、`I2V-01-live`)

![](img/5c7d339800cfba337fbb43641da3c5a6.png)

最后需要注意，这种方案依赖于你个人的海螺账号，消耗的也是你个人额度，所以如果想要快速不排队，视频无水印，还是需要氪金买会员的。

如果有需要可以去闲鱼或者找小伙伴拼车，这样成本低一些。
