# 延时回复消息

> 来源：[https://my.feishu.cn/docx/TYkLdicE6o8xmsxMIQ6cRXIJnic](https://my.feishu.cn/docx/TYkLdicE6o8xmsxMIQ6cRXIJnic)

找到该路径下的【wechat_channel.py】文件

![](img/813feb2e17de08e53d71e118e60890ec.png)

**代码修改**

最上面增加import random

![](img/c55e7d053da996568f91abb4a3421d21.png)

然后找到这个位置

![](img/67c3cc0282edb4472e8f1cf7f51d8bc5.png)

把这段替换进去，random.uniform(2, 4)中的(2, 4)，代表在2-4秒间随机，时长仅供参考，可自行控制。该段不是必须的，如果不需要的话，就不用填下面#等待随机时间，及之后的代码

```
    # 统一的发送函数，每个Channel自行实现，根据reply的type字段发送不同类型的消息
    def send(self, reply: Reply, context: Context):
        receiver = context[&quot;receiver&quot;]
        if reply.type == ReplyType.TEXT:
            itchat.send(reply.content, toUserName=receiver)
            logger.info(&quot;[WX] sendMsg={}, receiver={}&quot;.format(reply, receiver))
            
            # 等待随机时间
            r_time = random.uniform(2, 4)
            time.sleep(r_time)
```

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>