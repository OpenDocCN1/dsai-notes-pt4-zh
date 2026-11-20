
![](img/f255d37e7b73b3e0a79e9e46e65b816f.png)

使用`/imagine prompt vibrant California poppies --mine`，被解释为`/imagine prompt vibrant California poppies --hd --ar 7:4`

`/prefer option list`

列出使用创建的所有选项`prefer option set`。用户最多可以有20个自定义选项。

![](img/f1a782f220f67f22684760d6e537c7c7.png)

要删除自定义选项，请使用`/prefer option set <name to delete>`值字段并将其留空。

## 自定义首选项：

`/prefer suffix`

在所有提示后自动附加指定的后缀。使用不带值的命令进行复位。

### 命令示例：

`/prefer suffix --uplight --video`

a. `Only Parameter`s可以与`/prefer suffix`一起使用

b. 支持：`prefer suffix --no orangeis`

c. 不支持：`prefer suffix orange::-1`

## 表情图标的功能

用指定的表情图标会触发常用功能

![](img/73520f231ed031ca265fc038feebba76.png)

![](img/7bdf4912ecba2a0a2d210a7eebc9933f.png)

`envelop`：信封表情符号可以将带有`seed`种子、生成ID和生成视频的图像发送给你的DM

`star`：星星表情符号可以将图像标记为“收藏”，收藏的图像会显示在画廊中，并将图像发送到`#favorites`频道

![](img/66f3e934d0930c499045fc2813e801a8.png)

`X`：X表情符号可以取消和删除图像，同时也会从画廊中输出
