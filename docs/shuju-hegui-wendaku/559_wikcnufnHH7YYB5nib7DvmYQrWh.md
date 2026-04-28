# NEARBY_WIFI_DEVICEs权限

> 来源：[https://docs.feishu.cn/docx/GaafdPhdMojvmzxGkltcEEwpnTc](https://docs.feishu.cn/docx/GaafdPhdMojvmzxGkltcEEwpnTc)

**-问**：请问如何理解NEARBY_WIFI_DEVICEs权限与实际位置信息无关 。

**-答1：****调临近设备与位置无关啊，字面**意思。这个之前检查里也有的，拒绝地理位置，不得通过其他权限收集位置信息。权限和个人信息两个维度。

**-答2：**室内相对位置和经纬度地理位置的差异。以 Android 13 为目标平台的应用现在可以通过 “neverForLocation” 属性来完善申请 NEARBY_WIFI_DEVICES 权限。

**-答3：**我理解是这样，**原来可**以通过这台设备周围wifi类似的相关信息(不需要任何授权)，例如ssid清单，序列号，周围基站等等来推断，在不获取设备位置权限(包括粗略位置权限)的情况下，也能推断相对位置的区域，这个确实不是真实的gps，不过在特定物理范围和典型wifi信息的特征下，也可以做到非常精准。现在gg要这个的单独授权了。

[权限获取](https://cbe8t1r4a5.feishu.cn/wiki/wikcn5sa8AZhNVbp1RH4oe0Msuh)

[技术概念科普](https://cbe8t1r4a5.feishu.cn/wiki/GfWtwCXWbiBzgQk1sxUcfFgXnSP)