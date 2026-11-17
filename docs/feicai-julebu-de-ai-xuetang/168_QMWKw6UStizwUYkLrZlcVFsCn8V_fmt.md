
# 如何安装调用Grok-2 Beta 模型 API

> 来源：`https://feicaiclub.feishu.cn/docx/Nwepdp9zgo7DV7xwGUEc8XWlnOc`

![](img/345cbb6ffd8ab07c8c6386d5168365ea.png)

## 视频链接

> B 站：`https://b23.tv/AlP0gZ8`
> 📕：`http://xhslink.com/a/KSdWaVGdj27Z`
> 抖音：`https://v.douyin.com/CeivEpGv/`

## 安装 Ollama、Docker 和 OpenWebUI 并配置 Grok-2 Beta 模型 API

### 1. 安装 Ollama

1. 访问 Ollama 官网

+ 打开浏览器，在地址栏输入 `https://ollama.com/`。在官网页面，根据你的操作系统（Windows 或 macOS），点击对应版本的下载按钮。

1. 下载并安装 Ollama

+ 下载完成后，导航到你的下载文件夹，找到安装文件。

+ 双击安装文件，按照提示完成安装。

+ 注意：安装路径通常为默认用户目录下的 `.ollama` 文件夹。如果需要更改模型存储路径，可以设置环境变量 `OLLAMA_MODELS` 来指定存储位置。

1. 验证安装

+ 打开终端（macOS 用户：Command + 空格 -> Terminal，Windows 用户：Win + R -> `cmd`），输入以下命令验证安装：

```
ollama --version
```

+ 如果显示版本号，说明安装成功。

---

### 2. 安装 Docker

1. 访问 Docker 官网

+ 打开浏览器，在地址栏输入 `https://www.docker.com/`。

1. 选择合适的版本

+ 根据你的操作系统，选择对应的 Docker 客户端版本下载。

1. 下载并安装 Docker

+ 双击下载的安装文件，按照安装向导完成安装。

+ 安装完成后，启动 Docker。如果是第一次使用，需注册一个 Docker 账号。

1. 验证安装

+ 打开终端，输入以下命令确认 Docker 是否正常运行：

```
docker --version
```

+ 如果显示版本号，说明 Docker 安装成功。

---

### 3. 安装 OpenWebUI

1. 访问 OpenWebUI 官网

+ 打开浏览器，访问 `https://openwebui.com/`。

1. 获取安装命令

+ 在 GitHub 页面中向下滚动，找到“Installation with Default Configuration”部分。

+ 根据页面指引，复制适合你的系统的安装命令。

+ 如果你已经安装了Ollama，可以使用第一个安装命令。

![](img/a415c0641ffa47f4f355e116eb304bcc.png)

1. 执行安装命令

+ 打开终端（macOS 用户：Command + 空格 -> Terminal，Windows 用户：Win + R -> `cmd`）。

+ 将复制的命令粘贴到终端中，按回车开始安装

```
docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

+ 注意：安装时间可能需要 10-15 分钟，具体取决于你的网络状况。

1. 打开 OpenWebUI 页面

+ 安装完成后，在浏览器中输入以下 URL 打开 OpenWebUI 界面（默认端口为 3000）：

```
http://localhost:3000
```

+ 按页面提示完成注册并登录。

---

### 4. 配置 Grok-2 Beta 模型 API

#### 生成 Grok API 密钥

1. 访问 Grok 官方网站

+ 打开浏览器，输入 `https://x.ai/api`。

![](img/cebdc717ac3ac32c275d23cb3f40d00a.png)

1. 注册账号

+ 点击“立即构建”按钮，按照提示注册一个账号。

+ 如果你已有 X（Twitter）账号或 Google 账号，可以直接登录。

1. 生成 API 密钥

+ 进入后台后，点击“Generate API Key”。

+ 系统会自动生成一个 API 密钥，复制该密钥保存备用。

#### 在 OpenWebUI 中配置 API 密钥

1. 进入管理页面

+ 在 OpenWebUI 界面中，点击右上角头像。

+ 在左侧导航栏选择 Admin Settings。

1. 配置 API 信息

+ 在 Connections 部分，找到 API Key 输入框。

+ 将刚刚复制的 API 密钥粘贴进去。

1. 设置域名

+ 删除默认域名，替换为正确的 Grok API 域名：

```
https://api.x.ai/v1
```

+ 确保域名输入正确，否则可能导致 API 请求失败。

1. 关闭其他 API 设置

+ 将 Ollama API 的选项关闭，保存设置。

---

### 5. 验证安装与配置

1. 查看模型列表

+ 返回 OpenWebUI 主界面，新建一个聊天窗口。

+ 点击左上角的大模型列表下拉菜单。

![](img/1dfe342c101116cb5b3cfa0a01378c97.png)

1. 验证 Grok 模型

+ 如果安装和配置成功，列表中会显示 Grok Beta 和 Grok Vision Beta 两个模型。

+ 选择模型后即可开始交互。

---

## 注意事项与提示

1. 网络连接

+ 安装 Docker 和 OpenWebUI 需要下载大量文件，确保网络连接稳定。

1. API 使用限制

+ 每个 Grok 账号默认拥有 25 美元的免费额度，注意控制使用量，避免超额。

1. 安全性

+ 确保 API 密钥保密，避免泄露。

1. 版本更新

+ 定期检查 Docker 和 OpenWebUI 的版本更新，以获取最新功能和安全修复。

---
