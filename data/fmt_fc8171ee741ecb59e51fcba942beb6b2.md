
| 参数名称 | 获取位置 | 说明 |
| --- | --- | --- |
| `clientId` | 应用凭证 → `AppKey` | 应用唯一标识 |
| `clientSecret` | 应用凭证 → `AppSecret` | 应用密钥 |

> **提示**：可以点击参数旁的复制按钮快速复制

![](img/eb4d0f86f7eef75e0588d8631b6edc49.png)

---

## 三、钉钉插件安装

在终端执行以下命令（推荐方式）：

```
openclaw plugins install https://github.com/soimy/clawdbot-channel-dingtalk.git
```

### 插件安装失败？

如果遇到因为网络或者其他原因安装钉钉插件失败的，按照下面的方法安装

Mac 打开终端输入下面的命令：

```
mkdir -p ~/.openclaw/extensions && open ~/.openclaw/extensions
```

Windows 根据打开的终端不同输入不同命令

CMD

```
mkdir "%USERPROFILE%\.openclaw\extensions" & explorer "%USERPROFILE%\.openclaw\extensions"
```

PowerShell

```
mkdir -Force ~\.openclaw\extensions; ii ~\.openclaw\extensions
```

上面的命令执行后会在 OpenClaw 的配置文件夹内创建扩展（`extensions`）文件夹并且通过系统资源管理器打开这个文件夹，把下面的钉钉插件源文件解压到扩展文件夹下。下面的图片是安装飞书插件的具体流程，钉钉插件的操作方式一样，注意文件夹名称路径别搞错就行了。

![](img/422873d9ab957ab366e52a2d5bb27599.png)

[dingtalk.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EsnYb3DhDoJExZxl6GDcMj6Kn5b)

![](img/1473882865be70af90aa584e1073db17.png)

然后执行插件的安装，新打开一个终端，定位到这个目录

Mac 这么操作可以进入到目录

![](img/a7b7e325e0e0216cc0433521837c3b82.png)

Windows 在文件夹空白处按住 Shift + 鼠标右键点击，从打开的菜单中选择一个和终端相关的选项

![](img/e1a676811ce0e227259e1d77dbc2782b.png)

在指定的目录下打开终端后执行下面的命令安装依赖

```
npm install --registry=https://registry.npmmirror.com
```

![](img/dc44a252728298f69b4df67be098e925.png)

---

## 四、配置参数

编辑 OpenClaw 配置文件 `~/.openclaw/openclaw.json`，增加下面绿色标注的部分，所有用到的标点符号都是英文：

```
{
  ... 其他内容不要改
  "channels": {
    "dingtalk": {
      "enabled": true,
      "clientId": "你的应用 Client ID",
      "clientSecret": "你的应用 Client Secret",
      "dmPolicy": "open",
      "groupPolicy": "open",
      "messageType": "markdown",
      "debug": false
    },
    ... 其他内容不要改
  }
}
```

messageType 选项说明：

+   `text` - 纯文本（兼容性最好）

+   `markdown` - Markdown 格式（推荐）

+   `card` - 互动卡片（需开通卡片权限，支持流式输出）

这几个选项大家可以自己测试一下效果

还要加一段：

```
{
  "gateway": {
    ... 其他不要改
    "http": {
      "endpoints": {
        "chatCompletions": {
          "enabled": true
        }
      }
    }
  }
}
```

![](img/b7b113606ce8c853fc87eb369aa7d760.png)

---

## 五、启动与测试

### 5.1 启动服务

```
openclaw start
```

### 5.2 测试机器人

#### 单聊测试

1. 在钉钉中搜索你的机器人名称

2. 发送消息测试对话

#### 群聊测试

1. 进入群聊 → `设置` → 群管理 → 机器人 → 在打开的面板中找到你添加的机器人

![](img/fcab342ced28bf6ea12364bf819031ef.png)

![](img/c9f783f337f3ef708de1d998172b72bd.png)

![](img/66f2517fc44452a6c777ea0ad66f13bd.png)

2. 在群中使用 @机器人名称 你的问题 触发对话

