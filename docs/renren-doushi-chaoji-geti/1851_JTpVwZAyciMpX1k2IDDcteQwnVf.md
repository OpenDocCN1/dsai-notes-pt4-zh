# Stable Diffusion 咒语60个唯美场景，与AI小姐姐相伴一生（1-30）

> 来源：[https://xuqiwei1986.feishu.cn/docx/Q4vtdWs0Qoi9qcxLI74c5vJ5nyR](https://xuqiwei1986.feishu.cn/docx/Q4vtdWs0Qoi9qcxLI74c5vJ5nyR)

大家好，我是旭日(原殇煜)，今天来给大家分享，如何用sd简单的咒语输出好看的图片的教程，几十种风格，总有一款适合你，全文4300字，阅读大概需要花费5分钟，话不多说直入主题。

## 1.基本设置：

**定义主体，正向提示词：**

```
absurdres , highres, ultra detailed, (1girl:1.3),
```

**反向提示词：**

```
EasyNegative, (worst quality, low quality:1.4), nsfw
```

**设置：**

**模型 ：**

revAnimated，breakdro，anything-v5-PrtRE，这几个二次元的大模型都还可以的（其他的没有用过，大家可以自由选择多多尝试），写实的大模型在这里效果较差，就不说了。VAE推荐使用：vae-ft-mse-840000-ema-pruned.safetensors，clip终止层数：2

迭代步数(steps):  30

采样方法(Sampler):  DPM++ 2M Karras,DPM++ SDE Karras 等等

宽度：768，1024

高度：512，768

**(****这**样设置方便出图，后续可以等比列放大，第一种直接使用sd放大(后续会分享或者圈里大佬有分享的大家可以试试)，第二种使用工具放大，我用的是：https://waifu2x.udp.jp/ 支持png和webp格式，1.6倍，2倍，4倍放大，主要方便不占用sd画图，另外小技巧，你可以多次放大)

总批次数：2

单批数量：2

**(这样设置出来的图，方便观察，先出来的是四张合在一起的**图，后面才是4张单独的图，方便调试参数)

提示词引导系数：7

随机数种子(seed)：279206436

设置完成出图：

![](img/2b56287a67832134fa8f812bfdbc5af8.png)

## 2.咒语及出图

### 日出

![](img/daeb3f93fb9cc42751a4811b0e27efff.png)

```
morning glow, pastel sky, cool tones, sun rays, soft light, peaceful atmosphere, early morning serenity
```

### 雨天

![](img/ef3a1eab6255e548836032571f5b46d1.png)

```
raindrops, wet surfaces, reflections, umbrellas, puddles, moody atmosphere, soothing sound of rain
```

### 雪景

![](img/ba02270cd791eb26874b98ece6ba712e.png)

```
snow-covered landscapes, winter wonderland, soft snowflakes, chilly atmosphere, frozen beauty, peaceful serenity
```

### 森林

![](img/c8da5f1d8e2758bfe0ab4a2d0b13805d.png)

```
lush greenery, towering trees, dappled sunlight, serene atmosphere, nature&#x27;s embrace, forest tranquility
```

### 海边

![](img/6d6b2325ff82c0a08c2be9bf6d053db9.png)

```
ocean waves, sandy shores, seashells, sea breeze, coastal landscapes, relaxing atmosphere, beach vibes
```

### 城市

![](img/23e2e5f5e57fc5303c0e585a14f9bb8e.png)

```
urban landscapes, city lights, skyscrapers, bustling streets, metropolitan atmosphere, modern architecture
```

### 山脉

![](img/3464aa6c9ce3d158c3d3bc2e05369aa7.png)

```
majestic peaks, rugged terrain, panoramic vistas, alpine beauty, outdoor adventure, breathtaking heights
```

### 沙漠

![](img/1e11b8ec85cc6b977c5eb1cd8a0e639f.png)

```
arid landscapes, sand dunes, endless horizons, sun-scorched earth, desert wildlife, stark beauty
```

### 花园

![](img/2c42d6cac20adf3c5a61f7cd0392e38d.png)

```
blooming flowers, colorful petals, fragrant scents, nature&#x27;s bounty, vibrant gardens, peaceful scenery
```

### 夜空

![](img/8b3f3c50d2bcd39bce5abdb88cee3c38.png)

```
starry skies, moonlit nights, celestial wonders, constellations, tranquil darkness, cosmic beauty
```

### 日常生活

![](img/e3f65e877120374d571044fcdfa657cb.png)

```
everyday moments, candid snapshots, familiar surroundings, personal narratives, human connection, authentic experiences
```

### 儿童游戏

![](img/d5c0305d7dac6d3dc4150a23774d6b81.png)

```
playful children, youthful energy, laughter, innocent joy, carefree fun, heartwarming scenes
```

### 家庭时光

![](img/1d613ea1f34e4c26f5d63a996e2d9149.png)

```
family gatherings, shared memories, loving connections, generational bonds, intimate moments, emotional support
```

### 友谊

![](img/fe9b7e2a3750a74ef0e8a943f9106479.png)

```
close friends, shared laughter, lasting bonds, heartfelt conversations, camaraderie, trust, and loyalty
```

### 爱情

![](img/f69de33fe61d09e2eb64ff2292c127ba.png)

grid-0438.png

```
romantic moments, loving gestures, passionate embraces, tender affection, intimate connections, heartfelt emotions
```

### 冒险

![](img/207d8dd2048c2dde68702abd44f4752a.png)

```
daring adventures, exhilarating experiences, outdoor exploration, personal growth, risk-taking, thrilling challenges
```

### 运动

![](img/0d03cd6dc1b8b8591c5c9baf1ff343af.png)

```
athletic pursuits, intense competition, physical exertion, teamwork, dedication, personal triumphs
```

### 音乐

![](img/0037fad1faa9ff90b4da8e72efa48d27.png)

```
live performances, rhythmic beats, emotional melodies, musical instruments, creative expression, shared passions
```

### 舞蹈

![](img/0d97c19499889507f570625c64e1082e.png)

```
graceful movements, rhythmic motion, elegant expressions, cultural traditions, passionate performances
```

### 烹饪

![](img/4408174ceb74ca179e5395dcf208412c.png)

```
culinary creations, mouthwatering dishes, cultural cuisine, fresh ingredients, delicious flavors, shared meals
```

### 节日

![](img/c80cb6739a7958c22588f5175d5f2e29.png)

```
festive celebrations, colorful decorations, traditional customs, cultural performances, communal gatherings, high spirits
```

### 旅行

![](img/74444ae0684ddb045554911be9a50744.png)

```
exploring new destinations, cultural immersion, wanderlust, breathtaking sights, global adventures, memorable experiences
```

### 艺术与创意

![](img/433feb3cb646a983f9090cecc5db492d.png)

```
artistic expression, creative projects, inspiring works, imaginative ideas, personal passions, skillful craftsmanship
```

### 宠物与动物

![](img/bfc405dbee8a09901004182242534411.png)

```
furry friends, loyal companions, wild creatures, animal encounters, playful antics, heartwarming bonds
```

### 时尚

![](img/7352085dcd5226f54eb1a6c7639992f1.png)

```
stylish clothing, fashion-forward trends, personal style, self-expression, runway shows, designer creations
```

### 放松

![](img/18d35d20d00602fc1d29468a00dc31ab.png)

```
calming moments, peaceful solitude, rejuvenating activities, self-care, restful environments, mental well-being
```

### 工作场所

![](img/4c919f2a7ed76dbefaab20758be00cc1.png)

```
professional settings, collaborative teams, workplace challenges, career accomplishments, shared goals, personal growth
```

### 教育

![](img/82b16dd77cd5cba2b220721e145c4f8d.png)

```
learning environments, classrooms, teachers and students, educational materials, academic achievements, lifelong learning
```

### 灵性

![](img/8706a4b24bb4f4c5eaf239df37b4d81c.png)

```
meditation, mindfulness, spiritual practices, self-discovery, inner peace, personal growth, emotional healing
```

### 婚礼

![](img/8af4706d2d3cce5bc27d0b33c954ae1b.png)

```
nuptial celebrations, loving vows, bridal attire, wedding parties, memorable moments, romantic ambiance
```

先到这里，手好酸，给个小提示，本文涉及到的**场景，**除了人物图外，可修改提示词用来展现风景图，将人物相关的提示词去掉即可，也就是主体中 (1girl:1.3)。

后续持续更新。。。。