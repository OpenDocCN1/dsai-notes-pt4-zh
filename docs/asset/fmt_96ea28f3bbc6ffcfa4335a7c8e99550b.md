
# 搭积木：n8n+飞书多维表格=免费自动reddit需求挖掘机+产品开发文档（附模板）

> 来源：[https://yunyinghui.feishu.cn/docx/GK7Hd57TQoiKkvxAUZFc0fJQnqf](https://yunyinghui.feishu.cn/docx/GK7Hd57TQoiKkvxAUZFc0fJQnqf)

## 前言

随着 vibe coding 的技术进步，做 web 出海的朋友越来越多了，如何挖掘真需求一直是个头疼的问题，而 reddit 被誉为出海金矿，可以直接在上面挖掘海外用户的真实需求。

本文将教会你：

搭建一个自动需求挖掘机，只需设定好搜索关键词，每天自动收集 reddit 上的用户需求到多维表格，并且可以自动分析，呈现出可视化商业分析报告和产品开发文档（PRD, Product Requirements Document），可以直接丢给 cursor、claude code、v0 等等 vibe coding 工具直接开发，节省 mvp 产出时间。

其实已经有圈友在赚钱发过 n8n 挖掘 reddit 需求相关的帖子了，本文在整合这些工作流基础上添加进了飞书多维表格利用自带 ai（deepseek）来处理数据的逻辑。

文章中直接给出 n8n 工作流源文件和飞书多维表格模板，可以直接上手。

废话不多说，直奔主题，阅读本篇文章，你将获得：

## 效果展示

### 1、输入

自定义多维表格输入界面可以自定义关键词，reddit 小组，每个搜索请求（每行）要设一个 `id`

![](img/ea0470a3ec94822b96544c0395d19d82.png)

### 输出结果

可以每天自动定时抓取 reddit 标题、正文、原贴链接、发布时间、小组

![](img/02831ac789f2e51bf9b1388b233cf3df.png)

![](img/42b185402633197b354892384df850ae.png)

多维表格自带的字段捷径可以帮我们生成：

### 1、帖子内容中文总结

![](img/735cebacb35479f4cfab657ae0fbb152.png)

### 2、deepseek 商业分析报告（可视化网页版）

![](img/03a9e52774179d6abd3d643f8e2aff48.png)

直接可以点击生成网页版可视化商业分析报告

![](img/6647208b3358c73cd546526f0b0ba6d1.png)

### 3、直接可用的产品开发文档

（丢给 vibe coding 工具，可以马上直接做出 mvp）

![](img/36b596f2b855683c7d87351dc8b160ca.png)

[`产品开发文档.md`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LRXdbXZd4op0GkxUDIhcQHqunYc)

## 操作步骤

### 1、n8n 部署

首先，n8n 的部分记得开魔法

我这里用的是 clawcloudrun，注册超过 180 天的 github 账号可以直接白嫖每月 5 美元的额度

官网地址：

https://ap-northeast-1.run.claw.cloud/signin

![](img/5e61da907e97edd3eaedfb44028e6018.png)

![](img/7793cb872794137952ef2d072621de06.png)

github 账号登录即可，如果没有老 github 号，只能首月 5 美元，可以换号白嫖，不过要重新部署了

![](img/b4f0047829e4f9351a67e78c99300f81.png)

这里选的美国西区，可能 reddit 比较稳定

![](img/6ff4938ef7bd60fc449bb704fb9f8805.png)

搜索 n8n

![](img/e37e3db57f632e5d82e3ac97ac2a17c0.png)

点 `deploy` 部署，每天 0.1 美金，一个月正好 3 美金，赠款 5 美金绰绰有余了

![](img/b4eb256df3e89dd1f285afbb22460526.png)

部署完之后桌面找到部署好的 n8n，打开

![](img/189d461a4b4881bcc21fa389bdd51775.png)

n8n 新账号要注册

![](img/ac28402c7d37cfd6851e6124c6504f1e.png)

注册完以后点击 `setting`，先安装飞书 mcp

![](img/446470457af357d7568f4f3f07f72c0f.png)

点最下面 community nodes

![](img/9fa12265427dfa1a56ec910c83c0eca0.png)

点击 `Install`，填写我们要安装的界面名称，

```
n8n-nodes-feishu-lite
```

![](img/67c8704651185cb3f672af3bd1b6c9db.png)

然后再点击 Install，完成

回到主界面，点击 `import from file`，下载我已经配置好的工作流（见下方）

![](img/6d255a4b68e168167378b1ba037b1195.png)

导入文件

![](img/e06c14a0a5b5f6dd11b01f050d36bafd.png)

这样 n8n 部署完成

### 2、模板下载

#### 1、已配置好的 n8n 工作流

![](img/363bc298f9483e6f14dc78464a5afe23.png)

[`n8n_feishu.json`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GK5wblCn3o2SssxakkfcdA0mnDc)

#### 2、飞书多维表格模板，右上角使用

链接：

[n8n-reddit](https://quih408vgi.feishu.cn/base/FcDcbNiCPaYBTCsCNY1cdKjcnsb?table=tblrkibIaQ7vCDM3&view=vew3B1iLoW)

![](img/b1cb809c72c06d6d781a8579c89b6c0d.png)

### 3、n8n 工作流程

整体流程如下

![](img/dfc3239e5306e56dbff0431e043c5614.png)

#### 1、自动触发工作流

首先右上角 `settings` 修改时区，改成国内时间

![](img/ca02ccecdfa622af466a8f697f15ed77.png)

这里是 shanghai

![](img/fdc2bd2ebd697fea743aa09482f35697.png)

双击第一个节点

![](img/29f03cafa8c253e00d2242b0be2b6e86.png)

![](img/341d4181c75e8c3d7b01bec0e0dbb53f.png)

设置定时触发时间，我这里是每天凌晨 12 点自动运行，想要更短的时间间隔可以自行修改

#### 2、解析飞书表格

双击解析飞书表格地址

![](img/72d145ddd2f910f935f404b4912589b4.png)

飞书的认证，先填入最下方飞书表格链接（用我的模板之后新标签页打开），再点 `create new credential`，

![](img/20c0aad8d1b6a4a6bc5d41dc8c24e6d4.png)

接下来进行认证

![](img/bcbd3c866f4b511be4b97487e873e29d.png)

飞书应用的认证密钥获取，

官网

https://open.feishu.cn/app

创建企业自建应用

![](img/a2b331c93bf25f81417befc44dd17547.png)

输入名称和描述（任意）

![](img/803c614931591f459a35c792d4e9bbb7.png)

记得创建版本（随便填写）

![](img/52bb2186657158d26c3a27312f72b531.png)

![](img/ddce7799f8137b2b4be67aba48cf4000.png)

拉到下面保存-确认发布

点击凭证与基础信息，就能获得 `app id` 和 `key`

![](img/32080f4066f01f54eb6f35990373db6f.png)

复制填入

![](img/974d3a982c806b580ff43ac256946d69.png)

回到飞书应用，设置权限，点权限管理

![](img/8a535fc28ab33a849e987aaef23241f3.png)

开通权限，这里我全都开了

![](img/635814720f61ea151f217b68c8fa6a75.png)

直接全选开通

回到多维表格，右上角三个点添加文档应用

![](img/b22cdc8c2ae8f437929943dd7c82d891.png)

添加文档应用

![](img/7298b7f4aaeca3ac53d49930c74b126d.png)

输入你刚刚创建好的应用名字，可编辑权限打开（一定要可编辑）

![](img/1fc42f0b001f9135618dc2f4f99b1b4e.png)

#### 3、获取多维表格元数据

双击

![](img/b762be2588c08cced1d67a72773c709d.png)

把前面输出的 app token 拖进来

![](img/7d065d47b6fd5960a5f1d78cf9d8e97e.png)

![](img/24fbb2474beb5505bf1c72e989386c37.png)

点 `execute step`

![](img/7c9e0ace63233e0b62ffc312b224f355.png)

表示成功了

#### 4、读取数据表

接下来双击

![](img/f9ea1642055bc65237dc83d8fabbd5eb.png)

填入

![](img/15554aa1bbf3a8da4261d6dda87f8ada.png)

下一个节点是数据格式修改，把数据分开的作用

![](img/5cd232cbfb35a39c370a905ff562ebc9.png)

#### 5、reddit 输入

设置输入给 reddit 的搜索参数：

![](img/003685797dd95b3bd6caee579a1ef89e.png)

#### 6、reddit 节点

![](img/5d2a4e2ac7ede7850800b3f9fec90525.png)

认证密钥部分：

![](img/786ac8082b098d8641732257d9f70b0b.png)

[如何免费自动化寻找蓝海市场：用AI Agent+N8N工作流在Reddit上发现真实痛点](https://eafz7mo3of.feishu.cn/docx/Y2kkdmdPFolDJlxkLlKcY4Sunjd)@达轮 的这篇帖子写的非常详细了，照做就行

完成认证之后，把搜索的参数给到，（可以选搜索帖子的数量，模板里面默认是 5 篇）

![](img/be094e7347a4f304c3546e1c29e24544.png)

`Return all` 是全部符合的帖子都返回，打开会很慢，`limit` 下面就是返回帖子的数量

![](img/7cde81ff93bea33317f839c3cfeb8004.png)

#### 7、筛选帖子

帖子太多了，只保留特定条件的

![](img/3838e2ba9336e08a62c847d736429ee3.png)

这里筛选，参数都可以自己设置，

分别为，点赞>2，内容不为空，时间 365 天内

#### 8、选择保留的参数

也是参数太多了，只保留几个有用的：

点赞数、标题、正文、帖子链接、时间戳

![](img/37d624c8c41eaa5662b4550c9308baea.png)

![](img/2c43d27069bcf095edc03420bf85cc4f.png)

#### 9、AI 判断这个帖子是否是个需求

双击

![](img/2a4a6523a2d1fcdcb63a89a5edecbbe2.png)

![](img/17fce0f82f964d1a6be810811a42edd1.png)

这里的提示词填入：（这里用的圈友@四喜 的提示词）

```
decide whether this Reddit post is a business-related problem or a need for a solution or already a solution. The post should mention a specific challenge or requirement that a business is trying to address.​Reddit Post: ​{{ $json.selftext }}

##Output Format​ ￼
Return Boolean output​If the answer is affirmative, return Yes, if the output is negative, return No
只输出 yes 或者 no
```

填写自己的 deepseek apikey

![](img/27312d35aa4a39f8410b4e422e43adcc.png)

#### 10、整合

把 AI 返回的 yes/no，跟有用信息整合在一起

#### 11、过滤

只有 ai 判断为 `yes` 的帖子保留

![](img/712ef876a866df01e4e921fc27710de5.png)

#### 12、导出数据，写入到飞书多维表格

最后一个节点

![](img/4217f33cf49c2db154defa18c0a4376c.png)

这里填写的是【搜索结果】这个飞书子表格的 `tableid`，一定要认真核对

![](img/30690654417f22bc5a6ce73c4c6f1b7b.png)

![](img/e739bff845d97223de250f0412b60893.png)

![](img/a43023bbe108503fff69b5830dde705a.png)

`table=【】&view`，这一串就是【搜索结果】子表格 `tableid`，填进去

至此，n8n 工作流搭建完成

### 4、飞书多维表格部分

这部分就简单了，都是用的自带的字段捷径

#### 1、总结字段

主要是方便自己看的作用，我的提示词比较简单，直接用的飞书多维表格预设的总结，你也可以用 deepseek 字段，自己设置提示词

![](img/5c41b973d21e4d9b8ff00760026bb20c.png)

总结效果如下：

![](img/3fce48002122a2e293d45f6a9152700f.png)

#### 2、商业分析字段

![](img/25751cffd9c12f7a3d62a2f0d96e0b95.png)

![](img/f489ee9b0c1cbdc539c207921aad0cb5.png)

采用的是@饼干哥哥 的提示词：记得替换黄底字为实际引用
