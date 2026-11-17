# 别再手动复制粘贴了！B站好物提效篇 ——选品、内容、工具三维突破的实操

> 来源：[https://xuqiwei1986.feishu.cn/docx/F25IdQ9TIoYJgJx23Otc7qXqn9z](https://xuqiwei1986.feishu.cn/docx/F25IdQ9TIoYJgJx23Otc7qXqn9z)

# 一、引言

一、引言

00后，毕业2年，一个普通的打工族，大学自学了Python，学习半年开始兼职接数据采集的单子。毕业后靠着Python的底子，花了十几天打通影刀RPA。依靠着编程和RPA的基础，在我进入深海圈开始尝试自己剪辑的时候，发现了一系列的难题。

例如：

1.  对标B站视频下面有现成的链接，一个个复制难？（视频下的链接需逐个复制，操作繁琐）

1.  获取到链接了，但是一个个去搜索查询佣金烦？（逐个搜索查询佣金，耗时费力）

1.  获得最终的链接后，获取产品参数还得手动复制？（流程冗余）

1.  终于复制完参数了，PPT图片的产品参数我还得一个个复制，有没有办法提效？（重复手动录入）

1.  产品参数复制完了，PPT图片的商品主图还得一个个找？（主图仍需逐个查找，效率低下）

1.  千辛万苦做完产品PPT了，脚本怎么办？（脚本创作成新难题）

1.  脚本写完了，剪辑怎么办？（视频剪辑环节再次面临效率瓶颈）


![](img/67ed570cee13b10c6208daab0a60081a.png)


![](img/c8e258ae593245374f06f479782f5cff.png)


![](img/6861f60ef26d0f595dec7fffe1c4026f.png)


![](img/d3ec82960eeea8fdfa8faa8d624e76bc.png)


![](img/984cf2e714ec3b9956b8a90ba6c91155.png)

![](img/fa8e4b2bef111b05dae4219a56c6136c.png)


![](img/18c60f0674a2002aed88fdbdfba2b894.png)

![](img/03fea6ab72f18740698091d19983509c.png)


![](img/cac421a5a90dfc4e39f8a55e1293a2be.png)


![](img/ab74b0d4abb23bd0f2898885e17ad5cf.png)

~~~

接下来我会给大家分享我这两个月的提效总结。近两个月成果，打通了0-1**。**给我的教训是：工作中不要主次不分，即使写提效代码 ，也要每天坚持更新视频，保持内容创作的节奏。

# 二、选品提效

### 2.1 批量获取对标视频的带货链接

**这里直接提供工具**化解决方案，直接写**成了工具：**window**系统**可以直接下载使用；mac电脑请把代码丢给豆包，让豆包教你如何使用，获取专属使用指导
[使用说明.txt](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Pdkab9IeHoJAU9xSH6fcl7KHntb)

注意里面的**导出Excel功能**，可以直接导出可以使用的京东或者淘宝的链接，而不是B站压缩的链接格式，便于后续操作。
[B站评论提取工具-清泉.exe](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CPOIbIMzeotavfxpr1acFd0Snwb)
**mac版**
[mac版代码.rar](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CqJDbh4t5oMM37xfKf0cGIW4nYg)

### 2.2 批量查询佣金

最简单的方法：点击链接选品，输入待查询链接，识别链接，工具自动识别并输出佣金数据，然后看数据
### 2.3 批量获取产品参数

这里手动获取办法是打开链接，查看商品的详细参数，手动复制到PPT中，流程繁琐，如下图所示：
影刀逐个提取产品详情参数截图。

提效：[提效代码](https://zqxbobng8rp.feishu.cn/docx/YACMd91o4oIYyqxsqMVcs6wenIe?from=from_copylink)

查看里面的**影刀如何实现自动化抓取产品表格**

抓取完表格，接下来可以有两种路可以走：

1.  **AI识别**

> 把这些产品参数表格统统丢给豆包，让豆包识别给出参数

1.  **影刀代码**

这里说一下大体的逻辑了

1.  **在自动化抓取产**品表格的基础上，使用影刀离线ocr的功能，把每个产品参数图的数据分别写入到单个txt文档中，txt文件命名就用产品名字。如下所示。
[俏侬 猪肉酥皮馅饼1.15kg10片 手抓饼煎饼大葱饼 学生早餐健康.txt](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UWZabGwWhoy5n7xnPoGcKWB9nzg)

1.  把单个txt文档，全部写入到一个总的txt文档，用分隔符区分开，每个分隔。

例如 ：
[总参数.txt](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GnFUbtE7nozKpWxCe26c8MA9njP)

=== 文件: 麦西恩原味卷饼270g_3 8英寸 早餐手抓饼墨西哥卷饼塔可饼皮.txt ===

['品牌', '麦西恩', '商品编号', '100136527424', '其他', '产地', '类别', '上海', '国产', '国产/进口', '保质期', '120天', '包装清单', '麦西恩原味卷饼270g*3']

=== 文件结束: 麦西恩原味卷饼270g_3 8英寸 早餐手抓饼墨西哥卷饼塔可饼皮.txt ===

1.  把这个总的txt文档发给豆包。提示词如下：

```
请将txt文件里面的内容以 json的格式输出，便于我复制。另外，如果输出部分内容不确定，也可参考文件名获取相关信息，文件名就是商品，注意：严格按照我给的格式，引号需要成对出现。注意：每一个文件名都要输出，不能遗漏
输出格式例如：[
{
&quot;文件名&quot;: &quot;xxxx&quot;,
&quot;品牌&quot;: &quot;博世（BOSCH）&quot;,
&quot;类型&quot;: &quot;滚筒单洗&quot;,
&quot;能效等级&quot;: &quot;一级能效&quot;,
&quot;变频 / 定频&quot;: &quot;变频&quot;,
},
{
&quot;文件名&quot;: &quot;xxxx&quot;,
&quot;品牌&quot;: &quot;博世（BOSCH）&quot;,
&quot;类型&quot;: &quot;滚筒单洗&quot;,
&quot;能效等级&quot;: &quot;一级能效&quot;,
&quot;变频 / 定频&quot;: &quot;变频&quot;,
},
]

![](img/058054af688c73c038fedfc66e93fe32.png)

![](img/8f875e901c59fb4d706eeaf242b3d531.png)

![](img/e0aa0bece6c214f13ed27a50a1c3e691.png)

![](img/bf7fdd3cdc4d05b73349fae13e0201c3.png)

![](img/14e2c10515e8e2b72fb2cc3af8988c72.png)

![](img/62af813b0eda7ddaee1819299080bdc5.png)

![](img/8a987629d3b5743667d6e0c44546dd9f.png)


![](img/35068e0a11d87d0d9f02b50ec7cee3fb.png)

![](img/8c8e5e3368f4848168f909137f2b4a8a.png)

![](img/6a0b227effba6887d59178289fb4e412.png)

![](img/f7300aacbbc9596c5f79e393cbe17aed.png)

![](img/9b06d48133085556a075bf74464c4fad.png)


![](img/bae39d6e1b07709d6c0487e2630e6f1b.png)

![](img/66829d605379c37a67babc531742c46c.png)


![](img/dc6824bb9b866bd335617140386ee287.png)

![](img/94833299d7e6b833d9ae4e418622d23e.png)

![](img/54d57ccad1971e6b2275e56f1e37d601.png)

![](img/72a4b6bbed9a4f9479a50dca0af1685e.png)

![](img/5f715cac1fd3cbd42af079dd4830ab53.png)


![](img/750d1889336824df361e266f4127a02e.png)

![](img/5de1375c3940393b8401611fc11fa060.png)


![](img/fcf7d253b366dfad6310be9e06ac0cc6.png)


![](img/f01608382c22b2346a8b5cbe49baf17e.png)


![](img/9897bcbaf8ceb4778ad6660e99293280.png)


![](img/4e3a18736e722cc900739b5b81705bb4.png)

```

会得到一个json文件

1.  接下来就是编写一个**魔法指令**，让影刀根据json文件提取参数写入到Excel中。

这样就大功告成了===

因为代码编写涉及到许多魔法指令以及图像识别，所以不便分享。

仅供参考，建议还是自己写一个符合自己项目的更快一点。

**主流程**
**2.获取截图的ocr识别**
**2.5获取产品参数种类**
**3.写入参数到Excel**
**获取截图子流程**
### 2.4 批量获取京东联盟商品信息

**自己开发思路：**

1.  打开影刀，使用网页自动化功能， 使用最上方的批量抓取功能。

1.  使用魔法指令，建立筛选逻辑，导出表格。

**使用方法：**

Chrome浏览器打开京东联盟，搜索好产品，并选择"自营"。
https://api.winrobot360.com/redirect/robot/share?inviteKey=1470c7fe6b4e4eb1

影刀链接如上。用**chrome浏览器**打开这个页面后，点击运行就可以了。输入一下导出路径和导出表格名称。
这个筛选逻辑是学习自圈友王小小的精华帖，并优化了一下价格区间。

**显示结果：**
**应用场景： 选择高佣产品。**

### 2.5 目前我在用的方法

注意：我的方法是环环相扣的，需要一个个影刀脚本去执行。

1.  **影刀脚本：****批量填充分类数据**

1.  **影刀脚本：**导出详情图txt-11月30日更新

1.  **影刀脚本：****填补空缺参数**

需要逐个去运行。

现在我使用的方法：

1.  找到一个对标的B站链接，把它放到Excel中。
[分类 - 模板.xlsx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/F30obZ9odoz3yjxLr9QcLJy7n3f)

```
**使用方法：
  **  创建**完文件夹后，把这个Excel放入其中，重命名为分类**.xlsx 
    写一个分类名称，放入对应的链接
    注意这个文件夹，后续两个脚本使用的文件夹路径就是自己创建的这个。只需要选到这个文件夹即可。
```

1.  **影刀脚本：****批量填充分类数据****。**就可以一键提取对标链接到Excel中，并把链接对应的佣金及推广链接导出到Excel中。(进行下一步前 可以手动添加其他的链接补充)

```
1**.读取**存放在Ex**cel里**面的B站链接
2.根据B站链接获取对应的对标视频链接，区分出是京东链接还是淘宝/天猫链接，写入到Excel里。代码参考2.1
3.分别读取Excel里面商品链接，将商品链接填写到京东联盟-商品推广的输入框中，逐个添加到选品库，最后点击批量推广-&gt;一键推广，会导出一个存放着产品佣金信息的csv表格。
4.数据处理：将csv表格数据读取出来，填写到Excel中。
具体流程看演示视频。
```

```
https://api.winrobot360.com/redirect/robot/share?inviteKey=39421485e4958c5f

需要电脑有chrome浏览器和edge浏览器。
需要chrome浏览器的京东联盟处于登录状态
需要chrome浏览器的在下载之前询问每个文件的保存位置这个功能要打开

拿另外一台笔记本，测试通过，可以正常使用。
注：由于每个人的电脑情况不同。网络速度不同，无法运行是正常情况

![](img/ad3b6c174bb353349c88d5fde932ef1a.png)

```
[演示第一步和第二步.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VQ4gb5kbSoT71ox1a0acISfVntb)

1.  **影刀脚本：****导出详情图txt ****。输入路径，就可以一键实现把Excel里面**的对标链接的商品图片及详情图都爬取下来。

```
2025年11月30日更新
说一个情况，使用的这个插件-cube-seller，获取不到京东的产品详情图片了，插件作者也不更新。
无奈，动用了一下爬虫知识，自己写了一个爬取京东链接的产品详情图的代码。
更新后的影刀链接，这次是个大更新，摒弃了以往需要点击插件才能采集商品详情的方法。采用独家开发的京东商品采集详情的代码。 
本次更新优点：适配不同电脑，不用准备插件。不涉及个人账号
本次更新缺点：不支持淘宝链接
影刀链接：
https://api.winrobot360.com/redirect/robot/share?inviteKey=a005f2c75d8c8437
```

弃用：只需要准备好Excel就可以，其他的条件不需要了。比如京东

```
1.准备好一个Excel，里面**存**放好要读取的商**品链接。

**2.准备好一个网页插件，例如 cubeseller-**魔方电商**助手**，****要求可以打包下载****到本**地  或者你有神奇的代码，可以无视京东/淘宝平台，直接爬取商品详情图。

3.首先先实现单个商品链接的商品详情图爬取，例如：使用cubeseller,点击插件图标，逐个点击图标，最后使用影刀的下载文件功能，修改保存地址重命名保存。(备注：使用插件下载，无法使用网页元素定位，采用图像识别定位方法点击，由于图像识别存在误判，故影刀代码通用性差。)

4.实现单个商品链接的详情图爬取后，批量读取Excel链接，写个循环批量批量爬取。

5.批量解压得到详情图后，使用影刀的离线OCR识别，循环读取图片并把识别结果保存到txt里面。(备注：魔法指令可以多用用)
```

```
https://api.winrobot360.com/redirect/robot/share?inviteKey=fa826333189330b0

注意事项：(不需要了，最新版的影刀代码不需要这个注意事项了，输入直接用就可以)
需要edge浏览器的京东处于登录状态
需要edge浏览器安装扩展 cubeseller-魔方电商助手，并固定在工具栏，并把这个扩展的显示页面悬浮按钮 这个按钮关掉。
还有一个要求就是浏览器的在下载之前询问每个文件的保存位置这个功能要打开


拿另外一台笔记本测试，有个图像识别没有通过，修改了一下测试通过。
注意⚠️： 
1.因为涉及到图像识别/每个人的电脑不同，有概率报错。
2.受限于网速，可能会运行出错，重新运行。
3.看到我的演示视频，有思路的同学，可以直接拿影刀手搓一个了

![](img/6e6175868cebd16bc5d2936f4f7b77f7.png)

![](img/e6db782a40db36917786831f728c1ce6.png)

```
[演示3.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KKXlbKOZqodcJex1exycn700nYk)

1.  **影刀脚本：****填补空缺参数。 **输入路径及需要的参数。就可以一键实现把参数提取到Excel中。

```
**所有的代码其实是环环相扣的，但是由于影刀执行的不稳定性，选择分成多个脚本执****行****。

这**一步的原理是其实是利用AI，把每个产品对应的详情图发给豆包，再发送提示词给豆包，得到json格式的返回数据，读取json数据写入到excel中更新参数。

备注：我这里使用的是影刀操作网页，自动输入参数及提示词得到返回结果。有条件可以采用API的形式得到返回结果。

提示词：
注意： 给出的参数要被用在PPT展示，除了推荐理由和文件名这个字段，其他参数字数一律限制在13个字以内。注意：文件名要用txt里面的完整输出。
注意：输出的第一个文件名，后面不要用简称，txt里面是什么名字就填什么名字。
例如：色域控制在“95% DCI-P3，100% SRGB”，接口控制在“2DP1.4+2HDMI2.0+Audio”。不能超过我给出的示例字数
推荐理由标准：
    1. 结构强制要求（缺一不可）
必须包含“精准目标人群 + 具体场景痛点 + 参数对应解决方案 + 适配/性价比细节”，逻辑闭环，拒绝抽象概括。
    2. 场景化表达规范
    人群要“具体到身份”
    痛点要“落地到动作”
    参数要“转化到利益”
    优势要“补充到细节”
    3. 风格与字数
    语言口语化，像“朋友推荐”的语气，有画面感；控制在40-50字，一句话讲清核心价值，无冗余修饰。
 
注意：如果有未明确的参数，去互联网查找相关产品的相关参数，写出来。不要写未明确，未提及。
请基于提供的文档内容，提取指定的参数信息并以 JSON 格式输出，需满足「商品介绍场景通用展示」需求（如 PPT、详情页、宣传物料等），具体要求如下：
参数覆盖：严格按照指定参数维度提取，每个维度均需填写，未明确的信息可结合文件名及文档上下文合理补充，确保无遗漏。
内容规范：
优先提取核心关键信息，避免冗余技术细节（如复杂操作步骤、非核心测试数据），“特性” 字段汇总商品核心功能、认证、赠品、售后等优势，“推荐理由” 浓缩为 1-2 句商品核心价值总结（突出差异化优势）；
数值单位统一（如 Hz、nit、ms 小写，分辨率用 “×” 连接，避免 “*”），同类参数表述一致（如 “超频”“原生”“认证” 等关键词用法统一）。
注意：txt文件里面的每一个产品都要被提取到，不能有遗漏。txt文件名显示为1-20，说明有20个产品，一个也不能少。
格式要求：
JSON 结构规范，引号成对，无语法错误，每个商品为独立 JSON 对象，整体用数组包裹；
不遗漏任何文件名，文件名需与文档中完全一致，品牌信息从文件名或文档内容中准确提取。
通用适配：输出内容需轻量化、易读易懂，参数值用 “关键数据 + 核心属性” 简洁表述，可直接用于各类展示场景，无需二次大量调整。
注意文件名，不要采用简称，采用原始名称
输出格式例如：[
{
&quot;文件名&quot;: &quot;xxxx&quot;,
&quot;商品简称&quot;: &quot;xxx&quot;,
&quot;类型&quot;: &quot;滚筒单洗&quot;,
&quot;能效等级&quot;: &quot;一级能效&quot;,
&quot;变频 / 定频&quot;: &quot;变频&quot;,
},
{
&quot;文件名&quot;: &quot;xxxx&quot;,
&quot;商品简称&quot;: &quot;xxx&quot;,
&quot;类型&quot;: &quot;滚筒单洗&quot;,
&quot;能效等级&quot;: &quot;一级能效&quot;,
&quot;变频 / 定频&quot;: &quot;变频&quot;,
},
]
```

```
**http**s**://api.winrobo**t360.com/redirect/robot/share?inviteKey=b67340a416053657

需要edge/chrome浏览器的豆包处于登录状态
需要edge浏览器的京东处于登录状态

输入参数有两个，第一个很简单，就是自己创建的文件夹。文件夹里面是分类.xlsx
第二个这里有个需要注意的地方，就是第一个是商品简称，我建议保留，因为PPT制作时不可能把商品名称全部填写上去。注意一下，参数输入是英文逗号，还有就是可以在自己想要的参数后面 加上， 适用人群，适用场景，推荐理由等参数。
如下:
[&quot;商品简称&quot;,&quot;面板&quot;, &quot;对比度&quot;, &quot;类型&quot;, &quot;响应时间&quot;, &quot;屏幕刷新率&quot;, &quot;接口&quot;,  &quot;特性&quot;, &quot;分辨率&quot;,&quot;屏幕尺寸&quot;, &quot;亮度&quot;, &quot;支架底座&quot;,  &quot;色域&quot;,&quot;推荐理由&quot;]
[&quot;商品简称&quot;,&quot;防水指数&quot;, &quot;耐磨指数&quot;, &quot;透湿性能&quot;, &quot;适用人群&quot;,  &quot;适用场景&quot;,&quot;推荐理由&quot;]
[&quot;商品简称&quot;,&quot;品牌&quot;, &quot;桌面长度&quot;, &quot;是否可定制&quot;, &quot;桌面可调角度&quot;, &quot;桌面材质&quot;, &quot;控制方式&quot;, &quot;适用人群&quot;,&quot;适用场景&quot;,&quot;推荐理由&quot;]
注意：
自己测试，没有登录豆包，会出现bug,因为无法复制。
还有有时候，会出现延迟的情况，会出现 一个弹窗，请手动复制，这个时候点击一下 复制按钮，再点击确定即可。


![](img/96a82df2a10996ff4adfcdc0955bc8d8.png)


![](img/36e61ecd6403932b23cdf106c17b2933.png)


![](img/7116d611d8670ffbc9028528646bce6c.png)

```
**最终效果**

完美解决参数问题，再也不需要手动复制了
[分类.xlsx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WkZ5bV5sroJ1jMxfnlccvDUNnvd)

**演示影刀代码搭建-给想要自己搭建的**小伙伴一个借鉴的思路，演示时长半个小时。
[录屏_20251128_230239.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/I24YbNzA8opl2Bx2lpucDFBunzb)

**魔法指令**

```
**# 使用此指**令前，请确保安装**必要****的Pyt**hon库，例如使用以**下命令安装：****
# p****ip ins****tall** pandas **openpy****xl
**
imp**ort ****pandas** as pd
**fr**o**m ope****npyxl **impo**rt l**oad_wo**rkbook**
**from ****ope**npyx**l.s**tyles im**por**t Font, Alig**nm**ent, B**ord**e**r,**** Side**

from typin**g impo**rt *
try**:
    **from xbot.a**pp**.logging imp**ort t**race** as** print
e**xc**ept:
    fro**m x**b**ot** import prin**t

def** **creat**e_sheets**_b**y_category_w**ith_fo**r**mat(e****xce**l_path**):****
  **  &quot;&quot;&quot;**
 **   title: 在**原文**件中创建统**一格**式**的Ex**c**el工作**表
    de**script**i**on:** 读取Ex**ce****l文件**中 % 分**类 **% 列的**内容，****为每个**分类在原文件中创**建新的**工作表，对所**有工**作表（除&quot;分类&quot;**外）**设**置统一**格式，列名使用方**正仿宋_GB**K加粗，**数据行**使用等线字体。将**Sh**eet1重命名为&quot;分**类&quot;**。
    inputs**:
**      **  -** **ex**cel_path (fi**le):** Exc**el文**件**路径，e**g: &quot;data**.xl**sx&quot;
    o**ut**p**uts: **N**one
 **   &quot;&quot;&quot;
 **   ****
 ** **  def _re**ad_excel**_an**d_get_cat**eg**o**ries(**file_path):
**  ** **   ****  &quot;&quot;**&quot;读取Excel文件并获取分类列的唯一值&quot;&quot;&quot;
    **    t**ry:
    **   **     df =** p**d**.read**_**excel**(file_path)
**   **         **if** **&#x27;分类&#x27; **not **in **df.colum**ns:**
       **      **   raise Va**lu**eError(&quot;Exce**l文件中未**找到&#x27;分类&#x27;列&quot;**)
 **          ** c**ategories = **df****[&#x27;分类&#x27;].d**ropna().uniq**ue****().tolis**t()
        **  **  return cat**ego**r**ie**s
      **  e**xcept Except**io**n as e:
    **  **    **  **r**ais**e Except**ion(f**&quot;读取Excel文件失败: {str(e)}&quot;)
    
    def _check_sheet_exists_with_columns(workbook, sheet_name, required_columns):
        &quot;&quot;&quot;检查工作表是否存在且包含指定列名&quot;&quot;&quot;
        if sheet_name not in workbook.sheetnames:
            return False
        
        worksheet = workbook[sheet_name]
        if worksheet.max_row == 0:
            return False
        
        # 获取第一行的值作为列名
        first_row = [cell.value for cell in worksheet[1]]
        existing_columns = [col for col in first_row if col is not None]
        
        # 检查是否包含所有必需的列
        return all(col in existing_columns for col in required_columns)
    
    def _clean_sheet_name(category_name):
        &quot;&quot;&quot;清理工作表名称，确保不包含&#x27;sheet&#x27;并符合Excel命名规则&quot;&quot;&quot;
        sheet_name = str(category_name).replace(&#x27;sheet&#x27;, &#x27;&#x27;).replace(&#x27;Sheet&#x27;, &#x27;&#x27;).replace(&#x27;SHEET&#x27;, &#x27;&#x27;)
        # 移除Excel不允许的字符
        invalid_chars = [&#x27;\\&#x27;, &#x27;/&#x27;, &#x27;*&#x27;, &#x27;[&#x27;, &#x27;]&#x27;, &#x27;:&#x27;, &#x27;?&#x27;]
        for char in invalid_chars:
            sheet_name = sheet_name.replace(char, &#x27;_&#x27;)
        # 确保名称不为空且不超过31个字符
        sheet_name = sheet_name.strip()
        if not sheet_name:
            sheet_name = &quot;分类表&quot;
        return sheet_name[:31]
    
    def _rename_sheet1_to_category(workbook):
        &quot;&quot;&quot;将Sheet1重命名为分类&quot;&quot;&quot;
        if &#x27;Sheet1&#x27; in workbook.sheetnames:
            sheet1 = workbook[&#x27;Sheet1&#x27;]
            if &#x27;分类&#x27; not in workbook.sheetnames:
                sheet1.title = &#x27;分类&#x27;
            else:
                # 如果分类已存在，删除Sheet1
                workbook.remove(sheet1)
    
    def _format_worksheet(worksheet, column_names):
        &quot;&quot;&quot;设置工作表统一格式：固定行高、列宽和预设格式，确保后续新增列也能保持统一&quot;&quot;&quot;
        # 定义每列的固定宽度
        column_widths = {
            &#x27;联盟类型&#x27;: 12,
            &#x27;商品链接&#x27;: 20,
            &#x27;佣金&#x27;: 10,
            &#x27;佣金比例&#x27;: 12,
            &#x27;推广链接&#x27;: 20,
            &#x27;商品名称&#x27;: 18,
            &#x27;到手价&#x27;: 10,
            &#x27;好评数&#x27;: 10
        }
        
        # 统一的字体和样式设置
        header_font = Font(name=&#x27;方正仿宋_GBK&#x27;, size=11, bold=True, color=&#x27;000000&#x27;)  # 列名字体不变
        data_font = Font(name=&#x27;等线&#x27;, size=10, color=&#x27;000000&#x27;)  # 数据行字体改为等线
        header_alignment = Alignment(horizontal=&#x27;center&#x27;, vertical=&#x27;center&#x27;)
        data_alignment = Alignment(horizontal=&#x27;left&#x27;, vertical=&#x27;center&#x27;)
        border = Border(
            left=Side(border_style=&#x27;thin&#x27;, color=&#x27;000000&#x27;),
            right=Side(border_style=&#x27;thin&#x27;, color=&#x27;000000&#x27;),
            top=Side(border_style=&#x27;thin&#x27;, color=&#x27;000000&#x27;),
            bottom=Side(border_style=&#x27;thin&#x27;, color=&#x27;000000&#x27;)
        )
        
        # 清空工作表内容并添加列名
        worksheet.delete_rows(1, worksheet.max_row)
        
        # 设置所有列的默认宽度和格式（预设到Z列，确保后续新增列也有统一格式）
        for col_idx in range(1, 27):  # A到Z列
            column_letter = worksheet.cell(row=1, column=col_idx).column_letter
            worksheet.column_dimensions[column_letter].width = 15
            worksheet.column_dimensions[column_letter].auto_size = False
        
        # 设置指定的列名和其对应的列宽
        for col_idx, col_name in enumerate(column_names, 1):
            cell = worksheet.cell(row=1, column=col_idx)
            cell.value = col_name
            cell.font = header_font
            cell.alignment = header_alignment
            cell.border = border
            
            # 设置特定列的宽度
            column_letter = cell.column_letter
            width = column_widths.get(col_name, 15)
            worksheet.column_dimensions[column_letter].width = width
        
        # 为标题行的所有列（A到Z）预设格式，确保后续新增列名也有统一格式
        for col_idx in range(1, 27):  # A到Z列
            cell = worksheet.cell(row=1, column=col_idx)
            if not cell.value:  # 如果单元格为空，不设置值
                pass
            cell.font = header_font
            cell.alignment = header_alignment
            cell.border = border
        
        # 设置固定行高（禁用自适应）
        # 标题行
        worksheet.row_dimensions[1].height = 24
        worksheet.row_dimensions[1].auto_size = False
        
        # 预设数据行的固定行高和格式（预设500行，确保后续新增数据也有统一格式）
        for row_num in range(2, 501):
            worksheet.row_dimensions[row_num].height = 20
            worksheet.row_dimensions[row_num].auto_size = False
            
            # 为数据行的所有列预设格式（使用等线字体）
            for col_idx in range(1, 27):  # A到Z列
                cell = worksheet.cell(row=row_num, column=col_idx)
                cell.font = data_font  # 数据行使用等线字体
                cell.alignment = data_alignment
                cell.border = border
        
        # 冻结首行
        worksheet.freeze_panes = &#x27;A2&#x27;
    
    def _create_and_format_sheets(file_path, categories):
        &quot;&quot;&quot;创建工作表并格式化所有工作表（除分类外）&quot;&quot;&quot;
        column_names = [&#x27;联盟类型&#x27;, &#x27;商品链接&#x27;, &#x27;佣金&#x27;, &#x27;佣金比例&#x27;, &#x27;推广链接&#x27;, &#x27;商品名称&#x27;, &#x27;到手价&#x27;, &#x27;好评数&#x27;]
        
        try:
            workbook = load_workbook(file_path)
        except Exception as e:
            raise Exception(f&quot;无法打开Excel文件: {str(e)}&quot;)
        
        # 将Sheet1重命名为分类
        _rename_sheet1_to_category(workbook)
        
        # 创建新的分类工作表
        for category in categories:
            sheet_name = _clean_sheet_name(category)
            
            # 跳过&quot;分类&quot;工作表
            if sheet_name == &#x27;分类&#x27;:
                continue
                
            # 检查工作表是否已存在且包含指定列名
            if _check_sheet_exists_with_columns(workbook, sheet_name, column_names):
                continue
            
            # 创建或清理工作表
            if sheet_name not in workbook.sheetnames:
                workbook.create_sheet(title=sheet_name)
        
        # 格式化所有工作表（除了&quot;分类&quot;）
        for sheet_name in workbook.sheetnames:
            if sheet_name != &#x27;分类&#x27;:
                worksheet = workbook[sheet_name]
                _format_worksheet(worksheet, column_names)
        
        # 保存文件
        workbook.save(file_path)
        workbook.close()
    
    # 获取分类列表
    categories = _read_excel_and_get_categories(excel_path)
    
    if not categories:
        raise ValueError(&quot;分类列中没有找到有效数据&quot;)
    
    # 创建工作表并格式化
    _create_and_format_sheets(excel_path, categories)

```

```
**# 使用此指****令前，请确保****安装必要**的Pyth**on库，例如****使用以下命令****安装：
# ****pip **install **openpy****xl **requ**ests**** beaut**ifulsou**p4**
**
impo****rt ope**npyx**l
im**port r**equest**s**
from**** bs**4 im**por**t Beauti**ful**Soup
import **re****
impor**t time

from typing imp**ort *
tr**y:
    from **xb****ot.app**.logging imp**ort tr**ace as p**rint
e**xcept:
    **fr**om xbot impo**rt pri**nt

**def** get_bil**ib**i**li_**t**itles_and_**update**_ex**cel(excel_fi**le):
 ** **  &quot;&quot;&quot;**
    tit**le: 获取**B**站视频**标题并更**新Exce****l
 **   descri**pt**ion:** 从E**xcel文件的&quot;**分类**&quot;sheet**中的 **%** 对**标链接 % 列读取B站链**接，获取视****频标题后****写入 %** 对标标题 % **列的相**应行中，直**接修**改**原文件。
**    inputs:
**  **      - exce**l_fi**le (file**):** Excel文件路径**，e**g**: &quot;d**ata.xlsx&quot;
  **  out**puts: No**ne**
    &quot;&quot;&quot;
  **  **
**    **def _get_bil**ibili**_title(u**rl)**:
   **  ** **  &quot;&quot;&quot;**
        获取B**站视**频**标题
****  ****      &quot;&quot;****&quot;
 **       try:
**  ****     ****     **# 设置请求头，**模拟浏览器**访问
 **      **     header**s **= {
    **     **       &#x27;User-Agent&#x27;: &#x27;Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36&#x27;,
                &#x27;Accept&#x27;: &#x27;text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8&#x27;,
                &#x27;Accept-Language&#x27;: &#x27;zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2&#x27;,
                &#x27;Accept-Encoding&#x27;: &#x27;gzip, deflate&#x27;,
                &#x27;Connection&#x27;: &#x27;keep-alive&#x27;,
                &#x27;Upgrade-Insecure-Requests&#x27;: &#x27;1&#x27;
            }
            
            response = requests.get(url, headers=headers, timeout=10)
            response.raise_for_status()
            
            soup = BeautifulSoup(response.content, &#x27;html.parser&#x27;)
            
            # B站视频标题通常在title标签或特定的meta标签中
            title_element = soup.find(&#x27;title&#x27;)
            if title_element:
                title = title_element.get_text().strip()
                # 去掉B站标题后缀
                title = re.sub(r&#x27;_哔哩哔哩_bilibili$&#x27;, &#x27;&#x27;, title)
                return title
            
            # 尝试从meta标签获取
            meta_title = soup.find(&#x27;meta&#x27;, property=&#x27;og:title&#x27;)
            if meta_title:
                return meta_title.get(&#x27;content&#x27;, &#x27;&#x27;).strip()
                
            return &quot;无法获取标题&quot;
            
        except Exception as e:
            return f&quot;获取失败: {str(e)}&quot;
    
    def _is_bilibili_url(url):
        &quot;&quot;&quot;
        检查是否为B站链接
        &quot;&quot;&quot;
        if not isinstance(url, str):
            return False
        bilibili_patterns = [
            r&#x27;https?://www\.bilibili\.com/video/&#x27;,
            r&#x27;https?://b23\.tv/&#x27;,
            r&#x27;https?://bilibili\.com/video/&#x27;,
            r&#x27;https?://m\.bilibili\.com/video/&#x27;
        ]
        return any(re.match(pattern, url.strip()) for pattern in bilibili_patterns)
    
    try:
        # 加载Excel文件
        workbook = openpyxl.load_workbook(excel_file)
        
        # 检查是否存在&quot;分类&quot;工作表
        if &quot;分类&quot; not in workbook.sheetnames:
            raise ValueError(&quot;Excel文件中未找到名为&#x27;分类&#x27;的工作表&quot;)
        
        worksheet = workbook[&quot;分类&quot;]
        
        # 查找&quot;对标链接&quot;和&quot;对标标题&quot;列
        header_row = 1
        link_col = None
        title_col = None
        
        for col in range(1, worksheet.max_column + 1):
            cell_value = worksheet.cell(row=header_row, column=col).value
            if cell_value == &quot;对标链接&quot;:
                link_col = col
            elif cell_value == &quot;对标标题&quot;:
                title_col = col
        
        if link_col is None:
            raise ValueError(&quot;未找到&#x27;对标链接&#x27;列&quot;)
        if title_col is None:
            raise ValueError(&quot;未找到&#x27;对标标题&#x27;列&quot;)
        
        processed_count = 0
        
        # 遍历所有行，处理B站链接
        for row in range(header_row + 1, worksheet.max_row + 1):
            link_cell = worksheet.cell(row=row, column=link_col)
            title_cell = worksheet.cell(row=row, column=title_col)
            
            # 如果链接单元格为空或标题已存在，跳过
            if not link_cell.value or title_cell.value:
                continue
            
            url = str(link_cell.value).strip()
            
            # 检查是否为B站链接
            if _is_bilibili_url(url):
                print(f&quot;正在处理第{row}行的B站链接: {url}&quot;)
                
                # 获取视频标题
                title = _get_bilibili_title(url)
                
                # 写入标题
                title_cell.value = title
                processed_count += 1
                
                print(f&quot;获取到标题: {title}&quot;)
                
                # 添加延时，避免请求过于频繁
                time.sleep(1)
        
        # 保存修改到原文件
        workbook.save(excel_file)
        workbook.close()
        
        print(f&quot;处理完成！成功处理了{processed_count}个B站链接&quot;)
        
    except Exception as e:
        raise Exception(f&quot;处理Excel文件时出错: {str(e)}&quot;)

```

```
**# 使用此指****令前，请确保****安装必要的P****ython库****，例如使用以****下命令安装：****
# p**ip install req**uests ****pandas**** openp**yxl tqdm**

****impo**rt req**uests
****import**** jso**n
import** re
im****por**t ha**shli****b
impo**rt urll**ib**
**impor****t time**
fro**m ur**llib.p**arse i**m**port ****quo**te, **url**parse, p**ar**se_qs
im**port** os
impo**rt pan**das **as **pd
from **tq**dm import tq**dm
im****port op**enpyxl

**from **typing i**mp**o**rt *****
try:**
    from xb**ot.ap**p.logging impo**rt tr**ace as p**rint
e**xcep**t:
**    from** xbot **impo**rt **print

d**ef c**rawl_bilibili_**com**ments_an**d_writ**e_to**_ca**tegory_s**he**ets_ordered(**excel**_file, c**ookie_**cont**ent**=&quot;&quot;, mod**e=3**):
    &quot;&quot;&quot;
 **  ** **title: B站评**论爬取并按顺**序写入E****xce**l
    descri**pt**ion: 从Excel文件&quot;分类&quot;sheet的 % 对标链接 **% **列读取B站链**接，获取****评论****区商品**链**接，并**按原始评论**顺序**写入对应**的sh****eet中**，数据从第二行开始写入。**
   ** **inputs:
  **      **- ex****cel**_file (fi**le**)**: Excel文件**路径，eg: &quot;**data.x**lsx&quot;**
  **      - **coo**kie_content **(str)****: B**站Cookie内容，用于**访问**A**PI，**e**g: &quot;b****uvid3=**x**xx; ****_uuid**=xxx&quot;
      **  -** mode (int): 评论爬取模式，2为最新评论，3为热门评论，eg: 3
    **outp****ut****s: ****None
 **   &quot;&quot;&quot;
    
**    de****f _g**et_header(co**okie_c**o**nten**t):
    **    &quot;&quot;**&quot;获取B站请求头&quot;&quot;&quot;
**      ** ** if **cook**ie_**content.**st**r**ip(**):
         **   hea**der = {
**   **         **  **  &quot;Cookie&quot;: **co**okie_cont**en**t.strip(**),**
        **  **      **&quot;U**ser-Agent**&quot;:** &#x27;Mozil**la**/5.0 (Win**do****ws NT **10.0; Wi**n64**; x64) Ap**pl**eWebKit/537.**36** (KHTML, **li**ke Gecko**) **Chrome/13**4.**0.0.0 **Sa**fari/537.**36** Edg/13**4.**0.0.0&#x27;
  **  ****      **  }
    **    el**se:
**   ****   **      he**ader **= {
    **   ****  ** **      &quot;Us**er-Agent&quot;: &#x27;**Mozil**la/5.0 (Wind**ows** NT 10**.0****; **Win64;** x6**4**) ****Apple****We**bKit/537.3**6 ****(KHTM****L,**** like**** Gec****ko) C****hrom****e/****134.0**.0.0 Saf**ari/5**37.36 Ed**g/134.**0.0.**0&#x27;,**
       **   **      &quot;Refer**er&quot;: **&quot;https://www**.bili**bili.com/&quot;,
**  ** **   ****     ****     &quot;**Accept&quot;: &quot;ap**plica**tion/json, t**ext/p**lain, */*&quot;,
**      **        **  &quot;Acc**ept-Languag**e&quot;**: &quot;zh-CN,zh;**q=0.9**,en;q=0.8&quot;,
**      **    **   **   &quot;Acce**pt-**Encoding&quot;: &quot;**gzip,** deflate, br**&quot;,**
**     ****      ** **    &quot;**Connection&quot;:** &quot;keep**-**aliv**e&quot;
     **      ** }
        r**eturn **h**eade**r

 **   **def _get**_v**i**deo**_**info(bv, c**ookie_**con**tent):
     **   &quot;&quot;&quot;**通**过BV号获**取视频的oid和**标题&quot;&quot;&quot;
** **   **    **heade****rs **= _get_he**ad**er(c**ook**ie_conte**nt)**
        res**p **=** re****quest**s.get(f&quot;http**s://**w**ww.b****il****ibil**i.com/video/**{bv}/**?p=1&quot;, heade**rs=hea**d**ers,** timeout**=10)
 **       
    **    i**f resp.s**tatus_**code != 200**:
**            **raise** Exc**ept**ion(f&quot;获取**视频信**息失败，状态码: {re**sp**.**sta****tus_c****ode}&quot;)**
**     **   
        **# 提取视**频oid
       ** o**bj = re.compile**(f&#x27;**&quot;**ai****d&quot;:(?****P&lt;id&gt;.*****?),&quot;b**vid&quot;:&quot;{bv}&quot;&#x27;**)
   **     match =** obj.**search(resp.**tex**t)
        if** n**o**t match:
**            **raise** Exception(&quot;**无法****获取视**频oid&quot;**)
** **     ****  o**id = **ma**t**ch.gr**o**up(&#x27;**id&#x27;)

      **  ret**urn oid

   ** de**f _md5_encrypt**(c**o**de):
    ****   ** **&quot;&quot;&quot;**M**D5加**密**&quot;&quot;&quot;**
**   ** **   ** **MD5** **= h**a**shl****ib.md****5()
  **      MD5.u**pd****ate(c****ode.enco**de(&#x27;utf-8&#x27;))**
    **    return M**D5.****hexdi****gest()**
**
   ** def _genera**te_api**_url(oid, m**od****e):
 ****      ** **&quot;&quot;&quot;生成**B站评论API的**URL&quot;&quot;&quot;**
        pl**at** = 1
       ** type** = 1  
     **   **web_location** = 131****5875**
        wts** = int**(**time.**time**())**
       ** 
   **     pag**inati**on_str =** &#x27;{&quot;o**ffset&quot;:&quot;**&quot;}**&#x27;
    **   ** **co**de = f&quot;mode=**{mode****}&amp;oi****d={o**id}&amp;pagi**natio**n_str={u**rll**ib.pa**rs**e**.quot**e(pagination**_str)**}&amp;plat={plat**}&amp;**seek_rpid=&amp;t**ype=**{type}&amp;w**eb**_location=**{w**e**b_lo**cation}&amp;wts=**{wts}**&quot; + &#x27;ea1**db**124af3c7062474**69**3**fa70**4f4ff8&#x27;
    **    w**_rid = _**md5_e**ncrypt(c**ode**)
   **  ** **  url** = f&quot;https:/**/a**p**i.b****ilibili.****com****/x/****v2****/rep**ly/wbi/main?**oi****d={oi****d}&amp;ty****pe={t****yp****e}&amp;mo****de**={mode}&amp;paginat**ion****_s****tr={u****rlli****b.par****se.q**u**ote****(pagi****nati****on_st****r, s****afe=&#x27;****:&#x27;)}&amp;**plat=1&amp;s**eek_r**pid=**&amp;web_l**ocation=131**58**75&amp;w_rid**={w_r**id}&amp;wts=**{wt**s}&quot;
    **    
 **       retur**n ur**l

    d**ef _g**et_comments_data(bv, mode, cookie_content):
        &quot;&quot;&quot;获取B站评论数据&quot;&quot;&quot;
        oid = _get_video_info(bv, cookie_content)
        url = _generate_api_url(oid, mode)
        
        headers = _get_header(cookie_content)
        response = requests.get(url, headers=headers, timeout=10)
        
        if response.status_code != 200:
            raise Exception(f&quot;获取评论失败，状态码: {response.status_code}&quot;)
            
        return response.json()

    def _extract_messages_from_data_ordered(data):
        &quot;&quot;&quot;从评论数据中按顺序提取所有message内容&quot;&quot;&quot;
        messages = []
        
        def find_all_messages_ordered(obj, path=&quot;&quot;):
            if isinstance(obj, dict):
                # 按键的顺序遍历，保持原始结构顺序
                for key, value in obj.items():
                    current_path = f&quot;{path}.{key}&quot; if path else key
                    if key == &quot;message&quot; and isinstance(value, str):
                        messages.append(value)
                    else:
                        find_all_messages_ordered(value, current_path)
            elif isinstance(obj, list):
                # 按索引顺序遍历列表
                for i, item in enumerate(obj):
                    current_path = f&quot;{path}[{i}]&quot;
                    find_all_messages_ordered(item, current_path)
        
        find_all_messages_ordered(data)
        return messages

    def _expand_b23_link(short_url, cookie_content):
        &quot;&quot;&quot;展开B站短链接&quot;&quot;&quot;
        try:
            b23_pattern = r&#x27;https://b23\.tv/[a-zA-Z0-9-]+&#x27;
            match = re.search(b23_pattern, str(short_url))
            
            if not match:
                return None
            
            valid_url = match.group(0)
            headers = _get_header(cookie_content)
            headers[&#x27;Accept&#x27;] = &#x27;text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8&#x27;
            
            try:
                response = requests.get(valid_url, headers=headers, allow_redirects=True, timeout=10)
                if response.status_code == 200 and response.url != valid_url:
                    return response.url
            except:
                pass
            
            return None
        except:
            return None

    def _analyze_link_type(url):
        &quot;&quot;&quot;分析链接类型&quot;&quot;&quot;
        if not url:
            return &quot;未知类型&quot;
        
        parsed_url = urlparse(url)
        domain = parsed_url.netloc.lower()
        path = parsed_url.path.lower()
        query = parsed_url.query.lower()
        full_url = url.lower()
        
        taobao_keywords = [
            &#x27;taobao.com&#x27;, &#x27;tmall.com&#x27;, &#x27;alimama.com&#x27;, &#x27;alibaba.com&#x27;,
            &#x27;taobao&#x27;, &#x27;tmall&#x27;, &#x27;alimama&#x27;, &#x27;alibaba&#x27;, &#x27;mall&#x27;, 
            &#x27;item.taobao&#x27;, &#x27;detail.tmall&#x27;, &#x27;click.taobao&#x27;
        ]
        
        jd_keywords = [
            &#x27;jd.com&#x27;, &#x27;jd.cn&#x27;, &#x27;jd&#x27;, &#x27;jingdong&#x27;, &#x27;jdunion&#x27;,
            &#x27;item.jd&#x27;, &#x27;product.jd&#x27;, &#x27;click.jd&#x27;
        ]
        
        for keyword in taobao_keywords:
            if keyword in domain or keyword in path or keyword in query or keyword in full_url:
                return &quot;淘宝联盟&quot;
        
        for keyword in jd_keywords:
            if keyword in domain or keyword in path or keyword in query or keyword in full_url:
                return &quot;京东联盟&quot;
        
        return &quot;其他链接&quot;

    def _process_links_ordered(messages, cookie_content):
        &quot;&quot;&quot;按顺序处理评论中的链接，保持原始评论顺序&quot;&quot;&quot;
        processed_links = []
        seen_links = set()  # 用于去重，但不影响顺序
        
        print(f&quot;开始按顺序分析 {len(messages)} 条评论中的链接...&quot;)
        
        # 按评论顺序处理每条消息
        for msg_index, message in enumerate(messages):
            print(f&quot;处理第 {msg_index + 1} 条评论: {message[:30]}...&quot;)
            
            # 从当前消息中提取所有链接
            current_links = []
            
            # 查找所有b23.tv链接
            b23_links = re.findall(r&#x27;https://b23\.tv/[a-zA-Z0-9-]+&#x27;, message)
            current_links.extend(b23_links)
            
            # 查找其他常见链接
            other_links = re.findall(r&#x27;https?://[^\s]+&#x27;, message)
            current_links.extend(other_links)
            
            # 按在评论中出现的顺序处理链接
            for link in current_links:
                if link not in seen_links:  # 去重但保持顺序
                    seen_links.add(link)
                    
                    print(f&quot;  处理链接: {link[:50]}...&quot;)
                    
                    if &#x27;b23.tv&#x27; in link:
                        print(f&quot;    展开B站短链接...&quot;)
                        real_url = _expand_b23_link(link, cookie_content)
                        if real_url:
                            print(f&quot;    展开成功: {real_url[:50]}...&quot;)
                        else:
                            print(f&quot;    展开失败，使用原链接&quot;)
                            real_url = link
                    else:
                        real_url = link
                    
                    if real_url:
                        link_type = _analyze_link_type(real_url)
                        processed_links.append({
                            &#x27;联盟类型&#x27;: link_type,
                            &#x27;商品链接&#x27;: real_url,
                            &#x27;佣金&#x27;: &#x27;&#x27;,
                            &#x27;佣金比例&#x27;: &#x27;&#x27;,
                            &#x27;推广链接&#x27;: &#x27;&#x27;,
                            &#x27;商品名称&#x27;: &#x27;&#x27;,
                            &#x27;到手价&#x27;: &#x27;&#x27;,
                            &#x27;好评数&#x27;: &#x27;&#x27;
                        })
                        print(f&quot;    ✅ 添加链接: [{link_type}] {real_url[:50]}...&quot;)
        
        print(f&quot;✅ 按顺序处理完成，共找到 {len(processed_links)} 个唯一商品链接&quot;)
        return processed_links

    def _crawl_bilibili_comments_and_analyze_links_ordered(bv_number, cookie_content, mode):
        &quot;&quot;&quot;根据BV号爬取B站评论并按顺序分析链接&quot;&quot;&quot;
        try:
            print(&quot;开始获取B站评论数据...&quot;)
            comment_data = _get_comments_data(bv_number.strip(), mode, cookie_content)
            
            print(&quot;按顺序提取评论内容...&quot;)
            messages = _extract_messages_from_data_ordered(comment_data)
            
            if not messages:
                print(&quot;未找到任何评论内容&quot;)
                return []
            
            print(f&quot;找到 {len(messages)} 条评论，按顺序分析链接...&quot;)
            processed_links = _process_links_ordered(messages, cookie_content)
            
            print(f&quot;✅ 分析完成，按顺序找到 {len(processed_links)} 条商品链接&quot;)
            return processed_links
            
        except Exception as e:
            print(f&quot;处理失败: {str(e)}&quot;)
            return []

    def _extract_bv_number(url):
        &quot;&quot;&quot;从B站链接中提取BV号&quot;&quot;&quot;
        try:
            bv_pattern = r&#x27;(BV[a-zA-Z0-9]{10})&#x27;
            match = re.search(bv_pattern, url)
            if match:
                return match.group(1)
            return None
        except:
            return None

    def _is_bilibili_url(url):
        &quot;&quot;&quot;检查是否为B站链接&quot;&quot;&quot;
        if not isinstance(url, str):
            return False
        bilibili_patterns = [
            r&#x27;https?://www\.bilibili\.com/video/&#x27;,
            r&#x27;https?://b23\.tv/&#x27;,
            r&#x27;https?://bilibili\.com/video/&#x27;,
            r&#x27;https?://m\.bilibili\.com/video/&#x27;
        ]
        return any(re.match(pattern, url.strip()) for pattern in bilibili_patterns)

    def _check_excel_file_access(excel_file):
        &quot;&quot;&quot;检查Excel文件是否可以访问&quot;&quot;&quot;
        try:
            if not os.path.exists(excel_file):
                raise Exception(f&quot;文件不存在: {excel_file}&quot;)
            
            with open(excel_file, &#x27;r+b&#x27;) as f:
                pass
            
            print(f&quot;✅ Excel文件访问正常: {excel_file}&quot;)
            return True
            
        except PermissionError:
            raise Exception(f&quot;文件被占用或权限不足，请关闭Excel后重试: {excel_file}&quot;)
        except Exception as e:
            raise Exception(f&quot;文件访问错误: {str(e)}&quot;)

    def _write_links_to_category_sheet_from_row2(excel_file, category_name, processed_links):
        &quot;&quot;&quot;将商品链接数据从第二行开始按顺序写入到对应分类的sheet中&quot;&quot;&quot;
        try:
            if not processed_links:
                print(f&quot;分类 &#x27;{category_name}&#x27; 没有商品链接需要写入&quot;)
                return False
            
            print(f&quot;准备从第二行开始按顺序写入 {len(processed_links)} 个商品链接到 &#x27;{category_name}&#x27; sheet&quot;)
            
            workbook = openpyxl.load_workbook(excel_file)
            
            if category_name not in workbook.sheetnames:
                print(f&quot;❌ 未找到分类 &#x27;{category_name}&#x27; 对应的sheet&quot;)
                workbook.close()
                return False
            
            category_sheet = workbook[category_name]
            print(f&quot;✅ 找到目标sheet: &#x27;{category_name}&#x27;&quot;)
            
            # 设置表头
            headers = [&#x27;联盟类型&#x27;, &#x27;商品链接&#x27;, &#x27;佣金&#x27;, &#x27;佣金比例&#x27;, &#x27;推广链接&#x27;, &#x27;商品名称&#x27;, &#x27;到手价&#x27;, &#x27;好评数&#x27;]
            
            print(&quot;设置表头（第一行）...&quot;)
            for col, header in enumerate(headers, 1):
                category_sheet.cell(row=1, column=col, value=header)
            
            # 清除第二行及以后的所有数据
            print(&quot;清除原有数据...&quot;)
            if category_sheet.max_row &gt; 1:
                for row in range(2, category_sheet.max_row + 1):
                    for col in range(1, 9):
                        category_sheet.cell(row=row, column=col, value=None)
            
            # 从第二行开始按顺序写入数据
            print(&quot;从第二行开始按评论顺序写入数据...&quot;)
            for i, link_data in enumerate(processed_links):
                row_num = 2 + i
                
                try:
                    category_sheet.cell(row=row_num, column=1).value = str(link_data[&#x27;联盟类型&#x27;])
                    category_sheet.cell(row=row_num, column=2).value = str(link_data[&#x27;商品链接&#x27;])
                    category_sheet.cell(row=row_num, column=3).value = str(link_data[&#x27;佣金&#x27;])
                    category_sheet.cell(row=row_num, column=4).value = str(link_data[&#x27;佣金比例&#x27;])
                    category_sheet.cell(row=row_num, column=5).value = str(link_data[&#x27;推广链接&#x27;])
                    category_sheet.cell(row=row_num, column=6).value = str(link_data[&#x27;商品名称&#x27;])
                    category_sheet.cell(row=row_num, column=7).value = str(link_data[&#x27;到手价&#x27;])
                    category_sheet.cell(row=row_num, column=8).value = str(link_data[&#x27;好评数&#x27;])
                    
                    print(f&quot;  ✅ 第{i+1}行写入成功 (第{row_num}行): [{link_data[&#x27;联盟类型&#x27;]}] {link_data[&#x27;商品链接&#x27;][:50]}...&quot;)
                    
                except Exception as write_error:
                    print(f&quot;  ❌ 第{i+1}行写入失败: {str(write_error)}&quot;)
                    continue
            
            print(&quot;正在保存文件...&quot;)
            try:
                workbook.save(excel_file)
                workbook.close()
                print(f&quot;✅ 成功按顺序保存到 &#x27;{category_name}&#x27; sheet，数据从第2行开始&quot;)
                return True
            except Exception as save_error:
                print(f&quot;❌ 保存文件失败: {str(save_error)}&quot;)
                workbook.close()
                return False
                
        except Exception as e:
            print(f&quot;❌ 写入操作失败: {str(e)}&quot;)
            try:
                workbook.close()
            except:
                pass
            return False

    try:
        print(&quot;检查Excel文件访问权限...&quot;)
        _check_excel_file_access(excel_file)
        
        workbook = openpyxl.load_workbook(excel_file)
        print(f&quot;✅ 成功加载Excel文件: {excel_file}&quot;)
        print(f&quot;可用的sheet: {workbook.sheetnames}&quot;)
        
        if &quot;分类&quot; not in workbook.sheetnames:
            workbook.close()
            raise ValueError(&quot;Excel文件中未找到名为&#x27;分类&#x27;的工作表&quot;)
        
        worksheet = workbook[&quot;分类&quot;]
        
        # 查找所需列
        header_row = 1
        link_col = None
        category_col = None
        
        print(&quot;正在查找列标题...&quot;)
        for col in range(1, worksheet.max_column + 1):
            cell_value = worksheet.cell(row=header_row, column=col).value
            print(f&quot;第{col}列: {cell_value}&quot;)
            if cell_value == &quot;对标链接&quot;:
                link_col = col
            elif cell_value == &quot;分类&quot;:
                category_col = col
        
        if link_col is None:
            workbook.close()
            raise ValueError(&quot;未找到&#x27;对标链接&#x27;列&quot;)
        if category_col is None:
            workbook.close()
            raise ValueError(&quot;未找到&#x27;分类&#x27;列&quot;)
        
        print(f&quot;找到列位置 - 对标链接:{link_col}, 分类:{category_col}&quot;)
        
        workbook.close()
        processed_count = 0
        
        # 重新打开文件读取数据
        workbook = openpyxl.load_workbook(excel_file)
        worksheet = workbook[&quot;分类&quot;]
        
        # 遍历所有行，处理B站链接
        for row in range(header_row + 1, worksheet.max_row + 1):
            link_cell = worksheet.cell(row=row, column=link_col)
            category_cell = worksheet.cell(row=row, column=category_col)
            
            if not link_cell.value:
                continue
            
            url = str(link_cell.value).strip()
            category_name = str(category_cell.value).strip() if category_cell.value else &quot;&quot;
            
            if _is_bilibili_url(url):
                print(f&quot;\n=== 处理第{row}行 ===&quot;)
                print(f&quot;B站链接: {url}&quot;)
                print(f&quot;对应分类sheet: {category_name}&quot;)
                
                bv_number = _extract_bv_number(url)
                if bv_number:
                    print(f&quot;提取到BV号: {bv_number}&quot;)
                    
                    workbook.close()
                    
                    # 按顺序获取评论区商品链接
                    processed_links = _crawl_bilibili_comments_and_analyze_links_ordered(bv_number, cookie_content, mode)
                    
                    if category_name and processed_links:
                        success = _write_links_to_category_sheet_from_row2(excel_file, category_name, processed_links)
                        if success:
                            print(f&quot;✅ 成功按顺序写入商品链接到 &#x27;{category_name}&#x27; sheet&quot;)
                        else:
                            print(f&quot;❌ 写入商品链接到 &#x27;{category_name}&#x27; sheet 失败&quot;)
                    elif not category_name:
                        print(&quot;⚠️ 未指定分类，跳过商品链接写入&quot;)
                    else:
                        print(&quot;ℹ️ 没有商品链接需要写入&quot;)
                    
                    workbook = openpyxl.load_workbook(excel_file)
                    worksheet = workbook[&quot;分类&quot;]
                else:
                    print(&quot;❌ 无法提取BV号&quot;)
                
                processed_count += 1
                
                print(&quot;等待2秒...&quot;)
                time.sleep(2)
        
        workbook.close()
        
        print(f&quot;\n🎉 处理完成！成功处理了{processed_count}个B站链接，所有数据按评论顺序从第二行开始写入&quot;)
        
    except Exception as e:
        print(f&quot;❌ 处理Excel文件时出错: {str(e)}&quot;)
        try:
            workbook.close()
        except:
            pass
        raise Exception(f&quot;处理Excel文件时出错: {str(e)}&quot;)

```

```
**# 使用此指**令前，请确保安装**必要****的Pytho****n库，例**如使用以下命令安**装：
# p****ip **inst**all ****pandas** openpy**xl**
**
impo****rt pan**das **as p**d
impo**rt os
**
**from ****typ**ing **im**p**ort** *
try:
**    f**rom **xb**o**t.a**pp.loggi**ng im**port** tr**ace as p**rin**t
except:
  **  **from xbo**t i**m**po****rt pr****int

d**ef extract_j**d_**links_fr**om_**e**xc****el(ex****cel_fi**le_path):
  **  &quot;&quot;&quot;
**    titl**e: 提取E**xcel文件中京东联盟**商品**链接
    descr**iptio**n: 从指定的Excel**文件中提取除**了&quot;分类**&quot;sh**eet外其他所有**she**et中联盟类型为&quot;京东联**盟&quot;**的商品链接，输出时包含%** s****heet名称 %**和对应的% 链接列表 %**。
 ****  ** inputs:
   **     -** excel_file**_p****ath (****file): E**xcel文件路径**，e**g**: &quot;**C:/data/prod**ucts.**xlsx&quot;
  **  outp**uts:**
     **   - result** (**list): 包**含shee**t名称和链接的字典列表，eg: &quot;[{&#x27;sheet_name&#x27;: &#x27;Sheet1&#x27;, &#x27;links&#x27;: [&#x27;http://example1.com&#x27;, &#x27;http://example2.com&#x27;]}]&quot;
    &quot;&quot;&quot;
    
    # 1. 检查输入有效性
    if not os.path.exists(excel_file_path):
        raise FileNotFoundError(f&quot;Excel文件不存在: {excel_file_path}&quot;)
    
    if not excel_file_path.lower().endswith((&#x27;.xlsx&#x27;, &#x27;.xls&#x27;)):
        raise ValueError(&quot;输入文件必须是Excel格式(.xlsx或.xls)&quot;)
    
    def _extract_links_from_sheet(sheet_data, sheet_name):
        &quot;&quot;&quot;
        从单个sheet中提取京东联盟链接
        &quot;&quot;&quot;
        try:
            # 检查必要的列是否存在
            if &#x27;联盟类型&#x27; not in sheet_data.columns:
                print(f&quot;警告: Sheet &#x27;{sheet_name}&#x27; 中未找到&#x27;联盟类型&#x27;列，跳过处理&quot;)
                return []
            
            if &#x27;商品链接&#x27; not in sheet_data.columns:
                print(f&quot;警告: Sheet &#x27;{sheet_name}&#x27; 中未找到&#x27;商品链接&#x27;列，跳过处理&quot;)
                return []
            
            # 筛选联盟类型为&quot;京东联盟&quot;的行
            jd_rows = sheet_data[sheet_data[&#x27;联盟类型&#x27;] == &#x27;京东联盟&#x27;]
            
            # 提取商品链接列，去除空值
            links = jd_rows[&#x27;商品链接&#x27;].dropna().tolist()
            
            return links
            
        except Exception as e:
            print(f&quot;处理Sheet &#x27;{sheet_name}&#x27; 时出错: {str(e)}&quot;)
            return []
    
    # 2. 读取Excel文件并处理
    try:
        # 读取所有sheet名称
        excel_file = pd.ExcelFile(excel_file_path)
        sheet_names = excel_file.sheet_names
        
        result = []
        
        # 遍历所有sheet（除了&quot;分类&quot;）
        for sheet_name in sheet_names:
            if sheet_name == &#x27;分类&#x27;:
                continue
                
            try:
                # 读取sheet数据
                sheet_data = pd.read_excel(excel_file_path, sheet_name=sheet_name)
                
                # 提取京东联盟链接
                links = _extract_links_from_sheet(sheet_data, sheet_name)
                
                # 添加到结果中
                if links:  # 只有当有链接时才添加
                    result.append({
                        &#x27;sheet_name&#x27;: sheet_name,
                        &#x27;links&#x27;: links
                    })
                    
            except Exception as e:
                print(f&quot;读取Sheet &#x27;{sheet_name}&#x27; 时出错: {str(e)}&quot;)
                continue
        
        excel_file.close()
        
        if not result:
            print(&quot;未找到任何京东联盟链接&quot;)
        
        return result
        
    except Exception as e:
        raise Exception(f&quot;处理Excel文件时出错: {str(e)}&quot;)

```

```
**# 使用此指****令前，请确保****安装必要**的Python库，例**如使用以下命****令安装：**
# pip i**nstall**** op**enpy**xl

****import** csv
im**po**r**t os
****from o**penp**yxl **import** Workb**o**ok

f****rom** typ**in**g** im**port *
t**ry:
 **   f**ro**m** xb**ot.app.l**oggin**g im**po**r**t t**race as **print**
exc**ep**t:
    from **xbo**t** im**port** pr**int

def** cs**v_to_excel_w**ith_**c**sv_m**odule(csv_file_path, excel_f**il****e_**p**ath****):
   **** &quot;&quot;****&quot;
    ****title:**** 使用**CSV模块转换**CS****V文**件为Excel**文件****
    d****escrip**tion: 使用**Python**内置的csv模块读取CS**V文件 %c**sv_f**ile**_path%，并**转换为**Excel文件保存到** %**excel_file_p**ath****%。
 ** **  in**puts:
        - csv_file_pat**h ****(fil****e): C**SV文件路径，eg: &quot;**data.c****sv&quot;
    **    - excel_**file_p**ath (str): **Ex****ce****l文件保存**路径，eg: &quot;**ou**t**put**.xlsx&quot;
    o**utput**s:
     **   - r**esul**t (**str): 转换**结果信****息，**e**g: &quot;转换成功&quot;**
    &quot;&quot;&quot;
   ** 
 ****  ** **# 检查输入文件是**否存在
**   ** if **not os**.path.exist**s(**csv_file**_path**):
 **      ** raise FileNotFoundError(f&quot;CSV文件不存在: {csv_file_path}&quot;)
    
    # 检查输入文件是否为CSV格式
    if not csv_file_path.lower().endswith(&#x27;.csv&#x27;):
        raise ValueError(&quot;输入文件必须是CSV格式&quot;)
    
    # 检查输出文件路径是否为Excel格式
    if not excel_file_path.lower().endswith((&#x27;.xlsx&#x27;, &#x27;.xls&#x27;)):
        raise ValueError(&quot;输出文件必须是Excel格式(.xlsx或.xls)&quot;)
    
    # 确保输出目录存在
    output_dir = os.path.dirname(excel_file_path)
    if output_dir and not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    def _detect_delimiter(file_path, encoding):
        &quot;&quot;&quot;检测CSV文件分隔符&quot;&quot;&quot;
        try:
            with open(file_path, &#x27;r&#x27;, encoding=encoding, newline=&#x27;&#x27;) as csvfile:
                sample = csvfile.read(1024)
                if not sample.strip():
                    return &#x27;,&#x27;  # 空文件使用默认逗号
                
                sniffer = csv.Sniffer()
                try:
                    delimiter = sniffer.sniff(sample).delimiter
                    return delimiter
                except csv.Error:
                    # 自动检测失败，尝试常见分隔符
                    common_delimiters = [&#x27;,&#x27;, &#x27;;&#x27;, &#x27;\t&#x27;, &#x27;|&#x27;]
                    for delim in common_delimiters:
                        if delim in sample:
                            return delim
                    return &#x27;,&#x27;  # 默认使用逗号
        except Exception:
            return &#x27;,&#x27;
    
    def _read_csv_data(file_path):
        &quot;&quot;&quot;读取CSV文件数据&quot;&quot;&quot;
        data = []
        encodings = [&#x27;utf-8&#x27;, &#x27;gbk&#x27;, &#x27;utf-8-sig&#x27;, &#x27;gb2312&#x27;]
        
        for encoding in encodings:
            try:
                # 检测分隔符
                delimiter = _detect_delimiter(file_path, encoding)
                
                with open(file_path, &#x27;r&#x27;, encoding=encoding, newline=&#x27;&#x27;) as csvfile:
                    reader = csv.reader(csvfile, delimiter=delimiter)
                    data = list(reader)
                    break
            except UnicodeDecodeError:
                continue
            except Exception as e:
                if encoding == encodings[-1]:  # 最后一个编码也失败
                    raise ValueError(f&quot;读取CSV文件失败: {str(e)}&quot;)
        
        if not data:
            raise ValueError(&quot;CSV文件为空或无法读取&quot;)
        
        return data
    
    def _write_excel_data(data, file_path):
        &quot;&quot;&quot;写入Excel文件&quot;&quot;&quot;
        wb = Workbook()
        ws = wb.active
        ws.title = &quot;Sheet1&quot;
        
        # 写入数据
        for row_index, row_data in enumerate(data, 1):
            for col_index, cell_value in enumerate(row_data, 1):
                ws.cell(row=row_index, column=col_index, value=cell_value)
        
        wb.save(file_path)
    
    try:
        # 读取CSV数据
        csv_data = _read_csv_data(csv_file_path)
        
        # 写入Excel文件
        _write_excel_data(csv_data, excel_file_path)
        
    except Exception as e:
        raise ValueError(f&quot;转换过程中发生错误: {str(e)}&quot;)
    
    return f&quot;转换成功！已将 {os.path.basename(csv_file_path)} 转换为 {os.path.basename(excel_file_path)}，共 {len(csv_data)} 行数据&quot;

```

```
**# 使用此指****令前，请****确保安装必要****的Pytho**n库，例如使用以**下命****令安装：**
# pip i**nstall**** op**enpy**xl

****import** openpy**xl**
**from ****openpy**xl.u**tils**.dataf**rame i**m**port ****dat**afra**me**_**to_****ro**w**s
i**mport pandas** a**s** pd**

from t**yping** imp**ort** *
try:
**   ** from xbot.a**pp.log**ging imp**ort tr**ace as prin**t
**except:
    **from **xbot** im**port pri**nt
**
def **re**p**lace_**sheet_conten**t(t**arget**_e**x**cel_p****ath,** she**et_**name, so**urc****e_**e**xcel_path**):
    &quot;**&quot;&quot;
****  ** ** title: 替**换Excel工作表内容
**   **** d**e**scription**: 读取** % **source_e**xc**el_path % 文件**的内容**，**清空** % target_ex**cel_p**ath % 中指**定 % sh**eet_**name %** 的数据，然后将源文件**内容**写入目标工作表，**保持原有格**式不变。
    inputs:
        - target_excel_path (file): 目标Excel文件路径，eg: &quot;C:/target.xlsx&quot;
        - sheet_name (str): 目标工作表名称，eg: &quot;Sheet1&quot;
        - source_excel_path (file): 待写入的Excel文件路径，eg: &quot;C:/source.xlsx&quot;
    outputs:
        - result (str): 操作结果信息，eg: &quot;成功替换工作表内容&quot;
    &quot;&quot;&quot;
    
    # 检查输入有效性
    if not target_excel_path or not sheet_name or not source_excel_path:
        raise ValueError(&quot;所有参数都不能为空&quot;)
    
    def _read_source_data(source_path):
        &quot;&quot;&quot;读取源Excel文件的第一个工作表数据&quot;&quot;&quot;
        try:
            df = pd.read_excel(source_path, engine=&#x27;openpyxl&#x27;)
            return df
        except Exception as e:
            raise ValueError(f&quot;读取源Excel文件失败: {str(e)}&quot;)
    
    def _clear_sheet_content(worksheet):
        &quot;&quot;&quot;清空工作表内容但保持格式&quot;&quot;&quot;
        max_row = worksheet.max_row
        max_col = worksheet.max_column
        
        # 清空所有单元格的值
        for row in range(1, max_row + 1):
            for col in range(1, max_col + 1):
                cell = worksheet.cell(row=row, column=col)
                cell.value = None
    
    def _write_data_to_sheet(worksheet, data_df):
        &quot;&quot;&quot;将数据写入工作表&quot;&quot;&quot;
        # 写入列头
        for col_idx, column_name in enumerate(data_df.columns, 1):
            worksheet.cell(row=1, column=col_idx, value=column_name)
        
        # 写入数据
        for row_idx, row_data in enumerate(data_df.values, 2):
            for col_idx, value in enumerate(row_data, 1):
                worksheet.cell(row=row_idx, column=col_idx, value=value)
    
    try:
        # 读取源数据
        source_data = _read_source_data(source_excel_path)
        
        # 打开目标Excel文件
        workbook = openpyxl.load_workbook(target_excel_path)
        
        # 检查工作表是否存在
        if sheet_name not in workbook.sheetnames:
            raise ValueError(f&quot;工作表 &#x27;{sheet_name}&#x27; 在目标文件中不存在&quot;)
        
        # 获取目标工作表
        target_sheet = workbook[sheet_name]
        
        # 清空工作表内容
        _clear_sheet_content(target_sheet)
        
        # 写入新数据
        _write_data_to_sheet(target_sheet, source_data)
        
        # 保存文件
        workbook.save(target_excel_path)
        workbook.close()
        
        return f&quot;成功替换工作表 &#x27;{sheet_name}&#x27; 的内容，共写入 {len(source_data)} 行数据&quot;
        
    except Exception as e:
        raise Exception(f&quot;操作失败: {str(e)}&quot;)

```

```
**import**** os
**
from ty**ping i****mpo**rt ***
try****:
    **from xb**ot**.**app.l****ogging** imp**ort **trace **as pri**n**t
exc****ept**:
  **  **f**rom** xbo**t **i**mpo**rt print**

def** cre**ate**_require**d_**s**ubf**olde**rs(**input_folde**r)**:
    &quot;&quot;&quot;
    title: 检查并创建指定子文件夹
    description: 在指定的文件夹 % input_folder % 下检查是否存在&quot;参数图片&quot;、&quot;参数txt&quot;、&quot;详情图片&quot;、&quot;详情图txt&quot;这四个子文件夹，如果不存在则自动创建。如果指定的主文件夹不存在，也会自动创建。
    inputs:
        - input_folder (folder): 要检查的文件夹路径，eg: &quot;C:/Users/Desktop/项目文件夹&quot;
    outputs:
        - None
    &quot;&quot;&quot;
    
    # 如果输入文件夹不存在，则创建它
    if not os.path.exists(input_folder):
        os.makedirs(input_folder)
    
    # 检查是否为文件夹类型
    if not os.path.isdir(input_folder):
        raise ValueError(f&quot;指定的路径不是文件夹: {input_folder}&quot;)
    
    # 定义需要检查和创建的子文件夹列表
    required_subfolders = [&quot;参数图片&quot;, &quot;参数txt&quot;, &quot;详情图片&quot;, &quot;详情图txt&quot;]
    
    def _create_subfolder_if_not_exists(parent_folder, subfolder_name):
        &quot;&quot;&quot;
        检查并创建子文件夹
        &quot;&quot;&quot;
        subfolder_path = os.path.join(parent_folder, subfolder_name)
        if not os.path.exists(subfolder_path):
            os.makedirs(subfolder_path)
    
    # 遍历所有需要的子文件夹并创建
    for subfolder in required_subfolders:
        _create_subfolder_if_not_exists(input_folder, subfolder)

```

```
**# 使用此指**令前，请确保安装**必要****的Pytho****n库，例**如使用以下命令安**装：
# p****ip **inst**all ****pandas** openpy**xl**
**
impo****rt pan**das **as p**d
impo**rt os
**
**from ****typ**ing **im**p**ort** *
try:
**    f**rom **xb**o**t.a**pp.loggi**ng im**port** t**r**ace**** a**s** pr**int
exce**pt:
 **   f**rom** xbot im**por**t print

def** extra**ct_produ**ct_inf**o_from_exce**l(**folder_path,** file**_nam**e, **product_**nam****e_c**olumn**, ****li**nk_co**lum**n**):**
    &quot;&quot;&quot;
   ** t****itle:**** 提取Exc**el中商品信息
    **des**cription: 读取**Ex****ce****l文件****中除了**&quot;分类&quot;之外的所有she**et，提取每**个sheet中的** % pro**duct_name_c**ol**umn % 列和 % l**ink_c**olumn % 列数据，**商品名称格式****为&quot;s**heet名称_**商品**名称&quot;。
    inp**ut**s:
 **  ** **   ** - folde**r_pat**h (f**old**er): Excel文件**所在**文件夹路**径，**e**g: **&quot;C:/data**/&quot;
  **    **  - f**ile_**name (**str): Excel文件名，eg: &quot;products.xlsx&quot;
        - product_name_column (str): 商品名称列名，eg: &quot;商品名称&quot;
        - link_column (str): 联盟推广链接列名，eg: &quot;联盟推广链接&quot;
    outputs:
        - product_data (list): 包含商品名称和联盟推广链接的字典列表，eg: &quot;[{&#x27;商品名称&#x27;: &#x27;sheet1_苹果&#x27;, &#x27;联盟推广链接&#x27;: &#x27;http://example.com&#x27;}]&quot;
    &quot;&quot;&quot;
    
    # 拼接完整的Excel文件路径
    excel_path = os.path.join(folder_path, file_name)
    
    # 检查文件夹是否存在
    if not os.path.exists(folder_path):
        raise FileNotFoundError(f&quot;文件夹不存在: {folder_path}&quot;)
    
    # 检查Excel文件是否存在
    if not os.path.exists(excel_path):
        raise FileNotFoundError(f&quot;Excel文件不存在: {excel_path}&quot;)
    
    # 检查列名参数是否为空
    if not product_name_column or not link_column:
        raise ValueError(&quot;商品名称列名和链接列名不能为空&quot;)
    
    def _read_excel_sheets(excel_path):
        &quot;&quot;&quot;
        读取Excel文件的所有sheet名称
        &quot;&quot;&quot;
        try:
            excel_file = pd.ExcelFile(excel_path)
            return excel_file.sheet_names
        except Exception as e:
            raise Exception(f&quot;读取Excel文件失败: {str(e)}&quot;)
    
    def _extract_product_data_from_sheet(excel_path, sheet_name, product_col, link_col):
        &quot;&quot;&quot;
        从指定sheet中提取商品名称和联盟推广链接
        &quot;&quot;&quot;
        try:
            df = pd.read_excel(excel_path, sheet_name=sheet_name)
            
            # 检查必需的列是否存在
            required_columns = [product_col, link_col]
            missing_columns = [col for col in required_columns if col not in df.columns]
            
            if missing_columns:
                print(f&quot;警告: sheet &#x27;{sheet_name}&#x27; 中缺少列: {missing_columns}&quot;)
                return []
            
            # 提取数据，过滤掉空值
            sheet_data = []
            for index, row in df.iterrows():
                if pd.notna(row[product_col]) and pd.notna(row[link_col]):
                    # 将商品名称格式化为&quot;sheet名称_商品名称&quot;
                    formatted_product_name = f&quot;{sheet_name}_{str(row[product_col]).strip()}&quot;
                    sheet_data.append({
                        &#x27;商品名称&#x27;: formatted_product_name,
                        &#x27;联盟推广链接&#x27;: str(row[link_col]).strip()
                    })
            
            return sheet_data
            
        except Exception as e:
            print(f&quot;警告: 读取sheet &#x27;{sheet_name}&#x27; 时出错: {str(e)}&quot;)
            return []
    
    # 获取所有sheet名称
    all_sheets = _read_excel_sheets(excel_path)
    
    # 过滤掉&quot;分类&quot;sheet
    target_sheets = [sheet for sheet in all_sheets if sheet != &quot;分类&quot;]
    
    if not target_sheets:
        raise ValueError(&quot;除了&#x27;分类&#x27;外没有找到其他sheet&quot;)
    
    # 提取所有目标sheet的商品数据
    all_product_data = []
    for sheet_name in target_sheets:
        sheet_data = _extract_product_data_from_sheet(excel_path, sheet_name, product_name_column, link_column)
        all_product_data.extend(sheet_data)
    
    if not all_product_data:
        raise ValueError(&quot;未找到任何有效的商品数据&quot;)
    
    print(f&quot;成功提取了 {len(all_product_data)} 条商品信息，来自 {len(target_sheets)} 个sheet&quot;)
    
    return all_product_data

```

```
**import**** os
im****port** re

fro**m typi****ng **impo**rt *****
try:
**    fro**m **x**bot.a****pp.log**ging** imp**ort tr**ace as** **print****
ex**cept**:
** **   **from xbo**t imp**ort **pr**i**nt
**
def fil**ter_p**rodu**ct**s**_by**_**zip_files(**folder_path, pr**oduc**t_list):**
    **&quot;&quot;&quot;
**   ** title: **根据Z**IP文件过滤商品列表并清**理禁用**字符
    des**cr****ip****tio**n: 查询指定文**件夹 % f**older_path %** 下的ZI**P文件名，从商品**字典列表 %** product_li**st** % 中删除商品名称包含**这些ZIP**文件名的字典，然**后清理剩余商**品名称中**的Wi**ndows文件名**禁用字**符，返回处理后的商**品列**表 % filtered**_l**i**st **%**。
    inpu**ts:
      **  - **fo**ld**er_path **(fo**l**de****r): 要****查询的文**件夹路径，eg: &quot;C:**/Us**ers/Docume**nt****s&quot;**
        -** pr**oduct_list** (****list)****: 包含商**品名称和链接的字典列表，**eg**: [{&#x27;商品名**称&#x27;: &#x27;学**生党显示**器_S**ANC盛色23.**8英寸**&#x27;, &#x27;联盟推广链**接&#x27;**: &#x27;https**://u.j**d.co**m/0rux**MM2&#x27;}]
    outputs:
        - filtered_list (list): 过滤并清理后的商品字典列表，eg: [{&#x27;商品名称&#x27;: &#x27;其他商品_清理后名称&#x27;, &#x27;联盟推广链接&#x27;: &#x27;https://example.com&#x27;}]
    &quot;&quot;&quot;
    
    # 检查文件夹路径是否存在
    if not os.path.exists(folder_path):
        raise ValueError(f&quot;文件夹路径不存在: {folder_path}&quot;)
    
    if not os.path.isdir(folder_path):
        raise ValueError(f&quot;指定路径不是文件夹: {folder_path}&quot;)
    
    if not isinstance(product_list, list):
        raise ValueError(&quot;product_list 必须是列表类型&quot;)
    
    def _get_zip_files_without_extension(folder_path):
        &quot;&quot;&quot;
        获取文件夹中所有ZIP文件名（不含后缀）
        &quot;&quot;&quot;
        zip_files = []
        try:
            for filename in os.listdir(folder_path):
                file_path = os.path.join(folder_path, filename)
                if os.path.isfile(file_path) and filename.lower().endswith(&#x27;.zip&#x27;):
                    name_without_extension = os.path.splitext(filename)[0]
                    zip_files.append(name_without_extension)
        except PermissionError:
            raise PermissionError(f&quot;没有权限访问文件夹: {folder_path}&quot;)
        except Exception as e:
            raise Exception(f&quot;遍历文件夹时发生错误: {str(e)}&quot;)
        
        return zip_files
    
    def _filter_products_by_zip_names(zip_files, product_list):
        &quot;&quot;&quot;
        根据ZIP文件名过滤商品列表
        &quot;&quot;&quot;
        filtered_list = []
        
        for product in product_list:
            if not isinstance(product, dict) or &#x27;商品名称&#x27; not in product:
                raise ValueError(&quot;商品字典必须包含&#x27;商品名称&#x27;键&quot;)
            
            product_name = product[&#x27;商品名称&#x27;]
            should_keep = True
            
            # 检查商品名称是否包含任何ZIP文件名
            for zip_name in zip_files:
                if zip_name and zip_name in product_name:
                    should_keep = False
                    break
            
            # 如果商品名称不包含任何ZIP文件名，则保留该商品
            if should_keep:
                filtered_list.append(product)
        
        return filtered_list
    
    def _clean_invalid_characters(product_list):
        &quot;&quot;&quot;
        清理商品名称中的Windows文件名禁用字符
        &quot;&quot;&quot;
        # Windows文件名禁用字符: / \ : * ? &quot; &lt; &gt; |
        invalid_chars = r&#x27;[/\\:*?&quot;&lt;&gt;|]&#x27;
        cleaned_list = []
        
        for product in product_list:
            # 创建产品副本以避免修改原始数据
            cleaned_product = product.copy()
            
            # 清理商品名称中的禁用字符，替换为下划线
            original_name = cleaned_product[&#x27;商品名称&#x27;]
            cleaned_name = re.sub(invalid_chars, &#x27;_&#x27;, original_name)
            
            # 去除多余的下划线（连续的下划线替换为单个）
            cleaned_name = re.sub(&#x27;_+&#x27;, &#x27;_&#x27;, cleaned_name)
            
            # 去除首尾的下划线
            cleaned_name = cleaned_name.strip(&#x27;_&#x27;)
            
            cleaned_product[&#x27;商品名称&#x27;] = cleaned_name
            cleaned_list.append(cleaned_product)
        
        return cleaned_list
    
    # 获取ZIP文件名列表
    zip_files = _get_zip_files_without_extension(folder_path)
    
    # 根据ZIP文件名过滤商品列表
    filtered_list = _filter_products_by_zip_names(zip_files, product_list)
    
    # 清理商品名称中的禁用字符
    cleaned_list = _clean_invalid_characters(filtered_list)
    
    return cleaned_list

```

```
**# 使用此指****令前，请确保****安装必要的P****ython库****，例如使用以****下命令**安装：
**# pip ****instal**l py7zr rarfil**e

i****mpo**rt o**s
impo****rt zip**file
import **tarf****ile
**import g**zip
im****por**t sh**util****
try:
**    imp**or**t** rarf****ile
ex**cept** Imp**ortErr**or:
  ** ** rarf****ile** = N**on**e**
tr**y:
    i**mport** py7**zr**
**exc**ept Impo**rtErr**or:
**  ** ** py**7zr = No**ne

f**rom **ty**p**ing** import ***
try**:
  **  f**rom xbot**.ap**p.logging **im**port trace a**s ****pri**nt
ex**ce****pt****:
   ** from xb**ot imp**ort **pri**nt

def **ext**ract_all_arc**hi****ves_**in_folder(parent**_f**older_path, **sub_****fo**lder_name**):**
**    ****&quot;&quot;&quot;
 ****   t**itle: 批量解压子文件夹中的**压缩**包
    descri**ptio****n:** 在父文件夹 **`p**a**rent****_fold****er_p**ath` 下找到子文件夹 `sub_folder**_n**ame`，将其中的所有压**缩包解压****到各自的**子文件夹中，并删除**原压缩包****。
**    inputs:
**    **    - parent_folde**r_p**a**th ****(fol**der): **父文件夹****路径****，eg:** **&quot;C:/****Us**ers/Desktop&quot;**
   ****     **- sub_fo**lder_n**ame (str): **子文**件夹名称，eg: &quot;Do**wnloa**ds&quot;
**  ** ** ou**tputs:
 **      ** - r**esu**lt (str): 处理结果**信息**，eg: &quot;成功**解压3**个压缩包&quot;
    &quot;&quot;**&quot;
**    
    if **not o**s.path.exists(parent_folder_p**ath)**:
      **  rais**e ValueErro**r(**f&quot;父文件夹路径不存在:** {parent**_fol**de**r_pa**th}&quot;)
**    
    if not os.path.isdir(parent_folder_path):
        raise ValueError(f&quot;父路径不是文件夹: {parent_folder_path}&quot;)
    
    # 拼接完整的子文件夹路径
    target_folder_path = os.path.join(parent_folder_path, sub_folder_name)
    
    if not os.path.exists(target_folder_path):
        raise ValueError(f&quot;子文件夹不存在: {target_folder_path}&quot;)
    
    if not os.path.isdir(target_folder_path):
        raise ValueError(f&quot;子路径不是文件夹: {target_folder_path}&quot;)
    
    def _get_archive_files(folder_path):
        &quot;&quot;&quot;获取文件夹中的所有压缩包文件&quot;&quot;&quot;
        archive_extensions = [&#x27;.zip&#x27;, &#x27;.rar&#x27;, &#x27;.7z&#x27;, &#x27;.tar&#x27;, &#x27;.tar.gz&#x27;, &#x27;.tgz&#x27;, &#x27;.gz&#x27;]
        archive_files = []
        
        for filename in os.listdir(folder_path):
            file_path = os.path.join(folder_path, filename)
            if os.path.isfile(file_path):
                for ext in archive_extensions:
                    if filename.lower().endswith(ext):
                        archive_files.append(file_path)
                        break
        
        return archive_files
    
    def _extract_archive(archive_path, extract_folder):
        &quot;&quot;&quot;根据文件类型解压压缩包&quot;&quot;&quot;
        filename = os.path.basename(archive_path)
        
        try:
            if archive_path.lower().endswith(&#x27;.zip&#x27;):
                with zipfile.ZipFile(archive_path, &#x27;r&#x27;) as zip_ref:
                    zip_ref.extractall(extract_folder)
            
            elif archive_path.lower().endswith(&#x27;.rar&#x27;):
                if rarfile is None:
                    raise ImportError(&quot;需要安装rarfile库来处理RAR文件&quot;)
                with rarfile.RarFile(archive_path, &#x27;r&#x27;) as rar_ref:
                    rar_ref.extractall(extract_folder)
            
            elif archive_path.lower().endswith(&#x27;.7z&#x27;):
                if py7zr is None:
                    raise ImportError(&quot;需要安装py7zr库来处理7Z文件&quot;)
                with py7zr.SevenZipFile(archive_path, mode=&#x27;r&#x27;) as z:
                    z.extractall(extract_folder)
            
            elif archive_path.lower().endswith((&#x27;.tar&#x27;, &#x27;.tar.gz&#x27;, &#x27;.tgz&#x27;)):
                with tarfile.open(archive_path, &#x27;r:*&#x27;) as tar_ref:
                    tar_ref.extractall(extract_folder)
            
            elif archive_path.lower().endswith(&#x27;.gz&#x27;) and not archive_path.lower().endswith(&#x27;.tar.gz&#x27;):
                # 处理单个.gz文件
                with gzip.open(archive_path, &#x27;rb&#x27;) as f_in:
                    output_filename = os.path.splitext(filename)[0]
                    output_path = os.path.join(extract_folder, output_filename)
                    with open(output_path, &#x27;wb&#x27;) as f_out:
                        shutil.copyfileobj(f_in, f_out)
            
            else:
                raise ValueError(f&quot;不支持的压缩格式: {filename}&quot;)
                
        except Exception as e:
            raise Exception(f&quot;解压文件 {filename} 时出错: {str(e)}&quot;)
    
    # 获取目标文件夹中的所有压缩包文件
    archive_files = _get_archive_files(target_folder_path)
    
    if not archive_files:
        return f&quot;子文件夹 &#x27;{sub_folder_name}&#x27; 中没有找到压缩包文件&quot;
    
    extracted_count = 0
    failed_files = []
    
    # 逐个解压压缩包
    for archive_path in archive_files:
        try:
            filename = os.path.basename(archive_path)
            # 创建解压目录，去掉文件扩展名
            archive_name = os.path.splitext(filename)[0]
            # 处理.tar.gz等双重扩展名
            if archive_name.endswith(&#x27;.tar&#x27;):
                archive_name = os.path.splitext(archive_name)[0]
            
            extract_folder = os.path.join(target_folder_path, archive_name)
            
            # 如果目录已存在，添加数字后缀
            counter = 1
            original_extract_folder = extract_folder
            while os.path.exists(extract_folder):
                extract_folder = f&quot;{original_extract_folder}_{counter}&quot;
                counter += 1
            
            # 创建解压目录
            os.makedirs(extract_folder, exist_ok=True)
            
            # 解压文件
            _extract_archive(archive_path, extract_folder)
            
            # 删除原压缩包
            os.remove(archive_path)
            
            extracted_count += 1
            
        except Exception as e:
            failed_files.append(f&quot;{filename}: {str(e)}&quot;)
            continue
    
    # 生成结果信息
    result_msg = f&quot;在子文件夹 &#x27;{sub_folder_name}&#x27; 中成功解压{extracted_count}个压缩包&quot;
    if failed_files:
        result_msg += f&quot;，失败{len(failed_files)}个文件:\n&quot; + &quot;\n&quot;.join(failed_files)
    
    return result_msg

```

```
**import**** os
im****port** shutil
**
from ****typ**ing **impo****rt *
t**ry:
   ** f**r**om xb****ot.app**.log**ging** impor**t trac**e** as p****rin**t
ex**ce**p**t:
**    from** xbot** import print

def m**ove_**vide**os_**from_sub**folder**s(source_folder, ta**rg**et_f**old**er):
   ** &quot;&quot;&quot;
**    titl**e: 剪切子**文件夹中**的视频**到新文件夹
**  **  descri**pt**ion: 遍历指定文件夹** `s**o**urce**_**fo**l**der`**** 下****的所有****子文件夹****，****将每个子****文件夹****中的视频文****件剪切到 `**target_fold**er****` 中，并**将视频文**件重命名为其**所在子文件夹的名字。
    inputs:
        - source_folder (folder): 源文件夹路径，eg: &quot;C:/Videos&quot;
        - target_folder (folder): 目标文件夹路径，eg: &quot;C:/CollectedVideos&quot;
    outputs:
        - result (str): 操作结果统计信息，eg: &quot;成功移动了5个视频文件&quot;
    &quot;&quot;&quot;
    
    # 检查源文件夹是否存在
    if not os.path.exists(source_folder):
        raise ValueError(f&quot;源文件夹不存在: {source_folder}&quot;)
    
    # 创建目标文件夹（如果不存在）
    os.makedirs(target_folder, exist_ok=True)
    
    def _is_video_file(filename):
        &quot;&quot;&quot;判断文件是否为视频文件&quot;&quot;&quot;
        video_extensions = {&#x27;.mp4&#x27;, &#x27;.avi&#x27;, &#x27;.mov&#x27;, &#x27;.wmv&#x27;, &#x27;.flv&#x27;, &#x27;.mkv&#x27;, &#x27;.webm&#x27;, &#x27;.m4v&#x27;, &#x27;.3gp&#x27;, &#x27;.f4v&#x27;}
        return os.path.splitext(filename.lower())[1] in video_extensions
    
    def _get_unique_filename(target_path, base_name, extension):
        &quot;&quot;&quot;生成唯一的文件名，避免冲突&quot;&quot;&quot;
        counter = 1
        new_name = f&quot;{base_name}{extension}&quot;
        while os.path.exists(os.path.join(target_path, new_name)):
            new_name = f&quot;{base_name}_{counter}{extension}&quot;
            counter += 1
        return new_name
    
    moved_count = 0
    
    # 遍历源文件夹下的所有项目
    for item in os.listdir(source_folder):
        item_path = os.path.join(source_folder, item)
        
        # 检查是否为子文件夹
        if os.path.isdir(item_path):
            subfolder_name = item
            
            # 遍历子文件夹中的文件
            for file in os.listdir(item_path):
                file_path = os.path.join(item_path, file)
                
                # 检查是否为视频文件
                if os.path.isfile(file_path) and _is_video_file(file):
                    # 获取文件扩展名
                    _, extension = os.path.splitext(file)
                    
                    # 生成新的文件名（使用子文件夹名字）
                    new_filename = _get_unique_filename(target_folder, subfolder_name, extension)
                    target_file_path = os.path.join(target_folder, new_filename)
                    
                    # 移动文件
                    try:
                        shutil.move(file_path, target_file_path)
                        moved_count += 1
                        print(f&quot;已移动: {file} -&gt; {new_filename}&quot;)
                    except Exception as e:
                        print(f&quot;移动文件失败 {file}: {str(e)}&quot;)
    
    result = f&quot;成功移动了{moved_count}个视频文件&quot;
    return result

```

```
**import**** os
**
from ty**ping i****mpo**rt ***
try****:
    **from xb**ot**.**app.l****ogging** imp**ort **trace **as pri**n**t
exc****ept**:
  **  **f**rom** xbot im**port **prin**t
**
**def** find_de**tail_**and_**mai**n_image_**pat**hs_in_subfol**der**s(fold**er****_p**ath):
  **  &quot;&quot;&quot;
**    title: 查**找子文件夹**中包含&quot;详情&quot;和**&quot;主图&quot;的路**径
  **  d**escripti**on:** 读取指定文件夹 % f**old**er_pat**h ****% **下的所有子文**件夹**，然后在每个**子文**件夹中查找名**字包**含&quot;详情&quot;和&quot;主**图&quot;的文件或**文件夹的绝对路径，输出子**文件夹名字**和对应的路径列表**。
    **inpu**ts**:**
  **      - **folde**r_path (**folder**): 指**定的文**件夹路径，eg: &quot;C**:/**Users/De**sk**top/主文件夹&quot;
  **  out**puts:
      **  -** resul**t ****(dict**): 包含子文件**夹名字和**对应详情、主图路径的字典**，eg: **&quot;{&#x27;子**文件夹1&#x27;:** [&#x27;C:/Users/Desktop/主文件夹/子文件夹1/产品详情.txt&#x27;, &#x27;C:/Users/Desktop/主文件夹/子文件夹1/主图.jpg&#x27;], &#x27;子文件夹2&#x27;: []}&quot;
    &quot;&quot;&quot;
    
    # 检查输入路径是否存在且为文件夹
    if not os.path.exists(folder_path):
        raise ValueError(f&quot;指定路径不存在: {folder_path}&quot;)
    
    if not os.path.isdir(folder_path):
        raise ValueError(f&quot;指定路径不是文件夹: {folder_path}&quot;)
    
    def _get_subfolders(path):
        &quot;&quot;&quot;获取指定路径下的所有子文件夹&quot;&quot;&quot;
        subfolders = []
        try:
            for item in os.listdir(path):
                item_path = os.path.join(path, item)
                if os.path.isdir(item_path):
                    subfolders.append(item)
        except PermissionError:
            print(f&quot;权限不足，无法访问: {path}&quot;)
        return subfolders
    
    def _find_target_items(subfolder_path):
        &quot;&quot;&quot;在指定文件夹中查找名字包含&#x27;详情&#x27;或&#x27;主图&#x27;的文件或文件夹&quot;&quot;&quot;
        target_paths = []
        try:
            for item in os.listdir(subfolder_path):
                if &quot;详情&quot; in item or &quot;主图&quot; in item:
                    target_paths.append(os.path.abspath(os.path.join(subfolder_path, item)))
        except PermissionError:
            print(f&quot;权限不足，无法访问: {subfolder_path}&quot;)
        return target_paths
    
    # 获取所有子文件夹
    subfolders = _get_subfolders(folder_path)
    
    if not subfolders:
        print(f&quot;在路径 {folder_path} 下未找到任何子文件夹&quot;)
        return {}
    
    # 处理每个子文件夹
    result = {}
    for subfolder in subfolders:
        subfolder_path = os.path.join(folder_path, subfolder)
        target_paths = _find_target_items(subfolder_path)
        result[subfolder] = target_paths
        
        # 打印结果
        if target_paths:
            print(f&quot;子文件夹 &#x27;{subfolder}&#x27; 中包含&#x27;详情&#x27;或&#x27;主图&#x27;的路径:&quot;)
            for path in target_paths:
                print(f&quot;  - {path}&quot;)
        else:
            print(f&quot;子文件夹 &#x27;{subfolder}&#x27; 中未找到包含&#x27;详情&#x27;或&#x27;主图&#x27;的文件或文件夹&quot;)
    
    return result

```

```
import os

from typing import *
try:
    from xbot.app.logging import trace as print
except:
    from xbot import print


def write_text_to_file(text_content, parent_folder_path, sub_folder_path, file_name):
    &quot;&quot;&quot;
    title: 将文本内容写入txt文件
    description: 将获取到的文本内容 % text_content % 写入到父路径 % parent_folder_path % 和子路径 % sub_folder_path % 组成的文件夹下的 % file_name % 文件中，自动添加.txt后缀。
    inputs:
        - text_content (str): 要写入的文本内容，eg: &quot;这是要写入文件的文本内容&quot;
        - parent_folder_path (folder): 父文件夹路径，eg: &quot;C:/Documents&quot;
        - sub_folder_path (str): 子文件夹路径，eg: &quot;MyProject/TextFiles&quot;
        - file_name (str): 文件名（不含扩展名），eg: &quot;output&quot;
    outputs: None
    &quot;&quot;&quot;
    
    # 检查输入有效性
    if not isinstance(text_content, str):
        raise TypeError(&quot;文本内容必须为字符串类型&quot;)
    
    if not parent_folder_path:
        raise ValueError(&quot;父文件夹路径不能为空&quot;)
    
    if not sub_folder_path:
        raise ValueError(&quot;子文件夹路径不能为空&quot;)
    
    if not file_name:
        raise ValueError(&quot;文件名不能为空&quot;)
    
    def _ensure_folder_exists(folder_path):
        &quot;&quot;&quot;
        确保文件夹存在，如果不存在则创建
        &quot;&quot;&quot;
        if not os.path.exists(folder_path):
            os.makedirs(folder_path)
    
    def _write_content_to_file(content, parent_path, sub_path, filename):
        &quot;&quot;&quot;
        将内容写入到指定文件
        &quot;&quot;&quot;
        try:
            # 组合完整文件夹路径
            full_folder_path = os.path.join(parent_path, sub_path)
            
            # 确保文件夹存在
            _ensure_folder_exists(full_folder_path)
            
            # 自动添加.txt后缀
            full_filename = f&quot;{filename}.txt&quot;
            
            # 组合完整文件路径
            full_path = os.path.join(full_folder_path, full_filename)
            
            with open(full_path, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as file:
                file.write(content)
        except IOError as e:
            raise IOError(f&quot;文件写入失败: {str(e)}&quot;)
        except Exception as e:
            raise Exception(f&quot;写入过程中发生错误: {str(e)}&quot;)
    
    # 执行文件写入操作
    _write_content_to_file(text_content, parent_folder_path, sub_folder_path, file_name)

```

```
import os

from typing import *
try:
    from xbot.app.logging import trace as print
except:
    from xbot import print


def merge_txt_files_from_subfolders(parent_folder, subfolder_name, output_folder, output_filename):
    &quot;&quot;&quot;
    title: 合并指定子文件夹中的txt文件
    description: 在 % parent_folder % 下找到 % subfolder_name % 子文件夹，将其中的txt文件内容合并写入到 % output_folder % 路径下的 % output_filename % 文件中。
    inputs:
        - parent_folder (folder): 父文件夹路径，eg: &quot;C:/data/parent_folder&quot;
        - subfolder_name (str): 子文件夹名称，eg: &quot;images&quot;
        - output_folder (folder): 输出文件夹路径，eg: &quot;C:/output&quot;
        - output_filename (str): 输出文件名，eg: &quot;merged_texts&quot;
    outputs:
        - result (str): 处理结果信息，eg: &quot;成功合并了3个txt文件&quot;
    &quot;&quot;&quot;
    
    # 检查父文件夹是否存在
    if not os.path.exists(parent_folder):
        raise FileNotFoundError(f&quot;父文件夹不存在: {parent_folder}&quot;)
    
    if not os.path.isdir(parent_folder):
        raise ValueError(f&quot;父路径不是文件夹: {parent_folder}&quot;)
    
    # 构建完整的输入路径和输出路径
    input_folder = os.path.join(parent_folder, subfolder_name)
    if not os.path.exists(input_folder):
        raise FileNotFoundError(f&quot;子文件夹不存在: {input_folder}&quot;)
    
    # 确保输出文件夹存在
    if not os.path.exists(output_folder):
        os.makedirs(output_folder)
    
    # 自动添加.txt后缀
    if not output_filename.lower().endswith(&#x27;.txt&#x27;):
        output_filename += &#x27;.txt&#x27;
    
    output_file = os.path.join(output_folder, output_filename)
    
    def _find_and_merge_txt_files(input_folder, output_file):
        &quot;&quot;&quot;
        查找并合并txt文件
        &quot;&quot;&quot;
        merged_count = 0
        
        with open(output_file, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as output_f:
            # 遍历输入文件夹下的所有子文件夹
            for root, dirs, files in os.walk(input_folder):
                # 查找当前文件夹中的txt文件
                txt_files = [f for f in files if f.lower().endswith(&#x27;.txt&#x27;)]
                
                for txt_file in txt_files:
                    txt_path = os.path.join(root, txt_file)
                    
                    # 获取相对路径作为标识
                    relative_path = os.path.relpath(txt_path, input_folder)
                    folder_name = os.path.basename(root) if root != input_folder else &quot;根目录&quot;
                    file_name_without_ext = os.path.splitext(txt_file)[0]
                    
                    try:
                        with open(txt_path, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as txt_f:
                            content = txt_f.read()
                        
                        # 写入标识和内容
                        output_f.write(f&quot;=== {folder_name} - {file_name_without_ext} ===\n&quot;)
                        output_f.write(f&quot;文件夹路径: {os.path.dirname(relative_path) if os.path.dirname(relative_path) else &#x27;根目录&#x27;}\n&quot;)
                        output_f.write(f&quot;文件路径: {relative_path}\n&quot;)
                        output_f.write(&quot;-&quot; * 50 + &quot;\n&quot;)
                        output_f.write(content)
                        output_f.write(&quot;\n&quot; + &quot;=&quot; * 60 + &quot;\n\n&quot;)
                        
                        merged_count += 1
                    
                    except Exception as e:
                        output_f.write(f&quot;=== 读取失败: {relative_path} ===\n&quot;)
                        output_f.write(f&quot;错误信息: {str(e)}\n&quot;)
                        output_f.write(&quot;=&quot; * 60 + &quot;\n\n&quot;)
        
        return merged_count
    
    merged_count = _find_and_merge_txt_files(input_folder, output_file)
    
    if merged_count == 0:
        return f&quot;在 {subfolder_name} 文件夹中未找到任何txt文件&quot;
    else:
        return f&quot;成功合并了{merged_count}个txt文件到 {output_file}&quot;

```

```
**import**** os
im****port** re
**from o****s im**port list**dir, m****akedir**s
from o**s.****path** import isfi**le, jo****in, **splite**xt
imp**o**rt pa****nd**a**s a**s pd**
from**** fu**zzyw**uzzy i****mpo**rt f**uzz**  # 需要安装: pi**p **install **fu****zzy**w**uz**zy
from xbot** impo**rt print
# -**--------**--------**---**------** 请****修改以**下配置项 -----------------**---**------
s**ou**rce_folder =** 详情图t**xt文件夹  #** 原TX**T文件存放文件夹
out**put_f****older **= 参数合并文件夹  **  **# 合并**文件保存文****件夹**
**
tx**t_pe**r_bat****ch **= 2**0 ****  ****   ****  ** **   **    **   # ****每个合**并文件包含的TXT数**量
**exce**l_**file_**pa**th =** Exc**el文件**路径** # Ex**ce**l文件路**径
pr**oduc**t_**name_col = &quot;**商品名**称**&quot; ****   ****  **  # **Ex**cel中商品名称所在列的**列名
**#** -**--------**-----**-**---**---**--**--------**--------**----**---**---------------**--****----**------

**# 确**保输出文件夹存在
if n**ot** **os.path.e**xists(output**_f****old**er):
   ** m**akedirs(outp**ut**_**fol**d**er)
**    prin**t(f&quot;**已**自动创**建输出文件**夹：****{ou****tp**ut_f**ol**d**er}**&quot;)

def **clean_fi****len**ame(**nam**e):
    &quot;&quot;&quot;**清理**文**件名中的特**殊字符，用于匹配&quot;&quot;&quot;
  **  #**** 移除**文件名中不允许的**字符
 ** **  cl**eaned = re.sub(r&#x27;[\\/***?**:&quot;&lt;&gt;|]&#x27;, &#x27;&#x27;,** na**me)
    # **统一****转为小****写并去除**空**格，提高**匹配率
    return cleaned.lowe**r(****).repl****ace****(&quot; &quot;**,** &quot;&quot;)**

# 1. 读取Excel文件中的所有sheet**和商****品名称
ex**cel_sheets **= ****{}
tr****y:
    #**** 获取Exc**el中所有的sheet**名称****
    ****excel_sh**eet_name**s = p****d.E****xcelF****ile(e****xcel_****file_****path)****.shee****t_nam****es
****  **  
 **  **** for** sheet_n**ame i**n excel_**she**et_names:
  **  ** **   # 读取每个**sheet的数据
   **     **df = pd.**re**a**d_e**xcel(excel_f**ile_p****ath****, **shee**t_****name**=sheet_n**ame)
**        **
  **    **  # **检**查商**品**名称列是否存在
 **       if pr**oduct**_name_co**l **n**ot **in df.column**s:
  ****  ** **   **    **print****(f&quot;警**告：Sh**eet &#x27;****{shee**t_name}&#x27; 中未找到 &#x27;{product_name_col}&#x27; 列，已跳过该sheet&quot;)
            continue
            
        # 提取商品名称并清理，同时保留原始名称用于报告
        product_names = []
        for name in df[product_name_col].dropna():  # 排除空值
            product_names.append({
                &quot;original&quot;: str(name),
                &quot;cleaned&quot;: clean_filename(str(name))
            })
        
        if product_names:
            excel_sheets[sheet_name] = product_names
            print(f&quot;已读取Sheet &#x27;{sheet_name}&#x27;，包含 {len(product_names)} 个商品&quot;)
        else:
            print(f&quot;警告：Sheet &#x27;{sheet_name}&#x27; 中未找到有效商品名称，已跳过该sheet&quot;)
            
except Exception as e:
    print(f&quot;错误：读取Excel文件失败 - {str(e)}&quot;)
    exit()

if not excel_sheets:
    print(&quot;错误：未从Excel文件中读取到任何有效数据，程序退出&quot;)
    exit()

# 2. 读取并整理TXT文件
txt_files = {}
all_txt_files = [f for f in listdir(source_folder) if isfile(join(source_folder, f)) and f.endswith(&quot;.txt&quot;)]

if not all_txt_files:
    print(&quot;错误：原文件夹内未找到任何TXT文件，请检查路径！&quot;)
    exit()

# 按sheet分组TXT文件
for txt_file in all_txt_files:
    # 提取sheet名称（按第一个下划线分割）
    if &quot;_&quot; in txt_file:
        sheet_name = txt_file.split(&quot;_&quot;)[0]
    else:
        sheet_name = &quot;未命名sheet&quot;
    
    # 提取商品名称（去除sheet名前缀和.txt后缀）
    if &quot;_&quot; in txt_file:
        product_part = &quot;_&quot;.join(txt_file.split(&quot;_&quot;)[1:])
    else:
        product_part = txt_file
    product_name = splitext(product_part)[0]
    
    # 初始化sheet组
    if sheet_name not in txt_files:
        txt_files[sheet_name] = []
    
    # 存储TXT文件信息
    txt_files[sheet_name].append({
        &quot;filename&quot;: txt_file,
        &quot;original_product_name&quot;: product_name,
        &quot;cleaned_product_name&quot;: clean_filename(product_name),
        &quot;full_path&quot;: join(source_folder, txt_file)
    })

# 3. 按Excel中的顺序匹配并排序TXT文件
matched_files = {}  # 存储匹配成功的文件
unmatched_excel_products = {}  # 存储Excel中未匹配到的商品
unmatched_txt_files = {}  # 存储未匹配到Excel的TXT文件

for sheet_name, excel_products in excel_sheets.items():
    # 初始化当前sheet的存储结构
    matched_files[sheet_name] = []
    unmatched_excel_products[sheet_name] = []
    
    # 检查是否有对应的TXT文件
    if sheet_name not in txt_files:
        print(f&quot;警告：Sheet &#x27;{sheet_name}&#x27; 在TXT文件夹中没有对应的文件&quot;)
        unmatched_excel_products[sheet_name] = [p[&quot;original&quot;] for p in excel_products]
        continue
    
    # 复制一份TXT文件列表用于匹配（避免重复匹配）
    available_txts = txt_files[sheet_name].copy()
    
    # 遍历Excel中的商品，寻找最佳匹配
    for excel_product in excel_products:
        best_match = None
        best_score = 0
        
        # 寻找最佳匹配
        for i, txt_file in enumerate(available_txts):
            # 使用模糊匹配算法计算相似度
            score = fuzz.ratio(excel_product[&quot;cleaned&quot;], txt_file[&quot;cleaned_product_name&quot;])
            
            # 找到更优的匹配
            if score &gt; best_score and score &gt;= 60:  # 相似度阈值，可调整
                best_score = score
                best_match = i
        
        # 处理匹配结果
        if best_match is not None:
            matched_files[sheet_name].append(available_txts.pop(best_match))
        else:
            unmatched_excel_products[sheet_name].append(excel_product[&quot;original&quot;])
    
    # 记录未匹配的TXT文件
    unmatched_txt_files[sheet_name] = [txt[&quot;filename&quot;] for txt in available_txts]

# 4. 合并文件
total_merged_files = 0
total_merged_products = 0

for sheet_name, sorted_txts in matched_files.items():
    if not sorted_txts:
        continue
        
    # 计算需要分几批
    batch_count = (len(sorted_txts) + txt_per_batch - 1) // txt_per_batch
    
    for batch_idx in range(batch_count):
        # 计算当前批的范围
        start = batch_idx * txt_per_batch
        end = min(start + txt_per_batch, len(sorted_txts))
        current_batch = sorted_txts[start:end]
        
        # 生成合并文件名
        product_start = start + 1
        product_end = end
        merged_filename = f&quot;{sheet_name}_合并_{product_start}-{product_end}.txt&quot;
        merged_filepath = join(output_folder, merged_filename)
        
        # 合并文件内容
        with open(merged_filepath, &quot;w&quot;, encoding=&quot;utf-8&quot;) as merged_f:
            for txt_file in current_batch:
                try:
                    # 尝试用utf-8编码读取
                    with open(txt_file[&quot;full_path&quot;], &quot;r&quot;, encoding=&quot;utf-8&quot;) as txt_f:
                        txt_content = txt_f.read()
                except UnicodeDecodeError:
                    try:
                        # 尝试用gbk编码读取
                        with open(txt_file[&quot;full_path&quot;], &quot;r&quot;, encoding=&quot;gbk&quot;) as txt_f:
                            txt_content = txt_f.read()
                    except Exception as e:
                        print(f&quot;警告：文件{txt_file[&#x27;filename&#x27;]}编码异常，已跳过！错误：{e}&quot;)
                        continue
                except Exception as e:
                    print(f&quot;警告：读取文件{txt_file[&#x27;filename&#x27;]}失败，已跳过！错误：{e}&quot;)
                    continue
                
                # 写入带标识的内容
                merged_f.write(f&quot;【{txt_file[&#x27;original_product_name&#x27;]} 详情开始】\n&quot;)
                merged_f.write(txt_content.strip() + &quot;\n&quot;)
                merged_f.write(f&quot;【{txt_file[&#x27;original_product_name&#x27;]} 详情结束】\n\n&quot;)
        
        print(f&quot;✅ 已生成：{merged_filename}（包含{len(current_batch)}个TXT）&quot;)
        total_merged_files += 1
        total_merged_products += len(current_batch)

# 5. 输出结果报告
print(&quot;\n&quot; + &quot;=&quot;*50)
print(&quot;🎉 合并操作完成！&quot;)
print(f&quot;📁 合并文件保存路径：{output_folder}&quot;)
print(f&quot;📊 共处理 {len(matched_files)} 个sheet，生成 {total_merged_files} 个合并文件，合并 {total_merged_products} 个商品&quot;)

# 6. 输出未匹配项报告
has_unmatched = False

print(&quot;\n&quot; + &quot;=&quot;*50)
print(&quot;🔍 匹配状态报告：&quot;)

for sheet_name, products in unmatched_excel_products.items():
    if products:
        has_unmatched = True
        print(f&quot;\n⚠️ Sheet &#x27;{sheet_name}&#x27; 中有 {len(products)} 个Excel商品未找到匹配的TXT文件：&quot;)
        for i, product in enumerate(products[:5]):  # 只显示前5个
            print(f&quot;   - {product}&quot;)
        if len(products) &gt; 5:
            print(f&quot;   ... 还有 {len(products)-5} 个未显示&quot;)

for sheet_name, files in unmatched_txt_files.items():
    if files:
        has_unmatched = True
        print(f&quot;\n⚠️ Sheet &#x27;{sheet_name}&#x27; 中有 {len(files)} 个TXT文件未在Excel中找到匹配：&quot;)
        for i, file in enumerate(files[:5]):  # 只显示前5个
            print(f&quot;   - {file}&quot;)
        if len(files) &gt; 5:
            print(f&quot;   ... 还有 {len(files)-5} 个未显示&quot;)

if not has_unmatched:
    print(&quot;\n✅ 所有商品和TXT文件都已成功匹配！&quot;)
else:
    print(&quot;\n提示：请检查上述未匹配项，可能需要调整文件名或Excel中的商品名称以提高匹配率&quot;)
print(&quot;=&quot;*50)

```

```
**import**** os
**from x**bot im**p**ort p****ri****nt
**
# 指**定目标**文**件夹路径**
**fo**lder_path = 参数合并文件夹  # 请替换**为实际路**径

# 初始化**存储****txt**文**件路径的****列表
t**xt_p**aths ****= []
**
# 检查文件夹是否存在且有效
if os.path.exists(folder_path) and os.path.isdir(folder_path):
    # 只获取当前文件夹中的文件（不包含子文件夹）
    files = os.listdir(folder_path)
    for file in files:
        # 拼接完整路径
        file_path = os.path.join(folder_path, file)
        # 筛选出txt文件（不区分大小写）且是文件而不是文件夹
        if os.path.isfile(file_path) and file.lower().endswith(&quot;.txt&quot;):
            txt_paths.append(file_path)
else:
    print(f&quot;错误：&#x27;{folder_path}&#x27; 不是有效的文件夹路径&quot;)

# 输出结果
print(txt_paths)
```

```
**# 使用此指****令前，请确保****安装必要的P****ytho**n库，例如使用以**下命令安装：****
# **pip **inst****all op**enpyxl
**
i**m**port ****openpy**xl
i**mpor**t os
i**mport **a**st

f****rom** typ**in**g import *
try**:
** **   f**rom xbot**.app.**logg**in**g** import tr**ace as print
ex**cep**t:
    f**ro**m xbot import print**

d**ef add_colum**ns_****to**_**exc**e**l_sheets(f**older_path, fil**e_na**me, column_n**ames, **start_co**lumn**):
 **   &quot;**&quot;**&quot;
 ** **  title: 批**量新增Excel列名（跳过分类**shee**t）
    d**esc****ript**ion: 打开指**定文件夹中的**Excel文件，在除了名**为&quot;分类&quot;**的she**et之外的**所有sh**eet中新**增 % **co**l**umn**_names % 列表**中的**列**名。从**第** % start_c**olumn % 列开始检**查，如**果列名已存在则跳**过不添加。**直接修改**原文**件**。
 ** **  inputs:
**        - folde**r_p**at**h **(folder)**: Exc**el文件**所在的**文件夹路径，eg**: **&quot;**C:/**Documents&quot;
 **      ** - file_**name ****(st**r): Exce**l文件**名，eg:** &quot;**d**ata.x**lsx&quot;
       ** -** column_name**s **(**lis**t**): 要****新增的****列名****列表，eg****: [&quot;****姓名&quot;,** &quot;年龄&quot;, &quot;**部门&quot;]
**        **- s**tart_**co**l**umn (**int): 开始检查列名**的列**数，eg: &quot;3&quot;
  **  **o**utp**u**ts:
****   ****  ****   **- None
 **   &quot;&quot;**&quot;
    
 **   # **1. 检查输入有**效性和**类型转换
    i**f ****col**umn_names is** N**one:
        r**ais**e ValueError(&quot;**列名参**数**不能**为None&quot;)
    **
   **** # 自动**处理不同类型的输**入
   ** if isin**stanc**e(column**_na**mes, str):
  **  ** **   # 检查是否**是字符串形式的列表
  **     **** co**lumn_names_s**tripp**ed = column_**na**m**es.****str****ip()
**    **   ** if colu**mn**_**nam**es_stripped.**start**swith(&#x27;[**&#x27;) an**d column**_na**mes_stripped**.e**ndswith(&#x27;]&#x27;)**:
****     ****       t**ry:
    **     **       #** 尝试解析**字符串形**式的列表
 ****  **        **     **colu**mn_nam**es = ast.li**te**ral_eval**(colu**mn_names_stripped)
                if not isinstance(column_names, list):
                    column_names = [column_names]
            except (ValueError, SyntaxError):
                # 如果解析失败，当作普通字符串处理
                column_names = [column_names]
        else:
            # 如果是普通字符串，转换为单元素列表
            column_names = [column_names]
    elif not isinstance(column_names, list):
        # 如果是其他可迭代类型（如元组、集合等），尝试转换为列表
        try:
            column_names = list(column_names)
        except TypeError:
            raise ValueError(&quot;列名参数必须是字符串、列表或其他可迭代类型&quot;)
    
    print(f&quot;处理后的列名列表: {column_names}&quot;)
    print(f&quot;列名数量: {len(column_names)}&quot;)
    
    if not file_name or not isinstance(file_name, str):
        raise ValueError(&quot;文件名不能为空且必须为字符串类型&quot;)
    
    if not isinstance(start_column, int) or start_column &lt; 1:
        raise ValueError(&quot;开始列数必须为正整数&quot;)
    
    def _add_columns_to_sheet(sheet, columns, start_col):
        &quot;&quot;&quot;
        在指定sheet中新增列名
        &quot;&quot;&quot;
        # 如果没有要添加的列名，直接返回
        if len(columns) == 0:
            return
        
        print(f&quot;\n=== 处理工作表: {sheet.title} ===&quot;)
        
        # 通过扫描第1行来找到最后一个有数据的列
        last_col_with_data = 0
        max_check_col = max(sheet.max_column, 200)  # 扩大检查范围
        
        # 先清理可能存在的 Unnamed 列名
        for col in range(1, max_check_col + 1):
            cell = sheet.cell(row=1, column=col)
            cell_value = cell.value
            if cell_value is not None:
                cell_str = str(cell_value).strip()
                # 如果发现 Unnamed 格式的列名，清空它
                if cell_str.startswith(&quot;Unnamed:&quot;):
                    print(f&quot;发现并清除无效列名: {cell_str} (位置: 第{col}列)&quot;)
                    cell.value = None
                elif cell_str != &quot;&quot;:
                    last_col_with_data = col
        
        print(f&quot;清理后，检测到的最后一列位置: {last_col_with_data}&quot;)
        
        # 获取从start_col开始的现有列名
        existing_columns = []
        all_existing_columns = []  # 用于调试显示
        
        for col in range(1, last_col_with_data + 1):
            cell_value = sheet.cell(row=1, column=col).value
            if cell_value is not None:
                cell_str = str(cell_value).strip()
                all_existing_columns.append(f&quot;第{col}列: &#x27;{cell_str}&#x27;&quot;)
                if col &gt;= start_col and cell_str != &quot;&quot;:
                    existing_columns.append(cell_str)
        
        print(f&quot;所有现有列名: {all_existing_columns}&quot;)
        print(f&quot;从第{start_col}列开始的现有列名: {existing_columns}&quot;)
        
        # 找出需要添加的新列名（不在现有列名中的）
        new_columns = []
        for col_name in columns:
            col_name_str = str(col_name).strip()
            if col_name_str and col_name_str not in existing_columns:
                new_columns.append(col_name_str)
            else:
                print(f&quot;跳过已存在的列名: &#x27;{col_name_str}&#x27;&quot;)
        
        print(f&quot;需要添加的新列名: {new_columns}&quot;)
        print(f&quot;需要添加的列数: {len(new_columns)}&quot;)
        
        # 从下一列开始添加新的列名
        next_col = last_col_with_data + 1
        for i, col_name in enumerate(new_columns):
            current_col = next_col + i
            cell = sheet.cell(row=1, column=current_col)
            
            print(f&quot;准备在第{current_col}列写入: &#x27;{col_name}&#x27; (类型: {type(col_name)})&quot;)
            
            # 确保写入的是字符串
            cell.value = str(col_name)
            
            # 验证写入结果
            written_value = sheet.cell(row=1, column=current_col).value
            print(f&quot;写入后验证 - 第{current_col}列的值: &#x27;{written_value}&#x27;&quot;)
            
            if str(written_value) != str(col_name):
                print(f&quot;警告: 写入验证失败! 期望: &#x27;{col_name}&#x27;, 实际: &#x27;{written_value}&#x27;&quot;)
    
    try:
        # 2. 组合完整的Excel文件路径
        excel_path = os.path.join(folder_path, file_name)
        
        # 3. 检查文件是否存在
        if not os.path.exists(excel_path):
            raise FileNotFoundError(f&quot;找不到指定的Excel文件：{excel_path}&quot;)
        
        # 4. 打开Excel文件
        workbook = openpyxl.load_workbook(excel_path)
        print(f&quot;成功打开Excel文件，包含的工作表: {workbook.sheetnames}&quot;)
        
        # 5. 遍历所有sheet并添加列名（跳过名为&quot;分类&quot;的sheet）
        for sheet_name in workbook.sheetnames:
            if sheet_name == &quot;分类&quot;:
                print(f&quot;跳过工作表: {sheet_name}&quot;)
                continue
            sheet = workbook[sheet_name]
            _add_columns_to_sheet(sheet, column_names, start_column)
        
        # 6. 保存修改后的文件
        print(&quot;开始保存文件...&quot;)
        workbook.save(excel_path)
        workbook.close()
        print(&quot;Excel文件处理完成&quot;)
        
    except FileNotFoundError as e:
        raise e
    except Exception as e:
        raise Exception(f&quot;处理Excel文件时发生错误：{str(e)}&quot;)

```

```
import os
import json

from typing import *
try:
    from xbot.app.logging import trace as print
except:
    from xbot import print


def merge_all_txt_files(folder_path, output_file_path):
    &quot;&quot;&quot;
    title: 合并文件夹中所有txt文件
    description: 将指定文件夹 % folder_path % 下面的所有txt文件合并到一个文件 % output_file_path % 中。每个txt文件包含JSON格式的字典列表，最终合并所有字典到输出文件。
    inputs:
        - folder_path (folder): 包含txt文件的文件夹路径，eg: &quot;C:/data&quot;
        - output_file_path (file): 合并后的输出文件路径，eg: &quot;C:/output/merged_data.txt&quot;
    outputs:
        - result_path (str): 合并后的文件路径，eg: &quot;C:/output/merged_data.txt&quot;
    &quot;&quot;&quot;
    
    if not os.path.exists(folder_path):
        raise FileNotFoundError(f&quot;文件夹路径不存在：{folder_path}&quot;)
    
    # 确保输出文件的目录存在
    output_dir = os.path.dirname(output_file_path)
    if output_dir and not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    def _get_all_txt_files():
        &quot;&quot;&quot;获取文件夹中所有txt文件&quot;&quot;&quot;
        txt_files = []
        try:
            for file_name in os.listdir(folder_path):
                if file_name.lower().endswith(&#x27;.txt&#x27;):
                    file_path = os.path.join(folder_path, file_name)
                    if os.path.isfile(file_path):
                        txt_files.append(file_path)
            return sorted(txt_files)  # 按名称排序确保处理顺序一致
        except Exception as e:
            raise Exception(f&quot;读取文件夹失败：{str(e)}&quot;)
    
    def _read_file_content(file_path):
        &quot;&quot;&quot;读取并解析txt文件中的JSON列表内容&quot;&quot;&quot;
        try:
            with open(file_path, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as f:
                content = f.read().strip()
                if not content:
                    return []  # 空文件返回空列表
                # 使用json.loads解析JSON格式
                data = json.loads(content)
                if not isinstance(data, list):
                    raise ValueError(f&quot;文件 {file_path} 内容格式错误，应为列表格式&quot;)
                return data
        except json.JSONDecodeError as e:
            raise Exception(f&quot;文件 {file_path} JSON格式错误：{str(e)}&quot;)
        except Exception as e:
            raise Exception(f&quot;读取文件 {file_path} 失败：{str(e)}&quot;)
    
    def _merge_all_txt_files():
        &quot;&quot;&quot;合并所有txt文件中的字典&quot;&quot;&quot;
        txt_files = _get_all_txt_files()
        
        if not txt_files:
            raise ValueError(f&quot;文件夹 {folder_path} 中没有找到txt文件&quot;)
        
        all_dictionaries = []
        
        for file_path in txt_files:
            file_data = _read_file_content(file_path)
            
            # 检查列表中的元素是否为字典
            for item in file_data:
                if not isinstance(item, dict):
                    raise ValueError(f&quot;文件 {file_path} 中存在非字典元素：{type(item)}&quot;)
                all_dictionaries.append(item)
        
        return all_dictionaries
    
    # 合并所有字典
    merged_data = _merge_all_txt_files()
    
    # 写入合并后的文件（保持JSON格式）
    try:
        with open(output_file_path, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as f:
            json.dump(merged_data, f, ensure_ascii=False, indent=2)
    except Exception as e:
        raise Exception(f&quot;写入合并文件失败：{str(e)}&quot;)
    
    return output_file_path

```

```
# 使用此指令前，请确保安装必要的Python库，例如使用以下命令安装：
# pip install pandas openpyxl

import pandas as pd
import ast
import os
import re
from openpyxl import load_workbook

from typing import *
try:
    from xbot.app.logging import trace as print
except:
    from xbot import print


def update_excel_with_txt_data(excel_folder_path, excel_filename, txt_path, output_folder_path, output_filename):
    &quot;&quot;&quot;
    title: 根据txt文件数据更新Excel商品信息（保持原格式）
    description: 读取 % txt_path % 文件中的字典列表，直接使用商品名称在所有sheet中匹配&quot;商品名称&quot;列，将匹配的数据填入对应行列，保存到指定输出文件夹，完全保持原Excel格式。详细显示每个字段的更新情况。
    inputs:
        - excel_folder_path (folder): Excel文件所在文件夹路径，eg: &quot;C:/data&quot;
        - excel_filename (str): Excel文件名，eg: &quot;products.xlsx&quot;
        - txt_path (file): txt文件路径，eg: &quot;C:/data/product_data.txt&quot;
        - output_folder_path (folder): 输出文件夹路径，eg: &quot;C:/output&quot;
        - output_filename (str): 输出Excel文件名，eg: &quot;updated_products.xlsx&quot;
    outputs: None
    &quot;&quot;&quot;
    
    def _build_file_path(folder_path, filename):
        &quot;&quot;&quot;构建完整文件路径&quot;&quot;&quot;
        return os.path.join(folder_path, filename)
    
    def _read_txt_data(file_path):
        &quot;&quot;&quot;读取并解析txt文件中的字典列表&quot;&quot;&quot;
        try:
            with open(file_path, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as f:
                content = f.read().strip()
            return ast.literal_eval(content)
        except Exception as e:
            raise ValueError(f&quot;txt文件格式错误，无法解析: {e}&quot;)
    
    def _normalize_string(s):
        &quot;&quot;&quot;标准化字符串：统一分隔符、去除多余空格、转换为小写&quot;&quot;&quot;
        if isinstance(s, str):
            # 去除首尾空格和制表符
            normalized = s.strip().rstrip(&#x27;\t&#x27;)
            
            # 统一各种分隔符：将 _ / \ | - 等都替换为空格，然后统一处理
            normalized = re.sub(r&#x27;[_/\\|\\-]&#x27;, &#x27; &#x27;, normalized)
            
            # 将多个连续空格替换为单个空格
            normalized = re.sub(r&#x27;\s+&#x27;, &#x27; &#x27;, normalized)
            
            # 转换为小写
            return normalized.lower()
        return str(s).lower() if s is not None else &#x27;&#x27;
    
    def _clean_string(s):
        &quot;&quot;&quot;清理字符串，去除首尾空格和制表符&quot;&quot;&quot;
        if isinstance(s, str):
            return s.strip().rstrip(&#x27;\t&#x27;)
        return s
    
    def _calculate_similarity(str1, str2):
        &quot;&quot;&quot;计算两个字符串的相似度（基于标准化后的字符串）&quot;&quot;&quot;
        norm_str1 = _normalize_string(str1)
        norm_str2 = _normalize_string(str2)
        
        # 完全匹配
        if norm_str1 == norm_str2:
            return 1.0
        
        # 包含匹配（双向检查）
        if norm_str1 in norm_str2:
            return len(norm_str1) / len(norm_str2)
        elif norm_str2 in norm_str1:
            return len(norm_str2) / len(norm_str1)
        
        # 计算共同字符占比
        set1 = set(norm_str1)
        set2 = set(norm_str2)
        intersection = set1.intersection(set2)
        union = set1.union(set2)
        
        if len(union) == 0:
            return 0.0
        
        return len(intersection) / len(union)
    
    def _find_matching_rows_in_sheet(sheet_df, product_name, sheet_name):
        &quot;&quot;&quot;在指定sheet中寻找匹配的行&quot;&quot;&quot;
        matches = []
        
        if &#x27;商品名称&#x27; not in sheet_df.columns:
            return [], &quot;无商品名称列&quot;
        
        # 创建标准化的商品名称
        norm_target_name = _normalize_string(product_name)
        
        print(f&quot;  在sheet &#x27;{sheet_name}&#x27; 中查找商品名称: &#x27;{product_name}&#x27;&quot;)
        print(f&quot;  标准化后: &#x27;{norm_target_name}&#x27;&quot;)
        
        # 遍历所有行，计算相似度
        for idx, row_product_name in sheet_df[&#x27;商品名称&#x27;].items():
            if pd.isna(row_product_name):
                continue
                
            # 清理Excel中的商品名称（去除制表符等）
            cleaned_row_name = _clean_string(str(row_product_name))
            similarity = _calculate_similarity(cleaned_row_name, product_name)
            
            if similarity &gt; 0.4:  # 降低阈值，显示更多候选项
                matches.append((idx, similarity, cleaned_row_name))
                norm_row_name = _normalize_string(cleaned_row_name)
                print(f&quot;    候选匹配 [{idx}]: &#x27;{cleaned_row_name}&#x27; (相似度: {similarity:.3f})&quot;)
        
        if not matches:
            return [], &quot;无匹配&quot;
        
        # 按相似度排序，选择最佳匹配
        matches.sort(key=lambda x: x[1], reverse=True)
        best_similarity = matches[0][1]
        
        # 只选择相似度最高的匹配项
        best_matches = [match for match in matches if match[1] == best_similarity]
        
        match_indices = [match[0] for match in best_matches]
        
        if best_similarity &gt;= 0.95:
            return match_indices, &quot;完全匹配&quot;
        elif best_similarity &gt;= 0.8:
            return match_indices, f&quot;高相似度匹配({best_similarity:.3f})&quot;
        elif best_similarity &gt;= 0.6:
            return match_indices, f&quot;中相似度匹配({best_similarity:.3f})&quot;
        elif best_similarity &gt;= 0.4:
            return match_indices, f&quot;低相似度匹配({best_similarity:.3f})&quot;
        else:
            return [], &quot;相似度过低，跳过匹配&quot;
    
    def _get_column_index_by_name(ws, column_name):
        &quot;&quot;&quot;根据列名获取列索引&quot;&quot;&quot;
        for cell in ws[1]:  # 假设第一行是标题行
            if cell.value == column_name:
                return cell.column
        return None
    
    def _update_sheet_data_preserve_format(ws, sheet_df, sheet_name, target_data_list):
        &quot;&quot;&quot;更新指定sheet的数据，保持原格式&quot;&quot;&quot;
        updated_count = 0
        
        if &#x27;商品名称&#x27; not in sheet_df.columns:
            print(f&quot;警告: sheet &#x27;{sheet_name}&#x27; 中没有&#x27;商品名称&#x27;列，跳过处理&quot;)
            return updated_count
        
        print(f&quot;\n开始处理sheet &#x27;{sheet_name}&#x27;...&quot;)
        print(f&quot;该sheet中共有 {len(sheet_df)} 行数据&quot;)
        
        # 建立列名到列索引的映射
        column_mapping = {}
        for cell in ws[1]:  # 第一行是标题行
            if cell.value:
                column_mapping[cell.value] = cell.column
        
        print(f&quot;Excel中可用的列名: {list(column_mapping.keys())}&quot;)
        
        for data_item in target_data_list:
            try:
                # 直接使用文件名作为商品名称进行匹配
                product_name = data_item.get(&#x27;文件名&#x27;, &#x27;&#x27;)
                if not product_name:
                    print(&quot;跳过：数据项中没有&#x27;文件名&#x27;字段&quot;)
                    continue
                
                print(f&quot;\n处理商品: {product_name}&quot;)
                print(f&quot;数据项包含的字段: {list(data_item.keys())}&quot;)
                
                # 在当前sheet中查找匹配的行
                matching_rows, match_type = _find_matching_rows_in_sheet(sheet_df, product_name, sheet_name)
                
                if len(matching_rows) &gt; 0:
                    print(f&quot;找到 {len(matching_rows)} 个匹配行 (匹配类型: {match_type})&quot;)
                    for row_idx in matching_rows:
                        excel_product_name = _clean_string(str(sheet_df.loc[row_idx, &#x27;商品名称&#x27;]))
                        print(f&quot;  匹配行 {row_idx}: &#x27;{excel_product_name}&#x27;&quot;)
                    
                    # 统计更新情况
                    updated_fields = []
                    skipped_fields = []
                    
                    # 更新匹配行的数据，直接写入openpyxl的工作表
                    for key, value in data_item.items():
                        if key == &#x27;文件名&#x27;:
                            continue  # 跳过文件名字段
                        
                        if key in column_mapping:
                            col_idx = column_mapping[key]
                            for row_idx in matching_rows:
                                # row_idx是pandas的索引，需要转换为Excel的行号（+2，因为Excel从1开始，且有标题行）
                                excel_row = row_idx + 2
                                ws.cell(row=excel_row, column=col_idx, value=value)
                            updated_fields.append(key)
                        else:
                            skipped_fields.append(key)
                    
                    updated_count += len(matching_rows)
                    print(f&quot;已更新 {len(matching_rows)} 行数据&quot;)
                    print(f&quot;  成功更新的字段 ({len(updated_fields)}个): {updated_fields}&quot;)
                    if skipped_fields:
                        print(f&quot;  跳过的字段 ({len(skipped_fields)}个，Excel中无对应列): {skipped_fields}&quot;)
                else:
                    print(f&quot;在sheet &#x27;{sheet_name}&#x27; 中未找到商品 &#x27;{product_name}&#x27; 的匹配&quot;)
                    
            except Exception as e:
                print(f&quot;处理数据项时出错: {e}&quot;)
                continue
        
        return updated_count
    
    # 构建完整文件路径
    excel_path = _build_file_path(excel_folder_path, excel_filename)
    output_path = _build_file_path(output_folder_path, output_filename)
    
    # 检查输入文件是否存在
    if not os.path.exists(excel_path):
        raise FileNotFoundError(f&quot;Excel文件不存在: {excel_path}&quot;)
    
    if not os.path.exists(txt_path):
        raise FileNotFoundError(f&quot;txt文件不存在: {txt_path}&quot;)
    
    # 确保输出文件夹存在
    os.makedirs(output_folder_path, exist_ok=True)
    
    # 读取txt文件数据
    try:
        product_data = _read_txt_data(txt_path)
        if not isinstance(product_data, list):
            raise ValueError(&quot;txt文件内容应该是一个列表&quot;)
        print(f&quot;成功读取txt文件，共有 {len(product_data)} 条数据&quot;)
    except Exception as e:
        raise ValueError(f&quot;读取txt文件失败: {e}&quot;)
    
    # 用pandas读取Excel文件的所有sheet（用于数据分析）
    try:
        excel_data = pd.read_excel(excel_path, sheet_name=None)
        print(f&quot;成功读取Excel文件，共有 {len(excel_data)} 个sheet: {list(excel_data.keys())}&quot;)
    except Exception as e:
        raise ValueError(f&quot;读取Excel文件失败: {e}&quot;)
    
    # 用openpyxl加载Excel文件（用于保持格式）
    try:
        workbook = load_workbook(excel_path)
        print(&quot;成功加载Excel工作簿，保持原有格式&quot;)
    except Exception as e:
        raise ValueError(f&quot;加载Excel工作簿失败: {e}&quot;)
    
    total_updated = 0
    
    # 处理每个sheet
    for sheet_name in excel_data.keys():
        if sheet_name in workbook.sheetnames:
            try:
                sheet_df = excel_data[sheet_name]
                ws = workbook[sheet_name]
                
                updated_count = _update_sheet_data_preserve_format(ws, sheet_df, sheet_name, product_data)
                total_updated += updated_count
                print(f&quot;\nsheet &#x27;{sheet_name}&#x27; 处理完成，更新了 {updated_count} 行数据&quot;)
            except Exception as e:
                raise RuntimeError(f&quot;处理sheet &#x27;{sheet_name}&#x27; 时发生错误: {e}&quot;)
        else:
            print(f&quot;警告: sheet &#x27;{sheet_name}&#x27; 在workbook中未找到&quot;)
    
    # 保存更新后的Excel文件，保持所有原有格式
    try:
        workbook.save(output_path)
        print(f&quot;成功保存Excel文件，保持原有格式: {output_path}&quot;)
    except Exception as e:
        raise RuntimeError(f&quot;保存Excel文件失败: {e}&quot;)
    
    print(f&quot;\n处理完成！成功更新{total_updated}个商品信息，已保存到: {output_path}&quot;)

```

```
**# 使用此指****令前，请确保****安装必要的P****ytho**n库，例如使用以**下命令安装：****
# **pip **inst****all py**win32

**im**p**ort o****s
impo**rt w**in32**clipbo**ard
im**p**ort w****in3**2con**

**f**rom** typing **impor**t *
**tr**y**:
 **   from **xbot.**app.**lo**g**gin**g import** trac**e as** pr**int
exce**pt:**
    fro**m xbot** import pri**nt**

def copy_t**xt_**file_to_clip**board(****file**_path):
    **&quot;&quot;&quot;
 **   t**itl**e: 将txt文**件复制到剪贴**板**
   ** des**cripti**on: 根据指定的tx**t文**件路径 %fil**e_pat**h%，将txt文件本身复制到剪贴板，以便粘贴到其他位置（如文件管理器）。
    inputs:
        - file_path (file): txt文件的完整路径，eg: &quot;C:/documents/example.txt&quot;
    outputs:
        - None
    &quot;&quot;&quot;
    
    # 检查文件路径是否存在
    if not os.path.exists(file_path):
        raise FileNotFoundError(f&quot;指定的文件路径不存在: {file_path}&quot;)
    
    # 检查是否为文件（而不是文件夹）
    if not os.path.isfile(file_path):
        raise ValueError(f&quot;指定的路径不是文件: {file_path}&quot;)
    
    # 检查文件扩展名是否为txt
    if not file_path.lower().endswith(&#x27;.txt&#x27;):
        raise ValueError(f&quot;文件必须是txt格式: {file_path}&quot;)
    
    def _copy_file_to_clipboard(file_path):
        &quot;&quot;&quot;将文件复制到剪贴板&quot;&quot;&quot;
        try:
            # 将路径转换为绝对路径并规范化
            abs_path = os.path.abspath(file_path).replace(&#x27;/&#x27;, &#x27;\\&#x27;)
            
            # 构造DROPFILES结构
            # DROPFILES结构: 20字节头部 + 文件路径列表 + 双空字符结尾
            file_list = abs_path + &#x27;\0\0&#x27;  # 文件路径 + 双空字符结尾
            file_bytes = file_list.encode(&#x27;utf-16le&#x27;)
            
            # DROPFILES结构头部（20字节）
            drop_files_header = b&#x27;\x14\x00\x00\x00&#x27;  # 结构大小(20)
            drop_files_header += b&#x27;\x00\x00\x00\x00&#x27;  # 鼠标位置X
            drop_files_header += b&#x27;\x00\x00\x00\x00&#x27;  # 鼠标位置Y
            drop_files_header += b&#x27;\x00\x00\x00\x00&#x27;  # 是否在NC区域
            drop_files_header += b&#x27;\x01\x00\x00\x00&#x27;  # Unicode标志
            
            # 完整的剪贴板数据
            clipboard_data = drop_files_header + file_bytes
            
            # 设置剪贴板
            win32clipboard.OpenClipboard()
            win32clipboard.EmptyClipboard()
            win32clipboard.SetClipboardData(win32con.CF_HDROP, clipboard_data)
            win32clipboard.CloseClipboard()
            
        except Exception as e:
            try:
                win32clipboard.CloseClipboard()
            except:
                pass
            raise Exception(f&quot;复制文件到剪贴板时出错: {str(e)}&quot;)
    
    try:
        _copy_file_to_clipboard(file_path)
        return None
    except Exception as e:
        raise Exception(f&quot;复制txt文件到剪贴板时出错: {str(e)}&quot;)

```

```
**import**** os
**
from **xbot i**m**port ****print**

文件名 = os.path.basename(txt文件路径)
print(文件名)
```

```
# 使用此指令前，请确保安装必要的Python库，例如使用以下命令安装：
# pip install pyperclip

import pyperclip
import os

from typing import *
try:
    from xbot.app.logging import trace as print
except:
    from xbot import print


def write_clipboard_to_txt(folder_path, file_name):
    &quot;&quot;&quot;
    title: 将剪贴板内容写入txt文件
    description: 获取剪贴板中的内容，并将其写入到指定文件夹路径下的txt文件中。需要输入 % folder_path % 和 % file_name % 两个参数。
    inputs:
        - folder_path (str): 文件夹路径，eg: &quot;C:/Users/Desktop&quot;
        - file_name (str): 文件名（包含.txt扩展名），eg: &quot;clipboard_content.txt&quot;
    outputs: None
    &quot;&quot;&quot;
    
    # 检查输入有效性
    if not folder_path or not file_name:
        raise ValueError(&quot;文件夹路径和文件名不能为空&quot;)
    
    if not file_name.endswith(&#x27;.txt&#x27;):
        file_name += &#x27;.txt&#x27;
    
    def _get_clipboard_content():
        &quot;&quot;&quot;获取剪贴板内容&quot;&quot;&quot;
        try:
            content = pyperclip.paste()
            return content
        except Exception as e:
            raise RuntimeError(f&quot;无法获取剪贴板内容: {str(e)}&quot;)
    
    def _ensure_folder_exists(folder_path):
        &quot;&quot;&quot;确保文件夹存在，如果不存在则创建&quot;&quot;&quot;
        if not os.path.exists(folder_path):
            os.makedirs(folder_path)
    
    def _write_to_file(file_path, content):
        &quot;&quot;&quot;将内容写入文件&quot;&quot;&quot;
        try:
            with open(file_path, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as f:
                f.write(content)
        except Exception as e:
            raise RuntimeError(f&quot;写入文件失败: {str(e)}&quot;)
    
    # 获取剪贴板内容
    clipboard_content = _get_clipboard_content()
    
    # 确保文件夹存在
    _ensure_folder_exists(folder_path)
    
    # 构建完整文件路径
    full_file_path = os.path.join(folder_path, file_name)
    
    # 写入文件
    _write_to_file(full_file_path, clipboard_content)


![](img/25662f5b887d6c4a2bd356a0f7e10a42.png)

```

# 三、PPT提效

基于我上面自己的方法的基础上进一步优化PPT制作效率；

首先说明，我用的是**稿定设计**做PPT

### 3.1 稿定设计如何快速批量套版
[提效代码](https://zqxbobng8rp.feishu.cn/docx/YACMd91o4oIYyqxsqMVcs6wenIe?from=from_copylink)** 具体流程参考此文档**

### 3.2 PPT图片的商品主图如何快速找

```
imp**ort os
import shut**il
import re
import ast
import json
import numpy as np
import pandas as pd
from openpyxl import load_workbook
from PIL import Image

def sanitize_filename(name):
    &quot;&quot;&quot;去除文件名中不允许的字符&quot;&quot;&quot;
    return re.sub(r&#x27;[\\/*?:&quot;&lt;&gt;|]&#x27;, &#x27;&#x27;, name)

def clean_text(text):
    &quot;&quot;&quot;清洗文本：保留更多字符，避免过度清洗影响关键词匹配&quot;&quot;&quot;
    if not text:
        return &quot;&quot;
    # 保留字母、数字、中文、常见符号（如Δ、°等），只移除空格和常见标点
    cleaned = re.sub(r&#x27;[\s\n\r\t,，。！？；：&quot;&quot;&#x27;&#x27;（）()【】\[\]{}]&#x27;, &#x27;&#x27;, text)
    return cleaned.lower()

def parse_ocr_text(ocr_list_str):
    &quot;&quot;&quot;解析图片对应的OCR列表字符串，提取有效文本（减少过度过滤）&quot;&quot;&quot;
    try:
        ocr_list = ast.literal_eval(ocr_list_str.strip())
        valid_text = [
            str(item).strip() for item in ocr_list 
            if item and str(item).strip()  # 移除长度限制，保留更多文本
        ]
        # 不去重，保持原始顺序，避免丢失重要信息
        return &quot; &quot;.join(valid_text)
    except (SyntaxError, ValueError):
        return re.sub(r&#x27;\s+&#x27;, &#x27; &#x27;, ocr_list_str.strip())

def parse_ocr_collection(ocr_txt_content):
    &quot;&quot;&quot;解析TXT中的多图OCR集合，返回{图片名: OCR文本}的字典&quot;&quot;&quot;
    img_ocr_dict = {}
    img_segments = re.split(r&#x27;=== 根目录 - &#x27;, ocr_txt_content)
    for segment in img_segments:
        if not segment.strip():
            continue
        img_name_match = re.match(r&#x27;([^=]+)\s+===&#x27;, segment)
        if not img_name_match:
            continue
        img_name = img_name_match.group(1).strip().lower()
        ocr_content_match = re.search(r&#x27;--------------------------------------------------\n(.*?)(?=\n============================================================|$)&#x27;, segment, re.DOTALL)
        if ocr_content_match:
            ocr_list_str = ocr_content_match.group(1).strip()
            img_ocr_dict[img_name] = parse_ocr_text(ocr_list_str)
    return img_ocr_dict

def count_valid_text(text):
    &quot;&quot;&quot;统计有效文本长度（排除空格、符号，仅算中英文/数字）&quot;&quot;&quot;
    valid_chars = re.findall(r&#x27;[\u4e00-\u9fa5a-zA-Z0-9]&#x27;, text)
    return len(valid_chars)

def get_image_dimensions_and_ratio(image_path):
    &quot;&quot;&quot;获取图片的宽、高和宽高比，检测750×1499比例&quot;&quot;&quot;
    try:
        with Image.open(image_path) as img:
            width, height = img.size
            if height == 0:
                return width, height, 0, False
            ratio = width / height
            is_target_ratio = (abs(width - 750) &lt;= 10) and (abs(height - 1499) &lt;= 10)
            return width, height, ratio, is_target_ratio
    except Exception as e:
        print(f&quot;⚠️ 获取图片信息失败 {image_path}: {str(e)}&quot;)
        return 0, 0, 0, False

# 核心负面关键词（绝对不能出现，即使兜底也要排除）
CRITICAL_NEGATIVE_KEYWORDS = [
    &quot;购前须知&quot;, &quot;关于签收&quot;, &quot;关于售后&quot;, &quot;售后问题&quot;, &quot;退换货&quot;, &quot;包材&quot;,
    &quot;开箱视频&quot;, &quot;通电验机&quot;, &quot;屏幕碎裂&quot;, &quot;外观破损&quot;, &quot;缺少附件&quot;,
    &quot;在线客服&quot;, &quot;24小时&quot;, &quot;驳回申请&quot;, &quot;包装&quot;, &quot;保留3个月&quot;,
    &quot;影响退换货&quot;, &quot;无理由退货&quot;, &quot;质保期&quot;, &quot;三包服务&quot;, &quot;完整退回&quot;,
    &quot;附件缺失&quot;, &quot;全国联保&quot;, &quot;服务网点&quot;, &quot;售后&quot;, &quot;服务时间&quot;,
    &quot;易碎品&quot;, &quot;屏幕漏液&quot;, &quot;支架配件&quot;, &quot;包装清单&quot;,
    &quot;扫码&quot;, &quot;关注公众号&quot;, &quot;联系客服&quot;, &quot;客服&quot;, &quot;二维码&quot;, &quot;扫描&quot;,
     &quot;直播间&quot;, &quot;官网地址&quot;, &quot;扫码下单&quot;, &quot;公众号&quot;,
    &quot;购买后&quot;, &quot;超过15天&quot;, &quot;一键申请&quot;, &quot;快速处理&quot;,
    &quot;下拉详情&quot;, &quot;下拉查看&quot;, &quot;详情页查看&quot;, &quot;包装附件&quot;,
]

# 次要负面关键词（严格筛选时排除，兜底时可以接受）
SECONDARY_NEGATIVE_KEYWORDS = [
    &quot;中国能效标识&quot;, &quot;china energy label&quot;, &quot;gb21520&quot;, 
    &quot;耗能&quot;, &quot;能源效率&quot;, &quot;关闭状态功率&quot;, &quot;睡眠状态功率&quot;,
    &quot;生产者名称&quot;, &quot;规格型号&quot;, &quot;lcd显示器&quot;, &quot;暗夜武士&quot;, &quot;扬声器&quot;,
    &quot;第一步&quot;, &quot;点击此处&quot;, &quot;安装指南&quot;, &quot;使用前阅读&quot;, &quot;操作步骤&quot;,
    &quot;使用说明&quot;, &quot;安装步骤&quot;,
    &quot;图片仅供参考&quot;, &quot;以实物为准&quot;, &quot;颜色差异&quot;, &quot;批次差异&quot;,
    &quot;收到的实物为准&quot;, &quot;个体差异&quot;, &quot;软件版本&quot;, &quot;使用条件&quot;,
    &quot;环境因素&quot;, &quot;产品信息仅供参考&quot;, &quot;请以实际为准&quot;,
    &quot;超频刷新率&quot;, &quot;原生刷新率&quot;,
    &quot;Δe&quot;, &quot;色彩准确度&quot;, &quot;dic技术&quot;, &quot;dynamic image&quot;, &quot;动态清晰度&quot;,
    &quot;减少拖影&quot;, &quot;开启条件&quot;, &quot;freesync&quot;, &quot;hdr&quot;, &quot;实验室数据&quot;,
    &quot;有可能改进&quot;, &quot;具体信息参照实物&quot;,
    # 品牌宣传类（方案1）
    &quot;品牌后盾&quot;, &quot;公司成立于&quot;, &quot;总资产&quot;, &quot;员工总数&quot;, &quot;占地面积&quot;,
    &quot;国家及地区&quot;, &quot;均有销售&quot;, &quot;智能显示科技园区&quot;, &quot;业内不超过&quot;,
    &quot;宣宾智造&quot;, &quot;显未见&quot;, &quot;盛色智造&quot;, &quot;top5&quot;, &quot;品牌销量&quot;,
    &quot;万平方米&quot;, &quot;亿元&quot;, &quot;万台&quot;, &quot;年质保&quot;, &quot;质保&quot;, &quot;保修&quot;,
 
]

# 所有负面关键词（用于严格筛选）
NEGATIVE_KEYWORDS = CRITICAL_NEGATIVE_KEYWORDS + SECONDARY_NEGATIVE_KEYWORDS

CLEANED_NEGATIVE_KEYWORDS = [clean_text(kw) for kw in NEGATIVE_KEYWORDS]

def has_negative_keywords(ocr_text, debug=False):
    cleaned_text = clean_text(ocr_text)
    
    if debug:
        print(f&quot;🔍 检查负面关键词，原始文本: {ocr_text[:150]}...&quot;)
        print(f&quot;🔍 清洗后文本: {cleaned_text[:150]}...&quot;)
    
    for kw in CLEANED_NEGATIVE_KEYWORDS:
        if kw in cleaned_text:
            if debug:
                print(f&quot;❌ 发现负面关键词: &#x27;{kw}&#x27; 在文本中&quot;)
            return True, kw
    
    return False, None

def is_energy_efficiency_image(ocr_text):
    energy_keywords = [&quot;中国能效标识&quot;, &quot;china energy label&quot;, &quot;gb21520&quot;]
    cleaned_text = clean_text(ocr_text)
    return any(clean_text(kw) in cleaned_text for kw in energy_keywords)

def has_critical_negative_keywords(ocr_text):
    &quot;&quot;&quot;检查是否包含核心负面关键词（售后等绝对不能出现的）&quot;&quot;&quot;
    cleaned_text = clean_text(ocr_text)
    cleaned_critical = [clean_text(kw) for kw in CRITICAL_NEGATIVE_KEYWORDS]
    for kw in cleaned_critical:
        if kw in cleaned_text:
            return True, kw
    return False, None

def select_best_main_image(all_images, img_ocr_dict, folder_path):
    &quot;&quot;&quot;选择最佳主图，可以从&#x27;主图_&#x27;或&#x27;详情_&#x27;开头的图片中选择&quot;&quot;&quot;
    all_images_lower = [img.lower() for img in all_images]
    img_map = {img.lower(): img for img in all_images}
    
    # 优先选择&quot;主图_&quot;开头的图片，如果没有则选择&quot;详情_&quot;开头的图片
    main_candidates = [img for img in all_images_lower if img.startswith(&quot;主图_&quot;)]
    if not main_candidates:
        main_candidates = [img for img in all_images_lower if img.startswith(&quot;详情_&quot;)]
        if not main_candidates:
            print(&quot;❌ 没有找到&#x27;主图_&#x27;或&#x27;详情_&#x27;开头的图片&quot;)
            return None
    
    valid_candidates = []
    for img in main_candidates:
        img_path = os.path.join(folder_path, img_map[img])
        ocr_text = img_ocr_dict.get(img, &quot;&quot;)
        
        # 1. 排除参数说明类图片
        parameter_keywords = [
            &quot;技术指标&quot;
        ]
        cleaned_ocr = clean_text(ocr_text)
        has_parameters = any(clean_text(kw) in cleaned_ocr for kw in parameter_keywords)
        if has_parameters:
            print(f&quot;❌ 排除主图候选 {img_map[img]}: 含过多技术参数文字&quot;)
            continue
        
        # 2. 排除色彩单调的图片（基于色彩丰富度）
        try:
            with Image.open(img_path) as img_pil:
                img_rgb = img_pil.convert(&quot;RGB&quot;)
                histogram = img_rgb.histogram()
                color_diversity = sum(1 for val in histogram if val &gt; 0)
                
                if color_diversity &lt; 60:
                    print(f&quot;❌ 排除主图候选 {img_map[img]}: 色彩过于单调（丰富度低）&quot;)
                    continue
        except Exception as e:
            print(f&quot;⚠️ 分析图片 {img_map[img]} 色彩时出错: {e}，跳过色彩筛选&quot;)
        
        # 3. 排除产品主体占比过低的图片（如大量留白、主体过小）
        try:
            with Image.open(img_path) as img_pil:
                img_gray = img_pil.convert(&quot;L&quot;)
                pixels = list(img_gray.getdata())
                non_white_pixels = sum(1 for p in pixels if p &lt; 240)
                total_pixels = len(pixels)
                if total_pixels &gt; 0:
                    subject_ratio = non_white_pixels / total_pixels
                    if subject_ratio &lt; 0.3:
                        print(f&quot;❌ 排除主图候选 {img_map[img]}: 产品主体占比过低 ({subject_ratio:.2%})&quot;)
                        continue
        except Exception as e:
            print(f&quot;⚠️ 分析图片 {img_map[img]} 主体占比时出错: {e}，跳过该筛选&quot;)
        
        # 4. 排除色彩分布过于均匀的图片（如单一角度侧面图）
        try:
            with Image.open(img_path) as img_pil:
                img_rgb = img_pil.convert(&quot;RGB&quot;)
                pixels = list(img_rgb.getdata())
                sample_size = min(10000, len(pixels))
                sampled_pixels = np.random.choice(pixels, sample_size)
                
                r_vals = [p[0] for p in sampled_pixels]
                g_vals = [p[1] for p in sampled_pixels]
                b_vals = [p[2] for p in sampled_pixels]
                
                r_std = np.std(r_vals)
                g_std = np.std(g_vals)
                b_std = np.std(b_vals)
                avg_std = (r_std + g_std + b_std) / 3
                
                if avg_std &lt; 15:
                    print(f&quot;❌ 排除主图候选 {img_map[img]}: 色彩分布过于均匀（标准差: {avg_std:.2f}）&quot;)
                    continue
        except Exception as e:
            print(f&quot;⚠️ 分析图片 {img_map[img]} 色彩分布时出错: {e}，跳过该筛选&quot;)
        
        # 5. 检查图片尺寸和比例：排除极窄高、极宽扁和750×1499比例
        try:
            width, height, ratio, is_target_ratio = get_image_dimensions_and_ratio(img_path)
            if ratio &lt; 0.5:
                print(f&quot;❌ 排除主图候选 {img_map[img]}: 极窄高比例 (宽高比: {ratio:.2f})&quot;)
                continue
            if ratio &gt; 2.0:
                print(f&quot;❌ 排除主图候选 {img_map[img]}: 极宽扁比例 (宽高比: {ratio:.2f})&quot;)
                continue
            if is_target_ratio:
                print(f&quot;❌ 排除主图候选 {img_map[img]}: 750×1499比例（{width}×{height}）&quot;)
                continue
        except Exception as e:
            print(f&quot;⚠️ 分析图片 {img_map[img]} 比例时出错: {e}，跳过该筛选&quot;)
        
        # 6. 检查OCR内容：能效标识和负面关键词
        if is_energy_efficiency_image(ocr_text):
            print(f&quot;❌ 排除主图候选 {img_map[img]}: 包含能效标识&quot;)
            continue
        
        has_negative, matched_kw = has_negative_keywords(ocr_text, debug=True)
        if has_negative:
            print(f&quot;❌ 排除主图候选 {img_map[img]}: 包含负面关键词 &#x27;{matched_kw}&#x27;&quot;)
            continue
        
        text_length = count_valid_text(ocr_text)
        if text_length &lt;= 50:
            valid_candidates.append((img, text_length))
    
    if valid_candidates:
        valid_candidates.sort(key=lambda x: x[1])
        return img_map[valid_candidates[0][0]]
    else:
        print(&quot;⚠️ 无完全符合条件的主图，使用宽松筛选（只排除负面关键词和异常比例）&quot;)
        # 宽松筛选：至少要排除负面关键词、能效标识和异常比例
        loose_main_candidates = []
        for img in main_candidates:
            img_path = os.path.join(folder_path, img_map[img])
            ocr_text = img_ocr_dict.get(img, &quot;&quot;)
            
            # 检查宽高比
            try:
                width, height, ratio, is_target_ratio = get_image_dimensions_and_ratio(img_path)
                if ratio &lt; 0.5 or ratio &gt; 2.0 or is_target_ratio:
                    continue
            except:
                pass
            
            # 检查负面关键词
            if is_energy_efficiency_image(ocr_text):
                continue
            has_negative, _ = has_negative_keywords(ocr_text)
            if has_negative:
                continue
            
            text_length = count_valid_text(ocr_text)
            loose_main_candidates.append((img, text_length))
        
        if loose_main_candidates:
            loose_main_candidates.sort(key=lambda x: x[1])
            print(f&quot;✅ 宽松筛选选中主图: {img_map[loose_main_candidates[0][0]]}&quot;)
            return img_map[loose_main_candidates[0][0]]
        else:
            print(&quot;⚠️ 宽松筛选也无结果，启动兜底机制（只排除核心负面关键词）&quot;)
            # 兜底机制：只排除核心负面关键词和能效标识
            fallback_candidates = []
            for img in main_candidates:
                img_path = os.path.join(folder_path, img_map[img])
                ocr_text = img_ocr_dict.get(img, &quot;&quot;)
                
                # 只检查核心负面关键词
                if is_energy_efficiency_image(ocr_text):
                    continue
                has_critical, critical_kw = has_critical_negative_keywords(ocr_text)
                if has_critical:
                    print(f&quot;⚠️ 兜底排除 {img_map[img]}: 包含核心负面关键词 &#x27;{critical_kw}&#x27;&quot;)
                    continue
                
                text_length = count_valid_text(ocr_text)
                fallback_candidates.append((img, text_length))
            
            if fallback_candidates:
                fallback_candidates.sort(key=lambda x: x[1])
                print(f&quot;✅ 兜底机制选中主图: {img_map[fallback_candidates[0][0]]}&quot;)
                return img_map[fallback_candidates[0][0]]
            else:
                print(&quot;❌ 兜底机制也无法找到主图（所有图片都包含核心负面关键词）&quot;)
                return None

def select_best_detail_image(all_images, img_ocr_dict, excluded_main_img, folder_path):
    &quot;&quot;&quot;选择最佳详情图，优先从&#x27;详情_&#x27;开头的图片中选择，实在没有则从主图候选中选择&quot;&quot;&quot;
    all_images_lower = [img.lower() for img in all_images]
    img_map = {img.lower(): img for img in all_images}
    excluded_lower = excluded_main_img.lower() if excluded_main_img else None
    
    # 优先选择&quot;详情_&quot;开头的图片（排除已选的主图）
    detail_candidates = [
        img for img in all_images_lower 
        if img.startswith(&quot;详情_&quot;) and img != excluded_lower
    ]
    
    # 如果没有详情图候选，则从主图候选中选择（排除已选的主图）
    if not detail_candidates:
        print(&quot;⚠️ 没有找到&#x27;详情_&#x27;开头的图片，从&#x27;主图_&#x27;候选中选择详情图&quot;)
        detail_candidates = [
            img for img in all_images_lower 
            if img.startswith(&quot;主图_&quot;) and img != excluded_lower
        ]
        if not detail_candidates:
            print(&quot;❌ 也没有找到&#x27;主图_&#x27;开头的图片&quot;)
            return None
    
    valid_candidates = []
    for img in detail_candidates:
        img_full_path = os.path.join(folder_path, img_map[img])
        
        # 检查图片尺寸和比例：排除750×1499比例、极窄高和极宽扁比例
        width, height, ratio, is_target_ratio = get_image_dimensions_and_ratio(img_full_path)
        if is_target_ratio:
            print(f&quot;❌ 排除详情候选 {img_map[img]}: 750×1499比例（{width}×{height}）&quot;)
            continue
        
        if ratio &lt; 0.7:
            print(f&quot;❌ 排除详情候选 {img_map[img]}: 极窄高比例 (宽高比: {ratio:.2f})&quot;)
            continue
        
        if ratio &gt; 2.0:
            print(f&quot;❌ 排除详情候选 {img_map[img]}: 极宽扁比例 (宽高比: {ratio:.2f})&quot;)
            continue
        
        ocr_text = img_ocr_dict.get(img, &quot;&quot;)
        
        if is_energy_efficiency_image(ocr_text):
            print(f&quot;❌ 排除详情候选 {img_map[img]}: 包含能效标识&quot;)
            continue
        
        has_negative, matched_kw = has_negative_keywords(ocr_text, debug=True)
        if has_negative:
            print(f&quot;❌ 排除详情候选 {img_map[img]}: 包含负面关键词 &#x27;{matched_kw}&#x27;&quot;)
            continue
        
        text_length = count_valid_text(ocr_text)
        if 30 &lt;= text_length &lt;= 100:
            valid_candidates.append((img, text_length, ratio))
    
    if not valid_candidates:
        print(&quot;⚠️ 无完全符合条件的详情图，使用宽松筛选（忽略文字长度要求，但仍检查负面关键词和比例）&quot;)
        loose_candidates = []
        for img in detail_candidates:
            img_full_path = os.path.join(folder_path, img_map[img])
            
            # 仍然要检查宽高比（宽松阈值0.5-2.0）
            width, height, ratio, is_target_ratio = get_image_dimensions_and_ratio(img_full_path)
            if is_target_ratio or ratio &lt; 0.5 or ratio &gt; 2.0:
                continue
            
            ocr_text = img_ocr_dict.get(img, &quot;&quot;)
            
            # 仍然要排除能效标识和负面关键词
            if is_energy_efficiency_image(ocr_text):
                continue
            if has_negative_keywords(ocr_text)[0]:
                continue
            
            text_length = count_valid_text(ocr_text)
            loose_candidates.append((img, text_length, ratio))
        
        if loose_candidates:
            # 优先选择宽高比更接近1.0的图片（更方正好看），其次考虑文字长度
            loose_candidates.sort(key=lambda x: (abs(x[2] - 1.0), abs(x[1] - 50)))
            print(f&quot;✅ 宽松筛选选中详情图: {img_map[loose_candidates[0][0]]} (宽高比: {loose_candidates[0][2]:.2f})&quot;)
            return img_map[loose_candidates[0][0]]
        else:
            print(&quot;⚠️ 宽松筛选也无结果，启动兜底机制（只排除核心负面关键词和极端比例）&quot;)
            # 兜底机制：只排除核心负面关键词、能效标识和极端比例，允许750×1499
            fallback_candidates = []
            for img in detail_candidates:
                img_full_path = os.path.join(folder_path, img_map[img])
                ocr_text = img_ocr_dict.get(img, &quot;&quot;)
                
                # 检查极端比例（比例范围放宽到0.5-5.0，允许750×1499）
                width, height, ratio, is_target_ratio = get_image_dimensions_and_ratio(img_full_path)
                if ratio &lt; 0.5 or ratio &gt; 5.0:  # 只排除极端比例，允许750×1499
                    continue
                
                # 只检查核心负面关键词
                if is_energy_efficiency_image(ocr_text):
                    continue
                has_critical, critical_kw = has_critical_negative_keywords(ocr_text)
                if has_critical:
                    print(f&quot;⚠️ 兜底排除 {img_map[img]}: 包含核心负面关键词 &#x27;{critical_kw}&#x27;&quot;)
                    continue
                
                text_length = count_valid_text(ocr_text)
                fallback_candidates.append((img, text_length))
            
            if fallback_candidates:
                fallback_candidates.sort(key=lambda x: x[1])
                print(f&quot;✅ 兜底机制选中详情图: {img_map[fallback_candidates[0][0]]}（文字长度: {fallback_candidates[0][1]}）&quot;)
                return img_map[fallback_candidates[0][0]]
            else:
                print(&quot;⚠️ 详情图候选都不符合要求，尝试从主图中选择&quot;)
                # 最后兜底：从主图中选择（排除已选的主图）
                main_candidates = [
                    img for img in all_images_lower 
                    if img.startswith(&quot;主图_&quot;) and img != excluded_lower
                ]
                
                if not main_candidates:
                    print(&quot;❌ 也没有可用的主图&quot;)
                    return None
                
                # 从主图中选择最佳的作为详情图
                main_fallback_candidates = []
                for img in main_candidates:
                    img_full_path = os.path.join(folder_path, img_map[img])
                    ocr_text = img_ocr_dict.get(img, &quot;&quot;)
                    
                    # 检查宽高比和核心负面关键词
                    width, height, ratio, is_target_ratio = get_image_dimensions_and_ratio(img_full_path)
                    if ratio &lt; 0.5 or ratio &gt; 5.0:
                        continue
                    
                    if is_energy_efficiency_image(ocr_text):
                        continue
                    has_critical, critical_kw = has_critical_negative_keywords(ocr_text)
                    if has_critical:
                        continue
                    
                    main_fallback_candidates.append((img, ratio))
                
                if main_fallback_candidates:
                    # 优先选择宽高比接近1.0的主图
                    main_fallback_candidates.sort(key=lambda x: abs(x[1] - 1.0))
                    print(f&quot;✅ 从主图中选择详情图: {img_map[main_fallback_candidates[0][0]]} (宽高比: {main_fallback_candidates[0][1]:.2f})&quot;)
                    return img_map[main_fallback_candidates[0][0]]
                else:
                    print(&quot;❌ 所有主图也不符合要求&quot;)
                    return None
    
    # 如果有符合条件的候选，优先选择宽高比接近1.0的（更方正好看）
    if valid_candidates:
        valid_candidates.sort(key=lambda x: (abs(x[2] - 1.0), abs(x[1] - 50)))
        print(f&quot;✅ 严格筛选选中详情图: {img_map[valid_candidates[0][0]]} (宽高比: {valid_candidates[0][2]:.2f})&quot;)
        return img_map[valid_candidates[0][0]]
    else:
        print(&quot;⚠️ 没有找到符合条件的&#x27;详情_&#x27;图片&quot;)
        return None

def process_image_folders(source_root, ppt_root, excel_path, ocr_txt_root):
    os.makedirs(ppt_root, exist_ok=True)
    xls = pd.ExcelFile(excel_path)
    
    processing_results = {}
    
    for folder_name in os.listdir(source_root):
        folder_path = os.path.join(source_root, folder_name)
        if not os.path.isdir(folder_path):
            continue
        
        print(f&quot;\n{&#x27;=&#x27;*40}\n开始处理文件夹: {folder_name}&quot;)
        
        if &#x27;_&#x27; not in folder_name:
            print(f&quot;❌ 文件夹命名错误(缺少下划线): {folder_name}，跳过&quot;)
            continue
        sheet_name, product_name = folder_name.split(&#x27;_&#x27;, 1)
        
        if sheet_name not in xls.sheet_names:
            print(f&quot;❌ Excel中无对应sheet: {sheet_name}，跳过&quot;)
            continue
        df = xls.parse(sheet_name)
        if &#x27;商品名称&#x27; not in df.columns:
            print(f&quot;❌ sheet {sheet_name} 中无&#x27;商品名称&#x27;列，跳过&quot;)
            continue
        
        # 先将_替换为/，再去除特殊符号，确保匹配一致性
        normalized_product_name = product_name.replace(&#x27;_&#x27;, &#x27;/&#x27;)
        sanitized_product = sanitize_filename(normalized_product_name)
        
        matched_row = None
        for idx, row in df.iterrows():
            excel_product = sanitize_filename(str(row[&#x27;商品名称&#x27;]))
            # 匹配逻辑：子串包含即可
            if sanitized_product in excel_product or excel_product in sanitized_product:
                matched_row = idx + 2
                break
        if matched_row is None:
            print(f&quot;❌ 未匹配到商品: {product_name}，跳过&quot;)
            continue
        image_number = matched_row - 1
        
        all_images = [
            f for f in os.listdir(folder_path) 
            if f.lower().endswith((&#x27;.jpg&#x27;, &#x27;.jpeg&#x27;, &#x27;.png&#x27;, &#x27;.gif&#x27;))
        ]
        
        if len(all_images) &lt; 2:
            print(f&quot;❌ 图片数量不足2张（仅{len(all_images)}张），跳过&quot;)
            continue
        
        ocr_txt_filename = f&quot;{folder_name}.txt&quot;
        ocr_txt_path = os.path.join(ocr_txt_root, ocr_txt_filename)
        img_ocr_dict = {}
        if os.path.exists(ocr_txt_path):
            with open(ocr_txt_path, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as f:
                ocr_content = f.read()
            img_ocr_dict = parse_ocr_collection(ocr_content)
        else:
            print(f&quot;⚠️ OCR文件缺失: {ocr_txt_path}，仅按文件名和比例筛选&quot;)
        
        selected_main = select_best_main_image(all_images, img_ocr_dict, folder_path)
        
        selected_detail = None
        if selected_main:
            selected_detail = select_best_detail_image(all_images, img_ocr_dict, selected_main, folder_path)
            
        
        # 检查是否成功选择了主图和详情图
        if not selected_main or not selected_detail:
            print(f&quot;⚠️ 跳过文件夹 {folder_name}: 没有找到合适的主图或详情图&quot;)
            continue
        
        # 记录处理结果
        processing_results[image_number] = {
            &#x27;folder_name&#x27;: folder_name,
            &#x27;sheet_name&#x27;: sheet_name,
            &#x27;all_images&#x27;: all_images,
            &#x27;img_ocr_dict&#x27;: img_ocr_dict,
            &#x27;folder_path&#x27;: folder_path,
            &#x27;selected_main&#x27;: selected_main,
            &#x27;selected_detail&#x27;: selected_detail
        }
        
        main_target_folder = os.path.join(ppt_root, f&quot;{sheet_name}_图1&quot;)
        detail_target_folder = os.path.join(ppt_root, f&quot;{sheet_name}_图2&quot;)
        os.makedirs(main_target_folder, exist_ok=True)
        os.makedirs(detail_target_folder, exist_ok=True)
        
        shutil.copy2(
            os.path.join(folder_path, selected_main),
            os.path.join(main_target_folder, f&quot;{image_number}.jpg&quot;)
        )
        shutil.copy2(
            os.path.join(folder_path, selected_detail),
            os.path.join(detail_target_folder, f&quot;{image_number}.jpg&quot;)
        )
        
        print(f&quot;✅ 处理完成: 主图={selected_main}（图1文件夹）, 详情图={selected_detail}（图2文件夹）, 编号={image_number}&quot;)
    
    # 保存编号到源文件夹的映射
    img_num_to_source_map = {}
    for folder_name in os.listdir(source_root):
        folder_path = os.path.join(source_root, folder_name)
        if not os.path.isdir(folder_path):
            continue
        
        if &#x27;_&#x27; not in folder_name:
            continue
        
        sheet_name, product_name = folder_name.split(&#x27;_&#x27;, 1)
        
        try:
            images = [f for f in os.listdir(folder_path) if f.lower().endswith((&#x27;.jpg&#x27;, &#x27;.jpeg&#x27;, &#x27;.png&#x27;, &#x27;.gif&#x27;))]
            if not images:
                continue
            
            # 从文件夹名称中提取产品名称，然后从 Excel 中查找编号
            normalized_product_name = product_name.replace(&#x27;_&#x27;, &#x27;/&#x27;)
            sanitized_product = sanitize_filename(normalized_product_name)
            
            xls_temp = pd.ExcelFile(excel_path)
            for sheet in xls_temp.sheet_names:
                df_temp = xls_temp.parse(sheet)
                if &#x27;商品名称&#x27; not in df_temp.columns:
                    continue
                
                for idx, row in df_temp.iterrows():
                    excel_product = sanitize_filename(str(row[&#x27;商品名称&#x27;]))
                    if sanitized_product in excel_product or excel_product in sanitized_product:
                        img_num = idx + 2  # Excel行号是从2开始（第1行是标题）
                        img_num_to_source_map[img_num] = source_root
                        break
        except:
            continue
    
    # 保存映射文件
    mapping_file = os.path.join(ppt_root, &#x27;img_num_to_source.json&#x27;)
    try:
        with open(mapping_file, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as f:
            json.dump(img_num_to_source_map, f, ensure_ascii=False, indent=2)
    except:
        pass
    
    return processing_results

def reselect_images_interactive(ppt_root, selected_source_folder, category_excel, output_folder_type=&quot;图1&quot;):
    &quot;&quot;&quot;交互式重选图片，打开编号对应的产品文件夹让用户手动选择图片&quot;&quot;&quot;
    # 从 PPT 文件夹中提取 sheet_name
    sheet_name = None
    for folder_name in os.listdir(ppt_root):
        if &quot;_图1&quot; in folder_name:
            sheet_name = folder_name.replace(&quot;_图1&quot;, &quot;&quot;)
            break
    
    if not sheet_name:
        print(&quot;❌ 没有找到PPT文件夹中的图片文件夹&quot;)
        return
    
    # 加载Excel，根据sheet_name加载对应的sheet
    xls = pd.ExcelFile(category_excel)
    if sheet_name not in xls.sheet_names:
        print(f&quot;❌ Excel中没有sheet: {sheet_name}&quot;)
        return
    df = xls.parse(sheet_name)
    
    main_target_folder = os.path.join(ppt_root, f&quot;{sheet_name}_图1&quot;)
    detail_target_folder = os.path.join(ppt_root, f&quot;{sheet_name}_图2&quot;)
    
    # 扫描已处理的编号
    processed_numbers = set()
    if os.path.exists(main_target_folder):
        for img_file in os.listdir(main_target_folder):
            if img_file.lower().endswith((&#x27;.jpg&#x27;, &#x27;.jpeg&#x27;, &#x27;.png&#x27;, &#x27;.gif&#x27;)):
                try:
                    img_num = int(img_file.split(&#x27;.&#x27;)[0])
                    processed_numbers.add(img_num)
                except ValueError:
                    pass
    
    if not processed_numbers:
        print(&quot;❌ 没有找到之前的处理结果&quot;)
        return
    
    print(&quot;\n&quot; + &quot;=&quot;*60)
    print(&quot;📋 已处理的图片编号列表:&quot;)
    for img_num in sorted(processed_numbers):
        print(f&quot;  编号 {img_num}&quot;)
    
    while True:
        user_input = input(&quot;\n请输入要重选的图片编号（单个，如 9），或输入 &#x27;q&#x27; 退出: &quot;).strip()
        if user_input.lower() == &#x27;q&#x27;:
            print(&quot;\n✅ 退出重选界面&quot;)
            break
        
        try:
            img_num = int(user_input)
        except ValueError:
            print(&quot;❌ 输入格式错误，请输入数字或 &#x27;q&#x27;&quot;)
            continue
        
        if img_num not in processed_numbers:
            print(f&quot;❌ 编号 {img_num} 不存在&quot;)
            continue
        
        # 从Excel中查找编号对应的商品名称
        try:
            product_name = df.iloc[img_num - 1][&#x27;商品名称&#x27;]
        except (IndexError, KeyError):
            print(f&quot;❌ 编号 {img_num} 在 Excel 中不存在&quot;)
            continue
        
        if not product_name:
            print(f&quot;❌ 编号 {img_num} 的商品名称为空&quot;)
            continue
        
        # 先将_替换为/，再去除特殊符号，确保匹配一致性
        normalized_product_name = product_name.replace(&#x27;_&#x27;, &#x27;/&#x27;)
        sanitized_product = sanitize_filename(normalized_product_name)
        product_folder = None
        
        if not os.path.exists(selected_source_folder):
            print(f&quot;❌ 源文件夹不存在: {selected_source_folder}&quot;)
            continue
        
        for folder_name in os.listdir(selected_source_folder):
            folder_path = os.path.join(selected_source_folder, folder_name)
            if not os.path.isdir(folder_path):
                continue
            
            if &#x27;_&#x27; not in folder_name:
                continue
            
            # 提取文件夹中的product_name部分
            sheet_name_folder, product_name_folder = folder_name.split(&#x27;_&#x27;, 1)
            sanitized_folder_product = sanitize_filename(product_name_folder)
            
            # 使用原本的匹配逻辑：子串包含即可
            if sanitized_product in sanitized_folder_product or sanitized_folder_product in sanitized_product:
                try:
                    images = [f for f in os.listdir(folder_path) if f.lower().endswith((&#x27;.jpg&#x27;, &#x27;.jpeg&#x27;, &#x27;.png&#x27;, &#x27;.gif&#x27;))]
                    if images:
                        product_folder = folder_path
                        break
                except:
                    continue
        
        if not product_folder:
            print(f&quot;❌ 编号 {img_num}（商品: {product_name}）的产品文件夹不存在&quot;)
            continue
        
        print(f&quot;\n🔄 重选编号 {img_num} 的图片&quot;)
        print(f&quot;📂 正在打开产品文件夹: {product_folder}&quot;)
        print(&quot;请选择一个图片文件，然后关闭Windows文件管理器窗口&quot;)
        
        # 打开产品文件夹
        import subprocess
        subprocess.Popen(f&#x27;explorer &quot;{product_folder}&quot;&#x27;)
        
        # 等待用户选择图片
        selected_file = input(&quot;\n请输入选择的图片文件名（不需要路径，仅需要文件名）: &quot;).strip()
        
        if not selected_file:
            print(&quot;❌ 未选择图片&quot;)
            continue
        
        # 检查文件是否存在
        selected_file_path = os.path.join(product_folder, selected_file)
        if not os.path.exists(selected_file_path):
            print(f&quot;❌ 文件不存在: {selected_file}&quot;)
            continue
        
        # 覆盖主图和详情图
        try:
            # 确保目标文件夹存在
            os.makedirs(main_target_folder, exist_ok=True)
            os.makedirs(detail_target_folder, exist_ok=True)
            
            # 根据选择只覆盖对应的输出文件夹
            if output_folder_type == &quot;图1&quot;:
                shutil.copy2(
                    selected_file_path,
                    os.path.join(main_target_folder, f&quot;{img_num}.jpg&quot;)
                )
            else:  # 图2
                shutil.copy2(
                    selected_file_path,
                    os.path.join(detail_target_folder, f&quot;{img_num}.jpg&quot;)
                )
            print(f&quot;\n✅ 已更新编号 {img_num}:&quot;)
            print(f&quot;   图片: {selected_file}&quot;)
            print(f&quot;\n📄 请查看更新后的图片是否满意&quot;)
            print(&quot;=&quot;*60)
        except Exception as e:
            print(f&quot;\n❌ 覆盖图片失败: {str(e)}&quot;)

def write_to_excel(ppt_root, ref_excel_path):
    for folder_name in os.listdir(ppt_root):
        folder_path = os.path.join(ppt_root, folder_name)
        if not os.path.isdir(folder_path) or &quot;_图&quot; not in folder_name:
            continue
        
        images = []
        for f in os.listdir(folder_path):
            if f.lower().endswith((&#x27;.jpg&#x27;, &#x27;.jpeg&#x27;, &#x27;.png&#x27;, &#x27;.gif&#x27;)):
                try:
                    img_num = int(f.split(&#x27;.&#x27;)[0])
                    images.append(img_num)
                except ValueError:
                    print(f&quot;⚠️ 图片 {f} 命名不符合数字格式，已跳过&quot;)
        
        images.sort()
        if not images:
            print(f&quot;❌ 文件夹 {folder_name} 中无有效图片，跳过Excel生成&quot;)
            continue
        
        excel_path = os.path.join(folder_path, f&quot;{folder_name}.xlsx&quot;)
        shutil.copy2(ref_excel_path, excel_path)
        
        wb = load_workbook(excel_path)
        ws = wb.active
        
        if ws.max_column &gt; 2:
            for col in range(ws.max_column, 2, -1):
                ws.delete_cols(col)
        
        ws.cell(row=2, column=2).value = &quot;图片_1&quot;
        
        for i, img_num in enumerate(images):
            row = i + 3
            ws.cell(row=row, column=1).value = f&quot;页面{i+1}&quot;
            ws.cell(row=row, column=2).value = str(img_num)
        
        wb.save(excel_path)
        print(f&quot;📊 生成Excel: {excel_path}，共{len(images)}个页面&quot;)

def process_videos_and_update_excel(video_folder, ppt_root, category_excel):
    xls = pd.ExcelFile(category_excel)
    for video_name in os.listdir(video_folder):
        video_path = os.path.join(video_folder, video_name)
        if not os.path.isfile(video_path) or not video_name.lower().endswith(&#x27;.mp4&#x27;):
            continue
        
        base_name = os.path.splitext(video_name)[0]
        if &#x27;_&#x27; not in base_name:
            print(f&quot;❌ 视频命名错误: {video_name}，跳过&quot;)
            continue
        sheet_name, product_name = base_name.split(&#x27;_&#x27;, 1)
        
        if sheet_name not in xls.sheet_names:
            print(f&quot;❌ 视频 {video_name} 无对应sheet: {sheet_name}，跳过&quot;)
            continue
        df = xls.parse(sheet_name)
        if &#x27;商品名称&#x27; not in df.columns:
            print(f&quot;❌ sheet {sheet_name} 无&#x27;商品名称&#x27;列，跳过视频处理&quot;)
            continue
        
        sanitized_product = sanitize_filename(product_name)
        matched_row = None
        for idx, row in df.iterrows():
            excel_product = sanitize_filename(str(row[&#x27;商品名称&#x27;]))
            if sanitized_product in excel_product or excel_product in sanitized_product:
                matched_row = idx + 2
                break
        if matched_row is None:
            print(f&quot;❌ 视频 {video_name} 未匹配到商品，跳过&quot;)
            continue
        target_number = matched_row - 1
        
        for suffix in [&#x27;_图1&#x27;, &#x27;_图2&#x27;]:
            excel_folder = f&quot;{sheet_name}{suffix}&quot;
            excel_path = os.path.join(ppt_root, excel_folder, f&quot;{excel_folder}.xlsx&quot;)
            if not os.path.exists(excel_path):
                print(f&quot;❌ 未找到Excel文件: {excel_path}，跳过&quot;)
                continue
            
            wb = load_workbook(excel_path)
            ws = wb.active
            for row in range(3, ws.max_row + 1):
                cell_val = ws.cell(row=row, column=2).value
                if cell_val and str(cell_val) == str(target_number):
                    ws.cell(row=row, column=2).value = &quot;&quot;
            wb.save(excel_path)
            print(f&quot;🔄 更新Excel {excel_folder}.xlsx: 清空编号 {target_number}&quot;)

def load_previous_results(ppt_root, selected_source_folder, category_excel):
    &quot;&quot;&quot;从已有的PPT文件夹中加载已处理的编号&quot;&quot;&quot;
    processing_results = {}
    if not os.path.exists(ppt_root):
        return processing_results
    
    # 加载 Excel，从编号查找商品名称
    xls = pd.ExcelFile(category_excel)
    sheet_name = xls.sheet_names[0]
    df = xls.parse(sheet_name)
    
    # 优先加载保存的源文件夹映射
    img_num_to_source_map = {}
    mapping_file = os.path.join(ppt_root, &#x27;img_num_to_source.json&#x27;)
    if os.path.exists(mapping_file):
        try:
            with open(mapping_file, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as f:
                img_num_to_source_map = json.load(f)
                # 转换为整数键
                img_num_to_source_map = {int(k): v for k, v in img_num_to_source_map.items()}
        except:
            pass
    
    # 扫描所有可用的源文件夹中的产品文件夹
    param_root = os.path.join(base_path, &quot;参数&quot;)
    img_num_to_folder_map = {}
    
    for sub_folder in [&#x27;详情图片&#x27;, &#x27;主图图片&#x27;]:
        source_folder = os.path.join(param_root, sub_folder)
        if not os.path.exists(source_folder):
            continue
        
        for folder_name in os.listdir(source_folder):
            folder_path = os.path.join(source_folder, folder_name)
            if not os.path.isdir(folder_path):
                continue
            
            # 文件夹格式: sheet_name_product_name
            if &#x27;_&#x27; not in folder_name:
                continue
            
            # 检查是否有图片
            try:
                images = [f for f in os.listdir(folder_path) if f.lower().endswith((&#x27;.jpg&#x27;, &#x27;.jpeg&#x27;, &#x27;.png&#x27;, &#x27;.gif&#x27;))]
                if images:
                    img_num_to_folder_map[folder_name] = folder_path
            except:
                continue
    
    # 从 PPT 文件夹中读取已处理的编号
    for folder_name in os.listdir(ppt_root):
        if &quot;_图1&quot; in folder_name:
            sheet_name_ppt = folder_name.replace(&quot;_图1&quot;, &quot;&quot;)
            # 从文件夹名称提取信息
            for img_file in os.listdir(os.path.join(ppt_root, folder_name)):
                if img_file.lower().endswith((&#x27;.jpg&#x27;, &#x27;.jpeg&#x27;, &#x27;.png&#x27;, &#x27;.gif&#x27;)):
                    try:
                        img_num = int(img_file.split(&#x27;.&#x27;)[0])
                        if img_num not in processing_results:
                            # 从 Excel 中查找编号对应的商品名称
                            product_name = None
                            if &#x27;商品名称&#x27; in df.columns:
                                try:
                                    product_name = df.iloc[img_num - 1][&#x27;商品名称&#x27;]
                                except:
                                    pass
                            
                            # 找到对应的产品文件夹
                            product_folder = None
                            if product_name:
                                # 洗治商品名称（移除特殊符号）
                                sanitized_product = sanitize_filename(str(product_name))
                                print(f&quot;\n🔍 [调试] 编号 {img_num}: 商品名称=&#x27;{product_name}&#x27;&quot;)
                                print(f&quot;   洗治后: &#x27;{sanitized_product}&#x27;&quot;)
                                print(f&quot;   扫描的文件夹数量: {len(img_num_to_folder_map)}&quot;)
                                
                                for folder_name_src, folder_path_src in img_num_to_folder_map.items():
                                    # 洗治文件夹名称
                                    sanitized_folder = sanitize_filename(folder_name_src)
                                    
                                    # 检查文件夹名是否匹配产品名称
                                    if sanitized_product in sanitized_folder or sanitized_folder in sanitized_product:
                                        print(f&quot;   ✅ 匹配成功: {folder_name_src}&quot;)
                                        product_folder = folder_path_src
                                        break
                                
                                if not product_folder:
                                    print(f&quot;   ❌ 没有找到匹配的文件夹&quot;)
                                    print(f&quot;   所有可用的文件夹 ({len(img_num_to_folder_map)} 个):&quot;)
                                    for i, folder_name_src in enumerate(list(img_num_to_folder_map.keys())[:10]):
                                        print(f&quot;     {i+1}. {folder_name_src}&quot;)
                            
                            processing_results[img_num] = {
                                &#x27;sheet_name&#x27;: sheet_name_ppt,
                                &#x27;folder_name&#x27;: folder_name,
                                &#x27;all_images&#x27;: [],
                                &#x27;img_ocr_dict&#x27;: {},
                                &#x27;folder_path&#x27;: product_folder,
                                &#x27;selected_main&#x27;: None,
                                &#x27;selected_detail&#x27;: None
                            }
                    except ValueError:
                        pass
    
    return processing_results

if __name__ == &quot;__main__&quot;:
    print(&quot;\n&quot; + &quot;=&quot;*60)
    print(&quot;📄 选择模式&quot;)
    print(&quot;=&quot;*60)
    print(&quot;1. 直接运行 - 自动处理所有图片，然后进入重选界面&quot;)
    print(&quot;2. 输入查找 - 选择文件夹，然后进入重选界面&quot;)
    print(&quot;=&quot;*60)
    
    while True:
        choice = input(&quot;\n请选择 (1 或 2): &quot;).strip()
        if choice in [&quot;1&quot;, &quot;2&quot;]:
            break
        print(&quot;❌ 无效的选择，请输入 1 或 2&quot;)
    
    # 根据选择设置基本路径，修改下面这个红色的路径就好了
    base_path = r&quot;E:\B站\选品\批量\暖冬好物1- 冲锋衣&quot;
    video_folder = os.path.join(base_path, &quot;视频&quot;)
    ppt_folder = os.path.join(base_path, &quot;PPT图片批量导入&quot;)
    category_excel = os.path.join(base_path, &quot;分类.xlsx&quot;)
    ref_excel = os.path.join(base_path, &quot;1-数据上传.xlsx&quot;)
    
    # 模式 1: 直接运行
    if choice == &quot;1&quot;:
        source_folder = os.path.join(base_path, &quot;参数&quot;, &quot;详情图片&quot;)
        ocr_txt_root = os.path.join(base_path, &quot;参数&quot;, &quot;详情图txt&quot;)
        selected_source_folder = source_folder
        
        print(&quot;\n✅ 开始直接运行...&quot;)
        print(&quot;=&quot;*60)
        print(&quot;开始处理图片...&quot;)
        processing_results = process_image_folders(source_folder, ppt_folder, category_excel, ocr_txt_root)
        
        if processing_results:
            print(&quot;\n&quot; + &quot;=&quot;*60)
            print(&quot;开始生成Excel...&quot;)
            write_to_excel(ppt_folder, ref_excel)
            
            # 交互式重选
            reselect_images_interactive(ppt_folder, source_folder, category_excel)
            
            # 重新生成Excel（如果有重选）
            print(&quot;\n&quot; + &quot;=&quot;*60)
            print(&quot;重新生成Excel...&quot;)
            write_to_excel(ppt_folder, ref_excel)
            
            print(&quot;\n&quot; + &quot;=&quot;*60)
            print(&quot;开始处理视频...&quot;)
            process_videos_and_update_excel(video_folder, ppt_folder, category_excel)
        
        print(&quot;\n&quot; + &quot;=&quot;*60)
        print(&quot;所有操作完成！&quot;)
    
    elif choice == &quot;2&quot;:
        print(&quot;\n✅ 开始输入查找...&quot;)
        print(&quot;=&quot;*60)
        print(&quot;📁 选择源文件夹&quot;)
        print(&quot;=&quot;*60)
        print(&quot;1. 参数/详情图片&quot;)
        print(&quot;2. 参数/主图图片&quot;)
        print(&quot;=&quot;*60)
        
        # 输入文件夹固定为详情图片
        selected_source_folder = os.path.join(base_path, &quot;参数&quot;, &quot;详情图片&quot;)
        
        # 选择输出文件夹类型
        print(&quot;=&quot;*60)
        print(&quot;📂 选择要重选的输出文件夹&quot;)
        print(&quot;=&quot;*60)
        print(&quot;1. 图1（主图）&quot;)
        print(&quot;2. 图2（详情图片）&quot;)
        print(&quot;=&quot;*60)
        
        while True:
            folder_choice = input(&quot;\n请选择 (1 或 2): &quot;).strip()
            if folder_choice == &quot;1&quot;:
                output_folder_type = &quot;图1&quot;
                break
            elif folder_choice == &quot;2&quot;:
                output_folder_type = &quot;图2&quot;
                break
            else:
                print(&quot;❌ 无效的选择，请输入 1 或 2&quot;)
        
        print(f&quot;\n✅ 已选择: {output_folder_type}&quot;)
        
        # 交互式重选
        reselect_images_interactive(ppt_folder, selected_source_folder, category_excel, output_folder_type)
        
        # 重新生成Excel
        print(&quot;\n&quot; + &quot;=&quot;*60)
        print(&quot;重新生成Excel...&quot;)
        write_to_excel(ppt_folder, ref_excel)
        
        print(&quot;\n&quot; + &quot;=&quot;*60)
        print(&quot;所有操作完成！&quot;)


![](img/c37541a0b0e97f3dbe70a32f080eb85f.png)


![](img/9fd2ad3aba936844fe918bc6f8bd3cb8.png)


![](img/28f367566d6bd8c8fb49fbf042c9aa1f.png)


![](img/6365ad05c012fb9d1672b94f0b7b07bf.png)


![](img/2fd95efeff0207f47be75ba9b33fb757.png)

![](img/b75aa19a7bc42170a1f07e04137a8beb.png)

```
**使用方法：**

安装完基本的模块后，修改下面的红色路径。

还需要把 1-数据上传.xlsx  放到文件夹下面。
[1-数据上传.xlsx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Y3iLbPLWNodtD9xXbohcIbFKnfc)

运行代码，会得到如下的结果，**图1是主图**，图2是详情图片（不用管）

如果主图里面有不符合的，代码里面有切换的功能。
得到主图后，找到3.1批量套版得到的Excel，先把**这个Exce**l放到**主图文件夹**中，如图中的冲锋衣_图1，注意主图文件夹中会有一个代码生成的Excel，可以直接删掉。

然后再编辑Excel，在里面加入 图片_1这一列。 图片_1这一列下面的数字填写为1,2,3,4等 ，和文件夹中的图片命名一致即可。 接下来就是把 冲锋衣_图1这个文件夹 压缩成zip文件，导入批量套版即可。
### 3.3 broll 素材提效

还是在我的方法基础上，影刀代码生成的参数文件夹里面，有几个子**文件夹，有些为空，因为这**是其他影刀代码如2.3的批量**获取产品参数会用**到的。我现在基本上都只用2.5的影刀代码生成。
写了一个代码，可以在参数文件夹的基础上，对应每一句话得到对应的图片。这样Broll的一部分素材就有了。

代码如下：

```
import os
import re
import json
import subprocess
from pathlib import Path
from typing import List, Dict, Tuple, Optional
import tkinter as tk
from tkinter import ttk, filedialog, messagebox
from PIL import Image, ImageTk
import difflib
from collections import defaultdict
import jieba
try:
    import win32clipboard
    HAS_WIN32 = True
except ImportError:
    HAS_WIN32 = False
import io

class SentenceImageMatcher:
    &quot;&quot;&quot;句子与图片匹配引擎&quot;&quot;&quot;
    
    def __init__(self, param_folder_path: str):
        self.param_folder = Path(param_folder_path)
        self.txt_folder = self.param_folder / &quot;详情图txt&quot;
        self.img_folder = self.param_folder / &quot;详情图片&quot;
        
        self.txt_data = {}  # {产品名: {图片名: [ocr文字列表]}}
        self.img_paths = {}  # {产品名: {图片名: 图片路径}}
        
        self._load_data()
    
    def _load_data(self):
        &quot;&quot;&quot;加载txt和图片数据&quot;&quot;&quot;
        if not self.txt_folder.exists():
            raise ValueError(f&quot;详情图txt文件夹不存在: {self.txt_folder}&quot;)
        if not self.img_folder.exists():
            raise ValueError(f&quot;详情图片文件夹不存在: {self.img_folder}&quot;)
        
        # 加载txt文件
        for txt_file in self.txt_folder.glob(&quot;*.txt&quot;):
            product_name = self._extract_product_name(txt_file.name)
            self.txt_data[product_name] = self._parse_txt_file(txt_file)
        
        # 加载图片文件夹
        for img_dir in self.img_folder.iterdir():
            if img_dir.is_dir():
                product_name = self._extract_product_name(img_dir.name)
                self.img_paths[product_name] = {}
                for img_file in img_dir.glob(&quot;*&quot;):
                    if img_file.suffix.lower() in [&#x27;.jpg&#x27;, &#x27;.jpeg&#x27;, &#x27;.png&#x27;, &#x27;.gif&#x27;, &#x27;.bmp&#x27;]:
                        img_name = img_file.stem  # 不含扩展名
                        self.img_paths[product_name][img_name] = str(img_file)
    
    def _extract_product_name(self, filename: str) -&gt; str:
        &quot;&quot;&quot;从文件名提取产品名称&quot;&quot;&quot;
        # 格式: 前缀_产品名称
        # 移除 .txt 扩展名
        filename = filename.replace(&#x27;.txt&#x27;, &#x27;&#x27;)
        
        # 尝试按 &#x27;_&#x27; 拆分，但不是所有文件名都有 &#x27;_&#x27;
        # 有些文件名可能是完整的产品名称，没有前缀
        if &#x27;_&#x27; in filename:
            # 如果有 &#x27;_&#x27;，返回整个文件名（包括前缀）
            # 这样可以正确匹配长文件名
            return filename
        return filename
    
    def _parse_txt_file(self, txt_file: Path) -&gt; Dict[str, List[str]]:
        &quot;&quot;&quot;解析txt文件，提取每张图片的OCR文字&quot;&quot;&quot;
        result = {}
        with open(txt_file, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as f:
            content = f.read()
        
        # 按照 === 根目录 - 图片名.jpg === 分割（支持大写和小写）
        pattern = r&#x27;=== 根目录 - (.+?\.(jpg|JPG|jpeg|JPEG)) ===\n.*?\n.*?\n--+\n\[(.+?)\]&#x27;
        matches = re.findall(pattern, content, re.DOTALL)
        
        for match in matches:
            img_name = match[0]  # 完整的图片名（包含扩展名）
            ocr_text = match[2]  # OCR文字
            img_key = img_name.rsplit(&#x27;.&#x27;, 1)[0]  # 去掉扩展名
            # 解析列表格式的OCR文字
            try:
                ocr_list = eval(&#x27;[&#x27; + ocr_text + &#x27;]&#x27;)
                result[img_key] = ocr_list
            except:
                result[img_key] = [ocr_text]
        
        return result
    
    def match_sentence_to_images(self, sentence: str, product_name: str, threshold: float = 0.3) -&gt; List[Tuple[str, float, str]]:
        &quot;&quot;&quot;
        匹配句子到图片
        返回: [(图片名, 相似度, 图片路径), ...]
        &quot;&quot;&quot;
        if product_name not in self.txt_data:
            return []
        
        txt_data = self.txt_data[product_name]
        img_paths = self.img_paths.get(product_name, {})
        
        results = []
        
        for img_name, ocr_list in txt_data.items():
            if img_name not in img_paths:
                continue
            
            # 合并OCR文字
            ocr_text = &#x27; &#x27;.join(str(item) for item in ocr_list)
            
            # 计算相似度
            similarity = self._calculate_similarity(sentence, ocr_text)
            
            if similarity &gt;= threshold:  # 使用阈值筛选
                results.append((img_name, similarity, img_paths[img_name]))
        
        # 按相似度排序
        results.sort(key=lambda x: x[1], reverse=True)
        return results
    
    def match_sentence_to_all_images(self, sentence: str, threshold: float = 0.3) -&gt; List[Tuple[str, float, str, str]]:
        &quot;&quot;&quot;
        从所有产品中匹配句子到图片
        返回: [(产品名, 图片名, 相似度, 图片路径), ...]
        &quot;&quot;&quot;
        all_results = []
        
        for product_name in self.txt_data.keys():
            matches = self.match_sentence_to_images(sentence, product_name, threshold)
            for img_name, similarity, img_path in matches:
                all_results.append((product_name, img_name, similarity, img_path))
        
        # 按相似度排序
        all_results.sort(key=lambda x: x[2], reverse=True)
        return all_results
    
    def _calculate_similarity(self, sentence: str, ocr_text: str) -&gt; float:
        &quot;&quot;&quot;计算句子与OCR文字的相似度&quot;&quot;&quot;
        # 使用jieba分词提取关键词
        sentence_words = list(jieba.cut(sentence))
        ocr_words = list(jieba.cut(ocr_text))
        
        # 过滤掉太短的词（長度&lt;2）和空白
        sentence_words = [w for w in sentence_words if len(w) &gt;= 2 and w.strip()]
        ocr_words = [w for w in ocr_words if len(w) &gt;= 2 and w.strip()]
        
        if not sentence_words or not ocr_words:
            return 0
        
        # 计算重叠词汇比例
        overlap = len(set(sentence_words) &amp; set(ocr_words))
        max_len = max(len(sentence_words), len(ocr_words))
        
        keyword_score = overlap / max_len if max_len &gt; 0 else 0
        
        # 计算字符串相似度
        string_similarity = difflib.SequenceMatcher(None, sentence, ocr_text).ratio()
        
        # 综合评分（关键词权重提高到70%）
        return keyword_score * 0.7 + string_similarity * 0.3
    
    def split_sentences(self, text: str) -&gt; List[str]:
        &quot;&quot;&quot;将文本拆分成句子（使用高级拆分逻辑）&quot;&quot;&quot;
        # 第一步：清洗文本
        text = re.sub(r&quot;\s+&quot;, &quot; &quot;, text.strip())
        # 合并连续的句末标点
        text = re.sub(r&quot;([\u3002\uff01\uff1f])\1+&quot;, r&quot;\1&quot;, text)
        
        # 第二步：标记不可拆分区域（括号、引号、数字等）
        non_split_pairs = [
            (&quot;（&quot;, &quot;）&quot;), (&quot;(&quot;, &quot;)&quot;), (&quot;【&quot;, &quot;】&quot;), (&quot;[&quot;, &quot;]&quot;),
            (&quot;“&quot;, &quot;”&quot;), (&quot;&#x27;&quot;, &quot;&#x27;&quot;), (&#x27;&quot;&#x27;, &#x27;&quot;&#x27;), (&quot;——&quot;, &quot;——&quot;), (&quot;-&quot;, &quot;-&quot;)
        ]
        non_split_map = {}
        for idx, (left, right) in enumerate(non_split_pairs):
            left_esc = re.escape(left)
            right_esc = re.escape(right)
            pattern = f&quot;({left_esc}.*?{right_esc})&quot;
            for match in re.finditer(pattern, text):
                placeholder = f&quot;【NS_{idx}_{match.start()}】&quot;
                non_split_map[placeholder] = match.group(1)
                text = text.replace(match.group(1), placeholder, 1)
        
        # 第三步：按中文句号、分号拆分
        # 按中文句号、感叹号、问号、分号拆分
        sentences = re.split(r&#x27;[\u3002\uff01\uff1f\uff1b;]&#x27;, text)
        
        # 第四步：恢复不可拆分区域
        sentences = [s.strip() for s in sentences if s.strip()]
        sentences = [self._restore_non_split_regions(s, non_split_map) for s in sentences]
        
        # 第五步：过滤掉太短的句子（低于5个字）
        sentences = [s for s in sentences if len(s) &gt;= 5]
        
        return sentences
    
    def _restore_non_split_regions(self, text: str, non_split_map: Dict[str, str]) -&gt; str:
        &quot;&quot;&quot;恢复不可拆分区域&quot;&quot;&quot;
        for placeholder, original in non_split_map.items():
            text = text.replace(placeholder, original)
        return text
    
    def get_all_products(self) -&gt; List[str]:
        &quot;&quot;&quot;获取所有产品名称&quot;&quot;&quot;
        return list(self.txt_data.keys())

class SentenceImageMatcherGUI:
    &quot;&quot;&quot;GUI界面&quot;&quot;&quot;
    
    def __init__(self, root):
        self.root = root
        self.root.title(&quot;句子配图工具&quot;)
        self.root.geometry(&quot;1200x800&quot;)
        self.root.configure(bg=&#x27;#f5f5f5&#x27;)
        
        # 设置字体
        self.font_title = (&#x27;Segoe UI&#x27;, 12, &#x27;bold&#x27;)
        self.font_normal = (&#x27;Segoe UI&#x27;, 10)
        self.font_small = (&#x27;Segoe UI&#x27;, 9)
        
        # 设置颜色
        self.color_primary = &#x27;#2196F3&#x27;      # 主色（蓝色）
        self.color_bg = &#x27;#f5f5f5&#x27;           # 背景色
        self.color_text = &#x27;#333333&#x27;         # 文字色
        self.color_border = &#x27;#e0e0e0&#x27;       # 边框色
        self.color_hover = &#x27;#f0f0f0&#x27;        # 悬停色
        
        self.matcher = None
        self.sentences = []
        self.current_sentence_idx = 0
        self.current_product = None
        self.current_matches = []
        self.current_match_idx = 0
        self.similarity_threshold = 0.09  # 相似度阈值（默认 0.09 = 9%）
        
        self._create_widgets()
    
    def _create_widgets(self):
        &quot;&quot;&quot;创建UI组件&quot;&quot;&quot;
        # 顶部：文件夹选择
        top_frame = ttk.Frame(self.root)
        top_frame.pack(fill=tk.X, padx=10, pady=10)
        
        ttk.Label(top_frame, text=&quot;参数文件夹:&quot;).pack(side=tk.LEFT)
        self.folder_var = tk.StringVar()
        ttk.Entry(top_frame, textvariable=self.folder_var, width=60).pack(side=tk.LEFT, padx=5)
        ttk.Button(top_frame, text=&quot;浏览&quot;, command=self._select_folder).pack(side=tk.LEFT)
        ttk.Button(top_frame, text=&quot;加载&quot;, command=self._load_folder).pack(side=tk.LEFT, padx=5)
        
        # 相似度阈值控件（百分比指示）
        ttk.Separator(top_frame, orient=tk.VERTICAL).pack(side=tk.LEFT, fill=tk.Y, padx=10)
        ttk.Label(top_frame, text=&quot;相似度阈值 (%):&quot;).pack(side=tk.LEFT, padx=(0, 5))
        self.threshold_var = tk.DoubleVar(value=9)  # 改为百分比：9%
        self.threshold_scale = ttk.Scale(top_frame, from_=0, to=100, orient=tk.HORIZONTAL, 
                                         variable=self.threshold_var, command=self._on_threshold_change,
                                         length=150)
        self.threshold_scale.pack(side=tk.LEFT, padx=5)
        self.threshold_entry_var = tk.StringVar(value=&quot;9&quot;)  # 改为百分比
        self.threshold_entry = ttk.Entry(top_frame, textvariable=self.threshold_entry_var, width=5)
        self.threshold_entry.pack(side=tk.LEFT, padx=5)
        ttk.Label(top_frame, text=&quot;%&quot;).pack(side=tk.LEFT, padx=(0, 5))
        self.threshold_entry.bind(&#x27;&lt;KeyRelease&gt;&#x27;, self._on_threshold_entry_change)
        
        # 中间：左侧文本输入，右侧图片显示
        main_frame = ttk.Frame(self.root)
        main_frame.pack(fill=tk.BOTH, expand=True, padx=10, pady=10)
        
        # 左侧：输入文本和句子列表
        left_frame = ttk.Frame(main_frame)
        left_frame.pack(side=tk.LEFT, fill=tk.BOTH, expand=True, padx=(0, 10))
        
        ttk.Label(left_frame, text=&quot;输入文本:&quot;, font=self.font_title, foreground=self.color_primary).pack(anchor=tk.W)
        self.text_input = tk.Text(left_frame, height=8, width=50, font=self.font_normal, 
                                  bg=&#x27;white&#x27;, fg=self.color_text, insertbackground=self.color_primary,
                                  relief=tk.FLAT, borderwidth=1)
        self.text_input.pack(fill=tk.BOTH, expand=True, pady=5)
        
        button_frame = ttk.Frame(left_frame)
        button_frame.pack(anchor=tk.W, pady=5)
        ttk.Button(button_frame, text=&quot;拆分句子&quot;, command=self._split_sentences).pack(side=tk.LEFT, padx=2)
        ttk.Button(button_frame, text=&quot;更新列表&quot;, command=self._update_sentence_list).pack(side=tk.LEFT, padx=2)
        
        ttk.Label(left_frame, text=&quot;句子列表:&quot;, font=self.font_title, foreground=self.color_primary).pack(anchor=tk.W, pady=(10, 0))
        
        # 句子列表框
        list_frame = ttk.Frame(left_frame)
        list_frame.pack(fill=tk.BOTH, expand=True)
        
        scrollbar = ttk.Scrollbar(list_frame)
        scrollbar.pack(side=tk.RIGHT, fill=tk.Y)
        
        self.sentence_listbox = tk.Listbox(list_frame, yscrollcommand=scrollbar.set, font=self.font_normal,
                                           bg=&#x27;white&#x27;, fg=self.color_text, selectbackground=self.color_primary,
                                           selectforeground=&#x27;white&#x27;, relief=tk.FLAT, borderwidth=1)
        self.sentence_listbox.pack(side=tk.LEFT, fill=tk.BOTH, expand=True)
        self.sentence_listbox.bind(&#x27;&lt;&lt;ListboxSelect&gt;&gt;&#x27;, self._on_sentence_select)
        scrollbar.config(command=self.sentence_listbox.yview)
        
        # 右侧：产品选择和图片显示
        right_frame = ttk.Frame(main_frame)
        right_frame.pack(side=tk.RIGHT, fill=tk.BOTH, expand=True)
        
        ttk.Label(right_frame, text=&quot;选择产品:&quot;, font=self.font_title, foreground=self.color_primary).pack(anchor=tk.W)
        self.product_var = tk.StringVar()
        self.all_products = []  # 保存所有产品列表
        self.product_combo = ttk.Combobox(right_frame, textvariable=self.product_var, font=self.font_normal)
        self.product_combo.pack(fill=tk.X, pady=5)
        self.product_combo.bind(&#x27;&lt;&lt;ComboboxSelected&gt;&gt;&#x27;, self._on_product_select)
        self.product_combo.bind(&#x27;&lt;KeyRelease&gt;&#x27;, self._on_product_input)  # 添加模糊匹配
        
        # 图片昺示
        ttk.Label(right_frame, text=&quot;匹配图片:&quot;, font=self.font_title, foreground=self.color_primary).pack(anchor=tk.W, pady=(5, 0))
        
        # 使用grid布局以不让内容自动调整大小
        image_container = tk.Frame(right_frame, bg=&#x27;#e8e8e8&#x27;, relief=tk.FLAT, borderwidth=0, height=500, width=600)
        image_container.pack(fill=tk.BOTH, expand=False, pady=3)
        image_container.pack_propagate(False)  # 固定大小，防止移位
        
        self.image_canvas = tk.Canvas(image_container, bg=&#x27;#e8e8e8&#x27;, highlightthickness=0)
        self.image_canvas.pack(fill=tk.BOTH, expand=True)
        
        # 图片信息和导航（固定高度，防止移位）
        info_frame = tk.Frame(right_frame, height=85, bg=&#x27;white&#x27;)
        info_frame.pack(fill=tk.X, pady=3)
        info_frame.pack_propagate(False)  # 不让框根据内容自动调整大小
        
        self.image_info_label = tk.Label(info_frame, text=&quot;&quot;, justify=tk.LEFT, font=self.font_small, 
                                         bg=&#x27;white&#x27;, fg=self.color_text)
        self.image_info_label.pack(anchor=tk.NW, padx=5, pady=5, fill=tk.BOTH, expand=True)
        
        # 导航按钮
        nav_frame = tk.Frame(right_frame, height=40, bg=&#x27;#f5f5f5&#x27;)
        nav_frame.pack(fill=tk.X, pady=3)
        nav_frame.pack_propagate(False)  # 固定高度
        
        ttk.Button(nav_frame, text=&quot;&lt; 上一张&quot;, command=self._prev_image).pack(side=tk.LEFT, padx=2, pady=5)
        ttk.Button(nav_frame, text=&quot;下一张 &gt;&quot;, command=self._next_image).pack(side=tk.LEFT, padx=2, pady=5)
        ttk.Button(nav_frame, text=&quot;复制图片&quot;, command=self._copy_image).pack(side=tk.LEFT, padx=2, pady=5)
        ttk.Button(nav_frame, text=&quot;清空&quot;, command=self._clear_image).pack(side=tk.LEFT, padx=2, pady=5)
        
        # 底部：状态栏
        self.status_var = tk.StringVar(value=&quot;就绪&quot;)
        status_bar = ttk.Label(self.root, textvariable=self.status_var, relief=tk.SUNKEN)
        status_bar.pack(fill=tk.X, side=tk.BOTTOM)
    
    def _on_threshold_change(self, value):
        &quot;&quot;&quot;阈值滑块变化事件（百分比）&quot;&quot;&quot;
        percentage = float(value)
        self.similarity_threshold = percentage / 100.0  # 转换为小数
        self.threshold_entry_var.set(f&quot;{percentage:.0f}&quot;)
        # 重新更新匹配结果
        self._update_matches()
    
    def _on_threshold_entry_change(self, event):
        &quot;&quot;&quot;阈值输入框变化事件（百分比）&quot;&quot;&quot;
        try:
            percentage = float(self.threshold_entry_var.get())
            # 限制范围在 0 - 100
            if percentage &lt; 0:
                percentage = 0
            elif percentage &gt; 100:
                percentage = 100
            self.similarity_threshold = percentage / 100.0  # 转换为小数
            self.threshold_var.set(percentage)
            # 重新更新匹配结果
            self._update_matches()
        except ValueError:
            # 如果输入不是数字，忽略
            pass
    
    def _select_folder(self):
        &quot;&quot;&quot;选择参数文件夹&quot;&quot;&quot;
        folder = filedialog.askdirectory(title=&quot;选择参数文件夹&quot;)
        if folder:
            self.folder_var.set(folder)
    
    def _load_folder(self):
        &quot;&quot;&quot;加载参数文件夹&quot;&quot;&quot;
        folder = self.folder_var.get()
        if not folder:
            messagebox.showwarning(&quot;提示&quot;, &quot;请先选择参数文件夹&quot;)
            return
        
        try:
            self.matcher = SentenceImageMatcher(folder)
            products = self.matcher.get_all_products()
            
            # 检查是否成功加载了数据
            if not products:
                # 详细诊断
                txt_folder = Path(folder) / &quot;详情图 txt&quot;
                img_folder = Path(folder) / &quot;详情图片&quot;
                
                error_msg = &quot;数据加载失败:\n\n&quot;
                
                if not txt_folder.exists():
                    error_msg += f&quot;❌ 详情图 txt 文件夹不存在\n   预期位置: {txt_folder}\n\n&quot;
                else:
                    txt_files = list(txt_folder.glob(&quot;*.txt&quot;))
                    error_msg += f&quot;✓ 详情图 txt 文件夹存在 ({len(txt_files)} 个 txt 文件)\n\n&quot;
                
                if not img_folder.exists():
                    error_msg += f&quot;❌ 详情图片 文件夹不存在\n   预期位置: {img_folder}\n\n&quot;
                else:
                    img_dirs = [d for d in img_folder.iterdir() if d.is_dir()]
                    error_msg += f&quot;✓ 详情图片 文件夹存在 ({len(img_dirs)} 个产品文件夹)\n\n&quot;
                
                error_msg += &quot;解决方案:\n&quot;
                error_msg += &quot;1. 运行 &#x27;\u521d\u59cb\u5316\u53c2\u6570\u6587\u4ef6\u5939.py&#x27; 来创\u5efa文件\u5939\u7ed3\u6784\n&quot;
                error_msg += &quot;2. 将 txt 文件放入 &#x27;\u8be6\u60c5\u56fe txt&#x27; 文件\u5939\n&quot;
                error_msg += &quot;3. 在 &#x27;\u8be6\u60c5\u56fe\u7247&#x27; 中创\u5efa产\u54c1文件\u5939\uff0c放入\u56fe\u7247\n&quot;
                
                messagebox.showerror(&quot;错误&quot;, error_msg)
                self.status_var.set(&quot;数据加载失败&quot;)
                return
            
            # 添加&quot;全选&quot;选项
            self.all_products = [&quot;【全选】所有产品&quot;] + products
            self.product_combo[&#x27;values&#x27;] = self.all_products
            if self.all_products:
                self.product_combo.current(0)
                self.current_product = &quot;【全选】所有产品&quot;
            self.status_var.set(f&quot;已加载 {len(products)} 个产品&quot;)
        except ValueError as e:
            # 文件夹不存在的错误
            error_msg = f&quot;{str(e)}\n\n解决方案:\n&quot;
            error_msg += &quot;1. 运行 &#x27;\u521d\u59cb\u5316\u53c2\u6570\u6587\u4ef6\u5939.py&#x27;\n&quot;
            error_msg += &quot;2. 或者手动创\u5efa &#x27;\u8be6\u60c5\u56fe txt&#x27; 和 &#x27;\u8be6\u60c5\u56fe\u7247&#x27; 文\u4ef6\u5939&quot;
            messagebox.showerror(&quot;错误&quot;, error_msg)
            self.status_var.set(&quot;加载失败&quot;)
        except Exception as e:
            messagebox.showerror(&quot;错误&quot;, f&quot;加载失败: {str(e)}&quot;)
            self.status_var.set(&quot;加载失败&quot;)
    
    def _split_sentences(self):
        &quot;&quot;&quot;拆分句子，並在输入框中按换行显示&quot;&quot;&quot;
        if not self.matcher:
            messagebox.showwarning(&quot;提示&quot;, &quot;请先加载参数文件夹&quot;)
            return
        
        text = self.text_input.get(&quot;1.0&quot;, tk.END).strip()
        if not text:
            messagebox.showwarning(&quot;提示&quot;, &quot;请输入文本&quot;)
            return
        
        # 拆分句子
        self.sentences = self.matcher.split_sentences(text)
        
        # 更新输入框：每句话一行
        self.text_input.delete(&quot;1.0&quot;, tk.END)
        self.text_input.insert(&quot;1.0&quot;, &quot;\n&quot;.join(self.sentences))
        
        # 更新句子列表
        self.sentence_listbox.delete(0, tk.END)
        for i, sentence in enumerate(self.sentences, 1):
            self.sentence_listbox.insert(tk.END, f&quot;{i}. {sentence}&quot;)
        
        self.current_sentence_idx = 0
        self.status_var.set(f&quot;已拆分 {len(self.sentences)} 个句子，你可以在上方输入框中调整&quot;)
        
        if self.sentences:
            self.sentence_listbox.selection_set(0)
            self._on_sentence_select(None)
    
    def _update_sentence_list(self):
        &quot;&quot;&quot;从输入框中更新句子列表（按换行拆分）&quot;&quot;&quot;
        text = self.text_input.get(&quot;1.0&quot;, tk.END).strip()
        if not text:
            messagebox.showwarning(&quot;提示&quot;, &quot;输入框为空&quot;)
            return
        
        # 按换行拆分（一个换行 = 一句话）
        self.sentences = [s.strip() for s in text.split(&#x27;\n&#x27;) if s.strip()]
        
        # 更新句子列表
        self.sentence_listbox.delete(0, tk.END)
        for i, sentence in enumerate(self.sentences, 1):
            self.sentence_listbox.insert(tk.END, f&quot;{i}. {sentence}&quot;)
        
        self.current_sentence_idx = 0
        self.status_var.set(f&quot;已更新 {len(self.sentences)} 个句子&quot;)
        
        if self.sentences:
            self.sentence_listbox.selection_set(0)
            self._on_sentence_select(None)
    
    def _on_sentence_select(self, event):
        &quot;&quot;&quot;句子选择事件&quot;&quot;&quot;
        selection = self.sentence_listbox.curselection()
        if selection:
            self.current_sentence_idx = selection[0]
            self._update_matches()
    
    def _on_product_input(self, event):
        &quot;&quot;&quot;产品输入框模糊匹配事件&quot;&quot;&quot;
        input_text = self.product_var.get()
        
        # 如果输入为空，显示所有产品
        if not input_text:
            self.product_combo[&#x27;values&#x27;] = self.all_products
            return
        
        # 模糊匹配：不区分大小写，包含匹配
        filtered_products = [
            p for p in self.all_products 
            if input_text.lower() in p.lower()
        ]
        
        # 更新下拉框选项
        self.product_combo[&#x27;values&#x27;] = filtered_products if filtered_products else self.all_products
    
    def _on_product_select(self, event):
        &quot;&quot;&quot;产品选择事件&quot;&quot;&quot;
        self.current_product = self.product_var.get()
        self._update_matches()
    
    def _update_matches(self):
        &quot;&quot;&quot;更新匹配结果&quot;&quot;&quot;
        if not self.sentences or self.current_sentence_idx &gt;= len(self.sentences):
            return
        
        sentence = self.sentences[self.current_sentence_idx]
        
        if not self.current_product:
            return
        
        # 如果选择&quot;全选&quot;，从所有产品中筛选
        if self.current_product == &quot;【全选】所有产品&quot;:
            self.current_matches = self.matcher.match_sentence_to_all_images(sentence, self.similarity_threshold)
        else:
            self.current_matches = self.matcher.match_sentence_to_images(sentence, self.current_product, self.similarity_threshold)
        
        self.current_match_idx = 0
        
        if self.current_matches:
            self._display_image()
        else:
            self._clear_image()
            self.image_info_label.config(text=&quot;未找到匹配的图片&quot;)
    
    def _display_image(self):
        &quot;&quot;&quot;显示当前图片&quot;&quot;&quot;
        if not self.current_matches or self.current_match_idx &gt;= len(self.current_matches):
            return
        
        match_data = self.current_matches[self.current_match_idx]
        
        # 处理两种数据格式：全选模式是4元组，单产品模式是3元组
        if len(match_data) == 4:
            product_name, img_name, similarity, img_path = match_data
            info_prefix = f&quot;产品: {product_name}\n&quot;
        else:
            img_name, similarity, img_path = match_data
            info_prefix = &quot;&quot;
        
        try:
            # 检查图片文件是否存在
            if not os.path.exists(img_path):
                raise FileNotFoundError(f&quot;图片文件不存在: {img_path}&quot;)
            
            # 加载并显示图片
            img = Image.open(img_path)
            
            # 根据容器大小缩放（容器600x500px）
            # 保持完整的宽高比
            container_width = 590
            container_height = 490
            
            # 计算缩放比例，确保图片完全展示
            img_ratio = img.width / img.height
            container_ratio = container_width / container_height
            
            if img_ratio &gt; container_ratio:
                # 图片较宽，按宽度限制
                new_width = container_width
                new_height = int(container_width / img_ratio)
            else:
                # 图片较窄，按高度限制
                new_height = container_height
                new_width = int(container_height * img_ratio)
            
            img = img.resize((new_width, new_height), Image.Resampling.LANCZOS)
            photo = ImageTk.PhotoImage(img)
            
            # 使用Canvas显示图片，并居中显示
            self.image_canvas.delete(&#x27;all&#x27;)
            
            # 强制更新Canvas以获取正确的宽高
            self.image_canvas.update_idletasks()
            canvas_width = self.image_canvas.winfo_width()
            canvas_height = self.image_canvas.winfo_height()
            
            # 如果canvas仍未初始化，使用容器大小
            if canvas_width &lt;= 1:
                canvas_width = 590
            if canvas_height &lt;= 1:
                canvas_height = 490
            
            x = canvas_width / 2
            y = canvas_height / 2
            self.image_canvas.create_image(x, y, image=photo)
            # 保持PhotoImage引用，防止被垃圾回收
            if not hasattr(self, &#x27;_photo_images&#x27;):
                self._photo_images = []
            self._photo_images.append(photo)
            # 保留最多10张图片的引用
            if len(self._photo_images) &gt; 10:
                self._photo_images.pop(0)
            
            # 更新信息
            similarity_pct = similarity * 100
            current_idx = self.current_match_idx + 1
            total_count = len(self.current_matches)
            
            # 构建更清晰的信息文本
            info_lines = []
            if info_prefix:
                info_lines.append(info_prefix.strip())
            info_lines.append(f&quot;图片: {img_name}&quot;)
            info_lines.append(f&quot;相似度: {similarity_pct:.1f}%&quot;)
            info_lines.append(f&quot;进度: {current_idx}/{total_count}&quot;)
            
            info_text = &quot;\n&quot;.join(info_lines)
            self.image_info_label.config(text=info_text)
            
            self.current_image_path = img_path
        except FileNotFoundError as e:
            self.image_info_label.config(text=f&quot;文件不存在: {str(e)}&quot;)
            self.image_canvas.delete(&#x27;all&#x27;)
        except Exception as e:
            self.image_info_label.config(text=f&quot;加载图片失败: {str(e)}&quot;)
            self.image_canvas.delete(&#x27;all&#x27;)
    
    def _prev_image(self):
        &quot;&quot;&quot;上一张图片&quot;&quot;&quot;
        if self.current_matches and self.current_match_idx &gt; 0:
            self.current_match_idx -= 1
            self._display_image()
    
    def _next_image(self):
        &quot;&quot;&quot;下一张图片&quot;&quot;&quot;
        if self.current_matches and self.current_match_idx &lt; len(self.current_matches) - 1:
            self.current_match_idx += 1
            self._display_image()
    
    def _copy_image(self):
        &quot;&quot;&quot;复制图片到剪贴板&quot;&quot;&quot;
        if not hasattr(self, &#x27;current_image_path&#x27;) or self.current_image_path is None:
            messagebox.showwarning(&quot;提示&quot;, &quot;没有要复制的图片&quot;)
            return
        
        if not os.path.exists(self.current_image_path):
            messagebox.showerror(&quot;错误&quot;, &quot;图片文件不存在&quot;)
            return
        
        # 优先级：1. win32clipboard  2. ctypes  3. PowerShell Set-Clipboard  4. clip.exe  5. 直接写入剪贴板
        try:
            if HAS_WIN32:
                self._copy_image_win32()
            else:
                self._copy_image_fallback()
        except Exception as e1:
            try:
                self._copy_image_fallback()
            except Exception as e2:
                try:
                    self._copy_image_powershell()
                except Exception as e3:
                    try:
                        self._copy_image_via_clipboard_file()
                    except Exception as e4:
                        try:
                            self._copy_image_via_clip_exe()
                        except Exception as e5:
                            messagebox.showerror(&quot;错误&quot;, f&quot;所有复制方案均失败:\n1. win32: {str(e1)}\n2. ctypes: {str(e2)}\n3. PowerShell: {str(e3)}\n4. Set-Clipboard: {str(e4)}\n5. clip.exe: {str(e5)}&quot;)
    
    def _copy_image_win32(self):
        &quot;&quot;&quot;使用 win32clipboard 复制图片&quot;&quot;&quot;
        try:
            img = Image.open(self.current_image_path)
            
            output = io.BytesIO()
            img.convert(&#x27;RGB&#x27;).save(output, &#x27;BMP&#x27;)
            bmp_data = output.getvalue()
            output.close()
            
            # BMP文件格式：前14字节是文件头，需要去掉
            dib_data = bmp_data[14:]
            
            win32clipboard.OpenClipboard()
            win32clipboard.EmptyClipboard()
            win32clipboard.SetClipboardData(win32clipboard.CF_DIB, dib_data)
            win32clipboard.CloseClipboard()
            
            messagebox.showinfo(&quot;成功&quot;, &quot;图片已复制到剪贴板&quot;)
        except Exception as e:
            raise Exception(f&quot;win32clipboard 复制失败: {str(e)}&quot;)
    
    def _copy_image_fallback(self):
        &quot;&quot;&quot;备用方案：使用 ctypes 直接操作 Windows 剪贴板&quot;&quot;&quot;
        import ctypes
        
        try:
            img = Image.open(self.current_image_path)
            
            output = io.BytesIO()
            img.convert(&#x27;RGB&#x27;).save(output, &#x27;BMP&#x27;)
            bmp_data = output.getvalue()
            output.close()
            
            # 去掉 BMP 文件头（前14字节）
            dib_data = bmp_data[14:]
            
            user32 = ctypes.windll.user32
            kernel32 = ctypes.windll.kernel32
            
            # 打开剪贴板
            if not user32.OpenClipboard(None):
                raise Exception(&quot;打开剪贴板失败&quot;)
            
            # 清空剪贴板
            user32.EmptyClipboard()
            
            # 分配全局内存
            size = len(dib_data)
            hglobal = kernel32.GlobalAlloc(0x0002, size)
            if not hglobal:
                user32.CloseClipboard()
                raise Exception(&quot;分配内存失败&quot;)
            
            # 锁定内存并复制数据
            ptr = kernel32.GlobalLock(hglobal)
            if not ptr:
                kernel32.GlobalFree(hglobal)
                user32.CloseClipboard()
                raise Exception(&quot;锁定内存失败&quot;)
            
            ctypes.memmove(ptr, dib_data, size)
            kernel32.GlobalUnlock(hglobal)
            
            # 设置剪贴板数据（CF_DIB = 8）
            if not user32.SetClipboardData(8, hglobal):
                kernel32.GlobalFree(hglobal)
                user32.CloseClipboard()
                raise Exception(&quot;设置剪贴板数据失败&quot;)
            
            user32.CloseClipboard()
            messagebox.showinfo(&quot;成功&quot;, &quot;图片已复制到剪贴板&quot;)
        except Exception as e:
            raise Exception(f&quot;ctypes 复制失败: {str(e)}&quot;)
    
    def _copy_image_powershell(self):
        &quot;&quot;&quot;第三个备选：使用 PowerShell 复制图片&quot;&quot;&quot;
        import subprocess
        
        try:
            # 转换路径为 Windows 格式（使用原始路径）
            img_path = os.path.abspath(self.current_image_path)
            
            # PowerShell 脚本：将图片复制到剪贴板
            # 使用 Add-Type 加载程集，比 LoadWithPartialName 更可靠
            ps_script = f&quot;&quot;&quot;
            Add-Type -AssemblyName System.Windows.Forms
            Add-Type -AssemblyName System.Drawing
            $img = [System.Drawing.Image]::FromFile(&#x27;{img_path}&#x27;)
            [System.Windows.Forms.Clipboard]::SetImage($img)
            $img.Dispose()
            &quot;&quot;&quot;
            
            # 执行 PowerShell 脚本
            result = subprocess.run(
                [&#x27;powershell&#x27;, &#x27;-NoProfile&#x27;, &#x27;-Command&#x27;, ps_script],
                capture_output=True,
                text=True,
                timeout=5,
                encoding=&#x27;utf-8&#x27;
            )
            
            if result.returncode != 0:
                raise Exception(f&quot;PowerShell 错误: {result.stderr}&quot;)
            
            messagebox.showinfo(&quot;成功&quot;, &quot;图片已复制到剪贴板&quot;)
        except Exception as e:
            raise Exception(f&quot;PowerShell 复制失败: {str(e)}&quot;)
    
    def _copy_image_via_clipboard_file(self):
        &quot;&quot;&quot;第四个备选：使用 PowerShell 的 Set-Clipboard 复制图片文件（最稳定）&quot;&quot;&quot;
        try:
            # 最简单的方案：使用 PowerShell 的 Set-Clipboard 命令复制文件
            # PowerShell 5.0+ 自带此命令，不需要加载程集
            
            img_path = os.path.abspath(self.current_image_path)
            
            # 使用 PowerShell 的 Set-Clipboard 命令，传入文件路径
            # PowerShell 会自动检测文件类型并复制为图片
            ps_script = f&quot;&quot;&quot;
            $file = Get-Item -LiteralPath &#x27;{img_path}&#x27;
            if ($file.PSIsContainer) {{
                throw &#x27;Path is a directory, not a file&#x27;
            }}
            Set-Clipboard -Path &#x27;{img_path}&#x27;
            &quot;&quot;&quot;
            
            result = subprocess.run(
                [&#x27;powershell&#x27;, &#x27;-NoProfile&#x27;, &#x27;-Command&#x27;, ps_script],
                capture_output=True,
                text=True,
                timeout=5,
                encoding=&#x27;utf-8&#x27;
            )
            
            if result.returncode != 0:
                # 如果 Set-Clipboard 失败，尝试使用 clip.exe
                raise Exception(f&quot;Set-Clipboard 失败: {result.stderr}&quot;)
            
            messagebox.showinfo(&quot;成功&quot;, &quot;图片已复制到剪贴板&quot;)
        except Exception as e:
            raise Exception(f&quot;剪贴板复制失败: {str(e)}&quot;)
    
    def _copy_image_via_clip_exe(self):
        &quot;&quot;&quot;第五个备选：使用 clip.exe 复制图片文件路径&quot;&quot;&quot;
        try:
            # clip.exe 是 Windows 内置工具，会自动检测文件类型
            img_path = os.path.abspath(self.current_image_path)
            
            # 使用 PowerShell 调用 clip.exe
            ps_script = f&quot;&quot;&quot;
            $file = Get-Item -LiteralPath &#x27;{img_path}&#x27;
            if ($file.PSIsContainer) {{
                throw &#x27;Path is a directory&#x27;
            }}
            $file.FullName | clip.exe
            &quot;&quot;&quot;
            
            result = subprocess.run(
                [&#x27;powershell&#x27;, &#x27;-NoProfile&#x27;, &#x27;-Command&#x27;, ps_script],
                capture_output=True,
                text=True,
                timeout=5,
                encoding=&#x27;utf-8&#x27;
            )
            
            if result.returncode != 0:
                raise Exception(f&quot;clip.exe 失败: {result.stderr}&quot;)
            
            messagebox.showinfo(&quot;成功&quot;, &quot;图片已复制到剪贴板&quot;)
        except Exception as e:
            raise Exception(f&quot;clip.exe 复制失败: {str(e)}&quot;)
    
    def _clear_image(self):
        &quot;&quot;&quot;清空图片昫示&quot;&quot;&quot;
        self.image_canvas.delete(&#x27;all&#x27;)
        self.image_canvas.image = None
        self.image_info_label.config(text=&quot;&quot;)
        self.current_image_path = None

def main():
    root = tk.Tk()
    app = SentenceImageMatcherGUI(root)
    root.mainloop()

if __name__ == &quot;__main__&quot;:
    main()

```

> 这里的代码是今天才开发的11.22号，也是基于现有系统上，

> 功能呢，首先需要点击浏览，找到对应**的参数文件夹。参数文件夹存放的文件夹看 2.**5我在用的方法， 就是你自己创建的文件夹，经过影刀代码的处理，会产生参数文件夹。

> 浏览完，点击加载。

> **如果对于**拆分句子的结果不满意，可以在输入文本这里自己换行，然后点击更新列表。

> 选择产品这里，支持下拉框，选择其他的产品图片文件夹。这里我的想法是，如果你想介绍**产品的时****候，一句话对应一张图片**的话，可以把介绍产品的段落放到输入文本里面。然后在选择产品这里，下拉选择到对应产品图片文件夹进行图片匹配。支持输入关键词模糊匹配

> **相似度阈值**这里，可以调整，调的越小，匹配到的图片越多。

> **复制图片**，**windows系统**测试可以用。Mac电脑没有测试

**如何使用？  问豆包！**

### **3.4 快速获取产品宣传视频**

这个也算是Broll提效的一部分。

我这里写的代码是 只能获取京东的宣传视频

```
import requests
import re
import json
import os
from pathlib import Path
from playwright.sync_api import sync_playwright
import time
from concurrent.futures import ThreadPoolExecutor, as_completed
import threading

class JDVideoCrawlerFast:
    def __init__(self, save_dir=&quot;./jd_video_downloads&quot;, max_workers=3):
        self.save_dir = save_dir
        Path(save_dir).mkdir(parents=True, exist_ok=True)
        self.max_workers = max_workers  # 并发线程数
        self.lock = threading.Lock()  # 线程锁，用于打印输出
        
        self.headers = {
            &quot;User-Agent&quot;: &quot;Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36&quot;,
            &quot;Referer&quot;: &quot;https://www.jd.com/&quot;,
        }
    
    def extract_sku_id(self, url):
        &quot;&quot;&quot;从URL中提取商品SKU ID&quot;&quot;&quot;
        match = re.search(r&#x27;/(\d+)\.html&#x27;, url)
        if match:
            return match.group(1)
        return None
    
    def extract_video_ids(self, content):
        &quot;&quot;&quot;从页面内容中提取视频ID&quot;&quot;&quot;
        video_ids = []
        
        pattern1 = r&#x27;data-vu=[&quot;\&#x27;](\d+)[&quot;\&#x27;]&#x27;
        matches = re.findall(pattern1, content)
        video_ids.extend(matches)
        
        pattern2 = r&#x27;mainVideoId[&quot;\&#x27;]?\s*[:=]\s*[&quot;\&#x27;](\d+)[&quot;\&#x27;]&#x27;
        matches = re.findall(pattern2, content)
        video_ids.extend(matches)
        
        pattern3 = r&#x27;vid[&quot;\&#x27;]?\s*[:=]\s*[&quot;\&#x27;](\d+)[&quot;\&#x27;]&#x27;
        matches = re.findall(pattern3, content)
        video_ids.extend(matches)
        
        return list(set(video_ids))
    
    def get_video_from_tencent_api(self, video_id, sku_id):
        &quot;&quot;&quot;通过视频ID从腾讯视频API获取视频URL&quot;&quot;&quot;
        try:
            api_url = f&quot;https://api.m.jd.com/tencent/video_v3?callback=jQuery123&amp;vid={video_id}&amp;type=1&amp;from=1&amp;appid=item-v3&amp;functionId=pc_tencent_video_v3&quot;
            
            response = requests.get(api_url, headers=self.headers, timeout=10)
            response.raise_for_status()
            
            jsonp_match = re.search(r&#x27;jQuery\d+\((.*)\)&#x27;, response.text)
            if jsonp_match:
                json_str = jsonp_match.group(1)
                video_data = json.loads(json_str)
                
                if video_data.get(&#x27;code&#x27;) == 0 and video_data.get(&#x27;playUrl&#x27;):
                    duration = video_data.get(&#x27;duration&#x27;, 0)
                    play_url = video_data.get(&#x27;playUrl&#x27;)
                    return play_url, duration
            
            return None, 0
        except Exception as e:
            return None, 0
    
    def check_video_exists_with_playwright(self, url):
        &quot;&quot;&quot;使用Playwright检查页面中是否存在视频&quot;&quot;&quot;
        try:
            with sync_playwright() as p:
                browser = p.chromium.launch(headless=True, args=[&quot;--no-sandbox&quot;])
                context = browser.new_context(extra_http_headers=self.headers)
                page = context.new_page()
                
                page.goto(url, timeout=15000, wait_until=&quot;domcontentloaded&quot;)
                time.sleep(3)  # 减少等待时间以加快速度
                
                content = page.content()
                video_ids = self.extract_video_ids(content)
                
                browser.close()
                
                if video_ids:
                    return True, video_ids
                return False, []
                
        except Exception as e:
            return False, []
    
    def extract_video_urls(self, url, sku_id):
        &quot;&quot;&quot;提取页面中的所有视频URL&quot;&quot;&quot;
        video_urls = []
        
        try:
            with sync_playwright() as p:
                browser = p.chromium.launch(headless=True, args=[&quot;--no-sandbox&quot;])
                context = browser.new_context(extra_http_headers=self.headers)
                page = context.new_page()
                
                page.goto(url, timeout=15000, wait_until=&quot;domcontentloaded&quot;)
                time.sleep(3)
                
                content = page.content()
                video_ids = self.extract_video_ids(content)
                
                for video_id in video_ids:
                    play_url, duration = self.get_video_from_tencent_api(video_id, sku_id)
                    if play_url:
                        video_urls.append({
                            &#x27;url&#x27;: play_url,
                            &#x27;duration&#x27;: duration,
                            &#x27;video_id&#x27;: video_id,
                            &#x27;source&#x27;: &#x27;tencent_api&#x27;
                        })
                
                browser.close()
                
        except Exception as e:
            pass
        
        return video_urls
    
    def download_video(self, video_url, filename):
        &quot;&quot;&quot;下载单个视频&quot;&quot;&quot;
        try:
            save_path = os.path.join(self.save_dir, filename)
            
            if os.path.exists(save_path):
                with self.lock:
                    print(f&quot;  ⚠️  文件已存在，跳过：{filename}&quot;)
                return True
            
            with self.lock:
                print(f&quot;  🔄 正在下载：{filename}&quot;)
            
            response = requests.get(video_url, headers=self.headers, stream=True, timeout=30)
            response.raise_for_status()
            
            total_size = int(response.headers.get(&#x27;content-length&#x27;, 0))
            downloaded = 0
            chunk_size = 1024 * 1024
            
            with open(save_path, &#x27;wb&#x27;) as f:
                for chunk in response.iter_content(chunk_size=chunk_size):
                    if chunk:
                        f.write(chunk)
                        downloaded += len(chunk)
            
            file_size = os.path.getsize(save_path) / 1024 / 1024
            with self.lock:
                print(f&quot;  ✅ 下载完成：{filename} ({file_size:.2f} MB)&quot;)
            return True
            
        except Exception as e:
            with self.lock:
                print(f&quot;  ❌ 下载失败：{str(e)}&quot;)
            return False
    
    def process_url(self, url):
        &quot;&quot;&quot;处理单个URL&quot;&quot;&quot;
        with self.lock:
            print(f&quot;\n{&#x27;=&#x27;*70}&quot;)
            print(f&quot;处理URL：{url}&quot;)
            print(f&quot;{&#x27;=&#x27;*70}&quot;)
        
        if &#x27;jd.com&#x27; not in url:
            with self.lock:
                print(&quot;❌ 不是有效的京东URL&quot;)
            return False
        
        sku_id = self.extract_sku_id(url)
        if not sku_id:
            with self.lock:
                print(&quot;❌ 无法从URL中提取商品ID&quot;)
            return False
        
        with self.lock:
            print(f&quot;✅ 商品ID：{sku_id}&quot;)
        
        with self.lock:
            print(f&quot;\n【第一步】检查是否存在视频...&quot;)
        
        has_video, video_ids = self.check_video_exists_with_playwright(url)
        
        if not has_video:
            with self.lock:
                print(f&quot;❌ 该商品页面不存在视频&quot;)
            return False
        
        with self.lock:
            print(f&quot;✅ 检测到视频存在（{len(video_ids)}个视频ID）&quot;)
        
        with self.lock:
            print(f&quot;\n【第二步】提取视频URL...&quot;)
        
        video_urls = self.extract_video_urls(url, sku_id)
        
        if not video_urls:
            with self.lock:
                print(f&quot;❌ 无法提取视频URL&quot;)
            return False
        
        with self.lock:
            print(f&quot;✅ 找到 {len(video_urls)} 个视频&quot;)
        
        with self.lock:
            print(f&quot;\n【第三步】下载视频...&quot;)
        
        success_count = 0
        
        for i, video_info in enumerate(video_urls, 1):
            video_url = video_info[&#x27;url&#x27;]
            duration = video_info[&#x27;duration&#x27;]
            filename = f&quot;jd_video_{sku_id}_{i}.mp4&quot;
            
            with self.lock:
                print(f&quot;\n视频{i}/{len(video_urls)} (时长：{duration}秒)&quot;)
            
            if self.download_video(video_url, filename):
                success_count += 1
            
            time.sleep(0.5)
        
        with self.lock:
            print(f&quot;\n✅ 成功下载 {success_count}/{len(video_urls)} 个视频&quot;)
        
        return True
    
    def process_urls_batch(self, urls):
        &quot;&quot;&quot;批量处理URL（多线程）&quot;&quot;&quot;
        print(f&quot;\n{&#x27;=&#x27;*70}&quot;)
        print(f&quot;开始批量处理 {len(urls)} 个URL（并发数：{self.max_workers}）&quot;)
        print(f&quot;{&#x27;=&#x27;*70}\n&quot;)
        
        success_count = 0
        failed_count = 0
        
        with ThreadPoolExecutor(max_workers=self.max_workers) as executor:
            # 提交所有任务
            futures = {executor.submit(self.process_url, url): url for url in urls}
            
            # 处理完成的任务
            for future in as_completed(futures):
                url = futures[future]
                try:
                    result = future.result()
                    if result:
                        success_count += 1
                    else:
                        failed_count += 1
                except Exception as e:
                    with self.lock:
                        print(f&quot;❌ 处理失败 {url}：{str(e)}&quot;)
                    failed_count += 1
        
        print(f&quot;\n{&#x27;=&#x27;*70}&quot;)
        print(f&quot;【批量处理完成】&quot;)
        print(f&quot;成功：{success_count} 个&quot;)
        print(f&quot;失败：{failed_count} 个&quot;)
        print(f&quot;总计：{len(urls)} 个&quot;)
        print(f&quot;{&#x27;=&#x27;*70}\n&quot;)
    
    def process_urls_from_file(self, file_path):
        &quot;&quot;&quot;从文件中批量处理URL&quot;&quot;&quot;
        try:
            with open(file_path, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as f:
                urls = [line.strip() for line in f if line.strip()]
            
            self.process_urls_batch(urls)
            
        except FileNotFoundError:
            print(f&quot;❌ 文件不存在：{file_path}&quot;)
        except Exception as e:
            print(f&quot;❌ 错误：{str(e)}&quot;)

def main():
    print(&quot;=&quot; * 70)
    print(&quot;京东视频批量爬取工具 - 快速版（多线程）&quot;)
    print(&quot;=&quot; * 70)
    
    # 设置并发线程数（根据网络情况调整）
    # 建议：网络好用3-5，网络一般用2-3
    crawler = JDVideoCrawlerFast(max_workers=3)
    
    while True:
        print(&quot;\n请选择操作模式：&quot;)
        print(&quot;1. 输入单个URL&quot;)
        print(&quot;2. 从文件批量处理URL（多线程）&quot;)
        print(&quot;3. 设置并发线程数&quot;)
        print(&quot;4. 退出&quot;)
        
        choice = input(&quot;\n请输入选择 (1/2/3/4)：&quot;).strip()
        
        if choice == &#x27;1&#x27;:
            url = input(&quot;\n请输入京东商品链接：&quot;).strip()
            if url:
                crawler.process_url(url)
        
        elif choice == &#x27;2&#x27;:
            file_path = input(&quot;\n请输入URL文件路径：&quot;).strip()
            if file_path:
                crawler.process_urls_from_file(file_path)
        
        elif choice == &#x27;3&#x27;:
            try:
                workers = int(input(&quot;\n请输入并发线程数（建议2-5）：&quot;).strip())
                if 1 &lt;= workers &lt;= 10:
                    crawler.max_workers = workers
                    print(f&quot;✅ 并发线程数已设置为：{workers}&quot;)
                else:
                    print(&quot;❌ 请输入1-10之间的数字&quot;)
            except ValueError:
                print(&quot;❌ 请输入有效的数字&quot;)
        
        elif choice == &#x27;4&#x27;:
            print(&quot;\n👋 再见！&quot;)
            break
        
        else:
            print(&quot;❌ 无效的选择&quot;)

if __name__ == &quot;__main__&quot;:
    main()

```

```
================================================================================
                    京东视频批量爬取工具 - 使用指南
================================================================================

【功能概述】
这是一个智能的京东商品视频爬取工具，可以：
✅ 自动检测京东商品页面是否存在视频
✅ 智能提取视频ID和视频URL
✅ 自动下载视频到本地
✅ 支持单个URL或批量处理
✅ 自动去重和断点续传

================================================================================
【快速开始】
================================================================================

1. 安装依赖
   pip install requests playwright
   python -m playwright install chromium

2. 运行爬虫
   python jd_video_crawler_v2.py

3. 选择操作模式
   - 选项1：输入单个URL
   - 选项2：从文件批量处理URL
   - 选项3：退出

================================================================================
【使用示例】
================================================================================

【示例1】单个URL爬取
---------
1. 运行程序：python jd_video_crawler_v2.py
2. 选择选项1
3. 输入京东链接：https://item.jd.com/100032139847.html
4. 程序自动检测、提取、下载视频

【示例2】批量URL爬取
---------
1. 创建文本文件 urls.txt，内容如下：
   https://item.jd.com/100032139847.html
   https://item.jd.com/100000000000.html
   https://item.jd.com/100000000001.html

2. 运行程序：python jd_video_crawler_v2.py
3. 选择选项2
4. 输入文件路径：urls.txt
5. 程序自动批量处理所有URL

================================================================================
【工作流程】
================================================================================

输入URL
  ↓
验证URL格式（必须是jd.com）
  ↓
提取商品ID（从URL中）
  ↓
【检查视频】使用Playwright加载页面，提取视频ID
  ↓
【提取视频URL】通过腾讯视频API获取播放地址
  ↓
【下载视频】分块下载到本地
  ↓
完成

================================================================================
【输出文件】
================================================================================

所有下载的视频保存在 jd_video_downloads 目录中

文件名格式：jd_video_{商品ID}_{序号}.mp4

例如：
  jd_video_100032139847_1.mp4
  jd_video_100032139847_2.mp4

================================================================================
【支持的视频来源】
================================================================================

1. 腾讯视频API - 京东通过腾讯视频API加载的视频（主要）
2. 直接视频URL - 页面中直接包含的视频链接
3. HTML5 Video标签 - 通过&lt;video&gt;标签嵌入的视频
4. Source标签 - &lt;video&gt;标签内的&lt;source&gt;子元素

================================================================================
【常见问题】
================================================================================

Q: 为什么有些商品没有视频？
A: 并非所有京东商品都有视频。工具会自动检测并跳过没有视频的商品。

Q: 下载速度很慢怎么办？
A: 这取决于网络速度和视频服务器的响应速度。可以尝试：
   - 检查网络连接
   - 更换时间段重试
   - 使用代理加速

Q: 如何处理登录验证？
A: 如果遇到登录验证，可以：
   1. 从浏览器F12获取Cookie
   2. 修改代码中的 HEADERS 字典，添加Cookie

Q: 支持其他视频网站吗？
A: 目前主要针对京东商品页面。可以根据需要修改代码以支持其他网站。

Q: 如何查看下载的视频？
A: 打开 jd_video_downloads 文件夹，所有下载的视频都在这里。

Q: 可以同时下载多个视频吗？
A: 可以。使用批量模式（选项2），在文本文件中输入多个URL，程序会依次处理。

================================================================================
【注意事项】
================================================================================

⚠️ 重要提示：
- 请遵守京东的服务条款和爬虫协议
- 不要过度爬取，控制请求频率
- 下载的视频仅供个人学习使用，不得用于商业目的
- 尊重视频版权，不得传播或二次发布
- 程序会自动添加延迟以避免被反爬

================================================================================
【技术栈】
================================================================================

- requests - HTTP请求库
- Playwright - 浏览器自动化工具（支持JavaScript渲染）
- re - 正则表达式
- json - JSON解析

================================================================================
【文件说明】
================================================================================

jd_video_crawler_v2.py    - 主程序（推荐使用）
jd_video_crawler.py       - 旧版本（已弃用）
test_crawler_v2.py        - 功能测试脚本
test_urls.txt             - 测试URL文件示例
README.md                 - 详细文档
使用指南.txt              - 本文件

================================================================================
【版本信息】
================================================================================

版本：v2.0
发布日期：2025-11-22
主要改进：
- ✅ 改进视频ID提取逻辑
- ✅ 优化腾讯视频API调用
- ✅ 增强错误处理
- ✅ 提高稳定性

================================================================================
【联系与反馈】
================================================================================

如有问题或建议，欢迎反馈！

================================================================================


![](img/783901213fdfeb02e9a0fefd5309e104.png)

![](img/99f15bd91082555b9e0bd0ec1639aca3.png)


![](img/1e39392115f753129d9e948784815989.png)

![](img/25ed472db9d7aee04c0db7f92356b5bd.png)

```

**如何使用？  问豆包！**

### 3.5 好看的雷达图设计方法
具体设计方法看[提效代码](https://zqxbobng8rp.feishu.cn/docx/YACMd91o4oIYyqxsqMVcs6wenIe?from=from_copylink) ，文档**里面有说明**

雷达图这里主要用在产品图片PPT中比较多， 通过差异化设计突出产品多维度优势。

# 四、脚本提效

**拆解对标**，先写出来几个自己满意的作为**对标稿子(终极稿子)**

然后再去拆解出对应的AI提示词， 在写稿子的时候，投入对应的素材。

**比如讲解产**品的时候，**我的系统里面参数文****件****夹 会有一个详**情图txt文件夹，里面保存的是每个产品的详情图ocr识别结果。我会把所有的txt以及 分类.xlsx 发给AI，让它在这些内容的基础上创作脚本，最大程度上保证了参数的正确性，比如价格。
这样写产品介绍的文案就会很快，提示词采用的是百万哥的帖子里写的

```
**我现在在做B**站好物带货项目，我提**供给**你的**文件中： “终极稿子.txt”，是我已经做完的，4个反响**还不错的视频口播**稿，也就是我自己的****风格；**12个产品详情内容txt文档是我准备推荐的产品--【副屏】； 
我需要你： 帮我以这12个产品为主，先分析我过去口播稿的内容、结构和风格（比如采用了【引入痛点-介绍产品-总结】的结构、使用真实经历增加可信度、每几十秒就有一个吸引观众的“钩子”，增加口播视频完播率，以上只是举例，具体的需要你自己分析），再参考分类.xlsx里面的sheet=副屏的产品参数，为我创作出属于我自己的【副屏】带货口播稿。
口播稿需要满足以下几个要求：
 1. 提到的所有的产品可以使用产品标题中的简称； 
 2. 所有产品的参数一定要正确，都要来源于其产品链接中，不可以使用未提及的参数，如果有未提及的参数，请使用【待确认】预留占位； 
 3. 总字数在4000-5000字；
  4. 口播稿中的up主自称为 xx ； 
  5. 口播稿要完全按照口播的形式，不需要“发言人 08:55”这种时间轴，也不需要表格，要完全符合口语化的叙述风格； 
  6. 款式介绍不要使用一二三这种称呼，要用“第一款”、“第二款”、“第三款”这种称呼；
  7. 口播稿中避免使用“**”、“•” 、“/”这些符号，而是使用口语化的表达，比如“**”完全可以去掉，多行产品前的“•”可以改为首先...其次...然后...最后...的口语化表达方式，而&quot;/&quot;要改为“或”，双引号和xx Script备注也需要去掉，以此类推，类似的符号表达尽量都避免，而使用口语化的表达，直接念出来就是口播内容。
  8. 对技术术语和英文缩写不要进行音译处理和口语化替代。 好了，要求如上述，给我完整的口播稿内容。
  9. 要求每个产品都要被介绍到，不能重复介绍也不能缺少介绍。

![](img/4ab7f43cb8a31bfcb5b69365554930fa.png)


![](img/13ffcfd4c22760090392afe6ba5b8f88.png)


![](img/86791f6913a7bf9b529c8dd9a7936157.png)


![](img/8295ee12c1da3248faf2b9831ceb4b8b.png)


![](img/622bd31476668a1699a35b46c0cac17e.png)


![](img/49a97f34b96b510524e818da85ce8d55.png)


![](img/6c18a77d29124649de0dfdc6dc484888.png)


![](img/184e6d449f476df620ea03aa76f86e30.png)


![](img/057613f0f627522fe5778b0d45d5fff4.png)


![](img/d8965b18dbe7aa99e46039a1ecb0d226.png)

```

得到初始的稿子后，接下来就是去校正有没有错误，润色文案。

**使用方法：**

**修改提示词标记部分，上传所有产品的txt文件，上**传分类.xlsx, 上传终极稿子.txt

注意⚠️：终极稿子是自己调教好的脚本

**使用效果：**
# 五、其他提效

### 5.1 蓝链

**tips1:蓝链输入的时候，采用倒序输入链接的方**式，得到的链接顺序会自动校正为正常顺序。

这里指的是链接选品的时候，倒序插入商品链接，点击确定后，得到的产品顺序是正序的
**点击链接选品**
倒序输入链接，识别链接，点击确定后就是正序的

tips2:想要实现流水线生产视频到发布，这一步是必须要解决的

想要实现**自动化输入蓝链**的，可以参考下我的研究笔记。[提效代码](https://zqxbobng8rp.feishu.cn/docx/YACMd91o4oIYyqxsqMVcs6wenIe?from=from_copylink)
### 5.2 数据采集

推荐GitHub上面一个自媒体采集的项目，下面这个是原本的代码
[MediaCrawler-main.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XGyOborfPoUpHhxOnL1cD5dknRf)

可以采集知乎，📕，抖音，B站等平台的内容。比如知乎文章，评论等

**使用场景：**

1.可以使用这个工具，把B站想要调研的UP的所有视频信息采集到本地，进行AI调研。

2.可以使用这个工具，把知乎的测评文章及评论采集到本地，进行脚本撰写的参考。

### 5.3 B站字幕批量下载
具体方法见[提效代码](https://zqxbobng8rp.feishu.cn/docx/YACMd91o4oIYyqxsqMVcs6wenIe?from=from_copylink)文档说明

### 5.4 B站视频批量下载
具体方法见[提效代码](https://zqxbobng8rp.feishu.cn/docx/YACMd91o4oIYyqxsqMVcs6wenIe?from=from_copylink)文档说明

### 5.5 去水印

推荐工具链接：https://www.kaipai.com/video-tool/remove-watermark

### 5.6 获取B站视频脚本

**方法1****：**

下载B站视频https://snapany.com/zh/bilibili

通义听悟，视频转文本

**方法2：**

get笔记网页版，输入B站链接。

### 5.7 视频半自动化剪辑系统

**开发中 ，****敬请****等待下一篇**
**脚本编辑页面**
**样式管理页面**
**配置生成页面**

```
🎬 B站批量剪辑工具 - 核心功能介绍
这是一个专业的视频批量生成系统，专为B站内容创作者设计。以下是它能实现的主要功能：
🎯 核心功能
1. 📝 脚本管理
编写和管理视频脚本
自动按行分段（每行一个段落）
实时段落数统计
2. 🎨 样式配置系统
从剪映导入样式模板
为每个段落配置不同的样式
支持文字、图片、视频、贴纸等多种元素
支持多元素样式（如三图逐个呈现）
支持额外样式叠加
3. 🎤 智能配音生成
Edge TTS（免费）：支持多种中文音色
MiniMax TTS（高质量）：支持情感控制和克隆音色
智能增量生成：只重新生成修改的部分
语速调节：0.5x-2.0x 倍速
4. 🎬 视频草稿生成
一键生成剪映草稿文件
自动应用样式、动画、转场
支持多元素时间线（按关键词逐个显示元素）
自动生成字幕
5. 📁 项目化管理
独立的项目工作空间
素材库管理（表情包、视频、音效、产品图）
配置文件自动保存
支持项目导入导出
6. 🔄 增量生成
智能检测脚本和配置变化
只重新生成修改的部分
大幅提高工作效率

🚀 工作流程
编写脚本 → 配置样式 → 生成配音 → 生成视频草稿 → 导入剪映 → 导出最终视频

🎯 适用人群
✅ B站知识分享创作者 - 快速批量制作讲解视频
✅ 产品运营 - 批量生成产品介绍视频
✅ 教育培训 - 快速制作教程类视频
✅ 内容创业者 - 提高视频产出效率

💡 实际应用场景
✅ 知识分享视频 - 快速生成讲解类视频
✅ 产品介绍视频 - 批量生成产品展示视频
✅ 教程类视频 - 自动配音和样式应用
✅ 营销视频 - 批量生成营销内容
✅ 短视频创作 - 快速批量制作
```

# 六、乘风破浪一起去做“提效航海家”

在 B 站好物创作的提效之路上，从选品的精准化、PPT 制作的高效化，到脚本的智能化、工具的集成化，每一个环节的优化都是对内容创作效率与质量的双重提升。这些提效方法与工具，不仅是我个人在两年职场与自学探索中的沉淀，更希望能为同样在 B 站内容电商领域耕耘的从业者提供一份实用的参考。

很幸运能加入**赚钱 B 站好物|深海圈**和充满活力的航海群，和各位圈友交流碰撞、共享资源、拆解难题 —— 是社群里的思维火花让我在批量查询佣金时想去找到更优解方法，在 PPT 套版环节少走了弯路，找到了自己B 站好物提效的全链路解决方案。

提效的路还长，有了深海圈和社群的陪伴，我不再是单打独斗的创作者。期待我们继续在这个圈子里互通有无、迭代方法，把提效**的势能转化**为内容的声量与商业的增量，在 B 站内容电商的领域里，打造出属于自己的内容与商业价值闭环。一起做乘风破浪的 “提效航海家”！