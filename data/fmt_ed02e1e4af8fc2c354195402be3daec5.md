
# 教程 | 强化学习训练 Chrome 小恐龙 Dino Run：最高超过 4000 分

选自 Paperspace

**作者：Ravi Munde**

**机器之心编译**

**参与：Panda**

> 强化学习是当前人工智能领域内一个非常热门的研究方向，在游戏智能体方面的进展尤其耀眼。美国东北大学在读硕士 Ravi Munde 近日发文介绍了其构建《小恐龙快跑（Dino Run）》强化学习智能体的过程。《小恐龙快跑》是 Chrome 浏览器上的一款隐藏小游戏，当你的浏览器断开网络时，你的屏幕上就会出现这只小恐龙，此时只需点击方向键 ↑ 即可开启游戏。

DeepMind 2013 年发表的论文《使用深度强化学习玩 Atari 游戏（Playing Atari with Deep Reinforcement Learning）》为强化学习引入了一种新的深度学习模型，并展现了其仅使用原始像素作为输入就能掌握 Atari 2600 电脑游戏的不同控制策略的能力。在本教程中，我将使用 Keras 实现这篇论文。我首先会介绍强化学习的基础知识，然后深入代码以获得实践性的理解。

![](img/3306fc69de066f4b261bef9b1974196b-fs8.png)

*AI 玩《小恐龙快跑》*

我在 2018 年 3 月初开始了这个项目并得到了一些优良的结果。但是，这个只有 CPU 的系统没法学习更多功能。强大的 GPU 能显著提升其性能表现。

在我们得到一个可运行的模型之前，有很多步骤和概念需要我们理解。

步骤：

* 构建一个浏览器（JavaScript）和模型（Python）之间的双向接口
* 获取和预处理图像
* 训练模型
* 评估

源代码：`https://github.com/Paperspace/DinoRunTutorial.git`

## 开始

要这样完成训练和玩游戏，你可以在设置好环境后克隆这个 GitHub 库：

```
git clone https://github.com/Paperspace/DinoRunTutorial.git
```

然后在 Jupyter Notebook 上操作

```
Reinforcement Learning Dino Run.ipynb
```

要确保你首先运行了 `init_cache()` 来初始化文件系统结构。

## 强化学习

> 一个学习走路的孩子

对很多人来说，这可能是一个新词汇，但我们每个人都使用强化学习（RL）的概念学习过走路，而且我们的大脑现在依然这样运作。奖励系统是任何强化学习算法的基础。如果我们回到小孩走路的比喻，正面奖励可能是父母的掌声或拿到糖果；负面奖励就是没有糖果。然后，孩子在开始走路之前首先要学会站立。就人工智能而言，智能体（我们这里就是小恐龙 Dino）的主要目标是通过在环境中执行一个特定的动作序列来最大化特定的数值奖励。强化学习中最大的难题是没有监督（有标注数据）来引导智能体。它必须进行探索，靠自己学习。智能体首先会随机执行动作，然后观察每个动作所产生的奖励，再学习预测面临相似的环境状态时可能最好的动作。

![](img/f9f7b317c3543407db584893c6c4dea5-fs8.png)

*最简单纯粹的强化学习框架*

## Q 学习（Q-learning）

Q 学习是一种强化学习技术，在这种技术中，我们试图近似一个特定函数，使其能为任意环境状态序列得到动作-选择策略。Q 学习是强化学习的一种无模型的实现，其中维护着一个相对每个状态、所采取的动作和所得到的奖励的 Q 值表。一个样本 Q 值表应该能让我们了解数据的结构。在我们的案例中，状态即是游戏截屏，动作则是什么也不做和跳 [0,1]

![](img/cbf1541972d9bbafdbdb47df1192d1e6-fs8.png)

*一个样本 Q 值表*

