
打开默认的 `UI`，在里面找到“代理”（`Agent`），然后找到 `Skill`，把你所需要的内容直接点击“使能”就可以了。

### 第一种方式：默认 UI 安装

![](img/f745e6f6e4c98f5fb379276927ca29bf.png)

### 第二种方式：插件市场下载（安全）

`https://clawhub.ai/`

首先先安装这个商城的插件工具：

```
npm i -g clawhub
```

![](img/efc62331c7d12ee19b95be00b0d4ac95.png)

![](img/d22579f3abfc2c91c9dc324ac5d7fe90.png)

安装的时候，直接参考你最后的这个标签，按照下面的格式输入即可：

```
clawhub install "liang-tavily-search"
```

### 第三种方式：GitHub下载（有风险）

`https://github.com/vercel-labs/skills`

```
npx skills add vercel-labs/agent-skills
```

比如，以这个仓库为例：`https://github.com/JimLiu/baoyu-skills?tab=readme-ov-file`

下载查找你有哪些`Skill`s

仓库：`https://github.com/vercel-labs/skills/tree/main/skills/find-skills`

```
npx skills add https://github.com/vercel-labs/skills/tree/main/skills/find-skills
```

![](img/4814048a399aecc5b04d0631218b98e3.png)

![](img/0ab223904cdbef9f2ab389fa9e0099af.png)

## 4.3 如何使用`Skill`发🛰️👸号

第一步：下载baoyu的👸号发布包

![](img/5f7d72b1ecebb33bacadbc0c6009d835.png)

[`baoyu-post-to-wechat.zip`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IVPvbNPZkolbvGxkEbHcPrZanCd)

```
npx skills add https://github.com/JimLiu/baoyu-skills/tree/main/skills/baoyu-post-to-wechat
```

将这个`skill`放到自己的`openclaw`的`skill`文件夹中

![](img/d12b744a48363b4b051556307424a4be.png)

第二步：在这个`Skill`下面新建一个`md`文件，命名：`EXTEND.md`

```
default_theme: default
default_color: blue
default_publish_method: api
default_author: 科学羊
need_open_comment: 1
only_fans_can_comment: 0
```

![](img/facfff922e3ee51d8e5e68a31ca21f9f.png)

```
WECHAT_APP_ID=xxxx
WECHAT_APP_SECRET=xxx
```

直接对话即可！注意🛰️👸号，开启白名单！

![](img/100df666bb67f7d3593f81e0bb4d6129.png)

![](img/f11525c531e91de8e2f0740f25fa61b9.png)

![](img/29e5f9027330c9f762fb82f386741ef0.png)

## 4.4 图像生成`skill`

```
npx skills add https://github.com/JimLiu/baoyu-skills/tree/main/skills/baoyu-image-gen  
```

![](img/ccdf56340885b8f858df3e9614bacdf5.png)

`https://aistudio.google.com/app`

![](img/45e48d3ee9db507bfa45384003114273.png)

```
GOOGLE_API_KEY=AIXXXXXXXXXXXXX
GOOGLE_IMAGE_MODEL=gemini-3.1-flash-image-preview
```

接下来你就可以让 AI 配置，大家记得这个技能是出自宝玉的 GitHub。

大家只要按照这个命令安装完之后，就直接把你的这个 Key 去配置好就可以了。

![](img/02d2ec96b963e47f9d733f2e4d27f67a.png)

## 4.5 `IMA-Skill`安装 @20260318更新

配置地址：`https://ima.qq.com/agent-interface`

![](img/ac58ff917a61a20af0008c8f7663997b.png)

把第一步的内容直接一键复制，然后发给小龙虾，告诉他让他自己去配置。顺便你可以把这个 Key 直接给他，等他安装好之后就可以测试了。

![](img/553e9447c9669c3a9c49a0cb97569cbd.png)

![](img/4662ff671a25f41526a37fbb87be9cf3.png)

完毕！

# 五、如何创建群聊机器人

> 编写中...

第一步：打开飞书创建群聊，至少要拉一个人进群，才能创建群聊

![](img/3cf3313571923f9e1d9931d258d37bd8.png)

![](img/2c21dd3831eec696a65b9c7333a6d2e7.png)

![](img/e08064c10f7d7e2ebb25c2d5b6e382de.png)

