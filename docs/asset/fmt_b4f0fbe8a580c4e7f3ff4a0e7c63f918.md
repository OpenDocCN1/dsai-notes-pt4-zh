
```json
{   
  "dify_api_base": "https://api.dify.ai/v1",  
  "dify_api_key": "app-xxx",  
  "dify_app_type": "chatbot",  
  "channel_type": "wx",  
  "model": "dify",  
  "single_chat_prefix": [""],  
  "single_chat_reply_prefix": "",  
  "group_chat_prefix": ["@bot"],  
  "group_name_white_list": ["ALL_GROUP"]
}
```

我们按照 2.2.1 小节启动程序并扫码登录，然后给🛰️机器人发送消息，进行测试

![](img/92539059896824d24c74307a23117fef.png)

🛰️机器人的回复与在 Dify 测试页面上的回复一致。恭喜你更进一步，把工作流编排应用接入了个人🛰️，你可以向知识库中导入更多的 Dify 官方文档，让🛰️机器人为你解答更多的 Dify 相关问题。

## 2.4. 把 Agent 接入🛰️

### 2.4.1. 创建 Agent 应用

进入工作室页面，点击创建空白应用，选择 Agent，设置图标和应用名称，最后点击创建

创建成功后，我们会进入 Agent 应用配置页面，在这个页面我们选择好对话模型，然后添加工具。我们首先添加 DALL-E 绘画工具，首次使用该工具需要授权，一般我们设置好 OpenAI API key 和 OpenAI base URL 即可使用该 DALL-E 绘画工具。

授权成功后，我们添加 DALL-E 3 绘画工具

接着，继续添加 DuckDuckGo 搜索引擎和数学工具，进行后续的工具测试

![](img/ee959c865a99aaa804ab966b0cf44f92.png)

我们输入问题"搜索开源项目 Dify 的 star 数量，这个数量乘以 3.14 是多少"，确认应用能够正常调用工具，我们依次点击发布、更新、访问 API

### 2.4.2. 生成 Agent API 密钥

我们继续参照 2.1.1 小节（4）获取智能助手应用的 API 密钥与 API 服务器地址

### 2.4.3. 接入🛰️

我们在项目根目录创建名为 `config.json` 的文件，文件内容如下，同样把 `dify_api_base` 配置为智能助手应用的 API 服务器地址；`dify_api_key` 配置为智能助手应用的 API 密钥，注意该应用为智能助手类型应用，还需要把 `dify_app_type` 设置为 `agent`，其他配置保持不变

复制

```json
{    
  "dify_api_base": "https://api.dify.ai/v1",    
  "dify_api_key": "app-xxx",    
  "dify_app_type": "agent",    
  "channel_type": "wx",    
  "model": "dify",    
  "single_chat_prefix": [""],    
  "single_chat_reply_prefix": "",    
  "group_chat_prefix": ["@bot"],    
  "group_name_white_list": ["ALL_GROUP"] 
  }
```

继续参照 2.2.1 小节启动程序并扫码登录，然后给🛰️机器人发送消息，进行测试

![](img/69e25f2b229554574c15101a91a25dbc.png)

可以看到🛰️机器人可以正常使用搜索和绘画工具。再一次恭喜你，把 Dify Agent 应用接入🛰️。也恭喜我，写到这里可以先睡觉了。

## 2.5. 把工作流接入🛰️

### 2.5.1. 创建工作流应用

