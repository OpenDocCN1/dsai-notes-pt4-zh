
# 提示词模板与AI应用指南

```
你是一位精通简体中文的专业翻译，尤其擅长将专业学术论文翻译成浅显易懂的科普文章。我希望你能帮我将以下英文论文段落翻译成中文，风格与科普杂志的中文版相似。
规则:

-翻译时要准确传达原文的事实和背景
-即使上意译也要保留原始段落格式，以及保留术语，例如 `FLAC`，`JPEG`等。保留公司缩写，例如 Microsoft,Amazon等
-同时要保留引用的论文，例如 [20]这样的引用。
-对于 Figure 和 Table，翻译的同时保留原有格式，例如:“Figure 1:”翻译为“图1:”，“Table 1:”翻译为:“表1:”。
-全角括号换成半角括号，并在左括号前面加半角空格，右括号后面加半角空格。
-输入格式为 Markdown 格式，输出格式也必须保留原始 Markdown 格式。
-以下是常见的 A!相关术语词汇对应表:
* `Transformer` -&gt; `Transformer`
* `LLM/Large Language Model` -&gt; `大语言模型`
* `Generative Al` -&gt; `生成式 Al`

策略:
分成两次翻译，并且打印每一次结果:
1.根据英文内容直译，保持原有格式，不要遗漏任何信息。
2.根据第一次直译的结果重新意译，遵守原意的前提下让内容更通俗易懂、符合中文表达习惯，但要保留原有格式不变。
```

## 数据分析

### [可实用]📕数据分析师

```
角色名称
分析师：小红书数据分析师
Profile:
Author: ChampionGPT
Version: 1.0
Language: 中文
Description: 作为一名专业的数据分析师，你专注于对小红书笔记及其评论数据进行深入分析，包括数据预处理、内容分析、情感分析、互动与关联分析、趋势分析，并提出策略建议和用户体验改进措施，最终整合成详细报告。

Skill:
数据预处理与清洗，确保数据质量。
内容分析与关键词提取，识别热门主题。
运用自然语言处理技术进行评论分析。
情感分析，评估用户反馈。
互动与关联分析，找出高互动特征。
趋势分析，识别时间变化的模式。
策略建议与用户体验改进。
报告生成，包含关键指标和图表。

Goals:
完成数据预处理，确保数据格式统一且无重复。
进行内容分析，提取关键词和热门主题。
实施情感分析，了解用户情感倾向。
分析互动频率和内容关联，确定讨论焦点。
识别数据中的时间趋势。
根据分析结果提出策略建议。
提出用户体验改进措施。
整合分析结果，生成详细报告。

Constrains:
数据处理需遵守数据隐私和安全规范。
分析结果需客观准确，避免主观臆断。
策略建议需切实可行，符合实际业务需求。
报告内容需清晰易懂，便于理解和应用。

OutputFormat:
数据清洗结果报告。
关键词提取与热门主题汇总。
情感分析结果概览。
互动频率与内容关联分析报告。
时间趋势分析报告。
策略建议与用户体验改进措施。
整合分析报告，包含关键指标和可视化图表。

Workflow:
数据预处理：首先，对抓取的小红书笔记和评论数据进行清洗，去除无效或重复内容，统一数据格式。
内容分析与关键词提取：分析笔记原文，运用自然语言处理技术提取主要关键词，识别热门主题和内容趋势。
情感分析：使用情感分析工具对评论内容进行评估，区分正面和负面反馈。
互动与关联分析：计算笔记与评论的互动频率，分析评论内容与笔记主题之间的关联，找出高互动笔记的特征和用户讨论的焦点。
趋势分析：识别笔记和评论数据随时间变化的热门趋势和模式。
策略建议与用户体验改进：根据分析结果，提出内容策略和营销策略的优化建议，以及基于用户反馈的用户体验改进措施。
报告生成：将所有分析结果整合成一份详细报告，包含关键指标和可视化图表，以便于理解和应用。

Initialization:
作为一名小红书数据分析师，我将遵循上述工作流程，对您提供的小红书笔记及其评论数据进行深入分析。接下来，我将一步一步地进行数据处理和分析，直至生成最终的详细报告。请问您是否有特定的数据预处理要求或特定的分析重点？如果有，请告诉我，这将有助于我更好地进行分析工作。
```

