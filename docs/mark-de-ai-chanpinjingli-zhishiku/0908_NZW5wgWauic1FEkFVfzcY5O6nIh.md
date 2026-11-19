
# Claude Code 打通 Figma，2 分钟搞定产品 UI 界面（附保姆级安装教程）

> 来源：https://qqs7y1hozd1.feishu.cn/docx/U5TLdFmXKoHHrFxXzIKcrfF6nSf

## 一、安装 claude code

### 第 1 步：下载 Cursor

安装 claude code 之前，要先安装 Cursor、VSCode 等都可以。

直接让 Cursor 帮忙安装 claude code

![](img/c73a29a22e61f2fa10a3e8e415587163.png)

```
提示词：请直接在终端里帮我执行 npm install -g @anthropic-ai/claude-code 来安装 Claude Code CLI
```

![](img/5ed60c197babf61ccb1cbeb5709bdccf.png)

然后如果遇到停顿，你就点击【RUN】就可以了。到下图的这一步，你就回复【用本地安装方案就行】

![](img/82b3077402033ef583ca1a40eef7f38e.png)

直到右图的显示，就说明安装成功了。

### 第 2 步：打开 Cursor 底部的“终端 (Terminal)”

如果你现在界面底部没有那个用来敲命令的黑框框，你可以按下键盘快捷键 Cmd + J（Mac 系统），终端就会弹出来。

![](img/0633b4fc0f95b675b2435266ad5147cd.png)

### 第 3 步：输入启动命令并回车

在弹出的终端里直接输入下面这三个单词，然后按下回车键：

```
npm run claude
```

如果遇到报错，直接截图发给 Cursor 问，根据他的步骤来解决。解决完报错问题，显示以下界面说明 claude code 安装成功了。

![](img/1f99e8863a99ecc9112e672385a6be33.png)

## 二、接入 API

Claude Code 我们可以使用两种方式，第一种是官方版，第二种是使用国内模型如 GLM4.6 驱动 Claude Code。

> 使用官方版需要我们在 Claude.ai 进行订阅，目前 Claud 官方有 Pro、5x MAX、20x MAX 三种套餐形式，对应的价格分别为 20 刀、100 刀、200 刀。

> 另外因为 Claude 一直对国内进行封锁，所以想使用 Claude 官方版本需要突破网络和海外信用卡支付两个关卡，门槛较高。所以主要推荐大家使用国内的模型 GLM4.6 驱动 Claude Code

![](img/513650458de21d65e3fea89478165f2e.png)

如果你不是新用户，选择点击【GLM Coding Plan】

![](img/0c10cae75b33975d6f889ec2c7fb9f80.png)

想试试水的话，选择第一个【GLM Coding Lite】就行了。

然后点击左侧菜单栏的 API Key【添加新的 API Key】

![](img/e59746bb33a391bbdc9f8b4c063ca9fd.png)

复制下图的 API key。

![](img/2dfe80d4ed532ed15db78167bd99718a.png)

## 三、下载 cc switch

然后让 AI 直接帮你下载 cc switch，我是让 codex 帮我下载的。

codex 下载 mac 地址：https://openai.com/zh-Hans-CN/codex/codex

下载 windows 地址：https://apps.microsoft.com/detail/9plm9xgg6vks?hl=zh-CN&gl=CN

```
帮我下载 cc switch：https://github.com/farion1231/cc-switch/releases
```

![](img/3709d9be9b090c16ca5e610ff916487b.png)

下载完后，打开 CC Switch，点击右上角的添加【+】按钮。

![](img/2e8653273818346752beba5686cac6ca.png)

选择智谱模型，再往下滑输入 API。

粘贴刚刚复制的 API key，点击【添加】就可以了。

![](img/e9d16f3c6cf440d7dda94f55dfa116c6.png)

再点击设置按钮。

![](img/724fd7a01a2c4baeae90cca14379c0c0.png)

这里有个设置一定要开，点击下面的【应用到 claude code 插件】开关。

![](img/62faa30c7b34fb7f24f8ee85fd8b423d.png)

## 四、启动 claude code

点击删除按钮，删掉后，重新【command+j】打开终端。

