
# 🎬 即梦 Seedance 2.0 使用手册（全新多模态创作体验）

> 来源：[https://aigcreative.feishu.cn/docx/UoKxd4WNworCP5x4OG3cXGZynUd](https://aigcreative.feishu.cn/docx/UoKxd4WNworCP5x4OG3cXGZynUd)

> 请在即梦官方网站体验，保证最佳效果哦！

> 官方网站：https://jimeng.jianying.com/

> Seedance2.0模型已全量上线。

> 最近使用人数过多，服务器压力较大，可以尝试Seedance 2.0 Fast模型获取更流畅的体验（现在知道的人还比较少哦～）

## 🌀 视频 Seedance 2.0 正式上线即梦！

还记得从只能用文字和首/尾帧「讲故事」的那天起，我们就想做出一个真正听得懂你表达的视频模型。今天，它真的来了！

> 即梦 Seedance 2.0 现在支持图像、视频、音频、文本四种模态输入，表达方式更丰富，生成也更可控

你可以用一张图定下画面风格，用一个视频指定角色的动作和镜头的变化，再用几秒音频带起节奏氛围……搭配提示词，让创作过程变得更自然、更高效，也更像真正的“导演”。

这次升级中，“参考能力”是最大亮点：

+ 📷 参考图像可精准还原画面构图、角色细节

+ 🎥 参考视频支持镜头语言、复杂的动作节奏、创意特效的复刻

+ ⏱ 视频支持平滑延长与衔接，可按用户提示生成连续镜头，不止生成，还能“接着拍”

+ ✂️ 编辑能力同步增强，支持对已有视频进行角色更替、删减、增加

我们知道，视频创作从来不仅是“生成”，更是对表达的控制。2.0 不只是多模态，更是一种真正可控的创作方式

> Seedance 2.0，多模态创作，从这里启程。请你们大胆想象，其余的交给它

### 参数预览

| 核心维度 | Seedance 2.0 |

| --- | --- |

| 图片输入 | + ≤ 9 张 |

| 视频输入 | + ≤ 3 个，总时长［2,15］s |

| 音频输入 | + ≤ 3 个，总时长不超过15s |

| 文本输入 | 自然语言 |

| 生成时长 | ≤ 15s，可自由选择4-15s |

| 声音输出 | 自带音效/配乐 |

| 交互限制：目前支持的混合输入总上限是 12 个文件。建议优先上传对画面或节奏影响最大的素材，合理分配不同模态的文件数量 | |

### 交互形式

#### ⚠️注意：

即梦Seedance 2.0 支持「首尾帧」和「全能参考」入口，智能多帧和主体参考无法选中。若你只上传首帧图 + `prompt`，可走首尾帧入口；如需多模态（图、视频、音频、文本）组合输入，则需进入全能参考入口

当前支持的交互方式是通过 `@素材名` 来指定每个图片、视频、音频的用途，例如：`@图片1` 作为首帧，`@视频1` 参考镜头语言，`@音频1` 用于配乐

#### ⚠️温馨提示：关于上传写实人脸素材的说明

由于平台合规要求，目前暂不支持上传包含写实真人脸部的素材（图片和视频素材均不行）。为保障用户权益与生成安全，系统会自动拦截此类素材，上传后将无法生成视频内容。

这意味着如果你上传了真人照片（尤其是清晰可辨识的人脸），模型将无法处理对应生成。

我们理解这可能会带来一些限制，但这一措施是为了确保内容安全和平台规范运行。感谢大家的理解与配合！后续如果有 `update` 我们会及时在文档里更新，再次感谢理解～

---

下面是一些不同场景下的用法和玩法，帮助你更好地理解 Seedance 2.0 在生成质量上、控制能力和创意表现上的升级。如果你还不知道从哪开始，不如先看看这些例子，激发灵感～

---

## 即梦Seedance 2.0 能力 / 提升预览

### 基础能力显著增强：更稳、更顺、更像真的！

不只是多模态，Seedance 2.0 在基础层面显著增强，物理规律更合理、动作表现更自然流畅、指令理解更精准、风格保持更稳定，不仅能稳定完成复杂动作、连续运动等高难度生成任务，也让整体视频效果更真实、更顺滑，是一次底层能力的全面进化！

Case：

| `prompt` | `img` | `vid1` |

| --- | --- | --- |

| 女孩在优雅的晒衣服，晒完接着在桶里拿出另一件，用力抖一抖衣服。 | ![](img/e91274cfa1efec6307e32c7167b907f0.png) | |

| 画里面的人物心虚的表情，眼睛左右看了看探出画框，快速的将手伸出画框拿起可乐喝了一口，然后露出一脸满足的表情，这时传来脚步声，画中的人物赶紧将可乐放回原位，此时一位西部牛仔拿起杯子里的可乐走了，最后镜头前推画面慢慢变得纯黑背景只有顶光照耀的罐装可乐，画面最下方出现艺术感字幕和旁白：“宜口可乐，不可不尝！” | ![](img/8bc0e3846b52d091039a97d53aece462.png) | |

| 镜头小幅度拉远（露出街头全景）并跟随女主移动，风吹拂着女主的裙摆，女主走在19世纪的伦敦大街上；女主走着走着右边街道驶来一辆蒸汽机车，快速驶过女主身旁，风将女主的裙摆吹起，女主一脸震惊的赶忙用双手向下捂住裙摆；背景音效为走路声，人群声，汽车声等等 | ![](img/9c7fc1236a3da1da7a4cd911849ebd0b.png) | |

| 镜头跟随黑衣男子快速逃亡，后面一群人在追，镜头转为侧面跟拍，人物惊慌撞倒路边的水果摊爬起来继续逃，人群慌乱的声音。 | ![](img/24e5fd99f3e6537f656268d76d677f51.png) | |

![](img/b4e876dfe859fe758e7faf984dbe4a02.png)

[jimeng-2026-02-06-4829-固定镜头，女孩在优雅的晒衣服，晒完接着在桶里拿出另一件，用力抖一抖衣服.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/U6DfbRrIsoFLjdxKcRecTJo3nrI)

![](img/96d7dd26323f04eee4d1585da55a6fe6.png)

[jimeng-2026-02-05-3220-画里面的人物心虚的表情，眼睛左右看了看探出画框，快速的将手伸出画框拿起可乐喝了一....mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IbZfbY2E7owB2KxHlbicg5nQnZc)

![](img/2ed6df4adda05ea989798b11491e9a5d.png)

[jimeng-2026-02-05-9575-镜头小幅度拉远（露出街头全景）并跟随女主移动，风吹拂着女主的裙摆，女主走在19世....mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Lmr8bNXNXoG0oBxCJvlc2mCNnUb)

![](img/23e3e5c91e33ebe69c5db5d6f71be3d4.png)

[jimeng-2026-02-06-5100-镜头跟随黑衣男子快速逃亡，后面跟着一群人在追逐，镜头侧面跟拍，人物惊慌撞倒路边的....mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GKr9bEPAKoqmI4xYNLAc26hbnPb)

### 多模态全面升级：视频创作进入“自由组合”时代！

#### Seedance 2.0 多模态介绍

+ 支持上传文本、图片、视频、音频，这些素材都可以被用作使用对象或参考对象。你可以参考任何内容的动作、特效、形式、运镜、人物、场景、声音，只要提示词写得清楚，模型都能理解。

+ Seedance 2.0 = 多模态参考能力（可参考万物） + 强创意生成 + 指令响应精准（理解力很棒）

+ 用自然语言描述你想要的画面和动作就可以啦，明确是参考，还是编辑～素材多的时候，建议你多检查一下各个 `@对象` 有没有标清楚，别把图、视频、角色搞混了哦

#### 特殊使用方式（不设限，仅供参考）：

+ 有首帧/尾帧图？还想参考视频动作？

 → 提示词中写清楚，如：“`@图1`为首帧，参考`@视频1`的打斗动作”

+ 想延长一个已有的视频？

 → 说明延长时间，如“将`@视频1`延长 5s”，注意：此时选择的生成时长应为“新增部分”的时长（例如延长 5s，生成长度也选 5s

+ 想融合多个视频？

 → 提示词中说明合成逻辑，如：“我要在`@视频1`和`@视频2`之间加一个场景，内容为xxx”

+ 没音频素材？可以直接参考视频里的声音

+ 想生成连续动作？

 → 可以在提示词中加入连续性描述，如：“角色从跳跃直接过渡到翻滚，保持动作连贯流畅”`@图1``@图2``@图3`...

#### 一致性全面提升

你可能遇到过这些烦恼：画面里人物前后长得不一样、商品细节丢了、小字模糊、场景跳变、镜头风格无法统一……这些在创作中常见的一致性问题，现在在 2.0 中都能被解决。从人脸到服装，再到字体细节，整体一致性更稳、更准

###### Case：

| `prompt` | `img1` | `img2` | `img3` | `img4` | `vid1` | 生成结果 |

| --- | --- | --- | --- | --- | --- | --- |

| 男人`@图片1`下班后疲惫的走在走廊，脚步变缓，最后停在家门口，脸部特写镜头，男人深呼吸，调整情绪，收起了负面情绪，变得轻松，然后特写翻找出钥匙，插入门锁，进入家里后，他的小女儿和一只宠物狗，欢快的跑过来迎接拥抱，室内非常的温馨，全程自然对话 | ![](img/293d45c2a1722b8b23093cff8a236f3e.png) | | | | | |

| 将`@视频1`中的女生换成戏曲花旦，场景在一个精美的舞台上，参考`@视频1`的运镜和转场效果，利用镜头匹配人物的动作，极致的舞台美感，增强视觉冲击力 | | | | | | |

| 参考`@视频1`的所有转场和运镜，一镜到底，画面以棋局为起始，镜头左移，展示地板的黄色沙砾，镜头上移来到一个沙滩，沙滩上有足印，一个穿着白色素衣的女生在沙滩上渐行渐远，镜头切到空中的俯拍视角，海水在冲刷（不要出现人物），无缝渐变转场，冲刷的海浪变成飘动的窗帘，镜头拉远，展示女孩的面部特写，一镜到底 | | | | | | |

| 0-2秒画面：快速四格闪切，红、粉、紫、豹纹四款蝴蝶结依次定格，特写缎面光泽与 “chéri” 品牌字样。画外音“Chéri 자석 리본으로 무궁무진한 아름다움을 연출해 보세요!” | ![](img/16030de65bde4edce8e8e9f6132710f0.png) | | | | | |

| 对`@图片2`的包包进行商业化的摄像展示，包包的侧面参考`@图片1`，包包的表面材质参考`@图片3`，要求将包包的细节均有所展示，背景音恢宏大气， | ![](img/ec63eadba1eb5cb2ca497cd8f43a8343.png) | ![](img/ce2e5c31fee452dc057e7c638b3dcf3e.png) | ![](img/fed9503a1f7b22328b5df42f029eb801.png) | | | |

| 把`@图片1`作为画面的首帧图，第一人称视角，参考`@视频1`的运镜效果，上方场景参考`@图片2`，左边场景参考`@图片3`，右边场景参考`@图片4`。 | ![](img/79f95b1bdf5479eb6395081a4ece0c4b.png) | ![](img/0d52b0be7d5e79291a9d3c018c5ca931.png) | ![](img/780fdfa4911f2c52546db20220c0111d.png) | ![](img/ce143ff0f2742814b833547d30300808.png) | | |

![](img/31333bb9101cc80a47861948ef77b5ba.png)

[jimeng-2026-02-06-3748.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/H2ptby9XNoanfZxauWncZhNQnic)

![](img/fbc9219a6f9664c90704d18185716385.png)

[发现更多精彩视频 - 抖音搜索 (32).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/W5BNbtJERohrB1xd4GFco72nnWg)

![](img/14ba12ccbddca699e245136af3a58811.png)

[jimeng-2026-02-06-2924.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AHaPbdaVNoeDnuxkQSCcrDsznUf)

![](img/b62041741265513f588058f4ba150c3f.png)

[2月6日 (1)(10).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IfhdbqfBwoZ8NExltwCcGaFjn9d)

![](img/113cffc170de0f423622b89e5526a837.png)

[jimeng-2026-02-06-5842.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LQXIblyxpo6Ow2xzLzHc61QInCd)

![](img/2272deb2d97b75953d49098b3b6678c5.png)

[磁吸蝴蝶结.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FsQdbOmGUoredGxvS7bcwVxVnNf)

![](img/86c728b5710ff26f6b77b4c6de64b393.png)

[手提包.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/D5CpbSg83oTqVzxzN56cWvTYn3c)

![](img/49e3feb87df9da1c925de3bbbc9d1d31.png)

[实拍运镜.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PXZjbReepoc0oBxdqIYch9qgnCd)

![](img/e890a484fcf397eb13121a9c325b712d.png)

[恐怖片运镜.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HLFhbyeuOoF06TxIUwYci85An3c)

### 高难度/可控的运镜和动作精准复刻

以前想让模型模仿电影里的走位、运镜或者复杂动作，要么写一堆细节提示词，要么干脆做不到。而现在，只需要上传一段参考视频，就可以了

###### Case：

| `prompt` | `img1` | `img2` | `img3` | `img4` | `img5` | `img6` | `img7` | `vid1` | `vid2` | `vid3` | 生成结果 |

| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

| 参考`@图1`的男人形象，他在`@图2`的走廊中，完全参考`@视频1`的所有运镜效果，还有主角的面部表情，镜头跟随主角在`@图2`拐角奔跑，然后在`@图3`的长廊里，镜头从背面的跟随视角，通过低视角环绕到主角正面；镜头再右摇90度拍摄`@图片4`的分叉路口，急停后右摇180度，怼脸拍摄主角正面：主角气喘吁吁，镜头跟随主角的视角环顾四周，参考`@视频1`里急速的左右环绕运镜展示场景，后拉到`@图片5`的场景，继续跟拍主角奔跑的侧面视角 | ![](img/8e985b577c370e231a510cdf10d67d8b.png) | ![](img/55bb6d8d02c4f9a8cd60ab4d566272d4.png) | ![](img/555278230e150a7e034b349ad350098f.png) | ![](img/111fd4fa1a35d0b5db85c6625231f30f.png) | ![](img/caede299a2f1ffd81cf4dfb0c0b47726.png) | | | | | |



| @图片1的平板电脑作为主体，运镜参考@视频1，推近到屏幕的特写，镜头旋转后平板反转展示全貌，屏幕中的数据流一直在变化，周围的环境逐渐变成科幻风格的数据空间 | ![](img/f2a54cfb7b5c4f2c629b90a6c86c421a.png) |

| @图片1的女星作为主体，参考@视频1的运镜方式进行有节奏的推拉摇移，女星的动作也参考@视频1中女子的舞蹈动作，在舞台上活力十足地表演 | ![](img/1139accef13a41a57d178f8c48a88b05.png) |

| 参考@图1@图2长枪角色，@图3@图4双刀角色，模仿@视频1的动作，在@图5的枫叶林中打斗 | ![](img/46e26afd79b99256ae45ace301569423.png) | ![](img/56b3d4ead9030c171df21f5f926702fb.png) | ![](img/3923ddc831d2faa23e89e04b858e187d.png) | ![](img/219e04d081c9304dd61430c84ff88a87.png) | ![](img/f42b17a8c8a13c0498d717360400c10d.png) |

| 参考视频1的人物动作，参考视频2的环绕运镜镜头语言，生成角色1和角色2的打斗场面，打斗发生在星夜中，打斗的过程中有白色灰尘扬起，打斗场面非常华丽，气氛十分紧张。 | ![](img/925d6b21e6e83e89d654b79b09b25e8b.png) | ![](img/b0b3057c4cdb359db676ea43a67e80cc.png) |

| 参考视频1的运镜、画面切换节奏，拿图片1的红色超跑进行复刻。 | ![](img/6d9b2fedd77a22dfa3321b4499050238.png) |

![](img/e38ca5e1513dfd00cd5b973cef6d87e6.png)

[1月30日(4).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AawobWWUyoYl9lxTDAecdGlhnxh)

![](img/c59b1a4388886b6bdbbeeda8af457bf4.png)

[jimeng-2026-02-05-8410.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RYYYbsxJwoKh2lxlB1VcVqOonxw)

![](img/4c8d7252f8f3b4c05b8aff99dc0eb80d.png)

[1月30日(1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/F9DwbgFCtosIPvxTwzQcnpBTnwb)

[jimeng-2026-02-05-3654.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Es4Rb81tTol9RlxAugpcJCnAnp6)

[聚焦旋转.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PFLObGK6So2uhPxLKWJceTX0nWb)

![](img/5c4f908399bcdef7253f5db5b6bcba68.png)

[jimeng-2026-02-05-3975.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Snlob5pgAouS9px5zcdcCv80nNd)

![](img/d437929bab87b67898b89167d5519ebe.png)

[推拉舞蹈运镜.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JmGObIiRuo4mpkxnULoc2bMknch)

![](img/874f69ec9a5b88909f9717799c81fef7.png)

[jimeng-2026-02-05-2445.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SF7Bb7Wtwo2mXGx9dmQcBRTinkc)

![](img/befb87a5f532a4019e940a6ffb3d182f.png)

[1月30日(7).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EzOgbw1NSoLzXaxYYwDcxLSFnYg)

![](img/91c3fd6f50f7c1f855f6d6f3e3362fb6.png)

[okI4k2Elxfe5eFYM86DD4BAvIkCfLJ4ApIAIEc.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DQjNb9YnYoPhUKxIiDScj8LZnzh)

[打斗.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZDiUbdVkPoVt5LxCa7JcAL30nxb)

![](img/ddbf63ac23e8b86c7f11bee65ef44186.png)

[02176967295935700000000000000000000ffffac15af9c90c167.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LnY3b9HWVor82OxNS6XcHgzQnXg)

![](img/7b0d59c0193ec2431f6f794f12d28a44.png)

[jimeng-2026-02-05-5400.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SZOLbGA3eo4E1BxiAqgc1r5inNf)

![](img/cc7390e142aac4e11f706f85fd661468.png)

[汽车运镜.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZqkgbvXHxoLDpcx20Zhc73H8nk6)

![](img/6e61d6830b1f299fcd106f628388ece4.png)

[汽车运镜.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JRQjbGYNJoNvUdxSmYYcyNgZnxm)

## 创意模版 / 复杂特效精准复刻

不止能生图写故事，Seedance 2.0 还支持“照着模仿”——创意转场、广告成片、电影片段、复杂剪辑，只要你有参考图或视频，模型就能识别动作节奏、镜头语言、视觉结构，并精准复刻出来。不懂专业术语也没关系，写清楚你想参考的部分，比如“参考 @视频1 的节奏和运镜，@图1 的角色造型”，模型就能高质量生成属于你的版本。大胆试！它真的能做到

### Case：

| `prompt` | `img1` | `img2` | `img3` | `img4` | `img5` | `img6` | `img7` | `vid1` | 生成结果 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

| 将@视频1的人物换成@图片1，@图片1为首帧，人物带上虚拟科幻眼镜，参考@视频1的运镜，及近的环绕镜头，从第三人称视角变成人物的主观视角，在AI虚拟眼镜中穿梭，来到@图片2的深邃的蓝色宇宙，出现几架飞船穿梭向远方，镜头跟随飞船穿梭到@图片3的像素世界，镜头低空飞过像素的山林世界，里面的树木生长形式出现，随后视角仰拍，急速穿梭到@图片4的浅绿色纹理的星球，镜头穿梭并掠过星球表面 | ![](img/996cd4d8d9efe730cdc67571fab1090f.png) | ![](img/9af57e249d6e4711cc744e41ee26d710.png) | ![](img/53072d0b5e1c2159ac6e5a4bf58f3be8.png) | ![](img/f9260543c8ed48d4ddec60237dd2e208.png) |

| 参考第一张图片里模特的五官长相。模特分别穿着第2-6张参考图里的服装凑近镜头，做出调皮、冷酷、可爱、惊讶、耍帅的造型，每一个造型穿着不同服装，每次更换，画面伴随会切镜，参考视频的里鱼眼镜头效果、重影闪烁的炫影画面效果，参考@视频1的效果 | ![](img/9cf0f7825c38f41fc60f4d4cad80c47b.png) | ![](img/39c2fefa526b94e69a0a2b6be53177bf.png) | ![](img/829fc4bfc679b23c020390015e40d6b6.png) | ![](img/128c287296c962e92daeb7a47a63f938.png) | ![](img/64754925f02265b5e22a644324e222ab.png) | ![](img/cced390b8318fdc5bcb777e8597a9d66.png) |

| 参考视频的广告创意，用提供的羽绒服图片，并参考鹅绒图片、天鹅图片，搭配以下广告词“这是根鹅绒，这是暖天鹅，这是能穿的极地天鹅绒羽绒服，新年穿得暖，生活过得暖”，生成新的羽绒服广告视频。 | ![](img/83da5369c780da3ce81cab8f481a53a9.png) | ![](img/43c1a6a5cd18599d45a736f510397e0b.png) | ![](img/11e31d2de4dd344487ffa5126fcad434.png) |

| 黑白水墨风格，@图片1的人物参考@视频1的特效和动作，上演一段水墨太极功夫 | ![](img/a182f94f648113776866e856beaaa423.png) |

| 将@视频1的首帧人物替换成@图片1，完全@参考视频1的特效和动作，手里的花蕊长出玫瑰花瓣，裂纹在脸部向上延伸，逐渐被杂草覆盖，人物双手拂过脸部，杂草变成粒子消散，最后变成@图片2的长相 | ![](img/db0feeab1280937eec4ec899da143621.png) | ![](img/f98463580a4109be6bae364ad41a199c.png) |

| 由@图片1的天花板开始，参考@视频1的拼图破碎效果进行转场，“BELIEVE”字体替换成“Seedance”，参考@图2的字体 | ![](img/d32219dfa5905ce9e4fd3b2b7426a637.png) | ![](img/9c4d6061fd463b478ed6bf63d2acc9ed.png) |

| 以黑幕开场，参考视频1的粒子特效和材质，金色鎏金材质的沙砾从画面左边飘出并向右覆盖，参考@视频1的粒子吹散效果，@图片1的字体逐渐出现在画面中心 | ![](img/1cdbf69bf7137ed30e3ed451a94d44b0.png) |

| @图片1的人物参考@视频1中的动作和表情变化，展示吃泡面的抽象行为 | ![](img/d04cbf71e0fe0f2a02e036c4702f0886.png) |

![](img/8883b9b523144af781b76de489dcbf58.png)

[2月4日.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QWeLbxUR7oKo98xkxGkcBZsunwc)

![](img/5fe084c0ceb14884a8a0fc367e61089d.png)

[jimeng-2026-02-05-6539.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ENBkbHlaaoo40gxFmHscAzSRnFc)

![](img/6f3217482e427650df96cf713c1a1575.png)

[2月5日(3).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BuhpbCKq6oLvFtx485dcvQa8nZd)

![](img/f43fa1147f779011406b24c5d40a9ed3.png)

[jimeng-2026-02-05-8964.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LtLObUe5qocBeZxKzMncjiNanMe)

![](img/54179df926d76e5dc9401480684bde59.png)

[v15-1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QPcLb7o7IoW4ejx6NVdc7xJYnxg)

![](img/f4f5235a721bb4b544e3a774fe581389.png)

[32.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Hk1lbOHJQoUksXxZOlHcb3plnrh)

![](img/a46786bcaf1ddf395ae4f6d5aa84b1f9.png)

[1月30日(6).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VcAEb7YEwocI10xhvhncqeDdnEb)

![](img/ee1154504cf421b73c4ffc5b1b69b731.png)

[jimeng-2026-02-05-1007.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/N6V2boPSIopylcxAQr4c92PXn4e)

![](img/028df6b478356a2f12c45adffb36dc78.png)

[变装.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RNeib86sxookCKxCEMIc2lkvnne)

![](img/a3a11e3a2498bf805c031c2ad8498d45.png)

[jimeng-2026-02-05-8841.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/K5E2buSMgoLDU7xZHIMcFGvPnrm)

![](img/60fb0690583bf1d1f86ae5c780eb0399.png)

[1月30日(3).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/B8fDbfwAuofqUIxvHLacWLCMn4e)

![](img/495ad3990056505e87ee491a9c56a720.png)

[oAgveDLfVIGKTuAL8uABeQIyleIm2wPCIA4c5E.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OeqJbjzhUoziAIxbJj2cMAOznKd)

![](img/5f1525e2eb9c32ca2d987c4f71cf8308.png)



[ae片头.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GU4qbJlWzoOccexmWTTc8HGNnNd)

![](img/9f6d2ba87bafe0d08830f6569c0c8ebd.png)

[jimeng-2026-02-05-5566.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GC3TbvmPzoQK7MxSGAucHQ6wn3f)

![](img/94ddb26b678042b080967118b16256a3.png)

[1月21日(8).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Myx8bTh6eorKPGxunvwcJMpQnUe)

![](img/a23bd4285f4fbc75b92e726d362ae79d.png)

[jimeng-2026-02-05-4864.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GbFjbYq3Bo0IhFxp2UScrOAEnOd)

### 模型的创意性、剧情补全能力

#### Case：

| prompt | img1 | img2 | img3 | img4 | img5 | img6 | img7 | vid1 | 生成结果 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 将@图1以从左到右从上到下的顺序进行漫画演绎，保持人物说的台词与图片上的一致，分镜切换以及重点的情节演绎加入特殊音效，整体风格诙谐幽默；演绎方式参考@视频1 | ![](img/f53d0bc2b2ea1248e281dcf45038f545.png) |  |  |  |  |  |  |  |  |
| 参考@图片1的专题片的分镜头脚本，参考@图片1的分镜、景别、运镜、画面和文案，创作一段15s的关于“童年的四季”的治愈系片头 | ![](img/319cd8b76ae0da1765d1c0715124c14d.png) |  |  |  |  |  |  |  |  |
| 参考视频1的音频，根据图1、图2、图3、图4、图5为灵感，发散出一条情绪向的视频。背景音乐参考@视频1 | ![](img/e4eb39006eecb8885386c79342f481f0.png) | ![](img/60f622c002e56d48f20e02c7301aa7e0.png) | ![](img/32f07bda434e21cb4a2c6671c35faee7.png) | ![](img/2ece7164ca653d256b6ef2ad9cde0f38.png) | ![](img/d4d35c2ff4b23fb5dc3f38cd65f0e10f.png) |  |  |  |  |

![](img/a82203aa405cb1d7f41feb359264eb97.png)

[2月4日(1) (1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MAfVbQN8robZUixbfx5cEnpRnvb)

![](img/c6926919639e0a86fbb710bccba0dd68.png)

[jimeng-2026-02-05-8318-先是固定镜头，主体涂口红然后抿了抿嘴唇，镜头开始向镜子前推，主体对着镜子用口红在....mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PinVblXCXo22Dcx4x2KcNzmHnNb)

![](img/a30ef1847d9a8e338006ce5dcbaee00b.png)

[jimeng-2026-02-05-7071.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OwC9bNJvPolsrWxj7QFc4vmwnpy)

![](img/613e14a1d458c3fbfbcd0b200e5bfca2.png)

[jimeng-2026-02-05-4263.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Qep3b3OznokWnPxXLZwcTt0dncb)

### 视频延长

#### Case：

| 生成时长 | prompt | img1 | img2 | img3 | img4 | img5 | img6 | img7 | vid1 | 生成结果 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 15s | 延长15s视频，参考@图片1、@图片2的驴骑摩托车的形象，补充一段脑洞广告  画面1：侧面固定镜头，驴骑着摩托车冲出棚栏，旁边的鸡受到惊吓，  画面2：驴骑着摩托在沙地盘旋，先特写摩托轮胎，再切到半空中俯拍驴骑着摩托车做着盘旋特技，掀起烟雾  画面3：背景是雪山镜头，驴骑着车从山坡飞越过，广告语在主体背后，通过遮罩的形式（驴和摩托车飞过时）中间出现"Inspire Creativity,Enrich Life"，最后在摩托飞过，扬起一阵尘烟 | ![](img/88188a31414465ca3e8df31586bf9d68.png) | ![](img/3b2bdd5c73c0b168a6fc9dc87d52fc36.png) |  |  |  |  |  |  |  |
| 6s | 将视频延长6s，出现电吉他的激昂音乐，视频中间出现“JUST DO IT”的广告字体后逐渐淡化，镜头上移到天花板，一个健硕的男人拉着吊环，上半身穿着@图1的紧身健身服，背面印有@图2的“Fitness”logo，男人用健硕的上肢拉上吊环，随后视频中间出现“DO SOME SPORT”的广告结束字体。 | ![](img/56601b50ae4523fc750944ec0c81b08c.png) | ![](img/4016beb311ff74d2771fd8b3fb1d5dee.png) |  |  |  |  |  |  |  |
| 15s | 将@视频1延长15秒。1-5秒：光影透过百叶窗在木桌、杯身上缓缓滑过，树枝伴随着轻微呼吸般的晃动。6-10秒：一粒咖啡豆从画面上方轻轻飘落，镜头向咖啡豆推进至画面黑屏。11-15秒：英文渐显第一行“Lucky Coffee ”，第二行“Breakfast”，第三行“AM 7:00-10:00”。 |  |  |  |  |  |  |  |  |  |
| 10s | 向前延长10s，温暖的午后光线里，镜头先从街角那排被微风掀动的遮阳篷开始，慢慢下移到墙根处几株探出头的小雏菊。紧接着，画面里出现主人公的红色板鞋，他正蹲在街边花摊前，笑着把一大捧向日葵拢进怀里，花瓣蹭过他的白 T 恤。他转身踏上滑板时，花摊老板笑着喊了句 “小心花瓣飞啦！”，他冲老板挥了挥手，然后才开始滑行，几片金黄的花瓣已经先一步从花束里挣脱出来，落在了滑板的板面。 |  |  |  |  |  |  |  |  |  |

![](img/1f9d789fb756affc19cad72d6ec0d251.png)

[2月5日(10).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TTUXb3xznob8l1xWqRScXFwrnac)

![](img/51bda8f4d4b13210a4765510ace20c6e.png)

[jimeng-2026-02-05-3767.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/P8Fdbd3J5o9Y77xkAQbcjgrMne9)

![](img/14e5537e8c2740e1d184c5fa7cbcbe80.png)

[jimeng-2026-02-05-1923.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JYdhbMUiPofrqRxj0JGcEbBknuf)

![](img/6a1c820cb658a3750ea1743809c0da42.png)

[jimeng-2026-02-05-3437.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/R3Ssb3qguoxSvjxLQSyca4mMnvg)

![](img/e875c4467362756aac4aedc2976f0ca0.png)

[jimeng-2026-02-04-2947-延时摄影，咖啡杯长出树枝，上面挂着曲奇.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Ftj7bBgsno177Uxl8cjcBd5JnTd)

![](img/bcaa14bdeed6259967a351d1bb2853a8.png)

[jimeng-2026-02-05-4162.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Xk76b1Rb3oZHnUxgDPFcs8cUn0f)

![](img/dc16fb8e86da97098dff704b113f19b8.png)

[向日葵滑板车.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TA6tbdn48oDEB6xEySbc1gELnGf)

![](img/800f76b70d84c242c08ffb59a9b84ca1.png)

[jimeng-2026-02-05-5137.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XTribx2Z9oiq5tx6bThc4WObnFB)

### 音色更准，声音更真

#### Case：

| prompt | img1 | img2 | img3 | vid1 | vid2 | vid3 | 生成结果 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 固定镜头，中央鱼眼镜头透过圆形孔洞向下窥视，参考视频1的鱼眼镜头，让@视频2中的马看向鱼眼镜头，参考@视频1中的说话动作，背景BGM参考@视频3中的音效。 |  |  |  |  |  |  |  |
| 根据提供的写字楼宣传照，生成一段15秒电影级写实风格的地产纪录片，采用2.35:1宽银幕，24fps，细腻的画面风格，其中旁白的音色参考@视频1，拍摄 “写字楼的生态”，呈现楼内不同企业的运作，结合旁白阐述写字楼如何成为一个充满活力的商业生态系统. | ![](img/ed83c5f8bb10ce475b1087ff1f103a57.png) | ![](img/0170e5ae1292cebcea2d0be5ef8dce4d.png) | ![](img/5c42b34cc2349fef2628fc9e279a63ef.png) |  |  |  |  |
| 在“猫狗吐槽间”里的一段吐槽对话，要求情感丰沛，符合脱口秀表演：  喵酱（猫主持，舔毛翻眼）：“家人们谁懂啊，我身边这位，每天除了摇尾巴、拆沙发，就只会用那种“我超乖求摸摸”的眼神骗人类零食，明明拆家的时候比谁都凶，还好意思叫旺仔，我看叫“旺拆”还差不多哈哈哈“  旺仔（狗主持，歪头晃尾巴）：“你还好意思说我？你每天睡18个小时，醒了就蹭人类腿要罐头，掉毛掉得人类黑衣服上全是你的毛，人家扫完地，你转身又在沙发上滚一圈，还好意思装高冷贵族？” | ![](img/84d2615b85f5cd84e68f96c414928589.png) |  |  |  |  |  |  |
| 豫剧经前桥段《铡美案》的伴奏响起，左侧的黑衣包拯指着右侧的红衣陈世美，咬牙切齿地唱着豫剧：“刀对鞘，真凭实据你敢不招？” 陈世美的眼珠左右滴溜溜乱转，寻找着权宜之策，面色窘迫至极。此时，画面外传来一声豫剧旦角的念白：“且慢！”包拯和陈世美一齐向画面右侧看去。 | ![](img/27e134668314b9abdb106e0e030315a6.png) |  |  |  |  |  |  |
| 生成一个15秒的MV视频。关键词：稳重构图 / 轻推拉 / 低角度英雄感 / 纪实但高级A超广角建立镜头，低机位轻微仰拍，悬崖土路与复古旅行车占画面下三分之一，远处海面与地平线拉开空间，夕阳侧逆光体积光穿过尘粒，电影级构图，真实胶片颗粒，微风吹动衣角。 | ![](img/95d45f064a235725b8f935e666c50af2.png) |  |  |  |  |  |  |
| 画面中间戴帽子的女孩温柔地唱着说"I'm so proud of my family!"，之后转身拥抱中间的黑人女孩。黑人女孩感动地回应"My sweetie, you're the heart of our family"，回抱她。左侧的黄衣服男孩开心地说"Folks, let's dance together to celebrate！” 最右侧的女孩紧接着回复： “I'll bring the music!"，背景拉美音乐响起，左侧穿橙色裙的女性（朱丽叶塔）笑着点头，右侧扎辫女性（路易莎）握紧拳头挥动手臂。人群中有人开始踏起步子，孩子们跟着节奏拍手，整个家族即将围成圈，伴着欢快的音乐，裙摆飞扬，在五彩的街道上尽情舞动，传递着喜悦与温暖。 | ![](img/e3930460343f46de000210a4b7b4c7b4.png) |  |  |  |  |  |  |
| 固定镜头。站着的壮汉（队长）握拳挥臂用西班牙语说着：“三分钟后突袭！”，金发队员站在检查枪械，绿发队员握紧战术手电。黑人队员搭肩问同伴用西班牙语说：“侧翼包抄？”队长点头用西班牙语说：“老规矩，活口留审讯。”全员肃然，装备碰撞声中完成战术手势，默契起身，大家都严阵以待，左侧两个男生也争先站起来准备战斗。 | ![](img/03026c73943f6a5d34e16ed875b91c20.png) |  |  |  |  |  |  |
| 0-3秒：开头闹钟响起来，画面朦胧中出现画面1； | ![](img/af22b5056a2b46befba3109d65eb6638.png) | ![](img/0a3017048afb71a42dc2bfce03c51dac.png) |  |  |  |  |  |
| @图片1的猴子走向奶茶店柜台，镜头跟随在他身后，一位@图片2的比熊服务员正在吧台处擦拭制作工具，猴子向服务员用四川口音点单：“幺妹儿，霸王别姬有得没得？”  切镜，特写。  服务员放下手里的活，怪异地看了老头一眼后回答：“没得，美式要不要得嘛”  切镜，镜头给到猴子。 | ![](img/e2d9b57e0e71be55885b5fe6fb92f61c.png) | ![](img/6ac5839907c5a95fcef2e3797c27b25e.png) | ![](img/773eac6d546df8e278b392d516da64a5.png) |  |  |  |  |
| 用科普风格和音色，将图片1中的内容演绎出来，内容包括悟空为过火焰山，到翠云山向铁扇公主借芭蕉扇。铁扇公主因红孩儿被悟空降伏拜观音为童子，母子分离，不肯借扇还欲报仇。悟空好言相劝无果，二人随即起了争执的小故事进行讲解。 | ![](img/a8d2692c430b2c09c8fc2f846cc222a9.png) |  |  |  |  |  |  |

![](img/cf171732f90097f18747958f2aed7ba2.png)

[鱼眼.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Me2kbin7EodtnFxVmXjcmNHNnfe)

![](img/10fef678e454ef777195a33237f1b8ca.png)

[jimeng-2026-02-03-3673-红色的背景下，突然一个头顶红布、睁大无辜眼睛的马头从下面抬起，然后马儿摇摇耳朵，....mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OEmRbycchoWXKIxuI1mcxXDvnMc)

![](img/5c8330f4f982e7a75904f20fbe927ac9.png)



# 视频生成功能示例

[无标题视频——使用Clipchamp制作 (16).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BZmGbFvW3onNU5xy4tWcNogtnmb)

![](img/c628866ce47beeae452091993dc03c10.png)

[jimeng-2026-02-05-9949.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BQjobPf70ozwTWxHry8cXP9inRc)

![](img/b57eebf9a38ba3ca0a8c88d8750e3d5d.png)

[无标题视频——使用Clipchamp制作 (18).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/G95gbsoGZohExWxA386cPRWGnrb)

![](img/793b2ef93633f22d569504af620556d2.png)

[jimeng-2026-02-05-4039.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Lw8YbPMTGoCFeUxGubmc9eWWnte)

![](img/cc0449e5fe2f45c2d3eb5c75555cc506.png)

[jimeng-2026-02-05-9740.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/M2bHb5yztoB0UbxLRCCczdwvnNh)

![](img/7e7123d4c5519cac7e054240908b7607.png)

[jimeng-2026-02-05-7415.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IXG6bl3owoJdyhx7mDvcyAsLnrb)

![](img/d5ca7d48827ff554e3dc4e3b15b130d4.png)

[jimeng-2026-02-05-1458.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SBjSbXFdsodPe1xLreccjVb3nqb)

![](img/94f938bd8e6dc825e31e8d887c972479.png)

[jimeng-2026-02-05-1958.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EndubqBFRodv1TxYujScVSpBnOh)

![](img/eb64312d8c2ca8534bb7d25ca5caf2e4.png)

[jimeng-2026-02-05-1805.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OACoblyP3oz50Rxb8t3cw6pAn3f)

![](img/8b997754fbe6f9eb7228f53648cc410a.png)

[无标题视频——使用Clipchamp制作 (19).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GjGkb7CHzo61C6xK3WRc7RKZn63)

![](img/b1233d1a0a395ef0bf5ab89b9f7d35dc.png)

[jimeng-2026-02-05-2906.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DdCIbxI9do2v1Ex40ONcKoujnFc)

![](img/25a969e0f42afceda5d1e5d6e9f45148.png)

[jimeng-2026-02-05-8320.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RF8obZDj1omPxpx7RYmcqXQWnLt)

![](img/48328e2c56c0d469b4b921cdb6599152.png)

[jimeng-2026-02-05-2803.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/H0HUbUA4eo9NuKx5nf6c3hLVnwc)

## 镜头连贯性（一镜到底）更强

### Case：

| `prompt` | `img1` | `img2` | `img3` | `img4` | `img5` | `img6` | `img7` | 生成结果 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `@图片1` `@图片2` `@图片3` `@图片4` `@图片5`，一镜到底的追踪镜头，从街头跟随跑步者上楼梯、穿过走廊、进入屋顶，最终俯瞰城市。 | ![](img/7633c3daaa39af152d2d6cf153397458.png) | ![](img/f287cbb93e98d6c9e0265900dd137b1f.png) | ![](img/da44153b5af62059ba1743eff6f696f7.png) | ![](img/4b3c08990b5151d09d6684bf0454f0b8.png) | ![](img/71034e1879ced11aea0cef8862c11689.png) |  |  |  |
| 以`@图片1`为首帧，画面放大至飞机舷窗外，一团团云朵缓缓飘至画面中，其中一朵为彩色糖豆点缀的云朵，始终在画面中居中，然后缓缓变形为`@图片2`的冰淇淋，镜头推远回到机舱内，坐在窗边的`@图片3`伸手从窗外拿进冰淇淋，吃了一口，嘴巴上沾满奶油，脸上洋溢出甜蜜的笑容 | ![](img/3bf7616e2e23bd48687d3d6879465d59.png) | ![](img/5ce66f994559fd28f1297065f0f0c26c.png) | ![](img/415cecb05b5cede9c511dcaea38e52fb.png) |  |  |  |  |  |
| 谍战片风格，`@图片1`作为首帧画面，镜头正面跟拍穿着红风衣的女特工向前走，镜头全景跟随，不断有路人遮挡红衣女子，走到一个拐角处，参考`@图片2`的拐角建筑，固定镜头红衣女子离开画面，走在拐角处消失，一个戴面具的女孩在拐角处躲着恶狠狠的盯着她，面具女孩形象参考`@图片3`，只参考形象，女孩站在拐角处。镜头往前摇向红衣女特工，她走进一座豪宅消失不见了，豪宅参考`@图片4`。全程不要切镜头，一镜到底。 | ![](img/061960d59eebfb0ef669a559b754fe5e.png) | ![](img/70f03392e8feb7ccc047e3e5d3db294d.png) | ![](img/94e07346d27eaa561f41047439cbaaab.png) | ![](img/89ffd2ded6ff9f7cac74113005e637e7.png) |  |  |  |  |
| 根据`@图片1`外景的镜头，第一人称主观视角快推镜头到木屋内的环境场景近景，一只小鹿`@图片2`和一只羊`@图片3`在围炉旁喝茶聊天，镜头推进特写茶杯的样式参考`@图片4`. | ![](img/7b0edfeacf96b0a20d0b0ae5c32ed3c4.png) | ![](img/750a9a694626542c3a1d46ce6307fb07.png) | ![](img/e33190d3b928debfed853f73833b8a31.png) | ![](img/a8aebecd3b341d03ccaed39c52dd48d2.png) |  |  |  |  |
| `@图片1` `@图片2` `@图片3` `@图片4` `@图片5`，主观视角一镜到底的惊险过山车的镜头，过山车的速度越来越快。 | ![](img/dee225cf172de0616e2cc0b17decdf29.png) | ![](img/a70b0d9ce7672b654dd214d69ca539f9.png) | ![](img/933b74ea36ebddefe461450373666abd.png) | ![](img/47f5030183f3c32463948f24048bb479.png) | ![](img/848959c4b30ffae94becf3a83daf6825.png) |  |  |  |

![](img/b40ee87dcaf5af6abee219532877c96b.png)

[oECdEZG7A3kCeBDBBAAF2fh3EfIHSZECqAI18l.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WvodbSTB7oRL0DxvlbycpJrsnpg)

![](img/fe46ae7b220843598629b644c8574df2.png)

[jimeng-2026-02-05-4364.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Gc43b78YWon1kFxfVUjckTtCnag)

![](img/7285815b2a34c362bc3e07dfcd4b31a7.png)

[jimeng-2026-02-05-7710.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/T1cebZEGOoHIqTxUCoDcYdbTnSb)

![](img/c65ef7ad92ea4c85cf91ed9f4801c849.png)

[jimeng-2026-02-05-3336.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OjYeb7VKdo9O0xxdE1bcbvbOnjq)

![](img/17e21001d57a65414ee86b7620ad49bb.png)

[jimeng-2026-02-05-6048.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Okaqb1wPBoc5UXxph15cDFn9neb)

## 视频编辑可用度高

有时候你已经有了一段视频，不想从头再找图或重做一遍，只是希望调整其中一小段动作、延长几秒钟，或让角色表现更贴近你的想法。现在你可以直接用已有视频作为输入，在不改变其它内容的前提下，指定片段、动作或节奏进行定向修改。无需重头生成，也能快速完成调整

### Case：

| `prompt` | `img1` | `img2` | `img3` | `img4` | `vid1` | 生成结果 |
| --- | --- | --- | --- | --- | --- | --- |
| 颠覆`@视频1`里的剧情，男人眼神从温柔瞬间转为冰冷狠厉，在女主毫无防备的瞬间，猛地将女主从桥上往外推，把女主推进水里。动作干脆利落，带着蓄谋已久的决绝，没有丝毫犹豫，彻底颠覆原有的深情人物设定。女主坠入水中的瞬间，没有尖叫，只有难以置信的眼神，她抬头冲男主嘶吼：“你从一开始就在骗我！”男主站在桥上上，脸上露出阴冷的笑容，对着水面低声说：“这是你欠我家族的。” |  |  |  |  |  |  |
| 颠覆`@视频1`的整个剧情 |  |  |  |  |  |  |
| `视频1`中的女主唱换成`图片1`的男主唱，动作完全模仿原视频，不要出现切镜，乐队演唱音乐。 | ![](img/edba5519231d49be1c9a4a1715a230a5.png) |  |  |  |  |  |
| 将`视频1`女人发型变成红色长发，`图片1`中的大白鲨缓缓浮出半个脑袋，在她身后。 | ![](img/8b0f303ce770ce996bc5ca614b81db13.png) |  |  |  |  |  |
| `视频1`镜头右摇，炸鸡老板忙碌地将炸鸡递给排队的客户，用普通话说“做完他的，做你的，大家文明排队。”一说完，就去拿纸袋子去装炸鸡。特写展示老板拿印有`图1`的纸袋子，特写展示递给客户的手部特写。 | ![](img/c4d14f7ae38ba4f109e805bb3d43ebde.png) |  |  |  |  |  |

![](img/71568086fceb9d3cdd58a1d089b0904f.png)

[jimeng-2026-02-03-4484-中景拍摄，古装服饰的一对男女主角，站在桥上赏月.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VdwSbInVkoliO7xub0acDiEcndc)

![](img/9869f2fab3e7dff56f589a9b78acdb2e.png)

[推下水.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EXjob5FBhoM7GIxM6r2cmVmXnad)

![](img/b41eb1f4d471b52d772fcf19095412b4.png)

[男女主(1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/F11BbRWYWoWsbMxTuyrcQhNuntc)

![](img/967f540e386176498df318b1e32818eb.png)

[零食.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RGZ3bXAqtoV7rFxO2CYc4lS1n1b)

![](img/b19c6025347b9d49acfa701d3fc8aa08.png)

[乐队(1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Bt4rbtBpdoZUtJxRfBxc0wwHnFh)

![](img/fd7942bf9c1f5a45f763e93611e7db87.png)

[乐队2.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/U8OGbXtXpokIhPxEpSIc0cPdn2d)

![](img/d72c63c78c27a06bc69e91a75ac3eb8b.png)

[水面(1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HF1EbBlt8o0aXCx5VYXcg2C9nig)

![](img/afd093a93b571eb11e611ab9fe3be7fa.png)

[水面.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HvhEbIj9GoyFYfxFgPQcGtZ4nTv)

![](img/ffbe9bbdbde6d0c2058f51eb55a0a473.png)

[炸鸡(1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HKC3bL7fmoKqvbxzRiRcQdjlnec)

![](img/8e47710badcd70045ba9b9dbbee7c2f1.png)

[印度炸鸡.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WoVfbiP2zoLXNZxZFxwc9DrQnid)

## 可进行音乐卡点

### Case：

| `prompt` | `img1` | `img2` | `img3` | `img4` | `img5` | `img6` | `img7` | `vid1` | 生成结果 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |



| 海报中的女生在不停的换装，服装参考`@图片1``@图片2`的样式，手中提着`@图片3`的包，视频节奏参考`@视频` | ![](img/43cc00dfc54b47444d429da8a1d8a9a6.png) | ![](img/dbde7f03495775e8f00cac9644fb4a47.png) | ![](img/7f6a755f26c5252b90163a2045c3c26d.png) | ![](img/3f2d38d380dfea61da5637659df673a2.png) |  |  |  |  |  |

| `@图片1``@图片2``@图片3``@图片4``@图片5``@图片6``@图片7`中的图片根据`@视频`中的画面关键帧的位置和整体节奏进行卡点，画面中的人物更有动感，整体画面风格更梦幻，画面张力强，可根据音乐及画面需求自行改变参考图的景别，及补充画面的光影变化 | ![](img/a64333c76c932876b1a7f8a00bc3a129.png) | ![](img/a7a4d148d84cc76daf11717d7d624dfd.png) | ![](img/6de30b25fc8b9bbb2c3d9251d1ce029a.png) | ![](img/c0c4864eb4ff1443d74afe119eae21c9.png) | ![](img/c1314ee2837dc0758c24190fa820128d.png) | ![](img/67ab2833798596cf1b0d85abe6000644.png) |  |  |  |

| `@图片1``@图片2``@图片3``@图片4``@图片5``@图片6`的风光场景图，参考`@视频`中的画面节奏，转场间画面风格及音乐节奏进行卡点 | ![](img/834468a351ca64f037bb428bdb993df6.png) | ![](img/d05e09bc6e2d9f40f518f9b8d695da09.png) | ![](img/f4636757672fe20b9218f09ffc255478.png) | ![](img/807db222344d99fc927e7adc8d71530a.png) | ![](img/6c5accf66b0a89e8a0d2965f13a43f2e.png) | ![](img/51d792d520e344219659a379c208cb2b.png) |  |  |  |

| 8秒智性博弈式战斗动漫片段，贴合复仇主题。0-3秒：分镜图1中女主转身坐下，转镜头，女主下了一步棋子，并说“你输了”，参考分镜图2。3-4秒：快速摇镜头，转向对面男人面部特写，参考分镜图3，男人咬牙切齿，对结果很不满。4-6秒：切镜头，俯拍，女人下了一步棋，对面的人们惊叹，参考分镜图4。6-8秒：镜头迅速向下摇，画面黑屏转场，后画面渐亮，昏暗室内，女人看着窗外月色静静地说“我们走着瞧”，参考分镜图5。 |  |  |  |  |  |  |  |  |  |

![](img/1bbd2354957c624401de7e6cc1df5a58.png)

[音乐卡点.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/G5E1btjKSog0SbxPgbkcZq1xn5d)

![](img/ad0090cc3fbf0319ba1c2a666f681a93.png)

[jimeng-2026-02-05-5881.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Hrl4blX13oZXHoxKDzkcc6MInGh)

![](img/23cce75ca21e5f668c7d6d8b00cd196d.png)

[卡点.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HmN5bhjS1o0YPGx0mlncpA7unPc)

![](img/146015f8a0c971889e286ca3c56cf06a.png)

[jimeng-2026-02-05-4918.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Qz15b2h2AoBSZGxfBKmcLOuXnxg)

![](img/9994af5c28e60b74925923be28cf09c4.png)

[2月5日.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/N8SJbFrK9o3icXxQWvucPxiOnMc)

![](img/10b4d311b24c27973dcbff51f542f40d.png)

[jimeng-2026-02-05-9009.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Q89QbP9mAor7AGxRB8RcOuSYnGg)

![](img/474b9c870c8832a6b212055f2aafdebf.png)

[jimeng-2026-02-05-3301-3D 动画风格，中景平视镜头，保持公园长椅场景，1-2s两人起身争执，男性怒吼前....mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MMlWbK1bPofJEnxqtODc3iybnJb)

## 情绪演绎更好

### Case：

| `prompt` | `img1` | `img2` | `img3` | `img4` | `img5` | `img6` | `img7` | `vid1` | `生成结果` |

| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

| `@图片1`的女子走到镜子前，看着镜子里面的自己，姿势参考`@图片2`，沉思了一会突然开始崩溃大叫，抓镜子的动作崩溃大叫的情绪和表情完全参考`@视频1`。 | ![](img/5602f52922275e0a6c29a7f9612d7779.png) | ![](img/729677bc17b143dab2162028acfdd379.png) |  |  |  |  |  |  |  |

| 这是一个油烟机广告，`@图片1`作为首帧画面，女人在优雅的做饭，没有烟雾，镜头快速向右边摇动，拍摄`@图片2`男人满头大汗面红耳赤在做饭，浓烟滚滚，镜头向左边摇动推进拍摄`@图片1`桌面上的一个油烟机，油烟机参考`@图片3`，油烟机在疯狂抽烟。 | ![](img/f30e25b8e4895d1332b8a162f699a2a5.png) | ![](img/cb1bc08df45774b9287c20cd07009af2.png) | ![](img/f4c18f56137666b9dd9c7e7bde683613.png) |  |  |  |  |  |  |

| `@图片1`作为画面的首帧图，镜头旋转推近，人物突然抬头，人物面部长相参考`@图片2`，开始大声咆哮，激动带有一些喜剧色彩，参考`@图片3`的表情神态。然后人物身体变身成为一只熊，参考`@图片4`.。 | ![](img/1235eb782907beb58a67482e550e7e59.png) | ![](img/995f1975a2c128f751927a9054d00ad3.png) | ![](img/196ea692ba306a134de45a133e0d7499.png) | ![](img/d03ee52e8826ac3ec3acf15bdbecac2c.png) |  |  |  |  |  |

![](img/2fbb042836cd73f763c8280cf1656b23.png)

[情绪3.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JO46bXCDyobhH2xmoF6cZTugncg)

![](img/2e6653c8ceb66371972d1bba93891f40.png)

[jimeng-2026-02-05-2315.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/K3xRbFDvioqLyXxrgnvcHTrynFd)

![](img/aa571e32816f6cc6f24973444cfaa7a8.png)

[jimeng-2026-02-05-6028.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GtnlbUym7oHUy1xhFw4c5wvrnAg)

![](img/fc192a9138a58e284f79294e67cf1a57.png)

[jimeng-2026-02-05-2154.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UaTvb2DznoV3SYxRxixcSE0onnb)

# 🏁 最后说两句

Seedance 2.0 的多模态能力正处于不断进化中，我们会持续更新能力、支持更多种输入组合方式。希望这份使用手册能帮你更自由地发挥创意！

如果你遇到了 Bug，或者有用法建议、需求场景，欢迎留言、私信、敲锣打鼓告诉我们！我们会持续优化，一起把即梦变成真正让你们开心、方便的生产力工具 ❤️
