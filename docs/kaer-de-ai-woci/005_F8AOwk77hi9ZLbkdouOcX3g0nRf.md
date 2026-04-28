# 数字人换脸Roop篇 - 霉霉跳韩舞

> 来源：[https://aiwarts101.feishu.cn/docx/LYkbdih14oeMEdxonKVcK3uXnQb](https://aiwarts101.feishu.cn/docx/LYkbdih14oeMEdxonKVcK3uXnQb)

大家好，我是卡尔的AI沃茨，这篇文章是系列文，动态更新。

起因是最近推上这个ins女孩爆火，是换脸做出来的。B站上也有人做电影场景换脸，有百万播放。

所以我今天重新玩了玩数字人换脸，目前主流项目有三个：

[Rope](https://github.com/Hillobar/Rope)

[Facefusion](https://github.com/facefusion/facefusion)

[Roop](https://github.com/s0md3v/roop)

这几个项目部署到本地有一定门槛，但是colab很好解决这个问题。

> colab的使用只需要你有谷歌账号，打开对应的代码后就能“蹭”谷歌资源跑项目

刚好看到之前收藏的[Roop Colab脚本](https://colab.research.google.com/github/FurkanGozukara/Stable-Diffusion/blob/main/ColabNotebooks/1_click_deep_fake_for_free_by_SECourses.ipynb#scrollTo=t1yPuhdySqCq)在12月24号更新了，这次就来跑一下，让大家看看效果。

1.  先运行第一个单元格

![](img/a9d3429ec963e0d4a2474ced3ee866d6.png)

1.  将你要上传的图片和视频改为红框对应的名字，点击选择对应的单元格，按shift+enter就可以运行。

![](img/da421ba9d84b3b680b0e9e3ae936ab23.png)

1.  剩下的单元格也是直接点击运行即可（一分钟的视频换脸时间是10分钟左右）

原图：

![](img/06aea287522685b7a2fb2bd434e145d6.png)

原视频：

![](img/00590629ed19a1aa8128a56a6c6c99d6.png)

[bard org.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Nzz5bWsxXo6q8mxatZ2cHRV0n2e)

换脸视频：

![](img/c1e1b089b074d992b50081839259b794.png)

[face_restored_video3.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ANQubpqTtosdd9x96XOc63lSnsf)

实际效果：尽管是在跳舞场景，有头发和手部遮挡的环境下，原视频内脸部能清晰看到片段都完成了换脸。如果你对数字人换脸有兴趣的话，欢迎链接我。微信🌍：aiwarts101