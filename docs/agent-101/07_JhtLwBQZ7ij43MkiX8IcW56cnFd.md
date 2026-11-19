
# 通过coze构建一键生成抖音、📕视频RPA自动化工具

> 来源：[https://docs.feishu.cn/docx/PRwZdUfC1oMDV2xtrdRcbatmnKe](https://docs.feishu.cn/docx/PRwZdUfC1oMDV2xtrdRcbatmnKe)

该教程详细介绍了如何通过coze工作流构建一键生成图文类视频。

## 效果演示

![](img/706628607c20337c9139f8e9c166486c.png)

[📕-工作流.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CO8fbcl6WoSiFMxjQBRckcwfnVd)

https://telegraph-image-djt.pages.dev/file/afe453234bf035d1bb77f.mp4

## 工作流

该工作流主要使用了以下节点：

+ 大模型节点
+ 该节点主要是通过大模型节点输出文本内容并以JSON形式输出
+ 函数节点
+ 函数节点主要是对大模型的输出处理
+ 插件
+ 该节点的核心是调用构建的视频生成API，对大模型输出的文本内容转换成视频服务

![](img/28e17bbd7d718ddeddad1611f119e5c3.png)

## 详细步骤

### 前置条件

视频生成服务是基于remotion构建的，以下是remotion的相关资料和信息：

+ Remotion：是一个开源框架，允许开发者使用 React 组件来创建高质量的视频。它将 React 的声明式编程模型应用于视频制作，使得程序员可以用编写网页的方式来制作复杂的动画和视频内容。

此外运行remotion必须有`Node 16`及以上的环境才可以。

![](img/59153a83511f1dd1f25300bf8a64c19a.png)

+ 文档：

### 步骤一：构建remotion项目

1. 根据上文中分享的remotion相关文档，使用命令行：

```
npx create-video@latest
```

安装完成以后你本地文件会有以下类似的目录结构，每个文件以及作用如下：

![](img/1ee3025d2f2fa607fc6a8e273ddb5331.png)

然后在该目录下使用命令行 `npm install` 以及 `npm start` 完成安装和项目启动；当然如果你本地还没有搭建具体环境只是想快速了解下remotion的魅力支持，你可以通过官方提供的沙盒环境快速运行：https://remotion-helloworld.vercel.app/?/HelloWorld

![](img/b21942d61a4ae0f92ae85417e88e2b74.png)

### 步骤二：构建图文视频模板

完成步骤一以后，我们就可以进行构建我们想要的视频模板了，构建视频模板之前首先需要构建模板组件，此处我将展示并分享我构建的视频模板的代码：

1. 我们所构建的组件代码必须都在`src`目录下，你可以根据你要做的模板起名并创建目录文件，下图为我创建的文件目录：

![](img/efeee44965d973d584c7a8bc6d4907b0.png)

其中：`AudioFade.jsx` 定义了背景音乐组件，`Subtitles.jsx` 定义了字幕组件，`MyVideo.jsx` 引用了 Audio 以及 Subtitles组件构成了主视频组件，效果如下图所示：

![](img/cc61c9d3c16ddac3e61af810f2dee99e.png)

以上实现代码我将上传至github，地址在这里：https://github.com/DangJin/remotion2Agent；以上代码均为gpt生成，我仅作了复制粘贴！不信你看：

![](img/04b497a315e652ce9225fea4afc34299.png)

[CleanShot 2024-07-30 at 00.04.32.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MM2dbz2nwoW0CNxSadocOMuQnJM)

### 步骤三：构建API服务

完成视频模板创建并运行成功以后，我们将通过`express`构建API，具体代码逻辑如下：

![](img/accc18a44afcd7b32a5a057e94f64be9.png)

### 步骤四：部署

此次部署采用了阿里云轻量应用服务器完成部署，你只需要安装好`Node`相关环境，代码上传至服务器并启动项目即可，此处不做赘述，如若代码部署问题，可以留言给我！

### 步骤五：构建text2video插件

在编排工作流之前，我们需要通过coze的插件能力，如何创建插件此处不再赘述，可以查看我之前的教程；

第1步：定义元数据：

![](img/0fbda09d2ce9bb404691b3ce6ce27184.png)

第2步：删除插件默认生成的代码保留如下图所示

![](img/6d0bf3d0780203a6d4980b22dbbe839f.png)

第3步：使用快捷键`Command+I` 唤起 AI ，输入以下prompt让AI帮你生成代码：

prompt：写一个post请求：请求地址：xxx.com，body参数为{"text":"} ,待AI生成代码以后，点击接受即可，具体代码如下：

![](img/584f352e8fc64ccaa27ef5230904a283.png)

### 步骤六：编排工作流

![](img/1ed5bed5cd50b3cc33d05ef0340c0bdf.png)

### 步骤七：最终效果展示

![](img/2b4c1de3549d4db929d4b2a334d2d5e9.png)

[📕-工作流.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Zh6sbXvDwoftaIxLYDccBzKvnnf)

## 其他

+ 项目代码：https://github.com/DangJin/remotion2Agent
+ Remotion 模板：https://remotion-helloworld.vercel.app/?/HelloWorld
+ Remotion Docs：https://www.remotion.dev/docs/
+ 扣子如何创建插件：https://www.coze.cn/docs/guides/create_plugin

## 加入Agent101 获取更多 Agent 开发知识：

![](img/b551cc82ece13ad0a534381de0947ef7.png)
