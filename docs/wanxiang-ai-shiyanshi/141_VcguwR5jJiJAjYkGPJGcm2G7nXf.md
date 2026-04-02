# 主体参考功能使用教程

> 来源：[https://yunyinghui.feishu.cn/docx/FmeAdoLf3oVV8Lxsm1rct5yQn4f](https://yunyinghui.feishu.cn/docx/FmeAdoLf3oVV8Lxsm1rct5yQn4f)

海螺AI的**主体参考**功能可以通过识别用户上传照片中的面部信息，生成拥有对应面部信息的主体形象，并根据文字Prompt生成该主体形象的视频画面。

使**用海螺AI主**体参考**功能进行视频生成需要**用户提供【主体面部照片】和【视频生成Prompt】

进入创作页面，选择主体参考模型。依次添加参考角色，输入prompt，点击生成，等待片刻就完成了一次创作。

![](img/853294d07b9cdc1726ff90515e4d1b4b.png)

## 第一步：选取参考角色

主体参考功能需要上传可识别的面部信息，作为视频主体生成的面部参考。当前版本暂时只支持人物面部识别；

海螺AI 能够准确识别照片中不同性别、年龄、人种的面部特征。

图片符合相应的**规格、主体角度和画面光照信息**，可以让主体参考功能达到最佳的视频生成效果。

### **基础规格**

> **面部数量：**主体一致功能支持上传**单人照片**进行面部识别

> **图片尺寸：**分辨率不低于**120*120**，图片不大于20M

> **其他：**面部不可被遮挡，照片不可虚焦或有过多滤镜特效

**标准面部照片：**

![](img/4a605b491fc71cf41b271b51f8e9a95e.png)

![](img/437dce0e6b660f8e9bfa605c11670dd9.png)

![](img/bf6602a917c759b8dcfa1b1b72aad7c3.png)

![](img/b67c9ba890d109d601a916ddf89308aa.png)

![](img/b1103f0c21400e4acc01c21852c9572f.png)

![](img/38e193250cb0833db62cc805b7305779.png)

**不适合用于参考的照片示例：**

![](img/3300e2eceda107f066fb6c8c601737e9.png)

![](img/2ccf04a15ea7a797170186056a68dd0e.png)

![](img/5adf31acf3c88a5811236024fcb08460.png)

![](img/ada8032faaf27fecb49054dbc4260b15.png)

### **主体角度**

> 上传的照片中主体角度需**正对镜头**或**轻微侧对镜头**

> **正对镜头：**照片中的主体正对镜头，五官清晰出现在画面内，能够提供最完整的面部信息。

> **轻微侧对镜头：****需保证人的面部五官和脸部轮廓**完整出现在照片内，避免正侧角度或五官不在画面内的面部照片。

**不适合用于参考的主体角度示例：**

![](img/86de3d1d59e511dc898ec72f5d8d776c.png)

![](img/a046a111ec1d668f0d14fca27fc6de3f.png)

![](img/7a8f88ebc0b20d28d6ec5c61a6ea6991.png)

![](img/fe34461a63c6e192963b667bac59d478.png)

### 面部光照

> **为了获得最贴近照片中**面**部的视频**，上传的照**片中主体**面部光照需要规避大面积的黑色阴影或局部过曝，同时保证光色自然，面部可以有一定的阴影细节。

不适合用于参考的主体面部光照示例：

![](img/3dd23b1e55bfeed8aaf76b4f1af1599e.png)

![](img/535ea5a0f0bfa8e7888f0eaebcb4ea27.png)

![](img/6fd3221be15734411dca7141c3513c6e.png)

![](img/d6b1f2bbb2220ef504ed526d17790fff.png)

## 第二步：添加视频提示词（Prompt）

主体参考功能的生成逻辑和文生视频相似，详见文生视频Prompt教程。

> **用于主体参考功**能的Prompt，只涉及单人内容，可以获得最准确的表现效果。

> 若Prompt涉及多主体，可以补充对于Pr**ompt特**定主体的描述，海螺AI能够通过年龄、性别识别对应面部主体并完成生成。

**单人内容示例**

| **面部信息** | **Prompt** | **视频** |
| --- | --- | --- |
| ![](img/08d8fa5fabf07273b90ca7b022312d1d.png) | 阴天，在石板铺就的古朴小巷里，模特穿着一件棕色灯芯绒夹克，搭配米色裤子和短靴，头戴一顶复古贝雷帽。镜头从模特身后的肩膀开始，跟随他的脚步，捕捉模特晃动的身姿。随后，镜头从模特身旁侧向前移，展现他微笑着用手拢了拢贝雷帽的自然动作。接着，镜头稍微俯拍，捕捉模特站在转角扶墙的优雅姿态，最后以上升的镜头结束，模特对着镜头微笑。视频光色自然，画面具有电影质感。 |  |
| ![](img/e98b8b64672c4db493b784373a2fb6b4.png) | 阴天，在石板铺就的古朴小巷里，模特穿着一件棕色灯芯绒夹克，搭配米色裤子和短靴，头戴一顶复古贝雷帽。镜头从模特身后的肩膀开始，跟随他的脚步，捕捉模特晃动的身姿。随后，镜头从模特身旁侧向前移，展现他微笑着用手拢了拢贝雷帽的自然动作。接着，镜头稍微俯拍，捕捉模特站在转角扶墙的优雅姿态，最后以上升的镜头结束，模特对着镜头微笑。视频光色自然，画面具有电影质感。 |  |
| ![](img/4d284642725feb7ce739d064e032ecc2.png) | 在一个光线柔和的现代居家空间，模特穿着一件柔软的米白色针织毛衣，搭配高腰牛仔裤和简约的白色运动鞋，整个人散发出舒适随意的气质。模特身后是一面透着阳光的大玻璃窗，阳光洒在木地板上，模特正悠闲地坐在沙发上翻阅一本书，身边是一杯热咖啡。镜头缓慢推近，捕捉她轻轻抬起头，看向窗外瞬间，眼神安静温柔。接着，镜头环绕到她的侧面，展示她整理头发的动作。 |  |
| ![](img/d6b52c4a63d2fff4230b3f074af43760.png) | 在一个光线柔和的现代居家空间，模特穿着一件柔软的米白色针织毛衣，搭配高腰牛仔裤和简约的白色运动鞋，整个人散发出舒适随意的气质。模特身后是一面透着阳光的大玻璃窗，阳光洒在木地板上，模特正悠闲地坐在沙发上翻阅一本书，身边是一杯热咖啡。镜头缓慢推近，捕捉她轻轻抬起头，看向窗外瞬间，眼神安静温柔。接着，镜头环绕到她的侧面，展示她整理头发的动作。 |  |
| ![](img/6fe0b114e48fc877295daa6f5e4e258d.png) | 一个牛仔骑着马，穿过一片雪景，身后绑着一支步枪。他戴着宽边帽，他粗犷而醒目的五官格外显眼。在他身后的远处，是高大的云杉林，林外是令人叹为观止的白雪皑皑的群山，地面上覆盖着洁白的积雪，牛仔缓缓在寒冷的旷野中骑马前行。他盯着前方，显得坚毅且勇敢。 |  |
| ![](img/0490694582f9190e1f96a33a80c53323.png) | 一名模特，穿着复古的围裙，脖子上系着一条丝带，脚踩高跟短靴。她站在昏暗的木质酒馆吧台后，手里端着一杯倒满威士忌的玻璃杯，背景是挂满酒瓶和油灯的木架。她卷曲的头发垂落在肩膀上，脸上带着一丝狡黠的笑容，镜头大幅度运动。 |  |

![](img/9734b08072b40109c4a0bc004b03e2a1.png)

[正面-男.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Kgfxbu2qYogRnTxsrr5cM00cnkh)

![](img/cc17bc96ad172fead7cc602bbd686cec.png)

[侧面-男.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Lc6Zb8p2ioLRPhxAbkHcKj0dnug)

![](img/d78583bba2c59115bb993ea3d07176c3.png)

[正面-女2.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MkYNbOaieolCkOxReIScTRXQnKb)

![](img/995f21a72b68c710a80a567f1aa85703.png)

[侧面-女.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EgJ0bLP4do3IvmxUd1dcUHV2n5d)

![](img/093344504d6ccfbc174ad15565a57bc2.png)

[光照-男333262437424898052.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Fn2sbaO3aonpQKxWaRocCNkAnec)

![](img/0518b5a7d135a208f4a424c469a6fab9.png)

[光照-女.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/N8vwbjCyyofZvpxmswiceW7DnRf)

### 多主体下如何准确映射

1.  在**Prompt**中补充面部照片的性别信息，可以帮助海螺AI锁定响应的主体并生成符合预期的画面。

| **面部信息** | **Prompt** | **视频** |
| --- | --- | --- |
| ![](img/daa11640b38a78fb2dc194d5bf168b92.png) | 画面中一个**黑发男性**身穿西装坐在欧式餐桌旁看向镜头，在他身旁，是一个留着棕发的女性微笑着看向镜头。 |  |
| ![](img/7a60729b7d0886340046019e1718b44d.png) | **画面中一个黑发**男性身穿西装坐在欧式餐桌旁看向镜头，在他身旁，是一个留着棕发的女性微笑着看向镜头。 |  |

![](img/19062b29c169489bd5f4e7caba3077eb.png)

[性别-男.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Ab5YbBIk4oPOjMxgKMtcUB4qnWb)

![](img/35141ef04b435cf83b7e9375096973cf.png)

[333294378157121548.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GU28brbbmouM1qxO2QYcYfFGnkf)

1.  在**Prompt**中补充面部照片的年龄信息，可以帮助海螺AI锁定响应的主体并生成符合预期的画面。

| **面部信息** | **Prompt** | **视频** |
| --- | --- | --- |
| ![](img/5a3508ee2081f6407f66b9233e8d7d5f.png) | **一个身穿衬**衫的年迈的老人抱着一个身穿红衣的幼年小女孩，两人坐在温馨的室内，相拥而笑。 |  |
| ![](img/dfd2a18eed34e0689dff4636299cd675.png) | 一个身穿衬衫的**年迈的老人**抱着一个身穿红衣的幼年小女孩，两人坐在温馨的室内，相拥而笑。 |  |

![](img/b52aa0cc982efd493fbf2ae44468df7a.png)

[年龄.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Nspybn06roEuD0xVIKYcU0iwnAf)

![](img/bf7ac4137e544b646bd9942ca3ed11e4.png)

[333299236532695047.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RAH1bQKnuoqTakxfQ2Jc4W5DnEg)