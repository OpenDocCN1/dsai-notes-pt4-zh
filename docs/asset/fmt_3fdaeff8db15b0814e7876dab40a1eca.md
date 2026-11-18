
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
