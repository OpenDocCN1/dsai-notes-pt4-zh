
```python
                                          auto_run = auto_run)

                return messages

    # While条件不满足，或执行完While循环之后，提取返回结果

    final_response = response_message

    display(Markdown(final_response.content))

    messages.append(final_response.model_dump())

    return messages
```

## 函数功能测试

```python
messages = [
    {"role": "user", "content": "你好"},
]

```

```python
response_message = check_code_run(messages=messages)
```

你好👋！我是人工智能助手智谱清言，可以叫我小智🤖，很高兴见到你，欢迎问我任何问题。

```python
messages = [
    {"role": "system", "content": data_dictionary},
    {"role": "user", "content": "请问什么是机器学习？"},
]

```

```python
functions_list = [python_inter]
```

```python
tools = [
    {
        "type": "function",
        "function": python_inter_function_info
    }
]

```

```python
response_message = check_code_run(messages=messages, 
                                  functions_list = functions_list, 
                                  tools = tools)
```

## 机器学习简介

机器学习是人工智能（AI）的一个分支，它赋予了机器从数据中学习并做出决策或预测的能力。而不是依赖于明确的编程指令来执行特定任务。机器学习算法利用统计学方法，让计算机系统通过对数据的分析和处理学习，从而使系统能够对新数据做出预测或决策。

机器学习可以分为几种主要类型：

1. 监督学习（Supervised Learning）: 在这种模式下，算法通过一个已标记的数据集进行学习，也就是说，每个训练样本都有一个对应的标签或答案。监督学习的目标是训练出一个模型，能够对新的、未见过的数据进行准确的预测或分类。

2. 无监督学习（Unsupervised Learning）: 算法在没有标记的数据集上进行训练，目的是发现数据中的模式或结构。无监督学习不需要外界告诉它什么是正确的输出，而是通过探索数据来发现数据之间的关联性。

3. 半监督学习（Semi-supervised Learning）: 这种方法介于监督学习和无监督学习之间，其中一部分数据是标记的，但大部分数据是未标记的。

4. 强化学习（Reinforcement Learning）: 在这种类型中，算法（通常被称为“智能体”）通过与环境进行交互来学习如何完成特定任务。智能体接收到环境反馈，并根据这些反馈调整其行为。

机器学习在各个领域都有广泛的应用，包括但不限于图像和语音识别、自然语言处理、医疗诊断、金融预测、推荐系统等。随着技术的发展和数据的增长，机器学习的作用和影响力日益增大，成为现代社会不可或缺的一部分。

## telco_data 数据集加载示例

```python
messages = [
    {"role": "system", "content": data_dictionary},
    {"role": "user", "content": "请帮我读取telco_data数据集到本地Python环境，并将其命名为data"},
]

```

```python
response_message = check_code_run(messages=messages, 
                                  functions_list = functions_list, 
                                  tools = tools)
```

正在调用外部函数...

即将执行以下代码：

```python
data = pd.read_csv("telco_data.csv")
```

正在执行代码，请稍后...

外部函数已运行完毕

数据集 `telco_data.csv` 已被成功读取到本地Python环境中，并命名为 `data`。您现在可以使用这个变量名 `data` 来访问数据集并进行进一步的数据处理和分析。如果您需要进行数据清洗、探索性数据分析或者模型构建，请告诉我您的具体需求。

```python
data
```

|  | customerID | gender | SeniorCitizen | Partner | Dependents | tenure | PhoneService | MultipleLines | InternetService | OnlineSecurity | ... | DeviceProtection | TechSupport | StreamingTV | StreamingMovies | Contract | PaperlessBilling | PaymentMethod | MonthlyCharges | TotalCharges | Churn |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 7590-VHVEG | Female | 0 | Yes | No | 1 | No | No phone service | DSL | No | ... | No | No | No | No | Month-to-month | Yes | Electronic check | 29.85 | 29.85 | No |
| 1 | 5575-GNVDE | Male | 0 | No | No | 34 | Yes | No | DSL | Yes | ... | Yes | No | No | No | One year | No | Mailed check | 56.95 | 1889.5 | No |
| 2 | 3668-QPYBK | Male | 0 | No | No | 2 | Yes | No | DSL | Yes | ... | No | No | No | No | Month-to-month | Yes | Mailed check | 53.85 | 108.15 | Yes |
| 3 | 7795-CFOCW | Male | 0 | No | No | 45 | No | No phone service | DSL | Yes | ... | Yes | Yes | No | No | One year | No | Bank transfer (automatic) | 42.30 | 1840.75 | No |
| 4 | 9237-HQITU | Female | 0 | No | No | 2 | Yes | No | Fiber optic | No | ... | No | No | No | No | Month-to-month | Yes | Electronic check | 70.70 | 151.65 | Yes |
| ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |
| 7038 | 6840-RESVB | Male | 0 | Yes | Yes | 24 | Yes | Yes | DSL | Yes | ... | Yes | Yes | Yes | Yes | One year | Yes | Mailed check | 84.80 | 1990.5 | No |
| 7039 | 2234-XADUH | Female | 0 | Yes | Yes | 72 | Yes | Yes | Fiber optic | No | ... | Yes | No | Yes | Yes | One year | Yes | Credit card (automatic) | 103.20 | 7362.9 | No |
| 7040 | 4801-JZAZL | Female | 0 | Yes | Yes | 11 | No | No phone service | DSL | Yes | ... | No | No | No | No | Month-to-month | Yes | Electronic check | 29.60 | 346.45 | No |
| 7041 | 8361-LTMKD | Male | 1 | Yes | No | 4 | Yes | Yes | Fiber optic | No | ... | No | No | No | No | Month-to-month | Yes | Mailed check | 74.40 | 306.6 | Yes |
| 7042 | 3186-AJIEK | Male | 0 | No | No | 66 | Yes | No | Fiber optic | Yes | ... | Yes | Yes | Yes | Yes | Two year | Yes | Bank transfer (automatic) | 105.65 | 6844.5 | No |

