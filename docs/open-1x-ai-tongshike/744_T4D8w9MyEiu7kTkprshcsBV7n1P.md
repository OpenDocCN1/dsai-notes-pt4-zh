# 在Coze中接入GPT4o或其它自定义模型

> 来源：[https://datawhaler.feishu.cn/docx/D0a2dWMTRoK0GgxqW2tcybTfnfh](https://datawhaler.feishu.cn/docx/D0a2dWMTRoK0GgxqW2tcybTfnfh)

> 在 Coze 中，目前支持的 LLM 模型数量有限。当我们想要使用 GPT-4o 或其他尚未支持的模型时，往往会面临诸多限制。

> 接下来，我将分享如何在 Coze 中接入 GPT-4o 的方法，希望能为大家提供一些参考和帮助。

# 思路介绍

在 Coze 中，可以通过对话流模式来处理消息，这是一种高效且灵活的方式。为了接入 GPT-4o，我们可以创建一个自定义插件，并在其中调用该插件。

![](img/f7e7169c152c355fc17c43d96069002d.png)

使用自定义插件时，会有一个问题，我们无法直接获取用户的聊天记录。为了解决这一问题，可以引入数据库作为解决方案。

![](img/ff6a530c24ba37112d60847be130f63a.png)

我们可以在对话流程中将用户的聊天记录实时存储到数据库中，而在调用 自定义插件 时，再从数据库中读取这些聊天记录。这样即可解决聊天记录的问题

![](img/0e14ecfb5e7dea56b15fad3c18eccded.png)

希望这个方法能为大家在类似场景下提供一些思路和帮助。

# 详细步骤

## 一、新建插件

> 以下是自定义插件的详细步骤（高手可略过）、

在资源库中，点击新建插件

![](img/a145d13fccfc12ae27fb1170189368d0.png)

写入插件名称，和插件描述，然后选择云侧插件 - 在Coze IDE中创建。

IDE运行时，可以根据自己的喜好进行添加，可以使用Nodejs也可以使用Python，我使用的Python进行演示

![](img/aa44ce3a0a112131c21449a4770cb1f6.png)

之后的步骤，其实就是下一步下一步，然后输入工具名称和工具介绍。

Coze中的插件，是一个插件可以新建多个工具，所以我们新建插件之后，还需要新建工具。

![](img/410cb69ef75dfffb201c2a9425792dfb.png)

![](img/b33179ab06e029d1b80fb87261628d25.png)

![](img/f2645b74fdfc1fed78ae712e04abb8b2.png)

在coze ide中，可以添加依赖，我们添加一个openai的依赖，点击添加依赖，在弹出的框中输入openai即可，之后即可安装上openai的依赖。

![](img/dc8b5670eead304a49ff370c1fe22307.png)

在元数据的选项卡中，我们需要编辑一下输入参数和输出参数，按照下面的方式添加好即可。

参数信息为

+   messagelist，这便是我们传递给大模型的消息

+   model

+   baseurl

+   apikey

+   system_prompt 系统提示词

![](img/863f1529f57dc4d85290ce9b7189ec89.png)

下面是对应的代码，其实就是调用openai接口的最简单的形式。

```
from openai import OpenAI

def handler(args: Args[Input])-&gt;Output:
    modelname = args.input.model
    messagelist = args.input.messagelist
    base_url = args.input.base_url
    api_key = args.input.api_key

    client = OpenAI(
        base_url=base_url,
        api_key=api_key,
    )
    system_prompt = args.input.system_prompt
    systemprompt = system_prompt + &quot;\n\n&quot; + &quot;已知信息：\n&quot;

    messages = [{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: systemprompt}]
    if messagelist:
        for message in messagelist:
            messages.append({&quot;role&quot;:message.role,&quot;content&quot;:message.content})

    print(messages)
    
    response = client.chat.completions.create(
            model=modelname,
            messages=messages,# type: ignore
            stream=True,
            temperature = 0.6
        )# type: ignore
    content = &quot;&quot;
    for chunk in response:
        # 检查chunk是否有content属性并且不为空
        if hasattr(chunk.choices[0].delta, &#x27;content&#x27;) and chunk.choices[0].delta.content:
            content += chunk.choices[0].delta.content
    return {&quot;response&quot;: content}
```

![](img/b700cac33bbd8bcd23cb87daac0c6dfe.png)

配置完成后，点击发布，然后下一步下一步即可

![](img/cf0b000eeaec9e46d0d4182d5f96229f.png)

## 二、新建数据库

在智能体的下面数据库那里点击添加，然后新建数据表，

![](img/d56395ca00013f7b6185008db2cea4fe.png)

点击新建数据表，然后写入数据表名称，数据表描述，再点击确定

![](img/403531df2b2c73a08c4b114e36167b0f.png)

![](img/69c9d47671927c44dffdcebfb0bb404a.png)

新建两个字段，role，content。然后点击保存即可

![](img/ca09da668c32d6fd852e691bdab8f2d1.png)

## 三、对话流添加

> 对话流模式中，我们每次对话都会进入一个固定的对话流，在这个流程中，我们可以添加自己的逻辑。使得我们可以完全掌控Agent的每一步运行。

我们需要选择单Agent（对话流模式）

![](img/ded5928009c1b8318f64da8a1b3b88bf.png)

之后点击添加对话流，并且输入对话流名称以及描述信息，再点击确认即可

![](img/a8795596f522b19be75132cc4da7c4f4.png)

![](img/bbbb221a98f5baa2af6c63712cdbecde.png)

下图便是一个对话流，初始对话流包括两个节点，开始和结束，开始节点中有两个变量USER_INPUT、CONVERSATION_NAME。USER_INPUT中即是用户的输入。

目前尚不明确另一个参数的用途，我经过测试CONVERSATION_NAME的值无论是调试时，还是正式发布后，全部传递的是Default

![](img/18cb6a9e086913f17f3e5f0c1d735e3d.png)

## 四、对话流配置

首先我们点击页面最下方的添加节点按钮，添加一个新建数据的节点，在该节点中我们将用户输入的消息添加到数据库中。role字段设置为user，content设置为开始节点的USER_INPUT

![](img/4a2c18dfb92127698e0aa8bcd5ddcc69.png)

之后添加一个代码节点，在这个节点中，可以配置一些参数信息，点击在IDE中编辑即可打开代码编辑页面。

![](img/d2a941f1c9d440cb1255025c2c7b457f.png)

根据自己的喜好选择语言，可以使用JavaScript也可以使用Python

![](img/d0648adb0fc56a245c8ddca8e9e27d4c.png)

在页面右侧设置好几个输出参数，然后在左侧的代码中为这些输出参数赋值。

![](img/e6e84656ddda5d52dffba9871eb13e51.png)

设置好变量后，添加一个查询数据的节点，用来查询用户的聊天历史。

![](img/789db3822e13921ef35ee53295ee9726.png)

查询到聊天的历史记录后，添加一个插件节点，然后选择我们刚刚创建的工具，然后点击添加

![](img/b9102de130270ab008584a00c8b59d78.png)

设置好插件的变量，messagelist是从数据库的查询中获得的，其它参数是在前面的代码中设置的。

![](img/fe2b6ad6f7e348d5164c86ba96fdff88.png)

再添加一个新建数据的节点，向数据库中添加AI的回复。然后将插件的回复信息，添加到结束节点中。

![](img/bb30982f557889535725510d6c3f7154.png)

设置完成后，点击下方的试运行，测试一下是否成功。

![](img/841391884f9960f7f7436514484f625f.png)

成功后发布该对话流即可

![](img/8f2878fa4365cfd4aa3da14906375de5.png)