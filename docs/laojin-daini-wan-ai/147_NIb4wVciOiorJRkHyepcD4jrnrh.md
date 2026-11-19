
# Discord账号多配置方法

> 来源：[https://my.feishu.cn/docx/TIIadhNfAomgsUxz9kqcg4HKnNf](https://my.feishu.cn/docx/TIIadhNfAomgsUxz9kqcg4HKnNf)
> 教程来自 空心菜、王长东（阿飞）

## 配置目的和原理

Coze对Discord单账号使用次数有限制，每个账号只能每天回复50-100轮对话，传到🛰️里也会被同样限制，肯定是不够用的。根据官网说明如下：

目前的coze对每个Discord用户做了标识,在Discord中每个用户对不同的模型的调用有次数限制,具体如下:

```
GPT-4 Turbo (128k) - 50次/天
GPT-4 (8k) - 100次/天
GPT-4o (128k) - 100次/天
GPT-3.5 (16k) - 500次/天
```

可配置多Discord用户`Authorization`(参考[环境变量](https://github.com/deanxv/coze-discord-proxy#%E7%8E%AF%E5%A2%83%E5%8F%98%E5%8F%98%E9%87%8F)`USER_AUTHORIZATION`)实现叠加请求次数及请求负载均衡。

## 配置方法

1. 在Discord中已经配置好的cozebot服务器上右键选“邀请其他人”，得到一个邀请链接，点击复制，链接保存到一边备用。

![](img/a6892c0479830d0279c7f7d390401254.png)

![](img/e551b1052b6d2b3c44e0ac6c60758d93.png)

2. 注册新谷歌账号，或者掏出多余的谷歌账号，此步骤省略。

3. 打开另一个未登录过Discord的浏览器或者谷歌无痕浏览器（这步要特别注意，后边解释），用步骤2的谷歌账号注册一个新Discord账号

4. 复制步骤1的邀请链接到注册账号的新浏览器中，此时就将新Discord账号拉入了cozebot服务器，如果需要多个就多次重复以上步骤。但请注意不要解除前边账号的登陆状态，所以特别推荐谷歌无痕浏览器。

5. 按F12,新账号在对话框中发言，复制新账号的`USER_AUTHORIZATION`值，多个账号就挨个发言+复制`USER_AUTHORIZATION`值。

6. 打开宝塔，在`/data/coze-Discord-proxy/docker-compose.yml`文档中，将多个`USER_AUTHORIZATION`值粘贴进来，以逗号分开。然后`docker-compose up -d`重启cdp即可。

![](img/c90e36fbc86440aa80003f5e4123b582.png)

7. 重新扫码后你的coze对话轮次就多了n倍啦。

8. 如何验证自己成功了？最简单的办法是手机下载Discord，登录后打开通知。每当有对话产生，你的手机都会收到消息，当每条消息发送账号都不同，就说明你已经成功了。当然每轮对话都会在Discord以临时聊天框的形式存在一小会儿，那里也能看到不同账号的参与。

![](img/ba5b05d87c21081f6a847387542c25df.png)

## Q&A

### 为啥要保证之前账号的登陆状态？

因为账号登出后`USER_AUTHORIZATION`值就变了，需要重新取用，很多人发现突然登不上去了，就是因为登出状态后这个值变化了。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