![](img/b073bb03057fb14460104f41beeade41.png)

![](img/0074179184fe24700e6ca0a8c9a92085.png)

输入命令 `claude` 回车，就可以聊天了！

## 五、连接 figma mcp

### 第 1 步

退回到最外层的电脑终端。如果你现在还在 `>` 符号后面，请输入 `/exit` 然后回车。确保你回到了带有 `%` 符号的普通 Mac 终端界面。

### 第 2 步

执行官方的“全局安装”命令。在带有 `%` 的终端里，复制粘贴这行 Figma 官方专门给 Claude Code 准备的安装代码：

```
claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp
```

（注意：这里加了 `--scope user`，意思是给你电脑上所有的代码项目都装上这个插件，以后换个文件夹敲代码也不用重复装了。复制完按下回车执行。）

### 第 3 步

重新叫出 claude code。代码执行完毕后，再次输入启动口令，按回车：`claude`

### 第 4 步

关键一步——去 Figma 官网领“通行证”。在小机器人的 `>` 后面，输入 `/mcp` 并回车。看到 `>` 在 figma 中，继续回车。

![](img/3b79928d504856a2965ddaec393f5d80.png)

请选择 Authenticate(授权) 并再次回车。电脑会自动弹出一个浏览器网页。在网页里登录你的 Figma 账号，并点击 Allow Access (允许访问)。网页授权成功后，切回终端，你就会看到一句闪亮的 Authentication successful. Connected to figma.！

![](img/fbc8278e0712450097e890743f01aabc.png)

## 六、安装 figma plugin

复制下面这条命令，粘贴到 claude code，然后回车

```
/plugin install figma@claude-plugins-official
```

显示下面这个页面后，继续回车。

![](img/eedfee854b29ef2743c94b6d2593f4f0.png)

继续输入 `/plugin` 回车，会看到以下界面，键盘右键选择 `installed`，再按回车。

![](img/fd969c42fec649372ddab22ac685f372.png)

这样插件就安装成功了！然后连按几下键盘左上角的 Esc 键，直到退回那个只有 `>` 的普通聊天状态。

![](img/ee60cca591a0e1e2e440cf7e86b0c629.png)

## 七、报错解决

这里有一个报错卡了我很久，就是下图这个 400。

![](img/e950f9e5380778fab3793559a510eecd.png)

原本 Claude code v2.1.69 的版本降到 68 就可以了。

![](img/28c5cfe661cefaa041eaa522dfa1110e.png)

继续让 codex 帮你的版本改成低一个版本。提示词：帮我的 claude code 版本改成 v2.1.68

![](img/e202ccc3200cf2eb03de67529cca832f.png)

## 八、Claude code to figma

我们到 figma【复制链接】建一个空的文件。

![](img/2f7576becea21d545179578c8535638d.png)

![](img/964fd3ddac3439aea785db90cde97345.png)

回到 Cursor 中，输入提示词：需要在下面的提示词中粘贴你的 figma 链接。

```
直接在这个空的 Figma 文件里，为我设计一个现代风格的设计师个人网站首页，包含导航栏、个人简介、作品集展示区和页脚。设计完成后，再帮我生成对应的 HTML/CSS/JS 代码：https://www.figma.com/design/obFdrXRrkBf7YlQzLNrr5F/Untitled?node-id=0-1&amp;t=YaP6DHdJZ4fm0s
```

接着会一直做一些选项，一直选择 yes 回车就行。

最后会自动跳转到浏览器，然后可以直接点击【发送到 figma】

就会自动发送到 figma 文件中。

![](img/8beab83bbce07287d15df4c83bdf0fe7.png)

看看它生成的效果，不愧是 claude code，没话说，效果非常好！

## 九、修改完设计如何传回 code 里？

选择设计稿，右键复制 `copy link to selection`。

![](img/b5486c5034d37f80e357d02976626e25.png)

回到 claude code 中输入：

```
update changes:【这里是刚刚复制的链接】
```

![](img/5a83360acc7521efba63131e8dce6b5c.png)

claude 就重新构建了对应的部分，就这么简单。

![](img/db9934d56d1b5a9ec489087bf75cdddf.png)
