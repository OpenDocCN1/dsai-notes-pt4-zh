
## 3.1 MinerU 环境搭建与依赖安装

`conda create --name mineru_2.5 python==3.11 -y`

![](img/a86464eb8cebe875f85a4e26c2698b1f.png)

![](img/65cf4a2f5210a3af9d8fd7b249cd4c66.png)

![](img/90645a2f875cc9b81f8ce66018922eeb.png)

![](img/7ceb69f939337424ca4d01f61a8789e2.png)

![](img/1ca6be38411a109361e237e5cb586c7e.png)

### Step 5. 激活虚拟环境

创建完虚拟环境后，使用 Conda 激活虚拟环境，通过 `conda activate mineru_2.5` 命令激活，如下图所示：

### Step 6. 下载 MinerU 源码文件

截止目前最新的 MinerU 项目源码版本为是：2.6.4-released，但当前版本的源码在使用 MCP Server 时存在一些 Bug，所以我们针对源码做了一些修改。因此大家一定要下载课程网盘中的源码文件进行部署和使用。下载后上传至服务器中，解压文件，并进入解压后的文件夹，如下图所示：

注意，大家这里一定要使用课程提供的源码版本，因为我们对源码进行了非常多的修改才能适配 MCP 服务下使用 MinerU vllm 解析服务。

### Step 7. 安装 MinerU 项目依赖

使用 Conda 安装 MinerU 项目依赖，需要通过如下命令在新建的 `mineru` 虚拟环境中安装运行 MinerU 项目程序的所有依赖，命令如下：

```bash
pip install -e .[all] -i https://mirrors.aliyun.com/pypi/simple
```

![](img/4e5473a05b5d7b35f81a43857cc715fb.png)

![](img/5496d10ef5a217452e02e391fbabd2c4.png)

通过 `pip show mineru` 命令查看 MinerU 项目的版本：

至此，基础的 MinerU 项目依赖就安装完成了，接下来我们需要下载 MinerU 项目中用到的模型文件，并进行项目配置。

### Step 8. 下载 MinerU 项目中用到的模型文件

新版本的 MinerU 项目提供了一键下载所有模型文件的脚本，我们只需要执行如下命令即可：

```bash
mineru-models-download
```

![](img/eb6099ba742fb0eef1ecb988429daf5b.png)

执行该命令需要我们根据自己的需求灵活选择要下载到本地的模型。其中 `modelscope` 和 `huggingface` 是两个不同的模型下载源，我们只需要选择其中一个即可。（注意：国内用户建议使用 `modelscope` 下载模型文件）。其次，`pipeline` 指的是用于文档解析的一系列模型，而 `vlm` 指的是用于视觉语言模型的模型，即 `MinerU2.0-2505-0.9B` 模型。如果是 `all`，则会全部下载。

等待下载完成后，所有模型文件的默认存储路径是：

```bash
/root/.cache/modelscope/hub/models/OpenDataLab
```

![](img/1766f5f4afd3355f962b174cdeabfb5e.png)

![](img/d9f8a5d8d02c9bec8c9cfa34fec7510c.png)

![](img/84297eb8a38c5476cef86ee64ce0c19e.png)

![](img/bda91e5308e466f3b6c560a2e8cca877.png)

并且，也会自动在 `/root/mineru.json` 文件中配置用于后续使用的模型路径。

至此，MinerU 项目的本地配置就全部完成了，接下来我们可以尝试运行 MinerU 项目并进行 PDF 文档解析测试。

## 3.2 MinerU 启动 vLLM API 推理服务

接下来新打开一个终端，启动 `mineru-api` 服务：

```bash
export MINERU_MODEL_SOURCE=local  # 注意：这里需要将模型源设置为本地

export CUDA_VISIBLE_DEVICES=3  # 使用 哪一块 GPU

mineru-api --host 192.168.110.131 --port 50000
```

![](img/b1650fb9cca93913674b990af1a36394.png)

![](img/046c23f9aaf7c5fa16f225aa92254270.png)

启动后，在 `192.168.110.131:50000/docs` 中可以看到接口服务：

## 3.3 MinerU vLLM API 服务连接测试

