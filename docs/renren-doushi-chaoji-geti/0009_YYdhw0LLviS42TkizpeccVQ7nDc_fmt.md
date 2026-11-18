
# 2026-03-05【教程】n8n全网资讯热点捕捉器 - Xuan酱 0114 副本

> 来源：[https://xuqiwei1986.feishu.cn/docx/NoJgd5QHgojmaTx7Tc1cuWCWngd](https://xuqiwei1986.feishu.cn/docx/NoJgd5QHgojmaTx7Tc1cuWCWngd)

## [Xuan酱的AI知识库](https://q7y0qvi7ov.feishu.cn/wiki/RxmAw9xGhiFx0CkptXFcGxFNn8c?from=from_copylink)他往期视频的教程文档都在👉

## 工作流文件

![](img/6a99fa5f1611fd8944a8cd10425d9727.png)

[`n8n信息聚合器.json`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RVIabqlWEoYNdfxteDLcO1mEnHb)

## Step 1 本地部署n8n

1. 下载安装Docker，默认选项即可

1. Docker 官网：https://www.docker.com/

1. 下载n8n

1. Windows和Mac都是打开终端，输入以下命令：

```
docker volume create n8n_data
docker run -d --name n8n --restart unless-stopped -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
```

1. 如果由于网络环境原因安装失败的朋友可以使用以下命令

```
docker volume create n8n_data
docker run -d --name n8n --restart unless-stopped -p 5678:5678 -v n8n_data:/home/node/.n8n n8nio/n8n
```

1. 访问网址 `http://localhost:5678` ，即可打开n8n

1. 后续使用在Docker软件界面里可以开关，打开运行后访问网址 `http://localhost:5678` ，即可打开n8n

![](img/9dccdd9f095102ea8bbfeb1566ec597d.png)

![](img/c351b4974559390a01372e95fcea5cac.png)

## Step 2 收集信息

### RSS

#### RSS链接在哪里寻找？

1. 直接搜索媒体的RSS源。推荐用官方RSS源，或者在FeedSpot、GitHub、Reddit上找。

1. 别人推荐的RSS源清单。

1. FeedSpot上搜索词条（`https://rss.feedspot.com/`）

“Artificial Intelligence”“Venture Capital”“Product Management”“Video game”“coffee”……

1. GitHub上

1. 中文优质RSS集合：`https://github.com/weekend-project-space/top-rss-list`

1. 个人博客集合：`https://github.com/timqian/chinese-independent-blogs`

1. 科技人安利：`https://github.com/Gracker/Rss-IT`

#### YouTube博主RSS订阅

拼接模板：`https://www.youtube.com/feeds/videos.xml?channel_id=<油管频道id>`

例如，`https://www.youtube.com/feeds/videos.xml?channel_id=UCNJ1Ymd5yFuUPtn21xtRbbw`。其中，`UCNJ1Ymd5yFuUPtn21xtRbbw`是AI Explained的油管频道id，该RSS链接订阅的就是AI Explained的油管频道（`https://www.youtube.com/@aiexplained-official`）

##### 油管id频道获取

1. 进入要订阅的博主的主页，点击「更多」

1. 弹出的框下拉至底部，按下图顺序点击「分享频道」-「复制频道ID」，即可获取频道ID

#### X博主RSS订阅

需付费的第三方工具：`RSS.app`（`https://rss.app/`）

按照网站提示操作，即可生成RSS链接

除了X博主还支持以下平台的RSS订阅

![](img/ed2c269210314fb2a83b34c55ba10dcc.png)

### API

TikTok API：`https://rapidapi.com/Lundehund/api/tiktok-api23/playground`

YouTube API：`https://console.cloud.google.com/apis/api/youtube.googleapis.com/`

Reddit API：`https://www.reddit.com/dev/api/`。`https://www.reddit.com/prefs/apps`。

X API：`https://twitterapi.io/`

每个平台官方API、第三方API提供的功能都不同，可以按照自己的需求去配置

下面提供了X热点API配置的详细操作步骤，可以按照这个举一反三，边配置边问AI

参考提示词：「如何用某某API+n8n搭建一个监控某某平台AI相关热点帖子的工作流」

看不懂API文档，可以把API文档复制全部丢给AI，让它告诉你该如何配置n8n的HTTP节点

#### 以X热点为例

用的是第三方的`twitterapi`，它提供的功能有几十个，抓取热点的话我们用这个「高级搜索」的功能

`https://docs.twitterapi.io/api-reference/endpoint/tweet_advanced_search`

我们看这里写的`GET`，就是HTTP节点里`Method`要填的东西

URL就是把这一行复制粘贴过来

![](img/908c39ffa9f29ac60ddaf5aa798842c9.png)

![](img/36c2ac3922377353cc08604e44b2d6de.png)

下面的「`Authorizations`」，就是HTTP节点里「`Authentication`」要填的东西

选一下第三个「`Generic Credential Type`」

然后我们看左图「`Authorizations`」这里写的是在「`header`」的

所以我们这里选头部认证

然后配置我们的API

注册`twitterapi`账号后，在该链接获取`API Key`：`https://twitterapi.io/dashboard`

Name填「`X-API-Key`」

然后把`API Key`复制粘贴到下面`Value`这里

最后填一下查询参数

点开这个按钮

然后在下面的`Name`里填`query`

`Value`里填你想跟踪的热点词，我这里填的是AI

查询类型我填的`Top`

## Step 3 信息预处理

可看视频、工作流文档中的配置

注意处理时间时`发布日期`与`iso日期`的区别（可以和AI聊）

## Step 4 AI加工

可看视频、工作流文档中的配置

Deepseek开放平台：`https://platform.deepseek.com/`

## Step 5 存档和推送

### 存档

可以用飞书多维表格、Notion、谷歌表格等

#### 以飞书多维表格为例

下载飞书社区节点：`n8n-nodes-feishu-lite`

新建飞书节点「多维表格-新增记录」

配置飞书API。在开发者后台，创建应用，并获取`appid`和`appsecret`

飞书开放平台（若遇到问题报错，可将报错信息粘贴到右上角搜索框）：`https://open.feishu.cn/?lang=zh-CN`

飞书开发者后台（用来创建应用并获取`appid`和`appsecret`）：`https://open.feishu.cn/app`

获取多维表格等`Token`和`ID`。

创建多维表格，并开放编辑权限。

注意多维表格链接含「`base`」的才行（一般是放在云空间的），放在知识库里的就不行。

填写请求体JSON，设置多维表格表头

前面处理的时候有哪些字段，想存储哪些字段，就填哪些

多维表格表头也一一对应地创建好

**配置过程中有报错都可以问AI！**

```
{"fields":{
  "标题":"{{ $json['标题'] }}",
  "内容":"{{ $json['内容'] }}",
  "日期":"{{ $json['日期']}}",
"链接": {
  "text": "原文链接",
  "link": "{{ $json["链接"].replace(/\n/g, "") }}",
},
"来源":"{{ $json['来源'] }}",
"板块":"{{ $json['板块'] }}",
"分类":"{{ $json['分类'] }}",
"分类理由":"{{ $json['分类理由'] }}"
}}
```

### 推送

可查看工作流文档中的配置

代码如下：

```
// 获取上游输入的所有新闻项
const items = $input.all();

// 只保留分类为 “新闻” 的数据
const filtered = items
  .map(i => i.json)
  .filter(n => ["新闻", "视频"].includes(n["分类"]));

// 将数据按板块分组
const grouped = {};
for (const n of filtered) {
  const section = n["板块"] || "其他";
  if (!grouped[section]) grouped[section] = [];
  grouped[section].push(n);
}

// 日期取今天
const todayDate = new Date();
const todayDateStr = todayDate.toISOString().slice(0, 10);

// 构建 Markdown 输出
let md = `# ${todayDateStr} AI资讯小报\n`;

// 🌟 明星公司动态（同来源合并）
if (grouped["明星公司动态"] && grouped["明星公司动态"].length > 0) {
  md += `\n🌟 **明星公司动态**\n`;
  
  const companyMap = {};
  grouped["明星公司动态"].forEach(n => {
    const source = n["来源"];
    if (!companyMap[source]) companyMap[source] = [];
    companyMap[source].push(`- ${n["内容"]}\n${n["链接"]}`);
  });

  for (const [source, posts] of Object.entries(companyMap)) {
    md += `\n**${source}**\n${posts.join("\n")}\n`;
  }

  md += `\n---\n\n`;
}

// 📰 新闻汇总（来源：标题 + 内容 + 链接）
if (grouped["新闻"] && grouped["新闻"].length > 0) {
  md += `\n📰 **新闻汇总**\n`;
  grouped["新闻"].forEach(n => {
    md += `- 《${n["来源"]}》：${n["标题"]}\n ${n["内容"]}\n${n["链接"]}\n`;
  });
  md += `\n---\n\n`;
}

// 🕶️ 大佬动态（同来源合并）
if (grouped["大佬动态"] && grouped["大佬动态"].length > 0) {
  md += `\n🕶️ **大佬动态**\n`;

  const leaderMap = {};
  grouped["大佬动态"].forEach(n => {
    const source = n["来源"];
    if (!leaderMap[source]) leaderMap[source] = [];
    leaderMap[source].push(`- ${n["内容"]}\n${n["链接"]}`);
  });

  for (const [source, posts] of Object.entries(leaderMap)) {
    md += `\n**${source}**\n${posts.join("\n")}\n`;
  }

  md += `\n---\n\n`;
}

// 🌊 油管博主更新
if (grouped["油管博主"] && grouped["油管博主"].length > 0) {
  md += `\n🫙 **油管博主更新**\n`;
  grouped["油管博主"].forEach(n => {
    md += `- 《${n["来源"]}》：${n["标题"]}\n${n["链接"]}\n`;
  });
  md += `\n---\n`;
}

// 输出 Markdown
return [{ json: { markdown: md.trim() } }];

```



```
// 获取上游输入的所有新闻项
const items = $input.all();

// 只保留指定分类的数据
const filtered = items
  .map(i => i.json)
  .filter(n => ["干货"].includes(n["分类"]));

// 将数据按板块分组
const grouped = {};
for (const n of filtered) {
  const section = n["板块"] || "其他";
  if (!grouped[section]) grouped[section] = [];
  grouped[section].push(n);
}

// 日期取今天
const todayDate = new Date();
const todayDateStr = todayDate.toISOString().slice(0, 10);

// 构建 Markdown 输出
let md = `# ${todayDateStr} AI干货小报\n`;

// 🌟 明星公司动态（同来源合并）
if (grouped["明星公司动态"] && grouped["明星公司动态"].length > 0) {
  md += `\n🌟 **明星公司动态**\n`;
  
  const companyMap = {};
  grouped["明星公司动态"].forEach(n => {
    const source = n["来源"];
    if (!companyMap[source]) companyMap[source] = [];
    companyMap[source].push(`- ${n["内容"]}\n${n["链接"]}`);
  });

  for (const [source, posts] of Object.entries(companyMap)) {
    md += `\n**${source}**\n${posts.join("\n")}\n`;
  }

  md += `\n---\n\n`;
}

// 🕶️ 大佬动态（同来源合并）
if (grouped["大佬动态"] && grouped["大佬动态"].length > 0) {
  md += `\n🕶️ **大佬动态**\n`;

  const leaderMap = {};
  grouped["大佬动态"].forEach(n => {
    const source = n["来源"];
    if (!leaderMap[source]) leaderMap[source] = [];
    leaderMap[source].push(`- ${n["内容"]}\n${n["链接"]}`);
  });

  for (const [source, posts] of Object.entries(leaderMap)) {
    md += `\n**${source}**\n${posts.join("\n")}\n`;
  }

  md += `\n---\n\n`;
}

// 📰 新闻汇总（来源：标题 + 内容 + 链接）
if (grouped["新闻"] && grouped["新闻"].length > 0) {
  md += `\n📰 **新闻汇总**\n`;
  grouped["新闻"].forEach(n => {
    md += `- 《${n["来源"]}》：${n["标题"]}\n ${n["内容"]}\n${n["链接"]}\n`;
  });
  md += `\n---\n`;
}

// 输出 Markdown
return [{ json: { markdown: md.trim() } }];
```

```
// 获取上游输入的所有新闻项
const items = $input.all();

// 将不同板块的新闻分类
const grouped = {};
for (const item of items) {
  const data = item.json;
  const section = data["板块"] || "其他";
  if (!grouped[section]) grouped[section] = [];
  grouped[section].push(data);
}

// 日期取今天
const todayDate = new Date();
const todayDateStr = todayDate.toISOString().slice(0, 10);

// 构建 Markdown 输出
let md = `# ${todayDateStr} AI热点小报\n`;

// 函数：生成互动数据字符串，值为0的不显示
function formatStats(n) {
  const stats = [];
  if (n["点赞"]) stats.push(`点赞👍 ${n["点赞"]}`);
  if (n["观看"]) stats.push(`观看👁️ ${n["观看"]}`);
  if (n["评论"]) stats.push(`评论💬 ${n["评论"]}`);
  if (n["收藏"]) stats.push(`收藏⭐ ${n["收藏"]}`);
  if (n["转发"]) stats.push(`转发🔁 ${n["转发"]}`);
  if (n["引用"]) stats.push(`引用❗ ${n["引用"]}`);
  return stats.length > 0 ? `【${stats.join(" | ")}】` : "";
}

// Reddit热点
if (grouped["Reddit热点"] && grouped["Reddit热点"].length > 0) {
  md += `\n🔥 **Reddit热点**\n`;
  grouped["Reddit热点"].forEach(n => {
    md += `- ${n["标题"]} \n${formatStats(n)}\n${n["链接"]}\n`;
  });
  md += `\n---\n\n`;
}

// YouTube热点
if (grouped["YouTube热点"] && grouped["YouTube热点"].length > 0) {
  md += `\n🔥 **YouTube热点**\n`;

  // 按视频类型分组（短视频、中视频、长视频）
  const typeMap = {};
  grouped["YouTube热点"].forEach(n => {
    const type = n["来源"] || "其他";
    if (!typeMap[type]) typeMap[type] = [];
    typeMap[type].push(n);
  });

  for (const [type, videos] of Object.entries(typeMap)) {
    md += `\n**${type}**\n`;
    videos.forEach(v => {
      md += `- ${v["标题"]}\n ${formatStats(v)}\n${v["链接"]}\n`;
    });
  }

  md += `\n---\n\n`;
}

// Twitter热点
if (grouped["Twitter热点"] && grouped["Twitter热点"].length > 0) {
  md += `\n🔥 **Twitter热点**\n`;
  grouped["Twitter热点"].forEach(n => {
    md += `- ${n["标题"]} \n${formatStats(n)}\n${n["链接"]}\n`;
  });
  md += `\n---\n\n`;
}

// TikTok热点
if (grouped["TikTok热点"] && grouped["TikTok热点"].length > 0) {
  md += `\n🎵 **TikTok热点**\n`;
  grouped["TikTok热点"].forEach(n => {
    md += `- ${n["标题"]} \n${formatStats(n)}\n${n["链接"]}\n`;
  });
  md += `\n---\n`;
}

return [{ json: { markdown: md.trim() } }];
```

# 如何保证24小时运行

1. 部署在自己电脑上，保证24小时不关机不休眠

1. 购买官方`n8n`会员，开箱即用，**小白友好但贵**（`https://n8n.io/`）

1. 第三方云服务

1. 国内外知名云厂商的服务器，**小白不友好**

1. Sealos云服务，平台已经部署好了`n8n`，你只需要在里面注册登录，就能完成部署，小白友好且没那么贵（`https://bja.sealos.run/`）

![](img/a2f9d1e5a0c12d6cca78ea967da37cc0.png)

![](img/b31df98b9471a61046e00953e7a10b59.png)

部署后，访问公网地址，注册登录即可使用。

但注意，如果`RSS`链接里有外媒的话，左上角这里需要设置在国外，否则网络请求很有可能会失败。

![](img/65321a0eb774ed47c9e38b447ae7e2db.png)
