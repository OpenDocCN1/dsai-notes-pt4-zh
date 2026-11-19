
# LLM 整体接口的重构（在Lagent侧实现）

> 来源：[https://aicarrier.feischu.cn/docx/AbCFdKZs0o5lmvx81nrcQIWOnzf](https://aicarrier.feischu.cn/docx/AbCFdKZs0o5lmvx81nrcQIWOnzf)

## LLM 整体接口的重构（在Lagent侧实现，MindSearch）

1、设计文档（统一的入口、不同的后端，研究员实时交流，不同后端基本已经实现）

2、按照设计文档开发

3、测试

现在的代码都在 `https://github.com/InternLM/lagent/tree/main/lagent/llms`

### 背景

现在 Lagent 实现了 `Vllm`/`LMDeploy`/`HF`/`API` 等多种推理引擎，但是如果想使用各家模型 需要自己写一个对话模板 同时还要自己去选择调用那个推理引擎，但是大家常用习惯可能是 每一家的模型有一个自己的入口，然后可以通过入口配置用什么推理引擎，自动`dispatch`到对应的推理引擎，也不用自己配置各家模型的对话模版，因为在入口里已经已经配置好了

这个可能是一个相对比较大的任务 可以合作完成，如果想做的比较好也是一般标准的流程的 大概路线应该是

### 开发路线

设计文档(简单的包含一些调研)->一些开发者的评审->多个开发者开发 + 单元测试-> 最后是文档部分(使用教程，新增模型教程，设计方案)

如果是粗糙版的话，可以路线里做一些删减，

```
internlm_server = dict(type=LMDeployServer,
                       path=&#x27;internlm/internlm2_5-7b-chat&#x27;,
                       model_name=&#x27;internlm2&#x27;,
                       meta_template=INTERNLM2_META,
                       top_p=0.8,
                       top_k=1,
                       temperature=0,
                       max_new_tokens=8192,
                       repetition_penalty=1.02,
                       stop_words=[&#x27;&lt;|im_end|&gt;&#x27;])
```

现在如果是internlm 用deploy作为推理后端 是这样的 
理想情况 应该是

```
internlm_server = dict(type=Internlm2Chat,
                       backend=&#x27;lmdeploy&#x27;，
                       **backend_cfg
                       )
```

这种，对于使用者肯定是越简单越好，不需要过于关注怎么启动推理引擎，用什么对话模板之类的，

```
class LLMMixin:
    def chat_completion
    def completion


class Interlm2Chat(LLMMixin):
    def __init__(self, backend: Union[str, object]=&#x27;lmdeploy&#x27;, chat_template:str=&#x27;jinja2 模板&#x27;, **kwargs):
        self.model = dispatch(backend, chat_template, **kwargs)
    


class Qwen2Chat(LLMMixin):
    def __init__(self, backend: Union[str, object]=&#x27;lmdeploy&#x27;, chat_template:str=&#x27;jinja2 模板&#x27;, **kwargs):
        self.model = dispatch(backend, chat_template, **kwargs)
    





class LMDeployBackend(LLMMixin):
    def __init__(self, **kwargs):
        pass
        
    def chat_completion:
    def completion:
    
class TransformerBackend(LLMMixin):
    def __init__(self, **kwargs):
        pass
        
    def chat_completion:
    def completion:
    
class OllamaBackend
```

备注：大概研究员 2-3 天的工作量
