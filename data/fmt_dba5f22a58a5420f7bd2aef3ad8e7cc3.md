
Training text-to-video generation systems requires a large amount of videos with corresponding text captions. We apply the re-captioning technique introduced in DALL·E 330 to videos. We first train a highly descriptive captioner model and then use it to produce text captions for all videos in our training set. We find that training on highly descriptive video captions improves text fidelity as well as the overall quality of videos.

训练文本到视频生成系统需要大量带有相应文字标题的视频。我们将在DALL·E 3中引入的重新标注技术应用到视频上。我们首先训练一个高度描述性的标注模型，然后使用它为我们训练集中的所有视频生成文字标题。我们发现，在高度描述性的视频标题上进行训练可以提高文本的准确性以及视频的整体质量。

Similar to DALL·E 3, we also leverage GPT to turn short user prompts into longer detailed captions that are sent to the video model. This enables Sora to generate high quality videos that accurately follow user prompts.

类似于DALL·E 3，我们也利用GPT将用户的简短提示转换成更长的详细说明，然后发送给视频模型。这使得Sora能够生成高质量的视频，准确地遵循用户的提示。

![](img/fabff450ecfbc8be7e7ac85e8a0ac2fd.png)

## Prompting with images and videos 使用图片和视频进行提示

All of the results above and in our landing page show text-to-video samples. But Sora can also be prompted with other inputs, such as pre-existing images or video. This capability enables Sora to perform a wide range of image and video editing tasks—creating perfectly looping video, animating static images, extending videos forwards or backwards in time, etc.

上述结果以及我们的登录页面展示了文本到视频的样本。但是Sora也可以通过其他输入进行提示，例如预先存在的图片或视频。这项能力使得Sora能够执行广泛的图像和视频编辑任务——创建完美循环的视频，为静态图像添加动画，向前或向后延长视频的时间等。

### Animating DALL·E images 制作DALL·E图像动画

Sora is capable of generating videos provided an image and prompt as input. Below we show example videos generated based on DALL·E 231 and DALL·E 330 images.

Sora能够根据输入的图片和提示生成视频。下面我们展示了基于DALL·E 2 31 和DALL·E 3 30 图片生成的示例视频。

![](img/12a290b273b813b9f9f2303035c10a56.png)

![](img/1682fd76eaaba062d950a8ede5b7f8ac.png)

### Extending generated videos 延长生成的视频

Sora is also capable of extending videos, either forward or backward in time. Below are four videos that were all extended backward in time starting from a segment of a generated video. As a result, each of the four videos starts different from the others, yet all four videos lead to the same ending.

Sora也能够将视频向前或向后延长时间。下面是四个视频，它们都是从生成的视频片段开始向后延长的。因此，这四个视频的开头各不相同，但最终都会达到相同的结局。

![](img/1809d90f710fefd4d9d28ed7fd358841.png)

We can use this method to extend a video both forward and backward to produce a seamless infinite loop.

我们可以使用这种方法将视频向前和向后扩展，以制作出无缝的无限循环。

### Video-to-video editing 视频到视频编辑

Diffusion models have enabled a plethora of methods for editing images and videos from text prompts. Below we apply one of these methods, SDEdit,32 to Sora. This technique enables Sora to transform the styles and environments of input videos zero-shot.

扩散模型使得从文本提示编辑图像和视频的方法层出不穷。下面我们将其中一种方法，SDEdit，应用于Sora。这项技术使得Sora能够零次学习地转换输入视频的风格和环境。

![](img/792ac6cff798578bc1a3c075af5cfae0.png)

### Connecting videos 连接视频

We can also use Sora to gradually interpolate between two input videos, creating seamless transitions between videos with entirely different subjects and scene compositions. In the examples below, the videos in the center interpolate between the corresponding videos on the left and right.

我们还可以使用Sora在两个输入视频之间逐渐插值，创建在完全不同主题和场景构成的视频之间的无缝过渡。在下面的例子中，中间的视频在左右两边对应视频之间进行插值。

![](img/ee63ce0c34d381d37e350f6736d3baed.png)

![](img/18b9e7ce494982f9b0e79ea66cad03e3.png)

## Image generation capabilities 图像生成能力

Sora is also capable of generating images. We do this by arranging patches of Gaussian noise in a spatial grid with a temporal extent of one frame. The model can generate images of variable sizes—up to 2048x2048 resolution.

Sora也能够生成图像。我们通过在具有一个帧时间范围的空间网格中排列高斯噪声块来实现这一点。该模型可以生成不同大小的图像——分辨率最高可达2048x2048。

![](img/73454edc26dce53c64e5d9b20918d4a6.png)

Close-up portrait shot of a woman in autumn, extreme detail, shallow depth of field

秋天里一位女性的特写肖像，极致细节，浅景深

![](img/b0be3e57cbc2cc3f6eb74b821bd4ffd8.png)

