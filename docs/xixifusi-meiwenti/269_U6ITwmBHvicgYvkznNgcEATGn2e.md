
# Git

> 来源：[https://docs.feishu.cn/docx/CDvzduCoIoGYsQxiWWqcahiQnJe](https://docs.feishu.cn/docx/CDvzduCoIoGYsQxiWWqcahiQnJe)

## 当不小心传了特别大的文件的时候

```
git log 看你的历史版本
git reset --hard <commit-hash> 这将重置当前分支到指定的提交，其中 <commit-hash> 是之前提交的哈希值。请注意，这将删除当前分支上所有未提交的更改，因此请确保在重置之前保存您的更改。
然后再删除大文件，或者`gitignore`掉
```
