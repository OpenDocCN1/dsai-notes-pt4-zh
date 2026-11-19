
# Task3：进阶 `baseline2`【微调方向】 + 知识点讲解

> 来源：[`https://datawhaler.feishu.cn/docx/ZopidaBFRoHg6exsCqJcGVVKn9d`](https://datawhaler.feishu.cn/docx/ZopidaBFRoHg6exsCqJcGVVKn9d)

> `教程贡献者说`：

> 看到这里相信你已经跃跃欲试，想挑战更多更好的方案了。

> 通过这个task的学习你可以得到一些别样的思路（正好也能学学讯飞的微调平台怎么用）。

> 本节主要介绍了一个对原始数据做基础抽取后，通过讯飞微调平台微调抽取大模型的方案。

> 多个方法综合在一起实现抽取任务，这里尽可能把每一步都写清楚，也请大家耐心看完。

> 预计需要半天到一天的时间来完成这个任务，也许你可以准备一个慵懒的下午开始算法大神之路（大部分时间在等待模型推理与训练~）。

> 更新日志：2024.06.28 更新 V2版本`baseline2` 对先前版本推理部分做更新，推理效果更好。

## 数据集制作

详见`baseline2`的第一部分（点击即可跳转）

> 链接：`https://aistudio.baidu.com/projectdetail/8090135`

## 平台微调

### 训练数据上传

+   [登录微调平台（点击可跳转）](`https://training.xfyun.cn/overview`)

> 链接：`https://training.xfyun.cn/overview`

+   上传第一步制作的数据集

![](img/511a31a5bcf58e002dcbb6d40ae6966f.png)

![](img/f4f4d4297c27e338c142f7f70dfbd95c.png)

![](img/99fa6eb5e66ea3fb1e8a95acc3decc13.png)

+   点击确定，等待上传成功

+   运行成功训练表示数据上传结束

![](img/28c1ab0c525f279301a4ccc5dece202b.png)

### 测试数据上传

回到：`https://training.xfyun.cn/dataset/datasetIndex`

这次我们选择测试集即可。

![](img/c5664c901a02ef6b8f9af03ce925b128.png)

上传我们的`test.csv`文件即可。

![](img/1f4320dae17a9c68a3e95f6fdf86ee69.png)

![](img/eb1412766f19ffefacde60b2ce161958.png)

到这里我们数据集就传好啦~

### 微调配置

进入创建微调页面：`https://training.xfyun.cn/model/add`

基本配置与版本配置如下，我们选择性能比较好的`Spark Pro`模型~

![](img/17e626f20b80f3bf07b72fb6187c84bb.png)

数据配置与参数配置，选择代金券，通义协议，提交训练。

![](img/0f0b514806f4c5e005944f4bdcdecccf.png)

![](img/8667d91620c1406f25fceb3ca8008fe1.png)

## 结果处理

训练完成后，进入模型页面。

![](img/51b9323247dc68651e041eac6df00a0f.png)

点击右侧，发布为服务

![](img/beecf2b5dc0fd9a13543d89199c05de6.png)

点击左侧导航栏的`我的模型服务`接着拿到`resourceId`、`APPID`、`APIKey`、`APISecret`

![](img/55575d6b6881428e390662ce416cb2d7.png)

接下来到`baseline2`中继续执行~

`https://aistudio.baidu.com/projectdetail/8090135`

在`main.ipynb`的微调推理部分填入`APPID`、`APIKey`、`APISecret`（注意顺序）

![](img/e950303dbdecec5d3149075c22bd41ec.png)

在`SparkApi.py`文件的108行，引号中填入你的`resourceId`

![](img/a7b18d048735f3622bd195c2aa535a9f.png)

## 提交文件

快去提交结果吧

`https://challenge.xfyun.cn/h5/detail?type=role-element-extraction&ch=dw24_y0SCtd`

大概就是这样的分数，这只是一个很简单的版本。

![](img/a7d27994cfb22bb71bdb4ae8b2d7a733.png)

> 本教程主要由   、 写作贡献，经过多位助教内测和建议修改，期待你的点赞和评论～
