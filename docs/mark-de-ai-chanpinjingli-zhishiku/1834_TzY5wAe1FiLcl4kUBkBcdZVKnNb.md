
# 从0-1飞书智能机器人落地应用

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/LmJ8dii6koVyJqxB1lOca0EvnBg](https://qqs7y1hozd1.feishu.cn/docx/LmJ8dii6koVyJqxB1lOca0EvnBg)

## 项目介绍

Feishu OpenAI，结合 OpenAI 强大能力的智能机器人框架。为用户提供一个多话题、场景化、角色扮演的互动平台。

> 👍项目文件：👉👉👉

![](img/0275436fae2a6f3956e21f96b9abba05.png)

[飞书智能机器人-项目文档.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CKFkb3lDGoWsbRx6DS2chln1nLd)

## 应用场景

在企业协作、客户服务、教育辅导、创意写作等领域，Feishu OpenAI 都能大展身手。

> 特别适用于需要即时反馈、个性化交流的场合。

> 无论是作为团队内部的知识助手，自动汇总会议记录，还是在客户支持中提供即时而精准的回答，它的多功能性使之成为一个强大的工作伙伴。

## 项目亮点

+ 仿真交流体验：独特的打字机效果，增加对话沉浸感。

+ 深度上下文理解：保留对话历史，持续深入交流无碍。

+ 简易部署：无论是本地部署、Serverless，还是 Docker，都有详尽指南。

+ 灵活性与定制性：允许通过多样化的 API 接入自己的业务逻辑或 AI 模型。

## 接入方式

### 步骤一：集成 SDK

#### 安装 SDK

```
pip install lark-oapi==1.1.0b1
```

步骤说明：

1. `lark.EventDispatcherHandler.builder()` 初始化事件处理器（`event_handler`），两个参数建议填空字符串

1. `event_handler` 的 `register_xxxx()` 方法监听不同的事件类型，上述示例中分别监听了「接收消息 v1.0」和「接收消息 v2.0」两个事件，更多事件可参考 处理消息事件回调 v1.0没写

1. `lark.ws.Client()` 初始化长连接客户端，必填参数为应用的 `APP_ID` 和 `APP_SECRET`，可在开发者后台获取

1. `event_handler`，同时可设置日志级别

1. `cli.start()` 启动客户端，如连接成功，控制台会打印 `"connected to wss://xxxxx"`，主线程将阻塞，直到进程结束

### 步骤二：切换订阅模式

1. 启动本地长连接客户端（如上述）

1. 登录开发者后台，选择一个企业自建应用；（不支持商店应用）

1. 进入「事件与回调」-「事件配置」页面

1. 编辑订阅方式，选择「使用长连接接收事件」，并保存（必须本地客户端启动正常，有长连接在线的情况下才能保存成功）

## 注意事项

+ 与 `Webhook` 相同，长连接模式下开发者接收到消息后，需要在 3 秒内处理完成，否则会触发超时重推

+ 消息推送为集群模式，不支持广播，即如果同一应用部署了多个客户端，只有一个会收到消息

+ 目前尚不支持卡片回调

+ 目前只支持飞书，不支持 `Lark`

# 长连接模式事件回调（免搭建服务器）

## 功能介绍

开发者通过集成飞书 SDK 与开放平台建立一条 `WebSocket` 全双工通道，当有事件回调发生时，开放平台会通过该通道向开发者发送消息。

与传统的 `Webhook` 模式相比，长连接模式大大降低了接入成本，将原先 1 周左右的开发周期降低到 5 分钟。

> 具体优势如下：

1. 测试阶段无需使用内网穿透工具，通过长连接模式在本地开发环境中即可接收事件回调；

1. 只在建连时进行鉴权，后续事件推送均为明文数据，无需开发者再处理解密和验签逻辑；

1. 只需保证运行环境具备访问公网能力即可，无需提供公网 IP 或域名；

1. 无需部署防火墙和配置白名单。

## 技术剖析

该项目构建于 `Go` 语言之上，提供了丰富的 SDK（如 `Go-OpenAI`）来简化开发流程。

> 它支持流式文本输出，实现仿真打字效果，增强对话的真实感，并且通过集成各类 AI 模型，实现了上下文理解、富文本回复等多种高级功能。

> 这一技术栈保证了高效率的同时，也确保了扩展性和稳定性，非常适合企业级应用。

## 扩展使用

开始发挥想象定制脚本了，常见的有以下：

1. 聊天触发器：接收消息后修改本地 `txt` 或其他文件，然后触发运行

1. 聊天调度：接受消息后调用 `api`，这样你就可以调度多台电脑运行任务（企业版功能）

1. 远程关机：接收消息后执行一条 `cmd` 命令，让电脑关机

1. 发送电脑文件：接收消息后，让机器人发送指定电脑文件

# 智能机器人配置

配置文件的模板在根目录的 `config-template.json` 中，需复制该模板创建最终生效的 `config.json` 文件：

```
cp config-template.json config.json
```

然后在 `config.json` 中填入配置，以下是对默认配置的说明，可根据需要进行自定义修改（注意实际使用时请去掉注释，保证 `JSON` 格式的完整）：

```
# config.json 文件内容示例
{
  "model": "gpt-3.5-turbo",                                   # 模型名称, 支持 gpt-3.5-turbo, gpt-4, gpt-4-turbo, wenxin, xunfei, glm-4, claude-3-haiku, moonshot
  "open_ai_api_key": "YOUR API KEY",                          # 如果使用 openAI 模型则填入上面创建的 OpenAI API KEY
  "proxy": "",                                                # 代理客户端的 ip 和端口，国内环境开启代理的需要填写该项，如 "127.0.0.1:7890"
  "single_chat_prefix": ["bot", "@bot"],                      # 私聊时文本需要包含该前缀才能触发机器人回复
  "single_chat_reply_prefix": "[bot] ",                       # 私聊时自动回复的前缀，用于区分真人
  "group_chat_prefix": ["@bot"],                              # 群聊时包含该前缀则会触发机器人回复
  "group_name_white_list": ["ChatGPT测试群", "ChatGPT测试群2"], # 开启自动回复的群名称列表
}
```

配置说明：

1. 个人聊天

+ 个人聊天中，需要以 `"bot"` 或 `"@bot"` 为开头的内容触发机器人，对应配置项 `single_chat_prefix`（如果不需要以前缀触发可以填写 `"single_chat_prefix": [""]`）

+ 机器人回复的内容会以 `"[bot] "` 作为前缀，以区分真人，对应的配置项为 `single_chat_reply_prefix`（如果不需要前缀可以填写 `"single_chat_reply_prefix": ""`）

2. 群组聊天

+ 群组聊天中，群名称需配置在 `group_name_white_list` 中才能开启群聊自动回复。如果想对所有群聊生效，可以直接填写 `"group_name_white_list": ["ALL_GROUP"]`

+ 默认只要被人 @ 就会触发机器人自动回复；另外群聊天中只要检测到以 `"@bot"` 开头的内容，同样会自动回复（方便自己触发），这对应配置项 `group_chat_prefix`

+ 可选配置：`group_name_keyword_white_list` 配置项支持模糊匹配群名称，`group_chat_keyword` 配置项则支持模糊匹配群消息内容，用法与上述两个配置项相同。（Contributed by evolay）

+ `group_chat_in_one_session`：使群聊共享一个会话上下文，配置 `["ALL_GROUP"]` 则作用于所有群聊

3. 语音识别

+ `"speech_recognition": true` 将开启语音识别，默认使用 `openai` 的 `whisper` 模型识别为文字，同时以文字回复，该参数仅支持私聊（注意由于语音消息无法匹配前缀，一旦开启将对所有语音自动回复，支持语音触发画图）；

+ `"group_speech_recognition": true` 将开启群组语音识别，默认使用 `openai` 的 `whisper` 模型识别为文字，同时以文字回复，参数仅支持群聊（会匹配 `group_chat_prefix` 和 `group_chat_keyword`，支持语音触发画图）；

+ `"voice_reply_voice": true` 将开启语音回复语音（同时作用于私聊和群聊）

4. 其他配置

+ `model`：模型名称，目前支持 `g-t-3.5-turbo`, `g-t-4o`, `g-t-4-turbo`, `g-t-4`, `wenxin`, `claude`, `gemini`, `glm-4`, `xunfei`, `moonshot`

+ `temperature`, `frequency_penalty`, `presence_penalty`: `Chat API` 接口参数。

+ `proxy`：由于目前 `openai` 接口国内无法访问，需配置代理客户端的地址，详情参考

+ 对于图像生成，在满足个人或群组触发条件外，还需要额外的关键词前缀来触发，对应配置 `image_create_prefix`

+ 关于 `OpenAI` 对话及图片接口的参数配置（内容自由度、回复字数限制、图片大小等），可以参考 对话接口 和 图像接口 文档，在 `config.py` 中检查哪些参数在本项目中是可配置的。

+ `conversation_max_tokens`：表示能够记忆的上下文最大字数（一问一答为一组对话，如果累积的对话字数超出限制，就会优先移除最早的一组对话）

+ `rate_limit_chatg-t`, `rate_limit_dalle`：每分钟最高问答速率、画图速率，超速后排队按序处理。

+ `clear_memory_commands`: 对话内指令，主动清空前文记忆，字符串数组可自定义指令别名。

+ `hot_reload`: 程序退出后，暂存等于状态，默认关闭。

+ `character_desc` 配置中保存着你对机器人说的一段话，他会记住这段话并作为他的设定，你可以为他定制任何人格（关于会话上下文的更多内容参考该 issue）

+ `subscribe_msg`：订阅消息，👸号和企业 `channel` 中请填写，当被订阅时会自动回复，可使用特殊占位符。目前支持的占位符有 `{trigger_prefix}`，在程序中它会自动替换成 bot 的触发词。

5. LinkAI 配置 (可选)

+ `use_linkai`: 是否使用 `LinkAI` 接口，开启后可国内访问，使用知识库和 `Midjourney` 绘画，参考 文档

+ `linkai_api_key`: `LinkAI Api Key`，可在 控制台 创建

+ `linkai_app_code`: `LinkAI` 应用或工作流的 code，选填

# 智能机器人运行

## 1. 本地运行

如果是开发机本地运行，直接在项目根目录下执行：

```
python3 app.py                                    # windows 环境下该命令通常为 python app.py
```

终端输出二维码后，进行扫码登录，当输出 `"Start auto replying"` 时表示自动回复程序已经成功运行了（注意：用于登录的账号需要已完成实名认证）。扫码登录后你的账号就成为机器人了，可以在手机端通过配置的关键词触发自动回复。

## 2. 服务器部署

使用 `nohup` 命令在后台运行程序：

```
nohup python3 app.py & tail -f nohup.out          # 在后台运行程序并通过日志输出二维码
```

> 扫码登录后程序即可运行于服务器后台，此时可通过 `ctrl+c` 关闭日志，不会影响后台程序的运行。使用 `ps -ef | grep app.py | grep -v grep` 命令可查看运行于后台的进程，如果想要重新启动程序可以先 `kill` 掉对应的进程。日志关闭后如果想要再次打开只需输入 `tail -f nohup.out`。此外，`scripts` 目录下有一键运行、关闭程序的脚本供使用。

> 多账号支持：将项目复制多份，分别启动程序，用不同账号扫码登录即可实现同时运行。

> 特殊指令：用户向机器人发送 `#reset` 即可清空该用户的上下文记忆。

## 3. Docker 部署

> 使用 `Docker` 部署无需下载源码和安装依赖，只需要获取 `docker-compose.yml` 配置文件并启动容器即可。

> 前提是需要安装好 `docker` 及 `docker-compose`

(1) 下载 `docker-compose.yml` 文件

```
wget https://open-1317903499.cos.ap-guangzhou.myqcloud.com/docker-compose.yml
```

下载完成后打开 `docker-compose.yml` 修改所需配置，如 `OPEN_AI_API_KEY` 和 `GROUP_NAME_WHITE_LIST` 等。

(2) 启动容器

在 `docker-compose.yml` 所在目录下执行以下命令启动容器：

```
sudo docker compose up -d
```

运行 `sudo docker ps` 能查看到 NAMES 为 `chatgpt-on-wechat` 的容器即表示运行成功。

> 注意：

+ 如果 `docker-compose` 是 `1.X` 版本 则需要执行 `sudo docker-compose up -d` 来启动容器

+ 该命令会自动去 `Docker Hub` 拉取 `latest` 版本的镜像，`latest` 镜像会在每次项目 release 新的版本时生成

最后运行以下命令可查看容器运行日志，扫描日志中的二维码即可完成登录：

```
sudo docker logs -f chatgpt-on-wechat
```

(3) 插件使用

如果需要在 `Docker` 容器中修改插件配置，可通过挂载的方式完成，将 插件配置文件 重命名为 `config.json`，放置于 `docker-compose.yml` 相同目录下，并在 `docker-compose.yml` 中的 `chatgpt-on-wechat` 部分下添加 `volumes` 映射：

```
volumes:
  - ./config.json:/app/plugins/config.json
```

## Railway 部署

> Railway 每月提供 5 刀和最多 500 小时的免费额度。（07.11 更新：目前大部分账号已无法免费部署）

1. 进入 Railway

1. 点击 Deploy Now 按钮。

1. 设置环境变量来重载程序运行的参数，例如 `open_ai_api_key`, `character_desc`。

# 智能机器人的关键技术

智能机器人之所以能够实现自主决策和任务执行，离不开以下几项关键技术：

## 机器视觉

机器视觉是智能机器人的重要感知能力之一。通过摄像头等传感器，机器可以获取周围环境的图像或视频流。然后，利用计算机视觉算法，机器可以识别、跟踪和分析图像中的对象、人脸、文字等信息。这项技术在无人驾驶汽车、工业生产线上的质量控制、医疗影像诊断等领域有着广泛的应用。

```
# 使用 Python 的 OpenCV 库进行图像处理和对象识别
import cv2

# 读取图像
image = cv2.imread('robot_image.jpg')

# 进行对象识别
# ...

# 显示识别结果
cv2.imshow('Object Recognition', image)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

## 机器学习与深度学习

机器学习和深度学习是智能机器人实现自主决策的核心技术。通过训练模型，机器可以从大量数据中学习规律和模式，然后根据学到的知识做出决策。深度学习技术，特别是神经网络，已经在语音识别、自然语言处理、图像识别等领域取得了重大突破。



```python
# 使用Python的TensorFlow库进行深度学习
import tensorflow as tf

# 构建神经网络模型
model = tf.keras.Sequential([
    tf.keras.layers.Conv2D(32, (3, 3), activation='relu', input_shape=(224, 224, 3)),
    tf.keras.layers.MaxPooling2D((2, 2)),
    tf.keras.layers.Flatten(),
    tf.keras.layers.Dense(128, activation='relu'),
    tf.keras.layers.Dense(10)
])

# 编译模型
model.compile(optimizer='adam',
              loss=tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True),
              metrics=['accuracy'])

