
# 原理进阶

> 来源：[https://geekbang.feishu.cn/docx/GEOzd7aznoTucQxgO1gcFq0un6c](https://geekbang.feishu.cn/docx/GEOzd7aznoTucQxgO1gcFq0un6c)

## Stable Diffusion

> Stable Diffusion XL（SDXL）是稳定扩散模型的一个更大、更强大的迭代版本，能够生成更高分辨率的图像。

> `SDXL`的`UNet`比原版大3倍，并在架构中添加了第二个文本编码器。根据您可用的硬件情况，这可能需要很高的计算资源，并且可能无法在像Tesla T4这样的消费级GPU上运行。为了将这个较大模型适应内存并加快训练速度，请尝试启用`gradient_checkpointing`、`mixed_precision`和`gradient_accumulation_steps`。通过启用`xFormers`中的内存高效注意力和使用`bitsandbytes`的8位优化器，您甚至可以进一步减少内存使用量。

## Lora

> `LoRA`（大型语言模型的低秩适应）是一种流行且轻量级的训练技术，可以显著减少可训练参数的数量。它通过向模型中插入较少数量的新权重，并仅对这些权重进行训练来实现。这使得使用`LoRA`进行训练更快速、内存效率高，并产生较小的模型权重（几百兆字节），更易于存储和共享。`LoRA`还可以与其他训练技术（如`DreamBooth`）结合以加快训练速度。

## Contronet

> `ControlNet`是一种通过使用额外的输入图像对图像扩散模型进行条件控制的模型类型。有许多类型的条件输入（如`Canny`边缘、用户素描、人体姿势、深度等），您可以使用它们来控制扩散模型。这非常有用，因为它使您能够更好地控制图像生成过程，从而更容易生成特定的图像，而无需尝试不同的文本提示或去噪值。
