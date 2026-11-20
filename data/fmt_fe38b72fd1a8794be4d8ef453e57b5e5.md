
# Mo平台项目发布指南-Datawhale专版

> 来源：https://datawhaler.feishu.cn/docx/NC9Zds8lGotQJhxdk8GcFdsCnHf
> 欢迎大家入驻 Mo平台~
> 最最最重要的是——发布项目的时候，标签记得填【Datawhale】哟~
> 如果与DeepSeek相关，类别也可以选择【DeepSeek】

## 平台介绍

### 进入平台
平台网址：https://momodel.cn/

进入Mo平台，注册账号后登录

### 模块介绍

1. **首页**
1. **课程**：你可以在“课程”界面进行学习。涵盖`Python`、机器学习、深度学习、深度学习进阶、实战练习、Datawhale课程、XEdu课程等多种好课。
1. **项目**：你可以在项目探索学习公开的项目，发现喜欢的项目可以点赞收藏。如果你有优质的资源我们也欢迎你与其他数据爱好者共享。
1. **数据集**：你可以在数据集寻找所需要的公开数据集，发现喜欢的数据集可以收藏。如果你有优质的资源我们也欢迎你与其他数据爱好者共享。
1. **讨论**：讨论是用户的交流区。你可以在这里发布想法、算法实现的思路、开发经验等，也可以提出自己的需求来寻找问题的解决方案。
1. **资讯抢先看**：有全球每天热点新闻及有趣的视频。
1. **AI大赛**：平台主办的AI赛事
1. **AI测评**：由浙江大学团队提供的权威人工智能评测系统，帮助企业更科学、低成本识人用人。

详细介绍可以进入帮助文档中查看：产品介绍 - Document - MO

## 二、项目

### 新建项目
点击导航栏的【项目】，展示所有公开的项目。点击【新建项目】。

![](img/26b6cde8e56539738d67d601987808b8.png)

![](img/bccd3c83fe4ce46fd630e0d1e055df78.png)

![](img/0ebad5aed8a9876d598cde7efafe5319.png)

### 编写内容
用户可以先阅读项目中的【`_README.ipynb`】文件，熟悉我们的开发环境。

![](img/ef747a1aadb0acee71af09d7a8c44b0d.png)

编写课程内容时，需要新建`ipynb`文件。

![](img/8969717968dfca3ac7b9e928f88c1040.png)

点击下方切换`cell`为`markdown`或者`Code`类型。

![](img/6778ebda1bfb0d562be625d489a7e8e7.png)

`Markdown`形式

![](img/938d38320049cc4b418943d8f137297e.png)

点击运行后呈现`markdown`效果

![](img/3d178455c60c440874a08de77fefc746.png)

`Code`形式，可以在线运行`python`代码

![](img/af7c1b1fd7a79a4ab71bd83b89ea320c.png)

### 项目公开
点击【详情页】

![](img/9c103c513814d2608c1e15b01b253d91.png)

点击【编辑项目】

![](img/5e3b18797265e3c00e220a2017ce3a94.png)

点击权限为公开，修改你的项目名称、描述、类别、主文件、添加标签，描述越清晰越容易被大家看到哦。

**注**：Datawhale小伙伴可以选择，类别为DeepSeek或其他项目类别、标签为Datawhale。

![](img/07be682b98cdd2dbfa1e4541ca37ce73.png)

## 三、课程内容编写规范

### 1. 标题

#### 1.1 标题使用三段式，类似如下：

题目是一级标题：`#小样本学习概述`

内容二级标题：`## 1. 半监督学习`（注意数字1后面需要 `.`）

内容三级标题：`### 1.1 问题设置`（注意`1.1` 后面需要一个空格）

内容四级标题：`#### 1.1.1 问题设置1`（尽量不使用，如有必要也可以使用）

每一个标题请不要在一个 `cell` 中（换句话说：一个 `cell` 不要有 2 个标题）

![](img/63565e251f97eb366cd363b7d86409f1.png)

#### 1.2 标题名称最好使用中文

（例如：`小样本学习概述`，或者`YOLO 介绍`），不要全使用英文

### 2. 内容

#### 2.1 内容中英文之间加空格、数字与中文之间加空格

#### 2.2 文件名，或者代码使用代码格式，见下图

![](img/5fc7795fd3213738e7efed4c86d5502d.png)

#### 2.3 长文字拆分成多行

例如：

![](img/2f8e36621629f5fec44a8cfc79fc2019.png)

改为：

![](img/37ab2d8b0bc9b93f9a40eb63b3f1344e.png)

