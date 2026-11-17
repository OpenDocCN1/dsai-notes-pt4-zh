
# vLLM LLM 类与 SequenceGroup

## 一、LLM 类与 generate() 调用流程

```
vllm/entrypoints/llm.py
class LLM:
    """An LLM for generating texts from given prompts and sampling parameters.
       ...
    """
    def __init__(self, model: str, tokenizer: Optional[str] = None, tokenizer_mode: str = "auto", trust_remote_code: bool = False, tensor_parallel_size: int = 1, dtype: str = "auto", quantization: Optional[str] = None, revision: Optional[str] = None, tokenizer_revision: Optional[str] = None, seed: int = 0, gpu_memory_utilization: float = 0.9, swap_space: int = 4, enforce_eager: bool = False, max_context_len_to_capture: int = 8192, disable_custom_all_reduce: bool = True, **kwargs,) -> None:
        ...

==============================================================================
使用配置好的engine参数，初始化LLMEngine实例
==============================================================================
self.llm_engine = LLMEngine.from_engine_args(engine_args, usage_context=UsageContext.LLM_CLASS)

==============================================================================
用于全局唯一的request_id，在vLLM中内核引擎的处理中，1个prompt视为1个request，分配全局唯一的request_id
==============================================================================
self.request_counter = Counter()

    def generate(self, prompts: Optional[Union[str, List[str]]] = None,
        sampling_params: Optional[SamplingParams] = None,
        prompt_token_ids: Optional[List[List[int]]] = None,
        use_tqdm: bool = True,
        lora_request: Optional[LoRARequest] = None,
        multi_modal_data: Optional[MultiModalData] = None,) -> List[RequestOutput]:
        """Generates the completions for the input prompts.

        NOTE: This class automatically batches the given prompts, considering
        the memory constraint. For the best performance, put all of your prompts
        into a single list and pass it to this method.

        Args:
            prompts: prompts可以是str，也可以是list[str]
            sampling_params: 采样超参，例如温度、top_k等；如果为None则使用vLLM默认的参数
            prompt_token_ids: prompt对应的token_id，如果没有提供的话，vllm会调用tokenizer进行转换
            use_tqdm: 是否要展示process bar
            lora_request: 如果想请求特定的lora_adapter，可以将它的path等信息包装在该请求中,
                          但vLLM建议尽量不要使用这种方式，因为私有的lora adapter可能会带来一些
                          安全性的问题
            multi_modal_data: 多模态相关的数据

        Returns:
            A list of RequestOutput objects containing the generated
            completions in the same order as the input prompts.
        """
        if prompts is None and prompt_token_ids is None:
            raise ValueError("Either prompts or prompt_token_ids must be provided.")
        if isinstance(prompts, str):
            prompts = [prompts]
        if (prompts is not None and prompt_token_ids is not None and len(prompts) != len(prompt_token_ids)):
            raise ValueError("The lengths of prompts and prompt_token_ids must be the same.")
        if sampling_params is None:
            sampling_params = SamplingParams()
        if multi_modal_data:
            multi_modal_data.data = multi_modal_data.data.to(torch.float16)

============================================================================
将request添加到engine中在vLLM内核运算逻辑中，1个prompt算1个request，需要有1个全局唯一的request_id
============================================================================
        num_requests = len(prompts) if prompts is not None else len(prompt_token_ids)
        for i in range(num_requests):
            prompt = prompts[i] if prompts is not None else None
            token_ids = None if prompt_token_ids is None else prompt_token_ids[i]

========================================================================
将每个prompt添加进LLMEngine中，_add_request具体做了以下几件事：
- 将每个prompt处理成特定的输入类型（SequenceGroup实例，后文会细说）
- 将每个prompt加入Scheduler的waiting队列，等待处理
========================================================================
            self._add_request(prompt,
                sampling_params,
                token_ids,
                lora_request=lora_request,
                multi_modal_data=MultiModalData(type=multi_modal_data.type, data=multi_modal_data.data[i].unsqueeze(0)) if multi_modal_data else None,)

============================================================================
把这个batch的所有prompt都添加完后，执行推理，详情参见_run_engine
============================================================================
        return self._run_engine(use_tqdm)

    def _add_request(self, prompt: Optional[str],
        sampling_params: SamplingParams,
        prompt_token_ids: Optional[List[int]],
        lora_request: Optional[LoRARequest] = None,
        multi_modal_data: Optional[MultiModalData] = None,) -> None:
        每个prompt赋1个request_id
        request_id = str(next(self.request_counter))
        self.llm_engine.add_request(request_id, prompt, sampling_params, prompt_token_ids,
            lora_request=lora_request,
            multi_modal_data=multi_modal_data)

    def _run_engine(self, use_tqdm: bool) -> List[RequestOutput]:
        Initialize tqdm.
        if use_tqdm:
            num_requests = self.llm_engine.get_num_unfinished_requests()
            pbar = tqdm(total=num_requests, desc="Processed prompts", dynamic_ncols=True)

===========================================================================
如果当前调度器中还有没完成推理的请求（调度器中waiting/running/swapped任一队列非空）
===========================================================================
        outputs: List[RequestOutput] = []
        while self.llm_engine.has_unfinished_requests():

=========================================================================
执行1次推理调度（step），决定哪些请求的数据可以参与到这次推理中
=========================================================================
            step_outputs = self.llm_engine.step()
            for output in step_outputs:

=====================================================================
如果本step后，有请求已经完成了推理，就将推理结果装进outputs中
=====================================================================
                if output.finished:
                    outputs.append(output)
            if use_tqdm:
                pbar.update(1)
        if use_tqdm:
            pbar.close()

        Sort the outputs by request ID.
        This is necessary because some requests may be finished earlier than
        its previous requests.
        outputs = sorted(outputs, key=lambda x: int(x.request_id))
        return outputs
```

