
+`eval_iters`: 评估的迭代次数，默认为`100`。

+`eval_interval`: 评估的间隔（`steps`），默认为`None`，即设置为`save_interval`。

## 混合精度参数

+`fp16`: `fp16`模式。默认为`False`。会根据模型的`torch_dtype`进行设置。请使用`--torch_dtype`进行设置，默认读取`config.json`。

+`bf16`: `bf16`模式。默认为`False`。会根据模型的`torch_dtype`进行设置。

+`apply_query_key_layer_scaling`: 将`Q * K^T`缩放为`1 / 层数`（例如：第`layer_num`层则除以`layer_num`）。这对`fp16`训练很有帮助。默认为`None`，即若使用`--fp16`，则设置为`True`。

+`attention_softmax_in_fp32`: 在`attention_mask`和`softmax`中使用`fp32`进行计算。默认为`True`。

## 模型参数

（以下参数通常不需要进行设置，会根据HF模型的`config.json`进行配置，用户无需关心）

+`num_layers`: `transformer` layers的层数，默认为`None`。

+`hidden_size`: `transformer` hidden size，默认为`None`。

+`ffn_hidden_size`: `transformer` FFN层的hidden size。默认为`None`，设置为`4*hidden_size`。

+`num_attention_heads`: `transformer` attention heads的个数，默认为`None`。

+`group_query_attention`: 默认为`None`。若`num_query_groups > 1`，`group_query_attention`设置为`True`，否则为`False`。

+`num_query_groups`: 默认为`1`。

+`max_position_embeddings`: 位置编码的最大长度，默认为`None`。

+`position_embedding_type`: 位置编码的类型，可选为`'learned_absolute'`、`'rope'`、`'relative'`和`'none'`，默认为`'rope'`。

+`rotary_base`: 默认为`10000`。

+`rotary_percent`: 默认为`1.`。

+`normalization`: 可选为`'LayerNorm'`、`'RMSNorm'`，默认为`RMSNorm`。

+`norm_epsilon`: 默认为`1e-5`。

+`swiglu`: 使用`swiglu`替代默认的`gelu`。默认为`True`。

+`untie_embeddings_and_output_weights`: 解开`embedding`和输出权重的绑定，默认为`True`。

+`disable_bias_linear`: 禁用`linear`层的`bias`。默认为`True`。

+`add_qkv_bias`: 仅在`QKV`的`linear`中增加`bias`，默认为`True`。

+`attention_dropout`: 默认为`0.`。

+`hidden_dropout`: 默认为`0.`。

+`transformer_impl`: 使用哪种`transformer`实现，可选项为`'local'`和`'transformer_engine'`。默认为`transformer_engine`。

+`padded_vocab_size`: 完整词表大小，默认为`None`。

+`rope_scaling`: `rope_scaling`相关参数，默认为`None`。格式参考`llama3.1 config.json`，传入`json`字符串。

### Megatron训练参数

Megatron训练参数继承自Megatron参数和基本参数。基本参数的内容可以参考。此外还包括以下参数：

+`add_version`: 在`save`上额外增加目录`'<版本号>-<时间戳>'`防止权重覆盖，默认为`True`。

+`packing`: 是否使用序列`packing`，默认为`False`。

+`streaming`: 流式读取并处理数据集，默认`False`。通常在处理大型数据集时，设置为`True`。更多流式的参数查看命令行参数文档。

+`lazy_tokenize`: 默认为`False`。若该参数设置为`False`，则在训练之前对所有的数据集样本进行`tokenize`（这可以避免在训练中出现报错）；设置为`True`，则在训练中对数据集进行`tokenize`（这可以节约内存）。
