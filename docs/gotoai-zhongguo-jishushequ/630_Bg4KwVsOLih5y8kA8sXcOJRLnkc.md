# FastAPI-

> 来源：[https://gotoaiworld.feishu.cn/docx/Ah9odl5GNoqzBUxeAP3cgPAqnjh](https://gotoaiworld.feishu.cn/docx/Ah9odl5GNoqzBUxeAP3cgPAqnjh)

FastAPI framework, high performance, easy to learn, fast to code, ready for production

fastapi.tiangolo.com/

https://github.com/fastapi/fastapi

FastAPI 是一款**基于 Python 3.8+** 的高性能、易上手的 Web 框架，专为构建 API 设计（尤其适合机器学习 / AI 模型推理服务），由 Sebas**tián **R**amíre**z 开发，核心基于 Starlette（异步 Web 框架）和 Pydantic（数据校验），兼具异步高性能与强类型校验的特点，是当前 Python 生态中构建推理 API 最主流的选择之一。

### 核心特点（适配你的多模型部署场景）

### 与你的场景结合：FastAPI 典型用法（多模态模型接口）

以你需要的「文本 + 图片生成」为例，FastAPI 可快速封装模型为标准化 API，且天然支持「进程隔离 + 显存释放」：

python

运行

```
from fastapi import FastAPI
from pydantic import BaseModel  # 数据校验import torch
from vllm import LLM  # 文本生成from diffusers import StableDiffusionPipeline  # 图片生成# 1. 初始化 FastAPI 应用
app = FastAPI(title=&quot;多模态模型推理API&quot;, version=&quot;1.0&quot;)# 2. 定义请求参数格式（强类型校验）class TextInferRequest(BaseModel):
    prompt: str
    max_tokens: int = 200class ImageInferRequest(BaseModel):
    prompt: str
    width: int = 512
    height: int = 512# 3. 加载模型（启动时加载，按需占用显存）# 文本生成模型（vLLM 优化）
llm = LLM(model=&quot;vicuna-7b-v1.5&quot;, gpu_memory_utilization=0.4)# 图片生成模型（SD + 显存优化）
sd_pipe = StableDiffusionPipeline.from_pretrained(&quot;runwayml/stable-diffusion-v1-5&quot;,
    torch_dtype=torch.float16,
    use_xformers=True).to(&quot;cuda:0&quot;)
sd_pipe.enable_attention_slicing()# 4. 定义 API 接口@app.post(&quot;/infer/llm&quot;)  # 文本生成接口def infer_llm(request: TextInferRequest):&quot;&quot;&quot;LLM 文本生成接口&quot;&quot;&quot;with torch.no_grad():
        outputs = llm.generate(request.prompt, max_tokens=request.max_tokens)return {&quot;result&quot;: outputs[0].outputs[0].text}@app.post(&quot;/infer/sd&quot;)  # 图片生成接口def infer_sd(request: ImageInferRequest):&quot;&quot;&quot;Stable Diffusion 图片生成接口&quot;&quot;&quot;with torch.no_grad():
        image = sd_pipe(
            prompt=request.prompt,
            width=request.width,
            height=request.height
        ).images[0]# 转换为 Base64 返回（前端可直接展示）return {&quot;image_base64&quot;: image_to_base64(image)}# 5. 释放显存接口（主动释放模型）@app.post(&quot;/release/{model_type}&quot;)def release_model(model_type: str):&quot;&quot;&quot;手动释放指定模型的显存&quot;&quot;&quot;global llm, sd_pipe
    if model_type == &quot;llm&quot;:del llm
    elif model_type == &quot;sd&quot;:del sd_pipe
    torch.cuda.empty_cache()return {&quot;status&quot;: f&quot;{model_type} 显存已释放&quot;}# 启动服务（终端执行：uvicorn main:app --host 0.0.0.0 --port 8000）if __name__ == &quot;__main__&quot;:import uvicorn
    uvicorn.run(app, host=&quot;0.0.0.0&quot;, port=8000)
```

### 部署与运行

1.  **安装依赖**：pip install fastapi uvicorn pydantic torch vllm diffusers xformers

1.  **启动服务**：uvicorn main:app --host 0.0.0.0 --port 8000 --workers 1（--workers 1 避免多进程显存冲突）

1.  **调试接口**：访问 http://localhost:8000/docs，可直接在线测试文本 / 图片生成接口，或调用 /release 接口释放显存。

### FastAPI 与其他工具的关系（对你的场景）

1.  **与 vLLM/SD 结合**：FastAPI 是「外壳」，负责接收 HTTP 请求、返回结果；vLLM/SD 是「内核」，负责模型推理，两者结合实现「模型服务化」；

1.  与 Ray Serve/Modal 结合：Ray Serve/Modal 可将 FastAPI 封装的服务进一步管理（如空闲释放、显存调度），FastAPI 负责接口层，后者负责资源层；

1.  与 LiteLLM/One API 结合：LiteLLM/One API 是「统一网关」，可路由请求到多个 FastAPI 封装的模型服务（如 LLM 服务、SD 服务）。

### 核心总结

**Fast**API 是你单机部署多模态模型的基础工具—— 它能快速将文本 / 图片 / 视频生成模型封装为标准化 HTTP API，支持高并发、参数校验、显存释放接口设计，且轻量级、易部署，是前文方案中「模型进程 + API 网关」的核心组件。

![](img/ddae7905f905e96030c38636ce87c59e.png)

FastAPI framework, high performance, easy to learn, fast to code, ready for production

[ht](https://github.com/fastapi/fastapi/actions/workflows/test.yml/badge.svg?event=push&branch=master)tps://github.com/fastapi/fastapi/actions/workflows/test.yml/badge.svg?event=push&branch=master

[ ](https://github.com/fastapi/fastapi/actions?query=workflow%3ATest+event%3Apush+branch%3Amaster)

[https:](https://camo.githubusercontent.com/417c9fb0bf086f6618004816890b222c8b5c1d7922ea9b0b85b95b4c15fe9c4e/68747470733a2f2f636f7665726167652d62616467652e73616d75656c636f6c76696e2e776f726b6572732e6465762f666173746170692f666173746170692e737667)//camo.githubusercontent.com/417c9fb0bf086f6618004816890b222c8b5c1d7922ea9b0b85b95b4c15fe9c4e/68747470733a2f2f636f7665726167652d62616467652e73616d75656c636f6c76696e2e776f726b6572732e6465762f666173746170692f666173746170692e737667

Documentation: https://fastapi.tiangolo.com

Source Code: https://github.com/fastapi/fastapi

---

FastAPI is a modern, fast (high-performance), web framework for building APIs with Python based on standard Python type hints.

The key features are:

+   Fa[st: Very high performance,](https://github.com/fastapi/fastapi#performance) on par with NodeJS and Go (thanks to Starlette and Pydantic). One of the fastest Python frameworks available.

+   Fast to code: Increase the speed to develop features by about 200% to 300%. *

+   Fewer bugs: Reduce about 40% of human (developer) induced errors. *

+   Intuitive: Great editor support. Completion everywhere. Less time debugging.

+   Easy: Designed to be easy to use and learn. Less time reading docs.

+   Short: Minimize code duplication. Multiple features from each parameter declaration. Fewer bugs.

+   Robust: Get production-ready code. With automatic interactive documentation.

+   Standards-based: Based on (and fully compat[ible wi](https://github.com/OAI/OpenAPI-Specification)[th) the ope](https://json-schema.org/)n standards for APIs: OpenAPI (previously known as Swagger) and JSON Schema.

+   estimation based on tests conducted by an internal development team, building production applications.

## Sponsors

### Keystone Sponsor

### Gold and Silver Sponsors

### [Other sponsors](https://fastapi.tiangolo.com/fastapi-people/#sponsors)

## Opinions

"[...] I'm using FastAPI a ton these days. [...] I'm actually planning to use it for all of my team's ML services at Microsoft. Some of them are getting integrated into the core Windows product and some Office products."

[Kabir](https://github.com/fastapi/fastapi/pull/26) Khan - Microsoft (ref)

---

"We adopted the FastAPI library to spawn a REST server that can be queried to obtain predictions. [for Ludwig]"

P[iero ](https://eng.uber.com/ludwig-v0-2/)Molino, Yaroslav Dudin, and Sai Sumanth Miryala - Uber (ref)

---

"Netflix is pleased to announce the open-source release of our crisis management orchestration framework: Dispatch! [built with FastAPI]"

K[evin ](https://netflixtechblog.com/introducing-dispatch-da4b8a2a8072)Glisson, Marc Vilanova, Forest Monsen - Netflix (ref)

---

"I’m over the moon excited about FastAPI. It’s so fun!"

Brian Okken - [Python Bytes](https://pythonbytes.fm/episodes/show/123/time-to-right-the-py-wrongs?time_in_sec=855) podcast host [(ref)](https://x.com/brianokken/status/1112220079972728832)

---

"Honestly, what you've built looks super solid and polished. In many ways, it's what I wanted Hug to be - it's really inspiring to see someone build that."

[Tim](https://github.com/hugapi/hug)othy Cros[ley -](https://news.ycombinator.com/item?id=19455465) Hug creator (ref)

---

"If you're looking to learn one modern framework for building REST APIs, check out FastAPI [...] It's fast, easy to use and easy to learn [...]"

"We've switched over to FastAPI for our APIs [...] I think you'll like it [...]"

[Ines Montani](https://explosion.ai/) - Matthew H[onnib](https://spacy.io/)al - Explo[sion ](https://x.com/_inesmontani/status/1144173225322143744)AI [found](https://x.com/honnibal/status/1144031421859655680)ers - spaCy creators (ref) - (ref)

---

"If anyone is looking to build a production Python API, I would highly recommend FastAPI. It is beautifully designed, simple to use and highly scalable, it has become a key component in our API first development strategy and is driving many automations and services such as our Virtual TAC Engineer."

[Deon ](https://www.linkedin.com/posts/deonpillsbury_cisco-cx-python-activity-6963242628536487936-trAp/)Pillsbury - Cisco (ref)

---

## Typer, the FastAPI of CLIs