# 训练模型
model.fit(train_images, train_labels, epochs=10)
```

## 传感器技术

传感器是智能机器人的“感官”，它们可以获取有关环境的各种信息，包括温度、湿度、光线强度、距离、声音等。这些传感器数据对于机器决策和行动至关重要。

## 核心算法原理

在知识图谱与人工智能聊天机器人领域的应用中，主要涉及以下几个核心算法：

> 实体识别(Entity Recognition)：将自然语言文本中的实体识别出来，并将其映射到知识图谱中对应的实体。

> 关系推理(Relation Inference)：根据知识图谱中的实体和关系，推导出新的关系。

> 对话管理(Dialogue Management)：控制聊天机器人与用户交互的过程，包括对话的开始、结束、转移等。

## 具体操作步骤

实现知识图谱与人工智能聊天机器人的应用，可以按照以下步骤进行：

> 构建知识图谱：首先需要构建一个知识图谱，包括实体、关系、属性和属性值等。

> 训练自然语言处理模型：使用自然语言处理技术，将用户输入的自然语言转换为计算机可以理解的形式。

> 实现实体识别：将自然语言文本中的实体识别出来，并将其映射到知识图谱中对应的实体。

> 实现关系推理：根据知识图谱中的实体和关系，推导出新的关系。

> 实现对话管理：控制聊天机器人与用户交互的过程，包括对话的开始、结束、转移等。

## 数学模型公式详细讲解

在知识图谱与人工智能聊天机器人领域的应用中，主要涉及以下几个数学模型：

实体识别模型：

> 其中，表示实体 `e` 在文本 `w` 中的概率，表示实体 `e` 在文本 `w` 中的相关性分数。

关系推理模型：`P(r|e1,e2) = \frac{exp(s(r,e1,e2))}{\sum{r'}exp(s(r',e1,e2))}` 其中，`r` 表示关系 `r` 在实体 `e1` 和 `e2` 之间的概率，`s(r,e1,e2)` 表示关系 `r` 在实体 `e1` 和 `e_2` 之间的相关性分数。

对话管理模型：`P(a{t+1}|a1,a2,...,at,u1,u2,...,ut) = \sum{a{t+1}}P(a{t+1}|at,ut)P(ut|at)` 其中，`a{t+1}` 表示对话的下一步行动 `a{t+1}` 在对话历史 `a1,a2,...,at,u1,u2,...,ut` 的概率，`P(a{t+1}|at,ut)` 表示对话的下一步行动 `a{t+1}` 在当前对话行动 `at` 和用户输入 `ut` 的概率，`P(ut|at)` 表示用户输入 `ut` 在当前对话行动 `at` 的概率。

## 具体代码实例和详细解释说明

### 实体识别示例

在实体识别示例中，我们可以使用以下Python代码实现：

```python
import spacy
```

### 加载spacy模型

```python
nlp = spacy.load("encoreweb_sm")
```

### 输入文本

```python
text = "Barack Obama was the 44th President of the United States."
```

### 对文本进行实体识别

```python
doc = nlp(text)
```

### 输出实体和实体类型

```python
for ent in doc.ents:
    print(ent.text, ent.label_)
```

### 关系推理示例

在关系推理示例中，我们可以使用以下Python代码实现：

```python
from rdfquerysolver import RDFQuerySolver
```

### 加载RDF数据

```python
rdfdata = RDFQuerySolver.loadrdf_data("example.ttl")
```

### 输入查询

```python
query = "SELECT ?x WHERE { ?x rdf:type ns:Person . }"
```

### 对查询进行关系推理

```python
results = RDFQuerySolver.query(rdf_data, query)
```

### 输出结果

```python
for result in results:
    print(result["x"])
```

### 对话管理示例

在对话管理示例中，我们可以使用以下Python代码实现：

```python
from rasa.nlu.model import Interpreter
```

### 加载NLU模型

```python
interpreter = Interpreter.load("nlu_model")
```

### 输出解析结果

```python
print(interpretation.get("entities")) print(interpretation.get("intent").get("name"))
```
