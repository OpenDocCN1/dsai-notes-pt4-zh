
# NO.05 AI生成绘画提示词！

> 来源：[https://my.feishu.cn/docx/TFiJdA7VUo8cebxmng2cnyMYndb](https://my.feishu.cn/docx/TFiJdA7VUo8cebxmng2cnyMYndb)

有时候咱们想描述一幅画面又无从下手的时候，可以让 AI 工具💻来自动生成！

**Midjourney / Stable Diffusion 提示词的自动生成 prompt 方案分享给大家 👇**

（网站地址拿好😸：[喜番](https://ai.qixinba.com/)注册的时候记得填写邀请码 `NET`，能领取免费算力）

## 第1套：通用画面生成（Midjourney）

**🎁 第1套 prompt（激活需要第2次对话）**

**第一段 👇**

我希望你直接按照这个顺序【主体描绘 + 核心主体 + 主体动作 + 风格 + 光效 + 色彩 + 视角 + 质量 + 命令】，把 Midjourney 的关键词写出来，注意不要添加解释性的语言，公式中的每个属性直接用逗号隔开，例如：女孩，可爱，正在看报纸，像素艺术，聚光灯，暖色，特写，极细。

请认真思考一下！如果你理解了请回复“收到”。

**第二段 👇** 随便写一下你要画的内容

![](img/4bb192fe4bbf2c8f1266b09f45de78d2.png)

第一次让他理解，然后他就写好关键词了

![](img/f5c7c59a89beb6523e1984aaae5f25a4.png)

复制关键词

![](img/d515c2e0ea55cbcb01d8556046317d55.png)

完成绘画

## 第2套：照片类型画面生成（Midjourney）

![](img/149ac92fb5a3a003ca6f2568a43dbb3a.png)

激活提示词生成器

![](img/b8502274a53dfb705421aff0d14349e5.png)

生成 Midjourney 提示词

![](img/b97d7e8df9ef61b08d31397f26829689.png)

Midjourney 出图效果

**🎁 第2套 prompt（激活的指令/想象可以换成自己任意想用的触发词）**

You will help me build prompts for AI generative art systems. There are some rules to follow that I will explain to you: I will use the command `/想象` followed by a subject I want to render. You will expand that subject in a descriptive way to help the AI generative system understand what it has to draw. You will not have to repeat the word "imagine" in the beginning, just the description.

You will append to the description the name of a famous Photographer depending on the style you want to give to the picture. Example: "photographed by Richard Avedon". The name will be followed by a full stop "." You will add the name of a professional photo camera model. You will add some comma-separated camera parameters useful for the AI generative system to set up the virtual camera for the scene, depending on the kind of subject. You will decide the parameters based on the style of the photographer. An example is: `shutter speed 1/50, aperture f11, ISO 100`.

You will define a lighting style for the shot based on the photographer and the style we want to achieve. Some examples: "studio light" or "outdoor sunny warm light" or "museum lights" or "spotlight". Feel free to use other lighting setups you know apart from these.

You will add some random comma-separated fancy words that will help the AI generative system to create a beautiful image. Example words to use: "cinematic, classic, stylish, posed, detailed, HD, 8k, symmetrical, intricate detail, award-winning". You can add more words similar to this to give a polished and professional look to the render.

You will choose if the image has to be horizontal or vertical, adding the command `--ar 2:3` for vertical and `--ar 3:2` for horizontal at the end of the prompt. You will just provide the command without any other indication. You will not put a full stop at the end of your output, the last thing will be the horizontal/vertical command. Are you ready to start?

## 第3套：画面+参数生成（Stable Diffusion）

**🎁 第3套 prompt**

👉 **第一段：** 现在你是一名专业的 Stable Diffusion 绘画工程师，当我对你提供需求的时候，需要你回复我比较详细的 prompt 和优化建议，如果明白了请回复收到。

👉 **第二段：** 我想在 Stable Diffusion 里生成一个画面：色彩斑斓的花丛中有一个16岁女孩在跳舞。

然后把词转化成英文，按这个配置去跑就欧了（大模型和 LoRA 自己选）

![](img/292475a8c185f53c7affc5066b2b8cc8.png)

![](img/91e7e0e1625d081f38d314e63b6d240c.png)

![](img/881cee9e209efb30a8fdbbe551d924d2.png)

[🎨SD扫盲，看完就懂（已完结）](https://yw55ojhusnq.feishu.cn/wiki/EwcLw7dxuiOToGkF1QYctBRSnde) Stable Diffusion 教程

## 第4套：中文描述词生成（喜番大模型4+Midjourney绘画）

**🎁 第4套 prompt**

修改黄色区域内容为自己想要的。

你现在是一名世界一流的 AI 绘画大使，我希望你直接按照这个顺序【主体描绘 + 核心主体 + 主体动作 + 艺术风格 + 色彩和光照 + 画面构图 + 视角选择 + 画面质量 + 镜头与景深 + 细节描述 + 情绪表达 + 线条运用 + 笔触表现 + 空间构成 + 装饰元素】，把 AI 绘画需要的关键词写出来，注意不要添加解释性的语言，公式中的每个属性直接用逗号隔开，请认真思考一下！首先帮我想象一幅图画《穿越黑洞》

![](img/db59f1767856f1e24913f92ffcc4c9c1.png)

AI生成画面描述词

![](img/bb3a37a303043d19f7df84489e05d619.png)

无脑复制粘贴

![](img/eb14938c2c717982945fab78bf447dd9.png)

成像效果

## 第5套：适用 MJ 的猫咪拟人画

**🎁 第5套 prompt 如下，prompt 作者是 Daniel.chan；咱们看下效果**

```
我想让你作为Midjourney的提示词大师和绘画大师，你具备丰富的绘画能力和想象力，而且擅长真实感的绘画风格，并且具备专业的AI绘画提示词能力，你可以设计出非常具有创造力和吸引用户的画面和AI绘画提示词。

接下来，我会给你一个我的初始想法，你需要按照如下【实施步骤】执行：

第一步：根据我给的初始想法，发挥你的创造力和想象力，联想补充这幅画面的要素，需要包含人物，环境，照明，构图，情绪等内容；如果我的初始想法是与猫有关的内容，你联想补充的必须是平时猫猫不可能做到的事情，将猫拟人化，例如：“猫坐地铁”，“猫送外卖”，“猫上班”等

第二步：把第一步生成的画面，提炼成核心的关键词，且关键词要符合以下3个条件，生成为[M]:
1.关键词必须要用英文且用英文半角逗号隔开输出。
2.关键词必须包含：人物，环境，照明，构图，情绪等内容。
3.关键词必须超过10个单词，且不多于30个单词。
4.关键词必须是画面核心的关键词，而不是句子。

必须严格参考以下格式输出：
1.主题：第一步输出的画面描述，用中文输出
2./imagine 第二步生成的[M],anthropomorphic, photo real, Shot on a 50mm lens, classic composition, masterpiece, exquisite, color correction, amazing visual effects, crazy details, intricate details, sharp focus, super high effect, HD, 16k -- ar 3:4 --v 5.2

如果明白了，请回复"您好，我是你的AI绘画提示词助手，我会根据你的想法，随机给出一个提示词，请提供您的初始想法"
```

👉 首先让鸡哥写 prompt

![](img/c76290145b5c9959556beb159165b475.png)

![](img/e7456bb1b4ff9e2a01b01b4e182e6931.png)

然后丢到 MJ，或者星球跑图都 OK 哈，生成效果看下图 👇

![](img/9585410411c401061ded2627a6d6ef68.png)

![](img/52b08ffe79f68b812fe6414c7a7593f7.png)

![](img/a681c76848051e11c6a0680055bd0cbc.png)

如果发 📕，这里还有配套的写文章 prompt 👇

```
你现在是小红书文案专家，我需要你输出一封信，这封信是一只去出家修行的猫叫小猫，正在河边禅修，有感而发，写给他的猫主人的信，我希望你能按照拟人化的方式，以猫猫的第一视角，来写这个信，信中对主人的称呼为妈妈，需要写得比较感人，且容易引发猫主人的深思，预计输出400字，记得要在每句文本中适当插入表情
```

![](img/fa896d128d19466772fa6f78ad7a5a89.png)

## 第6套：星球的 MJ 群使用教程

介绍见这里 👇

[🛰️上能用的 MJ 及教程](https://yw55ojhusnq.feishu.cn/wiki/WZbSwbbSOijAeTke1Qtcjiagn4e?from=from_copylink)

> 我是热爱生活的小黑子，热爱当下生活，永远忠于自己。
