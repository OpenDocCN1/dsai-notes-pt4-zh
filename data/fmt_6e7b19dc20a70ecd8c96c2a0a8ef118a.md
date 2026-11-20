
# RPA+Excel 提取表格中的超链接

> 来源：[https://my.feishu.cn/docx/WA7OdpBtWoHaTWxj7uXcMIiQnff](https://my.feishu.cn/docx/WA7OdpBtWoHaTWxj7uXcMIiQnff)

用echotik发现导出的数据中，链接的部分居然是超链接，循环Excel的时候就没办法打开网页。想了好多办法还问了AI，结果最后影刀中有个小功能，Excel扩展指令，很方便的就提取出超链接的链接了。

## 痛点

echotik导出的视频数据想要下载视频，但是发现没有视频链接...只能先提取echotik链接，然后用循环打开的方式点击跳转到TikTok，在把视频链接填写到表格中，最后表格中提取出TikTok视频链接，再用哼哼猫批量下载。

## 提取链接

![](img/f99f9cbcd2957e7a53c4e257dd9f5552.png)

然后再循环打开，就可以提取出视频的链接了，然后在用哼哼猫批量下载。

![](img/1cb8990f4cd6a772dff57053e7b649af.png)
