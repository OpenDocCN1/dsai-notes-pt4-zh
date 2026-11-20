
```
#将output文件夹压缩，如果这段代码无法生成output.zip，运行下面的代码
_ = !zip -r output.zip output/
```

```
#如果上面代码无响应，运行下面这段代码
import zipfile, os
path = './output/'  # 要压缩的文件路径
zipName = 'output.zip'  # 压缩后的zip文件路径及名称

# 创建一个新的zip文件
f = zipfile.ZipFile(zipName, 'w', zipfile.ZIP_DEFLATED)
#使用zipfile模块创建一个新的zip文件对象，指定为写模式('w')并采用ZIP_DEFLATED压缩算法。

# 遍历指定路径下的所有文件和文件夹
for dirpath, dirnames, filenames in os.walk(path): #使用os.walk函数遍历指定路径下的所有文件和文件夹，包括子文件夹
    for filename in filenames: #遍历每个文件夹中的文件名。
        print(filename)
        # 将文件添加到zip文件中
        f.write(os.path.join(dirpath, filename))

# 关闭zip文件
f.close()
```

把`output.zip`下载到本地，打开[赛事主页](https://tianchi.aliyun.com/s/e09ba4f624e4678219c563c15f1bb5a2)，在**提交结果**初提交`output.zip`，稍等片刻评测成功，在**我的成绩**处可以看到成绩

![](img/42633bfcfba21958e484847ca54eb99d.png)

![](img/cd1bdf9d1c72bdbc274f238aa244f914.png)

## 完整代码

![](img/6c33bb7d5eb42857289d34a9a7d8be31.png)

[`weather_ml_baseline_update代码注释.ipynb`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/T6YubVgvAo50k5xQ9efcWgiqnuh)

# 任务3：更多优化方向

可以看到前面所使用的训练数据和模型参数设置，都是有所保留的，以及特征提取也没挖掘太多，还有很大的优化空间，具体可以是：

1. 扩大使用的训练数据，从10份扩展到20份、30份、50份；
2. 参数优化，学习率和迭代次数；
3. 特征优化，经纬度对应的变量可以进行聚合统计，比如横向、纵向，又或者相邻经纬的变量，统计方法如最大、最小、均值、中位数、方差等。

# 附录

## [QA文档](https://xqbbve5hpvs.feishu.cn/docx/DiooduxPHoCbv4xk5atc9Td5npe#EgbkdQYRhon3KwxMb5Xc1kuVnBh)

[Ai for science 赛道问题库](https://xqbbve5hpvs.feishu.cn/docx/DiooduxPHoCbv4xk5atc9Td5npe#EgbkdQYRhon3KwxMb5Xc1kuVnBh)

## 实践环境配置

### AI环境配置：

+ **视频讲解：**AI夏令营：手把手带你配置AI环境_哔哩哔哩_bilibili

+ **图文材料：**https://gitee.com/anine09/learn-python-the-smart-way-v2/blob/main/slides/chapter_0-Installation.ipynb

## 工具使用与提分思路

+ 竞赛实践路线：[竞赛实践路线分享](https://datawhaler.feishu.cn/docx/EJ2Edl0hXoIWwuxO15CcEj9Wnxn)

+ 时间序列预测方法总结：https://zhuanlan.zhihu.com/p/67832773
