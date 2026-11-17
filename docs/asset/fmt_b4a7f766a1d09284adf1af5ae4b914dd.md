
# 20240414-Grok-1.5 视觉预览

> 来源：[https://aigcreative.feishu.cn/docx/VyUAdJokOot71qxeM71cpVN2nIQ](https://aigcreative.feishu.cn/docx/VyUAdJokOot71qxeM71cpVN2nIQ)

![](img/db613909bbd6867c28f3eb8f4074b74d.png)

首次推出我们的多模态模型，实现数字世界与物理世界的无缝连接。

介绍 Grok-1.5V，我们推出的首款多模态模型。Grok 不仅拥有出色的文本处理能力，还能分析包括文档、图表、图示、屏幕截图和照片在内的广泛视觉信息。Grok-1.5V 即将向我们的早期测试者及现有用户开放。

## 能力展示

在多个领域，Grok-1.5V 与当前领先的多模态模型不相上下，涵盖从多学科推理到理解文档、科学图示、图表、屏幕截图及照片等。我们对 Grok 在解读物理世界方面的卓越能力感到格外激动。在我们新开发的 RealWorldQA 真实世界空间理解基准测试中，Grok 的表现超越了其它同类模型。对于所有这些数据集，我们都采用了零样本 (zero-shot) 方法进行评估，且未使用思维链提示。

![](img/2040d74ac7cdd6de1f3a585bcafb0b5d.png)

## 案例1：根据图表写代码

![](img/755673e3bcd52dd46b4def2b67d3bd46.png)

用户：你能把这个翻译成 Python 代码吗？

Grok：可以的！你提供的流程图描述了一个计算机生成随机数的简单猜数字游戏，玩家需要猜出这个数字。以下是根据流程图逻辑编写的 Python 代码：

```
import random
def guess_number(): # Generate a random number between 1 and 10
    target = random.randint(1, 10)
    while True:
        guess = int(input("Guess a number between 1 and 10: "))
        if guess == target:
            print("You won!")
            break
        else:
            print("Wrong guess, try again!")

if __name__ == "__main__":
    guess_number()
```

这段代码将实现流程图中描述的猜数字游戏。用户会不断被邀请输入他们的猜测，直到猜中正确的数字。

## 案例2：计算卡路里

![](img/74fd677424d6f0d9fe24f58519ef0e6d.png)

用户：这五片总共有多少卡路里？

Grok：如果每3片算一份，含60卡路里，那么5片大约有100卡路里。首先计算每片的卡路里：60卡路里除以3片等于每片20卡路里。然后，将每片卡路里乘以片数：20卡路里乘以5片等于100卡路里。

## 案例3：从画作到睡前故事

![](img/a86c11d579fb4cca95e9c7a89f4f60cd.png)

用户：我儿子画了这幅画，你能根据它讲一个睡前故事吗？

Grok：从前有个名叫 Timmy 的小男孩，他住在一个充满奇迹的神奇土地上。Timmy 和家人住在一个舒适的小屋里，他最喜欢的事情就是去探索周围的世界。

在一个晴朗的日子，Timmy 决定去附近的森林冒险。他准备了自己最爱的零食、一只指南针，以及他的笔记本，打算记录下他看到的一切奇妙景象。

深入森林后，他发现了一条闪烁着阳光的魔法河流。这条河流非常迷人，Timmy 决定做一只小纸船，将它放在河上，送给森林精灵一个特别的信息。

Timmy 认真地折了一只纸船，在上面画了一个笑脸，希望给森林精灵带去快乐。他小心地将纸船放入河中，观看它带着友爱和对自然的热爱缓缓漂走。

随着夕阳西下，Timmy 知道该回家了。他感激这一天的神奇体验，并相信这次冒险的回忆将永存心间。

回家后，Timmy 向家人讲述了他的冒险故事，家人听后都感到非常高兴。他们认为 Timmy 勇敢又善良，未来定会有更多精彩的探险等着他。

带着满心的爱和梦想，Timmy 很快进入了甜美的梦乡，憧憬着在他的想象世界中即将到来的新冒险。

## 案例4：解释梗图

![](img/b8c03c3492b397bf03de45ccbc8d0124.png)

用户：我不太明白，能解释一下吗？

Grok：这幅图幽默地比较了创业公司与大公司的不同。在左侧，标记为“创业公司”的区域，可以看到一群人正在齐心协力挖掘一个坑，每个人都在积极作业，坑也挖得很深了。

而在右侧，标记为“大公司”的区域，虽然人群围着同样一个坑，但除了一个人在挖之外，其他人或是在旁观，或是忙其他的事。

图中的幽默在于夸大了这两种环境的对比：在创业公司，每个人都有紧迫感，积极参与到具体任务中；相反，大公司则被描绘为流程多、个人直接参与项目的机会少。这种夸张描绘带出了两者工作方式的鲜明差异。

## 案例5：将表格转换为CSV

![](img/78e9aff056f1a7769b38940a41ba9999.png)

用户：请帮我把这个表格转成 CSV 格式。

Grok：图片中的表格可以这样转换成 CSV 格式：

```
medal,name,sport,event,date
21 gold,El Amin Chentouf,Athletics,Men's marathon - T13,11 September
31 gold,Mohamed Amguoun,Athletics,Men's 400m - T13,15 September
41 gold,Azeddine Nouiri,Athletics,Men's shot put - T34,15 September
52 silver,Mahdi Afri,Athletics,Men's 400m - T12,9 September
62 silver,El Amin Chentouf,Athletics,Men's 5000m - T13,15 September
73 bronze,Mahdi Afri,Athletics,Men's 200m - T12,17 September
83 bronze,Mohamed Lahna,Paratriathlon,Men's PT2,10 September
```

请注意，CSV 格式不支持样式或格式化，所有信息都将以纯文本形式展示。

## 案例6：甲板上的木头腐烂了怎么办？

![](img/2516f035f4325a17d2402ac6ba60b932.png)

用户：我甲板上的螺丝周围出现了这些孔洞。是腐烂了吗？需要更换木板吗？

Grok：从你描述的情况来看，你的甲板木材已经开始腐烂。螺丝周围出现孔洞是木材腐朽的典型迹象。

为了保证甲板的安全和使用寿命，建议你更换那些受损的木板，这可以有效防止甲板进一步损坏，确保其结构稳固。

如果你不确定如何更换木板，最好咨询一位专业的甲板建造师或承包商。他们可以实地评估损害的程度，并为你的甲板维修提供专业的建议和方案。

## 案例7：解决编程问题

![](img/3cac23f76830e7e70ac5418295518a62.png)

用户：你能写Python代码来解决这个问题吗？

Grok：

```
def find_triplets(nums):
    result = []
    nums.sort()
    
    for i in range(len(nums) - 2):
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        
        target = -nums[i]
        left = i + 1
        right = len(nums) - 1
        
        while left < right:
            if nums[left] + nums[right] == target:
                result.append([-target, nums[left], nums[right]])
                left += 1
                right -= 1
                while left < right and nums[left] == nums[left - 1]:
                    left += 1
                while left < right and nums[right] == nums[right + 1]:
                    right -= 1
            elif nums[left] + nums[right] < target:
                left += 1
            else:
                right -= 1
    
    return result
```

## 现实世界理解

为了开发出真正有用的现实世界AI助手，关键在于加深模型对物理世界的理解。为此，我们推出了一个新的评估标准——RealWorldQA。该标准专门设计用来测试多模态模型在现实世界空间理解方面的基本能力。尽管该标准中的许多示例对人类而言相对简单，但对于最新的模型而言，这些测试往往是个不小的挑战

![](img/a54bada157729b0e6fe5dd26625026a3.png)

RealWorldQA的首版已发布，包括700多张图片，每张图片附带一个问题及其可验证的答案。这些数据主要由车辆拍摄的匿名化图片和其他实景图片构成。我们非常期待将这一资源分享给大家，并计划随着多模态模型的进步持续扩充此数据集。RealWorldQA已根据CC BY-ND 4.0许可证发布。点击这里（文件大小677MB）下载数据集。

## 展望未来

为了构建能深入理解宇宙的有益通用人工智能（AGI），推进我们在多模态理解与生成技术上的能力是关键。未来几月，我们期待在图像、音频和视频等多种模态上实现这些能力的显著提升。

如果您希望加入这一伟大的探索之旅，欢迎加入我们的团队。
