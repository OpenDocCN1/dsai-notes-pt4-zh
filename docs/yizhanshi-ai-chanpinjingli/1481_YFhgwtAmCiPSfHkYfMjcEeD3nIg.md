
# `processdta_01.ipynb`

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Zmuqdgt3AoHApYxDuZhcNeolnRb](https://v11enp9ok1h.feishu.cn/docx/Zmuqdgt3AoHApYxDuZhcNeolnRb)

![](img/a2b8ca0ffa1806c97ba5d194a8ef27e5.png)

[`processdta_01.ipynb`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QcZcbTUB6ovkgex4Zg5c8i8jn9d)



# 下载数据

## 导入依赖

```python
import os
import shutil
from pathlib import Path
import numpy as np
import pandas as pd
from joblib import Parallel, delayed
import re
from typing import List, Dict
import requests
from tqdm import tqdm
```

## 定义下载函数

```python
def download_images(url: str, image_save_path: Path) -> None:
    if not image_save_path.exists():
        try:
            web = requests.get(url)
            web_status = web.status_code
            if web_status == 200:
                with open(image_save_path, mode='wb') as f:
                    f.write(web.content)

        except Exception as e:
            pass

# download_images(url=url, image_save_path=Path("bigdata/image_data/2.png"))
```

## 创建保存目录

```python
image_save_dir = "bigdata/image_data"

os.makedirs(name=image_save_dir, exist_ok=True)
```

## 读取数据

```python
# Path(image_save_dir).joinpath("hello.png")
test_data = pd.read_csv("bigdata/raw_data/test-2.6w.csv")
train_data = pd.read_csv("bigdata/raw_data/train-137w.csv")
```

## 查看数据前几行

```python
all_image_data = pd.concat([train_data[['url', 'filename']], test_data[['url', 'filename']]]).reset_index(drop=True)
all_image_data.head(3)
```

输出：

```
                                                 url     filename
0  https://gimg2.baidu.com/image_search/src=http%...  train-0.jpg
1  https://gss0.baidu.com/70cFfyinKgQFm2e88IuM_a/...  train-1.jpg
2  https://gimg2.baidu.com/image_search/src=http%...  train-4.jpg
```

## 复制数据子集

```python
smalldata = all_image_data.copy()#.sample(20).reset_index(drop=True)
# smalldata
```

## 计算分块数量

```python
chunk_size = 4000
start_id = range(0, smalldata.shape[0], chunk_size)
len(list(start_id))
```

输出：

```
350
```

## 注释掉的分片操作

```python
# smalldata[4:(4 + chunk_size)]
```

## 并行下载图像

```python
for i in tqdm(start_id, desc='main---->'):
    temp_data = smalldata[i:(i+chunk_size)].copy().reset_index(drop=True)
    _ = Parallel(n_jobs=10)(delayed(download_images)(row.url, Path(image_save_dir).joinpath(row.filename)) for index, row in temp_data.iterrows())
```

输出：

```
main---->: 100%|██████████| 350/350 [13:40<00:00,  2.34s/it]
```
