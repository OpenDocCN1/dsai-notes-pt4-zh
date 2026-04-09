# GPT-SOVITS（当前性价比最高方案）

> 来源：[https://seigy6zzam.feishu.cn/docx/BdvBdZFG9oEneTx7GaBcwlZtnSb](https://seigy6zzam.feishu.cn/docx/BdvBdZFG9oEneTx7GaBcwlZtnSb)

# 官方材料

+   [花儿不哭](https://space.bilibili.com/5760446/)介绍视频

+   官方训练介绍

+   训练推理整合包网盘下载地址：[度盘](https://pan.baidu.com/s/1OE5qL0KreO-ASHwm6Zl9gA?pwd=mqpi) 和 [123盘](https://www.123pan.com/s/5tIqVv-GVRcv.html)

+   开源项目地址：[原github地址](https://github.com/RVC-Boss/GPT-SoVITS) 和 [镜像地址](https://bgithub.xyz/RVC-Boss/GPT-SoVITS)

+   [云端训练](https://www.codewithgpu.com/i/RVC-Boss/GPT-SoVITS/GPT-SoVITS-Official)

# 推理服务

## WebTTS

[WebTTS](https://yxi3w0wmgv2.feishu.cn/wiki/JilfwWyNRiTWlkkheeUck40qnaf)

## TTS for GPT-soVITS：一个推理特化的项目

不做搬运，作者原文档

> [注意](https://www.yuque.com/xter/zibxlp/sz10r2g30y6yexkh)：推理[特化版本有一](https://github.com/X-T-E-R/TTS-for-GPT-soVITS)些模型放置的约束，具体参考这里，相关开源代码地址。简单来说，所有角色模型放在trained文件夹下面，用角色名称命名文件夹，文件夹一般包含三个文件

+   **pth文件：**SoVITS_weights权重文件

+   **ckpt文件：**GPT_weights权重文件

+   **wav文件**（若干）：参考音频文件（作用是让生成出来的音频像这段参考音频语气）

+   【可选】infer_config.json文件：指定情绪参数的模型配置信息

> 另外可以通过 character_info.json 中 default_character 指定默认推理角色

## SRT for GPT-soVITS：通过SRT文件生成时间戳音频

[开源代码地址](https://github.com/YYuX-1145/Srt-AI-Voice-Assistant)，详细教程参考

# 模型分享

> 注意下面文档中关于模型分享的说明

+   [**模型分享：解压即用的模型**](https://www.yuque.com/xter/zibxlp/gsximn7ditzgispg)

+   模型分享：不能直接用，要加参考音频的

+   [**有关于模型定制/训练**](https://www.yuque.com/xter/zibxlp/exuh1p32hzee1ilz)

# 各种Demo

+   [gpt_sovits_demo](https://hf-mirror.com/spaces/kevinwang676/gpt_sovits_demo)

+   [gpt_sovits_jay](https://hf-mirror.com/spaces/kevinwang676/gpt_sovits_jay)：https://kevinwang676-gpt-sovits-jay.hf.space/

+   支持中日英三语自动标注及训练 + 中文方言训练