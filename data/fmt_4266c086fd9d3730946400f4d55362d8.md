
# 网页仿写

> 来源：[`https://deepwisdom.feishu.cn/docx/KwCjdY5wvoprHpxUQ6iczt76nbe`](https://deepwisdom.feishu.cn/docx/KwCjdY5wvoprHpxUQ6iczt76nbe)

## 概述

网页仿写涉及创建现有网页的副本或类似版本，适用于多种用途，如功能性测试、视觉设计比较，以及作为教授网页设计和开发的辅助工具。仿写过程涉及目标网页的布局、风格和内容

## 示例：使用GPT-4V仿写网页

### 任务

给定一个网页的URL或图片，使用MetaGPT内部集成GPT-4 Vision的工具`GPTvGenerator`仿写出一个相似的网页

### 代码

```
python examples/di/imitate_webpage.py
```

### 运行结果

原网页截图：

![](img/8dd4165929de3c60c633bcb947f36b19.png)

仿写后的网页：

![](img/246e00cf455efc2dff2e263544eedbc5.png)

## 机制解释

1.  `interpreter`规划任务`Plan`时，生成了若干个任务，根据所有注册工具的`docstring`分配任务的工具类型。`interpreter`自动将生成网页的中间任务类型设置为`image2webpage`，在执行任务时将扫描是否有`GPTvGenerator`相关工具可用。

2.  发现可用工具：`['GPTvGenerator']`，加载工具代码，根据当前需求使用`generate_webpages`方法生成相关的前端代码，并使用`save_webpages`方法保存。
