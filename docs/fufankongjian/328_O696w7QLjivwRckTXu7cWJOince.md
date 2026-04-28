# 代码实战：手动编写Qwen模型的Chat 方法并接入vLLM框架，实现多轮对话及WebUI应用

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/WDZFdPZP1o0trCxVDsBczox0n4d](https://kq4b3vgg5b.feishu.cn/docx/WDZFdPZP1o0trCxVDsBczox0n4d)

# 本地部署开源大模型

# 3.手动实现基于vLLM框架的chat 方法

所谓续写模式和对话模式，源于模型的训练数据不同，对于Qwen-7B基座模型来说，其训练数据的格式是这样的：

```
输入：1月30日上午，中央气象台官微发布一张郑州市降水预报图，直呼：“河南郑州本次降雪预报图，这么离谱的预报图头一次见！&lt;|endoftext|&gt;
输出：预计1月31日至2月5日，我国中东部地区将进入入冬以来持续时间最长、影响范围最广的雨雪冰冻天气过程。10省份将出现暴雪或大暴雪，河南、河北、山东、辽宁、湖北等地的日降水量或累计降雪量具有极端性，河南、湖北、安徽、湖南、贵州5省将出现冻雨。&lt;|endoftext|&gt;
...
...
输入：1月28日，“格力2024全球梦想盛典”在珠海举行。格力电器员工在“格力全球梦想盛典”上合唱《我妈董明珠》的视频引起网友热议。&lt;|endoftext|&gt;
输出：这个节目名为音乐快板《大“格”局 新魅“力”》，表演者当时为了增加幽默气氛，开了个小玩笑，原话是“我妈就是董明珠……我妈最喜欢董明珠”。&lt;|endoftext|&gt;
```

而对话模型，其训练数据往往是一问一答的形式。每个模型都有其开发者自定义的输入输出格式，比如gpt3.5是这样的：

```
{&quot;messages&quot;: [{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;Marv is a factual chatbot that is also sarcastic.&quot;}, 
              {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;What&#x27;s the capital of France?&quot;}, 
              {&quot;role&quot;: &quot;assistant&quot;, &quot;content&quot;: &quot;Paris, as if everyone doesn&#x27;t know that already.&quot;}]}

{&quot;messages&quot;: [{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;Marv is a factual chatbot that is also sarcastic.&quot;}, 
              {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Who wrote &#x27;Romeo and Juliet&#x27;?&quot;}, 
              {&quot;role&quot;: &quot;assistant&quot;, &quot;content&quot;: &quot;Oh, just some guy named William Shakespeare. Ever heard of him?&quot;}]}

{&quot;messages&quot;: [{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;Marv is a factual chatbot that is also sarcastic.&quot;}, 
              {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;How far is the Moon from Earth?&quot;}, 
              {&quot;role&quot;: &quot;assistant&quot;, &quot;content&quot;: &quot;Around 384,400 kilometers. Give or take a few, like that really matters.&quot;}]},]}]}
```

Qwen微调的对话格式是这样的：

```
[
  {
    &quot;id&quot;: &quot;identity_0&quot;,
    &quot;conversations&quot;: [
      {
        &quot;from&quot;: &quot;user&quot;,
        &quot;value&quot;: &quot;你好&quot;
      },
      {
        &quot;from&quot;: &quot;assistant&quot;,
        &quot;value&quot;: &quot;我是一个语言模型，我叫通义千问。&quot;
      }
    ]
  }
]
```

Qwen的开源系列中的Chat类对话模型（Qwen-7B-Chat），它是基于对应的预训练版本（Qwen-7B）进行微调（SFT，S监督学习，FT微调）得到的，所以基于上述对话形式，就会通过如下方式，来界定某一个角色下的输入和输出：

```
输入：&lt;|im_start|&gt;system
      你是一个智能助手。&lt;|im_end|&gt;                 （这一部分是系统信息）
      
      &lt;|im_start|&gt;user
      你好呀，请你介绍一下你自己&lt;|im_end|&gt;
      &lt;|im_start|&gt;assitant
      我是Qwen-Chat模型，一个非常厉害的人工智能助手，你可以向我提出任意的提问&lt;|im_end|&gt;
      &lt;|im_start|&gt;user
      好的，我想吃点甜食，你能不能帮我推荐一些好吃的食物？&lt;|im_end|&gt;
      &lt;|im_start|&gt;assitant
      当然可以，如果你喜欢吃甜食，那么推荐你吃巧克力的甜甜圈，太好吃了&lt;|im_end|&gt;       （这一部分是历史对话信息）
      
      &lt;|im_start|&gt;user
      那么去哪里可以买到呢？&lt;|im_end|&gt;
       &lt;|im_start|&gt;assitant&lt;|endoftext|&gt;    （这一部分是最新一轮的用户提问）

输出：你可以去超市、蛋糕店等地方买到这个美味的食物&lt;|im_end|&gt;&lt;|endoftext|&gt;     （这部分是Qwen模型针对最新一轮的提问，结合历史内容生成的回复）
```

所以我们能表象看到的微调格式，并不是实际意义上送入Qwen模型训练的形式，而是会经过一系列格式的转化后，再送入模型。可以在这里看到：

![](img/3ab751e7b5efa9d6f206ad23514b588d.png)

这种格式，被Qwen官方定义为ChatML格式。上述的输出，对应的就是我们上一步使用chat()方法，对Qwen模型执行的三次对话，也就是这样的三次操作：

```
# 第一轮对话
response, history = model.chat(tokenizer, &quot;你好呀，请你介绍一下你自己&quot;, history=&quot;你是一个智能助手。&quot;)
print(response)   -- &gt;  我是Qwen-Chat模型，一个非常厉害的人工智能助手，你可以向我提出任意的提问

# 第一轮对话
response, history = model.chat(tokenizer, &quot;好的，我想吃点甜食，你能不能帮我推荐一些好吃的食物？&quot;, history=history)
print(response)  -- &gt;  当然可以，如果你喜欢吃甜食，那么推荐你吃巧克力的甜甜圈，太好吃了

# 第三轮对话
response, history = model.chat(tokenizer, &quot;那么去哪里可以买到呢？&quot;, history=history)
print(response)  -- &gt; 你可以去超市、蛋糕店等地方买到这个美味的食物
```

这里的<|im_end|>、<|im_start|>及<|endoftext|>，也是Qwen官方定义的特殊Token，其中Qwen-7B中有<|endoftext|>，Qwen-7B-Chat中有<|endoftext|>、<|im_start|>以及<|im_end|>。 同时词表中也留有供扩展的特殊token位，可用<|extra_0|>到<|extra_204|>来指代。我们可以通过tokenizer.special_tokens来查看：

```
# 导入Transformer依赖库
from transformers import AutoModelForCausalLM, AutoTokenizer

# 初始化tokenizer
tokenizer = AutoTokenizer.from_pretrained(&quot;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-7B-Chat/&quot;, trust_remote_code=True,)

# 打印特殊Token
tokenizer.special_tokens
```

```
/home/util/anaconda3/envs/qwen_7b_chat/lib/python3.11/site-packages/tqdm/auto.py:21: TqdmWarning: IProgress not found. Please update jupyter and ipywidgets. See https://ipywidgets.readthedocs.io/en/stable/user_install.html
  from .autonotebook import tqdm as notebook_tqdm





{&#x27;&lt;|endoftext|&gt;&#x27;: 151643,
 &#x27;&lt;|im_start|&gt;&#x27;: 151644,
 &#x27;&lt;|im_end|&gt;&#x27;: 151645,
 &#x27;&lt;|extra_0|&gt;&#x27;: 151646,
 &#x27;&lt;|extra_1|&gt;&#x27;: 151647,
 &#x27;&lt;|extra_2|&gt;&#x27;: 151648,
 &#x27;&lt;|extra_3|&gt;&#x27;: 151649,
 &#x27;&lt;|extra_4|&gt;&#x27;: 151650,
 &#x27;&lt;|extra_5|&gt;&#x27;: 151651,
 &#x27;&lt;|extra_6|&gt;&#x27;: 151652,
 &#x27;&lt;|extra_7|&gt;&#x27;: 151653,
 &#x27;&lt;|extra_8|&gt;&#x27;: 151654,
 &#x27;&lt;|extra_9|&gt;&#x27;: 151655,
 &#x27;&lt;|extra_10|&gt;&#x27;: 151656,
 &#x27;&lt;|extra_11|&gt;&#x27;: 151657,
 &#x27;&lt;|extra_12|&gt;&#x27;: 151658,
 &#x27;&lt;|extra_13|&gt;&#x27;: 151659,
 &#x27;&lt;|extra_14|&gt;&#x27;: 151660,
 &#x27;&lt;|extra_15|&gt;&#x27;: 151661,
 &#x27;&lt;|extra_16|&gt;&#x27;: 151662,
 &#x27;&lt;|extra_17|&gt;&#x27;: 151663,
 &#x27;&lt;|extra_18|&gt;&#x27;: 151664,
 &#x27;&lt;|extra_19|&gt;&#x27;: 151665,
 &#x27;&lt;|extra_20|&gt;&#x27;: 151666,
 &#x27;&lt;|extra_21|&gt;&#x27;: 151667,
 &#x27;&lt;|extra_22|&gt;&#x27;: 151668,
 &#x27;&lt;|extra_23|&gt;&#x27;: 151669,
 &#x27;&lt;|extra_24|&gt;&#x27;: 151670,
 &#x27;&lt;|extra_25|&gt;&#x27;: 151671,
 &#x27;&lt;|extra_26|&gt;&#x27;: 151672,
 &#x27;&lt;|extra_27|&gt;&#x27;: 151673,
 &#x27;&lt;|extra_28|&gt;&#x27;: 151674,
 &#x27;&lt;|extra_29|&gt;&#x27;: 151675,
 &#x27;&lt;|extra_30|&gt;&#x27;: 151676,
 &#x27;&lt;|extra_31|&gt;&#x27;: 151677,
 &#x27;&lt;|extra_32|&gt;&#x27;: 151678,
 &#x27;&lt;|extra_33|&gt;&#x27;: 151679,
 &#x27;&lt;|extra_34|&gt;&#x27;: 151680,
 &#x27;&lt;|extra_35|&gt;&#x27;: 151681,
 &#x27;&lt;|extra_36|&gt;&#x27;: 151682,
 &#x27;&lt;|extra_37|&gt;&#x27;: 151683,
 &#x27;&lt;|extra_38|&gt;&#x27;: 151684,
 &#x27;&lt;|extra_39|&gt;&#x27;: 151685,
 &#x27;&lt;|extra_40|&gt;&#x27;: 151686,
 &#x27;&lt;|extra_41|&gt;&#x27;: 151687,
 &#x27;&lt;|extra_42|&gt;&#x27;: 151688,
 &#x27;&lt;|extra_43|&gt;&#x27;: 151689,
 &#x27;&lt;|extra_44|&gt;&#x27;: 151690,
 &#x27;&lt;|extra_45|&gt;&#x27;: 151691,
 &#x27;&lt;|extra_46|&gt;&#x27;: 151692,
 &#x27;&lt;|extra_47|&gt;&#x27;: 151693,
 &#x27;&lt;|extra_48|&gt;&#x27;: 151694,
 &#x27;&lt;|extra_49|&gt;&#x27;: 151695,
 &#x27;&lt;|extra_50|&gt;&#x27;: 151696,
 &#x27;&lt;|extra_51|&gt;&#x27;: 151697,
 &#x27;&lt;|extra_52|&gt;&#x27;: 151698,
 &#x27;&lt;|extra_53|&gt;&#x27;: 151699,
 &#x27;&lt;|extra_54|&gt;&#x27;: 151700,
 &#x27;&lt;|extra_55|&gt;&#x27;: 151701,
 &#x27;&lt;|extra_56|&gt;&#x27;: 151702,
 &#x27;&lt;|extra_57|&gt;&#x27;: 151703,
 &#x27;&lt;|extra_58|&gt;&#x27;: 151704,
 &#x27;&lt;|extra_59|&gt;&#x27;: 151705,
 &#x27;&lt;|extra_60|&gt;&#x27;: 151706,
 &#x27;&lt;|extra_61|&gt;&#x27;: 151707,
 &#x27;&lt;|extra_62|&gt;&#x27;: 151708,
 &#x27;&lt;|extra_63|&gt;&#x27;: 151709,
 &#x27;&lt;|extra_64|&gt;&#x27;: 151710,
 &#x27;&lt;|extra_65|&gt;&#x27;: 151711,
 &#x27;&lt;|extra_66|&gt;&#x27;: 151712,
 &#x27;&lt;|extra_67|&gt;&#x27;: 151713,
 &#x27;&lt;|extra_68|&gt;&#x27;: 151714,
 &#x27;&lt;|extra_69|&gt;&#x27;: 151715,
 &#x27;&lt;|extra_70|&gt;&#x27;: 151716,
 &#x27;&lt;|extra_71|&gt;&#x27;: 151717,
 &#x27;&lt;|extra_72|&gt;&#x27;: 151718,
 &#x27;&lt;|extra_73|&gt;&#x27;: 151719,
 &#x27;&lt;|extra_74|&gt;&#x27;: 151720,
 &#x27;&lt;|extra_75|&gt;&#x27;: 151721,
 &#x27;&lt;|extra_76|&gt;&#x27;: 151722,
 &#x27;&lt;|extra_77|&gt;&#x27;: 151723,
 &#x27;&lt;|extra_78|&gt;&#x27;: 151724,
 &#x27;&lt;|extra_79|&gt;&#x27;: 151725,
 &#x27;&lt;|extra_80|&gt;&#x27;: 151726,
 &#x27;&lt;|extra_81|&gt;&#x27;: 151727,
 &#x27;&lt;|extra_82|&gt;&#x27;: 151728,
 &#x27;&lt;|extra_83|&gt;&#x27;: 151729,
 &#x27;&lt;|extra_84|&gt;&#x27;: 151730,
 &#x27;&lt;|extra_85|&gt;&#x27;: 151731,
 &#x27;&lt;|extra_86|&gt;&#x27;: 151732,
 &#x27;&lt;|extra_87|&gt;&#x27;: 151733,
 &#x27;&lt;|extra_88|&gt;&#x27;: 151734,
 &#x27;&lt;|extra_89|&gt;&#x27;: 151735,
 &#x27;&lt;|extra_90|&gt;&#x27;: 151736,
 &#x27;&lt;|extra_91|&gt;&#x27;: 151737,
 &#x27;&lt;|extra_92|&gt;&#x27;: 151738,
 &#x27;&lt;|extra_93|&gt;&#x27;: 151739,
 &#x27;&lt;|extra_94|&gt;&#x27;: 151740,
 &#x27;&lt;|extra_95|&gt;&#x27;: 151741,
 &#x27;&lt;|extra_96|&gt;&#x27;: 151742,
 &#x27;&lt;|extra_97|&gt;&#x27;: 151743,
 &#x27;&lt;|extra_98|&gt;&#x27;: 151744,
 &#x27;&lt;|extra_99|&gt;&#x27;: 151745,
 &#x27;&lt;|extra_100|&gt;&#x27;: 151746,
 &#x27;&lt;|extra_101|&gt;&#x27;: 151747,
 &#x27;&lt;|extra_102|&gt;&#x27;: 151748,
 &#x27;&lt;|extra_103|&gt;&#x27;: 151749,
 &#x27;&lt;|extra_104|&gt;&#x27;: 151750,
 &#x27;&lt;|extra_105|&gt;&#x27;: 151751,
 &#x27;&lt;|extra_106|&gt;&#x27;: 151752,
 &#x27;&lt;|extra_107|&gt;&#x27;: 151753,
 &#x27;&lt;|extra_108|&gt;&#x27;: 151754,
 &#x27;&lt;|extra_109|&gt;&#x27;: 151755,
 &#x27;&lt;|extra_110|&gt;&#x27;: 151756,
 &#x27;&lt;|extra_111|&gt;&#x27;: 151757,
 &#x27;&lt;|extra_112|&gt;&#x27;: 151758,
 &#x27;&lt;|extra_113|&gt;&#x27;: 151759,
 &#x27;&lt;|extra_114|&gt;&#x27;: 151760,
 &#x27;&lt;|extra_115|&gt;&#x27;: 151761,
 &#x27;&lt;|extra_116|&gt;&#x27;: 151762,
 &#x27;&lt;|extra_117|&gt;&#x27;: 151763,
 &#x27;&lt;|extra_118|&gt;&#x27;: 151764,
 &#x27;&lt;|extra_119|&gt;&#x27;: 151765,
 &#x27;&lt;|extra_120|&gt;&#x27;: 151766,
 &#x27;&lt;|extra_121|&gt;&#x27;: 151767,
 &#x27;&lt;|extra_122|&gt;&#x27;: 151768,
 &#x27;&lt;|extra_123|&gt;&#x27;: 151769,
 &#x27;&lt;|extra_124|&gt;&#x27;: 151770,
 &#x27;&lt;|extra_125|&gt;&#x27;: 151771,
 &#x27;&lt;|extra_126|&gt;&#x27;: 151772,
 &#x27;&lt;|extra_127|&gt;&#x27;: 151773,
 &#x27;&lt;|extra_128|&gt;&#x27;: 151774,
 &#x27;&lt;|extra_129|&gt;&#x27;: 151775,
 &#x27;&lt;|extra_130|&gt;&#x27;: 151776,
 &#x27;&lt;|extra_131|&gt;&#x27;: 151777,
 &#x27;&lt;|extra_132|&gt;&#x27;: 151778,
 &#x27;&lt;|extra_133|&gt;&#x27;: 151779,
 &#x27;&lt;|extra_134|&gt;&#x27;: 151780,
 &#x27;&lt;|extra_135|&gt;&#x27;: 151781,
 &#x27;&lt;|extra_136|&gt;&#x27;: 151782,
 &#x27;&lt;|extra_137|&gt;&#x27;: 151783,
 &#x27;&lt;|extra_138|&gt;&#x27;: 151784,
 &#x27;&lt;|extra_139|&gt;&#x27;: 151785,
 &#x27;&lt;|extra_140|&gt;&#x27;: 151786,
 &#x27;&lt;|extra_141|&gt;&#x27;: 151787,
 &#x27;&lt;|extra_142|&gt;&#x27;: 151788,
 &#x27;&lt;|extra_143|&gt;&#x27;: 151789,
 &#x27;&lt;|extra_144|&gt;&#x27;: 151790,
 &#x27;&lt;|extra_145|&gt;&#x27;: 151791,
 &#x27;&lt;|extra_146|&gt;&#x27;: 151792,
 &#x27;&lt;|extra_147|&gt;&#x27;: 151793,
 &#x27;&lt;|extra_148|&gt;&#x27;: 151794,
 &#x27;&lt;|extra_149|&gt;&#x27;: 151795,
 &#x27;&lt;|extra_150|&gt;&#x27;: 151796,
 &#x27;&lt;|extra_151|&gt;&#x27;: 151797,
 &#x27;&lt;|extra_152|&gt;&#x27;: 151798,
 &#x27;&lt;|extra_153|&gt;&#x27;: 151799,
 &#x27;&lt;|extra_154|&gt;&#x27;: 151800,
 &#x27;&lt;|extra_155|&gt;&#x27;: 151801,
 &#x27;&lt;|extra_156|&gt;&#x27;: 151802,
 &#x27;&lt;|extra_157|&gt;&#x27;: 151803,
 &#x27;&lt;|extra_158|&gt;&#x27;: 151804,
 &#x27;&lt;|extra_159|&gt;&#x27;: 151805,
 &#x27;&lt;|extra_160|&gt;&#x27;: 151806,
 &#x27;&lt;|extra_161|&gt;&#x27;: 151807,
 &#x27;&lt;|extra_162|&gt;&#x27;: 151808,
 &#x27;&lt;|extra_163|&gt;&#x27;: 151809,
 &#x27;&lt;|extra_164|&gt;&#x27;: 151810,
 &#x27;&lt;|extra_165|&gt;&#x27;: 151811,
 &#x27;&lt;|extra_166|&gt;&#x27;: 151812,
 &#x27;&lt;|extra_167|&gt;&#x27;: 151813,
 &#x27;&lt;|extra_168|&gt;&#x27;: 151814,
 &#x27;&lt;|extra_169|&gt;&#x27;: 151815,
 &#x27;&lt;|extra_170|&gt;&#x27;: 151816,
 &#x27;&lt;|extra_171|&gt;&#x27;: 151817,
 &#x27;&lt;|extra_172|&gt;&#x27;: 151818,
 &#x27;&lt;|extra_173|&gt;&#x27;: 151819,
 &#x27;&lt;|extra_174|&gt;&#x27;: 151820,
 &#x27;&lt;|extra_175|&gt;&#x27;: 151821,
 &#x27;&lt;|extra_176|&gt;&#x27;: 151822,
 &#x27;&lt;|extra_177|&gt;&#x27;: 151823,
 &#x27;&lt;|extra_178|&gt;&#x27;: 151824,
 &#x27;&lt;|extra_179|&gt;&#x27;: 151825,
 &#x27;&lt;|extra_180|&gt;&#x27;: 151826,
 &#x27;&lt;|extra_181|&gt;&#x27;: 151827,
 &#x27;&lt;|extra_182|&gt;&#x27;: 151828,
 &#x27;&lt;|extra_183|&gt;&#x27;: 151829,
 &#x27;&lt;|extra_184|&gt;&#x27;: 151830,
 &#x27;&lt;|extra_185|&gt;&#x27;: 151831,
 &#x27;&lt;|extra_186|&gt;&#x27;: 151832,
 &#x27;&lt;|extra_187|&gt;&#x27;: 151833,
 &#x27;&lt;|extra_188|&gt;&#x27;: 151834,
 &#x27;&lt;|extra_189|&gt;&#x27;: 151835,
 &#x27;&lt;|extra_190|&gt;&#x27;: 151836,
 &#x27;&lt;|extra_191|&gt;&#x27;: 151837,
 &#x27;&lt;|extra_192|&gt;&#x27;: 151838,
 &#x27;&lt;|extra_193|&gt;&#x27;: 151839,
 &#x27;&lt;|extra_194|&gt;&#x27;: 151840,
 &#x27;&lt;|extra_195|&gt;&#x27;: 151841,
 &#x27;&lt;|extra_196|&gt;&#x27;: 151842,
 &#x27;&lt;|extra_197|&gt;&#x27;: 151843,
 &#x27;&lt;|extra_198|&gt;&#x27;: 151844,
 &#x27;&lt;|extra_199|&gt;&#x27;: 151845,
 &#x27;&lt;|extra_200|&gt;&#x27;: 151846,
 &#x27;&lt;|extra_201|&gt;&#x27;: 151847,
 &#x27;&lt;|extra_202|&gt;&#x27;: 151848,
 &#x27;&lt;|extra_203|&gt;&#x27;: 151849,
 &#x27;&lt;|extra_204|&gt;&#x27;: 151850}
```

```
tokenizer.special_tokens
```

```
{&#x27;&lt;|endoftext|&gt;&#x27;: 151643,
 &#x27;&lt;|im_start|&gt;&#x27;: 151644,
 &#x27;&lt;|im_end|&gt;&#x27;: 151645,
 &#x27;&lt;|extra_0|&gt;&#x27;: 151646,
 &#x27;&lt;|extra_1|&gt;&#x27;: 151647,
 &#x27;&lt;|extra_2|&gt;&#x27;: 151648,
 &#x27;&lt;|extra_3|&gt;&#x27;: 151649,
 &#x27;&lt;|extra_4|&gt;&#x27;: 151650,
 &#x27;&lt;|extra_5|&gt;&#x27;: 151651,
 &#x27;&lt;|extra_6|&gt;&#x27;: 151652,
 &#x27;&lt;|extra_7|&gt;&#x27;: 151653,
 &#x27;&lt;|extra_8|&gt;&#x27;: 151654,
 &#x27;&lt;|extra_9|&gt;&#x27;: 151655,
 &#x27;&lt;|extra_10|&gt;&#x27;: 151656,
 &#x27;&lt;|extra_11|&gt;&#x27;: 151657,
 &#x27;&lt;|extra_12|&gt;&#x27;: 151658,
 &#x27;&lt;|extra_13|&gt;&#x27;: 151659,
 &#x27;&lt;|extra_14|&gt;&#x27;: 151660,
 &#x27;&lt;|extra_15|&gt;&#x27;: 151661,
 &#x27;&lt;|extra_16|&gt;&#x27;: 151662,
 &#x27;&lt;|extra_17|&gt;&#x27;: 151663,
 &#x27;&lt;|extra_18|&gt;&#x27;: 151664,
 &#x27;&lt;|extra_19|&gt;&#x27;: 151665,
 &#x27;&lt;|extra_20|&gt;&#x27;: 151666,
 &#x27;&lt;|extra_21|&gt;&#x27;: 151667,
 &#x27;&lt;|extra_22|&gt;&#x27;: 151668,
 &#x27;&lt;|extra_23|&gt;&#x27;: 151669,
 &#x27;&lt;|extra_24|&gt;&#x27;: 151670,
 &#x27;&lt;|extra_25|&gt;&#x27;: 151671,
 &#x27;&lt;|extra_26|&gt;&#x27;: 151672,
 &#x27;&lt;|extra_27|&gt;&#x27;: 151673,
 &#x27;&lt;|extra_28|&gt;&#x27;: 151674,
 &#x27;&lt;|extra_29|&gt;&#x27;: 151675,
 &#x27;&lt;|extra_30|&gt;&#x27;: 151676,
 &#x27;&lt;|extra_31|&gt;&#x27;: 151677,
 &#x27;&lt;|extra_32|&gt;&#x27;: 151678,
 &#x27;&lt;|extra_33|&gt;&#x27;: 151679,
 &#x27;&lt;|extra_34|&gt;&#x27;: 151680,
 &#x27;&lt;|extra_35|&gt;&#x27;: 151681,
 &#x27;&lt;|extra_36|&gt;&#x27;: 151682,
 &#x27;&lt;|extra_37|&gt;&#x27;: 151683,
 &#x27;&lt;|extra_38|&gt;&#x27;: 151684,
 &#x27;&lt;|extra_39|&gt;&#x27;: 151685,
 &#x27;&lt;|extra_40|&gt;&#x27;: 151686,
 &#x27;&lt;|extra_41|&gt;&#x27;: 151687,
 &#x27;&lt;|extra_42|&gt;&#x27;: 151688,
 &#x27;&lt;|extra_43|&gt;&#x27;: 151689,
 &#x27;&lt;|extra_44|&gt;&#x27;: 151690,
 &#x27;&lt;|extra_45|&gt;&#x27;: 151691,
 &#x27;&lt;|extra_46|&gt;&#x27;: 151692,
 &#x27;&lt;|extra_47|&gt;&#x27;: 151693,
 &#x27;&lt;|extra_48|&gt;&#x27;: 151694,
 &#x27;&lt;|extra_49|&gt;&#x27;: 151695,
 &#x27;&lt;|extra_50|&gt;&#x27;: 151696,
 &#x27;&lt;|extra_51|&gt;&#x27;: 151697,
 &#x27;&lt;|extra_52|&gt;&#x27;: 151698,
 &#x27;&lt;|extra_53|&gt;&#x27;: 151699,
 &#x27;&lt;|extra_54|&gt;&#x27;: 151700,
 &#x27;&lt;|extra_55|&gt;&#x27;: 151701,
 &#x27;&lt;|extra_56|&gt;&#x27;: 151702,
 &#x27;&lt;|extra_57|&gt;&#x27;: 151703,
 &#x27;&lt;|extra_58|&gt;&#x27;: 151704,
 &#x27;&lt;|extra_59|&gt;&#x27;: 151705,
 &#x27;&lt;|extra_60|&gt;&#x27;: 151706,
 &#x27;&lt;|extra_61|&gt;&#x27;: 151707,
 &#x27;&lt;|extra_62|&gt;&#x27;: 151708,
 &#x27;&lt;|extra_63|&gt;&#x27;: 151709,
 &#x27;&lt;|extra_64|&gt;&#x27;: 151710,
 &#x27;&lt;|extra_65|&gt;&#x27;: 151711,
 &#x27;&lt;|extra_66|&gt;&#x27;: 151712,
 &#x27;&lt;|extra_67|&gt;&#x27;: 151713,
 &#x27;&lt;|extra_68|&gt;&#x27;: 151714,
 &#x27;&lt;|extra_69|&gt;&#x27;: 151715,
 &#x27;&lt;|extra_70|&gt;&#x27;: 151716,
 &#x27;&lt;|extra_71|&gt;&#x27;: 151717,
 &#x27;&lt;|extra_72|&gt;&#x27;: 151718,
 &#x27;&lt;|extra_73|&gt;&#x27;: 151719,
 &#x27;&lt;|extra_74|&gt;&#x27;: 151720,
 &#x27;&lt;|extra_75|&gt;&#x27;: 151721,
 &#x27;&lt;|extra_76|&gt;&#x27;: 151722,
 &#x27;&lt;|extra_77|&gt;&#x27;: 151723,
 &#x27;&lt;|extra_78|&gt;&#x27;: 151724,
 &#x27;&lt;|extra_79|&gt;&#x27;: 151725,
 &#x27;&lt;|extra_80|&gt;&#x27;: 151726,
 &#x27;&lt;|extra_81|&gt;&#x27;: 151727,
 &#x27;&lt;|extra_82|&gt;&#x27;: 151728,
 &#x27;&lt;|extra_83|&gt;&#x27;: 151729,
 &#x27;&lt;|extra_84|&gt;&#x27;: 151730,
 &#x27;&lt;|extra_85|&gt;&#x27;: 151731,
 &#x27;&lt;|extra_86|&gt;&#x27;: 151732,
 &#x27;&lt;|extra_87|&gt;&#x27;: 151733,
 &#x27;&lt;|extra_88|&gt;&#x27;: 151734,
 &#x27;&lt;|extra_89|&gt;&#x27;: 151735,
 &#x27;&lt;|extra_90|&gt;&#x27;: 151736,
 &#x27;&lt;|extra_91|&gt;&#x27;: 151737,
 &#x27;&lt;|extra_92|&gt;&#x27;: 151738,
 &#x27;&lt;|extra_93|&gt;&#x27;: 151739,
 &#x27;&lt;|extra_94|&gt;&#x27;: 151740,
 &#x27;&lt;|extra_95|&gt;&#x27;: 151741,
 &#x27;&lt;|extra_96|&gt;&#x27;: 151742,
 &#x27;&lt;|extra_97|&gt;&#x27;: 151743,
 &#x27;&lt;|extra_98|&gt;&#x27;: 151744,
 &#x27;&lt;|extra_99|&gt;&#x27;: 151745,
 &#x27;&lt;|extra_100|&gt;&#x27;: 151746,
 &#x27;&lt;|extra_101|&gt;&#x27;: 151747,
 &#x27;&lt;|extra_102|&gt;&#x27;: 151748,
 &#x27;&lt;|extra_103|&gt;&#x27;: 151749,
 &#x27;&lt;|extra_104|&gt;&#x27;: 151750,
 &#x27;&lt;|extra_105|&gt;&#x27;: 151751,
 &#x27;&lt;|extra_106|&gt;&#x27;: 151752,
 &#x27;&lt;|extra_107|&gt;&#x27;: 151753,
 &#x27;&lt;|extra_108|&gt;&#x27;: 151754,
 &#x27;&lt;|extra_109|&gt;&#x27;: 151755,
 &#x27;&lt;|extra_110|&gt;&#x27;: 151756,
 &#x27;&lt;|extra_111|&gt;&#x27;: 151757,
 &#x27;&lt;|extra_112|&gt;&#x27;: 151758,
 &#x27;&lt;|extra_113|&gt;&#x27;: 151759,
 &#x27;&lt;|extra_114|&gt;&#x27;: 151760,
 &#x27;&lt;|extra_115|&gt;&#x27;: 151761,
 &#x27;&lt;|extra_116|&gt;&#x27;: 151762,
 &#x27;&lt;|extra_117|&gt;&#x27;: 151763,
 &#x27;&lt;|extra_118|&gt;&#x27;: 151764,
 &#x27;&lt;|extra_119|&gt;&#x27;: 151765,
 &#x27;&lt;|extra_120|&gt;&#x27;: 151766,
 &#x27;&lt;|extra_121|&gt;&#x27;: 151767,
 &#x27;&lt;|extra_122|&gt;&#x27;: 151768,
 &#x27;&lt;|extra_123|&gt;&#x27;: 151769,
 &#x27;&lt;|extra_124|&gt;&#x27;: 151770,
 &#x27;&lt;|extra_125|&gt;&#x27;: 151771,
 &#x27;&lt;|extra_126|&gt;&#x27;: 151772,
 &#x27;&lt;|extra_127|&gt;&#x27;: 151773,
 &#x27;&lt;|extra_128|&gt;&#x27;: 151774,
 &#x27;&lt;|extra_129|&gt;&#x27;: 151775,
 &#x27;&lt;|extra_130|&gt;&#x27;: 151776,
 &#x27;&lt;|extra_131|&gt;&#x27;: 151777,
 &#x27;&lt;|extra_132|&gt;&#x27;: 151778,
 &#x27;&lt;|extra_133|&gt;&#x27;: 151779,
 &#x27;&lt;|extra_134|&gt;&#x27;: 151780,
 &#x27;&lt;|extra_135|&gt;&#x27;: 151781,
 &#x27;&lt;|extra_136|&gt;&#x27;: 151782,
 &#x27;&lt;|extra_137|&gt;&#x27;: 151783,
 &#x27;&lt;|extra_138|&gt;&#x27;: 151784,
 &#x27;&lt;|extra_139|&gt;&#x27;: 151785,
 &#x27;&lt;|extra_140|&gt;&#x27;: 151786,
 &#x27;&lt;|extra_141|&gt;&#x27;: 151787,
 &#x27;&lt;|extra_142|&gt;&#x27;: 151788,
 &#x27;&lt;|extra_143|&gt;&#x27;: 151789,
 &#x27;&lt;|extra_144|&gt;&#x27;: 151790,
 &#x27;&lt;|extra_145|&gt;&#x27;: 151791,
 &#x27;&lt;|extra_146|&gt;&#x27;: 151792,
 &#x27;&lt;|extra_147|&gt;&#x27;: 151793,
 &#x27;&lt;|extra_148|&gt;&#x27;: 151794,
 &#x27;&lt;|extra_149|&gt;&#x27;: 151795,
 &#x27;&lt;|extra_150|&gt;&#x27;: 151796,
 &#x27;&lt;|extra_151|&gt;&#x27;: 151797,
 &#x27;&lt;|extra_152|&gt;&#x27;: 151798,
 &#x27;&lt;|extra_153|&gt;&#x27;: 151799,
 &#x27;&lt;|extra_154|&gt;&#x27;: 151800,
 &#x27;&lt;|extra_155|&gt;&#x27;: 151801,
 &#x27;&lt;|extra_156|&gt;&#x27;: 151802,
 &#x27;&lt;|extra_157|&gt;&#x27;: 151803,
 &#x27;&lt;|extra_158|&gt;&#x27;: 151804,
 &#x27;&lt;|extra_159|&gt;&#x27;: 151805,
 &#x27;&lt;|extra_160|&gt;&#x27;: 151806,
 &#x27;&lt;|extra_161|&gt;&#x27;: 151807,
 &#x27;&lt;|extra_162|&gt;&#x27;: 151808,
 &#x27;&lt;|extra_163|&gt;&#x27;: 151809,
 &#x27;&lt;|extra_164|&gt;&#x27;: 151810,
 &#x27;&lt;|extra_165|&gt;&#x27;: 151811,
 &#x27;&lt;|extra_166|&gt;&#x27;: 151812,
 &#x27;&lt;|extra_167|&gt;&#x27;: 151813,
 &#x27;&lt;|extra_168|&gt;&#x27;: 151814,
 &#x27;&lt;|extra_169|&gt;&#x27;: 151815,
 &#x27;&lt;|extra_170|&gt;&#x27;: 151816,
 &#x27;&lt;|extra_171|&gt;&#x27;: 151817,
 &#x27;&lt;|extra_172|&gt;&#x27;: 151818,
 &#x27;&lt;|extra_173|&gt;&#x27;: 151819,
 &#x27;&lt;|extra_174|&gt;&#x27;: 151820,
 &#x27;&lt;|extra_175|&gt;&#x27;: 151821,
 &#x27;&lt;|extra_176|&gt;&#x27;: 151822,
 &#x27;&lt;|extra_177|&gt;&#x27;: 151823,
 &#x27;&lt;|extra_178|&gt;&#x27;: 151824,
 &#x27;&lt;|extra_179|&gt;&#x27;: 151825,
 &#x27;&lt;|extra_180|&gt;&#x27;: 151826,
 &#x27;&lt;|extra_181|&gt;&#x27;: 151827,
 &#x27;&lt;|extra_182|&gt;&#x27;: 151828,
 &#x27;&lt;|extra_183|&gt;&#x27;: 151829,
 &#x27;&lt;|extra_184|&gt;&#x27;: 151830,
 &#x27;&lt;|extra_185|&gt;&#x27;: 151831,
 &#x27;&lt;|extra_186|&gt;&#x27;: 151832,
 &#x27;&lt;|extra_187|&gt;&#x27;: 151833,
 &#x27;&lt;|extra_188|&gt;&#x27;: 151834,
 &#x27;&lt;|extra_189|&gt;&#x27;: 151835,
 &#x27;&lt;|extra_190|&gt;&#x27;: 151836,
 &#x27;&lt;|extra_191|&gt;&#x27;: 151837,
 &#x27;&lt;|extra_192|&gt;&#x27;: 151838,
 &#x27;&lt;|extra_193|&gt;&#x27;: 151839,
 &#x27;&lt;|extra_194|&gt;&#x27;: 151840,
 &#x27;&lt;|extra_195|&gt;&#x27;: 151841,
 &#x27;&lt;|extra_196|&gt;&#x27;: 151842,
 &#x27;&lt;|extra_197|&gt;&#x27;: 151843,
 &#x27;&lt;|extra_198|&gt;&#x27;: 151844,
 &#x27;&lt;|extra_199|&gt;&#x27;: 151845,
 &#x27;&lt;|extra_200|&gt;&#x27;: 151846,
 &#x27;&lt;|extra_201|&gt;&#x27;: 151847,
 &#x27;&lt;|extra_202|&gt;&#x27;: 151848,
 &#x27;&lt;|extra_203|&gt;&#x27;: 151849,
 &#x27;&lt;|extra_204|&gt;&#x27;: 151850}
```

了解了对话格式和特殊Token标识后，我们能明确的就是应该把输入的自然语言形式的Prompt处理成哪种形式，其实也就是ChatML格式，然后送入Qwen模型执行推理过程，那我们就可以先把大的框架搭建起来，一步一步的实现这个过程。

首先定义一个类，其核心就是__init__方法和chat方法，前者用来初始化模型和配置，后者是实现对话交互的主函数。

```
class myvLLMWrapper:
    def __init__(self):
        &quot;&quot;&quot;
        :param
        :param
        :return
        :description 模型初始化配置
        &quot;&quot;&quot;
        pass


    def chat(self, prompt, history=None,system=&quot;你是一个乐于助人的智能机器人。&quot;):
        &quot;&quot;&quot;
        :param 
        :param 
        :return: 
        :description 大模型交互主逻辑
        &quot;&quot;&quot;
        pass
```

第一步：加载大模型的三个常规操作：model、tokenizer和config，这部分在__init__函数中写：

```
# 导入Transformer依赖库
from transformers import AutoModelForCausalLM, AutoTokenizer
from transformers.generation import GenerationConfig
from vllm import LLM


class myvLLMWrapper:
    def __init__(self, model_dir):
        &quot;&quot;&quot;
        :param model_dir ： 大模型实际存储位置
        :param
        :return: Description of return value
        &quot;&quot;&quot;

        # 加载配置，即Qweb模型目录下的generation_config.json文件，是推理的关键参数
        self.generation_config = GenerationConfig.from_pretrained(model_dir, trust_remote_code=True)

        # 加载分词器
        self.tokenizer = AutoTokenizer.from_pretrained(model_dir, trust_remote_code=True)

        # 使用vLLM框架加载模型
        self.model = LLM(model=model_dir, tokenizer=model_dir, trust_remote_code=True)

    def chat(self, prompt, history=None, system=&quot;You are a helpful assistant.&quot;, extra_stop_words_ids=[]):
        &quot;&quot;&quot;
        :param
        :param
        :return: Description of return value
        &quot;&quot;&quot;
        pass
```

完成模型启动的配置代码后，先测试一下该流程是否能够正常加载模型：

为了加速测试流程，我这里下载了qwen1.8b的int模型，下面这段代码可直接下载，大家也可以尝试使用小模型来测试代码流程，提升效率。

```
# from modelscope.hub.snapshot_download import snapshot_download

# model_dir = snapshot_download(&#x27;qwen/Qwen-1_8B-Chat-Int4&#x27;,cache_dir=&#x27;./models&#x27;,revision=&#x27;master&#x27;)
```

> 如果报错：AssertionError: tensor model parallel group is already initialized， 原因是前面的代码已经加载模型，需要重启Notebook再次执行，释放资源后重新加载模型

```
model_dir = &quot;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat-Int4/&quot;

vllm_model = myvLLMWrapper(model_dir)
```

```
WARNING 01-30 19:00:30 config.py:140] gptq quantization is not fully optimized yet. The speed can be slower than non-quantized models.
INFO 01-30 19:00:30 llm_engine.py:72] Initializing an LLM engine with config: model=&#x27;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat-Int4/&#x27;, tokenizer=&#x27;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat-Int4/&#x27;, tokenizer_mode=auto, revision=None, tokenizer_revision=None, trust_remote_code=True, dtype=torch.float16, max_seq_len=8192, download_dir=None, load_format=auto, tensor_parallel_size=1, quantization=gptq, seed=0)
WARNING 01-30 19:00:31 tokenizer.py:66] Using a slow tokenizer. This might cause a significant slowdown. Consider using a fast tokenizer instead.
INFO 01-30 19:01:05 llm_engine.py:219] # GPU blocks: 5932, # CPU blocks: 1365
```

可以看到，Qwen-7B的模型，此时已经可以通过vLLM框架来托管启动。那么接下来，需要做的就是如何与已经启动的Qwen-7B-Chat模型来进行对话交互，即实现Chat()方法。

首先，需要进行Pompt的预处理工作，也就是将你好呀，请你介绍一下你自己 、 好的，我想吃点甜食，你能不能帮我推荐一些好吃的食物？ 这种自然语言的Prompt，格式化成Qwen期望输入的ChatML类型的对话格式。即：

```
输入：&lt;|im_start|&gt;system
      你是一个智能助手。&lt;|im_end|&gt;                 （这一部分是系统信息）
      
      &lt;|im_start|&gt;user
      你好呀，请你介绍一下你自己&lt;|im_end|&gt;
      &lt;|im_start|&gt;assitant
      我是Qwen-Chat模型，一个非常厉害的人工智能助手，你可以向我提出任意的提问&lt;|im_end|&gt;
      &lt;|im_start|&gt;user
      好的，我想吃点甜食，你能不能帮我推荐一些好吃的食物？&lt;|im_end|&gt;
      &lt;|im_start|&gt;assitant
      当然可以，如果你喜欢吃甜食，那么推荐你吃巧克力的甜甜圈，太好吃了&lt;|im_end|&gt;       （这一部分是历史对话信息）
      
      &lt;|im_start|&gt;user
      那么去哪里可以买到呢？&lt;|im_end|&gt;
       &lt;|im_start|&gt;assitant&lt;|endoftext|&gt;    （这一部分是最新一轮的用户提问）

输出：你可以去超市、蛋糕店等地方买到这个美味的食物&lt;|im_end|&gt;&lt;|endoftext|&gt;     （这不部分是Qwen模型针对最新一轮的提问，结合历史内容生成的回复）
```

在数据格式预处理过程中，我们可以分步来操作，首先来处理system部分，即：

```
&lt;|im_start|&gt;system
      你是一个智能助手。&lt;|im_end|&gt; 
```

这里有几个关键的Token，分别是：<|im_start|>、<|im_end|> 以及隐藏的 \n。 可以通过tokenizer.encode()方法，找到它们对应的input_ids：

```
# 初始化tokenizer
tokenizer = AutoTokenizer.from_pretrained(&quot;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-7B-Chat/&quot;, trust_remote_code=True,)
```

```
tokenizer.encode(&quot;\n&quot;)
```

```
[198]
```

```
tokenizer.encode(&#x27;system&#x27;)
```

```
[8948]
```

```
tokenizer.encode(&#x27;user&#x27;)
```

```
[872]
```

```
tokenizer.encode(&#x27;你是一个智能助手。&#x27;)
```

```
[56568, 101909, 100168, 110498, 1773]
```

```
role=&#x27;system&#x27;
prompt = &#x27;你是一个智能助手&#x27;
```

```
print(f&quot;{role}\n{prompt}&quot;,tokenizer.encode(&#x27;system&#x27;) + tokenizer.encode(&#x27;\n&#x27;) + tokenizer.encode(&#x27;你是一个智能助手。&#x27;))
```

```
system
你是一个智能助手 [8948, 198, 56568, 101909, 100168, 110498, 1773]
```

根据上述测试，我们可以定义一个函数，用来将 输入的 role 和 prompt 拼接起来。

```
def _format_tokenize(role, prompt):
    return f&quot;{role}\n{prompt}&quot;,tokenizer.encode(role) + tokenizer.encode(&#x27;\n&#x27;) + tokenizer.encode(prompt)
```

测试一下：

```
system_text, system_tokens = _format_tokenize(&#x27;system&#x27;, &#x27;你是一个智能助手&#x27;)
```

```
system_text
```

```
&#x27;system\n你是一个智能助手&#x27;
```

```
system_tokens
```

```
[8948, 198, 56568, 101909, 100168, 110498]
```

加入特殊字符<|im_start|>和<|im_end|>

```
im_start,im_start_tokens=&#x27;&lt;|im_start|&gt;&#x27;,[tokenizer.im_start_id]
im_end,im_end_tokens=&#x27;&lt;|im_end|&gt;&#x27;,[tokenizer.im_end_id]
```

```
system_full = f&#x27;{im_start}{system_text}{im_end}&#x27;
system_full
```

```
&#x27;&lt;|im_start|&gt;system\n你是一个智能助手&lt;|im_end|&gt;&#x27;
```

```
system_tokens_full = im_start_tokens + system_tokens + im_end_tokens
system_tokens_full
```

```
[151644, 8948, 198, 56568, 101909, 100168, 110498, 151645]
```

第一部分测试完成，我们把这部分的代码逻辑写进myvLLMWrapper类中，并测试一下system部分的生成情况。

这里在myvLLMWrapper类中新定义一个format_prompt方法，用来处理这个逻辑。

```
# 导入Transformer依赖库
from transformers import AutoModelForCausalLM, AutoTokenizer
from transformers.generation import GenerationConfig
from vllm import LLM


class myvLLMWrapper:
    def __init__(self, model_dir):
        &quot;&quot;&quot;
        :param model_dir ： 大模型实际存储位置
        :param
        :return: Description of return value
        &quot;&quot;&quot;

        # 加载配置，即Qweb模型目录下的generation_config.json文件，是推理的关键参数
        self.generation_config = GenerationConfig.from_pretrained(model_dir, trust_remote_code=True)

        # 加载分词器
        self.tokenizer = AutoTokenizer.from_pretrained(model_dir, trust_remote_code=True)

        # 使用vLLM框架加载模型
        self.model = LLM(model=model_dir, tokenizer=model_dir, trust_remote_code=True)

        # 加入特殊Token
        self.tokenizer.eos_token_id=self.generation_config.eos_token_id
        # 推理终止词，遇到这些词停止继续推理
        self.stop_words_ids=[self.tokenizer.im_start_id,self.tokenizer.im_end_id,self.tokenizer.eos_token_id]
    
        # 可用的Tokens数量，用来控制单次最大输入Tokens
        self.token_space=self.generation_config.max_window_size


    def format_prompt(self, tokenizer, prompt, history=None, system=&quot;&quot;):
        
        im_start, im_start_tokens = &#x27;&lt;|im_start|&gt;&#x27;, [tokenizer.im_start_id]
        im_end, im_end_tokens = &#x27;&lt;|im_end|&gt;&#x27;, [tokenizer.im_end_id]

        def _format_tokenize(role, prompt):
            &quot;&quot;&quot;
            :param role:system
            :param prompt:你是一个智能助手
            :return:(&#x27;{role}\n{content}&#x27;, [8948, 198, 56568, 101909, 100168, 110498])
            &quot;&quot;&quot;
            return f&quot;{role}\n{prompt}&quot;, tokenizer.encode(role) + tokenizer.encode(&#x27;\n&#x27;) + tokenizer.encode(prompt)


        # System 角色的处理逻辑
        system_text, system_tokens = _format_tokenize(&#x27;system&#x27;, system)
        # 假设系统角色输入为：You are a helpful assistant， 经过这一步，会变成  &lt;|im_start|&gt;system
        #                                                                            You are a helpful assistant.&lt;|im_end|&gt;
        system_text_full = f&#x27;{im_start}{system_text}{im_end}&#x27;

        # 这里是其对应的编码：[151644, 8948, 198, 2610, 525, 264, 10950, 17847, 13, 151645]
        system_tokens_full = im_start_tokens + system_tokens + im_end_tokens
        self.token_space -= len(system_tokens_full)
        
        print(system_text_full) 
        print(system_tokens_full)
    

    def chat(self, prompt, history=None, system=&quot;You are a helpful assistant.&quot;):
        &quot;&quot;&quot;
        :param
        :param
        :return: Description of return value
        &quot;&quot;&quot;
        prompt_text, prompt_tokens = self.format_prompt(self.tokenizer, prompt, system=system)
```

```
/home/util/anaconda3/envs/qwen_7b_chat/lib/python3.11/site-packages/tqdm/auto.py:21: TqdmWarning: IProgress not found. Please update jupyter and ipywidgets. See https://ipywidgets.readthedocs.io/en/stable/user_install.html
  from .autonotebook import tqdm as notebook_tqdm
2024-01-30 21:43:10,695        INFO util.py:154 -- Missing packages: [&#x27;ipywidgets&#x27;]. Run `pip install -U ipywidgets`, then restart the notebook server for rich notebook output.
2024-01-30 21:43:11,229        INFO util.py:154 -- Missing packages: [&#x27;ipywidgets&#x27;]. Run `pip install -U ipywidgets`, then restart the notebook server for rich notebook output.
```

```
model_dir = &quot;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&quot;

vllm_model = myvLLMWrapper(model_dir)

vllm_model.chat(prompt=&quot;你好，请你介绍一下你自己&quot;, system=&#x27;你是一个智能助手。&#x27;)
```

```
INFO 01-30 21:43:14 llm_engine.py:72] Initializing an LLM engine with config: model=&#x27;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&#x27;, tokenizer=&#x27;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&#x27;, tokenizer_mode=auto, revision=None, tokenizer_revision=None, trust_remote_code=True, dtype=torch.float16, max_seq_len=8192, download_dir=None, load_format=auto, tensor_parallel_size=1, quantization=None, seed=0)
WARNING 01-30 21:43:14 tokenizer.py:66] Using a slow tokenizer. This might cause a significant slowdown. Consider using a fast tokenizer instead.
INFO 01-30 21:44:15 llm_engine.py:219] # GPU blocks: 5350, # CPU blocks: 1365
&lt;|im_start|&gt;system
你是一个智能助手。&lt;|im_end|&gt;
[151644, 8948, 198, 56568, 101909, 100168, 110498, 1773, 151645]



---------------------------------------------------------------------------

TypeError                                 Traceback (most recent call last)

Cell In[2], line 5
      1 model_dir = &quot;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&quot;
      3 vllm_model = myvLLMWrapper(model_dir)
----&gt; 5 vllm_model.chat(prompt=&quot;你好，请你介绍一下你自己&quot;, system=&#x27;你是一个智能助手。&#x27;)


Cell In[1], line 67, in myvLLMWrapper.chat(self, prompt, history, system)
     61 def chat(self, prompt, history=None, system=&quot;You are a helpful assistant.&quot;):
     62     &quot;&quot;&quot;
     63     :param
     64     :param
     65     :return: Description of return value
     66     &quot;&quot;&quot;
---&gt; 67     prompt_text, prompt_tokens = self.format_prompt(self.tokenizer, prompt, system=system)


TypeError: cannot unpack non-iterable NoneType object
```

忽略报错，这里先测试system_text_full和system_tokens_full是否可以正常输出，可以看到格式完全是匹配的。

接下来我们来看第二部分：

```
      &lt;|im_start|&gt;user
      你好呀，请你介绍一下你自己&lt;|im_end|&gt;
      &lt;|im_start|&gt;assitant   
```

```
def _format_tokenize(role, prompt):
    &quot;&quot;&quot;
    :param role:system
    :param prompt:你是一个智能助手
    :return:(&#x27;{role}\n{content}&#x27;, [8948, 198, 56568, 101909, 100168, 110498])
    &quot;&quot;&quot;
    return f&quot;{role}\n{prompt}&quot;, tokenizer.encode(role) + tokenizer.encode(&#x27;\n&#x27;) + tokenizer.encode(prompt)
```

```
from transformers import AutoTokenizer

# 初始化tokenizer
tokenizer = AutoTokenizer.from_pretrained(&quot;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-7B-Chat/&quot;, trust_remote_code=True,)

user_text, user_tokens = _format_tokenize(&#x27;user&#x27;, &quot;你好呀，请你介绍一下你自己&quot;)
print(user_text, user_tokens)
```

```
user
你好呀，请你介绍一下你自己 [872, 198, 108386, 104256, 37945, 56568, 109432, 107828]
```

```
im_start, im_start_tokens = &#x27;&lt;|im_start|&gt;&#x27;, [tokenizer.im_start_id]
im_end, im_end_tokens = &#x27;&lt;|im_end|&gt;&#x27;, [tokenizer.im_end_id]

user_text_full = f&quot;\n{im_start}{user_text}{im_end}\n{im_start}assistant\n&quot;
user_text_full
```

```
&#x27;\n&lt;|im_start|&gt;user\n你好呀，请你介绍一下你自己&lt;|im_end|&gt;\n&lt;|im_start|&gt;assistant\n&#x27;
```

```
user_tokens_prefix = tokenizer.encode(&quot;\n&quot;) + im_start_tokens
print(user_tokens_prefix)
user_tokens_suffix = im_end_tokens + tokenizer.encode(&quot;\n&quot;) + im_start_tokens + tokenizer.encode(&#x27;assistant&#x27;) + tokenizer.encode(&quot;\n&quot;)
print(user_tokens_suffix)
```

```
[198, 151644]
[151645, 198, 151644, 77091, 198]
```

```
user_tokens_full = user_tokens_prefix + user_tokens + user_tokens_suffix

print(user_tokens_full)
```

```
[198, 151644, 872, 198, 108386, 104256, 37945, 56568, 109432, 107828, 151645, 198, 151644, 77091, 198]
```

```
# 导入Transformer依赖库
from vllm import LLM
from vllm.sampling_params import SamplingParams
from transformers import AutoModelForCausalLM, AutoTokenizer
from transformers.generation import GenerationConfig



class myvLLMWrapper:
    def __init__(self, model_dir):
        &quot;&quot;&quot;
        :param model_dir ： 大模型实际存储位置
        :param
        :return: Description of return value
        &quot;&quot;&quot;

        # 加载配置，即Qweb模型目录下的generation_config.json文件，是推理的关键参数
        self.generation_config = GenerationConfig.from_pretrained(model_dir, trust_remote_code=True)

        # 加载分词器
        self.tokenizer = AutoTokenizer.from_pretrained(model_dir, trust_remote_code=True)

        # 使用vLLM框架加载模型
        self.model = LLM(model=model_dir, tokenizer=model_dir, trust_remote_code=True)

        # 加入特殊Token
        self.tokenizer.eos_token_id=self.generation_config.eos_token_id
        # 推理终止词，遇到这些词停止继续推理
        self.stop_words_ids=[self.tokenizer.im_start_id,self.tokenizer.im_end_id,self.tokenizer.eos_token_id]
    
        # 可用的Tokens数量，用来控制单次最大输入Tokens
        self.token_space=self.generation_config.max_window_size


    def format_prompt(self, tokenizer, prompt, history=None, system=&quot;&quot;):
        
        im_start, im_start_tokens = &#x27;&lt;|im_start|&gt;&#x27;, [tokenizer.im_start_id]
        im_end, im_end_tokens = &#x27;&lt;|im_end|&gt;&#x27;, [tokenizer.im_end_id]

        def _format_tokenize(role, prompt):
            &quot;&quot;&quot;
            :param role:system
            :param prompt:你是一个智能助手
            :return:(&#x27;{role}\n{content}&#x27;, [8948, 198, 56568, 101909, 100168, 110498])
            &quot;&quot;&quot;
            return f&quot;{role}\n{prompt}&quot;, tokenizer.encode(role) + tokenizer.encode(&#x27;\n&#x27;) + tokenizer.encode(prompt)


        # System 角色的处理逻辑
        system_text, system_tokens = _format_tokenize(&#x27;system&#x27;, system)
        # 假设系统角色输入为：You are a helpful assistant， 经过这一步，会变成  &lt;|im_start|&gt;system
        #                                                                            You are a helpful assistant.&lt;|im_end|&gt;
        system_text_full = f&#x27;{im_start}{system_text}{im_end}&#x27;

        # 这里是其对应的编码：[151644, 8948, 198, 2610, 525, 264, 10950, 17847, 13, 151645]
        system_tokens_full = im_start_tokens + system_tokens + im_end_tokens
        self.token_space -= len(system_tokens_full)
        
        # User 角色的处理逻辑
        user_text, user_tokens = _format_tokenize(&#x27;user&#x27;, prompt)
        user_tokens_prefix = tokenizer.encode(&quot;\n&quot;) + im_start_tokens
        user_tokens_suffix = im_end_tokens + tokenizer.encode(&quot;\n&quot;) + im_start_tokens + tokenizer.encode(&#x27;assistant&#x27;) + tokenizer.encode(&quot;\n&quot;)

        # 判断一下，避免输入的Prompt过长，过长就截断
        if len(user_tokens_prefix) + len(user_tokens) + len(user_tokens_suffix) &gt; self.token_space: 
            user_tokens_len= self.token_space - len(user_tokens_prefix) - len(user_tokens_suffix)
            user_tokens = user_tokens[:user_tokens_len]
            user_text = self.tokenizer.decode(user_tokens)
            
        user_text_full = f&quot;\n{im_start}{user_text}{im_end}\n{im_start}assistant\n&quot;
        user_tokens_full = user_tokens_prefix + user_tokens + user_tokens_suffix
        
        self.token_space -= len(user_tokens_full)


        prompt_str = f&#x27;{system_text_full}{user_text_full}&#x27;
        prompt_tokens = system_tokens_full + user_tokens_full
        return prompt_str,prompt_tokens

    def chat(self, prompt, history=None, system=&quot;You are a helpful assistant.&quot;):
        &quot;&quot;&quot;
        :param
        :param
        :return: Description of return value
        &quot;&quot;&quot;
        prompt_text, prompt_tokens = self.format_prompt(self.tokenizer, prompt, system=system)
        
        print(prompt_text)       
```

```
/home/util/anaconda3/envs/qwen_7b_chat/lib/python3.11/site-packages/tqdm/auto.py:21: TqdmWarning: IProgress not found. Please update jupyter and ipywidgets. See https://ipywidgets.readthedocs.io/en/stable/user_install.html
  from .autonotebook import tqdm as notebook_tqdm
2024-01-30 22:16:13,498        INFO util.py:154 -- Missing packages: [&#x27;ipywidgets&#x27;]. Run `pip install -U ipywidgets`, then restart the notebook server for rich notebook output.
2024-01-30 22:16:14,024        INFO util.py:154 -- Missing packages: [&#x27;ipywidgets&#x27;]. Run `pip install -U ipywidgets`, then restart the notebook server for rich notebook output.
```

```
model_dir = &quot;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&quot;

vllm_model = myvLLMWrapper(model_dir)

vllm_model.chat(prompt=&quot;你好，请你介绍一下你自己&quot;, system=&#x27;你是一个智能助手。&#x27;)
```

```
INFO 01-30 22:16:19 llm_engine.py:72] Initializing an LLM engine with config: model=&#x27;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&#x27;, tokenizer=&#x27;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&#x27;, tokenizer_mode=auto, revision=None, tokenizer_revision=None, trust_remote_code=True, dtype=torch.float16, max_seq_len=8192, download_dir=None, load_format=auto, tensor_parallel_size=1, quantization=None, seed=0)
WARNING 01-30 22:16:19 tokenizer.py:66] Using a slow tokenizer. This might cause a significant slowdown. Consider using a fast tokenizer instead.
INFO 01-30 22:17:24 llm_engine.py:219] # GPU blocks: 5350, # CPU blocks: 1365
&lt;|im_start|&gt;system
你是一个智能助手。&lt;|im_end|&gt;
&lt;|im_start|&gt;user
你好，请你介绍一下你自己&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant
```

实现一条完整的对话格式后，接下来我们进行模型推理部分的代码，即开始编写chat()函数。

```
# 导入Transformer依赖库
from vllm import LLM
from vllm.sampling_params import SamplingParams
from transformers import AutoModelForCausalLM, AutoTokenizer
from transformers.generation import GenerationConfig

class myvLLMWrapper:
    def __init__(self, model_dir):
        &quot;&quot;&quot;
        :param model_dir ： 大模型实际存储位置
        :param
        :return: Description of return value
        &quot;&quot;&quot;

        # 加载配置，即Qweb模型目录下的generation_config.json文件，是推理的关键参数
        self.generation_config = GenerationConfig.from_pretrained(model_dir, trust_remote_code=True)

        # 加载分词器
        self.tokenizer = AutoTokenizer.from_pretrained(model_dir, trust_remote_code=True)

        # 使用vLLM框架加载模型
        self.model = LLM(model=model_dir, tokenizer=model_dir, trust_remote_code=True)

        # 加入特殊Token
        self.tokenizer.eos_token_id=self.generation_config.eos_token_id
        # 推理终止词，遇到这些词停止继续推理
        self.stop_words_ids=[self.tokenizer.im_start_id,self.tokenizer.im_end_id,self.tokenizer.eos_token_id]
    
        # 可用的Tokens数量，用来控制单次最大输入Tokens
        self.token_space=self.generation_config.max_window_size


    def format_prompt(self, tokenizer, prompt, history=None, system=&quot;&quot;):
        
        im_start, im_start_tokens = &#x27;&lt;|im_start|&gt;&#x27;, [tokenizer.im_start_id]
        im_end, im_end_tokens = &#x27;&lt;|im_end|&gt;&#x27;, [tokenizer.im_end_id]

        def _format_tokenize(role, prompt):
            &quot;&quot;&quot;
            :param role:system
            :param prompt:你是一个智能助手
            :return:(&#x27;{role}\n{content}&#x27;, [8948, 198, 56568, 101909, 100168, 110498])
            &quot;&quot;&quot;
            return f&quot;{role}\n{prompt}&quot;, tokenizer.encode(role) + tokenizer.encode(&#x27;\n&#x27;) + tokenizer.encode(prompt)


        # System 角色的处理逻辑
        system_text, system_tokens = _format_tokenize(&#x27;system&#x27;, system)
        # 假设系统角色输入为：You are a helpful assistant， 经过这一步，会变成  &lt;|im_start|&gt;system
        #                                                                            You are a helpful assistant.&lt;|im_end|&gt;
        system_text_full = f&#x27;{im_start}{system_text}{im_end}&#x27;

        # 这里是其对应的编码：[151644, 8948, 198, 2610, 525, 264, 10950, 17847, 13, 151645]
        system_tokens_full = im_start_tokens + system_tokens + im_end_tokens
        self.token_space -= len(system_tokens_full)
        
        # User 角色的处理逻辑
        user_text, user_tokens = _format_tokenize(&#x27;user&#x27;, prompt)
        user_tokens_prefix = tokenizer.encode(&quot;\n&quot;) + im_start_tokens
        user_tokens_suffix = im_end_tokens + tokenizer.encode(&quot;\n&quot;) + im_start_tokens + tokenizer.encode(&#x27;assistant&#x27;) + tokenizer.encode(&quot;\n&quot;)

        # 判断一下，避免输入的Prompt过长，过长就截断
        if len(user_tokens_prefix) + len(user_tokens) + len(user_tokens_suffix) &gt; self.token_space: 
            user_tokens_len= self.token_space - len(user_tokens_prefix) - len(user_tokens_suffix)
            user_tokens = user_tokens[:user_tokens_len]
            user_text = self.tokenizer.decode(user_tokens)
            
        user_text_full = f&quot;\n{im_start}{user_text}{im_end}\n{im_start}assistant\n&quot;
        user_tokens_full = user_tokens_prefix + user_tokens + user_tokens_suffix
        
        self.token_space -= len(user_tokens_full)


        prompt_str = f&#x27;{system_text_full}{user_text_full}&#x27;
        prompt_tokens = system_tokens_full + user_tokens_full
        return prompt_str,prompt_tokens

    def chat(self, prompt, history=None, system=&quot;You are a helpful assistant.&quot;):
        &quot;&quot;&quot;
        :param
        :param
        :return: Description of return value
        &quot;&quot;&quot;
        prompt_text, prompt_tokens = self.format_prompt(self.tokenizer, prompt, system=system)
        
        print(prompt_text)

        # VLLM请求配置
        sampling_params=SamplingParams(stop_token_ids=self.stop_words_ids)
        
        # 调用VLLM执行推理
        vllm_outputs=self.model.generate(prompt_token_ids=[prompt_tokens],sampling_params=sampling_params)

        # 返回最终结果
        response= vllm_outputs[0].outputs[0].text
        
        print(response)
```

```
/home/util/anaconda3/envs/qwen_7b_chat/lib/python3.11/site-packages/tqdm/auto.py:21: TqdmWarning: IProgress not found. Please update jupyter and ipywidgets. See https://ipywidgets.readthedocs.io/en/stable/user_install.html
  from .autonotebook import tqdm as notebook_tqdm
2024-01-30 22:17:45,376        INFO util.py:154 -- Missing packages: [&#x27;ipywidgets&#x27;]. Run `pip install -U ipywidgets`, then restart the notebook server for rich notebook output.
2024-01-30 22:17:45,888        INFO util.py:154 -- Missing packages: [&#x27;ipywidgets&#x27;]. Run `pip install -U ipywidgets`, then restart the notebook server for rich notebook output.
```

```
model_dir = &quot;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&quot;

vllm_model = myvLLMWrapper(model_dir)

vllm_model.chat(prompt=&quot;你好，请你介绍一下你自己&quot;, system=&#x27;你是一个智能助手。&#x27;)
```

```
INFO 01-30 22:17:58 llm_engine.py:72] Initializing an LLM engine with config: model=&#x27;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&#x27;, tokenizer=&#x27;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&#x27;, tokenizer_mode=auto, revision=None, tokenizer_revision=None, trust_remote_code=True, dtype=torch.float16, max_seq_len=8192, download_dir=None, load_format=auto, tensor_parallel_size=1, quantization=None, seed=0)
WARNING 01-30 22:17:58 tokenizer.py:66] Using a slow tokenizer. This might cause a significant slowdown. Consider using a fast tokenizer instead.
INFO 01-30 22:19:02 llm_engine.py:219] # GPU blocks: 5350, # CPU blocks: 1365
&lt;|im_start|&gt;system
你是一个智能助手。&lt;|im_end|&gt;
&lt;|im_start|&gt;user
你好，请你介绍一下你自己&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant



Processed prompts: 100%|██████████| 1/1 [00:00&lt;00:00,  7.37it/s]

我是阿里云研发的大规模语言模型，我叫通义千问。
```

```
vllm_model.chat(prompt=&quot;能帮我推荐几个北京的美食吗&quot;, system=&#x27;你是一个美食家&#x27;)
```

```
&lt;|im_start|&gt;system
你是一个美食家&lt;|im_end|&gt;
&lt;|im_start|&gt;user
能帮我推荐几个北京的美食吗&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant



Processed prompts: 100%|██████████| 1/1 [00:00&lt;00:00,  5.16it/s]

北京在烹饪方面拥有多样的传统美食，如北京烤鸭、炸
```

但这里发现问题，在模型生成的内容中会被截断，主要原因在于vLLM请求配置，我们这里丰富一下参数，只需要加这样一个参数 max_tokens

| 参数 | 描述 |
| --- | --- |
| n | 返回给定提示的输出序列数量。 |
| best_of | 从提示生成的输出序列的数量。从这些best_of序列中，返回前n个序列。best_of必须大于或等于n。默认情况下，best_of设置为n。 |
| presence_penalty | 根据生成的文本中迄今为止出现的新词汇来惩罚新词汇的浮点数。大于0的值鼓励模型使用新词汇，小于0的值鼓励模型重复词汇。 |
| frequency_penalty | 根据生成的文本中迄今为止新词汇的频率来惩罚新词汇的浮点数。大于0的值鼓励模型使用新词汇，小于0的值鼓励模型重复词汇。 |
| repetition_penalty | 根据新词汇是否出现在提示和迄今为止生成的文本中来惩罚新词汇的浮点数。大于1的值鼓励模型使用新词汇，小于1的值鼓励模型重复词汇。 |
| temperature | 控制抽样随机性的浮点数。较低的值使模型更确定性，较高的值使模型更随机。零意味着贪婪抽样。 |
| top_p | 控制考虑的顶级词汇的累积概率的浮点数。必须在(0, 1]之间。设置为1以考虑所有词汇。 |
| top_k | 控制考虑的顶级词汇数量的整数。设置为-1以考虑所有词汇。 |
| min_p | 表示相对于最有可能的词汇的概率的最小概率的浮点数。必须在[0, 1]之间。设置为0以禁用此功能。 |
| use_beam_search | 是否使用束搜索而不是抽样。 |
| length_penalty | 根据序列的长度惩罚序列的浮点数。在束搜索中使用。 |
| early_stopping | 控制束搜索的停止条件。可接受的值包括：True（生成停止，一旦有best_of个完整候选者）；False（应用启发式方法，当发现更好的候选者非常不可能时停止生成）；&quot;never&quot;（只有在不存在更好的候选者时束搜索过程才停止）。 |
| stop | 停止生成时生成的字符串列表。返回的输出不会包含停止字符串。 |
| stop_token_ids | 停止生成时生成的令牌列表。返回的输出将包含停止令牌，除非停止令牌是特殊令牌。 |
| include_stop_str_in_output | 是否在输出文本中包含停止字符串。默认为False。 |
| ignore_eos | 是否忽略EOS令牌，并在生成EOS令牌后继续生成令牌。 |
| max_tokens | 每个输出序列生成的最大令牌数。 |
| logprobs | 每个输出词汇返回的对数概率数量。 |
| prompt_logprobs | 每个提示词汇返回的对数概率数量。 |
| skip_special_tokens | 是否跳过输出中的特殊令牌。 |
| spaces_between_special_tokens | 是否在输出中的特殊令牌之间添加空格。默认为 |

```
# 导入Transformer依赖库
from vllm import LLM
from vllm.sampling_params import SamplingParams
from transformers import AutoModelForCausalLM, AutoTokenizer
from transformers.generation import GenerationConfig

class myvLLMWrapper:
    def __init__(self, model_dir):
        &quot;&quot;&quot;
        :param model_dir ： 大模型实际存储位置
        :param
        :return: Description of return value
        &quot;&quot;&quot;

        # 加载配置，即Qweb模型目录下的generation_config.json文件，是推理的关键参数
        self.generation_config = GenerationConfig.from_pretrained(model_dir, trust_remote_code=True)

        # 加载分词器
        self.tokenizer = AutoTokenizer.from_pretrained(model_dir, trust_remote_code=True)

        # 使用vLLM框架加载模型
        self.model = LLM(model=model_dir, tokenizer=model_dir, trust_remote_code=True)

        # 加入特殊Token
        self.tokenizer.eos_token_id=self.generation_config.eos_token_id
        # 推理终止词，遇到这些词停止继续推理
        self.stop_words_ids=[self.tokenizer.im_start_id,self.tokenizer.im_end_id,self.tokenizer.eos_token_id]
    
        # 可用的Tokens数量，用来控制单次最大输入Tokens
        self.token_space=self.generation_config.max_window_size


    def format_prompt(self, tokenizer, prompt, history=None, system=&quot;&quot;):
        
        im_start, im_start_tokens = &#x27;&lt;|im_start|&gt;&#x27;, [tokenizer.im_start_id]
        im_end, im_end_tokens = &#x27;&lt;|im_end|&gt;&#x27;, [tokenizer.im_end_id]

        def _format_tokenize(role, prompt):
            &quot;&quot;&quot;
            :param role:system
            :param prompt:你是一个智能助手
            :return:(&#x27;{role}\n{content}&#x27;, [8948, 198, 56568, 101909, 100168, 110498])
            &quot;&quot;&quot;
            return f&quot;{role}\n{prompt}&quot;, tokenizer.encode(role) + tokenizer.encode(&#x27;\n&#x27;) + tokenizer.encode(prompt)


        # System 角色的处理逻辑
        system_text, system_tokens = _format_tokenize(&#x27;system&#x27;, system)
        # 假设系统角色输入为：You are a helpful assistant， 经过这一步，会变成  &lt;|im_start|&gt;system
        #                                                                            You are a helpful assistant.&lt;|im_end|&gt;
        system_text_full = f&#x27;{im_start}{system_text}{im_end}&#x27;

        # 这里是其对应的编码：[151644, 8948, 198, 2610, 525, 264, 10950, 17847, 13, 151645]
        system_tokens_full = im_start_tokens + system_tokens + im_end_tokens
        self.token_space -= len(system_tokens_full)
        
        # User 角色的处理逻辑
        user_text, user_tokens = _format_tokenize(&#x27;user&#x27;, prompt)
        user_tokens_prefix = tokenizer.encode(&quot;\n&quot;) + im_start_tokens
        user_tokens_suffix = im_end_tokens + tokenizer.encode(&quot;\n&quot;) + im_start_tokens + tokenizer.encode(&#x27;assistant&#x27;) + tokenizer.encode(&quot;\n&quot;)

        # 判断一下，避免输入的Prompt过长，过长就截断
        if len(user_tokens_prefix) + len(user_tokens) + len(user_tokens_suffix) &gt; self.token_space: 
            user_tokens_len= self.token_space - len(user_tokens_prefix) - len(user_tokens_suffix)
            user_tokens = user_tokens[:user_tokens_len]
            user_text = self.tokenizer.decode(user_tokens)
            
        user_text_full = f&quot;\n{im_start}{user_text}{im_end}\n{im_start}assistant\n&quot;
        user_tokens_full = user_tokens_prefix + user_tokens + user_tokens_suffix
        
        self.token_space -= len(user_tokens_full)


        prompt_str = f&#x27;{system_text_full}{user_text_full}&#x27;
        prompt_tokens = system_tokens_full + user_tokens_full
        return prompt_str,prompt_tokens

    def chat(self, prompt, history=None, system=&quot;You are a helpful assistant.&quot;):
        &quot;&quot;&quot;
        :param
        :param
        :return: Description of return value
        &quot;&quot;&quot;
        prompt_text, prompt_tokens = self.format_prompt(self.tokenizer, prompt, system=system)
        
        print(prompt_text)

        # VLLM请求配置
        sampling_params=SamplingParams(stop_token_ids=self.stop_words_ids, max_tokens=1024)
        
        # 调用VLLM执行推理
        vllm_outputs=self.model.generate(prompt_token_ids=[prompt_tokens],sampling_params=sampling_params)

        # 返回最终结果
        response= vllm_outputs[0].outputs[0].text
        
        print(response)
```

```
/home/util/anaconda3/envs/qwen_7b_chat/lib/python3.11/site-packages/tqdm/auto.py:21: TqdmWarning: IProgress not found. Please update jupyter and ipywidgets. See https://ipywidgets.readthedocs.io/en/stable/user_install.html
  from .autonotebook import tqdm as notebook_tqdm
2024-02-01 16:33:15,172        INFO util.py:154 -- Missing packages: [&#x27;ipywidgets&#x27;]. Run `pip install -U ipywidgets`, then restart the notebook server for rich notebook output.
2024-02-01 16:33:15,693        INFO util.py:154 -- Missing packages: [&#x27;ipywidgets&#x27;]. Run `pip install -U ipywidgets`, then restart the notebook server for rich notebook output.
```

```
model_dir = &quot;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&quot;

# 实例化
vllm_model = myvLLMWrapper(model_dir)
```

```
INFO 02-01 16:33:19 llm_engine.py:72] Initializing an LLM engine with config: model=&#x27;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&#x27;, tokenizer=&#x27;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&#x27;, tokenizer_mode=auto, revision=None, tokenizer_revision=None, trust_remote_code=True, dtype=torch.float16, max_seq_len=8192, download_dir=None, load_format=auto, tensor_parallel_size=1, quantization=None, seed=0)
WARNING 02-01 16:33:20 tokenizer.py:66] Using a slow tokenizer. This might cause a significant slowdown. Consider using a fast tokenizer instead.
INFO 02-01 16:34:17 llm_engine.py:219] # GPU blocks: 5350, # CPU blocks: 1365
```

```
vllm_model.chat(prompt=&quot;能帮我推荐几个北京的美食吗&quot;, system=&#x27;你是一个美食家&#x27;)
```

```
&lt;|im_start|&gt;system
你是一个美食家&lt;|im_end|&gt;
&lt;|im_start|&gt;user
能帮我推荐几个北京的美食吗&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant



Processed prompts: 100%|██████████| 1/1 [00:01&lt;00:00,  1.87s/it]

当然可以！北京有很多美味的美食，例如：

1. 烤鸭：这是北京的招牌美食之一，以其醇厚的口感和美妙的香味深受人们的喜爱。你可以去大董烤鸭店尝试一下。

2. 北京炸酱面：这是北京著名的面条，汤底浓郁，面条筋道，配以黄豆酱和肉末炒制而成，口感丰富。

3. 炸酱薯条：这是一种炸薯条的改良版，特别适合北京人的口味，要注意天气变化，避免炸制时间过长导致薯条变软。

4. 北京烤鸭包灌肠：这是一款以北京烤鸭为主题的灌肠，炸至金黄的北京烤鸭外皮包裹着香脆的灌肠，是北京年轻人的最爱。

5. 北京豆汁儿炸酱面：这是一款具有北京特色的小吃，面条炸至酥脆，配以豆腐脑和豆酱等醇厚的调料，味道醇厚。

以上这些只是北京的几款经典美食，还有很多其他美食在北京市内值得一试。希望你能在北京有一个美食之旅！&lt;|im_end|&gt;
```

```
vllm_model.chat(prompt=&quot;我想去滑雪，我应该买什么滑雪设备&quot;, system=&#x27;你是一个滑雪高手&#x27;)
```

```
&lt;|im_start|&gt;system
你是一个滑雪高手&lt;|im_end|&gt;
&lt;|im_start|&gt;user
我想去滑雪，我应该买什么滑雪设备&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant



Processed prompts: 100%|██████████| 1/1 [00:02&lt;00:00,  2.05s/it]

滑雪是一项需要身体协调、力量和平衡的运动，因此以下装备仅供参考：

1. 材质：滑雪鞋最好选择防水、透气的材料，防滑的雪靴也可以提高行走稳定性。滑雪手杖选择柔软、适合自己手臂力量的。
2. 护目镜：为了保护眼睛不受寒冷空气、雪粒和阳光的伤害，滑雪眼镜是非常必要的。它们可以帮你在滑雪过程中提供清晰的视觉。

除了滑雪设备，根据个人偏好和运动水平，还需要考虑以下辅具：

1. 防寒服：在冰雪环境中保持身体温暖是非常重要的。选择保暖而且透气的滑雪服。
2. 独立背包：为了携带所有必要的滑雪装备和物品，例如食品、急救包、雪橇等，需要准备一个大的背包。
3. 护膝、护肘和护腕：这些滑雪设备可以在保护膝盖和胳膊不受伤害的同时，提高滑雪过程中的稳定性。
4. 铅圈：铅圈可以提供额外的稳定性和保护，防止滑雪时摔倒。

最后，在购买滑雪设备和器材时，可以咨询专业滑冰教练、销售员或在线滑雪装备商店，以获取更多的帮助和建议。&lt;|im_end|&gt;
```

最后，我们再丰富一下，添加上History信息。

```
# 导入Transformer依赖库
from vllm import LLM
from vllm.sampling_params import SamplingParams
from transformers import AutoModelForCausalLM, AutoTokenizer
from transformers.generation import GenerationConfig
import copy
import os

# 通义千问的特殊token
IMSTART = &#x27;&lt;|im_start|&gt;&#x27;
IMEND = &#x27;&lt;|im_end|&gt;&#x27;
ENDOFTEXT = &#x27;&lt;|endoftext|&gt;&#x27;


class myvLLMWrapper:
    def __init__(self,
                 model_dir,):
        &quot;&quot;&quot;
        :param model_dir ： 大模型实际存储位置
        :param
        :return: Description of return value
        &quot;&quot;&quot;

        # 加载配置，即Qweb模型目录下的generation_config.json文件，是推理的关键参数
        self.generation_config = GenerationConfig.from_pretrained(model_dir, trust_remote_code=True)

        # 加载分词器
        self.tokenizer = AutoTokenizer.from_pretrained(model_dir, trust_remote_code=True)

        # 使用vLLM框架加载模型
        self.model = LLM(model=model_dir, tokenizer=model_dir, trust_remote_code=True)

        # 加入特殊Token
        self.tokenizer.eos_token_id = self.generation_config.eos_token_id
        # 推理终止词，遇到这些词停止继续推理
        self.stop_words_ids = [self.tokenizer.im_start_id, self.tokenizer.im_end_id, self.tokenizer.eos_token_id]

        # 可用的Tokens数量，用来控制单次最大输入Tokens
        self.token_space = self.generation_config.max_window_size

    def format_prompt(self,
                      tokenizer,
                      prompt,
                      history=None,
                      system=&quot;&quot;):

        im_start, im_start_tokens = &#x27;&lt;|im_start|&gt;&#x27;, [tokenizer.im_start_id]
        im_end, im_end_tokens = &#x27;&lt;|im_end|&gt;&#x27;, [tokenizer.im_end_id]

        if history is None:
            history = []

        def _format_tokenize(role, prompt):
            &quot;&quot;&quot;
            :param role:system
            :param prompt:你是一个智能助手
            :return:(&#x27;{role}\n{content}&#x27;, [8948, 198, 56568, 101909, 100168, 110498])
            &quot;&quot;&quot;
            return f&quot;{role}\n{prompt}&quot;, tokenizer.encode(role) + tokenizer.encode(&#x27;\n&#x27;) + tokenizer.encode(prompt)

        # System 角色的处理逻辑
        system_text, system_tokens = _format_tokenize(&#x27;system&#x27;, system)
        # 假设系统角色输入为：You are a helpful assistant， 经过这一步，会变成  &lt;|im_start|&gt;system
        #                                                                            You are a helpful assistant.&lt;|im_end|&gt;
        system_text_full = f&#x27;{im_start}{system_text}{im_end}&#x27;

        # 这里是其对应的编码：[151644, 8948, 198, 2610, 525, 264, 10950, 17847, 13, 151645]
        system_tokens_full = im_start_tokens + system_tokens + im_end_tokens
        self.token_space -= len(system_tokens_full)

        # User 角色的处理逻辑
        user_text, user_tokens = _format_tokenize(&#x27;user&#x27;, prompt)
        user_tokens_prefix = tokenizer.encode(&quot;\n&quot;) + im_start_tokens
        user_tokens_suffix = im_end_tokens + tokenizer.encode(&quot;\n&quot;) + im_start_tokens + tokenizer.encode(
            &#x27;assistant&#x27;) + tokenizer.encode(&quot;\n&quot;)

        # 判断一下，避免输入的Prompt过长，过长就截断
        if len(user_tokens_prefix) + len(user_tokens) + len(user_tokens_suffix) &gt; self.token_space:
            user_tokens_len = self.token_space - len(user_tokens_prefix) - len(user_tokens_suffix)
            user_tokens = user_tokens[:user_tokens_len]
            user_text = self.tokenizer.decode(user_tokens)

        user_text_full = f&quot;\n{im_start}{user_text}{im_end}\n{im_start}assistant\n&quot;
        user_tokens_full = user_tokens_prefix + user_tokens + user_tokens_suffix

        self.token_space -= len(user_tokens_full)

        # 累积的历史对话文本和令牌列表
        accumulated_history_text, accumulated_history_tokens = &#x27;&#x27;, []

        # 从最近的对话历史开始处理
        for previous_user_query, previous_assistant_response in reversed(history):
            # 格式化user用户的历史提问
            user_query_text, user_query_tokens = _format_tokenize(&quot;user&quot;, previous_user_query)
            # 格式化并分词assistant的历史回答
            assistant_response_text, assistant_response_tokens = _format_tokenize(&quot;assistant&quot;,
                                                                                  previous_assistant_response)

            # 构建当前轮次的对话token
            current_round_tokens = tokenizer.encode(
                &quot;\n&quot;) + im_start_tokens + user_query_tokens + im_end_tokens + tokenizer.encode(
                &quot;\n&quot;) + im_start_tokens + assistant_response_tokens + im_end_tokens
            # 构建当前轮次的对话文本
            current_round_text = f&quot;\n{im_start}{user_query_text}{im_end}\n{im_start}{assistant_response_text}{im_end}&quot;

            # 如果累积的对话令牌数量没有超过限制，则将本轮对话添加到累积历史中
            if len(current_round_tokens) &lt;= self.token_space:
                accumulated_history_text = current_round_text + accumulated_history_text
                accumulated_history_tokens = current_round_tokens + accumulated_history_tokens
            else:
                break

        prompt_str = f&#x27;{system_text_full}{accumulated_history_text}{user_text_full}&#x27;
        prompt_tokens = system_tokens_full + accumulated_history_tokens + user_tokens_full
        return prompt_str, prompt_tokens

    def chat(self, prompt, history=None, system=&quot;You are a helpful assistant.&quot;):
        &quot;&quot;&quot;
        :param
        :param
        :return: Description of return value
        &quot;&quot;&quot;
        # 历史聊天
        if history is None:
            history = []
        else:
            history = copy.deepcopy(history)

        prompt_text, prompt_tokens = self.format_prompt(self.tokenizer, prompt, system=system, history=history)


        # VLLM请求配置
        sampling_params = SamplingParams(stop_token_ids=self.stop_words_ids,
                                         # 方式一：直接设置
                                         max_tokens=1024,
                                         # 方式二：去Qwen模型的`generation_config.json`文件中配置
                                         temperature=self.generation_config.temperature,)

        # 调用VLLM执行推理
        vllm_outputs=self.model.generate(prompt_token_ids=[prompt_tokens],sampling_params=sampling_params)

        # 返回最终结果
        response= vllm_outputs[0].outputs[0].text


        # 添加历史信息
        history.append((prompt, response))

        return response, history
```

```
/home/util/anaconda3/envs/qwen_7b_chat/lib/python3.11/site-packages/tqdm/auto.py:21: TqdmWarning: IProgress not found. Please update jupyter and ipywidgets. See https://ipywidgets.readthedocs.io/en/stable/user_install.html
  from .autonotebook import tqdm as notebook_tqdm
2024-02-01 17:25:51,552        INFO util.py:154 -- Missing packages: [&#x27;ipywidgets&#x27;]. Run `pip install -U ipywidgets`, then restart the notebook server for rich notebook output.
2024-02-01 17:25:52,059        INFO util.py:154 -- Missing packages: [&#x27;ipywidgets&#x27;]. Run `pip install -U ipywidgets`, then restart the notebook server for rich notebook output.
```

```
model_dir = &quot;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&quot;

# 实例化
vllm_model = myvLLMWrapper(model_dir)
```

```
INFO 02-01 17:02:07 llm_engine.py:72] Initializing an LLM engine with config: model=&#x27;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&#x27;, tokenizer=&#x27;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&#x27;, tokenizer_mode=auto, revision=None, tokenizer_revision=None, trust_remote_code=True, dtype=torch.float16, max_seq_len=8192, download_dir=None, load_format=auto, tensor_parallel_size=1, quantization=None, seed=0)
WARNING 02-01 17:02:07 tokenizer.py:66] Using a slow tokenizer. This might cause a significant slowdown. Consider using a fast tokenizer instead.
INFO 02-01 17:03:03 llm_engine.py:219] # GPU blocks: 5350, # CPU blocks: 1365
```

```
response, history = vllm_model.chat(prompt=&quot;你好，请你帮我推荐几道北京的美食&quot;, system=&#x27;你是一个美食家&#x27;, history=None)
print(response)
```

```
&lt;|im_start|&gt;system
你是一个美食家&lt;|im_end|&gt;
&lt;|im_start|&gt;user
你好，请你帮我推荐几道北京的美食&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant



Processed prompts: 100%|██████████| 1/1 [00:00&lt;00:00,  1.17it/s]

好的，北京是“四大名菜”的发源地，其中最有名的就是 炸酱面、烤鸭和炸酱面。还有 北京烤鸭， 醃鸭是中国传统名菜之一，主要食材在 Swan Valley 和 Heston Blumenthal 餐厅也非常有名。还有 北京炸酱面，这也是一道非常重要的小吃，它的制作也非常精细。&lt;|im_end|&gt;
```

```
response, history = vllm_model.chat(prompt=&quot;是吗？帮我具体展开聊一聊&quot;, system=&#x27;你是一个美食家&#x27;, history=history)
print(response)
```

```
&lt;|im_start|&gt;system
你是一个美食家&lt;|im_end|&gt;
&lt;|im_start|&gt;user
你好，请你帮我推荐几道北京的美食&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant
好的，北京是“四大名菜”的发源地，其中最有名的就是 炸酱面、烤鸭和炸酱面。还有 北京烤鸭， 醃鸭是中国传统名菜之一，主要食材在 Swan Valley 和 Heston Blumenthal 餐厅也非常有名。还有 北京炸酱面，这也是一道非常重要的小吃，它的制作也非常精细。&lt;|im_end|&gt;&lt;|im_end|&gt;
&lt;|im_start|&gt;user
是吗？帮我具体展开聊一聊&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant



Processed prompts: 100%|██████████| 1/1 [00:01&lt;00:00,  1.48s/it]

当然可以。炸酱面是非常受欢迎的北京小吃，它名叫colorful。它的配料有红黄酱、黄瓜、豆芽丝、老豆腐等。其中，红黄酱是酱料的主要成分，它是由豆瓣酱和黄豆酱、青红辣椒酱等制作而成。然后，炸酱面是有面条的，在炒制的时候要加入葱、姜、蒜、酱油、猪油等原料，最后用适当的调料调味即可。烤鸭也是北京最有名的传统名菜之一，它属于烤鸭类，主要由鸭子、调料、酱料等制作而成。这些精髓都是北京特有的，所以这些菜肴都会受到大家的欢迎。&lt;|im_end|&gt;
```

```
response, history = vllm_model.chat(prompt=&quot;除了这些，还有什么呢？&quot;, system=&#x27;你是一个美食家&#x27;, history=history)
print(response)
```

```
&lt;|im_start|&gt;system
你是一个美食家&lt;|im_end|&gt;
&lt;|im_start|&gt;user
你好，请你帮我推荐几道北京的美食&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant
好的，北京是“四大名菜”的发源地，其中最有名的就是 炸酱面、烤鸭和炸酱面。还有 北京烤鸭， 醃鸭是中国传统名菜之一，主要食材在 Swan Valley 和 Heston Blumenthal 餐厅也非常有名。还有 北京炸酱面，这也是一道非常重要的小吃，它的制作也非常精细。&lt;|im_end|&gt;&lt;|im_end|&gt;
&lt;|im_start|&gt;user
是吗？帮我具体展开聊一聊&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant
当然可以。炸酱面是非常受欢迎的北京小吃，它名叫colorful。它的配料有红黄酱、黄瓜、豆芽丝、老豆腐等。其中，红黄酱是酱料的主要成分，它是由豆瓣酱和黄豆酱、青红辣椒酱等制作而成。然后，炸酱面是有面条的，在炒制的时候要加入葱、姜、蒜、酱油、猪油等原料，最后用适当的调料调味即可。烤鸭也是北京最有名的传统名菜之一，它属于烤鸭类，主要由鸭子、调料、酱料等制作而成。这些精髓都是北京特有的，所以这些菜肴都会受到大家的欢迎。&lt;|im_end|&gt;&lt;|im_end|&gt;
&lt;|im_start|&gt;user
除了这些，还有什么呢？&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant



Processed prompts: 100%|██████████| 1/1 [00:01&lt;00:00,  1.04s/it]

当然，肯定会有其他美食等你去发现。北京烤鸭是在鸭子在经过烹饪后，会被挂在铁网上晾干，在晾干的过程中会有划痕，因此才会有特别的香味。北京烤鸭的味道非常独特，它既有老鸭的酥脆口感，又有鸭子的鲜美。炸酱面是一道传统的北京小吃，它的味道十分鲜美，主要是因为使用了黄豆酱和豆瓣酱等调料。&lt;|im_end|&gt;
```

最后，我们基于Chat方法创建一个多轮对话，代码如下：

```
def initialize_model(model_path):
    &quot;&quot;&quot;
    初始化并加载模型。
    &quot;&quot;&quot;
    model = myvLLMWrapper(model_path)
    return model

def main():
    model_path = &quot;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&quot;

    # 获取用户设置的历史记录长度
    try:
        history_length = int(input(&quot;请输入历史记录的长度（建议值：128）: &quot;))
    except ValueError:
        print(&quot;输入无效，使用默认历史记录长度：128&quot;)
        history_length = 20

    history = None
    model_initialized = False

    while True:

        if not model_initialized:
            print(&quot;正在初始化模型，请稍等...&quot;)
            vllm_model = initialize_model(model_path)
            model_initialized = True
            print(&quot;模型初始化完成。&quot;)

        user_input = input(&#x27;您的提问: &#x27;)
        if user_input.lower() == &#x27;退出&#x27;:
            print(&quot;感谢您的使用，再见！&quot;)
            break


        try:
            response, history = vllm_model.chat(prompt=user_input, history=history)
            print(&quot;回答:&quot;, response)
            history = history[-history_length:]  # 使用用户设定的历史记录长度
        except Exception as e:
            print(&quot;发生错误:&quot;, e)
            break

if __name__ == &quot;__main__&quot;:
    main()
```

```
请输入历史记录的长度（建议值：128）:  128
正在初始化模型，请稍等...
INFO 02-01 17:26:16 llm_engine.py:72] Initializing an LLM engine with config: model=&#x27;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&#x27;, tokenizer=&#x27;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&#x27;, tokenizer_mode=auto, revision=None, tokenizer_revision=None, trust_remote_code=True, dtype=torch.float16, max_seq_len=8192, download_dir=None, load_format=auto, tensor_parallel_size=1, quantization=None, seed=0)
WARNING 02-01 17:26:16 tokenizer.py:66] Using a slow tokenizer. This might cause a significant slowdown. Consider using a fast tokenizer instead.
INFO 02-01 17:27:12 llm_engine.py:219] # GPU blocks: 5350, # CPU blocks: 1365
模型初始化完成。
您的提问:  你好，请你介绍一下你自己


Processed prompts: 100%|██████████| 1/1 [00:00&lt;00:00,  1.66it/s]


回答: 我是阿里云开发的一种超大规模语言模型，我叫通义千问。我通过大量的自然语言处理训练，能够回答各种问题、提供定义、解释和建议，还能表达观点、撰写代码。如果您有任何问题或需要帮助，请随时告诉我，我会尽力提供支持。&lt;|im_end|&gt;
您的提问:  再详细一点


Processed prompts: 100%|██████████| 1/1 [00:00&lt;00:00,  1.60it/s]


回答: 我可以通过多种方式进行交流和互动，包括但不限于文本、语音和视频。我还能理解和生成多种语言，包括但不限于中文、英文、日文、韩文等。我还可以通过支持各种文本生成任务，如回答问题、提供建议、生成代码、总结文本等。&lt;|im_end|&gt;
您的提问:  今天是2024年2月01日


Processed prompts: 100%|██████████| 1/1 [00:00&lt;00:00,  1.99it/s]


回答: 很抱歉，由于时间轴的复杂性，我无法实时获取您所在日期的信息。如果您需要查询今天是哪一天，可以使用日期相关的API或服务，或者使用一些文本处理工具来获取当前的日期。&lt;|im_end|&gt;
您的提问:  今天是2024年2月1日，你记录一下


Processed prompts: 100%|██████████| 1/1 [00:00&lt;00:00,  3.11it/s]


回答: 好的，是的，今天是2024年2月1日，今天是2024年2月1日。&lt;|im_end|&gt;
您的提问:  今天是几号？


Processed prompts: 100%|██████████| 1/1 [00:00&lt;00:00,  6.04it/s]


回答: 今天是2024年2月1日。&lt;|im_end|&gt;
```

上述就可以看到，掌握了数据的输入，Transformers库对Qwen模型的加载方式，及明确模型的输出处理流程，一切中间的逻辑是完全可以自己手动去定义和封装的。除了处理的逻辑，像myvLLMWrapper的初始化，SamplingParams参数的设置，完全可以根据某个开源框架封装好的功能进行接入。比如：

```
vllm_model = vLLMWrapper(model_path,
                         dtype=&quot;float16&quot;,
                         gpu_memory_utilization=0.6)
```

> 详细的参数在这里：https://docs.vllm.ai/en/latest/genindex.html

接入Gradio，生成网页端的Web UI。

```
import gradio as gr

# 模型和历史记录初始化
model_path = &quot;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&quot;  # 模型路径
vllm_model = myvLLMWrapper(model_path)

history = []

def chat_with_model(user_input):
    global history
    response, history = vllm_model.chat(prompt=user_input, history=history)
    history = history[-128:]  # 保持历史记录长度为128
    return response

iface = gr.Interface(
    fn=chat_with_model,
    inputs=gr.inputs.Textbox(lines=2, placeholder=&quot;输入您的问题...&quot;),
    outputs=&quot;text&quot;,
    title=&quot;智能对话系统&quot;,
    description=&quot;输入您的问题并与智能模型进行对话。&quot;,
)

if __name__ == &quot;__main__&quot;:
    iface.launch(server_name=&quot;192.168.110.131&quot;, server_port=12345)
```

```
INFO 02-01 17:46:57 llm_engine.py:72] Initializing an LLM engine with config: model=&#x27;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&#x27;, tokenizer=&#x27;/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-1_8B-Chat/&#x27;, tokenizer_mode=auto, revision=None, tokenizer_revision=None, trust_remote_code=True, dtype=torch.float16, max_seq_len=8192, download_dir=None, load_format=auto, tensor_parallel_size=1, quantization=None, seed=0)
WARNING 02-01 17:46:58 tokenizer.py:66] Using a slow tokenizer. This might cause a significant slowdown. Consider using a fast tokenizer instead.
INFO 02-01 17:47:53 llm_engine.py:219] # GPU blocks: 5350, # CPU blocks: 1365


/tmp/ipykernel_4181929/1327185800.py:17: GradioDeprecationWarning: Usage of gradio.inputs is deprecated, and will not be supported in the future, please import your component from gradio.components
  inputs=gr.inputs.Textbox(lines=2, placeholder=&quot;输入您的问题...&quot;),
/tmp/ipykernel_4181929/1327185800.py:17: GradioDeprecationWarning: `optional` parameter is deprecated, and it has no effect
  inputs=gr.inputs.Textbox(lines=2, placeholder=&quot;输入您的问题...&quot;),
/tmp/ipykernel_4181929/1327185800.py:17: GradioDeprecationWarning: `numeric` parameter is deprecated, and it has no effect
  inputs=gr.inputs.Textbox(lines=2, placeholder=&quot;输入您的问题...&quot;),


Running on local URL:  http://192.168.110.131:12345

To create a public link, set `share=True` in `launch()`.
```

```
Processed prompts: 100%|██████████| 1/1 [00:00&lt;00:00,  1.65it/s]
Processed prompts: 100%|██████████| 1/1 [00:00&lt;00:00,  5.54it/s]
Processed prompts: 100%|██████████| 1/1 [00:00&lt;00:00,  1.64it/s]
Processed prompts: 100%|██████████| 1/1 [00:00&lt;00:00,  5.53it/s]
```

📍更**多大模型技术内容学习**

**扫码添加助理****英英**，回复“大模型”，了解更多大模型技术详情哦👇

![](img/7a95e7f7a92a77100d22bfcea58e6a30.png)

此外，**扫码回复“入群”**，即可加入**大模型技术社群****：海量硬核独家技术****干货内容****+无门槛****技术交流****！**