
# Modelscope Agent-轻量级单机多模型推理框架（开箱即用）

> 来源：[https://gotoaiworld.feishu.cn/docx/U55PdIe74ouUaexxNRicEh6snAc](https://gotoaiworld.feishu.cn/docx/U55PdIe74ouUaexxNRicEh6snAc)

「具备基本的显存动态分配、自动释放、多模型共享」的核心逻辑，用户只需：

1. 配置模型路径 / 显存限制；
1. 启动服务 / 加载模型；
1. 关闭模型 / 服务时，项目自动释放显存（无需手动调用`empty_cache()`等）。

## modelscope-agent（阿里通义千问开源）

- **项目地址**：[https://github.com/modelscope/modelscope-agent](https://github.com/modelscope/modelscope-agent)
- **核心能力**：
  - 一键加载多模型（支持 LLM / 视觉 / 语音），自动管理 GPU 显存；
  - 内置显存优化（量化 / 按需加载），模型卸载时自动释放显存；
  - 支持多模型并行推理，显存不足时自动降级到 CPU；
- **简化点**：无需手动写显存释放逻辑，一行代码加载 / 卸载模型：

```python
from modelscope_agent.agents import FunctionCallAgent

# 初始化Agent（自动管理GPU显存）
agent = FunctionCallAgent(model='qwen-7b-chat', device='cuda:0')
# 调用模型推理
agent.run("帮我生成一段文案")
# 卸载模型（自动释放显存）
agent.release()
```
