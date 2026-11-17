
```python
# 功能四：音频对话系统最小运行示例
import tempfile
import os
from pathlib import Path
from openai import OpenAI
from pydub import AudioSegment
from moviepy.editor import VideoFileClip
from langchain_openai import ChatOpenAI
from langchain_core.messages import HumanMessage, SystemMessage

class AudioChatSystem:
    """简单音频对话系统 - 支持音频转写与智能问答"""
    
    def __init__(self, api_key: str, base_url: str = "https://api.openai.com/v1"):
        # OpenAI客户端用于Whisper转写
        self.openai_client = OpenAI(
            api_key=api_key,
            base_url=base_url
        )
        
        # LangChain模型用于智能问答
        self.chat_model = ChatOpenAI(
            model="gpt-4o",
            api_key=api_key,
            base_url=base_url,
            temperature=0.7,
            max_tokens=2048
        )
        
        # 存储音频转写结果
        self.transcriptions = []
    
    def convert_to_audio(self, file_path: str, file_ext: str) -> str:
        """将视频文件转换为音频文件"""
        if file_ext.lower() in ['.mp4', '.avi', '.mov', '.mkv', '.webm']:
            # 视频文件，提取音频
            with tempfile.NamedTemporaryFile(suffix='.wav', delete=False) as temp_file:
                temp_audio_path = temp_file.name
            
            # 使用moviepy提取音频
            video = VideoFileClip(file_path)
            video.audio.write_audiofile(temp_audio_path, verbose=False, logger=None)
            video.close()
            
            return temp_audio_path
        else:
            # 已经是音频文件，直接返回
            return file_path
    
    def optimize_audio_for_whisper(self, audio_path: str) -> str:
        """优化音频文件以适配Whisper"""
        # 加载音频
        audio = AudioSegment.from_file(audio_path)
        
        # 转换为适合Whisper的格式
        # - 采样率: 16kHz
        # - 声道: 单声道
        # - 格式: WAV
        audio = audio.set_frame_rate(16000).set_channels(1)
        
        # 保存优化后的音频
        with tempfile.NamedTemporaryFile(suffix='.wav', delete=False) as temp_file:
            optimized_path = temp_file.name
        
        audio.export(optimized_path, format="wav")
        return optimized_path
    
    def transcribe_audio(self, file_path: str, filename: str) -> dict:
        """转写音频文件"""
        try:
            # 检测文件类型
            file_ext = Path(filename).suffix.lower()
            
            # 转换为音频（如果是视频）
            audio_path = self.convert_to_audio(file_path, file_ext)
            
            # 优化音频格式
            optimized_path = self.optimize_audio_for_whisper(audio_path)
            
            try:
                # 使用Whisper进行转写
                with open(optimized_path, "rb") as audio_file:
                    transcript = self.openai_client.audio.transcriptions.create(
                        model="whisper-1",
                        file=audio_file,
                        response_format="text"
                    )
                
                # 获取音频时长
                audio = AudioSegment.from_file(optimized_path)
                duration = len(audio) / 1000.0  # 转换为秒
                
                result = {
                    "filename": filename,
                    "transcription": transcript,
                    "duration": duration,
                    "format": file_ext,
                    "status": "success"
                }
                
                # 保存转写结果
                self.transcriptions.append(result)
                
                return result
                
            finally:
                # 清理临时文件
                if optimized_path != audio_path:
                    os.unlink(optimized_path)
                if audio_path != file_path:
                    os.unlink(audio_path)
                    
        except Exception as e:
            return {
                "filename": filename,
                "transcription": "",
                "duration": 0,
                "format": file_ext,
                "status": "error",
                "error": str(e)
            }
    
    def chat_with_audio_content(self, question: str, transcription_text: str) -> str:
        """基于音频转写内容回答问题"""
        system_prompt = """你是一个专业的音频内容分析助手。请基于提供的音频转写内容回答用户问题。

回答要求：
1. 仅基于提供的转写内容进行回答
2. 如果转写内容中没有相关信息，请明确说明
3. 可以对音频内容进行分析、总结、提取关键信息
4. 保持回答的准确性和客观性

请仔细分析转写内容并提供有价值的回答。"""
        
        user_prompt = f"""音频转写内容：
{transcription_text}

用户问题：{question}

请基于以上音频转写内容回答问题。"""
        
        messages = [
            SystemMessage(content=system_prompt),
            HumanMessage(content=user_prompt)
        ]
        
        response = self.chat_model.invoke(messages)
        return response.content
    
    def summarize_audio_content(self, transcription_text: str) -> str:
        """生成音频内容摘要"""
        summary_prompt = f"""请为以下音频转写内容生成一个详细的摘要：

{transcription_text}

摘要要求：
1. 提取主要内容和关键信息
2. 识别重要的观点和结论
3. 保持逻辑结构清晰
4. 长度控制在300字以内

请提供专业的内容摘要。"""
        
        messages = [
            SystemMessage(content="你是一个专业的内容摘要助手。"),
            HumanMessage(content=summary_prompt)
        ]
        
        response = self.chat_model.invoke(messages)
        return response.content
    
    def extract_key_points(self, transcription_text: str) -> str:
        """提取音频内容的关键要点"""
        extract_prompt = f"""请从以下音频转写内容中提取关键要点：

{transcription_text}

提取要求：
1. 识别并列出主要观点（最多10个）
2. 每个要点用简洁的语言描述
3. 按重要性排序
4. 包含具体的数据或例子（如果有）

请以清晰的列表形式呈现关键要点。"""
        
        messages = [
            SystemMessage(content="你是一个专业的内容分析助手。"),
            HumanMessage(content=extract_prompt)
        ]
        
        response = self.chat_model.invoke(messages)
        return response.content
```

# 4. 项目结构详解与完整部署指南

最后，我来为大家详细介绍整个项目的组织结构和完整的部署流程。一个清晰的目录结构是项目成功的基础。采用了分层分模块的组织方式：
