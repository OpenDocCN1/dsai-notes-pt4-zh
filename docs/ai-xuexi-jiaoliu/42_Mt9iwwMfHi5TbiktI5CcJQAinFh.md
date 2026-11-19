
# LM Studio 本地部署 Deepseek

> 来源：[https://zareers.feishu.cn/docx/ALfad3oTKorRdcxhHyOcCwXJnHg](https://zareers.feishu.cn/docx/ALfad3oTKorRdcxhHyOcCwXJnHg)

## 注意事项

建议使用 NVIDIA 卡，体验会好不少，AMD 显卡未做测试，但应该也是能跑的。

欢迎各位在本文档评论区反馈各自运行情况，这样可以帮到更多人。

## LM Studio 安装

LM Studio 是一款能够本地离线运行各类型大语言模型的客户端应用，通过 LM Studio 可以快速搜索所需的 llms 类型的开源大语言模型，并运行。

在本地运行大语言模型可以更加快速的运行，流畅的提问，并在独立的环境中保障数据不被监听和收集。

特点：本地、独立、离线

官网下载：https://lmstudio.ai/

直接下载 ↓ 【Windows 版】

![](img/1f1ca136e9639c7ef8de9630af85c07e.png)

[`LM-Studio-0.3.8-4-x64.exe`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CQE8bleI0oBNnjxAjDvclaYAntb)

直接下载 ↓ 【macOS 版】

![](img/2b5c62d178df9b645c8f3af69ecc8b54.png)

[`LM-Studio-0.3.8-4-arm64.dmg`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KXSNbTGPLoWRYcxefTGcNMfMnsb)

## 通过 LM Studio 部署本地 Deepseek

1. 双击 LM Studio 图标，打开 LM Studio。

    ![](img/43ce3179826e65d67a702ef31341e595.png)

2. 选中一个模型，这里默认可以选择 LLM 模型，也可以点击如下图的“跳过”菜单进行跳过。跳过后进入下一步。

    ![](img/b08f48a539ae89b406dba759c027ddd2.png)

3. 进入到 chat 界面并点击顶部的选择按钮，进行搜索和下载新的模型：

    ![](img/e91c3e6079107ddc502933135e9b65bf.png)

4. 搜索 Deepseek。

    ![](img/536d6b85d4d6cda0f8814f5df791d193.png)

5. 点击“search more”进入到搜索界面，选中并点击下载即可：

    ![](img/00fc2cbb9daa468049c2228b6159902d.png)

6. 等待下载完成。

    ![](img/f2d7270fdeeb5e5d9a3c9c901610a1f7.png)

7. 下载完成后，chat 界面选择运行的模型及配置信息，如下图：

    ![](img/20dfb81685048352865e2609193a3f8c.png)

8. 点击模型，弹出配置参数表单，可以根据自己的电脑性能进行选择（性能越高，速度越快）：

    ![](img/cacde0b1c9d3d9fd6f2739d31773cefa.png)

9. 确认后，点击 load model 即可，对于 model 的管理可以点击左侧的菜单进入：

    ![](img/dc458d2df5ce9179b49b6442591470e2.png)

10. 本地运行界面：

    ![](img/ea9cbb86ec3974d8181b3b6d7a818c27.png)

## 模型列表

请选择文件体积小于自己显存大小的模型，略大一些的虽然也能跑，但是速度会慢很多。因个人能力有限，以下模型推荐不一定是最好的。

更多模型下载

链接1：https://www.modelscope.cn/organization/lmstudio-community

记得用搜索功能搜索 DeepSeek 相关模型

链接2：https://www.modelscope.cn/collections/DeepSeek-R1-Distill-GGUF-eec5fee2f2ee42

相比链接1会多一点不同量化精度的模型。

**点击模型名称可以直接下载**

| 显存大小 | 推荐模型 | 备注 |
| --- | --- | --- |
| 32GB | [`DeepSeek-R1-Distill-Qwen-32B-Q6_K.gguf`](https://www.modelscope.cn/models/lmstudio-community/DeepSeek-R1-Distill-Qwen-32B-GGUF/resolve/master/DeepSeek-R1-Distill-Qwen-32B-Q6_K.gguf) |  |
| 24GB | [`DeepSeek-R1-Distill-Qwen-32B-Q4_K_M.gguf`](https://www.modelscope.cn/models/lmstudio-community/DeepSeek-R1-Distill-Qwen-32B-GGUF/resolve/master/DeepSeek-R1-Distill-Qwen-32B-Q4_K_M.gguf) | 二选一即可，比较推荐第一个 |
| 16GB | [`DeepSeek-R1-Distill-Qwen-14B-Q6_K.gguf`](https://www.modelscope.cn/models/lmstudio-community/DeepSeek-R1-Distill-Qwen-14B-GGUF/resolve/master/DeepSeek-R1-Distill-Qwen-14B-Q6_K.gguf) |  |
| 12GB | [`DeepSeek-R1-Distill-Qwen-14B-Q4_K_M.gguf`](https://www.modelscope.cn/models/lmstudio-community/DeepSeek-R1-Distill-Qwen-14B-GGUF/resolve/master/DeepSeek-R1-Distill-Qwen-14B-Q4_K_M.gguf) |  |
| 11GB | [`DeepSeek-R1-Distill-Qwen-14B-Q4_K_M.gguf`](https://www.modelscope.cn/models/lmstudio-community/DeepSeek-R1-Distill-Qwen-14B-GGUF/resolve/master/DeepSeek-R1-Distill-Qwen-14B-Q4_K_M.gguf) |  |
| 8GB | [`DeepSeek-R1-Distill-Qwen-7B-Q6_K.gguf`](https://www.modelscope.cn/models/lmstudio-community/DeepSeek-R1-Distill-Qwen-7B-GGUF/resolve/master/DeepSeek-R1-Distill-Qwen-7B-Q6_K.gguf) | 二选一即可 |
| 6GB | [`DeepSeek-R1-Distill-Qwen-7B-Q4_K_M.gguf`](https://www.modelscope.cn/models/lmstudio-community/DeepSeek-R1-Distill-Qwen-7B-GGUF/resolve/master/DeepSeek-R1-Distill-Qwen-7B-Q4_K_M.gguf) | 二选一即可 |
| 4GB | [`DeepSeek-R1-Distill-Qwen-7B-Q3_K_L.gguf`](https://www.modelscope.cn/models/lmstudio-community/DeepSeek-R1-Distill-Qwen-7B-GGUF/resolve/master/DeepSeek-R1-Distill-Qwen-7B-Q3_K_L.gguf) | 二选一即可，比较推荐第一个 |
| 3GB、2GB | [`DeepSeek-R1-Distill-Qwen-1.5B-Q8_0.gguf`](https://www.modelscope.cn/models/lmstudio-community/DeepSeek-R1-Distill-Qwen-1.5B-GGUF/resolve/master/DeepSeek-R1-Distill-Qwen-1.5B-Q8_0.gguf) |  |

## 如何查看自己的显存

![](img/187903d44f38633431e03b67cad75d6c.png)

| 显存大小 | 对应N卡型号 |
| --- | --- |
| 2GB | GTX 1050、GT 1030 |
| 3GB | GTX 1060 3GB |
| 4GB | GTX 1050 Ti |
| 6GB | GTX 1060 6GB、RTX 2060 6GB、RTX 3050 (6GB) |
| 8GB | GTX 1080、GTX 1070 Ti、GTX 1070、RTX 2080 SUPER、RTX 2080、RTX 2070 SUPER、RTX 2070、RTX 2060 SUPER、RTX 3070 Ti、RTX 3070、RTX 3060 Ti、RTX 3060 (8GB)、RTX 3050 (8GB)、RTX 4080、RTX 4060 Ti 8GB、RTX 4060、RTX 5070 |
| 11GB | GTX 1080 Ti、RTX 2080 Ti |
| 12GB | RTX 2060 12GB、RTX 3060 (12GB)、RTX 4070 Ti SUPER、RTX 4070、RTX 5070 Ti |
| 16GB | RTX 4060 Ti 16GB、RTX 5080 |
| 24GB | RTX 3090 Ti、RTX 3090、RTX 4090 |
| 32GB | RTX 5090 |
