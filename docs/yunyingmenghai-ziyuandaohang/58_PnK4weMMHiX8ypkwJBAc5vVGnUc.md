
# Win11安装Kimi code

> 来源：[https://yunyingmenghai.feishu.cn/docx/GOZyd4l9ioSEn7xwB9RcCFountf](https://yunyingmenghai.feishu.cn/docx/GOZyd4l9ioSEn7xwB9RcCFountf)

`https://www.kimi.com/code`

某些安全软件会阻止新线程的网络访问：

## 临时关闭防火墙测试

`powershell`

复制

```powershell
netsh advfirewall set allprofiles state off
# 测试 curl 是否正常
curl https://api.moonshot.cn
# 测试完记得开启
netsh advfirewall set allprofiles state on
```

## 检查第三方安全软件拦截

检查是否有第三方安全软件拦截（如 360、火绒、卡巴斯基 等），临时退出测试。

## 最通用的自动检测路径的防火墙规则命令

复制粘贴即可，无需手动修改路径：

`powershell` # 自动检测 `kimi` 路径并添加防火墙规则（管理员 `PowerShell`）

```powershell
$kimiPath = (Get-Command kimi).Source; netsh advfirewall firewall add rule name="Kimi CLI" dir=out action=allow program="$kimiPath" enable=yes
```

或者分步写法（更稳定）：

`powershell`

```powershell
# 1. 获取实际路径
$kimiPath = (Get-Command kimi).Source

# 2. 添加规则
netsh advfirewall firewall add rule name="Kimi CLI" dir=out action=allow program="$kimiPath" enable=yes

# 3. 验证
netsh advfirewall firewall show rule name="Kimi CLI"
```

## PowerShell 默认阻止执行网上下载的脚本

你需要调整执行策略。

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

```powershell
irm https://code.kimi.com/install.ps1 | iex
```

## uv 已经安装成功后，终端没识别到新路径

添加路径到系统变量。

```powershell
[Environment]::SetEnvironmentVariable("Path", "$env:USERPROFILE\.local\bin;" + [Environment]::GetEnvironmentVariable("Path", "User"), "User")
```

```powershell
uv tool install --python 3.13 kimi-cli
```

## 临时禁用实时保护

如果方案 1 无效，可能是 Windows Defender 阻止了：

1. Windows 安全中心 → 病毒和威胁防护 → 管理设置
2. 暂时关闭实时保护（安装完记得重新打开）关闭杀毒软件如腾讯管家
3. 再次运行安装命令

```powershell
uv cache clean
uv tool install kimi-cli --reinstall
```

## 如果安装较慢或失败，可以尝试国内镜像加速

```powershell
# 设置国内镜像（清华源）$env:UV_DEFAULT_INDEX = "https://pypi.tuna.tsinghua.edu.cn/simple"
uv tool install kimi-cli
```

![](img/d63966d9aee8648e40b9e849266ed07a.png)

### 如何升级 Kimi Code CLI

使用 `uv` 升级到最新版本：

```powershell
uv tool upgrade kimi-cli --no-cache
```
