
# Deepseek+ollama本地部署种草

> 来源：[https://vcn0zlxse9ua.feishu.cn/docx/Kev2dMIHuo7ZvUxpUH4cmkLcn4f](https://vcn0zlxse9ua.feishu.cn/docx/Kev2dMIHuo7ZvUxpUH4cmkLcn4f)

本文档主要是指引 Deepseek 本地部署教程，可以平移到其他的模型也适用。

如：清华的 ChatGLM、阿里的千问以及 Meta 的 llama 等。当前主要演示 window 的版本，其他版本类似。

## 前言

目前本地部署的 Deepseek R1 的 1.5B 等小参数模型基本是将推理能力提炼到 Qwen 或 Llama 的蒸馏版本，性能是远远比不上官网的版本的，你可以根据你自身的情况判断是否需要本地部署。

## 什么是本地部署？

DeepSeek 本地部署是指将 DeepSeek 人工智能模型直接安装到用户的本地设备（如电脑或服务器）上运行，非依赖云端服务。

## 为什么要做本地部署？

+ 确保数据隐私和安全

+ 减少网络延迟（效果对设备有要求）

+ 无网络环境下的离线运行 AI 功能

+ 定制化集成（企业内部灵活对接）

+ 自有模型训练

## 安装 ollama

更多功能可查看官方文档：`https://ollama.readthedocs.io/quickstart/`

### 下载 ollama

#### 官网下载

地址：`https://ollama.com/`

选择对应的版本下载

![](img/4831e9e7de37a0d00f6b343e2b07d4e1.png)

![](img/c3ec76d0e13da9eafe072480482ac966.png)

#### 加速下载（如下载比较慢的情况）

+ 用 github 代理加速（`https://ghfast.top/`）

+ 把 `ollama` 在 github 的下载链接 `https://github.com/ollama/ollama/releases/download/v0.5.7/OllamaSetup.exe` 输进去

![](img/89f49dc5eb156ffd835bb88e9060f409.png)

#### 安装：一路 “下一步” 就搞定安装

### 设置模型位置（可选）

#### 打开环境变量

搜索【环境】点击 编辑系统环境变量

![](img/86d095d0ffa651b5c7fbabcbfc3c07be.png)

![](img/25f2c056272fb574c7922e87b172f731.png)

#### 新建 ollama 环境变量

`OLLAMA_HOST` 配置访问的 ip，方便其他电脑访问

`OLLAMA_MODELS` 配置模型的下载位置，默认 C 盘，容易爆盘

`OLLAMA_ORIGINS` 设置跨域

```
OLLAMA_HOST 0.0.0.0
OLLAMA_MODELS E:\job\OLLAMA_MODELS
OLLAMA_ORIGINS *
```

点击【新建】按钮

![](img/94486967ff7653e91ff7f2900d100433.png)

### 终端使用验证

安装成功后，win 搜索 `cmd`，打开终端应用

![](img/45d96d2e98db6b0d6c28d686c0b46939.png)

#### 验证是否安装成功

复制到 `cmd` 中执行

```
ollama help
```

如下

![](img/d5c493f6fb3896d657a5681f65fb56ea.png)

### 启动

#### 启动 ollama

复制到 `cmd` 中执行

```
ollama serve
```

#### 是否启动成功

浏览器访问：`http://127.0.0.1:11434/`

出现如下标志表示运行成功，否则重新启动

![](img/7a6fefa9fa5889374e6f4afa168e7cb5.png)

## Deepseek 显卡硬件要求（实操可跳过）

一般需要 GPU，显卡的算力能力

### 精准计算

+ 确定模型参数规模

例如：DeepSeek R1-7B 对应 7B 参数（十亿：7×10^9）。

+ 选择计算精度

+ FP16：每个参数占 2 字节

+ 4-bit 量化：每个参数占 0.5 字节（4/8=0.5）

+ 8-bit 量化：每个参数占 1 字节

+ 场景系数选择

+ 推理：仅需参数和激活值（约 1.2 倍参数体积）

示例：7B 模型 FP16 推理显存 ≈ 7×2×1.2 = 16.8GB

+ 额外开销（KV 缓存）

每 token 的 KV 缓存显存 ≈ 2×层数×隐藏维度×序列长度×批次大小。例如：

+ 7B 模型（32 层，4096 隐藏维度）在 1024 序列长度下，KV 缓存 ≈ 2×32×4096×1024×1 ≈ 256MB。

+ 量化优化

4-bit 量化可减少 75%参数体积，但需额外 10%-20%显存用于量化映射表

示例：7B 模型 4-bit 推理显存 ≈ 7×0.5×1.2 + 0.3GB ≈ 4.5GB

### 简单计算

7B 参数（十亿：7×10^9），以字节为单位。

1G=1024*1024*1024 = 1B 参数

需要显存 ≈ 参数*每个参数字节数*1.5（还有其他缓存类）

例如：DeepSeek R1-7B 对应 7B 参数（十亿：7×10^9）。

显存配置 = 7*0.5*（量化的字节数）*1.5 ≈ 5G

### 大概的算力对照表

![](img/a38981866b043527ba5e565dc8cae05c.png)

价格差异问题：200B vs 671B：671B 采用 MoE 架构+显存压缩技术（如 Int4 量化），单卡 RTX 4090 即可部署，而 200B 需 8x A100 集群，更详细请查看更多官方资料

### 说明

+ 量化支持：4-bit 量化可减少约 75%显存占用，但可能损失 5-10%精度

+ 分布式训练：200B 以上参数需多显卡并行，建议使用 NVIDIA NVLink 互联

+ 推理优化：使用 `vLLM` 等推理框架可进一步降低显存占用（如 14B 模型 INT8 推理仅需 9GB）

+ 硬件兼容性：AMD Radeon 显卡支持部分模型（如 RX 7900XTX 可运行 14B 量化版），但生态支持弱于 NVIDIA

建议根据任务复杂度选择模型，例如日常使用选 7B/14B，科研选 70B+，超大规模任务需集群部署。实际部署前建议测试量化版本，平衡性能与资源消耗。

## 选择 Deepseek 模型

打开 Deepseek 模型：

`https://ollama.com/library/deepseek-r1`

### 模型选择

根据电脑的配置，可以选择最小的 1.5b 模型

![](img/38f7392c8861d1c918d449debc4a11c4.png)

![](img/49b780121fe4ad54ec7973b8c6fe9044.png)

### 拉取模型

复制命令到 `cmd` 中执行

```
ollama pull deepseek-r1:1.5b
```

![](img/1254a3e86003db6b385f7f0716a57963.png)

### 运行模型（测试效果）

复制命名到终端运行

如 `ollama run deepseek-r1:1.5b`

```
ollama run deepseek-r1:1.5b
```

![](img/14f59766656a82abb1c6b0aa124a5ef9.png)

可按住：Ctrl+D 或者输入 `[/bye]` 退出

### 查看所有模型

复制到 `cmd` 中执行，可查看刚刚运行的模型

```
ollama list
```

效果：表示已经安装了 Deepseek-r1:8b 模型

![](img/4ef301b74c7036cb58beb31f7d8d1b75.png)

## 搭配界面 UI（穿上马甲）

> 终端操作不是很方便操作，所以我们需要一个界面来承载。

### UI 马甲对比

目前推荐两个，Chatbox 和 Page Assist，可以任意选择一个，比较适合个人实用，更多功能可试试 Cherry Studio 或者 Open WebUI。

几款 UI 对比：

### Chatbox

官方入口：`https://chatboxai.app/zh`

两种安装方式

#### 使用网页版

##### 配置

打开 `https://web.chatboxai.app/`

![](img/082f1041fa37b16ee0a1a587640bfa87.png)

![](img/443e907049bbcbb1e3ab7535f8f8f929.png)

输入地址：`http://127.0.0.1:11434/`

![](img/45f734925949dffb4f74f2f0d720174f.png)

##### 效果

![](img/83c5665c2bf153008661749ec1bcf11b.png)

#### 使用桌面版

打开入口：

`https://chatboxai.app/zh`

下载对应桌面版，配置参考网页版

![](img/ae4784643dc1a43706caa4167ba600f5.png)

### Page Assist

> Page Assist 作为一款开源的 Chrome 扩展程序，堪称本地 AI 模型交互的得力助手。它为用户打造了直观易用的交互界面，让本地 AI 模型的应用更加便捷。

#### 安装扩展

可选择一种方式，在线或者离线安装

##### 在线安装：谷歌商店

我们可以直接从 Chrome Web Store 或 Firefox Add-ons 商店下载安装，也可以通过手动安装的方式进行。

访问 `https://chromewebstore.google.com/search/`，搜索 Page Assist：

![](img/4fab975e7e4608fd2cba96c0ad797042.png)

##### 离线安装

###### 下载方式

+ 从 GitHub 下载：访问 Page Assist 的 GitHub 仓库，进入该页面后点击 “Releases” 标签，在里面找到适合你浏览器版本的插件文件进行下载。比如你使用 Chrome 浏览器，可下载对应的 Chrome 版本插件文件。

+ 从第三方插件网站下载：可以通过 [crxsoso](https://www.crxsoso.com/webstore/detail/jfgfiigpkhlkbnfnbobbkinehhfdhndo) 等第三方插件下载网站进行下载，但要注意选择正规、可靠的网站，避免下载到恶意软件或损坏的文件。

###### GitHub 下载插件安装

通过 GitHub 下载插件

![](img/db27536b62061c8f15b47071fc488cff.png)

然后打开浏览器【扩展程序】->【管理扩展程序】

![](img/6f540c931d71183e7b2c8bf65168ab51.png)

![](img/b00b3d089883e001f16804d2fb89309a.png)

将文件解压后把下载的插件 crx 文件后将文件拖过去安装提示成功就行

![](img/72f3533ec004cc70f55b9ef188560c33.png)

#### 固定窗口

点击插件详情进去

![](img/4b30db46caa39a49d2a4b12e40e11759.png)

![](img/e3b2ec7ec0910964df69fcece946e11f.png)

#### 界面效果，会自动识别 ollama

##### 打开：两个入口

![](img/7acca5262457dc7c59f5ad15df1d5884.png)

##### 效果

![](img/0cdbc60b37b1bb1576a71dbb22bd8b82.png)

![](img/fe7af041297e5593f781862680a8be21.png)

## 其他

### 手机端

Chatbox 可以用网页版或 APP 版

ps：需要云部署把服务放出来才能访问

### 本地知识库

看下一期的内容
