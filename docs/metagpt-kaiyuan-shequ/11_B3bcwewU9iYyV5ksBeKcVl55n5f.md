# 代码贡献

> 来源：[https://deepwisdom.feishu.cn/docx/YSYYdvZoAonpjsxY1OdcCAH3nic](https://deepwisdom.feishu.cn/docx/YSYYdvZoAonpjsxY1OdcCAH3nic)

# 开发环境配置

您可以参考[安装教程](https://deepwisdom.feishu.cn/wiki/C2H3wIluSiVPClkPddqcVDyOnzf)部分来配置开发环境。

# 提交前的准备

1.  搜索检查：在提交前，搜索查看是否有相似的问题或 PR（Pull Request，拉取请求），以避免重复创建。

1.  清晰描述：尽可能清晰地描述问题或提交的 PR。

1.  代码格式化和单元测试：在编写代码之前，进行全面的代码格式化和单元测试检查，确保通过后再提交。

1.  依赖包更新：检查是否有更新的依赖包版本，以避免在新环境中出现安装失败的情况。

# 代码提交方式

请使用 fork and pull request 的方式提交代码。

通常情况下，LGTM（Looks Good To Me）需要由 2 位贡献者审核通过后，才允许合并。提交超过 10 行代码的 PR 后，提交者可以加入 MetaGPT-dev 组。一般来说，鼓励更频繁地提交小量代码，大型 PR 提交往往需要更多的上下文理解和审核时间，如果能提供更多支持材料会更好。

# PR

PR 通常包括 bug 修复和新功能提交，两者都需要尽可能按照以下流程规范进行提交。

## 提交前

+   确保修改现有代码所涉及的单元测试或新代码对应的单元测试能够通过自测。

+   确保提交的代码具有完整的 Google Docstring 描述和过程功能注释。

+   我们使用pytest进行单元测试。在提交前，安装pytest相关的依赖包，更新修改代码的单元测试或为新添加的代码添加单元测试。一般来说，我们期望单元测试文件名以test_开头，并使用与模块相同的路径创建。例如：role.py实现了role抽象，并放置在metagpt/roles/下，对应的单元测试文件应为tests/metagpt/roles/test_role.py。

+   对于需要网络访问的单元测试，如provider，我们建议使用mock方法来添加，这样将不会受到网络的影响。一个参考示例是：tests/metagpt/provider/test_qianfan_api.py。

```
pip3 install pytest==7.2.2 pytest-mock==3.11.1 pytest-asyncio==0.21.1 # Recommended
pytest tests # Example, execute all test files
pytest tests/metagpt/environment/* # Example, execute multiple test files
pytest tests/metagpt/environment/test_base_env.py # Example, execute a single test file
```

+   在提交前，安装pre-commit并使用它来检查代码规范是否满足提交条件。具体用法如下：

```
pip3 install pre-commit
pre-commit install
pre-commit run --all-files # or check individual files pre-commit run --files metagpt/roles/*
```

+   执行后，它会自动根据规范要求修改本地代码，您需要再次进行git add。

## 提交时

+   我们默认添加了PULL_REQUEST_TEMPLATE模板。在提交时，需要添加必要的信息，包括：

+   Features（功能）：当前 PR 添加的功能（解释必要性）或修复的问题。必填项。

+   Features Docs（功能文档）：对应功能站的文档描述。可选项。

+   Influence（影响）：此功能可能带来的影响。可选项。

+   Result（结果）：（部分）单元测试执行结果，示例执行结果文件或日志（如有）等。必填项。

+   Other（其他）：可以补充的其他材料。可选项。

+   上述信息有助于代码审查者了解 PR 的上下文，并加快 PR 的审查速度。

## 提交后

+   提交后，将默认使用 github-ci 工作流来检查代码规范和单元测试。如果不通过，将被退回修改，直到通过为止。因此，为了提高效率，建议离线完成pre-commit检查和单元测试结果审查。

# Issue 内容

Issue 内容可以包括 bug 反馈、期望支持的新功能描述、已支持功能的深入优化等。

其中，新功能、优化项等可以根据您的需求场景进行描述，为双方进一步沟通提供足够的上下文。提交后，社区工作人员将与您联系，我们将在沟通和确认后将其更新到 ROADMAP 中。我们默认添加了request_new_features模板。在提交时，需要添加必要的信息，包括：

+   Feature description（功能描述）：必填项。

+   Your Feature（您的功能）：描述实现该功能的动机、想法、参考来源或实现过程。可选项。

对于 bug 反馈，为了有足够的问题上下文进行分析，我们默认添加了show_me_the_bug模板。在提交时，需要添加必要的信息，包括：

+   Bug description（bug 描述）：必填项。

+   Bug solved method（bug 解决方法）：如果您知道如何解决它，则为可选项。

+   Environment information（环境信息）：包括使用的大型模型类型配置、系统版本、python 版本以及错误堆栈中的一些依赖包版本。必填项。

+   Screenshots or logs（截图或日志）：与 bug 对应的现场截图或日志。必填项。