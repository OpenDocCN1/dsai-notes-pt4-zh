
# GitHub初始化项目

> 来源：[https://now.feishu.cn/docx/FcAedam7goV8NJxHdPQcyu5Nnc3](https://now.feishu.cn/docx/FcAedam7goV8NJxHdPQcyu5Nnc3)

> 将项目`git`初始化并推送到GitHub

Cursor：

以下是将项目`Git`初始化并推送到GitHub的详细步骤：

## 操作步骤

1. 进入项目目录

```
cd /path/to/your/project
```

2. 初始化`Git`仓库

```
git init
```

3. 添加所有文件到暂存区

```
git add .
```

4. 提交初始代码

```
git commit -m "Initial commit"
```

5. 在GitHub上创建新仓库

+ 打开GitHub网站
+ 点击右上角的`+`号
+ 选择 "New repository"
+ 输入仓库名称
+ 选择是否公开
+ 不要勾选初始化`README`、`.gitignore`和`LICENSE`

6. 添加远程仓库

```
git remote add origin https://github.com/your-username/your-repository.git
```

7. 推送代码到GitHub

```
git push -u origin master
```

## 注意事项

+ 确保已安装`Git`
+ 替换`your-username` 和 `your-repository` 为实际的GitHub用户名和仓库名
+ 如果是首次使用，可能需要配置`Git`用户名和邮箱
