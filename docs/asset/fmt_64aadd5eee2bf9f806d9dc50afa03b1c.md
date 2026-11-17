
## 定义、提交与监控命令作业

1.  定义命令作业（Define the Command Job）：这是流程的核心。不再使用旧版的 `ScriptRunConfig`，而是实例化一个 `command` 对象来清晰地定义作业的所有组成部分：

+   **代码（Code）：** 指定包含训练脚本的本地文件夹路径。
+   **命令（Command）：** 定义在云端计算节点上需要执行的确切终端命令，包括如何调用 Python 脚本以及如何传递参数。
+   **输入（Inputs）：** 以键值对的形式，将云端的数据资产（如 `diabetes` 数据集）映射到命令中的具名参数。
+   **计算资源（Compute）：** 指定执行此命令的计算目标（例如 `aml-cluster`）。
+   环境（Environment）：指定运行此命令所需的软件环境（例如，一个包含 `scikit-learn` 和 `pandas` 的 Docker 镜像或 Conda 环境）。

2.  提交作业（Submit the Job）：使用 `ml_client.jobs.create_or_update()` 方法将这个完整定义的 `command` 作业对象提交到 Azure ML。

3.  监控和审查结果（Monitor and Review）：提交后，你会得到一个指向 Studio 页面的链接，可以实时监控作业的运行日志和状态。作业完成后，其输出（如训练好的模型）会被保存在 Azure ML 中。

### 关键知识点介绍

+   Azure ML SDK v2：这是 Azure ML Python SDK 的最新版本。它的设计更加面向对象和声明式，核心理念是将一切操作都视为对“资产”（`Assets`）或“作业”（`Jobs`）的管理。

+   `MLClient`：SDK v2 中与 Azure ML 工作区进行交互的主要客户端。所有操作，如创建计算资源、注册数据集、提交作业等，都通过这个客户端的方法来完成。

+   **作业（`Job`）：** 在 SDK v2 中，“作业”是执行一次操作的统一规范。它取代了旧版 SDK 中的“运行”（`Run`）和各种配置对象。作业可以是训练模型的脚本、数据处理的流程，或是任何你希望在 Azure ML 计算资源上执行的任务。

+   命令作业（`Command Job`）：最基础也是最常用的一种作业类型。它封装了执行一个命令行脚本所需的所有信息：要运行的代码、要执行的命令、所需的输入数据、运行的环境以及使用的计算资源。这种封装方式使得整个任务非常独立和可移植。

+   输入与输出（`Inputs & Outputs`）：SDK v2 强调对作业的输入和输出进行显式定义。你可以将一个 Azure ML 数据资产（`azureml:<dataset_name>:<version>`）作为输入，并将其映射到脚本的命令行参数上。同样，你也可以定义输出，以便将脚本产生的文件（如模型）保存回 Azure ML。

+   YAML 定义：虽然本实验主要在 Python 中定义作业，但 SDK v2 的一个强大之处在于，所有作业和资产都可以用 YAML 文件来定义。这实现了配置与代码的彻底分离，非常适合 MLOps 场景，便于版本控制和 CI/CD 集成。

---

## 实验核心流程分析（基于 `Labs/08/Run script as a command job.ipynb` 代码）

接下来，我们深入分析该 Notebook 中的核心代码片段。

```
from azure.ai.ml import command
from azure.ai.ml import Input

# Define the command job
job = command(
    code="./src",  # location of source code
    command="python main.py --data ${{inputs.diabetes}} --reg_rate ${{inputs.reg_rate}}",
    inputs={
        "diabetes": Input(
            type="uri_file",
            path="azureml:diabetes-folder/diabetes.csv"
        ),
        "reg_rate": 0.01
    },
    compute="aml-cluster",
    environment="AzureML-sklearn-1.0-ubuntu20.04-py38-cpu@latest",
    display_name="diabetes-train-sdk-v2",
    experiment_name="diabetes-training"
)

# submit the command
returned_job = ml_client.jobs.create_or_update(job)
```

### 核心步骤分解

1.  `from azure.ai.ml import command, Input`：导入核心组件

这行代码导入了定义一个命令作业和其输入所需的核心类。

2.  `job = command(...)`：实例化并配置命令作业

这是整个流程的核心。它创建了一个 `command` 对象，并以一种非常清晰、结构化的方式定义了作业的每一个方面：

