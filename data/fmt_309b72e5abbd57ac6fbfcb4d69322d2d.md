
## 4. 结果可视化展示 (核心教学环节)

```python
print("\n" + "="*50)

display(Markdown(f"### 🤖 AI 回答:\n\n{response.response}"))

print("="*50 + "\n")

print("🔍 --- [RAG 检索证据展示] ---")

# 遍历所有检索到的节点 (Source Nodes)
for i, node_with_score in enumerate(response.source_nodes):
    node = node_with_score.node
    score = node_with_score.score
    print(f"\n🏷️ [证据 #{i+1}] 相似度: {score:.4f}")
    # 判断是图片还是文本
    # 注意：LlamaIndex 的 ImageNode 通常包含 image_path
    if hasattr(node, "image_path") and node.image_path:
        print(f"📸 类型: 图片节点")
        print(f"📂 路径: {node.image_path}")
        try:
            display(Image(filename=node.image_path, width=500))
        except:
            print("⚠️ 图片无法显示")
    else:
        print(f"📄 类型: 文本节点")
        # 截取前 200 字展示，避免刷屏
        preview_text = node.text[:200].replace('\n', ' ')
        print(f"📝 内容片段: \"{preview_text}...")

print("\n🎉 恭喜！全链路多模态 RAG 完美跑通！")
```

❓ [提问]: 根据文档内容，`LangChain` 的整体架构包含哪几个核心层？请结合架构图进行说明。

⏳ 正在检索上下文并进行视觉推理...

==================================================

### 🤖 AI 回答:

根据文档内容和架构图，`LangChain` 的整体架构包含以下几个核心层：

1. Architecture (架构层):

+ `LangChain`: 作为模型能力增强器，提供模块化功能，如模型接口封装、输出结构化、`Memory`管理、`Tool`接入、`Agent`架构、`RAG`集成、`Server/API`发布、`Debug & Callback`等。

+ `LangGraph`: 基于 `LangChain` 的更高层次封装，用于搭建图结构的大模型工作流，特别是 `Multi-Agent` 系统。

1. Components (组件层):

+ `Integrations`: 提供与其他工具和平台的集成，支持开源（OSS）。

1. Deployment (部署层):

+ `LangGraph Platform`: 商业化平台，用于部署和管理 `LangGraph` 工作流。

+ `LangSmith`: 提供调试、测试、监控等功能的商业化平台，帮助开发者监控和测试基于 `LangChain` 的 `Agent`。

这些层次共同构成了 `LangChain` 的整体架构，支持从开发到部署的完整流程。

==================================================

🔍 --- [RAG 检索证据展示] ---

