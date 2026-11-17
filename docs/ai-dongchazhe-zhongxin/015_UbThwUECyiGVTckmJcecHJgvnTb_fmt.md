
# 扣子使用问题FAQ

> 来源：[https://bluefocus.feishu.cn/docx/O5TzdbZhpo8p5xxFFTict1HrnHb](https://bluefocus.feishu.cn/docx/O5TzdbZhpo8p5xxFFTict1HrnHb)

为方便技术人员更快了解情况和解决问题，反馈时请提供以下信息：

## 请提供以下信息，帮助定位问题

1. 操作过程
2. 异常状况截图及描述
3. Bot/工作流/插件 编辑页面的网址链接
4. [`logid`](https://bytedance.larkoffice.com/docx/PTbYd85sHonfzTxZxx3c2lx0nFe#doxcn7IYrRaIEPwEnyTCKhQdE5d)

反馈邮箱：feedback@coze.cn

## [常见问题解答](https://bytedance.larkoffice.com/docx/PTbYd85sHonfzTxZxx3c2lx0nFe#KlHEdid3HoBZQWxpeHwcjuzlnjf)

展开左侧导航列表查看

## [`logid`查询方式](https://bytedance.larkoffice.com/docx/PTbYd85sHonfzTxZxx3c2lx0nFe#doxcnjqM7l3lJzGIlz1EBehmyjd)

展开左侧导航列表查看全部内容

- [Bot对话页面](https://bytedance.larkoffice.com/docx/PTbYd85sHonfzTxZxx3c2lx0nFe#doxcnmlTHsQvnXfuZcqvBrMr10b)
- [工作流](https://bytedance.larkoffice.com/docx/PTbYd85sHonfzTxZxx3c2lx0nFe#doxcnJc3oYYA1BbZtcjFpUhmeZg)
- [图像流](https://bytedance.larkoffice.com/docx/PTbYd85sHonfzTxZxx3c2lx0nFe#doxcn2C0QXrzyBYNUE9ELajyzde)
- [知识库](https://bytedance.larkoffice.com/docx/PTbYd85sHonfzTxZxx3c2lx0nFe#doxcnRL8EpXaEnMthmcwTFaSZ6d)

## 一、常见问题解答

### 扣子整体

#### 扣子使用额度

```
扣子使用额度规则：
1.目前扣子有使用额度限制，当日额度用完后，可以尝试切换其他模型使用，或者等待第二天额度刷新。
3.预计7月中旬上线专业版，开放额度充值，如果您已将扣子用于企业业务，可以添加下方企微，获得优先对接。
```

![](img/76fb3347ab8fdbd2719ecacb993c88b7.png)

#### Bot可以回复，但是“无法生成用户问题建议”

```
目前问题推荐走单独的模型和额度统计，每人每天100次使用额度，与Bot所使用的模型无关。额度用完后需要等待第二天额度刷新。
```

#### 提示词相关教程

```
Prompt engineering: https://platform.openai.com/docs/guides/prompt-engineering
扣子教程-编写提示词：https://www.coze.cn/docs/guides/prompt
```

#### Bot 被封禁

```
为进一步优化扣子商店环境，减少劣质及虚假 Bot 对大家造成的困惑，我们将持续对存在冒充、欺骗行为的 Bot 进行封禁处理。具体说明如下：

1. Bot 一旦被封禁，将不能再进行修改或发布操作，您需重新创建 Bot 方可使用。对于符合封禁条件的 Bot，我们不会提供解封服务。后续还有可能将处罚范围扩大至账户限制，所以请您在使用时务必尽量避免违规行为。具体可参考使用条款：https://www.coze.cn/docs/guides/content_principles

2. 若您的 Bot 存在被误封禁的情况，可邮件联系 feedback@coze.cn 进行反馈，邮件中请提供您的 Bot 名称、账号信息以及相关说明。

3. 若您发现有疑似违规 Bot，请邮件联系 report@coze.cn 进行反馈，并附上相关 Bot 链接或名称及截图。

非常感谢您的支持与理解！
```

#### Bot 发布被拒

运营判断属于发布标准和规范禁止发布的内容：

```
Bot 审核相关策略，您可以参考《扣子平台内容发布标准和规范》：https://www.coze.cn/docs/guides/content_principles。出于安全规范原因，无法向您提供更多审核细节信息。
```

#### 找不到已收藏的Bot

```
在扣子主页，点击右上角“Bots”，即可以查看已收藏的Bot，对话框@相关bot还可以进行对话。
```

![](img/ba0444cf015378a8135a49d3dcc6dcbd.png)

#### 扣子 “Bot分析”覆盖渠道

```
目前Coze.cn的各发布渠道，以下渠道的使用数据会纳入Bot编排页的分析统计：豆包、飞书、微信客服、微信公众号（服务号）、微信公众号（订阅号）、Bot商店、掘金。

以下渠道的使用数据暂时未纳入Bot编排页的分析统计：Bot as API、抖音评论区、抖音企业号、微信小程序
```

### 知识库

#### 知识库效果不佳常见设置问题

```
知识库效果不佳常见设置问题：
1. 表格知识库，索引列设置不准确。解决：需要把最接近用户提问内容的列设置为索引列；
2. 知识库召回设置为按需调用，但是没有在提示词中设置调用方式。解决：可以切换为自动调用，或者在提示词中添加关于调用方式的描述；
3. 知识库-最大召回数量，过大或过小。解决：可以先设置为5，然后根据对话效果逐步调整。
4. 知识库-最小匹配值，过大或过小。解决：可以先设置在0.2-0.4之间，然后根据实际对话效果进行调整。
5. 知识库-搜索策略，尝试切换模式进行测试，如果不确认可以先使用混合模式。
   5.1 语义模式：基于向量的文本相关性查询，推荐在需要理解语义关联度和跨语言查询的场景使用。
   5.2 全文模式：依赖于关键词的全文搜索，推荐在搜索具有特定名称、缩写词、短语或ID的场景使用。
   5.3 混合模式：结合全文检索与语义检索的优势，并对结果进行综合排序。

您还可以参考相关文档指南：
- 知识库介绍 https://www.Coze.cn/docs/guides/knowledge
- 知识库使用指南 https://www.Coze.cn/docs/guides/use_knowledge
- 知识库最佳实践 https://www.Coze.cn/docs/guides/product_knowledge_Bot
- 知识库社区案例 https://bytedance.larkoffice.com/docx/YNj2dVYo7o4L1Uxilq2c6A3Qnuc#JHLddkUVfos1QYxdc5Zcu1kgnmf
```

#### 知识库无法导入飞书文档

```
关于无法导入飞书在线文档的问题，可能是以下原因导致的：

- 您没有文档所有权（需要文档所有者才可以导入，管理者不行）
- 文档属于知识库/共享空间则无法进行导入，目前暂时只支持导入个人云空间下的文档
- 飞书文档由本地文档导入生成（暂时不支持此类文档）
- 文档权限设置，没有开启“允许内容被分享到组织外”
- 文档分享设置，没有开启“互联网获得链接的人可阅读”
- 扣子账号当前绑定的飞书账号，与您文档所归属的飞书账号不一致。您可以在扣子页面左下角点击头像-设置-数据源-授权（飞书）-点击获取当前应用-获取-授权并安装，然后返回扣子知识库导入页面，选择对应的飞书账号，导入文档。

如果您确认已满足以上要求，请提供知识库编辑页网址链接、相关飞书文档链接、相关截图，以便我们核实问题。
```

#### 希望Bot只引用知识库内容

```
Bot编排页-知识库调用设置-找到“回复”，选择开启“自定义Prompt”
```

![](img/0667ccda032359b9a015f3c7795618b3.png)

#### 上传表格，服务器处理一直进度为0

```
目前上传表格到扣子知识库时，需要满足单行全部内容不超过2000字符，每个单元格的内容不超过1024字符。
如果您的文件已满足要求，可以提供以下信息，我们会进行问题排查：

知识库页面的网址链接
所上传原始文件一份（仅用于测试）
上传过程logid

logid查询方式：
1.打开 Chrome 浏览器，进入扣子平台；
2.新建一个知识库，进入文件上传页面，按 F12 键打开 开发者工具，点击顶部的 “Network”，在开发中工具左上角输入框输入“creat”；
3.上传文件直到复现问题；
4.在开发者工具中，点击下方搜出的信息（creat?ms..），在右侧下滑查看X-Tt-Logid，将该串数字复制发送给扣子工作人员。
```

![](img/623e5f8f5761e5789cae9dac3840ca10.png)

### 图像流

```
图像流教程
官方教程 https://www.coze.cn/docs/guides/n1j2h6zx
社区案例 https://bytedance.larkoffice.com/docx/YNj2dVYo7o4L1Uxilq2c6A3Qnuc#AxU2dgxmaoKUu3x34T2cxcdwnid
```

### 卡片

#### 卡片教程

```
卡片教程
官方教程-卡片 https://www.coze.cn/docs/guides/message_card
社区案例 https://bytedance.larkoffice.com/docx/YNj2dVYo7o4L1Uxilq2c6A3Qnuc#R6kldymWaoS5puxwJK5cYlQ9n0e
```

#### 卡片绑定数组

```
需要在工作流代码节点中，点击数组变量并添加所有子节点，然后再进行卡片绑定。
```

![](img/97a7c7ce78e05edb628d17d75837f51d.png)

### 数据库

#### 数据库教程文档

```
官方文档：数据库 https://www.coze.cn/docs/guides/database
Coze工作流【数据库】节点教程 https://www.bilibili.com/read/cv35570626/
社区案例 https://bytedance.larkoffice.com/docx/YNj2dVYo7o4L1Uxilq2c6A3Qnuc#RWPjdXPvjo54v2xrlPYc4jbHnmg
```

#### 数据库查询/为什么在🛰️/豆包/飞书..记录了数据库，在扣子网页看不到对应记录

```
1. 在不同平台使用扣子，所存储的数据库记录是相互隔离的，无法跨平台查询。
1.1 在Bot调试页右上角“Memory”，可以查询在扣子网页端对话产生的数据库记录，无法查询用户在其他平台对话的数据库记录。
1.2 查询自己在豆包/公众号/飞书等等所有非扣子网页端对话产生的数据库记录，需要在对应的平台，以对话的形式进行数据库的读/写/修改/删除操作。

2. 如果您是想在同一个平台增删改查其他用户的数据库信息，需要满足以下两个条件：
2.1 数据库开启多用户模式
2.2 通过工作流来管理数据库信息 https://www.Coze.cn/docs/guides/database
```

#### 工作流试运行能查到数据，Bot里查不到 / Bot数据库有数据，工作流试运行查不到

```
工作流中的测试数据与 Bot 中的真实数据是隔离的。
1.在Bot调试页，无法调取工作流里的测试数据库信息。如果真实数据库为空，需要先插入数据后再进行查、删、改等操作。
2.在工作流中进行数据库节点测试时，无法调取Bot数据表中的真实数据。如果测试数据为空，需要先插入数据，再进行查、删、改等操作的测试。
```

#### 数组变量批量插入数据库

```
关于将数组变量批量插入数据库，您可以参考这条SQL语句：insert into table_name (col1,col2,col3) values (v1-1,v2-1,v3-1),(v1-2,v2-2,v3-2)

批量插入在数据库节点拼SQL会比较麻烦，建议您通过代码节点来拼SQL语句，然后传到数据库节点。关于代码节点的使用，您暂时可以参考工作流相关通用教程中的代码节点部分，后续我们会更新专门的数据库相关代码节点教程文档。
```

#### 数据库底层时间格式

```
数据库底层是用的 datetime 类型，带时分秒的。
```

### 工作流

#### 工作流相关教程

```
工作流相关教程：
工作流介绍 https://www.Coze.cn/docs/guides/workflow
使用工作流 https://www.Coze.cn/docs/guides/use_workflow
工作流最佳实践 https://www.Coze.cn/docs/guides/workflow_search_news
社区案例 https://bytedance.larkoffice.com/docx/YNj2dVYo7o4L1Uxilq2c6A3Qnuc#SDGYdohBpoLCdIxfbGEcheSYnBb
```

#### 提高工作流调用率

```
可以按照以下方法提高工作流调用率：
- 在提示词加强限制，要求 “每次用工作流处理所有用户对话”，不需要填写其他引导内容；
- 在工作流介绍中增加描述信息，帮助大模型理解和成功调用工作流。
- 工作流名称简洁明了。
```

![](img/694661e33279073b503098bc86a7c937.png)

#### 工作流超时限制

```
工作流单个节点有1分钟超时限制，工作流整体有2分钟超时限制，需要您根据要求调整配置，在限制的时间内完成运行。
```

#### 批处理参考文档

```
批处理参考文档 https://www.coze.com/docs/guides/llm_batch?_lang=zh

示例：以插件批处理为例，批处理的入参为实际的批处理入参，必须为数组；红框二为每次运行的入参，以这里为例，每次运行的入参是getNews引入的每一项item
```

![](img/a12d3bdda6c9e339bfab3208e6c3ec3f.png)

#### 如何直接输出工作流内容，不经过bot处理

```
可以在结束节点中，开启“使用设定的内容直接回答”
```

![](img/a33c29f9fe35caf70944f9d2f69dcaa2.png)

### 插件

#### 插件教程

```
插件相关教程

插件介绍 https://www.Coze.cn/docs/guides/create_plugin
教程：基于 API 创建一个插件 https://www.Coze.cn/docs/guides/plugin_example
基于已有服务创建插件 https://www.Coze.cn/docs/guides/services
使用 IDE 创建插件 https://www.Coze.cn/docs/guides/ide
通过 JSON 或 YAML 文件导入插件 https://www.Coze.cn/docs/guides/import
使用代码注册插件 https://www.Coze.cn/docs/guides/use_code
社区案例 https://bytedance.larkoffice.com/docx/YNj2dVYo7o4L1Uxilq2c6A3Qnuc#R7aRd9Gewo6SSCxu3LZc5wicnbF
```

#### 如何创建Oauth模式插件




# [Oauth模式插件教程文档](https://bytedance.larkoffice.com/docx/AIcddc7JLouM2bxvZ7pcSpsYncc)

## 自建的插件试运行正常，在工作流中报错

可能是以下原因导致的：

1. 插件服务本身运行不稳定，偶发报错；可以在插件试运行多尝试几次，进行优化调试；
2. 插件运行超过1分钟，会触发工作流节点超时限制，需要优化插件配置，保持能在1分钟内稳定运行。

如果确认满足以上要求，可以提供以下信息，我们会进行问题排查：

1. 插件编辑页面的网址链接
2. 插件编辑页面完整截图
3. 工作流运行插件报错的 `logid`
   - 打开 Chrome 浏览器，进入扣子平台
   - 打开遇到问题/报错的页面，按 F12 打开开发者工具，点击顶部的【Network】
   - 工作流中点击【试运行】直到复现问题
   - 查看 `old_test` header - `logid`

![](img/d702e379494adfcd5eed3c9bc0dcb079.png)

## 插件发布后可以调用，但是页面显示成功率为0%

插件需要上架商店后被使用，才会纳入成功率统计；
成功率是动态统计，当天的使用，最快需要第二天才会进入数据统计；

## 多Agent

### 多Agent跳转逻辑及优化

多Agent整体任务推进和节点切换，会由大模型根据Bot提示词、Agent提示词、Agent场景介绍信息，以及用户的输入内容，去尝试定义用户的使用场景，并调用适合的Agent来处理问题。

您可以参考多Agent文档对上述模块设置做整体优化：https://www.coze.cn/docs/guides/multiagent

1. 在Bot提示词中设置详细的整体定位、功能和执行流程（每一步流程由哪个Agent执行）
2. 在Agent适用场景中，详细介绍Agent的整体功能、定位和适用场景，帮助前序节点理解什么情况下应该切换到此节点。
   例：
   - 用于{功能}，帮助用户解决{场景}相关的问题；
   - 帮助用户解决健身相关的问题。
   - 搜索景点并制定旅行规划。
   - 根据用户要求生成图片。
3. 在Agent提示词中，详细说明当前Agent的运行逻辑和处理问题的步骤。
4. 在每个Agent中，设置“切换节点设置”，推荐“由独立于当前节点的模型识别”+“大模型语言”，并在 `Prompt` 中根据示例详细设置跳转逻辑。
5. 设置全局跳转调试：添加节点-全局跳转条件。
6. 在每个Agent中，尝试调整模型设置，降低生成随机性将可以让模型更严格遵守指令的要求（但会降低生成创新性）。
7. 在每个Agent中，尝试切换不同模型进行测试使用。不同模型对不同场景的理解能力会有差异，可以具体测试，选择最适合当前场景的模型进行使用。

![](img/003b4bcf59eb5b74ee0dc52a4cd25dc8.png)

![](img/045849fe052d694c016fea4f8712a261.png)

![](img/1678176607d5ba824035c9d98e092836.png)

### 多Agent教程

- 多Agent教程文档：官方文档：多 Agent 模式 https://www.Coze.cn/docs/guides/multiagent
- 使用Coze的muilt-agent驯服GPT-4：https://mp.weixin.qq.com/s/sjRFE44cjBfn4p3_3vv4vg
- Multi-agent mode 实践 | 用Coze手捏一个Bot：https://www.bilibili.com/video/BV1Ew4m1S7eq
- Multi-agent mode 实践 | 用Coze手捏一个Bot：https://mp.weixin.qq.com/s/YV7M7othSzMjKpm8xNHU-w
- 视频：分享一个在Coze上做提示词工程以及设计多智能体应用的最佳实践案例：https://www.bilibili.com/video/BV1MC411h7ok/
- 视频：【Agent共学第二期】Kai分享 | 一百个字搭谁是卧底-多Agent：https://www.bilibili.com/video/BV1Ew4m1S7eq/

## API

### API发布报错

发布API，需要先完成令牌创建。https://www.Coze.cn/open

如果发布API遇到报错，可以根据以下常见原因进行排查：

- 配置 `key` 的时候，权限需要覆盖当前 Bot 所在的空间；
- Bot 需要在自己所拥有的空间内创建，不要在他人空间内创建；
- 确保请求 header 中关于 body 格式的正确设置。`Content-Type: application/json`，注意大小写。
- 填写 `key` 和 `Botid` 时，不要添加`{{}}`

如果您确认满足以上条件，需要麻烦您在调用接口报错时，记录下接口的返回体 Header 中的 `x-tt-logid` 字段。提供该信息将有助于排查问题。在初次调用接口时，我们推荐使用接口调试工具（比如 Postman、Apifox 等）发送一次请求，帮助您快速调通接口。

如果您未记录请求 id 信息，需要麻烦您提供以下信息，以帮助我们定位问题：
请求的接口域名（比如 `api.Coze.com`）；
`Bot_id`；
调用接口传入的 `query`；

其他参考信息：
API 官方文档：https://www.Coze.cn/docs/developer_guides/Coze_api_overview

![](img/e056fef0a16ee6f509390b6c8fd9a25c.png)

### API是否支持输出语音

API能返回 `ogg` 格式的语音数据给用户，需用用户在自己的端内进行解析。

## 其他渠道

### 豆包

#### 我的豆包智能体没有上传图片、拍照功能

关于豆包智能体的拍照、上传图片等功能，豆包官方目前没有对用户开放这些功能的白名单申请，只定向邀请极个别智能体开通了该功能。关于何时开通该类功能的申请，您可以持续关注豆包官方的最新动态，也可以直接咨询豆包官方：
反馈邮箱：feedback@mail.doubao.com
APP反馈：豆包APP，点击“我的” -> 点击右上角的设置按钮 -> 找到“帮助与反馈”，进行反馈咨询。

#### 我的豆包智能体不能分享

只使用豆包模型+APP更新到最新版+等待豆包官方审核完成，就可以进行分享了

### 🛰️

#### 👸号/订阅号总是提示“继续”

可以参考这里进行优化。受限于微信平台有15秒回复的要求，无法完全避免“继续”的情况 https://www.Coze.cn/docs/guides/wechat_subscription

#### 发布服务号后无响应

很高兴为您提供协助。根据微信规则，目前服务号需要完成企业认证后才能发布 Bot 使用。刚发布后可能会有10分钟左右延迟。

发布时，配置步骤需要遵循：先配置扣子，然后配置公众号。复制粘贴公众号 `AppID` 等内容时，请注意仔细检查，确保没有多余空格。正确配置成功后，大概会有10分钟左右的延迟，然后就可以进行正常对话。

如果您的账号满足条件，且已完成正确配置。需要麻烦您提供下 Bot 编辑页面网址链接和服务号的 `AppID`（`wx` 开头的一段字符），我们的技术人员会进行问题排查。

#### 发布到🛰️客服，提示“回调域名失败”

目前需要完成企业认证后才可以发布到微信客服（还在认证中也无法进行发布）。您也可以先发布到订阅号进行使用，没有认证要求。订阅号发布教程：https://www.Coze.cn/docs/guides/wechat_subscription

#### 发布🛰️客服失败/无法使用

很高兴为您提供协助。关于您反馈的问题，可以参考以下信息：

1. 需要完成企业认证后才能发布到微信客服进行使用。若您没有完成认证，也可以先发布到订阅号进行使用（无认证要求）。订阅号发布教程：https://www.Coze.cn/docs/guides/wechat_subscription
2. 目前扣子支持发布到微信客服，暂未支持发布到企业微信客服。如果您之前的操作是发布企业微信，则可以在微信客服重新进行发布使用。
3. 如果您之前是发布微信客服遇到问题，则可以检查下复制相关字符内容时，是否存在多余空格。
4. Bot 发布会经过审核，如果还未通过审核，需要您耐心等待审核通过；如果您的发布申请被拒，需要您修改 Bot 配置后重新提交审核（审核规则参考：https://www.Coze.cn/docs/guides/content_principles）
5. Bot 成功发布到微信渠道后，可能会有10分钟左右延迟才会生效。
6. 如果您确认操作没有问题，可以提供 Bot编辑页面网址链接、微信企业ID（`ww` 开头的字符），我们会进行问题核查。

#### 如何让🛰️客服/👸号的bot直接显示图片，而不是url

需要通过提示词限制Bot在输出图片时不要输出其他文字内容，才可以直接显示图片。
因为微信的限制，bot每次只能回复1条消息，如果同时存在文字和图片，图片就只能以url的形式存在，否则就变成2条消息违反微信规则了。

### 飞书

#### Bot发布飞书后需要管理员审核，找不到审核的页面

可以在这个页面进行审核 `feishu.cn/admin/appCenter/audit`

### 发布小程序

#### bot发布页，无法绑定小程序

发布小程序需要满足以下要求：

1. 一个 Bot 只能发布到一个微信小程序。
2. 小程序主体类型为企业微信程序号。不支持个人主体的小程序。
3. 已完成微信小程序备案，备案流程可参考“微信开放平台文档” https://developers.weixin.qq.com/miniprogram/product/record/record_guidelines.html

更多内容，请参考教程文档：https://www.coze.cn/docs/guides/publish_to_wechat_app

#### 发布🛰️小程序，需要备案证明

可以在这里查看所需的备案编号等信息： https://www.coze.cn/docs/guides/terms-of-llm

### 抖音评论区

#### 发布到抖音评论区后，如何进行对话

在所发布的抖音账号的公开作品中，评论区直接评论作品，即可获得Bot回复；如果是回复其他人的评论，则需要@作者才可以获得回复。
目前Bot不会回复作者本人的评论，不会回复纯图片内容。
评论区Bot只会文字回复，无法生成图片内容。

## 二、logid查询

- [Bot 对话页](https://bytedance.larkoffice.com/docx/PTbYd85sHonfzTxZxx3c2lx0nFe#doxcnNHrxaAkd7NMhw98AjNRIvc)
- [工作流、图像流](https://bytedance.larkoffice.com/docx/PTbYd85sHonfzTxZxx3c2lx0nFe#doxcnOhe7HcSH97x7EPIkTXOoEc)
- [知识库](https://bytedance.larkoffice.com/docx/PTbYd85sHonfzTxZxx3c2lx0nFe#doxcnRL8EpXaEnMthmcwTFaSZ6d)
- [插件](https://bytedance.larkoffice.com/docx/PTbYd85sHonfzTxZxx3c2lx0nFe#doxcnjEFdeNhFMxW2JSjlyN7LEf)

## Bot 对话页

### 调试页面-logid

辛苦查询 `logid` 信息，复制发送给工作人员：

`logid` 查询方式：
1. 在扣子官网打开对应Bot的编辑页面，在对话框内提问直到复现问题
2. 点击页面右上角“调试”-复制 `logid` 信息发送给工作人员

![](img/46c69616cfa3e931c6efd533fa694792.png)

### 商店对话页logid

`logid` 查询方式：
- Chrome 浏览器打开Bot的商店页面
- 按 F12 打开开发者工具，点击顶部的 "Network"
- Bot 对话框内提问，直到复现问题
- 在开发者工具中，查看 `chat` 接口请求头-`logid`

![](img/69d2e1c0526edd86948bfb1def08c0a5.png)

## 工作流、图像流

### 工作流-logid

`logid` 查询方式：
1. 打开 Chrome 浏览器，进入扣子平台
2. 打开遇到问题/报错的页面，按 F12 打开开发者工具，点击顶部的【Network】
3. 工作流中点击【试运行】直到复现问题
4. 查看 `old_test` header - `logid`

![](img/b1d14fdfc4c60315cec4755f4f38137f.png)

### 图像流-logid

`logid` 查询方式：
1. 打开 Chrome 浏览器，进入扣子平台
2. 打开遇到问题/报错的页面，按 F12 打开开发者工具，点击顶部的【Network】
3. 图像流中点击【试运行】直到复现问题
4. 查看 `old_test` header - `logid`

![](img/2f577b3767a8c1a93865a3a185792102.png)

## 知识库

### 知识库无法上传-logid

`logid` 查询方式：
1. 打开 Chrome 浏览器，进入扣子平台；
2. 新建一个知识库，进入文件上传页面先不要上传，按 F12 键打开开发者工具，点击顶部的 "Network"；
3. 上传PDF文件直到复现问题；
4. 在开发者工具中，在左上角输入框输入“`upload`”，点击下方搜出的信息，在右侧下滑查看`X-Tt-Logid`，将该串数字复制发送给扣子工作人员。

![](img/46cabd5fe0b9a992634096aa1182b968.png)

### 知识库无法分片-logid

`logid` 查询方式：
1. 打开 Chrome 浏览器，进入扣子平台；
2. 新建一个知识库，进入文件上传页面，按 F12 键打开开发者工具，点击顶部的 "Network"，在开发中工具左上角输入框输入“`creat`”；
3. 上传文件直到复现问题；
4. 在开发者工具中，点击下方搜出的信息（`creat?ms..`），在右侧下滑查看`X-Tt-Logid`，将该串数字复制发送给扣子工作人员。

![](img/c82990e0b606a1d1.png)

## 插件

### 插件发布异常-logid

`logid` 查询方式：
1. 打开 Chrome 浏览器，进入扣子平台，打开相关插件详情页面
2. 按 F12 键打开开发者工具，点击顶部的 "Network"，在右侧输入框中，输入“`publish`”
3. 点击插件发布按钮，进行发布操作
4. 在开发者工具中，点击下方搜出的“`publish_pligin`”信息，在右侧下滑查看`X-Tt-Logid`，将该串数字复制发送给扣子工作人员。

![](img/cf7845363b5603c4071bcd0c3ea3998f.png)

## 查询Bot发布状态

![](img/cd79abb2d7736209ed01acd252bbd3bd.png)

### 工作流试运行能查到数据，Bot里查不到 / Bot数据库有数据，工作流试运行查不到

工作流中的测试数据与 Bot 中的真实数据是隔离的。

1. 在Bot调试页，无法调取工作流里的测试数据库信息。如果真实数据库为空，需要先插入数据后再进行查、删、改等操作。
2. 在工作流中进行数据库节点测试时，无法调取Bot数据表中的真实数据。如果测试数据为空，需要先插入数据，再进行查、删、改等操作的测试。

### 数组变量批量插入数据库




关于[将数组变量批量插入数据库](https://www.coze.cn/docs/guides/use_workflow)，您可以参考这条SQL语句：

```sql
insert into table_name (col1,col2,col3) values (v1-1,v2-1,v3-1),(v1-2,v2-2,v3-2)
```

批量插入在数据库节点拼SQL会比较麻烦，建议您通过代码节点来拼SQL语句，然后传到数据库节点。关于代码节点的使用，您暂时可以参考工作流相关通用教程中的代码节点部分，后续我们会更新专门的数据库相关代码节点教程文档。

### 数据库底层时间格式

数据库底层是用的 `datetime` 类型，带时分秒的。

## 工作流

### 工作流相关教程

工作流相关[教程](https://bytedance.larkoffice.com/docx/YNj2dVYo7o4L1Uxilq2c6A3Qnuc#SDGYdohBpoLCdIxfbGEcheSYnBb)：

- 工作流介绍：https://www.Coze.cn/docs/guides/workflow
- 使用工作流：https://www.Coze.cn/docs/guides/use_workflow
- 工作流最佳实践：https://www.Coze.cn/docs/guides/workflow_search_news
- 社区案例：https://bytedance.larkoffice.com/docx/YNj2dVYo7o4L1Uxilq2c6A3Qnuc#SDGYdohBpoLCdIxfbGEcheSYnBb

### 提高工作流调用率

可以按照以下方法提高工作流调用率：

- 在提示词加强限制，要求“每次用工作流处理所有用户对话”，不需要填写其他引导内容；
- 在工作流介绍中增加描述信息，帮助大模型理解和成功调用工作流。
- 工作流名称简洁明了。

![](img/26899cb576f8fd2682452a4b96253ba1.png)

### 工作流超时限制

工作流单个节点有1分钟超时限制，工作流整体有2分钟超时限制，需要您根据要求调整配置，在限制的时间内完成运行。

### 批处理参考文档

批处理参考文档：[https://www.Coze.com/docs/guides/llm_batch?_lang=zh](https://www.Coze.com/docs/guides/llm_batch?_lang=zh)

示例：以插件批处理为例，批处理的入参为实际的批处理入参，必须为数组；红框二为每次运行的入参，以这里为例，每次运行的入参是 `getNews` 引入的每一项 `item`。

![](img/bcf3cde1b7e730dce0d3c72ddced7213.png)

### 如何直接输出工作流内容，不经过bot处理

可以在结束节点中，开启“使用设定的内容直接回答”。

![](img/ff6ca66fa0795c019c75e2fcfaa5dae7.png)

## 插件

### 插件教程

插件相关教程：

- 插件介绍：https://www.Coze.cn/docs/guides/create_plugin
- 教程：基于 API 创建一个插件：https://www.Coze.cn/docs/guides/plugin_example
- 基于已有服务创建插件：https://www.Coze.cn/docs/guides/services
- 使用 IDE 创建插件：https://www.Coze.cn/docs/guides/ide
- 通过 JSON 或 YAML 文件导入插件：https://www.Coze.cn/docs/guides/import
- 使用代码注册插件：https://www.Coze.cn/docs/guides/use_code
- 社区案例：https://bytedance.larkoffice.com/docx/YNj2dVYo7o4L1Uxilq2c6A3Qnuc#R7aRd9Gewo6SSCxu3LZc5wicnbF

### 如何创建Oauth模式插件

Oauth模式插件教程文档：[https://bytedance.larkoffice.com/docx/AIcddc7JLouM2bxvZ7pcSpsYncc](https://bytedance.larkoffice.com/docx/AIcddc7JLouM2bxvZ7pcSpsYncc)

### 自建的插件试运行正常，在工作流中报错

可能是以下原因导致的：

1. 插件服务本身运行不稳定，偶发报错；可以在插件试运行多尝试几次，进行优化调试；
2. 插件运行超过1分钟，会触发工作流节点超时限制，需要优化插件配置，保持能在1分钟内稳定运行。

如果确认满足以上要求，可以提供以下信息，我们会进行问题排查：

1. 插件编辑页面的网址链接
2. 插件编辑页面完整截图
3. 工作流运行插件报错的 `logid`

- 打开 Chrome 浏览器，进入扣子平台
- 打开遇到问题/报错的页面，按 F12 打开开发者工具，点击顶部的【Network】
- 工作流中点击【试运行】直到复现问题
- 查看 `old_test` header - `logid`

![](img/781a6bd4091fd2d685e3d16e524d075f.png)

### 插件发布后可以调用，但是页面显示成功率为0%

插件需要上架商店后被使用，才会纳入成功率统计；
成功率是动态统计，当天的使用，最快需要第二天才会进入数据统计。

## 多Agent

### 多Agent跳转逻辑及优化

[多Agent整体任务推进和节点切换](https://www.coze.cn/docs/guides/multiagent)会由大模型根据Bot提示词、Agent提示词、Agent场景介绍信息，以及用户的输入内容，去尝试定义用户的使用场景，并调用适合的Agent来处理问题。

您可以参考多Agent文档对上述模块设置做整体优化：https://www.coze.cn/docs/guides/multiagent

1. 在Bot提示词中设置详细的整体定位、功能和执行流程（每一步流程由哪个Agent执行）

2. 在Agent适用场景中，详细介绍Agent的整体功能、定位和适用场景，帮助前序节点理解什么情况下应该切换到此节点。
例：
- 用于 `{功能}`，帮助用户解决 `{场景}` 相关的问题；
- 帮助用户解决健身相关的问题。
- 搜索景点并制定旅行规划。
- 根据用户要求生成图片。

3. 在Agent提示词中，详细说明当前Agent的运行逻辑和处理问题的步骤。

4. 在每个Agent中，设置“切换节点设置”，推荐“由独立于当前节点的模型识别”+“大模型语言”，并在“Prompt”中根据示例详细设置跳转逻辑。

5. 设置全局跳转调试：添加节点-全局跳转条件。

6. 在每个Agent中，尝试调整模型设置，降低生成随机性将可以让模型更严格遵守指令的要求（但会降低生成创新性）。

7. 在每个Agent中，尝试切换不同模型进行测试使用。不同模型对不同场景的理解能力会有差异，可以具体测试，选择最适合当前场景的模型进行使用。

![](img/e191667ee013b7c58408242aa7b3b1b0.png)

### 多Agent教程

- [多Agent教程文档](https://www.coze.cn/docs/guides/multiagent)
- 官方文档：多 Agent 模式 https://www.Coze.cn/docs/guides/multiagent
- 使用Coze的muilt-agent驯服GPT-4：https://mp.weixin.qq.com/s/sjRFE44cjBfn4p3_3vv4vg
- Multi-agent mode实践 | 用Coze手捏一个Bot：https://mp.weixin.qq.com/s/YV7M7othSzMjKpm8xNHU-w
- 视频：分享一个在Coze上做提示词工程以及设计多智能体应用的最佳实践案例：https://www.bilibili.com/video/BV1MC411h7ok/
- 视频：【Agent共学第二期】Kai分享 | 一百个字搭谁是卧底-多Agent：https://www.bilibili.com/video/BV1Ew4m1S7eq/

## API

### API发布报错

发布API，需要先完成令牌创建：https://www.Coze.cn/open

如果发布API遇到报错，可以根据以下常见原因进行排查：

- 配置 `key` 的时候，权限需要覆盖当前Bot所在的空间；
- Bot需要在自己所拥有的空间内创建，不要在他人空间内创建；
- 确保请求 header 中关于 body 格式的正确设置。`Content-Type: application/json`，注意大小写。
- 填写 `key` 和 `Bot_id` 时，不要添加 `{{}}`

如果您确认满足以上条件，需要麻烦您在调用接口报错时，记录下接口的返回体 Header 中的 `x-tt-logid` 字段。提供该信息将有助于排查问题。在初次调用接口时，我们推荐使用接口调试工具（比如 Postman、Apifox 等）发送一次请求，帮助您快速调通接口。

如果您未记录请求 id 信息，需要麻烦您提供以下信息，以帮助我们定位问题：

- 请求的接口域名（比如 api.Coze.com）；
- `Bot_id`；
- 调用接口传入的 `query`；

其他参考信息：
API 官方文档：https://www.Coze.cn/docs/developer_guides/Coze_api_overview

![](img/3de0b021850f7ad609540305cee8d1f7.png)

### API是否支持输出语音

API能返回ogg格式的语音数据给用户，需用用户在自己的端内进行解析。

## 其他渠道

### 豆包

#### 我的豆包智能体没有上传图片、拍照功能

关于豆包智能体的拍照、上传图片等功能，豆包官方目前没有对用户开放这些功能的白名单申请，只定向邀请极个别智能体开通了该功能。关于何时开通该类功能的申请，您可以持续关注豆包官方的最新动态，也可以直接咨询豆包官方：

反馈邮箱：feedback@mail.doubao.com
APP反馈：豆包APP，点击“我的” -> 点击右上角的设置按钮 -> 找到“帮助与反馈”，进行反馈咨询。

#### 我的豆包智能体不能分享

只使用豆包模型+APP更新到最新版+等待豆包官方审核完成，就可以进行分享了

### 🛰️

#### 👸号/订阅号总是提示“继续”

可以参考这里进行优化。受限于微信平台有15秒回复的要求，无法完全避免“继续”的情况：https://www.Coze.cn/docs/guides/wechat_subscription

#### 发布服务号后无响应

很高兴为您提供协助。根据微信规则，目前服务号需要完成企业认证后才能发布 Bot 使用。刚发布后可能会有10分钟左右延迟。

发布时，配置步骤需要遵循：先配置扣子，然后配置公众号。复制粘贴公众号 `AppID` 等内容时，请注意仔细检查，确保没有多余空格。正确配置成功后，大概会有10分钟左右的延迟，然后就可以进行正常对话。

如果您的账号满足条件，且已完成正确配置。需要麻烦您提供下 Bot 编辑页面网址链接和服务号的 `AppID`（`wx` 开头的一段字符），我们的技术人员会进行问题排查。

#### 发布到🛰️客服，提示“回调域名失败”

目前需要完成企业认证后才可以发布到微信客服（还在认证中也无法进行发布）。您也可以先发布到订阅号进行使用，没有认证要求。订阅号发布教程：https://www.Coze.cn/docs/guides/wechat_subscription

#### 发布🛰️客服失败/无法使用

很高兴为您提供协助。关于您反馈的问题，可以参考以下信息：

1. 需要完成企业认证后才能发布到微信客服进行使用。若您没有完成认证，也可以先发布到订阅号进行使用（无认证要求）。订阅号发布教程：https://www.Coze.cn/docs/guides/wechat_subscription
2. 目前扣子支持发布到微信客服，暂未支持发布到企业微信客服。如果您之前的操作是发布企业微信，则可以在微信客服重新进行发布使用。
3. 如果您之前是发布微信客服遇到问题，则可以检查下复制相关字符内容时，是否存在多余空格。
4. Bot 发布会经过审核，如果还未通过审核，需要您耐心等待审核通过；如果您的发布申请被拒，需要您修改 Bot 配置后重新提交审核（审核规则参考：https://www.Coze.cn/docs/guides/content_principles）
5. Bot 成功发布到微信渠道后，可能会有10分钟左右延迟才会生效。
6. 如果您确认操作没有问题，可以提供 Bot 编辑页面网址链接、微信企业ID（`ww` 开头的字符），我们会进行问题核查。

#### 如何让🛰️客服/👸号的bot直接显示图片，而不是url

需要通过提示词限制Bot在输出图片时不要输出其他文字内容，才可以直接显示图片。
因为微信的限制，bot每次只能回复1条消息，如果同时存在文字和图片，图片就只能以url的形式存在，否则就变成2条消息违反微信规则了。

### 飞书

#### Bot发布飞书后需要管理员审核，找不到审核的页面

可以在这个页面进行审核：https://feishu.cn/admin/appCenter/audit

### 发布小程序

#### bot发布页，无法绑定小程序

发布小程序需要满足以下要求：

1. 一个 Bot 只能发布到一个微信小程序。
2. 小程序主体类型为企业微信程序号。不支持个人主体的小程序。
3. 已完成微信小程序备案，备案流程可参考“微信开放平台文档”：https://developers.weixin.qq.com/miniprogram/product/record/record_guidelines.html

更多内容，请参考教程文档：https://www.coze.cn/docs/guides/publish_to_wechat_app

#### 发布🛰️小程序，需要备案证明

可以在这里查看所需的备案编号等信息：https://www.coze.cn/docs/guides/terms-of-llm

### 抖音评论区

#### 发布到抖音评论区后，如何进行对话

在所发布的抖音账号的公开作品中，评论区直接评论作品，即可获得Bot回复；如果是回复其他人的评论，则需要@作者才可以获得回复。
目前Bot不会回复作者本人的评论，不会回复纯图片内容。
评论区Bot只会文字回复，无法生成图片内容。

## 二、logid查询

- [Bot 对话页](https://bytedance.larkoffice.com/docx/PTbYd85sHonfzTxZxx3c2lx0nFe#doxcnNHrxaAkd7NMhw98AjNRIvc)
- [工作流、图像流](https://bytedance.larkoffice.com/docx/PTbYd85sHonfzTxZxx3c2lx0nFe#doxcnOhe7HcSH97x7EPIkTXOoEc)
- [知识库](https://bytedance.larkoffice.com/docx/PTbYd85sHonfzTxZxx3c2lx0nFe#doxcnRL8EpXaEnMthmcwTFaSZ6d)
- [插件](https://bytedance.larkoffice.com/docx/PTbYd85sHonfzTxZxx3c2lx0nFe#doxcnjEFdeNhFMxW2JSjlyN7LEf)

## Bot 对话页

### 调试页面 - logid

辛苦查询 `logid` 信息，复制发送给工作人员：

`logid` 查询方式：
1. 在扣子官网打开对应Bot的编辑页面，在对话框内提问直到复现问题
2. 点击页面右上角“调试”-复制 `logid` 信息发送给工作人员

![](img/05050066a4c58b9aa274f137e8f5d452.png)

### 商店对话页logid

`logid` 查询方式：

- Chrome 浏览器打开Bot的商店页面
- 按 F12 打开开发者工具，点击顶部的"Network"
- Bot 对话框内提问，直到复现问题
- 在开发者工具中，查看 `chat` 接口请求头-`logid`

![](img/90edca6d93aeeabc67a812f307de6fdc.png)



## 工作流、图像流

### 工作流-logid

```
logid查询方式：
1.打开 Chrome 浏览器，进入扣子平台
2.打开遇到问题/报错的页面，按 F12 打开开发者工具，点击顶部的【Network】
3.工作流中点击【试运行】直到复现问题
4.查看old_test header - logid
```

![](img/f339c78114872f9c36a5dd9726c7dd4b.png)

### 图像流-logid

```
logid查询方式：
1.打开 Chrome 浏览器，进入扣子平台
2.打开遇到问题/报错的页面，按 F12 打开开发者工具，点击顶部的【Network】
3.图像流中点击【试运行】直到复现问题
4.查看old_test header - logid
```

![](img/b6ba5e179818e8ed8eaab671dd3a7b6c.png)

## 知识库

### 知识库无法上传-logid

```
logid查询方式：
1.打开 Chrome 浏览器，进入扣子平台；
2.新建一个知识库，进入文件上传页面先不要上传，按 F12 键打开 开发者工具，点击顶部的 "Network"；
3.上传PDF文件直到复现问题；
4.在开发者工具中，在左上角输入框输入“upload”，点击下方搜出的信息，在右侧下滑查看X-Tt-Logid，将该串数字复制发送给扣子工作人员。
```

![](img/767e86bdaab6579455061c54e167b8b6.png)

### 知识库无法分片-logid

```
logid查询方式：
1.打开 Chrome 浏览器，进入扣子平台；
2.新建一个知识库，进入文件上传页面，按 F12 键打开 开发者工具，点击顶部的 “Network”，在开发者工具左上角输入框输入“creat”；
3.上传文件直到复现问题；
4.在开发者工具中，点击下方搜出的信息（creat?ms..），在右侧下滑查看X-Tt-Logid，将该串数字复制发送给扣子工作人员。
```

![](img/f01bc7995ad2e26e0d75fcc253f32bc0.png)

## 插件

### 插件发布异常-logid

```
logid查询方式：
1.打开 Chrome 浏览器，进入扣子平台，打开相关插件详情页面
2、3.按 F12 键打开 开发者工具，点击顶部的 “Network”，在右侧输入框中，输入“publish”
4.点击插件发布按钮，进行发布操作
5、6.在开发者工具中，点击下方搜出的“publish_pligin”信息，在右侧下滑查看X-Tt-Logid，将该串数字复制发送给扣子工作人员。
```

![](img/8c97faf33e041668e4ba13a9c912cdf3.png)

## 查询Bot发布状态

![](img/1197d7ceb39e6ed1811f7565606cc107.png)
