# train_model_02.ipynb

> 来源：[https://v11enp9ok1h.feishu.cn/docx/BOcndxPsFobvEVxOks7cIPJynKh](https://v11enp9ok1h.feishu.cn/docx/BOcndxPsFobvEVxOks7cIPJynKh)

![](img/ee5463fa720f15ed50d0ef79dacca74a.png)

[train_model_02.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EuY8bylamoPtdJx88JKcchbVnEh)

```
{
 &quot;cells&quot;: [
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 1,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;from PIL import Image\n&quot;,
    &quot;import requests\n&quot;,
    &quot;from transformers import ChineseCLIPProcessor, ChineseCLIPModel, Trainer,TrainingArguments\n&quot;,
    &quot;import torch\n&quot;,
    &quot;from datasets import load_dataset,Dataset\n&quot;,
    &quot;from pathlib import Path\n&quot;,
    &quot;import pandas as pd \n&quot;,
    &quot;import numpy as np \n&quot;,
    &quot;from tqdm import tqdm &quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 2,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;model_name_or_path = \&quot;OFA-Sys/chinese-clip-vit-large-patch14\&quot;\n&quot;,
    &quot;model = ChineseCLIPModel.from_pretrained(model_name_or_path)\n&quot;,
    &quot;processor = ChineseCLIPProcessor.from_pretrained(model_name_or_path)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 3,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;data&quot;: {
      &quot;text/plain&quot;: [
       &quot;{&#x27;input_ids&#x27;: [[101, 671, 702, 3749, 6756, 102], [101, 2408, 691, 5546, 4649, 4173, 7890, 976, 2533, 5401, 679, 5401, 6821, 1126, 4157, 2408, 2466, 4173, 7890, 976, 3791, 4908, 6394, 2523, 7028, 6206, 102], [101, 2458, 2552, 7937, 5709, 1920, 4510, 2512, 2137, 3440, 517, 1909, 3821, 4294, 4172, 2630, 518, 6856, 3683, 5439, 3926, 3173, 102], [101, 6617, 749, 6821, 671, 1767, 4801, 801, 117, 5074, 5381, 1377, 809, 2128, 2552, 5023, 3616, 3152, 1726, 3341, 749, 102]], &#x27;token_type_ids&#x27;: [[0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]], &#x27;attention_mask&#x27;: [[1, 1, 1, 1, 1, 1], [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1], [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1], [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]]}&quot;
      ]
     },
     &quot;execution_count&quot;: 3,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;text_str = [&#x27;一个汽车&#x27;, &#x27;广东脆皮烧鸭做得美不美 这几点广式烧鸭做法秘诀很重要&#x27;, &#x27;开心麻花大电影定档 《夏洛特烦恼》逗比老清新&#x27;,\&quot;赢了这一场硬仗,篮网可以安心等欧文回来了\&quot;]\n&quot;,
    &quot;processor(text=text_str)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 4,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;data&quot;: {
      &quot;text/plain&quot;: [
       &quot;{&#x27;pixel_values&#x27;: [array([[[ 0.14932826,  0.14932826,  0.14932826, ..., -0.55139625,\n&quot;,
       &quot;         -0.5659947 , -0.5805931 ],\n&quot;,
       &quot;        [ 0.14932826,  0.14932826,  0.14932826, ..., -0.55139625,\n&quot;,
       &quot;         -0.5659947 , -0.5805931 ],\n&quot;,
       &quot;        [ 0.14932826,  0.14932826,  0.14932826, ..., -0.55139625,\n&quot;,
       &quot;         -0.55139625, -0.5659947 ],\n&quot;,
       &quot;        ...,\n&quot;,
       &quot;        [-0.20103405, -0.12804192,  0.07633615, ..., -1.0331444 ,\n&quot;,
       &quot;         -0.7995695 , -0.6973805 ],\n&quot;,
       &quot;        [-0.24482933, -0.47840413, -0.5805931 , ..., -0.9893491 ,\n&quot;,
       &quot;         -0.94555384, -0.78497106],\n&quot;,
       &quot;        [-0.15723877, -0.43460885, -0.60978997, ..., -0.7703726 ,\n&quot;,
       &quot;         -0.8579632 , -0.78497106]],\n&quot;,
       &quot;\n&quot;,
       &quot;       [[ 1.0843711 ,  1.0543556 ,  1.0393478 , ...,  0.55909926,\n&quot;,
       &quot;          0.54409146,  0.5290837 ],\n&quot;,
       &quot;        [ 1.0543556 ,  1.0543556 ,  1.0393478 , ...,  0.55909926,\n&quot;,
       &quot;          0.54409146,  0.5290837 ],\n&quot;,
       &quot;        [ 1.0543556 ,  1.0393478 ,  1.0393478 , ...,  0.55909926,\n&quot;,
       &quot;          0.55909926,  0.54409146],\n&quot;,
       &quot;        ...,\n&quot;,
       &quot;        [-0.10124261, -0.04121154,  0.15388943, ..., -0.82161546,\n&quot;,
       &quot;         -0.53646785, -0.40139794],\n&quot;,
       &quot;        [-0.13125814, -0.38639018, -0.49144456, ..., -0.7315688 ,\n&quot;,
       &quot;         -0.65652996, -0.461429  ],\n&quot;,
       &quot;        [ 0.01881953, -0.2813358 , -0.4764368 , ..., -0.461429  ,\n&quot;,
       &quot;         -0.53646785, -0.4164057 ]],\n&quot;,
       &quot;\n&quot;,
       &quot;       [[ 1.6055346 ,  1.6624149 ,  1.7477353 , ...,  1.6339747 ,\n&quot;,
       &quot;          1.6197547 ,  1.6055346 ],\n&quot;,
       &quot;        [ 1.6624149 ,  1.705075  ,  1.7619553 , ...,  1.6339747 ,\n&quot;,
       &quot;          1.6197547 ,  1.6055346 ],\n&quot;,
       &quot;        [ 1.7477353 ,  1.7619553 ,  1.8046155 , ...,  1.6339747 ,\n&quot;,
       &quot;          1.6339747 ,  1.6197547 ],\n&quot;,
       &quot;        ...,\n&quot;,
       &quot;        [-0.49903518, -0.3710546 , -0.10087335, ..., -0.21463387,\n&quot;,
       &quot;          0.0413273 ,  0.16930789],\n&quot;,
       &quot;        [-0.48481512, -0.6412359 , -0.68389606, ..., -0.14353354,\n&quot;,
       &quot;         -0.08665328,  0.09820756],\n&quot;,
       &quot;        [-0.32839438, -0.5274753 , -0.6412359 , ...,  0.09820756,\n&quot;,
       &quot;          0.01288717,  0.1266477 ]]], dtype=float32)]}&quot;
      ]
     },
     &quot;execution_count&quot;: 4,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;image_str = \&quot;bigdata/image_data/test-31115.jpg\&quot;\n&quot;,
    &quot;image_input = Image.open(image_str)\n&quot;,
    &quot;processor(images=image_input)\n&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 5,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;data&quot;: {
      &quot;application/vnd.jupyter.widget-view+json&quot;: {
       &quot;model_id&quot;: &quot;7784adb47ed848a89f99c6cf5da51697&quot;,
       &quot;version_major&quot;: 2,
       &quot;version_minor&quot;: 0
      },
      &quot;text/plain&quot;: [
       &quot;  0%|          | 0/1346 [00:00&lt;?, ?ba/s]&quot;
      ]
     },
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;display_data&quot;
    },
    {
     &quot;data&quot;: {
      &quot;application/vnd.jupyter.widget-view+json&quot;: {
       &quot;model_id&quot;: &quot;22ef6a2c8f63478f985017e8149186e7&quot;,
       &quot;version_major&quot;: 2,
       &quot;version_minor&quot;: 0
      },
      &quot;text/plain&quot;: [
       &quot;  0%|          | 0/1 [00:00&lt;?, ?ba/s]&quot;
      ]
     },
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;display_data&quot;
    },
    {
     &quot;data&quot;: {
      &quot;text/plain&quot;: [
       &quot;DatasetDict({\n&quot;,
       &quot;    train: Dataset({\n&quot;,
       &quot;        features: [&#x27;image_path&#x27;, &#x27;text&#x27;, &#x27;input_ids&#x27;, &#x27;attention_mask&#x27;],\n&quot;,
       &quot;        num_rows: 1345373\n&quot;,
       &quot;    })\n&quot;,
       &quot;    test: Dataset({\n&quot;,
       &quot;        features: [&#x27;image_path&#x27;, &#x27;text&#x27;, &#x27;input_ids&#x27;, &#x27;attention_mask&#x27;],\n&quot;,
       &quot;        num_rows: 270\n&quot;,
       &quot;    })\n&quot;,
       &quot;})&quot;
      ]
     },
     &quot;execution_count&quot;: 5,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;def tokenizer_text(examples) :\n&quot;,
    &quot;    res = processor(text=examples[&#x27;text&#x27;],max_length=64, padding=\&quot;max_length\&quot;, return_tensors=\&quot;pt\&quot;, truncation=True)\n&quot;,
    &quot;    examples[&#x27;input_ids&#x27;] = res[&#x27;input_ids&#x27;]\n&quot;,
    &quot;    examples[&#x27;attention_mask&#x27;] = res[&#x27;attention_mask&#x27;]\n&quot;,
    &quot;    return examples\n&quot;,
    &quot;\n&quot;,
    &quot;def transform_images(examples):\n&quot;,
    &quot;    images = [Image.open(i) for i in examples[&#x27;image_path&#x27;]]\n&quot;,
    &quot;    images = [processor(images=i, return_tensors=\&quot;pt\&quot;)[&#x27;pixel_values&#x27;] for i in images]\n&quot;,
    &quot;    examples[&#x27;pixel_values&#x27;] = images\n&quot;,
    &quot;    return examples\n&quot;,
    &quot;\n&quot;,
    &quot;\n&quot;,
    &quot;dataset = Dataset.from_pandas(df=pd.read_csv(\&quot;bigdata/clean_train_test/train.csv\&quot;))\n&quot;,
    &quot;dataset = dataset.train_test_split(test_size=0.0002)\n&quot;,
    &quot;dataset = dataset.map(\n&quot;,
    &quot;    function=tokenizer_text,\n&quot;,
    &quot;    batched=True\n&quot;,
    &quot;)\n&quot;,
    &quot;# dataset = dataset.map(\n&quot;,
    &quot;#     function=transform_images,\n&quot;,
    &quot;#     batched=True\n&quot;,
    &quot;# )\n&quot;,
    &quot;\n&quot;,
    &quot;dataset.set_transform(transform=transform_images)\n&quot;,
    &quot;\n&quot;,
    &quot;dataset\n&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 6,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;data&quot;: {
      &quot;text/plain&quot;: [
       &quot;torch.Size([1, 3, 224, 224])&quot;
      ]
     },
     &quot;execution_count&quot;: 6,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;# dataset[&#x27;train&#x27;][2][&#x27;input_ids&#x27;]\n&quot;,
    &quot;dataset[&#x27;train&#x27;][3][&#x27;pixel_values&#x27;].shape&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 7,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;100%|██████████| 200/200 [00:07&lt;00:00, 28.25it/s]\n&quot;
     ]
    },
    {
     &quot;data&quot;: {
      &quot;text/plain&quot;: [
       &quot;{torch.Size([64])}&quot;
      ]
     },
     &quot;execution_count&quot;: 7,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;a = set(torch.tensor(dataset[&#x27;train&#x27;][i][&#x27;input_ids&#x27;]).shape for i in tqdm(range(200)))\n&quot;,
    &quot;a&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 8,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;100%|██████████| 200/200 [00:02&lt;00:00, 77.88it/s]\n&quot;
     ]
    },
    {
     &quot;data&quot;: {
      &quot;text/plain&quot;: [
       &quot;{torch.Size([64])}&quot;
      ]
     },
     &quot;execution_count&quot;: 8,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;b = set(torch.tensor(dataset[&#x27;train&#x27;][i][&#x27;attention_mask&#x27;]).shape for i in tqdm(range(200)))\n&quot;,
    &quot;b&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 9,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;data&quot;: {
      &quot;text/plain&quot;: [
       &quot;torch.Size([2, 64])&quot;
      ]
     },
     &quot;execution_count&quot;: 9,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;torch.stack([torch.tensor(dataset[&#x27;train&#x27;][0][&#x27;input_ids&#x27;]),\n&quot;,
    &quot;             torch.tensor(dataset[&#x27;train&#x27;][0][&#x27;input_ids&#x27;])]).shape&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 10,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;data&quot;: {
      &quot;text/plain&quot;: [
       &quot;torch.Size([3, 224, 224])&quot;
      ]
     },
     &quot;execution_count&quot;: 10,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;dataset[&#x27;train&#x27;][0][&#x27;pixel_values&#x27;].squeeze(0).shape&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 11,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;data&quot;: {
      &quot;text/plain&quot;: [
       &quot;torch.Size([2, 3, 224, 224])&quot;
      ]
     },
     &quot;execution_count&quot;: 11,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;torch.stack([dataset[&#x27;train&#x27;][0][&#x27;pixel_values&#x27;].squeeze(0),\n&quot;,
    &quot;             dataset[&#x27;train&#x27;][0][&#x27;pixel_values&#x27;].squeeze(0),]).shape&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 12,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;# for i in tqdm(range(dataset[&#x27;train&#x27;].__len__())):\n&quot;,
    &quot;#     try:\n&quot;,
    &quot;#         image = dataset[&#x27;train&#x27;][i][&#x27;image_path&#x27;]\n&quot;,
    &quot;#         image = Image.open(image)\n&quot;,
    &quot;#         processor(images=image, return_tensors=\&quot;pt\&quot;)\n&quot;,
    &quot;#     except Exception as e:\n&quot;,
    &quot;#         print(dataset[&#x27;train&#x27;][i][&#x27;image_path&#x27;])\n&quot;,
    &quot;#         break\n&quot;,
    &quot;\n&quot;,
    &quot;    &quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 14,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;# dataset[&#x27;train&#x27;][5374]#[&#x27;image_path&#x27;]&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 15,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;data&quot;: {
      &quot;text/plain&quot;: [
       &quot;{&#x27;pixel_values&#x27;: tensor([[[[ 0.2515,  0.4705,  0.4559,  ...,  0.0179,  0.0033,  0.0033],\n&quot;,
       &quot;          [ 0.3683,  0.5873,  0.6457,  ...,  0.0033, -0.0113, -0.0113],\n&quot;,
       &quot;          [ 0.3975,  0.4705,  0.6019,  ...,  0.0179,  0.0617,  0.0763],\n&quot;,
       &quot;          ...,\n&quot;,
       &quot;          [ 0.8792,  0.7041,  0.4121,  ...,  1.1858,  1.2296,  1.1712],\n&quot;,
       &quot;          [ 0.5581,  0.4413,  0.2953,  ...,  1.1712,  1.1858,  1.2004],\n&quot;,
       &quot;          [-0.0988, -0.1280, -0.1572,  ...,  1.1128,  1.1566,  1.1712]],\n&quot;,
       &quot;\n&quot;,
       &quot;         [[ 0.3340,  0.5591,  0.5441,  ...,  0.0488,  0.0338,  0.0338],\n&quot;,
       &quot;          [ 0.4540,  0.6792,  0.7392,  ...,  0.0338,  0.0188,  0.0188],\n&quot;,
       &quot;          [ 0.4841,  0.5591,  0.6942,  ...,  0.0488,  0.0939,  0.1089],\n&quot;,
       &quot;          ...,\n&quot;,
       &quot;          [ 1.1594,  0.9793,  0.6792,  ...,  1.2344,  1.2795,  1.2194],\n&quot;,
       &quot;          [ 0.7842,  0.6642,  0.5141,  ...,  1.2194,  1.2344,  1.2495],\n&quot;,
       &quot;          [ 0.0789,  0.0488,  0.0338,  ...,  1.1594,  1.2044,  1.2194]],\n&quot;,
       &quot;\n&quot;,
       &quot;         [[ 0.5675,  0.7808,  0.7666,  ...,  0.1835,  0.1693,  0.1693],\n&quot;,
       &quot;          [ 0.6812,  0.8945,  0.9514,  ...,  0.1693,  0.1551,  0.1551],\n&quot;,
       &quot;          [ 0.7097,  0.7808,  0.9088,  ...,  0.1835,  0.2262,  0.2404],\n&quot;,
       &quot;          ...,\n&quot;,
       &quot;          [ 1.5344,  1.3638,  1.0794,  ...,  1.4349,  1.4776,  1.4207],\n&quot;,
       &quot;          [ 1.1932,  1.0794,  0.9372,  ...,  1.4207,  1.4349,  1.4491],\n&quot;,
       &quot;          [ 0.5248,  0.4964,  0.4821,  ...,  1.3638,  1.4065,  1.4207]]]])}&quot;
      ]
     },
     &quot;execution_count&quot;: 15,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;\n&quot;,
    &quot;\n&quot;,
    &quot;image = dataset[&#x27;train&#x27;][5374][&#x27;image_path&#x27;]\n&quot;,
    &quot;image = Image.open(image)\n&quot;,
    &quot;processor(images=image, return_tensors=\&quot;pt\&quot;)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 17,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;PyTorch: setting up devices\n&quot;,
      &quot;The default value for the training argument `--report_to` will change in v5 (from all installed integrations to none). In v5, you will need to use `--report_to all` to get the same behavior as now. You should start updating your code and make this info disappear :-).\n&quot;,
      &quot;Using cuda_amp half precision backend\n&quot;,
      &quot;***** Running training *****\n&quot;,
      &quot;  Num examples = 1345373\n&quot;,
      &quot;  Num Epochs = 1\n&quot;,
      &quot;  Instantaneous batch size per device = 28\n&quot;,
      &quot;  Total train batch size (w. parallel, distributed &amp; accumulation) = 56\n&quot;,
      &quot;  Gradient Accumulation steps = 2\n&quot;,
      &quot;  Total optimization steps = 24025\n&quot;,
      &quot;  Number of trainable parameters = 406233089\n&quot;
     ]
    },
    {
     &quot;data&quot;: {
      &quot;application/vnd.jupyter.widget-view+json&quot;: {
       &quot;model_id&quot;: &quot;535c6c4fb6b240318dd77253bae148cf&quot;,
       &quot;version_major&quot;: 2,
       &quot;version_minor&quot;: 0
      },
      &quot;text/plain&quot;: [
       &quot;  0%|          | 0/24025 [00:00&lt;?, ?it/s]&quot;
      ]
     },
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;display_data&quot;
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;c:\\Users\\yuanz\\anaconda3\\envs\\mynet2\\lib\\site-packages\\PIL\\Image.py:3074: DecompressionBombWarning: Image size (98988480 pixels) exceeds limit of 89478485 pixels, could be decompression bomb DOS attack.\n&quot;,
      &quot;  warnings.warn(\n&quot;,
      &quot;c:\\Users\\yuanz\\anaconda3\\envs\\mynet2\\lib\\site-packages\\PIL\\Image.py:979: UserWarning: Palette images with Transparency expressed in bytes should be converted to RGBA images\n&quot;,
      &quot;  warnings.warn(\n&quot;,
      &quot;***** Running Evaluation *****\n&quot;,
      &quot;  Num examples = 270\n&quot;,
      &quot;  Batch size = 28\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;loss&#x27;: 3.3329, &#x27;learning_rate&#x27;: 0.0004978699183320599, &#x27;epoch&#x27;: 0.04}\n&quot;
     ]
    },
    {
     &quot;data&quot;: {
      &quot;application/vnd.jupyter.widget-view+json&quot;: {
       &quot;model_id&quot;: &quot;97829a396eef48c999a2c6362e87a17c&quot;,
       &quot;version_major&quot;: 2,
       &quot;version_minor&quot;: 0
      },
      &quot;text/plain&quot;: [
       &quot;  0%|          | 0/10 [00:00&lt;?, ?it/s]&quot;
      ]
     },
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;display_data&quot;
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Saving model checkpoint to clip_chinese_02\\checkpoint-1000\n&quot;,
      &quot;Configuration saved in clip_chinese_02\\checkpoint-1000\\config.json\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;eval_loss&#x27;: 3.3026041984558105, &#x27;eval_runtime&#x27;: 9.1508, &#x27;eval_samples_per_second&#x27;: 29.506, &#x27;eval_steps_per_second&#x27;: 1.093, &#x27;epoch&#x27;: 0.04}\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Model weights saved in clip_chinese_02\\checkpoint-1000\\pytorch_model.bin\n&quot;,
      &quot;c:\\Users\\yuanz\\anaconda3\\envs\\mynet2\\lib\\site-packages\\PIL\\Image.py:979: UserWarning: Palette images with Transparency expressed in bytes should be converted to RGBA images\n&quot;,
      &quot;  warnings.warn(\n&quot;,
      &quot;***** Running Evaluation *****\n&quot;,
      &quot;  Num examples = 270\n&quot;,
      &quot;  Batch size = 28\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;loss&#x27;: 3.3321, &#x27;learning_rate&#x27;: 0.0004915075250885755, &#x27;epoch&#x27;: 0.08}\n&quot;
     ]
    },
    {
     &quot;data&quot;: {
      &quot;application/vnd.jupyter.widget-view+json&quot;: {
       &quot;model_id&quot;: &quot;8be40c91f0204fd7adc23bdb49e7bea4&quot;,
       &quot;version_major&quot;: 2,
       &quot;version_minor&quot;: 0
      },
      &quot;text/plain&quot;: [
       &quot;  0%|          | 0/10 [00:00&lt;?, ?it/s]&quot;
      ]
     },
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;display_data&quot;
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Saving model checkpoint to clip_chinese_02\\checkpoint-2000\n&quot;,
      &quot;Configuration saved in clip_chinese_02\\checkpoint-2000\\config.json\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;eval_loss&#x27;: 3.3026041984558105, &#x27;eval_runtime&#x27;: 3.4482, &#x27;eval_samples_per_second&#x27;: 78.302, &#x27;eval_steps_per_second&#x27;: 2.9, &#x27;epoch&#x27;: 0.08}\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Model weights saved in clip_chinese_02\\checkpoint-2000\\pytorch_model.bin\n&quot;,
      &quot;Deleting older checkpoint [clip_chinese_02\\checkpoint-20] due to args.save_total_limit\n&quot;,
      &quot;c:\\Users\\yuanz\\anaconda3\\envs\\mynet2\\lib\\site-packages\\PIL\\Image.py:979: UserWarning: Palette images with Transparency expressed in bytes should be converted to RGBA images\n&quot;,
      &quot;  warnings.warn(\n&quot;,
      &quot;c:\\Users\\yuanz\\anaconda3\\envs\\mynet2\\lib\\site-packages\\PIL\\Image.py:3074: DecompressionBombWarning: Image size (138921744 pixels) exceeds limit of 89478485 pixels, could be decompression bomb DOS attack.\n&quot;,
      &quot;  warnings.warn(\n&quot;,
      &quot;***** Running Evaluation *****\n&quot;,
      &quot;  Num examples = 270\n&quot;,
      &quot;  Batch size = 28\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;loss&#x27;: 3.3321, &#x27;learning_rate&#x27;: 0.000481033946311067, &#x27;epoch&#x27;: 0.12}\n&quot;
     ]
    },
    {
     &quot;data&quot;: {
      &quot;application/vnd.jupyter.widget-view+json&quot;: {
       &quot;model_id&quot;: &quot;706f6070730640beade23c641cdd53b2&quot;,
       &quot;version_major&quot;: 2,
       &quot;version_minor&quot;: 0
      },
      &quot;text/plain&quot;: [
       &quot;  0%|          | 0/10 [00:00&lt;?, ?it/s]&quot;
      ]
     },
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;display_data&quot;
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Saving model checkpoint to clip_chinese_02\\checkpoint-3000\n&quot;,
      &quot;Configuration saved in clip_chinese_02\\checkpoint-3000\\config.json\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;eval_loss&#x27;: 3.3026041984558105, &#x27;eval_runtime&#x27;: 3.3923, &#x27;eval_samples_per_second&#x27;: 79.592, &#x27;eval_steps_per_second&#x27;: 2.948, &#x27;epoch&#x27;: 0.12}\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Model weights saved in clip_chinese_02\\checkpoint-3000\\pytorch_model.bin\n&quot;,
      &quot;Deleting older checkpoint [clip_chinese_02\\checkpoint-40] due to args.save_total_limit\n&quot;,
      &quot;***** Running Evaluation *****\n&quot;,
      &quot;  Num examples = 270\n&quot;,
      &quot;  Batch size = 28\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;loss&#x27;: 3.3322, &#x27;learning_rate&#x27;: 0.0004666070773952711, &#x27;epoch&#x27;: 0.17}\n&quot;
     ]
    },
    {
     &quot;data&quot;: {
      &quot;application/vnd.jupyter.widget-view+json&quot;: {
       &quot;model_id&quot;: &quot;033a9949932e4aef9e10a9db7e10c482&quot;,
       &quot;version_major&quot;: 2,
       &quot;version_minor&quot;: 0
      },
      &quot;text/plain&quot;: [
       &quot;  0%|          | 0/10 [00:00&lt;?, ?it/s]&quot;
      ]
     },
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;display_data&quot;
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Saving model checkpoint to clip_chinese_02\\checkpoint-4000\n&quot;,
      &quot;Configuration saved in clip_chinese_02\\checkpoint-4000\\config.json\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;eval_loss&#x27;: 3.3026041984558105, &#x27;eval_runtime&#x27;: 3.3887, &#x27;eval_samples_per_second&#x27;: 79.676, &#x27;eval_steps_per_second&#x27;: 2.951, &#x27;epoch&#x27;: 0.17}\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Model weights saved in clip_chinese_02\\checkpoint-4000\\pytorch_model.bin\n&quot;,
      &quot;Deleting older checkpoint [clip_chinese_02\\checkpoint-60] due to args.save_total_limit\n&quot;,
      &quot;c:\\Users\\yuanz\\anaconda3\\envs\\mynet2\\lib\\site-packages\\PIL\\Image.py:979: UserWarning: Palette images with Transparency expressed in bytes should be converted to RGBA images\n&quot;,
      &quot;  warnings.warn(\n&quot;,
      &quot;***** Running Evaluation *****\n&quot;,
      &quot;  Num examples = 270\n&quot;,
      &quot;  Batch size = 28\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;loss&#x27;: 3.3321, &#x27;learning_rate&#x27;: 0.00044850157434820096, &#x27;epoch&#x27;: 0.21}\n&quot;
     ]
    },
    {
     &quot;data&quot;: {
      &quot;application/vnd.jupyter.widget-view+json&quot;: {
       &quot;model_id&quot;: &quot;5d33909f7bfb4ca9a73631b4a7a205cb&quot;,
       &quot;version_major&quot;: 2,
       &quot;version_minor&quot;: 0
      },
      &quot;text/plain&quot;: [
       &quot;  0%|          | 0/10 [00:00&lt;?, ?it/s]&quot;
      ]
     },
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;display_data&quot;
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Saving model checkpoint to clip_chinese_02\\checkpoint-5000\n&quot;,
      &quot;Configuration saved in clip_chinese_02\\checkpoint-5000\\config.json\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;eval_loss&#x27;: 3.3026041984558105, &#x27;eval_runtime&#x27;: 3.3772, &#x27;eval_samples_per_second&#x27;: 79.949, &#x27;eval_steps_per_second&#x27;: 2.961, &#x27;epoch&#x27;: 0.21}\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Model weights saved in clip_chinese_02\\checkpoint-5000\\pytorch_model.bin\n&quot;,
      &quot;Deleting older checkpoint [clip_chinese_02\\checkpoint-1000] due to args.save_total_limit\n&quot;,
      &quot;c:\\Users\\yuanz\\anaconda3\\envs\\mynet2\\lib\\site-packages\\PIL\\Image.py:979: UserWarning: Palette images with Transparency expressed in bytes should be converted to RGBA images\n&quot;,
      &quot;  warnings.warn(\n&quot;,
      &quot;***** Running Evaluation *****\n&quot;,
      &quot;  Num examples = 270\n&quot;,
      &quot;  Batch size = 28\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;loss&#x27;: 3.3321, &#x27;learning_rate&#x27;: 0.000426990388338636, &#x27;epoch&#x27;: 0.25}\n&quot;
     ]
    },
    {
     &quot;data&quot;: {
      &quot;application/vnd.jupyter.widget-view+json&quot;: {
       &quot;model_id&quot;: &quot;25c610e247d64758aaffbc9a7e133334&quot;,
       &quot;version_major&quot;: 2,
       &quot;version_minor&quot;: 0
      },
      &quot;text/plain&quot;: [
       &quot;  0%|          | 0/10 [00:00&lt;?, ?it/s]&quot;
      ]
     },
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;display_data&quot;
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Saving model checkpoint to clip_chinese_02\\checkpoint-6000\n&quot;,
      &quot;Configuration saved in clip_chinese_02\\checkpoint-6000\\config.json\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;eval_loss&#x27;: 3.3026041984558105, &#x27;eval_runtime&#x27;: 3.4124, &#x27;eval_samples_per_second&#x27;: 79.122, &#x27;eval_steps_per_second&#x27;: 2.93, &#x27;epoch&#x27;: 0.25}\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Model weights saved in clip_chinese_02\\checkpoint-6000\\pytorch_model.bin\n&quot;,
      &quot;Deleting older checkpoint [clip_chinese_02\\checkpoint-2000] due to args.save_total_limit\n&quot;,
      &quot;c:\\Users\\yuanz\\anaconda3\\envs\\mynet2\\lib\\site-packages\\PIL\\Image.py:979: UserWarning: Palette images with Transparency expressed in bytes should be converted to RGBA images\n&quot;,
      &quot;  warnings.warn(\n&quot;,
      &quot;***** Running Evaluation *****\n&quot;,
      &quot;  Num examples = 270\n&quot;,
      &quot;  Batch size = 28\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;loss&#x27;: 3.3321, &#x27;learning_rate&#x27;: 0.0004024830452650919, &#x27;epoch&#x27;: 0.29}\n&quot;
     ]
    },
    {
     &quot;data&quot;: {
      &quot;application/vnd.jupyter.widget-view+json&quot;: {
       &quot;model_id&quot;: &quot;2c206738fd00496284f115a763530e49&quot;,
       &quot;version_major&quot;: 2,
       &quot;version_minor&quot;: 0
      },
      &quot;text/plain&quot;: [
       &quot;  0%|          | 0/10 [00:00&lt;?, ?it/s]&quot;
      ]
     },
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;display_data&quot;
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Saving model checkpoint to clip_chinese_02\\checkpoint-7000\n&quot;,
      &quot;Configuration saved in clip_chinese_02\\checkpoint-7000\\config.json\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;eval_loss&#x27;: 3.3026041984558105, &#x27;eval_runtime&#x27;: 3.7669, &#x27;eval_samples_per_second&#x27;: 71.676, &#x27;eval_steps_per_second&#x27;: 2.655, &#x27;epoch&#x27;: 0.29}\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Model weights saved in clip_chinese_02\\checkpoint-7000\\pytorch_model.bin\n&quot;,
      &quot;Deleting older checkpoint [clip_chinese_02\\checkpoint-3000] due to args.save_total_limit\n&quot;,
      &quot;c:\\Users\\yuanz\\anaconda3\\envs\\mynet2\\lib\\site-packages\\PIL\\Image.py:979: UserWarning: Palette images with Transparency expressed in bytes should be converted to RGBA images\n&quot;,
      &quot;  warnings.warn(\n&quot;,
      &quot;***** Running Evaluation *****\n&quot;,
      &quot;  Num examples = 270\n&quot;,
      &quot;  Batch size = 28\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;loss&#x27;: 3.4755, &#x27;learning_rate&#x27;: 0.00037537729147642094, &#x27;epoch&#x27;: 0.33}\n&quot;
     ]
    },
    {
     &quot;data&quot;: {
      &quot;application/vnd.jupyter.widget-view+json&quot;: {
       &quot;model_id&quot;: &quot;7dfb4bd92c404859aaacdd1f742c81ab&quot;,
       &quot;version_major&quot;: 2,
       &quot;version_minor&quot;: 0
      },
      &quot;text/plain&quot;: [
       &quot;  0%|          | 0/10 [00:00&lt;?, ?it/s]&quot;
      ]
     },
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;display_data&quot;
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Saving model checkpoint to clip_chinese_02\\checkpoint-8000\n&quot;,
      &quot;Configuration saved in clip_chinese_02\\checkpoint-8000\\config.json\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;eval_loss&#x27;: 27.78020477294922, &#x27;eval_runtime&#x27;: 3.3844, &#x27;eval_samples_per_second&#x27;: 79.777, &#x27;eval_steps_per_second&#x27;: 2.955, &#x27;epoch&#x27;: 0.33}\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Model weights saved in clip_chinese_02\\checkpoint-8000\\pytorch_model.bin\n&quot;,
      &quot;Deleting older checkpoint [clip_chinese_02\\checkpoint-4000] due to args.save_total_limit\n&quot;,
      &quot;c:\\Users\\yuanz\\anaconda3\\envs\\mynet2\\lib\\site-packages\\PIL\\Image.py:979: UserWarning: Palette images with Transparency expressed in bytes should be converted to RGBA images\n&quot;,
      &quot;  warnings.warn(\n&quot;,
      &quot;***** Running Evaluation *****\n&quot;,
      &quot;  Num examples = 270\n&quot;,
      &quot;  Batch size = 28\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;loss&#x27;: 3.3596, &#x27;learning_rate&#x27;: 0.00034610484869133153, &#x27;epoch&#x27;: 0.37}\n&quot;
     ]
    },
    {
     &quot;data&quot;: {
      &quot;application/vnd.jupyter.widget-view+json&quot;: {
       &quot;model_id&quot;: &quot;fcbde947a87a4a55b8052f97953a8c5c&quot;,
       &quot;version_major&quot;: 2,
       &quot;version_minor&quot;: 0
      },
      &quot;text/plain&quot;: [
       &quot;  0%|          | 0/10 [00:00&lt;?, ?it/s]&quot;
      ]
     },
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;display_data&quot;
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Saving model checkpoint to clip_chinese_02\\checkpoint-9000\n&quot;,
      &quot;Configuration saved in clip_chinese_02\\checkpoint-9000\\config.json\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;eval_loss&#x27;: 3.301671266555786, &#x27;eval_runtime&#x27;: 3.3943, &#x27;eval_samples_per_second&#x27;: 79.546, &#x27;eval_steps_per_second&#x27;: 2.946, &#x27;epoch&#x27;: 0.37}\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Model weights saved in clip_chinese_02\\checkpoint-9000\\pytorch_model.bin\n&quot;,
      &quot;Deleting older checkpoint [clip_chinese_02\\checkpoint-5000] due to args.save_total_limit\n&quot;,
      &quot;***** Running Evaluation *****\n&quot;,
      &quot;  Num examples = 270\n&quot;,
      &quot;  Batch size = 28\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;loss&#x27;: 3.3321, &#x27;learning_rate&#x27;: 0.00031519144087211273, &#x27;epoch&#x27;: 0.42}\n&quot;
     ]
    },
    {
     &quot;data&quot;: {
      &quot;application/vnd.jupyter.widget-view+json&quot;: {
       &quot;model_id&quot;: &quot;305bc3ed12f74f5cafc8d1899f764421&quot;,
       &quot;version_major&quot;: 2,
       &quot;version_minor&quot;: 0
      },
      &quot;text/plain&quot;: [
       &quot;  0%|          | 0/10 [00:00&lt;?, ?it/s]&quot;
      ]
     },
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;display_data&quot;
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Saving model checkpoint to clip_chinese_02\\checkpoint-10000\n&quot;,
      &quot;Configuration saved in clip_chinese_02\\checkpoint-10000\\config.json\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stdout&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;{&#x27;eval_loss&#x27;: 3.3030922412872314, &#x27;eval_runtime&#x27;: 3.3937, &#x27;eval_samples_per_second&#x27;: 79.56, &#x27;eval_steps_per_second&#x27;: 2.947, &#x27;epoch&#x27;: 0.42}\n&quot;
     ]
    },
    {
     &quot;name&quot;: &quot;stderr&quot;,
     &quot;output_type&quot;: &quot;stream&quot;,
     &quot;text&quot;: [
      &quot;Model weights saved in clip_chinese_02\\checkpoint-10000\\pytorch_model.bin\n&quot;,
      &quot;Deleting older checkpoint [clip_chinese_02\\checkpoint-6000] due to args.save_total_limit\n&quot;,
      &quot;c:\\Users\\yuanz\\anaconda3\\envs\\mynet2\\lib\\site-packages\\PIL\\Image.py:979: UserWarning: Palette images with Transparency expressed in bytes should be converted to RGBA images\n&quot;,
      &quot;  warnings.warn(\n&quot;
     ]
    }
   ],
   &quot;source&quot;: [
    &quot;def collate_fn(examples):\n&quot;,
    &quot;    pixel_values = torch.stack([i[&#x27;pixel_values&#x27;].squeeze(0) for i in examples])\n&quot;,
    &quot;    input_ids = torch.tensor([example[\&quot;input_ids\&quot;] for example in examples], dtype=torch.long)#torch.stack([torch.tensor(i, dtype=torch.long) for i in examples[&#x27;input_ids&#x27;]])\n&quot;,
    &quot;    attention_mask = torch.tensor([example[\&quot;attention_mask\&quot;] for example in examples], dtype=torch.long)#torch.stack([torch.tensor(i) for i in examples[&#x27;attention_mask&#x27;]])\n&quot;,
    &quot;    return {\n&quot;,
    &quot;        \&quot;pixel_values\&quot;: pixel_values,\n&quot;,
    &quot;        \&quot;input_ids\&quot;: input_ids,\n&quot;,
    &quot;        \&quot;attention_mask\&quot;: attention_mask,\n&quot;,
    &quot;        \&quot;return_loss\&quot;: True,\n&quot;,
    &quot;    }\n&quot;,
    &quot;\n&quot;,
    &quot;\n&quot;,
    &quot;train_argument = TrainingArguments(\n&quot;,
    &quot;    output_dir=\&quot;clip_chinese_02\&quot;,\n&quot;,
    &quot;    per_device_train_batch_size=28,\n&quot;,
    &quot;    per_device_eval_batch_size=28,\n&quot;,
    &quot;    evaluation_strategy=\&quot;steps\&quot;,\n&quot;,
    &quot;    eval_steps=1000,\n&quot;,
    &quot;    logging_steps=1000,\n&quot;,
    &quot;    gradient_accumulation_steps=2,\n&quot;,
    &quot;    num_train_epochs=1,\n&quot;,
    &quot;    weight_decay=0.1,\n&quot;,
    &quot;    lr_scheduler_type=\&quot;cosine\&quot;,\n&quot;,
    &quot;    learning_rate=5e-4,\n&quot;,
    &quot;    save_steps=1000,\n&quot;,
    &quot;    fp16=True,\n&quot;,
    &quot;    remove_unused_columns=False,\n&quot;,
    &quot;    save_total_limit=4\n&quot;,
    &quot;\n&quot;,
    &quot;)\n&quot;,
    &quot;\n&quot;,
    &quot;\n&quot;,
    &quot;\n&quot;,
    &quot;trainer = Trainer(\n&quot;,
    &quot;    model=model,\n&quot;,
    &quot;    args=train_argument,\n&quot;,
    &quot;    train_dataset=dataset[&#x27;train&#x27;],\n&quot;,
    &quot;    eval_dataset=dataset[&#x27;test&#x27;],\n&quot;,
    &quot;    data_collator=collate_fn,\n&quot;,
    &quot;)\n&quot;,
    &quot;trainer.train()&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;dataset[&#x27;train&#x27;][5374]#[&#x27;image_path&#x27;]&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot; &quot;
   ]
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