🏷️ [证据 #1] 相似度: 0.7582

📄 类型: 文本节点

📝 内容片段: "更加适用于当前`Agent`开发的`LangChain`工具生态  在经历了短暂的阵痛后，`LangChain`果断进行了大刀阔斧的改革。`LangChain`调整的思路非常简单：  1. `LangChain`本身仍然坚守作为“模型能力增强器”的功能定位，并且逐渐稳定更新节奏和频率，虽说实际使用`LangChain`进行开发的代码量仍然没变，但模块划分更加清晰、功能更加丰富和稳定，逐步达到企业级应用水准。目前最新版La..."

🏷️ [证据 #2] 相似度: 0.7009

📄 类型: 文本节点

📝 内容片段: "[](`images/7598546bae7e34f86b59c86df2861984f1061c866e15dc7af5a44123ea0a184e.jpg`)   `LangChain`官网：https://www.langchain.com/  # 2.备受争议的工具功能  虽说`LangChain`的开源，在短时间内收获了大量的开发者用户，这也一度使得`LangChain`在2023年成为最受欢迎的大模型..."

🏷️ [证据 #3] 相似度: 0.4960

📸 类型: 图片节点

📂 路径: `mineru_output\c25ef0fd-eb56-4851-967a-959e56fe93bd\images\2cebee5ebf8eec7ed07852655b11655a8692178737c5aef72131ee770ed92ca9.jpg`

🎉 恭喜！全链路多模态 RAG 完美跑通！

## Part 5. 多模态文搜图、图搜图RAG检索实战

```python
# === Cell 1: 升级版 Qwen2.5-VL Embedding (带自动压缩功能) ===

import os

import dashscope

import tempfile

from typing import List, Any

from pathlib import Path

from http import HTTPStatus

from PIL import Image

from llama_index.core.embeddings import MultiModalEmbedding

from llama_index.core.schema import ImageType

# 确保安装了 dashscope

# !pip install dashscope pillow

class DashScopeMultiModalEmbedding(MultiModalEmbedding):

    """

    适配阿里百炼 Qwen2.5-VL-Embedding 模型

    ✨ 新增特性：上传前自动压缩图片，提升速度并减少 Token 消耗

    """

    def __init__(self, api_key: str, model_name: str = "qwen2.5-vl-embedding"):

        super().__init__(model_name=model_name)

        dashscope.api_key = api_key

    def _call_api(self, input_data: list) -> List[float]:

        try:

            resp = dashscope.MultiModalEmbedding.call(

                model=self.model_name,

                input=input_data

            )

            if resp.status_code == HTTPStatus.OK:

                if 'embeddings' in resp.output and len(resp.output['embeddings']) > 0:

                    return resp.output['embeddings'][0]['embedding']

                else:

                    print(f"⚠️ API 返回结果为空: {resp}")

                    return [0.0] * 1024 

            else:

                print(f"⚠️ API 调用失败: {resp.message}")

                return [0.0] * 1024

        except Exception as e:

            print(f"⚠️ 网络或处理异常: {e}")

            return [0.0] * 1024

    # --- ✨ 新增：图片压缩辅助函数 ---

    def _compress_image(self, image_path: str, max_size: int = 1024, quality: int = 85) -> str:

        """

        读取图片，压缩大小和质量，保存到临时文件，返回临时文件路径。

        """

        try:

            with Image.open(image_path) as img:

                # 1. 转换格式：如果是 PNG 透明图，转为 RGB，防止保存为 JPEG 出错

                if img.mode in ('RGBA', 'P'):

                    img = img.convert('RGB')

                # 2. 调整尺寸：保持比例，长边不超过 max_size

                # thumbnail 方法是原地修改，且只有当图片大于指定尺寸时才缩放

                img.thumbnail((max_size, max_size))

                # 3. 创建临时文件

                # delete=False 确保我们可以把路径传给 API，用完后再手动删

                temp_file = tempfile.NamedTemporaryFile(suffix=".jpg", delete=False)

                img.save(temp_file.name, format="JPEG", quality=quality)

                # print(f"📉 图片已压缩: {os.path.basename(image_path)} -> {os.path.getsize(temp_file.name)/1024:.1f} KB")

                return temp_file.name

        except Exception as e:

            print(f"⚠️ 图片压缩失败，将使用原图: {e}")

            return image_path

    # --- 1. 图片向量化 (修改版) ---

    def _get_image_embedding(self, img_file_path: ImageType) -> List[float]:

        # 1. 先进行本地压缩，获取临时文件路径

        compressed_path = self._compress_image(img_file_path)

        try:

            # 2. 转换为绝对路径并加 file:// 前缀

            abs_path = Path(compressed_path).absolute()

            input_data = [{'image': f"file://{abs_path}"}]

            # 3. 调用 API

            return self._call_api(input_data)

        finally:

            # 4. 清理：如果 compressed_path 是临时文件（和原图路径不同），则删除

            if compressed_path != img_file_path and os.path.exists(compressed_path):

                try:

                    os.remove(compressed_path)

                except:

                    pass

    # --- 2. 文档文本向量化 ---

    def _get_text_embedding(self, text: str) -> List[float]:

        input_data = [{'text': text}]

        return self._call_api(input_data)

    # --- 3. 查询文本向量化 ---

    def _get_query_embedding(self, query: str) -> List[float]:

        input_data = [{'text': query}]

        return self._call_api(input_data)

    # --- 4. 异步方法 ---

    async def _aget_image_embedding(self, img_file_path: ImageType) -> List[float]:

        return self._get_image_embedding(img_file_path)

    async def _aget_text_embedding(self, text: str) -> List[float]:

        return self._get_text_embedding(text)

    async def _aget_query_embedding(self, query: str) -> List[float]:

        return self._get_query_embedding(query)

print(f"✅ Qwen2.5-VL 适配版 (带自动压缩) 定义完成！")
```

✅ Qwen2.5-VL 适配版 (带自动压缩) 定义完成！

```python
# === Cell 2: 使用 Qwen 构建多模态索引 ===

from llama_index.core import SimpleDirectoryReader

from llama_index.core.indices import MultiModalVectorStoreIndex

# 1. 设置您的阿里云 API Key

# 建议直接替换字符串，或者从环境变量读取

my_api_key = os.environ["DASHSCOPE_API_KEY"]

# 2. 初始化我们的自定义模型

print("🚀 正在初始化 Qwen2.5-VL Embedding 模型...")

qwen_embed_model = DashScopeMultiModalEmbedding(api_key=my_api_key)
```

🚀 正在初始化 Qwen2.5-VL Embedding 模型...

```python
# 2. 初始化我们的自定义模型

print("🚀 正在初始化 Qwen2.5-VL Embedding 模型...")

qwen_embed_model = DashScopeMultiModalEmbedding(api_key=my_api_key)

# 3. 加载本地图片数据

image_dir = "test_images"

if not os.path.exists(image_dir):

    print(f"⚠️ 警告: 目录 {image_dir} 不存在，请先运行上一轮的下载图片代码！")

else:

    print(f"📂 正在读取目录: {image_dir} ...")

    documents = SimpleDirectoryReader(image_dir).load_data()

    # 4. 构建索引 (关键一步)

    # image_embed_model 指定为我们的 qwen 对象

    print("🧠 正在调用阿里百炼 API 构建索引 (这会消耗 token)...")
```
