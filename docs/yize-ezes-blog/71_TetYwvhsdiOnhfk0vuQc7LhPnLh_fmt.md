
# AI 产品动态的个人理解｜By 一泽Eze

> 来源：[https://chatmemo.feishu.cn/docx/EpicdTleHozrNSxDGlAc2xkVnrC](https://chatmemo.feishu.cn/docx/EpicdTleHozrNSxDGlAc2xkVnrC)

> AI 模型、产品发的越来越频繁，挑一些自己想关注的产品，简介并记录下个人理解（肯定不全对）。逼自己思考一下。

+ 欢迎使用【评论】功能，理性展开讨论，多碰撞 hh

+ 也可以【收藏】本文档，跟踪后续可能的更新

## 2025-05 w4

### 昆仑万维 Opera 发布 Agentic Browser：Opera Neon

![](img/dd8fb927a58503ca0955b9b02f7713ab.png)

![](img/1cb786bbffcf76cfca6a9f604bf13469.png)

#### 能力方面：

+ 可随时与浏览器的网页进行 AI 聊天，获取研究、写作、翻译、生图 AI 功能

+ 和浏览器功能打通，模拟用户交互操作浏览器

+ 标签页管理：可用命令关闭、分组、保存、固定标签页

+ 代理用户进行注册、下单等操作

+ AI 编程功能

目前还在内测阶段，需要在官网加入`waitlist`

> 没有产品实测，仅从官网介绍，看上去比较类似 fellou 的思路：模拟人类用户交互，代理操作浏览器。

> 好处是：
> + 与用户操作浏览器的 context 同步，用户和 Browser AI 互相接力阅读、使用
> + 利用本地浏览器的访问优势，能够共享用户登录 Cookie，无痛获取📕、飞书文档等私有站点（AI 搜索无法抓取、爬虫无法访问的）的页面信息

> 大部分能力，靠高权限的 Chrome 插件也可以做出来，AI 浏览器会开发更方便些。

>

> 目前我会把这类产品当做【浏览器版本通用 Agent + 去年比较火的浏览器 AI chat 插件 + Deep Research 产品】来理解：
> + 通用 Agent：告诉 AI 它可以操作浏览器，有 xx 功能触发指令可用（开标签页、填充密码等）
> + AI chat 插件：与网页聊天。Dia 也在做这个；Monica 算是在插件方案里做得好的
> + Deep Research：直接以用户本地浏览器，作为 DR Agent 的执行环境，进行跨页面搜索、阅读、执行，完成各类内容、代码生成

>

> 总得来说，目前更认为：
> + 这是 AI 时代面向高级白领的中间态产品，1️⃣ 要占用用户浏览器操作，2️⃣ 得用户自己拆分意图，输入操作 Prompt，并做好随时接管、协助 AI 的准备。
> + Browser Agent 的权限范围，低于拥有 linux 云电脑的通用 Agent，能做的事情也会更少
> + 和用户挤占浏览器操作是不对的；要用户随时准备接管也是不对的。

>

> 具体能实测了可以再看看。

### Ref：

+ Opera Neon官网：`https://www.operaneon.com`

+ 官方介绍视频：`https://youtu.be/ubCY1kS42yo`