![](img/4f0eaf57dc907ce6447d00ab845291aa.png)

---

## 六、常见问题

### Q1: 机器人无响应？

+   检查 `~/.openclaw/openclaw.json` 配置是否正确

+   确认应用已发布且可见范围包含当前用户

+   查看 OpenClaw 日志：`openclaw logs`

### Q2: Stream 模式连接失败？

+   检查网络连接，确保可访问钉钉服务

+   重启 OpenClaw 服务：`openclaw restart`

### Q3: 权限不足错误？

+   返回钉钉开放平台，确认所需权限已开通

+   重新发布应用使权限生效

---

## 七、参考资源

+   [OpenClaw 钉钉插件仓库](https://github.com/soimy/openclaw-channel-dingtalk)

+   钉钉开发者文档 - 创建机器人应用

+   钉钉 API - 企业内部应用机器人

---

**配置完成！** 现在你可以在钉钉中愉快地使用 AI 助手了 🎉

# 案例篇：实操带你演示如何使用 OpenClaw

## 配置浏览器

### 准备内容

1. 谷歌浏览器

2. 终端

### 获取浏览器插件

1. 在终端中输入

```
openclaw browser extension install
```

![](img/2d1fe4807d0810e6092805aab2e3a52d.png)

2. 执行以后会给一个本地文件夹地址，里面有浏览器插件的文件

### 安装浏览器插件

1. 让 OpenClaw 用它的用户文件打开浏览器，我们在新增的浏览器里安装插件。

2. 在打开的浏览器点击 `管理扩展程序`

![](img/1a8b6f03a3cac02e342ee28a07350b36.png)

3. 打开 `开发者模式` 开关，再点击左侧出现的 `加载未打包的扩展程序`

![](img/8e830883438500fa90cc6b08be538260.png)

**macOS/苹果电脑**

打开一个文件夹窗口，顶部选择“前往”->“前往文件夹”，输入 “～/。openclaw/browser”

![](img/6abb2ab677535d99c1669b4daf18f935.png)

把插件目录拖过来选择

![](img/5248229294c296edbbd6a8dafa899045.png)

**Windows**

直接在打开的选择窗口粘贴获取到的文件夹路径，选择即可

### 开启浏览器插件

现在让 OpenClaw 调用浏览器打开百度，在打开的浏览器执行下面的操作

```
我给你装好了浏览器插件，你试试调用浏览器打开百度，搜索个最热的新闻给我
```

![](img/977a11cec7b0d742785a0cbe0bcbe2b1.png)

这样机器人就能操控浏览器了

## 开发/安装技能

这块其实是个磨合的过程，你可以和机器人描述你需求的 SOP，让它去构建一个 Skills，这样它自己遇到相同的需求就会调用这个 Skills 了，也可以让它去安装其他开发好的技能。

访问 https://skills.sh/，查找你想要的技能，让它运行命令安装即可。可以先给它安装个 `find-skills` 技能，再通过这个技能

1. B 站链接转文章直接发布到👸号草稿箱

2. 爬取热点新闻做成四格漫画

3. 对标运营 X/📕/抖音 账号，自动获取博主分享内容，分析运营策略，怎么对标

---

# 📱交流合作

持续关注并更新 OpenClaw 的相关资讯动态，研究报告，商业落地，常见问题，永久免费在线查看。已建立社群聊聊 OpenClaw 发展、应用、对接及合作，邀请嘉宾讲课，没进群的可以扫码加入，群满可加大国🛰️ ID：zhangyuguo94s（备注：OpenClaw），已经在社群内的无需重复加入，开源知识库及社群学习全部为公开免费分享。

> **特别说明：**不要申请编辑和复制文档，有需求请进群加🛰️沟通。

![](img/4b7641891704a99782a4e1a522d5fcce.png)

![](img/9d62bc17b12ac4baa4b98cbd560d675d.png)

**🛰️学习交流群**

🛰️ID：zhangyuguo94s，备注：OpenClaw

**扫码交流、进群、合作**

![](img/204d65add9935fc72fcd181dde32dcfd.png)

**飞书学习交流群**
