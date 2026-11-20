
# Sonic：一个用 Go 语言开发的高性能博客平台

> 来源：[https://yunyingmenghai.feishu.cn/docx/NSged66NooNFX7xSTZoc6MEanng](https://yunyingmenghai.feishu.cn/docx/NSged66NooNFX7xSTZoc6MEanng)

Sonic 是 Go 开发的博客平台，简单而强大：

![](img/d6f25bb8b2673ea763468ca78550da70.png)

## 简介

Sonic，一个以其速度如声速般快速而命名的博客平台，是一个用 Go 语言开发的高性能博客系统。正如其名字所暗示的，Sonic 旨在提供一个简单而强大的博客解决方案。这个项目受到了 Halo 项目团队的启发，前端部分是基于 Halo 项目的分支。

## 特点

Sonic 具有以下显著特点：

1. **多数据库支持**：支持 SQLite 和 MySQL（PostgreSQL 支持待开发）。
2. **轻量级**：安装文件仅 10MB 大小。
3. **高性能**：文章详情页能够承受 2500 QPS 的访问量（测试环境：Intel Xeon Platinum 8260 4C 8G，SQLite3）。
4. **主题更换支持**：用户可以根据自己的喜好更换博客主题。
5. **跨平台支持**：支持 Linux、Windows、Mac OS，以及 x86、x64、Arm、Arm64、MIPS 架构。
6. **对象存储支持**：支持 MINIO、Google Cloud、AWS、AliYun 等对象存储服务。

## 预览

Sonic 的默认主题 Anatole 提供了一个简洁而现代的界面，以下是其截图预览：

[Anatole 主题截图](https://github.com/go-sonic/default-theme-anatole/raw/master/screenshot.png)

## 安装

安装 Sonic 的步骤如下：

1. **下载最新安装包**：

   ```bash
   wget https://github.com/go-sonic/sonic/releases/latest/download/sonic-linux-amd64.zip -O sonic.zip
   ```

2. **解压**：解压下载的 `sonic.zip` 文件。

3. **启动**：

   ```bash
   cd sonic
   ./sonic -config conf/config.yaml
   ```

4. **初始化**：打开浏览器访问 `http://ip:port/admin#install` 进行初始化设置。初始化完成后，你可以通过浏览器访问 Sonic，管理后台的 URL 为 `http://ip:port/admin`，默认端口为 8080。

## 构建

如果你想要自己构建 Sonic，可以按照以下步骤操作：

1. **拉取项目**：

   ```bash
   git clone --recursive --depth 1 https://github.com/go-sonic/sonic
   ```

2. **运行**：在 Windows 上编译此包，你需要安装 `gcc` 编译器，例如 `TDM-GCC` 工具链。

构建完成后，你的项目就准备好使用了。

## Docker

Sonic 也提供了 [Docker 镜像](https://hub.docker.com/r/gosonic/sonic)，可以在 Docker Hub 上找到：`gosonic/sonic`。

## 主题生态

Sonic 拥有一个活跃的主题生态，以下是一些可用的主题：

+ Anatole
+ Journal
+ Clark
+ Earth
+ PaperMod
+ Tink

## 待办事项

Sonic 的开发团队正在努力实现以下功能：

+ 国际化（`i18n`）
+ PostgreSQL 支持
+ 更好的错误处理
+ 基于 `Wasm` 的插件系统
+ 使用新的 Web 框架（`Hertz`）

## 贡献

Sonic 欢迎任何形式的贡献。你可以提出问题或提交 Pull Requests。Sonic 遵循 Contributor Covenant 行为准则。

### 贡献者

这个项目得以存在，感谢所有贡献者的努力。特别感谢 Evan（evanzhao@88.com），他设计了 Sonic 的 Logo。

## 许可

Sonic 的源代码在 MIT 许可下可用。
