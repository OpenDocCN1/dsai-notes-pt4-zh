# 零代码打造飞书热点新闻助手

> 来源：[https://yunyinghui.feishu.cn/docx/OSn0dLeAPoShxuxDkUNcvlp0nyd](https://yunyinghui.feishu.cn/docx/OSn0dLeAPoShxuxDkUNcvlp0nyd)

---

**n8n 零坑保姆级教程（网上大部分教程参**差不齐，坑多填少，实际真实运行成功率低）

**一**个工作流同时满足：每天 8 点从多个科技平台进行前沿科技新闻的录入（新闻标题、时间、简要、具体内容），并提炼 5 条新闻，进行群机器人推送。

![](img/2fc6f3ffa45e26d739f4ff2dfcbbe148.png)

[NVIDIA_Overlay_0twniQEQ3I.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CyVSb0ZXhoeXDKxEJ8gcMwaQnff)

# 课前准备工作

1.  **确认 windows 的版本：**

按 Win + R 输入 winver，查看系统版本，如果是家庭版不支持原生 Hyper-V 需要另行手动部署，专业版/企业版/教育版等直接通过系统启动即可：

![](img/9b79cb39e205d88aa096327f4af1cd27.png)

启用方式：搜索启用或关闭 windows 功能，勾选 Hyper-V 和虚拟机平台并重启电脑。

![](img/28408112848f9d5c537ca687ae647340.png)

![](img/5727cdf0aed309cc9b47a01d76d7f2dc.png)

1.  **安装 docker：**

下面主要是为了部署 n8n 的本地端，这里介绍最稳定的方式——docker 的 n8n 本地部署方式

进入[ docker 的官网](https://www.docker.com/get-started/)：选择对应的版本

![](img/1f686b1a5b8a3ebaadbe3867adce315c.png)

安装后，注册时需要使用科学上网。如果出现以下界面，这个时候 windows subsystem for linux 版本太旧，需要更新。

![](img/3a91ef79fc4b9171f7da0c509c771fb7.png)

复制 docker 中的提示代码，并打开 cmd，进行下载。

![](img/a9540eeef96a75eee41ca553aae25b68.png)

安装后，打开设置：

![](img/d97500e95bccd1ecff7af6dd837bf21c.png)

选择指定位置，以后拉取的项目都会存在这里。

![](img/50469ff68b075791f5fb96a09516f3ff.png)

1.  **安装 n8n**

打开 cmd 来拉取 n8n

```
docker volume create n8n_data
docker run -it --rm --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
```

同学们如果出现这个问题

![](img/fc434dc4434030571ae33654d215aba2.png)

进行 docker 换源，设置→Docker Engine→把以下这段复制进去→记得科学上网一定要开全局模式并重新输入之前的命令行

```
{
  &quot;debug&quot;: false,
  &quot;experimental&quot;: false,
  &quot;features&quot;: {
    &quot;buildkit&quot;: true
  },
  &quot;insecure-registries&quot;: null,
  &quot;registry-mirrors&quot;: [
    &quot;https://7bezldxe.mirror.aliyuncs.com/&quot;,
    &quot;https://docker.mirrors.ustc.edu.cn/&quot;,
    &quot;https://hub-mirror.c.163.com&quot;,
    &quot;https://registry.docker-cn.com&quot;
  ]
}
```

![](img/8219bc5491e3cb5620a1c494eb60dce5.png)

成功后：命令行显示如下

![](img/5329cb8b24ffcec8942900cbc5ce7541.png)

containers 打开就会显示 n8n：

![](img/38d1b79c3f32a1b56192214a47044425.png)

**打开这个 ports，**进行 n8n 账号的登录和注册：这一步需要科学上网哈～

![](img/d87185fc54d2dafc035d46b06ccbd544.png)

1.  **准备大模型账号：**

蓝耘 MaaS：https://console.lanyun.net/#/register?promoterCode=0168

这个平台对新用户免费赠送 1000 万模型 tokens 资源包 +20 算力券，完全个体用户的使用，后续我们的工作流需要进行大模型的配置，因此大家需要提前准备好账号。

![](img/5288a3cb734943644435bd7908bbd0cc.png)

1.  准备飞书账号。

# 开始制作飞书新闻助手

1.  第一步：点击从头开始

![](img/5b6e300278edafffc422c4508f51495f.png)

1.  **第二步：定时触发 （Schedule），我们需**要定时发送每日的日报，设定上午 8 点。

+   点击 "+" 号，在搜索框中输入 Schedule 并选择它。

+   **配置参数**：

+   Trigger Interval: 选择 Days （按天间隔，并设置Days Between Triggers=1）。

+   **Trigger at Hour**: 设置您希望接收新闻的时间，例如 8 （代表早上 8 点）。

（此为示例图片，请以实际界面为准）

![](img/964a15565d463666f24784fdef64be99.png)

![](img/7bff77a4f956de80afe002bf07aeec4a.png)

1.  第三步：获取新闻源 （RSS Feed）

我们需要一个稳定的新闻来源。很多科技网站都提供 RSS 订阅**源，这是**一个非常方便的方式。这里我们以 36 氪 的 "前沿科技" 频道为例。

+   点击 Schedule 节点右边的 "+" 号。

+   搜索 RSS Read 并选择它。

**配置参数**：

+   **URL**: 填入您想订阅的科技新闻 RSS 地址。我们设置了五个节点，这些 RSS 都是真实可用的聚合内容（RSS 的原理）

**36kr**: https://www.36kr.com/feed

**虎嗅网**：https://www.huxiu.com/rss/0.xml

**钛媒体**：https://www.tmtpost.com/rss.xml

**掘金**：https://juejin.cn/rss

**开源中国**：https://www.oschina.net/news/rss

点击 **"Execute Node"** 按钮测试一下，成功时右侧看到返回的新闻列表数据。

![](img/fb66c5a7f9abc624427ac47970bede3b.png)

![](img/c015aeed8978379794acf6180e596e9a.png)

1.  第四步：格式化新闻内容 （Set）

这一步的核心目的：在众多新闻中，但我们只关心和特定主题（比如人工智能）相关的内容。这时，我们可以使用 Code 节点，通过一小段 JavaScript 代码，像一个智能机器人一样帮我们筛选出真正需要的信息。

点击 RSS Read 节点后的 + 号。

添加节点，选择 **Code**。

将以下代码完整粘贴到代码区域：

```
// 1. 定义你的关键词列表，如果需要更改领域修改绿色文字即可，其他代码不要变动
const keywords = [
  &#x27;人工智能&#x27;, &#x27;AI&#x27;, &#x27;机器学习&#x27;, &#x27;深度学习&#x27;, &#x27;神经网络&#x27;,
  &#x27;算法&#x27;, &#x27;大模型&#x27;, &#x27;LLM&#x27;, &#x27;GPT&#x27;, &#x27;ChatGPT&#x27;, &#x27;BERT&#x27;, &#x27;Transformer&#x27;
];

// 2. 创建一个不区分大小写的正则表达式
// 这会将关键词列表用 &quot;或&quot; (|) 连接起来，例如 (人工智能|AI|机器学习|...)
// &#x27;i&#x27; 标志表示不区分大小写 (case-insensitive)
const regex = new RegExp(keywords.join(&#x27;|&#x27;), &#x27;i&#x27;);

// 3. 获取上一个节点传入的所有项目
const items = $input.all();

// 4. 过滤这些项目
const filteredItems = items.filter(item =&gt; {
  // 安全地获取标题和描述，如果不存在则为空字符串
  const title = item.json.title || &#x27;&#x27;;
  const description = item.json.description || item.json.content || &#x27;&#x27;;
  
  

  // 检查标题 或 描述中是否包含任何一个关键词
  // regex.test() 会返回 true 或 false
  return regex.test(title) || regex.test(description);
});

// 5. 只返回匹配成功的项目
return filteredItems;

```

执行后可以看到，输入源原本有 30 多条信息，被浓缩到了 7 条。

![](img/b79de99dc90a8ac54337a0259dcfce5c.png)

1.  第五步：聚合、筛选与去重新闻内容 （Merge）

**Merge （合并数据）**

+   添加 Merge 节点。

+   将前面 5 个 Code 节点的输出端，全部连接到这一个 Merge 节点上。

+   **模式 （Mode）**: 设置为 Append，将所有来源的新闻合并成一个大列表

![](img/87d59d93de74e1b8d96c52b979bd4946.png)

![](img/0e2ef75c7b897c8b846c0faf3dc64318.png)

![](img/7649b72bf42161531a40c0194c368e7c.png)

1.  第六步：Filter / Code （关键词筛选）

因为我们希望是每日日报，所以我们配置了 Filter 节点，其核心功能是筛选出发布时间在过去 24 小时内的数据。点击上个节点后的+号，并

```
对应下图位置填入代码
{{ $json.pubDate }}

{{new Date(Date.now() - 1 * 24 * 60 * 60 * 1000)}}

{{ $json.pubDate }}
```

![](img/6f2b3f44dd4bedb260e495f218eb859d.png)

![](img/1468e632f7f1cc5b9fbef7ebede9345f.png)

1.  第七步：使用 Code （代码） 节点批量转换数据以对接飞书

**数据格式转换与聚合：**它将上游节点传入的多个分离的数据项（items），整合并构造成一个符合飞书（Lark）多维表格 API 要求的、单一的 JSON 数据包。

添加节点，选择 **Code**。

将以下代码完整粘贴到代码区域：

```
// **第一步：获取上游传入的所有新闻项目**
const items = $items(); 

// **第二步：将每个项目转换为飞书需要的 {fields: {...}} 格式**
const recordsToCreate = items.map(item =&gt; {
  return {
    fields: {
      // **请确保这里的中文名和你的多-维表格列名完全一致**
      &quot;新闻标题&quot;: item.json.title,
      &quot;新闻链接&quot;: item.json.link,
      &quot;发布时间&quot;: item.json.isoDate,
      &quot;内容摘要&quot;: item.json.contentSnippet,
      &quot;具体内容&quot;: item.json.content
    }
  };
});

// **第三步：构建最终的、完整的请求体 JSON**
const finalJsonPayload = {
  records: recordsToCreate
};

// **第四步：将这个完整的JSON作为单个项目返回给下一个节点**
return [
  {
    json: finalJsonPayload
  }
];
```

转换成为适合飞书的信息结构：

![](img/19ecf7305c1f22141a313ca58a5562af.png)

1.  第八步：使用 Code （代码） 节点批量转换数据以对接飞书

这个节点是为了将信息和飞书的节点接入，能够录入进去飞书的多维表格，核心问题就是，需要得到飞书的权限。

需要在飞书后台完成所有准备工作。

1.  创建一个“企业自建应用（飞书网页端）

这是让 n8n 能够通过 API 与您的飞书账户沟通的“身份凭证”。

1.  登录 [**飞书开放平台**](https://open.feishu.cn/)。

1.  **进入“开发者**后台”，点击“创建应用”，选择“企业自建应用”。

1.  填写应用的名称（例如“n8n 新闻助手”）和描述，然后创建。

1.  在应用详情页的 **“凭证与基础信息”** 中，找到并复制保存好您的 **App ID** 和 **App Secret**。这是后续 n8n 连接飞书的关键。

![](img/5a94cd60c1931abb2402865d38dc43a2.png)

![](img/70222c5538e57abf10a166118f000e27.png)

1.  **为应用开通权限**

1.  在应用详情页左侧菜单，进入 **“权限管理”**。

1.  在搜索框中搜索“**多维表格**”。

1.  找到并**开通**以下权限（建议全部开通，以备后用）：

建议把多维表格的全部权限打开，以及应用发送消息的权限，后面机器人会使用到，并打开机器人功能。

![](img/60a5739a305f8a1c609d478671570b38.png)

![](img/780c2aded2c8859b22b73a57c73f49ab.png)

![](img/e1f2b253f566f3e9bc65603982de408e.png)

![](img/95331920ec89a0971edf919f81fccfc3.png)

![](img/b413b4fbbf3db2ba9f8b2b94b389f3e1.png)

**【! ! !** 最关键的一步**！！！】 添加**完权限后，必须回到左侧的 “应**用发**布” 菜单，点击 “创建新版本”，然后将这个包含了新权限的版本发布。只有发布后，权限才会生效。

1.  **创建多维表格**

1.  **在飞书中，直接通过**“多维表格”应用入口创建一个全新的、独立的表格。

1.  **【重要提示】**：请**不要**在任何“知识库”或“文档”中创建表格，因为我们发现 n8n 的节点可能不兼容这种内嵌式表格，会导致 NOTEXIST 错误。

1.  在新表格中，创建以下列（字段），并设置好对应的字段类型：

新闻标题 新闻链接 发布时间 内容摘要 具体内容

![](img/eeab1de91297c0e600194473eafb529c.png)

1.  获取表格的 Token 和 ID

1.  在浏览器中打开您刚刚创建的多维表格。

1.  查看浏览器地址栏的网址 （URL），它的结构类似： https://.../base/basxxxxxxxxxxxxxxx?table=tblxxxxxxxxxxxxxxx&...

1.  复制并保存好这两个 ID：

+   **多维表格 Token**: base/ 和 ?table= 之间的那串字符 （以 bas 开头）。

+   **多维表格 ID**: ?table= 和 &view= 之间的那串字符 （以 tbl 开头）。

![](img/e203616eb3405e16534a497dfc9a504e.png)

1.  **飞书多维表格节点配置**

其中 n8n 初始不带有飞书的节点配置，需要先在社区中安装飞书节点

点击左下角用户头像→setting→community nodes→右上角 install 打开→填入n8n-nodes-feishu-lite

![](img/86cff3787795f221ebf288ab8e115589.png)

回到 n8n 工作流，增加新的节点：feishu node→多维表格批量新增记录

![](img/e1353492de96a300644008c17557e5d9.png)

设置节点：

![](img/2bdbca451ff1d7f90b821052c8f38be2.png)

填入之前开发者平台得到的 Appid 和 Appsecret，以及之前得到的多维表格的 ID 和 TOKEN，请求体中填入{{JSON.stringify($json)}}

![](img/c77217efd42345d12ae4fccae5540866.png)

![](img/dfd482fcde98fa0c4cdbef2388ed868b.png)

成功后，会显示 success，并且表格中会出现新增的数据。

![](img/18a345c9dcda1f59701493f7a0253ff7.png)

1.  **飞书机器人配置**

这个流程是坑最多的地方，主要是飞书的权限，大模型配置等问题。

由于数据源来自于之前的新闻源，只是需要提炼 5 条，因此接入之前的 code 代码。蓝耘目前只支持 Http Request。

![](img/c5f93131f3b9645982da3560efdd206b.png)

+   **“蓝耘” ****HTTP Request**** 节点**

1.  **Method （请求方法）**: 选择 POST。

1.  **URL （接口地址）**:

在他们的 API 开发文档中可以找到。

URL: https://maas-api.lanyun.net/v1/chat/completions

1.  **Send Headers**

1.  Content-Type: application/json

1.  Authorization: Bearer sk-xxxxx

![](img/85ef600186e4055c4fe35e66d3aed785.png)

其中这个秘银来自于蓝耘的 api key：右上角控制台→Api key 管理→新建→一键复制（注意坑 1：复制后粘贴出来看，有一部分无用信息，只粘贴 sk 部分）

![](img/9c7c6099be56f57ac190556544da3751.png)

1.  Send Body （发送请求体）:

1.  打开 Send Body 开关。

1.  Body Content Type: 选择 JSON。

1.  **Specify Body**: 选择 Using JSON。

1.  **JSON 字段**：

1.  在这里粘贴调用蓝耘模型所需的 JSON 结构。您需要根据蓝耘的 API 文档来构建，但通常会包含 model， messages 等字段。

1.  messages 中的 content 应该是动态的，用来接收上一个节点传来的数据。例如，如果您想总结 Code in JavaScript5 节点处理后的第一条新闻，可以这样写：

```
{
  &quot;model&quot;: &quot;/maas/deepseek-ai/DeepSeek-V3.2-Exp&quot;,
  &quot;messages&quot;: [
    {
      &quot;role&quot;: &quot;system&quot;,
      &quot;content&quot;: &quot;你是一名专业的科技新闻编辑和日报助手。你的核心任务是高效、精准地处理每日的科技新闻资讯，筛选、总结并严格按照用户要求的格式，整理成一份结构清晰、适合阅读的科技日报。发布时间雨科技新闻日报的具体时间比如和输入的严格统一，不能产生ai幻觉。&quot;
    },
    {
      &quot;role&quot;: &quot;user&quot;,
      &quot;content&quot;: &quot;请根据以下新闻数据和格式要求，生成一份科技日报。输出要markdown格式\n\n# 指示与步骤\n1.  **挑选**：仔细阅读所有新闻的标题和摘要，评估其对科技行业或公众的影响力，选出你认为最重要的5条。\n2.  **浓缩**：对于你选出的每一条新闻，请将原始的“内容摘要”浓缩成一句更加精炼、准确、能抓住核心要点的总结。\n3.  **格式化**：严格按照下面“输出格式”部分指定的 Markdown 格式来组织你的最终报告，不要有任何多余的文字或解释。\n\n# 输出格式\n**科技新闻日报 **\n\n---\n\n**1. [此处填写第一条新闻的标题]**\n* **核心内容**：[此处填写浓缩后的一句话总结]\n***新闻链接**：[此处填写新闻链接]\n\n**2. [此处填写第二条新闻的标题]**\n* **核心内容**：[此处填写浓缩后的一句话总结]\n* **新闻链接**：[此处填写新闻链接]\n\n**3. [此处填写第三条新闻的标题]**\n* **核心内容**：[此处填写浓缩后的一句话总结]\n*  **新闻链接**：[此处填写新闻链接]\n\n# 输入数据\n\n{{ JSON.stringify($json) }}&quot;
    }
  ],
  &quot;max_tokens&quot;: 2000,
  &quot;temperature&quot;: 0.3
}
```

1.  **配置 ****Code in Jav**aScript 节点 （格式处理器）

这个节点是“翻译官”，负责将 AI 返回的“原始数据”转换成飞书能“看懂”的“消息卡片”，且我们要把其中的 Markdown 文档提炼出来。

将下面这段我们最终调试成功的**完整代码**复制粘贴到这个 Code 节点中：

```
// 1. 获取上一个AI节点（名为 &quot;HTTP Request&quot;）的输出数据
const item = $items(&quot;蓝耘&quot;)[0]; 

// 2. 从深层结构中提取AI生成的内容字符串
let aiMarkdownContent = item.json.choices[0].message.content;

// 3. (关键步骤) 清理字符串：去掉AI回复中多余的代码块标记 ` ``` `
aiMarkdownContent = aiMarkdownContent.replace(/^`{3}(markdown)?\n/, &#x27;&#x27;).replace(/\n`{3}$/, &#x27;&#x27;).trim();

// 4. 【核心修改】只构建飞书“消息卡片”的 card 对象部分
const feishuCardObject = {
  &quot;config&quot;: {
    &quot;wide_screen_mode&quot;: true
  },
  &quot;elements&quot;: [
    {
      &quot;tag&quot;: &quot;markdown&quot;,
      &quot;content&quot;: aiMarkdownContent
    }
  ]
};

// 5. 用我们新构建的 card 对象 替换掉整个旧的 json 数据
item.json = feishuCardObject;

// 6. 返回处理好的、可以直接发送给飞书的单个项目
return item;
```

1.  **配置 ****Message:sen**d message1 节点 （飞书发送器）

这个节点只负责最后一步的发送工作。

**飞书节点 （****M**essage:send message）:

+   **职责****：负**责投递。接收打包好**的“消息**卡片”对象，用 JSON.stringify() 将其转换成字符串。

![](img/6fd72546b83a68c35c37e932e897e829.png)

用户 ID 类型：Chat ID

接受者 ID：oc_xxx

打开飞书调试台：https://open.feishu.cn/api-explorer/cli_a86c91cc6fba500e?apiName=create&project=im&resource=message&version=v1

并点击快速复制即可：

![](img/124dec04d56703dac7526f3ee1792485.png)

消息类型：卡片

消息内容：{{JSON.stringify($json)}}

成功后：

![](img/bdc16dcc659425878c34546b1d73ee2b.png)