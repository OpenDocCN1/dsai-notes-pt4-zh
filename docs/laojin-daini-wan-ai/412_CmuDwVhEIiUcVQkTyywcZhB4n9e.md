# Dify本地部署

> 来源：[https://my.feishu.cn/docx/T2oldBGWAoxKXmxg4MAcxru7nHd](https://my.feishu.cn/docx/T2oldBGWAoxKXmxg4MAcxru7nHd)

# 一、Dify下载和安装

首先需要安装2个工具：

Git，参考[Git 安装](https://tffyvtlai4.feishu.cn/wiki/MF1ywmPKhig7p3kpIqNcFsmPnPe)

Docker，参考[Docker安装](https://tffyvtlai4.feishu.cn/wiki/C4pzw37L9ibcf2kx99OcY3nznJc)

打开任意想安装的路径，Shift+右键，打开PowerShell，粘贴以下代码：

```
git clone http://github.com/langgenius/dify.git
```

![](img/98ad6134cec12243a0108cbc295cbca2.png)

安装项目路径下，搜索docker-compose.yaml，右键打开所在文件夹

![](img/579a061034a5db820ba95e4aee86b52f.png)

打开文件夹后，在地址栏中输入powershell，回车。

![](img/53b95daa287f8d36ac9b170c9df83ab3.png)

启动 Docker 容器

根据你系统上的 Docker Compose 版本[，选择合适的命令来启动](https://docs.docker.com/compose/#compose-v2-and-the-new-docker-compose-command)容器。你可以通过 $ dock[Docker 安装 ](https://tffyvtlai4.feishu.cn/wiki/C4pzw37L9ibcf2kx99OcY3nznJc)r compose version 命令检查版本，详细说明请参考 Docker 官方文档，若没安装Docker，则参考 进行安装：

# 2024/9/10新增：

升级0.8版本后会有API和WOKER报错，无法启动

.env.example 复制并修改成.env 文件。

![](img/a3876a1e99077b219d046f76a31e2291.png)

找到REDIS_SENTINEL_SOCKET_TIMEOUT和CELERY_SENTINEL_SOCKET_TIMEOUT修改为10即可。

![](img/e609598a4c4c765ed0cb76f61d849ef8.png)

如果版本是 Docker Compose V2，使用以下命令：

```
docker compose up -d
```

![](img/9d126f48c1ed23e5276728771c16af5f.png)

要下载很多，直到出现PS 地址

![](img/8af18cf9152f3b50eb66b3bc8881ebe1.png)

此时点击桌面右下角的小船一样的图标，打开docker

![](img/42b1d1d6cebb30f7b929f989d4dd7b97.png)

![](img/07a3144ed72acd472378014656a41f3f.png)

# 二、Dify配置

打开浏览器，本地访问：127.0.0.1

第一次打开要注册，注册后直接登录。

![](img/612fc112c6c23618a65c6e653fcf9337.png)

依次点击，配置Ollama模型。

![](img/eb6cdaf5bb5f1cc00f2d6fc5cedc1f5a.png)

![](img/1f6cd89ee29d336cb7feb9e4b75c6e38.png)

模型名称：qwen2:7b

URL填写刚刚一样的：http://host.docker.internal:11434

![](img/928f1b357d09a07a0da90ebd4113d0c4.png)

Dify界面有4个主要功能区：

1、探索区：别人做好的agent，可以添加后修改套用；

2、工作室：自己设置的agent

3、知识库：可以上传文档格式包括：txt，markdown，pdf，html，elsx，xls，docx，csv，每个不超过15m

4、工具区：一些外部工具

![](img/ee1e8b4be8ddc80cc93ab620ef374c09.png)

# 三、更新Dify

进入 dify 源代码的 docker 目录，按顺序执行以下命令：

```
cd dify/docker
docker compose down
git pull origin main
docker compose pull
docker compose up -d
```

**同步环境变量配置 (重要！)**

+   如果 .env.example 文件有更新，请务必同步修改您本地的 .env 文件。

+   检查 .env 文件中的所有配置项，确保它们与您的实际运行环境相匹配。您可能需要将 .env.example 中的新变量添加到 .env 文件中，并更新已更改的任何值。

**访问 Dify**

在浏览器中输入 http://localhost 访问 Dify。

**自定义配置**

编辑 .env 文件中的环境变量值。然后，重新启动 Dify：

复制

```
docker compose downdocker compose up -d
```

完整的环境变量集合可以在 docker/.env.example 中找到。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>