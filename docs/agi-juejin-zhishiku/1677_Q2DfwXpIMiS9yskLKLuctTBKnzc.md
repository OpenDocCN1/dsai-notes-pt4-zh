
# 0门槛教你用天工AI免费制作小游戏（音乐、动画、代码一站式搞定）

> 来源：[https://agijuejin.feishu.cn/docx/BWm9do72loY5tMxcYXlc5W4QnWE](https://agijuejin.feishu.cn/docx/BWm9do72loY5tMxcYXlc5W4QnWE)

> 作者：Aitrainee AI进修生

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/M1WimFCpHWCsA4GtR0_mxA)qq.com/s/M1WimFCp...

音乐、动画、代码一站式搞定。

![](img/b8eafd1eedcd9fa7e750bb3de3ec5ca3.png)

[0b2eiuaaeaaaruahqf4gfftfarodajcqaaqa.f10002.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Iv9Lb0sgxo1zjexeBi8czcG3nkc)

注：以上音乐、代码、动画均由天工AI生成

Hi，这里是Aitrainee，欢迎阅读本期新文章。

如果你有关注的话，实际上我已经写了好几篇有关于 文本到应用程序 的文章了，其中有用到`Aider`以及`LLM API`这些操作，不过对于无专业知识的读者不太友好。

那么，今天这一篇让我们尝试使用更简单易上手的AI工具——天工AI来进行 文本到游戏 这一实践。

我们主要会用到天工AI的 代码生成+音乐生成+图像生成 去 制作一个跑酷小游戏（ 效果 如上 视频 ），除此以外，文末还有一个农场小游戏，也是用 天工 AI 制作的。

> 🌟 视频中的演示，障碍物还过于单一，但是我们主要想讲AI生成代码、音乐、动画的整个框架过程。并且整个过程尽量都使用提示词直接修改代码而无需人为修增功能。

下面就把制作教程保姆级呈现给大家！

## 天工AI使用入口

网页端：`www.tiangong.cn`

APP端：应用商店搜索“天工”或“天工AI智能助手”

小程序：

![](img/ac2835db4976b222b9eb26c7de12d4f4.png)

## 制作跑酷小游戏

### 第一步：输入游戏需求提示词

首先 找到【编程助理】智能体，将 游戏需求提示词 输给天工AI编程助理

你或许可能会问 为什么提示词这么复杂，能不能简单点，实际上，如果是一段简单的提示词的话我实践中发现生成的代码，要么过于简单没有完成需求，要么存在bug。

你可能会问这个提示词是 怎么得到的，实际上这段提示词也是通过和天工AI对话得到的，首先告诉他，采用什么实现方式，需要完成哪些功能，迭代几次就有了现在这个提示词。

天工AI输出三个代码文件（滚动截屏不了，就排列截个屏吧，代码文件放到最后）

### 第二步：在IDE中创建文件并运行

在IDE中创建这三个代码的文件

点击`index.html`

到此我们就得到一个最初级的版本如下：

游戏原功能是按`空格` 键跳跃的，视频中无响应，并且方块也不太美观。

### 第三步：修复BUG并美化主角

所以我们把想法告诉天工AI：

根据他的回答，这个bug确实修复好了；我还想美化一下游戏的主人公，

先让他修改原始代码将方块替换成人物图标：

然后我们利用天工AI图片生成，制作游戏主角：

把背景抠一下，保留人物，用下面这个抠像网站：

除了主人公之外，再做一个障碍物的：

都扣好了

把上面两张扣好的图片，根据他给的代码结构扔到这个文件夹里（后面我又扣了一个月亮）：

### 第四步：添加背景音乐

有了图片，然后准备给游戏再创作一首背景音乐：

先修改代码：

然后开始制作音乐，先和他对话，生成歌名和歌词：

```
歌名：Epic,triumphant background music
歌词：Racing against time, racing against time,
Everysecond, every climb.
Racingagainst time, racing against time,
Hearthe heartbeat, hear the chime.
Pushahead, don&#x27;t look back,
Speedand focus, on the track.
Pushahead, feel the rush,
Everymoment, feel the crush.
```

再挑选参考的音频，莫扎特这个不错：

