# MJ实用进阶教程

> 来源：[https://aigclena.feishu.cn/docx/Yr9ndvbhxo2AtIxtEwCcYVzinsc](https://aigclena.feishu.cn/docx/Yr9ndvbhxo2AtIxtEwCcYVzinsc)

> 版权申明:无授权，不可利用原创课程，进行商业牟利!

# 图生图

①/describe

很多时候，我们在找素材的时候，看到一张好看的图片，直接使用的话会有极高的侵权风险，这时候可以用/describe命令来辅助出相似风格的图，避免侵权风险。这个命令可以推导出图片的关键词，有上传本地图片和输入网络链接地址2种方式。

![](img/2fef65d1db6a3162d74dca9eb802fe1c.png)

比如我在追波（设计师素材网站dribbble）看到了这张的图片👇 想应用到自己的设计稿里面

![](img/87d575ca9a764db0c19cd294c79d3a69.png)

键入/describe后选择link，粘贴地址后回车

![](img/e0a1e5d23c5955ee55fa98bcd84e9dd1.png)

这时候mj会推导出4段关键词，我们点击下面的1就表示使用第一段关键词来生成，可以适当修改关键词和图片比例（步骤2）后提交

![](img/9f0aba79d12a078affb29a556d1a61b4.png)

这是生成以后的效果

![](img/8734543dbafc2d0059a08a8be580526f.png)

image all - 表示4段关键词都想全部生成图片，使用这个命令时候确保是在fast模式下

![](img/0fb0bb19df9697dda9451faa2831ed69.png)

② --sref 风格一致

画了一只熊，可是感觉风格不是自己想要的，除了可以用关键词写更改画面风格以外，还可以用到垫图的方式

![](img/cee054c793afe716c96949a641e85d33.png)

在原关键词后跟一个 --sref+图片地址，就可以得到和参考风格比较相似的图片

![](img/1bc97f0814cdb087f8b29c119878a94d.png)

这一张也用到了一样风格的参考图片

![](img/348cd298f2509a9d6f9a1802bce89c0c.png)

# 插画系列/ip角色一致性

插画系列对视觉呈现有比较高的一致性和连贯性需求，人物形象面部要保持稳定，确保其整体风格贯穿始终。一般的应用场景：人物角色固定，在保持其核心特征恒定的基础上，赋予角色在服饰搭配、面部表情、肢体语言及所处环境等方面的多维度变化。midjourney具有非常大的随机性，即使你使用相同的关键词，也会生成完全不一样的图片。为此，我们可以采取seed命令来实现这一需求，这可以帮助我们生成固定ip角色的不同场景下的组图。

①seed使用

生成的每一张图片，MJ都会随机给到一个seed值，seed值是图片的对应编码，如果想做类似效果，就需要添加 seed 值。

假如想保持下面这个人物👇👇一致性，点击图片图片右边三个点，点击信封

![](img/ab97595747cb7ea1aa87a79c3dae5edf.png)

点击完信封，回到页面最顶部，点开机器人，复制seed值

![](img/323298cbd65e9d23211326a5053f2583.png)

写一段人物其他动作/场景的关键词，加上seed值即可

![](img/c13884420f96e2a0006d40f1da0ea91e.png)

得到如下👇👇效果，手指、服饰、背景、光线等各方面还达不到商用级别，如果要商用，可以根据商用场景结合重绘、增减关键词或者结合sd等各种方法再做细化和调整

![](img/6eca56287001cf94919a9c957c79ccbc.png)

原人物ip

![](img/2552fc0a4c161f12b745c52de27deb6c.png)

![](img/116af4829de2d33348836e2a3b7149ac.png)

改变动作后

![](img/570ce7f0788c888450fd4defc2b7fe91.png)

![](img/8b2ff5928918d0e2211e1255fb69d946.png)

改变动作后

# 如何控制画面风格的一致性

如果喜欢以下这种风格的图片，也想生成一样风格的

![](img/b4ced6de002b52fbab66b6b73708d341.png)

![](img/615f859f2da730f67d2338b33a0ae9a4.png)

可以使用关键词+sref垫图方式来达到效果，sref后也可以增加参数--sw[num]，num值从0-1000，来控制风格迁移的程度，亲测后，变化没有那么明显。这是sref第一张图结合sw参数，可以对照看看以下效果👇👇

![](img/1dd6f488a75c40013e5eee47253c2c86.png)

![](img/5f435340c191030af5dd5f41a576de4e.png)

![](img/9cab9d9a19b52ef785c83572bdba2733.png)

![](img/548facd59199788f3dd9de2511a4d5a4.png)

![](img/6ed5c19e46f54d39f62d9280e9ba45d1.png)

![](img/4cd6b4389b3639eddda695f83a7310d6.png)

![](img/971b2729131c33b1b00b580a7e79aa59.png)

![](img/ccd19329e904549b1a8bc582bae139f3.png)

![](img/51f6ed00a493e0dce48a2cf784884e06.png)

![](img/d28d1c4ea568e7f4141b73c27678025f.png)

![](img/9476b4087eaa0eff774434c5bb685f8d.png)

这是1张sref垫图+1张图片垫图，结合sw参数刷出来的画面，sw值不同以下效果👇👇

![](img/0621370cac78f10031f7fb99e246843b.png)

![](img/207237c50c6d2d381da2823d29582f5f.png)

![](img/a96eb5f513ca8f038314b943e4b32576.png)

![](img/a0962946ee1821e8b3a96247d1679643.png)

![](img/254602d751aa4b183bc9b953f6c87c1d.png)

![](img/0f8882d9a9b89af3f7b9790a1017b106.png)

![](img/3f445af833078173d0d444734f8cd31d.png)

![](img/4bb83733257d0da4ba92ce8a1ee806d7.png)

![](img/7196bca09ab25ab4ab06776c938e04cd.png)

![](img/f516e95e805e08acb8df4b7387776419.png)

![](img/be56c6495956b4c69e17292fc05661c7.png)

后面我还尝试了2张sref的垫图，和cref垫图，图太多就不贴出来了，若想要熟练应用垫图，可以多上手实操。

# 分享几个比较重要的关键词

① Dbz Goku with amazing aura going Super Saiyan emitting powerful lines of energy like heat waves

发出电能或能量, 是爆发动感

② dynamic poses 摆出动态姿势必备关键词

③ masculine 肌肉感

④ glimmering light effets, len flare 画面充满光线

⑤ magical, awesome, spectacular 没测试如果没有, 有没有关係, 但按字面是有帮助的

⑥ 其他摄影和风格词, 例如 highly detailed, cinematic lighting, hyper realistic, Intricate fine details 应该大家都会

# 摄影名词

镜头拉远 - Zoom out

深度 - Depth

畸变 - Distortion

慢动作 - Slow motion

快进 - Time-lapse

快动作 - Fast motion

纪实风格 - Documentary style

悬念 - Suspense

静态 - Stillness

黑白电影 - Black and white

大胆的颜色 - Bold colors

暖色调 - Warm tones

冷色调 - Cool tones

镜头眩晕 - Lens flare

ND滤镜 - Neutral Density filter

UV滤镜 - Ultraviolet filter

偏光镜 - Polarizing filter

散景滤镜 - Diffusion filter

烟雾滤镜 - Smoke filter

逆光滤镜 - Backlight filter

水晶滤镜 - Crystal filter

鱼眼镜 - Fisheye lens

魔镜 - Funhouse mirror

双重曝光 - Double exposure

延时摄影 - Time-lapse photography

电影胶片 - Film stock

镜头炸裂 - Lens flare

手绘 - Hand-painted

动画 - Animation

去色 - Desaturation

暗角 - Vignette

毛玻璃 - Frosted glass

喷泉 - Sparkle

星芒 - Star filter

![](img/e58e4feda2f246ad1e093b826dbbad20.png)

[描述语关键词表.xls](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RvKPbK93To9SP6xOYyzcruF9nzb)

## 案例

# 垫图-色彩

![](img/701fe7f18ef745908d854117adc58328.png)

材质 待更新

![](img/e6deb5dd660569e0b2bd0ea79038dde3.png)

@AI设计师Lena

10年+ UI设计师 | 出海电商

MJ/AIGC干货教程 | AI前沿资讯

创意内容生产 | 定制服务

每天分享最新的AI工具及资讯的AIGC群，欢迎大家一起学习探讨，想进群的小伙伴儿，添加微信请备注“进群”

all设计/AI相关业务欢迎咨询