+   `code="./src"`：**代码在哪里？** 指定包含我们训练脚本（`main.py`）的本地文件夹是 `./src`。在提交时，这个文件夹的内容会被自动上传。

+   `command="python main.py --data ${{inputs.diabetes}} --reg_rate ${{inputs.reg_rate}}"`：要执行什么命令？ 这定义了作业启动后，在计算节点上要执行的确切终端命令。

+   `python main.py`：调用我们的训练脚本。

+   `--data ${{inputs.diabetes}}`：这是一个关键的表达式绑定。它告诉 Azure ML：

+   创建一个名为 `--data` 的命令行参数。

+   这个参数的值应该被替换为下面 `inputs` 字典中 `diabetes` 这个键所指向的实际路径。`${{...}}` 是 v2 SDK 中用于引用的标准语法。

+   `--reg_rate ${{inputs.reg_rate}}`：同样，将 `--reg_rate` 参数与 `inputs` 字典中 `reg_rate` 的值绑定。

+   `inputs={...}`：**命令的输入是什么？** 这个字典定义了 `command` 字符串中引用的所有输入。

+   `"diabetes": Input(...)`：定义一个名为 `diabetes` 的输入。

+   `type="uri_file"`：指定输入类型是一个指向文件的 URI。

+   `path="azureml:diabetes-folder/diabetes.csv"`：**指定输入的来源。** 这是一个标准的资产 ID，指向名为 `diabetes-folder` 的数据资产的 `diabetes.csv` 文件。这实现了数据与代码的解耦。

+   `"reg_rate": 0.01`：定义一个名为 `reg_rate` 的输入，它的值是一个字面量（`literal`）`0.01`。这使得超参数的配置非常清晰。

+   `compute="aml-cluster"`：在哪里运行？ 指定使用名为 `aml-cluster` 的计算集群。

+   `environment="AzureML-sklearn-1.0-ubuntu20.04-py38-cpu@latest"`：在什么软件环境中运行？ 这里直接引用了一个由 Azure ML 官方维护的“策展环境”（Curated Environment）。这种方式非常方便，因为它免去了你手动定义所有库依赖的麻烦。

+   `display_name="..."` & `experiment_name="..."`：如何组织和展示？ 为这次运行指定一个易于辨认的显示名称，并将其归类到 `diabetes-training` 这个实验中。

3.  `returned_job = ml_client.jobs.create_or_update(job)`：提交作业

最后，通过 `MLClient` 的 `.jobs.create_or_update()` 方法，将这个完整配置好的 `job` 对象提交到 Azure ML 服务。服务会解析这个对象的全部配置，准备好计算资源、环境和数据，然后执行你指定的命令。这个方法会返回一个代表已提交作业的对象，你可以用它来监控作业的状态。

## Use MLflow to track training jobs

https://github.com/MicrosoftLearning/mslearn-azure-ml/blob/main/Instructions/08-Script-mlflow-tracking.md

### 实验特点与知识点分析（基于 `Instructions/08-Script-mlflow-tracking.md`）

#### 核心目标

这个实验的核心目标是让你学会如何将 `MLflow` 强大的跟踪能力深度集成到标准化的 SDK v2 命令作业中。你将从之前手动编写 MLflow 记录代码，升级为利用 MLflow 的自动化日志记录（`autologging`）功能。最终目标是创建一个“自描述”、“自记录”的训练作业，它在执行时能自动捕获关键信息，极大地提升实验的可追溯性和效率。

#### 流程分析

这个实验在 Jupyter Notebook 中完成，其流程展示了如何将 MLflow 无缝嵌入到现代化的作业提交过程中：

1.  连接工作区（Connect to Workspace）：使用 `MLClient` 与 Azure ML 工作区建立连接，这是所有后续操作的起点。

2.  准备数据与计算资源（Prepare Data & Compute）：确保实验所需的数据资产和计算集群已经就绪。

3.  创建带有自动化日志的训练脚本（Create Script with Autologging）：这是流程中的一个关键演进。你会编写一个新的训练脚本（`main.py`），其核心变化是在训练逻辑开始前，仅仅添加一行 `mlflow.autolog()`。

4.  定义并提交命令作业（Define and Submit Command Job）：像上一个实验一样，使用 `command` 对象来封装作业的定义。值得注意的是，作业的定义方式与不使用 MLflow 时几乎完全一样。这体现了 MLflow 集成的非侵入性。

