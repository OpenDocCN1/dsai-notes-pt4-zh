
# Claude Code Skills实战：最新接口测试用例自动化生成教程

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Ss5yd3IvXo42HaxKyB8cPfctnOf](https://qqs7y1hozd1.feishu.cn/docx/Ss5yd3IvXo42HaxKyB8cPfctnOf)

文章详细介绍了Claude Code Skills在接口测试用例自动生成中的应用，对比了Skills、MCP与Subagent的区别，并通过ClaudeCodeSDK+Qwen3 Coder+Skills的demo展示了具体实现方法。通过定义不同协议的Skills，可实现自动调用、节省token并避免信息污染，为接口测试自动化提供了高效解决方案。

---

## 背景与技术对比

Claude Code Skills早在10月份就已经上线了，这个功能很多人应该很早就有了解，但是很多人一开始搞不清楚Skills、MCP、Subagent之间到底有啥区别。

如果有不太了解Claude Code和Skills的小伙伴，我这里就不赘述了，具体教程可以查看官方：

`https://code.claude.com/docs/zh-CN/skills`

其实本质上都类似，都是为了拓展通过Agent能力而生的，但是使用场景和侧重点有所不同。

MCP的设计初衷是为了拓展模型或通用Agent与其他系统的数据交换的能力，这个关键在于模型和外部世界拿数据，主体分析还是在主模型中，但是现在有很多`mcp` server已经成了披着“协议”外衣的Agent杂烩。

Subagent（子代理）则是为了避免主AI进程中的上下文过长，把专门进行某个领域工作的思考和分析单拎出来，不占用主进程的上下文，主进程则来管理各个子代理的数据交换。

Skills（技能）则主要是为了管理prompt，核心思想是"prompt渐进式披露"，减少上下文token和污染，比如，你扔给模型一个任务，模型自己判断什么时候用哪个Skill（这个Skill中放着prompt、数据、脚本等资源），这个prompt是分级加载的，而不是一股脑全给模型，否则会导致上下文污染，prompt也可以被更加灵活的运用（这里面就是Claude Code做的巧妙机制）。

![](img/e1f94f1ba25c04801ca2d6ac646d9e07.png)

## 接口测试用例自动化生成Demo

现在我们就拿接口测试用例生成这个案例来展示。

项目接口比较复杂有可能为自己封装的C++、Http、gRPC等协议接口，所以这个生成工作流需要支持的生成模板比较多，同事也要考虑混合多接口的业务流程。

在demo中，我采用了`ClaudeCodeSDK`+`Qwen3 Coder`+`Skills`的方式（如果你们没有本地模型，也可以用`modelspace`中的模型，只是有次数限制，不过要注意，`ClaudeCode`需要接入有工具调用能力的模型，而且在国内需要用`Proxy`），原始输入有接口文档、`DLL`/头文件、接口生成规范、数据 四个类型的资源。

整体demo结构为：

```
claude_code_generate_test_gtest/
├── API_REFERENCE_CPP.md
├── main.py
├── p/
│   ├── bin/
│   ├── include/
│   └── lib/
├── tests/
│   ├── CMakeLists.txt   # 编译脚本CMakeLists.txt文件（也是Claude Code生成）
│   ├── main.cpp
│   ├── NodeTest.cpp   # 接口用例
│   └── build/   # 编译产物
├── .claude/     # 项目级别skill配置（只有在当前路径下启动claude code可以被发现）
└── skills/
    └── generate_api_cpp_test_scripts/
        ├── SKILL.md
        └── REFERENCE.md
```

除了这种`C++`项目，其他协议类型的接口也可以是一样，区别就是在项目路径下定义不同的`skill`，在一个`skill`中`SKILL.md`是这个技能的主要`prompt`，包含`name`、`description`、`prompt`细节，`REFERENCE.md`则是可能依赖的其他资源，在这里就是生成`C++ gtest`测试脚本的详细规范说明（格式、语法、变量命名、颗粒度等等）。

当然其他协议`http`等也是一样的道理，大家有兴趣可以尝试以下效果。

### SKILL.md

```markdown
---
name: generate-api-test-scripts
description: 根据用户提供的C++接口文档，生成C++ gtest接口测试用例。在需要生成C++项目的接口测试用例时使用
---

# Generate API Test Scripts

## Instructions

查看用户提供的接口文档（`api_path`）和源码头文件和DLL路径（`src_path`），查看并仔细阅读接口文档，重点查阅以下章节内容：

* [项目信息]
* [详细接口说明]
* [类型定义与枚举]

生成C++ gtest，接口测试用例脚本

## Rules

1. 在用户提供的输出路径（`ouput_path`）下，构建脚本和`CMakeList`，保证所有脚本均可以正常编译
2. 请详细根据[详细接口说明]和[错误处理]，依据正常工作流、错误工作流、异常工作流、边界值、多接口等方式进行接口用例脚本生成
3. 必须保证所有已发布接口都生成了测试脚本，不得有任何遗漏
4. 请仔细检查生成的测试脚本，严格遵循C++和gtest语法，不可以存在语法错误，查看脚本编写规范，查看 [`REFERENCE.md`](.\REFERENCE.md)
5. 脚本编写规范，需要严格遵循并查看：[`REFERENCE.md`](.\REFERENCE.md).
```

### main.py

```python
import asyncio
from claude_agent_sdk import query, ClaudeSDKClient, ClaudeAgentOptions, AssistantMessage, TextBlock, ResultMessage

async def main_ClaudeSDKClient_query(user_prompt: str):
    async with ClaudeSDKClient(options=options) as client:
        await client.query(user_prompt)
        async for message in client.receive_response():
            if isinstance(message, AssistantMessage):
                for block in message.content:
                    if isinstance(block, TextBlock):
                        print(f"Claude: {block.text}")

if __name__ == "__main__":
    user_prompt = f"""
项目源码头文件和Dll目录src_path:{code_path}，接口文档目录api_path:{api_md_path}，输出路径：{ouput_path}, 请根据接口文档内容扩展NodeTest对应的接口测试用例脚本
请保留当前tests下的CMakeLists内容，确保生成的测试脚本 cpp文件不得有任何语法错误遵循c++和gtest语法规范，如果要调用skill则需要告知
"""
    asyncio.run(main_ClaudeSDKClient_query(user_prompt))
```

最终的执行结果

![](img/8300f3d88f7c300ed20b8aec6284d159.png)

当然`gtest`输出的报告，还可以接入`allure`，进行测试报告展示。

通过Skills实现不同协议接口用例自动生成，无需多agent，只需要定义对应的Skill生成prompt，CC会自动选择调用，且不占用全量上下文，节省token，避免信息污染。
