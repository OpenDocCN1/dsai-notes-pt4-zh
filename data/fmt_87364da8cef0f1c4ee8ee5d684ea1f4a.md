
# 基于 Sovits 的声音训练及推理

> 来源：[https://xmgtoagi.feishu.cn/docx/OavbdhKEcoIvfOxqAjjcXui5nyb](https://xmgtoagi.feishu.cn/docx/OavbdhKEcoIvfOxqAjjcXui5nyb)

## 👋 欢迎来到 SVC 的世界！

Sovits 是一个音声转换的项目，可以将已有的歌声或者语音中的声线换成你想要的声线，从而达到“翻唱”的效果。

原作者是 [bilibili@羽毛布团](https://space.bilibili.com/3493141443250876?spm_id_from=333.1007.0.0) 的 So-VITS-SVC 一站式整合包的用户手册。

本文结合原文，并添加了关键步骤的截图，方便大家学习。如有问题，可以查看原文，有更专业的讲解。

原文地址：https://www.yuque.com/umoubuton/ueupp5

视频教程地址：https://www.bilibili.com/video/BV1H24y187Ko/?spm_id_from=333.788&vd_source=35e62d366d8173e12669705f7aedd122

结合另一个视频观看：https://www.bilibili.com/video/BV1ea4y1G7gx/?spm_id_from=333.788.recommend_more_video.-1&vd_source=35e62d366d8173e12669705f7aedd122

以下是教程正文：

## 1. 完整整合包（v2.3.4）下载

[百度网盘下载](https://pan.baidu.com/s/12u_LDyb5KSOfvjJ9LVwCIQ?pwd=g8n4)

提取码：g8n4

解压后如下图：

![](img/85f095e25ab4cbbff29ce658761d07dd.png)

## 2. 下载底模 DLC

预训练模型（底模）可以大大减少小数据集的训练难度。整合包内已经自带了底模，并会在训练时自动加载。但是目前 Vec768l12 编码器有一个更强的底模。你可以自行下载并替换原来的底模。

下载地址：

[ms903/sovits4.0-768vec-layer12](https://huggingface.co/datasets/ms903/sovits4.0-768vec-layer12/tree/main/sovits_768l12_pre_large_320k)

![](img/59aa627f7959efc7de7fb722b5bddd12.png)

1 将下载的底模分别改名为 `G_0.pth` 和 `D_0.pth`

这里注意源文件名中的 D 和 G，不要搞错了

2 替换到 `pre_trained_model/768l12` 目录下同名文件，

![](img/d50c9d802c7d61d17017b10ae71e61f4.png)

## 3. 点击 启动 `webui.bat`

等待运行

![](img/62786c4f8b5d0ceb648c703464f06343.png)

程序会自动打开浏览器并展示工作页面

![](img/f7d35d2ed2db6e11c7e8871e8bcc3868.png)

注：推理即转换的意思。新下载的整合包是不带任何模型的，所以无法立即体验，所以需要先进行训练

## 4. 训练

点开训练选项卡

![](img/385edad23d91aa45303d34f61c26ba3a.png)

在整合包的界面里，对训练和推理起到明显作用的参数，都做了解释和说明，无需特地去查询和搜索，非常友好。

训练的步骤大致如下：

1. 准备数据集
2. 识别数据集
3. 数据预处理
4. 写入配置文件
5. 从头开始训练

以下配合截图分别说明

### 4.1 准备数据集

目标说话人的授权数据集（至少 30 分钟的纯人声 / 歌声，1-2 小时最佳），这一步预计耗时 1 个工作日。

如果你想训练一个孙燕姿的音色，你需要收集孙燕姿的声音数据，比如歌声、访谈、演讲等等。

可以从一些音乐网站下载高清音乐，考虑到推理完成后，很多人还是需要合成 mv 的画面，这里提供一个更简单的下载方式，即 b 站视频。我们使用“唧唧 down”软件 http://client.jijidown.com/

这是一个用于下载 bilibili 视频的 PC 应用程序，它能够下载 99% bilibili 视频（不支持番剧类型），并且它将会终身免费提供使用。现在它已经支持弹幕，杜比视界/杜比全景声，真彩 HDR，超高清 8K，超清 4K，AVC / HEVC / AV1 编码，互动视频，mp3 音频，up 主投稿，up 主合集和列表，个人收藏夹以及批量下载

安装的时候注意把其他软件的小勾勾去掉

![](img/5209542198b1883727eb5849129411df.png)

安装完成，可以登录 b 站账号，没有的话可以不登陆

![](img/462af43dd874f5e609961f4c63fc1179.png)

打开一个你想下载的视频，复制地址即可自动识别，然后点击批量下载

![](img/1148e983d48e5302c0b0bd0b72eb3a7b.png)

![](img/b37bfad869eccfb9a572b43419b845c6.png)

下载下来的是视频，下一步提取声音

这里使用的是 urv5 软件提取

b 站教程：https://www.bilibili.com/video/BV1ga411S7gP/?spm_id_from=333.788.video.desc.click&vd_source=35e62d366d8173e12669705f7aedd122

Github 链接：https://github.com/Anjok07/ultimatevocalremovergui/

百度网盘：https://pan.baidu.com/s/14iK32JKIPvjmf1Kfq21mzg?pwd=hjhj 提取码：hjhj

![](img/05864e48e5eb46cb79f12e0c81bf136a.png)

这里的模型一定要下载，软件本体只预装了部分模型，模型的安装比较简单，拷贝就行，详情请看网盘里的说明。

界面是这个样子，选择好输入输出路径，选择模型，提取人声的最佳模型是 mdx-net，然后选择 uvr-mdx-netmain 提取伴奏用 VR，但是训练不需要伴奏，可以忽略这一步，只是顺便提一下，后期合成 mv 的话，是需要推理后的声音和伴奏剪辑到一起的。

![](img/690475b1199ac617ac68e92c901615ba.png)

![](img/c445750b8b9896729ec34078ff416412.png)

提取出来的声音一般来说还需要精细的处理，去掉杂音、和声和混响，达到俗称“干声”的效果

这里介绍 2 个软件来处理声音

iZotope RX，用来去掉混响和杂音

链接：https://pan.baidu.com/s/1NX-h67SViKm39zT08U7-zg?pwd=kmhd

提取码：kmhd

安装的时候，记得把 vst3 和 aax 勾上，否则后面找不到对应的文件夹

![](img/2b309c75265b3b7d8c4119d3f1485e8a.png)

主程序安装完成后，替换补丁，是直接复制粘贴，不需要双击运行

![](img/73ebd6411f60965365182f7dc34be1f2.png)

安装完成后，界面如下，打开你提取的人声，左下角的控件移动到最左边，让图形更加清爽（这一步对声音没有任何处理，仅是调整视图）

去混响：点击顺序如图中箭头所示，通过增加减少和伪影平滑的数值来调整效果，我这里没有标准数值，以耳朵为准，点击预听感受效果，如果效果合适，点击渲染，应用到整首歌。

![](img/7574ce9864dc9791431969ddde30c771.png)

去杂音：这里的杂音是指非主人公的声音，比如经常出现的掌声，笑声，欢呼声，如果这种杂音只出现一次，那么选中对应的音轨，右键-渲染-静音即可，如果是重复出现，并且有一定的相似性，那么可以选中这一段音轨，然后查找类似，然后右键-静音

![](img/ff32662971a76b31e42ed00f59f9348c.png)

如果杂音和主音柔和在一起，无法智能提取，一般有 2 个思路，一个是，直接去掉这段主音，因为我们需要收集的声音量很多，去掉一两句影响不大，如果精益求精，可以用下面这个软件精修

ripx，对声音进行精修（也可以用来提取音轨，伴奏等等，需要动手能力强）

链接：https://pan.baidu.com/s/1ClBvqlnA1cONVs8YU-ldcw?pwd=5mrs

提取码：5mrs

![](img/f9d12f8ef9c5a4b28150d602cd59d010.png)

这个软件的交互体验非常优秀，基本不需要什么教程，左右键点一点音轨图形就知道怎么做了，把杂音的音轨删除，主音的音轨可以剪切，然后导出即可。

所有声音导出成 wav 格式，最终我们处理的干声大概是以下标准，请对比感受

![](img/fad28e4b2901b176a6dc1cef9890b6cb.png)

[赐我测试用.wav](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MBvebU7iEoJK8yxkf9ic7d0xnXc)

![](img/6feb2f4950c664233922a9b00c932a5c.png)

[小星星.wav](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HaRdb0r3uoHNSRxL7Lecq0cQnIe)

![](img/cbe5cf9bf426219a2b58774ccc10dd53.png)

[泡沫干音测试素材.wav](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Mlb8bRcmhofyrox1ZdCcee7Unkg)

![](img/d6c8a860f46a34e34830bac44459fbc8.png)

[富士山下测试干声.wav](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BNkZbSFr6o1wL7xGMgUcYNTWnMc)

目前我们是按照一首歌为单位提取的声音，接下来我们需要用到切片工具，将歌曲切成片段，方便训练时的投喂。

我们使用 Audio Slicer：

Github 链接：https://github.com/flutydeer/audio-slicer/blob/main/README.zh-CN.md

网盘链接：https://henji.lanzout.com/iuSOk0uv354j

操作比较简单，不再赘述，参数默认即可

![](img/e868dfd55b502b39ff099b36bba3c91b.png)

1 个小时左右的数据，大概能切成 6-700 片，每个片段的最佳长度是 3-15 秒

最好将声音都听一遍，把奇怪的声音删除。数据集的质量比数量更重要！

大概率文件名如下：

![](img/882b2e9202171f69147f2a014f9c12a4.png)

这样的文件名很容易无法被识别，文件名不要包含 _ 括号等，所以我们需要用到批量改名工具，全部改成数字是最保险的：

批量重命名程序：

Download Advanced Renamer 3.88 for Windows

https://advancedrenamer.com/download

打开软件，批量加入文件-加入专案-新名称-数字编码递增--可以看到在列表中修改后的效果，然后右上角 start batch

![](img/1a272af8f16996f701e988751508655c.png)

更名完成：

![](img/fbde465486b35949380cfcc152b8dca4.png)

### 4.2 识别数据集

将声音文件夹改名，例如 `sunjie1`，拷贝至 `so-vits-svc\dataset_raw` 下，如图，每次训练只放一个文件夹，避免出错

![](img/0be12098d21c7f461460527cd7d6ceb5.png)

### 4.3 数据预处理

回到 webui 界面，识别数据集：

![](img/9b2259d45468f606d14339707f865751.png)

参数选择默认就行。

点击数据预处理，一般来说没啥报错，我遇到的报错都是文件名的错误，看左下角的进度条，100%就 ok 了，1 个多小时的数据集，大约需要 5-10 分钟，看显卡

![](img/ac08c0478060822780b64017ecc82e42.png)

### 4.4 写入配置文件

进度条走完之后，这里的说话人列表会显示对应的名称：`sunjie1:0`

![](img/52464ae581bcccbb4bfe6b3efcf8ca20.png)

这里可以调整的是

批量大小（`batch_size`），默认 48，我 12g 现存，填的是 72

仅保留最新的 X 个模型，超出该数字的旧模型会被删除。设置为 0 则永不删除，默认是 10，如果你是晚上挂机训练，建议改成 0，早上起来后，把不要的模型删掉就行。现在存储又不贵。因为并不是最后一份模型就是最优秀的，整体来说是震荡上行。3060 12g 的显卡，挂机 8 小时，大约可以训练 2 万多步，但是有可能训练日志显示 14000 步的效果最好，如果不全部保留的话，可能就被清理掉了。

点击写入配置文件

### 4.5 从头开始训练

配置文件写入完成后，点击从头开始训练，运行界面可以看到训练过程，此时你的显存占用应该达到 100%，所以尽量不要使用电脑处理其他事务。

在训练日志中，主要看该模型的 `reference_loss` 的数值，越低越好，目前我的经历中最低的有 24.xxxxxxxx，一般来说 2 万步以上效果提升的不太明显。

Losses 详解

你不需要理解每一个 loss 的具体含义，大致来说：

●`loss/g/f0`、`loss/g/mel` 和 `loss/g/total` 应当是震荡下降的，并最终收敛在某个值

●`loss/g/kl` 应当是低位震荡的

●`loss/g/fm` 应当在训练的中期持续上升，并在后期放缓上升趋势甚至开始下降

观察 losses 曲线的趋势可以帮助你判断模型的训练状态。但 losses 并不能作为判断模型训练状态的唯一参考，甚至它的参考价值其实并不大，你仍需要通过自己的耳朵来判断模型是否训练好了。

对于小数据集（30 分钟甚至更小），在加载底模的情况下，不建议训练过久，这样是为了尽可能利用底模的优势。数千步甚至数百步就能有最好的结果。

当你觉得数值差不多的时候，在运行界面点击 `ctrl c`，训练就会暂停（无需做其他的动作，此时切回到推理界面，就可以选择刚刚练好的模型进行推理试听了）

![](img/73dc9733415f29ea675364c0a1d68466.png)

如果这是你的第一炉模型，请打开 `so-vits-svc\logs\44k` 文件夹，该文件夹下的文件名应该是这样：

文件名的数字代表了步数

![](img/013b84b71663f11d55e3b0fe1d46469b.png)

## 5. 推理

### 5.1 加载模型

切到推理界面，刷新选项，下拉模型选择，你会看到你刚刚保存下来的模型，例如：`G_0` 开始到 `G_11200`

这里面代表了在多少步时保存下来的模型，选择模型后，再选择配置文件，点击加载模型

![](img/0fea7a7cbced46e816311fedafd8162f.png)

这样就表示加载成功了，这里的说话人会显示模型的名称，我这里是手动改过模型的名字，如果没改的话，仍然显示 `G_XXXX`

![](img/39627dd83cbc5dbe4e8d421ff0eb212c.png)

### 5.2 上传音频

也就是想推理的对象，我想用 `sunjie1` 的声音去唱孙燕姿的歌，那么这里我就应该传一个孙燕姿的歌声 wav 文件

![](img/a3f20b6e8a0b7efb9c95d89097877d60.png)

这里待转变的音频文件，尽量使用干声，即去掉伴奏，和声和杂音，否则会推理出奇奇怪怪的声音。

优秀的输入源音频的要求是：

●纯人声，无伴奏、底噪、和声、混响等

●WAV 格式，44100 hz，16 bit

这里可以更改的参数：

变调：男声模型去翻唱女生歌曲，需要降调（这里的降调是降低你上传的音频的调子），-5 到 -8，酌情调整，女生模型翻唱男声，先升调。

![](img/dee535d191458e447a9f7535067c4464.png)

自动 `f0` 预测和预测器选择，如果是说话就勾选自动 `f0` 预测，唱歌就不勾

`f0` 预测器我个人经验 crepe 效果还不错

![](img/85682b5bf38b2821fc5f5bdbd18ceabf.png)

### 5.3 点击音频转换

![](img/9af78a83adec05e80b51ffd633a60c06.png)

稍等即可。

## 6. 继续训练

如果对效果不满意，可以在数据集的声音加入更多高质量的干声，重复第四步的识别和预处理，然后点击继续上一次训练。

也可以不增加数据集，直接继续上一步训练，通过训练时长的增加来改善效果。

如果你对当前的某个模型比较满意，那么可以保留对应的 `G` 开头的 `pth` 文件，以及 `config.json` 文件，其余的可以删除。把 `44k` 文件夹下的 `G-pth` 文件以及 `config.json` 名字改掉，这样下次加载模型的时候可以很好的识别，

![](img/5d1990417df180cdb1d5ed857b016d3e.png)
