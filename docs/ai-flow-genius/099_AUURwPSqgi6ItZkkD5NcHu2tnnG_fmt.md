
# Midjourney 推出神器「局部重绘」，这是一份官方教程

> 来源：[https://jalicvoygc0.feishu.cn/docx/R9imdGY8qomehrxTI0tcCU1nncd](https://jalicvoygc0.feishu.cn/docx/R9imdGY8qomehrxTI0tcCU1nncd)

> Midjourney 在昨天晚上发布了局部重绘「`Vary(Region)`」的更新，这次的功能比较复杂他们还让discord专门开发了一个界面来处理。下面是一份的 NIji 官方教程，详细介绍了局部重绘的各种高级用法。可以在指定区域放图片重绘也太强了。

引用：https://mp.weixin.qq.com/s/-zrnUzwxlJ0wWcTU8g_4lg

现在，我们将向您展示如何使用 `Vary(Region)` 功能的高级技术。

![](img/6e87eba3d7bc71dbfc2c9172b1674d20.png)

## 在哪里找到 `Vary(Region)` 功能

您会在放大结果中找到“`Vary(Region)`”，位于“`Vary(Subtle)` 和 `Vary(Strong)`”按钮旁边。

![](img/679c1cf10c56ad002f23b0db9319be79.png)

## 请务必更新你的 Discord 客户端

如果你没有看到“`Vary(Region)`”按钮，请在 Discord 客户端窗口上按 `ctrl + shift + R` 进行更新并重新启动。

## 选择区域

该界面中有 4 个重要部分。

![](img/bc2f6e95cbdcbcdd68397b7581e67482.png)

1) 选取框允许你选择一个矩形区域。

2) 套索工具可让你徒手绘制一个区域。

3) 撤消按钮可修复错误。

4) 提示框允许你修改提示。

## 提示词

注意：如果您使用Midjourney，则需要打开 `remix mode` 才能看到提示对话框。这个提示对话框在 nijijourney 上默认启用。

## 修复元素

要修复区域，请不要修改提示词。只需选择一个区域并点击确认箭头即可重新绘制受影响的区域。在这里，我们重新绘制了畸形的手。

![](img/dc91b8c25dd26906952bf5ef248c22ed.png)

![](img/db9d3fed4dce331903319f447d25df03.png)

## 更改元素

你可以通过在提示框中输入不同的提示来对图片进行更大幅度的更改。在这里，我们用一朵花代替了匕首。

![](img/7e4636edf4568a92b39e9e8f3a122eca.png)

![](img/05ddb60cbe4d233a36d53a095922c830.png)

![](img/e02dfbc337bb90f50ebcef42d48d7c05.png)

`girl with white hair, blue eyes, holding a sword`

## 添加元素

要添加图片中不存在的元素，请选择该区域，然后将要添加的元素附加到提示中。 （参见“简化提示”部分）

![](img/d0e72fe4a07231e43bb793b246353d86.png)

![](img/f120fd3fa5d30b25756a7c65caed89cf.png)

![](img/ad47bf5b025445b3c1b73709b94f5d89.png)

`girl with white hair, blue eyes, holding a sword` `girl with a teddy bear`

## 删除元素

要删除元素，请选择要删除的项目，然后将要删除的区域的元素提示词删掉。

![](img/fd8cee19c1b0f4fe951179655489a845.png)

![](img/2f8221cd24ef2871e7b7bb9a855c3d64.png)

![](img/621703c8449a6a959a0b4e125e329926.png)

`girl with white hair, blue eyes, holding a swor` `girl`

你必须删除这个区域的提示词，而不是将权重改为负数

要从这张照片中删除手，不要写“女孩，没有手”，只需写“女孩”

## 高级提示

### 进行增量重绘

可以在 `Vary(Region)` 结果上按 `Vary(Region)`。这允许通过增量调整进行编辑。

固定手：

![](img/2e5b0e6c9c178beec997d69d932b46db.png)

![](img/1b92c6fe66d61a12f99fd0c89dbab51c.png)

固定剑：

![](img/28a906bfada2c9fe7e6579ddeae3b754.png)

![](img/f7a834835de8f91b9c3038f86ed63745.png)

