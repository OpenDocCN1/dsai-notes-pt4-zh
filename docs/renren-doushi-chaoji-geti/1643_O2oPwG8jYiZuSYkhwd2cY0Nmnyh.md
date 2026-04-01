# 2023-07-15一个人，一天，打造出一个包含ChatGPT+MJ绘画的成熟AI产品！

> 来源：[https://xuqiwei1986.feishu.cn/docx/D0SfdzzyLoQj94xUzRecFlO3nSf](https://xuqiwei1986.feishu.cn/docx/D0SfdzzyLoQj94xUzRecFlO3nSf)

> 🔗 原文链接： h[ttps://articles.zs](https://articles.zsxq.com/id_i1cd1puxho11.html)xq.com/id_i1cd1p...

>

# 前言

在使用zelinai和MewXAI小程序的时候，就在想这些应用是如何在不需要特殊环境的情况下，实现Midjourney绘画功能的？应该是走的服务器代理之类的，于是就去github上找一找有没有相应的内容，结果还真给自己找着了。在github上搜midjourney可以看到两个很火的项目，star量都很高。

![](img/890480f3cec2dd37cb42547876fc5f0f.png)

其[中midjourne](http://readme.md/)y-proxy是后端服务，ChatGPT-MidJourney是前端服务，相应的部署文档在项目 README.md 文件里也都有写，其实就是弄一个web项目，最终的成果可以参考下面这张图片。可以看到在聊天框不仅能和GPT对话，而且还可以发送mj绘画命令出图，重点是国内也可以正常使用！完完全全就是一个成熟的AI产品，而且功能强大！

![](img/59773300820192c3e009f36c818118b1.png)

看到这效果自己也是眼前一亮，不过在实操体验之后，感觉部署文档还是有些东西没写清楚，踩了一些坑，这篇文章会详细介绍自己的部署过程。就算不是程序员，看不懂代码，也可以轻松部署！

# 前提条件

1. 准备好网络环境

2. github账号，码农必备

3. openAI账号，需要用到API key

4. Midjounery账号，某宝的共享账号也ok

# 部署过程

## 1. fork项目

登录 https://github.com 官网，没有账号的注册一个，找到我们要的项目，fork(克隆)到自己的仓库，midjourney-proxy和ChatGPT-MidJourney两个项目都要

![](img/79923ada7186e3ea0b4e01ecb7d33b15.png)

fork完成之后可以在自己的仓库看到这两个项目

![](img/7a5ec8e5e41d3680b31af07548d085ea.png)

## 2. OpenAI API key

登录 https://openai.com 官网，点击右上角的账号图标，选择弹窗中的view API keys，跳转到api-keys页面，生成自己的key

![](img/13fc34bda4d1ff50b3e43872990a785f.png)

## 3. MJ账号关联信息

在[discord登录自己的mj账号，需要 获取用户Token、服务器ID、频道](https://github.com/novicezk/midjourney-proxy/blob/main/docs/discord-params.md)ID，参考github上文档给的链接 https://github.com/novicezk/midjourney-proxy/blob/main/docs/discord-params.md ，这里面已经写的很清楚了。另外有一个风险须知，在部署服务时需要合理设置自己的mj账号参数配置

![](img/f14373d01504931d22464b72ab8e82ac.png)

所有mj参数配置项说明地址： [https://github.com/novicezk/midjourne](https://github.com/novicezk/midjourney-proxy/wiki/%E9%85%8D%E7%BD%AE%E9%A1%B9)y-proxy/wiki/%E9%85%8D%E7%BD%AE%E9%A1%B9

这里从简单入手，就用3个配置项就行。登录账号后进入自己的频道，从网址显示的url(url格式： discord.com/channels/ 服务器ID/频道ID)，取出服务器ID和频道ID

![](img/80b48b71f3a58d832bf52fa36caf0b98.png)

打开浏览器开发者工具，快捷键F12，获取用户token

![](img/a7b04587cd8e5357c38a17c138d4256c.png)

得到的三个配置项如下，注意书写格式，方便后续复制粘贴，其中mj_discord_guild_id是服务器ID，mj_discord_channel_id是用户频道ID，mj_discord_user_token是用户token

mj_discord_guild_id=112******992

mj_discord_channel_id=112******995

mj_discord_user_token=MTAyM******1YwaUs

## 4. 服务部署

github上介绍了好几种部署方式，这里以zeabur平台部署为例

### 1.部署midjourney-proxy后端服务

1. 打开网址 https://zeabur.com/zh-CN , 点击价格，有3种租用服务器收费方案，先选择免费的即可，白嫖7天使用期限

![](img/23423e2de93b4f9826074e4c5b5cbfe5.png)

2. 登录账号，选择github账号登录

![](img/0594899d8c6e42a70b827cef44871e8e.png)

3. 创建项目，点击create Project 并输入一个项目名称，如qnMJ

![](img/1ceb217038e9e78af202e03d40e2c4ea.png)

4. 点击 + 添加服务，会跳转新页面并弹窗，选择 Git-Deploy service from source code in GitHub repository

![](img/4ff27ae0a922d11d6dd63fc2591ffaf8.png)

![](img/a5275ff5fc0d7061b34fe0b5885906cf.png)

5. 点击configure github，然后选择select repositores，选上自己fork的两个项目

![](img/609f29a5344d8ab4e0c0072739478a85.png)

![](img/2bc78ea65d1b3233bc2fb52301abf272.png)

6. 配置完成回到搜索框，可以看到配置的两个项目，点击midjournery-proxy

![](img/35a511a1ec2973c277adcf0cd5cf2ee2.png)

然后services列表里就会有我们添加的服务，并且服务正在构建中(building)

![](img/e57e31793f260a8e1dec5a0c8d431df3.png)

7. 设置环境变量，打开Variable，点击编辑

![](img/0c86a2a1be8c22d08c4cd922cac63907.png)

将之前保存的mj配置项复制过来，另外设置服务端口号8080，点击保存

![](img/b29d9b472b6e34d5dc497346c9de82f1.png)

配置完成可以看到配置列表成功展示

![](img/79178d2a2dc9193d655f88e91918ffe0.png)

8. 配置域名，打开Domains，点击Generate，输入可有效使用的域名，如midjourney-proxy-wwqn，保存

![](img/6881564e8710e7150c31a3f1481abefb.png)

9. 重启服务，配置完参数和域名，之前自动building的服务应该已经部署好了，需要重启配置才生效

![](img/f8f92681d46972cf3584e4db9c2efcae.png)

[10. 部署服务大概需要几分钟，耐心等待部署成功](https://midjourney-proxy-demo.zeabur.app/mj)，访问 https://midjourney-proxy-wwqn.zeabur.app/mj , 看到我们码农熟悉的swagger界面就说明midjourney-proxy后端服务部署成功了

![](img/27958540a4b9074b9f1457a3d5bfbd1b.png)

常用的两个接口：提交imagine任务、指定ID获取任务，但是获取的图片是一个discord服务的url，无法正常下载，所以就需要部署前端项目了

![](img/be35b82f6ad96696fb7938f0e40efb01.png)

### 2.部署ChatGPT-midjourney前端服务

1. 依据之前部署后端服务步骤中的步骤6，添加chat-gpt-midjourney服务，最终我们的services列表中就会有一前一后两个服务

![](img/c0f7799d2a441764891f67db61bd9775.png)

2. 设置前端服务环境变量

OPENAI_API_KEY : openAI的API key

CODE : 前端页面访问密码

BASE_URL : openai官网

MIDJOURNEY_PROXY_URL : 前端需要访问的mj服务器，设置成我们刚才配置的mj服务域名就好

PORT : 前端服务端口号

![](img/20d91ba92705c29a7da80322536d9a69.png)

3. 同样设置域名，例如 chatgpt-midjourney-wwqn.zeabur.app

4. 重启服务，前端部署会久一些，等待部署完成，访问域名，成功打开UI界面，大功告成？还没有...

![](img/1e84e81c8c363c7902b74656afd51c3d.png)

## 5. 运用实践

1. 设置访问密码，因为在配置前端服务时设置了“CODE=88888888”, 所以需要添加该密码才能正常使用功能，打开界面左下角的设置按钮

![](img/4f6709dee813a3930c168bdfed738679.png)

2. 设置成功后就可以愉快的聊天了

![](img/dd79d9965db008b79c5262e72474506f.png)

3. 尝试mj绘画功能，输入命令，discord上mj同样收到了消息

![](img/c5828bf35d307b0f5942dcf8123db78e.png)

显示结果如下，点击图片，根据图片url可以看出，图片其实已经转到了我们部署的服务器上，可以随意下载，完工！

![](img/ce2ad3b4f2b556f3a778fd51e92c24ef.png)

# 其他事项

1. chatgpt-midjouery项目还有不少亮点，如面具

![](img/74756d5656e33b06eff9b89d5b08c41c.png)

2. 设置里还可以选择其他模型

![](img/442365a8f9b51fed89992aedbeaac4bd.png)

3. /mj绘画命令需要调用midjounery-proxy后端服务接口，如果mj是relax模型，等待时间过久就有可能超时，返回失败，但其实discord上面是成功的，建议mj是设置成fast模式

4. 搭建的服务域名可以区分不同用户设备，也就相当于一个账号可以多人使用

5. 至于其他功能，期待其他大佬探索开发。另外chatgpt-midjourney开发者提到的pro版本功能更加强大，不过没还得研究。感谢大佬搭建的github项目！