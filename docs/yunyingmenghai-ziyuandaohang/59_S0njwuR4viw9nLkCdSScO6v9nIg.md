# OpenClaw部署教程

> 来源：[https://yunyingmenghai.feishu.cn/docx/XCoEd2wMpoieGqx1MQqcsIoAnHd](https://yunyingmenghai.feishu.cn/docx/XCoEd2wMpoieGqx1MQqcsIoAnHd)

我来为你详细讲解在带 RTX 4080 的主机上安装 Ubuntu 24.04 LTS Server（无图形界面，最适合跑 AI 服务）的完整流程。

---

## 📝 准备阶段

### 1. 下载镜像

访问 ubuntu.com/download/server 下载：

+   Ubuntu Server 24.04.1 LTS（约 2GB）

+   **架构**：选择 64-bit PC (AMD64)

> **为什么不选 Desktop？** Server 版没有 GUI，开机内存占用仅 500MB 左右，且不会占用 GPU 显存跑桌面特效，16GB 显存全给模型。

### 2. 制作启动 U 盘（8GB+）

**Windows 下**：

+   下载 [Rufus](https://rufus.ie)（轻量，推荐）或 [Ventoy](https://ventoy.net)（支持多镜像）

+   **插入 **U盘 → Rufus 选择镜像 → 分区类型选 GPT（对应 UEFI）→ 开始写入

**Mac/Linux 下**：

```
# 查找 U盘设备名（通常是 /dev/sdb 或 /dev/disk2）
lsblk  # Linux
diskutil list  # Mac

# 写入（替换 sdX 为你的 U盘，会清空数据！）
sudo dd if=ubuntu-24.04.iso of=/dev/sdX bs=4M status=progress
```

---

## ⚙️ BIOS 设置（关键步骤）

开机按 **Del/F2/F12**（根据主板品牌）进入 BIOS：

| 设置项 | 推荐值 | 说明 |
| --- | --- | --- |
| **Secure Boot** | **Disabled** | 必须关闭，否则 NVIDIA 驱动无法加载 |
| **Fast Boot** | **Disabled** | 避免因启动太快识别不到 U盘 |
| Above 4G Decoding | **Enabled** | 大显存显卡（4080 16GB）必须开启 |
| **Re-Size BAR** | **Enabled** | 提升显卡性能 |
| **CSM Support** | **Disabled** | 纯 UEFI 模式更稳定 |
| **Boot Priority** | U盘第一 | 临时调整，装完改回硬盘 |

**保存并重启**，插入制作好的 U盘。

---

## 💿 安装 Ubuntu Server

### 开始安装

1.  从 U盘启动 → 选择 Try or Install Ubuntu Server

1.  语言选 **English****（避免中文路**径导致的潜在问题）或 中文(简体)

1.  **Keyboard layout**：默认即可

### 网络配置

+   插网线：会自动 DHCP 获取 IP

+   WiFi：选择对应 SSID 输入密码

+   **记下显示的 IP 地址**（如 192.168.1.100），后续 SSH 连接用

### 硬盘分区（重要）

**你有 R**TX 4080，说明这是主力机，建议手动分区（假设是 1TB NVMe）：

```
/boot/efi    1GB     FAT32    # EFI 引导
/boot        2GB     ext4     # 内核存放
/swap        32GB    swap     # 内存不足时备用（如果你内存 32GB+，可省略）
/            200GB   ext4     # 系统根目录（剩余空间）
/home        剩余全部 ext4     # 数据、模型、Docker 卷存放
```

**或者简单方案**（推荐新手）：选择 Use an entire disk → 勾选项 Set up this disk as an LVM group（方便后续扩容）

### 用户设置

+   **Your name**: ai（或你喜欢的短用户名）

+   **Server name**: openclaw-server

+   **Username**: ai

+   **Password**: 强密码（务必记住）

**勾选** "Install OpenSSH server"（必须，后续远程管理全靠它）

### 特色软件选择

按 **Space** 勾选：

+   **Docker**（省心，后续不用手动装）

+   **NVIDIA drivers**（如果列表里有，但 Server 版通常没有，我们后面手动装）

等待安装完成（约 5-10 分钟），拔出 U盘，重启。

---

## 🎮 RTX 4080 驱动配置（安装后第一步）

新系统开机后是黑屏命令行，输入用户名密码登录。

### 1. 更新系统并安装 NVIDIA 驱动

```
# 更新软件源
sudo apt update &amp;&amp; sudo apt upgrade -y

# 添加显卡驱动 PPA（可选，但通常更新）
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update

# 查看推荐驱动
ubuntu-drivers devices

# 安装最新稳定版（通常是 nvidia-driver-535 或 550+）
sudo apt install -y nvidia-driver-550 nvidia-utils-550

# 重启
sudo reboot
```

### 2. 验证显卡

重启后登录，输入：

```
nvidia-smi
```

应看到类似输出：

---

## 🐳 安装 Docker + NVIDIA Container Toolkit

这是让 OpenClaw 和 Ollama 能调用 4080 的关键。

```
# 1. 安装 Docker（如果安装时没选）
sudo apt install -y docker.io docker-compose

# 2. 添加用户到 docker 组（免 sudo 运行）
sudo usermod -aG docker $USER
newgrp docker

# 3. 安装 NVIDIA Container Toolkit
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt update
sudo apt install -y nvidia-container-toolkit

# 4. 重启 Docker
sudo systemctl restart docker

# 5. 验证 GPU 在 Docker 中可用
docker run --rm --runtime=nvidia nvidia/cuda:12.0-base nvidia-smi
```

如果看到和刚才一样的显卡信息，说明 Docker 已能调用 4080。

---

## 🚀 部署 OpenClaw 环境

```
# 创建应用目录
mkdir -p ~/apps/openclaw &amp;&amp; cd ~/apps/openclaw

# 下载官方 compose 文件（示例）
wget https://raw.githubusercontent.com/openclaw-org/openclaw/main/docker-compose.yml

# 创建环境变量文件
cat &gt; .env &lt;&lt; EOF
# KIMI API 配置
LLM_PROVIDER=openai
OPENAI_API_KEY=sk-your-moonshot-key
OPENAI_BASE_URL=https://api.moonshot.cn/v1
LLM_MODEL=moonshot-v1-128k

# 本地 Ollama 配置（用于离线模式）
OLLAMA_HOST=http://localhost:11434

# Discord/WhatsApp 等机器人 Token（按需填写）
EOF

# 启动（以后台方式）
docker-compose up -d
```

---

## 🔧 常用维护命令（备忘）

```
# 查看系统状态
htop          # 资源占用
df -h         # 磁盘空间
ip addr       # 查看 IP

# 显卡监控
watch -n 1 nvidia-smi    # 实时监控 4080 负载

# Docker 管理
docker ps                 # 查看运行中的容器
docker logs -f openclaw   # 查看 OpenClaw 日志
docker-compose restart    # 重启服务

# SSH 远程连接（从其他电脑）
ssh ai@192.168.1.100      # 替换为你的 IP
```

---

## ⚠️ 常见问题排查

**Q: 安装时黑屏/卡死？**

+   可能是 nouveau 驱动冲突。在 GRUB 启动界面按 e，在 linux 行末加 nomodeset nouveau.modeset=0，按 F10 启动。

Q: nvidia-smi 显示失败？

+   检查 Secure Boot 是否彻底关闭（部分主板需设置管理员密码才能关）

+   执行 sudo modprobe nvidia 手动加载内核模块

Q: 如何关机但保持网络唤醒（Wake-on-LAN）？

+   长期运行建议保持开机。如需远程开机，需在 BIOS 开启 WoL，并确保网卡支持。

现在你的 4080 盒子已是专业的 AI 服务器，接下来就可以按之前的方案配置 OpenClaw + KIMI + 本地大模型了。需要我提供 docker-compose.yml 的完整配置模板吗？