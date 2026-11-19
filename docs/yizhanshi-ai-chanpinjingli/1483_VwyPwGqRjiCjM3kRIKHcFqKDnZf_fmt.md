
# `processdta_03.ipynb`

> 来源：[https://v11enp9ok1h.feishu.cn/docx/🐧YzdoU6totDEExi3IOcrT6ynQd](https://v11enp9ok1h.feishu.cn/docx/🐧YzdoU6totDEExi3IOcrT6ynQd)

![](img/2043c83a8556b47df61d35118a29ccd6.png)

[`processdta_03.ipynb`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ERWJbjYgXoagwMxU4DOcWOZ7nag)



# 制作干净的数据

## 导入依赖库

```python
import numpy as np
import pandas as pd
from pathlib import Path
import os
import shutil
import imghdr
from glob import glob
from tqdm import tqdm
```

## 加载图片路径数据

```python
image_path_df = pd.read_csv("bigdata/temp_dir/temp_image_url.csv")
image_path_df.head(3)
```

## 加载训练集与测试集文本数据

```python
train_test_data = pd.concat([
    pd.read_csv("bigdata/raw_data/test-2.6w.csv"),
    pd.read_csv("bigdata/raw_data/train-137w.csv")
]).reset_index(drop=True)
train_test_data.head(3)
```

## 清洗并合并数据

```python
clean_train_test_df = image_path_df.pipe(
    lambda x: x.assign(**{
        'filename': x['image_path'].apply(lambda j: Path(j).name)
    })
).pipe(
    lambda x: x.merge(
        right=train_test_data,
        how='left',
        left_on=['filename'],
        right_on=['filename']
    )
)[['image_path', 'text']].pipe(
    lambda x: x.assign(**{
        'train_or_test': x['image_path'].apply(
            lambda j: 'train' if j.find('train') != -1 else 'test'
        )
    })
).pipe(
    lambda x: x.assign(**{
        'image_path': x['image_path'].apply(lambda j: j.replace('\\', '/'))
    })
)

clean_train_test_df.head(3)
```

## 创建输出目录

```python
clean_train_test_dir = "bigdata/clean_train_test"
if Path(clean_train_test_dir).exists():
    shutil.rmtree(clean_train_test_dir, ignore_errors=True)

os.makedirs(clean_train_test_dir)
```

## 导出测试集和训练集

```python
clean_train_test_df.query('train_or_test == "test"').drop(columns=['train_or_test']).to_csv(
    Path(clean_train_test_dir).joinpath("test.csv"), index=False
)
clean_train_test_df.query('train_or_test == "train"').drop(columns=['train_or_test']).to_csv(
    Path(clean_train_test_dir).joinpath("train.csv"), index=False
)
```

## 导出采样后的小数据集

```python
clean_train_test_df.query('train_or_test == "test"').drop(columns=['train_or_test']).sample(200).to_csv(
    Path(clean_train_test_dir).joinpath("test_small.csv"), index=False
)
clean_train_test_df.query('train_or_test == "train"').drop(columns=['train_or_test']).sample(1000).to_csv(
    Path(clean_train_test_dir).joinpath("train_small.csv"), index=False
)
```

## 计算训练集文本长度中位数

```python
clean_train_test_df.query('train_or_test == "train"').sample(20000).pipe(
    lambda x: x['text'].apply(lambda j: len(j))
).median()
```

## 注释掉的查询语句

```python
# clean_train_test_df.query('train_or_test == "train"')
```
