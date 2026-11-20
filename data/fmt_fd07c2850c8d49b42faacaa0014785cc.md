
# Cursor-Pro: 一键突破Claude-3.7限制的实用工具

> 来源：[https://my.feishu.cn/docx/ZSSgd4AnCoHPhBxLcZQc8kMgnPg](https://my.feishu.cn/docx/ZSSgd4AnCoHPhBxLcZQc8kMgnPg)

大家好，我是老金，相信大家都清楚Cursor免费用户已经屏蔽了Claude3.7的使用。

来自老金交流群的一名小伙伴 - Jskeaaa 开源了本工具，它能突破此限制。

以下内容来自于作者。

---

大家好，今天我要为大家介绍一个非常实用的小工具——Cursor-Pro，它能帮助我们一键突破Claude-3.7的使用限制。作为开发者和AI使用者，我们经常会遇到各种各样的限制，而这个工具正是为解决这些问题而生。

## 什么是Cursor-Pro？

Cursor-Pro是一个专为Cursor IDE设计的清理工具。Cursor是一款基于VS Code的AI增强型IDE，内置了强大的Claude-3.7模型。但在使用过程中，有时候我们会遇到各种限制和问题。Cursor-Pro通过清理特定的缓存文件和历史记录，帮助我们绕过这些限制，提升使用体验。

## 核心功能

这个工具的主要功能包括：

* 删除 `globalStorage/state.vscdb` 和 `globalStorage/state.vscdb.backup` 文件
* 清空 `History` 文件夹内的所有内容
* 删除 `workspaceStorage` 文件夹及其内容

通过清理这些文件，Cursor-Pro能够有效地重置一些使用状态，突破下面的问题。

## 常见问题解决方案

![](img/e853e4ba4e338d7d09667917ae48baa5.png)

当你遇到上图所示的问题时，Cursor-Pro可以帮你一键解决。不过需要注意的是，如果清理后仍然不起作用，很可能是IP污染问题，建议更换IP后再试。

## 项目连接

`https://github.com/Jskeaaa/cursor_pro`

### Windows系统用户

1. 下载打包好的exe文件
2. 右键点击exe文件，选择"以管理员身份运行"
3. 程序会自动完成清理工作

### Mac系统用户

有两种使用方式：

#### 使用Python脚本

1. 打开终端(Terminal)
2. 进入脚本所在目录
3. 执行命令:

```
sudo python3 cursor_clean_mac.py
```

#### 使用打包后的可执行文件

1. 打开终端(Terminal)
2. 进入可执行文件所在目录
3. 执行命令:

```
sudo ./cursor_clean_mac
```

注意：Mac系统需要管理员权限才能运行，因为程序需要访问系统受保护的文件。

### 配置文件说明

工具会自动创建配置文件，你可以根据自己的需要进行修改：

Windows系统配置文件(`config.env`)：

```
[PATHS]
# Cursor用户数据目录路径
base_path = C:\Users\用户名\AppData\Roaming\Cursor\User
```

Mac系统配置文件(`config_mac.env`)：

```
[PATHS]
# Cursor用户数据目录路径
base_path = ~/Library/Application Support/Cursor/User
```

配置文件需要和脚本或可执行文件放在同一目录。在Windows系统中，exe文件会在`dist`文件夹中，配置文件也需要放在相同位置。

## 为什么需要这个工具？

随着AI辅助编程的普及，Cursor IDE已经成为许多开发者的首选工具。然而，内置的Claude-3.7模型虽然强大，但也存在各种使用限制。这些限制有时会影响我们的开发效率和体验。

Cursor-Pro正是为了解决这些问题而生，它通过清理特定的缓存文件和历史记录，帮助我们绕过这些限制，让我们能够更加自由地使用Cursor IDE和Claude-3.7模型。

## 使用技巧和注意事项

1. 使用前最好备份自己的提示词
2. 如果清理后仍然出现问题，请尝试更换IP
3. 定期清理可以保持最佳使用状态
4. 配置文件可以根据实际情况进行调整

## 技术原理简析

本工具的核心原理是通过删除Cursor IDE的特定缓存文件和历史记录，重置使用状态。这些文件记录了我们的使用情况、提示词历史等信息，清理后可以"重新开始"，从而绕过一些限制。

工具采用Python编写，并分别为Windows和Mac系统提供了适配版本。对于不熟悉命令行的用户，Windows版本还提供了打包好的exe文件，使用更加方便。

## 结语

Cursor-Pro虽然是一个小工具，但它解决了我们在使用Cursor IDE和Claude-3.7模型时的一些痛点问题。希望这个工具能够帮助到更多的开发者，让我们能够更加自由地使用AI辅助编程工具，提升开发效率和体验。

如果你觉得这个工具有用，欢迎分享给更多需要的朋友！

---

## 免责声明

本文仅出于学习和研究目的分享，不对使用者因采用本文方法而可能产生的任何后果承担责任。请注意:

1. 本文内容仅供学习和研究使用,不得用于任何商业目的或非法活动。
2. 使用者应当遵守Cursor和Claude的服务条款及相关法律法规。
3. 本文不鼓励也不支持任何形式的滥用、破解或规避付费服务的行为。请尊重知识产权和服务提供商的利益。
4. 如果您决定采用本文描述的方法,即表示您已完全理解并接受本免责声明的全部内容，请合理使用。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
