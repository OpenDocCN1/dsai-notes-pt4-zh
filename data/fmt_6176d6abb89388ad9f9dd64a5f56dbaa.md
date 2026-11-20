
英文听起来很不错，其他语言，大家可以线下自己试试。

[附-官网地址： Fish Audio官网](https://fish.audio/zh-CN/)

# Edge-TTS

Edge-TTS是由微软推出的文本转语音Python库，通过微软Azure Cognitive Services转化文本为自然语音。适合需要语音功能的开发者，GitHub上超3000星。作为国内付费TTS服务的替代品，`Edge-TTS`支持40多种语言和300种声音，提供优质的语音输出，满足不同开发需求。该库提供了一个简单的`API`，可以将文本转换为语音，并且支持多种语言和声音。允许你在`Python`代码中或使用提供的命令行工具轻松访问`Microsoft Edge`的在线文字转语音服务。要使用`Edge-TTS`库，首先需要安装上`Edge-TTS`库，安装直接使用`pip`进行安装即可。

## 安装

使用`pip`安装

```bash
pip install edge-tts
```

## 基本使用

### 命令行使用

#### 文本转语音

```bash
edge-tts --text "你好，世界！" --write-media hello.mp3
```

其中，`edge-tts`是命令，`text`后面跟着的是要合成的文字，`write-media`后面需要保存的语音文件（这里可以指定文件路径，用引号包围起来了。我的默认在当前用户名目下。）

注意：若不指定合成语言的话，默认语言和你的合成文本有关，若待合成的文本是中文，则合成的发音人默认是中文；若待合成的文本是英文，则合成的发音人默认是英文。那如何指定发音人呢？我们需要改变语音，来实现符合自己要求的语音。

#### 支持的语言和音色

```bash
# 查看可用的声音角色列表，推荐 zh-CN-XiaoxiaoNeural 女性，声音好听，也有部分角色是方言
edge-tts --list-voices

Name: af-ZA-AdriNeural Gender: Female
........
........
........
Name: zh-CN-XiaoxiaoNeural Gender: Female
Name: zh-CN-XiaoyiNeural Gender: Female
Name: zh-CN-YunjianNeural Gender: Male
Name: zh-CN-YunxiNeural Gender: Male
Name: zh-CN-YunxiaNeural Gender: Male
Name: zh-CN-YunyangNeural Gender: Male
Name: zh-CN-liaoning-XiaobeiNeural Gender: Female
Name: zh-CN-shaanxi-XiaoniNeural Gender: Female
Name: zh-HK-HiuGaaiNeural Gender: Female
Name: zh-HK-HiuMaanNeural Gender: Female
Name: zh-HK-WanLungNeural Gender: Male
Name: zh-TW-HsiaoChenNeural Gender: Female
Name: zh-TW-HsiaoYuNeural Gender: Female
Name: zh-TW-YunJheNeural Gender: Male
Name: zu-ZA-ThandoNeural Gender: Female
Name: zu-ZA-ThembaNeural Gender: Male
```

这里会打印出来所有支持的语音（`edge-tts`支持英语、汉语、日语、韩语、法语等40多种语言，共300多种可选声音），查询结果中的`Gender`为声音的性别，`Name`为声音的名字，如`zh-CN-YunjianNeural`，其中`zh`表示语言，`CN`表示国家或地区，可以根据需求选择不同的声音。我们使用`zh-CN-YunxiNeural`，来听听`zh-CN-YunxiNeural`生成的语音。这里我们只需要添加`--voice`命令，来指定输出的语音。

```bash
edge-tts --voice zh-CN-YunxiNeural --text "hello 大家好，这里是程序员盒子" --write-media hello.mp3
```

#### 调整语速、音量和音调

当然`edge-tts`还支持调整语速与音量，我们只需要添加`rate`与`volume`指令。

```bash
edge-tts --voice zh-CN-YunxiNeural --rate=-4% --text "hello 大家好" --write-media hello1.mp3
edge-tts --voice zh-CN-YunxiNeural --volume=-4% --text "hello 大家好" --write-media hello1.mp3
```

`rate=-4%`意思是语速降低4%，`volume=-4%`意思是音量降低4%，当然若是加号，便是增加多少。

### 集成EdgeTTS（Python代码使用）

在上面的介绍中，我们使用的是命令的方式来运行合成语音文件，但是如果是个web产品，如何集成这个能力呢？其实我们就是希望可以得到一个`API`接口，可以直接调用api，实现TTS功能的产品化。

这里使用`Python`的`FastAPI`提供了一个`POST`请求的接口，创建`tts.py`:

```python
import uvicorn
import edge_tts
from fastapi import FastAPI, Request, Body

app = FastAPI()
@app.post("/api/tts")
async def post_data(text: str = Body('', title='文本', embed=True),
                    voice: str = Body('', title='语言参数', embed=True),
                    name: str = Body('', title='文件名', embed=True),
                    rate: str = Body('', title='文件名', embed=True),
                    volume: str = Body('', title='文件名', embed=True)):
    output = "/home/work/tts/result/" + name + ".mp3"
    tts = edge_tts.Communicate(text=text, voice=voice, rate=rate, volume=volume)
    await tts.save(output)
    return {"message": output}
if name == 'main':
    uvicorn.run('tts:app', host='0.0.0.0', post=8080)
```

创建启动文件`tts.sh`

```bash
nohup uvicorn tts:app --host 0.0.0.0 --port 8080 >../tts/tts.log &
```

这条命令意在后台启动服务并记录日志。`nohup`命令可以让进程在用户退出终端后继续运行，而`&`符号则是将整个命令放到后台执行。日志会被重定向到`../tts/tts.log`文件中。

`--host 0.0.0.0`表示允许搜索id可以访问这个api服务，`--port 8080`是访问端口（可以修改）。

后面可以通过`curl`方式调用`edge-tts`服务了，好啦，人人都可以搭建一套自己的TTS服务啦。

## 测试效果

现在让我们愉快的测试`edge-tts`的合成效果吧（为了体现对比性，我们所使用的测试文本和`Fish Speech`的测试文本相同）

### 中文句子 1

```
人间灯火倒映湖中，她的渴望让静水泛起涟漪。若代价只是孤独，那就让这份愿望肆意流淌。
流入她所注视的世间，也流入她如湖水般澄澈的目光。
```

| 说话人 | 合成音频 |
| --- | --- |
| 陕西方言的女声 | |
| 台湾口音 | |

![](img/342dd717cbf5634b02d9820970ef655d.png)

[out1.mp3](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RhGobtiTfoZam4xChOVcNrj9ndg)

![](img/6770f3a817466556bfb005e4c57d461c.png)

[out2.mp3](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XfcfbmriUos8ukxqcDfcudOfnxb)

### 特殊中文句子 1

```
小王是个不怕折腾的年轻人，他总是充满活力地寻找各种机会。有一次，他决定自己创业开一家小店，但因为缺乏经验，在初期遇到了很多困难，甚至一度担心会折本。不过，通过不断地努力和调整策略，比如优化进货渠道避免商品积压造成损失，以及亲自设计店面布局吸引顾客，最终成功让生意步入正轨，并且没有再出现任何折断发展的迹象。
```

| 说话人 | 合成音频 |
| --- | --- |
| 随机说话人 | |

![](img/1706ff5e6448c48eb3dd64762936359b.png)

[多音字测试.mp3](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XvUXbPIbpowjMlxYHCScttPsncb)

看以听到，和`Fish-Speech`相比，`edge-tts`对于多音字的发音，准确率很高，那我们也测试“地”字的发音

### 特殊中文句子 2

```
在广阔的田野地面上，孩子们欢快地奔跑着，他们兴奋地讨论着谁跑得最远、跳得最高。小明突然指着远处说：“看，那边的花开得多美丽地”。
```

| 说话人 | 合成音频 |
| --- | --- |
| 随机说话人 | |

![](img/6ac8465f633c52a68dbb92b01c094ed6.png)

["地"字测试.mp3](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MZTEbyfSloVKsExIqvlcsb9Tntf)

针对“地”字的发音，`edge-tts`也很准确的发音正确

### 特殊中文句子 3

```
在学习语言的过程中，我们不仅要掌握听、说、读、写的基本技能，还需要了解背后的文化背景。比如，在日常交流中适当地使用一些`English words`可以使我们的表达更加丰富多彩。特别是在国际化的今天，`having some basic knowledge of different languages`是非常有帮助的。
```

| 说话人 | 合成音频 |
| --- | --- |
| 随机说话人 | |

![](img/6728a0ee07be04223e0614cb597f9b86.png)

[中英夹杂.mp3](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AjI5bWAfqoCLIWxaT7WcTQtRnpb)

总体而言，无需安装`Microsoft Edge`浏览器、不依赖`Windows`操作系统和`API`密钥，就能利用微软`Edge`提供的在线文本转语音功能。该项目是直接调用微软`Edge`浏览器的文本朗读有关`API`实现的文本转语音功能，不需要本地部署模型，使用简单，声音效果好。

## 缺点

+ 无法控制情感
+ 不支持音色克隆，只能用预定义音色
