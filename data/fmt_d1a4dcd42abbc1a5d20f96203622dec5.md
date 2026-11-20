
## 2.2 Sequence 的状态

+ `FINISHED_STOPPED`：正常执行完毕，例如碰到 `<eos>` 符号，该 `seq` 的推理正常结束了

+ `FINISHED_LENGTH_CAPPED`：因为 `seq` 的长度达到最大长度限制，而结束推理

+ `FINISHED_ABORTED`：因不正常状态，而被终止的推理。例如客户端断开连接，则服务器会终止相关 `seq` 的推理

+ `FINISHED_IGNORED`：因 `prompt` 过长而被终止执行的推理。本质上也是受到长度限制

+ 在 `vLLM` 中有一个重要假设：一个 `seq_group` 中的所有 `seq` 共享 1 个 `prompt`。

我们来通过一个具体的例子，更好感受一下 `SequenceGroup` 的作用：

![](img/eb607f8eaf8529934b3d5da6e0a63e9f.png)

+ 在推理开始之前，这个 `seq_group` 下只有 1 条 `seq`，它就是 `prompt`，状态为 `waiting`。

+ 在第 1 个推理阶段，调度器选中了这个 `seq_group`，由于它的采样参数中 `n = 4`，所以在做完 `prefill` 之后，它会生成 4 个 `seq`，它们的状态都是 `running`。

+ 在若干个推理阶段后，`gpu` 上的资源不够了，这个 `seq_group` 不幸被调度器抢占（`preemption`），它相关的 `KV block` 也被 `swap out` 到 `cpu` 上。此时所有 `seq` 的状态变为 `swapped`。这里要注意，当一个 `seq_group` 被抢占时，对它的处理有两种方式：

+ `Swap`：如果该 `seq_group` 下的 `seq` 数量 > 1，此时会采取 `swap` 策略，即把 `seq_group` 下【所有】`seq` 的 `KV block` 从 `gpu` 上卸载到 `cpu` 上。（`seq` 数量比较多，直接把算出的 `KV block` 抛弃，比较可惜）

+ `Recomputation`：如果该 `seq_group` 下的 `seq` 数量 = 1，此时会采取 `recomputation` 策略，即把该 `seq_group` 相关的物理块都释放掉，然后将它重新放回 `waiting` 队列中。等下次它被选中推理时，就是从 `prefill` 阶段开始重新推理了，因此称为“重计算”。（`seq` 数量少，重新计算 `KV block` 的成本不高）

【注意，并不是每个 `seq_group` 都会经历抢占，具体要看调度器策略和 `gpu` 资源使用情况】

+ 又过了若干个推理阶段，`gpu` 上的资源又充足了，此时执行 `swap in` 操作，将卸载到 `cpu` 上的 `KV block` 重新读到 `gpu` 上，继续对该 `seq_group` 做推理，此时 `seq` 的状态又变为 `running`。

+ 又过了若干个推理阶段，该 `seq_group` 中有 1 个 `seq` 已经推理完成了，它的状态就被标记为 `finish`，此后这条已经完成的 `seq` 将不参与调度。

+ 又过了若干个推理阶段，这个 `seq_group` 下所有的 `seq` 都已经完成推理了，这样就可以把它作为最终 `output` 返回了。

相信通过这个例子，我们已经能更好理解为什么 `vLLM` 要把 1 个 `prompt` 包装成 `SequenceGroup` 实例了。接下来我们就来看 `SequenceGroup` 实例的具体结构。

## 2.3 SequenceGroup 的结构

`SequenceGroup` 相关的脚本在 `vllm/sequence.py` 中，下图给出了 `SequenceGroup` 的结构图解（仅列出重要的属性和方法）：

![](img/6ca1e9170d3268f96e5ca05440ae33c7.png)

### （1）结构总述

#### SequenceGroup

+ `self.seqs_dict`：`{seq_id: seq}`，其中每个 `seq` 是一个 `Sequence` 对象。正如我们前文介绍的那样，一个 `seq_group` 下包含若干 `seqs`

+ `self.sampling_params`：采样参数

+ `self.metrics`：记录该 `seq_group` 相关的指标，例如该 `seq_group` 是什么时候被加入 `LLMEngine` 的（`arrival_time`），该 `seq_group` 第一次被调度器选中调度是什么时候等等。调度器在选择时，会参考 `seq_groups` 们的这些指标来做决策。

