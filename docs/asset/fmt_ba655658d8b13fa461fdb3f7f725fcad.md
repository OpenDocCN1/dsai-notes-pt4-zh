
# AI-102 实验

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/Rc9wdcqU2oCNZQxsZjmcEyU0ntf](https://zsyhjtnsa5.feishu.cn/docx/Rc9wdcqU2oCNZQxsZjmcEyU0ntf)

## AI 概览

### 分析图片

https://learn.microsoft.com/zh-cn/rest/api/computervision/analyze-image/analyze-image?view=rest-computervision-v3.2&tabs=HTTP#code-try-0

![](img/81437b59257ac4c6c8f4fef5a2c10885.png)

素材

https://upload.wikimedia.org/wikipedia/commons/b/bb/Asianelephant3.jpg

增加参数

```
visualFeatures 
Categories,Adult,Tags,Description,Faces,Color,ImageType,Objects,Brands
```

输出

```
{
  &quot;categories&quot;: [],
  &quot;adult&quot;: {
    &quot;isAdultContent&quot;: false,
    &quot;isRacyContent&quot;: false,
    &quot;isGoryContent&quot;: false,
    &quot;adultScore&quot;: 0.01300809346139431,
    &quot;racyScore&quot;: 0.0640704333782196,
    &quot;goreScore&quot;: 0.00121571053750813
  },
  &quot;color&quot;: {
    &quot;dominantColorForeground&quot;: &quot;White&quot;,
    &quot;dominantColorBackground&quot;: &quot;Brown&quot;,
    &quot;dominantColors&quot;: [
      &quot;Brown&quot;,
      &quot;White&quot;
    ],
    &quot;accentColor&quot;: &quot;958936&quot;,
    &quot;isBwImg&quot;: false,
    &quot;isBWImg&quot;: false
  },
  &quot;imageType&quot;: {
    &quot;clipArtType&quot;: 0,
    &quot;lineDrawingType&quot;: 0
  },
  &quot;tags&quot;: [
    {
      &quot;name&quot;: &quot;animal&quot;,
      &quot;confidence&quot;: 0.999893307685852
    },
    {
      &quot;name&quot;: &quot;mammal&quot;,
      &quot;confidence&quot;: 0.9998044371604919
    },
    {
      &quot;name&quot;: &quot;elephant&quot;,
      &quot;confidence&quot;: 0.9983136653900146
    },
    {
      &quot;name&quot;: &quot;outdoor&quot;,
      &quot;confidence&quot;: 0.9963775277137756
    },
    {
      &quot;name&quot;: &quot;tree&quot;,
      &quot;confidence&quot;: 0.9856871366500854
    },
    {
      &quot;name&quot;: &quot;elephants and mammoths&quot;,
      &quot;confidence&quot;: 0.9711592197418213
    },
    {
      &quot;name&quot;: &quot;asian elephant&quot;,
      &quot;confidence&quot;: 0.968725323677063
    },
    {
      &quot;name&quot;: &quot;indian elephant&quot;,
      &quot;confidence&quot;: 0.9646812081336975
    },
    {
      &quot;name&quot;: &quot;sky&quot;,
      &quot;confidence&quot;: 0.9505905508995056
    },
    {
      &quot;name&quot;: &quot;grass&quot;,
      &quot;confidence&quot;: 0.9481005072593689
    },
    {
      &quot;name&quot;: &quot;plant&quot;,
      &quot;confidence&quot;: 0.944722056388855
    },
    {
      &quot;name&quot;: &quot;terrestrial animal&quot;,
      &quot;confidence&quot;: 0.9430838823318481
    },
    {
      &quot;name&quot;: &quot;zoo&quot;,
      &quot;confidence&quot;: 0.9215219616889954
    },
    {
      &quot;name&quot;: &quot;african elephant&quot;,
      &quot;confidence&quot;: 0.9079951643943787
    },
    {
      &quot;name&quot;: &quot;tusk&quot;,
      &quot;confidence&quot;: 0.8962415456771851
    },
    {
      &quot;name&quot;: &quot;wildlife&quot;,
      &quot;confidence&quot;: 0.8827495574951172
    },
    {
      &quot;name&quot;: &quot;standing&quot;,
      &quot;confidence&quot;: 0.8641688823699951
    },
    {
      &quot;name&quot;: &quot;ground&quot;,
      &quot;confidence&quot;: 0.6812244653701782
    }
  ],
  &quot;description&quot;: {
    &quot;tags&quot;: [
      &quot;tree&quot;,
      &quot;outdoor&quot;,
      &quot;elephant&quot;,
      &quot;grass&quot;,
      &quot;standing&quot;,
      &quot;enclosure&quot;,
      &quot;zoo&quot;,
      &quot;dirt&quot;
    ],
    &quot;captions&quot;: [
      {
        &quot;text&quot;: &quot;an elephant walking in a zoo&quot;,
        &quot;confidence&quot;: 0.39302825927734375
      }
    ]
  },
  &quot;faces&quot;: [],
  &quot;objects&quot;: [
    {
      &quot;rectangle&quot;: {
        &quot;x&quot;: 813,
        &quot;y&quot;: 41,
        &quot;w&quot;: 162,
        &quot;h&quot;: 346
      },
      &quot;object&quot;: &quot;tree&quot;,
      &quot;confidence&quot;: 0.519,
      &quot;parent&quot;: {
        &quot;object&quot;: &quot;plant&quot;,
        &quot;confidence&quot;: 0.52
      }
    },
    {
      &quot;rectangle&quot;: {
        &quot;x&quot;: 429,
        &quot;y&quot;: 200,
        &quot;w&quot;: 494,
        &quot;h&quot;: 423
      },
      &quot;object&quot;: &quot;elephant&quot;,
      &quot;confidence&quot;: 0.813,
      &quot;parent&quot;: {
        &quot;object&quot;: &quot;mammal&quot;,
        &quot;confidence&quot;: 0.93,
        &quot;parent&quot;: {
          &quot;object&quot;: &quot;animal&quot;,
          &quot;confidence&quot;: 0.932
        }
      }
    }
  ],
  &quot;brands&quot;: [],
  &quot;requestId&quot;: &quot;6ec58062-dbed-43f5-b688-09e421e805ac&quot;,
  &quot;metadata&quot;: {
    &quot;height&quot;: 750,
    &quot;width&quot;: 1000,
    &quot;format&quot;: &quot;Jpeg&quot;
  },
  &quot;modelVersion&quot;: &quot;2021-05-01&quot;
}
```

> 这个输出是一个 JSON 格式的图像分析结果，通常由计算机视觉 API（如 Microsoft Azure Computer Vision API）生成。以下是对各个字段的解释：

1. `categories`

> 当前为空数组，表示系统没有为这张图片归类到任何预定义的类别中。

1. `adult`

> 包含图片是否包含成人内容的判断：

> `isAdultContent`： false（非成人内容）

> `isRacyContent`： false（非色情内容）

> `isGoryContent`： false（非暴力血腥内容）

> 对应的分数（值越低越安全）：

> `adultScore`： 0.013（成人内容可能性极低）

> `racyScore`： 0.064（色情内容可能性低）

> `goreScore`： 0.001（暴力血腥内容可能性极低）

1. `color`

> 图片的颜色分析：

> 前景主色：White（白色）

> 背景主色：Brown（棕色）

> 所有主色：["Brown", "White"]（棕色和白色）

> 强调色：958936（十六进制颜色码，一种土黄色）

> `isBwImg`： false（非黑白图片）

1. `imageType`

> 图片类型判断：

> `clipArtType`： 0（非剪贴画）

> `lineDrawingType`： 0（非线描图）

1. `tags`

> 系统识别的标签列表，按置信度排序：

> 高置信度标签：animal（动物）、mammal（哺乳动物）、elephant（大象）、outdoor（户外）、tree（树）等。

> 其他相关标签：grass（草地）、zoo（动物园）、tusk（象牙）等。

1. `description`

> 对图片的文本描述：

> tags： 相关关键词（如tree， elephant， zoo等）。

> captions： 生成的描述文本：

> "an elephant walking in a zoo"（一只大象在动物园里行走），置信度为 0.393。

1. `faces`

> 为空数组，表示图片中未检测到人脸。

1. `objects`

> 检测到的具体对象及其位置：

> **树**：

> 位置：矩形框（`x`=813, `y`=41, 宽=162, 高=346）

> 置信度：0.519

> 父对象：plant（植物，置信度 0.52）

> **大象**：

> 位置：矩形框（`x`=429, `y`=200, 宽=494, 高=423）

> 置信度：0.813

> 父对象层级：mammal（哺乳动物）→ animal（动物）。

1. `brands`

> 为空数组，表示未检测到品牌标志。

1. `metadata`

+ 图片元数据：

+ 高度：750像素

+ 宽度：1000像素

+ 格式：Jpeg

1. `modelVersion`

+ 使用的模型版本：2021-05-01

> **总结**

> 这张图片展示了一只大象（可能是亚洲象或非洲象）在动物园的户外环境中站立或行走，背景有树木和草地。图片无成人或暴力内容，主色调为棕色和白色，且未检测到人脸或品牌。分析结果由 2021 年 5 月版本的模型生成。

### 使用 REST 和 SDK 调用 Azure AI Service

https://microsoftlearning.github.io/mslearn-ai-services/Instructions/Exercises/01-use-azure-ai-services.html

`.env`

```
AI_SERVICE_ENDPOINT=YOUR_AI_SERVICES_ENDPOINT
AI_SERVICE_KEY=YOUR_AI_SERVICES_KEY
```

## 计算机视觉

### 图像分析

https://microsoftlearning.github.io/mslearn-ai-vision/Instructions/Exercises/01-analyze-images.html
