
# 增量开发

> 来源：[https://deepwisdom.feishu.cn/docx/I0YIdwLxioOyCSxfS0CcUAR1ntg](https://deepwisdom.feishu.cn/docx/I0YIdwLxioOyCSxfS0CcUAR1ntg)

您可以通过以下步骤改进现有项目代码：

+ 在计算机上的任意位置找到您想要改进的由 MetaGPT 创建的项目的文件夹
+ 执行 `metagpt <user-requirements-described-in-natural-language-about-new-idea-or-bug-feedback> --project-path <the-project-path-created-by-metagpt>`

## metagpt 选项

通过使用以下参数，可以连续迭代 MetaGPT 生成的现有项目。

| CLI 参数名称 | Value Type | 可选/必填 | Description | Usage |
| --- | --- | --- | --- | --- |
| `--project-path` | TEXT | Optional | 指定 `metagpt` 创建的旧版本项目的目录路径，以满足增量需求。 | `metagpt“BUG_FEEDBACK_XXX”--项目路径“YOUR_PROJECT_FULL_PATH” metagpt“INCRMENT_REQUIREMENTS”--项目路径“YOUR_PROJECT_FULL_PATH”` |
| `--project-name` | TEXT | Optional | 唯一的项目名称，例如 `"game_2048"`。 | `metagpt“NEW_REQUIREMENTS”--项目名称“YOUR_PROJECT_NAME”` |

有关更多命令选项，请检查 `metagpt --help` 。

## 用例1

以贪吃蛇为例，我们将演示如何增量地进行软件迭代。

### 创建一个新的贪吃蛇游戏项目。

我们使用以下命令参数使用 gpt-4-turbo 创建一个 Snake 软件项目。

```
metagpt "Write a cli snake game based on pygame" --project-name "snake_game" --run-tests --n-round 20 --max-auto-summarize-code 1
```

其中，相关参数的功能说明如下：

| CLI 参数名称 | Value | Description |
| --- | --- | --- |
| `--project-name` | `"snake_game"` | 新创建的软件项目的文件夹名称和项目名称是 `"snake_game"`。 |
| `--run-tests` | | 启用单元测试阶段。 |
| `--n-round` | `20` | 进行20轮。 |
| `--max-auto-summarize-code` | `1` | 修复“总结代码”中发现的问题最多允许 1 次。 |

### 向现有项目添加新需求。

我们使用Azure的GPT-4和以下参数来附加要求：

```
metagpt "Add a randomly appearing enemy that lasts for 5 seconds. If the enemy is eaten, the game ends. If the enemy is not eaten, it disappears after 5 seconds." --project-path "/Users/iorishinier/github/bak/MetaGPT/workspace/snake_game" --run-tests --n-round 20 --max-auto-summarize-code 1
```

其中，相关参数的功能说明如下：

| CLI 参数名称 | Value | Description |
| --- | --- | --- |
| `--project-path` | `"/Users/orishinier/github/bak/MetaGPT/workspace/snake_game"` | 现有项目的项目路径。 |
| `--run-tests` | | 启用单元测试阶段。 |
| `--n-round` | `20` | 进行20轮。 |
| `--max-auto-summarize-code` | `1` | 修复“总结代码”中发现的问题最多允许 1 次。 |

整个流程如下：最终LLM代码生成请求超时，但不会影响项目后续的增量迭代。由于新的需求已经在PRD中累积，重新运行或添加新的需求将基于当前的PRD。

### 修复错误

我们使用 Azure 的 GPT-4 和以下参数来修复错误。为了演示错误修复过程，我们直接将错误代码引入到生成的代码中以触发错误。随后，我们将bug信息作为需求内容的一部分提交，如下图：

```
metagpt "TypeError: draw() takes 1 positional argument but 2 were given" --project-path "/Users/iorishinier/github/bak/MetaGPT/workspace/snake_game" --run-tests --n-round 10 --max-auto-summarize-code 0
```

其中，相关参数的功能说明如下：

| CLI 参数名称 | Value | Description |
| --- | --- | --- |
| `--project-path` | `"/Users/orishinier/github/bak/MetaGPT/workspace/snake_game"` | 现有项目的项目路径。 |
| `--run-tests` | | 启用单元测试阶段。 |
| `--n-round` | `10` | 进行 10 轮。 |
| `--max-auto-summarize-code` | `0` | 不要修复“总结代码”中发现的问题。 |

### 尚未解决的问题

> Software Company 是一个实验室项目，旨在展示 MetaGPT 框架的功能，而不是解决生产中的实际问题。因此，MetaGPT 软件公司不支持输入以下性质的新要求：

1. 与架构相关的需求，例如 `setup.py` 或 `requirements.txt` 如何编写。
1. 指定特定函数应执行特定操作。

> 其原因如下：

1. 这种“要求”本质上是约束或指示。与真实需求不同，它们指定系统内某些模块的功能，而不是整个系统的功能。
1. MetaGPT Software Company 将软件开发过程简化为类视图和序列图的需求，然后直接翻译为源代码。诸如上述的系统内部模块的设计要求，由于缺乏明确的实现流程，因此被放弃。

### 编辑项目文件

您可以通过直接编辑软件公司生成的项目文件来添加新的需求。对相关文件进行必要的修改后，使用 `metagpt` 命令启动新一轮的增量迭代。以下是软件公司生成的文件夹结构的说明。项目迭代过程中使用的文件应放在 `docs` 目录下。

| 路径 | 是否可编辑 | 描述 |
| --- | --- | --- |
| `.dependencies.json` | No | 显式依赖关系文件 |
| `docs/requirement.txt` | No | 本次迭代新增需求文件 |
| `docs/bugfix.txt` | No | 本轮 bug 反馈信息文件 |
| `docs/prds` | Yes | 最终项目需求细分文件 |
| `docs/system_designs` | Yes | 项目的最终系统设计文件 |
| `docs/tasks` | Yes | 项目的编码任务文件 |
| `docs/code_summaries` | Yes | 整个代码库的审查结果文件 |
| `resources/competitive_analysis` | No | 竞争对手分析文件 |
| `resources/data_api_design` | No | 类视图文件 |
| `resources/seq_flow` | No | 序列视图文件 |
| `resources/system_design` | No | 系统设计文件 |
| `resources/prd` | No | PRD 文件 |
| `resources/api_spec_and_tasks` | No | 编码任务文件 |
| `tmp` | No | 项目处理过程中生成的中间文件 |
| `<workspace>` | Yes | 项目源代码目录 |
| `tests` | Yes | 单元测试代码 |
| `test_outputs` | No | 单元测试执行结果 |

## 用例2

以Flappy Bird为例

### 创建一个Flappy Bird游戏

使用 gpt-4-turbo 使用以下命令创建一个新的 flappybird Game 项目：

```
metagpt "write a flappy bird game without any obstacles" --project-name "flappy_bird" --run-tests --n-round 20 --max-auto-summarize-code 0
```
