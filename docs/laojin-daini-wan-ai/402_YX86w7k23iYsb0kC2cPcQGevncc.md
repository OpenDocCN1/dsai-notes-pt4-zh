# XingliuArt - Liblib官方API

> 来源：[https://my.feishu.cn/docx/PZoGdfo7TolfOpxlVHYcnIZpnQg](https://my.feishu.cn/docx/PZoGdfo7TolfOpxlVHYcnIZpnQg)

### 原文[🤖️【XingliuArt】AI绘画插件使用指南](https://rq4rfacax27.feishu.cn/wiki/TLgrw1rJ8iI9dPkySoMcTxiOnvg)

注：本期的插件为订制插件，作者未开源，需要的联系老金

XingliuArt 是一个适用于Chatgpt-on-wechat项目的 AI 绘图插件，基于 Liblib官方API 开发，支持多种绘图模型及自定义参数设置，让用户仅仅使用🛰️聊天工具就能轻松生成高质量的 AI 图像。

## 一、**插件特点及开发目的**

### 插件特点

+   优势

+   功能强大易上手，支持多种绘图模式，无需理解复杂的参数界面，仅靠🛰️聊天轻松复刻SDwebUI的效果

+   会员模型随便用，无限制使用Liblib官网的会员模型，不会像非会员在网页端使用有次数限制和排队限速

+   生成图片可商用，可出售生成图片或用于商业目的,含F.1商用使用权

+   随用随充更灵活，注册即送500积分，用多少充多少，且积分永久有效不会像月会员过期清零

+   局限性

+   不能使用非商业许可模型（这一点没有办法替代Liblib网页端）

+   出图含内容审核，不可使用违禁词和敏感词

### 开发目的

无需高配电脑，方便用户随时随地通过🛰️聊天工具即可开启AI绘画之旅，生成可商用的高质量AI作品：

+   对于没有AI绘画基础的人，这款插件旨在让用户通过最简单的方式快速得到正反馈，而不是一上来就被复杂的参数界面和晦涩的专业术语所劝退

+   对于有专业绘画需求的人，这款插件能够让用户通过最轻量化的方式快速实现各种Checkpoint和Lora模型效果测试，而无需耗费大量时间等待模型下载和参数调试

## 二、插件安装和**使用方法**

### 获取密钥

访问Liblib API 开放平台主页，注册账号并获取 access_key 和 secret_key

### 安装插件

下载XingliuArt插件包，解压后将XingliuArt文件夹上传至到chatgpt-on-wechat下的plugins目录

确保目录层级符合以下格式：

```
   plugins
   └── XingliuArt
           └── config.json
           └── XingliuArt.py
           └── LibModel.py
           └── __init__.py
```

### 配置密钥

+   将密钥填入 config.json 文件

+   要使用提示词自动优化功能，需配置相应的enhance_prompt_api信息，推荐使用智谱或硅基免费模型

（当然使用付费模型效果会更好）

+   设置每日插件使用次数，可避免积分消耗过快

+   设置管理员昵称为自己的🛰️昵称，可在🛰️群聊中使用管理员专属指令“禁用星流绘画”和“开启星流绘画”

```
{
  &quot;access_key&quot;: &quot;your_liblib_access_key&quot;,
  &quot;secret_key&quot;: &quot;your_liblib_secret_key&quot;,
  &quot;enhance_prompt_api_url&quot;: &quot;any_llm_api_url&quot;,
  &quot;enhance_prompt_api_key&quot;: &quot;any_llm_api_key&quot;,
  &quot;enhance_model&quot;: &quot;any_llm_model&quot;,     
  &quot;daily_limit&quot;: 50,
  &quot;admin_nicknames&quot;: [&quot;管理员昵称1&quot;, &quot;管理员昵称2&quot;]
}
```

### 重启chatgpt-on-wechat

重启chatgpt-on-wechat项目并输入#scanp命令扫描新插件

在🛰️聊天窗口输入#help XingliuArt查看帮助信息，成功返回则说明插件安装成功

## 三、指令参考及效果展示

+   以下案例均为本人🛰️端直出效果，可以直接复制或略作修改后使用（后面会不定期更新，建议收藏）

+   注意有些案例使用了--enhance 提示词自动优化指令，只复制优化前的提示词即可

| **绘图指令** | **参考图片** | **生成结果** |
| --- | --- | --- |
| 星流文生图 minimalism,Chinese ink painting,ink painting,1girl,solo, upper body,black hair,long_hair,hair bun,earrings,jewelry, hair ornament,makeup,eyeshadow, looking at viewer,off-shoulder,hanfu, long sleeves, closed mouth,flower, bamboo, leaf,simple background,master works, flat, high definition, --ar 3:4 | 无 | ![](img/4d635a7908657735869ac815eb4d33d1.png) |
| 星流文生图 Illustration,AYU,Artistic illustration,A young girl with long, wavy hair and a cozy sweater, sitting at a wooden table in a modern, warmly lit Starbucks. She holds a steaming cup of cappuccino, adorned with a delicate heart design in the foam. The background features large windows with a cityscape view, green indoor plants, and soft, ambient lighting. The scene is captured in a high-resolution, realistic style with vibrant colors, emphasizing the inviting atmosphere and the intricate details of the coffee cup and the girl&#x27;s thoughtful expression. --ar 9:16 | 无 | ![](img/6154574e355198e1eabb012b286592f5.png) |
| 星流文生图 Whimsical cartoon style, a small person with rosy cheeks and wild hair racing a toy car with flames and sparks, in a vibrant, exaggerated environment with bold black outlines, bright pastel colors, and dynamic textures, emphasizing the character&#x27;s joyful, toothy grin and playful atmosphere.--ar 3:4 | 无 | ![](img/077c62f247160b0a8681c3e1b5eb07d4.png) |
| FLUX文生图 sunglasses,big head,girl wear sunglasses,big head,stand,double ponytail,camisole short skirt,cartoon,&lt;Lora:1.0,8d337b6f590f4105af9b45412abb4594&gt; --ar 3:4 | 无 | ![](img/4ac5f69bff95bb67088820b2de51cb11.png) |
| FLUX文生图 waitgoo,Microscale landscape, super close-up, shallow depth of field, among Van Gogh&#x27;s starry sky, on a luminous boat, sitting a little prince holding a bright red rose, furry sea, Van Gogh style, dreamy scattered scenery, streamer, swirling starry sky, brilliant starry man, bioluminescence, ultra-fine section, 3D stereo | 无 | ![](img/1d167fa4e8648a142d32ee21b98f00a9.png) |
| FLUX文生图 advanced digital art, poster of new_year&#x27;s day with big designed words&quot;2025&quot;, flower and butterfly in background, &lt;Lora:1.0,a93008c72d6e4d088034b43f82ae1696&gt; --ar 3:4 | 无 | ![](img/e95cb7f4ec5ab37d78cf7b6466086bcb.png) |
| FLUX文生图 zihui_f1, this is a lively picture A vivid, colorful, cartoon-style digital drawing depicting the Tesla logo in a lively, colorful and stylized way. It features a large statue of an astronaut wearing a helmet and a space suit standing atop a rock outcrop. The statue is surrounded by a series of smaller stylized elements including spaceships, satellites, flying saucers and rockets, and auspicious clouds. The background is pure white, ensuring that the focus remains on the stylized colorful illustration, creating a sci-fi atmosphere. It is surrounded by lush green plants and castle-like buildings. The illustration is rich in detail, with smooth lines and bright colors, which enhances the ethereal and sci-fi-like quality of the scene.&lt;Lora:0.85,bdeb40ebcc754939a5f2971c777fe556&gt;--ar 3:4 | 无 | ![](img/f9e94073d505c273d8d1a047e5528f48.png) |
| FLUX文生图 zihui_f1, This is a vibrant, colorful, cartoon-style digital drawing with a huge, stylized huawei logo in the center. It features a giant Chinese dragon hovering over the top of the logo. Surrounding the dragon are a series of smaller stylized elements, including spaceships, satellites, flying saucers, and rockets. The background is pure white, ensuring that the focus remains on the stylized huawei logo color illustration, creating a sci-fi atmosphere. It is surrounded by lush green plants, auspicious clouds, and ancient Chinese palaces. The illustration is rich in detail, with smooth lines and bright colors, which enhances the ethereal and sci-fi feel of the scene.&lt;Lora:0.85,bdeb40ebcc754939a5f2971c777fe556&gt;--ar 3:4 | 无 | ![](img/c2808c7c5dd421556a9934c6fca60835.png) |
| FLUX文生图 钢铁侠，写意水墨风格 | 无 | ![](img/dcf580570de43a9cb65384487dbf04dd.png) |
| FLUX文生图 扁平插画风格，一个汉服美女 &lt;Ckpt:8e53c30ec1094993ab2b3343b7187169&gt;&lt;Lora:0.8,aa6542ca704e438d9a41e43ca8e69091&gt; --ar 3:4 --enhance | 无 | ![](img/9ed139eeadcecc0e31162ea02e4babd3.png) |
| FLUX文生图 JJFG, Minimalist style characters,JJFG,Minimalist style characters,A stylized,high-quality digital illustration of a woman with pale skin,black hair styled in an elegant updo,and wearing a black outfit with a gradient green and red scarf. She has red lips,blue eyeshadow,and large,geometric earrings with dangling elements. The background is a soft light blue,and the composition follows the rule of thirds. The image has a shallow depth of field,with the background slightly blurred. The lighting is soft and even,with no strong shadows. The aesthetic quality is very high.,&lt;Ckpt:8e53c30ec1094993ab2b3343b7187169&gt;&lt;Lora:0.8,aa6542ca704e438d9a41e43ca8e69091&gt; --ar 3:4 | 无 | ![](img/b2d0dbc41d66120c0fa17e6780c56e85.png) |
| FLUX文生图 diffusion,1girl,solo,gradient background,gradient,dress,black dress,gloves,black hair,black gloves,purple background,closed eyes,elbow gloves,hair bun,feathers,&lt;Lora:0.8,ee774a57fe5a4d51adf7c3ad80276ba3&gt;--ar 3:4 |  | ![](img/66ef110589bc7fbf09325558d768ad85.png) |
| FLUX文生图 Ink sketch,color ink painting,((HRD, HUD, 8K)),((masterpiece, best quality)),highly detailed,soft light, InkPainting-Xiake, Donald Trump,Donald Trump&#x27;s Classic Actions,male focus,solo,(Black and white background of Tai Chi diagram:1.3),Medium Long Hair, Donald Trump,1man,hands in trouser pockets,Perfect body proportions,&lt;Lora:0.8,af6608f886164a0e902bccc3b0eeae98&gt; --ar 3:4 | 无 | ![](img/788c147a26542b3ebc19f9e25eba1211.png) |
| FLUX文生图 Ink sketch,color ink painting,((HRD, HUD, 8K)),((masterpiece, best quality)),highly detailed,soft light, InkPainting-Xiake, Elon Musk,Elon Musk&#x27;s Classic Actions,male focus,solo,(Black and white background of Tai Chi diagram:1.3),Medium Long Hair, Elon Musk,1man,hands in trouser pockets,Perfect body proportions,&lt;Lora:0.8,af6608f886164a0e902bccc3b0eeae98&gt; --ar 3:4 | 无 | ![](img/a82da260938310fe886f8d422946dbc7.png) |
| FLUX文生图 guomo,ink art, minimalism，dressed in Georgian Peacoat, Oriental Shorthair Cat, Melancholy and Icy, with a Ant, Detestable Flower in hair, Screen print, Embarrassing, natural lighting &lt;Lora:1.0,f5f956a9863d44e4904c5072223869fb&gt; --ar 3:4 | 无 | ![](img/2135c842265e0191dcc06835c5484534.png) |
| FLUX文生图 Boy and dog, signature strokes,black lines,white background,intermittent black lines,simple line drawing,vector illustration,flat design,white background,colorful lines,doodle style,simple coloring,&lt;Lora:0.8,afe7ec86f6e54ac69bdc2cd0da369bf1&gt;--ar 3:4 | 无 | ![](img/5eb9fdd7ba3e3a0c266cffc847c9c99c.png) |
| FLUX文生图 blend of real and lego style_CHLO,Halloween theme,Lego Avengers Movie Poster, Lego Star Avengers, fantasy, Lego People, glowing, (Use Dream Diffusion Secret Prompt), UHD,pumpkins, Halloween dresses,bats,full moon | 无 | ![](img/3087b26aea5e97a08cbfa92ad8879f59.png) |
| FLUX文生图 architectural design,Futuristic architecture,Towards High Definition, The image showcases a futuristic, organic-shaped house nestled amidst snowy mountains during sunset. The house is **** of multiple interconnected spheres, each with large windows offering a panoramic view of the landscape. The interior is warmly lit, revealing cozy interiors with furniture and plants. The surrounding environment is serene with snow-covered trees and mountains in the distance. The art style is modern with a touch of surrealism, and the drawing style is highly detailed and realistic.&lt;Ckpt:F1_Architecture&gt; --ar 3:4 | 无 | ![](img/4fa87ad104befe44cd421ada04fa3c90.png) |
| FLUX文生图 A skincare lotion surrounded by flowers and plants,Herbs that embody nature and purity,High-quality and minimalist product image,High detail,8K,High quality, | 无 | ![](img/ede0c01a9651962670325535a9a4f71d.png) |
| FLUX文生图 Q version of the small snake,The Q version of the little snake, cartoon style little snake, beautiful facial features, wearing a snow cap, elegant posture, surrounded by romantic flowers and petals, red lanterns in the distance, under the pink peach blossom tree, snowing, beautiful and fresh atmosphere, the most high quality,&lt;Ckpt:F1_3Dcartoon&gt;&lt;Lora:0.8,27078e683955454e8990f0b89e1be530&gt;--ar 3:4 | 无 | ![](img/0eea47b796d8b7482f0de0fa95efc8b9.png) |
| FLUX文生图 (masterpiece, best quality, very fine CG, intricate details:1.2), 1girl,solo,weapon,dress,black hair,electricity,sword,closed,&lt;Lora:0.8,ad6cf76272c64ed5bc261f3a488b3fe0&gt;--ar 3:4 | 无 | ![](img/fdc9aad53c3a7c92cbddcf59dca494d1.png) |
| FLUX文生图 (masterpiece, best quality, very fine CG, intricate details:1.2), 1girl,solo,weapon,dress,black hair,electricity,sword,closed,breasts,huge breasts,big breasts,fire,flowing hair,burning,wrapped in flames,&lt;Ckpt:1a10e11044da40078e7a355ed11cfdb9&gt;&lt;Lora:0.7,ad6cf76272c64ed5bc261f3a488b3fe0&gt;&lt;Lora:0.5,0261abaa569948e98ec792e5cd3098d8&gt;--ar 3:4 |  | ![](img/93af57753b3a72740fcfff8d2cd89d80.png) |
| FLUX文生图 This digital artwork,reminiscent of a fantasy anime style,depicts a fierce,ethereal warrior woman with long,flowing white hair adorned with blue feathers. She has pale skin,sharp features,and large,expressive eyes. She wears a dark,armor-like outfit with metallic accents and blue,glowing details,evoking a sense of otherworldly power. Her wings,delicate and translucent, resemble butterfly wings with blue hues. The background is an enchanted forest bathed in ethereal light,with glowing blue butterflies and mystical creatures,creating a surreal,magical atmosphere.&lt;Ckpt:F1_3Dcartoon&gt;--ar 3:4 | 无 | ![](img/a1d55b89dba8e4340426a5d84ba465d5.png) |
| FLUX文生图 This is a high-quality digital anime-style drawing of Hatsune Miku,a popular Vocaloid character. She has teal twin-tails,large teal eyes,and a cute smile. She wears a white tank top with &quot;JUST&quot; and a black choker with a cross pendant. The background is white,emphasizing her colorful,vibrant appearance. The composition uses rule of thirds,with the character centered. The lighting is bright and even,with no shadows. The image has a very high aesthetic quality.&lt;Ckpt:8e53c30ec1094993ab2b3343b7187169&gt;--ar3:4 | 无 | ![](img/6f0118be3fd5717340b98d9fb8752d88.png) |
| FLUX文生图 This is a high-quality digital anime-style drawing of Hatsune Miku,a popular Vocaloid character. She has teal twin-tails,large teal eyes,and a cute smile. She wears a white tank top with &quot;JUST&quot; and a black choker with a cross pendant. The background is white,emphasizing her colorful,vibrant appearance. The composition uses rule of thirds,with the character centered. The lighting is bright and even,with no shadows. The image has a very high aesthetic quality.&lt;Ckpt:8e53c30ec1094993ab2b3343b7187169&gt;--ar3:4 | 无 | ![](img/8021b0bf67c886e5e6005864ce963092.png) |
| FLUX文生图 A woman with long blonde hair is posing for the camera. Her hair is straight and cascades over her shoulders. She has bangs that are swept to the side. She is wearing a red dress with a silver necklace around her neck. Her makeup is natural and she has a neutral expression. She appears to be standing in front of a wooden dresser.,&lt;Ckpt:571d4b8c22fb4ddc87ca08b55ea2fdf3&gt; --ar3:4 | 无 | ![](img/f99cf274e264b8c8257af6f16fe4b165.png) |
| FLUX文生图 A dramatic portrait of a woman with hair like burning embers,wearing a dress made of fire and shadow,overlooking a burning cityscape at sunset. High resolution,8k,fire lighting,hyperrealistic rendering,detailed textures,burning cityscape,fire,shadow,dramatic,masterpiece.,&lt;Ckpt:571d4b8c22fb4ddc87ca08b55ea2fdf3&gt; --ar 3:4 | 无 | ![](img/1f8e680afe49673ff8e92e51779116fc.png) |
| FLUX文生图 SM,Stylized Portrait of a Woman with Red Hair and Earrings,featuring watercolor-**** brushstrokes.&lt;Lora:1.0,a30c2f6de63641cfb0a63f21711ca6d9&gt;--ar 3:4 |  | ![](img/c7ef313c5b7901aef07a23894e5bf9c7.png) |
| FLUX文生图 shinvtu,SM,Martin style illustration,flat Illustration,Intricate traditional Chinese portrait on rice paper,embracing Song Dynasty aesthetics,a stunning ancient Chinese beauty with delicate facial features,lively and soft large eyes,slender and elegant waist,adorned in exquisitely crafted Han Dynasty attire with Morandi-inspired color palette,beautiful hair,and meticulous brushwork,showcasing five slender fingers,SM,&lt;Lora:0.9,db86793501944bde9df98ef6f46df882&gt;--ar 3:4 |  | ![](img/35b858015457b996e9a6430057b6a510.png) |
| FLUX文生图 shinvtu,This is a highly detailed digital illustration in a traditional East Asian art style. The subject is a young woman of East Asian descent with delicate features and fair skin. She has an ethereal, serene expression, with her dark, almond-shaped eyes gently gazing at the viewer. Her long, dark hair is styled in an elegant updo, adorned with a delicate floral crown that adds a touch of royalty. She is dressed in an elaborate, flowing robe made of sheer, translucent fabric in shades of white and soft pastels, with intricate gold embroidery and a delicate, lacy texture. The garment is designed to drape gracefully, revealing the subtle curves of her body while maintaining a modest, timeless aesthetic. Her hands are elegantly positioned, with one hand resting on her lap and the other gently holding a ribbon that cascades down, adding a sense of movement to the still image.The background is a muted, earthy palette of greens and browns, with an ornate, gold-leafed pattern of clouds and waves that echo the fluidity of her attire. The overall composition is harmonious, blending traditional elements with a modern digital artistry that emphasizes both beauty and tranquility.good hands,natural finger shape,see-through, highly detailed,ultra-high resolution,32K UHD,sharp focus,best-quality,masterpiece,masterful details,temperate atmosphere,with a high-end texture,concept art,absurdres,official art,detailed background,&lt;Lora:0.8,db86793501944bde9df98ef6f46df882&gt;--ar 3:4 |  | ![](img/640f830de9c1c8748f9739bbce6b3241.png) |
| FLUX文生图 masterpiece:1.2),best quality,high resolution,unity 8k wallpaper,(illustration:1),perfect lighting,extremely detailed CG,finely detail,extremely detailed,soft lighting and shadow,soft yet striking lighting,film grain:1.2,(skin pores:1.2),(detailed skin texture:1),((solo:1.5)),Detailed face,(see-through:1.1),misty,Low Key:1.1,depth of field,A young woman in a white dress,sitting on her bed with contemplative expression.&lt;Ckpt:F1_Photo&gt; --ar 3:4 | 无 | ![](img/2d4ea6c8def96896f1c580ae805cf1d2.png) |
| FLUX文生图luoxue,snow,sdmai,lanmeng,denim,shorts,navel,realistic,jewelry,earrings,thighhighs,long hair,denim shorts,belt,hand in pocket,brown hair,midriff,looking at viewer,short shorts,lips,parted lips,&lt;Ckpt:F1_Photo&gt;&lt;Lora:0.9,fdf30e2a449f476da6a678f5391d3ca4&gt; --ar3:4 | 无 | ![](img/30b800204cfe3b57ea19043970a6c734.png) |
| FLUX文生图 (masterpiece),best quality,8K,Indoor hallway,a beautiful woman with a perfect hourglass figure is standing confidently. She is wearing a fashionable long black trench coat,which is open to reveal a tight black bustier and short black shorts underneath. She pairs this with (black suspender stockings:1.2),with red suspender straps. Her body shape is stunning,with (large breasts:1.3),(slim waist:1.3),She is wearing high heels and strikes a confident pose,facing directly toward the camera with a poised and powerful expression. The shot is captured from a low angle, emphasizing her curves and long legs. (Action shot:1.2) of her throwing the hem of the windbreaker,(perfect body curves accentuated:1.3) by the open coat. Soft indoor lighting casting gentle shadows,(creating a high-fashion yet sensual vibe:1.3),with dynamic movement in the fabric and striking posture.&lt;Ckpt:F1_Photo&gt;&lt;Lora:0.7,b0023ba9629e44a2885f4eff16571936&gt; --ar 3:4 | 无 | ![](img/435c0e4b56b64c7a2c5413efe6182025.png) |
| FLUX文生图 trendy,this is a high-resolution photograph featuring a young Asian woman standing indoors,like in a bedroom or a similarly intimate setting. She has a slender physique with a petite frame and fair skin. Her long,dark hair is styled in a sleep,high ponytail,with a few loose strands framing her face.She is fitting a sleeveless red dress that accentuates her slim wait and curves. The dress has a unique design with a single,large cutout on the left shoulder,Christmas-style makeup,red hip-wrap skirt,Christmas tree behind,surrounded by elements of Christmas and New Year&#x27;s Day,&lt;Ckpt:58cb1fe14f0441f1adef4e984d3f334d&gt;&lt;Lora:0.7,b0023ba9629e44a2885f4eff16571936&gt; --ar3:4 | 无 | ![](img/699954013e091b52d6aeb627cad0615e.png) |
| FLUX文生图 shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,3D rendering,cinematic lighting effects,00 renderer,&lt;Ckpt:58cb1fe14f0441f1adef4e984d3f334d&gt;&lt;Lora:0.8,fdf30e2a449f476da6a678f5391d3ca4&gt;&lt;Lora:0.8,b2c3af94d5cb46528572d9ce8e19373b&gt; --ar3:4 | 无 | ![](img/2ab1cad67937f5064244d2e861ef0e20.png) |
| FLUX文生图 JQZX,This image is a highly detailed,photorealistic CGI artwork featuring a young woman with an ethereal,almost otherworldly beauty. She has fair skin,a slender yet curvy physique,&lt;Lora:0.8,3e03bd1ae4d14beabd40962191bb2885&gt;--ar 3:4 |  | ![](img/7743f769bb1b604d872dc351e373eec9.png) |
| FLUX文生图  asian young woman,stylish advanced digital comic illustration by atey ghailan and artgerm,Cosplay costumes of two girls,large breasts,during the day,long hair,looking at the audience,parted lips,realistic,red lips,standing,&lt;Ckpt:F1_Photo&gt; &lt;Lora:0.7,90f00c4ab6de404b8161be0ac545fc2a&gt;&lt;Lora:0.8,b9c24be385d04984a671b0a89bc0a852&gt; --ar 3:4 | 无 | ![](img/11af5cdca4557b5caa46c817eb5de275.png) |
| FLUX文生图  asian young woman,Cosplay costumes,big breasts,daytime,long hair,facing the audience,open lips,realistic,red lips,standing full body photo,high thighs,high heels,beautiful hands,best quality,(masterpiece:1.3),fullbody,ultra-detailed,solo:1.3,cinematic lighting,&lt;Lora:0.7,90f00c4ab6de404b8161be0ac545fc2a&gt;&lt;Lora:1.0,87c979a2f53f4341bd2c94109738cb14&gt; --ar 3:4 | 无 | ![](img/53bb2f8b4ea1662f69ea632e45a00d98.png) |
| FLUX文生图  asian young woman,Cosplay costumes,big breasts,daytime,long hair,facing the audience,open lips,realistic,red lips,standing full body photo,high thighs,high heels,beautiful hands,best quality,(masterpiece:1.3),fullbody,ultra-detailed,solo:1.3,cinematic lighting,&lt;Lora:1.0,87c979a2f53f4341bd2c94109738cb14&gt; --ar 3:4 | 无 | ![](img/f65fbee147e2e53b35050c8767a182e1.png) |
| FLUX文生图 Retro movie style film photography,Retro movie style film photography,. This is a photograph of a young Asian woman with fair skin and straight,dark brown hair,which falls just below her shoulders. She has prominent bangs that frame her forehead. Her eyes are large and light in color,and her lips are full and slightly parted. She is looking upward with a serene expression,as if she is admiring the lush greenery around her., She is standing outdoors,with sunlight filtering through the dense,leafy branches of a tree that partially obscures her face. The leaves are vivid green,and the texture of the branches and leaves appears soft and delicate. The background is a mix of green and light blue,suggesting a bright,clear day. She is wearing a light-colored,floral-patterned dress with a sleeveless design,visible at the top of the image. The overall mood of the photograph is calm and peaceful,with a natural,almost ethereal quality.&lt;Ckpt:F1_Photo&gt;&lt;Lora:0.8,e1516a19546143518429b9b42924c01f&gt;&lt;Lora:0.3,5b0a160f9e8645249b69f23166efd832&gt; |  | ![](img/dc50f0086cd897309e69b95777b0218f.png) |
| FLUX文生图 Polaroid style filters,Leica filters,Film photography,qrixi,senxqf,a woman with long dark hair,wearing a multi-colored sleeveless dress,adorned with white beads and silver earrings. She is adorned with a silver necklace and silver bracelets. Her lips are painted a vibrant red,adding a pop of color to the scene. The backdrop,a backdrop of a cityscape,is a body of water,with tall buildings,a bridge,and a few trees. The sky is a pale gray,with a few white clouds. &lt;Lora:0.8,1ee5766e380f45d68937e47516c8db3c&gt;&lt;Lora:0.6,e1516a19546143518429b9b42924c01f&gt; |  | ![](img/eee4eb64da50e3f566baf39d6625f802.png) |
| FLUX文生图  GFSM, XS, masterpiece:1.2),best quality,high resolution,unity 8k wallpaper,(illustration:1),perfect lighting,extremely detailed CG,finely detail,extremely detailed,soft lighting and shadow,soft yet striking lighting,film grain:1.2,(skin pores:1.2),(detailed skin texture:1),((solo:1.5)),Detailed face,(see-through:1.1),misty,Low Key:1.1,depth of field, A woman in a black coat stands confidently amidst an urban backdrop,her gaze directed off to one side.&lt;Lora:1.0,f585ba92347b4e4ab52990fe777c5d58&gt; --ar 3:4 | 无 | ![](img/ed4451f19ce1d62a337aecb7b8deba4a.png) |
| FLUX文生图 cheongsam,beauty girl,The suit leaked big snow-white breasts,masterpiece,oval face,print_cheongsam,zhanghanyun,shiny pantyhose |  | ![](img/02023be050f7a09fa23814eeab43f5f0.png) |
| FLUX文生图 Nice Nature,\(umamusume\),(masterpiece)),((best quality)),(ultra-detailed),illustration,beautiful art,artistic,realistic,8k,wallpaper,beautiful,high resolution,good composition,good art,scenery,cinematic,detailed skin,(oily skin),glossy skin,fine skin,(beautiful hair),(beautiful eyes),hand with five delicate fingers,(perfect anatomy),(correct limb),(realistic),a little girl,small breasts,sweaty,ballroom dance,sequin,(dance dress, flat chest:1.2),skinny,Naked apron,&lt;Ckpt:38f5181f8f0f4702928ae10c2096c7d5&gt;&lt;Lora:0.8,a1aa0018b9ca4e6cbb65daf516286609&gt;--ar 3:4 |  | ![](img/5d322573a12496ecdab27438ff34dbae.png) |
| FLUX文生图 &lt;Ckpt:F1_DRealistic&gt;,seductive leaning forward detailed skin,ultra realistic 8k cg,picture-perfect face,flawless,clean,masterpiece,professional artwork,famous artwork,cinematic lighting,cinematic bloom,(( brown_eyes, brown_hair, )),(1girl, beautiful girl),perfect face,(young:1.1),((fit figure)),((25 year old)),beautiful face,beautiful blue eyes,detail eyes,detail face,(((perfect female body, ))),(large breasts:1.2),(girl,Newspaper dress ,outdoors,from below,full bodylong legs),(nsfw:0.1),Warm colors, | 无 | ![](img/3d56817bd3e997586c53bb2497b238c9.png) |
| FLUX文生图 Black and white line art, Asian woman &lt;Lora:0.7,fdf30e2a449f476da6a678f5391d3ca4&gt; --ar 3:4 | 无 | ![](img/a9e85baedeeb5024d53560e12817ecfa.png) |
| FLUX文生图 luoxue,snow,sdmai,lanmeng,denim,shorts,navel,realistic,jewelry,earrings,thighhighs,long hair,denim shorts,belt,hand in pocket,brown hair,midriff,looking at viewer,short shorts,lips,parted lips,&lt;Ckpt:F1_Photo&gt;&lt;Lora:0.7,fdf30e2a449f476da6a678f5391d3ca4&gt; --ar 3:4 | 无 | ![](img/796b364a9e67ea8d62f017aae3643576.png) |
| FLUX文生图  asian young woman,stylish advanced digital comic illustration by atey ghailan and artgerm,Cosplay costumes,during the day,long hair,looking at the audience,parted lips,realistic,red lips,standing &lt;Lora:1.0,b9c24be385d04984a671b0a89bc0a852&gt; --ar 9:16 |  | ![](img/6c1a70ddaf1d2f7154da492a456357b8.png) |
| FLUX文生图  flat illustration poster of anime adult mature-looking slim chinese girl at new year&#x27;s day,with big excellent designed words&quot;2025&quot;,festival atmosphere,&lt;Lora:1.0,a93008c72d6e4d088034b43f82ae1696&gt; --ar 9:16 | 无 | ![](img/e23245ca3d3fb9abaa4f0b83a210e5e0.png) |
| FLUX文生图 bigbigcat,Long shot,a big cat tail around a girl sitting on a stone,Gongbi style,&lt;Lora:0.8,7b3e0974389d4d208e95d30b5c0e59e6&gt;--ar 9:16 |  | ![](img/aa73e23797acfaf6b3f0453aafac53e9.png) |
| FLUX文生图  big excellent designed words&quot;2025&quot;displayed with snow sparkle in background, nvdi, Rough brushstroke digital painting of one-piece anime female character Boa Hancock in light blue chinese side slit dress with long hair, white snake in background, heavy snowing, outdoors, snowflake,&lt;Lora:0.5,a93008c72d6e4d088034b43f82ae1696&gt;&lt;Lora:0.6,ee68f017ea004534b8c981edf8d123ef&gt;&lt;Lora:0.7,33f0e769d5f64a92af44d8f79ba9d119&gt; --ar 9:16 | 无 | ![](img/5df7aaa8d0684c9ab056fc736a249af9.png) |
| FLUX文生图 photo of white skin asian slim sexy woman in tube bra, looking at viewer, soft natural liht, bokeh, from below, close-up, udder boob,&lt;Ckpt:F1_Photo&gt;&lt;Lora:0.7,90f00c4ab6de404b8161be0ac545fc2a&gt; --ar 9:16 | 无 | ![](img/d08aa3b7b57040f609accab5bb0b94f4.png) |
| FLUX文生图  XS, masterpiece:1.2),best quality,high resolution,unity 8k wallpaper,(illustration:1),perfect lighting,extremely detailed CG,finely detail,extremely detailed,soft lighting and shadow,soft yet striking lighting,film grain:1.2,(skin pores:1.2),(detailed skin texture:1),((solo:1.5)),Detailed face,(see-through:1.1),misty,Low Key:1.1,depth of field, A woman posing on a bed with her back to us,showcasing an off-the shoulder top and denim shorts.&lt;Ckpt:F1_Photo&gt;&lt;Lora:0.8,f585ba92347b4e4ab52990fe777c5d58&gt; --ar 9:16 | 无 | ![](img/60cb40cd79074159b317d30f0f613b28.png) |
| FLUX文生图  GFSM, masterpiece:1.2),best quality,high resolution,unity 8k wallpaper,(illustration:1),perfect lighting,extremely detailed CG,finely detail,extremely detailed,soft lighting and shadow,soft yet striking lighting,film grain:1.2,(skin pores:1.2),(detailed skin texture:1),((solo:1.5)),Detailed face,(see-through:1.1),misty,Low Key:1.1,depth of field, (a woman in a red Dunhuang Flying Apsaras costume sitting on a blue bench, gorgeous chinese model, ceremonial clouds, beautiful fashion model body, royal bird, inspired by Huang Ding, wood chair, thin waist, bust with a long beautiful neck, charismatic beautiful, thin-waist, empress),Dunhuang Flying Apsaras costume,messy long hair,Ji Jian,Dunhuang style,a lot of smoke,superb,elegant colors,(realistic, high detail, masterpiece, super detailed, dynamic angle, mural background, ink wash, amazing, movie lighting, illustration, chiaroscuro, backlighting, god rays, holland, best quality, best quality),((Chinese style:1)),moon,Beautiful lighting among background,Floating colorful sparkles (1) illuminate her glowing skin against the cosmic backdrop,creating a tranquil mosaic reminiscent of blue and green jellyfish (1.4) amidst the photorealistic (1.4) depths.&lt;Ckpt:F1_Photo&gt;&lt;Lora:0.8,f585ba92347b4e4ab52990fe777c5d58&gt; --ar 9:16 | 无 | ![](img/bbbf027b2a009bec029b00c34cfdf53b.png) |
| FLUX文生图 A female swordsman with a cool temperament. Her right hand clenches the sword, her left hand holds a long sword in front of her, the sword body is filled with immortal air, making an aggressive gesture, her eyes are firm and deep, revealing the strength and determination to be underestimated, as if ready to meet any challenge.&lt;Lora:0.7,385ce7433bb74545aac61c536f48ab24&gt;&lt;Lora:0.5,5ec043115d87412a8cac671ca18be4ef&gt;--ar 9:16 | 无 | ![](img/f82fc3e211e05045ffe0a96e937bffca.png) |
| FLUX文生图 1girl,qun,a secondary beauty,best quality,masterpiece,extremely delicate and beautiful,extremely detailed,8K,fine details,navel exposure,&lt;Ckpt:cc6e3e42ffa24526bdeea98bf39e837d&gt;--ar 9:16 | 无 | ![](img/6576fca5cda53beb5139300bfef97928.png) |
| FLUX文生图  XUER INS style,1girl,solo, | 无 | ![](img/83078112caecf6b56f354f40992e63b6.png) |
| FLUX文生图 A surreal composition with flowers bursting in motion around the model, creating an artistic, high-energy visual impact. The flowers seem to be swirling and levitating, blending with the model&#x27;s attire, creating a harmonious yet chaotic artistic expression. Environment: A colorful, whimsical background with light pastel tones and abstract patterns, enhancing the creative and energetic atmosphere. Style: Contemporary fashion photography with a surreal, artistic twist. Lighting: Bright, studio lighting with subtle shadows to bring out the three-dimensional depth of the flowers and the model&#x27;s form. Mood: Creative, artistic, and vibrant. Resolution: Ultra-high resolution to capture the explosion of details in both the flowers and the model&#x27;s attire.&lt;Ckpt:a5e907699dd2427098e87af837efd172&gt; --ar 3:4 | 无 | ![](img/95a623cdf39515f326f9d66edf31a236.png) |
| FLUX文生图 a girl wearing a black belly dance costume, with intricate details and a graceful pose.Her hair is pulled back in a bun and she has a serene expression on her face,standing in a serene natural setting.She is looking directly at the camera with a calm and composed expression.The cave&#x27;s walls are rocky, and there are plants and foliage surrounding the water.&lt;Lora:0.7,3e03bd1ae4d14beabd40962191bb2885&gt; --ar 9:16 | 无 | ![](img/9057175ff3acc11f4e3ad49caa8c49b1.png) |
| FLUX文生图 A female mech warrior&lt;IPAdapter:0.7,https://i.ibb.co/xgwzk3n/20241220053641.png&gt; --ar 3:4 | ![](img/7d00830d76736236526cc6d37688ada8.png) | ![](img/dce1bde2ff7919a19016df666ee700be.png) |
| FLUX图生图 big head,girl wear sunglasses,big head,stand,double ponytail,camisole short skirt,cartoon,&lt;Lora:1.0,8d337b6f590f4105af9b45412abb4594&gt;&lt;denoise:0.85,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4 | ![](img/84abccbd30d21baa25c57e38d20889be.png) | ![](img/257481d28e7afb1ff722a15203398f98.png) |
| SDXL文生图 A female mech warrior&lt;XL_depth:0.7,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4 | ![](img/46e0669cd202d87ca054f0540e564e18.png) | ![](img/37c0654df9548d72f219c73b591efc2d.png) |
| SDXL文生图 &lt;Ckpt:e008596bc0c64ae6969f1f9a50bb37c3&gt;boa boa hancock one piece, collarbone, snake earrings, cowboy shot, bed, lying in bed, high heels, close-up, 1 girl, solo, dress, jewelry, earrings, long hair, sleeves past wrists, bare shoulders, white background, flowers background, close-up details, realistic style photo young woman, outstanding style, tall, cute, in her teens. Use soft lighting to cast gentle shadows on the subject, adding a touch of dimension to your images without compromising details, ear ornament, hair ornament, eyelashes, makeup, light pink eye shadow, light pink lipstick, pale skin, whirls of vapor, iridescent textures, (Anime:1.1), soft neon light, ethereal ambiance, delicate details, magical atmosphere, concept art, highly detailed, ultra-high resolution, 32K UHD, sharp focus, best quality, masterpiece, golden hour, unconventional supreme masterpiece, masterful details, temperate atmosphere, with a high-end texture, in the style of fashion photography, magazine cover, dynamic angle, dynamic posture, (cowboy_shot:1.2), photography, (artist:quasarcake:0.8), extreme aesthetic, (wlop:0.6), (honjou raita, lack, rella, wanke:0.5), emotionalization, visual impact, a shot with tension, impactful picture, official art, colorful, splash of color, movie perspective, amazing quality, very aesthetic, absurdres, --ar 9:16. | 无 | ![](img/e90bbdaeed4cdb6fbd7a584cc7b0b3f0.png) |
| SDXL文生图  CZG,Fluid texture,no humans,black background,orange theme,space,from side,&lt;Ckpt:16c5f1eec32e47c2bc41009e598d2527&gt;&lt;Lora:0.8,bbc080acca124995b3dfbd26e56bb278&gt;&lt;XL_canny:0.75,https://i.ibb.co/ydfXRSw/0da98d078a59.jpg&gt; --ar 3:4 | ![](img/e0167d4a305d4d3c7e87ad3c579f3366.png) | ![](img/4a500d08e37bf2876c143f6ebf97ff8c.png) |
| SDXL文生图  CandyStyle,top view,close view,gummy,candies,stacked,different shapes,different colors,(white background:1.5),studio lighting,Depth of Field,&lt;lora:CandyStyle2XL-000014:1.4&gt;,, Hyper Real, Photorealistic, 16K, high detail, high resolution, (((masterpiece))), (((best quality))), ((ultra-detailed)),&lt;Lora:0.8,3f1b92c93b284ba28a075176c8a02afa&gt;&lt;XL_canny:0.75,https://i.ibb.co/ydfXRSw/0da98d078a59.jpg&gt; --ar 3:4 | 同上 | ![](img/51165a688aba501ef1b2bcd1c5f59ddd.png) |
| SDXL文生图  purple,Plush wool,Plush,soft edge,matching,plush,3D,real,3d model,white wool,Inflation,expansion,soft gradient colors,studio lighting,light reflection, (masterpiece:1.2),exquisite,high resolution,perfectillumination,(Extreme detail cG:1.2),best quality,8K,&lt;Lora:0.8,a3ca70c5932e493ea8e2912fac297ba3&gt;&lt;XL_canny:0.75,https://i.ibb.co/ydfXRSw/0da98d078a59.jpg&gt; --ar 3:4 | 同上 | ![](img/77cd99ff66cadb07e4a294f9031328c3.png) |
| SDXL文生图  China-Chic style,on the water surface,reflection,Pink jadeite material,jade,transparent jade,crystal clear,Light pink jadeite,gold border,gold wire,water surface,lake,Pink peach blossoms on both sides of the lake,There is a transparent peach blossom,Pink peach blossoms,obvious correlation,water flower,pine,cypress,welcoming pine,classical charm,fairy spirit,smoke,ancient architecture,Southeast Asian architecture,cloud,blue sky,OC rendering,3D style,virtual engine,dreamy scenes,masterpieces,high-definition details,super details,minimalist style,C4D,&lt;Ckpt:fb917d74ba8f4b838afc77da0e17d163&gt;&lt;XL_canny:0.75,https://i.ibb.co/ydfXRSw/0da98d078a59.jpg&gt; --ar 3:4 | 同上 | ![](img/8bd309dd8ee024de038abbf4dde6930c.png) |
| SDXL文生图  aigc_huafont,(The letter &quot;Autumn Equinox&quot; **** of flowers and green plants:1.5),Parrot,flower,plant,orange_flower,flower,blue_flower,plant,white_flower,leaf,pink_flower,dinosaur,butterflies,((best quality)),(((masterpiece))),8k&lt;Ckpt:16c5f1eec32e47c2bc41009e598d2527&gt;&lt;Lora:0.8,c32b65e10f6243078415a04185ba2a7d&gt;&lt;XL_canny:0.75,https://i.ibb.co/ydfXRSw/0da98d078a59.jpg&gt; --ar 3:4 | 同上 | ![](img/564192afa3e282be0f7dfb85fadb542a.png) |
| SDXL文生图  shuijinjing font,no humans,abstract,frosted metal,(black_background:1.2),(simple background:1.4),((best quality)),((masterpiece)),8k,metal,&lt;Ckpt:16c5f1eec32e47c2bc41009e598d2527&gt;&lt;Lora:0.8,dde0e5b6d7214bf3bc055f15db84c4ce&gt;&lt;XL_canny:0.75,https://i.ibb.co/ydfXRSw/0da98d078a59.jpg&gt; --ar 3:4 | 同上 | ![](img/b89ee276b05cda485712ebeb18a6b517.png) |

