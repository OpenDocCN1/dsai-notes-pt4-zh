
# CS143 编译原理笔记 - 提纲

> 来源：[https://learning.feishu.cn/docx/PAYgd99dcoLI9oxAOEAcpFAxnCd](https://learning.feishu.cn/docx/PAYgd99dcoLI9oxAOEAcpFAxnCd)

## 课程整体信息

| 课程主页 | Stanford CS143 编译原理 `https://web.stanford.edu/class/cs143/` |
| --- | --- |
| 视频 | B 站: [https://www.bilibili.com](https://www.bilibili.com/video/BV1NE411376V/) `/video/BV1NE411376V/` |
| 时长 | 1. 总时长约 18h。分 10 个 week 18 个 Lecture。 |
| 点评 | 1. 非常好的课程，通俗易懂，提纲挈领。比自己读龙书愉快很多。 |
| 后续课程 | 这是 `cs143`， 还有 `cs243` 和 `cs343`。 |

## 学习进展

| weeks | topic | 视频时长 | 用时/二刷 |
| --- | --- | --- | --- |
| week 1 | Overview | 1.5h | 2h |
| week 2 | Lexical Analysis | 2h | 5h/3h |
| week 3 | parsing | 1.5h | ~10h/4h |
| Week 4 | parsing | 2.5h | ~10h |
| week 5 | typing system | 1.5h | 2h |
| Week 6-1 | self-type | 0.5h | 1h |
| week 6-2 | Runtime | 1h | 1h |
| week 7-1 | code gen | 1.5h | 3h |
| week 7-2 | semantics | 1h | 跳过 |
| week 8 | optimization | 2h | 4h |
| week 9-1 | register allocation | 40min | 1h |
| week 9-2 | 引用计数的垃圾回收 | 1h | 跳过 |
| Week 10 | Java | 1.5h | 跳过 |
| 总计 | - | 18h | 46h |

## 要点总结

Compiler 的 5 个阶段：

| 阶段 | 功能 | 要点 |
| --- | --- | --- |
| Lexical analysis | 1. 源码切成 `tokens`。 | 1. 正则，有限状态机（NFA，DFA，table based 实现） |
| Parsing | 1. 构造 parse tree | 1. context-free grammar。特别是 terminal, non-terminal & productions |
| Semantic analysis | 基于 type system 的 error check | 1. Type 就是 value + set of operations。 |
| Optimization | 代码的自动优化 | 1. 关键概念：IR，basic block，control-flow graph |
| codegen | 代码的自动生成，包括汇编和 IR | 1. 1-register Stack machine 的设计和运行机制 |

其他一些 highlights：

1. 当 Software cost > hardware cost 时，大家开始做 compiler。

1. 演进老的语言，成本一般高于设计新语言。因为，programmer training 是主要成本。

1. 我们平时写的正则，可以理解为，针对试图解析的内容设计了一个 DSL。正则就是 DSL 的 grammar。

1. 正则，依赖有限状态机。不能描述无限或动态状态数的情况。比如，无限递归，训练次数不定的 loop。

1. CFG 对应 2 个概念：context free grammar，control-flow graph。根据场景区分。

1. Parsing 部分，有点枯燥 & 困难。坚持理解一下，还是比较开脑的。

1. Optimization 部分的 dataflow analysis，讲的不错。特别是，Deep learning 模型大多建模为 dataflow 形式。

## 下一步

问问 `chatGPT`

`You are an expert in Compilers and Deep Learning Frameworks. Your task is to guide me to become a technical expert in compilers, especially compilers for deep learning.`

`I have finished the detailed courses on compilers, deep learning frameworks and deep learning models. In order to go further, what should I do next? Please give me some plans with time schedule to choose.`

`I want to do some coding or read the source code of a famous compiler. does it sound good? do you have any better suggestions.`

`Congratulations on finishing the detailed courses on compilers, deep learning frameworks, and deep learning models! To become a technical expert in compilers for deep learning, there are several steps you can take:`

1. Get hands-on experience: Start by experimenting with different deep learning frameworks and compilers, and try building some basic models using these tools. You can try out `TensorFlow`, `PyTorch`, `MXNet`, and other popular frameworks, and compare their performance using different compilers. This will help you gain practical knowledge and understand how compilers translate high-level code to low-level instructions.

1. Focus on optimization techniques: As compilers for deep learning are all about optimizing the code for better performance, it's crucial to learn about different optimization techniques. Some of the common techniques include loop optimizations, memory optimizations, and parallelization techniques. Explore these techniques thoroughly, and try implementing them in your own models.

1. Stay updated with the latest research: Deep learning is a rapidly evolving field, and new techniques and algorithms are being developed all the time. Keep yourself updated with the latest research papers and publications from top-tier conferences such as NeurIPS, ICML, and ICLR. This will help you stay ahead of the curve and develop advanced models with optimal performance.

1. Join online communities: Join online communities such as Reddit, Stack Overflow, and GitHub to stay connected with other experts in the field. These communities offer a platform for you to ask questions, share your knowledge, and get feedback on your work. You can also contribute to open source projects related to compilers and deep learning frameworks to gain more experience and exposure.

Here are some plans with time schedules to help you get started:

Month 1-2: Experiment with different deep learning frameworks and compilers, and build some basic models using these tools.

Month 3-4: Focus on optimization techniques and implement them in your own models.

Month 5-6: Read research papers and publications from top-tier conferences to stay updated with the latest developments in the field.

Month 7-8: Join online communities and start contributing to open source projects related to compilers and deep learning frameworks.

By following these steps, you'll be well on your way to becoming a technical expert in compilers for deep learning.

If you're looking for a good compiler to read the source code of, there are several great options depending on your interests and goals:

1. `GCC`: This is one of the most widely used open-source compilers and is used for many languages such as C, C++, and Objective-C. `GCC` uses a combination of parsing techniques, including `LALR(1)` parser generators like `Bison` and a number of optimization passes, making it a great choice if you want to learn more about code optimization.

1. `LLVM`: `LLVM` is another popular open-source compiler infrastructure that is used in many modern compilers and programming languages. Its modular architecture makes it an excellent choice if you want to understand how a modern compiler pipeline works, with its various phases like parsing, optimization, and code generation.

1. `Clang`: `Clang` is a front-end for C, C++, and Objective-C languages that is built on top of `LLVM`. `Clang` has a reputation for being easy to read and understand, making it a great option if you're new to reading compiler source code.

1. `Rust`: `Rust` is a newer systems language that prioritizes memory safety and performance. The `Rust` compiler uses a custom syntax highlighted `LL(1)` parser, so it might be a great choice if you want to understand how a modern compiler implements its own grammar.

Overall, reading the source code of a famous compiler can be incredibly valuable for improving your understanding of compilers and seeing how theory is put into practice. Just make sure to start with small parts of the codebase that you can easily understand and build up over time!