## 提示词设计思考

> 不知道大家有没有发现：

> 实际指令词中，并不是像提示词结构模板中，将所有信息都列入。

> 因为每个人在面对复杂的需求时，需要的信息可能并不一样。

> 那这些指令是不是最好的呢？

> **肯定也不是的！**

> 至少，不是最适合你的指令，在实际运用中，可以通过自己修改的方式，将指令修改成最适合自己的。

## 实用：AI绘画指令词

AI绘画，说容易也容易，说难也难。

容易是因为：AI绘画当你知道了具体的指令词之后，很容易就能让它按照你的需求生成内容。

难点在于你很难借助AI一次性生成垂类的需求，例如互联网海报很难生成有文字的图，电商海报你需要掌握更好的技巧才能生成衣服的模特图或者是换装……

### AI绘画技巧

利用小红笔ai 找到你想要的场景的主体元素：`redpen.bj.cn`

重点推荐

可视化编辑提示词，一个开源的提示词管理工具，可以连接用户字典 `https://moonvy.com/apps/ops/`

推荐几个提示词网站:

魔咒百科:`https://aitag.top/`

提示词finding站:`https://finding.art/`

Danbooru 标签超市:`https://tags.novelai.dev/`

魔咒图库:`https://prompt.plus/`

Novelai词条共享站:`https://www.cerfai.com/#/index`

混合型网站(`sd+mj`): Al4交流社区:`https://www.ai4hub.com/Al`

绘画群体工具站:`https://novelai.dev/`

图词:`https://www.prompttool.com/NovelAl2023-04-20`

### 绘画指南

#### 提示词

+   提示词指令输入顺序并不重要。因为AI识别指令时，给每个指令词的权重都是一样的，所以你在风格这块描述越多，图片就越风格化，主体部分描述越多，就更接近我们表达的内容。

+   怎么确定提示词?不是去记忆什么指令库，而是不断尝试。

+   给AI下指令的第三个要点，就是按照“确认任务”、“确认主体”、“确认风格”、“润色和修正”的顺序，依次去尝试不同的关键词。

+   发现要写好咒语的一个重要前提要求是，人的语言表达能力要有比较高的水平对相似含义的词的词汇量在头脑里要丰富。否则，咒语会变单调，出图效果也就有限。(这也是很多人要得到别人好咒语的原因之一吧)。AI虽然降低了对专业计算机编程语言的要求，却提高了自然语言语文水平的要求。

#### 人像绘画

+   像人像创作赛博分身这样的任务，封闭性很强，需要垫图，目的是让AI有参照基础，后期可以通过`iw`系数调整AI参考和自由发挥的比重;

+   创作赛博分身的指令公式是:`赛博分身=垫图+效果细节+风格描述词+输出效果+人物细节`，在这个基础上，还可以在后缀加入参数和特殊模式。

+   【prompt灵感】西部世界(西部风格)、第九区(外星人:虾人)、沙丘(空旷、史诗感)、星际穿越(无垠宇宙)、黑神话悟空游戏(神秘)、超人(一本正经)、蜘蛛侠(3D感?)、黑豹、钢铁侠(科技感)、疯狂的麦克斯(赛博朋克、野性)、荒野大镖客游戏(西部、帧帧可做壁纸)、新海诚(晴朗、明媚)、迪士尼动画(温馨)

+   MJ默认输出的人物主题长得太西方人了，增加prompt亚洲人的描述:`-Asian`。

#### 用AI设计物件

+   体现专属感的第一招是把抽象词作为“主体”，表达你心中的想法;

+   第二招是修改材质，并匹配和材质相关的风格;

+   第三招是加入你的“专属元素”比如说你们的共同回忆。

#### 家具绘画

+   家具设计，不管是什么，一定告诉A1大小，然后多尝试几遍;

+   我们可以规定家具的位置，引导AI设计出正确的形象;

+   打光非常重要，光源的位置、强弱，和自然光的关系，都会影响整体效果;

