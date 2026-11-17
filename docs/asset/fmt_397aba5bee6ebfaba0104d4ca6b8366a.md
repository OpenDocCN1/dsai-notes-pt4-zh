
你可以为每个 `worktree` 启动一个 Claude 实例，让它们同时处理不同的功能开发或 bug 修复，互不干扰。

## c. 使用无头模式与自定义脚本

结合无头模式和自定义脚本，可以实现大规模的自动化工作流。

+ 扇出（Fanning out）：适用于大规模迁移或分析。首先让 Claude 生成一个任务列表（如需要迁移的 2000 个文件），然后编写脚本循环调用无头模式的 Claude 来处理每个任务。

+ 管道（Pipelining）：将 Claude 集成到现有的数据处理管道中。例如：`claude -p “<your prompt>” --json | your_next_command`，利用 Claude 的处理能力作为管道中的一个环节。

---

`https://huchegames.feishu.cn/sync/Hzv6dRSGJsmHgOb4eSHcN1BHntg`
