# --stylize（风格化）

> 来源：[https://my.feishu.cn/docx/SA1Dd1Qkwo1UgUx1vVKctxFznzb](https://my.feishu.cn/docx/SA1Dd1Qkwo1UgUx1vVKctxFznzb)

Midjourney Bot 经过训练可以生成具有艺术色彩、构图和形式的图像。--stylize或--s参数会影响此训练的应用强度。低风格化值生成的图像与提示非常匹配，但艺术性较差。高风格化值生成的图像非常具有艺术性，但与提示的联系较少。

**--stylize**的默认值为 100，使用当前模型时接受整数值 0–1000

---

## 常见风格化设置

### Stylize 对模型版本 6 的影响

提示示例：**/imagine prompt** child's drawing of a cat --s 100

![](img/e3e910ed4d68370ed031456ffc938004.png)

提示示例：**/imagine prompt** colorful risograph of a fig --s 100

![](img/9bf54e2fc5693614411b9cf23485f5e2.png)

---

## 如何使用风格化

### 使用风格化参数

在 D**iscord 或网站的**想象栏中，将--stylize <value>或添加--s <value>到提示的末尾：

![](img/c005fb56d068ee6300f4383e23b18b2f.png)

### 使用 Discord 设置命令

**/settings**从菜单中输入并选择您喜欢的风格化值。

**🖌️ Style Low **🖌**️ Style Med 🖌**️** Style High 🖌️** Style Very High

### 网站设置

您可以使用想象栏中的“设置”按钮设置默认风格：

![](img/7bac68b183697af0cfbcf9ba71b07daa.png)

向左调整滑块可获得较低的值，向右调整滑块可获得较高的值。

**您在此处设置的值将**应用于所有提示，除非**您在单**个提示中使用--stylize或参数指定不同的值。--s

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>