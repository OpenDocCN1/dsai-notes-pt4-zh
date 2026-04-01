# ChatGPT与Discord无缝接入指南（保姆级教程）

> 来源：[https://xmgtoagi.feishu.cn/docx/KFdCdkiBHoEmvFx2f7kcwNAbnpg](https://xmgtoagi.feishu.cn/docx/KFdCdkiBHoEmvFx2f7kcwNAbnpg)

ChatGPT和Discord都非常流行，而在Discord内使用AI绘画也很受欢迎。如何将ChatGPT与Discord对接，在Discord内实现一个聊天机器人。

## 第一步：获取OpenAI API密钥。请按照以下步骤操作：

1.  打开此链接 [https://platform.openai.](https://platform.openai.com/account/api-keys)com/account/api-keys

1.  请按照页面上的提示进行身份验证，然后登录您的ChatGPT账号。

1.  点击“这里创建”以创建一个新的API密钥。

1.  复制生成的API密钥以备将来使用。

![](img/bf61c198a20dacfacf1f9c6e3c335501.png)

![](img/96383af99091e8dc8f31bcd2874b4c9c.png)

## 第二步：获取Discord Token。请按照以下步骤操作：

1.  打开网站[https://discord.com/dev](https://discord.com/developers/applications)elopers/applications

1.  登录您的Discord账号。

![](img/aa69694044103754c4a8cf8f46d631a1.png)

1.  点击右上角的“New Application”按钮创建一个新的应用程序。

![](img/92d50479e95d187eaf665000139da582.png)

1.  在新页面上，输入您的应用程序名称，并点击“Create”按钮。

![](img/ddfb887892302096b486b45d83db13d4.png)

1.  选择“Bot”选项卡，并点击“Add Bot”按钮，然后点击“Yes, do it!”以确认添加机器人。

![](img/c4cc2ca8cdaf6d57f7b91fea90e4fff8.png)

![](img/4e7854099c8a3dc19120ee246cfa00e3.png)

创建完机器人之后，可以编辑一些资料，比如头像、名字，还有各种权限。

![](img/59061dbc0fe897faa4474a57c89b725a.png)

T**oken 这里可能**是查看令牌 View Token 或者是重置令牌 Reset Token 的按鈕，点击一些就，复制令牌出来备用。

![](img/1a78ad19cfe9295020ff5d3a401a9f4d.png)

1.  往下拉，找到MESSAGE CONTENT INTENT，打开，然后点击Save Change。

![](img/d39ba3445643195cbb8dcfcd579fc7e9.png)

1.  然后设置OAuth2。按照下图步骤设置。

![](img/38da8ad05a3cc57902eab0ca5e6ad18d.png)

1.  然后点击复制链接，浏览器打开。

![](img/5bf80d78bebd4ed84ef67ec0e401680f.png)

1.  然后选择要加入的服务器。

![](img/ce5d2d68081224ea7dc177c11e4cbd63.png)

请注意，您的Discord Token是私密信息，请勿与他人分享。

## 第三步：注册GitHub，有账号的可以直接登录。

无账号注册，有账号跳过以下步骤，直接登录即可：

1.  打开网站https://github.com/

1.  如果您已经拥有GitHub账号，请在右上角登录。否则，请单击右上角的“Sign up”按钮创建一个新账号。

1.  在新页面上，输入您的用户名、电子邮件地址和密码，并完成验证过程。

1.  点击“Create account”按钮以创建您的GitHub账号。

**请注意，您的GitHub账号可以让您**与其他开发者协作，并分享您的代码和项目。

注册/登录，然后请按照以下步骤操作：

1.  打开网站[h](https://github.com/TheExplainthis/ChatGPT-Discord-Bot)ttps://github.com/TheExplainthis/ChatGPT-Discord-Bot。

1.  点击页面右上角的“Fork”按钮，以将该项目复制到您的GitHub账号下的存储库中。

**请**注意，在Forking存储库后，您将在自己的账号下创建一个新的存储库副本，您可以在其中进行更改，而不会影响原始存储库。

![](img/bdf207c38337dd1c492047b74c9c2939.png)

![](img/2e98f1522099c864bfdc9a075596df53.png)

## **第四步：****在线开发环境搭建Replit：****请按照以下步骤操作**

**请**注意，Replit是一个在线开发环境，它可以让您在浏览器中编写、运行和分享代码，而无需安装本地开发环境。

1.  打开网站https://replit.com/

1.  点击页面右上角的“Sign Up”按钮，用github直接授权登录即可。

![](img/d34443d96055c38726cbd85f54a1f02a.png)

![](img/95e0e5b7a0989f1b3ac4ba484952b561.png)

![](img/6c81bf0502eb8fa8142037dc664c300b.png)

1.  登录之后，选择一下基本的东西。

![](img/c76bc1a926b2dc28f93a4193a6b11678.png)

![](img/6fe2c21143690103b6c310e710762c86.png)

![](img/902a50f68b6e875b027d4ad3f71e4398.png)

1.  进入首页之后点击创建Repi

![](img/a3968dd5f1b405e9a62f9278500be502.png)

1.  跳出选择框之后，选择右上角这个Import from Github。

![](img/1117248830be0f0e50feddfaaf3be6e9.png)

1.  然后再点击这个Connect GitHub to import your private repos，

![](img/75f2fd7f3846b2ea54a8818813b5ca71.png)

1.  弹出这个页面，根据下图一样，选择好了就安装。

![](img/242875783f4025bdacbe9c76671a85ae.png)

1.  然后返回这个页面，可以选择刚刚安装的代码了。

![](img/5b1c6cb25940b60629ce3eaaee44b598.png)

![](img/8fbb752b8ed148b63cdde4d0c366be4d.png)

1.  安装需要一段时间，安装完成之后跳过教程。

![](img/d51b718412ff1a8eb6d0ed570dc47ad0.png)

1.  跳过了，点击右侧的tools，然后在里面找到Secrets，再点击右边的Got it。

![](img/8a4ddd8160f067cb6ca61f847c13e75d.png)

1.  然后会弹出一个框，key：OPENAI_API，  value填写你的 openai  key，sk开头的

![](img/e1aeade97d9abc573d3c1af6fb4d5bcf.png)

1.  然后继续添加，key:OPENAI_MODEL_ENGINE      value:gpt-3.5-turbo

![](img/07e43ed8f08415b799f3d674b0212f0f.png)

1.  继续添加，key:SYSTEM_MESSAGE      value:You are a helpful assistant.

![](img/1b92d35cfc1d89b73c904217bd8e7b61.png)

1.  再添加最后一个。key:DISCORD_TOKEN      value:  前面Discord的令牌 MTA开头的。

![](img/7884dfb8fd81475ea74ae13610e89c5a.png)

1.  添加完四个之后，点击上面的Run 运行。

![](img/5968d8d0623c166d29ff50bff2cd9b89.png)

1.  运行等一会，成功之后右边会出现有Hel**lo. I am alive!** 的页面，然后我们复制上面的地址出来备用。有效时间一小时，后面的步骤需要一小时内完成。

![](img/600d3aba3e8900a06c592e5616dd0729.png)

## 第五步：注册cron-job.org。请按照以下步骤操作：

**请注意，****cron-job.org**是一个在线计划任务管理工具，它可以让您定期运行代码和脚本。

1.  注册如下，如有账号可跳过：

打开网站https://cron-job.org/en/

![](img/127df714898480840a3c7bf90c604f84.png)

如果您已经拥有账号，请在页面右上角登录。否则，请单击页面右上角的“Sign Up”按钮创建一个新账号。

![](img/9e34742b0f45c7e6de0aeccfd7a4d5cc.png)

在新页面上，输入您的用户名、电子邮件地址和密码，并完成验证过程。

点击“Create Account”按钮以创建您的cron-job.org账号。

1.  登录之后，点击右边的CREATE CRONJOB。

![](img/a8cb7ad83e310fb92bb8c22b25084304.png)

1.  Title输入ChatGPT-Discord-Bot，网址输入上一步的网址，选择五分钟，然后点击CREATE 创建。

注意：输入的网址不要含有s,否则定时任务会运行失败。

![](img/250c8e34f65406f79a520bd050cca84c.png)

![](img/da740a0359cbe8a67ed693917aefd6df.png)

以上步骤完成搭建，然后打开Discord，你就能看到ChatGPT机器人在线了。

![](img/7d06415b812a1693d71963a31d80fba8.png)

/chat        在输入框直接输入会后/chat直接message输入文字，机器人就会回复了。

/reset        ChatGPT 会记住前十次的提问记录，调用此指令则会清除。

/imagine        在输入框输入/imagine会prompt后直接输入文字，可以生成图片。