
# Gemini 3 Pro 使用体验报告

给它一个GitHub issue："这个API响应太慢，优化一下。"

它直接分析代码，找到3处性能瓶颈，给出优化方案，还写了完整的单元测试。

2.5 Pro也能做，但经常需要我反复提示。

3 Pro一次到位。

更牛的是生成式UI，这是全新功能。

你说："做个天气查询页面。"

它不只给你代码，直接生成一个可运行的完整网页。

拖拽窗口、点击按钮、实时更新，全部自动设计好。

这个2.5 Pro完全做不到。

## 图片理解，真的准

还有个让我意外的功能，图片理解。

我以前也用GPT-5.1和Claude 4.5看图，它们也能看懂。

但总感觉是先把图转成文字描述，再理解。

Gemini不一样，是直接理解图片本身。

我试了张产品原型图，问："这页面布局有啥UX问题？"

它说："导航栏太密，移动端会误触。按钮间距建议`48px`以上，符合触控友好设计。"

连`48px`这种具体数字都给出来了。

又试了张系统架构图，各种箭头、模块、注释密密麻麻。

它直接指出："数据库模块是单点故障，建议加主从复制。"

GPT和Claude看图也行，但没这么细。

## 实时联网，查最新信息不用自己搜了

Gemini 3 Pro能直接联网查信息。

我试了一下，问它："2025年11月AI圈有啥热点？"

它直接列出来：

OpenAI发布GPT-5.1多模态升级。

Anthropic推出Claude 4.5 Sonnet。

xAI发布Grok 4.1深度推理版。

Gemini 3 Pro Preview上线。

GitHub Copilot集成多模型支持。

每条都带来源链接。

我点开看了几个，确实是这几天的新闻，没瞎编。

Gemini这个实时联网，原生支持，不用配置，直接问就行。

## 编程能力飞跃，竞赛级水平

Gemini 3 Pro的编程能力，真的吓到我了。

我试着问它："能不能在浏览器里做个操作系统界面？"

它直接给我写了个完整的MacOS风格界面。

窗口可以拖拽，dock栏能点击，连系统设置都模拟出来了。

全是纯前端代码，可以直接跑。

有人测过，它在编程竞赛中能达到金牌选手水平。

这不是吹的。

## Google官方工具，专业选手的最爱

Google还出了官方工具，方便开发者用Gemini 3。

### `Gemini CLI`（命令行工具）

直接在终端用Gemini，选Pro模式自动用`gemini-3-pro-preview`。

适合开发者写脚本、自动化任务。

但需要白名单资格。

`Cli`：Ultra、API付费用户可以自动通过申请。

![](img/c7334ec16fbd74b443acf5c52ca5446c.png)

### `Antigravity IDE`

Google官方的开发环境，专门为Gemini优化。

下载地址：`https://antigravity.google/download`

支持代码补全、调试、项目管理，和Cursor一样的。

这俩工具一出，说明Google是认真的。

不只是发个模型完事，连配套工具都齐了。

Antigravity虽说Gemini 3 和 Claude 4.5都免费，但是BUG还挺多，尤其卡在登录上的，官媒已经爆了。

`Antigravity`：就都登录上了无法回调，包括我，已知方法是修改成`TUN`模式，或者更换账号，我是这两种方法都不行。

我`Debug`了一下，是他没有调用`Localhost`，配置`Proxy`也不好使，感觉是走不了系统代理。

X上的官媒评论区都是这个问题。

![](img/b48d2c5be0f39fa774c3eb996094e3aa.png)

## 当然也有短板

Gemini 3 Pro不是完美的。

中文理解还差点意思，特别是一些梗、俚语，它经常get不到点。

插件太少了，GPT有一堆插件，Claude也有`Tool Use`，Gemini基本啥都没，都得自己配。

稳定性有点抽风，毕竟Preview版本，刚试用就碰到1次返回错误。

国内访问需要魔法，你懂的。

但这些小问题不影响大局。

## 怎么用？2步搞定

最简单方式（推荐）：

1. 打开 `https://ai.google.dev/`

2. 登录Google账号，选择`gemini-3-pro-preview`

## 别死守一个AI了

2025年11月，AI工具早就不是"二选一"了。

长文档用Gemini，100万token无敌。

写代码用Claude 4.5和GPT-5.1，各有所长。

追热点用Grok，实时信息最快。

各有各的强，但Gemini最灵活。

---

数据摆这了。

LMARENA第一。

Artificial Analysis第一。

ARC-AGI-2第一。

239天。

从3月25日Gemini 2.5 Pro，到11月19日Gemini 3 Pro。

239天。

它来了。

不只是它。

Gemini 3 Pro，语言模型第一。

Veo 3.1，视频生成带原生音频。

Imagen 4，图片细节清晰度炸裂。

Lyria 2，音乐生成。

Nano Banana 2.0，下一代图像模型即将上线。

一年前，Google的AI产品线，哪个拿出来都打不过人家。

现在，每个拿出来都能打。

Google，回来了。

---

PS：Gemini 3 Pro目前还是Preview版本，预计12月初正式发布。现在用是抢先体验，碰到bug别惊讶。

PPS：

学生党福利来了！

Google官方刚宣布，美国大学生 延长 免费用一年Google AI Pro计划。

包括Gemini 3 Pro更多访问权限（超过每月10000 tokens的限制）、NotebookLM更高限制、2TB存储空间等等。

申请地址：`gemini.google/students`

![](img/54dd1bbb96a3e666223c740904946884.png)

`https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc`
