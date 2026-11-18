
# 机器学习建模 with 使用工具

> 来源：[https://deepwisdom.feishu.cn/docx/YkQAdznrAorlcBxNCORcJmSsnwe](https://deepwisdom.feishu.cn/docx/YkQAdznrAorlcBxNCORcJmSsnwe)

## 工具批量使用

### 任务

使用 `DataInterpreter` 对 [titanic](https://www.kaggle.com/competitions/titanic/data) 数据集进行建模预测，并在数据预处理和特征工程阶段批量调用工具（已经预先创建了多个 `data_preprocess` 和 `feature_engineering` 类型的工具，位于目录`metagpt/tools/libs`下）。

### 代码

```
python examples/di/machine_learning_with_tools.py
```

### 运行结果

数据预处理代码（其中包含 3 个工具的使用：`FillMissingValue`、`StandardScale` 和 `OneHotEncode`）如下：

```
from metagpt.tools.libs.data_preprocess import FillMissingValue, StandardScale, OneHotEncode

fill_age = FillMissingValue(features=[&#x27;Age&#x27;], strategy=&#x27;median&#x27;)
fill_embarked = FillMissingValue(features=[&#x27;Embarked&#x27;], strategy=&#x27;most_frequent&#x27;)

train_data = fill_age.fit_transform(train_data)
train_data = fill_embarked.fit_transform(train_data)

if &#x27;Cabin&#x27; in train_data.columns:
    train_data.drop(&#x27;Cabin&#x27;, axis=1, inplace=True)

numerical_features = [&#x27;Age&#x27;, &#x27;SibSp&#x27;, &#x27;Parch&#x27;, &#x27;Fare&#x27;]

scale = StandardScale(features=numerical_features)
scaled_features = scale.fit_transform(train_data)

for feature in numerical_features:
    train_data[feature] = scaled_features[feature]

categorical_features = [&#x27;Sex&#x27;, &#x27;Embarked&#x27;]
one_hot = OneHotEncode(features=categorical_features)
train_data = one_hot.fit_transform(train_data)

non_informative_columns = [&#x27;PassengerId&#x27;, &#x27;Name&#x27;, &#x27;Ticket&#x27;]
train_data.drop(columns=non_informative_columns, axis=1, inplace=True, errors=&#x27;ignore&#x27;)
train_data.head()
```

特征工程代码（其中包含 1 个工具的使用：`PolynomialExpansion`）如下：

```
from metagpt.tools.libs.feature_engineering import PolynomialExpansion
numeric_features = [&#x27;Pclass&#x27;, &#x27;Age&#x27;, &#x27;SibSp&#x27;, &#x27;Parch&#x27;, &#x27;Fare&#x27;]
poly_expansion = PolynomialExpansion(cols=numeric_features, label_col=&#x27;Survived&#x27;)
train_data = poly_expansion.fit_transform(train_data)
train_data.head()
```

### 演示

![](img/27502ed69c742114b0e585b7d84e56bb.png)

[电力负荷预测（中）.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EbHQb5QCtoaTOBxT3Iyc62ECnvs)

## 机制解释

### 工具创建

在 `metagpt.tools.libs` 目录下，我们预先定义了两类工具：数据预处理（`data_preprocess`）和特征工程（`feature_engineering`）。下面以数据预处理任务为例，给出一个简单工具的完整创建指引，包括工具的定义和注册方法。

+   首先，在 `metagpt.tools.libs` 目录下创建文件 `data_preprocess.py`。
+   在该文件中，您将定义您的数据预处理工具，并利用 `@register_tool` 装饰器一键完成工具的注册。此外，你也可以通过设置 `tags` 参数，指定工具的类别。

### 代码示例：

```
from typing import Literal

import pandas as pd
from sklearn.impute import SimpleImputer

from metagpt.tools.tool_registry import register_tool


TAGS = [&quot;data preprocessing&quot;, &quot;machine learning&quot;]

@register_tool(tags=TAGS)
class FillMissingValue:
    &quot;&quot;&quot;
    Completing missing values with simple strategies.
    &quot;&quot;&quot;

    def __init__(
        self, features: list, strategy: Literal[&quot;mean&quot;, &quot;median&quot;, &quot;most_frequent&quot;, &quot;constant&quot;] = &quot;mean&quot;, fill_value=None
    ):
        &quot;&quot;&quot;
        Initialize self.

        Args:
            features (list): Columns to be processed.
            strategy (Literal[&quot;mean&quot;, &quot;median&quot;, &quot;most_frequent&quot;, &quot;constant&quot;], optional): The imputation strategy, notice &#x27;mean&#x27; and &#x27;median&#x27; can only
                                      be used for numeric features. Defaults to &#x27;mean&#x27;.
            fill_value (int, optional): Fill_value is used to replace all occurrences of missing_values.
                                        Defaults to None.
        &quot;&quot;&quot;
        self.features = features
        self.strategy = strategy
        self.fill_value = fill_value
        self.si = None

    def fit(self, df: pd.DataFrame):
        &quot;&quot;&quot;
        Fit the FillMissingValue model.

        Args:
            df (pd.DataFrame): The input DataFrame.
        &quot;&quot;&quot;
        if len(self.features) == 0:
            return
        self.si = SimpleImputer(strategy=self.strategy, fill_value=self.fill_value)
        self.si.fit(df[self.features])

    def transform(self, df: pd.DataFrame) -&gt; pd.DataFrame:
        &quot;&quot;&quot;
        Transform the input DataFrame with the fitted model.

        Args:
            df (pd.DataFrame): The input DataFrame.

        Returns:
            pd.DataFrame: The transformed DataFrame.
        &quot;&quot;&quot;
        if len(self.features) == 0:
            return df
        new_df = df.copy()
        new_df[self.features] = self.si.transform(new_df[self.features])
        return new_df

    def fit_transform(self, df: pd.DataFrame) -&gt; pd.DataFrame:
        &quot;&quot;&quot;
        Fit and transform the input DataFrame.

        Args:
            df (pd.DataFrame): The input DataFrame.

        Returns:
            pd.DataFrame: The transformed DataFrame.
        &quot;&quot;&quot;
        self.fit(df)
        return self.transform(df)
```

在上述示例中，`FillMissingValue` 类具体实现了缺失值的填充功能，并通过 `@register_tool` 装饰器注册为数据预处理工具。

注册工具后，系统将自动根据工具函数的注释在 `metagpt.tools.schemas` 目录下为您生成每个工具生成相应的 schema 文件（如 `FillMissingValue.yml`），详细描述工具类的结构、方法以及参数类型，确保 LLM 能够准确理解并使用该工具。

Schema 文件示例（`FillMissingValue.yml`）：

```
type: class
description: Completing missing values with simple strategies.
methods:
  __init__:
    type: function
    description: &#x27;Initialize self. &#x27;
    signature:
      &#x27;(self, features: &#x27;&#x27;list&#x27;&#x27;, strategy: &quot;Literal[&#x27;&#x27;mean&#x27;&#x27;, &#x27;&#x27;median&#x27;&#x27;,
      &#x27;&#x27;most_frequent&#x27;&#x27;, &#x27;&#x27;constant&#x27;&#x27;]&quot; = &#x27;&#x27;mean&#x27;&#x27;, fill_value=None)&#x27;
    parameters:
      &#x27;Args: features (list): Columns to be processed. strategy (Literal[&quot;mean&quot;,
      &quot;median&quot;, &quot;most_frequent&quot;, &quot;constant&quot;], optional): The imputation strategy,
      notice &#x27;&#x27;mean&#x27;&#x27; and &#x27;&#x27;median&#x27;&#x27; can only be used for numeric features. Defaults
      to &#x27;&#x27;mean&#x27;&#x27;. fill_value (int, optional): Fill_value is used to replace all occurrences
      of missing_values. Defaults to None.&#x27;
  fit:
    type: function
    description: &#x27;Fit a model to be used in subsequent transform. &#x27;
    signature: &quot;(self, df: &#x27;pd.DataFrame&#x27;)&quot;
    parameters: &#x27;Args: df (pd.DataFrame): The input DataFrame.&#x27;
  fit_transform:
    type: function
    description: &#x27;Fit and transform the input DataFrame. &#x27;
    signature: &quot;(self, df: &#x27;pd.DataFrame&#x27;) -&gt; &#x27;pd.DataFrame&#x27;&quot;
    parameters:
      &#x27;Args: df (pd.DataFrame): The input DataFrame. Returns: pd.DataFrame:
      The transformed DataFrame.&#x27;
  transform:
    type: function
    description: &#x27;Transform the input DataFrame with the fitted model. &#x27;
    signature: &quot;(self, df: &#x27;pd.DataFrame&#x27;) -&gt; &#x27;pd.DataFrame&#x27;&quot;
    parameters:
      &#x27;Args: df (pd.DataFrame): The input DataFrame. Returns: pd.DataFrame:
      The transformed DataFrame.&#x27;
```

### 自动工具注册与选择

通过设置`DataInterpreter`的参数`tools`，可以人为指定使用哪些工具。如果希望使用所有工具，可以设定`tools=["<all>"]`。

当 `DataInterpreter` 启动时，它会自动注册 `tools` 列表中的所有工具。同时，在任务规划阶段，`DataInterpreter` 会根据需求为每个任务推荐可用的工具，并挑选出 `TOP-k` 供 LLM 使用。

### 工具组合与调用

在执行任务时，一旦可用的 `TOP` 工具被识别，`DataInterpreter` 会根据任务需求自动组合并调用其中的有用工具，以形成针对特定任务的代码解决方案。
