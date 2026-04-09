# 体系结构-量化研究方法

> 来源：[https://learning.feishu.cn/docx/CoQLdQDoYoYvqZx1RfWc68bwnTf](https://learning.feishu.cn/docx/CoQLdQDoYoYvqZx1RfWc68bwnTf)

# Chap4 - 向量、SIMD 和 GPU 中的数据并行

SIMD 的三种变体

+   vector processor。向量体系结构。

+   SIMD 指令集扩展：MMX，SSE，AVX 等。

+   SIMT

向量体系结构，比其他 2 个早 30 年以上。

以 pipeline 的形式，执行许多数据操作。

传统认为，成本太高，来自 2 点：

1.  晶体管

1.  DRAM bandwidth