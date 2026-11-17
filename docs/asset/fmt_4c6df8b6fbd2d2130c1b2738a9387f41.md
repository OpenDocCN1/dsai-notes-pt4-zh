
# [Agn-AI]用Agn-AI与AI老婆对话(调用openai/claude/text-generation-webui/kob

> 来源：[https://seigy6zzam.feishu.cn/docx/QUOJdtwwwotreGxfcJgcv3TdnNb](https://seigy6zzam.feishu.cn/docx/QUOJdtwwwotreGxfcJgcv3TdnNb)

> 🔗 原文链接：https://www.bilibili.com/read/cv23387641?spm_id_from=333.999.0.0

## 前言

本来视频已经做好了，但是因为不可抗拒力，只能转专栏了。

Agn-AI与TavernAI相比，可以对接的API更多（claude，scale等），还有Memory book，对手机端的UI界面适配更好（TavernAI的移动端界面适配一塌糊涂），目前我已经转用Agn-AI了。

Agn-AI有官方网页版，如果想体验一下在线版再考虑本地部署的话，可以去试试

`https://agnai.chat/`

本地部署最重要的是，聊天数据由自己掌控，不会出现跑路的情况。

![](img/11e53343bfba0f69dd6381b314107eff.png)

## 使用截图

![](img/95d141547d2862f15e08dfc65ab0f5ef.png)

## Agn-AI安装和使用教程

### 第一步：安装nodejs

①首先，进入`nodejs`官方网站 `https://nodejs.org/zh-cn`

选择左侧稳定版

下载安装，一路安装下去就行。

②检测`nodejs`是否正确安装

`win+r`，输入`cmd`，然后输入`node`

如果显示`Welcome to Node.js`等显示版本号等字样，说明`nodejs`安装成功，且已经成功配置系统变量。

![](img/e8f2f21b3868c6ddc2b291cb184df909.png)

### 第二步：安装Agn-AI

①打开Agn-AI的github项目页面：`https://github.com/luminai-companion/agn-ai`

下载最新文件（当然你也可以直接选择`git clone`）

解压文件夹

②对文件夹空白处右键，点击"在终端打开"。

依次输入

```
npm run deps
npm run build:all
```

`npm run deps`

`npm run build:all`

③经过这两个指令后，Agn-AI已经构建完成，可以随时启动了。但我建议你进行以下操作，对`openai`的`api`地址从

官方地址 (`https://api.openai.com`)

改成

`我使用vercel搭建的反代服务器 (https://r.cydrvp.top/proxy/api.openai.com)`

通过反代服务器，就不需要挂🪜就可以调用`openai`的`api`了。

修改方法：

打开Agn-AI文件根目录的`srv/adapter/`下的`openai.js`和`openai.ts`

均将这两个文件里的`baseUrl`变量中的 `https://api.openai.com`

```
https://api.openai.com
```

修改成 `https://r.cydrvp.top/proxy/api.openai.com`

```
https://r.cydrvp.top/proxy/api.openai.com
```

两个文件修改后记得保存。

![](img/795c34ffd09ffb1fc9ae7cc39e69f5de.png)

### 第三步：运行Agn-AI

在`cmd`终端输入(在`agn-ai`文件夹路径下)

```
node srv/bin.js
```

当出现类似于这种情况时，说明Agn-AI服务已成功运行。

在浏览器输入

```
http://127.0.0.1:3001/
```

便可进入Agn-AI界面。

往后如果要启动Agn-AI，在`agn-ai`文件夹路径下的`cmd`终端输入刚刚的指令即可

```
node srv/bin.js
```

![](img/46c7ffc6d82a28775c3517dc7ed07d48.png)

### 第四步：对接API

我演示两个常用的`api`对接，一个是`openai`（GPT3.5和GPT4），一个是`text-generation-webui`（即部署本地模型）。至于如何快速使用`Text-generation-webui`，我的最新视频就是介绍如何在`autodl`上部署，我目前已经在`autodl`发布了一键部署的镜像。

`https://www.bilibili.com/video/BV1CM4y1h73u`

#### ①对接`openai`篇（closeai）

打开左侧的`settings`选项

来到 `AI Settings` 选项，`Default AI Service` 处选择`OpenAI`

下方选择`OpenAI`的具体设置

`Default Preset`可以选择`Turbo`

然后下方填入你的`API KEY`

老惯例，我分享一个API，但我希望各位把这个API当作测试反代服务是否配置正确，而不是直接拿这个高强度使用，一天干完5美元。

```
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
```

最后点击右下角的`Update Settings`保存设置。

#### ② 对接`text-generation-webui`篇

我这里使用我在`autodl`上的发布的`text-generation-webui`镜像来做示范。

启动模型后

打开`autodl`的自定义服务，复制链接。

来到左侧的`Settings`

`Default AI Service`选择`Kobold`

下方`KoboldAI`设置里面，粘贴你复制的链接

最后点击右下角的`Update Settings`保存设置。

![](img/26b204bee35af85f4606334b129c4c50.png)

### 第五步：测试API对接情况

来到左侧的`Chats`，点击右上角的`New`

因为是测试，所以直接点击下方的`create`按钮开始对话。

输入`Hello!`

AI进行回复，代表`API`对接成功。

若无法回复，如果你是对接`Openai`，请检查你的`api key`是否无法使用或反代理服务器没有设置好；如果你是对接`text-generation-webui`，请检查`webui`是否可以正常使用，且启动参数是否添加了

```
--no-stream --api
```

![](img/e1f8ffd71033a297204a74e9db875baa.png)

### 第六步：自定义角色

这一步靠大家自己了，你可以在

这个网站：`https://zoltanai.github.io/character-editor/`

选择右边的`new character`

开始你的自定义人设（请使用英文）

最后上传角色的头像

下方按钮导出

选择左侧下载角色卡片

请注意，这张`png`图片包含了你刚刚输入的所有人设信息。

最后来到Agn-AI左侧的`Characters`选项

选择导入`png`图片

最后点击右下的`import`即可。

当然，你也可以直接在Agn-AI里选择`Create`按钮来创建角色（在`import`按钮的右侧）

这里我提供一张`catgirl`的角色卡，比较简陋，仅供参考。

难蚌，B站会压缩图片，无法正确导入。想要使用这个角色卡的话还是下载，阿里云盘链接：

```
https://www.aliyundrive.com/s/fqCbWkh2sCm
```

保存原图后导入即可。图片是用`stable-diffusion-webui`的`AOM3`模型生成的。

最后在`Chat`中根据你自定义的角色，进行聊天。
