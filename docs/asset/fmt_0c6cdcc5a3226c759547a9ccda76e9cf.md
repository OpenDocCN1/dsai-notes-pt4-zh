
# LLaMA 中文分词器扩充

## Add Chinese tokens to LLaMA tokenizer

```text
llama_spm_tokens_set = set(p.piece for p in llama_spm.pieces)
print(len(llama_spm_tokens_set))
print(f"Before:{len(llama_spm_tokens_set)}")
for p in chinese_spm.pieces:
    piece = p.piece
    if piece not in llama_spm_tokens_set:
        new_p = sp_pb2_model.ModelProto().SentencePiece()
        new_p.piece = piece
        new_p.score = 0
        llama_spm.pieces.append(new_p)
print(f"New model pieces: {len(llama_spm.pieces)}")
```

输出：

```text
32000
Before:32000
New model pieces: 49953
```

### 保存处理后的新的分词器

```text
output_sp_dir = 'merged_tokenizer_sp'
output_hf_dir = 'merged_tokenizer_hf' # the path to save Chinese-LLaMA tokenizer
os.makedirs(output_sp_dir, exist_ok=True)
with open(output_sp_dir + '/chinese_llama.model', 'wb') as f:
    f.write(llama_spm.SerializeToString())
tokenizer = LlamaTokenizer(vocab_file=output_sp_dir + '/chinese_llama.model')

tokenizer.save_pretrained(output_hf_dir)
print(f"Chinese-LLaMA tokenizer has been saved to {output_hf_dir}")
```

### 测试并比较两个分词器的效果

```text
llama_tokenizer = LlamaTokenizer.from_pretrained('../../../vllm/ckpt/vicuna-13b-v1.3')
chinese_llama_tokenizer = LlamaTokenizer.from_pretrained(output_hf_dir)
text = "木末芙蓉花，山中发红萼。涧户寂无人，纷纷开且落。"
print("Test text:\n", text)
print(f"Tokenized by LLaMA tokenizer:{llama_tokenizer.tokenize(text)}")
print(f"Tokenized by Chinese-LLaMA tokenizer:{chinese_llama_tokenizer.tokenize(text)}")
```

结果如下：

```text
Test text:
 木末芙蓉花，山中发红萼。涧户寂无人，纷纷开且落。
Tokenized by LLaMA tokenizer:['▁', '木', '<0xE6>', '<0x9C>', '<0xAB>', '<0xE8>', '<0x8A>', '<0x99>', '<0xE8>', '<0x93>', '<0x89>', '花', '，', '山', '中', '发', '红', '<0xE8>', '<0x90>', '<0xBC>', '。', '<0xE6>', '<0xB6>', '<0xA7>', '户', '<0xE5>', '<0xAF>', '<0x82>', '无', '人', '，', '<0xE7>', '<0xBA>', '<0xB7>', '<0xE7>', '<0xBA>', '<0xB7>', '开', '<0xE4>', '<0xB8>', '<0x94>', '<0xE8>', '<0x90>', '<0xBD>', '。']
Tokenized by Chinese-LLaMA tokenizer:['▁', '木', '末', '芙蓉', '花', '，', '山', '中', '发', '红', '萼', '。', '涧', '户', '寂', '无人', '，', '纷纷', '开', '且', '落', '。']
```

原版的小分词器分词后的词元数达到了 45，而新的扩充后的分词器只有 22，并且通过上述结果也能看出，扩充后的效果更好。

## 参考资料

[1] [What is Tokenization? Everything You Need to Know | TokenEx](https://link.zhihu.com/?target=https%3A//www.tokenex.com/resource-center/what-is-tokenization)

[2] [zhuanlan.zhihu.com/p/109054674](https://zhuanlan.zhihu.com/p/109054674)

[3] [blog.csdn.net/songbinxu/article/details/80209197](https://link.zhihu.com/?target=https%3A//blog.csdn.net/songbinxu/article/details/80209197%3Fdepth_1-utm_source%3Ddistribute.pc_relevant.none-task%26utm_source%3Ddistribute.pc_relevant.none-task)

[4] [blog.csdn.net/Zh823275484/article/details/87878512](https://link.zhihu.com/?target=https%3A//blog.csdn.net/Zh823275484/article/details/87878512)

[5] [github.com/liuhuanyong/WordSegment](https://link.zhihu.com/?target=https%3A//github.com/liuhuanyong/WordSegment)

[6] [NLP BERT GPT等模型中 tokenizer 类别说明详解 - 云+社区 - 腾讯云](https://link.zhihu.com/?target=https%3A//cloud.tencent.com/developer/article/1865689)

[7] [ymcui/Chinese-LLaMA-Alpaca: 中文LLaMA&Alpaca大语言模型+本地CPU/GPU训练部署](https://link.zhihu.com/?target=https%3A//github.com/ymcui/Chinese-LLaMA-Alpaca)

[8] [大模型词表扩充必备工具SentencePiece - 知乎](https://zhuanlan.zhihu.com/p/630696264)

[9] [【万字长文】LLaMA, ChatGLM, BLOOM的参数高效微调实践](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/JWuzln2ltd6-5f7QbS2yUw)

> No matter how hard we try to be mature, we will always be a kid when we all get hurt and cry. ---- Peter and Wendy
