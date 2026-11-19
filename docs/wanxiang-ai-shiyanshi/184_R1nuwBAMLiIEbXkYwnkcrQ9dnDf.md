
# 保姆级影刀RPA中的飞书多维表格使用教程

> 来源：[https://yunyinghui.feishu.cn/docx/BrTSdzntQoxKmLxICZxcenFzn9g](https://yunyinghui.feishu.cn/docx/BrTSdzntQoxKmLxICZxcenFzn9g)

你好，我是龙哥，RPA和AI智能体,AI编程教练，专注于使用RPA，AI智能体，AI编程等AI技术提效。

在网上没有找到更简便的影刀RPA中使用飞书多维表格教程，挺麻烦的，很多教程也没有写清楚，与其这样，不如自己来写一篇教程

> 注意这里我只写最简单最实用的使用教程，以及帮大家避坑，很多人还是不太会在RPA中使用飞书多维表格。

## 一，打开影刀RPA查找必要的信息

| 1. 打开影刀RPA，然后在指令搜索框中输入：`表格` | ![](img/14199afa2d1c3aa10bce06a476cb23a6.png) |

| --- | --- |

| 1. 双击建立表格链接，发现需要3个参数，然后我们去飞书开发者后台获取`AppId`和`AppSecret` | ![](img/7abad3b99fb2fcdd4de9f941c43881a4.png) |

## 二，创建应用获取`AppID`和`AppSecreate`

| 首先打开飞书开发者后台(`https://open.feishu.cn/app`) 创建企业自建应用 | ![](img/1304249c7d5be8b9b4f5874c421963cc.png) |

| --- | --- |

| 填写应用名，记住这个应用名，后需要用到的 | ![](img/349fde0be313d2baea3f12273c04685d.png) |

| 获取`AppId`和`App Secret` | ![](img/ebf80fc389540778d1da301336e5e9dc.png) |

| 创建版本 | ![](img/7010d929a828bdfe73a45eab71edbe16.png) |

| 输入版本号，点击保存 | ![](img/3cf670a0abbfd2f7b06f69939e5af8ea.png) |

| 确认发布 | ![](img/04153fbdaa0bf6793040874ad5625062.png) |

| 1. 点击权限管理 | ![](img/30fa8483545c410248950c71ad807a24.png) |

## 三，在飞书中创建电子表格并获取`Spreadsheet ID`的获取(飞书多维表格也是这样创建)

| 点击主页和知识库所对应的空间都是wiki，这里面获取你的`spreadsheet`并不能直接获取，不建议用这种方式，我建议在云盘中创建电子表格，飞书多维表格也一样是这么操作的 | ![](img/e5031357d35d7db00f0b6cc81b6c417e.png) |

| --- | --- |

| | ![](img/2ab039104473f293ccb9dd9fc4bd069d.png) |

| | ![](img/6cdb80760a0e3dac9af3e33aa8c71b77.png) |

| | ![](img/77e9bd205679599e36afd52ea5782006.png) |

| | ![](img/272141cd3a6e1952941678c8df1af7c5.png) |

| 这个`SpreadsheetID`获取就是从上面电子表格的URL中`sheets/`后面的内容，红框中的内容 | ![](img/c4187e317527e606e61768f0f58723d1.png) |

| | |

> 注意这一步特别重要，很容易出错，很多教程都没有写清楚，在云盘中创建表格就不需要对表格ID进行二次转换，这个就特别方便了，很多教程没有写。

## 四，RPA中操作飞书电子表格

| 根据前面获取的`AppId`，`AppSecret`，`Spreadsheet`放入到建立表格连接中，测试 | ![](img/ff037385d786f9f25db8df6781c2b460.png) |

| --- | --- |

| 数据写入方式,参考影刀官方文档：`https://www.yingdao.com/yddoc/rpa/716937844642525184` | ![](img/13a54b2ef889ec896782a257edc7c75a.png) |

| 写入单元格的方式 | ![](img/9beedfc0f54b6ca1c02bfb7c50621871.png) |

| 按行输入，需要点亮图标 | ![](img/6c7bffaac3fc5e72df420d2d7dd24be1.png) |

| 按区域写入数据， | ![](img/41006c55427b03f3418f2741d3b62946.png) |

| 插入到其他的`Sheet`表中 | ![](img/61432c5b4070f0e825b2c36f01bfe628.png) |

| 读取行数据 | ![](img/98ac09cf2376d4e39a5cd4d0fc6a8ae4.png) |

| 读取区域数据 | ![](img/3bceb081a6e4624df7f9afc4a35af689.png) |

## 五，RPA中操作飞书多维表格

按步骤 [影刀RPA中的飞书多维表格使用教程](https://svox0dxsvym.feishu.cn/wiki/WKxTwz3bgij6JRkYkU7c31xdnpc#share-DxxodxbVYoYmotxrUtQcKJmanQh) 创建好多维表格

| 打开影刀RPA指令搜索输入`多维表格` | ![](img/05e2330112d1e21e18d7c4bb630d8363.png) |

| --- | --- |

| 打开多维表格的链接，在`base/***?table`之间的字段就是多维表格ID，直接复制就好了 | ![](img/06fa2c9195194acc796a37cb9950e507.png) |

| 获取到多维表格ID之后，填入测试，发现执行成功。 | ![](img/8eacf179b7d4b4e592b7d418abf250ea.png) |

| 修改飞书多维表格列名，和右图excel同步一个表头 | ![](img/481e38c2f6682357cce96485ed884fc6.png) |

| 列出字段 | ![](img/89e4b4bea26390b8878c9029d2eb3830.png) |

| 插入数据 | ![](img/97be188b8c5d10fc7bea1324eedc1830.png) |

| 查询记录 | ![](img/2d92f56ac2ff08adcca5a072b5f4c7f1.png) |

| 特别容易出错的地方 | ![](img/e714488b33b01cc61ccde20915cfda64.png) |

| 飞书多维表格的设计太牛逼了 | |

| 多维表格 | ![](img/23f9dd3d086dce9073ee9b73e9ae5433.png) |

![](img/7c69c059427e4697bfd363ac450f0da3.png)

![](img/c1b41d2f4e46feb1bd312fc6df1905e0.png)

至此影刀RPA中使用飞书表格和多维表格教程讲述完毕，赶快动手实操吧
