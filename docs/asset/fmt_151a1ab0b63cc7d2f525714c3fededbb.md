
# 半小时，帮你实现一个AI版“图片搜索引擎”

> 来源：[https://aiyanxishe.feishu.cn/docx/I6U9d4mUPo3k7KxSpCxcLn4uncf](https://aiyanxishe.feishu.cn/docx/I6U9d4mUPo3k7KxSpCxcLn4uncf)

## 程序员AI破局简介

> [《程序员AI破局指南》](https://t.zsxq.com/19EOflzkQ)由字节、百度、阿里等互联网大厂的AI业务相关高管共创，分享开箱即用、面向市场的AI编程知识和工具。

> 指南包括六个专栏：**AI编程实战、AI编程工具、AI高效办公、AI副业赚钱、AI提示词工程、AI私人助手**

> [程序员AI破局指南](https://t.zsxq.com/19EOflzkQ) 原价 599 元，限时特惠 199 元/年，5月1日恢复原价。

> 加入我们：<https://t.zsxq.com/19EOflzkQ>

> 购买后，添加群主🛰️加入 AI研习社陪伴社群，限时领取超值福利。

## 一、技术方案

### 一）概览

**我们都**知道搜索引擎「百度」「谷歌」可以搜索互联网上的所有文字内容。

搜索引擎对**结构化的数据可以**识别得非常精准，但是对于非结构化的数据，比如图片、视频、音频却无能为力。

下面这个百度搜索出的图片也是根据图片下面的文字构建的索引，是搜不出图片内的信息的。比如我是个自媒体作者想要引用电影中某个片段，直接用传统搜索引擎完全不行。

![](img/f1429723fa44055201b550efdc961a36.png)

**AI时代这里就需要新一代**的搜索引擎了。Embedding登场，它可以把所有非结构化数据统一转化为向量，进行向量搜索进行包含语义的查找。

[关于什么是RAG和Embedding就不赘述了，之前这篇讲](https://mp.weixin.qq.com/s/Onn5AiwZBenvPHxIyYoIXg)得很详细：<https://mp.weixin.qq.com/s/Onn5AiwZBenvPHxIyYoIXg>

RAG搜索引擎公式：

`语料+EmbeddingModel+向量库+LLM（可选）=搜索引擎`

`图片文件+图片EmbeddingModel+向量库=图片搜索引擎`

`视频文件+视频EmbeddingModel+向量库=视频搜索引擎`

`音乐文件+音乐EmbeddingModel+向量库=音乐搜索引擎`

其他垂直行业，比如人脸识别、专利文件、病毒特征，只要训练好对应的`EmbeddingModel`都可以实现对应的搜索引擎。

**本次就采用上述**的技术方案来完成图片搜索引擎的功能。

拆分成下面几步：选型`EmbeddingModel`，选型向量库，代码编写，代码示例。

### 二）Embedding Model选型

Embedding是一个浮点数向量（列表）。两个向量之间的距离测量它们的相关性。较小的距离表示高相关性，较大的距离表示低相关性。

简单来说就是把所有非结构化数据转化成可以让计算机理解的有语义的数据。

图片Embedding，只会调用openai接口可搞不定了，需要本地运行模型了。这就不得不了解huggingface这个神器了。

huggingface图片分类相关模型：<https://huggingface.co/models?pipeline_tag=zero-shot-image-classification>

![](img/2dc5e54ec232734945044f450d71a9f9.png)

我[们](https://huggingface.co/openai/clip-vit-base-patch16)还是用老大哥openai的[clip](https://openai.com/research/clip)模型试试水。暂时就用最新的，且资源占用不太多的[`openai/clip-vit-base-patch16`](https://huggingface.co/openai/clip-vit-base-patch16)。

![](img/7d95a97cd740fcd32b869fa5c3d04dc5.png)

### 三）向量库技术选型

向量数据库简单来说就是用来存储向量，查询向量的数据库。

**1.专用向量数据库**

这些数据库从一开始就设计用于处理和检索向量数据，专门为高效的向量搜索和机器学习应用优化。

**Milvus**

优点: 高效的向量检索，适合大规模数据集。

缺点: 相对较新，社区和资源可能不如成熟的传统数据库丰富。

**Weaviate**

优点: 支持图结构和自然语言理解，适用于复杂查询。

缺点: 作为新兴技术，可能存在稳定性和成熟度方面的问题。

**2.传统数据库的向量扩展**

这些是传统数据库，通过扩展或插件支持向量数据的存储和检索。

**Elasticsearch**

优点: 强大的全文搜索功能，大型社区支持。

缺点: 向量搜索可能不如专用向量数据库那么高效。

PostgreSQL（使用`PGVector`或其他向量扩展）

优点: 结合了传统的关系数据库强大功能和向量搜索。

缺点: 向量搜索的性能和优化可能不及专用向量数据库。

**Redis**

优点: Redis是一种非常流行的开源内存数据结构存储系统，它以其高性能和灵活性而闻名。Redis通过模块如`RedisAI`和`RedisVector`支持向量数据，这使得它能够进行高速向量计算和近似最近邻（ANN）搜索，非常适合实时应用。

缺点: 由于Redis主要是作为内存数据库，大规模的向量数据集可能会受到内存大小的限制。此外，它在处理大量复杂的向量操作方面可能不如专用向量数据库那样高效。

**3. 向量库的云服务**

这些服务提供了向量数据处理和搜索的云解决方案，通常易于扩展和维护。

**Pinecone**

优点: 易于扩展，无需管理基础设施，适合快速部署。

缺点: 依赖于云提供商，可能存在成本和数据迁移方面的考虑。

Amazon Elasticsearch Service

优点: 完全托管，与AWS生态系统紧密集成。

缺点: 可能涉及较高成本，且高度依赖AWS服务。

对于我来说更倾向于使用传统数据库的向量扩展，比如这里我使用`Redis+RedisSearch`来实现向量库。

因为企业级项目很多时候已经维护了一个大的Redis集群，而且很多云厂商也暂时不支持专用向量数据库。

Redis官方对向量支持介绍：<https://redis.com/blog/rediscover-redis-for-vector-similarity-search/>

## 二、代码实战

### 一）技术架构

分为导入和搜索两个流程。

导入过程：

把我们需要导入的一篮子图片集通过Clip转成向量，然后把向量导入Redis存储起来。

搜索过程：

搜索过程也是类似，把我们想要搜索的图片和文本同样使用Clip使用也进行向量化，然后去Redis中进行向量检索，就实现图片搜索引擎啦！

![](img/caee3ed23f7819ed5423f24a84ff7fd5.png)

### 二）准备图片

[既然是图片搜](https://github.com/FFmpeg/FFmpeg)索引擎，当然得有海量图片了。处理多媒体文件的神器登场：FFmpeg

#### 1、安装`ffmpeg`

官方下载链接：<https://ffmpeg.org/download.html>

mac系统安装更简单，直接用homebrew安装

```
brew install ffmpeg
```

其他操作系统（linux、window）安装也比较简单，可以直接去官网下载。

官方下载链接：<https://ffmpeg.org/download.html>

![](img/4173b7c9367d1fc4c0fb1f32fe48a13c.png)

#### 2、生成图片

我选了个自己比较喜欢的电影《楚门的世界》，把视频文件进行每秒抽帧来生成图片素材。

```
ffmpeg -i Top022.楚门的世界.The.Truman.Show.1998.Bluray.1080p.x265.AAC\(5.1\).2Audios.GREENOTEA.mkv  -r 1 truman-%3d.png
```

![](img/01810ff7adceb61bf79f4980f7a64e19.png)

经过一段时间的cpu飞速运转，得到了2000多张图片

![](img/f0714b3c90ce974dd380a5eb78c1fa8a.png)

### 三）环境准备

#### 1、安装`Python`

安装`python`的可以直接去官网 <https://www.python.org/downloads/>

英文不太好可以去看这个国人出的菜鸟教程：<https://www.runoob.com/python3/python3-install.html>

#### 2、安装`redis`

推荐使用`docker`安装`redis`，我们选用了7.2.0的新版本redis

`docker-compose`文件参考：

```
version: "2.4"

services:
  redis-server:
    image: redis/redis-stack-server:7.2.0-v6
    container_name: redis-server
    ports:
      - 6379:6379
    volumes:
      - ./redis-data:/data
```

启动`redis`

```
docker-compose up -d
```

![](img/0fa3933a805eabbd1c8b43407c888524.png)

### 四）图片向量化

#### 1、导入向量库

指定我们图片的目录，把每张图片使用`clip`进行向量化，然后存入`redis`向量库中。

```
import torch
import numpy as np
from transformers import CLIPProcessor, CLIPModel
from PIL import Image
import time
import os
import redis

# 连接 Redis 数据库，地址换成你自己的 Redis 地址
client = redis.Redis(host="localhost", port=6379, decode_responses=True)
res = client.ping()
print("redis connected:", res)

model_name_or_local_path = "openai/clip-vit-base-patch16"
model = CLIPModel.from_pretrained(model_name_or_local_path)
processor = CLIPProcessor.from_pretrained(model_name_or_local_path)

# 换成你的图片目录
image_directory = "/Users/david/Downloads/turman"

png_files = [filename for filename in os.listdir(image_directory) if filename.endswith(".png")]
sorted_png_files = sorted(png_files, key=lambda x: int(x.split('-')[-1].split('.')[0]))

# 初始化 Redis Pipeline
pipeline = client.pipeline()
for i, png_file in enumerate(sorted_png_files, start=1):
    # 初始化 Redis，先使用 PNG 文件名作为 Key 和 Value，后续再更新为图片特征向量
    pipeline.json().set(png_file, "$", png_file)

batch_size = 1

with torch.no_grad():
    for idx, png_file in enumerate(sorted_png_files, start=1):
        print(f"{idx}: {png_file}")
        start = time.time()
        image = Image.open(f"{image_directory}/{png_file}")
        inputs = processor(images=image, return_tensors="pt", padding=True)
        image_features = model.get_image_features(inputs.pixel_values)[batch_size-1]
        embeddings = image_features.numpy().astype(np.float32).tolist()
        print('image_features:', embeddings)
        vector_dimension = len(embeddings)
        print('vector_dimension:', vector_dimension)
        end = time.time()
        print('%s Seconds'%(end-start))
        # 更新 Redis 数据库中的文件向量
        pipeline.json().set(png_file, "$", embeddings)
        res = pipeline.execute()
        print('redis set:', res)
```

导入完成后可以看到，我们的2000多张图片都作为`key`转存储到`redis`中了。

![](img/f4925d0a96fccc52d4ae6739e0869e40.png)

我们看到每个`key`的`value`都是512位的浮点数组，512位就代表我们向量的维度是512维，维度越高代表着存储的特征越多。

```
import redis

# 连接 Redis 数据库，地址换成你自己的 Redis 地址
client = redis.Redis(host="localhost", port=6379, decode_responses=True)
res = client.ping()
print("redis connected:", res)

res = client.json().get("truman-1234.png")
print(res)
print(len(res))
```

![](img/ec60f42d5010d2c716a00a84cf3cd1f9.png)

#### 2、构建向量索引

使用`RedisSearch`来把上面的向量构建索引。
