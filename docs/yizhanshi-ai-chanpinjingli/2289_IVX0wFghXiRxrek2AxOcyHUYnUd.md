# 被夺舍的小爱同学！——把大模型接入小米音箱（小白操作全步骤教程）

> 来源：[https://v11enp9ok1h.feishu.cn/docx/AapXdz34TogBAhxY4uEcgVLMnTe](https://v11enp9ok1h.feishu.cn/docx/AapXdz34TogBAhxY4uEcgVLMnTe)

[项目作者是idootop，地址：h](https://github.com/idootop/mi-gpt.git)ttps://github.com/idootop/mi-gpt.git

官方教学视频：（不太适合小白）

<https://player.bilibili.com/player.html?bvid=1zb421H7cS&vd_source=89579609ff9a5bb5330aaf89ebde811a>

⬇️ 视频配套 PPT 文件

蓝奏云：[https://wwt.lanz](https://wwt.lanzout.com/iLHm2238xf5c)out.com/iLHm2238xf5c

阿里云盘：[https://www.alip](https://www.alipan.com/s/Yf2RQvN961T)an.com/s/Yf2RQvN961T

# 更新说明：

+   更新migpt-cli项目，作者是lmk123，项目地址：https://github.com/lmk123/migpt-cli

+   更新第二种只需要安装python和node.js的操作教学，建议小白直接采用第二个方案来做

+   node.js方案有些配置可参考第一个方案中的内容，我会标注出来到第几步去找

# 支持的小米音箱设备型号

## **✅ 完美运行**

已知可以完美运行 **MiGPT** 的小爱音箱型号有：

![](img/87ff6b30a9ba56f31d9734cacdf5da11.png)

## **🚗 正常运行**

部分机型的 MIoT 接口不支持查询设备播放状态[或查询状态](https://github.com/idootop/mi-gpt/issues/14)异常，比如小米音箱 Play 增强版（L05C），将会导致 MiGPT 部分功能异常，无法使用连续对话等，此时需要关闭 streamResponse。相关 issue

可以正常运行 MiGPT，但不支持连续对话的小爱音箱型号有：

![](img/052807ca8cd174fe6a7c939399d3cbc2.png)

## **❌ 不支持**

完全不支持 MiGPT 的小爱音箱型号有：

![](img/d67863bbe980236ec8f1c7fc05eaf6b5.png)

## 案例设备：我用的是小米AI音箱 1代，S12

+   先只看型号，找到自己小米音箱的型号是否在上面的列表当中

# 方案一、本地部署教学（vscode）

+   一遍看官方教学一遍摸索，不懂的地方问GPT，所以搭建过程用了挺久。如果稍微有点基础的话，应该半个小时到一个小时就能弄完了。

+   另外这个方案需要电脑一直开着migpt的服务才行，关了电脑大模型就不生效了，你可以把小爱同学当作你的电脑音箱，还是那种可以随时解答问题的音箱，其实也挺有意思的

## 第一步：电脑需要安装的软件和环境配置：

+   Python 3.8以上的版本 （ https://www.python.org/）

+   Vscode   （ https://code.visualstudio.com/）

+   nodeJS   （https://nodejs.org/zh-cn）

+   Github 需要有账号 （https://github.com/）

+   Github 经常会打不开，可以下载一个steam++，https://steampp.net

重要！安装完成后，需要配置环境，否则后面的部署过程中运行会报错

1.  搜索 windows powershell

![](img/d5de79a4979a5c05b030a1dd2f994f83.png)

1.  用管理员身份运行，进入命令行工具页面

![](img/c79860d6104d9a9277b3d391bde1dee3.png)

输入以下命令，按回车

```
Set-ExecutionPolicy RemoteSigned
```

![](img/1c161c709de49a3b37fad1e52d25b8fd.png)

第一次执行会弹出以下窗口，会出现执行策略更改确认时，输入A后按回车确认，关闭运行窗口

![](img/dbe4cec42f1429a3f54dba9e50383f95.png)

这一步我们做的是安装软件，配置基础环境

## 第二步：准备小米音箱

1.  查看小米音箱的型号

1.  型号1：小米AI音箱（1代），这个型号根据前述表格，型号码为S12（底部标签不一致，请按表格中型号为准



1.  型号2：小米AI音箱（2代），型号码为L15A（看底部标签）


![](img/9e8910254a08ed3f321c0a668312c165.png)

1.  手机上下载米家APP或者小爱音箱APP，这里有几个参数要记住：​

+   小米账号：登录米家或者小爱音箱APP后，可以在"我的"界面中看到ID，我这里是9开头的一串数字


![](img/4d76f3d64435bb05f924b440529f517f.png)

+   记住登录小米账号的密码：记得使用密码登录，等下需要配置

+   通过米家APP或小爱音箱APP配置小孩音箱设备，连接到家里的网络，请保持你连接的网络必须和电脑在同一局域网下

+   记住音箱的名称：我这里就是  小米AI音箱

![](img/afbcb1f9c1ab6a0e3da50a731e27ff78.png)

![](img/68c32eb81ac7f34a900c75cbeff2746b.png)

**在这一步，我们需要的参数是：小米UID，登录密码，音箱绑定的**名称，我这里绑定的名称就是 小米AI音箱

## 第三步：申请大模型的API接口，以silicon为例

官方提供的接入API的教学文档：​

<https://migptgui.com/docs/apply/>

+   以我平时使用的silicon接口为例，有众多开源模型（Yi**、Qwe**n、Llama、Gemma 等）免费使用。另赠送 14 元体验金，有效期未知，是个人认为API接口最方便最实惠的接口了

+   silicon注册和使用地址：

<https://account.siliconflow.cn/login?redirect=https%3A%2F%2Fcloud.siliconflow.cn&invitation=ESTKPm3J>

+   邀请码：ESTKPm3J（谢谢支持）​

+   注册登录后，单击左边栏的API密钥，单击新建API密钥

![](img/318cc650d1afe563319dd7d369f27b10.png)

![](img/bda8cef94e3c20efbb7ce49086ab479c.png)

![](img/c7caa9daac8133bef3f4570448bc3c02.png)

单击密钥即可完成API密钥的复制

+   silicon支持多种大模型，也支持文生图、图生图、文生视频，可自行探索

![](img/01452eb2e59b5eb0802aebe47e7f331d.png)

![](img/331d381f6265ce4ddd574cc9b4c65386.png)

+   这一步得到silicon的密钥即可，我们可以调用千问2.5的这个模型，满足日常对话完全没有问题，并且是免费调用的

![](img/8bd9389484b019521b474c08fbb16515.png)

**这一步我们做的是注册silicon账号，申**请API密钥，保存密钥，等下配置需要填写

## 第四步：进入github复制mi-gpt项目

打开官方仓库：https://github.com/idootop/mi-gpt

![](img/78fe3d634dd62ec895d8ccec97e0c609.png)

Fork项目到自己的账号

单击Fork

![](img/1f0e8a6e17ac8e4c8bba463f2a7ed9ed.png)

单击右下角 Create fork

![](img/5ebff9af2c79563b3a39d12fed6fe34a.png)

这样就已经把该项目fork到自己的仓库中了

![](img/56600ec0bdd455fbef0b00b334b36c68.png)

找自己仓库项目的方法

进入主页，单击左上角的"三"

![](img/817dfb94e9014f5498a70b4b598b37e0.png)

单击home，就可以看到刚才fork过来的mi-gpt项目了，单击可进入项目页面

![](img/847494302027a420ab30cbaafef3b2aa.png)

![](img/1113c127964d9dca0a602579d332b46a.png)

制仓库地址：先确认是否在自己的仓库项目页面，再单击绿色的“code”按钮，再点击地址栏的复制按钮

![](img/87cab467784371ce8c3374f353010565.png)

这一步是复制官方项目到自己的仓库里，并得到自己的仓库地址

# 第五步：打开vscode，克隆项目，并找到配置文件

打开vscode

![](img/6585492477e6ef7243939a4ed10a5be2.png)

安装中文插件，到下图这个界面，再次打开vscode就变成中文的了

![](img/c7f29b4e8dfe6cebe29a3dc71a3f0d0f.png)

选择一个文件夹，文件夹名字请随便起，我们可以起一个migpt的文件夹，到下图这一步：

![](img/89f535f40cf91db6d16093786dc2a1a4.png)

![](img/236932167be8856d16e3406b19ba3743.png)

![](img/81b92bd3583a35e0f06d8c87a761f17b.png)

![](img/1b6cb1dfeef382b80c69ca46c528d02f.png)

如果弹出信任提示，单击“是”即可

在vscode中建立migpt项目终端，进入右图状态

![](img/3cb258506daf4ff0a648cd795fd285d0.png)

![](img/c2d0bfb86f71006b515a69b5eba10fc7.png)

输入 get clone 第四步复制的你自己仓库中项目地址：

如：git clone https://github.com/idootop/mi-gpt.git

![](img/054069a9937570191d643d353dbccc51.png)

按回车，克隆仓库项目，出先下图即克隆完成，点开左边栏mi-gpt就可以看到项目文件

![](img/f11866412f2da586d719182174f5d3a7.png)

如果出现这样的报错：fatal: unable to access 'https://github.com/em......y/mi-gpt.git/': SSL certificate problem: unable to get local issuer certificate，可运行下面的代码后重新克隆一遍

![](img/cac9e952952a43477ff6723c16e16074.png)

修改配置文件，这里两个带有example的文件就是我们需要修改的文件

![](img/419874671455d20023ae3533cdaa8a88.png)

重命名这两个文件，修改为.env和.migpt.js

![](img/1a6ec28cc8cb66bc931335e3bb4de943.png)

![](img/33bcc8b9ef732fa4e7ab4f9801e2010d.png)

# 第六步：修改vscode配置文件

修改.env文件，可参照官方文档

这里我们使用千问2.5-7b-instruction模型，配置参数如下：

```
OPENAI_MODEL = Qwen/Qwen2.5-7B-Instruct
OPENAI_API_KEY= sk-xxxxxxxx   # 这里填写第三步中申请的API密钥即可
OPENAI_BASE_URL= https://api.siliconflow.cn/v1
```

修改完成后如下图：

![](img/eaeb53a7a746c49a85f9ef4272b14d5e.png)

接下来就可以配置.migpt.js文件，可参考官方文档修改指令和大模型的人设提示词

填写第二步我们获得的小米ID等信息，分别对应小米账号ID，登录密码，设备名称，大概在93行左右

![](img/f08d98a8b4f32e0c75fbe7167cbe19b0.png)

```
    // 小米 ID
    userId: &quot;9xxxx&quot;, // 注意：不是手机号或邮箱，请在「个人信息」-「小米 ID」查看
    // 账号密码
    password: &quot;axxxxx&quot;,
    // 小爱音箱 DID 或在米家中设置的名称
    did: &quot;小米AI音箱&quot;, // 注意空格、大小写和错别字（音响 👉 音箱）
```

修改MIoT设备指令，如下图

![](img/50f9b7b6f2c131d28ddfb72c7963962c.png)

我自己的两台设备，我在表格中找到的指令是：

![](img/76b9936fabf71786bfce85a2869195bd.png)

+   我绑定了S12小米AI音箱，表中command分别是[5，1]，[5，3]，和默认的一致则不用修改，表中playingcommand为空，则不用添加

+   如果是L15A，那么就要改成对应的编码，并把playingcommand之前的两个//删除，如下图

![](img/96c41de13085632ba14286f339a5e5ee.png)

+   切记，这里的所有符号必须使用英文符号，否则会报错

修改人设，可以参考官方文档，也可以自行设定，，可以先跑通再改，提供可复制的案例如下：​

第4-16行，人物提示词，可以先跑通再改

```
// 小爱音箱扮演角色的简介
const botProfile = `
性别：女
性格：高冷御姐
爱好：喜欢看电影，看书，偶尔也会去健身房锻炼，擅长游泳，爱吃醋，爱撒娇，喜欢思考哲学问题，很爱干净，喜欢穿性感服饰给哥哥看，特别喜欢挑逗哥哥。
`.trim();

// 小爱音箱主人（你）的简介
const masterProfile = `
性别：男
性格：善良正直
其他：明星，总喜欢打篮球，唱，跳，RAP，练习时长两年半，是莉莉娅的男朋友。
`.trim();
```

第78-87行，人物姓名（参考），可以先跑通再改

```
export default {
  systemTemplate,
  bot: {
    name: &quot;莉莉娅&quot;,
    profile: botProfile,
  },
  master: {
    name: &quot;哥哥&quot;,
    profile: masterProfile,
  },
```

第104-119行，回复唤醒指令，更改姓名，让名字一致，可以先跑通再改

```
   // 当消息以下面的关键词开头时，会调用 AI 来回复消息
    callAIKeywords: [&quot;请&quot;, &quot;你&quot;, &quot;莉莉娅&quot;],
    // 当消息以下面的关键词开头时，会进入 AI 唤醒状态
    wakeUpKeywords: [&quot;打开&quot;, &quot;进入&quot;, &quot;召唤&quot;],
    // 当消息以下面的关键词开头时，会退出 AI 唤醒状态
    exitKeywords: [&quot;关闭&quot;, &quot;退出&quot;, &quot;再见&quot;],
    // 进入 AI 模式的欢迎语
    onEnterAI: [&quot;我是莉莉娅，又见到哥哥啦，太开心了&quot;], // 设为空数组时可关闭提示语
    // 退出 AI 模式的提示语
    onExitAI: [&quot;莉莉娅先回去咯&quot;], // 为空时可关闭提示语
    // AI 开始回答时的提示语
    onAIAsking: [&quot;让我先想想&quot;, &quot;请稍等&quot;], // 为空时可关闭提示语
    // AI 结束回答时的提示语
    onAIReplied: [&quot;我说完了&quot;, &quot;还有其他问题吗&quot;], // 为空时可关闭提示语
    // AI 回答异常时的提示语
    onAIError: [&quot;啊哦，出错了，请稍后再试吧！&quot;], // 为空时可关闭提示语
```

```
    // 当消息以下面的关键词开头时，会调用 AI 来回复消息
    callAIKeywords: [&quot;请&quot;, &quot;你&quot;, &quot;莉莉娅&quot;],
    // 当消息以下面的关键词开头时，会进入 AI 唤醒状态
    wakeUpKeywords: [&quot;打开&quot;, &quot;进入&quot;, &quot;召唤&quot;],
    // 当消息以下面的关键词开头时，会退出 AI 唤醒状态
    exitKeywords: [&quot;关闭&quot;, &quot;退出&quot;, &quot;再见&quot;],
    // 进入 AI 模式的欢迎语
    onEnterAI: [&quot;我是莉莉娅，又见到哥哥啦，太开心了&quot;], // 设为空数组时可关闭提示语
    // 退出 AI 模式的提示语
    onExitAI: [&quot;莉莉娅先回去咯&quot;], // 为空时可关闭提示语
    // AI 开始回答时的提示语
    onAIAsking: [], // 为空时可关闭提示语
    // AI 结束回答时的提示语
    onAIReplied: [], // 为空时可关闭提示语
    // AI 回答异常时的提示语
    onAIError: [&quot;啊哦，出错了，请稍后再试吧！&quot;], // 为空时可关闭提示语
```

第149-156行，打开连续对话功能，是否支持请根据官方文档自行判断

（部分机型的 MIoT 接口不支持查询设备播放状态或查询状态异常，比如小米音箱 Play 增强版（L05C），将会导致 MiGPT 部分功能异常，无法使用连续对话等，此时需要关闭 streamResponse。）

```
    // 是否启用连续对话功能，部分小爱音箱型号无法查询到正确的播放状态，需要关闭连续对话
    streamResponse: true,
    // 连续对话时，无响应多久后自动退出
    exitKeepAliveAfter: 30, // 默认 30 秒，建议不要超过 1 分钟
    // 连续对话时，下发 TTS 指令多长时间后开始检测设备播放状态（默认 3 秒）
    checkTTSStatusAfter: 3, // 当小爱长文本回复被过早中断时，可尝试调大该值
    // 连续对话时，播放状态检测间隔（单位毫秒，最低 500 毫秒，默认 1 秒）
    checkInterval: 1000, // 调小此值可以降低小爱回复之间的停顿感，请酌情调节
```

这里一般只需要把连续对话功能改为true或false，如果设备支持可以改为true

保存文件，打开左上角文件，单击全部保存即可

![](img/df5180f437871047a7140b8517025686.png)

# 第七步：安装依赖并运行

在下方终端窗口，单击终端，输入 cd mi-gpt

```
cd mi-gpt   #这里的F:\migpt是你的文件夹目录，使用cd命令 进入mi-gpt文件
```

![](img/09b2c25cbcc3fb4e4063e44542769eed.png)

安装依赖，输入 pnpm install

```
pnpm install
```

安装完成后，界面如下：

![](img/523acd19373f9be7ff3eedcf5060046d.png)

如果安装过程报错，出现下图情况：

![](img/739f61e3e74ec1cf8c18b342913280c1.png)

请在命令栏输入：npm cache clean --force  清除缓存后重新运行 pnpm install

```
npm cache clean --force
```

![](img/7cb36a754cc745f988f8817068a0a786.png)

构建项目，输入 pnpm build

```
 pnpm build
```

出现以下界面即安装完成

![](img/29872b46316126ab4b0303e2231c11e0.png)

如果出现以下报错，找到mi-gpt中 package.json文件，将第28行build进行修改

![](img/f54f4e0bcacefe2cd24715af379dd92d.png)

```
npx -y prisma generate &amp;&amp; tsup
```

注意不要把引号删除。修改完成单击文件，一定要保存

![](img/e1d1df558cb4801c741d20c19d3d8e5d.png)

保存后运行pnpm build，即可正常安装完成

运行项目，输入pnpm run dev

```
pnpm run dev
```

运行成功时，进入下图页面，每次对话都会在终端中显示

![](img/b15b9e491df114f063f62a49b66278b8.png)

![](img/db83b8f933ec7894cdffa7cdfed36c7c.png)

每次需要打开服务时，按照下图方式进入终端，先输入cd mi-gpt ，再运行 pnpm run dev 即可打开服务

![](img/27d488b75bf9afe661a7e2bb1a476d2e.png)

如果出现以下错误不要慌，是网络问题，请重新运行 pnpm run dev

![](img/0a7d73865a35d01f91f3962812453af0.png)

看下运行效果，记得给大模型思考的时间，说话太快会出问题的