然后生成音乐

成品如下：

把图片和音乐加进去之后，我们来看看最后的效果

关闭

观看更多

更多

退出全屏

切换到横屏模式

[🎦 点击观看视频](https://mpvideo.qpic.cn/0b2eiuaaeaaaruahqf4gfftfarodajcqaaqa.f10002.mp4?dis_k=b11fa1bb8d239e1dfc7911e0fc218e6d&dis_t=1724902714&play_scene=10120&auth_info=db2I+/0HFAdloKCFzSRMAjhAWUV8JWENOURCbQd+PThMQjE0VghJaAdmORFiBVs4Og==&auth_key=abb9958dd5477c396a6239d72e24ae38&vid=wxv_3531553933074825221&format_id=10002&support_redirect=0&mmversion=false)

> 无法获取音频链接

视频详情

背景从原来白的 优化了一下变成这样点状的黑色（需要改什么，实际上告诉他就行），比如后面增加了分数、月亮以及角色跳动会有 “咚” 的一下的音乐，但是在一些改动的过程中，可能需要多次迭代修复bug。

## 制作农场小游戏

后续用同样的方式创建了一个小农场的小游戏

![](img/89f4d41b3012d162b0faac2fafd17cd2.png)

![](img/b64d57430995d588e50d8e51998b2173.png)

[0b2ehqaaeaaaqeahk64gfftfapgdai6aaaqa.f10002.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PdPAb24x4oIE1Exdop2c7kCHnIg)

## 总结

总的来说，利用天工AI迭代修改游戏程序还是比较方便的，并且在同一个平台上可以同时进行代码、动画和音乐的制作也是天工的一个独家优势，行业内AI产品也独此一家。

不过需要注意的是整个过程中，一定程度上还是比较依赖于提示词的使用，同时有些 bug或需求可能需要多次迭代才能解决。

比如障碍物过于单一的问题，我尝试了几次通过提示词`Debug`，但AI没有很好的解决，索性我放弃了，毕竟这是考验AI一次性生成并且成功的能力，而非我自己来写代码。

如果你们对我用AI创建完整应用程序感兴趣，请到AI社群交流~

此外，如有兴趣，下面这些实操文章也很有意思：

[`Aider` + `DeepSeek-Coder-V2` ：使用 AI 生成星球大战游戏（`Ollama`）](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247490297&idx=1&sn=76e03f11b185793cbf229d18ddbe49fd&chksm=c1e09739f6971e2ff84ab80bfa2aaea5a2168c4c8b9b0dd5c34249b7b18896699407f1974ace&scene=21#wechat_redirect)

[2024-07-14](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247490297&idx=1&sn=76e03f11b185793cbf229d18ddbe49fd&chksm=c1e09739f6971e2ff84ab80bfa2aaea5a2168c4c8b9b0dd5c34249b7b18896699407f1974ace&scene=21#wechat_redirect)

![](img/36c4f37f7b9423deeaaed4570240d71b.png)

[`Aider` + `Claude 3.5 Sonnet`：使用 AI 生成全栈应用程序（`NextJS` + `Supabase`）](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247490221&idx=1&sn=4137aa182028ed9ccf31378902a39090&chksm=c1e0976df6971e7b91c2b9e3d9a001c2c24a611117cc63cf54d77cb82607db18c9e94845d330&scene=21#wechat_redirect)

[2024-07-13](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247490221&idx=1&sn=4137aa182028ed9ccf31378902a39090&chksm=c1e0976df6971e7b91c2b9e3d9a001c2c24a611117cc63cf54d77cb82607db18c9e94845d330&scene=21#wechat_redirect)

![](img/c09668a24197e07f99dfd9ae2043f004.png)

[`Devyan` (`CrewAI`) + `DeepSeek-Coder-V2`：几秒钟生成一个应用程序（本地、快速、一次提示）](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247490127&idx=1&sn=320857ca7559edbc6c981f74ccb38ea8&chksm=c1e0978ff6971e9970c545fbe8ba8115ed57148d6b5084d0f787d77fbaf0af3bd33efafcaaea&scene=21#wechat_redirect)

[2024-07-12](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247490127&idx=1&sn=320857ca7559edbc6c981f74ccb38ea8&chksm=c1e0978ff6971e9970c545fbe8ba8115ed57148d6b5084d0f787d77fbaf0af3bd33efafcaaea&scene=21#wechat_redirect)

![](img/81fc3487b67df99aae0f3b816de4f900.png)

[`Plandex` + `DeepSeek-Coder-V2`：使用一个提示创建应用程序（基于`Agents` 和 `Ollama`）](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247489954&idx=2&sn=ae136c02b4cb60942084d38ffb0b1aeb&chksm=c1e09462f6971d74e343779bbfdc01165a80a8814865513410e012bdc138566641f3af2833ad&scene=21#wechat_redirect)

[2024-07-09](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247489954&idx=2&sn=ae136c02b4cb60942084d38ffb0b1aeb&chksm=c1e09462f6971d74e343779bbfdc01165a80a8814865513410e012bdc138566641f3af2833ad&scene=21#wechat_redirect)

![](img/3fb5912945d96cbda2159715eb70e657.png)

[`Aider` + `Claude` + `Invoke` + `Udio` ：从头到尾使用 AI 创建游戏！（音乐、代码、动画）](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247489838&idx=1&sn=c8354773a3bffeddeeecd3f2f95beb2d&chksm=c1e094eef6971df85cf44949630447c0c490c854719674ce0a2ee267565b5ef98f4f132230f4&scene=21#wechat_redirect)

[2024-07-07](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247489838&idx=1&sn=c8354773a3bffeddeeecd3f2f95beb2d&chksm=c1e094eef6971df85cf44949630447c0c490c854719674ce0a2ee267565b5ef98f4f132230f4&scene=21#wechat_redirect)

![](img/909b7a5ec08ba3ca809f5bce3f1ef738.png)

[`Aider` 更新：终端命令行+`AI`、最好的个人 AI 编码助手、连接到几乎任何 `LLM`（`Ollama`等）！](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247489838&idx=2&sn=48ef0d82089e9891d4f679f74cf61676&chksm=c1e094eef6971df8c20b62c85558efbbf8c6a87bbe36aacd4b843cb26e67528168a7de39f6ad&scene=21#wechat_redirect)

[2024-07-07](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247489838&idx=2&sn=48ef0d82089e9891d4f679f74cf61676&chksm=c1e094eef6971df8c20b62c85558efbbf8c6a87bbe36aacd4b843cb26e67528168a7de39f6ad&scene=21#wechat_redirect)

![](img/0fe54ad611db7042496d0365b11b2628.png)

希望这篇文章对你有帮助，感谢阅读！

## 往期推荐

[AgentStudio ：联合国际顶尖高校 昆仑万维开源智能体研发工具包，从0到1，轻松构建Agent](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247489164&idx=1&sn=487a4480e41eddaf61b1b7848d9328c8&chksm=c1e09b4cf697125aea4ffb4130f15a46b7217ba07bbef9b74b9126fbe7676e4a38d7a8dc2d70&scene=21#wechat_redirect)

`OpenAI`封锁中国`API`？天工一键搬家`API`实操指南



# [Vitron：颜水成团队开源通用视觉多模态大模型](http://mp.weixin.qq.com/s?__biz=MzkyMzY1NTM0Mw==&mid=2247488930&idx=1&sn=14f5ba323b8bf36c4dc68a329e110c9c&chksm=c1e09862f6971174b06607476ede268ed91c20b1bb57ccc00ded29ae023ecf08c35975ca344c&scene=21#wechat_redirect)
像素级智能、一统理解/生成/分割/编辑

## 微调LLMs：概述、方法和最佳实践（附天工Skywork-13B微调）

代码：https://pan.baidu.com/s/1YFg9easS0yaT8uWvaud4WA?pwd=1234

## 知音难求，自我修炼亦艰

抓住前沿技术的机遇，与我们一起成为创新的超级个体

## （把握AIGC时代的个人力量）

![](img/9c3ff3b41c6b8b6f59069a1382e890db.png)

## 点这里👇关注我，记得标星哦～

一键三连「分享」、「点赞」和「在看」

## 科技前沿进展日日相见 ~
