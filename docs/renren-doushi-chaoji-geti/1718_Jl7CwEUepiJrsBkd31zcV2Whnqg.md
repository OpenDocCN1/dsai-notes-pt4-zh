
# ChatGPT to Notion 插件复盘-ChatGPT 优化版本

> 来源：`https://xuqiwei1986.feishu.cn/docx/AoXtdCCssojUUCxpeVQcivUTn2d`
> 以下编写后经过 ChatGPT 优化

## 背景

1. 项目起源：由于个人需求，我开发了一个将 ChatGPT 聊天记录保存到 Notion 的插件。
2. 在谷歌商店审核通过并上线后，我在龙哥盟的星球发了一个帖子。随后，树哥“出海研习社”的 Rocks 大佬发到群里了，吸引了众多大佬的关注和建议。

## 收到的建议与问题

1. 更换 Logo：让插件形象更专业，提高品牌识别度。
2. 建立官方网站：Aladdin 大佬赞助了一个域名，`chatgpt2notion.com`，用于宣传和介绍插件。
3. 在 Twitter 和 Product Hunt 进行推广：扩大影响力和用户群体。
4. 开发同类其他产品：树哥提议拓展产品线，为用户提供更多实用工具。

## 实施过程与复盘

1. 洗澡灵感：洗澡时决定立刻开始实施大佬们的建议，优化项目。
2. 高效执行：经过约两小时的努力，基本完成了建议内容处理。
3. 复盘分享：通过这篇文章记录优化过程，以便总结经验教训。（实际上，写这篇文章所花费的时间比完成优化工作的时间还要长。）

## 换 Logo（使用 SD 生成）

在过去，我制作插件 Logo 时通常会使用阿里的智能 Logo 服务或在线生成图标工具。但这一次，我尝试采用了一种新的方法——利用 AI 绘画技术，结合 Stable Diffusion 和 ControllNet 插件，为插件创作了独特的 Logo。

### 使用 SD 生成 Logo 的步骤

1. 在 Stable Diffusion（SD）中生成 Logo：多次生成，从中挑选一个自己认为满意的作品。选择尺寸为 512x512，以保证清晰度。此次，我随意设置了关键词“logo”和“simple”以作为提示。
2. 去除背景：因为 Logo 中存在灰色背景，所以我使用 `remove.bg` 工具将其去除。
3. 调整尺寸：将 Logo 的尺寸调整为 128x128，以满足谷歌插件的 Logo 尺寸要求。由于我的电脑上安装了 Photoshop，我直接使用它进行尺寸调整。当然，您也可以在网上找到其他更改尺寸的在线工具。
4. 更新 Logo：完成以上步骤后，Logo 制作及更新工作就完成了。

### 其他说明

1. 阿里的智能 Logo 服务是收费的。但实际上，我通常不会付费使用。我会在生成一些 Logo 后，挑选一个觉得满意的，然后直接截图。接下来，重复前述步骤，去除背景并调整尺寸。
2. 对于在线生成图标工具，您可以在网上搜索找到很多相关资源。此外，英文站的航海工具集合里也有详细介绍。

## 建立网站（使用 ChatGPT 输出代码）

考虑到这是一个简单的插件介绍网站，我参考了 ChatGPT for Google 插件的网站设计。最初，我计划使用 Vercel（`https://vercel.com/`）这个在线搭建网站工具，但在尝试几次后发现部署过程总是卡在 Deploy 环节，于是我决定放弃。因为这只是一个简单的介绍页面，我选择直接制作一个静态网页。

在未来，如果 GPT4 推出上传图片功能，我们可以直接将草图上传并让 AI 生成相应的设计。这将会更加便捷高效。

使用 ChatGPT 生成静态页面代码：为 AI 提供需求描述，让其生成对应的代码。

ChatGPT 提问内容：

```
开发一个介绍一个谷歌插件“ChatGPT to Notion”的静态页面，纯 html5,css3 输出，简洁。
1、页面左上角显示 logo，右上角显示 twitter 和 github 图标链接。
2、第一行显示网站名称“ChatGPT to Notion”，h1 大字体。
3、第三行是描述，“ChatGPT to Notion is a tool that simplifies saveing your ChatGPT conversations to Notion. Just one click, saving you time. Say goodbye to the hassle of manual transfer and hello to a more efficient way of working. Try ChatGPT to Notion now!”
4、下一行是两个按钮，一个“Add to Chrome”，一个 “Add to Edge&quot;”，按钮内有图标。
5、下一行是一个显示 youtube 视频的位置。
6、下一行是一个 FAQ 标题，
7、下一行是显示 FAQ 的具体内容，每个项目右边有个向下的箭头，点击可以看到详细内容。
    1）What is ChatGPT?
    2）What is ChatGPT to Notion?
    3）Is ChatGPT to Notion free to use?
    3）Do I need a ChatGPT/OpenAI account?
5、页脚是联系方式
    Contact: chatgpt2notion@gmail.com
```

源代码，就不贴了，可以直接看：`view-source:https://chatgpt2notion.com/`

将 ChatGPT 生成的 HTML 和 CSS 代码整合在一起，并使用浏览器进行预览。发现基本效果满意后，我对部分细节进行了微调：

