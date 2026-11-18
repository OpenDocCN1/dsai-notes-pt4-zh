
# CMake & c++ 编译体系

> 来源：[https://learning.feishu.cn/docx/KaL4dEwCsob3kBxhhDVc6QpEn5b](https://learning.feishu.cn/docx/KaL4dEwCsob3kBxhhDVc6QpEn5b)

## Summary

1. 入口文件 `CMakeLists.txt`。是一个 script，从上到下依次执行。
2. 是 build system generator，不是 build system。生成的东西，可以给 `make`, `Ninja` 等用。

CMake 入门材料，预计需要 1 天时间。

+ `CMake` - the essential package https://www.youtube.com/watch?v=UH6F6ypdYbw

+ 重点是 step 1-4 https://cmake.org/cmake/help/latest/guide/tutorial/index.html

## CMake vs Bazel

[`Bazel`](https://bazel.build/) is a multi-language build system focused on reproducible builds to enable dependency analysis and caching for fast incremental builds.

CMake v.s. `Bazel`：

1. 在 `llvm` 和 `mlir` 生态里，尽量还是 `cmake`。遵循社区主流。
2. 其他情况，`bazel` 大概率更好用。

`llvm-project` 对 `Bazel` 的支持不太稳定。

Reviewers should not ask authors to update `Bazel` build files unless the author has opted in to support `Bazel`. Keeping the `Bazel` build files up-to-date is on the people who use the `Bazel` build.

## CMake

### Example

一共 3 个文件：

+ `CMakeLists.txt`
+ `run.sh`
+ `test.cpp`

`CMakeLists.txt` 的简单例子

```
cmake_minimum_required(VERSION 3.20.0)

project(CmakeTutorial-Jack)

# add_executable(${PROJECT_NAME} test.cpp)
add_executable(b.out test.cpp)

# install(TARGETS b.out DESTINATION bin)
```

运行脚本 `run.sh`

```
mkdir -p build && cd build

# Generating Build files
# -S source dir
# -B build dir
cmake -S .. -B .
# Building CXX object and Linking
make -j8  # Run make with 8 threads
# run executable target
./b.out
```

`test.cpp` for demo

```
#include <iostream>
#include <vector>

class MyClass {
 public:
  int x_;
  int y_;

  MyClass(int x, int y) : x_(x), y_(y) {
    std::cout << "Create class" << std::endl;
  }

  ~MyClass() { std::cout << "Destroy class" << std::endl; }

  // Copy Constructor
  MyClass(const MyClass& my_class) {
    std::cout << "Copy Constructor Called" << std::endl;
    x_ = my_class.x_;
  }

  // Move Constructor
  MyClass(MyClass&& my_class) {
    std::cout << "Move Constructor Called" << std::endl;
    x_ = std::move(my_class.x_);
  }
};

int main() {
  std::vector<MyClass> vec;
  // Reserve space to avoid reallocation
  vec.reserve(2);

  std::cout << "\n--- push_back ---" << std::endl;
  vec.push_back(MyClass(1, 2));

  std::cout << "\n--- emplace_back ---" << std::endl;
  vec.emplace_back(1, 2);

  std::cout << "\n--- Finish ---" << std::endl;
  return 0;
}
```

流程图

![](img/3e6a30be5511074629bb25df8356a673.png)

![](img/b0f2138a0cf0d92f678d49a81422f911.png)

### Example

![](img/cdc7be2f9b6c8077c04f877802df8714.png)

build 目录下有生成的 `Makefile`, `CMakeCache` 和其他中间结果的文件夹。

![](img/8acfc873fca38f6ba7ae63d51e75ae38.png)

`-D` 覆盖 cmake cache 的内容。

![](img/d984f4eb46edc34ff0789a45f7c57c89.png)

`FORCE` 参数，禁止 `-D` 修改。

![](img/cf477cdffbf70b71886e56b9ad0ccde2.png)

### Lib

![](img/e4e7f21600789872d4863ee04fa39cea.png)

![](img/01f219b6ba03eae5533a589a32dfb3c1.png)

![](img/965b0084cfc9e128b8ab5103eba8bc68.png)

![](img/ab06c0fb6f6bd896810ff5cd17673229.png)

### Typical Project Structure

![](img/21084822ddc0e10a7823b4c3fe7a9ee3.png)

### Another Example

![](img/e6720adc748587a3e2997aa1884d78f6.png)

![](img/ca99659a39da450541c85d99a2f30ceb.png)

![](img/b1667e335768605d8a1cf06eef7529dd.png)

## Ninja

PyTorch 用 `ninja` 作为构建系统的一部分。

设计哲学：避免任何模糊的内容。

不是给人写的，只是用于作为其它程序生成的目标。

比如：

`makefile` 里面可能经常会用 `dir/*.cpp` 来表示源文件，但是这需要查询文件系统才能得到具体内容，是一个很慢而且很不确定的操作。`ninja` 把这些模糊的操作都交给元构建系统（meta-build system，例如 `cmake` ），只留下真正需要编译的命令。

`ninja` 的构建配置文件一般叫 `build.ninja`。

## CMake tutorial

https://cmake.org/cmake/help/latest/guide/tutorial/index.html

### Tutorial 01

```
-- The C compiler identification is AppleClang 14.0.3.14030022
-- The CXX compiler identification is AppleClang 14.0.3.14030022
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compiler ABI info - done
-- Configuring done (1.4s)
-- Generating done (0.0s)
-- Build files have been written to: /xxxxx/cmake-tutorial-01/out/build
```

### Tutorial 02

```
# add in CMakeLists.txt
install(TARGETS b.out DESTINATION bin)

# run in build dir after cmake to install it
sudo make install
```

![](img/600c6468b928cea49a26ace81ee0feba.png)