我们使用深度神经网络，通过回归方法来解决这一问题，然后选择有最高预测 Q 值的动作。若想详细了解 Q 学习，可参看 Tambet Matiisen 的这篇出色文章：`https://ai.intel.com/demystifying-deep-reinforcement-learning/`。你也可以参看我之前的文章，了解 Q 学习的所有超参数：`https://medium.com/acing-ai/how-i-build-an-ai-to-play-dino-run-e37f37bdf153`

## 设置

首先设置训练过程所需的环境。

1. 选择虚拟机（VM）：我们需要一个完整的桌面环境，让我们可以在其中获取截屏并将其用于训练。我选择了一个 Paperspace ML-in-a-box (MLIAB) Ubuntu 镜像。MLIAB 的优势在于预装了 Anaconda 和很多其它的机器学习库。

![](img/67e3e6355727d3e87699e460430a57ee-fs8.png)

*ML-in-a-box (MLIAB)*

2. 配置和安装 Keras 并使用 GPU

我们需要安装 Keras 和 TensorFlow 的 GPU 版本。Paperspace 的虚拟机预装了这些，但如果没有安装，可以执行下列操作来安装：

```
pip install keras
pip install tensorflow
```

另外，要确保 GPU 能被识别出来。执行下列 Python 代码，你应该能看到可用的 GPU 设备：

```
from keras import backend as K
K.tensorflow_backend._get_available_gpus()
```

3. 安装依赖包

* Selenium：`pip install selenium`
* OpenCV：`pip install opencv-python`
* 下载 `Chromedriver`：`http://chromedriver.chromium.org`

## 游戏框架

你可以将你的浏览器指向 `chrome://dino` 或直接拔下网络插口来启动游戏。另一种方法是从 Chromium 的开源库提取这个游戏——如果我们想要修改游戏代码的话。

我们的模型是用 Python 写的，而游戏是用 JavaScript 构建的。要让它们之间能进行通信，我们需要一些接口工具。

Selenium 是一种常用的浏览器自动化工具，可用于向浏览器发送动作和获取当前分数等不同的游戏参数。

现在我们有可以向游戏发送动作的接口了，我们还需要一个获取游戏画面的机制。

Selenium 和 OpenCV 能分别为截屏和图像预处理提供最佳的表现，能实现 6-7 FPS 的帧率。

我们只需要 4 FPS 的帧率，所以足够了。

## 游戏模块

我们使用这个模块实现了 Python 和 JavaScript 之间的接口。下面的代码片段应该能让你一窥该模块所执行的功能。

```
class Game:

def __init__(self):

self._driver = webdriver.Chrome(executable_path = chrome_driver_path)

self._driver.set_window_position(x=-10,y=0)

self._driver.get(game_url)

def restart(self):

self._driver.execute_script("Runner.instance_.restart()")

def press_up(self):

self._driver.find_element_by_tag_name("body").send_keys(Keys.ARROW_UP)

def get_score(self):

score_array = self._driver.execute_script("return Runner.instance_.distanceMeter.digits")

score = ''.join(score_array).

return int(score)
```

## 智能体模块

我们使用智能体模块封装了所有接口。我们使用这一模块控制小恐龙 Dino 以及获取智能体在环境中的状态。

```
class DinoAgent:

def __init__(self,game): #takes game as input for taking actions

self._game = game; 

self.jump(); #to start the game, we need to jump once

def is_crashed(self):

return self._game.get_crashed()

def jump(self):

self._game.press_up()
```

## 游戏状态模块

为了将动作发送给模块并得到由该动作导致的环境转换的结果状态，我们使用了游戏-状态模块。通过接收&执行动作、确定奖励和返回经历元组，其简化了这一过程。

```
class Game_sate:

def __init__(self,agent,game):

self._agent = agent

self._game = game

def get_state(self,actions):

score = self._game.get_score() 

reward = 0.1 #survival reward

is_over = False #game over

if actions[1] == 1: #else do nothing

self._agent.jump()

image = grab_screen(self._game._driver) 

if self._agent.is_crashed():

self._game.restart()

reward = -1

is_over = True

return image, reward, is_over #return the Experience tuple 
```

