
# Task3：星火 API & Gradio 学习指南

> 来源：https://datawhaler.feishu.cn/docx/SChhdjNJZoh3wdxC9W2cYcy4n1b

> 如何从一段代码行对话，变成我们日常使用的APP、小程序、网页等工具？

> 如果我们想快速添加常用的Prompt模板、或是想要拥有一个历史会话功能，应该怎么办？

> 此时，我们就需要通过代码级开发来自定义前端了，前面大家已经在Task1快速入门了代码级开发，复刻了自己的应用demo，相信下面的内容对你来说也绝对不在话下——

> 配套魔搭示例代码项目可见：https://modelscope.cn/studios/Datawhale/datawhale_spark_2024

## 零、预备：了解基础概念

关于开发类的内容，可能会涉及到很多新概念，为了尽量压缩篇幅，且让大家不用有那么多认知负担，我们仅会对`Gradio`框架以及相关术语进行简单介绍，主要目标是引导大家进入实操，且开始掌握如何解决问题的思路。

大家如果对相关信息有更多的学习需求，可以通过官方文档、浏览器搜索等方式研习。

`Gradio` 官方文档详见：https://www.gradio.app/

要想拥有更符合心意的应用，需要自己进行开发，即：我们需要把星火大模型的 `API` 作为一个功能组件，加入到我们自己的应用中去，做出属于我们创意的内容~

创建应用有很多种方式，常见如：网页、浏览器插件、小程序、移动应用程序(App)、桌面应用软件等。

一般来说，小程序、App、企业🛰️、订阅号服务等还有一定的开发及审核门槛，且需要学习较多的前端甚至UI设计相关的知识，需要具有一定的专业度和人力投入。

因此我们在快速验证功能和设计demo时，往往较优的选择是直接通过 `Gradio`/`Streamlit` 快捷开发可交互的demo。

