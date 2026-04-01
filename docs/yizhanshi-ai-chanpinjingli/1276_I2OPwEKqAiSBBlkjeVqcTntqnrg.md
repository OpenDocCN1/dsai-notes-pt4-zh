# 小红书自动发布助手！用本地Cherry-Studio客户端搭建

> 来源：[https://v11enp9ok1h.feishu.cn/docx/VKWXdIB5NoMDbMxI9MfcIxUVn9u](https://v11enp9ok1h.feishu.cn/docx/VKWXdIB5NoMDbMxI9MfcIxUVn9u)

# 项目说明

---

### 安装

安装 ChromeDriver：

1.  查找你的 Chrome 浏览器版本（例如 "134.0.6998.166"）

1.  运行命令下载对应版本：

```
npx @puppeteer/browsers install chromedriver@134.0.6998.166
```

1.  将 ChromeDriver 复制到系统路径或添加路径到环境变量中

### 登录

在终端运行以下命令（请使用绝对路径指定 PATH_TO_STORE_YOUR_COOKIES，例如 /Users/Bruce/。该 MCP 服务器会将你的 cookie 存储在此路径）：

```
env phone=YOUR_PHONE_NUMBER json_path=PATH_TO_STORE_YOUR_COOKIES uvx --from xhs_mcp_server@latest login
```

终端会显示：

```
无效的 cookies，已清理  
请输入验证码:  
```

此时需在终端输入接收到的验证码并回车。

### 验证登录

```
env phone=YOUR_PHONE_NUMBER json_path=PATH_TO_STORE_YOUR_COOKIES uvx --from xhs_mcp_server@latest login
```

若成功会显示：

```
使用 cookies 登录成功
```

### 检查工具（Inspector）

在终端启动检查工具：

```
npx @modelcontextprotocol/inspector -e phone=YOUR_PHONE_NUMBER -e json_path=PATH_TO_STORE_YOUR_COOKIES uvx xhs_mcp_server@latest
```

在检查工具中，你可以使用本地图片：

输入图片路径（例如 ["C:\\路径\\到\\你的\\图片.jpg"]），图片路径需要用双引号包裹。

注：发送时可能会显示 "Error Request timed out" 警告，但实际帖子仍会发布成功。

### 启动服务器

#### 方式1：直接运行命令

```
env phone=YOUR_PHONE_NUMBER json_path=PATH_TO_STORE_YOUR_COOKIES uvx xhs_mcp_server@latest
```

#### 方式2：配置文件设置

在配置文件中添加以下内容：

```
{
    &quot;mcpServers&quot;: {
        &quot;xhs-mcp-server&quot;: {
            &quot;command&quot;: &quot;uvx&quot;,
            &quot;args&quot;: [
                &quot;xhs_mcp_server@latest&quot;
            ],
            &quot;env&quot;: {
                &quot;phone&quot;: &quot;YOUR_PHONE_NUMBER&quot;,
                &quot;json_path&quot;: &quot;PATH_TO_STORE_YOUR_COOKIES&quot;
            }
        }
    }
}
```

# 让AI自动发布小红书笔记

## step1：打开命令行，运行以下命令安装环境

```
# 1、安装Node.js和UV，如果没有python环境，请自行安装
brew install node
brew install uv
# 2、安装xhs-mcp-serve
pip install xhs-mcp-server
# 3、安装chromedriver
npx @puppeteer/browsers install chromedriver@134.0.6998.166
```

## step2：登陆小红书账号（只需1次）

+   在终端运行以下命令，后台会自动弹出浏览器，并打开小红书创作平台的登陆页面，自动发送验证码。

⚠️将YOUR_PHONE_NUMBER改成自己的手机号

⚠️将PATH_TO_STORE_YOUR_COOKIES改成cookies的保存目录，比如'/Users/xxx/'

```
env phone=YOUR_PHONE_NUMBER json_path=PATH_TO_STORE_YOUR_COOKIES uvx --from xhs_mcp_server@latest login
```

+   检查手机收到的验证码，在终端中输入

```
无效的cookies，已清理
请输入验证码:
```

+   再次运行上面的那行命令，如果显示“使用cookies登录成功”就登陆成功啦。

## step3（可选）：用inspector调试一下发布功能

+   启动inspector调试界面

⚠️同样需要设置YOUR_PHONE_NUMBER和PATH_TO_STORE_YOUR_COOKIES，后者请用上面登陆时的cookies的目录，比如'/Users/xxx/'

```
npx @modelcontextprotocol/inspector -e phone=YOUR_PHONE_NUMBER -e json_path=PATH_TO_STORE_YOUR_COOKIES uvx xhs_mcp_server@latest
```

+   打开inspector调试界面，List Tools里面你会发现有两个tool，create_note和create_video_note，前者是发布图片笔记，后者是发布视频笔记。

💡如果你不熟悉inspector的操作，不知道怎么找到List[ Tools，请参考魔搭更详细的教程：https](https://modelscope.cn/headlines/article/1139)://modelscope.cn/headlines/article/1139

点击create_note，输入标题、内容和图片，其中图片支持网络图片URL，也支持本地图片路径。

以下是本地图片的路径的例子：

![](img/dc326ab9684779c3b87567994cf871e6.png)

+   点击Run Tool，后台会自动拉起浏览器，打开创作服务平台的发布入口，自动上传图片和填写文字内容并发布出去。

![](img/31dcbd60d2185612739c768043e476fa.png)

+   发布完成的界面

![](img/6c3e1a054c2ce357458dab8acdf246cc.png)

## 让Qwen3帮我们生成文案，自动发布小红书

在客户端上配置小红书发布助手，借助Qwen3-235B模型的强大能力来帮我们生成标题和正文的文案。

Qwen3-235B-A22B模型已在魔搭部署为API服务，可通过API-Inference免费调用。

模型地址：[https://modelscope.cn/models/](https://modelscope.cn/models/Qwen/Qwen3-235B-A22B)Qwen/Qwen3-235B-A22B

我们在Cherry Studio上配上Qwen3:

![](img/ea174615c862b8ad07fa20cf618af4cf.png)

## 再在MCP服务器里配置小红书发布MCP

小红书MCP魔搭地址：[h](https://www.modelscope.cn/mcp/servers/@XGenerationLab/xhs_mcp_server)ttps://www.modelscope.cn/mcp/servers/@XGenerationLab/xhs_mcp_server

```
&quot;xhs-mcp-server&quot;: {
      &quot;name&quot;: &quot;xhs-mcp-server&quot;,
      &quot;type&quot;: &quot;stdio&quot;,
      &quot;isActive&quot;: true,
      &quot;registryUrl&quot;: &quot;&quot;,
      &quot;command&quot;: &quot;uvx&quot;,
      &quot;args&quot;: [
        &quot;xhs_mcp_server@latest&quot;
      ],
      &quot;env&quot;: {
        &quot;phone&quot;: &quot;YOUR_PHONE_NUMBER&quot;,
        &quot;json_path&quot;: &quot;PATH_TO_STORE_YOUR_COOKIES&quot;
      }
    }
```

⚠️同样需要设置YOUR_PHONE_NUMBER和PATH_TO_STORE_YOUR_COOKIES，后者请用上面登陆时的cookies的目录，比如'/Users/xxx/'

然后让它写个关于小红书发布MCP的简短介绍文案：

![](img/ce3af9b9137e337a9e55f1c8653f2662.png)

再打开MCP服务器，配上我自己的本地视频地址：

![](img/65f2a1f8553ee97f10c9aafdac54652a.png)

然后它就刷刷刷开始干活了～

自动打开浏览器发布视频，发布成功后自动关闭浏览器，整个过程非常丝滑。

![](img/55fb1e26e850f9f96c3e3c4b574aba3e.png)

![](img/325d41d740a294f11ef2c3f5e48ddb93.png)

## 让AI帮我们生成图片或视频，自动发布小红书

在Cherry Studio MCP服务器里配置文生图MCP-server，魔搭上目前有几个支持生图的MCP，比如ModelScope-Image-Generation-MCP和MiniMax-MCP，两者都在魔搭的云端资源上部署，可以支持SSE的方式调用。值得提一句，前者是免费的！

魔搭文生图MCP地址（免费）：https://www.modelscope.cn/mcp/servers/@modelscope/ModelScope-Image-Generation-MCP

MiniMax-MCP地址：[h](https://modelscope.cn/mcp/servers/@MiniMax-AI/MiniMax-MCP)ttps://modelscope.cn/mcp/servers/@MiniMax-AI/MiniMax-MCP

最重要的是，这些服务完全不需要手动一个个的配置！

因为在Cherry Studio里集成了ModelScope托管的MCP服务。你只要在Cherry Studio的MCP服务器页面，填入 ModelScope的 API令牌把服务器一键同步过来，就可以在Cherry Studio上使用所有魔搭已经托管的MCP服务了，非常的方便。

![](img/ff6e609b56875cab78b8da4b74c85971.png)

## 同步完后，可以在chat界面的MCP列表中找到：

![](img/46e319610921dfde72d0a40877d0be6c.png)

准备工作完毕，下面让我们开始

这次我们让它介绍一下介绍一下魔搭今日上新的qwen3模型

会自动调用tavily-search搜索资讯，总结文案

![](img/bcbf68ecba17124c3d376dc62acd5422.png)

**然后我们让它生成一张配图**

![](img/0347bd3f965fc7e73fe437ebc39338c9.png)

最后让它用生成的图片和文案发布笔记

![](img/962884f46759941c9fe75a3fed19de98.png)

这下又可以看到AI自动打开浏览器干活的样子。

![](img/3112d4956c616aa8ea6abda1d9fbec59.png)