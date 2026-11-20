
```
import torch.nn as nn
import torch.nn.functional as F

class BasicBlock(nn.Module):
    def __init__(self, in_channels, out_channels, stride=1, downsample=None):
        super(BasicBlock, self).__init__()
        self.conv1 = nn.Conv2d(in_channels, out_channels, kernel_size=3, stride=stride, padding=1)
        self.bn1 = nn.BatchNorm2d(out_channels)
        self.conv2 = nn.Conv2d(out_channels, out_channels, kernel_size=3, stride=1, padding=1)
        self.bn2 = nn.BatchNorm2d(out_channels)
        self.downsample = downsample
        self.relu = nn.ReLU(inplace=True)

    def forward(self, x):
        identity = x
        out = self.conv1(x)
        out = self.bn1(out)
        out = self.relu(out)
        out = self.conv2(out)
        out = self.bn2(out)
        
        if self.downsample is not None:
            identity = self.downsample(x)
        
        out += identity
        out = self.relu(out)
        return out

class ResNetClassifier(nn.Module):
    def __init__(self, block, layers, num_classes=11):
        super(ResNetClassifier, self).__init__()
        self.in_channels = 64
        self.conv1 = nn.Conv2d(3, 64, kernel_size=7, stride=2, padding=3)
        self.bn1 = nn.BatchNorm2d(64)
        self.relu = nn.ReLU(inplace=True)
        self.maxpool = nn.MaxPool2d(kernel_size=3, stride=2, padding=1)
        self.layer1 = self._make_layer(block, 64, layers[0])
        self.layer2 = self._make_layer(block, 128, layers[1], stride=2)
        self.layer3 = self._make_layer(block, 256, layers[2], stride=2)
        self.layer4 = self._make_layer(block, 512, layers[3], stride=2)
        self.avgpool = nn.AdaptiveAvgPool2d((1, 1))
        self.fc = nn.Linear(512, num_classes)

    def _make_layer(self, block, out_channels, blocks, stride=1):
        downsample = None
        if stride != 1 or self.in_channels != out_channels:
            downsample = nn.Sequential(
                nn.Conv2d(self.in_channels, out_channels, kernel_size=1, stride=stride),
                nn.BatchNorm2d(out_channels),
            )

        layers = []
        layers.append(block(self.in_channels, out_channels, stride, downsample))
        self.in_channels = out_channels
        for _ in range(1, blocks):
            layers.append(block(out_channels, out_channels))

        return nn.Sequential(*layers)

    def forward(self, x):
        x = self.conv1(x)
        x = self.bn1(x)
        x = self.relu(x)
        x = self.maxpool(x)

        x = self.layer1(x)
        x = self.layer2(x)
        x = self.layer3(x)
        x = self.layer4(x)

        x = self.avgpool(x)
        x = x.view(x.size(0), -1)
        x = self.fc(x)
        return x

# 使用ResNet18风格的层数
model = ResNetClassifier(BasicBlock, [2, 2, 2, 2])
```

# 数据/图像增强

图像数据增强是提升卷积神经网络（CNN）性能的重要策略之一，尤其是在数据量有限的情况下。通过对现有训练数据进行各种变换，数据增强可以有效扩展数据集规模，增强模型的鲁棒性和泛化能力。

## 几何变换（Geometric Transformations）

- **旋转（Rotation）**：随机旋转图像一定角度，有助于CNN学习旋转不变性，使得模型能够更好地处理不同角度的对象。

- **平移（Translation）**：对图像进行随机水平或垂直方向的平移，帮助模型更好地应对对象位置的变化。

- **翻转（Flipping）**：常见的操作是水平翻转，用于应对左右对称的对象，如人脸、动物等。

- **缩放（Scaling）**：随机缩放图像可以帮助模型学习处理不同尺度的对象，增强模型的尺度不变性。

- **剪切（Shearing）**：通过改变图像形状，模拟对象的倾斜或变形，这有助于模型在面对非标准形状的对象时仍能做出正确的判断。

```
from torchvision import transforms

# 旋转：随机旋转图像一定角度
transform_rotate = transforms.RandomRotation(degrees=30)

# 平移：随机水平或垂直方向的平移
transform_translate = transforms.RandomAffine(degrees=0, translate=(0.1, 0.1))

# 翻转：水平翻转
transform_flip = transforms.RandomHorizontalFlip(p=0.5)

# 缩放：随机缩放图像
transform_scale = transforms.RandomResizedCrop(size=224, scale=(0.8, 1.0))

# 剪切：随机剪切图像
transform_shear = transforms.RandomAffine(degrees=0, shear=20)
```

## 颜色变换（Color Transformations）

- 亮度调整（Brightness Adjustment）：通过随机增加或减少图像的亮度，可以模拟不同光照条件下的图像，增强模型的光照不变性。

- 对比度调整（Contrast Adjustment）：随机改变图像的对比度，帮助模型在不同对比度的场景中保持鲁棒性。

- 饱和度调整（Saturation Adjustment）：调整图像的颜色饱和度，增强模型对颜色变化的适应性。