Vibrant coral reef teeming with colorful fish and sea creatures

充满活力的珊瑚礁，挤满了五彩缤纷的鱼类和海洋生物

Digital art of a young tiger under an apple tree in a matte painting style with gorgeous details

数字艺术：一只幼年老虎在苹果树下，采用哑光绘画风格，细节华丽

![](img/a7c47053b33b204f14169f146b5e7772.png)

A snowy mountain village with cozy cabins and a northern lights display, high detail and photorealistic dslr, 50mm f/1.2

一个雪山村庄，有着舒适的小木屋和北极光展示，高清晰度和逼真的数码单反相机，50mm f/1.2镜头拍摄。

## Emerging simulation capabilities 涌现的模拟能力

We find that video models exhibit a number of interesting emergent capabilities when trained at scale. These capabilities enable Sora to simulate some aspects of people, animals and environments from the physical world. These properties emerge without any explicit inductive biases for 3D, objects, etc.—they are purely phenomena of scale.

3D consistency. Sora can generate videos with dynamic camera motion. As the camera shifts and rotates, people and scene elements move consistently through three-dimensional space.

3D一致性。Sora能够生成具有动态相机运动的视频。随着相机的移动和旋转，人物和场景元素在三维空间中保持一致地移动。

**Long-range coherence and object permanence.** A significant challenge for video generation systems has been maintaining temporal consistency when sampling long videos. We find that Sora is often, though not always, able to effectively model both short- and long-range dependencies. For example, our model can persist people, animals and objects even when they are occluded or leave the frame. Likewise, it can generate multiple shots of the same character in a single sample, maintaining their appearance throughout the video.

长距离一致性和物体恒存性。对于视频生成系统来说，一个重大挑战是在采样长视频时保持时间上的连贯性。我们发现，尽管不总是如此，Sora通常能够有效地建模短距离和长距离依赖关系。例如，我们的模型即使在人、动物和物体被遮挡或离开画面时，也能持续保持它们的存在。同样，它能在单个样本中生成同一角色的多个镜头，并在整个视频中保持其外观。

Interacting with the world. Sora can sometimes simulate actions that affect the state of the world in simple ways. For example, a painter can leave new strokes along a canvas that persist over time, or a man can eat a burger and leave bite marks.

与世界互动。Sora有时可以模拟一些简单的动作来影响世界的状态。例如，画家可以在画布上留下随时间持续存在的新笔触，或者一个人可以吃一个汉堡并留下咬痕。

Simulating digital worlds. Sora is also able to simulate artificial processes–one example is video games. Sora can simultaneously control the player in Minecraft with a basic policy while also rendering the world and its dynamics in high fidelity. These capabilities can be elicited zero-shot by prompting Sora with captions mentioning “Minecraft.”

模拟数字世界。Sora也能够模拟人工过程——一个例子是视频游戏。Sora可以在同时控制《我的世界》中的玩家采用基本策略的同时，还能以高保真度渲染世界及其动态。通过用提到“我的世界”的字幕提示Sora，可以零次尝试地引发这些能力。

These capabilities suggest that continued scaling of video models is a promising path towards the development of highly-capable simulators of the physical and digital world, and the objects, animals and people that live within them.

这些能力表明，持续扩展视频模型是朝着开发高度能够模拟物理和数字世界及其内部的物体、动物和人类的有希望的道路。

![](img/2991079c5a95ae6a2b1c7d534325df03.png)

## Discussion 讨论

Sora currently exhibits numerous limitations as a simulator. For example, it does not accurately model the physics of many basic interactions, like glass shattering. Other interactions, like eating food, do not always yield correct changes in object state. We enumerate other common failure modes of the model—such as incoherencies that develop in long duration samples or spontaneous appearances of objects—in our landing page.

Sora作为一个模拟器目前展现出许多限制。例如，它并没有准确地模拟许多基本互动的物理效应，比如玻璃破碎。其他互动，比如吃食物，不总是产生正确的物体状态变化。我们在我们的登录页面列举了模型的其他常见故障模式——比如在长时间样本中发展的不连贯性或物体的自发出现。

We believe the capabilities Sora has today demonstrate that continued scaling of video models is a promising path towards the development of capable simulators of the physical and digital world, and the objects, animals and people that live within them.

我们相信，Sora目前的能力表明，持续扩展视频模型是朝着开发能够模拟物理和数字世界及其内部的物体、动物和人类的有能力的模拟器的有希望的道路。

## 看看SORA生成视频的效果究竟如何

提示词: 一位时髦的女士穿行在东京的街头，街道充满了温暖的霓虹灯光和动感的城市标志。她穿着一件黑色皮夹克，一条长红裙和黑色靴子，手拿一个黑色手提包。她戴着太阳镜和红色口红。她走路既自信又随意。街道潮湿且能反射，创造出彩色灯光的镜面效果。许多行人来来往往。
