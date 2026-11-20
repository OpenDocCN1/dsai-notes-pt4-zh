
# Code Interpreter

> 来源：[https://docs.feishu.cn/docx/PYSadx2m1oPauTxWsQZc4Zb2nnc](https://docs.feishu.cn/docx/PYSadx2m1oPauTxWsQZc4Zb2nnc)

## 他能做什么

从名字可以理解，他是一位代码的翻译官，可以理解并执行编程语言的指令。

可以理解为有这样一个「房间」，你递给他一些文件，他会帮你理解并且执行代码。

具体分为

+ 读取文件、代码
+ 现已支持多文件
+ 解析代码
+ 执行代码

### 代码能力

+ `Python`：执行`Python`代码
+ 数据处理：可以使用`python`的数据处理库，如`pandas`和`numpy`
+ 数据可视化：可以使用`Python`的可视化库，如`matplotlib`，创建图表
+ 文件处理：读取和写入文件，包括`txt`文件、`CSV`文件、`Excel`文件等（对中文支持比较差）
+ 数学计算：可以使用`Python`的计算库，如`scipy`和`numpy`
+ 机器学习模型：可以使用简单的`Python` `ML`库
+ 文本分析：可以使用`Python`的文本处理库

### 限制

+ 无网络访问
+ 无本地文件系统访问权限，无法写入本地文件；无操作系统权限
+ 资源限制

## 需求场景

### 数据分析

+ 清理数据
+ 理解、分析、解释数据
+ 数据可视化
+ 预测数据

### PDF操作

> 目前已经用Claude 2

### PDF导出图片

可以有更灵活的要求，比如导出图片为2行2列

### OCR

给他一张图片，让他`OCR`识别文字，返回一个`txt`文件

### 生成二维码

生成一个二维码，内容为：……

### `Markdown`格式 —— 文本 转换

> [来源](https://m.okjike.com/originalPosts/64a8a930b1d391675decf9df?s=eyJ1IjoiNjI1ZmQzNzdmNzU0NTMwMDEwYzc2MjcwIiwiZCI6MX0%3D)

场景：`Markdown`格式 —— 文本 转换

使用方式：把`Markdown`文件发给Chat GPT，让他原文输出，但是只把相关文字连接的格式改成文字

## 优势

1. 数学能力和更精确的文字工作
1. 通过代码，帮助AI降低幻觉和迷惑的概率
1. 图文结合
1. 辅助编程、执行程序

## 参考

「1」https://mp.weixin.qq.com/s?__biz=MzUxOTg0MDkxNg==&mid=2247484325&idx=1&sn=40a8a07bb0751dd30349c0c8bbab1554&chksm=f9f2365ece85bf487b6d17efc48e78d7926b0fe881100a642a6da82a0939101dae3e051dea64&mpshare=1&scene=1&srcid=0708nX3pRUElutjz5ZPF50Bz&sharer_sharetime=1688817405410&sharer_shareid=e6b076d366bef10182cfdf42b22845f6#rd

「2」https://mp.weixin.qq.com/s?__biz=Mzg5NTc0MjgwMw==&mid=2247489302&idx=1&sn=9e0f22e13f0e77f7f001b406d66f8857&chksm=c00af12af77d783c6344c102d4577dc52bcb2df5fe5320439a77fc665d19c8261046e5ee44bc&mpshare=1&scene=1&srcid=0714qE6ikVqaWqVC0sCf9Ivj&sharer_sharetime=1689335095084&sharer_shareid=53419fa24e911f32abc2fd25def030d6#rd
