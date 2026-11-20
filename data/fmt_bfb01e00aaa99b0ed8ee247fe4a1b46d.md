
# ComfyUI更新版本方法

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/GIiCdmxOkoTPKVx0k2WcgqR2nuf](https://xatc0v8uz5m.feishu.cn/docx/GIiCdmxOkoTPKVx0k2WcgqR2nuf)

> **注意：更新ComfyUI版本会导致部分插件不可使用，请自行判断是否升级！！！**

建议保存数据，使用官方版创建实例来更新。

如果选择升级，一定要备份当前实例，一定要保存镜像。

![](img/9864b2d7eb6415b6abff8ee6891c9f8e.png)

ComfyUI官方版本举例：

打开ComfyUI页面，点击顶部工具栏中的【日志与终端】，切换终端模式，输入下方代码

![](img/20b04be6fce48ff94e32219acff7957c.png)

## 准备环境

```bash
cd /root/ComfyUI/.ci/update_windows
pip install pygit2
```

![](img/bbaa1c53c78d321da9da58e728c99574.png)

## 升级方法

### 升级到最新稳定版

```bash
python update.py ../../../ComfyUI/ --stable
```

![](img/c0b029fd0a0af37cc5ae4179b4592293.png)

### 升级到最新开发版

```bash
python update.py ../../../ComfyUI/
```

![](img/f6967ee5d058c964467dedea52a643a6.png)

## 启动问题修复

如更新后无法启动，请按照以下步骤进行。

更新到最新版本的ComfyUI后，执行这3个命令，可以修复无法启动问题。

```bash
if ! grep 'enable-cors-header' /usr/bin/comfyui-startup.env; then
sed -i 's#ARGS="#ARGS="--enable-cors-header #g' /usr/bin/comfyui-startup.env ;
fi
supervisorctl restart comfyui
```

![](img/063e922a6ea87d36820216815c51672c.png)
