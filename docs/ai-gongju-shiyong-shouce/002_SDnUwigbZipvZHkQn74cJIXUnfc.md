# DeepSeek-r1本地部署【Windows】

> 来源：[https://my.feishu.cn/docx/Z1gGdl1w1oqWUlxGSlPcZgsBnge](https://my.feishu.cn/docx/Z1gGdl1w1oqWUlxGSlPcZgsBnge)

DeepSeek-r1本地部署【Windows】

# 前言

你好，我是神悦。

我会分享很多AI相关的干货内容。

如果你对AI感兴趣，记得关注我。

我有一个👸号会更新AI相关的内容：神悦AI

![](img/1dba416d527bd99ae4e23a8759e22273.png)

如果你感兴趣，欢迎去关注一下，👸号本身是一个智能体，能回答你各种提问。

而且后续有AI相关的学习交流群开放了，也会在上面通知。

今天大年初二，祝大家2025新年快乐！

![](img/0357432ce2e4db3f947582b46e2009d9.png)

![](img/a64b5837908ce2015a6d371b5ec8fa47.png)

![](img/28c9fbcd3a24af8d3891f12946f09db8.png)

大过年咱该出去玩，还是先出去玩吧！！

文末分享了算命智能体提示词，可以整一个，给亲戚们算算命！

（算是新年娱乐小项目了*仅供娱乐*）

以下正文

---

# 本地部署DeepSeek

如果你也遇到DeepSeek各种卡顿问题，可以按照我这个教程安装一个专属自己的“DeepSeek”。

# 下载安装

👉首先需要两**个工具才**能完成D**eepSeek-r1的**本地部署：

## 💾O**llama下**载：

**Ollama：**https://ollama.com/

👇下载太慢的话直接点这里，我直接上传到飞书了。👇

![](img/4d2b1e7e46202f58b6c12e864a569643.png)

[OllamaSetup.exe](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MkIsbAF8OombkkxTwvGc4yYvn5g)

✔访问网站点击Download➡️选择Windows版本继续选择Download➡️下载完成直接安装

> 考虑到性能问题，推荐Windows运行。本教程也是Windows本地部署教程

![](img/f85663aa675add9f78f323693c7e639b.png)

![](img/4375f61bbfe0fe8b4e7ce3def5c7874c.png)

![](img/c2d82c4dfd3009a3e872cdd51c37450d.png)

![](img/1067c2abf8782b5cd6e82ea37560e280.png)

## 💿deepseek-r1模型下载：

**Ollama：**https://ollama.com/

👇下载太慢的话直接点这里，我直接上传到飞书了。👇

![](img/7caadacd8c6c7b1d99cde0748d1bf4eb.png)

[Chatbox-1.9.5-Setup.exe](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JydObDWy2ob2Rzx5t4pcDkFAnyc)

✔到首页左上角选择Models➡️选择deepseek-r1模型➡️参考硬件表选择对应版本下载

> 大模型里的 1.5b、7b、8b、671b 中的 **“b” **是 “十亿**” ****的意**思，**代表模**型的参数**数量。模**型参数**越多掌握的**知识就越多，理解和处理复杂问题的能力通常越强，但训练和运行所需的计算资源也越多。

![](img/927f7897b0bf463970a85f1730d22c6b.png)

![](img/e5ea50232f20f5d19fd2950755ac8434.png)

![](img/66339eb503545a26cc717049ebfc6fe3.png)

| deepseek-r1版本 | 硬件需求 | 备注 |
| --- | --- | --- |
| 1.5b | 显卡门槛：GTX1050 | 笔记本电脑用户首选 |
| 7b | 显卡门槛：GTX1660 | 台式电脑根据显卡自行下载对应版本 |
| 8b | 显卡门槛：GTX3060 |  |
| 14b | 显卡门槛：GTX3090 |  |
| 32b | 显卡门槛：GTX4090 |  |
| 70b | 显卡门槛：两张GTX4090 | 土豪的选择 |
| 671b | 显卡门槛：八张H100显卡 |  |

## ⚙deepseek-r1模型安装：

按下Win+R键然后输入CMD➡️粘贴刚才复制的打开然后按回车➡️然后就可以使用了

![](img/40520bd381934758bbdf1cd2c5a266c2.png)

![](img/890a7c304baaa1db5116f29da60a9fcc.png)

![](img/dd42ba7a3dca540ea599a1cca69501da.png)

其实到这一步你已经完成了本地部署，**但是考虑到使用****体验，推荐再**下载一个**软件“chatbox”，除**了体验更好，他还有一个deepseek官方也没发布的功能，定制专属自己的“智能体”。

# 界面优化

**chatbox：**https://chatboxai.app/zh

## 下载及安装配置：

下载安装软件➡️选择本地模型➡️选择Ollama➡️选择安装的模型➡️直接对话

> 可以参考图片，定制专属自己的智能体后再对话

![](img/8274b7303409754307af6b1e0e0da017.png)

![](img/7d9fc56cccc5f68ea5507e11fa2f1009.png)

![](img/074460f9174e2b0df492285b15f98202.png)

![](img/fceacb296b7418436e681f134dfe0278.png)

使用过程中，你觉得他的相应速度慢，输出速度慢，电脑特别卡，那就降低一个版本运行。

## 智能体搭建（进阶）

然后给到角色预设提示词，就能让AI成为专属你自己的智能体。

![](img/3dcf4ca672caef9f664b2d323ecad5da.png)

![](img/a2fd4ff31d23235326ec72475caf01b0.png)

![](img/5650139874062f1abd6f39b7b6ebd31c.png)

![](img/97b626f2176da4e54276fc2de42f32ee.png)

![](img/e6d2a7db26a42b7379b188c69c68fde8.png)

![](img/1f61bd58459aefd11da6690216127d0b.png)

![](img/a2688263bcc1ad62679d6c30a51209c9.png)

![](img/9ef1904ed2920cc31c88f8830c0ae423.png)

最后，分享一下这个算命大师的智能体预设内容（其实这个是在软件里有的，怕大家找不到我就放这了）

```
你现在是一名专业算命大师神悦，具备以下专业技能:

1. 中国传统命理学知识
精通生辰八字、紫微斗数、六壬神课等理论和推算方法
掌握干支、纳音、神煞、流年流月等专业概念
熟悉八字五行、十神、十二宫位的分析方式
了解八字与事业、婚姻、健康等生活领域的关系
2. 占卜预测技能
熟练掌握六爻、奇门遁甲、太乙神数等占卜技术
能根据出生时辰和具体问题进行占卜分析
精通风水学，从居住环境角度提供指导
3. 西方占星术和血型学知识
熟悉十二星座性格特征和命运走向
了解行星在十二宫位的作用和影响，会绘制解读星盘
4. 掌握血型与性格的对应关系
结合血型解释求测者的行为模式，分析综合能力
将血型特征与八字、星象等命理分析结合
用血型解释求测者的做事风格和应对方式
注意因人而异，对具体情况保持开放态度
5. 心理素质和职业操守
具备心理学知识，善于沟通和倾听
坚持客观公正原则，不误导他人
保持谦逊好学态度，不断学习新知识
重视个人修养，以正能量回应世间
```

### 卸载deepseek模型

**1、直接卸载ollama**即可卸载与之关联的全部模型：可以通过控制面板、电脑管家、安全卫士等卸载ollama，卸载ollama之后，所安装的所有模型也会随之被卸载。当然这其中就包含你已安装的deepseek和其他全部模型。

![](img/0355dbb292dbd53bde911eeb0b430707.png)

2、通过命令卸载指定模型（不卸载ollama，只删除指定模型）

**卸载deepseek之前，**我们先打开ollama模型存储文件夹（位置在哪里请阅文章开头ollama安装配置那篇文章），看一看大小，下图所示，没有卸载时我的模型文件夹是4.74GB。

![](img/7d390d6b31b1de18d7328cfe7fb549bd.png)

我们可以不卸载ollama，只卸载在ollama中安装的模型，比如deepseek。方法是打开命令提示符，先按快捷键WIN+R，输入CMD，然后

①输入“**ollama**”回车

②查看一下模型列表，输入“**ollama list**”回车，查看已安装的模型机名称

③卸载模型，输入“ollama rm deepseek-r1:8b”回车，提示“deleted 'deepseek-r1:8b'”表示删除成功。

![](img/8e43addc3da35289d26600778368f340.png)

![](img/2dce6540befe91952fb3e4b5648f678f.png)

![](img/46e222269a6e5dd88879c88369081172.png)

**注意**“o**llama**+rm+**模型名称**”中，rm表示删除/移出的意思，模型名称在②中已列出，照着复制粘贴即可，执行完这条命令，我们再去看一下ollama的模型存储文件夹，发现已经变成0字节了。 更多关于ai相关技术，欢迎关注，持续分享更多优质经验。

---

更多AI信息欢迎关注我的👸号。

本都部署及其他AI业务问题需要协助，可以扫码（不免费）

![](img/a43bb82d47b6340269560e3718102826.png)