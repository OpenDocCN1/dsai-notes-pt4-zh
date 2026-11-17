
# LangChain百万代码全解析：这个模型胃口很大！

> 来源：[https://agijuejin.feishu.cn/docx/ZmL5de3cjo3jDax63fJcD9qynNh](https://agijuejin.feishu.cn/docx/ZmL5de3cjo3jDax63fJcD9qynNh)

> 作者：AI小智

> 原文：[https://mp.weixin.qq.com/s/DdtqDJ76We6WLgFn8o9ztA](https://mp.weixin.qq.com/s/DdtqDJ76We6WLgFn8o9ztA)

![](img/a7ba5f8b5f387998a8e6564cec410019.png)

你好呀，我是小智!

熟悉我的小伙伴们，一定看过我之前介绍Agent如何实现百万上下文RAG的那篇文章。不得不说AI发展真的是日新月异，这才没过多久，模型本身的上下文能力也来到了百万级别。

小智也是在苦思如何解决大模型代码库解析跨文件关联问题时，无意中在智谱AI的大模型开放平台（bigmodel.cn）上发现了支持1M上下文（约150-200万汉字）的GLM-4-Long。更没想到的是，模型一换，瞬间就解决了我好几天工程化都未能解决的麻烦！今天，就让我带大家一起看看，GLM-4-Long能力几何吧！

本文所有示例代码已上传github，有兴趣的小伙伴可以到文末自取。

## 小试牛刀，很懂《三体》恋爱脑云天明！

在谈论AI的代码解析能力之前，我们不妨先看一下它在处理自然语言文本方面的表现。为了测试GLM-4-Long的理解力，我决定让它尝试阅读并理解我最爱的科幻小说-94万字的《三体》。

### 抽取概括能力

作为一个具有强大理解能力的模型，必须具备从复杂的文本中抽取出关键的信息，并进行总结。当一次性输入整本小说，模型能有怎样的表现呢？

我决定以《三体》中的恋爱脑云天明为例，我们可以让模型梳理出云天明的个人故事，并按照时间线进行概括。

![](img/1a3e4d4789f9eeaf620f7a0e449faf4b.png)

结果分析：

+ 十分准确的，提取到了（危机纪元，广播纪元，银河纪元）三个阶段，并严格遵守了指令时间线梳理的要求
+ 正确梳理出来了云天明的个人故事时间线，标注出了关键事件节点。
+ 正确挖掘到了关键关联人物-程心，并找到了核心的关联事件
+ 对云天明总结也十分不错，虽然我对他的映像只有恋爱脑

![](img/e94557fc3dc2cf08ac54c43a2afa3224.png)

### 上下文串联与推理能力

除了信息抽取，一个优秀的长文本大模型还需要具备强大的上下文串联与推理能力。假设我们提出一个极端的假设：如果僵尸吃掉了云天明的脑子，三体的故事将会怎么发展？通过这样的假设，模型可以根据已有的故事情节推测出后续的发展方向。

+ 假如僵尸吃掉了云天明的脑子，三体的故事将会怎么发展？

![](img/d49de7214446ce311875be0545aec24e.png)

结果分析：不得不说，相比时间抽取任务上的精彩表现，在通过长文本基础上进一步推理思考的任务中，模型表现得则没有那么惊艳。

+ 没有脱离大的故事框架，但把对宇宙的思考局限在了地球和三体上，这想象力完全不及大刘
+ 针对地球和三体世界的关系和交流方式上，存在一定误解，云天明并不是交流工具！
+ 总体来说，在如此长上下文的基础上，还具备基本的推理想象力，是值得进一步使用的模型

## 不仅能读小说，GLM-4-Long解析代码项目能力几何

用模型读小说，只是为了一探模型能力的究竟。真正能验证模型能力的，还得是真实的生产任务。接下来整活结束，让小智带领大家将GLM-4-Long用到代码开发中！

考虑到代码保密的要求，接下来小智将用LangChain代码用作示例演示。LangChain官网的AI助手是基于RAG实现的，现在既然我们拥有了超强上下文的能力，不如我们自己做一个。

### 准备工作，LangChain-Core代码一网打尽

RAG（检索增强生成）技术虽然强大，但在某些复杂的代码解析任务中，官方提供的LangChain助手并不能完全满足需求。因此，我们决定自己动手，利用GLM-4-Long打包整个项目的代码，并进行深度解析。

首先我们需要将`langchain-core`代码整体打包到一个文本文档中，并标注好文件目录

```
import os

def integrate_python_files(directory, output_file, identifier='---'):
    # 确保输出文件是空的
    open(output_file, 'w', encoding='utf-8').close()
    
    # 遍历目录
    for root, dirs, files in os.walk(directory):
        for file in files:
            if file.endswith('.py'):
                # 构建文件的完整路径
                full_path = os.path.join(root, file)
                # 获取文件相对于主目录的相对路径
                relative_path = os.path.relpath(full_path, directory)
                # 读取文件内容
                with open(full_path, 'r', encoding='utf-8') as f:
                    file_content = f.read()
                
                # 写入输出文件
                with open(output_file, 'a', encoding='utf-8') as out_f:
                    # 添加标识符和文件的相对路径
                    out_f.write(f"{identifier} {relative_path}\n")
                    out_f.write(file_content)
                    out_f.write("\n")  # 添加一个空行作为分隔

# 调用函数
directory_path = 'langchain/libs/core/langchain_core'  # 替换为你的目录路径
output_file_path = 'integrated_code.txt'  # 替换为你的输出文件路径
integrate_python_files(directory_path, output_file_path, identifier='=======')
```

### GLM-4-Long 轻松帮你打造 LangChain AI 助手

对于一个完整的项目，GLM-4-Long在没有额外文档的情况下，是否能准确理解并解析项目的功能和组件呢？我们将其应用于一个实际的代码项目，看看它能给出怎样的分析。

问题1、项目主要做了什么？实现了什么功能?

通过将项目代码输入GLM-4-Long，我们希望模型能够给出项目的整体概览，包括其主要功能和实现的目标。

![](img/30b2893f8292945bb325a2d6f0f14d20.png)

结果分析：

+ 模型在没有文档情况下，只基于代码正确总结了LangChain的主要功能、主要组件、使用场景
+ 对于LangChain核心设计思想，链式编程等做了十分完善且正确的总结
+ 相关的使用场景的列举，也没有超出LangChain框架的范畴，并没有出现幻觉

问题2、概括LangChain核心组件和概念

LangChain是一个用于构建语言模型应用的框架，包含多个核心组件。我们让GLM-4-Long对这些组件进行概括，并解释它们在项目中的作用。

![](img/53a6f4efac14e25b94016cb515775115.png)

结果分析：

+ 模型一口气提取了32项LangChain对LLM开发核心概念的抽象
+ 未出现不存在的组件，没有出现幻觉
+ 针对组件的说明正确无误

问题3、请以代码方式输出经典的LCEL使用案例

在理解了项目和组件之后，我们进一步要求GLM-4-Long展示一个经典的LCEL（LangChain Expression Language）使用案例，以便读者更好地理解LangChain的应用场景。

![](img/48dcc7df6a635fada0abe0a1e5abee8c.png)

结果分析：

+ 代码完整正确，且能正常运行，学过LangChain的小伙伴对这个demo肯定倍感亲切
+ 配套的案例说明，也能较好的说明LCEL语法概念及使用方式

问题4、上强度，通过Multi Query构建一个RAG案例，要求合并输出文档里的连续片段

在常规的解析任务之外，我们让GLM-4-Long处理一个高难度任务：通过Multi Query（多查询）构建一个RAG案例，并合并输出文档中的连续片段。这不仅测试了模型的理解能力，还考验了它的合成与输出能力。

![](img/79b6e59ec33842645baa69512bb69f43.png)

结果分析：

+ 模型正确分析了用户的需求，并按需求正确输出了执行步骤分析
+ 基本正确的输出了执行RAG逻辑的相关代码，包括向量存储等
+ 没能正确提供加载RAG Chain的逻辑，而是直接幻觉出了加载RAG模型

虽然没能正确完成高阶的代码开发案例，但在如此简单的提示下，模型的表现仍然不错。如果真的完成了需求，可能广大程序员朋友们真的得担心失业问题了。😄

## 总结

通过对《三体》的分析，我们可以看到GLM-4-Long在处理超长文本时，展现出卓越的信息抽取、上下文串联与推理能力。无论是概括复杂的故事情节，还是推理出假设情境下的故事发展，GLM-4-Long都表现得非常出色。

更为重要的是，GLM-4-Long在代码解析方面的能力令人印象深刻。它不仅能够理解项目代码的整体结构和功能，还能深入到具体的组件和应用案例中，提供详尽的分析与建议。这使得开发者在面对复杂的代码库时，可以更为高效地理解和改进代码。

回到文章主题，我想有了GLM-4-Long，我再也不怕接手同事的💩山代码了🤣。

本文代码示例：https://github.com/q2wxec/langgraph-demo/tree/master/glm4long

接口文档：https://bigmodel.cn/dev/api#glm-4

体验中心：https://bigmodel.cn/console/trialcenter
