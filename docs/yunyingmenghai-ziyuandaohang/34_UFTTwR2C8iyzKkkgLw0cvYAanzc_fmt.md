
# VMware安装微软官方win11镜像

> 来源：[https://yunyingmenghai.feishu.cn/docx/UwmKd4KPWoRnygx70ogckL9YnOf](https://yunyingmenghai.feishu.cn/docx/UwmKd4KPWoRnygx70ogckL9YnOf)

## 直接通过 VMware 更新服务器下载

如果你在注册 Broadcom 账号或下载时遇到奇奇怪怪的问题，或者根本不想注册，可以直接通过 VMware 的更新服务器下载：

1. 访问 [VMware 更新软件源](https://softwareupdate.vmware.com/cds/vmw-desktop/ws/)网站，在列表中找到最新版本，如 `17.x.x`。
2. 通过类似以下地址/目录下载 `tar` 压缩包，并解压获得安装文件。

网盘下载：`https://pan.quark.cn/s/dc01fc6c2f8e`

```
https://softwareupdate.vmware.com/cds/vmw-desktop/ws/17.6.1/24319023/windows/core/
```

注：个人免费

![](img/32569d550ac8a64ed25e57c5fafb7908.png)

## 微软官网win11企业版虚拟机镜像

`https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/`

下载链接：`https://pan.quark.cn/s/fa6215ec3989`

设置中文：

![](img/fb928fc0b1043e828e9dd2ae6375aa92.png)

配置完后拍快照

## 常见问题

VMware Workstation 在此主机上不支持嵌套虚拟化。

模块`HV`启动失败。

未能启动虚拟机。

![](img/748ca3ce9e70d8583c73b8756b204160.png)

原因分析：

不要同时开启`hyper-V`和VMware虚拟机软件，两者只能选择其一来使用，要用VMware虚拟机就必须关闭`hyper-V`。

解决方案：

开始菜单上右键选择“`Windows PowerShell (管理员)`”

输入

```
bcdedit /set hypervisorlaunchtype off
```

回车后，记得需要重启才算真正生效！

等重启后，VMware虚拟机使用就没问题了！
