# 消息分段发送

> 来源：[https://my.feishu.cn/docx/VKFud41jCoBlanx7wqJcrDuznPd](https://my.feishu.cn/docx/VKFud41jCoBlanx7wqJcrDuznPd)

##** ****//n** 符号是自己设定的，只要**代码**和**提示词**对应上，即可实现分段

## 效果展示

![](img/c9560cac81a163128ec97b1af88f435f.png)

## 逻辑说明

1.  修改cow的send文本代码，其中//n是在提示词中添加分段标识。

1.  LLM中的提示词中增加对应的分段标识符，那么LLM的回复会变成这样

你好!//n我是您的人工智能助理~

那么经过COW处理后，会变成2个消息发送。

```
      你好！
      我是您的人工智能助理~
```

## 代码修改

找到该路径下的【wechat_channel.py】文件

![](img/4d1cbde48e7eeec8717f5145869aefa6.png)

找到如下区域，Ctrl+F搜索关键字Replytype.Text，红框内源代码不是这样的，要修改成这样。

![](img/8152b98ad1851b3e149d00d7df10e5e7.png)

代码内容如下，直接拷贝进去：

```
        if reply.type == ReplyType.TEXT:
            # 使用正则表达式来分割消息
            split_punctuation = [&#x27;//n&#x27;]
            pattern = &#x27;|&#x27;.join(map(lambda x: re.escape(x), split_punctuation))
            split_messages = re.split(pattern, reply.content)
            # 移除空行
            split_messages = [msg.strip() for msg in split_messages if msg.strip() != &#x27;&#x27;]
            for msg in split_messages:
                itchat.send(msg, toUserName=receiver)
                logger.info(&quot;[WX] sendMsg={}, receiver={}&quot;.format(msg, receiver))
```

## 提示词修改参考

LLM中设定 - COW或LLMOps中都行，看你把提示词填在哪里了，就在那加就好）：

```
## 人设
- //n 表示段落，我总是使用段落叙述进行回应。

## 案例
- Somebody：你为什么有时候温柔有时候冷傲呢？
You： 这问题问的//n老金就这样，多面体，时而温柔时而冷傲，这不挺有意思的嘛[旺柴]
```

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>