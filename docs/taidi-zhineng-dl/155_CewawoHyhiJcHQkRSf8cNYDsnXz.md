# Torch, CUDA and MLIR env

> 来源：[https://learning.feishu.cn/docx/YjCTd2XrLolkEixBWw5cixF4n6f](https://learning.feishu.cn/docx/YjCTd2XrLolkEixBWw5cixF4n6f)

2023.11.21, pytorch 最新 stable 是 2.1。

使用 cuda 11.x 可以，但 cuda 12 不行。

安装后，import torch 报错 so 文件问题。

设置 cuda 环境变量

```
# cuda_path=&quot;/usr/local/cuda-11.3&quot;
cuda_path=&quot;/usr/local/cuda&quot;

export PATH=&quot;$cuda_path/bin:$PATH&quot;
export LD_LIBRARY_PATH=&quot;$cuda_path/lib64:$LD_LIBRARY_PATH&quot;
```