
# 第一部分：浏览器插件开发与问题排查

开干环节可能没多少，但是这里问题的环节其实开发的重点。

一直在不停的跟 `claude code` 交互，让他解决问题。

比如：下面的问题

## 问题排查

### 问题一：爬虫关键词触发 anthropic 关键字

大家仔细看红字说的，并且还有个链接，应该是触发了 Anthropic 公司的使用策略。

我猜测爬虫/采集这些都是不能让 ai 去干活的。

![](img/0077caf94a8c694abd906fe3eb485a33.png)

那么解决方法很简单：你就让他换个别的方式达到你的目的，如果不好使的话，也没关系，后面多的是机会来解决这个问题。

> 所以在做这些工作的时候，尽量避免正面提爬虫/采集这些对于 Anthropic 的违禁词。

### 问题二：无法采集数据

```
页面检测结果: {isValid: false, url: &#x27;https://www.xiaohongshu.com/user/profile/68497faa0…7HFRUObas-vCl_PSLHKnTLshQwak=&amp;xsec_source=pc_feed&#x27;, userId:   null, message: &#x27;请访问小红书个人主页（格式：https://www.xiaohongshu.com/user/profile/xxx）&#x27;}，页面已经检测倒是 https://www.xiaohongshu.com/user/profile/ 开头的格式，但是还是“页面不可采集“的术语，并且布局不太合理，[Image #1]
```

![](img/5af5d8986e2158123bc675bcb04d1bcd.png)

### 问题三：`popup` 布局问题

![](img/99686d320a5c005616a45a861d350944.png)

### 问题四：无法弹出侧边栏的问题

![](img/7815af741db15f264a41670255916418.png)

### 问题五：信息采集不全的问题

![](img/f0d91a3bc53e10ee528250b0270ece92.png)

### 结果查看：采集到用户数据

![](img/273db148111b2724f42ce5240ba6fed7.png)

### 问题六：采集文章数据排查

```
采集文章的数据，需要点击每个打开后才能采集到标题、正文这些元素的。ultrathink
```

中间又调整了很多策略

```
采集策略，先不滚动页面，等采集完用户数据后才开始采集文章数据，默认进来就会有一些文章，先采集已经在页面上的文章数据，需要有鼠标点击打开文章的操作，然后再采集文章的要素，比如标题、正文、评论等，ultrathink，补充信息，正常人点击的是 class=&quot;note-item&quot; 下的class=&quot;cover&quot; 的a标签
```

![](img/f7ca39a004f13054a5ecb24c94cfef9c.png)

### 结果查看：最后总算看到了数据

![](img/9b643aea0f09a6b1b34a5da1609547a9.png)

采集到的详细数据：

![](img/91507bcde6496655e96a0c33a5d4c1f1.png)

可以进行导出为 json 的数据

## 步骤七：同步 n8n 的配置

这里其实要到 n8n 的工作流配置后，才能到这个地步。

为了调试方便，我把前面的数据，使用了 APIFox 这个软件进行接口的调试。

> https://apifox.com/

> 是一个 http 接口调试工具，对于小白来说可能太高深。

> 可以跟 AI 对话：http 是什么，接口是什么？我是个小白，帮我解释下，别人用APIFox 这个工具是干嘛的？

![](img/95133dab3de68004684066147c0b86d9.png)

如果调试好接口后，可以拷贝代码，让 `claude code` 知道请求参数和响应体是什么样的。

避免接口参数对不上，调试麻烦。

> 当然也可以写好接口文档，因为这里只有一个接口，所以就直接粘贴到对话框提示词里面了。

```
同步n8n接口数据的参考代码为：{粘贴刚刚复制的 APIfox 代码}
```

![](img/c479fe7314789d6f5c18c6c3501679ed.png)

返回数据格式：

```
{
    &quot;success&quot;: true,
    &quot;message&quot;: &quot;✅ 数据保存成功&quot;,
    &quot;errors&quot;: [],
    &quot;details&quot;: {
        &quot;authorSaved&quot;: true,
        &quot;notesSaved&quot;: 114,
        &quot;totalProcessed&quot;: 115
    },
    &quot;timestamp&quot;: &quot;2025-08-20T09:30:24.997Z&quot;
}
```

![](img/526317053d383bf14fe4d65714a8f6f4.png)

这里继续调试返回接口的数据格式：

![](img/771f70fbc363356863b5fdd15f8bdac5.png)

![](img/71f8483b989d8591f339bd7777e96511.png)

## 步骤八：同步交互体验修复

这里主要是对采集数据的一个同步优化的提升功能。

```
[Image #1]，每次采集都会新增一个记录，我理解是不是只同步每次最新的那个就行，还是说每个采集的记录都可以有同步的按钮操作，深度思考下这个交互怎么好做。ultrathink
```

![](img/68c9ce41324ba522b08584418474674c.png)

## 步骤九：界面原型开发问题

这里如下图，是个界面的布局问题。

其实这个问题修复了好久。搞了好几次。

![](img/45ad36c5e0d48633f2e0e7073a011ee1.png)

```
[Image #1]，看这个布局，使用ascii复现给我画出来，我好确认你已经知道我的问题
```

