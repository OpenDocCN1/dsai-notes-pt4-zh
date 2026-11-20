
## Griffin-Lim 算法

原始的音频很难提取特征，需要进行傅里叶变换将时域信号转换到频域进行分析。音频进行傅里叶变换后，结果为复数，复数的绝对值就是幅度谱，而复数的实部与虚部之间形成的角度就是相位谱。经过傅里叶变换之后获得的幅度谱特征明显，可以清楚看到基频和对应的谐波。基频一般是声带的频率，而谐波则是声音经过声道、口腔、鼻腔等器官后产生的共振频率，且频率是基频的整数倍。

Griffin-Lim 将幅度谱恢复为原始波形，但是相比原始波形，幅度谱缺失了原始相位谱信息。音频一般采用的是短时傅里叶变化，因此需要将音频分割成帧（每帧 20ms～50ms），再进行傅里叶变换，帧与帧之间是有重叠的。Griffin-Lim 算法利用两帧之间有重叠部分的这个约束重构信号，因此如果使用 Griffin-Lim 算法还原音频信号，就需要尽量保证两帧之间重叠越多越好，一般帧移为每一帧长度的 25%左右，也就是帧之间重叠 75%为宜。

Griffin-Lim 在已知幅度谱，不知道相位谱的情况下重建语音，算法的实现较为简单，整体是一种迭代算法，迭代过程如下：

1. 随机初始化一个相位谱；
2. 用相位谱和已知的幅度谱经过逆短时傅里叶变换（ISTFT）合成新语音；
3. 对合成的语音做短时傅里叶变换，得到新的幅度谱和相位谱；
4. 丢弃新的幅度谱，用相位谱和已知的幅度谱合成语音，如此重复，直至达到设定的迭代轮数。

在迭代过程中，预测序列与真实序列幅度谱之间的距离在不断缩小，类似于 EM 算法。推导过程参见：Griffin Lim 算法的过程和证明和 Griffin Lim 声码器介绍。

### 代码实现

