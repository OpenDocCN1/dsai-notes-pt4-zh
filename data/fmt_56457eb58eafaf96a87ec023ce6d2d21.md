
# FigmaMCP实现代码⇄设计稿实时双向同步（实测）

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/HkJ1dzyFeovOglxPhMUck2UznJg](https://qqs7y1hozd1.feishu.cn/docx/HkJ1dzyFeovOglxPhMUck2UznJg)

## 一、Codex 安装流程(超简单)

主要用Codex APP来给大家演示，因为这种方式装起来是真的丝滑，直接点点点就完事。

### 安装Figma技能

更新Codex APP到最新版，在“技能”推荐列表里已经内置了Figma技能，点击“+”号直接安装：

![](img/7579e0e3f60ec3dea942276d4acae307.png)

### 安装Figma MCP

打开设置，在“MCP服务器”推荐列表里同样已经内置了Figma MCP，点击安装：

![](img/3aeafc050cbf774e130729e6dd802b81.png)

### 身份验证

MCP安装完成后会看到“进行身份验证”提示，点击后会打开浏览器登录Figma并授权：

![](img/bfeb9c55a5a6c3354f44bd5e07437d96.png)

进行身份验证

## 二、Claude Code安装

### Claude Code

执行一行命令:

```
claude mcp add --transport http figma https://mcp.figma.com/mcp
```

想全局生效加个`--scope user`:

```
claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp
```

启动Claude Code后输入`/mcp`，选择figma进行身份验证即可。

### Codex CLI

同样执行命令安装:

```
codex mcp add figma --url https://mcp.figma.com/mcp
```

按提示授权，然后就能在终端里用了。

## 三、实战：代码推送到Figma(太爽了)

为了验证效果，让Codex做了个“项目进度追踪单页面”，直接：“发送到Figma”。

**Codex做了什么**：自动在代码里插入：

```
&lt;script src=&quot;https://mcp.figma.com/mcp/html-to-design/capture.js&quot; async&gt;&lt;/script&gt;
```

![](img/36ee64de5f409334bb314e830476c5a1.png)

> 然后启动本地服务，打开浏览器，界面上会出现一个Figma捕获工具栏，有三个按钮:

+ `Entire screen`：推送整个页面

+ `Select element`：选中某个元素单独推送

+ `Open file`：跳转到Figma查看推送的文件

![](img/d8daf800396291a955e4258fcfc284ea.png)

![](img/9c447b5f10e094c186d6499454573e2a.png)

点击“Open file”就能在Figma里看到推送的设计稿了：

![](img/338e146fd4739644212c26c959d7a130.png)

**支持选中元素单独推送**。

比如我只想推送“Task Board”这个模块，选中后点击一下：

![](img/159bac3d5bfd7a06b554db1eba54df2a.png)

Figma里立刻就有了：

![](img/888482bbb91000a226734088a3e1cbd9.png)

## 实战：Figma修改同步回代码(这才是真正的双向)

设计稿推上去了，设计师改了颜色怎么办**？这就到了体现**官方MCP牛逼的地方了——真正的双向同步。

**操作很简单**：右键“Task Board”，选择“Copy/Paste as” → “Copy link to selection”：

![](img/6b85e2b2ef3593d9e323dd1c16702d56.png)

![](img/25f8d0ca54a7905c61eda75a160a31f9.png)

回到Codex，输入“更新UI”加上刚复制的链接：

Codex会调用Figma MCP的`get_design_context`工具读取修改，然后自动调整代码。

**支持选中元素单独推送**。

比如我只想推送“Task Board”这个模块，选中后点击一下，Figma里立刻就有了

![](img/b2444c76507a7d1e5ca1b589a2a02a50.png)

![](img/6232f85996428f083c678daf95908232.png)

点击“Open file”就能在Figma里看到推送的设计稿了

![](img/f6529bbf7c7d17c96c9129182ac9d2cb.png)

![](img/abbde8b61131597cd0c0700e042cddf5.png)

## 关于调用次数限制

需要注意的是，`generate_figma_design`(UI推送)、`add_code_connect_map`、`whoami`这几个工具不受速率限制。也就是说推送UI和确认身份这些核心操作不占用配额。