+ `get_max_num_running_steps`：该 `seq_group` 在剩余生命周期内并行 `running` 的最大 `seq` 数量。“剩余生命周期”指从此刻一直到 `seq_group` 中所有的 `seq` 都做完推理。举个例子来说，我们看 2.2 节配图中倒数第 3 个时刻，此时这个 `seq_group` 内所有的 `seq` 都还没结束推理，所以若调用这个方法，则返回值为 4；再看倒数第 2 个时刻，此时有 1 个 `seq` 已经完成了推理，所以若调用这个方法，则返回值为 3。在后续调度策略代码中，我们将经常看到这个方法被调用，目的是用于估计若当前对一个 `seq_group` 做推理，它将消耗多少 `gpu` 资源。

我们来详细看下 `get_max_num_running_steps` 代码实现（一切尽在注释中）：

```
    def get_max_num_running_seqs(self) -&gt; int:&quot;&quot;&quot;The maximum number of sequences running in parallel in the remaining
        lifetime of the request.
        返回请求在其剩余生命周期中并行运行的最大序列数。
        &quot;&quot;&quot;============================================================================若采用beam search，每1个推理阶段都是best_of（束宽）个seq在running============================================================================if self.sampling_params.use_beam_search:return self.sampling_params.best_of============================================================================如果不采用beam search============================================================================else:=========================================================================此时best_of默认和n一致，即表示我们希望1个prompt产出n个outputs。因此理论上，这个seq_group下会维护best_of个seq（这就是self.num_seqs()的返回值）。如果出现best_of &gt; self.num_seqs()的情况，说明该seq_group刚从waiting变成running准备做推理（参考2.2节配图中左侧第1个时刻），此时对于这个seq_group来说，其剩余生命周期并行运行的最大seq数量为best_of=========================================================================if self.sampling_params.best_of &gt; self.num_seqs():At prompt stage, the sequence group is not yet filled upand only have one sequence running. However, in thegeneration stage, we will have best_of sequences running.return self.sampling_params.best_of=========================================================================其余时刻（例如2.2节配图中非左侧第1个时刻的所有时刻）下，我们就返回这个seq_group中未完成推理的seq数量。根据2.2节介绍，我们知道一个seq的完成状态有四种：SequenceStatus.FINISHED_STOPPED,SequenceStatus.FINISHED_LENGTH_CAPPED,SequenceStatus.FINISHED_ABORTED,SequenceStatus.FINISHED_IGNORED=========================================================================return self.num_unfinished_seqs()
```

#### Sequence

对于一个 `seq`，我们重点来看它的属性 `self.logical_token_blocks`（逻辑块）和方法 `_append_tokens_to_blocks`（生成逻辑块的方法）。在 `vLLM` 中，每个 `seq` 都单独维护一份属于自己的逻辑块，不同的逻辑块可以指向同一个物理块（此刻你一定很关心逻辑块和物理块是如何做映射的，我们会循序渐进地讲解这点，现在你可以先忽略映射方法，把目光聚焦于“一个 `seq` 的逻辑块长什么样，怎么初始化它的逻辑块”）

### （2）1 个逻辑块的结构

我们先来回答“1 个逻辑块长什么样”这个问题，逻辑块定义的代码比较简单，所以我们直接看代码（一切尽在注释中），代码路径 `vllm/block.py`

```
class LogicalTokenBlock:&quot;&quot;&quot;A block that stores a contiguous chunk of tokens from left to right.

    Logical blocks are used to represent the states of the corresponding
    physical blocks in the KV cache.
    
    KV cache的逻辑块
    &quot;&quot;&quot;def init(self,block_number: int, 逻辑块的序号block_size: int, 每个逻辑块中有多少个槽位（默认为16）) -&gt; None:self.block_number = block_numberself.block_size = block_size逻辑块刚初始化时，将其中的每个token_id都初始化为_BLANK_TOKEN_ID（-1）self.token_ids = [_BLANK_TOKEN_ID] * block_size 
        当前逻辑块中已经装下的token的数量self.num_tokens = 0def is_empty(self) -&gt; bool:&quot;&quot;&quot;判断当前逻辑块是为空&quot;&quot;&quot;return self.num_tokens == 0def get_num_empty_slots(self) -&gt; int:&quot;&quot;&quot;当前逻辑块的空余槽位&quot;&quot;&quot;return self.block_size - self.num_tokensdef is_full(self) -&gt; bool:&quot;&quot;&quot;判断当前逻辑块是否已经被装满&quot;&quot;&quot;return self.num_tokens == self.block_sizedef append_tokens(self, token_ids: List[int]) -&gt; None:&quot;&quot;&quot;将给定的一些token_ids装入当前逻辑块中&quot;&quot;&quot;给定的token_ids的长度必须 &lt;= 当前逻辑块剩余的槽位assert len(token_ids) &lt;= self.get_num_empty_slots()当前逻辑块第一个空槽的序号curr_idx = self.num_tokens将这些tokens装进去self.token_ids[curr_idx:curr_idx + len(token_ids)] = token_ids更新当前逻辑块中tokens的数量self.num_tokens += len(token_ids)def get_token_ids(self) -&gt; List[int]:&quot;&quot;&quot;获取当前逻辑块中所有被装满的位置的token_ids&quot;&quot;&quot;return self.token_ids[:self.num_tokens]def get_last_token_id(self) -&gt; int:&quot;&quot;&quot;获取当前逻辑块所所有被装满的位置的最后一个token_id&quot;&quot;&quot;assert self.num_tokens &gt; 0return self.token_ids[self.num_tokens - 1]
```

