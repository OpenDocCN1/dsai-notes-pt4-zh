
# Python 心形曲线绘制示例

## 代码实现

```python
import matplotlib.pyplot as plt
import numpy as np

# Define the parametric equations for the heart shape
t = np.linspace(0, 2 * np.pi, 1000)
x = 16 * np.sin(t)**3
y = 13 * np.cos(t) - 5 * np.cos(2*t) - 2 * np.cos(3*t) - np.cos(4*t)

# Create the figure and plot the curve
plt.figure(figsize=(6, 6))
plt.plot(x, y, 'r')
plt.title('Heart Shape')
plt.xlabel('x-axis')
plt.ylabel('y-axis')
plt.grid(True)
plt.axis('equal')

# Show the plot
plt.show()
```

## 完整代码

```python
import matplotlib.pyplot as plt
import numpy as np

# Define the parametric equations for the heart shape
t = np.linspace(0, 2 * np.pi, 1000)
x = 16 * np.sin(t)**3
y = 13 * np.cos(t) - 5 * np.cos(2*t) - 2 * np.cos(3*t) - np.cos(4*t)

# Create the figure and plot the curve
plt.figure(figsize=(6, 6))
plt.plot(x, y, 'r')
plt.title('Heart Shape')
plt.xlabel('x-axis')
plt.ylabel('y-axis')
plt.grid(True)
plt.axis('equal')

# Show the plot
plt.show()
```

## 运行输出

Received: image `<PIL.PngImagePlugin.PngImageFile image mode=RGBA size=543x547 at 0x7F3ED1BDA260>`

## 可视化效果

可视化效果如下：

![](img/f7b32fef35ed10ffacb3b7196c280b1b.png)
