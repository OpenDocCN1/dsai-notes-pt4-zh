
# Midjourney AI 新手秒变高手

> 来源：[https://aigcreative.feishu.cn/docx/Pi64dAXg1opuNoxakzvct3mMn9e](https://aigcreative.feishu.cn/docx/Pi64dAXg1opuNoxakzvct3mMn9e)

> 来源：👸号：RaDesign，作者：五花肉

> 链接：https://mp.weixin.qq.com/s/VtzGAqHdenFq4wgobxgUXQ

## 前言

最近组建了一个🛰️群，群里面的小伙伴都在自学，氛围感满满，整理了这几天常见且有效的公式和一些探索中的关键词。同时看到很多很多要付费学习的，那么我个人觉得没必要，在去年的UI中国平台上发了一篇相关文章，也就是9月21日，将近五个多月，要说这个有多难，我没看到，外网有很多免费教学，国内现在也有很多，B站上可以学。我特意自学了两天，出了这一篇文章，因为我看到教学的基本都是这些关键词。那么今天教大家三个通用公式（角色人物通用公式/3D类通用公式/插画通用公式且进阶教程）和一些关键词，可以自己练习一下。

## 正文

### 一. 通用公式

#### 01. 角色人物公式

人物姓名（命名）+描述词（重要描述词和人物特征描述词）+风格+官方命令词。

例子：进击巨人里（特定来源）贝尔托特（人物姓名）Q版形象（重要描述词），风格是迪士尼或者皮克斯的（风格），且小巧可爱（描述）。（官方 `--ar 16:9`（比例） `--V4`（版本格式）这里我没有加）

整理成英文：
`Attack on Titan, Bertolt Hoover, full body, isometric, 3d, cute, Little adorable chibi graphics, disney pixar Style, cute mobile game style, Hyper Realistic, cinema lighting`

翻译：Attack on Titan（进击的巨人），名字：Bertolt Hoover，全身立体，等距，3D，可爱，小巧可爱的卡通图形，迪士尼/皮克斯风格，可爱的手机游戏风格，超现实主义，电影般的灯光。

![](img/1a7fedd03145a8b718f006e283611d55.png)

注意：当这个人物名字大众不熟知，或者是你的朋友（无中生友的时候）需要喂图+人物取一个姓名来固定这个角色。（目前 `V4` 版本）重要关键词为：`chibi` 中文意思: Q版。

#### 02. 3D 公式

主体+描述词（重要描述词和物体特征描述词）+风格+灯光+官方命令词

例子：我想要一个白色背景的（描述词） `3D`（风格）灯光柔和的（灯光）太空飞船（主体）（官方命令同上）。整理成英文：`spaceship isometric, 3d, soft shadows, white background, soft lighting -- v 4`

翻译：太空飞船，等距离3D设计，柔和阴影，白色背景，灯光柔和，当前V4版本

![](img/a240e57af03f5493ea4874e1ae52e335.png)

关键词：`isometric` 中文: 等距的

#### 03. 插画公式

主题描述词语+风格（风格名称或画家）+描述+颜色灯光+官方命令

例子：骑电动车的小女孩（主题）美丽的宫崎骏动画场景（画家风格）东京的雪景城市，巨大的宫崎骏云朵在蓝色的天空中飘荡（描述），迷人的雪景东京城市，有着刺眼的阳光（灯光）宫崎骏的风景画面（特定的风格颜色），日本艺术（风格）。`--ar 16:9`（比例） `-v4`（版本格式）

整理成英文：
`The little girl riding an electric scooter/bike, in a beautiful anime scene by Hayao Miyazaki: a snowy Tokyo city with massive Miyazaki clouds floating in the blue sky, enchanting snowscapes of the city with bright sunlight, Miyazaki's landscape imagery, Japanese art -- 16:9 --v 4`

翻译：骑电动车的小女孩，美丽的宫崎骏动画场景：东京的雪景城市，巨大的宫崎骏云朵在蓝色的天空中飘荡，迷人的雪景东京城市，有着刺眼的阳光，宫崎骏的风景画面，日本艺术。

![](img/b6123cd832e3b524c14faf0cc97111b9.png)

关键词：艺术家风格，这里是 Hayao Miyazaki 中文：宫崎骏

注意：风格可以多加，一样或者不一样的都可以，比例格式一定要对，否则官方命令不会执行。`--ar 16:9` `--v 4`（很显然我上图的比例格式没对，你们要注意）

### 进阶版

如果想要难度加大的，可以学一下角色连续且场景变换的。需要两个公式：一个角色公式，另一个木偶公式。

#### 角色/场景公式

上传图片（喂图：文章下方有解释）+人物描写+场景风格描写+官方命令。

上传图片：人物背景需要干净，清晰。

人物描写：给角色起名字，形容它是一个什么样的角色，（注意：融入其他场景需要关键词 `white background`）

场景风格描写：这里可以加同类型风格，颜色，以及场景描述。

官方命令：`--v N --aspect x:y` 这里 `-v 4`（本版本）`aspec` 比例填写。以上只是第一步，生成角色图。第二步生成场景，公式和角色公式一致。只是把角色描述换成场景描述。且风格要一致。

第一步/第二步例子：玛丽，她穿了一件绿色皮夹克，黑色牛皮裤，拥有黑色直长发，白色背景（`fwhite background`），宫崎骏风格，新海诚风格，平面插画，动漫，少女风格，官方命令

翻译：
`Mary is wearing a green leather jacket, black leather pants, and has straight black hair against a white background，white background，Miyazaki-style, Shinkai-style, flat illustration, anime, and girlish style，--ar 3:2 --v 4`

![](img/aab3fea08492e172dbfee72287cf85d0.png)

![](img/713727cb8bbc4b42db040454c67a658e.png)

场景：在东京的地标性建筑前，宫崎骏风格、新海诚风格、平面插画、动漫和少女风格。

翻译：
`In front of a landmark building in Tokyo，Miyazaki-style, Shinkai-style, flat illustration, anime, and girlish style，-- ar 3:2 v 4`

![](img/4258d619359d177d25276819526f06a5.png)

![](img/e6e285aacb48155d2885f2dc2b01fa06.png)

注意：这里生成了一张有人物的背景，不需要管，在人物和背景合成后会消失。

第三步：复制刚才生成两张图的链接，一张角色图，一张背景图，粘贴到输入框里，记住上传图片链接之间要空格。复制刚才角色公式的描述，把 `white background` 改成 `walking in a futuristic cyber-punk city`（结合场景），出图。

第三步例子：根据上面的步骤，复制人物的全部描述即可，然后把 `white background` 改成 `walking in a futuristic cyber-punk city`输出到满意为止。

翻译：
`Mary is wearing a green leather jacket, black leather pants, and has straight black hair against a white background，walking in a futuristic cyber-punk city，Miyazaki-style, Shinkai-style, flat illustration, anime, and girlish style.`

![](img/6dfe670376ad03ebe8866207a714da6a.png)

第四步：角色连贯--需要木偶公式：人物图片+场景+人物+动作+风格+官方命令

木偶公式地址：
`https://docs.google.com/document/u/1/d/e/2PACX-1vRahIr3-h_V31w2dMZXTAV9R2aXMB9Yoi-bIMQsRIanMXUO9rDtj5x7cL__hG05bmow-GvP7vcLlAcX/pub?utm_source=docs.google.com&utm_medium=tutorial&utm_campaign=midjourney`

人物图片：上传复制粘贴链接（喂你上面生成的人物图）

场景：`setting is place::` 让角色带入环境/场景，直接在 `place` 替换上填入场景

人物：`name is description::` 这里 `name` 是刚才角色起的名字，替换即可，`description` 是形容你角色的外表，替换复制刚才角色的全部描述即可。

动作：`Setting contains name doing activity::` 这里 `name` 同样替换角色的名字。`doing activity` 可以替换动作和场景。

风格：`atmosphere, filler details, media type, artists` 替换你刚才全部的场景风格就可以了。

官方命令：`--v N --aspect x:y` 这里 `-v 4`（本版本）`aspec` 比例填写。把这些命令修改过的再次放到输入框生成。如果想要改动场景和动作，按照上面替换就好了。

第四步例子：套上面木偶公式的模版

翻译：
`setting is outdoor location with hot springs:: Mary is wearing a green leather jacket, black leather pants, and has straight black hair against a white background:: Setting contains Mary doing activity::Miyazaki-style, Shinkai-style, flat illustration, anime, and girlish style -- v 4 --aspect 3:2`

![](img/0b963055acf2167eb991f4713e169907.png)

到这里就已经成功了，如果觉得不可以，可以拖到ps里修改一番，如果还想在深一步的话，去上面木偶公式地址里，有让脸型有变化的公式。

### 二. 关键词

#### 01. 转换关键词

`"A" AS "B"`

注意：A可以是人名，可以是动物，也可以靠你想像的描述物体。B是指环境，场景等。例子：工作中的绵羊翻译：`Sheep as at work`

![](img/bf4c37ad9954ee4dd0dc0ec1430f8198.png)

例子：扫茅厕的扎克伯格（在社群的知道，我开始用的杰克马，担心举报就拿扎克伯格开刀吧）

翻译：
`Zuckerberg as sweeping the toilet`

![](img/5ae512e834f9dd8f6418c1cd9a5e2932.png)

#### 02. 设计概念

`Knolling+描述的物体`

注意：可以理解为把物品拆分在整齐排列

例子：钢铁侠的设计概念（拆分）

翻译：
`knolling Iron Man suit 8K`

![](img/fe4a8bc218fe1737e6c6d40509c4d881.png)

例子：引擎的设计概念（拆分）

翻译：
`knolling engine 8K`

![](img/337adc26d860afde1cb15e64c7a7fa63.png)

#### 03. 像素风

`8 bit game pixel art +描述的氛围+场景` `16 bit game pixel art +描述的氛围+场景`

注意：`8 bit game pixel art` 的像素风是80后乃至90后的游戏机上的画风，`16 bit game pixel art` 也是，但要比 `8 bit game pixel art` 更细腻一些。

例子：像素风的生动春节

翻译：`16 bit pixel art lively Spring Festival.`

![](img/244188bbd607ef2d7d1ad348612f17d8.png)

例子：8位像素艺术的科技太空飞船舱室

翻译：`8 bit pixel art Techy spacecraft cabin.`

![](img/a1404064bb06ff00ed974fa41264a5.png)

#### 04. 折纸艺术

`Layered Paper+添加任意词汇`

注意：具备3D折纸的艺术形式

例子：下雪的冬天 折纸艺术

翻译：`Layered Paper Snowy Winte`

![](img/8792189306459cf21032b678be2ac830.png)

例子：大连的海滩折纸艺术

翻译：`Layered Paper Dalian Beach`

![](img/8cfd27da51faadbe515b3c2b2bc844da.png)

#### 05. 立体等轴/等距

`isometric+物体描述`

注意：描述的物体越准确，出图满意率越高。

例子：带墨镜的橘猫等距

翻译：`isometric orange cat with sunglasses`

![](img/6a13a730bfa92fd045656719922d1354.png)

例子：中国古庙建筑内饰，等距离视角的3D效果，柔和的阴影和光线，温暖的色彩。

翻译：`Ancient Temple in the Middle Age, living room, isometric 3d, soft shadows, soft light, warm colors, 4k, 8k`

![](img/9e64adb6e2e795bdb52f1c7fd16bc42a.png)

例子：等距离视角，3D效果，一个带有小露天温泉老式日本旅馆。

翻译：`isometric`, an old Japanese ryokan with a small open air hot spring, 4k, 8k

![](img/7fa394f4c3e8f4a43d2580537af858e1.png)

#### 06. 吉祥物标志（logo）

`Mascot logo+描述词`

注意：`logo` 可以单独拆分出来，比如扁平的 `logo`，3D `logo`。

例子：吉祥物熊猫 logo

翻译：`Mascot logo panda`

![](img/aa77f8689c86487bb6dae4f8736f88ef.png)

例子：3D 熊猫 logo

翻译：`3D logo panda`

![](img/f62a28b9dd5140adebcc4fad846c467a.png)

#### 07. 纹身设计

描述词（人物名/动物/植物/有故事的叙述）+`tattoo design`

注意：人物类的会有皮肤背景，可以加上背景描述。

例子：蝴蝶纹身

翻译：`butterfly tattoo design`

![](img/a49feda713c2d858491b91a80474864a.png)

例子：简笔画风格蝴蝶纹身

翻译：`Stick figure butterfly tattoo design`

![](img/47594b67e7176132c733d49daa278c47.png)

#### 08. 真实照片图像

`Photorealistic`（用在多文字内容描述中）

注意：简单描述+`Photorealistic` 也是可以的，但是精准度随机。

例子：蒸汽火车真实图像

翻译：`Steam train Photorealistic`

![](img/0ef8b8f4e3092a56dfa5bfc8980ed02d.png)

例子：带灯光的UFO

翻译：`ufo with lights photorealistic`

![](img/fd78475f16a86f46fe02517872bf9b3e.png)

#### 09. 贴纸风格

`Design a Sticker of+任意描述词`

注意：多词描述时可直接用 `-Sticker+chibi-`（开头讲过的Q版）

例子：小浣熊贴纸

翻译：`Design a Sticker of in English is "raccoon."`

![](img/82980b0b4590f0b1ea932ec7b8c8f27b.png)

例子：可爱的卡通狐狸拿着复活节彩蛋的贴纸，卡通人物风格，白色背景。

翻译：`cute cartoon fox with an easter egg - Sticker - chibi style - white background`

![](img/abecbed7007ebd42055eb12e1f8b6bed.png)

#### 10. 物体和材质融合

`物体+out of+材质`

注意：完整句型是 —`made out of`—，语意是 A 物体被 B 材质覆盖。



例子：3C材质，磨砂质感，大小不一的球体，覆盖在运动鞋上。

翻译：`athletic shoes out of spherical objects of varying sizes with a frosted texture made of 3C material，4k，8k -- ar 16:9`

![](img/842ada43f75db4c5c1da9b404728a37b.png)

例子：带着火焰的机甲

翻译：`mecha out of flame`

![](img/17d137c2c33de3fe20ee151e282bcf26.png)

## 11.儿童艺术（有待探索）：`naive art`+描述关键词

注意：这属于特有风格，淳朴艺术。

例子：儿童机甲

翻译：`naive art mecha`

![](img/5583c1ace3d039e958af26b38fb0451f.png)

例子：天空中飞翔着的海盗船（明显没飞起来）

翻译：`pirate ship flying in the sky naive art`

![](img/4c5ba298ae3b27e7e02f14cb17d24433.png)

## 12.怀旧风格（赛博）：描述词+`Synthwave`

注意：此描述词可以通过喂图的方式，把人物变成怀旧风。

例子：80年代少女怀旧风

翻译：`girl from the 80s synthwave`

![](img/07213069235d3ea8278e1a4e670c825d.png)

例子：90年代汽车

翻译：`cars from the 90s synthwave`

![](img/a6b9a21cdc49a218c8580361b932ff74.png)

## 13.物体拟人化：`Anthropomorphize`+描述词

注意：没有注意事项。

例子：开心的萝卜在喝萝卜汤（显然它并没有喝，还装起来了）

翻译：`Anthropomorphize the happy carrot is drinking carrot soup`

![](img/4bb89301dd21723d8bcf6b5b6e0e596d.png)

例子：有一双大眼睛，咧嘴笑的汽车

翻译：`Anthropomorphize car a car with big eyes and a grin on its face`

![](img/1ecd4aa3b4332f8d09a2314348efd833.png)

## 14.设计类软件：描述+`blender 3D/C4D/OC`

注意：这几个包括以上这些的关键词，在`3D`类里都可以尝试，但加太多也会导致让这个`V4`版本随机率加大。（`V4`版本不要试手套/手指/眼睛，其他都可以）

例子：材质为`3C`的打印机，背景为白色，`blender 3D`

翻译：`A 3C printer with a white background, blender 3D`

![](img/b03643e1a74bcea198c86ac33b1f9ded.png)

综合以上的部分关键词组合，看看能得到什么。

例子：小巧可爱的等距离办公室，柔和的光线和粉彩色调。`3D`图标粘土渲染，使用`Blender 3D`软件，粉色背景。

翻译：`Tiny cute isometric outside of officeemoji, soft lighting, soft pastel colors.3d icon clay render, blender 3d, pastelbackground`

![](img/8ba87452f63f8e65d65439c2f2ac3f58.png)

## 15.妖精系：`Fairy Kei fashion`+描述关键词

注释：来自某岛国，80年代漫画。

例子：迷路的小公主（并没有迷路，什么时候拿的蛋糕）

翻译：`Fairy Kei fashion The Little Princess Who Got Lost`

![](img/3abc8c7baf33d8b2f9bf4ed41c638578.png)

例子：美少女 手拿电棍（电棍为什么变成银光棒了，什么鬼）

翻译：`Fairy Kei fashion Beautiful girl holding an electric baton`

![](img/34f53d463ed35e42fc908860055f906a.png)

### 注释：什么叫喂图

答：把自己找到的参考图（一类或者一组风格的）拖动到Midjourney对话框内，回车上传，上传后点开图片，左下角 “在浏览器打开”，打开后复制图片链接。格式：`/imagine prompt:` 粘贴在输入框+空格+描述的关键词（图片格式仅支持`jpg/png`）

### 什么叫官方命令词

答：让画质更好用且常用的官方命令。官方手册：`https://docs.midjourney.com/docs/models`

### 公式结构是否固定。

答：公式结构目前没有固定，除特殊（木偶公式）`V5`更新后可能更方便。

### 现在还能用bug无限次数嘛

答：可以，不可以的小伙伴们有两个问题，1.没按步骤操作，比如客户端没有下载，没有打开无痕模式下操作。2.高科技需要把全局打开。

### 生成的图不满意，怎样修改关键词描述

答：输入官方命令 `/settings` ，第一次为开启，再次输入是关闭，开启后找到像筛子的图标点成绿色（`Remix mode`），则开启修改。

### 这些关键词从哪里知道的

答：`YouTube`上很多免费教程，一位叫 AI绘画 的博主 提供了怎么找关键词，方法是：去`promptbase`网站，找相应画风的标题去看2～3次，总结出相应的关键词。好，今天就教大家这些，让我们看看我的社区里这几天都有哪些有意思的作品。

## 三.社区成员作品

`Richer face, round face, more natural action, smile,with round glasses, Disney Princess, lovely cartoon girl, glowing stars and white roses around her, 3D, light background, C4D, IP character, Blender, bright color, 8K, HD`（社区成员-foodhungry）

![](img/388b59f8ed3a84bec692c1860af616bd.png)

`Richer face, more natural action, Disney Princess, lovely cartoon girl, 3D, light background, C4D, IP character, Blender, bright color, 8K, HD`（社区成员-nevergiveup）

![](img/fe9fb2d2c7ce643bde856cac637c0200.png)

`c4d,cute,normal`（社区成员-tuotuo）

![](img/3562fc2b0712997a8e863a1974ac88ba.png)

`Female wearing cyberneticexoskeleton character design,concept design sheet, whitebackground, style of yoji shinkawa`（社区成员-121212 ）

![](img/386a44120b9d4457abf5699094efb4de.png)

`landscape with the Nile river in Egypt as in a computer game --ar 2:1`（社区成员-121212）

![](img/f5776813c6fba43c8b8ea243c1fb5749.png)

`illustration cat wear flowers and summer clothes ::5, 2D render, vector --v 4`（社区成员--121212）

![](img/663d1a32cb7f38af1ab77655086129d2.png)

`Dribbble illustration style, primeval forest, daytime, sunny, green trees, blue sky, cute chicks`（社区成员-糖果虎8）

![](img/f53bec54e0ac53d83ce64ef65016f32d.png)

`andscape,illustration,Korea style,4k,green`（社区成员-tuotuo）

![](img/43345d7b36886ff76d7a187a3b29edb9.png)

`refined painted anime girl live in real world photograp, xianxia, hyper detailed, anime girl mix realism, best quality, master piece, 4k 8k high res::3 --ar 16:9 --no 3d render real face`（社区成员-把你美得卡斯兰 ）

![](img/59c6e36646df9d45262d051fe330fc51.png)

`Cyberpunk style, epic, grand, strong impact, panoramic, Chinese style Chinese elements, woods, pressure, close-up, Tiger Girl, 8k --ar 16:9` （社区成员-1122233 ）

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>