这里的有个小技巧不是布局问题，可以让 `claude code` 可以用 ascii 图画出来。

一是确认`claude code` 理解了你的问题。

二是确认 `Claude code` 找到了解决办法。

![](img/91e29c7e03fa555a33a93f9d1ffdc07c.png)

没解决，就继续

> 加入 `ultrathink`

```
实际“采集数据“，“查看已采集数据“ 两个按钮还是太长，看是否可以做成自动适应父容器的，离父容器有个`padding`的距离。ultrathink
```

![](img/a3d2c3b168b95599f00c930354ea1df1.png)

## 步骤十：调试插件

打开谷歌浏览器

![](img/1b2552225cf750d6d17e2293f9778db8.png)

一定要打开开发模式

![](img/cd1139690978ad0d0c02d94fe1487346.png)

然后就可以看见左边的三个按钮

![](img/488d76dc2ec07e13e17509f8e4b42e1b.png)

选择目录。

一般是在项目目录下的 `dist` 目录。

![](img/5370a517e39426781df2f6aa2e5c974a.png)

浏览器插件记得置顶下，就可以看见了。

![](img/0d6b2ffbd6db2eb993af23200c98c4ea.png)

调试的方法：

![](img/f00aeb3f7b09f3e55d0b44aee266fa7e.png)

如果要调试网络请求，可以切到 network 界面

![](img/e45a60066cd5f6b52f92d1b481ec1515.png)

## 最后的总结

到这里基本就完成了浏览器的工作开发

> 其实有很多确实要有一些基础知识，比如 xpath 表达式，还有 http 接口，F12调试窗口。

> 还有一种方法，你搞不定，你可以找个搞得定的人来帮你搞定。

# 第二部分：n8n agent 开发

> 到这里就更精彩了，可以说是全文的精华部分，也是最精彩的部分。

> 建议你倒好茶后，慢慢细品。

> 这里主要就是下面几个点

> 1、n8n 的工作都是 `claude code` 开发，我没有配置一个

> 2、接入 AI，生成个性化的博主的报告

> 3、数据都存储在数据库中（supabase）

## 步骤一：准备建表的工作

因为数据要存储，所以需要建表工作。

> 这里可能又要难道小白了，因为涉及后端数据库的概念，还有表的语法。

> 不懂记得问 AI，如今时代学习能力我觉得是最需要的能力。

### 1、建表需求

提示词如下：

![](img/8e8a97861eb054884026ac1dabcdac57.png)

主要就是把 json 的数据结构，文章的和博主的都需要粘贴到提示词里。

这样 `claude code` 可以很精准的推断出需要的表的建表语句。

```
{这里粘贴 json 的数据格式数据}

上面的数据是小红书的笔记和作者的数据，我希望有2个 supabase 的表来存储，分别是作者列表，以及笔记列表。

其他要求如下：
- 笔记列表就是保存对应的笔记数据，同时需要有一个作者id的字段能够联系起来。
- 作者表需要额外添加一个 report 字段
- 表不需要外键约束
- 暂时不需要 RLS 策略

请你给到我在supabase中创建这两个表的SQL语句，保存到 `@supabase.sql` 文件中
```

### 2、创建需要存储数据的表（supabase）

这里就是让 `claude code` 创建 sql 的建表语句。

```
@prd_ddl.md 根据文件中的描述，生成所需要的sql内容
```

![](img/497efa6399c7b5a9eb023eccfb9b28cd.png)

创建了 sql 文件

![](img/f144744b1ddd95402d15f06b12b31caa.png)

supabase 执行 sql

![](img/afacbc4bb27ce2a8ea981dbb3bda0979.png)

> 这里有 supabase 的概念。

> 可以说就是云数据库，很方便集成了很多功能。

> 现在基本建站都会基于 supabase 来弄。

> supabase 也是本地整个系统最要的组件之一。

> 相当于电脑的硬盘，存储着从📕采集的数据。

> 后续我们的系统也会从这里读取数据。

> 所以是屯粮基地，不容许有失。

> supabase 的使用很简单，注册完后。

> 创建一个项目就可以用了。

## 步骤二：n8n 工作流 json 配开发

> ！！！重点！！！重点！！！重点！！！

> 它来了，它来了。

> 这里就是使用 `claude code` 开发 n8n 工作流的重点。两种方式

> 方式一：直接生成 n8n 的 json 配置文件

> 方式二：使用 `n8n-mcp` 修改工作流配置。

> 废话少说，开干。

### 1、准备 n8n mcp 环境

很简单：直接启动 mcp 服务

参考地址：https://github.com/czlonkowski/n8n-mcp

> 前提装好 nodejs 环境。

```
npx n8n-mcp
```

然后在项目的目录下新建 `.mcp.json` 文件，配置如下

```
{
  &quot;mcpServers&quot;: {
    &quot;n8n-mcp&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;n8n-mcp&quot;],
      &quot;env&quot;: {
        &quot;MCP_MODE&quot;: &quot;stdio&quot;,
        &quot;LOG_LEVEL&quot;: &quot;error&quot;,
        &quot;DISABLE_CONSOLE_OUTPUT&quot;: &quot;true&quot;,
        &quot;N8N_API_URL&quot;: &quot;你的n8n 地址&quot;,
        &quot;N8N_API_KEY&quot;: &quot;你的 n8n key 配置&quot;
      }
    }
  }
}
```

