
接着可以进行基础扩展测试：将训练步数从2逐步增加到5，分别进行基准测试。与矩阵乘法不同，当增加MLP的前向传播和反向传播次数时，运行时间应该呈线性增长，实际数据也印证了这一点，每次MLP执行约5秒，总体运行时间基本符合`n×5秒`的规律。同样地，当网络层数从2、3、4增加到5层时，运行时间也随之递增。这次仍然呈现线性增长趋势：单层运行约5秒（略少于5秒），总体耗时约为层数的四倍。再次验证了线性缩放规律。

这完全符合预期，无论是训练步数还是网络层数，都与运行时间存在线性关系。关于批处理规模的测试就此跳过，因为当前追踪的数据量已经有些过于庞杂。那么基准测试这部分就到此为止。

我们创建这个基准测试的函数，它进行少量预热操作，执行CUDA同步，并且能够测量我们所需的任何代码的运行时间。我们可以测量新架构的运行耗时。但是基准测试是个粗粒度的测试工具，它能告诉你代码运行缓慢，但无法指出时间具体消耗在何处。因此我们更倾向于使用性能分析这一个更加细粒度的测试。

## 7.3 性能分析（Profiling）工具

分析函数的结构大致如下，同样进行预热，执行TorchCUDA同步，然后调用分析器同时追踪CPU和GPU时间。接着运行目标代码，再次同步后输出所有时间的平均值表格。

```
def profile(description: str, run: Callable, num_warmups: int = 1, with_stack: bool = False):# 预热for _ in range(num_warmups):
        run()if torch.cuda.is_available():
        torch.cuda.synchronize()  # 等待CUDA线程结束# 使用性能分析器运行代码with torch.profiler.profile(
            activities=[ProfilerActivity.CPU, ProfilerActivity.CUDA],# 输出堆栈跟踪以进行可视化
            with_stack=with_stack,#  需要导出堆栈跟踪以进行可视化
            experimental_config=torch._C._profiler._ExperimentalConfig(verbose=True)) as prof:
        run()if torch.cuda.is_available():
            torch.cuda.synchronize()  # 等待CUDA线程结束# 打印表格
    table = prof.key_averages().table(sort_by=&quot;cuda_time_total&quot;,
                                      max_name_column_width=80,
                                      row_limit=10)#text(f&quot;## {description}&quot;)#text(table, verbatim=True)# Write stack trace visualizationif with_stack:
        text_path = f&quot;var/stacks_{description}.txt&quot;
        svg_path = f&quot;var/stacks_{description}.svg&quot;
        prof.export_stacks(text_path, &quot;self_cuda_time_total&quot;)return tableCopy to clipboardErrorCopied
```

