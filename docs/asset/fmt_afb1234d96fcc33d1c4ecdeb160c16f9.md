
# Agent之Cover Agent：Cover Agent

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/HLU4duONjo5dryxUfTJcJgwhnlc](https://qqs7y1hozd1.feishu.cn/docx/HLU4duONjo5dryxUfTJcJgwhnlc)

## Cover Agent的简介

CodiumAI Cover-Agent：一款AI驱动的工具，用于自动化测试生成和代码覆盖率的提升！

2024年5月9日，CodiumAI发布Cover Agent，它旨在通过自动生成合格的测试来提高代码覆盖率，以增强现有的测试套件。Cover-Agent 现在支持几乎任何世界上的 LLM 模型，使用 LiteLLM 包。这个专注的项目利用生成式 AI 自动化并增强测试的生成（目前主要是单元测试），旨在简化开发工作流程。Cover-Agent 可以通过终端运行，并计划集成到流行的 CI 平台中。

Cover-Agent是利用生成式AI自动化和提升生成测试(目前主要是单元测试)的专注项目，旨在简化开发工作流程。它能通过命令行运行，并计划集成到流行的CI平台中。Cover-Agent能通过Python包或独立二进制文件安装和运行。它支持多种编程语言，并计划支持更丰富的测试场景。未来还将整合主流CI平台，提高用户体验。

总之，Cover-Agent利用先进的生成式AI为自动化单元测试生成量身定制的高质量测试用例，旨在简化软件测试流程，提升代码质量。它的开源性质也将为相关研究与应用提供支持。

## Cover-Agent系统的主要组成部分

>> 测试运行器(Test Runner)：执行命令或脚本来运行测试套件并生成代码覆盖报告。

>> 代码覆盖解析器(Coverage Parser)：验证新增测试是否能增加代码覆盖率，从而确保新测试有助于提升整体测试质量。

>> 提示构建器(Prompt Builder)：从代码库中收集必要数据，构建传递给大语言模型(LLM)的提示。

>> AI调用器(AI Caller)：根据提示与LLM交互，由LLM基于提示生成测试用例。

## 新闻和更新

2024-05-26：`Cover-Agent` 现在支持几乎任何世界上的 LLM 模型，使用 `LiteLLM` 包。

注意，`GPT-4` 在代码任务和遵循复杂指令方面几乎优于任何开源模型。然而，我们更新了后处理脚本，使其更加全面，并成功运行了基准脚本，例如使用 `llama3-8B` 和 `llama3-70B` 模型。

2024-05-09：该存储库包含首个已知的 `TestGen-LLM` 实现，在 Meta 的论文《使用大型语言模型自动改进单元测试》中有所描述。

## CodiumAI

`CodiumAI` 的使命是帮助忙碌的开发团队提高和维护代码完整性。我们提供各种工具，包括我们的开源工具的“专业版”，这些工具旨在处理企业级代码复杂性，并且支持多存储库代码库。

### Cover Agent安装和使用方法

#### 1、安装和使用

要求

在开始之前，请确保您拥有以下内容：

>> 环境变量中设置的 `OPENAI_API_KEY`，这是调用 OpenAI API 所需的。

>> 代码覆盖工具：工具需要 Cobertura XML 代码覆盖报告才能正常运行。

例如，在 Python 中可以使用 `pytest-cov`。运行 `pytest` 时添加 `--cov-report=xml` 选项。

如果直接从存储库运行，您还需要：

系统上安装的 Python。

#### 2、独立运行

`Cover-Agent` 可以作为 Python `Pip` 包安装或作为独立可执行文件运行。

##### `Python Pip`

要直接通过 GitHub 安装 Python `Pip` 包，请运行以下命令：

```
pip install git+https://github.com/Codium-ai/Cover-Agent.git
```

##### 二进制文件

二进制文件可以在您的系统上未安装任何 Python 环境的情况下运行（例如在未包含 Python 的 Docker 容器中）。您可以通过导航到项目的发布页面下载适用于您的系统的版本。

##### 存储库设置

运行以下命令以安装所有依赖项并从源代码运行项目：

```
poetry install
```

##### 运行代码

下载可执行文件或安装 `Pip` 包后，您可以运行 `Cover-Agent` 以生成和验证单元测试。在命令行中使用以下命令执行它：

```
Cover-Agent
--source-file-path "<path_to_source_file>"
--test-file-path "<path_to_test_file>"
--code-coverage-report-path "<path_to_coverage_report>"
--test-command "<test_command_to_run>"
--test-command-dir "<directory_to_run_test_command>"
--coverage-type "<type_of_coverage_report>"
--desired-coverage <desired_coverage_between_0_and_100>
--max-iterations <max_number_of_llm_iterations>
--included-files "<optional_list_of_files_to_include>"
```

您可以使用此存储库中的示例项目作为测试运行此代码。

按照位于 `templated_tests/python_fastapi/` 目录中的 `README.md` 文件中的步骤操作，然后返回存储库根目录，并运行以下命令以向 python fastapi 示例添加测试：

```
Cover-Agent
--source-file-path "templated_tests/python_fastapi/app.py"
--test-file-path "templated_tests/python_fastapi/test_app.py"
--code-coverage-report-path "templated_tests/python_fastapi/coverage.xml"
--test-command "pytest --cov=. --cov-report=xml --cov-report=term"
--test-command-dir "templated_tests/python_fastapi"
--coverage-type "cobertura"
--desired-coverage 70
--max-iterations 10
```

对于使用 go 的示例，进入 `templated_tests/go_webservice` 目录，按照 `README.md` 设置项目。要处理覆盖报告，您需要安装 `gocov` 和 `gocov-xml`。

然后运行以下命令：

```
Cover-Agent
--source-file-path "app.go"
--test-file-path "app_test.go"
--code-coverage-report-path "coverage.xml"
--test-command "go test -coverprofile=coverage.out && gocov convert coverage.out | gocov-xml > coverage.xml"
--test-command-dir $(pwd)
--coverage-type "cobertura"
--desired-coverage 70
--max-iterations 1
```

尝试通过在此存储库的根目录运行以下命令向该项目添加更多测试：

```
poetry run Cover-Agent
--source-file-path "cover_agent/main.py"
--test-file-path "tests/test_main.py"
--code-coverage-report-path "coverage.xml"
--test-command "poetry run pytest --junitxml=testLog.xml --cov=templated_tests --cov=cover_agent --cov-report=xml --cov-report=term --log-cli-level=INFO"
--coverage-type "cobertura"
--desired-coverage 70
--max-iterations 1
--model "gpt-4o"
```

> **注意：**如果您使用的是 `Poetry`，请使用 `poetry run Cover-Agent` 命令而不是 `Cover-Agent run` 命令。

#### 输出

一些调试文件将本地输出到存储库中（它们是 `.gitignore` 的一部分）

+ `generated_prompt.md`：发送给 LLM 的完整提示

+ `run.log`：转储到 stdout 的日志副本

+ `test_results.html`：包含以下内容的生成测试结果表

> 测试状态
>
> 失败原因（如适用）
>
> 退出代码
>
> `stderr`
>
> `stdout`
>
> 生成的测试
>
> 使用其他 LLM

此项目使用 `LiteLLM` 与 OpenAI 和其他托管 LLM 通信（支持至今超过 100 个 LLM）。要使用除 OpenAI 默认之外的其他模型，您需要：

根据 `LiteLLM` 说明导出支持的 LLM 所需的任何环境变量。

调用 `Cover-Agent` 时使用 `--model` 选项调用模型名称。

例如（如 `LiteLLM` 快速入门指南中所述）：

```
export VERTEX_PROJECT="hardy-project"
export VERTEX_LOCATION="us-west"

Cover-Agent
...
--model "vertex_ai/gemini-pro"
```

#### 3、开发

本节讨论此项目的开发。

##### 版本控制

在合并到主分支之前，请确保手动增加存储库根目录中 `cover_agent/version.txt` 中的版本号。

##### 运行测试

运行 `poetry install` 命令设置开发环境，如上所述。

注意：对于旧版本的 `Poetry`，您可能需要包含 `--dev` 选项以安装开发依赖项。

**设置环境后，运行以下命令：**

```
poetry run pytest --junitxml=testLog.xml --cov=templated_tests --cov=cover_agent --cov-report=xml --cov-report=term --log-cli-level=INFO
```

这还将生成 `.github/workflows/ci_pipeline.yml` 中生成的所有日志和输出报告。

## 路线图

以下是计划功能的路线图及当前实现状态：

### 自动生成软件项目的单元测试

利用先进的 AI 模型确保全面的测试覆盖率和质量保证。（类似 Meta）

+ 能够为不同编程语言生成测试

+ 能够处理多种测试场景

+ 为被测试代码生成行为分析，并相应生成测试

检查测试的不稳定性，例如按 `TestGen-LLM` 建议运行 5 次

### 覆盖更多的测试生成痛点

+ 生成专注于 PR 变更集的新测试

+ 在整个存储库/代码库上运行，并尝试增强所有现有测试套件

### 改善可用性

为 GitHub Actions、Jenkins、CircleCI、Travis CI 等平台提供连接器

集成到数据库、API、OpenTelemetry 和其他数据源中，以提取相关的输入/输出进行测试生成

添加设置文件
