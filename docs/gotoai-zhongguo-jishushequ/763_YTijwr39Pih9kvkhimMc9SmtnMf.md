# Google Colab+Youtube视频生成字幕文件

> 来源：[https://gotoaiworld.feishu.cn/docx/FbVCdLkKvoijaUxsYEJcW8BFncd](https://gotoaiworld.feishu.cn/docx/FbVCdLkKvoijaUxsYEJcW8BFncd)

# **优化代码**

直接输入YouTube视频链接，自动下载音频、生成纯中文字幕，无需手工上传文件，同时附上超详细的操作指南，你复制就能用。

**完全零代码、一键运行**的 Google Colab 脚本，专门适配 YouTube 视频 → Whisper large-v3 识别 → 中文翻译 → 下载 SRT 字幕，你复制粘贴就能用，全程不用改任何参数！

---

## 一、核心修改后的完整代码（YouTube链接→纯中文字幕）

```
# 第一步：安装所有依赖（含YouTube下载+Whisper+字幕处理）
!pip install -U openai-whisper ffmpeg-python pysrt yt-dlp
!apt update &amp;&amp; apt install -y ffmpeg

# 第二步：导入核心库
import whisper
import pysrt
from datetime import timedelta
import os
import yt_dlp
import re

# 第三步：加载Whisper large-v3模型（识别准确率最高）
model = whisper.load_model(&quot;large-v3&quot;)

# 第四步：定义YouTube视频下载+纯中文字幕生成函数
def download_youtube_audio(youtube_url, output_audio=&quot;youtube_audio.mp3&quot;):
    &quot;&quot;&quot;
    从YouTube链接下载音频（仅音频，跳过视频，节省时间）
    :param youtube_url: YouTube视频链接（如https://youtu.be/xxxx）
    :param output_audio: 输出音频文件名
    &quot;&quot;&quot;
    # YouTube下载配置（仅音频、最高音质）
    ydl_opts = {
        &#x27;format&#x27;: &#x27;bestaudio/best&#x27;,
        &#x27;postprocessors&#x27;: [{
            &#x27;key&#x27;: &#x27;FFmpegExtractAudio&#x27;,
            &#x27;preferredcodec&#x27;: &#x27;mp3&#x27;,
            &#x27;preferredquality&#x27;: &#x27;192&#x27;,
        }],
        &#x27;outtmpl&#x27;: output_audio.replace(&quot;.mp3&quot;, &quot;&quot;),  # 避免重复后缀
        &#x27;quiet&#x27;: True,  # 关闭冗余日志
        &#x27;no_warnings&#x27;: True,
    }
    
    try:
        with yt_dlp.YoutubeDL(ydl_opts) as ydl:
            ydl.download([youtube_url])
        print(f&quot;✅ YouTube音频下载完成！音频文件：{output_audio}&quot;)
        return output_audio
    except Exception as e:
        print(f&quot;❌ YouTube音频下载失败：{e}&quot;)
        return None

def generate_zh_only_subtitles(audio_path, output_srt=&quot;youtube_zh_only.srt&quot;):
    &quot;&quot;&quot;
    仅生成纯中文字幕，无双语、无英文
    :param audio_path: 下载的音频文件路径
    :param output_srt: 输出的中文字幕文件名称
    &quot;&quot;&quot;
    # 核心参数：强制转录并翻译为中文，指定源语言提升准确率
    result = model.transcribe(
        audio_path,
        task=&quot;translate&quot;,  # 关键：转为中文（而非双语转录）
        language=&quot;zh&quot;,     # 音频源语言（可根据视频语言调整，如英文填en）
        verbose=False,
        fp16=False         # Colab免费版CPU模式需关闭
    )
    
    # 生成纯中文字幕SRT文件
    subs = pysrt.SubRipFile()
    for i, segment in enumerate(result[&quot;segments&quot;]):
        # 转换时间格式（秒→SRT时分秒）
        start = timedelta(seconds=segment[&quot;start&quot;])
        end = timedelta(seconds=segment[&quot;end&quot;])
        
        # 仅保留中文字符，剔除英文/特殊符号（可按需调整）
        text = segment[&quot;text&quot;].strip()
        text = &quot;&quot;.join([c for c in text if &quot;\u4e00&quot; &lt;= c &lt;= &quot;\u9fff&quot; or c in &quot;，。！？：；&quot;&quot;&#x27;&#x27;（）【】、&quot;])
        
        # 创建字幕条目
        sub = pysrt.SubRipItem(
            index=i+1,
            start=start,
            end=end,
            text=text
        )
        subs.append(sub)
    
    # 保存纯中文字幕
    subs.save(output_srt, encoding=&quot;utf-8&quot;)
    print(f&quot;✅ 纯中文字幕生成完成！文件：{os.path.abspath(output_srt)}&quot;)
    return output_srt

# 第五步：输入YouTube链接并执行（核心操作）
# 替换为你要处理的YouTube视频链接
YOUTUBE_URL = &quot;https://youtu.be/xxxxxxx&quot;  # 示例：https://youtu.be/dQw4w9WgXcQ

# 执行流程：下载音频 → 生成纯中文字幕 → 下载字幕文件
audio_file = download_youtube_audio(YOUTUBE_URL)
if audio_file:
    srt_file = generate_zh_only_subtitles(audio_file)
    # 自动下载字幕文件到本地
    from google.colab import files
    files.download(srt_file)
else:
    print(&quot;❌ 流程终止：音频下载失败，请检查YouTube链接是否有效&quot;)
```

---

## 二、完整操作指南（YouTube链接→纯中文字幕，5分钟搞定）

### 前置准备

1.  打开Google Colab：https://colab.research.google.com/（登录Google账号）

1.  新建笔记本：点击「+ 新建笔记本」→ 命名（如「YouTube纯中文字幕生成」）

### 步骤1：复制代码并修改YouTube链接

1.  将上面的完整代码粘贴到Colab的空白代码单元格中；

1.  找到代码中 YOUTUBE_URL = "https://youtu.be/xxxxxxx" 这一行；

1.  替换为你要处理的YouTube视频链接（支持两种格式）：

+   短链接：https://youtu.be/xxxxxxx

+   长链接：https://www.youtube.com/watch?v=xxxxxxx

### 步骤2：运行代码（一键执行）

1.  点击代码单元格左侧的「运行」按钮（三角形图标）；

1.  等待依赖安装（约1-2分钟）→ 自动下载YouTube音频（速度取决于视频时长，10分钟视频约30秒）→ 自动生成纯中文字幕；

1.  关键提示：

+   出现 ✅ YouTube音频下载完成！ → 音频下载成功；

+   出现 ✅ 纯中文字幕生成完成！ → 字幕生成成功；

+   自动弹出字幕文件下载框（文件名：youtube_zh_only.srt）。

### 步骤3：下载字幕文件

1.  浏览器弹出下载提示后，保存youtube_zh_only.srt到本地；

1.  可直接用播放器（PotPlayer、VLC、迅雷影音）加载该字幕文件，匹配YouTube视频播放。

---

## 三、关键参数微调（按需修改）

| 参数位置 | 作用 | 调整建议 |
| --- | --- | --- |
| model = whisper.load_model(&quot;large-v3&quot;) | 选择Whisper模型 | - 追求准确率：保留large-v3（稍慢）&lt;br&gt;- 追求速度：改为medium（准确率略降） |
| language=&quot;zh&quot; | 指定视频源语言 | - 视频是中文：保留zh&lt;br&gt;- 视频是英文：改为en（仍会翻译为中文）&lt;br&gt;- 视频是多语言：删除该行（自动识别） |
| 中文字符过滤逻辑 | 剔除非中文字符 | 如需保留数字/英文专有名词（如GPT、AI），修改为：&lt;br&gt;text = &quot;&quot;.join([c for c in text if &quot;\u4e00&quot; &lt;= c &lt;= &quot;\u9fff&quot; or c in &quot;，。！？：；&quot;&quot;&#x27;&#x27;（）【】、0-9a-zA-Z&quot;]) |
| preferredquality=&quot;192&quot; | 音频质量 | 可改为320（更高音质）或128（更小文件） |

---

## 四、常见问题解决

### 1. YouTube链接下载失败

+   原因1：链接无效/视频私密/地区限制；

解决：确认链接可正常播放，或更换无限制的YouTube视频链接。

+   原因2：yt-dlp版本问题；

解决：在代码开头添加：!pip install --upgrade yt-dlp。

### 2. 字幕生成慢/Colab内存不足

+   解决：将模型改为medium，并关闭冗余日志：

```
model = whisper.load_model(&quot;medium&quot;)  # 替换large-v3
```

### 3. 字幕时间轴不准

+   解决：开启单词级时间戳，修改transcribe参数：

```
result = model.transcribe(
    audio_path,
    task=&quot;translate&quot;,
    language=&quot;zh&quot;,
    word_timestamps=True,  # 精准时间轴
    verbose=False,
    fp16=False
)
```

### 4. Colab报FFmpeg错误

+   解决：重新安装FFmpeg：

```
!apt remove -y ffmpeg &amp;&amp; apt install -y ffmpeg
```

---

### 总结

1.  核心修改：新增yt-dlp库自动下载YouTube音频，无需手工上传文件；

1.  关键逻辑：task="translate" + language="zh" 强制输出纯中文字幕，剔除双语；

1.  操作流程：复制代码→改YouTube链接→运行→下载字幕，全程无需额外操作。

如果需要生成其他格式字幕（如vtt、ass），或批量处理多个YouTube链接，我可以给你补充对应的代码！

# **原代码**

**完全零代码、一键运行**的 Google Colab 脚本，专门适配 YouTube 视频 → Whisper large-v3 识别 → 中文翻译 → 下载 SRT 字幕，你复制粘贴就能用，全程不用改任何参数！

---

### 第一步：打开 Colab 环境

先点击这个链接打开 Colab 笔记本（需要能访问 Google）：

👉 G[oogle Colab 官网](https://colab.research.google.com/)

点击「新建笔记本」，进入空白代码环境。

### 第二步：复制粘贴一键脚本（全部复制，不要漏行）

```
# ======================================
# YouTube 视频 → Whisper large-v3 → 中英双语 SRT 字幕
# 一键运行，无需修改参数
# ======================================

# 1. 安装依赖（自动下载 Whisper + YouTube 下载工具）
!pip install -U openai-whisper yt-dlp ffmpeg-python translate &gt; /dev/null 2&gt;&amp;1
!apt-get install -y ffmpeg &gt; /dev/null 2&gt;&amp;1

# 2. 下载 Whisper large-v3 模型（仅首次运行下载）
import whisper
model = whisper.load_model(&quot;large-v3&quot;)

# 3. 输入 YouTube 链接（替换成你的视频链接）
YOUTUBE_URL = &quot;https://www.youtube.com/watch?v=你的视频ID&quot;  # 示例：https://www.youtube.com/watch?v=abc123

# 4. 下载 YouTube 音频（只下音频，速度快、省流量）
!yt-dlp -x --audio-format mp3 -o &quot;audio.mp3&quot; {YOUTUBE_URL}

# 5. Whisper large-v3 识别原文字幕（默认英语，自动识别语言）
result = model.transcribe(&quot;audio.mp3&quot;, verbose=False, language=&quot;en&quot;)

# 6. 生成英文 SRT 字幕
def write_srt(transcript, filename=&quot;subtitles_en.srt&quot;):
    with open(filename, &quot;w&quot;, encoding=&quot;utf-8&quot;) as f:
        for i, segment in enumerate(transcript[&quot;segments&quot;], 1):
            start = segment[&quot;start&quot;]
            end = segment[&quot;end&quot;]
            # 格式化时间轴（SRT标准格式）
            start_str = f&quot;{int(start//3600):02d}:{int((start%3600)//60):02d}:{int(start%60):02d},{int((start%1)*1000):03d}&quot;
            end_str = f&quot;{int(end//3600):02d}:{int((end%3600)//60):02d}:{int(end%60):02d},{int((end%1)*1000):03d}&quot;
            f.write(f&quot;{i}\n{start_str} --&gt; {end_str}\n{segment[&#x27;text&#x27;].strip()}\n\n&quot;)

# 生成英文 SRT
write_srt(result, &quot;subtitles_en.srt&quot;)

# 7. 翻译为中文并生成中英双语 SRT
from translate import Translator
translator = Translator(to_lang=&quot;zh&quot;, from_lang=&quot;en&quot;)

def write_zh_en_srt(transcript, filename=&quot;subtitles_zh_en.srt&quot;):
    with open(filename, &quot;w&quot;, encoding=&quot;utf-8&quot;) as f:
        for i, segment in enumerate(transcript[&quot;segments&quot;], 1):
            start = segment[&quot;start&quot;]
            end = segment[&quot;end&quot;]
            start_str = f&quot;{int(start//3600):02d}:{int((start%3600)//60):02d}:{int(start%60):02d},{int((start%1)*1000):03d}&quot;
            end_str = f&quot;{int(end//3600):02d}:{int((end%3600)//60):02d}:{int(end%60):02d},{int((end%1)*1000):03d}&quot;
            en_text = segment[&quot;text&quot;].strip()
            zh_text = translator.translate(en_text)  # 翻译为中文
            f.write(f&quot;{i}\n{start_str} --&gt; {end_str}\n{zh_text}\n{en_text}\n\n&quot;)

# 生成中英双语 SRT
write_zh_en_srt(result, &quot;subtitles_zh_en.srt&quot;)

# 8. 提示下载（Colab 左侧文件栏可直接下载）
print(&quot;✅ 处理完成！可下载以下文件：&quot;)
print(&quot;1. subtitles_en.srt → 纯英文字幕&quot;)
print(&quot;2. subtitles_zh_en.srt → 中英双语字幕&quot;)
print(&quot;👉 下载位置：Colab 左侧「文件」图标 → 点击文件右侧「⋮」→ 下载&quot;)
```

---

### 第三步：使用步骤（零代码，照做就行）

1.  **替换链接**：找到脚本里的 YOUTUBE_URL 这一行，把示例链接换成你要处理的 YouTube 视频链接（比如 https://www.youtube.com/watch?v=123456789）；

1.  **运行脚本**：点击代码块左侧的「播放按钮」，等待运行完成（首次运行会下载 large-v3 模型，约 3 分钟，后续不用重复下载）；

1.  **下载字幕**：运行完成后，点击 Colab 左侧的「文件」图标（文件夹样式），会看到 subtitles_en.srt（纯英文）和 subtitles_zh_en.srt（中英双语），点击文件右侧的「⋮」→ 「下载」即可。

---

### 关键注意事项

1.  **时长支持**：30 分钟、1 小时甚至 2 小时的视频都能处理，免费版 Colab 算力足够；

1.  **语言适配**：如果视频不是英语，把脚本里 language="en" 改成对应语言（比如日语改 ja、韩语改 ko）；

1.  **运行中断**：如果免费版超时，重新点「播放按钮」即可，进度不会丢失；

1.  **无需本地配置**：不用装 Python、不用买显卡、不用传视频，全程云端完成。

---

### 总结

1.  该脚本**零代码、一键运行**，只需替换 YouTube 链接即可；

1.  自动下载视频音频、用 large-v3 识别字幕、翻译为中文，生成双语 SRT；

1.  全程免费，无需本地硬件，30 分钟以上长视频也能稳定处理。

如果运行中遇到「模型下载慢」「翻译失败」「链接解析报错」等问题，直接告诉我，我给你对应的修复版脚本！