`N8N_API_URL`：你的n8n 地址，类似：https://xxx.xxx.xxx/。也就是你访问 n8n 系统的地址。

`N8N_API_KEY`：你的 n8n key 配置。

进入到 n8n的设置页面，在「n8n API」菜单页面进行设置就可以。

> 其实这里 `N8N_API_URL` 和 `N8N_API_KEY` 的不设置，也可以写出json 的工作流文件。

> 这里配置就是为了

![](img/66664eb5b7ed971fe1eb0af79768ca00.png)

> 这里说下 n8n 的环境有很多种。

> 大多数的都是本地 docker 搭建的，这个大家可以看龙哥的文档，写的很详细。

> 我有云服务器，直接用的 zeabur 的模板，搭建在自己的 zeabur 管理的服务器上。很快就可以搞定环境。

## 步骤三：写需求文档

又到了这个步骤，所以说现在的开发流程并没有减少，并且对个人的要求能力越来越高。

因为我发现，我现在代码写的越来越好，需求文档写的越来越多了。

### 1、n8n 工作流的需求文档

![](img/3e98f7900cedd998bba1f330e51bc645.png)

> 主要就是的关键点：

> 1、写的是 n8n json 文件

> 2、使用 webhook 的触发方式

> 3、使用 ai 写博主的特殊报告

> 4、保存数据到数据库中

```
帮我编写一个n8n的agent的对应的json文件，这个agent的要求如下：

1. 能够以webhook的方式触发，POST请求，接受的数据的结构示例如下，这其实是一份小红书中获取到的创作者的相关数据，包含作者信息和笔记信息，@data.json

2. AI Agent接收到这个数据之后，能够根据数据中的信息，分析出这个用户的一些特征，并生成一个创作者的分析报告，报告要包含以下的主题：数据概览、赛道分析、标题风格分析、笔记风格分析、热门笔记分析。把分析的报告保存到`report`字段中

3. AI Agent分析完成后，需要解析对应的数据，并且分别把数据保存到对应的表中，分别有两个表，`xiaohongshu_authors` 表和 `xiaohongshu_notes` 表，分别对应作者列表和笔记列表，结构见文件 @supabase.sql 

4. 2个数据保存成功后，能够返回一个true，如果保存失败，返回false

其他要求：
1. ChatModel 选择 OpenRouter，模型使用 deepseek
2. 报告应该体现专业性
3. AI Agent 的 prompt 要求能够完成我们上述的要求
```

当然需求文档也是借鉴了下别人的，感谢 yihui 大佬。

### 2、`claude code`开发json工作流阶段

直接就艾特需求文档，让 `claude code` 开发了。

```
@prd_n8n.md 根据这个文档的需求，并使用 n8n-mcp 生成对应的n8n的配置json文件，ultrathink
```

注意：前期都是使用 plan 模式，并且我加上了 `ultrathink`。

![](img/bbbf4edb78e838ea69114285b73fde05.png)

得到执行计划

![](img/533bd7f066bb35381e26db3b99741f8e.png)

> 其实这里可以看到有每个节点

> 后续生成的时候，每个节点都会调用 n8n mcp 获取详细配置信息。

> 这样 `claude code` 就知道怎么生成这个节点的配置信息。

> 特别是需要处理数据的地方，就是用代码节点，而写代码是 `claude code` 的强项。

经过开发后就成功创建了 json 的配置文件

![](img/8d5b45e66a7431813701e24a04b538bd.png)

其实到这里，我还半信半疑，这个 json 文件能用吗？

那我们就试试不就知道了

### 3、json 文件导入到 n8n 工作流里面

直接新建一个 n8n 项目。

然后找到项目的右上角的三个点，选择导入文件。

![](img/b4ddcceb191db2f4597721c80f558d93.png)

选择我们刚刚生成的 json 文件就可以。

### 4、设置使用的 AI KEY 信息

> 可以看到如下界面。当时我还是惊讶了下，我啥也没干，就是写了个需求文档。

> 这个导入后居然还是像模像样的。

> 目前足矣证明`claude code` + n8n 的方式是可行的。

![](img/3c39ccd40deb3506fbe442a52b0c9547.png)

不过有几个节点有些红色的叉，没关系，就是需要设置下对应 `openrouter` 和 supabase 的信息。

1、`openrouter` 的好填，直接填 API KEY 就行

2、supabase 的信息

![](img/41754920990cccb1d3aad4dc31c31ed1.png)

第一个 host 的地方：

![](img/67b986f43c1f83abb908832500654ba0.png)

第二个：Service role secret

![](img/142394103b9472874179d7e8f1e42037.png)

填好信息后就可以看见红色的❌消失了。

### 5、测试整个n8n工作流

下一步就是测试了。

点击工作流下面的「Execute workflow」，就进入了测试模式。

![](img/c9e2ee7ee2c97291d326f6eb508b97e0.png)