```python
#!/usr/bin/env python3
"""
测试 MinerU API 的不同 backend
通过 50000 端口调用，传递不同的 backend 参数来使用不同的模型
"""

import requests
import sys
from pathlib import Path


def test_mineru_api(pdf_path: str, backend: str = "pipeline"):
    """
    测试 MinerU API

    Args:
        pdf_path: PDF 文件路径
        backend: 后端类型，可选值：
                - "pipeline" (默认，使用本地 PyTorch)
                - "vlm-vllm-async-engine" (使用 vLLM 加速)
    """
    api_url = "http://192.168.130.4:50000/file_parse"
    print(f"\n{'=' * 60}")
    print(f"测试 MinerU API with backend: {backend}")
    print(f"{'=' * 60}")
    print(f"API URL: {api_url}")
    print(f"PDF 文件: {pdf_path}")
    print(f"Backend: {backend}")
    print()

    # 检查文件是否存在
    if not Path(pdf_path).exists():
        print(f"错误: 文件不存在 - {pdf_path}")
        return None

    try:
        # 打开文件并发送请求
        with open(pdf_path, 'rb') as f:
            files = [('files', (Path(pdf_path).name, f, 'application/pdf'))]
            data = {
                'backend': backend,
                'parse_method': 'auto',
                'lang_list': 'ch',
                'return_md': 'true',
                'return_middle_json': 'false',
                'return_model_output': 'false',
                'return_content_list': 'false',
                'start_page_id': '0',
                'end_page_id': '1',  # 只处理前2页，快速测试
            }
            print("发送请求...")
            response = requests.post(
                api_url,
                files=files,
                data=data,
                timeout=300
            )

        # 检查响应
        if response.status_code != 200:
            print(f"请求失败: HTTP {response.status_code}")
            print(f"响应: {response.text[:500]}")
            return None

        # 解析 JSON 响应
        result = response.json()

        # 提取信息
        backend_used = result.get('backend', 'unknown')
        version = result.get('version', 'unknown')
        results = result.get('results', {})

        print(f"请求成功!")
        print(f"   使用的 backend: {backend_used}")
        print(f"   版本: {version}")
        print(f"   结果数量: {len(results)}")

        # 提取 markdown 内容
        if results:
            file_key = list(results.keys())[0]
            md_content = results[file_key].get('md_content', '')
            print(f"\nMarkdown 内容预览 (前500字符):")
            print("-" * 60)
            print(md_content[:500])
            print("-" * 60)

            # 保存 markdown 到文件
            output_file = f"output_{backend}.md"
            with open(output_file, 'w', encoding='utf-8') as f:
                f.write(md_content)

            print(f"\n完整 Markdown 已保存到: {output_file}")
            return md_content
        else:
            print("未找到结果")
            return None

    except Exception as e:
        print(f"测试失败: {e}")
        import traceback
        traceback.print_exc()
        return None


def main():
    # 默认测试文件
    pdf_path = "./2507.05595v1.pdf"
    print(f"\n开始测试 MinerU API 的不同 backend")
    print(f"测试文件: {pdf_path}\n")

    # 测试 1: pipeline backend (本地 PyTorch)
    print("\n" + "=" * 60)
    print("测试 1: pipeline backend (本地 PyTorch)")
    print("=" * 60)
    result_pipeline = test_mineru_api(pdf_path, backend="pipeline")

    # 测试 2: vLLM backend (vLLM 加速)
    print("\n" + "=" * 60)
    print("测试 2: vlm-vllm-async-engine backend (vLLM 加速)")
    print("=" * 60)
    result_vllm = test_mineru_api(pdf_path, backend="vlm-vllm-async-engine")

    # 总结
    print("\n" + "=" * 60)
    print("测试总结")
    print("=" * 60)
    print(f"Pipeline backend: {'成功' if result_pipeline else '失败'}")
    print(f"vLLM backend: {'成功' if result_vllm else '失败'}")

    if result_pipeline and result_vllm:
        print("\n所有测试通过! MinerU 可以通过 backend 参数切换不同模型")

    print("\n💡 提示:")
    print("  - pipeline: 使用本地 PyTorch，适合调试")
    print("  - vlm-vllm-async-engine: 使用 vLLM 加速，速度更快")


if __name__ == "__main__":
    main()
```

```text
开始测试 MinerU API 的不同 backend

测试文件: ./2507.05595v1.pdf

============================================================

测试 1: pipeline backend (本地 PyTorch)

============================================================

============================================================

测试 MinerU API with backend: pipeline

============================================================

API URL: http://192.168.130.4:50000/file_parse

PDF 文件: ./2507.05595v1.pdf

Backend: pipeline

发送请求...

请求成功!

   使用的 backend: pipeline

   版本: 2.6.4

   结果数量: 1

Markdown 内容预览 (前500字符):

------------------------------------------------------------

# PaddleOCR 3.0 Technical Report

Cheng Cui, Ting Sun, Manhui Lin, Tingquan Gao, Yubo Zhang, Jiaxuan Liu, Xueqing Wang, Zelun Zhang, Changda Zhou, Hongen Liu, Yue Zhang, Wenyu Lv, Kui Huang, Yichao Zhang, Jing Zhang, Jun Zhang, Yi Liu, Dianhai Yu, Yanjun Ma

PaddlePaddle Team, Baidu Inc. paddleocr@baidu.com
```
