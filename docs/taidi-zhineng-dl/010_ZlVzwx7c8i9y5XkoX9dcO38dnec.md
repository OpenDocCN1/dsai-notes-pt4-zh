# Triton 问题排查

> 来源：[https://learning.feishu.cn/docx/SH3MdgHc6o4u4Ux2Uk6cl8ZznDb](https://learning.feishu.cn/docx/SH3MdgHc6o4u4Ux2Uk6cl8ZznDb)

必备选项

```
export TRITON_ALWAYS_COMPILE=1

export MLIR_ENABLE_DUMP=1
```

现象 Segmentation fault (core dumped)

```
gdb --args python 02-fused-softmax.py

r # run
bt # show stack trace
```

![](img/72c5399718e114ca40db61076e1a9491.png)

```
export MLIR_ENABLE_DUMP=1

python 02-fused-softmax.py
```

现象说明 -> 没有进入 triton compile pass

![](img/ecb86a9b405e55ae1282120f4aa0170a.png)

![](img/42048a0a41cbd453f28737293fdff516.png)

![](img/46d7c39d644be9e8ce7ddefb2314fe6a.png)

找到 triton python 代码的入口位置

![](img/028d65f3a66beb7f267821313f4b6573.png)

用 pdb 跟 python 代码

```
# gdb for python
import pdb; pdb.set_trace();

# 常用指令缩写: s, n, r, c
# 调用: bt
```

准确找到 python function 的定义代码 的方法

![](img/2a6fc86808afa470ac9593d913caf9c8.png)

python 调用 c++ 的工具 https://github.com/pybind/pybind11

Compiler 的关键概念: Pass