### （3）再回到 `Sequence` 上来

知道了每个逻辑块的结构，我们现在来回答“怎么给一个 `seq` 分配逻辑块”这个问题，也就是回到 2.3（1）中 `Sequence` 的 `_append_tokens_to_blocks` 方法上来：当一个 `seq` 只有 `prompt` 时，这个方法负责给 `prompt` 分配逻辑块；当这个 `seq` 开始产出 `output` 时，这个方法负责给每一个新生成的 `token` 分配逻辑块，整个过程如下图（图片来自 `vLLM` 论文，大家忽略图中 `block_table` 的部分）：

![](img/c5f449da956764227d1e2e3b32d16441.png)

代码如下（一切尽在注释中，`/vllm/sequence.py`）:

```
 def _append_tokens_to_blocks(self, token_ids: List[int]) -&gt; None:&quot;&quot;&quot;
        将token_ids动态填入逻辑块列表中
        Args:
            token_ids: prompt部分的token_ids
        &quot;&quot;&quot;cursor = 0遍历prompt token_ids中的每一个token_idwhile cursor &lt; len(token_ids):如果当前逻辑块列表（`logical_token_blocks`）为空if not self.logical_token_blocks:则先append一个逻辑块，该逻辑块index为0，大小为16，其中的每一个token_id为-1self._append_logical_block()取出逻辑块列表中的最后一个逻辑块last_block = self.logical_token_blocks[-1]如果这最后一个逻辑块中已经没有槽位if last_block.is_full():那么再append一个逻辑块，其大小为16，其中每一个token_id为-1self._append_logical_block()把这个新append的逻辑块取出来last_block = self.logical_token_blocks[-1]检查当前取出的逻辑块中空槽位的数量num_empty_slots = last_block.get_num_empty_slots()用当前的token_ids填充空槽位，直到无法填满为止last_block.append_tokens(token_ids[cursor:cursor +num_empty_slots])cursor += num_empty_slots
```

好，到目前为止，我们就把 `vLLM` 对输入数据做预处理的部分介绍完了，简单总结下：

+ 在 `vLLM` 内部计算逻辑中，1 个 `prompt` 是 1 个 `request`

+ 每个 `prompt` 将被包装成一个 `SequenceGroup` 实例提供给调度器做调度

+ 1 个 `SequenceGroup` 实例下维护着若干个 `Sequence` 实例，对应着“1 个 `prompt` -> 多个 `outputs`"这种更一般性的解码场景。

+ 1 个 `Sequence` 实例下维护着属于自己的逻辑块列表，数据类型为 `List[LogicalTokenBlock]`

## 三、`add_request()`：将 `seq_group` 添加进调度器 `waiting` 队列

写了这么多，你是不是已经忘记上面都说了些什么了，不要紧，我们快速回顾下：

+ 首先，我们明确了 `vLLM` 最重要的推理内核引擎是 `LLMEngine`

+ `LLMEngine` 下有两个最重要的方法：`add_request()` 和 `step()`

+ `add_request()` 负责将每个 `prompt` 都包装成一个 `SequenceGroup` 对象，送入调度器的 `waiting` 队列中等待调度

+ `step()` 负责执行 1 次推理过程，在这个过程中，调度器首先决定哪些 `seq_group` 可以被送去推理，然后 `model_executor` 负责实际执行推理。

现在，在知道 `SequenceGroup` 相关定义的基础上，我们可以来看 `add_request()` 了，我们直接来看代码（一切尽在注释中，为了方便阅读，代码有所省略）：