使用 [飞桨AI Studio](https://aistudio.baidu.com/index) 、[魔搭Notebook](https://modelscope.cn/my/mynotebook/preset) 等云开发平台，可以减少很多本地环境配置的麻烦。

`dwspark`、`Gradio` 也只需要简单通过`pip`安装对应的`Python`库即可。

在学习之前，这里还有几个基础概念你需要知道：

- `Python`：一种简洁、易读、功能强大的高级编程语言，常言道“人生苦短，我用Python”。`Gradio` 是基于`Python`开发并开源的库。

- `pip`：`Python`的包管理工具，主要用于安装、管理和卸载`Python`的第三方库。它是`Python`开发中不可或缺的工具之一，常用命令：

  `pip install xxx`、`pip install --upgrade xxx`、`pip uninstall xxx`、`pip show xxx`、`pip install xxx==1.1.1`、`pip list`

  安装指定`python`库、升级指定`python`库、卸载指定`python`库、查看指定`Python`库的安装情况、安装指定`Python`库的指定版本、列出所有通过`pip`安装的`Python`库

- 在使用`gradio`、`streamlit`、以及`appbuilder-SDK`时，记得安装这些库哟~

- `SDK`：即“软件开发工具包”（Software Development Kit），一般会包含一系列工具和文档的集合，旨在帮助开发者更有效地开发特定的软件应用程序。工具可能包括编程语言的库、`APIs`、开发环境（IDE）、编译器、调试&性能分析工具、相关文档等。

- [`dwspark`](https://pypi.org/project/dwspark) 就是我们为了方便大家调用星火大模型`API`而封装开发的SDK

- **飞桨 AI Studio**：基于百度深度学习平台飞桨（PaddlePaddle）的人工智能学习与实训社区。它提供了一站式的模型在线开发与应用环境，支持多种编程语言和深度学习框架。

- 我们可以在AI Studio上进行`python`代码编写与运行，且可以免费使用其CPU环境（GPU环境需要付费）

- 另外我们的 `Gradio` 项目完成开发后，还可以通过其进行部署，分享给他人体验

- **魔搭ModelScope**：

  如果有本地开发的需求，欢迎自己探索环境配置，带着实操经验和问题在学习群进行提问和交流

## 一、[dwspark](https://pypi.org/project/dwspark) 进阶指南

### 为什么会有 `dwspark`？

考虑到官方提供很多模型能力，但各自有自己的SDK且调用起来较为麻烦，所以我们对基础的模型进行了代码封装，以方便大家以较为简洁的方式调用这些模型能力。

具体的模型官方接口文档见下表：

### 如何使用 `dwspark`？

#### 安装SDK

pypi包地址：https://pypi.org/project/dwspark

```
pip install dwspark
```

#### 加载配置

```
from dwspark.config import Config
# 加载系统环境变量：SPARKAI_APP_ID、SPARKAI_API_KEY、SPARKAI_API_SECRET
config = Config()
# 自定义key写入
config = Config('14****93', 'eb28b****b82', 'MWM1MzBkOD****Mzk0')
```

#### 调用模型

```
# SDK引入模型
from dwspark.models import ChatModel, Text2Img, ImageUnderstanding, Text2Audio, Audio2Text, EmbeddingModel
# 讯飞消息对象
from sparkai.core.messages import ChatMessage
# 日志
from loguru import logger

'''
对话
'''
# 模拟问题
question = '你好呀'
logger.info('----------批式调用对话----------')
model = ChatModel(config, stream=False)
logger.info(model.generate([ChatMessage(role="user", content=question)]))
logger.info('----------流式调用对话----------')
model = ChatModel(config, stream=True)
[logger.info(r) for r in model.generate_stream(question)]
logger.info('done.')

'''
文字生成语音
'''
text = '2023年5月，讯飞星火大模型正式发布，迅速成为千万用户获取知识、学习知识的“超级助手”，成为解放生产力、释放想象力的“超级杠杆”。2024年4月，讯飞星火V3.5春季升级长文本、长图文、长语音三大能力。一年时间内，讯飞星火从1.0到3.5，每一次迭代都是里程碑式飞跃。'
audio_path = './demo.mp3'
t2a = Text2Audio(config)
# 对生成上锁，预防公有变量出现事务问题，但会降低程序并发性能。
t2a.gen_audio(text, audio_path)

'''
语音识别文字
'''
a2t = Audio2Text(config)
# 对生成上锁，预防公有变量出现事务问题，但会降低程序并发性能。
audio_text = a2t.gen_text(audio_path)
logger.info(audio_text)

'''
生成图片
'''
logger.info('----------生成图片----------')
prompt = '一只鲸鱼在快乐游泳的卡通头像'
t2i = Text2Img(config)
t2i.gen_image(prompt, './demo.jpg')

'''
图片解释
'''
logger.info('----------图片解释----------')
prompt = '请理解一下图片'
iu = ImageUnderstanding(config)
logger.info(iu.understanding(prompt, './demo.jpg'))

'''
获取文本向量
'''
logger.info('----------获取文本向量----------')
em = EmbeddingModel(config)
vector = em.get_embedding("我们是datawhale")
logger.info(vector)
```

#### 模型列表

| 模型名称 | 调用方式 |
| --- | --- |
| 批式调用对话 | `model = ChatModel(config, stream=False)`<br>`model.generate([ChatMessage(role="user", content="对话内容")])` |
| 流式调用对话 | `model = ChatModel(config, stream=True)`<br>`[logger.info(r) for r in model.generate_stream("对话内容")]` |
| 文字生成语音 | `t2a = Text2Audio(config)`<br>`t2a.gen_audio("你好啊", "生成音频地址")` |
| 生成图片 | `t2i = Text2Img(config)`<br>`t2i.gen_image("生成图片需求", "图片地址")` |
| 图片解释 | `iu = ImageUnderstanding(config)`<br>`iu.understanding("图片理解方向描述", "图片地址")` |
| 获取文本向量 | `em = EmbeddingModel(config)`<br>`vector = em.get_embedding("需要向量化的文本")` |

### dwspark 代码详解

`dwspark`主要包含以下几个文件：

- `config.py`：统一的配置文件
- `utils`：一些工具类
- `models`：存放模型的地方
- `models.ChatModel.py`：对话相关模型
- `models.AudioModel.py`：音频相关模型
- `models.ImageModel.py`：图片相关模型
- `models.EmbeddingModel.py`：向量化模型

下面是其中两个代码的源码：

#### `config.py`

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @File  : config.py
# @Author: Richard Chiming Xu
# @Date  : 2024/6/24
# @Desc  :

import os


class Config():
    def __init__(self, appid: str = None, apikey: str = None, apisecret: str = None):
        """
        讯飞API统一的环境配置
        :param appid: appid
        :param apikey: api key
        :param apisecret: api secret

        调用方式：
        # 加载系统环境变量SPARKAI_APP_ID、SPARKAI_API_KEY、SPARKAI_API_SECRET
        # 系统环境变量需要进行赋值
        config = Config()
        # 自定义key写入
        config = Config('14****93', 'eb28b****b82', 'MWM1MzBkOD****Mzk0')
        """
        if appid is None:
            self.XF_APPID = os.environ["SPARKAI_APP_ID"]
        else:
            self.XF_APPID = appid
        if apikey is None:
            self.XF_APIKEY = os.environ["SPARKAI_API_KEY"]
        else:
            self.XF_APIKEY = apikey
        if apisecret is None:
            self.XF_APISECRET = os.environ["SPARKAI_API_SECRET"]
        else:
            self.XF_APISECRET = apisecret
```

#### `ChatModel.py`

