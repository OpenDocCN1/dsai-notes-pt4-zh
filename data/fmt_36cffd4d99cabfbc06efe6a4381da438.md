
```python
import random

from fastapi import FastAPI, Request, UploadFile, File, HTTPException, Form
from fastapi.staticfiles import StaticFiles
from fastapi.templating import Jinja2Templates
from fastapi.responses import HTMLResponse, JSONResponse, FileResponse
from pathlib import Path
import os
import uuid
import time
import asyncio
import json
import requests
from typing import Optional
import time
import random

# 创建 FastAPI 应用
app = FastAPI(
    title="AI视频特效制作平台",
    description="基于 ComfyUI 的图片转视频特效制作系统",
    version="1.0.0"
)

# 配置静态文件
static_path = Path(__file__).parent.parent / "frontend" / "static"
app.mount("/static", StaticFiles(directory=str(static_path)), name="static")

# 配置模板
templates_path = Path(__file__).parent.parent / "frontend" / "templates"
templates = Jinja2Templates(directory=str(templates_path))

# 配置目录
COMFYUI_INPUT_DIR = Path("comfyui/input")
COMFYUI_OUTPUT_DIR = Path("comfyui/output/video")
UPLOADS_DIR = Path("uploads")

# 确保目录存在
COMFYUI_INPUT_DIR.mkdir(parents=True, exist_ok=True)
COMFYUI_OUTPUT_DIR.mkdir(parents=True, exist_ok=True)
UPLOADS_DIR.mkdir(parents=True, exist_ok=True)

# 任务状态存储
tasks = {}

# ComfyUI 配置
COMFYUI_URL = "http://180.184.93.83:8188"
WORKFLOW_TEMPLATE_PATH = Path("workflow_template.json")

# 主页路由
@app.get("/", response_class=HTMLResponse)
async def index(request: Request):
    """渲染主页"""
    return templates.TemplateResponse("index.html", {"request": request})

# 健康检查接口
@app.get("/health")
async def health_check():
    """健康检查接口"""
    return {"status": "healthy", "message": "AI视频特效制作平台运行正常"}

# 1. 上传图片接口
@app.post("/api/upload")
async def upload_image(image: UploadFile = File(...)):
    """
    上传图片到 comfyui/input 文件夹，文件名保持为用户原始文件名

    Args:
        image: 上传的图片文件

    Returns:
        JSON响应，包含上传结果信息
    """
    try:
        # 验证文件类型
        if not image.content_type.startswith('image/'):
            raise HTTPException(status_code=400, detail="只支持图片文件")

        # 验证文件大小（限制为10MB）
        if image.size > 10 * 1024 * 1024:
            raise HTTPException(status_code=400, detail="文件大小不能超过10MB")

        # 使用原始文件名
        safe_filename = image.filename
        input_path = COMFYUI_INPUT_DIR / safe_filename
        upload_path = UPLOADS_DIR / safe_filename

        # 保存到 comfyui/input 目录
        with open(input_path, "wb") as buffer:
            content = await image.read()
            buffer.write(content)
        # 同时保存到 uploads 目录作为备份
        with open(upload_path, "wb") as buffer:
            buffer.write(content)

        return JSONResponse(content={
            "success": True,
            "message": "图片上传成功",
            "file_name": safe_filename,
            "original_name": safe_filename
        })

    except Exception as e:
        raise HTTPException(status_code=500, detail=f"上传失败: {str(e)}")

# 2. 核心特效处理函数接口
@app.post("/api/create-effect")
async def create_effect(image_name: str = Form(...)):
    """
    核心特效处理函数接口 - 调用 ComfyUI 的 prompt 接口

    Args:
        image_name: 图片文件名（原始文件名）

    Returns:
        JSON响应，包含任务 ID
    """
    try:
        # 验证图片文件是否存在
        input_path = COMFYUI_INPUT_DIR / image_name
        if not input_path.exists():
            raise HTTPException(status_code=404, detail="图片文件不存在")

        # 生成任务 ID
        task_id = str(uuid.uuid4())

        # 创建任务信息
        tasks[task_id] = {
            "task_id": task_id,
            "image_name": image_name,
            "status": "processing",
            "created_at": time.time(),
            "video_path": None
        }

        # 调用 ComfyUI 处理特效
        result = await call_comfyui_prompt_simple(task_id, image_name)

        if result:
            return JSONResponse(content={
                "success": True,
                "message": "特效任务创建成功",
                "task_id": task_id
            })
        else:
            raise HTTPException(status_code=500, detail="ComfyUI 处理失败")

    except Exception as e:
        raise HTTPException(status_code=500, detail=f"创建特效失败: {str(e)}")

def generate_unique_seed():
    now = int(time.time() * 1e6)  # 当前时间戳（微秒级）
    rand = random.randint(0, 99999)
    return int(f"{now}{rand % 100:02d}")  # 保证纯数字且高唯一性

# 简化的 ComfyUI prompt 调用
async def call_comfyui_prompt_simple(task_id: str, image_name: str) -> bool:
    """
    简化的 ComfyUI prompt 调用

    Args:
        task_id: 任务 ID
        image_name: 图片文件名

    Returns:
        bool: 是否成功
    """
    try:
        # 读取 JSON 模板文件
        json_file_path = WORKFLOW_TEMPLATE_PATH
        with open(json_file_path, 'r', encoding='utf-8') as f:
            data = json.load(f)

        # 设置参数值
        noise_seed = generate_unique_seed()
        image = image_name

        # 设置 'text' 和 'widgets_values' 字段的新值
        data["prompt"]["27"]["inputs"]["seed"] = noise_seed
        data["prompt"]["90"]["inputs"]["seed"] = noise_seed
        data["prompt"]["67"]["inputs"]["image"] = image
        data["prompt"]["139"]["inputs"]["filename_prefix"] = f"video/ComfyUI_{task_id}"

        # 确保 widgets_values 列表至少有一个元素
        if len(data["extra_data"]["extra_pnginfo"]["workflow"]["nodes"][0]["widgets_values"]) == 0:
            data["extra_data"]["extra_pnginfo"]["workflow"]["nodes"][0]["widgets_values"].append(None)

        # # 现在可以安全地赋值
        # data["extra_data"]["extra_pnginfo"]["workflow"]["nodes"][4]["widgets_values"][0] = text_value
        # # data["extra_data"]["extra_pnginfo"]["workflow"]["nodes"][11]["widgets_values"][0] = noise_seed
        # data["extra_data"]["extra_pnginfo"]["workflow"]["nodes"][6]["widgets_values"][0] = image

        # 使用默认请求头
        headers = {'Content-Type': 'application/json'}

        # 发送 POST 请求
        url = f"{COMFYUI_URL}/prompt"
        response = requests.post(url, headers=headers, json=data)

        if response.status_code == 200:
            # 请求成功
            result = response.json()
            if task_id in tasks:
                tasks[task_id]["prompt_id"] = result.get("prompt_id")
                tasks[task_id]["status"] = "processing"
                print(f"任务 {task_id} 已提交到 ComfyUI，prompt_id: {result.get('prompt_id')}")
            return True
        else:
            # 请求失败
            print(f"ComfyUI 请求失败: {response.status_code} - {response.text}")
            if task_id in tasks:
                tasks[task_id]["status"] = "failed"
                tasks[task_id]["error_message"] = f"ComfyUI 请求失败: {response.status_code}"
            return False

    except Exception as e:
        print(f"调用 ComfyUI 失败: {e}")
        if task_id in tasks:
            tasks[task_id]["status"] = "failed"
            tasks[task_id]["error_message"] = str(e)
        return False

# 检查 ComfyUI 状态
async def check_comfyui_status() -> bool:
    """
    检查 ComfyUI 服务是否运行

    Returns:
        bool: ComfyUI 是否运行
    """
    try:
        response = requests.get(f"{COMFYUI_URL}/system_stats", timeout=5)
        return response.status_code == 200
    except:
        return False

# 3. 获取结果接口
@app.get("/api/result/{task_id}")
async def get_result(task_id: str):
    """
    获取结果接口 - 检查 comfyui/output/video 文件夹是否有新文件生成

    Args:
        task_id: 任务 ID

    Returns:
        JSON响应，包含结果状态
    """
    try:
        task = tasks.get(task_id)
        if not task:
            raise HTTPException(status_code=404, detail="任务不存在")

        # 搜索匹配的 mp4 文件
        prefix = f"ComfyUI_{task_id}"
        matched_files = list((COMFYUI_OUTPUT_DIR / "video").glob(f"{prefix}*.mp4"))

        if matched_files:
            # 默认取第一个匹配文件（如有多个）
            video_path = matched_files[0]
            task["status"] = "completed"
            task["video_path"] = str(video_path)

            return JSONResponse(content={
                "success": True,
                "status": "completed",
                "task_id": task_id,
                "video_url": f"/api/download/{task_id}",
                "video_path": str(video_path)
            })
        else:
            # 检查任务是否还在处理中
            if task["status"] == "processing":
                # 检查任务是否超时（5分钟）
                if time.time() - task["created_at"] > 300:
                    task["status"] = "timeout"
                    return JSONResponse(content={
                        "success": False,
                        "status": "timeout",
                        "message": "任务处理超时"
                    })

                return JSONResponse(content={
                    "success": True,
                    "status": "processing",
                    "message": "任务正在处理中"
                })
            else:
                return JSONResponse(content={
                    "success": False,
                    "status": task["status"],
                    "message": "任务处理失败"
                })

    except Exception as e:
        raise HTTPException(status_code=500, detail=f"获取结果失败: {str(e)}")

# 下载视频接口
@app.get("/api/download/{task_id}")
async def download_video(task_id: str):
    """
    下载生成的视频文件

    Args:
        task_id: 任务 ID

    Returns:
        视频文件流
    """
    try:
        task = tasks.get(task_id)
        if not task or task["status"] != "completed":
            raise HTTPException(status_code=404, detail="视频文件不存在")

        video_path = Path(task["video_path"])
        if not video_path.exists():
            raise HTTPException(status_code=404, detail="视频文件不存在")

        return FileResponse(
            path=str(video_path),
            media_type="video/mp4",
            filename=f"effect_{task_id}.mp4"
        )

    except Exception as e:
        raise HTTPException(status_code=500, detail=f"下载失败: {str(e)}")

# 获取任务状态接口
@app.get("/api/task-status/{task_id}")
async def get_task_status(task_id: str):
    """
    获取任务状态

    Args:
        task_id: 任务 ID

    Returns:
        JSON响应，包含任务状态
    """
    try:
        task = tasks.get(task_id)
        if not task:
            raise HTTPException(status_code=404, detail="任务不存在")

        return JSONResponse(content={
            "task_id": task_id,
            "status": task["status"],
            "image_name": task["image_name"],
            "created_at": task["created_at"],
            "video_path": task.get("video_path"),
            "prompt_id": task.get("prompt_id")
        })

    except Exception as e:
        raise HTTPException(status_code=500, detail=f"获取任务状态失败: {str(e)}")

# 启动事件
@app.on_event("startup")
async def startup_event():
    """应用启动时的初始化操作"""
    print("🚀 AI视频特效制作平台已启动")
    print(f"📁 ComfyUI输入目录: {COMFYUI_INPUT_DIR}")
    print(f"📁 ComfyUI输出目录: {COMFYUI_OUTPUT_DIR}")
    print(f"📁 上传目录: {UPLOADS_DIR}")
    print(f"🔗 ComfyUI地址: {COMFYUI_URL}")
    print(f"📄 工作流模板: {WORKFLOW_TEMPLATE_PATH}")

# 关闭事件
@app.on_event("shutdown")
async def shutdown_event():
    """应用关闭时的清理操作"""
    print("🛑 AI视频特效制作平台已关闭")

import uvicorn
uvicorn.run(app, host="0.0.0.0", port=8000)
```
