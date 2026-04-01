# infer.ipynb

> 来源：[https://v11enp9ok1h.feishu.cn/docx/MClYdLZTUog8GVxNsUUcdWZvnMd](https://v11enp9ok1h.feishu.cn/docx/MClYdLZTUog8GVxNsUUcdWZvnMd)

```
{
 &quot;cells&quot;: [
  {
   &quot;cell_type&quot;: &quot;markdown&quot;,
   &quot;metadata&quot;: {
    &quot;collapsed&quot;: false
   },
   &quot;source&quot;: [
    &quot;## 介绍\n&quot;,
    &quot;这个是`chinese-gpt2`的推理代码\n&quot;,
    &quot;1. 将`model_name_or_path = \&quot;checkpoint-36000\&quot;`里面的`\&quot;checkpoint-36000\&quot;`,修改为模型所在的路径。\n&quot;,
    &quot;2. 然后运行下面一个代码块，即可输出文本生成结果&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 2,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;from transformers import GPT2Tokenizer, GPT2LMHeadModel, AutoTokenizer\n&quot;,
    &quot;\n&quot;,
    &quot;model_name_or_path = \&quot;checkpoint-36000\&quot;\n&quot;,
    &quot;tokenizer = AutoTokenizer.from_pretrained(model_name_or_path)\n&quot;,
    &quot;\n&quot;,
    &quot;# add the EOS token as PAD token to avoid warnings\n&quot;,
    &quot;model = GPT2LMHeadModel.from_pretrained(model_name_or_path, pad_token_id=tokenizer.eos_token_id)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 6,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Output:\n&quot;,
      &quot;----------------------------------------------------------------------------------------------------\n&quot;,
      &quot;你 是 谁?, 简 单 的 描 述 是, 答 案 是 你 好 ， 我 是 一 个 女 孩 子 的 妈 妈 。 她 是 我 的 初 中 同 学 。 我 们 一 起 读 书 的 时 候 ， 有 一 次 我 和 她 聊 天 ， 她 问 我 ， 你 为 什 么 喜 欢 我 ？ 我 说 ， 因 为 我 知 道 你 的 家 庭 条 件 不 错 ， 所 以 我 也 很 爱 她 。 后 来 她 告 诉 我 她 的 父 母 都 是 农 村 出 身 的 ， 而 且 是 个 很 有 文 化 的 人 。 当 时 我 就 觉 得 很 奇 怪 ， 就 问 了 她 一 句 ： 你 们 这 些 人 是 怎 么 想 的 ？ 她 回 答 说 ： 是 的 。 那 一 刻 我 感 觉 到 了 自 己 的 力 量 ， 也 就 是 说 我 对 这 个 世 界 充 满 了 好 奇 和 向 往 。 可 是 当 我\n&quot;
     ]
    }
   ],
   &quot;source&quot;: [
    &quot;txt = \&quot;\&quot;\&quot;\\\n&quot;,
    &quot;你是谁\n&quot;,
    &quot;\&quot;\&quot;\&quot;\n&quot;,
    &quot;# encode context the generation is conditioned on\n&quot;,
    &quot;input_ids = tokenizer.encode(txt, return_tensors=&#x27;pt&#x27;, add_special_tokens=False)\n&quot;,
    &quot;# set no_repeat_ngram_size to 2\n&quot;,
    &quot;beam_output = model.generate(\n&quot;,
    &quot;    input_ids, \n&quot;,
    &quot;    max_length=200, \n&quot;,
    &quot;    num_beams=5, \n&quot;,
    &quot;    no_repeat_ngram_size=2, \n&quot;,
    &quot;    early_stopping=True\n&quot;,
    &quot;)\n&quot;,
    &quot;\n&quot;,
    &quot;print(\&quot;Output:\\n\&quot; + 100 * &#x27;-&#x27;)\n&quot;,
    &quot;print(tokenizer.decode(beam_output[0], skip_special_tokens=True))\n&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: []
  }
 ],
 &quot;metadata&quot;: {
  &quot;kernelspec&quot;: {
   &quot;display_name&quot;: &quot;mynet&quot;,
   &quot;language&quot;: &quot;python&quot;,
   &quot;name&quot;: &quot;python3&quot;
  },
  &quot;language_info&quot;: {
   &quot;codemirror_mode&quot;: {
    &quot;name&quot;: &quot;ipython&quot;,
    &quot;version&quot;: 3
   },
   &quot;file_extension&quot;: &quot;.py&quot;,
   &quot;mimetype&quot;: &quot;text/x-python&quot;,
   &quot;name&quot;: &quot;python&quot;,
   &quot;nbconvert_exporter&quot;: &quot;python&quot;,
   &quot;pygments_lexer&quot;: &quot;ipython3&quot;,
   &quot;version&quot;: &quot;3.10.8&quot;
  },
  &quot;orig_nbformat&quot;: 4,
  &quot;vscode&quot;: {
   &quot;interpreter&quot;: {
    &quot;hash&quot;: &quot;b4e9266737a3d6b71b60265cf120055eda92ec7823ffe2a0ed53da8b39d4a1eb&quot;
   }
  }
 },
 &quot;nbformat&quot;: 4,
 &quot;nbformat_minor&quot;: 2
}
```