```
openclaw config set channels.feishu.groupPolicy open
openclaw config set channels.feishu.replyToMode off
openclaw gateway restart
```

# 六、安全问题

如何加固你的小龙虾？

### Step 1：`Gateway`绑定到本地 + 安全组双重封锁

这是最容易被忽视也是最致命的问题，需要两道防线同时做：

第一道：在`OpenClaw`配置里把`Gateway`绑定到本地地址，让端口物理上不对外开放——相当于龙虾根本不出门，门都没开，保证只有在这台机器本地运行的程序才能访问它：

```
{
    "gateway": {
        "bind": "loopback"
     }
}
```

第二道：在云服务器安全组（腾讯云/阿里云）里限制`18789`端口，只允许你自己的IP访问，其他来源全部拒绝——相当于小区大门就把陌生人挡住了：

```
入站规则：TCP 18789 端口，来源 → 你的IP地址（如 123.456.789.0/32）
删除或禁用：0.0.0.0/0 对18789的任何开放规则
```

### Step 2：管好你的`APIKey`

不要把密钥直接写在配置文件里，使用环境变量引用；

在模型提供商后台设置消费上限（比如`Kimi`的49元月卡就是天然的消费上限）；

定期检查消费记录，发现异常及时轮换`key`。

### Step 3：不要随便装来路不明的`Skill`

安装社区`skill`之前，至少做三件事：

看`SKILL.md`里有没有奇怪的命令、看`scripts`目录里的脚本在干什么、优先使用`ClawHub`上有审核评分的`skill`。

同时安装`Skill Vette`这款`Skill`，会在安装任何`Skill`之前，帮你把它审查一遍，类似于杀毒软件里。

### Step 4：群聊权限要隔离

接入群聊时：开启 `requireMention: true` 只在被`@`时响应、设置 `groupPolicy: "allowlist"` 限定群组、设置`session.dmScope: "per-channel-peer"`、群聊场景不开放`exec`和文件读写权限。

然后也记得从聊天软件层面开启防护，比如我使用`Discord`群聊的操作就是关闭群聊的邀请。

### Step 5：设置算力消耗的"保险丝"

在API提供商后台设消费上限、定时任务设合理超时时间防死循环、定期看 `openclaw logs` 有没有异常高频调用。避免第二天起床看到账单被刷爆。

### Step 6：定期跑安全审计

```
openclaw security audit基础审计
openclaw security audit --deep深度审计
openclaw security audit --fix自动修复
```

### Step 7：开启沙盒模式

让龙虾在`Docker`容器里运行，就算它要搞事情，也只能在容器里面折腾。特别是当龙虾跑在你自己常用的机器里的时候，需要特别注意这一点。

开启沙盒后，龙虾的`exec`默认是`deny`模式，需要你手动配置白名单来放行必要的命令。

建议至少每周一次，改完配置后立即跑一次。审计跑完后会给一些问题清单给你，可以直接把这些清单发给龙虾，让他给你修复的建议。

# 七、其他扩展

## 4.1 企业🛰️+云服务器

其实你也可以尝试搭建在云端服务器，然后绑定你的企业🛰️来测试，比如，我测试过：

`https://cloud.tencent.com/act/pro/double12-2025`（注意需要买海外的服务器，轻量级就可以）

![](img/13fea0f9ce2d421339e074e8868353b9.png)

搭建方法和我们mac的方式一样。

假如你需要对接企业🛰️，就需要成为企业开发者然后用企业🛰️来实现远程操作你的agent，如下图所示。具体关于企业🛰️的配置我就先不写了，大家如果感兴趣，我在后续补充。

![](img/761016180206adfd5cf0aae36bbbbd50.png)

## 4.2 常用指令

启动`Openclaw`的`TUI`

`openclaw tui`

重启网关

`openclaw gateway restart`

开启新对话

`/new`

添加备用模型

`openclaw models fallbacks add [模型公司代号/模型名称]`

例如：`openai-codex/gpt-5.2-codex`

设置默认模型

`openclaw models set [模型公司代号/模型名称]`

例如：`kimi-code/kimi-for-coding`

`openclaw gateway start`【启动】

`openclaw gateway restart`【重启】

`openclaw gateway stop`【停止】

