
# Langbot

> 来源：[https://my.feishu.cn/docx/Tvm4d6WRkowWO9x1QAAcqlfvnFg](https://my.feishu.cn/docx/Tvm4d6WRkowWO9x1QAAcqlfvnFg)

## 克隆主仓库

```
git clone https://github.com/RockChinQ/LangBot
cd LangBot
```

![](img/d0d9827d078c38e26ceb5c581645ee67.png)

## 构建前端

```
# 构建前端，需要 NodeJS >= 22
cd web
npm install && npm run build
cd ..
```

## 创建虚拟环境

在一些使用系统包管理器管理 Python 依赖的 Linux 系统上，可能会出现依赖冲突，故请使用 `venv` 创建虚拟环境。Windows 用户可以跳过此步骤。

```
python -m venv venv
source venv/bin/activate
```

![](img/512c579b29cca481b91c542580a32261.png)

## 安装依赖

```
pip install -r requirements.txt
```

![](img/f1b6feda081566fbe2ee0fb6a17701bf.png)

## 运行主程序并生成配置文件

运行一次主程序，生成配置文件：

```
python main.py
```

显示以下信息正常：

```
(venv) (AI) [root@VM-4-8-opencloudos LangBot]# python main.py

 _                   ___      _   
| |   __ _ _ _  __ _| _ ) ___| |_ 
| |__/ _` | ' \/ _` | _ \/ _ \  _|
|____\__,_|_||_\__, |___/\___/\__|
               |___/              

⭐️开源地址: https://github.com/RockChinQ/LangBot
📖文档地址: https://docs.langbot.app

以下文件不存在，已自动生成：
- plugins/__init__.py
- plugins/plugins.json
- data/config/command.json
- data/config/pipeline.json
- data/config/platform.json
- data/config/provider.json
- data/config/system.json
- data/scenario/default.json
[03-05 13:01:35.709] manager.py (78) - [INFO] : 初始化平台适配器 0: aiocqhttp
[03-05 13:01:36.362] show_notes.py (26) - [INFO] : 当前版本：v3.4.10
[2025-03-05 13:01:36 +0800] [1907759] [INFO] Running on http://0.0.0.0:2280 (CTRL + C to quit)
[2025-03-05 13:01:36 +0800] [1907759] [INFO] Running on http://0.0.0.0:5300 (CTRL + C to quit)
[03-05 13:01:36.906] app.py (172) - [INFO] : =======================================
[03-05 13:01:36.907] app.py (172) - [INFO] : ✨ 您可通过以下方式访问管理面板
[03-05 13:01:36.907] app.py (172) - [INFO] : 
[03-05 13:01:36.907] app.py (172) - [INFO] : 🏠 本地地址：http://127.0.0.1:5300/
[03-05 13:01:36.907] app.py (172) - [INFO] : 🌐 公网地址：http://0.0.0.0:5300/
[03-05 13:01:36.907] app.py (172) - [INFO] : 
[03-05 13:01:36.908] app.py (172) - [INFO] : 📌 如果您在容器中运行此程序，请确保容器的 5300 端口已对外暴露
[03-05 13:01:36.908] app.py (172) - [INFO] : 🔗 若要使用公网地址访问，请阅读以下须知
[03-05 13:01:36.908] app.py (172) - [INFO] :    1. 公网地址仅供参考，请以您的主机公网 IP 为准；
[03-05 13:01:36.908] app.py (172) - [INFO] :    2. 要使用公网地址访问，请确保您的主机具有公网 IP，并且系统防火墙已放行 5300 端口；
[03-05 13:01:36.908] app.py (172) - [INFO] : 
[03-05 13:01:36.909] app.py (172) - [INFO] : 🤯 WebUI 仍处于 Beta 测试阶段，如有问题或建议请反馈到 https://github.com/RockChinQ/LangBot/issues
[03-05 13:01:36.909] app.py (172) - [INFO] : =======================================
```
