
![](img/5ceab0467f9310402af490498f96e8a3.png)

### 4.3 在 Cline 中配置 `mcp-mongo-server`

接下来，我们需要找到一个支持 MongoDB 的 MCP Server，本来想自己开发一个，但是简单搜了一下，发现已经有几个了：

![](img/e6cbcf8a8589c8354e62168e289765fd.png)

这里我们选择使用 `mcp-mongo-server`：

![](img/96f6c32bb1349df882ef604362ef3283.png)

可以发现这里内置了一批 MCP Server 列表，并且也支持一键安装：

> 下面我们开始在 Cline 中配置 MCP Server，首先点击上方工具栏的 MCP Server 图标

然后发现打开了一个 JSON 文件，这就是 Cline 用于存放 MCP 配置的地方，里面只有一个空对象：

> 但是搜了下我们选择的 MCP Server 并没有在这个内置列表里，所以我们选择手动配置，其实手动配置也非常简单，我们点击 installed tab，然后点击下方的 Configure MCP Servers，

![](img/d76a437c3761bcfc0c5efdb7d13a79ad.png)

其实在任何支持了 MCP 的客户端中都可以这样配置：

> 每个 MCP Server 都会提供这份配置的写法，比如我们选择的 `mcp-mongo-server`，这里所说的在 Claude Desktop 中使用

![](img/abab1c65d52721152e570e6ee30783f0.png)

配置看着挺复杂的，但实际上它支持提供了三种配置不同的写法，其实每个配置中只包含两个关键参数：

+ `command`：指定在命令行中通过什么命令进行执行，在这个例子中，所有配置都使用 `npx` 命令（也就是 Node.js 环境）。

+ `args`：是一个数组，包含传递给 `command` 的参数。

这三个配置的意思分别是：

+ `mongodb`：运行 `mcp-mongo-server` npm 包，并且使用指定的 MongoDB 连接字符串，默认是读写模式。

+ `mongodb-readonly`：同样运行 `mcp-mongo-server` npm 包，使用相同的 MongoDB 连接字符串，但增加了 `--read-only` 参数，意味着这个配置会以只读模式运行。

+ `mongodb-github`：从 GitHub 上的 `kiliczsh/mcp-mongo-server` 仓库获取包来运行，而不是 npm 包，使用相同的 MongoDB 连接字符串，并且也设置了只读模式。

可以直接到客户端查看这个数据库的连接地址：

> 这里我们直接使用其最简单的配置，本地 mongo 一般不会设置用户名密码。

![](img/3f976ed46078927019463deddca920e9.png)

然后配置如下：

```json
{
    "mongodb": {
      "command": "npx",
      "args": [
        "mcp-mongo-server",
        "mongodb://localhost:27017/studentManagement?authSource=admin"
      ]
    }
  }
}
```

粘贴到 Cline 的 MCP 配置文件，然后我们发现左侧的 `mongodb` 绿灯亮起，说明配置成功：

### 4.4 通过 Prompt 优化查询效果

> MCP Server 为模型提供了访问数据库的能力，但是数据库的表结构对于模型还是完全黑盒的。

> 所以模型只能靠猜，或者去先获取一下表结构，再进行后续操作，猜还是有可能会出错的，多获取一次表结构也会让回答速度变慢，以及消耗更多的 Token。

关于表结构的说明大家可以去用 DeepSeek 来生成：

> 所以这里有个优化技巧，我们直接在全局提示词里将表结构的关键信息，和我们的明确要求告诉模型，就能让模型更准确、高效的响应了

```
使用中文回复。

当用户提问中涉及学生、教师、成绩、班级、课程等实体时，需要使用 MongoDB MCP 进行数据查询和操作，表结构说明如下：

# 学生管理系统数据库表结构说明

## 1. 教师表 (teachers)

| 字段名 | 类型 | 描述 | 约束 | 示例 |
|--------|------|------|------|------|
| _id | String | 教师ID | 主键 | "T001" |
| name | String | 教师姓名 | 必填 | "张建国" |
| gender | String | 性别 | "男"或"女" | "男" |
| subject | String | 教授科目 | 必填 | "数学" |
| title | String | 职称 | 必填 | "教授" |
| contact.phone | String | 联系电话 | 必填 | "13812345678" |
| contact.office | String | 办公室位置 | 必填 | "博学楼301" |
| contact.wechat | String | 微信(可选) | 可选 | "lily_teacher" |
| isHeadTeacher | Boolean | 是否为班主任 | 可选 | true |

## 2. 班级表 (classes)

| 字段名 | 类型 | 描述 | 约束 | 示例 |
|--------|------|------|------|------|
| _id | String | 班级ID | 主键 | "202301" |
| className | String | 班级名称 | 必填 | "2023级计算机1班" |
| grade | Number | 年级 | 必填 | 2023 |
| headTeacherId | String | 班主任ID | 外键(teachers._id) | "T003" |
| classroom | String | 教室位置 | 必填 | "1号楼302" |
| studentCount | Number | 学生人数 | 必填 | 35 |
| remark | String | 备注信息 | 可选 | "市级优秀班集体" |

```

然后我们到 Cline 设置中找到 Custom Instructions ，把这段提示词粘贴进去，点击 Done：

然后我们开一个新的聊天窗口，重新来问这个问题：

> 有哪些同学期末考试比平时的考试成绩好？

在最后的结论中，按课程进行分组，给出了具体的各个科目期末考试大于平时考试的情况：

![](img/0d10802ad787e538aaf8222311a72b4a.png)

### 4.5 对比知识库

然后，我们将同样的数据集导入知识库，这里我们使用 Coze 进行演示，先来试试一张表的情况：

![](img/3e8297b02cfd0c8b6f745234b99dc9bd.png)

效果还可以，然后换上我们刚刚复杂一点的问题：

![](img/3bb3fdf6b473fa734adfe5a98cd271e8.png)

模型依然无法分析出准确信息，这个对比就很明显了，在这种数据的检索场景下，MCP + 数据库明显优于传统 RAG 的方式。
