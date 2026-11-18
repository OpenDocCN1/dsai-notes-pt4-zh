
# Auto-import 自动导入

> 来源：[https://kcn9ut4m7icc.feishu.cn/docx/PaWwdtHGOoSqWrxCv9Ucbttinyd](https://kcn9ut4m7icc.feishu.cn/docx/PaWwdtHGOoSqWrxCv9Ucbttinyd)

Auto-import feature in Tab helps add module imports automatically in TypeScript and Python projects

Tab 中的自动导入功能有助于在 TypeScript 和 Python 项目中自动添加模块导入

## Overview 概览

In TypeScript and Python (beta) project, Tab can automatically import modules and functions from elsewhere in your project, without you having to manually type the import statement.

在 TypeScript 和 Python（测试版）项目中，Tab 可以自动从项目的其他部分导入模块和函数，无需您手动输入导入语句。

Just start using the method you want from an existing file, and Tab will automatically suggest the import statement for you. If you accept, the import statement will be added to your file without pulling you away from the code you are writing.

直接从现有文件中使用您想要的方法，Tab 将自动为您建议导入语句。如果您接受，导入语句将被添加到您的文件中，而不会让您离开正在编写的代码。

## Troubleshooting 故障排除

If you are having issues with auto-import, please confirm you have the necessary extensions (e.g. a language server) for your project language, as this is required for auto-import to work.

如果您在自动导入方面遇到问题，请确认您已为项目语言安装必要的扩展（例如语言服务器），因为这是自动导入功能正常工作的前提。

You can confirm if this is working, by moving your cursor to a function or method that is not yet imported, and hit `⌘ + .` or `Ctrl + .` to see if the import is suggested in the Quick Fix suggestions - if not, then the language server is not working.

您可以通过将光标移至尚未导入的函数或方法，然后按 `⌘ + .` 或 `Ctrl + .` 来确认是否正常工作，以查看是否在快速修复建议中建议导入 - 如果没有，则表示语言服务器未正常工作。
