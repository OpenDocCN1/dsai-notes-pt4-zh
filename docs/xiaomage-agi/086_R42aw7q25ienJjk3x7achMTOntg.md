# ChatGPT 接入飞书教程，创建自己的聊天机器人（保姆级教程）

> 来源：[https://xmgtoagi.feishu.cn/docx/RsgMdvKYfozGLNxNcQScDJ2cnRg](https://xmgtoagi.feishu.cn/docx/RsgMdvKYfozGLNxNcQScDJ2cnRg)

首先，准备三个账号：ChatGPT账号、飞书账号和Aircode账号。

飞书账号请自行注册，访问链接 https://www.feishu.cn/ 即可登录。

Aircode账号注册方式如下：访问网站 https://aircode.io/ ，点击页面上的“免费注册”按钮，填写表单并选择相应的选项即可。

![](img/4e9969c581687d2b7804032eede164b8.png)

然后选择邮箱注册。

![](img/fa6175caad153f0a40a99aff7f0cc6e3.png)

然后输入邮箱，下一步。然后等几秒，邮箱会收到验证码，验证一下就完成注册了。

![](img/164fe248a8c3a7d39b1727fa47616197.png)

重点来了！接下来讲搭建教程。

# 第一步，飞书进入开发者平台。点击创建应用。

![](img/58318e20892560df40b24d708e572eb1.png)

![](img/c5f5d4b62746590db30b295369c9daeb.png)

填写应用名称和描述，还有头像也可以自己修改。

![](img/40477701f2cd40463399b9d679b4d6c4.png)

然后点击左边的添加应用，添加机器人。

![](img/1e42670cb79ad9c025033f5f7b647c19.png)

# 第二步，打开Aircode，点击创建应用，上面输入名字，下面选择Node.js v16.

![](img/9e5ea6a856f1cdeddd4955dbaafe75c5.png)

创建后页面会有一些代码，我们全选，然后把他删除掉，然后打开下面的源码地址，进去复制代码。

源码地址：[https://github.com/be](https://github.com/bestony/ChatGPT-Feishu)stony/ChatGPT-Feishu

![](img/54f787f6e3582dfad605c8c2006864a6.png)

![](img/9c5be1ec9de5f64096f8dca6182ab87e.png)

复制代码之后粘贴到Aircode，然后点击Deploy。输入一些日志变更。

![](img/084321d614cb001a0acb77c9e231be2d.png)

![](img/4a313c5912e2616558f172160ff6eef6.png)

等几秒钟布署，下面出现这行代码，或者右侧这里也出现描述则表示完成布署。

![](img/c689a51fb1897da536a5bd68b59f14f5.png)

然后在左下角这里搜索 axios和 @larksuiteoapi/node-sdk   然后进行安装，等一会安装完成后再点击Deploy进行布署。

![](img/e429d7574a3d9999c835f38ad2639d61.png)

![](img/62a2dbf8fa669e4823e7100d06047000.png)

![](img/191c058a1231275014c68fab66eda92b.png)

#  第三步，配置环境，点击Environments，创建四个变量，全部要大写。

APPID 、SECRET 、 BOTNAME、KEY

![](img/52854691986efe3c58613190cc5d293c.png)

KEY，是 OpenAI 的 KEY

[打开这个链接，然后登陆你ChatGPT账号  h](https://platform.openai.com/account/api-keys)ttps://platform.openai.com/account/api-keys

登陆后点击这里创建，然后复制回去Aircode粘贴。

![](img/3fa03244d75e37e5f50aac60d3914126.png)

![](img/93114c2cad4584dda07a1fae4c27ced3.png)

![](img/01bdf97d9152f36a9bca01dd331caa13.png)

全部粘贴后，点击Deploy进行布署。

![](img/2c3b1b3c9746bab0044e8e1c268fba6c.png)

布署完成之后，点击复制这个网站，然后打开，看看是否配置成功。

![](img/ec6b47d7227f871e59070e1a870cd8e1.png)

成功之后，打开飞书，配置一下权限，以下6个都要开，有些人账号不一样，某些权限要审核。

im:message

im:message.group_at_msg

im:message.group_at_msg:readonly

im:message.p2p_msg

im:message.p2p_msg:readonly

im:message:send_as_bot

![](img/61d74181bc3fc65115915efcb2f1c9ca.png)

![](img/3cd4b82f836e4a1bfe22b14a3f9df679.png)

开启之后，再返回AirCode ，复制函数的调用地址，然后再返回飞书，点击事件订阅，把网址粘贴到配置地址。

![](img/15e7277b838fa5c91b5bb52c088f1b38.png)

![](img/c73f87254bc8b6ed899a046f1feb4f38.png)

然后点击添加事件，找到 消息与群组，选择接收消息v2.0，添加即可。

![](img/bf43c342bb279e6df49d34b40b689231.png)

然后打开版本管理与发布，创建版本，输入版本号，还有输入刚刚Aircode的调试地址，点击保存，然后点击右上角申请发布即可。我这里已经申请了，所以变成了撤回。

![](img/ab2fe857fcbbae4d09d8ff5dfbea6e25.png)

![](img/607f804932b8d2cd71456f6f79829921.png)

然后你的飞书就会收到消息，点击进入管理后台审核，通过就可以了。

![](img/59f290012b12f4f025982dd8f05456e1.png)

审核用过后，打开应用，你就可以使用你的ChatGPT机器人了。

![](img/4c27668d4a0ef482b0a58918f83b1304.png)

![](img/817b38286cbccf7d1a0d2263b45acd11.png)