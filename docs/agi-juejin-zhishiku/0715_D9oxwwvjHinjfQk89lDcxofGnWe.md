# 「豆包Marscode体验官」AI加持的云端IDE——三种方法高效开发前后端聊天交互功能

> 来源：[https://agijuejin.feishu.cn/docx/VvLRdEHGMoGDgixRRh6cPgAln1F](https://agijuejin.feishu.cn/docx/VvLRdEHGMoGDgixRRh6cPgAln1F)

> 作者：努力的小雨

> 链接：https://juejin.cn/post/7389912762044923904

> 来源：稀土掘金

> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

> 豆包 MarsCode 是一个集成了AI功能的编程助手和云端IDE，旨在提高开发效率和质量。它支持多种编程语言和IDE，提供智能代码补全、代码解释、单元测试生成和问题修复等功能，同时具备AI对话视图和开发工具。

## 豆包 MarsCode

豆包 MarsCode 编程助手支持的 IDE: 支持Visual Studio Code 1.67.0及以上版本，以及JetBrains系列IDE，如IntelliJ IDEA、Pycharm等，版本要求为221.5080.210及以上。

豆包 MarsCode IDE 是一个云端 AI 集成开发环境(IDE)，具备原生 AI 编程助手，提供代码补全、生成、优化等功能，支持多种编程语言和框架，无需本地资源，实现随时随地的开发。

豆包 MarsCode官方文档地址：docs.marscode.cn/introductio…

### 故事起源

故事要从上周六说起。周五下班时大家都兴致勃勃，冲动之下，我也顾不上带电脑，径直开车上高速回家，急着与家里的丫头玩耍。但一到家才发现，书包居然没带。但我手头上还有一些未完成的开源项目工作，以及一个即将截止的征文活动，这些都需要我尽快完成。面对家中没有集成开发环境（IDE）的电脑，可愁死我了，正好当时我在浏览技术社区掘金时，偶然发现了豆包 MarsCode IDE，便立即开始使用这款在线IDE。

用的还不错吧，虽然小问题一堆，但是没啥大毛病，正好把我使用的体验感受写一下分享给大家。

### 先说体验

#### 时间效率

在整个流程中，毫无疑问，使用豆包 IDE 比起先装环境要快得多。程序员们最头疼的就是环境安装，特别是在老式电脑上，我甚至担心会浪费时间。因此，使用豆包 IDE 的直观感受是它快速而稳定。整个环境安装再加上熟悉豆包 IDE，总共花了不到一个小时就可以正常启动项目，其中大约30分钟用在了配置mvn依赖上。这个过程有点坑，难怪会耽误时间。但如果是一个正常的项目，我相信在30分钟内就能搞定。

#### 代码质量

其次，再谈谈我对AI功能的使用体验。实际上，最常用的是代码注释，因为大多数程序员包括我在内，不太喜欢写注释。使用云端IDE平台带来的便利在于，首先无需安装额外的编程助手；其次，通过AI悬挂图标直接选中问题或代码段，能快速解决困扰。这样做非常方便，而且最重要的是可以提升代码质量：一方面减少了对变量名的纠结，另一方面也让代码注释变得更为到位。

## 深度体验豆包 MarsCode IDE

以下是整体流程的详细描述，以确保信息清晰和逻辑连贯，防止大家头脑混乱：

> 登录豆包 MarsCode IDE 工作台 ——> 导入 GitHub 仓库 ——> 选择开发环境模板 ——> 配置项目 ——> 运行项目 ——> 体验项目 ——> 代码提交

### spring-ai

使用云端 IDE 的主要优势之一是其适用于个人开发者，而大公司通常有自己的代码仓库。对于个人开发者而言，选择云端 IDE 编写开源项目再合适不过，特别是豆包 IDE 提供的资源分配非常充足。这意味着不必担心本地环境可能导致的宕机、电脑死机或磁盘损坏等问题。

[然而，对我来说，稳定的网络连](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2FStudiousXiaoYu%2Fspring-ai-demo)接是最关键的因素。在家里，访问 Github 时经常面临网络连接问题或速度极慢的情况，这对开发效率有显著影响。因此，我决定通过编写我的开源项目【spring-ai-demo】来深入体验豆包 IDE 的功能和性能。

#### 快速导入GitHub 仓库

请访问豆包 MarsCode AI IDE 的官方网站：www.marscode.cn/dashboard

在开始之前，请登录豆包 MarsCode IDE 工作台，并导入我们的GitHub仓库项目，操作步骤如下所示：

![](img/c7a619dd4c66b26b65af34477ea60010.png)

在进行 GitHub 授权后，你可以访问自己的仓库项目列表。在选择要开发的项目时，系统会自动识别适用的语言模板，例如，如果你选择的项目是 Java 的话，它会自动识别为 Java 模板。

> tips：单个账号在豆包 MarsCode IDE 上创建的项目数量不得超过 10 个

![](img/cf31fac3816dc2affcd0bebf628e8a6a.png)

当你导入创建项目时，速度非常快，基本可以在几秒钟内完成，而且还会自动识别为 Maven 项目。

#### 依赖环境

拿到新项目后，我们的操作步骤与通常一样，唯一的不同是无需搭建运行环境，只需下载依赖即可。更方便的是，我们可以直接从阿里云仓库下载依赖，无需单独配置mvn的setting文件，这一点特别贴心。

这一步直接报错了。按照官方的依赖包下载的话，在阿里云镜像中是找不到的，具体如下所示：

![](img/f4fb046fb84f039a3ba941efb32ea1b0.png)

> tips：在pom中指定了repositories仓库地址，也不会生效，可以认为只能去阿里云仓库

![](img/64b784c4217772a15c45e73dedc98b24.png)

当即使指定了下载包仓库，仍然无法成功时，我出于好奇查看了阿里的镜像，发现其不存在。进一步调查后发现，阿里云的镜像groupid已经更改。因此，我也需要相应地进行修改：

修改后，终于成功实现了正常的下载功能，但我仍不清楚是否损失了哪些功能。这可能被视为一个缺陷，因为我的项目是开源的。如果某些小公司内部依赖于私有仓库的包，那么他们可能会遇到问题。

#### 启动项目

在启动项目时，这里会持续提示各种配置项，只需直接启用它们，即可顺利完成启动操作。

![](img/c386d7bb52b2bac0edfcff231d5059cc.png)

确保在启动任何maven项目之前，务必确保项目已成功编译完成，否则可能会导致出现各种文件找不到的错误。

![](img/44da598cd68a1537223e422ffaed948e.png)

同样地，你也可以在这里配置适合自己习惯的操作方式：如clean install -DskipTests=true

#### 体验项目

启动成功后，将会显示相应的路径及其所监听的端口信息，让我们来查看一下吧。

![](img/275304de9795403fdf63e6717bd742bf.png)

这里是我们项目真正使用的端口监听。如果需要详细了解，请查看项目配置文件中的相关信息。好的，让我们进行访问。

当我们查看 web-view 时，请将输入的路径转换成对应的路径。举例来说，如果我直接访问 swagger-ui，那么输入的路径就是 doc.html。

![](img/b82e2224fb71482ff2be27f7a8fee732.png)

此外，除了这些路径，我只需在此页面上点击即可查看和操作剩余的所有路径。

![](img/3a99beb06f1c700ffe9dac5b95f676da.png)

当然，如果你不希望在这里使用webview，也可以复制路径并在浏览器中打开以访问。总体而言，项目启动的基本环境依赖操作通常可以在30分钟内完成。

![](img/47560e363304546377ca1c571343ba1f.png)

在这里，不仅可以根据相应的路径找到对应的控制器，还可以观察到垃圾回收器的工作状态，这使得这个视图变得非常有用和优秀。

![](img/20de5a82754b2f84576ae5166a52d0a9.png)

当鼠标悬停在注解上时，甚至可以显示出本类中的对象是如何被注入的。这对于研究源码来说也是非常有用的功能。

![](img/d7d905dc8dcd5fd38b7a5f5927c82582.png)

小操作的一项优化是支持文件重命名时，同时修改其中的类名。

### 编辑项目

完成了上述基本操作后，接下来我们将开始今天的主题：继续维护我的开源项目spring-ai-demo。今天我们将深入探讨函数调用。

大型语言模型（LLM）在训练完成后被冻结，导致其知识陈旧，并限制了其访问和修改外部数据的能力。

为了解决这些问题，函数调用机制应运而生。这种机制允许你注册自定义函数，将大型语言模型连接到外部系统的API。这些外部系统能够为LLM提供实时数据，并代表它执行数据处理操作。

Spring AI简化了支持函数调用所需的代码编写。它处理了函数调用的对话流程，使你能够轻松地注册函数。你只需提供函数的Bean名称作为a，然后在提示选项中选择该名称以激活该函数。此外，您还可以在单个提示中定义和引用多个函数。

![](img/b47013db141dbefcb41aadfb41f38c19.png)

1.  执行聊天请求及函数定义信息的处理，例如解释模型何时应该调用函数以及函数的输入参数模式。

1.  模型决定调用函数时，会传递输入参数给相应的函数，并将输出结果返回给模型。

1.  Spring AI 管理此对话流程，将函数调用分发给适当的函数，并将结果传送回模型。

1.  模型可以执行多个函数调用以获取所需的所有信息。

1.  一旦收集到所有必要信息，模型将生成响应。

#### 补全代码

这里先快速写一个入口，tab补全代码

![](img/05a804b5f1f8e1a42e3ec348f3faac82.png)

#### 报错修复

在复制粘贴过程中，IDE并不会自动添加相关的依赖包。因此，每当遇到类报错时，如果IDE提供了快速修复选项，请优先点击执行修复操作；如果没有快速修复选项，则建议参考AI fix的建议进行手动修复。

![](img/823eaa1e25f4d7fc44cf3c22345c2f98.png)

#### 代码注释

这个功能简直是太赞了，我真的很喜欢它。因为有时候实在懒得打字，但在线IDE可以根据我的输入自动理解代码的意图，并且根据我的简短提示自动生成后续的注释。这对于我这种懒人来说简直是福音啊！就看看它的效果吧。

#### AI 问答

有任何报错，无论是来自控制台还是其他地方，他都能够利用智能问答进行优化和解决。

同样也有历史记录进行维护，这样可以方便回顾和查看之前的错误记录。在记录问题时，根据解决方法详细描述，以确保下次不会再犯同样的错误。

最后重新启动项目即可。尽管已经进行了函数回调，但由于地址传错，无法避免。毕竟这些API都由开发者自行管理，最初我以为是官方的问题，但询问后发现与官方无关。

![](img/d451e4d7fa7a563afb5b5e9af3e53a33.png)

> tips：这里注意下每个路径都是有时间限制的，所以如果长时间不刷新，路径也将会失效，点击刷新即可。url路径是不会改变的

![](img/829f213d060c0a667a911c9dabfae1a6.png)

#### 单元测试

对于程序员另一个福音则是快速生成单元测试，对于开源项目来说，单元测试同样具有重要价值。它们不仅能够保证代码的质量和稳定性，还能够吸引更多的贡献者参与到项目中来。一个拥有良好单元测试的项目，可以降低新贡献者的入门门槛，因为他们可以快速地验证自己的代码更改是否符合预期，从而更有信心地参与到项目的开发中。

然而，编写单元测试确实需要投入额外的时间和精力。毕竟，开发工作往往需要"用爱发电"，即在有限的资源和时间内，尽可能地发挥创造力和热情。这种情况下，就需要开发者权衡利弊，决定是否有足够的精力来维护单元测试。

#### 提交变更

当你完成开发并维护相关代码之后，可以直接使用此接口进行Git提交，从而实现一站式的服务。

## 前端UI聊天交互页面

前几天有粉丝在评论区建议增加一个前端交互页面，认为这会使体验更加丰富，于是我们便安排了这个任务。我们主要探索了两种不同的前端页面设计：一种是采用豆包的React模版，另一种是直接使用HTML模版，还有一种特殊的Python模版。好的，言归正传，让我们首先来探索React模版。

### React

#### 项目模版创建

我掌握了一些HTML、JavaScript和CSS的基本操作，对React框架几乎没有经验。因此，我会使用IDE提供的模板来创建项目，而不必过多关注环境依赖，直接启动即可成功。好的，那么让我们开始吧。

![](img/73e4d70506510ff57d34f4761e0614e3.png)

当你进入这个页面时，你会发现官方提供了一份贴心的README文件，可以帮助你快速了解如何启动这个程序。

#### UI交互

启动结束后，我浏览了一下页面，但我对这个东西基本上一头雾水，页面上的元素我都不认识。好的，我想通过AI来了解一下，看看它是否能够满足我的需求。

> tips：这里注意下，问问题的时候，一定要选择修改的页面，如果选的不对，AI也是无法直接回答问题的。

![](img/4313b0cc666c29f45675cd68add7f760.png)

生成的结果看起来确实不错，但幸好我有一些前端基础知识，因此能够发现生成的属性有问题。经过修改后，问题得以解决。

![](img/e8e7d98a726eabb752c49a4976d283cc.png)

接着立即观察生成的结果。

> tips：这里发现个问题，webview无法显示alter弹窗信息，不过复制出来在浏览器中使用即可，无伤大雅

![](img/6c2359582f7b5f8bb47827e8e4df01d6.png)

好的，我们已经确定了基本组件。剩下的优化工作我们稍后再进行，先完成后优化师我开发的原则。接下来要做的是生成按钮点击后要跳转的页面。让AI来帮我整理一下吧。

![](img/f6ce9e57215049dd8dc2be4073dd7b5c.png)

好吧，生成的内容一直报错，而且他们还不给修复。

![](img/d683b26b1a7d8977ea66a0fc169a9e4a.png)

那么我们可以考虑另一种解决方案。考虑到这是一个框架，应该有专门的组件可以直接完成这项任务，特别是在当今LLM流行的环境下，聊天页面也正迎来了蓬勃发展。

![](img/2d5781a7bdc69c0ce7e056bd53369117.png)

#### ChatSDK

找了半天，确实没找到满意的解决方案。于是我向各位前端大佬请教了一下，毕竟我在前端框架方面几乎没有经验。他们根据我的需求推荐了一个非常强大的一站式UI解决方案——阿里的chatSDK，你可以在以下地址找到它：

[chatbot.console.ali](https://link.juejin.cn?target=https%3A%2F%2Fchatbot.console.aliyun.com%2FChatSDK%23%2Fsdk)yun.com/ChatSDK#/sd…

![](img/eefbd6e13ba053d7c8ddf6e6209eec2c.png)

这就是我想要的结果，不过在配置的过程中遇到了很多问题，因为我对此一无所知。尽管如此，我仍然坚持学习并逐步掌握。幸运的是，通过查阅官方文档，我得以解决了这些困惑，即便我的AI助手也跟我一样陷入了困境。

![](img/308a960f65d572e2bd8d048a34e86346.png)

#### Rsbuild

没错，就是指的rsbuild。虽然文档写得很详细，但我还是去看了如何设置index.html。结果发现操作很简单，只需要简单配置一下就行。可能是因为rsbuild并不像webpack那样流行，所以网上关于它的配置信息不多，我一直找不到相关的资料。

![](img/17db64bb9831a0a90148eae5ffdfa052.png)

这里来详细介绍一下rsbuild。为什么在React项目中选择自动集成rsbuild，而不是像市面上流行的webpack等其他工具呢？

Rsbuild是一个基于Rspack的高性能构建工具，它不仅提供丰富的构建功能和优化的配置选项，还支持多种前端UI框架。Rsbuild的特点包括出色的性能表现、简便的配置方式、丰富的插件生态系统、稳定可靠的构建产物以及与各种前端框架的无缝兼容性。与类似的工具如Vite、Create React App和Vue CLI相比，Rsbuild不仅能够实现更高效的构建过程，而且能够更好地融入复杂的生态系统中。

![](img/7ef3a3962b0dd74bc76f29dc0ea8d0fc.png)

Rsbuild，由字节跳动推出，旨在构建一个开放的技术生态系统，以积极推动前端技术的进步和发展。

因此，首先，Rsbuild 拥有更高的构建性能，能够提供比基于 Webpack 的 Create React App 和 Vue CLI 等工具高达 5 到 10 倍的效率，同时还具备更为轻量的依赖体积。其次，Rsbuild 与豆包一样是属于一家公司的产品，所以成为了一个优选的选择。

这里唯一的不足是社区问题太少，当我遇到错误时，很难找到解决方法，只能自己苦苦探索，甚至要通过Google搜索来解决。有时甚至连关键字【Rsbuild+问题】都无法搜索到相关信息。这方面值得进一步优化。

其实一句话即可。我们去看下去

![](img/25908339995b9d4e5194cdf8377cc4db.png)

设置成功后，我们将按照阿里提供的SDK快速开发文档进行配置，然后启动应用程序进行测试。

![](img/b1f0dc1499f559ffb535724c335348c3.png)

可以看到，所有必要的内容已经成功添加到系统中。接下来，我们将继续开发以实现跳转到聊天页面的功能。首先，我们需要完善各种路由的配置，确保一切就绪后再继续进行跳转功能的开发。这一步骤只需要完成一个小组件即可。

```
import React from &#x27;react&#x27;;
import ReactDOM from &#x27;react-dom/client&#x27;;
import App from &#x27;./App&#x27;;
import ChatHome from &#x27;../components/chat&#x27;;
import { BrowserRouter as Router, Routes, Route } from &#x27;react-router-dom&#x27;
const root = ReactDOM.createRoot(document.getElementById(&#x27;root&#x27;)!);
root.render(
  &lt;React.StrictMode&gt; 
    &lt;Router&gt;
        &lt;Routes&gt;
            &lt;Route path=&#x27;/talk&#x27; element={&lt;ChatHome /&gt;}&gt;&lt;/Route&gt;
            &lt;Route path=&#x27;/&#x27; element={&lt;App /&gt;}&gt;&lt;/Route&gt;
        &lt;/Routes&gt;
    &lt;/Router&gt;
  &lt;/React.StrictMode&gt;,
);
```

当需要添加链接时，剩下的任务就是在合适的位置插入超链接。可以简单地使用代码自动补全完成这个任务。

![](img/6eaf20fa576a41b6109846d67e0b0bee.png)

当最后成功集成进来时，所带来的效果如下所示：

![](img/8e87b2b39acc0053d66d480908a2a7bf.png)

#### 优化后端

接下来，我们将继续对接之前我们开发的spring-ai项目，以实现一个真正交互式的界面。

![](img/c60040ded4cd7ccea19a7af29fb410dc.png)

那么为什么不能直接使用record返回呢？因为前端不仅仅支持文本，还有其他方案。所以还是自己写吧。总之，目前即便是AI也可能会出错，即使指定返回特定格式，也不一定正确。

具体修改后如下：

```
@PostMapping(&quot;/ai&quot;)
ChatDataPO generationByText(@RequestParam(&quot;userInput&quot;)  String userInput) {
    String content = this.myChatClientWithSystem.prompt()
                .user(userInput)
                .call()
                .content();
    log.info(&quot;content: {}&quot;, content);
    ChatDataPO chatDataPO = ChatDataPO.builder().code(&quot;text&quot;).data(ChildData.builder().text(content).build()).build();;
    return chatDataPO;
}
```

在启动后端的spring-ai-demo项目后，我们可以复制networking目录下的URL路径。这样做是为了确保前后端项目能够同时成功启动。

![](img/c6573a35f58a2f82feee9b15e8d75f2e.png)

接下来需要对前端发送的 API 接口信息进行修改和优化。

```
send: function send(msg) {
      let sessionId = &#x27;M9u913BFyxIuTdRv7O73nx9SfkWgerhk&#x27;;

      return {
        url: &#x27;https://jtdedyc1-kbb5pits-q02qbpq08o1p.c1.mcprev.cn/ai&#x27;,
        data: {
          userInput: msg.content.text,
          // sessionId: sessionId,
        },
        method: &#x27;POST&#x27;
      };
    }
```

### 效果演示

前端项目启动完成后，我们可以继续前往演示其效果。

![](img/a7b1e6ef422c7644fdd0e1a83714ebbd.png)

### Python

我们再创建一个新的项目，这次以Python为模板，为了确保项目从一开始就符合我们的需求，我们需要对初始模板进行一些调整。首先，我们将删除原有的代码，因为它们仅是构建一个基础的Python HTTP服务项目的基础，而并非我们项目所需的具体功能。

我们要以Streamlit这一强大的工具为主，Streamlit不仅以其强大的功能和易用性著称，还能让我们以前所未有的速度构建出具有流式打字机效果的聊天应用。、

对于那些像我一样对复杂的前端代码感到畏惧的开发者来说，Streamlit无疑是一个巨大的福音。它简化了开发流程，让我们能够专注于应用的核心功能和逻辑，而无需深陷前端开发。

#### streamlit

在这次实操中，我们将跳过那些深奥的理论知识，将全部的注意力集中在实际的操作和应用上。

首先，我们需要确保基础环境已经搭建好。幸运的是，基础环境的搭建工作已经顺利完成，我们不需要再为此花费额外的时间和精力。接下来，我们需要做的就是安装一些必要的开发环境即可：

> pip install streamlit

在进行项目开发的过程中，我们常常会遇到需要从外部获取各种资源和依赖的情况。豆包IDE默认已经不再是官方源了，而是国内的仓库。、

接下来，我们将进行一个简单的测试，以验证我们的环境配置是否正确。首先，我们需要将示例代码复制到我们的开发环境中。启动后，检查是否能够正常访问，页面是否显示预期的内容。

```
import streamlit as st
import random
import time

# Streamed response emulator
def response_generator():
    response = random.choice(
        [
            &quot;Hello there! How can I assist you today?&quot;,
            &quot;Hi, human! Is there anything I can help you with?&quot;,
            &quot;Do you need help?&quot;,
        ]
    )
    for word in response.split():
        yield word + &quot; &quot;
        time.sleep(0.05)

st.title(&quot;Simple chat&quot;)

# Initialize chat history
if &quot;messages&quot; not in st.session_state:
    st.session_state.messages = []

# Display chat messages from history on app rerun
for message in st.session_state.messages:
    with st.chat_message(message[&quot;role&quot;]):
        st.markdown(message[&quot;content&quot;])

# Accept user input
if prompt := st.chat_input(&quot;What is up?&quot;):
    # Add user message to chat history
    st.session_state.messages.append({&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: prompt})
    # Display user message in chat message container
    with st.chat_message(&quot;user&quot;):
        st.markdown(prompt)

    # Display assistant response in chat message container
    with st.chat_message(&quot;assistant&quot;):
        response = st.write_stream(response_generator())
    # Add assistant response to chat history
    st.session_state.messages.append({&quot;role&quot;: &quot;assistant&quot;, &quot;content&quot;: response})
```

> streamlit run main.py

在启动Streamlit项目的过程中，我们直接选择使用命令行工具来执行，而不是依赖于图形界面的“运行”按钮。

#### 后端优化

我们之前的文章开发过程基于对接大模型的。而这一次，我们计划采用Spring-AI后台进行对接，以开发实时交互聊天功能。

首先，我们需要对代码进行相应的修改。

```
@PostMapping(&quot;/ai-stream&quot;)
Flux&lt;String&gt; generationByStream(@RequestParam(&quot;userInput&quot;) String userInput) {
    Flux&lt;String&gt; output = chatClient.prompt()
            .user(userInput)
            .stream()
            .content();
    return output;
}
```

#### 代码生成

启动成功后，我们要求AI协助生成请求代码。

这段代码拿过来后需要简单修改一下，因为我们需要走流式回答，并不是阻塞式的调用。最后代码如下：

```
def response_generator(prompt):
    url = &#x27;https://fq5jh3sk-kr95txxo-prhof0hgd4fs.c1.mcprev.cn/ai-stream&#x27;
    url += &#x27;?userInput=&#x27; + prompt
    headers = {
        &#x27;Accept&#x27;: &#x27;*/*&#x27;,
        &#x27;Content-Type&#x27;: &#x27;application/x-www-form-urlencoded&#x27;
    }

    # 发送POST请求
    response = requests.get(url, headers=headers, stream=True)

    # print(response.text)
    # 处理流式响应
    if response.status_code == 200:
        # 获取可读流
        for chunk in response.iter_content(chunk_size=1024):
            if chunk:
                # 在这里处理你的业务逻辑，例如打印或者其他操作
                yield chunk.decode(&#x27;utf-8&#x27;)  # 假设服务器返回UTF-8编码的文本
    else:
        print(f&quot;Error fetching data: {response.status_code}&quot;)
```

> tips：这里需要注意的是，请不要在后端调用其他豆包启动的项目，因为这样可能会被系统拦截。不像前端页面那样可以直接跳转，后端如果直接调用 API 是行不通的。因此，我在本地启动了一次联调。

### 演示效果

Python的实现相当简单，我们现在来看一下它的调用结果：

![](img/5da3ae098367d605cb6f66e88626644a.png)

好的，我们这期更新工作已经全部完成。原本还有一部分关于编写HTML模板的内容要完成，不过我使用了一下HTML模版总的来说，除了上文提到的一些优缺点外，目前没有其他需要特别注意的地方。因此，我决定不再进一步详细讨论HTML模板的编辑工作。

还是上面那句话，用起来是可以的，特别是在开源项目中。它能够快速实现功能，无需额外的环境配置。当然，我希望豆包IDE在未来能够进一步改进，毕竟在使用了Java、Python、React、HTML模版一整套下来，会遇到一些小问题。但这些问题并不影响个人开发的使用体验。

## 总结

豆包 MarsCode 是现代开发者的利器，其集成了强大的AI功能和云端IDE，极大地提升了开发效率和代码质量。通过支持多种主流编程语言和IDE，豆包 MarsCode 不仅简化了环境配置过程，还提供了智能代码补全、单元测试生成、问题修复等多项AI驱动的功能，使开发者能够专注于创造性工作而非繁琐的设置。

在实际使用中，我深刻体验到豆包 MarsCode 的便利。无论是快速导入GitHub仓库、自动识别项目语言模板，还是轻松配置和启动项目，它都展现了出色的稳定性和高效性。与传统的本地IDE相比，豆包 MarsCode 的云端特性更加灵活，使我能够随时随地进行开发，无需担心硬件或网络环境的限制。

特别是在代码质量的提升方面，豆包 MarsCode 的AI功能展现了其独特优势。自动代码注释、智能问题解决建议，以及对话式AI视图的应用，不仅简化了日常开发中的繁琐任务，还显著提高了代码的可读性和可维护性。这些功能不仅让我在忙碌的开发过程中节省了大量时间，还使我的开发工作更加高效和愉悦。

总的来说，豆包 MarsCode 不仅是一个工具，更是现代软件开发中不可或缺的助手。它的出现不仅推动了开发工具的技术进步，更为开发者们带来了全新的开发体验和工作方式。期待未来豆包 MarsCode 在AI驱动开发领域继续创新，为全球开发者提供更多实用且强大的功能。