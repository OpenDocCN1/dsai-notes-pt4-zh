
分别对测试集、训练集进行降维，将人脸投影到10，20，30，…，160维空间中，计算未知人脸与所有已知人脸的距离（欧几里得距离），然后使用最近邻分类器`KNN`进行识别

![](img/fa54be5da7fea672787036ad61b82670.png)

![](img/393bddca70d7763ca3baa15f63929861.png)

### 6.3人脸图像降维与可视化

取出对应数量的特征脸（`n`维取`n`个），利用公式`projected_test_data = eigen_faces' * (test_data - mean_face)`对测试集或其他子集进行投影，投影后上色（同人同色）并使用`scatter`画图实现可视化

![](img/91db9016340dfc148f1054b2ae56f5c1.png)

![](img/19fcc35fc4cb7485651c95bcaafde415.png)