1. 添加了 Logo、Twitter、GitHub、Chrome 和 Edge 的图片。
2. 修改了 FAQ 样式：仅将标题加粗，而非全部文字加粗。
3. 完成调整后，我将静态网页上传到了自己的服务器（服务器已安装宝塔服务，方便搭建静态网站）。

注：服务器提供的 HTTPS 证书免费使用 3 个月，到期后会自动续期。

4. 进行域名解析，发现域名解析仍然在 Aladdin 大佬那里。今天请他帮忙进行域名转移后，几分钟后网站就可以正常访问了。

## 推广发文

### 发布 Product Hunt（借助 ChatGPT 撰写文案）

1. 登录 `https://www.producthunt.com` 发布产品。
2. 之前由于仅在该网站浏览，未验证邮箱，发现无法发布。于是重新发送验证邮件并验证通过，便可以发布产品了。
3. 使用 ChatGPT 撰写相关内容。

主要注意到，在发布产品的表单中，需要填写标签、描述等内容。由于不擅长撰写文案，便求助于 ChatGPT。（其他已有基本信息部分略过）

以下是一个有效的撰写请求：

> `write a [number] characters [topic] of [product name] to post to Product Hunt`。

#### Tagline

写标签文案

```
write a 60 words  Tagline description for ChatGPT to Notion to post to Product Hunt
```

![](img/10da98054a271c0700bfe00d602facfb.png)

#### Description

写描述文案

```
write a 260 characters Description of  ChatGPT to Notion to post to Product Hunt
```

![](img/930563d782c26bdae69a8dc8c2165376.png)

#### Comment Prompt

评论内容和反馈文案

```
Write the first comment  for ChatGPT to Notion to post to Product Hunt 
```

![](img/549abaf16e26a22e75753fb6f12a4559.png)

```
Ask for feedback  for ChatGPT to Notion to post to Product Hunt 
```

![](img/9bb81aa189b9210fd119335846614f1a.png)

这样操作后，我成功完成了产品信息的填写。点击“立即发布”，即可提交产品。最终发布效果：`https://www.producthunt.com/posts/chatgpt-to-notion`

### 发布 Twitter（借助 ChatGPT 撰写文案）

因为发推就发一条内容，比较简单，不过也找了 ChatGPT 写了相关文案。

1. 写一条适合在推上发布的文案，它还提示了字数限制。

```
Please generate a tweet based on the following key points: ChatGPT to Notion.
The tweet should be no longer than 280 characters and should be suitable for sharing on Twitter.
```

![](img/638ec7ce77a3bea629352a781b547b12.png)

2. 接下来，撰写几条用于在 Twitter 同一话题线（`thread`）中发布的文案。

```
Write a 50 words copy for a product called &quot;ChatGPT to Notion&quot; that helps saveing time to save chatGPT chat log, then ask them to install it

Write a 50 words copy for a product called &quot;ChatGPT to Notion&quot; that tell people I had build a product ChatGPT to Notion that helps saveing time to save chatGPT chat log, then ask them to install it
```

![](img/78c134a53578dab124cb745d380696d4.png)

## 提交插件到 Edge 浏览器扩展商店

由于 Edge 浏览器基本兼容谷歌插件，且我开发的这个插件没有使用到高级接口，因此基本无需做太多修改便可成功提交。可以直接上传插件，系统会提示需要修改的部分。

1. 在我的情况下，只需移除 `manifest.json` 文件中的 `update_url` 配置即可。
2. 其他基本信息按照谷歌插件的填写要求完成。
3. 需要注意的是，提交时需要填写一个搜索关键词。

![](img/08dfc2d8815f128b3e3f3e506ff7d2d4.png)

再次寻求 ChatGPT 提供建议：

```
write 10 Search terms for ChatGPT to Notion, Up to 7 words.
Each word should not exceed 30 characters. All search terms must contain no more than 21 independent terms.
```

![](img/c3fdf5a23d045cbed0c95607f8d61630.png)

## 其他

### 大佬们无私的支持

果然，龙哥盟星球的大佬们都充满利他精神。

在我撰写这篇文章的过程中，子木大佬和强哥大佬在私下里给我提供了许多宝贵建议，非常感谢他们。

### 让 ChatGPT 直接设计界面 UI 并输出代码

在撰写这篇文章时，我突发奇想，或许可以让 ChatGPT 来设计界面并输出代码。尝试后发现，确实可行。

多进行复盘分享也是有益的，因为它可以激发新的想法。

![](img/b2a913bd5bb7819735eb2164175c9d5d.png)

先这么多先了，继续撸起袖子加油干了。

最后，分享插件的相关信息，欢迎各位大佬们多多指导和支持。

网站主页：

`https://chatgpt2notion.com/`

ProductHunter：

`https://www.producthunt.com/posts/chatgpt-to-notion`

Twitter：

`https://twitter.com/ALuoker/status/1636408203008442368`

谷歌商店：

`https://chrome.google.com/webstore/detail/chatgpt-to-notion/bknieejaaomeegoflpgcckagimnbbgdp`

Edge 商店（审核中）：

`https://microsoftedge.microsoft.com/addons/detail/olooogglggmjmgbmcbpgbdcichofacje`
