
如果您决定将 AI 添加到工作区，则工作区中的每个成员都将能够使用 AI 功能 - 这包括工作区所有者、会员管理员和成员。访客将无法在您的工作空间中使用人工智能功能。

### 如何取消我的付费计划？

答：您的 Notion 订阅（按年或按月）将自动续订，直到您取消为止。您可以通过以下方式取消订阅：转到台式计算机左侧边栏中的“设置和会员”，然后选择“计划”，然后选择“降级”。取消后，您仍然可以使用所有付费功能，直到结算周期结束。

# 三、中文汉化

## 网页端汉化

### 方案一：Notion AI 汉化插件（谷歌）

![](img/b1f275aa66a1c8e0ba0e4229b5182c95.png)

**主要功能：**Notion 是个特别棒的笔记应用站点，但是对于很多不熟悉英文的用户来说，Notion 的英文操作界面会非常影响使用体验。安装此扩展之后，打开 Notion ，会将 Notion 的操作界面汉化为中文。非常方便中文用户。

**传送门：**https://chrome.google.com/webstore/detail/notion-%E4%B8%AD%E6%96%87%E7%89%88/cfocjnjbgnenmlbikfglmnociegpgcaj?hl=zh-CN

### 方案二：Notion 中文版（微软）

![](img/aed76e2a1b373bcf89cf5f47de1173f0.png)

**说明：**对 Notion 进行中文汉化，安装扩展后打开 Notion 将会自动翻译为中文

**传送门：**https://microsoftedge.microsoft.com/addons/detail/notion-%E4%B8%AD%E6%96%87%E7%89%88/megcjnjlndjjmbaipegndglofmmefggl

### 方案三：油猴插件汉化脚本汉化（备选谷歌）

1）首先需要安装油猴插件

![](img/d1e8715a0e56efb5f8c6eb60c53dd5a9.png)

**说明：**应用商店的搜索框搜索“Tampermonkey”，然后点击“获取”，即可安装成功油猴插件。

**传送门：**https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo

2）安装 `notion-zh_CN` notion 的汉化脚本

![](img/1c959763833ef53239711b3dd67b162b.png)

**说明：**`notion-zh_CN` 是对 notion 的汉化脚本。

**传送门：**https://greasyfork.org/zh-CN/scripts/430116-notion-zh-cn-notion%E7%9A%84%E6%B1%89%E5%8C%96%E8%84%9A%E6%9C%AC

## 客户端汉化

### 桌面端

#### windows

下载 Notion 中文语言包文件 `notion-zh_CN.js` ，下载网址：https://github.com/Reamd7/notion-zh_CN/releases/(无法下载是网络问题）

1. （自 `2.0.4` 版本后，任意语言都等价于中文了）

1. notion 安装目录：`C:\Users\用户名\AppData\Local\Programs\Notion\`

1. 打开 `C:\Users\用户名\AppData\Local\Programs\Notion\resources\app\renderer` 文件夹

1. 下载 `notion-zh_CN.js` 到上述文件夹（renderer）

1. 打开 `preload.js`

1. 在最后一行加上 `js //# sourceMappingURL=preload.js.map require("./notion-zh_CN") // 添加该行`

1. 重启

+ 上述操作也可以使用 PowerShell 命令来完成。

命令执行完成后，在 Notion 中使用 `CTRL+R` 可以热更新界面。

```
powershell Invoke-WebRequest -Uri "https://github.com/Reamd7/notion-zh_CN/releases/latest/download/notion-zh_CN.js" -OutFile "$HOME\AppData\Local\Programs\Notion\resources\app\renderer\notion-zh_CN.js" Add-Content "$HOME\AppData\Local\Programs\Notion\resources\app\renderer\preload.js" 'require("./notion-zh_CN")'
```

#### Mac

网页端 以及 windows 端，能够 100% 汉化，指的是时间显示也有国际化的能力，点击所有更新的时间轴中就能看出来，点击？悬浮按钮也能看到。

下载 Notion 中文语言包文件 `notion-zh_CN.js` ，下载网址：https://github.com/Reamd7/notion-zh_CN/releases/

1. 打开 Finder，应用程序，右键 `notion.app`，显示应用包内容

1. （自 `2.0.4` 版本后，任意语言都等价于中文了）

1. 打开 `Notion.app\Contents\Resources\app\renderer\`

1. 下载 `notion-zh_CN.js` 到上述文件夹（renderer）

1. 打开 `preload.js`

1. 在最后一行加上 `js //# sourceMappingURL=preload.js.map require("./notion-zh_CN") // 添加该行`

1. 重启

只是 同样打开 `Notion.app\Contents\Resources\app\renderer\` 即可。。（安装包显示有同样目录结构）

### 手机端

#### 安卓端

+ 汉化下载 apk：`https://github.com/Reamd7/notion-zh_CN/blob/main/apk/Notion_0.6.1122.beta(7122)_zh_cn.apk`

