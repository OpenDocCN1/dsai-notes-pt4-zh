
# chinese_falcon

> 来源：https://v11enp9ok1h.feishu.cn/docx/FdWDdD8L0ooWf1xyKnbc9wmxnbd

## 🚀 让人人都可以用70亿参数的大模型！（没有法律风险 可商用）

- 🎯 在效果上，只希望比肩`chatglm-6b`
- ⚙️ 使用`sft`数据训练中文`bloom`、`falcon`模型。
- 🎉 提供一整套的开源解决方案。让小公司、个人都可以玩起来。可商用、无任何法务风险。

### 🔄 更新记录

#### [2023-06-04] 日更新

- 支持对 [`falcon-7b`](https://huggingface.co/tiiuae/falcon-7b) 做 `sft`～

### 📣 介绍

- ✅ 基于 [`stanford_alpaca`](https://github.com/tatsu-lab/stanford_alpaca) 项目，使用`sft`格式数据对`bloom`、`falcon`模型微调；
- ✅ 支持`deepspeed-zero2`、`deepspeed-zero3`；
- ✅ 支持自定义数据，支持大数据训练；
- ✅ 得益于`bloom`本身的能力，微调后的模型支持中文、英文、代码、法语、西班牙语等；
- ✅ 微调后的模型，中文能力显著提升；
- ✅ 支持不同尺寸`bloom`模型，如`560m`、`3b`、`7b`、`13b`；
- ✅ 支持`falcon`模型，如 [`falcon-7b`](https://huggingface.co/tiiuae/falcon-7b)；

### 🥰 体验

- 🎊 模型已经上传到`huggingface`上，欢迎来尝试：
  - `bloom-560m_chat`: 想要体验一个轻量的模型，可以体验 [`chinese_bloom_560m`](https://huggingface.co/yuanzhoulvpi/chinese_bloom_560m)
  - `bloom-7b_chat`: 想要体验一个更大的模型，可以试一试70亿参数 [`chinese_bloom_7b_chat`](https://huggingface.co/yuanzhoulvpi/chinese_bloom_7b_chat)
- 🎉 在`huggingface`上部署了一个`cpu`版本的（有点慢，毕竟是🆓） [`chinese_bloom_560_chat`](https://huggingface.co/spaces/yuanzhoulvpi/chinese_bloom_560_chat)

### 🔄 模型

- `bloom`模型支持中文、英文、代码、法语、西班牙语。具体的训练数据的语言占比如下👇。

  ![](img/0c0baac6020c6785f1863fca760babfc.png)

- `bloom-3b`: [`bigscience/bloom-3b`](https://huggingface.co/bigscience/bloom-3b)
- `bloom`系列模型: https://huggingface.co/bigscience

### 💽 数据

- 数据来源于`BelleGroup`，主要是用到这几个数据集: `BelleGroup/generated_chat_0.4M`、`BelleGroup/school_math_0.25M`、`BelleGroup/train_2M_CN`、`BelleGroup/train_1M_CN`、`BelleGroup/train_0.5M_CN`、`BelleGroup/multiturn_chat_0.8M`；
- 可以基于这些数据样式，制作自己的数据，并训练；

### ⚙️ 步骤

#### 数据部分

- 运行`data_proj/process_data.ipynb`代码；或者模仿结果，制作自己的数据集；
- 运行结束之后，有一个文件夹`data_proj/opendata`。文件夹下有若干个`json`格式的文件。

#### 运行模型

- 基础运行策略

  ```
  sh base_run.sh
  ```

- `deepspeed`运行策略

  ```
  sh ds_all.sh
  ```

### 🎯 推理代码

- `infer.ipynb`文件
- `gradio`交互界面：[`chinese_bloom_560_chat`](https://huggingface.co/spaces/yuanzhoulvpi/chinese_bloom_560_chat) 因为是使用的`huggingface`的免费的`cpu`版本，所以推理速度比较慢。

### 效果

不管是写代码还是写文案，`bloom-7b`在中文领域有极大的潜力

- example 1
  ![](img/f2351c94fc20c2875b23e976a1174c30.png)
- example 2
  ![](img/c1263acb32f8fae27a8cbf3a951d5bca.png)
- example 3
  ![](img/6bf7b8b93b02b69bf18d6af7bf7895cd.png)
  ![](img/9c9deda24ee18e3ff18efa91d4bf6948.png)
- example 4
  ![](img/5bec256faff3146a4924300684ac3c7e.png)
- example 5
  ![](img/87602f71a28759f978a610e076b48a65.png)

### 体验

- [`chinese_bloom_560m`](https://huggingface.co/spaces/yuanzhoulvpi/chinese_bloom_560_chat)模型，可以在这里体验 https://huggingface.co/spaces/yuanzhoulvpi/chinese_bloom_560_chat
- `chinese_bloom_7b`模型，可以在这里体验 `http://101.68.79.42:7861`