## 四、**绘图模式及参数详解**

### **绘图模式**

+   以下分类方式主要基于官方API接口请求参数的类型

| 底模类型 | **Star-3 Alpha** | **Flux.1 Dev** |  | **SDXL** |  |
| --- | --- | --- | --- | --- | --- |
| 绘图模式/触发词 | 星流文生图 | FLUX文生图 | FLUX图生图 | SDXL文生图 | SDXL人脸参考 |
| 是否支持切换底模 | 否 | 是 | 是 | 是 | 是 |
| Lora最大串联数量 | 0 | 5 | 5 | 5 | 5 |
| 支持的通用参数类型 | --ar：绘图比例 | --ar：绘图比例 | --ar：绘图比例 | --ar：绘图比例 | --ar：绘图比例 |
| 支持的高级参数类型 | line、depth、pose、IPAdapter | IPAdapter | denoise（重绘幅度）、IPAdapter | XL_canny、XL_depth、XL_pose、XL_IPA | InstantID |

### **通用参数**

+   --ar：指定生成图片的宽高比，支持1:1, 4:3, 3:4, 16:9, 9:16，不写则默认1:1

+   --enhance：使用LLM API增强提示词，不写则默认不开启

+   <Ckpt:modelId>：指定使用的底模(星流生图模式下不支持)，modelID可以是预设的底模名称，也可以是32位的UUID（UUID获取方式详见本章第4小节）

