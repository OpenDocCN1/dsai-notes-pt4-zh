
# 设备信息权限是什么

> 来源：[https://docs.feishu.cn/docx/KCj9dJ56Yo9jknxWt9vcjUA5nbg](https://docs.feishu.cn/docx/KCj9dJ56Yo9jknxWt9vcjUA5nbg)

## 问

请问“在线影音类”App个人信息收集情况测试报告中的，”设备信息权限“是指什么？

![](img/2c71db072483a3991e8094cdfd6ad5f3.png)

## 答1

指的是`read privileged phone state`，这个权限。

Android 10 更改了设备标识符的权限，让所有设备标识符现在都受到 `READ_PRIVILEGED_PHONE_STATE` 权限的保护。在 Android 10 之前的版本中，永久性设备标识符（IMEI/MEID、IMSI、SIM 和 build 序列号）受到 `READ_PHONE_STATE` 运行时权限的保护。系统仅会向使用平台密钥进行签名的应用以及特权系统应用授予 `READ_PRIVILEGED_PHONE_STATE` 权限。

## 追问

这东西会弹窗索要权限吗？

## 答2

不会的。

![](img/cea28b205e2ce72e5cb3089c5258eb74.png)

[权限获取](https://cbe8t1r4a5.feishu.cn/wiki/wikcn5sa8AZhNVbp1RH4oe0Msuh)
