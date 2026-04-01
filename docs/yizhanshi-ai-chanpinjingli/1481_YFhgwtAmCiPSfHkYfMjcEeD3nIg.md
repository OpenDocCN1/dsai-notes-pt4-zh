# processdta_01.ipynb

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Zmuqdgt3AoHApYxDuZhcNeolnRb](https://v11enp9ok1h.feishu.cn/docx/Zmuqdgt3AoHApYxDuZhcNeolnRb)

![](img/a2b8ca0ffa1806c97ba5d194a8ef27e5.png)

[processdta_01.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QcZcbTUB6ovkgex4Zg5c8i8jn9d)

```
{
 &quot;cells&quot;: [
  {
   &quot;attachments&quot;: {},
   &quot;cell_type&quot;: &quot;markdown&quot;,
   &quot;metadata&quot;: {},
   &quot;source&quot;: [
    &quot;# 下载数据&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 1,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;import os \n&quot;,
    &quot;import shutil\n&quot;,
    &quot;from pathlib import Path\n&quot;,
    &quot;import numpy as np \n&quot;,
    &quot;import pandas as pd \n&quot;,
    &quot;from joblib import Parallel, delayed\n&quot;,
    &quot;import re \n&quot;,
    &quot;from typing import List, Dict\n&quot;,
    &quot;import requests\n&quot;,
    &quot;from tqdm import tqdm&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 2,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;def download_images(url:str, image_save_path:Path) -&gt; None:\n&quot;,
    &quot;    if not image_save_path.exists():\n&quot;,
    &quot;        try:\n&quot;,
    &quot;            web = requests.get(url)\n&quot;,
    &quot;            web_status = web.status_code\n&quot;,
    &quot;            if web_status == 200:\n&quot;,
    &quot;                with open(image_save_path, mode=&#x27;wb&#x27;) as f:\n&quot;,
    &quot;                    f.write(web.content)\n&quot;,
    &quot;\n&quot;,
    &quot;        except Exception as e:\n&quot;,
    &quot;            pass\n&quot;,
    &quot;\n&quot;,
    &quot;# download_images(url=url, image_save_path=Path(\&quot;bigdata/image_data/2.png\&quot;))\n&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 3,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;image_save_dir = \&quot;bigdata/image_data\&quot;\n&quot;,
    &quot;\n&quot;,
    &quot;os.makedirs(name=image_save_dir, exist_ok=True)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 4,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;# Path(image_save_dir).joinpath(\&quot;hello.png\&quot;)\n&quot;,
    &quot;test_data = pd.read_csv(\&quot;bigdata/raw_data/test-2.6w.csv\&quot;)\n&quot;,
    &quot;train_data = pd.read_csv(\&quot;bigdata/raw_data/train-137w.csv\&quot;)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 5,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;data&quot;: {
      &quot;text/html&quot;: [
       &quot;&lt;div&gt;\n&quot;,
       &quot;&lt;style scoped&gt;\n&quot;,
       &quot;    .dataframe tbody tr th:only-of-type {\n&quot;,
       &quot;        vertical-align: middle;\n&quot;,
       &quot;    }\n&quot;,
       &quot;\n&quot;,
       &quot;    .dataframe tbody tr th {\n&quot;,
       &quot;        vertical-align: top;\n&quot;,
       &quot;    }\n&quot;,
       &quot;\n&quot;,
       &quot;    .dataframe thead th {\n&quot;,
       &quot;        text-align: right;\n&quot;,
       &quot;    }\n&quot;,
       &quot;&lt;/style&gt;\n&quot;,
       &quot;&lt;table border=\&quot;1\&quot; class=\&quot;dataframe\&quot;&gt;\n&quot;,
       &quot;  &lt;thead&gt;\n&quot;,
       &quot;    &lt;tr style=\&quot;text-align: right;\&quot;&gt;\n&quot;,
       &quot;      &lt;th&gt;&lt;/th&gt;\n&quot;,
       &quot;      &lt;th&gt;url&lt;/th&gt;\n&quot;,
       &quot;      &lt;th&gt;filename&lt;/th&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;  &lt;/thead&gt;\n&quot;,
       &quot;  &lt;tbody&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;0&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;https://gimg2.baidu.com/image_search/src=http%...&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;train-0.jpg&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;1&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;https://gss0.baidu.com/70cFfyinKgQFm2e88IuM_a/...&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;train-1.jpg&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;2&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;https://gimg2.baidu.com/image_search/src=http%...&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;train-4.jpg&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;  &lt;/tbody&gt;\n&quot;,
       &quot;&lt;/table&gt;\n&quot;,
       &quot;&lt;/div&gt;&quot;
      ],
      &quot;text/plain&quot;: [
       &quot;                                                 url     filename\n&quot;,
       &quot;0  https://gimg2.baidu.com/image_search/src=http%...  train-0.jpg\n&quot;,
       &quot;1  https://gss0.baidu.com/70cFfyinKgQFm2e88IuM_a/...  train-1.jpg\n&quot;,
       &quot;2  https://gimg2.baidu.com/image_search/src=http%...  train-4.jpg&quot;
      ]
     },
     &quot;execution_count&quot;: 5,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;all_image_data = pd.concat([train_data[[&#x27;url&#x27;, &#x27;filename&#x27;]], test_data[[&#x27;url&#x27;, &#x27;filename&#x27;]]]).reset_index(drop=True)\n&quot;,
    &quot;all_image_data.head(3)\n&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 6,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;smalldata = all_image_data.copy()#.sample(20).reset_index(drop=True)\n&quot;,
    &quot;# smalldata&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 7,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;data&quot;: {
      &quot;text/plain&quot;: [
       &quot;350&quot;
      ]
     },
     &quot;execution_count&quot;: 7,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;chunk_size = 4000\n&quot;,
    &quot;start_id = range(0, smalldata.shape[0], chunk_size)\n&quot;,
    &quot;len(list(start_id))&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 34,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;# smalldata[4:(4 + chunk_size)]&quot;
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
      &quot;main----&gt;: 100%|██████████| 350/350 [13:40&lt;00:00,  2.34s/it]\n&quot;
     ]
    }
   ],
   &quot;source&quot;: [
    &quot;for i in tqdm(start_id, desc=&#x27;main----&gt;&#x27;):\n&quot;,
    &quot;    temp_data = smalldata[i:(i+chunk_size)].copy().reset_index(drop=True)\n&quot;,
    &quot;    _ = Parallel(n_jobs=10)(delayed(download_images)(row.url, Path(image_save_dir).joinpath(row.filename)) for index, row in temp_data.iterrows())\n&quot;
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