# 四、学习教程

## 学习视频

### 1.Notion 及 Notion AI 使用教程

#### 1）B 站 Notion 学习教程

![](img/8cd8b08bf54a9f4a28d7bb4cb4f500c8.png)

**说明：**B 站做的还算比较好的学习课程

**传送门：**https://space.bilibili.com/475961865/video?keyword=notion

#### 2）官方 Notion AI 入门学习课程

![](img/cfb91e0e4cfdb9a26bf45be9ac859946.png)

**说明：**了解如何使用人工智能来改进写作、简化任务以及从头开始生成文本。

**传送门：**https://www.notion.so/help/notion-academy/course/getting-started-with-notion-ai

## 学习文件

### 官方学习指南

![](img/4bfb5c749600b18a2a8ed761968203c6.png)

**说明：**提高您的技能并探索使用 Notion 的新方法。

**传送门：**https://www.notion.so/help/guides

### 2.开始使用 Notion AI

要使用 Notion AI，必须先呼出 Notion AI 菜单，这里有两种方法可以呼出 Notion AI 菜单：

+ **方法一：**左上角定位于空行开头，并按`空格键`呼出 AI 菜单；

+ **方法二：**选中一段文字，在悬浮菜单中点击询问 AI 呼出 AI 功能。

![](img/68d008c62c9fe1f2ee952915c29ba03d.png)

方法一：按`空格`呼出 AI 菜单

![](img/cd5a6fab31c9599a6aebdda869110917.png)

方法二：选中段落并通过按钮呼出 AI 菜单

在 Notion AI 菜单中，我们可以看到各种 AI 功能：

![](img/31625a266191e9d0a8f6a374f65458c2.png)

菜单上集成了 Notion AI 的所有功能

### Notion 学习手册中文版

![](img/4672c396eedd6b8aa5aff7cf9523c172.png)

**简介：**中文版的 Notion 学习手册

**传送门：**https://notionchina.co/

### 4.Notion 中文文档

![](img/9d78dfad51c6f64a4b9e49d7159bc6a2.png)

**简介：**关于 Notion 的相关使用教学内容汇总