+   设计儿童房时，应该考虑儿童房的特殊风格比如家具偏小、色彩明亮等等

#### 漫画

+   漫画跟绘本很像，但每张画面的切换过程，要体现人物情绪的变化、体现动作和场景的迁移。所以说，漫画对连贯性的要求更高，我们要关注人物和场景的连贯性;

+   在人物连贯性方面，第一种方式是直接让AI画出各种动作和表情的设定图，第二种方式是用垫图，让人物做出不同的表情和动作;

+   在场景连贯性方面，我们要利用好“分镜”，先在头脑里把故事的发展处理成几个连贯的画面，然后找到每一段故事情节的重点，再把它们一张张描述出来。

AI绘画关键词指令：[AI绘画超全面关键词释义-Midjourney ](`https://hka70cvw3i.feishu.cn/sheets/Wag1sjT3khqHCBtotIwcDUkqnWd`)

### 常用指令

+   肖像风格

```
simple avatar 简单的化身
protrait 肖像
Aisan/girl/boy/handsome man 亚洲人/女孩/男孩/帅哥
Big eyes 大眼睛
long/short/straight hair 长/短/直发
hoody/T-shirt/jeans/suit 帽衫T恤/牛仔裤/西装
Wearing glasses 戴眼镜
```

+   3D风格化:

```
3d rendering 3d渲染
C4D 3D建模渲染工具
blender 3D建模渲染工具
OctaneRender 3D建模渲染工具
Unreal Engine 虚幻引擎，知名的游戏3D引擎
```

卡通风格化:

```
Anime/Comic 动画/漫画
Pixar/Disney style 皮克斯/迪士尼风格(可以尝试其它知名的厂牌)
Game Character游戏角色
League of Legends 英雄联盟(可以尝试其它知名的游戏)
Blind Box Toy 盲盒
Pop Mart 泡泡玛特(可以尝试其它的知名玩具品牌)
```

+   适合手办的风格

```
3D
c4d/blender/octane render 都是3D建模渲染软件
Blind box toy 盲盒玩具
Studio light 工作室打光
isometric view 等距视图
Plaster Statue 石膏雕像
Renaissance style 文艺复兴Clay 粘土
porcelain 瓷器
Stone 石头材质的
Metal 金属的
```

+   适合参考风格的作品或者作者

```
Pixar/Disney 皮克斯/迪士尼
Starcraft 星际争霸
Dune 沙丘
Red Alert Game 红色警戒
Dragon Ball 七龙珠
Final Fantasy 最终幻想
GTA V 侠盗猎车手5
Shadow of the Colossus 汪达与巨像
Kunio Okawara 大河原邦男，《高达》的作者
Hayao Miyazaki, Co-founder ofStudio Ghibli(宫崎骏，吉卜力工作室的共同创始人)
Eiichiro Oda, One Piece(织田荣一郎，《海贼王》)
NaokoTakeuchi, Sailor Moon(竹内直子，美少女战士)
Takehiko Inoue, Slam Dunk(井上《灌篮高手》) Hisashi雄彦，
Hirai, Gundam(平井恒，高达)
NorioMatsumoto, Hunter x Hunter(松本则夫《猎人》)
Hiroshi Fujimoto, Doraemon(藤本浩，哆啦A梦)
HideakiAnno,Evangelion(庵野秀明，《福音战士)
Momoko Sakura, Chibi Maruko-chan(佐仓桃子，樱桃小丸子)
```

+   插画艺术家:

```
Elsa Beskow 瑞典知名插画师
Barry Blitt 纽约客讽刺漫画家
E.H.Shepard 英国知名插画师，创造了小熊维尼,形象
Yoshitaka Amano 最终幻想的插画师
Alison Bechdel知名漫画家，画风简洁
Daniel Clowes 美国知名漫画家，美式漫画风格
Les Edwards 英国知名插画师，风格主要是恐怖和科幻
Dave Perillo 风格鲜明的插画师，色彩有特色更多插画师风格，可戳此查看。也可复制网址到浏览器查看:https://www.midlibrary.io/categories/illustrators
```

+   海报风格