## 图像处理流程

### 获取图像

获取游戏画面的方法有很多，比如使用 PIL 和 MSS Python 库来获取整个屏幕的截屏然后裁剪相关区域。但是，其最大的缺点是对屏幕分辨率和窗口位置的敏感度。幸运的是，该游戏使用了 HTML Canvas。我们可以使用 JavaScript 轻松获取 base64 格式的图像。我们使用 Selenium 运行这个脚本。

```
//javascript code to get the image data from canvas
var canvas = document.getElementsByClassName('runner-canvas')[0];
var img_data = canvas.toDataURL()
return img_data
```

![](img/37e86fa0fec1bbdcaf1ebe291f04ba99-fs8.png)

*从 Canvas 提取出的图像*

```
def grab_screen(_driver = None):

image_b64 = _driver.execute_script(getbase64Script)

screen = np.array(Image.open(BytesIO(base64.b64decode(image_b64))))

image = process_img(screen)#processing image as required

return image
```

### 处理图像

获取得到的原始图像的分辨率大约是 600×150，有 3 个通道（RGB）。我们打算使用 4 个连续截屏作为该模型的单个输入。这会使得我们的单个输入的维度高达 600×150×3×4。这样的计算成本很高，而且并非所有特征都对玩游戏有用。所以我们使用 OpenCV 库对图像进行尺寸调整、裁剪和处理操作。处理后得到的最终输入图像尺寸只有 80×80 像素，且只有单个通道（灰度）。

```
def process_img(image):

image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

image = image[:300, :500]

return image
```

![](img/a107df16f56373b2d2483029a3b2d0fa-fs8.png)

*图像处理*

## 模型架构

所以我们得到了输入，并且能使用该模型的输出来玩游戏了，现在我们来看看模型的架构。

我们使用了按顺序连接的三个卷积层，之后再将它们展平成密集层和输出层。这个只使用 CPU 的模型不包含池化层，因为我去除了很多特征，添加池化层会导致已经很稀疏的特征出现显著损失。但借助 GPU，我们可以容纳更多特征，而无需降低帧率。

最大池化层能显著提升密集特征集的处理结果。

![](img/bb50d4a997c6e62848f0f8cefa2dccf9-fs8.png)

*模型架构*

我们的输出层由两个神经元组成，每一个都表示每个动作的最大预测奖励。然后我们选择有最大奖励（Q 值）的动作。

```
def buildmodel():

print("Now we build the model")

model = Sequential()

model.add(Conv2D(32, (8, 8), padding='same',strides=(4, 4),input_shape=(img_cols,img_rows,img_channels)))  #80*80*4

model.add(MaxPooling2D(pool_size=(2,2)))

model.add(Activation('relu'))

model.add(Conv2D(64, (4, 4),strides=(2, 2),  padding='same'))

model.add(MaxPooling2D(pool_size=(2,2)))

model.add(Activation('relu'))

model.add(Conv2D(64, (3, 3),strides=(1, 1),  padding='same'))

model.add(MaxPooling2D(pool_size=(2,2)))

model.add(Activation('relu'))

model.add(Flatten())

model.add(Dense(512))

model.add(Activation('relu'))

model.add(Dense(ACTIONS))

adam = Adam(lr=LEARNING_RATE)

model.compile(loss='mse',optimizer=adam)

print("We finish building the model")

return model
```

## 训练

训练阶段发生的事情有这些：

* 从无动作开始，得到初始状态（`s_t`）
* 观察 `OBSERVATION` 步数的玩游戏过程
* 预测和执行一个动作
* 将经历存储在重放记忆（Replay Memory）中
* 从重放记忆随机选取一批，然后在其上训练模型
* 如果游戏结束，则重新开始

这部分的代码有点长，但理解起来相当简单。
