# 个人玩家使用文本生成LLM

> 来源：[https://seigy6zzam.feishu.cn/docx/VyoXd0uquoCyF7xgb74cYuqbnJf](https://seigy6zzam.feishu.cn/docx/VyoXd0uquoCyF7xgb74cYuqbnJf)

# 角色扮演

## 聊天界面

### agn-ai

+   地址：https://github.com/luminai-companion/agn-ai

+   优点：界面比较简洁，移动端支持好，**支持更多的后端服务**（兼容TavernAI模型服务，可以平替）

+   缺点：没[有](https://www.bilibili.com/read/cv23387641)太多花哨的功能，没有内置角色，需要自己找或者自己捏，没有一键包，需要自己搭建环境，具体如何搭建可以参考图文教程：[Agn-AI]用Agn-AI与AI老婆对话(调用openai/claude/text-generation-webui/kob

![](img/2be807df6c8d63797d859dffff70a82b.png)

![](img/ef2a01e13f72231369d18d95118b0d4e.png)

### SillyTavern

+   地址：https://github.com/SillyTavern/SillyTavern

+   优点：对于TavernAI的升级，移动端使用体验比较好，提供了插件扩展的能力，内置的一些插件在对话场景下非常实用而且效果也不错

+   缺点：因为还比较早期，中文相关的资料比较少，安装比[较繁琐，没有一](https://docs.sillytavern.app/installation/windows/)键安装包，具体安装步骤可以参考官方的文档，根据自己的系统选择

![](img/52bce362b467c147033f97251f0bb5d9.png)

![](img/ef39174efe38d6d7f6ce31116b337ca7.png)

### TavernAI

+   地址：https://github.com/TavernAI/TavernAI

+   优点：内置角色丰富，PC端的体验良好，可以换主题，背**景，通过配置模型服务**[地址对接LLM服](https://github.com/KoboldAI/KoboldAI-Client)务[，提供一键整合](https://novelai.net/)包[的下载，解压就可以](https://rentry.org/pygmalion-ai)用[，支持模型丰富](https://chat.openai.com/)，[包括 Ko](https://openai.com/research/gpt-4)boldAI，NovelAI，Pygmalion，chatGPT，GPT-4

+   缺点：移动端适配比较差

![](img/50c43a57f8975bb3c305f36ae959bc82.png)

![](img/d761a34d76eaee369c8c32a8dad525ce.png)

### RisuAI

+   地址：https://github.com/kwaroran/RisuAI

+   优点：安装便捷，常规一键安装就可以；内置角色市场，点击直接下载使用，角色比较丰富；支持角色情绪图像，可以通过SD根据聊天对话生成图像

+   缺点：没有移动端

![](img/2dc059923f5355770b9cc62316975521.png)

![](img/35382b507f0dfc9d54193e1366392808.png)

## 角色卡

### 角色社区

这个就不做特别[的介绍了，](https://github.com/TavernAI/TavernAI/blob/main/faq.md#q-how-can-i-download-pre-made-characters-to-chat-with)大家可以优先使用TavernAI内置的角色卡，也可以看一些三方的社区（质量参差不齐，还有比较多比较差劲的内容，建议大家自己定制角色或者使用TavernAI内置角色）

### 自定义角色

简单实用的角色编辑器

+   https://zoltanai.github.io/character-editor/

![](img/f1330f71ac6c49bb2f18052cf3591165.png)

+   简单用法：制作角色卡，注意人设信息在导出的时候会存在图片里面，别担心丢了，反而传播更方便了

| 1.  选择右边的new character | 1.  自定义人设 | 1.  上传角色的头像 | 1.  点击导出，左侧下载角色卡 |
| --- | --- | --- | --- |
| ![](img/269a2c8f28d8a8389fb28fe20d3f13c6.png) | ![](img/1373e532a37932e449f953a077416432.png) | ![](img/019c5c5138796d47b993f8c592004961.png) | ![](img/d3b574a647e3428cdde9ac75c404a55f.png) |

+   如何制作更好的角色卡

+   完整使用演示：[书写自己的AI老婆角色(AI对话/text-genera](https://www.bilibili.com/read/cv23877210)tion-webui/Tavernai)

+   制作建议：

+   https://rentry.org/chai-pygmalion-tips

+   https://rentry.org/pygtips

+   个性描述词参考：https://vndb.org/i39

+   参考例子：

+   https://rentry.co/twszg

+   https://rentry.co/cringeguide

## 模型服务

屏蔽掉大语言模型之间的差异，提供统一的集成接口服务

+   text-generation-webui：https://github.com/oobabooga/text-generation-webui

+   KoboldAI：https://github.com/KoboldAI/KoboldAI-Client

## LLM模型

### pygmalion

[最适合自](https://huggingface.co/PygmalionAI)定义[角色的模](https://docs.pygmalion.chat/quickstart/)型，官方提供了多种尺寸，具体可以根据自己的电脑配置选择，你甚至可以在手机上跑模型（没有试过，手机上能跑的模型尺寸比较小，效果肯定会有一定打折，还不如电脑或者服务器上自己部署一个，然后通过手机浏览器访问），具体使用方法参考官方文档

+   兼顾电脑适用性和效果，比较好的是 6b 的版本，具体使用更推荐量化版本：

+   英文原版：

+   量化版本：https://huggingface.co/mayaeary/pygmalion-6b_dev-4bit-128g

+   官方版本：https://huggingface.co/PygmalionAI/pygmalion-6b

+   中文融合版本：

+   量化基础版本：https://huggingface.co/coyude/Chinese-Pygmalion-7b-GPTQ

+   量化增强版本：https://huggingface.co/coyude/Chinese-plus-Pygmalion-7b-GPTQ

+   官方说明文档：https://docs.pygmalion.chat/

### **Nous-Hermes**

NousResearc[h/Nous-Hermes-13b 模型自称在各种](https://www.bilibili.com/read/cv24276795)任务中表现不亚于GPT-3.5-turbo，长回复、低幻觉率和缺乏OpenAI审查机制的优点。 有人融合了chinese-alpaca-13b的lora和Nous-Hermes-13b，更详细的介绍参考 [AI对话]模型推荐Nous-Hermes-13b+chinese-alpaca-lora

+   融合模型（无量化）：https://huggingface.co/Bandifishing/Nous-Hermes-13b-Chinese （要加载的话建议启动bitsandbyts的load-in-4bit或load-in-8bit功能（懒人包的启动程序的设置页面有选项），不然会爆显存）

+   GGML版本：https://huggingface.co/coyude/Nous-Hermes-13b-Chinese-GGML (适合在无独立显卡/A卡/显存不足，可以使用显卡加速)

+   GPTQ版本：

+   https://huggingface.co/coyude/Nous-Hermes-13b-Chinese-GPTQ

+   https://huggingface.co/coyude/Nous-Hermes-13b-Chinese-plus-GPTQ

## 公司服务（模型服务+LLM模型）

### Novel AI

+   https://novelai.net/

![](img/9a42be84feb2730be7d9212f2a10a979.png)

![](img/45914c4952431194d33493a9cb42799b.png)

# 个性写作

# 文字任务