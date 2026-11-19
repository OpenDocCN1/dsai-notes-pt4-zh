
# 【n8n教程】一键生成爆款视频——动物跳水

> 来源：[https://xuqiwei1986.feishu.cn/docx/ZdGZducdkoklBix1NpIcd8apnEe](https://xuqiwei1986.feishu.cn/docx/ZdGZducdkoklBix1NpIcd8apnEe)

hello大家好，我是李香君，AI传术师，youtube深海圈教练，喜欢捣鼓工作流。周日的时候我在北京组了自己第一个线下的组局。讲的是一个一键生成小动物跳水的工作流，回家的时候刷了一下抖音，发现这个内容目前在抖音还是非常火爆的。独乐乐不如众乐乐。于是乎，我想着把这个工作流写成一个教程帖子分享给大家。这个是爆款视频：

![](img/f810771728d7a9799dfed6ba0fe4459b.png)

![](img/78423082e2f8e6f1e60a9224391b0b28.png)

这是抖音上的爆款视频

![](img/af30ad100b68b4ada22758339f9df3e7.png)

[ocEjWA36xNgfCEIociFfABg9dOLJbyQK9ODvAn.MP4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FZUHba6OzoHQ9txbfUIc4z4NnHh)

这个是我一键生成的视频（一键生成视频+配音，你只需要填上你想让跳水的动物即可）：

![](img/00b1d3d4fb9f1db2a4893f30762ad5e0.png)

[eehIFzSvpeV1N6ONRiwRD_output_video.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KDNnbR7QdonmHox4ghwcUW75nKe)

这个是整个工作流的全貌：

![](img/b962075c3e49c56a1e6508f2c266c77d.png)

是不是还挺有意思的？如果你感兴趣的话，废话不多说，咱们直接开始教程（如果你不想自己配置，文末也直接附赠了工作流，所有需要复制的内容我都贴到文末了）

## 一、工作流拆解

这个工作流分成了三层：

第一层是生图层，主要包含的节点有：`google sheet`节点（主要更新我们想跳水的动物）、提示词生成节点、还有生图节点

第二层是生视频层，主要包含的节点有：建立生图任务的`http`请求、延迟、视频下载节点、延迟节点。由于生成视频时间比较长，所以`fal api`这里的生成是异步的，可以理解成去餐厅吃饭服务员帮你点单（建立生图任务的`http`请求），等待上菜（延迟），服务员帮你端菜（延迟），你的菜还没好，服务员要时常跑厨房帮你去看菜（延迟）

第三层是配音层，主要给我们生成视频进行配音将视频的完成度再拉升一个档次，主要包含的节点有：从视频下载节点里面拿出来链接的`extract from file`节点，建立配音任务的`http`请求、延迟、饮品下载节点

最后还有一个视频更新节点，将我们生成好的视频链接更新到`google sheet`表格里面

### 如何导入已经配置好的工作流：

下载这个文档[跳水工作流配套内容](https://irxd56ii8t.feishu.cn/docx/TR51daiRVodD8YxJhE4cPhf7nVh)中的 JSON 文件，打开`n8n`在右上角选择创建新的工作流

![](img/ee0aa28fad466244cadbe106da63ed88.png)

然后在右上角的三个点里边选择从文件中导入。

![](img/75c63884630195f9d54818cfc78713bf.png)

然后你就发现你的工作流导入进来了

![](img/971f949cda11cb28927aa5c1dc0dd975.png)

## 二、生图层拆解

### 节点一

首先所有的节点都要一个触发器，这里我们为了方便选择的是点击触发器，把他拉到画布里面就不用管它了

![](img/adc8f4effc106e67a6e18ae3669c800d.png)

### 节点二：拿数据

第二个节`[从0开始手把手教你用n8n打造自动化生图工作流](https://irxd56ii8t.feishu.cn/docx/MOAXdcDJioc9kGxqm5qcMb1qntC?from=from_copylink)`是一个`google sheet`节点，选择`get row(s) in sheet`这个功能，首先没有在这里配置过认证的朋友要在这里配置一个`Google Sheets account`，具体怎么配置可以参考我以前的帖子配置`google drive account`的流程，流程是一模一样的，只不过认证的api换成`google sheet`，请大家移步阅读： （目录里面找到第二步：添加`google drive`节点）

![](img/9dbdcdd3df35bbfdb8090850a0cdac13.png)

![](img/28b15b4322ae89eb8c77d2a20c44b807.png)

然后去`https://docs.google.com/`，登录你刚才在`n8n`登录认证过的谷歌账号，开一张新表，表头跟我一致即可，并填写第一二列的数据（我们主要抓取第一列的动物信息，然后在第二列更新好配套的完成状态，没有开始跑之前一律把下拉菜单里面的状态更新成未完成，下拉菜单右键单元格就能找到），第三列留空，图片中的链接是我工作流跑完的结果，你们不用手动更新！没有跑过工作流的内容，初始设置参考第五行水豚的配置

![](img/48d0a3fb80a2735d5504b1e08390cacb.png)

配置好了这个表格之后，我们回到`get row(s) in sheet`这个节点里面，你们点开红箭头所指向的两个下拉菜单，应该就能看到刚才配置好的表格和那个`sheet`选中即可。在红框处跟我的配置保持一致（这一块主要是用于判断是否需要跑该行数据），并在`options`处，点击底下的`add option`，选择`Return only First Matching Row`，并点绿。当你完成所有操作之后这个节点就配置好了

![](img/06bbff3c4e234ee51d4b7efe0eed4978.png)

### 节点三：生成图片提示词

配置生成生图提示词的大模型节点，这里面比较简单，进来之后选择`define delow`，然后把我的提示词粘贴进去即可

![](img/0ccf5c05f6a1c8e81f059d4678ba2f2c.png)

```
你现在是一个专业的AI绘画提示词（Prompt）生成器。请你根据我提供的固定场景描述，生成1条独立的、可用于AI绘画的提示词。

固定场景要求如下：

视角与构图： 一个从侧面拍摄的中景镜头，构图稳定。焦点清晰地落在主体 {{ $json[&#x27;动物&#x27;] }} 身上。

地点与核心元素： 场景为一个光线明亮的室内奥运会跳水场馆。主体 {{ $json[&#x27;动物&#x27;] }} 正从右向左走在一块浅蓝色的跳水板上。

背景细节： 背景是一面纯净的蓝色墙壁，墙壁左上方印有清晰的白色奥运五环标志。在主体动物的后方，可以看到一名坐在高脚椅上、身穿红色上衣的模糊工作人员，以及更远处的模糊观众席。在画面的右侧，可以看到跳水台的银色金属扶手/楼梯结构。

你的任务：
在上述固定场景中，将核心主体设定为：一只 {{ $json[&#x27;动物&#x27;] }}。

输出要求：

生成1条独立的提示词。

最终输出的提示词要完整、流畅地融合所有场景描述。

注意！！！只输出最终生成的提示词内容，不要包含任何其他无关文字。

生成的动物要面冲着泳池
```

并在`[从0开始手把手教你用n8n打造自动化生图工作流](https://irxd56ii8t.feishu.cn/docx/MOAXdcDJioc9kGxqm5qcMb1qntC)`底下设置模型，如果你从来没用过`n8n`，就在第一行添加一个认证账号，很简单去`openrouter`官网新建一个`api key`登上就行，如果不会，还是参考上次我的帖子即可（传送门： ），然后选择模型，这我选择的是`gemini 2.5 flash`，它的特点是便宜，可靠，效果还算够用

![](img/ff23a208f7b89514b1b733a34adf706f.png)

配置完了上面的东西，就可以进行测试，点击`exevute step`，让他跑起来，旁边生成提示词。tips：结果你如果满意的话，就可以点右上角的大头针固定，这样，再测试后面节点的时候，这个节点会持续输出你固定的这一次的结果，比较省`token`（后面所有的节点都可以用这个方法固定，后面我就不做赘述了，但是记得全部配置完要用于生产的是时候要把大头针拔掉）

![](img/38de236d086bed392277e5b8ca186e5e.png)

### 节点四：生图

这里我用的是书虫老板的`api`（书虫的`api`聚合站`url`：`https://api.apicore.ai/register?aff=cAdU`），用的是`flux kontext`模型。这里有一个事情要注意，书虫老板的这个`api`生成好的图片地址仅会保存几分钟，所以隔的时间太长的情况下，链接会失效，所以这块要灵活的判断自己要不要用大头针固定结果

下面我们开始配置这个节点，会看`api`文档的看这里（文档`url`：`https://doc.apicore.ai/api-303532198`）

不会看文档的，咱们一步一步手把手来：

请求方式选择`post`，请求链接填`https://api.apicore.ai/v1/images/generations`，身份认证选择`generic credential type`，下面选择头部认证 `header auth`，然后在红框里面新建一个认证

![](img/162de8ff0c26404dfa690a67603eac84.png)

新建的认证这么写，`name`填的一定是`Authorization`这个是字段的头，是固定的一定不要瞎填！！！`value`这里填写`Bearer` 你的`api key`，注意到我的标红了吗？`Bearer`和`api key`中间是有一个空格的一定要注意，要不然百分之一个亿跑不通

![](img/3c20e513939130ce81c95a3c75ae883d.png)

下面按照我的配置来，把这个`text`的小标志直接拉到`prompt`的两个冒号中间，显示绿色的这个变量名称就证明你搞对了

![](img/fbdc863162bc96d12bbf09cd09d6460e.png)

这个`json`的内容如下：

```
{
    "model": "flux-kontext-pro",
    "prompt": "{{ $json.text }}",
    "seed": 0,
    "aspect_ratio": "16:9",
    "output_format": "jpeg",
    "webhook_url": "null",
    "webhook_secret": "null",
    "prompt_upsampling": false,
    "safety_tolerance": 2
}
```

这一步搞定点测试按钮，你就会稍微等一会你就能看到旁边的这个链接，证明你跑通了，然后把链接粘出来打开，你就能看到生成的图片

![](img/6b04f9aec3d13aa6b45c404d8f5e4930.png)

## 三、生视频层拆解

### 节点一：提交生成视频的请求

这次我们用到的是海螺2模型，这个模型以动作幅度大而著称，本次用到的是海螺2的图生视频模型，用到的是`fal`平台的`api`，为什么会选择`fal`？目前只有海外的`api`聚合平台有这个`api`。

还是老样子，会看文档的朋友，文档链接在这里：`https://fal.ai/models/fal-ai/minimax/hailuo-02/standard/image-to-video/api`

不会看文档的咱们还是一步一步来，请求方式选`post`，`url`填这个`https://queue.fal.run/fal-ai/minimax/hailuo-02/standard/image-to-video`

身份认证选择`generic credential type`，下面选择头部认证 `header auth`，然后在新建一个认证

![](img/6c3220276671649510932e4992cd0ba9.png)

`fal`的认证账户跟前面略有不同的点是，这里不是`Bearer`+空格+`api key`，这里变成了`Key`+空格+`api key`，其他都一样，配置的时候要注意！！

![](img/1005d4e7c2a51a7f6bac896de41e5791.png)

然后，按照我下图配置，记得把红框的部分拉到`image_url`底下的`value`里

![](img/34452771c29a31799b5f47f9da789167.png)

这个是我们要用到的提示词：

```
The animal leaped from the diving platform, performed a forward three-and-a-half somersault pike in mid-air, executed an elegant dive, and finally plunged into the pool.
```

### 节点二：延迟

因为视频生成的速度肯定是远远慢于生图，且官方采用了异步生成的方式，所以这里我们要加一个延迟，来帮助我们等待结果生成完毕，找到`wait`节点添加并串好，这个节点设置比较简单，按照我图里面设置好就行了，这里我选择延迟五分钟比较稳，你们也可以适当调快一点

![](img/9a503acd73dc64661befbb0d1db90d5a.png)

### 节点三：抓取视频

按照我的这个图中设置，并选取`fal`的认证，记得把红框的那个标签拉到`url`的文本框里面

![](img/02369b8e809e39aa8532ad24ecced65b.png)

设置好了之后，这里还有一个重要的设置，点击上面的第二个标签`settings`，在红框标注位置选择带着错误输出，这样我们的节点就有成功和不成功的两种输出状态，帮助我们判断，成功的状态我们后面要串到下一层的输入里面去，失败的状态我们可以加个延迟（这里我是延迟10秒钟，配置方法跟上面的延迟一样），然后再串回这个抓取视频的节点，这样如果一直没有抓取成功的话，这个抓取节点，每隔十秒钟就会重新进行一次下载任务，这样就能帮我们实现一个轮训的效果。串联效果可以看下图二

![](img/ba41359e2a5ecb573f637a9f99c8a671.png)

![](img/507c11629e58532b3b64e740d7176a92.png)

## 四、配音层拆解

### 节点一：`Extract from File`

不知道为什么我上一个节点`get`下来的东西，是一个`json`文件（其他圈友`get`下来的都是`url`），如果你跟我一样那么就加这个节点，按照我的配置去配置，如果直接`get`下来的是链接就不用这个。这个节点的原理就是从这个`json`文件里面把数都取出来

![](img/701b74aa049f4b075cf4c54f6df2c966.png)

### 节点二：提交配音请求

逻辑跟刚才提交视频的请求一样，这里大家就按照图配置吧，提示词我就直接用的是生成视频的提示词，然后`url`记得拉到对应位置

![](img/f4f6454248ec4f46ed8fd5fc40c372d1.png)

![](img/667f0002f745c5a781dc6a6370e04c0a.png)

### 节点三：延迟

这里跟上面的延迟配置方法一样，但是时长是1分钟

![](img/c55d33ae96d70463155c1681b3a0d232.png)

### 节点四：下载视频节点

配置跟上面的抓取视频一样，按照下图配置即可，抓取失败的输出记得串回延迟节点，达到轮询的效果，最后看到下图四的链接，就代表生成成功了

![](img/b23187492611240bc49125549e4f08dc.png)

![](img/a42e8b163011f5afd427a8118d590140.png)

![](img/abe8ac979b5b0e338ae655479682fbee.png)

![](img/ca4522a82550fc15a99da34bdc7f8029.png)

## 五、保存回表格

这里我们来到了最后一步，将输出的结果保存回表格，这里需要一个`google sheet`的`Update row in sheet`节点。把最开始的`google sheet`节点里面的行号信息拉过来方便工作流找到对应的行号，然后按照我的图进行配置

![](img/4172bb1c3fbb14ca95c03fa2877c34af.png)

![](img/0eabd93c3a16c1854cfe092d4dd41150.png)

## 六、运行&总结

每次我们想到新的动物素材的时候就在表格里面更新这前两列

![](img/e33d151dd05ae8102aa36a09f55d10d2.png)

然后点击这个按钮，工作流就会跑一次，并最终将结果更新在第三列

![](img/8bcdb78d947dcf5434a8ca7c05fc0533.png)

是不是还挺简单的？赶紧实操起来吧

下面就是这个工作流的`json`文件和工作流里面所有需要复制的内容⬇️⬇️⬇️⬇️⬇️

[跳水工作流配套内容](https://irxd56ii8t.feishu.cn/docx/TR51daiRVodD8YxJhE4cPhf7nVh?from=from_copylink)

祝大家玩的开心！！