### 3. 图片

#### 3.1 图片上传方法

1. 上传图片到文件夹中，复制路径

![](img/34ca444b88ab480f24170c32c7ba767a.png)

1. 点击`cell`，使用`markdown`格式编写，实际使用时路径替代括号的内容：`![](图片示例.png)`

![](img/b49e894237c64dac99c7c27e6910ef03.png)

1. 图片需要自己控制大小，尽可能更美观

#### 3.2 英文图片内的不好懂的文字加上中文的注释

#### 3.3 图片不清晰时可以google图片、或者自己亲自绘制，保证图片的清晰性

### 4. 代码

#### 4.1 代码需要清晰简洁的注释，并尽量使用中文注释

#### 4.2 函数上方加入函数、输入参数、返回值的说明

![](img/8188cd7fd45037877e77ab988052c53e.png)

#### 4.3 每个`cell`的代码尽量展示运行结果

![](img/6e9126f1eaa98f1822fa405bea7cf4f9.png)

#### 4.4 类 `class` 写到 `py` 文件里面去，然后分函数在 `notebook` 内讲解

#### 4.5 所有项目有一个`config.py` 文件，所有的路径地址都写入这个`py`文件

#### 4.6 代码块不要过长，函数和非函数、数学运算和可视化分块（写在不同的`cell`），可视化的图像调整字体保证清晰。

![](img/ca82e31f563575e000f52d838eed7b7c.png)

![](img/a36539425d89ba59778927afac228f64.png)

### 5. 课件中的问题提示与插入答案

如下图：

![](img/7bbc5ebac3443300bc9380e8fdaa6f30.png)

制作流程：

1. 打开`terminal`终端

![](img/f140f9d5bec749872b257d3e34532648.png)

1. 需要编写一个`json`文件，比如下面，同时在`cell`中`markdown`格式输入：

`<span class='md-answer-link pop 0'>问题提示</span> <span class='md-answer-link insert 0'>插入答案</span>`

（只需要更改`0`即可，表示在你`json`文件中列表的第几个）

`<span class='md-answer-link pop 0'>问题提示</span> <span class='md-answer-link insert 0'>插入答案</span>`

---

![](img/90e15488c163a028b4887ca4172575fe.png)

1. `json`文件格式如下：`vi .xxxxx(课件名).json`（注意在根目录下，`json`的文件名要和课件名一样）

```json
[
  {
    "hint": [
      "1. 使用 Sequential 模型",
      "\n",
      "2. 使用 Conv2D 创建卷积层",
      "\n",
      "3. 使用 MaxPooling2D 创建最大池化层"
    ],
    "answer": [
      "# 创建一个 cnn 模型",
      "def define_cnn_model():",
      "    model = Sequential()",
      "    # 卷积层    ",
      "    model.add(Conv2D(32, (3, 3), ",
      "                     activation='relu', ",
      "                     padding='same', ",
      "                     input_shape=(200, 200, 3)))",
      "    # 最大池化层    ",
      "    model.add(MaxPooling2D((2, 2)))",
      "    # Flatten 层",
      "    model.add(Flatten())",
      "    # 全连接层",
      "    model.add(Dense(128, ",
      "                     activation='relu'))",
      "    model.add(Dense(1, activation='sigmoid'))",
      "    ",
      "    # 编译模型",
      "    opt = SGD(lr=0.001, momentum=0.9)",
      "    model.compile(optimizer=opt, ",
      "                  loss='binary_crossentropy',",
      "                  metrics=['accuracy'])",
      "    return model"
    ]
  },
  {
    "hint": ["你填写问题提示"],
    "answer": ["你填写的插入答案"]
  }
]
```

---

1. 验证是否对应，比如上面的`json`我们输入`<span class='md-answer-link pop 0'>问题提示</span> <span class='md-answer-link insert 0'>插入答案</span>`后：

![](img/d111206b8049ea293623b82cbacceeb2.png)

![](img/91de17ea953c0d36aa3446868e86a16f.png)

### 6. 涉及到放b站视频

不能直接下载搬运，会侵权

1. 找到分享嵌入代码板块

![](img/8fc2ac841e6ab3f7c1cf430eb8e968db.png)

1. `markdown`模式下编辑

根据要求调整大小：

![](img/ad0f1df38815e750c9814a194a0bfd2a.png)

1. `<iframe src="//player.bilibili.com/player.html?isOutside=true&aid=114213448058562&bvid=BV1hbozY1Eje&cid=29036056479&p=1" width="1000px" height="800px" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>`
