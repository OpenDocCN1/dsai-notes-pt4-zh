
# 数据模型

## Session

```
class Session(BaseModel):
    """
    完整会话记录 (对应 session_id.json)
    """
    session_id: str
    created_at: float = Field(default_factory=time.time)  # 会话创建时间
    last_updated: float = Field(default_factory=time.time)  # 会话最后更新时间
    is_favorite: bool = False  # 标记是否为收藏的会话
    messages: List[Message] = Field(default_factory=list)  # 消息列表，存储所有对话消息
```

该类表示一个完整的对话会话记录。一个会话由多个消息（`Message`）组成，且包含以下字段：

+   `session_id`：唯一的会话 ID，通常由 UUID 生成，用于区分不同的会话。

+   `created_at`：会话的创建时间，使用 `time.time()` 获取当前时间戳。

+   `last_updated`：会话的最后更新时间，记录每次对话或状态更新的时间。

+   `is_favorite`：布尔值，表示该会话是否被标记为“收藏”。用户可以根据自己的需求标记某些会话为收藏。

+   `messages`：该会话的消息列表，包含该会话中的所有消息，每条消息都由 `Message` 类表示。

该类是会话的核心数据结构，帮助管理用户和助手之间的对话，以及会话的创建与更新。

## ChatRequest

```
class ChatRequest(BaseModel):
    session_id: str  # 会话 ID
    message: str  # 用户输入的文本消息
    model: str  # 用户选择的模型 ID
    user_images_base64: List[str] = Field(default_factory=list)  # 用户上传的图片 (Base64 格式)
```

该类用于表示发送给后端的聊天请求数据。字段包括：

+   `session_id`：当前会话的 ID，用于跟踪和关联消息。

+   `message`：用户输入的文本消息，后端将基于该消息生成回应。

+   `model`：用户选择的模型 ID，指定使用哪个模型生成回应。

+   `user_images_base64`：可选字段，包含用户上传的图像（以 Base64 编码的形式）。

## ChatResponse

```
class ChatResponse(BaseModel):
    assistant_message: str  # 助手的文本回复
    image_urls: List[str] = Field(default_factory=list)  # 本轮生成的图片 URL 列表
```

该类表示后端生成的聊天响应数据，字段包括：

+   `assistant_message`：助手生成的文本回复。

+   `image_urls`：生成图像的 URL 列表，通常是指向保存生成图像的路径或 URL。

## SessionSummary

```
class SessionSummary(BaseModel):
    """
    用于作品集/橱窗列表展示的简要信息
    """
    session_id: str  # 会话 ID
    last_updated: float  # 最后更新时间
    is_favorite: bool  # 是否为收藏
    cover_image_url: Optional[str] = None  # 橱窗展示用的封面图 (最后一张生成的图)
    cover_prompt: Optional[str] = None  # 封面图对应的提示词
```

该类用于生成会话摘要，通常用于作品集或橱窗展示。它包含以下字段：

+   `session_id`：会话的唯一 ID。

+   `last_updated`：最后一次更新时间。

+   `is_favorite`：是否为收藏的会话。

+   `cover_image_url`：用于展示的封面图 URL，通常是会话中最后生成的图像。

+   `cover_prompt`：生成封面图时使用的提示词。

# main.py 代码解释

让前端可以通过 `http://localhost:8002/images/xxx.png` 访问生成的图片

