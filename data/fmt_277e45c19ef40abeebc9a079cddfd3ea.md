
# 公共存储区域是什么

> 来源：[https://docs.feishu.cn/docx/DXD2dPQJZoHHpSxms66cO3oEnSd](https://docs.feishu.cn/docx/DXD2dPQJZoHHpSxms66cO3oEnSd)

## 问

请问公共存储区怎么理解呀？只知道剪切板会设计，单列不太懂其他具体场景或者技术功能。

## 答

简单来说，你可以这样理解：指需要`READ_EXTERNAL_STORAGE`和`WRITE_EXTERNAL_STORAGE`权限访问的存储区域（App自己目录之外的区域），一般来说在较高版本的安卓上App把内容存在自己目录下就好了，不需要申请这俩权限。

比如访问音频视频可以用媒体库，不需要这俩权限。

比如访问图片，可以用Android 13的图片选择器API。

但是，业务有时候为了兼容低版本的系统，还是得访问公共存储区。

[技术概念科普](https://cbe8t1r4a5.feishu.cn/wiki/GfWtwCXWbiBzgQk1sxUcfFgXnSP)
