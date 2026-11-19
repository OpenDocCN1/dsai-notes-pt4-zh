
# N8N自动化运营X（原twitter）平台

> 来源：[https://xuqiwei1986.feishu.cn/docx/GbCKdpIrIotByoxyPJuchLhOnfe](https://xuqiwei1986.feishu.cn/docx/GbCKdpIrIotByoxyPJuchLhOnfe)

> 大家好，我是达轮，今年418加入赚钱，有幸被邀成为`n8n`的航海教练，同时也参加了 “AI 自媒体 - X（推特）”的航海，因此，我还想写一篇关于`X`上自动化内容的文章。

> 上一篇 [<<](https://scys.com/articleDetail/xq_topic/2852428158445581)[用AI](https://scys.com/articleDetail/xq_topic/2852428158445581) `Agent`+`N8N`在`Reddit`找海外需求>> ，希望本篇文章对大家有用。

> 为你节约时间，提前准备好内容，一切就交给`n8n`了。让节约的时间做更有价值的事情。巴拉巴拉...

> 下列文档从简单到复杂设计的，小白肯定要循序渐进，要有耐心，啊哈哈！有基础的酌情考虑。

> 哪些写的不好，大家多多指正。🙏🏻🙏🏻

## 0.基础环境

我这边是云服务器部署`n8n`服务，也购买了对应的域名，`n8n`的版本如下图。`X`在`Oauth 2`授权的时候需要公网的域名。我的`X API`是`free`版本的，完全够用。

![](img/db807df5f87734abf5040783cac610e7.png)

![](img/5ac8dd988c8ed10033762be6bc2dfefc.png)

## 1.基本需求

需求就是将内容发布`X`上

### 1.1工作流主要组件

#### 1.1.1工作流总览

![](img/762978bf76e2193e86b22e395ca8bd3a.png)

#### 1.1.2工作流流程图

手动触发 ----> 发推

### 1.2工作流拆解

搜索组件“`twitter`”

![](img/85115b790fab2bd448f0997acfe6adff.png)

#### 1.2.1 X OAuth2授权给N8N

参考文档 `https://docs.n8n.io/integrations/builtin/credentials/twitter/` ，如果英文阅读困难，可以下载chrome插件“`沉浸式翻译`”

开通`X API` `https://developer.x.com/en/portal/petition/essential/terms?plan=free`

![](img/615d60805dc9fa37a63cebd21bf20295.png)

![](img/6a110e1df66abe626e25f0b7e65e5e3f.png)

填写如:`I will use the X API to collect and analyze public tweets for sentiment analysis, trend detection, and keyword tracking. This is for research and improving my project’s content strategy.I plan to use the X API to build a tool that allows users to view their own tweets, followers, and engagement metrics in a dashboard. The tool will only access user-authorized data.`

三个选项勾选

生成 `Client ID` 和 `Client Secret` 的步骤：

1.  打开 `Twitter Developer Portal` `https://developer.x.com/en/portal/projects`，进入你的项目。

1.  在项目的 `概览（Overview）` 标签页，找到 `Apps` 部分，点击 `Add App`。

1.  给应用取一个 `名字`，然后点击 `Next`。下图是创建好的`App`

![](img/d5e9752f4fcb7c4f7ff6a0a841b4ff50.png)

1.  进入 `App Settings`（应用设置）。

![](img/dfc1cb2572fb52deeda98a5b2f0e3d78.png)

1.  在 `User authentication settings`（用户认证设置） 中，点击 `Set Up`。

![](img/a42225f6a903d46b546504e263bed184.png)

1.  设置应用权限（`App permissions`）：

+   如果你希望在 `n8n` 的 `X` 节点里使用所有功能，请选择 `Read and write and Direct message`。

+   在 `Type of app`（应用类型） 部分，选择 `Web App`, `Automated App` or `Bot`。

![](img/a451c644b2cfd1bafe6f552aa9c29793.png)

1.  回到 `n8n`，复制 `OAuth Redirect URL`。

![](img/76cceb86889b859cd9522c8d62905b07.png)

1.  在 `X` 应用的 `App Info` 部分，把刚才复制的 URL 粘贴到 `Callback URI / Redirect URL` 中。

1.  添加一个 `网站 URL`（`Website URL`）。

![](img/b882ce1616a250dea2857bf19d4c0f74.png)

1.  复制 `X` 显示的 `Client ID` 和 `Client Secret`，粘贴到 `n8n` 凭据配置的对应字段。

![](img/f5a06dbc0a8d038d0380ba3bec934ce2.png)

![](img/a0def991701f7df5d4b0c9a612b8de2b.png)

1.  需要做下链接`X`账号的动作，点击`connect my account`，按照提示登录，出现授权页面，点击`Authorize App` 进行授权。

![](img/21576865a63864a5879b66ff4286fe03.png)

![](img/a741d379eeab02448f8c4db2554e40c7.png)

![](img/8db6fe21bb0d6cd6d7dcfe031b3ac521.png)

#### 1.2.2 测试结果

那我们来测试一下 发送 "`hello world` " 到`X`上，点击`Execute workflow`节点，执行成功之后，我们到`X`上就可以看到自己的推文了。

![](img/8ea27ef52b0a15e6f4488ac4acc069c1.png)

![](img/80b171d77dba5d9e4b532eb5d2908e0e.png)

## 2.进阶需求

我需要准备一个`Google Sheet` ，提前将`Google sheet`的推文准备好，定时发送发送推文。发送成功之后，标记已发送，防止工作流重复发送。

### 2.1 工作流主要组件

#### 2.1.1 工作流总览

![](img/3bc9ae83e539f7f61f51dcbd64aa3815.png)

#### 2.1.2 工作流流程图

### 2.2 工作流拆解

#### 2.2.1 Google sheet OAuth2 授权给N8N

1.  创建一个 `Google Cloud Console` 项目，`https://console.cloud.google.com/home/dashboard` 需要创建一个 `Google Cloud Console` 项目。如果你已经有项目，可以跳到下一步：使用你的 `Google` 账号登录 `Google Cloud Console`。在顶部菜单的项目下拉框中选择 `新建项目`，或者直接进入 `新建项目` 页面。输入项目名称，选择项目的归属位置。点击 `创建`。

![](img/8ce96fa3be0d397948ca5d7aacb40380.png)

1.  检查顶部导航栏，确保项目下拉框中已经选中你刚刚创建的项目。如果没有，请手动选择。

1.  项目创建好后，需要启用你要访问的 `API`：打开 `Google Cloud Console` - `API` 库，确认你处在正确的项目中。在左侧导航进入 `API` 和 `服务` > `库`。

1.  搜索并启用所需的 `API`。

+   例如：要使用 `Google Sheet` 节点，请搜索并启用 `Google Sheets`。

![](img/664af2d03a07cc06975a2ae65d8638a2.png)

1.  打开 `Google Cloud Console`，确保你在正确的项目中。在 `API` 和 `服务`中选择 `凭据`（`Credentials`）。点击 `+` 创建凭据 > `OAuth 客户端 ID`。

![](img/e034e637a9a6b73625ad8e0bdf18468d.png)

1.  在应用类型下拉框选择 `Web 应用`。填写名称。回到 `n8n` 的凭据页面，复制 `OAuth 重定向 URL`，粘贴到 `Google Console` 的 `授权重定向 URI` 中。点击 `创建`。

![](img/986a159c85e5af4bb1cd9765e1414f69.png)

1.  在 `Google` 弹出的 `OAuth 客户端`创建成功窗口中，复制 `Client ID`，粘贴到 `n8n` 的凭据中。同样复制 `Client Secret`，粘贴到 `n8n` 的凭据中。

![](img/3bd0fcb025b45bf21fca03055f7df7e2.png)

![](img/24911aa7bbd5ee5d9f86e923b74c8a63.png)

1.  在 `n8n` 中点击 `使用 Google 登录`（`Sign in with Google`），完成认证。

![](img/29c63d0cdcf9bfcb62b5e0250c8491ab.png)

![](img/599fc1e4abdf788cd05609f564f71cfc.png)

#### 2.2.2 `Schedule Trigger`定时器

![](img/b46156f94b6797f9a5264994a6ea2203.png)

#### 2.2.3 Google Sheet表数据结构

我们创建一个`google sheet`表 `https://docs.google.com/spreadsheets/u/0/`

第一列放发送内容 ，第二列放是否发送成功的标志。

![](img/bd2118955304eb94b5cc596e1e1f2db8.png)

回到`n8n`的 `google sheets`组件找到文件

![](img/c77eeeb7decd33f1f55fa42a32c07070.png)

注意我们需要选择返回一条数据

![](img/2a5290c52183e3478127140b2506b1b2.png)

我们将 左侧的`hello world 1`拖到`Text`输入框中

![](img/8df4e9f3e1866127e9aefe3201f64c92.png)

#### 2.2.4 `X`组件

![](img/4e495da5ea6896a5c217d4336eccffce.png)

#### 2.2.5 `IF`组件

主要的作用，对接`X`，如果返回`json.text`不为空的话，就可以判断推文发送成功。

![](img/3086305d2dc6f700ef496538831af5c6.png)

#### 2.2.6 更新`Google Sheet`表格

如果发推成功，将表中`isSent = 1`

![](img/6dc0c2c32ca58be27cfa951cad76c119.png)

![](img/d7e2c82f7bfd7cb00d2bfa2f21856f73.png)

#### 2.2.7 测试结果

![](img/9f21089a5c9ae4796ef0ddcae3e15848.png)

手动触发一下，就可以看到`Google Sheets`保存的推文发送你的`X`上了。

## 3.高阶需求

长线程的推文挺🔥，用`n8n`实现长线程的推文。

内容取自阿西老师的普通人如何在推特抓住大流量机会 `https://scys.com/articleDetail/xq_topic/1524185158481222` ，内容很精彩！！！

![](img/8cc2b56c8d414fa3a531120c1f8f3d59.png)

### 3.1 工作流主要组件

#### 3.1.1 工作流总览

![](img/5f5e6f56842254980ba5c8c13db472e2.png)

#### 3.1.2 工作流流程图

### 3.2 工作流拆解

`Google sheet OAuth2` 授权 `2.2.1`

`X OAuth2`授权 `1.2.1`

这里带过，可看前面的章节

#### 3.2.1 `Init Google Sheets`

做下筛选，将没有发送成功的数据拿出来

![](img/d0cc135bb5287ce2271c269281f16e38.png)

表格结构

![](img/417c1f8e2ff123ce346096a78bfdfe39.png)

#### 3.3.2 `Edit Fields`节点

筛选下表格字段

![](img/a4e4a54caf1bcc977b8010deebd59c16.png)

#### 3.3.3 `Code`节点

处理下数据，供后续的循环节点使用

![](img/08b52888c83ae53c946727d6479e86ab.png)

#### 3.3.4 `Loop Over Items`节点

将整理的数据做循环

![](img/836387d8e7a07c5010f34ef4eeea2ae8.png)

#### 3.3.5 `Search Google Sheets` 和 `IF1`

+   通过行码进行筛选数据读取`prev_id`，判断是否是第一条推文。

![](img/b3fd92523606f1224539f029828e7697.png)

![](img/ac85281c3c48e60dd40eb8acf85db765.png)

如果是第一条推文，调用`X`组件，获取`ID`,更新`prev_id`

![](img/24acddd54880ca5794be7cfb8826856b.png)

![](img/447f93c4ce77d91006d748ef6a400e7b.png)

接下来判断不是第一条推文的情况

#### 3.3.6 `Wait` 节点

防止`X2`节点报错

![](img/b3e7ee4d955e038b41bcb5efdc7b7dab.png)

这里我设置`300s`

![](img/fea12a456a7fb4556be85870f4fd0449.png)

通过`prev_id`进行形成长线程的推文

![](img/281393a31666c83e54a0d6fa4edc6801.png)

更新`prev_id`

![](img/3c6f93cf67ec3ae13e2f3dcc98ad5c21.png)

#### 3.2.7 测试结果

![](img/09ab7e8f6efb6d7b69fcb57c84cf679b.png)

## 4.举一反三

+   [<<](https://scys.com/articleDetail/xq_topic/2852428158445581)[用AI](https://scys.com/articleDetail/xq_topic/2852428158445581) `Agent`+`N8N`在`Reddit`找海外需求>>  这篇文章用到了`AI`组件，如果这篇文档你可以倒腾了， 可以配置一些`AI`节点，添加炫酷的提示词，润色你的推文。

+   我这边用到了`X`的`Create`的操作，大家可以尝试下搜索的动作,做一些收集数据的功能。

希望大家可以多多提意见，迭代我的文档。
