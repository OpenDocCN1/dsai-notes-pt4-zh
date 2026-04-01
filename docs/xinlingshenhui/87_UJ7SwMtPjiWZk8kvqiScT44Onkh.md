# 3步快速线稿上色

> 来源：[https://my.feishu.cn/docx/JwmqdE95wohvraxKoUdcsUnungJ](https://my.feishu.cn/docx/JwmqdE95wohvraxKoUdcsUnungJ)

不知道以前大家从线稿到效果图需要多久的时间，现在有了Stable Diffusion只需要几秒钟的时间就能出效果图。

哈喽，大家好，这里是心领神绘AI课堂。今天给大家分享一个超级实用的技巧如何使用Stable Diffusion快速进行线稿上色？

我们先让Midjoruney给我们画一张线稿图吧。

提示词给大家放这里：Three view production: front view, side view, back view, clean background, white background, line draft, cute cartoon IP character, black and white line draft, round head, big eyes, short nose, smile, small ears, soft curly hair, small hat, loose hoodie, shorts, round shoes, hands naturally hanging down, body proportion short, cute, simple and smooth lines, overall cute shape, front eyes wide open, side display facial contours, back display back clothing details, character posture natural and vivid. --ar 4:3 --niji 6

![](img/1862d205d8de1ab8f508f2a3a83f439d.png)

在这里我们选择第四张小男孩的正面视图进行演示。

![](img/ab9dd9b3af2ced8c7d72f24aaf2df845.png)

第一步：打开我们的云端SD

![](img/97c7d38ce36ec659583372d6a4dee9b2.png)

第二步：选择大模型

首先，我们先选择模型，模型的选择要注意选择一个与目标效果一致的大模型。

因为我们是只需要上色，所以这里模型可以选择

如果你要转3维的话可以选择

第二步：设置ControlNet

把线稿上传到控制单元，勾选启用，预处理器选择线稿（Lineart）,最后点击一下爆炸按钮，得到右边AI处理的线稿图。

![](img/c544179ee36462f217b30e4e4123bc58.png)

第三步：书写提示词

提示词也很关键，如果不会写提示词可以用SD的图生文反推一下提示词。

通常可以按照：主体+颜色+风格+正面质量词+反向质量词去写。

以上面的线稿图为例：

> 正向关键词：Cartoon boy ， light brown hair, healthy light skin tone, dark brown eyes, light blue cap, light yellow hoodie, light blue shorts, and white shoes, soft lighting, simple solid color background to highlight the character's flat cartoon style.

> 反向关键词：repetition,ugly,huge eyes,text,logo,monochrome,Worst face,crossed finLow quality,worst quality,watermark,sketch,gers,ugly fingers,redundant fingers,deformed fingers,bad artist,animation,multiple arms,disfigured faces,deformed hands and faces,(Long body:1.3),(mutation, poorly drawn),box,out of frame,60s,70s,80s,90s,00s,ugly,sick,mutant,gummed animal girl,gummed animal girl,anatomy,deformed limbs,deformed fingers,too many fingers,extra feet,short,3Low quality,worst quality,waterdark,sketch,repetition,ugly,huge eyes,text,logo,monochrome,Worst face,crossed fingers,ugly fingers,redundant fingers,deformed fingers,

反向关键词是可以通用的，大家可以保存下来。

第四步：出图

出图前记得检查尺寸是否一致

![](img/169d74fa99d646ae4208412da26148c9.png)