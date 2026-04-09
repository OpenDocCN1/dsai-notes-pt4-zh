# 图像生成（Image generation）

> 来源：[https://waytoagi.feishu.cn/docx/AJyKdbEEEoP5lvxMtZ8cNsI9n1c](https://waytoagi.feishu.cn/docx/AJyKdbEEEoP5lvxMtZ8cNsI9n1c)

了解如何使用我们的 DALL·E 模型生成或处理图像

## 介绍

图片 API 提供了三种与图片交互的方法：

1.  根据文本提示从头开始创建图像

1.  根据新文本提示创建现有图像的编辑

1.  创建现有图像的变体

本指南涵盖了使用这三个 API 端点的基础知识以及有用的代码[示例。要查看它们的实际效果](https://labs.openai.com/)，请查看我们的DALL·E 预览应用程序。

图片[ API 处](https://help.openai.com/en/articles/6696591)于测试阶段。在此期间，API 和模型将根据您的反馈进行改进。为确保所有用户都能轻松制作原型，默认速率限制为每分钟 50 张图像。如果您想提高速率限制，请查看这篇帮助中心文章。随着我们对使用和容量要求的更多了解，我们将提高默认速率限制。

## 用法

### 几代人

图像[生成](https://platform.openai.com/docs/api-reference/images/create)端点允许您在给定文本提示的情况下创建原始图像。生成的图像的大小可以为 256[x256、51](https://platform.openai.com/docs/api-reference/images/create#images/create-n)2x512 或 1024x1024 像素。较小的尺寸生成速度更快。您可以使用 n参数一次请求 1-10 张图像。

```
response = openai.Image.create(
  prompt=&quot;a white siamese cat&quot;,
  n=1,
  size=&quot;1024x1024&quot;
)
image_url = response[&#x27;data&#x27;][0][&#x27;url&#x27;]
```

描述越详细，您就越有可能获得您或您的最终用户想要的结果。您可以探索 DALL·E 预览应用程序中的示例以获得更多提示灵感。这是一个简单的例子：

使用 response_format参数，每个图像都可以作为 URL 或 Base64 数据返回。URL 将在一小时后过期。

### 编辑

图像[编辑](https://platform.openai.com/docs/api-reference/images/create-edit)端点允许您通过上传蒙版来编辑和扩展图像。遮罩的透明区域指示应编**辑图像的位置，提**示应描述完整的新图像，而不仅仅是擦除区域。此端点可以启用类似我们 DALL·E 预览应用程序中的编辑器的体验。

```
response = openai.Image.create_edit(
  image=open(&quot;sunlit_lounge.png&quot;, &quot;rb&quot;),
  mask=open(&quot;mask.png&quot;, &quot;rb&quot;),
  prompt=&quot;A sunlit indoor lounge area with a pool containing a flamingo&quot;,
  n=1,
  size=&quot;1024x1024&quot;
)
image_url = response[&#x27;data&#x27;][0][&#x27;url&#x27;]
```

提示：一个阳光充足的室内休息区，带有一个包含火烈鸟的游泳池

上传的图片和遮罩必须是小于 4MB 的正方形 PNG 图片，并且必须具有相同的尺寸。生成输出时不使用遮罩的非透明区域，因此它们不一定需要像上面的示例那样与原始图像匹配。

### 变化

图像[变体](https://platform.openai.com/docs/api-reference/images/create-variation)端点允许您生成给定图像的变体。

```
response = openai.Image.create_variation(
  image=open(&quot;corgi_and_cat_paw.png&quot;, &quot;rb&quot;),
  n=1,
  size=&quot;1024x1024&quot;
)
image_url = response[&#x27;data&#x27;][0][&#x27;url&#x27;]
```

与编辑端点类似，输入图像必须是大小小于 4MB 的方形 PNG 图像。

### 内容审核

提示和图像根据我们的[内容政策](https://labs.openai.com/policies/content-policy)进行过滤，当提示或图像被标记时返回错误。如果您[对误报或](https://help.openai.com/)相关问题有任何反馈，请通过我们的帮助中心联系我们。

## 特定语言提示

### 使用内存图像数据

上面指南中的 Node.js 示例使用该fs模块从磁盘读取图像数据。在某些情况下，您可能会将图像数据保存在内存中。下面是一个使用存储在 Node.jsBuffer对象中的图像数据的 API 调用示例：

```
// This is the Buffer object that contains your image data
const buffer = [your image data];
// Set a `name` that ends with .png so that the API knows it&#x27;s a PNG image
buffer.name = &quot;image.png&quot;;
const response = await openai.createImageVariation(
  buffer,
  1,
  &quot;1024x1024&quot;
);
```

### 使用打字稿

如果您使用的是 TypeScript，您可能会遇到一些图像文件参数的问题。下面是通过显式转换参数来解决类型不匹配的示例：

```
// Cast the ReadStream to `any` to appease the TypeScript compiler
const response = await openai.createImageVariation(
  fs.createReadStream(&quot;image.png&quot;) as any,
  1,
  &quot;1024x1024&quot;
);
```

这是内存中图像数据的类似示例：

```
// This is the Buffer object that contains your image data
const buffer: Buffer = [your image data];
// Cast the buffer to `any` so that we can set the `name` property
const file: any = buffer;
// Set a `name` that ends with .png so that the API knows it&#x27;s a PNG image
file.name = &quot;image.png&quot;;
const response = await openai.createImageVariation(
  file,
  1,
  &quot;1024x1024&quot;
);
```

### 错误处理

API 请求可能会由于无效输入、速率限制或其他问题而返回错误。这些错误可以用一条语句来处理，错误的详细信息可以在 ortry...catch中找到：error.response``error.message

```
try {
  const response = await openai.createImageVariation(
    fs.createReadStream(&quot;image.png&quot;),
    1,
    &quot;1024x1024&quot;
  );
  console.log(response.data.data[0].url);
} catch (error) {
  if (error.response) {
    console.log(error.response.status);
    console.log(error.response.data);
  } else {
    console.log(error.message);
  }
}
```