
# 通过 Warp 更换 VPS IP

> 来源：[https://docs.feishu.cn/docx/KqSgdbBZhoTo4Ax2jc7cC1Fhnle](https://docs.feishu.cn/docx/KqSgdbBZhoTo4Ax2jc7cC1Fhnle)

## 引言

首先非常感谢7Ebra，允许我搬运他的教程

搬运教程，7Ebra的【403】通过Warp更换IP的实验性解决办法

已经获取7Ebra许可

![](img/1b9c61897b6c98b70da4db6615f7c742.png)

（原教程作者看完搬运教程直言）

![](img/c39977d4dc67519a390ed75d6c2fce8f.png)

## 正文

教程基于1Panel终端，理论上其他终端也可以

实验性解决办法，不保证所有人可行

### 1. 打开服务器终端

![](img/1f1de4f1bfceb4cfb989562f6eba8ff5.png)

### 2. 输入下面代码然后回车

```
wget -N https://gitlab.com/fscarmen/warp/-/raw/main/menu.sh && bash menu.sh
```

输入`2`回车，即选择简体中文，之后等到安装依赖进入菜单界面

依次选择`1 1 1 3`

![](img/871f19670258a70eab7ffaebaf0984ad.png)

### 3. 成功换 IP

![](img/eacf8c6e0a94afd27202af67129db38e.png)

> 确保Clewd和SillyTavern都是`host`网络

至此应该已经可以了，请在酒馆自行测试回复

如果还是不行可以试试下面方法

### 继续更换 IP

```
warp
```

输入`4`回车关闭`warp`再打开更换 IP

![](img/64591562342714d95ed834d26a54b3bb.png)

第二种：此过程可能等待时间较长

```
warp
```

然后输入`6`

![](img/8f4e7e8152fa353b4115fa6aaf11aecc.png)

## 以下内容可以忽略，我要挂人了

![](img/ea19c1204f6f7f37a95d8cda7ed0bbc8.png)

顺带一提以上`hgfdsa`、`星様`都是作者的宠物
