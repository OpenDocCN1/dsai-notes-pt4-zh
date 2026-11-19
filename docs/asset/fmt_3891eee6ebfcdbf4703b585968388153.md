
# 网络安全魔法

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/LY8GdpEpFojAWPxtuR2csReOnse](https://xatc0v8uz5m.feishu.cn/docx/LY8GdpEpFojAWPxtuR2csReOnse)

## 服务器安全加固标准流程 (Ubuntu 24.04 专用版)

> 核心原则：先开窗（改防火墙/配置），后关门（重启服务）。全过程保持旧 SSH 窗口不关闭，直到新窗口连接成功。

### 第一阶段：X-UI 面板加固 (Web端)

先改面板，因为这不影响 SSH 连接，风险最小。

#### 修改端口与路径

在面板设置中，将端口 `54443` 改为 `34876`。

将 URL 根路径改为 `/HWBF2026/`。

#### 保存并重启面板

点击保存配置 -> 重启面板。

#### 验证

浏览器访问 `http://IP:34876/HWBF2026/` 确认能打开。

### 第一步：安装防火墙软件

```bash
apt update && apt install ufw -y
```

等待它跑完，直到出现命令提示符。

### 第二步：添加规则 (重做一遍，因为之前都没生效)

顺序至关重要！先开门，再启动！

#### 放行 SSH 端口 (必须做，否则立马锁死自己)

```bash
ufw allow 49123/tcp
```

#### 放行 面板端口

```bash
ufw allow 34876/tcp
```

#### 放行 节点端口 (443)

```bash
ufw allow 443/tcp
```

### 第三步：启动防火墙

输入以下命令启用：

```bash
ufw enable
```

系统会问你：`Command may disrupt existing ssh connections... (y|n)`？

输入 `y` 并回车。

### 第四步：检查状态 (最终确认)

输入：

```bash
ufw status
```

**成功标志：** 如果你看到类似下面的表格，状态是 `Active`，且列出了 `49123`、`34876`、`443` 且都是 `ALLOW`，那就大功告成了。

```text
Status: active

To                         Action      From
--                         ------      ----
49123/tcp                  ALLOW       Anywhere
34876/tcp                  ALLOW       Anywhere
443/tcp                    ALLOW       Anywhere
...
```

---

### 关于 Evoxt 后台那个防火墙 (`Image_668d82`)

你在网页后台看到的那个蓝色按钮 是外部硬件防火墙。

- 当前状态：关闭 (并未启动)。
- 建议：既然你已经在 `Linux` 内部装了 `ufw`，外部这个不要动，保持关闭。
- 理由：两个防火墙同时开容易打架，而且网页版配置如果不小心，更容易把你自己锁在外面。用 `Linux` 内部的 `ufw` 就足够了。

### 第二阶段：配置 UFW 防火墙 (门禁卡)

**最重要的一步！** 在修改 SSH 服务之前，必须先放行所有新端口。

#### 放行 SSH 新端口 (防自锁核心)

```bash
ufw allow 49123/tcp
```

#### 放行 X-UI 面板新端口

```bash
ufw allow 34876/tcp
```

#### 放行 VLESS 节点端口 (保业务)

```bash
ufw allow 443/tcp
# (如果你还有其他节点如 23628，也可以放行，没有则不跑) # ufw allow 23628/tcp 
```

#### 启动防火墙 (如果是第一次)

```bash
ufw enable
```

### 第三阶段：修改 SSH 配置文件 (改门牌)

这里是你草稿中顺序颠倒的地方。**必须先改文件，再重启服务。**

#### 编辑配置文件

```bash
nano /etc/ssh/sshd_config
```

#### 修改动作

找到 `#Port 22`。

删掉 `#`，改为 `Port 49123`。

#### 保存退出

`Ctrl + O` -> `Enter` -> `Ctrl + X`。

### 第四阶段：解决 Ubuntu 24.04 兼容性 & 重启服务

这是针对新版系统的关键补丁，否则修改不生效。

#### 停用 Socket 监听 (解除 22 端口锁定)

```bash
systemctl stop ssh.socket
systemctl disable ssh.socket
```

#### 重启 SSH 服务 (应用新配置)

```bash
systemctl restart ssh
```

#### 验证端口状态

```bash
ss -tulpn | grep ssh
```

**检查点：** 必须看到 `:49123` 才算成功。如果还是 `:22`，说明没改好，不要断开旧链接。

### 第五阶段：最终验证 (换锁)

保持 当前旧 SSH 窗口不关闭。

新建 一个 SSH 连接，端口填 `49123`。

连接成功后，才关闭旧窗口。
