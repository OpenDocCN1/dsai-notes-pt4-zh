
# Sora资料库【持续更新】

> 来源：[https://xmgtoagi.feishu.cn/docx/WEOzdSGbuozoVBx9qEScrL0xnvb](https://xmgtoagi.feishu.cn/docx/WEOzdSGbuozoVBx9qEScrL0xnvb)

> 作者：量子位

## 一、OpenAI Sora横空出世

2024年2月15日，OpenAI宣布推出全新的生成式人工智能模型“Sora”。

据OpenAI官方介绍，Sora能够根据用户的一句话生成长达一分钟的1080P高清视频，包含富有情感的多个角色、特定类型的运动以及主题和背景的准确细节的复杂场景，几乎达到了电影级别的逼真程度，视频的流畅度和稳定性皆在水准之上。

S

### 背后的研发团队

现在世界上最受关注的技术团队是哪一支？

**Sora团队**，已经来到聚光灯中心。

不仅项目负责人评论区被挤爆，成了𝕏最“景点”。

![](img/004e9aae0966de7fcefadf0e3f69b5dc.png)

天才成员们的履历，也正在持续引爆关注。

![](img/bc8a62e0a95e571ee9a2563658dd3cc4.png)

大家伙发现，这支团队挺年轻

两位负责人都是在去年（2023年）刚刚博士毕业，团队里甚至还有00后选手……

![](img/e35992ba4e9d2ef56b7d198bb9a5d32f.png)

但也是真的牛：

【Tim Brooks】

DALL·E 3作者之一，GitHub 5.7k️

项目`InstructPix2Pix`作者

2021-2022年在英伟达实习时，就是视频生成研究的项目负责人。

【William (Bill) Peebles】

和谢赛宁合作，搞出了Sora的技术基础之一`DiT`（扩散`Transformer`）

论文还曾入围CVPR 2022最佳论文候选。

……

这支团队到底什么来头，咱们一起仔细聊聊。

#### （1）应届博士带队

包括Tim和Bill在内，Sora的主要负责人一共有三名（以下排名不分先后）。

**Tim Brooks**，也是DALL·E 3的作者，2023年1月刚从加州大学伯克利分校博士毕业。

![](img/abe619d79be35ec6e3dfff4197381113.png)

+   Tim本科就读于卡内基梅隆大学，主修逻辑与计算，辅修计算机科学，其间在Facebook软件工程部门实习了四个月。

+   2017年，本科毕业的Tim先到Google工作了近两年，在Pixel手机部门中研究AI相机，之后到了伯克利AI实验室攻读博士。

+   在伯克利读博期间，Tim的主要研究方向就是图片与视频生成，他还在英伟达实习并主导了一项关于视频生成的研究。

+   回到校园后，Tim与导师Alexei Efros教授和同组博士后Aleksander Holynski（现在谷歌）一起研制了AI图片编辑工具`InstructPix2Pix`，并入选CVPR 2023 Highlight。

![](img/51bd17130dd8c110ee2cc18ecfa5f615.png)

+   2023年一月，Tim顺利毕业并取得了博士学位，转而加入OpenAI，并相继参与了DALL·E 3和Sora的工作。

值得一提的是，Tim不仅在专业领域拥有高超的技术水平，还是个多才多艺的人。

据Tim自己介绍，他还喜欢摄影和音乐，高中时他拍摄的照片获得过National Geographic颁发的奖项，本人到过百老汇演出，还获得过B-box国际奖项……

![](img/62c54cbdd8e2b868b05fca3f120b3fe8.png)

而与Tim师出同门、晚毕业4个月的William Peebles，也是Sora的另一名负责人。

（Peebles在𝕏上用昵称Bill，在Linkedin上及论文署名时用大名William，下文一律用Bill指代。）

![](img/ee3ea946446108900678a81a355507b1.png)

+   Bill本科就读于MIT，主修计算机科学，参加了`GAN`和`text2video`的研究，还在英伟达深度学习与自动驾驶团队实习，研究计算机视觉。

+   毕业后正式开始读博之前，他还参加了Adobe的暑期实习，研究的依然是`GAN`，该项目和（时任）卡内基梅隆大学华人学者朱俊彦（也是Efros教授学生，现在在MIT）组有合作，并成为CVPR 2022最佳论文候选。

![](img/739a3c31dbb39ec40da4201bb589e9be.png)

+   之后，学期开始，Bill到了伯克利Efros教授课题组攻读博士，研究成果多次入选SIGGRAPH、ICCV、CVPR等学术会议。

+   2022年5月，Bill到Meta进行了为期半年的实习，和谢赛宁（Bill开始实习时还未离开Meta）合作发表了`DiT`模型，首次将`Transformer`与扩散模型结合到了一起。该成果被ICCV 2023录用为Oral论文。值得一提的是，OpenAI此次发布的Sora，被认为正是基于`DiT`构建的。

+   2023年5月，Bill也从伯克利毕业，并入职OpenAI。

![](img/daaa113a810c680a47068d361972193f.png)

除了这两位去年加入的研究者，Sora团队的另一位负责人Aditya Ramesh则是OpenAI的“老人”。

![](img/c3f03bc54395919d79598826820f42a7.png)

Aditya是DALL·E 的创造者，主导了三代DALL·E 的研究，三个版本的论文当中他都是共同一作。

![](img/587cf1c4ebc2529f71a93f8071afc9cc.png)

而这样一位主导三代DALL·E ，如今又领导Sora团队的大神，却只有本科学历。

据LeCun介绍，Aditya本科就读于纽约大学，并在他的实验室参与过一些项目。

![](img/ea2b3863b9bb35882afccf3d91a3cc69.png)

其间，Aditya就已经在研究生成式模型，并和LeCun共同发表论文。

![](img/802b0f28b870b26584c467ab67360db2.png)

毕业之后，Aditya本想继续深造，但在OpenAI的暑期实习中被留了下来，成为了正式研究人员。

#### （2）00后已加入

Sora团队的本科生，还不止Aditya Ramesh一位。

前文提到，这支团队中有一位“00后”Will DePue，就是2022年才刚从密西根大学计算机系本科毕业的。

![](img/e029ed8515d900bf21f77e5a987e1c17.png)

这位小哥大四的时候创业搞了个市场咨询公司DeepResearch，这家公司后来被Commsor收购。

2023年7月，小哥加入OpenAI。根据他的领英信息，他是在今年1月才刚刚加入Sora项目组的。

另外，David Schnurr和Joe Taylor也都没有博士学位。前者毕业于加州大学圣塔芭芭拉分校，后者毕业于美国旧金山艺术大学。

而正如Aditya Ramesh自己所说，Sora团队的不少成员都是DALL·E 3的作者。

![](img/9c641ed36522bf56ee87a29924d1bc62.png)

包括两位华人Li Jing和Yufei Guo。

Li Jing是DALL·E 3的共同一作，2014年本科毕业于北京大学物理系，2019年获得MIT物理学博士学位。在Meta做了2年多博士后之后，Li Jing于2022年加入OpenAI。

华人作者中还有Ricky Wang，今年一月刚刚从Meta/Instagram跳槽到OpenAI，另外两位Yufei Guo、Clarence Ng没有太多公开资料。

新跳槽来的还有Conner Holmes，他在微软工作时以外援形式参与了DALL·E 3的推理优化工作，后来干脆加入OpenAI了。

来看一眼完整作者名单：

![](img/f636da3761e745ae2aa76bc73ca03068.png)

从团队的组建情况和研究基础来看，Sora应该是OpenAI近半年来的最新成果，而非网传“早已有之但憋着不发”。

不过，Sora炸场，顶级人才又持续星聚，还是惊得众人开始重新考量OpenAI的技术领先性。

作者释出的Sora新作，连“同一场景”下的多机位视频都整出来了。

![](img/265ef29ce200da180e24af9958ee2fc8.png)

网友们的心情be like：

![](img/1abb13b74db67bd52a1c71bce8238657.png)

### OpenAI Sora官网

> `https://openai.com/sora`

![](img/43c39d1705fce8b25e166a76b2d7bde4.png)

### 官方视频DEMO

![](img/af087de7271a39d0933b2aaf45078764.png)

[1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YebBbis5Iof3wJxzysbcWIujnqc)

![](img/fb6a033cfe4e58ef3095494a1c07d32a.png)

[e3e694bf029e8b28c24c6e6777095899.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HzEBbOT55oIBBYxBcmVcunyrnZe)

![](img/bedf5f4bd1edd480ea658f7e69742386.png)

[a2.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/U0gDboCPNoe9HJxUWLIcAmapnMd)

![](img/2d59cce36197fba5c3927405871fcbae.png)

[e1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Dv7NbonNuoJAM4xm9V4czFuQnjf)

![](img/f58a7339cead4cf63325ec8cc962863a.png)

[bcf150368e96422f8bfa958b0f1af9f8.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HImrbkSUboUTMkxSxZAchDl0nKb)

![](img/982bf65efb207e3bc820aef464ffd81b.png)

[bb0836b06cdb965faf0da0a2e34cc474.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/E9GPbr7pjoBa1ux86xacZxXtnvm)

![](img/ef486746fa9d420b2c24e3ad4636e669.png)

[simulation_0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/K817bltsLohUqix4sE0cWooynLf)

![](img/057a3f993b35ff005e365e4360346e1f.png)

[simulation_5.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QLYDbUZ8oom01ox6iNsctCDnnDc)

### 申请内测资格

（1）内部申请路径一

`https://forum.openai.com/`

（2）内部申请路径二

这是一个openai公司公布了一个 Open Al论坛，参与者可以参加Open Al组织的线上和线下活动并且同 Open AI员工深入交流，更有机会可以提前测试一些相关功能(Sora)。

加入条件: 展示出对申请者专业领域和AI交叉领域的浓厚兴趣。 有能力在每个财政季度中投入1小时的时间参与活动。

+   **第一步 登录openai官网：**`https://openai.com`

+   第二步 右上角点击 `Search` 按钮

![](img/fd646ba68bf40b15f7e6223c13da633b.png)

+   **第三步搜索栏搜索** `apply`

![](img/8a0c7e69f195f2e0d27bdc398f0f4393.png)

+   第四步点击 `Pages` 按钮，选择`Red Teaming`

![](img/851dd684626f8577d470d9acf141b11a.png)

**如果页面没有了，试试这个链接：**`https://openai.com/form/red-teaming-network`

+   **第五步 填写表单**

一些基本信息填写，建议全英文，选美国。

![](img/d5a1c176731717f3280ceb7318716f22.png)

表格中很重要的内容是填写你为什么有兴趣加入`Red Teaming`和你认为OpenAI成为红队的一个重要领域是什么（这个问题很拗口，请自行理解）。建议发挥想象力，体现你的专业能力背景和白左圣母心，可让GPT帮忙。

在表单的最后还有推特和领英的链接，如果有的话，尽量附上，对于通过应该有很大的加分，这是真实性背书。

![](img/2a6792a384121dc1a38f8b4a2768e2b8.png)

全部填写完成后，点击`Submit`提交。

可预见的申请数量极大，通过率会很低，放平心态，万一通过了呢。

## 二、Sora竞品对比

### 一图展示Sora与竞品对比

![](img/0fe05cbab822279cace9fde05e3b8f6f.png)

### 主流生成式AI同提示词效果对比

对比Runway、Pika、SVD，OpenAI Sora真遥遥领先

<https://player.bilibili.com/player.html?bvid=18K421k7Pp&spm_id_from=888.80997.embed_other.whitelist&vd_source=03b758d23c4587f27d67124cb89dbad4>

### Sora、Pika、Runway、Stable Video 四个模型对比

可以看到，相比于其他三个视频生成模型，Sora 在生成时长、连贯性等方面都有显著的优势。

![](img/2c3bf7f5bda6bedb8f3f55257b833ed6.png)

> 图源：`https://twitter.com/gabor/status/1758282791547232482`

### Sora与Runway对比

比如输入相同的 `prompt`「一窝金毛幼犬在雪地里玩耍，它们的头从雪中探出来，被雪覆盖。」

![](img/0e66a92b694f21a93f87ee4d9e02127a.png)

> 图源：`https://twitter.com/DailyUpdatesNet/status/1758646902751670355`

再比如输入相同的 `prompt`「几只巨大的毛茸茸的猛犸象踏着白雪皑皑的草地走来，长长的毛毛在风中轻轻飘动，远处覆盖着积雪的树木和雄伟的雪山，午后的阳光、缕缕云彩和远处高高的太阳营造出温暖的光芒，低相机视野令人惊叹地捕捉到了大型毛茸茸的哺乳动物与美丽的摄影，景深。」

虽然 Runway 和 Pika 表现都不错，但 Sora 的生成质量具有压倒性的优势。

![](img/894f9cbc5f8ab646e5d5b6d3cd60a42e.png)

> 图源：`https://twitter.com/keitowebai/status/1758384152670577136`

### Sora与Midjourney对比

![](img/af418fcedd741249937091ac7a4e08c2.png)

![](img/bf5ff7f97915d699501345ab2ba8ea79.png)

![](img/4cc9c5369ecdbf52d1d799451af2db2d.png)

![](img/77cf3918550976cc1485227e90fc644e.png)

## 三、Sora文生视频案例

### 案例一

Prompt:

A stylish woman walks down a Tokyo street filled with warm glowing neon and animated city signage. She wears a black leather jacket, a long red dress, and black boots, and carries a black purse. She wears sunglasses and red lipstick. She walks confidently and casually. The street is damp and reflective, creating a mirror effect of the colorful lights. Many pedestrians walk about.

> 提示：

> 一位时尚女性走在充满温暖霓虹灯和动画城市标牌的东京街道上。她穿着黑色皮夹克、红色长裙和黑色靴子，拎着黑色钱包。她戴着太阳镜，涂着红色口红。她走路自信又随意。街道潮湿且反光，在彩色灯光的照射下形成镜面效果。许多行人走来走去。

![](img/ba69da69b56bd67edb7e7e95dbfe0347.png)



# 三、Sora文生视频案例

## 案例一

[`tokyo-walk.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/M5Pfbdg6LoLWQUxees0c7ZMkn1d)

极其卓越！仅用60秒，实现了从头到尾的单镜头拍摄，而且不仅主角画面稳定，连背景中的人物也展现出了令人难以置信的稳定性，从广阔的中景轻松过渡到面部特写。

## 案例二

Prompt:

Several giant wooly mammoths approach treading through a snowy meadow, their long wooly fur lightly blows in the wind as they walk, snow covered trees and dramatic snow capped mountains in the distance, mid afternoon light with wispy clouds and a sun high in the distance creates a warm glow, the low camera view is stunning capturing the large furry mammal with beautiful photography, depth of field.

> 提示：

> 几只巨大的毛茸茸的猛犸象踏着白雪皑皑的草地走近，它们长长的毛茸茸的皮毛在风中轻轻飘动，远处覆盖着白雪的树木和雄伟的雪山，午后的阳光下有缕缕云彩，太阳高高地挂在空中距离产生温暖的光芒，低相机视角令人惊叹地捕捉到大型毛茸茸的哺乳动物，具有美丽的摄影和景深。

![](img/1e395bf67e76aa61b1d18883aecb2881.png)

[`wooly-mammoth.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/K98Db0jmsotc6nxOdr8c0e8AnwT)

## 案例三

Prompt:

A movie trailer featuring the adventures of a 30-year-old space man wearing a red wool knitted motorcycle helmet, blue sky, salt desert, cinematic style, shot in 35mm film, vivid colors.

> 提示：

> 电影预告片讲述了30岁太空人戴着红色羊毛针织摩托车头盔的冒险经历，蓝天、盐漠，电影风格，35毫米胶片拍摄，色彩鲜艳。

![](img/29e5bef16f50b462ed31aba52baa6aa3.png)

[`mitten-astronaut.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Aw6Vb8MjZo0dZwxuxyscSWZpnzh)

## 案例四

Prompt:

Drone view of waves crashing against the rugged cliffs along Big Sur’s garay point beach. The crashing blue waters create white-tipped waves, while the golden light of the setting sun illuminates the rocky shore. A small island with a lighthouse sits in the distance, and green shrubbery covers the cliff’s edge. The steep drop from the road down to the beach is a dramatic feat, with the cliff’s edges jutting out over the sea. This is a view that captures the raw beauty of the coast and the rugged landscape of the Pacific Coast Highway.

> 提示：

> 无人机拍摄的海浪拍击大苏尔加雷角海滩崎岖悬崖的景象。蔚蓝的海水激起白色的波浪，夕阳的金色光芒照亮了岩石海岸。远处有一座小岛，岛上有一座灯塔，悬崖边长满了绿色的灌木丛。从公路到海滩的陡峭落差是一项戏剧性的壮举，悬崖边缘伸出海面。这一景观捕捉到了海岸的原始之美和太平洋海岸公路的崎岖景观。

![](img/5cec54e388818dae64b8c05bd4848273.png)

[`big-sur.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FuJtbTugAoGTF1xV8KycPlGLnjh)

## 案例五

Prompt:

Animated scene features a close-up of a short fluffy monster kneeling beside a melting red candle. The art style is 3D and realistic, with a focus on lighting and texture. The mood of the painting is one of wonder and curiosity, as the monster gazes at the flame with wide eyes and open mouth. Its pose and expression convey a sense of innocence and playfulness, as if it is exploring the world around it for the first time. The use of warm colors and dramatic lighting further enhances the cozy atmosphere of the image.

> 提示：

> 动画场景的特写是一个毛茸茸的小怪物跪在融化的红蜡烛旁边。艺术风格是 3D 和现实的，重点是灯光和纹理。这幅画的气氛是一种惊奇和好奇，怪物睁大眼睛、张开嘴巴凝视着火焰。它的姿势和表情传达出一种天真和俏皮的感觉，就好像它第一次探索周围的世界一样。暖色调和戏剧性灯光的使用进一步增强了图像的舒适氛围。

![](img/39d5ad6abecdc8406f5f0336dfd4bc19.png)

[`monster-with-melting-candle.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Dr9ebHxbXoh4E7xwdrYc2Q1unzF)

## 案例六

Prompt:

A gorgeously rendered papercraft world of a coral reef, rife with colorful fish and sea creatures.

> 提示：

> 一个渲染华丽的珊瑚礁纸艺世界，充满了色彩缤纷的鱼类和海洋生物。

![](img/39931808a8b0cc70c2627834ff470310.png)

[`origami-undersea.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/H0subj3aKokPxixZUEKcCsnRnSh)

## 案例七

Prompt:

This close-up shot of a Victoria crowned pigeon showcases its striking blue plumage and red chest. Its crest is made of delicate, lacy feathers, while its eye is a striking red color. The bird’s head is tilted slightly to the side, giving the impression of it looking regal and majestic. The background is blurred, drawing attention to the bird’s striking appearance.

> 提示：

> 这张维多利亚冠鸽的特写镜头展示了其引人注目的蓝色羽毛和红色胸部。它的冠由精致的花边羽毛制成，而它的眼睛是醒目的红色。这只鸟的头稍微向一侧倾斜，给人一种富丽堂皇、威严的印象。背景变得模糊，将人们的注意力吸引到这只鸟引人注目的外表上。

![](img/c44689db09983fea9f819456298b16be.png)

[`victoria-crowned-pigeon.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OHIxbz3iYo3Q4vx5lsAc7iScnxg)

## 案例八

Prompt:

Photorealistic closeup video of two pirate ships battling each other as they sail inside a cup of coffee...

> 提示:

> 两艘海盗船在一杯咖啡中航行时相互争斗的逼真特关视频…

![](img/87663ab7957764c41143a16bb52bc222.png)

[`ships-in-coffee.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DbJIbS8oko18eXx7LcFcUO7Pn4b)

## 案例九

Prompt:

A young man at his 20s is sitting on a piece of cloud in the sky, reading a book.

> 提示：

> 一个20多岁的年轻人坐在天上的一片云上看书。

![](img/303f8138d211d6f1e194883dd2c5cac3.png)

[`man-on-the-cloud.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Vf3abqBVXo22ZtxSv75chtSmnQh)

# 四、Sora文生图案例

Sora图像生成能力：Sora能生成图像，虽不及Midjourney，但优于DALL·E 3

![](img/7d41aba87416b4102b8e8c270ea2ead2.png)

![](img/8e99ac76a781bfed2ce5eb97e96c9af2.png)

![](img/c75bb231f4d9eb2e31d0c18af09d08dd.png)

# 五、Sora的应用场景

Sora作为一种文本到视频的生成技术，其潜在的应用场景非常广泛，可以革新多个行业和领域。

以下是一些可能的应用场景：

+   【内容创作】

对于视频制作人、动画师和内容创作者来说，Sora可以快速将脚本或故事概念转化为视觉内容，大幅度提高内容生产的效率和创意表达的可能性。

+   【教育与培训】

在教育领域，Sora可以用来创建教学视频或模拟情景，使学习材料更加生动、直观，提高学习的兴趣和效果。

+   【广告与营销】

企业可以利用Sora快速生成针对不同市场和客户群体的定制化视频广告，提高营销活动的响应速度和个性化水平。

+   【游戏与娱乐】

Sora可以用于生成游戏内的动态场景、角色互动或者故事情节，为游戏开发和电影制作提供新的创意工具。

+   【新闻与媒体】

新闻机构可以使用Sora，根据报道的文本内容快速创建视觉支持材料，使新闻报道更加生动、吸引人。

+   【虚拟现实（VR）和增强现实（AR）】

Sora可以为VR和AR应用创建丰富的视觉内容，提供更加沉浸式的用户体验。

+   【个性化视频生成】

根据用户的兴趣和偏好，自动创建个性化视频内容，如个性化旅行指南、学习材料或健康指导视频。

+   【艺术创作】

艺术家和设计师可以使用Sora探索新的视觉表达方式，创作独一无二的数字艺术作品。

+   【自动视频摘要】

对于长视频内容，如会议、讲座或教程，Sora可以自动生成视频摘要，帮助用户快速获取关键信息。

Sora的应用潜力取决于其技术的成熟度、生成视频的质量以及用户界面的友好度。随着技术的不断进步和创新，未来Sora的应用场景将进一步扩展，为各行各业提供更多的可能性和机遇。

# 六、如何学习Sora

学习OpenAI的Sora这样的文本到视频生成技术，可以按照以下几个步骤进行：

## 了解基础知识

+   机器学习与人工智能基础：了解机器学习和人工智能的基本概念，特别是深度学习，因为这些技术是Sora背后的核心。

+   视频生成和处理基础：了解基本的视频处理技术，包括视频编辑、格式转换等，以及视频生成的基本原理。

## 学习具体技术

+   自然语言处理（NLP）：Sora依赖于理解和生成文本的能力，因此深入学习NLP是必不可少的。

+   生成对抗网络（GANs）和变分自编码器（VAEs）：这些是生成图像和视频的关键技术之一，学习它们的原理和应用会非常有帮助。

+   多模态学习：了解如何结合文本、图像和视频等不同类型的数据进行学习，因为Sora涉及从文本到视频的转换。

## 参加实践项目

+   动手实验：通过实际项目练习技术应用，比如使用`TensorFlow`或`PyTorch`等框架进行模型训练。

+   参与开源项目：加入相关的开源项目，可以让你在实践中学习并贡献自己的力量。

## 了解Sora的具体细节

+   关注OpenAI官方网站和社交媒体渠道，获取Sora的最新动态和官方教程。

+   学习Sora的API文档，理解如何调用其功能来生成视频。

## 加入社群

+   加入相关的论坛和社交媒体群组，如Reddit、GitHub或专门的AI论坛，与其他开发者交流学习经验。

+   参加相关的在线课程和研讨会，不断提升自己的技能和知识。

# 七、Sora变现途径

## 销售Sora账号或邀请码

通过销售Sora账号或邀请码来获取利润，是利用Sora早期访问限制的一种方式。

目前，Sora模型还未对普通用户开放，但人们都迫不及待地期待着它的发布。

就像当年的ChatGPT一样，OpenAI的账号销售必定会持续走俏。

然而，与国外的大语言模型相比，许多普通人并不能完全区分它们。

当真正的Sora对普通用户开放时，国内外之间的差距可能会更加明显。

## 销售高质量的视频生成提示词

2023年AI绘画提示词的交易非常热门，而今年AI生成视频的提示词肯定也会有市场，从提示词打包交易到提示词交易平台。

对于熟悉AI的人来说，好的提示词往往容易在提示词交易平台上售出。

在AI绘画领域，类似的交易平台有promptbase、prompthero等。

## 制作/代生成AI视频

有两类用户可能需要帮助，一类是想要体验AI视频但不愿注册或深入研究的用户，另一类是有定制需求的用户，如生日祝福、婚礼纪念、定制广告、企业宣传片等。

## 使用Sora生成的视频作为个人自媒体账号内容

如果你是一个短视频，或者自媒体从业者。那么你可以优先考虑，直接用Sora生成短视频，然后发布到你的自媒体账号上。国内的平台，比如抖音、快手、视频号、📕、b站，这些都是流量比较好的平台，尤其是像📕、视频号这样不那么内卷的平台。比如今天我们有些朋友就已经开始用一些能找到的Sora素材，开始发到视频号上，流量都还不错。

当然，长期来看，如果你想靠自媒体的流量进行转化变现，还是和上一条一样，需要形成自己独特的ai视频风格。比如去年爆火的几类AI视频风格有复活AI名人进行访谈、宠物视频、二次元动漫混剪、电影混剪等等。名人、宠物、大IP都是好的创作方向。

## 将Sora生成的视频上传到各大素材网站赚钱

生成视频素材，然后将其上传到各大素材网站上出售，可以赚取使用费。设计师或视频剪辑师应该都了解这一点，因为许多精美的素材如果想要商用，都需要付费获取。

国内类似的短视频素材交易网站也有很多。当然，这类的赚钱方式相对来说比较短，平台不可能任由用户上传大量的AI短视频来薅羊毛的。所以，这类方式的行动要快。

## 知识付费，制作Sora相关的使用教程

过去一年里，AI领域最经典的好生意就是知识付费，卖课、卖社群。无论你是写一篇Sora的文章，还是一系列的教程，或者一系列的课程视频。这些都是比较好的知识付费产品。光是今天一天，就出了好几篇Sora相关的爆文，有大几百万的人对Sora感兴趣。所以，关于 Sora的知识付费教程，后续是肯定不会缺少用户买单的。

## 做电商，围绕Sora相关的搜索词做生意

很多人容易遗漏掉电商平台用户的精准性。无论是主流的电商平台，比如某宝、某东、某多多。

还是类似某鱼、某转这样的平台，AI相关的精准付费用户是很多的。所以，任何一个AI相关的需求出现，这些电商平台上就会出现好生意。这些平台上无论是卖工具、卖教程还是卖课程，都非常多。所以，盯紧电商平台关于Sora的关键词，准没有错。

## 做套壳的Sora网站、工具等产品

这个对于普通人来说门槛有点高，但还是挺适合一些程序猿、产品经理等互联网从业者的。

比如去年AI绘图爆火后，做的比较好的AI绘图产品、导航网站。还有类似妙鸭这种AI写真工具，都吃到了比较大的红利。

## 用Sora做AI小说推文



如果说去年的AI小说推文还是集中在AI图片领域，那么今年，AI小说推文很可能直接跨越到AI视频。去年这块爆款很多，赚到钱的也有，但是实际上做过的人都知道花费的精力不少，今年这块很可能还会爆发一次。

# 七、AI小说推文与直播带货

## 直播带货，帮忙推广Sora相关的付费产品

对于没有产品制作能力的朋友，其实可以考虑通过直播进行售卖。核心就在于，短视频的生成天然吸引用户的注意力。比如去年很多runway的介绍直播间，流量都很高，但是因为不具备实际的应用场景，所以付费率不高，但是Sora很明显更加成熟。所以，如果你没有做产品的能力，可以考虑帮助别人推广。

# 八、Sora的原理

> Sora是一个扩散模型，它从类似于静态噪声的视频开始，通过多个步骤逐渐去除噪声，视频也从最初的随机像素转化为清晰的图像场景。与GPT模型类似，Sora使用了Transformer架构，有极强的扩展性。

> 视频和图像是被称为“补丁”的较小数据单位集合，每个“补丁”都类似于GPT中的一个标记（Token），通过统一的数据表达方式，可以在更广泛的视觉数据上训练和扩散变化，包括不同的时间、分辨率和纵横比。

> Sora基于过去对DALL·E和GPT的研究基础构建，利用DALL·E 3的重述提示词技术，为视觉模型训练数据生成高描述性的标注，因此模型能更好的遵循文本指令。

> 除了能够仅根据文本指令生成视频外，Sora 还能够获取现有的静态图像并从中生成视频，准确且细致地动画化图像内容。模型还可以取一个现有的视频并扩展它或填充缺失的帧。OpenAI 认为，Sora 为能够理解和模拟真实世界的模型奠定了基础，这将是实现 AGI 的一个重要里程碑。

如果说Sora是依赖使用Unreal Engine 5生成的大量合成数据进行训练，我一点都不会觉得意外。

因为它确实需要这样做！

我们来分析一段视频：

![](`img/3d951da07750b88b2e4979913b6b1944.png`)

[`ships-in-coffee (1).mp4`](`https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/L0pubpf3poiAG4xTo2Xcs3g9n3f`)

视频提示词是：“在一杯咖啡中航行的两艘海盗船进行战斗的逼真特写镜头。”

+   模拟器创建了两艘装饰各异的精美3D海盗船模型。Sora需要在其内部空间隐式地完成从文本描述到3D模型的转换。

+   这些3D模型能够在保持彼此距离的同时进行动态航行。

+   它还模拟了咖啡流体的动力学效果，包括船只周围形成的泡沫。值得一提的是，流体模拟本身就是计算机图形学中一个非常复杂的分支，需要依赖复杂的算法和方程式。

+   视频达到了近乎逼真的光影效果，仿佛是采用了光线追踪技术进行渲染。

+   考虑到杯子相比海洋的小尺寸，模拟器还采用了移轴摄影技术，营造出了一种微观世界的视觉感受。

+   虽然视频中的场景在现实世界中并不存在，但这个引擎还是按照我们所期望的物理规则正确地实现了场景模拟。

> 下一步是增加更多的输入模式和条件设置，我们就能得到一个全面的数据驱动的虚拟引擎，它将有望取代所有传统手工编制的图形处理流程。

# 九、Sora的局限性

+   OpenAI指出它可能难以准确模拟复杂场景的物理原理，并且可能无法理解因果关系。例如，在文本描述为“五只灰狼幼崽在一条偏僻的碎石路上互相嬉戏、追逐”的画面中，狼的数量会凭空出现或消失。

+   该模型还可能混淆提示的空间细节，例如混淆左右，并且可能难以精确描述随着时间推移发生的事件，例如遵循特定的相机轨迹。

+   官方发布的视频DEMO“与中国龙一起庆祝农历新年的视频”中，Sora无法准确生成视频画面中的中文受到了网友的关注。有相关人士对此调侃道：“莫非是中文太难了？”

![](`img/d4ef40f0745b4fbbf55a9aba989eebc5.png`)

# 十、Sora对行业的影响

> 来自于新浪科技

+   没有演员的影视作品出现，对演员来说是个小挑战，将出现真正的“虚拟偶像”，此前的二次元人物并没有真正达到“偶像”的级别。

+   利好编剧行业，剧本、文本创作力成为核心竞争力。

+   Sora可能才是真正的文生视频，此前的文生视频大多只有2秒，仅仅是对象的小幅度移动。

+   OpenAI继续拉大领先程度，对众多还在进行大模型测试打分pk的厂商，构成压力。

+   直接的影响是影视行业，特别是特效行业。使用AI来制作一些特效和高风险的镜头，可以大幅降低拍摄成本，也可以避免很多危险。

+   摄影师行业也会受到影响，用文本来生成一些视频，可以省去很多拍摄工作。

+   短视频流行开以后，视频剪辑师也随之成为一个热门职业。如果视频剪辑的工作可以用AI来代替，可能会有很多视频剪辑师失业。

+   对于很多短视频创作者来说，用AI来替代繁琐的剪辑工作，可以大幅提高工作效率。

+   很多歌手拍摄MV都是大成本制作，如果可以用AI来生成所需要的MV画面，也可以省去很大一部分制作成本。

+   另外，如果真正意义上的文生视频得以实现，可能会有不法份子利用这项技术实施新手段的违法犯罪。

Sora，作为一个文本到视频的生成工具，拥有广泛的未来发展前景，尤其在提升创作效率、内容定制化、以及跨媒体应用等方面。

随着技术的发展和应用场景的不断拓展，Sora的未来发展将可能重塑多个行业的内容创作和分发方式，提供前所未有的创作自由度和个性化体验。

# 十一、Sora对个体的影响

| 影响 | 说明 |

| --- | --- |

| 视频创作的民主化 | Sora可以让普通人只需输入文本即可生成高质量视频，降低了视频创作的门槛，使得视频创作更加民主化 |

| 文化多元化的促进 | Sora可以根据不同语言、文化和风格的文本指令生成视频，打破语言和文化壁垒，让世界变得更加多元化 |

| 违法犯罪的风险 | Sora可能会有不法分子利用这项技术实施新手段的违法犯罪，例如制作虚假的视频证据，或者侵犯他人的肖像权和隐私权 |

| 影视行业的冲击 | Sora可能会对影视行业，特别是特效行业，造成巨大的冲击，使用AI来制作一些特效和高风险的镜头，可以大幅降低拍摄成本，也可以避免很多危险 |

| 虚拟偶像的催生 | Sora可能会催生出真正的“虚拟偶像”，没有演员的影视作品出现，对演员来说是个小挑战，同时也会利好编剧行 |

不过，从Sora官网目前展示的视频画面效果来看，效果还没那么逼近真实，短期内不会产生让行业失业，但会有辅助作用，做个动画片应该问题不大。

OpenAI表示，Sora是能够理解和模拟现实世界的模型的基础，OpenAI相信这一能力将是实现AGI的重要里程碑。

至于Sora文生视频到底能带来多大影响，会不会翻车，还需要“让子弹飞一会儿”

# 十二、参考文章

+   360周鸿祎对视频大模型 Sora 的理解

![](`img/4008a0f9e21c5028380d721a0fb971cb.png`)

+   OpenAI全新发布文生视频模型Sora - 现实，不存在了

-👸号：数字生命卡兹克

`https://mp.weixin.qq.com/s/C-wSkjzkNsX0EFaJFTv38w`

+   OpenAI发布首个视频生成模型Sora - 现实，不存在了？

-👸号：小石AI之旅

`https://mp.weixin.qq.com/s/jb8uTGAsnN1amb5y0WC5nA`

+   OpenAI首个AI视频模型炸裂登场，彻底端掉行业饭碗！60秒一镜到底惊人，世界模型真来了？

-👸号：新智元

`https://mp.weixin.qq.com/s/93z4Ta91yLv7PB1pnBM9mg`

+   OpenAI Sora发布：视频AI界的top1

-👸号：AIGC新知

`https://mp.weixin.qq.com/s/c9ovxjBx0e-PoQikJd38lQ`

+   眼见不再为实！OpenAI发布最强文生视频模型Sora【附60s演示视频】

-👸号：腾讯科技

`https://mp.weixin.qq.com/s/fIM5PT5InZOAS8i2ZqB6sg`

+   OpenAI王炸模型引爆科技圈，我们第一时间深读了官方技术报告

-👸号：腾讯科技

`https://mp.weixin.qq.com/s/gSvxvOVqYtGcKw0ueDGbFA`

+   OpenAI首个文生视频模型亮相，你觉得咋样？

-👸号：央视网

`https://mp.weixin.qq.com/s/PS_WfoOX35XoScgoQJJVcA`

+   重磅！OpenAI发布文生视频模型Sora，一次可生成1分钟！

-👸号：AIGC开放社区

`https://mp.weixin.qq.com/s/m8DtOLmn_JkqZ5D5NYLIhA`

+   Sora来了，现实不存在了？

-👸号：每日经济新闻

`https://mp.weixin.qq.com/s/y82KaPSIjCROi9FLHXdf2A`

+   又一次被OpenAi刷屏，文生视频模型Sora真的要淘汰一波人

-👸号：伟豪学长AI

`https://mp.weixin.qq.com/s/Cksnawt-L73KInM2s5b7Mg`

+   OpenAI发布首个视频生成模型Sora

-👸号：CGTN

`https://mp.weixin.qq.com/s/fnAOmwH8gOoO-Mc-pHs2hw`

+   48个演示视频，最长60秒，看懂OpenAI视频大模型Sora

-👸号：深网腾讯新闻

`https://mp.weixin.qq.com/s/4_hBzafAqZSC9GWJsIMVPA`

+   OpenAI深夜发布首个文生视频模型Sora，现实将被彻底颠覆

-👸号：界面新闻

`https://mp.weixin.qq.com/s/0RdUwGoVfQjKYRULZLabxw`

+   炸裂AI技术Sora背后：奥特曼清单法

-👸号：孤独大脑

`https://mp.weixin.qq.com/s/XfUKz8Q0ybXaTgLIeA7Csg`
