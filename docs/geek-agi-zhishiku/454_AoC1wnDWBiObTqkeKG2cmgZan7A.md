# 最强声音克隆TTS项目，再也无法分辨是人还是AI在说话，因为它会笑和停顿

> 来源：[https://geekbang.feishu.cn/docx/MDBmdv85votFcGxzq6kc6aRUnae](https://geekbang.feishu.cn/docx/MDBmdv85votFcGxzq6kc6aRUnae)

![](img/5867f9e5c252bc17c5ca31a0fea2aaf8.png)

**板块主理人：尼福财**

**·向量光年科技有限公司创始人**

·《开源AI项目落地》👸号及社群主理人

专注于AI产品开发落地，移动端、PC端软件快速高质量交付

愿景：加速全行业向AI转变

![](img/0e50446456340c9989962820775b98bf.png)

**项目简介**

ChatTTS是一个针对日常对话场景设计的生成式语音合成模型。该模型支持英语和中文，适用于各类语音互动应用，如智能助手。ChatTTS不仅能够控制发音的细节，如笑声、停顿和插入语，还优于大多数开源语音合成模型，尤其是在语调上。此外，ChatTTS采用了40,000小时的预训练数据进行开发，虽然为非商业研究和教育目的，但已在模型中添加了一些特别措施，以防止其被滥用。项目旨在支持进一步的研究与开发，为语音合成领域提供强有力的工具。

**扫码加入AI交流群**

**获得更多技术支持和交流**

**（请注明自己的职业）**

![](img/c051487854831fcbf5e25cad414d7028.png)

**DEMO**

```
chat T T S is a text to speech model designed for dialogue applications. [uv_break]it supports mixed language input [uv_break]and offers multi speaker capabilities with precise control over prosodic elements [laugh]like like [uv_break]laughter[laugh], [uv_break]pauses, [uv_break]and intonation. [uv_break]it delivers natural and expressive speech,[uv_break]so please[uv_break] use the project responsibly at your own risk.[uv_break]
```

**亮点**

**1. 对话式文本转语音： **ChatTTS 针对基于对话的任务进行了优化，能够实现自然且富有表现力的语音合成。它支持多个发言者，便于进行互动对话。

**2. 细粒度控制： **该模型能够预测并控制细粒度韵律特征，包括笑声、停顿和插入语。

**3. 更好的韵律： **在语调方面，ChatTTS 超越了大多数开源文本转语音模型。提供预训练模型以支持进一步的研究和开发。

**使用**

**1.基础使用**

```
import ChatTTSfrom IPython.display import Audio
chat = ChatTTS.Chat()chat.load_models(compile=False) # Set to True for better performance
texts = [&quot;PUT YOUR TEXT HERE&quot;,]
wavs = chat.infer(texts, )
torchaudio.save(&quot;output1.wav&quot;, torch.from_numpy(wavs[0]), 24000)
```

**2.进阶使用**

```
#################################### Sample a speaker from Gaussian.
rand_spk = chat.sample_random_speaker()
params_infer_code = {  &#x27;spk_emb&#x27;: rand_spk, # add sampled speaker   &#x27;temperature&#x27;: .3, # using custom temperature  &#x27;top_P&#x27;: 0.7, # top P decode  &#x27;top_K&#x27;: 20, # top K decode}
#################################### For sentence level manual control.
# use oral_(0-9), laugh_(0-2), break_(0-7) # to generate special token in text to synthesize.params_refine_text = {  &#x27;prompt&#x27;: &#x27;[oral_2][laugh_0][break_6]&#x27;} 
wav = chat.infer(texts, params_refine_text=params_refine_text, params_infer_code=params_infer_code)
#################################### For word level manual control.text = &#x27;What is [uv_break]your favorite english food?[laugh][lbreak]&#x27;wav = chat.infer(text, skip_refine_text=True, params_refine_text=params_refine_text,  params_infer_code=params_infer_code)torchaudio.save(&quot;output2.wav&quot;, torch.from_numpy(wavs[0]), 24000)
```

**常见问题**

1.对于30秒的音频片段，至少需要4GB的GPU内存。对于4090 GPU，它可以每秒生成大约7个语义标记的音频。RTF约为0.3。

2.模型稳定性不够好，存在诸如多发言者或音质不佳的问题。这通常是自回归模型（例如 bark 和 valle）的一个问题，通常很难避免。可以尝试多个样本以找到合适的结果。

3.除了笑声，还能控制什么，能控制其他情绪吗？在当前发布的模型中，唯一的标记级控制单元是 [笑声]、[无声间断] 和 [长间断]。在未来版本中，可能会开源具有额外情绪控制能力的模型。

**项目链接**

https://github.com/2noise/ChatTTS

关注「 开源AI项目落地 」👸号

与AI时代更靠近一点