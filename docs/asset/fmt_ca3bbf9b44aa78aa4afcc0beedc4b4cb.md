
# XingliuArt - Liblib官方API

> 来源：[https://my.feishu.cn/docx/PZoGdfo7TolfOpxlVHYcnIZpnQg](https://my.feishu.cn/docx/PZoGdfo7TolfOpxlVHYcnIZpnQg)

## 原文 [🤖️【XingliuArt】AI绘画插件使用指南](https://rq4rfacax27.feishu.cn/wiki/TLgrw1rJ8iI9dPkySoMcTxiOnvg)

注：本期的插件为订制插件，作者未开源，需要的联系老金

XingliuArt 是一个适用于 `Chatgpt-on-wechat` 项目的 AI 绘图插件，基于 Liblib 官方 API 开发，支持多种绘图模型及自定义参数设置，让用户仅仅使用 🛰️ 聊天工具就能轻松生成高质量的 AI 图像。

## 一、插件特点及开发目的

### 插件特点

+ 优势

+ 功能强大易上手，支持多种绘图模式，无需理解复杂的参数界面，仅靠 🛰️ 聊天轻松复刻 SDwebUI 的效果

+ 会员模型随便用，无限制使用 Liblib 官网的会员模型，不会像非会员在网页端使用有次数限制和排队限速

+ 生成图片可商用，可出售生成图片或用于商业目的，含 F.1 商用使用权

+ 随用随充更灵活，注册即送 500 积分，用多少充多少，且积分永久有效，不会像月会员过期清零

+ 局限性

+ 不能使用非商业许可模型（这一点没有办法替代 Liblib 网页端）

+ 出图含内容审核，不可使用违禁词和敏感词

### 开发目的

无需高配电脑，方便用户随时随地通过 🛰️ 聊天工具即可开启 AI 绘画之旅，生成可商用的高质量 AI 作品：

+ 对于没有 AI 绘画基础的人，这款插件旨在让用户通过最简单的方式快速得到正反馈，而不是一上来就被复杂的参数界面和晦涩的专业术语所劝退

+ 对于有专业绘画需求的人，这款插件能够让用户通过最轻量化的方式快速实现各种 Checkpoint 和 Lora 模型效果测试，而无需耗费大量时间等待模型下载和参数调试

## 二、插件安装和使用方法

### 获取密钥

访问 Liblib API 开放平台主页，注册账号并获取 `access_key` 和 `secret_key`

### 安装插件

下载 XingliuArt 插件包，解压后将 `XingliuArt` 文件夹上传至 `chatgpt-on-wechat` 下的 `plugins` 目录

确保目录层级符合以下格式：

```
plugins
└── XingliuArt
    ├── config.json
    ├── XingliuArt.py
    ├── LibModel.py
    └── __init__.py
```

### 配置密钥

+ 将密钥填入 `config.json` 文件

+ 要使用提示词自动优化功能，需配置相应的 `enhance_prompt_api` 信息，推荐使用智谱或硅基免费模型

（当然使用付费模型效果会更好）

+ 设置每日插件使用次数，可避免积分消耗过快

+ 设置管理员昵称为自己的 🛰️ 昵称，可在 🛰️ 群聊中使用管理员专属指令“禁用星流绘画”和“开启星流绘画”

```
{
  "access_key": "your_liblib_access_key",
  "secret_key": "your_liblib_secret_key",
  "enhance_prompt_api_url": "any_llm_api_url",
  "enhance_prompt_api_key": "any_llm_api_key",
  "enhance_model": "any_llm_model",     
  "daily_limit": 50,
  "admin_nicknames": ["管理员昵称1", "管理员昵称2"]
}
```

### 重启 `chatgpt-on-wechat`

重启 `chatgpt-on-wechat` 项目并输入 `#scanp` 命令扫描新插件

在 🛰️ 聊天窗口输入 `#help XingliuArt` 查看帮助信息，成功返回则说明插件安装成功

## 三、指令参考及效果展示

+ 以下案例均为本人 🛰️ 端直出效果，可以直接复制或略作修改后使用（后面会不定期更新，建议收藏）

+ 注意有些案例使用了 `--enhance` 提示词自动优化指令，只复制优化前的提示词即可

| 绘图指令 | 参考图片 | 生成结果 |

| --- | --- | --- |

| `星流文生图 minimalism,Chinese ink painting,ink painting,1girl,solo, upper body,black hair,long_hair,hair bun,earrings,jewelry, hair ornament,makeup,eyeshadow, looking at viewer,off-shoulder,hanfu, long sleeves, closed mouth,flower, bamboo, leaf,simple background,master works, flat, high definition, --ar 3:4` | 无 | ![](img/4d635a7908657735869ac815eb4d33d1.png) |

| `星流文生图 Illustration,AYU,Artistic illustration,A young girl with long, wavy hair and a cozy sweater, sitting at a wooden table in a modern, warmly lit Starbucks. She holds a steaming cup of cappuccino, adorned with a delicate heart design in the foam. The background features large windows with a cityscape view, green indoor plants, and soft, ambient lighting. The scene is captured in a high-resolution, realistic style with vibrant colors, emphasizing the inviting atmosphere and the intricate details of the coffee cup and the girl's thoughtful expression. --ar 9:16` | 无 | ![](img/6154574e355198e1eabb012b286592f5.png) |

| `星流文生图 Whimsical cartoon style, a small person with rosy cheeks and wild hair racing a toy car with flames and sparks, in a vibrant, exaggerated environment with bold black outlines, bright pastel colors, and dynamic textures, emphasizing the character's joyful, toothy grin and playful atmosphere.--ar 3:4` | 无 | ![](img/077c62f247160b0a8681c3e1b5eb07d4.png) |

| `FLUX文生图 sunglasses,big head,girl wear sunglasses,big head,stand,double ponytail,camisole short skirt,cartoon,<Lora:1.0,8d337b6f590f4105af9b45412abb4594> --ar 3:4` | 无 | ![](img/4ac5f69bff95bb67088820b2de51cb11.png) |

| `FLUX文生图 waitgoo,Microscale landscape, super close-up, shallow depth of field, among Van Gogh's starry sky, on a luminous boat, sitting a little prince holding a bright red rose, furry sea, Van Gogh style, dreamy scattered scenery, streamer, swirling starry sky, brilliant starry man, bioluminescence, ultra-fine section, 3D stereo` | 无 | ![](img/1d167fa4e8648a142d32ee21b98f00a9.png) |

| `FLUX文生图 advanced digital art, poster of new_year's day with big designed words"2025", flower and butterfly in background, <Lora:1.0,a93008c72d6e4d088034b43f82ae1696> --ar 3:4` | 无 | ![](img/e95cb7f4ec5ab37d78cf7b6466086bcb.png) |

| `FLUX文生图 zihui_f1, this is a lively picture A vivid, colorful, cartoon-style digital drawing depicting the Tesla logo in a lively, colorful and stylized way. It features a large statue of an astronaut wearing a helmet and a space suit standing atop a rock outcrop. The statue is surrounded by a series of smaller stylized elements including spaceships, satellites, flying saucers and rockets, and auspicious clouds. The background is pure white, ensuring that the focus remains on the stylized colorful illustration, creating a sci-fi atmosphere. It is surrounded by lush green plants and castle-like buildings. The illustration is rich in detail, with smooth lines and bright colors, which enhances the ethereal and sci-fi-like quality of the scene.<Lora:0.85,bdeb40ebcc754939a5f2971c777fe556>--ar 3:4` | 无 | ![](img/f9e94073d505c273d8d1a047e5528f48.png) |

| `FLUX文生图 zihui_f1, This is a vibrant, colorful, cartoon-style digital drawing with a huge, stylized huawei logo in the center. It features a giant Chinese dragon hovering over the top of the logo. Surrounding the dragon are a series of smaller stylized elements, including spaceships, satellites, flying saucers, and rockets. The background is pure white, ensuring that the focus remains on the stylized huawei logo color illustration, creating a sci-fi atmosphere. It is surrounded by lush green plants, auspicious clouds, and ancient Chinese palaces. The illustration is rich in detail, with smooth lines and bright colors, which enhances the ethereal and sci-fi feel of the scene.<Lora:0.85,bdeb40ebcc754939a5f2971c777fe556>--ar 3:4` | 无 | ![](img/c2808c7c5dd421556a9934c6fca60835.png) |

| `FLUX文生图 钢铁侠，写意水墨风格` | 无 | ![](img/dcf580570de43a9cb65384487dbf04dd.png) |

| `FLUX文生图 扁平插画风格，一个汉服美女 <Ckpt:8e53c30ec1094993ab2b3343b7187169><Lora:0.8,aa6542ca704e438d9a41e43ca8e69091> --ar 3:4 --enhance` | 无 | ![](img/9ed139eeadcecc0e31162ea02e4babd3.png) |

| `FLUX文生图 JJFG, Minimalist style characters,JJFG,Minimalist style characters,A stylized,high-quality digital illustration of a woman with pale skin,black hair styled in an elegant updo,and wearing a black outfit with a gradient green and red scarf. She has red lips,blue eyeshadow,and large,geometric earrings with dangling elements. The background is a soft light blue,and the composition follows the rule of thirds. The image has a shallow depth of field,with the background slightly blurred. The lighting is soft and even,with no strong shadows. The aesthetic quality is very high.,<Ckpt:8e53c30ec1094993ab2b3343b7187169><Lora:0.8,aa6542ca704e438d9a41e43ca8e69091> --ar 3:4` | 无 | ![](img/b2d0dbc41d66120c0fa17e6780c56e85.png) |

| `FLUX文生图 diffusion,1girl,solo,gradient background,gradient,dress,black dress,gloves,black hair,black gloves,purple background,closed eyes,elbow gloves,hair bun,feathers,<Lora:0.8,ee774a57fe5a4d51adf7c3ad80276ba3>--ar 3:4` |  | ![](img/66ef110589bc7fbf09325558d768ad85.png) |

| `FLUX文生图 Ink sketch,color ink painting,((HRD, HUD, 8K)),((masterpiece, best quality)),highly detailed,soft light, InkPainting-Xiake, Donald Trump,Donald Trump's Classic Actions,male focus,solo,(Black and white background of Tai Chi diagram:1.3),Medium Long Hair, Donald Trump,1man,hands in trouser pockets,Perfect body proportions,<Lora:0.8,af6608f886164a0e902bccc3b0eeae98> --ar 3:4` | 无 | ![](img/788c147a26542b3ebc19f9e25eba1211.png) |
