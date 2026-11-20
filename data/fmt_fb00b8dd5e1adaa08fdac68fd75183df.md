
```
def on_handle_context(self, e_context: EventContext):
    if e_context['context'].type != ContextType.TEXT:
        return
    content = e_context['context'].content
    if content == "Hello":
        reply = Reply()
        reply.type = ReplyType.TEXT
        msg:ChatMessage = e_context['context']['msg']
        if e_context['context']['isgroup']:
            reply.content = f"Hello, {msg.actual_user_nickname} from {msg.from_user_nickname}"
        else:
            reply.content = f"Hello, {msg.from_user_nickname}"
        e_context['reply'] = reply
        e_context.action = EventAction.BREAK_PASS # 事件结束，并跳过处理context的默认逻辑
    if content == "End":
        # 如果是文本消息"End"，将请求转换成"IMAGE_CREATE"，并将content设置为"The World"
        e_context['context'].type = ContextType.IMAGE_CREATE
        content = "The World"
        e_context.action = EventAction.CONTINUE  # 事件继续，交付给下个插件或默认逻辑
```

## 插件设计建议

+   尽情将你想要的个性化功能设计为插件。
+   一个插件目录建议只注册一个插件类。建议使用单独的仓库维护插件，便于更新。
+   在测试调试好后提交PR，把自己的仓库加入到[仓库源](https://github.com/zhayujie/chatgpt-on-wechat/blob/master/plugins/source.json)中。
+   插件的`config`文件、使用说明`README.md`、`requirement.txt`等放置在插件目录中。
+   默认优先级不要超过管理员插件`Godcmd`的优先级(999)，`Godcmd`插件提供了配置管理、插件管理等功能。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
