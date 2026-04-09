# Hands On Triton v3.5

> 来源：[https://learning.feishu.cn/docx/InFRd3uiTovPR5x7iTYcGODznhe](https://learning.feishu.cn/docx/InFRd3uiTovPR5x7iTYcGODznhe)

Triton Code 版本：https://github.com/triton-lang/triton/tree/rc/3.3.x

[Triton 草稿纸](https://deep-learning.feishu.cn/wiki/Q6yswUOvRiPFgOkPGnqcYxeZngk?fromScene=spaceOverview)旧的笔记内容，比如，Compile Triton，在

要点提纲：

1.  从 kernel compile, kernel run & jit 机制 3 方面理解 triton 的核心。

1.  新后端接入的注意事项 & 值得深入研究的课题。

# Summary

理解 Triton Compiler，核心是理解 3 件事：

1.  kernel compile。

1.  编译输入：用@triton.jit修饰的 kernel function。一般是包含多个 cuda block 的 triton block。

1.  编译链路：Python AST → TTIR → TTGIR → LLVM IR → PTX → SASS

1.  输出：每个 kernel 一个 so (binary)

1.  kernel run。

1.  scope：主要是与 runtime/driver 对接，一般从 load_binary 开始，到 launch kernel 结束。

1.  特点：不难，但外部依赖很多，基本就是各种联调。

1.  挑战：barrier 插入 & sync 机制。（应该都是 UMD 做的，不用在 Triton 里处理）（其他？）

1.  compile & run 是如何无缝串起来的 - Triton Kernel 的 JIT 运行机制。

1.  cache 机制。

1.  auto tune 的空间 & 方法。

Code Reading 的 4 个重点：

1.  Triton Kernel 的运行机制。jit run & compile

1.  opt pass。关键 pass & hardware 适配的主要位置。

1.  Dialect 层级设计。-- why Adding this Dialect (IR)？

1.  跨模块的性能优化策略。比如，tiling。每个模块分别需要做什么，如何保证的协同安全。

一些值得深入的 topic：

1.  识别关键子系统。比如，Layout 类型系统。

1.  搞清楚每个模块内做 auto optimization 的难度、挑战 & 收益。

1.  模块之间，joint optimization 的可行性 & 挑战。

# triton 的概念澄清

Triton 中的 grid / block，不同于 cuda 的 grid/block。triton block 大小的确定方法。

program_id

tl.program_id 理解为 tile id 或 block id，更好一些。不要按照 cuda 继续理解为 thread id。

triton load / store 与 cuda 的核心差异点。

一个 triton block，一起 load。compiler 负责 coalesce。

block 内，如何分到各个 thread 上，是 Triton compiler 负责的。

mask 和 offset 机制，精确控制参与 load 的 element。功能上，不要求物理地址连续。

block pointer https://triton-lang.org/main/getting-started/tutorials/08-experimental-block-pointer.html

# Triton Kernel 的 JIT 运行机制

## Kernel 示例 - vector_add_1block

可能是最简的一个 Triton Kernel 代码示例

```
@triton.jit
def add_kernel(x_ptr, y_ptr, output_ptr,
               n_elements, BLOCK_SIZE: tl.constexpr):
    offsets = tl.arange(0, BLOCK_SIZE) 
    mask = offsets &lt; n_elements

    x = tl.load(x_ptr + offsets, mask=mask)
    y = tl.load(y_ptr + offsets, mask=mask)
    output = x + y
    tl.store(output_ptr + offsets, output, mask=mask)


def add(x: torch.Tensor, y: torch.Tensor):
    output = torch.empty_like(x, device=x.device)
    n_elements = output.numel()
    grid = (1,)
    add_kernel[grid](x, y, output, n_elements, BLOCK_SIZE=1024)
    return output
```

## Kernel 启动的语法糖

Triton Kenerl 启动的经典语法：

```
# args prepare
grid = (grid_x, grid_y, grid_z)
kernel_args = [x, y, output, n_elements]
meta_params = {&quot;BLOCK_SIZE&quot;: 1024}

# Kernel 启动的经典语法
add_kernel[grid](*kernel_args, **meta_params)

# 以上的语法糖，等价代码如下 ( [grid] =&gt; grid=grid )
wrapper = JITFunction(add_kernel)
wrapper.run(grid=grid, *kernel_args, **meta_params)
```

triton v3.3 的原始代码截图如下

![](img/a000080f83ceeb45a6c043650a7c8d80.png)

TODO

截图中有个细节，关于 warmup=False 参数。（已红色框出）

warmup 参数在做 benchmark & 生产环境 warmup 时，比较有用。

但是，JITFunction.run 的 warmup 没有默认值，导致调用时必须指定。

然后，通过基类 Kernelnterface 启动的这条路，hardcode 硬编码了 warmup=False。

个人感觉，这个 hardcode 不是一个优雅的工程实现。损失了一些功能和灵活性。

## 真实 main 函数 - JITFunction.run

从业务语义角度，JITFunction.run 是实质上的 triton main 函数，有 4 个可以展开讨论的核心功能：

1.  参数解析。校验 kernel 参数（开发者定义）与 device 固有参数的兼容性、按下游格式重新序列化。

1.  CompiledKernel Cache 机制。中了 cache 的 kernel，不会重新编译。

1.  Kernel Compile 流水线。调用了 JITFunction.compile 函数，Triton 的最核心环节。

1.  Kernel Run 机制。调用了 CompiledKernel.run 方法。本质是调 runtime 接口的一个 wrapper。主要是 3 个 API:

1.  load_binary,

1.  launch_metadata,

1.  launch_kernel.

接入一个新的 Chip Backend 时，

1.  主要是调试 2 个方法：

1.  JITFunction.compile。即 Kernel Compile。主要是对接 LLVM。

1.  CompiledKernel.run。即 Kernel Run。主要是对接 runtime & driver。

1.  Kernel Run 一般都是 async 机制，不等 device kernel 结束，host 侧代码就会继续执行，除非遇到用户显式指定的 sync 或 barrier。

1.  参数解析，琐碎又重要，虽有多次重构，配参数简单了很多，但，还是不够清晰醒目。容易错配、漏配。调 performance 时，更要小心。

1.  Cache 机制，个人感觉，无非特殊需求，一般不需要 hack。

核心代码如下

```
def run(self, *args, grid, warmup, **kwargs):
    # parse options
    device = driver.active.get_current_device()
    stream = driver.active.get_current_stream(device)

    kernel_cache, target, backend, binder = self.device_caches[device]
    bound_args, specialization, options = binder(*args, **kwargs)

    # compute cache key
    key = str(specialization) + str(options)
    kernel = kernel_cache.get(key, None)

    # Kernel is not cached; we have to compile.
    if kernel is None:
        # options
        options = backend.parse_options(kwargs)
        # signature
        sigkeys = [x.name for x in self.params]
        sigvals = [x[0] for x in specialization]
        signature = {k: v for (k, v) in zip(sigkeys, sigvals)}

        # constexprs
        constexprs = find_paths_if(sigvals, lambda _, val: val == &quot;constexpr&quot;)
        constexprs = {path: get_iterable_path(list(bound_args.values()), path) for path in constexprs}
        # attributes
        attrvals = [x[1] for x in specialization]
        attrs = find_paths_if(attrvals, lambda _, x: isinstance(x, str))
        attrs = {k: backend.parse_attr(get_iterable_path(attrvals, k)) for k in attrs}

        # compile the kernel
        src = self.ASTSource(self, signature, constexprs, attrs)
        kernel = self.compile(src, target=target, options=options.__dict__)
        kernel_cache[key] = kernel


    if not warmup:
        # canonicalize grid
        assert grid is not None
        if callable(grid):
            grid = grid(bound_args)
        grid_size = len(grid)
        grid_0 = grid[0]
        grid_1 = grid[1] if grid_size &gt; 1 else 1
        grid_2 = grid[2] if grid_size &gt; 2 else 1
        # launch kernel
        launch_metadata = kernel.launch_metadata(grid, stream, *bound_args.values())
        kernel.run(grid_0, grid_1, grid_2, stream, kernel.function, kernel.packed_metadata,
                   launch_metadata, self.CompiledKernel.launch_enter_hook, self.CompiledKernel.launch_exit_hook,
                   *bound_args.values())
    return kernel
```

### CompiledKernel.run - 运行时子系统

triton - runtime 对接

```
# parse options
device = driver.active.get_current_device()
stream = driver.active.get_current_stream(device)
```

然后是 grid，device_type 和 device 相关的 stream/num_warp/num_stages 等参数设定。

最后生成 unique key，有个 compilation cache。根据 key 可以清楚看到 unique parameter list。

![](img/e6b5ef43e3036b89a31bfd0b87316f4d.png)

compilation cache 是 memory based，就是一个 hasp map。

![](img/fe6304d22d39e18106e4954286902a82.png)

compile 之后，就用 binary 去 run 了。

如果是 warmup，只 compile，不 run。

run 的时候，用的是 c_wrapper。除了 grid，其他参数都是 compiled binary 里面了。

但没看到 block 的相关的传参。

TODO: 把 bin 给 dump 出来，能看看汇编 asm？

![](img/fbfca2d4b8a2bf77e466dcb4d63803cb.png)

最后有大量 threading.py 代码，应该就是 trigger kernel 任务时启动的。

1.  如果，CPU 上 triton 也是这种方式提交任务，对 command packet & control flow 设计，可能是重要信息。

1.  从硬件 prefer 的 command packet 大小，反推不同 hardware 的最佳 kernel trigger 粒度。

![](img/1ad932aadbdcc1bb977a271d669c0c9f.png)

```
+-------------------+     +-------------------+
| Entry Function    |     | JITFunction       |
| (Host Scheduling) | --&gt; | (Device Execution)|
+-------------------+     +-------------------+
       |                          |
       v                          v
Grid配置生成器               编译缓存管理器
       |                          |
       v                          v
CUDA Stream绑定              Triton编译器
```

### JITFunction.compile - 编译流水线

入口的核心代码如下

backend 是 third-party 目录下 backend/compiler.py 中定义的 BaseBackend 子类，比如 CUDABackend，HIPBackend。

选哪一个 backend，取决于 active driver 返回的 target 和 backend.supports_target 的检查结果

```
options = backend.parse_options(kwargs)

# attributes
attrvals = [x[1] for x in specialization]
attrs = find_paths_if(attrvals, lambda _, x: isinstance(x, str))
attrs = {k: backend.parse_attr(get_iterable_path(attrvals, k)) for k in attrs}

src = self.ASTSource(self, signature, constexprs, attrs)
# kernel is CompiledKernel
kernel = self.compile(src, target=target, options=options.__dict__)
```

如果 unique key 命中 cache，这部分不会重复跑。

代码比较简单，分 3 部分：

1.  参数准备

1.  self._call_hook。从实现看，是 legacy 代码支持的 hook。

1.  调用 真 compile 函数。

![](img/5aedc42a5a722aa0448709367c3944d0.png)

compile 函数是 从 compiler/compiler.py 里 import 的

![](img/61652d0527f44a0d98912634666af259.png)

self._call_hook。如果命中，就直接用 legacy compile 并 exit。不走后面的 cache & latest compile & run。

![](img/79cd08db8442eeab9338bf069e5bc1b0.png)

### cache 机制

核心代码

```
class JITFunction(KernelInterface[T]):

    def run(self, *args, grid, warmup, **kwargs):
        # xxxx
    
        # self.device_caches 就是 self.create_binder 的 cache.
        # device 存在时，从 cache 获取。如果是新 device，则执行一次 self.create_binder
        # 以 device 为 dict 的 key
        kernel_cache, target, backend, binder = self.device_caches[device]
        bound_args, specialization, options = binder(*args, **kwargs)
    
        # compute cache key
        key = str(specialization) + str(options)
        kernel = kernel_cache.get(key, None)
        # xxx
    
    def __init__(self, fn, xxx):
        # defaultdict(default_factory=None, /, [...]) --&gt; dict with default factory
        self.device_caches = defaultdict(self.create_binder)
        
    def create_binder(self):
        &quot;&quot;&quot;
        Precompute as much as possible.
        &quot;&quot;&quot;
        from ..compiler import CompiledKernel, compile, ASTSource, make_backend
        target = driver.active.get_current_target()
        backend = make_backend(target)
        self.CompiledKernel = CompiledKernel
        self.compile = compile
        self.ASTSource = ASTSource
        binder = create_function_from_signature(self.signature, self.params, backend)
        return {}, target, backend, binder
```

defaultdict(default_factory=None, /, [...]) --> dict with default factory

# notes

根据 meta_params 分析，

+   triton block：写 kernel 代码时，开发者显式定义 BLOCK_SIZE。可以自动算出 grid_size。

+   cuda block。一个 triton block，通常包含多个 cuda block。调度到哪个 sm，由 CUDA Driver + GPU Scheduler 协同完成

+   warp 调度。hardware 自己管理。

**运行时交互**：

+   参数打包：张量指针自动转换为设备指针

+   内存管理：通过PyTorch Allocator与CUDA Context交互

+   流同步：默认绑定当前CUDA流，支持异步执行

建议后续可扩展研究方向：

+   Triton编译器优化策略（如自动向量化、循环展开）

+   与TorchDynamo的协同工作机制

+   跨内核间通信的同步原语

## step 0 - Triton 初始化 & driver 配置

# vector-add

精简后的代码如下，

每个 add_kernel 只处理 1 个 block，内存连续。一次 load 即可。

load 时带上 mask，处理尾块数据不满。

作为对比，cuda 里写的好像是 warp 内 thread 的代码，每个 thread 处理的数据是不连续的。

但，cuda 一个 warp 一个 cycle 的数据应该是连续的(?)。

```
@triton.jit
def add_kernel(x_ptr, y_ptr, output_ptr,  # *Pointer* to output vector.
               n_elements, BLOCK_SIZE: tl.constexpr):
    pid = tl.program_id(axis=0)  # We use a 1D launch grid so axis is 0.
    block_start = pid * BLOCK_SIZE
    offsets = block_start + tl.arange(0, BLOCK_SIZE)
    mask = offsets &lt; n_elements
    # Load x and y from DRAM
    x = tl.load(x_ptr + offsets, mask=mask)
    y = tl.load(y_ptr + offsets, mask=mask)
    output = x + y
    # Write x + y back to DRAM.
    tl.store(output_ptr + offsets, output, mask=mask)


def add(x: torch.Tensor, y: torch.Tensor):
    output = torch.empty_like(x)
    assert x.is_cuda and y.is_cuda and output.is_cuda
    n_elements = output.numel()

    grid = lambda meta: (triton.cdiv(n_elements, meta[&#x27;BLOCK_SIZE&#x27;]), )
    add_kernel[grid](x, y, output, n_elements, BLOCK_SIZE=1024)
    # We return a handle to z but, since `torch.cuda.synchronize()` hasn&#x27;t been called, the kernel is still
    # running asynchronously at this point.
    return output
```

# fused softmax - v2.1

block level 的 reduction & performance tuning 机制。

block level 的 reduction，数据共享一般用 smem？

主要涉及:

1.  runtime/jit.py

1.  compiler/compiler.py

## example code

triton tutorial 02

```
@triton.jit
def softmax_kernel(output_ptr, input_ptr, input_row_stride, output_row_stride, n_cols, BLOCK_SIZE: tl.constexpr):
    # The rows of the softmax are independent, so we parallelize across those
    row_idx = tl.program_id(0)
    # The stride represents how much we need to increase the pointer to advance 1 row
    row_start_ptr = input_ptr + row_idx * input_row_stride
    # The block size is the next power of two greater than n_cols, so we can fit each
    # row in a single block
    col_offsets = tl.arange(0, BLOCK_SIZE)
    input_ptrs = row_start_ptr + col_offsets
    # Load the row into SRAM, using a mask since BLOCK_SIZE may be &gt; than n_cols
    row = tl.load(input_ptrs, mask=col_offsets &lt; n_cols, other=-float(&#x27;inf&#x27;))
    # Subtract maximum for numerical stability
    row_minus_max = row - tl.max(row, axis=0)
    # Note that exponentiation in Triton is fast but approximate (i.e., think __expf in CUDA)
    numerator = tl.exp(row_minus_max)
    denominator = tl.sum(numerator, axis=0)
    softmax_output = numerator / denominator
    # Write back output to DRAM
    output_row_start_ptr = output_ptr + row_idx * output_row_stride
    output_ptrs = output_row_start_ptr + col_offsets
    tl.store(output_ptrs, softmax_output, mask=col_offsets &lt; n_cols)
```

## step 1 - Triton JIT

### Triton JIT 的初始化过程

706 加一行 print，查看 fn list

![](img/912df7a3d3d094570e9b7c02835eca37.png)

fn list:

```
jit_fn: &lt;function cdiv at 0x7424c71104a0&gt;
jit_fn: &lt;function sigmoid at 0x7424c1f2e520&gt;
jit_fn: &lt;function softmax at 0x7424c1f2e700&gt;
jit_fn: &lt;function ravel at 0x7424c7110720&gt;
jit_fn: &lt;function swizzle2d at 0x7424c1f2e840&gt;
jit_fn: &lt;function zeros at 0x7424c1f2e8e0&gt;
jit_fn: &lt;function zeros_like at 0x7424c1f2e980&gt;
jit_fn: &lt;function minimum at 0x7424c1f2e5c0&gt;
jit_fn: &lt;function maximum at 0x7424c1f2e7a0&gt;
jit_fn: &lt;function _argmax_combine at 0x7424c1f2eca0&gt;
jit_fn: &lt;function _argmax_combine_tie_break_left at 0x7424c1f2ed40&gt;
jit_fn: &lt;function _argmax_combine_tie_break_fast at 0x7424c1f2ede0&gt;
jit_fn: &lt;function max at 0x7424c1f2ef20&gt;
jit_fn: &lt;function argmax at 0x7424c1f2efc0&gt;
jit_fn: &lt;function _argmin_combine at 0x7424c1f2ee80&gt;
jit_fn: &lt;function _argmin_combine_tie_break_left at 0x7424c1f2f060&gt;
jit_fn: &lt;function _argmin_combine_tie_break_fast at 0x7424c1f2f100&gt;
jit_fn: &lt;function min at 0x7424c1f2f240&gt;
jit_fn: &lt;function argmin at 0x7424c1f2f2e0&gt;
jit_fn: &lt;function _sum_combine at 0x7424c1f2f1a0&gt;
jit_fn: &lt;function sum at 0x7424c1f2f420&gt;
jit_fn: &lt;function _xor_combine at 0x7424c1f2f380&gt;
jit_fn: &lt;function cumsum at 0x7424c1f2eb60&gt;
jit_fn: &lt;function _prod_combine at 0x7424c1f2ec00&gt;
jit_fn: &lt;function cumprod at 0x7424c1f2f7e0&gt;
jit_fn: &lt;function _indicator at 0x7424c1f2f740&gt;
jit_fn: &lt;function _take_slice at 0x7424c1f2f880&gt;
jit_fn: &lt;function _compare_and_swap at 0x7424c1f2f920&gt;
jit_fn: &lt;function _bitonic_merge at 0x7424c1f2f9c0&gt;
jit_fn: &lt;function sort at 0x7424c1f2fba0&gt;
jit_fn: &lt;function philox_impl at 0x7424c1f2fe20&gt;
jit_fn: &lt;function philox at 0x7424c1f2fd80&gt;
jit_fn: &lt;function randint at 0x7424c1f48180&gt;
jit_fn: &lt;function randint4x at 0x7424c1f482c0&gt;
jit_fn: &lt;function uint_to_uniform_float at 0x7424c1f48360&gt;
jit_fn: &lt;function rand at 0x7424c1f48400&gt;
jit_fn: &lt;function rand4x at 0x7424c1f484a0&gt;
jit_fn: &lt;function pair_uniform_to_normal at 0x7424c1f48540&gt;
jit_fn: &lt;function randn at 0x7424c1f485e0&gt;
jit_fn: &lt;function randn4x at 0x7424c1f48680&gt;
jit_fn: &lt;function softmax_kernel at 0x7424c1f80cc0&gt;
softmax define
before softmax_kernel
after softmax_kernel
out_len: 1823
```

import triton 阶段，就执行了一堆 jit function。清单如下：

![](img/62899b816a9bf46eb1fd9818aef9be34.png)

然后，在 softmax_kernel 定义处，进入 runtime/jit.py:jit，并通过 decorator 进入 JITFunction。

![](img/bd860649224caf0855b461dc6868efdd.png)

JITFunction 有 400 行代码，init 函数在后半部分。

![](img/a2cfc824cfeefb526adf4190965619df.png)

执行 init 函数，有个 tma info。

tma 是 nvidia Hopper 架构的东西，Triton 最新代码，已经都移入了 third_party 的 nvidia backend 中。

![](img/950af42d7706fa1c1ea550cdaa0c1a06.png)

进入 softmax 以后，异步的提交了大量的 kernel instance。

此处的设计是：one kernel instance per row of the input matrix。

softmax_kernel[(n_rows,)] 把一个 kernel 分成了 n_rows 个 grid，每个 grid 是一个 kernel instance。

![](img/cb100c1c5a6d116b68877ce9fb6e9e5c.png)

```
cat v1.out | grep &#x27;enter softmax_kernel&#x27; | sort | uniq -c | wc -l
184

cat v1.out | grep &#x27;enter softmax_kernel&#x27; | sort | wc -l
14491

cat v1.out | grep &#x27;exit softmax_kernel&#x27; | sort | wc -l
47036

cat v1.out | grep &#x27;exit softmax_kernel&#x27; | sort | uniq -c | wc -l
414
```

最后有大量 threading.py 代码，应该就是 trigger kernel 任务时启动的。

1.  如果，CPU 上 triton 也是这种方式提交任务，对 command packet & control flow 设计，可能是重要信息。

1.  从硬件 prefer 的 command packet 大小，反推不同 hardware 的最佳 kernel trigger 粒度。

![](img/b62f5519bb480fb3dff525e28cabec76.png)

### kernel 执行过程

![](img/242ac88c34d7b08d0f91dbc695c45df0.png)

kernel 执行的入口是 __getitem__，实际调用 JITFunction.run。

(n_rows, ) 是 grid 划分，通过 partial bind 过来了。

调用 kernel 时，还有 num_warps=num_warps, 但 kernel 只接收了 BLOCK_SIZE，没处理 grid 和 warp。

即，

1.  调用方管 grid number

1.  kernel 管 BLOCK_SIZE

1.  hardware 管 warp

![](img/ddcf80eb8458cf16793e7c8c1f82c17d.png)

run 这个函数，有 200 行代码。

![](img/231cef5c81ad8dfe51ab2271cc3566ab.png)

### thread 数量分析

input 改成如下，只进入了一次 JITFunction.run

```
x = torch.randn(1, 781, device=&#x27;cuda&#x27;)
```

产生了 128 次的 kernel 进出。猜测是 1 个 grid 有 128 个 block。

也可能是 1 个 block 的 128 个 warp，概率低。

```
cat v2.out | grep &#x27;enter softmax_kernel&#x27; | wc -l
128
cat v2.out | grep &#x27;exit softmax_kernel&#x27; | wc -l
128
```

### JITFunction.run

首先是入参的处理，接管了 grid 和 num_warps 等参数。

![](img/1b51d40f0e5f4f43dd7d484ba2af1a7f.png)

然后是 grid，device_type 和 device 相关的 stream/num_warp/num_stages 等参数设定。

最后生成 unique key，有个 compilation cache。根据 key 可以清楚看到 unique parameter list。

![](img/59cd9966f7c00aea067b994e8ae2ddac.png)

compilation cache 是 memory based，就是一个 hasp map。

![](img/1a684b5bea3916729c9744fb002711a9.png)

compile 之后，就用 binary 去 run 了。

如果是 warmup，只 compile，不 run。

run 的时候，用的是 c_wrapper。除了 grid，其他参数都是 compiled binary 里面了。

但没看到 block 的相关的传参。

TODO: 把 bin 给 dump 出来，能看看汇编 asm？

![](img/9d6fbda607e4f107b5394549841c67fb.png)

### JITFunction.run 的 Compile 过程

如果 unique key 命中 cache，这部分不会重复跑。

代码比较简单，分 3 部分：

1.  参数准备

1.  self._call_hook。从实现看，是 legacy 代码支持的 hook。

1.  调用 真 compile 函数。

![](img/56a5df9b36e39785ece02829bf8f537b.png)

compile 函数是 从 compiler/compiler.py 里 import 的

![](img/8a209b14c19db186656c133bf8eddf98.png)

self._call_hook。如果命中，就直接用 legacy compile 并 exit。不走后面的 cache & latest compile & run。

![](img/2c0720fe873b016d90ce022d2f38b5b8.png)

## step 2 - Kernel Compilation

### compiler.py 真 compile

来自 compiler/compiler.py

文件一共 700 行，compile 函数 250 行，CompiledKernel 100 行。

在最新 main 分支 (2024.03) 上，已经明显改善，refactor 有成效。

整个文件 300 行，compile 函数已经没有 hardware 相关代码了。

前 100 行都在重整入参，没什么用。

![](img/a8bf84d0904701486f07e9b410578705.png)

接下来搞了几个新的数据结构: ClusterInfo / TMAInfos / _device_backend & target.

接下来就是 关键的 compilation stages 构造，只有 ast 和 ttir 是一样的。

后面就开始 hardware specific。但是，hardware 可以注册自己的一组 stages。

然后是没什么用的 find out the signature of the function

![](img/f5fa850211a9fbab05f5bbaa47bd63f7.png)

cache manager，看起来是比较重要的。

但跟 JITFunction 里的 cache 是什么关系？

根据 comment，这里是为了 dump and override IR for debugging？

然后开始 metadata。又是一轮数据重新组织。关键，还从 metadata 里读 tensormaps_info？

![](img/3e69f24e25a7be758eb786bb356dd662.png)

最后 100 行代码，终于开始 run pipeline 了。

first_stage 是 ast 或者 input IR file，已经有 output，不用 run 了。

所以，pipeline 跳过 first_stage 开始 run。

从 for-loop 里的 if-else 数量看出，通用性很差，全是特判。需要 stage by stage 的 code reading 才能理解细节。

编完的东西，写在了 asm 和 metadata 数据结构里。也是一堆 if-else 特判。

![](img/5caf6636789d7e50c587967a1c58da94.png)

编完又是一堆 if-else 特判。然后 生成 so path 并和 medata，asm 一起传给 CompiledKernel 返回。

![](img/7d52bec170b3fd2ac1e3cced31ef13ce.png)

代码问题：

stage 是一个普通的 dict。

list(stages.items())[first_stage:] 出来的 stage 顺序，换个 python 大版本，可能就是玄学了。

实际上，这个 stage 是非常确定的：ttir -> ttgir -> llir -> so/binary

![](img/21cd1a716df05ac0d6dbdf88f9c6843a.png)

### Compilation stages

| Stage / ir_name | device | parse | compile_kernel |
| --- | --- | --- | --- |
| ast | common | fn (original kernel) | None |
| ttir | common | parse_mlir_module(path, context), | lambda src: optimize_ttir( |
|  |  |  |  |
| ttgir | cuda | parse_mlir_module(path, context), | optimize_ttgir( |
| llir | cuda | Path(path).read_text(), | lambda src: ttgir_to_llir(src, extern_libs, target, tma_infos) |
|  |  |  |  |

## step 3 - Kernel Runtime

### CompiledKernel

看起来就是个简单的 runtime，用的是 c_wrapper。在 getitem 的时候，返回 runner。

可以注册 hook，用于 monitor the execution of triton kernels。

![](img/f671087f58fff467ffecd0b45082fffd.png)

# Codegen for CUDA

python/triton/compiler/code_generator.py 有 1200 行代码。

除了 50 行 ast_to_ttir 的胶水。都是 CodeGenerator class 或者他的辅助 api。

CodeGenerator 可能叫 ttir generator 更合理。method 就是一堆 tl 语法的 implementation。

![](img/d3a10d8e9bbd99a2d937ed1feabb8909.png)

## ast_to_ttir

ast 和 ttir 本来就是基本对应的，没啥实质东西。

一共 50 行胶水。

此处，处理了 divisibility。在 pytorch codegen 的时候，会用 triton language 指定这个字段。

![](img/29951571c269286b70010bc14f805157.png)

## ttir_to_ttgir

在 compiler.py 里面。

1.  就 1 个 pass。

1.  传了 magic number: 32。应该是支持 32 threads per warp，nvidia GPU 的配置参数。AMD 和 Intel 可能有 64 的。

```
def ttir_to_ttgir(mod, num_warps, num_ctas, target):
    pm = ir.pass_manager(mod.context)
    pm.enable_debug()
    pm.add_convert_triton_to_tritongpu_pass(num_warps, 32, num_ctas, target.capability)
    pm.run(mod)
    return mod
```

声明和实现，在 c code 里:

```
// triton/python/src/triton.cc
.def(
  &quot;add_convert_triton_to_tritongpu_pass&quot;,
  [](mlir::PassManager &amp;self, int numWarps, int threadsPerWarp) {
    self.addPass(mlir::triton::createConvertTritonToTritonGPUPass(
        numWarps, threadsPerWarp));
  },
  py::arg(&quot;numWarps&quot;) = 4, py::arg(&quot;threadsPerWarp&quot;) = 32)
```

createConvertTritonToTritonGPUPass 的相关 code 如下

![](img/e2794126c18406f0540d18f47e9cf443.png)

## ttgir_to_llir

updated on 2024.12.20

这部分代码，已经大幅度重构了。

```
def ttgir_to_llir(mod, extern_libs, target, tma_infos):
    if extern_libs:
        _add_external_libs(mod, extern_libs)
    # TODO: separate tritongpu_to_llvmir for different backends
    if _is_cuda(target):
        return translate_triton_gpu_to_llvmir(mod, target.capability, tma_infos, runtime.TARGET.NVVM)
    else:
        return translate_triton_gpu_to_llvmir(mod, 0, TMAInfos(), runtime.TARGET.ROCDL)
```

```
  m.def(
      &quot;translate_triton_gpu_to_llvmir&quot;,
      [](mlir::ModuleOp op, int computeCapability, bool isROCM) {
        py::gil_scoped_release allow_threads;
        llvm::LLVMContext llvmContext;
        auto llvmModule = ::mlir::triton::translateTritonGPUToLLVMIR(
            &amp;llvmContext, op, computeCapability, isROCM);
        if (!llvmModule)
          llvm::report_fatal_error(&quot;Failed to translate TritonGPU to LLVM IR.&quot;);

        std::string str;
        llvm::raw_string_ostream os(str);
        llvmModule-&gt;print(os, nullptr);
        os.flush();
        return str;
      },
      ret::take_ownership);
```

核心代码

![](img/eb1683d0915b1195052e498e0eec70bd.png)

# Triton Pass Overvew

triton/python/src/passes.cc 分的不错，overview 很清晰。

![](img/e0ee784f6dd236c87ffc24a18833c0f4.png)

| group | pass | notes |
| --- | --- | --- |
| Triton Common |  | 都是 MLIR 和 LLVM 的 pass，低优先级。 |
|  | createSCCPPass |  |
|  | createSymbolDCEPass |  |
|  | createInlinerPass |  |
|  | createCanonicalizerPass |  |
|  | createCSEPass |  |
|  | createLoopInvariantCodeMotionPass |  |
|  |  |  |
| ttir pass | createCombineOpsPass | +   include/triton/**Dialect/Triton**/Transforms/Passes.h |
|  | createReorderBroadcastPass | +   lib/Dialect/Triton/Transforms/ReorderBroadcast.cpp |
|  | createRewriteTensorPointerPass | +   lib/Dialect/Triton/Transforms/RewriteTensorPointer.cpp |
|  | createConvertTritonToTritonGPUPass | +   include/triton/Conversion/TritonToTritonGPU/Passes.td |
|  |  |  |
| ttgpuir pass | createCoalescePass | +   include/triton/Dialect/TritonGPU/Transforms/Passes.h |
|  | createOptimizeThreadLocalityPass | +   lib/Dialect/TritonGPU/Transforms/OptimizeThreadLocality.cpp |
|  | createPipelinePass |  |
|  | createPrefetchPass |  |
|  | createAccelerateMatmulPass |  |
|  | createReorderInstructionsPass |  |
|  | createOptimizeDotOperandsPass |  |
|  | createRemoveLayoutConversionsPass |  |
|  | createReduceDataDuplicationPass |  |
|  | createAllocateSharedMemoryPass |  |
|  |  |  |
| llvmir |  |  |
|  | createLLVMDIScopePass | DI Scope |
|  |  |  |
| passes_convert |  | ttir -&gt; ttgpuir -&gt; llvmir 的过程，没体现在这里。 |
|  | createConvertSCFToCFPass | add_scf_to_cf |
|  | createConvertControlFlowToLLVMPass | add_cf_to_llvmir |
|  | createConvertIndexToLLVMPass | add_index_to_llvmir |
|  | createArithToLLVMConversionPass | add_arith_to_llvmir |

# optimization Pass

核心基本都在 optimize_ttgir 一个 dialect 里面。

Triton 理念有别于 MLIR 多个 dialect 搞 multi-level lowering。

也是一个不错的思路。

## optimize_ttir

没什么重要的 pass

```
def optimize_ttir(mod, target):
    mod = inline_triton_ir(mod)
    mod = ttir_compute_capability_rewrite(mod, target)
    pm = ir.pass_manager(mod.context)
    pm.enable_debug()
    pm.add_inliner_pass()
    pm.add_triton_combine_pass()
    pm.add_canonicalizer_pass()
    pm.add_reorder_broadcast_pass()
    pm.add_cse_pass()
    pm.add_licm_pass()
    pm.add_symbol_dce_pass()
    pm.run(mod)
    return mod
```

## optimize_ttgir

大部分 pass 都比较重要。

但也已经大量重构了。

```
def optimize_ttgir(mod, num_stages, num_warps, num_ctas, target, cluster_info, enable_warp_specialization,
                   enable_persistent, optimize_epilogue):
    is_cuda = _is_cuda(target)
    if is_cuda:
        capability = target.capability
    pm = ir.pass_manager(mod.context)
    pm.enable_debug()
    pm.add_tritongpu_coalesce_pass()
    # TODO(Qingyi): Move PlanCTAPass to the front of CoalescePass
    pm.add_plan_cta_pass(cluster_info)
    if is_cuda:
        pm.add_tritongpu_rewrite_tensor_pointer_pass(capability)
        pm.add_plan_cta_pass(cluster_info)
    pm.add_tritongpu_remove_layout_conversions_pass()
    if is_cuda:
        pm.add_tritongpu_accelerate_matmul_pass(capability)
    pm.add_tritongpu_remove_layout_conversions_pass()
    if optimize_epilogue:
        pm.add_tritongpu_optimize_epilogue_pass()
    pm.add_tritongpu_optimize_dot_operands_pass()
    pm.add_cse_pass()
    ws_enabled = False
    # `num_warps` does not mean the total number of warps of a CTA when
    # warp specialization is enabled.
    # it&#x27;s the responsibility of the compiler to figure out the exact
    # `num_warps` to use.
    # TODO: support the case where `num_warps` from user is not 4.
    if capability // 10 &gt;= 9 and enable_warp_specialization and num_warps == 4:
        pm.add_tritongpu_ws_feasibility_checking_pass(capability)
        pm.run(mod)
        ws_enabled = ir.is_ws_supported(mod)
        pm = ir.pass_manager(mod.context)
        pm.enable_debug()
    if ws_enabled:
        pm.add_tritongpu_wsdecomposing_pass(capability)
        pm.add_tritongpu_wspipeline_pass(num_stages, num_warps, capability)
        pm.add_tritongpu_wsmutex_pass(capability)
        pm.add_tritongpu_wsmaterialization_pass(capability)
        pm.add_licm_pass()
        pm.add_cse_pass()
    else:
        pm.add_tritongpu_pipeline_pass(num_stages, num_warps, num_ctas, capability)
    pm.add_tritongpu_materialize_load_store_pass(num_warps, capability)
    if capability // 10 &lt;= 8:
        pm.add_tritongpu_prefetch_pass()
    pm.add_tritongpu_optimize_dot_operands_pass()
    pm.add_tritongpu_remove_layout_conversions_pass()
    pm.add_tritongpu_decompose_conversions_pass()
    pm.add_tritongpu_ws_fixup_missing_attrs_pass()
    pm.add_tritongpu_reorder_instructions_pass()
    pm.add_cse_pass()
    pm.add_symbol_dce_pass()
    if capability // 10 &gt;= 9:
        pm.add_tritongpu_fence_insertion_pass()
    pm.add_tritongpu_ws_fixup_missing_attrs_pass()
    pm.add_tritongpu_optimize_thread_locality_pass()
    pm.add_canonicalizer_pass()
    pm.run(mod)
    return mod
```

## others

llvm IR 没有 optimizer。

应该是用 LLVM 的 pass 去做优化了。

# Code Screen @ 2024.03.15

compile 函数，明显变简单了。

1.  核心代码都在 stages 的 compile_ir 实现里。

1.  stages 的 list，全在 backend.add_stages 里面。

![](img/18b1b108739d2054b802ca7a9095e16c.png)

选 hardware arch 相匹配的 third_party backend。

![](img/e9b5fb65f3cbdef9e9ddde157eb4bf50.png)

## third-party backend

backend 的实际实现，thirdparty 目录下

amd 的

![](img/b9cf1273a5c8ba4bd991bda283b93443.png)

nv 的

![](img/eb486d043506d4b0116e2060694998f4.png)

## make_ttir / ttgir / llir

拉了最新的 triton master 分支代码，backend 终于从 triton core 里面剥离了。但是，神特喵的 make_ttir 一模一样的代码写 2 遍。

但 make_ttgir 和 make_llir 又是自己管自己的。

真 framework，啥也不干。

![](img/69b5a16607d284311cac5f6bf33e6e53.png)

名叫 make_llir，实际都假定了上游是 ttgir，并且都先做了一些 ttgir 的后处理。再转 llvm ir。

amd 和 nv 转 llvm ir 的 pass 还不一样。

这种不一样，体现了，二者有 ttgir 和 llvmir 难以兼容的逻辑。是什么？

![](img/be3e04daffa9892652688ad882245ffa.png)

## ttir

3 个 transform，1 个 Conversion

### 3 个 transform pass

transform pass 都声明在 triton/include/triton/Dialect/Triton/Transforms/Passes.h

![](img/c8e7997419ecca431cd9df64edc90a81.png)

td 文件，更有用，description 写的很清晰。

![](img/faa1e58d1e574941dc0785b1ae77e639.png)

实现都在 triton/lib/Dialect/Triton/Transforms 目录下，3 个文件

combine 是注册了 8 个 pattern，然后 applyPatternsAndFoldGreedily。

6 个 pattern 实现在 td 文件里，2 个在 cpp 里。

![](img/53319a720653724b28af283d7e57b761.png)

ReorderBroadcastPass 逻辑简单，

实现上，有个细节没看懂。

注册了 2 组 pattern，每组都带一个 CanonicalizePattern，但没看懂在 Canonicalize 啥。

match 和 rewrite 都没重写。

然后是 2 个实质的 pattern: MoveBroadcastAfterElementwisePattern 和 MoveSplatAfterElementwisePattern

![](img/72579fc02993b2d26d64ac9d0e338e08.png)

RewriteTensorPointerPass 的实现，比较复杂一些。

但，注释已经写的很清楚了。

![](img/3311cf0699d110b2b5d42309170165e4.png)

调用链:

runOnOperation -> visitOperation -> loops rewriteOp -> rewriteXXXOp funcs

![](img/63ff3916128c17b187d717ef3b7c2883.png)

### IR td 文件

TritonTypes.td

triton/include/triton/Dialect/Triton/IR/TritonTypes.td

定义了 triton 内的数据类型，感觉比较有用。

比如，有 pointer 和 TT_MemDescType。

可以看到，

pointer 可以指向 scalar 和 tensor，分别是怎么定义的。

TT_MemDescType 包含 a base pointer (scalar) and a descriptor of the memory，如果 memory 是 constant, it can have multiple views and the descriptor can be changed without changing the underlying memory

TritonOps.td

一共定义了 39 个 TT_Op，在 triton/include/triton/Dialect/Triton/IR/TritonOps.td

![](img/642e99068bbfc9b7407de5724d1c1f1a.png)

一个 op 的 td 定义的例子

![](img/294715cbbe3f7193589ac1c3b2490f0e.png)

ag int_to_ptr 可以看到掉用处

根据 test/xxxx.mlir 可以看到 op 的 例子，input 是 int，output 是 ptr。

这个 op 的功能，类似于 c++ 的 cast API。

![](img/5d660ecebd6ecdcf304a715cde6cb680.png)

### Conversion Pass

pass.td 有信息量，解释了 4 个 option 的含义。

![](img/1eb4ec7c12bea7eb377da6661ba1b3e6.png)

+   CTA stands for Cooperative Thread Array, which is a group of threads that can cooperate and synchronize within a block in CUDA programming.

+   CGA stands for Cooperative Group of Threads, which is a higher-level abstraction introduced in newer versions of CUDA to enable more flexible and efficient thread cooperation across thread blocks.

CGA allows threads from different thread blocks to synchronize and communicate with each other.

pass.h 里面没东西，只是 include pass.td 生成的 .h.inc 文件

![](img/550995a108f37b02d69970ec274e1e4d.png)

lib 的实现，应该是依赖 triton IR 和 Triton GPU IR 的东西。

source 和 destination 都依赖，也合理。

triton/lib/Conversion/TritonToTritonGPU/TritonGPUConversion.cpp

这几个 illegalOp 的处理，需要关注一下

![](img/74b7ff61a52f3c78118a4881320b01af.png)

pass 的实现，在

triton/lib/Conversion/TritonToTritonGPU/TritonToTritonGPUPass.cpp

先是 patterns 里添加了很多 pattern

![](img/eb6c4fcdceda3f31676f9fd623253de4.png)

然后设置 attribute

![](img/230c82367db5d8efc7877e9c8a6e4f87.png)

最后 applyPartialConversion

![](img/663f7608e39b213396da652746dad6ef.png)

以 triton pattern 为例，注册的 pattern 清单如下

![](img/69c8c2f354ad01622fea54d958561205.png)

以其中的 TritonReducePattern 为例，matchAndRewrite 实现如下

![](img/28e1639707d2ab6888bfebbbe87bb83c.png)

粗看下来，就是一堆的 pattern matchAndRewrite，

然后带了 4 个非常 GPU 的参数：

+   numWarps

+   threadsPerWarp = 32，被 4 个 OP pattern 用到 dot, split, cat, expand.

+   numCTAs = 1

+   computeCapability = 80。好像没有用到，需要再排查一下。

threadsPerWarp 在 TritonToTritonGPUPass.cpp 里的使用

主要是 TritonDotPattern 里大量用到了，矩阵乘的 OP。

![](img/1e7896e5b7dc55e8532ae7f69af7b826.png)

另外是 TritonSplitOpPattern，调用了 2 次 getThreadsPerWarp，

![](img/b0f0d87005c7c0fbfe10122e4020aa46.png)

还有 2 个 pattern 是通过 getThreadsPerWarp 间接用到的。

TritonExpandDimsPattern

![](img/be7b38611839d90540bbb0b2003a8142.png)

TritonCatPattern

![](img/d44c4326b2b7f78440f0eaa1dfd38014.png)

## TritonGPU IR

### transform pass

大部分的 pass，都很关键。

跟 ttir 一样，

triton/include/triton/Dialect/TritonGPU/Transforms/Passes.h 包含里 10 个 transform pass 声明

![](img/440022f11b0a1c0cedbc5dd3a6f50ade.png)

跟 ttir 一样，pass.td 里面有 comment，更清晰。

TritonGPUPipeline 这个 pass 看起来，有点像 tiling 相关的。

把一个 loop 内的 blocking load 转成 async load & multi-buffer the data。

![](img/ed1571871f1c60c5fbb86a36f4359108.png)

也有 4 个参数，但与 ttir Conversion Pass 相比，

少了 numThreadPerWarp，新增了 numStages: number of pipeline stages

这个 TritonGPUPrefetch，也挺像 GPU 相关的。

![](img/365acbb1cb48cf0ce7db269fbe44d1a1.png)

TritonGPUAccelerateMatmul 是 layout 转换的关键 pass

![](img/973c96765c874a42add33f6af1bf6e79.png)

### IR td 文件

只有 10 个 op。作为对比，ttir 有 39 个。

都是 async 和 memory 操作相关的 op。--> 围绕 memory 的 tiling 操作？

![](img/163e0ba55cd1912e55ce6bfe3270118a.png)

type 只定义了 1 个 TTG_TokenType

![](img/b935b0921521c727afbdf73837c8dad4.png)

triton/include/triton/Dialect/TritonGPU/IR/TritonGPUAttrDefs.td

感觉非常有用。

![](img/d314dc31043faac2925379dfe1445557.png)

## NVGPU

没有 transform 文件，所以，好像啥都没干，用不上了？

td 定义了 12 个 op

![](img/67506204fc3e809534181339f6d22f65.png)

## TritonNvidiaGPU

td 定义了 6 个 op

![](img/afbf24653a86e4629b33b8ca43b55405.png)

2 个 pass，看名字都挺重要

1.  PlanCTA

1.  FenceInsertionP

![](img/26a7a4555e7a0a300016898f6bb75e5c.png)

PlanCTA 的 cpp 有 1000 多行，而且，与 tiling 有关

![](img/5b272f7af88a6dc52889f9cf027c85d4.png)

# Key Pass

## PlanCTA

TritonNvidiaGPU 的 pass

![](img/dc146fb30a9d75ce304c71d27174da0e.png)

依此执行:

1.  processDot

1.  processReduce

1.  processStoreLikeOps

![](img/c3a7cf410f7b7a924a4a2a2d7f483b12.png)

![](img/833045b255a4c52ed0a5026bc4833437.png)

![](img/064f9c468315c9bf273c293e8453dff5.png)