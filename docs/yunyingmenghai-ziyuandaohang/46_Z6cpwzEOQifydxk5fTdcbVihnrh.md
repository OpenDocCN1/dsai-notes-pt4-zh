
# Py转whl

> 来源：[https://yunyingmenghai.feishu.cn/docx/Y32pduc3vohWg4xb13hcPdbWnch](https://yunyingmenghai.feishu.cn/docx/Y32pduc3vohWg4xb13hcPdbWnch)

https://github.com/360PB/py2whl

---

## 推荐的项目结构调整

要支持 `python -m qimuweb` 和 `qimuweb` 命令，您需要将项目组织成一个 Python 包。推荐的结构如下：

```
project/
├── qimuweb/              # 包目录
│   ├── __init__.py       # 包初始化文件
│   ├── __main__.py       # 用于支持 `python -m qimuweb`
│   ├── qimuweb.py        # 主模块文件，包含 main 函数
│   ├── digital_human_app.py
├── setup.py              # 打包脚本
```

### 调整说明

1. **新建 `qimuweb/` 文件夹**

+ 将 `__init__.py`、`__main__.py`、`qimuweb.py` 和 `digital_human_app.py` 文件移动到 `qimuweb/` 文件夹中。

+ 这样可以将 `qimuweb` 作为一个 Python 包。

2. **`setup.py` 保留在项目根目录**

+ `setup.py` 文件应始终位于项目根目录。

---

## 修改 setup.py

在项目根目录的 `setup.py` 文件中，确保配置如下：

```
from setuptools import setup, find_packages

setup(
    name='qimuweb',
    version='1.0',
    packages=find_packages(),  # 自动查找子包
    entry_points={
        'console_scripts': [
            'qimuweb = qimuweb.qimuweb:main',  # 指定 qimuweb.py 中的 main() 函数
        ],
    },
    include_package_data=True,  # 确保非代码文件也被打包
    install_requires=[
        "gradio",
        "pydub",
        "psutil",
    ],
    python_requires=">=3.12",  # 指定支持的 Python 版本
)
```

---

## 修改 main.py

在 `qimuweb/__main__.py` 中，修改内容如下：

```
from qimuweb.qimuweb import main

if __name__ == "__main__":
    main()
```

### 解释

+ `python -m qimuweb` 会运行 `qimuweb/__main__.py` 文件。

+ 通过 `from qimuweb.qimuweb import main`，它会调用 `qimuweb.py` 中的 `main()` 函数。

---

## 确保 qimuweb.py 中定义了 main 函数

在 `qimuweb/qimuweb.py` 文件中，确保有一个 `main()` 函数，例如：

```
def main():
    print("qimuweb is running!")
```

---

## 打包与安装

### 清理旧的构建文件

在项目根目录下运行以下命令，清理旧的构建文件：

```
rm -rf build dist *.egg-info
```

### 重新打包

在项目根目录下运行以下命令，重新生成 `.whl` 文件：

```
python setup.py bdist_wheel
```

### 安装打包的文件

安装生成的 `.whl` 文件：

```
pip install dist/qimuweb-1.0-py3-none-any.whl --force-reinstall
pip install qimuweb-1.0-py3-none-any.whl --force-reinstall
```

---

![](img/594fb53bac9e04d3410bc7748a9881c3.png)

[qimuweb-1.0-py3-none-any.whl](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UE2Xb5H01oYuEPxWol9cboUwnOc)

## 测试运行

### 测试通过 python -m qimuweb 运行

---

## 如果不能调整目录结构

如果您不想调整目录结构（保持所有文件在同一级目录下），可以尝试以下修改：

### 修改 setup.py

```
from setuptools import setup

setup(
    name='qimuweb',
    version='1.0',
    py_modules=['qimuweb', 'digital_human_app'],  # 指定模块文件
    entry_points={
        'console_scripts': [
            'qimuweb = qimuweb:main',  # 指定 qimuweb.py 中的 main() 函数
        ],
    },
    include_package_data=True,
    install_requires=[
        "gradio",
        "pydub",
        "psutil",
    ],
    python_requires=">=3.6",
)
```

### 修改 main.py

将 `main.py` 修改为：

```
from qimuweb import main

if __name__ == "__main__":
    main()
```

### 测试运行

重新打包和安装后，测试以下两种运行方式：

1. 通过 `python -m qimuweb` 运行：

---

## 总结

+ 推荐调整项目结构，将 `qimuweb` 的所有文件放入一个子目录中（如 `qimuweb/`），以符合标准的 Python 包结构。

+ 如果不能调整目录结构，可以通过 `py_modules` 在 `setup.py` 中显式指定模块文件。

+ 关键在于确保 `main()` 函数的导入路径正确，以及打包后的文件能够被 Python 识别。
