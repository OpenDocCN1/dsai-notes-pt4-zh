
# Midjourney 不同模型和参数生成结果对比

> 来源：[https://my.feishu.cn/docx/ZVsLdKB3Xo0WKZx4UPWcgTIfnYm](https://my.feishu.cn/docx/ZVsLdKB3Xo0WKZx4UPWcgTIfnYm)

Niji 模型是 Midjourney 和 Spellbrush 之合作，可以制作出动漫和插图风格，适合动态和动作镜头、以角色为中心的构图。

用 Niji 模型时，将`--niji 5`参数添加到提示词末尾，或使用`/settings`命令并选择🍏 Niji version 5

`A horse is running across the prairie --niji 5`

![](img/8c51049df0dec7b249444fdf133a568b.png)

另外`--stylize` 可以用来微调参数，生成不同的风格，包括

`--style cute`（可爱）、`--style scenic`（场景）、`--style original`（原始）、`--style expressive`（表现力）

用示例说明这几种参数的区别

提示词：一匹马在草原上奔跑

`A horse is running across the prairie --niji 5`

![](img/c9e952d4913b682dbd055eb9ecdc3ec7.png)

`A horse is running across the prairie --niji 5 --style original`

相比于上一幅图，原始风格的画面简单，背景干净，颜色简单

![](img/472d55fbba929b309a3ee11db95ba3b3.png)

`A horse is running across the prairie --niji 5 --style cute`

可爱风格的图，可爱的表情，颜色丰富，更动漫化

![](img/3cb748b32826a31c9f6c0e4187c45dab.png)

`A horse is running across the prairie --niji 5 --style scenic`

场景化的图，会增加很多背景呈现，丰富的场景，弱化主体的表现

![](img/6da6888647c35bfa14ee7c7da1c417f5.png)

`A horse is running across the prairie --niji 5 --style expressive`

更具表现力的图，动作更加丰富，体现动态性

![](img/9f6edc6a75346028e257ae9f94250735.png)

`A horse is running across the prairie --v 5.2`

`niji5` 和 `v5` 之间的区别更加明显，更接近现实生活，更写实

![](img/98ce75f35f8863b19db355aeef4620ad.png)

好啦，参数的对比就到这里，实际使用过程中，根据自己的需求调整自己的参数
