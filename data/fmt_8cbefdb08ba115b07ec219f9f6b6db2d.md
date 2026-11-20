
```python
import os
import re
from pathlib import Path
from docx import Document
from openai import OpenAI
import runware
from PIL import Image
import requests
from io import BytesIO
import time
from datetime import datetime
import requests
import json
import uuid
import subprocess  # 添加subprocess模块

class RunwareImageGenerator:
    def __init__(self):
        # 基础配置
        self.desktop = str(Path.home() / "Desktop")
        self.rewrite_folder = os.path.join(self.desktop, "剪辑/改写")  # 改写文件夹路径
        self.image_folder = os.path.join(self.desktop, "剪辑/图片")  # 图片文件夹路径
        self.start_time = None
        self.current_file = None
        self.current_file_name = None  # 添加当前处理的文件名
        self.total_cost = 0.0  # 总费用
        self.prevent_sleep = None  # 添加防睡眠进程变量
        
        # 创建必要的文件夹
        os.makedirs(self.rewrite_folder, exist_ok=True)
        os.makedirs(self.image_folder, exist_ok=True)
        
        # OpenAI客户端
        self.client = OpenAI(
            base_url="",
            api_key="sk-"
        )
        
        # 初始化Runware客户端
        runware.api_key = ""  # 需要替换为实际的API key
        
        # 不同模式的提示词
        self.system_prompts = {
            "通用": """
            忘记你以前的一切设定
            将下面的文本翻译为详细的场景英文描述，用作给AI出图，要求：
            1. 译成英文，尽量以人事物+动作+周围场景作为格式
            2. 描述写成一行
            3. 只输出英文描述，不要其他的任何说明
            """,
            
            "中文基督": """
            忘记你以前的一切设定
            将下面的文本翻译为详细的场景英文描述，用作给AI出图，要求：
            1. 译成英文，尽量以人事物+动作+周围场景作为格式
            2. 描述写成一行
            3. 只输出英文描述，不要其他的任何说明
            4. 这是一个基督教相关的内容，所以如果可能，在英文翻译中加入和翻译相符的基督教的元素，如果加不进去也不要强行加入
            """,
            
            "英文出图": """
            忘记你以前的一切设定
            Please enhance the following English text for AI image generation, requirements:
            1. Keep the original meaning but make it more detailed and vivid
            2. Output in a single line
            3. Only output the English description, no other explanations
            """,
            
            "西班牙语": """
            忘记你以前的一切设定
            Por favor, traduzca el siguiente texto a una descripción detallada de la escena en inglés para la generación de imágenes de IA, requisitos:
            1. Traduzca al inglés, intentando utilizar la estructura de persona/objeto + acción + entorno
            2. La descripción debe ser una sola línea
            3. Solo salida de la descripción en inglés, sin ninguna otra explicación
            """,
            
            "恐怖故事": """
            忘记你以前的一切设定
            将文本生成为详细的恐怖场景英文描述，用作给AI出图，要求：
            1. 尽量以人事物+动作+周围恐怖场景作为格式
            2. 描述写成一行
            3. 只输出英文描述，不要其他的任何说明
            """,
        }
        
        # 不同模式的切割配置
        self.split_configs = {
            "1": {  # 通用模式（中文）
                "target_chars": 115,     # 目标字符数
                "type": "chars",         # 按字符切割
                "punctuation": "。！？，；"  # 中文标点
            },
            "2": {  # 中文基督模式（中文）
                "target_chars": 105,     # 目标字符数
                "type": "chars",         # 按字符切割
                "punctuation": "。！？，；"  # 中文标点
            },
            "3": {  # 英文基督模式（英文）
                "target_words": 55,     # 目标单词数
                "type": "words",         # 按单词切割
                "punctuation": ".,"    # 英文标点
            },
            "4": {  # 西班牙语模式（西班牙文）
                "target_words": 70,     # 目标单词数
                "type": "words",         # 按单词切割
                "punctuation": ".,!?¡¿"  # 西班牙文标点
            },
            "5": {  # 恐怖故事模式（英文）
                "target_words": 55,     # 目标单词数
                "type": "words",         # 按单词切割
                "punctuation": ".,"    # 英文标点
            }
        }
        
        # 当前使用的模式
        self.current_mode = None
        
        # 初始化结果列表
        self.gpt_results = []

    def get_current_time(self):
        """获取当前时间的格式化字符串"""
        return datetime.now().strftime("%Y-%m-%d %H:%M:%S")

    def log_message(self, message, level="INFO"):
        """添加日志记录功能"""
        current_time = self.get_current_time()
        print(f"[{current_time}] [{level}] {message}")

    def print_progress(self, current, total):
        """显示进度条"""
        progress = current / total
        filled_length = int(50 * progress)
        bar = '=' * filled_length + ' ' * (50 - filled_length)
        print(f"\r处理进度: [{bar}] {current}/{total}", end='', flush=True)

    def read_docx(self, file_path):
        """读取指定的docx文件内容"""
        try:
            self.log_message(f"尝试读取文件: {file_path}")
            doc = Document(file_path)
            
            # 初始化段落列表
            paragraphs = []
            
            # 收集所有段落的文本
            for para in doc.paragraphs:
                text = para.text.strip()
                if text:  # 只添加非空段落
                    paragraphs.append(text)
            
            return paragraphs
            
        except Exception as e:
            self.log_message(f"读取文件出错: {str(e)}", "ERROR")
            return None

    def split_paragraphs(self, paragraphs):
        """将段落分段"""
        try:
            segments = []
            
            # 对于英文基督模式和西班牙语，我们将所有段落合并成一个完整的文本
            if self.current_mode in ["3", "4", "5"]:  # 英文基督模式或西班牙语模式
                # 清理每个段落
                cleaned_paragraphs = []
                for para in paragraphs:
                    # 移除多余的空格、换行和标点符号前的空格
                    cleaned = re.sub(r'\s+', ' ', para).strip()
                    cleaned = re.sub(r'\s+([.,!?])', r'\1', cleaned)
                    if cleaned:
                        cleaned_paragraphs.append(cleaned)
                
                # 合并所有段落
                text = ' '.join(cleaned_paragraphs)
                # 再次清理合并后的文本
                text = re.sub(r'\s+', ' ', text).strip()
                text = re.sub(r'\s+([.,!?])', r'\1', text)
                return self.split_into_paragraphs(text)
            
            # 对于其他模式，保持原有的分段逻辑
            for para in paragraphs:
                # 清理段落文本
                lines = [line.strip() for line in para.splitlines() if line.strip()]
                if not lines:
                    continue
                    
                text = ' '.join(lines)
                
                # 通用模式和中文基督模式使用字符数量切割
                start_pos = 0
                while start_pos < len(text):
                    if len(text) - start_pos <= 150:
                        last_segment = text[start_pos:].strip()
                        if last_segment:
                            segments.append(last_segment)
                        break
                        
                    check_pos = start_pos + 150
                    found_punctuation = False
                    for i in range(check_pos, min(check_pos + 20, len(text))):
                        if text[i] in '"。，,¡!¿?':  # 添加西班牙语标点符号
                            segments.append(text[start_pos:i + 1].strip())
                            start_pos = i + 1
                            found_punctuation = True
                            break
                        
                    if not found_punctuation:
                        segments.append(text[start_pos:start_pos + 160].strip())
                        start_pos += 170
            
            return segments
            
        except Exception as e:
            self.log_message(f"分段处理出错: {str(e)}", "ERROR")
            return None

    def count_words(self, text):
        """计算英文文本中的单词数"""
        text = re.sub(r'[^\w\s]', '', text)
        words = [word for word in text.split() if word]
        return len(words)

    def split_into_paragraphs(self, text):
        """根据不同模式的配置将文本分段"""
        try:
            # 获取当前模式的配置
            config = self.split_configs.get(self.current_mode)
            if not config:
                self.log_message(f"未找到模式 {self.current_mode} 的配置", "ERROR")
                return None
            
            # 根据模式类型选择不同的切割方式
            if config["type"] == "chars":
                # 中文模式：按字符数切割
                current_chars = 0
                paragraphs = []
                current_paragraph = ""
                
                for char in text:
                    current_paragraph += char
                    current_chars += 1
                    
                    # 达到目标字符数后，寻找下一个标点
                    if current_chars >= config["target_chars"]:
                        # 在最后50个字符中查找标点
                        found_punct = False
                        for i in range(len(current_paragraph)-1, max(-1, len(current_paragraph)-50), -1):
                            if current_paragraph[i] in config["punctuation"]:
                                paragraphs.append(current_paragraph[:i+1].strip())
                                current_paragraph = current_paragraph[i+1:].strip()
                                current_chars = len(current_paragraph)
                                found_punct = True
                                break
                        
                        # 如果没找到标点，继续往后找
                        if not found_punct:
                            for i in range(len(current_paragraph)-1, -1, -1):
                                if current_paragraph[i] in config["punctuation"]:
                                    paragraphs.append(current_paragraph[:i+1].strip())
                                    current_paragraph = current_paragraph[i+1:].strip()
                                    current_chars = len(current_paragraph)
                                    found_punct = True
                                    break
                            
                            # 如果还是没找到标点，强制切割
                            if not found_punct and current_chars > config["target_chars"] * 1.5:
                                paragraphs.append(current_paragraph.strip())
                                current_paragraph = ""
                                current_chars = 0
                
                # 添加最后一段
                if current_paragraph:
                    paragraphs.append(current_paragraph.strip())
                    
            else:  # words模式（英文和西班牙文）
                words = text.split()
                paragraphs = []
                current_paragraph = []
                word_count = 0
                total_words = len(words)
                self.log_message(f"总单词数: {total_words}")
                
                for word in words:
                    current_paragraph.append(word)
                    word_count += 1
                    
                    # 达到目标单词数后，寻找下一个标点
                    if word_count >= config["target_words"]:
                        current_text = ' '.join(current_paragraph)
                        
                        # 在最后一部分文本中查找标点
                        last_pos = -1
                        for punct in config["punctuation"]:
                            pos = current_text.rfind(punct, max(0, len(current_text)-200))
                            if pos > last_pos:
                                last_pos = pos
                        
                        if last_pos != -1:
                            cut_text = current_text[:last_pos+1].strip()
                            remaining_text = current_text[last_pos+1:].strip()
                            cut_words = len(cut_text.split())
                            self.log_message(f"在标点处切割，本段单词数: {cut_words}")
                            paragraphs.append(cut_text)
                            current_paragraph = remaining_text.split()
                            word_count = len(current_paragraph)
                        else:
                            # 如果在最后200个字符没找到标点，强制切割
                            self.log_message(f"强制切割，本段单词数: {word_count}")
                            paragraphs.append(current_text.strip())
                            current_paragraph = []
                            word_count = 0
                
                # 添加最后一段
                if current_paragraph:
                    last_text = ' '.join(current_paragraph).strip()
                    last_words = len(last_text.split())
                    self.log_message(f"添加最后一段，单词数: {last_words}")
                    paragraphs.append(last_text)
                
                # 输出每段的单词数统计
                for i, para in enumerate(paragraphs, 1):
                    words_in_para = len(para.split())
                    self.log_message(f"第{i}段的单词数: {words_in_para}")
            
            return paragraphs
            
        except Exception as e:
            self.log_message(f"分段处理出错: {str(e)}", "ERROR")
            return None

    def calculate_cost(self, input_tokens, output_tokens):
        """计算API调用成本"""
        input_cost = (input_tokens / 1000000) * 3.00  # $3.00 per 1M tokens for input
        output_cost = (output_tokens / 1000000) * 12.00  # $12.00 per 1M tokens for output
        total_cost = input_cost + output_cost
        return total_cost

    def generate_image(self, prompt):
        """使用Runware API生成图片"""
        max_retries = 3
        retry_delay = 2
        
        for attempt in range(max_retries):
            try:
                # 添加请求延迟，避免API限流
                time.sleep(3)
                
                # API endpoint
                url = "https://api.runware.ai/v1/inference"
                
                # 请求头
                headers = {
                    "Authorization": f"Bearer {runware.api_key}",
                    "Content-Type": "application/json"
                }
                
                # 请求体
                data = [{
                    "taskType": "imageInference",
                    "taskUUID": str(uuid.uuid4()),
                    "model": "runware:101@1",
                    "positivePrompt": prompt,
                    "negativePrompt": "NSFW, nude, naked, blood, gore, violence, offensive, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry,letter",
                    "width": 1024,
                    "height": 576,
                    "steps": 20,
                    "scheduler": "FlowMatchEulerDiscreteScheduler",
                    "CFGScale": 3.5,
                    "outputType": "URL",
                    "outputFormat": "JPG",
                    "includeCost": True
                }]
                
                self.log_message("开始生成图片...")
                
                # 发送请求
                response = requests.post(url, headers=headers, json=data)
                response.raise_for_status()
                
                # 解析响应
                result = response.json()
                
                if isinstance(result, dict) and 'data' in result and isinstance(result['data'], list):
                    for item in result['data']:
                        if item['taskType'] == 'imageInference':
                            # 计算费用
                            if 'cost' in item:
                                cost = item['cost']
                                self.total_cost += cost
                                self.log_message(f"本次生成费用: ${cost:.4f}")
                                rmb_cost = cost * 7.2
                                self.log_message(f"本次生成费用(人民币): ¥{rmb_cost:.2f}")
                            
                            # 获取图片URL
                            task_id = item.get('taskUUID')
                            image_url = item.get('imageURL')
                            if task_id and image_url:
                                return task_id, image_url
                            
                    self.log_message("API响应中没有找到有效的图片URL", "ERROR")
                else:
                    self.log_message("API响应格式错误", "ERROR")
                return None, None
                
            except requests.exceptions.RequestException as e:
                if attempt < max_retries - 1:
                    self.log_message(f"API请求失败，{retry_delay}秒后重试 ({attempt + 1}/{max_retries}): {str(e)}", "WARNING")
                    time.sleep(retry_delay)
                    continue
                self.log_message(f"API请求失败: {str(e)}", "ERROR")
                return None, None
            except Exception as e:
                self.log_message(f"生成图片失败: {str(e)}", "ERROR")
                return None, None

    def save_image(self, image_url, image_name):
        """保存图片到本地"""
        try:
            # 创建保存目录
            save_folder = self.get_current_save_folder()
            if not save_folder:
                return False
            
            # 下载图片
            response = requests.get(image_url)
            if response.status_code != 200:
                self.log_message(f"下载图片失败: {response.status_code}", "ERROR")
                return False
            
            # 保存图片
            image_path = os.path.join(save_folder, f"{image_name}.png")
            with open(image_path, 'wb') as f:
                f.write(response.content)
            
            self.log_message(f"图片已保存: {image_path}")
            return True
            
        except Exception as e:
            self.log_message(f"保存图片失败: {str(e)}", "ERROR")
            return False

    def process_segment_with_gpt(self, segment, mode):
        """使用GPT处理段落"""
        try:
            if not segment:
                return None
                
            mode_map = {
                "1": "通用",
                "2": "中文基督",
                "3": "英文基督",
                "4": "西班牙语",
                "5": "恐怖故事"
            }
            system_prompt = self.system_prompts.get(mode_map.get(mode))
            if not system_prompt:
                self.log_message(f"无效的模式: {mode}", "ERROR")
                return None
                
            messages = [
                {"role": "system", "content": system_prompt},
                {"role": "user", "content": segment}
            ]
            
            try:
                response = self.client.chat.completions.create(
                    model="gpt-4o-mini",
                    messages=messages,
                    temperature=0.7,
                    max_tokens=2048
                )
                
                if response and response.choices:
                    prompt = response.choices[0].message.content.strip()
                    # 添加高质量图片关键词
                    quality_keywords = ", breathtaking photograph, cinematic lighting, 8k uhd, highly detailed, photorealistic, professional photography, masterpiece, sharp focus, high quality"
                    prompt = prompt + quality_keywords
                    return prompt
                    
            except Exception as e:
                self.log_message(f"调用GPT API失败: {str(e)}", "ERROR")
                return None
                
        except Exception as e:
            self.log_message(f"GPT处理失败: {str(e)}", "ERROR")
            return None

    def process_file(self, file_path, mode):
        """处理单个文件"""
        try:
            # 重置总费用
            self.total_cost = 0.0
            
            # 设置当前文件名和开始时间
            self.current_file = file_path
            self.current_file_name = os.path.splitext(os.path.basename(file_path))[0]
            self.start_time = time.time()
            
            self.log_message(f"开始处理文件: {file_path}")
            self.current_mode = mode
            
            # 读取文件内容
            doc = Document(file_path)
            
            # 收集所有段落文本
            paragraphs = []
            for para in doc.paragraphs:
                text = para.text.strip()
                if text:  # 只添加非空段落
                    paragraphs.append(text)
            
            # 合并所有文本并进行分段
            full_text = ' '.join(paragraphs)
            # 清理文本：移除多余的空格和换行
            full_text = re.sub(r'\s+', ' ', full_text).strip()
            segments = self.split_into_paragraphs(full_text)
            
            if not segments:
                self.log_message("没有找到有效的文本段落", "ERROR")
                return False
                
            self.log_message(f"{os.path.basename(file_path)}已被分割成 {len(segments)} 段")
            
            # 创建保存文件夹
            save_folder = self.get_current_save_folder()
            if not save_folder:
                return False
                
            # 处理每个段落
            for i, segment in enumerate(segments, 1):
                self.print_progress(i, len(segments))
                
                # 使用GPT处理文本
                prompt = self.process_segment_with_gpt(segment, mode)
                if not prompt:
                    continue
                    
                # 为每个prompt生成4张图片
                for j in range(4):
                    # 生成图片
                    task_id, image_url = self.generate_image(prompt)
                    if not task_id or not image_url:
                        continue
                        
                    # 保存图片，使用新的命名方式
                    image_name = f"{i}_{j+1}"  # 例如：1_1.png, 1_2.png, 1_3.png, 1_4.png
                    if not self.save_image(image_url, image_name):
                        continue
            
            print()  # 换行
            
            # 显示总费用
            if self.total_cost > 0:
                self.log_message(f"总费用: ${self.total_cost:.4f}")
                rmb_total_cost = self.total_cost * 7.2
                self.log_message(f"总费用(人民币): ¥{rmb_total_cost:.2f}")
            
            return True
            
        except Exception as e:
            self.log_message(f"处理文件失败: {str(e)}", "ERROR")
            return False

    def select_files(self, docx_files):
        """选择要处理的文件"""
        if not docx_files:
            self.log_message("未找到任何docx文件")
            return None, None
            
        # 按文件名字母顺序排序（忽略大小写）
        docx_files.sort(key=lambda x: os.path.basename(x).lower())
        
        while True:
            print("\n找到以下文件：")
            for i, file_path in enumerate(docx_files, 1):
                print(f"{i}. {os.path.basename(file_path)}")
            
            print("\n输入 'q' 退出程序")
            
            choice = input("\n请输入要处理的文件编号（多个文件用空格分隔）: ").strip()
            if choice.lower() == 'q':
                return None, None
            
            try:
                # 处理多个文件选择
                file_indices = [int(x) for x in choice.split()]
                selected_files = []
                
                for index in file_indices:
                    if 1 <= index <= len(docx_files):
                        selected_files.append(docx_files[index - 1])
                    else:
                        self.log_message(f"无效的文件编号: {index}", "ERROR")
                        return None, None
                
                if selected_files:
                    return selected_files, choice
                
            except ValueError:
                self.log_message("请输入有效的文件编号", "ERROR")
        
        return None, None

    def run(self):
        """主运行方法"""
        try:
            # 启动防睡眠
            self.prevent_sleep = subprocess.Popen(['caffeinate'])
            self.log_message("已启动防睡眠模式")
            
            # 查找所有非临时和非隐藏的docx文件
            docx_files = []
            for file in os.listdir(self.rewrite_folder):
                # 排除隐藏文件和临时文件
                if (file.endswith('.docx') and 
                    not file.startswith('.') and  # 隐藏文件
                    not file.startswith('~$') and  # Word临时文件
                    not file.startswith('~') and   # 其他临时文件
                    not file.endswith('.tmp')):    # 临时文件扩展名
                    docx_files.append(os.path.join(self.rewrite_folder, file))
            
            if not docx_files:
                print(f"\n在 {self.rewrite_folder} 中没有找到.docx文件")
                print("请将要处理的文件放入此文件夹，然后重新运行程序")
                return
            
            # 选择文件
            selected_files, choice = self.select_files(docx_files)
            if not selected_files:
                return
            
            # 为每个文件选择处理模式
            file_modes = {}
            print("\n为每个文件选择处理模式：")
            for file_path in selected_files:
                while True:
                    print(f"\n文件：{os.path.basename(file_path)}")
                    print("1. 通用模式")
                    print("2. 基督教内容")
                    print("3. 英文基督教内容")
                    print("4. 西班牙语模式")
                    print("5. 恐怖故事模式")
                    print("0. 取消处理")
                    
                    mode = input("请输入选项编号 (0/1/2/3/4/5): ").strip()
                    if mode == "0":
                        return  # 用户取消处理
                    elif mode in ['1', '2', '3', '4', '5']:
                        file_modes[file_path] = mode
                        break
                    else:
                        print("无效的选项，请重试")
            
            # 确认开始处理
            print("\n所有文件的处理模式已选择完毕：")
            for file_path, mode in file_modes.items():
                mode_name = {
                    "1": "通用模式",
                    "2": "基督教内容",
                    "3": "英文基督教内容",
                    "4": "西班牙语模式",
                    "5": "恐怖故事模式"
                }[mode]
                print(f"- {os.path.basename(file_path)}: {mode_name}")
            
            confirm = input("\n是否开始处理？(y/n): ").strip().lower()
            if confirm != 'y':
                print("已取消处理")
                return
            
            # 开始处理所有文件
            print("\n开始处理文件...")
            for file_path, mode in file_modes.items():
                print(f"\n正在处理：{os.path.basename(file_path)}")
                if not self.process_file(file_path, mode):
                    self.log_message(f"处理文件 {os.path.basename(file_path)} 失败", "ERROR")
            
            # 显示总费用
            print("\n=== 费用统计 ===")
            print(f"总费用: ${self.total_cost:.4f}")
            rmb_total = self.total_cost * 7.2
            print(f"总费用(人民币): ¥{rmb_total:.2f}")
            print("==============")
            
        except Exception as e:
            self.log_message(f"运行时发生错误: {str(e)}", "ERROR")
            
        finally:
            # 关闭防睡眠
            if self.prevent_sleep:
                self.prevent_sleep.terminate()
                self.log_message("已关闭防睡眠模式")

    def get_current_save_folder(self):
        """获取当前保存文件夹"""
        try:
            if not self.current_file_name:
                self.log_message("当前没有处理的文件名", "ERROR")
                return None
            
            # 在图片文件夹下创建以文档名命名的子文件夹
            save_folder = os.path.join(self.image_folder, self.current_file_name)
            
            # 如果文件夹不存在，则创建
            if not os.path.exists(save_folder):
                os.makedirs(save_folder)
                self.log_message(f"创建保存文件夹: {save_folder}")
            
            return save_folder
        
        except Exception as e:
            self.log_message(f"获取保存文件夹失败: {str(e)}", "ERROR")
            return None

    def process_text_to_images(self, content, save_folder, mode):
        """处理文本生成图片"""
        try:
            # 使用GPT生成英文描述
            print("正在生成英文描述...")
            
            # 计算输入token数量（粗略估计：每个字符4个token）
            input_tokens = len(content) * 4
            
            completion = self.client.chat.completions.create(
                model="o1-mini-2024-09-12",
                messages=[
                    {"role": "user", "content": f"{self.system_prompts[mode]}\n\n{content}"}
                ],
                max_completion_tokens=2000
            )
            
            english_description = completion.choices[0].message.content.strip()
            print(f"英文描述: {english_description}")
            
            # 计算输出token数量和成本
            output_tokens = len(english_description) * 4
            cost = self.calculate_cost(input_tokens, output_tokens)
            self.total_cost += cost
            print(f"本次调用成本: ${cost:.4f}")
            print(f"累计总成本: ${self.total_cost:.4f}")
            
            # 使用Runware生成图片
            print("正在生成图片...")
            response = runware.create_image(
                prompt=english_description,
                model="flux",
                n=1,
                size="1024x1024",
                quality="standard",
                style="cinematic"
            )
            
            # 保存图片
            image_url = response.data[0].url
            response = requests.get(image_url)
            if response.status_code == 200:
                # 生成唯一的文件名
                timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
                unique_id = str(uuid.uuid4())[:8]
                image_filename = f"image_{timestamp}_{unique_id}.png"
                image_path = os.path.join(save_folder, image_filename)
                
                # 保存图片
                with open(image_path, 'wb') as f:
                    f.write(response.content)
                print(f"图片已保存: {image_path}")
                return True
            else:
                print(f"下载图片失败: {response.status_code}")
                return False
                
        except Exception as e:
            print(f"生成图片时出错: {str(e)}")
            return False

if __name__ == "__main__":
    generator = RunwareImageGenerator()
    generator.run()
```
