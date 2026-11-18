
# Seeds 随机种子

> 来源：[https://my.feishu.cn/docx/EG2udT6SLoayuqxeyMPc27e9n5J](https://my.feishu.cn/docx/EG2udT6SLoayuqxeyMPc27e9n5J)

每个图像的 `seed` 都是随机生成的，可以使用 `--seed` 或 `--sameseed` 参数进行指定。使用相同的 `seed` 和提示会生成结果相近的图像。

+ `--seed` 接受 0-4294967295 的整数。
+ `--seed` 的值只会影响初始图像网格。
+ 在使用 [模型版本](https://docs.midjourney.com/models) 1、2、3、test 和 testp 时，相同的 `--seed` 值会生成具有类似构图、颜色和细节的图像。
+ 在使用 [模型版本](https://docs.midjourney.com/models) 4、5 和 niji 时，相同的 `--seed` 值会产生几乎完全相同的图像。
+ 种子数字不是静态的，不能在会话中依赖它们。

## Seed 参数

如果没有指定 `seed`，Midjourney 都会使用不同的随机种子来生成图片，因此产生的图像会有很大的差异和多样性。

使用随机 `seed` 生成三次试试看：

例子：`/imagine prompt celadon owl pitcher`

### 指定 `seed` 生成两次试试看 `--seed 123`：

例子：`/imagine prompt celadon owl pitcher --seed 123`

## Sameseed Parameter（v4 v5 用不上）

使用 `--seed` 参数将创建一个单一的随机 `seed`，并应用在所有的初始图像上。当使用 `--sameseed` 参数时，初始网格中的所有图像都使用相同的 `seed`，并且生成的图像非常相似。

`--sameseed` 参数只接受 0-4294967295 之间的整数。`--sameseed` 参数仅适用于模型版本 1、2、3、test 和 testp。

## 在哪里看 seed 值呢

对作业（Job）添加 ✉️ 信封图标来查找工作的 `seed`。

要获取之前图像的 `seed`，请[复制 `Job ID`](https://docs.midjourney.com/docs/show-job)，结合该 ID 使用 `/show <Job ID #>` 命令来恢复作业。然后，可以对新生成的工作添加 ✉️ 信封表情符号来获取工作的种子号码。

## 如何修改 seed 值

添加 `--seed <值` 或 `--sameseed <值` 在提示末尾。
