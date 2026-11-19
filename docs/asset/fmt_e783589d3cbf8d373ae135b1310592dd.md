
# CogVLM2 技术文档

> 来源：[https://ai.feishu.cn/docx/SWUJdrYIXoyHUExeNVpcB2AAnSc](https://ai.feishu.cn/docx/SWUJdrYIXoyHUExeNVpcB2AAnSc)

CogVLM2 官方仓库：`https://github.com/THUDM/CogVLM2`

国内体验链接：

| 在线体验链接 | 📙 Official Page | 📙[ Official Pag](https://modelscope.cn/studios/ZhipuAI/Cogvlm2-llama3-chinese-chat-Demo/summary)e 🤖 ModelScope | [🤗 Huggingface ](https://huggingface.co/THUDM/cogvlm2-video-llama3-chat) 🤖[ ModelScope](https://modelscope.cn/models/ZhipuAI/cogvlm2-video-llama3-chat) | [🤗 Huggingface ](https://huggingface.co/THUDM/cogvlm2-video-llama3-base) 🤖[ ModelScope](https://modelscope.cn/models/ZhipuAI/cogvlm2-video-llama3-base) |

| --- | --- | --- | --- | --- |

> 任何相关技术问题，请到github issue提问

## 模型信息

目前已经开源的部分模型信息：

| 模型名称 | `cogvlm2-llama3-chat-19B` | `cogvlm2-llama3-chinese-chat-19B` | `cogvlm2-video-llama3-chat` | `cogvlm2-video-llama3-base` |

| --- | --- | --- | --- | --- |

| 基座模型 | `Meta-Llama-3-8B-Instruct` | `Meta-Llama-3-8B-Instruct` | `Meta-Llama-3-8B-Instruct` | `Meta-Llama-3-8B-Instruct` |

| 语言 | 英文 | 中文、英文 | 英文 | 英文 |

| 任务 | 图像理解，多轮对话模型 | 图像理解，多轮对话模型 | 视频理解，单轮对话模型 | 视频理解，基座模型，不可对话 |

| 模型链接 | [🤗 Huggingface ](https://huggingface.co/THUDM/cogvlm2-llama3-chat-19B) 🤖[ ModelScope  ](https://modelscope.cn/models/ZhipuAI/cogvlm2-llama3-chat-19B/)💫 [Wise Model](https://wisemodel.cn/models/ZhipuAI/cogvlm2-llama3-chat-19B/) | [🤗 Huggingface ](https://huggingface.co/THUDM/cogvlm2-llama3-chinese-chat-19B)🤖[ ModelScope  ](https://modelscope.cn/models/ZhipuAI/cogvlm2-llama3-chinese-chat-19B/)💫 [Wise Model](https://wisemodel.cn/models/ZhipuAI/cogvlm2-llama3-chinese-chat-19B) | [🤗 Huggingface ](https://huggingface.co/THUDM/cogvlm2-video-llama3-chat) 🤖[ ModelScope](https://modelscope.cn/models/ZhipuAI/cogvlm2-video-llama3-chat) | [🤗 Huggingface ](https://huggingface.co/THUDM/cogvlm2-video-llama3-base) 🤖[ ModelScope](https://modelscope.cn/models/ZhipuAI/cogvlm2-video-llama3-base) |

| Experience Link | 📙 Official Page | 📙[ Official Pag](https://modelscope.cn/studios/ZhipuAI/Cogvlm2-llama3-chinese-chat-Demo/summary)e 🤖 ModelScope | 📙 Of[ficial Page  ](https://modelscope.cn/studios/ZhipuAI/Cogvlm2-Video-Llama3-Chat-Demo)  🤖 ModelScope | / |

| Int4 模型 | [🤗 Huggingface ](https://huggingface.co/THUDM/cogvlm2-llama3-chat-19B-int4) 🤖[ ModelScope  ](https://modelscope.cn/models/ZhipuAI/cogvlm2-llama3-chat-19B-int4)     💫 [Wise Model](https://wisemodel.cn/models/ZhipuAI/cogvlm2-llama3-chat-19B-int4/) | [🤗 Huggingface ](https://huggingface.co/THUDM/cogvlm2-llama3-chinese-chat-19B-int4)🤖[ ModelScope 💫](https://modelscope.cn/models/ZhipuAI/cogvlm2-llama3-chinese-chat-19B) [Wise Model](https://wisemodel.cn/models/ZhipuAI/cogvlm2-llama3-chinse-chat-19B-int4/) | / | / |

| 文本长度 | 8K | 8K | 2K | 2K |

| 图片分辨率 | 1344 * 1344 | 1344 * 1344 | 224 * 224 (视频,取前24帧) | 224 * 224 (视频,取平均24帧) |

---

## 🏠 👸号

![](img/706c64ae2af5e939b3f17838874b869d.png)

## ❤️ 交流群

![](img/cc4bf983c18e73a822206094f5d485a7.png)
