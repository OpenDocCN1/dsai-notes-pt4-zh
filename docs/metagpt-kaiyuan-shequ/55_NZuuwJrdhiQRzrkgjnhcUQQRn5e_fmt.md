
# 文生图

> 来源：[https://deepwisdom.feishu.cn/docx/TbL5dYDqEo9nqhx7knkcScxPn6f](https://deepwisdom.feishu.cn/docx/TbL5dYDqEo9nqhx7knkcScxPn6f)

## 概述

`Text2Image`，指通过文字描述获取图片

## 示例：使用`Stable Diffusion`工具完成`Text2Image`

### 任务

使用`stable diffusion`的文生图工具，生成给定文本描述的图片

### 代码

```
python examples/di/sd_tool_usage.py
```

### 运行结果

在`metagpt`的`workspace/resources/SD_Output`产生一张512x768的图

![](img/6bf29834bb87ce366d62c528c2b5eadb.png)

## 机制解释

1.  将`metagpt/tools`下的既有代码`sd_engine.py`，进行工具标准化定义提供给`DataInterpreter`，工具定义放在`metagpt/tools/functions/schemas`下，见 `stable_diffusion.yml`
1.  `DataInterpreter`规划任务时，自动将中间任务类型设置为`stable_diffusion`，执行任务时将扫描是否有`stable_diffusion`相关工具可用。
1.  发现可用工具：`['SDEngine']`，加载工具代码，自动根据当前需求使用`SD T2I`代码进行出图。出图前进行`SD Prompt`的自动扩写。
