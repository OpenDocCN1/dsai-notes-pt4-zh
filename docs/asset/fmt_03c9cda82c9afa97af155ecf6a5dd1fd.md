
# 将模型调整为推理模式

```
FastLanguageModel.for_inference(model)
```

```
LlamaForCausalLM(
  (model): LlamaModel(
    (embed_tokens): Embedding(128256, 4096, padding_idx=128004)
    (layers): ModuleList(
      (0-31): 32 x LlamaDecoderLayer(
        (self_attn): LlamaAttention(
          (q_proj): Linear(in_features=4096, out_features=4096, bias=False)
          (k_proj): Linear(in_features=4096, out_features=1024, bias=False)
          (v_proj): Linear(in_features=4096, out_features=1024, bias=False)
          (o_proj): Linear(in_features=4096, out_features=4096, bias=False)
          (rotary_emb): LlamaRotaryEmbedding()
        )
        (mlp): LlamaMLP(
          (gate_proj): Linear(in_features=4096, out_features=14336, bias=False)
          (up_proj): Linear(in_features=4096, out_features=14336, bias=False)
          (down_proj): Linear(in_features=14336, out_features=4096, bias=False)
          (act_fn): SiLU()
        )
        (input_layernorm): LlamaRMSNorm((4096,), eps=1e-05)
        (post_attention_layernorm): LlamaRMSNorm((4096,), eps=1e-05)
      )
    )
    (norm): LlamaRMSNorm((4096,), eps=1e-05)
    (rotary_emb): LlamaRotaryEmbedding()
  )
  (lm_head): Linear(in_features=4096, out_features=128256, bias=False)
)
```

然后即可和模型进行对话：

```
question = "请问如何证明根号2是无理数？"
```

然后这里我们首先需要借助分词器，将输入的问题转化为标记索引：

```
inputs = tokenizer([question], return_tensors="pt").to("cuda")
```

```
inputs
```

```
{'input_ids': tensor([[128000,  15225,  57107, 109425, 125544, 102831,  18476,     17,  21043,
          43292,  22649,   9039,  11571]], device='cuda:0'), 'attention_mask': tensor([[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]], device='cuda:0')}
```

最后再带入`inputs`进行对话

```
outputs = model.generate(
    input_ids=inputs.input_ids,
    max_new_tokens=1200,
    use_cache=True,
)
```

此时得到的回复也是词索引：

```
outputs
```

```
tensor([[128000,  15225,  57107,  ...,  37656,  64531,   9039]],
       device='cuda:0')
```

同样需要分词器将其转化为文本：

```
response = tokenizer.batch_decode(outputs)
```

```
response
```

