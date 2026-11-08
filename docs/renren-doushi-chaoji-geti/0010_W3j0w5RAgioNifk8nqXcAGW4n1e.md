# 【教程】n8n全网资讯热点捕捉器 - Xuan酱 0114

> 来源：[https://xuqiwei1986.feishu.cn/docx/Lb3fd4Pwro6c7Wx72Nwc6svwn9c](https://xuqiwei1986.feishu.cn/docx/Lb3fd4Pwro6c7Wx72Nwc6svwn9c)

> 本文档为Xuan酱 2026.1.14 《我用AI，做了个全网AI热点捕捉器！》视频配套教程文档

## 全平台@Xuan酱，关注我，和我一起探索AI的更多玩法

B站：https://space.bilibili.com/14848367?

抖音： https://v.douyin.com/i5Jqby5f/

📕：https://www.xiaohongshu.com/user/profile/583ab2525e87e729b60e3564

YouTube：https://www.youtube.com/@Xuan2333

视频号、👸号：直接搜索 Xuan酱，或扫码关注👇

![](img/cbaa757da1c79216e1dc8754e445a684.png)

👸号获取所有资料链接更快哦

# [Xuan酱的AI知识库](https://q7y0qvi7ov.feishu.cn/wiki/RxmAw9xGhiFx0CkptXFcGxFNn8c?from=from_copylink)他往期视频的教程文档都在👉

# 工作流文件

![](img/a09a6ab024b1d2326f6e20ff80392fc9.png)

[n8n信息聚合器.json](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MZlQbLtZsoJg3SxZaMAc6SMKnxc)

# Step 1 本地部署n8n

1.  下载安装Docker，默认选项即可

1.  Docker 官网：https://www.docker.com/

1.  下载n8n

1.  Windows和Mac都是打开终端，输入以下命令：

```
docker volume create n8n_data
docker run -d --name n8n --restart unless-stopped -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
```

1.  如果由于网络环境原因安装失败的朋友可以使用以下命令

```
docker volume create n8n_data
docker run -d --name n8n --restart unless-stopped -p 5678:5678 -v n8n_data:/home/node/.n8n n8nio/n8n
```

1.  访问网址 http://localhost:5678 ，即可打开n8n

1.  后续使用在docker软件界面里可以开关，打开运行后访问网址 http://localhost:5678 ，即可打开n8n

![](img/6fab3d9762a0ce6cb33926b9c731a151.png)

![](img/4cf56d1adbdd6f5bbc466aaf16e580b3.png)

# Step 2 收集信息

## RSS

### RSS链接在哪里寻找？

1.  直接搜索媒体的RSS源。推荐用官方RSS源，或者在FeedSpot、GitHub、Reddit上找。

1.  别人推荐的RSS源清单。

1.  FeedSpot上搜索词条（https://rss.feedspot.com/）

“Artificial Intelligence”“Venture Capital”“Product Management”“Video game”“coffee”……

1.  GitHub上

1.  中文优质RSS集合：https://github.com/weekend-project-space/top-rss-list

1.  个人博客集合：https://github.com/timqian/chinese-independent-blogs

1.  科技人安利：https://github.com/Gracker/Rss-IT

### YouTube博主RSS订阅

拼接模板：https://www.youtube.com/feeds/videos.xml?channel_id=<油管频道id>

例如，https://www.youtube.com/feeds/videos.xml?channel_id=UCNJ1Ymd5yFuUPtn21xtRbbw。其中，UCNJ1Ymd5yFuUPtn21xtRbbw是AI Explained的油管频道id，该RSS链接订阅的就是AI Explained的油管频道（https://www.youtube.com/@aiexplained-official）

#### 油管id频道获取

1.  进入要订阅的博主的主页，点击「更多」

![](img/9e9768981789c0afd24c06e38e3aadf7.png)

1.  弹出的框下拉至底部，按下图顺序点击「分享频道」-「复制频道ID」，即可获取频道ID

![](img/4897ca3007060fc5be5d464776346db5.png)

### X博主RSS订阅

需付费的第三方工具：RSS.app（https://rss.app/）

按照网站提示操作，即可生成RSS链接

除了X博主还支持以下平台的RSS订阅

![](img/4835fb35bd068ee3cb40f50f88ee50cf.png)

## API

TikTok API：https://rapidapi.com/Lundehund/api/tiktok-api23/playground

YouTube API：https://console.cloud.google.com/apis/api/youtube.googleapis.com/

Reddit API：https://www.reddit.com/dev/api/。 https://www.reddit.com/prefs/apps。

X API：https://twitterapi.io/

每个平台官方API、第三方API提供的功能都不同，可以按照自己的需求去配置

下面提供了X热点API配置的详细操作步骤，可以按照这个举一反三，边配置边问AI

参考提示词：「如何用某某API+n8n搭建一个监控某某平台AI相关热点帖子的工作流」

看不懂API文档，可以把API文档复制全部丢给AI，让它告诉你该如何配置n8n的HTTP节点

### 以X热点为例

用的是第三方的twitterapi，它提供的功能有几十个，抓取热点的话我们用这个「高级搜索」的功能

https://docs.twitterapi.io/api-reference/endpoint/tweet_advanced_search

我们看这里写的GET，就是HTTP节点里Method要填的东西

URL就是把这一行复制粘贴过来

![](img/e991a221e97b20ca4edb3688806a0070.png)

![](img/1d4e0a58191f10cb6b158709a5149863.png)

下面的「Authorizations」，就是HTTP节点里「Authentication」要填的东西

选一下第三个「Generic Credential Type」

然后我们看左图「Authorizations」这里写的是在「header」的

所以我们这里选头部认证

![](img/45f60c9800225c95903fcecab3b76f7f.png)

![](img/b8396aac57e1445b17c92cb4e2e34d5d.png)

![](img/5cf524bd820998ade3dc864771f0e128.png)

然后配置我们的API

注册twitterapi账号后，在该链接获取API Key：https://twitterapi.io/dashboard

Name填「X-API-Key」

然后把API Key复制粘贴到下面Value这里

![](img/c5a0e3e6926c9a5f29595e936465fde1.png)

![](img/e562cc86532d84630d0f6614dd09f2bd.png)

![](img/f3bef03408bffc60ace69d5aaca2fc2b.png)

![](img/4658238fd7914725815d22ea98422c2c.png)

最后填一下查询参数

点开这个按钮

然后在下面的Name里填query

Value里填你想跟踪的热点词，我这里填的是AI

查询类型我填的Top

![](img/355c23aec9ebfc6e89466925294d78ac.png)

![](img/6973a62c4ca8649bbc2e333a5d588b98.png)

![](img/b0cf4726e1cc8847919fcfc8868d21b3.png)

# Step 3 信息预处理

可看视频、工作流文档中的配置

注意处理时间时**发布日期**与**iso日期**的区别（可以和AI聊）

# Step 4 AI加工

可看视频、工作流文档中的配置

Deepseek开放平台：https://platform.deepseek.com/

# Step 5 存档和推送

## 存档

可以用飞书多维表格、Notion、谷歌表格等

### 以飞书多维表格为例

下载飞书社区节点：n8n-nodes-feishu-lite

新建飞书节点「多维表格-新增记录」

配置飞书API。在开发者后台，创建应用，并获取appid和appsecret

飞书开放平台（若遇到问题报错，可将报错信息粘贴到右上角搜索框）：https://open.feishu.cn/?lang=zh-CN

飞书开发者后台（用来创建应用并获取appid和appsecret）：https://open.feishu.cn/app

![](img/1e5dc0ac47515d117652ec3a3d933d08.png)

![](img/7617c3e31c659d6978c88c0618e368cf.png)

获取多维表格等Token和ID。

创建多维表格，并开放编辑权限。

注意多维表格链接含「base」的才行（一般是放在云空间的），放在知识库里的就不行。

![](img/8fd800f5604443c4b8a6380d6b05e5c2.png)

![](img/3c67bedc134cf5a5a2f89496165fb7cb.png)

![](img/63e0fa2d13b5af4a3785ecb4d5fe923e.png)

填写请求体JSON，设置多维表格表头

前面处理的时候有哪些字段，想存储哪些字段，就填哪些

多维表格表头也一一对应地创建好

**配置过程中有报错都可以问AI！**

```
{&quot;fields&quot;:{
  &quot;标题&quot;:&quot;{{ $json[&#x27;标题&#x27;] }}&quot;,
  &quot;内容&quot;:&quot;{{ $json[&#x27;内容&#x27;] }}&quot;,
  &quot;日期&quot;:&quot;{{ $json[&#x27;日期&#x27;]}}&quot;,
&quot;链接&quot;: {
  &quot;text&quot;: &quot;原文链接&quot;,
  &quot;link&quot;: &quot;{{ $json[&quot;链接&quot;].replace(/\n/g, &quot;&quot;) }}&quot;
},
&quot;来源&quot;:&quot;{{ $json[&#x27;来源&#x27;] }}&quot;,
&quot;板块&quot;:&quot;{{ $json[&#x27;板块&#x27;] }}&quot;,
&quot;分类&quot;:&quot;{{ $json[&#x27;分类&#x27;] }}&quot;,
&quot;分类理由&quot;:&quot;{{ $json[&#x27;分类理由&#x27;] }}&quot;
}}
```

![](img/139c121a7547e48ee18f1b3ce31a76ce.png)

## 推送

可查看工作流文档中的配置

代码如下：

```
// 获取上游输入的所有新闻项
const items = $input.all();

// 只保留分类为 “新闻” 的数据
const filtered = items
  .map(i =&gt; i.json)
  .filter(n =&gt; [&quot;新闻&quot;, &quot;视频&quot;].includes(n[&quot;分类&quot;]));

// 将数据按板块分组
const grouped = {};
for (const n of filtered) {
  const section = n[&quot;板块&quot;] || &quot;其他&quot;;
  if (!grouped[section]) grouped[section] = [];
  grouped[section].push(n);
}

// 日期取今天
const todayDate = new Date();
const todayDateStr = todayDate.toISOString().slice(0, 10);

// 构建 Markdown 输出
let md = `# ${todayDateStr} AI资讯小报\n`;

// 🌟 明星公司动态（同来源合并）
if (grouped[&quot;明星公司动态&quot;] &amp;&amp; grouped[&quot;明星公司动态&quot;].length &gt; 0) {
  md += `\n🌟 **明星公司动态**\n`;
  
  const companyMap = {};
  grouped[&quot;明星公司动态&quot;].forEach(n =&gt; {
    const source = n[&quot;来源&quot;];
    if (!companyMap[source]) companyMap[source] = [];
    companyMap[source].push(`- ${n[&quot;内容&quot;]}\n${n[&quot;链接&quot;]}`);
  });

  for (const [source, posts] of Object.entries(companyMap)) {
    md += `\n**${source}**\n${posts.join(&quot;\n&quot;)}\n`;
  }

  md += `\n---\n\n`;
}

// 📰 新闻汇总（来源：标题 + 内容 + 链接）
if (grouped[&quot;新闻&quot;] &amp;&amp; grouped[&quot;新闻&quot;].length &gt; 0) {
  md += `\n📰 **新闻汇总**\n`;
  grouped[&quot;新闻&quot;].forEach(n =&gt; {
    md += `- 《${n[&quot;来源&quot;]}》：${n[&quot;标题&quot;]}\n ${n[&quot;内容&quot;]}\n${n[&quot;链接&quot;]}\n`;
  });
  md += `\n---\n\n`;
}

// 🕶️ 大佬动态（同来源合并）
if (grouped[&quot;大佬动态&quot;] &amp;&amp; grouped[&quot;大佬动态&quot;].length &gt; 0) {
  md += `\n🕶️ **大佬动态**\n`;

  const leaderMap = {};
  grouped[&quot;大佬动态&quot;].forEach(n =&gt; {
    const source = n[&quot;来源&quot;];
    if (!leaderMap[source]) leaderMap[source] = [];
    leaderMap[source].push(`- ${n[&quot;内容&quot;]}\n${n[&quot;链接&quot;]}`);
  });

  for (const [source, posts] of Object.entries(leaderMap)) {
    md += `\n**${source}**\n${posts.join(&quot;\n&quot;)}\n`;
  }

  md += `\n---\n\n`;
}

// 🌊 油管博主更新
if (grouped[&quot;油管博主&quot;] &amp;&amp; grouped[&quot;油管博主&quot;].length &gt; 0) {
  md += `\n🫙 **油管博主更新**\n`;
  grouped[&quot;油管博主&quot;].forEach(n =&gt; {
    md += `- 《${n[&quot;来源&quot;]}》：${n[&quot;标题&quot;]}\n${n[&quot;链接&quot;]}\n`;
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
  .map(i =&gt; i.json)
  .filter(n =&gt; [&quot;干货&quot;].includes(n[&quot;分类&quot;]));

// 将数据按板块分组
const grouped = {};
for (const n of filtered) {
  const section = n[&quot;板块&quot;] || &quot;其他&quot;;
  if (!grouped[section]) grouped[section] = [];
  grouped[section].push(n);
}

// 日期取今天
const todayDate = new Date();
const todayDateStr = todayDate.toISOString().slice(0, 10);

// 构建 Markdown 输出
let md = `# ${todayDateStr} AI干货小报\n`;

// 🌟 明星公司动态（同来源合并）
if (grouped[&quot;明星公司动态&quot;] &amp;&amp; grouped[&quot;明星公司动态&quot;].length &gt; 0) {
  md += `\n🌟 **明星公司动态**\n`;
  
  const companyMap = {};
  grouped[&quot;明星公司动态&quot;].forEach(n =&gt; {
    const source = n[&quot;来源&quot;];
    if (!companyMap[source]) companyMap[source] = [];
    companyMap[source].push(`- ${n[&quot;内容&quot;]}\n${n[&quot;链接&quot;]}`);
  });

  for (const [source, posts] of Object.entries(companyMap)) {
    md += `\n**${source}**\n${posts.join(&quot;\n&quot;)}\n`;
  }

  md += `\n---\n\n`;
}

// 🕶️ 大佬动态（同来源合并）
if (grouped[&quot;大佬动态&quot;] &amp;&amp; grouped[&quot;大佬动态&quot;].length &gt; 0) {
  md += `\n🕶️ **大佬动态**\n`;

  const leaderMap = {};
  grouped[&quot;大佬动态&quot;].forEach(n =&gt; {
    const source = n[&quot;来源&quot;];
    if (!leaderMap[source]) leaderMap[source] = [];
    leaderMap[source].push(`- ${n[&quot;内容&quot;]}\n${n[&quot;链接&quot;]}`);
  });

  for (const [source, posts] of Object.entries(leaderMap)) {
    md += `\n**${source}**\n${posts.join(&quot;\n&quot;)}\n`;
  }

  md += `\n---\n\n`;
}

// 📰 新闻汇总（来源：标题 + 内容 + 链接）
if (grouped[&quot;新闻&quot;] &amp;&amp; grouped[&quot;新闻&quot;].length &gt; 0) {
  md += `\n📰 **新闻汇总**\n`;
  grouped[&quot;新闻&quot;].forEach(n =&gt; {
    md += `- 《${n[&quot;来源&quot;]}》：${n[&quot;标题&quot;]}\n ${n[&quot;内容&quot;]}\n${n[&quot;链接&quot;]}\n`;
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
  const section = data[&quot;板块&quot;] || &quot;其他&quot;;
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
  if (n[&quot;点赞&quot;]) stats.push(`点赞👍 ${n[&quot;点赞&quot;]}`);
  if (n[&quot;观看&quot;]) stats.push(`观看👁️ ${n[&quot;观看&quot;]}`);
  if (n[&quot;评论&quot;]) stats.push(`评论💬 ${n[&quot;评论&quot;]}`);
  if (n[&quot;收藏&quot;]) stats.push(`收藏⭐ ${n[&quot;收藏&quot;]}`);
  if (n[&quot;转发&quot;]) stats.push(`转发🔁 ${n[&quot;转发&quot;]}`);
  if (n[&quot;引用&quot;]) stats.push(`引用❗ ${n[&quot;引用&quot;]}`);
  return stats.length &gt; 0 ? `【${stats.join(&quot; | &quot;)}】` : &quot;&quot;;
}

// Reddit热点
if (grouped[&quot;Reddit热点&quot;] &amp;&amp; grouped[&quot;Reddit热点&quot;].length &gt; 0) {
  md += `\n🔥 **Reddit热点**\n`;
  grouped[&quot;Reddit热点&quot;].forEach(n =&gt; {
    md += `- ${n[&quot;标题&quot;]} \n${formatStats(n)}\n${n[&quot;链接&quot;]}\n`;
  });
  md += `\n---\n\n`;
}

// YouTube热点
if (grouped[&quot;YouTube热点&quot;] &amp;&amp; grouped[&quot;YouTube热点&quot;].length &gt; 0) {
  md += `\n🔥 **YouTube热点**\n`;

  // 按视频类型分组（短视频、中视频、长视频）
  const typeMap = {};
  grouped[&quot;YouTube热点&quot;].forEach(n =&gt; {
    const type = n[&quot;来源&quot;] || &quot;其他&quot;;
    if (!typeMap[type]) typeMap[type] = [];
    typeMap[type].push(n);
  });

  for (const [type, videos] of Object.entries(typeMap)) {
    md += `\n**${type}**\n`;
    videos.forEach(v =&gt; {
      md += `- ${v[&quot;标题&quot;]}\n ${formatStats(v)}\n${v[&quot;链接&quot;]}\n`;
    });
  }

  md += `\n---\n\n`;
}

// Twitter热点
if (grouped[&quot;Twitter热点&quot;] &amp;&amp; grouped[&quot;Twitter热点&quot;].length &gt; 0) {
  md += `\n🔥 **Twitter热点**\n`;
  grouped[&quot;Twitter热点&quot;].forEach(n =&gt; {
    md += `- ${n[&quot;标题&quot;]} \n${formatStats(n)}\n${n[&quot;链接&quot;]}\n`;
  });
  md += `\n---\n\n`;
}

// TikTok热点
if (grouped[&quot;TikTok热点&quot;] &amp;&amp; grouped[&quot;TikTok热点&quot;].length &gt; 0) {
  md += `\n🎵 **TikTok热点**\n`;
  grouped[&quot;TikTok热点&quot;].forEach(n =&gt; {
    md += `- ${n[&quot;标题&quot;]} \n${formatStats(n)}\n${n[&quot;链接&quot;]}\n`;
  });
  md += `\n---\n`;
}

return [{ json: { markdown: md.trim() } }];

```

# 如何保证24小时运行

1.  部署在自己电脑上，保证24小时不关机不休眠

1.  购买官方n8n会员，开箱即用，**小白友好但贵**（https://n8n.io/）

1.  第三方云服务

1.  国内外知名云厂商的服务器，**小白不友好**

1.  Sealos云服务，平台已经部署好了n8n**，你只需要在里面注**册登录，就能完成部署，小白友好且没那么贵（https://bja.sealos.run/）

![](img/c2d04427ee92efdabce016b520437160.png)

![](img/a34aae3d31b2c3efca552559bca9a6a4.png)

部署后，访问公网地址，注册登录即可使用。

但注意，如果RSS链接里有外媒的话，左上角这里需要设置在国外，否则网络请求很有可能会失败。

![](img/a85244ff19fd54b2d45872c367cddc10.png)