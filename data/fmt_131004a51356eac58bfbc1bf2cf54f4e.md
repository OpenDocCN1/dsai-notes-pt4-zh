
# Windows 安装 Docker

> 来源：[https://xmgtoagi.feishu.cn/docx/Du8QdhNeToQ1R6xAbtkcxd5VnLc](https://xmgtoagi.feishu.cn/docx/Du8QdhNeToQ1R6xAbtkcxd5VnLc)

试了很多种 Docker 安装方法，最后都没成功，废话不多说，直接开搞。

## 1. 下载 Windows 版本软件

下载地址：https://docs.docker.com/desktop/install/windows-install/

![](img/a9fa0ee0dd41e5efb2e3cd8583fa0428.png)

## 2. 开启 Windows 的 Hyper-V 虚拟化技术

打开控制面板——>程序——>启动或关闭 window 功能

![](img/0a3bed448e207d7689aaabb27b9a8895.png)

![](img/902b1c625667d7519fe10d45f22087db.png)

需要开启 Hyper-V 虚拟化技术。如果您的计算机不支持 Hyper-V 或者没有启用 Hyper-V，则会出现 Windows 无法启动 Docker 的问题。

![](img/2cc8b07cab2a88acc0d602db7f8395d0.png)

## 3. 安装 Docker

双击运行下载好的安装包

![](img/6e77101eb4d75792ea028c5bbfe16347.png)

有的同学可能会出现以下报错：`Docker Desktop requires Windows 10 Pro/Enterprise/Home version 19044 or above.`

意思是 `docker` 只支持 Windows 10 专业版/企业版/家庭版，并且版本要高于 19044

好吧，到这里我都想升级 Windows 系统或者重装系统了

![](img/bbf9821da7cfb03964598f7218e516ea.png)

但是不甘心，我的系统信息如下，大家如果没有出现这种报错就不用管了。操作系统版本已经是给我改过了，修改方式如下：

![](img/1431aaa46feeb6f09018f8c972143aa5.png)

解决方法

输入 `cmd` 命令，运行 `regedit`，回车，出现注册表编辑器，找到：`计算机\HKEY LOCAL MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion`。确认 `EditionId` 是否为 `Professional`，如果不是对其进行修改。

![](img/b109f37ce5abe2f3424443a2c49158b3.png)

修改 `CurrentBuild` 和 `CurrentBuildNumber`，改为提示要求的 19044

![](img/46ff67741bd05805892d8e694bb8ec17.png)

然后重新双击安装，进入到了安装界面，注意，不要勾选第一项，不然会安装失败，因为我们设置了 Hyper-V。

![](img/4597d05afe0aee5d976d9fe8da8f4bd7.png)

点击 close and log out

![](img/d3efdf12dadfff9c4259388ab881fbdf.png)

点击 close and log out，电脑会自动重启

![](img/cee893ee64b1f248ee597b1b3a92e00c.png)

选择第一个，使用推荐设置，点击完成

![](img/1acbf06b739e47d3aebca553f02f97d0.png)

不用登录，直接点击不登录

![](img/f112264c8eb0add6a303fa90717fb522.png)

进入到 Docker Desktop 界面

![](img/13691c4944bb0d83ddbaa38b608e7ca8.png)

设置资源下载位置，默认是在 C 盘，根据自己需求进行修改。

![](img/c4a8eefa47632d794448d9f0146ca2db.png)

设置镜像，不然无法下载镜像，配置如下：

![](img/c9cdd845652ad2cf198f8cc2989e12c5.png)

```
{
  "builder": {
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  },
  "experimental": false,
  "registry-mirrors": [
    "https://hub.littlediary.cn/"
  ]
}
```

然后 `cmd`，执行命令，查看 `docker` 版本，`docker` 安装成功。

```
docker --version
```

![](img/0b6e70cf9bba8b8f63553708f5d1a6ca.png)
