
# LLVM IR 语法 & LLVM OPs

> 来源：[https://learning.feishu.cn/docx/NlOUd0iHQoqmRXxbkqEcPp4yn0e](https://learning.feishu.cn/docx/NlOUd0iHQoqmRXxbkqEcPp4yn0e)

## LLVM IR 简介

LLVM IR 有 3 种等价的形式

+   in-memory compiler IR。

+   on-disk bitcode representation。扩展名 `*.bc`。紧凑的二进制格式

+   assembly form。扩展名 `*.ll`。人类可读的文本格式

LLVM container 对象，4 层嵌套：

+   `Module`: 顶层容器。包含 全局变量、`TargetTriple`、`metadata` 等。

+   `Function`：函数。

+   `BasicBlock`: 很关键，简称 `bb` 块。单入口、单出口的指令序列，控制流图的基本节点。

+   `Instruction`: 单条 `LLVM IR` 指令。包括算术、内存、控制流等操作。

核心基类：`Value`。

+   除了`Module`，其他核心对象都继承自 `Value`

+   建立了def-use链，是LLVM优化和分析的基石

+   `Function`/`BasicBlock`/`Instruction`/`GlobalVariable`都是`Value`

`LLVM container` 对象包含的详细内容，如下图

![](img/fa124c7b2cea6f334a72c5f6d94a1fba.png)

`module` 中的 target information，语法如下

![](img/c2ac8333e9137671c62391a511382367.png)

## LLVM IR 构建实践

### 顶层容器 `LLVM module`

`LLVM module` 是 top-level 的 container，包含：

1.  global variables

1.  functions

1.  data layout。大端法、小端法等。不同于 AI compiler 中 data layout 的概念。

1.  host triples

1.  and so on

与 LLVM essentials 书的 LLVM API 版本，有明显区别了。

以前是 `ModuleOb->dump()`，现在是 `module.print`

```
#include "llvm/IR/LLVMContext.h"
#include "llvm/IR/Module.h"

int main(int argc, char *argv[]) {
    llvm::LLVMContext context;
    llvm::Module module("my_module", context);

    // Output the module to stdout
    module.print(llvm::outs(), nullptr);
    return 0;
}
```

编译 & 运行

```
clang++ -O3 mymodule.cpp `llvm-config --cxxflags --ldflags --system-libs --libs core` -o a.out

./a.out
```

output：

```
; ModuleID = 'my_module'
source_filename = "my_module"
```

### 指令插入点管理：`llvm::IRBuilder`

新指令的插入点管理

要点：

1.  `IRBuilder` 是 `LLVM` 中用于生成 IR 指令的主要工具类。

1.  它维护一个插入点（Insertion Point），指令会被插入到这个点所在的基本块（`BasicBlock`）中。

1.  插入点可以设置到不同的基本块，也可以清除（变为未绑定状态）。

1.  生成指令时，`IRBuilder`必须绑定到一个基本块（即插入点有效）。

1.  控制流语句（如分支、循环）需要创建新的基本块，并通过`IRBuilder`设置插入点来填充指令。

1.  基本块必须以终止指令（如`br`, `ret`）结束，之后不应再向该基本块添加指令（应清除插入点）。

意义：

1.  提供了一种流式、直观的方式构建`LLVM IR`。

1.  隐藏了`IR`指令创建的复杂性，提供了高级API（如`CreateAdd`、`CreateLoad`等）。

1.  通过插入点机制，允许灵活地控制指令在基本块中的位置。

1.  是`LLVM`前端实现中不可或缺的部分，将抽象语法树（`AST`）转换为`IR`的核心工具。

常用例子：

1.  创建算术指令：加法、减法等。

1.  创建内存指令：加载、存储。

1.  创建控制流指令：分支、返回。

1.  创建函数调用。

appends instructions at an insertion point into a basic block

![](img/012c30effd3d8da0235157c375d798fa.png)

### Container & 核心基类：`Value`

## Building LLVM IR - scalar add

### Emitting a function in a module

`LLVM IR` 的函数，有 `declare` 和 `define` 2 个关键词。调用的时候用 `call`，函数内用 `ret` 返回。

使用 `llvm::IRBuilder`

emitting function 用 `llvm::FunctionType` 和 `llvm::Function::Create`

new code

```
llvm::IRBuilder<> builder(context);

// Create a function signature: int add(int, int)
llvm::FunctionType *funcType = llvm::FunctionType::get(builder.getInt32Ty(), 
    {builder.getInt32Ty(), builder.getInt32Ty()}, false);
// Create the function
llvm::Function *addFunction = llvm::Function::Create(funcType, 
    llvm::Function::ExternalLinkage, "add", module);
```

output：

```
; ModuleID = 'my_module'
source_filename = "my_module"

declare i32 @add(i32, i32)
```

### Adding a block to a function

使用 `llvm::BasicBlock::Create`

new code

```
// Create a basic block to hold the function body
llvm::BasicBlock *bb = llvm::BasicBlock::Create(context, "entry", addFunction);
builder.SetInsertPoint(bb);
```

output

```
; ModuleID = 'my_module'
source_filename = "my_module"

define i32 @add(i32 %0, i32 %1) {
entry:
}
```

### Create an instruction

获取 operand 变量以后，使用 `builder.CreateAdd` 创建 `add` 指令。

new code

```
// Get function arguments
auto args = addFunction->arg_begin();
llvm::Value *a = args++;
llvm::Value *b = args;

// Create the addition instruction
llvm::Value *sum = builder.CreateAdd(a, b, "sum");
```

output

```
; ModuleID = 'my_module'
source_filename = "my_module"

define i32 @add(i32 %0, i32 %1) {
entry:
  %sum = add i32 %0, %1
}
```

### Emitting a global variable

使用 `llvm::GlobalVariable`

要设置正确的 linkage type, alignment 等。

全局变量以 `@` 开头，局部变量以 `%` 开头。

1.  Are always pointers。只要是 pointer，就要 explicit 使用 `load`/`store` 指令读写 pointer 指向的数据。

1.  Always constant pointers。address 是 constant，value 是可变的。

![](img/eb7bd76667b62bb78dc93d59e30eae2f.png)

Local variables 有 2 种

1.  register allocated。临时变量，`%1 = some value` 语句创建的。

1.  stack allocated。由 `alloca` 指令创建。`%2 = alloca i32`

As global variables are part of a module, we create global variables after creating the module

new code

```
// Create a global variable: int globalVar = 42
llvm::Type *int32Type = llvm::Type::getInt32Ty(context);
llvm::GlobalVariable *globalVar = new llvm::GlobalVariable(
    module,                           // Module
    int32Type,                       // Type
    /*isConstant=*/false,           // isConstant
    llvm::GlobalValue::ExternalLinkage, // Linkage
    llvm::ConstantInt::get(int32Type, 42), // Initial value
    "globalVar"                     // Name
);

// xxxxxxx

// Load the global variable and use it (e.g., add it to the sum)
llvm::Value *globalValue = builder.CreateLoad(int32Type, globalVar, "globalValue");
llvm::Value *result = builder.CreateAdd(sum, globalValue, "result");
```

output

```
; ModuleID = 'my_module'
source_filename = "my_module"

@globalVar = global i32 42

define i32 @add(i32 %0, i32 %1) {
entry:
  %sum = add i32 %0, %1
  %globalValue = load i32, ptr @globalVar, align 4
  %result = add i32 %sum, %globalValue
}
```

![](img/21e4f1ce6e155ee9f71893e997b96a83.png)

### Emitting a return statement

使用 `builder.CreateRet` 创建，比较简单。

new code

```
// Return the result
builder.CreateRet(result);
```

output

```
; ModuleID = 'my_module'
source_filename = "my_module"

@globalVar = global i32 42

define i32 @add(i32 %0, i32 %1) {
entry:
  %sum = add i32 %0, %1
  %globalValue = load i32, ptr @globalVar, align 4
  %result = add i32 %sum, %globalValue
  ret i32 %result
}
```

### Emitting if-else condition IR

创建 1 个 condition, 3 个 block, 1 个 phi node。

用 `CreateCondBr` 在 `true block` 和 `false block` 上条件跳转，

2 个 block 最后都 `CreateBr` 无条件跳转到 merge block。

merge block 用 `phi node` 仲裁不同分支的变量。

`br` 是 `branch` 的缩写。

![](img/2d6ed8a583ddda31aadd383a1652f38d.png)

Incomplete IF statement，当 `cond` 为 false 时，跳到 `bb2`，另，`bb1` 结束后，也是 跳到 `bb2`。

从而实现左侧的 cfg 图。

![](img/c392ff14a400d4a1a28367f457e2850c.png)

使用 `builder.CreateICmpULT` 和 `builder.CreatePHI`。

其中，`ULT` 是 unsigned less than

```
// Create the conditional check: if (a < 100)
llvm::Value *condition = builder.CreateICmpULT(a, llvm::ConstantInt::get(int32Type, 100), "cond");
// Create blocks for true and false cases
llvm::BasicBlock *trueBlock = llvm::BasicBlock::Create(context, "true_block", addFunction);
llvm::BasicBlock *falseBlock = llvm::BasicBlock::Create(context, "false_block", addFunction);
llvm::BasicBlock *mergeBlock = llvm::BasicBlock::Create(context, "merge_block", addFunction);

// Conditional branch
builder.CreateCondBr(condition, trueBlock, falseBlock);

// True block: perform addition
builder.SetInsertPoint(trueBlock);
llvm::Value *sum = builder.CreateAdd(a, b, "sum");
builder.CreateBr(mergeBlock);  // Jump to merge block

// False block: return 0
builder.SetInsertPoint(falseBlock);
builder.CreateBr(mergeBlock);  // Jump to merge block

// Merge block
builder.SetInsertPoint(mergeBlock);
llvm::PHINode *phiNode = builder.CreatePHI(int32Type, 2, "result");
phiNode->addIncoming(sum, trueBlock);   // Incoming from true block
phiNode->addIncoming(llvm::ConstantInt::get(int32Type, 0), falseBlock);  // Incoming from false block
```

下面用 `sum` 变量的地方，换成用 `phiNode`。

```
llvm::Value *result = builder.CreateAdd(phiNode, globalValue, "result");
```

output



```
; ModuleID = &#x27;my_module&#x27;
source_filename = &quot;my_module&quot;

@globalVar = global i32 42

define i32 @add(i32 %0, i32 %1) {
entry:
  %cond = icmp ult i32 %0, 100
  br i1 %cond, label %true_block, label %false_block

true_block:                                       ; preds = %entry
  %sum = add i32 %0, %1
  br label %merge_block

false_block:                                      ; preds = %entry
  br label %merge_block

merge_block:                                      ; preds = %false_block, %true_block
  %result = phi i32 [ %sum, %true_block ], [ 0, %false_block ]
  %globalValue = load i32, ptr @globalVar, align 4
  %result1 = add i32 %result, %globalValue
  ret i32 %result1
}
```

可视化一下

```
./a.out &gt; condition-example.ll
opt -debug-pass-manager -passes=dot-cfg -disable-output condition-example.ll
dot -Tpng .add.dot -o condition-example.png
```

![](img/44f16db20dd8de858f26a9a74153737b.png)

## TODO - Emitting LLVM IR for loop

loop 的 `phi node` 和 `CFG`，比 `if-else`，更复杂一些。

loop 需要 4 个 `bb`。`if-else` 只需要 3 个 `bb`。

以 `factorial` 实现为例，右侧 `for_body` 的写法有问题。

![](img/69e2cb35b2986c01f7ab241843b13297.png)

报错信息：

![](img/674408fe74153f4b89bc0063bd848cc3.png)

![](img/45fdebfc83c80fa09c4b26f37f60d1f6.png)

![](img/2113e3b454fd07a6ca45e846add9ca18.png)

`cond` 里面带 `&&` 和 `||` 时，更复杂一些。

![](img/27c1287ddefe650623b22117887eb2ea.png)

![](img/a21b60e3712cf88df34b78cb338df828.png)

# Advanced LLVM IR

advanced 内容包括：

1.  memory operations,

1.  aggregate data types and operations on them. 最简单的 aggregate data type 是 array。

TODO:

memory operations =>  结合 `MLIR` 中的 `SideEffect` 机制理解

memory operations 分 2 步：

1.  get the location of the desired element

1.  `load` / `store` using the address

`http://llvm.org/docs/LangRef.html#masked-vector-load-and-store`

## LLVM’s type system

LLVM IR 是强类型的，有 typed information。可以构建 type system。

![](img/42307856f64a9199395b70c7e525f4b2.png)

![](img/374e352b3bd0697ae3cbd8badb1509b7.png)

![](img/3217b9824180b9d26385792fb8774131.png)

## Memory address and Load/Store

`getelementptr` 只会计算 address，不会 access the memory。

读写 address 内的 value，用 `load` / `store`。

![](img/173c4aaa5e20cff0a8b773e64fe519bc.png)

![](img/6855543a7f30bb7b356b0b64db85aad5.png)

![](img/a4eb3ec0902cd9f30e88587e2c06b063.png)

`result` 的 数据类型，与 base type 一致。是 `[6 * i8]`，而不是 `i8`

![](img/d580895749baf4a719609fb120cf8985.png)

在 `offset` 一次，就越界了。

![](img/96942ac16771ab85b0ab5434b771c048.png)

`GEP` 的第 1 种用法

Understand the first index：

1.  It does NOT change the resulting pointer type.

1.  It offsets by the base type

![](img/c212e0a1572f022a0cdb918708c59751.png)

`GEP` 的 第 2 种用法

![](img/40d115494aa5f7aef0a5d9df58f75cf3.png)

![](img/e797eb3ac0fb3a76847a2e97606f9ca5.png)

structs，

`offset = 1` 就开始越界了。只能用 `0`.

![](img/071a4be37005f77f4934818b1f6209d9.png)

用 `offset 0, 1` 拿到 `i32` 的第一个 element

![](img/a4ce8a76706dadb06401b2fea1b4da0d.png)

用 `0, 2` 拿到 `[3 * i32]`

用 `0, 2, 0` 拿到 `i32`

![](img/f97ebb934a217aa6daa1db3552d75cdd.png)

如果 struct indices 不是 `constant`，基本没法算 `offset` 了。

![](img/5e3d6d6a0e6db5628dec3bb2feacfcee.png)

理解 `GEP` 的终极测试题

![](img/eefb8492d7e0aab289f2a5464d33e92a.png)

![](img/19c9019713a08fcdbb93d721df7713c8.png)

不可以添加 extra zero。因为 `new_ptr` 的数据类型是 pointer to pointer，不是 aggregate data type。

![](img/df2b7dc9eb8f87c10a14ebf0bc35de0a.png)

![](img/17dac1dd65f81dcb5377b96b888b2d4f.png)

`getelementptr` 的缩写是 `GEP`，详细 doc: `https://llvm.org/docs/GetElementPtr.html`

3 个 operand，依次是：

1.  目标地址的 `value` 的 `type`

1.  `base pointer` & its type，允许是 `vector of pointers`

1.  `element indices` from `base pointer`。计算 `address` 时，要 `indices * type_size`

new code

```
// Define the type for an integer
llvm::Type *intType = llvm::Type::getInt32Ty(context);

// Create a pointer to an array of integers (a simple vector)
llvm::Value *size = llvm::ConstantInt::get(intType, 10); // Size of the vector
llvm::Value *vectorPtr = builder.CreateAlloca(intType-&gt;getPointerTo(), size, &quot;vector&quot;);

// Create a GEP instruction to access the first element of the vector
llvm::Value *zeroIndex = llvm::ConstantInt::get(intType, 0);
llvm::Value *gep = builder.CreateGEP(intType, vectorPtr, zeroIndex, &quot;gep&quot;);

// Store a value into the first element of the vector
llvm::Value *valueToStore = llvm::ConstantInt::get(intType, 42); // Value to store
builder.CreateStore(valueToStore, gep);

// Load the value from the first element of the vector
llvm::Value *valueLoaded = builder.CreateLoad(intType, gep, &quot;valueLoaded&quot;);

// Return the loaded value
builder.CreateRet(valueLoaded);
```

output

```
; ModuleID = &#x27;my_module&#x27;
source_filename = &quot;my_module&quot;

define i32 @loadStore(i32 %0, i32 %1) {
entry:
  %vector = alloca ptr, i32 10, align 8
  %gep = getelementptr i32, ptr %vector, i32 0
  store i32 42, ptr %gep, align 4
  %valueLoaded = load i32, ptr %gep, align 4
  ret i32 %valueLoaded
}
```

## `Alloca` instruction cheat `SSA` `CFG`

Frontend generates something different!

Gets around `SSA` restriction by writing to memory.

`https://godbolt.org/z/Nlx6T5`

(remember to untick the “hide comments” option!)

But the optimized code is similar to what we had (with `O1`):

`https://godbolt.org/z/OirW9y`

![](img/a62b3a2e3e2e2823200f249446237ac8.png)

![](img/a9b965733f3c464c326672ca53f9a29f.png)

![](img/c24dfbbdefbc90860979f7bebed29f22.png)

## TODO - Inserting & Extracting Scalars from a Vector

`vector` is different from an `array`

`Vector` types are used in `SIMD` instructions

`vector` requires 2 info:

1.  `vector` size

1.  `element` type

`insertelement` 和

The `insertelement` instruction looks like the following:

`%vec0 = insertelement <4 x double> Vec, %val0, %idx`

The first argument is a value of `vector` type

`Vec` is of `vector` type `< 4 x i32 >`

`val0` is the value to be inserted

`idx` is the index at which the value is to be inserted in the `vector`

code 测试下来，好像有 bug

# Basic IR Transformations

## opt & Pass Infra

官方 doc: `https://llvm.org/docs/WritingAnLLVMNewPMPass.html`

The New Pass Manager `https://blog.llvm.org/posts/2021-03-26-the-new-pass-manager/`

pass 编译成  `.o`, `.a` 或 `.so`，启动时被 pass manager 自动 `link` & 加载。

机器无关的 Pass，`opt` 调用。机器相关的，`llc` 调用。

most optimizations follow a simple three-part structure:

+   Look for a pattern to be transformed.

+   Verify that the transformation is safe/correct for the matched instance.

+   Do the transformation, updating the code.

pass 的类型很多，与 LLVM container 的层级，基本相对应，有

1.  `ModulePass`。the most general superclass，allow the entire module to be analyzed at once

1.  `FunctionPass`。execute on each function

1.  `BasicBlockPass`。不允许 `add`/`delete` blocks，不能改变 `CFG`。

1.  `LoopPass`。分析单个 `loop` 循环的。

![](img/6cb67bfe43e7bfec6185c79e917e8135.png)

一些 "peephole" Optimization 的 example

```
// X - 0 -&gt; X
if (match(Op1, m_Zero()))
  return Op0;

// X - X -&gt; 0
if (Op0 == Op1)
  return Constant::getNullValue(Op0-&gt;getType());

// (X*2) - X -&gt; X
if (match(Op0, m_Mul(m_Specific(Op1), m_ConstantInt&lt;2&gt;())))
  return Op1;

…

return 0;  // Nothing matched, return null to indicate no transformation.
```

以上，都是指令级别的，返回都是 `operand`，相当于做了 constant folding。

如果返回新的 `op`，可以用 `llvm::IRBuilder` 创建新的 `op` 并 `return`。

在 `basic block` 中，替换为新的 `instruction`。用 `replaceAllUsesWith()`。

```
for (BasicBlock::iterator I = BB-&gt;begin(), E = BB-&gt;end(); I != E; ++I)
  if (Value *V = SimplifyInstruction(I))
    I-&gt;replaceAllUsesWith(V);
```



# LLVM Pass 开发指南

## HelloWorld Pass 示例

```cpp
//=============================================================================
// Visits all functions in a module, prints their names and the number of arguments
//=============================================================================
#include "llvm/IR/LegacyPassManager.h"
#include "llvm/Passes/PassBuilder.h"
#include "llvm/Passes/PassPlugin.h"
#include "llvm/Support/raw_ostream.h"

using namespace llvm;

namespace {

// This method implements what the pass does
void visitor(Function &F) {
    errs() << "Func: " << F.getName() << ". arg_size: " << F.arg_size() << "\n";
}

// New PM implementation
struct HelloWorld : PassInfoMixin<HelloWorld> {
  // Main entry point, takes IR unit to run the pass on (&F) and the
  // corresponding pass manager (to be queried if need be)
  PreservedAnalyses run(Function &F, FunctionAnalysisManager &) {
    visitor(F);
    return PreservedAnalyses::all();
  }

  // Without isRequired returning true, this pass will be skipped for functions
  // decorated with the optnone LLVM attribute. Note that clang -O0 decorates
  // all functions with optnone.
  static bool isRequired() { return true; }
};
} // namespace

//-----------------------------------------------------------------------------
// New PM Registration
//-----------------------------------------------------------------------------
llvm::PassPluginLibraryInfo getHelloWorldPluginInfo() {
  return {LLVM_PLUGIN_API_VERSION, "HelloWorld", LLVM_VERSION_STRING,
          [](PassBuilder &PB) {
            PB.registerPipelineParsingCallback(
                [](StringRef Name, FunctionPassManager &FPM,
                   ArrayRef<PassBuilder::PipelineElement>) {
                  if (Name == "hello-world") {
                    FPM.addPass(HelloWorld());
                    return true;
                  }
                  return false;
                });
          }};
}

// This is the core interface for pass plugins. It guarantees that 'opt' will
// be able to recognize HelloWorld when added to the pass pipeline on the
// command line, i.e. via '-passes=hello-world'
extern "C" LLVM_ATTRIBUTE_WEAK ::llvm::PassPluginLibraryInfo
llvmGetPassPluginInfo() {
  return getHelloWorldPluginInfo();
}
```

![](img/5adc80a8dad47abab612b865d175ecc7.png)

## basic block pass

```cpp
void visitor(Function &F) {
    for (BasicBlock &BB : F) {
        // Analyze the basic block
        errs() << "Basic Block: " << BB.getName() << "\n";

        for (Instruction &I : BB) {
            errs() << "  Instruction: " << I << "\n";
        }
    }
}
```

![](img/ffed794cc8f582eefc2c4704cc36b083.png)

## TODO - Using other Pass info

特别是与 analysis 的配合机制。

pass 间 dependency 的声明方法，信息传递的机制。

## TODO - instruction simplification

## TODO - instruction combination

## Advanced IR Block Transformations

1. Loop processing
2. Scalar evolution
3. LLVM intrinsics
4. Vectorization

### Loop processing

go to: [Loop Optimizations by Compilers](https://deep-learning.feishu.cn/wiki/XDYfwxQFKiEju0kxAOhcCPTQnHh?fromScene=spaceOverview)

### TODO - Scalar evolution

### TODO - LLVM intrinsics

LLVM 添加 intrinsic 的教程：https://llvm.org/docs/ExtendingLLVM.html

### TODO - Vectorization
