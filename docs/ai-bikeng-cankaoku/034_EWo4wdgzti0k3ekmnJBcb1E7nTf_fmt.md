
# NAS服务器

> 来源：[https://xodnytdcaw.feishu.cn/docx/ZZesdqR3SomCgLxcPAacvzP3nwf](https://xodnytdcaw.feishu.cn/docx/ZZesdqR3SomCgLxcPAacvzP3nwf)

## 准备工作

飞牛系统镜像：[点此下载](https://www.fnnas.com/download?key=fnos)

U盘写入工具：[balena](https://etcher.balena.io/)

斐讯N1价格40，原文：https://x.com/HiFrey/status/2004821815944958265

硬盘盒10源

硬盘：100

输入：飞牛

## 刷机步骤

运行一个`docker`来做旁路由

下载[飞牛系统包](https://pan.baidu.com/s/1iPOpcatTzk7UXeAFocnqVA?pwd=uns3#list/path=%2F&parentPath=%2Fsharelink324355271-705914764496958)

刷到U盘，让N1从U盘启动

启动成功后登录，`root` 密码也是`root`，启动需要的时间比较久，大概需要15-20分钟，登录`root`后，输入

```
bash install-to-emmc.sh
```

刷入后拔掉U盘，启动即可
