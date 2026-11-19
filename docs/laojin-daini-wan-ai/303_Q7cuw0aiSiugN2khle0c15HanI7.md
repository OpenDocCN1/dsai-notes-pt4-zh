
# --no（不想出现的）

> 来源：[https://my.feishu.cn/docx/LFh8dYIyboehcZxycDlcD6GhnUf](https://my.feishu.cn/docx/LFh8dYIyboehcZxycDlcD6GhnUf)

## No 参数告诉 Midjourney Bot 图像中不应包含哪些内容。

`--no`接受用逗号分隔的多个单词：`--no item1, item2, item3, item4`

![](img/78eaf66673a546dfcf1654771e689f69.png)

## `--no`

Midjourney Bot 将提示中的每个单词视为图像的潜在元素。提示 still life gouache painting without any fruit 或更有 still life gouache painting, please dont add fruit! 可能生成包含水果的图片，因为 Midjourney Bot 不会像人类读者那样理解“没有”或“不”与“水果”之间的关系。为了改善结果，请将提示重点放在您希望在图像中看到的内容上，并使用 `--no` 参数指定您不想包含的概念。

![](img/da25aa12f2450673c29d52e5cc805f98.png)

## 多重提示

参数 `--no` 与称重部分相同，[多点提示](https://docs.midjourney.com/multi-prompts) 为 `-.5` 的 `still life gouache painting:: fruit::-.5` 与 `still life gouache painting --no fruit` 相同。

---

## 如何使用无参数

添加 `--no item1, item2, item3` 到提示的末尾。

![](img/174dc24c8806bf34f5953fc8cb3f3b8c.png)

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