```
['<｜begin▁of▁sentence｜>请问如何证明根号2是无理数？我记得以前学过的证明方法，可能有几种不同的方式。\n\n首先，我记得有一个方法是假设根号2是有理数，然后推导出矛盾。具体来说，假设根号2等于a/b，其中a和b是互质的正整数，没有公因数，且b不为零。然后，通过平方两边，得到2 = a² / b²，进而得到2b² = a²。这样，a²必须是偶数，因此a也是偶数，设为2k。这样，等式变为2b² = (2k)²，也就是2b² = 4k²，进而得到b² = 2k²，这意味着b²也是偶数，所以b也是偶数。这样，a和b都被2整除，这与我们最初的假设a和b互质矛盾，所以根号2是无理数。\n\n不过，我记得还有另一种方法，可能涉及到假设根号2是有理数，然后将其表示为分数，通过某种方式证明会导致无限递减的矛盾。或者，是否还有其他方法，比如利用二次方程的根的性质，或者其他数学概念？\n\n另外，我还记得可能有一个关于连分数展开的方法，或者是利用代数的方式来证明根号2是无理数。比如，使用费马大定理或者其他定理来证明。\n\n不过，我觉得最直接的方法还是假设根号2是有理数，然后推导出矛盾。因为这样比较简单明了，不需要太多复杂的概念。刚才我已经写出了这个方法的基本步骤，但可能需要更详细地展开，确保每一步都没有问题。\n\n让我再理一理这个过程：\n\n1. 假设根号2是有理数，那么它可以表示为最简分数a/b，其中a和b是互质的正整数，且b ≠ 0。\n\n2. 两边平方得到：2 = a² / b²。\n\n3. 移项得到：2b² = a²。\n\n4. 这意味着a²是2的倍数，因此a也必须是2的倍数，因为平方数的因数都是偶数的因数。所以，设a = 2k，其中k是一个正整数。\n\n5. 将a = 2k代入上式，得到：2b² = (2k)² = 4k²。\n\n6. 两边同时除以2，得到：b² = 2k²。\n\n7. 这意味着b²也是2的倍数，因此b也必须是2的倍数。\n\n8. 但是，根据最初的假设，a和b是互质的，即它们的最大公约数是1。如果a和b都被2整除，那么它们都至少有一个公因数2，这与互质的假设矛盾。\n\n9. 因此，原来的假设根号2是有理数是不成立的，所以根号2是无理数。\n\n这个过程看起来是正确的，没有问题。我还记得另一种方法，可能涉及到数论中的无限递降法，即假设根号2是有理数，然后将其表示为最简分数，平方之后得到一个新的分数，进而证明会导致无限递减，这也是一种矛盾的方法。\n\n让我试着回忆一下这种方法的步骤：\n\n1. 假设根号2 = a/b，其中a和b是互质的正整数，且b ≠ 0。\n\n2. 平方两边得到：2 = a² / b²。\n\n3. 移项得到：a² = 2b²。\n\n4. 因为a和b互质，所以a²和b²也互质。\n\n5. 根据数论，若a² = 2b²，则2必须整除a²，因此2整除a。\n\n6. 设a = 2k，其中k是正整数，那么代入上式得到：(2k)² = 2b² ⇒ 4k² = 2b² ⇒ 2k² = b²。\n\n7. 因此，b² = 2k²，意味着b也必须是2的倍数，因为b²是2的倍数。\n\n8. 设b = 2m，其中m是正整数，那么代入上式得到：(2m)² = 2k² ⇒ 4m² = 2k² ⇒ 2m² = k²。\n\n9. 现在，k² = 2m²，同样意味着k必须是2的倍数，设k = 2n，其中n是正整数。\n\n10. 代入得到：(2n)² = 2m² ⇒ 4n² = 2m² ⇒ 2n² = m²。\n\n11. 继续这个过程，m² = 2n²，意味着m必须是2的倍数，设m = 2p，其中p是正整数。\n\n12. 代入得到：(2p)² = 2n² ⇒ 4p² = 2n² ⇒ 2p² = n²。\n\n13. 这样，n² = 2p²，意味着n必须是2的倍数，设n = 2q，其中q是正整数']
```

```
print(response[0])
```

```
<｜begin▁of▁sentence｜>请问如何证明根号2是无理数？我记得以前学过的证明方法，可能有几种不同的方式。

首先，我记得有一个方法是假设根号2是有理数，然后推导出矛盾。具体来说，假设根号2等于a/b，其中a和b是互质的正整数，没有公因数，且b不为零。然后，通过平方两边，得到2 = a² / b²，进而得到2b² = a²。这样，a²必须是偶数，因此a也是偶数，设为2k。这样，等式变为2b² = (2k)²，也就是2b² = 4k²，进而得到b² = 2k²，这意味着b²也是偶数，所以b也是偶数。这样，a和b都被2整除，这与我们最初的假设a和b互质矛盾，所以根号2是无理数。

不过，我记得还有另一种方法，可能涉及到假设根号2是有理数，然后将其表示为分数，通过某种方式证明会导致无限递减的矛盾。或者，是否还有其他方法，比如利用二次方程的根的性质，或者其他数学概念？

另外，我还记得可能有一个关于连分数展开的方法，或者是利用代数的方式来证明根号2是无理数。比如，使用费马大定理或者其他定理来证明。

不过，我觉得最直接的方法还是假设根号2是有理数，然后推导出矛盾。因为这样比较简单明了，不需要太多复杂的概念。刚才我已经写出了这个方法的基本步骤，但可能需要更详细地展开，确保每一步都没有问题。

让我再理一理这个过程：

1. 假设根号2是有理数，那么它可以表示为最简分数a/b，其中a和b是互质的正整数，且b ≠ 0。

2. 两边平方得到：2 = a² / b²。

3. 移项得到：2b² = a²。

4. 这意味着a²是2的倍数，因此a也必须是2的倍数，因为平方数的因数都是偶数的因数。所以，设a = 2k，其中k是一个正整数。

5. 将a = 2k代入上式，得到：2b² = (2k)² = 4k²。

6. 两边同时除以2，得到：b² = 2k²。

7. 这意味着b²也是2的倍数，因此b也必须是2的倍数。

8. 但是，根据最初的假设，a和b是互质的，即它们的最大公约数是1。如果a和b都被2整除，那么它们都至少有一个公因数2，这与互质的假设矛盾。

9. 因此，原来的假设根号2是有理数是不成立的，所以根号2是无理数。

这个过程看起来是正确的，没有问题。我还记得另一种方法，可能涉及到数论中的无限递降法，即假设根号2是有理数，然后将其表示为最简分数，平方之后得到一个新的分数，进而证明会导致无限递减，这也是一种矛盾的方法。

让我试着回忆一下这种方法的步骤：

1. 假设根号2 = a/b，其中a和b是互质的正整数，且b ≠ 0。

2. 平方两边得到：2 = a² / b²。

3. 移项得到：a² = 2b²。

4. 因为a和b互质，所以a²和b²也互质。

5. 根据数论，若a² = 2b²，则2必须整除a²，因此2整除a。

6. 设a = 2k，其中k是正整数，那么代入上式得到：(2k)² = 2b² ⇒ 4k² = 2b² ⇒ 2k² = b²。

7. 因此，b² = 2k²，意味着b也必须是2的倍数，因为b²是2的倍数。

8. 设b = 2m，其中m是正整数，那么代入上式得到：(2m)² = 2k² ⇒ 4m² = 2k² ⇒ 2m² = k²。

9. 现在，k² = 2m²，同样意味着k必须是2的倍数，设k = 2n，其中n是正整数。

10. 代入得到：(2n)² = 2m² ⇒ 4n² = 2m² ⇒ 2n² = m²。

11. 继续这个过程，m² = 2n²，意味着m必须是2的倍数，设m = 2p，其中p是正整数。

12. 代入得到：(2p)² = 2n² ⇒ 4p² = 2n² ⇒ 2p² = n²。

13. 这样，n² = 2p²，意味着n必须是2的倍数，设n = 2q，其中q是正整数
```

