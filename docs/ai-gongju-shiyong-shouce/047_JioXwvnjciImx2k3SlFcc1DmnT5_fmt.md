
# SD教程NO.08👉Lora（上）

> 来源：[https://my.feishu.cn/docx/GISsdKyTFooMNFxsMAzcpXeonFz](https://my.feishu.cn/docx/GISsdKyTFooMNFxsMAzcpXeonFz)

小黑子来了😻因为lora内容太多我会分开更新，今天讲基础，所以咱们这一期标题叫Lora（上）

lora要说明白的话非常复杂，原理啥的大家感兴趣，就自己问主或者百度，这里我讲点自己的理解和应用。

👉前面讲的底模是大模型，决定你的画风，lora可以定义为小模型，决定你的内容。

![](img/44a400557c7dfa2faafab7bfa0c3983d.png)

👉大模型一般是2G-8个G，lora一般是几十M~100多M

【tips：一般sd爱好者说炼丹就是做lora小模型】

下载是一样的，都在🎨C站https://civitai.com/

💥大家使用lora一定要注意，把它的触发关键词放在你的正面tag

💥不然lora不会生效，红框这里就是触发词👇

👉一些复杂的lora比如hanfu汉服里面，有不同朝代的服饰需要用不同的触发词去控制，所以这块要弄明白了再去尝试。

![](img/ae7046f2baba1ac6e820d81a50ec626b.png)

文生图前面说过，就是通过tag来让ai生成图片；当我们想让sd生成一些特别的角色，比如原神派蒙LOL的拉克丝这些很有特色的，用tag实现的难度非常大，这时候就需要lora了。

![](img/87c66dca919cdcf1bc61108bd4938e51.png)

考虑到咱们这儿不一定都是游戏用户，我就拿这个lora举例吧。

下载地址：https://civitai.com/models/28531/miniature-world-style

总共就三个大步骤，看完就明白了；1️⃣下载2️⃣复制3️⃣出图

1️⃣首先下载lora，然后点启动器的这里

![](img/4d0d17525ae32537992a1a53bc99dc52.png)

如图示，把lora丢到这儿

![](img/69d6d51ab32a025a6dd727c3d63d41ef.png)

2️⃣然后咱们就能跑图了，不知道tag怎么写的话，回到C站，点图片看下作者怎么写的

进入这张图片后，右边区域往下拉

![](img/cb3e4f4b812860a391644cc17e986f3a.png)

一图看懂这些这些是啥

![](img/a4920f442ee3a89697b057a8eeb9dc97.png)

然后咱们点复制，粘贴到自己的sd web ui 的正面tag区域，再点这个箭头

![](img/a0de6ef1c7c4df00b6be80c5b2244a21.png)

**3️⃣👉这一步非常重要👈**

看下面这张图进入**lora**选择的界面，如果你是刚下载的lora记得点刷新。

记得检查下大模型是否正确。vae用默认`animevae.pt`这个就好

![](img/1be0a9c53ddd4b9268d8b7555fe9be0d.png)

然后啥也不用改了，直接点生成，等待两分钟。

![](img/ef9535a03783f588a6abe696fa2c96a5.png)

然后我显存爆掉了🤣1070不行啊，开高清修复就爆，把高清关了继续搞

![](img/d6edb39f4cedcf508518661184b00907.png)

好了图出来了

![](img/df96188620f29da002c2a9f28f06715c.png)

😢细节差太多了，富哥开下高清修复，跑一个放在评论区给我看看呗

如果按照以上步骤来做各种图，发现出来的图不一样，一般是以下几个原因：

1、现在发布的lora模型是新版，但是tag是老版本，结果就是**不触发lora**（检查方法往上翻在**3️⃣👉这一步非常重要👈**）

2、作者没把完整的tag放上去（虽然他没义务放但很容易被吐槽哈哈哈哈

3、作者混合了lora，做人物照片很常见，所以大家一定要注意

4、跟我一样硬件拉跨不能效果全开……

lora（上）篇先到这里，大家赶紧玩起来吧！

🎁哦对了，我自己整理了一些lora下载地址，省的大家找了：

链接：https://pan.baidu.com/s/1SnVSAbcHt2zoI9rVBqsm7A?pwd=axhz

提取码：axhz

有要补充的欢迎评论区留言

> 我是热爱生活的小黑子，热爱当下生活，永远忠于自己。