```
# main.py
import uvicorn
from fastapi import FastAPI, HTTPException, Query
from fastapi.middleware.cors import CORSMiddleware
from fastapi.staticfiles import StaticFiles
from typing import List, Optional

# 导入我们的本地模块
import models
import data_manager
import core_agent

# 1. 初始化 FastAPI 应用
app = FastAPI(title="AI Drawing Agent Backend")

# 2. 配置 CORS (允许跨域)
# 这对于前后端分离项目至关重要，允许前端 (通常在 3000/8080 端口) 访问后端 API
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],  # 生产环境建议指定具体前端域名
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# 3. 挂载静态文件目录
# 确保存储目录存在
data_manager.ensure_directories()
app.mount("/images", StaticFiles(directory="storage/images"), name="images")

# --- API 接口定义 ---

from pydantic import BaseModel

class ApiKeySetting(BaseModel):
    api_key: str

@app.post("/api/settings/apikey")
def set_api_key(setting: ApiKeySetting):
    """设置用户的 OpenRouter API Key"""
    data_manager.save_api_key(setting.api_key)
    return {"status": "success", "message": "API Key 已保存"}

@app.get("/api/settings/apikey")
def check_has_api_key():
    """检查当前是否已配置 Key (出于安全，不返回明文 Key，只返回是否已设置)"""
    key = data_manager.get_api_key()
    return {"has_key": bool(key)}

@app.post("/api/sessions", response_model=models.Session)
def create_new_session():
    """创建一个新的空白会话"""
    return data_manager.create_session()

@app.get("/api/sessions", response_model=List[models.SessionSummary])
def get_session_list(filter: Optional[str] = Query(None, description="输入 'favorite' 只查看收藏")):
    """
    获取作品集/会话列表。
    支持 filter=favorite 参数。
    """
    only_favorite = (filter == "favorite")
    return data_manager.list_sessions(only_favorite=only_favorite)

@app.get("/api/sessions/{session_id}", response_model=models.Session)
def get_session_detail(session_id: str):
    """获取指定会话的完整历史记录"""
    session = data_manager.load_session(session_id)
    if not session:
        raise HTTPException(status_code=404, detail="Session not found")
    return session

@app.delete("/api/sessions/{session_id}")
def delete_session(session_id: str):
    """删除会话及其关联图片"""
    data_manager.delete_session(session_id)
    return {"status": "success", "message": f"Session {session_id} deleted"}

@app.post("/api/sessions/{session_id}/favorite")
def toggle_session_favorite(session_id: str):
    """切换会话的收藏状态"""
    new_status = data_manager.toggle_favorite(session_id)
    return {"session_id": session_id, "is_favorite": new_status}

@app.post("/api/chat", response_model=models.ChatResponse)
def chat_endpoint(request: models.ChatRequest):
    """
    核心对话/绘图接口。
    前端发送文本、模型选择、以及可选的参考图，后端返回文本和生成的图片URL。
    """
    # 简单的参数校验
    if not request.session_id:
        raise HTTPException(status_code=400, detail="Session ID is required")
    
    # 调用核心 Agent 逻辑
    try:
        response = core_agent.process_chat(request)
        return response
    except Exception as e:
        # 捕获未预料的错误，避免服务崩溃
        print(f"Server Error: {e}")
        raise HTTPException(status_code=500, detail=str(e))

# 4. 启动入口
if __name__ == "__main__":
    print("正在启动 AI Drawing Agent 后端...")
    print(f"API 文档地址: http://localhost:8002/docs")
    # host="0.0.0.0" 允许局域网访问，port=8002 符合要求
    uvicorn.run("main:app", host="0.0.0.0", port=8002, reload=True)
```

## FastAPI 应用初始化

```
app = FastAPI(title="AI Drawing Agent Backend")
```

`FastAPI` 初始化

`FastAPI` 用于创建一个 Web 应用实例。通过传递 `title="AI Drawing Agent Backend"`，为 API 文档设置了应用的标题。`FastAPI` 会自动生成一个交互式 API 文档，方便开发和调试。API 文档地址通常是 `http://localhost:8002/docs`。

## 配置 CORS（跨域资源共享）

```
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],  # 生产环境建议指定具体前端域名
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

`CORSMiddleware` 配置

`CORSMiddleware` 用于处理跨域请求（CORS），使得前端应用可以从不同域名或端口访问这个后端 API。对于前后端分离的应用程序来说，这是非常重要的。这里的配置说明：

+   `allow_origins=["*"]`：允许所有来源的请求。生产环境中建议替换为指定的前端域名，避免潜在的安全风险。

+   `allow_methods=["*"]`：允许所有 HTTP 方法（如 `GET`、`POST`、`DELETE` 等）。

+   `allow_headers=["*"]`：允许所有 HTTP 请求头。

## 挂载静态文件目录

```
data_manager.ensure_directories()
app.mount("/images", StaticFiles(directory="storage/images"), name="images")
```

`app.mount("/images", StaticFiles(directory="storage/images"), name="images")`

使用 FastAPI 的 `StaticFiles` 组件将本地存储的图像文件夹（`storage/images`）挂载为静态文件目录。前端可以通过访问 `http://localhost:8002/images/{image_name}` 来获取生成的图像。

+   `data_manager.ensure_directories()` 确保相关目录存在，避免文件操作时出现路径错误。

## API 路由定义

### 设置 API Key

```
class ApiKeySetting(BaseModel):
    api_key: str

@app.post("/api/settings/apikey")
def set_api_key(setting: ApiKeySetting):
    """设置用户的 OpenRouter API Key"""
    data_manager.save_api_key(setting.api_key)
    return {"status": "success", "message": "API Key 已保存"}

@app.get("/api/settings/apikey")
def check_has_api_key():
    """检查当前是否已配置 Key (出于安全，不返回明文 Key，只返回是否已设置)"""
    key = data_manager.get_api_key()
    return {"has_key": bool(key)}
```

`set_api_key`：

该接口允许用户通过 `POST` 请求设置 OpenRouter 的 API Key。收到请求后，API Key 会被保存到 `settings.json` 文件中，确保后续请求能够成功验证。
