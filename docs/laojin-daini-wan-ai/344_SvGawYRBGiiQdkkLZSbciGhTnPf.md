
# Anaconda - Python环境安装

> 来源：[https://my.feishu.cn/docx/FpYtdAz96o02FTx9Gmoc8f79nVb](https://my.feishu.cn/docx/FpYtdAz96o02FTx9Gmoc8f79nVb)

## 简介

`Anaconda`，中文大蟒蛇，是一个开源的 [Python](https://baike.baidu.com/item/Python/407313?fromModule=lemma_inlink) 发行版本，其包含了 `conda`、`Python` 等 180 多个科学包及其依赖项。

宝塔界面左侧打开【终端】。`Anaconda` 版本可以按需下载，在网址中可以看到所有版本列表。

## 下载安装

```
echo "开始安装 Anaconda..."
wget https://repo.anaconda.com/archive/Anaconda3-2024.06-1-Linux-x86_64.sh
```

## 执行安装

```
bash Anaconda3-2024.06-1-Linux-x86_64.sh -b -p /root/anaconda
```

安装界面如图所示，要等最下方的 `done` 输出完成后再输入。

![](img/2d1cba6928882f402cdf2b331a56eb46.png)

## 清理安装包

```
rm -f Anaconda3-2021.05-Linux-x86_64.sh
```

继续粘贴，注意看图完成效果，再继续，注意 Python 版本按照自己的需求安装。

## 创建环境

```
/root/anaconda/bin/conda create -y --name AI python=3.11
```

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
