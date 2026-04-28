# Learning Hub- Vanna AI

> 来源：[https://gotoaiworld.feishu.cn/docx/MbhDdli1fogbKsxcruycgI8pneg](https://gotoaiworld.feishu.cn/docx/MbhDdli1fogbKsxcruycgI8pneg)

# **官方核心资料**

1.  **Vanna 官方文档**vanna.ai：这是最权威的入门资料（https://vanna.ai/docs/ ），内容覆盖从安装配置到进阶扩展的全流程。比如基础用法里的 API 密钥获取、模型设置、三种训练方式（SQL、DDL、文档训练），还有本地部署、数据库连接等关键操作，每个知识点都配有可直接复制运行的代码示例，同时支持查看不同数据库和 LLM 的适配方法。

1.  Vanna GitHub 开源仓库vanna.ai：作为开源项目核心，仓库（https://github.com/vanna-ai/vanna ）包含完整源代码、入门示例和贡献指南。其中examples目录下有多个实战案例，像结合 Flask 搭建 Web 应用、连接不同向量数据库的演示代码，能帮助学习者直观理解框架的可扩展性；README 文件还清晰说明了框架依赖、安装步骤和基础调用示例，是入门必看的开源资料。

# **中文实战教程与博客**

1.  **《零基础掌握 Vanna Tex**t2SQL 框架：从原理到实战训练指南》：发布于 CSDN 的免费博客，适合零基础学习者。文章先拆解 Vanna 的 RAG 核心原理和 7 步工作流程，再一步步讲解环境搭建、MySQL/SQLite 数据库连接，还详细介绍了通过 DDL、业务文档、SQL 三种维度训练模型的方法，搭配具体代码示例和效果说明，能快速掌握核心实战流程。

1.  《VannaAI 介绍及使用 - 第二篇》：CSDN 的系列教程，聚焦实操场景。分别讲解了在官网 Colab、本地 Jupyter 和 PyCharm 三种场景下的使用方法，尤其是官网 Colab 的快捷体验流程，无需本地配置环境，替换邮箱获取验证码即可运行示例，适合新手快速体验 Vanna 的自然语言转 SQL 功能。

1.  《【亲测免费】 Vanna 开源项目教程》：该 CSDN 博客简洁梳理了 Vanna 的核心定位和快速启动方式。不仅提供了项目的开源资源链接，还给出了与 Flask 结合搭建交互式 Web 应用的思路，同时补充了模型选择、SQL 验证、错误处理等实用建议，适合入门后初步尝试小型应用开发。

# **在线实操平台**

1.  Vanna 官网 Colab 示例：官网提供的在线 Jupyter Notebook 环境，无需本地安装依赖。进入官网后找到 “Getting Started - try it in jupyter”，点击跳转后可直接运行预设的app.ipynb文件，只需替换自己的邮箱获取验证码，就能体验模型训练和自然语言提问生成 SQL 的完整流程，实时查看 Prompt 构建、SQL 生成、结果输出的全过程。

1.  Kaggle+Vanna 实操组合：可在 Kaggle 免费的 Python 环境中导入 Vanna 库，结合 Kaggle 上的公开数据集（如电商订单、用户行为数据集）做实战练习。比如用公开的电商数据集训练 Vanna 模型，尝试用自然语言提问 “近 3 个月销量最高的商品”，验证生成 SQL 的准确性，适合巩固基础操作。

# **辅助学习社区与交流渠道**

1.  Vanna Discord 社区：官方推荐的交流平台，可加入与开发者和其他学习者交流入门问题，比如环境配置报错、模型训练效果优化等，能获取一手的问题解决方案。

1.  **博客园 Vanna 相关专栏**：部分技术博主会分享 Vanna 的进阶使用技巧和版本更新解读，比如框架的性能优化、与新 LLM 模型的适配案例等，适合入门后进一步拓展知识。