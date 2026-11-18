
# Task1：跑通`YOLO`方案`baseline`！

> 来源：[https://datawhaler.feishu.cn/docx/PuITdwSpHoe0Qdxv5ubc5IFJneh](https://datawhaler.feishu.cn/docx/PuITdwSpHoe0Qdxv5ubc5IFJneh)

> 本赛题最终目标是开发一套智能识别系统，用于自动检测和分类城市管理中的违规行为。

> **学习内容提要**：（此处需用一句话概述学习内容，如：大模型技术方案精读、进阶与优化，涉及赛题数据处理、多`agent`系统方案，再到微调大模型方案，会简单介绍 `机器学习` 解决赛题的思路）

## 小白零基础 30 分钟 速通指南

> **教程贡献者说：**

> 我们整个学习活动，将带你从跑通最简的 `Baseline`，到了解竞赛通用流程、深入各个竞赛环节、精读 `Baseline` 与进阶实践，这些一个都不会少！

> 千里之行，始于足下，从这里，开启你的 AI 学习之旅吧！

> 记得在每次完成学习后，写笔记打卡哟~ 及时总结沉淀能让学习事半功倍~

### Step1：报名赛事！(点击即可跳转)

> 赛事链接：https://www.marsbigdata.com/competition/details?id=3839107548872（报名需填写 Datawhale）

1. 登录/注册赛事平台
1. 修改昵称并进行实名认证
1. 报名参赛
1. 修改队伍名称~
1. 登录/注册赛事平台

![](img/6eb7781d9a6e94ee1f2f022525f984a8.png)

![](img/fb790f8efcd52cf45c4f5a3844fb5970.png)

![](img/61c9c10629a93fbc2290de9b7a489e05.png)

1. 修改昵称、实名认证

![](img/b3760bb8a7e99fc96ae02eeaefe5a672.png)

![](img/c2a39697cf39c7664b41c3aa6a6a7772.png)

![](img/21fb2741280e8c4f7a2b05745abdc119.png)

1. [点击打开赛事链接](https://www.marsbigdata.com/competition/details?id=3839107548872)报名参赛
1. 了解渠道记得填：Datawhale/AI 夏令营/夏令营

![](img/5e3375840532e6311e724ad248637cb7.png)

1. 修改队伍名称

![](img/65109a8ae8f998eb2819aead1e01f805.png)

### Step2：领取厚德云支持的GPU在线算力！ (点击即可跳转)

> 链接：https://portal.houdeyun.cn/register?from=Datawhale

> 注册厚德云平台、使用4090学习！

> 本次学习免费算力由厚德云提供支持，为每位学习者提供150元算力（仅限参与夏令营的学习者）

1. [注册厚德云平台（点击跳转注册）](https://portal.houdeyun.cn/register?from=Datawhale)

> 链接：https://portal.houdeyun.cn/register?from=Datawhale

![](img/56f3e7510bf78dc54e334efb61962c5a.png)

1. 租用4090！（点击跳转机器管理界面）

> 链接：https://portal.houdeyun.cn/console/apparatus/gpu/generic

![](img/d6f3193aa244edf108e0e0ecd973092d.png)

![](img/af85ea4ba5aa296c7d1b1494f8fe6c2f.png)

![](img/4bd495dfceb933154b844b1ec98bc1b1.png)

![](img/64912c9e098c588e717a317bde6bcc03.png)

![](img/63939cd5ce9a6f1c58b4dc7a72fbc384.png)

![](img/54fafe1cdfc00fa0101ca6f34e4b0416.png)

### Step3：30分钟体验baseline！

> 在这一步，我们将进行激动人心的模型训练！不用担心，只需要点点鼠标就可以！

1. 下载baseline相关文件

> 复制如下命令，在终端中执行：

```
apt install git-lfs
git lfs install
git clone https://www.modelscope.cn/datasets/Datawhale/AI_Camp5_baseline_CV.git
```

![](img/f00d1fa8a85c00916339776f66c6006d.png)

1. 一键运行baseline！**（大约需要25分钟）**

![](img/2be6de3d93e5c7f53358b9062ffeee17.png)

1. 下载结果文件`result.zip`

![](img/69fb82de7b3afbc525bfc4b50d143f20.png)

1. 记得GPU使用完成后，关机~

![](img/8134b698215f40283ce6056f5dac75ff.png)

![](img/4e5a573c514ba5f0ccb6c8ac68343aa2.png)

![](img/dd33d9d1a1e9746927e48f2c5b3dfcbb.png)

### Step4：上传提交结果到比赛网站！

> 评分大概需要 2 分钟～评分结束后可以在【结果提交】看到评分结果哟！（预期分数在0.00 4分左右😉~）

> ⚠️ 每天可提交3次，如果格式不正确会评分失败，但也会消耗提交次数

![](img/b761bb6c8679fba2470e0cf8974ab9dd.png)

![](img/40db0f0199de518dab67760233ef1ca1.png)

![](img/d47551b7e48a54599c3cf29dfb53d624.png)

# 🎉 恭喜你拿下了属于自己的第一个分数！

## 👍🏻 快来给自己点赞！

## 🛎️ 别忘记打卡&在群里接龙！

![](img/5c807e2f212155e02bb26186f2a86362.png)

## 📅 然后迎接后续的挑战吧！

### 从零上手CV竞赛

| Task | Task 描述 | 相关知识点 | 学习安排 |
| --- | --- | --- | --- |
| Task 1：2天 | | 1. 赛题解读 | 开营直播 |
| Task 2：3天 | | 1. `YOLO`模型介绍 | `baseline`精读直播 |
| Task 3：2天 | | 1. 数据集增强 | 上分思路交流会 |
| | | | |

> **打怪难度：简单**

1. 跑通 `baseline`，拿下第一个分数！
1. 写笔记，完成 Task1 打卡

> **打怪难度：⭐️**

1. 精读 `baseline`，在群里分享思考
1. 学习进阶思路
1. 写笔记，完成 Task2 打卡

> **打怪难度：⭐️⭐️**

1. 学习 数据集增强，在群里分享思考
1. 思考更多上分技巧，参与上分思路交流会
1. 写笔记，完成 Task3 打卡

### 祝大家上分顺利，学习愉快~ 多在群里提问和交流哟！

+ 赛事链接：[2024“大运河杯”数据开发应用创新](https://www.marsbigdata.com/competition/details?id=3839107548872)大赛——城市治理赛道(点击跳转赛事官网)

+ 提问前建议先查看 [QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/WGtfwSP0aiZsXZkrS2Fc1YxUnGg) 是否早已收录你的问题~（QA文档可任意编辑，助教和教程贡献者会不定时查看）

+ 更多学习活动问题可见：[从零上手CV竞赛学习者手册](https://datawhaler.feishu.cn/wiki/CtBmwnIuOi86p2kfYyecMh3OnYg)

# 🍰 下面正式来了解一下赛题！

### 你知道刚刚发生了什么吗？

> 刚刚我们跑通了MVP，想必你一定很好奇这背后发生了什么吧？在学习的过程中，不单单要知道如何做，更要知道为什么要这样做。这样我们在遇到同类型的问题时，才能举一反三。

> 一般情况下，拿到一个赛题之后，我们需要明确：

> 1）这是一个什么场景下的赛题；
> 2）这个赛题要解决什么问题。

### 本次学习活动的赛题是一个典型的视频物体检测问题

> 本赛题最终目标是开发一套智能识别系统，能够自动检测和分类城市管理中的违规行为。该系统应利用先进的图像处理和计算机视觉技术，通过对摄像头捕获的视频进行分析，自动准确识别违规行为，并及时向管理部门发出告警，以实现更高效的城市管理。

## 赛题解读

| 赛事名称 | |
| --- | --- |
| 赛事链接 | https://www.marsbigdata.com/competition/details?id=3839107548872 |
| 数据集等 | 初赛提供城管视频监控数据与对应违规行为标注。违规行为包括垃圾桶满溢、机动车违停、非机动车违停等。视频数据为`mp4`格式，标注文件为`json`格式，每个视频对应一个`json`文件。 |
| 评分规则介绍 | 分数越高越好，使用`F1score`、`MOTA`指标来评估模型预测结果。 |
| 任务提交格式说明 | 格式要求 |
| 赛程安排 | |
| 赛事奖励 | |

**初赛需提交：**

初赛任务是根据给定的城管视频监控数据集，进行城市违规行为的检测。违规行为主要包括垃圾桶满溢、机动车违停、非机动车违停等。

选手需要能够从视频中分析并标记出违规行为，提供违规行为发生的时间和位置信息。

**初赛需提交：**

初赛提供城管视频监控数据与对应违规行为标注。违规行为包括垃圾桶满溢、机动车违停、非机动车违停等。

视频数据为`mp4`格式，标注文件为`json`格式，每个视频对应一个`json`文件。

**初赛：晋级x支队伍**

+ 开始时间~截止时间
+

**复赛：晋级6支队伍**

+ 开始时间~截止时间
+

**决赛**

+ 具体时间
+

## 来精读一下baseline代码

### 什么是`YOLO`？什么是物体检测模型？

`YOLO`，全称为“You Only Look Once”（你只看一眼），是一种流行的实时目标检测系统，由Joseph Redmon等人在2015年提出。`YOLO`模型的核心思想是将目标检测任务视为一个单一的回归问题，通过一个卷积神经网络（CNN）直接从图像像素到边界框坐标和类别概率的映射。`YOLO`模型经过了多次迭代，包括`YOLOv2`（`YOLO9000`）、`YOLOv3`和`YOLOv4`等版本，每个版本都在性能和速度上有所提升，同时也引入了一些新的技术，如更深的网络结构、更好的锚框机制、多尺度特征融合等。

![](img/710cd51a062ebd8fd78cc2f067cf9ee7.png)

### `YOLO`模型需要什么训练格式？

`YOLO`使用的标注格式是每张图像一个文本文件，文件名与图像文件名相对应。文本文件中每一行对应一个边界框，格式为：`<class> <x_center> <y_center> <width> <height>`。

其中，`<class>`是类别索引，`<x_center>`和`<y_center>`是边界框中心点相对于图像宽度和高度的比例，`<width>`和`<height>`是边界框的宽度和高度相对于图像宽度和高度的比例。

![](img/4013e95169171fb6ba8b1996e8871813.png)

```python
# 读取训练集视频
for anno_path, video_path in zip(train_annos[:5], train_videos[:5]):
    print(video_path)
    anno_df = pd.read_json(anno_path)
    cap = cv2.VideoCapture(video_path)
    frame_idx = 0 
    # 读取视频所有画面
    while True:
        ret, frame = cap.read()
        if not ret:
            break

        img_height, img_width = frame.shape[:2]
        
        # 将画面写为图
        frame_anno = anno_df[anno_df['frame_id'] == frame_idx]
        cv2.imwrite('./yolo-dataset/train/' + anno_path.split('/')[-1][:-5] + '_' + str(frame_idx) + '.jpg', frame)

        # 如果存在标注
        if len(frame_anno) != 0:
            with open('./yolo-dataset/train/' + anno_path.split('/')[-1][:-5] + '_' + str(frame_idx) + '.txt', 'w') as up:
                for category, bbox in zip(frame_anno['category'].values, frame_anno['bbox'].values):
                    category_idx = category_labels.index(category)
                    
                    # 计算yolo标注格式
                    x_min, y_min, x_max, y_max = bbox
                    x_center = (x_min + x_max) / 2 / img_width
                    y_center = (y_min + y_max) / 2 / img_height
                    width = (x_max - x_min) / img_width
                    height = (y_max - y_min) / img_height

                    if x_center > 1:
                        print(bbox)
                    up.write(f'{category_idx} {x_center} {y_center} {width} {height}\n')
        
        frame_idx += 1
```

### 如何训练`YOLO`模型？

`Ultraalytics` 是一个提供多种计算机视觉模型的库，包括 `YOLO` 系列。这段代码是一个简单的训练启动示例。

```python
from ultralytics import YOLO

# 设置模型版本
model = YOLO("yolov8n.pt") 

# 设定数据集和训练参数
results = model.train(data="yolo-dataset/yolo.yaml", epochs=2, imgsz=1080, batch=16)
```

训练日志如下：

![](img/8433b70f5a0ec0bb8c8626b1650c8d49.png)

+ `box_loss` 是边界框回归损失，用于评估预测的边界框与真实边界框之间的差异。
+ `cls_loss` 是分类损失，用于评估类别预测的准确性。
+ `dfl_loss` 是防御性损失，用于提高模型的泛化能力。

从输出结果来看，经过两个训练周期后，模型的边界框损失、分类损失和防御性损失都有所下降，这表明模型在训练过程中学习了如何更好地预测边界框和分类。同时，模型的 `mAP50` 和 `mAP50-95` 指标也有所提高，这表明模型的整体性能有所提升。

## 参考资料

1. `YOLO`指南：https://docs.ultralytics.com/guides/
1. https://docs.ultralytics.com/quickstart/

## [QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/WGtfwSP0aiZsXZkrS2Fc1YxUnGg)！

> [QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/WGtfwSP0aiZsXZkrS2Fc1YxUnGg)任何想法都可以在学习群里交流或在 进行填写哟~

>

> 本教程主要由 Datawhale的小伙伴们 写作贡献，



> 融合了多位助教的内测体验和宝贵建议。
> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

## 另外别忘了写笔记打卡呀！！~

## 及时思考、沉淀和产出能让你的学习事半功倍！

> **注：**本文采用署名—非商业性使用—相同方式共享 4.0 协议国际版许可协议进行许可。
