
# Suna本地部署指南

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/MW1DdRe6Ko656Xx4GlAci7uZnFf](https://kq4b3vgg5b.feishu.cn/docx/MW1DdRe6Ko656Xx4GlAci7uZnFf)

## 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》(5月班)完整版付费课程

![](img/6031ba292244d718a9ed34f538a6a049.png)

![](img/41fb3365d964f1eeb0010a8909c9527b.png)

此外，公开课全套学习资料，已上传至网盘（链接: https://pan.baidu.com/s/19hS3AhpFm88xShoAbWaTtQ 提取码: 2wui ），需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

## 阶段一：准备工作

部署的第一阶段是准备工作，我们需要完成以下这些工作。

+ 下载Suna源码并解压缩
+ 安装`Anaconda`、`node.js`和`docker`
+ 一键下载依赖。

首先，我们需要将课件网盘中的Suna项目源码包下载并解压缩，

![](img/3d326c7dc885837baec5b5676a86e904.png)

然后在课件的软件包中找到`Anaconda`、`node.js`和`docker`三个软件安装包，下载并进行安装，这三个软件安装没有任何难度，一路点击下一步即可，此外，部署阶段建议全程开启科学网络工具，确保一些`Saas`服务能顺利运行。

紧接着，让我们进入Suna项目主目录内，点击进入后端（`backend`）文件夹，

![](img/3cdbdc644e7785dd8ac72645df656345.png)

然后右键点击空白处，选择在当前文件夹内开启终端，

![](img/dc304eb8ac92e9b00e00edb2b1ec5817.png)

然后使用`conda create`命令，创建一个Suna后端运行的虚拟环境，

```
# 1. 创建虚拟环境
conda create -n suna python=3.11

# 2. 初始化 conda（只需一次）
conda init

# ⚠️ 3. 重启命令行

# 4. 激活环境（PowerShell / cmd / Git Bash 都支持）
conda activate suna
```

安装完成后重启命令行，输入`conda activate`命令进入虚拟环境，

```
conda activate suna
```

然后使用`pip install`，安装Suna项目依赖

```
pip install -r requirements.txt
```

安装完成后，我们进入项目前端文件夹（`frontend`），

![](img/dd461369b039a081363fd6d20ec76c08.png)

同样在空白处点击右键，打开当前目录的命令行，然后使用`npm install`安装前端依赖，

```
# 可以设置代理环境加速安装
# set HTTP_PROXY=http://127.0.0.1:10080
# set HTTPS_PROXY=http://127.0.0.1:10080
npm install
```

等待完成后，我们第一阶段准备工作就全部完成了。

## 阶段二：配置Suna项目后端

然后进入第二阶段，让我们来完成Suna的后端配置。Suna的后端配置总共需要完成以下四步，主要是进行此前介绍的部分核心组件的配置，

+ 配置`tavily API-KEY`：开启网络搜索功能
+ 配置`firecrawl API-KEY`：开启网络爬虫功能
+ 配置`Daytona`：开启沙盒环境功能
+ 配置`supabase`：开启完整后端支持

第一步需要获取搜索引擎`tavily`的API KEY，我们需要登录`tavily`官网（https://www.tavily.com/），完成注册并获取API-KEY，

![](img/ef86c60a3e87aeb933b8656a144f66c5.png)

![](img/eca4a1bef3602bd29f8f5c1a6ce6bdd4.png)

然后进入Suna项目的后端（`backend`）文件夹，用文本编辑器打开`.env`文件，该文件是后端配置文件，

![](img/a0138fe912332394a877be883fdd13d8.png)

然后将刚刚复制的API-KEY写入`TAVILY_API_KEY`中，记得要随时保存。

![](img/a8fd48a93b6bb2f9a20e3f2e8aeebb01.png)

接下来继续获取网络爬虫`firecrawl`的API KEY，同样需要登录`firecrawl`官网（https://firecrawl.dev），完成注册后在`dashboard`页面复制API-KEY，

![](img/dbc5c43911e29dabe9dfd218e106d0d7.png)

![](img/be6fa9e3a2153aa2e3cd62707bcf78a8.png)

然后同样是在`.env`文件中写入`FIRECRAWL_API_KEY`。

![](img/a05ec025d88fc04f9e8c596e6b71c1dc.png)

接下来继续设置沙盒环境工具`Daytona`，`Daytona`的设置稍微比较复杂，我们需要先进入`Daytona`官网（https://www.daytona.io/）并进行注册，

![](img/ca628bfa939e292c575de8c04c0d8355.png)

然后`Daytona`需要搭配一个Suna镜像才能顺利运行，因此我们需要点击左侧的Image（镜像）选项，然后点击右上角的Create Image（创建镜像），然后输入既定的`Image Name`和`Entrypoint`，然后点击创建。这段文本较为复杂，大家可以领取文字版课件后直接复制。

![](img/aab28d10707eee947387a6fcea453df4.png)

+ `Image name`: `kortix/suna:0.1.2`
+ `Entrypoint`: `/usr/bin/supervisord -n -c /etc/supervisor/conf.d/supervisord.conf`

然后等待镜像导入完成即可。

![](img/877dc5412526d3cadd0a22822733659f.png)

接下来点击左侧`Keys`选项并创建`API-Key`，

![](img/a0b6c69ae28c7f045cddfc0184cb67b1.png)

最后，将创建好的`Daytona API-KEY`写入`.env`配置文件。到这里，第三项配置就完成了。

![](img/9461cefaf7906b6756bc5e848342be82.png)

最后一项，是配置后端服务工具`supabase`。还是一样，需要登录`supabase`官网，并根据引导完成注册和项目创建，

![](img/d0df597f7356a130f7d9473770f34fee.png)

例如这里我创建了一个名为`suna-test`的项目，

![](img/5df1833ff1f55e931292115b74ae055e.png)

然后在项目主页左侧选择`Project setting`，然后点击`Data API`，往下翻找到`schemas`选项，确保选择了如图所示的三种格式。

![](img/1ad7108563b8e05b5ec4273bb2ea47be.png)

然后在当前页面往上翻，找到如图所示三项核心信息，并分别复制填入`.env`文件中箭头所示这三个变量里。

![](img/13583160991845d98986b57824d88d08.png)

然后保存`.env`文件并退出。

接下来回到后端文件夹的命令行中，输入如下三项命令，第一条命令是登陆`supabase`，输入后会自动弹出确认登陆的网页，

![](img/8c71ad8294705c98aa78dbee16925447.png)

![](img/2631765c97f53b4b90fdc877e049c491.png)

输入账号密码即可。第二条`link`命令是在本地设置默认项目，需要关联到`supabase`对应的项目ID，

![](img/8d8bdc1852d4ccb828b95a04304b6e60.png)

而第三条`push`命令则是将本地数据表格式同步到关联的项目中。

![](img/5aad0d6c70ce9f1fa203cda72b226b06.png)

```
npx supabase login
npx supabase link --project-ref &lt;your-project-ref&gt;
npx supabase db push
```

全部执行完后，Suna后端配置全部完成。

## 阶段三：配置Suna底层大模型

最后第三个阶段，让我们设置Suna的基础模型配置。这里强烈推荐大家分别为Suna的前端和后端配置不同的模型，能够大幅加快Suna的响应速度。

+ 配置Suna后端大模型API：`Claude 3.7`模型
+ 配置Suna前端大模型API：`DeepSeek`模型

首先在后端配置中，推荐使用`Claude 3.7`模型，这是目前Agent能力最强的模型，同时也是Suna的默认模型。我们可以直接在某宝上购买`Claude`官方API-KEY，也可以自行注册，然后在后端的配置文件`.env`文件中写入`Claude API-KEY`即可。

![](img/6c9e8a66e0b88c5a287defd35a45f488.png)

此外，也可以输入`OpenRouter`的API-KEY，来调用包括`DeepSeek`模型在内的各项主流模型。

紧接着，我们用文本编辑器打开前端配置文件`.env.local`，

![](img/4ac56dfbcad8a860120905f568e7b116.png)

然后如图所示，把部分后端配置复制写入前端配置文件中，并在最后一行`OPENAI_API_KEY`一栏写入`DeepSeek`官方的API-KEY，这就是全部的前端配置了。

![](img/4f93277c92b6fe7355298d9c2d354f81.png)

然后记得保存并退出。

至此，准备工作全部完成，接下来即可按照如下流程启动Suna了！

+ 第一步：借助`docker`启动`Redis`
+ 第二步：启动Suna后端
+ 第三步：启动Suna前端

激动人心的时刻来了，首先需要借助`docker`启动`Redis`。我们需要确保之前安装的`docker`已经启动，然后在后端文件夹中打开命令行，输入`Redis`启动命令。

```
conda activate suna
docker compose up redis
```

然后同样在后端文件夹中再打开一个命令行，输入如下命令开启Suna的后端服务。

```
conda activate suna
python api.py
```

最后，在前端文件夹中打开命令行，输入如下命令开启Suna前端。

```
npm run dev
```

前端启动后，我们就能本地浏览器输入`localhost:3000`，即可使用Suna了！使用前会要求先注册，使用任意邮箱注册即可，

![](img/25d9fb5106d2ee51cf8e30100c01d891.png)

![](img/10d7bfd3c1d1eb117ef8bdebea527533.png)

然后即可登录到对话页面，开始进行对话了！`suna`支持普通对话聊天，也可以执行各类复杂任务，大家现在看到的就是一个完整的复杂任务执行任务流程，整个过程`Suan`会先进行任务规划，然后一步步执行，执行过程中能够调用命令行、操作浏览器、编写`Python`代码、并且还能在沙盒环境中创建和编写相关文件等等等等。
