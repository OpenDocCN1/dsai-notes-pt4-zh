# processdta_03.ipynb

> 来源：[https://v11enp9ok1h.feishu.cn/docx/🐧YzdoU6totDEExi3IOcrT6ynQd](https://v11enp9ok1h.feishu.cn/docx/🐧YzdoU6totDEExi3IOcrT6ynQd)

![](img/2043c83a8556b47df61d35118a29ccd6.png)

[processdta_03.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ERWJbjYgXoagwMxU4DOcWOZ7nag)

```
{
 &quot;cells&quot;: [
  {
   &quot;attachments&quot;: {},
   &quot;cell_type&quot;: &quot;markdown&quot;,
   &quot;metadata&quot;: {},
   &quot;source&quot;: [
    &quot;# 制作干净的数据&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 1,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;import numpy as np\n&quot;,
    &quot;import pandas as pd \n&quot;,
    &quot;from pathlib import Path\n&quot;,
    &quot;import os \n&quot;,
    &quot;import shutil\n&quot;,
    &quot;import imghdr\n&quot;,
    &quot;from glob import glob\n&quot;,
    &quot;from tqdm import tqdm &quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 2,
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
       &quot;      &lt;th&gt;image_path&lt;/th&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;  &lt;/thead&gt;\n&quot;,
       &quot;  &lt;tbody&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;0&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;bigdata/image_data\\test-0.jpg&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;1&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;bigdata/image_data\\test-1.jpg&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;2&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;bigdata/image_data\\test-10.jpg&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;  &lt;/tbody&gt;\n&quot;,
       &quot;&lt;/table&gt;\n&quot;,
       &quot;&lt;/div&gt;&quot;
      ],
      &quot;text/plain&quot;: [
       &quot;                       image_path\n&quot;,
       &quot;0   bigdata/image_data\\test-0.jpg\n&quot;,
       &quot;1   bigdata/image_data\\test-1.jpg\n&quot;,
       &quot;2  bigdata/image_data\\test-10.jpg&quot;
      ]
     },
     &quot;execution_count&quot;: 2,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;image_path_df = pd.read_csv(\&quot;bigdata/temp_dir/temp_image_url.csv\&quot;)\n&quot;,
    &quot;image_path_df.head(3)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 3,
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
       &quot;      &lt;th&gt;text&lt;/th&gt;\n&quot;,
       &quot;      &lt;th&gt;url&lt;/th&gt;\n&quot;,
       &quot;      &lt;th&gt;filename&lt;/th&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;  &lt;/thead&gt;\n&quot;,
       &quot;  &lt;tbody&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;0&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;欧美夏季ebay连衣裙 气质圆领通勤绑带收腰连衣裙 zc3730&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;https://gimg2.baidu.com/image_search/src=http%...&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;test-0.jpg&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;1&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;曾是名不见经传的王平,为何能够取代魏延,成为蜀汉&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;https://pic.rmb.bdstatic.com/19539b3b1a7e1daee...&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;test-1.jpg&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;2&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;女童黄色连衣裙&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;https://gimg2.baidu.com/image_search/src=http%...&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;test-2.jpg&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;  &lt;/tbody&gt;\n&quot;,
       &quot;&lt;/table&gt;\n&quot;,
       &quot;&lt;/div&gt;&quot;
      ],
      &quot;text/plain&quot;: [
       &quot;                               text  \\\n&quot;,
       &quot;0  欧美夏季ebay连衣裙 气质圆领通勤绑带收腰连衣裙 zc3730   \n&quot;,
       &quot;1          曾是名不见经传的王平,为何能够取代魏延,成为蜀汉   \n&quot;,
       &quot;2                           女童黄色连衣裙   \n&quot;,
       &quot;\n&quot;,
       &quot;                                                 url    filename  \n&quot;,
       &quot;0  https://gimg2.baidu.com/image_search/src=http%...  test-0.jpg  \n&quot;,
       &quot;1  https://pic.rmb.bdstatic.com/19539b3b1a7e1daee...  test-1.jpg  \n&quot;,
       &quot;2  https://gimg2.baidu.com/image_search/src=http%...  test-2.jpg  &quot;
      ]
     },
     &quot;execution_count&quot;: 3,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;train_test_data = pd.concat([pd.read_csv(\&quot;bigdata/raw_data/test-2.6w.csv\&quot;), pd.read_csv(\&quot;bigdata/raw_data/train-137w.csv\&quot;)]).reset_index(drop=True)\n&quot;,
    &quot;train_test_data.head(3)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 4,
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
       &quot;      &lt;th&gt;image_path&lt;/th&gt;\n&quot;,
       &quot;      &lt;th&gt;text&lt;/th&gt;\n&quot;,
       &quot;      &lt;th&gt;train_or_test&lt;/th&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;  &lt;/thead&gt;\n&quot;,
       &quot;  &lt;tbody&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;0&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;bigdata/image_data/test-0.jpg&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;欧美夏季ebay连衣裙 气质圆领通勤绑带收腰连衣裙 zc3730&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;test&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;1&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;bigdata/image_data/test-1.jpg&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;曾是名不见经传的王平,为何能够取代魏延,成为蜀汉&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;test&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;2&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;bigdata/image_data/test-10.jpg&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;考勤加班明细表&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;test&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;  &lt;/tbody&gt;\n&quot;,
       &quot;&lt;/table&gt;\n&quot;,
       &quot;&lt;/div&gt;&quot;
      ],
      &quot;text/plain&quot;: [
       &quot;                       image_path                              text  \\\n&quot;,
       &quot;0   bigdata/image_data/test-0.jpg  欧美夏季ebay连衣裙 气质圆领通勤绑带收腰连衣裙 zc3730   \n&quot;,
       &quot;1   bigdata/image_data/test-1.jpg          曾是名不见经传的王平,为何能够取代魏延,成为蜀汉   \n&quot;,
       &quot;2  bigdata/image_data/test-10.jpg                           考勤加班明细表   \n&quot;,
       &quot;\n&quot;,
       &quot;  train_or_test  \n&quot;,
       &quot;0          test  \n&quot;,
       &quot;1          test  \n&quot;,
       &quot;2          test  &quot;
      ]
     },
     &quot;execution_count&quot;: 4,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;clean_train_test_df = image_path_df.pipe(\n&quot;,
    &quot;    lambda x: x.assign(**{\n&quot;,
    &quot;        &#x27;filename&#x27;:x[&#x27;image_path&#x27;].apply(lambda j: Path(j).name)\n&quot;,
    &quot;    })\n&quot;,
    &quot;).pipe(\n&quot;,
    &quot;    lambda x: x.merge(\n&quot;,
    &quot;        right=train_test_data,\n&quot;,
    &quot;        how=&#x27;left&#x27;,\n&quot;,
    &quot;        left_on=[&#x27;filename&#x27;],\n&quot;,
    &quot;        right_on=[&#x27;filename&#x27;]\n&quot;,
    &quot;    )\n&quot;,
    &quot;)[[&#x27;image_path&#x27;, &#x27;text&#x27;]].pipe(\n&quot;,
    &quot;    lambda x: x.assign(**{\n&quot;,
    &quot;        &#x27;train_or_test&#x27;:x[&#x27;image_path&#x27;].apply(\n&quot;,
    &quot;            lambda j: &#x27;train&#x27; if j.find(&#x27;train&#x27;) != -1 else &#x27;test&#x27;\n&quot;,
    &quot;        )\n&quot;,
    &quot;    })\n&quot;,
    &quot;).pipe(\n&quot;,
    &quot;    lambda x: x.assign(**{\n&quot;,
    &quot;    &#x27;image_path&#x27;:x[&#x27;image_path&#x27;].apply(lambda j : j.replace(&#x27;\\\\&#x27;, &#x27;/&#x27;))\n&quot;,
    &quot;    })\n&quot;,
    &quot;)\n&quot;,
    &quot;\n&quot;,
    &quot;clean_train_test_df.head(3)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 5,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;clean_train_test_dir = \&quot;bigdata/clean_train_test\&quot;\n&quot;,
    &quot;if Path(clean_train_test_dir).exists():\n&quot;,
    &quot;    shutil.rmtree(clean_train_test_dir, ignore_errors=True)\n&quot;,
    &quot;\n&quot;,
    &quot;os.makedirs(clean_train_test_dir)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 6,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;clean_train_test_df.query(&#x27;train_or_test == \&quot;test\&quot;&#x27;).drop(columns=[&#x27;train_or_test&#x27;]).to_csv(Path(clean_train_test_dir).joinpath(\&quot;test.csv\&quot;), index=False)\n&quot;,
    &quot;clean_train_test_df.query(&#x27;train_or_test == \&quot;train\&quot;&#x27;).drop(columns=[&#x27;train_or_test&#x27;]).to_csv(Path(clean_train_test_dir).joinpath(\&quot;train.csv\&quot;), index=False)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 7,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;clean_train_test_df.query(&#x27;train_or_test == \&quot;test\&quot;&#x27;).drop(columns=[&#x27;train_or_test&#x27;]).sample(200).to_csv(Path(clean_train_test_dir).joinpath(\&quot;test_small.csv\&quot;), index=False)\n&quot;,
    &quot;clean_train_test_df.query(&#x27;train_or_test == \&quot;train\&quot;&#x27;).drop(columns=[&#x27;train_or_test&#x27;]).sample(1000).to_csv(Path(clean_train_test_dir).joinpath(\&quot;train_small.csv\&quot;), index=False)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 8,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;data&quot;: {
      &quot;text/plain&quot;: [
       &quot;24.0&quot;
      ]
     },
     &quot;execution_count&quot;: 8,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;clean_train_test_df.query(&#x27;train_or_test == \&quot;train\&quot;&#x27;).sample(20000).pipe(\n&quot;,
    &quot;    lambda x:x[&#x27;text&#x27;].apply(lambda j: len(j))\n&quot;,
    &quot;).median()&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 9,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;# clean_train_test_df.query(&#x27;train_or_test == \&quot;train\&quot;&#x27;)&quot;
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