5.  提交作业（Submit the Job）：使用 `ml_client.jobs.create_or_update()` 提交作业。Azure ML 后端会自动识别脚本中的 MLflow 指令，并将所有日志信息捕获到本次作业的运行记录中。

6.  审查自动记录的结果（Review Autologged Results）：作业完成后，核心的验证步骤是在 Azure ML Studio 中打开这次运行的详情页。你会发现，即使脚本中没有写一行 `mlflow.log_metric` 或 `mlflow.log_param`，运行的“参数”、“指标”和“产物”标签页下已经自动填充了所有相关的实验信息。

#### 关键知识点介绍

+   MLflow 自动日志记录（`autologging`）：这是本次实验的核心技术点。`mlflow.autolog()` 是一个强大的“一键启动”功能。当你调用它时，MLflow 会自动检测你代码中使用的常见机器学习库（如 `scikit-learn`、`TensorFlow`、`PyTorch`、`LightGBM` 等）。一旦检测到，它会在这些库的关键函数（如 `model.fit()`）执行前后，自动地记录下所有相关信息，包括：

+   超参数（`Parameters`）：模型的配置参数（例如，逻辑回归的 `C` 值）。

+   性能指标（`Metrics`）：模型的评估指标（例如，准确率、精确率、召回率、`F1` 分数等）。

+   模型产物（`Artifacts`）：训练好的模型本身，以及其他产出物（例如，特征重要性图）。

+   SDK v2 作业中的无缝 MLflow 集成：这是 Azure ML 平台的一个强大特性。当你在一个作为 Azure ML 作业运行的脚本中使用 MLflow 时，你不需要进行任何额外的配置。Azure ML 的运行环境会自动设置好 `MLflow Tracking URI`，确保所有 `mlflow` 的日志记录都会被自动捕获并关联到 Azure ML 中对应的“运行（`Run`）”上。

+   运行（`Run`）对象：每一次作业的提交都会在 Azure ML 中创建一个“运行”对象。这个对象不仅是作业执行的载体，也成为了一个集中的信息容器，所有由 MLflow（无论是自动还是手动）记录的参数、指标和产物都会被存储在这里，便于后续的查询、比较和分析。

+   作业输出 vs. MLflow 产物：在脚本中，我们依然可以将模型保存到 `outputs` 文件夹，这是作业的原生输出。同时，`mlflow.autolog()` 也会将模型记录为 MLflow 的产物。后者的优势在于它记录得更“智能”，会附带模型的“风格”（`flavor`），包含了环境依赖和加载方法，使得模型的复用和部署更加可靠。

---

### 实验核心流程分析（基于 `Labs/08/Use MLflow to track jobs.ipynb` 代码）

接下来，我们深入分析该 Notebook 中的核心代码片段。

1.  核心的训练脚本（`src/main.py`）

```
# ... imports ...
import mlflow
import argparse
from sklearn.linear_model import LogisticRegression
import joblib

# Start MLflow logging
mlflow.autolog()

def main():
    # parse arguments
    parser = argparse.ArgumentParser()
    parser.add_argument("--data", type=str, help="path to input data")
    args = parser.parse_args()

    # ... load data ...
    
    # Train a logistic regression model
    model = LogisticRegression().fit(X_train, y_train)

    # Save the model
    joblib.dump(value=model, filename="outputs/model.pkl")

if __name__ == '__main__':
    main()
```

+   `mlflow.autolog()`：这是整个脚本中最关键的一行代码。它的位置就在所有模型训练逻辑开始之前。

+   **自动化实现**：当 `model.fit(X_train, y_train)` 这行代码执行时，`autolog` 会自动发挥作用。因为它检测到 `model` 是一个 `scikit-learn` 模型，它会：

1.  **自动记录参数**：检查 `model` 的所有超参数（如 `C`、`solver` 等）并将它们记录下来。

2.  **自动记录指标**：在 `fit` 完成后，它会自动使用测试数据（如果有的话，或者在某些情况下会使用训练数据）对模型进行评估，并记录下一整套标准的分类指标（`accuracy`、`precision`、`recall`、`f1-score` 等）。

3.  **自动记录模型**：将训练好的 `model` 对象以 MLflow 的格式记录为一个产物，包含 `conda.yaml` 和 `MLmodel` 文件。

+   **代码简洁性**：请注意，这个脚本中没有任何手动的 `mlflow.log_param` 或 `mlflow.log_metric` 调用。所有的跟踪记录都由 `autolog()` 自动完成，极大地简化了代码。

2.  命令作业的定义
