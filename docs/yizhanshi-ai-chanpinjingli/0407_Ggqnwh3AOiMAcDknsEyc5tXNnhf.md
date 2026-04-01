# Pytorch torch 参考手册

> 来源：[https://v11enp9ok1h.feishu.cn/docx/WOIadH2dOo49ZUxZjjucC7b3nYg](https://v11enp9ok1h.feishu.cn/docx/WOIadH2dOo49ZUxZjjucC7b3nYg)

## PyTorch torch API 手册

### **Tensor 创建**

### **数学运算**

### **随机数生成**

### **线性代数**

---

### **设备管理**

### 实例

```
import torch

# 创建张量
x = torch.tensor([1, 2, 3])
y = torch.zeros(2, 3)

# 数学运算
z = torch.add(x, 1)  # 逐元素加 1
print(z)

# 索引和切片
mask = x &gt; 1
selected = torch.masked_select(x, mask)
print(selected)

# 设备管理
if torch.cuda.is_available():
    device = torch.device(&#x27;cuda&#x27;)
    x = x.to(device)
    print(x.device)
```