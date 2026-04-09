# LLVM, MLIR & RISC-V compile

> 来源：[https://learning.feishu.cn/docx/XLFRdZjuIoYzjaxmzVNc4eeAnWd](https://learning.feishu.cn/docx/XLFRdZjuIoYzjaxmzVNc4eeAnWd)

-G: Generator，编译生成LLVM的工具，有Unix Makefiles、Ninja、Xcode。

CMAKE_INSTALL_PREFIX=directory，如不指定，默认安装在/usr/local

DLLVM_DEFAULT_TARGET_TRIPLE: 默认是x86_64-unknown-linux-gnu

DLLVM_ENABLE_PROJECTS: 额外构建的LLVM子项目，如bolt;clang;clang-tools-extra;compiler-rt;cross-project-tests;libc;libclc;lld;lldb;mlir;openmp;polly;pstl;flang

bolt：Binary Optimization and Layout Tool，二进制优化和布局工具。

DLLVM_ENABLE_ASSERTIONS：断言编译，当条件不符合时打断编译。如果是Debug版本则默认开启，如Release版本则需要额外开启。

DLLVM_TARGETS_TO_BUILD:"host;RISCV;NVPTX"，host可以换成X86，NVPTX是GPU。

DLLVM_DEFAULT_TARGET_TRIPLE：指定riscv-gnu-toolchain使用的C库，如果指定glibc作为c运行时库就是riscv64-unknown-linux-gnu，编译时默认是动态链接，而spike+pk只支持静态链接，所以需要手动指定静态编译（加-static），指定newlib作为运行时库是riscv64-unknown-elf，编译时只支持静态编译。target triple: 目标三元组，最初只有三个部分，现在可以有两个到五个部分。语法：<arch><sub_arch>-<vendor>-<sys>-<env>