[首先你需要提前下载好我预先创建的 DSL 文件，点击此处下载](https://github.com/hanfangyuan4396/dify-on-wechat/blob/master/dsl/chat-workflow.yml)。下载完成后，进入工作室页面，点击导入 DSL 文件，上传提前下载好的文件，最后点击创建。

![](img/3b51827b50c9b9a4ca61b13cf92b4eec.png)

创建完成后，按照上图步骤进行测试。点击运行，输入你好，确保该工作流能正常输出结果。

你可以在 [工作流的基础上进行修改](https://github.com/hanfangyuan4396/dify-on-wechat)，但是对于工作流类型的应用，它的输入变量名称十分灵活，为了更方便地接入🛰️机器人，Dify on WeChat 项目约定工作流类型的应用输入变量命名为 `query`，输出变量命名为 `text`。

![](img/06c21b2d0154ad7ca8bfa81a3ec65c16.png)

测试没有问题后，按照上图步骤发布应用，依次点击发布、更新、访问 API。

### 2.5.2. 生成工作流 API 密钥

我们同样参照 2.1.1 小节（4）获取工作流应用的 API 密钥与 API 服务器地址。

### 2.5.3. 接入🛰️

我们在项目根目录创建名为 `config.json` 的文件，文件内容如下，同样把 `dify_api_base` 配置为工作流应用的 API 服务器地址；`dify_api_key` 配置为工作流应用的 API 密钥，注意该应用为工作流类型应用，还需要把 `dify_app_type` 设置为 `workflow`，其他配置保持不变

复制

```json
{    
  "dify_api_base": "https://api.dify.ai/v1",    
  "dify_api_key": "app-xxx",    
  "dify_app_type": "workflow",    
  "channel_type": "wx",    
  "model": "dify",    
  "single_chat_prefix": [""],    
  "single_chat_reply_prefix": "",    
  "group_chat_prefix": ["@bot"],    
  "group_name_white_list": ["ALL_GROUP"] 
  }
```

同样参照 2.2.1 小节启动程序并扫码登录，然后给🛰️机器人发送消息，进行测试。

![](img/12368bdd6890e6f00d94335957673ea8.png)

可以看到机器人成功接通了工作流 api 并进行了回复，至此我们已经完全掌握了如何创建 Dify 所有类型的应用：基础聊天助手、工作流聊天助手、智能助手、工作流，我们也学会了如何把上述应用发布为 API，并接入🛰️。

接下来我将会介绍如何把应用接入到🛰️的其他通道，如👸号、企业🛰️应用、企业🛰️客服等。

## 3. 接入企业🛰️个人号

1.  Dify 接入企业🛰️个人号（仅限 windows 环境）

1.  有封号风险，请使用企业🛰️小号测试

1.  [在登录旧版本的企业微信时可能会出现企业🛰️版本过低，无法登录情况，参考 `issue1525`，请尝试更换其他企业🛰️号重试](https://github.com/zhayujie/chatgpt-on-wechat/issues/1525)

### 3.1. 下载安装企业🛰️

确保你有一台 windows 系统的电脑，[然后在此电脑下载安装特定版本的企业🛰️](https://dldir1.qq.com/wework/work_weixin/WeCom_4.0.8.6027.exe)，[备用下载链接](https://www.alipan.com/s/UxQHrZ5WoxS)。

### 3.2. 创建 Dify 应用

我们已经在前面的 `2.1.1`、`2.3.2`、`2.4.1` 与 `2.5.1` 小节分别介绍了创建基础聊天助手、工作流聊天助手、智能助手、工作流这 4 种不同的 Dify 应用，你可以根据上面的教程任意创建一种应用。

### 3.3. 下载安装 Dify on WeChat

根据 `2.1.2（2）` 步骤，下载代码并安装依赖，为了后续能按照 `ntwork` 依赖，请确保你安装的 `python` 版本为 3.8、3.9 或 3.10。

### 3.4. 安装 ntwork 依赖

由于 `ntwork` 的安装源不是很稳定，可以下载对应的 whl 文件，使用 whl 文件离线安装 `ntwork`

首先需要查看你的 `python` 版本，在命令行中[输入 `python` 查看版本信息](https://github.com/hanfangyuan4396/ntwork-bin-backup/tree/main/ntwork-whl)，然后在 `ntwork-whl` 目录下找到对应的 whl 文件，运行 `pip install xx.whl` 安装 `ntwork` 依赖，注意 `"xx.whl"` 更换为 whl 文件的实际路径。

例如我的 `python` 版本信息为

```
"Python 3.8.5 (default, Sep 3 2020, 21:29:08) [MSC v.1916 64 bit (AMD64)]"
```

可以看到 `python` 版本是 `3.8.5`，并且是 `AMD64`，所以对应的 whl 文件为 `ntwork-0.1.3-cp38-cp38-win_amd64.whl`，需要执行如下命令安装

复制

```
pip install your-path/ntwork-0.1.3-cp38-cp38-win_amd64.whl
```

### 3.5. 填写配置文件

我们在 Dify on WeChat 项目根目录创建名为 `config.json` 的文件，下面是以 Dify 智能助手应用作为示例的配置文件，请正确填写你刚刚创建应用的 `dify_api_base`、`dify_api_key`、`dify_app_type` 信息，请注意 `channel_type` 填写为 `wework`

复制

```json
{   
  "dify_api_base": "https://api.dify.ai/v1",  
  "dify_api_key": "app-xxx",  
  "dify_app_type": "agent",  
  "channel_type": "wework",  
  "model": "dify",  
  "single_chat_prefix": [""],  
  "single_chat_reply_prefix": "",  
  "group_chat_prefix": ["@bot"],  
  "group_name_white_list": ["ALL_GROUP"]
}
```

### 3.6. 登录企业🛰️

务必提前在电脑扫码登录企业🛰️

### 3.7. 启动🛰️个人号机器人

运行如下命令启动机器人

复制

```
cd dify-on-wechatpython app.py
```

我们可以看到终端输出如下信息，等待 wework 程序初始化完成，最后启动成功～

复制

```
[INFO][2024-04-30 21:16:04][wework_channel.py:185] - 等待登录······
[INFO][2024-04-30 21:16:05][wework_channel.py:190] - 登录信息:>>>user_id:xxx>>>>>>>>>name:
[INFO][2024-04-30 21:16:05][wework_channel.py:191] - 静默延迟60s，等待客户端刷新数据，请勿进行任何操作······
[INFO][2024-04-30 21:17:05][wework_channel.py:224] - wework程序初始化完成······
```

![](img/17ace4a144719dcb60d0a1810bcafaaf.png)

现在我们给机器人发送消息，可以看到接入成功！

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
