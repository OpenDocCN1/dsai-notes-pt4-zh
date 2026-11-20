
# 20251203-终极技巧，用Gemini 3+Nano Banana Pro复刻任意风格、创意

> 来源：[https://aigcreative.feishu.cn/docx/LwkidzgotoEo2JxFTxvcHgKWn6M](https://aigcreative.feishu.cn/docx/LwkidzgotoEo2JxFTxvcHgKWn6M)

昨天的推文《》下面，有一位读者@突入 留言，

![](img/0ca9c4d9daa243fb4f1eb91eb2f202a8.png)

应该是对文中所讲的10个技巧还不太满意，哈哈，想要更进一步，大概意思是对任意风格进行复刻（懂的都懂），下面是他提供的技巧：

> "将图像转换为 JSON 提示符，包括大小和详细信息"

关于JSON格式的提示词，几个月前也写过一篇，是用来写VEO视频提示词的，感兴趣可以点击查看：《》

晚上经过我深度学习了一下，那咱就来安排一篇，看看是怎么回事。

## 01.单图拆解

找了一张最近在X上很火的一种风格，如下图：一个城市等距微缩卡通风格的天气预报，确实很精美，

**下面是生成它的原**提示词 from X@：TechieSA

> “Present a clear, 45° top-down isometric miniature 3D cartoon scene of [CITY], featuring its most iconic landmarks and architectural elements. Use soft, refined textures with realistic PBR materials and gentle, lifelike lighting and shadows. Integrate the current weather conditions directly into the city environment to create an immersive atmospheric mood.

> Use a clean, minimalistic composition with a soft, solid-colored background.

>

> At the top-center, place the title “[CITY]” in large bold text, a prominent weather icon beneath it, then the date (small text) and temperature (medium text).

> All text must be centered with consistent spacing, and may subtly overlap the tops of the buildings.

> Square 1080x1080 dimension。”

![](img/cf0e3aa62bd57c77760d2de52771b560.png)

我们把这张图拖到Gemini 3：

![](img/c3f56cb1cf8a3b0dc0d76a76e42440a5.png)

是得到以下结果：

> 这是一个基于你上传的伦敦 3D 风格图片生成的 JSON 格式提示词。我已经将画面中的关键视觉元素（如地标、天气组件、文字信息、风格材质）都包含在内。

```json
{
  &quot;prompt&quot;: &quot;Isometric 3D miniature diorama of London city on a square floating platform, claymorphism and plasticine style, cute and clean, C4D Octane render. Key landmarks included: Big Ben, The Shard, The Gherkin, London Eye, Tower Bridge, St Paul&#x27;s Cathedral. Details: River Thames flowing through, tiny red double-decker buses, black cabs, stylized green trees. Lighting: Soft global illumination, bright sunny day, soft shadows. Background: Solid pastel blue background with floating fluffy white clouds. UI Elements: A 3D weather icon (sun peeking behind cloud) floating above, large bold text &#x27;LONDON&#x27;, smaller text &#x27;December 2, 2025&#x27; and &#x27;11°C&#x27;. High quality, 8k resolution, smooth textures.&quot;,
  &quot;negative_prompt&quot;: &quot;photorealistic, dark, gritty, blurry, noise, low quality, distorted buildings, text errors, missing landmarks, messy, glitch, watermark, jpeg artifacts&quot;,
  &quot;dimensions&quot;: {
    &quot;width&quot;: 1024,
    &quot;height&quot;: 1024
  },
  &quot;aspect_ratio&quot;: &quot;1:1&quot;,
  &quot;seed&quot;: -1,
  &quot;steps&quot;: 30,
  &quot;cfg_scale&quot;: 7.0,
  &quot;model_suggestion&quot;: &quot;Stable Diffusion XL / Midjourney v6 (3D style) / Nano Banana Pro&quot;
}
```

对比看一下中文：

```json
{
  &quot;prompt&quot;: &quot;在方形浮动平台上的伦敦市等距3D微缩景观，黏土风与橡皮泥风格，可爱又整洁，C4D Octane渲染。包含的主要地标有：大本钟、碎片大厦、小黄瓜、伦敦眼、塔桥、圣保罗大教堂。细节：泰晤士河穿流而过，小小的红色双层巴士、黑色出租车、风格化的绿树。光照：柔和的全局光照，阳光明媚的晴天，柔和的阴影。背景：纯色柔和蓝色背景，飘浮着蓬松的白云。UI元素：一个3D天气图标（太阳从云后探出）飘浮在上方，大而粗体的文字“伦敦”，较小的文字“2025年12月2日”和“11°C”。高质量，8k分辨率，纹理平滑。&quot;,
  &quot;negative_prompt&quot;: &quot;写实、黑暗、粗糙、模糊、噪点、低质量、扭曲的建筑、文字错误、缺少地标、杂乱、故障、水印、JPEG伪影&quot;,
  &quot;dimensions&quot;: {
    &quot;width&quot;: 1024,
    &quot;height&quot;: 1024
  },
  &quot;aspect_ratio&quot;: &quot;1:1&quot;,
  &quot;seed&quot;: -1,
  &quot;steps&quot;: 30,
  &quot;cfg_scale&quot;: 7.0,
  &quot;model_suggestion&quot;: &quot;Stable Diffusion XL / Midjourney v6（3D风格） / Nano Banana Pro&quot;
}
```

得益于Gemini 3 强大的视觉推理能力，很轻松就得到了基于这张图的详细分析：

![](img/304767d6bd211e19b20546e26323393c.png)

我们尝试着输入北京、莫斯科明天的天气：

> Beijing，12月3日的

> 这是为您定制的北京版本 3D 微缩城市海报的 JSON 提示词。

> 我已将地标替换为北京的标志性建筑（故宫、天坛、大裤衩等），调整了日期为 12月3日，并预估了一个符合北京冬季的温度（3°C）。

```json
{
  &quot;prompt&quot;: &quot;Isometric 3D miniature diorama of Beijing city on a square floating platform, claymorphism and plasticine style, cute and clean, C4D Octane render. Key landmarks included: The Forbidden City (traditional red walls and golden glazed tile roofs), Temple of Heaven, CCTV Headquarters, China Zun Tower, Bird&#x27;s Nest Stadium. Details: Symmetrical layout, tiny cars on ring roads, stylized green pine trees. Lighting: Bright winter sunlight, clear crisp air, soft shadows. Background: Solid pastel blue background with floating fluffy white clouds. UI Elements: A 3D weather icon (sunny) floating above, large bold text &#x27;BEIJING&#x27;, smaller text &#x27;December 3, 2025&#x27; and &#x27;3°C&#x27;. High quality, 8k resolution, smooth textures.&quot;,
  &quot;negative_prompt&quot;: &quot;photorealistic, dark, smog, pollution, gloomy, blurry, noise, low quality, distorted buildings, text errors, missing landmarks, messy, glitch, watermark, jpeg artifacts&quot;,
  &quot;dimensions&quot;: {
    &quot;width&quot;: 1024,
    &quot;height&quot;: 1024
  },
  &quot;aspect_ratio&quot;: &quot;1:1&quot;,
  &quot;seed&quot;: -1,
  &quot;steps&quot;: 30,
  &quot;cfg_scale&quot;: 7.0,
  &quot;model_suggestion&quot;: &quot;Stable Diffusion XL / Midjourney v6 (3D style) / Nano Banana Pro&quot;
}
```

![](img/0467d473fc52adbc59e723b31749e224.png)

![](img/7fd7c1141d6cb3adf0317de308ae888f.png)

得到这两张图，风格迁移修改的非常好，如果是为了完全复刻，那相当不错了，Nano Banana Pro的学习能力还是超级强的。但其实不太对劲，在这个对话里，Gemini 3被上下文污染（`Context Pollution`）了。

Gemini 3 的超长上下文窗口既是优势也是劣势，当发第一张伦敦图，它的 `Image Embeddings`（图像嵌入信息）还停留在显存里。

为了不受当前对话的干扰，我重新开了一个对话，输入上面的JSON提示词：

![](img/9109b61c8be43ff0f0d97eff26849e93.png)

嗯，这才是新的执行提示词的结果，虽然不是百分之百到位，但效果也是挺不错的。

![](img/fe5a68909825aac605b043df498a1f60.png)

![](img/d56e8949adfaea3ebb176fc8b33198b8.png)

## 02.系列多图识别

上面对单个图的识别得到单个的提示词，如果对一系列的图识别，能不能得到一个系统指令？找来一个四宫格相同风格的图试一试:

![](img/c1b5f756971df7cc7fb20d5f3bdc283f.png)

把原来的提示词稍微改一改:

> "总结这一类图像，提炼系统指令，转换为 JSON 提示符，包括大小和详细信息"

```json
{
  &quot;image_generation_params&quot;: {
    &quot;style_preset&quot;: &quot;3D Isometric Diorama&quot;,
    &quot;model_ref&quot;: &quot;Nano Banana Pro / Midjourney V6 / Flux&quot;,
    &quot;aspect_ratio&quot;: &quot;1:1&quot;,
    &quot;quality&quot;: &quot;High Fidelity, 8k&quot;,
    &quot;lighting&quot;: &quot;Soft Global Illumination, Studio Lighting&quot;
  },
  &quot;prompt_structure&quot;: {
    &quot;subject&quot;: {
      &quot;type&quot;: &quot;Retail Store&quot;,
      &quot;brand_identity&quot;: &quot;[在此填入品牌，例如：Starbucks]&quot;,
      &quot;iconic_roof_object&quot;: &quot;[在此填入屋顶物体，例如：Giant Green Coffee Cup]&quot;,
      &quot;color_palette&quot;: &quot;[在此填入品牌主色，例如：Green and White]&quot;
    },
    &quot;composition&quot;: &quot;Isometric view, high angle, miniature diorama, street corner setting&quot;,
    &quot;art_style&quot;: &quot;3D render, C4D style, Blender Octane render, Chibi style, Pop Mart aesthetic, cute, plastic toy texture, clay material&quot;,
    &quot;details&quot;: {
      &quot;exterior&quot;: &quot;Glass walls, modern architecture, street lamps, cute trees, sidewalk benches&quot;,
      &quot;interior&quot;: &quot;Detailed shelves, counters, miniature furniture, warm interior lighting&quot;,
      &quot;characters&quot;: &quot;Tiny cute 3D chibi people, customers shopping, staff working, interaction&quot;
    },
    &quot;effects&quot;: &quot;Tilt-shift photography, depth of field, bokeh background, volumetric lighting, ray tracing, sharp focus on the store&quot;,
    &quot;negative_prompt&quot;: &quot;dark, gritty, realistic, distorted, blurry, low resolution, 2d, sketch, flat&quot;
  },
  &quot;compiled_prompt_text&quot;: &quot;Isometric view of a cute 3D chibi [Brand Name] store, the roof is shaped like a giant [Iconic Product], typically [Color] branding colors. Transparent glass walls revealing detailed interior with tiny furniture. Miniature chibi customers and staff interacting. Set on a clean street corner with cute trees and street lamps. 3D rendering, C4D, Blender, Octane render, Pop Mart toy style, clay texture, soft studio lighting, tilt-shift effect, bokeh background, 8k resolution, masterpiece. --ar 1:1&quot;
}
```

可以看一下重点:

![](img/d2fadfbab17bcc36d02e96bf9e7338a8.png)
