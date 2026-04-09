# LLVM tutorial - Kaleidoscope

> 来源：[https://learning.feishu.cn/docx/XEFBd0Udpo7luvxyJE4clMYFnCb](https://learning.feishu.cn/docx/XEFBd0Udpo7luvxyJE4clMYFnCb)

| link | https://llvm.org/docs/tutorial/MyFirstLanguageFrontend/index.html |
| --- | --- |
| Requirements | This tutorial assumes you know C++, but no previous compiler experience is necessary. |
| Notes | 一共 10 课，最终会写 a bit less than 1000 lines of code |

# Kaleidoscope Introduction

类型系统：只有 double （fp64）。因此，不需要 type declaration。

可以调用 standard library functions，LLVM JIT makes this really easy。用 extern 声明。

procedural language that allows you to define functions, use conditionals, math, etc.

extend Kaleidoscope to support the if/then/else construct, a for loop, user defined operators, JIT compilation with a simple command line interface, debug info, etc.

计算 Fibonacci numbers 的例子：

```
# Compute the x&#x27;th fibonacci number.
def fib(x)
  if x &lt; 3 then
    1
  else
    fib(x-1)+fib(x-2)

# This expression will compute the 40th number.
fib(40)
```

# Chap 1 & 2 - 实现 Lexer + Parser

1.  [CS143 编译原理笔记 2 - Parsing](https://dwpl6xgouw.feishu.cn/wiki/AbuHwUnb6ixRBAkvYBcc8qocnIc)果懂编译原理，直接看 code 比较省事。编译原理：

1.  与 LLVM 没什么关系。

Lexer

+   Input: text file

+   Output: tokens and potentially some metadata (e.g. the numeric value of a number).

Parser:

+   Input: tokens from lexer

+   Output: AST

uses a combination of Recursive Descent Parsing and Operator-Precedence Parsing

Code: https://llvm.org/docs/tutorial/MyFirstLanguageFrontend/LangImpl02.html#full-code-listing

收获，TVM 的 Expr 也是这个写法。

```
/// ExprAST - Base class for all expression nodes.
class ExprAST {
 public:
  virtual ~ExprAST() = default;
};

/// NumberExprAST - Expression class for numeric literals like &quot;1.0&quot;.
class NumberExprAST : public ExprAST {
  double Val;

 public:
  NumberExprAST(double Val) : Val(Val) {}
};

/// VariableExprAST - Expression class for referencing a variable, like &quot;a&quot;.
class VariableExprAST : public ExprAST {
  std::string Name;

 public:
  VariableExprAST(const std::string &amp;Name) : Name(Name) {}
};
```

# Chap 3 - codegen to LLVM IR

require LLVM 3.7 or later

setup：给每个 ExprAST 增加 virtual 的 codegen method。

return an LLVM Value object. “Value” is the class used to represent a “Static Single Assignment (SSA) register” or “SSA value” in LLVM.

实践上，应该用 visitor pattern。

TVM 的 IRModule 应该就来自于 IRBuilder 和 Module 的影响。

```
static std::unique_ptr&lt;LLVMContext&gt; TheContext;
static std::unique_ptr&lt;IRBuilder&lt;&gt;&gt; Builder(TheContext);
static std::unique_ptr&lt;Module&gt; TheModule;
static std::map&lt;std::string, Value *&gt; NamedValues;
```

The static variables will be used during code generation. TheContext is an opaque object that owns a lot of core LLVM data structures, such as the type and constant value tables. We don’t need to understand it in detail, we just need a single instance to pass into APIs that require it.

The Builder obje[ct is a h](https://llvm.org/doxygen/IRBuilder_8h_source.html)elper object that makes it easy to generate LLVM instructions. Instances of the IRBuilder class template keep track of the current place to insert instructions and has methods to create new instructions.

TheModule is an LLVM construct that contains functions and global variables. In many ways, it is the top-level structure that the LLVM IR uses to contain code. It will own the memory for all of the IR that we generate, which is why the codegen() method returns a raw Value*, rather than a unique_ptr<Value>.

The NamedValues map keeps track of which values are defined in the current scope and what their LLVM representation is. (In other words, it is a symbol table for the code). In this form of Kaleidoscope, the only things that can be referenced are function parameters. As such, function parameters will be in this map when generating code for their function body.

With these basics in place, we can start talking about how to generate code for each expression. Note that this assumes that the Builder has been set up to generate code into something. For now, we’ll assume that this has already been done, and we’ll just use it to emit code.

Generating LLVM code for expression nodes is very straightforward: less than 45 lines of commented code for all four of our expression nodes.

+   NumberExprAST

+   numeric constants are represented with the ConstantFP class, which holds the numeric value in an APFloat internally

+   APFloat has the capability of holding floating point constants of Arbitrary Precision

+   VariableExprAST

+   BinaryExprAST

+   FunctionAST，依赖 PrototypeAST 和 CallExprAST

In future chapters, we[’ll add support](https://llvm.org/docs/tutorial/MyFirstLanguageFrontend/LangImpl07.html#user-defined-local-variables) for loop induction variables in the symbol table, and for local variables.

Binary operators。

LLVM builder class is starting to show its value

IRBuilder knows where to insert the newly created instruction, all you have to do is specify what instruction to create (e.g. with CreateFAdd), which operands to use (L and R here) and optionally provide a name for the generated instruction.

Local value names for instructions are purely optional, but it makes it much easier to read the IR dumps.

以 number 为例的 llvm codegen，在 mlir 里，也大量的这种代码。看不懂 ConstantFP::get 和 TheContext 参数是在干什么。

```
Value *NumberExprAST::codegen() {
  return ConstantFP::get(*TheContext, APFloat(Val));
}
```

in the LLVM IR that constants are all uniqued together and shared. For this reason, the API uses the “foo::get(…)” idiom instead of “new foo(..)” or “foo::Create(..)”.

# Chap 4 - JIT and Optimizer

all calls to build LLVM IR go through the LLVM IR builder, the builder itself checked to see if there was a constant folding opportunity when you call it

(1+2+x)*(x+(1+2))

local analysis 搞不定。

requires two transformations: reassociation of expressions (to make the add’s lexically identical) and Common Subexpression Elimination (CSE) to delete the redundant add instruction.

支持 2 种 pass：

1.  “whole module” passes。look across as large of body of code as they can，经常是一个 file。如果是 link 时，就是 whole program 了。

1.  “per-function” passes。

FunctionPassManager hold and organize the LLVM optimizations

初始化以后，add 一堆 optimizations 进去。