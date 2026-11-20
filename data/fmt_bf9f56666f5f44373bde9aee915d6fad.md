
# TVM 源码编译

> 来源：[https://learning.feishu.cn/docx/E1iwdwfE5oy7pBxmEMNc4ppznrc](https://learning.feishu.cn/docx/E1iwdwfE5oy7pBxmEMNc4ppznrc)

# 编译 TVM

Doc: https://tvm.apache.org/docs/install/from_source.html

结果：成功安装 0.13.dev0

## 目标

编译出 2 个 shared libraries：`libtvm` 和 `libtvm_runtime`

不同的 OS 下，扩展名不同：

+   Linux: `libtvm.so` and `libtvm_runtime.so`

+   macOS: `libtvm.dylib` and `libtvm_runtime.dylib`

+   Windows: `libtvm.dll` and `libtvm_runtime.dll`

部署的时候，只需要 `libtvm_runtime`

Doc: https://tvm.apache.org/docs/how_to/deploy/index.html

## 环境依赖

Compiler 需要支持 c++ 17

Python 3.7 或 3.8

不支持 `python` 3.9+ https://github.com/apache/tvm/issues/8577

## 更新 `PYTHONPATH`

```
SCRIPT_DIR=$(cd $(dirname "${BASH_SOURCE[0]:-$0}") && pwd)

export TVM_HOME=$SCRIPT_DIR/tvm
export PYTHONPATH=$TVM_HOME/python:${PYTHONPATH}

python -c 'import tvm; print("tvm version: %s" % tvm.__version__)'
# tvm version: 0.13.dev0
```

Mac 下的 `llvm` post-installation steps

```
To use the bundled libc++ please add the following LDFLAGS:
  LDFLAGS="-L/usr/local/opt/llvm/lib/c++ -Wl,-rpath,/usr/local/opt/llvm/lib/c++"

llvm is keg-only, which means it was not symlinked into /usr/local,
because macOS already provides this software and installing another version in
parallel can cause all kinds of trouble.

If you need to have llvm first in your PATH, run:
  echo 'export PATH="/usr/local/opt/llvm/bin:$PATH"' >> ~/.zshrc

For compilers to find llvm you may need to set:
  export LDFLAGS="-L/usr/local/opt/llvm/lib"
  export CPPFLAGS="-I/usr/local/opt/llvm/include"
```

# TVM 介绍

![](img/d0cc720121624bce9e6f6286bfa417d7.png)

`Relay` is a functional language and intermediate representation (`IR`) for neural networks. It has support for:

+   Traditional data flow-style representations

+   Functional-style scoping, `let`-binding which makes it a fully featured differentiable language

+   Ability to allow the user to mix the two programming styles

`Relay` applies graph-level optimization passes to optimize the model.

`TE` also provides several schedule primitives to specify low-level loop optimizations, such as tiling, vectorization, parallelization, unrolling, and fusion.

To aid in the process of converting `Relay` representation into `TE` representation, `TVM` includes a Tensor Operator Inventory (`TOPI`) that has pre-defined templates of common tensor operators (e.g., `conv2d`, `transpose`).

2 auto-tuning module:

1.  `AutoTVM`: template-based

1.  `AutoScheduler` (a.k.a. `Ansor`): template-free

Tensor Intermediate Representation (`TIR`)

target compiler of the hardware platform

+   `LLVM`, which can target arbitrary microprocessor architecture including standard x86 and ARM processors, `AMDGPU` and `NVPTX` code generation, and any other platform supported by `LLVM`.

+   Specialized compilers, such as `NVCC`, NVIDIA’s compiler.

+   Embedded and specialized targets, which are implemented through `TVM`’s Bring Your Own Codegen (`BYOC`) framework.

Compile down to machine code.

a linkable object module

can then be run with a lightweight `TVM` runtime
