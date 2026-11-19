
# 2023-07-04【21天赛博朋克】AutoDL-rotbit镜像配置赛博朋克出图环境

> 来源：`https://xuqiwei1986.feishu.cn/docx/Ini7dhmcFo6Ff6xCIWLcE6N9njg`
> 🔗 原文链接：`https://articles.zsxq.com/id_k49zpbq9db5v.html`

昨晚吴东子大佬的直播很赞！从分析抖音短视频的异常数据到探索变现潜力，逻辑清晰、论据有力，真的很精彩。更重要的是，按照这个方式制作的作品酷炫到爆，我决定毫不犹豫地尝试了！当然不是为了赚钱，主要是因为太好玩了！

给大家看看昨天简单尝试的结果，可能做的没有其他大佬们做的好，不过也拿出来和大家分享下

![](img/9d3427e83827fdab8487d2a56308ac44.png)

接下来就开始讲解本节的内容，本节主要讲解使用`AutoDL`的`rotbit`镜像，如何配置赛博朋克的出图画环境。

看本节之前，建议先看 `https://t.zsxq.com/0fgBvV1AL`

在开始详细讲解之前，简单讲解下目前常见的一些AI绘图的配置方式。

## 一、部署方式介绍

### 1、本地安装

简单的说，就是在你自己的电脑上安装`stable diffusion`。这种方式对你的电脑显卡有要求，至少6G以上的显存。给大家贴一张显卡天梯图参考下，你的电脑如果是RTX 3060以上，应该是能跑的。

![](img/c029e7bc8d897eadfe896bdfa9473033.png)

### 2、阿里云安装

也有不少小伙伴使用阿里云的部署方式，据说可以白嫖机器，但这种方式其实我并没有研究过。星球里面有这类教程了，所以我不会在这里详细讲解

### 3、AutoDL方式安装

这种是我当前一直使用的方式，目前是1.24元/小时，不用的话就关掉。因为是我自己维护的镜像，有问题解决相对来说方便一些。这次针对赛博朋克出图环境的配置主要是这种部署方式进行。

以上的不同部署方式，对于出图效果没有太多的差别，出图的效果还是跟模型和提示词有关系。

## 二、环境部署

不知道怎么使用`AutoDL`的`rotbit`镜像，请参考 `https://t.zsxq.com/0fgBvV1AL` ，里面说的非常非常清楚了，看完还不懂的找我。

在部署完成之后，我们就开始安装 使用`AutoDL`的`rotbit`镜像需要的模型。需要模型主要有下面几类

### 2.1、大模型

东子哥给的百度云盘给了4个模型，当然，并不是所有的模型都是一定需要安装，我们选择适合自己的就可以了，目前我使用的就两个`全网首发_comix_v3.1_科幻plus_3.1.safetensors`和`revAnimated_v122.safetensors`。

![](img/a26db54b26631357f269304d688fee30.png)

下载地址为: `https://pan.baidu.com/s/1baNTCe9wGEN0sHYPvRabmA?pwd=wdz6`

提取码：wdz6

下载后如何安装大模型呢？还是参考 `https://t.zsxq.com/0fgBvV1AL` ,里面讲的很详细，看完还不会找我。

### 2.2、controlnet模型

使用`AutoDL`的`rotbit`镜像，`controlnet`不需要下载了 (22G的模型下载太久了，特别是百度云盘还限速。。。)，`rotbit`里面的镜像都有了。如果没有，找我，我来解决。

### 2.3、vae模型

这个模型是需要下载的，没有这个模型，出图的效果会差很多。我们需要先去百度云盘下载模型，模型的下载地址如下

下载地址: `https://pan.baidu.com/s/1baNTCe9wGEN0sHYPvRabmA?pwd=wdz6`

提取码：wdz6

![](img/a58693ea0139cf5e793ae12f902c5b16.png)

模型下载到本地后，我们需要上传到阿里云盘。 注意！！！要使用阿里云的应用程序上传，阿里云有秒传功能，我们上传可能也就几秒钟就完成了。

![](img/095929b6675b1e7a402657fe3d3d5a8b.png)

在我们上传模型到阿里云之后，就可以将`vae`模型下载我们的实例了。 注意！！！在开始这一步之前，需要将阿里云盘关联到`AutoDL`，流程请参考 `https://t.zsxq.com/0fgBvV1AL` ，关联阿里云盘到`AutoDL`后，就可以按照下面的步骤下载模型了

![](img/833761eaf0f9eca2895d987084b9670e.png)

要注意，我们需要等模型下载完成后才可以进行下一步。模型下载的进度可以下面的图的位置

![](img/5ab6170a5310ba30316d5fb4f70ac511.png)

在完成这些步骤之后，`vae`模型就下载到我们的机器上了。但是这并不是终点。。。我们还需要做最后一步的操作。具体就看看下面的图吧。

![](img/fa16ba24867c54c312fd796433f5a833.png)

选择我们需要的`vae`模型后，我们选择`cut`(剪切)操作后， 需要进入的`vae`模型目录，这一步很重要

![](img/b46dd00ba274f1078cd8e0c65cb41aae.png)

通过以上的操作，我们就把`vae`模型安装到我们的实例了。我们到`stable diffusion`页面，刷新一下就可以看到新增的`vae`模型了。

![](img/b97854e01681896a402430ea3bc53a1c.png)

## 三、常见问题

### 1、有一些小伙伴问过为什么一些教程是模型是放在「`stable-diffusion-webui/models`」，而现在的`AutoDL`的`rotbit`实例需要将模型放到「`autodl_tmp/models`」下，这里有什么区别？

默认情况下，`stable diffusion`的模型是放在「`stable-diffusion-webui/models`」文件夹下的，如下图所示，

![](img/83d75d3fd7db27319b2aec09cb6fef0e.png)

例如`controlnet`模型放在「`stable-diffusion-webui/models/controlnet`」，大模型放在「`stable-diffusion-webui/models/Stable-diffusion`」,依次类推。

`rotbit`的镜像默认情况下，模型也是放在上面的路径的。但是，因为所有模型都放在那个路径会导致磁盘空间不够用（只有50G，而且还不许扩容），所以`rotbit`镜像修改了模型存储的默认路径，改为`autodl_tmp`目录下。

以下是`rotbit`镜像每种模型需要放置的位置

1、大模型，放到`autodl_tmp`目录下即可

2、`controlnet`模型，放到`autodl-tmp/models/controlnet`

3、`vae`模型，放到`autodl-tmp/models/vae`

4、`lora`模型，放到`autodl-tmp/models/lora`

其他的依此类推

### 2、发现`autodl-tmp`下面没有任何模型?

![](img/505ed0e9f2ba5a2434a57d42fa941127.png)

模型在第一次启动时候会下载，所以刚开始是空的，启动之后会自动创建目录，下载模型。

本节分享到此结束，有问题欢迎私聊交流。

![](img/0e014ebb3c9d836c51b43c7e71a9a67a.png)

扫码加入星球

查看更多优质内容

`https://wx.zsxq.com/mweb/views/joingroup/join_group.html?group_id=15552545485212`
