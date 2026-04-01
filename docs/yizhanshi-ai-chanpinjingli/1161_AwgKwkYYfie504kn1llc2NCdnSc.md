# 推荐系统之排序

> 来源：[https://v11enp9ok1h.feishu.cn/docx/VQUXdAPVfo0o7vxtSlYcoQAjn6e](https://v11enp9ok1h.feishu.cn/docx/VQUXdAPVfo0o7vxtSlYcoQAjn6e)

## **推荐系统之排序**

+   多目标排序模型

+   回顾一下推荐系统的链路

![](img/0c6004dc3f21bfeb3eb44b22cecd8632.png)

**常见的交互指标**

![](img/b13983127f209b8257e03ead47e4da36.png)

**排序模型做的事情**

![](img/e4d7c8d067d85346aae2eda2a7e4c311.png)

**多目标模型就是要预测多个目标**

![](img/ae257ae7ea9cdfc4b52d6038db6783a2.png)

预测概率和实际是否交互求交叉熵损失

![](img/96199f4bc30f62d91b4ead4acea2274b.png)

训练时通常会遇到类别不平衡问题，可以考虑做采样可能导致预估点击率偏高

![](img/952e1e1f47420bb247e1fd23a8a0d990.png)

![](img/f9a108897e07dfa9fe6c76582244ccdf.png)

**可以通过校准公式进行校准**

![](img/f49268ef0a74040d1a55f15cb17b8fd6.png)

### Multi-gate Mixture-of-Experts

+   **几个专家就是放几个神经网络**

![](img/1dd142f8de3ab6a283f277a5b74b63b4.png)

进一步考虑对多个神经网络的输出进行加权

![](img/5a2e959b3656818663d4e4d32abb5a7e.png)

![](img/87c3e38a547ac156b98862e3c643a0ca.png)

+   **可能会出现极化的现象**

![](img/a374d1707392b5f76e0627679ae00756.png)

可以通过dropout的方式来解决极化

![](img/ed455311f19c85e25ec2f947dd439583.png)

多目标有多个预估分数就可以有不同融合方式

![](img/cbfc75ec950e2e3930649908135c8b4d.png)

![](img/e86df506ef244f78435ed11c2c6246df.png)

![](img/2bc0ba960a676cf7536e0e420d40bf65.png)

![](img/5adfa4f5f9b92c1358d008982efec783.png)

视频播放建模

播放时长建模

![](img/c732f8b55af94c0c475861da710ec8e5.png)

![](img/fdd5ae02a174b88d94d152fd7d47bc13.png)

![](img/7a569e2908a1bcb71a3938fbfef7f59f.png)

+   **视频完播用回归或分类都可以**

![](img/7348b9148bfad9436ab683a049be21f0.png)

![](img/bea37d5c5f836449891287ad582cebf4.png)

完播率通常和视频时长有关，不能直接把预估的完播率⽤到融分公式

![](img/c520752e8cba8852318b10d4e77dbe5a.png)

通常做个调整再用到融分公式

![](img/a3c6ac224e561a1decd8aa5a9fc17711.png)

### 排序模型的特征

**用户画像**

**   物品画像**

![](img/803302701d118e0b09faa9ac129c1305.png)

![](img/00caaaca823eb7c81fe6460e2007c168.png)

**用户统计特征**

**     笔记统计特征**

![](img/57e740c674098ba736a105b6fbc4ca9a.png)

![](img/8e2387bebcebd53d2b58204bc558bd2b.png)

**场景特征**

**        特征处理**

![](img/65478d566d6f90bee4ed9d84653c8a1c.png)

![](img/076ddeea245263126a3c39747fe5a562.png)

**特征覆盖率的概念**

![](img/0ef4ad2ffeb28b02674b8afa33de2d17.png)

数据服务分三步

![](img/e407156b11c2b391391a6965cc42181f.png)

![](img/b68fa95e93fc2ceb20fcf9304500844f.png)

![](img/051ed598bcfac4ee3241b2e87054cc00.png)

### **粗排**

+   **精排模型的线上推理代价大**

![](img/5f0341a79b9c1520d348afcbcf092c08.png)

![](img/7e6512f61e0f3d7dc65c8c903439bc3b.png)

粗排的三塔模型

![](img/127f252da6de7aa8b690f1b7eb59eee8.png)

![](img/517af4ecdf3673d06df1bca0cde276d7.png)

+   三塔模型的推理

![](img/c22b80c9561d60c9bf794384d219611e.png)

> **用户塔:**只做1次推理物品塔:未命中缓存时需要做推理。

> **交叉塔:**必须做n次推理。

> 上层网络做n次推理，给n个物品打分。