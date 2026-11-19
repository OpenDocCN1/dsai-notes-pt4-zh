
# AFlow

> 来源：[https://deepwisdom.feishu.cn/docx/TCHod0KmRo5w32xfzhIcFIAVnHc](https://deepwisdom.feishu.cn/docx/TCHod0KmRo5w32xfzhIcFIAVnHc)

AFlow 是一个用于自动生成和优化代理工作流的框架。它在代码表示的工作流空间中使用蒙特卡洛树搜索来寻找有效的工作流，用机器的努力取代手动开发。我们的方法显示出在各种任务上超越手工制作工作流的潜力。

使用 Github 上的开源代码

在 arXiv 上阅读我们的论文

论文解读：[MetaGPT开源自动生成智能体工作流，4.55%成本超GPT-4o](https://deepwisdom.feishu.cn/wiki/EmfWwgEWSiFOplkZv4ScHIQOnud)

![](img/9a9bdbceef1b29eb1df078f91ca75b5c.png)

## 框架组件

+ **节点**：LLM调用的基本单位。查看 `metagpt/actions/action_node.py` 获取控制LLM、温度、格式和提示的灵活接口。

+ **算子**：预定义的节点组合，用于提高搜索效率。封装了常见操作如生成、格式化、审查、修订、集成、测试和程序员等。详见 `metagpt/ext/aflow/operator.py`。你可以参考此代码实现来自定义你自己的算子。

+ **工作流**：由边连接的LLM调用节点序列。可以表示为图、神经网络或代码以表达各种执行结构。查看 `metagpt/ext/aflow/workflow.py` 了解我们的实现。

+ **优化器**：在蒙特卡洛树搜索变体中使用LLM来探索和改进工作流。基于性能迭代地选择、扩展、评估和更新工作流。详见 `metagpt/ext/aflow/scripts/optimizer.py`。

+ **评估器**：评估工作流在给定任务上的性能。提供反馈以指导优化过程朝向更有效的工作流。详见 `metagpt/ext/aflow/scripts/evaluator.py`。

![](img/01057766ab3c42db3af84ac0d168ec21.png)

## 数据集

### 实验数据集

我们在六个数据集（HumanEval、MBPP、GSM8K、MATH、[HotpotQA](https://drive.google.com/uc?export=download&id=1DNoegtZiUhWtvkd2xoIuElmIi4ah7k8e)、DROP）上进行了实验，并提供了它们的评估代码。数据可以在这个数据集链接中找到，或者你可以使用 `metagpt/ext/aflow/data/download_data.py` 下载。

![](img/3bbbeeed1a5b044495186cec96a7acd4.png)

### 自定义数据集

对于自定义任务，你可以参考 `metagpt/ext/aflow/benchmark` 文件夹中的代码。继承 `BaseBenchmark` 类并实现 `evaluate_problem`、`calculate_score` 和 `get_result_columns` 来添加你的自定义数据集基准。然后，在 `metagpt/ext/aflow/scripts/evaluator.py` 和 `metagpt/ext/aflow/scripts/optimizer.py` 中添加你的基准名称，为你的自定义数据集找到有效的工作流。

## 快速开始

1. 配置优化参数：

   + 使用命令行参数或修改 `examples/aflow/optimize.py` 中的默认参数：

     ```
     --dataset              # (必需) 数据集类型 (HumanEval/MBPP/GSM8K/MATH/HotpotQA/DROP)
     --sample 4             # 样本数量 - 要重采样的工作流数量
     --optimized_path PATH  # 优化结果保存路径
     --initial_round 1      # 初始轮次
     --max_rounds 20        # AFLOW的最大迭代轮次
     --check_convergence    # 是否启用早停
     --validation_rounds 5  # AFLOW的验证轮次
     --if_first_optimize    # 首次优化设置为True，之后设置为False
     ```

1. 在 `config/config2.yaml` 中配置LLM参数（参考 `examples/aflow/config2.example.yaml`）

1. 在 `optimize.py` 和 `optimized_path/template/operator.py`、`optimized_path/template/operator.json` 中设置算子。你可以参考我们的实现为特定数据集添加算子

1. 首次使用时，通过在 `examples/aflow/optimize.py` 中设置 `download(["datasets", "initial_rounds"])` 下载数据集和初始轮次

1. （可选）按照自定义数据集部分添加你的自定义数据集和相应的评估函数

1. （可选）如果你想使用部分验证数据，可以在 `examples/aflow/evaluator.py` 中设置 `va_list`

1. 运行优化：

   ```
   # 使用默认参数
   python -m examples.aflow.optimize --dataset MATH

   # 或使用自定义参数
   python -m examples.aflow.optimize --dataset MATH --sample n --optimized_path xxx ...
   ```

## 复现论文中的结果

1. 我们在这个[链接](https://drive.google.com/uc?export=download&id=1Sr5wjgKf3bN8OC7G6cO3ynzJqD4w6_Dv)中提供了实验获得的原始数据，包括每次迭代生成的工作流和提示，以及它们在验证数据集上的轨迹。我们还提供了每个数据集的最优工作流及其在测试数据集上的相应数据。你可以使用 `metagpt/ext/aflow/data/download_data.py` 下载这些数据。

1. 你可以通过使用 `examples/aflow/optimize.py` 的不同 `ExperimentConfig` 直接复现我们的实验结果。

## 引用

如果你在研究中使用了 AFlow，请引用我们的论文：

```
@misc{zhang2024aflow,
      title={AFlow: Automating Agentic Workflow Generation},
      author={Jiayi Zhang and Jinyu Xiang and Zhaoyang Yu and Fengwei Teng and Xionghui Chen and Jiaqi Chen and Mingchen Zhuge and Xin Cheng and Sirui Hong and Jinlin Wang and Bingnan Zheng and Bang Liu and Yuyu Luo and Chenglin Wu},
      year={2024},
      eprint={2410.10762},
      archivePrefix={arXiv},
      primaryClass={cs.AI},
      url={https://arxiv.org/abs/2410.10762}, 
}
```