**传送门：**[Notion – The all-in-one workspace for your notes， tasks， wikis， and databases。](https://www.notion.so/Notion-64ab96a7fccf4d74b9a9a68b338bc501)

### 5.Notion 充电中心

![](img/7f0dc9f1cc25d71fabfc3fcd8e7479d4.png)

**简介：**Notion 中文社区学习资料。

**传送门：**https://www.notion.so/Notion-2f3c5b06b3ad406bad8cf065ed3cca7c

### 6.Notion 使用教程&资源：从入门到精通

![](img/b1bc28889f94dbe1cd6fd22e84012d10.png)

**简介：**Notion 使用教程·系列专文。

**传送门：**Notion 使用教程 & 资源：从入门到精通

## 常见问题

### Notion 可以使用 Markdown 吗？

答：Notion 是一个近几年比较火的效率软件，它完全可以作为个人笔记系统来使用，里面可以写 markdown、备忘录、timeline、学习笔记、待办事项、读书计划、任务管理等等，All in one。

### 如何找到更多好看可爱的 emoji 表情？

答：常见的有以下几个：

1）EMOJIALL 中文网

![](img/b53d2004e23b7c151bf58158ffc11fcd.png)

**说明：**有分类和相关推荐，以及不同国家的 emoji 的使用趋势和排名

**传送门：**https://www.emojiall.com/zh-hans/all-emojis

2）阿里巴巴矢量图

![](img/ec26c558e11c9555893d1ff52a9357da.png)

**说明：**比较个性化的 emoji 且成套的。

**传送门：**iconfont-阿里巴巴矢量图标库

### Notion Page 是什么？

答：基础功能 Notion 的内容单位为「页」（Page），你可以将 Notion 中的页面理解为传统笔记工具中的一条笔记。 不同的是，Notion 中的页面支持无限层级，相当于有了类似于文件夹的功能，我们又能在 Notion 的每个页面中记录笔记。

### Notion 是否支持公式？

答：已经支持，具体请使用 `/math` 来进行输入。

# 五、插件推荐

说明：以下插件均为谷歌插件，请选择谷歌浏览器。

## 保存 ChatGPT 对话记录到 Notion

![](img/1676fcacd392acfbb5b3fe88f69f3314.png)

**主要功能：**将与 ChatGPT 的对话直接保存到 Notion 中！

**传送门：**https://chrome.google.com/webstore/detail/chatgpt-to-notion/oojndninaelbpllebamcojkdecjjhcle?hl=zh-CN

## Notion+ 标记管理器

![](img/0a4c42079b87fe2c81aaea58148f9301.png)

**主要功能：**高效管理 Notion 的颜色文字与注解。你可以用 Notion 自带的页标注，还可以一键显示页面所有标注，轻松定位到当前标注位置，以及导出/导入各种标注。

传送门：https://chrome.google.com/webstore/detail/notion%2B-mark-manager/hipgmnlpnimedfepbfbfiaobohhffcfc?hl=zh-CN

## Notion Web Clipper

![](img/a73fb9e40b68ccab26e093b39c7ada57.png)

**主要功能：**有 4 个方式进行剪藏，精确剪藏。它最大的优点，也是其他几款插件做不到的功能（包括官方剪藏插件），他可以成功剪藏👸号的文章！

**传送门：**https://chrome.google.com/webstore/detail/notion-web-clipper/knheggckgoiihginacbkhaalnibhilkk

# 六、模板推荐

## Notion 官方模板

![](img/91fec95c57a409e0e18bdf7f7fa03342.png)

**说明：**官方提供了数千个模板，还进行了分类。

**传送门：**https://www.notion.so/templates

## 2.大神自制模板

![](img/158645f04f5ef7e0b691ef832022ae9c.png)

**简介：**很好看，但只有小部分免费，大部分收费，十几刀一个模板。

**传送门：**https://www.easlo.co/

## 3.Notionery

![](img/341fc0479d52068eb632fcbc1bf6ce00.png)

**简介：**包含 200 多个免费 Notion 模板的精选市场。

**传送门：**https://www.notionery.com/free-templates

## 5.Notion Pages

![](img/b25b266ff90a975de18b0bcbc076dd76.png)

**简介：**发现新的、高效的 Notion 模板。

**传送门：**https://notionpages.com/

# 七、应用案例

## 1.Notion 极简简历模版

![](img/3710b5973ca5bf7295fde16f7d55f2f9.png)

**说明：**这个模版可以帮助你在 Notion 中快速创建一份清晰的简历

**传送门：**https://zuozizhen.lemonsqueezy.com/checkout?cart=dacb67f3-ac17-4b4d-a0cf-f7185c45111e

## 2.自动总结🛰️群消息

**传送门：**【工作流】使用 Notion AI 自动总结🛰️群消息

## 3.批量总结翻译

**传送门：**[【Notion AI】王炸更新 批量总结](https://www.bilibili.com/video/BV1Nm4y147Dg/?spm_id_from=333.337.search-card.all.click&vd_source=05604b6063f398633af513a2e9a7c804)、翻译太爽了_哔哩哔哩_bilibili

## 4.充当短视频编导

问题 1：

您将充当短视频导演。您需要创作一个 3 分钟的视频，其中包含不少于 8 个镜头。请告诉我每个镜头的时长和文案，并确保第一个镜头可以吸引大家点击进来，请确保你的内容可以让用户感兴趣，以观看我们的完整视频。我的第一个要求是：如何保持良好的运动和饮食习惯。

问题 2：

您将充当短视频博主。请根据以上内容生成 500 字以内的短视频文案，确保第一个开头吸引人观看。请尽量讲述一些用户不知道的知识或内容，来吸引用户完整观看我们的视频。我的要求是：如何保持良好的运动和饮食习惯。（也可以根据需求进行补充第二要求、第三要...）

问题 3：

围绕上述视频文案，请推荐 10 个适合该视频的抖音背景音乐

问题 4：

围绕上述视频文案和配乐，帮我写生成 10 个不超过 15 个字符的标题，尽量做到吸引眼球。

## 5.角色扮演式问答

1）人事专员。

我希望你担任招聘专家。我将提供关于职位空缺的信息，你需要制定寻找合适申请人的策略。包括在社交媒体、社交活动或参加招聘会上接触潜在候选人，以便为每个职位找到最佳人选。第一个要求是：“我需要你来协助优化我的简历。”（在这里你可以把你的简历先发上去，在选中内容进行指令的输入）

2）医生。

我希望你扮演医生。我会向你提供我过去的一些治疗信息。请你帮我诊断可能遇到的潜在问题，并根据实际情况提供最佳解决方案以及其他有助于保持口腔健康的护理技巧。首要任务：“我需要帮助解决对冷热酸甜食物敏感问题，我过去的牙科治疗记录是一个月洗一次牙，每天使用水牙线”

3）私人厨师。

我希望你扮演我的私人厨师。给我推荐一些食谱，我需要这些食物营养丰富，饱腹感比较强，适合像我这样经常忙到没时间吃饭的人。另外需要考虑时间成本，希望整体菜肴健康且制作的速度比较快。首个任务：“推荐一些在午休时间快速烹饪的清淡且充实的食物，请注意我对坚果过敏”

4）脱口秀喜剧演员。

（在提问以前需要给到一些信息或者话题参考，也可以在提问的时候给到，放在你的问题以前或者问题后都可以。）
