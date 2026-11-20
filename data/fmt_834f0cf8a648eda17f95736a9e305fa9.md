
# --cref（人物参考）

> 来源：[https://my.feishu.cn/docx/Dl13d4xTyoHEpoxUI7mcVYgOngf](https://my.feishu.cn/docx/Dl13d4xTyoHEpoxUI7mcVYgOngf)

您可以在提示中使用图像作为字符引用，以在不同情况下创建同一字符的图像。

要向提示中添加字符引用，请使用 `--cref` 参数和在线存储图像的网址：`--cref URL`

![](img/feba85cd844e5ff8bfba0020573e1604.png)

+ 当使用 Midjourney 生成的具有单个字符的图像时，字符引用效果最佳。
+ `--cref`并非设计用于真人照片，并且会使照片变形。
+ 特定雀斑或 T 恤徽标等精确的细节不太可能被复制。
+ `--cref`可与[Midjourney](https://docs.midjourney.com/v1/docs/character-reference#combining-style-and-character-references)和Niji Version 6一起使用，并可与结合使用`--sref`。
+ 您可以在单个提示中使用多个字符引用，但可能不需要。
+ 任何常规图像提示必须在`--cref`之前。

## 隐私声明

+ 使用 Midjourney Bot 在您的直接消息中上传图像，以防止其他服务器用户看到图像。
+ [除非您以隐身模式](https://docs.midjourney.com/v1/docs/stealth)生成，否则图像提示和参考资料在 Midjourney 网站上可见。

## 图像提示帮助

请参阅[图像提示](https://docs.midjourney.com/v1/docs/image-prompts)页面以获取有关在 Discord [提示中使用图像 URL](https://docs.midjourney.com/v1/docs/using-images) 的帮助，或参阅网页上的使用图像引用页面以获取有关在 midjourney.com 上使用各种图像引用的帮助。

## 示例

![](img/a8ad51bc135936e89b8f978229dd23dc.png)

## 角色权重参数

使用角色权重参数`--cw`来设置人物刻画的强度。`--cw`接受从 0 到 100 的值。`--cw 0`仅关注角色的脸部。较高的值使用角色的脸部、头发和衣服。`--cw 100`是默认值。

提示示例：`/imagine prompt illustration of a man sitting in a cafe --cref https://url.com/BlueHairGuy.png`

![](img/74c3a61ae4738d2e635c916d8cc0ba5e.png)

## 结合风格和性格参考

为了在使用字符引用时保持一致的样式，您可以组合使用`--sref`和`--cref`参数。有关使用 `--sref` 的更多信息，请参阅[样式参考页面](https://docs.midjourney.com/v1/docs/style-reference)。

`/imagine prompt illustration of a man sitting in a cafe --cref https://url.com/BlueHairGuy.png --sref https://url.com/BlueHairGuy.png`

![](img/eb6cbe4ddbcf81e864ff95a65f20585c.png)

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
