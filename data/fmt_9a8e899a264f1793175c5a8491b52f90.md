
# 如何用Trae打造一键登录神器？Chrome插件开发实战

> 来源：[https://agijuejin.feishu.cn/docx/LPAOdeuXqomWICx8Sf1cniSlnjb](https://agijuejin.feishu.cn/docx/LPAOdeuXqomWICx8Sf1cniSlnjb)
> 作者：Nickyang
> 链接：https://juejin.cn/post/7461993842503778342
> 来源：稀土掘金
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
> 如有侵权，请联系删除。

字节跳动昨天推出了一款针对中文开发者的 AI 集成开发环境（IDE）—— Trae ， 它是对标 Cursor 和 Windsurf全新的IDE ，支持AI问答、代码自动补全、基于Agent的AI编程等功能，可以帮助程序员自动化完成开发任务，但是目前仅支持macOS操作系统，但据相关人员介绍，Windows版本也即将上线。该工具提供简体中文和英文界面，内置了GPT-4o、Claude-3.5-Sonnet等模型供用户免费使用；

![](img/7654e36d81bd6b064d62d2e4a837a1de.png)

下载地址：`https://www.trae.ai/`

因为公司业务的原因（好几个系统，每个系统登录界面基本一致，每次登录的时候都要填账号和密码登录太麻烦了...）, 后面就想着开发个自动登录的插件，本来想着用 Cursor 来开发的, 后面想用的时候就收费了，计划就搁浅了。。。

昨天，刷其他文章的时候发现字节新出了一款类似的AI产品,就是我现在要讲的 Trae ，然后我马上去下载体验了一下，然后发现真的不错!后面也是将这个开发出来了，给大家看看大致功能页面：

![](img/888d62df93ae418aa7b775eeaa0f7c07.png)

下面我详细说一下我是怎么一步步开发的

## 一：大致描述一下你要做什么

比如说我的是：帮我写一个自动登录的google 插件； 它会创建google所需用到的文件、命令都给你，你就 应用 执行这些命令就行了

![](img/c7a396448082ccdd3e9a28faef2ec895.png)

![](img/048333260bfbf10c7e4f816b2e0108b3.png)

到这一步基本没啥问题，我将它打包加载在google浏览器中也能正常运行，不的不称赞一下： 牛🐂！

## 二：将要开发的一步步具体化

![](img/54f74e14303e654679decfff68bcdaef.png)

![](img/b55060a05dd7efdde5ad7661417ab42d.png)

### 错误处理

![](img/bf41c8b48857600f122919f4bb875886.png)

这里只截取部分截图展示，我也是按照这个步骤慢慢去调整代码的，到这里其实就一步步引导AI需要做什么就行了

## 三：结尾

不得不说，现在的 AI 实在是太强大，最近看的和体验的这几个AI工具：Cursor，Windsurf,Vscode+cline，还有就是现在的 Trae，还是 Trae 给我的体验是最好的(个人观点), 趁着现在还免费，赶紧用起来，感觉后期还是会收费的；

最后附上源码（仅供参考），大家可以在此基础上优化，帮忙点个赞 ）：`github.com/lance-Yang/…`
