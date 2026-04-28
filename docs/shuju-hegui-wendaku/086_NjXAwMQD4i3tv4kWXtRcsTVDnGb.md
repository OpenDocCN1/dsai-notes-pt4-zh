# 自带应用升级

> 来源：[https://docs.feishu.cn/docx/FQdJdIzUcojcMHx2NM7cVMi1nDd](https://docs.feishu.cn/docx/FQdJdIzUcojcMHx2NM7cVMi1nDd)

**-问：**手机自带的系统级应用，为什么不需要走权限申请？

**-答1：****预授权**，你在权限管理可以看。adb shell dumpsys package 包名也可以看。

**-答2：****因为很多**应用就是系统本身，但是口子也没你想的那么大，每个特殊需求都需要泰尔认可。

**-答3：**因为我们这边是直接把各种包拿来测试，还是会看到授权弹窗的。

**-追问：**我怎么没看到手机系统的语音应用弹窗申请麦克风权限。

**-答4：**在原生系统，谷歌给语音助手系统级别权限，他不仅有语音权限，还有各种数据的权限。在开机引导的时候告诉你了，你可以不开。

**-答5：**谷歌有清单，清单之外的，OEM（Original Entrusted Manufacture）的汉语意思是“原始设备制造商”，基本含义是定牌生产合作，俗称“代工”。要谷歌豁免。

**-答6：**还有CTS（全称是Compatibility Test Suite，也就是常说的谷歌兼容性认证）要求。