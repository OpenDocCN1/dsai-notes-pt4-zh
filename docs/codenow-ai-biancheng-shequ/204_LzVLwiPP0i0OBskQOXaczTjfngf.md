# GitHub初始化项目

> 来源：[https://now.feishu.cn/docx/FcAedam7goV8NJxHdPQcyu5Nnc3](https://now.feishu.cn/docx/FcAedam7goV8NJxHdPQcyu5Nnc3)

> 将项目git初始化并推送到github

Cursor：

以下是将项目Git初始化并推送到GitHub的详细步骤：

1.  进入项目目录

```
cd /path/to/your/project
```

1.  初始化Git仓库

```
git init
```

1.  添加所有文件到暂存区

```
git add .
```

1.  提交初始代码

```
git commit -m &quot;Initial commit&quot;
```

1.  在GitHub上创建新仓库

+   打开GitHub网站

+   点击右上角的 "+" 号

+   选择 "New repository"

+   输入仓库名称

+   选择是否公开

+   不要勾选初始化README、.gitignore和LICENSE

1.  添加远程仓库

```
git remote add origin https://github.com/your-username/your-repository.git
```

1.  推送代码到GitHub

```
git push -u origin master
```

注意事项：

+   确保已安装Git

+   替换 your-username 和 your-repository 为实际的GitHub用户名和仓库名

+   如果是首次使用，可能需要配置Git用户名和邮箱