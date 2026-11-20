
# AI实践：2小时带你使用 coze+windsurf 打造一个网站内容提取谷歌插件

> 来源：[https://agijuejin.feishu.cn/docx/UJDodFN6XoJiySxOy9tcrFSZn6b](https://agijuejin.feishu.cn/docx/UJDodFN6XoJiySxOy9tcrFSZn6b)

> 作者：战场小包

> 链接：https://juejin.cn/post/7442335226184351785

> 来源：稀土掘金

> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 前言

工作转眼就接近半年之久，潇洒生活一去不复返，充裕的时间变得捉襟见肘，不由得感到一丝丝焦虑，有些时候连阅读blog的时间都很难抽出来。

作为一个程序员，为了保持技术的敏感度，以及对于日常实事的了解，文章阅读是必不可少的，如何快速的筛选优质文章并进行高效阅读，成为这一段时间我苦思冥想的梦魇。

起初解决方案是压力自己，提高工作效率，这样就能实现早下班，抽时间来进行学习，但是作为一个AI时代的程序员，压力已经够大了，不能再自我pua了～。

这段时间 marscode、cursor、windsurf 等多款 AI 代码生成工具如火如荼，看了很多小伙伴的实践，可能我也可以做一款工具来改善现状，于是边查边学，就有了今天这个创意————谷歌网址内容总结插件。

当我真正开始做这个插件，才发现，AI 最近的进步已经开始超越了我的想象，实现的过程的简单程度也超出了我的想象。对于外行小白来说，AI 的入门门槛还是很高，特别是神经网络模型方面，自我体验下来有几分困难。后面转变了想法，做不了AI的推动者，可以做AI的实践者、应用者。

大家可以跟我一起体验一下用 coze 和 windsurf 做一个谷歌的插件有多简单和快乐（当时自己做这个插件的时候，大约是从晚上11点到凌晨1点左右，2个小时时间，就实现了核心功能）

不多啰嗦，进入正题，今天的主角是 coze 和 windsurf。

## coze 搭建内容提取工作流

> 后面有时间，会写一系列学习和使用 coze 的实践文章。

[coze](https://link.juejin.cn?target=https%3A%2F%2Fwww.coze.cn%2Fstore%2Fagent%3Fcate_type%3Drecommend) 大家应该不陌生了，在前一段时间，铺天盖地的宣传。简单来说，coze 是一个提供了智能体搭建的 AI 应用开发平台，官方提供了丰富的插件和工作流，使用这些可以较为轻松的搭建各式各样的智能体，诸如下面:

![](img/5946bdf7d853fb69c75d747db19251d3.png)

今天主要使用 coze 中的工作流模块，工作流指的是什么那？

> 工作流支持通过可视化的方式，对插件、大语言模型、代码块等功能进行组合，从而实现复杂、稳定的业务流程编排，例如旅行规划、报告分析等。

官方的讲述还是有些抽象，其实就类似于面向过程的编程思想，把一个个流程链接起来，实现较为复杂的功能，其中每种流程可能会使用不同的方式进行实现，例如借助插件、大模型等。

下面是工作流的入口，点击右上角新建一个工作流，创建的时候要注意，工作流名称只能是字母和下划线，描述尽可能清晰的介绍工作流的作用。

![](img/b63c000689d91e9d0907706b79d557f8.png)

创建好工作流后，就可以获得一个画布界面，左侧是实现流程的各类节点，右侧是画布，存在一个输入节点和输出节点，其中节点支持随意拖拽和链接。

上文说过工作流的搭建过程可以粗略的理解为面向过程的思想，那么实现一个内容提取的工作流，你会想到哪些步骤：

1. 获取网址，网址作为工作流的输入

1. 根据网址获取网站内容

1. 对内容进行总结

1. 输出内容

没错，就是这么简单的四步，那么咱们一步一步的开始，搭建工作流：

### 配置输入节点

配置一下输入的参数为 `article_url`，当然这个字段自由发挥，不用非按照我的，配置效果如下：

![](img/44ebf6e644f987a22020be0e9e685642.png)

### 获取网址内容

在以往，根据网址获取内容要通过爬虫等一些方式来实现，但是在 coze 中，只需要打开插件就可以看到一个链接读取的 `LinkReaderPlugin`，添加到工作流中，同时将输入节点和 `Link` 节点进行连线。

点击`Link`节点，需要配置输入的参数，参数值有两种声明方式，引用指使用前面节点的输出值，输入则指直接输入固定值。`skip_cache` 代表是否跳过缓存，为了token考虑，设置为 false，不跳过，避免反复总结，浪费token；`url` 使用输入节点传入的`article_url`字段。

### 内容总结

内容总结，就需要大模型节点出手了，点击左侧引入大模型节点，大模型节点可配置项比较多，这里粗略的讲一下:

+ 模型：coze提供的大模型还是较多的，有豆包系列、kimi系列、通义、智谱等等，这里我选择了豆包-pro 32k模型

+ 输入：大模型接受的输入值，这里配置 `Link` 节点`data`中的`content`即可

+ 系统提示词：可以理解为cos，你想让大模型cos什么行为，就写明什么，书写逻辑一般是:

+ 身份：您是一个xxx，能够xxx

+ 任务：把 xxx 转变成 xxx

+ 结果：得到 xxx

+ 输出：规定大模型输出的内容，这里大模型会根据描述将相应的内容映射到撰写的变量中

### 输出内容

输出节点比较简单，将大模型节点的返回值进行输入，唯一的交互在于选择回答方式，如果选择设定内容回答，可以在回答内容区约定一下输出格式。

![](img/18f4451b83795910f9907bbff1a5da17.png)

### 试运行

将所有的节点连接起来，就实现了一个简单的大模型，点击右上角的试运行，测试一下：

![](img/c71a56a6365737d0f3059b1d9b2e5c07.png)

试运行成功后，就可以点击发布了，就这样你就拥有了一个AI网址内容提取的工作流，输入网址即可得到想要的内容。

工作流发布后，可以通过 API 进行调用，以当前的工作流作为服务端，谷歌插件中使用api调用传入url，就可以实现内容提取了。

## coze API 调用

> [coze workflow](https://link.juejin.cn?target=https%3A%2F%2Fwww.coze.cn%2Fdocs%2Fdeveloper_guides%2Fworkflow_run) API 调用文档

官方文档中提供了如下案例，需要两个核心参数：授权令牌密钥和工作流 id。

![](img/ecefa6884dbf29f30721b9b54d4bbda3.png)

授权令牌密钥获取：

![](img/d87cfc345ec980c309f7999177864ca8.png)

![](img/d8b4f0a5333c615b4407f1976a3022bd.png)

进入刚才创建的工作流，url中 query 部分提供了工作流id

![](img/f004e2831ddb7b2fb1b3a185d287e3cd.png)

## windsurf 编写谷歌插件

[windsurf](https://link.juejin.cn?target=https%3A%2F%2Fcodeium.com%2Fwindsurf) 是 Codeium 团队推出的 AI 编辑器，类似于 cursor 和 marscode，暂时是免费的，具体就不详细介绍了，网上有各种各样的测评对比。

![](img/a36446f7e66423af6a3bbe8928b66e8a.png)

目前比较喜欢 windsurf，主要有两点原因，其一右侧的 cascade 支持 write 和 chat 两种模式，write 专注于代码生成，chat 则负责进行自然语言交互；其二暂时是免费的，而且使用下来代码生成效果还是较好的。

既然选择了使用 AI 助手进行编程，期待的是连脑子都不用动太多，更别说让我去写代码了，说说话就把事办了，所以核心的如何说明白，也就是如何书写提示词。

> 注：在这个模块中，可以使用cursor或者 marscode 进行代码生成，不用非使用 windsurf。

交互的过程还是比较多的，这里挑一些关键的节点进行一下叙述，大家可以参考一下我的过程。

**Step1**: 最开始的交互点明核心功能：您好，帮我创建一个谷歌插件开发程序。windsurf 非常简单和明了的告诉如何编写一个最基础的插件。

> 当然可以描述的更为细致和清晰，例如使用vue3+ts帮我创建一个xxx之类。

![](img/5f57e64c7cf544b12678737c9c174936.png)

**Step2**: 指令：在`popup.html`中我需要有一个按钮，点击可以获取到当前`url`，生成代码如下

+ html

```html
<!DOCTYPE html><html lang="en"><head>    <meta charset="UTF-8">    <title>My Extension</title>    <script>        document.addEventListener('DOMContentLoaded', function() {            document.getElementById('getUrlButton').addEventListener('click', function() {                chrome.tabs.query({active: true, currentWindow: true}, function(tabs) {                    var currentTab = tabs[0];                    alert('Current URL: 'currentTab.url);                });            });        });    </script></head><body>    <h1>Hello, Chrome Extension!</h1>    <button id="getUrlButton">Get Current URL</button></body></html>
```

还指出 `manifest.json` 中需要开启 `tabs` 权限。

```json
{  "manifest_version": 3,  "name": "My Chrome Extension",  "version": "1.0",  "description": "A simple Chrome extension example.",  "action": {    "default_popup": "popup.html",    "default_icon": "icon.png"  },  "permissions": ["tabs"]}
```

**Step3**: 发送请求，coze API 文档中提供的请求案例，并不是前端的常用方式，借助 windsurf 将其转变成 js 代码块等功能进行组合

![](img/5649cee35678252e8bb6abdb627d9442.png)

可以发现 windsurf 不给给出了请求的方法，还帮写了事件的触发方法。但此时需要注意一个问题，coze 工作流中的输入为 `article_url`，注意请求时传的参数与之相同。

![](img/033081edbf69164f593ed982734c911c.png)

**Step4**: 使用 chrome 加载当前插件

由于 windsurf 生成的 js 文件内嵌在 `popup.html` 中，此时引入后，点击图标会抛出一个错误：

![](img/64883921638acc8869f610621288accd.png)

将错误直接复制到 windsurf 中，问题原因为：Chrome扩展程序的内容安全策略（CSP）不允许内联脚本执行。

**Step5**: 初步测试，找一个文章，点击总结内容按钮，得到文章的总结和评分

> 上面的过程中简化了一些样式调整的交互

**Step6**: popup 处的点击有些麻烦，进一步的想法是有个悬浮球，点击悬浮球自动总结内容

![](img/ac9ee96c7eebd6a7b2eae67e3847ece2.png)

在这个步骤中，遇到一些挫折，windsurf 给出了一些错误的实现方式，主要原因还是在于提示词写得不够精确，经过一番调整，得到最终结果如下：

![](img/9219ec82aeb846d64324f8b90a6f0cbc.png)

## 总结

从头到尾大致耗费了两个小时左右，当然最后实现的插件还有很大的优化空间，但是基础的功能需求是实现的，通过这个插件，可以较为快速的筛选和阅读文章。

这个过程中，也学会了很多，对于当下的 AI 时代，做一些以往看起来非常困难的程序并不难，感觉应了那句话：种一棵树的最好时间是十年前，其次是现在。还有一个比较关键的点，从来没有开发过谷歌插件，但是借助 windsurf，比较迅速和成功的实现了一个可用的谷歌插件。

希望通过本文简单的分享，能够激起对 AI 的兴趣，加入 AI 实践者和应用者的行列。

下面总结一下本次插件搭建的一些新的收获：

1. coze 工作流搭建其实并不高大上，也没有想象中那么困难，只要提前的理清步骤，每个步骤使用相应的节点，就可以搭建起一些较为简单的工作流。

1. 在文本内容处理方面，coze 工作流可以成为日常开发应用的服务端，通过 coze api 来实现一些复杂逻辑。

1. 要精进提示词的编写能力，有几个要点：清晰、具体、多学习套路。

1. AI 编程助手的能力有点超乎想象，有什么想法理清思路后，感觉可以很快的实现。
