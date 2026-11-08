# [Agn-AI]用Agn-AI与AI老婆对话(调用openai/claude/text-generation-webui/kob

> 来源：[https://seigy6zzam.feishu.cn/docx/QUOJdtwwwotreGxfcJgcv3TdnNb](https://seigy6zzam.feishu.cn/docx/QUOJdtwwwotreGxfcJgcv3TdnNb)

> 🔗 原文链接： h[ttps://www.bilibil](https://www.bilibili.com/read/cv23387641?spm_id_from=333.999.0.0)i.com/read/cv233...

# **前言**

本来视频已经做好了，但是因为不可抗拒力，只能转专栏了。

Agn-AI与TavernAI相比，可以对接的API更多（claude，scale等），还有Memory book，对手机端的UI界面适配更好（TavernAI的移动端界面适配一塌糊涂），目前我已经转用Agn-AI了。

Agn-AI有官方网页版，如果想体验一下在线版再考虑本地部署的话，可以去试试

https://agnai.chat/

本地部署最重要的是，聊天数据由自己掌控，不会出现跑路的情况。

![](img/11e53343bfba0f69dd6381b314107eff.png)

# **使用截图**

![](img/95d141547d2862f15e08dfc65ab0f5ef.png)

# **Agn-AI安装和使用教程**

# **第一步：安装nodejs**

①首先，进入nodejs官方网站 https://nodejs.org/zh-cn

选择左侧稳定版

下载安装，一路安装下去就行。

②检测nodejs是否正确安装

win+r，输入cmd，然后输入node

如果显示Welcome to Node.js等显示版本号等字样，说明nodejs安装成功，且已经成功配置系统变量。

![](img/e8f2f21b3868c6ddc2b291cb184df909.png)

**第二步：安装Agn-AI**

①打开Agn-AI的github项目页面：https://github.com/luminai-companion/agn-ai

下载最新文件（当然你也可以直接选择git clone）

解压文件夹

②对文件夹空白处右键，点击"在终端打开"。

依次输入

```
npm run deps
npm run build:all
```

npm run deps

npm run build:all

③经过这两个指令后，Agn-AI已经构建完成，可以随时启动了。但我建议你进行以下操作，对openai的api地址从

官方地址 (https://api.openai.com)

改成

**我使用vercel搭建的反代服务器 (http**s://r.cydrvp.top/proxy/api.openai.com)

通过反代服务器，就不需要挂🪜就可以调用openai的api了。

**修改方法：

打开Agn-AI文件根目录的srv/ad**apter/下的openai.js和openai.ts

**均将这两个文件里的baseUrl变量中的 'htt**ps://api.openai.com'

```
https://api.openai.com
```

修改成 'https://r.cydrvp.top/proxy/api.openai.com'

```
https://r.cydrvp.top/proxy/api.openai.com
```

两个文件修改后记得保存。

![](img/795c34ffd09ffb1fc9ae7cc39e69f5de.png)

# **第三步：运行Agn-AI **

在cmd终端输入(在agn-ai文件夹路径下)

```
node srv/bin.js
```

当出现类似于这种情况时，说明Agn-AI服务已成功运行。

在浏览器输入

```
http://127.0.0.1:3001/
```

便可进入Agn-AI界面。

**往后如果要启动Agn-AI，在agn-ai文**件夹路径下的cmd终端输入刚刚的指令即可

```
node srv/bin.js
```

![](img/46c7ffc6d82a28775c3517dc7ed07d48.png)

# **第四步：对接API**

**我演示两个常用的api对接，一个是openai（GPT3.5和GPT4），一个是text-generation-webui（即部署本地**模型）。至于如何快速使用Text-generation-webui，我的最新视频就是介绍如何在autodl上部署，我目前已经在autodl发布了一键部署的镜像。

[**https://www.bilibili.co**](https://www.bilibili.com/video/BV1CM4y1h73u)m/video/BV1CM4y1h73u

# **①对接****openai****篇 ****(closeai)**

打开左侧的settings选项

来到 **AI Settings **选项， Default AI Service 处选择OpenAI

下方选择OpenAI的具体设置

Default Preset可以选择Turbo

然后下方填入你的API KEY

**老**惯例，我分享一个API，但我希望各位把这个API当作测试反代服务是否配置正确，而不是直接拿这个高强度使用，一天干完5美元。

```
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
```

最后点击右下角的Update Settings保存设置。

# ② 对接text-generation-webui篇

我这里使用我在autodl上的发布的text-generation-webui镜像来做示范。

启动模型后

打开autodl的自定义服务，复制链接。

来到左侧的Settings

Default AI Service选择Kobold

下方KoboldAI设置里面，粘贴你复制的链接

最后点击右下角的Update Settings保存设置。

![](img/26b204bee35af85f4606334b129c4c50.png)

**第五步：测试API对接情况**

来到左侧的Chats，点击右上角的New

因为是测试，所以直接点击下方的create按钮开始对话。

输入Hello!

AI进行回复，代表API对接成功。

若无法回复，如果你是对接Openai，请检查你的api key是否无法使用或反代理服务器没有设置好；如果你是对接text-generation-webui，请检查webui是否可以正常使用，且启动参数是否添加了

```
--no-stream --api
```

![](img/e1f8ffd71033a297204a74e9db875baa.png)

# **第六步：自定义角色**

这一步靠大家自己了，你可以在

这个网站：https://zoltanai.github.io/character-editor/

选择右边的new character

开始你的自定义人设（请使用英文）

最后上传角色的头像

下方按钮导出

选择左侧下载角色卡片

请注意，这张png图片包含了你刚刚输入的所有人设信息。

最后来到Agn-AI左侧的Characters选项

选择导入png图片

最后点击右下的import即可。

当然，你也可以直接在Agn-AI里选择Create按钮来创建角色（在import按钮的右侧）

这里我提供一张catgirl的角色卡，比较简陋，仅供参考。

难蚌，B站会压缩图片，无法正确导入。想要使用这个角色卡的话还是下载，阿里云盘链接：

```
https://www.aliyundrive.com/s/fqCbWkh2sCm
```

保存原图后导入即可。图片是用stable-diffusion-webui的AOM3模型生成的。

最后在Chat中根据你自定义的角色，进行聊天。