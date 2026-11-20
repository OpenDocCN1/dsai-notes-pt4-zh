
# LiteLLM-适合单机多模型快速落地

> 来源：[https://gotoaiworld.feishu.cn/docx/JavHdHhHFoYsn4xB42ccLLnbnFc](https://gotoaiworld.feishu.cn/docx/JavHdHhHFoYsn4xB42ccLLnbnFc)

## 主流开源 LLM API 网关方案对比（核心能力一览）

### LiteLLM（最适合单机多模型快速落地）

+ 项目地址：https://github.com/BerriAI/liteLLM

+ 核心能力：

  + 统一 OpenAI 接口封装 100 + 模型，支持 `vLLM`/`SD`/ 视频生成等多模态；
  + 代理模式支持进程级显存限制、空闲超时自动释放；
  + 故障转移、负载均衡、缓存 / 限流，适配单机显存优化场景。

+ 快速部署（Docker）：

  运行：

  ```bash
  docker run -p 4000:4000 berriai/liteLLM:latest --model vicuna-7b-v1.5 --gpu-memory-utilization 0.4
  ```

+ 优势：低代码接入，无缝集成 `vLLM` 等显存优化库，单机部署无依赖。