摘抄自[Build End-To-End TTS Tacotron](https://zhuanlan.zhihu.com/p/25002923): Griffin Lim 信号估计算法。

```
def griffin_lim(stftm_matrix, shape, min_iter=20, max_iter=50, delta=20):
    y = np.random.random(shape)
    y_iter = []
    for i in range(max_iter):
        if i >= min_iter and (i - min_iter) % delta == 0:
            y_iter.append((y, i))
        stft_matrix = librosa.core.stft(y)
        stft_matrix = stftm_matrix * stft_matrix / np.abs(stft_matrix)
        y = librosa.core.istft(stft_matrix)
    y_iter.append((y, max_iter))
    return y_iter
```

具体使用：

```
# assume 1 channel wav file
sr, data = scipy.io.wavfile.read(input_wav_path)
# 由 STFT -> STFT magnitude
stftm_matrix = np.abs(librosa.core.stft(data))
# + random 模拟 modification
stftm_matrix_modified = stftm_matrix + np.random.random(stftm_matrix.shape)
# Griffin-Lim 估计音频信号
y_iters = griffin_lim(stftm_matrix_modified, data.shape)
```

## STRAIGHT 声码器

### 概述

STARIGHT（Speech Transformation and Representation using Adaptive Interpolation of weiGHTed spectrum），即利用自适应加权谱内插进行语音转换和表征。STRAIGHT 将语音信号解析成相互独立的频谱参数（谱包络）和基频参数（激励部分），能够对语音信号的基频、时长、增益、语速等参数进行灵活的调整，该模型在分析阶段仅针对语音基音、平滑功率谱和非周期成分 3 个声学参数进行分析提取，在合成阶段利用上述 3 个声学参数进行语音重构。

STRAIGHT 采用源-滤波器表征语音信号，可将语音信号看作激励信号通过时变线性滤波器的结果。

> "对于能量信号和周期信号，其傅里叶变换收敛，因此可以用频谱（Spectrum）来描述；对于随机信号，傅里叶变换不收敛，因此不能用频谱进行描述，而应当使用功率谱（PSD），不严谨地说，功率谱可以看作是随机信号的频谱，参见[功率谱密度（PSD）](https://zhuanlan.zhihu.com/p/417454806)。"

### 特征提取

1. 平滑功率谱的提取，包括低频带补偿和清音帧处理等过程。STRAIGHT 分析阶段的一个关键步骤是进行自适应频谱分析，获取无干扰且平滑的功率谱。自适应加权谱的提取关键在于对提取出来的功率谱进行一系列的平滑和补偿。对输入信号进行：语音信号预处理->功率谱提取->低频噪声补偿->过平滑补偿->静音帧谱图的处理，最后得到自适应功率谱。

2. 非周期成分提取。

3. 通过小波时频分析的方式，提取基频轨迹。首先通过对语音信号中的基频信息进行解析，然后计算出相应的瞬时基频值，最后在频域进行谐波解析，并在频率轴进行平滑处理，获得语音信号的各个基频参数。

### 语音合成

STARIGHT 采用 PSOLA 技术和最小相位脉冲响应相结合的方式，在合成语音时输入待合成语音的基音频率轨迹和去除了周期性的二维短时谱包络。

[开源的 STRAIGHT](https://github.com/ashmanmode/StraightRepo)声码器大多是 [MATLAB 实现](https://github.com/CSTR-Edinburgh/merlin)，如 Legacy STRAIGHT，StraightRepo。在开源语音合成系统 merlin 中存在可用的 STRAIGHT 工具，参见 `StraightCopySynthesis`。

## WORLD 声码器

### 声学特征

WORLD 通过获取三个声学特征合成原始语音，这三个声学特征分别是：基频（fundamental frequency，F0），频谱包络（Spectrum Envelop，也称频谱参数 Spectrum Parameter，SP）和非周期信号参数（Aperiodic Parameter，AP）。

1. 基频 F0

基频 F0 决定浊音，对应激励部分的周期脉冲序列，如果将声学信号分为周期和非周期信号，基频 F0 部分包含了语音的韵律信息和结构信息。对于一个由振动而发出的声音信号，这个信号可以看作是若干组频率不同的正弦波叠加而成，其中频率最低的正弦波即为基频，其它则为泛音。

WORLD 提取基频的流程：首先，利用低通滤波器对原始信号进行滤波；之后，对滤波之后的信号进行评估，由于滤波之后的信号应该恰好是一个正弦波，每个波段的长度应该恰好都是一个周期长度，因此通过计算这四个周期的标准差，可以评估此正弦波正确与否；最后选取标准差最小周期的倒数作为最终的基频。

2. 频谱包络 SP

频谱包络 SP 决定音色，对应声道谐振部分时不变系统的冲激响应，可以看作通过此线性时不变系统之后，声码器会对激励与系统响应进行卷积。将不同频率的振幅最高点通过平滑的曲线连接起来，就是频谱包络，求解方法有多种，在求解梅尔倒谱系数时，使用的是倒谱法。

> "倒频谱（Cepstrum）也称为倒谱、二次谱和对数功率谱等，倒频谱的工程定义为：信号功率谱对数值进行傅里叶逆变换的结果，也就是：信号->求功率谱->求对数->求傅里叶逆变换。参见[信号频域分析方法的理解（频谱、能量谱、功率谱、倒频谱、小波分析）](https://zhuanlan.zhihu.com/p/34989414)。"

3. 非周期信号参数 AP

非周期信号参数 AP 决定清音，对应混合激励部分的非周期脉冲序列，一般的语音都是由周期和非周期信号组成，因此除了上述的周期信号的声学参数，还需要非周期信号参数，才能够恢复出原始信号。混合激励可以通过 AP 来控制浊音段中周期激励和噪音（非周期）成分的相对比重。

### WORLD 的分析功能

WORLD 包含 3 个语音分析模块，语音分析模型包括 DIO 模块、CheapTrick 模块，PLATINUM 模块。

![](img/ab9790fa42adaca743ed152fd9069090.png)

WORLD 声码器整体结构

WORLD 可以提取原始波形中的基频 F0，基频包络 SP 和非周期信号 AP，这三种声学特征对应三种提取算法：DIO 输入波形提取基频，CheapTrick 输入基频、波形提取频谱包络，D4C 输入基频、频谱包络和波形提取非周期信号。最终，通过这三种声学特征通过最小相位谱与激励信号卷积后，输出恢复的原始波形。

### DIO 算法提取基频 F0

F0 是周期信号最长持续时间的倒数，反过来，周期是基频的整数分之一。基频会产生二次谐波、三次谐波等，最长的周期理论上对应着频率最低的部分，也就是在语谱图上对应最下面的亮线，能量最高的部分。

![](img/c59fd9859cb8371d7e6bbf0597587a3e.png)

基频和谐波 vocoder_pitch_harmonic

有很多的算法估计基频 F0，可以分为两类：一个是利用时域特征，比如自相关；一个利用频谱特征，比如倒谱 cepstrum。WORLD 使用 DIO 估计基频 F0，比 YIN、SWIPE 要快，性能依然较好，DIO 分为以下三步。

1. 低通滤波器对原始信号进行滤波。使用不同频带的低通滤波器：因为不知道基频的位置，因此这一步包含不同周期的 sin 低通滤波器。

2. 取 4 个周期计算置信度。计算获得的各个可能基频 F0 的置信度，因为由基频分量组成的 sin 信号包含 4 个间隔（2 个顶点、2 个过零点）。如果滤波器得到的间隔长度一致，则说明是一个基波，如下图所示。

![](img/d2590aafc258a46272a4c0a57d4225c0.png)

![取四个间隔计算候选 F0 及其置信度](image/vocoder_world_dio1.png)

3. 从某个时间点的正弦波中提取出四个周期信号，并计算置信度，也就是标准差。然后选择标准差最低，也就是置信度最高的基波。

### CheapTrick 算法提取频谱包络 SP

声音包含不同频率的信号，覆盖 0 到 18000Hz，每个频率都有其振幅，定义每种频率中波的振幅最高点连线形成的图形为包络。频谱包络是个重要的参数，在频率-振幅图中，用平滑的曲线将所有共振峰连接起来，这个平滑的曲线就是频谱包络。

![](img/5d309136ed4999c8b3c66948de6caf1e.png)

![取四个间隔计算候选 F0 及其置信度](image/vocoder_world_sp.png)

提取频谱包络 SP 的典型算法有线性预测编码（Linear Predictive Coding，LPC）和 Cepstrum。线性预测编码 LPC 的原理是用若干个历史语音采样点的加权线性求和去不断逼近当前的语音采样点；Cepstrum 则是基于复数倒谱拥有频谱幅度与相位信息的原理，通过对一个信号进行快速傅里叶变换 FFT->取绝对值->取对数->相位展开->逆快速傅里叶变换 IFFT 的变换处理，从而得到对应的倒谱图。

WORLD 采用 CheapTrick 做谱分析，思想来自于音高同步分析（pitch synchronous analysis），其过程是：先将不同基频进行自适应加窗操作，以及功率谱平滑操作，随后将信号在频域上进行同态滤波。

### PLANTINUM 提取非周期信号

混合激励和非周期信号参数 AP 经常应用到合成中，在 Legacy-STRAIGHT 和 TANDEM-STRAIGHT 算法中，aperiodicity 被用于合成周期和非周期的信号。WORLD 直接通过 PLANTINUM 从波形、F0 和谱包络中得到混合激励的非周期信号。

### WORLD 的合成算法

TANDEM-STRAIGHT 直接使用周期响应计算声带的振动，而 Legacy-STRAIGHT 则操纵组延迟（group delay）以避免嗡嗡声。在 WORLD 中，利用最小相位响应和激励信号的卷积来计算声带的振动，从下图所示，可以看到，WORLD 的卷积比 STAIGHT 要少，因此计算量更少。

![](img/ac9873e2b5832ce1a3632d170251504e.png)

![WORLD 合成算法](image/vocoder_world_synthesis.png)

### 使用示例

WORLD 声码器有较为成熟的[开源实现](https://github.com/mmorise/World)，并且有对应的 Python 封装：[PyWORLD](https://github.com/JeremyCCHsu/Python-Wrapper-for-World-Vocoder) ([Python wrapper of WORLD Vocoder](http://ml.cs.yamanashi.ac.jp/world/english))，另有官方实现。以下示例包括了通过 PyWorld 提取声学参数，合成原始音频，修改部分声学参数，编辑原始音频。

```
import pyworld as pw
from scipy.io import wavfile
import matplotlib.pyplot as plt
import numpy as np
import os
import soundfile as sf
# 提取语音特征
x, fs = sf.read(WAV_FILE_PATH)
# f0 : ndarray
#     F0 contour. 基频等高线
# sp : ndarray
#     Spectral envelope. 频谱包络
# ap : ndarray
#     Aperiodicity. 非周期性
f0, sp, ap = pw.wav2world(x, fs)    # use default options
# 分别提取声学参数
# 使用DIO算法计算音频的基频F0
_f0, t = pw.dio(x, fs, f0_floor= 50.0, f0_ceil= 600.0, channels_in_octave= 2, frame_period=pw.default_frame_period)
# 使用CheapTrick算法计算音频的频谱包络
_sp = pw.cheaptrick(x, _f0, t, fs)
# 计算aperiodic参数
_ap = pw.d4c(x, _f0, t, fs)
# 基于以上参数合成原始音频
_y = pw.synthesize(_f0, _sp, _ap, fs, pw.default_frame_period)
# 1.变高频-更类似女性
high_freq = pw.synthesize(f0*2.0, sp, ap, fs)
# 2.直接修改基频，变为机器人发声
robot_like_f0 = np.ones_like(f0)*100
robot_like = pw.synthesize(robot_like_f0, sp, ap, fs)
# 3.提高基频，同时频谱包络后移 -> 更温柔的女性？
female_like_sp = np.zeros_like(sp)
for f in range(female_like_sp.shape[1]):
    female_like_sp[:, f] = sp[:, int(f/1.2)]
female_like = pw.synthesize(f0*2, female_like_sp, ap, fs)
# 4.转换基频（不能直接转换）
x2, fs2 = sf.read(WAV_FILE_PATH2)
f02, sp2, ap2 = pw.wav2world(x2, fs2)
f02 = f02[:len(f0)]
print(len(f0),len(f02))
other_like = pw.synthesize(f02, sp, ap, fs)
```

## NHV / NSF

### NSF: Neural Source Filter

> https://zhuanlan.zhihu.com/p/496658550

NSF的核心思想是使用`F0`构建`source`，然后是声码器使用`source`和`mel`共同生成WAV

`f0` + `uv` (由前面的 `fastspeech` 得到，跟 `mel` 谱放在一起预测） 得到的`source`

包含了谐波还有高斯噪声

![](img/37002958b73b82a7559977f62e982e81.png)

推理的时候，对这个 `source` 进行 相对应的 `conv` 下采样，匹配 `mel` 上采样的过程（直接跟 `mel` 谱相加）

基于 `f0` + `mel` 得到的音频

![](img/0d20acbcc4151170d33c90c38a8d9a7d.png)

举个例子

![](img/113a840591eb59f42f3d54bd27f987dc.png)

[sing-ori.wav](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AuF2btFKioLN6ZxrpVOcmFE7n5p)

![](img/2db61d79b7d5996842bd67b77bafa16d.png)

提取`f0`后，将`f0`转为8个谐波还原成的音频（图中没有加 `uv` 所以清音部分也有基频）

![](img/7f34a6409bb588a4de31c8af8e127489.png)

[f0-audio.wav](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GfVzbJzJtokp3TxSkY7ccWmUnCc)

![](img/e021be6f4b5b6f0f328308a7137acb19.png)

### 源滤波器合成原理
