
# 即梦动作模仿生动模式（DreamActor M1）使用指北

> 来源：[https://yunyinghui.feishu.cn/docx/OCYXdlenBoUUGpxdO2VcAwZMnye](https://yunyinghui.feishu.cn/docx/OCYXdlenBoUUGpxdO2VcAwZMnye)

> Hi 亲爱的即梦创作者～过去半年，我们收到了非常多对「动作模仿」效果的反馈和对新特性的呼声。这一次我们带来了全新的 DreamActor M1 模型，它具有更稳定、更逼真的效果，同时支持了大家呼声极高的横屏比例输出。我们准备了一份对能力的小介绍供参考，祝创作愉快～

+ **体验方式：**直接在即梦 Web 或 App 中使用「动作模仿-生动模式」功能进行生成即可

## 能力入口

![](img/5e3ec22aa505df75050984bd3269781e.png)

## 能力更新

1. 突破竖屏画幅限制，各种画幅、比例视频均能高度还原

![](img/1469d90d3eb0fcef1e1e657d06f5fa9a.png)

[5.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/E5NybIRTxo58Dmx1vQvcHns1nih)

![](img/1fbb8200c6f5cd686fee676aedb22404.png)

[1.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AIU7bWe9CoimrUxLV9AcUEM4ndc)

![](img/9fab9ae45998c727f4ff18297b7c0c24.png)

[8.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DNt2bz6RuovVXcxxY4ecrzYFnQg)

2. 支持更多风格角色

![](img/e93d53f0774fbaf595b680d840305b66.png)

[diversity-2.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WEv2bxvIso2ouqxv00Ucxl9wnmd)

![](img/1fe295b4b60c564e4c8b08c6b6f7124a.png)

[diversity-7.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DzIYbDOU4oCeDXxTUKRc1FWHnWf)

![](img/3c427f75ff5b27e78b53f7a49a09faf6.png)

[diversity-6.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QizJb8uzOoxlFOxL5u5c5tldnvh)

![](img/9228672edbaabe377c3ed3ef441d0293.png)

[diversity-5.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IcH3bVfzaos2aDxb7jxc7rkinve)

![](img/d2fb1773a4ad098bf432ba7034e29852.png)

[diversity-4.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IXCjbWhrxoOQXExvwR6cWue6nsd)

![](img/3aaf6aed16d65e2c34c15d725f906a5c.png)

[diversity-3.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CF96bXiFCogdgyxHMTHcDzoPnjh)

![](img/83cf08eb318f67c6b6ed66f771b14c5a.png)

[diversity-1.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BpA8buPjzosI4IxaBg1cjjNjnyd)

+ 具备一定的运镜还原能力

![](img/d2e80c8ace0037b17787d7bcc9fc9b75.png)

[102 (2).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YiOTbqVDkobPU5xGkyqcycHfnnb)

驱动视频

![](img/971af594c3ff5e36fa1ee17cb98a83be.png)

输入图片

![](img/4d7a7d239f484e12f5b87efe1152d35f.png)

[107.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PRXlbIrRCoou21xwMFPcX19snOD)

输出视频

## 一些基础参数

| 视频时长 | 最长 30s （为获得最佳效果，建议单个片段时长控制在 10s 左右） |

| --- | --- |

| 输入视频比例 | 不限 |

| 输出比例 | 16:9、4:3、1:1、3:4、9:16 |

| 帧率 | 24 FPS |

## 如何获得更好的生成效果

### Tip No.1：尽可能保证输入图片与视频的人物比例、画幅相近

驱动视频与图片中人物在身体露出比例、姿态上越相近，还原效果越稳定。另外，对于需要驱动四肢或手部进行运动的场景中，参考图中完整露出角色四肢/手部有助于获得更好的抽卡结果。

✅ 稳定 Case 例：

![](img/68447130ec918e6a96085ecd0d804ca5.png)

输入图片

![](img/965749932b20c2bfd78bc753e52b1e82.png)

输入视频

⚠️ 不稳定 Case 例：

![](img/040c531fe29ab0a84247186db20a64cc.png)

输入图片

![](img/e69435bab953a29e959b9cc429644d77.png)

输入视频

### Tip No.2：使用更高清晰度的视频作为输入

当需要生成精细部位动作，如手指动作时。我们建议尽可能使用清晰、手指分明的视频作为输入。对于需要还原微表情的视频时，清晰的视频输入也有利于提高模型精确还原的概率。

## 各品类内容支持情况

| 维度 | 类目 | 示例（如有） | 成熟度 |

| --- | --- | --- | --- |

| 画风 | 写实/影视 |  | ✅ 较稳定 |

|  | CG/3D/皮克斯 |  | ✅ 较稳定 |

|  | 平面/二次元/水墨 |  | ⚠️ 需要尝试 |

| 主体类型 | 具有人类特征的角色 |  | ✅ 较稳定 |

|  | 常见宠物（猫狗） |  | ⚠️ 需要尝试 |

|  | 其他动物 | - | ❌ 效果不稳定 |

| 输入视频中的镜头运动 | 固定镜头（人物景深位置变化） |  | ✅ 较稳定 |

|  | 简单运镜（跟随、简单推拉摇移） |  | ✅ 较稳定 |

|  | 复杂运镜（如环绕） | - | ❌ 效果不稳定 |

| 画幅及肢体露出 | 肩部及以上（不含手部） |  | ✅ 较稳定 |

|  | 肩部及以上（含手部动作） |  | ✅ 较稳定（输入视频尽可能露出清晰手部） |

|  | 半身近景 |  | ✅ 较稳定 |

|  | 全身中景 |  | ⚠️ 需要尝试（人物在画面中占比过小时，面部细节可能丢失） |

|  | 全身远景 | - | ❌ 效果不稳定 |

![](img/5900971c95140642de677b39ea759720.png)

[图片驱动结果视频 (16).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Yb79b8ItroK50Ix2HC4cVsGRnwb)

![](img/e74c4bfaa364a43a3bfb0c5e4469390a.png)

[图片驱动结果视频 (5).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/D4XYbKomColf18x0xLWcaKyYnLf)

![](img/eb423965fa0d591e352a1fd3b2fd0f47.png)

[图片驱动结果视频 (4).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CI9mb62KooOyNJx8r7JcTccjnFh)

![](img/5581e14aee5f98fc81b555e5a18817ba.png)

[下载 (35).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YCx8bxMSGoNciyxMhJpc14cunkf)

![](img/0bf91c66ea9c96bd436927e3d9982c18.png)

[jimeng-2025-06-30-8096.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CATSb8SzrolGadxUSRkcBxQEnWe)

![](img/2d7d5ea126a43618e2c12820e8c4b129.png)

[70.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KfLpb9EmxoMINQxSX4TciQcgneh)

![](img/519d4747011e715963bbc92b9766d9d5.png)

[95.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SKnPbSO53o4IgcxfZh9cr2gmnQb)

![](img/b56f036cfbb0d2744754259aafaf2dbf.png)

[72 (1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YfP2bOucjoN7drxQbESccsHjn7f)

![](img/a764f4d3f4daef87ade211aef7b2a823.png)

[图片驱动结果视频 (3).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QNeMbRwKAomR73xeZhfcpvjWn9g)

![](img/73949be9cb70273dcfbee333a75a91c1.png)

[test_girl.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HhNebwlk5oMliXxwUyrcsNzinag)

![](img/fe96693c25f27357ae9f36bb5f76af0b.png)

[下载 (2) (1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/A1nNbqZ29oaoAPxDKWhcyEaWnEg)

![](img/508d8e260f75ccb14964d10e8a1f7429.png)

[下载 (10) (1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GbvCbBLEco3g4FxTwuxcPKjnntb)
