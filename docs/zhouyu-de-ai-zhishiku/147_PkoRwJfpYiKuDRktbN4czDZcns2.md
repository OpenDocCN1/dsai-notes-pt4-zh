
# 安装 Gemini CLI

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/Sgd2dfTNcowyfcxrCAeclLFan8c](https://xatc0v8uz5m.feishu.cn/docx/Sgd2dfTNcowyfcxrCAeclLFan8c)

## 通过 GDP Premium 激活 Gemini Code Assist Standard 许可

恭喜您订阅了 Google 开发者计划高级方案！

如需开始使用 Gemini Code Assist，您需要先将许可与 Google Cloud 项目相关联，以激活许可。请按照以下适用于您偏好的环境的说明操作。

> 注意：Google 开发者高级订阅中的 GCA 标准版允许一位用户使用，但不支持席位分配。

### 方法 1：通过 IDE 激活

这是最直接的入门方式。

1. 在您首选的 IDE（例如 VS Code、Android Studio）。如果您尚未安装此插件，可以从 `codeassist.google` 下载最新版本。

2. 在 IDE 中使用 Gemini Code Assist 界面登录。

3. 请务必使用与您的 Google 开发者计划高级订阅相关联的同一 Google 账号。

4. 当插件提示时，选择或创建 Google Cloud 项目。

选择项目后，Gemini Code Assist 将处于活动状态，可在 IDE 中使用。

> 注意：您需要一个 Google Cloud 项目来管理该服务，但无需启用结算功能。您的 Google 开发者计划高级方案订阅涵盖了相关费用。

---

### 方法 2：通过网页或 CLI 激活

如果您主要使用基于网络的工具（例如 Firebase Studio 或 Gemini CLI 等命令行工具）来使用 Gemini Code Assist，请使用此方法。

1. 选择或创建 Google Cloud 项目。前往 Google Cloud 控制台，选择现有项目或创建新项目。

2. 启用该 API。在所选项目的控制台中，前往 API 库并启用 Gemini for Google Cloud API。

3. 登录该工具。打开您的服务（例如，Firebase Studio）或终端，然后使用拥有 Premium 订阅的同一 Google 账号登录。

Gemini Code Assist 现在将在所有已关联的 Google Cloud 服务中启用。

### 了解详情

如需了解所有功能和特性，请参阅官方文档：

+ Gemini Code Assist 文档

+ [Gemini CLI 文档](https://cloud.google.com/gemini/docs/codeassist/gemini-cli?hl=zh-cn)

+ Gemini Code Assist 编写代码文档。

# gcloud CLI 概览官方文档

`https://cloud.google.com/sdk/gcloud?hl=zh-cn`

你的 Mac 未安装 Homebrew 和 `gcloud`。我要为你自动安装并初始化 Gemini Code Assist 所需环境。将执行这些步骤：

1. 安装 Homebrew

2. 通过 Homebrew 安装 Google Cloud CLI（`gcloud`）

3. 配置 `PATH`（写入 `~/.zprofile`，便于以后终端可用）

4. 验证 `gcloud`，启动 `gcloud init`（浏览器登录并选择/创建项目）

5. 启用 API：`aiplatform.googleapis.com` 与 `gemini.googleapis.com`

说明：

+ 第 4 步会在终端中打开浏览器登录，你需要完成账户授权与项目选择。

+ 若项目未启用结算，启用 API 可能失败，稍后我会提示你在控制台开启结算。
