# cmd/PowerShell美化全攻略

> 来源：[https://xodnytdcaw.feishu.cn/docx/Qfusdp0MUogyTcxWKHWccEbmnde](https://xodnytdcaw.feishu.cn/docx/Qfusdp0MUogyTcxWKHWccEbmnde)

当95%的开发者还在使用默认终端时，高手早已通过Windows Terminal重构了生产力环境。支持GPU加速的文本渲染、自定义JSON配置的无限可能、WSL2无缝集成——这不是简单的界面美化，而是终端效能的次世代革命。我们拆解了微软官方文档中隐藏的7个性能优化参数，帮你打造毫秒级响应的梦幻命令行。

## 一.安装必备软件

在开始美化之前，先安装以下工具：

#### [oh-my-posh](https://github.com/JanDeDobbeleer/oh-my-posh/releases/tag/v26.18.0) —— 终端美化利器

#### [clink](https://github.com/chrisant996/clink)—— CMD 增强工具

#### Windows Terminal—— 现代终端

#### [Fastfetch](https://github.com/fastfetch-cli/fastfetch/releases/tag/2.51.1)—— 系统信息展示

## 二.cmd美化

#### 1.添加启动主题

进入 clink 安装目录，新建 oh-my-posh.lua 文件，写入以下内容：

```
load(io.popen(&#x27;oh-my-posh init cmd --config &quot;C:\\Users\\用户名\\AppData\\Local\\Programs\\oh-my-posh\\themes\\atomic.omp.json&#x27;):read(&quot;*a&quot;))()
```

主题文件存放路径：

```
C:\Users\Administrator\AppData\Local\Programs\oh-my-posh\themes  
```

更多主题参考 👉 oh-my-posh Themes

#### 2.隐藏cmd启动提示信息

每次启动都会显示 clink 版本信息？一条命令搞定：

```
clink set clink.logo none
```

#### 3.配置Fastfetch（可选）

> Fastfetch = 高性能的系统信息展示工具。

安装方式（自动配置环境变量）：

```
sudo winget install fastfetch
```

手动安装：

+   下载 fastfetch-windows-amd64.zip

+   解压到：

```
**C:\Users\用户名\AppData\Local\Mi**crosoft\WinGet\Links
```

+   添加环境变量

让 CMD 启动时自动运行 fastfetch，在 clink 插件目录创建 clink_startup.lua

```
-- 切换到指定目录
os.execute(&#x27;cd /d E:\\User\\lololowe\\Desktop&#x27;)

-- 设置 doskey 别名
os.execute(&#x27;doskey ll=lsd -lFha&#x27;)
os.execute(&#x27;doskey pwd=cd&#x27;)
os.execute(&#x27;doskey clear=cls&#x27;)

-- 运行fastfetch
os.execute(&#x27;C:\\Users\\用户名\\AppData\\Local\\Microsoft\\WinGet\\Links\\fastfetch.exe&#x27;)

-- 设置中文编码并隐藏输出(fastfetch会导致chcp 936失效)
os.execute(&#x27;chcp 936&gt;nul&#x27;)
```

fastfetch配置文件路径(%userprofile%\.config\fastfetch\config.jsonc) 如下

```
{
  &quot;$schema&quot;: &quot;https://github.com/fastfetch-cli/fastfetch/raw/dev/doc/json_schema.json&quot;,
  &quot;logo&quot;: {
    &quot;type&quot;: &quot;builtin&quot;,
    &quot;source&quot;: &quot;windows&quot;,
    &quot;height&quot;: 18,
    &quot;padding&quot;: {
      &quot;top&quot;: 2,  // 减小顶部边距，使logo上移
      &quot;bottom&quot;: 3  // 适当增加底部边距保持平衡
    }
  },
  &quot;display&quot;: {
    &quot;separator&quot;: &quot; : &quot;
  },
  &quot;modules&quot;: [
    {
      &quot;type&quot;: &quot;custom&quot;,
      &quot;format&quot;: &quot;\u001b[36m    OS Info&quot;
    },
    {
      &quot;type&quot;: &quot;custom&quot;,
      &quot;format&quot;: &quot;┌────────────────────────────────────────────────────────┐&quot;
    },
    {
      &quot;type&quot;: &quot;os&quot;,
      &quot;key&quot;: &quot;   OS&quot;,
      &quot;keyColor&quot;: &quot;red&quot;
    },
    {
      &quot;type&quot;: &quot;kernel&quot;,
      &quot;key&quot;: &quot;  󰻹 Kernel&quot;,
      &quot;keyColor&quot;: &quot;red&quot;
    },
    {
      &quot;type&quot;: &quot;host&quot;,
      &quot;key&quot;: &quot;   Machine&quot;,
      &quot;format&quot;: &quot;{name}{?vendor}&quot;,
      &quot;keyColor&quot;: &quot;red&quot;
    },
    {
      &quot;type&quot;: &quot;display&quot;,
      &quot;key&quot;: &quot;  󰹑 Display&quot;,
      &quot;format&quot;: &quot;{1}x{2} @ {3} Hz&quot;,
      &quot;keyColor&quot;: &quot;red&quot;
    },
    {
      &quot;type&quot;: &quot;uptime&quot;,
      &quot;key&quot;: &quot;  󱫐 Uptime &quot;,
      &quot;keyColor&quot;: &quot;red&quot;
    },
    {
      &quot;type&quot;: &quot;shell&quot;,
      &quot;key&quot;: &quot;   Shell&quot;,
      &quot;keyColor&quot;: &quot;red&quot;
    },
    {
      &quot;type&quot;: &quot;custom&quot;,
      &quot;format&quot;: &quot;└────────────────────────────────────────────────────────┘&quot;
    },
    &quot;break&quot;,
    {
      &quot;type&quot;: &quot;title&quot;,
      &quot;key&quot;: &quot;  &quot;
    },
    {
      &quot;type&quot;: &quot;custom&quot;,
      &quot;format&quot;: &quot;┌────────────────────────────────────────────────────────┐&quot;
    },
    {
      &quot;type&quot;: &quot;cpu&quot;,
      &quot;format&quot;: &quot;{1}&quot;,
      &quot;key&quot;: &quot;   CPU&quot;,
      &quot;keyColor&quot;: &quot;blue&quot;
    },
    {
      &quot;type&quot;: &quot;gpu&quot;,
      &quot;format&quot;: &quot;{2} {7}&quot;,
      &quot;key&quot;: &quot;  󰋵 GPU&quot;,
      &quot;keyColor&quot;: &quot;blue&quot;
    },
    {
      &quot;type&quot;: &quot;disk&quot;,
      &quot;key&quot;: &quot;   Disk&quot;,
      &quot;keyColor&quot;: &quot;magenta&quot;
    },
    {
      &quot;type&quot;: &quot;memory&quot;,
      &quot;key&quot;: &quot;   Memory&quot;,
      &quot;keyColor&quot;: &quot;magenta&quot;
    },
    {
      &quot;type&quot;: &quot;bluetooth&quot;,
      &quot;key&quot;: &quot;   Bluetooth&quot;,
      &quot;keyColor&quot;: &quot;magenta&quot;
    },
    {
      &quot;type&quot;: &quot;wifi&quot;,
      &quot;key&quot;: &quot;   WiFi&quot;,
      &quot;keyColor&quot;: &quot;magenta&quot;
    },
    {
      &quot;type&quot;: &quot;localip&quot;,
      &quot;showIpv6&quot;: false,
      &quot;showMac&quot;: false,
      &quot;showSpeed&quot;: false,
      &quot;showMtu&quot;: false,
      &quot;showLoop&quot;: false,
      &quot;showFlags&quot;: false,
      &quot;showAllIps&quot;: false,
      &quot;key&quot;: &quot;  󱦂 LAN&quot;,
      &quot;keyColor&quot;: &quot;magenta&quot;
    },
    {
      &quot;type&quot;: &quot;publicip&quot;,
      &quot;timeout&quot;: 500,
      &quot;url&quot;: &quot;http://4.ipw.cn&quot;,
      &quot;key&quot;: &quot;  󰩟 WAN&quot;,
      &quot;keyColor&quot;: &quot;magenta&quot;
    },
    {
      &quot;type&quot;: &quot;dns&quot;,
      &quot;key&quot;: &quot;  󰮌 DNS&quot;,
      &quot;keyColor&quot;: &quot;magenta&quot;
    },
    {
      &quot;type&quot;: &quot;custom&quot;,
      &quot;format&quot;: &quot;└────────────────────────────────────────────────────────┘&quot;
    },
    {
      &quot;type&quot;: &quot;colors&quot;,
      &quot;paddingLeft&quot;: 2,
      &quot;symbol&quot;: &quot;circle&quot;
    },
    &quot;break&quot;
  ]
}
```

配置启动效果如下,完整示例配置见 👉 GitHub: sqlsec/fastfetch

![](img/41c64adffc0afe2ce9af3cf43fdb5d4f.png)

#### 4.下一代 ls 命令: lsd（可选）

+   项目地址：[lsd](https://github.com/lsd-rs/lsd)

+   安装命令：

```
winget install --id lsd-rs.lsd
```

如果输入 ls 报错：

```
@echo off
dir
```

另存为**文件名** ls.bat，放到windows根目录下即可。

#### iFlow CLI

参考教程

AI智能终端，小白秒变大神的神仙玩法。。

[一个冷门但神奇的电脑用法！](https://www.bilibili.com/video/BV1ACeqzEE8g/?spm_id_from=333.1387.favlist.content.click&vd_source=206031f494850e57fd6c92ace02b1bed)

> 1、安装 Node.js：https://nodejs.org/zh-cn/download

> 2、配置加速镜像，打开终端输入：npm config set registry https://registry.npmmirror.com

> 3、安装 iFlow CLI，打开终端输入：npm install -g @iflow-ai/iflow-cli@latest

> 4、启动 iFlow CLI，打开终端输入：iflow

> 首次使用，可以通过上下键盘选择下认证方式，比如选择第1个默认的，回车后会自动打开浏览器，跳转到心流AI官网登录。

## 三.PowerShell美化

#### 1.修改 Windows Terminal 配置

按 Win + R 输入 wt → 设置 → 打开 JSON 文件，替换为配置：

里面用到的meinv.png图片可以加dqtx33领取

```
{
    &quot;$help&quot;: &quot;https://aka.ms/terminal-documentation&quot;,
    &quot;$schema&quot;: &quot;https://aka.ms/terminal-profiles-schema&quot;,
    &quot;actions&quot;: [],
    &quot;copyFormatting&quot;: &quot;none&quot;,
    &quot;copyOnSelect&quot;: false,
    &quot;defaultProfile&quot;: &quot;{61c54bbd-c2c6-5271-96e7-009a87ff44bf}&quot;,
    &quot;keybindings&quot;: 
    [
        {
            &quot;id&quot;: &quot;Terminal.CopyToClipboard&quot;,
            &quot;keys&quot;: &quot;ctrl+c&quot;
        },
        {
            &quot;id&quot;: &quot;Terminal.PasteFromClipboard&quot;,
            &quot;keys&quot;: &quot;ctrl+v&quot;
        },
        {
            &quot;id&quot;: &quot;Terminal.DuplicatePaneAuto&quot;,
            &quot;keys&quot;: &quot;alt+shift+d&quot;
        }
    ],
    &quot;newTabMenu&quot;: 
    [
        {
            &quot;type&quot;: &quot;remainingProfiles&quot;
        }
    ],
    &quot;profiles&quot;: 
    {
        &quot;defaults&quot;: {
            &quot;useAcrylic&quot;: true,
            &quot;acrylicOpacity&quot;: 0.2,
            &quot;backgroundImage&quot;: &quot;D:/web3/meinv.png&quot;,
            &quot;backgroundImageOpacity&quot;: 0.4,
            &quot;fontFace&quot;: &quot;AdwaitaMono Nerd Font&quot;,
            &quot;fontSize&quot;: 14
        },
        &quot;list&quot;: 
        [
            {
                &quot;commandline&quot;: &quot;%SystemRoot%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe&quot;,
                &quot;guid&quot;: &quot;{61c54bbd-c2c6-5271-96e7-009a87ff44bf}&quot;,
                &quot;hidden&quot;: false,
                &quot;name&quot;: &quot;Windows PowerShell&quot;
            },
            {
                &quot;commandline&quot;: &quot;%SystemRoot%\\System32\\cmd.exe&quot;,
                &quot;guid&quot;: &quot;{0caa0dad-35be-5f56-a8ff-afceeeaa6101}&quot;,
                &quot;hidden&quot;: false,
                &quot;name&quot;: &quot;\u547d\u4ee4\u63d0\u793a\u7b26&quot;
            },
            {
                &quot;guid&quot;: &quot;{b453ae62-4e3d-5e58-b989-0a998ec441b8}&quot;,
                &quot;hidden&quot;: false,
                &quot;name&quot;: &quot;Azure Cloud Shell&quot;,
                &quot;source&quot;: &quot;Windows.Terminal.Azure&quot;
            }
        ]
    },
    &quot;schemes&quot;: [],
    &quot;themes&quot;: []
}
```

#### 2.添加主题

code $PROFILE/ notepad $PROFILE

其他主题文件夹，进入C:\Users\Administrator\AppData\Local\Programs\oh-my-posh\themes

```
oh-my-posh init pwsh --config $env:POSH_THEMES_PATH\M365Princess.omp.json | Invoke-Expression
```

#### 3.解决字体乱码

安装 [Nerd Fonts](https://www.nerdfonts.com/font-downloads?utm_source=chatgpt.com)，解决乱码问题。

#### 隐藏powershell启动提示信息

打开设置,点开powershell的配置文件，在命令行处修改为：

```
powershell.exe -nologo
```

#### 4.集成到**右键菜单**

在 PowerShell 以管理员身份运行：

```
$key = &quot;HKEY_CLASSES_ROOT\Directory\Background\shell\wt&quot;
New-Item -Path &quot;Registry::$key&quot; -Force
New-ItemProperty -Path &quot;Registry::$key&quot; -Name &quot;(Default)&quot; -Value &quot;Open Terminal Here&quot; -Force
```

修改右键图标：

```
$terminalKey = &quot;HKEY_CLASSES_ROOT\Directory\Background\shell\wt&quot;
$psIconPath = &quot;C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe,0&quot;
Set-ItemProperty -Path &quot;Registry::$terminalKey&quot; -Name &quot;Icon&quot; -Value $psIconPath
Stop-Process -Name explorer -Force
```

## 四.常用命令

## 参考教程

https://blog.lololowe.com/posts/33ec/

参考教程https://www.bilibili.com/video/BV1dntWe9Eub/?spm_id_from=333.337.search-card.all.click&vd_source=206031f494850e57fd6c92ace02b1bed

参考教程https://www.bilibili.com/video/BV1Qa411T7Au/?spm_id_from=333.337.search-card.all.click&vd_source=206031f494850e57fd6c92ace02b1bed