让手势更加细腻：

![](img/61628fc17ed572a3e93b06005f2e830c.png)

![](img/5d3413e966ce863fc9c6d0a53e2b890b.png)

固定手掌：

![](img/3a34f07988e942d6f405100ecace2cb8.png)

![](img/08510ba158d4eea898e6a77e93281834.png)

最终对比：非常精致的手部姿势！

![](img/6b55e9432fbfa17ed423eb7336640e14.png)

![](img/6a8318fe280a936db1384dbabcb834dd.png)

### 简化提示

如果你要对提示进行修改，应该重绘的是niji所在的区域，而不是整幅画面。

例如，这张原始图像是用提示“白发、蓝眼、手持剑的女孩”制作的

`girl with white hair, blue eyes, holding a sword`

但是，在进行后续编辑时，我们应该减少对未重新绘制的区域的描述。

![](img/7ad2d61fd201a2bd894df26b839d1661.png)

`girl, holding a flower.`

通过使用“女孩，拿着一朵花”，我们可以达到以下结果。我们删除了关于女孩的细节，因为花朵是重画的焦点。

简化对正在更改的部分的提示将确保 niji 优先考虑所请求的编辑，而不是花费精力来使其余细节正确。

### 调整区域大小

修改提示时请仔细选择区域的大小。

![](img/c5dbadb9eb326a334df96c5d8e07c265.png)

如果边界太紧张，niji 将没有足够的空间来重新绘制您的请求。

![](img/008b3637cb5abbcd267c72222ef91923.png)

![](img/d0dff5358c3af26e1966b03cabddd9d5.png)

![](img/4ca612c3ea7df9bd0c940cdb003dce06.png)

如果所选区域太大，niji 最终会重新绘制您想要保留的部分

![](img/ee8a8c66b4e0cc4d079825721858d1bc.png)

![](img/09b433d43a024a71b7f9e10fae29b20c.png)

![](img/6b0e2b6b81e31543f18088ae3102bfa9.png)

确保合适地调整区域的大小。如果有疑问，可以选择更大一点。

![](img/cad4727dc30874c8173d39a14c60ee70.png)

![](img/d916f9b08af19645ab2d24730de3aa6f.png)

### 使用图像提示

当区域太小而无法正确重新绘制时，可以使用图像提示来强制校正。

只需将图像 `url` 放入提示框中即可。在这里，我们想送给她蓝色钻石耳环。这很难指定，因此我们将使用图像提示。

![](img/e1a8a1e4e3c98e7f9f3b3edf54eac2cb.png)

![](img/fc066f5cc1983352915f31e9d5a1c6cd.png)

结果：

![](img/99969665674cc133f42f0b64c884ac29.png)

请注意，图像提示将导致更不自然的重绘。然而，有时这对于确保所有细节正确是必要的！

### 混合风格和模型

您可以通过在提示中指定来切换用于新区域的模型。

例如，虽然此图像是在默认设置下制作的，但我们可以调用 `--style Cute` 以不同的风格绘制脸部。只需将后缀添加到提示中即可：

![](img/e623b70a4ccd4516c345f94ece16a0e5.png)

![](img/8f5e92ce3f91d7d91b79277922ede39e.png)

使用这种技术，我们甚至可以切换到中途模型：

![](img/9bf9ab8ab9031e10e79293b201cbdd17.png)

## 继续浏览全站内容

[🎪首页](https://jalicvoygc0.feishu.cn/wiki/DdEvwxYsSidd36kAIIVcnROwnmb)

[🤔学习AI工作流](https://jalicvoygc0.feishu.cn/wiki/W15IwochvimsqvkkuCPcZuBxnqg)

[🤖查找AI工具](https://jalicvoygc0.feishu.cn/wiki/Bu8GwqYu8i5LBRkyVKocUvX5n97)

[📊阅读AI文章](https://jalicvoygc0.feishu.cn/wiki/SIywwqrVji2lZkkqFFpcEHZ8nNc)

扫码加入AI Flow Genius社群，探讨使用AI来提升生产力的方法，成为超级个体

超过200需手动拉人 **备注：加群**

![](img/340fb4cdd4187525b72ef6032a3dc158.png)
