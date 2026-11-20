
# 一键开启AI编程新纪元：Windows版Trae初体验

> 来源：[https://agijuejin.feishu.cn/docx/PPtYdp9uNoqRHAxSpFoctmvvnWh](https://agijuejin.feishu.cn/docx/PPtYdp9uNoqRHAxSpFoctmvvnWh)
> 作者：有一只柴犬
> 链接：https://juejin.cn/post/7472248441454166031
> 来源：稀土掘金
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
> 如有侵权，请联系删除。

本文所使用的 Trae 免费下载链接：https://www.trae.ai

## 1、前言

继火爆全网的Cursor之后，字节也推出了Trae AI工具。但是前面只支持Mac OS版本，Windows版本于今天2025年2月17日正式发布，下班了赶紧下载体验一下。作为对标Cursor和windsurf的IDE，也是作为国产（第一款？）为中文开发者量身定制的工具，可谓是中文开发者的福音。

> 这是真正的程序员打败了程序员！

## 2、快速安装

网上开发安装教程很多，这里不做赘述，而且整体安装过程很简单，[直接下一步下一步就可以搞](https://www.trae.ai/?utm_source=juejin&utm_medium=juejin_trae&utm_campaign=AGI_article)定。下载地址：www.trae.ai/

![](img/bdde807cc138147507e8549cd2578568.png)

### 2.1、说说我的感受

整体的UI风格是我比较喜欢的，不是很生硬的风格。整个工具的字体，配色，主题接近于Vs Code风格。Trae的Windows版本在界面设计上100%复刻了macOS界面，整体风格简洁而美观。界面布局合理，功能分区明确，用户可以轻松找到所需的工具和选项。例如，代码编辑区位于中间位置，左侧是项目文件树，右侧是AI工作区，这种布局方式与VS Code相似，对于熟悉VS Code的用户来说，能够快速上手，大大降低了学习成本。

同时，Trae还提供了三种主题皮肤供用户选择，用户可以根据自己的喜好和工作环境，选择适合自己的主题：

![](img/492f660a6f9af164a6a2dfc3db8eae03.png)

讲真，左上角的logo，每次都让我感觉是一面五星红旗。瞬间感觉民族自豪感上升。

![](img/e9a903458f92b4a31411e1ec455e8845.png)

同时，支持你直接从Vs Code和Cursor直接导入配置，很多插件、配置可以直接同步过来，无需重新配置。

## 3、Chat和Builder模式

Trae拥有2中模式：Chat 模式和 Builder 模式。这两种模式各有特点，适用于不同的开发场景和需求。

### 3.1、Chat模式

> Chat 模式是 Trae 提供的一个全能 AI 伙伴，它能够以问答的形式为开发者提供各种支持。你可以随时向它提问，无论是编程问题的解答、代码仓库的解释、代码片段的生成，还是错误的修复等，它都能给出相应的回答和建议。此外，它还能理解当前项目的上下文，包括代码、文件和文件夹，从而更准确地满足开发需求。你还可以在聊天中添加图片，如错误截图、设计草图、参考样式等，以更准确、高效地表达你的需求。

开启 Chat 模式非常简单，只需使用快捷键 `Command + U` 打开侧边聊天框，然后点击聊天框左上角的 “Chat” 按钮即可。

![](img/5d022e1bb83672e710df957942fedd5d.png)

这里是因为我前面已经设定好了他的角色了。因此这里他回答的是我前面预先设定的信息。

不过，Chat 模式也有一定的局限性。由于它是基于问答的形式，对于一些复杂的开发任务，可能无法一次性给出完整的解决方案，需要你多次提问和交互才能逐步解决问题。此外，对于一些需要对代码进行大规模修改或重构的任务，Chat 模式可能无法直接操作代码文件，需要你手动进行修改。

### 3.2、Builder模式

Builder支持一键从无到有构建项目结构，编写项目代码等，实现高效项目构建与智能代码操作。Builder 模式是 Trae 的核心功能之一，它可以帮助你从零开始开发一个完整的项目，并无缝融入到你的项目开发流程中。在 Builder 模式下，AI 助手会根据你的需求调用各种工具，如分析代码文件、编辑代码文件、运行命令等，从而让回答更精确、更有效。它会主动读取当前项目文件的内容，把任务分解成小步骤，然后一步一步执行，包括提取相关的上下文、创建或修改文件、生成和运行命令、分析命令执行的状态等。

这里我使用Builder模式，让他为我生成一个小程序页面。并通过我手画的草稿图设计相应的页面。

我的草稿图，简单生成一个注册的表单页面：

![](img/b89bdc667b89480d1ed2fc77ca79125a.png)

打开Builder模式，并给他预先设定角色信息：

> 1.你是一个经验丰富的🛰️小程序UI工程师，熟悉🛰️的UI设计，设计风格简约明朗
> 2.你将负责设计🛰️小程序的UI
> 3.我会给你一个设计图，你需要解析这个图片，并设计生成一个小程序，实现这个页面功能。

![](img/ec2c6192166c0d4b8e9d8e8ef0dc4c23.png)

这里我将图片上传给他：

![](img/cbf3575283b2894f3b4b3ae3299054c6.png)

Trae的思考过程以及回复：

![](img/2fe39834b1ee57591dff70f99fbcac92.png)

由于这里是在我已有的工程结构里面，因此他对我原有的页面进行了部分修改。修改结束后，我们可以点全部接受来应用所有的修改到我们的代码中。修改完，我们打开Wechat工具看下编写好的页面效果：

```
&lt;view class=&quot;container&quot;&gt;
  &lt;view class=&quot;form-section&quot;&gt;
    &lt;view class=&quot;form-item&quot;&gt;
      &lt;text class=&quot;label&quot;&gt;姓名：&lt;/text&gt;
      &lt;input class=&quot;input&quot; type=&quot;text&quot; placeholder=&quot;请输入姓名&quot; bindinput=&quot;handleNameInput&quot;/&gt;
    &lt;/view&gt;
    
    &lt;view class=&quot;form-item&quot;&gt;
      &lt;text class=&quot;label&quot;&gt;手机：&lt;/text&gt;
      &lt;input class=&quot;input&quot; type=&quot;number&quot; placeholder=&quot;请输入手机号码&quot; bindinput=&quot;handlePhoneInput&quot;/&gt;
    &lt;/view&gt;
    
    &lt;view class=&quot;form-item&quot;&gt;
      &lt;text class=&quot;label&quot;&gt;验证码：&lt;/text&gt;
      &lt;view class=&quot;verify-code-container&quot;&gt;
        &lt;input class=&quot;input verify-input&quot; type=&quot;number&quot; placeholder=&quot;请输入验证码&quot; bindinput=&quot;handleCodeInput&quot;/&gt;
        &lt;button class=&quot;verify-btn&quot; bindtap=&quot;getVerifyCode&quot;&gt;获取验证码&lt;/button&gt;
      &lt;/view&gt;
    &lt;/view&gt;
    
    &lt;view class=&quot;form-item&quot;&gt;
      &lt;text class=&quot;label&quot;&gt;备注：&lt;/text&gt;
      &lt;textarea class=&quot;textarea&quot; placeholder=&quot;请输入备注信息&quot; bindinput=&quot;handleRemarkInput&quot;&gt;&lt;/textarea&gt;
    &lt;/view&gt;
  &lt;/view&gt;
  
  &lt;view class=&quot;note-section&quot;&gt;
    &lt;text class=&quot;note-text&quot;&gt;注明&lt;/text&gt;
  &lt;/view&gt;
  
  &lt;button class=&quot;submit-btn&quot; bindtap=&quot;handleSubmit&quot;&gt;注册&lt;/button&gt;
&lt;/view&gt;
```

效果图：

![](img/883393e6e62bfb6dd820a6f76ed28992.png)

基本效果大差不差。写到这，虎躯一颤：眼看就要失业了。

> 在项目开发过程中，Builder 模式还提供了预览功能，当 AI 助手完成你的需求后，会提供一个预览按钮，点击后可以在 Trae 中打开一个 `Webview` 窗口来展示结果，而且在预览过程中，如果你继续和 AI 助手交互并修改需求，结果会在 `Webview` 窗口中实时更新。此外，它还支持回退到之前的版本，你可以把项目回退到某次聊天之前的版本，方便你在开发过程中随时调整和修改。

### 3.3、Chat 模式与 Builder 模式对比

这里简单Chat模式和Builder模式做个对比，你就猜是不是用给Trae帮我生成的？

![](img/e03ef99aadc2e3d18ac488f7a311f79e.png)

## 4、总结

初步体验完，给我的感觉还是比较惊艳的。网上有人说Cursor的Composer项目构建能力比较强，但是Trae免费啊。有人说RooCode插件也能支持代码一键生成，但是Trae免费啊。免费的国产工具，真的很香！对于Trae我只想说：未来可期，让子弹再飞一会~

本文所使用的 Trae 免费下载链接：https://www.trae.ai
