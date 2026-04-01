# --seed（种子）

> 来源：[https://my.feishu.cn/docx/LDmNdCraToShuixDYZ4cAXHHn9b](https://my.feishu.cn/docx/LDmNdCraToShuixDYZ4cAXHHn9b)

Midjourney 机器人使用种子号创建一个视觉噪声场（如电视静态图像），作为生成初始图像网格的起点。种子号是为每个图像随机生成的，但可以使用参数指定--seed。如果您使用相同的种子号和提示，您将获得相似的最终图像。

+   **--seed**接受整数 0–4294967295。

+   **--seed**值仅影响初始图像网格。

+   **--seed**使用[模型版本](https://docs.midjourney.com/models) **1**、**2**、**3**、**test**和 的相同值**testp**将产生具有相似构图、颜色和细节的图像。

+   **--seed**使用[模型版本](https://docs.midjourney.com/models) **4**、**5**和**6**的相同值**niji**将产生几乎相同的图像。

+   种子数量不是静态的，不应在会话之间依赖它。

---

## 种子参数

如果未指定种子，Midjourney 将使用随机生成的种子号，每次使用提示时都会产生各种各样的选项。

### 作业使用随机种子运行三次：

提示示例：**/imagine prompt** celadon owl pitcher

![](img/71011ea94d27a54b2c9d40113f5de110.png)

### 作业运行三次**--seed 12345**：

提示示例：**/imagine prompt** celadon owl pitcher --seed 123

![](img/8c042b132d52d69bc4f3dc08b14df918.png)

---

## 如何查找工作的种子号码

### 使用 Discord 表情符号反应

通过对作业使用 ✉️ 信封表情符号做出反应，在 discord 中查找作业的种子编号。

### 使用 Show 命令恢复旧作业

要获取过去图像的种子编号，[请复制作业 ID](https://docs.midjourney.com/docs/show-job)并使用/show <Job ID #>具有该 ID 的命令来恢复作业。然后，您可以使用 ✉️ 信封表情符号对新生成的作业做出反应。

---

## 如何更改种子数量

### 使用**--seed**参数

添加**--seed <value>**到提示的末尾。

![](img/87792465eee4d09ee92f8f17f034bb63.png)

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>