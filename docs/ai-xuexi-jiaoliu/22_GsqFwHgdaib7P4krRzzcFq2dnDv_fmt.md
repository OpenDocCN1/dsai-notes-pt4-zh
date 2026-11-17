
# 实操案例

> 来源：[https://zcareers.feishu.cn/docx/EuLDdlsdFoPOIhxiidrcDvGynGe](https://zcareers.feishu.cn/docx/EuLDdlsdFoPOIhxiidrcDvGynGe)

## 案例1

比如我们需要做一个3D卡通形象，我们先来梳理下关键词Prompt

| **描述** | **描述内容** | **英文翻译** |
| --- | --- | --- |
| **主题描述** | 一个时尚个性的小女孩坐在石头上，长卷发，金色头发，卡通风格，魅惑的眼睛，带着个性的帽子 | A stylish and individualistic little girl is sitting on a rock, with long curly hair, golden locks, and a cartoon-style. Her captivating eyes are adorned with a unique hat |
| **环境背景** | 周围环绕着机械和线条 | There are mechanical and linear elements surrounding the scene |
| **艺术风格** | 皮克斯风格，渐变色彩，盲盒，干净的背景 | This is a description of an artwork or design that is in the style of Pixar. It features gradient colors, is presented in a blind box, and has a clean background |
| **视角和灯光** | 自然光线，超级细节，3D，景深效果，超级写实，光线追踪，OC渲染，3D渲染，8K | natural lighting, Super Detail, 3D, Depth of Field, , super realistic, light tracking, complex details, OC renderer, Ultra HD,  3D rendering, 8K |
| **辅助参数** | 默认状态1:1（`-- ar 1:1`） | / |
| **汇总Prompt** | A stylish and individualistic little girl is sitting on a rock, with long curly hair, golden locks, and a cartoon-style. Her captivating eyes are adorned with a unique hat::There are mechanical and linear elements surrounding the scene::This is a description of an artwork or design that is in the style of Pixar. It features gradient colors, is presented in a blind box, and has a clean background::natural lighting, Super Detail, 3D, Depth of Field, , super realistic, light tracking, complex details, OC renderer, Ultra HD,  3D rendering, 8K |  |

出图效果：

![](img/c6815f8a2154d4a47b9fb1a4b619983d.png)

![](img/e995c569d75296e5446f14bffc8687c2.png)

## 案例2

这次尝试用诗词来作画。

| **描述** | **描述内容** | **英文翻译** |
| --- | --- | --- |
| **主题描述** | 与世隔绝的小镇，水光潋滟晴方好，山色空蒙雨亦奇 | isolated town,The brimming waves delight the eye in sunny days::The dimming hills present rare view in rainy haze |
| **环境背景** | 白云，群山，湖面 | white clouds, mountains, lake |
| **艺术风格** | 日本动画风格，宫崎骏风格 | Anime,Animation,Hayao Miyazaki style |
| **视角和灯光** | 大远景，自然光线，超级细节，光线追踪，电影视角，8K | Extra Long Shot(ELS),natural lighting, Super Detail,light tracking,Cinematic shot,8K |
| **辅助参数** | 众横比16:9 | `--ar 16:9` |
| **汇总Prompt** | isolated town,The brimming waves delight the eye in sunny days::The dimming hills present rare view in rainy haze,white clouds, mountains, lake,Anime,Animation,Hayao Miyazaki style,Extra Long Shot(ELS),natural lighting, Super Detail,light tracking,Cinematic shot,8K --ar 16:9 |  |

![](img/0cf6fa37d57130d9dd3b860a8d972428.png)

![](img/d452d951ca168540071effb6d1425c87.png)

![](img/ac9b3b92a04d0f4831702dc7989ad2d0.png)

![](img/3aad9a51db65079ef88165be4bd49672.png)

![](img/2fcfdcea6ba8edeabaa0d1d7adbf4c52.png)

## 案例3

| **描述** | **描述内容** | **英文翻译** |
| --- | --- | --- |
| **主题描述** | 一个身着汉服的中国侠客，手持利器，目光坚毅，英姿飒爽 | A Chinese swordsman wearing Hanfu, holding a sharp weapon, with a determined look and heroic posture |
| **环境背景** | 清晨的山林，薄雾缭绕，阳光透过树叶照射下来，显得异常清新 | A mountain forest in the early morning, shrouded in mist, with sunlight filtering through the leaves, giving it an exceptionally fresh look |
| **艺术风格** | 传统水墨画风格，线条简洁流畅，气势磅礴，画面充满中国古典美感 | Traditional ink painting style, with simple and smooth lines, a majestic atmosphere, and full of classical Chinese beauty |
| **视角和灯光** | 俯视角度，画面将侠客的上半身展现得淋漓尽致，彰显其威武不屈的气质，自然光线，太阳的光芒透过树叶投射在侠客身上，形成柔和的光影，为画面增添了神秘感和浪漫气息 | A bird&#x27;s-eye view, displaying the swordsman&#x27;s upper body in great detail, showcasing his resolute and unyielding temperament,Natural light, with the sun&#x27;s rays shining through the leaves and casting soft shadows on the swordsman, adding a sense of mystery and romance to the scene |
| **辅助参数** | 众横比3:4 | `--ar 3:4` |
| **汇总** | A Chinese swordsman wearing Hanfu, holding a sharp weapon, with a determined look and heroic posture::A mountain forest in the early morning, shrouded in mist, with sunlight filtering through the leaves, giving it an exceptionally fresh look::Traditional ink painting style, with simple and smooth lines, a majestic atmosphere, and full of classical Chinese beauty:: A bird&#x27;s-eye view, displaying the swordsman&#x27;s upper body in great detail, showcasing his resolute and unyielding temperament,Natural light, with the sun&#x27;s rays shining through the leaves and casting soft shadows on the swordsman, adding a sense of mystery and romance to the scene --ar 3:4 |  |

## 案例4

| **描述** | **描述内容** | **英文翻译** |
| --- | --- | --- |
| **主题描述** | 未来世界的蒙面中国侠客 | A masked Chinese warrior in a futuristic world |
| **环境背景** | 高科技未来城市，充满了机器人和飞行器，建筑高耸入云 | A high-tech futuristic city filled with robots and flying vehicles, with towering buildings reaching into the clouds |
| **艺术风格** | 科幻风，未来主义，视觉效果明亮而强烈，线条硬朗 | Science fiction and futuristic, with a bright and intense visual effect and sharp lines |
| **视角和灯光** | 镜头低于人物，强调视觉效果，令人产生敬畏感,冷色调的荧光灯，营造出冷酷无情的氛围 | A low-angle shot, emphasizing the visual effect and creating a sense of awe in the viewer::Cold fluorescent lighting, creating a cold and ruthless atmosphere |
| **辅助参数** | 众横比3:4 | `--ar 3:4` |
| **汇总** | A masked Chinese warrior in a futuristic world, A high-tech futuristic city filled with robots and flying vehicles, with towering buildings reaching into the clouds::Science fiction and futuristic, with a bright and intense visual effect and sharp lines::A low-angle shot, emphasizing the visual effect and creating a sense of awe in the viewer,Cold fluorescent lighting, creating a cold and ruthless atmosphere --ar 3:4 |  |

![](img/1100d254f4a5d0430919ae9d9ea1223e.png)

![](img/6da4db006945f67c9e0843a88590c808.png)

**Digital **avatar, solid color background, futuristic and technological style, fashionable, front-facing view, highlighting the features and expression of the digital avatar, soft lighting emphasizing the contours and details, highlighting the three-dimensional sense of the digital avatar,3d,oc rander `--ar 3:4`

![](img/492aff3613f5d541e9345befe77cdc96.png)
