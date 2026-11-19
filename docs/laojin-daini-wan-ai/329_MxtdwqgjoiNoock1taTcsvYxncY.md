
# BizyAir云节点

> 来源：[https://my.feishu.cn/docx/OIUldVko7oEuCGxgoCQcDLHpnRe](https://my.feishu.cn/docx/OIUldVko7oEuCGxgoCQcDLHpnRe)

自从AI绘画一步步迭代，显存是真不够用了呀，不够，死活不够。

看看老金我的电脑，天天开着本地`Dify`+`Ollma`+`ComfyUI`，没事儿就崩一次。

但是！今儿老金发现了一个神奇的东西，显存需求直接降至最低！

低配电脑首选！它还是来源于硅基流动（`Siliconflow`）的一款插件。

它是`ComfyUI`的云节点插件`BizyAir`，最新版本支持上传`Lora`！

## 使用条件

[安装说明](https://tffyvtlai4.feishu.cn/wiki/T6KswU6jmiDMOUkgovhcK0XHncd) 地部署了`ComfyUI`，如果没安装，请看 。

## 插件下载地址

可以直接从`GitHub`下载 `BizyAir`（`https://github.com/siliconflow/BizyAir`），也可以在节点管理器里搜索 `BizyAir`。

## 安装步骤

1. 安装好插件后，重启一下服务，这步千万别忘了哦！
2. 如果你之前已经安装过了，建议先删除再重新安装一次，确保版本是最新的。

## 直接懒人教学

直接懒人教学，最简单的办法：【管理器】里，找到【节点管理】，搜索 `BizyAir`

![](img/f04ad83d18d7c240caa7763896c00d77.png)

![](img/d2615a0cd77b79285dd9f0b78117c5de.png)

如果你不知道`API KEY`是什么，没关系！老金我教你怎么弄。

你可以去[`Siliconflow`官网注册](https://cloud.siliconflow.cn/i/wdyoipMi)（`https://cloud.siliconflow.cn/i/wdyoipMi`）。

登录后在左侧菜单找到`API密钥`。

拿到`Key`之后，下一步就简单了。

## 插件激活

重启服务后，如果你看到弹出的`Key`输入框，那就对了！

没有弹出来的朋友也别慌，再重启一次`ComfyUI`，弹框会再次出现。

![](img/6c6a02ee8658a1ffa50ec74b99a3b678.png)

关闭后，在再次弹框里，把`Key`输入进来，在`Siliconflow`官网上先找到`Key`。

![](img/97d340d7306d0149f15302147b2ee4b0.png)

在官网拿到的`API KEY`输入进去，就可以愉快地继续下一步了。

![](img/8abd10939cf6ccf7bb311a0c84986060.png)

## 上传Lora步骤

完成后，在`ComfyUI`界面上，你能看到这样一个紫色的条，直接讲解怎么使用`Lora`。

![](img/7dd5b42f6dfe87cf26f1eceefc8e7818.png)

点击`Upload`提交`Lora`。

![](img/e69a01adf1e7afafe7c39c0c2af29916.png)

直到谈出提交成功之前，不要进行任何操作。

![](img/81b165d88094334e011b4b85911dde6f.png)

提交完成后，在【`Remote Folders`】中可以看到。

![](img/ea12abd8614094868acf0499798d17d1.png)

上传完成后，我们重启一下`ComfyUI`。

点击【`Examples`】可以看到已经设定好的一些工作流，选择【`Simple Lora`】

![](img/c1dc8dad785e1e844eb4b47cebd2ffd7.png)

可以看到`Lora`里已经有了我们提交的网红`Lora`。

![](img/5b64f8baf2a92a16ab78466671bf62f7.png)

输入个提示词：

```
xhsgirl,A woman with long white hair is wearing a black dress. She has a silver necklace around her neck. There are earrings hanging from her ears.
```

大功告成！

![](img/8e4c11653833e6ff44466e9b88b2ca47.png)

![](img/abdb5573014423b57fdff85ae63fe84a.png)

当然它里面也有很多节点可供大家选择，模型也有`Kolors`等等，用于生成汉字。

![](img/c8e99c6329cff11bade11cf42cf35c6b.png)

![](img/dd51c56c30474f4ae82be04d6e38fed3.png)

`https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc`
