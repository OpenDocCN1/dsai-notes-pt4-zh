# 安卓本地部署 Clewd+酒馆 Clewd旧版

> 来源：[https://sqivg8d05rm.feishu.cn/docx/UnzIdcKxJoJEs6xirDacErTIncf](https://sqivg8d05rm.feishu.cn/docx/UnzIdcKxJoJEs6xirDacErTIncf)

作者：

本旧版教程已不可用，仅做备份，请看新版教程：

[安卓本地部署 ClewdR+酒馆](https://sqivg8d05rm.feishu.cn/wiki/EY5TwjuwliCwZpk7Gy7cPGH1nvb)

# 一、喵喵~一键安卓脚本部署（Termux）

> **视频教程：****https://www.bilibili.co**m/video/BV14W4y1P7Tg

需要准备：

+   一部安卓手机，大概需要2g的存储空间，400mb的流量。

+   稍微好一点的魔法（魔法很重要）只有游戏加速器的对照换一个：

+   [霍格沃兹魔法|使用工具|订阅导入全系列教程](https://sqivg8d05rm.feishu.cn/docx/SgfCd73rLowe9vx46kVcuZdRn3b)

+   几个能用的Cookies，参考: [注册/抓取曲奇/购买 Cookie](https://sqivg8d05rm.feishu.cn/wiki/HST5wAAxeibfzEkrCvHcIU8dnge)和[水秋喵的小店](https://item.taobao.com/item.htm?ft=t&id=752741890015)。

## 1.配置环境

1.  **在群文件中或者**[**网盘(点击下载)**](https://www.123pan.com/s/P8OAjv-Glhtv.html)**找到Termux并安装。**如果安装不成功请检查手机是否已经装过termux/zerotermux，部分手机安装时需要关闭网络。

1.  **启动Termux****，打开termux,****输入下方的完整命令并回车。**脚本编辑：

```
curl -O https://raw.githubusercontent.com/hopingmiao/termux_using_Claue/main/tisac.sh &amp;&amp; chmod +x tisac.sh &amp;&amp; ./tisac.sh
```

1.  **等**待脚本自动下载安装，直到出现脚本主界面，如果出现报错，请检查自己的魔法是否有问题。安装失败或者中途卡住多半是魔法问题，换一套配置重新运行指令即可。

## 2、配置Clewd与酒馆

1.  **输入2以启动酒馆****，**出现**绿色网址****（如：h****t**tp://127.0**.0.1:8**000/）代表成功启动；若无自动跳转，则需要自行复制网址到浏览器打开，打不开的话尝试换个浏览器。

> 注意：Termux需要挂在后台才能正常运行，请使用锁后台等方法对Termux进行保活；

> 华为/荣耀：手机管家-启动管理-Termux设置为手动管理-勾选允许后台活动。

![](img/0e956a0054791747e5bfd8846324694f.png)

1.  稍等片刻弹出欢迎页，填写**用户名**进入酒馆。注意不要勾选Enable simple UI mode，会导致功能缺失。

![](img/cbfebafcc1d54da046cb2e10e222fef0.png)

1.  **回到Termux，对着手机左上部分边缘屏****幕右滑，然后点**击NEW SESSION。第一次会比较难滑出来，实在不行可先在手机系统设置中关闭全面屏手势。

[zerotermux设置方法](https://sqivg8d05rm.feishu.cn/wiki/DIuDwmwikigBx1k7TugcxNbwnYb)

![](img/ae139b8a79cf0ecb8857602c24c5a8e4.png)

![](img/843263e674f0c694194beb7234d9d68d.png)

1.  输入 3 选择修改Clewd设置，再选择Cookie**添加**。**填完所有 Cook**ie 之后先回车再按Ctrl+D退出。

Tips：如果是[买的Cookie](https://h5.m.taobao.com/awp/core/detail.htm?ft=t&id=924386149588)，直接全部复制并直接粘贴即可。

![](img/a92352906c5812bd75774c10b3f6f175.png)

![](img/d0d1ebebe904637d7a83fb55f15f9a4c.png)

1.  将PassParams的值从true改为false，防止出现400报错。

![](img/ba653196114764b5a6f0b2a5e9450746.png)

1.  **然****后选1启动clewd。显示Lo****ggin in等字样代表**启动成功。该黄色网址便是你的Clewd反代地址（可尝试在浏览器打开）。

![](img/07124d2147e0b8171fb7aeba1df68d6f.png)

警告，不要急着进酒馆！确认启动成功了再下一步！

![](img/40952be0371812a7188460c9bb5bce17.png)

你确定出现logged in字样了吗？没有出现就不要进酒馆😭。

（302，403，404等为网络错误，需要更换节点）

1.  **回到酒馆界面，按照图示设置酒馆**

> 模型选择OpenAI的**gpt3**.5或者gpt4，Clewd会自动选择正确的Claude模型！自己瞎选可能会无法使用。

如图配置即可：

Chat Completion为聊天补全

![](img/74c32b41b8961c0a301a735ac9ddf557.png)

1.  点击connect，下面变成Valid就可以了。

![](img/357ee1538ae0560f751514b5eb46b0d4.png)

如果有bug反馈一下（应该是没有的，而且如果有，我比较懒，不一定什么时候会去改）

**已完成，下一步：**[酒馆入门（必修）](https://sqivg8d05rm.feishu.cn/wiki/J8NDwc06JiuHXmk5cP2csfP6nmd)** **

> 题外话：作者hoping喵是保底的🔥🐱（月月给你们一拳！）

！酒馆启动失败bug临时修复方法，

1.  主界面输入0退出剧本

1.  然后输入exit。

1.  依次输入以下命令：

```
pkg update &amp;&amp; pkg upgrade -y

pkg install git nodejs -y
```

# 二、手动部署Clewd+酒馆（Termux）

## termux下安装Ubuntu或者其他完整Linux环境

+   首先更新包管理器（没换源开魔法）:

```
apt update &amp;&amp; apt upgrade  
pkg update &amp;&amp; pkg upgrade
```

+   安装Ubuntu环境：

```
pkg install proot-distro 
proot-distro list #可以省略
proot-distro install ubuntu
```

+   添加Ubuntu到自启动：

```
echo &quot;proot-distro login ubuntu&quot; &gt;&gt;/data/data/com.termux/files/home/.bashrc
source /data/data/com.termux/files/home/.bashrc #刷新自启动文件
```

+   命令执行完后变成这样就是进入了Ubuntu环境，称为初始环境：

![](img/1d1c01127fc3a7e01c534136df54ea25.png)

+   由于配置了自启动，每次打开termux就会默认进入Ubuntu环境，可以重新把酒馆也部署在Ubuntu里面，如果要退出Ubuntu回termux打开酒馆记得新开一个终端页面（不会的别急，下面有说），然后输入：

```
exit
```

---

> Ubuntu参考：

> Termux安装完整版Linux(Ubuntu)详细步骤

> centos参考：

> Termux在Andriod安装centos

## 更新包管理器，下载git等命令

+   先确保出现这个页面：

![](img/e7720ae674337900fd814aadf4c05149.png)

+   如果是参考Ubuntu只需要下载git就行，自带node（虽然是20.2.0版本，但是也可以使用clewd）

```
apt update &amp;&amp; apt upgrade
apt install git
apt install tar
apt install xz-utils
```

+   这里推荐手动安装node

```
curl -O https://nodejs.org/dist/v20.10.0/node-v20.10.0-linux-arm64.tar.xz
tar xf node-v20.10.0-linux-arm64.tar.xz
echo &quot;export PATH=\$PATH:/root/node-v20.10.0-linux-arm64/bin&quot; &gt;&gt;/etc/profile
source /etc/profile
```

+   查看git和node版本

```
git -v
node -v
```

![](img/c15bfc18d32864417fa9f2cf591f48d8.png)

+   出现版本号就可以进行下一步了

## 下载酒馆和clewd

+   直接用git下载最新版酒馆和clewd：

```
git clone https://github.com/SillyTavern/SillyTavern
git clone https://github.com/teralomaniac/clewd -b test
```

+   先第一次启动一次clewd生成config.js文件：

```
cd clewd
bash start.sh
```

黄色字样就是说明“config.js文件已经创建好了，可以打开进行编辑”

![](img/feb4106d5dcd5abc949b065e6bd7a275.png)

## 配置相应文件

+   修改cookie文件：

```
vim /root/clewd/config.js
```

+   我这里安装Ubuntu环境时，默认安装了vim编辑器（**如果没有则****运行以下代码****进行下载**）

```
apt install vim
```

+   **vim使用方法****（重难点）****：**

1.打开文件后一开始随便按动键盘都是输入不了任何字符进入config.js文件的：

![](img/706d03dcaf5934217cd30095dfed1a39.png)

2.按一下键盘上的英文字母**“i”**进入编辑模式：

![](img/2340183d383b2630a797b3f78f81514b.png)

3**.编辑完成后按一下****ESC退出编辑模式：**

![](img/ab3827fa4c77fc5ea54093c59ebfb8a0.png)

**进入编辑模式后，找到****CookieArray :[]****这一栏，****在[]里面输入cookie****s,本地部署需要使用英文双引号""来包裹每一个**[**曲奇，英文逗**](https://clewd.pages.dev/)**号隔开**，曲奇太多的话，可使用一键整理工具来处理

> 标准格式例如：["cookie1","cookie2","cookie3","cookie4"],

4.这个时候输入英文:wq**!进行强**制保存（注意：输入的是“:wq!”），:wq是普通保存，一般情况下两个都行，输入完进行回车。（如果不小心无处或者弄错不想保存，则输入:q!进行不保存强制退出）

![](img/80c30eceda2fb0e1ac2ac337687d170c.png)

5.酒馆和clewd自启动两个选一个，两个不能同时存在

+   **设置clewd自启动**：

```
echo &quot;cd /root/clewd&quot; &gt;&gt;/etc/profile
echo &quot;bash start.sh&quot; &gt;&gt;/etc/profile
echo &quot;cd /root&quot; &gt;&gt;/etc/profile
```

+   **设**置酒馆自启动（不会自动跳转到浏览器页面，要输入地址 127.0.0.1:8000进入，建议直接在浏览器里面保存酒馆书签）：

```
echo &quot;cd /root/SillyTavern&quot; &gt;&gt;/etc/profile
echo &quot;bash start.sh&quot; &gt;&gt;/etc/profile
echo &quot;cd /root&quot; &gt;&gt;/etc/profile 
```

+   **二选一执行后，****刷新****页面保存更改生效：**

```
source /etc/profile
```

**6.****打开新终端教程方法：**

手指大概放在下图二的这个位置然后向右滑

（全面屏手势用户注意：如果划不动的话，请在手机设置的系统导航里，把侧面返回手势变更为经典三段式或者导航键）

![](img/f4239b1370a27fbee532676c3f69384f.png)

![](img/089f70a1f54daa434abf1de489372172.png)

**点击****NEW SESSION****：**

![](img/b8dc0e3cd985f5cfc43354029b3b3138.png)

## 酒馆连接clewd

**先在一个页面启动clewd****：**

> 成功启动后是这样的

![](img/77e7c6c427b5c8ca08d0402ac851d400.png)

**然后打开新终端,****输入以下命令行****启动酒馆****：**

```
cd SillyTavern
bash start.sh
```

![](img/337f17eb1128b071c79eaecd521df352.png)

然后浏览器会跳到这个页面（没有跳转的手动输入地址）

![](img/278a1b812412872f01fcbfe5c7a33fb0.png)

**点击第二个图标后把设置调成这样****，****g**pt-3.5-turbo等连接之后可以根据具体模型进行调整：

> 反向代理填：http://127.0.0.1:8444/v1，代理密码没有在clewd里设置就随便填一串数字，不可不填

![](img/1419d2a2dcd7269f2a66ca03ce8a07e9.png)

**再**回到第二个图标，点击connect，下面变成Valid就可以了：

![](img/d2f6eb98ffd91671241dc30e3665db36.png)

---

# 三、手动部署Clewd+酒馆（Aidlux）（太老旧了，Aid占内存太大，弃置不用，学tremux即可）

首先感谢月歌妈妈，三月七和还有群友们的帮助，本人自己也是在摸索中部署成功的，可能跟着教程部署也会有些怪怪的问题，教程里有问题的话请指出QAQ

![](img/5955ebb4f94fb8ee42e297ddae70f395.png)

不建议完全不懂的小白部署，因为会有很多问题，有的问题我在群里看到了我也可能会装看不见

如果出问题了，比如什么文件路径找不到，自己看看安装在哪个路径了

需要的东西:

Aidlux（应用市场里就能搜到，对就是那个1g多大的.... 我尝试了用其他软件，但总是遇到奇奇怪怪的问题，目前可能可行的是 Userland，大概流程是差不多的，我自己用一直装不了ubuntan文件，很无语，有成功的群友可以反馈一下)

手机预留4g的储存空间（可能会更多?)

🪜（本地部署需要🪜才能连，可能比较垃圾的🪜就算部署好了也会直接403ww）

记得部署过程中以及日后使用不要用香港节点，用克劳德支持的，比如新加坡

Aidlux: aidlux.com

各大应用商店应该也有的下载，如果没有就去下面的官网吧，下载有点慢就是

下载链接：https://community.aidlux.com/aidluxdownload

正式进入教程

## 部署clewd

##### 0

如果是之前装的云酒馆，怕被偷，想把酒馆也安装到手机上，可以跳过这一步

安装这个软件之后，你的本地酒馆端口会被占用，所以要先给酒馆的端口改一下

先打开Termux，然后打开手机资源管理器，找到Termux的目录

![](img/85e337ae28c6ec8ecdce73a0a496a050.png)

打开傻酒馆

找到 config.conf 这个文件

mt管理器打开，如果要求选择打开方式就选编辑文本

第一行写着 const port = 8000

把这个8000改成你喜欢的四位数 8888也行

接下来回到Te里打开酒馆，放着先

1.  把后台权限给了

![](img/4ca1e436608983c23a0f1679ed027426.png)

1.  先不要开🪜，打开aidlux，权限全部允许，进去后会自动装好东西，然后可能会有个提示，点是，跟着手机上面提示的操作，会让你打开开发者模式，打开无线调试（需要先连上WIFI），然后点进去，点击使用配对码。

PS：无线调试仅在Android11/Android11+系统可用，过低版本的Android和鸿蒙系统无此功能，也没有进行此步骤的必要。（新版本Android对幻象进程的限制所导致）

![](img/15625cc2d6f7c971b84bb28f37eb5907.png)

![](img/6d1d450772623a5694dba2e601c909e3.png)

给这个打勾

然后点进去，找到使用配对码配对设备，点一下，等一下下就好了，如果失败重复上面操作

![](img/208c40cbce0239f6e9b0fc27c5645075.png)

成功进入画面

这个时候连接🪜

回到软件，这个软件里面的窗口左上角

第一个红色圈圈是关闭

第三个绿色圈圈是挂后台

第四个蓝色圈圈是刷新

不要乱点

我们先点红色

1.  部署clewd

打开

![](img/45fd05b8f4677a6c77a1d71dcdf5dccb.png)

![](img/b34e1decbf6f765e639cdc267f7dde4a.png)

PS:搜狗输入法的英文输入法在这里会发癫，点东西一定要慢点，一定要注意大小写!!!!!!!!!!!!!!!!!!!!!!!!!!!!

2.1 （此步骤编写参考教程:   https://docs.qq.com/doc/DWERuTEJWclhQa0NV   (其实就是照搬qwq））

接下来跟着下面的方法输入，回车是确认

我这就不截图了，输入东西进去后，等里面不弹其他东西就输入下一个指令

用root权限更新Ubuntu/Debian的软件源包索引

```
sudo apt update
```

使用curl命令安装nvm版本管理工具，版本为0.39.1

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

重新加载shell环境:

```
source ~/.bashrc
```

使用nvm安装最新版本的Node.js:

```
nvm install node
```

用root权限在系统上安装git版本控制系统

```
sudo apt install git
```

克隆clewd到本地 （这一块全部复制进去都可以，#后面的东西他不会读取）

```
#克隆clewd修改版到本地
git clone https://github.com/teralomaniac/clewd.git
#切换当前目录到clewd的目录
cd clewd
#用一键脚本安装依赖并启动clewd
bash start.sh
```

##### 填入cookie

不过第一次运行不会启动clewd，而是生成config.js配置文件，需要填cookie进去

![](img/51441744a3726ab93f9f099913c24295.png)

看到如上图就是正确的了。

点击左上角的绿色圈圈，来到桌面，点左下角的文件管理，里面的排序是按照abcde顺序排的，先找到 root 文件夹(可能是home文件夹），然后点击clewd，路径找到config.js文件，按照图，把你的cookie填入"Cookie"后面的引号内，记得删掉“SET YOUR COOKIE HERE”。

![](img/bc8551dc9343b8196641d6c94a526c09.png)

还有个非常重要的!!!!!!  把ProxyPassword设置个密码如下

```
&quot;ProxyPassword&quot;:&quot;123&quot;,
```

如果有多个cookie，则清空"Cookie"后，按照如下格式，把cookie都填入"CookieArray": [],中212

```
格式如下，在[]中分别用&quot;&quot;包裹cookie，用英文逗号分隔，但结尾不能有逗号例如&quot;CookieArray&quot;: [&quot;cookie1&quot;,&quot;cookie2&quot;,&quot;cookie3&quot;]

#举例：
&quot;CookieArray&quot;: [
&quot;sessionKey=sk-ant-sid01-bTiK8Mf6XqHODdxmwV&quot;,&quot;sessionKey=sk-ant-sid01-Sokt3wB7c3Yx_PIbLK&quot;,&quot;sessionKey=sk-ant-sid01-YylqgeaI6X0YxIWBS3E3gCN7Fg1hR754lT-RtN&quot;,&quot;sessionKey=sk-ant-sid01-ycbnhdXsaJOnKDOUT_NnvIHejk42n&quot;
    ],
    
#不用试啦我给的假cookie
```

填好cookie后，右边三个点，点击然后save

接下来在clewd的目录下，输入命令bash start.sh

clewd！启动！

像下面这样显示了反代地址和你的邮箱，就算部署成功了

把这个反代地址复制到剪切板:

http://127.0.0.1:8444/v1

![](img/3c20cb901cb85363088b6507f5a26326.png)

![](img/a4606adff31148e22a6e238c812cbc6a.png)

##### 如何更新？

#切换当前目录到clewd的目录

cd clewd

#用update.sh一键脚本即可更新

bash update.sh

## 部署酒馆(已经在Te部署好酒馆的跳过该步骤)

接下来我们把clewd挂在这里不管他，点第三个圈回到桌面，接下来开始部署酒馆，已经在te上部署好酒馆的就跳过这一步

在AidLux部署酒馆好像会有奇怪的问题，不建议在这部署，建议回到te里部署.....

大概就是换回te，步骤跟下面的一样

先留空，到时候补充

回到桌面，还是点下面那个命令窗口，打开一个新的

克隆最新版酒馆SillyTavern到本地

切换当前目录到酒馆的目录

用一键脚本安装依赖并启动酒馆

看到“SillyTavern is listening on: http://0.0.0.0:8000/”就说明启动成功了！

![](img/afa8722807c14ace2fafe3f7420af355.png)

如果遇到这个情况

![](img/387b7855bfeaa7a8a3dff8e3e5af00c4.png)

参考步骤0，更改你的端口, 根据补充饼干的方法，找到

（可能是Home可能是root文件夹) ->SillyTavern ->  config.conf 这个文件

双击打开

第一行写着 const port = 8000

把这个8000改成你喜欢的四位数 8888也行

记得右上角的save

回来再次启动

这里记住你的网页

http://0.0.0.0:你改后的端口/

用绿色关掉，打开下面那栏的地球图标（浏览器），输入上面那个网站

## 酒馆连接clewd

打开你的本地酒馆，这样设置

如果是刚部署完成的话，点击上面的插头,这样

![](img/446487273e261b461c6a21f15928754b.png)

Chat completion选择openai

然后回到左边的输入反代，密码为刚才你设置的密码，我是123

![](img/0f237d762a4eadbd5bc506887c30da91.png)

回到插头页面，点击connect 连接到clewd，成功连接上clewd，插头会亮，再勾选那个

show external models

模型拉到最下面

找到Claude-2

connect 然后 text message 这下是真启动成功了