
# 基于MCP搭建天气系统，到 AI + Excel 数据自动化处理

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/LvjedrRi7ohuWwxh6ojccOM8nJ3](https://qqs7y1hozd1.feishu.cn/docx/LvjedrRi7ohuWwxh6ojccOM8nJ3)

## 利用trae以及第三方MCP Server搭建一个天气系统网页

接下来，我们就展示下，如何在trae中使用mcp进行天气系统网站的搭建操作

下面是整个天气系统的布局以及最终效果展示

![](img/7d024c15dfa2b0f25581290de384aa11.png)

![](img/fb08216be3ea943bd31e4195954c9bea.png)

### 链接高德地图MCP

我们第一次在使用的时候是需要进行MCP服务的添加的

![](img/0ee5a476f994b0089252669ee8555a68.png)

我们这里需要使用到高德的MCP Servers

我们这里的市场是没有的，所以我们进行手动添加配置

### 点击手动配置

![](img/614faf0f0bcbf3f442140694f65a7bfd.png)

将下面的代码输入进去

```
{
  "mcpServers": {
    "amap-amap-sse": {
      "url": "https://mcp.amap.com/sse?key=您在高德官网上申请的key"
    }
  }
}
```

这里的我们需要去申请一个自己的API KEYS了

点击进入到高德平台

进行注册并且登录操作

在高德平台右上角进行应用的创建

![](img/b0e2b70d01403c8fc543883dc186a6aa.png)

创建好了系统之后我们进行添加Key的操作

![](img/04298a7b04844887a45b6b83c741416f.png)

这里的话我们就选择了Web服务了

![](img/fd0f0611eb05c2e457c46478727b27f2.png)

然后点击提交就能看到我们的API Keys了

![](img/465a00785277a3124adff9900d7b8207.png)

为了对 `Key` 的安全有效管理，请妥善保管你的 `Key`。

然后获取到了API之后，我们将这个API替换到原代码中的部分

![](img/664c17b232409cc6ab83a7efcd4cde25.png)

![](img/0e77426b2c2cacb22de8b1c2417301da.png)

配置好了之后并且链接成功了就是这个样子的

### 链接quickchart-server MCP Server

![](img/749d1f06708068c7751f9eb3cccc89ce.png)

它是一个基于 TypeScript 的服务器，集成了 quickchart.io 基于 URL 的图表生成服务。通过 MCP Server，用户可以通过提供数据和样式参数，使用 `Chart.js` 配置创建各种类型的图表，服务器会将这些配置转换为图表 URL 或可下载的图像1。

我们的思路就是将天气反馈，让这个做一个图表

我们还是一样的步骤，将下面的代码放到创建 MCP 里面

```
{
  "mcpServers": {
    "quickchart-server": {
      "command": "node",
      "args": ["/path/to/quickchart-server/build/index.js"]
    }
  }
}
```

这款 MCP Servers 支持的图表类型还是比较多的

![](img/359b9595ac8c1fbf387ef6976f80c3fb.png)

部署好了就是下面的样子

### 链接EdgeOne Pages Deploy MCP

`EdgeOne Pages Deploy MCP` 是一项专用服务，能够将 HTML 内容快速部署到 `EdgeOne Pages` 并生成公开访问链接。这使您能够立即预览和分享 AI 生成的网页内容。

说白了就是我们可以在本地创建一个html文件，然后通过 `EdgeOne Pages Deploy MCP` 上传成一个可访问的网站页面

还是一样的操作

```
{
  "mcpServers": {
    "edgeone-pages-mcp-server": {
      "command": "npx",
      "args": ["edgeone-pages-mcp"]
    }
  }
}
```

部署好了就是下面的这个样子

![](img/245f3a95747e264584740f3feeb1140c.png)

### 智能体的创建

![](img/9e7505c9bf3ebf1379cfc525e5d03600.png)

这里我们可以看到有一个提示：MCP Servers需要添加到智能体中才能使用

那么我们就创建一个智能体就行了

点击创建

![](img/e5679c05df9d79770bcfdc4c617bc43e.png)

将我的提示词放进去

```
# 角色  
你是一位专业的气象数据分析师，能够查询任意城市的未来4天天气，并生成可视化图表。  
  
## 技能  
### 技能 1: 查询天气数据  
- 使用高德MCP这个MCP服务，查询任意城市未来7天的天气数据。  
- 获取的数据应包括但不限于温度、湿度、风速、降水量等关键气象指标。  
  
### 技能 2: 生成可视化图表  
- 将查询到的天气数据用QuickChart这个MCP服务转换为易于理解的可视化图表。  
- 可视化图表可以是折线图、柱状图或热力图等形式，具体取决于用户的需求和数据特性。  
- 确保图表清晰、美观且易于解读。  
  
### 技能 3: 提供天气分析报告  
- 根据查询到的天气数据，提供简要的天气分析报告。  
- 报告应包括对未来4天天气趋势的总结，以及可能对用户活动产生影响的建议。  

### 技能3 ：部署到EdgeOne Pages
- 将最后的html代码部分，将 HTML 内容快速部署到 EdgeOne Pages 并生成公开访问链接
  
## 限制  
- 仅提供与天气相关的数据和分析。  
- 生成的图表必须基于查询到的实际天气数据。  
- 如果需要调用外部工具或API，请明确说明并提供相应的调用方法。  
- 所有输出的内容必须准确无误，不得包含任何误导性信息。

```

并且这里创建智能体还有一个好处，就是你用的啥服务就调用啥类型的MCP，不会造成额外扣费的情况

### 天气系统效果展示

1. 那么到这里了，我们的这个智能体就用到了三个MCP Servers了

2. 我来分析下，使用高德地图MCP Servers进行天气情况的获取操作

3. 利用quickchart-server MCP Server来进行表格的创建操作

4. 利用EdgeOne Pages Deploy MCP进行可视化网站的部署搭建操作

那么到这里我们就开始下面的部署操作了

我们直接在trae的builder模式这里进行对话，

```
利用高德MCP帮我获取湖北武汉最近7天的天气，利用quickchart-server MCP Server帮我进行图标的构建，利用EdgeOne Pages Deploy MCP帮我进行网站的部署操作

代码格式为html

```

然后他会逐步进行代码的构建并且利用我们的MCP SERVER进行构建操作

最值得夸赞的就是每次他都会询问我们是否执行这一步，这个行为在别的编译器是没见过的，对于小白真的很友好

![](img/0758299607df4ad95690ae418eb48f6c.png)

并且在他的回答中，就将访问的链接给我们了

访问链接：传送门

下面是效果展示

![](img/f54c749237b0d1db4510a11392b30738.png)

代码如下：
