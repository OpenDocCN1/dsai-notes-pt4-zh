# COW插件说明

> 来源：[https://my.feishu.cn/docx/ZIkvdbTvnopZjexCCNbcs6cynwh](https://my.feishu.cn/docx/ZIkvdbTvnopZjexCCNbcs6cynwh)

## **插件化初衷**

之前未插件化的代码耦合程度高，如果要定制一些个性化功能（如流量控制、接入NovelAI画图平台等），需要了解代码主体，避免影响到其他的功能。多个功能同时存在时，无法调整功能的优先级顺序，功能配置项也非常混乱。

此时插件化应声而出。

**插件化**: 在保证主体功能是ChatGPT的前提下，我们推荐将主体功能外的功能利用插件的方式实现。

+    可根据功能需要，下载不同插件。

+    插件开发成本低，仅需了解插件触发事件，并按照插件定义接口编写插件。

+    插件化能够自由开关和调整优先级。

+    每个插件可在插件文件夹内维护独立的配置文件，方便代码的测试和调试，可以在独立的仓库开发插件。

## **插件安装方法**

在本仓库中预置了一些插件，如果要安装其他仓库的插件，有两种方法。

+   第一种方法是在将下载的插件文件都解压到"plugins"文件夹的一个单独的文件夹，最终插件的代码都位于"plugins/PLUGIN_NAME/*"中。启动程序后，如果插件的目录结构正确，插件会自动被扫描加载。除此以外，注意你还需要安装文件夹中requirements.txt中的依赖。

+   第二种方法是Godcmd插件，它是预置的管理员插件，能够让程序在运行时就能安装插件，它能够自动安装依赖。

+   [安装插](https://github.com/zhayujie/chatgpt-on-wechat/blob/master/plugins/source.json)[件的](https://github.com/zhayujie/chatgpt-on-wechat/tree/master/plugins/godcmd)命令是"#installp 仓库源记录的插件名/仓库地址"。这是管理员命令，认证方法在这里。

+   安装[仓库源](https://github.com/zhayujie/chatgpt-on-wechat/blob/master/plugins/source.json)记录的插件：#installp sdwebui

+   [安装指定仓库的插件：#installp https](https://github.com/lanvent/plugin_sdwebui.git)://github.com/lanvent/plugin_sdwebui.git

+   在安装之后，需要执行"#scanp"命令来扫描加载新安装的插件（或者重新启动程序）。

安装插件后需要注意有些插件有自己的配置模板，一般要去掉".template"新建一个配置文件。

## **插件化实现**

插件化实现是在收到消息到发送回复的各个步骤之间插入触发事件实现的。

### **消息处理过程**

在了解插件触发事件前，首先需要了解程序收到消息到发送回复的整个过程。

插件化版本中，消息处理过程可以分为4个步骤：

```
    1.收到消息 ---&gt; 2.产生回复 ---&gt; 3.包装回复 ---&gt; 4.发送回复
```

[以下是它们的](https://tffyvtlai4.feishu.cn/wiki/VdgmwA3oBi153HkeR3IcoIChnXg?fromScene=spaceOverview#Avlhd9YGJoj4A6xmLGBc8Faanld)默认处理逻辑(太长不看，可跳到插件编写示例)：

**注意以下包含的代码是****v1.1.0**[**中的片段，已过时，只可用**](https://github.com/zhayujie/chatgpt-on-wechat/blob/master/channel/chat_channel.py)于理解事件，最新的默认代码逻辑请参考chat_channel

1.  **收到消息**

负责接收用户消息，根据用户的配置，判断本条消息是否触发机器人。如果触发，则会判断该消息的类型（声音、文本、画图命令等），将消息包装成如下的Context交付给下一个步骤。

```
    class ContextType (Enum):
        TEXT = 1         # 文本消息
        VOICE = 2        # 音频消息
        IMAGE_CREATE = 3 # 创建图片命令
    class Context:
        def __init__(self, type : ContextType = None , content = None,  kwargs = dict()):
            self.type = type
            self.content = content
            self.kwargs = kwargs
        def __getitem__(self, key):
            return self.kwargs[key]
```

Context中除了存放消息类型和内容外,还存放了一些与会话相关的参数。

例如，当收到用户私聊消息时，会存放以下的会话参数。

context.kwargs = {'isgroup': False, 'msg': msg, 'receiver': other_user_id, 'session_id': other_user_id}

+   isgroup: Context是否是群聊消息。

+   msg: itchat中原始的消息对象。

+   receiver: 需要回复消息的对象ID。

+   session_id: 会话ID(一般是发送触发bot消息的用户ID，如果在群聊中并且conf里设置了group_chat_in_one_session，那么此处便是群聊ID)

1.  **产生回复**

处理消息并产生回复。目前默认处理逻辑是根据Context的类型交付给对应的bot，并产生回复Reply。 如果本步骤没有产生任何回复，那么会跳过之后的所有步骤。

```
    if context.type == ContextType.TEXT or context.type == ContextType.IMAGE_CREATE:
        reply = super().build_reply_content(context.content, context) #文字跟画图交付给chatgpt
    elif context.type == ContextType.VOICE: # 声音先进行语音转文字后，修改Context类型为文字后，再交付给chatgpt
        cmsg = context[&#x27;msg&#x27;]
        cmsg.prepare()
        file_name = context.content
        reply = super().build_voice_to_text(file_name)
        if reply.type != ReplyType.ERROR and reply.type != ReplyType.INFO:
            context.content = reply.content # 语音转文字后，将文字内容作为新的context
            context.type = ContextType.TEXT
            reply = super().build_reply_content(context.content, context)
            if reply.type == ReplyType.TEXT:
                if conf().get(&#x27;voice_reply_voice&#x27;):
                    reply = super().build_text_to_voice(reply.content)
```

回复Reply的定义如下所示，它允许Bot可以回复多类不同的消息。同时也加入了INFO和ERROR消息类型区分系统提示和系统错误。

```
    class ReplyType(Enum):
        TEXT = 1        # 文本
        VOICE = 2       # 音频文件
        IMAGE = 3       # 图片文件
        IMAGE_URL = 4   # 图片URL

        INFO = 9
        ERROR = 10
    class Reply:
        def __init__(self, type : ReplyType = None , content = None):
            self.type = type
            self.content = content
```

1.  **装饰回复**

根据Context和回复Reply的类型，对回复的内容进行装饰。目前的装饰有以下两种:

+   TEXT文本回复:如果这次消息需要的回复是VOICE，进行文字转语音回复之后再次装饰。 否则根据是否在群聊中来决定是艾特接收方还是添加回复的前缀。

+   INFO或ERROR类型，会在消息前添加对应的系统提示字样。

如下是默认逻辑的代码：

```
    if reply.type == ReplyType.TEXT:
        reply_text = reply.content
        if context.get(&#x27;desire_rtype&#x27;) == ReplyType.VOICE:
            reply = super().build_text_to_voice(reply.content)
            return self._decorate_reply(context, reply)
        if context[&#x27;isgroup&#x27;]:
            reply_text = &#x27;@&#x27; +  context[&#x27;msg&#x27;].actual_user_nickname + &#x27; &#x27; + reply_text.strip()
            reply_text = conf().get(&quot;group_chat_reply_prefix&quot;, &quot;&quot;)+reply_text
        else:
            reply_text = conf().get(&quot;single_chat_reply_prefix&quot;, &quot;&quot;)+reply_text
        reply.content = reply_text
    elif reply.type == ReplyType.ERROR or reply.type == ReplyType.INFO:
        reply.content = str(reply.type)+&quot;:\n&quot; + reply.content
```

1.  **发送回复**

根据Reply的类型，默认逻辑调用不同的发送函数发送回复给接收方context["receiver"]。

### **插件触发事件**

主程序目前会在各个消息步骤间触发事件，监听相应事件的插件会按照优先级，顺序调用事件处理函数。

目前支持三类触发事件：

```
1.收到消息
---&gt; 
ON_HANDLE_CONTEXT
2.产生回复
---&gt; 
ON_DECORATE_REPLY
3.装饰回复
---&gt; 
ON_SEND_REPLY
4.发送回复
```

触发事件会产生事件的上下文EventContext，它包含了以下信息:

EventContext(Event事件类型, {'channel' : 消息channel, 'context': Context, 'reply': Reply})

插件处理函数可通过修改EventContext中的context和reply来实现功能。

## **插件编写示例**

以plugins/hello为例，其中编写了一个简单的Hello插件。

1.  **创建插件**

在plugins目录下创建一个插件文件夹hello。然后，在该文件夹中创建init.py文件，在init.py中将其他编写的模块文件导入。在程序启动时，插件管理器会读取init.py的所有内容。

```
plugins/
└── hello
    ├── __init__.py
    └── hello.py
```

init.py的内容：

```
from .hello import *
```

1.  **编写插件类**

在hello.py文件中，创建插件类，它继承自Plugin。

在类定义之前需要使用@plugins.register装饰器注册插件，并填写插件的相关信息，其中desire_priority表示插件默认的优先级，越大优先级越高。初次加载插件后可在plugins/plugins.json中修改插件优先级。

并在init中绑定你编写的事件处理函数。

Hello插件为事件ON_HANDLE_CONTEXT绑定了一个处理函数on_handle_context，它表示之后每次生成回复前，都会由on_handle_context先处理。

PS: ON_HANDLE_CONTEXT是最常用的事件，如果要根据不同的消息来生成回复，就用它。

```
@plugins.register(name=&quot;Hello&quot;, desc=&quot;A simple plugin that says hello&quot;, version=&quot;0.1&quot;, author=&quot;lanvent&quot;, desire_priority= -1)
class Hello(Plugin):
    def __init__(self):
        super().__init__()
        self.handlers[Event.ON_HANDLE_CONTEXT] = self.on_handle_context
        logger.info(&quot;[Hello] inited&quot;)
```

1.  **编写事件处理函数**

#### **修改事件上下文**

事件处理函数接收一个EventContext对象e_context作为参数。e_context包含了事件相关信息，利用e_context['key']来访问这些信息。

EventContext(Event事件类型, {'channel' : 消息channel, 'context': Context, 'reply': Reply})

处理函数中通过修改e_context对象中的事件相关信息来实现所需功能，比如更改e_context['reply']中的内容可以修改回复。

#### 决定是否交付给下个插件或默认逻辑

在处理函数结束时，还需要设置e_context对象的action属性，它决定如何继续处理事件。目前有以下三种处理方式：

+   EventAction.CONTINUE: 事件未结束，继续交给下个插件处理，如果没有下个插件，则交付给默认的事件处理逻辑。

+   EventAction.BREAK: 事件结束，不再给下个插件处理，交付给默认的处理逻辑。

+   EventAction.BREAK_PASS: 事件结束，不再给下个插件处理，跳过默认的处理逻辑。

#### **示例处理函数**

Hello插件处理Context类型为TEXT的消息：

+   如果内容是Hello，就将回复设置为Hello+用户昵称，并跳过之后的插件和默认逻辑。

+   如果内容是End，就将Context的类型更改为IMAGE_CREATE，并让事件继续，如果最终交付到默认逻辑，会调用默认的画图Bot来画画。

```
    def on_handle_context(self, e_context: EventContext):
        if e_context[&#x27;context&#x27;].type != ContextType.TEXT:
            return
        content = e_context[&#x27;context&#x27;].content
        if content == &quot;Hello&quot;:
            reply = Reply()
            reply.type = ReplyType.TEXT
            msg:ChatMessage = e_context[&#x27;context&#x27;][&#x27;msg&#x27;]
            if e_context[&#x27;context&#x27;][&#x27;isgroup&#x27;]:
                reply.content = f&quot;Hello, {msg.actual_user_nickname} from {msg.from_user_nickname}&quot;
            else:
                reply.content = f&quot;Hello, {msg.from_user_nickname}&quot;
            e_context[&#x27;reply&#x27;] = reply
            e_context.action = EventAction.BREAK_PASS # 事件结束，并跳过处理context的默认逻辑
        if content == &quot;End&quot;:
            # 如果是文本消息&quot;End&quot;，将请求转换成&quot;IMAGE_CREATE&quot;，并将content设置为&quot;The World&quot;
            e_context[&#x27;context&#x27;].type = ContextType.IMAGE_CREATE
            content = &quot;The World&quot;
            e_context.action = EventAction.CONTINUE  # 事件继续，交付给下个插件或默认逻辑
```

## **插件设计建议**

+   尽情将你想要的个性化功能设计为插件。

+   一个插件目录建议只注册一个插件类。建议使用单独的仓库维护插件，便于更新。

+   在测试调试好后提交PR，把自己的仓库加入到[仓库源](https://github.com/zhayujie/chatgpt-on-wechat/blob/master/plugins/source.json)中。

+   插件的config文件、使用说明README.md、requirement.txt等放置在插件目录中。

+   默认优先级不要超过管理员插件Godcmd的优先级(999)，Godcmd插件提供了配置管理、插件管理等功能。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>