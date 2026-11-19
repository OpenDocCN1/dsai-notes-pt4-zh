
# CogView-3 图像生成

> 来源：[https://v11enp9ok1h.feishu.cn/docx/EtxbdiyXNoHOSlxj6KTcRTuxnW5](https://v11enp9ok1h.feishu.cn/docx/EtxbdiyXNoHOSlxj6KTcRTuxnW5)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

模型编码：`cogview-3`

根据用户的文字描述生成图像，使用同步调用方式请求接口

CogView 根据文本提示生成图像。CogView 生成图像的尺寸为 1024x1024 像素。

模型编码：`cogview-3-plus`

提升版本 `CogView-3-Plus` 具备与当前最优的 `MJ-V6` 和 `FLUX` 等模型接近的性能，调用方式相同。

# CogView-3 Cookbook 调用教程

## 接口介绍

### 接口信息

| 传输方式 | https |

| --- | --- |

| 请求地址 | `https://open.bigmodel.cn/api/paas/v4/images/generations` |

| 调用方式 | 同步调用，等待模型执行完成并返回最终结果 |

| 字符编码 | UTF-8 |

| 接口请求格式 | JSON |

| 响应格式 | JSON |

| 接口请求类型 | POST |

| 开发语言 | 任意可发起 http 请求的开发语言 |

### 接口请求参数

| `参数名称` | `类型` | `是否必填` | `参数说明` |

| --- | --- | --- | --- |

| `model` | `String` | 是 | 模型名称 |

| `prompt` | `String` | 是 | 所需图像的文本描述 |

### 接口响应参数

| `参数名称` | `类型` | `参数说明` |

| --- | --- | --- |

| `created` | `String` | 请求创建时间，是以秒为单位的 Unix 时间戳。 |

| `data` | `List[String]` | 数组，包含生成的图片 URL。 |

| `url` | `String` | 图片链接 |

## 快速使用

您可以使用以下 Python 代码快速调用 `cogview` 能力。

```
from zhipuai import ZhipuAI
client = ZhipuAI(api_key="")
 
response = client.images.generations(
    model="cogview-3",
    prompt=" 一个城市在水晶瓶中欢快生活的场景，水彩画风格，展现出微观与珠宝般的美丽。",
)
print(response.data[0].url)
```

## 提示词优化

`策略：使用结构化的提示语`

在使用图像生成工具时，应采用精确、具体的视觉描述而非抽象概念。明确、清晰的结构化提示词可以帮助 CogView 创造出更高质量的图像。

+ 主体：人、动物、建筑、物体等

+ 媒介：照片、绘画、插图、雕塑、涂鸦等

+ 环境：竹林、荷塘、沙漠、月球上、水下等

+ 光线：自然光、体积光、霓虹灯、工作室灯等

+ 颜色：单色、复色、彩虹色、柔和色等

+ 情绪：开心、生气、悲伤、惊讶等

+ 构图/角度：肖像、特写、侧脸图、航拍图
