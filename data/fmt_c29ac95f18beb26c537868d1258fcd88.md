
# 2023-08-14 AI 绘画 Stable Diffusion | Prompt 人物描述篇

> 来源：[https://xuqiwei1986.feishu.cn/docx/CoS4dT8CqoKE9zx2gIfcd9RNnig](https://xuqiwei1986.feishu.cn/docx/CoS4dT8CqoKE9zx2gIfcd9RNnig)

> 🔗 原文链接：https://mp.weixin.qq.com/s/Qhn_Ud2vS9jrbISMZnda6g

原创 杨三季 杨三季 2023-08-14 14:01 发表于 北京

手机阅读

![](img/19d08e153de8a7e8768fadc4940b3e9a.png)

> 你「Stable diffusion WebUI」进行 AI 绘画时，是不是发现人物角色的形象很难控制？即使你输入了 `1girl` 或 `1boy` 后，生成的内容依旧与你想要的提示词不同。今天作者将分享如何通过提示词稳定人物的角色形象。让你通过 SDWebUI 批量生图时，不再因此烦恼。

在上一篇分享《 Stable Diffusion 提示词高阶用法（一） 》中，作者已经介绍了在 Stable-diffusion 中，不使用任何插件或模型，仅通过提示词（Prompt）的某些高阶写法进行单角色的画面控制技巧。而在本次分享中，作者将基于上一篇的内容，介绍一下如何通过提示词对人物角色进行控制，让你在进行故事性的图文创作时，对故事角色进行更好的控制。

内容包括：年龄、体型、肤色、发色、发型、服装 等。

## 人物年龄控制

在 Stable Diffusion 中，大家对于人物年龄控制的常用方式是输入以下内容：

女孩：`1girl`，

男孩：`1boy`，

婴儿：`1baby`，

成熟男性：`1mature male`，

成熟女性：`1mature female`

但在一些特殊场景中，以上的方式就很难生出你想要的人物年龄。这时候应该怎么办呢？作者将通过“注释”的方式介绍一下如何固定角色年龄（“注释”用法介绍请看：提示词高阶用法：注释）

### 青少年角色

作者首选通过设置角色年龄来进行区分：

1 个 10 岁的女孩：`1girl\(10year old)`，

![](img/dc811dbd858eabe4c142654e0d8bddf1.png)

1 个 10 岁的男孩：`1boy\(10year old)`，

在固定使用男孩或女孩后，不停的更换其年龄值，可以发现：

+ 女孩：脸部特点在低于 10 岁或超过 30 岁时，并不会随着年龄的改变有明显变化；

+ 男孩：脸部特点在低于 10 岁以及 30~60 时，并不会随着年龄的改变有明显变化；

因此女孩（`1girl`）& 男孩（`1boy`），适合用于青少年（18~35 岁）的角色定位。

### 儿童角色

那如何进行 3 岁左右的幼童或者十二三岁的小孩子的描述呢？这里就要参考英文中对不同年龄段的人群的单词定义：

> `toddler`：蹒跚学步的孩子，一般指 1-3 岁。`teenager`：青少年对应 13-19（即 thirteen - nineteen），简称为 `teens`。

![](img/584a76de3842605cd8696b13b440841c.png)

可以发现：

+ 在使用 `toddler` 和 `teenager` 后，分别得出了很形象的幼童和少年的人物形象；

+ 在作者实验中发现，如果未添加年龄的描述和性别的描述，得出的画面会存在一定随机性；

因此最终作者得到的关于幼童和少年的提示词：

幼女：`1toddler\(1girl, 1year old)`

幼童：`1toddler\(1boy, 1year old)`

少女：`1teenager\(1girl, 13year old)`

少年：`1teenager\(1boy, 13year old)`

### 中老年角色

现在我们看看关于中年和老年，我们应该如何定义。

![](img/237e4a2e3ec49b1ab9778b1cacc8086e.png)
