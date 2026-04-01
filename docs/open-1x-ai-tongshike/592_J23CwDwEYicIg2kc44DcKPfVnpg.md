# [案例]一张照片生成新年数字人拜年

> 来源：[https://datawhaler.feishu.cn/docx/GVoKdupufohMgkxoo78cD17An6g](https://datawhaler.feishu.cn/docx/GVoKdupufohMgkxoo78cD17An6g)

> 特别感谢@华泽 提供的数字人相关信息

# 先找一张图片

自己的图片，比如

![](img/7c953727ee218628084370dc6176f3ad.png)

换个喜庆点的装束

这里用instanceID来换，这个是开源免费的，但是需要排队使用，有时候不稳定

https://www.modelscope.cn/studios/instantx/InstantID/summary

![](img/39c8e664595304129f619fc4a125bc5a.png)

或者用其他的来做，比如通义千问APP，有拍新年照片的（固定模板）

![](img/1302bd46c4357d47958a0724d564b686.png)

## 拜年祝词

用人情世故大模型来写一段拜年祝词

![](img/93ce230a962aefeda77897449cf0cd64.png)

新春佳节到，给大家拜年啦！祝愿各位朋友们：笑一笑，十年少，财源滚滚如潮水，爱情甜蜜似蜜桃，身体健康胜神仙！鞭炮声声迎新春，好运连连福满门，记得吃饺子，换新衣，开怀畅饮，把欢乐传递给每一个人！ 如果哪天咱们在街头偶遇，别忘了互道一声：新年好！愿我们的友谊像这春节的烟花，绚烂生辉，永不散去！

# 拜年小视频

注意上传名人脸容易失败

类似DID、Heygen上传马斯克的脸都失败了

![](img/0552cc54a4e1d9de88ff0d036220226b.png)

这里用了DID来生成了一个，然后移除了一下水印

![](img/eca0b75da398007944607b4ce85b6833.png)

[kalvin_tool_大锤拜年.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WzYibanApozpVMxHImtcd6sFn5g)

[AI语音克隆教程🎤](https://qixiangxingqiu.feishu.cn/wiki/KJBGw9XMziPCWRkgSUicQwLhnMh?from=from_copylink)里声音没有换成大锤的，但其实声音也可以换的，参考

# **参考资料**

**数字人口播视频生成工具**

https://zhuanlan.zhihu.com/p/641563166

**人情世故大模型**

开源地址: https://github.com/SocialAI-tianji/Tianji

试用地址：http://120.76.130.14:6006/prompt/