
# Chaos 混沌值

> 来源：[https://my.feishu.cn/docx/SQqhdARC3oEhyDx21eLcPSp0n0f](https://my.feishu.cn/docx/SQqhdARC3oEhyDx21eLcPSp0n0f)

`--chaos` 或 `--c` 参数会影响图像混沌变化程度。`--chaos` 值越高，在某些情况下，高混沌值可以创造出审美上令人满意的结果，但是可能不能满足功能方面的要求。`--chaos` 值低，结果更可靠、可重复。

`--chaos` 可接受范围 0–100。

默认值 `--chaos` 0。

## Chaos 参数对生成图片的影响

### Low `--chaos` values

使用低的 `--chaos` 值或不指定该值将会在每次运行作业时产生细微变化的图像。

例子: `imagine/ prompt watermelon owl hybrid`

### High `--chaos` Values

使用高的 `--chaos` 值将会在每次运行作业时产生变化更多的图像。

例子: `imagine/ prompt watermelon owl hybrid --c 50`

### Very High `--chaos` Values

使用超高的 `--chaos` 值将会在每次运行作业时产生非常不一样的艺术效果的图像。

例子: `imagine/ prompt watermelon owl hybrid --c 100`

## 如何改变 Chaos 值

### 使用 `--chaos` 或者 `--c` 参数

提示末尾添加 `--chaos <value>` 或者 `--c <value>`。

![](img/ba8b614225e73a439dd4b38c07acd291.png)
