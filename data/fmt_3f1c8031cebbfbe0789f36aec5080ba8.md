
# 我怎么使用AI（四）

> 来源：[https://agijuejin.feishu.cn/docx/Xt7ddGJKLoGKRuxcl8rcnJS1nbf](https://agijuejin.feishu.cn/docx/Xt7ddGJKLoGKRuxcl8rcnJS1nbf)

## 👀快看👀

> **作者：**[**wharton0**](https://space.bilibili.com/35891473)

> **原文链接：**[**我怎么使用AI（四）**](https://www.bilibili.com/video/BV1KcWke2ENs/?spm_id_from=333.999.0.0&vd_source=5a54577a6bc7d876f6e5df88640c8e9f)

---

## ⚡️摘要⚡️

> 如何在 Docker 中使用 Python，包括创建一个包含 Python 3.11 的 Ubuntu 环境，以及如何安装其他 Python 包，并介绍了如何通过 Python 编程方式来管理 Docker 容器。

---

## 🤖分集🤖

### 我怎么使用AI（四）

介绍创建安装 Python 3.11 的 Ubuntu 环境过程，包括编写 `Dockerfile`、构建镜像、安装包等，还提到优化镜像大小及使用 `docker-py` 管理容器，强调使用 `Docker SDK` 的重要性及自动化管理注意事项。

![](img/dc3b05838aa82bdb5273b96027cf62bd.png)

[我怎么使用AI（四）.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PtwNbEDxLoVtLZx0ZbVcYntQnif)

---

## ✏️课代表✏️

+ 创建 Docker 环境：强调了使用 `Dockerfile` 来定义环境，并通过 `docker build` 和 `docker run` 命令来构建和启动容器。
+ Python 包安装：提到了在 Docker 环境中安装 Python 包的重要性，以及如何确保环境中包含了所有必要的依赖。
+ 镜像优化：指出了在构建 Docker 镜像时需要考虑的优化措施，如清理不必要的层以减小镜像大小。
+ 编程式容器管理：介绍了如何使用 Python 的 `Docker SDK` 来编程式地管理 Docker 容器，提高了开发和部署的效率。
+ 文件传输与错误处理：强调了在使用 `Docker SDK` 时需要注意的文件传输限制，以及实施适当的错误处理机制以防止资源泄漏。
