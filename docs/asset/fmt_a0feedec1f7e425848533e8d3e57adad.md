
# LLVM, MLIR & RISC-V 编译

> 来源：[https://learning.feishu.cn/docx/XLFRdZjuIoYzjaxmzVNc4eeAnWd](https://learning.feishu.cn/docx/XLFRdZjuIoYzjaxmzVNc4eeAnWd)

## CMake 编译选项

`-G`：Generator，编译生成`LLVM`的工具，有`Unix Makefiles`、`Ninja`、`Xcode`。

`CMAKE_INSTALL_PREFIX=directory`，如不指定，默认安装在`/usr/local`

`DLLVM_DEFAULT_TARGET_TRIPLE`：默认是`x86_64-unknown-linux-gnu`

`DLLVM_ENABLE_PROJECTS`：额外构建的`LLVM`子项目，如`bolt`;`clang`;`clang-tools-extra`;`compiler-rt`;`cross-project-tests`;`libc`;`libclc`;`lld`;`lldb`;`mlir`;`openmp`;`polly`;`pstl`;`flang`

`bolt`：Binary Optimization and Layout Tool，二进制优化和布局工具。

`DLLVM_ENABLE_ASSERTIONS`：断言编译，当条件不符合时打断编译。如果是`Debug`版本则默认开启，如`Release`版本则需要额外开启。

`DLLVM_TARGETS_TO_BUILD:"host;RISCV;NVPTX"`，`host`可以换成`X86`，`NVPTX`是`GPU`。

`DLLVM_DEFAULT_TARGET_TRIPLE`：指定`riscv-gnu-toolchain`使用的`c`库，如果指定`glibc`作为`c`运行时库就是`riscv64-unknown-linux-gnu`，编译时默认是动态链接，而`spike+pk`只支持静态链接，所以需要手动指定静态编译（加`-static`），指定`newlib`作为运行时库是`riscv64-unknown-elf`，编译时只支持静态编译。`target triple`：目标三元组，最初只有三个部分，现在可以有两个到五个部分。语法：`<arch><sub_arch>-<vendor>-<sys>-<env>`
