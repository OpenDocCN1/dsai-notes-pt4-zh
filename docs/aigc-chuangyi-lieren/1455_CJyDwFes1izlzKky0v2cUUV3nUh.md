# 20250816-简单2步，轻松做出炫酷裸眼3D效果 | 附提示词

> 来源：[https://aigcreative.feishu.cn/docx/RJ9jdIZQDoiX4Xx1ZHBcuJ8Snph](https://aigcreative.feishu.cn/docx/RJ9jdIZQDoiX4Xx1ZHBcuJ8Snph)

最近，一位名叫@aziz4ai 的博主在X（原推特）上发布了一组让人眼前一亮的创意。

他提出了一个“**Scr**eenbreak Sorcery”概念，简单来说就是让手机屏幕不再仅仅是一个显示器，而是变成一个“传送门”，让产品仿佛从屏幕中“跳”出来，营造出一种逼真的裸眼3D的效果。

这种创意不仅炫酷，还让人开始思考：未来广告可能会彻底改变模样。

@aziz4ai的创意并非空穴来风，他利用了当前主流的AI工具，如ChatGPT和Midjourney，通过设计出了一套复杂的JSON提示词，指导AI生成超写实的AR（增强现实）广告效果。

先来看看他的作品：

一杯星巴克咖啡仿佛从屏幕中溢出，周围环绕着“Sip into Bliss”的3D文字；

![](img/93d53dfd115718ffcca41ba579429da9.png)

[X2Twitter.com_rCpB586ZGTe5setl_1168p.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MrIibHP0doJZ9KxOukCcyN0Mnce)

一一瓶红牛功能饮料“冲”出屏幕，旁边写着“BOOST POWER，BREAK LIMITS”。

![](img/7423986af75cc7e1614ee5a12ef9dbcc.png)

[X2Twitter.com_-tw5EK0w5aXSxPX7_1168p.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/H3T1b62KaoyG2sxALtRcTlSKn3f)

这些画面不仅视觉冲击力强，还让人觉得产品仿佛触手可及。

这种技术的核心在于AR的运用。

AR技术近年来在营销中逐渐崭露头角，尤其是像Burger King那样，通过AR让消费者互动体验品牌的案例，已经证明了这种方式的潜力。

@aziz4ai通过AI工具实现了产品“从屏幕中跃出”的幻觉，极大地增强了广告的沉浸感和吸引力。

从技术角度来看，这种创意依赖于AI的图像生成和动画能力。

@aziz4ai详细分享了他在ChatGPT中使用的JSON提示词，强调了“超写实”“动态效果”和“产品特性匹配”等关键要素。

这种方法不仅展示了AI在视觉创作中的强大潜力，也为广告行业提供了一种全新的思路：不再只是静态的图片或视频，而是让消费者感受到一种“身临其境”的体验。

整个过程很简单，主要分两步：

第一步，使用ChatGPT生成图片：替换掉提示词中， "product": "[Enter product name]", [ ]中的产品名即可。

```
IMPORTANT: ONLY CHANGE THE PRODUCT NAME IN THE JSON 
{
    &quot;product&quot;: &quot;[Enter product name]&quot;,
    &quot;goal&quot;: &quot;Photorealistic AR-style ad where the product bursts out of the viewer&#x27;s phone screen toward them with immersive realism, automatically adapting all visual and motion elements to match the product&#x27;s unique characteristics&quot;,
    &quot;orientation&quot;: &quot;vertical 2:3&quot;,
    &quot;viewpoint&quot;: &quot;first-person perspective as if the viewer is holding their phone; product angled for maximum depth and impact&quot;,
    &quot;action&quot;: &quot;product-specific dynamic effects and key visual elements organically emerge and extend beyond the screen, creating a powerful pop-out illusion&quot;,
    &quot;effects&quot;: {
        &quot;primary&quot;: &quot;Automatically generated 3D effects matching the product&#x27;s nature — e.g., splashes for liquids, sparks for electronics, fabric motion for apparel&quot;,
        &quot;secondary&quot;: &quot;Subtle atmospheric enhancements (glow, vapor, dust) adapted to the product&#x27;s style&quot;,
        &quot;forbidden&quot;: [
            &quot;cartoonish rendering&quot;,
            &quot;unrealistic proportions&quot;,
            &quot;low-quality details&quot;
        ]
    },
    &quot;lighting&quot;: {
        &quot;style&quot;: &quot;Automatically matches the product&#x27;s material, brand palette, and mood — from high-Kelvin crisp tones to warm cinematic glows&quot;,
        &quot;shadows&quot;: &quot;Physically correct, adapting to both the product shape and scene orientation&quot;
    },
    &quot;materials_and_detail&quot;: &quot;Ultra-detailed, photorealistic surface textures, reflections, and finishes auto-matched to the product&#x27;s real-world counterpart&quot;,
    &quot;color_grading&quot;: &quot;Dynamic cinematic color palette harmonized with the product’s brand identity and core tones&quot;,
    &quot;depth_and_perspective&quot;: {
        &quot;technique&quot;: &quot;Exaggerated foreground scale and adaptive depth-of-field optimized per product type&quot;,
        &quot;aim&quot;: &quot;Maximize pop-out realism and immersive depth&quot;
    },
    &quot;environment&quot;: {
        &quot;context&quot;: &quot;Adaptive background that supports and enhances the product’s aesthetic — urban, natural, studio, or conceptual&quot;,
        &quot;integration&quot;: &quot;Lighting, reflections, and shadows auto-synced with product and background&quot;
    },
    &quot;slogan&quot;: {
        &quot;text&quot;: &quot;[Enter 4-word slogan]&quot;,
        &quot;word_count&quot;: 4,
        &quot;placement&quot;: &quot;Bold 3D text dynamically integrated into the scene’s motion flow, wrapping around the product or main visual effect&quot;,
        &quot;integration&quot;: &quot;Physically correct shadows, reflections, and perspective, adapting style to match product mood&quot;
    },
    &quot;composition&quot;: &quot;Agency-grade hero framing, with all visual weight, color, and motion designed automatically for the entered product&quot;,
    &quot;quality&quot;: &quot;Single ultra-realistic high-end output; commercial-grade execution&quot;,
    &quot;notes&quot;: &quot;Every visual, color, and motion element auto-adjusts to align perfectly with the product&#x27;s category, material, and brand identity&quot;
}
```

```
重要提示：仅修改JSON中的产品名称
{
    &quot;product&quot;: &quot;[输入产品名称]&quot;,
    &quot;goal&quot;: &quot;超写实AR风格广告，产品从观众的手机屏幕中向其方向冲出，具备沉浸式真实感，自动调整所有视觉和动态元素以匹配产品的独特特性&quot;,
    &quot;orientation&quot;: &quot;竖屏2:3比例&quot;,
    &quot;viewpoint&quot;: &quot;第一人称视角（仿佛观众手持手机）；产品角度调整以最大化深度和冲击力&quot;,
    &quot;action&quot;: &quot;针对产品的动态效果和关键视觉元素自然浮现并延伸至屏幕外，营造强烈的弹出式幻觉&quot;,
    &quot;effects&quot;: {
        &quot;primary&quot;: &quot;自动生成与产品特性匹配的3D效果（例如：液体的飞溅效果、电子产品的火花效果、服装的布料动态效果）&quot;,
        &quot;secondary&quot;: &quot;根据产品风格调整的微妙氛围增强效果（光晕、雾气、尘埃）&quot;,
        &quot;forbidden&quot;: [
            &quot;卡通化渲染&quot;,
            &quot;不真实的比例&quot;,
            &quot;低质量细节&quot;
        ]
    },
    &quot;lighting&quot;: {
        &quot;style&quot;: &quot;自动匹配产品材质、品牌色调和氛围（从高色温的清晰色调到温暖的电影级光晕）&quot;,
        &quot;shadows&quot;: &quot;符合物理规则，适配产品形状和场景方向&quot;
    },
    &quot;materials_and_detail&quot;: &quot;超精细的超写实表面纹理、反射效果和表面处理，自动匹配产品的真实外观&quot;,
    &quot;color_grading&quot;: &quot;动态电影级调色板，与产品品牌标识和核心色调协调一致&quot;,
    &quot;depth_and_perspective&quot;: {
        &quot;technique&quot;: &quot;根据产品类型优化的夸张前景比例和自适应景深&quot;,
        &quot;aim&quot;: &quot;最大化弹出真实感和沉浸深度&quot;
    },
    &quot;environment&quot;: {
        &quot;context&quot;: &quot;适配并强化产品美感的自适应背景（城市、自然、影棚或概念性背景）&quot;,
        &quot;integration&quot;: &quot;灯光、反射和阴影与产品及背景自动同步&quot;
    },
    &quot;slogan&quot;: {
        &quot;text&quot;: &quot;[输入4字标语]&quot;,
        &quot;word_count&quot;: 4,
        &quot;placement&quot;: &quot;粗体3D文字动态融入场景动态流程，环绕产品或主要视觉效果&quot;,
        &quot;integration&quot;: &quot;符合物理规则的阴影、反射和透视效果，调整风格以匹配产品氛围&quot;
    },
    &quot;composition&quot;: &quot;广告公司级主视觉构图，所有视觉重心、色彩和动态均为输入产品自动设计&quot;,
    &quot;quality&quot;: &quot;单张超写实高端输出；商业级制作&quot;,
    &quot;notes&quot;: &quot;所有视觉、色彩和动态元素自动调整，完美匹配产品类别、材质和品牌标识&quot;
}
```

例如：“耐克的运动跑鞋”

![](img/1c55bb6ead60fcb9acc8ab269229035d.png)

第二步：使用Midjourney的图生视频功能即可。直接使用下面的提示词：

> hyper-realistic AR animation where any subject appears magically trapped inside the phone screen, fixed in perfect position no matter how the phone is rotated, ultra-detailed lighting and shadows matching real-world environment, surrounded by bold premium 3D text of exactly four impactful words orbiting fluidly around the phone, text casting realistic shadows and reflections onto the phone edges, seamless parallax between subject, text, and background, subtle floating particle effects and soft atmospheric glow for depth, ultra-smooth motion at high frame rate, delivering a jaw-dropping pop-out illusion as if breaking the barrier between screen and realit

> 超写实增强现实（AR）动画，任何主体仿佛被神奇地困在手机屏幕内，无论手机如何旋转，主体都完美固定在原位；拥有与现实环境相匹配的超精细光影效果；由四个醒目有力的单词组成的粗体高级 3D 文字围绕手机流畅旋转，文字在手机边缘投射出逼真的阴影和倒影；主体、文字和背景之间实现无缝视差效果；有微妙的漂浮粒子效果和柔和的大气光晕，增添画面层次感；以高帧率呈现超顺滑的动态效果，营造出令人惊叹的弹出式错觉，仿佛打破了屏幕与现实之间的界限。

很快就能得到下面的动画视频了：

![](img/bc85c7eb496f466b4bb912b77011c4fa.png)

[0_1080_N.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BHHybSbT0oAxS4xo33WcpgsKnbd)

整体实践下来，非常的简单，轻松，ChatGPT提供了复杂的提示词设计能力，Midjourney等AI视频工具则能够生成高质量的图像动画or视频，两者的结合，让这个“裸眼3D”的创意成为了可能。

这种趋势也与近年来AR营销的兴起不谋而合。

研究表明，AR广告能够显著提高消费者参与度和品牌记忆率，尤其是在年轻人群体中，这种互动式体验更容易引发共鸣。

![](img/1b7801cfceb1799ae69ff3c0d8936593.png)

[X2Twitter.com_KlBfkSqsQ_FIdQlS_1168p.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BNuabwCphodiK1xuLiNcBkn0nvg)

不过，@aziz4ai的创意也引发了一些思考。

首先，这种技术是否会成为未来广告的主流？

其次，AI生成的内容是否会完全取代人类创意？

最后，消费者是否会因为过于“花哨”的广告而感到疲劳？

这些问题都需要时间来回答。

但有一点是明确的：这个“Screenbreak Sorcery”概念已经点燃了想象，激发了人们对未来广告形式的无限遐想。

![](img/a767408f47435d2012948f412664f142.png)

[X2Twitter.com_KuHierCeLjPaWRwk_1168p.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/X6bebskIao02b4xPghPcN30Snp3)

这个创意不仅炫酷，还让人看到了AI和AR技术在广告领域的巨大潜力。

未来，手机屏幕或许不再只是一个“看”的工具，而会变成一个“体验”的媒介。

广告界的新革命，或许已经悄然来临。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>

# 知识库动态