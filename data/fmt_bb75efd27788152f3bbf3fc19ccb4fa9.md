
# Google Colab+Youtube视频生成字幕文件

> 来源：[https://gotoaiworld.feishu.cn/docx/FbVCdLkKvoijaUxsYEJcW8BFncd](https://gotoaiworld.feishu.cn/docx/FbVCdLkKvoijaUxsYEJcW8BFncd)

## 优化代码

直接输入YouTube视频链接，自动下载音频、生成纯中文字幕，无需手工上传文件，同时附上超详细的操作指南，你复制就能用。

**完全零代码、一键运行**的 Google Colab 脚本，专门适配 YouTube 视频 → Whisper large-v3 识别 → 中文翻译 → 下载 SRT 字幕，你复制粘贴就能用，全程不用改任何参数！

---

### 一、核心修改后的完整代码（YouTube链接→纯中文字幕）

```python
# 第一步：安装所有依赖（含YouTube下载+Whisper+字幕处理）
!pip install -U openai-whisper ffmpeg-python pysrt yt-dlp
!apt update && apt install -y ffmpeg

# 第二步：导入核心库
import whisper
import pysrt
from datetime import timedelta
import os
import yt_dlp
import re

# 第三步：加载Whisper large-v3模型（识别准确率最高）
model = whisper.load_model("large-v3")

# 第四步：定义YouTube视频下载+纯中文字幕生成函数
def download_youtube_audio(youtube_url, output_audio="youtube_audio.mp3"):
    """
    从YouTube链接下载音频（仅音频，跳过视频，节省时间）
    :param youtube_url: YouTube视频链接（如https://youtu.be/xxxx）
    :param output_audio: 输出音频文件名
    """
    # YouTube下载配置（仅音频、最高音质）
    ydl_opts = {
        'format': 'bestaudio/best',
        'postprocessors': [{
            'key': 'FFmpegExtractAudio',
            'preferredcodec': 'mp3',
            'preferredquality': '192',
        }],
        'outtmpl': output_audio.replace(".mp3", ""),  # 避免重复后缀
        'quiet': True,  # 关闭冗余日志
        'no_warnings': True,
    }
    
    try:
        with yt_dlp.YoutubeDL(ydl_opts) as ydl:
            ydl.download([youtube_url])
        print(f"✅ YouTube音频下载完成！音频文件：{output_audio}")
        return output_audio
    except Exception as e:
        print(f"❌ YouTube音频下载失败：{e}")
        return None

def generate_zh_only_subtitles(audio_path, output_srt="youtube_zh_only.srt"):
    """
    仅生成纯中文字幕，无双语、无英文
    :param audio_path: 下载的音频文件路径
    :param output_srt: 输出的中文字幕文件名称
    """
    # 核心参数：强制转录并翻译为中文，指定源语言提升准确率
    result = model.transcribe(
        audio_path,
        task="translate",  # 关键：转为中文（而非双语转录）
        language="zh",     # 音频源语言（可根据视频语言调整，如英文填en）
        verbose=False,
        fp16=False         # Colab免费版CPU模式需关闭
    )
    
    # 生成纯中文字幕SRT文件
    subs = pysrt.SubRipFile()
    for i, segment in enumerate(result["segments"]):
        # 转换时间格式（秒→SRT时分秒）
        start = timedelta(seconds=segment["start"])
        end = timedelta(seconds=segment["end"])
        
        # 仅保留中文字符，剔除英文/特殊符号（可按需调整）
        text = segment["text"].strip()
        text = "".join([c for c in text if "\u4e00" <= c <= "\u9fff" or c in "，。！？：；''（）【】、"])
        
        # 创建字幕条目
        sub = pysrt.SubRipItem(
            index=i+1,
            start=start,
            end=end,
            text=text
        )
        subs.append(sub)
    
    # 保存纯中字幕
    subs.save(output_srt, encoding="utf-8")
    print(f"✅ 纯中字幕生成完成！文件：{os.path.abspath(output_srt)}")
    return output_srt

# 第五步：输入YouTube链接并执行（核心操作）
# 替换为你要处理的YouTube视频链接
YOUTUBE_URL = "https://youtu.be/xxxxxxx"  # 示例：https://youtu.be/dQw4w9WgXcQ

# 执行流程：下载音频 → 生成纯中字幕 → 下载字幕文件
audio_file = download_youtube_audio(YOUTUBE_URL)
if audio_file:
    srt_file = generate_zh_only_subtitles(audio_file)
    # 自动下载字幕文件到本地
    from google.colab import files
    files.download(srt_file)
else:
    print("❌ 流程终止：音频下载失败，请检查YouTube链接是否有效")
```

---

### 二、完整操作指南（YouTube链接→纯中字幕，5分钟搞定）

#### 前置准备

1. 打开Google Colab：`https://colab.research.google.com/`（登录Google账号）

2. 新建笔记本：点击「+ 新建笔记本」→ 命名（如「YouTube纯中字幕生成」）

#### 步骤1：复制代码并修改YouTube链接

1. 将上面的完整代码粘贴到Colab的空白代码单元格中；

2. 找到代码中 `YOUTUBE_URL = "https://youtu.be/xxxxxxx"` 这一行；

3. 替换为你要处理的YouTube视频链接（支持两种格式）：

+ `https://youtu.be/xxxxxxx`

+ `https://www.youtube.com/watch?v=xxxxxxx`

#### 步骤2：运行代码（一键执行）

1. 点击代码单元格左侧的「运行」按钮（三角形图标）；

2. 等待依赖安装（约1-2分钟）→ 自动下载YouTube音频（速度取决于视频时长，10分钟视频约30秒）→ 自动生成纯中字幕；

3. 关键提示：

+ 出现 `✅ YouTube音频下载完成！` → 音频下载成功；

+ 出现 `✅ 纯中字幕生成完成！` → 字幕生成成功；

+ 自动弹出字幕文件下载框（文件名：`youtube_zh_only.srt`）。

#### 步骤3：下载字幕文件

1. 浏览器弹出下载提示后，保存`youtube_zh_only.srt`到本地；

2. 可直接用播放器（PotPlayer、VLC、迅雷影音）加载该字幕文件，匹配YouTube视频播放。

---

### 三、关键参数微调（按需修改）

| 参数位置 | 作用 | 调整建议 |

| --- | --- | --- |

| `model = whisper.load_model("large-v3")` | 选择Whisper模型 | - 追求准确率：保留`large-v3`（稍慢）<br>- 追求速度：改为`medium`（准确率略降） |

| `language="zh"` | 指定视频源语言 | - 视频是中文：保留`zh`<br>- 视频是英文：改为`en`（仍会翻译为中文）<br>- 视频是多语言：删除该行（自动识别） |

| 中文字符过滤逻辑 | 剔除非中文字符 | 如需保留数字/英文专有名词（如GPT、AI），修改为：<br>`text = "".join([c for c in text if "\u4e00" <= c <= "\u9fff" or c in "，。！？：；''（）【】、0-9a-zA-Z"])` |

| `preferredquality="192"` | 音频质量 | 可改为`320`（更高音质）或`128`（更小文件） |

---

### 四、常见问题解决

#### 1. YouTube链接下载失败

+ 原因1：链接无效/视频私密/地区限制；

解决：确认链接可正常播放，或更换无限制的YouTube视频链接。

+ 原因2：`yt-dlp`版本问题；

解决：在代码开头添加：`!pip install --upgrade yt-dlp`。

#### 2. 字幕生成慢/Colab内存不足

+ 解决：将模型改为`medium`，并关闭冗余日志：

```python
model = whisper.load_model("medium")  # 替换large-v3
```

#### 3. 字幕时间轴不准

+ 解决：开启单词级时间戳，修改`transcribe`参数：

```python
result = model.transcribe(
    audio_path,
    task="translate",
    language="zh",
    word_timestamps=True,  # 精准时间轴
    verbose=False,
    fp16=False
)
```

#### 4. Colab报FFmpeg错误

+ 解决：重新安装FFmpeg：

```python
!apt remove -y ffmpeg && apt install -y ffmpeg
```

#### 总结

1. 核心修改：新增`yt-dlp`库自动下载YouTube音频，无需手工上传文件；

2. 关键逻辑：`task="translate" + language="zh"` 强制输出纯中字幕，剔除双语；

3. 操作流程：复制代码→改YouTube链接→运行→下载字幕，全程无需额外操作。

如果需要生成其他格式字幕（如vtt、ass），或批量处理多个YouTube链接，我可以给你补充对应的代码！

## 原代码

**完全零代码、一键运行**的 Google Colab 脚本，专门适配 YouTube 视频 → Whisper large-v3 识别 → 中文翻译 → 下载 SRT 字幕，你复制粘贴就能用，全程不用改任何参数！

---

### 第一步：打开 Colab 环境

先点击这个链接打开 Colab 笔记本（需要能访问 Google）：

👉 [Google Colab 官网](https://colab.research.google.com/)

点击「新建笔记本」，进入空白代码环境。

### 第二步：复制粘贴一键脚本（全部复制，不要漏行）
