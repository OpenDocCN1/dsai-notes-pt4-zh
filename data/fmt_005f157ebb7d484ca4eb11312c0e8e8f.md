
# AI生成冷门歌手翻唱

> 来源：[https://seigy6zzam.feishu.cn/docx/SMRXd63Eeo7YLhx3I0nca4GOnUh](https://seigy6zzam.feishu.cn/docx/SMRXd63Eeo7YLhx3I0nca4GOnUh)

## 背景

最近AI歌手很火啊！歸藏分享了他使用 `So-VITS-SVC` 项目包来推理和训练的详细全过程，并分享了所有的资料包和使用教程。

## 如何实现？

所有的应用都是基于 [`So-VITS-SVC 4.0`](https://github.com/svc-develop-team/so-vits-svc) 这个开源项目，B站上有两个做整合包的up主，分别是

+ **羽毛布団：**视频看这里👉https://www.bilibili.com/video/BV1H24y187Ko/

+ **领航员未鸟：**视频看这里👉https://www.bilibili.com/video/BV1Eb411f7gX/

请大家尊重up主的劳动成果，去给个三连哈，下面以第一个视频的整合包为例说明，最新的更新下载在这里：https://pan.baidu.com/s/1KYmibCqhm3cfZKa1ZmLBYA?pwd=8wp4

### 使用模型

> 如果只想使用别人训练好的模型尝鲜，那么只看这部分就可以了。

#### 原始声音处理

🏅目标：准备一段高品质且清晰的演唱音频，并分离出背景音乐和人声

> 安装UVR5

+ 安装软件：`UVR_v5.5.0_setup.exe` 安装文件较大，耐心等待，下载后一直“下一步”安装

+ 复制 `UVR5模型.zip` 模型文件到这里：`Ultimate Vocal Remover\models`

+ 需要安装的东西都在这个文件夹下面

![](img/5b46d46ea971571438f175e722c132f2.png)

+ 在处理之前你需要把你声音的格式转换成`WAV`格式，文件名只使用英文数字或者下划线，因为`So-VITS-SVC 4.0`只认`WAV`格式的音频文件，现在处理了后面会省事点。可以用这个工具处理：https://www.aconvert.com/cn/audio/mp4-to-wav/

+ 处理完音频文件后我们就要开始利用UVR去掉背景音了，一共需要过两次，每次的设置都是不同的，这样能最大限度的保证不需要的声音能被去除干净

| 第一次参数设置 | 第二次参数设置 | |

| --- | --- | --- |

| ![](img/5f262c4c76eab47b66f4afb5d0184ed6.png) | ![](img/476039dec98bc9d6c792947a70db988c.png) | ![](img/e016ca0191e5ee0a069b1387440fa9d8.png) |

+ 在`Select Input`选择你需要处理的音频文件，处理完成后可以在`Output`的文件夹下面找到处理完成的文件

+ 后缀有(`Vocals`)就是人声，后缀为(`Instrumental`)就是伴奏

+ 伴奏先不要删，我们后面合成的时候还需要

> 如果你愿意尝试在线服务（简单免费额度够了，批量生产就要付费了）也可以试试这个，达到这一步类似的效果：https://www.lalal.ai/

#### 推理替换音色

🏅目标：将第一步分离出来的人声通过工具，基于加载模型推理替换音色，生成推理后干声音频

> 安装整合包和测试模型

+ 下载整合包：`so-vits-svc.zip` 文件比较大，可以挂着慢慢下，下载好后解压到有空间的文件夹

+ 上面我分享的共享目录文件是5.8左右的，快速上手够了，如果想要用最新的，可以下载原作者最新版 https://docs.qq.com/doc/DUWdxS1ZaV29vZnlV

+ 将测试模型拷贝到特定目录下TODO？

+ 需要解压的压缩包在这个文件夹下面

![](img/e8ffd63b2d3fd5a0f3562a6b3799d9f5.png)

+ TODO：复制测试模型？

+ 启动 `webui.bat` 并使用`推理Tab`功能，选择模型与配置文件，加载模型，TODO换图，图糊了

| 进入推理功能 | 选择模型和配置文件 | 加载模型 |

| --- | --- | --- |

| ![](img/377708e90f5e7fcffdb543bc232ec2cf.png) | ![](img/6927e831e0d4f191351e32a2ba65fb53.png) | ![](img/d7f56529336c92105c3c0c91db096ab1.png) |

+ 加载模型后，勾选【`聚类f0`】【`F0均值滤波`】两个选项，并通过音频转换生成音乐

+ 【`聚类f0`】：会让输出效果更好，但是如果你的文件是歌声的话不要勾选这个选项，不然会疯狂跑调

+ 【`F0均值滤波`】：主要解决哑音问题，如果你输出的内容有比较明显的哑音的话可以勾选尝试一下，这个选项歌声可以使用

TODO换图，图糊了

| 导入第一步的干声`wav`文件 | 设置这两个选项 | 转换音频文件 | 试听下效果 |

| --- | --- | --- | --- |

| ![](img/1ca5571cd1bbd8580a980c47e86e2e52.png) | ![](img/45cdc256f18ebf2b84d0ccbc701b87a3.png) | ![](img/69f3a3ebfb52026ad7b47b605c8ccb.png) | ![](img/2fbad855de8430d3b0f3f6446045d31c.png) |

> 如果你用的其他人的模型的话你需要先把模型文件放进整合包对应的文件夹下面：

+ 首先是`GAN`模型和`Kmeans`模型就是模型文件夹下面后缀为`pth`和`pt`的两个文件放到整合包的`\logs\44k`文件夹下

+ 之后是配置文件，就是你下载下来的模型文件里那个叫`config.json`的json文件，放到整合包的`\configs`文件夹下面

#### 音轨合并

🏅目标：将第一步分离出来的背景音和第二步推理出来的干声合成成音乐

> 安装剪映或者必剪桌面版

+ 剪映下载：https://www.capcut.cn/

+ 必剪下载：https://bcut.bilibili.cn/

我们现在生成的是一段只有人声的干声，这时候我们第一步剥离出来的伴奏就有用了，把两段音频合成就行，比如使用剪映，直接把两段音轨拖进去导出就行，也可以加张图片变成视频

![](img/8ab76f236702274caffa475bbdb238ba.png)

### 训练模型

> 如果你希望训练个自己音色的模型，并用自己的模型来翻唱歌曲，可以看这部分，说明注意点

+ 首先模型的训练对显卡要求还挺高，小于6G内存会有各种各样的问题，不然太麻烦了不建议

+ 其次在训练的时候最少要准备30分钟以上的人声素材，一般一个小时到两个小时最好

+ 最后声音的质量大于时间长度，不要为了凑数搞一些质量不那么好的素材

#### 干声数据准备

🏅目标：准备质量比较高的声音，并对声音进行格式转换、半轴分离、文件分割等处理收集足够训练数据

> 安装声音文件处理软件

+ 格式工厂（可选）：http://formatfactory.org/CN/download.html

+ 解压安装 `slicer-gui.zip`

![](img/8f9143d0698e8074c5ce33af7bb8d967.png)

+ 先把我们的素材转换为`WAV`格式，批量转换的话还是用格式工厂之类的本地软件比较快

+ 获取[到我们个](https://seigy6zzam.feishu.cn/wiki/wikcnMvF3oMpHMdajHJ3svCNthd#GsGcdUoAioK4IYxQJndcm2YKnUg)`WAV`格式素材之后，继续去掉我们素材的伴奏以及混响之类的声音，只留下单纯的人声，具体的操作跟“使用模型”的第一部分一样，这里不赘述

+ 对处理后的干声数据，通过使用 `Audio Slicer` 进行更细的分段，方便后续的模型训练，因为如果训练的时候每段文件过长的话容易爆显存

+ 经过 `Audio Slicer` 处理完成后在输出文件夹把文件从大到小排序，看一下最大的文件时多长的，分割完的素材每一段尽量不要超过15秒。不然有可能会爆显存，如果打了可以调整下下面的可选参数

| 干声存放到一个目录下 | 素材拖到任务列表和输出目录 | 调整默认分割大小参数（可选） | 数据放到一个文件夹，文件夹放到 `so-vits-svc` 指定目录 |

| --- | --- | --- | --- |

| ![](img/82be5e8194d324bfc704cae0c11daacc.png) | ![](img/782d7d83b24ecd531457c5119ea2e2ee.png) | ![](img/49171d484f115fbef23dc7f2a95fd46b.png) | ![](img/b5d91a4ff45b0ad49ab32857c871b784.png) |

#### 训练自己的模型

[🏅目标](https://seigy6zzam.feishu.cn/wiki/wikcnMvF3oMpHMdajHJ3svCNthd#XgikdUiaaoCMisxrm6vc5FsXnHk)：基于处理过的干声数据，训练一个可以在上面“使用模型”的第二步使用的模型

> 无需安装新的软件

+ 解压安装 `slicer-gui.zip` ，如果之前做过了可以忽略

> 注意这里有个大坑，可能会折腾好久，你需要看一下你数据集里面有多少条数据，如果有几百条的的话，你需要把虚拟内存调大点，至于如何调整虚拟内存，这个百度就行，有很多教程

> 给个参考我的数据集是300多条，比如把虚拟内存调到100G才能保证顺利处理完，不会中途蓝屏

+ 启动 `webui.bat` 并切换到`训练Tab`下面。然后点击识别数据集，这时候上面就会展示你数据集文件夹的名字，也会是你模型的名字，TODO换图，图糊了

+ 特别说明“设置参数”的部分：

+ 【每隔多少步(`steps`)验证并保存一次模型】：这里默认的800步也就够了，他的意思是每训练800步就会保存一次模型，这个保存的模型你是可以用的

+ 【仅保留最新的`X`个模型】：这个就是字面意思如果每800步保存一次模型的话，你训练到8800的时候第800步的模型就会被自动删除，一个模型大概有1G左右这里看你的硬盘。如果设置成0的话就永远不会自动删除

+ 【批量大小】：这个参数跟你的显卡的显存有关，6G建议是4，比如4070Ti是12G设置的8，其实12也行，主要怕爆显存

| 进入训练Tab识别数据集 | 数据预处理看进度 | 设置参数 | 写入配置文件 | 开始训练 |

| --- | --- | --- | --- | --- |

| ![](img/489cf147138491aed22f70fdde4261ee.png) | ![](img/8f087ac94df9bb1c3aca68c2b0173530.png) | ![](img/3c5dfa02318e847e0e8b2faaa7366c3b.png) | ![](img/bac9d51b09342b59664745989603b85a.png) | ![](img/bd10f430c5e9823eedc1a3ed16370c50.png) |

在你点击按钮之后会弹出这样一个弹窗里面就是训练进度，我框起来的地方就是每200步输出的信息，那个`loss`的值就是判断模型质量好坏的标准，越低越好，如果你觉得现在的已经可以了的话按`CTAL+C`键就会停止训练，你可以去推理`tab`下尝试你的模型，如果不满意还是可以重新回来训练的

![](img/0dd4860c48305c7711cdadc4a4fc86bc.png)

注意你如果设置的每800步保存的话起码要到800才能暂停训练，不然没有保存的模型供你使用。下面这个图就是模型已经保存的提示。

![](img/048650b2bc0a3c77fdc7d9fbb79861ef.png)

如果你觉得可以了暂停训练之后回到推理 `Tab` 就能看到你刚才训练的模型了，可能会有好几个因为你选的最多保留十个，可以对比试验下效果，选择最好的一个

![](img/1a53ea45bf12fd641d80a576662feeeb.png)

#### 音轨合并

跟上面“使用模型”的[第三步](https://seigy6zzam.feishu.cn/wiki/wikcnMvF3oMpHMdajHJ3svCNthd#VEaodkiqWowwiexyyKHc5KdCnAe)一样，这里就不赘述了

## 参考资料

+ [https://mp.weixin.qq.com/s/GH](https://wx.qq.com/cgi-bin/mmwebwx-bin/webwxcheckurl?requrl=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FGHFBGLIkDBbiprux-BFyWA&skey=%40crypt_47f9d929_34a87e46b8489220968eb3ba0a6ffcdf&deviceid=e745934123265811&pass_ticket=C%252B%252BVYhuSH8%252FqWeDutkXVRhaAouBPmskEnzJl8seMR0soAHfYb2YLfSBtbfHdZQWb&opcode=2&scene=1&username=@a80a18c04d12d629f2a50e0396d014f617af166825dfc20a9105d8a940aad5ea)FBGLIkDBbiprux-BFyWA

+ https://mp.weixin.qq.com/s/hVH0wsHFmJOe4onBsE7JjQ

+ [https://mp.weixin.qq.com/s/bX](https://wx.qq.com/cgi-bin/mmwebwx-bin/webwxcheckurl?requrl=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FbXD1u6ysYkTEamt-PYI1RA&skey=%40crypt_47f9d929_34a87e46b8489220968eb3ba0a6ffcdf&deviceid=e745934123265811&pass_ticket=C%252B%252BVYhuSH8%252FqWeDutkXVRhaAouBPmskEnzJl8seMR0soAHfYb2YLfSBtbfHdZQWb&opcode=2&scene=1&username=@a80a18c04d12d629f2a50e0396d014f617af166825dfc20a9105d8a940aad5ea)D1u6ysYkTEamt-PYI1RA

+ [https://mp.weixin.qq.com/s/Ie](https://wx.qq.com/cgi-bin/mmwebwx-bin/webwxcheckurl?requrl=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FIeeW1PbMUbxMlLl529JTYQ&skey=%40crypt_47f9d929_34a87e46b8489220968eb3ba0a6ffcdf&deviceid=e745934123265811&pass_ticket=C%252B%252BVYhuSH8%252FqWeDutkXVRhaAouBPmskEnzJl8seMR0soAHfYb2YLfSBtbfHdZQWb&opcode=2&scene=1&username=@a80a18c04d12d629f2a50e0396d014f617af166825dfc20a9105d8a940aad5ea)eW1PbMUbxMlLl529JTYQ
