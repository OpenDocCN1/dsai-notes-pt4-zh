# Midjourney AI V6版本详解-官方公式

> 来源：[https://aigcreative.feishu.cn/docx/MT3Gd9JO0ors4exlT9ncQsp9nnd](https://aigcreative.feishu.cn/docx/MT3Gd9JO0ors4exlT9ncQsp9nnd)

> 公众号：RaDesign，作者：五花肉

> 链接：https://mp.weixin.qq.com/s/lIgo0DMXT4nQVBzzQo2trw

在我写v6版本详情的时候，官方在前一天公布了公式，该公式是官方设计总监发布的，也有人说是 Midjourney 的创始人 David Holz 提出了一种新的提示方法。公式来自 @ciguleva

# **官方公式 · 新提示结构**

MIDJOURNEY 的新提示结构，具备更好的提示理解能力。这意味着不再使用随机短语和词汇。我们将需要一种新的创建提示的方式。

![](img/d88f4b014a5be88d0354206e2a0b9f3a.png)

** 新提示结构 **

**公式：**Subject + Style + Setting + Composition + Lighting + Additional Info

**翻译： **主题 + 风格 + 环境 + 构图 + 光照 + 附加信息

结构解释

**1.风格**· 目的：提供具体的审美或艺术方向。· 包括的细节：偏好的风格或时代。

**2.主题**· 目的：定义图像的主要焦点。· 包括的细节：中心主题（如人物、物体、动物）的特征，包括外貌、颜色和独特特征。

**3.环境**· 目的：为主题建立环境或背景。· 包括的细节：位置（室内、室外、虚构的）、环境元素（自然、城市）、时间和天气条件。

**4.构图**· 目的：确定主题和元素的框架和视角。· 包括的细节：视角（特写、广角、航拍）、角度和具体的框架偏好。

**5.光照**· 目的：设置图像的情绪和视觉基调。· 包括的细节：照明类型（明亮、昏暗、自然）、情绪（愉快、神秘）和大气效果。

**6.附加信息**· 目的：为图像增添复杂性和深度。· 包括的细节：次要对象、角色、动物，以及它们与主题的相互作用或相对于主题的位置。

**示例：**a mechanic woman | GTA portrait flat illustration | pastel tones | garage in the background | videogame loading screen | digital 2D --v 6.0

**翻译：**女机械师（主题）| GTA肖像平面插图（风格） | 柔和的色调（光照） | 背景是车库（环境） | 电子游戏加载屏幕（构图） | 数字2 d（附加信息）--v 6.0

![](img/3620c161b7bb4addc3b045abfd0a4be5.png)

![](img/51e220a95f09714c1f8fd09d5ca04441.png)

# **官方公式 · 多提示结构**

MIDJOURNEY 的多提示结构，Midjourney V6 Alpha 开放测试，发现较短的版本对我的提示效果更好。此外，使用多提示产生了有趣的结果。

![](img/0a5fd861a4f55f9591078e0928c097e8.png)

** 多提示结构 **

+   **公式：Subject **+** **Style** **+** **Colors** **+** ****Mood::3Style **+** **Subject** **+** Background +**** **V**i**sual** Detail**s** ** + Mood翻译**： 主题 + 风格**+ 颜色 + 气氛**:****:3 风格 **主题 + 背景 +  视觉细节  气氛

**示例：**close-up photo of a man in a futuristic suit, grey blue, warm light, foggy::3 minimalist photo of an man in a futuristic suit, mountains, north nature, muted tones, warm light, low contrast, foggy --ar 21:9 --v 6.0

**翻译：**** **一个穿着未来服装的男子的特写照（主题）灰蓝色 | 温暖的光线（颜色）| 有雾::3（气氛）| 一个穿着未来服装的男子的极简主义照片（风格）| 山脉，北方自然 （背景）| 柔和的色调，温暖的光线，低对比度 （视觉细节）| 有雾（气氛）--ar 21:9 --v 6.0

![](img/28085a3dae5e30b7cd9dbb85401150f5.png)

![](img/c3c9eb4cac25b9ddb6b3237e0486ffaa.png)

# **官方公式 · 安全公式说明**

你知道吗，为了阻止 **混合污染** 效果，你可以在不希望出现的地方添加更多细节吗？如果你说“一位穿着传统长袍的印度妇女”，然后你开始在图片的另一位人物身上看到印度的项链，你可以具体指定另一位人物“颈部裸露”，或者说明他们戴了什么首饰，这将阻止混合效果。

![](img/c7c9a0ee991d4e393c31202849e69c92.png)

在 V6 中，有一种类似于“安全公式”的提示方法，它包括一个设置语句（我们在 V5 中写的方式 去 #Prompt-faps 频道查看，这个频道很重要），然后是对这些确切单词的回调。

** 看看这个提示 **

Two scientists argue over money in a twilight cafe. The scientist on the left is an adult female with hairstyle, clothing, details, attitudes, emotions, ethnicity, etc. The scientist on the right is an adult female wi**th ****hair**style, clothing, details, attitudes, emotions, ethnicity, etc. The twilight cafe is adjective, adjective, adjective. The image is a full-color vintage photograph inspired by Southern Living Magazine.两名科学家在黄昏咖啡馆争论金钱。左边的科学家是一位成年女性，有发型、服装、细节、态度、情感、种族等。右边的 科学家 是一位成年女性，有发型、服装、细节、态度、情感、种族等。黄昏 咖啡馆 是形容词、形容词、形容词。这张图片是一张彩色的复古照片，灵感来自《Southern Living Magazine》。

**暂时忽**略这些**幽默的**占**位符。只要注意对 科学家 和** 咖啡**馆 的回调。**重复这些词语会创建更强的联系。你不能太可靠地说 “第一位科学家” 和 “第二位科学家”，因为这对于 V6 来说意义不大。但是位置（左、中、右、前景、中心、背景）似乎有一定的意义。

![](img/16541224ecda923a931605a543d9268c.png)

# **官方构图 · 相机角度控制构图**

这些技巧源自摄影，最适用于通常在照片中找到的对象。对于更加异国情调的主题，它们可能效果较差，但对我而言效果不错。

+   Macro · 微距 Close-up · 特写  Wide Angle · 广角 - Diagonal Angle · 对角线角度

+   Aerial · 航拍 High Angle ·  俯视角Low Angle ·  仰视角 - Oblique Angle ·  斜视角

![](img/33893e23c49c7b17938b0c89615ab90a.png)

![](img/8e7aa49edbef6abddbc213b52b38def9.png)

# **V6其他要点 · 注意说明**

· V6 它不是默认设置。在/settings中选择它以使用它。

· 提示不起作用？努力写简单的英语句子，语法正确，拼写正确。

· 你可以指定颜色、位置和其他细节。但尽量不要过分依赖代词。最好是简单地重复主语。

· 不要过分担心“字数”。

· 如果文本用引号括起来并且有一个显示它的地方，你可以将文本添加到图像中。你可以提示超过一个主题。⁠

· 你可以像与 ChatGPT 交谈一样与 Midjourney 交谈。⁠

· Midjourney V6 能够理解标点和语法的微妙之处。（例如，“一只熊猫吃，射击，然后离开。”）

· 你可以使用 V6 制作漫画，只需设置面板，然后为每个面板提示一个简单的线条。纵横比必须与面板数量一样宽。

· 现在一些自然语言的否定形式也有效，尤其是“no”。你可以通过描述来为图像添加框架或边框。

⚠️ 我们还不确定 ::（即多提示）在 V6 中如何工作，但我们正在调查中。

![](img/3ff56581b426f79f53edabdd479c7204.png)

** **Midjourney 对话 V6 在 Midjourney 中与 V6 对话是一种新的体验。以下是一些关于如何与 Midjourney 进行有效对话的要点：

**不是默认设置：** 在 /settings 中选择 V6 以激活新版本。

**清晰简洁的语句：** 努力使用简单、清晰的英语句子，确保拼写和标点符号正确，这有助于更好地理解您的提示。

**自然语言的使用：** 可以像与 ChatGPT 一样对话，尽量将指令写成直接的 /imagine 格式，而不是使用复杂的 remixed 指令。

**理解标点和语法： **V6 能够理解标点和语法的微妙之处，因此您可以更自由地表达您的创意。

**多主题提示：** V6 允许您在一个提示中涉及多个主题，这使得创作更加灵活和富有变化。

总体而言，V6 提供了更直观、更灵活的对话方式，允许您更自**由地****与 Midjourne****y 进行交**流。

示例你不能告诉 Midjourney“<URL（URL是垫图的链接地址）>，但要将其制作成油画”或使用remix 并告诉 Midjourney “但在她头发中加入更多的花朵”。但是，单个直接的/imagine可以像一条指示一样编写，它可能会很好地发挥作用。

![](img/adbd04b75db08e6a8f1e8fd770b723fa.png)

传统的 midjourney 风格描述A raven perches on a dead branch, in the stvle of a gothic charcoal sketch on antique paper. --v 6 一只乌鸦栖息在一根枯枝上，仿照古色古香的纸上的哥特式炭笔素描。--v 6

**ChatGPT 风格描述**

Create a charcoal sketch of a raven. The sketch should look like it's antique paper Don"t include anv leaves on the branch, because it's a dead branch. Make it look gothic.

用炭笔画一只乌鸦。草图应该看起来像一张古董纸，不要在树枝上画任何叶子，因为这是一根枯枝。让它看起来像哥特式的。

Midjourney V6 能够理解标点和语法的细微差别

![](img/c6b40431a4793101b8f329179c888691.png)

A panda eats, shoots, and leaves. --seed 189189 --v 6

熊猫会吃、会芽、会叶

A panda eats shoots and leaves. --seed 189189 --v 6

熊猫吃嫩芽和树叶。

** 您可以使用v6制作漫画 **

点击这些以展开到全尺寸。只要你的纵横比合适，你就可以将简单的场景组织成面板。

![](img/7015f2e88b22e71e7a9c33a66913588e.png)

A comic book page has three panels. In the first panel is a boy making a sandwich in the evening in a modern kitchen. In the second panel, the boy is sleeping in his bedroom at night. In the third panel, an owl perches in front of the bedroom window at night.

漫画书的一页有三个嵌板。在第一幅画中，一个男孩在一个现代化的厨房里做三明治。在第二幅画中，男孩晚上在卧室里睡觉。在第三个面板上，一只猫头鹰在晚上栖息在卧室的窗户前。

![](img/8c02bbb10bcf7a7af4df5867e4015dc0.png)

A comic book page has three panes. The first pane depicts the moon,the second pane depicts a tree, and the third pane depicts a bedroom.

漫画书的一页有三个窗格。第一个窗格描绘了月亮，第二个窗格描绘了一棵树，第三个窗格描绘了一间卧室。

一些自然语言中的否定形式现在可行

参考上方 Midjourney V6 对话，记住，即使在这样的提示中，一个正确的元素也是很惊人的。

![](img/c3a9e27fbde75da446fae3121bc09428.png)

A boy is in a parking lot holding car keys, but there are no cars nearby. --v 6

一个男孩拿着车钥匙在停车场，但附近没有车。

您可以通过描述图像来为其添加框架或边框

Clipart of a lamp on a blank background. The image has a thin gold border or frame around it.空白背景上的灯剪贴画。图像周围有一个薄薄的金色边框或边框。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>