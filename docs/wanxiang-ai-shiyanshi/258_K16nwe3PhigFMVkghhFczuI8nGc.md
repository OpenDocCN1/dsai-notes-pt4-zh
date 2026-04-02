# 社区共学课件：OpenClaw 部署及发布实操指南

> 来源：[https://yunyinghui.feishu.cn/docx/Y4lpd0DFToezn8xnURCcYk9tnRf](https://yunyinghui.feishu.cn/docx/Y4lpd0DFToezn8xnURCcYk9tnRf)

![](img/b226d2e1e8c7b30a65255550629c7851.png)

# 🎯自我介绍

**铁柱**

+   万象 AI 实验室共创者

+   AI 自媒体博主（曹工不加班）

+   10 年全栈开发者

+   n8n 社区节点开发者

![](img/eecd624491cab84db87f09c7e2d1bf19.png)

---

# 🎯直播回放

> 直播回放：

![](img/0656651bcb42287aff1372a3f456f79d.png)

[OpenClaw部署及发布实操指南.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XML5bCkLEozZTwxOqLFcsOFSnfj)

# 准备篇：零门槛部署的硬件与软件清单

## 硬件要求

| **配置项** | macOS/Windows 本地 | **云服务器** |
| --- | --- | --- |
| **CPU** | M1/M2/Intel 均可 | 2 核+ |
| **内存** | 4GB+ | 2GB（推荐 8GB） |
| **存储** | 10GB+ | 10GB+ |
| **网络** | 稳定网络 | 公网 IP |

## 通用要求

1.  Node.js >= 22.0.0

1.  **npm**（Node.js 自带）

1.  **国内大模型 API Key**（智谱 GLM / Minimax / 月之暗面 / 千问等）

1.  **Git： 安装需要用到****，**[Windows 下载点这里](https://github.com/git-for-windows/git/releases/download/v2.52.0.windows.1/Git-2.52.0-64-bit.exe)，**Mac 用户终端执行命令**xc**ode-select -**-install ，Linux 玩家自己解决

## 使用费用

不要使用 Token 付费的方案

不要使用 Token 付费的方案

不要使用 Token 付费的方案

对话多次以后携带的上下文巨大，对话一次好几块钱

**推荐使用国内各家推出的 Coding Plan，比如 智谱 GLM，Minimax，K**imi 等等，按调用次数，周期刷新的套餐，不用考虑太多 Token 费用的问题。

## 安全问题

### 不要开放公网访问

在自己可控范围内使用，不要公开到公网上！

### 不要在公司部署

**如果你的机器人被入侵，会威胁它在**的所有环境，你也不想因为这个丢了工作对吧

### 保护好自己的 API Key

**给**其他人分享解决问题的时候，要把自己的关键信息，调用凭证去除。别人拿到你的信息就可以调用服务，最终付费的是你。

**如**果已经泄露，及时去平台进行删除，本文所有涉及的 API Key，Token 在直播结束后我都会在所属平台进行删除。

# 实操篇：手把手带你部署 OpenClaw

## 🎯 macOS 本地部署

### Step 1: 安装 Node.js

1.  访问 [Node.js 官网](https://nodejs.org/)

1.  下载 macOS 安装包（选择 22.x LTS 版本）

![](img/698b22319759935585dcc4683ac9ff08.png)

[node-v22.22.0.pkg](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IVv4bHo3ioNagvxvjTcc6Sepn2d)

1.  安装步骤如下：

| ![](img/fcd97305721c9a7720788bb9ca39416c.png) | ![](img/188d27053963f896b3f55db24486e2e8.png) | ![](img/44d7eb08e9e85e5bb0d6a7c8e7de188d.png) |
| --- | --- | --- |
| ![](img/3cc3675246363a8f2fc6b83433a6a339.png) | ![](img/a9050f629801e49c733ab3569deb692d.png) | 最后一步输入系统登录密码即可安装完成 |

1.  打开终端，可以在 mac 右上角找到放大镜，搜索“终端”

![](img/28b61807e562a4a755256d965816dab5.png)

```
node --version
npm --version
```

![](img/8f6405f663eb77347fee1e07be21598c.png)

### Step 2: 安装 OpenClaw

```
# 打开终端
# 全局安装最新版(完整复制第三行)
npm install -g openclaw@latest --registry=https://registry.npmmirror.com

# 验证安装
openclaw --version
```

![](img/eec3d1a8e53ab409498cd5e7c5edee4d.png)

⚠️ **注意：上面的图片是老版本安装时候的图片，除了**命令不同，其他流程都一样，不要输错了命令

✅ 安装完成！接下来请跳转到 统一配置与使用 章节。

---

## 🎯 Windows 本地部署

### Step 1: 安装 Node.js

1.  访问 [Node.js 官网](https://nodejs.org/)

1.  **下载 Windows 安**装包（选择 22.x LTS 版本）【进上面飞书群下载文件】

![](img/b0beadbff6950bf594e7690ea75e63bf.png)

[node-v22.22.0-x64.msi](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HM0nbUH7XoLIQVxsFS1c0R3ln3c)

1.  安装步骤如下

| ![](img/3fe8638b6ee8760d65622199b04aefdd.png) | ![](img/39accaf0f0544c0001c9e513bea8198b.png) | ![](img/db13dbf52754f1005f52ab65f26018e8.png) |
| --- | --- | --- |
| ![](img/fe7aec3ebba5bdae31b0d8101dbacac4.png) | ![](img/6ba93bea2a01b0f789e57643eb182d1f.png) | ![](img/a0e17b53dd7451ed0573b2773c6c9b72.png) |

1.  打开命令提示符（CMD）或 PowerShell 验证：

![](img/dc68765d7d1eec3aa00da21c770bf72c.png)

```
node --version
npm --version
```

![](img/94701e6a132c072046d5bd734832bea2.png)

### Step 2: 安装 OpenClaw

```
# 以管理员身份打开命令提示符或 PowerShell
# 全局安装最新版(完整复制第三行)
npm install -g openclaw@latest --registry=https://registry.npmmirror.com

# 验证安装
openclaw --version
```

![](img/3016c40ce95e16f70619c5eb375278c0.png)

⚠️ **注意：** 如果遇到权限错误，请确保以管理员身份运行命令提示符。

⚠️ **注意：上面的图片是老版本安装时候的图片，除了**命令不同，其他流程都一样，不要输错了命令

✅ 安装完成！接下来请跳转到 统一配置与使用 章节。

## 初始化配置

> 📌 说**明： **无论你选择哪种部署方式（macOS、Windows），配置和使用步骤都是相同的。

### Step 1: 启动配置向导

macOS/Linux（腾讯云）：

```
# 启动向导并自动设置后台服务
openclaw onboard --install-daemon
```

**Windows: **

```
# 以管理员身份运行
openclaw onboard --install-daemon
```

这个命令会：

+   启动交互式配置向导

+   自动配置后台服务（macOS 使用 daemon，Windows 使用系统服务，Linux 配置 systemd）

+   设置开机自启动

### Step 2: 跟随向导完成配置

下面是使用初始化命令来完成对 OpenCl**aw **的配置，按照截图上的选项选择以后按回车键进行下一步配置

**关键配置步骤：**

#### **忽略安全警告**

![](img/569325c4bb501024cb7d756064045832.png)

#### **配置模式**

![](img/c6db879943ccc2c4775e1a17cc4f89db.png)

#### **选择模型：**

![](img/50712b10d0d8d666b18c98d3d2fe9be7.png)

这边选择智谱作为示例

#### **填写 API 配置：**

+   Provider Name: **Z.AI**

![](img/49b2938f5e7ee0451a46dd45cac872be.png)

![](img/7c8c842bd1a617547caa067ce8a1318d.png)

#### **通信通道：**

这块咱们先跳过，因为列表里都是国内不支持的聊天软件，如果你有对应的渠道，这个配置应该也难不倒你

![](img/1c58e6d232a0ad7d5e50570525da2e35.png)

⚠️ **重要：** 暂时选择"暂不配置"，通过 Web Dashboard 使用即可。

#### 配置 skills（技能）

云服务器可以选 Yes，在后续的列表中选择想用的技能，本地部署选择 No，后面有更方便的方法配置

![](img/70fc1455bc5d82553f1ca5f5d4f3b79c.png)

#### **Hooks 钩子（可选）：**

![](img/bf6de3ec02b3b1b2492934a436cd0ab5.png)

#### 网关自启动

这一步会自动完成，等待即可

![](img/5b35d0a3286bddffcd27d713019f90b9.png)

#### 选择对话方式

![](img/850dbb8986dccbc7dbba52553da40ab2.png)

建议本地部署选择 **Web UI**，云服务器选择 **TUI**

完成向导后，OpenClaw 会自动在后台运行

### Step 3: 访问 Web Dashboard

#### 本地部署（macOS/Windows）：

输入下面的命令会自动使用系统默认浏览器打开 Web UI，如果没有打开，复制终端给出的完整 URL，粘贴到浏览器打开。

```
输入命令
openclaw dashboard
```

![](img/33395733e7729b6e1f1bfd0f7d925dba.png)

## 接入自定义大模型

我们以英伟达模型平台为例，现在注册可以免费调用模型，可以用来测试。

1.  前往 https://build.nvidia.com/ 英伟达模型平台注册账号

1.  创建一个 API Key

1.  用记事本或者代码编辑软件打开 ~/.openclaw/openclaw.json文件，在 models 部分增加下面的代码

```
{
  ... 其他内容不要改
  &quot;models&quot;: {
   &quot;providers&quot;:{
        &quot;英伟达nvidia&quot;: {
            &quot;baseUrl&quot;: &quot;https://integrate.api.nvidia.com/v1&quot;,
            &quot;apiKey&quot;: &quot;nvapi-HxKN8Zp2-xxxxx-替换成你的 key,
            &quot;api&quot;: &quot;openai-completions&quot;,
            &quot;models&quot;: [
              {
                &quot;id&quot;: &quot;moonshotai/kimi-k2.5&quot;,
                &quot;name&quot;: &quot;moonshotai/kimi-k2.5&quot;,
                &quot;reasoning&quot;: false,
                &quot;input&quot;: [
                  &quot;image&quot;,
                  &quot;text&quot;
                ],
                &quot;cost&quot;: {
                  &quot;input&quot;: 0,
                  &quot;output&quot;: 0,
                  &quot;cacheRead&quot;: 0,
                  &quot;cacheWrite&quot;: 0
                },
                &quot;contextWindow&quot;: 200000,
                &quot;maxTokens&quot;: 8192
              }
            ]
        }
        ... 其他内容不要改
    }
    ... 其他内容不要改
  }
}

```

1.  终端输入 openclaw config 按下面的选项选择

![](img/5b7d823ef1060f30660c52d367fe81c7.png)

![](img/8119204b1c1342c34335b8c05f18e3a8.png)

![](img/d131262b3a710b4934136f6085c824b6.png)

![](img/716adfe6fea14cb9229ef41acad53654.png)

![](img/21aab44c432d8d56eb03f8aed8e46088.png)

1.  输入openclaw gateway restart 重启网关，再输入 openclaw tui 测试模型，问他是什么模型，不要看下面指示的内容，不准的。

![](img/f3d56090fe336750cfe60303b3e64079.png)

**同理可以用相同的方法配置其他平台的模型，只要支**持 OpenAI 兼容模式的平台都可以。

# 发布篇：手把手教你如何接入钉钉

## 一、前置准备

+   钉钉企业管理员权限

+   已安装 OpenClaw

+   确保网络畅通

---

## 二、钉钉开放平台配置

### 2.1 创建企业内部应用

1.  访问 [钉钉开放平台](https://open-dev.dingtalk.com/)，登录企业账号

![](img/4aa2ca331ae2746bbab4beb6dff11eaf.png)

1.  进入 **应用开发** → **企业内部应用** → 点击 **创建应用**

![](img/1804f061411d4a525ad3f329c9e54792.png)

1.  填写基本信息：

+   **应用名称**：如 "AI 助手"

+   **应用描述**：简要说明功能

+   **应用图标**：上传图标（建议 512x512 像素）

1.  点击 **保存**

![](img/8dee6ddccd7be70a1ec1b23fe81e40de.png)

### 2.2 添加机器人

![](img/9b186d77d0f7d7273bd780c9a2439988.png)

### 2.3 配置机器人

1.  上一步添加完以后左侧导航选择 **机器人**

1.  开启 **机器人配置**，填写：

+   **机器人名称**：用户看到的机器人昵称

+   **消息接收模式**：选择 **Stream 模式**（重要！)

+   **机器人描述**：简要功能介绍

1.  点击 **发布**

![](img/ae5a9c7902bfb5376f171aa67ee2521c.png)

### 2.4 设置权限

进入 **权限管理**，搜索并开通以下权限：

+   ✅ **企业内机器人发送消息**（必选，默认已经选择）

+   ✅ **AI 卡片流式更新权限**（推荐，支持流式输出）

![](img/7ff2c445942650ff8ae4c72f6f881ea0.png)

+   ✅ **互动卡片实例写权限**（推荐，支持高级卡片）

![](img/0fcfe844586ba9545cff0c20f5a848b6.png)

### 2.5 发布应用

1.  进入 **版本管理与发布** → **查看版本详情** → **编辑**

![](img/9f5a2273f58053d19622f2f6d4c774a8.png)

![](img/dfedc2132d23b62f1ce5813b7f6ade1b.png)

1.  **可见范围** 设置为 **全部员工**（或指定部门）

![](img/5a9042add12a8463d556d22a985bdfd5.png)

1.  保存并点击 **确认发布**，如果是自己创建的企业自己审核就行了，目前这些权限是免审的

![](img/27bfd464ef7b0acd0d0f3cf4c4c95606.png)

### 2.6 获取配置参数

在应用详情页记录以下参数：

| 参数名称 | 获取位置 | 说明 |
| --- | --- | --- |
| clientId | 应用凭证 → **AppKey** | 应用唯一标识 |
| clientSecret | 应用凭证 → **AppSecret** | 应用密钥 |

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
mkdir -p ~/.openclaw/extensions &amp;&amp; open ~/.openclaw/extensions
```

Windows 根据打开的终端不同输入不同命令

CMD

```
mkdir &quot;%USERPROFILE%\.openclaw\extensions&quot; &amp; explorer &quot;%USERPROFILE%\.openclaw\extensions&quot;
```

PowerShell

```
mkdir -Force ~\.openclaw\extensions; ii ~\.openclaw\extensions
```

上面的命令执行后会在 OpenClaw 的配置文件夹内创建扩展（extensions）文件夹**并且通过系统资源管理器打开这个文件夹，把下面的钉钉插**件源文件解压到扩展文件夹下。下面的图片是安装飞书插件的具体流程，钉钉插件的操作方式一样，注意文件夹名称路径别搞错就行了。

![](img/422873d9ab957ab366e52a2d5bb27599.png)

[dingtalk.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EsnYb3DhDoJExZxl6GDcMj6Kn5b)

![](img/1473882865be70af90aa584e1073db17.png)

然后执行插件的安装，新打开一个终端，定位到这个目录

Mac 这么操作可以进入到目录

![](img/a7b7e325e0e0216cc0433521837c3b82.png)

Windows 在文件夹空白处按住 Shift +鼠标右键点击，从打开的菜单中选择一个和终端相关的选项

![](img/e1a676811ce0e227259e1d77dbc2782b.png)

在指定的目录下打开终端后执行下面的命令安装依赖

```
npm install --registry=https://registry.npmmirror.com
```

![](img/dc44a252728298f69b4df67be098e925.png)

---

## 四、配置参数

**编辑 O**penClaw 配置文件 ~/.openclaw/openclaw.json，增加下面绿色标注的部分，所有用到的标点符号都是英文：

```
{
  ... 其他内容不要改
  &quot;channels&quot;: {
    &quot;dingtalk&quot;: {
      &quot;enabled&quot;: true,
      &quot;clientId&quot;: &quot;你的应用 Client ID&quot;,
      &quot;clientSecret&quot;: &quot;你的应用 Client Secret&quot;,
      &quot;dmPolicy&quot;: &quot;open&quot;,
      &quot;groupPolicy&quot;: &quot;open&quot;,
      &quot;messageType&quot;: &quot;markdown&quot;,
      &quot;debug&quot;: false
    },
    ... 其他内容不要改
  }
}
```

messageType 选项说明：

+   text - 纯文本（兼容性最好）

+   markdown - Markdown 格式（推荐）

+   card - 互动卡片（需开通卡片权限，支持流式输出）

这几个选项大家可以自己测试一下效果

还要加一段：

```
{
  &quot;gateway&quot;: {
    ... 其他不要改
    &quot;http&quot;: {
      &quot;endpoints&quot;: {
        &quot;chatCompletions&quot;: {
          &quot;enabled&quot;: true
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

**单聊测试**：

1.  在钉钉中搜索你的机器人名称

1.  发送消息测试对话

**群聊测试**：

1.  进入群聊 → **设置** → 群管理 → 机器人 → 在打开的面板中找到你添加的机器人

![](img/fcab342ced28bf6ea12364bf819031ef.png)

![](img/c9f783f337f3ef708de1d998172b72bd.png)

![](img/66f2517fc44452a6c777ea0ad66f13bd.png)

1.  在群中使用 @机器人名称 你的问题 触发对话

![](img/4f0eaf57dc907ce6447d00ab845291aa.png)

---

## 六、常见问题

### Q1: 机器人无响应？

+   检查 ~/.openclaw/openclaw.json 配置是否正确

+   确认应用已发布且可见范围包含当前用户

+   查看 OpenClaw 日志：openclaw logs

### Q2: Stream 模式连接失败？

+   检查网络连接，确保可访问钉钉服务

+   重启 OpenClaw 服务：openclaw restart

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

1.  谷歌浏览器

1.  终端

### 获取浏览器插件

1.  在终端中输入

```
openclaw browser extension install
```

![](img/2d1fe4807d0810e6092805aab2e3a52d.png)

1.  执行以后会给一个本地文件夹地址，里面有浏览器插件的文件

### 安装浏览器插件

1.  让 openclaw 用它的用户文件打开浏览器，我们在新增的浏览器里安装插件。

1.  在打开的浏览器点击“**管理扩展程序**”

![](img/1a8b6f03a3cac02e342ee28a07350b36.png)

1.  打开**开发者模式**开关，再点击左侧出现的**加载未打包的扩展程序**

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

访问 https://skills.sh/，查找你想要的技能，让它运行命令安装即可。可以先给它安装个 find-skills 技能，再通过这个技能

1.  B 站链接转文章直接发布到公众号草稿箱

1.  爬取热点新闻做成四格漫画

1.  对标运营 X/小红书/抖音 账号，自动获取博主分享内容，分析运营策略，怎么对标

---

# 📱交流合作

持**续关注并更新 Op****enClaw 的相关资讯动态，研究报告，商业落地，常见问题，永久免费在线查看。已建立社群**聊聊 OpenClaw 发展、应用、对接及合作，邀请嘉宾讲课，没进群的可以扫码加入，群满可加大国微信 ID：zhangyuguo94s（备注：OpenClaw），已经在社群内的无需重复加入，开源知识库及社群学习全部为公开免费分享。

> **特别说明：**不要申请编辑和复制文档，有需求请进群加微信沟通。

![](img/4b7641891704a99782a4e1a522d5fcce.png)

![](img/9d62bc17b12ac4baa4b98cbd560d675d.png)

**微信学习交流群**

微信ID：zhangyuguo94s，备注：OpenClaw

**扫码交流、进群、合作**

![](img/204d65add9935fc72fcd181dde32dcfd.png)

**飞书学习交流群**