`openclaw tui`【命令行对话】

`openclaw config`【配置】(修改模型,通道,网关,等。。。)

`npm update -g openclaw`【更新`openclaw`主程序】

`npm uninstall -g openclaw`【卸载大龙虾】

模型管理命令

添加备用模型（主模型不可用时自动切换）

`openclaw models fallbacks add [提供商代号/模型名称]`

### 示例：添加OpenAI Codex作为备用

```
openclaw models fallbacks add openai-codex/gpt-5.2-codex
```

### 设置默认模型

```
openclaw models set [提供商代号/模型名称]
```

### 示例：将Kimi设为默认编程模型

```
openclaw models set kimi-code/kimi-for-coding
```

## 4.3 如何新增每日推送新闻订阅

你只需要在终端运行一行命令：

```
openclaw configure --section web
```

然后它会提示你输入 `API Key`，你粘贴进去就行。

![](img/a8d745ad628ae89f9376146e170743c1.png)

如果你还没有 `Brave API Key`：

1. 去 `https://brave.com/search/api/`

1. 注册免费账户（每月 2000 次搜索）

1. 创建 API Key，复制下来

1. 回来运行上面的命令，粘贴进去

具体执行截图：

![](img/2875c18ea8e1c3e8896674f542a1520a.png)

## 6.4 其他玩法

[`OpenClaw + 影刀RPA组队：小龙虾指挥影刀 RPA 干活`](https://yulu-tech.feishu.cn/docx/Ra9xdOnbWojwclxj9U0cftmynwf)

# 八、FAQ：智能体（`discord`、飞书等）突发中断怎么办？

重启大法：`openclaw gateway`

先启动`Openclaw`的`TUI`看对话是否正常。

`openclaw tui`

### 可能的原因：

1. **`Gateway` 连接断了** — Discord 连接可能掉了，需要重启

2. **`Token` 过期** — Discord bot token 可能过期了

3. **频道权限** — bot 可能被禁言或没权限读消息

### 检查/修复步骤：

1. 检查 Gateway 状态

在终端运行

```
openclaw gateway status
```

2. 如果状态异常，重启 Gateway

```
openclaw config set commands.restart true

openclaw gateway restart
```

3. 检查 Discord Bot Token

去 Discord Developer Portal (`https://discord.com/developers/applications`)：

+   你的应用 → Bot → 看 Token 是否有效

+   如果显示 "Invalid" 或 regenerate 过，需要更新 token

### 4. 检查频道权限

+   bot 是否还在频道里？

+   是否有 "Read Messages" 和 "Send Messages" 权限？

### 手动启动（每次开机后）

打开终端，运行：

```
openclaw gateway start
```

### 6.4 出现异常报错

```
⚠️ Agent failed before reply: All models failed (2): openai-codex/gpt-5.2-codex: OAuth token refresh failed for openai-codex: Failed to refresh OAuth token for openai-codex. Please try again or re-authenticate. (auth) | moonshot/kimi-k2.5: ⚠️ API rate limit reached. Please try again later. (rate_limit).Logs: openclaw logs --follow
```

这个错误意味着网关尝试使用您的 Codex OAuth 刷新令牌，但令牌已过期，刷新失败 (取消/轮换/无效)。当发生这种情况时，机器人无法进行模型调用→无响应。

重新认证 Codex OAuth(最常见的解决方案)

Re-run the Codex OAuth login on the gateway host:

在网关主机上重新运行 Codex OAuth 登录：

```
openclaw models auth login --provider openai-codex
openclaw gateway restart
```

只需要运行上面这两个指令就可以了。注意登录 Codex 的时候，需要做一个授权。

好了，以上就是今天的分享，但是文章并不止步，关于`OpenClaw`的详细用法和技巧，我会学到新的内容后第一时间来更新文章，欢迎大家的关注与支持。

## 信息不回复怎么办？

![](img/a021af412c08d6b185b1f3d4bff6529d.png)

我非常建议你至少要有 3 到 4 个客户端，然后分门别类地进行监控，这样会比较好一点。

比如我自己用的`telegram`、`discord`、飞书、`TUI`等

![](img/0ea3822cf904a1e27a0f2c82f96bd9cf.png)

![](img/57867524a39abbcef83bb915896bfbea.png)