- 色调调整（Hue Adjustment）：通过改变图像的色调，可以增强模型对不同颜色组合的鲁棒性。

```
from torchvision import transforms

# 亮度调整：随机增加或减少图像的亮度
transform_brightness = transforms.ColorJitter(brightness=0.5)

# 对比度调整：随机改变图像的对比度
transform_contrast = transforms.ColorJitter(contrast=0.5)

# 饱和度调整：调整图像的颜色饱和度
transform_saturation = transforms.ColorJitter(saturation=0.5)

# 色调调整：改变图像的色调
transform_hue = transforms.ColorJitter(hue=0.2)
```

## 噪声添加（Adding Noise）

- 高斯噪声（Gaussian Noise）：在图像中添加高斯噪声，可以使模型对真实世界中的噪声数据更加稳健。

- 椒盐噪声（Salt and Pepper Noise）：通过随机在图像中加入白色和黑色像素点，模拟图像传感器噪声，提升模型的抗噪能力。

```
import torch
import torchvision.transforms.functional as F
from PIL import Image
import numpy as np

# 高斯噪声
class AddGaussianNoise(object):
    def __init__(self, mean=0.0, std=1.0):
        self.mean = mean
        self.std = std

    def __call__(self, img):
        noise = torch.randn(img.size()) * self.std + self.mean
        noisy_img = img + noise
        return noisy_img.clamp(0, 1)

# 椒盐噪声
class AddSaltPepperNoise(object):
    def __init__(self, prob=0.01):
        self.prob = prob

    def __call__(self, img):
        img_np = np.array(img)
        num_salt = np.ceil(self.prob * img_np.size * 0.5)
        num_pepper = np.ceil(self.prob * img_np.size * 0.5)

        coords = [np.random.randint(0, i - 1, int(num_salt)) for i in img_np.shape]
        img_np[coords[0], coords[1], :] = 1

        coords = [np.random.randint(0, i - 1, int(num_pepper)) for i in img_np.shape]
        img_np[coords[0], coords[1], :] = 0

        return Image.fromarray(img_np)

transform_gaussian_noise = transforms.Compose([
    transforms.ToTensor(),
    AddGaussianNoise(mean=0, std=0.1),
    transforms.ToPILImage()
])

transform_salt_pepper_noise = transforms.Compose([
    AddSaltPepperNoise(prob=0.01)
])
```

## 裁剪和填充（Cropping and Padding）

- 随机裁剪（Random Cropping）：从图像中随机裁剪出一部分并进行训练，有助于模型聚焦于图像中的重要区域。

- 填充（Padding）：在图像周围添加像素，使得裁剪后的图像恢复到原始大小，这样可以保持数据的一致性。

```
from torchvision import transforms

# 随机裁剪
transform_random_crop = transforms.RandomCrop(size=224)

# 填充：在图像周围添加像素
transform_padding = transforms.Pad(padding=4)
```

## 高级数据增强技术

- 混合增强（MixUp）：通过将两张图像的像素值进行加权平均，生成新的训练样本，从而使模型在处理类间关系时更加稳健。

- 随机擦除（Random Erasing）：随机遮挡图像中的部分区域，迫使模型在信息不完整的情况下仍能正确分类，增强模型的鲁棒性。

- 对抗样本生成（Adversarial Augmentation）：生成对抗样本，通过微小的扰动引起模型错误分类，从而使模型对这种干扰更加不敏感。

```
import torchvision.transforms as transforms

# 混合增强：需要在数据加载时处理
def mixup_data(x, y, alpha=1.0):
    '''Returns mixed inputs, pairs of targets, and lambda'''
    if alpha > 0:
        lam = np.random.beta(alpha, alpha)
    else:
        lam = 1

    batch_size = x.size()[0]
    index = torch.randperm(batch_size)

    mixed_x = lam * x + (1 - lam) * x[index, :]
    y_a, y_b = y, y[index]
    return mixed_x, y_a, y_b, lam

# 随机擦除
transform_random_erasing = transforms.RandomErasing(p=0.5, scale=(0.02, 0.33), ratio=(0.3, 3.3), value=0)

# 对抗样本生成（Adversarial Augmentation）：需要使用外部库如`advertorch`，下面是一个简单的例子
from advertorch.attacks import LinfPGDAttack

def adversarial_attack(model, x, y):
    adversary = LinfPGDAttack(
        model, loss_fn=torch.nn.CrossEntropyLoss(), eps=0.3,
        nb_iter=40, eps_iter=0.01, rand_init=True, clip_min=0.0, clip_max=1.0,
        targeted=False)
    adv_perturbation = adversary.perturb(x, y)
    return adv_perturbation
```

学有余力的同学可以看：13.1.1. 常用的图像增广方法

## 更多QA

详见 [QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/BdEMwwcDoiUf72k0zADchzO2n1e)！

> [QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/BdEMwwcDoiUf72k0zADchzO2n1e) 任何想法都可以在学习群里交流或在进行填写哟~

> 本教程主要由    写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

> 注：本文采用 署名—非商业性使用—相同方式共享 4.0 协议国际版许可协议进行许可。
