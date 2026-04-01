# 微学习18 | 技术实操： openEuler 安装

> 来源：[https://datawhaler.feishu.cn/docx/MiiOd1U1boXbsaxLBZocqvQunMw](https://datawhaler.feishu.cn/docx/MiiOd1U1boXbsaxLBZocqvQunMw)

# openEuler配置安装,Linux 基础认知

---

## 目录

1.  掌握 openEuler 镜像的安装流程

1.  理解 Linux 操作系统与 openEuler 的关系

1.  完成基础系统配置

---

## 第一部分：openEuler 镜像安装指南

### 环境准备

+   **硬件要求**：

+   CPU：x86_64 或 ARM64 架构

+   内存：≥ 4GB（推荐 8GB+）

+   磁盘：≥ 20GB 可用空间

+   **工具准备**：

+   下载镜像：[openEuler 官网](https://openeuler.org/zh/download/)

---

## 第二部分：Linux 与 openEuler 的关系详解

### Linux 操作系统核心概念

+   **Linux 内核**：由 Linus Torvalds 创建的开源操作系统内核

+   **Linux 发行版** = Linux 内核 + 软件包管理系统 + 应用软件

### openEuler 的定位

| 特性 | 说明 |
| --- | --- |
| **本质** | 基于 Linux 的企业级开源操作系统 |
| **起源** | 源自华为 EulerOS，2020 年开源 |
| **特点** | 针对 ARM/x86 架构优化，支持高并发、分布式存储等企业级场景 |
| **生态** | 加入 OpenAtom 基金会，兼容主流开源工具链 |

---

## 第三部分：常见问题解答

### Q1：为什么选择 openEuler 而不是其他 Linux？

+   **企业级支持**：针对云计算/边缘计算深度优化

+   **自主可控**：国内主导的开源生态

+   **兼容性**：完美兼容 K8s/Docker 等云原生技术

## 第四部分：图形化操作页面安装

**登陆**

```
login：root
password：xxx
# 登录成功
```

在管理员账户下安装dde桌面环境

```
yum install dde
```

**选择 yes **

```
Is This ok[y/N]:y
```

**出现以下即安装完成**

```
complete！
```

安装完成之后，我们可以对其进行设置以图形化界面的方式来启动操作系统

```
systemctl set-default graphical.target
```

**使用命令重启系统**

```
reboot
#重启
```

### 成功~

---

## 第五部分：安装中文输入法

将系统的语言环境设置为简体中文，并使用UTF-8编码

```
LANG=&quot;zh_CN.UTF-8&quot;
```

**安装ibus 中文输入法**

```
sudo yum install ibus-libpinyin
```

**在终端中输入****ibus-setup****配置输入法快捷键等操作，但是缺少GTK的支持，**无法完成中文输入。因此需要安装以下软件：

```
[root@comput ~]# yum install cjkuni*  #字体显示包
[root@comput]# yum install -y \
ibus \
ibus-libpinyin \
ibus-gtk2 \
ibus-gtk3 \
im-chooser \
gtk2-immodule-xim \
gtk3-immodule-xim  #输入法相关包
```

**重新打开一个终端，再输入******ibus-daemon -drx**，就可以在桌面调用输**入法的切换图标，此时切换到智能拼音的输入方式，就可以在正确的使用智能拼音输入汉字

---

## 扩展

+   [openEuler 官方文档](https://docs.openeuler.org/zh/)

+   Linux 哲学三要素：

1.  一切皆文件

1.  小工具组合完成复杂任务

1.  文本化配置