性能分析将是[我](https://pytorch.org/tutorials/recipes/recipes/profiler_recipe.html)们需要进行的更精细化的操作。性能分析它不仅能查明时间消耗在哪个函数，我们可以查看调用栈，通常我们接触的是PyTorch接口层，即直接调用的PyTorch组件，但在PyTorch底层存在着完整的CUDA调用体系。运行分析器时可以一直追踪到底层调用，看清实际执行的代码路径。这样能更直观地理解程序在硬件上的真实执行过程。PyTorch内置了非常便捷的分析器工具。这让我们不离开Python或者PyTorch环境就能获得清晰的输出结果。PyTorch 有一个很棒的内置分析器 https://pytorch.org/tutorials/recipes/recipes/profiler_recipe.html

### 7.3.1 sleep函数的性能分析

使用这个`sleep`函数示例，就是那个休眠函数。

```
def profiling():
    sleep_function = lambda : time.sleep(50 / 1000)
    sleep_profile = profile("sleep", sleep_function) Copy to clipboardErrorCopied
```

![](img/ab4b01c3d9042595553a977873f4865e.png)

观察运行结果会发现100%的时间都消耗在名为`cudaDeviceSynchronize`的操作上，因为实际上没有GPU计算任务。我们现在就像是在分析空操作。

### 7.3.2 矩阵加法的性能分析

现在让我们看下面这个有实际意义的例子。这是个矩阵加法基础运算，先定义了接收参数`A`和`B`的加法函数执行矩阵相加。这个辅助函数会实例化两个随机高斯分布矩阵，然后调用操作参数中的内容。将两个2048维矩阵相加。

```
def run_operation2(dim: int, operation: Callable) -&gt; Callable:# Setup: create two random dim x dim matrices
    x = torch.randn(dim, dim, device=get_device())
    y = torch.randn(dim, dim, device=get_device())# Return a function to perform the operationreturn lambda : operation(x, y)def profiling():
    add_function = lambda a, b: a + b
    add_profile = profile("add", run_operation2(dim=2048, operation=add_function))Copy to clipboardErrorCopied
```

![](img/a97e098ac929b699178e1682afba67c3.png)

现在开始分析并调用分析器，会得到类似上面这个图片中的输出结果。这就是分析器返回的内容。

当我们在Python中调用加法函数时，我们显式接触的只有这个`add`函数，就是`A`加`B`。但底层发生的远不止这些。这个操作会被分派到GPU执行：首先经过`aten:add`（PyTorch的C++接口层,在表格中第二行字），这个封装器被调用后确认执行加法运算，这是最外层的调用封装。接着会分派到具体的内核函数`vectorized_elementwise_kernel4`（在表格中第三行），在 nativeCUDA 中执行向量加法等等，这才是实际执行加法的部分。同时还有CUDA启动内核操作也耗费时间，CUDA启动内核实际上是CPU执行指令并发送给GPU的过程，即内核启动耗时。最后CUDA设备同步需要等待GPU完成计算并传回数据，这个阶段同样耗时。仅仅同步这个操作本身就会消耗一定时间。最终我们得到的总时间是：CPU上1.4毫秒，CUDA上17微秒。

可见GPU运行极快，而CPU较慢。观察CPU耗时（即自占CPU时间），发现C++接口或C接口（比如`aten:add`）实际消耗了大量CPU时间，这些是向GPU传输数据时产生的开销。这就是加法函数的内部运行机制。

### 7.3.3 矩阵乘法

```
def run_operation2(dim: int, operation: Callable) -&gt; Callable:# Setup: create two random dim x dim matrices
    x = torch.randn(dim, dim, device=get_device())
    y = torch.randn(dim, dim, device=get_device())# Return a function to perform the operationreturn lambda : operation(x, y)def profiling():
    matmul_function = lambda a, b: a @ b
    matmul_profile = profile("matmul", run_operation2(dim=2048, operation=matmul_function))Copy to clipboardErrorCopied
```

矩阵乘法也是类似情况。这里我对`A`乘以`B`进行矩阵乘法运算，再次使用2048维矩阵并进行性能分析。此时看到`aten:matmul`调用，这说明底层接口执行矩阵乘法的过程。然后就会调用 Cutlass ,这是英伟达的高性能矩阵乘法CUDA库，随后分派到特定的Cutlass内核，其中包含分块尺寸参数。这实际上指向特定的分块（瓦片）尺寸和线程块数量等参数化配置，正是这些在执行矩阵乘法。同样在底部看到两个熟悉项：内核启动（`cuLaunchKernel`）和CUDA设备同步（`cudaDeviceSynchronize`）。可以再次观察到CPU时间与CUDA时间的分配情况。由于矩阵乘法比向量加法更耗时，CUDA部分占用时间显著增加。

![](img/f852fdefebd33b817a9473051c710956.png)

这里还有一个矩阵乘法示例

```
def run_operation2(dim: int, operation: Callable) -&gt; Callable:# Setup: create two random dim x dim matrices
    x = torch.randn(dim, dim, device=get_device())
    y = torch.randn(dim, dim, device=get_device())# Return a function to perform the operationreturn lambda : operation(x, y)def profiling():
    matmul_function_128 = lambda a, b: a @ b
    matmul_profile_128 = profile("matmul(dim=128)", run_operation2(dim=128, operation=matmul_function_128))   Copy to clipboardErrorCopied
```

在这里乘以128维矩阵。128乘以128，比上面这个小得多。

![](img/5ca39d3165cf9e99e690d103a98b696b.png)

你会看到现在它实际上直接执行这个不同的命令，从`sm80_xmma_gemm_f32f32_f32f32_f32_nn_n_tilesize32x32x8_stage3_warpsize1x2x1_ff`这一行可以看到和上面的矩阵乘法的不同，它执行的是`xmma_gemm`。GEMM是一种矩阵乘法类型。后面跟着`f32`，即`float32`。从该内核的命名可以看出实际发生的情况，即这是一种瓦片（分块）的矩阵乘法。它没有经过 Cutlass ，而是直接执行这个特定命令。

对于小矩阵乘法，我们会看到它现在分派到不同的内核。由此可见矩阵乘法的复杂性。当我们在这种高度抽象层面操作时，我们只把矩阵乘法视为单一事物，比如我们调用`A`乘以`B`就完成了。但在底层程序会根据你拥有的维度和硬件，它实际上会分派到完全不同的矩阵乘法原语。这会表现为截然不同的性能特征。一个有趣的技巧是`Torch.compile`（我们后面会讲到） ，它实际上有一个选项可以在你的硬件上对矩阵乘法性能进行宏观基准测试，然后它会为你的模型选择性能最高的矩阵乘法子程序，过去我发现这能免费带来10%的速度提升。优化这些内容能在现实中带来免费增益。

性能分析器相比原始基准测试的高级之处在于，我们现在可以看到正在调用哪些CUDA内核。我们可以看到不同大小的矩阵会导致不同的CUDA内核。我们看到 `cutlass_80simtt_sgemm` ，来自Cutlass线性代数库，它告诉我们分块大小等信息。

到目前为止，这些操作在某种程度上非常简单。比如矩阵乘法和加法。它们基本是一一对应的：CPU端有一个操作，它就转换为GPU操作直接传输过去。我们可以做一些更加复杂的操作。

### 7.3.4 `torch.cdist`计算距离的复杂操作

```
def run_operation2(dim: int, operation: Callable) -&gt; Callable:# Setup: create two random dim x dim matrices
    x = torch.randn(dim, dim, device=get_device())
    y = torch.randn(dim, dim, device=get_device())# Return a function to perform the operationreturn lambda : operation(x, y)def profiling():
    cdist_function = lambda a, b: torch.cdist(a, b)
    cdist_profile = profile("cdist", run_operation2(dim=2048, operation=cdist_function))Copy to clipboardErrorCopied
```

这个叫`torch.cdist`的操作，它的作用是计算两组矩阵之间向量的成对欧几里得距离。这将是我需要的`A`和`B`之间的大型距离矩阵计算，这就是`cdist`。这是一个更复杂的操作，要计算欧几里得距离需要计算点积，还需要计算平方根。

![](img/b19eb9d3d0b32ecf935042b302525b09.png)

当我们计算`cdist`时就会看到上面这张图片，这是`cdist`的分析输出。我们看到这个Torch Python 命令在C接口中的map映射到一些较低级的`cdist`。第一行的`aten::cdist`，然后映射到`aten::euclidean_dist`。接着这会分解成一整套操作：比如`aten::matmul`、`aten::pow`等等，因为这些是计算所有向量间欧几里得距离所需的基本原语。

当执行每个矩阵乘法、连接和乘幂操作时，都有一个对应的CUDA命令在这里被调用。我们有熟悉的GEMM，这是矩阵乘法，占用了GPU 78%的计算时间。还有数组的复制和连接，占执行时间的6%。然后这个向量化逐元素内核（进行乘幂运算）占GPU时间的5%，3%用于求和。现在我们得到了GPU时间分配的详细细分。

由此我们可以知道应该把优化时间花在哪里，比如我们可以优化矩阵乘法，因为它占GPU70%以上的时间。

### 7.3.5 `gelu` 和 `softmax`

```
def run_operation2(dim: int, operation: Callable) -&gt; Callable:# Setup: create two random dim x dim matrices
    x = torch.randn(dim, dim, device=get_device())
    y = torch.randn(dim, dim, device=get_device())# Return a function to perform the operationreturn lambda : operation(x, y)def profiling():
    gelu_function = lambda a, b: torch.nn.functional.gelu(a + b)
    gelu_profile = profile("gelu", run_operation2(dim=2048, operation=gelu_function))Copy to clipboardErrorCopied
```

`gelu`是一个非线性激活函数。如果大家还记得我们第一章的内容，就会知道它是高斯误差线性单元。它由`tanh`和指数函数的乘积构成。我们将进行各种运算操作：首先执行`A`与`B`的加法运算，然后调用`GELU`函数，模拟我们在多层感知机中可能存在的线性加非线性结构。

![](img/a0cbb7fe65e8cc049bef952937816de7.png)

我们再次观察到基本相同的运算映射，比如`aten::add` 对应`A+B`运算，接着看到其CUDA等效实现，最后这里还有一个完全用CUDA实现的`GELU`函数，这部分消耗了约33%的计算资源，比例很合理。
