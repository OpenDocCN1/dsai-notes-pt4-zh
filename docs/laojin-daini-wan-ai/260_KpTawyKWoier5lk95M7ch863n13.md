
# Cursor 0.45 之后没法免费续杯了？老金教你怎么搞！

> 来源：[https://my.feishu.cn/docx/Fz0odNYRXoNmAFx6Latc8SqonLg](https://my.feishu.cn/docx/Fz0odNYRXoNmAFx6Latc8SqonLg)

自从过年期间，Cursor 升级 0.45 版本之后，大家都发现了无法继续免费续杯了。

老金试了好久，最后无奈回退到了 0.44 版本，才得以恢复。

作为一个不懂代码，英语没过 4 级的人来讲，这太痛苦了。

因为现在我连文章都是拿 Cursor 写的 =。=

今天！它来了，老金刚刚亲测完成，下面给大家带来使用方法。

需要两个部分：

1. 破解 + 禁止自动更新
2. `fake`（在文章最后）

![](img/706e9616aadc78c3779bee4d1d169b65.png)

> ⚠️ 重要提示

> 本工具当前支持版本：

+ ✅ Cursor v0.44.11 及以下版本  
+ ✅ Windows: 最新的 0.45.x 版本（已支持）  
+ ✅ Mac/Linux: 最新的 0.45.x 版本（已支持，欢迎测试并反馈问题）

> 使用前请确认您的 Cursor 版本。

## 禁用自动更新

> 为防止 Cursor 自动更新到不支持的新版本，您可以选择禁用自动更新功能。

### 方法一：使用内置脚本（推荐）

在运行重置工具时，脚本会询问是否要禁用自动更新：

```
[询问] 是否要禁用 Cursor 自动更新功能？
否 - 保持默认设置 (按回车键)
是 - 禁用自动更新
```

选择 1 即可自动完成禁用操作。

### 方法二：手动禁用

#### Windows

1. 关闭所有 Cursor 进程  
1. 删除目录：`%LOCALAPPDATA%\cursor-updater`  
1. 在相同位置创建同名文件（不带扩展名）

#### macOS

```
# 关闭 Cursor
pkill -f "Cursor"
# 删除更新目录并创建阻止文件
rm -rf ~/Library/Application\ Support/cursor-updater
touch ~/Library/Application\ Support/cursor-updater
```

#### Linux

```
# 关闭 Cursor
pkill -f "Cursor"
# 删除更新目录并创建阻止文件
rm -rf ~/.config/cursor-updater
touch ~/.config/cursor-updater
```

> ⚠️ 注意：禁用自动更新后，需要手动下载并安装新版本。建议在确认新版本可用后再更新。

## 问题描述

> 当您遇到以下任一提示时：

### 问题一：试用账号限制

```
Too many free trial accounts used on this machine.
Please upgrade to pro. We have this limit in place
to prevent abuse. Please let us know if you believe
this is a mistake.
```

### 问题二：API 密钥限制

```
❗[New Issue]
Composer relies on custom models that cannot be billed to an API key.
Please disable API keys and use a Pro or Business subscription.
Request ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### 问题三：试用请求次数限制

> 这表示在 VIP 免费试用期间已达到使用次数限制：

```
You've reached your trial request limit.
```

## 解决方案

### 完全卸载 Cursor 并重新安装（API 密钥问题）

1. 下载 `Geek.exe` 卸载程序[免费]  
1. 完全卸载 Cursor 应用  
1. 重新安装 Cursor 应用  
1. 转到解决方案 1

### 方案一：快速重置（推荐）

1. 关闭 Cursor 应用  
1. 执行重置机器码脚本（见下方安装说明）  
1. 重新打开 Cursor 即可继续使用

### 方案二：账号切换

1. 文件 -> Cursor Settings -> 注销当前账号  
1. 关闭 Cursor  
1. 执行重置机器码脚本  
1. 使用新账号重新登录

### 方案三：网络优化

如果上述方案仍无法解决，可尝试：

+ 切换至低延迟节点（推荐区域：日本、新加坡、美国、香港）  
+ 确保网络稳定性  
+ 清除浏览器缓存后重试

## 一键解决方案

### 国内用户（推荐）

#### macOS

```
curl -fsSL https://aizaozao.com/accelerate.php/https://raw.githubusercontent.com/yuaotian/go-cursor-help/refs/heads/master/scripts/run/cursor_mac_id_modifier.sh | sudo bash
```

#### Linux

```
curl -fsSL https://aizaozao.com/accelerate.php/https://raw.githubusercontent.com/yuaotian/go-cursor-help/refs/heads/master/scripts/run/cursor_linux_id_modifier.sh | sudo bash
```

#### Windows

```
irm https://aizaozao.com/accelerate.php/https://raw.githubusercontent.com/yuaotian/go-cursor-help/refs/heads/master/scripts/run/cursor_win_id_modifier.ps1 | iex
```

### Windows 管理员终端运行和手动安装

#### 方法一：使用 Win + X 快捷键

```
按下 Win + X 组合键
在弹出的菜单中选择以下任一选项:
- "Windows PowerShell (管理员)"
- "Windows Terminal (管理员)"
- "终端(管理员)"
(具体选项因 Windows 版本而异)
```

#### 方法二：使用 Win + R 运行命令

```
按下 Win + R 组合键
在运行框中输入 powershell 或 pwsh
按 Ctrl + Shift + Enter 以管理员身份运行
或在打开的窗口中输入: Start-Process pwsh -Verb RunAs
在管理员终端中输入以下重置脚本:

irm https://aizaozao.com/accelerate.php/https://raw.githubusercontent.com/yuaotian/go-cursor-help/refs/heads/master/scripts/run/cursor_win_id_modifier.ps1 | iex
```

#### 方法三：通过搜索启动

![](img/e5c34f8b7967a343238ee9dd0a424b73.png)

> 在搜索框中输入 `pwsh`，右键选择"以管理员身份运行"

![](img/410b906a5dd945ac743b4118cb7191f7.png)

在管理员终端中输入重置脚本:

```
irm https://aizaozao.com/accelerate.php/https://raw.githubusercontent.com/yuaotian/go-cursor-help/refs/heads/master/scripts/run/cursor_win_id_modifier.ps1 | iex
```

## PowerShell 安装指南

如果您的系统没有安装 PowerShell，可以通过以下方法安装：

### 方法一：使用 Winget 安装（推荐）

1. 打开命令提示符或 PowerShell  
1. 运行以下命令:

```
winget install --id Microsoft.PowerShell --source winget
```

### 方法二：手动下载安装

1. 下载对应系统的安装包:

+ `PowerShell-7.4.6-win-x64.msi` (64位系统)  
+ `PowerShell-7.4.6-win-x86.msi` (32位系统)  
+ `PowerShell-7.4.6-win-arm64.msi` (ARM64系统)

1. 双击下载的安装包，按提示完成安装

> 💡 如果仍然遇到问题，可以参考 Microsoft 官方安装指南

### Windows 安装特性

+ 🔍 自动检测并使用 PowerShell 7（如果可用）  
+ 🛡️ 通过 UAC 提示请求管理员权限  
+ 📝 如果没有 PS7 则使用 Windows PowerShell  
+ 💡 如果提权失败会提供手动说明

完成后，脚本将：

1. ✨ 自动安装工具  
1. 🔄 立即重置 Cursor 试用期

## 手动安装

> 从 [releases](https://github.com/yuaotian/go-cursor-help/releases/latest) 下载适合您系统的文件

Windows 安装包、macOS 安装包、Linux 安装包

> 参考资料：https://github.com/yuaotian/go-cursor-help/tree/master

> 请遵守 MIT 许可协议：

> Copyright (c) 2024

> Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

> The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

## 删除账号 Fake 方法

没想到吧，还有，最后，你要使用老金最早推荐的删除账号 `fake` 方法，否则每日 50 次，3 天共计 150 次后账号还是需要升级 Pro。

详情请查看 [Cursor 0.44 版本说明](https://tffyvtlai4.feishu.cn/wiki/K6HYwMw31iBq91kGQ8tcNPBDnGg?fromScene=spaceOverview#Cf2Ed4bGHo7iMkxucp6cKSMOn6b) 最后的破解方法（即前文提到的 `fake`）。

> 项目地址：https://github.com/bestk/cursor-fake-machine

## 免责声明

本文仅出于学习和研究目的分享，不对使用者因采用本文方法而可能产生的任何后果承担责任。请注意：

1. 本文内容仅供学习和研究使用，不得用于任何商业目的或非法活动。  
1. 使用者应当遵守 Cursor 和 Claude 的服务条款及相关法律法规。  
1. 本文不鼓励也不支持任何形式的滥用、破解或规避付费服务的行为。请尊重知识产权和服务提供商的利益。  
1. 如果您决定采用本文描述的方法，即表示您已完全理解并接受本免责声明的全部内容，请合理使用。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