7043 rows × 21 columns

```python
messages
```

```json
[{'role': 'system',
  'content': '# telco_data_dictionary：telco_data数据集说明

本数据字典记录了telco_data数据集基本情况，telco_data数据集是一个电信用户流失数据集，用于描述电信用户基本情况与目前是否流失状态。

- 数据集本地存储地址

  telco_data数据集存储在本地文件夹中，存储地址在当前代码目录下，文件名为telco_data.csv。

- 数据集字段解释

| Column Name | Description | Value Range | Value Explanation |
|-------------|-------------|-------------|-------------------|
| customerID | 客户ID，user_demographics数据表主键 |              | 由数字和字母组成的 |
| gender | 用户的性别 | Female, Male | Female (女性), Male (男性) |
| SeniorCitizen | 是否为老人 | 0, 1 | 0 (不是), 1 (是) |
| Partner | 用户是否有伴侣 | Yes, No | Yes (有), No (没有) |
| Dependents | 用户经济是否独立，往往用于判断用户是否已经成年 | No, Yes | Yes (有), No (没有) |
| PhoneService | 用户是否有电话服务 | No, Yes | Yes (有), No (没有) |
| MultipleLines | 用户是否开通了多条电话业务 | No phone service, No, Yes | Yes (有多条电话线业务), No (没有多条电话线业务), No phone service (没有电话服务) |
| InternetService | 用户的互联网服务类型 | DSL, Fiber optic, No | DSL (DSL), Fiber optic (光纤), No (没有) |
| OnlineSecurity | 是否开通网络安全服务 | No, Yes, No internet service | Yes（有）、No（无） or No internetservice（没有网路服务） |
| OnlineBackup | 是否开通在线备份服务 | Yes, No, No internet service | Yes（有）、No（无） or No internetservice（没有网路服务） |
| DeviceProtection | 是否开通设备保护服务 | No, Yes, No internet service | Yes（有）、No（无） or No internetservice（没有网路服务） |
| TechSupport | 是否开通技术支持业务 | No, Yes, No internet service | Yes（有）、No（无） or No internetservice（没有网路服务） |
| StreamingTV | 是否开通网络电视 | No, Yes, No internet service | Yes（有）、No（无） or No internetservice（没有网路服务） |
| tenure | 用户入网时间 |  |  |
| Contract | 合同类型 | Month-to-month, One year, Two year | Month-to-month (月付), One year (一年付), Two year (两年付) |
| PaperlessBilling | 是否无纸化账单 | Yes, No | Yes (是), No (否) |
| PaymentMethod | 支付方式 | Electronic check, Mailed check, Bank transfer (automatic), Credit card (automatic) | Electronic check (电子检查), Mailed check (邮寄支票), Bank transfer (automatic) (银行转账), Credit card (automatic) (信用卡) |
| MonthlyCharges | 月费用 |  | 用户平均每月支付费用 |
| TotalCharges | 总费用 |  | 截至目前用户总消费金额 |
| Churn | 用户是否流失 | No, Yes | Yes (是), No (否) |

'},
 {'role': 'user', 'content': '请帮我读取telco_data数据集到本地Python环境，并将其命名为data'},
 {'content': None,
  'role': 'assistant',
  'tool_calls': [{'id': 'call_8356085305118912194',
    'function': {'arguments': '{"py_code":"data = pd.read_csv(\\"telco_data.csv\\")"}',
     'name': 'python_inter'},
    'type': 'function'}]},
 {'role': 'tool',
  'content': '已经顺利执行代码',
  'tool_call_id': 'call_8356085305118912194'},
 {'content': '数据集 `telco_data.csv` 已被成功读取到本地Python环境中，并命名为 `data`。您现在可以使用这个变量名 `data` 来访问数据集并进行进一步的数据处理和分析。如果您需要进行数据清洗、探索性数据分析或者模型构建，请告诉我您的具体需求。',
  'role': 'assistant',
  'tool_calls': None}]
```

```python
messages.append({"role": "user", "content": "请帮我查询data数据集数据缺失情况"})
```

```python
messages
```

```json
[{'role': 'system',
