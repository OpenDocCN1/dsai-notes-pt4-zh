
# 老金·邪修法则：CC每天自动获取25$额度Claude多账号，AnyRouter全自动签到

> 来源：[https://my.feishu.cn/docx/Xz0Sdc7pPoa2OmxtXwBcQqvvnZd](https://my.feishu.cn/docx/Xz0Sdc7pPoa2OmxtXwBcQqvvnZd)

## 项目简介

老金我又发现了个邪修法则，这次的项目每天可以自动签到。

`AnyRouter`这个赛博菩萨，每天签到会给25$，申请就获得100$的Claude额度。

先在这里申请账号：

https://anyrouter.top/register?aff=0FzF

> 每邀请一位好友注册，您和好友都可获得额外 $50 Claude Code 额度的奖励

> 通过划转功能将奖励额度转入到您的账户余额

目前Github不支持新账号注册了，只能用LinuxDO或者邮箱，邮箱仅支持edu.cn结尾的，可以在某鱼、某宝购买到。

![](img/ee4293e14d133f06506f1d8b2f7be567.png)

## 效果预览

先看效果，全自动领取：

![](img/adb81f3dd41faa2e082ed531694e6933.png)

## 操作步骤

首先需要`Fork`项目：https://github.com/millylee/anyrouter-check-in

然后登录`AnyRouter`网站。

点开`F12`，查看以下信息，复制并保存到其他地方，后续会用到。

`Application -> Cookies -> Session` 找到最下面对应的值。

![](img/932098619273bd76fd4fbd1711fa4697.png)

`Network -> Status -> New-Api-User` ，复制后面的ID，通常这个ID是5位数。

![](img/8a57e548f9eee05f7d22bff7411625a5.png)

在Github上打开你`Fork`的项目，也就是自己主页里的项目，千万别搞错了。

点击 `Settings -> Environments -> New environment`

![](img/f53da1f5bf9b34e46fb595f69e78c22d.png)

随便起个名字，如`Production`。

![](img/3c0a6caa78479f91d91361ad7e5c0c9a.png)

右侧框下拉，找到`Add environment secret`

![](img/36021982175a5a8a2f61680adc808f14.png)

随便输入个名字，如`ANYROUTER_ACCOUNTS`

`Value`里，输入上面保存的两项。

`session`对应就是上面保存的`Session`。

`api_user`对应的是上面的`New-Api-User`，5位数那个。

![](img/be5f51fdf1758c2caf98662429e932d3.png)

支持多账号，如下。

如果是单账号，把重复的删掉就行，小白们注意删除时记得把中括号后面的逗号也要删掉，或者你直接删除上面的重复的部分也可以。

```
[
  {
    "cookies": {
      "session": "abc123session"
    },
    "api_user": "user123"
  },
  {
    "cookies": {
      "session": "xyz789session"
    },
    "api_user": "user456"
  }
]
```

填写后，在Github上面找到`Actions`，然后你能看到`AnyRouter自动签到`，点击后选择`Enable Workflow`。

![](img/c605a26acbd0f38b6feb4fd882cfa75d.png)

选择`Run Workflow`。

![](img/2d372b3623d47d35a652b8e891cefaeb.png)

几秒之后，会出现第一条，这样就成功了。

它会每间隔6小时，自动登录一次。

![](img/871dd86a391218d8374b1ce9ca4772b3.png)

## 注意事项

`Session` 和 `New-Api-User` 有效期约为1个月，因此在设置前最好先重新登录一次。

如果更换账号，AnyRouter里需要清除一下Cookie。

至于怎么使用，你可以参考老金的这篇教学文章：

[Claude Code一键安装及白嫖方法](https://tffyvtlai4.feishu.cn/wiki/Ly89wbu3giQvuckMneFckyW9nUd)

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
