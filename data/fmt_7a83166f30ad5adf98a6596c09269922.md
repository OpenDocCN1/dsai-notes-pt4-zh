
# CLIP模型训练

> 来源：[https://v11enp9ok1h.feishu.cn/docx/FJh2d0HYGoTTqyxDlGScPwzJntf](https://v11enp9ok1h.feishu.cn/docx/FJh2d0HYGoTTqyxDlGScPwzJntf)

## 准备数据集

需要一个文本-图像对数据集，其中每对包含：

![](img/24db1c88443163aa7c5c3a233eb92dc5.png)

+   文字描述（例如，“两只狗在肮脏的道路上奔跑”）

+   其对应的图像（例如，两只狗在肮脏的道路上奔跑的图像）。

这些对被视为`正样本`（相关）。负样本是通过配对数据集中不相关的文本和图像隐式生成的。

## 通过编码器传递输入

文本编码器处理文本描述以生成文本嵌入，图像编码器处理图像以生成图像嵌入。

![](img/58001ee26b28af6d25a2bb3e81633f41.png)

## 跨模态相似性的对比学习

为了对齐文本和图像嵌入，我们应用了对比损失函数。目标是：

+   最大化匹配的（文本、图像）对的嵌入之间的相似性。

+   最小化不匹配的（文本、图像）对的嵌入之间的相似性。

![](img/31829962f6cf45ebb1de91f326059de0.png)

例如，在上图中，对角线上的所有对都对正例有贡献，而所有其他对都对负例有贡献。

最后，使用我们之前讨论过的对比损失函数来训练网络

![](img/34879aca95e6163c00d888de02830acf.png)

+   对于每个正对，损失鼓励嵌入之间的距离较小（接近于 0）。

+   对于每个负对，损失会导致嵌入之间的距离较大。

这就是我们构建 CLIP 模型的方法。

## CLIP 演示

现在我们了解了构建多模态 LLM 的基本细节，让我们看一下预先训练的 CLIP 模型的演示以及如何使用它：

+   文本到图像搜索

+   图像到图像搜索

+   零样本分类

### 设置

首先，从一些安装开始：

```
pip install transformers torch datasets
```

接下来我们将使用这个数据集，它提供了几个图像文本对：

`https://huggingface.co/datasets/jamescalam/image-text-demo?ref=dailydoseofds.com`

按照如下方式下载并使用库加载它`datasets`：

```
from datasets import load_dataset

data = load_dataset(
    "jamescalam/image-text-demo",
    split="train"
)
```

我们可以直接预览图像-文本对，如下所示：

![](img/2c82c8d6028c588665cb6d99d6a95420.png)

现在，让我们使用 Hugging Face 的库来设置 CLIP 模型`transformers`。在本例中，我们将使用 OpenAI 的预训练`clip-vit-base-patch32`模型，这是一个流行的轻量级 CLIP 版本。

如下所示：

```
from transformers import CLIPProcessor, CLIPModel
import torch

model_id = "openai/clip-vit-base-patch32"

processor = CLIPProcessor.from_pretrained(model_id)
model = CLIPModel.from_pretrained(model_id)

# move model to device if possible
device = 'cuda' if torch.cuda.is_available() else 'cpu'

model.to(device)
```

如上所示，我们使用 Huggingface 模型的标准流程，导入必要的组件——`CLIPProcessor`用于预处理和`CLIPModel`加载 CLIP 架构。模型和处理器均使用该`from_pretrained`方法加载。

```
text = data['text']
images = data['image']

inputs = processor(text=text, 
                   images=images,
                   return_tensors="pt",
                   padding=True
                   ).to(device)

>>> inputs.keys()
dict_keys(['input_ids', 'attention_mask', 'pixel_values'])
```

下一步涉及使用`processor`之前定义的方法来准备 CLIP 的输入，以便无缝处理文本和图像数据。

这里，`text`和`images`是从数据集中提取的。这些输入通过以下参数`images`传递给：`processor`

+   `text=text`：指定文本描述，将被标记并转换为文本编码器所需的格式。

+   `images=images`：指定一批图像，这些图像将被填充/调整大小以满足图像编码器的输入要求。

+   `return_tensors="pt"`：确保输出以 PyTorch 张量的形式返回，与 CLIP 模型兼容。

+   `padding=True`：向文本序列添加填充以确保它们在批次内具有一致的长度。

生成的`inputs`对象包含文本和图像编码器的预处理数据，其中包括：

+   `input_ids`：文本输入的标记化表示。

+   `attention_mask`：文本输入的注意掩码，其中0表示填充标记：

![](img/59a14e7bda262df9dcd5c55db2eac5a1.png)

+   `pixel_values`：预处理的图像张量。

下一步是将它们传递给 CLIP 模型以获得输出，如下所示：

```
outputs = model(**inputs)

>>> outputs.keys()
odict_keys(['logits_per_image', 'logits_per_text',
            'text_embeds', 'image_embeds', 
            'text_model_output', 'vision_model_output'])
```

在上面的代码中，`inputs`字典（包含文本和图像张量）被解包并输入到模型中。该模型通过其双编码器处理数据：

+   文本编码器为标记化的文本输入生成嵌入。

+   图像编码器为预处理的图像输入生成嵌入。

生成的`outputs`对象包含嵌入和附加信息，您可以通过调用来探索它们`outputs.keys()`。

与我们相关的输出键是`text_embeds`和`image_embeds`：

```
text_emb = outputs.text_embeds
image_emb = outputs.image_embeds

>>> print(text_emb.shape)      >>> print(image_emb.shape)
torch.Size([21, 512])          torch.Size([21, 512])
```

回到我们之前的讨论，如果我们测量上述两个嵌入矩阵之间的相似性，则该相似性矩阵对角线上的所有对都将表示正例，而所有其他对都将表示负例。

![](img/0871b791dee45a60805061e67ea26ade.png)

让我们在下面测量一下：

![](img/573356c525ff837530ea00b2e9f76b80.png)

接下来，让我们将其绘制为热图：

```
import matplotlib.pyplot as plt

plt.imshow(cos_sim)
plt.show()
```

这将生成以下图表，该图表描绘了沿对角线的高度相似性：

![](img/c7aa19ee9380ce61600983184cf7ed9b.png)

### 文本到图像搜索

由于每个图像和文本在共享嵌入空间中都有相应的嵌入，因此我们可以使用这些嵌入进行检索。

假设我有一个文本提示。我可以使用文本编码器将其嵌入到 CLIP 模型中，并与矢量数据库中所有图像的嵌入进行匹配，然后获取最佳结果。

从逻辑上讲，实现起来相当容易，如下面的代码所示：

```
query_text = "Dog running on grass"

# preprocess text (tokenize, etc.)
inputs = processor(text=[query_text], return_tensors="pt", padding=True).to(device)

# generate text embeddings
text_features = model.get_text_features(**inputs)

# normalize text embedding
text_features = text_features / text_features.norm(dim=-1, keepdim=True)
    
# Calculate similarity scores across all image embddings
similarity = torch.mm(text_features, image_emb.T)
    
# Get top-k matches
values, indices = similarity[0].topk(min(top_k, len(data)))
```

+   使用处理器将文本查询标记化并转换为 PyTorch 张量。

+   然后，将预处理的输入传递给模型的文本编码器以生成文本嵌入（`text_features`）。

+   然后在计算相似度分数之前对嵌入进行标准化。

+   接下来，计算文本嵌入和所有图像嵌入之间的相似度。

+   最后，利用`PyTorch`的`topk`函数，检索相似度得分最高的top-k张图像。

我们可以按如下方式显示这些结果：

```
fig, axes = plt.subplots(1, top_k, figsize=(15, 3))
    
for i, (idx, score) in enumerate(zip(indices, values)):
    # Print text and score
    print(f"{data['text'][idx]}: {score:.3f}")
        
    # Display image
    axes[i].imshow(data['image'][idx])
    axes[i].axis('off')
    axes[i].set_title(f"Score: {score:.3f}")
    
plt.tight_layout()
plt.show()
```

这会产生以下排名前 5 的匹配结果，大部分看起来都很棒。

![](img/1fed6b42d0c834424c4f6b0ac5bb715d.png)

### 图像到图像搜索

构建多模式 RAG 应用程序的很大一部分还涉及图像到图像的相似性，因此接下来，让我们看看如何使用 CLIP 进行图像到图像的搜索。

逻辑步骤将与以前相同：

```
query_image = data['image'][0]

# preprocess image
inputs = processor(images=query_image, return_tensors="pt", padding=True).to(device)

# generate image embeddings
image_features = model.get_image_features(**inputs)

# normalize image embedding
image_features = image_features / image_features.norm(dim=-1, keepdim=True)
    
# Calculate similarity scores across all image embddings
similarity = torch.mm(image_features, image_emb.T)
    
# Get top-k matches
values, indices = similarity[0].topk(min(top_k, len(data)))
```

+   使用处理器对图像进行预处理。

+   然后，将预处理的输入传递到模型的图像编码器以生成图像嵌入（`image_features`）。

+   然后在计算相似度分数之前对嵌入进行标准化。

+   接下来，计算该图像嵌入与所有其他图像嵌入之间的相似度。

+   最后，利用`PyTorch`的`topk`函数，检索相似度得分最高的top-k张图像。

在这种情况下，查询图像输入是以下图像：

![](img/77ba35eaabb054e25feaf024e2289a32.png)

我们可以按如下方式显示检索到的结果：

```
fig, axes = plt.subplots(1, top_k, figsize=(15, 3))
    
for i, (idx, score) in enumerate(zip(indices, values)):
    # Print text and score
    print(f"{data['text'][idx]}: {score:.3f}")
    
    # Display image
    axes[i].imshow(data['image'][idx])
    axes[i].axis('off')
    axes[i].set_title(f"Score: {score:.3f}")
    
plt.tight_layout()
plt.show()
```

这会产生以下排名前 5 的匹配结果，大部分看起来都很棒。

![](img/dfe767be981bb568889e639ba9065f90.png)

### 零样本分类

零样本分类是机器学习中的一种范例，其中模型可以将数据分类到从未明确训练过的类别中。

由于该模型已在海量数据集上进行训练，因此无需为每个类别添加任何标记样本。相反，该模型可以利用语义理解，通过自然语言描述将其推广到新的、未知的类别。

以下是使用 CLIP 进行零样本分类的快速演示：

+   首先，我们收集一张可以在线获取的图像：

```
url = "http://images.cocodataset.org/val2017/000000039769.jpg"
```

```
from PIL import Image
import requests
image = Image.open(requests.get(url, stream=True).raw)
```

+   我们有以下图像：

![](img/fae6feadf4393bba4fc7cd18fc29a877.png)

+   接下来，我们对其进行预处理并将其传递给模型：

```
inputs = processor(
    text=["a photo of a cat", "a photo of a dog"],
    images=image,
    return_tensors="pt",
    padding=True,
)

outputs = model(**inputs)
```

+   现在，这里有一些关于此代码需要注意的地方。

+   由于这是一项分类任务，我们已经知道我们想要将新数据归类到哪一类。

+   因此，在文本参数中，我们传递的不是“狗”和“猫”，而是类的描述，这样文本和图像之间的语义相似性就与模型最初的训练方式保持一致。

+   也就是说，一旦我们获得了输出，我们就可以获取 `logits` 并生成相应的 `softmax` 分数，如下所示：

```
logits_per_image = outputs.logits_per_image

probs = logits_per_image.softmax(dim=1)

>>> probs
tensor([[0.9949, 0.0051]])
```

`softmax` 分数与描述“ a photo of a cat”有很高的相似度，这确实是正确的。
