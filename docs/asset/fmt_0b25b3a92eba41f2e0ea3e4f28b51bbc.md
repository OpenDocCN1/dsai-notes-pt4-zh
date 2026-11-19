
# 视频指令维度

> 来源：[https://v11enp9ok1h.feishu.cn/docx/UPh5db4OFoi7fQxT7wecIOAmn5c](https://v11enp9ok1h.feishu.cn/docx/UPh5db4OFoi7fQxT7wecIOAmn5c)

将“视频生成质量”分解为多个定义明确的维度，从不同内容信息，列出一些提示维度采样情况

## `appearance_style`

Snow rocky mountains peaks canyon. snow blanketed rocky mountains surround and shadow deep canyons. the canyons twist and bend through the high elevated mountain peaks, oil painting

A beautiful coastal beach in spring, waves lapping on sand, in cyberpunk style

A couple in formal evening wear going home get caught in a heavy downpour with umbrellas, surrealism style

A boat sailing leisurely along the Seine River with the Eiffel Tower in background, in cyberpunk style

A beautiful coastal beach in spring, waves lapping on sand, surrealism style

### 解释

描述了生成视频中场景的整体风格和美学。这些例子展示了不同风格的场景生成，如油画风格、赛博朋克风格和超现实主义风格。每个示例都准确地捕捉到了所描述的风格，呈现出特定的视觉效果。

## `color`

a yellow car

![](img/fada936c513291e58b9ac8ae4a1672d2.png)

[`cogvideo_00027.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Ijunba6f8oDFX6xEDHncsVYxnqh)

a yellow umbrella

![](img/7d51174d306adf23baa658305151386a.png)

[`cogvideo_00028.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Faq0bGfWmoV7oVxszpCcjyonnoe)

a purple suitcase

[`cogvideo_00029.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/NzN1bRJWMosndYx1FiCcJqTJnIc)

a pink chair

![](img/3c5e562ac57e139e39fa0be0c1ab5c67.png)

[`cogvideo_00030.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WaSzb2mvvoqK8WxvZdGcfjQknyJ)

a yellow clock

[`cogvideo_00031.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IKzfbq0KqoDsGUxJTnscvddYnNc)

a white vase

[`cogvideo_00032.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YprjbGnPdoRoulxoLJvcY98xnlb)

### 解释

这一维度主要关注生成视频中的颜色表现。例如，黄色的车、黄色的雨伞、紫色的行李箱等颜色物体生成情况。模型能够很好地理解并生成所提示的颜色对象。

## `human_action`

A person is riding a bike

[`cogvideo_00052.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DGRzbu4nCorMsNxCnr4cSOcRnXd)

A person is roller skating

[`cogvideo_00054.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VOwVbpksjoYyAgxHhJWchn0bnzb)

A person is petting animal

[`cogvideo_00058.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/D3HwbAnB2oNfXTxHKifcggqdnpe)

A person is sweeping floor

[`cogvideo_00063.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XnXNb4vUFon9vix5lyac8Sr9nVe)

A person is skateboarding

[`cogvideo_00064.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GJHDbLsgloS5ZjxdMfecX7zEndb)

A person is skydiving

[`cogvideo_00069.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/H4ZUbsVXAotXZcxFIszcljm8n5e)

A person is shooting goal (soccer)

[`cogvideo_00070.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RPZzbQZyMooyDlx2qe6cXBEPnZg)

### 解释

该维度关注视频中人类动作的生成效果。包括骑自行车、滑冰、宠物动物等动作。这些例子展示了模型在生成人类活动场景时的表现，有时会因为剧烈运动而导致帧之间的连贯性出现问题。

## `multiple_objects`

a giraffe and a bird

![](img/ddbeab9c44330dda98ce20645ee7e884.png)

[`cogvideo_00077.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EWTnbfR0ioATOExgNFTcgMF6nYg)

### 解释

多个对象生成，`2b`无法完成，`5b`模型多物体会有几率奇怪的融合，该维度评估生成视频中多个对象的表现。例如，长颈鹿与鸟共处的场景。虽然模型可以生成多个对象，但偶尔会出现对象之间奇怪的融合，导致不自然的结果。

## `object_class`

a chair

[`cogvideo_00078.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/U9i2b1oZ2oth4txWUazczAUgnge)

a remote

[`cogvideo_00080.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EaR5bWlhxokFHTxqBtFca0UCnrh)

a keyboard

[`cogvideo_00081.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YQECb6WlSo7JSXx3oWicLjsznHd)

a cell phone

[`cogvideo_00082.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DTUSbalJookLw6xslKmc7n01n7c)

### 解释

评估生成视频中不同类别对象的表现，如椅子、遥控器、键盘等。模型通常能够准确生成这些对象，但有时会出现意外的附加信息，例如人物持有物体的场景。

## `overall_consistency`

Snow rocky mountains peaks canyon. snow blanketed rocky mountains surround and shadow deep canyons. the canyons twist and bend through the high elevated mountain peaks.

[`cogvideo_00083.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OCTMbL7byod9g3xNyUHceOXEnoe)

Splash of turquoise water in extreme slow motion, alpha channel included.

[`cogvideo_00084.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PeQwbQfQlooXOwxFNmucx8qHnFf)

An astronaut feeding ducks on a sunny afternoon, reflection from the water.

![](img/262f183a78bcf37c69edc5f8c26ab5d1.png)

[`cogvideo_00086.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EgTqbES63o2WTIxRFFXccWlyntb)

### 解释

整体一致性关注场景的整体连贯性。生成的场景能很好地遵循提示词的描述，如雪山峡谷和慢动作水花场景，展现出准确的内容一致性。

## `scene`

kitchen

![](img/25e98c4d3de0f9c911ad0789768def63.png)

[`cogvideo_00088.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JpWQbUH5coRv0ExAicDcxvJXnKE)

lighthouse

![](img/9a5a71d4de98b45d1ed7a54ec99c2d42.png)

[`cogvideo_00090.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KjQObIr1HoHCN5xI4BscWgEgnug)

### 解释

此维度检查特定场景的生成情况，如厨房或灯塔等。这些场景通常能够准确地捕捉到空旷的环境和符合提示词描述的内容。

## `spatial_relationship`

a horse on the right of a sheep, front view

a cow on the right of an elephant, front view

a bear on the right of a zebra, front view

a giraffe on the right of a bird, front view

a bottle on the left of a wine glass, front view

a banana on the bottom of an apple, front view

an apple on the bottom of a sandwich, front view

a sandwich on the bottom of a orange, front view

### 解释

空间关系描述了对象在场景中的空间排列。示例包括动物和物品之间的空间关系。然而，模型在处理这些空间提示词时可能会出现错误，如对象位置错位或违反物理规则。

## `subject_consistency`

a person drinking coffee in a cafe

![](img/364a898b09d54b07678608ee4b6e587e.png)

[`cogvideo_00099.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EREkbx1HBoloP3xRe4icfspInUf)

a bicycle leaning against a tree

[`cogvideo_00100.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MV3lbj561oeVQjxvfYWcicYpnwg)

a car stuck in traffic during rush hour

![](img/6a3be98082681ac41d7338e66423d64b.png)

[`cogvideo_00101.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/E32Zb3Um8oEMroxryRFcWyA0ntc)

a cow bending down to drink water from a river

![](img/2abcab7a0fb8aa5dac3957bb6256406b.png)

[`cogvideo_00106.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Jj0VbGTaSov6eZxBt6bcttUFn6d)

### 解释

主题一致性检查生成视频中不同主题的表现。例如，人在咖啡馆喝咖啡或自行车靠在树上的场景。模型通常能准确生成这些场景，但在涉及交通工具时，有时会出现帧间不连贯的情况。

## `temporal_flickering`

In a still frame, within the desolate desert, an oasis unfolded, characterized by the stoic presence of palm trees and a motionless, glassy pool of water

[`cogvideo_00111.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XpeUbRD44o8HaVxeJtmcgQeznwd)

In a still frame, in the picturesque square, a Gothic-style lamppost adorned with intricate stone carvings added a touch of medieval charm to the setting

![](img/ab8c479dd1d0a1fd093736ce8fa8eb85.png)

[`cogvideo_00114.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JIARb6iUNo4qqhxqheKcRTYSnZf)

In a still frame, amidst the hushed ambiance of the traditional tea room, a meticulously arranged tea set awaited, with porcelain cups, a bamboo whiskd, a weeping willow tree draped its branches gracefully over the water's surface, creating a serene tableau of reflection and calm

![](img/3954557ad4c6b52a850f24adf0176af4.png)
