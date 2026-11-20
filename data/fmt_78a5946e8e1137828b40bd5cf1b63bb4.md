
# MicroAgent：这个AI智能体一键写代码并且自动测试！它比 Aider 更好吗？（Ollama）

> 来源：[https://agijuejin.feishu.cn/docx/QvBgd7k8HoaUcbxW3BccIhUqndh](https://agijuejin.feishu.cn/docx/QvBgd7k8HoaUcbxW3BccIhUqndh)
> 作者：Aitrainee | AI进修生
> 原文：[https://mp.weixin.qq.com/s/zEiksOEvT-e8nqoVK6xVOw](https://mp.weixin.qq.com/s/zEiksOEvT-e8nqoVK6xVOw)
> 🌟 `MicroAgent` 是一种 AI 编码代理或文本到应用程序工具，可以生成代码并实时测试生成的代码。不过，它的方法存在一些问题，我将在本文中讨论这些问题。

`MicroAgent` 还可以与任何开源 LLM、`OpenAI` 模型或其他 `Claude` 模型一起使用，例如 `GPT-4O`、`Claude-3`、`CodeQwen`、`Mixtral 8x22b`、`Mixtral 8x7b`、`GPT-4`、`Grok-1.5` 和 `Gemini Code Assist`。

`https://github.com/BuilderIO/micro-agent`

Hi，这里是Aitrainee，欢迎阅读本期新文章。

## 背景与问题

一般来说，我们目前看到的文本到应用程序软件可以相当不错地生成应用程序，尽管它们大多数有一个问题，那就是生成的代码并不总是可靠的。

这里的可靠性是指代码是否能在首次运行时正确工作。这是我们大多数编码代理面临的一个重大问题。

例如，使用像`Aider`这样的工具：

![](img/6b0c71b1385ea02774528b963f3ce90b.png)

如果它生成了错误的代码，你需要来回与`Aider`交流，解决发生的错误等等。

![](img/8b83963f78e8d439dc6b1a74dd2c1af8.png)

这会成为一种麻烦，因为你无法依赖生成的代码，除非你自己进行了测试。

但是，如果我们使用的编码代理不仅能生成代码，还能为脚本编写测试，并运行测试以检查其是否正常工作，如果不正常，它还能修复呢？

这就是我今天要介绍的内容——`MicroAgent`。

![](img/a18fc5ebe156c7557a574ae5f53bb403.png)

## MicroAgent 工作原理

`MicroAgent`是一种与传统编码代理方法不同的编码代理。

![](img/705b48c2f8e74a44d871cdedd2951a8e.png)

通常，常规的编码代理生成代码，完全依赖用户检查其是否正常工作。但`MicroAgent`首先生成代码，然后为代码生成测试，然后运行测试并检查是否通过。

![](img/d4bbd158294b1351a48ddaff76cbd4e2.png)

如果代码通过了测试，则任务成功；如果没有，则将错误反馈给代理，代理修复代码，然后再次通过测试。这会递归进行，直到所有测试用例通过。

他们说，`MicroAgent`的理念是创建一个明确的测试用例，可以清楚地反馈代码是否按预期工作，并对代码进行迭代，直到所有测试用例通过。

![](img/a93348e888da7fc1c7559e831072aa21.png)

它支持`OpenAI`的`Claude`，并允许更改`OpenAI`模型端点，从而支持使用`Ollama`和`Groq`。

![](img/d10d04bf46449234d2d4e3a9f12401ae.png)

## 快速开始

因此，我将向你展示如何使用`OpenAI`和`Ollama`进行设置。让我们开始并检查一下。安装非常简单，你只需运行这个`npm`安装命令，它就会被安装。

![](img/7c9c1afb70d5aae91fc5d6aec53e821a.png)

一旦安装完成，你可以使用`micro agent`命令来使用它。但在此之前，我们需要先设置`API`密钥和模型。

我会先向你展示如何使用`OpenAI`，然后再展示如何使用`AMA`。只需运行这个`config set openAI key`命令，并输入你的`API`密钥。一旦完成，运行这个`set model`命令并将模型设置为`GPT-4o`。

![](img/ebbde62616f62175e4b026e5265a233d.png)

一旦完成，我们可以开始使用它了。你可以导航到任何之前的项目，或者创建一个新目录并在其中使用它。

我将会在一个新目录中使用它。进入目录后，只需运行`micro agent`命令。如果你在一个空目录中运行它，它会要求你创建一个`node`项目。

![](img/879f8163362108b1efe7cb2571bb2b08.png)

![](img/2e63be1e2f44e5ab71efabb8b97142ad.png)

好，现在完成了，让我们再次运行`micro agent`命令。现在你可以使用它了。让我们让它生成一个简单的程序来添加两个数字。

当你输入一个提示时，它会告诉你为文件命名，或者如果你不想命名，它也会为文件生成一个名称。你只需按下回车键即可使用生成的名称。

![](img/e6d5e7fdc3d5d53e79477c873d8e22af.png)

现在可以看到，它正在生成。它首先生成代码。一旦生成代码，它就会生成测试。

![](img/9930de3ade729e1cb8ce1f8d05f808fe.png)

可以看到，现在代码生成了，测试也生成了。它在询问生成的测试是否正确。 你需要发送`good`提示来告诉它测试是正确的，然后它会运行测试并检查是否正常工作，并在测试未通过时进行迭代修复。

![](img/c4c6b01c7699b96c46f3b5b67fac2002.png)

好了，任务完成了，

现在让我们看看代码。

![](img/1a824e39c84f6584e3ae9fa9fdfe76f7.png)

这看起来相当不错，程序是正确的，应该能正常工作。

![](img/b7727206679f0460820fafc1b9324102.png)

测试也创建得很好，这也很棒。

## 实测体验

但让我们也尝试用它做一些有用的事情。为此，让我们用`create next app`命令创建一个`next`应用程序。

![](img/90396cd6dcb2ec0bbf32288005858438.png)

现在让我们在这里启动`micro agent`。让我们让它创建一个登录页面，并告诉它这是一个`nextjs`应用程序。

![](img/3a31dbce60d9bf7962351fa5e50fb952.png)

好的，它正在生成代码、测试和所有相关内容。稍等一下。

好，经过多次尝试，测试未通过。

![](img/778c7e10e2cf3a659e725c6667f796c7.png)

据我所知，如果AI生成的代码可能有问题，那么AI生成的测试也可能有问题，而且它没有任何解决办法。

所以，如果生成的测试有问题，它将无法正常工作。而且，如果测试多次失败，它也不会尝试修复测试脚本，这意味着如果第一次测试脚本不正确，它将一直卡在循环中，直到最终退出。

尽管如此，生成的代码仍然会放入文件夹中，所以你可以查看它。生成的页面看起来像这样，非常基础，感觉像是回到了90年代。

![](img/22473d280419f3fc96c0d58867d3819e.png)

但我认为这是一个有缺陷的系统设计，因为它最终回到了AI生成代码的问题所在。我还尝试创建另一个只有一个标题标签的简单页面，它的测试也失败了。

![](img/99edb0f003cdc36511749813c53c63f6.png)

我认为这个页面根本不需要测试。所以，我认为它可能在功能性脚本和类似有最终输出的东西方面表现不错，但除此之外就不行了。

## 使用 Ollama 设置

现在让我也告诉你如何使用`Ollama`进行设置。首先，访问`Alama`网站，点击下载按钮，选择你的操作系统并进行安装。

![](img/67d999190093bd9611dab9e326a72442.png)

安装完成后，转到模型页面，选择`Deep Seek Coder V2`模型。点击它并复制安装命令，然后将其粘贴到终端并安装。安装完成后，发送一条消息并检查是否正常工作。

![](img/1e4e72bfa621f7f4487cc59aa292a88a.png)

现在我们需要将`Alama`的`API`基地址更改为本地主机`localhost`和端口`11434`，就像这样，这将指向`Alama`。还要用这个命令将模型更改为`Deep Seek Coder`。

![](img/b18835aa8c70ecbf0620ae361ad400b0.png)

一旦完成，你现在可以与`Ollam`一起使用它了，它也能正常工作。

![](img/439ab0b27ee82be082519fd1ec7b193c.png)

你还可以通过这个选项将其与其他兼容`OpenAI`的平台一起使用。

## 总结

总体而言，我会说，如果你通常进行功能性编程并使用测试，这是一个不错的选择。

我不使用测试，因为我认为你只能活一次，我直接`SSH`进入服务器并直接编辑代码。但我认为现在人们正在使用测试，所以对于那些使用测试的人来说，它可能是有益的。

## 相关文章

[`Aider` + `DeepSeek-Coder-V2` ：使用](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247490297&idx=1&sn=76e03f11b185793cbf229d18ddbe49fd&chksm=c1e09739f6971e2ff84ab80bfa2aaea5a2168c4c8b9b0dd5c34249b7b18896699407f1974ace&scene=21#wechat_redirect) AI 生成星球大战游戏（`Ollama`）

[2024-07-14](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247490297&idx=1&sn=76e03f11b185793cbf229d18ddbe49fd&chksm=c1e09739f6971e2ff84ab80bfa2aaea5a2168c4c8b9b0dd5c34249b7b18896699407f1974ace&scene=21#wechat_redirect)

![](img/57b952614f0743679e3e7ade9e0055f7.png)

[`Aider` + `Claude 3.5 Sonnet`：使用 AI 生成全栈应用程序（`NextJS` + `Supabase`](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247490221&idx=1&sn=4137aa182028ed9ccf31378902a39090&chksm=c1e0976df6971e7b91c2b9e3d9a001c2c24a611117cc63cf54d77cb82607db18c9e94845d330&scene=21#wechat_redirect)

[2024-07-13](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247490221&idx=1&sn=4137aa182028ed9ccf31378902a39090&chksm=c1e0976df6971e7b91c2b9e3d9a001c2c24a611117cc63cf54d77cb82607db18c9e94845d330&scene=21#wechat_redirect)

![](img/e187cc72739d1539f61fc701f6d7c6b1.png)

[`Devyan`（`CrewAI`）+ `DeepSeek-Coder-V2`：几秒钟生成一个应用程序（本地、快速、一次提示）](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247490127&idx=1&sn=320857ca7559edbc6c981f74ccb38ea8&chksm=c1e0978ff6971e9970c545fbe8ba8115ed57148d6b5084d0f787d77fbaf0af3bd33efafcaaea&scene=21#wechat_redirect)

[2024-07-12](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247490127&idx=1&sn=320857ca7559edbc6c981f74ccb38ea8&chksm=c1e0978ff6971e9970c545fbe8ba8115ed57148d6b5084d0f787d77fbaf0af3bd33efafcaaea&scene=21#wechat_redirect)

![](img/57d6b9e09c3bd7c5c495bd757c0c903f.png)

[`Plandex` + `DeepSeek-Coder-V2`：使用一个提示创建应用程序（基于`Agents` 和 `Ollama`](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247489954&idx=2&sn=ae136c02b4cb60942084d38ffb0b1aeb&chksm=c1e09462f6971d74e343779bbfdc01165a80a8814865513410e012bdc138566641f3af2833ad&scene=21#wechat_redirect)

[2024-07-09](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247489954&idx=2&sn=ae136c02b4cb60942084d38ffb0b1aeb&chksm=c1e09462f6971d74e343779bbfdc01165a80a8814865513410e012bdc138566641f3af2833ad&scene=21#wechat_redirect)

![](img/d6e7fb1f5640ff6582b202bd5f9c4266.png)

[`Aider` + `Claude` + `Invoke` + `Udio` ：从头到尾使用 AI 创建游戏！（音乐、代码、动画）](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247489838&idx=1&sn=c8354773a3bffeddeeecd3f2f95beb2d&chksm=c1e094eef6971df85cf44949630447c0c490c854719674ce0a2ee267565b5ef98f4f132230f4&scene=21#wechat_redirect)

[2024-07-07](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247489838&idx=1&sn=c8354773a3bffeddeeecd3f2f95beb2d&chksm=c1e094eef6971df85cf44949630447c0c490c854719674ce0a2ee267565b5ef98f4f132230f4&scene=21#wechat_redirect)

![](img/4f05ee911b643cd7ad99dc6bad9ee16e.png)

希望这篇文章对你有帮助，感谢阅读！

视频教程

`https://www.youtube.com/watch?v=w5of-CBDlfU`

参考链接：

[1] `https://github.com/BuilderIO/micro-agent`

知音难求，自我修炼亦艰

抓住前沿技术的机遇，与我们一起成为创新的超级个体

（把握AIGC时代的个人力量）

![](img/8d03adb6702726f8632a3b413372baf4.png)
