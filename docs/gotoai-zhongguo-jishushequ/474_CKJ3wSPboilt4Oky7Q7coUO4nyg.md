# MimiClaw 智能硬件10美金方案

> 来源：[https://gotoaiworld.feishu.cn/docx/OAUsd04kao4EEKxQ09XcsBz8nu2](https://gotoaiworld.feishu.cn/docx/OAUsd04kao4EEKxQ09XcsBz8nu2)

MimiClaw: Run OpenClaw on a $5 chip. No OS(Linux). No Node.js. No Mac mini. No Raspberry Pi. No VPS. Hardware agents OS.

MimiClaw 为嵌入式硬件重新实现了核心 AI Agent 架构 — 没有 Linux，没有服务器，只有一颗 $5 的芯片。

https://github.com/memovai/mimiclaw?tab=readme-ov-file

# MimiClaw: Pocket AI Assistant on a $5 Chip

![](img/a6da394d96888457515c7f4866da8add.png)

[English](https://github.com/memovai/mimiclaw/blob/main/README.md) | [中文](https://github.com/memovai/mimiclaw/blob/main/README_CN.md) | [日本語](https://github.com/memovai/mimiclaw/blob/main/README_JA.md)

The world's first AI assistant(OpenClaw) on a $5 chip. No Linux. No Node.js. Just pure C

MimiClaw turns a tiny ESP32-S3 board into a personal AI assistant. Plug it into USB power, connect to WiFi, and talk to it through Telegram — it handles any task you throw at it and evolves over time with local memory — all on a chip the size of a thumb.

## Meet MimiClaw

+   Tiny — No Linux, no Node.js, no bloat — just pure C

+   Handy — Message it from Telegram, it handles the rest

+   Loyal — Learns from memory, remembers across reboots

+   Energetic — USB power, 0.5 W, runs 24/7

+   Lovable — One ESP32-S3 board, $5, nothing else

## How It Works

[h](https://github.com/memovai/mimiclaw/raw/main/assets/mimiclaw.png)ttps://github.com/memovai/mimiclaw/raw/main/assets/mimiclaw.png

You send a message on Telegram. The ESP32-S3 picks it up over WiFi, feeds it into an agent loop — the LLM thinks, calls tools, reads memory — and sends the reply back. Supports both Anthropic (Claude) and OpenAI (GPT) as providers, switchable at runtime. Everything runs on a single $5 chip with all your data stored locally on flash.

## Quick Start

### What You Need

+   An ESP32-S3 dev board with 16 MB flash and 8 MB PSRAM (e.g. Xiaozhi AI board, ~$10)

+   A USB Type-C cable

+   [A Telegram](https://t.me/BotFather) bot token — talk to @BotFather on Telegram to create one

+   An Anthropic API key — from console.anthropic.com, or an OpenAI API key — from platform.openai.com

### Install

You need ESP-IDF v5.5+ installed first:# https://docs.espressif.com/projects/esp-idf/en/v5.5.2/esp32s3/get-started/

git clone https://github.com/memovai/mimiclaw.git

cd mimiclaw

idf.py set-target esp32s3

Ubuntu Install

Recommended baseline:

+   Ubuntu 22.04/24.04

+   Python >= 3.10

+   CMake >= 3.16

+   Ninja >= 1.10

+   Git >= 2.34

+   flex >= 2.6

+   bison >= 3.8

+   gperf >= 3.1

+   dfu-util >= 0.11

+   libusb-1.0-0, libffi-dev, libssl-dev

Install and build on Ubuntu:

sudo apt-get update

sudo apt-get install -y git wget flex bison gperf python3 python3-pip python3-venv \

cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0

./scripts/setup_idf_ubuntu.sh

./scripts/build_ubuntu.sh

macOS Install

Recommended baseline:

+   macOS 12/13/14

+   Xcode Command Line Tools

+   Homebrew

+   Python >= 3.10

+   CMake >= 3.16

+   Ninja >= 1.10

+   Git >= 2.34

+   flex >= 2.6

+   bison >= 3.8

+   gperf >= 3.1

+   dfu-util >= 0.11

+   libusb, libffi, openssl

Install and build on macOS:

xcode-select --install

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

./scripts/setup_idf_macos.sh

./scripts/build_macos.sh

### Configure

MimiClaw uses a two-layer config system: build-time defaults in mimi_secrets.h, with runtime overrides via the serial CLI. CLI values are stored in NVS flash and take priority over build-time values.

cp main/mimi_secrets.h.example main/mimi_secrets.h

Edit main/mimi_secrets.h:

#define MIMI_SECRET_WIFI_SSID       "YourWiFiName"

#define MIMI_SECRET_WIFI_PASS       "YourWiFiPassword"

#define MIMI_SECRET_TG_TOKEN        "123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11"

#define MIMI_SECRET_API_KEY         "sk-ant-api03-xxxxx"

#define MIMI_SECRET_MODEL_PROVIDER  "anthropic"     // "anthropic" or "openai"

#define MIMI_SECRET_SEARCH_KEY      ""              // optional: Brave Search API key

#define MIMI_SECRET_PROXY_HOST      ""              // optional: e.g. "10.0.0.1"

#define MIMI_SECRET_PROXY_PORT      ""              // optional: e.g. "7897"

Then build and flash:

# Clean build (required after any mimi_secrets.h change)

idf.py fullclean && idf.py build

# Find your serial port

ls /dev/cu.usb*          # macOS

ls /dev/ttyACM*          # Linux# Flash and monitor (replace PORT with your port)# USB adapter: likely /dev/cu.usbmodem11401 (macOS) or /dev/ttyACM0 (Linux)

idf.py -p PORT flash monitor

> Important: Plug into the correct USB port! Most ESP32-S3 boards have two USB-C ports. You must use the one labeled USB (native USB Serial/JTAG), not the one labeled COM (external UART bridge). Plugging into the wrong port will cause flash/monitor failures.

> Show reference photo

> [https://github.com/memovai/mimic](https://github.com/memovai/mimiclaw/raw/main/assets/esp32s3-usb-port.jpg)law/raw/main/assets/esp32s3-usb-port.jpg

### CLI Commands (via UART/COM port)

Connect via serial to configure or debug. Config commands let you change settings without recompiling — just plug in a USB cable anywhere.

Runtime config (saved to NVS, overrides build-time defaults):

```
mimi&gt; wifi_set MySSID MyPassword   # change WiFi network
mimi&gt; set_tg_token 123456:ABC...   # change Telegram bot token
mimi&gt; set_api_key sk-ant-api03-... # change API key (Anthropic or OpenAI)
mimi&gt; set_model_provider openai    # switch provider (anthropic|openai)
mimi&gt; set_model gpt-4o             # change LLM model
mimi&gt; set_proxy 127.0.0.1 7897  # set HTTP proxy
mimi&gt; clear_proxy                  # remove proxy
mimi&gt; set_search_key BSA...        # set Brave Search API key
mimi&gt; config_show                  # show all config (masked)
mimi&gt; config_reset                 # clear NVS, revert to build-time defaults
```

Debug & maintenance:

```
mimi&gt; wifi_status              # am I connected?
mimi&gt; memory_read              # see what the bot remembers
mimi&gt; memory_write &quot;content&quot;   # write to MEMORY.md
mimi&gt; heap_info                # how much RAM is free?
mimi&gt; session_list             # list all chat sessions
mimi&gt; session_clear 12345      # wipe a conversation
mimi&gt; heartbeat_trigger           # manually trigger a heartbeat check
mimi&gt; cron_start                  # start cron scheduler now
mimi&gt; restart                     # reboot
```

### USB (JTAG) vs UART: Which Port for What

Most ESP32-S3 dev boards expose two USB-C ports:

> REPL requires the UART (COM) port. The USB (JTAG) port does not support interactive REPL input.

Port details & recommended workflow

The ESP-IDF console/REPL is configured to use UART by default (CONFIG_ESP_CONSOLE_UART_DEFAULT=y).

If you have both ports connected simultaneously:

+   USB (JTAG) handles flash/download and provides secondary serial output

+   UART (COM) provides the primary interactive console for the REPL

+   macOS: both appear as /dev/cu.usbmodem* or /dev/cu.usbserial-* — run ls /dev/cu.usb* to identify

+   Linux: USB (JTAG) → /dev/ttyACM0, UART → /dev/ttyUSB0

Recommended workflow:

# Flash via USB (JTAG) port

idf.py -p /dev/cu.usbmodem11401 flash

# Open REPL via UART (COM) port

idf.py -p /dev/cu.usbserial-110 monitor

# or use any serial terminal: screen, minicom, PuTTY at 115200 baud

## Memory

MimiClaw stores everything as plain text files you can read and edit:

## Tools

MimiClaw supports tool calling for both Anthropic and OpenAI — the LLM can call tools during a conversation and loop until the task is done (ReAct pattern).

To enable web search, set a Brave Search API key via MIMI_SECRET_SEARCH_KEY in mimi_secrets.h.

## Cron Tasks

MimiClaw has a built-in cron scheduler that lets the AI schedule its own tasks. The LLM can create recurring jobs ("every N seconds") or one-shot jobs ("at unix timestamp") via the cron_add tool. When a job fires, its message is injected into the agent loop — so the AI wakes up, processes the task, and responds.

Jobs are persisted to SPIFFS (cron.json) and survive reboots. Example use cases: daily summaries, periodic reminders, scheduled check-ins.

## Heartbeat

The heartbeat service periodically reads HEARTBEAT.md from SPIFFS and checks for actionable tasks. If uncompleted items are found (anything that isn't an empty line, a header, or a checked - [x] box), it sends a prompt to the agent loop so the AI can act on them autonomously.

This turns MimiClaw into a proactive assistant — write tasks to HEARTBEAT.md and the bot will pick them up on the next heartbeat cycle (default: every 30 minutes).

## Also Included

+   WebSocket gateway on port 18789 — connect from your LAN with any WebSocket client

+   OTA updates — flash new firmware over WiFi, no USB needed

+   Dual-core — network I/O and AI processing run on separate CPU cores

+   HTTP proxy — CONNECT tunnel support for restricted networks

+   Multi-provider — supports both Anthropic (Claude) and OpenAI (GPT), switchable at runtime

+   Cron scheduler — the AI can schedule its own recurring and one-shot tasks, persisted across reboots

+   Heartbeat — periodically checks a task file and prompts the AI to act autonomously

+   Tool use — ReAct agent loop with tool calling for both providers

## For Developers

Technical details live in the docs/ folder:

+   docs/ARCHITECTURE.md — system design, module map, task layout, memory budget, protocols, flash partitions

+   [docs/TODO.md](https://github.com/memovai/mimiclaw/blob/main/docs/TODO.md) — feature gap tracker and roadmap

$5 芯片上的 AI 助理（OpenClaw）。没有 Linux，没有 Node.js，纯 C。

MimiClaw 把一块小小的 ESP32-S3 开发板变成你的私人 AI 助理。插上 USB 供电，连上 WiFi，通过 Telegram 跟它对话 — 它能处理你丢给它的任何任务，还会随时间积累本地记忆不断进化 — 全部跑在一颗拇指大小的芯片上。

## 认识 MimiClaw

+   小巧 — 没有 Linux，没有 Node.js，没有臃肿依赖 — 纯 C

+   好用 — 在 Telegram 发消息，剩下的它来搞定

+   忠诚 — 从记忆中学习，跨重启也不会忘

+   能干 — USB 供电，0.5W，24/7 运行

+   可爱 — 一块 ESP32-S3 开发板，$5，没了

## 工作原理

![](img/429ab439e8b474ed1b82c7ad5ae655c6.png)

你在 Telegram 发一条消息，ESP32-S3 通过 WiFi 收到后送进 Agent 循环 — LLM 思考、调用工具、读取记忆 — 再把回复发回来。同时支持 Anthropic (Claude) 和 OpenAI (GPT) 两种提供商，运行时可切换。一切都跑在一颗 $5 的芯片上，所有数据存在本地 Flash。

## 快速开始

### 你需要

+   一块 ESP32-S3 开发板，16MB Flash + 8MB PSRAM（如小智 AI 开发板，~¥30）

+   一根 USB Type-C 数据线

+   一个 Telegram Bot T[oken — 在 T](https://t.me/BotFather)elegram 找 @BotFather 创建

+   一个 Anthropic API Key — 从 console.anthropic.com 获取，或一个 OpenAI API Key — 从 platform.openai.com 获取

### 安装

需要先安装 ESP-IDF v5.5+:# https://docs.espressif.com/projects/esp-idf/en/v5.5.2/esp32s3/get-started/

git clone https://github.com/memovai/mimiclaw.git

cd mimiclaw

idf.py set-target esp32s3

Ubuntu 安装

建议基线：

+   Ubuntu 22.04/24.04

+   Python >= 3.10

+   CMake >= 3.16

+   Ninja >= 1.10

+   Git >= 2.34

+   flex >= 2.6

+   bison >= 3.8

+   gperf >= 3.1

+   dfu-util >= 0.11

+   libusb-1.0-0、libffi-dev、libssl-dev

Ubuntu 安装与构建：

sudo apt-get update

sudo apt-get install -y git wget flex bison gperf python3 python3-pip python3-venv \

cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0

./scripts/setup_idf_ubuntu.sh

./scripts/build_ubuntu.sh

macOS 安装

建议基线：

+   macOS 12/13/14

+   Xcode Command Line Tools

+   Homebrew

+   Python >= 3.10

+   CMake >= 3.16

+   Ninja >= 1.10

+   Git >= 2.34

+   flex >= 2.6

+   bison >= 3.8

+   gperf >= 3.1

+   dfu-util >= 0.11

+   libusb、libffi、openssl

macOS 安装与构建：

xcode-select --install

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

./scripts/setup_idf_macos.sh

./scripts/build_macos.sh

### 配置

MimiClaw 使用两层配置：mimi_secrets.h 提供编译时默认值，串口 CLI 可在运行时覆盖。CLI 设置的值存在 NVS Flash 中，优先级高于编译时值。

cp main/mimi_secrets.h.example main/mimi_secrets.h

编辑 main/mimi_secrets.h：

#define MIMI_SECRET_WIFI_SSID       "你的WiFi名"

#define MIMI_SECRET_WIFI_PASS       "你的WiFi密码"

#define MIMI_SECRET_TG_TOKEN        "123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11"

#define MIMI_SECRET_API_KEY         "sk-ant-api03-xxxxx"

#define MIMI_SECRET_MODEL_PROVIDER  "anthropic"     // "anthropic" 或 "openai"

#define MIMI_SECRET_SEARCH_KEY      ""              // 可选：Brave Search API key

#define MIMI_SECRET_PROXY_HOST      "10.0.0.1"      // 可选：代理地址

#define MIMI_SECRET_PROXY_PORT      "7897"           // 可选：代理端口

然后编译烧录：

# 完整编译（修改 mimi_secrets.h 后必须 fullclean）

idf.py fullclean && idf.py build

# 查找串口

ls /dev/cu.usb*          # macOS

ls /dev/ttyACM*          # Linux# 烧录并监控（将 PORT 替换为你的串口）# USB 转接器：大概率是 /dev/cu.usbmodem11401（macOS）或 /dev/ttyACM0（Linux）

idf.py -p PORT flash monitor

> 注意：请插对 USB 口！ 大多数 ESP32-S3 开发板有两个 Type-C 接口，必须插标有 USB 的那个口（原生 USB Serial/JTAG），不要插标有 COM 的口（外部 UART 桥接）。插错口会导致烧录/监控失败。

> 查看参考图片

![](img/c31f30c1940ae166139fd3063d5691b2.png)

### 代理配置（国内用户）

在国内需要代理才能访问 Telegram 和 Anthropic API。MimiClaw 内置 HTTP CONNECT 隧道支持。

前提：局域网内有一个支持 HTTP CONNECT 的代理（Clash Verge、V2Ray 等），并开启了「允许局域网连接」。

可以在 mimi_secrets.h 中编译时设置，也可以通过串口 CLI 随时修改：

```
mimi&gt; set_proxy 192.168.1.83 7897   # 设置代理
mimi&gt; clear_proxy                    # 清除代理
```

> 提示：确保 ESP32-S3 和代理机器在同一局域网。Clash Verge 在「设置 → 允许局域网」中开启。

### CLI 命令（通过 UART/COM 口连接）

通过串口连接即可配置和调试。配置命令让你无需重新编译就能修改设置 — 随时随地插上 USB 线就能改。

运行时配置（存入 NVS，覆盖编译时默认值）：

```
mimi&gt; wifi_set MySSID MyPassword   # 换 WiFi
mimi&gt; set_tg_token 123456:ABC...   # 换 Telegram Bot Token
mimi&gt; set_api_key sk-ant-api03-... # 换 API Key（Anthropic 或 OpenAI）
mimi&gt; set_model_provider openai    # 切换提供商（anthropic|openai）
mimi&gt; set_model gpt-4o             # 换模型
mimi&gt; set_proxy 192.168.1.83 7897  # 设置代理
mimi&gt; clear_proxy                  # 清除代理
mimi&gt; set_search_key BSA...        # 设置 Brave Search API Key
mimi&gt; config_show                  # 查看所有配置（脱敏显示）
mimi&gt; config_reset                 # 清除 NVS，恢复编译时默认值
```

调试与运维：

```
mimi&gt; wifi_status              # 连上了吗？
mimi&gt; memory_read              # 看看它记住了什么
mimi&gt; memory_write &quot;内容&quot;       # 写入 MEMORY.md
mimi&gt; heap_info                # 还剩多少内存？
mimi&gt; session_list             # 列出所有会话
mimi&gt; session_clear 12345      # 删除一个会话
mimi&gt; heartbeat_trigger           # 手动触发一次心跳检查
mimi&gt; cron_start                  # 立即启动 cron 调度器
mimi&gt; restart                     # 重启
```

### USB (JTAG) 与 UART：哪个口做什么

大多数 ESP32-S3 开发板有 两个 USB-C 口：

> REPL 必须连接 UART（COM）口。 USB（JTAG）口不支持交互式 REPL 输入。

端口详情与推荐工作流

ESP-IDF 控制台默认配置为 UART 输出（CONFIG_ESP_CONSOLE_UART_DEFAULT=y）。

同时连接两个口时：

+   USB（JTAG）口负责烧录/下载，并提供辅助串口输出

+   UART（COM）口提供主要的交互式控制台，用于 REPL

+   macOS 下两个口都会显示为 /dev/cu.usbmodem* 或 /dev/cu.usbserial-*，用 ls /dev/cu.usb* 区分

+   Linux 下 USB（JTAG）通常是 /dev/ttyACM0，UART 通常是 /dev/ttyUSB0

推荐工作流：

# 通过 USB（JTAG）口烧录

idf.py -p /dev/cu.usbmodem11401 flash

# 通过 UART（COM）口打开 REPL

idf.py -p /dev/cu.usbserial-110 monitor

# 或使用任意串口工具：screen、minicom、PuTTY，波特率 115200

## 记忆

MimiClaw 把所有数据存为纯文本文件，可以直接读取和编辑：

## 工具

MimiClaw 同时支持 Anthropic 和 OpenAI 的工具调用 — LLM 在对话中可以调用工具，循环执行直到任务完成（ReAct 模式）。

启用网页搜索需要在 mimi_secrets.h 中设置 Brave Search API key（MIMI_SECRET_SEARCH_KEY）。

## 定时任务（Cron）

MimiClaw 内置 cron 调度器，让 AI 可以自主安排任务。LLM 可以通过 cron_add 工具创建周期性任务（"每 N 秒"）或一次性任务（"在某个时间戳"）。任务触发时，消息会注入到 Agent 循环 — AI 自动醒来、处理任务并回复。

任务持久化存储在 SPIFFS（cron.json），重启后不会丢失。典型用途：每日总结、定时提醒、定期巡检。

## 心跳（Heartbeat）

心跳服务会定期读取 SPIFFS 上的 HEARTBEAT.md，检查是否有待办事项。如果发现未完成的条目（非空行、非标题、非已勾选的 - [x]），就会向 Agent 循环发送提示，让 AI 自主处理。

这让 MimiClaw 变成一个主动型助理 — 把任务写入 HEARTBEAT.md，机器人会在下一次心跳周期自动拾取执行（默认每 30 分钟）。

## 其他功能

+   WebSocket 网关 — 端口 18789，局域网内用任意 WebSocket 客户端连接

+   OTA 更新 — WiFi 远程刷固件，无需 USB

+   双核 — 网络 I/O 和 AI 处理分别跑在不同 CPU 核心

+   HTTP 代理 — CONNECT 隧道，适配受限网络

+   多提供商 — 同时支持 Anthropic (Claude) 和 OpenAI (GPT)，运行时可切换

+   定时任务 — AI 可自主创建周期性和一次性任务，重启后持久保存

+   心跳服务 — 定期检查任务文件，驱动 AI 自主执行

+   工具调用 — ReAct Agent 循环，两种提供商均支持工具调用

## 开发者

技术细节在 docs/ 文件夹：

+   docs/ARCHITECTURE.md — 系统设计、模块划分、任务布局、内存分配、协议、Flash 分区

+   [docs/TODO.md](https://github.com/memovai/mimiclaw/blob/main/docs/TODO.md) — 功能差距和路线图