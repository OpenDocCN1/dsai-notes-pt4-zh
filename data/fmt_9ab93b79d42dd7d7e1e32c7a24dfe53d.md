
```
"name": "claude-sonnet-4-5-20250929",
"api": "anthropic-messages",
"input": ["text"],
"contextWindow": 200000,
"maxTokens": 8192
}

]

}

}

}

# 目录结构

```
~/.openclaw/
├── openclaw.json        # OpenClaw 核心配置
├── env                  # 环境变量 (API Key 等)
├── backups/             # 配置备份
└── logs/                # 日志文件 (由 OpenClaw 管理)
```

## 🛡️ 安全建议

> ⚠️ 重要警告：OpenClaw 需要完全的计算机权限，请务必注意安全！

### 部署建议

1. 不要在主工作电脑上部署 - 建议使用专用服务器或虚拟机
2. 使用 AWS/GCP/Azure 免费实例 - 隔离环境更安全
3. Docker 部署 - 提供额外的隔离层

### 权限控制

1. 禁用危险功能（默认已禁用）
2. `security:enable_shell_commands: false`
3. `enable_file_access: false`
4. 启用沙箱模式
5. `security:sandbox_mode: true`
6. 限制允许的用户
7. `channels:telegram:allowed_users`:
   - `"only-your-user-id"`

### API Key 安全

+ 定期轮换 `API Key`
+ 不要在公开仓库中提交配置文件
+ 使用环境变量存储敏感信息

## 使用环境变量

```
export ANTHROPIC_API_KEY="sk-ant-xxx"
export TELEGRAM_BOT_TOKEN="xxx"
```

## ❓ 常见问题

### Q: 安装时提示 Node.js 版本过低？

#### macOS

```
brew install node@22
brew link --overwrite node@22
```

#### Ubuntu/Debian

```
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### Q: 启动后无法连接？

1. 检查配置文件是否正确
2. 运行诊断命令：`openclaw doctor`
3. 查看日志：`openclaw logs`

### Q: Telegram 机器人没有响应？

1. 确认 `Bot Token` 正确
2. 确认 `User ID` 在 `allowed_users` 列表中
3. 检查网络连接（可能需要代理）

### Q: 如何更新到最新版本？

#### 使用 npm 更新

```
npm update -g openclaw
```

#### 或使用配置菜单

```
./config-menu.sh
```

选择 `[7]` 高级设置 → `[7]` 更新 OpenClaw

### Q: 如何备份数据？

#### 手动备份

```
cp -r ~/.openclaw ~/openclaw_backup_$(date +%Y%m%d)
```

#### 使用命令备份

```
openclaw backup
```

### Q: 如何完全卸载？

#### 停止服务

```
openclaw gateway stop
```

#### 卸载程序

```
npm uninstall -g openclaw
```

#### 删除配置（可选）

```
rm -rf ~/.openclaw
```

## 📜 更新日志

### v1.0.0 (2026-01-29)

+ 🎉 首次发布
+ ✨ 支持一键安装部署
+ ✨ 交互式配置菜单
+ ✨ 多模型支持 (`Claude`/`GPT`/`Ollama`)
+ ✨ 多渠道支持 (`Telegram`/`Discord`/`WhatsApp`)
+ ✨ 技能系统
+ ✨ 安全配置

## 📄 许可证

本项目基于 `MIT` 许可证开源。

## 🔗 相关链接

+ [OpenClaw 官网](https://clawd.bot/)
+ [官方文档](https://clawd.bot/docs)
+ 🖥️ `OpenClaw Manager` 桌面版 - 图形界面管理工具
+ [安装工具仓库](https://github.com/miaoxworld/OpenClawInstaller) - 命令行版本
+ [OpenClaw 主仓库](https://github.com/openclaw/openclaw)
+ [社区讨论](https://github.com/miaoxworld/OpenClawInstaller/discussions)
