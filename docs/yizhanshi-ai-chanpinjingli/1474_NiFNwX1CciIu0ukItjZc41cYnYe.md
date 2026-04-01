# 03_predict.ipynb

> 来源：[https://v11enp9ok1h.feishu.cn/docx/G6vsdbaSSoCwOPxqTyWc3xpxnqf](https://v11enp9ok1h.feishu.cn/docx/G6vsdbaSSoCwOPxqTyWc3xpxnqf)

![](img/7f6368222ebe1f47564e16099e9f6a2e.png)

[code_03_predict.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HMevb49RjohPyQx9Z3gcONggnrf)

```
{
 &quot;cells&quot;: [
  {
   &quot;attachments&quot;: {},
   &quot;cell_type&quot;: &quot;markdown&quot;,
   &quot;metadata&quot;: {},
   &quot;source&quot;: [
    &quot;# 预测\n&quot;,
    &quot;也就是所谓的调用我们训练好的模型&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 4,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;from transformers import pipeline\n&quot;,
    &quot;\n&quot;,
    &quot;pipe = pipeline(task=\&quot;text-classification\&quot;,\n&quot;,
    &quot;                model=\&quot;model_result/checkpoint-800\&quot;)\n&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 5,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;data&quot;: {
      &quot;text/plain&quot;: [
       &quot;[{&#x27;label&#x27;: &#x27;LABEL_0&#x27;, &#x27;score&#x27;: 0.9969683289527893},\n&quot;,
       &quot; {&#x27;label&#x27;: &#x27;LABEL_1&#x27;, &#x27;score&#x27;: 0.9964021444320679}]&quot;
      ]
     },
     &quot;execution_count&quot;: 5,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;test_str = [&#x27;这个酒店也太差了&#x27;, &#x27;非常实惠&#x27;]\n&quot;,
    &quot;pipe(test_str)\n&quot;
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
   &quot;display_name&quot;: &quot;mynet2&quot;,
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
    &quot;hash&quot;: &quot;110bc624a448454d574a0cd6cc76359fd86f75739e493913b3d71c2e04f2ffdb&quot;
   }
  }
 },
 &quot;nbformat&quot;: 4,
 &quot;nbformat_minor&quot;: 2
}
```