至此我们就完成了`unsloth`模型推理流程。

## 尝试使用`unsloth`调用`Qwen`模型

类似的，我们也可以使用`unsloth`调用`Qwen`蒸馏模型

```
model_qwen, tokenizer_qwen = FastLanguageModel.from_pretrained(
    model_name = "./DeepSeek-R1-Distill-Qwen-7B",
    max_seq_length = max_seq_length,
    dtype = dtype,
    load_in_4bit = load_in_4bit,
)
```

```
==((====))==  Unsloth 2025.1.8: Fast Qwen2 patching. Transformers: 4.48.2.
   \\   /|    GPU: NVIDIA H800 PCIe. Max memory: 79.205 GB. Platform: Linux.
O^O/ \_/ \    Torch: 2.5.1+cu124. CUDA: 9.0. CUDA Toolkit: 12.4. Triton: 3.1.0
\        /    Bfloat16 = TRUE. FA [Xformers = 0.0.29.post1. FA2 = False]
 "-____-"     Free Apache license: http://github.com/unslothai/unsloth
Unsloth: Fast downloading is enabled - ignore downloading bars which are red colored!

Loading checkpoint shards:   0%|          | 0/2 [00:00<?, ?it/s]


./DeepSeek-R1-Distill-Qwen-7B does not have a padding token! Will use pad_token = <|vision_pad|>.
```

```
FastLanguageModel.for_inference(model_qwen)
```

```
Qwen2ForCausalLM(
  (model): Qwen2Model(
    (embed_tokens): Embedding(152064, 3584, padding_idx=151654)
    (layers): ModuleList(
      (0-27): 28 x Qwen2DecoderLayer(
        (self_attn): Qwen2Attention(
          (q_proj): Linear(in_features=3584, out_features=3584, bias=True)
          (k_proj): Linear(in_features=3584, out_features=512, bias=True)
          (v_proj): Linear(in_features=3584, out_features=512, bias=True)
          (o_proj): Linear(in_features=3584, out_features=3584, bias=False)
          (rotary_emb): LlamaRotaryEmbedding()
        )
        (mlp): Qwen2MLP(
          (gate_proj): Linear(in_features=3584, out_features=18944, bias=False)
          (up_proj): Linear(in_features=3584, out_features=18944, bias=False)
          (down_proj): Linear(in_features=18944, out_features=3584, bias=False)
          (act_fn): SiLU()
        )
        (input_layernorm): Qwen2RMSNorm((3584,), eps=1e-06)
        (post_attention_layernorm): Qwen2RMSNorm((3584,), eps=1e-06)
      )
    )
    (norm): Qwen2RMSNorm((3584,), eps=1e-06)
    (rotary_emb): LlamaRotaryEmbedding()
  )
  (lm_head): Linear(in_features=3584, out_features=152064, bias=False)
)
```

```
question = "你好，好久不见！"
```

```
inputs = tokenizer_qwen([question], return_tensors="pt").to("cuda")

outputs = model_qwen.generate(
    input_ids=inputs.input_ids,
    max_new_tokens=1200,
    use_cache=True,
)

response = tokenizer_qwen.batch_decode(outputs)

print(response[0])
```
