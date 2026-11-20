
# 这个神器让你白嫖Claude Opus 4.5，Gemini 3！还能接Claude Code等任意平台

> 来源：[https://my.feishu.cn/docx/Buldd4SDoo1uPwx1MBkcwZBMnTf](https://my.feishu.cn/docx/Buldd4SDoo1uPwx1MBkcwZBMnTf)

昨晚刷GitHub，老金我又挖到宝了。

看完这个项目，只有一个字：

**绝！**

![](img/768e84adc26b054ae0b7ea4ed3f863bf.png)

## Claude Code是真的香，但也是真的贵啊

用过Claude Code的都懂。

补全嗖嗖的，上下文贼准。

但钱包疼啊。

老金上周算了算。

写一天代码，Token消耗破百万。

一个月下来，几百刀没了。

钱包直接空了还写个屁代码？

## 谷歌出手了

11月份，谷歌搞了个大新闻。

推出Antigravity IDE。

安装吃点环境，教程老金写过：

https://ai.feishu.cn/wiki/UN4swf758ibgtHk5jyAcXuX3n6a

![](img/58779203e8bd35e3ca218975c221bc83.png)

免费送什么？

Gemini 3 Pro、Claude Sonnet 4.5、还有Claude Opus 4.5。

对，Opus 4.5也是白嫖的。

有速率限制？有。

够用吗？够！

老金测了一周，愣是没触发过。

这不就是赛博菩萨吗？

## 嫁接神器来了

但Antigravity有个问题。

界面嘛，和Claude Code比起来，差点儿意思。

然后GitHub上一哥们lbjlaq出手了。

搞了个项目叫Antigravity-Manager。

思路贼简单：

Claude Code界面好用但贵。

Antigravity免费但界面一般。

**那就把它俩接一块儿不就完了？**

说白了就是个中间人。

让Claude Code以为连的官方。

实际上呢？用的Antigravity免费额度。

偷梁换柱，懂了吧？

技术宅可以瞅瞅原理。

这货是个本地网关。

把Antigravity的session转成API。

Claude Code连本地就行。

牛逼在哪？

OpenAI、Anthropic、Gemini三种协议全支持。

智能路由，模型自己匹配。

限流了自动换号重试。

数据全存本地，加密的，不怕泄露。

Tauri v2 + React + Rust写的。

性能杠杠的。

## 配置教程

### 装软件

macOS用`brew`装：

```
brew install --cask antigravity
```

Windows下载`msi`安装包。

地址：https://github.com/lbjlaq/Antigravity-Manager/releases

### 启动服务

打开软件，找"API反代"那个页面。

点一下开启，端口默认8045。

![](img/33b4221401e714ce4111f2a40b7410c4.png)

### 改环境变量

重点来了，让`Claude Code`连咱本地：

```
export ANTHROPIC_API_KEY="sk-antigravity"
export ANTHROPIC_BASE_URL="http://127.0.0.1:8045"
claude
```

完事儿！

现在你用`Claude Code`，后台消耗的是Antigravity额度。

**关键是，免费的。**

当然你也可以用老金教过的CC Switch增加，填写区域如下图，之前整篇教学文章可以看这：

https://ai.feishu.cn/wiki/TZCvwXx9WivgJLkPRFlcqk0zn3d

![](img/2892a32d52784eac236d0b585bc448e5.png)

## 几个坑提前说

### 额度的事儿

免费版额度每周刷一次。

Pro用户5小时刷一次，量更大。

学生党注意了：

去申请Google AI Pro学生计划。

SheerID验证通过，白嫖12个月Pro会员。

### 风险的事儿

这种反代操作，可能违反服务条款。

项目README写了"Use at your own risk"。

老金的解释：自己玩没事，商用大规模薅别作死。

### Thinking模式不兼容

当你遇到下方的报错

![](img/fd16d7ca477907ac28049922adb2551d.png)

这个报错是说Antigravity的API代理不支持Extended Thinking模式。

如果你用的是Opus 4.5（带Extended Thinking），会报错。

因为代理转发的模型不支持thinking block格式。

解决办法：启动时指定模型即可。

然后直接使用就行了，这个文章的就是这么写出来的，一分钱没花。

![](img/6f98cdb8340c28d96c3ead095d59f0c3.png)

声明：本文仅供学习研究使用，请勿用于商业用途。

## 还有几个类似工具

### Anti-API

也是把Antigravity转成API的。

支持Cherry Studio、ChatWise。

地址：https://github.com/ink1ing/anti-api

### CLIProxyAPI

统一代理，支持好几个CLI。

`Gemini CLI`、`Antigravity`、`Qwen Code`都行。

地址：https://github.com/router-for-me/CLIProxyAPI

老金都试过，Antigravity-Manager最好用。

先用这个准没错。

## 老金说两句

谷歌这波属实大方。

Opus 4.5、Gemini 3 Pro，白送。

Antigravity-Manager把福利和Claude Code接上了。

免费额度 + 顶级界面，白嫖党狂喜。

但老金得说句实话：

自己玩可以。

商用？乖乖掏钱。

有问题留言，咱们一起琢磨！

## 参考来源

Antigravity-Manager项目：https://github.com/lbjlaq/Antigravity-Manager

Google Antigravity官网：https://antigravity.google

Anti-API项目：https://github.com/ink1ing/anti-api

CLIProxyAPI项目：https://github.com/router-for-me/CLIProxyAPI

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
