
# Dify自动生成播客

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Rj8qd2uJVoJgjLxXT6gcmmZanH2](https://qqs7y1hozd1.feishu.cn/docx/Rj8qd2uJVoJgjLxXT6gcmmZanH2)

## 需求

上传PDF文件，自动生成双人播客！

## 效果展示

点击[`audio.wav`](https://personel-zhouxinle888-a66353926f9185cff28f2bd374a5c3a9dd89d5206.gitlab.io/dify/audio.wav)下载，播放。

## Dify工作流编排

### 开始节点

上传PDF文件。

演示用PDF文件下载：[青少年信奥赛大纲](https://personel-zhouxinle888-a66353926f9185cff28f2bd374a5c3a9dd89d5206.gitlab.io/dify/%E9%9D%92%E5%B0%91%E5%B9%B4%E4%BF%A1%E5%A5%A5%E8%B5%9B%E5%A4%A7%E7%BA%B2.pdf)

![](`img/492c258e397984b242f1fdbfc9f3ac6a.png`)

### 文档提取器节点

将PDF文件中的纯文本提取出来。

![](`img/8833294206d78144b51eb9361f40a87e.png`)

### PDF文本处理节点

一般来说，由于字符、格式、Latex、表格等原因，PDF 提取出的内容可能会很凌乱。

对PDF文本进行处理，删除原始数据中包含换行符、Latex 数学公式以及一些可以完全删除的冗余内容。请去掉对播客作者记录无用的信息。

使用`gpt-4o-mini`模型对文本进行处理。

![](`img/3e3dd5b5b69b10efa1396fcd3d494591.png`)

### 播客讲稿撰写节点

使用`gpt-4o-mini`模型撰写播客讲稿。

![](`img/c13fe5603c0d56c9c068b94db7ca340a.png`)

### 播客讲稿润色节点

使用`gpt-4o-mini`模型润色播客讲稿。

生成播客讲稿结果：

```
[
(&quot;Speaker 1&quot;, &quot;欢迎来到我们的播客！今天我们要深入探讨全国青少年信息学奥林匹克竞赛，简称NOI。这个话题真是激动人心！自1984年创办以来，NOI已经吸引了超过十万名学生参与！这就像计算机界的超级碗一样热闹！我们将讨论这个竞赛的背景、目的，以及它如何帮助学生和教师。准备好了吗？&quot;),
(&quot;Speaker 2&quot;, &quot;嗯，听起来太棒了！我很好奇，NOI是怎么开始的？它背后有什么故事吗？&quot;),
(&quot;Speaker 1&quot;, &quot;哦，太好了！这个故事非常有趣。NOI起初是为了提升青少年的计算机科学技能。在1980年代，计算机还没有像今天这样普及。想象一下，那时只有少数人会编程。于是，一些热爱计算机的人决定举办比赛，激励更多学生参与。就像点燃了一把火，后来演变成了全国盛事！&quot;),
(&quot;Speaker 2&quot;, &quot;哇，真的很有意思！我能想象那种激情，像是一场初创企业的发布会，大家都在为新事物欢呼！那么现在，NOI的目的是什么呢？&quot;),
(&quot;Speaker 1&quot;, &quot;NOI的目的很明确。它不仅仅是比赛，更是一个教育平台，给学生展示才能的机会，也为教师提供指导方向。可以说，它就像连接学生和教师的桥梁，帮助他们更好地理解计算机科学的基本原理。就像经验丰富的教练指导年轻运动员如何发挥最佳状态。&quot;),
(&quot;Speaker 2&quot;, &quot;哦，我明白了！这就像在体育运动中，教练不仅教技巧，还帮运动员建立信心和团队精神。那么这个大纲是如何帮助学生和老师的呢？&quot;),
(&quot;Speaker 1&quot;, &quot;非常好的问题！大纲明确了知识层级，分为入门级、提高级和NOI级。这样，无论是刚入门的学生，还是准备参加高水平比赛的选手，都能找到合适的学习内容。就像学习乐器，先学会基础音阶，才能演奏复杂乐曲。&quot;),
(&quot;Speaker 2&quot;, &quot;哈哈，这个比喻太好了！那么在这些级别中，有哪些具体知识点或技能特别重要呢？&quot;),
(&quot;Speaker 1&quot;, &quot;当然有！在入门级，学生需要掌握C++程序设计、数据结构和算法基础，就像打地基，基础打好了，后面的建筑才能稳固。到了NOI级，学生需要掌握高级算法策略和数学知识，才能在比赛中脱颖而出。就像赛车手，只有掌握操控技巧，才能在比赛中超越对手。&quot;),
(&quot;Speaker 2&quot;, &quot;哇，听起来像是在准备高强度比赛！那这些知识点和技能是如何评估的呢？&quot;),
(&quot;Speaker 1&quot;, &quot;这是个好问题！NOI的评估不仅通过考试，而是通过实际编程比赛。学生在规定时间内解决算法问题，评估他们的思维和解决问题的能力。这就像马拉松比赛，他们需要在压力中保持冷静，找到最佳解决方案。&quot;),
(&quot;Speaker 2&quot;, &quot;这真是太刺激了！我能想象学生在比赛中紧张的样子，像面临最后一局的决斗！那么对于老师来说，这个大纲有什么建议呢？&quot;),
(&quot;Speaker 1&quot;, &quot;老师们可以根据大纲制定教学计划，确保教学内容与学生的比赛准备一致。就像音乐老师根据学生水平选择适合曲目指导。而且，大纲还强调避免对算法复杂度的常系数考察，集中考察经典内容，让学生更专注于核心知识学习。&quot;),
(&quot;Speaker 2&quot;, &quot;这真是个好主意！您认为这个大纲未来会有什么变化吗？&quot;),
(&quot;Speaker 1&quot;, &quot;正如任何优秀的教育体系，这个大纲会定期修订，以适应时代发展。想象一下，就像不断进化的游戏，随着新关卡解锁，玩家需要不断提升技能才能保持竞争力。&quot;),
(&quot;Speaker 2&quot;, &quot;真的太棒了！这让我对NOI和计算机科学的未来充满期待！谢谢您分享的精彩内容，让我大开眼界！&quot;),
(&quot;Speaker 1&quot;, &quot;不客气，我也很享受这个讨论！希望我们能激励更多年轻人投身计算机科学的世界！感谢大家收听今天的播客，期待下次再见！别忘了关注我们哦！&quot;)
]
```

哇，完美的生成一个双人播客讲稿结构化数据，它是一个数组结构的数据，里面包含多个元组数据，`Speaker 1` 和 `Speaker 2` 就是播客的两位播者。完美的结构化数据，为了接下来生成播客音频做准备。

### `HTTP请求节点`

请求部署在本地或者服务端的`Python WEB API`接口，生成音频。至于如何生成音频？先别急。

![](`img/625ac912ff5dbe564ae87fdf1ee3b3df.png`)

### 结束节点

输出音频url和生成的播客文本。

![](`img/83a6345d0c332b7d146d6af81649ac31.png`)

## 音频生成

### 安装`F5-TTS`

我们使用最近大火的`F5-TTS`：`F5-TTS`是由上海交通大学开源的一款高性能文本到语音（TTS）系统，基于流匹配的非自回归生成方法，结合扩散变换器（DiT）技术。系统在没有额外监督的情况下，基于零样本学习快速生成自然、流畅且忠实于原文的语音。`F5-TTS`支持多语言合成，包括中文和英文，能在长文本上进行有效的语音合成。

Github地址：`https://github.com/SWivid/F5-TTS`

注意：

`F5-TTS`不支持Mac架构，如果需要在Macbook上安装，可以安装`F5-TTS`的移植版本`F5-TTS-MLX`。Github地址：[`https://github.com/lucasnewman/f5-tts-mlx`](https://github.com/lucasnewman/f5-tts-mlx)

根据Github说明，在本机`Python`虚拟环境中安装`F5-TTS`依赖库。我使用的是`Python`版本是3.11。

### 编写`Python API`

使用`PyCharm IDE`创建一个依赖`Fast API`的`Pytyhon`项目。`Fast API`就是给`Pytyhon`项目提供对外API访问的能力。用于我们在Dify中`HTTP请求节点`调用。

代码流程：

+   接收上面播客讲稿润色节点生成的播客结构化数据。

+   解析播客结构化数据，生成分段数据，每一个分段就是一句话。

+   判断每一个分段的播者是谁。使用每一个分段中`Speaker 1`和`Speaker 2`标识去区分。`Speaker 1` 播者1 就使用女声参考音频生成音频结果；`Speaker 2` 播者2 就使用男声参考音频生成音频结果。

+   拼接每一个分段生成的音频文件，生成最终的完整音频文件。

核心代码：

接收Dify `HTTP请求节点`的播客结构化数据：

```
@app.post(&quot;/generate_podcast&quot;, response_model=PodcastResponse)
async def generate_podcast(req: PodcastRequest):
    try:
        print(req.content)
        # 生成分段数据
        segments = ast.literal_eval(req.content)
        
        # 生成音频片段
        audio_files = generate_podcast_segments(segments)
        
        # 合并音频文件
        output_path = &quot;./resources/_podcast.wav&quot;
        duration = merge_audio_files(audio_files, output_path)
        
        return PodcastResponse(
            audio_path=output_path,
            duration=duration
        )
        
    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))
```

生成播客片段音频文件：

```
def generate_podcast_segments(segments: List[Tuple[str, str]], output_dir: str = &quot;./resources/segments&quot;) -&gt; List[str]:
    &quot;&quot;&quot;生成播客片段音频文件&quot;&quot;&quot;
    os.makedirs(output_dir, exist_ok=True)
    
    audio_files = []
    for i, (speaker, text) in enumerate(segments, 1):
        output_path = f&quot;{output_dir}/_podcast_segment_{i}.wav&quot;
        
        # 根据说话人选择生成函数
        if speaker == &quot;Speaker 1&quot;:
            generate_speaker1_audio(text, output_path)
        else:  # Speaker 2
            generate_speaker2_audio(text, output_path)
            
        audio_files.append(output_path)
    
    return audio_files
```

合并音频文件：

```
def merge_audio_files(audio_files: List[str], output_path: str) -&gt; float:
    &quot;&quot;&quot;合并音频文件并返回总时长&quot;&quot;&quot;
    audio_data = []
    for file in audio_files:
        data, rate = sf.read(file)
        # 添加短暂停顿
        silence = np.zeros(int(SAMPLE_RATE * 0.5))  # 0.5秒的停顿
        audio_data.append(data)
        audio_data.append(silence)
    
    merged_audio = np.concatenate(audio_data)
    sf.write(output_path, merged_audio, SAMPLE_RATE)
    
    duration = len(merged_audio) / SAMPLE_RATE
    return duration
```

注意：直接调用`PyCharm`中的接口，`F5-TTS`下载所需模型很慢。即便`PyCharm`已经设置了代理。可以现在终端命令行中执行`F5-TTS`相关命令，会去下载所需模型。

### 始智AI平台部署`F5-TTS`

个人电脑配置有限，生成音频较慢。我们可以使用始智AI平台部署`F5-TTS`。

![](`img/6b2bfd380fad4808cdf5bb7def2c5233.png`)
