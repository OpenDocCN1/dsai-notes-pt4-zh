
然后修改`ultralytics/engine/exporter.py`文件中`torch.onnx.export`的参数，把模型的输出数量改为 1 个。

![](img/a0e20a267d2ad71090b8e0fffc4bc12f.png)

代码修改完成后，执行命令`pip install -e '.[dev]'`使之生效，然后再重新用`yolo`命令导出模型。用`netron`工具可以看到模型只有一个 shape 为 [1,8400,25684] 的输出。

![](img/628dbd9c4737cc30bdfe3bedb18e760f.png)

这样在后处理的时候就可以直接去解析`box`和`mask`了，并且`mask`的数据不需要进行`sigmoid`激活。

### 4. 参考资料

+   How to implement instance segmentation using YOLOv8 neural network

+   https://github.com/AndreyGermanov/yolov8_segmentation_python