总结来说，当我们调用 `outputs = llm.generate(prompts, sampling_params)` 时，它实际做了两件事情：

+   `_add_request`：将输入数据传给 `LLMEngine`，它具体做了如下事情：
    +   把每 1 个 `prompt` 包装成一个 `SequenceGroup` 对象。从客户端角度看，1 个请求可能包含多个 `prompts`，例如离线批处理场景下你可以将 1 个 batch 理解成 1 个请求；但是从 `LLMEngine` 的角度看，1 个 `prompt` 是 1 个请求，所以它会对输入数据进行预处理。在后文对 `SequenceGroup` 的讲解中，我们会来看 vLLM 这样做的意义。
    +   把包装成 `SequenceGroup` 对象的数据加入调度器（`Scheduler`）的 `waiting` 队列，等待处理。这一块相关的细节，我们放在后文说。

+   `_run_engine`：执行推理。只要调度器的 `waiting/running/swapped` 队列非空，我们就认为此时这批 batch 还没有做完推理，这时我们就会调用 `LLMEngine` 的 `step()` 函数，来完成 1 次调度以决定要送哪些数据去做推理。

所以，想要知道调度器的运作流程，我们只要从 `LLMEngine` 的 `add_request()` 和 `step()` 两个函数入手就好了。不过在正式进入这两个函数的讲解之前，我们先来看和输入数据一个问题：为什么要把每个 `prompt` 都包装成一个 `SequenceGroup` 实例？`SequenceGroup` 又长什么样呢？

## 二、SequenceGroup

### 2.1 原生输入

在一般的推理场景中，我们通常给模型传 1 个 `prompt` 及相关的采样参数，让模型来做推理。此时你的输入可能长下面这样：

```
("To be or not to be,", SamplingParams(temperature=0.8, top_k=5, presence_penalty=0.2)),
```

但在其余的场景中，模型 decoding 的策略可能更加复杂，例如：

+   Parallel Sampling：你传给模型 1 个 `prompt`，希望模型基于这个 `prompt`，给出 n 种不同的 output
+   Beam Search：你传给模型 1 个 `prompt`，在采用 Beam Search 时，每个推理阶段你都会产出 top `k` 个 output，其中 `k` 称为 Beam width（束宽）。

这些情况下，你传给模型的输入可能长下面这样：

```
Parallel Sampling("What is the meaning of life?", SamplingParams(n=2, temperature=0.8, top_p=0.95, frequency_penalty=0.1))
Beam Search (best_of = 束宽)("It is only with the heart that one can see rightly", SamplingParams(n=3, best_of=3, use_beam_search=True, temperature=0.0)),
```

【备注：`SamplingParams` 遵从 OpenAI [API 范式，对其中](https://link.zhihu.com/?target=https%3A//platform.openai.com/docs/api-reference/completions) 各种参数的解释可参见 OpenAI 官方文档】

总结来说，可能出现 `1 个`prompt` -> 多个`outputs`` 的情况。那是否能设计一种办法，对 1 个 `prompt` 下所有的 `outputs` 进行集中管理，来方便 vLLM 更好做推理呢？

### 2.2 SequenceGroup 的作用

+   `1 个`prompt` -> 多个`outputs`` 这样的结构组成一个 `SequenceGroup` 实例。

+   其中每组 "`prompt` -> `output`" 组成一个序列（`seq`，属于 `Sequence` 实例），每个 `seq` 下有若干状态（`status`）属性，包括：

+   `WAITING`：正在 `waiting` 队列中。`waiting` 队列中的序列都没有做过 `prefill`。

+   `RUNNING`：正在 `running` 队列中，即已经开始做推理。

+   `SWAPPED`：正在 `swapped` 队列中，表示此时 gpu 资源不足，相关的 `seq_group` 被抢占，导致其暂停推理，相关的 KV block 被置换到 cpu 上（`swap out`），等待 gpu 资源充足时再置换回来重新计算（`swap in`）。

+   若干和 `Finish` 相关的状态，表示该 `seq` 推理已经结束，具体包括：