+   <Lora:weight,modelId>：指定使用的Lora模型和权重(星流生图模式下不支持)，modelID是32位的UUID

***切记在使用<Ckpt:modelId>或<Lora:weight,modelId>指令时，32位的UUID不要换行（🛰️聊天中换行相      当于插入了空格符号，会破坏UUID的完整性），否则指定的模型不会生效***

### **高级参数**

+   <controlType:weight,imageUrl>：指定使用的ControlNet类型、权重和控制图像链接

+   <IPAdapter:weight,imageUrl>：指定使用的IPAdapter权重和风格参考图像链接

+   <denoise:weight,imageUrl>: 图生图模式指定的重绘幅度和控制图像链接

+   <InstantID:imageUrl>：SDXL人脸参考模式指定的控制图像链接

+    不知道如何获取imageUrl？

可以安装本人开源的[Img2Url插件](https://github.com/Lingyuzhou111/Img2Url)(点击蓝字跳转)，通过在🛰️聊天窗口上传图片即可获得对应链接：

![](img/fa59513ef546189332e1836b8e8cce4a.png)

### **预设底模**

除了星流绘画底模无法选择以外，FLUX和SDXL的绘画底模都经过精挑细选，新手建议使用默认底模，熟练了

之后可以再在插件目录下的config.json文件中根据个人喜好自定义添加或者修改底模名称及其对应的UUID。

+   预设**星流绘画底模**（不可更改）：

+   Star-3 Alpha：Prompt理解更强、美学质量更高的新一代大模型

+   预设**FLUX绘画底模**（可自行添加）：

+   F1_dev：Flux.1Dev，FLUX生图模式默认底模

+   F1_Photo：蓦然回首人像模型，适合生成真实感人像

+   F1_Niji：漫梦琉璃萧瑟模型，类似MJ中Niji的质感

+   F1_Realism：绘声绘色模型，适合生成写实风格图像

+   F1_DRealistic：真彩进阶v1.7，高颜值亚洲人脸模型

+   F1_Architecture：建筑设计模型，专门用于建筑场景

+   F1_illustration：次元插画模型，适合生成动漫风格图像

+   F1_3Dcartoon：3D卡通模型，适合生成3D卡通风格图像

+   F1_IPdesign：IP设计模型，适合生成IP三视图

+   F1_e-commerce：电商场景模型，适合电商产品展示

+   F1_Pixelwave：FluxDev的微调模型，艺术风格表现增强

+   预设**SDXL绘画底模**（可自行添加）：

+   XL_新世界V1：泛化能力较强，SDXL生图模式默认底模

+   XL_筑梦工业V8：筑梦工业V8凛冬奇迹

+   XL_筑梦动漫V5：筑梦动漫V5星辰之愿

+   XL_3D国潮V1：深度结合国潮元素，具备多样化生成能力

+   XL_一青十色V4：绪儿摄影写真模型，脸型触发词chinese girl

+   XL_红尘白梦V3：绪儿动漫人物模型，无需lora直出各种动漫角色

+   XL_高清人像：泛化较强的写实风格人像摄影模型

+   XL_老王建筑：老王建筑室内景观规划大模型

+   XL_AWPortrait：老王人像摄影专用模型

+   更多**自定义底模和Lora**的查找和添加使用方式：

1）在Lib官网检索可商用的Checkpoint和LoRA模型

![](img/96b4a80943450884b39dc6870ee9febe.png)

![](img/600fd546f98fa7bddd801a2e764fbd5d.png)

2）选择喜欢的模型版本，从浏览器网址中复制versionUuid

![](img/3e0c3161ee60a7358b839f549d2a5010.png)

3）粘贴到文生图或图生图的参数模板中使用

![](img/1528834c08b5506fc0bda91ff4d5bd50.png)

![](img/2227d7864662197dc763a078444f3cc9.png)

![](img/4f2c1c2a362d9d5860ff427181c5da04.png)

4）若忘记了在生图参数中应用的模型是哪一款，可以使用“哩布模型+模型ID”进行查询

![](img/f77893d5c5f9533dc9fabe8c2058a1b5.png)

## 五、高级参数及使用示例

### **星流文生图**

+   支持的高级参数类型：line、depth、pose、IPAdapter

+   参数格式：

+   星流文生图[图像描述]<line:0.75,https://example.com/image.jpg> --ar 3:4

+   星流文生图[图像描述]<depth:0.75,https://example.com/image.jpg> --ar 3:4

+   星流文生图[图像描述]<pose:0.75,https://example.com/image.jpg> --ar 3:4

+   星流文生图[图像描述]<IPAdapter:0.75,https://example.com/image.jpg> --ar 3:4

+   使用示例：

| **参考图** | **控制类型：line** | **控制类型：depth** | **控制类型：pose** |
| --- | --- | --- | --- |
| ![](img/77503c405e2ab2669789bb737343e68d.png) | ![](img/ba2524c966afe256ee8258d38fcebf89.png) | ![](img/f4f7924a7d92ebdb0264226a40fbaf1d.png) | ![](img/f96566814d8ed13b0b0aaeee528fa277.png) |
| +   带高级参数的星流文生图提示词示例➡️ | 星流文生图  shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,cinematic lighting effects,&lt;line:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4 | 星流文生图  shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,cinematic lighting effects,&lt;depth:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4 | 星流文生图  shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,cinematic lighting effects,&lt;pose:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4 |
| **控制能力** | 弱（可以看到脸上的头发实际就是左边参考图的人脸轮廓，说明它识别到了但就是不按线稿轮廓来） | 强（高度遵循提示词的同时，人物姿态和景深关系都得到了很好的还原） | 一般（可以看到人脸和五官基本上控制住了，但是肢体姿势控制不佳） |
| **推荐程度** | ⭐️ | **⭐️⭐️⭐️⭐️** | ⭐️⭐️ |

| **参考图** | **控制类型：IPAdapter** |
| --- | --- |
| ![](img/38c2c87a5107345269d6a5cc9b9afc0f.png) | ![](img/c5204255d7f0e44a7236e9cd9ba8bf33.png) |
| +   带高级参数的星流文生图提示词示例➡️ | 星流文生图  shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,cinematic lighting effects, &lt;IPAdapter:0.75,https://i.ibb.co/BrvXSwk/48ca5988f25a.png&gt; --ar 3:4 |
| **控制能力** | 强（高度遵循提示词的同时，画面风格和构图也得到了很好的迁移） |
| **推荐程度** | **⭐️⭐️⭐️⭐️** |

### **FLUX文生图**

+   支持的高级参数类型：IPAdapter（官方暂未开放其他控制类型的API接入方式）

+   参数格式：

+   FLUX文生图[图像描述]<IPAdapter:0.75,https://example.com/image.jpg> --ar 3:4

+   使用示例：

| **参考图** | **控制类型：IPAdapter** |
| --- | --- |
| ![](img/e98271d26d8b323011fca6db5ab43502.png) | ![](img/2ff4a837c90b037a3dcec8c5d133e6bf.png) |
| +   带高级参数的FLUX文生图提示词示例➡️ | FLUX文生图 Profile portrait of an Asian woman&lt;IPAdapter:0.7,https://i.ibb.co/8dGWSXk/48ca5988f25a.png&gt; --ar 3:4 |
| **控制能力** | 强（高度遵循提示词的同时，画面风格和构图也得到了很好的迁移） |
| **推荐程度** | **⭐️⭐️⭐️⭐️⭐️** |

### **FLUX图生图**

+   支持的高级参数类型：denoise（重绘幅度）、IPAdapter

***所谓图生图，本质上就是对原图进行逆采样(将一张图片逐步由像素恢复为原始噪点)的过程，因此相对原图     而言，降噪幅度(也就是重绘幅度)越低，生成的结果变化越小；降噪幅度越高，留给AI发挥的空间也就越大***

+   参数格式：

+   FLUX图生图[图像描述]<denoise:0.75,https://example.com/image.jpg> --ar 3:4

+   使用示例：

| **参考图** | **图生图类型：写实** | **图生图类型：卡通** | **图生图类型：动漫** |
| --- | --- | --- | --- |
| ![](img/7fb2f493f39f343f5971982e1e2dd453.png) | ![](img/e6991fb6af4fee3b5a845a86e56196af.png) | ![](img/e3e03997594cc9f44f31031fd0354bea.png) | ![](img/e54c00a062f2994e5b0eda8b52ed6192.png) |
| +   FLUX图生图提示词示例➡️ | FLUX图生图 Asian portrait,A young woman wearing a green baseball cap, close shot, background is coffee store, masterpiece, best quality &lt;Ckpt:F1_Photo&gt;&lt;denoise:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4 | FLUX图生图 Asian women, Disney style &lt;Ckpt:F1_3Dcartoon&gt;&lt;denoise:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4 | FLUX图生图 Asian women, black and white line art&lt;Ckpt:F1_illustration&gt;&lt;denoise:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4 |

### **SDXL文生图**

+   支持的高级参数类型：XL_canny、XL_depth、XL_pose、XL_IPA

+   参数格式：

+   SDXL文生图[图像描述]<XL_canny:0.75,https://example.com/image.jpg> --ar 3:4

+   SDXL文生图[图像描述]<XL_depth:0.75,https://example.com/image.jpg> --ar 3:4

+   SDXL文生图[图像描述]<XL_pose:0.75,https://example.com/image.jpg> --ar 3:4

+   SDXL文生图[图像描述]<XL_IPA:0.75,https://example.com/image.jpg> --ar 3:4

+   使用示例：

| **参考图** | **控制类型：XL_canny** | **控制类型：XL_depth** | **控制类型：XL_pose** |
| --- | --- | --- | --- |
| ![](img/5557a3fdd34c393abc157bb70ad9f53c.png) | ![](img/4d572ff961caed4d9fd932f5e81166ef.png) | ![](img/139f54ed537f8da22aba772459c5236a.png) | ![](img/3eae92faee5c32dc2a0e1c9f18352342.png) |
| +   带高级参数的SDXL文生图提示词示例➡️ | SDXL文生图  shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,cinematic lighting effects,&lt;XL_canny:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4 | SDXL文生图  shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,cinematic lighting effects,&lt;XL_depth:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4 | SDXL文生图  shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,cinematic lighting effects,&lt;XL_pose:0.75,https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4 |
| **控制能力** | 一般 | 强 | 弱 |
| **推荐程度** | ⭐️⭐️**⭐️** | **⭐️⭐️⭐️⭐️** | ⭐️ |

| **参考图** | **控制类型：XL_IPA** |
| --- | --- |
| ![](img/3a0bcf9c354e211ec8eb4ffeb9d0874c.png) | ![](img/4917362854e2dd2d7b726ce97586a7cc.png) |
| +   带高级参数的SDXL文生图提示词示例➡️ | SDXL文生图  shuimo,Epic CG masterpiece, A portrait of a Chinese girl with a gorgeous headpiece,exquisite makeup,gradient color background,collarbone visible,Sexy,cleavage, Smooth skin. Gentle smile. The wind blows her long hair,there are petals in the air, dynamic poses,stunning colors,cinematic lighting effects,&lt;Ckpt:XL_筑梦动漫V5&gt;&lt;XL_IPA:0.75,https://i.ibb.co/BrvXSwk/48ca5988f25a.png&gt; --ar 3:4 |
| **控制能力** | 强（高度遵循提示词的同时，画面风格和构图也得到了很好的迁移） |
| **推荐程度** | **⭐️⭐️⭐️⭐️** |

### **SDXL人脸参考**

+   支持的高级参数类型：InstantID

+   参数格式：

+   SDXL人脸参考[图像描述]<InstantID:https://example.com/image.jpg> --ar 3:4

| **参考图** | **控制类型：InstantID** | **参考图** | **控制类型：InstantID** |
| --- | --- | --- | --- |
| ![](img/f3889d5ca8907c856d1333e5435f7772.png) | ![](img/8d0a2eb63ce05feae9fea26f78afc09d.png) | ![](img/82a4f58d08ada80df715403dabb4c901.png) | ![](img/a5ad44160773972ffd5376c7231df172.png) |
| +   SDXL人脸参考提示词示例➡️ | SDXL人脸参考 Asian portrait,A young woman wearing a green baseball cap, close shot, background is coffee store, masterpiece, best quality &lt;InstantID:https://i.ibb.co/wYcLG0G/6a611349447e.jpg&gt; --ar 3:4 | +   SDXL人脸参考提示词示例➡️ | SDXL人脸参考 Asian portrait,A young woman wearing a green baseball cap, close shot, background is coffee store, masterpiece, best quality &lt;InstantID:https://i.ibb.co/hLdRBN8/c35dfbf6ece3.png&gt; --ar 3:4 |

## 六、常见问题

1.  生成失败？

+   检查 API 密钥是否正确

+   检查参数格式是否正确

+   验证网络连接是否正常

+   确认当日使用次数是否超限

1.  图片质量不理想？

+   尝试调整提示词

+   参考Lib官网别人的提示词

+   选择更适合的模型

+   使用提示词增强功能

## 七、**相关链接**

+   Liblib API 开放平台主页

+   [Liblib API 文档](https://liblibai.feishu.cn/wiki/UAMVw67NcifQHukf8fpccgS5n6d)

+   [插件作者 GitHub 主页](https://github.com/Lingyuzhou111)

+   插件定制咨询请扫码联系

![](img/c7b9648a203815089f8d727c566cc101.png)

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>