```
poster/ business poster 海报/商业海报
China Propaganda Poster 中国传统海报
Soviet Propaganda Poster 苏联传统海报
Flat 平面的
Iustration 插画
Manga 日式漫画
American Comics style 美国漫画风格
Vector 矢量的
Hand drawing 手绘
32 bit/8 bit 32比特/8比特画风
Pixel art 像素艺术
Digital art 数字艺术
Pop art 波普艺术
Gradient 渐变的
```

+   艺术风格

+   中式建筑主体:

```
Manor 庄园
Stone Bridge 石桥
Pavilion 凉亭
Chinese Courtyard 中式庭院
Balcony 阳台
Colonnade 柱廊
Garden 花园
Greenery 绿植
Staircase 楼梯
Arched Doorway 拱门
Corridor 走廊
```

+   景别:

```
Close up view 近景/特写
Wide angle view 广角镜头
Top view 顶视图
Front view 前视图
Side view 侧视图
Rear view 后视图
Long shot 远景
Wide shot 宽幅
Look up view 仰视图
```

+   拍摄风格

```
Axial Symmetry 轴对称
mirror symmetry 镜像对称
1940s/1970s/1990s..1940/1970/1990 年代
Vintage 复古
90&#x27;s video game 90年代游戏风
Sci-Fi 科幻的
Hollywood 好莱坞的
Modern 现代的
Industrial 工业的
Country 乡村风
```

+   摄影景别和风格:

```
Close up shot 特写/近景
Medium shot 中景
Full shot 全景
Long shot 远景
Wide shot 广角
Bird&#x27;s eye view 鸟瞰
Dutch Angle 荷兰角度
Point of view shot 第一人称摄影
Macro Shot 微距摄影影
Aerial photograph 航拍
Food photograph 食物摄影
Underwater photograph 水下摄影
Go Pro photograph 运动相机摄影
F2.1/F2.8 焦距
Polaroid 宝丽来
Fujifilm 富士相机拍摄
```

+   特殊风格

```
Steampunk 蒸汽朋克
cyberpunk 赛博朋克
Knolling 整齐排列
The Mad Max style 疯狂麦克斯风格
Marvel Movies 漫威电影风格(可尝试各种电影)
Yoshitomo Nara 奈良美智(可尝试各种艺术家)
Dribbble/Behance 知名设计网站(有时可提升视觉效果)可尝试的抽象词
Realistic 写实的
Fantastic 幻想的
Industrial 工业的
Mechanical/Mechanism 机械的
Impressive 令人印象深刻的
Chaotic 混乱的
Street art 街头艺术
```

+   首饰主体元素:

```
珍珠 pearls
水钻 rhinestones
水晶 crystals
珠片 sequins
珠线 beading thread
玻璃珠 glass beads
紫水晶 amethysts红宝石 rubies
蓝宝石 sapphires祖母绿 emeralds
钻石 diamonds
玛瑙 Onyx Stone
```

+   装修绘画

+   基础风格:

```
Furniture Design 家具设计
Interior Design 室内设计
```

+   房间主体:

```
living room 客厅
Dining room 餐厅
Kitchen 厨房
Bathroom 浴室
Bedroom 卧室
Playroom 儿童游戏室
Home office 家中办公室
Dressing room 更衣间
Baseroom 地下室
cony 阳台
```

+   家具主体元素:

```
End table 茶几
Rocking chair 摇椅
TV set 电视柜
Fireplace 壁炉
Cupboard 碗柜
Closet 衣橱
Bookcase 书柜
```

+   材质风格:

```
Antique 古董
Marble 大理石
Wood 木制
Bamboo 竹子的
Leather 皮革的
Plastic 塑料的
```

+   光线说明:

```
Soft light/illumination 软光
Hard light/illumination 硬光
Cold light/illumination 冷光
Warm light/illumination 暖光
Top light/illumination 顶光
Raking light/illumination 侧光
Edge light/illumination 边缘光
Back light/illumination 逆光
Sun light 日光
Studio light 工作室打光
Dramatic light 舞台打光
Neon light 霓虹打光
```

+   特殊风格:
