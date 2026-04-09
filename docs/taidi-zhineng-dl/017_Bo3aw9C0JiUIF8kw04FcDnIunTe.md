# Triton CPU: Triton-shared

> 来源：[https://learning.feishu.cn/docx/ALt4dWYlfo2y5BxL309cciqbnFc](https://learning.feishu.cn/docx/ALt4dWYlfo2y5BxL309cciqbnFc)

The middle-layer uses MLIR's Linalg and Tenor Dialects for operations on Triton block values. Operations on Triton pointers use the Memref Dialect.

triton shared 编译完，生成 /triton/build/tools/triton-shared-opt。

就是 MLIR 中最基本的 opt，该二进制文件可以完成从一个 dialect 向另外一个 dialect 的 lowering。

```
OVERVIEW: Triton-Shared test driver

Available Dialects: arith, builtin, cf, gpu, math, scf, triton_gpu, tt

USAGE: triton-shared-opt [options] &lt;input file&gt;
```