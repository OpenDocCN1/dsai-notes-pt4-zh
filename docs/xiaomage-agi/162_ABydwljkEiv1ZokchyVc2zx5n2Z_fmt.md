
# 生成视频

> 来源：[https://xmgtoagi.feishu.cn/docx/HzNcdY57tonAfbxoylec9TDmn0g](https://xmgtoagi.feishu.cn/docx/HzNcdY57tonAfbxoylec9TDmn0g)

## 注册账号

平台链接：`https://runwayml.com/` 进入平台后点击`Try RUNWAY FOR FREE`可进入使用界面

![](img/d90b0e9e26bd853307729b680d25b6d0.png)

![](img/f8be97af7496ab04193fcd3310c1fc1b.png)

该平台可免费使用

## Gen1: Video to Video

该工具是使用文本或者图片作为`Prompt`，使用视频生成新的视频

进入`Runway`工具界面后，点击左侧菜单栏的`Assets`，可以上传自己电脑上的素材，可上传视频、图片、文件夹。上传文件夹会把目标文件夹中含有的所有素材都上传至`Runway`

![](img/b8a21be927ea3b363708fc7afdc0271c.png)

点击左侧菜单栏的`AI Magic Tools`，选择`Gen1: Video to Video`

![](img/1adcca984341ee8f0e901bde697931d5.png)

进入`Gen1`工具后页面如下，此时我们需要上传一个视频素材所谓基本模板。

![](img/028ad2155f222eb9f724f9edcef2fac6.png)

此时我们选择平台自带的`demo`素材中的一个视频实验

![](img/7cc50553cc80bf9ed67ee8a7fb93db12.png)

此工具右上角中为视频风格迁移的`Prompt`，可使用图片、文字作为`Prompt`同时也可以使用系统自带预设。

![](img/6a1d7ffa56a8cb77914bf1057cff68a0.png)

我们使用图片作为`Prompt`，此时我们上传了一张图片，这中图片的风格很抽象。选中图片后点击`Generate`按钮后视频开始生成。

![](img/02882fa34dc87cc41e7e2af20562f3e4.png)

![](img/7a7fb7828f87d4404e8732644c852578.png)

经过一段时间的等候后，我们可以看到生成的视频，原本的视频的画风将会被我们选中的图片`prompt`的画风替代。

![](img/5798cfaa29fa61ea2caa23835ae9a5c1.png)

### 高级设置

在右侧菜单栏的`Settings`中，点击`Advanced`可调整更多设置

![](img/0dc735fc920acdf631354e44443cb7f8.png)

![](img/e123a504d2195d236262b767eeb08b4a.png)

#### structural consistency

`structural consistency`：更高的数值使输出的视频在结构上与输入的视频有更大的差异。推荐使用： 0到5。下图展示该属性不同值生成的结果。

![](img/a67f71134f53f69c57ae3fcc6d4480eb.png)

#### weight

`weight`： 更高的值会导致更多强调匹配风格而不是输入视频。推荐使用： 7.5至12.5。下图展示该属性不同值生成的结果。

![](img/2390eee8b5d1dd21409bae633437e187.png)

#### Frame Consistency

`Frame Consistency`： 低于1的值会降低整个时间段的一致性；高于1的值会增加框架与先前框架的紧密联系。建议： 1.0到1.25。

![](img/98df46e3c8d8e49a8de1c14df9afb57f.png)

#### Affect foreground only

`Affect foreground only`：选择后将只影响前景主体，而不影响背景。

![](img/f0070fdbdb1d9edfaa8436e1750af02f.png)

#### Affect background only

`Affect background only`：选择后将只影响背景，而不影响前景主体。

![](img/8facd8d5d729bde1df9b29278750a459.png)

#### Compare wipe

`Compare wipe`：选择后在原视频和新视频之间进行切换。

![](img/69cfcf3234698cdb8348ca96e10e1a05.png)
